# 6단계: ISO-TP 학습

## 학습 목표

ISO-TP는 8바이트 CAN 프레임으로 더 긴 진단 메시지를 주고받기 위한 전송 계층이다. 이 단계에서는 Single Frame, First Frame, Consecutive Frame, Flow Control 구조와 상태 머신을 학습한다.

## 참조 파일

- `src/CBD/Tp/tpmc.h`
- `src/CBD/Tp/tpmc.c`
- `src/CBD/Gen/tp_cfg.h`
- `src/CBD/Gen/tp_par.c`

## 이론 설명

CAN 한 프레임은 보통 최대 8바이트이다. 하지만 UDS 진단 요청/응답은 8바이트를 넘을 수 있다. ISO-TP는 긴 데이터를 여러 CAN 프레임으로 나눈다.

프레임 종류는 PCI 첫 nibble로 구분한다.

| PCI | 이름 | 의미 |
|---|---|---|
| `0x0` | Single Frame | 한 프레임에 전체 메시지 포함 |
| `0x1` | First Frame | 긴 메시지의 첫 프레임 |
| `0x2` | Consecutive Frame | First Frame 이후 연속 프레임 |
| `0x3` | Flow Control | 수신자가 송신자에게 계속 보내도 되는지 알림 |

Normal Addressing, 8바이트 CAN 기준으로 Single Frame은 최대 7바이트 payload를 담는다. First Frame은 전체 길이와 처음 6바이트 데이터를 담는다.

### ISO-TP가 필요한 이유

UDS는 진단 데이터를 byte stream으로 다룬다. 예를 들어 DID 읽기 응답은 수십 바이트가 될 수 있고, RoutineControl 응답도 8바이트를 넘을 수 있다. 하지만 Classical CAN frame은 최대 8바이트만 보낼 수 있다. ISO-TP는 이 차이를 해결하는 segmentation/reassembly 계층이다.

ISO-TP의 핵심은 “상위 계층에는 긴 payload 하나처럼 보이게 하고, 하위 CAN에는 여러 frame으로 나누어 보내는 것”이다. 따라서 UDS는 frame이 몇 개로 쪼개졌는지 몰라야 하고, CAN Driver는 UDS 서비스 의미를 몰라야 한다.

### 수신 상태 머신

수신 상태는 보통 다음처럼 단순화할 수 있다.

```text
IDLE
  | Single Frame 수신
  v
COMPLETE

IDLE
  | First Frame 수신
  v
WAIT_CF
  | Consecutive Frame 정상 수신
  v
WAIT_CF 또는 COMPLETE
```

First Frame을 받으면 전체 길이를 알 수 있다. 이후 Consecutive Frame의 sequence number를 1, 2, 3... 순서로 검사한다. sequence number가 틀리거나 timeout이 발생하면 수신을 중단하고 상위 계층에 error indication을 전달한다.

### 송신 상태 머신

송신은 반대로 payload 길이에 따라 Single Frame 또는 Multi-frame을 선택한다.

```text
IDLE
  | length <= SF capacity
  v
SEND_SF -> WAIT_CONFIRM -> IDLE

IDLE
  | length > SF capacity
  v
SEND_FF -> WAIT_FC -> SEND_CF -> WAIT_CONFIRM/DELAY -> IDLE
```

Flow Control은 수신자가 송신자에게 “계속 보내도 된다”, “기다려라”, “overflow라 중단하라”를 알려주는 frame이다. Block Size가 0이면 끝까지 연속 전송이 가능하고, 0이 아니면 지정된 수의 CF를 보낸 뒤 다시 FC를 기다린다.

### 타이밍과 오류 처리

ISO-TP 구현에서 가장 중요한 것은 정상 경로보다 오류 경로이다.

- First Frame 이후 buffer가 부족하면 overflow 처리
- Consecutive Frame sequence number가 틀리면 수신 중단
- Flow Control이 오지 않으면 Tx timeout
- Consecutive Frame이 오지 않으면 Rx timeout
- STmin보다 빨리 CF를 보내면 수신 측이 거부할 수 있음

CBD의 `TpRxTimeoutCF`, `TpTxTimeoutFC`, `TpSTMin` 같은 설정은 이런 상태 머신의 시간 기준이다.

### SPC58xC 적용 관점

`SPC58xC`에서는 ISO-TP 자체가 하드웨어에 종속되지는 않지만, timer tick과 CAN confirmation timing에 영향을 받는다. 일반적으로 10ms task tick만으로는 STmin이 작은 경우 정밀도가 부족할 수 있다. CAN-FD data phase를 사용하면 frame 간 간격이 더 짧아질 수 있으므로, STmin 처리에 사용할 timer 해상도를 설계해야 한다.

또한 ISR에서 TP 상태 머신을 전부 돌릴지, task에서 돌릴지도 결정해야 한다. 안전한 기본 구조는 ISR에서는 frame을 queue에 넣고, `TpTask()`에서 상태 머신을 진행하는 것이다.

### CAN-FD와 ISO-TP

CAN-FD에서는 한 frame payload가 커져 Single Frame으로 처리 가능한 UDS 메시지가 늘어난다. Multi-frame에서도 Consecutive Frame 하나에 더 많은 데이터를 넣을 수 있어 frame 수가 줄어든다. 하지만 ISO-TP over CAN-FD는 PCI encoding과 addressing format에 따라 세부 규칙이 달라질 수 있으므로, 구현 전에 사용할 표준 버전과 OEM 요구사항을 확정해야 한다.

## CBD 코드에서 관찰할 포인트

- `TpInit`
- `TpTask`
- `TpPrecopy`
- `TpTransmit`
- `TpRxTask`
- `TpTxTask`
- `TpRxResetChannel`
- `TpTxResetChannel`
- `DescGetBuffer`, `DescPhysReqInd`와 연결되는 부분

## 예제 코드

### Single Frame 수신

```c
#include "platform_types.h"

typedef void (*IsoTpRxCompleteCallback)(const vuint8 *data, vuint16 length);

static IsoTpRxCompleteCallback g_rx_complete;
static vuint8 g_rx_buffer[256];

void IsoTp_SetRxCompleteCallback(IsoTpRxCompleteCallback callback)
{
    g_rx_complete = callback;
}

void IsoTp_RxCanFrame(const vuint8 frame[8])
{
    vuint8 pci_type = frame[0] >> 4;
    vuint8 length = frame[0] & 0x0Fu;

    if (pci_type == 0x0u)
    {
        for (vuint8 i = 0u; i < length; i++)
        {
            g_rx_buffer[i] = frame[i + 1u];
        }

        if (g_rx_complete != 0)
        {
            g_rx_complete(g_rx_buffer, length);
        }
    }
}
```

### Single Frame 송신

```c
typedef void (*IsoTpCanTransmit)(const vuint8 frame[8]);

static IsoTpCanTransmit g_can_tx;

void IsoTp_SetCanTransmit(IsoTpCanTransmit transmit)
{
    g_can_tx = transmit;
}

vuint8 IsoTp_Transmit(const vuint8 *data, vuint16 length)
{
    vuint8 frame[8] = {0};

    if (length > 7u)
    {
        return 1u;
    }

    frame[0] = (vuint8)(0x00u | length);

    for (vuint8 i = 0u; i < length; i++)
    {
        frame[i + 1u] = data[i];
    }

    if (g_can_tx != 0)
    {
        g_can_tx(frame);
    }

    return 0u;
}
```

### Multi-frame 수신 핵심 구조

```c
typedef enum
{
    ISOTP_RX_IDLE,
    ISOTP_RX_WAIT_CF
} IsoTpRxState;

static IsoTpRxState g_rx_state;
static vuint16 g_rx_expected_length;
static vuint16 g_rx_current_length;
static vuint8 g_rx_next_sn;

static void IsoTp_HandleFirstFrame(const vuint8 frame[8])
{
    g_rx_expected_length = ((vuint16)(frame[0] & 0x0Fu) << 8) | frame[1];
    g_rx_current_length = 6u;
    g_rx_next_sn = 1u;

    for (vuint8 i = 0u; i < 6u; i++)
    {
        g_rx_buffer[i] = frame[i + 2u];
    }

    g_rx_state = ISOTP_RX_WAIT_CF;

    /* 실제 구현에서는 여기서 Flow Control frame을 송신한다. */
}
```

## 연습문제

1. First Frame을 받으면 Flow Control frame `{0x30, 0x00, 0x00, ...}`을 송신하도록 구현하라.
2. Consecutive Frame의 sequence number가 예상값과 다르면 수신을 중단하라.
3. 20바이트 데이터를 ISO-TP multi-frame으로 송신하는 함수를 작성하라.
4. STmin을 적용하여 Consecutive Frame 사이에 최소 지연 tick을 두라.
5. 수신 timeout counter를 추가하고 일정 tick 동안 CF가 오지 않으면 reset하라.

## 완료 기준

- 7바이트 이하 메시지를 Single Frame으로 송수신할 수 있다.
- 8바이트 초과 메시지를 First Frame + Consecutive Frame으로 재조립할 수 있다.
- Flow Control을 송수신 흐름에 포함할 수 있다.
- 완성된 payload를 UDS 계층 callback으로 넘길 수 있다.
