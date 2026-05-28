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
