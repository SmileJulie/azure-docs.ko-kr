---
title: 영역별 프런트 엔드가 있는 Load Balancer 만들기 - Azure PowerShell
titlesuffix: Azure Load Balancer
description: Azure PowerShell을 사용하여 영역별 프런트 엔드가 있는 표준 Load Balancer를 만드는 방법을 알아봅니다.
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2019
ms.author: allensu
ms.openlocfilehash: a2c637ddbf5b7f5883016afe4a6d58ad9752cfc5
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68275127"
---
#  <a name="create-a-standard-load-balancer-with-zonal-frontend-using-azure-powershell"></a>Azure PowerShell을 사용하여 영역별 프런트 엔드가 있는 표준 Load Balancer 만들기

이 문서에서는 공용 IP 표준 주소를 사용하여 영역별 프런트 엔드가 있는 공용 [표준 Load Balancer](https://aka.ms/azureloadbalancerstandard)를 만드는 단계를 안내합니다. Standard Load Balancer에서 가용성 영역이 작동하는 방법에 대한 내용은 [Standard Load Balancer 및 가용성 영역](load-balancer-standard-availability-zones.md)을 참조하세요. 

Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)을 만듭니다.

> [!NOTE]
> Azure 리소스, 지역 및 VM 크기 제품군을 선택하는 데 가용성 영역에 대한 지원을 사용할 수 있습니다. 시작하는 방법에 대한 자세한 내용 및 가용성 영역을 사용해 볼 수 있는 Azure 리소스, 지역 및 VM 크기 제품군은 [가용성 영역 개요](https://docs.microsoft.com/azure/availability-zones/az-overview)를 참조하세요. 지원을 위해 [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones)에 연결하거나 [Azure 지원 티켓을 열](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json) 수 있습니다.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="log-in-to-azure"></a>Azure에 로그인

`Connect-AzAccount` 명령으로 Azure 구독에 로그인하고 화면의 지시를 따릅니다.

```azurepowershell-interactive
Connect-AzAccount
```

## <a name="create-resource-group"></a>리소스 그룹 만들기

다음 명령을 사용하여 리소스 그룹을 만듭니다.

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroupZLB -Location westeurope
```

## <a name="create-a-public-ip-standard"></a>공용 IP 표준 만들기 
다음 명령을 사용하여 공용 IP 표준을 만듭니다.

```azurepowershell-interactive
$publicIp = New-AzPublicIpAddress -ResourceGroupName myResourceGroupZLB -Name 'myPublicIPZonal' `
  -Location westeurope -AllocationMethod Static -Sku Standard -zone 1
```

## <a name="create-a-front-end-ip-configuration-for-the-website"></a>웹 사이트에 대한 프런트 엔드 IP 구성 만들기

다음 명령을 사용하여 프런트 엔드 IP 구성을 만듭니다.

```azurepowershell-interactive
$feip = New-AzLoadBalancerFrontendIpConfig -Name 'myFrontEnd' -PublicIpAddress $publicIp
```

## <a name="create-the-back-end-address-pool"></a>백 엔드 주소 풀 만들기

다음 명령을 사용하여 백 엔드 주소 풀을 만듭니다.

```azurepowershell-interactive
$bepool = New-AzLoadBalancerBackendAddressPoolConfig -Name 'myBackEndPool'
```

## <a name="create-a-load-balancer-probe-on-port-80"></a>포트 80에서 부하 분산 장치 프로브 만들기

다음 명령을 사용하여 포트 80에서 부하 분산 장치에 대한 상태 프로브를 만듭니다.

```azurepowershell-interactive
$probe = New-AzLoadBalancerProbeConfig -Name 'myHealthProbe' -Protocol Http -Port 80 `
  -RequestPath / -IntervalInSeconds 360 -ProbeCount 5
```

## <a name="create-a-load-balancer-rule"></a>부하 분산 장치 규칙 만들기
 다음 명령을 사용하여 부하 분산 장치 규칙을 만듭니다.

```azurepowershell-interactive
   $rule = New-AzLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $feip -BackendAddressPool  $bepool -Probe $probe -Protocol Tcp -FrontendPort 80 -BackendPort 80
```

## <a name="create-a-load-balancer"></a>부하 분산 장치 만들기
다음 명령을 사용하여 표준 Load Balancer를 만듭니다.

```azurepowershell-interactive
$lb = New-AzLoadBalancer -ResourceGroupName myResourceGroupZLB -Name 'MyLoadBalancer' -Location westeurope `
  -FrontendIpConfiguration $feip -BackendAddressPool $bepool `
  -Probe $probe -LoadBalancingRule $rule -Sku Standard
```

## <a name="next-steps"></a>다음 단계
- [Standard Load Balancer 및 가용성 영역](load-balancer-standard-availability-zones.md)에 대해 자세히 알아봅니다.