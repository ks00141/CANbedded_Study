# 9단계: CCL / 통신 통합 제어 학습

## 학습 목표

CCL은 여러 계층의 통신 상태를 통합 제어한다. 이 단계에서는 애플리케이션, 진단, NM 등의 요청을 종합해 CAN/IL/TP 송수신을 켜고 끄는 구조를 학습한다.

## 참조 파일

- `src/CBD/Ccl/ccl.h`
- `src/CBD/Ccl/ccl_inc.h`
- `src/CBD/Ccl/ccl.c`
- `src/CBD/Gen/ccl_cfg.h`
- `src/CBD/Gen/ccl_par.*`

## 이론 설명

차량 ECU는 항상 CAN 통신을 켜 두지 않는다. 전원 상태, sleep 조건, 진단 세션, BusOff 상태에 따라 통신을 시작하거나 멈춘다.

CCL은 다음 요청을 통합한다.

- 일반 애플리케이션 통신 요청
- 진단 통신 요청
- BusOff 중 통신 중지
- UDS `0x28 CommunicationControl`
- sleep/wakeup 정책

핵심은 사용자별 요청 상태를 저장하고, 하나라도 요청 중이면 통신을 유지하는 것이다.

```text
APP request  = ON
DIAG request = OFF
=> communication active

APP request  = OFF
DIAG request = OFF
=> communication can stop
```

## CBD 코드에서 관찰할 포인트

- `CclRequestCommunication`
- `CclReleaseCommunication`
- `CclComStart`
- `CclComStop`
- `CclComWait`
- `CclComResume`
- `CclBusOffStart`
- `CclBusOffEnd`

`ccl_cfg.h`에서 사용자 핸들을 확인한다.

- `CCL_CommRequest`
- `CCL_DiagReq_0`

## 예제 코드

### ccl.c

```c
#include "platform_types.h"
#include "can.h"

typedef enum
{
    CCL_USER_APP,
    CCL_USER_DIAG,
    CCL_USER_COUNT
} CclUser;

typedef enum
{
    CCL_STATE_STOPPED,
    CCL_STATE_ACTIVE,
    CCL_STATE_BUSOFF
} CclState;

static vuint8 g_user_request[CCL_USER_COUNT];
static CclState g_ccl_state;
static vuint8 g_tx_enabled = 1u;
static vuint8 g_rx_enabled = 1u;

static vuint8 Ccl_HasAnyRequest(void)
{
    for (vuint8 i = 0u; i < CCL_USER_COUNT; i++)
    {
        if (g_user_request[i] != 0u)
        {
            return 1u;
        }
    }

    return 0u;
}

static void Ccl_ApplyState(void)
{
    if (g_ccl_state == CCL_STATE_BUSOFF)
    {
        Can_SetOffline();
        return;
    }

    if (Ccl_HasAnyRequest() != 0u)
    {
        Can_SetOnline();
        g_ccl_state = CCL_STATE_ACTIVE;
    }
    else
    {
        Can_SetOffline();
        g_ccl_state = CCL_STATE_STOPPED;
    }
}

void Ccl_Init(void)
{
    for (vuint8 i = 0u; i < CCL_USER_COUNT; i++)
    {
        g_user_request[i] = 0u;
    }

    g_tx_enabled = 1u;
    g_rx_enabled = 1u;
    g_ccl_state = CCL_STATE_STOPPED;
}

void Ccl_RequestCommunication(CclUser user)
{
    if (user >= CCL_USER_COUNT)
    {
        return;
    }

    g_user_request[user] = 1u;
    Ccl_ApplyState();
}

void Ccl_ReleaseCommunication(CclUser user)
{
    if (user >= CCL_USER_COUNT)
    {
        return;
    }

    g_user_request[user] = 0u;
    Ccl_ApplyState();
}

void Ccl_BusOffStart(void)
{
    g_ccl_state = CCL_STATE_BUSOFF;
    Can_SetOffline();
}

void Ccl_BusOffEnd(void)
{
    g_ccl_state = CCL_STATE_STOPPED;
    Ccl_ApplyState();
}

void Ccl_SetCommunicationMode(vuint8 enable_rx, vuint8 enable_tx)
{
    g_rx_enabled = enable_rx;
    g_tx_enabled = enable_tx;
}

vuint8 Ccl_IsTxEnabled(void)
{
    return g_tx_enabled;
}

vuint8 Ccl_IsRxEnabled(void)
{
    return g_rx_enabled;
}
```

## UDS 0x28과 연결 예시

```c
void AppDesc_SetCommModeFromUds(vuint8 control_type)
{
    switch (control_type)
    {
    case 0x00u:
        Ccl_SetCommunicationMode(1u, 1u);
        break;
    case 0x01u:
        Ccl_SetCommunicationMode(1u, 0u);
        break;
    case 0x02u:
        Ccl_SetCommunicationMode(0u, 1u);
        break;
    case 0x03u:
        Ccl_SetCommunicationMode(0u, 0u);
        break;
    default:
        break;
    }
}
```

## 연습문제

1. `Ccl_RequestCommunication()`이 중복 호출되어도 내부 상태가 꼬이지 않도록 구현하라.
2. BusOff 상태에서는 사용자 요청이 있어도 CAN online으로 전환되지 않도록 테스트하라.
3. `Ccl_IsTxEnabled()`가 0이면 `Can_Transmit()`이 실패하도록 CAN Driver와 연결하라.
4. `CCL_USER_DIAG` 요청이 들어오면 sleep 진입을 막는 정책을 추가하라.
5. CCL 상태 변화 시 애플리케이션 callback을 호출하도록 구현하라.

## 완료 기준

- 사용자별 통신 요청/해제 상태를 관리할 수 있다.
- 요청이 하나라도 있으면 통신 active, 모두 해제되면 stopped 상태가 된다.
- BusOff 상태가 CCL에 반영된다.
- UDS CommunicationControl과 Tx/Rx enable 상태가 연결된다.
