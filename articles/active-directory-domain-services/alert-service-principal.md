---
title: 'Azure Active Directory Domain Services: 서비스 주체 구성 문제 해결 | Microsoft Docs'
description: Azure AD Domain Services에 대한 서비스 주체 구성 문제 해결
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: ''
editor: ''
ms.assetid: f168870c-b43a-4dd6-a13f-5cfadc5edf2c
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/14/2019
ms.author: iainfou
ms.openlocfilehash: 5b94fca6087da61fe10d6c80b3fe7cfb5798f2d3
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67473907"
---
# <a name="troubleshoot-invalid-service-principal-configuration-for-your-managed-domain"></a>관리되는 도메인에 대한 잘못된 서비스 주체 구성 문제 해결

이 문서를 통해 다음과 같은 경고 메시지가 발생하는 서비스 주체 관련 구성 오류를 해결하고 문제를 해결합니다.

## <a name="alert-aadds102-service-principal-not-found"></a>경고 AADDS102: 서비스 주체를 찾을 수 없음

**경고 메시지:** *Azure AD Domain Services가 제대로 작동하는 데 필요한 서비스 주체가 Azure AD 디렉터리에서 삭제되었습니다. 이 구성은 관리되는 도메인을 모니터링, 관리, 패치, 동기화하는 Microsoft의 기능에 영향을 줍니다.*

[서비스 주체](../active-directory/develop/app-objects-and-service-principals.md)는 Microsoft에서 관리되는 도메인을 관리, 업데이트 및 유지 관리하는 데 사용하는 애플리케이션입니다. 이러한 서비스 주체가 삭제되면 Microsoft에서 도메인을 서비스하지 못하게 됩니다.


## <a name="check-for-missing-service-principals"></a>누락된 서비스 주체에 대한 확인
다음 단계에 따라 다시 만들어야 하는 서비스 주체를 확인합니다.

1. Azure Portal에서 [엔터프라이즈 애플리케이션 - 모든 애플리케이션](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps) 페이지로 이동합니다.
2. **표시** 드롭다운에서 **모든 애플리케이션**을 선택하고 **적용**을 클릭합니다.
3. 다음 표를 사용하여 검색 상자에 ID를 붙여 넣어 Enter 키를 눌러 각 애플리케이션 ID를 검색합니다. 검색 결과가 비어 있으면 "해결 방법" 열에 나와 있는 단계를 수행하여 서비스 주체를 다시 만들어야 합니다.

| 애플리케이션 UI | 해결 방법 |
| :--- | :--- |
| 2565bd9d-da50-47d4-8b85-4c97f669dc36 | [PowerShell을 사용하여 누락된 서비스 주체 다시 만들기](#recreate-a-missing-service-principal-with-powershell) |
| 443155a6-77f3-45e3-882b-22b3a8d431fb | [Microsoft.AAD 네임스페이스에 다시 등록](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| abba844e-bc0e-44b0-947a-dc74e5d09022  | [Microsoft.AAD 네임스페이스에 다시 등록](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| d87dcbc6-a371-462e-88e3-28ad15ec4e64 | [Microsoft.AAD 네임스페이스에 다시 등록](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |

## <a name="recreate-a-missing-service-principal-with-powershell"></a>PowerShell 사용하여 누락된 서비스 주체 다시 만들기
ID ```2565bd9d-da50-47d4-8b85-4c97f669dc36```가 있는 서비스 주체가 Azure AD 디렉터리에서 누락된 경우 다음 단계를 수행합니다.

**해결 방법:** 이 단계를 완료하려면 Azure AD PowerShell이 있어야 합니다. Azure AD PowerShell을 설치하는 방법에 대한 내용은 [이 문서](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.)를 참조하세요.

이 문제를 해결하려면 PowerShell 창에서 다음 명령을 입력합니다.
1. Azure AD PowerShell 모듈을 설치하고 가져옵니다.

    ```powershell
    Install-Module AzureAD
    Import-Module AzureAD
    ```

2. 다음 PowerShell 명령을 실행하여 Azure AD Domain Services에 필요한 서비스 주체가 디렉터리에서 누락되어 있는지 여부를 확인합니다.

    ```powershell
    Get-AzureAdServicePrincipal -filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
    ```

3. 다음 PowerShell 명령을 입력하여 서비스 주체를 만듭니다.

    ```powershell
    New-AzureAdServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
    ```

4. 누락된 서비스 주체를 만든 후 2시간 동안 기다렸다가 관리되는 도메인의 상태를 확인합니다.


## <a name="re-register-to-the-microsoft-aad-namespace-using-the-azure-portal"></a>Azure Portal을 사용하여 Microsoft AAD 네임스페이스에 다시 등록
ID ```443155a6-77f3-45e3-882b-22b3a8d431fb```, ```abba844e-bc0e-44b0-947a-dc74e5d09022``` 또는 ```d87dcbc6-a371-462e-88e3-28ad15ec4e64```가 있는 서비스 주체가 Azure AD 디렉터리에서 누락된 경우 이러한 단계를 따릅니다.

**해결 방법:** 다음 단계를 사용하여 디렉터리에서 Domain Services를 복원합니다.

1. Azure Portal의 [구독](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) 페이지로 이동합니다.
2. 관리되는 도메인에 연결된 구독을 테이블에서 선택합니다.
3. 왼쪽 탐색 영역에서 **리소스 공급자**를 선택합니다.
4. 테이블에서 "Microsoft.AAD"를 검색하고 **다시 등록**을 클릭합니다.
5. 경고가 해결되었는지 확인하려면 2시간 내에 관리되는 도메인에 대한 상태 페이지를 봅니다.


## <a name="alert-aadds105-password-synchronization-application-is-out-of-date"></a>경고 AADDS105: 암호 동기화 애플리케이션이 만료됨

**경고 메시지:** 애플리케이션 ID가 "d87dcbc6-a371-462e-88e3-28ad15ec4e64"인 서비스 주체가 삭제된 후에 다시 생성되었습니다. 다시 만들기를 수행할 경우 관리되는 도메인을 서비스하는 데 필요한 Azure AD Domain Services 리소스에 일치하지 않는 권한이 남게 됩니다. 관리되는 도메인에서 암호 동기화에 영향이 있을 수 있습니다.


**해결 방법:** 이 단계를 완료하려면 Azure AD PowerShell이 있어야 합니다. Azure AD PowerShell을 설치하는 방법에 대한 내용은 [이 문서](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.)를 참조하세요.

이 문제를 해결하려면 PowerShell 창에서 다음 명령을 입력합니다.
1. Azure AD PowerShell 모듈을 설치하고 가져옵니다.

    ```powershell
    Install-Module AzureAD
    Import-Module AzureAD
    ```
2. 다음 PowerShell 명령을 사용하여 이전 애플리케이션 및 개체 삭제

    ```powershell
    $app = Get-AzureADApplication -Filter "IdentifierUris eq 'https://sync.aaddc.activedirectory.windowsazure.com'"
    Remove-AzureADApplication -ObjectId $app.ObjectId
    $spObject = Get-AzureADServicePrincipal -Filter "DisplayName eq 'Azure AD Domain Services Sync'"
    Remove-AzureADServicePrincipal -ObjectId $app.ObjectId
    ```
3. 둘 모두를 삭제하면 시스템에서 자체적으로 해결하고 암호 동기화에 필요한 애플리케이션을 다시 만듭니다. 경고가 해결되었는지 확인하려면 2시간 후에 도메인의 상태를 확인하세요.


## <a name="contact-us"></a>문의처
[지원이 필요하거나 피드백을 공유하려면](contact-us.md)Azure Active Directory Domain Services 제품 팀에 문의하세요.
