---
title: Service Fabric 클러스터 용량 계획 | Microsoft Docs
description: Service Fabric 클러스터 용량 계획 고려 사항입니다. 노드 유형, 작업, 내구성 및 안정성 계층
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: chackdan
editor: ''
ms.assetid: 4c584f4a-cb1f-400c-b61f-1f797f11c982
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/09/2019
ms.author: chackdan
ms.openlocfilehash: 6b11a3ba4fbffe1d35b590f2e5c47f19b6fb028c
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67718123"
---
# <a name="service-fabric-cluster-capacity-planning-considerations"></a>서비스 패브릭 클러스터 용량 계획 고려 사항
프로덕션 배포의 경우 용량 계획은 중요한 단계입니다. 다음은 해당 프로세스의 일부로 고려해야 하는 항목 중 일부입니다.

* 클러스터에서 시작해야 하는 노드 형식의 수입니다.
* 각 노드 유형의 속성(크기, 기본, 인터넷 연결, VM의 수 등)
* 클러스터의 안정성 및 지속성 특성

> [!NOTE]
> 계획 중에는 모든 **허용되지 않음** 업그레이드 정책 값을 최소로 검토해야 합니다. 이는 값을 적절하게 설정하고 나중에 시스템 구성 설정을 변경할 수 없으므로 클러스터 소실을 완화하기 위한 것입니다. 
> 

이러한 각 항목을 간단히 검토해보겠습니다.

## <a name="the-number-of-node-types-your-cluster-needs-to-start-out-with"></a>클러스터에서 시작해야 하는 노드 형식의 수입니다.
먼저, 만드는 클러스터를 어디에 사용할지 결정해야 합니다.  이 클러스터에 어떤 종류의 애플리케이션을 배포할 계획인가요? 클러스터의 용도가 명확하지 않는 경우 상황에 맞는 용량 프로세스를 입력하도록 준비하지 앉아야 합니다.

클러스터에서 시작해야 하는 노드 유형의 수를 설정합니다.  각 노드 유형은 가상 머신 확장 집합에 매핑됩니다. 각 노드 형식은 독립적으로 확장 또는 축소되고, 다른 포트의 집합을 열며 다른 용량 메트릭을 가질 수 있습니다. 따라서 노드 유형의 수를 결정할 때 기본적으로 다음 사항을 고려합니다.

* 애플리케이션이 여러 서비스를 보유하고 해당 서비스 중 일부를 공용 또는 인터넷에 연결해야 하나요? 일반적인 애플리케이션은 클라이언트에서 입력을 수신하는 프런트 엔드 게이트웨이 서비스 및 프런트 엔드 서비스와 통신하는 하나 이상의 백 엔드 서비스를 포함합니다. 따라서 이 경우에 두 개 이상의 노드 유형을 보유하게 됩니다.
* 서비스(애플리케이션을 구성함)에는 더 유용한 RAM 또는 더 높은 CPU 사용률과 같은 서로 다른 인프라가 필요한가요? 예를 들어 배포하려는 애플리케이션에 프런트 엔드 서비스 및 백 엔드 서비스가 포함되어 있다고 가정합니다. 프런트 엔드 서비스는 인터넷에 열린 포트가 있는 작은 VM(D2와 같은 VM 크기)에서 실행할 수 있습니다.  하지만 백 엔드 서비스는 계산 집약적이며 인터넷에 연결되지 않은 비교적 큰 VM(D4, D6, D15과 같은 VM 크기로)에서 실행됩니다.
  
  이 예제에서는 하나의 노드 유형에 모든 서비스를 배치할 수 있지만 두 노드 유형을 사용하여 클러스터에 배치하는 것이 좋습니다.  그러면 각 노드 유형이 인터넷 연결 또는 VM 크기와 같은 고유 속성을 가질 수 있습니다. VM의 수를 독립적으로 확장할 수도 있습니다.  
* 미래를 예측할 수 없으므로 알고 있는 사실을 바탕으로 진행하고 애플리케이션 시작 시 팔요한 노드 형식의 수를 선택합니다. 나중에 노드 유형을 추가하거나 제거할 수 있습니다. Service Fabric 클러스터에는 하나 이상의 노드 유형이 있어야 합니다.

## <a name="the-properties-of-each-node-type"></a>각 노드 유형의 속성
**노드 유형**은 Cloud Services의 역할과 동등한 것으로 간주될 수 있습니다. 노드 유형에서 VM 크기, VM 수 및 VM 속성을 정의합니다. Service Fabric 클러스터에 정의된 모든 노드 형식은 [가상 머신 확장 집합](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview)에 매핑됩니다.  
각 노드 형식은 고유한 확장 집합으로, 독립적으로 확장 또는 축소되고, 다른 포트의 집합을 열며 다른 용량 메트릭을 가질 수 있습니다. 노드 형식과 가상 머신 확장 집합 간의 관계, 인스턴스 중 하나로 RDP하는 방법, 새 포트를 여는 방법 등에 대한 자세한 내용은 [Service Fabric 클러스터 노드 형식](service-fabric-cluster-nodetypes.md)을 참조하세요.

Service Fabric 클러스터는 둘 이상의 노드 형식으로 구성될 수 있습니다. 이 경우 클러스터는 하나의 주 노드 형식과 하나 이상의 주 노드가 아닌 노드 형식으로 구성됩니다.

단일 노드 형식은 SF 애플리케이션의 가상 머신 확장 집합당 100개 노드 이상으로 안정적으로 확장될 수 없습니다. 100개를 초과하는 노드를 안정적으로 확보하려면 가상 머신 확장 집합을 추가해야 합니다.

### <a name="primary-node-type"></a>주 노드 유형

Service Fabric 시스템 서비스(예: 클러스터 관리자 서비스 또는 이미지 저장소 서비스)는 주 노드 형식에 배치됩니다. 

![두 가지 노드 유형이 있는 클러스터의 스크린샷][SystemServices]

* 주 노드 유형에 대한 **최소 VM 크기**는 선택한 **내구성 계층**에 따라 결정됩니다. 기본 내구성 계층은 Bronze입니다. 자세한 내용은 [클러스터의 내구성 특성](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster)을 참조하세요.  
* 주 노드 유형에 대한 **최소 VM 수**는 선택한 **안정성 계층**에 따라 결정됩니다. 기본 안정성 계층은 Silver입니다. 자세한 내용은 [클러스터의 안정성 특성](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-reliability-characteristics-of-the-cluster)을 참조하세요.  

Azure Resource Manager 템플릿에서 주 노드 형식은 [노드 형식 정의](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/clusters#nodetypedescription-object) 아래의 `isPrimary` 특성으로 구성됩니다.

### <a name="non-primary-node-type"></a>주가 아닌 노드 유형

여러 노드 형식이 있는 클러스터에는 하나의 주 노드 형식이 있고 나머지는 주 노드가 아닌 노드 형식입니다.

* 주 노드가 아닌 노드 형식에 대한 **최소 VM 크기**는 선택한 **내구성 계층**에 따라 결정됩니다. 기본 내구성 계층은 Bronze입니다. 자세한 내용은 [클러스터의 내구성 특성](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster)을 참조하세요.  
* 주 노드가 아닌 노드 형식의 **최소 VM 수**는 1입니다. 그러나 이 노드 유형에서 실행하려는 애플리케이션/서비스의 복제본 수에 따라 이 수를 선택해야 합니다. 클러스터를 배포한 후에 노드 유형에서 VM의 수를 늘릴 수 있습니다.

## <a name="the-durability-characteristics-of-the-cluster"></a>클러스터의 지속성 특성
지속성 계층은 기본 Azure 인프라와 함께 VM에 있는 권한을 이 시스템에 표시하는 데 사용됩니다. 주 노드 유형에서 이 권한을 사용하면 Service Fabric이 시스템 서비스 및 상태 저장 서비스에 대한 쿼럼 요구 사항에 영향을 주는 VM 수준 인프라 요청(VM 다시 부팅, VM 이미지로 다시 설치, 또는 VM 마이그레이션 등)을 일시 중지할 수 있습니다. 주가 아닌 노드 형식에서 이 권한을 사용하면 Service Fabric이 상태 저장 서비스에 대한 쿼럼 요구 사항에 영향을 주는 VM 수준 인프라 요청(예: VM 다시 부팅, VM 이미지로 다시 설치, VM 마이그레이션)을 일시 중지할 수 있습니다.

| 내구성 계층  | 필요한 최소 VM 수 | 지원되는 VM SKU                                                                  | 가상 머신 확장 집합의 업데이트                               | Azure에서 시작되는 업데이트 및 유지 관리                                                              | 
| ---------------- |  ----------------------------  | ---------------------------------------------------------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| Gold             | 5                              | 단일 고객 전용인 전체 노드 SKU(예: L32s, GS5, G5, DS15_v2, D15_v2) | Service Fabric 클러스터에서 승인될 때까지 지연될 수 있음 | 복제본이 이전 오류에서 복구될 수 있는 추가 시간을 허용하기 위해 UD당 2시간 동안 일시 중지될 수 있음 |
| Silver           | 5                              | 단일 코어 이상을 Vm 50gb 이상 로컬 SSD                      | Service Fabric 클러스터에서 승인될 때까지 지연될 수 있음 | 오랜 기간 동안 지연될 수 없음                                                    |
| Bronze           | 1                              | 최소 50GB의 로컬 SSD 사용 하 여 Vm                                              | Service Fabric 클러스터에 의해 지연되지 않음           | 오랜 기간 동안 지연될 수 없음                                                    |

> [!WARNING]
> Bronze 내구성으로 실행되는 노드 형식은 _권한 없음_이 됩니다. 즉, 상태 비저장 워크로드에 영향을 주는 인프라 작업은 중지되거나 지연되지 않으며, 이는 워크로드에 영향을 줄 수 있습니다. 상태 비저장 워크로드만 실행하는 노드 형식에는 Bronze만 사용합니다. 프로덕션 워크로드의 경우 Silver 이상을 실행하는 것이 좋습니다. 
> 
> 내구성 수준에 관계 없이 VM 확장 집합의 [할당 취소](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/deallocate) 작업은 클러스터를 삭제합니다.

**Silver 또는 Gold 내구성 수준 사용 시 장점**
 
- 규모 감축 작업에 필요한 단계 수가 줄어듭니다(즉, 노드 비활성화 및 Remove-ServiceFabricNodeState가 자동으로 호출됨).
- 사용자가 시작한 원본 위치 VM SKU 변경 작업 또는 Azure 인프라 작업으로 인해 데이터 손실의 위험이 줄어듭니다.

**Silver 또는 Gold 내구성 수준 사용 시 단점**
 
- 가상 머신 확장 집합 및 기타 관련 Azure 리소스에 대한 배포가 지연되거나, 시간이 초과되거나, 클러스터 또는 인프라 수준에서 발생한 문제로 인해 완전히 차단될 수 있습니다. 
- Azure 인프라 작업 중에 자동으로 수행되는 노드 비활성화로 인해 [복제본 수명 주기 이벤트](service-fabric-reliable-services-lifecycle.md)(예: 기본 스왑) 수가 증가합니다.
- Azure 플랫폼 소프트웨어 업데이트 또는 하드웨어 유지 관리 작업이 발생하는 기간 동안 노드를 서비스 불가능 상태로 유지합니다. 이러한 작업 중에는 노드 상태가 비활성화 중/사용 안 함으로 표시될 수 있습니다. 이로 인해 클러스터 용량이 일시적으로 감소하지만 클러스터 또는 애플리케이션의 가용성에는 영향을 주지 않아야 합니다.

### <a name="recommendations-for-when-to-use-silver-or-gold-durability-levels"></a>Silver 또는 Gold 내구성 수준을 사용해야 하는 경우에 대한 권장 사항

규모 감축(VM 인스턴스 수 축소)이 자주 수행될 것으로 예상되는 상태 비저장 서비스를 호스트하는 모든 노드 유형에는 실버 또는 골드 재구성을 사용하고, 이러한 규모 감축 작업의 간소화를 위해 배포 작업이 지연되도록 하고 용량이 감소되도록 하는 것이 좋습니다. 규모 확장 시나리오(VM 인스턴스 추가)는 내구성 계층 선택에 영향을 주지 않으며, 규모 감축만 영향을 줍니다.

### <a name="changing-durability-levels"></a>내구성 수준 변경
- 내구성 수준이 Silver 또는 Gold인 노드 유형은 Bronze로 다운그레이드할 수 없습니다.
- Bronze에서 Silver 또는 Gold로 업그레이드하는 데 몇 시간이 걸릴 수 있습니다.
- 내구성 수준을 변경할 때는 가상 머신 확장 집합 리소스의 Service Fabric 확장 구성과 Service Fabric 클러스터 리소스의 노드 유형 정의 모두에서 업데이트해야 합니다. 이러한 값이 일치해야 합니다.

### <a name="operational-recommendations-for-the-node-type-that-you-have-set-to-silver-or-gold-durability-level"></a>Silver 또는 Gold 내구성 수준으로 설정한 노드 유형에 대한 운영 권장 사항입니다.

- 클러스터와 애플리케이션을 항상 정상 상태로 유지하고, 애플리케이션이 모든 [서비스 복제본 수명 주기 이벤트](service-fabric-reliable-services-lifecycle.md)(예: 빌드의 복제본에 문제가 있을 경우)에 제때에 응답하는지 확인하세요.
- VM SKU 변경(강화/규모 축소) 시 더 안전한 방법을 채택하세요. 다양 한 단계 및 고려 사항은 필요 가상 머신 확장 집합의 VM SKU를 변경 합니다. 일반적인 문제를 방지하기 위해 수행할 수 있는 프로세스는 다음과 같습니다.
    - **주가 아닌 노드 형식:** 새 가상 머신 확장 집합 만들기, 새 가상 머신 확장 집합/노드 형식을 포함 한 다음 (이렇게 하면 한 번에 한 노드씩 0으로 이전 가상 머신 확장 집합 인스턴스 수를 줄이십시오 하도록 서비스 배치 제약 조건을 수정 하는 것이 좋습니다. 있는지 노드 제거는 클러스터의 안정성 영향을 주지 않습니다).
    - **주 노드 유형의 경우:** 선택한 VM SKU 용량을 더 큰 VM SKU로 변경 하려는 경우 지침에 따릅니다 [주 노드 형식에 대 한 수직적 크기 조정은](https://docs.microsoft.com/azure/service-fabric/service-fabric-scale-up-node-type)합니다. 

- Gold 또는 Silver 내구성 수준이 활성화된 가상 머신 확장 집합의 노드 수는 최소 5개로 유지합니다.
- 내구성 수준이 Silver 또는 Gold인 각 가상 머신 확장 집합은 Service Fabric 클러스터에서 고유한 노드 형식에 매핑되어야 합니다. 여러 가상 머신 확장 집합을 단일 노드 형식에 매핑하면 Service Fabric 클러스터와 Azure 인프라 간의 조정이 제대로 작동하지 않습니다.
- 임의의 VM 인스턴스를 삭제하지 말고 항상 가상 머신 확장 집합 규모 축소 기능을 사용하세요. 임의의 VM 인스턴스를 삭제하면 UD와 FD 전체에서 VM 인스턴스의 불균형 생성이 확산될 가능성이 있습니다. 이러한 불균형은 서비스 인스턴스/서비스 복제본 간에 부하를 적절하게 분산하는 시스템 기능에 부정적인 영향을 줄 수 있습니다.
- 자동 크기 조정을 사용하는 경우 한 번에 한 노드에서만 규모 감축(VM 인스턴스 제거)이 수행되도록 규칙을 설정하세요. 한 번에 여러 인스턴스의 규모를 축소하는 방식은 안전하지 않습니다.
- 주 노드 형식에서 VM을 삭제하거나 할당 해제하는 경우 전용 VM 수를 안정성 계층에 필요한 수 미만으로 줄여서는 안 됩니다. 이러한 작업은 내구성 수준이 Silver 또는 Gold인 확장 집합에서 무기한 차단됩니다.

## <a name="the-reliability-characteristics-of-the-cluster"></a>클러스터의 안정성 특성
안정성 계층은 주 노드 유형에서 이 클러스터에서 실행하려는 시스템 서비스의 복제본 수를 설정하는 데 사용됩니다. 복제본 수가 많을수록 클러스터에서 시스템 서비스를 신뢰할 수 있습니다.  

안정성 계층은 다음 값을 사용할 수 있습니다.

* 플래티넘 - 7이라는 대상 복제본 세트 수를 가진 시스템 서비스를 실행합니다.
* 골드 - 7이라는 대상 복제본 세트 수를 가진 시스템 서비스를 실행합니다.
* 실버 - 5라는 대상 복제본 세트 수를 가진 시스템 서비스를 실행합니다. 
* 브론즈 - 3이라는 대상 복제본 세트 수를 가진 시스템 서비스를 실행합니다.

> [!NOTE]
> 선택한 안정성 계층은 주 노드 형식이 보유해야 하는 노드의 최소 수를 결정합니다. 
> 
> 

### <a name="recommendations-for-the-reliability-tier"></a>안정성 계층에 대한 권장 사항.

클러스터 크기(모든 노드 형식의 VM 인스턴스 합계)를 늘리거나 줄일 때는 클러스터의 안정성 계층을 업데이트해야 합니다. 이렇게 하면 시스템 서비스 복제본 세트 수를 변경하는 데 필요한 클러스터 업그레이드를 트리거합니다. 노드를 추가하는 것처럼 클러스터를 다르게 변경하기 전에 진행 중인 업그레이드가 완료될 때까지 기다립니다.  Service Fabric Explorer에서 또는 [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)를 실행하여 업그레이드 진행률을 모니터링할 수 있습니다.

안정성 계층 선택과 관련한 권장 사항은 다음과 같습니다.  또한 시드 노드 수는 안정성 계층의 최소 개수로 설정됩니다.  예를 들어 안정성 등급이 Gold인 클러스터는 시드 노드가 7개입니다.

| **클러스터 노드 수** | **안정성 계층** |
| --- | --- |
| 1 |안정성 계층 매개 변수를 지정하지 마세요. 시스템에서 매개 변수를 계산합니다. |
| 3 |Bronze |
| 5 또는 6|Silver |
| 7 또는 8 |Gold |
| 9 이상 |플래티넘 |

## <a name="primary-node-type---capacity-guidance"></a>주 노드 유형 - 용량 지침

다음은 주 노드 유형 용량 계획에 대한 지침입니다.

- **Azure에서 프로덕션 워크 로드를 실행 하기 위한 VM 인스턴스 수:** 5와는 안정성 Silver 계층의 최소 주 노드 형식 크기를 지정 해야 합니다.  
- **Azure에서 테스트 작업을 실행하기 위한 VM 인스턴스 수** 최소 주 노드 형식 크기를 1 또는 3으로 지정할 수 있습니다. 하나의 노드 클러스터는 특별한 구성으로 실행되므로 해당 클러스터의 규모 확장은 지원되지 않습니다. 하나의 노드 클러스터는 안정성이 없으므로 Resource Manager 템플릿에서 해당 구성을 제거해야 하며/지정해서는 안 됩니다(구성 값을 설정하지 않는 것은 충분하지 않음). 포털을 통해 하나의 노드 클러스터 설정을 설정한 경우 구성이 자동으로 처리됩니다. 1 및 3 노드 클러스터는 프로덕션 워크로드 실행에 대해 지원되지 않습니다. 
- **VM SKU:** 주 노드 유형 이므로 시스템 서비스가 실행 되는 VM SKU를 전체 최대 고려해 야 로드 해야 하는 것이 원하는 클러스터를 배치할 계획 합니다. 이러한 설명을 뒷받침하는 예가 다음에 나와 있습니다. 주 노드 유형을 "폐"라고 생각하면 폐는 뇌에 산소를 공급합니다. 또한 뇌에 충분한 산소가 공급되지 않으면 몸 전체에 문제가 발생합니다. 

클러스터의 용량 요구 사항은 클러스터에서 실행하려는 작업에 따라 결정됩니다. 따라서 특정 작업에 대해 정성적 지침을 제공할 수는 없지만 다음의 보편적인 지침을 통해 시작하는 데 필요한 도움을 얻을 수 있을 것입니다.

프로덕션 워크로드: 

- 클러스터 기본 NodeType을 시스템 서비스 전용으로 지정하고 배포 제약 조건을 사용하여 보조 NodeType에 애플리케이션을 배포하는 것이 좋습니다.
- 권장된 VM SKU는 표준 D2_V2 이거나 동급 50GB 로컬 SSD의 최소입니다.
- 지원 되는 최소 사용 VM SKU Standard_D2_V3 또는 표준 D1_V2 이거나 동급 50GB의 로컬 SSD의 경우 
- 권장 사항은 최소 50GB입니다. 워크로드를 위해, 특히 Windows 컨테이너를 실행하는 경우, 더 큰 디스크가 필요합니다. 
- 프로덕션 작업에는 표준 A0과 같은 부분 코어 VM SKU가 지원되지 않습니다.
- 성능상의 이유로 프로덕션 워크 로드에 대 한 VM Sku 시리즈 사용할 수 없습니다.
- 우선 순위가 낮은 VM은 지원되지 않습니다.

> [!WARNING]
> 실행 중인 클러스터에서 주 노드 VM SKU 크기를 변경하는 것은 크기 조정 작업이고 [가상 머신 확장 집합 확장](virtual-machine-scale-set-scale-node-type-scale-out.md) 설명서에 설명되어 있습니다.

## <a name="non-primary-node-type---capacity-guidance-for-stateful-workloads"></a>상태 저장 워크로드에 대한 주가 아닌 노드 유형 - 용량 지침

이 지침은 주 노드가 아닌 노드 형식에서 실행 중인 Service Fabric [Reliable Collections 또는 Reliable Actors](service-fabric-choose-framework.md)를 사용하는 상태 저장 작업과 관련된 것입니다.

**VM 인스턴스 수:** 상태 저장 방식의 프로덕션 워크 로드에 대 한 대상 및 최소 복제본 수가 5 사용 하 여 실행 하는 것이 좋습니다. 즉, 각 장애 도메인 및 업그레이드 도메인에 1개의 복제본(복제본 세트 중)이 있으면 안정적인 것입니다. 주 노드 형식에 대한 전체 안정성 계층 개념은 실제로 시스템 서비스에 대해 이 설정을 지정하는 방식에 불과합니다. 따라서 상태 저장 서비스에도 동일한 고려 사항이 적용됩니다.

따라서 프로덕션 워크로드에서 상태 저장 워크로드를 실행하는 경우 권장되는 최소 주가 아닌 노드 유형 크기는 5입니다.

**VM SKU:** 이 노드 유형을 응용 프로그램 서비스를 실행 하는 위치 이므로 VM SKU를 선택 하면 고려해 야는 최고 부하를 각 노드에 배치할 계획. 노드 형식의 용량 요구 사항은 클러스터에서 실행하려는 워크로드에 따라 결정됩니다. 특정 워크로드에 대해 정성적 지침을 제공할 수는 없지만 다음의 보편적인 지침을 통해 시작하는 데 필요한 도움을 얻을 수 있을 것입니다.

프로덕션 워크로드 

- 권장된 VM SKU는 표준 D2_V2 이거나 동급 50GB 로컬 SSD의 최소입니다.
- 지원 되는 최소 사용 VM SKU Standard_D2_V3 또는 표준 D1_V2 이거나 동급 50GB의 로컬 SSD의 경우 
- 프로덕션 작업에는 표준 A0과 같은 부분 코어 VM SKU가 지원되지 않습니다.
- 성능상의 이유로 프로덕션 워크 로드에 대 한 VM Sku 시리즈 사용할 수 없습니다.

## <a name="non-primary-node-type---capacity-guidance-for-stateless-workloads"></a>상태 비저장 워크로드에 대한 주가 아닌 노드 유형 - 용량 지침

이 지침은 주 노드가 아닌 노드 형식에서 실행 중인 상태 비저장 작업과 관련된 것입니다.

**VM 인스턴스 수:** 상태 비저장 프로덕션 워크 로드에 대 한 최소 지원 되는 주가 아닌 노드 형식 크기는 2입니다. 이 옵션을 사용하면 애플리케이션에 대해 2개의 상태 비저장 인스턴스를 실행할 수 있으며 서비스는 VM 인스턴스가 손실되어도 계속 실행될 수 있습니다. 

**VM SKU:** 이 노드 유형을 응용 프로그램 서비스를 실행 하는 위치 이므로 VM SKU를 선택 하면 고려해 야는 최고 부하를 각 노드에 배치할 계획. 노드 형식의 용량 요구 사항은 클러스터에서 실행하려는 워크로드에 따라 결정됩니다. 특정 워크로드에 대한 정성적 지침을 제공할 수는 없습니다.  하지만 시작 방법을 알려주는 방대한 지침이 제공됩니다.

프로덕션 워크로드 

- 권장된 VM SKU는 표준 D2_V2 또는 이와 동등한 됩니다. 
- 지원되는 최소 사용 VM SKU는 표준 D1 또는 표준 D1_V2이거나 동급 수준입니다. 
- 프로덕션 작업에는 표준 A0과 같은 부분 코어 VM SKU가 지원되지 않습니다.
- 성능상의 이유로 프로덕션 워크 로드에 대 한 VM Sku 시리즈 사용할 수 없습니다.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>다음 단계
용량 계획을 마치고 클러스터를 설정한 후에는 다음 항목을 확인하세요.

* [서비스 패브릭 클러스터 보안](service-fabric-cluster-security.md)
* [Service Fabric 클러스터 보안](service-fabric-cluster-scaling.md)
* [재해 복구 계획](service-fabric-disaster-recovery.md)
* [가상 머신 확장 집합에 대한 Nodetype의 관계](service-fabric-cluster-nodetypes.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png
