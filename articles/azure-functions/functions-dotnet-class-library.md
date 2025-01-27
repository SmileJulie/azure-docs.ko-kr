---
title: Azure Functions C# 개발자 참조
description: C#을 사용하여 Azure Functions를 개발하는 방법을 알아봅니다.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure 함수, 함수, 이벤트 처리, webhook, 동적 계산, 서버리스 아키텍처
ms.service: azure-functions
ms.devlang: dotnet
ms.topic: reference
ms.date: 09/12/2018
ms.author: glenga
ms.openlocfilehash: 388b389cca7c3e820ea3ccfd37a2a93ccd476b31
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68254647"
---
# <a name="azure-functions-c-developer-reference"></a>Azure Functions C# 개발자 참조

<!-- When updating this article, make corresponding changes to any duplicate content in functions-reference-csharp.md -->

이 문서는 .NET 클래스 라이브러리의 C#을 사용하여 Azure Functions를 개발하는 방법을 소개합니다.

Azure Functions는 C# 및 C# 스크립트 프로그래밍 언어를 지원합니다. [Azure Portal에서 C#을 사용하는 방법](functions-create-function-app-portal.md)에 대한 지침은 [C# 스크립트(.csx) 개발자 참조](functions-reference-csharp.md)를 참조하세요.

이 문서에서는 사용자가 이미 다음 문서를 읽었다고 가정합니다.

* [Azure Functions 개발자 가이드](functions-reference.md)
* [Azure Functions Visual Studio 2019 도구](functions-develop-vs.md)

## <a name="functions-class-library-project"></a>Functions 클래스 라이브러리 프로젝트

Visual Studio에서 **Azure Functions** 프로젝트 템플릿은 다음 파일이 포함된 C# 클래스 라이브러리 프로젝트를 만듭니다.

* [host.json](functions-host-json.md) -로컬로 또는 Azure에서 실행될 경우 프로젝트의 모든 함수에 영향을 주는 구성 설정을 저장합니다.
* [local.settings.json](functions-run-local.md#local-settings-file) - 로컬로 실행될 때 사용되는 앱 설정 및 연결 문자열을 저장합니다. 이 파일은 암호를 포함하며 Azure의 함수 앱에 게시되지 않습니다. 대신, [함수 앱에 앱 설정을 추가](functions-develop-vs.md#function-app-settings)합니다.

프로젝트를 빌드하면 빌드 출력 디렉터리에 다음 예제와 같은 폴더 구조가 생성 됩니다.

```
<framework.version>
 | - bin
 | - MyFirstFunction
 | | - function.json
 | - MySecondFunction
 | | - function.json
 | - host.json
```

이 디렉터리는 Azure의 함수 앱에 배포되는 디렉터리입니다. Functions 런타임의 [버전 2.x](functions-versions.md)에 필요한 바인딩 확장은 [NuGet 패키지로 프로젝트에 추가](./functions-bindings-register.md#vs)됩니다.

> [!IMPORTANT]
> 빌드 프로세스는 각 함수에 대해 *function.json* 파일을 만듭니다. 이 *function.json* 파일은 직접 편집할 수 없습니다. 이 파일을 편집하여 바인딩 구성을 변경하거나 함수를 사용하지 않도록 설정할 수 없습니다. 함수를 사용하지 않도록 설정하는 방법을 알아보려면 [함수를 사용하지 않도록 설정하는 방법](disable-function.md#functions-2x---c-class-libraries)을 참조하세요.

## <a name="methods-recognized-as-functions"></a>함수로 인식되는 메서드

클래스 라이브러리에서 함수는 다음 예제와 같이 `FunctionName` 및 트리거 특성을 포함하는 정적 메서드입니다.

```csharp
public static class SimpleExample
{
    [FunctionName("QueueTrigger")]
    public static void Run(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        ILogger log)
    {
        log.LogInformation($"C# function processed: {myQueueItem}");
    }
} 
```

`FunctionName` 특성은 메서드를 함수 진입점으로 표시합니다. 이름은 프로젝트 내에서 고유 해야 하 고, 문자로 시작 하 고, 문자, 숫자, `_`및 `-`문자 (최대 127 자)를 포함 해야 합니다. 프로젝트 템플릿에서 `Run` 메서드를 자주 만들지만, 유효한 C# 이름은 모두 메서드 이름이 될 수 있습니다.

트리거 특성은 트리거 유형을 지정하고, 입력 데이터를 메서드 매개 변수에 바인딩합니다. 예제 함수는 큐 메시지에 의해 트리거되며, 큐 메시지는 `myQueueItem` 매개 변수의 메서드에 전달됩니다.

## <a name="method-signature-parameters"></a>메서드 서명 매개 변수

트리거 특성에 사용되는 매개 변수 이외의 매개 변수가 메서드 서명에 포함될 수 있습니다. 다음은 포함할 수 있는 추가 매개 변수 중 일부입니다.

* 특성으로 데코레이팅하여 표시된 [입력 및 출력 바인딩](functions-triggers-bindings.md).  
* [로깅](#logging)에 대한 `ILogger` 또는 `TraceWriter`([버전 1.x 전용](functions-versions.md#creating-1x-apps)) 매개 변수.
* [정상 종료](#cancellation-tokens)를 위한 `CancellationToken` 매개 변수.
* 트리거 메타데이터를 가져오는 [바인딩 식](./functions-bindings-expressions-patterns.md) 매개 변수.

함수 시그니처에서 매개 변수의 순서는 중요하지 않습니다. 예를 들어, 다른 바인딩 전후에 트리거 매개 변수를 추가하고, 트리거 또는 바인딩 매개 변수 전후에 로거 매개 변수를 추가할 수 있습니다.

### <a name="output-binding-example"></a>출력 바인딩 예제

다음 예제에서는 출력 큐 바인딩을 추가하여 이전 예제를 수정합니다. 이 함수는 함수를 다른 큐의 새 큐 메시지로 트리거하는 큐 메시지를 씁니다.

```csharp
public static class SimpleExampleWithOutput
{
    [FunctionName("CopyQueueMessage")]
    public static void Run(
        [QueueTrigger("myqueue-items-source")] string myQueueItem, 
        [Queue("myqueue-items-destination")] out string myQueueItemCopy,
        ILogger log)
    {
        log.LogInformation($"CopyQueueMessage function processed: {myQueueItem}");
        myQueueItemCopy = myQueueItem;
    }
}
```

바인딩 참조 문서(예: [저장소 큐](functions-bindings-storage-queue.md))는 트리거, 입력 또는 출력 바인딩 특성에 사용할 수 있는 매개 변수 형식을 설명합니다.

### <a name="binding-expressions-example"></a>바인딩 식 예제

다음 코드는 앱 설정에서 모니터링할 큐의 이름을 가져오고, `insertionTime` 매개 변수에서 큐 메시지 작성 시간을 가져옵니다.

```csharp
public static class BindingExpressionsExample
{
    [FunctionName("LogQueueMessage")]
    public static void Run(
        [QueueTrigger("%queueappsetting%")] string myQueueItem,
        DateTimeOffset insertionTime,
        ILogger log)
    {
        log.LogInformation($"Message content: {myQueueItem}");
        log.LogInformation($"Created at: {insertionTime}");
    }
}
```

## <a name="autogenerated-functionjson"></a>자동 생성된 function.json

빌드 프로세스는 build 폴더의 function 폴더에 *function.json* 파일을 만듭니다. 앞에서 설명한 대로 이 파일은 직접 편집할 수 없습니다. 이 파일을 편집하여 바인딩 구성을 변경하거나 함수를 사용하지 않도록 설정할 수 없습니다. 

이 파일은 [소비 계획에 대한 크기 결정](functions-scale.md#how-the-consumption-and-premium-plans-work)에 사용하기 위해 크기 조정 컨트롤러에 정보를 제공하는 데 필요합니다. 이러한 이유로 이 파일에는 트리거 정보만 있고 입력 또는 출력 바인딩은 없습니다.

생성된 *function.json* 파일에는 바인딩에 *function.json* 구성 대신 .NET 특성을 사용하도록 런타임에 지시하는 `configurationSource` 속성이 포함되어 있습니다. 예를 들면 다음과 같습니다.

```json
{
  "generatedBy": "Microsoft.NET.Sdk.Functions-1.0.0.0",
  "configurationSource": "attributes",
  "bindings": [
    {
      "type": "queueTrigger",
      "queueName": "%input-queue-name%",
      "name": "myQueueItem"
    }
  ],
  "disabled": false,
  "scriptFile": "..\\bin\\FunctionApp1.dll",
  "entryPoint": "FunctionApp1.QueueTrigger.Run"
}
```

## <a name="microsoftnetsdkfunctions"></a>Microsoft.NET.Sdk.Functions

*function.json* 파일 생성은 [Microsoft\.NET\.Sdk\.Functions](https://www.nuget.org/packages/Microsoft.NET.Sdk.Functions) NuGet 패키지에서 수행됩니다. 

버전 1.x 및 2.x의 Functions 런타임 둘 다에 동일한 패키지가 사용됩니다. 대상 프레임워크가 1.x 프로젝트와 2.x 프로젝트에서 구분되는 측면입니다. 다음은 *.csproj* 파일의 관련 부분으로, 다른 대상 프레임워크와 동일한 `Sdk` 패키지를 보여 줍니다.

**Functions 1.x**

```xml
<PropertyGroup>
  <TargetFramework>net461</TargetFramework>
</PropertyGroup>
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Sdk.Functions" Version="1.0.8" />
</ItemGroup>
```

**Functions 2.x**

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.1</TargetFramework>
  <AzureFunctionsVersion>v2</AzureFunctionsVersion>
</PropertyGroup>
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Sdk.Functions" Version="1.0.8" />
</ItemGroup>
```

`Sdk` 패키지 종속성에는 트리거 및 바인딩이 있습니다. 1\.x 프로젝트는 1.x 트리거와 바인딩이 .NET Framework 대상으로 하는 반면, 2.x 트리거와 바인딩은 .NET Core를 대상으로 하기 때문에 1. x 트리거 및 바인딩을 참조 합니다.

`Sdk` 패키지는 [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json)에도 종속되며, [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage)에는 간접적으로 종속됩니다. 이러한 종속성 때문에 프로젝트는 대상이 되는 Functions 런타임 버전에서 작동하는 패키지 버전을 사용하게 됩니다. 예를 들어, `Newtonsoft.Json`에는 .NET Framework 4.6.1용 버전 11이 있지만, .NET Framework 4.6.1을 대상으로 하는 Functions 런타임은 `Newtonsoft.Json` 9.0.1과만 호환됩니다. 따라서 해당 프로젝트의 함수 코드도 `Newtonsoft.Json` 9.0.1을 사용해야 합니다.

`Microsoft.NET.Sdk.Functions`의 소스 코드는 [azure\-functions\-vs\-build\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk) GitHub 리포지토리에서 사용할 수 있습니다.

## <a name="runtime-version"></a>런타임 버전

Visual Studio는 [Azure Functions 핵심 도구](functions-run-local.md#install-the-azure-functions-core-tools)를 사용하여 Functions 프로젝트를 실행합니다. 핵심 도구는 Function 런타임에 대한 명령줄 인터페이스입니다.

npm을 사용하여 핵심 도구를 설치하는 경우 Visual Studio에서 사용되는 핵심 도구 버전에 영향을 주지 않습니다. Functions 런타임 버전 1.x의 경우, Visual Studio는 *%USERPROFILE%\AppData\Local\Azure.Functions.Cli*에 핵심 도구 버전을 저장하고 해당 위치에 저장된 최신 버전을 사용합니다. Functions 2.x의 경우, 핵심 도구는 **Azure Functions 및 Web Jobs Tools** 확장에 포함되어 있습니다. 1\.x 및 2.x 둘 다, Functions 프로젝트를 실행할 때 콘솔 출력에 사용되는 버전을 확인할 수 있습니다.

```terminal
[3/1/2018 9:59:53 AM] Starting Host (HostId=contoso2-1518597420, Version=2.0.11353.0, ProcessId=22020, Debug=False, Attempt=0, FunctionsExtensionVersion=)
```

## <a name="supported-types-for-bindings"></a>바인딩에 대해 지원되는 형식

각 바인딩에는 자체적인 지원 형식이 있습니다. 예를들 어, Blob 트리거 특성은 문자열 매개 변수, POCO 매개 변수, `CloudBlockBlob` 매개 변수 또는 지원되는 기타 몇 가지 형식에 적용될 수 있습니다. [Blob 바인딩에 대한 바인딩 참조 문서](functions-bindings-storage-blob.md#trigger---usage)에는 지원되는 모든 매개 변수 형식이 나와 있습니다. 자세한 내용은 [트리거 및 바인딩](functions-triggers-bindings.md) 및 [각 바인딩 형식에 대한 바인딩 참조 문서](functions-triggers-bindings.md#next-steps)를 참조하세요.

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="binding-to-method-return-value"></a>메서드 반환 값에 바인딩

메서드 반환 값에 특성을 적용하여 출력 바인딩에 대한 메서드 반환 값을 사용할 수 있습니다. 예제를 보려면 [트리거 및 바인딩](./functions-bindings-return-value.md)을 참조하세요. 

성공적인 함수 실행이 항상 출력 바인딩으로 전달할 반환 값을 생성하는 경우에만 반환 값을 사용합니다. 그렇지 않으면 다음 섹션에 나와 있는 것처럼 `ICollector` 또는 `IAsyncCollector`를 사용합니다.

## <a name="writing-multiple-output-values"></a>여러 출력 값 쓰기

출력 바인딩에 여러 값을 쓰거나 성공적인 함수 호출이 출력 바인딩에 전달할 값을 생성하지 않는 경우 [`ICollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) 또는 [`IAsyncCollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) 형식을 사용합니다. 이러한 형식은 메서드가 완료될 때 출력 바인딩에 기록되는 쓰기 전용 컬렉션입니다.

이 예제에서는 `ICollector`를 사용하여 동일한 큐에 여러 큐 메시지를 씁니다.

```csharp
public static class ICollectorExample
{
    [FunctionName("CopyQueueMessageICollector")]
    public static void Run(
        [QueueTrigger("myqueue-items-source-3")] string myQueueItem,
        [Queue("myqueue-items-destination")] ICollector<string> myDestinationQueue,
        ILogger log)
    {
        log.LogInformation($"C# function processed: {myQueueItem}");
        myDestinationQueue.Add($"Copy 1: {myQueueItem}");
        myDestinationQueue.Add($"Copy 2: {myQueueItem}");
    }
}
```

## <a name="logging"></a>로깅

C#의 스트리밍 로그에 대한 출력을 기록하려면 [ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.ilogger) 형식의 인수를 포함합니다. 다음 예제와 같이 `log`로 이름을 지정하는 것이 좋습니다.  

```csharp
public static class SimpleExample
{
    [FunctionName("QueueTrigger")]
    public static void Run(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        ILogger log)
    {
        log.LogInformation($"C# function processed: {myQueueItem}");
    }
} 
```

Azure Functions에서 `Console.Write`를 사용하지 마세요. 자세한 내용은 **Azure Functions 모니터링** 문서에서 [C# 함수로 로그 작성](functions-monitoring.md#write-logs-in-c-functions)을 참조하세요.

## <a name="async"></a>Async

[비동기화](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/) 함수를 만들려면 `async` 키워드를 사용하고 `Task` 개체를 반환합니다.

```csharp
public static class AsyncExample
{
    [FunctionName("BlobCopy")]
    public static async Task RunAsync(
        [BlobTrigger("sample-images/{blobName}")] Stream blobInput,
        [Blob("sample-images-copies/{blobName}", FileAccess.Write)] Stream blobOutput,
        CancellationToken token,
        ILogger log)
    {
        log.LogInformation($"BlobCopy function processed.");
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
}
```

`out` 매개 변수는 비동기 함수에 사용할 수 없습니다. 출력 바인딩에는 [함수 반환 값](#binding-to-method-return-value) 또는 [수집기 개체](#writing-multiple-output-values)를 대신 사용합니다.

## <a name="cancellation-tokens"></a>취소 토큰

함수는 함수가 종료될 때 운영 체제가 코드에 알릴 수 있게 해주는 [CancellationToken](/dotnet/api/system.threading.cancellationtoken) 매개 변수를 사용할 수 있습니다. 이 알림을 통해 함수가 예기치 않게 종료되어 데이터가 일관되지 않은 상태가 되는 것을 방지할 수 있습니다.

다음 예제에서는 임박한 함수 종료를 확인하는 방법을 보여 줍니다.

```csharp
public static class CancellationTokenExample
{
    public static void Run(
        [QueueTrigger("inputqueue")] string inputText,
        TextWriter logger,
        CancellationToken token)
    {
        for (int i = 0; i < 100; i++)
        {
            if (token.IsCancellationRequested)
            {
                logger.WriteLine("Function was cancelled at iteration {0}", i);
                break;
            }
            Thread.Sleep(5000);
            logger.WriteLine("Normal processing for queue message={0}", inputText);
        }
    }
}
```

## <a name="environment-variables"></a>환경 변수

환경 변수 또는 앱 설정 값을 가져오려면 다음 코드 예제와 같이 `System.Environment.GetEnvironmentVariable`을 사용합니다.

```csharp
public static class EnvironmentVariablesExample
{
    [FunctionName("GetEnvironmentVariables")]
    public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, ILogger log)
    {
        log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");
        log.LogInformation(GetEnvironmentVariable("AzureWebJobsStorage"));
        log.LogInformation(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
    }

    public static string GetEnvironmentVariable(string name)
    {
        return name + ": " +
            System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
    }
}
```

앱 설정은 로컬로 개발할 때와 Azure에서 실행할 때 환경 변수에서 읽을 수 있습니다. 로컬로 개발할 때 앱 설정은 `Values`local.settings.json*파일의* 컬렉션에서 가져옵니다. 로컬 및 Azure의 두 환경에서 `GetEnvironmentVariable("<app setting name>")`은 명명된 앱 설정의 값을 검색합니다. 예를 들어 로컬로 실행하는 경우 *local.settings.json* 파일에 `{ "Values": { "WEBSITE_SITE_NAME": "My Site Name" } }`이 포함된 경우 "My Site Name"이 반환됩니다.

[System.Configuration.ConfigurationManager.AppSettings](https://docs.microsoft.com/dotnet/api/system.configuration.configurationmanager.appsettings) 속성은 앱 설정 값을 가져오는 대체 API지만, 다음과 같이 `GetEnvironmentVariable`을 사용하는 것이 좋습니다.

## <a name="binding-at-runtime"></a>런타임에 바인딩

C# 및 기타 .NET 언어에서는 특성의 [*declarative*](https://en.wikipedia.org/wiki/Declarative_programming) 바인딩과 달리 [명령적](https://en.wikipedia.org/wiki/Imperative_programming) 바인딩 패턴을 사용할 수 있습니다. 명령적 바인딩은 바인딩 매개 변수를 디자인 타임이 아닌 런타임에 계산해야 할 경우 유용합니다. 이 패턴을 사용하면 함수 코드에서 지원되는 입력 및 출력 바인딩을 즉시 바인딩할 수 있습니다.

다음과 같이 명령적 바인딩을 정의합니다.

- 원하는 명령적 바인딩에 대한 함수 시그니처에 특성을 포함하지 **마세요**.
- 입력 매개 변수 [`Binder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) 또는 [`IBinder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs)에 전달합니다.
- 다음 C# 패턴을 사용하여 데이터 바인딩을 수행합니다.

  ```cs
  using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
  {
      ...
  }
  ```

  `BindingTypeAttribute`는 바인딩을 정의하는 .NET 특성이며, `T`는 해당 바인딩 형식에서 지원되는 입력 또는 출력 형식입니다. `T`는 `out` 매개 변수 형식(예: `out JObject`)일 수 없습니다. 예를 들어 Mobile Apps 테이블 출력 바인딩은 [6 개의 출력 형식을](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22)지원 하지만 명령적 바인딩과 함께 [\<ICollector T >](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) 또는 [iasynccollector\<T >](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) 만 사용할 수 있습니다.

### <a name="single-attribute-example"></a>단일 특성 예제

다음 예제 코드에서는 런타임에서 정의된 Blob경로를 사용하는 [Storage Blob 출력 바인딩](functions-bindings-storage-blob.md#output)을 만든 다음, Blob에 문자열을 씁니다.

```cs
public static class IBinderExample
{
    [FunctionName("CreateBlobUsingBinder")]
    public static void Run(
        [QueueTrigger("myqueue-items-source-4")] string myQueueItem,
        IBinder binder,
        ILogger log)
    {
        log.LogInformation($"CreateBlobUsingBinder function processed: {myQueueItem}");
        using (var writer = binder.Bind<TextWriter>(new BlobAttribute(
                    $"samples-output/{myQueueItem}", FileAccess.Write)))
        {
            writer.Write("Hello World!");
        };
    }
}
```

[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs)는 [Storage Blob](functions-bindings-storage-blob.md) 입력 또는 출력 바인딩을 정의하며, [TextWriter](/dotnet/api/system.io.textwriter)는 지원되는 출력 바인딩 형식입니다.

### <a name="multiple-attribute-example"></a>다중 특성 예제

앞의 예제에서는 함수 앱의 주 Storage 계정 연결 문자열(`AzureWebJobsStorage`)에 대한 앱 설정을 가져옵니다. [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)를 추가하고 `BindAsync<T>()`에 특성 배열을 전달하여 스토리지 계정에 사용할 사용자 지정 앱 설정을 지정할 수 있습니다. `IBinder`가 아닌 `Binder` 매개 변수를 사용합니다.  예를 들어:

```cs
public static class IBinderExampleMultipleAttributes
{
    [FunctionName("CreateBlobInDifferentStorageAccount")]
    public async static Task RunAsync(
            [QueueTrigger("myqueue-items-source-binder2")] string myQueueItem,
            Binder binder,
            ILogger log)
    {
        log.LogInformation($"CreateBlobInDifferentStorageAccount function processed: {myQueueItem}");
        var attributes = new Attribute[]
        {
        new BlobAttribute($"samples-output/{myQueueItem}", FileAccess.Write),
        new StorageAccountAttribute("MyStorageAccount")
        };
        using (var writer = await binder.BindAsync<TextWriter>(attributes))
        {
            await writer.WriteAsync("Hello World!!");
        }
    }
}
```

## <a name="triggers-and-bindings"></a>트리거 및 바인딩 

[!INCLUDE [Supported triggers and bindings](../../includes/functions-bindings.md)]

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [트리거 및 바인딩에 대해 자세히 알아보기](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Azure Functions에 대한 모범 사례에 대해 자세히 알아보기](functions-best-practices.md)
