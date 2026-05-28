# Nm 계층 파일별 상세 분석 보고서

- 분석 대상: `Nm` 계층
- 파일 수: 2
- 생성 시각: 2026-05-28 22:37:38
- 분석 방식: C 전처리 매크로, typedef, 전역 데이터 선언, 함수 선언/정의를 자동 추출하고 함수명/주석/계층 역할을 기준으로 한글 역할 설명을 부여함.

## 계층 요약

NM 계층은 Basic Network Management로 CAN 버스오프 감지, 복구 타이밍, 네트워크 상태 전환을 담당한다.

## 파일 목록

- `src/CBD/Nm/nm_basic.c` (80,570 bytes)
- `src/CBD/Nm/nm_basic.h` (16,218 bytes)

## 파일이름 : nm_basic

- 경로: `src/CBD/Nm/nm_basic.c`
- 파일 종류: `.c`
- 파일 크기: 80,570 bytes
- 파일 내용: Basic NM 구현/선언으로 BusOff 감지와 복구, 네트워크 상태 관리를 담당한다.

### 1. .c/.h 파일 이름

- `nm_basic.c`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 39
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 32

#### 주요 매크로
- L146 `NMBASIC_MODULE`
- L474 `NMBASIC_STATE_NO_REQUEST` = `0x00`
- L477 `cNmBasicBusOffSlowNotActive` = `0x00`
- L478 `cNmBasicBusOffSlowActive` = `0x01`
- L481 `cNmBasicBusOffNotDetected` = `0x00`
- L482 `cNmBasicBusOffNotRepaired` = `0x01`
- L485 `cNmBasicBusOffRepaired` = `0x02`
- L490 `cNmBasicBusOffFastToSlowNotDone` = `0x00`
- L491 `cNmBasicBusOffFastToSlowDone` = `0x01`
- L499 `cNmBasicBusOffRecTimer` = `(NmBasicBusOffRecTime_Field[channel] / NmBasicTaskPeriod_Field[channel])`
- L505 `cNmBasicBusOffRecTimer` = `NmBasicBusOffRecTime_Field[channel]`
- L510 `cNmBasicBusOffRecSlowTimer` = `(NmBasicBusOffRecTimeSlow_Field[channel] / NmBasicTaskPeriod_Field[channel])`
- L511 `cNmBasicBusOffFastToSlowTimer` = `(NmBasicBusOffFastToSlow_Field[channel] / NmBasicTaskPeriod_Field[channel])`
- L516 `cNmBasicBusOffRepairedTimer` = `(NmBasicBusOffRepairedTime_Field[channel] / NmBasicTaskPeriod_Field[channel])`
- L521 `cNmBasicInitObject` = `(NmBasicCanInitObject_Field[channel])`
- L528 `cNmBasicBusOffRecTimer` = `(cNmBasicBusOffRecTime / cNmBasicTaskPeriod)`
- L534 `cNmBasicBusOffRecTimer` = `cNmBasicBusOffRecTime`
- L539 `cNmBasicBusOffRecSlowTimer` = `(cNmBasicBusOffRecTimeSlow / cNmBasicTaskPeriod)`
- L540 `cNmBasicBusOffFastToSlowTimer` = `(cNmBasicBusOffChangeFastToSlow / cNmBasicTaskPeriod)`
- L545 `cNmBasicBusOffRepairedTimer` = `(cNmBasicBusOffRepairedTime / cNmBasicTaskPeriod)`
- L555 `cNmBasicCanDrvInInitMode` = `(vuint8)0x00`
- L556 `cNmBasicCanDrvIsBusOff` = `(vuint8)0x01`
- L607 `NmBasicAssert(p,e)` = `{if(!(p)){ApplNmBasicFatalError(channel, (e));}}`
- L608 `NmBasicAssertAlways(e)` = `{ApplNmBasicFatalError(channel, (e));}`
- L611 `NmBasicAssert(p,e)` = `{if(!(p)){ApplNmBasicFatalError((e));}}`
- L612 `NmBasicAssertAlways(e)` = `{ApplNmBasicFatalError((e));}`
- L616 `NmBasicAssert(p,e)`
- L617 `NmBasicAssertAlways(e)`
- L736 `bNmBasicState` = `bNmBasicState[channel]`
- L737 `bNmBasicStateRequest` = `bNmBasicStateRequest[channel]`
- L738 `bNmBasicBusOffRequest` = `bNmBasicBusOffRequest[channel]  /* ESCAN00015987 */`
- L740 `bNmBasicBusOffRecTimer` = `bNmBasicBusOffRecTimer[channel]`
- L743 `wNmBasicBusOffRecTimer` = `wNmBasicBusOffRecTimer[channel]`
- L747 `bNmBasicBusOffRecFastSlowTimer` = `bNmBasicBusOffRecFastSlowTimer[channel]`
- L748 `bNmBasicBusOffRecSlowActive` = `bNmBasicBusOffRecSlowActive[channel]`
- L749 `bNmBasicBusOffFastToSlowChange` = `bNmBasicBusOffFastToSlowChange[channel]`
- L751 `bNmBasicBusOffRepaired` = `bNmBasicBusOffRepaired[channel]`
- L755 `bNmBasicBusOffRepairedTimer` = `bNmBasicBusOffRepairedTimer[channel]`
- L763 `bNmBasicCanDrvIsBusOff` = `bNmBasicCanDrvIsBusOff[channel]`

#### 주요 typedef/구조체
- 없음

#### 주요 전역 변수/테이블 선언
- L626 `static vuint8 bNmBasicState[cNmBasicNrOfChannels];`
- L629 `static vuint8 bNmBasicState;`
- L634 `static vuint8 bNmBasicStateRequest[cNmBasicNrOfChannels];`
- L637 `static vuint8 bNmBasicStateRequest;`
- L643 `static vuint8 bNmBasicBusOffRecTimer[cNmBasicNrOfChannels];`
- L645 `static vuint8 bNmBasicBusOffRecTimer;`
- L650 `static vuint16 wNmBasicBusOffRecTimer[cNmBasicNrOfChannels];`
- L652 `static vuint16 wNmBasicBusOffRecTimer;`
- L661 `static vuint8 bNmBasicBusOffRecFastSlowTimer[cNmBasicNrOfChannels];`
- L664 `static vuint8 bNmBasicBusOffRecSlowActive[cNmBasicNrOfChannels];`
- L667 `static vuint8 bNmBasicBusOffFastToSlowChange[cNmBasicNrOfChannels];`
- L671 `static vuint8 bNmBasicBusOffRepaired[cNmBasicNrOfChannels];`
- L675 `static vuint8 bNmBasicBusOffRecFastSlowTimer;`
- L678 `static vuint8 bNmBasicBusOffRecSlowActive;`
- L681 `static vuint8 bNmBasicBusOffFastToSlowChange;`
- L685 `static vuint8 bNmBasicBusOffRepaired;`
- L698 `static vuint8 bNmBasicBusOffRepairedTimer[cNmBasicNrOfChannels];`
- L701 `static vuint8 bNmBasicBusOffRepairedTimer;`
- L708 `static vuint8 bNmBasicBusOffRequest[cNmBasicNrOfChannels];`
- L711 `static vuint8 bNmBasicBusOffRequest;`
- L721 `static vuint8 bNmBasicCanDrvIsBusOff[cNmBasicNrOfChannels];`
- L723 `static vuint8 bNmBasicCanDrvIsBusOff;`
- L775 `V_MEMROM0 extern V_MEMROM1 tVIdentityMsk V_MEMROM2 CanChannelIdentityAssignment[kCanNumberOfChannels];`
- L1015 `channel = NmBasicCanToNmIndirection[channel];`
- L1112 `channel = NmBasicCanToNmIndirection[channel];`
- L1181 `channel = NmBasicCanToNmIndirection[channel];`
- L1210 `channel = NmBasicCanToNmIndirection[channel];`
- L1244 `channel = NmBasicCanToNmIndirection[channel];`
- L1270 `channel = NmBasicCanToNmIndirection[channel];`
- L1307 `channel = NmBasicCanToNmIndirection[channel];`
- L1343 `channel = NmBasicCanToNmIndirection[channel];`
- L1409 `channel = NmBasicCanToNmIndirection[channel];`

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 14

- L786 `NmBasicDoTheStopHandling(NMBASIC_CHANNEL_NMTYPE_ONLY)` [선언]
- L791 `NmBasicDoBusOffAbort(NMBASIC_CHANNEL_NMTYPE_ONLY)` [선언]
- L808 `NmBasicDoTheStopHandling(NMBASIC_CHANNEL_NMTYPE_ONLY)` [정의]
- L886 `NmBasic_BusSleepModeNotification(ESCAN00012095) OR | NmBasicTask | PRECONDITIONS: none | INPUT PARAMETERS: indexed multi channel: network management channel | single channel: none | RETURN VALUES: none | DESCRIPTION: does all necessary actions if a current BusOff repair | mechanism is aborted by a stop request ****************************************************************************/ static void NmBasicDoBusOffAbort(NMBASIC_CHANNEL_NMTYPE_ONLY)` [정의]
- L961 `NmBasicInitPowerOn(void)` [정의]
- L1012 `NmBasicInit(NMBASIC_CHANNEL_NMTYPE_ONLY)` [정의]
- L1099 `NmBasicCanBusOff(NMBASIC_CHANNEL_CANTYPE_ONLY)` [정의]
- L1178 `NmBasicStart(NMBASIC_CHANNEL_NMTYPE_ONLY)` [정의]
- L1207 `NmBasicStop(NMBASIC_CHANNEL_NMTYPE_ONLY)` [정의]
- L1241 `NmBasicGetNetState(NMBASIC_CHANNEL_NMTYPE_ONLY)` [정의]
- L1267 `ApplCanInit(CAN_CHANNEL_CANTYPE_FIRST CanObjectHandle txHwObjectFirstUsed, CanObjectHandle txHwObjectFirstUnused)` [정의]
- L1304 `ApplCanTxObjStart(CAN_CHANNEL_CANTYPE_FIRST CanObjectHandle txHwObject)` [정의]
- L1340 `ApplCanTxObjConfirmed(CAN_CHANNEL_CANTYPE_FIRST CanObjectHandle txHwObject)` [정의]
- L1406 `NmBasicTask(NMBASIC_CHANNEL_NMTYPE_ONLY)` [정의]

### 4. 각 함수별 역할 설명

- `NmBasicDoTheStopHandling` (L786, 선언): list of internal used functions
- `NmBasicDoBusOffAbort` (L791, 선언): The function 'NmBasicDoBusOffAbort()' is not needed for this algorithm.
- `NmBasicDoTheStopHandling` (L808, 정의): NAME:             NmBasicDoTheStopHandling CALLED BY:        NmBasicTask PRECONDITIONS:    none INPUT PARAMETERS: indexed multi channel: network management channel single channel:        none RETURN VALUES:    none DESCRIPTION:      does all necessary actions 
- `NmBasic_BusSleepModeNotification` (L886, 정의): The function 'NmBasicDoBusOffAbort()' is not needed for this algorithm.
- `NmBasicInitPowerOn` (L961, 정의): NAME:             NmBasicInitPowerOn CALLED BY:        application PRECONDITIONS:    The CAN interrupts must be disabled. INPUT PARAMETERS: none RETURN VALUES:    none DESCRIPTION:      init node management
- `NmBasicInit` (L1012, 정의): NAME:             NmBasicInit CALLED BY:        node management / application PRECONDITIONS:    The CAN interrupts must be disabled. INPUT PARAMETERS: indexed multi channel: network management channel single channel:        none RETURN VALUES:    none DESCRIPT
- `NmBasicCanBusOff` (L1099, 정의): NAME:             NmBasicCanBusOff CALLED BY:        CAN driver PRECONDITIONS:    The node management must be initialized. INPUT PARAMETERS: indexed multi channel: network management channel single channel:        none RETURN VALUES:    none DESCRIPTION:      
- `NmBasicStart` (L1178, 정의): NAME:             NmBasicStart CALLED BY:        application PRECONDITIONS:    The node management must be initialized. INPUT PARAMETERS: indexed multi channel: network management channel single channel:        none RETURN VALUES:    none DESCRIPTION:      set
- `NmBasicStop` (L1207, 정의): NAME:             NmBasicStop CALLED BY:        application PRECONDITIONS:    The node management must be initialized. INPUT PARAMETERS: indexed multi channel: network management channel single channel:        none RETURN VALUES:    none DESCRIPTION:      set 
- `NmBasicGetNetState` (L1241, 정의): NAME:             NmBasicGetNetState CALLED BY:        application PRECONDITIONS:    The node management must be initialized. INPUT PARAMETERS: indexed multi channel: network management channel single channel:        none RETURN VALUES:    current state of the
- `ApplCanInit` (L1267, 정의): NAME:             ApplCanInit CALLED BY:        CAN driver PRECONDITIONS:    initialization INPUT PARAMETERS: CAN_CHANNEL_CANTYPE_FIRST = defined to nothing in single channel system CAN_CHANNEL_CANTYPE_FIRST = channel number in multi channel system txHwObjectF
- `ApplCanTxObjStart` (L1304, 정의): NAME:             ApplCanTxObjStart CALLED BY:        CAN driver PRECONDITIONS:    initialization INPUT PARAMETERS: CAN_CHANNEL_CANTYPE_FIRST = defined to nothing in single channel system CAN_CHANNEL_CANTYPE_FIRST = channel number in multi channel system txHwO
- `ApplCanTxObjConfirmed` (L1340, 정의): NAME:             ApplCanTxObjConfirmed CALLED BY:        CAN driver PRECONDITIONS:    initialization INPUT PARAMETERS: CAN_CHANNEL_CANTYPE_FIRST = defined to nothing in single channel system CAN_CHANNEL_CANTYPE_FIRST = channel number in multi channel system t
- `NmBasicTask` (L1406, 정의): NAME:             NmBasicTask CALLED BY:        application PRECONDITIONS:    The node management must be initialized. INPUT PARAMETERS: indexed multi channel: network management channel single channel:        none RETURN VALUES:    none DESCRIPTION:      node

### 5. 필요시 코드 부연 설명

대표 함수 원형 수준의 코드 맥락이다. 전체 구현은 원본 파일이 크므로 함수명/역할 중심으로 분석했다.

```c
// src/CBD/Nm/nm_basic.c:786
static void NmBasicDoTheStopHandling(NMBASIC_CHANNEL_NMTYPE_ONLY)
```
```c
// src/CBD/Nm/nm_basic.c:791
static void NmBasicDoBusOffAbort(NMBASIC_CHANNEL_NMTYPE_ONLY)
```
```c
// src/CBD/Nm/nm_basic.c:808
static void NmBasicDoTheStopHandling(NMBASIC_CHANNEL_NMTYPE_ONLY)
```
```c
// src/CBD/Nm/nm_basic.c:886
| CALLED BY: NmBasic_BusSleepModeNotification(ESCAN00012095) OR | NmBasicTask | PRECONDITIONS: none | INPUT PARAMETERS: indexed multi channel: network management channel | single channel: none | RETURN VALUES: none | DESCRIPTION: does all necessary actions if a current BusOff repair | mechanism is aborted by a stop request ****************************************************************************/ static void NmBasicDoBusOffAbort(NMBASIC_CHANNEL_NMTYPE_ONLY)
```
```c
// src/CBD/Nm/nm_basic.c:961
void NmBasicInitPowerOn(void)
```
```c
// src/CBD/Nm/nm_basic.c:1012
void NmBasicInit(NMBASIC_CHANNEL_NMTYPE_ONLY)
```
```c
// src/CBD/Nm/nm_basic.c:1099
void NmBasicCanBusOff(NMBASIC_CHANNEL_CANTYPE_ONLY)
```
```c
// src/CBD/Nm/nm_basic.c:1178
void NmBasicStart(NMBASIC_CHANNEL_NMTYPE_ONLY)
```

## 파일이름 : nm_basic

- 경로: `src/CBD/Nm/nm_basic.h`
- 파일 종류: `.h`
- 파일 크기: 16,218 bytes
- 파일 내용: Basic NM 구현/선언으로 BusOff 감지와 복구, 네트워크 상태 관리를 담당한다.

### 1. .c/.h 파일 이름

- `nm_basic.h`

### 2. 파일 내 변수, 매크로 등 전역 심볼

- 매크로 수: 56
- typedef/struct/enum 후보 수: 0
- 전역 변수/테이블 선언 후보 수: 11

#### 주요 매크로
- L91 `NMBASIC_IMPL_H`
- L101 `NM_PWRTRAINBASIC_VERSION` = `0x0115`
- L102 `NM_PWRTRAINBASIC_RELEASE_VERSION` = `0x05`
- L104 `NM_TYPE_NMBASIC`
- L107 `NMBASIC_STATE_POWER_OFF` = `0x00`
- L108 `NMBASIC_STATE_TX_ERROR` = `0x01 /* ESCAN00019119 */`
- L109 `NMBASIC_STATE_STOP` = `0x02`
- L110 `NMBASIC_STATE_RUN` = `0x03`
- L111 `NMBASIC_STATE_BUSOFF` = `0x04`
- L117 `NMBASIC_CHANNEL_NMTYPE_ONLY` = `CanChannelHandle channel`
- L118 `NMBASIC_CHANNEL_NMTYPE_FIRST` = `CanChannelHandle channel,`
- L119 `NMBASIC_CHANNEL_NMPARA_ONLY` = `channel`
- L120 `NMBASIC_CHANNEL_NMPARA_FIRST` = `channel,`
- L123 `NMBASIC_CHANNEL_CANTYPE_ONLY` = `CanChannelHandle channel`
- L124 `NMBASIC_CHANNEL_CANTYPE_FIRST` = `CanChannelHandle channel,`
- L125 `NMBASIC_CHANNEL_CANPARA_ONLY` = `NmBasicNmToCanIndirection[channel]`
- L126 `NMBASIC_CHANNEL_CANPARA_FIRST` = `NmBasicNmToCanIndirection[channel],`
- L128 `NMBASIC_CHANNEL_CANTYPE_ONLY` = `NMBASIC_CHANNEL_NMTYPE_ONLY`
- L129 `NMBASIC_CHANNEL_CANTYPE_FIRST` = `NMBASIC_CHANNEL_NMTYPE_FIRST`
- L130 `NMBASIC_CHANNEL_CANPARA_ONLY` = `NMBASIC_CHANNEL_NMPARA_ONLY`
- L131 `NMBASIC_CHANNEL_CANPARA_FIRST` = `NMBASIC_CHANNEL_NMPARA_FIRST`
- L134 `NMBASIC_CHANNEL_APPLTYPE_ONLY` = `NMBASIC_CHANNEL_NMTYPE_ONLY`
- L135 `NMBASIC_CHANNEL_APPLTYPE_FIRST` = `NMBASIC_CHANNEL_NMTYPE_FIRST`
- L136 `NMBASIC_CHANNEL_APPLPARA_ONLY` = `NMBASIC_CHANNEL_CANPARA_ONLY`
- L137 `NMBASIC_CHANNEL_APPLPARA_FIRST` = `NMBASIC_CHANNEL_CANPARA_FIRST`
- L142 `NMBASIC_CHANNEL_NMTYPE_ONLY` = `void`
- L143 `NMBASIC_CHANNEL_NMTYPE_FIRST`
- L144 `NMBASIC_CHANNEL_NMPARA_ONLY`
- L145 `NMBASIC_CHANNEL_NMPARA_FIRST`
- L148 `NMBASIC_CHANNEL_CANTYPE_ONLY` = `CanChannelHandle channel`
- L149 `NMBASIC_CHANNEL_CANTYPE_FIRST` = `CanChannelHandle channel,`
- L152 `NMBASIC_CHANNEL_CANPARA_ONLY` = `NMBASIC_CAN_CHANNEL`
- L153 `NMBASIC_CHANNEL_CANPARA_FIRST` = `NMBASIC_CAN_CHANNEL,`
- L158 `NMBASIC_CHANNEL_CANPARA_ONLY` = `0`
- L159 `NMBASIC_CHANNEL_CANPARA_FIRST` = `0,`
- L163 `NMBASIC_CHANNEL_CANTYPE_ONLY` = `NMBASIC_CHANNEL_NMTYPE_ONLY`
- L164 `NMBASIC_CHANNEL_CANTYPE_FIRST` = `NMBASIC_CHANNEL_NMTYPE_FIRST`
- L165 `NMBASIC_CHANNEL_CANPARA_ONLY` = `NMBASIC_CHANNEL_NMPARA_ONLY`
- L166 `NMBASIC_CHANNEL_CANPARA_FIRST` = `NMBASIC_CHANNEL_NMPARA_FIRST`
- L169 `NMBASIC_CHANNEL_APPLTYPE_ONLY` = `NMBASIC_CHANNEL_NMTYPE_ONLY`
- L170 `NMBASIC_CHANNEL_APPLTYPE_FIRST` = `NMBASIC_CHANNEL_NMTYPE_FIRST`
- L171 `NMBASIC_CHANNEL_APPLPARA_ONLY`
- L172 `NMBASIC_CHANNEL_APPLPARA_FIRST`
- L176 `NMBASICVAR_CH_DEF(var)` = `var[cNmBasicNrOfChannels] /* PRQA S 3410 */`
- L177 `NMBASICVAR(var)` = `var[channel]              /* PRQA S 3410 */`
- L179 `NMBASICVAR_CH_DEF(var)` = `var /* PRQA S 3410 */`
- L180 `NMBASICVAR(var)` = `var /* PRQA S 3410 */`
- L190 `kNmBasicErrStateUndefined` = `0x01 /* an undefined state is detected in the NM state machine */`
- L193 `kNmBasicErrInitObjOutOfRange` = `0x02 /* the initialization object is out of range */`
- L196 `kNmBasicErrBusOffFastTimeOutOfRange` = `0x03 /* the fast BusOff recovery time is out of range */`
- L200 `kNmBasicErrBusOffSlowTimeOutOfRange` = `0x04 /* the slow BusOff recovery time is out of range */`
- L201 `kNmBasicErrBusOffFastSlowTimeOutOfRange` = `0x05 /* the change-from-fast-to-slow BusOff recovery time is out of range */`
- L202 `kNmBasicErrFastLargerSlowTime` = `0x06`
- L204 `kNmBasicErrBusOffRepTimeOutOfRange` = `0x07 /* the BusOff repaired time is out of range */`
- L205 `kNmBasicErrBusOffRepairTimeTooSmall` = `0x08 /* the fast BusOff repaired time is too small */`
- L211 `kNmBasicErrWrongChannelHandle` = `0x0D /* NM Basic API is called for a CAN channel which does not belong to the current identity */`

#### 주요 typedef/구조체
- 없음

#### 주요 전역 변수/테이블 선언
- L225 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 kNmBasicMainVersion;`
- L226 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 kNmBasicSubVersion;`
- L227 `V_MEMROM0 extern V_MEMROM1 vuint8 V_MEMROM2 kNmBasicReleaseVersion;`
- L232 `extern CanChannelHandle MEMORY_ROM NmBasicCanToNmIndirection[kCanNumberOfChannels];`
- L233 `extern CanChannelHandle MEMORY_ROM NmBasicNmToCanIndirection[cNmBasicNrOfChannels];`
- L238 `extern V_MEMROM0 V_MEMROM1 vuint8 V_MEMROM2 NmBasicCanInitObject_Field[cNmBasicNrOfChannels];`
- L241 `extern V_MEMROM0 V_MEMROM1 vuint8 V_MEMROM2 NmBasicTaskPeriod_Field[cNmBasicNrOfChannels];`
- L245 `extern V_MEMROM0 V_MEMROM1 vuint16 V_MEMROM2 NmBasicBusOffRecTime_Field[cNmBasicNrOfChannels];`
- L250 `extern V_MEMROM0 V_MEMROM1 vuint16 V_MEMROM2 NmBasicBusOffRecTimeSlow_Field[cNmBasicNrOfChannels];`
- L253 `extern V_MEMROM0 V_MEMROM1 vuint16 V_MEMROM2 NmBasicBusOffFastToSlow_Field[cNmBasicNrOfChannels];`
- L258 `extern V_MEMROM0 V_MEMROM1 vuint16 V_MEMROM2 NmBasicBusOffRepairedTime_Field[cNmBasicNrOfChannels];`

### 3. 파일 내 함수명

- 함수 선언/정의 후보 수: 17

- L267 `NmBasicInitPowerOn(void)` [선언]
- L268 `NmBasicInit(NMBASIC_CHANNEL_NMTYPE_ONLY)` [선언]
- L269 `NmBasicTask(NMBASIC_CHANNEL_NMTYPE_ONLY)` [선언]
- L270 `NmBasicStart(NMBASIC_CHANNEL_NMTYPE_ONLY)` [선언]
- L271 `NmBasicStop(NMBASIC_CHANNEL_NMTYPE_ONLY)` [선언]
- L272 `NmBasicCanBusOff(NMBASIC_CHANNEL_CANTYPE_ONLY)` [선언]
- L273 `NmBasicGetNetState(NMBASIC_CHANNEL_NMTYPE_ONLY)` [선언]
- L281 `ApplNmBasicBusOffStart(NMBASIC_CHANNEL_APPLTYPE_ONLY)` [선언]
- L282 `ApplNmBasicBusOffEnd(NMBASIC_CHANNEL_APPLTYPE_ONLY)` [선언]
- L285 `ApplNmBasicFirstBusOffSlow(NMBASIC_CHANNEL_APPLTYPE_ONLY)` [선언]
- L286 `ApplNmBasicBusOffRestart(NMBASIC_CHANNEL_APPLTYPE_ONLY)` [선언]
- L291 `ApplNmBasicEnabledCom(NMBASIC_CHANNEL_APPLTYPE_ONLY)` [선언]
- L292 `ApplNmBasicSwitchTransceiverOn(NMBASIC_CHANNEL_APPLTYPE_ONLY)` [선언]
- L294 `ApplNmBasicDisabledCom(NMBASIC_CHANNEL_APPLTYPE_ONLY)` [선언]
- L295 `ApplNmBasicSwitchTransceiverOff(NMBASIC_CHANNEL_APPLTYPE_ONLY)` [선언]
- L299 `ApplNmBasicFatalError(NMBASIC_CHANNEL_APPLTYPE_FIRST vuint8 error)` [선언]
- L301 `ApplNmBasicFatalError(vuint8 error)` [선언]

### 4. 각 함수별 역할 설명

- `NmBasicInitPowerOn` (L267, 선언): 전원 인가 직후 한 번 수행되는 모듈 전역 초기화를 담당한다.
- `NmBasicInit` (L268, 선언): 모듈 런타임 상태, 버퍼, 카운터, 하드웨어/생성 파라미터를 초기화한다.
- `NmBasicTask` (L269, 선언): 주기적으로 호출되어 타이머, 상태 머신, 송수신 대기 작업을 진행한다.
- `NmBasicStart` (L270, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `NmBasicStop` (L271, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `NmBasicCanBusOff` (L272, 선언): CAN 버스오프 감지, 복구 타이밍, 통신 중단/재시작 상태를 처리한다.
- `NmBasicGetNetState` (L273, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `ApplNmBasicBusOffStart` (L281, 선언): CAN 버스오프 감지, 복구 타이밍, 통신 중단/재시작 상태를 처리한다.
- `ApplNmBasicBusOffEnd` (L282, 선언): CAN 버스오프 감지, 복구 타이밍, 통신 중단/재시작 상태를 처리한다.
- `ApplNmBasicFirstBusOffSlow` (L285, 선언): CAN 버스오프 감지, 복구 타이밍, 통신 중단/재시작 상태를 처리한다.
- `ApplNmBasicBusOffRestart` (L286, 선언): CAN 버스오프 감지, 복구 타이밍, 통신 중단/재시작 상태를 처리한다.
- `ApplNmBasicEnabledCom` (L291, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `ApplNmBasicSwitchTransceiverOn` (L292, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `ApplNmBasicDisabledCom` (L294, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `ApplNmBasicSwitchTransceiverOff` (L295, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `ApplNmBasicFatalError` (L299, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.
- `ApplNmBasicFatalError` (L301, 선언): 파일 내 이름/위치상 해당 모듈의 내부 보조 처리 또는 생성 파라미터 접근을 담당한다.

### 5. 필요시 코드 부연 설명

이 파일은 생성 설정/선언 성격이 강해 별도 코드 전문 인용 없이 심볼 표로 역할을 설명했다.

## CAN-FD 마이그레이션 관점

NM 계층은 payload 크기보다 controller error state와 BusOff 복구 정책에 영향을 받는다.

- CAN-FD controller의 BusOff, error passive, error warning 보고 방식이 기존 CAN Driver callback과 호환되는지 확인한다.
- BRS/data phase 오류가 BusOff 복구 정책에 어떤 영향을 주는지 HAL 문서 기준으로 점검한다.
- 복구 후 CAN-FD timing, payload size, mailbox 설정이 다시 적용되는지 확인한다.
