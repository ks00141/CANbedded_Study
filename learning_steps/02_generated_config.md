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

### 설정 파일과 구현 파일을 분리하는 이유

통신 미들웨어는 제품마다 다른 CAN DB, 진단 DB, 네트워크 채널 수, 메시지 주기, BusOff 정책을 사용한다. 이 차이를 구현 코드 안에 직접 넣으면 프로젝트가 바뀔 때마다 driver와 protocol 코드를 수정해야 한다. 반면 설정 파일에 차이를 몰아두면 구현 코드는 재사용하고, 생성 파일 또는 설정 파일만 바꾸는 구조가 된다.

CBD의 `Gen` 폴더는 바로 이 역할을 한다. `can_cfg.h`는 CAN Driver의 기능 스위치와 오브젝트 수를 알려주고, `tp_cfg.h`는 ISO-TP 주소 방식과 callback 연결을 알려주며, `il_cfg.h`는 IL의 Tx/Rx 사용 여부와 주기 시간을 알려준다. 구현 파일은 이런 매크로를 읽어 compile-time에 필요한 코드만 포함한다.

### compile-time 설정과 run-time 설정

설정은 크게 두 종류로 나눌 수 있다.

- compile-time 설정: 빌드 후 바뀌지 않는 구조적 설정이다. 예: 채널 수, Tx object 수, CAN-FD 지원 여부, 기능 enable/disable.
- run-time 설정: ECU 동작 중 바뀔 수 있는 상태이다. 예: online/offline, current session, security unlocked, BusOff recovery state.

초보 구현에서 자주 하는 실수는 이 둘을 섞는 것이다. 예를 들어 `MW_NUM_CAN_TX_OBJECTS`는 compile-time 설정이고, `Can_SetOffline()`은 run-time 상태 변경이다. 구조체와 매크로 이름에서 이 둘을 명확히 구분하면 코드가 훨씬 읽기 쉬워진다.

### SPC58EC70 적용 관점

`SPC58EC70`에서는 설정 계층이 MCU 주변장치 초기화와 직접 연결된다. 이 MCU는 8개 Bosch M_CAN 기반 ISO CAN-FD 인터페이스를 제공하며 두 CAN subsystem 안에서 Message RAM, clock, ECC 자원을 공유한다. 따라서 M_CAN 인스턴스 번호, shared Message RAM offset, interrupt vector, host/protocol clock, CAN bit timing, payload size, filter table 크기를 모두 설정값으로 관리하는 편이 좋다.

예를 들어 학습용 설정에는 처음에 다음 정도만 있어도 된다.

```c
#define MW_TARGET_SPC58EC70 1u
#define MW_MCAN_INSTANCE_COUNT 8u
#define MW_CAN_CONTROLLER_COUNT 1u
#define MW_CAN0_MCAN_INSTANCE 1u
#define MW_CAN0_MSG_RAM_WORD_OFFSET 0u
#define MW_CANFD_ENABLED 1u
#define MW_CAN_ARBITRATION_BITRATE 500000u
#define MW_CANFD_DATA_BITRATE 2000000u
```

실제 포팅 단계에서는 이 값들이 SPC58EC70의 clock tree와 CAN controller timing register 설정으로 연결된다. 따라서 설정 파일은 단순 숫자 모음이 아니라 “생성 코드와 하드웨어 초기화 사이의 계약서”로 보아야 한다.

### 설정 설계 원칙

1. 크기 값은 한 곳에서만 정의한다.
2. 메시지별 속성은 전역 설정으로 뭉개지 않는다.
3. Classical CAN과 CAN-FD가 공존할 수 있게 메시지별 frame format을 둔다.
4. callback 연결은 설정 테이블로 분리한다.
5. 진단/TP/IL이 CAN Driver 내부 구조를 직접 알지 않게 한다.

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

## 단계 산출물

이 단계의 결과물은 “코드를 바꾸지 않고 설정만 바꿔 미들웨어 구성이 달라지는 구조”이다.

- `middleware/config/v_cfg.h`: 미들웨어 기능 on/off, 주기, 빌드 variant 정의
- `middleware/config/can_cfg.h`: 채널 수, Tx/Rx object 수, ID 타입, queue 사용 여부, Classical CAN/CAN-FD 사용 여부
- `middleware/config/can_par.h`, `middleware/config/can_par.c`: Tx/Rx message object, filter, baudrate, callback table
- `tools/config_schema.md`: 사람이 설정 파일을 작성할 때 지켜야 할 필드 규칙
- `tests/config/test_config_size.c`: 설정값과 테이블 크기가 일치하는지 검증하는 테스트

HS-CAN 결과물은 `MW_CAN_USE_FD == 0`인 설정 variant로 생성하고, CAN-FD 결과물은 `MW_CAN_USE_FD == 1` 및 메시지별 FD/BRS/length 정책을 포함한 variant로 생성한다. 두 variant는 같은 구현 코드를 공유하고 설정 파일만 다르게 가져가는 구성이 목표다.

## 적용 고려사항과 트러블슈팅

설정 계층은 이후 모든 계층의 compile-time contract 역할을 한다. SPC58EC70 보드에서는 CAN clock source, M_CAN instance, shared Message RAM 영역, pin mux, transceiver enable pin 같은 하드웨어 조건도 설정의 일부로 다루어야 한다.

| 문제 케이스 | 원인 | 확인/대응 |
|---|---|---|
| Tx object 수를 바꿨더니 런타임에서 배열 범위를 벗어남 | 매크로와 설정 테이블 길이 불일치 | `MW_NUM_CAN_TX_OBJECTS`와 `CanTxConfig[]` 길이를 compile-time assert로 묶는다. |
| HS-CAN 빌드에 CAN-FD 필드가 섞여 빌드 오류 발생 | variant 분리 기준이 불명확 | Classical 공용 필드와 FD 전용 필드를 구조체/매크로로 명확히 분리한다. |
| 보드에서 통신 속도가 맞지 않음 | SPC58EC70 CAN clock, prescaler, sample point 계산 오류 | clock tree와 CAN nominal/data bit timing 계산표를 설정 문서에 함께 남긴다. |
| 프로젝트 A 설정이 프로젝트 B 빌드에 섞임 | include path 우선순위 오류 | variant별 config 디렉터리를 만들고 빌드 로그에서 include 경로 순서를 확인한다. |

트러블슈팅은 먼저 “현재 빌드가 어떤 variant 설정을 include했는지” 확인하는 것부터 시작한다. 설정 헤더에 `MW_CONFIG_VARIANT_NAME` 문자열 또는 매크로를 두면 로그와 map 파일에서 추적하기 쉽다.

## 완료 기준

- 숫자 상수를 구현 파일에 직접 쓰지 않는다.
- 기능 스위치 매크로로 코드 포함 여부를 제어할 수 있다.
- 설정 파일만 바꿔도 테이블 크기와 동작 정책이 바뀐다.
