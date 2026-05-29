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

### SPC584C70 적용 관점

`SPC584C70`에서는 설정 계층이 MCU 주변장치 초기화와 직접 연결된다. 이 MCU는 8개 Bosch M_CAN 기반 ISO CAN-FD 인터페이스를 제공하며 두 CAN subsystem 안에서 Message RAM, clock, ECC 자원을 공유한다. 따라서 M_CAN 인스턴스 번호, shared Message RAM offset, interrupt vector, host/protocol clock, CAN bit timing, payload size, filter table 크기를 모두 설정값으로 관리하는 편이 좋다.

`SPC58EC70`과 CAN-FD 주변장치 설명은 상당 부분 공유할 수 있지만, 설정 파일에서는 두 MCU를 같은 타깃으로 취급하지 않는다. `SPC584C70`은 single computing core 계열이므로 startup core, linker memory map, interrupt target core, core-local RAM 설정을 `SPC58EC70` dual-core 설정에서 그대로 가져오면 안 된다.

예를 들어 학습용 설정에는 처음에 다음 정도만 있어도 된다.

```c
#define MW_TARGET_SPC584C70 1u
#define MW_CPU_COMPUTING_CORE_COUNT 1u
#define MW_MCAN_INSTANCE_COUNT 8u
#define MW_CAN_CONTROLLER_COUNT 1u
#define MW_CAN0_MCAN_INSTANCE 1u
#define MW_CAN0_MSG_RAM_WORD_OFFSET 0u
#define MW_CANFD_ENABLED 1u
#define MW_CAN_ARBITRATION_BITRATE 500000u
#define MW_CANFD_DATA_BITRATE 2000000u
```

실제 포팅 단계에서는 이 값들이 SPC584C70의 clock tree와 CAN controller timing register 설정으로 연결된다. 따라서 설정 파일은 단순 숫자 모음이 아니라 “생성 코드와 하드웨어 초기화 사이의 계약서”로 보아야 한다.

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


## 단계 산출물

이 단계의 결과물은 “코드를 바꾸지 않고 설정만 바꿔 미들웨어 구성이 달라지는 구조”이다.

- `middleware/config/v_cfg.h`: 미들웨어 기능 on/off, 주기, 빌드 variant 정의
- `middleware/config/can_cfg.h`: 채널 수, Tx/Rx object 수, ID 타입, queue 사용 여부, Classical CAN/CAN-FD 사용 여부
- `middleware/config/can_par.h`, `middleware/config/can_par.c`: Tx/Rx message object, filter, baudrate, callback table
- `tools/config_schema.md`: 사람이 설정 파일을 작성할 때 지켜야 할 필드 규칙
- `bringup/config/config_bringup.c`: 설정값, 테이블 크기, M_CAN instance 선택값을 디버거로 확인하는 bring-up 코드

HS-CAN 결과물은 `MW_CAN_USE_FD == 0`인 설정 variant로 생성하고, CAN-FD 결과물은 `MW_CAN_USE_FD == 1` 및 메시지별 FD/BRS/length 정책을 포함한 variant로 생성한다. 두 variant는 같은 구현 코드를 공유하고 설정 파일만 다르게 가져가는 구성이 목표다.

## 구현 가이드

1. `mw_variant.h`에서 `MW_VARIANT_HSCAN`, `MW_VARIANT_CANFD`, `MW_TARGET_SPC584C70`, `MW_CPU_COMPUTING_CORE_COUNT`를 정의한다. 빌드 로그에서 실제 variant가 무엇인지 확인할 수 있도록 `MW_CONFIG_VARIANT_NAME`도 둔다.
2. `spc584c70_clock_cfg.h`에 M_CAN host clock, protocol clock, nominal bitrate, CAN-FD data bitrate, sample point 목표값을 정리한다. `TN1346` 계산 결과를 설정 주석에 남겨 이후 Driver 초기화 값과 추적 가능하게 만든다.
3. `can_cfg.h`에는 software channel 수와 M_CAN instance 번호를 분리해 둔다. 예를 들어 channel 0이 M_CAN1을 쓰더라도 `MW_CAN0_MCAN_INSTANCE`처럼 명시한다.
4. `can_par.c`에는 Tx/Rx handle, CAN ID, ID type, expected length, FD 여부, BRS 여부, callback pointer를 테이블로 작성한다. 테이블 길이는 `MW_NUM_CAN_TX_OBJECTS`, `MW_NUM_CAN_RX_OBJECTS`와 compile-time assert로 묶는다.
5. 보드 bring-up에서 디버거로 `CanTxConfig[]`, `CanRxConfig[]`, clock 설정 구조체, variant macro 결과를 확인한다. 설정값이 틀리면 이후 Driver 계층에서 원인을 찾기 어려우므로 이 단계에서 고정한다.

## 적용 고려사항과 트러블슈팅

설정 계층은 이후 모든 계층의 compile-time contract 역할을 한다. SPC584C70 보드에서는 CAN clock source, M_CAN instance, shared Message RAM 영역, pin mux, transceiver enable pin 같은 하드웨어 조건도 설정의 일부로 다루어야 한다.

| 문제 케이스 | 원인 | 확인/대응 |
|---|---|---|
| Tx object 수를 바꿨더니 런타임에서 배열 범위를 벗어남 | 매크로와 설정 테이블 길이 불일치 | `MW_NUM_CAN_TX_OBJECTS`와 `CanTxConfig[]` 길이를 compile-time assert로 묶는다. |
| HS-CAN 빌드에 CAN-FD 필드가 섞여 빌드 오류 발생 | variant 분리 기준이 불명확 | Classical 공용 필드와 FD 전용 필드를 구조체/매크로로 명확히 분리한다. |
| 보드에서 통신 속도가 맞지 않음 | SPC584C70 CAN clock, prescaler, sample point 계산 오류 | clock tree와 CAN nominal/data bit timing 계산표를 설정 문서에 함께 남긴다. |
| 프로젝트 A 설정이 프로젝트 B 빌드에 섞임 | include path 우선순위 오류 | variant별 config 디렉터리를 만들고 빌드 로그에서 include 경로 순서를 확인한다. |

트러블슈팅은 먼저 “현재 빌드가 어떤 variant 설정을 include했는지” 확인하는 것부터 시작한다. 설정 헤더에 `MW_CONFIG_VARIANT_NAME` 문자열 또는 매크로를 두면 로그와 map 파일에서 추적하기 쉽다.

## 완료 기준

- 숫자 상수를 구현 파일에 직접 쓰지 않는다.
- 기능 스위치 매크로로 코드 포함 여부를 제어할 수 있다.
- 설정 파일만 바꿔도 테이블 크기와 동작 정책이 바뀐다.

## Q&A

1. Q: Generated Config 계층은 실제 코드 생성기가 없어도 필요한가?
   A: 필요하다. 코드 생성기가 없더라도 설정값을 한곳에 모으는 구조를 먼저 만들어야 이후 자동 생성 코드로 바꾸기 쉽다. 학습용으로는 수동 작성한 `can_cfg.h`, `can_par.c`가 생성 코드 역할을 한다.

2. Q: 설정값을 `can.c` 안에 직접 쓰면 왜 안 되는가?
   A: Driver logic과 ECU variant data가 섞이면 재사용이 어려워진다. CAN channel 수, Tx/Rx handle, ID, bitrate, payload size는 설정 테이블로 빼야 HS-CAN과 CAN-FD variant를 분리할 수 있다.

3. Q: `MW_TARGET_SPC584C70` 같은 target macro는 어디에 쓰이는가?
   A: startup, compiler option, HAL register mapping, interrupt vector, clock 설정의 조건부 포함에 쓰인다. 단순 표시용이 아니라 잘못된 타깃 설정을 빌드 단계에서 막는 guard로 사용한다.

4. Q: SPC58EC70 자료를 참고해도 되는가?
   A: CAN-FD/M_CAN family-level 설명은 참고할 수 있지만 설정 파일은 `SPC584C70` 전용으로 작성한다. 특히 core 수, linker memory map, interrupt target, RAM 배치는 혼용하면 안 된다.

5. Q: `dlc`와 `length`를 모두 설정에 넣어야 하는가?
   A: CAN-FD까지 고려하면 둘을 분리하는 편이 안전하다. Classical CAN에서는 `dlc == length`가 많지만 CAN-FD에서는 DLC 9가 12바이트를 의미하는 등 매핑이 달라진다.

6. Q: Message RAM offset은 왜 설정으로 관리해야 하는가?
   A: M_CAN의 Rx FIFO, Tx buffer, filter 영역은 shared Message RAM 안에서 배치된다. offset이 겹치면 송수신 데이터가 깨지므로 설정 테이블과 계산표로 추적해야 한다.

7. Q: 기능 스위치 매크로는 많을수록 좋은가?
   A: 아니다. 너무 많은 스위치는 조합 폭발과 미검증 variant를 만든다. HS-CAN/CAN-FD, ISO-TP 사용 여부, UDS 서비스 사용 여부처럼 실제 산출물에 의미 있는 축만 둔다.

8. Q: 설정 오류는 보통 어떤 증상으로 나타나는가?
   A: 빌드는 되지만 frame이 bus에 나오지 않거나, 수신 interrupt가 없거나, callback handle이 틀리는 형태가 많다. 설정값과 register dump, analyzer trace를 함께 비교해야 한다.

9. Q: 설정 파일을 수정한 뒤 반드시 확인할 것은 무엇인가?
   A: variant name, M_CAN instance, bitrate 계산값, filter 수, Tx/Rx object 수, payload size, interrupt line, transceiver pin을 확인한다. 변경 전후 diff와 bring-up 로그를 함께 남긴다.

10. Q: CAN-FD variant는 기존 HS-CAN 설정을 복사해서 만들면 되는가?
    A: 구조는 복사할 수 있지만 payload size, data bitrate, FDF/BRS, Message RAM 배치, ISO-TP capacity는 다시 계산해야 한다. 특히 64바이트 object로 바꾸면 Message RAM 사용량이 크게 늘어난다.

11. Q: 설정 계층 완료 후 다음 단계에 넘겨야 할 핵심 산출물은 무엇인가?
    A: Tx/Rx handle table, controller config, bitrate config, feature switch, target macro, payload policy이다. 3단계 데이터 모델과 4단계 Driver는 이 계약을 기준으로 작성된다.
