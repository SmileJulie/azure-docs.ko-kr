---
title: 포함 파일
description: 포함 파일
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: a601b0c40f84832101e97a7abf7dd7418a0a5c69
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673438"
---
다음 단계에 따라 Resource Manager 배포 모델과 Azure Portal을 사용하여 VNet을 만들 수 있습니다. 가상 네트워크에 대한 자세한 내용은 [Virtual Network 개요](../articles/virtual-network/virtual-networks-overview.md)를 참조하세요.

>[!NOTE]
>VNet을 온-프레미스 위치에 연결하려면 온-프레미스 네트워크 관리자와 협력하여 이 가상 네트워크에 특별히 사용할 수 있는 IP 주소 범위를 구성합니다. VPN 연결의 양쪽 모두에 중복 주소 범위가 있는 경우 트래픽이 예기치 않은 방식으로 라우팅됩니다. 또한 이 VNet을 다른 VNet에 연결하려면 주소 공간이 다른 VNet과 겹치면 안됩니다. 이에 따라 네트워크 구성을 적절히 계획합니다.
>
>

1. [Azure Portal](https://portal.azure.com)에 로그인하고, **리소스 만들기**를 선택합니다. **새로 만들기** 페이지가 열립니다.

2. **Marketplace 검색** 필드에 *가상 네트워크*를 입력하고 반환된 목록에서 **가상 네트워크**를 선택합니다. **가상 네트워크** 페이지가 열립니다.

   ![Virtual Network 리소스 찾기 페이지](./media/vpn-gateway-basic-vnet-rm-portal-include/newvnetportal700.png "가상 네트워크 리소스 찾기 페이지")

3. 페이지 아래쪽 근처에 있는 **배포 모델 선택** 목록에서 **Resource Manager**를 선택한 다음, **만들기**를 클릭합니다. **가상 네트워크 만들기** 페이지가 열립니다.

   ![가상 네트워크 만들기 페이지](./media/vpn-gateway-basic-vnet-rm-portal-include/vnet.png "가상 네트워크 만들기 페이지")

4. **가상 네트워크 만들기** 페이지에서 VNet 설정을 구성합니다. 필드를 채울 때 필드에 입력한 문자의 유효성이 검사되면 빨간색 느낌표가 녹색 확인 표시가 됩니다. 일부 값은 자동으로 채워지며, 사용자 고유의 값으로 바꿀 수 있습니다.

   - **이름**: 가상 네트워크에 대한 이름을 입력합니다.

   - **주소 공간**: 주소 공간을 입력합니다. 추가할 주소 공간이 여러 개 있으면 첫 번째 주소 공간을 입력합니다. 다른 주소 공간은 VNet을 만든 후 나중에 추가할 수 있습니다.

   - **구독**: 나열된 구독이 올바른지 확인합니다. 드롭다운을 사용하여 구독을 변경할 수 있습니다.

   - **리소스 그룹**: 기존 리소스 그룹을 선택하거나 새 리소스 그룹에 대한 이름을 입력하여 새로 만듭니다. 새 그룹을 만드는 경우 계획된 구성 값에 따라 리소스 그룹의 이름을 지정합니다. 리소스 그룹에 대한 자세한 내용은 [Azure Resource Manager 개요](../articles/azure-resource-manager/resource-group-overview.md#resource-groups)를 참조하세요.

   - **위치**: VNet에 대한 위치를 선택합니다. 이 위치는 이 VNet에 배포하는 리소스가 상주하는 위치를 결정합니다.

   - **서브넷**: 서브넷 **이름** 및 서브넷 **주소 범위**를 추가합니다. 서브넷은 VNet을 만든 후 나중에 추가할 수 있습니다. 
     
5. **만들기**를 선택합니다.
