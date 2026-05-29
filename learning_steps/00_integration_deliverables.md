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
| 4 | HS-CAN Driver, mock HAL, SPC584C70 HAL adapter interface | IL/TP/NM/CCL의 하위 송수신 API |
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
    can_hal_mock.c
    can_hal_spc584c70.c
    can_par.c
  il/
  isotp/
  uds/
  nm/
  ccl/
  app/
tests/
  unit/
  integration/
  vectors/
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
- HS-CAN 회귀 테스트가 CAN-FD 변경 후에도 통과한다.

## 통합 시 적용 고려사항

- `SPC584C70`은 8개 M_CAN 기반 ISO CAN-FD 인터페이스를 제공하므로 HAL adapter는 M_CAN instance, shared Message RAM offset, interrupt line을 설정으로 받는다.
- `SPC58EC70`과 CAN-FD 주변장치 설명은 유사하지만, `SPC584C70`의 single computing core 전제에 맞춰 startup, linker, RAM map, interrupt target을 별도로 검증한다.
- peripheral clock과 CAN bit timing은 코드에 하드코딩하지 않고 설정 구조체로 관리한다.
- ISR과 task 사이의 공유 buffer는 lock, queue, double buffer 중 하나로 보호한다.
- UDS response buffer는 CAN-FD 전환 후 커질 수 있지만, RAM 사용량을 반드시 계산한다.
- CAN-FD BRS를 켤 때는 transceiver와 네트워크 전체가 data phase bitrate를 지원해야 한다.
- 모든 단계 산출물은 PC mock test와 MCU bring-up test를 분리한다.

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

1. PC mock에서 unit test를 모두 통과시킨다.
2. mock CAN loopback으로 Driver, IL, ISO-TP, UDS를 통합한다.
3. SPC584C70 보드에서 CAN controller loopback을 확인한다.
4. CAN analyzer로 HS-CAN frame 송수신을 확인한다.
5. BusOff와 CCL online/offline 정책을 확인한다.
6. CAN-FD controller 설정과 FD payload length를 확인한다.
7. ISO-TP over CAN-FD와 UDS 응답을 확인한다.
8. HS-CAN 회귀 테스트를 다시 실행한다.
