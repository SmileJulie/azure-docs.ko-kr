---
title: .NET 용 Microsoft Authentication Library의 클라이언트 어설션 | Microsoft
description: Microsoft Authentication Library for .NET (MSAL.NET)의 기밀 클라이언트 응용 프로그램에 대 한 서명 된 클라이언트 어설션 지원에 대해 알아봅니다.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/16/2019
ms.author: jmprieur
ms.reviewer: ''
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3a938f85b2047ea5cceada98df6adba2c560c1a1
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68278669"
---
# <a name="confidential-client-assertions"></a>기밀 클라이언트 어설션
기밀 클라이언트 응용 프로그램은 id를 증명 하기 위해 Azure AD와 암호를 교환 합니다. 비밀은 다음과 같을 수 있습니다.
- 클라이언트 암호 (응용 프로그램 암호)입니다.
- 표준 클레임을 포함 하는 서명 된 어설션을 빌드하는 데 사용 되는 인증서입니다.

이 비밀은 서명 된 어설션이 직접 일 수도 있습니다.

MSAL.NET에는 기밀 클라이언트 앱에 자격 증명 또는 어설션을 제공 하는 네 가지 메서드가 있습니다.
- `.WithClientSecret()`
- `.WithCertificate()`
- `.WithSignedAssertion()`
- `.WithClientClaims()`

### <a name="signed-assertions"></a>서명 된 어설션

서명 된 클라이언트 어설션은 Azure AD, Base64로 인코딩된 필수 인증 클레임을 포함 하는 페이로드에 서명 된 JWT 형식을 사용 합니다. 이를 사용하려면:

```CSharp
string signedClientAssertion = ComputeAssertion();
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
                                          .WithClientAssertion(signedClientAssertion)
                                          .Build();
```

Azure AD에서 필요한 클레임은 다음과 같습니다.

클레임 형식 | 값 | 설명
---------- | ---------- | ----------
aud | https://login.microsoftonline.com/{tenantId}/v2.0 | "Aud" (대상) 클레임은 JWT가 의도 된 받는 사람을 식별 합니다 (여기서는 Azure AD) [RFC 7519, 섹션 4.1.3]을 참조 하세요.
exp | 27 2019 15:04:17 GMT + 0200 (결혼 전 일광 절약 시간) | "exp"(만료 시간) 클레임은 JWT가 그 이후에는 처리를 허용하지 않아야 하는 만료 시간을 식별합니다. [RFC 7519, Section 4.1.4]를 참조 하세요.
iss | ClientID | "Iss" (발급자) 클레임은 JWT를 발급 한 보안 주체를 식별 합니다. 이 클레임의 처리는 응용 프로그램 마다 다릅니다. "Iss" 값은 StringOrURI 값을 포함 하는 대/소문자를 구분 하는 문자열입니다. [RFC 7519, 섹션 4.1.1]
jti | (Guid) | "Jti" (JWT ID) 클레임은 JWT에 대 한 고유 식별자를 제공 합니다. 식별자 값은 동일한 값이 다른 데이터 개체에 실수로 할당 될 확률이 무시 되도록 하는 방식으로 할당 되어야 합니다. 응용 프로그램에서 여러 발급자를 사용 하는 경우 여러 발급자가 생성 한 값 사이에서 충돌을 방지 해야 합니다. "Jti" 클레임은 JWT 재생을 방지 하는 데 사용할 수 있습니다. "Jti" 값은 대/소문자를 구분 하는 문자열입니다. [RFC 7519, 섹션 4.1.7]
nbf | 27 2019 14:54:17 GMT + 0200 (결혼 전 일광 절약 시간) | "nbf"(not before) 클레임은 JWT가 그 이전에는 처리를 허용하지 않아야 하는 시간을 식별합니다. [RFC 7519, 섹션 4.1.5]
sub | ClientID | "Sub" (주체) 클레임은 JWT의 주체를 식별 합니다. JWT의 클레임은 일반적으로 주체에 대 한 문입니다. 주체 값의 범위는 발급자의 컨텍스트에서 로컬로 고유 하거나 전역적으로 고유 해야 합니다. 자세한 내용은 [RFC 7519, Section 4.1.2]를 참조 하십시오.

이러한 클레임을 작성 하는 방법의 예는 다음과 같습니다.

```CSharp
private static IDictionary<string, string> GetClaims()
{
      //aud = https://login.microsoftonline.com/ + Tenant ID + /v2.0
      string aud = "https://login.microsoftonline.com/72f988bf-86f1-41af-hd4m-2d7cd011db47/v2.0";

      string ConfidentialClientID = "61dab2ba-145d-4b1b-8569-bf4b9aed5dhb" //client id
      const uint JwtToAadLifetimeInSeconds = 60 * 10; // Ten minutes
      DateTime validFrom = DateTime.UtcNow;
      var nbf = ConvertToTimeT(validFrom);
      var exp = ConvertToTimeT(validFrom + TimeSpan.FromSeconds(JwtToAadLifetimeInSeconds));

      return new Dictionary<string, string>()
           {
                { "aud", aud },
                { "exp", exp.ToString() },
                { "iss", ConfidentialClientID },
                { "jti", Guid.NewGuid().ToString() },
                { "nbf", nbf.ToString() },
                { "sub", ConfidentialClientID }
            };
}
```

서명 된 클라이언트 어설션을 특수 하 게 만드는 방법은 다음과 같습니다.

```CSharp
string Encode(byte[] arg)
{
    char Base64PadCharacter = '=';
    char Base64Character62 = '+';
    char Base64Character63 = '/';
    char Base64UrlCharacter62 = '-';
    char Base64UrlCharacter63 = '_';

    string s = Convert.ToBase64String(arg);
    s = s.Split(Base64PadCharacter)[0]; // RemoveAccount any trailing padding
    s = s.Replace(Base64Character62, Base64UrlCharacter62); // 62nd char of encoding
    s = s.Replace(Base64Character63, Base64UrlCharacter63); // 63rd char of encoding

    return s;
}

string GetAssertion()
{
    //Signing with SHA-256
    string rsaSha256Signature = "http://www.w3.org/2001/04/xmldsig-more#rsa-sha256";
    X509Certificate2 certificate = ReadCertificate(config.CertificateName);

    //Create RSACryptoServiceProvider
    var x509Key = new X509AsymmetricSecurityKey(certificate);
    var privateKeyXmlParams = certificate.PrivateKey.ToXmlString(true);
    var rsa = new RSACryptoServiceProvider();
    rsa.FromXmlString(privateKeyXmlParams);

    //alg represents the desired signing algorithm, which is SHA-256 in this case
    //kid represents the certificate thumbprint
    var header = new Dictionary<string, string>()
         {
              { "alg", "RS256"},
              { "kid", Encode(Certificate.GetCertHash()) }
         };

    //Please see the previous code snippet on how to craft claims for the GetClaims() method
    string token = Encode(Encoding.UTF8.GetBytes(JObject.FromObject(header).ToString())) + "." + Encode(Encoding.UTF8.GetBytes(JObject.FromObject(GetClaims())));

    string signature = Encode(rsa.SignData(Encoding.UTF8.GetBytes(token), new SHA256Cng()));
    string SignedAssertion = string.Concat(token, ".", signature);
    return SignedAssertion;
}
```

### <a name="withclientclaims"></a>WithClientClaims

`WithClientClaims(X509Certificate2 certificate, IDictionary<string, string> claimsToSign, bool mergeWithDefaultClaims = true)`기본적으로는 Azure AD에서 예상한 클레임 및 전송 하려는 추가 클라이언트 클레임을 포함 하는 서명 된 어설션을 생성 합니다. 다음은이 작업을 수행 하는 방법에 대 한 코드 조각입니다.

```CSharp
string ipAddress = "192.168.1.2";
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
                                          .WithAuthority(new Uri(config.Authority))
                                          .WithClientClaims(certificate, 
                                                                      new Dictionary<string, string> { { "client_ip", ipAddress } })
                                          .Build();

```

사전에 전달 하는 클레임 중 하나가 필수 클레임 중 하 나와 동일한 경우 추가 클레임 값이 고려 됩니다. MSAL.NET에서 계산 된 클레임을 재정의 합니다.

Azure AD에서 요구 하는 필수 클레임을 포함 하 여 고유한 클레임을 제공 하려는 경우 `false` `mergeWithDefaultClaims` 매개 변수에 대해를 전달 합니다.
