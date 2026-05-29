# CBD 미들웨어 단계별 학습 자료

이 폴더는 [loadmap.md](../loadmap.md)의 학습 단계를 기준으로 작성한 단계별 학습 자료이다. 학습용 재구현의 타깃 MCU는 STMicroelectronics `SPC584C70` MCU로 설정한다. 마지막 단계는 기존 HS-CAN 구조를 CAN-FD 통신 스펙으로 마이그레이션하는 확장 학습이다.

각 단계의 이론 설명은 PC mock 구현과 `SPC584C70` 적용 관점을 함께 다룬다. 즉, 처음에는 표준 C와 간단한 mock으로 동작 모델을 만들고, 이후 `SPC584C70`의 M_CAN/CAN-FD controller, interrupt, memory section, peripheral clock, endian/정렬 조건에 맞춰 이식하는 흐름을 전제로 한다.

각 파일은 다음 구성을 따른다.

- 이론 설명
- CBD 원본에서 확인할 파일과 관찰 포인트
- 학습용 C 예제 코드
- 연습문제
- 단계 산출물
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

권장 방식은 각 단계의 예제 코드를 별도 학습용 프로젝트에 직접 입력해 보고, 연습문제를 통과한 뒤 다음 단계로 넘어가는 것이다.
