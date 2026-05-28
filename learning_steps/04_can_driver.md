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
     * 학습용 mock:
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

## 연습문제

1. `Can_Transmit()`에서 Tx handle 범위 오류, offline 오류, DLC 오류를 서로 다른 return code로 구분하라.
2. 수신 ID가 테이블에 없을 때 호출되는 `CanNotMatchedCallback`을 추가하라.
3. Tx confirmation flag 배열을 만들고, 송신 완료 시 해당 flag를 1로 설정하라.
4. 송신 큐를 간단한 ring buffer로 구현하라.
5. BusOff 발생 후에는 송신 요청이 실패하도록 구현하라.

## 완료 기준

- Tx handle만으로 CAN 송신 mock이 가능하다.
- 수신 ID를 Rx handle로 매핑할 수 있다.
- Rx callback과 Tx callback을 통해 상위 계층이 연결된다.
- BusOff callback을 NM 단계에서 사용할 수 있다.
