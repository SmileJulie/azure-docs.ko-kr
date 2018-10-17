---
title: Azure Active Directory B2C의 사용자 지정 정책에서 SAML 기술 프로필 정의 | Microsoft Docs
description: Azure Active Directory B2C의 사용자 지정 정책에서 SAML 기술 프로필을 정의하는 방법을 설명합니다.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 301ae251413cc174f115479e9ebef2310aa83ba7
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162445"
---
# <a name="define-a-saml-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C 사용자 지정 정책에서 SAML 기술 프로필 정의

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure AD(Active Directory) B2C에서는 SAML 2.0 ID 공급자를 지원합니다. 이 문서에서는 이 표준 프로토콜을 지원하는 클레임 공급자와 상호 작용하기 위한 기술 프로필에 대해 구체적으로 설명합니다. SAML 기술 프로필을 사용하면 AD-FS 및 Salesforce와 같은 SAML 기반 ID 공급자를 페더레이션할 수 있습니다. 그러면 사용자가 기존 소셜 또는 엔터프라이즈 ID로 로그인할 수 있게 됩니다.

## <a name="metadata-exchange"></a>메타데이터 교환

메타데이터는 서비스 공급자 또는 ID 공급자와 같은 SAML 당사자의 구성을 표시하기 위해 SAML 프로토콜에서 사용되는 정보입니다. 메타데이터는 로그인/로그아웃, 인증서, 로그인 방법 등과 같은 서비스의 위치를 정의합니다. ID 공급자는 메타데이터를 사용하여 Azure AD B2C와 통신하는 방법을 확인합니다. 메타데이터는 XML 형식으로 구성되며, 다른 당사자가 메타데이터 무결성의 유효성을 검사할 수 있도록 디지털 서명으로 서명될 수 있습니다. Azure AD B2C는 SAML ID 공급자와 페더레이션될 때 SAML 요청을 시작하며 SAML 응답을 기다리는 서비스 공급자 역할을 합니다. 그리고 경우에 따라서는 원치 않는 SAML 인증(ID 공급자가 시작하는 인증이라고도 함)을 제외하기도 합니다. 

양 당사자에서 “정적 메타데이터” 또는 “동적 메타데이터”로 메타데이터를 구성할 수 있습니다. 정적 모드에서는 전체 메타데이터를 한 당사자에서 복사하여 다른 당사자에서 설정합니다. 동적 모드에서는 상대 당사자가 동적으로 구성을 읽는 동안 메타데이터에 대한 URL을 설정합니다. 두 모드의 원칙은 동일합니다. 즉, ID 공급자에서 Azure AD B2C 기술 프로필의 메타데이터를 설정하고 Azure AD B2C에서 ID 공급자의 메타데이터를 설정합니다.

각 SAML ID 공급자에서는 서로 단계를 수행하여 서비스 공급자(여기서는 Azure AD B2C)를 표시 및 설정하고 ID 공급자에서 Azure AD B2C 메타데이터를 설정합니다. 이 단계를 수행하는 방법의 지침은 사용 중인 ID 공급자의 설명서를 참조하세요.

다음 예제에서는 Azure AD B2C 기술 프로필의 SAML 메타데이터에 대한 URL 주소를 보여 줍니다.

```
https://login.microsoftonline.com/te/your-tenant/your-policy/samlp/metadata?idptp=your-technical-profile
```

다음 값을 바꿉니다.

- **your-tenant**: 실제 테넌트 이름(예: your-tenant.onmicrosoft.com)
- **your-policy**를 정책 이름으로. SAML 공급자 기술 프로필을 구성한 정책 또는 해당 정책에서 상속하는 정책을 사용합니다.
- **your-technical-profile**: 실제 SAML ID 공급자 기술 프로필 이름

## <a name="digital-signing-certificates-exchange"></a>디지털 서명 인증서 교환

Azure AD B2C와 SAML ID 공급자 간의 트러스트를 구축하려면 유효한 X509 인증서에 개인 키를 제공해야 합니다. 이렇게 하려면 개인 키(.pfx 파일)가 포함된 인증서를 Azure AD B2C 정책 키 저장소에 업로드합니다. Azure AD B2C는 제공된 인증서를 사용하여 SAML 로그인 요청에 디지털 서명을 합니다. 

인증서는 다음과 같은 방식으로 사용됩니다.

- Azure AD B2C가 인증서의 Azure AD B2C 개인 키를 사용하여 SAML 요청을 생성하고 서명을 합니다. SAML 요청이 ID 공급자에게 전송되면 ID 공급자가 인증서의 Azure AD B2C 공개 키를 사용하여 요청 유효성을 검사합니다. 기술 프로필 메타데이터를 통해 Azure AD B2C 공용 인증서에 액세스할 수 있습니다. .cer 파일을 SAML ID 공급자에 수동으로 업로드할 수도 있습니다.
- ID 공급자가 인증서의 ID 공급자 개인 키를 사용하여 Azure AD B2C로 전송된 데이터에 서명을 합니다. Azure AD B2C가 ID 공급자의 공용 인증서를 사용하여 데이터 유효성을 검사합니다. 각 ID 공급자의 설정 단계는 서로 다릅니다. 설정 방법의 지침은 사용 중인 ID 공급자의 설명서를 참조하세요. Azure AD B2C에서 정책은 ID 공급자 메타데이터를 사용하여 인증서 공개 키에 액세스해야 합니다.

대부분의 시나리오에서는 자체 서명된 인증서를 사용하면 됩니다. 프로덕션 환경에서는 인증 기관에서 발급한 X509 인증서를 사용하는 것이 좋습니다. 또한 이 문서 뒷부분에 설명되어 있는 것처럼 비프로덕션 환경에서는 양쪽에서 모두 SAML 서명을 사용하지 않도록 설정할 수 있습니다.

아래 다이어그램에는 메타데이터 및 인증서 교환 과정이 나와 있습니다.

![메타데이터 및 인증서 교환](media/saml-technical-profile/technical-profile-idp-saml-metadata.png)


## <a name="digital-encryption"></a>디지털 암호화

SAML 응답 어설션을 암호화하기 위해 ID 공급자는 항상 Azure AD B2C 기술 프로필 내 암호화 인증서의 공개 키를 사용합니다. Azure AD B2C는 데이터 암호를 해독해야 할 때 암호화 인증서의 개인 부분을 사용합니다.

SAML 응답 어설션을 암호화하려면 다음 단계를 수행합니다.

1. 개인 키(.pfx 파일)가 포함된 유효한 X509 인증서를 Azure AD B2C 정책 키 저장소에 업로드합니다.
2. 식별자가 `SamlAssertionDecryption`인 **CryptographicKey** 요소를 기술 프로필 **CryptographicKeys** 컬렉션에 추가합니다. **StorageReferenceId**를 1단계에서 만든 정책 키의 이름으로 설정합니다.
3. 기술 프로필 메타데이터 **WantsEncryptedAssertions**를 `true`로 설정합니다.
4. 새 Azure AD B2C 기술 프로필 메타데이터로 ID 공급자를 업데이트합니다. 인증서의 공개 키를 포함하며 **use** 속성이 `encryption`으로 설정된 **KeyDescriptor**가 표시되어야 합니다.

다음 예제에서는 메타데이터의 Azure AD B2C 기술 프로필 암호화 섹션을 보여 줍니다.

```XML
<KeyDescriptor use="encryption">
  <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
    <X509Data>
      <X509Certificate>valid certificate</X509Certificate>
    </X509Data>
  </KeyInfo>
</KeyDescriptor>
```

## <a name="identity-provider-initiated-flow"></a>ID 공급자가 시작하는 흐름

IDP가 시작한 Single Sign-On 세션(원치 않는 요청)에서는 원치 않는 SAML 응답이 서비스 공급자(여기서는 Azure AD B2C 기술 프로필)로 전송됩니다. 이 흐름에서는 사용자가 웹 응용 프로그램을 먼저 거치지 않고 ID 공급자로 이동됩니다. 요청이 전송되면 ID 공급자가 사용자에게 인증 페이지를 제공합니다. 사용자가 로그인을 완료하면 어설션이 포함된 SAML 응답과 함께 요청이 Azure AD B2C로 리디렉션됩니다. Azure AD B2C는 어설션을 읽고 새 SAML 토큰을 발급한 다음 사용자를 신뢰 당사자 응용 프로그램으로 다시 리디렉션합니다. **AssertionConsumerService** 요소의 **Location** 속성이 리디렉션을 수행합니다.


![SAML IDP 시작](media/saml-technical-profile/technical-profile-idp-saml-idp-initiated.png) 

ID 공급자가 시작하는 흐름을 만들 때는 다음 정책 요구 사항을 고려하세요.

- 첫 번째 오케스트레이션 단계는 SAML 기술 프로필을 가리키는 단일 클레임 교환이어야 합니다.
- 기술 프로필은 `true`로 설정된 **IdpInitiatedProfileEnabled** 메타데이터 항목을 포함해야 합니다.
- 신뢰 당사자 정책은 SAML 신뢰 당사자여야 합니다.
- 신뢰 당사자 정책은 `true`로 설정된 **IdpInitiatedProfileEnabled** 메타데이터 항목을 포함해야 합니다.
- 원치 않는 응답은 `/your-tenant/your-policy/samlp/sso/assertionconsumer` 엔드포인트로 전송되어야 합니다. 응답에 포함된 모든 릴레이 상태는 신뢰 당사자에게 전달됩니다. **your-tenant**는 실제 테넌트 이름으로 바꾸고, **your-policy**는 신뢰 당사자 정책 이름으로 바꿉니다.
    
## <a name="protocol"></a>프로토콜

Protocol 요소의 **Name** 특성은 `SAML2`로 설정해야 합니다. 

## <a name="metadata"></a>Metadata

| 특성 | 필수 | 설명 |
| --------- | -------- | ----------- |
| PartnerEntity | yes | SAML ID 공급자의 메타데이터 URL입니다. ID 공급자 메타데이터를 복사하여 CDATA 요소 `<![CDATA[Your IDP metadata]]>` 내에 추가합니다. |
| WantsSignedRequests | 아니요 | 기술 프로필에서 모든 발신 인증 요청에 서명을 해야 하는지 여부를 나타냅니다. 가능한 값은 `true` 또는 `false`입니다. 기본값은 `true`입니다. 값을 `true`로 설정하는 경우에는 **SamlMessageSigning** 암호화 키를 지정하고 모든 발신 인증 요청에 서명을 해야 합니다. 값을 `false`로 설정하는 경우에는 **SigAlg** 및 **Signature** 매개 변수(쿼리 문자열 또는 게시 매개 변수)가 요청에서 생략됩니다. 이 메타데이터는 메타데이터 **AuthnRequestsSigned** 특성도 제어합니다. 해당 특성은 ID 공급자와 공유되는 Azure AD B2C 기술 프로필의 메타데이터 출력입니다. |
| XmlSignatureAlgorithm | 아니요 | Azure AD B2C가 SAML 요청에 서명하는 데 사용하는 메서드입니다. 이 메타데이터는 SAML 요청의 **SigAlg** 매개 변수(쿼리 문자열 또는 게시 매개 변수) 값을 제어합니다. 가능한 값은 `Sha256`, `Sha384`, `Sha512` 또는 `Sha1`입니다. 양쪽의 서명 알고리즘을 같은 값으로 구성해야 합니다. 인증서가 지원하는 알고리즘만 사용하세요. | 
| WantsSignedAssertions | 아니요 | 기술 프로필에서 모든 수신 어설션에 서명을 해야 하는지 여부를 나타냅니다. 가능한 값은 `true` 또는 `false`입니다. 기본값은 `true`입니다. 값을 `true`로 설정하는 경우 ID 공급자가 Azure AD B2C로 전송하는 모든 어설션 섹션 `saml:Assertion`에 서명을 해야 합니다. 값을 `false`로 설정하는 경우에는 ID 공급자가 어설션에 서명을 하지 않아야 하며, 서명을 하더라도 Azure AD B2C가 서명의 유효성을 검사하지 않습니다. 이 메타데이터는 메타데이터 플래그 **WantsAssertionsSigned**도 제어합니다. 해당 플래그는 ID 공급자와 공유되는 Azure AD B2C 기술 프로필의 메타데이터 출력입니다. 어설션 유효성 검사를 사용하지 않도록 설정하는 경우에는 응답 서명 유효성 검사도 사용하지 않도록 설정하는 것이 좋습니다. 자세한 내용은 **ResponsesSigned**를 참조하세요. |
| ResponsesSigned | 아니요 | 가능한 값은 `true` 또는 `false`입니다. 기본값은 `true`입니다. 값을 `false`로 설정하는 경우에는 ID 공급자가 SAML 응답에 서명을 하지 않아야 하며, 서명을 하더라도 Azure AD B2C가 서명의 유효성을 검사하지 않습니다. 값을 `true`로 설정하는 경우에는 ID 공급자가 Azure AD B2C로 전송하는 SAML 응답에 서명이 되며 해당 서명의 유효성을 검사해야 합니다. SAML 응답 유효성 검사를 사용하지 않도록 설정하는 경우에는 어설션 서명 유효성 검사도 사용하지 않도록 설정하는 것이 좋습니다. 자세한 내용은 **WantsSignedAssertions**를 참조하세요. |
| WantsEncryptedAssertions | 아니요 | 기술 프로필에서 모든 수신 어설션을 암호화해야 하는지 여부를 나타냅니다. 가능한 값은 `true` 또는 `false`입니다. 기본값은 `false`입니다. 값을 `true`로 설정하는 경우에는 ID 공급자가 Azure AD B2C로 전송하는 어설션에 서명을 해야 하며 **SamlAssertionDecryption** 암호화 키를 지정해야 합니다. 값을 `true`로 설정하는 경우에는 Azure AD B2C 기술 프로필의 메타데이터에 **encryption** 섹션이 포함됩니다. ID 공급자는 메타데이터를 읽고 Azure AD B2C 기술 프로필의 메타데이터에서 제공되는 공개 키를 사용하여 SAML 응답 어설션을 암호화합니다. 어설션 암호화를 사용하도록 설정하는 경우에는 응답 서명 유효성 검사도 사용하지 않도록 설정해야 할 수 있습니다. 자세한 내용은 **ResponsesSigned**를 참조하세요. | 
| IdpInitiatedProfileEnabled | 아니요 |SAML ID 공급자 프로필에 의해 시작된 Single Sing-On 세션 프로필이 사용되는지 여부를 나타냅니다. 가능한 값은 `true` 또는 `false`입니다. 기본값은 `false`입니다. ID 공급자가 시작한 플로우에서는 사용자가 외부에서 인증되며 원치 않는 응답이 Azure AD B2C로 전송됩니다. 그러면 Azure AD B2C는 토큰을 사용하고 오케스트레이션 단계를 실행한 다음 신뢰 당사자 응용 프로그램에 응답을 보냅니다. |

## <a name="cryptographic-keys"></a>암호화 키

**CryptographicKeys** 요소에는 다음과 같은 특성이 포함됩니다.

| 특성 |필수 | 설명 |
| --------- | ----------- | ----------- |
| SamlMessageSigning |yes | SAML 메시지에 서명을 하는 데 사용할 X509 인증서(RSA 키 집합)입니다. Azure AD B2C는 이 키를 사용해 요청에 서명을 한 다음 ID 공급자에 요청을 전송합니다. |
| SamlAssertionDecryption |yes | SAML 메시지의 암호를 해독하는 데 사용할 X509 인증서(RSA 키 집합)입니다. ID 공급자가 이 인증서를 제공해야 합니다. Azure AD B2C는 이 인증서를 사용하여 ID 공급자가 전송하는 데이터의 암호를 해독합니다. |
| MetadataSigning |아니요 | SAML 메타데이터에 서명을 하는 데 사용할 X509 인증서(RSA 키 집합)입니다. Azure AD B2C는 이 키를 사용하여 메타데이터에 서명을 합니다.  |

## <a name="examples"></a>예

- [사용자 지정 정책을 사용하여 SAML ID 공급자로 ADFS 추가](active-directory-b2c-custom-setup-adfs2016-idp.md)
- [SAML을 통해 Salesforce 계정을 사용하여 로그인](active-directory-b2c-setup-sf-app-custom.md)












