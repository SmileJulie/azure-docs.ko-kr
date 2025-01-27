---
title: Azure PowerShell을 사용 하 여 RBAC-Azure Storage를 사용 하 여 blob 및 큐 데이터를 Azure AD 액세스 권한 관리
description: 컨테이너 및 역할 기반 액세스 제어 (RBAC)를 사용 하 여 큐에 대 한 액세스를 할당 하려면 Azure PowerShell을 사용 합니다. Azure Storage는 Azure AD 통해 인증에 대 한 기본 제공 및 사용자 지정 RBAC 역할을 지원합니다.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 06/26/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: bf888b72cca806822ca7a37542e71a5be0c8d5c3
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443735"
---
# <a name="grant-access-to-azure-blob-and-queue-data-with-rbac-using-powershell"></a>PowerShell을 사용 하 여 RBAC 사용 하 여 Azure blob 및 큐 데이터에 액세스 권한 부여

Azure AD(Azure Active Directory)에서는 [RBAC(역할 기반 액세스 제어)](../../role-based-access-control/overview.md)를 통해 보호된 리소스에 액세스 권한을 부여합니다. Azure Storage는 컨테이너 또는 큐에 액세스하는 데 사용되는 사용 권한의 공통 집합을 포함하는 기본 제공 RBAC 역할 집합을 정의합니다. 

RBAC 역할에는 Azure AD 보안 주체에 할당 된 Azure 부여 해당 보안 주체에 대 한 해당 리소스에 액세스 합니다. 액세스 권한은 구독, 리소스 그룹, 저장소 계정 또는 개별 컨테이너나 큐의 수준에 범위를 지정할 수 있습니다. 사용자, 그룹, 응용 프로그램 서비스 주체를 Azure AD 보안 주체 수 또는 [Azure 리소스에 대 한 id 관리](../../active-directory/managed-identities-azure-resources/overview.md)합니다.

이 문서에서는 기본 제공 RBAC 역할을 나열 하 고 사용자에 게 할당 하려면 Azure PowerShell을 사용 하는 방법을 설명 합니다. Azure PowerShell을 사용 하는 방법에 대 한 자세한 내용은 참조 하세요. [Azure PowerShell 개요](https://docs.microsoft.com/powershell/azure/overview)합니다.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="rbac-roles-for-blobs-and-queues"></a>Blob 및 큐의 RBAC 역할

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

## <a name="determine-resource-scope"></a>리소스 범위를 결정 합니다.

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="list-available-rbac-roles"></a>사용 가능한 RBAC 역할 목록

Azure PowerShell을 사용 하 여 사용 가능한 기본 제공 RBAC 역할을 나열 하려면 사용 합니다 [Get AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) 명령:

```powershell
Get-AzRoleDefinition | FT Name, Description
```

Azure의 다른 기본 제공 역할 함께 나열 된 기본 제공 Azure Storage 데이터 역할을 볼 수 있습니다.

```Example
Storage Blob Data Contributor             Allows for read, write and delete access to Azure Storage blob containers and data
Storage Blob Data Owner                   Allows for full access to Azure Storage blob containers and data, including assigning POSIX access control.
Storage Blob Data Reader                  Allows for read access to Azure Storage blob containers and data
Storage Queue Data Contributor            Allows for read, write, and delete access to Azure Storage queues and queue messages
Storage Queue Data Message Processor      Allows for peek, receive, and delete access to Azure Storage queue messages
Storage Queue Data Message Sender         Allows for sending of Azure Storage queue messages
Storage Queue Data Reader                 Allows for read access to Azure Storage queues and queue messages
```

## <a name="assign-an-rbac-role-to-a-security-principal"></a>보안 주체에 RBAC 역할 할당

RBAC 역할에 보안 주체를 할당 하려면 사용 합니다 [새로 만들기-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment) 명령입니다. 명령의 형식을 할당의 범위에 따라 달라질 수 있습니다. 다음 예제에서는 다양 한 범위의 사용자 역할을 할당 하는 방법을 보여주지만 동일한 명령을 사용 하 여 모든 보안 주체에 역할을 할당할 수 있습니다.

### <a name="container-scope"></a>컨테이너 범위

컨테이너에 범위가 지정 된 역할에 할당 하려면에 대 한 컨테이너의 범위를 포함 하는 문자열을 지정 합니다 `--scope` 매개 변수입니다. 컨테이너에 대 한 범위는 형식:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/<container-name>
```

다음 예제에서는 합니다 **Storage Blob 데이터 기여자** 라는 컨테이너에 범위가 지정 된 사용자에 게 역할 *샘플 컨테이너*합니다. 샘플 값와 대괄호 안의 자리 표시자 값을 고유한 값으로 바꿀 수 있는지 확인 합니다. 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/sample-container"
```

### <a name="queue-scope"></a>큐 범위

큐에 범위가 지정 된 역할에 할당 하려면 큐 범위를 포함 하는 문자열을 지정 합니다 `--scope` 매개 변수입니다. 큐에 대 한 범위는 형식:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/<queue-name>
```

다음 예제에서는 합니다 **Storage 큐 데이터 기여자** 라는 큐에 범위가 지정 된 사용자에 게 역할 *샘플 큐*합니다. 샘플 값와 대괄호 안의 자리 표시자 값을 고유한 값으로 바꿀 수 있는지 확인 합니다. 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Queue Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/sample-queue"
```

### <a name="storage-account-scope"></a>저장소 계정 범위

저장소 계정에 범위가 지정 된 역할에 할당 하려면에 대 한 저장소 계정 리소스의 범위를 지정 합니다 `--scope` 매개 변수입니다. 저장소 계정에 대 한 범위는 형식:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>
```

다음 예제에서는 범위 하는 방법을 합니다 **Storage Blob 데이터 판독기** 저장소 계정 수준에서 사용자 역할. 샘플 값을 고유한 값으로 대체 해야 합니다. 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Reader" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>"
```

### <a name="resource-group-scope"></a>리소스 그룹 범위

리소스 그룹으로 범위가 지정 된 역할에 할당 하려면 리소스 그룹 이름 또는 ID에 대 한 지정 된 `--resource-group` 매개 변수입니다. 다음 예제에서는 합니다 **Storage 큐 데이터 판독기** 리소스 그룹 수준의 사용자 역할. 사용자 고유의 값을 사용 하 여 샘플 값과 대괄호 안의 자리 표시자 값을 교체할 수 있는지 확인 합니다. 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Queue Data Reader" `
    -ResourceGroupName "sample-resource-group"
```

### <a name="subscription-scope"></a>구독 범위

구독에 범위가 지정 된 역할에 할당 하려면 구독에 대 한 범위를 지정 합니다 `--scope` 매개 변수입니다. 구독에 대 한 범위는 형식:

```
/subscriptions/<subscription>
```

다음 예제에서는 할당 하는 방법의 **Storage Blob 데이터 판독기** 저장소 계정 수준에서 사용자 역할. 샘플 값을 고유한 값으로 대체 해야 합니다. 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Reader" `
    -Scope  "/subscriptions/<subscription>"
```

## <a name="next-steps"></a>다음 단계

- [RBAC 및 Azure PowerShell을 사용하여 Azure 리소스에 대한 액세스 관리](../../role-based-access-control/role-assignments-powershell.md)
- [Azure CLI를 사용 하 여 RBAC 사용 하 여 Azure blob 및 큐 데이터에 액세스 권한 부여](storage-auth-aad-rbac-cli.md)
- [Azure blob 및 큐 데이터에 RBAC 사용 하 여 Azure portal에서 액세스 권한 부여](storage-auth-aad-rbac-portal.md)
