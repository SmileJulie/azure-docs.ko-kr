---
title: '빠른 시작: 텍스트 언어 검색, C# - Translator Text API'
titleSuffix: Azure Cognitive Services
description: 이 빠른 시작에서는 .NET Core 및 Translator Text REST API를 사용하여 제공된 텍스트의 언어를 감지하는 방법을 알아봅니다.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/13/2019
ms.author: swmachan
ms.openlocfilehash: a445d9244e08e6cd8a71334aa1fabddb677544c4
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705682"
---
# <a name="quickstart-use-the-translator-text-api-to-detect-text-language-using-c"></a>빠른 시작: Translator Text API를 사용하여 C#을 통해 텍스트 언어 검색

이 빠른 시작에서는 .NET Core, C# 7.1 이상 및 Translator Text REST API를 사용하여 제공된 텍스트의 언어를 감지하는 방법을 알아봅니다.

이 빠른 시작에는Translator Text 리소스와 함께 [Azure Cognitive Services 계정](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)이 필요합니다. 계정이 없는 경우 [평가판](https://azure.microsoft.com/try/cognitive-services/)을 사용하여 구독 키를 가져올 수 있습니다.

>[!TIP]
> 모든 코드를 한 번에 보려면 이 샘플의 소스 코드를 [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-C-Sharp)에서 사용할 수 있습니다.

## <a name="prerequisites"></a>필수 조건

* C# 7.1 이상
* [.NET SDK](https://www.microsoft.com/net/learn/dotnet/hello-world-tutorial)
* [Json.NET NuGet 패키지](https://www.nuget.org/packages/Newtonsoft.Json/)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) 또는 즐겨 사용하는 텍스트 편집기
* Translator Text에 대한 Azure 구독 키

## <a name="create-a-net-core-project"></a>.NET Core 프로젝트 만들기

새 명령 프롬프트(또는 터미널 세션)를 열고 이 명령을 실행합니다.

```console
dotnet new console -o detect-sample
cd detect-sample
```

첫 번째 명령은 두 가지 작업을 수행합니다. 새 .NET 콘솔 애플리케이션을 만들고 `detect-sample`이라는 디렉터리를 만듭니다. 두 번째 명령은 프로젝트의 디렉터리로 변경합니다.

그런 다음, Json.Net을 설치해야 합니다. 프로젝트의 디렉터리에서 다음을 실행합니다.

```console
dotnet add package Newtonsoft.Json --version 11.0.2
```

## <a name="select-the-c-language-version"></a>C# 언어 버전 선택

이 빠른 시작에는 C# 7.1 이상이 필요합니다. 프로젝트를 위해 C# 버전을 변경하는 몇 가지 방법이 있습니다. 이 가이드에서는 `detect-sample.csproj` 파일을 조정하는 방법을 설명합니다. Visual Studio에서 사용 가능한 모든 옵션(예: 언어 변경)에 대해서는 [C# 언어 버전 선택](https://docs.microsoft.com/dotnet/csharp/language-reference/configure-language-version)을 참조하세요.

프로젝트를 연 다음 `detect-sample.csproj`를 엽니다. `LangVersion`이 7.1 이상으로 설정되어 있는지 확인합니다. 언어 버전에 대한 속성 그룹이 없는 경우 다음 행을 추가합니다.

```xml
<PropertyGroup>
   <LangVersion>7.1</LangVersion>
</PropertyGroup>
```

## <a name="add-required-namespaces-to-your-project"></a>프로젝트에 필요한 네임스페이스 추가

이전에 실행한 `dotnet new console` 명령은 `Program.cs`를 포함한 프로젝트를 만듭니다. 이 파일은 애플리케이션 코드가 보관되는 곳입니다. `Program.cs`를 열고 문을 사용하여 기존 항목을 바꿉니다. 이러한 문은 빌드하고 샘플 앱을 빌드하고 실행하는 데 필요한 모든 형식에 액세스할 수 있는지 확인합니다.

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
// Install Newtonsoft.Json with NuGet
using Newtonsoft.Json;
```

## <a name="create-classes-for-the-json-response"></a>JSON 응답을 위한 클래스 만들기

다음으로, Translator Text API에서 반환된 JSON 응답을 역직렬화할 때 사용되는 클래스를 만듭니다.

```csharp
/// <summary>
/// The C# classes that represents the JSON returned by the Translator Text API.
/// </summary>
public class DetectResult
{
    public string Language { get; set; }
    public float Score { get; set; }
    public bool IsTranslationSupported { get; set; }
    public bool IsTransliterationSupported { get; set; }
    public AltTranslations[] Alternatives { get; set; }
}
public class AltTranslations
{
    public string Language { get; set; }
    public float Score { get; set; }
    public bool IsTranslationSupported { get; set; }
    public bool IsTransliterationSupported { get; set; }
}
```

## <a name="create-a-function-to-detect-the-source-texts-language"></a>원본 텍스트의 언어를 감지하는 함수 만들기

`Program` 클래스 내에서 `DetectTextRequest()`라는 함수를 만듭니다. 이 클래스는 Detect 리소스를 호출하는 데 사용되는 코드를 캡슐화하고 콘솔에 결과를 출력합니다.

```csharp
static public async Task DetectTextRequest(string subscriptionKey, string host, string route, string inputText)
{
  /*
   * The code for your call to the translation service will be added to this
   * function in the next few sections.
   */
}
```

## <a name="serialize-the-detect-request"></a>검색 요청 직렬화

다음으로, 언어 감지 대상 텍스트를 포함하는 JSON 개체를 만들고 직렬화해야 합니다.

```csharp
System.Object[] body = new System.Object[] { new { Text = inputText } };
var requestBody = JsonConvert.SerializeObject(body);
```

## <a name="instantiate-the-client-and-make-a-request"></a>클라이언트를 인스턴스화하고 요청 수행

이러한 줄은 `HttpClient` 및 `HttpRequestMessage`를 인스턴스화합니다.

```csharp
using (var client = new HttpClient())
using (var request = new HttpRequestMessage())
{
  // In the next few sections you'll add code to construct the request.
}
```

## <a name="construct-the-request-and-print-the-response"></a>요청 만들기 및 응답 출력

`HttpRequestMessage` 내부에서 다음을 수행합니다.

* HTTP 메서드 선언
* 요청 URI 만들기
* 요청 본문(직렬화된 JSON 개체) 삽입
* 필수 헤더 추가
* 비동기 요청 수행
* 응답 출력

`HttpRequestMessage`에 이 코드를 추가합니다.

```csharp
// Build the request.
request.Method = HttpMethod.Post;
// Construct the URI and add headers.
request.RequestUri = new Uri(host + route);
request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

// Send the request and get response.
HttpResponseMessage response = await client.SendAsync(request).ConfigureAwait(false);
// Read response as a string.
string result = await response.Content.ReadAsStringAsync();
// Deserialize the response using the classes created earlier.
DetectResult[] deserializedOutput = JsonConvert.DeserializeObject<DetectResult[]>(result);
// Iterate over the deserialized response.
foreach (DetectResult o in deserializedOutput)
{
    Console.WriteLine("The detected language is '{0}'. Confidence is: {1}.\nTranslation supported: {2}.\nTransliteration supported: {3}.\n",
        o.Language, o.Score, o.IsTranslationSupported, o.IsTransliterationSupported);
    // Create a counter
    int counter = 0;
    // Iterate over alternate translations.
    foreach (AltTranslations a in o.Alternatives)
    {
        counter++;
        Console.WriteLine("Alternative {0}", counter);
        Console.WriteLine("The detected language is '{0}'. Confidence is: {1}.\nTranslation supported: {2}.\nTransliteration supported: {3}.\n",
            a.Language, a.Score, a.IsTranslationSupported, a.IsTransliterationSupported);
    }
}
```

Cognitive Services 다중 서비스 구독을 사용하는 경우 요청 매개 변수에 `Ocp-Apim-Subscription-Region`도 포함해야 합니다. [다중 서비스 구독을 사용한 인증에 대해 자세히 알아봅니다](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="put-it-all-together"></a>모든 요소 결합

마지막 단계는 `Main` 함수에서 `DetectTextRequest()`를 호출하는 것입니다. `static void Main(string[] args)`를 찾아 다음 코드로 바꿉니다.

```csharp
static async Task Main(string[] args)
{
    // This is our main function.
    // Output languages are defined in the route.
    // For a complete list of options, see API reference.
    string subscriptionKey = "YOUR_TRANSLATOR_TEXT_KEY_GOES_HERE";
    string host = "https://api.cognitive.microsofttranslator.com";
    string route = "/detect?api-version=3.0";
    string breakSentenceText = @"How are you doing today? The weather is pretty pleasant. Have you been to the movies lately?";
    await DetectTextRequest(subscriptionKey, host, route, breakSentenceText);
}
```
## <a name="run-the-sample-app"></a>샘플 앱 실행

이제 끝났습니다. 샘플 앱을 실행할 준비가 되었습니다. 명령줄(또는 터미널 세션)에서 프로젝트 디렉터리로 이동하고 다음을 실행합니다.

```console
dotnet run
```

## <a name="sample-response"></a>샘플 응답

샘플을 실행하면 터미널에 다음이 출력됩니다.

> [!NOTE]
> 국가/지역 약어는 이 [언어 목록](https://docs.microsoft.com/azure/cognitive-services/translator/language-support)에서 확인하세요.

```bash
The detected language is 'en'. Confidence is: 1.
Translation supported: True.
Transliteration supported: False.

Alternative 1
The detected language is 'fil'. Confidence is: 0.82.
Translation supported: True.
Transliteration supported: False.

Alternative 2
The detected language is 'ro'. Confidence is: 1.
Translation supported: True.
Transliteration supported: False.
```

이 메시지는 다음과 같이 표시되며 원시 JSON에서 빌드됩니다.

```json
[  
    {  
        "language":"en",
        "score":1.0,
        "isTranslationSupported":true,
        "isTransliterationSupported":false,
        "alternatives":[  
            {  
                "language":"fil",
                "score":0.82,
                "isTranslationSupported":true,
                "isTransliterationSupported":false
            },
            {  
                "language":"ro",
                "score":1.0,
                "isTranslationSupported":true,
                "isTransliterationSupported":false
            }
        ]
    }
]
```

## <a name="clean-up-resources"></a>리소스 정리

샘플 앱의 소스 코드에서 구독 키와 같은 기밀 정보를 제거해야 합니다.

## <a name="next-steps"></a>다음 단계

Translator Text API로 할 수 있는 모든 것에 대해 알아보려면 API 참조를 살펴보세요.

> [!div class="nextstepaction"]
> [API 참조](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)

## <a name="see-also"></a>참고 항목

* [텍스트 번역](quickstart-csharp-translate.md)
* [텍스트 음역](quickstart-csharp-transliterate.md)
* [대체 번역 가져오기](quickstart-csharp-dictionary.md)
* [지원되는 언어 목록 가져오기](quickstart-csharp-languages.md)
* [입력으로 문장 길이 확인](quickstart-csharp-sentences.md)
