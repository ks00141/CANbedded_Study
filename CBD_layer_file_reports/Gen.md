# Gen 계층 파일별 상세 분석 보고서

- 분석 대상: `Gen` 계층
- 파일 수: 25
- 생성 시각: 2026-05-28 22:37:38
- 분석 방식: C 전처리 매크로, typedef, 전역 데이터 선언, 함수 선언/정의를 자동 추출하고 함수명/주석/계층 역할을 기준으로 한글 역할 설명을 부여함.

## 계층 요약

Gen 계층은 GENy/CANdelaGen에서 생성된 프로젝트별 설정/파라미터/진단 테이블/신호 접근 코드를 포함한다.

## 파일 목록

- `src/CBD/Gen/appdesc.c` (147,504 bytes)
- `src/CBD/Gen/appdesc.h` (10,367 bytes)
- `src/CBD/Gen/appdescdev.c` (1,879 bytes)
- `src/CBD/Gen/can_cfg.h` (9,491 bytes)
- `src/CBD/Gen/can_par.c` (22,483 bytes)
- `src/CBD/Gen/can_par.h` (34,428 bytes)
- `src/CBD/Gen/ccl_cfg.h` (6,208 bytes)
- `src/CBD/Gen/ccl_par.c` (26,542 bytes)
- `src/CBD/Gen/ccl_par.h` (4,160 bytes)
- `src/CBD/Gen/desc.c` (325,026 bytes)
- `src/CBD/Gen/desc.h` (70,872 bytes)
- `src/CBD/Gen/drv_par.c` (4,282 bytes)
- `src/CBD/Gen/drv_par.h` (17,547 bytes)
- `src/CBD/Gen/il_cfg.h` (7,850 bytes)
- `src/CBD/Gen/il_par.c` (67,094 bytes)
- `src/CBD/Gen/il_par.h` (40,080 bytes)
- `src/CBD/Gen/nmb_cfg.h` (3,618 bytes)
- `src/CBD/Gen/nmb_par.c` (2,611 bytes)
- `src/CBD/Gen/nmb_par.h` (2,543 bytes)
- `src/CBD/Gen/tp_cfg.h` (10,899 bytes)
- `src/CBD/Gen/tp_par.c` (2,077 bytes)
- `src/CBD/Gen/v_cfg.h` (7,974 bytes)
- `src/CBD/Gen/v_inc.h` (2,701 bytes)
- `src/CBD/Gen/v_par.c` (3,669 bytes)
- `src/CBD/Gen/v_par.h` (3,903 bytes)

## 파일이름 : appdesc

- 경로: `src/CBD/Gen/appdesc.c`
- 파일 종류: `.c`
- 파일 크기: 147,504 bytes
- 파일 내용: CANdesc 애플리케이션 콜백 구현으로 ECU별 UDS 서비스 훅을 제공한다.

### 1. .c/.h 파일 이름

- `appdesc.c`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 1
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 5

#### 주요 매크로
- L156 `kDescEcuCryptoKey` = `0x12345678`

#### 주요 typedef/구조체
- 없음

#### 주요 전역 변수/테이블 선언
- L78 `Ktw    Fixed             ESCAN00049396 UDS                            No positive response sent if SPRMIB=TRUE and API DescForceRcrRpResponse is used`
- L174 `static vuint32 g_applDescSeedX;`
- L175 `static vuint32 g_applDescSeedY;`
- L1962 `vuint32 tmpX, tmpY;`
- L1978 `vuint8_least iter;`

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 73

- L153 `constants(defines) ----------------------------------------------------------------------------- */ #define kDescEcuCryptoKey 0x12345678 /* ----------------------------------------------------------------------------- &&&~ Function prototypes ----------------------------------------------------------------------------- */ static vuint32 SecM_ComputeKey(void)` [선언]
- L165 `SecM_Serializer(DescMsg tgtPtr, vuint32 data)` [선언]
- L203 `ApplDescFatalError(vuint8 errorCode, vuint16 lineNumber)` [정의]
- L218 `ApplDescSpontaneousResponseConfirmation(DESC_CONTEXT_FORMAL_PARAM_DEF_FIRST vuint8 status)` [정의]
- L250 `ApplDescRcrRpConfirmation(vuint8 status)` [정의]
- L290 `ApplDescOnTransitionSession(DescStateGroup newState, DescStateGroup formerState)` [정의]
- L319 `ApplDescOnTransitionSecurityAccess(DescStateGroup newState, DescStateGroup formerState)` [정의]
- L364 `ApplDescEcuResetHard(DescMsgContext* pMsgContext)` [정의]
- L406 `ApplDescClearDiagInfo(DescMsgContext* pMsgContext)` [정의]
- L458 `ApplDescReadDtcRNODTCBSM(DescMsgContext* pMsgContext)` [정의]
- L513 `ApplDescReadDtcRDTCBSM(DescMsgContext* pMsgContext)` [정의]
- L568 `ApplDescReadDtcRDTCEDRBDN(DescMsgContext* pMsgContext)` [정의]
- L623 `ApplDescReadDtcRSUPDTC(DescMsgContext* pMsgContext)` [정의]
- L668 `ApplDescIoCtrlFrzCurrStateSupported_check_DID_F010(DescMsgContext* pMsgContext)` [정의]
- L710 `ApplDescIoCtrlRetCtrlToEcuHeat_Step_3_Operation(DescMsgContext* pMsgContext)` [정의]
- L752 `ApplDescIoCtrlShortTermAdjHeat_Step_3_Operation(DescMsgContext* pMsgContext)` [정의]
- L794 `ApplDescIoCtrlRetCtrlToEcuVent_Step_3_Operation(DescMsgContext* pMsgContext)` [정의]
- L836 `ApplDescIoCtrlShortTermAdjVent_Step_3_Operation(DescMsgContext* pMsgContext)` [정의]
- L878 `ApplDescIoCtrlFrzCurrStateSupported_check_DID_F040(DescMsgContext* pMsgContext)` [정의]
- L920 `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Foremost_Position_Recline(DescMsgContext* pMsgContext)` [정의]
- L962 `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Foremost_Position_Recline(DescMsgContext* pMsgContext)` [정의]
- L1004 `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Last_Position_Recline(DescMsgContext* pMsgContext)` [정의]
- L1046 `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Last_Position_Recline(DescMsgContext* pMsgContext)` [정의]
- L1088 `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Foremost_Position_Relax(DescMsgContext* pMsgContext)` [정의]
- L1130 `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Foremost_Position_Relax(DescMsgContext* pMsgContext)` [정의]
- L1172 `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Last_Position_Relax(DescMsgContext* pMsgContext)` [정의]
- L1214 `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Last_Position_Relax(DescMsgContext* pMsgContext)` [정의]
- L1256 `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Foremost_Position_Legrest(DescMsgContext* pMsgContext)` [정의]
- L1298 `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Foremost_Position_Legrest(DescMsgContext* pMsgContext)` [정의]
- L1340 `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Last_Position_Legrest(DescMsgContext* pMsgContext)` [정의]
- L1382 `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Last_Position_Legrest(DescMsgContext* pMsgContext)` [정의]
- L1424 `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Foremost_Position_LegExt(DescMsgContext* pMsgContext)` [정의]
- L1466 `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Foremost_Position_LegExt(DescMsgContext* pMsgContext)` [정의]
- L1508 `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Last_Position_LegExt(DescMsgContext* pMsgContext)` [정의]
- L1550 `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Last_Position_LegExt(DescMsgContext* pMsgContext)` [정의]
- L1592 `ApplDescControlDtcSettingEnable(DescMsgContext* pMsgContext)` [정의]
- L1634 `ApplDescControlDtcSettingDisable(DescMsgContext* pMsgContext)` [정의]
- L1653 `ApplDescOnCommunicationDisable(void)` [정의]
- L1668 `ApplDescOnCommunicationEnable(void)` [정의]
- L1705 `ApplDescCheckCommCtrl(DESC_CONTEXT_FORMAL_PARAM_DEF_FIRST DescOemCommControlInfo *commControlInfo)` [정의]
- L1783 `ApplDescSetCommMode(DescOemCommControlInfo *commControlInfo)` [정의]
- L1845 `ApplDescSetCommModeOnRxPath(DescOemCommControlInfo *commControlInfo)` [정의]
- L1874 `ApplDescCheckSessionTransition(DescStateGroup newState, DescStateGroup formerState)` [정의]
- L1902 `ApplDescSecurityAccessOnAttCntrChanged(DESC_CONTEXT_FORMAL_PARAM_DEF_FIRST DescSecAccAttCtrChgEvType chgEv, vuint8 newAttCounter)` [정의]
- L1939 `current(not yet changed) value of the counter */ if ( (DescGetAttemptCounterValue(DESC_CONTEXT_PARAM_ONLY) == 0) && (newAttCounter == 1))` [정의]
- L1960 `SecM_ComputeKey(void)` [정의]
- L1976 `SecM_Serializer(DescMsg tgtPtr, vuint32 data)` [정의]
- L2009 `ApplDescSecurityAccessGetSeed(DescSecurityAccessContext* pDescSecurityAccessContext)` [정의]
- L2016 `DescMake32Bit(0x12, 0x34, 0x56, 0x78)` [선언]
- L2017 `DescMake32Bit(0xF1, 0xF2, 0xF3, 0xF4)` [선언]
- L2053 `ApplDescSecurityAccessCheckKey(DescSecurityAccessContext* pDescSecurityAccessContext)` [정의]
- L2062 `SecM_ComputeKey()` [선언]
- L2110 `ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Power_Seat_Switch(DescMsgContext* pMsgContext)` [정의]
- L2155 `ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Power_Seat_Motor_Output(DescMsgContext* pMsgContext)` [정의]
- L2200 `ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Power_Seat_Position(DescMsgContext* pMsgContext)` [정의]
- L2245 `ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Power_Seat_Fail_Detection(DescMsgContext* pMsgContext)` [정의]
- L2290 `ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Heater_RR_NTC_1EA(DescMsgContext* pMsgContext)` [정의]
- L2335 `ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Vent_RR_Blower_2EA(DescMsgContext* pMsgContext)` [정의]
- L2380 `ApplDescReadDidVehicleManufacturerSparePartNumberDataIdentifier(DescMsgContext* pMsgContext)` [정의]
- L2425 `ApplDescReadDidECUManufacturingDateDataIdentifier(DescMsgContext* pMsgContext)` [정의]
- L2470 `ApplDescReadDidECUSerialNumberDataIdentifier(DescMsgContext* pMsgContext)` [정의]
- L2515 `ApplDescReadDidVehicleManufacturerECUHardwareNumberDataIdentifier(DescMsgContext* pMsgContext)` [정의]
- L2560 `ApplDescReadDidSystemSupplierECUHardwareVersionNumberDataIdentifier(DescMsgContext* pMsgContext)` [정의]
- L2605 `ApplDescReadDidECUSupplierCodeDataIdentifier(DescMsgContext* pMsgContext)` [정의]
- L2650 `ApplDescReadDidECUSoftwareUNITnumberDataIdentifier(DescMsgContext* pMsgContext)` [정의]
- L2695 `ApplDescReadDidECUSoftwareUNIT1VersionDataIdentifier(DescMsgContext* pMsgContext)` [정의]
- L2740 `ApplDescReadDidECUSoftwareUnit1IVDDataIdentifier(DescMsgContext* pMsgContext)` [정의]
- L2785 `ApplDescReadDidRXSWINDataidentifier(DescMsgContext* pMsgContext)` [정의]
- L2808 `ApplDescIsDataIdSupported(vuint16 pid)` [정의]
- L2846 `ApplDescRtnCtrlStartOtaReady(DescMsgContext* pMsgContext)` [정의]
- L2891 `ApplDescRtnCtrlStartRoutine_Identifier_Limit_Setting(DescMsgContext* pMsgContext)` [정의]
- L2946 `ApplDescRtnCtrlStopRoutine_Identifier_Limit_Setting(DescMsgContext* pMsgContext)` [정의]
- L3001 `ApplDescRtnCtrlReqResRoutine_Identifier_Limit_Setting(DescMsgContext* pMsgContext)` [정의]

### 4. 각 함수별 역할 설명

- `constants` (L153, 선언): Include the implementation prototypes for prototype checks
- `SecM_Serializer` (L165, 선언): ----------------------------------------------------------------------------- &&&~ Function prototypes -----------------------------------------------------------------------------
- `ApplDescFatalError` (L203, 정의): Function name:ApplDescFatalError Description: This function will be called each time while the debug mode is active a CANdesc fault has been detected. If you reach this function it makes no sence to continue the tests since CANdesc will not operate properly until next start of the ECU. Returns:  nothing Parameter(s): - errorCode: - The assert code text equivalent can be found i
- `ApplDescSpontaneousResponseConfirmation` (L218, 정의): When fatal error occurs, cause an ECU hang up at this point. Please set break point at this line to investigate both parameter values.
- `ApplDescRcrRpConfirmation` (L250, 정의): Function name:ApplDescRcrRpConfirmation Description:Will be called only if "DescForceRcrRpResponse" has been previously called. Returns:  nothing Parameter(s): - status: - Current RCR-RP transmission status (kDescOk/kDescFailed). - Access type: read Particularitie(s) and limitation(s): - The function "DescProcessingDone" may be called to close the service processing. - The func
- `ApplDescOnTransitionSession` (L290, 정의): Function name:ApplDescOnTransitionSession Description:Notification function for state change of the given state group, defined by CANdelaStudio. Returns:  nothing Parameter(s): - newState: - The state which will be set. - Access type: read - formerState: - The current state of this state group. - Access type: read Particularitie(s) and limitation(s): - The function "DescProcess
- `ApplDescOnTransitionSecurityAccess` (L319, 정의): Function name:ApplDescOnTransitionSecurityAccess Description:Notification function for state change of the given state group, defined by CANdelaStudio. Returns:  nothing Parameter(s): - newState: - The state which will be set. - Access type: read - formerState: - The current state of this state group. - Access type: read Particularitie(s) and limitation(s): - The function "Desc
- `ApplDescEcuResetHard` (L364, 정의): Function name:ApplDescEcuResetHard (Service request header:$11 $1 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/write - pMsgContext->reqDataLen: - Contains the 
- `ApplDescClearDiagInfo` (L406, 정의): Function name:ApplDescClearDiagInfo (Service request header:$14 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/write - pMsgContext->reqDataLen: - Contains the co
- `ApplDescReadDtcRNODTCBSM` (L458, 정의): Function name:ApplDescReadDtcRNODTCBSM (Service request header:$19 $1 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/write - pMsgContext->reqDataLen: - Contains 
- `ApplDescReadDtcRDTCBSM` (L513, 정의): Function name:ApplDescReadDtcRDTCBSM (Service request header:$19 $2 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/write - pMsgContext->reqDataLen: - Contains th
- `ApplDescReadDtcRDTCEDRBDN` (L568, 정의): Function name:ApplDescReadDtcRDTCEDRBDN (Service request header:$19 $6 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/write - pMsgContext->reqDataLen: - Contains
- `ApplDescReadDtcRSUPDTC` (L623, 정의): Function name:ApplDescReadDtcRSUPDTC (Service request header:$19 $A ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/write - pMsgContext->reqDataLen: - Contains th
- `ApplDescIoCtrlFrzCurrStateSupported_check_DID_F010` (L668, 정의): Function name:ApplDescIoCtrlFrzCurrStateSupported_check_DID_F010 (Service request header:$2F $F0 $10 $2 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/write - pM
- `ApplDescIoCtrlRetCtrlToEcuHeat_Step_3_Operation` (L710, 정의): Function name:ApplDescIoCtrlRetCtrlToEcuHeat_Step_3_Operation (Service request header:$2F $F0 $1C $0 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/write - pMsgC
- `ApplDescIoCtrlShortTermAdjHeat_Step_3_Operation` (L752, 정의): Function name:ApplDescIoCtrlShortTermAdjHeat_Step_3_Operation (Service request header:$2F $F0 $1C $3 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/write - pMsgC
- `ApplDescIoCtrlRetCtrlToEcuVent_Step_3_Operation` (L794, 정의): Function name:ApplDescIoCtrlRetCtrlToEcuVent_Step_3_Operation (Service request header:$2F $F0 $1D $0 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/write - pMsgC
- `ApplDescIoCtrlShortTermAdjVent_Step_3_Operation` (L836, 정의): Function name:ApplDescIoCtrlShortTermAdjVent_Step_3_Operation (Service request header:$2F $F0 $1D $3 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/write - pMsgC
- `ApplDescIoCtrlFrzCurrStateSupported_check_DID_F040` (L878, 정의): Function name:ApplDescIoCtrlFrzCurrStateSupported_check_DID_F040 (Service request header:$2F $F0 $40 $2 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/write - pM
- `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Foremost_Position_Recline` (L920, 정의): Function name:ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Foremost_Position_Recline (Service request header:$2F $F0 $41 $0 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access t
- `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Foremost_Position_Recline` (L962, 정의): Function name:ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Foremost_Position_Recline (Service request header:$2F $F0 $41 $3 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access t
- `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Last_Position_Recline` (L1004, 정의): Function name:ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Last_Position_Recline (Service request header:$2F $F0 $42 $0 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type:
- `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Last_Position_Recline` (L1046, 정의): Function name:ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Last_Position_Recline (Service request header:$2F $F0 $42 $3 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type:
- `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Foremost_Position_Relax` (L1088, 정의): Function name:ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Foremost_Position_Relax (Service request header:$2F $F0 $49 $0 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access typ
- `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Foremost_Position_Relax` (L1130, 정의): Function name:ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Foremost_Position_Relax (Service request header:$2F $F0 $49 $3 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access typ
- `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Last_Position_Relax` (L1172, 정의): Function name:ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Last_Position_Relax (Service request header:$2F $F0 $4A $0 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: r
- `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Last_Position_Relax` (L1214, 정의): Function name:ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Last_Position_Relax (Service request header:$2F $F0 $4A $3 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: r
- `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Foremost_Position_Legrest` (L1256, 정의): Function name:ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Foremost_Position_Legrest (Service request header:$2F $F0 $4D $0 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access t
- `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Foremost_Position_Legrest` (L1298, 정의): Function name:ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Foremost_Position_Legrest (Service request header:$2F $F0 $4D $3 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access t
- `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Last_Position_Legrest` (L1340, 정의): Function name:ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Last_Position_Legrest (Service request header:$2F $F0 $4E $0 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type:
- `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Last_Position_Legrest` (L1382, 정의): Function name:ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Last_Position_Legrest (Service request header:$2F $F0 $4E $3 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type:
- `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Foremost_Position_LegExt` (L1424, 정의): Function name:ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Foremost_Position_LegExt (Service request header:$2F $F0 $4F $0 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access ty
- `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Foremost_Position_LegExt` (L1466, 정의): Function name:ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Foremost_Position_LegExt (Service request header:$2F $F0 $4F $3 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access ty
- `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Last_Position_LegExt` (L1508, 정의): Function name:ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Last_Position_LegExt (Service request header:$2F $F0 $50 $0 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: 
- `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Last_Position_LegExt` (L1550, 정의): Function name:ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Last_Position_LegExt (Service request header:$2F $F0 $50 $3 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: 
- `ApplDescControlDtcSettingEnable` (L1592, 정의): Function name:ApplDescControlDtcSettingEnable (Service request header:$85 $1 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/write - pMsgContext->reqDataLen: - Co
- `ApplDescControlDtcSettingDisable` (L1634, 정의): Function name:ApplDescControlDtcSettingDisable (Service request header:$85 $2 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/write - pMsgContext->reqDataLen: - C
- `ApplDescOnCommunicationDisable` (L1653, 정의): Function name:ApplDescOnCommunicationDisable Description: Notification function that the communication has been disabled. Returns:  none Parameter(s):none Particularitie(s) and limitation(s): - The function "DescProcessingDone" may not be called. - The function "DescSetNegResponse" may not be called. ******************************************************************************
- `ApplDescOnCommunicationEnable` (L1668, 정의): Function name:ApplDescOnCommunicationEnable Description: Notification function that the communication has been restored. Returns:  none Parameter(s):none Particularitie(s) and limitation(s): - The function "DescProcessingDone" may not be called. - The function "DescSetNegResponse" may not be called. *******************************************************************************
- `ApplDescCheckCommCtrl` (L1705, 정의): Function name:ApplDescCheckCommCtrl Description:Check if the requested communication manipulation is possible to be performed by the ECU. Returns:  nothing Parameter(s): - iContext: - Diagnostic request handle used only in multi-context system (kDescNumContexts > 1). - Access type: read - commControlInfo->subNetTxNumber: - The application shall use this parameter to decied on w
- `ApplDescSetCommMode` (L1783, 정의): Function name:ApplDescSetCommMode Description:Manipulate application specific channels (LIN, MOST, etc.) Returns:  nothing Parameter(s): - commControlInfo->subNetTxNumber: - The application shall use this parameter to decied on which physical channels the communiaction will be manipulated. - Access type: read - commControlInfo->commCtrlChannel: - The application determines on w
- `ApplDescSetCommModeOnRxPath` (L1845, 정의): Function name:ApplDescSetCommModeOnRxPath Description: Manipulates only the RX path on CAN. For the other networks (if any) such LIN, MOST, etc. reffer to the ApplDescSetCommMode API. Returns:  nothing Parameter(s): - commControlInfo->subNetTxNumber: - The application shall use this parameter to decied on which physical channels the communiaction will be manipulated. - Access t
- `ApplDescCheckSessionTransition` (L1874, 정의): Function name:ApplDescCheckSessionTransition Description:Check if the given session transition is allowed or your ECU is currently not able to perform it. Returns:  nothing Parameter(s): - newState: - Contains the new state in which the state group will be set. - Access type: read - formerState: - Contains the current state of the state group. - Access type: read Particularitie
- `ApplDescSecurityAccessOnAttCntrChanged` (L1902, 정의): Function name:ApplDescSecurityAccessOnAttCntrChanged Description: Once the invalid key attempt counter has been changed, this event is triggered. Returns:  Random value Parameter(s): - iContext:(not available in single context systems) - Use this call-back handle for all API which need it. - Access type: read - chgEv: specifies the reason for hte counter change - You can use th
- `current` (L1939, 정의): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `SecM_ComputeKey` (L1960, 정의): Name         :  SecM_ComputeKey Called by    :  CheckKey Preconditions:  None Parameters   :  None Description  :
- `SecM_Serializer` (L1976, 정의): Name         :  SecM_Serializer Called by    :  CheckKey Preconditions:  None Parameters   :  None Description  :
- `ApplDescSecurityAccessGetSeed` (L2009, 정의): Function name:ApplDescSecurityAccessGetSeed Description: Each time called must generate a different value (e.g. current free running timer value). Returns:  Random value Parameter(s): - iContext:(not available in single context systems) - Use this call-back handle for all API which need it. - Access type: read - securityLevel: - The current security level represented by the gen
- `DescMake32Bit` (L2016, 선언): Generate each time a random 64Bit value !!! Example:
- `DescMake32Bit` (L2017, 선언): Generate each time a random 64Bit value !!! Example:
- `ApplDescSecurityAccessCheckKey` (L2053, 정의): Function name:ApplDescSecurityAccessCheckKey Description: The application must validate the received security key. Returns:  Random value Parameter(s): - iContext:(not available in single context systems) - Use this call-back handle for all API which need it. - Access type: read - securityLevel: - The current security level represented by the generated contsants (e.g kDescState
- `SecM_ComputeKey` (L2062, 선언): UDS SecurityAccess의 seed/key 생성, 검증, 시도 횟수와 지연 타이머를 처리한다.
- `ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Power_Seat_Switch` (L2110, 정의): Function name:ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Power_Seat_Switch (Service request header:$22 $A1 $1 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Acce
- `ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Power_Seat_Motor_Output` (L2155, 정의): Function name:ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Power_Seat_Motor_Output (Service request header:$22 $A1 $2 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. 
- `ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Power_Seat_Position` (L2200, 정의): Function name:ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Power_Seat_Position (Service request header:$22 $A1 $3 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Ac
- `ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Power_Seat_Fail_Detection` (L2245, 정의): Function name:ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Power_Seat_Fail_Detection (Service request header:$22 $A1 $4 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data
- `ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Heater_RR_NTC_1EA` (L2290, 정의): Function name:ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Heater_RR_NTC_1EA (Service request header:$22 $D2 $5 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Acce
- `ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Vent_RR_Blower_2EA` (L2335, 정의): Function name:ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Vent_RR_Blower_2EA (Service request header:$22 $E2 $6 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Acc
- `ApplDescReadDidVehicleManufacturerSparePartNumberDataIdentifier` (L2380, 정의): Function name:ApplDescReadDidVehicleManufacturerSparePartNumberDataIdentifier (Service request header:$22 $F1 $87 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/
- `ApplDescReadDidECUManufacturingDateDataIdentifier` (L2425, 정의): Function name:ApplDescReadDidECUManufacturingDateDataIdentifier (Service request header:$22 $F1 $8B ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/write - pMsgCo
- `ApplDescReadDidECUSerialNumberDataIdentifier` (L2470, 정의): Function name:ApplDescReadDidECUSerialNumberDataIdentifier (Service request header:$22 $F1 $8C ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/write - pMsgContext
- `ApplDescReadDidVehicleManufacturerECUHardwareNumberDataIdentifier` (L2515, 정의): Function name:ApplDescReadDidVehicleManufacturerECUHardwareNumberDataIdentifier (Service request header:$22 $F1 $91 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: rea
- `ApplDescReadDidSystemSupplierECUHardwareVersionNumberDataIdentifier` (L2560, 정의): Function name:ApplDescReadDidSystemSupplierECUHardwareVersionNumberDataIdentifier (Service request header:$22 $F1 $93 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: r
- `ApplDescReadDidECUSupplierCodeDataIdentifier` (L2605, 정의): Function name:ApplDescReadDidECUSupplierCodeDataIdentifier (Service request header:$22 $F1 $A1 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/write - pMsgContext
- `ApplDescReadDidECUSoftwareUNITnumberDataIdentifier` (L2650, 정의): Function name:ApplDescReadDidECUSoftwareUNITnumberDataIdentifier (Service request header:$22 $F1 $B0 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/write - pMsgC
- `ApplDescReadDidECUSoftwareUNIT1VersionDataIdentifier` (L2695, 정의): Function name:ApplDescReadDidECUSoftwareUNIT1VersionDataIdentifier (Service request header:$22 $F1 $B1 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/write - pMs
- `ApplDescReadDidECUSoftwareUnit1IVDDataIdentifier` (L2740, 정의): Function name:ApplDescReadDidECUSoftwareUnit1IVDDataIdentifier (Service request header:$22 $F1 $C1 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/write - pMsgCon
- `ApplDescReadDidRXSWINDataidentifier` (L2785, 정의): Function name:ApplDescReadDidRXSWINDataidentifier (Service request header:$22 $F1 $EF ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/write - pMsgContext->reqData
- `ApplDescIsDataIdSupported` (L2808, 정의): Function name:ApplDescIsDataIdSupported Description: Additionaly reject a supported PID (multi ECU configuration) Returns:  kDescTrue - if still supported, kDescFalse - if not supported Parameter(s):The PID number Particularitie(s) and limitation(s): - The function "DescProcessingDone" may not be called. - The function "DescSetNegResponse" may not be called. *******************
- `ApplDescRtnCtrlStartOtaReady` (L2846, 정의): Function name:ApplDescRtnCtrlStartOtaReady (Service request header:$31 $1 $3 $0 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/write - pMsgContext->reqDataLen: -
- `ApplDescRtnCtrlStartRoutine_Identifier_Limit_Setting` (L2891, 정의): Function name:ApplDescRtnCtrlStartRoutine_Identifier_Limit_Setting (Service request header:$31 $1 $12 $A7 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/write - 
- `ApplDescRtnCtrlStopRoutine_Identifier_Limit_Setting` (L2946, 정의): Function name:ApplDescRtnCtrlStopRoutine_Identifier_Limit_Setting (Service request header:$31 $2 $12 $A7 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/write - p
- `ApplDescRtnCtrlReqResRoutine_Identifier_Limit_Setting` (L3001, 정의): Function name:ApplDescRtnCtrlReqResRoutine_Identifier_Limit_Setting (Service request header:$31 $3 $12 $A7 ) Description: not available Returns:  nothing Parameter(s): - pMsgContext->reqData: - Points to the first service request data byte. - Access type: read - pMsgContext->resData: - Points to the first writeable byte for the service response data. - Access type: read/write -

### 5. 필요시 코드 부연 설명

대표 함수 원형 수준의 코드 맥락이다. 전체 구현은 원본 파일이 크므로 함수명/역할 중심으로 분석했다.

```c
// src/CBD/Gen/appdesc.c:153
&&&~ Preprocessor constants(defines) ----------------------------------------------------------------------------- */ #define kDescEcuCryptoKey 0x12345678 /* ----------------------------------------------------------------------------- &&&~ Function prototypes ----------------------------------------------------------------------------- */ static vuint32 SecM_ComputeKey(void)
```
```c
// src/CBD/Gen/appdesc.c:165
static void SecM_Serializer(DescMsg tgtPtr, vuint32 data)
```
```c
// src/CBD/Gen/appdesc.c:203
void DESC_API_CALLBACK_TYPE ApplDescFatalError(vuint8 errorCode, vuint16 lineNumber)
```
```c
// src/CBD/Gen/appdesc.c:218
void DESC_API_CALLBACK_TYPE ApplDescSpontaneousResponseConfirmation(DESC_CONTEXT_FORMAL_PARAM_DEF_FIRST vuint8 status)
```
```c
// src/CBD/Gen/appdesc.c:250
void DESC_API_CALLBACK_TYPE ApplDescRcrRpConfirmation(vuint8 status)
```
```c
// src/CBD/Gen/appdesc.c:290
void DESC_API_CALLBACK_TYPE ApplDescOnTransitionSession(DescStateGroup newState, DescStateGroup formerState)
```
```c
// src/CBD/Gen/appdesc.c:319
void DESC_API_CALLBACK_TYPE ApplDescOnTransitionSecurityAccess(DescStateGroup newState, DescStateGroup formerState)
```
```c
// src/CBD/Gen/appdesc.c:364
void DESC_API_CALLBACK_TYPE ApplDescEcuResetHard(DescMsgContext* pMsgContext)
```

## 파일이름 : appdesc

- 경로: `src/CBD/Gen/appdesc.h`
- 파일 종류: `.h`
- 파일 크기: 10,367 bytes
- 파일 내용: appdesc.c의 UDS 서비스 콜백 프로토타입을 선언한다.

### 1. .c/.h 파일 이름

- `appdesc.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 2
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 0

#### 주요 매크로
- L28 `__APPDESC_H__`
- L45 `DESC_APPLICATION_INTERFACE_MAGIC_NUMBER` = `7790`

#### 주요 typedef/구조체
- 없음

#### 주요 전역 변수/테이블 선언
- 없음

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 65

- L54 `ApplDescFatalError(vuint8 errorCode, vuint16 lineNumber)` [선언]
- L58 `ApplDescSpontaneousResponseConfirmation(DESC_CONTEXT_FORMAL_PARAM_DEF_FIRST vuint8 status)` [선언]
- L62 `ApplDescRcrRpConfirmation(vuint8 status)` [선언]
- L63 `ApplDescOnTransitionSession(DescStateGroup newState, DescStateGroup formerState)` [선언]
- L64 `ApplDescOnTransitionSecurityAccess(DescStateGroup newState, DescStateGroup formerState)` [선언]
- L65 `ApplDescEcuResetHard(DescMsgContext* pMsgContext)` [선언]
- L66 `ApplDescClearDiagInfo(DescMsgContext* pMsgContext)` [선언]
- L67 `ApplDescReadDtcRNODTCBSM(DescMsgContext* pMsgContext)` [선언]
- L68 `ApplDescReadDtcRDTCBSM(DescMsgContext* pMsgContext)` [선언]
- L69 `ApplDescReadDtcRDTCEDRBDN(DescMsgContext* pMsgContext)` [선언]
- L70 `ApplDescReadDtcRSUPDTC(DescMsgContext* pMsgContext)` [선언]
- L71 `ApplDescIoCtrlFrzCurrStateSupported_check_DID_F010(DescMsgContext* pMsgContext)` [선언]
- L72 `ApplDescIoCtrlRetCtrlToEcuHeat_Step_3_Operation(DescMsgContext* pMsgContext)` [선언]
- L73 `ApplDescIoCtrlShortTermAdjHeat_Step_3_Operation(DescMsgContext* pMsgContext)` [선언]
- L74 `ApplDescIoCtrlRetCtrlToEcuVent_Step_3_Operation(DescMsgContext* pMsgContext)` [선언]
- L75 `ApplDescIoCtrlShortTermAdjVent_Step_3_Operation(DescMsgContext* pMsgContext)` [선언]
- L76 `ApplDescIoCtrlFrzCurrStateSupported_check_DID_F040(DescMsgContext* pMsgContext)` [선언]
- L77 `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Foremost_Position_Recline(DescMsgContext* pMsgContext)` [선언]
- L78 `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Foremost_Position_Recline(DescMsgContext* pMsgContext)` [선언]
- L79 `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Last_Position_Recline(DescMsgContext* pMsgContext)` [선언]
- L80 `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Last_Position_Recline(DescMsgContext* pMsgContext)` [선언]
- L81 `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Foremost_Position_Relax(DescMsgContext* pMsgContext)` [선언]
- L82 `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Foremost_Position_Relax(DescMsgContext* pMsgContext)` [선언]
- L83 `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Last_Position_Relax(DescMsgContext* pMsgContext)` [선언]
- L84 `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Last_Position_Relax(DescMsgContext* pMsgContext)` [선언]
- L85 `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Foremost_Position_Legrest(DescMsgContext* pMsgContext)` [선언]
- L86 `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Foremost_Position_Legrest(DescMsgContext* pMsgContext)` [선언]
- L87 `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Last_Position_Legrest(DescMsgContext* pMsgContext)` [선언]
- L88 `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Last_Position_Legrest(DescMsgContext* pMsgContext)` [선언]
- L89 `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Foremost_Position_LegExt(DescMsgContext* pMsgContext)` [선언]
- L90 `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Foremost_Position_LegExt(DescMsgContext* pMsgContext)` [선언]
- L91 `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Last_Position_LegExt(DescMsgContext* pMsgContext)` [선언]
- L92 `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Last_Position_LegExt(DescMsgContext* pMsgContext)` [선언]
- L93 `ApplDescControlDtcSettingEnable(DescMsgContext* pMsgContext)` [선언]
- L94 `ApplDescControlDtcSettingDisable(DescMsgContext* pMsgContext)` [선언]
- L96 `ApplDescCheckCommCtrl(DESC_CONTEXT_FORMAL_PARAM_DEF_FIRST DescOemCommControlInfo *commControlInfo)` [선언]
- L99 `ApplDescSetCommMode(DescOemCommControlInfo *commControlInfo)` [선언]
- L103 `ApplDescSetCommModeOnRxPath(DescOemCommControlInfo *commControlInfo)` [선언]
- L107 `ApplDescOnCommunicationDisable(void)` [선언]
- L108 `ApplDescOnCommunicationEnable(void)` [선언]
- L110 `ApplDescCheckSessionTransition(DescStateGroup newState, DescStateGroup formerState)` [선언]
- L114 `ApplDescSecurityAccessGetSeed(DescSecurityAccessContext* pDescSecurityAccessContext)` [선언]
- L117 `ApplDescSecurityAccessCheckKey(DescSecurityAccessContext* pDescSecurityAccessContext)` [선언]
- L121 `ApplDescSecurityAccessOnAttCntrChanged(DESC_CONTEXT_FORMAL_PARAM_DEF_FIRST DescSecAccAttCtrChgEvType chgEv, vuint8 newAttCounter)` [선언]
- L124 `ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Power_Seat_Switch(DescMsgContext* pMsgContext)` [선언]
- L125 `ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Power_Seat_Motor_Output(DescMsgContext* pMsgContext)` [선언]
- L126 `ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Power_Seat_Position(DescMsgContext* pMsgContext)` [선언]
- L127 `ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Power_Seat_Fail_Detection(DescMsgContext* pMsgContext)` [선언]
- L128 `ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Heater_RR_NTC_1EA(DescMsgContext* pMsgContext)` [선언]
- L129 `ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Vent_RR_Blower_2EA(DescMsgContext* pMsgContext)` [선언]
- L130 `ApplDescReadDidVehicleManufacturerSparePartNumberDataIdentifier(DescMsgContext* pMsgContext)` [선언]
- L131 `ApplDescReadDidECUManufacturingDateDataIdentifier(DescMsgContext* pMsgContext)` [선언]
- L132 `ApplDescReadDidECUSerialNumberDataIdentifier(DescMsgContext* pMsgContext)` [선언]
- L133 `ApplDescReadDidVehicleManufacturerECUHardwareNumberDataIdentifier(DescMsgContext* pMsgContext)` [선언]
- L134 `ApplDescReadDidSystemSupplierECUHardwareVersionNumberDataIdentifier(DescMsgContext* pMsgContext)` [선언]
- L135 `ApplDescReadDidECUSupplierCodeDataIdentifier(DescMsgContext* pMsgContext)` [선언]
- L136 `ApplDescReadDidECUSoftwareUNITnumberDataIdentifier(DescMsgContext* pMsgContext)` [선언]
- L137 `ApplDescReadDidECUSoftwareUNIT1VersionDataIdentifier(DescMsgContext* pMsgContext)` [선언]
- L138 `ApplDescReadDidECUSoftwareUnit1IVDDataIdentifier(DescMsgContext* pMsgContext)` [선언]
- L139 `ApplDescReadDidRXSWINDataidentifier(DescMsgContext* pMsgContext)` [선언]
- L142 `ApplDescIsDataIdSupported(vuint16 pid)` [선언]
- L145 `ApplDescRtnCtrlStartOtaReady(DescMsgContext* pMsgContext)` [선언]
- L146 `ApplDescRtnCtrlStartRoutine_Identifier_Limit_Setting(DescMsgContext* pMsgContext)` [선언]
- L147 `ApplDescRtnCtrlStopRoutine_Identifier_Limit_Setting(DescMsgContext* pMsgContext)` [선언]
- L148 `ApplDescRtnCtrlReqResRoutine_Identifier_Limit_Setting(DescMsgContext* pMsgContext)` [선언]

### 4. 각 함수별 역할 설명

- `ApplDescFatalError` (L54, 선언): Assertion function for better integration support.
- `ApplDescSpontaneousResponseConfirmation` (L58, 선언): Assertion function for better integration support.
- `ApplDescRcrRpConfirmation` (L62, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescOnTransitionSession` (L63, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescOnTransitionSecurityAccess` (L64, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescEcuResetHard` (L65, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescClearDiagInfo` (L66, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescReadDtcRNODTCBSM` (L67, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescReadDtcRDTCBSM` (L68, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescReadDtcRDTCEDRBDN` (L69, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescReadDtcRSUPDTC` (L70, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescIoCtrlFrzCurrStateSupported_check_DID_F010` (L71, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescIoCtrlRetCtrlToEcuHeat_Step_3_Operation` (L72, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescIoCtrlShortTermAdjHeat_Step_3_Operation` (L73, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescIoCtrlRetCtrlToEcuVent_Step_3_Operation` (L74, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescIoCtrlShortTermAdjVent_Step_3_Operation` (L75, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescIoCtrlFrzCurrStateSupported_check_DID_F040` (L76, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Foremost_Position_Recline` (L77, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Foremost_Position_Recline` (L78, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Last_Position_Recline` (L79, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Last_Position_Recline` (L80, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Foremost_Position_Relax` (L81, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Foremost_Position_Relax` (L82, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Last_Position_Relax` (L83, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Last_Position_Relax` (L84, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Foremost_Position_Legrest` (L85, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Foremost_Position_Legrest` (L86, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Last_Position_Legrest` (L87, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Last_Position_Legrest` (L88, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Foremost_Position_LegExt` (L89, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Foremost_Position_LegExt` (L90, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescIoCtrlRetCtrlToEcuMove_P_Seat_to_the_Last_Position_LegExt` (L91, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescIoCtrlShortTermAdjMove_P_Seat_to_the_Last_Position_LegExt` (L92, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescControlDtcSettingEnable` (L93, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescControlDtcSettingDisable` (L94, 선언): If RCR-RP has been forced this confirmation function will notify the application about the transmission result.
- `ApplDescCheckCommCtrl` (L96, 선언): 통신 허용/차단, 네트워크 요청/해제 또는 통신 모드 전환을 처리한다.
- `ApplDescSetCommMode` (L99, 선언): 통신 허용/차단, 네트워크 요청/해제 또는 통신 모드 전환을 처리한다.
- `ApplDescSetCommModeOnRxPath` (L103, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `ApplDescOnCommunicationDisable` (L107, 선언): 통신 허용/차단, 네트워크 요청/해제 또는 통신 모드 전환을 처리한다.
- `ApplDescOnCommunicationEnable` (L108, 선언): 통신 허용/차단, 네트워크 요청/해제 또는 통신 모드 전환을 처리한다.
- `ApplDescCheckSessionTransition` (L110, 선언): UDS 진단 세션 상태 조회, 전환 또는 세션별 타이밍 처리를 담당한다.
- `ApplDescSecurityAccessGetSeed` (L114, 선언): application function which returns a randomly generated value each time it is called (e.g. current free running timer value)
- `ApplDescSecurityAccessCheckKey` (L117, 선언): application function which must evaluate the received key and confirm its validity.
- `ApplDescSecurityAccessOnAttCntrChanged` (L121, 선언): application notification on time expiration
- `ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Power_Seat_Switch` (L124, 선언): application notification on time expiration
- `ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Power_Seat_Motor_Output` (L125, 선언): application notification on time expiration
- `ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Power_Seat_Position` (L126, 선언): application notification on time expiration
- `ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Power_Seat_Fail_Detection` (L127, 선언): application notification on time expiration
- `ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Heater_RR_NTC_1EA` (L128, 선언): application notification on time expiration
- `ApplDescReadDidRDBI_Input_Output_Record_Local_Identifier_for_Vent_RR_Blower_2EA` (L129, 선언): application notification on time expiration
- `ApplDescReadDidVehicleManufacturerSparePartNumberDataIdentifier` (L130, 선언): application notification on time expiration
- `ApplDescReadDidECUManufacturingDateDataIdentifier` (L131, 선언): application notification on time expiration
- `ApplDescReadDidECUSerialNumberDataIdentifier` (L132, 선언): application notification on time expiration
- `ApplDescReadDidVehicleManufacturerECUHardwareNumberDataIdentifier` (L133, 선언): application notification on time expiration
- `ApplDescReadDidSystemSupplierECUHardwareVersionNumberDataIdentifier` (L134, 선언): application notification on time expiration
- `ApplDescReadDidECUSupplierCodeDataIdentifier` (L135, 선언): application notification on time expiration
- `ApplDescReadDidECUSoftwareUNITnumberDataIdentifier` (L136, 선언): application notification on time expiration
- `ApplDescReadDidECUSoftwareUNIT1VersionDataIdentifier` (L137, 선언): application notification on time expiration
- `ApplDescReadDidECUSoftwareUnit1IVDDataIdentifier` (L138, 선언): application notification on time expiration
- `ApplDescReadDidRXSWINDataidentifier` (L139, 선언): application notification on time expiration
- `ApplDescIsDataIdSupported` (L142, 선언): Additionaly reject a supported PID (multi ECU configuration)
- `ApplDescRtnCtrlStartOtaReady` (L145, 선언): Additionaly reject a supported PID (multi ECU configuration)
- `ApplDescRtnCtrlStartRoutine_Identifier_Limit_Setting` (L146, 선언): Additionaly reject a supported PID (multi ECU configuration)
- `ApplDescRtnCtrlStopRoutine_Identifier_Limit_Setting` (L147, 선언): Additionaly reject a supported PID (multi ECU configuration)
- `ApplDescRtnCtrlReqResRoutine_Identifier_Limit_Setting` (L148, 선언): Additionaly reject a supported PID (multi ECU configuration)

### 5. 필요시 코드 부연 설명

이 파일은 생성 설정/선언 성격이 강해 별도 코드 전문 인용 없이 심볼 표로 역할을 설명했다.

## CAN-FD 마이그레이션 관점

Gen 계층은 프로젝트별 설정과 생성 테이블을 담고 있으므로 CAN-FD 전환 시 가장 먼저 재설계해야 하는 데이터 원천이다.

- `can_cfg.h`: CAN-FD enable, BRS enable, FD payload size, FD-capable controller 옵션을 추가한다.
- `can_par.*`: 메시지별 Classical/FD 여부, 실제 payload length, FD DLC, data pointer 크기를 추가한다.
- `drv_par.*`: CAN-FD 메시지가 8바이트를 넘는 경우 구조체/union buffer 크기를 확장한다.
- `tp_cfg.h`: ISO-TP over CAN-FD 사용 여부와 frame payload 크기 설정을 추가한다.
- `desc.*`: UDS response buffer 크기와 responseTooLong 기준을 FD payload에 맞춰 재검토한다.

## 파일이름 : appdescdev

- 경로: `src/CBD/Gen/appdescdev.c`
- 파일 종류: `.c`
- 파일 크기: 1,879 bytes
- 파일 내용: 진단 개발/디버그용 보조 콜백 또는 장치별 확장 스텁이다.

### 1. .c/.h 파일 이름

- `appdescdev.c`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 0
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 0

#### 주요 매크로
- 없음

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

## 파일이름 : can_cfg

- 경로: `src/CBD/Gen/can_cfg.h`
- 파일 종류: `.h`
- 파일 크기: 9,491 bytes
- 파일 내용: CAN 드라이버 생성 설정 매크로와 오브젝트 수, 기능 스위치를 정의한다.

### 1. .c/.h 파일 이름

- `can_cfg.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 154
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 0

#### 주요 매크로
- L46 `__CAN_CFG_H__`
- L49 `HW_MPC55XX_FLEXCAN2CPUCANDLL_VERSION` = `0x0118`
- L50 `HW_MPC55XX_FLEXCAN2CPUCANDLL_RELEASE_VERSION` = `0x04`
- L52 `HW__BASECPUCANDLL_VERSION` = `0x0224`
- L53 `HW__BASECPUCANDLL_RELEASE_VERSION` = `0x00`
- L55 `DRVCAN__BASEDLL_VERSION` = `0x0321`
- L56 `DRVCAN__BASEDLL_RELEASE_VERSION` = `0x04`
- L58 `DRVCAN__BASERI14DLL_VERSION` = `0x0209`
- L59 `DRVCAN__BASERI14DLL_RELEASE_VERSION` = `0x01`
- L61 `DRVCAN__BASERI15DLL_VERSION` = `0x0105`
- L62 `DRVCAN__BASERI15DLL_RELEASE_VERSION` = `0x00`
- L64 `DRVCAN__BASEHLLDLL_VERSION` = `0x0305`
- L65 `DRVCAN__BASEHLLDLL_RELEASE_VERSION` = `0x03`
- L67 `DRVCAN__BASERI14HLLDLL_VERSION` = `0x0205`
- L68 `DRVCAN__BASERI14HLLDLL_RELEASE_VERSION` = `0x02`
- L70 `DRVCAN__BASERI15HLLDLL_VERSION` = `0x0101`
- L71 `DRVCAN__BASERI15HLLDLL_RELEASE_VERSION` = `0x03`
- L73 `CAN_DRV_MAC7100DLL_VERSION` = `0x0113`
- L74 `CAN_DRV_MAC7100DLL_RELEASE_VERSION` = `0x03`
- L77 `kCanNumberOfChannels` = `1`
- L78 `kCanNumberOfHwChannels` = `1`
- L79 `kCanNumberOfPhysChannels` = `3`
- L80 `C_DISABLE_MEMCOPY_SUPPORT`
- L81 `C_DISABLE_OSEK_OS`
- L82 `C_DISABLE_VARIABLE_DLC`
- L83 `C_DISABLE_DLC_FAILED_FCT`
- L84 `C_DISABLE_VARIABLE_RX_DATALEN`
- L85 `C_DISABLE_MULTI_ECU_CONFIG`
- L86 `C_DISABLE_MULTI_ECU_PHYS`
- L87 `C_DISABLE_EXTENDED_ID`
- L88 `C_DISABLE_MIXED_ID`
- L89 `C_DISABLE_RECEIVE_FCT`
- L91 `C_DISABLE_ECU_SWITCH_PASS`
- L92 `C_ENABLE_TRANSMIT_QUEUE`
- L93 `C_DISABLE_OVERRUN`
- L94 `C_DISABLE_INTCTRL_BY_APPL`
- L95 `C_DISABLE_COMMON_CAN`
- L96 `C_ENABLE_USER_CHECK`
- L97 `C_ENABLE_HARDWARE_CHECK`
- L98 `C_ENABLE_GEN_CHECK`
- L99 `C_ENABLE_INTERNAL_CHECK`
- L100 `C_DISABLE_DYN_RX_OBJECTS`
- L101 `C_DISABLE_DYN_TX_OBJECTS`
- L102 `C_DISABLE_DYN_TX_ID`
- L103 `C_DISABLE_DYN_TX_DLC`
- L104 `C_DISABLE_DYN_TX_DATAPTR`
- L105 `C_DISABLE_DYN_TX_PRETRANS_FCT`
- L106 `C_DISABLE_DYN_TX_CONF_FCT`
- L107 `C_DISABLE_EXTENDED_STATUS`
- L108 `C_ENABLE_TX_OBSERVE`
- L109 `C_DISABLE_HW_LOOP_TIMER`
- L110 `C_DISABLE_NOT_MATCHED_FCT`
- L111 `C_SECURITY_LEVEL` = `30`
- L113 `C_DISABLE_MULTICHANNEL_API`
- L114 `C_ENABLE_PART_OFFLINE`
- L115 `C_DISABLE_RANGE_0`
- L116 `C_DISABLE_RANGE_1`
- L117 `C_DISABLE_RANGE_2`
- L118 `C_DISABLE_RANGE_3`
- L119 `ApplCanBusOff` = `NmBasicCanBusOff`
- L121 `kCanNumberOfTxObjects` = `16`
- L122 `kCanNumberOfTxStatObjects` = `16`
- L123 `kCanNumberOfTxDynObjects` = `0`
- L124 `kCanNumberOfRxObjects` = `7`
- L125 `kCanNumberOfRxStatFullCANObjects` = `6`
- L126 `kCanNumberOfRxStatBasicCANObjects` = `1`
- L127 `kCanNumberOfRxDynFullCANObjects` = `0`
- L128 `kCanNumberOfRxDynBasicCANObjects` = `0`
- L129 `kCanNumberOfRxDynObjects` = `0`
- L130 `kCanNumberOfRxStatObjects` = `7`
- L131 `kCanNumberOfConfFlags` = `15`
- L132 `kCanNumberOfIndFlags` = `0`
- L133 `kCanNumberOfConfirmationFlags` = `2`
- L134 `kCanNumberOfIndicationFlags` = `0`
- L135 `kCanNumberOfInitObjects` = `1`
- L136 `kCanExtNumberOfInitObjects` = `0`
- L137 `kCanHwRxDynFullStartIndex` = `6`
- L138 `C_SEARCH_LINEAR`
- L140 `C_ENABLE_RX_MSG_INDIRECTION`
- L142 `C_ENABLE_CONFIRMATION_FLAG`
- L143 `C_DISABLE_INDICATION_FLAG`
- L144 `C_DISABLE_PRETRANSMIT_FCT`
- L145 `C_ENABLE_CONFIRMATION_FCT`
- L146 `C_DISABLE_INDICATION_FCT`
- L147 `C_ENABLE_PRECOPY_FCT`
- L148 `C_ENABLE_COPY_TX_DATA`
- L149 `C_ENABLE_COPY_RX_DATA`
- L150 `C_ENABLE_DLC_CHECK`
- L151 `C_DISABLE_DLC_CHECK_MIN_DATALEN`
- L153 `C_ENABLE_GENERIC_PRECOPY`
- L154 `APPL_CAN_GENERIC_PRECOPY` = `IlCanGenericPrecopy`
- L156 `C_SEND_GRP_NONE` = `0x00`
- L157 `C_SEND_GRP_ALL` = `0xFF`
- L158 `C_SEND_GRP_APPL` = `0x01`
- L159 `C_SEND_GRP_NM` = `0x02`
- L160 `C_SEND_GRP_DIAG` = `0x04`
- L161 `C_SEND_GRP_IL` = `0x08`
- L162 `C_SEND_GRP_TP` = `0x10`
- L163 `C_SEND_GRP_USER5` = `0x20`
- L164 `C_SEND_GRP_USER6` = `0x40`
- L165 `C_SEND_GRP_USER7` = `0x80`
- L166 `C_ENABLE_CAN_CANCEL_NOTIFICATION`
- L167 `APPL_CAN_CANCELNOTIFICATION` = `IlCanCancelNotification`
- L169 `kCanPhysToLogChannelIndex_2`
- L170 `C_ENABLE_RX_FULLCAN_OBJECTS`
- L171 `C_DISABLE_RX_BASICCAN_OBJECTS`
- L172 `kCanNumberOfRxFullCANObjects` = `6`
- L174 `kCanNumberOfRxBasicCANObjects` = `1`
- L175 `kCanInitObj1` = `0`
- L176 `C_DISABLE_TX_MASK_EXT_ID`
- L177 `C_DISABLE_RX_MASK_EXT_ID`
- L178 `C_MASK_EXT_ID` = `0xFFFFFFFF`
- L180 `C_ENABLE_CAN_CAN_INTERRUPT_CONTROL`
- L181 `C_DISABLE_CAN_TX_CONF_FCT`
- L183 `C_DISABLE_TX_POLLING`
- L184 `C_ENABLE_RX_BASICCAN_POLLING`
- L185 `C_DISABLE_RX_FULLCAN_POLLING`
- L186 `C_DISABLE_ERROR_POLLING`
- L187 `C_DISABLE_WAKEUP_POLLING`
- L188 `C_DISABLE_FULLCAN_OVERRUN`
- ... 생략 34개 (동일 패턴의 생성 심볼)

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

## 파일이름 : can_par

- 경로: `src/CBD/Gen/can_par.c`
- 파일 종류: `.c`
- 파일 크기: 22,483 bytes
- 파일 내용: CAN 메시지 ID, DLC, 데이터 버퍼, 콜백 테이블 등 프로젝트별 CAN 파라미터를 정의한다.

### 1. .c/.h 파일 이름

- `can_par.c`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 1
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 24

#### 주요 매크로
- L45 `C_DRV_INTERNAL`

#### 주요 typedef/구조체
- 없음

#### 주요 전역 변수/테이블 선언
- L51 `V_MEMROM0 V_MEMROM1 tCanTxId0 V_MEMROM2 CanTxId0[kCanNumberOfTxObjects] =`
- L74 `V_MEMROM0 V_MEMROM1 vuint8 V_MEMROM2 CanTxDLC[kCanNumberOfTxObjects] =`
- L98 `V_MEMROM0 V_MEMROM1 TxDataPtr V_MEMROM2 CanTxDataPtr[kCanNumberOfTxObjects] =`
- L124 `V_MEMROM0 V_MEMROM1 ApplPreTransmitFct V_MEMROM2 CanTxApplPreTransmitPtr[kCanNumberOfTxObjects] =`
- L150 `V_MEMROM0 V_MEMROM1 ApplConfirmationFct V_MEMROM2 CanTxApplConfirmationPtr[kCanNumberOfTxObjects] =`
- L177 `V_MEMROM0 V_MEMROM1 vuint8 V_MEMROM2 CanTxSendMask[kCanNumberOfTxObjects] =`
- L204 `V_MEMROM0 V_MEMROM1 vuint8 V_MEMROM2 CanConfirmationOffset[kCanNumberOfTxObjects] =`
- L227 `V_MEMROM0 V_MEMROM1 vuint8 V_MEMROM2 CanConfirmationMask[kCanNumberOfTxObjects] =`
- L257 `V_MEMROM0 V_MEMROM1 tCanRxId0 V_MEMROM2 CanRxId0[kCanNumberOfRxObjects] =`
- L271 `V_MEMROM0 V_MEMROM1 CanReceiveHandle V_MEMROM2 CanRxMsgIndirection[kCanNumberOfRxObjects] =`
- L285 `V_MEMROM0 V_MEMROM1 vuint8 V_MEMROM2 CanRxDataLen[kCanNumberOfRxObjects] =`
- L300 `V_MEMROM0 V_MEMROM1 RxDataPtr V_MEMROM2 CanRxDataPtr[kCanNumberOfRxObjects] =`
- L320 `V_MEMROM0 V_MEMROM1 ApplPrecopyFct V_MEMROM2 CanRxApplPrecopyPtr[kCanNumberOfRxObjects] =`
- L337 `V_MEMROM0 V_MEMROM1 ApplIndicationFct V_MEMROM2 CanRxApplIndicationPtr[kCanNumberOfRxObjects] =`
- L354 `V_MEMROM0 V_MEMROM1 vuint8 V_MEMROM2 CanIndicationOffset[kCanNumberOfRxObjects] =`
- L368 `V_MEMROM0 V_MEMROM1 vuint8 V_MEMROM2 CanIndicationMask[kCanNumberOfRxObjects] =`
- L391 `V_MEMROM0 V_MEMROM1 vuint8 V_MEMROM2 CanInitObjectStartIndex[2] =`
- L400 `V_MEMROM0 V_MEMROM1 tCanInitObject V_MEMROM2 CanInitObject[1] =`
- L411 `V_MEMROM0 V_MEMROM1 tCanInitBasicCan V_MEMROM2 CanInitBasicCan[1][1] =`
- L431 `V_MEMROM0 V_MEMROM1 vuint8 V_MEMROM2 CanPhysToLogChannel[kVNumberOfIdentities][kCanNumberOfPhysChannels] =`
- L447 `V_MEMROM0 V_MEMROM1 tVIdentityMsk V_MEMROM2 CanChannelIdentityAssignment[kCanNumberOfChannels] =`
- L458 `V_MEMROM0 V_MEMROM1 tVIdentityMsk V_MEMROM2 CanRxIdentityAssignment[kCanNumberOfRxObjects] =`
- L471 `V_MEMROM0 V_MEMROM1 tVIdentityMsk V_MEMROM2 CanTxIdentityAssignment[kCanNumberOfTxObjects] =`
- L498 `V_MEMROM0 V_MEMROM1 vuint8 V_MEMROM2 CanTxHwObj[kCanNumberOfTxObjects] =`

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 0

- 없음

### 4. 각 함수별 역할 설명

- 함수 없음: 설정/타입/매크로 전용 파일이다.

### 5. 필요시 코드 부연 설명

이 파일은 생성 설정/선언 성격이 강해 별도 코드 전문 인용 없이 심볼 표로 역할을 설명했다.

## 파일이름 : can_par

- 경로: `src/CBD/Gen/can_par.h`
- 파일 종류: `.h`
- 파일 크기: 34,428 bytes
- 파일 내용: CAN 메시지 ID, DLC, 데이터 버퍼, 콜백 테이블 등 프로젝트별 CAN 파라미터를 정의한다.

### 1. .c/.h 파일 이름

- `can_par.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 244
- typedef/struct/enum 후보 수: 31
- 전역 변수/테이블 선언 후보 수: 62

#### 주요 매크로
- L46 `__CAN_PAR_H__`
- L84 `CanTxIPSU_GST` = `0`
- L85 `CanTxDPSS_02_200ms` = `1`
- L86 `CanTxIPSU_14_200ms` = `2`
- L87 `CanTxIPSU_13_200ms` = `3`
- L88 `CanTxIPSU_12_200ms` = `4`
- L89 `CanTxIPSU_11_200ms` = `5`
- L90 `CanTxIPSU_10_200ms` = `6`
- L91 `CanTxIPSU_09_200ms` = `7`
- L92 `CanTxIPSU_08_200ms` = `8`
- L93 `CanTxIPSU_07_200ms` = `9`
- L94 `CanTxIPSU_06_200ms` = `10`
- L95 `CanTxIPSU_05_200ms` = `11`
- L96 `CanTxIPSU_04_200ms` = `12`
- L97 `CanTxIPSU_03_200ms` = `13`
- L98 `CanTxIPSU_02_200ms` = `14`
- L99 `CanTxIPSU_01_200ms` = `15`
- L106 `DPSS_02_200ms_conf_b` = `(CanConfirmationFlags.w[0].b0)`
- L107 `CanWriteSyncDPSS_02_200ms_conf_b(x)` = `\`
- L113 `IPSU_14_200ms_conf_b` = `(CanConfirmationFlags.w[0].b1)`
- L114 `CanWriteSyncIPSU_14_200ms_conf_b(x)` = `\`
- L120 `IPSU_13_200ms_conf_b` = `(CanConfirmationFlags.w[0].b2)`
- L121 `CanWriteSyncIPSU_13_200ms_conf_b(x)` = `\`
- L127 `IPSU_12_200ms_conf_b` = `(CanConfirmationFlags.w[0].b3)`
- L128 `CanWriteSyncIPSU_12_200ms_conf_b(x)` = `\`
- L134 `IPSU_11_200ms_conf_b` = `(CanConfirmationFlags.w[0].b4)`
- L135 `CanWriteSyncIPSU_11_200ms_conf_b(x)` = `\`
- L141 `IPSU_10_200ms_conf_b` = `(CanConfirmationFlags.w[0].b5)`
- L142 `CanWriteSyncIPSU_10_200ms_conf_b(x)` = `\`
- L148 `IPSU_09_200ms_conf_b` = `(CanConfirmationFlags.w[0].b6)`
- L149 `CanWriteSyncIPSU_09_200ms_conf_b(x)` = `\`
- L155 `IPSU_08_200ms_conf_b` = `(CanConfirmationFlags.w[0].b7)`
- L156 `CanWriteSyncIPSU_08_200ms_conf_b(x)` = `\`
- L162 `IPSU_07_200ms_conf_b` = `(CanConfirmationFlags.w[0].b10)`
- L163 `CanWriteSyncIPSU_07_200ms_conf_b(x)` = `\`
- L169 `IPSU_06_200ms_conf_b` = `(CanConfirmationFlags.w[0].b11)`
- L170 `CanWriteSyncIPSU_06_200ms_conf_b(x)` = `\`
- L176 `IPSU_05_200ms_conf_b` = `(CanConfirmationFlags.w[0].b12)`
- L177 `CanWriteSyncIPSU_05_200ms_conf_b(x)` = `\`
- L183 `IPSU_04_200ms_conf_b` = `(CanConfirmationFlags.w[0].b13)`
- L184 `CanWriteSyncIPSU_04_200ms_conf_b(x)` = `\`
- L190 `IPSU_03_200ms_conf_b` = `(CanConfirmationFlags.w[0].b14)`
- L191 `CanWriteSyncIPSU_03_200ms_conf_b(x)` = `\`
- L197 `IPSU_02_200ms_conf_b` = `(CanConfirmationFlags.w[0].b15)`
- L198 `CanWriteSyncIPSU_02_200ms_conf_b(x)` = `\`
- L204 `IPSU_01_200ms_conf_b` = `(CanConfirmationFlags.w[0].b16)`
- L205 `CanWriteSyncIPSU_01_200ms_conf_b(x)` = `\`
- L217 `CanRxCGW_05_200ms` = `0`
- L218 `CanRxCGW_04_200ms` = `1`
- L219 `CanRxCGW_03_200ms` = `2`
- L220 `CanRxCGW_02_200ms` = `3`
- L221 `CanRxCGW_01_200ms` = `4`
- L222 `CanRxGST_ALL` = `5`
- L223 `CanRxGST_IPSU` = `6`
- L231 `c1_CGW_05_200ms_c` = `(CGW_05_200ms._c[0])`
- L232 `c2_CGW_05_200ms_c` = `(CGW_05_200ms._c[1])`
- L233 `c3_CGW_05_200ms_c` = `(CGW_05_200ms._c[2])`
- L234 `c4_CGW_05_200ms_c` = `(CGW_05_200ms._c[3])`
- L235 `c5_CGW_05_200ms_c` = `(CGW_05_200ms._c[4])`
- L236 `c6_CGW_05_200ms_c` = `(CGW_05_200ms._c[5])`
- L237 `c7_CGW_05_200ms_c` = `(CGW_05_200ms._c[6])`
- L238 `c8_CGW_05_200ms_c` = `(CGW_05_200ms._c[7])`
- L241 `c1_CGW_04_200ms_c` = `(CGW_04_200ms._c[0])`
- L242 `c2_CGW_04_200ms_c` = `(CGW_04_200ms._c[1])`
- L243 `c3_CGW_04_200ms_c` = `(CGW_04_200ms._c[2])`
- L244 `c4_CGW_04_200ms_c` = `(CGW_04_200ms._c[3])`
- L245 `c5_CGW_04_200ms_c` = `(CGW_04_200ms._c[4])`
- L246 `c6_CGW_04_200ms_c` = `(CGW_04_200ms._c[5])`
- L247 `c7_CGW_04_200ms_c` = `(CGW_04_200ms._c[6])`
- L248 `c8_CGW_04_200ms_c` = `(CGW_04_200ms._c[7])`
- L251 `c1_CGW_03_200ms_c` = `(CGW_03_200ms._c[0])`
- L252 `c2_CGW_03_200ms_c` = `(CGW_03_200ms._c[1])`
- L253 `c3_CGW_03_200ms_c` = `(CGW_03_200ms._c[2])`
- L254 `c4_CGW_03_200ms_c` = `(CGW_03_200ms._c[3])`
- L255 `c5_CGW_03_200ms_c` = `(CGW_03_200ms._c[4])`
- L256 `c6_CGW_03_200ms_c` = `(CGW_03_200ms._c[5])`
- L257 `c7_CGW_03_200ms_c` = `(CGW_03_200ms._c[6])`
- L258 `c8_CGW_03_200ms_c` = `(CGW_03_200ms._c[7])`
- L261 `c1_CGW_02_200ms_c` = `(CGW_02_200ms._c[0])`
- L262 `c2_CGW_02_200ms_c` = `(CGW_02_200ms._c[1])`
- L263 `c3_CGW_02_200ms_c` = `(CGW_02_200ms._c[2])`
- L264 `c4_CGW_02_200ms_c` = `(CGW_02_200ms._c[3])`
- L265 `c5_CGW_02_200ms_c` = `(CGW_02_200ms._c[4])`
- L266 `c6_CGW_02_200ms_c` = `(CGW_02_200ms._c[5])`
- L267 `c7_CGW_02_200ms_c` = `(CGW_02_200ms._c[6])`
- L268 `c8_CGW_02_200ms_c` = `(CGW_02_200ms._c[7])`
- L271 `c1_CGW_01_200ms_c` = `(CGW_01_200ms._c[0])`
- L272 `c2_CGW_01_200ms_c` = `(CGW_01_200ms._c[1])`
- L273 `c3_CGW_01_200ms_c` = `(CGW_01_200ms._c[2])`
- L282 `c1_IPSU_GST_c` = `(IPSU_GST._c[0])`
- L283 `c2_IPSU_GST_c` = `(IPSU_GST._c[1])`
- L284 `c3_IPSU_GST_c` = `(IPSU_GST._c[2])`
- L285 `c4_IPSU_GST_c` = `(IPSU_GST._c[3])`
- L286 `c5_IPSU_GST_c` = `(IPSU_GST._c[4])`
- L287 `c6_IPSU_GST_c` = `(IPSU_GST._c[5])`
- L288 `c7_IPSU_GST_c` = `(IPSU_GST._c[6])`
- L289 `c8_IPSU_GST_c` = `(IPSU_GST._c[7])`
- L292 `c1_DPSS_02_200ms_c` = `(DPSS_02_200ms._c[0])`
- L293 `c2_DPSS_02_200ms_c` = `(DPSS_02_200ms._c[1])`
- L294 `c3_DPSS_02_200ms_c` = `(DPSS_02_200ms._c[2])`
- L295 `c4_DPSS_02_200ms_c` = `(DPSS_02_200ms._c[3])`
- L296 `c5_DPSS_02_200ms_c` = `(DPSS_02_200ms._c[4])`
- L297 `c6_DPSS_02_200ms_c` = `(DPSS_02_200ms._c[5])`
- L298 `c7_DPSS_02_200ms_c` = `(DPSS_02_200ms._c[6])`
- L299 `c8_DPSS_02_200ms_c` = `(DPSS_02_200ms._c[7])`
- L302 `c1_IPSU_14_200ms_c` = `(IPSU_14_200ms._c[0])`
- L303 `c2_IPSU_14_200ms_c` = `(IPSU_14_200ms._c[1])`
- L304 `c3_IPSU_14_200ms_c` = `(IPSU_14_200ms._c[2])`
- L305 `c4_IPSU_14_200ms_c` = `(IPSU_14_200ms._c[3])`
- L306 `c5_IPSU_14_200ms_c` = `(IPSU_14_200ms._c[4])`
- L307 `c6_IPSU_14_200ms_c` = `(IPSU_14_200ms._c[5])`
- L308 `c7_IPSU_14_200ms_c` = `(IPSU_14_200ms._c[6])`
- L309 `c8_IPSU_14_200ms_c` = `(IPSU_14_200ms._c[7])`
- L312 `c1_IPSU_13_200ms_c` = `(IPSU_13_200ms._c[0])`
- L313 `c2_IPSU_13_200ms_c` = `(IPSU_13_200ms._c[1])`
- L314 `c3_IPSU_13_200ms_c` = `(IPSU_13_200ms._c[2])`
- L315 `c4_IPSU_13_200ms_c` = `(IPSU_13_200ms._c[3])`
- L316 `c5_IPSU_13_200ms_c` = `(IPSU_13_200ms._c[4])`
- L317 `c6_IPSU_13_200ms_c` = `(IPSU_13_200ms._c[5])`
- L318 `c7_IPSU_13_200ms_c` = `(IPSU_13_200ms._c[6])`
- ... 생략 124개 (동일 패턴의 생성 심볼)

#### 주요 typedef/구조체
- L447 `typedef struct _c_GST_ALL_RDS_msgTypeTag`
- L459 `typedef struct _c_GST_IPSU_RDS_msgTypeTag`
- L471 `typedef struct _c_CGW_05_200ms_RDS_msgTypeTag`
- L485 `typedef struct _c_CGW_04_200ms_RDS_msgTypeTag`
- L499 `typedef struct _c_CGW_03_200ms_RDS_msgTypeTag`
- L513 `typedef struct _c_CGW_02_200ms_RDS_msgTypeTag`
- L527 `typedef struct _c_CGW_01_200ms_RDS_msgTypeTag`
- L543 `typedef struct _c_IPSU_GST_RDS_msgTypeTag`
- L555 `typedef struct _c_IPSU_02_200ms_RDS_msgTypeTag`
- L567 `typedef struct _c_IPSU_01_200ms_RDS_msgTypeTag`
- L579 `typedef struct _c_IPSU_04_200ms_RDS_msgTypeTag`
- L591 `typedef struct _c_IPSU_03_200ms_RDS_msgTypeTag`
- L603 `typedef struct _c_IPSU_05_200ms_RDS_msgTypeTag`
- L615 `typedef struct _c_IPSU_14_200ms_RDS_msgTypeTag`
- L630 `typedef struct _c_IPSU_13_200ms_RDS_msgTypeTag`
- L643 `typedef struct _c_IPSU_12_200ms_RDS_msgTypeTag`
- L655 `typedef struct _c_IPSU_11_200ms_RDS_msgTypeTag`
- L667 `typedef struct _c_IPSU_10_200ms_RDS_msgTypeTag`
- L679 `typedef struct _c_IPSU_09_200ms_RDS_msgTypeTag`
- L691 `typedef struct _c_IPSU_08_200ms_RDS_msgTypeTag`
- L703 `typedef struct _c_IPSU_07_200ms_RDS_msgTypeTag`
- L715 `typedef struct _c_IPSU_06_200ms_RDS_msgTypeTag`
- L727 `typedef struct _c_DPSS_02_200ms_RDS_msgTypeTag`
- L747 `typedef union _c_RDS0_bufTag`
- L752 `typedef union _c_RDS1_bufTag`
- L757 `typedef union _c_RDS2_bufTag`
- L762 `typedef union _c_RDS3_bufTag`
- L767 `typedef union _c_RDS4_bufTag`
- L772 `typedef union _c_RDS5_bufTag`
- L777 `typedef union _c_RDSBasic_bufTag`
- L782 `typedef union _c_RDS_Tx_bufTag`

#### 주요 전역 변수/테이블 선언
- L457 `} _c_GST_ALL_RDS_msgType;`
- L469 `} _c_GST_IPSU_RDS_msgType;`
- L483 `} _c_CGW_05_200ms_RDS_msgType;`
- L497 `} _c_CGW_04_200ms_RDS_msgType;`
- L511 `} _c_CGW_03_200ms_RDS_msgType;`
- L525 `} _c_CGW_02_200ms_RDS_msgType;`
- L541 `} _c_CGW_01_200ms_RDS_msgType;`
- L553 `} _c_IPSU_GST_RDS_msgType;`
- L565 `} _c_IPSU_02_200ms_RDS_msgType;`
- L577 `} _c_IPSU_01_200ms_RDS_msgType;`
- L589 `} _c_IPSU_04_200ms_RDS_msgType;`
- L601 `} _c_IPSU_03_200ms_RDS_msgType;`
- L613 `} _c_IPSU_05_200ms_RDS_msgType;`
- L628 `} _c_IPSU_14_200ms_RDS_msgType;`
- L641 `} _c_IPSU_13_200ms_RDS_msgType;`
- L653 `} _c_IPSU_12_200ms_RDS_msgType;`
- L665 `} _c_IPSU_11_200ms_RDS_msgType;`
- L677 `} _c_IPSU_10_200ms_RDS_msgType;`
- L689 `} _c_IPSU_09_200ms_RDS_msgType;`
- L701 `} _c_IPSU_08_200ms_RDS_msgType;`
- L713 `} _c_IPSU_07_200ms_RDS_msgType;`
- L725 `} _c_IPSU_06_200ms_RDS_msgType;`
- L745 `} _c_DPSS_02_200ms_RDS_msgType;`
- L749 `vuint8 _c[8];`
- L750 `_c_GST_ALL_RDS_msgType GST_ALL;`
- L751 `} _c_RDS0_buf;`
- L754 `vuint8 _c[8];`
- L755 `_c_GST_IPSU_RDS_msgType GST_IPSU;`
- L756 `} _c_RDS1_buf;`
- L759 `vuint8 _c[8];`
- L760 `_c_CGW_05_200ms_RDS_msgType CGW_05_200ms;`
- L761 `} _c_RDS2_buf;`
- L764 `vuint8 _c[8];`
- L765 `_c_CGW_04_200ms_RDS_msgType CGW_04_200ms;`
- L766 `} _c_RDS3_buf;`
- L769 `vuint8 _c[8];`
- L770 `_c_CGW_03_200ms_RDS_msgType CGW_03_200ms;`
- L771 `} _c_RDS4_buf;`
- L774 `vuint8 _c[8];`
- L775 `_c_CGW_02_200ms_RDS_msgType CGW_02_200ms;`
- L776 `} _c_RDS5_buf;`
- L779 `vuint8 _c[3];`
- L780 `_c_CGW_01_200ms_RDS_msgType CGW_01_200ms;`
- L781 `} _c_RDSBasic_buf;`
- L784 `vuint8 _c[8];`
- L785 `_c_IPSU_GST_RDS_msgType IPSU_GST;`
- L786 `_c_IPSU_02_200ms_RDS_msgType IPSU_02_200ms;`
- L787 `_c_IPSU_01_200ms_RDS_msgType IPSU_01_200ms;`
- L788 `_c_IPSU_04_200ms_RDS_msgType IPSU_04_200ms;`
- L789 `_c_IPSU_03_200ms_RDS_msgType IPSU_03_200ms;`
- L790 `_c_IPSU_05_200ms_RDS_msgType IPSU_05_200ms;`
- L791 `_c_IPSU_14_200ms_RDS_msgType IPSU_14_200ms;`
- L792 `_c_IPSU_13_200ms_RDS_msgType IPSU_13_200ms;`
- L793 `_c_IPSU_12_200ms_RDS_msgType IPSU_12_200ms;`
- L794 `_c_IPSU_11_200ms_RDS_msgType IPSU_11_200ms;`
- L795 `_c_IPSU_10_200ms_RDS_msgType IPSU_10_200ms;`
- L796 `_c_IPSU_09_200ms_RDS_msgType IPSU_09_200ms;`
- L797 `_c_IPSU_08_200ms_RDS_msgType IPSU_08_200ms;`
- L798 `_c_IPSU_07_200ms_RDS_msgType IPSU_07_200ms;`
- L799 `_c_IPSU_06_200ms_RDS_msgType IPSU_06_200ms;`
- L800 `_c_DPSS_02_200ms_RDS_msgType DPSS_02_200ms;`
- L801 `} _c_RDS_Tx_buf;`

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 3

- L33 `defined(C_MULTIPLE_RECEIVE_CHANNEL) || defined(C_SINGLE_RECEIVE_CHANNEL) #endif /* CODE CATEGORY 1 START */ extern vuint8 TpFuncPrecopy(CanRxInfoStructPtr rxStruct)` [선언]
- L64 `TpPrecopy(CanRxInfoStructPtr rxStruct)` [선언]
- L73 `TpDrvConfirmation(CanTransmitHandle txObject)` [선언]

### 4. 각 함수별 역할 설명

- `defined` (L33, 선언): Description: Toolversion: 06.00.09.01.20.03.74.00.00.00 Serial Number: CBD1200374 Customer Info: STMicroelectronics Application GmbH CBD Hyundai Kia Motors / HKMC Micro  STM SPC56x - SPC5604B Compiler Green Hills Compiler Generator Fwk   : GENy Generator Module: DrvCan__base Configuration   : D:\Software_Part\00_PRE\DAEWON\Premium\Geny\PRE_Premium_IPSU_R02_260519.gny ECU: Targe
- `TpPrecopy` (L64, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpDrvConfirmation` (L73, 선언): 송신 완료 통지나 확인 상태를 처리하고 상위 계층 콜백/플래그를 갱신한다.

### 5. 필요시 코드 부연 설명

이 파일은 생성 설정/선언 성격이 강해 별도 코드 전문 인용 없이 심볼 표로 역할을 설명했다.

## 파일이름 : ccl_cfg

- 경로: `src/CBD/Gen/ccl_cfg.h`
- 파일 종류: `.h`
- 파일 크기: 6,208 bytes
- 파일 내용: CCL 기능 스위치, 사용자 핸들, 콜백 선언과 주기 설정을 정의한다.

### 1. .c/.h 파일 이름

- `ccl_cfg.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 40
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 0

#### 주요 매크로
- L46 `__CCL_CFG_H__`
- L58 `CCL_DLL_VERSION` = `0x0201`
- L59 `CCL_DLL_BUGFIX_VERSION` = `0x00`
- L60 `CCL__COREDLL_VERSION` = `0x0314`
- L61 `CCL__COREDLL_RELEASE_VERSION` = `0x11`
- L66 `C_ENABLE_CCL`
- L68 `NO_CHANNEL_INFO_AVAILABLE` = `0xFF  /* This is used for the fatal error function when no Channel info is available. */`
- L70 `CCL_ENABLE_ERROR_HOOK` = `/* CclFatalError function is enabled */`
- L72 `CCL_ENABLE_DEBUG` = `/* enables the debug mode and switches the assertions to on */`
- L74 `CCL_DISABLE_EMC_WAKEUP`
- L76 `CCL_DISABLE_WAKEUP_REG`
- L77 `CCL_ENABLE_INTERNAL_REQUEST`
- L78 `CCL_DISABLE_EXTERNAL_REQUEST`
- L80 `CCL_ENABLE_CANBEDDED_HANDLING`
- L81 `CCL_DISABLE_SCHEDULE_TASK`
- L82 `CCL_ENABLE_CONTAINER_TASK`
- L83 `CCL_DISABLE_STOP_MODE_ECU`
- L84 `CCL_DISABLE_POWER_DOWN_MODE_ECU`
- L85 `CCL_ENABLE_CUSTOMER_MODE_ECU`
- L86 `CCL_DISABLE_NET_STATE_RESTRICTION`
- L88 `CCL_DISABLE_TRCV_PORT_INT`
- L90 `CCL_ENABLE_SYSTEM_SHUTDOWN`
- L92 `CCL_ENABLE_SW_COM_STATE`
- L94 `kCclNrOfSystemChannels` = `1`
- L96 `kCclNrOfChannels` = `1  /* number of used channels */`
- L98 `kCclNrOfNetworks` = `1  /* number of used networks */`
- L100 `CCL_DISABLE_MULTIPLE_NODES` = `/* no multiple nodes */`
- L102 `CCL_ENABLE_BUSOFF_START`
- L104 `CCL_DISABLE_BUSOFF_END`
- L106 `CCL_DISABLE_BUSSTART`
- L108 `kCclNetReqTableSize` = `1  /* size of network request tabless */`
- L109 `kCclNumberOfUser` = `2`
- L110 `kCclCycleTime` = `10`
- L114 `CCL_CommRequest` = `0`
- L115 `CCL_DiagReq_0` = `1`
- L122 `CclSet_CommRequest()` = `CclRequestCommunication(CCL_CommRequest)`
- L123 `CclSet_DiagReq_0()` = `CclRequestCommunication(CCL_DiagReq_0)`
- L127 `CclRel_CommRequest()` = `CclReleaseCommunication(CCL_CommRequest)`
- L128 `CclRel_DiagReq_0()` = `CclReleaseCommunication(CCL_DiagReq_0)`
- L154 `MAGIC_NUMBER` = `538736488`

#### 주요 typedef/구조체
- 없음

#### 주요 전역 변수/테이블 선언
- 없음

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 16

- L130 `CclComStart(void)` [선언]
- L131 `CclComStop(void)` [선언]
- L132 `CclComWait(void)` [선언]
- L133 `CclComResume(void)` [선언]
- L134 `ApplCclComStart(void)` [선언]
- L135 `ApplCclComStop(void)` [선언]
- L136 `ApplCclComWait(void)` [선언]
- L137 `ApplCclComResume(void)` [선언]
- L138 `CclBusOffStart(void)` [선언]
- L139 `CclBusOffEnd(void)` [선언]
- L140 `ApplCclBusOffStart(void)` [선언]
- L141 `ApplCclBusOffEnd(void)` [선언]
- L142 `ApplCclInit(void)` [선언]
- L143 `CclPollingTask(void)` [선언]
- L144 `Ccl_10_0msTaskCont(void)` [선언]
- L145 `Ccl_10_4msTaskCont(void)` [선언]

### 4. 각 함수별 역할 설명

- `CclComStart` (L130, 선언): ** Release Communication access macros ***
- `CclComStop` (L131, 선언): ** Release Communication access macros ***
- `CclComWait` (L132, 선언): ** Release Communication access macros ***
- `CclComResume` (L133, 선언): ** Release Communication access macros ***
- `ApplCclComStart` (L134, 선언): ** Release Communication access macros ***
- `ApplCclComStop` (L135, 선언): ** Release Communication access macros ***
- `ApplCclComWait` (L136, 선언): ** Release Communication access macros ***
- `ApplCclComResume` (L137, 선언): ** Release Communication access macros ***
- `CclBusOffStart` (L138, 선언): ** Release Communication access macros ***
- `CclBusOffEnd` (L139, 선언): ** Release Communication access macros ***
- `ApplCclBusOffStart` (L140, 선언): ** Release Communication access macros ***
- `ApplCclBusOffEnd` (L141, 선언): ** Release Communication access macros ***
- `ApplCclInit` (L142, 선언): ** Release Communication access macros ***
- `CclPollingTask` (L143, 선언): ** Release Communication access macros ***
- `Ccl_10_0msTaskCont` (L144, 선언): ** Release Communication access macros ***
- `Ccl_10_4msTaskCont` (L145, 선언): ** Release Communication access macros ***

### 5. 필요시 코드 부연 설명

이 파일은 생성 설정/선언 성격이 강해 별도 코드 전문 인용 없이 심볼 표로 역할을 설명했다.

## 파일이름 : ccl_par

- 경로: `src/CBD/Gen/ccl_par.c`
- 파일 종류: `.c`
- 파일 크기: 26,542 bytes
- 파일 내용: CCL 네트워크/채널/사용자별 생성 파라미터 테이블을 정의한다.

### 1. .c/.h 파일 이름

- `ccl_par.c`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 1
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 5

#### 주요 매크로
- L45 `CCL_PAR_MODULE`

#### 주요 typedef/구조체
- 없음

#### 주요 전역 변수/테이블 선언
- L57 `V_MEMROM0 V_MEMROM1 vuint8 V_MEMROM2 cclStartIndex[1] = {`
- L61 `V_MEMROM0 V_MEMROM1 vuint8 V_MEMROM2 cclStopIndex[1] = {`
- L64 `V_MEMROM0 V_MEMROM1 vuint8 V_MEMROM2 cclUserOffset[kCclNumberOfUser] = {`
- L68 `V_MEMROM0 V_MEMROM1 vuint8 V_MEMROM2 cclUserMask[kCclNumberOfUser] = {`
- L72 `V_MEMROM0 V_MEMROM1 vuint8 V_MEMROM2 cclUserChannel[kCclNumberOfUser] = {`

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 23

- L80 `CclInitPowerOnFct(void ) | CALLED BY: CclInitPowerOn | PRECONDITIONS: Application is NOT ALLOWED to call this function! | INPUT PARAMETERS: void | RETURN VALUE: void | DESCRIPTION: This function is a generated init container function. It | contains different init routines depending on the | used modules. |*****************************************************************************/ void CclInitPowerOnFct(void)` [정의]
- L105 `CclInitFct(void ) | CALLED BY: CclInit | PRECONDITIONS: Application is NOT ALLOWED to call this function! | INPUT PARAMETERS: void | RETURN VALUE: void | DESCRIPTION: This function is a generated init container function. It | contains different init routines depending on the | used modules. |*****************************************************************************/ void CclInitFct(void)` [정의]
- L128 `CclSystemStartupFct(void ) | CALLED BY: CclSystemStartup | PRECONDITIONS: Application is NOT ALLOWED to call this function! | INPUT PARAMETERS: void | RETURN VALUE: void | DESCRIPTION: This function is a generated system startup container | function. It could be used to start services or initialize | variables before the system goes into the "normal" mode. |******************************************************************************/ void CclSystemStartupFct(void)` [정의]
- L143 `CclSystemShutdownFct(void ) | CALLED BY: CclSystemShutdown | PRECONDITIONS: Application is NOT ALLOWED to call this function! | INPUT PARAMETERS: void | RETURN VALUE: void | DESCRIPTION: This function is a generated system shutdown container | function. It could be used to stop services or deinitialize | variables after the system leaves the "normal" mode. |*************************************************************************************/ void CclSystemShutdownFct(void)` [정의]
- L158 `CclPollingTask(void ) | CALLED BY: Application or operating system | PRECONDITIONS: looptime < callcycle | INPUT PARAMETERS: void | RETURN VALUE: void | DESCRIPTION: This function is a generated polling task container. It | contains different cyclic routines | depending on the used modules. |*********************************************************************************/ void CCL_API_CALL_TYPE CclPollingTask (void)` [정의]
- L173 `Ccl_10_0msTaskCont(void ) | CALLED BY: task handler | PRECONDITIONS: | INPUT PARAMETERS: void | RETURN VALUE: void | DESCRIPTION: This function is a generated <x> ms task container | function. It contains different cyclic routines | depending on the used modules. |***********************************************************************************************/ void CCL_API_CALL_TYPE Ccl_10_0msTaskCont(void)` [정의]
- L197 `Ccl_10_4msTaskCont(void ) | CALLED BY: task handler | PRECONDITIONS: | INPUT PARAMETERS: void | RETURN VALUE: void | DESCRIPTION: This function is a generated <x> ms task container | function. It contains different cyclic routines | depending on the used modules. |***********************************************************************************************/ void CCL_API_CALL_TYPE Ccl_10_4msTaskCont(void)` [정의]
- L219 `CclComStart(void) | CALLED BY: network management | PRECONDITIONS: Application is NOT ALLOWED to call this function! | INPUT PARAMETERS: | RETURN VALUE: void | DESCRIPTION: This function start the communication with interaction layer. |*****************************************************************************/ void CclComStart(void)` [정의]
- L235 `CclComStop(void) | CALLED BY: network management | PRECONDITIONS: Application is NOT ALLOWED to call this function! | INPUT PARAMETERS: | RETURN VALUE: void | DESCRIPTION: This function stop the communication with interaction layer. |*****************************************************************************/ void CclComStop(void)` [정의]
- L251 `CclComWait(void) | CALLED BY: network management | PRECONDITIONS: Application is NOT ALLOWED to call this function! | INPUT PARAMETERS: | RETURN VALUE: void | DESCRIPTION: This function start the communication with interaction layer. |*****************************************************************************/ void CclComWait(void)` [정의]
- L267 `CclComResume(void) | CALLED BY: network management | PRECONDITIONS: Application is NOT ALLOWED to call this function! | INPUT PARAMETERS: | RETURN VALUE: void | DESCRIPTION: This function stop the communication with interaction layer. |*****************************************************************************/ void CclComResume(void)` [정의]
- L287 `CclBusOffStart(void) | CALLED BY: network management | PRECONDITIONS: Application is NOT ALLOWED to call this function! | INPUT PARAMETERS: , | RETURN VALUE: void | DESCRIPTION: This function stop the communication with interaction layer. |*****************************************************************************/ void CclBusOffStart(void)` [정의]
- L304 `CclBusOffEnd(void) | CALLED BY: network management | PRECONDITIONS: Application is NOT ALLOWED to call this function! | INPUT PARAMETERS: | RETURN VALUE: void | DESCRIPTION: This function start the communication with interaction layer. |*****************************************************************************/ void CclBusOffEnd(void)` [정의]
- L322 `CclNmActiveReqFct(vuint8 network ) | CALLED BY: CclTask | PRECONDITIONS: Application is NOT ALLOWED to call this function! | INPUT PARAMETERS: vsint8 network | RETURN VALUE: vsint8 | DESCRIPTION: This function contains the algorithm to go in the active | mode. The algorithm depends on the used modules. |*********************************************************************************/ vsint8 CclNmActiveReqFct(vuint8 network)` [정의]
- L342 `CclNmPrepareSleepReqFct(void ) | CALLED BY: CclTask | PRECONDITIONS: Application is NOT ALLOWED to call this function! | INPUT PARAMETERS: void | RETURN VALUE: void | DESCRIPTION: This function contains the algorithm to set the system in | prepare sleep mode. The algorithm depends on the used | modules. |***********************************************************************************/ void CclNmPrepareSleepReqFct(void)` [정의]
- L357 `CclNmSleepReqFct(vuint8 network ) | CALLED BY: CclRelNetRequest | PRECONDITIONS: Application is NOT ALLOWED to call this function! | INPUT PARAMETERS: vuint8 network | RETURN VALUE: void | DESCRIPTION: This function contains the algorithm to go in the sleep | mode. The algorithm depends on the used modules. |**********************************************************************************/ void CclNmSleepReqFct(vuint8 network)` [정의]
- L379 `ApplDescFatalError(vuint8 errorCode, vuint16 lineNumber) | CALLED BY: diagx module | PRECONDITIONS: Application is NOT ALLOWED to call this function! | INPUT PARAMETERS: errorCode, | lineNumber | RETURN VALUE: void | DESCRIPTION: This function is mapped to the CclFatalError function. |*******************************************************************************************/ void DESC_API_CALLBACK_TYPE ApplDescFatalError(vuint8 errorCode, vuint16 lineNumber)` [정의]
- L395 `ApplCanFatalError(CAN_CHANNEL_CANTYPE_FIRST canuint8 errorNumber) | CALLED BY: can driver module | PRECONDITIONS: Application is NOT ALLOWED to call this function! | INPUT PARAMETERS: channel (when have a multichannel system), | errorNumber | RETURN VALUE: void | DESCRIPTION: This function is mapped to the CclFatalError function. |******************************************************************************************/ void ApplCanFatalError(canuint8 errorNumber)` [정의]
- L411 `ApplIlFatalError(vuint8 errorNumber) | CALLED BY: vector interaction layer module | PRECONDITIONS: Application is NOT ALLOWED to call this function! | INPUT PARAMETERS: ErrCode | RETURN VALUE: void | DESCRIPTION: This function is mapped to the CclFatalError function. |*****************************************************************************/ void ApplIlFatalError(vuint8 errorNumber)` [정의]
- L426 `ApplTpFatalError(canuint8 errorNumber) | CALLED BY: can driver module | PRECONDITIONS: Application is NOT ALLOWED to call this function! | INPUT PARAMETERS: channel (when have a multichannel system), | errorNumber | RETURN VALUE: void | DESCRIPTION: This function is mapped to the CclFatalError function. |*****************************************************************************/ void TP_API_CALLBACK_TYPE ApplTpFatalError(canuint8 errorNumber)` [정의]
- L440 `CclInitTrcvFct(void ) | CALLED BY: CclInitPortsPowerOn | PRECONDITIONS: Application is NOT ALLOWED to call this function! | INPUT PARAMETERS: void | RETURN VALUE: void | DESCRIPTION: This function contains the transceiver init routine. | The routine depends on the used transceiver type. |*******************************************************************************/ void CclInitTrcvFct(void)` [정의]
- L456 `CclWakeUpTrcvFct(void ) | CALLED BY: ApplNmCanNormal | PRECONDITIONS: Application is NOT ALLOWED to call this function! | INPUT PARAMETERS: void | RETURN VALUE: void | DESCRIPTION: This function contains the routine to wakeup the | transceiver. The routine depends on the used transceiver | type. |***********************************************************************************/ void CclWakeUpTrcvFct(void)` [정의]
- L471 `CclSleepTrcvFct(void ) | CALLED BY: ApplNmCanSleep | PRECONDITIONS: Application is NOT ALLOWED to call this function! | INPUT PARAMETERS: void | RETURN VALUE: void | DESCRIPTION: This function contains the routine to switch the | transceiver into sleep mode. The routine depends on | the used transceiver type. |***********************************************************************************/ void CclSleepTrcvFct(void)` [정의]

### 4. 각 함수별 역할 설명

- `CclInitPowerOnFct` (L80, 정의): 전원 인가 직후 한 번 수행되는 모듈 전역 초기화를 담당한다.
- `CclInitFct` (L105, 정의): NAME:             CclInitPowerOnFct PROTOTYPE:        void CclInitPowerOnFct( void ) CALLED BY:        CclInitPowerOn PRECONDITIONS:    Application is NOT ALLOWED to call this function! INPUT PARAMETERS: void RETURN VALUE:     void DESCRIPTION:      This funct
- `CclSystemStartupFct` (L128, 정의): NAME:             CclInitFct PROTOTYPE:        void CclInitFct( void ) CALLED BY:        CclInit PRECONDITIONS:    Application is NOT ALLOWED to call this function! INPUT PARAMETERS: void RETURN VALUE:     void DESCRIPTION:      This function is a generated in
- `CclSystemShutdownFct` (L143, 정의): NAME:             CclSystemStartupFct PROTOTYPE:        void CclSystemStartupFct( void ) CALLED BY:        CclSystemStartup PRECONDITIONS:    Application is NOT ALLOWED to call this function! INPUT PARAMETERS: void RETURN VALUE:     void DESCRIPTION:      This
- `CclPollingTask` (L158, 정의): NAME:             CclSystemShutdownFct PROTOTYPE:        void CclSystemShutdownFct( void ) CALLED BY:        CclSystemShutdown PRECONDITIONS:    Application is NOT ALLOWED to call this function! INPUT PARAMETERS: void RETURN VALUE:     void DESCRIPTION:      T
- `Ccl_10_0msTaskCont` (L173, 정의): NAME:             CclPollingTask PROTOTYPE:        void CclPollingTask( void ) CALLED BY:        Application or operating system PRECONDITIONS:    looptime < callcycle INPUT PARAMETERS: void RETURN VALUE:     void DESCRIPTION:      This function is a generated
- `Ccl_10_4msTaskCont` (L197, 정의): NAME:             Ccl_10_0msTaskCont PROTOTYPE:        void Ccl_10_0msTaskCont( void ) CALLED BY:        task handler PRECONDITIONS: INPUT PARAMETERS: void RETURN VALUE:     void DESCRIPTION:      This function is a generated <x> ms task container function. It
- `CclComStart` (L219, 정의): NAME:             Ccl_10_4msTaskCont PROTOTYPE:        void Ccl_10_4msTaskCont( void ) CALLED BY:        task handler PRECONDITIONS: INPUT PARAMETERS: void RETURN VALUE:     void DESCRIPTION:      This function is a generated <x> ms task container function. It
- `CclComStop` (L235, 정의): NAME:             CclComStart PROTOTYPE:        void CclComStart(void) CALLED BY:        network management PRECONDITIONS:    Application is NOT ALLOWED to call this function! INPUT PARAMETERS: RETURN VALUE:     void DESCRIPTION:      This function start the c
- `CclComWait` (L251, 정의): NAME:             CclComStop PROTOTYPE:        void CclComStop(void) CALLED BY:        network management PRECONDITIONS:    Application is NOT ALLOWED to call this function! INPUT PARAMETERS: RETURN VALUE:     void DESCRIPTION:      This function stop the comm
- `CclComResume` (L267, 정의): NAME:             CclComWait PROTOTYPE:        void CclComWait(void) CALLED BY:        network management PRECONDITIONS:    Application is NOT ALLOWED to call this function! INPUT PARAMETERS: RETURN VALUE:     void DESCRIPTION:      This function start the com
- `CclBusOffStart` (L287, 정의): NAME:             CclComResume PROTOTYPE:        void CclComResume(void) CALLED BY:        network management PRECONDITIONS:    Application is NOT ALLOWED to call this function! INPUT PARAMETERS: RETURN VALUE:     void DESCRIPTION:      This function stop the 
- `CclBusOffEnd` (L304, 정의): NAME:             CclBusOffStart PROTOTYPE:        void CclBusOffStart(void) CALLED BY:        network management PRECONDITIONS:    Application is NOT ALLOWED to call this function! INPUT PARAMETERS:                                          , RETURN VALUE:    
- `CclNmActiveReqFct` (L322, 정의): NAME:             CclBusOffEnd PROTOTYPE:        void CclBusOffEnd(void) CALLED BY:        network management PRECONDITIONS:    Application is NOT ALLOWED to call this function! INPUT PARAMETERS: RETURN VALUE:     void DESCRIPTION:      This function start the
- `CclNmPrepareSleepReqFct` (L342, 정의): NAME:             CclNmActiveReqFct PROTOTYPE:        vsint8 CclNmActiveReqFct( vuint8 network ) CALLED BY:        CclTask PRECONDITIONS:    Application is NOT ALLOWED to call this function! INPUT PARAMETERS: vsint8 network RETURN VALUE:     vsint8 DESCRIPTION
- `CclNmSleepReqFct` (L357, 정의): NAME:             CclNmPrepareSleepReqFct PROTOTYPE:        void CclNmPrepareSleepReqFct( void ) CALLED BY:        CclTask PRECONDITIONS:    Application is NOT ALLOWED to call this function! INPUT PARAMETERS: void RETURN VALUE:     void DESCRIPTION:      This 
- `ApplDescFatalError` (L379, 정의): NAME:             CclNmSleepReqFct PROTOTYPE:        void CclNmSleepReqFct( vuint8 network ) CALLED BY:        CclRelNetRequest PRECONDITIONS:    Application is NOT ALLOWED to call this function! INPUT PARAMETERS: vuint8 network RETURN VALUE:     void DESCRIPT
- `ApplCanFatalError` (L395, 정의): NAME:             ApplDescFatalError PROTOTYPE:        void ApplDescFatalError(vuint8 errorCode, vuint16 lineNumber) CALLED BY:        diagx module PRECONDITIONS:    Application is NOT ALLOWED to call this function! INPUT PARAMETERS: errorCode, lineNumber RETU
- `ApplIlFatalError` (L411, 정의): NAME:             ApplCanFatalError PROTOTYPE:        void ApplCanFatalError(CAN_CHANNEL_CANTYPE_FIRST canuint8 errorNumber) CALLED BY:        can driver module PRECONDITIONS:    Application is NOT ALLOWED to call this function! INPUT PARAMETERS: channel (when
- `ApplTpFatalError` (L426, 정의): NAME:             ApplIlFatalError PROTOTYPE:        void ApplIlFatalError(vuint8 errorNumber) CALLED BY:        vector interaction layer module PRECONDITIONS:    Application is NOT ALLOWED to call this function! INPUT PARAMETERS: ErrCode RETURN VALUE:     voi
- `CclInitTrcvFct` (L440, 정의): NAME:             ApplTpFatalError PROTOTYPE:        void ApplTpFatalError(canuint8 errorNumber) CALLED BY:        can driver module PRECONDITIONS:    Application is NOT ALLOWED to call this function! INPUT PARAMETERS: channel (when have a multichannel system)
- `CclWakeUpTrcvFct` (L456, 정의): RS Pin, configuration PORT, OUTPUT, LOW
- `CclSleepTrcvFct` (L471, 정의): NAME:             CclWakeUpTrcvFct PROTOTYPE:        void CclWakeUpTrcvFct( void ) CALLED BY:        ApplNmCanNormal PRECONDITIONS:    Application is NOT ALLOWED to call this function! INPUT PARAMETERS: void RETURN VALUE:     void DESCRIPTION:      This functi

### 5. 필요시 코드 부연 설명

이 파일은 생성 설정/선언 성격이 강해 별도 코드 전문 인용 없이 심볼 표로 역할을 설명했다.

## 파일이름 : ccl_par

- 경로: `src/CBD/Gen/ccl_par.h`
- 파일 종류: `.h`
- 파일 크기: 4,160 bytes
- 파일 내용: CCL 네트워크/채널/사용자별 생성 파라미터 테이블을 정의한다.

### 1. .c/.h 파일 이름

- `ccl_par.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 6
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 6

#### 주요 매크로
- L46 `__CCL_PAR_H__`
- L60 `NM_OK` = `0`
- L64 `CCL_DISABLE_WAKEUP_EV`
- L66 `CCL_DISABLE_SLEEP_COND`
- L68 `CCL_DISABLE_STOP_TIMEOUT`
- L98 `MAGIC_NUMBER` = `538736488`

#### 주요 typedef/구조체
- 없음

#### 주요 전역 변수/테이블 선언
- L71 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 cclNmWakeUpAble;`
- L72 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 cclStartIndex[1];`
- L73 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 cclStopIndex[1];`
- L74 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 cclUserOffset[kCclNumberOfUser];`
- L75 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 cclUserMask[kCclNumberOfUser];`
- L76 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 cclUserChannel[kCclNumberOfUser];`

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 11

- L78 `CclInitPowerOnFct(void)` [선언]
- L79 `CclInitFct(void)` [선언]
- L80 `CclSystemStartupFct(void)` [선언]
- L81 `CclSystemShutdownFct(void)` [선언]
- L82 `CclNmSleepReqFct(vuint8 network)` [선언]
- L83 `CclNmActiveReqFct(vuint8 network)` [선언]
- L84 `CclNmPrepareSleepReqFct(void)` [선언]
- L85 `CclInitTrcvFct(void)` [선언]
- L86 `CclWakeUpTrcvFct(void)` [선언]
- L87 `CclSleepTrcvFct(void)` [선언]
- L88 `CanTask(void)` [선언]

### 4. 각 함수별 역할 설명

- `CclInitPowerOnFct` (L78, 선언): 전원 인가 직후 한 번 수행되는 모듈 전역 초기화를 담당한다.
- `CclInitFct` (L79, 선언): 모듈 런타임 상태, 버퍼, 카운터, 하드웨어/생성 파라미터를 초기화한다.
- `CclSystemStartupFct` (L80, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `CclSystemShutdownFct` (L81, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `CclNmSleepReqFct` (L82, 선언): CAN 채널의 슬립/웨이크업 전환과 관련 상태를 처리한다.
- `CclNmActiveReqFct` (L83, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `CclNmPrepareSleepReqFct` (L84, 선언): CAN 채널의 슬립/웨이크업 전환과 관련 상태를 처리한다.
- `CclInitTrcvFct` (L85, 선언): 모듈 런타임 상태, 버퍼, 카운터, 하드웨어/생성 파라미터를 초기화한다.
- `CclWakeUpTrcvFct` (L86, 선언): CAN 채널의 슬립/웨이크업 전환과 관련 상태를 처리한다.
- `CclSleepTrcvFct` (L87, 선언): CAN 채널의 슬립/웨이크업 전환과 관련 상태를 처리한다.
- `CanTask` (L88, 선언): 주기적으로 호출되어 타이머, 상태 머신, 송수신 대기 작업을 진행한다.

### 5. 필요시 코드 부연 설명

이 파일은 생성 설정/선언 성격이 강해 별도 코드 전문 인용 없이 심볼 표로 역할을 설명했다.

## 파일이름 : desc

- 경로: `src/CBD/Gen/desc.c`
- 파일 종류: `.c`
- 파일 크기: 325,026 bytes
- 파일 내용: CANdesc UDS 진단 디스패처 구현으로 세션, 보안, DID/RID, 응답 타이밍을 처리한다.

### 1. .c/.h 파일 이름

- `desc.c`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 397
- typedef/struct/enum 후보 수: 41
- 전역 변수/테이블 선언 후보 수: 223

#### 주요 매크로
- L35 `V_NULL` = `0`
- L57 `DESC_DISABLE_EXTERNAL_CHECK_TA`
- L60 `DESC_DISABLE_MULTI_TP_CHANNEL_SUPPORT`
- L64 `DESC_ENABLE_OEM_MH_MULTI_CALL_PROTECTION`
- L72 `DESC_ENABLE_PROCESSING_DONE_REDIRECTOR`
- L74 `DESC_DISABLE_PROCESSING_DONE_REDIRECTOR`
- L83 `DESC_MAIN_VERSION` = `0x06`
- L84 `DESC_SUB_VERSION` = `0x11`
- L85 `DESC_BUGFIX_VERSION` = `0x02`
- L88 `VSTDLIB__COREHLL_VERSION` = `0x00`
- L94 `DescInterruptDisable()` = `VStdSuspendAllInterrupts()`
- L95 `DescInterruptRestore()` = `VStdResumeAllInterrupts()`
- L97 `DescInterruptDisable()` = `(CanGlobalInterruptDisable())`
- L98 `DescInterruptRestore()` = `(CanGlobalInterruptRestore())`
- L102 `V_BOOL_EXPR(exp)` = `((DescBool)((exp)?kDescTrue:kDescFalse))`
- L106 `DESC_IMPLEMENTATION_MAGIC_NUMBER` = `7790`
- L109 `kDescDebugPatternLen` = `2`
- L111 `kDescDebugPatternLen` = `0`
- L115 `kDescAssertBufferLenPattern0` = `(vuint8)0xbe`
- L116 `kDescAssertBufferLenPattern1` = `(vuint8)0xda`
- L139 `kDescTpNumContexts` = `2`
- L141 `kDescTpNumContexts` = `1`
- L145 `DESC_TPCONTEXT_PARAM_TYPE_ONLY` = `vuint8`
- L146 `DESC_TPCONTEXT_PARAM_TYPE_FIRST` = `vuint8,`
- L147 `DESC_TPCONTEXT_PARAM_VALUE` = `tpContext`
- L148 `DESC_TPCONTEXT_PARAM_ONLY` = `tpContext`
- L149 `DESC_TPCONTEXT_PARAM_FIRST` = `tpContext,`
- L150 `DESC_TPCONTEXT_FORMAL_PARAM_DEF_ONLY` = `DESC_TPCONTEXT_PARAM_TYPE_ONLY DESC_TPCONTEXT_PARAM_ONLY`
- L151 `DESC_TPCONTEXT_PARAM_DEF_LOCAL` = `DESC_TPCONTEXT_FORMAL_PARAM_DEF_ONLY;`
- L152 `DESC_TPCONTEXT_FORMAL_PARAM_DEF_FIRST` = `DESC_TPCONTEXT_PARAM_TYPE_ONLY DESC_TPCONTEXT_PARAM_ONLY,`
- L153 `DESC_TPCONTEXT_PARAM_WRAPPER_ONLY(contextConst)` = `contextConst`
- L154 `DESC_TPCONTEXT_PARAM_WRAPPER_FIRST(contextConst)` = `contextConst,`
- L155 `DESC_TPCONTEXT_PARAM_WRAPPER_INDEX(contextConst)` = `contextConst`
- L156 `DESC_TPCONTEXT_PARAM_DUMMY_USE` = `DESC_IGNORE_UNREF_PARAM(DESC_TPCONTEXT_PARAM_ONLY)`
- L158 `DESC_TPCONTEXT_PARAM_TYPE_ONLY` = `void`
- L159 `DESC_TPCONTEXT_PARAM_TYPE_FIRST`
- L160 `DESC_TPCONTEXT_PARAM_VALUE` = `((vuint8)0)`
- L161 `DESC_TPCONTEXT_PARAM_ONLY`
- L162 `DESC_TPCONTEXT_PARAM_FIRST`
- L163 `DESC_TPCONTEXT_FORMAL_PARAM_DEF_ONLY` = `void`
- L164 `DESC_TPCONTEXT_FORMAL_PARAM_DEF_FIRST`
- L165 `DESC_TPCONTEXT_PARAM_DEF_LOCAL`
- L166 `DESC_TPCONTEXT_PARAM_WRAPPER_ONLY(contextConst)`
- L167 `DESC_TPCONTEXT_PARAM_WRAPPER_FIRST(contextConst)`
- L168 `DESC_TPCONTEXT_PARAM_WRAPPER_INDEX(contextConst)` = `0`
- L169 `DESC_TPCONTEXT_PARAM_DUMMY_USE`
- L173 `kDescDebugNeedTpRxChannel_BusyRepReq` = `1`
- L174 `kDescDebugNeedTpTxChannel_BusyRepReq` = `1`
- L176 `kDescDebugNeedTpRxChannel_BusyRepReq` = `0`
- L177 `kDescDebugNeedTpTxChannel_BusyRepReq` = `0`
- L181 `kDescDebugNeedTpRxChannel_ParallelObd` = `1`
- L182 `kDescDebugNeedTpTxChannel_ParallelObd` = `1`
- L184 `kDescDebugNeedTpRxChannel_ParallelObd` = `0`
- L185 `kDescDebugNeedTpTxChannel_ParallelObd` = `0`
- L189 `kDescNeededRxTpChannels` = `(kDescDebugNeedTpRxChannel_BusyRepReq + kDescDebugNeedTpRxChannel_ParallelObd + 1)`
- L190 `kDescNeededTxTpChannels` = `(kDescDebugNeedTpTxChannel_BusyRepReq + kDescDebugNeedTpTxChannel_ParallelObd + 1)`
- L195 `kDescCan2TpChannel_0` = `kTpNoChannel`
- L198 `kDescCan2TpChannel_1` = `kTpNoChannel`
- L201 `kDescCan2TpChannel_2` = `kTpNoChannel`
- L204 `kDescCan2TpChannel_3` = `kTpNoChannel`
- L208 `kDescEcuNumber` = `TP_ECU_NUMBER /* Use the simple macro */`
- L210 `kDescEcuNumber` = `(TP_RX_ECU_NR(0))`
- L214 `kDescUsdtNetFuncInfPoolRef` = `(vuintx)(kTpRxChannelCount)`
- L216 `kDescUsdtNetFuncInfPoolRef` = `kDescPrimContext`
- L218 `kDescUsdtNetSecInfoPool` = `(vuintx)(kDescNumContexts - 1)`
- L221 `DescGetConnectionOfContext(tpContext)` = `(g_descDescConnections[tpContext])`
- L223 `DescGetConnectionOfContext(tpContext)` = `kDescDiagConnection`
- L227 `DanisIsoTpTransmit(TP_CHANNEL_TX_PARAM_NAME, dataPtr, dataLen)` = `(DOBTTransmit(TP_CHANNEL_TX_PARAM_FIRST (dataPtr), (dataLen)))`
- L229 `DanisIsoTpTransmit(TP_CHANNEL_TX_PARAM_NAME, dataPtr, dataLen)` = `(TpTransmit(TP_CHANNEL_TX_PARAM_FIRST (dataPtr), (dataLen)))`
- L233 `TP_CHANNEL_RX_PARAM_VALUE` = `TP_CHANNEL_RX_PARAM_NAME`
- L234 `TP_CHANNEL_RX_PARAM_ONLY` = `TP_CHANNEL_RX_PARAM_NAME`
- L235 `TP_CHANNEL_RX_PARAM_FIRST` = `TP_CHANNEL_RX_PARAM_NAME,`
- L238 `TP_CHANNEL_TX_PARAM_VALUE` = `TP_CHANNEL_TX_PARAM_NAME`
- L239 `TP_CHANNEL_TX_PARAM_ONLY` = `TP_CHANNEL_TX_PARAM_NAME`
- L240 `TP_CHANNEL_TX_PARAM_FIRST` = `TP_CHANNEL_TX_PARAM_NAME,`
- L243 `TP_CHANNEL_TX_PARAM_VALUE` = `TP_CHANNEL_TX_PARAM_NAME`
- L245 `TP_CHANNEL_TX_PARAM_VALUE` = `0`
- L247 `TP_CHANNEL_TX_PARAM_ONLY` = `TP_CHANNEL_TX_PARAM_VALUE`
- L248 `TP_CHANNEL_TX_PARAM_FIRST` = `TP_CHANNEL_TX_PARAM_VALUE,`
- L251 `TP_CHANNEL_RX_PARAM_VALUE` = `0`
- L252 `TP_CHANNEL_RX_PARAM_ONLY`
- L253 `TP_CHANNEL_RX_PARAM_FIRST`
- L254 `TP_CHANNEL_TX_PARAM_VALUE` = `0`
- L255 `TP_CHANNEL_TX_PARAM_ONLY`
- L256 `TP_CHANNEL_TX_PARAM_FIRST`
- L257 `tpTxChannel` = `0`
- L258 `tpRxChannel` = `0`
- L294 `DESCNET_USDT_STATIC` = `static`
- L296 `kDescUsdtNetInvalidDescContext` = `(t_descHandle)(0xFF)`
- L299 `kDescDescConnectionStateIdle` = `((vuint8)0x00)`
- L300 `kDescDescConnectionStateActive` = `((vuint8)0x01)`
- L303 `kDescNrcResponsePending` = `0x78`
- L307 `kDescReadDynPidConnStateActive` = `((vuint8)(kDescDescConnectionStateActive<<kDescReadDynPidContext))`
- L309 `kDescReadDynPidConnStateActive` = `((vuint8)(0))`
- L312 `kDescRoeConnStateActive` = `((vuint8)((kDescDescConnectionStateActive<<kDescRoeHelperContext) | \`
- L315 `kDescRoeConnStateActive` = `((vuint8)(0))`
- L319 `kDescDefaultResOnReq` = `0x01`
- L322 `kDescMainReception` = `0x01`
- L323 `kDescMainTransmission` = `0x02`
- L324 `kDescAddReception` = `0x03`
- L325 `kDescAddTransmission` = `0x04`
- L329 `kDescTxModeIdle` = `0x00`
- L330 `kDescTxModeRegularRes` = `0x01`
- L331 `kDescTxModeSpecialRes` = `0x02`
- L332 `kDescTxModeRcrRpRes` = `0x04`
- L333 `kDescTxModePeriodicRes` = `0x08`
- L334 `kDescTxModeRoeResponder` = `0x10`
- L337 `kDescForcedRcrRpIdle` = `0x00`
- L338 `kDescForcedRcrRpCharged` = `0x01`
- L341 `kDescFirstFrameDataLength` = `6`
- L346 `kDescSingleFrameDataLength` = `((vuint8)(kDescFirstFrameDataLength + 1))`
- L348 `kDescNumPhysReqParallel` = `1`
- L349 `kDescNumFuncReqContexts` = `0`
- L350 `kDescNumPhysReqContexts` = `1`
- L352 `kDescTxSuccess` = `((vuint8)kDescUsdtNetworkOk)`
- L355 `kDescTimingRefP2` = `0`
- L356 `kDescTimingRefP2Star` = `1`
- L358 `kDescNrcRejectStateSession` = `kDescNrcSubfunctionNotSupportedInActiveSession`
- L359 `kDescNrcRejectStateSecurityAccess` = `kDescNrcAccessDenied`
- L360 `kDescStateGroupNumTransition` = `5`
- ... 생략 277개 (동일 패턴의 생성 심볼)

#### 주요 typedef/구조체
- L489 `typedef struct`
- L495 `typedef union`
- L501 `typedef struct`
- L507 `typedef struct`
- L516 `typedef struct`
- L522 `typedef struct`
- L530 `typedef struct t_descUsdtNetInfoPoolTag t_descUsdtNetInfoPool;`
- L531 `typedef struct t_descUsdtNetInfoPoolTag* t_descUsdtNetInfoPoolPtr;`
- L547 `typedef struct`
- L556 `typedef struct`
- L570 `typedef vuint16 DescS1Timer;`
- L572 `typedef vuint16 DescT2Timer;`
- L576 `typedef vuint16 DescRcrrpLimitTimer;`
- L579 `typedef struct DescStateInfoTag`
- L586 `typedef vuint16    DescSetStateIndex;`
- L588 `typedef vuint8     DescSetStateIndex;`
- L591 `typedef vuint8 DescSvcHeadIndex; /* Typedef for the SvcHead table indexes- 8/16Bit depeding on its size. */`
- L592 `typedef vuint8_least DescSvcInstIndex; /* Type definition for the SvcInstTable indexes, depending on the SvcInstTable size. */`
- L593 `typedef vuint8 DescMemSvcInstIndex; /* Type definition for the DescMemSvcInstIndex indexes, depending on the SvcInstTable size. */`
- L594 `typedef vuint8 DescPostHandlerIndex; /* Type definition for the Post-handler reference table, depending on its size. */`
- L595 `typedef vuint8_least DescSvcInstHeadExtIndex; /* Type definition for the SvcInstHeadExtTable indexes, depending on the SvcInstTable size. */`
- L596 `typedef vuint8 DescMemSvcInstHeadExtIndex; /* Type definition for the DescMemSvcInstHeadExtIndex indexes, depending on the SvcInstTable size. */`
- L598 `typedef void DESC_API_CALL_TYPE (*DescPreHandler)      (DESC_CONTEXT_PARAM_TYPE_ONLY);`
- L599 `typedef void DESC_API_CALL_TYPE (*DescPostHandler)     (DESC_CONTEXT_PARAM_TYPE_FIRST vuint8);`
- L601 `typedef struct`
- L633 `typedef struct`
- L655 `typedef vuint8_least DescPidInstIndex;`
- L656 `typedef vuint8 DescMemPidInstIndex;`
- L657 `typedef enum`
- L665 `typedef enum`
- L672 `typedef struct`
- L686 `typedef struct`
- L692 `typedef struct`
- L707 `typedef vuint8_least DescRidLookUpIndex;`
- L708 `typedef vuint8 DescMemRidLookUpIndex;`
- L709 `typedef vuint8_least DescRidInstIndex;`
- L710 `typedef vuint8 DescMemRidInstIndex;`
- L711 `typedef vuint8_least  DescRidControlType;`
- L712 `typedef struct`
- L726 `typedef struct`
- L736 `typedef struct`

#### 주요 전역 변수/테이블 선언
- L491 `vuint8 resBuffer[3];`
- L492 `vuint8 tpRxChannel;`
- L493 `} DescAddResBuffer;`
- L497 `vuint8           reqBuffer[7];`
- L498 `DescAddResBuffer res;`
- L499 `} DescAddBuffer;`
- L503 `vuint8        status;`
- L504 `DescAddBuffer buffers;`
- L505 `} DescAddChannel;`
- L509 `vuint8         count;`
- L510 `DescAddChannel channel[kDescNumAddRequestChannels];`
- L511 `} DescAddChannelCtrl;`
- L518 `DescBitType type:4;`
- L519 `DescBitType info:4;`
- L520 `} DescTpMemberCtrl;`
- L524 `DescTpMemberCtrl rxPath[kTpRxChannelCount];`
- L525 `DescTpMemberCtrl txPath[kTpTxChannelCount];`
- L526 `} DescTpCtrl;`
- L540 `vuint16                 dataLength;`
- L542 `DescUsdtNetMsg          reqDataPtr;`
- L543 `DescUsdtNetMsg          resDataPtr;`
- L552 `DescBitType         dummy                      :4;`
- L553 `} DescContextCtrl;`
- L563 `DescBitType        isContextLocked        :1;`
- L564 `} DescInterruptContextCtrl;`
- L581 `DescStateGroup stateSession : 3;`
- L582 `DescStateGroup stateSecurityAccess : 2;`
- L583 `DescStateGroup stateGap_0 : 3;`
- L584 `} DescStateInfo;`
- L623 `vuint8                  minReqLength;`
- L626 `DescVariantMask         variantMask;`
- L629 `DescMemSvcInstIndex        svcInstFirstItem;`
- L630 `DescMemSvcInstHeadExtIndex svcInstHeadExtFirstItem;`
- L631 `} DescSvcHead;`
- L635 `DescMsgLen             reqLen;`
- L636 `DescMsgAddInfo         msgAddInfo;`
- L638 `DescStateInfo          checkState;`
- L640 `DescSetStateIndex      setStateIndex;`
- L644 `DescVariantMask        variantMask;`
- L647 `DescPreHandlerIndex    preHandlerRef;`
- L650 `DescPostHandlerIndex   postHandlerRef;`
- L652 `DescMainHandler        mainHandler;`
- L653 `} DescSvcInst;`
- L663 `}DescPidAnalyseFailureReason;`
- L670 `}DescPidClient;`
- L675 `DescStateInfo        checkState;`
- L678 `DescPreHandlerIndex  preHandlerRef;`
- L680 `DescMsgAddInfo       msgAddInfo;`
- L682 `DescVariantMask      variantMask;`
- L684 `}DescPidTinyInfo;`
- L688 `DescMemPidInstIndex  pidHandle;`
- L689 `DescPidTinyInfo      tinyInfo;`
- L690 `}DescPidClientInfo;`
- L694 `vuint16              reqPid;`
- L698 `DescMsgLen           resDataLen;`
- L700 `DescPidTinyInfo      tinyInfo;`
- L702 `DescPostHandlerIndex postHandlerRef;`
- L704 `DescMainHandler      mainHandler;`
- L705 `} DescPidInst;`
- L715 `DescStateInfo        checkState;`
- L718 `DescPreHandlerIndex  preHandlerRef;`
- L720 `DescMsgAddInfo       msgAddInfo;`
- L722 `DescVariantMask      variantMask;`
- L724 `}DescRidTinyInfo;`
- L729 `DescRidTinyInfo      tinyInfo;`
- L731 `DescPostHandlerIndex postHandlerRef;`
- L733 `DescMainHandler      mainHandler;`
- L734 `} DescRidInst;`
- L738 `DescBitType    pidCount            :8;`
- L739 `DescBitType    curPid              :8;`
- L740 `DescBitType    isPidReady          :1;`
- L741 `DescBitType    isPidProcessed      :1;`
- L742 `DescBitType    dummyGap            :6;`
- L743 `}DescPidProcessorState;`
- L956 `static vuint8 g_descUsdtNetTpTxChannel[kDescTpNumContexts];`
- L959 `static t_descUsdtNetInfoPool  g_descUsdtNetInfoPoolIsoTp[kDescNumContexts];`
- L963 `static t_descUsdtNetInfoPoolPtr g_busInfoPoolTxRef[kTpTxChannelCount];`
- L969 `static DescTpCtrl g_descTpCtrl;`
- L990 `static vuint8_least  g_descCanChannelMap;`
- L992 `static vuint8_least  g_descCanChannelMapQueue;`
- L996 `static t_descUsdtNetInfoPool  g_descUsdtNetSpontanResInfoPool;`
- L1008 `static vuint8 g_descActiveRequestsMask;`
- L1012 `static DescContextCtrl          g_descContextCtrl[kDescNumContexts];`
- L1013 `static DescInterruptContextCtrl g_descInterruptContextCtrl[kDescNumContexts];`
- L1015 `static vuint8                   g_descTesterAddress[kDescNumContexts];`
- L1021 `static DescRcrrpLimitTimer g_descRcrrpLimitCounter[kDescNumContexts];`
- L1025 `static DescT2Timer  g_descT2Timer[kDescNumContexts];`
- L1027 `static DescS1Timer  g_descS1Timer;`
- L1039 `static vuint8 g_descCurrSessionNumber;`
- L1043 `static DescBool g_descDoReloadS1Timer;`
- L1046 `static vuint16 g_descNpmTimer;`
- L1050 `static DescStateInfo g_descCurState;`
- L1054 `static DescBool g_descIsOemMainHdlrAlreadyCalled;`
- L1058 `static DescSvcHeadIndex      g_descCurReqSvc[kDescNumContexts];`
- L1061 `static DescMemSvcInstIndex   g_descCurReqSvcInst[kDescNumContexts];`
- L1064 `static DescMsgContext     g_descMsgContext[kDescNumContexts];`
- L1067 `static DescNegResCode     g_descNegResCode[kDescNumContexts];`
- L1071 `static DescMainHandler    g_descRecallHandler[kDescNumContexts];`
- L1076 `static DescMsgItem        g_descUserSIdBackup[kDescNumContexts];`
- L1081 `static DescOemCommControlInfo  g_descCommControlInfo;`
- ... 생략 123개 (동일 패턴의 생성 심볼)

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 297

- L752 `CheckTableConsistency(void)` [선언]
- L755 `DescDebugIterInit(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L758 `DescUsdtNetIsoTpInitPowerOn(void)` [선언]
- L759 `DescUsdtNetIsoTpInit(void)` [선언]
- L761 `DescUsdtNetIsoTpStateTask(void)` [선언]
- L763 `DescUsdtNetIsoTpPrepareResponse(t_descUsdtNetInfoPoolPtr infoPool)` [선언]
- L764 `DescUsdtNetIsoTpTransmitResponse(t_descUsdtNetInfoPoolPtr infoPool)` [선언]
- L765 `DescUsdtNetIsoTpReleaseInfoPool(t_descUsdtNetInfoPoolPtr infoPool)` [선언]
- L768 `DescUsdtNetIsoTpGetRingBuffTxMinLen(t_descUsdtNetInfoPoolPtr infoPool)` [선언]
- L772 `DescBusyResponseHandler(void)` [선언]
- L775 `DescDispatchServiceContext(vuint8 sid)` [선언]
- L777 `DescUsdtNetIsoTpInitContext(DESC_TPCONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L778 `DescUsdtNetIsoTpCopyToCan(TpCopyToCanInfoStructPtr infoStruct)` [선언]
- L781 `DescUsdtNetLSFinishReception(t_descUsdtNetInfoPoolPtr infoPool, t_descUsdtNetResult status)` [선언]
- L782 `DescUsdtNetLSFinishTransmission(t_descUsdtNetInfoPoolPtr infoPool, t_descUsdtNetResult status)` [선언]
- L783 `DescUsdtNetLSStartReception(t_descUsdtNetInfoPoolPtr infoPool)` [선언]
- L786 `DescUsdtNetLSRingBufferCopyRoutine(t_descUsdtNetInfoPoolPtr infoPool, DescUsdtNetMsg pDestination, vuint8 dataLength)` [선언]
- L790 `DescUsdtNetAbsFinishReception(t_descUsdtNetInfoPoolPtr infoPool, t_descUsdtNetResult status)` [선언]
- L791 `DescUsdtNetAbsFinishTransmission(t_descUsdtNetInfoPoolPtr infoPool, t_descUsdtNetResult status)` [선언]
- L792 `DescUsdtNetAbsStartReception(t_descUsdtNetInfoPoolPtr infoPool)` [선언]
- L795 `DescUsdtNetAbsRingBufferCopyRoutine(t_descUsdtNetInfoPoolPtr infoPool, DescUsdtNetMsg pDestination, vuint8 dataLength)` [선언]
- L798 `DescNetworkIterInit(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L803 `DescIsTesterPresent(t_descUsdtNetInfoPoolPtr infoPool)` [선언]
- L806 `DescNetworkInitPowerOn(void)` [선언]
- L807 `DescNetworkInit(void)` [선언]
- L809 `DescTransmitRcrRp(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L810 `DescReleaseContext(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L812 `DescTimingOnceInit(void)` [선언]
- L813 `DescTimingIterInit(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L817 `DescOemNpmTimer(void)` [선언]
- L819 `DescNpmProcessQueue(void)` [선언]
- L820 `DescNpmSetSleepInd(void)` [선언]
- L824 `DescGetSessionStateBitPosition(DescStateGroup sessionState)` [선언]
- L831 `DescOnTransitionStateSession(DescStateGroup newState, DescStateGroup currentState)` [선언]
- L834 `DescStateOnceInit(void)` [선언]
- L837 `DescDoPostProcessing(DESC_CONTEXT_FORMAL_PARAM_DEF_FIRST t_descUsdtNetResult status)` [선언]
- L839 `DescDispatcherIterInit(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L841 `DescFindSvcInst(DescConstPtr reqHeadPtr, V_MEMROM1 DescSvcHead V_MEMROM2 V_MEMROM3 * pSvcHead, vuint8_least* failedByteMask)` [선언]
- L843 `DescFindSvc(DescMsgItem reqSvcId)` [선언]
- L845 `DescDispatcher(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L848 `DescGetSvcInstHeadExtEntrySize(V_MEMROM1 DescSvcHead V_MEMROM2 V_MEMROM3 * pSvcHead)` [선언]
- L850 `DescGetSvcInstReqHeadExt(V_MEMROM1 DescSvcHead V_MEMROM2 V_MEMROM3 * pSvcHead, DescSvcInstIndex svcInstAbsRef)` [선언]
- L851 `DescGetSvcInstResHeadExt(V_MEMROM1 DescSvcHead V_MEMROM2 V_MEMROM3 * pSvcHead, DescSvcInstIndex svcInstAbsRef)` [선언]
- L853 `DescGetSvcInstReqHeadExt(V_MEMROM1 DescSvcHead V_MEMROM2 V_MEMROM3 * pSvcHead, DescSvcInstIndex svcInstAbsRef)` [선언]
- L854 `DescGetSvcInstResHeadExt(V_MEMROM1 DescSvcHead V_MEMROM2 V_MEMROM3 * pSvcHead, DescSvcInstIndex svcInstAbsRef)` [선언]
- L859 `DescFinalProcessingDone(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L862 `DescContextStateTask(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L863 `DescResponseTimeoutTask(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L865 `DescOemStartSessionDefault(DescMsgContext* pMsgContext)` [선언]
- L866 `DescOemStartSessionProgramming(DescMsgContext* pMsgContext)` [선언]
- L867 `DescOemStartSessionExtended(DescMsgContext* pMsgContext)` [선언]
- L868 `DescReadDataByIdentifier(DescMsgContext* pMsgContext)` [선언]
- L869 `DescOemGetRequestSeed_Unlock_Level_1(DescMsgContext* pMsgContext)` [선언]
- L870 `DescOemSendSendKey_Unlock_Level_1(DescMsgContext* pMsgContext)` [선언]
- L871 `DescOemCommCtrlEnableRxEnableTx(DescMsgContext* pMsgContext)` [선언]
- L872 `DescOemCommCtrlEnableRxDisableTx(DescMsgContext* pMsgContext)` [선언]
- L873 `DescOemCommCtrlDisableRxEnableTx(DescMsgContext* pMsgContext)` [선언]
- L874 `DescOemCommCtrlDisableRxDisableTx(DescMsgContext* pMsgContext)` [선언]
- L875 `DescRoutineControlByIdentifier(DescMsgContext* pMsgContext)` [선언]
- L876 `DescOemProcessTesterPresent(DescMsgContext* pMsgContext)` [선언]
- L877 `DescPostReadDataByIdentifier(vuint8 status)` [선언]
- L878 `DescOemPostCommonCommCtrlProcess(vuint8 status)` [선언]
- L880 `DescDummyPreHandler(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L883 `DescDummyPostHandler(DESC_CONTEXT_FORMAL_PARAM_DEF_FIRST vuint8 status)` [선언]
- L886 `DescOemCommonCommCtrlProcess(vuint8 reqInfo, DescMsgContext *pMsgContext)` [선언]
- L887 `DescCommCtrlManipulateCAN(DESC_COMM_CHANNEL_FORMAL_PARAM_DEF_ONLY)` [선언]
- L889 `DescOemCheckAndExtractCommTypeParam(DescOemCommControlInfo * pCommControlInfo, DescMsgItem comType)` [선언]
- L893 `DescOemPrepareSessionControl(DescMsgContext *pMsgContext, DescStateGroup targetSession)` [선언]
- L895 `DescOemCommonSessionPostProcessing(DESC_CONTEXT_FORMAL_PARAM_DEF_FIRST vuint8 status, DescStateGroup newSession)` [선언]
- L899 `DescOemOnInvalidReq_27(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L903 `DescSecurityOnceInitPowerOn(void)` [선언]
- L904 `DescSecurityAccessTimer(void)` [선언]
- L908 `DescSecurityOnceInit(void)` [선언]
- L911 `DescOemSecuritySeed(DescMsgContext* pMsgContext, DescStateGroup requestedSecurityLevel)` [선언]
- L912 `DescOemSecurityKey(DescMsgContext* pMsgContext, DescStateGroup requestedSecurityLevel)` [선언]
- L916 `defined(DESC_ENABLE_SECURITY_ACCESS_ALGORITHM2)) static vuint8_least DescGetLevelIdxOfState(DescStateGroup requestedSecurityLevel)` [선언]
- L922 `DescPmGetPidPoolHandle(vuint16 pid)` [선언]
- L923 `DescPmGetAvailablePidHandle(vuint16 pid)` [선언]
- L925 `DescPmGetSupportedPidClientHandle(vuint16 pid, V_MEMROM1 DescPidClientInfo V_MEMROM2 V_MEMROM3 * pClientInfoTbl, DescPidInstIndex topOfTable)` [선언]
- L928 `DescPmGetPidClientHandle(DescPidInstIndex pidInfoHandle, V_MEMROM1 DescPidClientInfo V_MEMROM2 V_MEMROM3 * pClientInfoTbl, DescPidInstIndex topOfTable)` [선언]
- L930 `DescPmAnalysePid(DESC_CONTEXT_FORMAL_PARAM_DEF_FIRST V_MEMROM1 DescPidTinyInfo V_MEMROM2 V_MEMROM3 * pRefTinyInfo)` [선언]
- L932 `defined(DESC_ENABLE_DYN_DEFINED_DPID_MODE) static DescMsgLen DescPmGetPidResponseLen (DescPidInstIndex pidHandle)` [선언]
- L937 `DescRidFindRoutineId(vuint16 rid)` [선언]
- L938 `DescRidFindSubFunction(vuint8 subFuncId, DescRidInstIndex ridRef)` [선언]
- L939 `DescRidAnalyseRoutine(DESC_CONTEXT_FORMAL_PARAM_DEF_FIRST V_MEMROM1 DescRidTinyInfo V_MEMROM2 V_MEMROM3 * pRefTinyInfo, DescBool isOnRid)` [선언]
- L940 `DescRidProcessingDone(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L942 `DescIterInitPidListProcessor(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L944 `DescPidProcessorTask(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L945 `DescPidProcessingDone(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L946 `DescPidDispatcher(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L1423 `DescNetworkOnceInit(void)` [선언]
- L1559 `DescStateCompInitOnceGroup() (g_descIsOemMainHdlrAlreadyCalled = 0) #endif #if defined(DESC_ENABLE_AUTO_STATES) # define DescOemOnTransitionSession(newState,currentState) (DescOnTransitionStateSession((newState), (currentState))) #endif /* Internal API for INIT */ #if !defined(DescStateCompInitOnceOem) # define DescStateCompInitOnceOem() /* Not used */ #endif #if !defined(DescStateCompInitOnceGroup) # define DescStateCompInitOnceGroup() /* Not used */ #endif #if defined(DESC_ENABLE_AUTO_STATES) # define DescStateCompInitOnceCore() (DescStateOnceInit()) #else # define DescStateCompInitOnceCore() /* Not used */ #endif #define DescSubcompOnceInitPowerOnState() /* Not used */ #define DescSubcompOnceInitState()` [정의]
- L1588 `DescSetState(DescSvcInstIndex svcInstHandle)` [선언]
- L1592 `DescCheckState(V_MEMROM1 DescStateInfo V_MEMROM2 V_MEMROM3 * refState)` [선언]
- L1842 `defined(DESC_ENABLE_RES_PENDING_TIME_LIMIT) && \ defined(DESC_ENABLE_RES_PENDING_COUNT_LIMIT) # error "Both type of response pending limitations are not possible at the same time!" #endif #if defined (C_MULTIPLE_RECEIVE_CHANNEL) || defined (C_SINGLE_RECEIVE_CHANNEL) /* config ok */ #else /* code doubled system */ # if (kDescNumCommChannels > 1) # if defined (kDescCanChannel) /* config ok */ # else # error "Missing kDescCanChannel define!" # endif # endif #endif #if defined (DESC_ENABLE_DEBUG_INTERNAL) /******************************************************************************* * NAME: CheckIndexTableConsistency * * CALLED BY: DescInitPowerOn * PRECONDITIONS: * * DESCRIPTION: Performs check if the table relations are consistent, and * if all main handler functions are not V_NULL. * *******************************************************************************/ static void CheckTableConsistency(void)` [정의]
- L1940 `DescDebugIterInit(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L1961 `defined(DESC_ENABLE_NODE_POWER_CONTROL) && \ !defined (DESC_ENABLE_DEFAULT_SESSION_SLEEP_PREVENTION) && \ !defined (DESC_ENABLE_COMMON_OEM_POST_HANDLER) # error "Enable OEM-post-handler support on service $10" #endif /******************************************************************************* * NAME: DescUsdtNetIsoTpInitPowerOn * * CALLED BY: CANdesc * PRECONDITIONS: * * DESCRIPTION: Power on initialization * * *******************************************************************************/ DESCNET_USDT_STATIC void DescUsdtNetIsoTpInitPowerOn(void)` [정의]
- L1997 `DescUsdtNetIsoTpInitContext(DESC_TPCONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L2004 `TpTxGetConnectionStatus(DescGetConnectionOfContext(DESC_TPCONTEXT_PARAM_VALUE))` [선언]
- L2045 `DescUsdtNetIsoTpInit(void)` [정의]
- L2095 `DescUsdtNetIsoTpStateTask(void)` [정의]
- L2105 `DescCheckTA(vuint8 targetAddress)` [정의]
- L2144 `DescDispatchServiceContext(vuint8 sid)` [정의]
- L2166 `DescGetBuffer(TP_CHANNEL_RX_FORMAL_PARAM_DEF_FIRST vuint16 dataLength)` [정의]
- L2174 `DescOemCheckPhysTargetAddress(TpGetTargetAddress(TP_CHANNEL_RX_PARAM_ONLY))` [선언]
- L2179 `DescOemCheckPhysSourceAddress(TpRxGetSourceAddress(TP_CHANNEL_RX_PARAM_ONLY))` [선언]
- L2188 `DescDispatchServiceContext(TpRxGetCanBuffer(tpRxChannel)[0])` [선언]
- L2222 `defined(TP_ENABLE_MIXED_29_ADDRESSING) g_descUsdtNetInfoPoolIsoTp[DESC_TPCONTEXT_PARAM_VALUE].busInfo.addressingInfo.isoTp.SourceAddress = TpRxGetSourceAddress(TP_CHANNEL_RX_PARAM_ONLY)` [선언]
- L2233 `TpRxGetCanChannel(TP_CHANNEL_RX_PARAM_ONLY)` [선언]
- L2235 `TpRxGetCanChannel()` [선언]
- L2251 `TpRxGetTaType(TP_CHANNEL_RX_PARAM_ONLY)` [선언]
- L2279 `DescUsdtNetStartReception(&g_descUsdtNetInfoPoolIsoTp[DESC_TPCONTEXT_PARAM_VALUE])` [선언]
- L2334 `DescPhysReqInd(TP_CHANNEL_RX_FORMAL_PARAM_DEF_FIRST vuint16 dataLen)` [정의]
- L2356 `TpRxGetChannelID(TP_CHANNEL_RX_PARAM_ONLY)` [선언]
- L2371 `TpRxGetEcuNumber(TP_CHANNEL_RX_PARAM_ONLY)` [선언]
- L2381 `TpRxGetBaseAddress(TP_CHANNEL_RX_PARAM_ONLY)` [선언]
- L2446 `DescRxErrorIndication(TP_CHANNEL_RX_FORMAL_PARAM_DEF_FIRST vuint8 status)` [정의]
- L2492 `DescUsdtNetIsoTpPrepareResponse(t_descUsdtNetInfoPoolPtr infoPool)` [정의]
- L2521 `DescUsdtNetIsoTpTransmitResponse(t_descUsdtNetInfoPoolPtr infoPool)` [정의]
- L2578 `defined(TP_ENABLE_MIXED_29_ADDRESSING) TpTxSetTargetAddress (TP_CHANNEL_TX_PARAM_FIRST infoPool->busInfo.testerId)` [선언]
- L2588 `defined(TP_ENABLE_MIXED_29_ADDRESSING) # if (TP_USE_MULTIPLE_ECU_NR == kTpOn) TpTxSetEcuNumber(TP_CHANNEL_TX_PARAM_FIRST infoPool->busInfo.addressingInfo.isoTp.SourceAddress)` [선언]
- L2617 `DanisIsoTpTransmit(TP_CHANNEL_TX_PARAM_VALUE, infoPool->resDataPtr, infoPool->dataLength)` [선언]
- L2622 `TpTransmitDiag(infoPool->resDataPtr, infoPool->dataLength)` [선언]
- L2625 `DanisIsoTpTransmit(TP_CHANNEL_TX_PARAM_VALUE, infoPool->resDataPtr, infoPool->dataLength)` [선언]
- L2644 `DescUsdtNetIsoTpGetRingBuffTxMinLen(t_descUsdtNetInfoPoolPtr infoPool)` [정의]
- L2659 `DescUsdtNetIsoTpCopyToCan(TpCopyToCanInfoStructPtr infoStruct)` [정의]
- L2678 `DescCopyToCAN(TpCopyToCanInfoStructPtr infoStruct)` [정의]
- L2702 `DescUsdtNetRingBufferCopyRoutine(g_busInfoPoolTxRef[TP_CHANNEL_TX_PARAM_VALUE], &g_descCopyToCanData[0], (vuint8)infoStruct->Length)` [선언]
- L2705 `DescUsdtNetRingBufferCopyRoutine(g_busInfoPoolTxRef[TP_CHANNEL_TX_PARAM_VALUE], (DescUsdtNetMsg)infoStruct->pDestination, (vuint8)infoStruct->Length)` [선언]
- L2753 `DescConfirmation(TP_CHANNEL_TX_FORMAL_PARAM_DEF_FIRST vuint8 status)` [정의]
- L2806 `DescTxErrorIndication(TP_CHANNEL_TX_FORMAL_PARAM_DEF_FIRST vuint8 status)` [정의]
- L2852 `DescUsdtNetIsoTpReleaseInfoPool(t_descUsdtNetInfoPoolPtr infoPool)` [정의]
- L2869 `DescBusyResponseHandler(void)` [정의]
- L2883 `TpTxGetFreeChannel(kDescDiagAddConnection)` [선언]
- L2912 `DescGetFuncBuffer(vuint16 dataLength)` [정의]
- L2930 `DescOemCheckFuncTargetAddress(TpFuncGetTargetAddress())` [선언]
- L2935 `DescOemCheckFuncSourceAddress(TpFuncGetSourceAddress())` [선언]
- L2944 `DescDispatchServiceContext(TpFuncGetCanBuffer()[0])` [선언]
- L2952 `TpFuncGetCanChannel()` [선언]
- L2957 `defined(TP_ENABLE_MIXED_29_ADDRESSING) g_descUsdtNetInfoPoolIsoTp[DESC_TPCONTEXT_PARAM_VALUE].busInfo.addressingInfo.isoTp.SourceAddress = TpFuncGetSourceAddress()` [선언]
- L2975 `DescUsdtNetStartReception(&g_descUsdtNetInfoPoolIsoTp[DESC_TPCONTEXT_PARAM_VALUE])` [선언]
- L3004 `DescFuncReqInd(vuint16 dataLen)` [정의]
- L3027 `TpFuncGetReceiveCanID()` [선언]
- L3033 `TpFuncGetTargetAddress()` [선언]
- L3044 `TpFuncGetBaseAddress()` [선언]
- L3065 `DescNetworkOnceInit(void)` [정의]
- L3107 `DescNetworkIterInit(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L3140 `DescReleaseContext(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L3169 `DescSendSpontaneousResponse(DescMsg resData, DescMsgLen resLen, t_descUsdtNetBus* pBusInfo, t_descUsdtNetResType resType)` [정의]
- L3211 `DescIsTesterPresent(t_descUsdtNetInfoPoolPtr infoPool)` [정의]
- L3255 `DescNetworkInitPowerOn(void)` [정의]
- L3271 `DescNetworkInit(void)` [정의]
- L3286 `DescGetActivityState(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L3302 `DescICNRxMsgInd(t_descUsdtNetInfoPoolPtr infoPool, t_descUsdtNetResult status)` [정의]
- L3339 `DescUsdtNetAbsStartReception(t_descUsdtNetInfoPoolPtr infoPool)` [정의]
- L3393 `DescUsdtNetAbsFinishReception(t_descUsdtNetInfoPoolPtr infoPool, t_descUsdtNetResult status)` [정의]
- L3434 `DescUsdtNetAbsFinishTransmission(t_descUsdtNetInfoPoolPtr infoPool, t_descUsdtNetResult status)` [정의]
- L3545 `DescTransmitRcrRp(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L3553 `DescUsdtNetGetTransmissionPtr(g_descInterruptContextCtrl[DESC_CONTEXT_PARAM_VALUE].infoPoolPtr)` [선언]
- L3581 `DescGetTesterAddress(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L3595 `DescGetCurrentBusInfo(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L3609 `DescTimingIterInit(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L3625 `DescForceRcrRpResponse(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L3647 `DescTimingOnceInit(void)` [정의]
- L3670 `DescNpmSetSleepInd(void)` [정의]
- L3704 `DescNpmProcessQueue(void)` [정의]
- L3737 `DescGetMirroredQueue(g_descCanChannelMapQueue, canChannelMapMirror)` [선언]
- L3760 `DescOemNpmTimer(void)` [정의]
- L3788 `DescStateOnceInit(void)` [정의]
- L3793 `DescStateInit(void)` [정의]
- L3800 `DescGetStateSession(void)` [정의]
- L3806 `DescSetStateSession(DescStateGroup descState)` [정의]
- L3816 `DescGetStateSecurityAccess(void)` [정의]
- L3822 `DescSetStateSecurityAccess(DescStateGroup descState)` [정의]
- L3832 `DescCheckState(V_MEMROM1 DescStateInfo V_MEMROM2 V_MEMROM3* refState)` [정의]
- L3846 `DescSetState(DescSvcInstIndex svcInstHandle)` [정의]
- L3876 `DescGetSessionStateOfSessionId(DescMsgItem sessionId)` [정의]
- L3883 `DescGetSvcInstHeadExtEntrySize(&g_descSvcHead[kDescSvcHeadOffsetSDS])` [선언]
- L3886 `DescGetSvcInstReqHeadExt(&g_descSvcHead[kDescSvcHeadOffsetSDS], kDescSvcInstOffsetSDS)` [선언]
- L3929 `DescGetSessionStateBitPosition(DescStateGroup sessionState)` [정의]
- L3958 `DescGetSessionIdOfSessionState(DescStateGroup sessionState)` [정의]
- L3992 `DescGetSessionTimings(DescStateGroup sessionState, vuint16* p2Time_1ms, vuint16* p2ExTime_10ms)` [정의]
- L3999 `DescGetSessionStateBitPosition(sessionState)` [선언]
- L4024 `DescOnTransitionStateSession(DescStateGroup newState, DescStateGroup currentState)` [정의]
- L4083 `DescStartRepeatedServiceCall(DESC_CONTEXT_FORMAL_PARAM_DEF_FIRST DescMainHandler mainHandler)` [정의]
- L4099 `DescSetNegResponse(DESC_CONTEXT_FORMAL_PARAM_DEF_FIRST DescNegResCode errorCode)` [정의]
- L4119 `DescGetSvcInstHeadExtEntrySize(V_MEMROM1 DescSvcHead V_MEMROM2 V_MEMROM3 * pSvcHead)` [정의]
- L4167 `DescGetSvcInstHeadExtEntrySize(pSvcHead)` [선언]
- L4185 `DescFindSvcInst(DescConstPtr reqHeadPtr, V_MEMROM1 DescSvcHead V_MEMROM2 V_MEMROM3 * pSvcHead, vuint8_least* failedByteMask)` [정의]
- L4210 `DescGetSvcInstReqHeadExt(pSvcHead, idx)` [선언]
- L4215 `V_BOOL_EXPR(reqHeadPtr[currColFound] == headEx[currColFound])` [선언]
- L4278 `DescFindSvcInst(DescConstPtr reqHeadPtr, V_MEMROM1 DescSvcHead V_MEMROM2 V_MEMROM3 * pSvcHead, vuint8_least* failedByteMask)` [정의]
- L4288 `DescGetSvcInstHeadExtEntrySize(pSvcHead)` [선언]
- L4303 `V_BOOL_EXPR(reqHeadPtr[currColFound] == g_descSvcInstHeadExt[iter + currColFound])` [선언]
- L4358 `DescFindSvc(DescMsgItem reqSvcId)` [정의]
- L4414 `DescGetServiceId(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L4444 `DescDispatcherIterInit(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L4469 `DescDispatcher(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L4494 `DescFindSvc(msg[0])` [선언]
- L4546 `CANBITTYPE_CAST(((supPosResBit & *msg)!= 0)?0x01:0x00)` [선언]
- L4632 `DescExtractReqExtHeadLen(refDescSvcHead->reqHeadExLen))` [선언]
- L4634 `DescExtractResExtHeadLen(refDescSvcHead->resHeadExLen))` [선언]
- L4784 `DescProcessingDone(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L4795 `DescPidProcessingDone(DESC_CONTEXT_PARAM_ONLY)` [선언]
- L4946 `DescExtractResExtHeadLen((vuintx)(g_descSvcHead[g_descCurReqSvc[DESC_CONTEXT_PARAM_VALUE]].resHeadExLen))` [선언]
- L4948 `DescGetSvcInstResHeadExt(&g_descSvcHead[g_descCurReqSvc[DESC_CONTEXT_PARAM_VALUE]], g_descCurReqSvcInst[DESC_CONTEXT_PARAM_VALUE]), (vuint16)resHeadLen)` [선언]
- L5010 `DescDoPostProcessing(DESC_CONTEXT_FORMAL_PARAM_DEF_FIRST t_descUsdtNetResult status)` [정의]
- L5041 `DescInitPowerOn(DescInitParam initParam)` [정의]
- L5079 `DescInit(DescInitParam initParam)` [정의]
- L5150 `DescTimerTask(void)` [정의]
- L5246 `DescResponseTimeoutTask(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L5426 `DescTask(void)` [정의]
- L5446 `DescStateTask(void)` [정의]
- L5504 `DescContextStateTask(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L5697 `DescUsdtNetGetTransmissionPtr(g_descInterruptContextCtrl[DESC_CONTEXT_PARAM_VALUE].infoPoolPtr)` [선언]
- L5722 `DescOemStartSessionDefault(DescMsgContext* pMsgContext)` [정의]
- L5739 `DescOemStartSessionProgramming(DescMsgContext* pMsgContext)` [정의]
- L5756 `DescOemStartSessionExtended(DescMsgContext* pMsgContext)` [정의]
- L5772 `DescOemGetRequestSeed_Unlock_Level_1(DescMsgContext* pMsgContext)` [정의]
- L5788 `DescOemSendSendKey_Unlock_Level_1(DescMsgContext* pMsgContext)` [정의]
- L5804 `DescOemCommCtrlEnableRxEnableTx(DescMsgContext* pMsgContext)` [정의]
- L5820 `DescOemCommCtrlEnableRxDisableTx(DescMsgContext* pMsgContext)` [정의]
- L5836 `DescOemCommCtrlDisableRxEnableTx(DescMsgContext* pMsgContext)` [정의]
- L5852 `DescOemCommCtrlDisableRxDisableTx(DescMsgContext* pMsgContext)` [정의]
- L5868 `DescDummyPreHandler(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L5885 `DescDummyPostHandler(DESC_CONTEXT_FORMAL_PARAM_DEF_FIRST vuint8 status)` [정의]
- L5902 `DescIsSuppressPosResBitSet(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L5919 `DescOemProcessTesterPresent(DescMsgContext *pMsgContext)` [정의]
- L5938 `DescOemCheckAndExtractCommTypeParam(DescOemCommControlInfo * pCommControlInfo, DescMsgItem comType)` [정의]
- L5987 `DescOemCommonCommCtrlProcess(vuint8 reqInfo, DescMsgContext *pMsgContext)` [정의]
- L6007 `DescOemCheckAndExtractCommTypeParam(&g_descCommControlInfo, pMsgContext->reqData[0])` [선언]
- L6095 `DescOemPostCommonCommCtrlProcess(DESC_CONTEXT_FORMAL_PARAM_DEF_FIRST vuint8 status)` [정의]
- L6118 `channel(CAN)*/ DescCommCtrlManipulateCAN(DESC_COMM_CHANNEL_PARAM_WRAP_ONLY(g_descCommControlInfo.reqCommChannel))` [선언]
- L6161 `DescCommCtrlManipulateCAN(DESC_COMM_CHANNEL_FORMAL_PARAM_DEF_ONLY)` [정의]
- L6191 `DescOemPrepareSessionControl(DescMsgContext *pMsgContext, DescStateGroup targetSession)` [정의]
- L6214 `DescGetHiByte(p2Time)` [선언]
- L6215 `DescGetLoByte(p2Time)` [선언]
- L6217 `DescGetHiByte(p2ExTime)` [선언]
- L6218 `DescGetLoByte(p2ExTime)` [선언]
- L6241 `DescOemCommonSessionPostProcessing(DESC_CONTEXT_FORMAL_PARAM_DEF_FIRST vuint8 status, DescStateGroup newSession)` [정의]
- L6249 `ok(no negative response, no transmission failure) */ if((status & kDescPostHandlerStateOk) != 0)` [정의]
- L6282 `DescSessionTransitionChecked(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L6291 `defined(DESC_ENABLE_RESET_AWAIT_KEY_ON_INVALID_REQ) /******************************************************************************* * NAME: DescOemOnInvalidReq_27 * * CALLED BY: DescDispatcher * PRECONDITIONS: * * DESCRIPTION: Special security state handling on invalid request * *******************************************************************************/ static void DescOemOnInvalidReq_27(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L6324 `DescSecurityOnceInitPowerOn(void)` [정의]
- L6332 `DescGetTicksOfMs(kDescPowerOnSecureTimer)` [선언]
- L6347 `DescSecurityAccessTimer(void)` [정의]
- L6380 `DescGetAttemptCounterValue(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L6396 `DescSetAttemptCounterValue(DESC_CONTEXT_FORMAL_PARAM_DEF_FIRST vuint8 value)` [정의]
- L6411 `DescGetTicksOfMs(kDescSecureTimer)` [선언]
- L6424 `DescStartSecureTimer(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L6428 `DescGetTicksOfMs(kDescSecureTimer)` [선언]
- L6441 `DescSecurityOnceInit(void)` [정의]
- L6449 `defined(DESC_ENABLE_SECURITY_ACCESS_ALGORITHM2)) /****************************************************************************** * Name : DescGetLevelIdxOfState * Called by : SA component * Preconditions: None * Parameters : StateLevel * Description : Converts bit map into bit position ******************************************************************************/ static vuint8_least DescGetLevelIdxOfState(DescStateGroup securityLevel)` [정의]
- L6484 `DescOemSecuritySeed(DescMsgContext* pMsgContext, DescStateGroup requestedSecurityLevel)` [정의]
- L6488 `DescGetSeedLength(requestedSecurityLevel)` [선언]
- L6534 `DescGetTicksOfMs(kDescSecureTimer)` [선언]
- L6591 `DescSecurityAccessSeedReady(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L6610 `DescOemSecurityKey(DescMsgContext* pMsgContext, DescStateGroup requestedSecurityLevel)` [정의]
- L6663 `DescSecurityAccessKeyChecked(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L6695 `DescGetTicksOfMs(kDescSecureTimer)` [선언]
- L6735 `defined(DESC_ENABLE_DYN_DEFINED_DPID_MODE) /******************************************************************************* * NAME: DescPmGetPidResponseLen * * CALLED BY: * PRECONDITIONS: * * DESCRIPTION: * *******************************************************************************/ static DescMsgLen DescPmGetPidResponseLen(DescPidInstIndex pidHandle)` [정의]
- L6750 `DescPmClientGetResLength(pidHandle)` [선언]
- L6771 `DescPmAnalysePid(DESC_CONTEXT_FORMAL_PARAM_DEF_FIRST V_MEMROM1 DescPidTinyInfo V_MEMROM2 V_MEMROM3 * pRefTinyInfo)` [정의]
- L6782 `DescCheckState(&(pRefTinyInfo->checkState))` [선언]
- L6834 `DescPmGetPidClientHandle(DescPidInstIndex pidInfoHandle, V_MEMROM1 DescPidClientInfo V_MEMROM2 V_MEMROM3 * pClientInfoTbl, DescPidInstIndex topOfTable)` [정의]
- L6870 `DescPmGetAvailablePidHandle(vuint16 pid)` [정의]
- L6874 `DescPmGetPidPoolHandle(pid)` [선언]
- L6875 `DescPmClientCheckPid(result, pid)` [선언]
- L6890 `DescPmGetSupportedPidClientHandle(vuint16 pid, V_MEMROM1 DescPidClientInfo V_MEMROM2 V_MEMROM3 * pClientInfoTbl, DescPidInstIndex topOfTable)` [정의]
- L6893 `DescPmGetAvailablePidHandle(pid)` [선언]
- L6895 `DescPmGetPidClientHandle(result, pClientInfoTbl, topOfTable)` [선언]
- L6909 `DescPmGetPidPoolHandle(vuint16 pid)` [정의]
- L6929 `ApplDescIsDataIdSupported(pid)` [선언]
- L6939 `DescPmCheckPidSecurityAccess(&(g_descPIDInfo[idx].tinyInfo.checkState))` [선언]
- L6993 `DescRidAnalyseRoutine(DESC_CONTEXT_FORMAL_PARAM_DEF_FIRST V_MEMROM1 DescRidTinyInfo V_MEMROM2 V_MEMROM3 * pRefTinyInfo, DescBool isOnRid)` [정의]
- L7003 `DescCheckState(&(pRefTinyInfo->checkState))` [선언]
- L7077 `DescRoutineControlByIdentifier(DescMsgContext* pMsgContext)` [정의]
- L7094 `DescRidFindRoutineId(DescMake16Bit(pMsgContext->reqData[1], pMsgContext->reqData[2]))` [선언]
- L7105 `DescRidFindSubFunction(pMsgContext->reqData[0], ridInstRef)` [선언]
- L7181 `DescRidProcessingDone(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L7205 `DescPostRoutineControlByIdentifier(DESC_CONTEXT_FORMAL_PARAM_DEF_FIRST vuint8 status)` [정의]
- L7224 `DescRidFindRoutineId(vuint16 rid)` [정의]
- L7255 `DescIterInitPidListProcessor(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L7275 `DescPidProcessingDone(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L7290 `ApplDescGetPidDataLength(pidTypeReadOnce, g_descPidList[DESC_CONTEXT_PARAM_VALUE][g_descPidProcessorState[DESC_CONTEXT_PARAM_VALUE].curPid])` [선언]
- L7294 `DescPmGetPidResponseLen(g_descPidList[DESC_CONTEXT_PARAM_VALUE][g_descPidProcessorState[DESC_CONTEXT_PARAM_VALUE].curPid])` [선언]
- L7427 `DescPidProcessorTask(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L7457 `DescGetHiByte(pidHandle)` [선언]
- L7458 `DescGetLoByte(pidHandle)` [선언]
- L7464 `DescGetHiByte(g_descPIDInfo[pidHandle].reqPid)` [선언]
- L7465 `DescGetLoByte(g_descPIDInfo[pidHandle].reqPid)` [선언]
- L7511 `DescPidDispatcher(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [정의]
- L7550 `ApplDescIsPidSupported(pidTypeReadOnce, pidHandle)` [선언]
- L7553 `DescPmGetAvailablePidHandle(DescMake16Bit(g_descMsgContext[DESC_CONTEXT_PARAM_VALUE].reqData[0], g_descMsgContext[DESC_CONTEXT_PARAM_VALUE].reqData[1]))` [선언]
- L7572 `ApplDescGetPidDataLength(pidTypeReadOnce, pidHandle)` [선언]
- L7575 `DescPmGetPidResponseLen(pidHandle)` [선언]
- L7755 `DescReadDataByIdentifier(DescMsgContext *pMsgContext)` [정의]

### 4. 각 함수별 역할 설명

- `CheckTableConsistency` (L752, 선언): ----------------------------------------------------------------------------- &&&~ Function prototypes -----------------------------------------------------------------------------
- `DescDebugIterInit` (L755, 선언): ----------------------------------------------------------------------------- &&&~ Function prototypes -----------------------------------------------------------------------------
- `DescUsdtNetIsoTpInitPowerOn` (L758, 선언): ----------------------------------------------------------------------------- &&&~ Function prototypes -----------------------------------------------------------------------------
- `DescUsdtNetIsoTpInit` (L759, 선언): ----------------------------------------------------------------------------- &&&~ Function prototypes -----------------------------------------------------------------------------
- `DescUsdtNetIsoTpStateTask` (L761, 선언): ----------------------------------------------------------------------------- &&&~ Function prototypes -----------------------------------------------------------------------------
- `DescUsdtNetIsoTpPrepareResponse` (L763, 선언): ----------------------------------------------------------------------------- &&&~ Function prototypes -----------------------------------------------------------------------------
- `DescUsdtNetIsoTpTransmitResponse` (L764, 선언): ----------------------------------------------------------------------------- &&&~ Function prototypes -----------------------------------------------------------------------------
- `DescUsdtNetIsoTpReleaseInfoPool` (L765, 선언): ----------------------------------------------------------------------------- &&&~ Function prototypes -----------------------------------------------------------------------------
- `DescUsdtNetIsoTpGetRingBuffTxMinLen` (L768, 선언): ----------------------------------------------------------------------------- &&&~ Function prototypes -----------------------------------------------------------------------------
- `DescBusyResponseHandler` (L772, 선언): ----------------------------------------------------------------------------- &&&~ Function prototypes -----------------------------------------------------------------------------
- `DescDispatchServiceContext` (L775, 선언): ----------------------------------------------------------------------------- &&&~ Function prototypes -----------------------------------------------------------------------------
- `DescUsdtNetIsoTpInitContext` (L777, 선언): ----------------------------------------------------------------------------- &&&~ Function prototypes -----------------------------------------------------------------------------
- `DescUsdtNetIsoTpCopyToCan` (L778, 선언): ----------------------------------------------------------------------------- &&&~ Function prototypes -----------------------------------------------------------------------------
- `DescUsdtNetLSFinishReception` (L781, 선언): ----------------------------------------------------------------------------- &&&~ Function prototypes -----------------------------------------------------------------------------
- `DescUsdtNetLSFinishTransmission` (L782, 선언): ----------------------------------------------------------------------------- &&&~ Function prototypes -----------------------------------------------------------------------------
- `DescUsdtNetLSStartReception` (L783, 선언): ----------------------------------------------------------------------------- &&&~ Function prototypes -----------------------------------------------------------------------------
- `DescUsdtNetLSRingBufferCopyRoutine` (L786, 선언): Copy data from ringbuffer into network layer
- `DescUsdtNetAbsFinishReception` (L790, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescUsdtNetAbsFinishTransmission` (L791, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescUsdtNetAbsStartReception` (L792, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescUsdtNetAbsRingBufferCopyRoutine` (L795, 선언): Copy data from ringbuffer into network layer
- `DescNetworkIterInit` (L798, 선언): Copy data from ringbuffer into network layer
- `DescIsTesterPresent` (L803, 선언): Copy data from ringbuffer into network layer
- `DescNetworkInitPowerOn` (L806, 선언): Copy data from ringbuffer into network layer
- `DescNetworkInit` (L807, 선언): Copy data from ringbuffer into network layer
- `DescTransmitRcrRp` (L809, 선언): Copy data from ringbuffer into network layer
- `DescReleaseContext` (L810, 선언): Copy data from ringbuffer into network layer
- `DescTimingOnceInit` (L812, 선언): Copy data from ringbuffer into network layer
- `DescTimingIterInit` (L813, 선언): Copy data from ringbuffer into network layer
- `DescOemNpmTimer` (L817, 선언): Copy data from ringbuffer into network layer
- `DescNpmProcessQueue` (L819, 선언): Copy data from ringbuffer into network layer
- `DescNpmSetSleepInd` (L820, 선언): Copy data from ringbuffer into network layer
- `DescGetSessionStateBitPosition` (L824, 선언): Copy data from ringbuffer into network layer
- `DescOnTransitionStateSession` (L831, 선언): Internal interface of session transition shall be before the generated code which defines the macro to nothing if if it doesn't exist.
- `DescStateOnceInit` (L834, 선언): Internal interface of session transition shall be before the generated code which defines the macro to nothing if if it doesn't exist.
- `DescDoPostProcessing` (L837, 선언): ---- Providing interface between subcomponents Dispatch and Network ----
- `DescDispatcherIterInit` (L839, 선언): 모듈 런타임 상태, 버퍼, 카운터, 하드웨어/생성 파라미터를 초기화한다.
- `DescFindSvcInst` (L841, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescFindSvc` (L843, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescDispatcher` (L845, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescGetSvcInstHeadExtEntrySize` (L848, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescGetSvcInstReqHeadExt` (L850, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescGetSvcInstResHeadExt` (L851, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescGetSvcInstReqHeadExt` (L853, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescGetSvcInstResHeadExt` (L854, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescFinalProcessingDone` (L859, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescContextStateTask` (L862, 선언): 주기적으로 호출되어 타이머, 상태 머신, 송수신 대기 작업을 진행한다.
- `DescResponseTimeoutTask` (L863, 선언): 주기적으로 호출되어 타이머, 상태 머신, 송수신 대기 작업을 진행한다.
- `DescOemStartSessionDefault` (L865, 선언): UDS 진단 세션 상태 조회, 전환 또는 세션별 타이밍 처리를 담당한다.
- `DescOemStartSessionProgramming` (L866, 선언): UDS 진단 세션 상태 조회, 전환 또는 세션별 타이밍 처리를 담당한다.
- `DescOemStartSessionExtended` (L867, 선언): UDS 진단 세션 상태 조회, 전환 또는 세션별 타이밍 처리를 담당한다.
- `DescReadDataByIdentifier` (L868, 선언): UDS DID/PID 데이터 조회, 지원 여부 확인, 응답 데이터 구성에 관여한다.
- `DescOemGetRequestSeed_Unlock_Level_1` (L869, 선언): UDS SecurityAccess의 seed/key 생성, 검증, 시도 횟수와 지연 타이머를 처리한다.
- `DescOemSendSendKey_Unlock_Level_1` (L870, 선언): 송신 요청을 준비하거나 CAN/TP/진단 응답 전송을 시작한다.
- `DescOemCommCtrlEnableRxEnableTx` (L871, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `DescOemCommCtrlEnableRxDisableTx` (L872, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `DescOemCommCtrlDisableRxEnableTx` (L873, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `DescOemCommCtrlDisableRxDisableTx` (L874, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `DescRoutineControlByIdentifier` (L875, 선언): UDS RoutineControl의 RID 조회, 서브기능 분석, 루틴 콜백 실행을 담당한다.
- `DescOemProcessTesterPresent` (L876, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescPostReadDataByIdentifier` (L877, 선언): UDS DID/PID 데이터 조회, 지원 여부 확인, 응답 데이터 구성에 관여한다.
- `DescOemPostCommonCommCtrlProcess` (L878, 선언): 통신 허용/차단, 네트워크 요청/해제 또는 통신 모드 전환을 처리한다.
- `DescDummyPreHandler` (L880, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescDummyPostHandler` (L883, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescOemCommonCommCtrlProcess` (L886, 선언): 통신 허용/차단, 네트워크 요청/해제 또는 통신 모드 전환을 처리한다.
- `DescCommCtrlManipulateCAN` (L887, 선언): 통신 허용/차단, 네트워크 요청/해제 또는 통신 모드 전환을 처리한다.
- `DescOemCheckAndExtractCommTypeParam` (L889, 선언): 통신 허용/차단, 네트워크 요청/해제 또는 통신 모드 전환을 처리한다.
- `DescOemPrepareSessionControl` (L893, 선언): UDS 진단 세션 상태 조회, 전환 또는 세션별 타이밍 처리를 담당한다.
- `DescOemCommonSessionPostProcessing` (L895, 선언): 통신 허용/차단, 네트워크 요청/해제 또는 통신 모드 전환을 처리한다.
- `DescOemOnInvalidReq_27` (L899, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescSecurityOnceInitPowerOn` (L903, 선언): Initialization of the sub-component and timing
- `DescSecurityAccessTimer` (L904, 선언): Initialization of the sub-component and timing
- `DescSecurityOnceInit` (L908, 선언): Initialization of the sub-component
- `DescOemSecuritySeed` (L911, 선언): UDS SecurityAccess의 seed/key 생성, 검증, 시도 횟수와 지연 타이머를 처리한다.
- `DescOemSecurityKey` (L912, 선언): UDS SecurityAccess의 seed/key 생성, 검증, 시도 횟수와 지연 타이머를 처리한다.
- `defined` (L916, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `DescPmGetPidPoolHandle` (L922, 선언): UDS DID/PID 데이터 조회, 지원 여부 확인, 응답 데이터 구성에 관여한다.
- `DescPmGetAvailablePidHandle` (L923, 선언): UDS DID/PID 데이터 조회, 지원 여부 확인, 응답 데이터 구성에 관여한다.
- `DescPmGetSupportedPidClientHandle` (L925, 선언): UDS DID/PID 데이터 조회, 지원 여부 확인, 응답 데이터 구성에 관여한다.
- `DescPmGetPidClientHandle` (L928, 선언): UDS DID/PID 데이터 조회, 지원 여부 확인, 응답 데이터 구성에 관여한다.
- `DescPmAnalysePid` (L930, 선언): UDS DID/PID 데이터 조회, 지원 여부 확인, 응답 데이터 구성에 관여한다.
- `defined` (L932, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `DescRidFindRoutineId` (L937, 선언): UDS RoutineControl의 RID 조회, 서브기능 분석, 루틴 콜백 실행을 담당한다.
- `DescRidFindSubFunction` (L938, 선언): UDS RoutineControl의 RID 조회, 서브기능 분석, 루틴 콜백 실행을 담당한다.
- `DescRidAnalyseRoutine` (L939, 선언): UDS RoutineControl의 RID 조회, 서브기능 분석, 루틴 콜백 실행을 담당한다.
- `DescRidProcessingDone` (L940, 선언): UDS RoutineControl의 RID 조회, 서브기능 분석, 루틴 콜백 실행을 담당한다.
- `DescIterInitPidListProcessor` (L942, 선언): UDS DID/PID 데이터 조회, 지원 여부 확인, 응답 데이터 구성에 관여한다.
- `DescPidProcessorTask` (L944, 선언): 주기적으로 호출되어 타이머, 상태 머신, 송수신 대기 작업을 진행한다.
- `DescPidProcessingDone` (L945, 선언): UDS DID/PID 데이터 조회, 지원 여부 확인, 응답 데이터 구성에 관여한다.
- `DescPidDispatcher` (L946, 선언): UDS DID/PID 데이터 조회, 지원 여부 확인, 응답 데이터 구성에 관여한다.
- `DescNetworkOnceInit` (L1423, 선언): 모듈 런타임 상태, 버퍼, 카운터, 하드웨어/생성 파라미터를 초기화한다.
- `DescStateCompInitOnceGroup` (L1559, 정의): Protocol specific initialization
- `DescSetState` (L1588, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescCheckState` (L1592, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `defined` (L1842, 정의): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `DescDebugIterInit` (L1940, 정의): --------------------------------
- `defined` (L1961, 정의): All necessary preprocessor directives existence checks
- `DescUsdtNetIsoTpInitContext` (L1997, 정의): NAME:              DescUsdtNetIsoTpInitContext CALLED BY:         CANdesc PRECONDITIONS: DESCRIPTION:       Re-initialization
- `TpTxGetConnectionStatus` (L2004, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `DescUsdtNetIsoTpInit` (L2045, 정의): NAME:              DescUsdtNetIsoTpInit CALLED BY:         CANdesc PRECONDITIONS: DESCRIPTION:       Re-initialization
- `DescUsdtNetIsoTpStateTask` (L2095, 정의): NAME:              DescUsdtNetIsoTpStateTask CALLED BY:         Transport layer PRECONDITIONS: DESCRIPTION:       State Task of the IsoTp driver
- `DescCheckTA` (L2105, 정의): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescDispatchServiceContext` (L2144, 정의): NAME:              DescDispatchServiceContext CALLED BY:         Desc start reception event PRECONDITIONS: DESCRIPTION:       this function dispatches the parallel request reception target context
- `DescGetBuffer` (L2166, 정의): NAME:              DescGetBuffer CALLED BY:         Transport layer PRECONDITIONS: DESCRIPTION:       StartOfFrame reception function
- `DescOemCheckPhysTargetAddress` (L2174, 선언): NAME:              DescGetBuffer CALLED BY:         Transport layer PRECONDITIONS: DESCRIPTION:       StartOfFrame reception function
- `DescOemCheckPhysSourceAddress` (L2179, 선언): NAME:              DescGetBuffer CALLED BY:         Transport layer PRECONDITIONS: DESCRIPTION:       StartOfFrame reception function
- `DescDispatchServiceContext` (L2188, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `defined` (L2222, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `TpRxGetCanChannel` (L2233, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxGetCanChannel` (L2235, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxGetTaType` (L2251, 선언): It has to be actviated for Extended Addressing
- `DescUsdtNetStartReception` (L2279, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescPhysReqInd` (L2334, 정의): NAME:              DescPhysReqInd CALLED BY:         Transport layer PRECONDITIONS: DESCRIPTION:       Rx path for physical requests. Function name must be entered to the CANgen OSEK-TP Options dialog in the fields "RxIndication".
- `TpRxGetChannelID` (L2356, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxGetEcuNumber` (L2371, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxGetBaseAddress` (L2381, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `DescRxErrorIndication` (L2446, 정의): NAME:              DescRxErrorIndication CALLED BY:         Transport layer PRECONDITIONS: DESCRIPTION:       Release the lock mechanism for Rx on error by receiving. Function name must be entered to the CANgen TP setup dialog in the fields "TpXXXErrorIndicati
- `DescUsdtNetIsoTpPrepareResponse` (L2492, 정의): NAME:              DescUsdtNetIsoTpPrepareResponse CALLED BY:         CANdesc (Dispatcher) PRECONDITIONS: DESCRIPTION:       Prepare address info for response message
- `DescUsdtNetIsoTpTransmitResponse` (L2521, 정의): NAME:              DescTransmitResponse CALLED BY:         CANdesc (Dispatcher) PRECONDITIONS: DESCRIPTION:       Response transmission function
- `defined` (L2578, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `defined` (L2588, 선언): use the abstract info for spontaneus responses
- `DanisIsoTpTransmit` (L2617, 선언): 송신 요청을 준비하거나 CAN/TP/진단 응답 전송을 시작한다.
- `TpTransmitDiag` (L2622, 선언): 송신 요청을 준비하거나 CAN/TP/진단 응답 전송을 시작한다.
- `DanisIsoTpTransmit` (L2625, 선언): 송신 요청을 준비하거나 CAN/TP/진단 응답 전송을 시작한다.
- `DescUsdtNetIsoTpGetRingBuffTxMinLen` (L2644, 정의): NAME:              DescUsdtNetIsoTpGetRingBuffTxMinLen CALLED BY:         CANdesc (Dispatcher) PRECONDITIONS: DESCRIPTION:       Get Minimum Length
- `DescUsdtNetIsoTpCopyToCan` (L2659, 정의): NAME:              DescUsdtNetIsoTpCopyToCan CALLED BY:         DescCopyToCAN PRECONDITIONS: DESCRIPTION:       Provides abstract copy functionality between CANdriver and DANIS.
- `DescCopyToCAN` (L2678, 정의): NAME:              DescCopyToCAN CALLED BY:         Transport layer PRECONDITIONS: DESCRIPTION:       Callback for filling the data into the Tx buffer. Function name must be entered to the CANgen TP setup dialog in the fields "TpCopyToCan".
- `DescUsdtNetRingBufferCopyRoutine` (L2702, 선언): NAME:              DescCopyToCAN CALLED BY:         Transport layer PRECONDITIONS: DESCRIPTION:       Callback for filling the data into the Tx buffer. Function name must be entered to the CANgen TP setup dialog in the fields "TpCopyToCan".
- `DescUsdtNetRingBufferCopyRoutine` (L2705, 선언): NAME:              DescCopyToCAN CALLED BY:         Transport layer PRECONDITIONS: DESCRIPTION:       Callback for filling the data into the Tx buffer. Function name must be entered to the CANgen TP setup dialog in the fields "TpCopyToCan".
- `DescConfirmation` (L2753, 정의): NAME:              DescConfirmation CALLED BY:         Transport layer PRECONDITIONS: DESCRIPTION:       Provides the post transmission processing. Function name must be entered to the CANgen TP setup dialog in the fields "TpConfirmation".
- `DescTxErrorIndication` (L2806, 정의): NAME:              DescTxErrorIndication CALLED BY:         Transport layer PRECONDITIONS: DESCRIPTION:       Release the lock mechanism for Tx on error by sending. Function name must be entered to the CANgen TP setup dialog in the fields "TpXXXErrorIndication
- `DescUsdtNetIsoTpReleaseInfoPool` (L2852, 정의): NAME:              DescUsdtNetIsoTpReleaseInfoPool CALLED BY:         CANdesc (Dispatcher) PRECONDITIONS: DESCRIPTION:       Release of infopool
- `DescBusyResponseHandler` (L2869, 정의): NAME:              DescBusyResponseHandler CALLED BY:         DescTask PRECONDITIONS: DESCRIPTION:       Busy response handler for additional received requests
- `TpTxGetFreeChannel` (L2883, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `DescGetFuncBuffer` (L2912, 정의): If we can not send the data, we have to terminate this channel and try it at next cycle
- `DescOemCheckFuncTargetAddress` (L2930, 선언): NAME:              DescGetFuncBuffer CALLED BY:         Transport layer PRECONDITIONS: DESCRIPTION:       StartOfFrame reception function
- `DescOemCheckFuncSourceAddress` (L2935, 선언): NAME:              DescGetFuncBuffer CALLED BY:         Transport layer PRECONDITIONS: DESCRIPTION:       StartOfFrame reception function
- `DescDispatchServiceContext` (L2944, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `TpFuncGetCanChannel` (L2952, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `defined` (L2957, 선언): Make the communication channel ID visible to the above layer
- `DescUsdtNetStartReception` (L2975, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescFuncReqInd` (L3004, 정의): NAME:              DescFuncReqInd CALLED BY:         Transport layer PRECONDITIONS: DESCRIPTION:       Rx path for physical requests. Function name must be entered to the CANgen OSEK-TP Options dialog in the fields "RxIndication".
- `TpFuncGetReceiveCanID` (L3027, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpFuncGetTargetAddress` (L3033, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `TpFuncGetBaseAddress` (L3044, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `DescNetworkOnceInit` (L3065, 정의): NAME:              DescNetworkOnceInit CALLED BY:         DescInit PRECONDITIONS: DESCRIPTION:       Initilizes the network subcomponent specific data.
- `DescNetworkIterInit` (L3107, 정의): NAME:              DescNetworkIterInit CALLED BY:         DescInit PRECONDITIONS: DESCRIPTION:       Initilizes the network subcomponent context specific data.
- `DescReleaseContext` (L3140, 정의): NAME:              DescReleaseContext CALLED BY:         CANdesc PRECONDITIONS: DESCRIPTION:       releases the RX path and manages the S1 timer
- `DescSendSpontaneousResponse` (L3169, 정의): NAME:              DescSendSpontaneousResponse CALLED BY:         Desc internal PRECONDITIONS: DESCRIPTION:       Any time can make the application a single frame response.
- `DescIsTesterPresent` (L3211, 정의): NAME:              DescIsTesterPresent CALLED BY:         Desc PRECONDITIONS: DESCRIPTION:
- `DescNetworkInitPowerOn` (L3255, 정의): NAME:              DescNetworkInitPowerOn CALLED BY:         Application PRECONDITIONS: DESCRIPTION:       Reports the current state of the CANdesc component
- `DescNetworkInit` (L3271, 정의): NAME:              DescNetworkInit CALLED BY:         Application PRECONDITIONS: DESCRIPTION:       Reports the current state of the CANdesc component
- `DescGetActivityState` (L3286, 정의): NAME:              DescGetActivityState CALLED BY:         Application PRECONDITIONS: DESCRIPTION:       Reports the current state of the CANdesc component
- `DescICNRxMsgInd` (L3302, 정의): NAME:              DescICNRxMsgInd CALLED BY:         Desc Network Layer PRECONDITIONS: DESCRIPTION:
- `DescUsdtNetAbsStartReception` (L3339, 정의): NAME:              DescUsdtNetAbsStartReception CALLED BY:         Desc Network Layer PRECONDITIONS: DESCRIPTION:       StartOfFrame reception function
- `DescUsdtNetAbsFinishReception` (L3393, 정의): NAME:              DescUsdtNetAbsFinishReception CALLED BY:         Desc Network Layer PRECONDITIONS: DESCRIPTION:       reception has finished
- `DescUsdtNetAbsFinishTransmission` (L3434, 정의): NAME:              DescUsdtNetAbsFinishTransmission CALLED BY:         Desc Network Layer PRECONDITIONS: DESCRIPTION:       transmission has finished
- `DescTransmitRcrRp` (L3545, 정의): NAME:              DescTransmitRcrRp CALLED BY:         Desc PRECONDITIONS: DESCRIPTION:       Transmission of RCR-RP
- `DescUsdtNetGetTransmissionPtr` (L3553, 선언): The response message is already filled with the necessary information
- `DescGetTesterAddress` (L3581, 정의): NAME:              DescGetTesterAddress CALLED BY:         Application PRECONDITIONS: DESCRIPTION:       Returns the tester address of the last request.
- `DescGetCurrentBusInfo` (L3595, 정의): NAME:              DescGetCurrentBusInfo CALLED BY:         Application PRECONDITIONS: DESCRIPTION:       Returns the current communication parameters.
- `DescTimingIterInit` (L3609, 정의): NAME:              DescTimingIterInit CALLED BY:         DescInit PRECONDITIONS: DESCRIPTION:       Initilizes the timing subcomponent context data.
- `DescForceRcrRpResponse` (L3625, 정의): NAME:              DescForceRcrRpResponse CALLED BY:         Application PRECONDITIONS: DESCRIPTION:       Forces the CANdesc to send immediately a RCR-RP response.
- `DescTimingOnceInit` (L3647, 정의): NAME:              DescTimingOnceInit CALLED BY:         DescInit PRECONDITIONS: DESCRIPTION:       Initilizes the network subcomponent common data.
- `DescNpmSetSleepInd` (L3670, 정의): NAME:              DescNpmSetSleepInd CALLED BY:         DescTask PRECONDITIONS: DESCRIPTION:       Timer which will be running only when DESC not idle mode
- `DescNpmProcessQueue` (L3704, 정의): NAME:              DescNpmProcessQueue CALLED BY:         DescTask PRECONDITIONS: DESCRIPTION:       Timer which will be running only when DESC not idle mode
- `DescGetMirroredQueue` (L3737, 선언): Merge the queued channel to the activity map
- `DescOemNpmTimer` (L3760, 정의): NAME:              DescOemNpmTimer CALLED BY:         DescTask PRECONDITIONS: DESCRIPTION:       Timer which will be running only in DESC idle mode and default session active
- `DescStateOnceInit` (L3788, 정의): NAME:              DescStateOnceInit CALLED BY:         DescInit PRECONDITIONS: DESCRIPTION:       Initilizes the state subcomponent common data.
- `DescStateInit` (L3793, 정의): NAME:              DescStateOnceInit CALLED BY:         DescInit PRECONDITIONS: DESCRIPTION:       Initilizes the state subcomponent common data.
- `DescGetStateSession` (L3800, 정의): NAME:              DescStateOnceInit CALLED BY:         DescInit PRECONDITIONS: DESCRIPTION:       Initilizes the state subcomponent common data.
- `DescSetStateSession` (L3806, 정의): NAME:              DescStateOnceInit CALLED BY:         DescInit PRECONDITIONS: DESCRIPTION:       Initilizes the state subcomponent common data.
- `DescGetStateSecurityAccess` (L3816, 정의): The new state shall not be zero
- `DescSetStateSecurityAccess` (L3822, 정의): The new state shall not be zero
- `DescCheckState` (L3832, 정의): The new state shall not be zero
- `DescSetState` (L3846, 정의): The new state shall not be zero
- `DescGetSessionStateOfSessionId` (L3876, 정의): NAME:              DescGetSessionStateOfSessionId CALLED BY:         Application PRECONDITIONS: DESCRIPTION:       Converts Session ID to its state.
- `DescGetSvcInstHeadExtEntrySize` (L3883, 선언): NAME:              DescGetSessionStateOfSessionId CALLED BY:         Application PRECONDITIONS: DESCRIPTION:       Converts Session ID to its state.
- `DescGetSvcInstReqHeadExt` (L3886, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescGetSessionStateBitPosition` (L3929, 정의): NAME:              DescGetSessionStateBitPosition CALLED BY:         SetState PRECONDITIONS: DESCRIPTION:       Gathers the set bit number (0,1,...).
- `DescGetSessionIdOfSessionState` (L3958, 정의): NAME:              DescGetSessionIdOfSessionState CALLED BY:         Application PRECONDITIONS: DESCRIPTION:       Converts given session state into its corresponding sub-function Id.
- `DescGetSessionTimings` (L3992, 정의): NAME:              DescGetSessionTimings CALLED BY:         Application PRECONDITIONS: DESCRIPTION:       Returns the session specific P2/P2* timings
- `DescGetSessionStateBitPosition` (L3999, 선언): NAME:              DescGetSessionTimings CALLED BY:         Application PRECONDITIONS: DESCRIPTION:       Returns the session specific P2/P2* timings
- `DescOnTransitionStateSession` (L4024, 정의): NAME:              DescOnTransitionStateSession CALLED BY:         SetState PRECONDITIONS: DESCRIPTION:       Special processing for the state group session.
- `DescStartRepeatedServiceCall` (L4083, 정의): NAME:              DescStartRepeatedServiceCall CALLED BY:         Application PRECONDITIONS: DESCRIPTION:       Enables the multicall feature for the current service.
- `DescSetNegResponse` (L4099, 정의): NAME:              DescSetNegResponse CALLED BY:         any application function PRECONDITIONS: DESCRIPTION:       Sets the error.
- `DescGetSvcInstHeadExtEntrySize` (L4119, 정의): NAME:              DescGetSvcInstHeadExtEntrySize CALLED BY:         Dispatcher PRECONDITIONS: DESCRIPTION:       Binary search for service instance.
- `DescGetSvcInstHeadExtEntrySize` (L4167, 선언): NAME:              DescGetSvcInstReqHeadExt CALLED BY:         Dispatcher PRECONDITIONS: DESCRIPTION:       Binary search for service instance.
- `DescFindSvcInst` (L4185, 정의): NAME:              DescFindSvcInst CALLED BY:         Dispatcher PRECONDITIONS: DESCRIPTION:       Binary search for service instance.
- `DescGetSvcInstReqHeadExt` (L4210, 선언): ------------------------------------------
- `V_BOOL_EXPR` (L4215, 선언): ------------------------------------------
- `DescFindSvcInst` (L4278, 정의): NAME:              DescFindSvcInst CALLED BY:         Dispatcher PRECONDITIONS: DESCRIPTION:       Linear search for service instance.
- `DescGetSvcInstHeadExtEntrySize` (L4288, 선언): NAME:              DescFindSvcInst CALLED BY:         Dispatcher PRECONDITIONS: DESCRIPTION:       Linear search for service instance.
- `V_BOOL_EXPR` (L4303, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `DescFindSvc` (L4358, 정의): NAME:              DescFindSvc CALLED BY:         Dispatcher,FuncReception PRECONDITIONS: DESCRIPTION:       Search a service ID.
- `DescGetServiceId` (L4414, 정의): NAME:              DescGetServiceId CALLED BY:         Application user service PRECONDITIONS: DESCRIPTION:       Returns the current request's SId.
- `DescDispatcherIterInit` (L4444, 정의): NAME:              DescDispatcherIterInit CALLED BY:         DescInit PRECONDITIONS: DESCRIPTION:       Initilizes the dispatcher subcomponent context specific data.
- `DescDispatcher` (L4469, 정의): NAME:              DescDispatcher CALLED BY:         DescTask PRECONDITIONS:     If there is at least one received request. DESCRIPTION:       Provides the kernel of the diagnostic
- `DescFindSvc` (L4494, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `CANBITTYPE_CAST` (L4546, 선언): Extract the information to the application
- `DescExtractReqExtHeadLen` (L4632, 선언): Prepare the application information
- `DescExtractResExtHeadLen` (L4634, 선언): This is the extension of the response header (the header without the resposne SID), so for the complete length - add 1
- `DescProcessingDone` (L4784, 정의): NAME:              DescProcessingDone CALLED BY:         Dispatch,  any application function PRECONDITIONS: DESCRIPTION:       Depending on the current situation, provides the right action.
- `DescPidProcessingDone` (L4795, 선언): Finalize the request processing
- `DescExtractResExtHeadLen` (L4946, 선언): This is the extension of the response header (the header without the resposne SID), so for the complete length - add 1
- `DescGetSvcInstResHeadExt` (L4948, 선언): This is the extension of the response header (the header without the resposne SID), so for the complete length - add 1
- `DescDoPostProcessing` (L5010, 정의): NAME:              DescDoPostProcessing CALLED BY:         DescConfirmation, DescProcessingDone PRECONDITIONS: DESCRIPTION:       Provides finalizing actions In case of secured Transmission
- `DescInitPowerOn` (L5041, 정의): NAME:              DescInitPowerOn CALLED BY:         Application PRECONDITIONS:     On system boot. DESCRIPTION:       Performs time consuming init/checks of the diag module.
- `DescInit` (L5079, 정의): NAME:              DescInit CALLED BY:         any application function, DescInitPowerOn PRECONDITIONS: DESCRIPTION:       Performs init of the diag module.
- `DescTimerTask` (L5150, 정의): NAME:              DescTimerTask CALLED BY:         DescTask PRECONDITIONS: DESCRIPTION:       Performs cyclic tasking for diagnostic purpouse.
- `DescResponseTimeoutTask` (L5246, 정의): NAME:              DescResponseTimeoutTask CALLED BY:         DescTask PRECONDITIONS: DESCRIPTION:       Performs cyclic tasking for diagnostic purpouse.
- `DescTask` (L5426, 정의): NAME:              DescTask CALLED BY:         any application function PRECONDITIONS: DESCRIPTION:       Performs cyclic tasking for diagnostic purpouse.
- `DescStateTask` (L5446, 정의): NAME:              DescStateTask CALLED BY:         any application function PRECONDITIONS: DESCRIPTION:       Performs cyclic tasking for diagnostic purpouse.
- `DescContextStateTask` (L5504, 정의): NAME:              DescContextStateTask CALLED BY:         any application function PRECONDITIONS: DESCRIPTION:       Performs cyclic tasking for diagnostic purpouse.
- `DescUsdtNetGetTransmissionPtr` (L5697, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescOemStartSessionDefault` (L5722, 정의): Function name:DescOemStartSessionDefault Description:Processes the session change request, parametrizing the common processing function. Returns:  nothing Parameter(s): - pMsgContext: - Contains all request properties. - Access type: read/write Particularitie(s) and limitation(s): none *******************************************************************************
- `DescOemStartSessionProgramming` (L5739, 정의): Function name:DescOemStartSessionProgramming Description:Processes the session change request, parametrizing the common processing function. Returns:  nothing Parameter(s): - pMsgContext: - Contains all request properties. - Access type: read/write Particularitie(s) and limitation(s): none *******************************************************************************
- `DescOemStartSessionExtended` (L5756, 정의): Function name:DescOemStartSessionExtended Description:Processes the session change request, parametrizing the common processing function. Returns:  nothing Parameter(s): - pMsgContext: - Contains all request properties. - Access type: read/write Particularitie(s) and limitation(s): none *******************************************************************************
- `DescOemGetRequestSeed_Unlock_Level_1` (L5772, 정의): Function name:DescOemGetRequestSeed_Unlock_Level_1 Description:Manages the security get seed function. Returns:  nothing Parameter(s): - pMsgContext: - Contains all request properties. - Access type: read/write Particularitie(s) and limitation(s): none *******************************************************************************
- `DescOemSendSendKey_Unlock_Level_1` (L5788, 정의): Function name:DescOemSendSendKey_Unlock_Level_1 Description:Manages the security send key function. Returns:  nothing Parameter(s): - pMsgContext: - Contains all request properties. - Access type: read/write Particularitie(s) and limitation(s): none *******************************************************************************
- `DescOemCommCtrlEnableRxEnableTx` (L5804, 정의): Function name:DescOemCommCtrlEnableRxEnableTx Description: not available Returns:  nothing Parameter(s): - pMsgContext: - Contains all request properties. - Access type: read/write Particularitie(s) and limitation(s): none *******************************************************************************
- `DescOemCommCtrlEnableRxDisableTx` (L5820, 정의): Function name:DescOemCommCtrlEnableRxDisableTx Description: not available Returns:  nothing Parameter(s): - pMsgContext: - Contains all request properties. - Access type: read/write Particularitie(s) and limitation(s): none *******************************************************************************
- `DescOemCommCtrlDisableRxEnableTx` (L5836, 정의): Function name:DescOemCommCtrlDisableRxEnableTx Description: not available Returns:  nothing Parameter(s): - pMsgContext: - Contains all request properties. - Access type: read/write Particularitie(s) and limitation(s): none *******************************************************************************
- `DescOemCommCtrlDisableRxDisableTx` (L5852, 정의): Function name:DescOemCommCtrlDisableRxDisableTx Description: not available Returns:  nothing Parameter(s): - pMsgContext: - Contains all request properties. - Access type: read/write Particularitie(s) and limitation(s): none *******************************************************************************
- `DescDummyPreHandler` (L5868, 정의): NAME:              DescDummyPreHandler CALLED BY:         DESCDispatcher PRECONDITIONS: DESCRIPTION:       Used instead of V_NULL pointer
- `DescDummyPostHandler` (L5885, 정의): NAME:              DescDummyPostHandler CALLED BY:         DESCDispatcher PRECONDITIONS: DESCRIPTION:       Used instead of V_NULL pointer
- `DescIsSuppressPosResBitSet` (L5902, 정의): NAME:              DescSessionGetSuppressPosBit CALLED BY:         Application PRECONDITIONS: DESCRIPTION:       Will return the current value of the SPRMIB.
- `DescOemProcessTesterPresent` (L5919, 정의): NAME:              DescOemProcessTesterPresent CALLED BY:         DescDispatcher PRECONDITIONS: DESCRIPTION:
- `DescOemCheckAndExtractCommTypeParam` (L5938, 정의): NAME:              DescOemCheckAndExtractCommTypeParam CALLED BY:         Default/CommunicationControl is activated PRECONDITIONS: DESCRIPTION:       Checks if the msg type is APPL or NM.
- `DescOemCommonCommCtrlProcess` (L5987, 정의): NAME:              DescOemCommonCommCtrlProcess CALLED BY:         DescOemCommCtrl_XXXableRxAndXXXableTx PRECONDITIONS: DESCRIPTION:       Central communciation control processing.
- `DescOemCheckAndExtractCommTypeParam` (L6007, 선언): 통신 허용/차단, 네트워크 요청/해제 또는 통신 모드 전환을 처리한다.
- `DescOemPostCommonCommCtrlProcess` (L6095, 정의): NAME:              DescOemPostCommonCommCtrlProcess CALLED BY:         Dispatcher PRECONDITIONS: DESCRIPTION:       Processes the request after the positive response was sent.
- `channel` (L6118, 선언): Manipulate application channels
- `DescCommCtrlManipulateCAN` (L6161, 정의): NAME:              DescCommCtrlManipulateCAN CALLED BY:         DescOemPostCommonCommCtrlProcess PRECONDITIONS: DESCRIPTION:       Processes requested action.
- `DescOemPrepareSessionControl` (L6191, 정의): NAME:              DescOemPrepareSessionControl CALLED BY:         CANdesc PRECONDITIONS: DESCRIPTION:       Prepares the response for DiagnosticSessionControl.
- `DescGetHiByte` (L6214, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescGetLoByte` (L6215, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescGetHiByte` (L6217, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescGetLoByte` (L6218, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescOemCommonSessionPostProcessing` (L6241, 정의): NAME:              DescOemCommonSessionPostProcessing CALLED BY:         Dispatcher PRECONDITIONS: DESCRIPTION:       Common DiagnosticSessionControl session post handler. Workaround for session self-transitioning.
- `ok` (L6249, 정의): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `DescSessionTransitionChecked` (L6282, 정의): NAME:              DescSessionTransitionChecked CALLED BY:         Application PRECONDITIONS: DESCRIPTION:       Session transition check finished.
- `defined` (L6291, 정의): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `DescSecurityOnceInitPowerOn` (L6324, 정의): Function name:ApplDescSecurityAccessInitPowerOn Description: Shall be called at ECU power on event. Returns:  - Parameter(s):none Particularitie(s) and limitation(s): *******************************************************************************
- `DescGetTicksOfMs` (L6332, 선언): If necessary at each ECU reset - set the penality timer always
- `DescSecurityAccessTimer` (L6347, 정의): Function name:ApplDescSecurityAccessTimer Description: Shall be called periodically by the application. Returns:  - Parameter(s):none Particularitie(s) and limitation(s): *******************************************************************************
- `DescGetAttemptCounterValue` (L6380, 정의): Function name:DescGetAttemptCounterValue Description: Reports the current value of the attempt counter Returns:  The current value of the attempt counter Parameter(s): iContext in multicontext system Particularitie(s) and limitation(s): *******************************************************************************
- `DescSetAttemptCounterValue` (L6396, 정의): Function name:DescSetAttemptCounterValue Description: Sets a new value of the attempt counter Returns:  - Parameter(s): iContext in multicontext system Particularitie(s) and limitation(s): *******************************************************************************
- `DescGetTicksOfMs` (L6411, 선언): Activate brute-force inhibition
- `DescStartSecureTimer` (L6424, 정의): Function name:DescStartSecureTimer Description: Activate brute-force inhibition Returns:  - Parameter(s): - Particularitie(s) and limitation(s): *******************************************************************************
- `DescGetTicksOfMs` (L6428, 선언): Function name:DescStartSecureTimer Description: Activate brute-force inhibition Returns:  - Parameter(s): - Particularitie(s) and limitation(s): *******************************************************************************
- `DescSecurityOnceInit` (L6441, 정의): NAME:              DescSecurityOnceInit CALLED BY:         DescInit PRECONDITIONS: DESCRIPTION:       Initilizes the network subcomponent common data.
- `defined` (L6449, 정의): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `DescOemSecuritySeed` (L6484, 정의): Name         :  DescOemSecuritySeed Called by    :  Dispatcher Preconditions:  None Parameters   :  Read in global Diag buffer Description  :  This function is called after seed bytes have been requested This is provided as example, has to be implemented by th
- `DescGetSeedLength` (L6488, 선언): UDS SecurityAccess의 seed/key 생성, 검증, 시도 횟수와 지연 타이머를 처리한다.
- `DescGetTicksOfMs` (L6534, 선언): Activate brute-force inhibition
- `DescSecurityAccessSeedReady` (L6591, 정의): Name         :  DescSecurityAccessSeedReady Called by    :  Application Preconditions:  None Parameters   :  iContext Description  :  This function is called after seed bytes have been written
- `DescOemSecurityKey` (L6610, 정의): Name         :  DescOemSecurityKey Called by    :  Dispatcher Preconditions:  None Parameters   :  Read in global Diag buffer Description  :  This function is called after key bytes have been received This is provided as example, has to be implemented by the u
- `DescSecurityAccessKeyChecked` (L6663, 정의): Name         :  DescSecurityAccessKeyChecked Called by    :  Application Preconditions:  None Parameters   :  iContext Description  :  This function is called after key bytes have been validates
- `DescGetTicksOfMs` (L6695, 선언): Activate brute-force inhibition
- `defined` (L6735, 정의): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `DescPmClientGetResLength` (L6750, 선언): NAME:              DescPmGetPidResponseLen CALLED BY: PRECONDITIONS: DESCRIPTION:
- `DescPmAnalysePid` (L6771, 정의): NAME:              DescAnalysePid CALLED BY: PRECONDITIONS: DESCRIPTION:
- `DescCheckState` (L6782, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescPmGetPidClientHandle` (L6834, 정의): NAME:              DescPmGetPidClientHandle CALLED BY: PRECONDITIONS: DESCRIPTION:
- `DescPmGetAvailablePidHandle` (L6870, 정의): NAME:              DescPmGetAvailablePidHandle CALLED BY: PRECONDITIONS: DESCRIPTION:
- `DescPmGetPidPoolHandle` (L6874, 선언): NAME:              DescPmGetAvailablePidHandle CALLED BY: PRECONDITIONS: DESCRIPTION:
- `DescPmClientCheckPid` (L6875, 선언): NAME:              DescPmGetAvailablePidHandle CALLED BY: PRECONDITIONS: DESCRIPTION:
- `DescPmGetSupportedPidClientHandle` (L6890, 정의): NAME:              DescPmGetSupportedPidClientHandle CALLED BY: PRECONDITIONS: DESCRIPTION:    Returns a reference of the DID in the client specific info table
- `DescPmGetAvailablePidHandle` (L6893, 선언): NAME:              DescPmGetSupportedPidClientHandle CALLED BY: PRECONDITIONS: DESCRIPTION:    Returns a reference of the DID in the client specific info table
- `DescPmGetPidClientHandle` (L6895, 선언): No check needed for invalidity since all the functions are protected by such checks
- `DescPmGetPidPoolHandle` (L6909, 정의): NAME:              DescPmGetPidPoolHandle CALLED BY: PRECONDITIONS: DESCRIPTION:
- `ApplDescIsDataIdSupported` (L6929, 선언): CANdesc가 호출하는 애플리케이션 진단 훅으로 해당 UDS 서비스 처리 결과를 구성한다.
- `DescPmCheckPidSecurityAccess` (L6939, 선언): UDS SecurityAccess의 seed/key 생성, 검증, 시도 횟수와 지연 타이머를 처리한다.
- `DescRidAnalyseRoutine` (L6993, 정의): NAME:              DescRidAnalyseRoutine CALLED BY: PRECONDITIONS: DESCRIPTION:
- `DescCheckState` (L7003, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescRoutineControlByIdentifier` (L7077, 정의): NAME:              DescRoutineControlByIdentifier CALLED BY:         DescDispatcher PRECONDITIONS: DESCRIPTION:       Main handler for RID handling.
- `DescRidFindRoutineId` (L7094, 선언): Check minimum length (SubFunction and RID must be inside)
- `DescRidFindSubFunction` (L7105, 선언): Check if sub-function valid (at all and for RID)
- `DescRidProcessingDone` (L7181, 정의): NAME:              DescRidProcessingDone CALLED BY:         Application PRECONDITIONS: DESCRIPTION:       Subcomponent processing done for a RID iContext for the main response is also the same for the RID
- `DescPostRoutineControlByIdentifier` (L7205, 정의): NAME:              DescPostRoutineControlByIdentifier CALLED BY:         DescTask PRECONDITIONS: DESCRIPTION:       Post handler for RID handling.
- `DescRidFindRoutineId` (L7224, 정의): NAME:              DescRidFindRoutineId CALLED BY:         Rid Processor PRECONDITIONS: DESCRIPTION:       Linear look-up for RID (there are not so many RIDs expected).
- `DescIterInitPidListProcessor` (L7255, 정의): NAME:              DescIterInitPidListProcessor CALLED BY:         CANdesc PRECONDITIONS: DESCRIPTION:       Subcomponent initialization
- `DescPidProcessingDone` (L7275, 정의): NAME:              DescPidProcessingDone CALLED BY:         Application PRECONDITIONS: DESCRIPTION:       Subcomponent processing done for a single PID iContext for the main response is also the same for the DID
- `ApplDescGetPidDataLength` (L7290, 선언): Check for dynamic response length and multiple PID request
- `DescPmGetPidResponseLen` (L7294, 선언): UDS DID/PID 데이터 조회, 지원 여부 확인, 응답 데이터 구성에 관여한다.
- `DescPidProcessorTask` (L7427, 정의): NAME:              DescPidProcessorTask CALLED BY:         DescTask PRECONDITIONS: DESCRIPTION:       Subcomponent task functionality. Polling mode.
- `DescGetHiByte` (L7457, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescGetLoByte` (L7458, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescGetHiByte` (L7464, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescGetLoByte` (L7465, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescPidDispatcher` (L7511, 정의): NAME:              DescPidDispatcher CALLED BY:         PidTask PRECONDITIONS: DESCRIPTION:       Processes a single PID with dispatcher functionality.
- `ApplDescIsPidSupported` (L7550, 선언): Process all PIDs until an error has been detected
- `DescPmGetAvailablePidHandle` (L7553, 선언): Find PID (use main pool since each read-able PID is accessible by $22)
- `ApplDescGetPidDataLength` (L7572, 선언): Check for dynamic response length and multiple PID request
- `DescPmGetPidResponseLen` (L7575, 선언): Check for dynamic response length and multiple PID request
- `DescReadDataByIdentifier` (L7755, 정의): NAME:              DescReadDataByIdentifier CALLED BY:         DescTask PRECONDITIONS: DESCRIPTION:       Main handler for PID list handling.

### 5. 필요시 코드 부연 설명

대표 함수 원형 수준의 코드 맥락이다. 전체 구현은 원본 파일이 크므로 함수명/역할 중심으로 분석했다.

```c
// src/CBD/Gen/desc.c:752
static void CheckTableConsistency(void)
```
```c
// src/CBD/Gen/desc.c:755
static void DescDebugIterInit(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)
```
```c
// src/CBD/Gen/desc.c:758
DESCNET_USDT_STATIC void DescUsdtNetIsoTpInitPowerOn(void)
```
```c
// src/CBD/Gen/desc.c:759
DESCNET_USDT_STATIC void DescUsdtNetIsoTpInit(void)
```
```c
// src/CBD/Gen/desc.c:761
DESCNET_USDT_STATIC void DescUsdtNetIsoTpStateTask(void)
```
```c
// src/CBD/Gen/desc.c:763
DESCNET_USDT_STATIC void DescUsdtNetIsoTpPrepareResponse(t_descUsdtNetInfoPoolPtr infoPool)
```
```c
// src/CBD/Gen/desc.c:764
DESCNET_USDT_STATIC void DescUsdtNetIsoTpTransmitResponse(t_descUsdtNetInfoPoolPtr infoPool)
```
```c
// src/CBD/Gen/desc.c:765
DESCNET_USDT_STATIC void DescUsdtNetIsoTpReleaseInfoPool(t_descUsdtNetInfoPoolPtr infoPool)
```

## 파일이름 : desc

- 경로: `src/CBD/Gen/desc.h`
- 파일 종류: `.h`
- 파일 크기: 70,872 bytes
- 파일 내용: CANdesc 타입, NRC, 서비스 API, 진단 메시지 컨텍스트를 정의한다.

### 1. .c/.h 파일 이름

- `desc.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 481
- typedef/struct/enum 후보 수: 28
- 전역 변수/테이블 선언 후보 수: 26

#### 주요 매크로
- L28 `__DESC_H__`
- L135 `V_ENABLE_USE_DUMMY_STATEMENT`
- L140 `DESC_ENABLE_GENTOOL_GENY`
- L142 `DESC_DISABLE_FLASHABLE_ECU`
- L144 `DESC_DISABLE_VECTOR_FBL_USAGE`
- L146 `DESC_DISABLE_FAR_BUFFER`
- L148 `DESC_DISABLE_PROTOCOL_KWP`
- L150 `DESC_ENABLE_PROTOCOL_UDS`
- L152 `DESC_ENABLE_DEBUG_USER`
- L154 `DESC_ENABLE_DEBUG_INTERNAL`
- L160 `DESC_ENABLE_ACCESS_TESTER_ADDRESS_API`
- L165 `DESC_USDTNET_DISABLE_MULTI_TP`
- L166 `DESC_USDTNET_ENABLE_SINGLE_TP_OPTIMIZED`
- L168 `DESC_USDTNET_ENABLE_MULTI_TP`
- L169 `DESC_USDTNET_DISABLE_SINGLE_TP_OPTIMIZED`
- L173 `DESC_USDTNET_DISABLE_LONGSERVICE_SUPPORT`
- L175 `DESC_USDTNET_DISABLE_RING_BUFFER`
- L177 `DESC_USDTNET_DISABLE_FAR_BUFFER`
- L179 `DESC_USDTNET_DISABLE_MULTI_OWNER_SHARED_BUFFER`
- L181 `DESC_USDTNET_DISABLE_DYNAMIC_BUFFER_LENGTH`
- L183 `DESC_USDTNET_ENABLE_VECTOR_ISO_TP`
- L186 `DESC_DISABLE_UUDT_NET`
- L188 `DESC_DISABLE_CHECK_MSG_RX_ACCEPTANCE`
- L190 `DESC_DISABLE_SPONTANEOUS_RES`
- L192 `DESC_DISABLE_SECURED_DATA_TRANSMISSION`
- L194 `DESC_DISABLE_MULTI_CHANNEL_SUPPORT`
- L196 `DESC_DISABLE_CODE_DOUBLED_DRIVER_API`
- L198 `DESC_DISABLE_BUSY_REPEAT_RESPONDER`
- L200 `DESC_DISABLE_OBD_FUNC_REQUEST`
- L202 `DESC_DISABLE_PARALLEL_OBD`
- L205 `DESC_ENABLE_CCL_SUPPORT`
- L207 `DESC_DISABLE_CCL_SUPPORT`
- L211 `DESC_DISABLE_MULTI_TP`
- L212 `DESC_ENABLE_SINGLE_TP_OPTIMIZED`
- L214 `DESC_ENABLE_MULTI_TP`
- L215 `DESC_DISABLE_SINGLE_TP_OPTIMIZED`
- L219 `DESC_DISABLE_PHYSREQ_TA_CHECK`
- L220 `DESC_DISABLE_PHYSREQ_SA_CHECK`
- L222 `DESC_DISABLE_FUNCREQ_TA_CHECK`
- L223 `DESC_DISABLE_FUNCREQ_SA_CHECK`
- L228 `DESC_ENABLE_NEG_RES_ON_SID_WITH_SET_BIT6`
- L231 `DESC_ENABLE_S1_TIME_MONITORING`
- L233 `DESC_DISABLE_RES_PENDING_TIME_LIMIT`
- L234 `DESC_DISABLE_RES_PENDING_COUNT_LIMIT`
- L236 `DESC_ENABLE_FORCE_RCR_RP`
- L238 `DESC_DISABLE_DYNAMICAL_P2_TIMINGS`
- L240 `DESC_DISABLE_OEM_HANDLING_ON_RCRRP_LIMIT_EXPIRATION`
- L243 `DESC_ENABLE_AUTO_STATES`
- L245 `DESC_DISABLE_ADDR_METHOD_CHECK_ON_SID`
- L247 `DESC_DISABLE_POS_RES_ON_SET_SPRMIB`
- L250 `DESC_DISABLE_SAME_NRC_ON_SUB_FUNC_AND_PARAM_ID`
- L253 `DESC_DISABLE_MAINHANDLER_MULTICALL`
- L254 `DESC_DISABLE_INDIVIDUAL_MAINHANDLER_MULTICALL`
- L255 `DESC_DISABLE_PERMANENT_MAINHANDLER_MULTICALL`
- L257 `DESC_ENABLE_MIN_REQ_LEN_CHECK`
- L259 `DESC_DISABLE_PREHANDLER_USAGE`
- L261 `DESC_ENABLE_POSTHANDLER_USAGE`
- L263 `DESC_DISABLE_GENERIC_USER_SERVICE_SUPPORT`
- L265 `DESC_DISABLE_GENERIC_USER_POST_HANDLER_SUPPORT`
- L267 `DESC_DISABLE_SESSION_FORMAT_STATE_CHECK`
- L270 `DESC_ENABLE_OBDII_NR_CONFORMANCE`
- L276 `DESC_ENABLE_SKIP_PID`
- L278 `DESC_DISABLE_SKIP_PID`
- L282 `DESC_DISABLE_ANY_PREHANDLER_USAGE`
- L284 `DESC_ENABLE_ANY_POSTHANDLER_USAGE`
- L286 `DESC_DISABLE_BINARY_SVCINST_SEARCH`
- L287 `DESC_ENABLE_LINEAR_SVCINST_SEARCH`
- L289 `DESC_ENABLE_SUB_SVC_USAGE`
- L291 `DESC_DISABLE_SESSION_SELFTRANSITION_SIM`
- L293 `DESC_ENABLE_COMM_CTRL`
- L295 `DESC_DISABLE_COM_CTRL_PARAM_CHECK`
- L297 `DESC_ENABLE_DYN_COM_CTRL_PARAM`
- L299 `DESC_ENABLE_RX_COMM_CONTROL`
- L301 `DESC_ENABLE_COMM_CTRL_SUBNET_SUPPORT`
- L303 `DESC_ENABLE_PROGRAMMING_SESSION`
- L305 `DESC_DISABLE_SERVICE_STOP_SESSION`
- L307 `DESC_ENABLE_P2_TIME_REPORT`
- L309 `DESC_ENABLE_SECURITY_ACCESS`
- L311 `DESC_DISABLE_DELAY_NRC_KEY`
- L313 `DESC_DISABLE_START_SA_TIMER_ON_POWERON`
- L315 `DESC_DISABLE_START_SA_TIMER_EXTERN`
- L317 `DESC_DISABLE_ATT_CNTR_RESET_ON_TIME_EXPIRE`
- L319 `DESC_DISABLE_REPORT_ATT_CNTR_STATE`
- L321 `DESC_DISABLE_ALLOW_KEY_REQ_ON_SEED_MISMATCH`
- L323 `DESC_ENABLE_RESET_AWAIT_KEY_ON_INVALID_REQ`
- L325 `DESC_DISABLE_ATT_CNTR_RESET_ON_KEY_OK`
- L327 `DESC_DISABLE_USE_OLD_SEED_PROTECTION`
- L329 `DESC_DISABLE_USE_ERR_CNTR_PROTECTION`
- L331 `DESC_DISABLE_MULTIPLE_SEED_LENGTH`
- L333 `DESC_DISABLE_TOTAL_ATTEMPT_COUNTER`
- L335 `DESC_DISABLE_UNIFIED_PID_MGR`
- L337 `DESC_DISABLE_SIM_PID_LIST_MODE`
- L339 `DESC_DISABLE_PID_PREHANDLER_USAGE`
- L341 `DESC_DISABLE_PID_POSTHANDLER_USAGE`
- L343 `DESC_DISABLE_DYN_DID_RES_LENGTH`
- L344 `DESC_DISABLE_PID_SECURITY_FILTER`
- L346 `DESC_ENABLE_ROUTINE_CONTROL_MODE`
- L348 `DESC_DISABLE_RID_PREHANDLER_USAGE`
- L350 `DESC_DISABLE_RID_POSTHANDLER_USAGE`
- L352 `DESC_ENABLE_RID_CTRL_OPER_CHECK`
- L354 `DESC_DISABLE_PERIODIC_MODE`
- L356 `DESC_DISABLE_SCHEDULER_UUDT_TRANSMITTER`
- L357 `DESC_DISABLE_SCHEDULER_USDT_TRANSMITTER`
- L359 `DESC_ENABLE_PID_LIST_MODE`
- L361 `DESC_DISABLE_DYN_DEFINED_DID_MODE`
- L363 `DESC_DISABLE_DEF_DYN_ID_BY_PID`
- L365 `DESC_DISABLE_DEF_DYN_ID_BY_MEM`
- L367 `DESC_DISABLE_DYN_DEFINED_DPID_MODE`
- L369 `DESC_DISABLE_ROE_SUPPORT`
- L371 `DESC_DISABLE_RES_RINGBUFFER`
- L373 `DESC_DISABLE_MULTI_VARIANT`
- L381 `DIAG_CANDESC__CORE_VERSION` = `0x0611`
- L382 `DIAG_CANDESC__CORE_RELEASE_VERSION` = `0x02`
- L383 `DESC_VERSION` = `0x0611`
- L384 `DESC_RELEASE_VERSION` = `0x02`
- L386 `kDescOk` = `((vuint8)0x00)`
- L387 `kDescFailed` = `((vuint8)0x01)`
- L392 `vuint8_least` = `vuintx`
- L395 `vsint8_least` = `vsintx`
- L401 `vuint8_least` = `vuintx`
- ... 생략 361개 (동일 패턴의 생성 심볼)

#### 주요 typedef/구조체
- L1002 `typedef vboolean DescBool;`
- L1004 `typedef unsigned int DescBitType;`
- L1006 `typedef vbittype     DescBitType;`
- L1010 `typedef struct t_AddrInfoIsoTpTag`
- L1040 `typedef enum`
- L1049 `typedef vuint8 t_descHandle;`
- L1052 `typedef vuint8 t_descUsdtNetBusHandle;`
- L1054 `typedef enum`
- L1061 `typedef enum`
- L1076 `typedef DESC_USDTNET_P2FARRAM(vuint8) DescUsdtNetMsg;`
- L1078 `typedef enum`
- L1083 `typedef union t_descUsdtNetAddrInfoTag`
- L1087 `typedef struct`
- L1098 `typedef vuint8                                        DescContextActivity;`
- L1099 `typedef vuint8                                        DescMsgItem;`
- L1101 `typedef DESC_P2FARRAM(DescMsgItem)                    DescMsg;`
- L1102 `typedef DESC_CONSTP2FARRAM(DescMsgItem)               DescConstPtr;`
- L1103 `typedef vuint16                                       DescMsgLen;`
- L1106 `typedef unsigned int DescStateGroup;`
- L1109 `typedef vuint8 DescNegResCode;`
- L1111 `typedef struct`
- L1119 `typedef struct`
- L1131 `typedef void DESC_API_CALL_TYPE (*DescMainHandler)     (DescMsgContext*);`
- L1134 `typedef vuint8 DescInitParam;`
- L1139 `typedef struct`
- L1154 `typedef vuint8 DescSecAccAttCtrChgEvType;`
- L1157 `typedef enum`
- L1163 `typedef struct`

#### 주요 전역 변수/테이블 선언
- L73 `Ktw    Fixed             ESCAN00049396 UDS                            No positive response sent if SPRMIB=TRUE and API DescForceRcrRpResponse is used`
- L1012 `vuint8  Dummy;`
- L1017 `vuint16 TransmitID;`
- L1018 `vuint16 ReceiveID;`
- L1026 `vuint8 SourceAddress;`
- L1027 `vuint8 TargetAddress;`
- L1030 `vuint8 CanChannel;`
- L1034 `vuint16 BaseAddress;`
- L1094 `vuint16               availBufferLength;`
- L1116 `} DescMsgAddInfo;`
- L1121 `DescMsg             reqData;`
- L1122 `DescMsgLen          reqDataLen;`
- L1123 `DescMsg             resData;`
- L1124 `DescMsgLen          resDataLen;`
- L1125 `DescMsgAddInfo      msgAddInfo;`
- L1128 `} DescMsgContext;`
- L1150 `}DescOemCommControlInfo;`
- L1161 `}DescSecurityAccessStatus;`
- L1165 `DescStateGroup            securityLevel;`
- L1166 `DescSecurityAccessStatus  status;`
- L1167 `vuint8                    dataLen;`
- L1168 `DescMsg                   dataPtr;`
- L1170 `} DescSecurityAccessContext;`
- L1284 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 g_descMainVersion;`
- L1285 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 g_descSubVersion;`
- L1286 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 g_descBugFixVersion;`

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 38

- L1180 `DescGetBuffer(TP_CHANNEL_RX_FORMAL_PARAM_DEF_FIRST vuint16 dataLength)` [선언]
- L1181 `DescRxErrorIndication(TP_CHANNEL_RX_FORMAL_PARAM_DEF_FIRST vuint8 status)` [선언]
- L1182 `DescPhysReqInd(TP_CHANNEL_RX_FORMAL_PARAM_DEF_FIRST vuint16 dataLen)` [선언]
- L1186 `DescGetFuncBuffer(vuint16 dataLength)` [선언]
- L1188 `DescFuncReqInd(vuint16 dataLen)` [선언]
- L1192 `DescCopyToCAN(TpCopyToCanInfoStructPtr infoStruct)` [선언]
- L1193 `DescTxErrorIndication(TP_CHANNEL_TX_FORMAL_PARAM_DEF_FIRST vuint8 status)` [선언]
- L1194 `DescConfirmation(TP_CHANNEL_TX_FORMAL_PARAM_DEF_FIRST vuint8 status)` [선언]
- L1199 `DescSendSpontaneousResponse(DescMsg resData, DescMsgLen resLen, t_descUsdtNetBus* pBusInfo, t_descUsdtNetResType repType)` [선언]
- L1203 `DescGetActivityState(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L1206 `DescGetTesterAddress(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L1210 `DescGetCurrentBusInfo(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L1214 `DescForceRcrRpResponse(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L1219 `DescStateInit(void)` [선언]
- L1222 `DescGetStateSession(void)` [선언]
- L1223 `DescSetStateSession(DescStateGroup descState)` [선언]
- L1224 `DescGetStateSecurityAccess(void)` [선언]
- L1225 `DescSetStateSecurityAccess(DescStateGroup descState)` [선언]
- L1228 `DescGetSessionStateOfSessionId(DescMsgItem sessionId)` [선언]
- L1230 `DescGetSessionIdOfSessionState(DescStateGroup sessionState)` [선언]
- L1232 `DescGetSessionTimings(DescStateGroup sessionState, vuint16* p2Time_1ms, vuint16* p2ExTime_10ms)` [선언]
- L1237 `DescStartRepeatedServiceCall(DESC_CONTEXT_FORMAL_PARAM_DEF_FIRST DescMainHandler mainHandler)` [선언]
- L1240 `DescProcessingDone(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L1242 `DescSetNegResponse(DESC_CONTEXT_FORMAL_PARAM_DEF_FIRST DescNegResCode errorCode)` [선언]
- L1245 `DescGetServiceId(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L1249 `DescInitPowerOn(DescInitParam initParam)` [선언]
- L1251 `DescInit(DescInitParam initParam)` [선언]
- L1254 `DescTask(void)` [선언]
- L1256 `DescTimerTask(void)` [선언]
- L1257 `DescStateTask(void)` [선언]
- L1260 `DescIsSuppressPosResBitSet(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L1263 `DescSecurityAccessSeedReady(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L1265 `DescSecurityAccessKeyChecked(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L1269 `DescGetAttemptCounterValue(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L1270 `DescSetAttemptCounterValue(DESC_CONTEXT_FORMAL_PARAM_DEF_FIRST vuint8 value)` [선언]
- L1273 `DescStartSecureTimer(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]
- L1362 `DescEnableCommunication(void)` [선언]
- L1369 `DescSessionTransitionChecked(DESC_CONTEXT_FORMAL_PARAM_DEF_ONLY)` [선언]

### 4. 각 함수별 역할 설명

- `DescGetBuffer` (L1180, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescRxErrorIndication` (L1181, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `DescPhysReqInd` (L1182, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescGetFuncBuffer` (L1186, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescFuncReqInd` (L1188, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescCopyToCAN` (L1192, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescTxErrorIndication` (L1193, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `DescConfirmation` (L1194, 선언): 송신 완료 통지나 확인 상태를 처리하고 상위 계층 콜백/플래그를 갱신한다.
- `DescSendSpontaneousResponse` (L1199, 선언): ---- Extended functionality ----
- `DescGetActivityState` (L1203, 선언): Reports the current diagnostic activity state (e.g. Idle, ActiveRx, ActiveProcess, ActiveTx.
- `DescGetTesterAddress` (L1206, 선언): Reports the current diagnostic activity state (e.g. Idle, ActiveRx, ActiveProcess, ActiveTx.
- `DescGetCurrentBusInfo` (L1210, 선언): Get access to the current communication parameters
- `DescForceRcrRpResponse` (L1214, 선언): Force CANdesc to send RCR-RP response
- `DescStateInit` (L1219, 선언): If needed - reset the whole state machine (only the states defined in the CDD!!!)
- `DescGetStateSession` (L1222, 선언): If needed - reset the whole state machine (only the states defined in the CDD!!!)
- `DescSetStateSession` (L1223, 선언): If needed - reset the whole state machine (only the states defined in the CDD!!!)
- `DescGetStateSecurityAccess` (L1224, 선언): If needed - reset the whole state machine (only the states defined in the CDD!!!)
- `DescSetStateSecurityAccess` (L1225, 선언): If needed - reset the whole state machine (only the states defined in the CDD!!!)
- `DescGetSessionStateOfSessionId` (L1228, 선언): Converts session id (sub-function) to session state
- `DescGetSessionIdOfSessionState` (L1230, 선언): Converts session state to session id (sub-function)
- `DescGetSessionTimings` (L1232, 선언): Get access to the session specific timings
- `DescStartRepeatedServiceCall` (L1237, 선언): Use this function if you want your application to be polled for a certain service
- `DescProcessingDone` (L1240, 선언): Activate linear response transmission
- `DescSetNegResponse` (L1242, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescGetServiceId` (L1245, 선언): Returns the current service's Sid
- `DescInitPowerOn` (L1249, 선언): Initilize the CANdesc on PowerOnReset using this function
- `DescInit` (L1251, 선언): Initilize the CANdesc if needed during the ECU  runtime using this function (status reset)
- `DescTask` (L1254, 선언): DescTask function must be call cyclically in the specified period of time
- `DescTimerTask` (L1256, 선언): DescTask function must be call cyclically in the specified period of time
- `DescStateTask` (L1257, 선언): DescTask function must be call cyclically in the specified period of time
- `DescIsSuppressPosResBitSet` (L1260, 선언): Returns true if the positive response will be supressed.
- `DescSecurityAccessSeedReady` (L1263, 선언): Confirmation for seed generation
- `DescSecurityAccessKeyChecked` (L1265, 선언): Confirmation for key validation
- `DescGetAttemptCounterValue` (L1269, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescSetAttemptCounterValue` (L1270, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescStartSecureTimer` (L1273, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescEnableCommunication` (L1362, 선언): Communication control anti-panic solution :)
- `DescSessionTransitionChecked` (L1369, 선언): Acknowledge the session transition

### 5. 필요시 코드 부연 설명

이 파일은 생성 설정/선언 성격이 강해 별도 코드 전문 인용 없이 심볼 표로 역할을 설명했다.

## 파일이름 : drv_par

- 경로: `src/CBD/Gen/drv_par.c`
- 파일 종류: `.c`
- 파일 크기: 4,282 bytes
- 파일 내용: DBC 신호 구조체와 메시지 버퍼를 생성한 드라이버 파라미터 파일이다.

### 1. .c/.h 파일 이름

- `drv_par.c`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 0
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 21

#### 주요 매크로
- 없음

#### 주요 typedef/구조체
- 없음

#### 주요 전역 변수/테이블 선언
- L54 `V_MEMRAM0 V_MEMRAM1 _c_IPSU_GST_buf V_MEMRAM2 IPSU_GST;`
- L55 `V_MEMRAM0 V_MEMRAM1 _c_IPSU_02_200ms_buf V_MEMRAM2 IPSU_02_200ms;`
- L56 `V_MEMRAM0 V_MEMRAM1 _c_IPSU_01_200ms_buf V_MEMRAM2 IPSU_01_200ms;`
- L57 `V_MEMRAM0 V_MEMRAM1 _c_IPSU_04_200ms_buf V_MEMRAM2 IPSU_04_200ms;`
- L58 `V_MEMRAM0 V_MEMRAM1 _c_IPSU_03_200ms_buf V_MEMRAM2 IPSU_03_200ms;`
- L59 `V_MEMRAM0 V_MEMRAM1 _c_IPSU_05_200ms_buf V_MEMRAM2 IPSU_05_200ms;`
- L60 `V_MEMRAM0 V_MEMRAM1 _c_CGW_01_200ms_buf V_MEMRAM2 CGW_01_200ms;`
- L61 `V_MEMRAM0 V_MEMRAM1 _c_IPSU_14_200ms_buf V_MEMRAM2 IPSU_14_200ms;`
- L62 `V_MEMRAM0 V_MEMRAM1 _c_IPSU_13_200ms_buf V_MEMRAM2 IPSU_13_200ms;`
- L63 `V_MEMRAM0 V_MEMRAM1 _c_IPSU_12_200ms_buf V_MEMRAM2 IPSU_12_200ms;`
- L64 `V_MEMRAM0 V_MEMRAM1 _c_IPSU_11_200ms_buf V_MEMRAM2 IPSU_11_200ms;`
- L65 `V_MEMRAM0 V_MEMRAM1 _c_IPSU_10_200ms_buf V_MEMRAM2 IPSU_10_200ms;`
- L66 `V_MEMRAM0 V_MEMRAM1 _c_IPSU_09_200ms_buf V_MEMRAM2 IPSU_09_200ms;`
- L67 `V_MEMRAM0 V_MEMRAM1 _c_IPSU_08_200ms_buf V_MEMRAM2 IPSU_08_200ms;`
- L68 `V_MEMRAM0 V_MEMRAM1 _c_IPSU_07_200ms_buf V_MEMRAM2 IPSU_07_200ms;`
- L69 `V_MEMRAM0 V_MEMRAM1 _c_IPSU_06_200ms_buf V_MEMRAM2 IPSU_06_200ms;`
- L70 `V_MEMRAM0 V_MEMRAM1 _c_CGW_05_200ms_buf V_MEMRAM2 CGW_05_200ms;`
- L71 `V_MEMRAM0 V_MEMRAM1 _c_CGW_04_200ms_buf V_MEMRAM2 CGW_04_200ms;`
- L72 `V_MEMRAM0 V_MEMRAM1 _c_CGW_03_200ms_buf V_MEMRAM2 CGW_03_200ms;`
- L73 `V_MEMRAM0 V_MEMRAM1 _c_CGW_02_200ms_buf V_MEMRAM2 CGW_02_200ms;`
- L74 `V_MEMRAM0 V_MEMRAM1 _c_DPSS_02_200ms_buf V_MEMRAM2 DPSS_02_200ms;`

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 0

- 없음

### 4. 각 함수별 역할 설명

- 함수 없음: 설정/타입/매크로 전용 파일이다.

### 5. 필요시 코드 부연 설명

이 파일은 생성 설정/선언 성격이 강해 별도 코드 전문 인용 없이 심볼 표로 역할을 설명했다.

## 파일이름 : drv_par

- 경로: `src/CBD/Gen/drv_par.h`
- 파일 종류: `.h`
- 파일 크기: 17,547 bytes
- 파일 내용: DBC 신호 구조체와 메시지 버퍼를 생성한 드라이버 파라미터 파일이다.

### 1. .c/.h 파일 이름

- `drv_par.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 2
- typedef/struct/enum 후보 수: 42
- 전역 변수/테이블 선언 후보 수: 105

#### 주요 매크로
- L46 `__DRV_PAR_H__`
- L532 `MAGIC_NUMBER` = `538736488`

#### 주요 typedef/구조체
- L52 `typedef struct _c_IPSU_GST_msgTypeTag`
- L63 `typedef struct _c_IPSU_02_200ms_msgTypeTag`
- L74 `typedef struct _c_IPSU_01_200ms_msgTypeTag`
- L85 `typedef struct _c_IPSU_04_200ms_msgTypeTag`
- L96 `typedef struct _c_IPSU_03_200ms_msgTypeTag`
- L107 `typedef struct _c_IPSU_05_200ms_msgTypeTag`
- L118 `typedef struct _c_CGW_01_200ms_msgTypeTag`
- L133 `typedef struct _c_IPSU_14_200ms_msgTypeTag`
- L147 `typedef struct _c_IPSU_13_200ms_msgTypeTag`
- L159 `typedef struct _c_IPSU_12_200ms_msgTypeTag`
- L170 `typedef struct _c_IPSU_11_200ms_msgTypeTag`
- L181 `typedef struct _c_IPSU_10_200ms_msgTypeTag`
- L192 `typedef struct _c_IPSU_09_200ms_msgTypeTag`
- L203 `typedef struct _c_IPSU_08_200ms_msgTypeTag`
- L214 `typedef struct _c_IPSU_07_200ms_msgTypeTag`
- L225 `typedef struct _c_IPSU_06_200ms_msgTypeTag`
- L236 `typedef struct _c_CGW_05_200ms_msgTypeTag`
- L249 `typedef struct _c_CGW_04_200ms_msgTypeTag`
- L262 `typedef struct _c_CGW_03_200ms_msgTypeTag`
- L275 `typedef struct _c_CGW_02_200ms_msgTypeTag`
- L288 `typedef struct _c_DPSS_02_200ms_msgTypeTag`
- L319 `typedef union _c_IPSU_GST_bufTag`
- L324 `typedef union _c_IPSU_02_200ms_bufTag`
- L329 `typedef union _c_IPSU_01_200ms_bufTag`
- L334 `typedef union _c_IPSU_04_200ms_bufTag`
- L339 `typedef union _c_IPSU_03_200ms_bufTag`
- L344 `typedef union _c_IPSU_05_200ms_bufTag`
- L349 `typedef union _c_CGW_01_200ms_bufTag`
- L354 `typedef union _c_IPSU_14_200ms_bufTag`
- L359 `typedef union _c_IPSU_13_200ms_bufTag`
- L364 `typedef union _c_IPSU_12_200ms_bufTag`
- L369 `typedef union _c_IPSU_11_200ms_bufTag`
- L374 `typedef union _c_IPSU_10_200ms_bufTag`
- L379 `typedef union _c_IPSU_09_200ms_bufTag`
- L384 `typedef union _c_IPSU_08_200ms_bufTag`
- L389 `typedef union _c_IPSU_07_200ms_bufTag`
- L394 `typedef union _c_IPSU_06_200ms_bufTag`
- L399 `typedef union _c_CGW_05_200ms_bufTag`
- L404 `typedef union _c_CGW_04_200ms_bufTag`
- L409 `typedef union _c_CGW_03_200ms_bufTag`
- L414 `typedef union _c_CGW_02_200ms_bufTag`
- L419 `typedef union _c_DPSS_02_200ms_bufTag`

#### 주요 전역 변수/테이블 선언
- L62 `} _c_IPSU_GST_msgType;`
- L73 `} _c_IPSU_02_200ms_msgType;`
- L84 `} _c_IPSU_01_200ms_msgType;`
- L95 `} _c_IPSU_04_200ms_msgType;`
- L106 `} _c_IPSU_03_200ms_msgType;`
- L117 `} _c_IPSU_05_200ms_msgType;`
- L132 `} _c_CGW_01_200ms_msgType;`
- L146 `} _c_IPSU_14_200ms_msgType;`
- L158 `} _c_IPSU_13_200ms_msgType;`
- L169 `} _c_IPSU_12_200ms_msgType;`
- L180 `} _c_IPSU_11_200ms_msgType;`
- L191 `} _c_IPSU_10_200ms_msgType;`
- L202 `} _c_IPSU_09_200ms_msgType;`
- L213 `} _c_IPSU_08_200ms_msgType;`
- L224 `} _c_IPSU_07_200ms_msgType;`
- L235 `} _c_IPSU_06_200ms_msgType;`
- L248 `} _c_CGW_05_200ms_msgType;`
- L261 `} _c_CGW_04_200ms_msgType;`
- L274 `} _c_CGW_03_200ms_msgType;`
- L287 `} _c_CGW_02_200ms_msgType;`
- L306 `} _c_DPSS_02_200ms_msgType;`
- L321 `vuint8 _c[8];`
- L322 `_c_IPSU_GST_msgType IPSU_GST;`
- L323 `} _c_IPSU_GST_buf;`
- L326 `vuint8 _c[8];`
- L327 `_c_IPSU_02_200ms_msgType IPSU_02_200ms;`
- L328 `} _c_IPSU_02_200ms_buf;`
- L331 `vuint8 _c[8];`
- L332 `_c_IPSU_01_200ms_msgType IPSU_01_200ms;`
- L333 `} _c_IPSU_01_200ms_buf;`
- L336 `vuint8 _c[8];`
- L337 `_c_IPSU_04_200ms_msgType IPSU_04_200ms;`
- L338 `} _c_IPSU_04_200ms_buf;`
- L341 `vuint8 _c[8];`
- L342 `_c_IPSU_03_200ms_msgType IPSU_03_200ms;`
- L343 `} _c_IPSU_03_200ms_buf;`
- L346 `vuint8 _c[8];`
- L347 `_c_IPSU_05_200ms_msgType IPSU_05_200ms;`
- L348 `} _c_IPSU_05_200ms_buf;`
- L351 `vuint8 _c[3];`
- L352 `_c_CGW_01_200ms_msgType CGW_01_200ms;`
- L353 `} _c_CGW_01_200ms_buf;`
- L356 `vuint8 _c[8];`
- L357 `_c_IPSU_14_200ms_msgType IPSU_14_200ms;`
- L358 `} _c_IPSU_14_200ms_buf;`
- L361 `vuint8 _c[8];`
- L362 `_c_IPSU_13_200ms_msgType IPSU_13_200ms;`
- L363 `} _c_IPSU_13_200ms_buf;`
- L366 `vuint8 _c[8];`
- L367 `_c_IPSU_12_200ms_msgType IPSU_12_200ms;`
- L368 `} _c_IPSU_12_200ms_buf;`
- L371 `vuint8 _c[8];`
- L372 `_c_IPSU_11_200ms_msgType IPSU_11_200ms;`
- L373 `} _c_IPSU_11_200ms_buf;`
- L376 `vuint8 _c[8];`
- L377 `_c_IPSU_10_200ms_msgType IPSU_10_200ms;`
- L378 `} _c_IPSU_10_200ms_buf;`
- L381 `vuint8 _c[8];`
- L382 `_c_IPSU_09_200ms_msgType IPSU_09_200ms;`
- L383 `} _c_IPSU_09_200ms_buf;`
- L386 `vuint8 _c[8];`
- L387 `_c_IPSU_08_200ms_msgType IPSU_08_200ms;`
- L388 `} _c_IPSU_08_200ms_buf;`
- L391 `vuint8 _c[8];`
- L392 `_c_IPSU_07_200ms_msgType IPSU_07_200ms;`
- L393 `} _c_IPSU_07_200ms_buf;`
- L396 `vuint8 _c[8];`
- L397 `_c_IPSU_06_200ms_msgType IPSU_06_200ms;`
- L398 `} _c_IPSU_06_200ms_buf;`
- L401 `vuint8 _c[8];`
- L402 `_c_CGW_05_200ms_msgType CGW_05_200ms;`
- L403 `} _c_CGW_05_200ms_buf;`
- L406 `vuint8 _c[8];`
- L407 `_c_CGW_04_200ms_msgType CGW_04_200ms;`
- L408 `} _c_CGW_04_200ms_buf;`
- L411 `vuint8 _c[8];`
- L412 `_c_CGW_03_200ms_msgType CGW_03_200ms;`
- L413 `} _c_CGW_03_200ms_buf;`
- L416 `vuint8 _c[8];`
- L417 `_c_CGW_02_200ms_msgType CGW_02_200ms;`
- L418 `} _c_CGW_02_200ms_buf;`
- L421 `vuint8 _c[8];`
- L422 `_c_DPSS_02_200ms_msgType DPSS_02_200ms;`
- L423 `} _c_DPSS_02_200ms_buf;`
- L434 `V_MEMRAM0 extern  V_MEMRAM1 _c_IPSU_GST_buf V_MEMRAM2 IPSU_GST;`
- L441 `V_MEMRAM0 extern  V_MEMRAM1 _c_IPSU_02_200ms_buf V_MEMRAM2 IPSU_02_200ms;`
- L445 `V_MEMRAM0 extern  V_MEMRAM1 _c_IPSU_01_200ms_buf V_MEMRAM2 IPSU_01_200ms;`
- L449 `V_MEMRAM0 extern  V_MEMRAM1 _c_IPSU_04_200ms_buf V_MEMRAM2 IPSU_04_200ms;`
- L453 `V_MEMRAM0 extern  V_MEMRAM1 _c_IPSU_03_200ms_buf V_MEMRAM2 IPSU_03_200ms;`
- L457 `V_MEMRAM0 extern  V_MEMRAM1 _c_IPSU_05_200ms_buf V_MEMRAM2 IPSU_05_200ms;`
- L461 `V_MEMRAM0 extern  V_MEMRAM1 _c_CGW_01_200ms_buf V_MEMRAM2 CGW_01_200ms;`
- L465 `V_MEMRAM0 extern  V_MEMRAM1 _c_IPSU_14_200ms_buf V_MEMRAM2 IPSU_14_200ms;`
- L469 `V_MEMRAM0 extern  V_MEMRAM1 _c_IPSU_13_200ms_buf V_MEMRAM2 IPSU_13_200ms;`
- L473 `V_MEMRAM0 extern  V_MEMRAM1 _c_IPSU_12_200ms_buf V_MEMRAM2 IPSU_12_200ms;`
- L477 `V_MEMRAM0 extern  V_MEMRAM1 _c_IPSU_11_200ms_buf V_MEMRAM2 IPSU_11_200ms;`
- L481 `V_MEMRAM0 extern  V_MEMRAM1 _c_IPSU_10_200ms_buf V_MEMRAM2 IPSU_10_200ms;`
- L485 `V_MEMRAM0 extern  V_MEMRAM1 _c_IPSU_09_200ms_buf V_MEMRAM2 IPSU_09_200ms;`
- L489 `V_MEMRAM0 extern  V_MEMRAM1 _c_IPSU_08_200ms_buf V_MEMRAM2 IPSU_08_200ms;`
- L493 `V_MEMRAM0 extern  V_MEMRAM1 _c_IPSU_07_200ms_buf V_MEMRAM2 IPSU_07_200ms;`
- L497 `V_MEMRAM0 extern  V_MEMRAM1 _c_IPSU_06_200ms_buf V_MEMRAM2 IPSU_06_200ms;`
- ... 생략 5개 (동일 패턴의 생성 심볼)

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 0

- 없음

### 4. 각 함수별 역할 설명

- 함수 없음: 설정/타입/매크로 전용 파일이다.

### 5. 필요시 코드 부연 설명

이 파일은 생성 설정/선언 성격이 강해 별도 코드 전문 인용 없이 심볼 표로 역할을 설명했다.

## 파일이름 : il_cfg

- 경로: `src/CBD/Gen/il_cfg.h`
- 파일 종류: `.h`
- 파일 크기: 7,850 bytes
- 파일 내용: IL 기능 스위치, 채널 수, Tx/Rx 오브젝트 수와 주기 설정을 정의한다.

### 1. .c/.h 파일 이름

- `il_cfg.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 76
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 0

#### 주요 매크로
- L46 `__IL_CFG_H__`
- L52 `IL_VECTORDLL_VERSION` = `0x0115`
- L53 `IL_VECTORDLL_RELEASE_VERSION` = `0x03`
- L54 `IL_IMPLEMENTATION_VERSION` = `0x0202`
- L61 `IL_DISABLE_SYS_INIT_FCT`
- L62 `IL_ENABLE_TX`
- L63 `IL_DISABLE_SYS_SIGNAL_INIT_FCT`
- L65 `IL_DISABLE_TX_SIGNAL_START_FCT`
- L66 `IL_DISABLE_TX_SIGNAL_STOP_FCT`
- L67 `IL_DISABLE_TX_CONFIRMATION_FLAG`
- L68 `IL_DISABLE_TX_TIMEOUT_FLAG`
- L69 `IL_DISABLE_TX_DEFAULTVALUE`
- L70 `IL_DISABLE_TX_UPDATE_BITS`
- L72 `IL_DISABLE_TX_START_DEFAULTVALUE`
- L73 `IL_DISABLE_TX_STOP_DEFAULTVALUE`
- L76 `IL_DISABLE_TX_TIMEOUT`
- L77 `IL_DISABLE_TX_SEND_ON_INIT`
- L78 `IL_DISABLE_TX_FAST_ON_START`
- L79 `IL_ENABLE_TX_SECURE_EVENT`
- L80 `IL_DISABLE_TX_CYCLIC_EVENT`
- L81 `IL_ENABLE_TX_POLLING`
- L82 `IL_DISABLE_TX_INTERRUPT`
- L83 `IL_DISABLE_TX_VARYING_TIMEOUT`
- L84 `IL_DISABLE_TX_MODE_SIGNALS`
- L87 `IL_DISABLE_TX_STARTSTOP_CYCLIC`
- L88 `IL_DISABLE_SYS_ARGCHECK`
- L89 `IL_ENABLE_SYS_TESTDEBUG`
- L90 `IL_ENABLE_RX`
- L91 `IL_DISABLE_SYS_TX_START_FCT`
- L92 `IL_DISABLE_SYS_TX_STOP_FCT`
- L93 `IL_DISABLE_SYS_TX_REPETITIONS_ARE_ACTIVE_FCT`
- L94 `IL_DISABLE_SYS_TX_SIGNALS_ARE_ACTIVE_FCT`
- L95 `IL_DISABLE_SYS_RX_RESET_TIMEOUT_FLAGS_ON_ILRXRELEASE`
- L97 `IL_DISABLE_RX_SIGNAL_START_FCT`
- L98 `IL_DISABLE_RX_SIGNAL_STOP_FCT`
- L99 `IL_DISABLE_RX_INDICATION_FLAG`
- L100 `IL_DISABLE_RX_TIMEOUT_FLAG`
- L101 `IL_DISABLE_RX_FIRSTVALUE_FLAG`
- L102 `IL_DISABLE_RX_DATACHANGED_FLAG`
- L103 `IL_DISABLE_RX_DEFAULTVALUE`
- L105 `IL_DISABLE_RX_START_DEFAULTVALUE`
- L106 `IL_DISABLE_RX_STOP_DEFAULTVALUE`
- L109 `IL_DISABLE_RX_TIMEOUT`
- L110 `IL_DISABLE_RX_POLLING`
- L111 `IL_ENABLE_RX_INTERRUPT`
- L112 `IL_DISABLE_RX_TIMEOUT_DELAY`
- L113 `IL_DISABLE_RX_MODE_SIGNALS`
- L116 `IL_DISABLE_SYS_RX_START_FCT`
- L117 `IL_DISABLE_SYS_RX_STOP_FCT`
- L118 `IL_DISABLE_TX_DYNAMIC_CYCLETIME`
- L119 `IL_DISABLE_SYS_MULTI_ECU_PHYS`
- L121 `IL_DISABLE_TX_VARYING_REPETITION`
- L125 `IL_DISABLE_RX_DYNAMIC_TIMEOUT`
- L134 `IL_TYPE_HMC`
- L141 `kIlNumberOfChannels` = `1`
- L142 `kIlNumberOfTxObjects` = `15`
- L144 `kIlNumberOfTxTimeoutCounters` = `0`
- L148 `kIlNumberOfRxTimeoutCounters` = `0`
- L152 `kIlNoRxTimeoutSupervision` = `0xFF`
- L156 `kIlNumberOfTxConfirmationFlags` = `0`
- L160 `kIlNumberOfTxTimeoutFlags` = `0`
- L164 `kIlNumberOfTimerFlagBytes` = `0`
- L168 `kIlNumberOfTxRepetitions` = `3`
- L171 `kIlTxCycleTime` = `10`
- L173 `kIlTxTimeout` = `5`
- L176 `kIlNumberOfRxObjects` = `5`
- L178 `kIlNumberOfRxIndicationFlags` = `0`
- L182 `kIlNumberOfRxTimeoutFlags` = `0`
- L186 `kIlNumberOfRxFirstvalueFlags` = `0`
- L190 `kIlNumberOfRxDataChangedFlags` = `0`
- L194 `kIlNumberOfRxIndicationBits` = `0`
- L197 `kIlRxCycleTime` = `10`
- L198 `kIlCanNumberOfRxObjects` = `5`
- L199 `kIlNumberOfIdentities` = `1`
- L206 `kIlNumberOfNodes` = `1`
- L217 `MAGIC_NUMBER` = `538736488`

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

## 파일이름 : il_par

- 경로: `src/CBD/Gen/il_par.c`
- 파일 종류: `.c`
- 파일 크기: 67,094 bytes
- 파일 내용: DBC 기반 IL 신호 접근 함수, 메시지 테이블, 주기/이벤트 설정을 생성한다.

### 1. .c/.h 파일 이름

- `il_par.c`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 38
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 45

#### 주요 매크로
- L94 `IlPrivateGetTxDPSS_Crc2Val()` = `(DPSS_02_200ms.DPSS_02_200ms.DPSS_Crc2Val)`
- L99 `IlPrivateGetTxDPSS_AlvCnt2Val()` = `(DPSS_02_200ms.DPSS_02_200ms.DPSS_AlvCnt2Val)`
- L104 `IlPrivateGetTxIPSU_Recline_VrtLmt_Set_Sta()` = `(IPSU_14_200ms.IPSU_14_200ms.IPSU_Recline_VrtLmt_Set_Sta)`
- L109 `IlPrivateGetTxIPSU_Slide_VrtLmt_Set_Sta()` = `(IPSU_14_200ms.IPSU_14_200ms.IPSU_Slide_VrtLmt_Set_Sta)`
- L114 `IlPrivateGetTxIPSU_Height_VrtLmt_Set_Sta()` = `(IPSU_14_200ms.IPSU_14_200ms.IPSU_Height_VrtLmt_Set_Sta)`
- L119 `IlPrivateGetTxIPSU_Tilt_VrtLmt_Set_Sta()` = `(IPSU_14_200ms.IPSU_14_200ms.IPSU_Tilt_VrtLmt_Set_Sta)`
- L124 `IlPrivateGetTxIPSU_Footrest_Current_Sta()` = `(IPSU_13_200ms.IPSU_13_200ms.IPSU_Footrest_Current_Sta)`
- L750 `DPSS_Crc2ValValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxDPSS_Crc2Val())))`
- L754 `DPSS_AlvCnt2ValValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxDPSS_AlvCnt2Val())))`
- L758 `IPSU_Recline_VrtLmt_Set_StaValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Recline_VrtLmt_Set_Sta())))`
- L762 `IPSU_Slide_VrtLmt_Set_StaValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Slide_VrtLmt_Set_Sta())))`
- L766 `IPSU_Height_VrtLmt_Set_StaValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Height_VrtLmt_Set_Sta())))`
- L770 `IPSU_Tilt_VrtLmt_Set_StaValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Tilt_VrtLmt_Set_Sta())))`
- L774 `IPSU_Footrest_Current_StaValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Footrest_Current_Sta())))`
- L778 `IPSU_Tilt_Min_VrtLmt_ValueValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Tilt_Min_VrtLmt_Value())))`
- L782 `IPSU_Tilt_Max_VrtLmt_ValueValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Tilt_Max_VrtLmt_Value())))`
- L786 `IPSU_Tilt_Min_PosValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Tilt_Min_Pos())))`
- L790 `IPSU_Tilt_Max_PosValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Tilt_Max_Pos())))`
- L794 `IPSU_Tilt_Current_PosValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Tilt_Current_Pos())))`
- L798 `IPSU_Tilt_Target_PosValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Tilt_Target_Pos())))`
- L802 `IPSU_Height_Min_VrtLmt_ValueValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Height_Min_VrtLmt_Value())))`
- L806 `IPSU_Height_Max_VrtLmt_ValueValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Height_Max_VrtLmt_Value())))`
- L810 `IPSU_Height_Min_PosValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Height_Min_Pos())))`
- L814 `IPSU_Height_Max_PosValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Height_Max_Pos())))`
- L818 `IPSU_Height_Current_PosValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Height_Current_Pos())))`
- L822 `IPSU_Height_Target_PosValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Height_Target_Pos())))`
- L826 `IPSU_Slide_Min_VrtLmt_ValueValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Slide_Min_VrtLmt_Value())))`
- L830 `IPSU_Slide_Max_VrtLmt_ValueValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Slide_Max_VrtLmt_Value())))`
- L834 `IPSU_Slide_Min_PosValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Slide_Min_Pos())))`
- L838 `IPSU_Slide_Max_PosValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Slide_Max_Pos())))`
- L842 `IPSU_Slide_Current_PosValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Slide_Current_Pos())))`
- L846 `IPSU_Slide_Target_PosValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Slide_Target_Pos())))`
- L850 `IPSU_Recline_Min_VrtLmt_ValueValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Recline_Min_VrtLmt_Value())))`
- L854 `IPSU_Recline_Max_VrtLmt_ValueValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Recline_Max_VrtLmt_Value())))`
- L858 `IPSU_Recline_Min_PosValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Recline_Min_Pos())))`
- L862 `IPSU_Recline_Max_PosValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Recline_Max_Pos())))`
- L866 `IPSU_Recline_Current_PosValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Recline_Current_Pos())))`
- L870 `IPSU_Recline_Target_PosValueChanged(sigData)` = `(((sigData) != (IlPrivateGetTxIPSU_Recline_Target_Pos())))`

#### 주요 typedef/구조체
- 없음

#### 주요 전역 변수/테이블 선언
- L137 `vuint16 rc;`
- L152 `vuint16 rc;`
- L167 `vuint32 rc;`
- L184 `vuint32 rc;`
- L201 `vuint32 rc;`
- L218 `vuint32 rc;`
- L235 `vuint16 rc;`
- L250 `vuint16 rc;`
- L265 `vuint32 rc;`
- L282 `vuint32 rc;`
- L299 `vuint32 rc;`
- L316 `vuint32 rc;`
- L333 `vuint16 rc;`
- L348 `vuint16 rc;`
- L363 `vuint32 rc;`
- L380 `vuint32 rc;`
- L397 `vuint32 rc;`
- L414 `vuint32 rc;`
- L431 `vuint16 rc;`
- L446 `vuint16 rc;`
- L461 `vuint32 rc;`
- L478 `vuint32 rc;`
- L495 `vuint32 rc;`
- L512 `vuint32 rc;`
- L528 `V_MEMROM0 V_MEMROM1 vuint8 V_MEMROM2 IlTxTimeoutIndirection[kIlNumberOfTxObjects] =`
- L550 `V_MEMROM0 V_MEMROM1 IltTxCounter V_MEMROM2 IlTxFastOnStartDuration[kIlNumberOfTxObjects] =`
- L572 `V_MEMROM0 V_MEMROM1 IltTxCounter V_MEMROM2 IlTxFastOnStartMuxDelay[kIlNumberOfTxObjects] =`
- L594 `V_MEMROM0 V_MEMROM1 IltTxCounter V_MEMROM2 IlTxStartCycles[kIlNumberOfTxObjects] =`
- L616 `V_MEMROM0 V_MEMROM1 IltTxUpdateCounter V_MEMROM2 IlTxUpdateCycles[kIlNumberOfTxObjects] =`
- L638 `V_MEMROM0 V_MEMROM1 IltTxCounter V_MEMROM2 IlTxCyclicCycles[kIlNumberOfTxObjects] =`
- L660 `V_MEMROM0 V_MEMROM1 IltTxCounter V_MEMROM2 IlTxEventCycles[kIlNumberOfTxObjects] =`
- L682 `V_MEMROM0 V_MEMROM1 IlConfirmationFct V_MEMROM2 IlTxConfirmationFctPtr[kIlNumberOfTxObjects] =`
- L704 `V_MEMROM0 V_MEMROM1 IltTxTimeoutCounter V_MEMROM2 IlTxTimeout[kIlNumberOfChannels] =`
- L712 `V_MEMROM0 V_MEMROM1 IltTxRepetitionCounter V_MEMROM2 IlTxRepetitionCounters[kIlNumberOfTxObjects] =`
- L734 `V_MEMROM0 V_MEMROM1 IlIndicationFct V_MEMROM2 IlCanRxIndicationFctPtr[kIlCanNumberOfRxObjects] =`
- L932 `vuint16 rc;`
- L947 `vuint16 rc;`
- L962 `vuint16 rc;`
- L977 `vuint16 rc;`
- L992 `vuint16 rc;`
- L1007 `vuint16 rc;`
- L1022 `vuint16 rc;`
- L1037 `vuint16 rc;`
- L1726 `V_MEMROM0 V_MEMROM1 CanTransmitHandle V_MEMROM2 IlTxIndirection[kIlNumberOfTxObjects] =`
- L1752 `V_MEMROM0 V_MEMROM1 vuint8 V_MEMROM2 IlTxType[kIlNumberOfTxObjects] =`

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 75

- L135 `IlPrivateGetTxIPSU_Tilt_Min_VrtLmt_Value(void)` [정의]
- L150 `IlPrivateGetTxIPSU_Tilt_Max_VrtLmt_Value(void)` [정의]
- L165 `IlPrivateGetTxIPSU_Tilt_Min_Pos(void)` [정의]
- L182 `IlPrivateGetTxIPSU_Tilt_Max_Pos(void)` [정의]
- L199 `IlPrivateGetTxIPSU_Tilt_Current_Pos(void)` [정의]
- L216 `IlPrivateGetTxIPSU_Tilt_Target_Pos(void)` [정의]
- L233 `IlPrivateGetTxIPSU_Height_Min_VrtLmt_Value(void)` [정의]
- L248 `IlPrivateGetTxIPSU_Height_Max_VrtLmt_Value(void)` [정의]
- L263 `IlPrivateGetTxIPSU_Height_Min_Pos(void)` [정의]
- L280 `IlPrivateGetTxIPSU_Height_Max_Pos(void)` [정의]
- L297 `IlPrivateGetTxIPSU_Height_Current_Pos(void)` [정의]
- L314 `IlPrivateGetTxIPSU_Height_Target_Pos(void)` [정의]
- L331 `IlPrivateGetTxIPSU_Slide_Min_VrtLmt_Value(void)` [정의]
- L346 `IlPrivateGetTxIPSU_Slide_Max_VrtLmt_Value(void)` [정의]
- L361 `IlPrivateGetTxIPSU_Slide_Min_Pos(void)` [정의]
- L378 `IlPrivateGetTxIPSU_Slide_Max_Pos(void)` [정의]
- L395 `IlPrivateGetTxIPSU_Slide_Current_Pos(void)` [정의]
- L412 `IlPrivateGetTxIPSU_Slide_Target_Pos(void)` [정의]
- L429 `IlPrivateGetTxIPSU_Recline_Min_VrtLmt_Value(void)` [정의]
- L444 `IlPrivateGetTxIPSU_Recline_Max_VrtLmt_Value(void)` [정의]
- L459 `IlPrivateGetTxIPSU_Recline_Min_Pos(void)` [정의]
- L476 `IlPrivateGetTxIPSU_Recline_Max_Pos(void)` [정의]
- L493 `IlPrivateGetTxIPSU_Recline_Current_Pos(void)` [정의]
- L510 `IlPrivateGetTxIPSU_Recline_Target_Pos(void)` [정의]
- L880 `IlTxSignalsAreActive(void)` [정의]
- L895 `IlResetCanIndicationFlags(void)` [정의]
- L911 `IlResetCanConfirmationFlags(void)` [정의]
- L930 `IlGetRxCGW_Tilt_Min_VrtLmt_Value(void)` [정의]
- L945 `IlGetRxCGW_Tilt_Max_VrtLmt_Value(void)` [정의]
- L960 `IlGetRxCGW_Height_Min_VrtLmt_Value(void)` [정의]
- L975 `IlGetRxCGW_Height_Max_VrtLmt_Value(void)` [정의]
- L990 `IlGetRxCGW_Slide_Min_VrtLmt_Value(void)` [정의]
- L1005 `IlGetRxCGW_Slide_Max_VrtLmt_Value(void)` [정의]
- L1020 `IlGetRxCGW_Recline_Min_VrtLmt_Value(void)` [정의]
- L1035 `IlGetRxCGW_Recline_Max_VrtLmt_Value(void)` [정의]
- L1056 `IlPutTxDPSS_Crc2Val(vuint8 sigData)` [정의]
- L1072 `IlPutTxDPSS_AlvCnt2Val(vuint8 sigData)` [정의]
- L1088 `IlPutTxDPSS_LumbarUpSw(vuint8 sigData)` [정의]
- L1101 `IlPutTxDPSS_LumbarMidSw(vuint8 sigData)` [정의]
- L1114 `IlPutTxDPSS_LumbarLowSw(vuint8 sigData)` [정의]
- L1127 `IlPutTxDPSS_LumbarDefSw(vuint8 sigData)` [정의]
- L1140 `IlPutTxDPSS_BolsterInfSw(vuint8 sigData)` [정의]
- L1153 `IlPutTxDPSS_BolsterDefSw(vuint8 sigData)` [정의]
- L1166 `IlPutTxDPSS_ActiveSw(vuint8 sigData)` [정의]
- L1179 `IlPutTxDPSS_CushInfSw(vuint8 sigData)` [정의]
- L1192 `IlPutTxDPSS_CushDefSw(vuint8 sigData)` [정의]
- L1205 `IlPutTxIPSU_Recline_VrtLmt_Set_Sta(vuint8 sigData)` [정의]
- L1221 `IlPutTxIPSU_Slide_VrtLmt_Set_Sta(vuint8 sigData)` [정의]
- L1237 `IlPutTxIPSU_Height_VrtLmt_Set_Sta(vuint8 sigData)` [정의]
- L1253 `IlPutTxIPSU_Tilt_VrtLmt_Set_Sta(vuint8 sigData)` [정의]
- L1269 `IlPutTxIPSU_Footrest_Current_Sta(vuint8 sigData)` [정의]
- L1285 `IlPutTxIPSU_Tilt_Min_VrtLmt_Value(vuint16 sigData)` [정의]
- L1302 `IlPutTxIPSU_Tilt_Max_VrtLmt_Value(vuint16 sigData)` [정의]
- L1319 `IlPutTxIPSU_Tilt_Min_Pos(vuint32 sigData)` [정의]
- L1338 `IlPutTxIPSU_Tilt_Max_Pos(vuint32 sigData)` [정의]
- L1357 `IlPutTxIPSU_Tilt_Current_Pos(vuint32 sigData)` [정의]
- L1376 `IlPutTxIPSU_Tilt_Target_Pos(vuint32 sigData)` [정의]
- L1395 `IlPutTxIPSU_Height_Min_VrtLmt_Value(vuint16 sigData)` [정의]
- L1412 `IlPutTxIPSU_Height_Max_VrtLmt_Value(vuint16 sigData)` [정의]
- L1429 `IlPutTxIPSU_Height_Min_Pos(vuint32 sigData)` [정의]
- L1448 `IlPutTxIPSU_Height_Max_Pos(vuint32 sigData)` [정의]
- L1467 `IlPutTxIPSU_Height_Current_Pos(vuint32 sigData)` [정의]
- L1486 `IlPutTxIPSU_Height_Target_Pos(vuint32 sigData)` [정의]
- L1505 `IlPutTxIPSU_Slide_Min_VrtLmt_Value(vuint16 sigData)` [정의]
- L1522 `IlPutTxIPSU_Slide_Max_VrtLmt_Value(vuint16 sigData)` [정의]
- L1539 `IlPutTxIPSU_Slide_Min_Pos(vuint32 sigData)` [정의]
- L1558 `IlPutTxIPSU_Slide_Max_Pos(vuint32 sigData)` [정의]
- L1577 `IlPutTxIPSU_Slide_Current_Pos(vuint32 sigData)` [정의]
- L1596 `IlPutTxIPSU_Slide_Target_Pos(vuint32 sigData)` [정의]
- L1615 `IlPutTxIPSU_Recline_Min_VrtLmt_Value(vuint16 sigData)` [정의]
- L1632 `IlPutTxIPSU_Recline_Max_VrtLmt_Value(vuint16 sigData)` [정의]
- L1649 `IlPutTxIPSU_Recline_Min_Pos(vuint32 sigData)` [정의]
- L1668 `IlPutTxIPSU_Recline_Max_Pos(vuint32 sigData)` [정의]
- L1687 `IlPutTxIPSU_Recline_Current_Pos(vuint32 sigData)` [정의]
- L1706 `IlPutTxIPSU_Recline_Target_Pos(vuint32 sigData)` [정의]

### 4. 각 함수별 역할 설명

- `IlPrivateGetTxIPSU_Tilt_Min_VrtLmt_Value` (L135, 정의): Handle:   16,Name:     IPSU_Tilt_Min_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPrivateGetTxIPSU_Tilt_Max_VrtLmt_Value` (L150, 정의): Handle:   17,Name:     IPSU_Tilt_Max_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPrivateGetTxIPSU_Tilt_Min_Pos` (L165, 정의): Handle:   18,Name:              IPSU_Tilt_Min_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Tilt_Max_Pos` (L182, 정의): Handle:   19,Name:              IPSU_Tilt_Max_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Tilt_Current_Pos` (L199, 정의): Handle:   20,Name:          IPSU_Tilt_Current_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Tilt_Target_Pos` (L216, 정의): Handle:   21,Name:           IPSU_Tilt_Target_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Height_Min_VrtLmt_Value` (L233, 정의): Handle:   22,Name:   IPSU_Height_Min_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPrivateGetTxIPSU_Height_Max_VrtLmt_Value` (L248, 정의): Handle:   23,Name:   IPSU_Height_Max_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPrivateGetTxIPSU_Height_Min_Pos` (L263, 정의): Handle:   24,Name:            IPSU_Height_Min_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Height_Max_Pos` (L280, 정의): Handle:   25,Name:            IPSU_Height_Max_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Height_Current_Pos` (L297, 정의): Handle:   26,Name:        IPSU_Height_Current_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Height_Target_Pos` (L314, 정의): Handle:   27,Name:         IPSU_Height_Target_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Slide_Min_VrtLmt_Value` (L331, 정의): Handle:   28,Name:    IPSU_Slide_Min_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPrivateGetTxIPSU_Slide_Max_VrtLmt_Value` (L346, 정의): Handle:   29,Name:    IPSU_Slide_Max_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPrivateGetTxIPSU_Slide_Min_Pos` (L361, 정의): Handle:   30,Name:             IPSU_Slide_Min_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Slide_Max_Pos` (L378, 정의): Handle:   31,Name:             IPSU_Slide_Max_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Slide_Current_Pos` (L395, 정의): Handle:   32,Name:         IPSU_Slide_Current_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Slide_Target_Pos` (L412, 정의): Handle:   33,Name:          IPSU_Slide_Target_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Recline_Min_VrtLmt_Value` (L429, 정의): Handle:   34,Name:  IPSU_Recline_Min_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPrivateGetTxIPSU_Recline_Max_VrtLmt_Value` (L444, 정의): Handle:   35,Name:  IPSU_Recline_Max_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPrivateGetTxIPSU_Recline_Min_Pos` (L459, 정의): Handle:   36,Name:           IPSU_Recline_Min_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Recline_Max_Pos` (L476, 정의): Handle:   37,Name:           IPSU_Recline_Max_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Recline_Current_Pos` (L493, 정의): Handle:   38,Name:       IPSU_Recline_Current_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Recline_Target_Pos` (L510, 정의): Handle:   39,Name:        IPSU_Recline_Target_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlTxSignalsAreActive` (L880, 정의): ----------------------------------------------------------------------------- &&&~ Implementation of a function to check IfActive flags -----------------------------------------------------------------------------
- `IlResetCanIndicationFlags` (L895, 정의): ----------------------------------------------------------------------------- &&&~ Implementation function to reset indication flags -----------------------------------------------------------------------------
- `IlResetCanConfirmationFlags` (L911, 정의): ----------------------------------------------------------------------------- &&&~ Implementation function to reset confirmation flags -----------------------------------------------------------------------------
- `IlGetRxCGW_Tilt_Min_VrtLmt_Value` (L930, 정의): Handle:    2,Name:      CGW_Tilt_Min_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlGetRxCGW_Tilt_Max_VrtLmt_Value` (L945, 정의): Handle:    3,Name:      CGW_Tilt_Max_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlGetRxCGW_Height_Min_VrtLmt_Value` (L960, 정의): Handle:    6,Name:    CGW_Height_Min_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlGetRxCGW_Height_Max_VrtLmt_Value` (L975, 정의): Handle:    7,Name:    CGW_Height_Max_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlGetRxCGW_Slide_Min_VrtLmt_Value` (L990, 정의): Handle:   10,Name:     CGW_Slide_Min_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlGetRxCGW_Slide_Max_VrtLmt_Value` (L1005, 정의): Handle:   11,Name:     CGW_Slide_Max_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlGetRxCGW_Recline_Min_VrtLmt_Value` (L1020, 정의): Handle:   14,Name:   CGW_Recline_Min_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlGetRxCGW_Recline_Max_VrtLmt_Value` (L1035, 정의): Handle:   15,Name:   CGW_Recline_Max_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPutTxDPSS_Crc2Val` (L1056, 정의): Handle:    0,Name:                   DPSS_Crc2Val,Size:  8,UsedBytes:  1,SingleSignal
- `IlPutTxDPSS_AlvCnt2Val` (L1072, 정의): Handle:    1,Name:                DPSS_AlvCnt2Val,Size:  4,UsedBytes:  1,SingleSignal
- `IlPutTxDPSS_LumbarUpSw` (L1088, 정의): Handle:    2,Name:                DPSS_LumbarUpSw,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxDPSS_LumbarMidSw` (L1101, 정의): Handle:    3,Name:               DPSS_LumbarMidSw,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxDPSS_LumbarLowSw` (L1114, 정의): Handle:    4,Name:               DPSS_LumbarLowSw,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxDPSS_LumbarDefSw` (L1127, 정의): Handle:    5,Name:               DPSS_LumbarDefSw,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxDPSS_BolsterInfSw` (L1140, 정의): Handle:    6,Name:              DPSS_BolsterInfSw,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxDPSS_BolsterDefSw` (L1153, 정의): Handle:    7,Name:              DPSS_BolsterDefSw,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxDPSS_ActiveSw` (L1166, 정의): Handle:    8,Name:                  DPSS_ActiveSw,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxDPSS_CushInfSw` (L1179, 정의): Handle:    9,Name:                 DPSS_CushInfSw,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxDPSS_CushDefSw` (L1192, 정의): Handle:   10,Name:                 DPSS_CushDefSw,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxIPSU_Recline_VrtLmt_Set_Sta` (L1205, 정의): Handle:   11,Name:    IPSU_Recline_VrtLmt_Set_Sta,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxIPSU_Slide_VrtLmt_Set_Sta` (L1221, 정의): Handle:   12,Name:      IPSU_Slide_VrtLmt_Set_Sta,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxIPSU_Height_VrtLmt_Set_Sta` (L1237, 정의): Handle:   13,Name:     IPSU_Height_VrtLmt_Set_Sta,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxIPSU_Tilt_VrtLmt_Set_Sta` (L1253, 정의): Handle:   14,Name:       IPSU_Tilt_VrtLmt_Set_Sta,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxIPSU_Footrest_Current_Sta` (L1269, 정의): Handle:   15,Name:      IPSU_Footrest_Current_Sta,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxIPSU_Tilt_Min_VrtLmt_Value` (L1285, 정의): Handle:   16,Name:     IPSU_Tilt_Min_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPutTxIPSU_Tilt_Max_VrtLmt_Value` (L1302, 정의): Handle:   17,Name:     IPSU_Tilt_Max_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPutTxIPSU_Tilt_Min_Pos` (L1319, 정의): Handle:   18,Name:              IPSU_Tilt_Min_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Tilt_Max_Pos` (L1338, 정의): Handle:   19,Name:              IPSU_Tilt_Max_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Tilt_Current_Pos` (L1357, 정의): Handle:   20,Name:          IPSU_Tilt_Current_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Tilt_Target_Pos` (L1376, 정의): Handle:   21,Name:           IPSU_Tilt_Target_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Height_Min_VrtLmt_Value` (L1395, 정의): Handle:   22,Name:   IPSU_Height_Min_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPutTxIPSU_Height_Max_VrtLmt_Value` (L1412, 정의): Handle:   23,Name:   IPSU_Height_Max_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPutTxIPSU_Height_Min_Pos` (L1429, 정의): Handle:   24,Name:            IPSU_Height_Min_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Height_Max_Pos` (L1448, 정의): Handle:   25,Name:            IPSU_Height_Max_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Height_Current_Pos` (L1467, 정의): Handle:   26,Name:        IPSU_Height_Current_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Height_Target_Pos` (L1486, 정의): Handle:   27,Name:         IPSU_Height_Target_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Slide_Min_VrtLmt_Value` (L1505, 정의): Handle:   28,Name:    IPSU_Slide_Min_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPutTxIPSU_Slide_Max_VrtLmt_Value` (L1522, 정의): Handle:   29,Name:    IPSU_Slide_Max_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPutTxIPSU_Slide_Min_Pos` (L1539, 정의): Handle:   30,Name:             IPSU_Slide_Min_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Slide_Max_Pos` (L1558, 정의): Handle:   31,Name:             IPSU_Slide_Max_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Slide_Current_Pos` (L1577, 정의): Handle:   32,Name:         IPSU_Slide_Current_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Slide_Target_Pos` (L1596, 정의): Handle:   33,Name:          IPSU_Slide_Target_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Recline_Min_VrtLmt_Value` (L1615, 정의): Handle:   34,Name:  IPSU_Recline_Min_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPutTxIPSU_Recline_Max_VrtLmt_Value` (L1632, 정의): Handle:   35,Name:  IPSU_Recline_Max_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPutTxIPSU_Recline_Min_Pos` (L1649, 정의): Handle:   36,Name:           IPSU_Recline_Min_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Recline_Max_Pos` (L1668, 정의): Handle:   37,Name:           IPSU_Recline_Max_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Recline_Current_Pos` (L1687, 정의): Handle:   38,Name:       IPSU_Recline_Current_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Recline_Target_Pos` (L1706, 정의): Handle:   39,Name:        IPSU_Recline_Target_Pos,Size: 32,UsedBytes:  4,SingleSignal

### 5. 필요시 코드 부연 설명

이 파일은 생성 설정/선언 성격이 강해 별도 코드 전문 인용 없이 심볼 표로 역할을 설명했다.

## 파일이름 : il_par

- 경로: `src/CBD/Gen/il_par.h`
- 파일 종류: `.h`
- 파일 크기: 40,080 bytes
- 파일 내용: DBC 기반 IL 신호 접근 함수, 메시지 테이블, 주기/이벤트 설정을 생성한다.

### 1. .c/.h 파일 이름

- `il_par.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 246
- typedef/struct/enum 후보 수: 5
- 전역 변수/테이블 선언 후보 수: 0

#### 주요 매크로
- L46 `__IL_PAR_H__`
- L78 `IlRxMsgHndCGW_05_200ms` = `0`
- L79 `IlRxMsgHndCGW_04_200ms` = `1`
- L80 `IlRxMsgHndCGW_03_200ms` = `2`
- L81 `IlRxMsgHndCGW_02_200ms` = `3`
- L82 `IlRxMsgHndCGW_01_200ms` = `4`
- L83 `IlTxMsgHndDPSS_02_200ms` = `0`
- L84 `IlTxMsgHndIPSU_14_200ms` = `1`
- L85 `IlTxMsgHndIPSU_13_200ms` = `2`
- L86 `IlTxMsgHndIPSU_12_200ms` = `3`
- L87 `IlTxMsgHndIPSU_11_200ms` = `4`
- L88 `IlTxMsgHndIPSU_10_200ms` = `5`
- L89 `IlTxMsgHndIPSU_09_200ms` = `6`
- L90 `IlTxMsgHndIPSU_08_200ms` = `7`
- L91 `IlTxMsgHndIPSU_07_200ms` = `8`
- L92 `IlTxMsgHndIPSU_06_200ms` = `9`
- L93 `IlTxMsgHndIPSU_05_200ms` = `10`
- L94 `IlTxMsgHndIPSU_04_200ms` = `11`
- L95 `IlTxMsgHndIPSU_03_200ms` = `12`
- L96 `IlTxMsgHndIPSU_02_200ms` = `13`
- L97 `IlTxMsgHndIPSU_01_200ms` = `14`
- L104 `IlRxSigHndCGW_Tilt_Min_VrtLmt_Value_Set` = `IlRxMsgHndCGW_05_200ms`
- L105 `IlRxSigHndCGW_Tilt_Max_VrtLmt_Value_Set` = `IlRxMsgHndCGW_05_200ms`
- L106 `IlRxSigHndCGW_Tilt_Min_VrtLmt_Value` = `IlRxMsgHndCGW_05_200ms`
- L107 `IlRxSigHndCGW_Tilt_Max_VrtLmt_Value` = `IlRxMsgHndCGW_05_200ms`
- L108 `IlRxSigHndCGW_Height_Min_VrtLmt_Value_Set` = `IlRxMsgHndCGW_04_200ms`
- L109 `IlRxSigHndCGW_Height_Max_VrtLmt_Value_Set` = `IlRxMsgHndCGW_04_200ms`
- L110 `IlRxSigHndCGW_Height_Min_VrtLmt_Value` = `IlRxMsgHndCGW_04_200ms`
- L111 `IlRxSigHndCGW_Height_Max_VrtLmt_Value` = `IlRxMsgHndCGW_04_200ms`
- L112 `IlRxSigHndCGW_Slide_Min_VrtLmt_Value_Set` = `IlRxMsgHndCGW_03_200ms`
- L113 `IlRxSigHndCGW_Slide_Max_VrtLmt_Value_Set` = `IlRxMsgHndCGW_03_200ms`
- L114 `IlRxSigHndCGW_Slide_Min_VrtLmt_Value` = `IlRxMsgHndCGW_03_200ms`
- L115 `IlRxSigHndCGW_Slide_Max_VrtLmt_Value` = `IlRxMsgHndCGW_03_200ms`
- L116 `IlRxSigHndCGW_Recline_Min_VrtLmt_Value_Set` = `IlRxMsgHndCGW_02_200ms`
- L117 `IlRxSigHndCGW_Recline_Max_VrtLmt_Value_Set` = `IlRxMsgHndCGW_02_200ms`
- L118 `IlRxSigHndCGW_Recline_Min_VrtLmt_Value` = `IlRxMsgHndCGW_02_200ms`
- L119 `IlRxSigHndCGW_Recline_Max_VrtLmt_Value` = `IlRxMsgHndCGW_02_200ms`
- L120 `IlRxSigHndCGW_Recline_VrtLmt_Clear` = `IlRxMsgHndCGW_01_200ms`
- L121 `IlRxSigHndCGW_Recline_Fwd_VrtLmt_Point_Set` = `IlRxMsgHndCGW_01_200ms`
- L122 `IlRxSigHndCGW_Recline_Bwd_VrtLmt_Point_Set` = `IlRxMsgHndCGW_01_200ms`
- L123 `IlRxSigHndCGW_Slide_VrtLmt_Clear` = `IlRxMsgHndCGW_01_200ms`
- L124 `IlRxSigHndCGW_Slide_Fwd_VrtLmt_Point_Set` = `IlRxMsgHndCGW_01_200ms`
- L125 `IlRxSigHndCGW_Slide_Bwd_VrtLmt_Point_Set` = `IlRxMsgHndCGW_01_200ms`
- L126 `IlRxSigHndCGW_Height_VrtLmt_Clear` = `IlRxMsgHndCGW_01_200ms`
- L127 `IlRxSigHndCGW_Height_Fwd_VrtLmt_Point_Set` = `IlRxMsgHndCGW_01_200ms`
- L128 `IlRxSigHndCGW_Height_Bwd_VrtLmt_Point_Set` = `IlRxMsgHndCGW_01_200ms`
- L129 `IlRxSigHndCGW_Tilt_VrtLmt_Clear` = `IlRxMsgHndCGW_01_200ms`
- L130 `IlRxSigHndCGW_Tilt_Fwd_VrtLmt_Point_Set` = `IlRxMsgHndCGW_01_200ms`
- L131 `IlRxSigHndCGW_Tilt_Bwd_VrtLmt_Point_Set` = `IlRxMsgHndCGW_01_200ms`
- L132 `IlTxSigHndDPSS_Crc2Val` = `IlTxMsgHndDPSS_02_200ms`
- L133 `IlTxSigHndDPSS_AlvCnt2Val` = `IlTxMsgHndDPSS_02_200ms`
- L134 `IlTxSigHndDPSS_LumbarUpSw` = `IlTxMsgHndDPSS_02_200ms`
- L135 `IlTxSigHndDPSS_LumbarMidSw` = `IlTxMsgHndDPSS_02_200ms`
- L136 `IlTxSigHndDPSS_LumbarLowSw` = `IlTxMsgHndDPSS_02_200ms`
- L137 `IlTxSigHndDPSS_LumbarDefSw` = `IlTxMsgHndDPSS_02_200ms`
- L138 `IlTxSigHndDPSS_BolsterInfSw` = `IlTxMsgHndDPSS_02_200ms`
- L139 `IlTxSigHndDPSS_BolsterDefSw` = `IlTxMsgHndDPSS_02_200ms`
- L140 `IlTxSigHndDPSS_ActiveSw` = `IlTxMsgHndDPSS_02_200ms`
- L141 `IlTxSigHndDPSS_CushInfSw` = `IlTxMsgHndDPSS_02_200ms`
- L142 `IlTxSigHndDPSS_CushDefSw` = `IlTxMsgHndDPSS_02_200ms`
- L143 `IlTxSigHndIPSU_Recline_VrtLmt_Set_Sta` = `IlTxMsgHndIPSU_14_200ms`
- L144 `IlTxSigHndIPSU_Slide_VrtLmt_Set_Sta` = `IlTxMsgHndIPSU_14_200ms`
- L145 `IlTxSigHndIPSU_Height_VrtLmt_Set_Sta` = `IlTxMsgHndIPSU_14_200ms`
- L146 `IlTxSigHndIPSU_Tilt_VrtLmt_Set_Sta` = `IlTxMsgHndIPSU_14_200ms`
- L147 `IlTxSigHndIPSU_Footrest_Current_Sta` = `IlTxMsgHndIPSU_13_200ms`
- L148 `IlTxSigHndIPSU_Tilt_Min_VrtLmt_Value` = `IlTxMsgHndIPSU_12_200ms`
- L149 `IlTxSigHndIPSU_Tilt_Max_VrtLmt_Value` = `IlTxMsgHndIPSU_12_200ms`
- L150 `IlTxSigHndIPSU_Tilt_Min_Pos` = `IlTxMsgHndIPSU_11_200ms`
- L151 `IlTxSigHndIPSU_Tilt_Max_Pos` = `IlTxMsgHndIPSU_11_200ms`
- L152 `IlTxSigHndIPSU_Tilt_Current_Pos` = `IlTxMsgHndIPSU_10_200ms`
- L153 `IlTxSigHndIPSU_Tilt_Target_Pos` = `IlTxMsgHndIPSU_10_200ms`
- L154 `IlTxSigHndIPSU_Height_Min_VrtLmt_Value` = `IlTxMsgHndIPSU_09_200ms`
- L155 `IlTxSigHndIPSU_Height_Max_VrtLmt_Value` = `IlTxMsgHndIPSU_09_200ms`
- L156 `IlTxSigHndIPSU_Height_Min_Pos` = `IlTxMsgHndIPSU_08_200ms`
- L157 `IlTxSigHndIPSU_Height_Max_Pos` = `IlTxMsgHndIPSU_08_200ms`
- L158 `IlTxSigHndIPSU_Height_Current_Pos` = `IlTxMsgHndIPSU_07_200ms`
- L159 `IlTxSigHndIPSU_Height_Target_Pos` = `IlTxMsgHndIPSU_07_200ms`
- L160 `IlTxSigHndIPSU_Slide_Min_VrtLmt_Value` = `IlTxMsgHndIPSU_06_200ms`
- L161 `IlTxSigHndIPSU_Slide_Max_VrtLmt_Value` = `IlTxMsgHndIPSU_06_200ms`
- L162 `IlTxSigHndIPSU_Slide_Min_Pos` = `IlTxMsgHndIPSU_05_200ms`
- L163 `IlTxSigHndIPSU_Slide_Max_Pos` = `IlTxMsgHndIPSU_05_200ms`
- L164 `IlTxSigHndIPSU_Slide_Current_Pos` = `IlTxMsgHndIPSU_04_200ms`
- L165 `IlTxSigHndIPSU_Slide_Target_Pos` = `IlTxMsgHndIPSU_04_200ms`
- L166 `IlTxSigHndIPSU_Recline_Min_VrtLmt_Value` = `IlTxMsgHndIPSU_03_200ms`
- L167 `IlTxSigHndIPSU_Recline_Max_VrtLmt_Value` = `IlTxMsgHndIPSU_03_200ms`
- L168 `IlTxSigHndIPSU_Recline_Min_Pos` = `IlTxMsgHndIPSU_02_200ms`
- L169 `IlTxSigHndIPSU_Recline_Max_Pos` = `IlTxMsgHndIPSU_02_200ms`
- L170 `IlTxSigHndIPSU_Recline_Current_Pos` = `IlTxMsgHndIPSU_01_200ms`
- L171 `IlTxSigHndIPSU_Recline_Target_Pos` = `IlTxMsgHndIPSU_01_200ms`
- L178 `IlEnterCriticalDPSS_Crc2Val()` = `CanGlobalInterruptDisable()`
- L179 `IlLeaveCriticalDPSS_Crc2Val()` = `CanGlobalInterruptRestore()`
- L180 `IlEnterCriticalDPSS_AlvCnt2Val()` = `CanGlobalInterruptDisable()`
- L181 `IlLeaveCriticalDPSS_AlvCnt2Val()` = `CanGlobalInterruptRestore()`
- L182 `IlEnterCriticalDPSS_LumbarUpSw()` = `CanGlobalInterruptDisable()`
- L183 `IlLeaveCriticalDPSS_LumbarUpSw()` = `CanGlobalInterruptRestore()`
- L184 `IlEnterCriticalDPSS_LumbarMidSw()` = `CanGlobalInterruptDisable()`
- L185 `IlLeaveCriticalDPSS_LumbarMidSw()` = `CanGlobalInterruptRestore()`
- L186 `IlEnterCriticalDPSS_LumbarLowSw()` = `CanGlobalInterruptDisable()`
- L187 `IlLeaveCriticalDPSS_LumbarLowSw()` = `CanGlobalInterruptRestore()`
- L188 `IlEnterCriticalDPSS_LumbarDefSw()` = `CanGlobalInterruptDisable()`
- L189 `IlLeaveCriticalDPSS_LumbarDefSw()` = `CanGlobalInterruptRestore()`
- L190 `IlEnterCriticalDPSS_BolsterInfSw()` = `CanGlobalInterruptDisable()`
- L191 `IlLeaveCriticalDPSS_BolsterInfSw()` = `CanGlobalInterruptRestore()`
- L192 `IlEnterCriticalDPSS_BolsterDefSw()` = `CanGlobalInterruptDisable()`
- L193 `IlLeaveCriticalDPSS_BolsterDefSw()` = `CanGlobalInterruptRestore()`
- L194 `IlEnterCriticalDPSS_ActiveSw()` = `CanGlobalInterruptDisable()`
- L195 `IlLeaveCriticalDPSS_ActiveSw()` = `CanGlobalInterruptRestore()`
- L196 `IlEnterCriticalDPSS_CushInfSw()` = `CanGlobalInterruptDisable()`
- L197 `IlLeaveCriticalDPSS_CushInfSw()` = `CanGlobalInterruptRestore()`
- L198 `IlEnterCriticalDPSS_CushDefSw()` = `CanGlobalInterruptDisable()`
- L199 `IlLeaveCriticalDPSS_CushDefSw()` = `CanGlobalInterruptRestore()`
- L200 `IlEnterCriticalIPSU_Recline_VrtLmt_Set_Sta()` = `CanGlobalInterruptDisable()`
- L201 `IlLeaveCriticalIPSU_Recline_VrtLmt_Set_Sta()` = `CanGlobalInterruptRestore()`
- L202 `IlEnterCriticalIPSU_Slide_VrtLmt_Set_Sta()` = `CanGlobalInterruptDisable()`
- L203 `IlLeaveCriticalIPSU_Slide_VrtLmt_Set_Sta()` = `CanGlobalInterruptRestore()`
- L204 `IlEnterCriticalIPSU_Height_VrtLmt_Set_Sta()` = `CanGlobalInterruptDisable()`
- L205 `IlLeaveCriticalIPSU_Height_VrtLmt_Set_Sta()` = `CanGlobalInterruptRestore()`
- L206 `IlEnterCriticalIPSU_Tilt_VrtLmt_Set_Sta()` = `CanGlobalInterruptDisable()`
- L207 `IlLeaveCriticalIPSU_Tilt_VrtLmt_Set_Sta()` = `CanGlobalInterruptRestore()`
- L208 `IlEnterCriticalIPSU_Footrest_Current_Sta()` = `CanGlobalInterruptDisable()`
- ... 생략 126개 (동일 패턴의 생성 심볼)

#### 주요 typedef/구조체
- L53 `typedef vuint8 IltTxCounter;`
- L57 `typedef vuint8 IltTxUpdateCounter;`
- L61 `typedef vuint8 IltTxTimeoutCounter;`
- L65 `typedef vuint8 IltRxTimeoutCounter;`
- L69 `typedef vuint8 IltTxRepetitionCounter;`

#### 주요 전역 변수/테이블 선언
- 없음

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 72

- L345 `IlPrivateGetTxIPSU_Tilt_Min_VrtLmt_Value(void)` [선언]
- L350 `IlPrivateGetTxIPSU_Tilt_Max_VrtLmt_Value(void)` [선언]
- L355 `IlPrivateGetTxIPSU_Tilt_Min_Pos(void)` [선언]
- L360 `IlPrivateGetTxIPSU_Tilt_Max_Pos(void)` [선언]
- L365 `IlPrivateGetTxIPSU_Tilt_Current_Pos(void)` [선언]
- L370 `IlPrivateGetTxIPSU_Tilt_Target_Pos(void)` [선언]
- L375 `IlPrivateGetTxIPSU_Height_Min_VrtLmt_Value(void)` [선언]
- L380 `IlPrivateGetTxIPSU_Height_Max_VrtLmt_Value(void)` [선언]
- L385 `IlPrivateGetTxIPSU_Height_Min_Pos(void)` [선언]
- L390 `IlPrivateGetTxIPSU_Height_Max_Pos(void)` [선언]
- L395 `IlPrivateGetTxIPSU_Height_Current_Pos(void)` [선언]
- L400 `IlPrivateGetTxIPSU_Height_Target_Pos(void)` [선언]
- L405 `IlPrivateGetTxIPSU_Slide_Min_VrtLmt_Value(void)` [선언]
- L410 `IlPrivateGetTxIPSU_Slide_Max_VrtLmt_Value(void)` [선언]
- L415 `IlPrivateGetTxIPSU_Slide_Min_Pos(void)` [선언]
- L420 `IlPrivateGetTxIPSU_Slide_Max_Pos(void)` [선언]
- L425 `IlPrivateGetTxIPSU_Slide_Current_Pos(void)` [선언]
- L430 `IlPrivateGetTxIPSU_Slide_Target_Pos(void)` [선언]
- L435 `IlPrivateGetTxIPSU_Recline_Min_VrtLmt_Value(void)` [선언]
- L440 `IlPrivateGetTxIPSU_Recline_Max_VrtLmt_Value(void)` [선언]
- L445 `IlPrivateGetTxIPSU_Recline_Min_Pos(void)` [선언]
- L450 `IlPrivateGetTxIPSU_Recline_Max_Pos(void)` [선언]
- L455 `IlPrivateGetTxIPSU_Recline_Current_Pos(void)` [선언]
- L460 `IlPrivateGetTxIPSU_Recline_Target_Pos(void)` [선언]
- L577 `IlGetRxCGW_Tilt_Min_VrtLmt_Value(void)` [선언]
- L582 `IlGetRxCGW_Tilt_Max_VrtLmt_Value(void)` [선언]
- L587 `IlGetRxCGW_Height_Min_VrtLmt_Value(void)` [선언]
- L592 `IlGetRxCGW_Height_Max_VrtLmt_Value(void)` [선언]
- L597 `IlGetRxCGW_Slide_Min_VrtLmt_Value(void)` [선언]
- L602 `IlGetRxCGW_Slide_Max_VrtLmt_Value(void)` [선언]
- L607 `IlGetRxCGW_Recline_Min_VrtLmt_Value(void)` [선언]
- L612 `IlGetRxCGW_Recline_Max_VrtLmt_Value(void)` [선언]
- L623 `IlPutTxDPSS_Crc2Val(vuint8 sigData)` [선언]
- L628 `IlPutTxDPSS_AlvCnt2Val(vuint8 sigData)` [선언]
- L633 `IlPutTxDPSS_LumbarUpSw(vuint8 sigData)` [선언]
- L638 `IlPutTxDPSS_LumbarMidSw(vuint8 sigData)` [선언]
- L643 `IlPutTxDPSS_LumbarLowSw(vuint8 sigData)` [선언]
- L648 `IlPutTxDPSS_LumbarDefSw(vuint8 sigData)` [선언]
- L653 `IlPutTxDPSS_BolsterInfSw(vuint8 sigData)` [선언]
- L658 `IlPutTxDPSS_BolsterDefSw(vuint8 sigData)` [선언]
- L663 `IlPutTxDPSS_ActiveSw(vuint8 sigData)` [선언]
- L668 `IlPutTxDPSS_CushInfSw(vuint8 sigData)` [선언]
- L673 `IlPutTxDPSS_CushDefSw(vuint8 sigData)` [선언]
- L678 `IlPutTxIPSU_Recline_VrtLmt_Set_Sta(vuint8 sigData)` [선언]
- L683 `IlPutTxIPSU_Slide_VrtLmt_Set_Sta(vuint8 sigData)` [선언]
- L688 `IlPutTxIPSU_Height_VrtLmt_Set_Sta(vuint8 sigData)` [선언]
- L693 `IlPutTxIPSU_Tilt_VrtLmt_Set_Sta(vuint8 sigData)` [선언]
- L698 `IlPutTxIPSU_Footrest_Current_Sta(vuint8 sigData)` [선언]
- L703 `IlPutTxIPSU_Tilt_Min_VrtLmt_Value(vuint16 sigData)` [선언]
- L708 `IlPutTxIPSU_Tilt_Max_VrtLmt_Value(vuint16 sigData)` [선언]
- L713 `IlPutTxIPSU_Tilt_Min_Pos(vuint32 sigData)` [선언]
- L718 `IlPutTxIPSU_Tilt_Max_Pos(vuint32 sigData)` [선언]
- L723 `IlPutTxIPSU_Tilt_Current_Pos(vuint32 sigData)` [선언]
- L728 `IlPutTxIPSU_Tilt_Target_Pos(vuint32 sigData)` [선언]
- L733 `IlPutTxIPSU_Height_Min_VrtLmt_Value(vuint16 sigData)` [선언]
- L738 `IlPutTxIPSU_Height_Max_VrtLmt_Value(vuint16 sigData)` [선언]
- L743 `IlPutTxIPSU_Height_Min_Pos(vuint32 sigData)` [선언]
- L748 `IlPutTxIPSU_Height_Max_Pos(vuint32 sigData)` [선언]
- L753 `IlPutTxIPSU_Height_Current_Pos(vuint32 sigData)` [선언]
- L758 `IlPutTxIPSU_Height_Target_Pos(vuint32 sigData)` [선언]
- L763 `IlPutTxIPSU_Slide_Min_VrtLmt_Value(vuint16 sigData)` [선언]
- L768 `IlPutTxIPSU_Slide_Max_VrtLmt_Value(vuint16 sigData)` [선언]
- L773 `IlPutTxIPSU_Slide_Min_Pos(vuint32 sigData)` [선언]
- L778 `IlPutTxIPSU_Slide_Max_Pos(vuint32 sigData)` [선언]
- L783 `IlPutTxIPSU_Slide_Current_Pos(vuint32 sigData)` [선언]
- L788 `IlPutTxIPSU_Slide_Target_Pos(vuint32 sigData)` [선언]
- L793 `IlPutTxIPSU_Recline_Min_VrtLmt_Value(vuint16 sigData)` [선언]
- L798 `IlPutTxIPSU_Recline_Max_VrtLmt_Value(vuint16 sigData)` [선언]
- L803 `IlPutTxIPSU_Recline_Min_Pos(vuint32 sigData)` [선언]
- L808 `IlPutTxIPSU_Recline_Max_Pos(vuint32 sigData)` [선언]
- L813 `IlPutTxIPSU_Recline_Current_Pos(vuint32 sigData)` [선언]
- L818 `IlPutTxIPSU_Recline_Target_Pos(vuint32 sigData)` [선언]

### 4. 각 함수별 역할 설명

- `IlPrivateGetTxIPSU_Tilt_Min_VrtLmt_Value` (L345, 선언): Handle:   16,Name:     IPSU_Tilt_Min_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPrivateGetTxIPSU_Tilt_Max_VrtLmt_Value` (L350, 선언): Handle:   17,Name:     IPSU_Tilt_Max_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPrivateGetTxIPSU_Tilt_Min_Pos` (L355, 선언): Handle:   18,Name:              IPSU_Tilt_Min_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Tilt_Max_Pos` (L360, 선언): Handle:   19,Name:              IPSU_Tilt_Max_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Tilt_Current_Pos` (L365, 선언): Handle:   20,Name:          IPSU_Tilt_Current_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Tilt_Target_Pos` (L370, 선언): Handle:   21,Name:           IPSU_Tilt_Target_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Height_Min_VrtLmt_Value` (L375, 선언): Handle:   22,Name:   IPSU_Height_Min_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPrivateGetTxIPSU_Height_Max_VrtLmt_Value` (L380, 선언): Handle:   23,Name:   IPSU_Height_Max_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPrivateGetTxIPSU_Height_Min_Pos` (L385, 선언): Handle:   24,Name:            IPSU_Height_Min_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Height_Max_Pos` (L390, 선언): Handle:   25,Name:            IPSU_Height_Max_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Height_Current_Pos` (L395, 선언): Handle:   26,Name:        IPSU_Height_Current_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Height_Target_Pos` (L400, 선언): Handle:   27,Name:         IPSU_Height_Target_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Slide_Min_VrtLmt_Value` (L405, 선언): Handle:   28,Name:    IPSU_Slide_Min_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPrivateGetTxIPSU_Slide_Max_VrtLmt_Value` (L410, 선언): Handle:   29,Name:    IPSU_Slide_Max_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPrivateGetTxIPSU_Slide_Min_Pos` (L415, 선언): Handle:   30,Name:             IPSU_Slide_Min_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Slide_Max_Pos` (L420, 선언): Handle:   31,Name:             IPSU_Slide_Max_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Slide_Current_Pos` (L425, 선언): Handle:   32,Name:         IPSU_Slide_Current_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Slide_Target_Pos` (L430, 선언): Handle:   33,Name:          IPSU_Slide_Target_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Recline_Min_VrtLmt_Value` (L435, 선언): Handle:   34,Name:  IPSU_Recline_Min_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPrivateGetTxIPSU_Recline_Max_VrtLmt_Value` (L440, 선언): Handle:   35,Name:  IPSU_Recline_Max_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPrivateGetTxIPSU_Recline_Min_Pos` (L445, 선언): Handle:   36,Name:           IPSU_Recline_Min_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Recline_Max_Pos` (L450, 선언): Handle:   37,Name:           IPSU_Recline_Max_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Recline_Current_Pos` (L455, 선언): Handle:   38,Name:       IPSU_Recline_Current_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPrivateGetTxIPSU_Recline_Target_Pos` (L460, 선언): Handle:   39,Name:        IPSU_Recline_Target_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlGetRxCGW_Tilt_Min_VrtLmt_Value` (L577, 선언): Handle:    2,Name:      CGW_Tilt_Min_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlGetRxCGW_Tilt_Max_VrtLmt_Value` (L582, 선언): Handle:    3,Name:      CGW_Tilt_Max_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlGetRxCGW_Height_Min_VrtLmt_Value` (L587, 선언): Handle:    6,Name:    CGW_Height_Min_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlGetRxCGW_Height_Max_VrtLmt_Value` (L592, 선언): Handle:    7,Name:    CGW_Height_Max_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlGetRxCGW_Slide_Min_VrtLmt_Value` (L597, 선언): Handle:   10,Name:     CGW_Slide_Min_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlGetRxCGW_Slide_Max_VrtLmt_Value` (L602, 선언): Handle:   11,Name:     CGW_Slide_Max_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlGetRxCGW_Recline_Min_VrtLmt_Value` (L607, 선언): Handle:   14,Name:   CGW_Recline_Min_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlGetRxCGW_Recline_Max_VrtLmt_Value` (L612, 선언): Handle:   15,Name:   CGW_Recline_Max_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPutTxDPSS_Crc2Val` (L623, 선언): Handle:    0,Name:                   DPSS_Crc2Val,Size:  8,UsedBytes:  1,SingleSignal
- `IlPutTxDPSS_AlvCnt2Val` (L628, 선언): Handle:    1,Name:                DPSS_AlvCnt2Val,Size:  4,UsedBytes:  1,SingleSignal
- `IlPutTxDPSS_LumbarUpSw` (L633, 선언): Handle:    2,Name:                DPSS_LumbarUpSw,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxDPSS_LumbarMidSw` (L638, 선언): Handle:    3,Name:               DPSS_LumbarMidSw,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxDPSS_LumbarLowSw` (L643, 선언): Handle:    4,Name:               DPSS_LumbarLowSw,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxDPSS_LumbarDefSw` (L648, 선언): Handle:    5,Name:               DPSS_LumbarDefSw,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxDPSS_BolsterInfSw` (L653, 선언): Handle:    6,Name:              DPSS_BolsterInfSw,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxDPSS_BolsterDefSw` (L658, 선언): Handle:    7,Name:              DPSS_BolsterDefSw,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxDPSS_ActiveSw` (L663, 선언): Handle:    8,Name:                  DPSS_ActiveSw,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxDPSS_CushInfSw` (L668, 선언): Handle:    9,Name:                 DPSS_CushInfSw,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxDPSS_CushDefSw` (L673, 선언): Handle:   10,Name:                 DPSS_CushDefSw,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxIPSU_Recline_VrtLmt_Set_Sta` (L678, 선언): Handle:   11,Name:    IPSU_Recline_VrtLmt_Set_Sta,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxIPSU_Slide_VrtLmt_Set_Sta` (L683, 선언): Handle:   12,Name:      IPSU_Slide_VrtLmt_Set_Sta,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxIPSU_Height_VrtLmt_Set_Sta` (L688, 선언): Handle:   13,Name:     IPSU_Height_VrtLmt_Set_Sta,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxIPSU_Tilt_VrtLmt_Set_Sta` (L693, 선언): Handle:   14,Name:       IPSU_Tilt_VrtLmt_Set_Sta,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxIPSU_Footrest_Current_Sta` (L698, 선언): Handle:   15,Name:      IPSU_Footrest_Current_Sta,Size:  2,UsedBytes:  1,SingleSignal
- `IlPutTxIPSU_Tilt_Min_VrtLmt_Value` (L703, 선언): Handle:   16,Name:     IPSU_Tilt_Min_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPutTxIPSU_Tilt_Max_VrtLmt_Value` (L708, 선언): Handle:   17,Name:     IPSU_Tilt_Max_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPutTxIPSU_Tilt_Min_Pos` (L713, 선언): Handle:   18,Name:              IPSU_Tilt_Min_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Tilt_Max_Pos` (L718, 선언): Handle:   19,Name:              IPSU_Tilt_Max_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Tilt_Current_Pos` (L723, 선언): Handle:   20,Name:          IPSU_Tilt_Current_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Tilt_Target_Pos` (L728, 선언): Handle:   21,Name:           IPSU_Tilt_Target_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Height_Min_VrtLmt_Value` (L733, 선언): Handle:   22,Name:   IPSU_Height_Min_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPutTxIPSU_Height_Max_VrtLmt_Value` (L738, 선언): Handle:   23,Name:   IPSU_Height_Max_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPutTxIPSU_Height_Min_Pos` (L743, 선언): Handle:   24,Name:            IPSU_Height_Min_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Height_Max_Pos` (L748, 선언): Handle:   25,Name:            IPSU_Height_Max_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Height_Current_Pos` (L753, 선언): Handle:   26,Name:        IPSU_Height_Current_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Height_Target_Pos` (L758, 선언): Handle:   27,Name:         IPSU_Height_Target_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Slide_Min_VrtLmt_Value` (L763, 선언): Handle:   28,Name:    IPSU_Slide_Min_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPutTxIPSU_Slide_Max_VrtLmt_Value` (L768, 선언): Handle:   29,Name:    IPSU_Slide_Max_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPutTxIPSU_Slide_Min_Pos` (L773, 선언): Handle:   30,Name:             IPSU_Slide_Min_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Slide_Max_Pos` (L778, 선언): Handle:   31,Name:             IPSU_Slide_Max_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Slide_Current_Pos` (L783, 선언): Handle:   32,Name:         IPSU_Slide_Current_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Slide_Target_Pos` (L788, 선언): Handle:   33,Name:          IPSU_Slide_Target_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Recline_Min_VrtLmt_Value` (L793, 선언): Handle:   34,Name:  IPSU_Recline_Min_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPutTxIPSU_Recline_Max_VrtLmt_Value` (L798, 선언): Handle:   35,Name:  IPSU_Recline_Max_VrtLmt_Value,Size: 16,UsedBytes:  2,SingleSignal
- `IlPutTxIPSU_Recline_Min_Pos` (L803, 선언): Handle:   36,Name:           IPSU_Recline_Min_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Recline_Max_Pos` (L808, 선언): Handle:   37,Name:           IPSU_Recline_Max_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Recline_Current_Pos` (L813, 선언): Handle:   38,Name:       IPSU_Recline_Current_Pos,Size: 32,UsedBytes:  4,SingleSignal
- `IlPutTxIPSU_Recline_Target_Pos` (L818, 선언): Handle:   39,Name:        IPSU_Recline_Target_Pos,Size: 32,UsedBytes:  4,SingleSignal

### 5. 필요시 코드 부연 설명

이 파일은 생성 설정/선언 성격이 강해 별도 코드 전문 인용 없이 심볼 표로 역할을 설명했다.

## 파일이름 : nmb_cfg

- 경로: `src/CBD/Gen/nmb_cfg.h`
- 파일 종류: `.h`
- 파일 크기: 3,618 bytes
- 파일 내용: Basic NM 채널 수, 복구 타이밍, 옵션 스위치를 정의한다.

### 1. .c/.h 파일 이름

- `nmb_cfg.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 23
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 0

#### 주요 매크로
- L46 `__NMB_CFG_H__`
- L49 `__NMBASIC_CFG_H__`
- L51 `NM_BASICDLL_VERSION` = `0x0104`
- L52 `NM_BASICDLL_RELEASE_VERSION` = `0x00`
- L53 `cNmBasicNrOfChannels` = `1`
- L55 `NM_TYPE_BASIC`
- L57 `NMBASIC_DISABLE_SOFTWARE_CHECK`
- L58 `NMBASIC_BUSOFF_RECOV_EXTENDED`
- L59 `NMBASIC_DISABLE_BUSOFFREP_TIMER`
- L60 `NMBASIC_ENABLE_BUSOFFREP_MSG`
- L62 `NMBASIC_DISABLE_INDEXED_NM`
- L63 `NMBASIC_DISABLE_TX_OBSERVATION`
- L64 `NMBASIC_DISABLE_EXTERNAL_CANONLINE_HANDLING`
- L65 `NMBASIC_DISABLE_EARLY_BUSOFF_REINIT`
- L66 `NMBASIC_DISABLE_SET_CONTEXT`
- L67 `NMBASIC_DISABLE_GET_CONTEXT`
- L69 `cNmBasicInitObject` = `0`
- L70 `cNmBasicTaskPeriod` = `10`
- L71 `cNmBasicBusOffRecTime` = `50`
- L72 `cNmBasicBusOffRecTimeSlow` = `100`
- L73 `cNmBasicBusOffChangeFastToSlow` = `450`
- L74 `cNmBasicBusOffRepairedTime` = `2000`
- L83 `MAGIC_NUMBER` = `538736488`

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

## 파일이름 : nmb_par

- 경로: `src/CBD/Gen/nmb_par.c`
- 파일 종류: `.c`
- 파일 크기: 2,611 bytes
- 파일 내용: Basic NM 생성 파라미터와 버스오프 관련 설정 테이블을 정의한다.

### 1. .c/.h 파일 이름

- `nmb_par.c`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 0
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 0

#### 주요 매크로
- 없음

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

## 파일이름 : nmb_par

- 경로: `src/CBD/Gen/nmb_par.h`
- 파일 종류: `.h`
- 파일 크기: 2,543 bytes
- 파일 내용: Basic NM 생성 파라미터와 버스오프 관련 설정 테이블을 정의한다.

### 1. .c/.h 파일 이름

- `nmb_par.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 2
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 0

#### 주요 매크로
- L46 `__NMB_PAR_H__`
- L55 `MAGIC_NUMBER` = `538736488`

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

## 파일이름 : tp_cfg

- 경로: `src/CBD/Gen/tp_cfg.h`
- 파일 종류: `.h`
- 파일 크기: 10,899 bytes
- 파일 내용: TP 주소 방식, 타임아웃, padding, CANdesc 콜백 연결을 정의한다.

### 1. .c/.h 파일 이름

- `tp_cfg.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 126
- typedef/struct/enum 후보 수: 2
- 전역 변수/테이블 선언 후보 수: 0

#### 주요 매크로
- L46 `__TP_CFG_H__`
- L49 `TP_MEMORY_MODEL_DATA`
- L51 `TP_ENABLE_SINGLE_MSG_OBJ`
- L52 `TP_ISO15765DLL_VERSION` = `0x0233`
- L53 `TP_ISO15765DLL_RELEASE_VERSION` = `0x00`
- L55 `kTpNumberOfCanChannels` = `1`
- L57 `kTpOn` = `1`
- L61 `kTpOff` = `0`
- L64 `TP_TYPE_SINGLE_NORMAL_ADDRESSING_MULTI_BASED`
- L75 `TP_USE_ISO_COMPLIANCE` = `kTpOn`
- L76 `TP_USE_FAST_PRECOPY` = `kTpOff`
- L77 `TP_USE_DIAGPRECOPY` = `kTpOff`
- L78 `TP_CAN_CHANNEL_INDEX` = `0`
- L79 `TP_ENABLE_ISO_15765_2_2`
- L80 `TP_DISABLE_ASR_SINGLE_TP`
- L81 `TP_DISABLE_OBD_PRECOPY`
- L87 `TP_USE_EXTENDED_API_BS` = `kTpOff`
- L88 `TP_USE_EXTENDED_API_STMIN` = `kTpOff`
- L89 `TP_USE_TX_OF_FC_IN_ISR` = `kTpOn`
- L90 `TP_USE_OVERRUN_INDICATION` = `kTpOff`
- L91 `TP_USE_ONLY_FIRST_FC` = `kTpOn`
- L92 `TP_USE_FAST_TX_TRANSMISSION` = `kTpOff`
- L93 `TP_USE_QUEUE_IN_ISR` = `kTpOn`
- L94 `TP_USE_NO_STMIN_AFTER_FC` = `kTpOff`
- L95 `MEMORY_NEAR_TP_SAVE` = `MEMORY_NEAR`
- L96 `TP_USE_TX_HANDLE_CHANGEABLE` = `kTpOff`
- L97 `TP_USE_GATEWAY_API` = `kTpOff`
- L98 `TP_USE_RX_CHANNEL_WITHOUT_FC` = `kTpOff`
- L99 `TP_USE_TX_CHANNEL_WITHOUT_FC` = `kTpOff`
- L100 `TP_USE_OLD_STMIN_CALCULATION` = `kTpOff`
- L101 `TP_USE_VARIABLE_RX_DLC_CHECK` = `kTpOff`
- L102 `TP_USE_FIX_RX_DLC_CHECK` = `kTpOff`
- L103 `TP_USE_PADDING` = `kTpOn`
- L104 `TP_PADDING_PATTERN` = `0xAA`
- L105 `TP_USE_VARIABLE_DLC` = `kTpOff`
- L106 `TP_DISABLE_IGNORE_FC_RES_STMIN`
- L107 `TP_DISABLE_IGNORE_FC_OVFL`
- L108 `TP_DISABLE_WAIT_FOR_CORRECT_SN`
- L109 `TP_DISABLE_SINGLE_CHAN_MULTICONN`
- L110 `TP_DISABLE_FC_WAIT`
- L111 `TP_USE_UNEXPECTED_FC_CANCELATION` = `kTpOff`
- L112 `TP_DISABLE_EXT_COPYFROMCAN_API`
- L119 `TP_USE_NORMAL_ADDRESSING` = `kTpOn`
- L120 `TP_USE_NORMAL_FIXED_ADDRESSING` = `kTpOff`
- L121 `TP_USE_EXTENDED_ADDRESSING` = `kTpOff`
- L122 `TP_USE_MIXED_11_ADDRESSING` = `kTpOff`
- L123 `TP_USE_MIXED_29_ADDRESSING` = `kTpOff`
- L124 `TP_ENABLE_SINGLE_CHANNEL_TP`
- L125 `TP_DISABLE_MULTIPLE_ADDRESSING`
- L126 `TP_DISABLE_MULTIPLE_NODES`
- L127 `TP_ENABLE_USER_CHECK`
- L128 `TP_ENABLE_INTERNAL_CHECK`
- L129 `TP_ENABLE_GEN_CHECK`
- L130 `TP_ENABLE_RUNTIME_CHECK`
- L137 `TP_USE_PRE_COPY_CHECK` = `kTpOff`
- L138 `TP_USE_APPL_PRECOPY` = `kTpOff`
- L139 `TP_USE_FREE_CHANNEL_SEARCH` = `kTpOff`
- L140 `TP_USE_CUSTOM_RX_MEMCPY` = `kTpOff`
- L141 `TP_DISABLE_FUNCTIONAL_FC_WAIT`
- L148 `TP_USE_STMIN_OF_FC` = `kTpOn`
- L149 `TP_USE_TP_TRANSMIT_QUEUE` = `kTpOff`
- L150 `TP_USE_DYN_ID` = `kTpOff`
- L151 `TP_USE_CUSTOM_TX_MEMCPY` = `kTpOn`
- L158 `TP_USE_WAIT_FRAMES` = `kTpOff`
- L159 `TP_USE_MULTIPLE_BASEADDRESS` = `kTpOff`
- L160 `TP_USE_PRE_DISPATCHED_MODE` = `kTpOff`
- L167 `TP_USE_EXT_IDS_FOR_NORMAL` = `kTpOff`
- L168 `TP_USE_MIXED_IDS_FOR_NORMAL` = `kTpOff`
- L175 `TP_DISABLE_MIN_TIMER`
- L176 `kTpTxChannelCount` = `1`
- L177 `kTpRxChannelCount` = `1`
- L179 `TpMemCpy` = `memcpy`
- L180 `TpTxCallCycle` = `10`
- L181 `TpRxCallCycle` = `10`
- L183 `kBSRequested` = `0`
- L184 `kTpTxConfirmationTimeout` = `11`
- L185 `kTpRxConfirmationTimeout` = `11`
- L186 `TpSTMin` = `3`
- L187 `TpTxTimeoutFC` = `17`
- L188 `TpRxTimeoutCF` = `17`
- L189 `TpTxTransmitCF` = `1`
- L190 `__ApplTpPreCopyCheckFunction(x)`
- L191 `__ApplTpPrecopyCheckFCFunctional(tpCurrentTargetAddress)`
- L192 `TP_DISABLE_CHECKTA_COMPATIBILITY`
- L199 `TP_FUNC_ENABLE_RECEPTION`
- L200 `TP_FUNC_ENABLE_NORMAL_ADDRESSING`
- L201 `TP_FUNC_DISABLE_NORMAL_FIXED_ADDRESSING`
- L202 `TP_FUNC_DISABLE_EXTENDED_ADDRESSING`
- L203 `TP_FUNC_DISABLE_MIXED_11_ADDRESSING`
- L204 `TP_FUNC_DISABLE_MIXED_29_ADDRESSING`
- L205 `TP_FUNC_DISABLE_APPL_PRECOPY`
- L206 `__ApplTpFuncGetBuffer` = `DescGetFuncBuffer`
- L208 `__ApplTpFuncIndication` = `DescFuncReqInd`
- L212 `TP_USE_TP_INDICATION` = `kTpOn`
- L213 `TP_USE_RX_ERROR_INDICATION` = `kTpOn`
- L214 `TP_USE_TP_CONFIRMATION` = `kTpOn`
- L215 `TP_USE_TP_RX_SF` = `kTpOff`
- L216 `TP_USE_TP_RX_FF` = `kTpOff`
- L217 `TP_USE_TP_RX_CF` = `kTpOff`
- L218 `TP_USE_TP_NOTIFY_TX` = `kTpOff`
- L219 `TP_USE_TP_CAN_MESSAGE_TRANSMITTED` = `kTpOff`
- L220 `TP_USE_TP_TX_FC` = `kTpOff`
- L221 `TP_USE_TP_RX_GET_BUFFER` = `kTpOn`
- L222 `TP_USE_TX_ERROR_INDICATION` = `kTpOn`
- L223 `TP_USE_TP_COPY_TO_CAN` = `kTpOn`
- L224 `TP_USE_TP_COPY_FROM_CAN` = `kTpOff`
- L225 `TP_USE_TP_RX_CAN_MESSAGE_TRANSMITTED` = `kTpOff`
- L226 `TP_USE_TP_TX_DELAY_FINISHED` = `kTpOff`
- L228 `kTpTxHandle` = `CanTxIPSU_GST`
- L230 `kTpTxData` = `IPSU_GST._c`
- L241 `__ApplTpRxIndication(tpChannel, datLen)` = `(DescPhysReqInd(datLen))`
- L242 `__ApplTpRxErrorIndication(tpChannel, errNo)` = `(DescRxErrorIndication(errNo))`
- L243 `__ApplTpTxConfirmation(tpChannel, state)` = `(DescConfirmation(state))`
- L244 `__ApplTpRxSF(tpChannel)`
- L245 `__ApplTpRxFF(tpChannel)`
- L246 `__ApplTpRxCF(tpChannel)`
- L247 `__ApplTpTxNotification(tpChannel, datLen)`
- L248 `__ApplTpTxCanMessageTransmitted(tpChannel)`
- L249 `__ApplTpTxFC(tpChannel)`
- L250 `__ApplTpRxGetBuffer(tpChannel, datLen)` = `(DescGetBuffer(datLen))`
- ... 생략 6개 (동일 패턴의 생성 심볼)

#### 주요 typedef/구조체
- L72 `typedef struct tTpCopyToCanInfoStruct_s tTpCopyToCanInfoStruct;`
- L73 `typedef struct tTpCopyToCanInfoStruct_s *TpCopyToCanInfoStructPtr;`

#### 주요 전역 변수/테이블 선언
- 없음

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 8

- L207 `DescGetFuncBuffer(vuint16 dataLength)` [선언]
- L209 `DescFuncReqInd(vuint16 dataLength)` [선언]
- L234 `DescPhysReqInd(canuint16 datLen)` [선언]
- L235 `DescRxErrorIndication(canuint8 errNo)` [선언]
- L236 `DescConfirmation(canuint8 state)` [선언]
- L237 `DescGetBuffer(canuint16 datLen)` [선언]
- L238 `DescTxErrorIndication(canuint8 errNo)` [선언]
- L239 `DescCopyToCAN(TpCopyToCanInfoStructPtr infoStruct)` [선언]

### 4. 각 함수별 역할 설명

- `DescGetFuncBuffer` (L207, 선언): ----------------------------------------------------------------------------- &&&~ -----------------------------------------------------------------------------
- `DescFuncReqInd` (L209, 선언): ----------------------------------------------------------------------------- &&&~ -----------------------------------------------------------------------------
- `DescPhysReqInd` (L234, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescRxErrorIndication` (L235, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `DescConfirmation` (L236, 선언): 송신 완료 통지나 확인 상태를 처리하고 상위 계층 콜백/플래그를 갱신한다.
- `DescGetBuffer` (L237, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.
- `DescTxErrorIndication` (L238, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `DescCopyToCAN` (L239, 선언): CANdesc 내부 UDS 진단 처리 함수로 요청 분석, 상태 관리, 응답 구성 중 일부를 담당한다.

### 5. 필요시 코드 부연 설명

이 파일은 생성 설정/선언 성격이 강해 별도 코드 전문 인용 없이 심볼 표로 역할을 설명했다.

## 파일이름 : tp_par

- 경로: `src/CBD/Gen/tp_par.c`
- 파일 종류: `.c`
- 파일 크기: 2,077 bytes
- 파일 내용: TP 생성 파라미터와 연결 테이블을 정의한다.

### 1. .c/.h 파일 이름

- `tp_par.c`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 0
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 0

#### 주요 매크로
- 없음

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

## 파일이름 : v_cfg

- 경로: `src/CBD/Gen/v_cfg.h`
- 파일 종류: `.h`
- 파일 크기: 7,974 bytes
- 파일 내용: Vector 스택 전체 활성 모듈, CPU/컴파일러/메모리 모델 설정을 정의한다.

### 1. .c/.h 파일 이름

- `v_cfg.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 67
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 0

#### 주요 매크로
- L46 `__V_CFG_H__`
- L49 `VGEN_GENY`
- L53 `GENy`
- L57 `SUPPLIER_CANBEDDED` = `30`
- L65 `VERSIONNUMBER` = `0x230802`
- L75 `V_OSTYPE_NONE`
- L85 `C_CPUTYPE_32BIT`
- L90 `V_CPUTYPE_BITARRAY_16BIT`
- L95 `C_CPUTYPE_BIGENDIAN`
- L100 `C_CPUTYPE_BITORDER_MSB2LSB`
- L105 `V_DISABLE_USE_DUMMY_FUNCTIONS`
- L110 `V_ENABLE_USE_DUMMY_STATEMENT`
- L115 `C_COMP_GHS_MPC55XX_FLEXCAN2`
- L120 `V_CPU_MPC55XX`
- L124 `V_COMP_GHS`
- L128 `V_COMP_GHS_MPC55XX`
- L132 `V_PROCESSOR_MPC5515`
- L137 `C_PROCESSOR_MPC5515`
- L147 `VGEN_ENABLE_DIAG_CANDESC_UDS`
- L148 `VGEN_ENABLE_CCL`
- L150 `VGEN_ENABLE_VSTDLIB`
- L154 `VSTD_ENABLE_DEFAULT_INTCTRL`
- L158 `VSTD_ENABLE_GLOBAL_LOCK`
- L162 `VSTD_DISABLE_DEBUG_SUPPORT`
- L166 `VSTD_ENABLE_LIBRARY_FUNCTIONS`
- L170 `VGEN_ENABLE_CAN_DRV`
- L171 `C_ENABLE_CAN_CHANNELS`
- L172 `V_BUSTYPE_CAN`
- L173 `VGEN_ENABLE_IL_VECTOR`
- L174 `VGEN_ENABLE_NM_BASIC`
- L175 `VGEN_ENABLE_TP_ISO_MC`
- L179 `kVNumberOfIdentities` = `1`
- L183 `tVIdentityMsk` = `vuint8`
- L187 `kVIdentityIdentity_0` = `(vuint8) 0`
- L191 `VSetActiveIdentity(identityLog)`
- L195 `V_ACTIVE_IDENTITY_MSK` = `1`
- L199 `V_ACTIVE_IDENTITY_LOG` = `0`
- L203 `DIAG_API_CALL_TYPE`
- L204 `DIAG_API_CALLBACK_TYPE`
- L205 `DIAG_INTERNAL_CALL_TYPE`
- L210 `CCL_API_CALL_TYPE`
- L211 `CCL_API_CALLBACK_TYPE`
- L212 `CCL_INTERNAL_CALL_TYPE`
- L215 `DRV_API_CALL_TYPE`
- L216 `DRV_API_CALLBACK_TYPE`
- L217 `TP_API_CALL_TYPE`
- L218 `TP_API_CALLBACK_TYPE`
- L219 `TP_INTERNAL_CALL_TYPE`
- L225 `VGEN_OEM_PRECONFIG_HMC_SLP6`
- L226 `VGEN_OEM_PRECONFIG_VERSION` = `0x0101`
- L227 `VGEN_OEM_PRECONFIG_RELEASE_VERSION` = `0x00`
- L228 `VGEN_USER_PRECONFIG_HMC_SLP5_HIGH_SPEED`
- L229 `VGEN_USER_PRECONFIG_VERSION` = `0x0000`
- L230 `VGEN_USER_PRECONFIG_RELEASE_VERSION` = `0x00`
- L237 `V_ATOMIC_BIT_ACCESS_IN_BITFIELD` = `STD_OFF`
- L238 `V_ATOMIC_VARIABLE_ACCESS` = `16`
- L244 `VGEN_ENABLE_VSTDLIB`
- L248 `C_CLIENT_HMC`
- L252 `__IPSU__`
- L259 `kComNumberOfNodes` = `kVNumberOfIdentities`
- L260 `ComSetCurrentECU` = `VSetActiveIdentity`
- L261 `comMultipleECUCurrent` = `vActiveIdentityLog`
- L264 `C_VERSION_REF_IMPLEMENTATION` = `0x150`
- L269 `VGEN_ENABLE_VSTDLIB`
- L273 `VGEN_ENABLE_VSTDLIB`
- L279 `VGEN_ENABLE_VSTDLIB`
- L291 `MAGIC_NUMBER` = `538736488`

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

## 파일이름 : v_inc

- 경로: `src/CBD/Gen/v_inc.h`
- 파일 종류: `.h`
- 파일 크기: 2,701 bytes
- 파일 내용: Vector 생성 설정과 각 모듈 헤더를 묶는 통합 include 헤더이다.

### 1. .c/.h 파일 이름

- `v_inc.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 2
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 0

#### 주요 매크로
- L46 `__V_INC_H__`
- L66 `MAGIC_NUMBER` = `538736488`

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

## 파일이름 : v_par

- 경로: `src/CBD/Gen/v_par.c`
- 파일 종류: `.c`
- 파일 크기: 3,669 bytes
- 파일 내용: Vector 전역 생성 파라미터와 DBC 버전 정보를 정의한다.

### 1. .c/.h 파일 이름

- `v_par.c`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 0
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 2

#### 주요 매크로
- 없음

#### 주요 typedef/구조체
- 없음

#### 주요 전역 변수/테이블 선언
- L59 `V_MEMROM0 V_MEMROM1 vuint8 V_MEMROM2 kGENyVersion[kGENyVersionNumberOfBytes] =`
- L77 `V_MEMROM0 V_MEMROM1 tDBCVersion V_MEMROM2 kDBCVersion[1] =`

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 0

- 없음

### 4. 각 함수별 역할 설명

- 함수 없음: 설정/타입/매크로 전용 파일이다.

### 5. 필요시 코드 부연 설명

이 파일은 생성 설정/선언 성격이 강해 별도 코드 전문 인용 없이 심볼 표로 역할을 설명했다.

## 파일이름 : v_par

- 경로: `src/CBD/Gen/v_par.h`
- 파일 종류: `.h`
- 파일 크기: 3,903 bytes
- 파일 내용: Vector 전역 생성 파라미터와 DBC 버전 정보를 정의한다.

### 1. .c/.h 파일 이름

- `v_par.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 13
- typedef/struct/enum 후보 수: 1
- 전역 변수/테이블 선언 후보 수: 7

#### 주요 매크로
- L46 `__V_PAR_H__`
- L60 `VGEN_DELIVERY_VERSION_BYTE_0` = `0x06`
- L61 `VGEN_DELIVERY_VERSION_BYTE_1` = `0x00`
- L62 `VGEN_DELIVERY_VERSION_BYTE_2` = `0x09`
- L63 `VGEN_DELIVERY_VERSION_BYTE_3` = `0x01`
- L64 `VGEN_DELIVERY_VERSION_BYTE_4` = `0x20`
- L65 `VGEN_DELIVERY_VERSION_BYTE_5` = `0x03`
- L66 `VGEN_DELIVERY_VERSION_BYTE_6` = `0x74`
- L67 `VGEN_DELIVERY_VERSION_BYTE_7` = `0x00`
- L68 `VGEN_DELIVERY_VERSION_BYTE_8` = `0x00`
- L69 `VGEN_DELIVERY_VERSION_BYTE_9` = `0x00`
- L70 `kGENyVersionNumberOfBytes` = `10`
- L94 `MAGIC_NUMBER` = `538736488`

#### 주요 typedef/구조체
- L77 `typedef struct tDBCVersionTag`

#### 주요 전역 변수/테이블 선언
- L72 `V_MEMROM0 extern  V_MEMROM1 vuint8 V_MEMROM2 kGENyVersion[kGENyVersionNumberOfBytes];`
- L79 `vuint8 kYear;`
- L80 `vuint8 kMonth;`
- L81 `vuint8 kWeek;`
- L82 `vuint8 kDay;`
- L83 `vuint32 kNumber;`
- L85 `V_MEMROM0 extern  V_MEMROM1 tDBCVersion V_MEMROM2 kDBCVersion[1];`

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 0

- 없음

### 4. 각 함수별 역할 설명

- 함수 없음: 설정/타입/매크로 전용 파일이다.

### 5. 필요시 코드 부연 설명

이 파일은 생성 설정/선언 성격이 강해 별도 코드 전문 인용 없이 심볼 표로 역할을 설명했다.
