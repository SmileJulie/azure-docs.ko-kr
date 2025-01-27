---
title: Azure Service Bus FAQ | Microsoft Docs
description: Azure Service Bus에 대 한 몇 가지 자주 묻는 질문에 답변 합니다.
services: service-bus-messaging
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.topic: article
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 80809afc9f2a8e8da2f6adecfe916141c4cd3e45
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68278333"
---
# <a name="service-bus-faq"></a>Service Bus FAQ

이 문서는 Microsoft Azure Service Bus에 대한 일부 자주 묻는 질문에 대해 설명합니다. 또한 일반적인 Azure 가격 책정 및 지원 정보는 [Azure 지원 FAQ](https://azure.microsoft.com/support/faq/)에서 확인할 수 있습니다.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="general-questions-about-azure-service-bus"></a>Azure Service Bus에 대한 일반적인 질문
### <a name="what-is-azure-service-bus"></a>Azure Service Bus란?
[Azure Service Bus](service-bus-messaging-overview.md)는 분리된 시스템 간에 데이터를 보낼 수 있도록 하는 비동기 메시지 클라우드 플랫폼입니다. Microsoft에서 이 기능을 서비스로 제공하므로 이를 사용하기 위해 고유한 하드웨어를 호스트할 필요는 없습니다.

### <a name="what-is-a-service-bus-namespace"></a>Service Bus 네임스페이스란?
[네임스페이스](service-bus-create-namespace-portal.md)는 애플리케이션 내에서 Service Bus 리소스의 주소를 지정하기 위한 범위 컨테이너를 제공합니다. 네임스페이스를 만드는 작업은 Service Bus를 사용하는 데 필요하고 Service Bus를 시작하는 첫 번째 단계 중 하나입니다.

### <a name="what-is-an-azure-service-bus-queue"></a>Azure Service Bus 큐란?
[Service Bus 큐](service-bus-queues-topics-subscriptions.md)는 메시지가 저장되는 엔터티입니다. 큐는 서로 통신해야 하는 여러 애플리케이션 또는 분산된 애플리케이션의 여러 부분이 있는 경우에 유용합니다. 큐는 여러 제품(메시지)를 해당 위치에서 받고 보내는 배포 센터와 비슷합니다.

### <a name="what-are-azure-service-bus-topics-and-subscriptions"></a>Azure Service Bus 토픽 및 구독이란?
토픽은 큐로 시각화할 수 있고 여러 구독을 사용하는 경우 더 풍부한 메시징 모델이 되며 기본적으로는 일대다 통신 도구입니다. 이 게시/구독 모델(또는 *pub/sub*)을 사용하면 여러 애플리케이션에서 받은 메시지를 여러 구독을 사용하여 토픽에 메시지를 전송하는 애플리케이션을 활성화합니다.

### <a name="what-is-a-partitioned-entity"></a>분할된 엔터티란?
일반적인 큐 또는 항목은 단일 메시지 broker에서 처리되며 하나의 메시징 저장소에 저장됩니다. 기본 및 표준 메시징 계층에서만 지원되는 [분할된 큐 또는 토픽](service-bus-partitioning.md)은 여러 메시지 broker에 의해 처리되고 여러 메시징 저장소에 저장됩니다. 즉, 분할된 큐 또는 항목의 전체 처리량은 단일 메시지 broker 또는 메시징 저장소의 성능으로 제한되지 않습니다. 또한 메시징 스토어가 일시적으로 중단된 경우에도 분할된 큐 또는 항목을 계속 렌더링할 수 없습니다.

분할된 엔터티를 사용하는 경우 순서는 보장할 수 없습니다. 파티션을 사용할 수 없는 이벤트에서도 여전히 메세지를 보내고 다른 파티션에서 메시지를 수신할 수 있습니다.

 분할된 엔터티는 [프리미엄 SKU](service-bus-premium-messaging.md)에서 지원되지 않습니다. 

### <a name="what-ports-do-i-need-to-open-on-the-firewall"></a>방화벽에서 열어야 하는 포트는 무엇 인가요? 
Azure Service Bus에서 다음 프로토콜을 사용 하 여 메시지를 보내고 받을 수 있습니다.

- AMQP(고급 메시지 큐 프로토콜)
- SBMP(Service Bus 메시징 프로토콜)
- HTTP

이러한 프로토콜을 사용 하 여 Azure Event Hubs와 통신 하기 위해 열어야 하는 아웃 바운드 포트는 다음 표를 참조 하세요. 

| 프로토콜 | 포트 | 세부 정보 | 
| -------- | ----- | ------- | 
| AMQP | 5671 및 5672 | [Amqp 프로토콜 가이드](service-bus-amqp-protocol-guide.md) 를 참조 하세요. | 
| SBMP | 9350 ~ 9354 | [연결 모드](/dotnet/api/microsoft.servicebus.connectivitymode?view=azure-dotnet) 를 참조 하세요. |
| HTTP, HTTPS | 80, 443 | 

### <a name="what-ip-addresses-do-i-need-to-whitelist"></a>허용 목록 하는 데 필요한 IP 주소는 무엇 인가요?
연결에 대 한 적절 한 IP 주소 목록을 찾으려면 다음 단계를 수행 합니다.

1. 명령 프롬프트에서 다음 명령을 실행 합니다. 

    ```
    nslookup <YourNamespaceName>.servicebus.windows.net
    ```
2. 에서 `Non-authoritative answer`반환 된 IP 주소를 적어둡니다. 이 IP 주소는 정적입니다. 다른 클러스터에 네임 스페이스를 복원 하는 경우에만 변경 되는 유일한 시점입니다.

네임 스페이스에 영역 중복성을 사용 하는 경우 몇 가지 추가 단계를 수행 해야 합니다. 

1. 먼저 네임 스페이스에서 nslookup을 실행 합니다.

    ```
    nslookup <yournamespace>.servicebus.windows.net
    ```
2. 다음 형식 중 하나에 해당 하는 **신뢰할 수 없는 응답** 섹션의 이름을 적어둡니다. 

    ```
    <name>-s1.servicebus.windows.net
    <name>-s2.servicebus.windows.net
    <name>-s3.servicebus.windows.net
    ```
3. 접미사 s1, s2 및 s 3이 포함 된 각 항목에 대해 nslookup을 실행 하 여 세 개의 가용성 영역에서 실행 되는 세 인스턴스의 IP 주소를 가져옵니다. 


## <a name="best-practices"></a>모범 사례
### <a name="what-are-some-azure-service-bus-best-practices"></a>일부 Azure Service Bus 모범 사례는 무엇인가요?
[Service Bus를 사용한 성능 향상에 대 한 모범 사례][Best practices for performance improvements using Service Bus] -이 문서에서는 메시지를 교환할 때 성능을 최적화 하는 방법을 설명 합니다.

### <a name="what-should-i-know-before-creating-entities"></a>엔터티를 만들기 전에 무엇을 알아야 하나요?
큐 및 토픽에서 다음 속성을 변경할 수 없습니다. 엔터티를 프로비전할 때 이러한 제한 사항을 고려하세요. 새 대체 엔터티를 만들지 않은 경우 이러한 속성을 수정할 수 없기 때문입니다.

* 분할
* 세션
* 중복 검색
* Express 엔터티

## <a name="pricing"></a>가격
이 섹션은 Service Bus 가격 책정 구조에 대한 일부 자주 묻는 질문을 답변합니다.

[Service Bus 가격 및 대금 청구](https://azure.microsoft.com/pricing/details/service-bus/) 문서에서는 Service Bus의 요금 청구 기준에 대해 설명합니다. Service Bus 가격 책정 옵션에 대한 특정 정보는 [Service Bus 가격 책정 세부 정보](https://azure.microsoft.com/pricing/details/service-bus/)를 참조하세요.

또한 일반적인 Azure 가격 책정 정보는 [Azure 지원 FAQ](https://azure.microsoft.com/support/faq/)에서 확인할 수 있습니다. 

### <a name="how-do-you-charge-for-service-bus"></a>Service Bus 요금을 어떻게 청구하나요?
Service Bus 가격 책정에 대한 전체 내용은 [Service Bus 가격 책정 세부 정보][Pricing overview]를 참조하세요. 언급된 가격 외에도 애플리케이션이 프로비전되는 데이터 센터의 외부에서 송신을 위해 연결된 데이터 전송에 요금이 부과됩니다.

### <a name="what-usage-of-service-bus-is-subject-to-data-transfer-what-is-not"></a>Service Bus를 어떻게 사용하면 데이터가 전송의 대상이 되나요? 어떤 경우에 대상이 아닌가요?
지정된 Azure 지역 내에서 데이터 전송은 비용뿐만 아니라 인바운드 데이터 전송 없이 제공됩니다. 지역 외부의 데이터 전송은 [여기](https://azure.microsoft.com/pricing/details/bandwidth/)에서 찾을 수 있는 송신 요금이 청구됩니다.

### <a name="does-service-bus-charge-for-storage"></a>Service Bus는 저장소에 대한 요금을 청구하나요?
아니요, Service Bus는 저장소에 대한 요금을 청구하지 않습니다. 하지만 큐/항목 당 지속될 수 있는 데이터의 최대 크기를 제한하는 할당량이 있습니다. 다음 FAQ를 참조하세요.

## <a name="quotas"></a>할당량

Service Bus 제한 및 할당량 목록은 [Service Bus 할당량 개요][Quotas overview]를 참조 하세요.

### <a name="does-service-bus-have-any-usage-quotas"></a>Service Bus는 사용 할당량이 있나요?
기본적으로 모든 클라우드 서비스의 경우 Microsoft는 모든 고객의 구독에 대해 계산되는 월별 사용 할당량을 집계합니다. 이러한 한도보다 많이 필요한 경우 이러한 한도를 적절하게 조정하기 위해 고객 서비스에 언제든지 문의할 수 있습니다. Service Bus의 경우 집계 사용 할당량은 매월 5십억 개의 메시지입니다.

Microsoft는 지정된 달에 사용 할당량을 초과하는 고객의 계정을 사용하지 않도록 설정하는 권한을 보유하지만 이메일 알림이 전송되고 조치를 취하기 전에 고객에게 연락을 여러 번 시도합니다. 이러한 할당량을 초과하는 고객은 할당량을 초과하는 요금을 지불해야 합니다.

Azure에서 다른 서비스와 마찬가지로 Service Bus는 리소스의 공정한 사용을 보장하기 위해 특정한 할당량 집합을 적용합니다. 이러한 할당량에 대 한 자세한 내용은 [Service Bus 할당량 개요][Quotas overview]에서 찾을 수 있습니다.

### <a name="how-to-handle-messages-of-size--1-mb"></a>1MB를 초과하는 메시지 크기를 처리하는 방법
Service Bus 메시징 서비스(큐 및 토픽/구독)를 사용하면 애플리케이션이 최대 256KB(표준 계층) 또는 1MB(프리미엄 계층) 크기의 메시지를 보낼 수 있습니다. 1MB보다 큰 크기의 메시지를 처리하는 경우 [이 블로그 게시물](https://www.serverless360.com/blog/deal-with-large-service-bus-messages-using-claim-check-pattern)에 설명된 클레임 검사 패턴을 사용합니다.

## <a name="troubleshooting"></a>문제 해결
### <a name="why-am-i-not-able-to-create-a-namespace-after-deleting-it-from-another-subscription"></a>네임스페이스를 다른 구독에서 삭제한 후 만들 수 없는 이유는 무엇인가요? 
구독에서 네임스페이스를 삭제한 후에 다른 구독에서 동일한 이름으로 다시 만들려면 4시간 정도 기다려야 합니다. 그렇지 않으면 다음 오류 메시지가 표시될 수 있습니다. `Namespace already exists` 

### <a name="what-are-some-of-the-exceptions-generated-by-azure-service-bus-apis-and-their-suggested-actions"></a>Azure Service Bus API 및 해당 제안된 작업에 의해 생성된 일부 예외는 무엇인가요?
가능한 Service Bus 예외의 목록은 [예외 개요][Exceptions overview]를 참조 하세요.

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>공유 액세스 서명이란 무엇이고 어떤 언어가 서명 생성을 지원하나요?
공유 액세스 서명은 SHA-256 보안 해시 또는 URI에 따른 인증 메커니즘입니다. Node.js, PHP, Java, Python 및 C#에서 사용자 고유의 서명을 생성 하는 방법에 대 한 자세한 내용은 [공유 액세스 서명][Shared Access Signatures] 문서를 참조 하세요.

## <a name="subscription-and-namespace-management"></a>구독 및 네임스페이스 관리
### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>다른 Azure 구독으로 네임스페이스를 마이그레이션하려면 어떻게 해야 하나요?

[Azure Portal](https://portal.azure.com) 또는 PowerShell 명령을 사용하여 Azure 구독 간에 네임스페이스를 이동할 수 있습니다. 작업을 실행하기 위해 네임스페이스가 활성화되어야 합니다. 명령을 실행하는 사용자는 원본 및 대상 구독에 대한 관리자여야 합니다.

#### <a name="portal"></a>포털

Azure Portal을 사용하여 Service Bus 네임스페이스를 다른 구독으로 마이그레이션하려면 [여기](../azure-resource-manager/resource-group-move-resources.md#use-the-portal)에 있는 지침을 따르세요. 

#### <a name="powershell"></a>PowerShell

다음 PowerShell 명령 시퀀스는 네임스페이스를 Azure 구독 간에 이동합니다. 이 작업을 실행하려면 네임스페이스 이미 활성 상태여야 하며 PowerShell 명령을 실행하는 사용자는 원본 및 대상 구독의 관리자여야 합니다.

```powershell
# Create a new resource group in target subscription
Select-AzSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>다음 단계
Service Bus에 대한 자세한 내용은 다음 문서를 참조하세요.

* [Azure Service Bus 프리미엄 소개(블로그 게시물)](https://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Azure Service Bus 프리미엄 소개(Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [Service Bus 개요](service-bus-messaging-overview.md)
* [Service Bus 큐 시작](service-bus-dotnet-get-started-with-queues.md)

[Best practices for performance improvements using Service Bus]: service-bus-performance-improvements.md
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Quotas overview]: service-bus-quotas.md
[Exceptions overview]: service-bus-messaging-exceptions.md
[Shared Access Signatures]: service-bus-sas.md
