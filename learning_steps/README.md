# CBD 미들웨어 단계별 학습 자료

이 폴더는 [loadmap.md](../loadmap.md)의 학습 단계를 기준으로 작성한 단계별 학습 자료이다. 학습용 재구현의 타깃 MCU는 STMicroelectronics `SPC584C70` MCU로 설정한다. 마지막 단계는 기존 HS-CAN 구조를 CAN-FD 통신 스펙으로 마이그레이션하는 확장 학습이다.

각 단계의 이론 설명은 `SPC584C70` 실보드 구현을 전제로 한다. MCU, 디버거, CAN/CAN-FD analyzer가 준비된 환경에서 `SPC584C70`의 M_CAN/CAN-FD controller, interrupt, memory section, peripheral clock, endian/정렬 조건에 맞춰 직접 구현하고 확인하는 흐름으로 진행한다.

각 파일은 다음 구성을 따른다.

- 이론 설명
- CBD 원본에서 확인할 파일과 관찰 포인트
- 학습용 C 예제 코드
- 단계 산출물
- 구현 가이드
- 적용 고려사항과 트러블슈팅
- 완료 기준

## 최종 결과물

이 학습자료의 최종 결과물은 아래 두 가지 동작 가능한 미들웨어이다.

1. `SPC584C70` MCU에서 HS-CAN/Classical CAN 통신이 가능한 미들웨어
2. `SPC584C70` MCU에서 CAN-FD 통신이 가능한 미들웨어

1~9단계 결과물을 통합하면 HS-CAN 미들웨어 baseline이 된다. 10단계 결과물을 적용하면 같은 구조를 CAN-FD variant로 확장한다. 각 단계는 반드시 코드/설정/테스트 산출물을 남겨 다음 단계에서 재사용해야 한다.

## 학습 순서

0. [00_integration_deliverables.md](00_integration_deliverables.md): 최종 산출물, 통합 전략, 공통 트러블슈팅
1. [01_common_vstdlib.md](01_common_vstdlib.md): 공통 타입과 VStdLib
2. [02_generated_config.md](02_generated_config.md): 생성 설정 구조
3. [03_can_data_model.md](03_can_data_model.md): CAN 메시지 데이터 모델
4. [04_can_driver.md](04_can_driver.md): CAN Driver
5. [05_interaction_layer.md](05_interaction_layer.md): IL, Interaction Layer
6. [06_iso_tp.md](06_iso_tp.md): ISO-TP
7. [07_uds_candesc.md](07_uds_candesc.md): UDS / CANdesc
8. [08_nm_busoff.md](08_nm_busoff.md): NM / BusOff 복구
9. [09_ccl_integration.md](09_ccl_integration.md): CCL / 통신 통합 제어
10. [10_canfd_migration.md](10_canfd_migration.md): HS-CAN에서 CAN-FD로 마이그레이션

권장 방식은 각 단계의 이론을 읽은 뒤 구현 가이드를 따라 `SPC584C70` 프로젝트에 산출물을 직접 추가하고, 디버거와 CAN 장비로 완료 기준을 확인한 뒤 다음 단계로 넘어가는 것이다.

## Q&A

1. Q: 이 학습 자료는 단순히 CBD 원본 코드를 해석하는 자료인가?
   A: 아니다. 원본 CBD 구조를 분석해 계층별 책임을 이해하고, 같은 개념을 학습용 C 미들웨어로 재구현하는 실습 자료이다. 최종 목표는 HS-CAN baseline과 CAN-FD variant가 실제 `SPC584C70` 보드에서 동작하는 것이다.

2. Q: 왜 1단계부터 바로 CAN Driver를 만들지 않고 Common 계층부터 시작하는가?
   A: CAN Driver, IL, TP, UDS는 모두 동일한 타입, 메모리 클래스, interrupt 보호 규칙을 공유한다. 이 기반이 흔들리면 이후 계층에서 같은 오류가 반복되므로 먼저 공통 계약을 고정한다.

3. Q: 각 단계 산출물은 독립 프로그램인가?
   A: 아니다. 각 단계 산출물은 다음 단계에서 재사용될 모듈이다. 예를 들어 3단계의 CAN 데이터 모델은 4단계 Driver 송신 테이블이 되고, 4단계 Driver callback은 5단계 IL과 6단계 ISO-TP의 입력이 된다.

4. Q: 왜 보드 중심 검증으로 진행하는가?
   A: 현재 실습 환경에는 `SPC584C70` MCU, 디버거, CAN/CAN-FD analyzer가 준비되어 있다. CAN clock, M_CAN Message RAM, interrupt routing, transceiver 제어는 실제 보드와 계측 장비에서 확인해야 의미가 있으므로 보드 중심으로 진행한다.

5. Q: HS-CAN과 Classical CAN은 같은 의미로 보아도 되는가?
   A: 이 자료에서 HS-CAN 결과물은 Classical CAN frame format을 사용하는 high-speed CAN bus 미들웨어를 뜻한다. CAN-FD 단계에서는 같은 bus 계층을 확장하되 FD frame, BRS, 64바이트 payload를 추가로 고려한다.

6. Q: CAN-FD는 10단계에서만 보면 되는가?
   A: 구현은 10단계에서 본격화하지만, 1단계부터 9단계까지는 CAN-FD 확장을 막지 않는 구조로 작성해야 한다. 특히 `data[8]` 고정, DLC와 length 혼동, 8바이트 전용 복사 함수는 초기에 피한다.

7. Q: SPC584C70과 SPC58EC70 설명이 함께 보이면 어떻게 해석해야 하는가?
   A: CAN-FD/M_CAN 주변장치 관점에서는 같은 family-level 설명을 참고할 수 있지만, 프로젝트 타깃은 `SPC584C70`이다. startup, linker, core 수, interrupt target, RAM map은 반드시 `SPC584C70` 기준으로 검증한다.

8. Q: 학습 중 언제 CAN analyzer trace를 저장해야 하는가?
   A: Driver bring-up, IL 주기 송신, ISO-TP multi-frame, UDS 응답, BusOff 복구, CAN-FD BRS on/off 시점마다 저장한다. 이 trace는 다음 단계 통합과 CAN-FD 회귀 비교의 기준 자료가 된다.

9. Q: 문서의 예제 코드를 그대로 제품 코드로 사용해도 되는가?
   A: 아니다. 예제는 계층 구조와 데이터 흐름을 이해하기 위한 최소 코드이다. 실제 ECU 수준에서는 timing, interrupt nesting, error handling, memory protection, MISRA-C, watchdog, bootloader 연계 등을 추가로 검토해야 한다.

10. Q: 최종적으로 어떤 파일 구조가 만들어져야 하는가?
    A: `middleware/common`, `middleware/config`, `middleware/can`, `middleware/il`, `middleware/tp`, `middleware/diag`, `middleware/nm`, `middleware/ccl`처럼 계층별 책임이 분리된 구조가 되어야 한다. `examples/hs_can_middleware`와 `examples/canfd_middleware`는 이 계층을 통합해 보드에서 실행하는 진입점이 된다.

11. Q: 단계 완료 기준은 문서 체크만으로 충족되는가?
    A: 아니다. 완료 기준은 실제 `SPC584C70` 보드에서 디버거 관찰값, register dump, CAN/CAN-FD analyzer trace로 확인되어야 한다. 문서 이해와 보드 관찰이 함께 맞아야 다음 단계로 넘어간다.
