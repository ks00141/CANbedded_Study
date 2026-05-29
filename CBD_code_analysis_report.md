# src/CBD 코드 분석 보고서

## 1. 분석 대상 및 결론

`src/CBD` 폴더는 현대/기아(HKMC) IPSU 노드용으로 생성된 Vector CANbedded 기반 CAN 통신 스택입니다. 단일 애플리케이션 로직이라기보다, CAN 드라이버, 인터랙션 레이어(IL), 네트워크 관리(NM), ISO-TP 전송 계층(TP), UDS 진단(CANdesc), 통신 제어(CCL), 공통 타입/라이브러리, 그리고 GENy/CANdelaGen 생성 설정을 포함하는 임베디드 미들웨어 묶음입니다.

주요 목적은 다음과 같습니다.

- MPC5515 / STM SPC56x 계열 MCU에서 FlexCAN 기반 CAN 통신을 초기화하고 송수신한다.
- IPSU 노드의 주기 메시지와 수신 메시지를 DBC 기반 구조체/버퍼로 제공한다.
- ISO 15765-2 기반 진단 TP와 UDS 진단 서비스 처리를 제공한다.
- 버스오프 복구, 통신 요청/해제, 진단 중 통신 제어 같은 차량 네트워크 상태 관리를 수행한다.

## 2. 생성 정보와 대상 환경

여러 `Gen/*.h` 파일의 헤더가 동일한 생성 정보를 담고 있습니다.

- 생성 도구: Vector GENy, CANdelaGen
- 생성 일시: 2026-05-19 13:26:16
- 고객/프로젝트 정보: STMicroelectronics Application GmbH, CBD Hyundai Kia Motors / HKMC
- MCU/타깃: STM SPC56x / SPC5604B 계열, TargetSystem `Hw_Mpc55xx_Flexcan2Cpu`, Derivate `MPC5515`
- 컴파일러: Green Hills Compiler
- CAN DB: `251001_PRE_Premium_IPSU_CANDB_REV06.dbc`
- 노드: `IPSU`
- GENy 설정 파일: `PRE_Premium_IPSU_R02_260519.gny`

따라서 이 코드는 PC용 일반 C 라이브러리가 아니라, 특정 차량 ECU와 특정 CAN DB에 맞춰 생성된 양산형 임베디드 통신 소프트웨어로 보는 것이 타당합니다.

### 재구현 학습 타깃 MCU

본 학습 문서 세트에서 재구현 대상으로 삼는 MCU는 STMicroelectronics `SPC58EC70` MCU입니다. 기존 CBD 원본 생성 정보에는 `MPC5515`/`SPC56x` 계열 설정이 보이지만, 학습용 재구현은 `SPC58EC70`을 기준으로 확장합니다.

`SPC58EC70` 적용 시 특히 다음을 고려해야 합니다.

- e200z420n3 듀얼 코어 Power Architecture automotive MCU 특성에 따른 endian, 정렬, interrupt ABI, VLE 컴파일 옵션
- 8개 Bosch M_CAN 인터페이스와 shared Message RAM 기반 ISO CAN-FD 구조
- CAN-FD 사용 시 arbitration/data phase bitrate 설정
- CAN message RAM 또는 mailbox payload size 설정
- flash/RAM section 배치를 위한 메모리 클래스 매크로 연결
- BusOff/error passive/error warning interrupt 처리
- transceiver 또는 SBC 제어가 필요한 경우 CCL/NM과 board support 계층의 연결

따라서 PC 학습용 구현에서는 mock CAN HAL로 시작하되, 구조체와 설정 테이블은 `SPC58EC70`의 CAN-FD 포팅을 수용할 수 있도록 설계하는 것이 좋습니다.

공식 ST 자료 기준으로 `SPC58EC70E3`/SPC58ECx 계열은 최대 180 MHz e200z420n3 dual core, HSM 내부 e200z0 코어, 8개 M_CAN ISO CAN-FD 인터페이스를 제공하는 것으로 정리합니다. Flash/RAM 용량은 주문 코드와 Flash option에 따라 달라질 수 있으므로, 실제 포팅 프로젝트에서는 DS11620 ordering information과 보드 BOM의 정확한 part number를 기준으로 확정해야 합니다.

## 3. 전체 폴더 구조와 역할

| 폴더/파일 | 역할 |
|---|---|
| `Can/` | FlexCAN 계열 CAN 드라이버 구현. 초기화, 송신, 수신, 인터럽트, 버스오프/웨이크업, 하드웨어 오브젝트 처리를 담당합니다. |
| `Il/` | Vector Interaction Layer. DBC 기반 신호/메시지의 주기 송신, 이벤트 송신, 수신 콜백, CAN 핸들 매핑을 담당합니다. |
| `Nm/` | Basic Network Management. 버스오프 감지/복구와 네트워크 상태 관리를 담당합니다. |
| `Tp/` | ISO 15765-2 Transport Protocol. UDS 진단용 다중 프레임 송수신, Flow Control, STmin/Block Size, 타임아웃 처리를 담당합니다. |
| `Ccl/` | CAN Communication Layer. 네트워크 요청/해제, 통신 시작/정지/대기/재개, 진단 요청에 따른 통신 상태 제어를 담당합니다. |
| `VStdLib/` | Vector 공통 라이브러리. 메모리 복사/초기화, 인터럽트 락/복구 같은 기반 함수를 제공합니다. |
| `Common/` | Vector 기본 타입, 메모리 클래스, 비트 타입, 포인터 타입 정의를 제공합니다. |
| `Gen/` | 프로젝트별 생성 코드와 설정. CAN ID/핸들, IL 파라미터, TP/NM/CCL 설정, UDS 서비스 테이블, 진단 애플리케이션 훅을 포함합니다. |
| `SipVersionCheck/` | 포함된 SIP/모듈 버전 정합성 확인용 코드입니다. |
| `RemapChannelSpc58.h` | SPC58 계열 채널/레지스터 리맵 관련 보조 헤더로 보입니다. |

## 4. 통신 스택 관점의 계층 구조

코드의 계층은 대략 아래와 같이 읽을 수 있습니다.

```text
Application / ECU logic
  |
  |-- appdesc.c / appdesc.h: UDS 서비스별 애플리케이션 훅
  |
CANdesc / Desc
  |
  |-- desc.c / desc.h: UDS 디스패처, 세션, 보안 접근, DID/RID 처리
  |
ISO-TP
  |
  |-- tpmc.c / tpmc.h: ISO 15765-2 TP 송수신 상태 머신
  |
CAN Interaction Layer
  |
  |-- il.c / il_def.h / il_par.c: 주기/이벤트 메시지 관리
  |
CAN Driver / CCL / NM
  |
  |-- can_drv.c: FlexCAN 하드웨어 제어
  |-- ccl.c: 통신 상태 제어
  |-- nm_basic.c: 버스오프 및 네트워크 관리
  |
MCU / FlexCAN hardware
```

`Gen/v_cfg.h`에서 활성 모듈이 `VGEN_ENABLE_DIAG_CANDESC_UDS`, `VGEN_ENABLE_CCL`, `VGEN_ENABLE_CAN_DRV`, `VGEN_ENABLE_IL_VECTOR`, `VGEN_ENABLE_NM_BASIC`, `VGEN_ENABLE_TP_ISO_MC`로 정의되어 있어 위 계층 구성이 명확합니다.

## 5. CAN 드라이버 분석

`Can/can_drv.c`, `Can/can_def.h`, `Gen/can_cfg.h`, `Gen/can_par.*`가 CAN 드라이버의 핵심입니다.

주요 특징은 다음과 같습니다.

- CAN 채널 수는 논리/하드웨어 채널 1개입니다.
- 물리 채널 수는 3개로 설정되어 있으나 실제 사용 채널은 `CAN` 1개입니다.
- 확장 ID는 비활성화되어 있고, 표준 ID 중심 구성입니다.
- 송신 큐가 활성화되어 있습니다.
- 송신 오브젝트는 16개, 수신 오브젝트는 7개입니다.
- FullCAN 수신 오브젝트 6개와 BasicCAN 수신 오브젝트 1개가 설정되어 있습니다.
- 수신은 FullCAN 인터럽트와 BasicCAN 폴링이 혼재된 형태입니다.
- BusOff 콜백은 `NmBasicCanBusOff`로 연결됩니다.
- Generic Precopy는 `IlCanGenericPrecopy`로 연결되어 수신 메시지가 IL/TP 계층으로 전달됩니다.

`Gen/can_par.h` 기준 송신 핸들은 `CanTxIPSU_GST`, `CanTxDPSS_02_200ms`, `CanTxIPSU_14_200ms`부터 `CanTxIPSU_01_200ms`까지 16개가 정의되어 있습니다. 수신 핸들은 `CanRxCGW_05_200ms`부터 `CanRxCGW_01_200ms`, `CanRxGST_ALL`, `CanRxGST_IPSU`까지 7개입니다.

즉 CAN 드라이버 계층은 IPSU ECU가 CGW/진단 프레임을 수신하고, IPSU 상태/위치/진단 응답 프레임을 송신할 수 있게 하는 하드웨어 추상화 계층입니다.

## 6. IL(Interaction Layer) 분석

`Il/il.c`, `Il/il_def.h`, `Gen/il_cfg.h`, `Gen/il_par.*`, `Gen/drv_par.*`가 IL 계층입니다.

주요 설정은 다음과 같습니다.

- TX/RX 모두 활성화되어 있습니다.
- TX는 polling 방식, RX는 interrupt 방식입니다.
- TX 주기는 10ms 기준이며, 실제 메시지 이름에는 다수의 `200ms` 주기 메시지가 나타납니다.
- TX 오브젝트 15개, RX 오브젝트 5개입니다.
- TX timeout, RX timeout, indication flag, confirmation flag 등은 대부분 비활성화되어 있습니다.
- OEM 타입은 `HMC`입니다.

`Gen/drv_par.h`는 DBC에서 생성된 신호 구조체를 제공합니다. 예를 들면:

- `IPSU_01_200ms`: Recline 현재/목표 위치
- `IPSU_04_200ms`: Slide 현재/목표 위치
- `IPSU_07_200ms`: Height 현재/목표 위치
- `IPSU_10_200ms`: Tilt 현재/목표 위치
- `IPSU_14_200ms`: Tilt/Height/Slide/Recline 제한 설정 상태
- `CGW_01_200ms`~`CGW_05_200ms`: CGW에서 내려오는 제한값 설정/해제 명령
- `DPSS_02_200ms`: 시트 스위치/럼버/볼스터/쿠션 관련 입력

따라서 IL은 DBC 메시지를 C 구조체와 CAN 송수신 핸들로 연결해 애플리케이션이 바이트 배열을 직접 다루지 않아도 되게 하는 계층입니다.

## 7. TP(ISO 15765-2) 분석

`Tp/tpmc.c`, `Tp/tpmc.h`, `Gen/tp_cfg.h`, `Gen/tp_par.c`가 진단 전송 계층입니다.

주요 설정은 다음과 같습니다.

- ISO 15765-2 TP가 활성화되어 있습니다.
- 단일 CAN 채널, 단일 TX/RX TP 채널 구성입니다.
- Normal Addressing만 사용합니다.
- Extended, Mixed 11/29, Normal Fixed Addressing은 비활성화되어 있습니다.
- Padding 사용, 패턴은 `0xAA`입니다.
- STmin은 3, TX/RX confirmation timeout은 11, FC/CF timeout은 17로 설정되어 있습니다.
- Functional request 수신도 활성화되어 있고, Functional request는 `DescGetFuncBuffer`, `DescFuncReqInd`로 연결됩니다.
- Physical request는 `DescGetBuffer`, `DescPhysReqInd`로 연결됩니다.
- TP 송신 핸들은 `CanTxIPSU_GST`, 송신 버퍼는 `IPSU_GST._c`에 연결됩니다.

즉 TP 계층은 진단 요청/응답이 8바이트 CAN 프레임을 넘을 때 ISO-TP 규칙에 따라 First Frame, Consecutive Frame, Flow Control을 처리하고, 상위 CANdesc 진단 계층과 연결합니다.

## 8. UDS/CANdesc 진단 분석

`Gen/desc.c`, `Gen/desc.h`, `Gen/appdesc.c`, `Gen/appdesc.h`, `Gen/appdescdev.c`가 UDS 진단 계층입니다.

`desc.c`는 생성된 진단 디스패처입니다. 주요 기능은 다음과 같습니다.

- 진단 요청 수신 컨텍스트 관리
- 서비스 ID와 서브 함수 매칭
- UDS 세션 상태 관리
- SecurityAccess seed/key 상태 관리
- DID(ReadDataByIdentifier) 처리
- RID(RoutineControl) 처리
- TesterPresent, CommunicationControl, ControlDTCSetting 처리
- 긍정/부정 응답 생성
- RCR-RP(Response Pending) 및 P2/P2* 타이밍 관리

지원 서비스로 확인되는 항목은 다음과 같습니다.

- `0x10`: DiagnosticSessionControl, Default/Programming/Extended
- `0x11`: ECUReset Hard
- `0x14`: ClearDiagnosticInformation
- `0x19`: ReadDTCInformation 일부 서브 기능
- `0x22`: ReadDataByIdentifier
- `0x27`: SecurityAccess, Level 1 seed/key
- `0x28`: CommunicationControl
- `0x2F`: InputOutputControlByIdentifier
- `0x31`: RoutineControl
- `0x3E`: TesterPresent
- `0x85`: ControlDTCSetting

`appdesc.c`는 CANdesc가 호출하는 애플리케이션 훅 예제/구현부입니다. 현재 코드에는 다음 성격의 함수들이 생성되어 있습니다.

- 세션 전환 알림: `ApplDescOnTransitionSession`
- SecurityAccess seed/key: `ApplDescSecurityAccessGetSeed`, `ApplDescSecurityAccessCheckKey`
- 통신 제어 검증/적용: `ApplDescCheckCommCtrl`, `ApplDescSetCommMode`
- DID 읽기: `F187`, `F18B`, `F18C`, `F191`, `F193`, `F1A1`, `F1B0`, `F1B1`, `F1C1`, `F1EF` 등
- IO Control: `F010`, `F01C`, `F01D`, `F040`, `F041`, `F042`, `F049`, `F04A`, `F04D`, `F04E`, `F04F`, `F050`
- RoutineControl: OTA Ready, Limit Setting (`0x12A7`)

주의할 점은 `appdesc.c` 헤더에 "Implementation example"이라고 되어 있고, 여러 함수 본문이 `DescProcessingDone` 또는 부정 응답 처리 중심의 기본 스텁 형태일 가능성이 높다는 점입니다. 실제 양산 기능은 이 훅 내부에서 별도 애플리케이션 모듈과 연결되어야 합니다.

## 9. NM(Network Management) 분석

`Nm/nm_basic.c`, `Nm/nm_basic.h`, `Gen/nmb_cfg.h`, `Gen/nmb_par.*`가 Basic NM 계층입니다.

주요 설정은 다음과 같습니다.

- 채널 수 1개
- Basic NM 타입
- Task period 10ms
- BusOff 복구 시간: 빠른 복구 50, 느린 복구 100
- Fast에서 slow 복구로 전환되는 기준: 450
- BusOff repaired time: 2000
- BusOff report message 활성화
- 외부 CAN online handling, indexed NM, TX observation 등은 비활성화

CAN 드라이버의 BusOff 콜백이 NM으로 연결되어 있으므로, CAN 버스오프 발생 시 NM이 복구 절차를 주도하는 구조입니다.

## 10. CCL(Communication Control Layer) 분석

`Ccl/ccl.c`, `Ccl/ccl.h`, `Ccl/ccl_inc.h`, `Gen/ccl_cfg.h`, `Gen/ccl_par.*`가 CCL 계층입니다.

주요 특징은 다음과 같습니다.

- CANbedded handling 활성화
- 내부 통신 요청 활성화, 외부 요청 비활성화
- Container task 방식 사용
- System shutdown 지원
- Software communication state 지원
- 네트워크/채널 수 1개
- 사용자 핸들 2개: `CCL_CommRequest`, `CCL_DiagReq_0`
- `CclSet_*`, `CclRel_*` 매크로로 통신 요청/해제 가능
- Appl callback: `ApplCclComStart`, `ApplCclComStop`, `ApplCclComWait`, `ApplCclComResume`

진단 서비스 `0x28 CommunicationControl`과도 연결되어, 진단 요청에 따라 CAN/IL/NM/TP 계층의 송수신 상태를 제어하는 역할을 합니다.

## 11. 공통 라이브러리와 타입 계층

`Common/v_def.h`는 Vector 계열 기본 타입과 메모리 모델 매크로를 제공합니다.

- `vuint8`, `vuint16`, `vuint32`, `vsint*`
- `TxDataPtr`, `RxDataPtr`
- 비트 필드 구조체
- ROM/RAM/near/far 메모리 클래스 매크로

`VStdLib/vstdlib.c`는 다음 기반 기능을 제공합니다.

- `VStdMemSetInternal`, `VStdMemClrInternal`
- `VStdRamMemCpy`, `VStdRomMemCpy`, 16/32비트 복사
- `VStdSuspendAllInterrupts`, `VStdResumeAllInterrupts`
- Global interrupt disable/restore

이 계층은 다양한 컴파일러/MCU 메모리 모델 차이를 흡수하기 위한 Vector 공통 기반입니다.

## 12. 데이터 흐름 요약

수신 흐름:

```text
FlexCAN HW interrupt/polling
  -> Can/can_drv.c
  -> Generic Precopy 또는 Rx handle 처리
  -> IL 수신 메시지 버퍼 또는 TP Precopy
  -> ISO-TP 재조립
  -> CANdesc UDS dispatcher
  -> appdesc.c 서비스별 애플리케이션 훅
```

송신 흐름:

```text
Application updates generated signal buffers
  -> IL Tx task 또는 TP response
  -> CanTransmit / CanMsgTransmit
  -> CAN driver TX queue / HW object
  -> FlexCAN HW transmission
  -> confirmation callback / flag
```

진단 흐름:

```text
Tester request
  -> CAN Rx GST_ALL/GST_IPSU
  -> ISO-TP physical/functional indication
  -> DescGetBuffer / DescPhysReqInd / DescFuncReqInd
  -> desc.c service lookup
  -> appdesc.c callback
  -> DescProcessingDone / negative response
  -> ISO-TP response
  -> CanTxIPSU_GST
```

## 13. 이 코드가 수행하는 업무적 의미

메시지 이름과 DID/IO Control 이름을 보면 이 노드는 전동 시트 계열 ECU, 특히 IPSU 관련 기능을 대상으로 합니다. 코드상 관찰되는 업무 도메인은 다음과 같습니다.

- Recline, Slide, Height, Tilt 위치/목표 위치 송신
- Recline/Slide/Height/Tilt 최소/최대 위치 및 virtual limit 값 송수신
- Footrest 상태 송신
- Heater/Vent/Seat movement 관련 IO Control
- Power seat switch/motor/position/fail detection DID 읽기
- OTA Ready 및 Limit Setting RoutineControl
- HKMC UDS 진단 세션/보안/통신 제어

따라서 `src/CBD`는 “IPSU ECU의 CAN 통신 및 진단 통신을 가능하게 하는 Vector CANbedded 생성 코드”라고 요약할 수 있습니다.

## 14. 유지보수 시 주의사항

1. `Gen/` 파일은 생성 산출물입니다. 수동 수정하면 다음 GENy/CANdelaGen 재생성 시 덮어쓰기될 수 있습니다.
2. `appdesc.c`는 애플리케이션 훅 성격이 강하지만, 파일 헤더상 예제 구현 코드로 분류됩니다. 실제 제품 기능 연결부인지, 생성 후 프로젝트에서 수정 관리하는 파일인지 확인이 필요합니다.
3. `Magic Number` 검사가 여러 생성 파일에 들어 있어, 서로 다른 시점에 생성된 파일을 섞으면 컴파일 에러가 발생할 수 있습니다.
4. CAN ID, 핸들, 신호 구조체는 DBC와 GENy 설정에 종속됩니다. DBC 변경 시 수동 수정이 아니라 생성 도구 재실행이 바람직합니다.
5. 확장 ID, 다중 채널, 다중 TP 채널, 동적 TX/RX 오브젝트 등은 대부분 비활성화되어 있으므로, 네트워크 확장 요구가 생기면 설정 변경과 전체 재검증이 필요합니다.
6. TP와 UDS는 타이밍, padding, functional/physical addressing이 명확히 설정되어 있으므로, 진단기 호환성 이슈는 `tp_cfg.h`, `desc.c`, `appdesc.c`를 함께 봐야 합니다.

## 15. 핵심 파일별 빠른 참조

- `src/CBD/Gen/v_cfg.h`: 전체 활성 모듈, 타깃 MCU/컴파일러/생성 정보
- `src/CBD/Gen/can_cfg.h`: CAN 드라이버 기능 스위치와 오브젝트 수
- `src/CBD/Gen/can_par.h`, `src/CBD/Gen/can_par.c`: CAN 송수신 핸들, ID, DLC, 버퍼 포인터
- `src/CBD/Gen/drv_par.h`, `src/CBD/Gen/drv_par.c`: DBC 기반 메시지/신호 버퍼 구조체
- `src/CBD/Gen/il_cfg.h`, `src/CBD/Gen/il_par.*`: IL 주기/이벤트 송수신 설정
- `src/CBD/Gen/tp_cfg.h`: ISO-TP 주소 방식, 타임아웃, 콜백 연결
- `src/CBD/Gen/desc.c`: UDS 서비스 테이블과 디스패처
- `src/CBD/Gen/appdesc.c`: UDS 서비스별 애플리케이션 콜백
- `src/CBD/Can/can_drv.c`: FlexCAN 드라이버 본체
- `src/CBD/Tp/tpmc.c`: ISO-TP 상태 머신
- `src/CBD/Nm/nm_basic.c`: Basic NM 및 BusOff 복구
- `src/CBD/Ccl/ccl.c`: 통신 상태/요청 관리

## 16. 최종 요약

`src/CBD`는 IPSU ECU용 Vector CANbedded 통신 스택입니다. 이 폴더의 코드는 CAN 하드웨어 드라이버부터 IL, NM, CCL, ISO-TP, UDS 진단까지 차량 CAN 통신에 필요한 거의 모든 기반 계층을 포함합니다. 프로젝트별 CAN DB와 진단 CDD/GENy 설정으로부터 생성되었으며, 애플리케이션은 이 코드가 제공하는 신호 버퍼와 진단 콜백을 통해 실제 시트 제어/상태 보고/진단 서비스를 구현하는 구조입니다.

## 17. CAN-FD 마이그레이션 관점

현재 구조는 HS-CAN, 즉 Classical CAN 8바이트 payload를 전제로 작성되어 있습니다. CAN-FD로 마이그레이션하려면 CAN 드라이버만 바꾸는 것으로 충분하지 않고, 생성 설정, 메시지 데이터 모델, IL 신호 버퍼, ISO-TP, UDS 응답 크기 정책까지 함께 확장해야 합니다.

### 핵심 변경 포인트

| 계층 | CAN-FD 전환 시 확인할 내용 |
|---|---|
| Common / VStdLib | payload length는 64까지 증가하지만 TP 전체 메시지 길이는 `vuint16` 이상 유지가 필요합니다. |
| Gen 설정 | `CAN-FD enable`, `BRS`, `payload length`, `FD frame 여부`를 전역/메시지별 설정으로 추가해야 합니다. |
| CAN Data Model | 기존 `DLC == length`, `data[8]` 전제를 제거하고 `is_fd`, `brs`, `dlc`, `length`, `data[64]` 또는 pointer+length 구조로 확장해야 합니다. |
| CAN Driver | FD-capable controller 초기화, data phase bitrate, FD mailbox payload size, FD DLC 변환, BRS/ESI 처리가 필요합니다. |
| IL | 신호 accessor가 8바이트 메시지에 묶이지 않도록 buffer 크기와 범위 검사를 확장해야 합니다. |
| ISO-TP | CAN-FD Single Frame/Consecutive Frame payload 크기가 커지므로 segmentation 정책과 timeout/Block Size를 재검토해야 합니다. |
| UDS / CANdesc | 서비스 로직은 유지하되 response buffer 크기, `responseTooLong` 기준, DID 응답 길이 정책을 확장해야 합니다. |
| NM / CCL | BusOff/error state와 online/offline 제어가 CAN-FD frame에도 동일하게 적용되는지 확인해야 합니다. |

### 권장 마이그레이션 순서

1. 기존 HS-CAN 동작을 테스트로 고정합니다.
2. CAN frame 구조체를 Classical/FD 공용 구조로 확장합니다.
3. CAN-FD DLC 변환 함수를 추가합니다.
4. CAN 설정 테이블에 메시지별 `is_fd`, `brs`, `length`를 추가합니다.
5. CAN Driver/HAL을 FD frame 송수신 가능 구조로 바꿉니다.
6. IL buffer와 signal accessor의 8바이트 제한을 제거합니다.
7. ISO-TP over CAN-FD payload 계산을 반영합니다.
8. UDS response buffer와 응답 길이 제한을 재검토합니다.
9. NM/CCL 통신 제어가 Classical/FD 송수신 모두에 적용되는지 검증합니다.

자세한 학습자료는 [learning_steps/10_canfd_migration.md](learning_steps/10_canfd_migration.md)를 참조하면 됩니다.
