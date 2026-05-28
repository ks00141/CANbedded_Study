# Tp 계층 파일별 상세 분석 보고서

- 분석 대상: `Tp` 계층
- 파일 수: 2
- 생성 시각: 2026-05-28 22:37:38
- 분석 방식: C 전처리 매크로, typedef, 전역 데이터 선언, 함수 선언/정의를 자동 추출하고 함수명/주석/계층 역할을 기준으로 한글 역할 설명을 부여함.

## 계층 요약

TP 계층은 ISO 15765-2 기반 진단 전송 계층으로 Single/First/Consecutive/FlowControl 프레임 상태 머신을 처리한다.

## 파일 목록

- `src/CBD/Tp/tpmc.c` (256,520 bytes)
- `src/CBD/Tp/tpmc.h` (95,441 bytes)

## 파일이름 : tpmc

- 경로: `src/CBD/Tp/tpmc.c`
- 파일 종류: `.c`
- 파일 크기: 256,520 bytes
- 파일 내용: ISO 15765-2 TP 상태 머신 구현/선언으로 다중 프레임 진단 송수신을 담당한다.

### 1. .c/.h 파일 이름

- `tpmc.c`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 358
- typedef/struct/enum 후보 수: 7
- 전역 변수/테이블 선언 후보 수: 56

#### 주요 매크로
- L461 `OSEKTPMC_C_MODULE` = `/*Get access to locally used generated header informations*/`
- L539 `FRAME_DATA_PTR` = `(rxStruct->pChipData)`
- L540 `RX_HANDLE` = `(rxStruct->Handle)`
- L541 `CAN_RX_ACTUAL_ID` = `(CanRxActualId(rxStruct))`
- L542 `CAN_RX_ACTUAL_ID_TYPE` = `(CanRxActualIdType(rxStruct))`
- L543 `CAN_RX_ACTUAL_DLC` = `(CanRxActualDLC(rxStruct))`
- L544 `CAN_RX_ACTUAL_CAN` = `((canbittype)(rxStruct->Channel))`
- L548 `CAN_RX_ACTUAL_ID_EXT_HI` = `(CanRxActualIdExtHi(rxStruct))`
- L550 `CAN_RX_ACTUAL_ID_EXT_MID_HI` = `(CanRxActualIdExtMidHi(rxStruct))`
- L552 `CAN_RX_ACTUAL_ID_EXT_MID_LO` = `(CanRxActualIdExtMidLo(rxStruct))`
- L554 `CAN_RX_ACTUAL_ID_EXT_LO` = `(CanRxActualIdExtLo(rxStruct))`
- L561 `FRAME_DATA_PTR` = `(rxStruct->pChipData)`
- L562 `CAN_RX_ACTUAL_ID` = `(rxStruct->CanRxActualId)`
- L563 `CAN_RX_ACTUAL_DLC` = `(rxStruct->CanRxActualDLC)`
- L564 `CAN_RX_ACTUAL_CAN` = `((canbittype)(rxStruct->Channel))`
- L568 `CAN_RX_ACTUAL_ID_EXT_HI` = `((CAN_RX_ACTUAL_ID & 0x1f000000)>>24)`
- L570 `CAN_RX_ACTUAL_ID_EXT_MID_HI` = `((CAN_RX_ACTUAL_ID & 0x00ff0000)>>16)`
- L572 `CAN_RX_ACTUAL_ID_EXT_MID_LO` = `((CAN_RX_ACTUAL_ID & 0x0000ff00)>>8)`
- L574 `CAN_RX_ACTUAL_ID_EXT_LO` = `((CAN_RX_ACTUAL_ID & 0x000000ff))`
- L586 `FRAME_DATA_PTR` = `rxDataPtr`
- L587 `RX_HANDLE` = `canRxHandle_0`
- L588 `CAN_RX_ACTUAL_ID` = `CanRxActualId_0`
- L589 `CAN_RX_ACTUAL_DLC` = `CanRxActualDLC_0`
- L590 `CAN_RX_ACTUAL_CAN` = `TP_CAN_CHANNEL`
- L594 `CAN_RX_ACTUAL_ID_EXT_HI` = `CanRxActualIdExtHi_0`
- L596 `CAN_RX_ACTUAL_ID_EXT_MID_HI` = `CanRxActualIdExtMidHi_0`
- L598 `CAN_RX_ACTUAL_ID_EXT_MID_LO` = `CanRxActualIdExtMidLo_0`
- L600 `CAN_RX_ACTUAL_ID_EXT_LO` = `CanRxActualIdExtLo_0`
- L604 `FRAME_DATA_PTR` = `rxDataPtr`
- L605 `RX_HANDLE` = `canRxHandle_1`
- L606 `CAN_RX_ACTUAL_ID` = `CanRxActualId_1`
- L607 `CAN_RX_ACTUAL_DLC` = `CanRxActualDLC_1`
- L608 `CAN_RX_ACTUAL_CAN` = `TP_CAN_CHANNEL`
- L612 `CAN_RX_ACTUAL_ID_EXT_HI` = `CanRxActualIdExtHi_1`
- L614 `CAN_RX_ACTUAL_ID_EXT_MID_HI` = `CanRxActualIdExtMidHi_1`
- L616 `CAN_RX_ACTUAL_ID_EXT_MID_LO` = `CanRxActualIdExtMidLo_1`
- L618 `CAN_RX_ACTUAL_ID_EXT_LO` = `CanRxActualIdExtLo_1`
- L622 `FRAME_DATA_PTR` = `rxDataPtr`
- L623 `RX_HANDLE` = `canRxHandle_2`
- L624 `CAN_RX_ACTUAL_ID` = `CanRxActualId_2`
- L625 `CAN_RX_ACTUAL_DLC` = `CanRxActualDLC_2`
- L626 `CAN_RX_ACTUAL_CAN` = `TP_CAN_CHANNEL`
- L630 `CAN_RX_ACTUAL_ID_EXT_HI` = `CanRxActualIdExtHi_2`
- L632 `CAN_RX_ACTUAL_ID_EXT_MID_HI` = `CanRxActualIdExtMidHi_2`
- L634 `CAN_RX_ACTUAL_ID_EXT_MID_LO` = `CanRxActualIdExtMidLo_2`
- L636 `CAN_RX_ACTUAL_ID_EXT_LO` = `CanRxActualIdExtLo_2`
- L640 `FRAME_DATA_PTR` = `rxDataPtr`
- L641 `RX_HANDLE` = `canRxHandle_3`
- L642 `CAN_RX_ACTUAL_ID` = `CanRxActualId_3`
- L643 `CAN_RX_ACTUAL_DLC` = `CanRxActualDLC_3`
- L644 `CAN_RX_ACTUAL_CAN` = `TP_CAN_CHANNEL`
- L648 `CAN_RX_ACTUAL_ID_EXT_HI` = `CanRxActualIdExtHi_3`
- L650 `CAN_RX_ACTUAL_ID_EXT_MID_HI` = `CanRxActualIdExtMidHi_3`
- L652 `CAN_RX_ACTUAL_ID_EXT_MID_LO` = `CanRxActualIdExtMidLo_3`
- L654 `CAN_RX_ACTUAL_ID_EXT_LO` = `CanRxActualIdExtLo_3`
- L658 `FRAME_DATA_PTR` = `rxDataPtr`
- L659 `RX_HANDLE` = `canRxHandle_4`
- L660 `CAN_RX_ACTUAL_ID` = `CanRxActualId_4`
- L661 `CAN_RX_ACTUAL_DLC` = `CanRxActualDLC_4`
- L662 `CAN_RX_ACTUAL_CAN` = `TP_CAN_CHANNEL`
- L666 `CAN_RX_ACTUAL_ID_EXT_HI` = `CanRxActualIdExtHi_4`
- L668 `CAN_RX_ACTUAL_ID_EXT_MID_HI` = `CanRxActualIdExtMidHi_4`
- L670 `CAN_RX_ACTUAL_ID_EXT_MID_LO` = `CanRxActualIdExtMidLo_4`
- L672 `CAN_RX_ACTUAL_ID_EXT_LO` = `CanRxActualIdExtLo_4`
- L679 `FRAME_DATA_PTR` = `rxDataPtr`
- L680 `RX_HANDLE` = `canRxHandle`
- L681 `CAN_RX_ACTUAL_ID` = `CanRxActualId`
- L682 `CAN_RX_ACTUAL_DLC` = `CanRxActualDLC`
- L683 `CAN_RX_ACTUAL_CAN` = `0`
- L687 `CAN_RX_ACTUAL_ID_EXT_HI` = `CanRxActualIdExtHi`
- L689 `CAN_RX_ACTUAL_ID_EXT_MID_HI` = `CanRxActualIdExtMidHi`
- L691 `CAN_RX_ACTUAL_ID_EXT_MID_LO` = `CanRxActualIdExtMidLo`
- L693 `CAN_RX_ACTUAL_ID_EXT_LO` = `CanRxActualIdExtLo`
- L706 `FRAME_DATA_PTR` = `CanRDSPtr_0`
- L707 `RX_HANDLE` = `rxObject`
- L708 `CAN_RX_ACTUAL_ID` = `CanRxActualId_0`
- L709 `CAN_RX_ACTUAL_DLC` = `CanRxActualDLC_0`
- L710 `CAN_RX_ACTUAL_CAN` = `TP_CAN_CHANNEL`
- L714 `CAN_RX_ACTUAL_ID_EXT_HI` = `CanRxActualIdExtHi_0`
- L716 `CAN_RX_ACTUAL_ID_EXT_MID_HI` = `CanRxActualIdExtMidHi_0`
- L718 `CAN_RX_ACTUAL_ID_EXT_MID_LO` = `CanRxActualIdExtMidLo_0`
- L720 `CAN_RX_ACTUAL_ID_EXT_LO` = `CanRxActualIdExtLo_0`
- L724 `FRAME_DATA_PTR` = `CanRDSPtr_1`
- L725 `RX_HANDLE` = `rxObject`
- L726 `CAN_RX_ACTUAL_ID` = `CanRxActualId_1`
- L727 `CAN_RX_ACTUAL_DLC` = `CanRxActualDLC_1`
- L728 `CAN_RX_ACTUAL_CAN` = `TP_CAN_CHANNEL`
- L732 `CAN_RX_ACTUAL_ID_EXT_HI` = `CanRxActualIdExtHi_1`
- L734 `CAN_RX_ACTUAL_ID_EXT_MID_HI` = `CanRxActualIdExtMidHi_1`
- L736 `CAN_RX_ACTUAL_ID_EXT_MID_LO` = `CanRxActualIdExtMidLo_1`
- L738 `CAN_RX_ACTUAL_ID_EXT_LO` = `CanRxActualIdExtLo_1`
- L742 `FRAME_DATA_PTR` = `CanRDSPtr_2`
- L743 `RX_HANDLE` = `rxObject`
- L744 `CAN_RX_ACTUAL_ID` = `CanRxActualId_2`
- L745 `CAN_RX_ACTUAL_DLC` = `CanRxActualDLC_2`
- L746 `CAN_RX_ACTUAL_CAN` = `TP_CAN_CHANNEL`
- L750 `CAN_RX_ACTUAL_ID_EXT_HI` = `CanRxActualIdExtHi_2`
- L752 `CAN_RX_ACTUAL_ID_EXT_MID_HI` = `CanRxActualIdExtMidHi_2`
- L754 `CAN_RX_ACTUAL_ID_EXT_MID_LO` = `CanRxActualIdExtMidLo_2`
- L756 `CAN_RX_ACTUAL_ID_EXT_LO` = `CanRxActualIdExtLo_2`
- L760 `FRAME_DATA_PTR` = `CanRDSPtr_3`
- L761 `RX_HANDLE` = `rxObject`
- L762 `CAN_RX_ACTUAL_ID` = `CanRxActualId_3`
- L763 `CAN_RX_ACTUAL_DLC` = `CanRxActualDLC_3`
- L764 `CAN_RX_ACTUAL_CAN` = `TP_CAN_CHANNEL`
- L768 `CAN_RX_ACTUAL_ID_EXT_HI` = `CanRxActualIdExtHi_3`
- L770 `CAN_RX_ACTUAL_ID_EXT_MID_HI` = `CanRxActualIdExtMidHi_3`
- L772 `CAN_RX_ACTUAL_ID_EXT_MID_LO` = `CanRxActualIdExtMidLo_3`
- L774 `CAN_RX_ACTUAL_ID_EXT_LO` = `CanRxActualIdExtLo_3`
- L778 `FRAME_DATA_PTR` = `CanRDSPtr_4`
- L779 `RX_HANDLE` = `rxObject`
- L780 `CAN_RX_ACTUAL_ID` = `CanRxActualId_4`
- L781 `CAN_RX_ACTUAL_DLC` = `CanRxActualDLC_4`
- L782 `CAN_RX_ACTUAL_CAN` = `TP_CAN_CHANNEL`
- L786 `CAN_RX_ACTUAL_ID_EXT_HI` = `CanRxActualIdExtHi_4`
- L788 `CAN_RX_ACTUAL_ID_EXT_MID_HI` = `CanRxActualIdExtMidHi_4`
- L790 `CAN_RX_ACTUAL_ID_EXT_MID_LO` = `CanRxActualIdExtMidLo_4`
- L792 `CAN_RX_ACTUAL_ID_EXT_LO` = `CanRxActualIdExtLo_4`
- L799 `FRAME_DATA_PTR` = `CanRDSPtr`
- L800 `RX_HANDLE` = `rxObject`
- ... 생략 238개 (동일 패턴의 생성 심볼)

#### 주요 typedef/구조체
- L1130 `typedef enum {`
- L1146 `typedef enum {`
- L1164 `typedef struct TpStateTag`
- L1183 `typedef volatile tTpTxState* tvolatileTpTxState;`
- L1193 `typedef struct`
- L1211 `typedef volatile tTpRxState* tvolatileTpRxState;`
- L5295 `typedef struct`

#### 주요 전역 변수/테이블 선언
- L489 `V_MEMROM0 V_MEMROM1 canuint8 V_MEMROM2 kTpBugFixVersion =             TP_ISO15765_RELEASE_VERSION;`
- L1167 `canuint8 BSCounter;`
- L1169 `canuint8 WFTCounter;`
- L1186 `MEMORY_NEAR static  tTpTxState      tpTxState[kTpTxChannelCount];`
- L1196 `canuint8 BSCounter;`
- L1198 `canuint8 WFTCounter;`
- L1214 `MEMORY_NEAR static  tTpRxState      tpRxState[kTpRxChannelCount];`
- L1218 `MEMORY_NEAR static canuint8 tpStateTaskBusy;`
- L1229 `MEMORY_NEAR_TP_SAVE CanTransmitHandle tpTxHandle;`
- L1233 `MEMORY_NEAR_TP_SAVE CanTransmitHandle tpTxHandle_Field[kTpTxChannelCount];`
- L1247 `MEMORY_NEAR_TP_SAVE CanTransmitHandle connHandle;`
- L1271 `extern tTpEngineTimer tpRxConfirmationTimeout [kTpRxChannelCount];`
- L1272 `extern tTpEngineTimer tpTxConfirmationTimeout [kTpTxChannelCount];`
- L1277 `V_MEMROM0 extern V_MEMROM1 tTpEngineTimer V_MEMROM2 tpTxConfirmationTimeout[kTpTxChannelCount];`
- L1278 `V_MEMROM0 extern V_MEMROM1 tTpEngineTimer V_MEMROM2 tpRxConfirmationTimeout[kTpRxChannelCount];`
- L1366 `V_MEMROM0 extern V_MEMROM1 canuint8 V_MEMROM2 TpTxTransmitCF[kTpTxChannelCount];`
- L1367 `V_MEMROM0 extern V_MEMROM1 canuint8 V_MEMROM2 TpRxTimeoutCF [kTpRxChannelCount];`
- L1368 `V_MEMROM0 extern V_MEMROM1 canuint8 V_MEMROM2 TpTxTimeoutFC [kTpTxChannelCount];`
- L1369 `V_MEMROM0 extern V_MEMROM1 canuint8 V_MEMROM2 TpSTMin       [kTpRxChannelCount];`
- L1370 `V_MEMROM0 extern V_MEMROM1 canuint8 V_MEMROM2 TpBlockSize   [kTpRxChannelCount];`
- L1378 `V_MEMROM0 extern V_MEMROM1 canuint8 V_MEMROM2 tpTxWFTmax[kTpTxChannelCount];`
- L1382 `extern tTpEngineTimer tpRxTimeoutCF [kTpRxChannelCount];`
- L1383 `extern tTpEngineTimer tpTxTimeoutFC [kTpTxChannelCount];`
- L1723 `canuint8 tpErrorIndResult;`
- L1828 `tpTxTimeoutFC[tpChannel]           = TpTxTimeoutFC;`
- L1951 `tpRxTimeoutCF[tpChannel]           = TpRxTimeoutCF;`
- L1992 `canuintCPUtype tpSTminInFrame;`
- L2002 `canuint8 tpCanRxActualDLC;`
- L2006 `canuint8 tpEcuNr;`
- L2010 `canuint8 tpPreCopyCheckFunctionReturn;`
- L2016 `CanTransmitHandle handle;`
- L2029 `vuint8 pIntermediateCANChipData[8];`
- L2165 `tpChannel = TpRxHandleToChannel[RX_HANDLE];`
- L2929 `canuint16 tmpSTmin;`
- L2941 `canuint16 tmpSTmin;`
- L3233 `canuintCPUtype preTransmitResult;`
- L3234 `canuint8 tpCanTxReturn;`
- L3400 `canuint8 tpCanTxReturn;`
- L3584 `canuintCPUtype i;`
- L3587 `canuintCPUtype rc;`
- L3590 `canuintCPUtype offset;`
- L3811 `canuintCPUtype tpChannel;`
- L3816 `tpChannel = TpTxHandleToChannel[txObject];`
- L3895 `tpTxState[tpChannel].Timer  = TpTxMinTimer[tpChannel];`
- L5300 `canuint16 DataLength;`
- L5308 `canuint8 TargetAddress;`
- L5316 `canuint8 SourceAddress;`
- L5324 `canuint8 AddressExtension;`
- L5330 `canuint8 CanChannel;`
- L5346 `MEMORY_NEAR static tTpRxStateEngine tpFuncState_engine;`
- L5348 `MEMORY_NEAR_TP_SAVE static tTpFuncInfoStruct tpFuncInfoStruct;`
- L5382 `canuint8 tpEcuNr;`
- L5384 `canuint8 FuncTpPrecopyReturn;`
- L5392 `CanTransmitHandle handle;`
- L5399 `vuint8 pFuncIntermediateCANChipData[8];`
- L5601 `vuint8 pFuncIntermediateCANChipData[8];`

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 75

- L1417 `TpTxInit(canuint8 errorCode)` [선언]
- L1418 `TpRxInit(canuint8 errorCode)` [선언]
- L1419 `TpTxPreCanTransmit(void)` [선언]
- L1420 `TpTxInternalPrepareOfCF(void)` [선언]
- L1421 `TpTxPostProcessing(void)` [선언]
- L1423 `TpRxPostProcessing(void)` [선언]
- L1436 `TpTxInit(canuint8 tpChannel, canuint8 errorCode)` [선언]
- L1437 `TpRxInit(canuint8 tpChannel, canuint8 errorCode)` [선언]
- L1438 `TpTxPreCanTransmit(canuintCPUtype tpChannel)` [선언]
- L1439 `TpTxInternalPrepareOfCF(canuintCPUtype tpChannel)` [선언]
- L1440 `TpTxPostProcessing(canuintCPUtype tpChannel)` [선언]
- L1442 `TpRxPostProcessing(canuintCPUtype tpChannel)` [선언]
- L1447 `TpFuncInit(void)` [선언]
- L1545 `TpIntMemCpy(TP_MEMORY_MODEL_DATA void *pDest, TP_MEMORY_MODEL_DATA void* pSrc, canuint16 nCnt)` [정의]
- L1574 `TpInitPowerOn(void)` [정의]
- L1660 `TpInit(void)` [정의]
- L1726 `TpGlobalInterruptDisable()` [선언]
- L1789 `TpTxGetSTMINDefaultTime(tpChannel)` [선언]
- L1857 `TpGlobalInterruptDisable()` [선언]
- L1893 `TpRxGetSTMINtimeDefault(tpChannel)` [선언]
- L2280 `__ApplTpRxGetBuffer((canuint8)tpChannel, tpRxInfoStruct[tpChannel].DataLength, rxStruct)` [선언]
- L2282 `__ApplTpRxGetBuffer((canuint8)tpChannel, tpRxInfoStruct[tpChannel].DataLength)` [선언]
- L2314 `TpRxConfirmationTimeout(tpChannel)` [선언]
- L2339 `TpRxConfirmationTimeout(tpChannel)` [선언]
- L2415 `TpRxConfirmationTimeout(tpChannel)` [선언]
- L2426 `kTimeoutCF(tpChannel)` [선언]
- L2766 `CANBITTYPE_CAST(tpRxInfoStruct[tpChannel].sequencenumber + 1)` [선언]
- L2777 `TpRxConfirmationTimeout(tpChannel)` [선언]
- L2786 `kTimeoutCF(tpChannel)` [선언]
- L2962 `TpTxGetSTMINtime(tpChannel)` [선언]
- L2985 `kTimeoutFC(tpChannel)` [선언]
- L2991 `kTimeoutFC(tpChannel)` [선언]
- L3139 `TpTxConfirmationTimeout(tpChannel)` [선언]
- L3181 `TpTask(void)` [정의]
- L3201 `TpTxAllStateTask(void)` [정의]
- L3254 `TPCANTRANSMIT(TpIntTxGetCanChannel(tpChannel)) (TP_TX_HANDLE(tpChannel))` [선언]
- L3286 `__TpTxPreCanTransmit(tpChannel)` [선언]
- L3292 `TPCANTRANSMIT(TpIntTxGetCanChannel(tpChannel)) (TP_TX_HANDLE(tpChannel))` [선언]
- L3307 `used(no buffer-underrun can happen) */ if ((tpTxState[tpChannel].engine == kTxState_WaitForCFConfIsr) || (tpTxState[tpChannel].engine == kTxState_WaitForLastCFConfIsr) || (tpTxState[tpChannel].engine == kTxState_WaitForFFConfIsr))` [정의]
- L3369 `TpRxAllStateTask(void)` [정의]
- L3421 `TPCANTRANSMIT(TpIntRxGetCanChannel(tpChannel)) (TP_RX_HANDLE(tpChannel))` [선언]
- L3521 `TpRxGetBlockSize(tpChannel)` [선언]
- L3522 `TpRxGetSTMINtime(tpChannel)` [선언]
- L3615 `CANBITTYPE_CAST(SF_OFFSET + FORMAT_OFFSET + tpTxInfoStruct[tpChannel].DataLength)` [선언]
- L3648 `CANBITTYPE_CAST(FRAME_LENGTH)` [선언]
- L3681 `CANBITTYPE_CAST(CF_OFFSET + FORMAT_OFFSET + tpCopyToCanInfoStruct.Length)` [선언]
- L3688 `CANBITTYPE_CAST(FRAME_LENGTH)` [선언]
- L3723 `__ApplTpTxCopyToCAN(&tpCopyToCanInfoStruct)` [선언]
- L3725 `CANUINTCPUTYPE_CAST(tpCopyToCanInfoStruct.Length + offset + FORMAT_OFFSET)` [선언]
- L3803 `TpDrvConfirmation(CanTransmitHandle txObject)` [정의]
- L3827 `defined(TP_ENABLE_SF_ACKNOWLEDGE) ) # if (TP_USE_RX_CHANNEL_WITHOUT_FC == kTpOn) assertInternal(tpChannel, (tpRxInfoStruct[tpChannel].withoutFC == 0), kTpErrChannelNotInUse)` [선언]
- L3918 `kTimeoutFC(tpChannel)` [선언]
- L3930 `TpTxGetSTMINtime(tpChannel)` [선언]
- L3951 `kTimeoutFC(tpChannel)` [선언]
- L3957 `TpTxGetSTMINtime(tpChannel)` [선언]
- L4024 `kTimeoutCF(tpChannel)` [선언]
- L4134 `CANBITTYPE_CAST(tpTxInfoStruct[tpChannel].sequencenumber + 1)` [선언]
- L4145 `TpTxConfirmationTimeout(tpChannel)` [선언]
- L4163 `TpTxTask(void)` [정의]
- L4222 `TpTxConfirmationTimeout(tpChannel)` [선언]
- L4281 `TpRxTask(void)` [정의]
- L4332 `TpRxConfirmationTimeout(tpChannel)` [선언]
- L4642 `TpRxSetWaitCorrectSN(canuint8 tpChannel, tpBool wait)` [정의]
- L4668 `TpTxSetCFDelay(canuint8 tpChannel, tTpEngineTimer time)` [정의]
- L4693 `TpRxSetTimeoutCF(canuint8 tpChannel, tTpEngineTimer time)` [정의]
- L4716 `TpRxSetTimeoutConfirmation(canuint8 tpChannel, tTpEngineTimer time)` [정의]
- L4865 `TpRxConfirmationTimeout(tpChannel)` [선언]
- L5172 `TpTxSetStrictFlowControlCheck(canuint8 tpChannel, tpBool strict)` [정의]
- L5198 `TpTxSetTimeoutFC(canuint8 tpChannel, tTpEngineTimer time)` [정의]
- L5221 `TpTxSetTimeoutConfirmation(canuint8 tpChannel, tTpEngineTimer time)` [정의]
- L5351 `TpFuncInit(void)` [정의]
- L5633 `__ApplTpFuncGetBuffer(tpFuncInfoStruct.DataLength)` [선언]
- L5678 `TpFuncResetChannel(void)` [정의]
- L5685 `defined(TP_FUNC_ENABLE_MIXED_29_ADDRESSING) ) /******************************************************************************* * NAME: TpFuncGetSourceAddress * * CALLED BY: Application * PRECONDITIONS: none * * PARAMETER: none * RETURN VALUE: none *******************************************************************************/ canuint8 TP_API_CALL_TYPE TpFuncGetSourceAddress(void)` [정의]
- L5703 `defined(TP_FUNC_ENABLE_MIXED_29_ADDRESSING) ) /******************************************************************************* * NAME: TpFuncGetTargetAddress * * CALLED BY: Application * PRECONDITIONS: none * * PARAMETER: none * RETURN VALUE: none *******************************************************************************/ canuint8 TP_API_CALL_TYPE TpFuncGetTargetAddress(void)` [정의]

### 4. 각 함수별 역할 설명

- `TpTxInit` (L1417, 선언): 모듈 런타임 상태, 버퍼, 카운터, 하드웨어/생성 파라미터를 초기화한다.
- `TpRxInit` (L1418, 선언): 모듈 런타임 상태, 버퍼, 카운터, 하드웨어/생성 파라미터를 초기화한다.
- `TpTxPreCanTransmit` (L1419, 선언): 송신 요청을 준비하거나 CAN/TP/진단 응답 전송을 시작한다.
- `TpTxInternalPrepareOfCF` (L1420, 선언): 전역 또는 CAN 관련 인터럽트 비활성화/복구 보호 구간을 제공한다.
- `TpTxPostProcessing` (L1421, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `TpRxPostProcessing` (L1423, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpTxInit` (L1436, 선언): defined (TP_ENABLE_SINGLE_CHANNEL_TP)
- `TpRxInit` (L1437, 선언): defined (TP_ENABLE_SINGLE_CHANNEL_TP)
- `TpTxPreCanTransmit` (L1438, 선언): defined (TP_ENABLE_SINGLE_CHANNEL_TP)
- `TpTxInternalPrepareOfCF` (L1439, 선언): defined (TP_ENABLE_SINGLE_CHANNEL_TP)
- `TpTxPostProcessing` (L1440, 선언): defined (TP_ENABLE_SINGLE_CHANNEL_TP)
- `TpRxPostProcessing` (L1442, 선언): defined (TP_ENABLE_SINGLE_CHANNEL_TP)
- `TpFuncInit` (L1447, 선언): defined (TP_ENABLE_SINGLE_CHANNEL_TP)
- `TpIntMemCpy` (L1545, 정의): NAME:              TpIntMemCpy CALLED BY:         TP PRECONDITIONS:     none PARAMETER:         Ptr to destination, source and the number of bytes to copy RETURN VALUE:      none DESCRIPTION:       Helper function: Copies nCnt bytes from pSrc to pDest
- `TpInitPowerOn` (L1574, 정의): NAME:              TpInitPowerOn CALLED BY:         Application PRECONDITIONS:     !! Call only once on startup !! !! If called a second time, the CAN driver initalization !! function 'CanInitPowerOn' has to be called before! PARAMETER:         none RETURN VAL
- `TpInit` (L1660, 정의): NAME:              TpInit CALLED BY:         Application PRECONDITIONS:     Reinitialization of the TransportLayer PARAMETER:         none RETURN VALUE:      none DESCRIPTION:       Initialization function to initialize all channels
- `TpGlobalInterruptDisable` (L1726, 선언): NAME:              TpTxInit CALLED BY:         Transport layer functions PRECONDITIONS:     none PARAMETER:         tpChannel number RETURN VALUE:      none DESCRIPTION:       Initialize the transmit tpChannel specified by parameter tpChannel
- `TpTxGetSTMINDefaultTime` (L1789, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `TpGlobalInterruptDisable` (L1857, 선언): NAME:              TpRxInit CALLED BY:         Transport layer functions PRECONDITIONS:     none PARAMETER:         Channel number RETURN VALUE:      none DESCRIPTION:       Initialize the receive Channel specified by parameter Channel Mixed11/29Addressing: tp
- `TpRxGetSTMINtimeDefault` (L1893, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `__ApplTpRxGetBuffer` (L2280, 선언): Set pointer to CAN buffer data to have access in GetBuffer function
- `__ApplTpRxGetBuffer` (L2282, 선언): Set pointer to CAN buffer data to have access in GetBuffer function
- `TpRxConfirmationTimeout` (L2314, 선언): 송신 완료 통지나 확인 상태를 처리하고 상위 계층 콜백/플래그를 갱신한다.
- `TpRxConfirmationTimeout` (L2339, 선언): 송신 완료 통지나 확인 상태를 처리하고 상위 계층 콜백/플래그를 갱신한다.
- `TpRxConfirmationTimeout` (L2415, 선언): 송신 완료 통지나 확인 상태를 처리하고 상위 계층 콜백/플래그를 갱신한다.
- `kTimeoutCF` (L2426, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `CANBITTYPE_CAST` (L2766, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `TpRxConfirmationTimeout` (L2777, 선언): 송신 완료 통지나 확인 상태를 처리하고 상위 계층 콜백/플래그를 갱신한다.
- `kTimeoutCF` (L2786, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `TpTxGetSTMINtime` (L2962, 선언): 전역 또는 CAN 관련 인터럽트 비활성화/복구 보호 구간을 제공한다.
- `kTimeoutFC` (L2985, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `kTimeoutFC` (L2991, 선언): wait frames not supported, cancel transmission (ESCAN00056224)
- `TpTxConfirmationTimeout` (L3139, 선언): (0)1 to 6/7 bytes will be a SingleFrame
- `TpTask` (L3181, 정의): NAME:              TpTask CALLED BY:         application PRECONDITIONS:     Initialized TPMC. This function must not be called outside of cyclic task context (e.g.: CAN Rx/Tx-Interrupt). PARAMETER:         none RETURN VALUE:      none DESCRIPTION:       Functi
- `TpTxAllStateTask` (L3201, 정의): NAME:              TpTxAllStateTask CALLED BY:         application PRECONDITIONS:     none PARAMETER:         none RETURN VALUE:      none DESCRIPTION:       Handles transmission request
- `TPCANTRANSMIT` (L3254, 선언): NAME:              TpTxStateTask CALLED BY: PRECONDITIONS: PARAMETER:         Number of current receive tpChannel RETURN VALUE:      none DESCRIPTION:       Transmit a CAN frame
- `__TpTxPreCanTransmit` (L3286, 선언): 송신 요청을 준비하거나 CAN/TP/진단 응답 전송을 시작한다.
- `TPCANTRANSMIT` (L3292, 선언): 송신 요청을 준비하거나 CAN/TP/진단 응답 전송을 시작한다.
- `used` (L3307, 정의): TpGlobalInterruptDisable(); No ISRlock needed - no other part can access other members while this time
- `TpRxAllStateTask` (L3369, 정의): NAME:              TpRxAllStateTask CALLED BY:         application PRECONDITIONS:     none PARAMETER:         none RETURN VALUE:      none DESCRIPTION:       Handles transmission request
- `TPCANTRANSMIT` (L3421, 선언): NAME:              TpRxStateTask CALLED BY:         Transport Layer (TpReceive) PRECONDITIONS: PARAMETER:         Number of current receive tpChannel RETURN VALUE:      none DESCRIPTION:       Transmit a CAN frame
- `TpRxGetBlockSize` (L3521, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxGetSTMINtime` (L3522, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `CANBITTYPE_CAST` (L3615, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `CANBITTYPE_CAST` (L3648, 선언): Set tx index to next free data element
- `CANBITTYPE_CAST` (L3681, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `CANBITTYPE_CAST` (L3688, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `__ApplTpTxCopyToCAN` (L3723, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `CANUINTCPUTYPE_CAST` (L3725, 선언): 전역 또는 CAN 관련 인터럽트 비활성화/복구 보호 구간을 제공한다.
- `TpDrvConfirmation` (L3803, 정의): NAME:              TpDrvConfirmation CALLED BY:         Confirmation Interrupt PRECONDITIONS:     Confirmation interrupts are configured PARAMETER:         Handle of message to be confirmed RETURN VALUE:      none DESCRIPTION:       Inform application and star
- `defined` (L3827, 선언): Only static normal addressing Multi TP
- `kTimeoutFC` (L3918, 선언): ----------------------------------------------------------------------------- FirstFrame -----------------------------------------------------------------------------
- `TpTxGetSTMINtime` (L3930, 선언): setting BSCounter to zero to avoid further FlowControls
- `kTimeoutFC` (L3951, 선언): Check for FC of counterpart now
- `TpTxGetSTMINtime` (L3957, 선언): Check for FC of counterpart now
- `kTimeoutCF` (L4024, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `CANBITTYPE_CAST` (L4134, 선언): Calculate next SN - SN is calculated modulo 15
- `TpTxConfirmationTimeout` (L4145, 선언): 송신 완료 통지나 확인 상태를 처리하고 상위 계층 콜백/플래그를 갱신한다.
- `TpTxTask` (L4163, 정의): NAME:              TpTxTask (Timer) CALLED BY:         OS, Application PRECONDITIONS:     Cyclical called function (e.g. every 10ms) PARAMETER:         none RETURN VALUE:      none DESCRIPTION:       Survey timing conditions, Transmit consecutive frames
- `TpTxConfirmationTimeout` (L4222, 선언): (0)1 to 6/7 bytes will be a SingleFrame
- `TpRxTask` (L4281, 정의): NAME:              TpRxTask (Timer) CALLED BY:         OS, Application PRECONDITIONS:     Cyclical called function (e.g. every 20ms) PARAMETER:         none RETURN VALUE:      none DESCRIPTION:       Survey receive timing conditions
- `TpRxConfirmationTimeout` (L4332, 선언): Force sending next FC with status wait
- `TpRxSetWaitCorrectSN` (L4642, 정의): NAME:              TpRxSetWaitCorrectSN CALLED BY:         Application PRECONDITIONS:     TL has to be initialized before PARAMETER:         tpChannel handle, tpBool wait (kTpTrue = strict SN check, kTpFalse = wait for correct SN) RETURN VALUE:      none DESCR
- `TpTxSetCFDelay` (L4668, 정의): NAME:              TpTxSetCFDelay CALLED BY:         Application PRECONDITIONS:     TL has to be initialized before PARAMETER:         tpChannel handle, tTpEngineTimer time RETURN VALUE:      none DESCRIPTION: REENTRANCE:        Possible for different channels
- `TpRxSetTimeoutCF` (L4693, 정의): NAME:              TpRxSetTimeoutCF CALLED BY:         Application PRECONDITIONS:     TL has to be initialized before PARAMETER:         tpChannel handle, tTpEngineTimer time RETURN VALUE:      none DESCRIPTION: REENTRANCE:        Possible for different channe
- `TpRxSetTimeoutConfirmation` (L4716, 정의): NAME:              TpRxSetTimeoutConfirmation CALLED BY:         Application PRECONDITIONS:     TL has to be initialized before PARAMETER:         tpChannel handle, tTpEngineTimer time RETURN VALUE:      none DESCRIPTION: REENTRANCE:        Possible for differ
- `TpRxConfirmationTimeout` (L4865, 선언): NAME:              TpRxSetClearToSend CALLED BY:         Application PRECONDITIONS:     none PARAMETER:         tpChannel in case of non singe channel tp RETURN VALUE:      FC status of channel
- `TpTxSetStrictFlowControlCheck` (L5172, 정의): NAME:              TpTxWithStrictFlowControlCheck CALLED BY:         Application PRECONDITIONS:     TL has to be initialized before PARAMETER:         tpChannel handle, tpBool strict ( kTpTrue  = strict FC check, kTpFalse = no strict FC check) RETURN VALUE:   
- `TpTxSetTimeoutFC` (L5198, 정의): NAME:              TpTxSetTimeoutFC CALLED BY:         Application PRECONDITIONS:     TL has to be initialized before PARAMETER:         tpChannel handle, tTpEngineTimer time RETURN VALUE:      none DESCRIPTION: REENTRANCE:        Possible for different channe
- `TpTxSetTimeoutConfirmation` (L5221, 정의): NAME:              TpTxSetTimeoutConfirmation CALLED BY:         Application PRECONDITIONS:     TL has to be initialized before PARAMETER:         tpChannel handle, tTpEngineTimer time RETURN VALUE:      none DESCRIPTION: REENTRANCE:        Possible for differ
- `TpFuncInit` (L5351, 정의): 모듈 런타임 상태, 버퍼, 카운터, 하드웨어/생성 파라미터를 초기화한다.
- `__ApplTpFuncGetBuffer` (L5633, 선언): Is maximum length of a SingleFrame exeeded ?
- `TpFuncResetChannel` (L5678, 정의): NAME:              TpFuncResetChannel CALLED BY:         Application PRECONDITIONS:     none PARAMETER:         none RETURN VALUE:      none
- `defined` (L5685, 정의): NAME:              TpFuncResetChannel CALLED BY:         Application PRECONDITIONS:     none PARAMETER:         none RETURN VALUE:      none
- `defined` (L5703, 정의): NAME:              TpFuncGetSourceAddress CALLED BY:         Application PRECONDITIONS:     none PARAMETER:         none RETURN VALUE:      none

### 5. 필요시 코드 부연 설명

대표 함수 원형 수준의 코드 맥락이다. 전체 구현은 원본 파일이 크므로 함수명/역할 중심으로 분석했다.

```c
// src/CBD/Tp/tpmc.c:1417
static void TpTxInit(canuint8 errorCode)
```
```c
// src/CBD/Tp/tpmc.c:1418
static void TpRxInit(canuint8 errorCode)
```
```c
// src/CBD/Tp/tpmc.c:1419
static canuintCPUtype TpTxPreCanTransmit(void)
```
```c
// src/CBD/Tp/tpmc.c:1420
static TP_INTERNAL_INLINE void TpTxInternalPrepareOfCF(void)
```
```c
// src/CBD/Tp/tpmc.c:1421
static TP_INTERNAL_INLINE void TpTxPostProcessing(void)
```
```c
// src/CBD/Tp/tpmc.c:1423
static TP_INTERNAL_INLINE void TpRxPostProcessing(void)
```
```c
// src/CBD/Tp/tpmc.c:1436
static void TpTxInit(canuint8 tpChannel, canuint8 errorCode)
```
```c
// src/CBD/Tp/tpmc.c:1437
static void TpRxInit(canuint8 tpChannel, canuint8 errorCode)
```

## 파일이름 : tpmc

- 경로: `src/CBD/Tp/tpmc.h`
- 파일 종류: `.h`
- 파일 크기: 95,441 bytes
- 파일 내용: ISO 15765-2 TP 상태 머신 구현/선언으로 다중 프레임 진단 송수신을 담당한다.

### 1. .c/.h 파일 이름

- `tpmc.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 257
- typedef/struct/enum 후보 수: 33
- 전역 변수/테이블 선언 후보 수: 52

#### 주요 매크로
- L432 `OSEKTPMC_H`
- L438 `TP_ISO15765_VERSION` = `0x0307`
- L439 `TP_ISO15765_RELEASE_VERSION` = `0x03`
- L441 `OSEK_TRANSPORT_LAYER_VERSION` = `TP_ISO15765_VERSION`
- L442 `OSEK_TRANSPORT_LAYER_BUGFIX_VERSION` = `TP_ISO15765_RELEASE_VERSION`
- L446 `OSEKTP_C_MODULE` = `/* Only for compatibility with tp_cfg.h */`
- L456 `TP_ENABLE_FC_SUPPRESS`
- L460 `VGEN_ENABLE_VSTDLIB`
- L467 `TP_ENABLE_NORMAL_ADDRESSING`
- L471 `TP_DISABLE_NORMAL_ADDRESSING`
- L476 `TP_DISABLE_NORMAL_ADDRESSING`
- L481 `TP_ENABLE_NORMAL_FIXED_ADDRESSING`
- L484 `TP_DISABLE_NORMAL_FIXED_ADDRESSING`
- L489 `TP_DISABLE_NORMAL_FIXED_ADDRESSING`
- L494 `TP_ENABLE_EXTENDED_ADDRESSING`
- L497 `TP_DISABLE_EXTENDED_ADDRESSING`
- L502 `TP_DISABLE_EXTENDED_ADDRESSING`
- L507 `TP_ENABLE_MIXED_29_ADDRESSING`
- L510 `TP_DISABLE_MIXED_29_ADDRESSING`
- L515 `TP_DISABLE_MIXED_29_ADDRESSING`
- L520 `TP_ENABLE_MIXED_11_ADDRESSING`
- L523 `TP_DISABLE_MIXED_11_ADDRESSING`
- L528 `TP_DISABLE_MIXED_11_ADDRESSING`
- L533 `TP_ENABLE_DYNAMIC_CHANNELS`
- L536 `TP_DISABLE_DYNAMIC_CHANNELS`
- L548 `TP_ENABLE_VARIABLE_DLC`
- L552 `TP_DISABLE_VARIABLE_DLC`
- L558 `TP_USE_WAIT_FOR_CORRECT_SN` = `kTpOff`
- L562 `TP_ENABLE_WAIT_FOR_CORRECT_SN`
- L573 `TP_DISABLE_CHECKTA_COMPATIBILITY`
- L576 `TP_ENABLE_CHECKTA_COMPATIBILITY`
- L585 `kTpNumberOfCanChannels` = `kCanNumberOfChannels`
- L590 `TP_CAN_CODEDOUBLED`
- L594 `TP_DISABLE_DYNAMIC_CHANNELS`
- L600 `TP_ENABLE_MULTI_CHANNEL_TP`
- L604 `TP_DISABLE_MULTIPLE_ADDRESSING`
- L610 `TP_DISABLE_ISO_15765_2_2`
- L615 `TP_USE_PRE_DISPATCHED_MODE` = `kTpOff`
- L618 `TP_ENABLE_PRE_DISPATCHED_MODE`
- L620 `TP_DISABLE_PRE_DISPATCHED_MODE`
- L624 `TP_USE_CONNECTIONS` = `kTpOn`
- L627 `TP_USE_FAST_PRECOPY` = `kTpOff`
- L630 `TP_USE_DIAGPRECOPY` = `kTpOff`
- L633 `TP_USE_CHANNEL_0_FOR_SPECIAL_PURPOSE` = `kTpOff`
- L636 `TP_USE_WAIT_FRAMES` = `kTpOff`
- L640 `TP_DISABLE_MCAN`
- L643 `VGEN_DISABLE_TP_MCAN`
- L646 `TP_ENABLE_MF_RECEPTION`
- L649 `TP_USE_APPL_PRECOPY` = `kTpOff`
- L652 `TP_USE_GATEWAY_API` = `kTpOff`
- L655 `TP_USE_EXTENDED_API_STMIN` = `kTpOff`
- L658 `TP_USE_RX_CHANNEL_WITHOUT_FC` = `kTpOff`
- L661 `TP_USE_TX_CHANNEL_WITHOUT_FC` = `kTpOff`
- L664 `TP_USE_MULTIPLE_ECU_NR` = `kTpOff`
- L667 `TP_USE_MULTIPLE_ECU` = `kTpOff`
- L670 `TP_USE_MULTIPLE_BASEADDRESS` = `kTpOff`
- L673 `TP_USE_TX_ERROR_IND_COMPATIBILITY` = `kTpOff`
- L676 `TP_USE_TX_HANDLE_CHANGEABLE` = `kTpOff`
- L679 `TP_USE_EXT_IDS_FOR_NORMAL` = `kTpOff`
- L682 `TP_USE_MIXED_IDS_FOR_NORMAL` = `kTpOff`
- L691 `TP_USE_STRICT_MSG_FLOW_CHECKING` = `kTpOn`
- L695 `TP_DISABLE_FC_MSG_FLOW_DYN_CHECK`
- L699 `TP_DISABLE_VARIABLE_DLC`
- L703 `TP_DISABLE_IGNORE_CONTENT_FC`
- L707 `TP_DISABLE_WAIT_FOR_CORRECT_SN`
- L711 `TP_DISABLE_DYN_AWAIT_CORRECT_SN`
- L715 `TP_DISABLE_DYN_TX_STMIN_TIMING`
- L719 `TP_DISABLE_DYN_CHANNEL_TIMING`
- L723 `TP_DISABLE_SINGLE_CHAN_MULTICONN`
- L727 `TP_DISABLE_EXT_COPYFROMCAN_API`
- L731 `TP_USE_OLD_STMIN_CALCULATION` = `kTpOff`
- L734 `TP_USE_PRE_COPY_CHECK` = `kTpOff`
- L737 `TP_USE_FAST_TX_TRANSMISSION` = `kTpOff`
- L740 `TP_USE_ISO_COMPLIANCE` = `kTpOff`
- L743 `TP_USE_OVERRUN_INDICATION` = `kTpOff`
- L746 `TP_USE_TX_OF_FC_IN_ISR` = `kTpOn`
- L749 `TP_USE_QUEUE_IN_ISR` = `kTpOn`
- L752 `TP_USE_NO_STMIN_AFTER_FC` = `kTpOff`
- L755 `TP_USE_VARIABLE_RX_DLC_CHECK` = `kTpOn`
- L758 `TP_USE_FIX_RX_DLC_CHECK` = `kTpOff`
- L761 `TP_USE_EXTENDED_API_BS` = `kTpOff`
- L764 `TP_USE_ONLY_FIRST_FC` = `kTpOff`
- L768 `TP_USE_WITHOUT_FC_TPCI_ADDON` = `kTpOff`
- L771 `TP_USE_TP_TX_FC` = `kTpOff`
- L775 `TP_HIGH_RX_LOW_TX_PRIORITY` = `kTpOn`
- L780 `TP_USE_STMIN_OF_FC` = `kTpOn`
- L784 `TP_USE_TP_INDICATION` = `kTpOff`
- L788 `TP_USE_TP_RX_SF` = `kTpOff`
- L792 `TP_USE_TP_RX_FF` = `kTpOff`
- L796 `TP_USE_TP_RX_CF` = `kTpOff`
- L800 `TP_USE_TP_RX_GET_BUFFER` = `kTpOff`
- L804 `TP_USE_TX_ERROR_INDICATION` = `kTpOff`
- L808 `TP_USE_CUSTOM_TX_MEMCPY` = `kTpOff`
- L812 `TP_USE_CUSTOM_RX_MEMCPY` = `kTpOff`
- L823 `TP_USE_TP_CONFIRMATION` = `kTpOff`
- L827 `TP_USE_TP_NOTIFY_TX` = `kTpOff`
- L831 `TP_USE_TP_CAN_MESSAGE_TRANSMITTED` = `kTpOff`
- L835 `TP_USE_TP_CAN_MESSAGE_RECEIVED` = `kTpOff`
- L839 `TP_DISABLE_FC_WAIT`
- L843 `TP_ENABLE_FC_SUPPRESS`
- L847 `TP_USE_UNEXPECTED_FC_CANCELATION` = `kTpOff`
- L851 `TP_USE_MICROSEC_STMIN` = `kTpOn`
- L856 `TP_DISABLE_TX_ERR_ON_RX_FC_WAIT`
- L860 `TP_DISABLE_DYN_EXT_ID`
- L863 `TP_RX_ENABLE_DYN_EXT_ID`
- L864 `TP_RX_ENABLE_DYN_EXT_ID`
- L867 `TP_RX_DISABLE_DYN_EXT_ID`
- L870 `TP_TX_DISABLE_DYN_EXT_ID`
- L875 `TP_ENABLE_FC_OVERFLOW`
- L877 `TP_DISABLE_FC_OVERFLOW`
- L883 `TP_ENABLE_REQUEST_QUEUE`
- L885 `TP_DISABLE_REQUEST_QUEUE`
- L890 `TP_DISABLE_OBD_PRECOPY`
- L894 `TP_DISABLE_IGNORE_FC_RES_STMIN`
- L898 `TP_DISABLE_IGNORE_FC_OVFL`
- L903 `__ApplTpPreCopyCheckFunction(x)` = `1`
- L909 `__ApplTpTxDelayFinished(tpChannel, state)`
- L914 `__ApplTpRxCanMessageTransmitted(tpChannel)`
- L921 `__ApplTpTxFC(tpChannel)`
- L927 `TP_USE_PADDING` = `kTpOn`
- ... 생략 137개 (동일 패턴의 생성 심볼)

#### 주요 typedef/구조체
- L1269 `typedef enum`
- L1275 `typedef vuint8 tTpConnectionHandle;`
- L1281 `typedef canuint8        canuintCPUtype;`
- L1282 `typedef cansint8        cansintCPUtype;`
- L1284 `typedef unsigned int    canuintCPUtype;`
- L1285 `typedef signed int      cansintCPUtype;`
- L1290 `typedef canuint16 tTpEngineTimer;`
- L1297 `typedef canuint8 tTpEngineTimer;`
- L1299 `typedef canuint16 tTpEngineTimer;`
- L1303 `typedef canuint16 tTpEngineTimer;`
- L1311 `typedef canuint8 CanChannelHandle;`
- L1313 `typedef struct`
- L1328 `typedef tCanRxInfoStruct          *CanRxInfoStructPtr;`
- L1333 `typedef struct`
- L1339 `typedef struct`
- L1438 `typedef struct`
- L1978 `typedef TP_MEMORY_MODEL_DATA canuint8 *  TP_API_CALLBACK_TYPE (*ApplTpGetRxBufferFct)(canuint8, canuint16, CanRxInfoStructPtr);`
- L1980 `typedef TP_MEMORY_MODEL_DATA canuint8 *  TP_API_CALLBACK_TYPE (*ApplTpGetRxBufferFct)(canuint8, canuint16);`
- L1982 `typedef void        TP_API_CALLBACK_TYPE (*ApplTpIndicationFct)(canuint8, canuint16);`
- L1983 `typedef void        TP_API_CALLBACK_TYPE (*ApplTpConfirmationFct)(canuint8, canuint8);`
- L1984 `typedef void        TP_API_CALLBACK_TYPE (*ApplTpRxErrorIndicationFct)(canuint8, canuint8);`
- L1985 `typedef canuint8    TP_API_CALLBACK_TYPE (*ApplTpTxErrorIndicationFct)(canuint8, canuint8);`
- L1986 `typedef canuint8    TP_API_CALLBACK_TYPE (*ApplTpCopyToCanFct)(TpCopyToCanInfoStructPtr);`
- L1987 `typedef void        TP_API_CALLBACK_TYPE (*ApplTpCopyFromCanFct)(canuint8, canuint8 *, canuint16);`
- L1988 `typedef void        TP_API_CALLBACK_TYPE (*ApplTpNotificationFct)(canuint8, canuint8);`
- L1989 `typedef void        TP_API_CALLBACK_TYPE (*ApplTpCanMessageTransmittedFct)(canuint8);`
- L1990 `typedef void        TP_API_CALLBACK_TYPE (*ApplTpCanMessageReceivedFct)(canuint8);`
- L1991 `typedef void        TP_API_CALLBACK_TYPE (*ApplTpTxFCFct)(canuint8);`
- L1992 `typedef void        TP_API_CALLBACK_TYPE (*ApplTpSFFct)(canuint8);`
- L1993 `typedef void        TP_API_CALLBACK_TYPE (*ApplTpFFFct)(canuint8);`
- L1994 `typedef void        TP_API_CALLBACK_TYPE (*ApplTpCFFct)(canuint8);`
- L2030 `typedef canuint8 (*TpCanTransmitFct)(CanTransmitHandle txObject);`
- L2032 `typedef void (*TpCanCancelTransmitFct)(CanTransmitHandle txObject);`

#### 주요 전역 변수/테이블 선언
- L1261 `V_MEMROM0 extern V_MEMROM1 canuint8 V_MEMROM2 kTpMainVersion;`
- L1262 `V_MEMROM0 extern V_MEMROM1 canuint8 V_MEMROM2 kTpSubVersion;`
- L1263 `V_MEMROM0 extern V_MEMROM1 canuint8 V_MEMROM2 kTpBugFixVersion;`
- L1316 `CanChipMsgPtr     pChipMsgObj;`
- L1318 `CanChipDataPtr    pChipData;`
- L1320 `canuint32         CanRxActualId;`
- L1322 `canuint16         CanRxActualId;`
- L1324 `canuint8          CanRxActualDLC;`
- L1325 `CanChannelHandle  Channel;`
- L1326 `} tCanRxInfoStruct;`
- L1335 `CanChipDataPtr DataCanBufferPtr;`
- L1336 `TP_MEMORY_MODEL_DATA canuint8 * DataApplBufferPtr;`
- L1344 `canuint16 DataIndex;`
- L1348 `canuint16 DataLength;`
- L1354 `canuint8 EcuNumber;`
- L1361 `canuint8 BlockSize;`
- L1367 `canuint8 STMin;`
- L1375 `canuint8  Connection;`
- L1383 `canuint8 FFDataBuffer[6];`
- L1385 `canuint8 FFDataBuffer[5];`
- L1440 `TP_MEMORY_MODEL_DATA canuint8 *DataBufferPtr;`
- L1443 `canuint16 DataIndex;`
- L1447 `canuint16 DataLength;`
- L1453 `canuint8 EcuNumber;`
- L1459 `canuint8 BlockSize;`
- L1465 `canuint8 STMin;`
- L1472 `canuint8 STminInFrame;`
- L1477 `canuint8  Connection;`
- L1786 `V_MEMROM0 extern V_MEMROM1 canuint8 V_MEMROM2 TpRxHandleToChannel[];`
- L1787 `V_MEMROM0 extern V_MEMROM1 canuint8 V_MEMROM2 TpTxHandleToChannel[];`
- L1791 `extern MEMORY_NEAR_TP_SAVE tTpRxInfoStruct tpRxInfoStruct[kTpRxChannelCount];`
- L1792 `extern MEMORY_NEAR_TP_SAVE tTpTxInfoStruct tpTxInfoStruct[kTpTxChannelCount];`
- L1796 `extern MEMORY_NEAR_TP_SAVE CanTransmitHandle tpTxHandle;`
- L1800 `extern MEMORY_NEAR_TP_SAVE CanTransmitHandle tpTxHandle_Field[kTpTxChannelCount];`
- L1808 `V_MEMROM0 extern V_MEMROM1 canuint8 V_MEMROM2 TpTxMinTimer[kTpTxChannelCount];`
- L1998 `V_MEMROM0 extern V_MEMROM1 ApplTpIndicationFct            V_MEMROM2 TpRxIndication[];`
- L1999 `V_MEMROM0 extern V_MEMROM1 ApplTpConfirmationFct          V_MEMROM2 TpTxConfirmation[];`
- L2000 `V_MEMROM0 extern V_MEMROM1 ApplTpRxErrorIndicationFct     V_MEMROM2 TpRxErrorIndication[];`
- L2001 `V_MEMROM0 extern V_MEMROM1 ApplTpTxErrorIndicationFct     V_MEMROM2 TpTxErrorIndication[];`
- L2002 `V_MEMROM0 extern V_MEMROM1 ApplTpCopyToCanFct             V_MEMROM2 TpTxCopyToCan[];`
- L2004 `V_MEMROM0 extern V_MEMROM1 ApplTpCopyFromCanFct           V_MEMROM2 TpRxCopyFromCan[];`
- L2006 `V_MEMROM0 extern V_MEMROM1 ApplTpNotificationFct          V_MEMROM2 TpTxNotification[];`
- L2007 `V_MEMROM0 extern V_MEMROM1 ApplTpCanMessageTransmittedFct V_MEMROM2 TpTxCanMessageTransmitted[];`
- L2008 `V_MEMROM0 extern V_MEMROM1 ApplTpCanMessageTransmittedFct V_MEMROM2 TpRxCanMessageTransmitted[];`
- L2009 `V_MEMROM0 extern V_MEMROM1 ApplTpTxFCFct                  V_MEMROM2 TpTxFC[];`
- L2010 `V_MEMROM0 extern V_MEMROM1 ApplTpSFFct                    V_MEMROM2 TpRxSF[];`
- L2011 `V_MEMROM0 extern V_MEMROM1 ApplTpFFFct                    V_MEMROM2 TpRxFF[];`
- L2012 `V_MEMROM0 extern V_MEMROM1 ApplTpCFFct                    V_MEMROM2 TpRxCF[];`
- L2013 `V_MEMROM0 extern V_MEMROM1 ApplTpGetRxBufferFct           V_MEMROM2 TpGetRxBuffer[];`
- L2015 `V_MEMROM0 extern V_MEMROM1 canuint8 V_MEMROM2 TpTxFlowControl[kTpTxChannelCount];`
- L2016 `V_MEMROM0 extern V_MEMROM1 canuint8 V_MEMROM2 TpRxFlowControl[kTpRxChannelCount];`
- L2019 `V_MEMROM0 extern V_MEMROM1 canuint8 V_MEMROM2 TpAddressingFormatOffset[kTpTxChannelCount];`

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 87

- L1523 `defined(TP_ENABLE_GEN_CHECK) extern void TP_API_CALLBACK_TYPE ApplTpFatalError(canuint8 errorNumber)` [선언]
- L1530 `TpPreCopyCheckFunction(CanRxInfoStructPtr rxStruct)` [선언]
- L1534 `TpPreCopyCheckFunction(CanChipDataPtr rxDataPtr)` [선언]
- L1537 `TpPreCopyCheckFunction(CanReceiveHandle rxObject)` [선언]
- L1552 `TpIntMemCpy(TP_MEMORY_MODEL_DATA void *pDest, TP_MEMORY_MODEL_DATA void* pSrc, canuint16 nCnt)` [선언]
- L1557 `TpInitPowerOn(void)` [선언]
- L1558 `TpInit(void)` [선언]
- L1566 `TpTask(void)` [선언]
- L1567 `TpTxTask(void)` [선언]
- L1568 `TpRxTask(void)` [선언]
- L1573 `TpTransmit(TP_MEMORY_MODEL_DATA canuint8* ptrData, canuint16 count)` [선언]
- L1574 `TpTxResetChannel(void)` [선언]
- L1578 `TpTxSetPriorityBits(canuint8 prio, canuint8 res, canuint8 dataPage)` [선언]
- L1579 `TpTxSetPGN(canuint8 pgn)` [선언]
- L1582 `TpRxSetPriorityBits(canuint8 prio, canuint8 res, canuint8 dataPage)` [선언]
- L1583 `TpRxSetPGN(canuint8 pgn)` [선언]
- L1589 `TpTxSetEcuNumber(canuint8 ecuNr)` [선언]
- L1592 `TpTxSetResponse(void)` [선언]
- L1597 `TpRxResetChannel(void)` [선언]
- L1598 `TpRxGetStatus(void)` [선언]
- L1600 `TpRxSetBS(canuint8 newBS)` [선언]
- L1601 `TpRxGetBS(void)` [선언]
- L1604 `TpRxGetTaType(void)` [선언]
- L1607 `TpRxSetSTMIN(canuint8 newSTmin)` [선언]
- L1608 `TpRxGetSTMIN(void)` [선언]
- L1611 `TpRxGetEcuNumber(void)` [선언]
- L1614 `TpTxPrepareSendImmediate(void)` [선언]
- L1615 `TpTxSendImmediate(void)` [선언]
- L1616 `TpTxGetSTminInFrame(void)` [선언]
- L1620 `TpRxGetCanChannel(void)` [선언]
- L1624 `TpRxGetCanBuffer(void)` [선언]
- L1627 `TpRxSetBufferOverrun(void)` [선언]
- L1630 `TpTxTestForceConfirmationTimeout(void)` [선언]
- L1631 `TpRxTestForceConfirmationTimeout(void)` [선언]
- L1634 `TpRxSetClearToSend(TP_MEMORY_MODEL_DATA canuint8 * pBuffer)` [선언]
- L1637 `TpRxSetFCStatus(canuint8 FCStatus)` [선언]
- L1638 `TpRxGetFCStatus(void)` [선언]
- L1645 `TpTransmit(canuint8 tpChannel, TP_MEMORY_MODEL_DATA canuint8 *ptrData, canuint16 count)` [선언]
- L1647 `TpTxResetChannel(canuint8 tpChannel)` [선언]
- L1653 `TpTxSetPriorityBits(canuint8 tpChannel, canuint8 prio, canuint8 res, canuint8 dataPage)` [선언]
- L1654 `TpTxSetPGN(canuint8 tpChannel, canuint8 pgn)` [선언]
- L1657 `TpRxSetPriorityBits(canuint8 tpChannel, canuint8 prio, canuint8 res, canuint8 dataPage)` [선언]
- L1658 `TpRxSetPGN(canuint8 tpChannel, canuint8 pgn)` [선언]
- L1664 `TpTxSetEcuNumber(canuint8 tpChannel, canuint8 ecuNr)` [선언]
- L1667 `TpRxResetChannel(canuint8 tpChannel)` [선언]
- L1668 `TpRxGetStatus(canuint8 tpChannel)` [선언]
- L1671 `TpRxSetBS(canuint8 tpChannel, canuint8 newBS)` [선언]
- L1672 `TpRxGetBS(canuint8 tpChannel)` [선언]
- L1675 `TpRxSetSTMIN(canuint8 tpChannel, canuint8 newSTmin)` [선언]
- L1676 `TpRxGetSTMIN(canuint8 tpChannel)` [선언]
- L1679 `TpRxGetTaType(canuint8 tpChannel)` [선언]
- L1682 `TpRxGetCanChannel(canuint8 tpChannel)` [선언]
- L1687 `TpRxGetEcuNumber(canuint8 tpChannel)` [선언]
- L1694 `TpTxSetResponse(canuint8 rxChannel, canuint8 txChannel)` [선언]
- L1699 `TpTxPrepareSendImmediate(canuint8 tpChannel)` [선언]
- L1700 `TpTxSendImmediate(canuint8 tpChannel)` [선언]
- L1701 `TpTxGetSTminInFrame(canuint8 tpChannel)` [선언]
- L1704 `TpRxGetCanBuffer(canuint8 tpChannel)` [선언]
- L1707 `TpRxSetBufferOverrun(canuint8 tpChannel)` [선언]
- L1710 `TpTxTestForceConfirmationTimeout(canuint8 tpChannel)` [선언]
- L1711 `TpRxTestForceConfirmationTimeout(canuint8 tpChannel)` [선언]
- L1715 `TpRxSetClearToSend(canuint8 tpChannel, TP_MEMORY_MODEL_DATA canuint8 * pBuffer)` [선언]
- L1718 `TpRxSetFCStatus(canuint8 tpChannel, canuint8 FCStatus)` [선언]
- L1719 `TpRxGetFCStatus(canuint8 tpChannel)` [선언]
- L1723 `TpRxSetWaitCorrectSN(canuint8 tpChannel, tpBool wait)` [선언]
- L1727 `TpTxSetStrictFlowControlCheck(canuint8 tpChannel, tpBool strict)` [선언]
- L1731 `TpTxSetCFDelay(canuint8 tpChannel, tTpEngineTimer time)` [선언]
- L1735 `TpRxSetTimeoutCF(canuint8 tpChannel, tTpEngineTimer time)` [선언]
- L1736 `TpRxSetTimeoutConfirmation(canuint8 tpChannel, tTpEngineTimer time)` [선언]
- L1737 `TpTxSetTimeoutFC(canuint8 tpChannel, tTpEngineTimer time)` [선언]
- L1738 `TpTxSetTimeoutConfirmation(canuint8 tpChannel, tTpEngineTimer time)` [선언]
- L1746 `TpTxStateTask(void)` [선언]
- L1749 `TpTxStateTask(canuint8 tpChannel)` [선언]
- L1750 `TpTxAllStateTask(void)` [선언]
- L1753 `TpRxStateTask(void)` [선언]
- L1756 `TpRxStateTask(canuint8 tpChannel)` [선언]
- L1757 `TpRxAllStateTask(void)` [선언]
- L1764 `TpTxPreTransmit(CanTxInfoStruct txStruct)` [선언]
- L1766 `TpDrvTxPreTransmit(vuintx tpChannel, CanTxInfoStruct txStruct)` [선언]
- L1770 `TpTxPreTransmit(CanChipDataPtr txDataPtr)` [선언]
- L1772 `TpDrvTxPreTransmit(vuintx tpChannel, CanChipDataPtr txDataPtr)` [선언]
- L2042 `TpFuncResetChannel(void)` [선언]
- L2051 `defined(TP_FUNC_ENABLE_MIXED_29_ADDRESSING) canuint8 TP_API_CALL_TYPE TpFuncGetSourceAddress(void)` [선언]
- L2057 `defined(TP_FUNC_ENABLE_MIXED_29_ADDRESSING) canuint8 TP_API_CALL_TYPE TpFuncGetTargetAddress(void)` [선언]
- L2063 `TpFuncGetCanChannel(void)` [선언]
- L2067 `TpFuncGetCanBuffer(void)` [선언]
- L2070 `TpFuncGetAddressExtension(void)` [선언]

### 4. 각 함수별 역할 설명

- `defined` (L1523, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `TpPreCopyCheckFunction` (L1530, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpPreCopyCheckFunction` (L1534, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpPreCopyCheckFunction` (L1537, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpIntMemCpy` (L1552, 선언): Vector 공통 메모리 복사/초기화 유틸리티 함수이다.
- `TpInitPowerOn` (L1557, 선언): 전원 인가 직후 한 번 수행되는 모듈 전역 초기화를 담당한다.
- `TpInit` (L1558, 선언): 모듈 런타임 상태, 버퍼, 카운터, 하드웨어/생성 파라미터를 초기화한다.
- `TpTask` (L1566, 선언): 주기적으로 호출되어 타이머, 상태 머신, 송수신 대기 작업을 진행한다.
- `TpTxTask` (L1567, 선언): 주기적으로 호출되어 타이머, 상태 머신, 송수신 대기 작업을 진행한다.
- `TpRxTask` (L1568, 선언): 주기적으로 호출되어 타이머, 상태 머신, 송수신 대기 작업을 진행한다.
- `TpTransmit` (L1573, 선언): 송신 요청을 준비하거나 CAN/TP/진단 응답 전송을 시작한다.
- `TpTxResetChannel` (L1574, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `TpTxSetPriorityBits` (L1578, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `TpTxSetPGN` (L1579, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `TpRxSetPriorityBits` (L1582, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxSetPGN` (L1583, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpTxSetEcuNumber` (L1589, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `TpTxSetResponse` (L1592, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `TpRxResetChannel` (L1597, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxGetStatus` (L1598, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxSetBS` (L1600, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxGetBS` (L1601, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxGetTaType` (L1604, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxSetSTMIN` (L1607, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxGetSTMIN` (L1608, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxGetEcuNumber` (L1611, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpTxPrepareSendImmediate` (L1614, 선언): 송신 요청을 준비하거나 CAN/TP/진단 응답 전송을 시작한다.
- `TpTxSendImmediate` (L1615, 선언): 송신 요청을 준비하거나 CAN/TP/진단 응답 전송을 시작한다.
- `TpTxGetSTminInFrame` (L1616, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `TpRxGetCanChannel` (L1620, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxGetCanBuffer` (L1624, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxSetBufferOverrun` (L1627, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpTxTestForceConfirmationTimeout` (L1630, 선언): 송신 완료 통지나 확인 상태를 처리하고 상위 계층 콜백/플래그를 갱신한다.
- `TpRxTestForceConfirmationTimeout` (L1631, 선언): 송신 완료 통지나 확인 상태를 처리하고 상위 계층 콜백/플래그를 갱신한다.
- `TpRxSetClearToSend` (L1634, 선언): 송신 요청을 준비하거나 CAN/TP/진단 응답 전송을 시작한다.
- `TpRxSetFCStatus` (L1637, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxGetFCStatus` (L1638, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpTransmit` (L1645, 선언): 송신 요청을 준비하거나 CAN/TP/진단 응답 전송을 시작한다.
- `TpTxResetChannel` (L1647, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `TpTxSetPriorityBits` (L1653, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `TpTxSetPGN` (L1654, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `TpRxSetPriorityBits` (L1657, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxSetPGN` (L1658, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpTxSetEcuNumber` (L1664, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `TpRxResetChannel` (L1667, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxGetStatus` (L1668, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxSetBS` (L1671, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxGetBS` (L1672, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxSetSTMIN` (L1675, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxGetSTMIN` (L1676, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxGetTaType` (L1679, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxGetCanChannel` (L1682, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxGetEcuNumber` (L1687, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpTxSetResponse` (L1694, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `TpTxPrepareSendImmediate` (L1699, 선언): 송신 요청을 준비하거나 CAN/TP/진단 응답 전송을 시작한다.
- `TpTxSendImmediate` (L1700, 선언): 송신 요청을 준비하거나 CAN/TP/진단 응답 전송을 시작한다.
- `TpTxGetSTminInFrame` (L1701, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `TpRxGetCanBuffer` (L1704, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxSetBufferOverrun` (L1707, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpTxTestForceConfirmationTimeout` (L1710, 선언): 송신 완료 통지나 확인 상태를 처리하고 상위 계층 콜백/플래그를 갱신한다.
- `TpRxTestForceConfirmationTimeout` (L1711, 선언): 송신 완료 통지나 확인 상태를 처리하고 상위 계층 콜백/플래그를 갱신한다.
- `TpRxSetClearToSend` (L1715, 선언): 송신 요청을 준비하거나 CAN/TP/진단 응답 전송을 시작한다.
- `TpRxSetFCStatus` (L1718, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxGetFCStatus` (L1719, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxSetWaitCorrectSN` (L1723, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpTxSetStrictFlowControlCheck` (L1727, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `TpTxSetCFDelay` (L1731, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `TpRxSetTimeoutCF` (L1735, 선언): 수신 프레임/메시지를 검사하고 버퍼 복사, 핸들 매핑, 상위 계층 통지를 수행한다.
- `TpRxSetTimeoutConfirmation` (L1736, 선언): 송신 완료 통지나 확인 상태를 처리하고 상위 계층 콜백/플래그를 갱신한다.
- `TpTxSetTimeoutFC` (L1737, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `TpTxSetTimeoutConfirmation` (L1738, 선언): 송신 완료 통지나 확인 상태를 처리하고 상위 계층 콜백/플래그를 갱신한다.
- `TpTxStateTask` (L1746, 선언): 주기적으로 호출되어 타이머, 상태 머신, 송수신 대기 작업을 진행한다.
- `TpTxStateTask` (L1749, 선언): 주기적으로 호출되어 타이머, 상태 머신, 송수신 대기 작업을 진행한다.
- `TpTxAllStateTask` (L1750, 선언): 주기적으로 호출되어 타이머, 상태 머신, 송수신 대기 작업을 진행한다.
- `TpRxStateTask` (L1753, 선언): 주기적으로 호출되어 타이머, 상태 머신, 송수신 대기 작업을 진행한다.
- `TpRxStateTask` (L1756, 선언): 주기적으로 호출되어 타이머, 상태 머신, 송수신 대기 작업을 진행한다.
- `TpRxAllStateTask` (L1757, 선언): 주기적으로 호출되어 타이머, 상태 머신, 송수신 대기 작업을 진행한다.
- `TpTxPreTransmit` (L1764, 선언): 송신 요청을 준비하거나 CAN/TP/진단 응답 전송을 시작한다.
- `TpDrvTxPreTransmit` (L1766, 선언): 송신 요청을 준비하거나 CAN/TP/진단 응답 전송을 시작한다.
- `TpTxPreTransmit` (L1770, 선언): 송신 요청을 준비하거나 CAN/TP/진단 응답 전송을 시작한다.
- `TpDrvTxPreTransmit` (L1772, 선언): 송신 요청을 준비하거나 CAN/TP/진단 응답 전송을 시작한다.
- `TpFuncResetChannel` (L2042, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `defined` (L2051, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `defined` (L2057, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `TpFuncGetCanChannel` (L2063, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `TpFuncGetCanBuffer` (L2067, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `TpFuncGetAddressExtension` (L2070, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.

### 5. 필요시 코드 부연 설명

이 파일은 생성 설정/선언 성격이 강해 별도 코드 전문 인용 없이 심볼 표로 역할을 설명했다.

## CAN-FD 마이그레이션 관점

TP 계층은 CAN-FD 전환 효과가 매우 큰 계층이다. Classical CAN에서는 8바이트 payload 때문에 ISO-TP segmentation이 자주 발생하지만, CAN-FD에서는 더 큰 Single Frame과 Consecutive Frame payload를 사용할 수 있다.

- Single Frame 최대 payload 계산을 CAN-FD 여부에 따라 분기한다.
- Consecutive Frame payload 크기를 7바이트 고정으로 두지 않는다.
- First Frame 이후 복사 길이와 sequence number 처리는 유지하되 frame payload capacity를 설정에서 읽는다.
- Block Size, STmin, timeout은 FD data phase 속도와 줄어든 frame 수에 맞춰 재검토한다.
- UDS 계층은 변경하지 않고 TP가 완성 payload만 전달하도록 경계를 유지한다.
