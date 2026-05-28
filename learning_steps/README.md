# CBD 미들웨어 단계별 학습 자료

이 폴더는 [loadmap.md](../loadmap.md)의 학습 단계를 기준으로 작성한 단계별 학습 자료이다. 마지막 단계는 기존 HS-CAN 구조를 CAN-FD 통신 스펙으로 마이그레이션하는 확장 학습이다.

각 파일은 다음 구성을 따른다.

- 이론 설명
- CBD 원본에서 확인할 파일과 관찰 포인트
- 학습용 C 예제 코드
- 연습문제
- 완료 기준

## 학습 순서

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
