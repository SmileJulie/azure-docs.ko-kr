---
title: Application Insights, C#
titleSuffix: Azure Cognitive Services
description: 이 자습서에서는 Application Insights 원격 분석 데이터 스토리지에 봇 및 Language Understanding 정보가 추가됩니다.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 06/16/2019
ms.author: diberry
ms.openlocfilehash: 720352403fd5f5937669f9838f3974cb0d3f8797
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657795"
---
# <a name="add-luis-results-to-application-insights-from-a-bot-in-c"></a>C#의 봇에서 Application Insights에 LUIS 결과 추가

이 자습서에서는 [Application Insights](https://azure.microsoft.com/services/application-insights/) 원격 분석 데이터 스토리지에 봇 및 Language Understanding 정보가 추가됩니다. 해당 데이터가 있으면 Kusto 언어 또는 Power BI로 데이터를 쿼리하여 발화의 의도 및 엔터티를 실시간으로 분석, 집계 및 보고할 수 있습니다. 이 분석을 통해 LUIS 앱의 의도와 엔터티를 추가하거나 편집해야 할지 결정할 수 있습니다.

이 자습서에서는 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * Application Insights에서 봇 및 Language Understanding 데이터 캡처
> * Language Understanding 데이터에 대한 Application Insights 쿼리

## <a name="prerequisites"></a>필수 조건

* Application Insights를 사용하여 만든 Azure Bot Service 봇
* 이전 봇 **[자습서](luis-csharp-tutorial-bf-v4.md)** 에서 다운로드한 봇 코드 
* [봇 에뮬레이터](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio Code](https://code.visualstudio.com/Download)

이 자습서의 모든 코드는 [Azure 샘플 Language Understanding GitHub 리포지토리](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/v4/luis-csharp-bot-johnsmith-src-telemetry)에서 사용할 수 있습니다. 

## <a name="add-application-insights-to-web-app-bot-project"></a>웹앱 봇 프로젝트에 Application Insights 추가

현재 이 웹앱 봇에 사용되는 Application Insights 서비스는 봇에 대한 일반 상태 원격 분석 데이터를 수집합니다. LUIS 정보는 수집하지 않습니다. 

LUIS 정보를 캡처하려면 웹앱 봇에 **[Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/)** NuGet 패키지가 설치되고 구성되어야 합니다.  

1. Visual Studio에서 솔루션에 종속성을 추가합니다. **솔루션 탐색기**에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭하고 **NuGet 패키지 관리...** 를 선택합니다. NuGet 패키지 관리자는 설치된 패키지 목록을 표시합니다. 
1. **찾아보기**를 선택한 다음 **Microsoft.ApplicationInsights**를 검색합니다.
1. 패키지를 설치합니다. 

## <a name="capture-and-send-luis-query-results-to-application-insights"></a>LUIS 쿼리 결과 캡처 및 Application Insights에 보내기

1. `LuisHelper.cs` 파일을 열고 콘텐츠를 다음 코드로 바꿉니다. **LogToApplicationInsights** 메서드는 봇 및 LUIS 데이터를 캡처하여 Application Insights에 `LUIS`라는 Trace 이벤트로 보냅니다.

    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    using System;
    using System.Linq;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Bot.Builder;
    using Microsoft.Bot.Builder.AI.Luis;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Logging;
    using Microsoft.ApplicationInsights;
    using System.Collections.Generic;
    
    namespace Microsoft.BotBuilderSamples
    {
        public static class LuisHelper
        {
            public static async Task<BookingDetails> ExecuteLuisQuery(IConfiguration configuration, ILogger logger, ITurnContext turnContext, CancellationToken cancellationToken)
            {
                var bookingDetails = new BookingDetails();
    
                try
                {
                    // Create the LUIS settings from configuration.
                    var luisApplication = new LuisApplication(
                        configuration["LuisAppId"],
                        configuration["LuisAPIKey"],
                        "https://" + configuration["LuisAPIHostName"]
                    );
    
                    var recognizer = new LuisRecognizer(luisApplication);
    
                    // The actual call to LUIS
                    var recognizerResult = await recognizer.RecognizeAsync(turnContext, cancellationToken);
    
                    LuisHelper.LogToApplicationInsights(configuration, turnContext, recognizerResult);
    
                    var (intent, score) = recognizerResult.GetTopScoringIntent();
                    if (intent == "Book_flight")
                    {
                        // We need to get the result from the LUIS JSON which at every level returns an array.
                        bookingDetails.Destination = recognizerResult.Entities["To"]?.FirstOrDefault()?["Airport"]?.FirstOrDefault()?.FirstOrDefault()?.ToString();
                        bookingDetails.Origin = recognizerResult.Entities["From"]?.FirstOrDefault()?["Airport"]?.FirstOrDefault()?.FirstOrDefault()?.ToString();
    
                        // This value will be a TIMEX. And we are only interested in a Date so grab the first result and drop the Time part.
                        // TIMEX is a format that represents DateTime expressions that include some ambiguity. e.g. missing a Year.
                        bookingDetails.TravelDate = recognizerResult.Entities["datetime"]?.FirstOrDefault()?["timex"]?.FirstOrDefault()?.ToString().Split('T')[0];
                    }
                }
                catch (Exception e)
                {
                    logger.LogWarning($"LUIS Exception: {e.Message} Check your LUIS configuration.");
                }
    
                return bookingDetails;
            }
            public static void LogToApplicationInsights(IConfiguration configuration, ITurnContext turnContext, RecognizerResult result)
            {
                // Create Application Insights object
                TelemetryClient telemetry = new TelemetryClient();
    
                // Set Application Insights Instrumentation Key from App Settings
                telemetry.InstrumentationKey = configuration["BotDevAppInsightsKey"];
    
                // Collect information to send to Application Insights
                Dictionary<string, string> logProperties = new Dictionary<string, string>();
    
                logProperties.Add("BotConversation", turnContext.Activity.Conversation.Name);
                logProperties.Add("Bot_userId", turnContext.Activity.Conversation.Id);
    
                logProperties.Add("LUIS_query", result.Text);
                logProperties.Add("LUIS_topScoringIntent_Name", result.GetTopScoringIntent().intent);
                logProperties.Add("LUIS_topScoringIntentScore", result.GetTopScoringIntent().score.ToString());
    
    
                // Add entities to collected information
                int i = 1;
                if (result.Entities.Count > 0)
                {
                    foreach (var item in result.Entities)
                    {
                        logProperties.Add("LUIS_entities_" + i++ + "_" + item.Key, item.Value.ToString());
                    }
                }
    
                // Send to Application Insights
                telemetry.TrackTrace("LUIS", ApplicationInsights.DataContracts.SeverityLevel.Information, logProperties);
    
            }
        }
    }
    ```

## <a name="add-application-insights-instrumentation-key"></a>Application Insights 계측 키 추가 

Application Insights에 데이터를 추가하려면 계측 키가 필요합니다.

1. 브라우저에서 [Azure Portal](https://portal.azure.com)로 이동하여 봇의 **Application Insights** 리소스를 찾습니다. 대부분의 이름이 봇의 이름이고 이름 끝에 `luis-csharp-bot-johnsmithxqowom` 같은 임의의 문자가 있습니다. 
1. Application Insights 리소스의 **개요** 페이지에서 **계측 키**를 복사합니다.
1. Visual Studio에서 봇 프로젝트의 루트에 있는 **appsettings.json** 파일을 엽니다. 이 파일은 모든 환경 변수를 포함합니다.
1. 계측 키 값을 가진 새 변수 `BotDevAppInsightsKey`를 추가합니다. 값은 따옴표로 묶어야 합니다. 

## <a name="build-and-start-the-bot"></a>봇 빌드 및 시작

1. Visual Studio에서 봇을 빌드하고 실행합니다. 
1. 봇 에뮬레이터를 시작하고 봇을 엽니다. 이 [단계](luis-csharp-tutorial-bf-v4.md#use-the-bot-emulator-to-test-the-bot)는 이전 자습서에 나와 있습니다.

1. 봇에 질문을 합니다. 이 [단계](luis-csharp-tutorial-bf-v4.md#ask-bot-a-question-for-the-book-flight-intent)는 이전 자습서에 나와 있습니다.

## <a name="view-luis-entries-in-application-insights"></a>Application Insights에서 LUIS 항목 보기

Application Insights를 열어 LUIS 항목을 확인합니다. 데이터가 Application Insights에 나타나는 데 몇 분 정도 걸릴 수 있습니다.

1. [Azure Portal](https://portal.azure.com)에서 봇의 Application Insights 리소스를 엽니다. 
1. 리소스가 열리면 **검색**을 선택하고 이벤트 유형이 **Trace**인 최근 **30분** 동안의 모든 데이터를 검색합니다. 이름이 **LUIS**인 추적을 선택합니다. 
1. 봇 및 LUIS 정보는 **사용자 지정 속성**에서 사용할 수 있습니다. 

    ![Application Insights에 저장된 LUIS 사용자 지정 속성을 검토합니다.](./media/luis-tutorial-appinsights/application-insights-luis-trace-custom-properties-csharp.png)

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Application Insights에서 의도, 점수 및 발화 쿼리
Application Insights를 사용하면 [Kusto](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview#what-language-do-log-queries-use) 언어로 데이터를 쿼리하고 [Power BI](https://powerbi.microsoft.com)로 데이터를 내보낼 수 있습니다. 

1. **Log(Analytics)** 를 선택합니다. 새 창이 열리며 맨 위에는 쿼리 창이 있고 그 아래에는 데이터 테이블 창이 있습니다. 이전에 데이터베이스를 사용한 경우, 이 배열이 친숙합니다. 쿼리는 이전에 필터링된 데이터를 나타냅니다. **CustomDimensions** 열에 봇 및 LUIS 정보가 있습니다.
1. 상위 의도, 점수 및 발화를 끌어오려면 쿼리 창의 마지막 줄(`|top...`줄) 바로 위에 다음을 추가합니다.

    ```kusto
    | extend topIntent = tostring(customDimensions.LUIS_topScoringIntent_Name)
    | extend score = todouble(customDimensions.LUIS_topScoringIntentScore)
    | extend utterance = tostring(customDimensions.LUIS_query)
    ```

1. 쿼리를 실행합니다. topIntent, score 및 utterance의 새 열을 사용할 수 있습니다. topIntent 열을 선택하여 정렬합니다.

[Kusto 쿼리 언어](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries) 또는 [Power BI로 데이터 내보내기](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi)에 대해 자세히 알아보세요. 


## <a name="learn-more-about-bot-framework"></a>Bot Framework에 대해 자세히 알아보기

[Bot Framework](https://dev.botframework.com/)에 대해 자세히 알아봅니다.

## <a name="next-steps"></a>다음 단계

Application Insights 데이터에 추가할 수 있는 기타 정보에는 앱 ID, 버전 ID, 마지막 모델 변경 날짜, 마지막 학습 날짜, 마지막 게시 날짜가 포함됩니다. 이러한 값은 엔드포인트 URL(앱 ID 및 버전 ID) 또는 Authoring API 호출에서 검색한 다음, 웹앱 봇 설정에서 설정하고 끌어올 수 있습니다.  

둘 이상의 LUIS 앱에 대해 동일한 엔드포인트 구독을 사용하는 경우, 구독 ID 및 공유 키임을 나타내는 속성도 포함해야 합니다.

> [!div class="nextstepaction"]
> [예제 발언에 대해 자세히 알아보기](luis-how-to-add-example-utterances.md)
