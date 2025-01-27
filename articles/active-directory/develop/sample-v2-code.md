---
title: Microsoft identity 플랫폼 코드 샘플 | Microsoft Docs
description: 플랫폼 ((v2.0 끝점) 코드 샘플, 시나리오 별로 사용할 수 있는 Microsoft id의 인덱스를 제공 합니다.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/26/2018
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: ceae71766c3e51a84cd09e8ac88740353e783bea
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67482526"
---
# <a name="microsoft-identity-platform-code-samples-v20-endpoint"></a>Microsoft identity 플랫폼 코드 샘플 ((v2.0 끝점)

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Microsoft ID 플랫폼을 사용하여 다음을 수행할 수 있습니다.

- 웹 애플리케이션 및 web API에 인증 및 권한 부여 추가.
- 보호되는 web API에 액세스하려면 액세스 토큰 요구.

이 문서는 간략하게 설명 하 고 Microsoft id 플랫폼 끝점에 대 한 샘플의 링크를 제공 합니다. 이러한 샘플에는 작업이 완료 되 고 응용 프로그램에서 사용할 수 있는 코드 조각을 제공 하는 방법을 보여 줍니다. 코드 샘플 페이지에서 요구 사항, 설치 및 설치에 도움이 되는 자세한 추가 정보 항목을 찾을 수 있습니다. 주석을 코드 내에서 중요 섹션을 이해할 수 있도록 합니다.

> [!NOTE]
> V1.0 샘플에 관심이 있는 경우 참조 [Azure AD 코드 샘플 ((v1.0 끝점)](sample-v1-code.md)합니다.

각 샘플 유형의 기본 시나리오를 이해 하려면 [Microsoft id 플랫폼 끝점에 대 한 앱 유형에](v2-app-types.md)합니다.

GitHub의 샘플에 참여할 수도 있습니다. 자세한 방법은 [Microsoft Azure Active Directory 샘플 및 설명서](https://github.com/Azure-Samples?page=3&query=active-directory)를 참조하세요.

## <a name="single-page-applications"></a>단일 페이지 응용 프로그램

이러한 샘플에는 Microsoft id 플랫폼을 사용 하 여 보호 되는 단일 페이지 응용 프로그램을 작성 하는 방법을 보여 줍니다. 이러한 샘플 MSAL.js의 버전 중 하나를 사용 합니다.

| 플랫폼 | 설명 | 연결 |
| -------- | --------------------- | -------- |
| ![이 이미지는 JavaScript 로고](media/sample-v2-code/logo_js.png) [JavaScript (msal.js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) | Microsoft Graph 호출 |[javascript-graphapi-web-v2](https://github.com/Azure-Samples/active-directory-javascript-graphapi-web-v2) |
| ![이 이미지는 JavaScript 로고](media/sample-v2-code/logo_js.png) [JavaScript (msal.js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) | B2C를 호출합니다. |[b2c-javascript-msal-singlepageapp](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp) |
| ![이 이미지는 JavaScript 로고](media/sample-v2-code/logo_js.png) [JavaScript (msal.js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) | 호출 자체 웹 API |[javascript-singlepageapp-dotnet-webapi-v2](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2) |
| ![이 이미지는 Angular JS 로고](media/sample-v2-code/logo_angular.png) [JavaScript (MSAL AngularJS)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angularjs)| Microsoft Graph 호출  | [MsalAngularjsDemoApp](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-angularjs/samples/MsalAngularjsDemoApp)
| ![이 이미지는 Angular 로고](media/sample-v2-code/logo_angular.png) [JavaScript (MSAL Angular)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular)| Microsoft Graph 호출  | [MSALAngularDemoApp](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-angular/samples/MSALAngularDemoApp) |

## <a name="web-applications"></a>웹 애플리케이션

다음 샘플은 사용자를 로그인하는 웹 애플리케이션을 보여 줍니다. 일부 샘플은 사용자 ID를 사용하여 Microsoft Graph 또는 사용자 고유의 웹 API를 호출하는 애플리케이션도 보여 줍니다.

| 플랫폼 | 사용자만 로그인 | 사용자를 로그인하고 Microsoft Graph를 호출 |
| -------- | ------------------- | --------------------------------- |
| ![이 이미지는 ASP.NET Core 로고를 보여 줍니다.](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Core 2.2 | [ASP.NET Core 웹 앱 로그인 사용자 자습서](https://aka.ms/aspnetcore-webapp-sign-in) | 동일한 샘플에 [Microsoft Graph를 호출 하는 ASP.NET Core 웹 앱](https://aka.ms/aspnetcore-webapp-call-msgraph) 단계 |
| ![이 이미지는 ASP.NET 로고를 보여 줍니다.](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET | [ASP.NET 빠른 시작](https://github.com/AzureAdQuickstarts/AppModelv2-WebApp-OpenIDConnect-DotNet) </p> [dotnet-webapp-openidconnect-v2](https://github.com/azure-samples/active-directory-dotnet-webapp-openidconnect-v2)  |  [dotnet-admin-restricted-scopes-v2](https://github.com/azure-samples/active-directory-dotnet-admin-restricted-scopes-v2) </p> |[msgraph-training-aspnetmvcapp](https://github.com/microsoftgraph/msgraph-training-aspnetmvcapp)
| ![이 이미지 Node.js 로고를 보여 줍니다.](media/sample-v2-code/logo_nodejs.png)  |                   | [Node.js 빠른 시작](https://github.com/azureadquickstarts/appmodelv2-webapp-openidconnect-nodejs) |
| ![이 이미지는 Ruby 로고를 보여 줍니다.](media/sample-v2-code/logo_ruby.png) |                   | [msgraph-training-rubyrailsapp](https://github.com/microsoftgraph/msgraph-training-rubyrailsapp) |

## <a name="desktop-and-mobile-public-client-apps"></a>데스크톱 및 모바일 공용 클라이언트 앱

다음 샘플 응용 프로그램 (데스크톱 또는 모바일 응용 프로그램)는 Microsoft Graph API 또는 사용자 이름을 사용자 고유의 웹 API에 액세스 하는 공용 클라이언트를 표시 합니다. 이러한 모든 클라이언트 응용 프로그램을 사용 하 여 Microsoft 인증 라이브러리 (MSAL).

| 클라이언트 애플리케이션 | 플랫폼 | 흐름/권한 부여 | Microsoft Graph 호출 | ASP.NET Core 2.0 웹 API를 호출합니다. |
| ------------------ | -------- |  ----------| ---------- | ------------------------- |
| 데스크톱(WPF)      | ![이 이미지는.NET /C# 로고](media/sample-v2-code/logo_NET.png) | [대화형](msal-authentication-flows.md#interactive)| [dotnet-desktop-msgraph-v2](https://github.com/azure-samples/active-directory-dotnet-desktop-msgraph-v2) | [dotnet-native-aspnetcore-v2](https://aka.ms/msidentity-aspnetcore-webapi) |
| 데스크톱(콘솔)   | ![이 이미지는.NET /C# (데스크톱) 로고](media/sample-v2-code/logo_NET.png) | [Windows 통합 인증](msal-authentication-flows.md#integrated-windows-authentication) | [dotnet-iwa-v2](https://github.com/azure-samples/active-directory-dotnet-iwa-v2) |  |
| 데스크톱(콘솔)   | ![이 이미지는.NET /C# (데스크톱) 로고](media/sample-v2-code/logo_NETcore.png) | [사용자 이름/암호](msal-authentication-flows.md#usernamepassword) |[dotnetcore-up-v2](https://github.com/azure-samples/active-directory-dotnetcore-console-up-v2) |  |
| 모바일(Android, iOS, UWP)   | ![이 이미지는.NET /C# (Xamarin) 로고](media/sample-v2-code/logo_xamarin.png) | [대화형](msal-authentication-flows.md#interactive) |[xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) |  |
| 모바일(iOS)       | ![이 이미지는 iOS/Objective-c 또는 Swift를 보여 줍니다.](media/sample-v2-code/logo_iOS.png) | [대화형](msal-authentication-flows.md#interactive) |[ios-swift-native-v2](https://github.com/azure-samples/active-directory-ios-swift-native-v2) </p> [ios-native-nxoauth2-v2](https://github.com/azure-samples/active-directory-ios-native-nxoauth2-v2) |  |
| 모바일(Android)   | ![이 이미지는 Android 로고를 보여 줍니다.](media/sample-v2-code/logo_Android.png) | [대화형](msal-authentication-flows.md#interactive) |  [android-native-v2](https://github.com/azure-samples/active-directory-android-native-v2 ) |  |

## <a name="daemon-applications"></a>디먼 애플리케이션

다음 샘플은 고유 ID(사용자 없음)로 Microsoft Graph API에 액세스하는 애플리케이션을 보여 줍니다.

| 클라이언트 애플리케이션 | 플랫폼 | 흐름/권한 부여 | Microsoft Graph 호출 |
| ------------------ | -------- | ---------- | -------------------- |
| 콘솔 | ![이 이미지는.NET Core 로고를 보여 줍니다.](media/sample-v2-code/logo_NETcore.png)</p> ASP.NET  | [클라이언트 자격 증명](msal-authentication-flows.md#client-credentials) | [dotnetcore-daemon-v2](https://github.com/azure-samples/active-directory-dotnetcore-daemon-v2) |
| 웹 앱 | ![이 이미지는 ASP.NET 로고를 보여 줍니다.](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET  | [클라이언트 자격 증명](msal-authentication-flows.md#client-credentials) | [dotnet-daemon-v2](https://github.com/azure-samples/active-directory-dotnet-daemon-v2) |

## <a name="headless-applications"></a>헤드리스 애플리케이션

다음 샘플은 웹 브라우저가 없는 장치에서 실행되는 공용 클라이언트 애플리케이션을 보여 줍니다. 앱에는 명령줄 도구, Linux 또는 Mac 또는 IoT 응용 프로그램에서 실행 되는 앱 수 있습니다. 샘플에는 Microsoft Graph API 로그인 대화형으로 (예: 휴대폰) 다른 장치에서 사용자 이름에 액세스 하는 앱 기능. 이 클라이언트 응용 프로그램이 Microsoft 인증 라이브러리 (MSAL)를 사용 합니다.

| 클라이언트 애플리케이션 | 플랫폼 | 흐름/권한 부여 | Microsoft Graph 호출 |
| ------------------ | -------- |  ----------| ---------- |
| 데스크톱(콘솔)   | ![이 이미지는.NET /C# (데스크톱) 로고](media/sample-v2-code/logo_NETcore.png) | [장치 코드 흐름](msal-authentication-flows.md#device-code) |[dotnetcore-devicecodeflow-v2](https://github.com/azure-samples/active-directory-dotnetcore-devicecodeflow-v2) |

## <a name="web-apis"></a>Web API

다음 샘플에는 Microsoft id 플랫폼 끝점을 사용 하 여 web API를 보호 하는 방법 및 web API에서에서 다운스트림 API를 호출 하는 방법을 보여 줍니다.

| 플랫폼 | 샘플 |
| -------- | ------------------- |
| ![이 이미지는 ASP.NET Core 로고를 보여 줍니다.](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Core 2.2 | ASP.NET Core web API (서비스)의 [dotnet 네이티브-aspnetcore v2](https://aka.ms/msidentity-aspnetcore-webapi-calls-msgraph)  |
| ![이 이미지는 ASP.NET 로고를 보여 줍니다.](media/sample-v2-code/logo_NET.png)</p>ASP.NET MVC | 웹 API (서비스)의 [ms-identity-aspnet-webapi-영문](https://github.com/Azure-Samples/ms-identity-aspnet-webapi-onbehalfof) |

## <a name="other-microsoft-graph-samples"></a>다른 Microsoft Graph 샘플

Azure AD 인증을 포함하여 Microsoft Graph API에 대한 여러 사용 패턴을 보여주는 [샘플](https://github.com/microsoftgraph/msgraph-community-samples/tree/master/samples#aspnet) 및 자습서에 대한 내용은 [Microsoft Graph 커뮤니티 샘플 및 자습서](https://github.com/microsoftgraph/msgraph-community-samples)를 참조하세요.

## <a name="see-also"></a>참고 항목

- [Azure Active Directory (v1.0) 개발자 가이드](v1-overview.md)
- [Azure AD Graph API 개념 및 참조](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Azure AD Graph API 도우미 라이브러리](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
