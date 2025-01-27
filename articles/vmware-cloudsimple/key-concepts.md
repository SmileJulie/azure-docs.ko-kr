---
title: CloudSimple 하 여 Azure VMware 솔루션을 관리 하기 위한 주요 개념
description: CloudSimple 하 여 Azure VMware 솔루션을 관리 하기 위한 주요 개념을 설명 합니다.
author: sharaths-cs
ms.author: b-shsury
ms.date: 04/24/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 6d87871fe8faaaab2e56d4a0426cd5e5f0899c8f
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67595613"
---
# <a name="key-concepts-for-administration-of-azure-vmware-solution-by-cloudsimple"></a>CloudSimple 하 여 Azure VMware 솔루션의 관리를 위한 주요 개념

CloudSimple 하 여 Azure VMware 솔루션을 관리 하려면 다음 개념을 이해를 필요 합니다.

* CloudSimple 서비스 CloudSimple-서비스에서 Azure VMware 솔루션으로 표시 됨
* CloudSimple 노드 여 CloudSimple-노드 Azure VMware 솔루션으로 표시 됨
* CloudSimple 사설 클라우드
* 네트워킹 서비스
* CloudSimple 가상 머신 CloudSimple-가상 컴퓨터에서 Azure VMware 솔루션으로 표시 됨

## <a name="cloudsimple-service"></a>CloudSimple 서비스

CloudSimple 서비스를 사용 하 여 만들고 하 솔루션과 관련 된 VMware CloudSimple 하 여 Azure portal에서 모든 리소스를 관리 합니다. 서비스를 사용 하려는 모든 지역에서 서비스 리소스를 만듭니다.

에 대 한 자세한 정보는 [CloudSimple 서비스](cloudsimple-service.md)합니다.

## <a name="cloudsimple-node"></a>CloudSimple 노드

CloudSimple 노드는 VMware ESXi 하이퍼바이저에 배포 된, 전용 베어 메탈 하이퍼 수렴 형 계산 및 저장소 호스트. 이 노드는 다음 VMware vSphere, vCenter, vSAN을 및 NSX 플랫폼에 통합 됩니다. CloudSimple 네트워킹 서비스와 edge 네트워킹 서비스를 활성화할 수도 있습니다. 각 노드 역할을 만들려면 프로 비전 할 수 있는 계산 및 저장소 용량 단위의 [CloudSimple 사설 클라우드](cloudsimple-private-cloud.md)합니다. 프로 비전 하거나 CloudSimple 서비스를 사용할 수 있는 지역에서 노드를 예약 합니다.


에 대해 자세히 알아보세요 [CloudSimple 노드](cloudsimple-node.md)합니다.

## <a name="cloudsimple-private-cloud"></a>CloudSimple 사설 클라우드

CloudSimple 사설 클라우드는 자체 관리 도메인에서 vCenter 서버에서 관리 되는 격리 된 VMware 스택 환경입니다. VMware 스택을 ESXi 호스트, vSphere, vCenter, vSAN을 및 NSX 포함 됩니다. stack 실행 전용 노드 (격리 및 전용 베어 메탈 하드웨어) 및 vCenter 및 NSX 관리자를 포함 하는 네이티브 VMware 도구를 통해 사용자가 사용 됩니다. 전용된 노드는 Azure 위치에 배포 되며 Azure에서 관리 됩니다. 각 사설 클라우드에 분할 하 고 Vlan 및 서브넷 및 방화벽 테이블과 같은 네트워킹 서비스를 사용 하 여 보호할 수 있습니다. 온-프레미스 환경과 Azure 네트워크에 대 한 연결을 사용 하 여 안전한 개인 VPN을 Azure ExpressRoute 연결이 생성 됩니다.

에 대해 자세히 알아보세요 [CloudSimple 사설 클라우드](cloudsimple-private-cloud.md)합니다.

## <a name="service-networking"></a>네트워킹 서비스

CloudSimple 서비스 CloudSimple 서비스를 배포할 지역 마다 네트워크를 제공 합니다. 네트워크가 라우팅을 기본적으로 사용할 수 있는 단일 TCP 계층 3 주소 공간입니다. 모든 사설 클라우드 및이 지역에서 만든 서브넷 추가 구성 없이 서로 통신 합니다. Vlan을 사용 하 여 vcenter 분산된 포트 그룹을 만듭니다. 구성 하 고 사설 클라우드에서 워크 로드 리소스를 보호 하려면 다음과 같은 네트워크 기능을 사용할 수 있습니다.

* [Vlan 및 서브넷](cloudsimple-vlans-subnets.md)
* [방화벽 테이블](cloudsimple-firewall-tables.md)
* [VPN gateway](cloudsimple-vpn-gateways.md)
* [공용 IP](cloudsimple-public-ip-address.md)
* [Azure 네트워크에 연결](cloudsimple-azure-network-connection.md)

## <a name="cloudsimple-virtual-machine"></a>CloudSimple 가상 머신

CloudSimple 서비스를 사용 하 여 Azure portal에서 VMware 가상 컴퓨터를 관리할 수 있습니다. 서비스를 만든 구독에 하나 이상의 클러스터 또는 vSphere 환경에서 리소스 풀을 매핑할 수 있습니다.

다음에 대해 자세히 알아봅니다.

* [CloudSimple 가상 컴퓨터](cloudsimple-virtual-machines.md)
* [Azure 구독 매핑](https://docs.azure.cloudsimple.com/azure-subscription-mapping/)
