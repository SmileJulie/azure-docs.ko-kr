---
title: Azure Service Fabric에 대 한 용량 계획 및 크기 조정 | Microsoft Docs
description: Service Fabric 클러스터 및 애플리케이션 계획 및 크기 조정을 위한 모범 사례를 소개합니다.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2019
ms.author: pepogors
ms.openlocfilehash: fe0af4ca7b6860fff19f4df3165a975c42b54a03
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68277771"
---
# <a name="capacity-planning-and-scaling-for-azure-service-fabric"></a>Azure Service Fabric에 대 한 용량 계획 및 크기 조정

Azure Service Fabric 클러스터를 만들거나 클러스터를 호스트 하는 계산 리소스의 크기를 조정 하기 전에 용량을 계획 하는 것이 중요 합니다. 용량 계획에 대한 자세한 내용은 [Service Fabric 클러스터 용량 계획](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity)을 참조하세요. 클러스터 확장성에 대 한 추가 모범 사례 지침은 [Service Fabric 확장성 고려 사항](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric#scalability-considerations)을 참조 하세요.

노드 유형 및 클러스터 특성을 고려 하는 것 외에도 프로덕션 환경에 대 한 크기 조정 작업을 완료 하는 데 1 시간 이상 소요 될 것으로 예측할 수 있습니다. 이 고려 사항은 추가 하는 Vm의 수에 관계 없이 적용 됩니다.

## <a name="autoscaling"></a>자동 확장
[리소스 구성을 코드로]( https://docs.microsoft.com/azure/service-fabric/service-fabric-best-practices-infrastructure-as-code)처리 하는 것이 가장 좋은 방법 이므로 Azure Resource Manager 템플릿을 통해 크기 조정 작업을 수행 해야 합니다. 

가상 머신 확장 집합을 통해 자동 크기 조정을 사용 하면 버전이 지정 된 리소스 관리자 템플릿이 가상 머신 확장 집합의 인스턴스 수를 정확 하 게 정의 하 게 됩니다. 부정확 한 정의는 향후 배포에서 의도 하지 않은 크기 조정 작업을 야기 하는 위험을 늘립니다. 일반적으로 다음과 같은 경우 자동 크기 조정을 사용 해야 합니다.

* 적절한 용량을 선언하여 Resource Manager 템플릿을 배포하는 과정에서 사용 사례가 지원되지 않는 경우
     
   수동으로 크기를 조정 하는 것 외에도 [Azure 리소스 그룹 배포 프로젝트를 사용 하 여 Azure DevOps Services에서 지속적인 통합 및 배달 파이프라인](https://docs.microsoft.com/azure/vs-azure-tools-resource-groups-ci-in-vsts)을 구성할 수 있습니다. 이 파이프라인은 일반적으로 [Azure Monitor REST API](https://docs.microsoft.com/azure/azure-monitor/platform/rest-api-walkthrough)에서 쿼리 된 가상 컴퓨터 성능 메트릭을 사용 하는 논리 앱에 의해 트리거됩니다. 파이프라인은 리소스 관리자 템플릿을 최적화 하는 동안 원하는 메트릭에 따라 효율적으로 크기를 조정 합니다.
* 한 번에 하나의 가상 머신 확장 집합 노드만 수평 확장 해야 합니다.
   
   한 번에 세 개 이상의 노드로 확장 하려면 [가상 머신 확장 집합을 추가 하 여 Service Fabric 클러스터를 확장](virtual-machine-scale-set-scale-node-type-scale-out.md)해야 합니다. 가상 머신 확장 집합을 한 번에 하나씩 수평으로 확장 및 축소 하는 것이 가장 안전 합니다.
* 자동 크기 조정 규칙을 구성 하는 모든 규모에서 Service Fabric 클러스터에 대해 실버 안정성 이상이 있고 실버 내구성이 높습니다.
  
   자동 크기 조정 규칙의 최소 용량은 5 개의 가상 컴퓨터 인스턴스와 같거나 커야 합니다. 주 노드 형식에 대 한 안정성 계층 최소값 보다 크거나 같아야 합니다.

> [!NOTE]
> Service Fabric 상태 저장 서비스 패브릭:/System/InfastructureService/< NODE_TYPE_NAME >는 은색 이상의 내구성이 있는 모든 노드 형식에서 실행 됩니다. 클러스터 노드 형식의 Azure에서 실행 하도록 지원 되는 유일한 시스템 서비스입니다.

## <a name="vertical-scaling-considerations"></a>수직 크기 조정 관련 고려 사항

Azure Service Fabric에서 노드 유형을 [수직 확장](https://docs.microsoft.com/azure/service-fabric/virtual-machine-scale-set-scale-node-type-scale-out) 하려면 여러 단계와 고려 사항이 필요 합니다. 예를 들어:

* 크기 조정 전에 클러스터가 정상 상태여야 합니다. 그렇지 않으면 클러스터를 추가로 불안정 하 게 됩니다.
* 상태 저장 서비스를 호스트 하는 모든 Service Fabric 클러스터 노드 형식에는 실버 내구성 수준 이상이 필요 합니다.

> [!NOTE]
> 상태 저장 Service Fabric 시스템 서비스를 호스트 하는 주 노드 형식은 실버 내구성 수준 이상 이어야 합니다. 실버 내구성을 사용 하도록 설정한 후에는 업그레이드, 노드 추가 또는 제거와 같은 클러스터 작업이 속도가 더 느릴 수 있습니다 .이는 시스템에서 작업 속도를 통해 데이터 안전을 최적화 하기 때문입니다.

가상 머신 확장 집합의 수직 확장은 파괴적인 작업입니다. 대신 원하는 SKU를 사용 하 여 새 확장 집합을 추가 하 여 클러스터를 수평으로 확장 합니다. 그런 다음 서비스를 원하는 SKU로 마이그레이션하여 안전한 수직 크기 조정 작업을 완료 합니다. 가상 머신 확장 집합 리소스 SKU를 변경 하는 작업은 호스트를 다시 이미지 하 여 모든 로컬에서 지속 되는 상태를 제거 하기 때문에 파괴적인 작업입니다.

클러스터는 Service Fabric [노드 속성 및 배치 제약 조건을](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-resource-manager-cluster-description#node-properties-and-placement-constraints) 사용 하 여 응용 프로그램의 서비스를 호스트할 위치를 결정 합니다. 주 노드 형식의 크기를 세로로 조정 하는 경우에 대해 `"nodeTypeRef"`동일한 속성 값을 선언 합니다. 이러한 값은 가상 머신 확장 집합에 대 한 Service Fabric 확장에서 찾을 수 있습니다. 

리소스 관리자 템플릿의 다음 코드 조각은 선언할 속성을 보여 줍니다. 크기를 조정 하는 새로 프로 비전 된 확장 집합에 대해 동일한 값을 가지 며, 클러스터에 대 한 임시 상태 저장 서비스로만 지원 됩니다.

```json
"settings": {
   "nodeTypeRef": ["[parameters('primaryNodetypeName')]"]
}
```

> [!NOTE]
> 성공적인 수직 크기 조정 작업을 완료 하는 데 필요한 것 보다 `nodeTypeRef` 더 긴 동일한 속성 값을 사용 하는 여러 확장 집합을 사용 하 여 클러스터를 실행 하지 마세요.
>
> 프로덕션 환경에 대 한 변경을 시도 하기 전에 항상 테스트 환경에서 작업의 유효성을 검사 합니다. 기본적으로 Service Fabric 클러스터 시스템 서비스에는 대상 주 노드 형식에 대 한 배치 제약 조건만 있습니다.

노드 속성 및 배치 제약 조건을 선언한 후 다음 단계를 한 번에 한 VM 인스턴스씩 수행합니다. 이렇게 하면 새 복제본이 생성 될 때 제거 하려는 VM 인스턴스에서 시스템 서비스 (및 상태 저장 서비스)를 정상적으로 종료할 수 있습니다.

1. PowerShell에서 의도 `RemoveNode` 한 `Disable-ServiceFabricNode` 대로를 실행 하 여 제거 하려는 노드를 사용 하지 않도록 설정 합니다. 번호가 가장 큰 노드 형식을 제거합니다. 예를 들어 6 개 노드 클러스터가 있는 경우 "MyNodeType_5" 가상 머신 인스턴스를 제거 합니다.
2. `Get-ServiceFabricNode`를 실행하여 노드가 사용하지 않도록 전환되었는지 확인합니다. 그렇지 않은 경우 노드가 사용되지 않도록 설정될 때까지 기다립니다. 각 노드에 대해 몇 시간이 걸릴 수 있습니다. 노드가 사용 안 함 상태로 전환될 때까지는 다음 단계를 진행하지 마세요.
3. 해당 노드 형식에서 Vm 수를 하나씩 줄입니다. 그러면 번호가 가장 큰 VM 인스턴스가 제거됩니다.
4. 필요에 따라 1~3단계를 반복하되, 주 노드 형식의 인스턴스 수를 안정성 계층이 경고하는 크기보다 작게 줄이지 않아야 합니다. 권장 인스턴스 목록은 [Service Fabric 클러스터 용량 계획](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity)을 참조하세요.
5. 모든 Vm이 사라진 후 ("Down"으로 표시 됨) fabric:/System/InfrastructureService/[node name]에 오류 상태가 표시 됩니다. 그런 다음 클러스터 리소스를 업데이트 하 여 노드 유형을 제거할 수 있습니다. ARM 템플릿 배포를 사용 하거나 [Azure resource manager](https://resources.azure.com)를 통해 클러스터 리소스를 편집할 수 있습니다. 그러면 오류 상태인 fabric:/System/InfrastructureService/[node type] 서비스가 제거 되는 클러스터 업그레이드가 시작 됩니다.
 6. 그런 다음 필요에 따라 VMScaleSet를 삭제할 수 있습니다. 그러나이 경우에도 노드는 Service Fabric Explorer 보기에서 "다운"으로 표시 됩니다. 마지막 단계는 명령으로 `Remove-ServiceFabricNodeState` 정리 하는 것입니다.

## <a name="horizontal-scaling"></a>수평적 크기 조정

[수동](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-scale-up-down) 또는 [프로그래밍 방식으로](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-programmatic-scaling)수평 크기 조정을 수행할 수 있습니다.

> [!NOTE]
> 실버 또는 골드 내구성이 있는 노드 유형의 크기를 조정 하는 경우 크기 조정 속도가 느려집니다.

### <a name="scaling-out"></a>확장

특정 가상 머신 확장 집합에 대 한 인스턴스 수를 늘려서 Service Fabric 클러스터를 확장 합니다. 및 원하는 확장 집합의 ID를 `AzureClient` 사용 하 여 프로그래밍 방식으로 규모를 확장 하 여 용량을 늘릴 수 있습니다.

```c#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
```

수동으로 규모를 확장 하려면 원하는 [가상 머신 확장 집합](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile) 리소스의 SKU 속성에서 용량을 업데이트 합니다.

```json
"sku": {
    "name": "[parameters('vmNodeType0Size')]",
    "capacity": "[parameters('nt0InstanceCount')]",
    "tier": "Standard"
}
```

### <a name="scaling-in"></a>규모 감축

규모를 감축하려면 확장할 때보다 더 많은 사항을 고려해야 합니다. 예를 들어:

* Service Fabric 시스템 서비스는 클러스터의 주 노드 형식에서 실행 됩니다. 인스턴스 수가 안정성 계층에서 보증하는 수보다 적어지도록 해당 노드 형식의 인스턴스를 종료하거나 인스턴스 수를 축소하지 마세요. 
* 상태 저장 서비스의 경우 가용성을 유지 하 고 서비스 상태를 유지 하기 위해 항상 필요한 특정 수의 노드가 필요 합니다. 최소한 파티션 또는 서비스의 대상 복제본 집합 수와 동일한 수의 노드가 필요 합니다.

수동으로 규모를 감축하려면 다음 단계를 수행합니다.

1. PowerShell에서 의도 `RemoveNode` 한 `Disable-ServiceFabricNode` 대로를 실행 하 여 제거 하려는 노드를 사용 하지 않도록 설정 합니다. 번호가 가장 큰 노드 형식을 제거합니다. 예를 들어 6 개 노드 클러스터가 있는 경우 "MyNodeType_5" 가상 머신 인스턴스를 제거 합니다.
2. `Get-ServiceFabricNode`를 실행하여 노드가 사용하지 않도록 전환되었는지 확인합니다. 그렇지 않은 경우 노드가 사용되지 않도록 설정될 때까지 기다립니다. 각 노드에 대해 몇 시간이 걸릴 수 있습니다. 노드가 사용 안 함 상태로 전환될 때까지는 다음 단계를 진행하지 마세요.
3. 해당 노드 형식에서 Vm 수를 하나씩 줄입니다. 그러면 번호가 가장 큰 VM 인스턴스가 제거됩니다.
4. 원하는 용량을 프로 비전 할 때까지 필요에 따라 1 ~ 3 단계를 반복 합니다. 단, 주 노드 형식의 인스턴스 수가 안정성 계층에서 보증되는 수보다 적어지도록 클러스터를 축소해서는 안 됩니다. 권장 인스턴스 목록은 [Service Fabric 클러스터 용량 계획](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity)을 참조하세요.

수동으로 크기를 조정 하려면 원하는 [가상 머신 확장 집합](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile) 리소스의 SKU 속성에서 용량을 업데이트 합니다.

```json
"sku": {
    "name": "[parameters('vmNodeType0Size')]",
    "capacity": "[parameters('nt0InstanceCount')]",
    "tier": "Standard"
}
```

프로그래밍 방식으로 크기를 조정 하려면 종료 하기 위해 노드를 준비 해야 합니다. 제거할 노드 (가장 높은 인스턴스 노드)를 찾습니다. 예를 들어:

```c#
using (var client = new FabricClient())
{
    var mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync())
        .Where(n => n.NodeType.Equals(NodeTypeToScale, StringComparison.OrdinalIgnoreCase))
        .Where(n => n.NodeStatus == System.Fabric.Query.NodeStatus.Up)
        .OrderByDescending(n =>
        {
            var instanceIdIndex = n.NodeName.LastIndexOf("_");
            var instanceIdString = n.NodeName.Substring(instanceIdIndex + 1);
            return int.Parse(instanceIdString);
        })
        .FirstOrDefault();
```

이전 코드에서 사용한 것과 동일한 `FabricClient` 인스턴스 (`client` 이 경우) 및 노드 인스턴스 (`instanceIdString` 이 경우)를 사용 하 여 노드를 비활성화 하 고 제거 합니다.

```c#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);

// Remove the node from the Service Fabric cluster
ServiceEventSource.Current.ServiceMessage(Context, $"Disabling node {mostRecentLiveNode.NodeName}");
await client.ClusterManager.DeactivateNodeAsync(mostRecentLiveNode.NodeName, NodeDeactivationIntent.RemoveNode);

// Wait (up to a timeout) for the node to gracefully shut down
var timeout = TimeSpan.FromMinutes(5);
var waitStart = DateTime.Now;
while ((mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Up || mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Disabling) &&
        DateTime.Now - waitStart < timeout)
{
    mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync()).FirstOrDefault(n => n.NodeName == mostRecentLiveNode.NodeName);
    await Task.Delay(10 * 1000);
}

// Decrement virtual machine scale set capacity
var newCapacity = (int)Math.Max(MinimumNodeCount, scaleSet.Capacity - 1); // Check min count 

scaleSet.Update().WithCapacity(newCapacity).Apply();
```

> [!NOTE]
> 클러스터를 축소 하면 제거 된 노드/v m 인스턴스가 Service Fabric Explorer에서 비정상 상태로 표시 되는 것을 볼 수 있습니다. 이 동작에 대 한 설명은 [Service Fabric Explorer에서 관찰할 수 있는 동작](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-scale-up-down#behaviors-you-may-observe-in-service-fabric-explorer)을 참조 하세요. 다음을 할 수 있습니다.
> * 해당 노드 이름을 사용 하 여 [remove-servicefabricnodestate 명령을](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate?view=azureservicefabricps) 호출 합니다.
> * 클러스터에 [Service Fabric 자동 크기 조정 도우미 응용 프로그램](https://github.com/Azure/service-fabric-autoscale-helper/) 을 배포 합니다. 이 응용 프로그램은 축소 된 노드가 Service Fabric Explorer에서 지워지는 지 확인 합니다.

## <a name="reliability-levels"></a>안정성 수준

[안정성 수준은](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) Service Fabric 클러스터 리소스의 속성입니다. 개별 노드 형식에 대해 다르게 구성할 수 없습니다. 클러스터에 대한 시스템 서비스의 복제 인자를 제어하는 이 속성은 클러스터 리소스 수준의 설정입니다. 

안정성 수준에 따라 주 노드 형식이 포함해야 하는 최소 노드 수가 결정됩니다. 안정성 계층은 다음 값을 사용할 수 있습니다.

* 플래티넘 7 ~ 9 개의 시드 노드를 사용 하 여 대상 복제본 세트 수를 가진 시스템 서비스를 실행 합니다.
* 골드 7 개 및 7 개의 시드 노드를 사용 하 여 대상 복제본 세트 수를 가진 시스템 서비스를 실행 합니다.
* 은색 5 개 및 5 개 시드 노드의 대상 복제본 집합 수를 사용 하 여 시스템 서비스를 실행 합니다.
* 색 3 개 및 3 개의 시드 노드에 대 한 대상 복제본 세트 수를 가진 시스템 서비스를 실행 합니다.

최소 권장 안정성 수준은 Silver입니다.

안정성 수준은 다음과 같이 [Microsoft.ServiceFabric/clusters resource](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/2018-02-01/clusters)의 속성 섹션에서 설정됩니다.

```json
"properties":{
    "reliabilityLevel": "Silver"
}
```

## <a name="durability-levels"></a>내구성 수준

> [!WARNING]
> Bronze 내구성으로 실행되는 노드 형식은 _권한 없음_이 됩니다. 상태 비저장 워크 로드에 영향을 주는 인프라 작업은 중지 되거나 지연 되지 않으므로 워크 로드에 영향을 줄 수 있습니다. 
>
> Bronze 내구성은 상태 비저장 워크로드를 실행하는 노드 형식에만 사용해야 합니다. 프로덕션 워크 로드의 경우 상태 일관성을 유지 하려면 실버 이상을 실행 합니다. [용량 계획 설명서](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity)의 지침에 따라 올바른 안정성을 선택합니다.

내구성 수준은 두 리소스에서 설정해야 합니다. 하나는 [가상 머신 확장 집합 리소스](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile)의 확장 프로필입니다.

```json
"extensionProfile": {
    "extensions":          {
        "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
        "properties": {
            "settings": {
                "durabilityLevel": "Bronze"
            }
        }
    }
}
```

다른 리소스 `nodeTypes` 는 [ServiceFabric/클러스터 리소스](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/2018-02-01/clusters)에 있습니다. 

```json
"nodeTypes": [
    {
        "name": "[variables('vmNodeType0Name')]",
        "durabilityLevel": "Bronze"
    }
]
```

## <a name="next-steps"></a>다음 단계

* Windows Server를 실행하는 VM 또는 컴퓨터에서 클러스터 만들기: [Windows Server용 Service Fabric 클러스터 만들기](service-fabric-cluster-creation-for-windows-server.md)
* Linux를 실행하는 VM 또는 컴퓨터에서 클러스터 만들기: [Linux 클러스터 만들기](service-fabric-cluster-creation-via-portal.md)
* [Service Fabric 지원 옵션](service-fabric-support.md)에 대해 알아봅니다.

[Image1]: ./media/service-fabric-best-practices/generate-common-name-cert-portal.png
