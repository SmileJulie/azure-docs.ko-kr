---
title: Resource Manager 템플릿 배포 시 Azure Key Vault 통합 | Microsoft Docs
description: Azure Key Vault를 사용하여 Resource Manager 템플릿 배포 중에 보안 매개 변수 값을 전달하는 방법을 알아봅니다.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 05/23/2019
ms.topic: tutorial
ms.author: jgao
ms.custom: seodec18
ms.openlocfilehash: b91a3488750795e8ee9df6602bb2b3df8a9b08ec
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67436586"
---
# <a name="tutorial-integrate-azure-key-vault-in-your-resource-manager-template-deployment"></a>자습서: Resource Manager 템플릿 배포에 Azure Key Vault 통합

Azure Key Vault에서 비밀을 검색하여 Azure Resource Manager를 배포할 때 비밀을 매개 변수로 전달하는 방법을 알아봅니다. 이 매개 변수 값은 해당 Key Vault ID만 참조하기 때문에 절대 노출되지 않습니다. 자세한 내용은 [Azure Key Vault를 사용하여 배포 중에 보안 매개 변수 값 전달](./resource-manager-keyvault-parameter.md)을 참조하세요.

[리소스 배포 순서 설정](./resource-manager-tutorial-create-templates-with-dependent-resources.md) 자습서에서는 VM(가상 머신)을 만듭니다. VM 관리자 사용자 이름과 암호를 제공해야 합니다. 암호를 제공하는 대신, Azure Key Vault에 암호를 미리 저장한 다음, 배포 중에 키 자격 증명 모음에서 암호를 검색하도록 템플릿을 사용자 지정할 수 있습니다.

![키 자격 증명 모음과 Resource Manager 템플릿의 통합을 표시하는 다이어그램](./media/resource-manager-tutorial-use-key-vault/resource-manager-template-key-vault-diagram.png)

이 자습서에서 다루는 작업은 다음과 같습니다.

> [!div class="checklist"]
> * 키 자격 증명 모음 준비
> * 빠른 시작 템플릿 열기
> * 매개 변수 파일 편집
> * 템플릿 배포
> * 배포 유효성 검사
> * 리소스 정리

Azure 구독이 아직 없는 경우 시작하기 전에 [체험](https://azure.microsoft.com/free/) 계정을 만듭니다.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>필수 조건

이 문서를 완료하려면 다음이 필요합니다.

* [Resource Manager Tools 확장](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites)이 있는 [Visual Studio Code](https://code.visualstudio.com/).
* 보안을 강화하려면 VM 관리자 계정용으로 생성된 암호를 사용하세요. 암호를 생성하는 방법에 대한 샘플은 다음과 같습니다.

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    생성된 암호가 VM 암호 요구 사항을 충족하는지 확인합니다. Azure 서비스마다 특정한 암호 요구 사항이 있습니다. VM 암호 요구 사항은 [VM을 만들 때의 암호 요구 사항은 무엇인가요?](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm)를 참조하세요.

## <a name="prepare-a-key-vault"></a>키 자격 증명 모음 준비

이 섹션에서는 템플릿을 배포할 때 비밀을 검색할 수 있도록 키 자격 증명 모음을 만들고 비밀을 키 자격 증명 모음에 추가합니다. 키 자격 증명 모음을 만드는 여러 가지 방법이 있습니다. 이 자습서에서는 Azure PowerShell을 사용하여 [Resource Manager 템플릿](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorials-use-key-vault/CreateKeyVault.json)을 배포합니다. 이 템플릿은 다음을 수행합니다.

* `enabledForTemplateDeployment` 속성이 활성화된 키 자격 증명 모음을 만듭니다. 이 속성이 *true*여야 템플릿 배포 프로세스가 이 키 자격 증명 모음에 정의된 비밀에 액세스할 수 있습니다.
* 키 자격 증명 모음에 비밀을 추가합니다. 비밀에는 VM 관리자 암호가 저장됩니다.

> [!NOTE]
> 가상 머신 템플릿을 배포하는 사용자가 키 자격 증명 모음의 소유자나 기여자가 아닌 경우에는 소유자나 기여자가 키 자격 증명 모음의 *Microsoft.KeyVault/vaults/deploy/action*에 대한 액세스 권한을 해당 사용자에게 부여해야 합니다. 자세한 내용은 [Azure Key Vault를 사용하여 배포 중에 보안 매개 변수 값 전달](./resource-manager-keyvault-parameter.md)을 참조하세요.

다음 Azure PowerShell 스크립트를 실행하려면 **사용해 보세요**를 선택하여 Azure Cloud Shell을 엽니다. 스크립트를 붙여넣으려면 마우스 오른쪽 단추로 셸 창을 클릭한 다음, **붙여넣기**를 선택합니다.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$upn = Read-Host -Prompt "Enter your user principal name (email address) used to sign in to Azure"
$secretValue = Read-Host -Prompt "Enter the virtual machine administrator password" -AsSecureString

$resourceGroupName = "${projectName}rg"
$keyVaultName = $projectName
$adUserId = (Get-AzADUser -UserPrincipalName $upn).Id
$templateUri = "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorials-use-key-vault/CreateKeyVault.json"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -keyVaultName $keyVaultName -adUserId $adUserId -secretValue $secretValue
```

> [!IMPORTANT]
> * 리소스 그룹 이름은 **rg**가 추가된 프로젝트 이름입니다. [이 자습서에서 만든 리소스를 더 쉽게 정리](#clean-up-resources)하려면 [다음 템플릿을 배포](#deploy-the-template)할 때 동일한 프로젝트 이름과 리소스 그룹 이름을 사용하세요.
> * 비밀의 기본 이름은 **vmAdminPassword**입니다. 이 이름은 템플릿에 하드 코드되어 있습니다.
> * 템플릿에서 비밀을 검색할 수 있도록 하려면 키 자격 증명 모음에 템플릿 배포를 위해 Azure Resource Manager에 대한 액세스 사용이라는 액세스 정책을 사용하도록 설정해야 합니다. 이 정책은 템플릿에서 사용하도록 설정됩니다. 이 액세스 정책에 대한 자세한 내용은 [키 자격 증명 모음 및 비밀 배포](./resource-manager-keyvault-parameter.md#deploy-key-vaults-and-secrets)를 참조하세요.

템플릿에는 *keyVaultId*라는 하나의 출력 값이 있습니다. 가상 머신을 배포할 때 나중에 사용할 ID 값을 적어 둡니다. 리소스 ID 형식은 다음과 같습니다.

```json
/subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>
```

ID를 복사하여 붙여넣으면 ID가 여러 줄로 분할될 수 있습니다. 줄을 병합하고 여분의 공백을 제거하세요.

배포의 유효성을 검사하려면 동일한 셸 창에서 다음 PowerShell 명령을 실행하여 비밀을 일반 텍스트로 검색합니다. 이 명령은 이전 PowerShell 스크립트에 정의된 *$keyVaultName* 변수를 사용하므로 동일한 셸 세션에서만 작동합니다.

```azurepowershell
(Get-AzKeyVaultSecret -vaultName $keyVaultName  -name "vmAdminPassword").SecretValueText
```

이제 키 자격 증명 모음 및 비밀이 준비되었습니다. 다음 섹션에서는 배포 중에 비밀을 검색하도록 기존 템플릿을 사용자 지정하는 방법을 보여줍니다.

## <a name="open-a-quickstart-template"></a>빠른 시작 템플릿 열기

Azure 빠른 시작 템플릿은 Resource Manager 템플릿용 리포지토리입니다. 템플릿을 처음부터 새로 만드는 대신 샘플 템플릿을 찾아서 사용자 지정할 수 있습니다. 이 자습서에 사용되는 템플릿의 이름은 [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/)입니다.

1. Visual Studio Code에서 **파일** > **파일 열기**를 차례로 선택합니다.

1. **파일 이름** 상자에 다음 URL을 붙여넣습니다.

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```

1. **열기**를 선택하여 파일을 엽니다. [자습서: 종속 리소스가 있는 Azure Resource Manager 템플릿 만들기](./resource-manager-tutorial-create-templates-with-dependent-resources.md)에서 사용한 대로 Cloud Shell 배포 방법을 사용합니다.
   템플릿은 5개의 리소스를 정의합니다.

   * `Microsoft.Storage/storageAccounts`. [템플릿 참조](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts)를 참조하세요.
   * `Microsoft.Network/publicIPAddresses`. [템플릿 참조](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses)를 참조하세요.
   * `Microsoft.Network/virtualNetworks`. [템플릿 참조](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks)를 참조하세요.
   * `Microsoft.Network/networkInterfaces`. [템플릿 참조](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces)를 참조하세요.
   * `Microsoft.Compute/virtualMachines`. [템플릿 참조](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines)를 참조하세요.

   템플릿을 사용자 지정하기 전에 템플릿의 몇 가지 기본적인 내용을 이해하면 유용합니다.

1. **파일** > **다른 이름으로 저장**을 선택하여 파일 복사본을 로컬 컴퓨터에 *azuredeploy.json*이라는 이름으로 저장합니다.

1. 1~3단계를 반복하여 다음 URL을 연 다음, 파일을 *azuredeploy.parameters.json*으로 저장합니다.

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.parameters.json
    ```

## <a name="edit-the-parameters-file"></a>매개 변수 파일 편집

템플릿 파일은 변경하지 않아도 됩니다.

1. 열려 있지 않은 경우 Visual Studio Code에서 *azuredeploy.parameters.json*을 엽니다.
1. `adminPassword` 매개 변수를 다음과 같이 업데이트합니다.

    ```json
    "adminPassword": {
        "reference": {
            "keyVault": {
            "id": "/subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>"
            },
            "secretName": "vmAdminPassword"
        }
    },
    ```

    > [!IMPORTANT]
    > **id**의 값을 이전 절차에서 만든 키 자격 증명 모음의 리소스 ID로 바꿉니다.

    ![키 자격 증명 모음과 Resource Manager 템플릿 가상 머신 배포 매개 변수 파일 통합](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-vm-parameters-file.png)

1. 다음 값을 업데이트합니다.

    * **adminUsername**: 가상 머신 관리자 계정의 이름입니다.
    * **dnsLabelPrefix**: dnsLabelPrefix 값의 이름을 지정합니다.

    이름의 예는 이전의 이미지를 참조하세요.

1. 변경 내용을 저장합니다.

## <a name="deploy-the-template"></a>템플릿 배포

[템플릿 배포](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template)의 지침을 따릅니다. *azuredeploy.json* 및 *azuredeploy.parameters.json*을 모두 Cloud Shell에 업로드한 후 다음 PowerShell 스크립트를 사용하여 템플릿을 배포합니다.

```azurepowershell
$projectName = Read-Host -Prompt "Enter the same project name that is used for creating the key vault"
$location = Read-Host -Prompt "Enter the same location that is used for creating the key vault (i.e. centralus)"
$resourceGroupName = "${projectName}rg"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile "$HOME/azuredeploy.json" `
    -TemplateParameterFile "$HOME/azuredeploy.parameters.json"
```

템플릿을 배포할 때는 키 자격 증명 모음에서 사용한 것과 동일한 리소스 그룹을 사용합니다. 이 접근 방식을 사용하면 리소스 그룹 2개를 삭제하지 않고 1개만 삭제하면 되므로 리소스를 더 쉽게 정리할 수 있습니다.

## <a name="validate-the-deployment"></a>배포 유효성 검사

가상 머신 배포에 성공한 후에는 키 자격 증명 모음에 저장된 암호를 사용하여 로그인 자격 증명을 테스트합니다.

1. [Azure Portal](https://portal.azure.com)을 엽니다.

1. **리소스 그룹** >  **\<*YourResourceGroupName*>**  > **simpleWinVM**을 선택합니다.
1. 위쪽에 있는 **연결**을 선택합니다.
1. **RDP 파일 다운로드**를 선택한 다음, 지침에 따라 키 자격 증명 모음에 저장된 암호를 사용하여 가상 머신에 로그인합니다.

## <a name="clean-up-resources"></a>리소스 정리

Azure 리소스가 더 이상 필요하지 않은 경우 리소스 그룹을 삭제하여 배포된 리소스를 정리합니다.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that is used for creating the key vault"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -Name $resourceGroupName
```

## <a name="next-steps"></a>다음 단계

이 자습서에서는 Azure Key Vault에서 비밀을 검색했습니다. 그런 다음, 템플릿 배포에 비밀을 사용했습니다. 연결된 템플릿을 만드는 방법을 알아보려면 다음을 참조하세요.

> [!div class="nextstepaction"]
> [연결된 템플릿 만들기](./resource-manager-tutorial-create-linked-templates.md)
