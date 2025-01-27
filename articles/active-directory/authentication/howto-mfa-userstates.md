---
title: Azure Multi-factor Authentication 사용자 상태-Azure Active Directory
description: Azure Multi-Factor Authentication에서 사용자 상태를 알아봅니다.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 01/11/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 54146927bf344eed63e24a3df073aa13f7fa0676
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68319924"
---
# <a name="how-to-require-two-step-verification-for-a-user"></a>사용자에 대해 2단계 인증을 요구하는 방법

2단계 인증이 필요한 두 가지 방법 중 하나를 수행할 수 있습니다. 두 방법 모두 글로벌 관리자 계정을 사용해야 합니다. 첫 번째 옵션은 각 사용자가 Azure MFA(Multi-Factor Authentication)를 사용하도록 설정하는 것입니다. 사용자가 개별적으로 설정되면 신뢰할 수 있는 IP 주소에서 로그인하거나 _기억된 디바이스_ 기능이 설정된 경우와 같이 몇 가지 예외를 제외하고는, 로그인할 때마다 2단계 인증을 수행합니다. 두 번째 옵션은 특정 조건에서 2 단계 인증을 요구 하는 조건부 액세스 정책을 설정 하는 것입니다.

> [!TIP]
> 조건부 액세스 정책을 사용 하 여 Azure Multi-factor Authentication을 사용 하도록 설정 하는 것이 좋습니다. 사용자가 로그인 할 때마다 MFA를 수행 해야 하므로 라이선스에 조건부 액세스가 포함 되지 않은 경우 사용자 상태를 변경 하는 것은 더 이상 권장 되지 않습니다.

## <a name="choose-how-to-enable"></a>사용하도록 설정하는 방법 선택

**사용자 상태를 변경하여 사용하도록 설정** - 2단계 인증을 요구하기 위한 일반적인 방법으로, 이 문서에 설명되어 있습니다. 이 방법은 클라우드의 Azure MFA와 Azure MFA Server 둘 다에서 작동합니다. 이 방법을 사용 하려면 사용자가 로그인 할 **때마다** 2 단계 인증을 수행 하 고 조건부 액세스 정책을 재정의 해야 합니다.

조건부 액세스 정책에 따라 사용-사용자에 대해 2 단계 인증을 사용 하도록 설정 하는 가장 유연한 방법입니다. 조건부 액세스 정책을 사용 하도록 설정 하는 것은 클라우드의 Azure MFA에만 적용 되며 Azure AD의 프리미엄 기능입니다. 이 방법에 대한 자세한 내용은 [클라우드 기반 Azure Multi-factor Authentication 배포](howto-mfa-getstarted.md)에서 확인할 수 있습니다.

Azure AD ID 보호에 따라 사용하도록 설정 - 이 방법은 Azure AD ID 보호 위험 정책을 사용하여 모든 클라우드 애플리케이션에 대해 로그인 위험이 있을 때만 2단계 인증을 요구합니다. 이 방법에는 Azure Active Directory P2 라이선스가 필요합니다. 이 방법에 대한 자세한 내용은 [Azure Active Directory ID 보호](../identity-protection/howto-sign-in-risk-policy.md)에서 확인할 수 있습니다.

> [!Note]
> 라이선스 및 가격 책정에 대한 자세한 내용은 [Azure AD](https://azure.microsoft.com/pricing/details/active-directory/
) 및 [Multi-Factor Authentication](https://azure.microsoft.com/pricing/details/multi-factor-authentication/) 가격 책정 페이지에서 확인할 수 있습니다.

## <a name="enable-azure-mfa-by-changing-user-state"></a>사용자 상태를 변경하여 Azure MFA를 사용하도록 설정

Azure Multi-Factor Authentication의 사용자 계정은 다음과 같은 3가지 상태를 갖습니다.

| Status | 설명 | 영향 받는 비브라우저 앱 | 영향 받는 브라우저 앱 | 영향 받는 최신 인증 |
|:---:|:---:|:---:|:--:|:--:|
| 사용 안 함 |Azure MFA에 등록되지 않은 새 사용자에 대한 기본 상태입니다. |아니요 |아니요 |아니요 |
| Enabled |사용자가 Azure MFA에 등록되었지만 등록하지 않았습니다. 다음에 로그인할 때 등록하라는 메시지가 표시됩니다. |아니요.  등록 프로세스가 완료될 때까지 계속 작업합니다. | 예. 세션이 만료되면 Azure MFA 등록이 필요합니다.| 예. 액세스 토큰이 만료되면 Azure MFA 등록이 필요합니다. |
| 적용 |사용자가 등록되었으며 Azure MFA를 위한 등록 프로세스를 완료했습니다. |예. 앱에 앱 암호가 필요합니다. |예. 로그인할 때 Azure MFA가 필요합니다. | 예. 로그인할 때 Azure MFA가 필요합니다. |

사용자의 상태는 관리자가 사용자를 Azure MFA에 등록했는지 그리고 사용자가 등록 프로세스를 완료했는지 여부를 반영합니다.

모든 사용자는 *사용 안 함*으로 시작합니다. Azure MFA에 사용자를 등록하면 상태는 *사용*으로 변경됩니다. 사용된 사용자가 로그인하고 등록 프로세스를 완료하면 상태는 변경 *적용*으로 변경됩니다.  

### <a name="view-the-status-for-a-user"></a>사용자 상태 보기

다음 단계를 통해 사용자 상태를 보고 관리할 수 있는 페이지에 액세스합니다.

1. 관리자로 [Azure Portal](https://portal.azure.com)에 로그인합니다.
2. **Azure Active Directory** > **사용자 및 그룹** > **모든 사용자**로 차례로 이동합니다.
3. **Multi-Factor Authentication**을 선택합니다.
   ![Multi-Factor Authentication 선택](./media/howto-mfa-userstates/selectmfa.png)
4. 사용자 상태를 표시하는 새 페이지가 열립니다.
   ![다단계 인증 사용자 상태 - 스크린샷](./media/howto-mfa-userstates/userstate1.png)

### <a name="change-the-status-for-a-user"></a>사용자 상태 변경

1. 이전 단계를 통해 Azure Multi-Factor Authentication **사용자** 페이지로 이동합니다.
2. Azure MFA을 사용하도록 설정할 사용자를 찾습니다. 위쪽에서 보기를 변경해야 할 수도 있습니다.
   ![사용자 탭에서 상태를 변경할 사용자를 선택 합니다.](./media/howto-mfa-userstates/enable1.png)
3. 이름 옆의 확인란을 선택합니다.
4. 오른쪽의 **빠른 단계**에서 **사용** 또는 **사용 안 함**을 선택합니다.
   ![빠른 단계 메뉴에서 사용을 클릭 하 여 선택한 사용자를 사용 하도록 설정 합니다.](./media/howto-mfa-userstates/user1.png)

   > [!TIP]
   > Azure MFA에 등록하면 *사용* 사용자는 *적용*으로 자동으로 전환됩니다. 사용자 상태를 수동으로 *적용*으로 변경하지 마세요.

5. 열리는 팝업 창에서 선택한 내용을 확인합니다.

사용자를 사용으로 설정한 후 전자 메일을 통해 알립니다. 다음에 로그인할 때 등록하도록 요구된다고 알려줍니다. 또한 조직에서 최신 인증을 지원하지 않는 비브라우저 앱을 사용하는 경우 앱 암호를 만들어야 합니다. 또한 사용자가 시작할 수 있도록 [Azure MFA 최종 사용자 가이드](../user-help/multi-factor-authentication-end-user.md)에 연결되는 링크를 포함할 수도 있습니다.

### <a name="use-powershell"></a>PowerShell 사용

[Azure AD PowerShell](/powershell/azure/overview)을 사용하여 사용자 상태를 변경하려면 `$st.State`를 변경합니다. 여기에는 세 가지 상태가 있습니다.

* Enabled
* 적용
* 사용 안 함  

사용자를 *적용* 상태로 직접 전환하지 마세요. 이렇게 전환하면 사용자가 Azure MFA 등록을 마치고 [앱 암호](howto-mfa-mfasettings.md#app-passwords)를 가져오지 못했기 때문에 비 브라우저 기반 앱 작동이 중단됩니다.

다음을 사용하여 먼저 모듈을 설치합니다.

   ```PowerShell
   Install-Module MSOnline
   ```

> [!TIP]
> 먼저 **Connect-MsolService**를 사용하여 연결해야 합니다.

이 예제 PowerShell 스크립트는 개별 사용자에 대해 MFA를 사용하도록 설정합니다.

   ```PowerShell
   Import-Module MSOnline
   $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
   $st.RelyingParty = "*"
   $st.State = "Enabled"
   $sta = @($st)
   Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta
   ```

PowerShell은 사용자를 대량 설정해야 할 때 적합한 옵션입니다. 예를 들어 다음 스크립트는 사용자 목록을 반복하고 해당 계정에서 MFA를 사용하도록 설정합니다.

   ```PowerShell
   $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
   foreach ($user in $users)
   {
       $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
       $st.RelyingParty = "*"
       $st.State = "Enabled"
       $sta = @($st)
       Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
   }
   ```

MFA를 사용하지 않으려면 다음 스크립트를 사용합니다.

   ```PowerShell
   Get-MsolUser -UserPrincipalName user@domain.com | Set-MsolUser -StrongAuthenticationMethods @()
   ```

다음과 같이 축약할 수도 있습니다.

   ```PowerShell
   Set-MsolUser -UserPrincipalName user@domain.com -StrongAuthenticationRequirements @()
   ```

### <a name="convert-users-from-per-user-mfa-to-conditional-access-based-mfa"></a>사용자 단위 MFA의 사용자를 조건부 액세스 기반 MFA로 변환

다음 PowerShell은 Azure Multi-factor Authentication을 기반으로 조건부 액세스를 변환 하는 데 도움이 될 수 있습니다.

```PowerShell
# Disable MFA for all users, keeping their MFA methods intact
Get-MsolUser -All | Disable-MFA -KeepMethods

# Enforce MFA for all users
Get-MsolUser -All | Set-MfaState -State Enforced

# Wrapper to disable MFA with the option to keep the MFA
# methods (to avoid having to proof-up again later)
function Disable-Mfa {

    [CmdletBinding()]
    param(
        [Parameter(ValueFromPipeline=$True)]
        $User,
        [switch] $KeepMethods
    )

    Process {

        Write-Verbose ("Disabling MFA for user '{0}'" -f $User.UserPrincipalName)
        $User | Set-MfaState -State Disabled

        if ($KeepMethods) {
            # Restore the MFA methods which got cleared when disabling MFA
            Set-MsolUser -ObjectId $User.ObjectId `
                         -StrongAuthenticationMethods $User.StrongAuthenticationMethods
        }
    }
}

# Sets the MFA requirement state
function Set-MfaState {

    [CmdletBinding()]
    param(
        [Parameter(ValueFromPipelineByPropertyName=$True)]
        $ObjectId,
        [Parameter(ValueFromPipelineByPropertyName=$True)]
        $UserPrincipalName,
        [ValidateSet("Disabled","Enabled","Enforced")]
        $State
    )

    Process {
        Write-Verbose ("Setting MFA state for user '{0}' to '{1}'." -f $ObjectId, $State)
        $Requirements = @()
        if ($State -ne "Disabled") {
            $Requirement =
                [Microsoft.Online.Administration.StrongAuthenticationRequirement]::new()
            $Requirement.RelyingParty = "*"
            $Requirement.State = $State
            $Requirements += $Requirement
        }

        Set-MsolUser -ObjectId $ObjectId -UserPrincipalName $UserPrincipalName `
                     -StrongAuthenticationRequirements $Requirements
    }
}

```

## <a name="next-steps"></a>다음 단계

* 사용자에게 MFA를 수행하라는 메시지가 표시되거나 표시되지 않는 이유는? [Azure Multi-Factor Authentication 문서에서 보고서의 Azure AD 로그인 보고서](howto-mfa-reporting.md#azure-ad-sign-ins-report) 섹션을 참조하세요.
* 신뢰할 수 있는 IP, 사용자 지정 음성 메시지 및 사기 행위 경고와 같은 추가 설정을 구성하려면 [Azure Multi-Factor Authentication 설정 구성](howto-mfa-mfasettings.md) 문서를 참조하세요.
* Azure Multi-Factor Authentication에 대한 사용자 설정 관리에 대한 내용은 [클라우드에서 Azure Multi-Factor Authentication을 사용하여 사용자 설정 관리](howto-mfa-userdevicesettings.md) 문서에서 확인할 수 있습니다.
