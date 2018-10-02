---
title: Microsoft Azure 및 Azure Monitor의 클래식 경고 개요
description: 클래식 경고는 더 이상 사용되지 않습니다. 경고를 통해 Azure 리소스 메트릭, 이벤트 또는 로그를 모니터링하고 사용자가 지정한 조건에 부합하면 알림을 받을 수 있습니다.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: robb
ms.component: alerts
ms.openlocfilehash: 7046a0c6ac84ad5f156098a26dcef2b8accd50af
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987648"
---
# <a name="what-are-classic-alerts-in-microsoft-azure"></a>Microsoft Azure의 클래식 경고란?

> [!NOTE]
> 이 문서에서는 기존 클래식 메트릭 경고를 만드는 방법을 설명합니다. 이제 Azure Monitor는 [최신 근 실시간 메트릭 경고 및 새 경고 환경](monitoring-overview-alerts.md)을 지원합니다. 
>

경고를 통해 데이터에 대한 조건을 구성하고 최신 모니터링 데이터가 조건에 일치하면 알림을 받을 수 있습니다.

## <a name="old-and-new-alerting-capabilities"></a>이전 및 새 경고 기능

이전 Azure Monitor에서 Application Insights, Log Analytics 및 Service Health에는 개별 경고 기능이 있습니다. 시간이 경과함에 따라 Azure가 개선되어 여러 다른 경고 방법과 사용자 인터페이스를 결합했습니다. 통합은 여전히 진행 중입니다. 경고

Azure Portal의 클래식 경고 사용자 화면에서만 클래식 경고를 볼 수 있습니다. 경고 화면의 **클래식 경고 보기** 단추를 누르면 이 화면을 표시할 수 있습니다. 

 ![Azure Portal의 경고 선택 사항](./media/monitoring-overview-alerts-classic/monitor-alert-screen2.png) 

새로운 경고 사용자 환경에서는 클래식 경고 환경에 비해 다음과 같은 이점이 있습니다.
-   **향상된 알림 시스템** - 모든 최신 경고는 여러 경고에서 다시 사용할 수 있는 알림 및 작업으로 명명된 그룹인 작업 그룹을 사용합니다. 클래식 메트릭 경고 및 이전 Log Analytics 경고는 작업 그룹을 사용하지 않습니다.
-   **통합 작성 환경** - Azure Monitor, Log Analytics, Application Insights 전체의 메트릭, 로그 및 활동 로그에 대한 모든 경고가 한곳에서 생성됩니다.
-   **Azure Portal에서 발생한 Log Analytics 경고 보기** - 이제 구독에서 발생한 Log Analytics 경고를 볼 수도 있습니다. 이전에는 별도의 포털에 있었습니다.
-   **발생한 경고 및 경고 규칙의 분리** - 경고 규칙(경고를 트리거하는 조건의 정의)과 발생한 경고(경고 규칙 발생의 인스턴스)가 구별되므로 운영 및 구성 보기가 구분됩니다.
-   **향상된 워크플로** - 새로운 경고 작성 환경은 알림을 받을 항목을 더 간단하게 검색할 수 있는 경고 규칙 구성 프로세스를 사용자에게 안내합니다.
-   **스마트 경고 통합** 및 **경고 상태 설정** -최신 경고에는 사용자 인터페이스의 오버로드를 줄이기 위해 비슷한 경고를 함께 표시하는 자동 그룹화 기능을 포함합니다. 

새로운 메트릭 경고는 클래식 메트릭 경고에 비해 다음과 같은 이점이 있습니다.
-   **개선된 대기 시간**: 최신 메트릭 경고는 1분 간격으로 자주 실행할 수 있습니다. 이전 메트릭 경고는 항상 5분 주기로 실행됩니다. 새로운 경고에서는 문제 발생부터 알림 또는 작업까지의 지연 시간이 줄어들었습니다(3 ~ 5분). 이전 경고는 유형에 따라 5 ~ 15분입니다.  정보를 수집하는 데 걸리는 시간 때문에 로그 경고에서는 10 ~ 15분이 지연되지만, 새로운 처리 방법에서는 이 시간이 줄어듭니다. 
-   **다차원 메트릭에 대한 지원**: 메트릭의 흥미로운 세그먼트를 모니터링할 수 있도록 차원이 포함된 메트릭에서 경고를 수행할 수 있습니다.
-   **메트릭 조건 세부 제어**: 경고 규칙을 다양하게 정의할 수 있습니다. 최신 경고는 메트릭의 최대, 최소, 평균 및 전체 값을 모니터링하도록 지원합니다.
-   **여러 가지 메트릭의 결합 모니터링**: 단일 규칙을 사용하여 다중 메트릭(현재 최대 2개)을 모니터링할 수 있습니다. 두 메트릭이 지정된 기간 동안 해당 임계값을 위반하면 경고가 트리거됩니다.
-   **향상된 알림 시스템**: 모든 최신 경고는 여러 경고에서 다시 사용할 수 있는 알림 및 작업으로 명명된 그룹인 [작업 그룹](../monitoring-and-diagnostics/monitoring-action-groups.md)을 사용합니다.  클래식 메트릭 경고 및 이전 Log Analytics 경고는 작업 그룹을 사용하지 않습니다. 
-   **로그의 메트릭**(공용 미리 보기): 이제 Log Analytics로 이동하는 로그 데이터를 추출하고 Azure Monitor 메트릭으로 변환한 다음, 다른 메트릭과 마찬가지로 경고할 수 있습니다. 클래식 경고의 고유 용어는 [경고(클래식)](monitoring-overview-alerts-classic.md)를 참조하세요. 


## <a name="classic-alerts-on-azure-monitor-data"></a>Azure Monitor 데이터에 대한 클래식 경고
두 가지 종류의 사용 가능한 클래식 경고가 있습니다. 메트릭 경고 및 활동 로그 경고

* **클래식 메트릭 경고** - 이 경고는 특정 메트릭의 값이 사용자가 할당한 임계값을 초과했을 때 트리거됩니다. 경고는 “활성화됨”인 경우(임계값을 초과하고 경고 조건이 충족된 경우) 알림을 생성합니다. “해결”인 경우(임계값을 다시 초과하고 조건이 더 이상 충족되지 않는 경우)에도 다른 알림을 생성합니다.

* **클래식 활동 로그 경고** - 사용자가 할당한 필터 기준에 부합하는 활동 로그 이벤트가 생성되면 트리거되는 스트리밍 로그 알림입니다. 경고 엔진은 단순히 필터 기준을 모든 새 이벤트에 적용하므로 이러한 경고의 상태는 "활성화됨"뿐입니다. 이러한 경고를 통해 새 서비스 상태 사건이 발생하거나 사용자 또는 응용 프로그램이 구독에서 작업을 수행할 때(예: "가상 머신 삭제") 알림을 받을 수 있습니다.

Azure Monitor를 통해 제공되는 진단 로그 데이터의 경우 데이터를 Log Analytics(이전 OMS)로 전달하고 Log Analytics 쿼리 경고를 사용합니다. 이제 Log Analytics는 [새로운 경고 메서드](monitoring-overview-unified-alerts.md)를 사용합니다. 

다음 다이어그램에서는 Azure Monitor의 데이터 원본을 요약하고 그러한 데이터에 대해 경고 받는 방법을 개념적으로 설명약합니다.

![경고를 설명합니다.](./media/monitoring-overview-alerts-classic/Alerts_Overview_Resource_v5.png)

## <a name="taxonomy-of-alerts-classic"></a>경고(클래식) 분류
Azure는 다음 용어를 사용하여 클래식 경고와 그 기능을 설명합니다.
* **경고** - 조건에 부합하면 활성화되는 기준의 정의(하나 이상의 규칙 또는 조건)
* **활성** - 클래식 경고로 정의한 기준에 부합하는 상태
* **해결** - 클래식 경고에서 정의한 기준에 이전에는 부합했고 이제 더 이상은 부합하지 않는 상태
* **알림** - 활성화된 클래식 경고를 기반으로 수행되는 조치
* **작업** - 알림 수신자에게 보내는 특정 호출(예: 주소로 이메일 보내기 또는 웹후크 URL에 게시). 알림은 보통 여러 작업을 트리거할 수 있습니다.

## <a name="how-do-i-receive-a-notification-from-an-azure-monitor-classic-alert"></a>Azure Monitor 클래식 경고에서 알림을 받는 방법
지금까지 다양한 서비스의 Azure 경고는 자체 기본 제공 알림 방법을 사용했습니다.  

Azure Monitor에서 *작업 그룹*이라는 재사용 가능한 알림 그룹화를 만듭니다. 작업 그룹은 알림에 대한 수신자 집합을 지정하며 작업 그룹을 참조하는 경고가 활성화될 때마다 모든 수신자가 해당 알림을 수신하게 됩니다. 작업 그룹을 통해 여러 경고 개체에서 수신자 그룹(예: 엔지니어 전화걸기 목록)을 재사용할 수 있습니다. 작업 그룹은 메일 주소, SMS 번호 및 다양한 다른 작업 외에도 webhook URL 게시를 통한 알림도 지원합니다.  자세한 내용은 [작업 그룹](monitoring-action-groups.md)을 참조하세요. 

이전 클래식 활동 로그 경고는 작업 그룹을 사용합니다.

그러나 이전 메트릭 경고는 작업 그룹을 사용하지 않습니다. 대신 다음 작업을 구성할 수 있습니다. 
- 서비스 관리자, 공동 관리자 또는 사용자가 지정한 추가 이메일 주소로 이메일 알림을 보냅니다.
- 추가 자동화 작업을 시작할 수 있게 webhook를 호출합니다.

Webhook는 다음 예시 항목을 사용하여 자동화 및 조치를 활성화합니다.
    - Azure Automation Runbook
    - Azure Function
    - Azure Logic App
    - 타사 서비스

## <a name="next-steps"></a>다음 단계
다음을 사용하여 경고 규칙에 대한 정보를 확인하고 구성할 수 있습니다.

* [메트릭](monitoring-overview-metrics.md)에 대해 자세히 알아보기
* [Azure Portal을 통해 클래식 메트릭 경고](insights-alerts-portal.md) 구성
* [클래식 메트릭 경고 PowerShell](insights-alerts-powershell.md) 구성
* [클래식 메트릭 경고 CLI(명령줄 인터페이스)](insights-alerts-command-line-interface.md) 구성
* [클래식 메트릭 경고 Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx) 구성
* [활동 로그](monitoring-overview-activity-logs.md)에 대해 자세히 알아보기
* [Azure Portal을 통해 활동 로그 경고](monitoring-activity-log-alerts.md) 구성
* [Resource Manager를 통해 활동 로그 경고](monitoring-create-activity-log-alerts-with-resource-manager-template.md) 구성
* [활동 로그 경고 웹후크 스키마](monitoring-activity-log-alerts-webhook.md) 검토
* [작업 그룹](monitoring-action-groups.md)에 대해 자세히 알아보세요.
* [최신 경고](monitor-alerts-unified-usage.md) 구성