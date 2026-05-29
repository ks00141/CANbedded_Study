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

`SPC584C70` 실습에서는 메모리 클래스 매크로를 단순히 비워 두지 않는다. 처음에는 빌드가 가능한 최소 정의로 시작하되, 곧바로 linker script와 compiler pragma/attribute를 연결해 상수 테이블은 flash/ROM 영역으로, runtime buffer는 RAM 영역으로 배치되는지 확인한다.

### 왜 공통 타입 계층이 먼저 필요한가

차량용 미들웨어는 수명이 길고, 컴파일러와 MCU가 바뀌어도 동일한 상위 로직을 유지해야 한다. 이때 C 표준 타입을 직접 사용하는 코드와, 플랫폼 추상 타입을 사용하는 코드는 유지보수성이 크게 다르다. 예를 들어 `unsigned long`의 크기는 컴파일러/ABI에 따라 달라질 수 있지만, CAN payload의 32비트 신호는 항상 정확히 32비트여야 한다. 그래서 미들웨어는 `vuint32`처럼 의미가 고정된 타입을 사용한다.

또한 임베디드 C에서는 데이터가 어디에 배치되는지가 중요하다. calibration 상수, CAN ID 테이블, 진단 서비스 테이블은 flash/ROM에 두는 것이 일반적이고, runtime 상태, Rx/Tx buffer, 타이머 카운터는 RAM에 둔다. Vector 코드의 `V_MEMROM0`, `V_MEMROM1`, `V_MEMRAM0` 같은 매크로는 이런 배치 정책을 소스 코드에 직접 흩뿌리지 않고, 컴파일러별 memory section 문법으로 치환하기 위한 장치이다.

### SPC584C70에서의 의미

타깃 MCU를 `SPC584C70`으로 잡으면 공통 계층은 더 중요해진다. `SPC584C70`은 single computing e200z420n3 기반 Power Architecture 계열 차량용 MCU이며 VLE 명령 집합을 지원하므로 다음을 고려해야 한다.

- 정렬: 16비트/32비트 접근은 정렬 조건을 만족시키는 편이 안전하다. CAN payload byte array에서 16/32비트 값을 바로 캐스팅해 읽기보다 byte 조립 함수를 사용하는 것이 이식성이 좋다.
- endian: SPC58/SPC5 계열 Power Architecture 환경은 STM32처럼 little-endian ARM으로 가정하면 안 된다. 신호 packing/unpacking, 진단 payload, flash 저장 구조체는 CPU endian에 의존하지 않도록 명시적인 shift/mask 방식으로 작성한다.
- memory section: flash 상수 테이블과 RAM buffer를 구분해야 한다. MCU 포팅 단계에서는 linker script와 section pragma로 연결하고 map 파일에서 배치를 확인한다.
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

/* 초기 bring-up에서는 기본 정의로 시작하고, 이후 linker section 매크로로 확장한다. */
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


## 단계 산출물

이 단계를 끝내면 이후 모든 계층이 공통으로 사용하는 최소 기반 라이브러리가 생성되어야 한다.

- `middleware/common/platform_types.h`: `vuint8`, `vuint16`, `vuint32`, `vboolean` 등 고정 폭 타입 정의
- `middleware/common/compiler.h`: `V_MEMROM`, `V_MEMRAM`, `V_MEMROM0`, `V_MEMROM1` 등 메모리/컴파일러 추상화 매크로
- `middleware/common/vstdlib.h`, `middleware/common/vstdlib.c`: 메모리 초기화, 복사, 비교, 간단한 보호 구간 진입/해제 wrapper
- `bringup/common/common_bringup.c`: 디버거에서 타입 크기, 메모리 배치, 복사/초기화 결과를 확인하는 bring-up 코드

HS-CAN 미들웨어와 CAN-FD 미들웨어는 같은 공통 계층을 공유해야 한다. CAN-FD에서는 payload가 64바이트까지 커지므로, 이 단계의 메모리 복사/초기화 함수가 8바이트 전용 가정 없이 임의 길이를 처리하는지 반드시 검증한다.

## 구현 가이드

1. `platform_types.h`를 만들고 `vuint8`, `vuint16`, `vuint32`, `vboolean`, `TxDataPtr`, `RxDataPtr`를 정의한다. Green Hills 또는 현재 사용하는 SPC5 컴파일러에서 각 타입의 크기를 디버거 watch 또는 compile-time assert로 확인한다.
2. `compiler.h` 또는 `memclass.h`에 `V_MEMROM*`, `V_MEMRAM*` 매크로를 만든다. 첫 빌드에서는 비워 둘 수 있지만, 즉시 `const` 테이블과 RAM buffer 예제를 하나씩 만들어 linker map에서 `.rodata`와 RAM section 배치를 확인한다.
3. `vstdlib.h/c`에 `VStdMemSetInternal`, `VStdMemClrInternal`, `VStdRamMemCpy`, `VStdRomMemCpy`를 구현한다. CAN-FD 64바이트 payload 복사를 고려해 길이 인자는 최소 64바이트 이상을 안전하게 처리할 수 있어야 한다.
4. `VStdGlobalIntDisable`/`VStdGlobalIntRestore`는 실제 interrupt mask 저장/복구 wrapper로 설계한다. 초기에는 보드 support 함수나 compiler intrinsic을 감싸고, ISR 공유 buffer 보호에 사용할 수 있게 반환 타입을 고정한다.
5. `common_bringup.c`에서 flash 상수 테이블, RAM buffer, 8/64바이트 복사 예제를 만들고, 디버거로 주소 영역과 값 변화를 확인한다. 이 결과가 다음 단계의 설정 테이블과 CAN buffer의 기반이 된다.

## 적용 고려사항과 트러블슈팅

SPC584C70은 Power Architecture 계열 MCU이며 컴파일러, 링커 스크립트, 메모리 섹션 정책의 영향을 크게 받는다. 공통 헤더를 작성할 때는 실제 MCU 빌드용 정의를 기준으로 API 이름을 유지해야 이후 계층을 쉽게 이식할 수 있다.

| 문제 케이스 | 원인 | 확인/대응 |
|---|---|---|
| 타입 크기가 의도와 다름 | 컴파일러 기본 타입 폭을 암묵적으로 사용 | `stdint.h` 기반으로 타입을 고정하고 `sizeof` 정적 검사를 추가한다. |
| `V_MEMROM`이 붙은 테이블이 RAM에 배치됨 | 링커 섹션 매핑 누락 | `compiler.h` 매크로와 링커 스크립트의 section 이름을 함께 검토한다. |
| 인터럽트와 main loop가 공유하는 flag가 갱신되지 않음 | `volatile` 누락 또는 최적화 영향 | ISR 공유 변수에는 volatile qualifier를 사용하고 접근 단위를 8/16/32비트로 제한한다. |
| CAN-FD 64바이트 payload 복사 중 buffer overflow 발생 | 기존 HS-CAN 8바이트 크기를 그대로 사용 | 공통 함수는 length 인자를 신뢰하지 말고 호출 계층에서 destination capacity를 함께 검증한다. |

트러블슈팅 순서는 `타입 크기 확인 -> 메모리 섹션 map 파일 확인 -> volatile 대상 확인 -> 디버거 watch 확인 -> MCU 최소 예제 재빌드` 순서가 좋다.

## 완료 기준

- 공통 타입 헤더만 include해도 상위 모듈에서 `vuint8`, `vuint16`, `TxDataPtr`를 사용할 수 있다.
- 메모리 클래스 매크로가 실제 linker section 정책과 연결될 수 있다.
- 메모리 초기화/복사 함수가 SPC584C70 보드에서 디버거로 확인 가능한 결과를 만든다.
