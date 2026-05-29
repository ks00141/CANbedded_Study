# 4단계: CAN Driver 학습

## 학습 목표

CAN Driver는 미들웨어와 CAN 하드웨어 사이의 경계이다. 이 단계에서는 논리 메시지 핸들, 하드웨어 송신 mailbox, 수신 필터, Tx confirmation, Rx indication, BusOff callback 구조를 학습한다.

## 참조 파일

- `src/CBD/Can/can_inc.h`
- `src/CBD/Can/can_def.h`
- `src/CBD/Can/can_drv.c`
- `src/CBD/Gen/can_cfg.h`
- `src/CBD/Gen/can_par.*`

## 이론 설명

CAN Driver는 상위 계층이 하드웨어 레지스터를 몰라도 송수신할 수 있게 해준다.

핵심 흐름은 다음과 같다.

```text
상위 계층
  -> CanTransmit(txHandle)
  -> Tx 설정 테이블 조회
  -> CAN HAL 또는 레지스터에 ID/DLC/data 기록
  -> 송신 완료 interrupt
  -> confirmation callback
```

수신 흐름은 반대이다.

```text
CAN Rx interrupt
  -> 수신 ID/DLC/data 획득
  -> Rx 설정 테이블에서 ID 매칭
  -> Rx 버퍼 복사
  -> IL 또는 TP precopy callback 호출
```

BusOff는 CAN 컨트롤러가 버스 오류 누적으로 통신 불능 상태가 되었음을 의미한다. Driver는 BusOff를 감지하면 NM 또는 CCL에 알리고, 복구 정책은 상위 계층이 결정한다.

### CAN Driver의 책임 범위

CAN Driver는 프로토콜을 해석하지 않는다. Driver가 알아야 하는 것은 CAN ID, frame format, DLC, data, mailbox, interrupt, controller state이다. UDS 서비스 ID나 IL 신호 의미는 알면 안 된다. 계층을 잘 나누면 Driver는 다음만 담당한다.

- CAN controller 초기화
- Tx mailbox 또는 Tx queue 관리
- Rx mailbox/filter 설정
- 송신 요청을 hardware frame으로 변환
- 수신 hardware frame을 software Rx handle로 매핑
- Tx confirmation 전달
- BusOff, wakeup, error interrupt 처리

이 책임 범위를 넘어서면 Driver가 IL/TP/UDS에 종속되고, CAN-FD 전환이나 다른 MCU 이식이 어려워진다.

### SPC584C70에서 Driver가 고려할 하드웨어 요소

`SPC584C70` MCU로 이식할 때 CAN Driver는 다음 하드웨어 항목과 연결된다.

- M_CAN controller instance 선택
- peripheral clock enable, host clock, protocol clock source 설정
- CAN bit timing register 설정
- CAN-FD 사용 시 data phase bit timing과 BRS 설정
- shared Message RAM offset과 payload object size 설정
- Rx filter/acceptance mask 설정
- Tx/Rx interrupt vector 등록
- BusOff/error state interrupt 처리
- controller freeze/init/normal mode 전환

이 단계는 실제 `SPC584C70` 보드에서 수행한다. `Can_Init()`이 아무 인자 없이 동작하더라도 내부적으로는 `CanControllerConfig` 테이블을 읽고, 해당 M_CAN instance의 clock, Message RAM, filter, interrupt를 초기화하는 구조가 좋다.

### Tx 경로의 핵심 상태

송신은 단순히 `memcpy` 후 끝나는 일이 아니다. 실제 ECU에서는 다음 상태를 관리한다.

1. 상위 계층이 Tx handle로 송신 요청
2. Driver가 handle 유효성, online 상태, DLC/length 검사
3. 사용 가능한 Tx mailbox 또는 queue 확인
4. CAN ID, frame format, DLC, data를 hardware register/message RAM에 기록
5. 송신 요청 bit set
6. Tx complete interrupt 발생
7. confirmation flag/callback 처리

Tx queue가 있으면 mailbox가 바쁠 때 요청을 보관하고, confirmation 시 다음 frame을 꺼내 전송한다.

### Rx 경로의 핵심 상태

수신은 Rx filter가 먼저 frame을 통과시키고, Driver가 ID를 기준으로 Rx handle을 찾는다. FullCAN 방식은 특정 mailbox가 특정 ID를 받도록 설정하고, BasicCAN 방식은 넓은 filter로 받은 뒤 software에서 ID를 찾는다. CBD 설정에는 FullCAN과 BasicCAN 개념이 함께 보인다.

Rx 처리의 주의점은 ISR에서 너무 많은 일을 하지 않는 것이다. ISR에서는 frame을 안전한 buffer로 복사하고 flag를 세운 뒤, task context에서 IL/TP callback을 호출하는 설계가 안정적이다. 단, latency가 중요한 경우 일부 precopy는 ISR에서 처리할 수 있다.

### CAN-FD 확장 여지

Classical CAN Driver를 작성할 때도 `dlc`와 `length`를 구분해두면 좋다. Classical CAN에서는 둘이 거의 같지만 CAN-FD에서는 DLC 9가 12바이트를 의미한다. Driver 내부 frame 구조체는 처음부터 다음 필드를 고려한다.

- `is_extended`
- `is_fd`
- `brs`
- `esi`
- `dlc`
- `length`
- `data[64]`

## CBD 코드에서 관찰할 포인트

먼저 다음 함수를 중심으로 본다.

- `CanInitPowerOn`
- `CanInit`
- `CanTransmit`
- `CanMsgTransmit`
- `CanRxFullCANTask`
- `CanRxBasicCANTask`
- `CanHL_ReceivedRxHandle`
- `CanHL_TxConfirmation`
- `CanOnline`
- `CanOffline`
- `CanSleep`
- `CanWakeUp`
- `CanStart`
- `CanStop`

## 예제 코드

### can.h

```c
#ifndef CAN_H
#define CAN_H

#include "platform_types.h"
#include "can_par.h"

typedef void (*CanRxCallback)(CanRxHandle rx);
typedef void (*CanTxCallback)(CanTxHandle tx);
typedef void (*CanBusOffCallback)(void);

void Can_Init(void);
vuint8 Can_Transmit(CanTxHandle tx);
void Can_RxIndication(vuint16 id, const vuint8 *data, vuint8 dlc);
void Can_SetRxCallback(CanRxCallback callback);
void Can_SetTxCallback(CanTxCallback callback);
void Can_SetBusOffCallback(CanBusOffCallback callback);
void Can_SetOnline(void);
void Can_SetOffline(void);

#endif
```

### can.c

```c
#include "can.h"
#include "vstdlib.h"

static vuint8 g_can_online;
static CanRxCallback g_rx_callback;
static CanTxCallback g_tx_callback;
static CanBusOffCallback g_busoff_callback;

void Can_Init(void)
{
    g_can_online = 1u;
}

void Can_SetRxCallback(CanRxCallback callback)
{
    g_rx_callback = callback;
}

void Can_SetTxCallback(CanTxCallback callback)
{
    g_tx_callback = callback;
}

void Can_SetBusOffCallback(CanBusOffCallback callback)
{
    g_busoff_callback = callback;
}

void Can_SetOnline(void)
{
    g_can_online = 1u;
}

void Can_SetOffline(void)
{
    g_can_online = 0u;
}

vuint8 Can_Transmit(CanTxHandle tx)
{
    const CanTxObjectConfig *cfg;

    if (g_can_online == 0u)
    {
        return 1u;
    }

    cfg = Can_GetTxConfig(tx);
    if (cfg == 0)
    {
        return 2u;
    }

    /*
     * 초기 bring-up:
     * 실제 구현에서는 여기서 CAN HAL 송신 함수 또는 레지스터를 호출한다.
     */

    if (g_tx_callback != 0)
    {
        g_tx_callback(tx);
    }

    return 0u;
}

void Can_RxIndication(vuint16 id, const vuint8 *data, vuint8 dlc)
{
    CanRxHandle rx;

    for (rx = 0u; rx < CanRxCount; rx++)
    {
        if ((CanRxConfig[rx].id == id) && (dlc <= CanRxConfig[rx].dlc))
        {
            VStdRamMemCpy(CanRxConfig[rx].data, data, dlc);

            if (g_rx_callback != 0)
            {
                g_rx_callback(rx);
            }
            return;
        }
    }
}

void Can_TestForceBusOff(void)
{
    g_can_online = 0u;

    if (g_busoff_callback != 0)
    {
        g_busoff_callback();
    }
}
```


## 단계 산출물

이 단계의 결과물은 실제 CAN controller를 추상화하는 Driver 계층이다.

- `middleware/can/can.h`: `Can_Init`, `Can_Write`, `Can_MainFunction`, callback API
- `middleware/can/can.c`: 상태 관리, Tx queue, Rx dispatch, BusOff 감지 연결
- `middleware/can/can_hal.h`: SPC584C70 레지스터 접근을 감추는 HAL 인터페이스
- `middleware/can/can_hal_spc584c70.c`: SPC584C70 HS-CAN용 HAL skeleton
- `bringup/can/hscan_bringup.c`: internal loopback, analyzer 송수신, BusOff 관찰용 bring-up 코드

HS-CAN 미들웨어의 첫 번째 동작 목표는 `Can_Init -> Can_Write -> Tx confirmation -> Rx indication` 흐름이 실제 보드와 CAN analyzer에서 확인되는 것이다. CAN-FD 미들웨어에서는 같은 API로 FD frame을 보낼 수 있도록 `CanFrame` format 정보를 HAL까지 전달해야 한다.

## 구현 가이드

1. `can_hal_spc584c70.c`에서 M_CAN module clock enable, reset 해제, INIT/CCE 진입, nominal bit timing 설정, Message RAM 영역 초기화를 구현한다.
2. `CanControllerConfig`에 M_CAN instance 번호, Message RAM base/offset, Rx FIFO 크기, Tx buffer 수, interrupt line, transceiver enable GPIO를 넣는다.
3. acceptance filter를 먼저 최소 1개 ID로 설정한다. ST 예제처럼 filter가 없으면 수신 buffer에 저장되지 않을 수 있으므로, Rx path 확인 전 filter table을 먼저 검증한다.
4. internal loopback으로 Tx request bit, Tx complete interrupt, Rx FIFO new message flag를 디버거로 확인한다. 이후 external mode로 전환해 CAN analyzer에서 동일 ID/DLC/payload를 확인한다.
5. `Can_Write()`는 CCL online 상태, handle 범위, frame validation, mailbox busy 상태를 순서대로 검사한다. 실패 원인은 return code 또는 debug counter로 남겨 bring-up 중 즉시 구분 가능하게 한다.
6. BusOff는 analyzer 또는 transceiver 조건을 이용해 유도하고, error counter, interrupt flag, Driver state 전환을 확인한다.

### SPC584C70 M_CAN bring-up 체크리스트

| 순서 | 확인 항목 | 구현/관찰 포인트 |
|---|---|---|
| 1 | clock/pinmux/transceiver | M_CAN host/protocol clock enable, TX/RX pin alternate function, transceiver standby 해제 |
| 2 | INIT/설정 진입 | `CCCR.INIT=1`, `CCCR.CCE=1` 상태에서 bit timing과 Message RAM 설정 |
| 3 | nominal timing | `NBTP` 값이 목표 bitrate/sample point와 일치하는지 계산표와 비교 |
| 4 | filter base | `SIDFC`, `XIDFC`, `RXGFC` 설정 후 수신 ID가 filter에 매칭되는지 확인 |
| 5 | Rx 영역 | `RXF0C` 또는 Rx buffer 설정, `RXESC` payload size, Message RAM offset overlap 확인 |
| 6 | Tx 영역 | `TXBC`, `TXESC`, Tx element payload size, `TXBAR` request bit 확인 |
| 7 | interrupt | `IR` flag clear, `IE`, `ILS`, `ILE`, INTC vector 연결, ISR 진입 확인 |
| 8 | 상태 레지스터 | `PSR`, `ECR`, last error code, error passive, BusOff flag 확인 |
| 9 | Message RAM/ECC | reset 후 Message RAM 초기화, ECC error flag가 없는지 확인 |
| 10 | analyzer 확인 | analyzer ID/DLC/payload와 내부 `CanFrame` 값 비교 |

이 표는 bring-up 로그의 기준으로 사용한다. 각 단계가 끝날 때 register dump와 analyzer trace를 저장하면 이후 CAN-FD 전환 시 회귀 비교가 쉬워진다.

## 적용 고려사항과 트러블슈팅

SPC584C70에서 Driver 구현 시에는 CAN module clock enable, soft reset, bit timing, message buffer 초기화, interrupt routing, pin mux, transceiver standby 제어가 모두 맞아야 실제 bus에 frame이 나온다. 이 중 하나라도 빠지면 소프트웨어 로직이 맞아도 통신이 되지 않는다.

| 문제 케이스 | 원인 | 확인/대응 |
|---|---|---|
| `Can_Write()`는 성공하지만 bus에 frame이 없음 | pin mux, transceiver enable, mailbox request 누락 | oscilloscope/CAN analyzer로 TXD와 CANH/CANL을 분리 확인한다. |
| Rx interrupt는 발생하지만 상위 callback이 호출되지 않음 | hardware object와 software handle 매핑 오류 | Rx object index, filter ID, callback table index를 로그로 남긴다. |
| BusOff 후 복구되지 않음 | controller reset/recovery sequence 누락 | BusOff 상태 진입, offline 전환, 재초기화, online 복귀 단계를 상태도로 검증한다. |
| CAN-FD frame 송신 실패 | payload size 설정 또는 FD enable 누락 | message buffer payload size, FDF/BRS bit, data phase timing 설정을 확인한다. |

Driver 문제는 “레지스터 설정 dump -> Tx mailbox pending bit -> interrupt flag -> analyzer frame” 순서로 좁혀가며 확인한다.

## 완료 기준

- Tx handle만으로 실제 M_CAN 송신 요청을 만들 수 있다.
- 수신 ID를 Rx handle로 매핑할 수 있다.
- Rx callback과 Tx callback을 통해 상위 계층이 연결된다.
- BusOff callback을 NM 단계에서 사용할 수 있다.
