---
title: PowerShell을 사용하여 Azure 앱에 대한 ID 만들기 | Microsoft Docs
description: Azure PowerShell을 사용하여 Azure Active Directory 애플리케이션 및 서비스 주체를 만들고 역할 기반 액세스 제어를 통해 리소스에 대한 액세스를 부여하는 방법을 설명합니다. 인증서를 사용하여 애플리케이션을 인증하는 방법을 보여줍니다.
services: active-directory
documentationcenter: na
author: rwike77
manager: CelesteDG
ms.assetid: d2caf121-9fbe-4f00-bf9d-8f3d1f00a6ff
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/24/2018
ms.author: ryanwi
ms.reviewer: tomfitz
ms.collection: M365-identity-device-management
ms.openlocfilehash: 73033f91e9d20c56fedc6b4faf26dcf312fce1e1
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68321102"
---
# <a name="how-to-use-azure-powershell-to-create-a-service-principal-with-a-certificate"></a>방법: Azure PowerShell을 사용하여 인증서로 서비스 주체 만들기

리소스에 액세스해야 하는 앱 또는 스크립트가 있는 경우 앱에 대한 ID를 설정하고 자체 자격 증명으로 앱을 인증할 수 있습니다. 이 ID를 서비스 주체라고 합니다. 이 접근 방법을 사용하면 다음을 수행할 수 있습니다.

* 자체 사용 권한과 다른 앱 ID에 대한 사용 권한을 할당합니다. 일반적으로 이러한 권한은 정확히 앱 실행에 필요한 것으로 제한됩니다.
* 무인 스크립트를 실행할 때 인증을 위해 인증서를 사용합니다.

> [!IMPORTANT]
> 서비스 주체를 만드는 대신 애플리케이션 ID에 Azure 리소스에 대한 관리 ID를 사용하는 것이 좋습니다. 코드가 관리 ID를 지원하는 서비스에서 실행되고 Azure AD(Azure Active Directory) 인증을 지원하는 리소스에 액세스하는 경우 관리 ID를 사용하는 것이 더 좋습니다. Azure 리소스에 대한 관리 ID 및 이것이 지원되는 서비스를 알아보려면 [Azure 리소스에 대한 관리 ID란?](../managed-identities-azure-resources/overview.md)을 참조하세요.

이 아티클에서는 인증서로 인증하는 서비스 주체를 만드는 방법을 보여줍니다. 암호를 사용하여 서비스 주체를 설정하려면 [Azure PowerShell을 사용하여 Azure 서비스 주체 만들기](/powershell/azure/create-azure-service-principal-azureps)를 참조하세요.

이 문서를 진행하려면 [최신 버전](/powershell/azure/install-az-ps)의 PowerShell이 있어야 합니다.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="required-permissions"></a>필요한 사용 권한

이 문서를 완료하려면 Azure AD와 Azure 구독에 대한 충분한 권한이 있어야 합니다. 특히, Azure AD에서 앱을 만들고 역할에 서비스 주체를 할당할 수 있어야 합니다.

계정에 적절한 사용 권한이 있는지를 확인하는 가장 쉬운 방법은 포털을 통하는 것입니다. [필요한 사용 권한 확인](howto-create-service-principal-portal.md#required-permissions)을 참조하세요.

## <a name="create-service-principal-with-self-signed-certificate"></a>자체 서명된 인증서를 사용하여 서비스 주체 만들기

다음 예제는 간단한 시나리오를 다룹니다. 이 시나리오에서는 [New-AzADServicePrincipal](/powershell/module/az.resources/new-azadserviceprincipal)을 사용하여 자체 서명된 인증서로 서비스 주체를 만들고, [New-AzureRmRoleAssignment](/powershell/module/az.resources/new-azroleassignment)를 사용하여 [참가자](../../role-based-access-control/built-in-roles.md#contributor) 역할을 서비스 주체에 할당합니다. 역할 할당의 범위가 현재 선택된 Azure 구독에 지정됩니다. 다른 구독을 선택하려면 [Set-AzContext](/powershell/module/Az.Accounts/Set-AzContext)를 사용합니다.

```powershell
$cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" `
  -Subject "CN=exampleappScriptCert" `
  -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

$sp = New-AzADServicePrincipal -DisplayName exampleapp `
  -CertValue $keyValue `
  -EndDate $cert.NotAfter `
  -StartDate $cert.NotBefore
Sleep 20
New-AzRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

이 예제는 새 서비스 주체가 Azure AD 전체에 전파될 시간을 허용하기 위해 20분간 대기합니다. 스크립트가 대기하는 시간이 충분히 길지 않으면 "보안 주체 {ID}이(가) 디렉터리 {DIR-ID}에 없습니다."라는 오류가 표시됩니다. 이 오류를 해결하려면 잠시 기다린 다음, **New-AzRoleAssignment** 명령을 다시 실행합니다.

**ResourceGroupName** 매개 변수를 사용하여 역할 할당의 범위를 특정 리소스 그룹으로 지정할 수 있습니다. **ResourceType** 및 **ResourceName** 매개 변수를 사용하여 범위를 특정 리소스로 지정할 수도 있습니다. 

**Windows 10 또는 Windows Server 2016**이 설치되지 않은 경우 Microsoft 스크립트 센터에서 [자체 서명된 인증서 생성기](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/)를 다운로드해야 합니다. 해당 내용을 추출하고 필요한 cmdlet을 가져옵니다.

```powershell
# Only run if you could not use New-SelfSignedCertificate
Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
```

스크립트에서 다음 두 줄을 바꾸어 인증서를 생성합니다.

```powershell
New-SelfSignedCertificateEx -StoreLocation CurrentUser `
  -Subject "CN=exampleapp" `
  -KeySpec "Exchange" `
  -FriendlyName "exampleapp"
$cert = Get-ChildItem -path Cert:\CurrentUser\my | where {$PSitem.Subject -eq 'CN=exampleapp' }
```

### <a name="provide-certificate-through-automated-powershell-script"></a>자동화된 PowerShell 스크립트를 통해 인증서 제공

서비스 주체로 로그인할 때마다 AD 앱에 디렉터리의 테넌트 ID를 제공해야 합니다. 테넌트는 Azure AD의 인스턴스입니다.

```powershell
$TenantId = (Get-AzSubscription -SubscriptionName "Contoso Default").TenantId
$ApplicationId = (Get-AzADApplication -DisplayNameStartWith exampleapp).ApplicationId

 $Thumbprint = (Get-ChildItem cert:\CurrentUser\My\ | Where-Object {$_.Subject -eq "CN=exampleappScriptCert" }).Thumbprint
 Connect-AzAccount -ServicePrincipal `
  -CertificateThumbprint $Thumbprint `
  -ApplicationId $ApplicationId `
  -TenantId $TenantId
```

## <a name="create-service-principal-with-certificate-from-certificate-authority"></a>인증 기관의 인증서를 사용하여 서비스 주체 만들기

다음 예제에서는 인증 기관에서 발급한 인증서를 사용하여 서비스 주체를 만듭니다. 지정된 Azure 구독에 할당 범위가 지정됩니다. [참가자](../../role-based-access-control/built-in-roles.md#contributor) 역할에 서비스 주체를 추가합니다. 역할 할당 중에 오류가 발생하는 경우 할당을 다시 시도합니다.

```powershell
Param (
 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword
 )

 Connect-AzAccount
 Import-Module Az.Resources
 Set-AzContext -Subscription $SubscriptionId
 
 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force

 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $KeyValue = [System.Convert]::ToBase64String($PFXCert.GetRawCertData())

 $ServicePrincipal = New-AzADServicePrincipal -DisplayName $ApplicationDisplayName
 New-AzADSpCredential -ObjectId $ServicePrincipal.Id -CertValue $KeyValue -StartDate $PFXCert.NotBefore -EndDate $PFXCert.NotAfter
 Get-AzADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzRoleAssignment -ObjectId $ServicePrincipal.Id -ErrorAction SilentlyContinue
    $Retries++;
 }
 
 $NewRole
```

### <a name="provide-certificate-through-automated-powershell-script"></a>자동화된 PowerShell 스크립트를 통해 인증서 제공
서비스 주체로 로그인할 때마다 AD 앱에 디렉터리의 테넌트 ID를 제공해야 합니다. 테넌트는 Azure AD의 인스턴스입니다.

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
 $PFXCert = New-Object `
  -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 `
  -ArgumentList @($CertPath, $CertPassword)
 $Thumbprint = $PFXCert.Thumbprint

 Connect-AzAccount -ServicePrincipal `
  -CertificateThumbprint $Thumbprint `
  -ApplicationId $ApplicationId `
  -TenantId $TenantId
```

애플리케이션 ID 및 테넌트 ID는 대/소문자를 구분하지 않으므로 스크립트에 직접 포함할 수 있습니다. 테넌트 ID를 검색해야 할 경우 다음을 사용합니다.

```powershell
(Get-AzSubscription -SubscriptionName "Contoso Default").TenantId
```

애플리케이션 ID를 검색해야 할 경우 다음을 사용합니다.

```powershell
(Get-AzADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="change-credentials"></a>자격 증명 변경

보안 위협이 발생했거나 자격 증명이 만료되어 AD 앱용 자격 증명을 변경하려는 경우에는 [Remove-AzADAppCredential](/powershell/module/az.resources/remove-azadappcredential) 및 [New-AzADAppCredential](/powershell/module/az.resources/new-azadappcredential) cmdlet을 사용합니다.

애플리케이션에 대한 자격 증명을 모두 제거하려면 다음을 사용합니다.

```powershell
Get-AzADApplication -DisplayName exampleapp | Remove-AzADAppCredential
```

인증서 값을 추가하려면 이 문서에 설명된 대로 자체 서명된 인증서를 만듭니다. 그 후 다음을 사용합니다.

```powershell
Get-AzADApplication -DisplayName exampleapp | New-AzADAppCredential `
  -CertValue $keyValue `
  -EndDate $cert.NotAfter `
  -StartDate $cert.NotBefore
```

## <a name="debug"></a>디버그

서비스 주체를 만들 때 다음과 같은 오류가 발생할 수 있습니다.

* **"Authentication_Unauthorized"** 또는 **"컨텍스트에서 구독을 찾을 수 없습니다."** - 계정에 Azure AD에서 앱을 등록하는 데 [필요한 권한](#required-permissions)이 없으면 이 오류가 발생합니다. 일반적으로 Azure Active Directory의 관리 사용자만 앱을 등록할 수 있고 내 계정이 관리자가 아니면 이 오류가 표시됩니다. 관리자 역할에 사용자를 할당하거나 사용자가 앱을 등록할 수 있게 설정하도록 관리자에게 요청합니다.

* 계정에 **"'/subscriptions/{guid}' 범위에 대해 'Microsoft.Authorization/roleAssignments/write' 작업을 수행할 수 있는 권한이 없습니다."** - 이 오류는 ID에 역할을 할당할 수 있는 충분한 권한이 계정에 없을 때 표시됩니다. 구독 관리자에게 사용자 액세스 관리자 역할에 사용자를 추가할 것을 요청합니다.

## <a name="next-steps"></a>다음 단계

* 암호를 사용하여 서비스 주체를 설정하려면 [Azure PowerShell을 사용하여 Azure 서비스 주체 만들기](/powershell/azure/create-azure-service-principal-azureps)를 참조하세요.
* 리소스 관리를 위해 Azure에 애플리케이션을 통합하는 자세한 단계를 보려면 [Azure Resource Manager API를 사용한 권한 부여 개발자 가이드](../../azure-resource-manager/resource-manager-api-authentication.md)를 참조하세요.
* 애플리케이션 및 서비스 주체에 대한 자세한 내용은 [애플리케이션 개체 및 서비스 주체 개체](app-objects-and-service-principals.md)를 참조하세요.
* Azure AD 인증에 대한 자세한 내용은 [Azure AD의 인증 시나리오](authentication-scenarios.md)를 참조하세요.
