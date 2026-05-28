# 2단계: Generated Config 학습

## 학습 목표

CBD 미들웨어는 많은 기능이 설정 매크로로 켜지고 꺼진다. 이 단계에서는 설정 파일이 구현 코드를 어떻게 제어하는지, 그리고 생성 코드 구조를 직접 설계할 때 어떤 값을 설정으로 빼야 하는지 학습한다.

## 참조 파일

- `src/CBD/Gen/v_cfg.h`
- `src/CBD/Gen/can_cfg.h`
- `src/CBD/Gen/il_cfg.h`
- `src/CBD/Gen/tp_cfg.h`
- `src/CBD/Gen/nmb_cfg.h`
- `src/CBD/Gen/ccl_cfg.h`

## 이론 설명

미들웨어 구현 코드는 가급적 재사용되어야 하고, 프로젝트마다 달라지는 값은 설정 파일에 있어야 한다.

예를 들어 다음 정보는 프로젝트마다 다르다.

- CAN 채널 수
- CAN Tx/Rx 오브젝트 수
- 표준 ID/확장 ID 사용 여부
- TP 주소 방식
- 진단 서비스 활성 여부
- 주기 task 시간
- BusOff 복구 시간

Vector 계열 코드는 이런 값을 대부분 `#define`으로 표현한다. C 컴파일러는 전처리 단계에서 사용하지 않는 기능을 제거할 수 있으므로, 임베디드 환경에서 코드 크기를 줄이는 데 유리하다.

## CBD 코드에서 관찰할 포인트

`v_cfg.h`에서 활성 모듈을 확인한다.

- `VGEN_ENABLE_CAN_DRV`
- `VGEN_ENABLE_IL_VECTOR`
- `VGEN_ENABLE_TP_ISO_MC`
- `VGEN_ENABLE_DIAG_CANDESC_UDS`
- `VGEN_ENABLE_NM_BASIC`
- `VGEN_ENABLE_CCL`

`can_cfg.h`에서 오브젝트 수와 기능 스위치를 확인한다.

- `kCanNumberOfChannels`
- `kCanNumberOfTxObjects`
- `kCanNumberOfRxObjects`
- `C_ENABLE_TRANSMIT_QUEUE`
- `C_DISABLE_EXTENDED_ID`

## 예제 코드

### v_cfg.h

```c
#ifndef MW_V_CFG_H
#define MW_V_CFG_H

#define MW_ENABLE_CAN_DRIVER
#define MW_ENABLE_IL
#define MW_ENABLE_ISOTP
#define MW_ENABLE_UDS
#define MW_ENABLE_NM
#define MW_ENABLE_CCL

#define MW_TASK_PERIOD_MS 10u

#endif
```

### can_cfg.h

```c
#ifndef MW_CAN_CFG_H
#define MW_CAN_CFG_H

#define MW_NUM_CAN_CHANNELS   1u
#define MW_NUM_CAN_TX_OBJECTS 16u
#define MW_NUM_CAN_RX_OBJECTS 7u

#define MW_CAN_USE_STANDARD_ID 1u
#define MW_CAN_USE_EXTENDED_ID 0u
#define MW_CAN_USE_TX_QUEUE    1u

#endif
```

### 설정값을 사용하는 구현 코드

```c
#include "can_cfg.h"
#include "platform_types.h"

typedef struct
{
    vuint8 online;
} CanChannelState;

static CanChannelState g_can_channel[MW_NUM_CAN_CHANNELS];

void Can_Init(void)
{
    for (vuint8 i = 0u; i < MW_NUM_CAN_CHANNELS; i++)
    {
        g_can_channel[i].online = 1u;
    }
}
```

## 연습문제

1. `MW_ENABLE_ISOTP`가 정의된 경우에만 `IsoTp_Init()` 호출 코드가 컴파일되도록 작성하라.
2. `MW_NUM_CAN_TX_OBJECTS` 값을 4로 바꿨을 때 Tx 설정 테이블 크기도 같이 바뀌도록 구현하라.
3. `MW_CAN_USE_EXTENDED_ID`가 0이면 29비트 ID 송신을 거부하는 코드를 작성하라.
4. 설정 파일을 `project_a`와 `project_b`로 나누고, 같은 구현 코드가 다른 설정으로 컴파일되게 구성하라.

## 완료 기준

- 숫자 상수를 구현 파일에 직접 쓰지 않는다.
- 기능 스위치 매크로로 코드 포함 여부를 제어할 수 있다.
- 설정 파일만 바꿔도 테이블 크기와 동작 정책이 바뀐다.
