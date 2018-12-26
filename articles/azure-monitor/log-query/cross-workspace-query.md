---
title: Azure Log Analytics로 리소스 검색 | Microsoft Docs
description: 이 문서에서는 구독의 여러 작업 영역과 특정 App Insights 앱에서 리소스를 쿼리하는 방법을 설명합니다.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: magoedte
ms.openlocfilehash: 1140e9d0a9df717b8e249d8dad895baec955b754
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53192977"
---
# <a name="perform-cross-resource-log-searches-in-log-analytics"></a>Log Analytics에서 리소스 간 로그 검색 수행  

이전에는 Azure Log Analytics로 현재 작업 영역 내의 데이터만 분석할 수 있었기 때문에 구독에 정의된 여러 작업 영역을 쿼리할 수 없었습니다.  또한 Application Insights에서 직접 또는 Visual Studio에서, Application Insights를 사용하여 웹 기반 애플리케이션에서 수집된 원격 분석 항목만 검색할 수 있습니다.  이로 인해 운영 및 애플리케이션 데이터를 고유하게 분석하는 데 어려움이 나타납니다.   

이제 여러 Log Analytics 작업 영역뿐만 아니라 동일한 리소스 그룹, 다른 리소스 그룹 또는 다른 구독의 특정 Application Insights 앱의 데이터도 쿼리가 가능합니다. 이를 통해 시스템 차원의 데이터 보기가 가능합니다.  [로그 분석](portals.md#log-analytics-page)에서 이러한 유형의 쿼리만 수행할 수 있습니다. 단일 쿼리에 포함할 수 있는 리소스 (Log Analytics 작업 영역 및 Application Insights 앱)의 수는 100개로 제한됩니다. 

## <a name="querying-across-log-analytics-workspaces-and-from-application-insights"></a>Log Analytics 작업 영역 전체 및 Application Insights 쿼리
쿼리에 다른 작업 영역을 참조하려면 [*workspace*](https://docs.microsoft.com/azure/log-analytics/query-language/workspace-expression) 식별자를 사용하고 Application Insights의 앱의 경우 [*app*](https://docs.microsoft.com/azure/log-analytics/query-language/app-expression) 식별자를 사용합니다.  

### <a name="identifying-workspace-resources"></a>작업 영역 리소스 식별
다음 예제에서는 현재 작업 영역과 *contosoretail-it*이라는 작업 영역의 업데이트 테이블에서 요약된 로그 수를 반환하기 위한 Log Analytics 작업 영역의 쿼리를 보여줍니다. 

작업 영역 식별은 여러 가지 방법으로 수행될 수 있습니다.

* 리소스 이름 - 사람이 읽을 수 있는 작업 영역 이름이며 *구성 요소 이름*이라고도 합니다. 

    `workspace("contosoretail-it").Update | count`
 
    >[!NOTE]
    >작업 영역을 이름으로 식별하면 액세스 가능한 모든 구독에서 고유한 것으로 가정합니다. 지정된 이름의 응용 프로그램이 여러 개 있으면 모호성으로 인해 쿼리가 실패합니다. 이런 경우 다른 식별자 중 하나를 사용해야 합니다.

* 정규화된 이름 - 구독 이름, 리소스 그룹 및 구성 요소 이름이 *subscriptionName/resourceGroup/componentName* 형식으로 구성된 작업 영역의 "전체 이름"입니다. 

    `workspace('contoso/contosoretail/contosoretail-it').Update | count `

    >[!NOTE]
    >Azure 구독 이름은 고유하지 않기 때문에 이 식별자는 모호할 수 있습니다. 
    >

* 작업 영역 ID - 작업 영역 ID는 GUID(Globally Unique Identifier)로 나타낸 각 작업 영역에 할당된 변경되지 않는 고유한 식별자입니다.

    `workspace("b459b4u5-912x-46d5-9cb1-p43069212nb4").Update | count`

* Azure Resource ID – Azure에서 정의한 작업 영역의 고유 ID입니다. 리소스 이름이 모호한 경우 리소스 ID를 사용합니다.  작업 영역의 형: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft.OperationalInsights/workspaces/componentName*.  

    예: 
    ``` 
    workspace("/subscriptions/e427519-5645-8x4e-1v67-3b84b59a1985/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail-it").Update | count
    ```

### <a name="identifying-an-application"></a>응용 프로그램 식별
다음 예제에서는 Application Insights에서 *fabrikamapp*이라는 앱에 대해 요청된 요청의 요약된 수를 반환합니다. 

Application Insights에서 애플리케이션 식별은 *app(Identifier)* 식으로 수행될 수 있습니다.  *Identifier* 인수는 다음을 사용하여 앱을 지정합니다.

* 리소스 이름 - 사람이 읽을 수 있는 앱의 이름이며 *구성 요소 이름*이라고도 합니다.  

    `app("fabrikamapp")`

* 정규화된 이름 - 구독 이름, 리소스 그룹 및 구성 요소 이름이 *subscriptionName/resourceGroup/componentName* 형식으로 구성된 앱의 "전체 이름"입니다. 

    `app("AI-Prototype/Fabrikam/fabrikamapp").requests | count`

     >[!NOTE]
    >Azure 구독 이름은 고유하지 않기 때문에 이 식별자는 모호할 수 있습니다. 
    >

* ID - 응용프로그램의 앱 GUID입니다.

    `app("b459b4f6-912x-46d5-9cb1-b43069212ab4").requests | count`

* Azure Resource ID – Azure에서 정의한 앱의 고유 ID입니다. 리소스 이름이 모호한 경우 리소스 ID를 사용합니다. 형식은 */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft.OperationalInsights/components/componentName*입니다.  

    예: 
    ```
    app("/subscriptions/b459b4f6-912x-46d5-9cb1-b43069212ab4/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp").requests | count
    ```

### <a name="performing-a-query-across-multiple-resources"></a>여러 리소스에서 쿼리 수행
리소스 인스턴스 중 하나에서 여러 리소스를 쿼리할 수 있습니다. 이러한 항목은 작업 영역 및 앱이 결합된 것일 수 있습니다.
    
두 개의 작업 영역에서 쿼리에 대한 예제:    

```
union Update, workspace("contosoretail-it").Update, workspace("b459b4u5-912x-46d5-9cb1-p43069212nb4").Update
| where TimeGenerated >= ago(1h)
| where UpdateState == "Needed"
| summarize dcount(Computer) by Classification
```

## <a name="using-cross-resource-query-for-multiple-resources"></a>여러 리소스에 리소스 간 쿼리 사용
리소스 간 쿼리를 사용하여 여러 Log Analytics 및 Application Insights 리소스에서 데이터의 상관 관계를 지정하는 경우 쿼리는 복잡하고 유지 관리하기 어려워질 수 있습니다. [Log Analytics의 함수](../../azure-monitor/log-query/functions.md)를 활용하여 쿼리 리소스 범위에서 쿼리 논리를 분리합니다. 이는 쿼리 구조를 간소화합니다. 다음 예제에서는 여러 Application Insights 리소스를 모니터링하고 애플리케이션 이름으로 실패한 요청의 수를 시각화하는 방법을 보여줍니다. 

Application Insights 리소스의 범위를 참조하는 다음과 같은 쿼리를 만듭니다. `withsource= SourceApp` 명령은 로그를 전송한 애플리케이션 이름을 지정하는 열을 추가합니다. 별칭 _applicationsScoping_을 사용하여 [함수로 쿼리를 저장합니다](../../azure-monitor/log-query/functions.md#create-a-function).

```Kusto
// crossResource function that scopes my Application Insights resources
union withsource= SourceApp
app('Contoso-app1').requests, 
app('Contoso-app2').requests,
app('Contoso-app3').requests,
app('Contoso-app4').requests,
app('Contoso-app5').requests
```



이제 다음과 같은 리소스 간 쿼리에서 [이 함수를 사용](../../azure-monitor/log-query/functions.md#use-a-function)할 수 있습니다. 함수 별칭 _applicationsScoping_은 정의된 모든 애플리케이션에서 요청 테이블의 통합을 반환합니다. 그런 다음, 쿼리는 실패한 요청에 대해 필터링하고 애플리케이션별로 추세를 시각화합니다. _구문 분석_ 연산자는 이 예제에서 선택 사항입니다. _SourceApp_ 속성에서 애플리케이션 이름을 추출합니다.

```Kusto
applicationsScoping 
| where timestamp > ago(12h)
| where success == 'False'
| parse SourceApp with * '(' applicationName ')' * 
| summarize count() by applicationName, bin(timestamp, 1h) 
| sort by count_ desc 
| render timechart
```
![Timechart](media/cross-workspace-query/chart.png)

## <a name="next-steps"></a>다음 단계

Log Analytics에 사용할 수 있는 모든 쿼리 구문 옵션을 보려면 [Log Analytics 로그 검색 참조](https://docs.microsoft.com/azure/log-analytics/query-language/kusto)를 검토하세요.    