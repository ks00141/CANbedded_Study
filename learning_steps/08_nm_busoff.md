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
