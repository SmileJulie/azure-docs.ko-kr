---
title: Azure Active Directory에서 엔터프라이즈 앱에 사용자 또는 그룹 할당 | Microsoft Docs
description: Azure Active Directory에서 사용자 또는 그룹을 할당할 엔터프라이즈 앱을 선택하는 방법
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: mimart
ms.reviewer: luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: 249dfeeb231c61b05af2e89f0dc02822cc18e627
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67702180"
---
# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory"></a>Azure Active Directory에서 엔터프라이즈 앱에 사용자 또는 그룹 할당

엔터프라이즈 앱에 사용자 또는 그룹을 할당하려면 엔터프라이즈 앱을 관리하기 위한 적절한 권한이 있어야 하고 해당 디렉터리에 대한 전역 관리자여야 합니다. Microsoft 애플리케이션(예: Office 365 앱)의 경우 PowerShell을 사용하여 엔터프라이즈 앱에 사용자를 할당합니다.

> [!NOTE]
> 이 문서에서 설명하는 기능에 대한 라이선스 요구 사항은 [Azure Active Directory 가격 책정 페이지](https://azure.microsoft.com/pricing/details/active-directory)를 참조하세요.

## <a name="assign-a-user-to-an-app---portal"></a>앱에 사용자 할당 - 포털

1. 디렉터리에 대한 전역 관리자인 계정으로 [Azure Portal](https://portal.azure.com)에 로그인합니다.
1. **모든 서비스**를 선택하고 텍스트 상자에 Azure Active Directory를 입력한 다음, **입력**을 선택합니다.
1. **Enterprise 애플리케이션**을 선택합니다.
1. 에 **엔터프라이즈 응용 프로그램-모든 응용 프로그램** 창 관리할 수 있는 앱 목록을 표시 합니다. 앱을 선택 합니다.
1. 에 ***appname*** 창 (즉, 창 제목에서 선택된 된 앱의 이름)을 선택 **사용자 및 그룹**합니다.
1. 에 ***appname*** **-사용자 및 그룹** 창 **추가 사용자**합니다.
1. 에 **할당 추가** 창 **사용자 및 그룹**합니다.

   ![앱에 사용자 또는 그룹 할당](./media/assign-user-or-group-access-portal/assign-users.png)

1. 에 **사용자 및 그룹** 창 목록에서 하나 이상의 사용자 또는 그룹을 선택 하 고 선택한 합니다 **선택** 창의 맨 아래에 있는 단추입니다.
1. 에 **할당 추가** 창 **역할**입니다. 그런 다음 합니다 **역할 선택** 선택 창에서 선택한 사용자 또는 그룹에 적용할 역할을 선택 **확인** 창의 맨 아래에 있습니다.
1. 에 **할당 추가** 창 합니다 **할당** 창의 맨 아래에 있는 단추입니다. 할당된 사용자 또는 그룹은 이 엔터프라이즈 앱에 대한 선택된 역할로 정의된 사용 권한을 갖습니다.

## <a name="allow-all-users-to-access-an-app---portal"></a>모든 사용자가 앱에 액세스하도록 허용 - 포털

1. 디렉터리에 대한 전역 관리자인 계정으로 [Azure Portal](https://portal.azure.com)에 로그인합니다.
1. **모든 서비스**를 선택하고 텍스트 상자에 Azure Active Directory를 입력한 다음, **입력**을 선택합니다.
1. **Enterprise 애플리케이션**을 선택합니다.
1. **엔터프라이즈 애플리케이션** 창에서 **모든 애플리케이션**을 선택합니다. 그러면 관리할 수 있는 앱이 표시됩니다.
1. **엔터프라이즈 애플리케이션 - 모든 애플리케이션** 창에서 앱을 선택합니다.
1. 에 ***appname*** 창 **속성**합니다.
1. 에  ***appname* -속성** 창 설정 합니다 **사용자 할당 필요?** 로 설정 **아니요**합니다.

**사용자 할당 필요?** 옵션:

- 응용 프로그램 액세스 패널에 응용 프로그램 표시 여부를 바꾸지는지 않습니다. 애플리케이션을 액세스 패널에 표시하려면 애플리케이션에 적절한 사용자 또는 그룹을 할당해야 합니다.
- SAML Single Sign-On에 대해 구성된 클라우드 애플리케이션 및 애플리케이션 프록시로 구성된 온-프레미스 애플리케이션으로만 작동합니다. [애플리케이션에 대한 Single Sign-On](what-is-single-sign-on.md)을 참조하세요.
- 사용자는 애플리케이션에 동의해야 합니다. 관리자는 모든 사용자에 대한 동의를 부여할 수 있습니다.  [최종 사용자가 애플리케이션에 동의하는 방법 구성](configure-user-consent.md)을 참조하세요.

## <a name="assign-a-user-to-an-app---powershell"></a>앱에 사용자 할당 - PowerShell

1. 관리자 권한 Windows PowerShell 명령 프롬프트를 엽니다.

   > [!NOTE]
   > Azure AD 모듈을 설치해야 합니다(`Install-Module -Name AzureAD` 명령 사용). NuGet 모듈 또는 새로운 Azure Active Directory V2 PowerShell 모듈을 설치하라는 메시지가 표시되면 Y를 입력하고 Enter 키를 누릅니다.

1. `Connect-AzureAD`를 실행하고 전역 관리자 사용자 계정으로 로그인합니다.
1. 다음 스크립트를 사용하여 애플리케이션에 사용자 및 역할을 할당합니다.

    ```powershell
    # Assign the values to the variables
    $username = "<You user's UPN>"
    $app_name = "<Your App's display name>"
    $app_role_name = "<App role display name>"

    # Get the user to assign, and the service principal for the app to assign to
    $user = Get-AzureADUser -ObjectId "$username"
    $sp = Get-AzureADServicePrincipal -Filter "displayName eq '$app_name'"
    $appRole = $sp.AppRoles | Where-Object { $_.DisplayName -eq $app_role_name }

    # Assign the user to the app role
    New-AzureADUserAppRoleAssignment -ObjectId $user.ObjectId -PrincipalId $user.ObjectId -ResourceId $sp.ObjectId -Id $appRole.Id
    ```

애플리케이션 역할에 사용자를 할당하는 방법에 대한 자세한 내용은 [New-AzureADUserAppRoleAssignment](https://docs.microsoft.com/powershell/module/azuread/new-azureaduserapproleassignment?view=azureadps-2.0)에 대한 설명서를 참조하세요.

그룹을 엔터프라이즈 앱에 할당하려면 `Get-AzureADUser`를 `Get-AzureADGroup`으로 바꿔야 합니다.

### <a name="example"></a>예제

이 예제에서는 PowerShell을 사용하여 사용자 Britta Simon을 [Microsoft Workplace Analytics](https://products.office.com/business/workplace-analytics) 애플리케이션에 할당합니다.

1. PowerShell에서 변수 $username, app_name $ 및 $app_role_name에 해당 값을 할당합니다.

    ```powershell
    # Assign the values to the variables
    $username = "britta.simon@contoso.com"
    $app_name = "Workplace Analytics"
    ```

1. 이 예제에서는 Britta Simon에 할당하려는 애플리케이션 역할의 정확한 이름을 모릅니다. 다음 명령을 실행하여 사용자 UPN 및 서비스 주체 표시 이름으로 사용자($user) 및 서비스 주체($sp)를 가져옵니다.

    ```powershell
    # Get the user to assign, and the service principal for the app to assign to
    $user = Get-AzureADUser -ObjectId "$username"
    $sp = Get-AzureADServicePrincipal -Filter "displayName eq '$app_name'"
    ```

1. 명령 `$sp.AppRoles`를 실행하여 Workplace Analytics 애플리케이션에 대해 사용할 수 있는 역할을 표시합니다. 이 예제에서는 Britta Simon을 Analyst(제한된 액세스) 역할에 할당하려고 합니다.

   ![Workplace Analytics 역할을 사용 하 여 사용자에 게 사용할 수 있는 역할을 보여 줍니다.](./media/assign-user-or-group-access-portal/workplace-analytics-role.png)

1. 역할 이름을 `$app_role_name` 변수에 할당합니다.

    ```powershell
    # Assign the values to the variables
    $app_role_name = "Analyst (Limited access)"
    $appRole = $sp.AppRoles | Where-Object { $_.DisplayName -eq $app_role_name }
    ```

1. 다음 명령을 실행하여 앱 역할에 사용자를 할당합니다.

    ```powershell
    # Assign the user to the app role
    New-AzureADUserAppRoleAssignment -ObjectId $user.ObjectId -PrincipalId $user.ObjectId -ResourceId $sp.ObjectId -Id $appRole.Id
    ```

## <a name="next-steps"></a>다음 단계

- [내 그룹 모두 보기](../fundamentals/active-directory-groups-view-azure-portal.md)
- [엔터프라이즈 앱에서 사용자 또는 그룹 할당 제거](remove-user-or-group-access-portal.md)
- [엔터프라이즈 앱에 대한 사용자 로그인 비활성화](disable-user-sign-in-portal.md)
- [엔터프라이즈 앱의 이름 또는 로고 변경](change-name-or-logo-portal.md)
