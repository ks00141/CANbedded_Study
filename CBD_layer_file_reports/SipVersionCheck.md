# SipVersionCheck 계층 파일별 상세 분석 보고서

- 분석 대상: `SipVersionCheck` 계층
- 파일 수: 2
- 생성 시각: 2026-05-28 22:37:38
- 분석 방식: C 전처리 매크로, typedef, 전역 데이터 선언, 함수 선언/정의를 자동 추출하고 함수명/주석/계층 역할을 기준으로 한글 역할 설명을 부여함.

## 계층 요약

SipVersionCheck 계층은 포함된 Vector 모듈 버전 조합의 정합성을 확인한다.

## 파일 목록

- `src/CBD/SipVersionCheck/sip_vers.c` (8,170 bytes)
- `src/CBD/SipVersionCheck/sip_vers.h` (958 bytes)

## 파일이름 : sip_vers

- 경로: `src/CBD/SipVersionCheck/sip_vers.c`
- 파일 종류: `.c`
- 파일 크기: 8,170 bytes
- 파일 내용: Vector SIP 구성품의 버전 정합성 확인을 수행한다.

### 1. .c/.h 파일 이름

- `sip_vers.c`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 0
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 0

#### 주요 매크로
- 없음

#### 주요 typedef/구조체
- 없음

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

CAN-FD 지원 모듈로 교체하거나 추가할 경우 SIP/모듈 버전 정합성 확인 항목도 갱신해야 한다.

- CAN Driver, TP, IL 생성 모듈이 CAN-FD 지원 버전인지 확인한다.
- Classical CAN 전용 모듈과 CAN-FD 모듈이 섞이지 않도록 버전 체크 조건을 추가한다.

## 파일이름 : sip_vers

- 경로: `src/CBD/SipVersionCheck/sip_vers.h`
- 파일 종류: `.h`
- 파일 크기: 958 bytes
- 파일 내용: Vector SIP 구성품의 버전 정합성 확인을 수행한다.

### 1. .c/.h 파일 이름

- `sip_vers.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 1
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 3

#### 주요 매크로
- L16 `_SIP_VERSION_CHECK_`

#### 주요 typedef/구조체
- 없음

#### 주요 전역 변수/테이블 선언
- L20 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 kSipMainVersion;`
- L21 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 kSipSubVersion;`
- L22 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 kSipBugFixVersion;`

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 0

- 없음

### 4. 각 함수별 역할 설명

- 함수 없음: 설정/타입/매크로 전용 파일이다.

### 5. 필요시 코드 부연 설명

이 파일은 생성 설정/선언 성격이 강해 별도 코드 전문 인용 없이 심볼 표로 역할을 설명했다.
