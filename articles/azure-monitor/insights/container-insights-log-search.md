---
title: 컨테이너의 Azure Monitor에서 로그를 쿼리 하는 방법 | Microsoft Docs
description: 컨테이너에 대 한 Azure Monitor는 메트릭 및 로그 데이터를 수집 하 고이 문서에서는 레코드를 설명 하 고 샘플 쿼리를 포함 합니다.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/12/2019
ms.author: magoedte
ms.openlocfilehash: d6e65331db53be5ba13a75e6b03b271f1071716d
ms.sourcegitcommit: 6b41522dae07961f141b0a6a5d46fd1a0c43e6b2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/15/2019
ms.locfileid: "67989826"
---
# <a name="how-to-query-logs-from-azure-monitor-for-containers"></a>컨테이너의 Azure Monitor에서 로그를 쿼리 하는 방법

컨테이너 Azure Monitor는 컨테이너 호스트와 컨테이너에서 성능 메트릭, 인벤토리 데이터 및 상태 정보를 수집 하 고 Azure Monitor의 Log Analytics 작업 영역으로 전달 합니다. 데이터는 3분마다 수집됩니다. 이 데이터는 Azure Monitor에서 [쿼리에](../../azure-monitor/log-query/log-query-overview.md) 사용할 수 있습니다. 마이그레이션 계획, 용량 분석, 검색 및 주문형 성능 문제 해결을 포함하는 시나리오에 이 데이터를 적용할 수 있습니다.

## <a name="container-records"></a>컨테이너 레코드

다음 표에는 컨테이너용 Azure Monitor에서 수집하는 레코드 및 로그 검색 결과에 표시되는 데이터 유형의 예가 나와 있습니다.

| 데이터 형식 | 로그 검색의 데이터 유형 | 필드 |
| --- | --- | --- |
| 호스트 및 컨테이너에 대한 성능 | `Perf` | 컴퓨터, ObjectName, CounterName &#40;%프로세서 시간, 디스크 읽기 MB, 디스크 쓰기 MB, 메모리 사용 MB, 네트워크 수신 바이트, 네트워크 송신 바이트, 프로세서 사용 초, 네트워크&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem |
| 컨테이너 인벤토리 | `ContainerInventory` | TimeGenerated, 컴퓨터, 컨테이너 이름, ContainerHostname, 이미지, ImageTag, ContainerState, ExitCode, EnvironmentVar, 명령, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID |
| 컨테이너 로그 | `ContainerLog` | TimeGenerated, 컴퓨터, 이미지 ID, 컨테이너 이름, LogEntrySource, LogEntry, SourceSystem, ContainerID |
| 컨테이너 노드 인벤토리 | `ContainerNodeInventory`| TimeGenerated, 컴퓨터, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem|
| Kubernetes 클러스터의 Pod 인벤토리 | `KubePodInventory` | TimeGenerated, 컴퓨터, ClusterId, ContainerCreationTimeStamp, PodUid, PodCreationTimeStamp, ContainerRestartCount, PodRestartCount, PodStartTime, ContainerStartTime, ServiceName, ControllerKind, ControllerName, 컨테이너 상태,  ContainerStatusReason, ContainerID, ContainerName, Name, PodLabel, Namespace, PodStatus, ClusterName, PodIp, SourceSystem |
| Kubernetes 클러스터의 노드 부분 인벤토리 | `KubeNodeInventory` | TimeGenerated, Computer, ClusterName, ClusterId, LastTransitionTimeReady, Labels, Status, KubeletVersion, KubeProxyVersion, CreationTimeStamp, SourceSystem | 
| Kubernetes 이벤트 | `KubeEvents` | TimeGenerated, Computer, ClusterId_s, FirstSeen_t, LastSeen_t, Count_d, ObjectKind_s, Namespace_s, Name_s, Reason_s, Type_s, TimeGenerated_s, SourceComponent_s, ClusterName_s, Message,  SourceSystem | 
| Kubernetes 클러스터의 서비스 | `KubeServices` | TimeGenerated, ServiceName_s, Namespace_s, SelectorLabels_s, ClusterId_s, ClusterName_s, ClusterIP_s, ServiceType_s, SourceSystem | 
| Kubernetes 클러스터의 노드 부분에 대한 성능 메트릭 | Perf &#124; where ObjectName == “K8SNode” | Computer, ObjectName, CounterName &#40;CpuAllocatableBytes, memoryAllocatableBytes, CpuCapacityNanoCores, MemoryCapacityBytes, memoryRssBytes, CpuUsageNanoCores, MemoryWorkingsetBytes, restartTimeEpoch&#41;, countervalue, TimeGenerated, CounterPath, SourceSystem | 
| Kubernetes 클러스터의 컨테이너 부분에 대한 성능 메트릭 | Perf &#124; where ObjectName == “K8SContainer” | CounterName &#40; CpuRequestNanoCores, memoryRequestBytes, CpuLimitNanoCores, MemoryWorkingSetBytes, RestartTimeEpoch, CpuUsageNanoCores, memoryRssBytes&#41;, Countervalue, Timegenerated, countervalue, SourceSystem | 
| 사용자 지정 메트릭 |`InsightsMetrics` | 컴퓨터, 이름, 네임 스페이스, 원본, SourceSystem, 태그<sup>1</sup>, Timegenerated, Type, Va, _resourceid | 

<sup>1</sup> *Tags* 속성은 해당 메트릭에 대 한 [여러 차원을](../platform/data-platform-metrics.md#multi-dimensional-metrics) 나타냅니다. `InsightsMetrics` 테이블에 수집 및 저장 된 메트릭과 레코드 속성에 대 한 설명에 대 한 자세한 내용은 [InsightsMetrics 개요](https://github.com/microsoft/OMS-docker/blob/vishwa/june19agentrel/docs/InsightsMetrics.md)를 참조 하세요.

>[!NOTE]
>프로메테우스에 대 한 지원은 현재 공개 미리 보기의 기능입니다.
>

## <a name="search-logs-to-analyze-data"></a>로그를 검색하여 데이터 분석

Azure Monitor 로그는 추세를 찾고, 병목 상태를 진단 하거나, 예측 하거나, 현재 클러스터 구성이 최적 상태 인지 여부를 확인 하는 데 도움이 되는 데이터의 상관 관계를 확인 하는 데 도움이 됩니다. 미리 정의된 로그 검색을 즉시 사용하거나, 원하는 방식으로 정보를 반환하도록 사용자 지정할 수 있습니다.

미리 보기 창에서 **Kubernetes 이벤트 로그 보기** 또는 **컨테이너 로그 보기** 옵션을 선택하여 작업 영역에서 데이터에 대한 대화형 분석을 수행할 수 있습니다. 사용자가 있던 Azure Portal 페이지의 오른쪽에 **로그 검색** 페이지가 나타납니다.

![Log Analytics에서 데이터 분석](./media/container-insights-analyze/container-health-log-search-example.png)   

작업 영역에 전달 되는 컨테이너 로그 출력은 STDOUT 및 STDERR입니다. 컨테이너용 Azure Monitor는 AKS(Azure로 관리되는 Kubernetes)를 모니터링하고 이로 인해 생성되는 데이터 양이 많으므로 Kube 시스템은 오늘 수집되지 않습니다. 

### <a name="example-log-search-queries"></a>로그 검색 쿼리 예제

한두 가지 예제로 시작하는 쿼리를 작성한 다음, 요구 사항에 맞게 수정하는 것이 유용한 경우가 많습니다. 고급 쿼리를 작성하는 데 도움이 되도록 다음과 같은 샘플 쿼리를 테스트해 볼 수 있습니다.

| Query | 설명 | 
|-------|-------------|
| ContainerInventory<br> &#124; project Computer, Name, Image, ImageTag, ContainerState, CreatedTime, StartedTime, FinishedTime<br> &#124; render table | 컨테이너의 수명 주기 정보 모두 나열| 
| KubeEvents_CL<br> &#124; where not(isempty(Namespace_s))<br> &#124; sort by TimeGenerated desc<br> &#124; render table | kubernetes 이벤트|
| ContainerImageInventory<br> &#124; summarize AggregatedValue = count() by Image, ImageTag, Running | 이미지 인벤토리 | 
| **꺾은선형 차트 표시 옵션을 선택**:<br> Perf<br> &#124; where ObjectName == "K8SContainer" and CounterName == "cpuUsageNanoCores" &#124; summarize AvgCPUUsageNanoCores = avg(CounterValue) by bin(TimeGenerated, 30m), InstanceName | 컨테이너 CPU | 
| **꺾은선형 차트 표시 옵션을 선택**:<br> Perf<br> &#124; where ObjectName == "K8SContainer" and CounterName == "memoryRssBytes" &#124; summarize AvgUsedRssMemoryBytes = avg(CounterValue) by bin(TimeGenerated, 30m), InstanceName | 컨테이너 메모리 |

다음 예는 프로메테우스 메트릭 쿼리입니다. 수집 된 메트릭은 특정 기간 내에 발생 한 오류 수를 확인 하기 위해 개수를 계산 하는 데 사용할 수 있습니다. 데이터 집합은 *partitionKey*에 의해 분할 됩니다. 즉, 고유한 이름, *호스트* *이름*및 *OperationType*집합에 대해 해당 집합에 대 한 하위 쿼리를 실행 합니다.  해당 시간에 대해 기록 된 이전 *Timegenerated* 및 count를 찾아 요금을 확인 합니다.

```
let data = InsightsMetrics 
| where Namespace contains 'prometheus' 
| where Name == 'kubelet_docker_operations' or Name == 'kubelet_docker_operations_errors'    
| extend Tags = todynamic(Tags) 
| extend OperationType = tostring(Tags['operation_type']), HostName = tostring(Tags.hostName) 
| extend partitionKey = strcat(HostName, '/' , Name, '/', OperationType) 
| partition by partitionKey ( 
    order by TimeGenerated asc 
    | extend PrevVal = prev(Val, 1), PrevTimeGenerated = prev(TimeGenerated, 1) 
    | extend Rate = iif(TimeGenerated == PrevTimeGenerated, 0.0, Val - PrevVal) 
    | where isnull(Rate) == false 
) 
| project TimeGenerated, Name, HostName, OperationType, Rate; 
let operationData = data 
| where Name == 'kubelet_docker_operations' 
| project-rename OperationCount = Rate; 
let errorData = data 
| where Name == 'kubelet_docker_operations_errors' 
| project-rename ErrorCount = Rate; 
operationData 
| join kind = inner ( errorData ) on TimeGenerated, HostName, OperationType 
| project-away TimeGenerated1, Name1, HostName1, OperationType1 
| extend SuccessPercentage = iif(OperationCount == 0, 1.0, 1 - (ErrorCount / OperationCount))
```

출력은 다음과 유사한 결과를 표시 합니다.

![데이터 수집 볼륨의 쿼리 결과를 기록 합니다.](./media/container-insights-log-search/log-query-example-prometheus-metrics.png)

## <a name="next-steps"></a>다음 단계

컨테이너의 Azure Monitor에는 미리 정의 된 경고 집합이 포함 되지 않습니다. [컨테이너에 대 한 Azure Monitor를 사용 하 여 성능 경고 만들기](container-insights-alerts.md) 를 검토 하 여 devops 또는 운영 프로세스 및 절차를 지원 하기 위해 높은 CPU 및 메모리 사용률에 대해 권장 되는 경고를 만드는 방법을 알아봅니다. 