---
title: Azure CLI 스크립트 샘플 - NLB를 사용하여 Linux VM 만들기 | Microsoft Docs
description: Azure CLI 스크립트 샘플 - NLB를 사용하여 Linux VM 만들기
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
ms.openlocfilehash: 2e9b536ae6c7056348ac964a2c85ccd698a80ae4
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709362"
---
# <a name="create-a-highly-available-vm"></a>고가용성 VM 만들기

이 스크립트 샘플에서는 항상 사용 가능하고 부하 분산된 구성에서 구성된 여러 Ubuntu 가상 머신을 실행하는 데 필요한 모든 항목을 만듭니다. 스크립트를 실행하면 3개의 가상 머신이 Azure 가용성 집합에 조인되고 Azure Load Balancer를 통해 액세스할 수 있습니다.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>샘플 스크립트

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>배포 정리

다음 명령을 실행하여 리소스 그룹, VM 및 모든 관련된 리소스를 제거할 수 있습니다.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>스크립트 설명

이 스크립트는 다음 명령을 사용하여 리소스 그룹, 가상 머신, 가용성 집합, 부하 분산 장치 및 모든 관련된 리소스를 만듭니다. 테이블에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | 모든 리소스가 저장되는 리소스 그룹을 만듭니다. |
| [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet) | Azure Virtual Network 및 서브넷을 만듭니다. |
| [az network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip) | 고정 IP 주소 및 연결된 DNS 이름을 사용하여 공용 IP 주소를 만듭니다. |
| [az network lb create](https://docs.microsoft.com/cli/azure/network/lb) | Azure NLB(Network Load Balancer)를 만듭니다. |
| [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe) | NLB 프로브를 만듭니다. NLB 프로브는 NLB 집합에서 각 VM을 모니터링하는 데 사용됩니다. 모든 VM이 액세스할 수 없게 되면 트래픽은 VM에 라우팅되지 않습니다. |
| [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule) | NLB 규칙을 만듭니다. 이 샘플에서는 포트 80에 대한 규칙을 만듭니다. HTTP 트래픽이 NLB에 도착하면 NLB 집합에서 VM 중 하나인 포트 80에 라우팅됩니다. |
| [az network lb inbound-nat-rule create](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule) | NLB NAT(네트워크 주소 변환) 규칙을 만듭니다.  NAT 규칙은 VM에 있는 포트에 NLB의 포트를 매핑합니다. 이 샘플에서는 NLB 집합의 각 VM에 SSH 트래픽에 대한 NAT 규칙을 만듭니다.  |
| [az network nsg create](https://docs.microsoft.com/cli/azure/network/nsg) | 인터넷과 가상 컴퓨터 간에 보안 경계인 NSG(네트워크 보안 그룹)을 만듭니다. |
| [az network nsg rule create](https://docs.microsoft.com/cli/azure/network/nsg/rule) | 인바운드 트래픽을 허용하도록 NSG 규칙을 만듭니다. 이 샘플에서 SSH 트래픽에 대해 포트 22가 열립니다. |
| [az network nic create](https://docs.microsoft.com/cli/azure/network/nic) | 가상 네트워크 카드를 만들고 가상 네트워크, 서브넷 및 NSG에 연결합니다. |
| [az vm availability-set create](https://docs.microsoft.com/cli/azure/network/lb/rule) | 가용성 집합을 만듭니다. 가용성 집합을 사용하면 오류가 발생하는 경우 물리적 리소스 간에 가상 머신을 분산하여 애플리케이션 가동 시간을 확인합니다. 전체 집합은 영향을 받지 않습니다. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm/availability-set) | 가상 머신을 만들고 네트워크 카드, 가상 네트워크, 서브넷 및 NSG에 연결합니다. 또한 이 명령은 사용할 가상 머신 이미지와 관리 자격 증명을 지정합니다.  |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension) | 모든 중첩 리소스를 포함한 리소스 그룹을 삭제합니다. |

## <a name="next-steps"></a>다음 단계

Azure CLI에 대한 자세한 내용은 [Azure CLI 설명서](https://docs.microsoft.com/cli/azure)를 참조하세요.

추가 가상 머신 CLI 스크립트 샘플은 [Azure Linux VM 설명서](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)에서 확인할 수 있습니다.
