# CBD 계층별 파일 상세 분석 보고서 인덱스

`src/CBD` 전체 파일을 계층별 Markdown 보고서로 분리 작성했다. 각 보고서는 파일별로 매크로, typedef/구조체, 전역 변수/테이블, 함수 목록, 함수별 역할 설명을 포함한다.

- [Can](Can.md): 3개 파일
- [Ccl](Ccl.md): 3개 파일
- [Common](Common.md): 1개 파일
- [Gen](Gen.md): 25개 파일
- [Il](Il.md): 4개 파일
- [Nm](Nm.md): 2개 파일
- [SipVersionCheck](SipVersionCheck.md): 2개 파일
- [Tp](Tp.md): 2개 파일
- [VStdLib](VStdLib.md): 2개 파일
- [Root_Remapping](Root_Remapping.md): 1개 파일

## CAN-FD 마이그레이션 참고

현재 계층별 분석은 HS-CAN/Classical CAN 기반 CBD 코드 구조를 기준으로 한다. CAN-FD로 전환할 때는 특히 `Can`, `Gen`, `Il`, `Tp`, `UDS/CANdesc` 관련 항목을 함께 봐야 한다.

- `Can`: FD frame flag, BRS, DLC-length 변환, 64바이트 payload buffer, FD-capable HAL 초기화 추가
- `Gen`: 메시지별 `is_fd`, `brs`, `length`, CAN-FD DLC 설정 추가
- `Il`: 8바이트 고정 payload 전제 제거, signal accessor 범위 검사 확장
- `Tp`: ISO-TP over CAN-FD Single/Consecutive Frame payload 계산 변경
- `Gen/desc`, `Gen/appdesc`: UDS response buffer, responseTooLong 기준, DID 응답 길이 재검토
- `Nm`, `Ccl`: FD frame도 기존 online/offline, BusOff, CommunicationControl 정책을 동일하게 따르는지 검증

상세 학습자료는 [../learning_steps/10_canfd_migration.md](../learning_steps/10_canfd_migration.md)를 참조한다.
