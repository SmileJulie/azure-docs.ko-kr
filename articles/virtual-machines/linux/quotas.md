---
title: Azure에 대한 vCPU 할당량 | Microsoft Docs
description: Azure에 대한 vCPU 할당량에 대해 알아봅니다.
keywords: ''
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/31/2018
ms.author: cynthn
ms.openlocfilehash: 40fd91ed39731b95cf01a627e3178ab6cfaaa7d3
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67670997"
---
# <a name="virtual-machine-vcpu-quotas"></a>가상 머신 vCPU 할당량

가상 머신 및 가상 머신 확장 집합에 대한 vCPU 할당량은 각 지역의 각 구독에 대해 두 가지 계층으로 정렬됩니다. 첫 번째 계층은 지역별 총 vCPU이며 두 번째 계층은 D 시리즈 vCPU와 같은 다양한 VM 크기 제품군 코어입니다. 새 VM이 배포될 때마다 VM에 대한 vCPU는 VM 크기 제품군 또는 지역별 총 vCPU 할당량에 대한 vCPU 할당량을 초과하지 않아야 합니다. 이러한 할당량 중 하나가 초과되면 VM 배포가 허용되지 않습니다. 지역에서 전체 가상 머신의 수에 대한 할당량도 있습니다. 이러한 각 할당량의 세부 정보는 [Azure Portal](https://portal.azure.com)에서 **구독** 페이지의 **사용량 + 할당량** 섹션에서 볼 수 있습니다. 또는 Azure CLI를 사용하여 값에 대해 쿼리할 수 있습니다.


## <a name="check-usage"></a>사용량 확인

[az vm list-usage](/cli/azure/vm)를 사용하여 할당량 사용량을 확인할 수 있습니다.

```azurecli-interactive
az vm list-usage --location "East US" -o table
```

출력은 다음과 비슷합니다.


```
Name                                CurrentValue    Limit
--------------------------------  --------------  -------
Availability Sets                              0     2000
Total Regional vCPUs                          29      100
Virtual Machines                               7    10000
Virtual Machine Scale Sets                     0     2000
Standard DSv3 Family vCPUs                     8      100
Standard DSv2 Family vCPUs                     3      100
Standard Dv3 Family vCPUs                      2      100
Standard D Family vCPUs                        8      100
Standard Dv2 Family vCPUs                      8      100
Basic A Family vCPUs                           0      100
Standard A0-A7 Family vCPUs                    0      100
Standard A8-A11 Family vCPUs                   0      100
Standard DS Family vCPUs                       0      100
Standard G Family vCPUs                        0      100
Standard GS Family vCPUs                       0      100
Standard F Family vCPUs                        0      100
Standard FS Family vCPUs                       0      100
Standard Storage Managed Disks                 5    10000
Premium Storage Managed Disks                  5    10000
```

## <a name="reserved-vm-instances"></a>예약 VM 인스턴스
VM 크기 유연성 없이 단일 구독으로 범위가 설정되는 예약 VM 인스턴스는 vCPU 할당량에 새 요소를 추가합니다. 이러한 값은 구독에 배포할 수 있어야 하는 언급된 크기의 인스턴스 수를 설명합니다. 해당 할당량이 Azure Reservations를 구독에 배포할 수 있도록 예약되도록 할당량 시스템에서 자리 표시자로 작동합니다. 예를 들어 특정 구독에 10개의 Standard_D1 예약이 있는 경우 Standard_D1 예약에 대한 사용량 한도는 10이 됩니다. 이렇게 하면 Azure에서 Standard_D1 인스턴스에 사용될 지역별 총 vCPU 할당량에서 항상 최소 10개의 vCPU를 사용할 수 있고 Standard_D1 인스턴스에 사용될 표준 D 제품군 vCPU 할당량에서 최소 10개의 vCPU를 사용할 수 있도록 보장합니다.

단일 구독 RI를 구매하는 데 할당량 증가가 필요한 경우 구독에서 [할당량 증가를 요청](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request)할 수 있습니다.

## <a name="next-steps"></a>다음 단계

청구 및 할당량에 대한 자세한 내용은 [Azure 구독 및 서비스 제한, 할당량 및 제약 조건](https://docs.microsoft.com/azure/azure-subscription-service-limits?toc=/azure/billing/TOC.json)을 참조하세요.
