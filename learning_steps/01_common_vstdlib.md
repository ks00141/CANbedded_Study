# 1단계: Common / VStdLib 학습

## 학습 목표

CBD 미들웨어의 가장 아래에는 공통 타입과 유틸리티 함수가 있다. 이 단계에서는 Vector 코드가 왜 `uint8_t` 대신 `vuint8` 같은 타입을 쓰는지, 왜 `V_MEMROM`, `V_MEMRAM` 같은 빈 매크로가 많은지 이해한다.

## 참조 파일

- `src/CBD/Common/v_def.h`
- `src/CBD/VStdLib/vstdlib.h`
- `src/CBD/VStdLib/vstdlib.c`

## 이론 설명

임베디드 미들웨어는 여러 MCU, 컴파일러, 메모리 모델에서 재사용되어야 한다. 그래서 코드가 표준 C 타입과 표준 함수만 직접 쓰지 않고, 한 번 감싼 타입과 매크로를 사용한다.

대표 개념은 다음과 같다.

- 고정폭 타입: `vuint8`, `vuint16`, `vuint32`
- signed 타입: `vsint8`, `vsint16`, `vsint32`
- 메모리 클래스: ROM/RAM, near/far, huge 영역을 표현하는 매크로
- 포인터 클래스: 특정 메모리 영역의 포인터를 표현하는 매크로
- 공통 함수: 메모리 초기화, 메모리 복사, 인터럽트 보호 구간 진입/해제

PC에서 학습용으로 재구현할 때는 메모리 클래스 매크로를 비워 두어도 된다. 중요한 것은 상위 코드가 “어떤 메모리 영역인지”를 추상적으로 표현한다는 점이다.

## CBD 코드에서 관찰할 포인트

`v_def.h`에서 다음을 확인한다.

- `typedef unsigned char vuint8;`
- `typedef unsigned short vuint16;`
- `typedef unsigned long vuint32;`
- `TxDataPtr`, `RxDataPtr`
- `V_MEMROM*`, `V_MEMRAM*` 계열 매크로

`vstdlib.c`에서 다음 함수를 확인한다.

- `VStdMemSetInternal`
- `VStdMemClrInternal`
- `VStdRamMemCpy`
- `VStdRomMemCpy`
- `VStdGlobalIntDisable`
- `VStdGlobalIntRestore`

## 예제 코드

### platform_types.h

```c
#ifndef PLATFORM_TYPES_H
#define PLATFORM_TYPES_H

#include <stdint.h>

typedef uint8_t  vuint8;
typedef uint16_t vuint16;
typedef uint32_t vuint32;

typedef int8_t   vsint8;
typedef int16_t  vsint16;
typedef int32_t  vsint32;

typedef uint8_t  vboolean;

#define V_TRUE  ((vboolean)1u)
#define V_FALSE ((vboolean)0u)

typedef vuint8 *TxDataPtr;
typedef vuint8 *RxDataPtr;

#endif
```

### memclass.h

```c
#ifndef MEMCLASS_H
#define MEMCLASS_H

/* PC 학습 환경에서는 비워 둔다. */
#define V_MEMROM0
#define V_MEMROM1
#define V_MEMROM2
#define V_MEMROM3

#define V_MEMRAM0
#define V_MEMRAM1
#define V_MEMRAM2
#define V_MEMRAM3

#endif
```

### vstdlib.c

```c
#include "platform_types.h"
#include <string.h>

void VStdMemSetInternal(void *dest, vuint8 pattern, vuint16 count)
{
    (void)memset(dest, pattern, count);
}

void VStdMemClrInternal(void *dest, vuint16 count)
{
    (void)memset(dest, 0, count);
}

void VStdRamMemCpy(void *dest, const void *src, vuint16 count)
{
    (void)memcpy(dest, src, count);
}

typedef vuint32 tVStdIrqStateType;

tVStdIrqStateType VStdGlobalIntDisable(void)
{
    return 0u;
}

void VStdGlobalIntRestore(tVStdIrqStateType old_status)
{
    (void)old_status;
}
```

## 연습문제

1. `vuint8`, `vuint16`, `vuint32`를 사용해 8바이트 CAN payload 구조체를 정의하라.
2. `VStdMemClrInternal()`을 사용해 8바이트 버퍼를 0으로 초기화하는 테스트 코드를 작성하라.
3. `V_MEMROM1 const vuint8 V_MEMROM2 table[4]` 형태의 선언이 PC 환경에서 컴파일되도록 매크로를 정의하라.
4. `VStdRamMemCpy()`에 `NULL` 포인터가 들어오면 어떻게 처리할지 정책을 정하고 구현하라.

## 완료 기준

- 공통 타입 헤더만 include해도 상위 모듈에서 `vuint8`, `vuint16`, `TxDataPtr`를 사용할 수 있다.
- 메모리 클래스 매크로가 있어도 PC 컴파일이 깨지지 않는다.
- 메모리 초기화/복사 함수가 간단한 테스트에서 정상 동작한다.
