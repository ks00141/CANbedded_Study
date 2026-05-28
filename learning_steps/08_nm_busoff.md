# 8단계: NM / BusOff 복구 학습

## 학습 목표

NM 계층은 네트워크 상태와 BusOff 복구를 관리한다. 이 단계에서는 CAN Driver가 BusOff를 감지했을 때 통신을 중지하고 일정 시간이 지난 뒤 복구하는 상태 머신을 학습한다.

## 참조 파일

- `src/CBD/Nm/nm_basic.h`
- `src/CBD/Nm/nm_basic.c`
- `src/CBD/Gen/nmb_cfg.h`
- `src/CBD/Gen/nmb_par.*`

## 이론 설명

CAN BusOff는 CAN 컨트롤러가 오류 누적으로 버스에서 분리된 상태이다. 일반적으로 즉시 통신을 재개하지 않고, 복구 타이머를 둔 뒤 CAN controller를 재초기화하거나 online으로 전환한다.

Basic NM의 핵심 책임은 다음과 같다.

- BusOff 발생 감지
- 통신 중지
- 빠른 복구 시도
- 반복 BusOff 시 느린 복구로 전환
- 복구 완료 후 통신 재개
- 애플리케이션 또는 CCL에 상태 통지

### BusOff가 발생하는 이유

CAN은 오류 검출과 오류 제한 기능을 프로토콜에 포함한다. 송신 오류가 반복되면 controller의 transmit error counter가 증가하고, 일정 수준을 넘으면 error passive, 더 심하면 BusOff 상태가 된다. BusOff 상태에서는 해당 노드가 버스에 영향을 주지 않도록 통신에서 분리된다.

BusOff는 단순 software 오류가 아니라 배선 문제, termination 문제, bitrate mismatch, transceiver 문제, 심한 EMC 노이즈, 잘못된 CAN-FD timing 설정 등으로 발생할 수 있다. 그래서 Driver는 BusOff를 감지하고, NM은 복구 정책을 실행한다.

### NM과 CAN Driver의 경계

CAN Driver는 BusOff interrupt를 감지할 수 있지만, “얼마나 기다렸다가 복구할지”, “반복되면 느리게 복구할지”, “애플리케이션에 어떤 상태를 알릴지”는 네트워크 관리 정책이다.

역할 분리는 다음과 같다.

```text
CAN Driver
  -> BusOff 감지
  -> NmBasicCanBusOff callback

NM
  -> 통신 중지
  -> 복구 타이머 시작
  -> 필요 시 CAN 재초기화 요청
  -> 복구 완료 통지
```

### 빠른 복구와 느린 복구

BusOff가 드물게 한 번 발생했다면 빠르게 복구해도 된다. 하지만 짧은 시간에 반복 BusOff가 발생한다면 버스 상태가 불안정하다는 뜻이므로 느린 복구로 전환해야 한다. CBD의 Basic NM 설정에는 fast recovery time, slow recovery time, fast-to-slow 전환 기준이 있다.

학습용 구현에서는 BusOff 횟수를 세고, 일정 횟수 이상이면 recovery timer를 길게 잡는 방식으로 충분하다.

### SPC58xC 적용 관점

`SPC58xC`의 CAN/CAN-FD controller는 BusOff, error warning, error passive 같은 상태를 register/interrupt로 제공한다. 실제 이식에서는 다음이 중요하다.

- BusOff interrupt가 어느 controller instance에서 발생했는지 식별
- BusOff 상태에서 controller를 어떤 순서로 freeze/init/normal mode로 되돌릴지 정의
- CAN-FD data phase timing 오류가 BusOff 반복을 만들 수 있으므로 timing 설정 검증
- transceiver enable/standby pin이 있다면 NM/CCL 상태와 함께 제어
- watchdog이 있는 시스템에서 복구 루프가 watchdog policy와 충돌하지 않도록 설계

### NM 상태와 진단

진단 중 BusOff가 발생하면 UDS 응답을 보내지 못할 수 있다. 따라서 NM 상태는 CCL과 UDS에도 영향을 준다. 예를 들어 BusOff 중에는 CommunicationControl 요청을 처리할 수 없거나, response pending 이후 timeout이 발생할 수 있다. 학습용 구조에서도 NM 상태를 읽을 수 있는 API를 두면 상위 계층이 방어적으로 동작할 수 있다.

## CBD 코드에서 관찰할 포인트

- `NmBasicInitPowerOn`
- `NmBasicInit`
- `NmBasicCanBusOff`
- `NmBasicTask`
- `NmBasicStart`
- `NmBasicStop`
- `NmBasicGetNetState`

`nmb_cfg.h`에서 복구 타이밍 설정도 확인한다.

- `cNmBasicBusOffRecTime`
- `cNmBasicBusOffRecTimeSlow`
- `cNmBasicBusOffChangeFastToSlow`
- `cNmBasicBusOffRepairedTime`

## 예제 코드

### nm_basic.c

```c
#include "platform_types.h"
#include "can.h"

typedef enum
{
    NM_STATE_STOPPED,
    NM_STATE_NORMAL,
    NM_STATE_BUSOFF_FAST_RECOVERY,
    NM_STATE_BUSOFF_SLOW_RECOVERY
} NmState;

static NmState g_nm_state;
static vuint16 g_recovery_timer;
static vuint16 g_busoff_count;

#define NM_FAST_RECOVERY_TICKS  5u
#define NM_SLOW_RECOVERY_TICKS  100u
#define NM_FAST_TO_SLOW_COUNT   3u

void NmBasic_Init(void)
{
    g_nm_state = NM_STATE_NORMAL;
    g_recovery_timer = 0u;
    g_busoff_count = 0u;
}

void NmBasic_Start(void)
{
    g_nm_state = NM_STATE_NORMAL;
    Can_SetOnline();
}

void NmBasic_Stop(void)
{
    g_nm_state = NM_STATE_STOPPED;
    Can_SetOffline();
}

void NmBasic_CanBusOff(void)
{
    Can_SetOffline();
    g_busoff_count++;

    if (g_busoff_count >= NM_FAST_TO_SLOW_COUNT)
    {
        g_nm_state = NM_STATE_BUSOFF_SLOW_RECOVERY;
        g_recovery_timer = NM_SLOW_RECOVERY_TICKS;
    }
    else
    {
        g_nm_state = NM_STATE_BUSOFF_FAST_RECOVERY;
        g_recovery_timer = NM_FAST_RECOVERY_TICKS;
    }
}

void NmBasic_Task(void)
{
    if ((g_nm_state != NM_STATE_BUSOFF_FAST_RECOVERY) &&
        (g_nm_state != NM_STATE_BUSOFF_SLOW_RECOVERY))
    {
        return;
    }

    if (g_recovery_timer > 0u)
    {
        g_recovery_timer--;
    }

    if (g_recovery_timer == 0u)
    {
        Can_Init();
        Can_SetOnline();
        g_nm_state = NM_STATE_NORMAL;
    }
}

NmState NmBasic_GetState(void)
{
    return g_nm_state;
}
```

## 연습문제

1. BusOff 복구 성공 후 일정 시간 동안 BusOff가 다시 발생하지 않으면 `g_busoff_count`를 0으로 초기화하라.
2. BusOff 시작/종료 시 애플리케이션 callback을 호출하도록 구현하라.
3. `NM_STATE_STOPPED` 상태에서는 BusOff callback이 들어와도 복구하지 않도록 처리하라.
4. 빠른 복구와 느린 복구 시간을 설정 파일에서 가져오도록 바꿔라.
5. 복구 횟수와 현재 상태를 읽는 debug API를 추가하라.

## 완료 기준

- CAN Driver의 BusOff callback과 NM이 연결된다.
- BusOff 발생 시 CAN 송신이 중지된다.
- 주기 task를 통해 복구 타이머가 동작한다.
- 복구 완료 시 CAN이 다시 online 상태가 된다.
