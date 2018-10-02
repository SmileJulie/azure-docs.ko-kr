---
title: Azure Active Directory의 웹앱
description: 웹앱을 소개하고 이 앱 유형에 대한 프로토콜 흐름, 등록 및 토큰 만료 기본 사항을 설명합니다.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: saeeda, jmprieur, andret
ms.custom: aaddev
ms.openlocfilehash: ea1117e202868465e3ea4004d21cf62d175a357a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46963920"
---
# <a name="web-apps"></a>웹 앱

웹앱은 웹 브라우저의 사용자를 웹 응용 프로그램에 인증하는 응용 프로그램입니다. 이 시나리오에서는 웹 응용 프로그램이 사용자의 브라우저를 Azure AD에 로그인하도록 지시합니다. Azure AD는 보안 토큰에 사용자에 대한 클레임이 포함된 로그인 응답을 사용자의 브라우저를 통해 반환합니다. 이 시나리오에서는 OpenID Connect, SAML 2.0 및 WS-Federation 프로토콜을 사용한 로그온이 지원됩니다.

## <a name="diagram"></a>다이어그램

![브라우저-웹 응용 프로그램의 인증 흐름](./media/authentication-scenarios/web_browser_to_web_api.png)

## <a name="protocol-flow"></a>프로토콜 흐름

1. 사용자가 응용 프로그램을 방문할 때 로그인해야 하는 경우 로그인 요청을 통해 Azure AD의 인증 엔드포인트로 리디렉션됩니다.
1. 사용자가 로그인 페이지에서 로그인합니다.
1. 인증에 성공하면 Azure AD에서 인증 토큰을 만들고, Azure Portal에서 구성한 응용 프로그램의 회신 URL에 대한 로그인 응답을 반환합니다. 프로덕션 응용 프로그램의 경우 이 회신 URL은 HTTPS여야 합니다. 반환된 토큰에는 응용 프로그램이 토큰의 유효성을 검사하는 데 필요한, 사용자 및 Azure AD에 대한 클레임이 포함됩니다.
1. 응용 프로그램이 Azure AD에 대한 페더레이션 메타데이터 문서에서 제공되는 공개 서명 키 및 발급자 정보를 사용하여 토큰의 유효성을 검사합니다. 응용 프로그램이 토큰의 유효성을 검사한 후에 사용자의 새 세션을 시작합니다. 이 세션에서 사용자는 응용 프로그램이 만료될 때까지 응용 프로그램에 액세스할 수 있습니다.

## <a name="code-samples"></a>코드 샘플

웹 브라우저-웹 응용 프로그램 시나리오에 대한 코드 샘플을 참조하세요. 그리고 새로운 샘플이 자주 추가되므로 자주 확인해 보세요.

## <a name="app-registration"></a>앱 등록

웹앱을 등록하려면 [Azure AD v2.0 엔드포인트에 앱 등록](quickstart-v2-register-an-app.md)을 참조하세요.

* 단일 테넌트 - 조직 전용 응용 프로그램을 빌드하는 경우 Azure Portal을 사용하여 해당 응용 프로그램을 회사 디렉터리에 등록해야 합니다.
* 다중 테넌트 - 조직 외부 사용자가 사용할 수 있는 응용 프로그램을 빌드하는 경우 해당 응용 프로그램을 회사 디렉터리에 등록해야 하며, 동시에 응용 프로그램을 사용할 각 조직의 디렉터리에도 등록해야 합니다. 해당 디렉터리에서 응용 프로그램을 사용할 수 있게 만들려면 고객이 응용 프로그램에 동의할 수 있게 하는 등록 프로세스를 포함하면 됩니다. 사용자가 응용 프로그램에 등록할 때 응용 프로그램에 필요한 권한을 보여 주는 대화 상자가 나타난 다음 동의하는 옵션이 나타납니다. 필요한 권한에 따라, 다른 조직의 관리자가 동의해야 할 수도 있습니다. 사용자 또는 관리자가 동의하면 응용 프로그램이 이들의 디렉터리에 등록됩니다.

## <a name="token-expiration"></a>토큰 만료

Azure AD에서 발급된 토큰의 수명이 만료되면 사용자의 세션이 만료됩니다. 원한다면 비활성 기간을 기준으로 사용자를 로그아웃시키는 등, 응용 프로그램이 세션 만료 시간을 단축할 수 있습니다. 세션이 만료되면 사용자에게는 다시 로그인하라는 메시지가 표시됩니다.

## <a name="next-steps"></a>다음 단계

* 다른 [응용 프로그램 유형 및 시나리오](app-types.md)에 대해 자세히 알아보기
* Azure AD [인증 기본 사항](authentication-scenarios.md)에 대해 자세히 알아보기