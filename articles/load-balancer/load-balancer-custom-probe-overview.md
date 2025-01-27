---
title: Azure Load Balancer 상태 프로브를 사용하여 서비스에 대해 확장된 고가용성을 제공합니다.
titlesuffix: Azure Load Balancer
description: 상태 프로브를 사용하여 Load Balancer 뒤의 인스턴스를 모니터링하는 방법을 알아봅니다.
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
ms.date: 05/07/2019
ms.author: allensu
ms.openlocfilehash: 75009530940a0cce7adb8469ead5f55f509a1faa
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68275344"
---
# <a name="load-balancer-health-probes"></a>Load Balancer 상태 프로브

Azure Load Balancer는 부하 분산 규칙에 사용하기 위한 상태 프로브를 제공합니다.  상태 프로브 구성 및 프로브 응답은 새 흐름을 받을 백 엔드 풀 인스턴스를 결정합니다. 상태 프로브를 사용하여 백 엔드 인스턴스에서 애플리케이션 오류를 검색할 수 있습니다. 또한 상태 프로브에 대한 사용자 지정 응답을 생성하고 흐름 제어를 위해 상태 프로브를 사용하여 부하 또는 계획된 가동 중지 시간을 관리할 수 있습니다. 상태 프로브가 실패하면 Load Balancer에서 각 비정상 인스턴스에 대한 새 연결 보내기를 중지합니다.

상태 프로브는 여러 프로토콜을 지원합니다. 특정 프로토콜을 지원하기 위한 특정 유형의 상태 프로브 가용성은 Load Balancer SKU에 따라 다릅니다.  이 서비스의 동작 또한 Load Balancer SKU에 따라 다릅니다.

| | 표준 SKU | 기본 SKU |
| --- | --- | --- |
| [상태 프로브 유형](#types) | TCP, HTTP, HTTPS | TCP, HTTP |
| [프로브 다운 동작](#probedown) | 모든 프로브가 다운되고, 모든 TCP 흐름이 계속됩니다. | 모든 프로브는 모든 TCP 흐름이 만료 됩니다. | 

> [!IMPORTANT]
> Load Balancer 상태 프로브는 168.63.129.16 IP 주소에서 시작되며, 프로브에서 인스턴스를 표시하기 위해 차단되지 않아야 합니다.  자세한 내용은 [원본 IP 주소 프로브](#probesource)를 검토하세요.

## <a name="types"></a>프로브 유형

다음 세 가지 프로토콜을 사용하여 수신기에 대한 상태 프로브를 구성할 수 있습니다.

- [TCP 수신기](#tcpprobe)
- [HTTP 엔드포인트](#httpprobe)
- [HTTPS 엔드포인트](#httpsprobe)

사용 가능한 상태 프로브 유형은 선택한 Load Balancer SKU에 따라 달라집니다.

|| TCP | HTTP | HTTPS |
| --- | --- | --- | --- |
| 표준 SKU |    &#9989; |   &#9989; |   &#9989; |
| 기본 SKU |   &#9989; |   &#9989; | &#10060; |

상태 프로브는 실제 서비스가 제공되는 포트를 포함하여 백 엔드 인스턴스의 모든 포트를 관찰할 수 있습니다.

### <a name="tcpprobe"></a> TCP 프로브

TCP 프로브는 정의된 포트를 통해 3방향 개방형 TCP 핸드셰이크를 수행하여 연결을 시작합니다.  TCP 프로브는 4방향 폐쇄형 TCP 핸드셰이크를 사용하여 연결을 종료합니다.

최소 프로브 간격은 5초이고, 비정상 응답의 최소 수는 2입니다.  모든 간격의 총 지속 시간은 120초를 초과할 수 없습니다.

TCP 프로브가 실패하는 경우는 다음과 같습니다.
* 인스턴스의 TCP 수신기가 제한 시간 동안 전혀 응답하지 않습니다.  프로브는 프로브 다운을 표시하기 전에 응답하지 않도록 구성된 실패한 프로브 요청의 수에 따라 표시됩니다.
* 프로브가 인스턴스에서 TCP 재설정을 받습니다.

#### <a name="resource-manager-template"></a>Resource Manager 템플릿

```json
    {
      "name": "tcp",
      "properties": {
        "protocol": "Tcp",
        "port": 1234,
        "intervalInSeconds": 5,
        "numberOfProbes": 2
      },
```

### <a name="httpprobe"></a> <a name="httpsprobe"></a> HTTP/HTTPS 프로브

> [!NOTE]
> HTTPS 프로브는 [표준 Load Balancer](load-balancer-standard-overview.md)에만 제공됩니다.

HTTP 및 HTTPS 프로브는 TCP 프로브에서 구축되고, 지정된 경로가 포함된 HTTP GET을 실행합니다. 두 프로브 모두 HTTP GET에 대한 상대 경로를 지원합니다. HTTPS 프로브는 TLS(전송 계층 보안, SSL이라고도 함)가 추가되었다는 점을 제외하면 HTTP 프로브와 동일합니다. 인스턴스에서 제한 시간 내에 200 HTTP 상태로 응답하면 상태 프로브가 표시됩니다.  이 상태 프로브는 기본적으로 구성된 상태 프로브 포트를 15초마다 확인하려고 합니다. 최소 프로브 간격은 5초입니다. 모든 간격의 총 지속 시간은 120초를 초과할 수 없습니다.

HTTP/HTTPS 프로브는 상태 프로브를 나타내려는 경우에도 유용할 수 있습니다.  프로브 포트가 서비스 자체의 수신기이기도 한 경우 부하 분산 장치 순환에서 인스턴스를 제거하는 고유한 논리를 구현하세요. 예를 들어 CPU 백분율이 90%를 초과하고 200 이외의 HTTP 상태를 반환하는 경우 인스턴스를 제거하도록 결정할 수 있습니다. 

Cloud Services를 사용하고 w3wp.exe를 사용하는 웹 역할이 있는 경우 웹 사이트의 자동 모니터링도 수행할 수 있습니다. 웹 사이트 코드에서 실패하면 부하 분산 장치 프로브로 200이 아닌 상태를 반환합니다.

HTTP/HTTPS 프로브가 실패하는 경우는 다음과 같습니다.
* 프로브 엔드포인트에서 200 이외의 HTTP 응답 코드(예: 403, 404 또는 500)를 반환합니다. 이 경우 상태 프로브가 즉시 가동 중단 상태로 표시됩니다. 
* 프로브 엔드포인트는 31초의 제한 시간 동안 전혀 응답하지 않습니다. 프로브가 실행 중이 아니라고 표시되고, 모든 시간 제한 간격의 합계에 도달하기 전에 다중 프로브 요청이 응답하지 않을 수 있습니다.
* 프로브 엔드포인트에서 TCP 재설정을 통해 연결을 닫습니다.

#### <a name="resource-manager-templates"></a>리소스 관리자 템플릿

```json
    {
      "name": "http",
      "properties": {
        "protocol": "Http",
        "port": 80,
        "requestPath": "/",
        "intervalInSeconds": 5,
        "numberOfProbes": 2
      },
```

```json
    {
      "name": "https",
      "properties": {
        "protocol": "Https",
        "port": 443,
        "requestPath": "/",
        "intervalInSeconds": 5,
        "numberOfProbes": 2
      },
```

### <a name="guestagent"></a>게스트 에이전트 프로브(클래식만)

클라우드 서비스 역할(작업자 역할 및 웹 역할)은 기본적으로 게스트 에이전트를 사용하여 프로브를 모니터링합니다.  게스트 에이전트 프로브는 최후의 구성입니다.  상태 프로브는 항상 TCP 또는 HTTP 프로브를 통해 명시적으로 사용합니다. 게스트 에이전트 프로브는 대부분의 애플리케이션 시나리오에 대해 명시적으로 정의된 프로브만큼 효과적이지 않습니다.

게스트 에이전트 프로브는 VM 내부의 게스트 에이전트를 검사합니다. 그런 다음 인스턴스가 준비 상태인 경우에만 수신 대기하며 HTTP 200 OK로 응답합니다. 다른 상태는 사용 중, 재생 중 또는 중지 중입니다.

자세한 내용은 [상태 프로브에 대한 서비스 정의 파일(csdef) 구성](https://msdn.microsoft.com/library/azure/ee758710.aspx) 또는 [클라우드 서비스를 위한 공용 부하 분산 장치 만들기 시작](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services)을 참조하세요.

게스트 에이전트가 HTTP 200 OK로 응답하는 데 실패하면 부하 분산 장치가 인스턴스를 응답하지 않음으로 표시합니다. 그런 다음, 해당 인스턴스에 흐름을 보내지 않도록 중지합니다. 부하 분산 장치는 인스턴스를 계속 검사합니다. 

게스트 에이전트에서 200 HTTP로 응답하면 부하 분산 장치는 새 흐름을 해당 인스턴스에 다시 보냅니다.

웹 역할을 사용하는 경우 웹 사이트 코드는 일반적으로 w3wp.exe에서 실행되며 이는 Azure 패브릭 또는 게스트 에이전트에서 모니터링하지 않습니다. w3wp.exe(예: HTTP 500 응답)의 실패는 게스트 에이전트에 보고되지 않습니다. 결과적으로 부하 분산 장치는 해당 인스턴스를 순환에서 제거하지 않습니다.

<a name="health"></a>
## <a name="probehealth"></a>상태 프로브

TCP, HTTP 및 HTTPS 상태 프로브가 정상으로 간주되고 역할 인스턴스를 정상 상태로 표시하는 경우는 다음과 같습니다.

* VM이 부팅된 후에는 상태 프로브가 성공 상태로 표시됩니다.
* 역할 인스턴스를 정상 상태로 표시하는 데 필요한 지정된 수의 프로브를 달성했습니다.

> [!NOTE]
> 프로브의 상태가 변동되는 경우 부하 분산 디바이스는 역할 인스턴스를 다시 정상 상태로 전환하기 전에 좀 더 오래 기다립니다. 이러한 추가 대기 시간은 의도적인 정책으로, 사용자 및 인프라를 보호합니다.

## <a name="probe-count-and-timeout"></a>프로브 수 및 시간 제한

프로브 동작은 다음 사항에 따라 달라집니다.

* 인스턴스가 가동 상태로 표시될 수 있는 성공한 프로브의 수
* 인스턴스가 가동 중지 상태로 표시되는 실패한 프로브의 수

지정된 제한 시간 및 간격 값은 인스턴스를 가동 중지 상태로 표시할지 또는 가동 상태로 표시할지를 결정합니다.

## <a name="probedown"></a>프로브 다운 동작

### <a name="tcp-connections"></a>TCP 연결

정상적인 나머지 백 엔드 인스턴스가 TCP로 새로 연결됩니다.

백 엔드 인스턴스의 상태 프로브가 실패하더라도 이 백 엔드 인스턴스에 설정된 TCP 연결은 계속 유지됩니다.

백 엔드 풀의 모든 인스턴스에 대한 모든 프로브가 실패하면 새 흐름을 백 엔드 풀로 보내지 않습니다. 표준 Load Balancer는 설정된 TCP 흐름이 계속되도록 허용합니다.  기본 Load Balancer는 백 엔드 풀에 대한 기존의 모든 TCP 흐름을 종료합니다.
 
Load Balancer는 통과 서비스(TCP 연결을 종료하지 않음)이며, 클라이언트와 VM의 게스트 OS 및 애플리케이션 사이에 항상 흐름이 존재합니다. 흐름을 받고 SYN-ACK로 응답할 정상적인 백 엔드 인스턴스가 없으므로 모든 프로브가 가동 중단된 풀로 인해 프런트 엔드에서 TCP 연결 열기 시도(SYN)에 응답하지 않게 됩니다.

### <a name="udp-datagrams"></a>UDP 데이터그램

UDP 데이터 그램은 정상적인 백 엔드 인스턴스에 전달됩니다.

UDP는 비연결형이며 UDP에 대해 추적된 흐름 상태가 없습니다. 백 엔드 인스턴스의 상태 프로브가 실패하면 기존 UDP 흐름이 백 엔드 풀에서 정상적인 다른 인스턴스로 이동할 수 있습니다.

백 엔드 풀의 모든 인스턴스에 대한 모든 프로브가 실패하면 기본 및 표준 Load Balancer에 대한 기존 UDP 흐름이 종료됩니다.

<a name="source"></a>
## <a name="probesource"></a>원본 IP 주소 프로브

Load Balancer는 내부 상태 모델에 분산 프로빙 서비스를 사용합니다. 프로빙 서비스는 VM이 있는 각 호스트에 상주하며, 필요할 때 고객 구성별로 상태 프로브를 생성하도록 프로그래밍할 수 있습니다. 상태 프로브 트래픽은 상태 프로브를 생성하는 프로빙 서비스와 고객 VM 간을 직접 이동합니다. 모든 Load Balancer 상태 프로브는 해당 원본으로 168.63.129.16 IP 주소에서 시작됩니다.  RFC1918 공간이 아닌 VNet 내의 IP 주소 공간을 사용할 수 있습니다.  전역적으로 예약된 Microsoft 소유의 IP 주소를 사용하면 IP 주소가 VNet 내에서 사용하는 IP 주소 공간과 충돌할 가능성이 줄어듭니다.  이 IP 주소는 모든 지역에서 동일하고 변경되지 않으며, 내부 Azure 플랫폼 구성 요소만 이 IP 주소에서 패킷을 소싱할 수 있으므로 보안 위험을 초래하지 않습니다. 

AzureLoadBalancer 서비스 태그는 [네트워크 보안 그룹](../virtual-network/security-overview.md)에서 이 원본 IP 주소를 식별하고 기본적으로 상태 프로브 트래픽을 허용합니다.

상태 프로브를 Load Balancer 하는 것 외에도 [다음 작업은이 IP 주소를 사용](../virtual-network/what-is-ip-address-168-63-129-16.md)합니다.

- VM 에이전트에서 플랫폼과 통신하도록 설정하여 "준비" 상태에 있음을 알립니다.
- DNS 가상 서버와 통신하도록 설정하여 사용자 지정 DNS 서버를 정의하지 않은 고객에게 필터링된 이름 확인을 제공합니다.  이 필터링을 통해 고객은 배포의 호스트 이름만 확인할 수 있습니다.
- VM이 Azure의 DHCP 서비스에서 동적 IP 주소를 가져올 수 있도록 합니다.

## <a name="design"></a> 디자인 지침

상태 프로브는 서비스를 탄력적으로 유지하고 확장할 수 있도록 하는 데 사용됩니다. 구성 또는 디자인 패턴이 잘못되면 서비스의 가용성 및 확장성에 영향을 줄 수 있습니다. 이 전체 문서를 검토하고, 이 프로브 응답이 가동 중지 또는 가동 상태로 표시될 경우 시나리오에 미치는 영향과 애플리케이션 시나리오의 가용성에 어떤 영향을 미칠지 고려하세요.

애플리케이션에 대한 상태 모델을 디자인할 경우, 해당 인스턴스의 상태 __및__ 사용자가 제공하는 애플리케이션 서비스를 반영하는 백 엔드 인스턴스에서 포트를 프로브해야 합니다.  애플리케이션 포트와 프로브 포트는 동일하지 않아도 됩니다.  일부 시나리오에서는 프로브 포트가 애플리케이션이 서비스를 제공하는 포트와 달라야 할 수 있습니다.  

때때로 애플리케이션이 애플리케이션 상태를 검색할 뿐만 아니라 인스턴스가 새 흐름을 수신할지 여부를 Load Balancer에 직접 신호로 알리는 상태 프로브 응답을 생성하는 것이 유용할 수 있습니다.  상태 프로브가 실패하도록 하여 애플리케이션이 새 흐름에 대한 역압 및 스로틀 전달을 생성할 수 있도록 프로브 응답을 조작하거나 애플리케이션의 유지 관리를 준비하고 시나리오 드레이닝을 시작할 수 있습니다.  표준 Load Balancer를 사용하는 경우 [프로브의 작동 중지](#probedown) 신호가 발생하면 유휴 시간 종료 또는 연결 종료가 나타날 때까지 항상 TCP 흐름이 계속됩니다. 

UDP 부하 분산을 위해서는 백 엔드 인스턴스에서 사용자 지정 상태 프로브 신호를 생성하고, 해당 수신기를 대상으로 하는 TCP, HTTP 또는 HTTPS 상태 프로브를 사용하여 UDP 애플리케이션의 상태를 반영해야 합니다.

[표준 Load Balancer](load-balancer-standard-overview.md)와 함께 [HA 포트 부하 분산 규칙](load-balancer-ha-ports-overview.md)을 사용하면, 모든 포트의 부하가 분산되고 단일 상태 프로브 응답에는 전체 인스턴스의 상태가 반영되어야 합니다.

이 구성을 사용할 경우 시나리오에서 연속 오류로 이어질 수 있으므로 VNet의 다른 인스턴스로 상태 프로브를 받는 인스턴스를 통해 상태 프로브를 변환하거나 프록시하지 않아야 합니다.  타사 어플라이언스 세트가 Load Balancer 리소스의 백 엔드 풀에 배포되어 어플라이언스에 대한 확장 및 중복성을 제공하고, 타사 어플라이언스가 어플라이언스 뒤에 있는 다른 가상 머신으로 프록시 또는 변환하는 포트를 프로브하도록 상태 프로브가 구성되어 있다고 가정합니다.  사용 중인 동일한 포트를 프로브하여 요청을 변환하거나 어플라이언스 뒤의 다른 가상 머신으로 프록시하려는 경우, 어플라이언스 뒤의 단일 가상 머신에서 어떤 프로브 응답이 발생하더라도 어플라이언스 자체는 중단 상태로 표시됩니다. 이와 같이 구성하면 어플라이언스 뒤에 단일 백 엔드 인스턴스가 있게 되므로 전체 애플리케이션 시나리오가 연속적으로 실패할 수 있습니다.  간헐적 프로브 실패는 Load Balancer가 원래 대상(어플라이언스 인스턴스)을 작동 중단으로 표시하도록 하고, 전체 애플리케이션 시나리오를 비활성화하도록 하는 트리거로 작용할 수 있습니다. 대신 어플라이언스 자체의 상태를 프로브합니다. 상태 신호를 판별하기 위한 프로브는 NVA(네트워크 가상 어플라이언스) 시나리오에서 중요한 고려 사항이며, 이러한 시나리오에 적합한 상태 신호는 애플리케이션 공급업체에 문의해야 합니다.

방화벽 정책에서 프로브의 [원본 IP](#probesource)를 허용하지 않으면 인스턴스에 연결할 수 없으므로 상태 프로브가 실패하게 됩니다.  차례로 상태 프로브 실패로 인해 Load Balancer에서 인스턴스를 표시합니다.  이 잘못된 구성으로 인해 부하 분산된 애플리케이션 시나리오가 실패할 수 있습니다.

Load Balancer의 상태 프로브에서 인스턴스를 표시하려면 모든 Azure [네트워크 보안 그룹](../virtual-network/security-overview.md) 및 로컬 방화벽 정책에서 이 IP 주소를 **허용해야 합니다**.  기본적으로, 모든 네트워크 보안 그룹은 상태 프로브 트래픽을 허용하기 위해 [서비스 태그](../virtual-network/security-overview.md#service-tags) AzureLoadBalancer를 포함합니다.

상태 프로브 실패를 테스트하거나 개별 인스턴스를 표시하려는 경우 [네트워크 보안 그룹](../virtual-network/security-overview.md)을 사용하여 상태 프로브(대상 포트 또는 [원본 IP](#probesource))를 명시적으로 차단하고 프로브 실패를 시뮬레이트할 수 있습니다.

168.63.129.16이 포함된 Microsoft 소유의 IP 주소 범위를 사용하여 VNet을 구성하지 않도록 합니다.  이러한 구성은 상태 프로브의 IP 주소와 충돌하여 시나리오 실패를 야기할 수 있습니다.

VM에 여러 인터페이스가 있는 경우 받은 인터페이스의 프로브에 응답하도록 보장해야 합니다.  인터페이스당 VM에서 이 주소를 변환하려면 네트워크 주소를 소싱해야 할 수 있습니다.

[TCP 타임스탬프](https://tools.ietf.org/html/rfc1323)를 사용하지 않도록 설정합니다.  TCP 타임스탬프를 사용하도록 설정하면 VM의 게스트 OS TCP 스택에 의해 삭제되는 TCP 패킷으로 인해 상태 프로브가 실패하므로, Load Balancer에서 각 엔드포인트를 가동 중단 상태로 표시합니다.  TCP 타임스탬프는 기본적으로 보안이 강화된 VM 이미지에서 주기적으로 사용하도록 설정되며 사용하지 않도록 설정해야 합니다.

## <a name="monitoring"></a>모니터링

공용 및 내부 [표준 Load Balancer](load-balancer-standard-overview.md)는 Azure Monitor를 통해 엔드포인트 및 백 엔드 인스턴스별 상태를 다차원 메트릭으로 공개합니다. 이러한 메트릭은 다른 Azure 서비스 또는 파트너 응용 프로그램에서 사용 될 수 있습니다. 

기본 공용 Load Balancer는 Azure Monitor 로그를 통해 백 엔드 풀에 요약 된 상태 프로브 상태를 노출 합니다.  Azure Monitor 로그는 내부 기본 부하 분산 장치에 사용할 수 없습니다.  [Azure Monitor 로그](load-balancer-monitor-log.md) 를 사용 하 여 공용 부하 분산 장치 프로브 상태 및 프로브 수를 확인할 수 있습니다. Power BI 또는 Azure Operational Insights에서 로깅을 사용하여 부하 분산 장치 상태에 대한 통계를 제공할 수 있습니다.

## <a name="limitations"></a>제한 사항

- HTTPS 프로브는 클라이언트 인증서를 사용한 상호 인증을 지원하지 않습니다.
- TCP 타임스탬프를 사용하도록 설정하면 상태 프로브가 실패합니다.

## <a name="next-steps"></a>다음 단계

- [표준 Load Balancer](load-balancer-standard-overview.md)에 대해 자세히 알아봅니다.
- [PowerShell을 사용하여 Resource Manager에서 공용 부하 분산 장치 만들기 시작](load-balancer-get-started-internet-arm-ps.md)
- [상태 프로브용 REST API](https://docs.microsoft.com/rest/api/load-balancer/loadbalancerprobes/)
- [Load Balancer의 Uservoice](https://aka.ms/lbuservoice)를 사용하여 새 상태 프로브 기능 요청
