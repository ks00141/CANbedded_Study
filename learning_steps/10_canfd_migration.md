# 10단계: HS-CAN에서 CAN-FD로 마이그레이션

## 학습 목표

현재 CBD 미들웨어 구조는 Classical CAN, 즉 HS-CAN 8바이트 payload 중심으로 작성되어 있다. 이 단계에서는 기존 구조를 최대한 유지하면서 CAN-FD 통신 스펙으로 확장하는 방법을 학습한다.

목표는 단순히 payload 배열을 8바이트에서 64바이트로 늘리는 것이 아니다. CAN-FD는 DLC encoding, BRS, data phase bitrate, ISO-TP 분할 정책, driver mailbox 설정, 통신 DB 생성 방식까지 함께 바뀐다.

## 참조 파일

기존 코드에는 CAN-FD 전용 구현이 없으므로, 아래 파일을 CAN-FD 관점에서 재검토한다.

- `src/CBD/Gen/can_cfg.h`
- `src/CBD/Gen/can_par.h`
- `src/CBD/Gen/can_par.c`
- `src/CBD/Gen/drv_par.h`
- `src/CBD/Can/can_def.h`
- `src/CBD/Can/can_drv.c`
- `src/CBD/Tp/tpmc.h`
- `src/CBD/Tp/tpmc.c`
- `src/CBD/Gen/tp_cfg.h`
- `src/CBD/Gen/il_par.*`
- `src/CBD/Gen/desc.*`

## 이론 설명

### Classical CAN과 CAN-FD 차이

| 항목 | Classical CAN / HS-CAN | CAN-FD |
|---|---|---|
| 최대 payload | 8바이트 | 64바이트 |
| DLC | 0~8이 길이와 동일 | 0~8, 12, 16, 20, 24, 32, 48, 64로 매핑 |
| Bitrate | arbitration/data 동일 | arbitration/data phase 분리 가능 |
| BRS | 없음 | Bit Rate Switching 지원 |
| ESI | 없음 | Error State Indicator 지원 |
| CRC | Classical CAN CRC | payload 길이에 따라 FD CRC 확장 |
| ISO-TP | Single Frame 최대 7바이트 | CAN-FD에서는 더 큰 Single Frame 가능 |

### CAN-FD 마이그레이션에서 깨지기 쉬운 지점

1. `uint8_t data[8]` 고정 배열
2. DLC를 실제 길이로 그대로 해석하는 코드
3. `if (dlc > 8)` 같은 Classical CAN 검증
4. ISO-TP Single Frame 길이 계산
5. CAN ID와 frame format만 있고 FD 여부가 없는 설정 테이블
6. 하드웨어 mailbox가 8바이트 payload로 설정된 초기화 코드
7. DBC 신호 구조체가 8바이트 message를 전제로 생성된 경우

## CBD 계층별 마이그레이션 관점

### Common / VStdLib

공통 타입은 큰 변경이 없다. 다만 payload length가 64까지 증가하므로 길이 타입은 최소 `vuint8`로 충분하지만, TP 전체 메시지 길이는 `vuint16` 이상을 유지해야 한다.

### Generated Config

CAN-FD 활성 여부와 payload 크기를 설정으로 분리한다.

```c
#define MW_CANFD_ENABLED 1u
#define MW_CAN_CLASSIC_MAX_PAYLOAD 8u
#define MW_CANFD_MAX_PAYLOAD 64u
#define MW_CANFD_USE_BRS 1u
```

메시지별로 Classical/FD가 공존할 수 있으므로 전역 설정 하나만 두지 말고 메시지 설정에도 `is_fd`를 둔다.

### CAN Data Model

기존 Tx/Rx 설정 구조체에 FD 속성을 추가한다.

```c
typedef struct
{
    vuint32 id;
    vuint8 is_extended;
    vuint8 is_fd;
    vuint8 brs;
    vuint8 dlc;
    vuint8 length;
    vuint8 *data;
} CanObjectConfig;
```

### CAN Driver

Driver는 Classical frame과 FD frame을 구분해 HAL에 전달해야 한다.

- Classical frame: 최대 8바이트
- FD frame: 최대 64바이트
- DLC와 length 변환 필요
- FD mailbox payload size 설정 필요
- BRS enable 여부 설정 필요

### IL

IL은 신호 위치와 길이에 따라 buffer를 읽고 쓴다. CAN-FD에서는 한 메시지에 더 많은 신호가 들어갈 수 있으므로 다음을 바꿔야 한다.

- 메시지 buffer 크기 8 고정 제거
- signal accessor가 message length를 넘지 않는지 검사
- 주기 송신 정책은 유지하되 FD message의 payload length를 설정 테이블에서 읽기

### ISO-TP

CAN-FD에서 가장 큰 변화가 있는 계층이다.

Classical CAN Normal Addressing:

- Single Frame payload: 최대 7바이트
- First Frame payload: 6바이트
- Consecutive Frame payload: 7바이트

CAN-FD 64바이트 Normal Addressing:

- Single Frame payload: 더 큰 길이를 담을 수 있음
- Consecutive Frame payload: 최대 63바이트 수준까지 확장 가능
- 큰 UDS 응답도 Multi-frame 개수가 크게 줄어듦

정확한 Single Frame 길이 encoding은 사용하는 ISO-TP 버전과 addressing format에 맞춰 결정해야 한다.

### UDS / CANdesc

UDS 서비스 자체는 CAN-FD 여부를 몰라도 되도록 유지하는 것이 좋다. UDS는 request/response payload만 다루고, frame 분할은 TP가 담당해야 한다.

변경 필요 지점:

- response buffer 크기
- 최대 DID 응답 길이
- responseTooLong 판단 기준
- P2/P2* 타이밍 재검토

### NM / CCL

NM과 CCL은 frame payload보다 통신 상태 제어가 핵심이다. 변경은 작지만 다음을 확인해야 한다.

- CAN-FD controller도 BusOff/error state를 같은 방식으로 보고하는지
- CCL의 communication enable/disable이 FD frame에도 적용되는지
- 진단 CommunicationControl이 Classical/FD Tx 모두 제어하는지

## 예제 코드

### CAN-FD frame 타입

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

### CAN-FD DLC 변환

```c
vuint8 CanFd_DlcToLength(vuint8 dlc)
{
    static const vuint8 table[16] =
    {
        0u, 1u, 2u, 3u, 4u, 5u, 6u, 7u,
        8u, 12u, 16u, 20u, 24u, 32u, 48u, 64u
    };

    if (dlc > 15u)
    {
        return 0u;
    }

    return table[dlc];
}

vuint8 CanFd_LengthToDlc(vuint8 length)
{
    if (length <= 8u)  { return length; }
    if (length <= 12u) { return 9u; }
    if (length <= 16u) { return 10u; }
    if (length <= 20u) { return 11u; }
    if (length <= 24u) { return 12u; }
    if (length <= 32u) { return 13u; }
    if (length <= 48u) { return 14u; }
    if (length <= 64u) { return 15u; }
    return 0xFFu;
}
```

### Classical / FD 공용 송신 검증

```c
vuint8 Can_ValidateFrame(const CanFrame *frame)
{
    if (frame == 0)
    {
        return 1u;
    }

    if (frame->is_fd == 0u)
    {
        if (frame->length > 8u)
        {
            return 2u;
        }
    }
    else
    {
        if (frame->length > 64u)
        {
            return 3u;
        }

        if (CanFd_LengthToDlc(frame->length) == 0xFFu)
        {
            return 4u;
        }
    }

    return 0u;
}
```

### ISO-TP payload 크기 분기

```c
vuint8 IsoTp_GetConsecutiveFramePayload(vuint8 is_canfd, vuint8 addressing_bytes)
{
    vuint8 frame_payload;

    if (is_canfd != 0u)
    {
        frame_payload = 64u;
    }
    else
    {
        frame_payload = 8u;
    }

    /*
     * 1 byte PCI + addressing byte 수를 제외한다.
     */
    return (vuint8)(frame_payload - 1u - addressing_bytes);
}
```

## 마이그레이션 절차

1. 기존 HS-CAN 동작을 테스트로 고정한다.
2. CAN frame 구조체에 `is_fd`, `brs`, `length`, `dlc`, `data[64]`를 추가한다.
3. Classical CAN 메시지는 기존 8바이트 동작을 유지한다.
4. CAN-FD DLC 변환 함수를 추가한다.
5. CAN HAL 송신/수신 인터페이스를 FD frame 가능 형태로 바꾼다.
6. CAN 설정 테이블에 메시지별 FD 여부와 payload length를 추가한다.
7. IL buffer 크기와 signal accessor 범위 검사를 확장한다.
8. ISO-TP에서 CAN-FD Single Frame / Consecutive Frame payload 크기를 반영한다.
9. UDS response buffer와 responseTooLong 기준을 재검토한다.
10. NM/CCL에서 FD frame도 동일하게 online/offline 제어되는지 검증한다.

## 연습문제

1. `CanFd_DlcToLength()`와 `CanFd_LengthToDlc()`의 테스트 케이스를 작성하라.
2. Classical CAN frame에 `length=12`가 들어오면 송신을 거부하는 코드를 작성하라.
3. FD frame `length=12`가 들어오면 DLC `9`로 변환되는지 확인하라.
4. 기존 `uint8_t data[8]` 기반 Tx/Rx 설정 구조체를 `data[64]` 또는 pointer+length 구조로 변경하라.
5. ISO-TP Single Frame 송신 함수가 CAN-FD 여부에 따라 Single Frame 가능 길이를 다르게 판단하도록 구현하라.
6. 기존 UDS `0x22` 응답이 30바이트일 때 Classical CAN과 CAN-FD에서 각각 몇 개 프레임으로 나뉘는지 계산하라.
7. CCL에서 Tx disable 상태일 때 Classical CAN과 CAN-FD 송신이 모두 막히는지 테스트하라.

## 완료 기준

- 기존 HS-CAN 메시지가 회귀 테스트에서 그대로 통과한다.
- CAN-FD frame이 12/16/20/24/32/48/64바이트 payload로 송신 검증을 통과한다.
- DLC와 실제 payload length를 분리해 관리한다.
- CAN Driver가 Classical frame과 FD frame을 구분해 처리한다.
- ISO-TP 계층이 CAN-FD payload 크기를 반영한다.
- UDS 계층은 frame format에 직접 의존하지 않고 TP payload만 처리한다.

## 최종 체크리스트

- [ ] `data[8]` 고정 배열을 모두 식별했다.
- [ ] DLC를 length로 오해하는 코드를 모두 식별했다.
- [ ] CAN-FD 메시지별 `is_fd`, `brs`, `length` 설정이 존재한다.
- [ ] CAN HAL이 FD mailbox/payload size/data bitrate를 설정한다.
- [ ] ISO-TP over CAN-FD 정책이 명확하다.
- [ ] Classical CAN 회귀 테스트와 CAN-FD 신규 테스트가 모두 존재한다.
