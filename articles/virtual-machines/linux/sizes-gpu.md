---
title: 가속 azure Linux VM 크기-compute | Microsoft Docs
description: Azure의 Linux 가상 머신에 사용할 수 있는 다양한 GPU 최적화 크기를 나열합니다. 이 시리즈의 크기에 대한 저장소 처리량 및 네트워크 대역폭뿐만 아니라 vCPU, 데이터 디스크 및 NIC의 수에 대한 정보를 제공합니다.
services: virtual-machines-linux
documentationcenter: ''
author: jonbeck7
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/11/2019
ms.author: jonbeck
ms.openlocfilehash: 64cbcd375840d78916810abf9ccb8478ef9a1359
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67708842"
---
# <a name="gpu-optimized-virtual-machine-sizes"></a>GPU 최적화 가상 머신 크기

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-distributions-and-drivers"></a>지원되는 배포판 및 버전

Linux를 실행하는 Azure N 시리즈 VM의 GPU 기능을 최대한 활용하려면 NVIDIA GPU 드라이버를 설치해야 합니다. [NVIDIA GPU 드라이버 확장](../extensions/hpccompute-gpu-linux.md)은 N 시리즈 VM에 적절한 NVIDIA CUDA 또는 GRID 드라이버를 설치합니다. Azure CLI 또는 Azure Resource Manager 템플릿과 같은 도구나 Azure Portal을 사용하여 확장을 설치 또는 관리합니다. 지원되는 배포판 및 배포 단계는 [NVIDIA GPU 드라이버 확장 설명서](../extensions/hpccompute-gpu-linux.md)를 참조하세요. VM 확장에 대한 일반적인 내용은 [Azure 가상 머신 확장 및 기능](../extensions/overview.md)을 참조하세요.

NVIDIA GPU 드라이버를 수동으로 설치하려는 경우 지원되는 배포, 드라이버 및 설치 및 확인 단계는 [Linux에 대한 N 시리즈 GPU 드라이버 설치](n-series-driver-setup.md)를 참조하세요.


[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

* Ubuntu NC VM에 `Nouveau` 드라이버를 사용하는 X 서버 또는 기타 시스템을 설치하지 않습니다. NVIDIA GPU 드라이버를 설치하기 전에 `Nouveau` 드라이버를 사용하지 않도록 설정해야 합니다.  

## <a name="other-sizes"></a>기타 크기
- [범용](sizes-general.md)
- [컴퓨팅 최적화](sizes-compute.md)
- [메모리에 최적화](sizes-memory.md)
- [Storage에 최적화](sizes-storage.md)
- [고성능 계산](sizes-hpc.md)
- [이전 세대](sizes-previous-gen.md)

## <a name="next-steps"></a>다음 단계
[ACU(Azure 컴퓨팅 단위)](acu.md)가 Azure SKU 간의 Compute 성능을 비교하는 데 어떻게 도움을 줄 수 있는지 알아봅니다.