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
