---
title: 부하 분산 장치에 대한 Azure Resource Manager 지원 | Microsoft Docs
description: Azure Resource Manager와 함께 부하 분산 장치용 PowerShell을 사용합니다. 부하 분산 장치에 템플릿을 사용합니다.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: allensu
ms.openlocfilehash: 839b607b7787d51151401737848a46d7b66229dd
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68275481"
---
# <a name="using-azure-resource-manager-support-with-azure-load-balancer"></a>Azure Load Balancer에 대한 Azure Resource Manager 지원 사용



Azure의 서비스용 관리 프레임워크로는 기본적으로 Azure Resource Manager가 사용됩니다. 이제 Azure Resource Manager 기반 API 및 도구를 사용하여 Azure Load Balancer를 관리할 수 있습니다.

## <a name="concepts"></a>개념

Resource Manager를 사용하는 경우 Azure Load Balancer에 다음과 같은 자식 리소스가 포함됩니다.

* 프런트 엔드 IP 구성 - 부하 분산 장치는 VIP(가상 IP)라고도 하는 프런트 엔드 IP 주소를 하나 이상 포함할 수 있습니다. 이러한 IP 주소는 트래픽에 대한 수신으로 사용됩니다.
* 백 엔드 주소 풀 - 부하가 분산될 가상 머신 NIC(네트워크 인터페이스 카드)와 연결된 IP 주소입니다.
* 부하 분산 규칙-규칙 속성은 지정 된 프런트 엔드 IP 및 포트 조합을 백 엔드 IP 주소와 포트 조합 집합에 매핑합니다. 부하 분산 장치 하나에 여러 부하 분산 규칙이 포함될 수 있습니다. 각 규칙은 Vm과 연결 된 프런트 엔드 IP와 포트 및 백 엔드 IP와 포트의 조합입니다.
* 검색 - 검색을 사용하여 VM 인스턴스의 상태를 추적할 수 있습니다. 상태 검색에 실패하면 해당 VM 인스턴스는 자동으로 회전에서 제외됩니다.
* 인바운드 NAT 규칙-프런트 엔드 IP를 통해 이동 하 여 백 엔드 IP에 분산 되는 인바운드 트래픽을 정의 하는 NAT 규칙입니다.

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a>빠른 시작 템플릿

Azure Resource Manager를 사용하면 선언적 템플릿을 통해 애플리케이션을 프로비전할 수 있습니다. 단일 템플릿에서 여러 서비스를 해당 종속 항목과 함께 배포할 수 있습니다. 동일한 템플릿을 사용하여 애플리케이션 수명 주기의 각 단계 중에 애플리케이션을 반복해서 배포합니다.

템플릿은 Virtual Machines, Virtual Network, 가용성 집합, NIC(네트워크 인터페이스), Storage 계정, 부하 분산 장치, 네트워크 보안 그룹 및 공용 IP에 대한 정의를 포함할 수 있습니다. 템플릿을 사용하면 복잡한 애플리케이션에 필요한 모든 항목을 만들 수 있습니다. 버전 제어 및 협업을 위해 템플릿 파일을 콘텐츠 관리 시스템에 체크 인할 수 있습니다.

[템플릿에 대한 자세한 정보](../azure-resource-manager/resource-manager-template-walkthrough.md)

[네트워크 리소스에 대한 자세한 정보](../networking/networking-overview.md)

Azure Load Balancer를 사용하는 빠른 시작 템플릿은 커뮤니티 생성 템플릿 집합을 호스트하는 [GitHub 리포지토리](https://github.com/Azure/azure-quickstart-templates)를 참조하세요.

템플릿의 예:

* [부하 분산 장치의 2개 VM 및 부하 분산 규칙](https://go.microsoft.com/fwlink/?LinkId=544799)
* [내부 부하 분산 장치를 포함하는 VNET의 2개 VM 및 부하 분산 장치 규칙](https://go.microsoft.com/fwlink/?LinkId=544800)
* [부하 분산 장치의 2개 VM 및 LB에서 NAT 규칙 구성](https://go.microsoft.com/fwlink/?LinkId=544801)

## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a>PowerShell 또는 CLI를 사용하여 Azure 부하 분산 장치 설정

Azure Resource Manager cmdlet, 명령줄 도구 및 REST API 시작

* [Azure 네트워킹 Cmdlet](https://docs.microsoft.com/powershell/module/az.network#networking) 을 사용하여 부하 분산 장치를 만들 수 있습니다.
* [Azure Resource Manager를 사용하여 부하 분산 장치를 만드는 방법](load-balancer-get-started-ilb-arm-ps.md)
* [Azure 리소스 관리에서 Azure CLI 사용](../xplat-cli-azure-resource-manager.md)
* [부하 분산 장치 REST API](https://msdn.microsoft.com/library/azure/mt163651.aspx)

## <a name="next-steps"></a>다음 단계

[인터넷 연결 부하 분산 장치를 시작](load-balancer-get-started-internet-arm-ps.md)하고 특정 부하 분산 장치 네트워크 트래픽 동작에 대한 [배포 모드](load-balancer-distribution-mode.md) 유형을 구성할 수도 있습니다.

[부하 분산 장치의 유휴 TCP 시간 제한 설정](load-balancer-tcp-idle-timeout.md)을 관리하는 방법을 파악합니다. 애플리케이션이 부하 분산 장치를 통해 서버에 대한 연결 상태를 유지해야 하는 경우에는 이 내용을 숙지하고 있어야 합니다.
