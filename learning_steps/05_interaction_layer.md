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

## 완료 기준

- 신호 put 함수가 Tx buffer를 정확히 갱신한다.
- 신호 get 함수가 Rx buffer에서 값을 정확히 조립한다.
- 주기 task가 설정된 주기마다 CAN 송신을 요청한다.
- CAN Driver의 Rx callback과 IL이 연결된다.
