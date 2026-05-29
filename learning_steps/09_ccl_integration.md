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

### CCL이 필요한 이유

CAN Driver에는 online/offline 함수가 있을 수 있지만, 누가 어떤 이유로 통신을 켜야 하는지까지 알지는 못한다. 애플리케이션은 정상 운전 상태에서 통신을 요청할 수 있고, 진단은 TesterPresent나 active session 동안 통신을 유지해야 할 수 있으며, NM은 BusOff 중 통신을 강제로 막아야 한다. CCL은 이 요청들을 한 곳에서 합산해 실제 통신 상태를 결정한다.

CCL 없이 각 계층이 직접 `Can_SetOnline()`과 `Can_SetOffline()`을 호출하면 다음 문제가 생긴다.

- 진단이 통신을 요구 중인데 애플리케이션이 offline으로 내려버릴 수 있다.
- BusOff 복구 중인데 다른 계층이 online으로 올릴 수 있다.
- sleep 진입 조건을 판단하기 어렵다.
- CommunicationControl 서비스가 IL/TP/CAN 어디까지 막아야 하는지 불명확해진다.

### 요청 기반 상태 모델

가장 단순한 CCL 모델은 사용자별 request bit를 두는 것이다.

```text
request[APP]  = 1
request[DIAG] = 0
request[NM]   = 0
=> communication requested
```

하지만 실제 ECU에서는 inhibit 조건도 필요하다.

```text
any request == true
busoff inhibit == false
sleep inhibit == false
=> communication active
```

즉 CCL은 “요청”과 “금지 조건”을 모두 고려한다.

### CommunicationControl과 CCL

UDS `0x28 CommunicationControl`은 진단기가 ECU의 송수신을 제어하는 서비스이다. 이 서비스는 단순히 CAN Driver를 offline으로 내리는 것과 다르다. 예를 들어 Rx는 허용하고 Tx만 막거나, Tx는 허용하고 Rx만 막는 조합이 가능하다.

따라서 CCL에는 최소한 다음 상태가 있어야 한다.

- network requested 여부
- Tx enabled 여부
- Rx enabled 여부
- busoff inhibit 여부
- sleep allowed 여부

CAN Driver의 `Can_Transmit()`은 CCL의 Tx enabled 상태를 확인할 수 있고, Rx indication 경로는 Rx enabled 상태에 따라 상위 계층 전달을 막을 수 있다.

### SPC584C70 적용 관점

`SPC584C70`에서 CCL은 MCU의 CAN controller mode와 transceiver 제어에도 연결될 수 있다. 실제 시스템에는 CAN transceiver standby pin, wakeup pin, SBC(System Basis Chip) 제어가 있을 수 있다. CCL은 이런 하드웨어 제어를 직접 레지스터 수준으로 처리하기보다, `CanIf` 또는 board support 함수로 추상화하는 편이 좋다.

또한 CAN-FD controller가 Classical CAN mailbox와 FD mailbox를 모두 갖는 구조라면, CCL의 Tx/Rx enable 정책이 두 frame format 모두에 적용되어야 한다. 진단만 CAN-FD로 올라가고 일반 신호는 Classical CAN으로 남는 혼합 구조도 가능하므로, CCL은 frame format을 가리지 않는 상위 정책 계층이어야 한다.

### 설계 기준

1. CCL은 통신 정책을 결정한다.
2. CAN Driver는 CCL이 정한 online/offline/Tx enable 상태를 실행한다.
3. UDS CommunicationControl은 CCL API를 통해 상태를 바꾼다.
4. NM BusOff는 CCL에 inhibit 조건으로 반영된다.
5. 애플리케이션은 직접 CAN Driver를 내리지 않고 CCL에 요청/해제를 알린다.

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


## 단계 산출물

이 단계의 결과물은 앞 단계 산출물을 하나의 통신 미들웨어로 묶는 Communication Control Layer이다.

- `middleware/ccl/ccl.h`: 미들웨어 초기화, online/offline, Tx enable/disable, main function API
- `middleware/ccl/ccl.c`: Can/IL/TP/UDS/NM 초기화 순서와 상태 전파
- `middleware/ccl/ccl_cfg.h`: 채널별 기능 사용 여부, 진단/NM 연동 정책
- `examples/hs_can_middleware/main.c`: SPC584C70 HS-CAN middleware 통합 예제
- `bringup/integration/hscan_stack_bringup.c`: 실제 보드에서 HS-CAN 전체 stack을 구동하는 통합 bring-up 코드

이 단계를 완료하면 첫 번째 최종 결과물인 “SPC584C70 MCU에서 HS-CAN 통신이 가능한 미들웨어”의 baseline이 만들어져야 한다. 이후 CAN-FD 단계는 이 baseline을 깨지 않고 확장하는 방식으로 진행한다.

## 구현 가이드

1. `Ccl_Init()`에서 Common, Config, Can, IL, ISO-TP, UDS, NM 순으로 초기화한다. interrupt enable은 모든 buffer와 callback table이 준비된 뒤 수행한다.
2. `Ccl_RequestCom()`과 `Ccl_ReleaseCom()`은 application, diagnostic, NM 요청을 분리해 관리한다. 하나라도 통신 요청이 있으면 online을 유지하고, 모든 요청이 해제되어야 offline 전환을 허용한다.
3. `Ccl_SetTxAllowed()`와 `Ccl_SetRxAllowed()`를 만들어 IL periodic Tx, event Tx, TP/UDS response가 모두 같은 gate를 통과하게 한다.
4. UDS `0x28 CommunicationControl`이 들어오면 CCL policy에 반영한다. 진단 요청이 일반 메시지 송신을 막아도 필요한 진단 응답은 OEM 정책에 맞게 허용/차단을 구분한다.
5. 실제 보드에서 `Ccl_MainFunction()` 주기를 고정하고 CAN analyzer로 주기 메시지, 진단 응답, BusOff 복구 후 재송신 동작을 확인한다.
6. 9단계 완료 산출물은 HS-CAN baseline이다. analyzer trace, map file, 설정 파일, 주요 register dump를 보관해 10단계 CAN-FD 전환의 회귀 기준으로 사용한다.

### HS-CAN baseline acceptance matrix

| 영역 | 확인 항목 | 완료 조건 |
|---|---|---|
| 주기 Tx | IL 주기 메시지 trace | ID, DLC, payload, period jitter가 설정 범위 안에 있음 |
| Rx IL | 외부 analyzer 송신 frame 수신 | Rx buffer와 signal accessor 값이 기대값과 일치 |
| ISO-TP | 8바이트 HS-CAN Multi-frame | FF/FC/CF 순서, SN rollover, timeout이 정상 |
| UDS 기본 | `0x10`, `0x3E`, `0x22 F190` | positive response와 NRC 정책이 기대값과 일치 |
| CommunicationControl | UDS `0x28` | CCL Tx/Rx gate가 IL/TP 송수신에 동일하게 적용 |
| BusOff | 의도적 bus error 조건 | Tx 차단, 복구 timer, re-init, online 복귀가 동작 |
| 회귀 자료 | trace/map/register dump | 10단계 CAN-FD 전환 전후 비교 기준으로 보관 |

## 적용 고려사항과 트러블슈팅

CCL은 각 계층의 API를 단순 호출하는 파일이 아니라 시스템 상태를 하나로 묶는 조정자다. 초기화 순서, offline 정책, diagnostic communication control, BusOff 전파가 이 계층에서 엇갈리면 개별 모듈 테스트는 통과해도 전체 시스템이 불안정해진다.

| 문제 케이스 | 원인 | 확인/대응 |
|---|---|---|
| 초기화 중 Rx interrupt가 먼저 들어옴 | Driver enable 시점이 너무 빠름 | 모든 buffer와 callback table 초기화 후 interrupt를 enable한다. |
| 진단 service로 통신 off를 요청했는데 주기 Tx가 계속됨 | CCL Tx gate가 IL periodic path에 적용되지 않음 | event Tx와 periodic Tx가 같은 `Ccl_IsTxAllowed()`를 통과하게 한다. |
| BusOff 후 UDS가 여전히 positive response 생성 | channel offline 상태가 TP/UDS에 전달되지 않음 | TP transmit request 단계에서 channel online 여부를 확인한다. |
| CAN-FD 확장 시 일부 계층이 Classical 전용 API 호출 | CCL 통합 API가 format 정보를 전달하지 않음 | frame format은 CanFrame/config를 통해 하위로 흐르게 하고 CCL에서 임의 변환하지 않는다. |

트러블슈팅은 전체 main function 호출 순서를 먼저 확인한다. 권장 순서는 `Ccl_MainFunction -> Nm/BusOff -> Can_MainFunction -> Il_MainFunction -> IsoTp_MainFunction -> Uds_MainFunction`처럼 상태 관리가 통신 처리보다 앞서도록 설계하는 것이다.

## 완료 기준

- 사용자별 통신 요청/해제 상태를 관리할 수 있다.
- 요청이 하나라도 있으면 통신 active, 모두 해제되면 stopped 상태가 된다.
- BusOff 상태가 CCL에 반영된다.
- UDS CommunicationControl과 Tx/Rx enable 상태가 연결된다.

## Q&A

1. Q: CCL은 왜 필요한가?
   A: 여러 계층과 application이 통신 enable/disable을 동시에 요구할 수 있기 때문이다. CCL은 Driver mode, IL 주기 송신, UDS CommunicationControl, BusOff 상태를 하나의 정책으로 조정한다.

2. Q: CCL이 CAN Driver register를 직접 제어해도 되는가?
   A: 권장하지 않는다. CCL은 `Can_SetControllerMode`, `CanIf` 또는 board support API 같은 추상화된 함수로 요청을 전달해야 한다. register 직접 접근은 Driver/HAL 책임이다.

3. Q: Tx disable과 Rx disable은 항상 같이 움직이는가?
   A: 아니다. UDS `0x28`이나 OEM 정책에 따라 Tx만 막고 Rx는 유지할 수 있다. CCL 상태는 `tx_enabled`, `rx_enabled`, `online`, `busoff`를 분리해서 표현하는 것이 좋다.

4. Q: 여러 사용자가 통신을 요청하면 어떻게 관리하는가?
   A: user별 request bit mask나 reference count를 사용한다. 하나라도 active이면 통신을 유지하고, 모든 request가 해제되었을 때 stopped로 전환한다.

5. Q: BusOff 중에 application이 통신 요청을 하면 어떻게 해야 하는가?
   A: 요청 자체는 기록할 수 있지만 실제 Tx enable은 막아야 한다. BusOff 복구 후 NM/Driver 상태가 정상으로 돌아오면 CCL이 pending request를 반영해 online으로 전환한다.

6. Q: IL 주기 송신은 CCL 상태를 어디에서 확인해야 하는가?
   A: IL이 송신 요청을 만들기 전 또는 Driver가 송신을 받기 전 둘 중 하나 이상에서 확인해야 한다. 이상적으로는 IL과 Driver 양쪽에 방어가 있어 잘못된 송신을 막는다.

7. Q: UDS CommunicationControl과 CCL은 어떻게 연결하는가?
   A: UDS handler는 sub-function을 해석한 뒤 CCL API를 호출한다. CCL은 요청자를 diagnostic user로 기록하고 Tx/Rx enable 정책을 갱신한다.

8. Q: main function 호출 순서가 중요한 이유는 무엇인가?
   A: 상태 관리가 늦게 실행되면 이미 막아야 할 Tx가 먼저 나갈 수 있다. 일반적으로 CCL/NM 상태 갱신을 먼저 하고 Driver, IL, TP, UDS 처리를 이어가는 흐름이 안전하다.

9. Q: CCL 통합 후 analyzer에서 무엇을 확인해야 하는가?
   A: normal 상태에서 주기 frame이 나오고, Tx disable 후 frame이 멈추며, Rx enable 상태에서는 진단 요청을 받을 수 있는지 확인한다. BusOff 후 복구 시 주기 송신 재개 시점도 기록한다.

10. Q: CAN-FD variant에서도 CCL을 별도로 만들어야 하는가?
    A: 별도 계층을 만들기보다 같은 CCL 정책을 Classical CAN과 CAN-FD frame 모두에 적용하는 것이 좋다. 단 Driver/HAL mode 설정과 FD frame validation은 variant별로 다를 수 있다.

11. Q: CCL 버그는 어떤 증상으로 나타나는가?
    A: 통신 disable 상태인데 frame이 나가거나, BusOff 복구 후 online으로 돌아오지 않거나, UDS `0x28` 이후 IL 송신만 멈추고 TP 송신은 계속되는 식으로 나타난다. Tx/Rx enable 적용 지점을 모두 확인해야 한다.
