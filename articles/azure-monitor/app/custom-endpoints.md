---
title: Azure Monitor-Azure Application Insights 재정의 기본 SDK 끝점 | Microsoft Docs
description: Azure Government와 같은 지역에 대 한 기본 Azure Application Insights SDK 끝점을 수정 합니다.
services: application-insights
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: mbullwin
ms.openlocfilehash: d086815373b84c0f2a70144a505108875fc04981
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443315"
---
 # <a name="application-insights-overriding-default-endpoints"></a>기본 끝점을 재정의 하는 application Insights

데이터를 보내려면 Application Insights에서 특정 지역에 기본 끝점 주소를 재정의 해야 합니다. 각 SDK에는이 문서에 설명 되어 있는 모든 다른 약간 수정 해야 합니다. 이러한 변경 내용에 대 한 자리 표시자 값을 바꾸고 샘플 코드를 조정 해야 `QuickPulse_Endpoint_Address`, `TelemetryChannel_Endpoint_Address`, 및 `Profile_Query_Endpoint_address` 특정 지역에 대 한 실제 끝점 주소를 사용 합니다. 이 문서의 끝이이 구성이 필요한 경우 지역에 대 한 끝점 주소에 대 한 링크를 포함 합니다.

## <a name="sdk-code-changes"></a>SDK 코드 변경 내용

### <a name="net-with-applicationinsightsconfig"></a>Applicationinsights.config 사용 하 여.NET

> [!NOTE]
> Applicationinsights.config 파일은 SDK에 업그레이드를 언제 든 지에 자동으로 덮어씁니다. SDK 업그레이드를 수행한 후 지역 특정 끝점 값을 다시 입력 해야 합니다.

```xml
<ApplicationInsights>
  ...
  <TelemetryModules>
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse.QuickPulseTelemetryModule, Microsoft.AI.PerfCounterCollector">
      <QuickPulseServiceEndpoint>Custom_QuickPulse_Endpoint_Address</QuickPulseServiceEndpoint>
    </Add>
  </TelemetryModules>
    ...
  <TelemetryChannel>
     <EndpointAddress>TelemetryChannel_Endpoint_Address</EndpointAddress>
  </TelemetryChannel>
  ...
  <ApplicationIdProvider Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.ApplicationInsightsApplicationIdProvider, Microsoft.ApplicationInsights">
    <ProfileQueryEndpoint>Profile_Query_Endpoint_address</ProfileQueryEndpoint>
  </ApplicationIdProvider>
  ...
</ApplicationInsights>
```

### <a name="net-core"></a>.NET Core

다음과 같이 기본 끝점을 조정 하기 위해 프로젝트에서 appsettings.json 파일을 수정 합니다.

```json
"ApplicationInsights": {
    "InstrumentationKey": "instrumentationkey",
    "TelemetryChannel": {
      "EndpointAddress": "TelemetryChannel_Endpoint_Address"
    }
  }
```

라이브 메트릭 및 프로필 쿼리 끝점에 대 한 값은 코드를 통해만 설정할 수 있습니다. 코드를 통해 모든 끝점의 기본 값을 재정의 하려면 다음과 같이 변경에 `ConfigureServices` 메서드는 `Startup.cs` 파일:

```csharp
using Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId;
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse; //place at top of Startup.cs file

   services.ConfigureTelemetryModule<QuickPulseTelemetryModule>((module, o) => module.QuickPulseServiceEndpoint="QuickPulse_Endpoint_Address");

   services.AddSingleton(new ApplicationInsightsApplicationIdProvider() { ProfileQueryEndpoint = "Profile_Query_Endpoint_address" });

   services.AddSingleton<ITelemetryChannel>(new ServerTelemetryChannel() { EndpointAddress = "TelemetryChannel_Endpoint_Address" });

    //place in ConfigureServices method. If present, place this prior to   services.AddApplicationInsightsTelemetry("instrumentation key");
```

### <a name="java"></a>Java

기본 끝점 주소를 변경 하도록 applicationinsights.xml 파일을 수정 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
  <InstrumentationKey>ffffeeee-dddd-cccc-bbbb-aaaa99998888</InstrumentationKey>
  <TelemetryModules>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
  </TelemetryModules>
  <TelemetryInitializers>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>
  </TelemetryInitializers>
  <!--Add the following Channel value to modify the Endpoint address-->
  <Channel type="com.microsoft.applicationinsights.channel.concrete.inprocess.InProcessTelemetryChannel">
  <EndpointAddress>TelemetryChannel_Endpoint_Address</EndpointAddress>
  </Channel>
</ApplicationInsights>
```

### <a name="spring-boot"></a>Spring Boot

수정 된 `application.properties` 파일을 추가 합니다.

```yaml
azure.application-insights.channel.in-process.endpoint-address= TelemetryChannel_Endpoint_Address
```

### <a name="nodejs"></a>Node.js

```javascript
var appInsights = require("applicationinsights");
appInsights.setup('INSTRUMENTATION_KEY');
appInsights.defaultClient.config.endpointUrl = "TelemetryChannel_Endpoint_Address"; // ingestion
appInsights.defaultClient.config.profileQueryEndpoint = "Profile_Query_Endpoint_address"; // appid/profile lookup
appInsights.defaultClient.config.quickPulseHost = "QuickPulse_Endpoint_Address"; //live metrics
appInsights.Configuration.start();
```

환경 변수를 통해 끝점을 구성할 수도 있습니다.

```
Instrumentation Key: "APPINSIGHTS_INSTRUMENTATIONKEY"
Profile Endpoint: "Profile_Query_Endpoint_address"
Live Metrics Endpoint: "QuickPulse_Endpoint_Address"
```

### <a name="javascript"></a>JavaScript

```javascript
<script type="text/javascript">
   var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(e){
      function n(e){t[e]=function(){var n=arguments;t.queue.push(function(){t[e].apply(t,n)})}}var t={config:e};t.initialize=!0;var i=document,a=window;setTimeout(function(){var n=i.createElement("script");n.src=e.url||"https://az416426.vo.msecnd.net/next/ai.2.min.js",i.getElementsByTagName("script")[0].parentNode.appendChild(n)});try{t.cookie=i.cookie}catch(e){}t.queue=[],t.version=2;for(var r=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];r.length;)n("track"+r.pop());n("startTrackPage"),n("stopTrackPage");var s="Track"+r[0];if(n("start"+s),n("stop"+s),n("setAuthenticatedUserContext"),n("clearAuthenticatedUserContext"),n("flush"),!(!0===e.disableExceptionTracking||e.extensionConfig&&e.extensionConfig.ApplicationInsightsAnalytics&&!0===e.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){n("_"+(r="onerror"));var o=a[r];a[r]=function(e,n,i,a,s){var c=o&&o(e,n,i,a,s);return!0!==c&&t["_"+r]({message:e,url:n,lineNumber:i,columnNumber:a,error:s}),c},e.autoExceptionInstrumented=!0}return t
   }({
      instrumentationKey:"INSTRUMENTATION_KEY"
      endpointUrl: "TelemetryChannel_Endpoint_Address"
   });

   window[aiName]=aisdk,aisdk.queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
</script>
```

## <a name="regions-that-require-endpoint-modification"></a>끝점 수정 해야 하는 지역

현재는 끝점 수정 해야 하는 유일한 지역이 [Azure Government](https://docs.microsoft.com/azure/azure-government/documentation-government-services-monitoringandmanagement#application-insights) 하 고 [Azure 중국](https://docs.microsoft.com/azure/china/resources-developer-guide)합니다.

|지역 |  엔드포인트 이름 | 값 |
|-----------------|:------------|:-------------|
| Azure China | 원격 분석 채널 | `https://dc.applicationinsights.azure.cn/v2/track` |
| Azure China | QuickPulse (라이브 메트릭) |`https://quickpulse.applicationinsights.azure.cn/QuickPulseService.svc` |
| Azure China | 프로필 쿼리 |`https://dc.applicationinsights.azure.cn/api/profiles/{0}/appId`  |
| Azure Government | 원격 분석 채널 |`https://dc.applicationinsights.us/v2/track` |
| Azure Government | QuickPulse (라이브 메트릭) |`https://quickpulse.applicationinsights.us/QuickPulseService.svc` |
| Azure Government | 프로필 쿼리 |`https://dc.applicationinsights.us/api/profiles/{0}/appId` |

> [!NOTE]
> 코드 없는 에이전트/확장 기반의 Azure App Services에 대 한 모니터링 **현재 지원 되지 않습니다** 이러한 지역에서. 이 기능은 사용할 수 있게 하는 즉시이 문서에서는 업데이트 됩니다.

## <a name="next-steps"></a>다음 단계

- Azure Government에 대 한 사용자 지정 수정에 대 한 자세한 내용은 참조에 대 한 자세한 지침은 [Azure 모니터링 및 관리](https://docs.microsoft.com/azure/azure-government/documentation-government-services-monitoringandmanagement#application-insights)합니다.
- Azure 중국에 대 한 자세한 내용은 참조는 [Azure 중국 플레이 북](https://docs.microsoft.com/azure/china/)합니다.