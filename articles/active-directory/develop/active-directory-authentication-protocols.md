---
title: Azure Active Directory 인증 프로토콜 | Microsoft Docs
description: Azure AD(Active Directory)에서 지원되는 인증 프로토콜 개요
documentationcenter: dev-center-name
author: rwike77
services: active-directory
manager: CelesteDG
editor: ''
ms.assetid: 7a838ae2-c24c-4304-b6c0-e77fb888e6c0
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: hirsin
ms.collection: M365-identity-device-management
ms.openlocfilehash: f964c5882432ae0637039e32ca961008e8223b6b
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67612575"
---
# <a name="azure-active-directory-authentication-protocols"></a>Azure Active Directory 인증 프로토콜
Azure AD(Azure Active Directory)는 가장 널리 사용되는 다수의 인증 및 권한 부여 프로토콜을 지원합니다. 이 섹션의 항목에서는 Azure AD에서 지원되는 프로토콜 및 이 프로토콜의 구현에 대해 설명합니다. 이 항목에서는 지원되는 클레임 유형을 검토하고, 페더레이션 메타데이터 사용에 대해 소개하고, 자세한 OAuth 2.0. 및 SAML 2.0 프로토콜 참조 설명서 및 문제 해결 섹션을 소개합니다.

## <a name="authentication-protocols-articles-and-reference"></a>인증 프로토콜 기사 및 참조
* [Azure AD의 서명 키 롤오버에 대한 중요한 정보](active-directory-signing-key-rollover.md) - Azure AD의 서명 키 롤오버 주기, 키를 자동으로 업데이트하기 위해 수행할 수 있는 변경 사항, 가장 일반적인 애플리케이션 시나리오를 업데이트하는 방법에 대해 알아봅니다.
* [지원되는 토큰 및 클레임 유형](v1-id-and-access-tokens.md) - Azure AD에서 발급되는 토큰의 클레임에 대해 알아봅니다.
* [페더레이션 메타데이터](azure-ad-federation-metadata.md) - Azure AD에서 생성되는 메타데이터 문서를 찾고 해석하는 방법을 알아봅니다.
* [Azure AD의 OAuth 2.0](v1-protocols-oauth-code.md) - Azure AD의 OAuth 2.0 구현에 대해 알아봅니다.
* [OpenID Connect 1.0](v1-protocols-openid-connect-code.md) - 권한 부여 프로토콜 OAuth 2.0을 인증에 사용하는 방법을 알아봅니다.
* [클라이언트 자격 증명을 사용한 서비스 간 호출](v1-oauth2-client-creds-grant-flow.md) - 서비스 간 호출을 위해 OAuth 2.0 클라이언트 자격 증명 부여 흐름을 사용하는 방법을 알아봅니다.
* [On-Behalf-Of 흐름을 사용한 서비스 간 호출](v1-oauth2-on-behalf-of-flow.md) - 서비스 간 호출을 위해 OAuth 2.0 On-Behalf-Of 흐름을 사용하는 방법을 알아봅니다.
* [SAML 프로토콜 참조](active-directory-saml-protocol-reference.md) - Azure AD의 Single Sign On 및 Single Sign Out SAML 프로필에 대해 알아봅니다.

## <a name="see-also"></a>참고 항목
[Azure Active Directory 개발자 가이드](v1-overview.md)

[Active Directory 코드 샘플](sample-v1-code.md)
