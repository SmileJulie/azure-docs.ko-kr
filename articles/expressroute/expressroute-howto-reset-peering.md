---
title: Azure ExpressRoute 피어링 다시 설정 | Microsoft Docs
description: ExpressRoute 회로 피어링을 사용하거나 사용하지 않도록 설정하는 방법
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: conceptual
ms.date: 08/15/2018
ms.author: charwen
ms.openlocfilehash: b8c9bc1944e9ed0281616062a84958c953d08694
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/17/2018
ms.locfileid: "40180667"
---
# <a name="reset-expressroute-peerings"></a>ExpressRoute 피어링 다시 설정

이 문서에서는 PowerShell을 사용하여 ExpressRoute 회로의 피어링을 사용하거나 사용하지 않도록 설정하는 방법을 설명합니다. 피어링을 사용하지 않도록 설정하는 경우 ExpressRoute 회로의 기본 연결 및 보조 연결 둘 다에서 BGP 세션이 종료됩니다. 이 피어링을 통한 Microsoft와의 연결이 손실됩니다. 피어링을 사용하도록 설정하는 경우 ExpressRoute 회로의 기본 연결 및 보조 연결 둘 다에서 BGP 세션이 설정됩니다. 이 피어링을 통한 Microsoft와의 연결이 다시 설정됩니다. ExpressRoute 회로에서 Microsoft 피어링 및 Azure 개인 피어링을 별도로 사용 및 사용하지 않도록 설정할 수 있습니다. ExpressRoute 회로에서 피어링을 처음 구성할 때는 피어링이 기본적으로 사용되도록 설정됩니다. 

ExpressRoute 피어링을 다시 설정하는 것이 도움이 되는 몇 가지 시나리오가 있습니다.
* 재해 복구 디자인 및 구현을 테스트합니다. 예를 들어 두 개의 ExpressRoute 회로가 있습니다. 한 회로의 피어링은 사용하지 않도록 설정하고 네트워크 트래픽을 다른 회로로 강제로 장애 조치(Failover)할 수 있습니다.
* ExpressRoute 회로의 Azure 개인 피어링에서 BFD(양방향 전달 검색)를 사용하도록 설정합니다. BFD는 ExpressRoute 회로가 2018년 8월 1일 이후에 만들어진 경우 기본적으로 사용되도록 설정됩니다. 그 이전에 회로를 만들었으면 BFD가 사용되도록 설정되지 않습니다. 피어링을 사용하지 않도록 설정했다가 다시 사용하도록 설정하여 BFD를 사용하도록 설정할 수 있습니다. BFD는 Azure 개인 피어링에서만 지원됩니다.


## <a name="reset-a-peering"></a>피어링 다시 설정

1. 최신 버전의 Azure Resource Manager PowerShell cmdlet을 설치합니다. 자세한 내용은 [Azure PowerShell 설치 및 구성](/powershell/azure/install-azurerm-ps)을 참조하세요.

2. 상승된 권한으로 PowerShell 콘솔을 열고 계정에 연결합니다. 연결에 도움이 되도록 다음 예제를 사용합니다.

  ```powershell
  Connect-AzureRmAccount
  ```
3. Azure 구독이 여러 개인 경우 계정의 구독을 확인합니다.

  ```powershell
  Get-AzureRmSubscription
  ```
4. 사용할 구독을 지정합니다.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```
5. ExpressRoute 회로를 검색하려면 다음 명령을 실행합니다.

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```
6. 사용하거나 사용하지 않도록 설정하려는 피어링을 식별합니다. *피어링*은 배열입니다. 다음 예제에서 피어링[0]은 Azure 개인 피어링이고 피어링[1]은 Microsoft 피어 링입니다.

  ```powershell
Name                             : ExpressRouteARMCircuit
ResourceGroupName                : ExpressRouteResourceGroup
Location                         : westus
Id                               : /subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
Etag                             : W/"cd011bef-dc79-49eb-b4c6-81fb6ea5d178"
ProvisioningState                : Succeeded
Sku                              : {
                                     "Name": "Standard_MeteredData",
                                     "Tier": "Standard",
                                     "Family": "MeteredData"
                                   }
CircuitProvisioningState         : Enabled
ServiceProviderProvisioningState : Provisioned
ServiceProviderNotes             :
ServiceProviderProperties        : {
                                     "ServiceProviderName": "Coresite",
                                     "PeeringLocation": "Los Angeles",
                                     "BandwidthInMbps": 50
                                   }
ServiceKey                       : ########-####-####-####-############
Peerings                         : [
                                     {
                                       "Name": "AzurePrivatePeering",
                                       "Etag": "W/\"cd011bef-dc79-49eb-b4c6-81fb6ea5d178\"",
                                       "Id": "/subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit/peerings/AzurePrivatePeering",
                                       "PeeringType": "AzurePrivatePeering",
                                       "State": "Enabled",
                                       "AzureASN": 12076,
                                       "PeerASN": 123,
                                       "PrimaryPeerAddressPrefix": "10.0.0.0/30",
                                       "SecondaryPeerAddressPrefix": "10.0.0.4/30",
                                       "PrimaryAzurePort": "",
                                       "SecondaryAzurePort": "",
                                       "VlanId": 789,
                                       "MicrosoftPeeringConfig": {
                                         "AdvertisedPublicPrefixes": [],
                                         "AdvertisedCommunities": [],
                                         "AdvertisedPublicPrefixesState": "NotConfigured",
                                         "CustomerASN": 0,
                                         "LegacyMode": 0,
                                         "RoutingRegistryName": "NONE"
                                       },
                                       "ProvisioningState": "Succeeded",
                                       "GatewayManagerEtag": "",
                                       "LastModifiedBy": "Customer",
                                       "Connections": []
                                     },
                                     {
                                       "Name": "MicrosoftPeering",
                                       "Etag": "W/\"cd011bef-dc79-49eb-b4c6-81fb6ea5d178\"",
                                       "Id": "/subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit/peerings/MicrosoftPeering",
                                       "PeeringType": "MicrosoftPeering",
                                       "State": "Enabled",
                                       "AzureASN": 12076,
                                       "PeerASN": 123,
                                       "PrimaryPeerAddressPrefix": "3.0.0.0/30",
                                       "SecondaryPeerAddressPrefix": "3.0.0.4/30",
                                       "PrimaryAzurePort": "",
                                       "SecondaryAzurePort": "",
                                       "VlanId": 345,
                                       "MicrosoftPeeringConfig": {
                                         "AdvertisedPublicPrefixes": [
                                           "3.0.0.3/32"
                                         ],
                                         "AdvertisedCommunities": [],
                                         "AdvertisedPublicPrefixesState": "ValidationNeeded",
                                         "CustomerASN": 0,
                                         "LegacyMode": 0,
                                         "RoutingRegistryName": "NONE"
                                       },
                                       "ProvisioningState": "Succeeded",
                                       "GatewayManagerEtag": "",
                                       "LastModifiedBy": "Customer",
                                       "Connections": []
                                     }
                                   ]
Authorizations                   : []
AllowClassicOperations           : False
GatewayManagerEtag               :
  ```
7. 피어링의 상태를 변경하려면 다음 명령을 실행합니다.

  ```powershell
  $ckt.Peerings[0].State = "Disabled"
  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```
피어링은 설정한 상태를 유지합니다. 

## <a name="next-steps"></a>다음 단계
ExpressRoute 문제 해결에 도움이 필요한 경우 다음 문서를 확인합니다.
* [ExpressRoute 연결 확인](expressroute-troubleshooting-expressroute-overview.md)
* [네트워크 성능 문제 해결](expressroute-troubleshooting-network-performance.md)