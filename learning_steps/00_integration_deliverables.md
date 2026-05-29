# 0단계: 최종 산출물과 통합 전략

## 최종 목표

이 학습자료를 따라 구현하면 최종적으로 두 가지 미들웨어 산출물이 만들어져야 한다.

1. `SPC584C70` MCU에서 HS-CAN/Classical CAN 통신이 가능한 미들웨어
2. `SPC584C70` MCU에서 CAN-FD 통신이 가능한 미들웨어

두 산출물은 서로 다른 프로젝트가 아니라 같은 아키텍처에서 나온 variant로 관리한다. 공통 계층, UDS, CCL, NM, IL 대부분은 공유하고, CAN Driver 설정, frame model, ISO-TP frame capacity, CAN-FD DLC/BRS 처리만 variant별로 달라지게 하는 것이 목표이다.

## 단계별 산출물 지도

| 단계 | 생성해야 하는 결과물 | 다음 단계와의 연결 |
|---|---|---|
| 1 | 공통 타입, memory class, VStdLib | 모든 C 파일의 기본 include |
| 2 | config header와 variant macro | 테이블 크기, 기능 enable, MCU 설정 |
| 3 | CAN message handle, Tx/Rx config, buffer | CAN Driver와 IL이 공유 |
| 4 | HS-CAN Driver, SPC584C70 M_CAN HAL adapter | IL/TP/NM/CCL의 하위 송수신 API |
| 5 | IL signal accessor, periodic Tx scheduler | 애플리케이션 신호와 CAN Driver 연결 |
| 6 | ISO-TP classic 상태 머신 | UDS request/response 전송 |
| 7 | UDS dispatcher와 app callback | 진단 서비스 처리 |
| 8 | NM BusOff recovery | CAN Driver error와 CCL 상태 연결 |
| 9 | CCL communication policy | 앱/진단/NM 통신 제어 통합 |
| 10 | CAN-FD frame model, DLC 변환, ISO-TP FD 확장 | CAN-FD variant 완성 |

## 최종 디렉터리 예시

```text
middleware/
  common/
  config/
    mw_variant.h
    spc584c70_clock_cfg.h
    can_cfg_hscan.h
    can_cfg_canfd.h
  can/
    can.h
    can.c
    can_frame.h
    can_hal.h
    can_hal_spc584c70.c
    can_par.c
  il/
  isotp/
  uds/
  nm/
  ccl/
  app/
bringup/
  hscan_loopback/
  hscan_bus_analyzer/
  canfd_bus_analyzer/
  uds_diagnostic/
logs/
  map_files/
  analyzer_traces/
  debugger_watch/
```

## HS-CAN 미들웨어 통합 조건

HS-CAN 산출물은 다음 조건을 만족해야 한다.

- Classical CAN 11-bit/29-bit ID 정책이 설정으로 분리되어 있다.
- payload length는 0~8바이트 범위에서 검증된다.
- Tx handle 기반 송신과 Rx ID 기반 dispatch가 동작한다.
- IL signal accessor가 payload를 정확히 packing/unpacking한다.
- ISO-TP classic Single/Multi-frame 송수신이 동작한다.
- UDS 기본 서비스 `0x10`, `0x3E`, `0x22`가 응답한다.
- BusOff callback과 복구 task가 동작한다.
- CCL Tx/Rx enable과 online/offline 정책이 Driver에 적용된다.

## CAN-FD 미들웨어 통합 조건

CAN-FD 산출물은 HS-CAN 조건을 모두 유지하면서 다음을 추가로 만족해야 한다.

- frame 구조에 `is_fd`, `brs`, `esi`, `dlc`, `length`, `data[64]`가 있다.
- CAN-FD DLC와 실제 length를 분리해서 관리한다.
- 12/16/20/24/32/48/64바이트 payload 송수신이 검증된다.
- BRS on/off variant를 설정할 수 있다.
- ISO-TP over CAN-FD의 Single Frame capacity와 Consecutive Frame capacity가 설정 기반으로 계산된다.
- UDS 계층은 CAN-FD 여부를 직접 알지 않고 TP payload만 처리한다.
- HS-CAN 회귀 검증이 CAN-FD 변경 후에도 통과한다.

## 단계별 구현 흐름

1. 공통 타입과 메모리 추상화부터 만든 뒤, linker map에서 ROM/RAM 배치가 의도대로 되었는지 확인한다.
2. 설정 계층에서 `SPC584C70` 보드의 M_CAN instance, pinmux, transceiver enable pin, nominal/data bitrate를 먼저 고정한다.
3. CAN data model은 Classical CAN과 CAN-FD가 함께 쓸 수 있게 `dlc`와 `length`를 분리한다.
4. CAN Driver는 처음부터 실제 M_CAN register, Message RAM, interrupt, transceiver 제어를 대상으로 bring-up한다.
5. IL, ISO-TP, UDS, NM, CCL은 실제 CAN analyzer trace와 디버거 watch 값을 기준으로 단계별로 붙인다.
6. 9단계 완료 시 HS-CAN 미들웨어 baseline을 만들고, 10단계에서 같은 구조를 CAN-FD로 확장한다.

## 통합 시 적용 고려사항

- `SPC584C70`은 8개 M_CAN 기반 ISO CAN-FD 인터페이스를 제공하므로 HAL adapter는 M_CAN instance, shared Message RAM offset, interrupt line을 설정으로 받는다.
- `SPC58EC70`과 CAN-FD 주변장치 설명은 유사하지만, `SPC584C70`의 single computing core 전제에 맞춰 startup, linker, RAM map, interrupt target을 별도로 검증한다.
- peripheral clock과 CAN bit timing은 코드에 하드코딩하지 않고 설정 구조체로 관리한다.
- ISR과 task 사이의 공유 buffer는 lock, queue, double buffer 중 하나로 보호한다.
- UDS response buffer는 CAN-FD 전환 후 커질 수 있지만, RAM 사용량을 반드시 계산한다.
- CAN-FD BRS를 켤 때는 transceiver와 네트워크 전체가 data phase bitrate를 지원해야 한다.
- 모든 단계 산출물은 `SPC584C70` 보드, 디버거, CAN/CAN-FD analyzer에서 확인 가능한 관찰 포인트를 가져야 한다.

## 공통 문제 케이스와 트러블슈팅

| 문제 | 가능한 원인 | 확인 방법 | 해결 방향 |
|---|---|---|---|
| 빌드 실패 | 설정 macro 누락, include 순서 오류 | 전처리 결과와 include tree 확인 | 공통 config header를 먼저 include |
| 송신 요청 실패 | offline 상태, invalid handle, mailbox busy | Driver return code와 state log 확인 | CCL 상태와 Tx queue 확인 |
| 수신 callback 없음 | filter/mask 오류, ID table mismatch | raw Rx interrupt와 Rx handle mapping 비교 | filter와 `can_par` 테이블 수정 |
| 신호 값 이상 | endian, bit offset, update 중 경쟁 상태 | payload dump와 accessor 결과 비교 | shift/mask 수정, lock 적용 |
| ISO-TP timeout | FC/CF 누락, STmin 처리 오류 | TP state, timer, SN log 확인 | timer resolution과 state transition 수정 |
| UDS NRC 이상 | session/security/service table 오류 | SID, sub-function, state dump | condition table과 callback return code 수정 |
| BusOff 반복 | bitrate, termination, transceiver, CAN-FD data phase 오류 | analyzer/error counter/scope 확인 | timing, wiring, BRS, transceiver 설정 재검토 |
| CAN-FD length 오류 | DLC와 length 혼동 | DLC/length 로그와 payload dump | DLC 변환 함수 강제 사용 |

## 권장 검증 순서

1. SPC584C70 보드에서 clock, pinmux, transceiver enable, M_CAN reset 해제 상태를 디버거로 확인한다.
2. M_CAN internal loopback으로 Tx/Rx interrupt와 Message RAM 배치를 확인한다.
3. CAN analyzer로 HS-CAN frame 송수신을 확인한다.
4. IL signal packing/unpacking 결과를 analyzer payload와 디버거 watch 값으로 비교한다.
5. ISO-TP Single/Multi-frame과 UDS 기본 응답을 진단 장비 또는 CAN analyzer로 확인한다.
6. BusOff와 CCL online/offline 정책을 실제 bus 오류 조건에서 확인한다.
7. CAN-FD controller 설정, FD payload length, BRS on/off를 확인한다.
8. CAN-FD 적용 후 HS-CAN baseline trace를 다시 비교한다.

## Q&A

1. Q: 0단계에서 코드를 많이 작성하지 않는데 왜 중요한가?
   A: 0단계는 전체 미들웨어의 계층 경계와 최종 산출물을 고정하는 단계이다. 여기서 HS-CAN baseline과 CAN-FD 확장 방향을 명확히 해 두면 이후 단계가 단편 예제로 흩어지지 않는다.

2. Q: HS-CAN 결과물과 CAN-FD 결과물은 완전히 별도 프로젝트로 만들어야 하는가?
   A: 권장 구조는 공통 계층을 공유하고 variant 설정만 분리하는 방식이다. Common, IL, TP, UDS, NM, CCL의 대부분은 공유하고 CAN Driver HAL, bit timing, payload 정책, message 설정만 분기한다.

3. Q: 통합 순서를 바꾸어 UDS부터 구현해도 되는가?
   A: 학습용으로 UDS 문법만 먼저 볼 수는 있지만, 실제 동작 미들웨어를 만들려면 아래 계층부터 올리는 순서가 안전하다. UDS는 ISO-TP, ISO-TP는 CAN Driver, CAN Driver는 설정과 공통 타입에 의존한다.

4. Q: 각 단계 산출물의 성공 여부는 무엇으로 판단하는가?
   A: 단순 빌드 성공이 아니라 보드에서 관찰 가능한 결과로 판단한다. 예를 들어 Driver 단계는 M_CAN register와 analyzer frame, IL 단계는 payload와 signal 값, TP 단계는 PCI/SN/FC 흐름으로 확인한다.

5. Q: 통합 중 가장 먼저 깨지는 부분은 보통 어디인가?
   A: 설정 계층과 Driver 계층의 경계가 가장 자주 문제를 만든다. M_CAN instance, Message RAM offset, interrupt vector, transceiver pin, bit timing 중 하나만 틀려도 상위 계층은 정상이어도 통신이 되지 않는다.

6. Q: CAN-FD를 위해 0단계에서 미리 결정해야 하는 것은 무엇인가?
   A: `dlc`와 `length` 분리, payload 최대 크기, FD/BRS message policy, Message RAM 크기 배분, ISO-TP over CAN-FD 정책을 미리 결정해야 한다. 이 결정을 미루면 10단계에서 구조 변경 폭이 커진다.

7. Q: 디렉터리 구조는 문서와 반드시 같아야 하는가?
   A: 이름은 프로젝트 상황에 맞게 조정할 수 있지만 계층 책임은 유지해야 한다. Driver가 UDS를 알거나 CCL이 M_CAN register를 직접 만지는 식의 역방향 의존은 피한다.

8. Q: 통합 로그는 어떤 형식으로 남기는 것이 좋은가?
   A: 단계, variant, bitrate, M_CAN instance, 주요 register dump, analyzer trace 파일명, pass/fail 판단을 한 줄로 남긴다. 나중에 CAN-FD 회귀 확인 시 HS-CAN baseline과 바로 비교할 수 있어야 한다.

9. Q: BusOff, CCL, UDS CommunicationControl은 왜 0단계부터 같이 고려하는가?
   A: 모두 통신 가능 상태를 제어하는 기능이기 때문이다. Driver만 online이어도 CCL이 Tx disable이면 송신이 막혀야 하고, BusOff 중에는 UDS 요청이 들어와도 정책에 맞게 제한되어야 한다.

10. Q: 최종 산출물의 “동작”은 어느 수준까지 의미하는가?
    A: HS-CAN은 주기 송신, 수신 callback, ISO-TP/UDS 기본 서비스, BusOff 복구, CCL 제어가 실제 bus에서 확인되는 수준을 뜻한다. CAN-FD는 여기에 FD frame, BRS on/off, 64바이트 payload, ISO-TP over FD 검증이 추가된다.

11. Q: 통합 후 문제가 생기면 어느 계층부터 확인해야 하는가?
    A: 물리 연결과 transceiver, M_CAN clock/pinmux, Driver register, CAN analyzer trace, 상위 계층 로그 순서로 올라간다. 상위 계층부터 추측하면 하드웨어 설정 오류를 놓치기 쉽다.
