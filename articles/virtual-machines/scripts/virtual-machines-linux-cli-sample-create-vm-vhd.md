---
title: Azure CLI 스크립트 샘플 - VHD를 사용하여 VM 만들기 | Microsoft 문서
description: Azure CLI 스크립트 샘플 - 가상 하드 디스크를 사용하여 VM 만들기.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/09/2017
ms.author: douge
ms.custom: mvc
ms.openlocfilehash: 32f6794f6607fee04364d10a3f91128f169802b0
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/15/2019
ms.locfileid: "67875419"
---
# <a name="create-a-vm-with-a-virtual-hard-disk"></a>가상 하드 디스크를 사용하여 VM 만들기

이 예제는 VHD를 사용하여 가상 머신을 만듭니다.
리소스 그룹, 저장소 계정 및 컨테이너를 만들고 VHD를 컨테이너로 업로드하여 VM을 만듭니다.
VM에 액세스할 수 있도록 SSH 공개 키를 사용자 공개 키로 바꿉니다.

부팅 가능 VHD가 필요합니다. 스크립트는 `~/sample.vhd`를 검색합니다.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>샘플 스크립트

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-vhd/create-vm-vhd.sh "Create VM using a VHD")]

## <a name="clean-up-deployment"></a>배포 정리 

다음 명령을 실행하여 리소스 그룹, VM 및 모든 관련된 리소스를 제거할 수 있습니다.

```azurecli-interactive 
az group delete -n az-cli-vhd
```

## <a name="script-explanation"></a>스크립트 설명

이 스크립트는 다음 명령을 사용하여 리소스 그룹, 가상 머신, 가용성 집합, 부하 분산 장치 및 모든 관련된 리소스를 만듭니다. 테이블에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | 모든 리소스가 저장되는 리소스 그룹을 만듭니다. |
| [az storage account list](https://docs.microsoft.com/cli/azure/storage/account) | 저장소 계정을 나열합니다. |
| [az storage account check-name](https://docs.microsoft.com/cli/azure/storage/account) | 저장소 계정 이름이 유효하고 이미 존재하는 계정인지 확인합니다. |
| [az storage account keys list](https://docs.microsoft.com/cli/azure/storage/account/keys) | 저장소 계정의 키를 나열합니다. |
| [az storage blob exists](https://docs.microsoft.com/cli/azure/storage/blob) | Blob이 있는지 확인합니다. |
| [az storage container create](https://docs.microsoft.com/cli/azure/storage/container) | 저장소 계정으로 컨테이너를 만듭니다. |
| [az storage blob upload](https://docs.microsoft.com/cli/azure/storage/blob) | VHD를 업로드하여 컨테이너에서 Blob을 만듭니다. |
| [az vm list](https://docs.microsoft.com/cli/azure/vm) | `--query`와 함께 사용하여 VM 이름이 사용 중인지 확인합니다. | 
| [az vm create](https://docs.microsoft.com/cli/azure/vm/availability-set) | 가상 머신을 만듭니다. |
| [az vm list-ip-addresses](https://docs.microsoft.com/cli/azure/vm#az-vm-list-ip-addresses) | 만들어진 VM의 IP 주소를 가져옵니다. |

## <a name="next-steps"></a>다음 단계

Azure CLI에 대한 자세한 내용은 [Azure CLI 설명서](https://docs.microsoft.com/cli/azure)를 참조하세요.

추가 가상 머신 CLI 스크립트 샘플은 [Azure Linux VM 설명서](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)에서 확인할 수 있습니다.
