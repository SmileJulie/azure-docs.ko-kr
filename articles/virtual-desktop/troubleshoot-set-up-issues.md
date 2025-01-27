---
title: Windows 가상 데스크톱 테 넌 트 및 호스트 풀 만들기-Azure
description: Windows 가상 데스크톱 테 넌 트 환경을 설치 하는 동안 테 넌 트 및 호스트 풀 문제를 해결 하는 방법입니다.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 07/10/2019
ms.author: v-chjenk
ms.openlocfilehash: 9e58c3bfc0e74f9aac61085608a843954b0923c0
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68249550"
---
# <a name="tenant-and-host-pool-creation"></a>테넌트 및 호스트 풀 만들기

이 문서에서는 Windows 가상 데스크톱 테 넌 트 및 관련 세션 호스트 풀 인프라의 초기 설정 중에 발생 하는 문제에 대해 설명 합니다.

## <a name="provide-feedback"></a>피드백 제공

Windows Virtual Desktop이 미리 보기로 제공되는 기간에는 지원 사례를 접수하지 않습니다. [Windows Virtual Desktop 기술 커뮤니티](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop)를 방문하여 제품 팀 및 활발하게 활동하는 커뮤니티 멤버들과 Windows Virtual Desktop 서비스에 대해 토론해 보세요.

## <a name="acquiring-the-windows-10-enterprise-multi-session-image"></a>Windows 10 Enterprise 다중 세션 이미지 가져오기

Windows 10 Enterprise 다중 세션 이미지를 사용 하려면 Azure Marketplace으로 이동 하 여 **시작** > **Microsoft windows 10** > 및 [Windows 10 Enterprise for Virtual 데스크톱 Preview, 버전 1809](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsdesktop.windows-10?tab=PlansAndPrice)을 선택 합니다.

![가상 데스크톱 Preview 버전 1809에 대 한 Windows 10 Enterprise를 선택 하는 스크린샷](media/AzureMarketPlace.png)

## <a name="creating-windows-virtual-desktop-tenant"></a>Windows 가상 데스크톱 테 넌 트 만들기

이 섹션에서는 Windows 가상 데스크톱 테 넌 트를 만들 때 발생할 수 있는 문제에 대해 설명 합니다.

### <a name="error-the-user-isnt-authorized-to-query-the-management-service"></a>오류: 사용자에 게 관리 서비스를 쿼리할 수 있는 권한이 없습니다.

![사용자에 게 관리 서비스를 쿼리할 수 있는 권한이 없는 PowerShell 창의 스크린샷](media/UserNotAuthorizedNewTenant.png)

원시 오류의 예:

```Error
   New-RdsTenant : User isn't authorized to query the management service.
   ActivityId: ad604c3a-85c6-4b41-9b81-5138162e5559
   Powershell commands to diagnose the failure:
   Get-RdsDiagnosticActivities -ActivityId ad604c3a-85c6-4b41-9b81-5138162e5559
   At line:1 char:1
   + New-RdsTenant -Name "testDesktopTenant" -AadTenantId "01234567-89ab-c ...
   + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
       + CategoryInfo          : FromStdErr: (Microsoft.RDInf...nt.NewRdsTenant:NewRdsTenant) [New-RdsTenant], RdsPowerSh
      ellException
       + FullyQualifiedErrorId : UnauthorizedAccess,Microsoft.RDInfra.RDPowershell.Tenant.NewRdsTenant
```

**원인:** 로그인 한 사용자에 게 Azure Active Directory에 TenantCreator 역할이 할당 되지 않았습니다.

**방법을** [Azure Active Directory 테 넌 트의 사용자에 게 TenantCreator 응용 프로그램 역할 할당](https://docs.microsoft.com/azure/virtual-desktop/tenant-setup-azure-active-directory#assign-the-tenantcreator-application-role-to-a-user-in-your-azure-active-directory-tenant)의 지침을 따릅니다. 이 지침을 수행 하면 사용자가 TenantCreator 역할에 할당 됩니다.

![지정 된 TenantCreator 역할의 스크린샷](media/TenantCreatorRoleAssigned.png)

## <a name="creating-windows-virtual-desktop-session-host-vms"></a>Windows 가상 데스크톱 세션 호스트 Vm 만들기

세션 호스트 Vm은 여러 가지 방법으로 만들 수 있지만 원격 데스크톱 서비스/Windows 가상 데스크톱 팀은 Azure Resource Manager 템플릿과 관련 된 VM 프로 비전 문제도 지원 합니다. Azure Resource Manager 템플릿은 [Azure Marketplace](https://azuremarketplace.microsoft.com/) 및 [GitHub](https://github.com/)에서 사용할 수 있습니다.

## <a name="issues-using-windows-virtual-desktop--provision-a-host-pool-azure-marketplace-offering"></a>Windows 가상 데스크톱을 사용 하는 문제-호스트 풀 Azure Marketplace 제공 제공

Windows 가상 데스크톱 – 호스트 풀 템플릿 프로 비전 Azure Marketplace에서 사용할 수 있습니다.

### <a name="error-when-using-the-link-from-github-the-message-create-a-free-account-appears"></a>오류: GitHub의 링크를 사용 하는 경우 "무료 계정 만들기" 라는 메시지가 표시 됩니다.

![무료 계정을 만들기 위한 스크린샷.](media/be615904ace9832754f0669de28abd94.png)

**원인 1:** Azure에 로그인 하는 데 사용 된 계정에 활성 구독이 없거나 사용 된 계정에 구독을 볼 수 있는 권한이 없습니다.

**수정 1:** 참가자 액세스 권한이 있는 계정 (최소)으로 세션 호스트 Vm을 배포할 구독에 로그인 합니다.

**원인 2:** 사용 중인 구독은 CSP (Microsoft 클라우드 서비스 공급자) 테 넌 트의 일부입니다.

**수정 2:** **새 Windows 가상 데스크톱 호스트 풀 만들기 및 프로 비전** 을 위한 GitHub 위치로 이동 하 고 다음 지침을 따릅니다.

1. **Azure에 배포** 를 마우스 오른쪽 단추로 클릭 하 고 **링크 주소 복사**를 선택 합니다.
2. **메모장** 을 열고 링크를 붙여 넣습니다.
3. # 문자 앞에 CSP 최종 고객 테 넌 트 이름을 삽입 합니다.
4. 브라우저에서 새 링크를 열면 Azure Portal 템플릿이 로드 됩니다.

    ```Example
    Example: https://portal.azure.com/<CSP end customer tenant name>
    #create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%
    2FRDS-Templates%2Fmaster%2Fwvd-templates%2FCreate%20and%20provision%20WVD%20host%20pool%2FmainTemplate.json
    ```

## <a name="azure-resource-manager-template-and-powershell-desired-state-configuration-dsc-errors"></a>Azure Resource Manager 템플릿 및 PowerShell DSC (필요한 상태 구성) 오류

다음 지침에 따라 Azure Resource Manager 템플릿 및 PowerShell DSC의 실패 한 배포 문제를 해결 합니다.

1. [Azure Resource Manager에서 배포 작업 보기](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-operations)를 사용 하 여 배포 오류를 검토 합니다.
2. 배포에 오류가 없으면 활동 로그 보기를 사용 하 여 활동 로그의 오류를 검토 하 여 [리소스에 대 한 작업을 감사](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit)합니다.
3. 오류가 식별 되 면 오류 메시지 및 문제 해결을 위해 [Azure Resource Manager 일반적인 Azure 배포 오류 해결](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-common-deployment-errors) 의 리소스를 사용 합니다.
4. 이전 배포 중에 만들어진 모든 리소스를 삭제 하 고 템플릿을 다시 배포 하는 작업을 다시 시도 합니다.

### <a name="error-your-deployment-failedhostnamejoindomain"></a>오류: 배포 하지 못했습니다.\<> 호스트 이름/joindomain

![배포에 실패 했습니다.](media/e72df4d5c05d390620e07f0d7328d50f.png)

원시 오류의 예:

```Error
 {"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. 
 Please see https://aka.ms/arm-debug for usage details.","details":[{"code":"Conflict","message":"{\r\n \"status\": \"Failed\",\r\n \"error\":
 {\r\n \"code\": \"ResourceDeploymentFailure\",\r\n \"message\": \"The resource operation completed with terminal provisioning state 'Failed'.
 \",\r\n \"details\": [\r\n {\r\n \"code\": \"VMExtensionProvisioningError\",\r\n \"message\": \"VM has reported a failure when processing
 extension 'joindomain'. Error message: \\\"Exception(s) occurred while joining Domain 'diamondsg.onmicrosoft.com'\\\".\"\r\n }\r\n ]\r\n }\r\n}"}]}
```

**원인 1:** Vm을 도메인에 가입 시키기 위해 제공 된 자격 증명이 잘못 되었습니다.

**수정 1:** Vm에 대 한 "잘못 된 자격 증명" 오류는 [세션 호스트 VM 구성](troubleshoot-vm-configuration.md)에서 도메인에 가입 되지 않음을 참조 하세요.

**원인 2:** 도메인 이름이 확인 되지 않습니다.

**수정 2:** Vm에 대 한 "도메인 이름 확인 안 함" 오류는 [세션 호스트 VM 구성](troubleshoot-vm-configuration.md)의 도메인에 가입 되지 않음을 참조 하세요.


### <a name="error-your-deployment-failedunauthorized"></a>오류: 배포에 실패 했습니다. ..\Unauthorized

```Error
{"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/arm-debug for usage details.","details":[{"code":"Unauthorized","message":"{\r\n \"Code\": \"Unauthorized\",\r\n \"Message\": \"The scale operation is not allowed for this subscription in this region. Try selecting different region or scale option.\",\r\n \"Target\": null,\r\n \"Details\": [\r\n {\r\n \"Message\": \"The scale operation is not allowed for this subscription in this region. Try selecting different region or scale option.\"\r\n },\r\n {\r\n \"Code\": \"Unauthorized\"\r\n },\r\n {\r\n \"ErrorEntity\": {\r\n \"ExtendedCode\": \"52020\",\r\n \"MessageTemplate\": \"The scale operation is not allowed for this subscription in this region. Try selecting different region or scale option.\",\r\n \"Parameters\": [\r\n \"default\"\r\n ],\r\n \"Code\": \"Unauthorized\",\r\n \"Message\": \"The scale operation is not allowed for this subscription in this region. Try selecting different region or scale option.\"\r\n }\r\n }\r\n ],\r\n \"Innererror\": null\r\n}"}]}
```

**원인:** 사용 중인 구독은 고객이 배포를 시도 하는 지역의 필수 기능에 액세스할 수 없는 유형입니다. 예를 들어 MSDN, 무료 또는 교육 구독에이 오류가 표시 될 수 있습니다.

**방법을** 구독 유형 또는 지역을 필요한 기능에 액세스할 수 있는 것으로 변경 합니다.

### <a name="error-vmextensionprovisioningerror"></a>오류: VMExtensionProvisioningError

![터미널 프로 비전 상태가 실패 하 여 배포의 스크린샷에 실패 했습니다.](media/7aaf15615309c18a984673be73ac969a.png)

**원인 1:** Windows 가상 데스크톱 환경에서 일시적 오류가 발생 했습니다.

**원인 2:** 연결에 일시적인 오류가 발생 했습니다.

**방법을** PowerShell을 사용 하 여 로그인 하면 Windows 가상 데스크톱 환경이 정상 상태 인지 확인 합니다. [PowerShell을 사용 하 여 호스트 풀 만들기](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell)에서 수동으로 VM 등록을 완료 합니다.

### <a name="error-the-admin-username-specified-isnt-allowed"></a>오류: 지정 된 관리자 사용자 이름은 허용 되지 않습니다.

![지정 된 관리자가 허용 되지 않는 배포의 스크린샷](media/f2b3d3700e9517463ef88fa41875bac9.png)

원시 오류의 예:

```Error
 { "id": "/subscriptions/EXAMPLE/resourceGroups/demoHostDesktop/providers/Microsoft.
  Resources/deployments/vmCreation-linkedTemplate/operations/EXAMPLE", "operationId": "EXAMPLE", "properties": { "provisioningOperation":
 "Create", "provisioningState": "Failed", "timestamp": "2019-01-29T20:53:18.904917Z", "duration": "PT3.0574505S", "trackingId":
 "1f460af8-34dd-4c03-9359-9ab249a1a005", "statusCode": "BadRequest", "statusMessage": { "error": { "code": "InvalidParameter", "message":
 "The Admin Username specified is not allowed.", "target": "adminUsername" } }, "targetResource": { "id": "/subscriptions/EXAMPLE
 /resourceGroups/demoHostDesktop/providers/Microsoft.Compute/virtualMachines/demo", "resourceType": "Microsoft.Compute/virtualMachines", "resourceName": "demo" } }}
```

**원인:** 제공 된 암호에 금지 된 부분 문자열 (admin, administrator, root)이 포함 되어 있습니다.

**방법을** 사용자 이름을 업데이트 하거나 다른 사용자를 사용 합니다.

### <a name="error-vm-has-reported-a-failure-when-processing-extension"></a>오류: VM이 확장을 처리할 때 오류를 보고 했습니다.

![배포에서 터미널 프로 비전 상태를 사용 하 여 완료 된 리소스 작업의 스크린샷에 실패 했습니다.](media/49c4a1836a55d91cd65125cf227f411f.png)

원시 오류의 예:

```Error
{ "id": "/subscriptions/EXAMPLE/resourceGroups/demoHostD/providers/Microsoft.Resources/deployments/
 rds.wvd-hostpool4-preview-20190129132410/operations/5A0757AC9E7205D2", "operationId": "5A0757AC9E7205D2", "properties":
 { "provisioningOperation": "Create", "provisioningState": "Failed", "timestamp": "2019-01-29T21:43:05.1416423Z",
 "duration": "PT7M56.8150879S", "trackingId": "43c4f71f-557c-4abd-80c3-01f545375455", "statusCode": "Conflict",
 "statusMessage": { "status": "Failed", "error": { "code": "ResourceDeploymentFailure", "message":
 "The resource operation completed with terminal provisioning state 'Failed'.", "details": [ { "code":
 "VMExtensionProvisioningError", "message": "VM has reported a failure when processing extension 'dscextension'. 
 Error message: \"DSC Configuration 'SessionHost' completed with error(s). Following are the first few: 
 PowerShell DSC resource MSFT_ScriptResource failed to execute Set-TargetResource functionality with error message: 
 One or more errors occurred. The SendConfigurationApply function did not succeed.\"." } ] } }, "targetResource": 
 { "id": "/subscriptions/EXAMPLE/resourceGroups/demoHostD/providers/Microsoft. 
 Compute/virtualMachines/desktop-1/extensions/dscextension",
 "resourceType": "Microsoft.Compute/virtualMachines/extensions", "resourceName": "desktop-1/dscextension" } }}
```

**원인:** PowerShell DSC 확장에서 VM에 대 한 관리자 액세스 권한을 얻을 수 없습니다.

**방법을** 사용자 이름 및 암호에 가상 컴퓨터에 대 한 관리자 권한이 있는지 확인 하 고 Azure Resource Manager 템플릿을 다시 실행 하십시오.

### <a name="error-deploymentfailed--powershell-dsc-configuration-firstsessionhost-completed-with-errors"></a>오류: DeploymentFailed – PowerShell DSC 구성 ' FirstSessionHost '이 (가) 완료 되었으나 오류가 발생 했습니다.

![오류로 인해 PowerShell DSC 구성 ' FirstSessionHost '를 완료 하 여 배포 실패의 스크린샷](media/64870370bcbe1286906f34cf0a8646ab.png)

원시 오류의 예:

```Error
{
    "code": "DeploymentFailed",
   "message": "At least one resource deployment operation failed. Please list 
 deployment operations for details. 4 Please see https://aka.ms/arm-debug for usage details.",
 "details": [
         { "code": "Conflict",  
         "message": "{\r\n \"status\": \"Failed\",\r\n \"error\": {\r\n \"code\":
         \"ResourceDeploymentFailure\",\r\n \"message\": \"The resource
         operation completed with terminal provisioning state 'Failed'.\",\r\n
         \"details\": [\r\n {\r\n \"code\":
        \"VMExtensionProvisioningError\",\r\n \"message\": \"VM has
              reported a failure when processing extension 'dscextension'.
              Error message: \\\"DSC Configuration 'FirstSessionHost'
              completed with error(s). Following are the first few:
              PowerShell DSC resource MSFT ScriptResource failed to
              execute Set-TargetResource functionality with error message:
              One or more errors occurred. The SendConfigurationApply
              function did not succeed.\\\".\"\r\n }\r\n ]\r\n }\r\n}"  }

```

**원인:** PowerShell DSC 확장에서 VM에 대 한 관리자 액세스 권한을 얻을 수 없습니다.

**방법을** 입력 한 사용자 이름 및 암호에 가상 머신에 대 한 관리 권한이 있는지 확인 하 고 Azure Resource Manager 템플릿을 다시 실행 합니다.

### <a name="error-deploymentfailed--invalidresourcereference"></a>오류: DeploymentFailed-InvalidResourceReference

원시 오류의 예:

```Error
{"code":"DeploymentFailed","message":"At least one resource deployment operation
failed. Please list deployment operations for details. Please see https://aka.ms/arm-
debug for usage details.","details":[{"code":"Conflict","message":"{\r\n \"status\":
\"Failed\",\r\n \"error\": {\r\n \"code\": \"ResourceDeploymentFailure\",\r\n
\"message\": \"The resource operation completed with terminal provisioning state
'Failed'.\",\r\n \"details\": [\r\n {\r\n \"code\": \"DeploymentFailed\",\r\n
\"message\": \"At least one resource deployment operation failed. Please list
deployment operations for details. Please see https://aka.ms/arm-debug for usage
details.\",\r\n \"details\": [\r\n {\r\n \"code\": \"BadRequest\",\r\n \"message\":
\"{\\r\\n \\\"error\\\": {\\r\\n \\\"code\\\": \\\"InvalidResourceReference\\\",\\r\\n
\\\"message\\\": \\\"Resource /subscriptions/EXAMPLE/resourceGroups/ernani-wvd-
demo/providers/Microsoft.Network/virtualNetworks/wvd-vnet/subnets/default
referenced by resource /subscriptions/EXAMPLE/resourceGroups/ernani-wvd-
demo/providers/Microsoft.Network/networkInterfaces/erd. Please make sure that
the referenced resource exists, and that both resources are in the same
region.\\\",\\r\\n\\\"details\\\": []\\r\\n }\\r\\n}\"\r\n }\r\n ]\r\n }\r\n ]\r\n }\r\n}"}]}
```

**원인:** 리소스 그룹 이름의 일부가 템플릿에서 만드는 특정 리소스에 사용 됩니다. 기존 리소스와 일치 하는 이름 때문에 템플릿은 다른 그룹에서 기존 리소스를 선택할 수 있습니다.

**방법을** Azure Resource Manager 템플릿을 실행 하 여 세션 호스트 Vm을 배포 하는 경우 구독 리소스 그룹 이름에 대해 처음 두 문자를 고유 하 게 만듭니다.

### <a name="error-deploymentfailed--invalidresourcereference"></a>오류: DeploymentFailed-InvalidResourceReference

원시 오류의 예:

```Error
{"code":"DeploymentFailed","message":"At least one resource deployment operation
failed. Please list deployment operations for details. Please see https://aka.ms/arm-
debug for usage details.","details":[{"code":"Conflict","message":"{\r\n \"status\":
\"Failed\",\r\n \"error\": {\r\n \"code\": \"ResourceDeploymentFailure\",\r\n
\"message\": \"The resource operation completed with terminal provisioning state
'Failed'.\",\r\n \"details\": [\r\n {\r\n \"code\": \"DeploymentFailed\",\r\n
\"message\": \"At least one resource deployment operation failed. Please list
deployment operations for details. Please see https://aka.ms/arm-debug for usage
details.\",\r\n \"details\": [\r\n {\r\n \"code\": \"BadRequest\",\r\n \"message\":
\"{\\r\\n \\\"error\\\": {\\r\\n \\\"code\\\": \\\"InvalidResourceReference\\\",\\r\\n
\\\"message\\\": \\\"Resource /subscriptions/EXAMPLE/resourceGroups/ernani-wvd-
demo/providers/Microsoft.Network/virtualNetworks/wvd-vnet/subnets/default
referenced by resource /subscriptions/EXAMPLE/resourceGroups/DEMO/providers/Microsoft.Network/networkInterfaces
/EXAMPLE was not found. Please make sure that the referenced resource exists, and that both
resources are in the same region.\\\",\\r\\n \\\"details\\\": []\\r\\n }\\r\\n}\"\r\n
}\r\n ]\r\n }\r\n ]\r\n }\r\n\
```

**원인:** 이 오류는 Azure Resource Manager 템플릿으로 만든 NIC가 VNET에 이미 있는 다른 NIC와 동일한 이름을 갖고 있기 때문입니다.

**방법을** 다른 호스트 접두사를 사용 하십시오.

### <a name="error-deploymentfailed--error-downloading"></a>오류: DeploymentFailed – 다운로드 오류

원시 오류의 예:

```Error
\\\"The DSC Extension failed to execute: Error downloading
https://catalogartifact.azureedge.net/publicartifacts/rds.wvd-hostpool-3-preview-
2dec7a4d-006c-4cc0-965a-02bbe438d6ff-private-preview-
1/Artifacts/DSC/Configuration.zip after 29 attempts: The remote name could not be
resolved: 'catalogartifact.azureedge.net'.\\nMore information about the failure can
be found in the logs located under
'C:\\\\WindowsAzure\\\\Logs\\\\Plugins\\\\Microsoft.Powershell.DSC\\\\2.77.0.0' on
the VM.\\\"
```

**원인:** 이 오류는 Azure Resource Manager 템플릿에 연결 된 zip 파일의 다운로드를 차단 하는 고정 경로, 방화벽 규칙 또는 NSG 때문입니다.

**방법을** 차단 고정 경로, 방화벽 규칙 또는 NSG를 제거 합니다. 필요에 따라 텍스트 편집기에서 Azure Resource Manager 템플릿 json 파일을 열고 zip 파일에 대 한 링크를 가져온 다음 허용 된 위치에 리소스를 다운로드 합니다.

### <a name="error-the-user-isnt-authorized-to-query-the-management-service"></a>오류: 사용자에 게 관리 서비스를 쿼리할 수 있는 권한이 없습니다.

원시 오류의 예:

```Error
"response": { "content": { "startTime": "2019-04-01T17:45:33.3454563+00:00", "endTime": "2019-04-01T17:48:52.4392099+00:00", 
"status": "Failed", "error": { "code": "VMExtensionProvisioningError", "message": "VM has reported a failure when processing 
extension 'dscextension'. Error message: \"DSC Configuration 'FirstSessionHost' completed with error(s). 
Following are the first few: PowerShell DSC resource MSFT_ScriptResource failed to execute Set-TargetResource
 functionality with error message: User is not authorized to query the management service.
\nActivityId: 1b4f2b37-59e9-411e-9d95-4f7ccd481233\nPowershell commands to diagnose the failure:
\nGet-RdsDiagnosticActivities -ActivityId 1b4f2b37-59e9-411e-9d95-4f7ccd481233\n 
The SendConfigurationApply function did not succeed.\"." }, "name": "2c3272ec-d25b-47e5-8d70-a7493e9dc473" } } }}
```

**원인:** 지정 된 Windows Virtual Desktop 테 넌 트 관리자에 올바른 역할 할당이 없습니다.

**방법을** Windows Virtual Desktop 테 넌 트를 만든 사용자는 Windows 가상 데스크톱 PowerShell에 로그인 하 여 시도 된 사용자에 게 역할 할당을 할당 해야 합니다. GitHub Azure Resource Manager 템플릿 매개 변수를 실행 하는 경우 PowerShell 명령을 사용 하 여 다음 지침을 따르세요.

```PowerShell
Add-RdsAccount -DeploymentUrl “https://rdbroker.wvd.microsoft.com”
New-RdsRoleAssignment -TenantName <Windows Virtual Desktop tenant name> -RoleDefinitionName “RDS Contributor” -SignInName <UPN>
```

### <a name="error-user-requires-azure-multi-factor-authentication-mfa"></a>오류: 사용자에 게 Azure MFA (Multi-factor Authentication) 필요

![MFA (Multi-factor Authentication)가 없어서 배포의 스크린샷 실패](media/MFARequiredError.png)

원시 오류의 예:

```Error
"message": "{\r\n  \"status\": \"Failed\",\r\n  \"error\": {\r\n    \"code\": \"ResourceDeploymentFailure\",\r\n    \"message\": \"The resource operation completed with terminal provisioning state 'Failed'.\",\r\n    \"details\": [\r\n      {\r\n        \"code\": \"VMExtensionProvisioningError\",\r\n        \"message\": \"VM has reported a failure when processing extension 'dscextension'. Error message: \\\"DSC Configuration 'FirstSessionHost' completed with error(s). Following are the first few: PowerShell DSC resource MSFT_ScriptResource  failed to execute Set-TargetResource functionality with error message: One or more errors occurred.  The SendConfigurationApply function did not succeed.\\\".\"\r\n      }\r\n    ]\r\n  }\r\n}"
```

**원인:** 지정 된 Windows Virtual Desktop 테 넌 트 관리자에 게 로그인 하려면 Azure MFA (Multi-factor Authentication)가 필요 합니다.

**방법을** 자습서의 [단계에 따라 서비스 주체를 만들고 Windows 가상 데스크톱 테 넌 트의 역할을 할당 합니다. PowerShell](https://docs.microsoft.com/azure/virtual-desktop/create-service-principal-role-powershell)을 사용 하 여 서비스 사용자 및 역할 할당을 만듭니다. 서비스 주체를 사용 하 여 Windows 가상 데스크톱에 로그인 할 수 있는지 확인 한 후에는 사용 중인 방법에 따라 Azure Marketplace 제공 또는 GitHub Azure Resource Manager 템플릿을 다시 실행 합니다. 메서드에 대 한 올바른 매개 변수를 입력 하려면 아래 지침을 따르세요.

Azure Marketplace 제공을 실행 하는 경우 다음 매개 변수에 대 한 값을 제공 하 여 Windows 가상 데스크톱에 올바르게 인증 합니다.

- Windows 가상 데스크톱 테 넌 트 RDS 소유자: 서비스 주체
- 애플리케이션 ID: 만든 새 서비스 주체의 응용 프로그램 id
- 암호/암호 확인: 서비스 사용자에 대해 생성 한 암호 암호입니다.
- Azure AD 테 넌 트 ID: 만든 서비스 사용자의 Azure AD 테 넌 트 ID

GitHub Azure Resource Manager 템플릿을 실행 하는 경우 다음 매개 변수에 대 한 값을 제공 하 여 Windows 가상 데스크톱에 올바르게 인증 합니다.

- 테 넌 트 관리자 UPN (사용자 계정 이름) 또는 응용 프로그램 ID: 만든 새 서비스 주체의 응용 프로그램 id
- 테 넌 트 관리자 암호: 서비스 사용자에 대해 생성 한 암호 암호입니다.
- IsServicePrincipal: **true**
- AadTenantId: 만든 서비스 사용자의 Azure AD 테 넌 트 ID

## <a name="next-steps"></a>다음 단계

- Windows 가상 데스크톱 및 에스컬레이션 트랙 문제 해결에 대 한 개요는 [문제 해결 개요, 사용자 의견 및 지원](troubleshoot-set-up-overview.md)을 참조 하세요.
- Windows 가상 데스크톱에서 VM (가상 컴퓨터)을 구성 하는 동안 발생 하는 문제를 해결 하려면 [세션 호스트 가상 컴퓨터 구성](troubleshoot-vm-configuration.md)을 참조 하세요.
- Windows 가상 데스크톱 클라이언트 연결 문제를 해결 하려면 [원격 데스크톱 클라이언트 연결](troubleshoot-client-connection.md)을 참조 하세요.
- Windows 가상 데스크톱과 함께 PowerShell을 사용할 때 발생 하는 문제를 해결 하려면 [Windows 가상 데스크톱 PowerShell](troubleshoot-powershell.md)을 참조 하세요.
- 미리 보기 서비스에 대 한 자세한 내용은 [Windows Desktop Preview environment](https://docs.microsoft.com/azure/virtual-desktop/environment-setup)를 참조 하세요.
- 문제 해결 자습서를 진행하려면 [자습서: 리소스 관리자 템플릿 배포](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-tutorial-troubleshoot)문제를 해결 합니다.
- 감사 작업에 대해 알아보려면 [리소스 관리자로 작업 감사](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit)를 참조하세요.
- 배포 중 오류를 확인하는 작업에 대해 알아보려면 [배포 작업 보기](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-operations)를 참조하세요.
