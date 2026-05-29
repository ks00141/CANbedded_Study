# 5단계: IL, Interaction Layer 학습

## 학습 목표

IL은 DBC 신호와 CAN 메시지 버퍼를 연결하는 계층이다. 이 단계에서는 신호 put/get 함수, 주기 송신 task, 수신 버퍼 갱신 구조를 학습한다.

## 참조 파일

- `src/CBD/Il/il_inc.h`
- `src/CBD/Il/il_def.h`
- `src/CBD/Il/il.c`
- `src/CBD/Gen/il_cfg.h`
- `src/CBD/Gen/il_par.h`
- `src/CBD/Gen/il_par.c`

## 이론 설명

애플리케이션이 CAN payload의 특정 비트를 직접 조작하면 오류가 생기기 쉽다. IL은 이런 조작을 함수로 감싼다.

예를 들어 애플리케이션은 다음처럼 사용한다.

```c
IlPutTxIPSU_Recline_Current_Pos(1234u);
```

그러면 IL은 이 값을 대응되는 CAN 메시지 버퍼의 여러 바이트에 나누어 저장한다. 주기 task가 돌면 해당 메시지를 CAN Driver로 송신한다.

수신은 반대이다. CAN Driver가 수신 데이터를 버퍼에 복사하면, 애플리케이션은 `IlGetRx...()` 함수로 신호 값을 읽는다.

### IL이 해결하는 문제

CAN 메시지는 8바이트 또는 CAN-FD에서는 최대 64바이트의 byte stream이다. 하지만 애플리케이션 입장에서는 `Recline_Current_Pos`, `Slide_Target_Pos`, `Footrest_Current_Sta` 같은 신호가 필요하다. IL은 이 둘 사이의 변환 계층이다.

IL이 없으면 애플리케이션은 다음을 직접 알아야 한다.

- 어떤 메시지에 어떤 신호가 있는지
- 신호 시작 bit와 bit length가 무엇인지
- byte order가 Intel인지 Motorola인지
- 주기 송신인지 이벤트 송신인지
- 수신 timeout이나 indication flag가 필요한지

IL은 이 정보를 generated code로 감싸서 애플리케이션이 함수 호출만 하도록 만든다.

### Tx IL의 동작 모델

Tx 방향에서 IL은 애플리케이션이 쓴 신호 값을 Tx buffer에 반영하고, 주기 task 또는 이벤트 trigger 시 CAN Driver에 송신 요청을 보낸다.

기본 흐름:

```text
Application
  -> IlPutTxSignal(value)
  -> Tx message buffer update
  -> IlTxTask()
  -> CanTransmit(txHandle)
```

주기 메시지는 counter를 사용한다. 예를 들어 task 주기가 10ms이고 메시지 주기가 200ms이면 counter가 20에 도달할 때 송신한다. 이벤트 메시지는 신호 값 변경 또는 명시적 send 요청에 의해 더 빨리 나갈 수 있다.

### Rx IL의 동작 모델

Rx 방향에서 Driver가 수신 frame을 buffer에 복사하면 IL은 수신 상태를 갱신한다. 설정에 따라 indication flag, timeout counter, first value flag, data changed flag를 관리할 수 있다. 현재 CBD 설정에서는 timeout/flag 기능이 많이 비활성화되어 있지만, 구조적으로는 IL이 담당하는 영역이다.

기본 흐름:

```text
CAN Driver
  -> Rx handle matched
  -> Rx data buffer copy
  -> IlCanGenericPrecopy / indication
  -> Application IlGetRxSignal()
```

### SPC58EC70 적용 관점

IL은 MCU register를 직접 다루지 않는다. 하지만 `SPC58EC70`에서 interrupt와 task가 동시에 buffer에 접근할 수 있으므로 concurrency를 고려해야 한다.

- Rx buffer는 CAN ISR 또는 Driver task에서 갱신될 수 있다.
- Application task는 같은 buffer를 읽을 수 있다.
- 16/32비트 신호를 여러 byte로 읽는 중 ISR이 buffer를 갱신하면 tearing이 생길 수 있다.

해결 방법은 다음 중 하나이다.

- 신호 읽기 중 짧은 interrupt lock 사용
- double buffer 사용
- Rx indication 시점에 shadow copy 생성
- 8/16/32비트 atomic 접근이 보장되는 신호만 직접 접근

학습용 구현에서는 단순하게 시작하되, 실제 MCU 이식 단계에서는 buffer 보호 정책을 반드시 정해야 한다.

### CAN-FD와 IL

CAN-FD에서는 한 메시지에 더 많은 신호를 넣을 수 있다. 따라서 IL은 message length를 설정에서 읽고, signal accessor가 payload 범위를 넘지 않는지 검사해야 한다. 기존 HS-CAN 메시지는 8바이트 layout을 유지하고, FD 메시지만 확장 layout을 쓰는 식으로 공존 설계를 하는 편이 안전하다.

## CBD 코드에서 관찰할 포인트

- `IlTxTask`: 주기 송신 처리
- `IlCanGenericPrecopy`: CAN 수신 프레임을 IL/TP로 분배
- `IlPutTx...`: 송신 신호 쓰기
- `IlGetRx...`: 수신 신호 읽기
- `kIlTxCycleTime`, `kIlRxCycleTime`

## 예제 코드

### il.c

```c
#include "platform_types.h"
#include "can.h"
#include "can_par.h"

static vuint8 g_il_tx_started;
static vuint8 g_il_rx_started;
static vuint8 g_ipus_01_counter;

extern vuint8 IPSU_01_200ms[8];
extern vuint8 CGW_01_200ms[8];

void Il_Init(void)
{
    g_il_tx_started = 0u;
    g_il_rx_started = 0u;
    g_ipus_01_counter = 0u;
}

void Il_TxStart(void)
{
    g_il_tx_started = 1u;
}

void Il_TxStop(void)
{
    g_il_tx_started = 0u;
}

void Il_RxStart(void)
{
    g_il_rx_started = 1u;
}

void Il_RxStop(void)
{
    g_il_rx_started = 0u;
}

void IlPutTxIPSU_Recline_Current_Pos(vuint32 value)
{
    IPSU_01_200ms[0] = (vuint8)(value & 0xFFu);
    IPSU_01_200ms[1] = (vuint8)((value >> 8) & 0xFFu);
    IPSU_01_200ms[2] = (vuint8)((value >> 16) & 0xFFu);
    IPSU_01_200ms[3] = (vuint8)((value >> 24) & 0xFFu);
}

vuint16 IlGetRxCGW_Recline_Min_VrtLmt_Value(void)
{
    return (vuint16)CGW_01_200ms[0] |
           ((vuint16)CGW_01_200ms[1] << 8);
}

void Il_TxTask_10ms(void)
{
    if (g_il_tx_started == 0u)
    {
        return;
    }

    g_ipus_01_counter++;
    if (g_ipus_01_counter >= 20u)
    {
        g_ipus_01_counter = 0u;
        (void)Can_Transmit(CanTxIPSU_01_200ms);
    }
}

void Il_CanRxIndication(CanRxHandle rx)
{
    if (g_il_rx_started == 0u)
    {
        return;
    }

    if (rx == CanRxCGW_01_200ms)
    {
        /* CGW_01_200ms 버퍼는 CAN Driver에서 이미 갱신되었다. */
    }
}
```

## 연습문제

1. `IlPutTxIPSU_Recline_Target_Pos()`를 구현하라. 4~7바이트에 32비트 값을 저장한다.
2. `IlGetRxCGW_Recline_Max_VrtLmt_Value()`를 구현하라. 2~3바이트에서 16비트 값을 읽는다.
3. 100ms 주기 메시지와 200ms 주기 메시지를 각각 다른 카운터로 송신하라.
4. Rx indication flag를 추가하고, 애플리케이션이 읽으면 flag가 clear되도록 구현하라.
5. `Il_TxStop()` 상태에서는 주기 송신이 발생하지 않음을 테스트하라.

## 단계 산출물

이 단계의 결과물은 CAN message를 application signal 단위로 바꾸는 Interaction Layer이다.

- `middleware/il/il.h`: signal read/write, Tx request, Rx indication API
- `middleware/il/il.c`: Tx 주기 처리, Rx buffer 갱신, timeout 감시
- `middleware/il/il_par.h`, `middleware/il/il_par.c`: message별 signal layout, 주기, timeout, 초기값 설정
- `tests/il/test_il_signal.c`: endian/offset/length signal packing 테스트
- `tests/il/test_il_periodic.c`: 주기 송신과 timeout 검증 테스트

HS-CAN 결과물에서는 8바이트 message 안에서 signal packing/unpacking이 정확해야 한다. CAN-FD 결과물에서는 64바이트 payload 내부 signal도 같은 API로 접근하되, 기존 8바이트 메시지의 동작은 변하지 않아야 한다.

## 적용 고려사항과 트러블슈팅

IL은 application과 bus 사이의 데이터 계약이다. 이 계층에서 signal 위치, endian, scaling, timeout 정책이 틀리면 Driver가 정상이어도 application은 잘못된 값을 보게 된다.

| 문제 케이스 | 원인 | 확인/대응 |
|---|---|---|
| 수신 signal 값이 2배 또는 256배 차이남 | endian 또는 bit offset 해석 오류 | DBC의 byte order와 start bit 규칙을 기준으로 packing 테스트를 만든다. |
| 주기 송신이 빨라지거나 늦어짐 | scheduler tick과 message period 단위 혼동 | `MW_TASK_PERIOD_MS` 기준으로 tick count를 계산하고 remainder 정책을 문서화한다. |
| Rx buffer 값이 가끔 깨짐 | ISR와 main loop 동시 접근 | double buffer 또는 짧은 critical section으로 buffer copy를 보호한다. |
| CAN-FD 확장 후 일부 signal이 잘림 | message buffer를 8바이트로 고정 | message별 configured length를 사용하고 signal end bit가 length 안에 있는지 검사한다. |

트러블슈팅할 때는 한 개 message를 골라 “원본 payload hex -> unpacked signal -> application 값”을 기록한다. Tx 방향은 반대로 “application 값 -> packed payload hex -> analyzer frame”을 비교한다.

## 완료 기준

- 신호 put 함수가 Tx buffer를 정확히 갱신한다.
- 신호 get 함수가 Rx buffer에서 값을 정확히 조립한다.
- 주기 task가 설정된 주기마다 CAN 송신을 요청한다.
- CAN Driver의 Rx callback과 IL이 연결된다.
