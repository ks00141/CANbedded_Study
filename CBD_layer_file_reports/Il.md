# Il 계층 파일별 상세 분석 보고서

- 분석 대상: `Il` 계층
- 파일 수: 4
- 생성 시각: 2026-05-28 22:37:38
- 분석 방식: C 전처리 매크로, typedef, 전역 데이터 선언, 함수 선언/정의를 자동 추출하고 함수명/주석/계층 역할을 기준으로 한글 역할 설명을 부여함.

## 계층 요약

IL 계층은 DBC 기반 메시지/신호를 애플리케이션 버퍼와 CAN 핸들로 연결하고 주기/이벤트 송신 및 수신 통지를 처리한다.

## 파일 목록

- `src/CBD/Il/_il_inc.h` (7,732 bytes)
- `src/CBD/Il/il.c` (128,921 bytes)
- `src/CBD/Il/il_def.h` (151,160 bytes)
- `src/CBD/Il/il_inc.h` (7,732 bytes)

## 파일이름 : _il_inc

- 경로: `src/CBD/Il/_il_inc.h`
- 파일 종류: `.h`
- 파일 크기: 7,732 bytes
- 파일 내용: 해당 계층의 생성 코드 또는 보조 헤더이다.

### 1. .c/.h 파일 이름

- `_il_inc.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 1
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 0

#### 주요 매크로
- L97 `V_IL_INC_COMPONENT_HEADER`

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

IL 계층은 DBC 신호와 CAN payload buffer를 연결한다. CAN-FD 전환 시 IL은 하드웨어보다는 payload 크기와 signal layout의 영향을 받는다.

- 메시지 buffer를 8바이트 고정으로 가정하는 부분을 제거한다.
- signal accessor가 `message_length`를 넘지 않는지 검사한다.
- 기존 HS-CAN 메시지는 8바이트 layout을 유지하고, FD 메시지만 확장 layout을 사용하도록 메시지별 설정을 둔다.
- 주기 송신 task는 유지하되 송신 시 CAN Driver에 FD 여부와 payload length를 함께 전달한다.

## 파일이름 : il

- 경로: `src/CBD/Il/il.c`
- 파일 종류: `.c`
- 파일 크기: 128,921 bytes
- 파일 내용: Vector IL 런타임 구현으로 Tx/Rx 시작/정지, 타이머, 수신 통지, 송신 이벤트를 처리한다.

### 1. .c/.h 파일 이름

- `il.c`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 59
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 36

#### 주요 매크로
- L308 `V_IL_VECTOR_COMPONENT_SOURCE`
- L377 `EnableRx(channel)` = `(Il_ChannelState((channel)) = (vuint8)(((vuint8)(Il_ChannelState((channel)) & kIlIsNotRxWait)) | kIlIsRxRun))`
- L379 `EnableTx(channel)` = `(Il_ChannelState((channel)) = (vuint8)(((vuint8)(Il_ChannelState((channel)) & kIlIsNotTxWait)) | kIlIsTxRun))`
- L381 `WaitRx(channel)` = `(Il_ChannelState((channel)) = (vuint8)(((vuint8)(Il_ChannelState((channel)) & kIlIsNotRxRun)) | kIlIsRxWait))`
- L383 `WaitTx(channel)` = `(Il_ChannelState((channel)) = (vuint8)(((vuint8)(Il_ChannelState((channel)) & kIlIsNotTxRun)) | kIlIsTxWait))`
- L385 `SuspendRx(channel)` = `(Il_ChannelState((channel)) = (vuint8)(((vuint8)(Il_ChannelState((channel)) & kIlIsNotRxRun)) & kIlIsNotRxWait))`
- L387 `SuspendTx(channel)` = `(Il_ChannelState((channel)) = (vuint8)(((vuint8)(Il_ChannelState((channel)) & kIlIsNotTxRun)) & kIlIsNotTxWait))`
- L396 `TxEnabled(channel)` = `((Il_ChannelState((channel)) & kIlIsTxRun) != 0)`
- L398 `RxWaiting(channel)` = `((Il_ChannelState((channel)) & kIlIsRxWait) != 0)`
- L400 `TxWaiting(channel)` = `((Il_ChannelState((channel)) & kIlIsTxWait) != 0)`
- L402 `TxSuspended(channel)` = `((Il_ChannelState((channel)) & (kIlIsTxRun | kIlIsTxWait)) == 0)`
- L404 `TxNotSuspended(channel)` = `((Il_ChannelState((channel)) & (kIlIsTxRun | kIlIsTxWait)) != 0)`
- L406 `RxNotSuspended(channel)` = `((Il_ChannelState((channel)) & (kIlIsRxRun | kIlIsRxWait)) != 0)`
- L421 `IlSetChannelReceived(channel)` = `(ilChannelReceive[(channel)] = 0x01)`
- L422 `IlGetChannelReceived(channel)` = `(ilChannelReceive[(channel)])`
- L423 `IlClrChannelReceived(channel)` = `(ilChannelReceive[(channel)] = 0x00)`
- L425 `IlSetChannelReceived(channel)` = `(ilChannelReceive =  0x01)`
- L426 `IlGetChannelReceived(channel)` = `(ilChannelReceive)`
- L427 `IlClrChannelReceived(channel)` = `(ilChannelReceive =  0   )`
- L442 `ResetTxTimeoutFlags(channel)` = `(VStdMemNearClr(&ilTxTimeoutFlags[IlTxTimeoutFlagsStartIndex[(channel)]],(vuint8)(IlTxTimeoutFlagsStartIndex[(channel)+1...`
- L444 `ResetTxTimeoutFlags(channel)` = `(VStdMemNearClr(ilTxTimeoutFlags,(vuint8)kIlNumberOfTxTimeoutFlags))`
- L447 `ResetTxTimeoutFlags(channel)`
- L452 `ResetConfirmationFlags(channel)` = `(VStdMemNearClr(&ilTxConfirmationFlags[IlTxConfirmationFlagsStartIndex[(channel)]],(vuint8)(IlTxConfirmationFlagsStartIn...`
- L454 `ResetConfirmationFlags(channel)` = `(VStdMemNearClr(ilTxConfirmationFlags,(vuint8)kIlNumberOfTxConfirmationFlags))`
- L457 `ResetConfirmationFlags(channel)`
- L462 `ResetRxTimeoutFlags(channel)` = `(VStdMemNearClr(&ilRxTimeoutFlags[IlRxTimeoutFlagsStartIndex[(channel)]],(vuint8)(IlRxTimeoutFlagsStartIndex[(channel)+1...`
- L464 `ResetRxTimeoutFlags(channel)` = `(VStdMemNearClr(ilRxTimeoutFlags,(vuint8)kIlNumberOfRxTimeoutFlags))`
- L467 `ResetRxTimeoutFlags(channel)`
- L473 `ResetRxTimerFlags(channel)` = `(VStdMemNearClr(&ilRxTimerFlags[IlRxTimerFlagsStartIndex[(channel)]],(vuint8)(IlRxTimerFlagsStartIndex[(channel)+1]-IlRx...`
- L475 `ResetRxTimerFlags(channel)` = `(VStdMemNearClr(ilRxTimerFlags,(vuint8)(kIlNumberOfTimerFlagBytes)))`
- L478 `ResetRxTimerFlags(channel)`
- L484 `ResetIndicationFlags(channel)` = `(VStdMemNearClr(&ilRxIndicationFlags[IlRxIndicationFlagsStartIndex[(channel)]],(vuint8)(IlRxIndicationFlagsStartIndex[(c...`
- L486 `ResetIndicationFlags(channel)` = `(VStdMemNearClr(ilRxIndicationFlags,(vuint8)kIlNumberOfRxIndicationFlags))`
- L489 `ResetIndicationFlags(channel)`
- L494 `ResetFirstvalueFlags(channel)` = `(VStdMemNearClr(&ilRxFirstvalueFlags[IlRxFirstvalueFlagsStartIndex[(channel)]],(vuint8)(IlRxFirstvalueFlagsStartIndex[(c...`
- L496 `ResetFirstvalueFlags(channel)` = `(VStdMemNearClr(ilRxFirstvalueFlags,(vuint8)kIlNumberOfRxFirstvalueFlags))`
- L499 `ResetFirstvalueFlags(channel)`
- L504 `ResetDataChangedFlags(channel)` = `(VStdMemNearClr(&ilRxDataChangedFlags[IlRxDataChangedFlagsStartIndex[(channel)]],(vuint8)(IlRxDataChangedFlagsStartIndex...`
- L506 `ResetDataChangedFlags(channel)` = `(VStdMemNearClr(ilRxDataChangedFlags,(vuint8)kIlNumberOfRxDataChangedFlags))`
- L509 `ResetDataChangedFlags(channel)`
- L528 `IlGetRxDataPtr(ilRxHnd)` = `(IlRxDataPtr[(ilRxHnd)])`
- L530 `IlGetRxDataPtr(ilRxHnd)` = `(CanGetRxDataPtr((ilRxHnd)))`
- L541 `IlGetRxDataLen(ilRxHnd)` = `(IlRxDataLen[(ilRxHnd)])`
- L543 `IlGetRxDataLen(ilRxHnd)` = `(CanGetRxDataLen((ilRxHnd)))`
- L557 `IlGetStartRxTimeoutCounter(ilRxHnd)` = `(ilRxDynamicTimeoutCounter[(ilRxHnd)])`
- L560 `IlGetStartRxTimeoutCounter(ilRxHnd)` = `(IlRxTimeoutTbl[V_ACTIVE_IDENTITY_LOG][(ilRxHnd)])`
- L562 `IlGetStartRxTimeoutCounter(ilRxHnd)` = `(IlRxTimeoutTbl[(ilRxHnd)])`
- L574 `IlGetInitRxTimeoutCounter(ilRxHnd)` = `(IlRxTimeoutTbl[V_ACTIVE_IDENTITY_LOG][(ilRxHnd)])`
- L576 `IlGetInitRxTimeoutCounter(ilRxHnd)` = `(IlRxTimeoutTbl[(ilRxHnd)])`
- L588 `IlGetTxTimeout(channel)` = `(IlTxTimeout[channel])`
- L590 `IlGetTxTimeout(channel)` = `(kIlTxTimeout)`
- L593 `IlGetTxTimeout(channel)` = `(kIlTxTimeout)`
- L604 `IlGetTxCyclicCycles(ilTxHnd)` = `(ilTxDynCyclicCycles[(ilTxHnd)])`
- L606 `IlGetTxCyclicCycles(ilTxHnd)` = `(IlTxCyclicCycles[(ilTxHnd)])`
- L618 `IlAssert(ilCondition, ilErrorCode)` = `if ((ilCondition)==0) {(ApplIlFatalError((ilErrorCode)));} /* PRQA S 3412  *//* MD_CBD_19.4 */`
- L620 `IlAssert(ilCondition, ilErrorCode)`
- L2033 `canRxHnd` = `(rcvObject)`
- L2037 `canRxHnd` = `(rcvObject)`
- L2041 `canRxHnd` = `(rxStruct->Handle)`

#### 주요 typedef/구조체
- 없음

#### 주요 전역 변수/테이블 선언
- L634 `V_MEMRAM0 static V_MEMRAM1 vuintx V_MEMRAM2 ilECUMask;`
- L642 `V_MEMRAM0 static V_MEMRAM1 vuint8 V_MEMRAM2 ilChannelReceive[kIlNumberOfChannels];`
- L644 `V_MEMRAM0 static V_MEMRAM1 vuint8 V_MEMRAM2 ilChannelReceive;`
- L656 `V_MEMRAM0 static V_MEMRAM1 IltTxCounter V_MEMRAM2 ilTxEventCounter[kIlNumberOfTxObjects];`
- L659 `V_MEMRAM0 static V_MEMRAM1 IltTxCounter V_MEMRAM2 ilTxCyclicCounter[kIlNumberOfTxObjects];`
- L663 `V_MEMRAM0 static V_MEMRAM1 IltTxCounter V_MEMRAM2 ilTxDynCyclicCycles[kIlNumberOfTxObjects];`
- L672 `V_MEMRAM0 V_MEMRAM1 vuint8 V_MEMRAM2 ilChannelState[kIlNumberOfChannels];`
- L674 `V_MEMRAM0 V_MEMRAM1 vuint8 V_MEMRAM2 ilChannelState;`
- L681 `V_MEMRAM0 V_MEMRAM1 IltRxTimeoutCounter V_MEMRAM2 ilRxTimeoutCounter[kIlNumberOfRxTimeoutCounters];`
- L683 `V_MEMRAM0 V_MEMRAM1 IltRxTimeoutCounter V_MEMRAM2 ilRxDynamicTimeoutCounter[kIlNumberOfRxTimeoutCounters];`
- L686 `V_MEMRAM0 V_MEMRAM1_NEAR vuint8 V_MEMRAM2_NEAR ilRxTimerFlags[kIlNumberOfTimerFlagBytes];`
- L691 `V_MEMRAM0 V_MEMRAM1_NEAR vuint8 V_MEMRAM2_NEAR ilRxIndicationFlags[kIlNumberOfRxIndicationFlags];`
- L696 `V_MEMRAM0 V_MEMRAM1_NEAR vuint8 V_MEMRAM2_NEAR ilRxFirstvalueFlags[kIlNumberOfRxFirstvalueFlags];`
- L701 `V_MEMRAM0 V_MEMRAM1_NEAR vuint8 V_MEMRAM2_NEAR ilRxTimeoutFlags[kIlNumberOfRxTimeoutFlags];`
- L706 `V_MEMRAM0 V_MEMRAM1_NEAR vuint8 V_MEMRAM2_NEAR ilRxDataChangedFlags[kIlNumberOfRxDataChangedFlags];`
- L710 `V_MEMRAM0 V_MEMRAM1_NEAR _ilModeIndicationFlags V_MEMRAM2_NEAR ilModeIndicationFlags;`
- L717 `V_MEMRAM0 V_MEMRAM1_NEAR volatile vuint8 V_MEMRAM2_NEAR ilTxState[kIlNumberOfTxObjects];`
- L719 `V_MEMRAM0 V_MEMRAM1 IltTxUpdateCounter V_MEMRAM2 ilTxUpdateCounter[kIlNumberOfTxObjects];`
- L722 `V_MEMRAM0 V_MEMRAM1 IltTxTimeoutCounter V_MEMRAM2 ilTxTimeoutCounter[kIlNumberOfTxTimeoutCounters];`
- L725 `V_MEMRAM0 V_MEMRAM1 IltTxRepetitionCounter V_MEMRAM2 ilTxNSendCounter[kIlNumberOfTxObjects];`
- L731 `V_MEMRAM0 V_MEMRAM1_NEAR vuint8 V_MEMRAM2_NEAR ilTxTimeoutFlags[kIlNumberOfTxTimeoutFlags];`
- L736 `V_MEMRAM0 V_MEMRAM1_NEAR vuint8 V_MEMRAM2_NEAR ilTxConfirmationFlags[kIlNumberOfTxConfirmationFlags];`
- L801 `V_MEMROM1 vuint8 V_MEMROM2 V_MEMROM3 *src = IlRxDefaultInitValue[ilRxHnd];`
- L835 `IL_MEMROM1 vuint8 IL_MEMROM2 IL_MEMROM3 *src = IlTxDefaultInitValue[ilTxHnd];`
- L923 `CanChannelHandle channel;`
- L1539 `CanReceiveHandle canRxHnd;`
- L1593 `vuint8 ilRxPackedIndex;`
- L1594 `vuint8 ilRxPackedMask = 0x01;`
- L1611 `vuint8 ilBitMask = 0xFF;`
- L1612 `vuint8 ilFlagMask;`
- L1693 `CanTransmitHandle cth;`
- L2309 `vuint8 rc;`
- L2445 `CanChannelHandle channel;`
- L2451 `vuintx i;`
- L2555 `CanChannelHandle channel;`
- L2561 `vuintx i;`

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 53

- L758 `IlSendMessage(IlTransmitHandle ilTxHnd)` [선언]
- L772 `SetRxDefaultWithMask(IlReceiveHandle ilRxHnd, V_MEMROM1 vuint8 V_MEMROM2 V_MEMROM3 * msk)` [선언]
- L786 `SetTxDefaultWithMask(IlTransmitHandle ilTxHnd, IL_MEMROM1 vuint8 IL_MEMROM2 IL_MEMROM3 * msk)` [선언]
- L797 `SetRxDefaultWithMask(IlReceiveHandle ilRxHnd, V_MEMROM1 vuint8 V_MEMROM2 V_MEMROM3 * msk)` [정의]
- L799 `IlGetRxDataPtr(ilRxHnd)` [선언]
- L800 `IlGetRxDataLen(ilRxHnd)` [선언]
- L824 `SetRxDefaultWithMask() */ #endif #if defined( IL_ENABLE_TX_STOP_DEFAULTVALUE ) || defined( IL_ENABLE_TX_START_DEFAULTVALUE ) /********************************************************************************************************************** SetTxDefaultWithMask **********************************************************************************************************************/ static void SetTxDefaultWithMask(IlTransmitHandle ilTxHnd, IL_MEMROM1 vuint8 IL_MEMROM2 IL_MEMROM3 * msk)` [정의]
- L833 `IlGetTxDataPtr(ilTxHnd)` [선언]
- L834 `IlGetTxDlc(ilTxHnd)` [선언]
- L857 `SetTxDefaultWithMask() */ #endif #if defined ( IL_ENABLE_TX ) /********************************************************************************************************************** IlSendMessage **********************************************************************************************************************/ static void IlSendMessage(IlTransmitHandle ilTxHnd)` [정의]
- L910 `IlSendMessage() */ #endif /* IL_ENABLE_TX */ /********************************************************************************************************************** GLOBAL FUNCTIONS **********************************************************************************************************************/ /********************************************************************************************************************** IlInitPowerOn **********************************************************************************************************************/ void IlInitPowerOn(void)` [정의]
- L929 `IlInitPowerOn() */ /********************************************************************************************************************** IlInit **********************************************************************************************************************/ void IlInit(IL_CHANNEL_ILTYPE_ONLY)` [정의]
- L976 `IlGetRxDataLen(ilRxHnd))` [선언]
- L982 `IlGetRxDataLen(ilRxHnd))` [선언]
- L996 `IlGetInitRxTimeoutCounter(ilRxHnd)` [선언]
- L1043 `IlGetTxDlc(ilTxHnd))` [선언]
- L1050 `IlGetTxDlc(ilTxHnd))` [선언]
- L1099 `IlInit() */ /********************************************************************************************************************** IlRxStart **********************************************************************************************************************/ void IlRxStart(IL_CHANNEL_ILTYPE_ONLY)` [정의]
- L1152 `IlGetStartRxTimeoutCounter(ilRxToHnd)` [선언]
- L1188 `IlRxStart() */ /********************************************************************************************************************** IlTxStart **********************************************************************************************************************/ void IlTxStart(IL_CHANNEL_ILTYPE_ONLY)` [정의]
- L1295 `IlTxStart() */ /********************************************************************************************************************** IlRxStop **********************************************************************************************************************/ void IlRxStop(IL_CHANNEL_ILTYPE_ONLY)` [정의]
- L1347 `IlRxStop() */ /********************************************************************************************************************** IlTxStop **********************************************************************************************************************/ void IlTxStop(IL_CHANNEL_ILTYPE_ONLY)` [정의]
- L1419 `IlRxWait() */ /********************************************************************************************************************** IlTxWait **********************************************************************************************************************/ void IlTxWait(IL_CHANNEL_ILTYPE_ONLY)` [정의]
- L1436 `IlTxWait() */ /********************************************************************************************************************** IlRxRelease **********************************************************************************************************************/ void IlRxRelease(IL_CHANNEL_ILTYPE_ONLY)` [정의]
- L1470 `IlGetStartRxTimeoutCounter(ilRxToHnd)` [선언]
- L1488 `IlRxRelease() */ /********************************************************************************************************************** IlTxRelease **********************************************************************************************************************/ void IlTxRelease(IL_CHANNEL_ILTYPE_ONLY)` [정의]
- L1506 `IlTxRelease() */ /********************************************************************************************************************** IlRxTask **********************************************************************************************************************/ void IlRxTask(IL_CHANNEL_ILTYPE_ONLY)` [정의]
- L1515 `IlRxTask() */ /********************************************************************************************************************** IlRxStateTask **********************************************************************************************************************/ void IlRxStateTask(IL_CHANNEL_ILTYPE_ONLY)` [정의]
- L1554 `IlGetRxIndicationMask(canRxHnd)` [선언]
- L1578 `IlRxStateTask() */ /********************************************************************************************************************** IlRxTimerTask **********************************************************************************************************************/ void IlRxTimerTask(IL_CHANNEL_ILTYPE_ONLY)` [정의]
- L1624 `IlGetStartRxTimeoutCounter(ilRxToHnd)` [선언]
- L1675 `IlRxTimerTask() */ /********************************************************************************************************************** IlInit **********************************************************************************************************************/ void IlTxTask(IL_CHANNEL_ILTYPE_ONLY)` [정의]
- L1684 `IlTxTask() */ /********************************************************************************************************************** IlTxStateTask **********************************************************************************************************************/ void IlTxStateTask(IL_CHANNEL_ILTYPE_ONLY)` [정의]
- L1714 `IlGetIlTxIndirection(ilTxHnd)` [선언]
- L1723 `IlGetTxConfirmationMask(cth)` [선언]
- L1754 `IlGetTxTimeout(IL_CHANNEL_ILPARA_MACRO)` [선언]
- L1775 `IlTxStateTask() */ /********************************************************************************************************************** IlTxTimerTask **********************************************************************************************************************/ void IlTxTimerTask(IL_CHANNEL_ILTYPE_ONLY)` [정의]
- L1858 `IlGetTxCyclicCycles(ilTxHnd)` [선언]
- L1873 `IlGetTxCyclicCycles(ilTxHnd)` [선언]
- L1971 `IlTxTimerTask() */ #if defined ( C_ENABLE_CAN_CANCEL_NOTIFICATION ) /********************************************************************************************************************** IlCanCancelNotification **********************************************************************************************************************/ /** \brief This method is called if a transmit message is deleted (CanCancelTransmit, CanOffline or CanInit) and the update counter will be reset. \param txHandle Handle of the Can Driver tx message. \return none \context The function can be called on task and interupt level. \note The function is called by the Can Driver. Either IlCanCancelNotification is exclusively called or the Can Driver sets a confirmation flag or calls a confirmation function. **********************************************************************************************************************/ void IlCanCancelNotification(CanTransmitHandle txHandle)` [정의]
- L1990 `CanGetChannelOfTxObj(txHandle)` [선언]
- L2088 `IlCanGenericPrecopy() */ #endif /* !IL_TYPE_AUTOSAR && C_ENABLE_GENERIC_PRECOPY */ /********************************************************************************************************************** IlGetStatus **********************************************************************************************************************/ Il_Status IlGetStatus(IL_CHANNEL_ILTYPE_ONLY)` [정의]
- L2097 `IlGetStatus() */ #if defined( IL_ENABLE_TX_SEND_ON_INIT ) /********************************************************************************************************************** IlSendOnInitMsg **********************************************************************************************************************/ void IlSendOnInitMsg(IL_CHANNEL_ILTYPE_ONLY)` [정의]
- L2295 `IlGetTxRepetitionCounter(ilTxHnd)` [선언]
- L2299 `IlSecureEvent() */ #endif #if defined ( IL_ENABLE_RX ) # if defined ( IL_ENABLE_RX_INDICATION_FLAG ) /********************************************************************************************************************** IlGetSignalIndicationFlag **********************************************************************************************************************/ vuint8 IlGetSignalIndicationFlag(IlReceiveFlagHandle ilRxFlagHnd)` [정의]
- L2323 `IlGetSignalIndicationFlag() */ # endif /* ( IL_ENABLE_RX_INDICATION_FLAG ) */ #endif /********************************************************************************************************************** IlResetRxTimeoutFlags **********************************************************************************************************************/ void IlResetRxTimeoutFlags(IL_CHANNEL_ILTYPE_ONLY)` [정의]
- L2340 `IlResetRxTimeoutFlags() */ #if defined ( IL_ENABLE_TX ) && defined( IL_ENABLE_SEND_DIRECT ) /********************************************************************************************************************** IlSendDirect **********************************************************************************************************************/ void IlSendDirect(IL_CHANNEL_ILTYPE_FIRST IlTransmitHandle ilTxHnd)` [정의]
- L2368 `IlGetTxTimeout(IL_CHANNEL_ILPARA_MACRO)` [선언]
- L2376 `IlSendDirect() */ #endif /* ( IL_ENABLE_TX ) && ( IL_ENABLE_SEND_DIRECT ) */ #if defined( VGEN_GENY ) && defined( IL_ENABLE_TX_DYNAMIC_CYCLETIME ) /********************************************************************************************************************** IlSetDynTxCycleTimeRaw **********************************************************************************************************************/ void IlSetDynTxCycleTimeRaw(IlTransmitHandle ilTxHnd, IltTxCounter ilTxTicks)` [정의]
- L2394 `IlSetDynTxCycleTimeRaw() */ #endif /* VGEN_GENY && IL_ENABLE_TX_DYNAMIC_CYCLETIME */ #if defined( VGEN_GENY ) && defined( IL_ENABLE_SYS_TX_REPETITIONS_ARE_ACTIVE_FCT ) /********************************************************************************************************************** IlTxRepetitionsAreActive **********************************************************************************************************************/ Il_Boolean IlTxRepetitionsAreActive(IL_CHANNEL_ILTYPE_ONLY)` [정의]
- L2442 `IlGetModuleContext(tIlModuleContextStructPtr pContext)` [정의]
- L2545 `IlGetModuleContext() */ #endif /* IL_ENABLE_SYS_GET_CONTEXT || VGEN_ENABLE_MDWRAP */ #if defined ( IL_ENABLE_SYS_SET_CONTEXT ) || defined( VGEN_ENABLE_QWRAP ) /********************************************************************************************************************** IlSetModuleContext **********************************************************************************************************************/ vuint8 IlSetModuleContext(tIlModuleContextStructPtr pContext)` [정의]
- L2659 `IlSetModuleContext() */ #endif /* IL_ENABLE_SYS_SET_CONTEXT || VGEN_ENABLE_QWRAP */ /********************************************************************************************************************** CanCopyFromCan **********************************************************************************************************************/ #if defined ( VGEN_CANGEN_VERSION ) && defined ( IL_ENABLE_RX_MODE_SIGNALS ) && !defined ( C_ENABLE_MEMCOPY_SUPPORT ) void CanCopyFromCan(void *dst, CanChipDataPtr src, vuint8 len)` [정의]

### 4. 각 함수별 역할 설명

- `IlSendMessage` (L758, 선언): \brief    This method performs the transmission request to the CAN Driver. \param    ilTxHnd  Handle of the tx message. \return   none \context  The function is called by IlTxTimerTask or IlSendDirect.
- `SetRxDefaultWithMask` (L772, 선언): \brief    This method copies default values to the rx message buffer. \param    ilRxHnd  Handle of the rx message. \param    msk      pointer to a mask array of the rx message. At every position, where the mask bit is set, the default value is set. \return   none \context  The function is called by IlRxStart or IlRxStop.
- `SetTxDefaultWithMask` (L786, 선언): \brief    This method copies default values to the tx message buffer. \param    ilTxHnd  Handle of the tx message. \param    msk      pointer to a mask array of the tx message. At every position, where the mask bit is set, the default value is set. \return   none \context  The function is called by IlTxStart or IlTxStop.
- `SetRxDefaultWithMask` (L797, 정의): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `IlGetRxDataPtr` (L799, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `IlGetRxDataLen` (L800, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `SetRxDefaultWithMask` (L824, 정의): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `IlGetTxDataPtr` (L833, 선언): 생성된 IL 수신/송신 신호 버퍼에서 신호 값을 조립해 반환한다.
- `IlGetTxDlc` (L834, 선언): 생성된 IL 수신/송신 신호 버퍼에서 신호 값을 조립해 반환한다.
- `SetTxDefaultWithMask` (L857, 정의): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `IlSendMessage` (L910, 정의): clear send request flag  -----------------------------------
- `IlInitPowerOn` (L929, 정의): 전원 인가 직후 한 번 수행되는 모듈 전역 초기화를 담당한다.
- `IlGetRxDataLen` (L976, 선언): -- Set receive buffer to default values ---------------------------
- `IlGetRxDataLen` (L982, 선언): -- No default value for this buffer. Set to '0' -------------------
- `IlGetInitRxTimeoutCounter` (L996, 선언): --- initialize dynamic Rx timeout objects ---------------------------------------
- `IlGetTxDlc` (L1043, 선언): -- Set receive buffer to default values ---------------------------
- `IlGetTxDlc` (L1050, 선언): Clear message-transmit data-buffer
- `IlInit` (L1099, 정의): 모듈 런타임 상태, 버퍼, 카운터, 하드웨어/생성 파라미터를 초기화한다.
- `IlGetStartRxTimeoutCounter` (L1152, 선언): Timeout observation starts with first receive of a message
- `IlRxStart` (L1188, 정의): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `IlTxStart` (L1295, 정의): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `IlRxStop` (L1347, 정의): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `IlRxWait` (L1419, 정의): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `IlTxWait` (L1436, 정의): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `IlGetStartRxTimeoutCounter` (L1470, 선언): Timeout observation starts with first receive of a message
- `IlRxRelease` (L1488, 정의): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `IlTxRelease` (L1506, 정의): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `IlRxTask` (L1515, 정의): 주기적으로 호출되어 타이머, 상태 머신, 송수신 대기 작업을 진행한다.
- `IlGetRxIndicationMask` (L1554, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `IlRxStateTask` (L1578, 정의): 주기적으로 호출되어 타이머, 상태 머신, 송수신 대기 작업을 진행한다.
- `IlGetStartRxTimeoutCounter` (L1624, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `IlRxTimerTask` (L1675, 정의): 주기적으로 호출되어 타이머, 상태 머신, 송수신 대기 작업을 진행한다.
- `IlTxTask` (L1684, 정의): 주기적으로 호출되어 타이머, 상태 머신, 송수신 대기 작업을 진행한다.
- `IlGetIlTxIndirection` (L1714, 선언): Go from IL handle to CAN handle
- `IlGetTxConfirmationMask` (L1723, 선언): 송신 완료 통지나 확인 상태를 처리하고 상위 계층 콜백/플래그를 갱신한다.
- `IlGetTxTimeout` (L1754, 선언): start Timeout supervision for this message
- `IlTxStateTask` (L1775, 정의): 주기적으로 호출되어 타이머, 상태 머신, 송수신 대기 작업을 진행한다.
- `IlGetTxCyclicCycles` (L1858, 선언): -- Reload the cycle counter for the cyclic message ----------
- `IlGetTxCyclicCycles` (L1873, 선언): -- Reload the cycle counter for the cyclic message ----------
- `IlTxTimerTask` (L1971, 정의): 주기적으로 호출되어 타이머, 상태 머신, 송수신 대기 작업을 진행한다.
- `CanGetChannelOfTxObj` (L1990, 선언): \brief    This method is called if a transmit message is deleted (CanCancelTransmit, CanOffline or CanInit) and the update counter will be reset. \param    txHandle     Handle of the Can Driver tx message. \return   none \context  The function can be called on task and interupt level. \note     The function is called by the Can Driver. Either IlCanCancelNotification is exclusiv
- `IlCanGenericPrecopy` (L2088, 정의): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `IlGetStatus` (L2097, 정의): 생성된 IL 수신/송신 신호 버퍼에서 신호 값을 조립해 반환한다.
- `IlGetTxRepetitionCounter` (L2295, 선언): 생성된 IL 수신/송신 신호 버퍼에서 신호 값을 조립해 반환한다.
- `IlSecureEvent` (L2299, 정의): --- Start immediately transmission of cyclic event messages -------------
- `IlGetSignalIndicationFlag` (L2323, 정의): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `IlResetRxTimeoutFlags` (L2340, 정의): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `IlGetTxTimeout` (L2368, 선언): start Timeout supervision for this message
- `IlSendDirect` (L2376, 정의): start Timeout supervision for this message
- `IlSetDynTxCycleTimeRaw` (L2394, 정의): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `IlGetModuleContext` (L2442, 정의): 생성된 IL 수신/송신 신호 버퍼에서 신호 값을 조립해 반환한다.
- `IlGetModuleContext` (L2545, 정의): 생성된 IL 수신/송신 신호 버퍼에서 신호 값을 조립해 반환한다.
- `IlSetModuleContext` (L2659, 정의): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.

### 5. 필요시 코드 부연 설명

대표 함수 원형 수준의 코드 맥락이다. 전체 구현은 원본 파일이 크므로 함수명/역할 중심으로 분석했다.

```c
// src/CBD/Il/il.c:758
static void IlSendMessage(IlTransmitHandle ilTxHnd)
```
```c
// src/CBD/Il/il.c:772
static void SetRxDefaultWithMask(IlReceiveHandle ilRxHnd, V_MEMROM1 vuint8 V_MEMROM2 V_MEMROM3 * msk)
```
```c
// src/CBD/Il/il.c:786
static void SetTxDefaultWithMask(IlTransmitHandle ilTxHnd, IL_MEMROM1 vuint8 IL_MEMROM2 IL_MEMROM3 * msk)
```
```c
// src/CBD/Il/il.c:797
static void SetRxDefaultWithMask(IlReceiveHandle ilRxHnd, V_MEMROM1 vuint8 V_MEMROM2 V_MEMROM3 * msk)
```
```c
// src/CBD/Il/il.c:799
vuint8 *dst = IlGetRxDataPtr(ilRxHnd)
```
```c
// src/CBD/Il/il.c:800
vuint8 len = IlGetRxDataLen(ilRxHnd)
```
```c
// src/CBD/Il/il.c:824
} /* End of SetRxDefaultWithMask() */ #endif #if defined( IL_ENABLE_TX_STOP_DEFAULTVALUE ) || defined( IL_ENABLE_TX_START_DEFAULTVALUE ) /********************************************************************************************************************** SetTxDefaultWithMask **********************************************************************************************************************/ static void SetTxDefaultWithMask(IlTransmitHandle ilTxHnd, IL_MEMROM1 vuint8 IL_MEMROM2 IL_MEMROM3 * msk)
```
```c
// src/CBD/Il/il.c:833
vuint8 *dst = IlGetTxDataPtr(ilTxHnd)
```

## 파일이름 : il_def

- 경로: `src/CBD/Il/il_def.h`
- 파일 종류: `.h`
- 파일 크기: 151,160 bytes
- 파일 내용: IL 공통 타입, 매크로, API 선언과 내부 데이터 구조를 정의한다.

### 1. .c/.h 파일 이름

- `il_def.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 127
- typedef/struct/enum 후보 수: 20
- 전역 변수/테이블 선언 후보 수: 76

#### 주요 매크로
- L272 `V_IL_DEF_COMPONENT_HEADER`
- L285 `IL_VECTOR_VERSION` = `0x0507`
- L287 `IL_VECTOR_RELEASE_VERSION` = `0x02`
- L289 `IL_VECTOR_BUILD_VERSION` = `121`
- L296 `IL_OK` = `0`
- L299 `IL_ERROR` = `-3`
- L306 `IL_QUEUED` = `-2`
- L312 `IL_FALSE` = `((Il_Boolean)0)`
- L313 `IL_TRUE` = `((Il_Boolean)1)`
- L320 `kTxNoTxType` = `(vuint8)0x00`
- L322 `kTxSendCyclic` = `(vuint8)0x01`
- L324 `kTxSendCyclicEvent` = `(vuint8)0x02`
- L326 `kTxCheckTimeout` = `(vuint8)0x04`
- L328 `kTxNSendRequest` = `(vuint8)0x08`
- L330 `kTxFastOnStart` = `(vuint8)0x10`
- L332 `kTxQueueInit` = `(vuint8)0x20`
- L334 `kTxSendRequest` = `(vuint8)0x80`
- L337 `kTxNotSendCyclic` = `(vuint8)0xfe`
- L339 `kTxNotSendCyclicEvent` = `(vuint8)0xfd`
- L341 `kTxNotCheckTimeout` = `(vuint8)0xfb`
- L343 `kTxNotNSendRequest` = `(vuint8)0xf7`
- L345 `kTxNotFastOnStart` = `(vuint8)0xef`
- L347 `kTxNotQueueInit` = `(vuint8)0xdf`
- L349 `kTxNotSendRequest` = `(vuint8)0x7f`
- L356 `ILERR_ILLTXMSGHANDLE` = `(vuint8) 0x02`
- L358 `ILERR_ILLRXMSGHANDLE` = `(vuint8) 0x03`
- L360 `ILERR_ILLTXTIMEOUTINDEX` = `(vuint8) 0x12`
- L362 `ILERR_ILLRXTIMEOUTINDEX` = `(vuint8) 0x13`
- L364 `ILERR_ILLCHANNEL` = `(vuint8) 0x17`
- L371 `kIlIsSuspend` = `(vuint8)0x00`
- L373 `kIlIsTxWait` = `(vuint8)0x01`
- L375 `kIlIsTxRun` = `(vuint8)0x02`
- L377 `kIlIsRxWait` = `(vuint8)0x04`
- L379 `kIlIsRxRun` = `(vuint8)0x08`
- L381 `kIlIsRxTxRun` = `(vuint8)0x0A`
- L384 `kIlIsNotSuspend` = `(vuint8)0xff`
- L386 `kIlIsNotTxWait` = `(vuint8)0xfe`
- L388 `kIlIsNotTxRun` = `(vuint8)0xfd`
- L390 `kIlIsNotRxWait` = `(vuint8)0xfb`
- L392 `kIlIsNotRxRun` = `(vuint8)0xf7`
- L394 `kIlIsNotRxTxRun` = `(vuint8)0xf5`
- L404 `kCanTxHandleNotUsed` = `((CanTransmitHandle) 0xFFFFFFFF)`
- L409 `kIlStopUpdateCounterValue` = `((IltTxUpdateCounter)0xFFFFu)`
- L413 `kIlNoTxToutIndirection` = `0x00u`
- L415 `kIlNoFastOnStartDuration` = `0x01u`
- L417 `kIlNoCycleTime` = `0x01u`
- L419 `kIlNoDelayTime` = `0x01u`
- L421 `kIlNoCycleTimeFast` = `0x01u`
- L429 `IL_CHANNEL_ILTYPE_ONLY` = `CanChannelHandle channel`
- L430 `IL_CHANNEL_ILTYPE_FIRST` = `CanChannelHandle channel,`
- L431 `IL_CHANNEL_ILTYPE_LOCAL` = `CanChannelHandle channel;`
- L432 `IL_CHANNEL_ILPARA_ONLY` = `channel`
- L433 `IL_CHANNEL_ILPARA_FIRST` = `channel,`
- L434 `IL_CHANNEL_ILPARA_MACRO` = `channel`
- L435 `IL_CHANNEL_ILPRECOPY_MACRO` = `rxStruct->Channel`
- L437 `IL_CHANNEL_ILTYPE_ONLY` = `void`
- L438 `IL_CHANNEL_ILTYPE_FIRST`
- L439 `IL_CHANNEL_ILTYPE_LOCAL`
- L440 `IL_CHANNEL_ILPARA_ONLY`
- L441 `IL_CHANNEL_ILPARA_FIRST`
- L442 `IL_CHANNEL_ILPARA_MACRO` = `0`
- L443 `IL_CHANNEL_ILPRECOPY_MACRO` = `0`
- L452 `IL_PRETRANSMIT_PARA` = `ctis`
- L453 `IL_PRETRANSMIT_TYPE` = `CanTxInfoStruct`
- L454 `IL_PRETRANSMIT_DATA_PTR(para)` = `para.pChipData`
- L456 `IL_PRETRANSMIT_PARA` = `txObject`
- L457 `IL_PRETRANSMIT_TYPE` = `CanChipDataPtr`
- L458 `IL_PRETRANSMIT_DATA_PTR(para)` = `para`
- L468 `IL_ENABLE_RX_MULTI_ECU_PHYS`
- L470 `IL_DISABLE_RX_MULTI_ECU_PHYS`
- L474 `IL_ENABLE_RX_MULTIPLE_NODES`
- L475 `IL_ENABLE_RX_MULTI_ECU_PHYS`
- L476 `IL_ENABLE_SYS_MULTI_ECU_PHYS`
- L478 `tVIdentityMsk` = `vuint8`
- L479 `IlRxMsgECUMask` = `IlCanRxIdentityAssignment`
- L480 `IlTxMsgECUMask` = `IlTxIdentityAssignment`
- L481 `kIlNumberOfIdentities` = `kIlNumberOfNodes`
- L482 `V_ACTIVE_IDENTITY_LOG` = `comMultipleECUCurrent`
- L483 `V_ACTIVE_IDENTITY_MSK` = `ilECUMask`
- L486 `IL_DISABLE_RX_MULTI_ECU_PHYS`
- L496 `IL_MEMROM1` = `V_MEMROM1`
- L497 `IL_MEMROM2` = `V_MEMROM2`
- L498 `IL_MEMROM3` = `V_MEMROM3`
- L521 `CANGENEXE_VERSION` = `0x0000u`
- L522 `CANGENEXE_RELEASE_VERSION` = `0x00u`
- L540 `IL_VECTORDLL_VERSION` = `0x0000u`
- L541 `IL_VECTORDLL_RELEASE_VERSION` = `0x00u`
- L1145 `IlGetTxDataPtr(ilTxHnd)` = `(IlTxDataPtr[(ilTxHnd)])`
- L1147 `IlGetTxDataPtr(ilTxHnd)` = `(CanGetTxDataPtr(IlGetIlTxIndirection((ilTxHnd))))`
- L1158 `IlGetTxDlc(ilTxHnd)` = `(IlTxDLC[(ilTxHnd)])`
- L1160 `IlGetTxDlc(ilTxHnd)` = `(XT_TX_DLC(CanGetTxDlc(IlGetIlTxIndirection((ilTxHnd)))))`
- L1172 `IlGetIlTxIndirection(ilTxHnd)` = `(IlTxIndirection[V_ACTIVE_IDENTITY_LOG][(ilTxHnd)])`
- L1174 `IlGetIlTxIndirection(ilTxHnd)` = `(IlTxIndirection[(ilTxHnd)])`
- L1187 `IlGetRxMsgStartIndex(channel)` = `(IlRxMsgStartIndex[channel])`
- L1189 `IlGetRxMsgStartIndex(channel)` = `0`
- L1201 `IlGetTxMsgStartIndex(channel)` = `(IlTxMsgStartIndex[channel])`
- L1203 `IlGetTxMsgStartIndex(channel)` = `0`
- L1216 `Il_ChannelState(channel)` = `(ilChannelState[(channel)])`
- L1218 `Il_ChannelState(channel)` = `(ilChannelState)`
- L1231 `IlGetTxRepetitionCounter(ilTxHnd)` = `(IlTxRepetitionCounters[ilTxHnd])`
- L1233 `IlGetTxRepetitionCounter(ilTxHnd)` = `((IltTxRepetitionCounter) kIlNumberOfTxRepetitions)`
- L1236 `IlGetTxRepetitionCounter(ilTxHnd)` = `((IltTxRepetitionCounter) kIlNumberOfTxRepetitions)`
- L1251 `RxSuspended(channel)` = `((Il_ChannelState((channel)) & (kIlIsRxRun | kIlIsRxWait))==0)`
- L1253 `RxEnabled(channel)` = `((Il_ChannelState((channel)) & kIlIsRxRun) != 0)`
- L1268 `IlIsTxRun(state)` = `(((state) & kIlIsTxRun)               ? 1 : 0)`
- L1270 `IlIsTxWait(state)` = `(((state) & kIlIsTxWait)              ? 1 : 0)`
- L1272 `IlIsTxSuspend(state)` = `(((state) & (kIlIsTxWait|kIlIsTxRun)) ? 0 : 1)`
- L1274 `IlIsRxRun(state)` = `(((state) & kIlIsRxRun)               ? 1 : 0)`
- L1276 `IlIsRxWait(state)` = `(((state) & kIlIsRxWait)              ? 1 : 0)`
- L1278 `IlIsRxSuspend(state)` = `(((state) & (kIlIsRxWait|kIlIsRxRun)) ? 0 : 1)`
- L1283 `IlEnterCriticalFlagAccess()` = `CanGlobalInterruptDisable()`
- L1285 `IlLeaveCriticalFlagAccess()` = `CanGlobalInterruptRestore()`
- L1296 `IlConvertMs2RxToutCnt(ilChannelCycles, msTime)` = `((IltRxTimeoutCounter) ((msTime) / (ilChannelCycles)))`
- L1305 `IlConvertRxToutCnt2Ms(ilChannelCycles, ilRxTimeoutHnd)` = `(ilRxDynamicTimeoutCounter[(ilRxTimeoutHnd)] * (ilChannelCycles))`
- L1317 `IlConvertChannelCyclesAndMs2TxTicks(ilChannelCycles, ilMsTime)` = `((IltTxCounter) ((ilMsTime) / (ilChannelCycles)))`
- L1341 `IlSetDynTxCycleTimeMs(ilChannelCycles, ilTxHnd, ilMsTime)` = `IlSetDynTxCycleTimeRaw((ilTxHnd), IlConvertChannelCyclesAndMs2TxTicks((ilChannelCycles), (ilMsTime)))`
- L1343 `IlSetDynTxCycleTimeMs(ilTxHnd, ilMsTime)` = `IlSetDynTxCycleTimeRaw((ilTxHnd), IlConvertChannelCyclesAndMs2TxTicks((kIlTxCycleTime), (ilMsTime)))`
- L1352 `IlGetTxConfirmationFlags(x)` = `(CanConfirmationFlags._c[x])`
- L1353 `IlGetTxConfirmationMask(x)` = `(CanGetConfirmationMask(x))`
- L1354 `IlGetTxConfirmationOffset(x)` = `(CanGetConfirmationOffset(x))`
- ... 생략 7개 (동일 패턴의 생성 심볼)

#### 주요 typedef/구조체
- L1367 `typedef void (*IlTxModeCopyFct) (void);`
- L1368 `typedef void (*IlIndicationFct) (void);`
- L1369 `typedef void (*IlTimeoutIndicationFct) (void);`
- L1370 `typedef void (*IlConfirmationFct) (void);`
- L1371 `typedef vsint8 Il_Status;`
- L1372 `typedef vsint8 Il_Boolean;`
- L1376 `typedef vuint8 IlTransmitHandle;`
- L1379 `typedef vuint16 IlTransmitHandle;`
- L1386 `typedef vuint8 IlReceiveHandle;`
- L1389 `typedef vuint16 IlReceiveHandle;`
- L1397 `typedef vuint8 IlReceiveFlagHandle;`
- L1400 `typedef vuint16 IlReceiveFlagHandle;`
- L1409 `typedef vuint8 IltTxRepetitionCounter;`
- L1411 `typedef CanReceiveHandle IlReceiveHandle;`
- L1412 `typedef CanTransmitHandle IlTransmitHandle;`
- L1413 `typedef CanReceiveHandle IlReceiveFlagHandle;`
- L1419 `typedef vuint8 IlRxTimeoutHandle;`
- L1422 `typedef vuint16 IlRxTimeoutHandle;`
- L1435 `typedef struct`
- L1514 `typedef tIlModuleContextStruct *tIlModuleContextStructPtr;`

#### 주요 전역 변수/테이블 선언
- L1437 `vuint32 ilMagicNumber;`
- L1440 `vuintx ilECUMask;`
- L1444 `vuint8 ilChannelState[kIlNumberOfChannels];`
- L1446 `vuint8 ilChannelReceive[kIlNumberOfChannels];`
- L1449 `vuint8 ilChannelState;`
- L1451 `vuint8 ilChannelReceive;`
- L1456 `vuint8 ilTxState[kIlNumberOfTxObjects];`
- L1470 `vuint8 ilTxTimeoutFlags[kIlNumberOfTxTimeoutFlags];`
- L1473 `vuint8 ilTxConfirmationFlags[kIlNumberOfTxConfirmationFlags];`
- L1489 `vuint8 ilRxTimerFlags[kIlNumberOfTimerFlagBytes];`
- L1495 `vuint8 ilRxDataChangedFlags[kIlNumberOfRxDataChangedFlags];`
- L1498 `vuint8 ilRxIndicationFlags[kIlNumberOfRxIndicationFlags];`
- L1501 `vuint8 ilRxFirstvalueFlags[kIlNumberOfRxFirstvalueFlags];`
- L1504 `vuint8 ilRxTimeoutFlags[kIlNumberOfRxTimeoutFlags];`
- L1523 `V_MEMRAM0 extern V_MEMRAM1 vuint8 V_MEMRAM2 ilChannelState[kIlNumberOfChannels];`
- L1525 `V_MEMRAM0 extern V_MEMRAM1 vuint8 V_MEMRAM2 ilChannelState;`
- L1533 `V_MEMRAM0 extern V_MEMRAM1 IltRxTimeoutCounter V_MEMRAM2 ilRxTimeoutCounter[kIlNumberOfRxTimeoutCounters];`
- L1537 `V_MEMRAM0 extern V_MEMRAM1 IltRxTimeoutCounter V_MEMRAM2 ilRxDynamicTimeoutCounter[kIlNumberOfRxTimeoutCounters];`
- L1542 `V_MEMRAM0 extern V_MEMRAM1_NEAR vuint8 V_MEMRAM2_NEAR ilRxTimerFlags[kIlNumberOfTimerFlagBytes];`
- L1548 `V_MEMRAM0 extern V_MEMRAM1_NEAR vuint8 V_MEMRAM2_NEAR ilRxIndicationFlags[kIlNumberOfRxIndicationFlags];`
- L1554 `V_MEMRAM0 extern V_MEMRAM1_NEAR vuint8 V_MEMRAM2_NEAR ilRxFirstvalueFlags[kIlNumberOfRxFirstvalueFlags];`
- L1560 `V_MEMRAM0 extern V_MEMRAM1_NEAR vuint8 V_MEMRAM2_NEAR ilRxTimeoutFlags[kIlNumberOfRxTimeoutFlags];`
- L1566 `V_MEMRAM0 extern V_MEMRAM1_NEAR vuint8 V_MEMRAM2_NEAR ilRxDataChangedFlags[kIlNumberOfRxDataChangedFlags];`
- L1572 `V_MEMRAM0 extern V_MEMRAM1_NEAR _ilModeIndicationFlags V_MEMRAM2_NEAR ilModeIndicationFlags;`
- L1591 `V_MEMRAM0 extern V_MEMRAM1_NEAR volatile vuint8 V_MEMRAM2_NEAR ilTxState[kIlNumberOfTxObjects];`
- L1594 `V_MEMRAM0 extern V_MEMRAM1 IltTxUpdateCounter V_MEMRAM2 ilTxUpdateCounter[kIlNumberOfTxObjects];`
- L1598 `V_MEMRAM0 extern V_MEMRAM1 IltTxTimeoutCounter V_MEMRAM2 ilTxTimeoutCounter[kIlNumberOfTxTimeoutCounters];`
- L1602 `V_MEMRAM0 extern V_MEMRAM1 IltTxRepetitionCounter V_MEMRAM2 ilTxNSendCounter[kIlNumberOfTxObjects];`
- L1611 `V_MEMRAM0 extern V_MEMRAM1_NEAR IlTIfActiveFlag V_MEMRAM2_NEAR ilIfActiveFlags;`
- L1614 `V_MEMRAM0 extern V_MEMRAM1_NEAR _ilIfActiveFlags V_MEMRAM2_NEAR ilIfActiveFlags;`
- L1621 `V_MEMRAM0 extern V_MEMRAM1_NEAR vuint8 V_MEMRAM2_NEAR ilTxTimeoutFlags[kIlNumberOfTxTimeoutFlags];`
- L1626 `V_MEMRAM0 extern V_MEMRAM1_NEAR vuint8 V_MEMRAM2_NEAR ilTxConfirmationFlags[kIlNumberOfTxConfirmationFlags];`
- L1631 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 kIlMainVersion;`
- L1633 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 kIlSubVersion;`
- L1635 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 kIlReleaseVersion;`
- L1641 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 IlRxDataLen[kIlNumberOfRxObjects];`
- L1643 `V_MEMROM0 extern V_MEMROM1 RxDataPtr V_MEMROM2 IlRxDataPtr[kIlNumberOfRxObjects];`
- L1650 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 CanRxDataLen[kCanNumberOfRxObjects];`
- L1655 `V_MEMROM0 extern V_MEMROM1 RxDataPtr V_MEMROM2 CanRxDataPtr[kCanNumberOfRxObjects];`
- L1680 `V_MEMROM0 extern V_MEMROM1 IlIndicationFct V_MEMROM2 IlCanRxIndicationFctPtr[kIlCanNumberOfRxObjects];`
- L1685 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 IlIndicationOffset[kIlNumberOfRxIndicationBits];`
- L1687 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 IlIndicationMask[kIlNumberOfRxIndicationBits];`
- L1692 `V_MEMROM0 extern V_MEMROM1 IlTimeoutIndicationFct V_MEMROM2 IlRxTimeoutFctPtr[kIlNumberOfRxTimeoutCounters];`
- L1699 `V_MEMROM0 extern V_MEMROM1 IltRxTimeoutCounter V_MEMROM2 IlRxTimeoutTbl[kIlNumberOfRxTimeoutCounters];`
- L1707 `V_MEMROM0 extern V_MEMROM1 tVIdentityMsk V_MEMROM2 IlCanRxIdentityAssignment[kIlCanNumberOfRxObjects];`
- L1713 `V_MEMROM0 extern V_MEMROM1 tVIdentityMsk V_MEMROM2 IlCanRxIdentityAssignment[kCanNumberOfRxObjects];`
- L1720 `V_MEMROM0 extern V_MEMROM1 IlReceiveHandle V_MEMROM2 IlRxMsgStartIndex[kIlNumberOfChannels + 1];`
- L1724 `V_MEMROM0 extern V_MEMROM1 CanReceiveHandle V_MEMROM2 IlCanRxMsgStartIndex[kIlNumberOfChannels + 1];`
- L1729 `V_MEMROM0 extern V_MEMROM1 IlReceiveHandle V_MEMROM2 IlRxTimeoutCntStartIndex[kIlNumberOfChannels + 1];`
- L1734 `V_MEMROM0 extern V_MEMROM1 IlReceiveHandle V_MEMROM2 IlRxTimerFlagsStartIndex[kIlNumberOfChannels + 1];`
- L1739 `V_MEMROM0 extern V_MEMROM1 IlReceiveHandle V_MEMROM2 IlRxTimeoutFlagsStartIndex[kIlNumberOfChannels + 1];`
- L1744 `V_MEMROM0 extern V_MEMROM1 IlReceiveHandle V_MEMROM2 IlRxFirstvalueFlagsStartIndex[kIlNumberOfChannels + 1];`
- L1749 `V_MEMROM0 extern V_MEMROM1 IlReceiveHandle V_MEMROM2 IlRxIndicationFlagsStartIndex[kIlNumberOfChannels + 1];`
- L1754 `V_MEMROM0 extern V_MEMROM1 IlReceiveHandle V_MEMROM2 IlRxDataChangedFlagsStartIndex[kIlNumberOfChannels + 1];`
- L1764 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 IlTxDLC[kIlNumberOfTxObjects];`
- L1766 `V_MEMROM0 extern V_MEMROM1 TxDataPtr V_MEMROM2 IlTxDataPtr[kIlNumberOfTxObjects];`
- L1773 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 CanTxDlc[kCanNumberOfTxObjects];`
- L1778 `V_MEMROM0 extern V_MEMROM1 TxDataPtr V_MEMROM2 CanTxDataPtr[kCanNumberOfTxObjects];`
- L1784 `V_MEMROM0 extern V_MEMROM1 IltTxRepetitionCounter V_MEMROM2 IlTxRepetitionCounters[kIlNumberOfTxObjects];`
- L1806 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 IlTxType[kIlNumberOfTxObjects];`
- L1810 `V_MEMROM0 extern V_MEMROM1 IltTxCounter V_MEMROM2 IlTxCyclicCycles[kIlNumberOfTxObjects];`
- L1813 `V_MEMROM0 extern V_MEMROM1 IltTxCounter V_MEMROM2 IlTxStartCycles[kIlNumberOfTxObjects];`
- L1816 `V_MEMROM0 extern V_MEMROM1 IltTxUpdateCounter V_MEMROM2 IlTxUpdateCycles[kIlNumberOfTxObjects];`
- L1820 `V_MEMROM0 extern V_MEMROM1 IltTxCounter V_MEMROM2 IlTxEventCycles[kIlNumberOfTxObjects];`
- L1826 `V_MEMROM0 extern V_MEMROM1 IltTxCounter V_MEMROM2 IlTxFastOnStartDuration[kIlNumberOfTxObjects];`
- L1832 `V_MEMROM0 extern V_MEMROM1 IltTxCounter V_MEMROM2 IlTxFastOnStartMuxDelay[kIlNumberOfTxObjects];`
- L1837 `V_MEMROM0 extern V_MEMROM1 IlConfirmationFct V_MEMROM2 IlTxConfirmationFctPtr[kIlNumberOfTxObjects];`
- L1843 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 IlTxTimeoutIndirection[kIlNumberOfTxObjects];`
- L1852 `V_MEMROM0 extern V_MEMROM1 CanTransmitHandle V_MEMROM2 IlTxIndirection[kIlNumberOfTxObjects];`
- L1857 `V_MEMROM0 extern V_MEMROM1 IlTxModeCopyFct V_MEMROM2 IlTxModeCopyFunctions[kIlNumberOfTxObjects];`
- L1863 `V_MEMROM0 extern V_MEMROM1 tVIdentityMsk V_MEMROM2 IlTxIdentityAssignment[kIlNumberOfTxObjects];`
- L1869 `V_MEMROM0 extern V_MEMROM1 IlTransmitHandle V_MEMROM2 IlTxMsgStartIndex[kIlNumberOfChannels + 1];`
- L1873 `V_MEMROM0 extern V_MEMROM1 IlTransmitHandle V_MEMROM2 IlTxTimeoutCntStartIndex[kIlNumberOfChannels + 1];`
- L1878 `V_MEMROM0 extern V_MEMROM1 IlTransmitHandle V_MEMROM2 IlTxConfirmationFlagsStartIndex[kIlNumberOfChannels + 1];`
- L1883 `V_MEMROM0 extern V_MEMROM1 IlTransmitHandle V_MEMROM2 IlTxTimeoutFlagsStartIndex[kIlNumberOfChannels + 1];`
- L1887 `V_MEMROM0 extern V_MEMROM1 IltTxTimeoutCounter V_MEMROM2 IlTxTimeout[kIlNumberOfChannels];`

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 43

- L1909 `Ccl(Communication Control Layer). **********************************************************************************************************************/ extern void IlInitPowerOn(void)` [선언]
- L1924 `Ccl(Communication Control Layer) or IlInitPowerOn. **********************************************************************************************************************/ extern void IlInit(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L1938 `Nm(Network Management). **********************************************************************************************************************/ extern void IlRxStart(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L1952 `Nm(Network Management). **********************************************************************************************************************/ extern void IlTxStart(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L1967 `Nm(Network Management). **********************************************************************************************************************/ extern void IlRxStop(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L1982 `Nm(Network Management). **********************************************************************************************************************/ extern void IlTxStop(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L1995 `Nm(Network Management). **********************************************************************************************************************/ extern void IlRxWait(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2008 `Nm(Network Management). **********************************************************************************************************************/ extern void IlTxWait(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2021 `Nm(Network Management). **********************************************************************************************************************/ extern void IlRxRelease(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2033 `Nm(Network Management). **********************************************************************************************************************/ extern void IlTxRelease(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2047 `Ccl(Communication Control Layer). **********************************************************************************************************************/ extern void IlRxTask(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2061 `Ccl(Communication Control Layer). **********************************************************************************************************************/ extern void IlTxTask(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2078 `IlRxStateTask(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2093 `IlTxStateTask(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2109 `IlRxTimerTask(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2125 `IlTxTimerTask(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2223 `IlSendOnInitMsg(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2232 `IlIsTxRun(state) : Tx is running \li IlIsTxWait(state) : Tx is waiting \li IlIsTxSuspend(state) : Tx is suspended \li IlIsRxRun(state) : Rx is running \li IlIsRxWait(state) : Rx is waiting \li IlIsRxSuspend(state) : Rx is suspended \context The function can be called on task and interrupt level. \note The function is called by the Application. **********************************************************************************************************************/ extern Il_Status IlGetStatus(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2254 `IlSendDirect(IL_CHANNEL_ILTYPE_FIRST IlTransmitHandle ilTxHnd)` [선언]
- L2268 `IlGetModuleContext(tIlModuleContextStructPtr pContext)` [선언]
- L2277 `IlSetModuleContext(tIlModuleContextStructPtr pContext)` [선언]
- L2299 `ApplIlInit(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2311 `ApplIlRxStart(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2323 `ApplIlTxStart(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2335 `ApplIlRxStop(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2347 `ApplIlTxStop(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2355 `ApplIlFatalError(vuint8 errorNumber)` [선언]
- L2371 `IlGetSignalIndicationFlag(IlReceiveFlagHandle ilRxFlagHnd)` [선언]
- L2399 `IlSetDynTxCycleTimeRaw(IlTransmitHandle ilTxHnd, IltTxCounter ilTxTicks)` [선언]
- L2414 `IlTxRepetitionsAreActive(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2429 `IlTxSignalsAreActive(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2445 `IlResetRxTimeoutFlags(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2462 `CanCopyFromCan(void *dst, CanChipDataPtr src, vuint8 len)` [선언]
- L2476 `InitIfActiveFlags(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2489 `IlResetCanConfirmationFlags(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2502 `IlResetCanIndicationFlags(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2515 `IlResetModeIndicationFlags(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2529 `IlCheckTxTimeout(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2542 `IlSignalInit(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2555 `IlSignalRxStart(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2568 `IlSignalTxStart(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2581 `IlSignalRxStop(IL_CHANNEL_ILTYPE_ONLY)` [선언]
- L2594 `IlSignalTxStop(IL_CHANNEL_ILTYPE_ONLY)` [선언]

### 4. 각 함수별 역할 설명

- `Ccl` (L1909, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `Ccl` (L1924, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `Nm` (L1938, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `Nm` (L1952, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `Nm` (L1967, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `Nm` (L1982, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `Nm` (L1995, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `Nm` (L2008, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `Nm` (L2021, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `Nm` (L2033, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `Ccl` (L2047, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `Ccl` (L2061, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `IlRxStateTask` (L2078, 선언): \brief    This method is called periodically by the IlRxTask. The function can be called in a faster rate than the IlRxTask to check additionally for polled indication events. The usage of the IlRxTask shall be preferred. \param_i  channel : Handle of the logical Can Driver channel. \return   none \context  The function must be called on task level.\n The function must not inte
- `IlTxStateTask` (L2093, 선언): \brief    This method is called periodically by the IlTxTask. The function can be called in a faster rate, than the IlTxTask, to check additionally for polled confirmation events. The usage of the IlTxTask shall be preferred. \param_i  channel : Handle of the logical Can Driver channel. \return   none \context  The function must be called on task level.\n The function must not 
- `IlRxTimerTask` (L2109, 선언): \brief    This method is called periodically by the IlRxTask. If the application separates the handling of notification and timing and the IlRxTask is not used, this function must be called periodically in the Rx task cycle time configured in the generation tool. The usage of the IlRxTask shall be preferred. \param_i  channel : Handle of the logical Can Driver channel. \return 
- `IlTxTimerTask` (L2125, 선언): \brief    This method is called periodically by the IlTxTask. If the application separates the handling of notification and timing and the IlTxTask is not used, this function must be called periodically in the Tx task cycle time configured in the generation tool. The usage of the IlTxTask shall be preferred. \param_i  channel : Handle of the logical Can Driver channel. \return 
- `IlSendOnInitMsg` (L2223, 선언): \brief    This method serves to set a transmission request flag for all messages configured as SendOnInit messages. \param_i  channel : Handle of the logical Can Driver channel. \return   none \context  The function must be called on task level. \note     The function is called by the Application.
- `IlIsTxRun` (L2232, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `IlSendDirect` (L2254, 선언): \brief    Refer to the Application Note AN-ISC-8-1045_IlSendDirect.pdf \param    ilTxHnd  Handle of the Tx message. \param_i  channel : Handle of the logical Can Driver channel. \return   none \warning  Do not use this API, without taking the application note into account.
- `IlGetModuleContext` (L2268, 선언): \brief    This function copies the context of the component to a structure. \param    pContext  pointer to a tIlModuleContextStructPtr context structure. \return   none \pre      The component context must be saved \post     The structure contains the component state \context  The function must be called with disabled interrupts.
- `IlSetModuleContext` (L2277, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `ApplIlInit` (L2299, 선언): \brief    This method is called to indicate the performed initialization. \param_i  channel : Handle of the logical Can Driver channel. \return   none \context  The function is called by the IL in the context of IlInit.
- `ApplIlRxStart` (L2311, 선언): \brief    This method is called to indicate the performed transition start for the Rx state machine. \param_i  channel : Handle of the logical Can Driver channel. \return   none \context  The function is called by the IL in the context of IlRxStart.
- `ApplIlTxStart` (L2323, 선언): \brief    This method is called to indicate the performed transition start for the Tx state machine. \param_i  channel : Handle of the logical Can Driver channel. \return   none \context  The function is called by the IL in the context of IlTxStart.
- `ApplIlRxStop` (L2335, 선언): \brief    This method is called to indicate the performed transition stop for the Rx state machine. \param_i  channel : Handle of the logical Can Driver channel. \return   none \context  The function is called by the IL in the context of IlRxStop.
- `ApplIlTxStop` (L2347, 선언): \brief    This method is called to indicate the performed transition stop for the Tx state machine. \param_i  channel : Handle of the logical Can Driver channel. \return   none \context  The function is called by the IL in the context of IlTxStop.
- `ApplIlFatalError` (L2355, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `IlGetSignalIndicationFlag` (L2371, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `IlSetDynTxCycleTimeRaw` (L2399, 선언): \brief    Sets the cycle time of an Il message. The macro is called from the application. The function has no effect, if the Il message handle is not transmitted cyclically. This is defined in the dbc database file with the attributes GenMsgSendType and GenSigSendType. Please refer to the technical reference for details. To activate the cyclic transmission of an Il message hand
- `IlTxRepetitionsAreActive` (L2414, 선언): \brief    This method can be used to detect if messages with repetitions are queued for transmission on a channel. \param_i  channel : Handle of the logical Can Driver channel. \return   IL_TRUE   : Messages with repetitions are queued for transmission.\n IL_FALSE  : No message with repetitions is queued for transmission. \context  The function can be called on task and interru
- `IlTxSignalsAreActive` (L2429, 선언): \brief    This method can be used to detect if signals are active on a channel. \param_i  channel : Handle of the logical Can Driver channel. \return   IL_TRUE   : Signals are in the active state.\n IL_FALSE  : No signal is in the active state. \context  The function can be called on task and interrupt level. \note     The function is called by the Application. \warning  The fu
- `IlResetRxTimeoutFlags` (L2445, 선언): \brief    This method clears Rx timeout flags of the Application and internal ilNodeCommActiveTimeoutFlags. \param_i  channel : Handle of the logical Can Driver channel. \return   none \context  The function can be called on task and interrupt level. \note     The function is called by the Application. \warning  Do not call this Il internal API from the application!
- `CanCopyFromCan` (L2462, 선언): \brief    This method serves to copy data from the register of the CAN controller to the message buffer. \param    dst  Pointer to the message buffer. \param    src  Pointer to the register of the CAN controller. \param    len  Number of bytes to be copied \return   none \context  The function must be called in the call context of a Can Driver precopy function.
- `InitIfActiveFlags` (L2476, 선언): \brief    This method clears all if active flags of a channel. \param_i  channel : Handle of the logical Can Driver channel. \return   none \note     The function is called by the IlTxStart. \warning  Do not call this Il internal API from the application!
- `IlResetCanConfirmationFlags` (L2489, 선언): \brief    This method clears all Can Driver confirmation flags of IL Tx messages of a channel. \param_i  channel : Handle of the logical Can Driver channel. \return   none \note     The function is called by the IlTxStart. \warning  Do not call this Il internal API from the application!
- `IlResetCanIndicationFlags` (L2502, 선언): \brief    This method clears all Can Driver indication flags of IL Rx messages of a channel. \param_i  channel : Handle of the logical Can Driver channel. \return   none \note     The function is called by the IlRxStart. \warning  Do not call this Il internal API from the application!
- `IlResetModeIndicationFlags` (L2515, 선언): \brief    This method clears IL internal mode indication flags of a channel. \param_i  channel : Handle of the logical Can Driver channel. \return   none \note     The function is called by the IlRxStart. \warning  Do not call this Il internal API from the application!
- `IlCheckTxTimeout` (L2529, 선언): \brief    This method checks the IL internal Tx timeout flags, decrements Tx timeout counters, calls Tx timeout callback functions and sets Tx timeout flags of a channel. \param_i  channel : Handle of the logical Can Driver channel. \return   none \note     The function is called by the IlTxTimerTask. \warning  Do not call this Il internal API from the application!
- `IlSignalInit` (L2542, 선언): \brief    This method calls all configured init signal callback functions of a channel. \param_i  channel : Handle of the logical Can Driver channel. \return   none \note     The function is called by IlInit. \warning  Do not call this Il internal API from the application!
- `IlSignalRxStart` (L2555, 선언): \brief    This method calls all configured Rx start signal callback functions of a channel. \param_i  channel : Handle of the logical Can Driver channel. \return   none \note     The function is called by IlRxStart. \warning  Do not call this Il internal API from the application!
- `IlSignalTxStart` (L2568, 선언): \brief    This method calls all configured Tx start signal callback functions of a channel. \param_i  channel : Handle of the logical Can Driver channel. \return   none \note     The function is called by IlTxStart. \warning  Do not call this Il internal API from the application!
- `IlSignalRxStop` (L2581, 선언): \brief    This method calls all configured Rx stop signal callback functions of a channel. \param_i  channel : Handle of the logical Can Driver channel. \return   none \note     The function is called by IlRxStop. \warning  Do not call this Il internal API from the application!
- `IlSignalTxStop` (L2594, 선언): \brief    This method calls all configured Tx stop signal callback functions of a channel. \param_i  channel : Handle of the logical Can Driver channel. \return   none \note     The function is called by IlTxStop. \warning  Do not call this Il internal API from the application!

### 5. 필요시 코드 부연 설명

이 파일은 생성 설정/선언 성격이 강해 별도 코드 전문 인용 없이 심볼 표로 역할을 설명했다.

## 파일이름 : il_inc

- 경로: `src/CBD/Il/il_inc.h`
- 파일 종류: `.h`
- 파일 크기: 7,732 bytes
- 파일 내용: 해당 계층의 생성 코드 또는 보조 헤더이다.

### 1. .c/.h 파일 이름

- `il_inc.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 1
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 0

#### 주요 매크로
- L97 `V_IL_INC_COMPONENT_HEADER`

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
