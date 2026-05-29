# 3단계: CAN 메시지 데이터 모델 학습

## 학습 목표

CAN Driver가 실제 메시지를 송수신하려면 메시지 ID, DLC, 데이터 버퍼, callback 정보를 알아야 한다. 이 단계에서는 DBC 기반 생성 코드가 어떤 정적 데이터 모델을 만드는지 학습한다.

## 참조 파일

- `src/CBD/Gen/can_par.h`
- `src/CBD/Gen/can_par.c`
- `src/CBD/Gen/drv_par.h`
- `src/CBD/Gen/drv_par.c`

## 이론 설명

CAN 미들웨어는 메시지를 이름으로 직접 처리하지 않고 정수 핸들로 처리한다.

예를 들면 다음과 같다.

```c
#define CanTxIPSU_GST      0
#define CanTxIPSU_01_200ms 15
#define CanRxCGW_01_200ms  4
```

핸들은 배열 인덱스로 사용된다. 즉 `CanTxIPSU_01_200ms`는 Tx 설정 테이블에서 특정 행을 가리킨다.

CAN 메시지 데이터 모델의 핵심은 다음 4개이다.

- 핸들: 메시지를 식별하는 내부 번호
- ID: CAN arbitration ID
- DLC: payload 길이
- data pointer: 실제 송수신 데이터 버퍼

DBC 신호는 보통 바이트 배열 또는 bit-field 구조체로 표현된다. 학습용 구현에서는 먼저 바이트 배열로 시작하고, 나중에 신호 accessor 함수를 추가하는 방식이 좋다.

### 메시지 핸들이 필요한 이유

애플리케이션 코드가 CAN ID `0x510`을 직접 넘겨 송신할 수도 있다. 하지만 미들웨어에서는 보통 `CanTxIPSU_01_200ms` 같은 핸들을 쓴다. 핸들은 메시지의 내부 이름이고, CAN ID는 네트워크에서 사용하는 외부 식별자이다. 이 둘을 분리하면 다음 장점이 있다.

- CAN ID가 변경되어도 애플리케이션 API는 유지된다.
- 메시지별 DLC, buffer, callback을 배열 인덱스로 빠르게 찾을 수 있다.
- generated code가 모든 메시지를 일관된 방식으로 다룰 수 있다.
- 동일한 메시지를 여러 identity/variant에 매핑할 수 있다.

즉 핸들은 소프트웨어 내부의 안정적인 참조이고, CAN ID는 네트워크 설정의 일부이다.

### 정적 테이블 중심 설계

CBD의 `can_par.c`는 대표적인 정적 테이블 설계이다. Tx ID 배열, DLC 배열, data pointer 배열, callback 배열이 나뉘어 있고, 같은 index가 같은 메시지를 의미한다. 이런 방식은 메모리 사용량을 줄이고 생성기가 만들기 쉽다. 학습용으로는 구조체 배열 하나에 묶는 편이 이해하기 쉽다.

구조체 배열 방식:

```c
{ id, dlc, data, callback }
```

분리 배열 방식:

```c
CanTxId[handle]
CanTxDlc[handle]
CanTxDataPtr[handle]
CanTxConfirmation[handle]
```

처음 구현할 때는 구조체 배열로 시작하고, 생성기 스타일을 따라가고 싶을 때 분리 배열로 최적화해도 된다.

### 신호 layout과 endian

CAN payload 안의 신호는 byte 단위 또는 bit 단위로 배치된다. 예를 들어 32비트 위치값이 byte 0~3에 little-endian으로 들어갈 수도 있고, big-endian bit numbering으로 들어갈 수도 있다. 타깃이 `SPC584C70`처럼 Power Architecture 계열이고 기존 설정이 big-endian 계열인 경우에도, CAN 신호 layout은 CPU endian과 별개로 DBC가 정의한 byte order를 따라야 한다.

따라서 안전한 방식은 다음과 같다.

- payload는 항상 `vuint8 data[]`로 보관한다.
- 신호 accessor에서 shift/mask로 값을 조립한다.
- 구조체 bit-field는 컴파일러 bit ordering 영향을 받을 수 있으므로 학습용 이해에는 좋지만 이식성 높은 구현에는 주의한다.

### CAN-FD를 고려한 데이터 모델

처음부터 `data[8]`로 구조를 고정하면 10단계 CAN-FD 전환에서 많이 고쳐야 한다. 학습용 Classical CAN 단계에서도 다음처럼 pointer+length 구조를 생각해두면 좋다.

```c
typedef struct
{
    vuint32 id;
    vuint8 is_fd;
    vuint8 dlc;
    vuint8 length;
    vuint8 *data;
} CanObjectConfig;
```

Classical CAN 메시지는 `is_fd = 0`, `length <= 8`로 사용하고, CAN-FD 메시지는 `is_fd = 1`, `length <= 64`로 확장한다.

## CBD 코드에서 관찰할 포인트

`can_par.h`에서 다음을 확인한다.

- `CanTx...` 송신 핸들
- `CanRx...` 수신 핸들
- Tx/Rx 하드웨어 오브젝트 매핑

`can_par.c`에서 다음 테이블을 확인한다.

- `CanTxId0`
- `CanTxDLC`
- `CanTxDataPtr`
- `CanRxId0`
- `CanRxDataLen`
- `CanRxDataPtr`

`drv_par.h`에서 다음을 확인한다.

- `_c_IPSU_01_200ms_msgType`
- `_c_CGW_01_200ms_msgType`
- 메시지별 union buffer

## 예제 코드

### can_par.h

```c
#ifndef CAN_PAR_H
#define CAN_PAR_H

#include "platform_types.h"

typedef vuint8 CanTxHandle;
typedef vuint8 CanRxHandle;

#define CanTxIPSU_GST       ((CanTxHandle)0u)
#define CanTxIPSU_01_200ms  ((CanTxHandle)1u)
#define CanTxCount          ((CanTxHandle)2u)

#define CanRxCGW_01_200ms   ((CanRxHandle)0u)
#define CanRxGST_IPSU       ((CanRxHandle)1u)
#define CanRxCount          ((CanRxHandle)2u)

typedef struct
{
    vuint32 id;
    vuint8 is_extended;
    vuint8 is_fd;
    vuint8 brs;
    vuint8 dlc;
    vuint8 length;
    vuint8 *data;
} CanTxObjectConfig;

typedef struct
{
    vuint32 id;
    vuint8 is_extended;
    vuint8 is_fd;
    vuint8 brs;
    vuint8 dlc;
    vuint8 length;
    vuint8 *data;
} CanRxObjectConfig;

extern const CanTxObjectConfig CanTxConfig[CanTxCount];
extern const CanRxObjectConfig CanRxConfig[CanRxCount];

#endif
```

### can_par.c

```c
#include "can_par.h"

vuint8 IPSU_GST[8];
vuint8 IPSU_01_200ms[8];
vuint8 CGW_01_200ms[8];
vuint8 GST_IPSU[8];

const CanTxObjectConfig CanTxConfig[CanTxCount] =
{
    { 0x700u, 0u, 0u, 0u, 8u, 8u, IPSU_GST },
    { 0x510u, 0u, 0u, 0u, 8u, 8u, IPSU_01_200ms }
};

const CanRxObjectConfig CanRxConfig[CanRxCount] =
{
    { 0x410u, 0u, 0u, 0u, 8u, 8u, CGW_01_200ms },
    { 0x7E0u, 0u, 0u, 0u, 8u, 8u, GST_IPSU }
};
```

### 핸들로 설정 조회

```c
const CanTxObjectConfig *Can_GetTxConfig(CanTxHandle handle)
{
    if (handle >= CanTxCount)
    {
        return 0;
    }

    return &CanTxConfig[handle];
}
```


## 단계 산출물

이 단계의 결과물은 CAN 계층 전체가 공유할 frame/message data model이다.

- `middleware/can/can_types.h`: CAN ID, DLC, channel, handle, 상태 enum 정의
- `middleware/can/can_frame.h`: Classical CAN과 CAN-FD를 모두 표현할 수 있는 `CanFrame` 구조체
- `middleware/can/can_dlc.h`, `middleware/can/can_dlc.c`: DLC와 실제 payload length 변환 함수
- `middleware/can/can_par.h`, `middleware/can/can_par.c`: message handle과 ID/filter/length 정책 매핑
- `bringup/can/can_model_bringup.c`: frame 생성 결과와 DLC/length 변환 결과를 디버거로 확인하는 bring-up 코드

HS-CAN 산출물에서는 `length <= 8`인 frame만 허용한다. CAN-FD 산출물에서는 `length <= 64`, DLC 9~15의 비선형 payload 길이, BRS 여부를 구조체에서 표현할 수 있어야 한다.

## 구현 가이드

1. `can_types.h`에 `CanChannel`, `CanTxHandle`, `CanRxHandle`, `CanIdType`, `CanFrameFormat`, `CanReturnType`을 정의한다. handle은 배열 index로 사용할 수 있도록 unsigned 정수로 고정한다.
2. `can_frame.h`의 `CanFrame`은 `id`, `is_extended`, `is_fd`, `brs`, `esi`, `dlc`, `length`, `data[64]`를 포함한다. Classical CAN도 같은 구조체를 쓰되 `is_fd=0`, `length<=8`로 제한한다.
3. `can_dlc.c`에 `CanFd_DlcToLength()`와 `CanFd_LengthToDlc()`를 구현한다. 0~8은 동일 매핑, 9~15는 12/16/20/24/32/48/64로 변환한다.
4. `Can_ValidateFrame()`을 작성해 ID 범위, format별 최대 length, DLC/length 일치 여부, BRS 사용 조건을 검사한다. 이 함수는 Driver 송신 직전에도 재사용한다.
5. 디버거 watch 또는 CAN analyzer 송신 준비 buffer로 8, 12, 16, 64바이트 frame을 확인한다. 특히 `DLC=9`가 `length=12`로 처리되는지 반드시 확인한다.

## 적용 고려사항과 트러블슈팅

데이터 모델은 한 번 잘못 잡으면 Driver, IL, ISO-TP, UDS까지 모두 영향을 받는다. 따라서 “DLC는 버스에 실리는 코드값이고 length는 실제 바이트 수”라는 규칙을 코드와 문서에서 분명히 유지해야 한다.

| 문제 케이스 | 원인 | 확인/대응 |
|---|---|---|
| CAN-FD 12바이트 메시지가 9바이트로 처리됨 | DLC 9를 length 9로 오해 | `CanFd_DlcToLength(9) == 12` 테스트를 필수로 둔다. |
| 표준 ID와 확장 ID filter가 섞임 | ID 타입 flag 누락 | `id_type` 또는 `is_extended` 필드를 명시하고 범위 검사를 분리한다. |
| IL에서 signal offset이 맞지 않음 | bit numbering/endian 정책 미정 | Motorola/Intel bit order 규칙을 data model 문서에 고정한다. |
| payload overflow가 Driver에서 뒤늦게 발견됨 | 상위 계층 length 검증 누락 | frame 생성 함수에서 format별 최대 payload를 먼저 검증한다. |

트러블슈팅 시에는 raw CAN frame dump와 내부 `CanFrame` 값을 나란히 기록한다. ID, IDE, FDF, BRS, DLC, length, data byte를 같은 순서로 출력하면 계층 간 해석 차이를 빨리 찾을 수 있다.

## 완료 기준

- CAN 메시지를 이름이 아니라 핸들로 접근할 수 있다.
- 핸들에서 ID, DLC, data pointer를 얻을 수 있다.
- CAN Driver가 이 테이블만 보고 송신할 수 있다.
