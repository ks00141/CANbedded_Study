# Common 계층 파일별 상세 분석 보고서

- 분석 대상: `Common` 계층
- 파일 수: 1
- 생성 시각: 2026-05-28 22:37:38
- 분석 방식: C 전처리 매크로, typedef, 전역 데이터 선언, 함수 선언/정의를 자동 추출하고 함수명/주석/계층 역할을 기준으로 한글 역할 설명을 부여함.

## 계층 요약

Common 계층은 모든 Vector 모듈이 공유하는 정수 타입, 메모리 클래스, 포인터/비트 타입의 기반 정의를 제공한다.

## 파일 목록

- `src/CBD/Common/v_def.h` (44,691 bytes)

## 파일이름 : v_def

- 경로: `src/CBD/Common/v_def.h`
- 파일 종류: `.h`
- 파일 크기: 44,691 bytes
- 파일 내용: Vector 공통 정수 타입, 메모리 클래스, 비트 구조체를 정의한다.

### 1. .c/.h 파일 이름

- `v_def.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 91
- typedef/struct/enum 후보 수: 10
- 전역 변수/테이블 선언 후보 수: 0

#### 주요 매크로
- L419 `V_DEF_H`
- L426 `COMMON_VDEF_VERSION` = `0x0332`
- L427 `COMMON_VDEF_RELEASE_VERSION` = `0x00`
- L430 `V_DEF_VERSION` = `COMMON_VDEF_VERSION`
- L437 `V_DEF_SUPPORTED_PLATFORM`
- L459 `canuint8` = `vuint8`
- L464 `cansint8` = `vsint8`
- L470 `canuint16` = `vuint16`
- L475 `cansint16` = `vsint16`
- L482 `canuint32` = `vuint32`
- L487 `cansint32` = `vsint32`
- L509 `vuintx` = `vuint8`
- L512 `vsintx` = `vsint8`
- L518 `vuintx` = `vuint16`
- L521 `vsintx` = `vsint16`
- L527 `vuintx` = `vuint32`
- L530 `vsintx` = `vsint32`
- L544 `canbittype` = `vbittype`
- L754 `MEMORY_HUGE` = `/* no entry                         */`
- L766 `V_MEMROM0` = `/* addition qualifier data access in ROM  */`
- L770 `V_MEMROM1_NEAR` = `/* fast data access in ROM */`
- L774 `V_MEMROM1` = `/* fast data access in ROM */`
- L778 `V_MEMROM1_FAR` = `/* slow addressing mode in ROM */`
- L784 `V_MEMROM2_NEAR` = `const    /* fast data access in ROM */`
- L787 `MEMORY_ROM_NEAR` = `V_MEMROM2_NEAR`
- L789 `V_MEMROM2_NEAR` = `MEMORY_ROM_NEAR`
- L794 `V_MEMROM2` = `const    /* fast data access in ROM */`
- L797 `MEMORY_ROM` = `V_MEMROM2`
- L799 `V_MEMROM2` = `MEMORY_ROM`
- L804 `V_MEMROM2_FAR` = `const    /* slow addressing mode in ROM */`
- L807 `MEMORY_ROM_FAR` = `V_MEMROM2_FAR`
- L809 `V_MEMROM2_FAR` = `MEMORY_ROM_FAR`
- L813 `V_MEMROM3`
- L819 `V_MEMRAM0` = `/* addition qualifier data access in RAM  */`
- L823 `V_MEMRAM1_NEAR` = `/* fast data access in RAM */`
- L827 `V_MEMRAM1` = `/* fast data access in RAM */`
- L831 `V_MEMRAM1_FAR` = `/* slow addressing mode in RAM */`
- L837 `V_MEMRAM2_NEAR` = `/* fast data access in RAM */`
- L840 `MEMORY_NEAR` = `V_MEMRAM2_NEAR`
- L842 `V_MEMRAM2_NEAR` = `MEMORY_NEAR`
- L847 `V_MEMRAM2` = `/* fast data access in RAM */`
- L850 `MEMORY_NORMAL` = `V_MEMRAM2`
- L852 `V_MEMRAM2` = `MEMORY_NORMAL`
- L857 `V_MEMRAM2_FAR` = `/* slow addressing mode in RAM */`
- L860 `MEMORY_FAR` = `V_MEMRAM2_FAR`
- L862 `V_MEMRAM2_FAR` = `MEMORY_FAR`
- L866 `V_MEMRAM3_NEAR` = `/* fast data access in RAM */`
- L870 `V_MEMRAM3` = `/* fast data access in RAM */`
- L874 `V_MEMRAM3_FAR` = `/* slow addressing mode in RAM */`
- L880 `VUINT8_CAST`
- L883 `VSINT8_CAST`
- L886 `VUINT16_CAST`
- L889 `VSINT16_CAST`
- L892 `CANBITTYPE_CAST`
- L895 `CANSINTCPUTYPE_CAST`
- L898 `CANUINTCPUTYPE_CAST`
- L915 `V_NULL` = `((void*)0)`
- L920 `NULL` = `V_NULL`
- L926 `V_ENABLE_CBD_ABSTRACTION`
- L927 `STATIC` = `static /* MSR3 */`
- L928 `CAN_STATIC` = `static /* MSR4 */`
- L929 `AUTOMATIC`
- L931 `NULL_PTR` = `V_NULL`
- L934 `V_NONE` = `/* empty storage used instead of extern, static, volatile... */`
- L937 `C_CALLBACK_1`
- L940 `C_CALLBACK_2`
- L943 `C_API_1`
- L946 `C_API_2`
- L949 `C_API_3`
- L951 `V_DEF_VAR(storage, vartype, memclass)` = `V_MEMRAM0 storage V_MEMRAM1 vartype V_MEMRAM2`
- L952 `V_DEF_VAR_NEAR(storage, vartype)` = `V_MEMRAM0 storage V_MEMRAM1_NEAR vartype V_MEMRAM2_NEAR`
- L953 `V_DEF_VAR_FAR(storage, vartype)` = `V_MEMRAM0 storage V_MEMRAM1_FAR vartype V_MEMRAM2_FAR`
- L954 `V_DEF_VAR_TYPE(storage, vartype)` = `typedef storage V_MEMRAM1 vartype V_MEMRAM2`
- L955 `V_DEF_P2VAR(storage, ptrtype, memclass, ptrclass)` = `V_MEMRAM0 storage V_MEMRAM1 ptrtype V_MEMRAM2 *`
- L956 `V_DEF_P2VAR_PARA(storage, ptrtype, memclass, ptrclass)` = `storage V_MEMRAM1 ptrtype V_MEMRAM2 *`
- L957 `V_DEF_P2VAR_TYPE(storage, ptrtype, ptrclass)` = `typedef storage V_MEMRAM1 ptrtype V_MEMRAM2 *`
- L959 `V_DEF_P2SFR_CAN(storage, ptrtype, memclass)` = `V_MEMRAM0 storage V_MEMRAM1 ptrtype MEMORY_CAN *`
- L960 `V_DEF_P2SFR_CAN_TYPE(storage, ptrtype)` = `typedef storage V_MEMRAM1 ptrtype MEMORY_CAN *`
- L962 `V_DEF_CONSTP2VAR(storage, ptrtype, memclass, ptrclass)` = `V_MEMROM0 storage V_MEMRAM1 ptrtype V_MEMRAM2 V_MEMRAM3 * V_MEMROM1 V_MEMROM2`
- L963 `V_DEF_CONST(storage, type, memclass)` = `V_MEMROM0 storage V_MEMROM1 type V_MEMROM2`
- L964 `V_DEF_CONST_NEAR(storage, type)` = `V_MEMROM0 storage V_MEMROM1_NEAR type V_MEMROM2_NEAR`
- L965 `V_DEF_CONST_FAR(storage, type)` = `V_MEMROM0 storage V_MEMROM1_FAR type V_MEMROM2_FAR`
- L966 `V_DEF_CONST_TYPE(storage, type, memclass)` = `typedef storage V_MEMROM1 type V_MEMROM2`
- L967 `V_DEF_P2CONST(storage, ptrtype, memclass, ptrclass)` = `V_MEMRAM0 storage V_MEMROM1 ptrtype V_MEMROM2 V_MEMROM3 * V_MEMRAM1 V_MEMRAM2`
- L968 `V_DEF_P2CONST_PARA(storage, ptrtype, memclass, ptrclass)` = `storage V_MEMROM1 ptrtype V_MEMROM2 V_MEMROM3 * V_MEMRAM1 V_MEMRAM2`
- L969 `V_DEF_P2CONST_TYPE(storage, ptrtype, ptrclass)` = `typedef storage V_MEMROM1 ptrtype V_MEMROM2 V_MEMROM3 *`
- L970 `V_DEF_CONSTP2CONST(storage, ptrtype, memclass, ptrclass)` = `V_MEMROM0 storage V_MEMROM1 ptrtype V_MEMROM2 V_MEMROM3 * V_MEMROM1 V_MEMROM2`
- L971 `V_DEF_FUNC(storage, rettype, memclass)` = `storage rettype`
- L972 `V_DEF_FUNC_API(storage, rettype, memclass)` = `storage C_API_1 rettype C_API_2`
- L973 `V_DEF_FUNC_CBK(storage, rettype, memclass)` = `storage C_CALLBACK_1 rettype C_CALLBACK_2`
- L974 `V_DEF_P2FUNC(storage, rettype, ptrclass, fctname)` = `storage C_CALLBACK_1 rettype (C_CALLBACK_2 *fctname)`

#### 주요 typedef/구조체
- L457 `typedef unsigned char  vuint8;`
- L462 `typedef signed char    vsint8;`
- L468 `typedef unsigned short vuint16;`
- L473 `typedef signed short   vsint16;`
- L480 `typedef unsigned long  vuint32;`
- L485 `typedef signed long    vsint32;`
- L491 `typedef unsigned char *TxDataPtr;              /* ptr to transmission data buffer */`
- L492 `typedef unsigned char *RxDataPtr;              /* ptr to receiving data buffer    */`
- L540 `typedef unsigned short   vbittype;`
- L542 `typedef unsigned int     vbittype;`

#### 주요 전역 변수/테이블 선언
- 없음

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 0

- 없음

### 4. 각 함수별 역할 설명

- 함수 없음: 설정/타입/매크로 전용 파일이다.

### 5. 필요시 코드 부연 설명

이 파일은 생성 설정/선언 성격이 강해 별도 코드 전문 인용 없이 심볼 표로 역할을 설명했다.

## CAN-FD 마이그레이션 관점

Common 계층의 변경은 작다. 다만 64바이트 payload와 더 큰 진단 응답을 다루므로 길이 타입과 buffer helper의 사용 범위를 점검해야 한다.

- CAN frame payload length는 `vuint8`로 표현 가능하지만, TP/UDS 전체 메시지 길이는 `vuint16` 이상을 유지한다.
- 메모리 복사 함수는 64바이트 이상의 buffer에도 안전하게 사용되도록 인자 타입과 NULL 정책을 확인한다.
- FD frame 구조체가 공통 타입에 들어간다면 Classical/FD 공용 필드를 명확히 분리한다.
