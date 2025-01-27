---
title: 보호 된 웹 API-앱 등록 | Azure
description: 보호 된 web API 및 앱을 등록 하는 데 필요한 정보를 빌드하는 방법에 알아봅니다.
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
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e4622cffedc159ce85166eafe571ccb26c2c1b4d
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67536856"
---
# <a name="protected-web-api-app-registration"></a>보호 된 web API: 앱 등록

이 문서는 보호 된 web API에 대 한 앱 등록의 세부 정보를 설명합니다.

[빠른 시작: Microsoft id 플랫폼을 사용 하 여 응용 프로그램을 등록](quickstart-register-app.md) 앱을 등록 하기 위한 일반적인 단계입니다.

## <a name="accepted-token-version"></a>허용 되는 토큰 버전

Microsoft id 플랫폼 끝점 두 가지 유형의 토큰을 발급할 수 있습니다: 토큰 v1.0 및 v2.0 토큰입니다. 이러한 토큰에 대 한 자세한 내용은 참조 하세요. [액세스 토큰을](access-tokens.md)입니다. 허용 되는 토큰 버전에 따라 달라 집니다 합니다 **지원 되는 계정 유형** 응용 프로그램을 만들 때 선택한:

- 경우 값 **지원 되는 계정 유형** 됩니다 **모든 조직 디렉터리에 개인 Microsoft 계정 (예: Skype, Xbox, Outlook.com) 계정을**, 허용 되는 토큰 버전 v2.0 됩니다.
- 그렇지 않은 경우 허용 되는 토큰 버전 v1.0 됩니다.

응용 프로그램을 만든 후에 확인 하거나 다음 단계를 수행 하 여 허용 된 토큰 버전을 변경할 수 있습니다.

1. Azure portal에서 앱을 선택 하 고 다음을 선택 합니다 **매니페스트** 앱에 대 한 합니다.
2. 검색할 매니페스트에서 **"accessTokenAcceptedVersion"** 합니다. 해당 값은 **2**합니다. 이 속성은 Azure Active Directory (Azure AD)로 지정 web API는 v2.0 토큰을 수락 합니다. 값이 **null**, 허용 되는 토큰 버전은 v1.0 합니다.
3. 토큰 버전을 변경 하면 선택할 **저장할**합니다.

> [!NOTE]
> Web API 허용 (v1.0 또는 v2.0) 토큰 버전을 지정 합니다. Microsoft id 플랫폼 v2.0 끝점에서 웹 API에 대 한 토큰을 요청 하는 클라이언트, 웹 API에 의해 허용 되는 버전을 나타내는 토큰을 얻을 수 있습니다.

## <a name="no-redirect-uri"></a>없는 리디렉션 URI

Web Api는 사용자가 대화형으로 로그인 하기 때문에 리디렉션 URI를 등록 하지 않아도 됩니다.

## <a name="expose-an-api"></a>API를 노출 합니다.

다른 설정은 웹 Api에 특정에 노출 된 API 및 노출 된 범위입니다.

### <a name="resource-uri-and-scopes"></a>리소스 URI 및 범위

범위는 일반적으로 폼 `resourceURI/scopeName`합니다. Microsoft Graph에 대 한 범위 바로 가기가 같은 `User.Read`합니다. 이 문자열에 대 한 바로 가기입니다 `https://graph.microsoft.com/user.read`합니다.

앱 등록 하는 동안 이러한 매개 변수를 정의 해야 합니다.

- 리소스 URI 응용 프로그램 등록 포털을 권장 하는 기본적으로 사용 하 여 `api://{clientId}`입니다. 이 리소스 URI는 고유 하지만 사람이 아닙니다 읽을 수 있습니다. 변경할 수 있지만 새 값이 고유 해야 합니다.
- 하나 이상의 *범위*합니다. (클라이언트 응용 프로그램에 표시 됩니다로 *위임 된 권한* web API에 대 한 합니다.)
- 하나 이상의 *앱 역할*입니다. (클라이언트 응용 프로그램에 표시 됩니다로 *응용 프로그램 사용 권한* web API에 대 한 합니다.)

범위를 앱의 최종 사용자에 게 표시 되는 동의 화면에 표시 됩니다. 따라서 범위를 설명 하는 해당 문자열을 제공 해야 합니다.

- 최종 사용자가 볼 수 있듯이 합니다.
- 관리자 동의 부여할 수 있는 테 넌 트 관리자가 볼 수 있듯이 합니다.

### <a name="exposing-delegated-permissions-scopes"></a>위임 된 권한 (범위)를 노출합니다.

1. 선택 된 **API를 노출** 응용 프로그램 등록의 섹션입니다.
1. **범위 추가**를 선택합니다.
1. 메시지가 표시 되 면 수락 제안 된 응용 프로그램 ID URI (`api://{clientId}`)을 선택 하 여 **저장 하 고 계속**합니다.
1. 이러한 매개 변수를 입력 합니다.
      - 에 대 한 **범위 이름**를 사용 하 여 **access_as_user**합니다.
      - 에 대 한 **동의할 수 있는 사람**, 했는지 **관리자 및 사용자** 을 선택 합니다.
      - **관리자 동의 표시 이름**를 입력 **사용자로 액세스 TodoListService**합니다.
      - **관리자 동의 설명**를 입력 **사용자로 TodoListService Web API에 액세스**합니다.
      - **사용자 동의 표시 이름**를 입력 **사용자로 액세스 TodoListService**합니다.
      - **사용자 동의 설명**를 입력 **사용자로 TodoListService Web API에 액세스**합니다.
      - 유지할 **상태** 로 설정 **Enabled**합니다.
      - 선택 **범위 추가**합니다.

### <a name="if-your-web-api-is-called-by-a-daemon-app"></a>디먼 앱에서 web API를 호출

이 섹션에서는 디먼 앱에서 안전 하 게 호출할 수 있도록 보호 된 web API를 등록 하는 방법에 알아봅니다.

- 노출 해야 *응용 프로그램 사용 권한*합니다. 위임 된 권한 이치에 맞지 않습니다 있도록 디먼 앱 사용자와 상호 작용 하지 때문에 응용 프로그램 권한만 선언할 수 있습니다.
- 테 넌 트 관리자는 web API의 응용 프로그램 사용 권한 중 하나에 액세스 하도록 등록 된 응용 프로그램에만 웹 API에 대 한 토큰을 발급 하려면 Azure Active Directory (Azure AD)를 요구할 수 있습니다.

#### <a name="exposing-application-permissions-app-roles"></a>응용 프로그램 (앱 역할)는 사용 권한을 노출

응용 프로그램 사용 권한, 노출 하는 매니페스트를 편집 해야 합니다.

1. 응용 프로그램에 대 한 응용 프로그램 등록을 선택 **매니페스트**합니다.
1. 배치 하 여 매니페스트를 편집 합니다 `appRoles` 설정 하 고 하나 이상의 응용 프로그램 역할을 추가 합니다. 역할 정의 다음 샘플 JSON 블록에 제공 됩니다. 유지 된 `allowedMemberTypes` 로 `"Application"` 만 합니다. 있는지 확인 합니다 `id` 고유 GUID를 지정 하 고는 `displayName` 및 `value` 공백이 없는지 합니다.
1. 매니페스트를 저장합니다.

다음 샘플에서는 내용의 `appRoles`합니다. (의 `id` 모든 고유 GUID 일 수 있습니다.)

```JSon
"appRoles": [
    {
    "allowedMemberTypes": [ "Application" ],
    "description": "Accesses the TodoListService-Cert as an application.",
    "displayName": "access_as_application",
    "id": "ccf784a6-fd0c-45f2-9c08-2f9d162a0628",
    "isEnabled": true,
    "lang": null,
    "origin": "Application",
    "value": "access_as_application"
    }
],
```

#### <a name="ensuring-that-azure-ad-issues-tokens-for-your-web-api-to-only-allowed-clients"></a>Azure AD에 웹 API에만 허용 되는 클라이언트에 대 한 토큰을 발급 있는지 확인 합니다.

Web API 앱 역할을 검사합니다. (즉 개발자 서 응용 프로그램 사용 권한을 표시 합니다.) 하지만 Azure AD 테 넌 트 관리자가 API에 액세스 하 여 승인 된 앱에만 웹 API에 대 한 토큰을 발급 하도록 구성할 수도 있습니다. 이 향상 된 보안을 추가 합니다.

1. 앱에서 **개요** 앱 등록 페이지에서 앱의 이름 사용 하 여 링크를 선택 합니다 **로컬 디렉터리에서 응용 프로그램을 관리 되는**합니다. 이 필드에 대 한 제목 잘릴 수 있습니다. 예를 들어, 나타날 수 있습니다 **에서 응용 프로그램을 관리 하는 중...**

   > [!NOTE]
   >
   > 이 링크를 선택 하면 이동 합니다 **Enterprise 응용 프로그램 개요** 테 넌 트를 만든 위치에서 응용 프로그램의 서비스 주체와 연결 된 페이지입니다. 브라우저의 뒤로 단추를 사용 하 여 앱 등록 페이지로 탐색할 수 있습니다.

1. 선택 합니다 **속성** 페이지에 **관리** 엔터프라이즈 응용 프로그램 페이지의 섹션입니다.
1. 특정 클라이언트에만에서 web API에 액세스할 수 있도록 Azure AD를 사용 하도록 하려는 경우 설정 **사용자 할당 필요?** 하 **예**합니다.

   > [!IMPORTANT]
   >
   > 설정 하는 경우 **사용자 할당 필요?** 하 **예**, Azure AD는 웹 API에 대 한 액세스 토큰을 요청할 때 클라이언트 앱 역할 할당을 확인 합니다. 클라이언트는 모든 응용 프로그램 역할에 할당 되지, Azure AD는 오류가 반환 됩니다 `invalid_client: AADSTS501051: Application <application name> is not assigned to a role for the <web API>`합니다.
   >
   > 유지 하는 경우 **사용자 할당 필요?** 로 설정 **아니오**를 *클라이언트가 웹 API에 대 한 액세스 토큰을 요청 하는 경우 Azure AD 앱 역할 할당을 확인 하지 않습니다*합니다. 모든 디먼 클라이언트 (즉, 클라이언트 자격 증명 흐름을 사용 하 여 모든 클라이언트) 대상을 지정 하 여 API에 대 한 액세스 토큰을 가져올 수 됩니다. 모든 응용 프로그램에 대 한 권한을 요청 하지 않고도 API에 액세스할 수 없게 됩니다. 하지만 웹 API 수 항상 이전 섹션에 설명 된 대로 있는지 확인 합니다. 응용 프로그램 (수 있는 테 넌 트 관리자에 의해 권한이) 올바른 역할입니다. 액세스 토큰에는 역할이 있는지 유효성을 검사 하 여이 확인을 수행 하는 API를 클레임 하 고이 클레임에 대 한 값이 올바른 합니다. (이 경우 값은 `access_as_application`.)

1. **저장**을 선택합니다.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [앱의 코드 구성](scenario-protected-web-api-app-configuration.md)
