---
title: Linux VM의 Azure Key Vault 설정 | Microsoft Docs
description: Azure CLI를 사용하여 Azure Resource Manager 가상 머신에서 사용할 Key Vault를 설정하는 방법을 설명합니다.
services: virtual-machines-linux
documentationcenter: ''
author: singhkays
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: bccdd5ab-5ccf-4760-9039-92c6eafb15bd
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 02/24/2017
ms.author: kasing
ms.openlocfilehash: 736a30fe83ff26cd1dd4f197de9e7db925b7243b
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67671365"
---
# <a name="how-to-set-up-key-vault-for-virtual-machines-with-the-azure-cli"></a>Azure CLI를 사용하여 가상 머신용 Key Vault를 설정하는 방법

Azure Resource Manager 스택에서 암호/인증서는 Key Vault에서 제공하는 리소스로 모델링됩니다. Azure Key Vault에 대한 자세한 내용을 보려면 [Azure Key Vault란?](../../key-vault/key-vault-whatis.md) Key Vault을 Azure Resource Manager VM에서 사용하려면 Key Vault에 대한 *EnabledForDeployment* 속성을 true로 설정해야 합니다. 이 문서에서는 Azure CLI를 사용하여 Azure VM(가상 머신)에서 사용할 Key Vault를 설정하는 방법을 설명합니다. 

이러한 단계를 수행하려면 최신 [Azure CLI](/cli/azure/install-az-cli2)를 설치하고 [az login](/cli/azure/reference-index)을 사용하여 Azure 계정에 로그인해야 합니다.

## <a name="create-a-key-vault"></a>주요 자격 증명 모음 만들기
[az keyvault create](/cli/azure/keyvault)를 사용하여 주요 자격 증명 모음을 만들고 배포 정책을 할당합니다. 다음 예제에서는 `myResourceGroup` 리소스 그룹에 `myKeyVault`이라는 주요 자격 증명 모음을 만듭니다.

```azurecli
az keyvault create -l westus -n myKeyVault -g myResourceGroup --enabled-for-deployment true
```

## <a name="update-a-key-vault-for-use-with-vms"></a>VM과 함께 사용할 Key Vault를 업데이트합니다.
[az keyvault update](/cli/azure/keyvault)를 사용하여 기존 주요 자격 증명 모음에 대한 배포 정책을 설정합니다. 다음에서는 `myResourceGroup` 리소스 그룹에 `myKeyVault`이라는 주요 자격 증명 모음을 업데이트합니다.

```azurecli
az keyvault update -n myKeyVault -g myResourceGroup --set properties.enabledForDeployment=true
```

## <a name="use-templates-to-set-up-key-vault"></a>템플릿을 사용하여 주요 자격 증명 모음 설정
템플릿을 사용하는 경우 다음과 같이 `enabledForDeployment` 속성을 Key Vault 리소스의 `true`로 설정해야 합니다.

```json
{
    "type": "Microsoft.KeyVault/vaults",
    "name": "ContosoKeyVault",
    "apiVersion": "2015-06-01",
    "location": "<location-of-key-vault>",
    "properties": {
    "enabledForDeployment": "true",
    ....
    ....
    }
}
```

## <a name="next-steps"></a>다음 단계
템플릿을 사용하여 Key Vault을 만들 때 구성할 수 있는 다른 옵션에 대해서는 [주요 자격 증명 모음 만들기](https://azure.microsoft.com/documentation/templates/101-key-vault-create/)를 참조하세요.
