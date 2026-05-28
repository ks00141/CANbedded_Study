# CBD 미들웨어 C 구현 학습 로드맵

## 1. 목표

이 로드맵의 목표는 `src/CBD`에 포함된 Vector CANbedded 계열 미들웨어 묶음을 학습하여, 동일한 구조의 C 언어 기반 미들웨어를 단계적으로 재구현할 수 있는 이해 순서와 구현 순서를 잡는 것이다. 재구현 학습의 타깃 MCU는 STMicroelectronics `SPC58xC` 시리즈로 설정한다.

`SPC58xC`는 차량용 32-bit Power Architecture 계열 MCU 제품군으로, 파생 제품에 따라 CAN/CAN-FD 컨트롤러, 자동차용 안전/진단 기능, 플래시/RAM, 타이머, 인터럽트 컨트롤러, 시스템 보호 기능을 제공한다. 따라서 학습용 미들웨어는 PC mock 환경에서 먼저 구현하되, 최종 구조는 `SPC58xC`의 CAN-FD 가능 CAN 컨트롤러, interrupt vector, big-endian/정렬 이슈, 메모리 배치, peripheral clock 설정을 고려해 설계한다.

대상 미들웨어는 다음 계층으로 구성된다.

```text
Application / Diagnostic Service Hooks
  |
  |-- CANdesc / UDS Diagnostic
  |
  |-- ISO-TP Transport Protocol
  |
  |-- IL, Interaction Layer
  |
  |-- CCL / NM
  |
  |-- CAN Driver
  |
  |-- CAN-FD Migration Layer / FD-capable CAN HAL
  |
  |-- Common Types / VStdLib
  |
MCU / CAN Hardware
```

처음부터 전체 기능을 동일하게 구현하려고 하기보다, 타입과 설정 모델을 먼저 만들고 CAN 송수신, IL, ISO-TP, UDS, NM/CCL 순서로 기능을 확장하는 방식을 추천한다. 마지막 단계에서는 기존 HS-CAN, 즉 Classical CAN 8바이트 payload 기반 구조를 CAN-FD 64바이트 payload와 FD bit-rate switching 구조로 확장한다.

## 2. 전체 학습 순서 요약

| 순서 | 학습 계층 | 핵심 목표 | 우선 참조 파일 |
|---|---|---|---|
| 1 | Common / VStdLib | 공통 타입, 메모리 매크로, 기본 유틸 이해 | `Common/v_def.h`, `VStdLib/vstdlib.*` |
| 2 | Generated Config | 전체 모듈 활성화 상태와 정적 설정 이해 | `Gen/v_cfg.h`, `Gen/*_cfg.h` |
| 3 | CAN Data Model | CAN 핸들, ID, DLC, 메시지 버퍼 구조 이해 | `Gen/can_par.*`, `Gen/drv_par.*` |
| 4 | CAN Driver | CAN 초기화, 송신, 수신, ISR, BusOff 이해 | `Can/can_inc.h`, `Can/can_def.h`, `Can/can_drv.c` |
| 5 | IL | 신호 put/get, 주기 송신, 수신 분배 이해 | `Il/*`, `Gen/il_par.*` |
| 6 | ISO-TP | 진단 다중 프레임 송수신 상태 머신 이해 | `Tp/tpmc.*`, `Gen/tp_cfg.h` |
| 7 | UDS / CANdesc | UDS 서비스 디스패처와 서비스 훅 이해 | `Gen/desc.*`, `Gen/appdesc.*` |
| 8 | NM | BusOff 복구와 네트워크 상태 관리 이해 | `Nm/nm_basic.*`, `Gen/nmb_cfg.h` |
| 9 | CCL | 통신 요청/해제, 진단 통신 제어 통합 이해 | `Ccl/*`, `Gen/ccl_cfg.h` |
| 10 | CAN-FD Migration | HS-CAN 구조를 CAN-FD 데이터 길이, DLC, BRS, ISO-TP over CAN-FD로 확장 | 기존 전체 계층, 신규 FD 설정 |

## 2.1. 타깃 MCU: SPC58xC 적용 관점

`SPC58xC` 시리즈를 대상으로 재구현할 때는 다음 관점을 처음부터 염두에 둔다.

- CPU/컴파일러 관점: Power Architecture 기반 automotive MCU이므로 정렬, endian, interrupt ABI, compiler intrinsic 사용 가능성을 고려한다.
- CAN 주변장치 관점: 파생 제품에 따라 Classical CAN과 CAN-FD 지원 컨트롤러 구성이 다를 수 있으므로, CAN Driver는 HAL 추상화 계층을 둔다.
- 메모리 관점: Vector 코드의 `V_MEMROM`, `V_MEMRAM` 같은 메모리 클래스 매크로는 SPC58xC의 flash/RAM section 배치로 연결될 수 있다.
- 인터럽트 관점: CAN Rx/Tx/BusOff interrupt는 ISR에서 최소 처리 후 task/context 함수로 넘기는 구조가 안전하다.
- 타이밍 관점: CAN-FD에서는 arbitration phase와 data phase bitrate가 분리될 수 있으므로, CAN bit timing 설정을 Driver 설정 구조에 포함한다.
- 안전성 관점: ECU 미들웨어에서는 invalid handle, DLC mismatch, buffer overflow, BusOff 반복, diagnostic session timeout을 모두 방어적으로 처리해야 한다.

## 3. 1단계: 공통 타입과 유틸리티 학습

### 참조 파일

- `src/CBD/Common/v_def.h`
- `src/CBD/VStdLib/vstdlib.h`
- `src/CBD/VStdLib/vstdlib.c`

### 학습 목표

- `vuint8`, `vuint16`, `vuint32`, `vsint*` 타입 체계를 이해한다.
- `V_MEMROM`, `V_MEMRAM`, `MEMORY_NEAR`, `MEMORY_FAR` 같은 메모리 클래스 매크로의 목적을 이해한다.
- 임베디드 코드에서 컴파일러/MCU별 차이를 매크로로 흡수하는 방식을 파악한다.
- `VStdMemSetInternal`, `VStdMemClrInternal`, `VStdRamMemCpy`, `VStdGlobalIntDisable`, `VStdGlobalIntRestore` 같은 기반 함수를 이해한다.

### 구현 연습

최소 구현 파일을 다음처럼 구성한다.

```text
middleware/common/platform_types.h
middleware/common/memmap.h
middleware/common/vstdlib.h
middleware/common/vstdlib.c
```

먼저 아래 기능만 구현한다.

- 고정폭 정수 타입 alias
- boolean 타입
- ROM/RAM 메모리 클래스 매크로는 빈 매크로로 정의
- `memset`, `memcpy` wrapper
- interrupt lock/unlock stub

### 완료 기준

- 공통 타입 헤더만 포함해도 모든 상위 모듈이 타입 정의를 참조할 수 있어야 한다.
- PC 테스트 환경에서는 메모리 클래스 매크로가 컴파일을 방해하지 않아야 한다.

## 4. 2단계: 생성 설정 구조 학습

### 참조 파일

- `src/CBD/Gen/v_cfg.h`
- `src/CBD/Gen/can_cfg.h`
- `src/CBD/Gen/il_cfg.h`
- `src/CBD/Gen/tp_cfg.h`
- `src/CBD/Gen/nmb_cfg.h`
- `src/CBD/Gen/ccl_cfg.h`

### 학습 목표

- 전체 미들웨어가 기능 스위치 매크로 기반으로 구성되는 방식을 이해한다.
- 채널 수, Tx/Rx 오브젝트 수, TP 채널 수, 주기 시간, 타임아웃 값을 정적 설정으로 관리하는 방식을 파악한다.
- 프로젝트별 설정 파일과 공통 구현 파일이 분리되는 구조를 이해한다.

### 구현 연습

다음 파일을 만든다고 가정한다.

```text
middleware/config/v_cfg.h
middleware/config/can_cfg.h
middleware/config/il_cfg.h
middleware/config/tp_cfg.h
middleware/config/nm_cfg.h
middleware/config/ccl_cfg.h
```

초기에는 아래 설정만 둔다.

```c
#define MW_NUM_CAN_CHANNELS 1
#define MW_NUM_CAN_TX_OBJECTS 16
#define MW_NUM_CAN_RX_OBJECTS 7

#define MW_ENABLE_CAN_DRIVER
#define MW_ENABLE_IL
#define MW_ENABLE_ISOTP
#define MW_ENABLE_UDS
#define MW_ENABLE_NM
#define MW_ENABLE_CCL
```

### 완료 기준

- 모든 모듈은 직접 숫자를 박지 않고 설정 매크로를 통해 크기와 기능 여부를 참조한다.
- 설정 파일을 바꾸면 테이블 크기나 기능 스위치가 바뀌는 구조가 된다.

## 5. 3단계: CAN 메시지 데이터 모델 학습

### 참조 파일

- `src/CBD/Gen/can_par.h`
- `src/CBD/Gen/can_par.c`
- `src/CBD/Gen/drv_par.h`
- `src/CBD/Gen/drv_par.c`

### 학습 목표

- CAN Tx/Rx 핸들이 정수 상수로 정의되는 방식을 이해한다.
- CAN ID, DLC, 데이터 버퍼 포인터, confirmation callback, precopy callback 테이블 구조를 파악한다.
- DBC 신호가 C 구조체와 union 버퍼로 생성되는 방식을 이해한다.

### 구현 연습

다음 개념을 구현한다.

```c
typedef uint8_t CanTxHandle;
typedef uint8_t CanRxHandle;

typedef struct
{
    uint16_t id;
    uint8_t dlc;
    uint8_t *data;
    uint8_t send_group;
} CanTxObjectConfig;

typedef struct
{
    uint16_t id;
    uint8_t dlc;
    uint8_t *data;
} CanRxObjectConfig;
```

메시지 버퍼는 우선 단순 배열로 시작한다.

```c
extern uint8_t IPSU_01_200ms[8];
extern uint8_t IPSU_GST[8];
extern uint8_t CGW_01_200ms[8];
```

### 완료 기준

- Tx/Rx 메시지를 핸들로 접근할 수 있다.
- 핸들에서 ID, DLC, 데이터 포인터를 찾을 수 있다.
- 이후 CAN Driver가 이 테이블만 보고 송수신할 수 있다.

## 6. 4단계: CAN Driver 학습 및 최소 구현

### 참조 파일

- `src/CBD/Can/can_inc.h`
- `src/CBD/Can/can_def.h`
- `src/CBD/Can/can_drv.c`
- `src/CBD/Gen/can_cfg.h`
- `src/CBD/Gen/can_par.*`

### 먼저 볼 함수

- `CanInitPowerOn`
- `CanInit`
- `CanTransmit`
- `CanMsgTransmit`
- `CanRxFullCANTask`
- `CanRxBasicCANTask`
- `CanHL_ReceivedRxHandle`
- `CanHL_TxConfirmation`
- `CanSetActive`
- `CanSetPassive`
- `CanOnline`
- `CanOffline`
- `CanSleep`
- `CanWakeUp`
- `CanStart`
- `CanStop`

### 학습 목표

- CAN Driver가 하드웨어 오브젝트와 논리 Tx/Rx 핸들을 분리하는 방식을 이해한다.
- 송신 큐, confirmation flag, callback 구조를 이해한다.
- 수신 시 CAN ID와 Rx object를 매칭하고 상위 계층 callback으로 전달하는 방식을 파악한다.
- BusOff 발생 시 NM으로 연결되는 구조를 이해한다.

### 구현 연습

초기에는 실제 CAN 하드웨어 없이 mock driver로 시작한다.

```text
middleware/can/can.h
middleware/can/can.c
middleware/can/can_types.h
middleware/can/can_cfg.h
middleware/can/can_par.c
```

최소 API:

```c
void Can_Init(void);
uint8_t Can_Transmit(CanTxHandle tx);
void Can_RxIndication(uint16_t id, const uint8_t *data, uint8_t dlc);
void Can_MainFunction(void);
void Can_SetOnline(void);
void Can_SetOffline(void);
```

### 완료 기준

- `Can_Transmit(CanTxIPSU_01_200ms)` 호출 시 설정 테이블의 ID/DLC/data를 사용한다.
- `Can_RxIndication()`으로 들어온 ID를 Rx 테이블에서 찾아 상위 callback을 호출한다.
- BusOff callback을 등록할 수 있다.

## 7. 5단계: IL 학습 및 신호 계층 구현

### 참조 파일

- `src/CBD/Il/il_inc.h`
- `src/CBD/Il/il_def.h`
- `src/CBD/Il/il.c`
- `src/CBD/Gen/il_cfg.h`
- `src/CBD/Gen/il_par.h`
- `src/CBD/Gen/il_par.c`

### 먼저 볼 함수

- `IlInit`
- `IlTxStart`
- `IlTxStop`
- `IlRxStart`
- `IlRxStop`
- `IlTxTask`
- `IlRxTask`
- `IlCanGenericPrecopy`
- `IlPutTx...`
- `IlGetRx...`

### 학습 목표

- IL이 CAN 메시지 버퍼와 신호 accessor를 연결하는 방식을 이해한다.
- 주기 송신 카운터와 이벤트 송신 구조를 이해한다.
- Rx 메시지가 들어왔을 때 데이터 버퍼 갱신과 indication 흐름을 파악한다.

### 구현 연습

처음에는 다음 기능만 구현한다.

- Tx 신호 put 함수
- Rx 신호 get 함수
- 주기 송신 task
- CAN Rx callback에서 IL buffer 갱신

예시:

```c
void IlPutTxIPSU_Recline_Current_Pos(uint32_t value);
uint16_t IlGetRxCGW_Recline_Min_VrtLmt_Value(void);
void Il_TxTask_10ms(void);
uint8_t Il_CanRxIndication(CanRxHandle rx, const uint8_t *data, uint8_t dlc);
```

### 완료 기준

- 애플리케이션이 `IlPutTx...()`로 값을 쓰면 대응 CAN Tx 버퍼가 갱신된다.
- `Il_TxTask_10ms()`가 주기 메시지를 CAN Driver에 송신 요청한다.
- 수신 메시지 데이터가 Rx 버퍼에 저장되고 `IlGetRx...()`로 읽힌다.

## 8. 6단계: ISO-TP 학습 및 구현

### 참조 파일

- `src/CBD/Tp/tpmc.h`
- `src/CBD/Tp/tpmc.c`
- `src/CBD/Gen/tp_cfg.h`
- `src/CBD/Gen/tp_par.c`

### 먼저 볼 함수

- `TpInitPowerOn`
- `TpInit`
- `TpTask`
- `TpRxTask`
- `TpTxTask`
- `TpPrecopy`
- `TpTransmit`
- `TpTxPreTransmit`
- `TpConfirmation`
- `TpRxResetChannel`
- `TpTxResetChannel`

### 학습 목표

- ISO 15765-2의 프레임 종류를 이해한다.
  - Single Frame
  - First Frame
  - Consecutive Frame
  - Flow Control
- 수신 재조립 버퍼와 송신 분할 버퍼의 상태 머신을 이해한다.
- STmin, Block Size, timeout, padding 처리 방식을 파악한다.
- TP가 CANdesc callback과 연결되는 지점을 이해한다.

### 구현 순서

1. Single Frame 수신
2. Single Frame 송신
3. First Frame 수신 후 Flow Control 송신
4. Consecutive Frame 수신 재조립
5. Multi-frame 송신
6. Flow Control 수신 처리
7. timeout 처리

### 완료 기준

- 7바이트 이하 UDS 요청/응답을 Single Frame으로 처리할 수 있다.
- 8바이트 초과 요청을 재조립해 상위 UDS 계층에 전달할 수 있다.
- 8바이트 초과 응답을 다중 프레임으로 분할 송신할 수 있다.

## 9. 7단계: UDS / CANdesc 학습 및 구현

### 참조 파일

- `src/CBD/Gen/desc.h`
- `src/CBD/Gen/desc.c`
- `src/CBD/Gen/appdesc.h`
- `src/CBD/Gen/appdesc.c`

### 먼저 볼 함수

- `DescInitPowerOn`
- `DescInit`
- `DescTask`
- `DescGetBuffer`
- `DescPhysReqInd`
- `DescFuncReqInd`
- `DescDispatcher`
- `DescProcessingDone`
- `DescSetNegResponse`
- `DescGetStateSession`
- `DescSetStateSession`
- `DescSecurityAccessSeedReady`
- `DescSecurityAccessKeyChecked`

### 학습 목표

- UDS 요청이 TP에서 올라와 진단 컨텍스트에 저장되는 흐름을 이해한다.
- 서비스 ID 기반 디스패처 구조를 이해한다.
- 서비스별 session/security 조건 검사 방식을 파악한다.
- Positive Response, Negative Response, Response Pending 처리 방식을 이해한다.
- `appdesc.c`가 실제 ECU별 서비스 훅이라는 점을 이해한다.

### 우선 구현할 UDS 서비스

1. `0x10 DiagnosticSessionControl`
2. `0x3E TesterPresent`
3. `0x22 ReadDataByIdentifier`
4. `0x27 SecurityAccess`
5. `0x28 CommunicationControl`
6. `0x31 RoutineControl`

### 완료 기준

- TP에서 받은 요청을 UDS dispatcher로 넘길 수 있다.
- 지원하지 않는 서비스에 대해 `0x7F SID NRC` 형식의 부정 응답을 보낼 수 있다.
- `0x10`, `0x3E`, `0x22`는 최소 정상 응답을 생성할 수 있다.
- 서비스별 애플리케이션 callback 테이블을 분리할 수 있다.

## 10. 8단계: NM 학습 및 BusOff 복구 구현

### 참조 파일

- `src/CBD/Nm/nm_basic.h`
- `src/CBD/Nm/nm_basic.c`
- `src/CBD/Gen/nmb_cfg.h`
- `src/CBD/Gen/nmb_par.*`

### 먼저 볼 함수

- `NmBasicInitPowerOn`
- `NmBasicInit`
- `NmBasicCanBusOff`
- `NmBasicTask`
- `NmBasicStart`
- `NmBasicStop`
- `NmBasicGetNetState`

### 학습 목표

- CAN Driver의 BusOff callback이 NM으로 연결되는 구조를 이해한다.
- 빠른 복구/느린 복구 타이머 구조를 이해한다.
- BusOff 중 통신 중지와 복구 후 재시작 흐름을 파악한다.

### 구현 연습

상태를 단순화한다.

```c
typedef enum
{
    NM_STATE_NORMAL,
    NM_STATE_BUSOFF_FAST_RECOVERY,
    NM_STATE_BUSOFF_SLOW_RECOVERY,
    NM_STATE_STOPPED
} NmState;
```

### 완료 기준

- CAN Driver가 BusOff를 보고하면 NM 상태가 BusOff 복구 상태로 전환된다.
- 주기 task에서 복구 타이머가 감소한다.
- 복구 완료 시 CAN 재초기화 또는 online 전환 callback을 호출한다.

## 11. 9단계: CCL 학습 및 통합 제어 구현

### 참조 파일

- `src/CBD/Ccl/ccl.h`
- `src/CBD/Ccl/ccl_inc.h`
- `src/CBD/Ccl/ccl.c`
- `src/CBD/Gen/ccl_cfg.h`
- `src/CBD/Gen/ccl_par.*`

### 먼저 볼 함수

- `CclInitPowerOn`
- `CclInit`
- `CclTask`
- `CclRequestCommunication`
- `CclReleaseCommunication`
- `CclComStart`
- `CclComStop`
- `CclComWait`
- `CclComResume`
- `CclBusOffStart`
- `CclBusOffEnd`

### 학습 목표

- 여러 사용자 요청을 기준으로 통신 허용/차단 상태를 결정하는 구조를 이해한다.
- 진단 요청과 일반 애플리케이션 요청이 CCL에서 통합되는 방식을 파악한다.
- UDS `0x28 CommunicationControl`과 CCL의 관계를 이해한다.

### 구현 연습

초기 사용자 핸들은 2개만 둔다.

```c
typedef enum
{
    CCL_USER_APP,
    CCL_USER_DIAG,
    CCL_USER_COUNT
} CclUser;
```

### 완료 기준

- 한 명 이상의 사용자가 통신을 요청하면 CAN/IL 송수신이 online 상태가 된다.
- 모든 사용자가 해제하면 통신을 offline 또는 sleep 준비 상태로 전환할 수 있다.
- 진단 CommunicationControl 요청으로 Tx/Rx enable/disable을 제어할 수 있다.

## 12. 10단계: HS-CAN에서 CAN-FD로 마이그레이션

### 참조 파일

기존 CBD 코드는 Classical CAN 중심이므로, 특정 CAN-FD 구현 파일은 아직 없다. 따라서 다음 기존 계층을 모두 CAN-FD 관점에서 다시 확인한다.

- `src/CBD/Gen/can_cfg.h`
- `src/CBD/Gen/can_par.h`
- `src/CBD/Gen/can_par.c`
- `src/CBD/Gen/drv_par.h`
- `src/CBD/Can/can_def.h`
- `src/CBD/Can/can_drv.c`
- `src/CBD/Tp/tpmc.h`
- `src/CBD/Tp/tpmc.c`
- `src/CBD/Gen/tp_cfg.h`
- `src/CBD/Il/il.c`
- `src/CBD/Gen/il_par.*`
- `src/CBD/Gen/desc.*`

### 학습 목표

- Classical CAN과 CAN-FD의 차이를 이해한다.
- 기존 8바이트 payload 고정 구조를 최대 64바이트 payload 구조로 확장한다.
- CAN-FD DLC encoding을 이해한다.
- BRS(Bit Rate Switching), ESI(Error State Indicator), FD frame flag를 데이터 모델에 추가한다.
- ISO-TP over CAN-FD에서 Single Frame 수용 길이와 First Frame 분할 전략이 달라지는 점을 이해한다.
- DBC 대신 CAN-FD용 DBC 또는 ARXML/새 통신 DB에서 생성되는 구조 차이를 예측한다.

### HS-CAN과 CAN-FD의 핵심 차이

| 항목 | HS-CAN / Classical CAN | CAN-FD |
|---|---|---|
| 최대 payload | 8바이트 | 64바이트 |
| DLC 의미 | 0~8이 실제 길이 | 0~8, 12, 16, 20, 24, 32, 48, 64 매핑 |
| 속도 | arbitration/data 동일 bitrate | arbitration bitrate와 data bitrate 분리 가능 |
| BRS | 없음 | data phase 고속 전환 가능 |
| ESI | 없음 | 송신 노드 error state 표시 |
| ISO-TP Single Frame | Normal addressing 기준 최대 7바이트 | CAN-FD에서는 더 큰 Single Frame 가능 |
| Driver buffer | 보통 8바이트 배열 | 64바이트 배열 필요 |

### 마이그레이션 설계 방향

1. 기존 CAN 모델을 깨지 않고 FD 확장 필드를 추가한다.
2. Classical CAN 메시지는 기존처럼 8바이트 이하로 유지한다.
3. CAN-FD 메시지는 `is_fd`, `brs`, `payload_length`를 명시한다.
4. DLC는 실제 payload length와 분리해 관리한다.
5. IL 신호 accessor는 8바이트 제한을 제거하되, 기존 신호 layout은 유지한다.
6. ISO-TP는 CAN-FD Single Frame과 Multi-frame 모두 지원하도록 분기한다.
7. UDS 계층은 가능하면 변경하지 않고 TP 계층의 payload 전달 크기만 확장한다.
8. NM/CCL은 frame format보다 통신 상태 제어가 핵심이므로 변경 범위를 최소화한다.

### 구현 연습

CAN frame 타입을 다음처럼 확장한다.

```c
typedef struct
{
    vuint32 id;
    vuint8 is_extended;
    vuint8 is_fd;
    vuint8 brs;
    vuint8 esi;
    vuint8 dlc;
    vuint8 length;
    vuint8 data[64];
} CanFrame;
```

CAN-FD DLC 변환 함수를 추가한다.

```c
vuint8 CanFd_LengthToDlc(vuint8 length);
vuint8 CanFd_DlcToLength(vuint8 dlc);
```

TP 설정에는 CAN-FD 사용 여부와 한 프레임 payload 크기를 둔다.

```c
#define MW_CANFD_ENABLED 1u
#define MW_CANFD_MAX_PAYLOAD 64u
#define MW_ISOTP_CANFD_PAYLOAD 64u
```

### 완료 기준

- 기존 Classical CAN 8바이트 메시지가 그대로 송수신된다.
- CAN-FD 12/16/20/24/32/48/64바이트 메시지가 DLC 변환을 통해 송수신된다.
- CAN Driver가 Classical frame과 FD frame을 구분한다.
- ISO-TP Single Frame 최대 payload 계산이 CAN-FD 여부에 따라 달라진다.
- UDS 계층은 payload 크기 증가에도 별도 서비스 로직 변경 없이 동작한다.

## 13. 추천 구현 디렉터리 구조

학습용 재구현 프로젝트는 다음 구조를 추천한다.

```text
middleware/
  common/
    platform_types.h
    vstdlib.h
    vstdlib.c
  config/
    v_cfg.h
    can_cfg.h
    il_cfg.h
    tp_cfg.h
    uds_cfg.h
    nm_cfg.h
    ccl_cfg.h
  can/
    can.h
    can.c
    can_types.h
    canfd.h
    canfd.c
    can_par.h
    can_par.c
  il/
    il.h
    il.c
    il_par.h
    il_par.c
  isotp/
    isotp.h
    isotp.c
    isotp_cfg.h
    isotp_canfd.c
  uds/
    uds.h
    uds.c
    uds_services.h
    uds_services.c
  nm/
    nm_basic.h
    nm_basic.c
  ccl/
    ccl.h
    ccl.c
  app/
    appdesc.h
    appdesc.c
tests/
  test_can.c
  test_il.c
  test_isotp.c
  test_uds.c
```

## 14. 추천 구현 마일스톤

### Milestone 1: 공통 타입과 CAN 설정

- 공통 타입 정의
- CAN Tx/Rx 핸들 정의
- CAN 메시지 설정 테이블 정의
- mock CAN 송신 로그 출력

### Milestone 2: CAN Driver MVP

- `Can_Init`
- `Can_Transmit`
- `Can_RxIndication`
- Tx confirmation callback
- Rx dispatch callback

### Milestone 3: IL MVP

- Tx 신호 put 함수
- Rx 신호 get 함수
- 10ms task 기반 주기 송신
- CAN Rx에서 IL buffer 갱신

### Milestone 4: ISO-TP MVP

- Single Frame 송수신
- Multi-frame 수신 재조립
- Multi-frame 송신 분할
- Flow Control 처리

### Milestone 5: UDS MVP

- UDS request buffer
- Service dispatcher
- Negative response
- `0x10`, `0x3E`, `0x22` 구현

### Milestone 6: Security / Routine / Communication Control

- `0x27 SecurityAccess`
- `0x28 CommunicationControl`
- `0x31 RoutineControl`
- session/security 조건 검사

### Milestone 7: NM / CCL 통합

- BusOff 상태 머신
- 통신 요청/해제 상태
- UDS CommunicationControl과 CCL 연동

### Milestone 8: 실제 CAN HAL 연결

- mock CAN 대신 MCU/HAL CAN 송수신 함수 연결
- ISR callback 연결
- BusOff interrupt 연결
- 타이머 tick 연결

### Milestone 9: CAN-FD 마이그레이션

- `CanFrame`을 Classical/FD 공용 구조로 확장
- CAN-FD DLC 변환 함수 구현
- 64바이트 Tx/Rx buffer 지원
- CAN HAL의 FD frame 송수신 API 연결
- ISO-TP over CAN-FD Single Frame / Multi-frame 처리
- 기존 Classical CAN 회귀 테스트 수행

## 15. 학습 시 주의할 점

1. `Gen/` 파일은 구현 로직이라기보다 설정 테이블과 생성된 glue code가 많다.
2. `can_drv.c`, `tpmc.c`, `desc.c`는 매우 크므로 처음부터 전체를 읽지 말고 API와 상태 흐름 중심으로 본다.
3. Vector 코드의 메모리 클래스 매크로는 실제 구현 시 처음에는 빈 매크로로 대체해도 된다.
4. 재구현 시에는 AUTOSAR 스타일의 복잡한 매크로보다 명확한 C 구조체와 함수 포인터 테이블로 시작하는 편이 좋다.
5. ISO-TP와 UDS는 반드시 테스트 벡터를 만들어 검증해야 한다.
6. 진단 서비스 구현은 dispatcher와 application callback을 분리해야 나중에 서비스 추가가 쉽다.
7. CAN-FD 전환 시 payload 길이만 늘리는 것으로 끝나지 않는다. DLC encoding, BRS, controller timing, acceptance filter, TP segmentation 정책을 함께 재검토해야 한다.
8. 기존 HS-CAN과 CAN-FD가 공존할 수 있으므로, frame format을 전역 설정 하나로만 결정하지 말고 메시지별 속성으로 관리하는 편이 안전하다.

## 16. 추천 읽기 순서

가장 먼저 읽을 파일 순서는 다음과 같다.

```text
1. src/CBD/Gen/v_cfg.h
2. src/CBD/Gen/can_cfg.h
3. src/CBD/Gen/can_par.h
4. src/CBD/Gen/drv_par.h
5. src/CBD/Common/v_def.h
6. src/CBD/Can/can_inc.h
7. src/CBD/Can/can_drv.c
8. src/CBD/Gen/il_cfg.h
9. src/CBD/Gen/il_par.c
10. src/CBD/Tp/tpmc.h
11. src/CBD/Gen/tp_cfg.h
12. src/CBD/Gen/desc.h
13. src/CBD/Gen/desc.c
14. src/CBD/Gen/appdesc.c
15. src/CBD/Nm/nm_basic.c
16. src/CBD/Ccl/ccl.c
17. CAN-FD 마이그레이션 설계: can_cfg/can_par/tp_cfg/IL buffer/TP buffer를 FD 관점에서 재검토
```

## 17. 최종 권장 전략

처음부터 Vector CANbedded와 1:1로 동일한 매크로 구조를 따라가지 말고, 다음 순서로 접근하는 것이 좋다.

1. 기능을 단순 C 구조체와 함수로 재현한다.
2. 동작 검증이 끝난 뒤 설정 테이블 기반 구조로 바꾼다.
3. 마지막에 컴파일러/MCU별 추상화 매크로를 추가한다.

즉, 먼저 이해하기 쉬운 C 코드로 동작 모델을 만들고, 그 다음 생성 코드 스타일의 정적 테이블/매크로 구조로 확장하는 방식이 가장 안전하다.

CAN-FD 마이그레이션은 마지막에 별도 프로젝트처럼 진행한다. 먼저 기존 Classical CAN 동작을 테스트로 고정하고, 그 위에 FD frame 속성, 64바이트 buffer, ISO-TP over CAN-FD를 확장해야 한다. 이렇게 해야 HS-CAN 기능을 깨뜨리지 않고 FD 전환을 검증할 수 있다.
