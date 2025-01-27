---
title: 포함 파일
description: 포함 파일
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 301abe95b245603e5414eef84ce74cdc8de01d19
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509888"
---
# <a name="call-the-microsoft-graph-api-from-a-windows-desktop-app"></a>Windows 데스크톱 앱에서 Microsoft Graph API 호출

이 가이드에서는 네이티브 Windows Desktop.NET(XAML) 애플리케이션에서 액세스 토큰을 사용하여 Microsoft Graph API를 호출하는 방법을 보여줍니다. 이 앱은 개발자 v2.0 엔드포인트에 대한 Microsoft ID 플랫폼의 액세스 토큰을 필요로 하는 기타 API에 액세스할 수 있습니다. 이 플랫폼은 이전에 Azure AD라고 명명되었습니다.

이 가이드를 완료하면 애플리케이션은 개인 계정(outlook.com, live.com 등 포함)을 사용하는 보호된 API를 호출할 수 있습니다. 또한 애플리케이션은 Azure Active Directory를 사용하는 모든 회사 또는 조직의 회사 및 학교 계정을 사용합니다.  

> [!NOTE]
> 이 가이드에는 Visual Studio 2015 업데이트 3, Visual Studio 2017 또는 Visual Studio 2019가 필요합니다. 이러한 버전이 설치되어 있지 않나요? [체험용 Visual Studio 2019를 다운로드](https://www.visualstudio.com/downloads/)합니다.

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>이 가이드에서 생성된 샘플 앱의 작동 원리

![이 자습서에서 생성된 샘플 앱의 작동 방식 표시](./media/active-directory-develop-guidedsetup-windesktop-intro/windesktophowitworks.svg)

이 가이드를 사용하여 만든 샘플 애플리케이션을 사용하면 Windows 데스크톱 애플리케이션이 Microsoft Graph API를 쿼리하거나 Web API가 Microsoft ID 플랫폼 엔드포인트에서 토큰을 수락할 수 있습니다. 이 시나리오에서는 인증 헤더를 통해 HTTP 요청에 토큰을 추가합니다. MSAL(Microsoft 인증 라이브러리)에서는 토큰 획득 및 갱신을 처리합니다.

## <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>보호되는 Web API에 액세스하기 위한 토큰 획득 처리

사용자가 인증된 후에 샘플 애플리케이션은 개발자를 위한 Microsoft ID 플랫폼으로 보호되는 Microsoft Graph API 또는 Web API를 쿼리하는 데 사용할 수 있는 토큰을 수신합니다.

Microsoft Graph와 같은 API에는 특정 리소스에 대한 액세스를 허용하는 토큰이 필요합니다. 예를 들어 사용자 프로필을 읽고, 사용자 일정에 액세스하고, 메일을 보내는 데 토큰이 필요합니다. 애플리케이션에서는 MSAL을 사용하여 액세스 토큰을 요청하고 API 범위를 지정하여 이러한 리소스에 액세스할 수 있습니다. 그런 다음 이 액세스 토큰은 보호되는 리소스에 대한 모든 호출에 사용될 수 있도록 HTTP 인증 헤더에 추가됩니다.

MSAL은 사용자를 대신해 액세스 토큰 캐싱 및 새로 고침을 관리하므로 애플리케이션에서 이러한 작업을 수행할 필요가 없습니다.

## <a name="nuget-packages"></a>NuGet 패키지

이 가이드에서는 다음 NuGet 패키지를 사용합니다.

|라이브러리|설명|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft 인증 라이브러리(MSAL.NET)|
