# Root_Remapping 계층 파일별 상세 분석 보고서

- 분석 대상: `Root_Remapping` 계층
- 파일 수: 1
- 생성 시각: 2026-05-28 22:37:39
- 분석 방식: C 전처리 매크로, typedef, 전역 데이터 선언, 함수 선언/정의를 자동 추출하고 함수명/주석/계층 역할을 기준으로 한글 역할 설명을 부여함.

## 계층 요약

루트 보조 파일 계층은 특정 MCU/채널 매핑 보정처럼 여러 모듈에서 참조 가능한 보조 정의를 제공한다.

## 파일 목록

- `src/CBD/RemapChannelSpc58.h` (4,886 bytes)

## 파일이름 : RemapChannelSpc58

- 경로: `src/CBD/RemapChannelSpc58.h`
- 파일 종류: `.h`
- 파일 크기: 4,886 bytes
- 파일 내용: SPC58/FlexCAN 채널 또는 레지스터 리맵을 위한 보드/MCU 보조 헤더이다.

### 1. .c/.h 파일 이름

- `RemapChannelSpc58.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 61
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 0

#### 주요 매크로
- L11 `CBD_GEN_REMAPSINGLECHANNELSPC58_H_`
- L17 `C_TOUCAN_BASIS` = `1U  /* MCAN1 */`
- L18 `USE_MCAN1`
- L21 `C_TOUCAN_BASIS` = `6U  /* MCAN6 */`
- L22 `USE_MCAN6`
- L25 `C_TOUCAN_BASIS` = `6U  /* MCAN6 */`
- L26 `USE_MCAN6`
- L50 `C_TOUCAN_BASIS_0` = `1U  /* MCAN1 */`
- L51 `USE_MCAN1`
- L82 `C_TOUCAN_BASIS_1` = `6U  /* MCAN6 */`
- L83 `USE_MCAN6`
- L119 `MCAN0_SID_FILTER_SIZE` = `4`
- L120 `MCAN0_RX_BUFFER_SIZE` = `8`
- L121 `MCAN0_RX_FIFO0_SIZE` = `8`
- L122 `MCAN0_RX_FIFO1_SIZE` = `8`
- L123 `MCAN0_TX_BUFFER_SIZE` = `4`
- L124 `MCAN0_TX_FIFO_SIZE` = `4`
- L129 `MCAN1_SID_FILTER_SIZE` = `64`
- L130 `MCAN1_XID_FILTER_SIZE` = `64`
- L131 `MCAN1_RX_BUFFER_SIZE` = `0`
- L132 `MCAN1_RX_FIFO0_SIZE` = `32`
- L133 `MCAN1_RX_FIFO1_SIZE` = `0`
- L134 `MCAN1_TX_BUFFER_SIZE` = `32`
- L135 `MCAN1_TX_FIFO_SIZE` = `0`
- L140 `MCAN2_SID_FILTER_SIZE` = `3`
- L141 `MCAN2_RX_BUFFER_SIZE` = `8`
- L142 `MCAN2_RX_FIFO0_SIZE` = `8`
- L143 `MCAN2_RX_FIFO1_SIZE` = `8`
- L144 `MCAN2_TX_BUFFER_SIZE` = `4`
- L145 `MCAN2_TX_FIFO_SIZE` = `4`
- L150 `MCAN3_SID_FILTER_SIZE` = `3`
- L151 `MCAN3_RX_BUFFER_SIZE` = `8`
- L152 `MCAN3_RX_FIFO0_SIZE` = `8`
- L153 `MCAN3_RX_FIFO1_SIZE` = `8`
- L154 `MCAN3_TX_BUFFER_SIZE` = `4`
- L155 `MCAN3_TX_FIFO_SIZE` = `4`
- L160 `MCAN4_SID_FILTER_SIZE` = `16`
- L161 `MCAN4_RX_BUFFER_SIZE` = `2`
- L162 `MCAN4_RX_FIFO0_SIZE` = `8`
- L163 `MCAN4_RX_FIFO1_SIZE` = `2`
- L164 `MCAN4_TX_BUFFER_SIZE` = `8`
- L165 `MCAN4_TX_FIFO_SIZE` = `8`
- L170 `MCAN5_SID_FILTER_SIZE` = `3`
- L171 `MCAN5_RX_BUFFER_SIZE` = `8`
- L172 `MCAN5_RX_FIFO0_SIZE` = `8`
- L173 `MCAN5_RX_FIFO1_SIZE` = `8`
- L174 `MCAN5_TX_BUFFER_SIZE` = `4`
- L175 `MCAN5_TX_FIFO_SIZE` = `4`
- L180 `MCAN6_SID_FILTER_SIZE` = `128`
- L181 `MCAN6_XID_FILTER_SIZE` = `0`
- L182 `MCAN6_RX_BUFFER_SIZE` = `32`
- L183 `MCAN6_RX_FIFO0_SIZE` = `32`
- L184 `MCAN6_RX_FIFO1_SIZE` = `0`
- L185 `MCAN6_TX_BUFFER_SIZE` = `32`
- L186 `MCAN6_TX_FIFO_SIZE` = `0`
- L191 `MCAN7_SID_FILTER_SIZE` = `3`
- L192 `MCAN7_RX_BUFFER_SIZE` = `8`
- L193 `MCAN7_RX_FIFO0_SIZE` = `8`
- L194 `MCAN7_RX_FIFO1_SIZE` = `8`
- L195 `MCAN7_TX_BUFFER_SIZE` = `4`
- L196 `MCAN7_TX_FIFO_SIZE` = `4`

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

채널/레지스터 리맵 파일은 CAN-FD controller 인스턴스 또는 FD-capable channel 선택과 관련될 수 있다.

- 기존 채널이 CAN-FD를 지원하는 하드웨어 인스턴스인지 확인한다.
- Classical CAN controller와 CAN-FD controller의 base address, mailbox layout, interrupt vector가 다르면 리맵 정의를 분리한다.
