---
title: Azure Application Insights의 애플리케이션 맵 | Microsoft Docs
description: 애플리케이션 맵을 사용하여 복잡한 애플리케이션 토폴로지 모니터링
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 3bf37fe9-70d7-4229-98d6-4f624d256c36
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/15/2019
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: 73cf6fd1c20f2e4208d1f7c28a756f28a2fad839
ms.sourcegitcommit: af58483a9c574a10edc546f2737939a93af87b73
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68302566"
---
# <a name="application-map-triage-distributed-applications"></a>애플리케이션 맵: 분산 애플리케이션 심사

애플리케이션 맵을 사용하면 분산된 애플리케이션의 모든 구성 요소에서 성능 병목 상태 또는 실패 핫스폿을 찾는 데 도움이 됩니다. 맵의 각 노드는 애플리케이션 구성 요소 또는 해당 종속성을 나타내며 상태 KPI 및 경고 상태를 나타냅니다. 구성 요소부터 Application Insights 이벤트와 같은 보다 자세한 진단까지 클릭하면서 살펴볼 수 있습니다. 앱에서 Azure 서비스를 사용하는 경우 SQL Database Advisor 권장 사항과 같은 Azure 진단도 살펴볼 수 있습니다.

## <a name="what-is-a-component"></a>구성 요소란?

구성 요소는 독립적으로 배포할 수 있는 분산/마이크로 서비스 애플리케이션의 부분입니다. 개발자 및 운영 팀은 이러한 애플리케이션 구성 요소에서 생성된 원격 분석에 대한 코드 수준의 가시성 또는 액세스 권한을 갖습니다. 

* 구성 요소는 팀/조직에서 액세스할 수 없는 SQL, EventHub 등과 같은 "관찰된" 외부 종속성(코드 또는 원격 분석)과 다릅니다.
* 구성 요소는 서버/역할/컨테이너 인스턴스의 수에 관계없이 실행됩니다.
* 구성 요소는 별도의 Application Insights 계측 키(구독이 다른 경우에도) 또는 단일 Application Insights 계측 키에 보고하는 다른 역할일 수 있습니다. 맵 미리 보기 환경은 구성 요소를 설정 방식에 관계없이 표시합니다.

## <a name="composite-application-map"></a>복합 애플리케이션 맵

여러 수준의 관련된 애플리케이션 구성 요소의 전체 애플리케이션 토폴로지를 볼 수 있습니다. 구성 요소는 다른 Application Insights 리소스이거나 단일 리소스 내의 다른 역할일 수 있습니다. 앱은 Application Insights SDK가 설치된 서버 간에 수행된 HTTP 종속성 호출에 따라 구성 요소를 찾습니다. 

이러한 환경은 구성 요소를 점진적으로 검색으로 시작됩니다. 응용 프로그램 맵을 처음 로드할 때이 구성 요소와 관련 된 구성 요소를 검색 하기 위해 쿼리 집합이 트리거됩니다. 왼쪽 위 구석에 있는 단추는 검색된 애플리케이션의 구성 요소 수로 업데이트됩니다. 

"맵 구성 요소 업데이트"를 클릭하면 맵이 해당 시점까지 검색된 모든 구성 요소로 새로 고쳐집니다. 애플리케이션의 복잡성에 따라, 로드하는 데 다소 시간이 걸릴 수 있습니다.

모든 구성 요소가 단일 Application Insights 리소스 내의 역할인 경우 이 검색 단계가 필요하지 않습니다. 이러한 애플리케이션에 대한 초기 로드 시에는 모든 구성 요소가 포함됩니다.

![애플리케이션 맵 스크린샷](media/app-map/app-map-001.png)

수백 가지 구성 요소가 있는 복잡한 토폴로지를 시각화하는 것이 이 환경의 주요 목표 중 하나입니다.

모든 구성 요소를 클릭하여 관련 정보를 확인하고, 해당 구성 요소에 대한 성능 및 실패 심사 환경으로 이동합니다.

![플라이아웃](media/app-map/application-map-002.png)

### <a name="investigate-failures"></a>오류 조사

**오류 조사**를 선택하여 오류 창을 실행합니다.

![오류 조사 단추 스크린샷](media/app-map/investigate-failures.png)

![오류 환경 스크린샷](media/app-map/failures.png)

### <a name="investigate-performance"></a>성능 조사

성능 문제를 해결하려면 **성능 조사**를 선택합니다.

![성능 조사 단추 스크린샷](media/app-map/investigate-performance.png)

![성능 환경 스크린샷](media/app-map/performance.png)

### <a name="go-to-details"></a>세부 정보로 이동

**세부 정보로 이동** 을 선택 하 여 호출 스택 수준까지 뷰를 제공할 수 있는 종단 간 트랜잭션 환경을 탐색 합니다.

![세부 정보로 이동 단추 스크린샷](media/app-map/go-to-details.png)

![엔드투엔드 트랜잭션 세부 정보 스크린샷](media/app-map/end-to-end-transaction.png)

### <a name="view-in-analytics"></a>Analytics에서 보기

애플리케이션 데이터를 더 쿼리하고 조사하려면 **Analytics에서 보기**를 클릭합니다.

![Analytics에서 보기 단추 스크린샷](media/app-map/view-in-analytics.png)

![Analytics 환경 스크린샷](media/app-map/analytics.png)

### <a name="alerts"></a>,

경고가 트리거될 수 있는 활성 경고 및 기본 규칙을 보려면 **경고**를 선택합니다.

![경고 단추 스크린샷](media/app-map/alerts.png)

![Analytics 환경 스크린샷](media/app-map/alerts-view.png)

## <a name="set-cloud-role-name"></a>클라우드 역할 이름 설정

응용 프로그램 맵은 **클라우드 역할 이름** 속성을 사용 하 여 맵의 구성 요소를 식별 합니다. Application Insights SDK는 구성 요소에서 내보낸 원격 분석에 클라우드 역할 이름 속성을 자동으로 추가 합니다. 예를 들어 SDK는 클라우드 역할 이름 속성에 웹 사이트 이름 또는 서비스 역할 이름을 추가 합니다. 그러나 기본값을 대체할 수 있는 경우가 있습니다. 클라우드 역할 이름을 재정의 하 고 응용 프로그램 맵에 표시 되는 항목을 변경 하려면:

### <a name="netnet-core"></a>.NET/.NET Core

**아래와 같이 사용자 지정 TelemetryInitializer을 작성 합니다.**

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace CustomInitializer.Telemetry
{
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize(ITelemetry telemetry)
        {
            if (string.IsNullOrEmpty(telemetry.Context.Cloud.RoleName))
            {
                //set custom role name here
                telemetry.Context.Cloud.RoleName = "Custom RoleName";
                telemetry.Context.Cloud.RoleInstance = "Custom RoleInstance";
            }
        }
    }
}
```

**ASP.NET apps: 활성 TelemetryConfiguration에 이니셜라이저를 로드 합니다.**

ApplicationInsights에서 다음을 수행 합니다.

```xml
    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="CustomInitializer.Telemetry.MyTelemetryInitializer, CustomInitializer"/>
        ...
      </TelemetryInitializers>
    </ApplicationInsights>
```

웹 앱을 ASP.NET 하는 다른 방법은 코드에서 이니셜라이저를 인스턴스화하는 것입니다 (예: Global.aspx.cs).

```csharp
 using Microsoft.ApplicationInsights.Extensibility;
 using CustomInitializer.Telemetry;

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers.Add(new MyTelemetryInitializer());
    }
```

> [!NOTE]
> 또는를 사용 `ApplicationInsights.config`하여 이니셜라이저를추가하는것은ASP.NETCore응용프로그램에적합하지않습니다.`TelemetryConfiguration.Active` 

**ASP.NET Core 앱: TelemetryConfiguration에 이니셜라이저 로드**

[ASP.NET Core](asp-net-core.md#adding-telemetryinitializers) 응용 프로그램의 경우 아래와 `TelemetryInitializer` 같이 종속성 주입 컨테이너에 새를 추가 하 여 새를 추가 합니다. 클래스의 메서드에서`ConfigureServices` 이 작업을 수행 합니다. `Startup.cs`

```csharp
 using Microsoft.ApplicationInsights.Extensibility;
 using CustomInitializer.Telemetry;
 public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<ITelemetryInitializer, MyTelemetryInitializer>();
}
```

### <a name="nodejs"></a>Node.js

```javascript
var appInsights = require("applicationinsights");
appInsights.setup('INSTRUMENTATION_KEY').start();
appInsights.defaultClient.context.tags["ai.cloud.role"] = "your role name";
appInsights.defaultClient.context.tags["ai.cloud.roleInstance"] = "your role instance";
```

### <a name="alternate-method-for-nodejs"></a>Node.js에 대한 대체 방법

```javascript
var appInsights = require("applicationinsights");
appInsights.setup('INSTRUMENTATION_KEY').start();

appInsights.defaultClient.addTelemetryProcessor(envelope => {
    envelope.tags["ai.cloud.role"] = "your role name";
    envelope.tags["ai.cloud.roleInstance"] = "your role instance"
});
```

### <a name="java"></a>Java

Application Insights Spring Boot 스타터에서 Spring Boot를 사용하는 경우 application.properties 파일에서 애플리케이션에 대한 사용자 지정 이름을 설정하도록 변경하기만 하면 됩니다.

`spring.application.name=<name-of-app>`

스프링 부팅 스타터는 spring.application.name 속성에 대해 사용자가 입력 한 값에 클라우드 역할 이름을 자동으로 할당 합니다.

Java 상관 관계 및 SpringBoot 되지 않는 응용 프로그램에 대 한 클라우드 역할 이름을 구성 하는 방법에 대 한 자세한 내용은 상관 관계에서이 [섹션](https://docs.microsoft.com/azure/application-insights/application-insights-correlation#role-name) 을 체크 아웃 합니다.

### <a name="clientbrowser-side-javascript"></a>클라이언트/브라우저 쪽 JavaScript

```javascript
appInsights.queue.push(() => {
appInsights.addTelemetryInitializer((envelope) => {
  envelope.tags["ai.cloud.role"] = "your role name";
  envelope.tags["ai.cloud.roleInstance"] = "your role instance";
});
});
```

### <a name="understanding-cloud-role-name-within-the-context-of-the-application-map"></a>응용 프로그램 맵의 컨텍스트 내에서 클라우드 역할 이름 이해

**클라우드 역할 이름**에 대해 생각 하는 것과 같이 여러 개의 클라우드 역할 이름이 있는 응용 프로그램 맵을 살펴보면 도움이 될 수 있습니다.

![애플리케이션 맵 스크린샷](media/app-map/cloud-rolename.png)

위의 응용 프로그램 맵에서 녹색 상자에는이 특정 배포 응용 프로그램의 다양 한 측면에 대 한 클라우드 역할 이름 값이 있습니다. 따라서이 앱의 `Authentication`역할은 `Inventory Management`, `acmefrontend`,, a `Payment Processing Worker Role`로 구성 됩니다. 

이 앱의 경우 각 클라우드 역할 이름은 고유한 계측 키가 있는 다른 고유한 Application Insights 리소스도 나타냅니다. 이 응용 프로그램의 소유자는 이러한 4 개의 서로 다른 Application Insights 리소스에 대해 액세스할 수 있으므로 응용 프로그램 맵은 기본 관계의 맵을 함께 연결할 수 있습니다.

[공식 정의](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/39a5ef23d834777eefdd72149de705a016eb06b0/Schema/PublicSchema/ContextTagKeys.bond#L93)의 경우:

```
   [Description("Name of the role the application is a part of. Maps directly to the role name in azure.")]
    [MaxStringLength("256")]
    705: string      CloudRole = "ai.cloud.role";
    
    [Description("Name of the instance where the application is running. Computer name for on-premises, instance name for Azure.")]
    [MaxStringLength("256")]
    715: string      CloudRoleInstance = "ai.cloud.roleInstance";
```

또는 클라우드 역할 **이름이** 문제를 웹 프런트 엔드에 있는 것으로 알려주는 시나리오에서 클라우드 **역할 인스턴스** 를 사용할 수 있지만, 여러 부하 분산 된 서버에서 웹 프런트 엔드를 실행 하 여 다음을 수행할 수 있습니다. Kusto 쿼리를 통해 더 심층적으로 계층을 드릴 다운 하 고 문제가 모든 웹 프런트 엔드 서버/인스턴스에 영향을 주는지 아니면 하나만 매우 중요할 수 있는지 파악 합니다.

앱이 특정 문제를 찾기에 충분 한 정보가 없을 수 있는 컨테이너 화 된 환경에서 앱이 실행 되는 경우 클라우드 역할 인스턴스의 값을 재정의 하는 것이 좋습니다.

원격 분석 이니셜라이저 [를 사용 하 여 클라우드 역할 이름 속성을 재정의 하는 방법에 대 한 자세한 내용은 속성 추가: ITelemetryInitializer 추가](api-filtering-sampling.md#add-properties-itelemetryinitializer)를 참조하세요.

## <a name="troubleshooting"></a>문제 해결

Application Map이 예상대로 작동하지 않는 경우 다음 단계를 수행하세요.

### <a name="general"></a>일반

1. 공식적으로 지원되는 SDK를 사용하고 있는지 확인합니다. 비지원/커뮤니티 SDK는 상관 관계를 지원하지 않을 수 있습니다.

    지원되는 SDK 목록에 대해서는 이 [문서](https://docs.microsoft.com/azure/application-insights/app-insights-platforms)를 참조하세요.

2. 모든 구성 요소를 최신 SDK 버전으로 업그레이드합니다.

3. C#으로 Azure Functions를 사용하는 경우 [Functions V2](https://docs.microsoft.com/azure/azure-functions/functions-versions)로 업그레이드합니다.

4. [클라우드 역할 이름이](#set-cloud-role-name) 올바르게 구성 되었는지 확인 합니다.

5. 종속성이 누락된 경우 [자동 수집 종속성](https://docs.microsoft.com/azure/application-insights/auto-collect-dependencies) 목록에 들어 있는지 확인합니다. 이 목록에 없어도 [종속성 호출 추적](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackdependency)을 사용하여 수동으로 계속 추적할 수 있습니다.

### <a name="too-many-nodes-on-the-map"></a>맵에 노드가 너무 많음

응용 프로그램 맵은 요청 원격 분석에 있는 고유한 각 클라우드 역할 이름에 대 한 응용 프로그램 노드를 생성 하 고 종속성 원격 분석에서 형식, 대상 및 클라우드 역할 이름의 각 고유 조합에 대 한 종속성 노드를 생성 합니다. 원격 분석에 노드가 1만 개 이상 있는 경우 응용 프로그램 맵은 모든 노드와 링크를 인출할 수 없으므로 map이 완전 하지 않습니다. 이 경우 맵을 볼 때 경고 메시지가 표시 됩니다.

또한 응용 프로그램 맵은 한 번에 렌더링 된 분리 되지 않은 개별 노드를 최대 1000 개만 지원 합니다. 응용 프로그램 맵은 동일한 유형 및 호출자를 포함 하는 종속성을 그룹화 하 여 시각적 복잡성을 줄여 주지만 원격 분석에 고유한 클라우드 역할 이름이 너무 많거나 너무 많은 종속성 유형이 있는 경우에는 해당 그룹화에 충분 하지 않으며 지도에서 다음을 수행할 수 없습니다. 못하게.

이 문제를 해결 하려면 클라우드 역할 이름, 종속성 유형 및 종속성 대상 필드를 올바르게 설정 하도록 계측을 변경 해야 합니다.

* 종속성 대상은 종속성의 논리적 이름을 나타내야 합니다. 대부분의 경우 종속성의 서버 또는 리소스 이름과 동일 합니다. 예를 들어 HTTP 종속성의 경우 호스트 이름으로 설정 됩니다. 한 요청에서 다른 요청으로 변경 되는 고유 Id 또는 매개 변수를 포함 해서는 안 됩니다.

* 종속성 유형은 종속성의 논리적 유형을 나타내야 합니다. 예를 들어 HTTP, SQL 또는 Azure Blob은 일반적으로 종속성 유형입니다. 고유 Id를 포함 해서는 안 됩니다.

* 클라우드 역할 이름의 용도는 [위의 섹션](https://docs.microsoft.com/azure/azure-monitor/app/app-map#set-cloud-role-name)에 설명 되어 있습니다.

## <a name="portal-feedback"></a>포털 사용자 의견

사용자 의견을 제공하려면 사용자 의견 옵션을 사용하세요.

![MapLink-1 이미지](./media/app-map/14-updated.png)

## <a name="next-steps"></a>다음 단계

* [상관 관계 이해](https://docs.microsoft.com/azure/application-insights/application-insights-correlation)
