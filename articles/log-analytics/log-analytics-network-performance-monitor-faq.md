---
title: FAQ - Azure의 네트워크 성능 모니터 솔루션 | Microsoft Docs
description: 이 문서에서는 Azure의 NPM에 대 한 질문과 대답을 캡처합니다. NPM(네트워크 성능 모니터)을 사용하면 네트워크 성능을 거의 실시간으로 모니터링하여 네트워크 성능 병목을 감지하고 찾을 수 있습니다.
services: log-analytics
documentationcenter: ''
author: vinynigam
manager: agummadi
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2018
ms.author: vinynigam
ms.openlocfilehash: 2821f3fa07d8d9ada02da212084639c93e469d0b
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49408883"
---
# <a name="network-performance-monitor-solution-faq"></a>네트워크 성능 모니터 솔루션 FAQ

![네트워크 성능 모니터 기호](media/log-analytics-network-performance-monitor/npm-symbol.png)

이 문서에서는 NPM(네트워크 성능 모니터)에 대한 FAQ(질문과 대답)를 캡처합니다.

[네트워크 성능 모니터](/azure/networking/network-monitoring-overview)는 네트워크 인프라의 다양한 지점 간 네트워크 성능을 모니터링하는 데 도움이 되는 클라우드 기반 [하이브리드 네트워크 모니터링](log-analytics-network-performance-monitor-performance-monitor.md) 솔루션입니다. 또한 [서비스 및 응용 프로그램 엔드포인트](log-analytics-network-performance-monitor-service-endpoint.md)에 대한 네트워크 연결을 모니터링하고 [Azure ExpressRoute의 성능을 모니터링하는](log-analytics-network-performance-monitor-expressroute.md) 데 도움이 됩니다. 

네트워크 성능 모니터는 트래픽 블랙홀링, 라우팅 오류와 같은 네트워크 문제와 기존 네트워크 모니터링 방법으로 감지할 수 없는 문제를 감지합니다. 이 솔루션은 네트워크 링크에 임계값이 위반되면 경고를 생성하고 사용자에게 알립니다. 또한 네트워크 성능 문제를 적시에 감지하고 문제의 원인을 특정 네트워크 세그먼트 또는 장치로 국한시킵니다. 

[네트워크 성능 모니터](https://docs.microsoft.com/azure/networking/network-monitoring-overview)에서 지원하는 다양한 기능에 대한 자세한 내용은 온라인으로 제공됩니다.

## <a name="set-up-and-configure-agents"></a>에이전트 설정 및 구성

### <a name="what-are-the-platform-requirements-for-the-nodes-to-be-used-for-monitoring-by-npm"></a>NPM에서 모니터링에 사용할 노드에 대한 플랫폼 요구 사항은 무엇입니까?
NPM의 다양한 기능에 대한 플랫폼 요구 사항은 아래와 같습니다.

- NPM의 성능 모니터 및 서비스 연결 모니터 기능은 Windows 서버(2008 SP1 이상) 및 Windows 데스크톱/클라이언트 운영 체제(Windows 10, Windows 8.1, Windows 8 및 Windows 7)를 지원합니다. 
- NPM의 ExpressRoute 모니터 기능은 Windows 서버(2008 SP1 이상) 운영 체제만 지원합니다.

### <a name="can-i-use-linux-machines-as-monitoring-nodes-in-npm"></a>NPM에서 Linux 머신을 모니터링 노드로 사용할 수 있나요?
Linux 기반 노드를 사용하여 네트워크를 모니터링하는 기능은 현재 비공개 미리 보기 중입니다. 더 자세한 내용은 계정 관리자에 문의하세요. 작업 영역 ID를 제공하면 당사에서 계속 진행하여 해당 기능을 사용하도록 설정합니다. Linux 에이전트는 NPM의 성능 모니터 기능에 대해서만 모니터링 기능을 제공하고 서비스 연결 모니터 및 ExpressRoute 모니터 기능에 대해서는 사용할 수 없습니다.

### <a name="what-are-the-size-requirements-of-the-nodes-to-be-used-for-monitoring-by-npm"></a>NPM에서 모니터링에 사용할 노드에 대한 크기 요구 사항은 얼마인가요?
네트워크를 모니터링하기 위해 노드 VM에서 NPM 솔루션을 실행하려면 노드에 최소 메모리 500 MB 및 코어 한 개가 있어야 합니다. NPM을 실행하기 위해 별도 노드를 사용할 필요는 없습니다. 다른 워크로드가 실행 중인 노드에서 이 솔루션을 실행할 수 있습니다. 이 솔루션에는 CPU를 5%를 초과하여 이용하는 경우 모니터링 프로세스를 중지하는 기능이 있습니다.

### <a name="to-use-npm-should-i-connect-my-nodes-as-direct-agent-or-through-system-center-operations-manager"></a>NPM을 사용하려면 내 노드를 직접 에이전트로 또는 System Center Operations Manager를 통해 연결해야 하나요?
성능 모니터 및 서비스 연결 모니터 기능 모두 [직접 에이전트로 연결된](log-analytics-agent-windows.md) 노드뿐만 아니라 [Operations Manager를 통해 연결된 노드도 지원합니다](log-analytics-om-agents.md).

ExpressRoute 모니터 기능의 경우 Azure 노드를 직접 에이전트로만 연결해야 합니다. Operations Manager를 통해 연결된 Azure 노드는 지원되지 않습니다. 온-프레미스 노드의 경우 직접 에이전트로 연결된 노드뿐만 아니라 Operations Manager를 통해 연결된 노드도 ExpressRoute 회로 모니터링에 대해 지원됩니다.

### <a name="which-protocol-among-tcp-and-icmp-should-be-chosen-for-monitoring"></a>모니터링을 위해 TCP와 ICMP 중 어느 프로토콜을 선택해야 하나요?
Windows 서버 기반 노드를 사용하여 네트워크를 모니터링하는 경우 더 나은 정확도 제공하는 TCP를 모니터링 프로토콜로 사용하는 것이 좋습니다. 

Windows 데스크톱/클라이언트 운영 체제 기반 노드의 경우 ICMP는 원시 소켓 상에서 TCP 데이터를 전송할 수 없는 플랫폼이므로 사용하지 않는 것이 좋습니다(NPM에서 네트워크 토폴로지를 검색하려면 이 전송 기능이 필요함).

각 프로토콜의 상대적 이점에 대한 세부 정보는 [여기](log-analytics-network-performance-monitor-performance-monitor.md#choose-the-protocol)에서 얻을 수 있습니다.

### <a name="how-can-i-configure-a-node-to-support-monitoring-using-tcp-protocol"></a>TCP 프로토콜을 사용하여 모니터링을 지원하려면 노드를 어떻게 구성할 수 있나요?
노드가 TCP 프로토콜을 사용하여 모니터링을 지원하려면: 
* 노드 플랫폼이 Windows 서버(2008 SP1 이상)인지 확인합니다.
* 노드에서 [EnableRules.ps1](https://aka.ms/npmpowershellscript) Powershell 스크립트를 실행합니다. 자세한 내용은 [지침](log-analytics-network-performance-monitor.md#configure-log-analytics-agents-for-monitoring)을 참조하세요.


### <a name="how-can-i-change-the-tcp-port-being-used-by-npm-for-monitoring"></a>NPM에서 모니터링을 위해 사용 중인 TCP 포트를 어떻게 변경할 수 있나요?
NPM에서 모니터링을 위해 사용하는 TCP 포트는 [EnableRules.ps1](https://aka.ms/npmpowershellscript) 스크립트를 실행하여 변경할 수 있습니다. 사용하려는 포트 번호를 매개 변수로 입력해야 합니다. 예를 들어 포트 8060에서 TCP를 사용하려면 `EnableRules.ps1 8060`을 실행합니다. 모니터링에 사용 중인 모든 노드에서 동일한 TCP 포트를 사용하는지 확인합니다.

스크립트는 Windows 방화벽만 로컬로 구성합니다. 네트워크 방화벽 또는 NSG(네트워크 보안 그룹) 규칙이 있는 경우, 해당 규칙이 NPM에서 사용하는 TCP 포트를 대상으로 하는 트래픽을 허용하는지 확인합니다.

### <a name="how-many-agents-should-i-use"></a>에이전트를 얼마나 많이 사용해야 하나요?
모니터링하려는 각 서브넷에 대해 최소 한 개의 에이전트를 사용해야 합니다.

## <a name="monitoring"></a>모니터링

### <a name="how-are-loss-and-latency-calculated"></a>손실 및 대기 시간 계산 방법
원본 에이전트는 원본과 대상 IP 조합 간의 모든 경로가 확실히 포함되도록 TCP SYN 요청(TCP를 모니터링 프로토콜로 선택한 경우) 또는 ICMP ECHO 요청(ICMP를 모니터링 프로토콜로 선택한 경우)을 정기적인 간격으로 대상 IP에 보냅니다. 각 경로의 손실 및 대기 시간을 계산하기 위해 수신된 패킷의 비율과 패킷 왕복 시간을 측정합니다. 이 데이터는 특정 폴링 간격의 IP 조합에 대한 손실 및 대기 시간의 집계된 값을 얻기 위해 폴링 간격 및 모든 경로에 대해 집계됩니다.

### <a name="with-what-frequency-does-the-source-agent-send-packets-to-the-destination-for-monitoring"></a>원본 에이전트가 패킷을 모니터링 대상에 보내는 빈도는 어느 정도입니까?
성능 모니터 및 ExpressRoute 모니터 기능의 경우 원본은 매 5초마다 패킷을 보내고 네트워크 측정값을 기록합니다. 이 데이터는 손실 및 대기 시간의 평균 및 최고 값을 계산하기 위해 3분의 폴링 간격에 대해 집계됩니다. 서비스 연결 모니터 기능의 경우 네트워크 측정을 위해 패킷을 보내는 빈도는 테스트를 구성하는 동안 특정 테스트에 대해 사용자가 입력하는 빈도에 의해 결정됩니다.

### <a name="how-many-packets-are-sent-for-monitoring"></a>모니터링을 위해 보내는 패킷 수는 얼마인가요?
원본 에이전트가 폴링 시 대상에 보내는 패킷 수는 적응형이며 네트워크 토폴로지마다 다를 수 있는 당사의 독자적인 알고리즘에 의해 결정됩니다. 원본과 대상 IP 조합 간의 네트워크 경로 수가 많을수록 보내는 패킷 수가 더 많습니다. 시스템은 원본과 대상 IP 조합 간의 모든 경로가 포함되었는지 확인합니다.

### <a name="how-does-npm-discover-network-topology-between-source-and-destination"></a>NPM은 원본과 대상 간의 네트워크 토폴로지를 어떻게 검색하나요?
NPM은 경로 추적을 기반으로 하는 독자적인 알고리즘을 사용하여 원본과 대상 간의 모든 경로와 홉을 검색합니다.

### <a name="does-npm-provide-routing-and-switching-level-info"></a>NPM은 라우팅 및 전환 수준 정보를 제공하나요 
NPM은 원본 에이전트와 대상 간의 모든 가능한 경로 검색할 수 있지만 특정 워크로드에서 보내는 패킷이 선택하는 경로에 대한 가시성을 제공하지 않습니다. 이 솔루션은 예상보다 더 많은 대기 시간을 추가하는 경로 및 기본 네트워크 홉을 식별하도록 도와 줄 수 있습니다.

### <a name="why-are-some-of-the-paths-unhealthy"></a>경로 중 일부의 상태가 비정상인 이유는 무엇인가요?
원본 IP와 대상 IP 간에 서로 다른 네트워크 경로가 존재할 수 있으며 각 경로의 손실 및 대기 시간 값이 다를 수 있습니다. NPM은 손실 및/또는 대기 시간의 값이 모니터링 구성에 설정된 해당 임계값보다 큰 경로를 비정상으로 표시합니다(빨간색으로 표시).

### <a name="what-does-a-hop-in-red-color-signify-in-the-network-topology-map"></a>네트워크 토폴로지 맵에서 빨간색의 홉이 나타내는 것은 무엇인가요?
홉이 빨간색이면 적어도 한 개의 비정상 경로의 일부임을 나타냅니다. NPM은 경로를 비정상으로 표시할 뿐, 각 경로의 정상/비정상 상태를 분리하지 않습니다. 문제가 있는 홉을 식별하려면 홉별 대기 시간을 보고 예상하는 대기 시간보다 더 많이 추가하는 홉을 분리할 수 있습니다.

### <a name="how-does-fault-localization-in-performance-monitor-work"></a>성능 모니터링의 고장 지역화는 어떻게 작동하나요?
NPM은 확률적 메커니즘을 사용하여 홉이 속한 비정상 경로 수를 기반으로 각 네트워크 경로, 네트워크 세그먼트 및 구성 요소 네트워크 홉에 고장 확률을 할당합니다. 네트워크 세그먼트와 홉이 더 많은 비정상 경로의 일부가 됨에 따라 그와 연결된 고장 확률도 높아집니다. 서로 연결된 NPM 에이전트가 있는 노드 수가 많은 경우 고장 확률 계산을 위한 데이터 요소가 증가하므로 이 알고리즘이 가장 적합합니다.

### <a name="how-can-i-create-alerts-in-npm"></a>NPM에서 경고를 얼마나 많이 만들 수 있나요?
[설명서의 경고 섹션](https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor#alerts)에서 단계별 지침을 참조하세요.

### <a name="can-npm-monitor-routers-and-servers-as-individual-devices"></a>NPM은 라우터 및 서버를 개별 장치로 모니터링할 수 있나요?
NPM은 원본 IP와 대상 IP 간의 기본 네트워크 홉(스위치, 라우터, 서버 등)의 IP와 호스트 이름만 식별합니다. 또한 이렇게 식별된 홉 간의 대기 시간도 식별합니다. 이러한 기본 홉을 개별적으로 모니터링하지 않습니다.

### <a name="can-npm-be-used-to-monitor-network-connectivity-between-azure-and-aws"></a>NPM을 사용하여 Azure와 AWS 간의 네트워크 연결을 모니터링할 수 있나요?
예. 자세한 내용은 [NPM을 사용하여 Azure, AWS 및 온-프레미스 네트워크 모니터링](https://blogs.technet.microsoft.com/msoms/2016/08/30/monitor-on-premises-cloud-iaas-and-hybrid-networks-using-oms-network-performance-monitor/) 문서를 참조하세요.

### <a name="is-the-expressroute-bandwidth-usage-incoming-or-outgoing"></a>ExpressRoute 대역폭 사용량은 들어오거나 나가는 사용량인가요?
대역폭 사용량은 들어오고 나가는 대역폭의 합계이며, 비트/초 단위로 표시됩니다.

### <a name="can-we-get-incoming-and-outgoing-bandwidth-information-for-the-expressroute"></a>ExpressRoute에 대해 들어오고 나가는 대역폭 정보를 얻을 수 있나요?
주 및 보조 대역폭 둘 다에 대해 들어오고 나가는 값을 캡처할 수 있습니다.

피어링 수준 정보를 보려면 로그 검색에서 아래와 같은 쿼리를 사용합니다.

    NetworkMonitoring 
    | where SubType == "ExpressRoutePeeringUtilization"
    | project CircuitName,PeeringName,PrimaryBytesInPerSecond,PrimaryBytesOutPerSecond,SecondaryBytesInPerSecond,SecondaryBytesOutPerSecond
  
회로 수준 정보를 보려면 아래와 같은 쿼리를 사용합니다. 

    NetworkMonitoring 
    | where SubType == "ExpressRouteCircuitUtilization"
    | project CircuitName,PrimaryBytesInPerSecond, PrimaryBytesOutPerSecond,SecondaryBytesInPerSecond,SecondaryBytesOutPerSecond

### <a name="which-regions-are-supported-for-npms-performance-monitor"></a>NPM의 성능 모니터에 대해 지원되는 하위 지역은 어느 지역인가요?
NPM은 [지원되는 하위 지역](log-analytics-network-performance-monitor.md#supported-regions) 중 하나에서 호스팅되는 작업 영역에서 세계 어느 부분에 있는 네트워크 사이의 연결도 모니터링할 수 있습니다.

### <a name="which-regions-are-supported-for-npms-service-connectivity-monitor"></a>NPM의 서비스 연결 모니터에 대한 지원되는 하위 지역은 어느 지역인가요?
NPM은 [지원되는 하위 지역](log-analytics-network-performance-monitor.md#supported-regions) 중 하나에서 호스팅되는 작업 영역에서 세계 어느 부분에 있는 네트워크 사이의 연결도 모니터링할 수 있습니다.

### <a name="which-regions-are-supported-for-npms-expressroute-monitor"></a>NPM의 ExpressRoute 모니터에 대해 지원되는 하위 지역은 어느 하위 지역인가요?
NPM은 어떤 Azure 하위 지역에 위치한 ExpressRoute 회로도 모니터링할 수 있습니다. NPM에 합류하려면 Log Analytics 작업 영역을 [지원되는 하위 지역](/azure/expressroute/how-to-npm#regions) 중 하나에서 호스팅해야 합니다.

## <a name="troubleshoot"></a>문제 해결

### <a name="why-are-some-of-the-hops-marked-as-unidentified-in-the-network-topology-view"></a>홉의 일부가 네트워크 토폴로지 보기에서 식별되지 않은 것으로 표시되는 이유는 무엇입니까?
NPM은 경로 추적의 수정된 버전을 사용하여 원본 에이전트에서 대상까지 토폴로지를 검색합니다. 식별되지 않은 홉은 네트워크 홉이 원본 에이전트의 경로 추적 요청에 응답하지 않았음을 나타냅니다. 연속 3개의 네트워크 홉이 에이전트의 경로 추적에 응답하지 않는 경우 솔루션은 반응하지 않는 홉을 식별되지 않은 것으로 표시하고 더 이상 홉의 검색을 시도하지 않습니다.

홉은 아래 시나리오 중 하나 이상에서 경로 추적에 응답하지 않을 수 있습니다.

* 라우터가 자신의 ID를 찾을 수 없도록 구성되었습니다.
* 네트워크 장치가 ICMP_TTL_EXCEEDED 트래픽을 허용하지 않습니다.
* 방화벽이 네트워크 장치에서 ICMP_TTL_EXCEEDED 응답을 차단합니다.

### <a name="the-solution-shows-100-loss-but-there-is-connectivity-between-the-source-and-destination"></a>솔루션은 100% 손실을 보여 주지만 원본과 대상 사이에 연결이 있습니다.
이 상황은 호스트 방화벽 또는 중간 방화벽(네트워크 방화벽 또는 Azure NSG)이 NPM에서 모니터링하는 데 사용 중인 포트(고객이 변경하지 않았다면 기본적으로 포트 8084) 상에서 원본 에이전트와 대상 간의 통신을 차단합니다.

* 호스트 방화벽이 필요한 포트에서 통신을 차단하지 않는지 확인하려면 네트워크 성능 모니터 -> 구성 -> 노드 보기에서 원본 및 대상 노드의 정상/비정상 상태를 봅니다. 
  비정상 상태인 경우 지침을 보고 정정 작업을 실행합니다. 노드가 정상인 경우 b 단계로 이동합니다. 알아봅니다.
* 중간 네트워크 방화벽 또는 Azure NSG가 필요한 포트에서 통신을 차단하지 않는지 확인하려면 아래 지침을 통해 제3자 PsPing 유틸리티를 사용합니다.
  * PsPing 유틸리티는 [여기](https://technet.microsoft.com/sysinternals/psping.aspx)에서 다운로드할 수 있습니다. 
  * 원본 노드에서 다음 명령을 실행합니다.
    * psping -n 15 \<대상 노드 IP 주소\>:포트 번호. 기본적으로 NPM은 8084 포트를 사용합니다. EnableRules.ps1 스크립트를 사용하여 이 명령을 명시적으로 변경한 경우 사용하는 사용자 지정 포트 번호를 입력합니다. 이는 Azure 머신에서 온-프레미스로의 ping입니다.
* ping이 성공적인지 확인합니다. 그렇지 않은 경우 중간 네트워크 방화벽 또는 Azure NSG가 이 포트에서 트래픽을 차단함을 나타냅니다.
* 이제 대상 노드에서 원본 노드 IP로 명령을 실행합니다.


### <a name="there-is-loss-from-node-a-to-b-but-not-from-node-b-to-a-why"></a>노드 A에서 B로는 손실이 있지만 노드 B에서 A로는 손실이 없는 이유가 무엇인가요?
A에서 B로의 네트워크 경로는 B에서 A로의 네트워크 경로와 다를 수 있으므로 손실 및 대기 시간에 대해 서로 다른 값이 관찰될 수 있습니다.

### <a name="why-are-all-my-expressroute-circuits-and-peering-connections-not-being-discovered"></a>나의 ExpressRoute 회로 및 피어링 연결 중 일부가 검색되지 않는 이유는 무엇인가요?
이 상황은 회로와 피어링 연결이 여러 구독에 걸쳐 배포된 경우에 발생할 수 있습니다. NPM은 ExpressRoute에 연결된 VNET이 NPM 작업 영역에 연결된 것과 동일한 구독에 있는 ExpressRoute 비공개 피어링 연결만 검색합니다. 또한 NPM은 연결된 ExpressRoute 회로가 NPM 작업 영역에 연결된 것과 동일한 구독에 있는 Microsoft 피어링 연결만 검색합니다. 아래 예제에서 이에 대해 자세히 설명할 수 있습니다.

 구독 A에 2개의 VNET-VNET A가 있고 구독 B에 VNET B가 있는 경우 구독 C의 ExpressRoute에 연결했습니다. 또한 구독 C에 또 다른 VNET -VNET C가 있으며, ER에도 구독 C에 MS 피어링이 있습니다. 

그렇다면

* NPM 작업 영역이 구독 A에 연결된 경우 ER과 VNET A를 통해서만 연결을 모니터링할 수 있습니다.
* NPM 작업 영역이 구독 B에 연결된 경우 ER과 VNET B를 통해서만 연결을 모니터링할 수 있습니다.
* NPM 작업 영역이 구독 C에 연결된 경우 ER과 VNET C 및 MS 피어링을 통해 연결을 모니터링할 수 있습니다.

구독 간의 교차 지원은 곧 사용할 수 있게 될 것입니다. 그렇게 되면 서로 다른 구독의 모든 ExpressRoute 비공개 및 Microsoft 피어링 연결을 하나의 작업 영역에서 모니터링할 수 있게 될 것입니다.
### <a name="the-er-monitor-capability-has-a-diagnostic-message-traffic-is-not-passing-through-any-circuit-what-does-that-mean"></a>ER 모니터 기능에는 "트래픽이 어떤 회로도 통과하지 않습니다" 진단 메시지가 있습니다. 이는 무엇을 의미하나요?

온-프레미스 노드와 Azure 노드 간에 정상 연결이 있지만 트래픽이 NPM에서 모니터링하도록 구성된 ExpressRoute 회로를 통해 이동하지 않는 시나리오가 있을 수 있습니다. 

이 상황은 다음과 같은 경우에 발생할 수 있습니다.

* ER 회로가 다운되었습니다.
* 경로 필터가 의도한 ExpressRoute 회로보다 다른 경로(예: VPN 연결 또는 다른 ExpressRoute 회로)에 우선권을 부여하는 방법으로 구성되었습니다. 
* 모니터링 구성에서 ExpressRoute 회로를 모니터링하기 위해 선택한 온-프레미스 및 Azure 노드에, 의도한 ExpressRoute 회로를 통한 상호 연결이 없습니다. 모니터링하려는 ExpressRoute 회로를 통한 상호 연결이 있는 올바른 노드를 선택했는지 확인합니다.

### <a name="while-configuring-monitoring-of-my-expressroute-circuit-the-azure-nodes-are-not-being-detected"></a>내 ExpressRoute 회로의 모니터링을 구성하는 동안 Azure 노드가 감지되지 않습니다.
이 상황은 Azure 노드가 Operations Manager를 통해 연결된 경우 발생할 수 있습니다. ExpressRoute 모니터 기능은 직접 에이전트로 연결된 Azure 노드만 지원합니다.

### <a name="i-cannot-discover-by-expressroute-circuits-in-the-oms-portal"></a>OMS 포털에서 ExpressRoute 회로에 의해 검색할 수 없습니다.
NPM은 Azure Portal뿐만 아니라 OMS 포털에서도 사용할 수 있지만 ExpressRoute 모니터 기능의 회로 검색은 Azure Portal을 통해서만 작동합니다. 회로가 Azure Portal을 통해 검색된 후 두 포털 중 하나에서 이 기능을 사용할 수 있습니다. 

### <a name="in-the-service-connectivity-monitor-capability-the-service-response-time-network-loss-as-well-as-latency-are-shown-as-na"></a>서비스 연결 모니터 기능에서 서비스 응답 시간, 네트워크 연결 끊김뿐만 아니라 대기 시간도 NA로 표시됩니다.
이 상황은 다음 중 하나 이상이 해당하는 경우에 발생할 수 있습니다.

* 서비스가 다운되었습니다.
* 서비스에 대한 네트워크 연결을 확인하는 데 사용되는 노드가 중단되었습니다.
* 테스트 구성에 입력한 대상이 올바르지 않습니다.
* 노드는 네트워크에 연결되지 않습니다.

### <a name="in-the-service-connectivity-monitor-capability-a-valid-service-response-time-is-shown-but-network-loss-as-well-as-latency-are-shown-as-na"></a>서비스 연결 모니터 기능에서 유효한 서비스 응답 시간이 표시되지만 네트워크 연결 끊김뿐만 아니라 대기 시간도 NA로 표시됩니다.
 이 상황은 다음 중 하나 이상이 해당하는 경우에 발생할 수 있습니다.

* 서비스에 대한 네트워크 연결을 확인하는 데 사용되는 노드가 Windows 클라이언트 컴퓨터인 경우 대상 서비스가 ICMP 요청을 차단하거나 네트워크 방화벽이 노드에서 시작되는 ICMP 요청을 차단하게 됩니다.
* 네트워크 측정 수행 확인란이 테스트 구성에서 비어 있습니다.

### <a name="in-the-service-connectivity-monitor-capability-the-service-response-time-is-na-but-network-loss-as-well-as-latency-are-valid"></a>서비스 연결 모니터 기능에서 서비스 응답 시간이 NA이지만 네트워크 연결 끊김뿐만 아니라 대기 시간도 유효합니다.
이 상황은 대상 서비스가 웹 응용 프로그램이 아니지만 테스트가 웹 테스트로 구성된 경우에 발생할 수 있습니다. 테스트 구성을 편집하고, 테스트 형식을 웹이 아닌 네트워크로 선택합니다.

## <a name="miscellaneous"></a>기타

### <a name="is-there-a-performance-impact-on-the-node-being-used-for-monitoring"></a>모니터링에 사용하는 노드의 성능에 영향이 있나요?
NPM 프로세스는 호스트 CPU 리소스의 5%를 초과하여 이용하는 경우 중지하도록 구성됩니다. 이는 성능에 영향을 주지 않고 노드를 일상적인 워크로드에 계속 사용할 수 있도록 하기 위한 것입니다.

### <a name="does-npm-edit-firewall-rules-for-monitoring"></a>NPM은 방화벽 모니터링 규칙을 편집하나요?
NPM은 에이전트들이 지정된 포트에서 서로 TCP 연결을 만들 수 있도록 EnableRules.ps1 Powershell 스크립트를 실행하는 노드에 대해서만 로컬 Windows 방화벽 규칙을 만듭니다. 솔루션은 네트워크 방화벽 규칙 또는 NSG(네트워크 보안 그룹) 규칙을 수정하지 않습니다.

### <a name="how-can-i-check-the-health-of-the-nodes-being-used-for-monitoring"></a>모니터링에 사용하는 노드의 상태를 어떻게 확인할 수 있나요?
다음 보기에서 모니터링에 사용하는 노드의 정상/비정상 상태를 볼 수 있습니다. 네트워크 성능 모니터 -> 구성 -> 노드. 노드가 비정상인 경우 오류 세부 정보를 보고 권장 작업을 실행할 수 있습니다.

### <a name="can-npm-report-latency-numbers-in-microseconds"></a>NPM은 대기 시간 숫자를 마이크로초 단위로 보고할 수 있습니까?
NPM은 UI 및 밀리초 단위의 대기 시간 숫자를 반올림합니다. 동일한 데이터를 더 높은 세분성(가끔 최대 소수 네 자리)으로 저장됩니다.

## <a name="next-steps"></a>다음 단계

- [Azure의 네트워크 성능 모니터 솔루션](log-analytics-network-performance-monitor.md)을 참조하여 네트워크 성능 모니터에 대해 알아봅니다.