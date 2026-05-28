# VStdLib 계층 파일별 상세 분석 보고서

- 분석 대상: `VStdLib` 계층
- 파일 수: 2
- 생성 시각: 2026-05-28 22:37:39
- 분석 방식: C 전처리 매크로, typedef, 전역 데이터 선언, 함수 선언/정의를 자동 추출하고 함수명/주석/계층 역할을 기준으로 한글 역할 설명을 부여함.

## 계층 요약

VStdLib 계층은 Vector 스택에서 공통으로 쓰는 메모리 처리와 인터럽트 보호 유틸리티를 제공한다.

## 파일 목록

- `src/CBD/VStdLib/vstdlib.c` (31,513 bytes)
- `src/CBD/VStdLib/vstdlib.h` (28,083 bytes)

## 파일이름 : vstdlib

- 경로: `src/CBD/VStdLib/vstdlib.c`
- 파일 종류: `.c`
- 파일 크기: 31,513 bytes
- 파일 내용: Vector 공통 메모리/인터럽트 유틸리티 구현 또는 선언이다.

### 1. .c/.h 파일 이름

- `vstdlib.c`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 9
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 0

#### 주요 매크로
- L179 `_VSTD_LIB_C_`
- L192 `VSTD_ENABLE_MSR_LIB_VERSION_CHECK`
- L216 `VSTDLIB_LIB_VERSION` = `VSTDLIB_MPC_LIB_VERSION`
- L268 `PPC_INTC_CPR` = `0xFFF48008`
- L270 `VSTD_INTC_CPR` = `(*((volatile vuint32 *)((vuint32)(PPC_INTC_CPR))))`
- L273 `VSTD_MSR_MASK` = `((tVStdIrqStateType)0x8000)`
- L276 `VSTD_STATIC_DECL` = `static`
- L305 `VStdAssert(p,e)` = `if(!(p)){ ApplVStdFatalError(e); }`
- L307 `VStdAssert(p,e)`

#### 주요 typedef/구조체
- 없음

#### 주요 전역 변수/테이블 선언
- 없음

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 5

- L323 `VStdInitPowerOn(void)` [정의]
- L589 `VStdLL_GlobalInterruptDisable(tVStdIrqStateType *oldStatusPtr)` [정의]
- L592 `VStdGlobalIntDisable()` [선언]
- L605 `VStdLL_GlobalInterruptRestore(tVStdIrqStateType oldStatus)` [정의]
- L608 `VStdGlobalIntDisable()` [선언]

### 4. 각 함수별 역할 설명

- `VStdInitPowerOn` (L323, 정의): 전원 인가 직후 한 번 수행되는 모듈 전역 초기화를 담당한다.
- `VStdLL_GlobalInterruptDisable` (L589, 정의): 전역 또는 CAN 관련 인터럽트 비활성화/복구 보호 구간을 제공한다.
- `VStdGlobalIntDisable` (L592, 선언): 전역 또는 CAN 관련 인터럽트 비활성화/복구 보호 구간을 제공한다.
- `VStdLL_GlobalInterruptRestore` (L605, 정의): 전역 또는 CAN 관련 인터럽트 비활성화/복구 보호 구간을 제공한다.
- `VStdGlobalIntDisable` (L608, 선언): 전역 또는 CAN 관련 인터럽트 비활성화/복구 보호 구간을 제공한다.

### 5. 필요시 코드 부연 설명

대표 함수 원형 수준의 코드 맥락이다. 전체 구현은 원본 파일이 크므로 함수명/역할 중심으로 분석했다.

```c
// src/CBD/VStdLib/vstdlib.c:323
VSTD_API_0 void VSTD_API_1 VStdInitPowerOn(void)
```
```c
// src/CBD/VStdLib/vstdlib.c:589
void VStdLL_GlobalInterruptDisable(tVStdIrqStateType *oldStatusPtr)
```
```c
// src/CBD/VStdLib/vstdlib.c:592
vstdMSR = VStdGlobalIntDisable()
```
```c
// src/CBD/VStdLib/vstdlib.c:605
void VStdLL_GlobalInterruptRestore(tVStdIrqStateType oldStatus)
```
```c
// src/CBD/VStdLib/vstdlib.c:608
vstdMSR = VStdGlobalIntDisable()
```

## 파일이름 : vstdlib

- 경로: `src/CBD/VStdLib/vstdlib.h`
- 파일 종류: `.h`
- 파일 크기: 28,083 bytes
- 파일 내용: Vector 공통 메모리/인터럽트 유틸리티 구현 또는 선언이다.

### 1. .c/.h 파일 이름

- `vstdlib.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 90
- typedef/struct/enum 후보 수: 1
- 전역 변수/테이블 선언 후보 수: 3

#### 주요 매크로
- L179 `_VSTDLIB_H_`
- L188 `VSTDLIB_MPC_VERSION` = `0x0115u`
- L189 `VSTDLIB_MPC_RELEASE_VERSION` = `0x00u`
- L192 `VSTDLIB__COREHLL_VERSION` = `0x0305u`
- L193 `VSTDLIB__COREHLL_RELEASE_VERSION` = `0x01u`
- L196 `COMMON_VSTDLIB_VERSION` = `VSTDLIB__COREHLL_VERSION`
- L197 `COMMON_VSTDLIB_RELEASE_VERSION` = `VSTDLIB__COREHLL_RELEASE_VERSION`
- L201 `VSTD_API_0`
- L205 `VSTD_API_1`
- L209 `VSTD_API_2`
- L213 `VSTD_CALLBACK_0`
- L217 `VSTD_CALLBACK_1`
- L223 `VSTD_ENABLE_INTCTRL_HANDLING`
- L224 `VSTD_HL_ENABLE_SUPPORT_IRQ_FCT` = `/* compatibility for Autosar */`
- L225 `VSTD_ENABLE_SUPPORT_IRQ_FCT`
- L227 `VSTD_DISABLE_INTCTRL_HANDLING`
- L228 `VSTD_HL_DISABLE_SUPPORT_IRQ_FCT` = `/* compatibility for Autosar */`
- L229 `VSTD_DISABLE_SUPPORT_IRQ_FCT`
- L233 `VSTD_ENABLE_LIBRARY_FUNCTIONS`
- L239 `kVStdErrorIntDisableTooOften` = `((vuint8)0x01)`
- L240 `kVStdErrorIntRestoreTooOften` = `((vuint8)0x02)`
- L241 `kVStdErrorMemClrInvalidParameter` = `((vuint8)0x03)`
- L242 `kVStdErrorMemCpyInvalidParameter` = `((vuint8)0x04)`
- L243 `kVStdErrorFunctionNotSupportedByHw` = `((vuint8)0x05)`
- L244 `kVStdErrorMemSetInvalidParameter` = `((vuint8)0x06)`
- L245 `kVStdErrorUnexpectedLockState` = `((vuint8)0x07)`
- L251 `VSTD_HL_DISABLE_SUPPORT_MEM_FCT`
- L252 `VSTD_HL_ENABLE_MEM_FCT_MAPPING`
- L253 `VSTD_HL_USE_VSTD_MEMCLR`
- L254 `VSTD_HL_USE_VSTD_MEMSET`
- L255 `VSTD_HL_USE_VSTD_RAMMEMCPY`
- L256 `VSTD_HL_USE_VSTD_ROMMEMCPY`
- L257 `VSTD_HL_USE_VSTD_WORD_MEMCPY`
- L258 `VSTD_HL_USE_VSTD_DWORD_MEMCPY`
- L284 `VStdDeclareGlobalInterruptOldStatus` = `tVStdIrqStateType localInterruptOldStatus;`
- L285 `VStdPutGlobalInterruptOldStatus(x)` = `(localInterruptOldStatus = (x))`
- L286 `VStdGetGlobalInterruptOldStatus(x)` = `((x) = localInterruptOldStatus)`
- L287 `VStdGlobalInterruptDisable()` = `VStdLL_GlobalInterruptDisable(&localInterruptOldStatus)`
- L288 `VStdGlobalInterruptRestore()` = `VStdLL_GlobalInterruptRestore(localInterruptOldStatus)`
- L290 `VStdDeclareGlobalInterruptOldStatus`
- L291 `VStdPutGlobalInterruptOldStatus(x)`
- L292 `VStdGetGlobalInterruptOldStatus(x)`
- L294 `VStdGlobalInterruptDisable()` = `OsSaveDisableGlobalNested()`
- L295 `VStdGlobalInterruptRestore()` = `OsRestoreEnableGlobalNested()`
- L296 `VStdSuspendAllInterrupts()` = `OsSaveDisableGlobalNested()`
- L297 `VStdResumeAllInterrupts()` = `OsRestoreEnableGlobalNested()`
- L299 `VStdGlobalInterruptDisable()` = `SuspendAllInterrupts()`
- L300 `VStdGlobalInterruptRestore()` = `ResumeAllInterrupts()`
- L301 `VStdSuspendAllInterrupts()` = `SuspendAllInterrupts()`
- L302 `VStdResumeAllInterrupts()` = `ResumeAllInterrupts()`
- L305 `VStdDeclareGlobalInterruptOldStatus`
- L306 `VStdPutGlobalInterruptOldStatus(x)`
- L307 `VStdGetGlobalInterruptOldStatus(x)`
- L308 `VStdGlobalInterruptDisable()` = `VStdUserNestedDisable()`
- L309 `VStdGlobalInterruptRestore()` = `VStdUserNestedRestore()`
- L310 `VStdSuspendAllInterrupts()` = `VStdUserNestedDisable()`
- L311 `VStdResumeAllInterrupts()` = `VStdUserNestedRestore()`
- L315 `VStdNestedGlobalInterruptDisable()` = `VStdGlobalInterruptDisable()`
- L316 `VStdNestedGlobalInterruptRestore()` = `VStdGlobalInterruptRestore()`
- L323 `VStdMemSet(d, p, l)` = `VStdMemSetInternal((void*)(d), (p), (vuint16)(l))`
- L324 `VStdMemNearSet(d, p, l)` = `VStdMemSetInternal((void*)(d), (p), (vuint16)(l))`
- L325 `VStdMemFarSet(d, p, l)` = `VStdMemSetInternal((void*)(d), (p), (vuint16)(l))`
- L326 `VStdMemClr(d, l)` = `VStdMemClrInternal((void*)(d), (vuint16)(l))`
- L327 `VStdMemNearClr(d, l)` = `VStdMemClrInternal((void*)(d), (vuint16)(l))`
- L328 `VStdMemFarClr(d, l)` = `VStdMemClrInternal((void*)(d), (vuint16)(l))`
- L330 `VStdMemCpyRamToRam(d, s, l)` = `VStdRamMemCpy((void*)(d), (void*)(s), (vuint16)(l))`
- L331 `VStdMemCpyRamToNearRam(d, s, l)` = `VStdRamMemCpy((void*)(d), (void*)(s), (vuint16)(l))`
- L332 `VStdMemCpyRamToFarRam(d, s, l)` = `VStdRamMemCpy((void*)(d), (void*)(s), (vuint16)(l))`
- L333 `VStdMemCpyFarRamToRam(d, s, l)` = `VStdRamMemCpy((void*)(d), (void*)(s), (vuint16)(l))`
- L334 `VStdMemCpyFarRamToNearRam(d, s, l)` = `VStdRamMemCpy((void*)(d), (void*)(s), (vuint16)(l))`
- L335 `VStdMemCpyFarRamToFarRam(d, s, l)` = `VStdRamMemCpy((void*)(d), (void*)(s), (vuint16)(l))`
- L337 `VStdMemCpyRomToRam(d, s, l)` = `VStdRomMemCpy((void*)(d), (V_MEMROM1 void V_MEMROM2 V_MEMROM3 *)(s), (vuint16)(l))`
- L338 `VStdMemCpyRomToNearRam(d, s, l)` = `VStdRomMemCpy((void*)(d), (V_MEMROM1 void V_MEMROM2 V_MEMROM3 *)(s), (vuint16)(l))`
- L339 `VStdMemCpyRomToFarRam(d, s, l)` = `VStdRomMemCpy((void*)(d), (V_MEMROM1 void V_MEMROM2 V_MEMROM3 *)(s), (vuint16)(l))`
- L340 `VStdMemCpyFarRomToRam(d, s, l)` = `VStdRomMemCpy((void*)(d), (V_MEMROM1 void V_MEMROM2 V_MEMROM3 *)(s), (vuint16)(l))`
- L341 `VStdMemCpyFarRomToNearRam(d, s, l)` = `VStdRomMemCpy((void*)(d), (V_MEMROM1 void V_MEMROM2 V_MEMROM3 *)(s), (vuint16)(l))`
- L342 `VStdMemCpyFarRomToFarRam(d, s, l)` = `VStdRomMemCpy((void*)(d), (V_MEMROM1 void V_MEMROM2 V_MEMROM3 *)(s), (vuint16)(l))`
- L344 `VStdMemCpy16RamToRam(d, s, l)` = `VStdRamMemCpy16((void*)(d), (void*)(s), (vuint16)(l))`
- L345 `VStdMemCpy16RamToFarRam(d, s, l)` = `VStdRamMemCpy16((void*)(d), (void*)(s), (vuint16)(l))`
- L346 `VStdMemCpy16FarRamToRam(d, s, l)` = `VStdRamMemCpy16((void*)(d), (void*)(s), (vuint16)(l))`
- L348 `VStdMemCpy32RamToRam(d, s, l)` = `VStdRamMemCpy32((void*)(d), (void*)(s), (vuint16)(l))`
- L349 `VStdMemCpy32RamToFarRam(d, s, l)` = `VStdRamMemCpy32((void*)(d), (void*)(s), (vuint16)(l))`
- L350 `VStdMemCpy32FarRamToRam(d, s, l)` = `VStdRamMemCpy32((void*)(d), (void*)(s), (vuint16)(l))`
- L356 `VStdMemCpy(d,s,l)` = `VStdMemCpyRamToRam((d),(s),(l))`
- L360 `VStdRamMemCpy(d,s,l)` = `VStdMemCpyRamToRam((d),(s),(l))`
- L365 `VStdRomMemCpy(d,s,l)` = `VStdMemCpyRomToRam((d),(s),(l))`
- L427 `VSTD_NOINLINE_PRE`
- L430 `VSTD_NOINLINE_POST`
- L453 `VStdLL_GlobalInterruptDisable(oldStatusPtr)` = `*(oldStatusPtr) = VStdGlobalIntDisable()`
- L454 `VStdLL_GlobalInterruptRestore(oldStatus)` = `VStdGlobalIntRestore(oldStatus)`

#### 주요 typedef/구조체
- L277 `typedef vuint32 tVStdIrqStateType;`

#### 주요 전역 변수/테이블 선언
- L264 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 kVStdMainVersion;`
- L265 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 kVStdSubVersion;`
- L266 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 kVStdReleaseVersion;`

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 4

- L374 `VStdInitPowerOn(void)` [선언]
- L449 `VStdLL_GlobalInterruptDisable(tVStdIrqStateType *oldStatusPtr)` [선언]
- L450 `VStdLL_GlobalInterruptRestore(tVStdIrqStateType oldStatus)` [선언]
- L464 `ApplVStdFatalError(vuint8 nErrorNumber)` [선언]

### 4. 각 함수별 역할 설명

- `VStdInitPowerOn` (L374, 선언): 전원 인가 직후 한 번 수행되는 모듈 전역 초기화를 담당한다.
- `VStdLL_GlobalInterruptDisable` (L449, 선언): 전역 또는 CAN 관련 인터럽트 비활성화/복구 보호 구간을 제공한다.
- `VStdLL_GlobalInterruptRestore` (L450, 선언): 전역 또는 CAN 관련 인터럽트 비활성화/복구 보호 구간을 제공한다.
- `ApplVStdFatalError` (L464, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.

### 5. 필요시 코드 부연 설명

이 파일은 생성 설정/선언 성격이 강해 별도 코드 전문 인용 없이 심볼 표로 역할을 설명했다.

## CAN-FD 마이그레이션 관점

VStdLib 자체는 CAN-FD에 직접 종속되지 않는다. 하지만 CAN-FD 전환 후 64바이트 payload, 더 큰 TP/UDS buffer 복사가 늘어나므로 메모리 유틸리티의 안전성이 더 중요해진다.

- `VStdRamMemCpy`, `VStdMemClrInternal`이 64바이트 이상 copy/clear에 문제없는지 확인한다.
- interrupt lock 구간이 긴 FD buffer 복사 때문에 과도하게 길어지지 않도록 계층별 임계구역을 재검토한다.
