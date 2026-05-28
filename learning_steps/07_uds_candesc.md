# 7단계: UDS / CANdesc 학습

## 학습 목표

UDS 계층은 ISO-TP로 올라온 진단 요청을 해석하고, 서비스별 handler를 호출해 응답을 만든다. 이 단계에서는 UDS dispatcher, session/security 상태, positive/negative response, application callback 분리를 학습한다.

## 참조 파일

- `src/CBD/Gen/desc.h`
- `src/CBD/Gen/desc.c`
- `src/CBD/Gen/appdesc.h`
- `src/CBD/Gen/appdesc.c`

## 이론 설명

UDS 요청의 첫 바이트는 Service ID이다.

예:

```text
10 01       DiagnosticSessionControl, default session
3E 00       TesterPresent
22 F1 90    ReadDataByIdentifier
27 01       SecurityAccess request seed
```

UDS 계층은 SID를 보고 handler를 선택한다. 지원하지 않는 서비스는 negative response를 보낸다.

Negative Response 형식:

```text
7F <요청 SID> <NRC>
```

Positive Response는 일반적으로 SID에 `0x40`을 더한다.

```text
요청: 10 01
응답: 50 01 ...
```

### UDS 계층의 책임

UDS 계층은 CAN frame을 직접 다루지 않는다. TP에서 완성된 request payload를 받아 서비스 ID를 해석하고 response payload를 만든다. 이 분리를 지키면 Classical CAN에서 CAN-FD로 바뀌어도 UDS 서비스 로직은 거의 유지된다.

UDS 계층이 담당하는 핵심 기능은 다음과 같다.

- request length 검사
- service ID / sub-function dispatch
- session 조건 검사
- security access 조건 검사
- positive response 생성
- negative response 생성
- response pending 관리
- application callback 호출

### 진단 컨텍스트

CANdesc 스타일 구현에서는 진단 요청 하나를 처리하기 위한 context가 있다. context에는 보통 다음 정보가 들어간다.

- request buffer pointer
- request length
- response buffer pointer
- response length
- 현재 service ID
- addressing type, physical/functional 여부
- tester address
- 처리 상태, busy 여부
- response pending 필요 여부

이 context를 두면 서비스 handler가 전역 변수를 마구 만지지 않고도 요청/응답을 구성할 수 있다.

### session과 security

UDS 서비스는 아무 때나 실행되지 않는다. 예를 들어 어떤 DID는 default session에서도 읽을 수 있지만, 어떤 routine은 extended session이나 programming session에서만 허용될 수 있다. SecurityAccess도 마찬가지로, seed/key 인증이 성공해야 특정 서비스가 열린다.

따라서 dispatcher는 handler 호출 전에 다음을 검사해야 한다.

1. 서비스가 존재하는가
2. 요청 길이가 맞는가
3. 현재 session에서 허용되는가
4. 현재 security level에서 허용되는가
5. functional request로 허용되는 서비스인가

검사 실패 시에는 handler를 호출하지 않고 적절한 NRC를 반환한다.

### application callback 분리

`appdesc.c`의 역할은 UDS core와 실제 ECU 기능을 분리하는 것이다. 예를 들어 `0x22 F190` VIN 읽기 서비스가 있다면 UDS core는 `0x22` 요청 형식과 응답 형식을 알고, 실제 VIN 데이터는 application callback이 제공한다. 이 구조를 유지해야 진단 stack을 다른 ECU에도 재사용할 수 있다.

### SPC58xC 적용 관점

`SPC58xC`에서는 UDS 서비스가 flash write, reset, bootloader jump, NVM access 같은 MCU 기능과 연결될 수 있다. 이때 UDS core가 직접 flash controller를 만지면 안 된다. application callback 또는 service adapter 계층을 두어 다음처럼 분리한다.

```text
UDS Core
  -> AppDesc callback
  -> ECU service adapter
  -> SPC58xC flash/NVM/reset driver
```

또한 reset 서비스나 programming session은 interrupt, watchdog, flash state와 연결되므로 단순 예제 코드처럼 즉시 실행하지 말고, response 전송 완료 후 deferred action으로 처리하는 것이 안전하다.

### CAN-FD와 UDS

CAN-FD로 전환되면 UDS response가 더 적은 frame으로 나가지만, UDS 서비스 자체의 의미는 변하지 않는다. 다만 response buffer 크기, `responseTooLong` 판단 기준, DID 묶음 응답 길이는 재검토해야 한다. UDS core는 “내가 CAN-FD인지”보다 “TP가 허용하는 최대 payload가 얼마인지”만 알도록 설계하는 것이 좋다.

## CBD 코드에서 관찰할 포인트

- `DescGetBuffer`
- `DescPhysReqInd`
- `DescFuncReqInd`
- `DescDispatcher`
- `DescProcessingDone`
- `DescSetNegResponse`
- `DescGetStateSession`
- `DescSetStateSession`
- `ApplDesc...` 서비스 callback

## 예제 코드

### uds.h

```c
#ifndef UDS_H
#define UDS_H

#include "platform_types.h"

#define UDS_NRC_SERVICE_NOT_SUPPORTED 0x11u
#define UDS_NRC_SUBFUNCTION_NOT_SUPPORTED 0x12u
#define UDS_NRC_INCORRECT_LENGTH 0x13u

typedef vuint8 (*UdsServiceHandler)(const vuint8 *req, vuint16 req_len,
                                    vuint8 *res, vuint16 *res_len);

void Uds_Init(void);
void Uds_OnRequest(const vuint8 *data, vuint16 length);

#endif
```

### uds.c

```c
#include "uds.h"

static vuint8 g_uds_response[256];

static void Uds_SendResponse(const vuint8 *data, vuint16 length)
{
    /*
     * 실제 구현에서는 IsoTp_Transmit(data, length)를 호출한다.
     */
    (void)data;
    (void)length;
}

static void Uds_SendNegativeResponse(vuint8 sid, vuint8 nrc)
{
    g_uds_response[0] = 0x7Fu;
    g_uds_response[1] = sid;
    g_uds_response[2] = nrc;
    Uds_SendResponse(g_uds_response, 3u);
}

static vuint8 Uds_HandleSessionControl(const vuint8 *req, vuint16 req_len,
                                       vuint8 *res, vuint16 *res_len)
{
    if (req_len != 2u)
    {
        return UDS_NRC_INCORRECT_LENGTH;
    }

    res[0] = 0x50u;
    res[1] = req[1];
    res[2] = 0x00u;
    res[3] = 0x32u;
    res[4] = 0x01u;
    res[5] = 0xF4u;
    *res_len = 6u;
    return 0u;
}

static vuint8 Uds_HandleTesterPresent(const vuint8 *req, vuint16 req_len,
                                      vuint8 *res, vuint16 *res_len)
{
    if (req_len != 2u)
    {
        return UDS_NRC_INCORRECT_LENGTH;
    }

    res[0] = 0x7Eu;
    res[1] = req[1];
    *res_len = 2u;
    return 0u;
}

void Uds_OnRequest(const vuint8 *data, vuint16 length)
{
    vuint8 sid;
    vuint8 nrc;
    vuint16 res_len = 0u;

    if (length == 0u)
    {
        return;
    }

    sid = data[0];

    switch (sid)
    {
    case 0x10u:
        nrc = Uds_HandleSessionControl(data, length, g_uds_response, &res_len);
        break;
    case 0x3Eu:
        nrc = Uds_HandleTesterPresent(data, length, g_uds_response, &res_len);
        break;
    default:
        Uds_SendNegativeResponse(sid, UDS_NRC_SERVICE_NOT_SUPPORTED);
        return;
    }

    if (nrc != 0u)
    {
        Uds_SendNegativeResponse(sid, nrc);
    }
    else
    {
        Uds_SendResponse(g_uds_response, res_len);
    }
}
```

## 연습문제

1. `0x22 ReadDataByIdentifier` handler를 추가하고 DID `0xF190`에 대해 임의 VIN 데이터를 반환하라.
2. `0x27 SecurityAccess`의 seed 요청과 key 요청을 분리해 처리하라.
3. 현재 session이 extended session이 아닐 때 특정 DID 요청을 NRC `0x31`로 거부하라.
4. suppress positive response bit가 설정된 `3E 80` 요청에 대해 positive response를 보내지 않도록 구현하라.
5. service table 구조체를 만들고 switch문 대신 테이블 lookup 방식으로 dispatcher를 구현하라.

## 완료 기준

- ISO-TP callback에서 UDS request를 받을 수 있다.
- 지원 서비스는 positive response를 생성한다.
- 미지원 서비스는 negative response를 생성한다.
- 서비스 handler와 애플리케이션 데이터 제공 함수를 분리할 수 있다.
