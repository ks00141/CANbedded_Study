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
    vuint16 id;
    vuint8 dlc;
    vuint8 *data;
} CanTxObjectConfig;

typedef struct
{
    vuint16 id;
    vuint8 dlc;
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
    { 0x700u, 8u, IPSU_GST },
    { 0x510u, 8u, IPSU_01_200ms }
};

const CanRxObjectConfig CanRxConfig[CanRxCount] =
{
    { 0x410u, 8u, CGW_01_200ms },
    { 0x7E0u, 8u, GST_IPSU }
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

## 연습문제

1. Tx 메시지 3개, Rx 메시지 3개를 갖는 설정 테이블을 작성하라.
2. CAN ID를 입력받아 대응되는 `CanRxHandle`을 찾는 함수를 작성하라.
3. DLC가 8보다 큰 설정값이 들어오면 컴파일 또는 초기화 단계에서 오류를 반환하도록 구현하라.
4. `IPSU_01_200ms` 버퍼의 0~3바이트를 32비트 little-endian 값으로 쓰는 함수를 작성하라.

## 완료 기준

- CAN 메시지를 이름이 아니라 핸들로 접근할 수 있다.
- 핸들에서 ID, DLC, data pointer를 얻을 수 있다.
- CAN Driver가 이 테이블만 보고 송신할 수 있다.
