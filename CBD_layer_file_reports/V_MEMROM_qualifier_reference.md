# V_MEMROM0/1/2/3 Qualifier Reference

## 목적

이 문서는 `V_MEMROM0`, `V_MEMROM1`, `V_MEMROM2`, `V_MEMROM3`가 왜 `const`와 함께 쓰이는지, 각 매크로 자리가 어떤 종류의 compiler qualifier를 수용하기 위한 것인지 정리한다.

대상 코드는 Vector CANbedded/VStdLib 계열의 레거시 임베디드 C 코드이며, 현재 프로젝트 설정에서는 다음처럼 전개된다.

```c
#define V_MEMROM0
#define V_MEMROM1
#define V_MEMROM2 const
#define V_MEMROM3
```

따라서 현재 타깃에서 아래 선언은 사실상 `extern vuint8 const kVStdMainVersion;`와 같은 의미가 된다.

```c
V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 kVStdMainVersion;
```

하지만 이 매크로들은 현재 타깃만을 위한 장식이 아니다. 여러 MCU, 컴파일러, 메모리 모델에서 ROM/Flash 상수와 ROM 포인터를 같은 소스 코드로 선언하기 위한 insertion slot이다.

## 표준인가, 관습인가

`const`는 ISO C 표준의 타입 한정자다. 그러나 `const`는 C 타입 시스템에서 "이 lvalue를 통해 값을 수정하지 않는다"는 의미이지, 객체를 반드시 Flash/ROM에 배치한다는 표준 보장은 아니다. 실제 `.rodata` 또는 Flash 배치는 컴파일러, 링커 스크립트, startup/linker 설정의 영역이다.

반면 `V_MEMROM0`, `V_MEMROM1`, `V_MEMROM2`, `V_MEMROM3`라는 이름과 번호 방식은 ISO C 표준도 아니고 AUTOSAR 표준 매크로명도 아니다. Vector 계열 코드에서 쓰는 벤더/스택 관습으로 보는 것이 정확하다.

다만 이 방식의 의도는 AUTOSAR Compiler Abstraction과 닮아 있다. AUTOSAR는 `CONST(type, memclass)`, `P2CONST(ptrtype, memclass, ptrclass)` 같은 표준화된 추상 매크로를 제공하며, `memclass`는 객체 또는 포인터 자체의 메모리 분류, `ptrclass`는 포인터 거리/주소 공간 분류를 표현한다. AUTOSAR 문서도 Metrowerks, Cosmic, Tasking 같은 컴파일러별로 `const`, `memclass`, `ptrclass`의 위치가 달라지는 예를 제시한다.

즉 결론은 다음과 같다.

- `const`: 표준 C.
- `near`, `far`, `huge`, `__far`, `__rom`, `@far`, `__attribute__((section))`, `#pragma location` 등: 컴파일러/타깃 확장.
- `V_MEMROM0/1/2/3`: Vector식 numbered placeholder 관습.
- AUTOSAR `CONST`, `P2CONST` 등: AUTOSAR가 표준화한 compiler abstraction 관습.

## 로컬 코드에서의 정의

`src/CBD/Common/v_def.h`의 기본 정의는 다음 역할을 갖는다.

| 매크로 | 현재 기본값 | 로컬 주석/의도 | 대표 위치 |
|---|---:|---|---|
| `V_MEMROM0` | 빈 값 | ROM data access용 추가 qualifier | `src/CBD/Common/v_def.h:765` |
| `V_MEMROM1` | 빈 값 | ROM fast data access qualifier | `src/CBD/Common/v_def.h:773` |
| `V_MEMROM2` | `const` | ROM fast data access qualifier, 현재 타깃에서 실제 read-only 성격 부여 | `src/CBD/Common/v_def.h:792` |
| `V_MEMROM3` | 빈 값 | 주로 포인터 선언에서 `*` 앞쪽에 들어가는 추가 qualifier 자리 | `src/CBD/Common/v_def.h:812` |

`MEMORY_ROM`과도 호환된다. `MEMORY_ROM`이 없으면 `V_MEMROM2`를 `const`로 두고 `MEMORY_ROM`을 `V_MEMROM2`로 alias한다. 반대로 어떤 모듈이 `MEMORY_ROM`을 이미 정의했다면 `V_MEMROM2`가 그 값을 따라간다. 이 구조는 구형 `MEMORY_ROM` 계열 모듈과 신형 `V_MEMROMx` 계열 모듈을 함께 빌드하기 위한 호환 계층이다.

CANdesc 쪽 `src/CBD/Gen/desc.h`도 fallback으로 `V_MEMROM0/1/3`을 빈 값, `V_MEMROM2`를 `MEMORY_ROM`으로 맞춘다.

## 각 자리의 의미

Vector 스타일 선언은 보통 다음 두 패턴으로 이해할 수 있다.

```c
/* ROM 상수 객체 */
V_MEMROM0 storage V_MEMROM1 type V_MEMROM2 object;

/* ROM 상수를 가리키는 포인터 */
V_MEMROM1 type V_MEMROM2 V_MEMROM3 * pointer;
```

### V_MEMROM0

선언 전체 앞쪽에 오는 추가 qualifier 자리다.

올 수 있는 후보는 다음과 같다.

- 빈 값
- compiler-specific prefix keyword
- section/placement 관련 prefix 확장
- linker garbage collection 방지용 keyword, 예: IAR 계열 `__root`
- 선언 앞쪽에 와야 하는 vendor storage annotation

이 자리는 `extern`, `static` 같은 C storage class와 함께 쓰이는 앞쪽 공간이다. 로컬 코드에서는 다음처럼 쓰인다.

```c
V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 kVStdMainVersion;
```

실제 정의에서는 다음처럼 `extern` 없이 객체를 정의한다.

```c
V_MEMROM0 V_MEMROM1 vuint8 V_MEMROM2 kVStdMainVersion = ...;
```

현재 프로젝트에서는 빈 값이라 영향이 없지만, 다른 컴파일러에서는 "이 상수를 특정 ROM 영역에 배치하라" 또는 "사용되지 않아도 제거하지 말라" 같은 prefix 성격의 확장을 넣을 수 있는 자리다.

### V_MEMROM1

타입 앞쪽에 들어가는 ROM 접근 qualifier 자리다.

올 수 있는 후보는 다음과 같다.

- 빈 값
- `__rom`
- `__far`
- `near`, `far`, `huge`
- `@far`, `@near` 같은 컴파일러별 메모리 모델 지정자

`V_MEMROM1`은 "이 타입/객체가 어떤 주소 공간 또는 접근 모델에 속하는가"를 표현하기 위한 자리다. 일부 8/16비트 MCU나 Harvard architecture 계열에서는 ROM과 RAM의 주소 공간이 다르거나, near/far 포인터의 크기와 접근 명령이 다를 수 있다.

현재 프로젝트에서는 빈 값이다. 하지만 `VStdRomMemCpy` 선언을 보면 이 자리가 ROM source pointer의 타입 앞쪽에도 들어간다.

```c
VStdRomMemCpy(void *pDest,
              V_MEMROM1 void V_MEMROM2 V_MEMROM3 *pSrc,
              vuint16 nCnt);
```

### V_MEMROM2

타입 뒤쪽, 객체 이름 또는 포인터 `*` 앞쪽에 들어가는 read-only/ROM qualifier 자리다.

현재 프로젝트에서 가장 중요한 자리이며 기본값은 `const`다.

올 수 있는 후보는 다음과 같다.

- `const`
- `MEMORY_ROM`
- `__attribute__((section(".rodata.xxx")))` 같은 GCC/Clang 확장
- `__declspec(allocate("..."))` 같은 MSVC 계열 확장
- 컴파일러별 ROM memory qualifier
- 빈 값

로컬 코드에서는 버전 상수 정의가 대표적이다.

```c
V_MEMROM0 V_MEMROM1 vuint8 V_MEMROM2 kVStdMainVersion =
  (vuint8)(((vuint16)VSTDLIB_MPC_VERSION) >> 8);
```

현재 전개 결과는 사실상 다음과 같다.

```c
vuint8 const kVStdMainVersion = 0x01u;
```

중요한 점은 `const`가 read-only 타입을 만들 뿐이고, 실제 Flash/ROM 배치는 링커가 `.rodata` 또는 특정 section을 어디에 배치하느냐에 의해 확정된다는 것이다.

### V_MEMROM3

주로 포인터 선언에서 `*` 바로 앞쪽에 들어가는 추가 qualifier 자리다.

올 수 있는 후보는 다음과 같다.

- 빈 값
- `__far`
- `far`
- `huge`
- address-space qualifier
- 특정 컴파일러가 포인터 declarator 근처에 요구하는 ROM pointer qualifier

이 자리는 일반 상수 객체 선언에서는 잘 드러나지 않고, ROM 포인터에서 의미가 커진다.

```c
V_MEMROM1 void V_MEMROM2 V_MEMROM3 *pSrc
```

현재 프로젝트에서는 `V_MEMROM3`이 빈 값이므로 단순히 `void const *pSrc`에 가깝다. 그러나 어떤 컴파일러에서는 ROM을 가리키는 포인터가 RAM 포인터와 크기, 주소 계산, 접근 명령이 달라서 `*` 근처에 별도 qualifier가 필요할 수 있다.

## 로컬 사용 예

### VStdLib 버전 상수

`src/CBD/VStdLib/vstdlib.h:264`:

```c
V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 kVStdMainVersion;
V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 kVStdSubVersion;
V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 kVStdReleaseVersion;
```

`src/CBD/VStdLib/vstdlib.c:252`:

```c
V_MEMROM0 V_MEMROM1 vuint8 V_MEMROM2 kVStdMainVersion =
  (vuint8)(((vuint16)VSTDLIB_MPC_VERSION) >> 8);
```

헤더는 extern 선언만 제공하고, 실제 storage는 `.c` 파일에서 한 번만 정의한다. `V_MEMROM2`가 현재 `const`이므로 버전 값은 read-only 상수 객체가 된다.

### ROM to RAM 복사 API

`src/CBD/VStdLib/vstdlib.h:394`:

```c
VSTD_API_0 void VSTD_API_1 VStdRomMemCpy(
  void *pDest,
  V_MEMROM1 void V_MEMROM2 V_MEMROM3 *pSrc,
  vuint16 nCnt) VSTD_API_2;
```

`pDest`는 RAM destination이고, `pSrc`는 ROM source다. 그래서 source pointer 쪽에 `V_MEMROM1/2/3`가 붙는다.

`src/CBD/VStdLib/vstdlib.c:433`에서는 byte 단위 복사를 위해 source pointer를 다시 ROM-qualified byte pointer로 캐스팅한다.

```c
((vuint8*)pDest)[nCnt] =
  ((V_MEMROM1 vuint8 V_MEMROM2 V_MEMROM3 *)pSrc)[nCnt];
```

### 공통 선언 매크로

`src/CBD/Common/v_def.h:963` 부근에는 이 slot들을 조합한 더 일반적인 선언 매크로가 있다.

```c
#define V_DEF_CONST(storage, type, memclass) \
  V_MEMROM0 storage V_MEMROM1 type V_MEMROM2

#define V_DEF_P2CONST(storage, ptrtype, memclass, ptrclass) \
  V_MEMRAM0 storage V_MEMROM1 ptrtype V_MEMROM2 V_MEMROM3 * V_MEMRAM1 V_MEMRAM2

#define V_DEF_CONSTP2CONST(storage, ptrtype, memclass, ptrclass) \
  V_MEMROM0 storage V_MEMROM1 ptrtype V_MEMROM2 V_MEMROM3 * V_MEMROM1 V_MEMROM2
```

이 매크로들은 AUTOSAR의 `CONST`, `P2CONST`, `CONSTP2CONST`와 비슷한 목적을 갖지만, 이름과 slot 구성은 Vector 자체 스타일이다.

## 예시 코드 1: 현재 프로젝트와 비슷한 단순 PowerPC/GHS 계열

이 예시는 `const`만 의미 있고 나머지는 빈 값인 경우다.

```c
#define V_MEMROM0
#define V_MEMROM1
#define V_MEMROM2 const
#define V_MEMROM3

V_MEMROM0 extern V_MEMROM1 unsigned char V_MEMROM2 AppMainVersion;

V_MEMROM0 V_MEMROM1 unsigned char V_MEMROM2 AppMainVersion = 0x01u;

void CopyFromRom(unsigned char *dst,
                 V_MEMROM1 unsigned char V_MEMROM2 V_MEMROM3 *src,
                 unsigned short len)
{
  while (len > 0u) {
    --len;
    dst[len] = src[len];
  }
}
```

전개하면 핵심은 다음과 같다.

```c
extern unsigned char const AppMainVersion;
unsigned char const AppMainVersion = 0x01u;
void CopyFromRom(unsigned char *dst, unsigned char const *src, unsigned short len);
```

## 예시 코드 2: GCC/Clang section attribute를 쓰는 Flash calibration 영역

이 예시는 GCC/Clang 계열 확장 예시다. `section` attribute만으로 Flash 배치가 끝나는 것은 아니고, 링커 스크립트에서 `.calib_rodata`를 Flash 영역에 배치해야 한다.

```c
#define V_MEMROM0
#define V_MEMROM1
#define V_MEMROM2 const __attribute__((section(".calib_rodata")))
#define V_MEMROM3

V_MEMROM0 V_MEMROM1 unsigned short V_MEMROM2 CalibTable[] = {
  10u, 20u, 30u
};

V_MEMROM1 unsigned short V_MEMROM2 V_MEMROM3 *CalibTablePtr = CalibTable;
```

이 스타일은 `V_MEMROM2` 자리에 `const`와 section attribute를 함께 넣은 예시다. 실제 프로젝트에서는 이런 확장을 직접 `V_MEMROM2`에 넣기보다 compiler abstraction 또는 MemMap 계층에서 관리하는 경우가 많다.

## 예시 코드 3: ROM 주소 공간과 far pointer가 분리된 MCU 예시

이 예시는 특정 컴파일러 문법을 설명하기 위한 가상/예시적 매핑이다. 실제 키워드 위치는 컴파일러 매뉴얼에 맞춰야 한다.

```c
#define V_MEMROM0
#define V_MEMROM1 __rom
#define V_MEMROM2 const
#define V_MEMROM3 __far

V_MEMROM0 V_MEMROM1 unsigned char V_MEMROM2 DiagServiceIds[] = {
  0x10u, 0x11u, 0x19u
};

V_MEMROM1 unsigned char V_MEMROM2 V_MEMROM3 *DiagServiceIdPtr =
  DiagServiceIds;
```

의도는 다음과 같다.

- `V_MEMROM1 __rom`: 대상 객체 또는 pointee가 ROM address space에 있음을 표시.
- `V_MEMROM2 const`: 읽기 전용 데이터임을 표시.
- `V_MEMROM3 __far`: 포인터가 far ROM 영역까지 도달해야 함을 표시.

이런 구조에서는 `const unsigned char *`만으로는 부족할 수 있다. ROM과 RAM의 주소 공간이 다르거나 near/far pointer 크기가 다르면, 컴파일러가 올바른 load 명령과 포인터 크기를 알 수 있도록 별도 qualifier가 필요하다.

## 예시 코드 4: AUTOSAR Compiler Abstraction식 표현과의 비교

AUTOSAR 스타일은 numbered slot 대신 의미 있는 인자를 사용한다.

```c
#define APP_CONST
#define APP_APPL_CONST

#define CONST(type, memclass) const type
#define P2CONST(ptrtype, memclass, ptrclass) const ptrtype *

CONST(unsigned char, APP_CONST) AppVersion = 0x01u;
P2CONST(unsigned char, AUTOMATIC, APP_APPL_CONST) AppVersionPtr = &AppVersion;
```

복잡한 컴파일러에서는 위 매크로가 다음처럼 달라질 수 있다.

```c
#define P2CONST(ptrtype, memclass, ptrclass) const ptrtype memclass * ptrclass
```

AUTOSAR 방식은 "무엇을 표현하는가"가 이름에 드러난다. Vector `V_MEMROM0/1/2/3` 방식은 "C 선언의 어느 위치에 끼워 넣을 것인가"가 번호로 드러난다.

## 주의점

1. `const`와 ROM 배치는 같은 말이 아니다. `const`는 타입 한정자이고, ROM/Flash 배치는 링커 섹션 배치의 결과다.
2. `V_MEMROM1/3`이 빈 값이어도 제거하면 안 된다. 현재 타깃에서는 의미가 없어 보여도 다른 컴파일러/MCU에서 필요한 slot이다.
3. 포인터 선언에서는 `const pointer`와 `pointer to const`를 구분해야 한다. `V_MEMROM1 type V_MEMROM2 V_MEMROM3 *p`는 보통 "ROM/const 데이터를 가리키는 포인터" 쪽이다.
4. `V_MEMROM2`가 `const`라면 `V_MEMROM0 V_MEMROM1 type V_MEMROM2 obj`는 `type const obj`에 가깝다. C에서는 `const type obj`와 `type const obj`가 같은 의미다.
5. section attribute를 매크로에 넣는 경우 선언과 정의 양쪽의 컴파일러 제약을 확인해야 한다. 예를 들어 GCC는 initialized global definition에 section attribute를 쓰는 전형적 예를 제시하며, 단순 미초기화 선언에서는 경고/무시가 생길 수 있다.

## 참고 자료

- 로컬 코드: `src/CBD/Common/v_def.h`, `src/CBD/VStdLib/vstdlib.h`, `src/CBD/VStdLib/vstdlib.c`, `src/CBD/Gen/desc.h`
- AUTOSAR, Specification of Compiler Abstraction, `P2VAR`, `P2CONST`, `CONST`, `VAR` macro definitions and compiler-specific placement examples: https://www.autosar.org/fileadmin/standards/R20-11/CP/AUTOSAR_SWS_CompilerAbstraction.pdf
- GCC documentation, variable attributes and `section` attribute: https://gcc.gnu.org/onlinedocs/gcc-4.3.2/gcc/Variable-Attributes.html
- IAR ColdFire C/C++ Compiler Reference, extended keywords such as `__far`, `__near`, `__root`, and pragma behavior: https://wwwfiles.iar.com/cf/EWCF_CompilerReference.pdf
