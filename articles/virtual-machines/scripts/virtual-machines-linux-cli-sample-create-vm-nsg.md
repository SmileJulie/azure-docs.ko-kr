---
title: Azure CLI 스크립트 샘플 - 내부 및 외부 NSG를 사용하여 두 개의 VM 만들기 | Microsoft Docs
description: Azure CLI 스크립트 샘플 - 내부 및 외부 NSG를 사용하여 두 개의 VM 만들기
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 0ed3ed5c3f44f9ddf912b420c4a89b6ca0fa94af
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709583"
---
# <a name="secure-network-traffic-between-virtual-machines"></a>가상 머신 간의 네트워크 트래픽 보안

이 스크립트는 두 개의 가상 머신을 만들고 해당 컴퓨터에 들어오는 트래픽의 보안을 유지합니다. 첫 번째 가상 머신은 인터넷에 액세스할 수 있고 포트 22 및 포트 80에서 트래픽을 허용하도록 NSG(네트워크 보안 그룹)를 구성합니다. 두 번째 가상 머신은 인터넷에 액세스할 수 없고 NSG가 첫 번째 가상 머신의 트래픽을 허용하도록 구성합니다.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>샘플 스크립트

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nsg/create-vm-nsg.sh "Create VM with NSG")]

## <a name="clean-up-deployment"></a>배포 정리

다음 명령을 실행하여 리소스 그룹, VM 및 모든 관련된 리소스를 제거할 수 있습니다.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>스크립트 설명

이 스크립트는 다음 명령을 사용하여 리소스 그룹, 가상 컴퓨터 및 모든 관련된 리소스를 만듭니다. 테이블에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | 모든 리소스가 저장되는 리소스 그룹을 만듭니다. |
| [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet) | Azure Virtual Network 및 서브넷을 만듭니다. |
| [az network vnet subnet create](https://docs.microsoft.com/cli/azure/network/vnet/subnet) | 서브넷을 만듭니다. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm) | 가상 머신을 만들고 네트워크 카드, 가상 네트워크, 서브넷 및 NSG에 연결합니다. 또한 이 명령은 사용할 가상 머신 이미지와 관리 자격 증명을 지정합니다.  |
| [az network nsg rule list](https://docs.microsoft.com/cli/azure/network/nsg/rule) | 네트워크 보안 그룹 규칙에 대한 정보를 반환합니다. 이 샘플에서 규칙 이름은 스크립트에서 나중에 사용하기 위해 변수에 저장됩니다. |
| [az network nsg rule update](https://docs.microsoft.com/cli/azure/network/nsg/rule) | NSG 규칙을 업데이트합니다. 이 샘플에서 백 엔드 규칙은 프런트 엔드 서브넷의 트래픽을 통과하도록 업데이트됩니다. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension) | 모든 중첩 리소스를 포함한 리소스 그룹을 삭제합니다. |

## <a name="next-steps"></a>다음 단계

Azure CLI에 대한 자세한 내용은 [Azure CLI 설명서](https://docs.microsoft.com/cli/azure)를 참조하세요.

추가 가상 머신 CLI 스크립트 샘플은 [Azure Linux VM 설명서](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)에서 확인할 수 있습니다.
