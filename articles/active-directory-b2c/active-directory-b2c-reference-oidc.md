---
title: OpenID Connect로 웹 로그인 - Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C에서 Openid connect Connect 인증 프로토콜을 사용 하 여 웹 응용 프로그램을 빌드합니다.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: marsma
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: c5626e2ddfc24eeaeed562f3eaf73d16626eb458
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68278030"
---
# <a name="web-sign-in-with-openid-connect-in-azure-active-directory-b2c"></a>Azure Active Directory B2C에서 OpenID Connect로 웹 로그인

OpenID Connect는 웹 애플리케이션에 사용자를 안전하게 로그인하는 데 사용할 수 있도록 OAuth 2.0을 기반으로 하여 빌드된 인증 프로토콜입니다. OpenID Connect의 Azure AD B2C(Azure Active Directory B2C) 구현을 사용하여 웹 애플리케이션의 등록, 로그인 및 기타 ID 관리 환경을 Azure AD( Azure Active Directory)로 아웃소싱할 수 있습니다. 이 가이드에서는 언어에 관계 없이 이 작업을 수행하는 방법을 보여 줍니다. 오픈 소스 라이브러리를 사용하지 않고 HTTP 메시지를 보내고 받는 방법을 설명합니다.

[OpenID Connect](https://openid.net/specs/openid-connect-core-1_0.html)는 *인증* 프로토콜로 사용하기 위해 OAuth 2.0 *권한 부여* 프로토콜을 확장합니다. 이 인증 프로토콜을 사용 하 여 single sign-on을 수행할 수 있습니다. 클라이언트에서 사용자의 id를 확인 하 고 사용자에 대 한 기본 프로필 정보를 얻을 수 있도록 하는 *ID 토큰*의 개념을 소개 합니다.

OAuth 2.0을 확장 하므로 응용 프로그램에서 *액세스 토큰*을 안전 하 게 가져올 수 있습니다. 액세스 토큰을 사용하여 [권한 부여 서버](active-directory-b2c-reference-protocols.md)로 보안이 유지되는 리소스에 액세스할 수 있습니다. 서버에서 호스트 되 고 브라우저를 통해 액세스 되는 웹 응용 프로그램을 빌드하는 경우 Openid connect 연결을 권장 합니다. Azure AD B2C를 사용 하 여 모바일 또는 데스크톱 응용 프로그램에 id 관리를 추가 하려면 Openid connect Connect 대신 [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) 을 사용 해야 합니다. 토큰에 대 한 자세한 내용은 Azure Active Directory B2C의 [토큰 개요](active-directory-b2c-reference-tokens.md) 를 참조 하세요.

Azure AD B2C는 단순한 인증 및 권한 부여 보다 더 많은 작업으로 표준 OpenID Connect 프로토콜을 확장합니다. Openid connect Connect를 사용 하 여 등록, 로그인 및 프로필 관리와 같은 사용자 환경을 응용 프로그램에 추가할 수 있는 [사용자 흐름 매개 변수](active-directory-b2c-reference-policies.md)를 소개 합니다.

## <a name="send-authentication-requests"></a>인증 요청 보내기

웹 응용 프로그램이 사용자를 인증 하 고 사용자 흐름을 실행 해야 하는 경우 사용자를 `/authorize` 끝점으로 보낼 수 있습니다. 사용자가 사용자 흐름에 따라 동작을 수행 합니다.

이 요청에서 클라이언트는 매개 변수에서 `scope` 사용자 로부터 획득 해야 하는 사용 권한과 `p` 매개 변수에서 실행할 사용자 흐름이 표시 됩니다. 각각 서로 다른 사용자 흐름을 사용하는 세 가지 예제가 다음 섹션에서 제공됩니다(쉽게 읽을 수 있도록 줄 바꿈 적용). 각 요청의 작동 방식에 대해 이해하려면 요청을 브라우저에 붙여 넣고 실행합니다. 가 있고 사용자 `fabrikamb2c` 흐름을 만든 테 넌 트의 이름으로 바꿀 수 있습니다.

#### <a name="use-a-sign-in-user-flow"></a>로그인 사용자 흐름 사용
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-user-flow"></a>가입 사용자 흐름 사용
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-user-flow"></a>프로필 편집 사용자 흐름 사용
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| 매개 변수 | 필수 | Description |
| --------- | -------- | ----------- |
| client_id | 예 | [Azure Portal](https://portal.azure.com/) 응용 프로그램에 할당 된 응용 프로그램 ID입니다. |
| response_type | 예 | Openid connect Connect에 대 한 ID 토큰을 포함 해야 합니다. 웹 응용 프로그램에 웹 API를 호출 하는 데 필요한 토큰만 있으면를 사용할 `code+id_token`수 있습니다. |
| redirect_uri | 아니요 | 응용 프로그램에서 인증 응답을 보내고 받을 수 있는 응용 프로그램의 매개변수입니다.`redirect_uri` URL로 인코딩해야 한다는 점을 제외 하 `redirect_uri` 고 Azure Portal에 등록 한 매개 변수 중 하 나와 정확 하 게 일치 해야 합니다. |
| 범위 | 예 | 공백으로 구분된 범위 목록입니다. `openid` 범위는 사용자에게 로그인하고 ID 토큰 형식으로 사용자에 대한 데이터를 가져올 권한을 나타냅니다. 범위 `offline_access` 는 웹 응용 프로그램에 대 한 선택 사항입니다. 응용 프로그램에 리소스에 대 한 확장 액세스를 위한 *새로 고침 토큰이* 필요 함을 나타냅니다. |
| response_mode | 아니요 | 결과 권한 부여 코드를 응용 프로그램에 다시 보내는 데 사용 되는 메서드입니다. `query`, `form_post` 또는 `fragment` 중 하나일 수 있습니다.  최상의 보안을 위해 `form_post` 응답 모드를 사용하는 것이 좋습니다. |
| state | 아니요 | 토큰 응답에도 반환 되는 요청에 포함 된 값입니다. 원하는 모든 콘텐츠의 문자열일 수 있습니다. 일반적으로 교차 사이트 요청 위조 공격을 방지하기 위해 임의로 생성된 고유 값이 사용됩니다. 상태는 인증 요청이 발생 하기 전에 응용 프로그램에서 사용자의 상태에 대 한 정보를 인코딩하는 데에도 사용 됩니다 (예: 설정 된 페이지). |
| nonce | 예 | 응용 프로그램에 의해 생성 된 요청에 포함 된 값으로, 결과 ID 토큰에 클레임으로 포함 됩니다. 그러면 응용 프로그램에서이 값을 확인 하 여 토큰 재생 공격을 완화할 수 있습니다. 값은 일반적으로 요청의 출처를 식별하는 데 사용할 수 있는 임의의 고유 문자열입니다. |
| p | 예 | 실행 되는 사용자 흐름입니다. Azure AD B2C 테 넌 트에서 생성 된 사용자 흐름의 이름입니다. 사용자 흐름의 이름은으로 `b2c\_1\_`시작 해야 합니다. |
| prompt | 아니요 | 필요한 사용자 상호 작용의 형식입니다. 현재 유효한 값은 `login`뿐이며, 이는 사용자가 해당 요청에 대한 자격 증명을 입력하도록 합니다. |

이 시점에서 사용자에 게 워크플로를 완료 하 라는 메시지가 표시 됩니다. 사용자는 사용자 이름 및 암호를 입력 하거나, 소셜 id로 로그인 하거나, 디렉터리에 등록 해야 할 수 있습니다. 사용자 흐름이 정의 된 방법에 따라 다른 수의 단계가 있을 수 있습니다.

사용자가 사용자 흐름을 완료 한 후에는 `redirect_uri` `response_mode` 매개 변수에 지정 된 메서드를 사용 하 여 지정 된 매개 변수에서 응용 프로그램에 대 한 응답이 반환 됩니다. 응답은 사용자 흐름과 무관 하 게 위의 각 사례에 대해 동일 합니다.

`response_mode=fragment` 를 사용한 성공적인 응답은 다음과 같습니다.

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| 매개 변수 | Description |
| --------- | ----------- |
| id_token | 응용 프로그램이 요청한 ID 토큰입니다. ID 토큰을 사용하여 사용자 ID를 확인하고 사용자와 세션을 시작할 수 있습니다. |
| code | 를 사용한 `response_type=code+id_token`경우 응용 프로그램이 요청한 인증 코드입니다. 응용 프로그램은 권한 부여 코드를 사용 하 여 대상 리소스에 대 한 액세스 토큰을 요청할 수 있습니다. 인증 코드는 일반적으로 약 10 분 후에 만료 됩니다. |
| state | 요청에 `state` 매개 변수가 포함되어 있으면 동일한 값이 응답에도 나타나야 합니다. 응용 프로그램은 요청 및 응답 `state` 의 값이 동일한 지 확인 해야 합니다. |

응용 프로그램에서 적절 하 게 처리할 수 `redirect_uri` 있도록 오류 응답을 매개 변수로 보낼 수도 있습니다.

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| 매개 변수 | Description |
| --------- | ----------- |
| error | 발생 한 오류 유형을 분류 하는 데 사용할 수 있는 코드입니다. |
| error_description | 인증 오류의 근본 원인을 식별 하는 데 도움이 될 수 있는 특정 오류 메시지입니다. |
| state | 요청에 `state` 매개 변수가 포함되어 있으면 동일한 값이 응답에도 나타나야 합니다. 응용 프로그램은 요청 및 응답 `state` 의 값이 동일한 지 확인 해야 합니다. |

## <a name="validate-the-id-token"></a>ID 토큰 유효성 검사

ID 토큰을 받는 것만으로는 사용자를 인증할 수 없습니다. ID 토큰의 서명 유효성을 검사 하 고 응용 프로그램의 요구 사항에 따라 토큰의 클레임을 확인 합니다. Azure AD B2C는 [JWT(JSON 웹 토큰)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) 및 공개 키 암호화를 사용하여 토큰에 서명하고 토큰이 유효한지 확인합니다. 기본 설정의 언어에 따라 JWT의 유효성 검사에 사용할 수 있는 다양한 공개 소스 라이브러리가 있습니다. 고유한 유효성 검사 논리를 구현하는 것보다 이러한 옵션을 탐색하는 것이 좋습니다. 

Azure AD B2C에는 응용 프로그램이 런타임에 Azure AD B2C에 대 한 정보를 가져올 수 있도록 하는 Openid connect Connect 메타 데이터 끝점이 있습니다. 이 정보에는 엔드포인트, 토큰 콘텐츠 및 토큰 서명 키가 포함됩니다. B2C 테넌트에서 각 사용자 흐름에 대한 JSON 메타데이터 문서가 있습니다. 예를 들어 `fabrikamb2c.onmicrosoft.com`의 `b2c_1_sign_in` 사용자 흐름에 대한 메타데이터 문서는 다음 위치에 있습니다.

`https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

이 구성 문서의 속성 중 하나가 `jwks_uri`이며, 동일한 사용자 흐름에 대한 값은 다음과 같습니다.

`https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

ID 토큰을 서명 하는 데 사용 된 사용자 흐름과 메타 데이터를 가져올 위치를 확인 하려면 두 가지 옵션을 사용할 수 있습니다. 먼저 사용자 흐름 이름이 ID 토큰의 `acr` 클레임에 포함됩니다. 다른 옵션은 요청을 실행할 때 `state` 매개 변수의 값에 사용자 흐름을 인코딩한 다음 이를 디코딩하여 어떤 사용자 흐름을 사용할지 결정하는 것입니다. 두 방법 중 하나는 유효합니다.

Openid connect Connect 메타 데이터 끝점에서 메타 데이터 문서를 가져온 후에는 RSA 256 공개 키를 사용 하 여 ID 토큰 서명의 유효성을 검사할 수 있습니다. 이 끝점에는 `kid` 클레임으로 식별 되는 키가 여러 개 있을 수 있습니다. ID 토큰의 헤더에는 `kid` 클레임도 포함되어 있으며, 이는 이러한 키 중에서 ID 토큰 서명에 사용된 키를 나타냅니다.

Azure AD B2C에서 토큰을 확인 하려면 지 수 (e) 및 모듈러스 (n)를 사용 하 여 공개 키를 생성 해야 합니다. 따라서 해당 프로그래밍 언어에서이 작업을 수행 하는 방법을 결정 해야 합니다. RSA 프로토콜을 사용 하 여 공개 키 생성에 대 한 공식 설명서는 다음 페이지에서 확인할 수 있습니다. https://tools.ietf.org/html/rfc3447#section-3.1

ID 토큰의 서명에 대한 유효성을 검사한 후에는 확인해야 할 몇 가지 클레임이 있습니다. 말합니다.

- `nonce` 클레임의 유효성을 검사하여 토큰 재생 공격을 방지합니다. 해당 값이 로그인 요청에 지정된 값이어야 합니다.
- `aud` 클레임의 유효성을 검사 하 여 응용 프로그램에 대 한 ID 토큰이 발급 되었는지 확인 합니다. 해당 값은 응용 프로그램의 응용 프로그램 ID 여야 합니다.
- `iat` 및`exp` 클레임의 유효성을 검사 하 여 ID 토큰이 만료 되지 않았는지 확인 합니다.

또한 수행해야 하는 몇 가지 추가 유효성 검사가 있습니다. 유효성 검사는 [Openid connect Connect Core 사양](https://openid.net/specs/openid-connect-core-1_0.html)에 자세히 설명 되어 있습니다. 시나리오에 따라 추가 클레임의 유효성을 검사할 수도 있습니다. 몇 가지 일반적인 유효성 검사는 다음과 같습니다.

- 사용자/조직이 응용 프로그램에 등록 했는지 확인 합니다.
- 사용자에게 적절한 권한 부여/권한이 있는지 확인
- Azure Multi-Factor Authentication과 같은 특정 강도의 인증이 발생했는지 확인

ID 토큰의 유효성을 검사 한 후 사용자와 세션을 시작할 수 있습니다. ID 토큰의 클레임을 사용 하 여 응용 프로그램의 사용자에 대 한 정보를 가져올 수 있습니다. 이 정보의 용도로 표시, 레코드 및 권한 부여가 있습니다.

## <a name="get-a-token"></a>토큰 가져오기

사용자 흐름이 실행 되는 웹 응용 프로그램이 필요한 경우 다음 몇 가지 섹션을 건너뛸 수 있습니다. 이러한 섹션은 웹 API에 대 한 인증 된 호출을 수행 해야 하는 웹 응용 프로그램에만 적용 되며 Azure AD B2C도 보호 됩니다.

`response_type=code+id_token`을 사용하여 `POST` 요청을 `/token` 엔드포인트로 보내 원하는 리소스에 대한 토큰에 대해 얻은 권한 부여 코드를 사용할 수 있습니다. Azure AD B2C에서는 요청에서 해당 범위를 지정 하 여 일반적인 방식으로 [다른 API에 대 한 액세스 토큰을 요청할](active-directory-b2c-access-tokens.md#request-a-token) 수 있습니다.

앱의 클라이언트 ID를 요청 된 범위로 사용 하는 규칙에 따라 앱 자체 백 엔드 웹 API에 대 한 액세스 토큰을 요청할 수도 있습니다 (해당 클라이언트 ID가 "대상"으로 지정 된 액세스 토큰을 생성 함).

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://fabrikamb2c.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| 매개 변수 | 필수 | Description |
| --------- | -------- | ----------- |
| p | 예 | 권한 부여 코드를 획득하는 데 사용된 사용자 흐름입니다. 이 요청에서는 다른 사용자 흐름을 사용할 수 없습니다. 이 매개 변수를 POST 본문이 아니라 쿼리 문자열에 추가 합니다. |
| client_id | 예 | [Azure Portal](https://portal.azure.com/) 응용 프로그램에 할당 된 응용 프로그램 ID입니다. |
| grant_type | 예 | 권한 유형입니다. 인증 코드 흐름의 경우 `authorization_code`이어야 합니다. |
| 범위 | 아니요 | 공백으로 구분된 범위 목록입니다. `openid` 범위는 사용자에게 로그인하고 id_token 매개 변수의 형식으로 사용자에 대한 데이터를 가져올 권한을 나타냅니다. 클라이언트와 동일한 응용 프로그램 ID로 표시 되는 응용 프로그램 자체 백 엔드 웹 API에 대 한 토큰을 가져오는 데 사용할 수 있습니다. 범위 `offline_access` 는 리소스에 대 한 확장 된 액세스를 위해 응용 프로그램에 새로 고침 토큰이 필요 함을 나타냅니다. |
| code | 예 | 사용자 흐름의 시작 부분에서 얻은 인증 코드입니다. |
| redirect_uri | 예 | 권한 부여 코드를 받은 애플리케이션의 `redirect_uri` 매개 변수입니다. |
| client_secret | 예 | [Azure Portal](https://portal.azure.com/)에서 생성 된 응용 프로그램 암호입니다. 이 애플리케이션 암호는 중요한 보안 아티팩트입니다. 서버에 안전하게 저장해야 합니다. 정기적으로이 클라이언트 암호를 변경 합니다. |

성공적인 토큰 응답은 다음과 같습니다.

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| 매개 변수 | Description |
| --------- | ----------- |
| not_before | epoch 시간에서 토큰은 유효한 것으로 간주되는 시간입니다. |
| token_type | 토큰 형식 값입니다. `Bearer`는 유일 하 게 지원 되는 유형입니다. |
| access_token | 사용자가 요청한 서명된 JWT 토큰입니다. |
| 범위 | 토큰이 유효한 범위입니다. |
| expires_in | 액세스 토큰이 유효한 시간(초)입니다. |
| refresh_token | OAuth 2.0 새로 고침 토큰입니다. 응용 프로그램은이 토큰을 사용 하 여 현재 토큰이 만료 된 후 추가 토큰을 가져올 수 있습니다. 새로 고침 토큰은 오랜 시간 동안 리소스에 대 한 액세스를 유지 하는 데 사용할 수 있습니다. 이 범위 `offline_access` 는 새로 고침 토큰을 수신 하기 위해 권한 부여 및 토큰 요청에서 모두 사용 해야 합니다. |

오류 응답은 다음과 같습니다.

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| 매개 변수 | 설명 |
| --------- | ----------- |
| error | 발생 한 오류 유형을 분류 하는 데 사용할 수 있는 코드입니다. |
| error_description | 인증 오류의 근본 원인을 식별 하는 데 도움이 될 수 있는 메시지입니다. |

## <a name="use-the-token"></a>토큰 사용

액세스 토큰을 성공적으로 얻었으므로 이제 `Authorization` 헤더에 포함하여 백 엔드 웹 API에 대한 요청에서 토큰을 사용할 수 있습니다.

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-token"></a>토큰 새로 고침

ID 토큰은 짧은 시간 동안 만료 됩니다. 토큰이 만료 된 후에도 계속 해 서 리소스에 액세스할 수 있도록 토큰을 새로 고칩니다. 다른 `POST` 요청을 `/token` 끝점에 제출 하 여 토큰을 새로 고칠 수 있습니다. 이번에는 `code` 매개 변수 대신 `refresh_token` 매개 변수를 제공합니다.

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://fabrikamb2c.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| 매개 변수 | 필수 | 설명 |
| --------- | -------- | ----------- |
| p | 예 | 원래의 새로 고침 토큰을 얻는 데 사용된 사용자 흐름입니다. 이 요청에서는 다른 사용자 흐름을 사용할 수 없습니다. 이 매개 변수를 POST 본문이 아니라 쿼리 문자열에 추가 합니다. |
| client_id | 예 | [Azure Portal](https://portal.azure.com/) 응용 프로그램에 할당 된 응용 프로그램 ID입니다. |
| grant_type | 예 | 권한 부여 코드 흐름의이 부분에 대 한 새로 고침 토큰 이어야 하는 권한 부여 형식입니다. |
| 범위 | 아니요 | 공백으로 구분된 범위 목록입니다. `openid` 범위는 사용자에게 로그인하고 ID 토큰 형식으로 사용자에 대한 데이터를 가져올 권한을 나타냅니다. 클라이언트와 동일한 응용 프로그램 ID로 표시 되는 응용 프로그램 자체 백 엔드 웹 API에 토큰을 보내는 데 사용할 수 있습니다. 범위 `offline_access` 는 리소스에 대 한 확장 된 액세스를 위해 응용 프로그램에 새로 고침 토큰이 필요 함을 나타냅니다. |
| redirect_uri | 아니요 | 권한 부여 코드를 받은 애플리케이션의 `redirect_uri` 매개 변수입니다. |
| refresh_token | 예 | 흐름의 두 번째 부분에서 획득 한 원래 새로 고침 토큰입니다. 이 `offline_access` 범위는 새로 고침 토큰을 수신 하기 위해 권한 부여 및 토큰 요청에서 모두 사용 해야 합니다. |
| client_secret | 예 | [Azure Portal](https://portal.azure.com/)에서 생성 된 응용 프로그램 암호입니다. 이 애플리케이션 암호는 중요한 보안 아티팩트입니다. 서버에 안전하게 저장해야 합니다. 정기적으로이 클라이언트 암호를 변경 합니다. |

성공적인 토큰 응답은 다음과 같습니다.

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| 매개 변수 | Description |
| --------- | ----------- |
| not_before | epoch 시간에서 토큰은 유효한 것으로 간주되는 시간입니다. |
| token_type | 토큰 형식 값입니다. `Bearer`는 유일 하 게 지원 되는 유형입니다. |
| access_token | 요청 된 서명 된 JWT 토큰입니다. |
| 범위 | 토큰이 유효한 범위입니다. |
| expires_in | 액세스 토큰이 유효한 시간(초)입니다. |
| refresh_token | OAuth 2.0 새로 고침 토큰입니다. 응용 프로그램은이 토큰을 사용 하 여 현재 토큰이 만료 된 후 추가 토큰을 가져올 수 있습니다. 새로 고침 토큰은 오랜 시간 동안 리소스에 대 한 액세스를 유지 하는 데 사용할 수 있습니다. |

오류 응답은 다음과 같습니다.

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| 매개 변수 | Description |
| --------- | ----------- |
| error | 발생 한 오류 유형을 분류 하는 데 사용할 수 있는 코드입니다. |
| error_description | 인증 오류의 근본 원인을 식별 하는 데 도움이 될 수 있는 메시지입니다. |

## <a name="send-a-sign-out-request"></a>로그아웃 요청 보내기

응용 프로그램에서 사용자를 로그 아웃 하려는 경우 응용 프로그램의 쿠키를 지우거 나 사용자를 사용 하 여 세션을 종료 하는 것 만으로는 충분 하지 않습니다. 로그 아웃 Azure AD B2C 사용자를 리디렉션합니다. 그렇게 하지 않으면 사용자가 자격 증명을 다시 입력 하지 않고도 응용 프로그램에 다시 인증할 수 있습니다.

앞서 설명한 openid connect Connect 메타 데이터 문서에 `end_session` 나열 된 끝점으로 사용자를 리디렉션할 수 있습니다.

```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| 매개 변수 | 필수 | 설명 |
| --------- | -------- | ----------- |
| p | 예 | 애플리케이션에서 사용자를 로그아웃하는 데 사용하려는 사용자 흐름입니다. |
| post_logout_redirect_uri | 아니요 | 성공적으로 로그 아웃 한 후에 사용자가 리디렉션되는 URL입니다. 포함 되어 있지 않으면 Azure AD B2C 사용자에 게 일반 메시지를 표시 합니다. |

사용자를 `end_session` 끝점으로 전달 하면 Azure AD B2C를 사용 하 여 일부 사용자의 single sign-on 상태를 지우지만 사용자가 IDP (소셜 id 공급자) 세션에서 로그 아웃 하지 않습니다. 사용자가 후속 로그인 중에 동일한 IDP를 선택 하면 자격 증명을 입력 하지 않고 다시 인증 됩니다. 사용자가 응용 프로그램에서 로그 아웃 하려는 경우 반드시 Facebook 계정에서 로그 아웃 하려는 것은 아닙니다. 그러나 로컬 계정을 사용 하는 경우 사용자의 세션이 제대로 종료 됩니다.

