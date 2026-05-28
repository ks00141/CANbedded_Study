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

### 왜 공통 타입 계층이 먼저 필요한가

차량용 미들웨어는 수명이 길고, 컴파일러와 MCU가 바뀌어도 동일한 상위 로직을 유지해야 한다. 이때 C 표준 타입을 직접 사용하는 코드와, 플랫폼 추상 타입을 사용하는 코드는 유지보수성이 크게 다르다. 예를 들어 `unsigned long`의 크기는 컴파일러/ABI에 따라 달라질 수 있지만, CAN payload의 32비트 신호는 항상 정확히 32비트여야 한다. 그래서 미들웨어는 `vuint32`처럼 의미가 고정된 타입을 사용한다.

또한 임베디드 C에서는 데이터가 어디에 배치되는지가 중요하다. calibration 상수, CAN ID 테이블, 진단 서비스 테이블은 flash/ROM에 두는 것이 일반적이고, runtime 상태, Rx/Tx buffer, 타이머 카운터는 RAM에 둔다. Vector 코드의 `V_MEMROM0`, `V_MEMROM1`, `V_MEMRAM0` 같은 매크로는 이런 배치 정책을 소스 코드에 직접 흩뿌리지 않고, 컴파일러별 memory section 문법으로 치환하기 위한 장치이다.

### SPC58xC에서의 의미

타깃 MCU를 `SPC58xC`로 잡으면 공통 계층은 더 중요해진다. `SPC58xC`는 차량용 Power Architecture 계열 MCU이므로 다음을 고려해야 한다.

- 정렬: 16비트/32비트 접근은 정렬 조건을 만족시키는 편이 안전하다. CAN payload byte array에서 16/32비트 값을 바로 캐스팅해 읽기보다 byte 조립 함수를 사용하는 것이 이식성이 좋다.
- endian: 기존 CBD 설정에는 big-endian 계열 설정이 보인다. 신호 packing/unpacking은 CPU endian에 의존하지 않도록 명시적인 shift/mask 방식으로 작성한다.
- memory section: flash 상수 테이블과 RAM buffer를 구분해야 한다. 학습용 PC 구현에서는 매크로를 비워도 되지만, MCU 포팅 단계에서는 linker script와 section pragma로 연결한다.
- interrupt 보호: CAN ISR과 주기 task가 같은 buffer를 만질 수 있으므로, 공통 계층에서 interrupt lock/unlock 추상화를 제공한다.

### 공통 계층을 설계할 때의 기준

공통 계층은 가능한 작고 보수적으로 유지한다. 여기에는 하드웨어 기능을 많이 넣지 않는다. 타입, boolean, null, memory class, byte copy, interrupt lock 정도만 둔다. 그래야 CAN Driver, IL, TP, UDS가 모두 같은 기반을 공유하면서도 서로 강하게 묶이지 않는다.

학습용 구현에서는 `VStdRamMemCpy()`가 단순히 `memcpy()`를 감싸도 된다. 하지만 실제 ECU 구현에서는 다음 정책을 정해야 한다.

- `NULL` 포인터를 assert로 잡을지, error hook을 호출할지
- copy 길이가 0일 때 허용할지
- ISR 안에서 호출 가능한 함수와 task에서만 호출 가능한 함수를 구분할지
- 큰 buffer copy 중 interrupt disable 시간을 최소화할지

이 단계의 핵심은 “상위 모듈이 플랫폼 차이를 직접 알지 않게 만든다”는 감각을 잡는 것이다.

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
