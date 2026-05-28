# Ccl 계층 파일별 상세 분석 보고서

- 분석 대상: `Ccl` 계층
- 파일 수: 3
- 생성 시각: 2026-05-28 22:37:38
- 분석 방식: C 전처리 매크로, typedef, 전역 데이터 선언, 함수 선언/정의를 자동 추출하고 함수명/주석/계층 역할을 기준으로 한글 역할 설명을 부여함.

## 계층 요약

CCL 계층은 ECU 통신 요청/해제, 통신 모드 전환, 버스오프 시작/종료 연동, 진단 통신 제어와 통합된 네트워크 상태 제어를 담당한다.

## 파일 목록

- `src/CBD/Ccl/ccl.c` (117,154 bytes)
- `src/CBD/Ccl/ccl.h` (22,057 bytes)
- `src/CBD/Ccl/ccl_inc.h` (42,479 bytes)

## 파일이름 : ccl

- 경로: `src/CBD/Ccl/ccl.c`
- 파일 종류: `.c`
- 파일 크기: 117,154 bytes
- 파일 내용: Communication Control Layer 구현으로 통신 요청, 상태 전환, 버스오프 연동, 콜백을 처리한다.

### 1. .c/.h 파일 이름

- `ccl.c`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 11
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 35

#### 주요 매크로
- L287 `CCL_MODULE`
- L357 `CAN_CHANNEL_CANTYPE_ONLY` = `void`
- L362 `cclExtWakeUpReq` = `cclExtCanWakeUpReq`
- L364 `CclExtComReqFctTbl` = `CclExtCanComReqFctTbl`
- L366 `CclExtComReqFct` = `CclExtCanComReqFct`
- L370 `CCL_ENABLE_CANBEDDED_HANDLING`
- L375 `CanGlobalInterruptDisable` = `CanInterruptDisable`
- L376 `CanGlobalInterruptRestore` = `CanInterruptRestore`
- L384 `DRV_API_CALLBACK_TYPE`
- L392 `CanResetWakeup(a)` = `((void)CanWakeUp((a)))`
- L394 `CanResetWakeup()` = `((void)CanWakeUp())`

#### 주요 typedef/구조체
- 없음

#### 주요 전역 변수/테이블 선언
- L408 `V_MEMRAM0 static V_MEMRAM1 vuint8 V_MEMRAM2 cclExtCanWakeUpReq[kCclNrOfChannels];`
- L419 `V_MEMRAM0 static V_MEMRAM1 vuint8 V_MEMRAM2 cclCanSleepRepetition[kCclNrOfChannels];`
- L428 `V_MEMRAM0 static V_MEMRAM1 vuint8 V_MEMRAM1 cclCanSleepReturnCode[kCclNrOfChannels];`
- L439 `V_MEMRAM0 static V_MEMRAM1 vuint8 V_MEMRAM2 cclIntNetReq[kCclNrOfNetworks];`
- L446 `V_MEMRAM0 static V_MEMRAM1 vuint8 V_MEMRAM2 cclIntNetRel[kCclNrOfNetworks];`
- L453 `V_MEMRAM0 static V_MEMRAM1 vuint8 V_MEMRAM2 cclIntNetRelHistory[kCclNrOfNetworks];`
- L463 `V_MEMRAM0 static V_MEMRAM1 vuint8 V_MEMRAM2 cclComHwState[kCclNrOfChannels];`
- L472 `V_MEMRAM0 static V_MEMRAM1 vuint8 V_MEMRAM2 cclBusState[kCclNrOfChannels];`
- L481 `V_MEMRAM0 static V_MEMRAM1 vuint8 V_MEMRAM2 cclWakeUpIntState[kCclNrOfChannels];`
- L493 `V_MEMRAM0 static V_MEMRAM1 vuint8 V_MEMRAM2 cclStackInit;`
- L523 `V_MEMRAM0 static V_MEMRAM1 vuint8 V_MEMRAM2 cclNmActivationTimer[kCclNrOfChannels];`
- L539 `V_MEMRAM0 V_MEMRAM1 vuint8 V_MEMRAM2 cclChannelNumber;`
- L546 `V_MEMRAM0 V_MEMRAM1 vuint16 V_MEMRAM2 cclErrorCode;`
- L553 `V_MEMRAM0 V_MEMRAM1 vuint16 V_MEMRAM2 cclErrorLine;`
- L560 `V_MEMRAM0 V_MEMRAM1 vuint16 V_MEMRAM2 cclComponentID;`
- L569 `V_MEMRAM0 volatile V_MEMRAM1 vuint8 V_MEMRAM2 cclIntNetState[kCclNetReqTableSize];`
- L579 `V_MEMRAM0 V_MEMRAM1 vuint8 V_MEMRAM2 cclNmState[kCclNrOfChannels];`
- L592 `V_MEMRAM0 V_MEMRAM1 vuint8 V_MEMRAM2 cclComSwState[kCclNrOfChannels];`
- L1392 `vuint8 count;`
- L1536 `CanChannelHandle channel;`
- L1626 `CanChannelHandle channel;`
- L1661 `CanChannelHandle channel;`
- L1819 `vuint8 intNetState;`
- L1821 `vuint8 index;`
- L1825 `vuint8 channel;`
- L1826 `vuint8 retValue;`
- L1965 `vuint8 intNetState;`
- L1966 `vuint8 channel;`
- L1968 `vuint8 index;`
- L2177 `vuint8 retValue;`
- L2213 `vuint8 retValue;`
- L2214 `vuint8 intNetState;`
- L2216 `vuint8 index;`
- L2322 `vuint8 retVal;`
- L2366 `vuint8 retVal;`

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 37

- L608 `CclInitPorts(CCL_CHANNEL_CCLTYPE_ONLY)` [선언]
- L628 `CclFatalError(vuint8 ChannelNumber, vuint16 ErrorCode, vuint16 ErrorLine, vuint8 ComponentID) | CALLED BY: assertions within the CANbedded stack and/or CCL | re-directe the call of the "FatalError"-functions | Application is NOT ALLOWED to call this function! | PRECONDITIONS: 'use CCL ErrorHook' has to be activated in the used generation tool to notify the application | INPUT PARAMETERS: ChannelNumber: 0-255 (255 default for 'NO_CHANNEL_INFO_AVAILABLE') | ErrorCode : 0-65.535 (individual specified in each layer) | ErrorLine : __LINE__ (0 if no information is available) | ComponentID : s. CCL_INC.H | RETURN VALUE: void | DESCRIPTION: Convert the given error information to the global error handling | variables and call a application function to handle the error. |*************************************************************************/ void CCL_API_CALL_TYPE CclFatalError(CanChannelHandle ChannelNumber, vuint16 ErrorCode, vuint16 ErrorLine, vuint8 ComponentID)` [정의]
- L657 `CclInitPorts(CanChannelHandle channel) | or | void CclInitPorts(void) | CALLED BY: CclInitPowerOn | Application is NOT ALLOWED to call this function! | PRECONDITIONS: to be called in loop for each channel | INPUT PARAMETERS: channel (multiple channel) | or | void (single channel) | RETURN VALUE: void | DESCRIPTION: calls function container to inizialise all HW ports | (both the transceiver port register and the | transceiver port interrupt) with the channel specific | parameters. |*************************************************************************/ static void CCL_API_CALL_TYPE CclInitPorts(CCL_CHANNEL_CCLTYPE_ONLY)` [정의]
- L729 `ApplCanWakeUp(CanChannelHandle channel) | or | void ApplCanWakeUp(void) | CALLED BY: CANdrv: CanWakeUpInt / CanWakeUpTask (internal wake up INT) | CCL: CclCanWakeUpInt (external wake up INT) | Application is NOT ALLOWED to call this function! | PRECONDITIONS: - | INPUT PARAMETERS: channel (multiple channel) | or | void (single channel) | RETURN VALUE: void | DESCRIPTION: Function notifies WakeUp over RX/INH transceiver port | (handled in CCL) or WakeUp over CAN controller | (handled in CAN driver), sets the external channel | wakeup request flag and requests the power manager state. |*************************************************************************/ void DRV_API_CALLBACK_TYPE ApplCanWakeUp(CAN_CHANNEL_CANTYPE_ONLY)` [정의]
- L799 `CclCanNormal(CanChannelHandle channel) | or | void CclCanNormal(void) | CALLED BY: CCL | Application is NOT ALLOWED to call this function! | PRECONDITIONS: - | INPUT PARAMETERS: channel (multiple channel) | or | void (single channel) | RETURN VALUE: void | DESCRIPTION: set CAN transceiver and CAN controller to NORMAL mode |*************************************************************************/ void CCL_API_CALL_TYPE CclCanNormal(CCL_CHANNEL_CCLTYPE_ONLY)` [정의]
- L892 `CclCanStandby(CanChannelHandle channel) | or | void CclCanStandby(void) | CALLED BY: CCL | Application is NOT ALLOWED to call this function! | PRECONDITIONS: - | INPUT PARAMETERS: channel (multiple channel) | or | void (single channel) | RETURN VALUE: void | DESCRIPTION: set CAN controller and CAN transceiver to SLEEP mode, | release the power manager state and enable the | external wakeup port INT |*************************************************************************/ void CCL_API_CALL_TYPE CclCanStandby(CCL_CHANNEL_CCLTYPE_ONLY)` [정의]
- L920 `CanSleep(CCL_CHANNEL_CANPARA_ONLY)` [선언]
- L1051 `ApplCclCanStandby(CCL_CHANNEL_CCLPARA_FIRST cclCanSleepReturnCode[CCL_CHANNEL_CCLINDEX])` [선언]
- L1377 `CclInit(CanChannelHandle channel) | or | void CclInit(void) | CALLED BY: Application during startup the system or while runtime | PRECONDITIONS: - | INPUT PARAMETERS: channel (multiple channel) | or | void (single channel) | RETURN VALUE: void | DESCRIPTION: channel specific initialisation of CCL and CANbedded stack |*************************************************************************/ void CCL_API_CALL_TYPE CclInit(CCL_CHANNEL_CCLTYPE_ONLY)` [정의]
- L1533 `CclInitPowerOn(CCL_ECUS_NODESTYPE_ONLY)` [정의]
- L1616 `CclSystemStartup(void) | CALLED BY: Application | PRECONDITIONS: | INPUT PARAMETERS: void | RETURN VALUE: void | DESCRIPTION: startup handling of CANbedded stack |*************************************************************************/ void CCL_API_CALL_TYPE CclSystemStartup(void)` [정의]
- L1649 `CclSystemShutdown(void) | CALLED BY: Application | PRECONDITIONS: This function has to be called with disabled global | interrupts. | INPUT PARAMETERS: void | RETURN VALUE: void | DESCRIPTION: shutdown handling of CANbedded stack |*************************************************************************/ void CCL_API_CALL_TYPE CclSystemShutdown(void)` [정의]
- L1705 `CclGetErrorPort(CanChannelHandle channel) | or | vuint8 CclGetErrorPort(void) | CALLED BY: Application | PRECONDITIONS: | INPUT PARAMETERS: channel (multiple channel) | or | void (single channel) | RETURN VALUE: state of transceiver error port | DESCRIPTION: returns the current transceiver error port state |*************************************************************************/ vuint8 CCL_API_CALL_TYPE CclGetErrorPort(CCL_CHANNEL_CCLTYPE_ONLY)` [정의]
- L1741 `CclGetChannelState(CanChannelHandle channel) | or | vuint8 CclGetChannelState(void) | CALLED BY: Application | PRECONDITIONS: | INPUT PARAMETERS: channel (multiple channel) | or | void (single channel) | RETURN VALUE: kCclNmStateSleep, sleep state | kCclNmStateGoToSleep, prepare sleep state | kCclNmStateActive, normal state | kCclNmPowerFail, power fail state | DESCRIPTION: returns the current CCL state |*************************************************************************/ vuint8 CCL_API_CALL_TYPE CclGetChannelState(CCL_CHANNEL_CCLTYPE_ONLY)` [정의]
- L1807 `CclRequestCommunication(vuint8 cclComHandle) | CALLED BY: Application | PRECONDITIONS: | INPUT PARAMETERS: cclComHandle: communication request handle | RETURN VALUE: kCclReqOk: the communication request is set correctly | kCclReqFailed: the communication request is stored but | not handeled | DESCRIPTION: request the communication on the CAN channels which | are configured for the cclComHandle |*************************************************************************/ vuint8 CCL_API_CALL_TYPE CclRequestCommunication(vuint8 cclComHandle)` [정의]
- L1955 `CclReleaseCommunication(vuint8 cclComHandle) | CALLED BY: Application | PRECONDITIONS: | INPUT PARAMETERS: cclComHandle: communication request handle | RETURN VALUE: - | DESCRIPTION: release the communication request for CAN channels which | are configured for the cclComHandle |*************************************************************************/ void CCL_API_CALL_TYPE CclReleaseCommunication(vuint8 cclComHandle)` [정의]
- L2167 `CclGetComHandleState(vuint8 cclComHandle) | CALLED BY: Application | PRECONDITIONS: | INPUT PARAMETERS: cclComHandle: communication request handle | RETURN VALUE: kCclComReqPending: the communication request is set | kCclComReqNotPending: the communication request is not set | DESCRIPTION: Return the communication handle request state |*************************************************************************/ vuint8 CCL_API_CALL_TYPE CclGetComHandleState(vuint8 cclComHandle)` [정의]
- L2200 `CclComRequestPending(void) or | vuint8 CCL_API_CALL_TYPE CclComRequestPending(CanChannelHandle channel) | CALLED BY: Application | PRECONDITIONS: | INPUT PARAMETERS: channel: communication request handle | RETURN VALUE: kCclComReqPending: minimum one communication request is set | for the channel | kCclComReqNotPending: no communication request is set | for the channel | DESCRIPTION: Return the communication state for the CAN channel |*************************************************************************/ vuint8 CCL_API_CALL_TYPE CclComRequestPending(CCL_CHANNEL_CCLTYPE_ONLY)` [정의]
- L2256 `CclEnableCommunication(vuint8 channel) | or | void CclEnableCommunication(void) | CALLED BY: DPM or Application | PRECONDITIONS: Not to be called within ISR | If the DPM component is configured than it is not allowed | to call this function by the application. | INPUT PARAMETERS: channel (multiple channel) | or | void (single channel) | RETURN VALUE: - | DESCRIPTION: enable the HW communication state of CAN controller | CAN transceiver and allow the communication (caused | by external channel and internal network requests) |*************************************************************************/ void CCL_API_CALL_TYPE CclEnableCommunication(CCL_CHANNEL_CCLTYPE_ONLY)` [정의]
- L2279 `CclDisableCommunication(vuint8 channel) | or | void CclDisableCommunication(void) | CALLED BY: DPM or Application | PRECONDITIONS: Not to be called within ISR | If the DPM component is configured than it is not allowed | to call this function by the application. | INPUT PARAMETERS: channel (multiple channel) | or | void (single channel) | RETURN VALUE: - | DESCRIPTION: disable the HW communication state of CAN controller | CAN transceiver and prevent the communication (caused | by external channel and internal network requests) |*************************************************************************/ void CCL_API_CALL_TYPE CclDisableCommunication(CCL_CHANNEL_CCLTYPE_ONLY)` [정의]
- L2308 `CclSetTrcvRxOnlyMode(CanChannelHandle channel) | CALLED BY: Application from task level | PRECONDITIONS: | INPUT PARAMETERS: channel (multiple receive channel) | or | void (single receive channel) | RETURN VALUE: kCclTrcvStateChangePerformed | or | kCclTrcvStateChangeNotPerformed | DESCRIPTION: informs the CCL to set the transceiver into the receive | only mode |*************************************************************************/ vuint8 CclSetTrcvRxOnlyMode(CCL_CHANNEL_CCLTYPE_ONLY)` [정의]
- L2353 `CclLeaveTrcvRxOnlyMode(CanChannelHandle channel) | CALLED BY: Application from task level | PRECONDITIONS: | INPUT PARAMETERS: channel (multiple receive channel) | or | void (single receive channel) | RETURN VALUE: kCclTrcvStateChangePerformed | or | kCclTrcvStateChangeNotPerformed | DESCRIPTION: informs the CCL to set the transceiver back to normal mode |*************************************************************************/ vuint8 CclLeaveTrcvRxOnlyMode(CCL_CHANNEL_CCLTYPE_ONLY)` [정의]
- L2439 `ApplNmCanNormal(void) or | void ApplNmCanNormal(CanChannelHandle channel) | CALLED BY: OSEK-NM from NmOsekInit() and NmTask() | PRECONDITIONS: | INPUT PARAMETERS: void (single channel) or channel (multiple channel) | RETURN VALUE: void | DESCRIPTION: Set the CAN controller and the CAN transceiver in NORMAL mode | and disable the external wakeup port INT. |*************************************************************************/ void NM_API_CALLBACK_TYPE ApplNmCanNormal(NM_CHANNEL_APPLTYPE_ONLY)` [정의]
- L2463 `ApplNmCanSleep(void) | void ApplNmCanSleep(CanChannelHandle channel) | CALLED BY: OSEK-NM from NmOsekInit() and NmTask() | PRECONDITIONS: | INPUT PARAMETERS: void (single channel) or channel (multiple channel) | RETURN VALUE: void | DESCRIPTION: Set the CAN controller and the CAN transceiver in SLEEP mode | and enables the external wakeup port INT. |*************************************************************************/ void NM_API_CALLBACK_TYPE ApplNmCanSleep(NM_CHANNEL_APPLTYPE_ONLY)` [정의]
- L2491 `ApplNmCanBusSleep(void) or | void ApplNmCanBusSleep(CanChannelHandle channel) | CALLED BY: OSEK-NM from NmOsekInit() and NmTask() | PRECONDITIONS: | INPUT PARAMETERS: void (single channel) or channel (multiple channel) | RETURN VALUE: void | DESCRIPTION: s. ApplNmCanSleep |*************************************************************************/ void NM_API_CALLBACK_TYPE ApplNmCanBusSleep(NM_CHANNEL_APPLTYPE_ONLY)` [정의]
- L2506 `ApplNmBusOff(void) or | void ApplNmBusOff(CanChannelHandle channel) | CALLED BY: OSEK-NM from NmCanError() (CAN driver error interrupt) | PRECONDITIONS: | INPUT PARAMETERS: void (single channel) or channel (multiple channel) | RETURN VALUE: void | DESCRIPTION: The CCL handles the configured interaction layer in | dependency of the CCL configuration. |*************************************************************************/ void NM_API_CALLBACK_TYPE ApplNmBusOff(NM_CHANNEL_APPLTYPE_ONLY)` [정의]
- L2530 `ApplNmBusOffEnd(void) or | void ApplNmBusOffEnd(CanChannelHandle channel) | CALLED BY: OSEK-NM from NmTask() | PRECONDITIONS: | INPUT PARAMETERS: void (single channel) or channel (multiple channel) | RETURN VALUE: void | DESCRIPTION: The CCL handles the configured interaction layer in | dependency of the CCL configuration. |*************************************************************************/ void NM_API_CALLBACK_TYPE ApplNmBusOffEnd(NM_CHANNEL_APPLTYPE_ONLY)` [정의]
- L2554 `ApplNmBusStart(void) or | void ApplNmBusStart(CanChannelHandle channel) | CALLED BY : OSEK-NM from NmOsekInit() and NmTask() | PRECONDITIONS: | INPUT PARAMETERS: void (single channel) or channel (multiple channel) | RETURN VALUE: void | DESCRIPTION: Start the communication via interaction layer. |*************************************************************************/ void NM_API_CALLBACK_TYPE ApplNmBusStart(NM_CHANNEL_APPLTYPE_ONLY)` [정의]
- L2651 `ApplNmWaitBusSleep(void) or | void ApplNmWaitBusSleep(CanChannelHandle channel) | CALLED BY : OSEK-NM from NmTask() | PRECONDITIONS: | INPUT PARAMETERS: void (single channel) or channel (multiple channel) | RETURN VALUE : void | DESCRIPTION : Stop the communication via interaction layer. |*************************************************************************/ void NM_API_CALLBACK_TYPE ApplNmWaitBusSleep(NM_CHANNEL_APPLTYPE_ONLY)` [정의]
- L2680 `ApplNmWaitBusSleepCancel(void) or | void ApplNmWaitBusSleepCancel(CanChannelHandle channel) | CALLED BY : OSEK-NM from NmTask() | PRECONDITIONS: | INPUT PARAMETERS: void (single channel) or channel (multiple channel) | RETURN VALUE : void | DESCRIPTION : Start the communication via interaction layer. |*************************************************************************/ void NM_API_CALLBACK_TYPE ApplNmWaitBusSleepCancel(NM_CHANNEL_APPLTYPE_ONLY)` [정의]
- L2729 `ApplNmBasicBusOffStart(void) or | void ApplNmBasicBusOffStart(CanChannelHandle channel) | CALLED BY: NM Powertrain Basic from NmBasicCanBusOff | (CAN driver error interrupt) | PRECONDITIONS: | INPUT PARAMETERS: void (single channel) or channel (multiple channel) | RETURN VALUE: void | DESCRIPTION : The CCL handles the configured interaction layer in | dependency of the CCL configuration. |*************************************************************************/ void ApplNmBasicBusOffStart(NMBASIC_CHANNEL_NMTYPE_ONLY)` [정의]
- L2754 `ApplNmBasicBusOffEnd(void) or | void ApplNmBasicBusOffEnd(CanChannelHandle channel) | CALLED BY: NM Powertrain Basic (task level) | PRECONDITIONS: | INPUT PARAMETERS: void (single channel) or channel (multiple channel) | RETURN VALUE: void | DESCRIPTION : The CCL handles the configured interaction layer in | dependency of the CCL configuration. |*************************************************************************/ void ApplNmBasicBusOffEnd(NMBASIC_CHANNEL_NMTYPE_ONLY)` [정의]
- L2778 `ApplNmBasicEnabledCom(void) or | void ApplNmBasicEnabledCom(CanChannelHandle channel) | CALLED BY: NM Powertrain Basic (task level) | PRECONDITIONS: | INPUT PARAMETERS: void (single channel) or channel (multiple channel) | RETURN VALUE: void | DESCRIPTION : Start the communication via interaction layer. |*************************************************************************/ void ApplNmBasicEnabledCom(NMBASIC_CHANNEL_NMTYPE_ONLY)` [정의]
- L2801 `ApplNmBasicDisabledCom(void) or | void ApplNmBasicDisabledCom(CanChannelHandle channel) | CALLED BY: NM Powertrain Basic (task level) | PRECONDITIONS: | INPUT PARAMETERS: void (single channel) or channel (multiple channel) | RETURN VALUE: void | DESCRIPTION : Stop the communication via interaction layer. |*************************************************************************/ void ApplNmBasicDisabledCom(NMBASIC_CHANNEL_NMTYPE_ONLY)` [정의]
- L2824 `ApplNmBasicSwitchTransceiverOn(void) or | void AApplNmBasicSwitchTransceiverOn(CanChannelHandle channel) | CALLED BY: NM Powertrain Basic (task level) | PRECONDITIONS: - | INPUT PARAMETERS: void (single channel) or channel (multiple channel) | RETURN VALUE: void | DESCRIPTION: Set the transceiver into the normal mode. |*************************************************************************/ void ApplNmBasicSwitchTransceiverOn(NMBASIC_CHANNEL_NMTYPE_ONLY)` [정의]
- L2848 `ApplNmBasicSwitchTransceiverOff(void) or | void ApplNmBasicSwitchTransceiverOff(CanChannelHandle channel) | CALLED BY: NM Powertrain Basic (task level) | PRECONDITIONS: - | INPUT PARAMETERS: void (single channel) or channel (multiple channel) | RETURN VALUE: vuint8 | DESCRIPTION: Set the transceiver into the standby/sleep mode. |*************************************************************************/ vuint8 ApplNmBasicSwitchTransceiverOff(NMBASIC_CHANNEL_NMTYPE_ONLY)` [정의]
- L2874 `ApplNmBasicFatalError(vuint8 error) or | void ApplNmBasicFatalError(CanChannelHandle channel, vuint8 error) | CALLED BY : NM Powertrain Basic | PRECONDITIONS: NM Powertrain Basic assert occurs | INPUT PARAMETERS: void (single channel) or channel (multiple channel) | RETURN VALUE: void | DESCRIPTION: Call the CCL error handler. |*************************************************************************/ void ApplNmBasicFatalError(NMBASIC_CHANNEL_APPLTYPE_FIRST vuint8 error)` [정의]

### 4. 각 함수별 역할 설명

- `CclInitPorts` (L608, 선언): 모듈 런타임 상태, 버퍼, 카운터, 하드웨어/생성 파라미터를 초기화한다.
- `CclFatalError` (L628, 정의): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `CclInitPorts` (L657, 정의): 모듈 런타임 상태, 버퍼, 카운터, 하드웨어/생성 파라미터를 초기화한다.
- `ApplCanWakeUp` (L729, 정의): CAN 채널의 슬립/웨이크업 전환과 관련 상태를 처리한다.
- `CclCanNormal` (L799, 정의): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `CclCanStandby` (L892, 정의): reset the CanSleep return value
- `CanSleep` (L920, 선언): set CAN controller to SLEEP mode
- `ApplCclCanStandby` (L1051, 선언): external EMC/CAN wake up notification
- `CclInit` (L1377, 정의): =====================================================================
- `CclInitPowerOn` (L1533, 정의): NAME:               CclInitPowerOn CALLED BY:          Application during startup the system or while runtime PRECONDITIONS:      global interrupts have to be disabled RETURN VALUE:       void DESCRIPTION:        system specific initialisation of CCL and CANbe
- `CclSystemStartup` (L1616, 정의): set repeated initialisation flag
- `CclSystemShutdown` (L1649, 정의): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `CclGetErrorPort` (L1705, 정의): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `CclGetChannelState` (L1741, 정의): no transceiver error port information available
- `CclRequestCommunication` (L1807, 정의): 통신 허용/차단, 네트워크 요청/해제 또는 통신 모드 전환을 처리한다.
- `CclReleaseCommunication` (L1955, 정의): ESCAN00010661 Enable the global interrupt before leaving the function.
- `CclGetComHandleState` (L2167, 정의): ESCAN00010661 Enable the global interrupt before leaving the function.
- `CclComRequestPending` (L2200, 정의): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `CclEnableCommunication` (L2256, 정의): 통신 허용/차단, 네트워크 요청/해제 또는 통신 모드 전환을 처리한다.
- `CclDisableCommunication` (L2279, 정의): set HW communication state to enable
- `CclSetTrcvRxOnlyMode` (L2308, 정의): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `CclLeaveTrcvRxOnlyMode` (L2353, 정의): By using the following table of function calls, code-doubled functions are called from the indexed CCL. This is done to simplify the called functions (no distinction of the parameter 'channel' is necessary).
- `ApplNmCanNormal` (L2439, 정의): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `ApplNmCanSleep` (L2463, 정의): NAME:                ApplNmCanNormal PROTOTYPE:           void ApplNmCanNormal(void) or void ApplNmCanNormal(CanChannelHandle channel) CALLED BY:           OSEK-NM from NmOsekInit() and NmTask() PRECONDITIONS: INPUT PARAMETERS:    void (single channel) or chan
- `ApplNmCanBusSleep` (L2491, 정의): NAME:                ApplNmCanSleep PROTOTYPE:           void ApplNmCanSleep(void) void ApplNmCanSleep(CanChannelHandle channel) CALLED BY:           OSEK-NM from NmOsekInit() and NmTask() PRECONDITIONS: INPUT PARAMETERS:    void (single channel) or channel (m
- `ApplNmBusOff` (L2506, 정의): NAME:                ApplNmCanBusSleep PROTOTYPE:           void ApplNmCanBusSleep(void) or void ApplNmCanBusSleep(CanChannelHandle channel) CALLED BY:           OSEK-NM from NmOsekInit() and NmTask() PRECONDITIONS: INPUT PARAMETERS:    void (single channel) o
- `ApplNmBusOffEnd` (L2530, 정의): NAME:               ApplNmBusOff PROTOTYPE:          void ApplNmBusOff(void) or void ApplNmBusOff(CanChannelHandle channel) CALLED BY:          OSEK-NM from NmCanError() (CAN driver error interrupt) PRECONDITIONS: INPUT PARAMETERS:   void (single channel) or c
- `ApplNmBusStart` (L2554, 정의): NAME:               ApplNmBusOffEnd PROTOTYPE:          void ApplNmBusOffEnd(void) or void ApplNmBusOffEnd(CanChannelHandle channel) CALLED BY:          OSEK-NM from NmTask() PRECONDITIONS: INPUT PARAMETERS:   void (single channel) or channel (multiple channel
- `ApplNmWaitBusSleep` (L2651, 정의): change CCL state to prepare sleep
- `ApplNmWaitBusSleepCancel` (L2680, 정의): set the CCL state to wait bus sleep
- `ApplNmBasicBusOffStart` (L2729, 정의): CCL is in wait bus sleep and the network management is woken up by a NM message
- `ApplNmBasicBusOffEnd` (L2754, 정의): NAME:               ApplNmBasicBusOffStart PROTOTYPE:          void ApplNmBasicBusOffStart(void) or void ApplNmBasicBusOffStart(CanChannelHandle channel) CALLED BY:          NM Powertrain Basic from NmBasicCanBusOff (CAN driver error interrupt) PRECONDITIONS: 
- `ApplNmBasicEnabledCom` (L2778, 정의): NAME:               ApplNmBasicBusOffEnd PROTOTYPE:          void ApplNmBasicBusOffEnd(void) or void ApplNmBasicBusOffEnd(CanChannelHandle channel) CALLED BY:          NM Powertrain Basic (task level) PRECONDITIONS: INPUT PARAMETERS:   void (single channel) or
- `ApplNmBasicDisabledCom` (L2801, 정의): NAME:               ApplNmBasicEnabledCom PROTOTYPE:          void ApplNmBasicEnabledCom(void) or void ApplNmBasicEnabledCom(CanChannelHandle channel) CALLED BY:          NM Powertrain Basic (task level) PRECONDITIONS: INPUT PARAMETERS:   void (single channel)
- `ApplNmBasicSwitchTransceiverOn` (L2824, 정의): NAME:               ApplNmBasicDisabledCom PROTOTYPE:          void ApplNmBasicDisabledCom(void) or void ApplNmBasicDisabledCom(CanChannelHandle channel) CALLED BY:          NM Powertrain Basic (task level) PRECONDITIONS: INPUT PARAMETERS:   void (single chann
- `ApplNmBasicSwitchTransceiverOff` (L2848, 정의): NAME:               ApplNmBasicSwitchTransceiverOn PROTOTYPE:          void ApplNmBasicSwitchTransceiverOn(void) or void AApplNmBasicSwitchTransceiverOn(CanChannelHandle channel) CALLED BY:          NM Powertrain Basic (task level) PRECONDITIONS:      - INPUT 
- `ApplNmBasicFatalError` (L2874, 정의): NAME:               ApplNmBasicSwitchTransceiverOff PROTOTYPE:          void ApplNmBasicSwitchTransceiverOff(void) or void ApplNmBasicSwitchTransceiverOff(CanChannelHandle channel) CALLED BY:          NM Powertrain Basic (task level) PRECONDITIONS:      - INPU

### 5. 필요시 코드 부연 설명

대표 함수 원형 수준의 코드 맥락이다. 전체 구현은 원본 파일이 크므로 함수명/역할 중심으로 분석했다.

```c
// src/CBD/Ccl/ccl.c:608
static void CCL_API_CALL_TYPE CclInitPorts(CCL_CHANNEL_CCLTYPE_ONLY)
```
```c
// src/CBD/Ccl/ccl.c:628
| PROTOTYPE: void CCL_API_CALL_TYPE CclFatalError(vuint8 ChannelNumber, vuint16 ErrorCode, vuint16 ErrorLine, vuint8 ComponentID) | CALLED BY: assertions within the CANbedded stack and/or CCL | re-directe the call of the "FatalError"-functions | Application is NOT ALLOWED to call this function! | PRECONDITIONS: 'use CCL ErrorHook' has to be activated in the used generation tool to notify the application | INPUT PARAMETERS: ChannelNumber: 0-255 (255 default for 'NO_CHANNEL_INFO_AVAILABLE') | ErrorCode : 0-65.535 (individual specified in each layer) | ErrorLine : __LINE__ (0 if no information is available) | ComponentID : s. CCL_INC.H | RETURN VALUE: void | DESCRIPTION: Convert the given error information to the global error handling | variables and call a application function to handle the error. |*************************************************************************/ void CCL_API_CALL_TYPE CclFatalError(CanChannelHandle ChannelNumber, vuint16 ErrorCode, vuint16 ErrorLine, vuint8 ComponentID)
```
```c
// src/CBD/Ccl/ccl.c:657
| PROTOTYPE: void CclInitPorts(CanChannelHandle channel) | or | void CclInitPorts(void) | CALLED BY: CclInitPowerOn | Application is NOT ALLOWED to call this function! | PRECONDITIONS: to be called in loop for each channel | INPUT PARAMETERS: channel (multiple channel) | or | void (single channel) | RETURN VALUE: void | DESCRIPTION: calls function container to inizialise all HW ports | (both the transceiver port register and the | transceiver port interrupt) with the channel specific | parameters. |*************************************************************************/ static void CCL_API_CALL_TYPE CclInitPorts(CCL_CHANNEL_CCLTYPE_ONLY)
```
```c
// src/CBD/Ccl/ccl.c:729
| PROTOTYPE: void ApplCanWakeUp(CanChannelHandle channel) | or | void ApplCanWakeUp(void) | CALLED BY: CANdrv: CanWakeUpInt / CanWakeUpTask (internal wake up INT) | CCL: CclCanWakeUpInt (external wake up INT) | Application is NOT ALLOWED to call this function! | PRECONDITIONS: - | INPUT PARAMETERS: channel (multiple channel) | or | void (single channel) | RETURN VALUE: void | DESCRIPTION: Function notifies WakeUp over RX/INH transceiver port | (handled in CCL) or WakeUp over CAN controller | (handled in CAN driver), sets the external channel | wakeup request flag and requests the power manager state. |*************************************************************************/ void DRV_API_CALLBACK_TYPE ApplCanWakeUp(CAN_CHANNEL_CANTYPE_ONLY)
```
```c
// src/CBD/Ccl/ccl.c:799
| PROTOTYPE: void CclCanNormal(CanChannelHandle channel) | or | void CclCanNormal(void) | CALLED BY: CCL | Application is NOT ALLOWED to call this function! | PRECONDITIONS: - | INPUT PARAMETERS: channel (multiple channel) | or | void (single channel) | RETURN VALUE: void | DESCRIPTION: set CAN transceiver and CAN controller to NORMAL mode |*************************************************************************/ void CCL_API_CALL_TYPE CclCanNormal(CCL_CHANNEL_CCLTYPE_ONLY)
```
```c
// src/CBD/Ccl/ccl.c:892
| PROTOTYPE: void CclCanStandby(CanChannelHandle channel) | or | void CclCanStandby(void) | CALLED BY: CCL | Application is NOT ALLOWED to call this function! | PRECONDITIONS: - | INPUT PARAMETERS: channel (multiple channel) | or | void (single channel) | RETURN VALUE: void | DESCRIPTION: set CAN controller and CAN transceiver to SLEEP mode, | release the power manager state and enable the | external wakeup port INT |*************************************************************************/ void CCL_API_CALL_TYPE CclCanStandby(CCL_CHANNEL_CCLTYPE_ONLY)
```
```c
// src/CBD/Ccl/ccl.c:920
cclCanSleepReturnCode[CCL_CHANNEL_CCLINDEX] = CanSleep(CCL_CHANNEL_CANPARA_ONLY)
```
```c
// src/CBD/Ccl/ccl.c:1051
cclCanSleepRepetition[CCL_CHANNEL_CCLINDEX] = ApplCclCanStandby(CCL_CHANNEL_CCLPARA_FIRST cclCanSleepReturnCode[CCL_CHANNEL_CCLINDEX])
```

## 파일이름 : ccl

- 경로: `src/CBD/Ccl/ccl.h`
- 파일 종류: `.h`
- 파일 크기: 22,057 bytes
- 파일 내용: CCL API, 타입, 매크로, 상태값을 정의하는 헤더이다.

### 1. .c/.h 파일 이름

- `ccl.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 7
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 0

#### 주요 매크로
- L225 `CCL_H`
- L253 `kCclExtChWupReq` = `0x01`
- L254 `kCclNoExtChWupReq` = `0xFF`
- L256 `kCclNoIntNetReq` = `0xFF`
- L257 `kCclNoIntNetRel` = `0xFF`
- L259 `kCclDisablePortIRQ` = `0x00`
- L260 `kCclEnablePortIRQ` = `0x01`

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

CCL 계층은 통신 상태를 제어하므로 Classical CAN과 CAN-FD 송수신을 동일한 정책으로 제어해야 한다.

- `CommunicationControl`로 Tx/Rx disable 시 FD frame도 함께 차단되는지 확인한다.
- online/offline 상태가 Classical mailbox와 FD mailbox 모두에 적용되는지 확인한다.
- 진단 요청이 CAN-FD TP를 통해 들어와도 기존 `DiagReq` 사용자 요청 정책과 연결되도록 한다.

## 파일이름 : ccl_inc

- 경로: `src/CBD/Ccl/ccl_inc.h`
- 파일 종류: `.h`
- 파일 크기: 42,479 bytes
- 파일 내용: CCL API, 타입, 매크로, 상태값을 정의하는 헤더이다.

### 1. .c/.h 파일 이름

- `ccl_inc.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 190
- typedef/struct/enum 후보 수: 2
- 전역 변수/테이블 선언 후보 수: 10

#### 주요 매크로
- L236 `CCL_INC_H`
- L255 `CCL__CORE_VERSION` = `0x0225`
- L256 `CCL__CORE_RELEASE_VERSION` = `0x02`
- L261 `CCL_CORE_VERSION` = `CCL__CORE_VERSION`
- L262 `CCL_CORE_BUGFIX_VERSION` = `CCL__CORE_RELEASE_VERSION`
- L263 `kCclBugfixVersion` = `kCclReleaseVersion`
- L272 `CclAssert(p,e)` = `{if(!(p)){CclFatalError(channel, (e), (vuint16)(__LINE__),kModuleCcl);}}`
- L274 `CclAssert(p,e)` = `{if(!(p)){CclFatalError((CanChannelHandle)kCclChannelIndex, (e), (vuint16)(__LINE__),kModuleCcl);}}`
- L280 `CclAssert(p,e)` = `{if(!(p)){CclFatalError(0, (e), (vuint16)(__LINE__),kModuleCcl);}}`
- L284 `CclAssert(p,e)`
- L289 `CCL_API_CALL_TYPE`
- L292 `CCL_API_CALLBACK_TYPE`
- L295 `CCL_INTERNAL_CALL_TYPE`
- L300 `cclNetRequestField` = `cclIntNetState`
- L301 `kCclNrOfNetworks` = `kCclNrOfNmNetworks`
- L302 `cclStateCom` = `cclComSwState`
- L304 `kCclComOn` = `kCclComSwOn`
- L305 `kCclComOff` = `kCclComSwOff`
- L308 `CCL_USE_REQUEST_SETINTCOM_FCT` = `CCL_USE_REQUEST_SETCOM_FCT`
- L309 `CCL_USE_REQUEST_SETEXTCOM_FCT` = `CCL_USE_REQUEST_SETCOM_FCT`
- L313 `CclInitTransFctTbl` = `CclInitTrcvFctTbl`
- L314 `CclWakeUpTransFctTbl` = `CclWakeUpTrcvFctTbl`
- L315 `CclSleepTransFctTbl` = `CclSleepTrcvFctTbl`
- L316 `CclComRelFctTbl` = `CclClearComReqFctTbl`
- L317 `CclExtCanComReqFctTbl` = `CclSetComReqFctTbl`
- L318 `CclIntComReqFctTbl` = `CclSetComReqFctTbl`
- L320 `CclInitTransFct` = `CclInitTrcvFct`
- L321 `CclWakeUpTransFct` = `CclWakeUpTrcvFct`
- L322 `CclSleepTransFct` = `CclSleepTrcvFct`
- L323 `CclComRelFct` = `CclClearComReqFct`
- L324 `CclExtCanComReqFct` = `CclSetComReqFct`
- L325 `CclIntComReqFct` = `CclSetComReqFct`
- L332 `CCL_ENABLE_COM_REQ_HANDLING_API`
- L337 `CclGetNmState` = `CclGetChannelState`
- L340 `kModuleCanDrv` = `kComponentCanDrv`
- L341 `kModuleLinDrv` = `kComponentLinDrv`
- L342 `kModuleIl` = `kComponentIl`
- L343 `kModuleDbk` = `kComponentDbk`
- L344 `kModuleMm` = `kComponentMm`
- L345 `kModuleTp` = `kComponentTp`
- L346 `kModuleNm` = `kComponentNm`
- L347 `kModuleNmPt` = `kComponentNmPt`
- L348 `kModuleSm` = `kComponentSm`
- L349 `kModuleNmVagC` = `kComponentNmVagC`
- L350 `kModuleDiagx` = `kComponentDiagx`
- L351 `kModuleCANdesc` = `kComponentCANdesc`
- L352 `kModuleCcl` = `kComponentCcl`
- L354 `kCclNmStateSleep` = `kCclStateSleep`
- L355 `kCclNmStateGoToSleep` = `kCclStateGoToSleep`
- L356 `kCclNmStateActive` = `kCclStateActive`
- L357 `kCclNmPowerFail` = `kCclPowerFail`
- L363 `CCL_CHANNEL_CCLPARA_ONLY` = `channel`
- L364 `CCL_CHANNEL_CCLPARA_FIRST` = `channel,`
- L365 `CCL_CHANNEL_CCLTYPE_ONLY` = `CanChannelHandle channel`
- L366 `CCL_CHANNEL_CCLTYPE_FIRST` = `CanChannelHandle channel,`
- L367 `CCL_CHANNEL_CCLINDEX` = `(vuint8)channel`
- L368 `CCL_CHANNEL_CANPARA_ONLY` = `channel`
- L369 `CCL_CHANNEL_CCLHANDLE_FIRST` = `(vuint8)channel,`
- L371 `CCL_CHANNEL_CCLPARA_ONLY`
- L372 `CCL_CHANNEL_CCLPARA_FIRST`
- L373 `CCL_CHANNEL_CCLTYPE_ONLY` = `void`
- L374 `CCL_CHANNEL_CCLTYPE_FIRST`
- L375 `CCL_CHANNEL_CCLINDEX` = `(vuint8)0`
- L376 `CCL_CHANNEL_CANPARA_ONLY` = `(CanChannelHandle)kCclChannelIndex`
- L377 `CCL_CHANNEL_CCLHANDLE_FIRST`
- L380 `CCL_CHANNEL_CCLPARA_ONLY`
- L381 `CCL_CHANNEL_CCLPARA_FIRST`
- L382 `CCL_CHANNEL_CCLTYPE_ONLY` = `void`
- L383 `CCL_CHANNEL_CCLTYPE_FIRST`
- L384 `CCL_CHANNEL_CCLINDEX` = `(vuint8)0`
- L385 `CCL_CHANNEL_CANPARA_ONLY`
- L386 `CCL_CHANNEL_CCLHANDLE_FIRST`
- L392 `CCL_NET_CCLPARA_ONLY` = `channel`
- L393 `CCL_NET_CCLPARA_FIRST` = `channel,`
- L394 `CCL_NET_CCLINDEX` = `channel`
- L395 `CCL_NET_CCLTYPE_ONLY` = `CanChannelHandle channel`
- L396 `CCL_NET_CCLTYPE_FIRST` = `CanChannelHandle channel,`
- L398 `CCL_NET_CCLPARA_ONLY`
- L399 `CCL_NET_CCLPARA_FIRST`
- L400 `CCL_NET_CCLTYPE_ONLY` = `void`
- L401 `CCL_NET_CCLTYPE_FIRST`
- L402 `CCL_NET_CCLINDEX` = `0`
- L405 `CCL_NET_CCLPARA_ONLY`
- L406 `CCL_NET_CCLPARA_FIRST`
- L407 `CCL_NET_CCLTYPE_ONLY` = `void`
- L408 `CCL_NET_CCLTYPE_FIRST`
- L409 `CCL_NET_CCLINDEX` = `0`
- L415 `CCL_ECUS_NODESTYPE_ONLY` = `cclECUHandle  CanEcuNr`
- L416 `CCL_ECUS_NODESTYPE_FIRST` = `cclECUHandle  CanEcuNr`
- L417 `CCL_ECUS_NODESPARA_ONLY` = `(vuint8)CanEcuNr`
- L419 `CCL_ECUS_NODESTYPE_ONLY` = `void`
- L420 `CCL_ECUS_NODESTYPE_FIRST` = `void`
- L421 `CCL_ECUS_NODESPARA_ONLY`
- L426 `CCL_ECUS_NODESTYPE_ONLY` = `vuint8  IdentityNr`
- L427 `CCL_ECUS_NODESTYPE_FIRST` = `vuint8  IdentityNr,`
- L428 `CCL_ECUS_NODESPARA_ONLY` = `IdentityNr`
- L430 `CCL_ECUS_NODESTYPE_ONLY` = `void`
- L431 `CCL_ECUS_NODESTYPE_FIRST`
- L432 `CCL_ECUS_NODESPARA_ONLY`
- L435 `CCL_ECUS_NODESTYPE_ONLY` = `void`
- L436 `CCL_ECUS_NODESTYPE_FIRST`
- L437 `CCL_ECUS_NODESPARA_ONLY`
- L441 `CCL_NM_INITPOINTER_TYPE`
- L442 `CCL_NM_INITPOINTER_PARA`
- L445 `CCL_SET_MULTIPLE_CFG_HANDLE(i)`
- L450 `kCclStateSleep` = `0x00    /* CCL state: sleep */`
- L451 `kCclStateWaitBusSleep` = `0x01    /* CCL state: wait bus sleep */`
- L452 `kCclStateGoToSleep` = `0x02    /* CCL state: go to sleep */`
- L453 `kCclStateActive` = `0x03    /* CCL state: active */`
- L454 `kCclPowerFail` = `0x04    /* CCL state: power fail active */`
- L457 `kCclComHwDisabled` = `0x00    /* HW communication state: disabled */`
- L458 `kCclComHwEnabled` = `0x01    /* HW communication state: enabled */`
- L460 `kCclComSwOff` = `0x00    /* SW communication state: off */`
- L461 `kCclComSwOn` = `0x01    /* SW communication state: on */`
- L464 `kCclBusOn` = `0x00     /* CAN hardware and transceiver are active */`
- L465 `kCclBusOff` = `0x01     /* CAN hardware and transceiver are inactive */`
- L467 `kCclNoCanAvailable` = `0x00    /* CAN HW is not stable */`
- L468 `kCclCanAvailable` = `0x01    /* CAN HW is stable */`
- L470 `kCclTrcvWakeIntPending` = `0x00`
- L471 `kCclTrcvWakeIntNoPending` = `0x01`
- ... 생략 70개 (동일 패턴의 생성 심볼)

#### 주요 typedef/구조체
- L651 `typedef vuint8 cclECUHandle; /* ECU handle */  /* ESCAN00011187 */`
- L661 `typedef vuint8 CanChannelHandle;`

#### 주요 전역 변수/테이블 선언
- L736 `V_MEMRAM0 extern V_MEMRAM1 vuint8 V_MEMRAM2 cclChannelNumber;`
- L737 `V_MEMRAM0 extern V_MEMRAM1 vuint16 V_MEMRAM2 cclErrorCode;`
- L738 `V_MEMRAM0 extern V_MEMRAM1 vuint16 V_MEMRAM2 cclErrorLine;`
- L739 `V_MEMRAM0 extern V_MEMRAM1 vuint16 V_MEMRAM2 cclComponentID;`
- L745 `V_MEMRAM0 extern V_MEMRAM1 vuint8 V_MEMRAM2 cclComSwState[kCclNrOfChannels];`
- L750 `V_MEMRAM0 extern volatile V_MEMRAM1 vuint8 V_MEMRAM2 cclIntNetState[kCclNetReqTableSize];`
- L759 `V_MEMRAM0 extern V_MEMRAM1 vuint8 V_MEMRAM2 cclNmState[kCclNrOfChannels];`
- L761 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 kCclMainVersion;`
- L762 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 kCclSubVersion;`
- L763 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 kCclReleaseVersion;`

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 10

- L668 `CclFatalError(CanChannelHandle ChannelNumber, vuint16 ErrorCode, vuint16 ErrorLine, vuint8 ComponentID)` [선언]
- L671 `CclCanNormal(CCL_CHANNEL_CCLTYPE_ONLY)` [선언]
- L672 `CclCanStandby(CCL_CHANNEL_CCLTYPE_ONLY)` [선언]
- L679 `CclInitPowerOn(CCL_ECUS_NODESTYPE_ONLY)` [선언]
- L680 `CclInit(CCL_CHANNEL_CCLTYPE_ONLY)` [선언]
- L683 `CclSystemStartup(void)` [선언]
- L728 `CclSetTrcvRxOnlyMode(CCL_CHANNEL_CCLTYPE_ONLY)` [선언]
- L729 `CclLeaveTrcvRxOnlyMode(CCL_CHANNEL_CCLTYPE_ONLY)` [선언]
- L768 `ApplCclCanStandby(CCL_CHANNEL_CCLTYPE_FIRST vuint8 sleepResult)` [선언]
- L773 `ApplCclFatalError(void)` [선언]

### 4. 각 함수별 역할 설명

- `CclFatalError` (L668, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `CclCanNormal` (L671, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `CclCanStandby` (L672, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `CclInitPowerOn` (L679, 선언): 전원 인가 직후 한 번 수행되는 모듈 전역 초기화를 담당한다.
- `CclInit` (L680, 선언): 모듈 런타임 상태, 버퍼, 카운터, 하드웨어/생성 파라미터를 초기화한다.
- `CclSystemStartup` (L683, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `CclSetTrcvRxOnlyMode` (L728, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `CclLeaveTrcvRxOnlyMode` (L729, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `ApplCclCanStandby` (L768, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `ApplCclFatalError` (L773, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.

### 5. 필요시 코드 부연 설명

이 파일은 생성 설정/선언 성격이 강해 별도 코드 전문 인용 없이 심볼 표로 역할을 설명했다.
