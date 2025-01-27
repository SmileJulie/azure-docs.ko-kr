---
title: .NET 애플리케이션에서 Azure Search를 사용하는 방법 - Azure Search
description: C# 및 .NET SDK를 사용하여 .NET 애플리케이션에서 Azure Search를 사용하는 방법을 알아봅니다. 코드 기반 작업에는 서비스에 연결, 콘텐츠 인덱싱 및 인덱스 쿼리가 포함됩니다.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: brjohnst
ms.openlocfilehash: 9f0af40d442747181636b50612f7d2162ead6a86
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67450008"
---
# <a name="how-to-use-azure-search-from-a-net-application"></a>.NET 애플리케이션에서 Azure Search를 사용하는 방법

이 문서는 [Azure Search .NET SDK](https://aka.ms/search-sdk)를 준비하여 실행하기 위한 연습입니다. Azure Search를 사용하여 애플리케이션에서 풍부한 검색 환경을 구현하는 .NET SDK를 사용할 수 있습니다.

## <a name="whats-in-the-azure-search-sdk"></a>Azure Search SDK의 주요 기능
SDK는 HTTP와 JSON의 세부 정보를 처리하지 않고도 인덱스, 데이터 원본, 인덱서, 동의어 맵을 관리할 뿐만 아니라 문서를 업로드 및 관리하고 쿼리를 실행할 수 있는 클라이언트 라이브러리 몇 개로 구성되어 있습니다. 이러한 클라이언트 라이브러리는 모두 NuGet 패키지로 배포됩니다.

기본 NuGet 패키지는 `Microsoft.Azure.Search`이며, 이는 다른 모든 패키지를 종속 항목으로 포함하는 메타 패키지입니다. 처음 시작하는 경우나 애플리케이션에 Azure Search의 모든 기능이 필요한 경우, 이 패키지를 사용합니다.

SDK의 다른 NuGet 패키지는 다음과 같습니다.
 
  - `Microsoft.Azure.Search.Data`: Azure Search를 사용하여 .NET 애플리케이션을 개발하는 경우 이 패키지를 사용합니다. 인덱스에서 문서를 쿼리하거나 업데이트하기만 하면 됩니다. 인덱스, 동의어 맵 또는 서비스 수준의 다른 리소스도 만들거나 업데이트해야 하는 경우에는 대신 `Microsoft.Azure.Search` 패키지를 사용합니다.
  - `Microsoft.Azure.Search.Service`: Azure Search 인덱스, 동의어 맵, 인덱서, 데이터 원본 또는 서비스 수준의 다른 리소스를 관리하기 위해 .NET에서 자동화를 개발하는 경우 이 패키지를 사용합니다. 인덱스에서 문서를 쿼리하거나 업데이트하기만 하면 되는 경우에는 대신 `Microsoft.Azure.Search.Data` 패키지를 사용합니다. Azure Search의 모든 기능이 필요한 경우에는 대신 `Microsoft.Azure.Search` 패키지를 사용합니다.
  - `Microsoft.Azure.Search.Common`: Azure Search .NET 라이브러리에 필요한 일반 형식입니다. 응용 프로그램에서 직접이 패키지를 사용할 필요가 없습니다. 종속성으로 사용 하는 기능만 합니다.

다양한 클라이언트 라이브러리가 `Index`, `Field`, `Document` 등의 클래스뿐만 아니라 `SearchServiceClient` 및 `SearchIndexClient` 클래스의 `Indexes.Create` 및 `Documents.Search` 등을 정의합니다. 이러한 클래스는 다음과 같은 네임 스페이스에 구성됩니다.

* [Microsoft.Azure.Search](https://docs.microsoft.com/dotnet/api/microsoft.azure.search)
* [Microsoft.Azure.Search.Models](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models)

SDK의 향후 업데이트에 대 한 피드백을 제공 하려는 경우 참조 우리의 [피드백 페이지](https://feedback.azure.com/forums/263029-azure-search/) 에서 문제를 만들거나 [GitHub](https://github.com/azure/azure-sdk-for-net/issues) 문제 제목에서 "Azure Search"을 언급 하 고 있습니다.

.NET SDK는 [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/)의 `2019-05-06` 버전을 지원합니다. 이 버전에 대 한 지원도 [복합 형식](search-howto-complex-data-types.md), [cognitive search](cognitive-search-concept-intro.md)를 [자동 완성](https://docs.microsoft.com/rest/api/searchservice/autocomplete), 및 [JsonLines 구문 분석 모드](search-howto-index-json-blobs.md) 때 Azure Blob 인덱싱. 

이 SDK는 Search 서비스 생성 및 확장, API 키 관리 등의 [관리 작업](https://docs.microsoft.com/rest/api/searchmanagement/)을 지원하지 않습니다. .NET 애플리케이션에서 Search 리소스를 관리해야 하는 경우 [Azure Search .NET 관리 SDK](https://aka.ms/search-mgmt-sdk)를 사용할 수 있습니다.

## <a name="upgrading-to-the-latest-version-of-the-sdk"></a>최신 버전의 SDK로 업그레이드
이전 버전의 Azure Search.NET SDK를 이미 사용 중이 고 최신 버전으로 업그레이드 하려는 경우 일반 공급 버전인 [이 문서에서는](search-dotnet-sdk-migration-version-9.md) 에 대해 설명 하는 방법입니다.

## <a name="requirements-for-the-sdk"></a>SDK의 요구 사항
1. Visual Studio 2017 이상입니다.
2. Azure Search 서비스 SDK를 사용하려면 서비스 이름과 하나 이상의 API 키가 필요합니다. [포털에서 서비스 만들기](search-create-service-portal.md)는 이들 단계를 통해 도움을 받을 수 있습니다.
3. Visual Studio에서 "NuGet 패키지 관리"를 사용하여 Azure Search .NET SDK [NuGet 패키지](https://www.nuget.org/packages/Microsoft.Azure.Search)를 다운로드하십시오. NuGet.org에서 패키지 이름 `Microsoft.Azure.Search`를 검색하기만 하면 됩니다. 아니면 기능 하위 집합만 필요한 경우에는 위에 나온 다른 패키지 이름 중 하나를 검색합니다.

Azure Search.NET SDK는.NET Framework 4.5.2를 대상으로 하는 응용 프로그램을 지원 뿐만 아니라.NET Core 2.0 이상으로 이상.

## <a name="core-scenarios"></a>핵심 시나리오
검색 애플리케이션에서 수행해야 할 몇 가지 작업이 있습니다. 이 자습서에서는 이러한 핵심 시나리오를 다룹니다.

* 인덱스 만들기
* 문서를 사용하여 인덱스 채우기
* 전체 텍스트 검색 및 필터를 사용하여 문서 검색하기

다음 샘플 코드에서는 이러한 각 시나리오를 보여 줍니다. 사용자 애플리케이션에서 코드 조각을 자유롭게 사용하세요.

### <a name="overview"></a>개요
"호텔"이라는 이름의 새로운 인덱스를 탐색하려는 샘플 애플리케이션은 몇몇 문서로 채운 다음, 일부 검색 쿼리를 실행합니다. 전체 흐름을 보여주는 주 프로그램은 다음과 같습니다.

```csharp
// This sample shows how to delete, create, upload documents and query an index
static void Main(string[] args)
{
    IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
    IConfigurationRoot configuration = builder.Build();

    SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);

    string indexName = configuration["SearchIndexName"];

    Console.WriteLine("{0}", "Deleting index...\n");
    DeleteIndexIfExists(indexName, serviceClient);

    Console.WriteLine("{0}", "Creating index...\n");
    CreateIndex(indexName, serviceClient);

    ISearchIndexClient indexClient = serviceClient.Indexes.GetClient(indexName);

    Console.WriteLine("{0}", "Uploading documents...\n");
    UploadDocuments(indexClient);

    ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

    RunQueries(indexClientForQueries);

    Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
    Console.ReadKey();
}
```

> [!NOTE]
> [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo)에서 이 연습에 사용된 샘플 애플리케이션의 전체 소스 코드를 찾을 수 있습니다.
> 
>

단계별로 연습해보겠습니다. 먼저 새로운 `SearchServiceClient`을(를) 만들어야 합니다. 이 개체를 통해 인덱스를 관리할 수 있습니다. 하나를 생성하기 위해 Azure Search 서비스 이름과 관리 API 키를 제공해야 합니다. [샘플 애플리케이션](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo)의 `appsettings.json` 파일에 이 정보를 입력할 수 있습니다.

```csharp
private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string adminApiKey = configuration["SearchServiceAdminApiKey"];

    SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
    return serviceClient;
}
```

> [!NOTE]
> 잘못된 키(예, 관리 키가 필요한 쿼리 키)를 제공하는 경우, `SearchServiceClient`은(는) `Indexes.Create`와(과) 같은 작업 메서드를 처음 호출하면 "사용할 수 없음"이라는 오류 메시지와 함께 `CloudException`을(를) 발생시킵니다. 이 경우 API 키를 다시 확인하십시오.
> 
> 

다음 몇 줄은 "호텔"이라는 인덱스를 만드는 메서드를 호출하며, 이미 있는 경우 이를 먼저 삭제합니다. 잠시 후 이들 메서드를 연습해보겠습니다.

```csharp
Console.WriteLine("{0}", "Deleting index...\n");
DeleteIndexIfExists(indexName, serviceClient);

Console.WriteLine("{0}", "Creating index...\n");
CreateIndex(indexName, serviceClient);
```

그런 다음 인덱스를 채워야 합니다. 인덱스를 채우지, 하려면를 `SearchIndexClient`입니다. 두 가지 방법 즉, 키를 생성하거나 `SearchServiceClient`에서 `Indexes.GetClient`을(를) 호출하여 키를 얻을 수 있습니다. 편의를 위해 후자를 사용합니다.

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient(indexName);
```

> [!NOTE]
> 일반적인 검색 응용 프로그램에서 인덱스 관리 및 채우기 검색 쿼리에서 별도 구성 요소에서 처리할 수 있습니다. `Indexes.GetClient` 문제 제공 하는 추가 저장 하기 때문에 인덱스를 채우는 편리한 `SearchCredentials`합니다. 이는 새 `SearchIndexClient`에 `SearchServiceClient`을(를) 만드는 데 사용하는 관리 키를 눌러 수행됩니다. 그러나 쿼리를 실행 하는 응용 프로그램의 부분에서는 것이 좋습니다 만들려면는 `SearchIndexClient` 직접만 관리자 키 대신 데이터를 읽을 수 있도록 허용 되는 쿼리 키를 전달할 수 있도록 합니다. 이는 최소 권한의 원칙와 일치하고 애플리케이션을 더욱 안전하게 하는데 도움이 됩니다. 관리 키와 쿼리 키에 대한 자세한 내용은 [여기](https://docs.microsoft.com/rest/api/searchservice/#authentication-and-authorization)에서 확인할 수 있습니다.
> 
> 

이제 `SearchIndexClient`이 생겼고 인덱스를 채울 수 있습니다. 인덱스 채우기 과정을 나중에 다른 메서드에 의해 수행 됩니다.

```csharp
Console.WriteLine("{0}", "Uploading documents...\n");
UploadDocuments(indexClient);
```

마지막으로 몇 가지 검색 쿼리를 실행하고 결과를 표시합니다. 이번에는 다른 `SearchIndexClient`를 사용할 것입니다.

```csharp
ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(indexName, configuration);

RunQueries(indexClientForQueries);
```

`RunQueries` 메서드에 대해서는 이후에 좀 더 자세히 알아보겠습니다. 다음은 새 `SearchIndexClient`를 만드는 코드입니다.

```csharp
private static SearchIndexClient CreateSearchIndexClient(string indexName, IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, indexName, new SearchCredentials(queryApiKey));
    return indexClient;
}
```

이번에는 인덱스에 대해 쓰기 액세스 권한이 필요하지 않으므로 쿼리 키를 사용합니다. [샘플 애플리케이션](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo)의 `appsettings.json` 파일에 이 정보를 입력할 수 있습니다.

유효한 서비스 이름 및 API 키를 사용 하 여이 응용 프로그램을 실행 하는 경우 출력이 예제에서와 같이 표시 됩니다. (일부 콘솔 출력이 바뀌었습니다 "..."를 사용 하 여 예를 들기.)

    Deleting index...

    Creating index...

    Uploading documents...

    Waiting for documents to be indexed...

    Search the entire index for the term 'motel' and return only the HotelName field:

    Name: Secret Point Motel

    Name: Twin Dome Motel


    Apply a filter to the index to find hotels with a room cheaper than $100 per night, and return the hotelId and description:

    HotelId: 1
    Description: The hotel is ideally located on the main commercial artery of the city in the heart of New York. A few minutes away is Times Square and the historic centre of the city, as well as other places of interest that make New York one of America's most attractive and cosmopolitan cities.

    HotelId: 2
    Description: The hotel is situated in a  nineteenth century plaza, which has been expanded and renovated to the highest architectural standards to create a modern, functional and first-class hotel in which art and unique historical elements coexist with the most modern comforts.


    Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:

    Name: Triple Landscape Hotel
    Last renovated on: 9/20/2015 12:00:00 AM +00:00

    Name: Twin Dome Motel
    Last renovated on: 2/18/1979 12:00:00 AM +00:00


    Search the hotel names for the term 'hotel':

    HotelId: 3
    Name: Triple Landscape Hotel
    ...

    Complete.  Press any key to end application... 

애플리케이션의 전체 소스 코드는 이 문서의 마지막 부분에 제공됩니다.

다음으로, `Main`에 의해 호출된 각 메서드를 좀더 자세히 살펴볼 것입니다.

### <a name="creating-an-index"></a>인덱스 만들기
만든 후는 `SearchServiceClient`, `Main` 이미 있는 경우 "호텔" 인덱스를 삭제 합니다. 삭제는 같은 방법으로 수행 됩니다.

```csharp
private static void DeleteIndexIfExists(string indexName, SearchServiceClient serviceClient)
{
    if (serviceClient.Indexes.Exists(indexName))
    {
        serviceClient.Indexes.Delete(indexName);
    }
}
```

이 메서드는 주어진 `SearchServiceClient` 을(를) 사용하여 인덱스가 존재하는지 확인하고 존재하면 이를 삭제합니다.

> [!NOTE]
> 이 문서의 예제 코드는 간단히 하기 위해 Azure Search.NET SDK의 동기 메서드를 사용합니다. 확장성과 응답성이 유지하기 위해 사용 중인 애플리케이션에서 비동기 메서드를 사용하는 것이 좋습니다. 예를 들어, 위의 메서드의 경우 `Exists` 및 `Delete` 대신`ExistsAsync` 및 `DeleteAsync`을(를) 사용할 수 있습니다.
> 
> 

그 다음 `Main`이(가) 이 메서드를 호출하여 새 "호텔" 인덱스를 만듭니다.

```csharp
private static void CreateIndex(string indexName, SearchServiceClient serviceClient)
{
    var definition = new Index()
    {
        Name = indexName,
        Fields = FieldBuilder.BuildForType<Hotel>()
    };
    
    serviceClient.Indexes.Create(definition);
}
```

이 메서드는 새 인덱스의 스키마를 정의하는 `Field` 개체 목록과 함께 새 `Index` 개체를 만듭니다. 각 필드에는 이름, 데이터 유형, 그리고 검색 동작을 정의하는 몇 가지 특성이 있습니다. `FieldBuilder` 클래스는 리플렉션을 사용하여 지정된 `Hotel` 모델 클래스의 public 속성 및 특성을 검사하고 인덱스에 대한 `Field` 개체 목록을 만듭니다. `Hotel` 클래스에 대해서는 이후에 좀 더 자세히 알아보겠습니다.

> [!NOTE]
> 필요한 경우 `FieldBuilder`를 사용하는 대신, `Field` 개체 목록을 항상 직접 만들 수 있습니다. 예를 들어 모델 클래스를 사용하지 않으려고 하거나, 특성을 추가하여 수정하지 않으려는 기존 모델 클래스를 사용해야 할 수 있습니다.
>
> 

필드 외에 추가할 수 확인 기, 점수 매기기 프로필, CORS 옵션 (간단히 나타내기 위해이 샘플에서 이러한 매개 변수는 생략 됨) 인덱스. [SDK 참조](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index)뿐만 아니라 [Azure Search REST API 참조](https://docs.microsoft.com/rest/api/searchservice/)에서 인덱스 개체와 그 구성 요소에 대한 자세한 정보를 찾을 수 있습니다.

### <a name="populating-the-index"></a>인덱스 채우기
다음 단계에서 `Main` 새로 만든 인덱스를 채웁니다. 이 인덱스 채우기는 다음 메서드로 수행 됩니다. (일부 코드 "..."를 사용 하 여 대체 이해를 돕기 위해.  전체 데이터 채우기 코드에 대 한 전체 샘플 솔루션 참조.)

```csharp
private static void UploadDocuments(ISearchIndexClient indexClient)
{
    var hotels = new Hotel[]
    {
        new Hotel()
        {
            HotelId = "1",
            HotelName = "Secret Point Motel",
            ...
            Address = new Address()
            {
                StreetAddress = "677 5th Ave",
                ...
            },
            Rooms = new Room[]
            {
                new Room()
                {
                    Description = "Budget Room, 1 Queen Bed (Cityside)",
                    ...
                },
                new Room()
                {
                    Description = "Budget Room, 1 King Bed (Mountain View)",
                    ...
                },
                new Room()
                {
                    Description = "Deluxe Room, 2 Double Beds (City View)",
                    ...
                }
            }
        },
        new Hotel()
        {
            HotelId = "2",
            HotelName = "Twin Dome Motel",
            ...
            {
                StreetAddress = "140 University Town Center Dr",
                ...
            },
            Rooms = new Room[]
            {
                new Room()
                {
                    Description = "Suite, 2 Double Beds (Mountain View)",
                    ...
                },
                new Room()
                {
                    Description = "Standard Room, 1 Queen Bed (City View)",
                    ...
                },
                new Room()
                {
                    Description = "Budget Room, 1 King Bed (Waterfront View)",
                    ...
                }
            }
        },
        new Hotel()
        {
            HotelId = "3",
            HotelName = "Triple Landscape Hotel",
            ...
            Address = new Address()
            {
                StreetAddress = "3393 Peachtree Rd",
                ...
            },
            Rooms = new Room[]
            {
                new Room()
                {
                    Description = "Standard Room, 2 Queen Beds (Amenities)",
                    ...
                },
                new Room ()
                {
                    Description = "Standard Room, 2 Double Beds (Waterfront View)",
                    ...
                },
                new Room()
                {
                    Description = "Deluxe Room, 2 Double Beds (Cityside)",
                    ...
                }
            }
        }
    };

    var batch = IndexBatch.Upload(hotels);

    try
    {
        indexClient.Documents.Index(batch);
    }
    catch (IndexBatchException e)
    {
        // Sometimes when your Search service is under load, indexing will fail for some of the documents in
        // the batch. Depending on your application, you can take compensating actions like delaying and
        // retrying. For this simple demo, we just log the failed document keys and continue.
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

    Console.WriteLine("Waiting for documents to be indexed...\n");
    Thread.Sleep(2000);
}
```

이 메서드는 네 부분으로 이루어져 있습니다. 첫 번째 3의 배열을 만들어 `Hotel` 3을 사용 하 여 각 개체 `Room` 인덱스에 업로드할 데이터 입력된으로 사용 될 개체입니다. 이 데이터는 간단히 하기 위해 하드 코딩합니다. 사용 중인 애플리케이션의 경우, 데이터는 SQL 데이터베이스와 같은 외부 데이터 원본에서 나올 가능성이 높습니다.

두 번째 부분은 문서를 포함하는 `IndexBatch`를 만듭니다. 배치를 만들 때 배치에 적용할 작업을 지정합니다. 이 경우에는 `IndexBatch.Upload`를 호출하여 지정합니다. 그런 다음 `Documents.Index` 메서드를 통해 Azure Search 인덱스에 일괄적으로 업로드합니다.

> [!NOTE]
> 이 예에서는 문서를 업로드하는 것입니다. 변경 사항을 기존 문서에 병합하거나 문서를 삭제하려면 `IndexBatch.Merge`, `IndexBatch.MergeOrUpload` 또는 `IndexBatch.Delete`를 호출하여 배치를 만듭니다. `IndexBatch.New`를 호출하여 단일 배치 내에 다른 작업들을 혼합할 수 있으며, 이것은 `IndexAction` 개체 컬렉션을 사용하고, 이들 각각은 Azure Search가 문서의 특정 작업을 수행하도록 지시합니다. `IndexAction.Merge`, `IndexAction.Upload` 등을 비롯한 해당 메서드를 호출하여 각 `IndexAction`과 자체 작업을 만들 수 있습니다.
> 
> 

이 메서드의 세 번째 부분은 인덱싱에 중요한 오류 사례를 처리하는 catch 블록입니다. Azure Search 서비스가 일괄 처리에서 문서 일부를 인덱싱하는데 실패하는 경우 `Documents.Index`에 의해 `IndexBatchException`이(가) 발생합니다. 이 예외는 서비스의 부하가 하는 동안에 문서를 인덱싱하는 경우에 발생할 수 있습니다. **이 경우 코드에서 명시적으로 처리하는 것이 좋습니다.** 실패한 문서 인덱싱을 잠시 후 다시 시도하거나, 샘플에서 하던 것처럼 기록하여 계속하거나, 애플리케이션의 데이터 일관성 요구 사항에 따라 다른 작업을 수행할 수 있습니다.

> [!NOTE]
> [`FindFailedActionsToRetry`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexbatchexception.findfailedactionstoretry) 메서드를 사용하여 이전 `Index` 호출에서 실패한 작업만 포함하는 새 일괄 처리를 생성할 수 있습니다. 이 메서드를 [StackOverflow](https://stackoverflow.com/questions/40012885/azure-search-net-sdk-how-to-use-findfailedactionstoretry)에서 적절히 사용하는 방법을 확인할 수 있습니다.
>
>

마지막으로, `UploadDocuments` 메서드가 2초 동안 지연됩니다. Azure Search 서비스에서 인덱싱이 비동기적으로 발생하기 때문에, 샘플 애플리케이션은 문서 검색을 위해 잠시 기다려야 합니다. 이와 같이 데모, 테스트, 샘플 애플리케이션에서는 일반적으로 지연만 필요합니다.

<a name="how-dotnet-handles-documents"></a>

#### <a name="how-the-net-sdk-handles-documents"></a>.NET SDK가 문서를 처리하는 방법
Azure Search.NET SDK가 어떻게 `Hotel` 와(과) 같은 사용자 정의 클래스의 인스턴스를 업로드할 수 있는지 궁금할 수 있습니다. 이 질문에 대답하기 위해 `Hotel` 클래스를 살펴보겠습니다.

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;
using Newtonsoft.Json;

public partial class Hotel
{
    [System.ComponentModel.DataAnnotations.Key]
    [IsFilterable]
    public string HotelId { get; set; }

    [IsSearchable, IsSortable]
    public string HotelName { get; set; }

    [IsSearchable]
    [Analyzer(AnalyzerName.AsString.EnLucene)]
    public string Description { get; set; }

    [IsSearchable]
    [Analyzer(AnalyzerName.AsString.FrLucene)]
    [JsonProperty("Description_fr")]
    public string DescriptionFr { get; set; }

    [IsSearchable, IsFilterable, IsSortable, IsFacetable]
    public string Category { get; set; }

    [IsSearchable, IsFilterable, IsFacetable]
    public string[] Tags { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public bool? ParkingIncluded { get; set; }

    // SmokingAllowed reflects whether any room in the hotel allows smoking.
    // The JsonIgnore attribute indicates that a field should not be created 
    // in the index for this property and it will only be used by code in the client.
    [JsonIgnore]
    public bool? SmokingAllowed => (Rooms != null) ? Array.Exists(Rooms, element => element.SmokingAllowed == true) : (bool?)null;

    [IsFilterable, IsSortable, IsFacetable]
    public DateTimeOffset? LastRenovationDate { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public double? Rating { get; set; }

    public Address Address { get; set; }

    [IsFilterable, IsSortable]
    public GeographyPoint Location { get; set; }

    public Room[] Rooms { get; set; }
}
```

가장 먼저 확인 하는 각 공용 속성의 이름을 합니다 `Hotel` 클래스는 인덱스 정의에 동일한 이름 가진 필드에 매핑됩니다. 소문자 ("카멜식 대 / 소문자")를 사용 하 여 시작 하려면 각 필드를 사용할 수 있도록 하려는 경우에 SDK를 사용 하 여 자동으로 카멜식 대 / 소문자 속성 이름을 매핑할을 지정할 수 있습니다는 `[SerializePropertyNamesAsCamelCase]` 클래스의 특성입니다. 이 시나리오는 "파스칼식 대 / 소문자" 명명.NET에 대 한 지침을 위반 하지 않고도 응용 프로그램 개발자의 제어 범위를 벗어난 대상 스키마 인 데이터 바인딩을 수행 하는.NET 응용 프로그램에서 일반적입니다.

> [!NOTE]
> Azure Search .NET SDK는 [NewtonSoft JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) 라이브러리를 사용하여 사용자 지정 모델 개체를 JSON과 직렬화 및 deserialize합니다. 필요한 경우 직렬화를 사용자 지정할 수 있습니다. 자세한 내용은 [JSON.NET으로 직렬화 사용자 지정](#JsonDotNet)합니다.
> 
> 

두 번째 부분은 각 속성은 특성을 사용 하 여 같은 `IsFilterable`, `IsSearchable`를 `Key`, 및 `Analyzer`합니다. 이러한 특성에 직접 매핑하는 [Azure Search 인덱스에서 해당 필드 특성](https://docs.microsoft.com/rest/api/searchservice/create-index#request)합니다. `FieldBuilder` 클래스는 인덱스에 대 한 필드 정의 생성 하려면 이러한 속성을 사용 합니다.

에 대 한 세 번째 중요 한 사항은 `Hotel` 클래스는 public 속성의 데이터 형식입니다. 이러한 속성의 .NET 유형은 인덱스 정의의 동등한 필드 유형에 매핑합니다. 예를 들어, `Category` 문자열 속성은 `Edm.String` 유형인 `category` 필드에 매핑됩니다. 간에 유사한 유형 매핑이 `bool?`, `Edm.Boolean`를 `DateTimeOffset?`, 및 `Edm.DateTimeOffset` 등입니다. 유형 매핑에 대한 특정 규칙은 [Azure Search .NET SDK 참조](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.get)에 `Documents.Get` 메서드로 문서화됩니다. `FieldBuilder` 클래스는 사용자를 위해 이러한 매핑을 처리하는 역할을 하지만 serialization 문제를 해결해야 하는 경우에도 알아두면 도움이 될 수 있습니다.

확인할 발생할 않았습니다는 `SmokingAllowed` 속성?

```csharp
[JsonIgnore]
public bool? SmokingAllowed => (Rooms != null) ? Array.Exists(Rooms, element => element.SmokingAllowed == true) : (bool?)null;
```

합니다 `JsonIgnore` 이 속성에는 특성은 지시를 `FieldBuilder` 인덱스 필드를 serialize 하지 않습니다.  이것이 응용 프로그램에서 도우미로 사용 하는 클라이언트 쪽 계산 된 속성을 만들 수 있는 좋은 방법입니다.  이 경우는 `SmokingAllowed` 속성을 반영 있는지 여부를 `Room` 에 `Rooms` 컬렉션 번호를 허용 합니다.  모두 false 이면 전체 호텔 번호를 허용 하지 않습니다 나타냅니다.

와 같은 일부 속성 `Address` 고 `Rooms` 는.NET 클래스의 인스턴스입니다.  이러한 더 복잡 한 데이터 구조를 나타내며 속성과, 결과적으로, 사용 하 여 필드를 필요는 [복잡 한 데이터 형식](https://docs.microsoft.com/azure/search/search-howto-complex-data-types) 인덱스에서.

합니다 `Address` 속성에 여러 값의 집합을 나타냅니다는 `Address` 아래 정의 된 클래스:

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Newtonsoft.Json;

namespace AzureSearch.SDKHowTo
{
    public partial class Address
    {
        [IsSearchable]
        public string StreetAddress { get; set; }

        [IsSearchable, IsFilterable, IsSortable, IsFacetable]
        public string City { get; set; }

        [IsSearchable, IsFilterable, IsSortable, IsFacetable]
        public string StateProvince { get; set; }

        [IsSearchable, IsFilterable, IsSortable, IsFacetable]
        public string PostalCode { get; set; }

        [IsSearchable, IsFilterable, IsSortable, IsFacetable]
        public string Country { get; set; }
    }
}
```

이 클래스에는 미국 또는 캐나다에서 주소를 설명 하는 데 표준 값을 포함 합니다. 인덱스의 논리적 필드를 그룹화 하려면 다음과 같은 형식에 사용할 수 있습니다.

합니다 `Rooms` 속성의 배열을 나타냅니다 `Room` 개체:

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Newtonsoft.Json;

namespace AzureSearch.SDKHowTo
{
    public partial class Room
    {
        [IsSearchable]
        [Analyzer(AnalyzerName.AsString.EnMicrosoft)]
        public string Description { get; set; }

        [IsSearchable]
        [Analyzer(AnalyzerName.AsString.FrMicrosoft)]
        [JsonProperty("Description_fr")]
        public string DescriptionFr { get; set; }

        [IsSearchable, IsFilterable, IsFacetable]
        public string Type { get; set; }

        [IsFilterable, IsFacetable]
        public double? BaseRate { get; set; }

        [IsSearchable, IsFilterable, IsFacetable]
        public string BedOptions { get; set; }

        [IsFilterable, IsFacetable]
        public int SleepsCount { get; set; }

        [IsFilterable, IsFacetable]
        public bool? SmokingAllowed { get; set; }

        [IsSearchable, IsFilterable, IsFacetable]
        public string[] Tags { get; set; }
    }
}
```

.NET 및 해당 인덱스 스키마에서 데이터 모델은 최종 사용자에 게 제공 하려는 검색 환경을 지원 하도록 설계 되어야 합니다. .NET에서 인덱스의 문서 ie 각 최상위 개체는 사용자 인터페이스에서 제공 하는 검색 결과에 해당 합니다. 예를 들어 호텔 검색 응용 프로그램을 최종 사용자가 할 호텔의 기능 또는 특정 사무실의 특성을 호텔 이름으로 검색 합니다. 다루게 몇 가지 쿼리 예 약간 나중입니다.

양방향;에서 작동 하는 고유한 클래스를 사용 하는 인덱스의 문서와 상호 작용 하는이 기능 검색 결과 검색 하 고 섹션에서 보듯이 자동으로 해당 사용자가 선택한 형식으로 deserialize SDK가 수도 있습니다.

> [!NOTE]
> Azure Search.NET SDK는 필드 이름을 필드 값에 매핑하는 키/값인 `Document` 클래스를 사용하여 동적 유형의 문서도 지원합니다. 디자인 타임에서 인덱스 스키마를 알 수 없거나 특정 모델 클래스에 바인딩하기 불편한 시나리오에서 유용합니다. 문서를 처리하는 SDK의 모든 메서드에는 `Document` 클래스와 연동하는 오버로드와 제네릭 유형 매개 변수를 취하는 강력한 유형의 오버로드가 있습니다. 이 자습서의 샘플 코드에서는 후자만 사용됩니다. [`Document`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.document) 클래스는 `Dictionary<string, object>`에서 상속합니다.
> 
>

**Null 허용 데이터 형식을 사용해야 하는 이유**

고유의 모델 클래스를 Azure Search 인덱스에 매핑하도록 설계하는 경우 `bool` 및 `int` 등과 같은 값 유형의 속성을 Null이 허용되도록 선언하는 것이 좋습니다(예: `bool` 대신 `bool?`). Null이 허용되지 않는 속성을 사용하는 경우 인덱스의 문서가 해당 필드에 대해 Null 값을 포함하지 않도록 **보장** 해야 합니다. SDK와 Azure Search 서비스 모두 이를 적용하는 데 활용할 수 없습니다.

이것은 가상의 문제가 아닙니다. `Edm.Int32` 형식인 기존 인덱스에 새 필드를 추가하는 시나리오를 가정해 보겠습니다. 인덱스 정의를 업데이트한 후 모든 문서는 해당하는 새 필드에 대해 Null 값을 포함하게 됩니다(Azure Search에서 모든 형식은 Null을 허용하기 때문). 그런 다음 해당 필드에 대해 Null이 허용되지 않는 `int` 속성으로 모델 클래스를 사용하는 경우 문서를 검색하려고 시도할 때 다음과 같은 `JsonSerializationException`이 발생합니다.

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

이러한 이유로 모델 클래스에는 Null을 허용하는 형식을 사용하는 것이 가장 좋습니다.

<a name="JsonDotNet"></a>

#### <a name="custom-serialization-with-jsonnet"></a>JSON.NET으로 직렬화 사용자 지정
이 SDK는 문서를 직렬화 및 역직렬화하는 데 JSON.NET을 사용합니다. Serialization을 사용자 지정 하 고 필요한 경우를 정의 하 여 사용자 고유의 deserialization `JsonConverter` 또는 `IContractResolver`합니다. 자세한 내용은 참조는 [JSON.NET 설명서](https://www.newtonsoft.com/json/help/html/Introduction.htm)합니다. 이 기능은 Azure Search에 사용할 애플리케이션에서 기존 모델 클래스를 적용하려는 경우와 기타 고급 시나리오에서 유용할 수 있습니다. 예를 들어 사용자 지정 serialization으로 다음을 수행할 수 있습니다.

* 모델 클래스의 특정 속성을 문서 필드로 저장하는 데 포함 또는 제외할 수 있습니다.
* 코드의 속성 이름과 인덱스의 필드 이름을 매핑할 수 있습니다.
* 문서 필드에 속성에 매핑하는 데 사용할 수 있는 사용자 지정 특성을 만듭니다.

GitHub에서 Azure Search .NET SDK에 대한 단위 테스트에서 사용자 지정 serialization을 구현하는 예제를 찾을 수 있습니다. 적절한 시작 지점은 [이 폴더](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models)입니다. 여기에는 사용자 지정 serialization 테스트에 사용되는 클래스가 있습니다.

### <a name="searching-for-documents-in-the-index"></a>인덱스에서 문서 검색
샘플 응용 프로그램의 마지막 단계에서는 인덱스의 일부 문서에 대 한 검색 하는 것:

```csharp
private static void RunQueries(ISearchIndexClient indexClient)
{
    SearchParameters parameters;
    DocumentSearchResult<Hotel> results;

    Console.WriteLine("Search the entire index for the term 'motel' and return only the HotelName field:\n");

    parameters =
        new SearchParameters()
        {
            Select = new[] { "HotelName" }
        };

    results = indexClient.Documents.Search<Hotel>("motel", parameters);

    WriteDocuments(results);

    Console.Write("Apply a filter to the index to find hotels with a room cheaper than $100 per night, ");
    Console.WriteLine("and return the hotelId and description:\n");

    parameters =
        new SearchParameters()
        {
            Filter = "Rooms/any(r: r/BaseRate lt 100)",
            Select = new[] { "HotelId", "Description" }
        };

    results = indexClient.Documents.Search<Hotel>("*", parameters);

    WriteDocuments(results);

    Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
    Console.Write("in descending order, take the top two results, and show only hotelName and ");
    Console.WriteLine("lastRenovationDate:\n");

    parameters =
        new SearchParameters()
        {
            OrderBy = new[] { "LastRenovationDate desc" },
            Select = new[] { "HotelName", "LastRenovationDate" },
            Top = 2
        };

    results = indexClient.Documents.Search<Hotel>("*", parameters);

    WriteDocuments(results);

    Console.WriteLine("Search the entire index for the term 'hotel':\n");

    parameters = new SearchParameters();
    results = indexClient.Documents.Search<Hotel>("hotel", parameters);

    WriteDocuments(results);
}
```

쿼리를 실행할 때마다 이 메서드는 먼저 새 `SearchParameters` 개체를 만듭니다. 이 개체는 정렬, 필터링, 페이징, 패싯 등 쿼리에 대 한 추가 옵션을 지정 하려면 사용 됩니다. 이 메서드에서 다른 쿼리에 대해 `Filter`, `Select`, `OrderBy` 및 `Top` 속성을 설정합니다. 모든 `SearchParameters` 속성은 [여기](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters)에 설명되어 있습니다.

다음 단계를 실제로 검색 쿼리를 실행합니다. 검색을 실행 이루어집니다를 사용 하 여는 `Documents.Search` 메서드. 각 쿼리에 대해 문자열(또는 검색 텍스트가 없는 경우 `"*"`)로 사용할 검색 텍스트와 앞서 만든 검색 매개 변수를 전달합니다. 또한 SDK에 검색 결과 내 문서를 `Hotel` 유형의 개체로 역직렬화하도록 명령하는 `Documents.Search`에 대한 유형 매개 변수로 `Hotel`을(를) 지정할 수도 있습니다.

> [!NOTE]
> 검색 쿼리 식 구문에 대한 자세한 내용은 [여기](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search)에서 찾을 수 있습니다.
> 
> 

마지막으로, 각 쿼리 후에 이 메서드는 검색 결과에서 일치하는 모든 항목을 반복하여 각 문서를 콘솔에 인쇄합니다.

```csharp
private static void WriteDocuments(DocumentSearchResult<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.Results)
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

이러한 각 쿼리를 차례대로 조금 더 자세히 살펴보겠습니다. 첫 번째 쿼리를 실행하는 코드는 다음과 같습니다.

```csharp
parameters =
    new SearchParameters()
    {
        Select = new[] { "HotelName" }
    };

results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

이 경우 검색 가능한 모든 필드에서 단어 "motel"에 대 한 전체 인덱스 검색 하 고 및 호텔 이름에 지정 된 대로 검색 하고자 합니다 `Select` 매개 변수입니다. 결과는 다음과 같습니다.

    Name: Secret Point Motel

    Name: Twin Dome Motel

다음 쿼리는 좀 더 흥미롭습니다.  100 달러의 숙박 요율이를 사용 하 여 확보 및 호텔 ID와 설명을 반환 하는 모든 호텔 찾기 해야 합니다.

```csharp
parameters =
    new SearchParameters()
    {
        Filter = "Rooms/any(r: r/BaseRate lt 100)",
        Select = new[] { "HotelId", "Description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);
```

이 쿼리는 OData `$filter` 식 `Rooms/any(r: r/BaseRate lt 100)`을 사용하여 인덱스의 문서를 필터링합니다. 사용 하는 [모든 연산자](https://docs.microsoft.com/azure/search/search-query-odata-collection-operators) 적용할는 ' BaseRate lt 100' 방 컬렉션의 모든 항목을 합니다. Azure Search가 지원하는 OData 구문에 대한 자세한 내용은 [여기](https://docs.microsoft.com/azure/search/query-odata-filter-orderby-syntax)에서 확인할 수 있습니다.

다음은 쿼리 결과입니다.

    HotelId: 1
    Description: The hotel is ideally located on the main commercial artery of the city in the heart of New York...

    HotelId: 2
    Description: The hotel is situated in a nineteenth century plaza, which has been expanded and renovated to...

다음에는 가장 최근에 리모델링한 상위 두 개 호텔을 찾은 후 호텔 이름 및 마지막 보수 날짜를 표시하려고 합니다. 코드는 다음과 같습니다. 

```csharp
parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "LastRenovationDate desc" },
        Select = new[] { "HotelName", "LastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);
```

이 경우 OData 구문을 다시 사용하여 `OrderBy` 매개 변수를 `lastRenovationDate desc`로 지정합니다. 또한 `Top`을 2로 설정하여 상위 2개의 문서만 표시합니다. 앞에서 나온 것처럼 `Select`를 설정하여 반환할 필드를 지정합니다.

결과는 다음과 같습니다.

    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

마지막으로 "호텔" 이라는 단어와 일치 하는 모든 호텔 이름 찾기 해야 합니다.

```csharp
parameters = new SearchParameters()
{
    SearchFields = new[] { "HotelName" }
};
results = indexClient.Documents.Search<Hotel>("hotel", parameters);

WriteDocuments(results);
```

결과는 다음과 같습니다. 여기서는 `Select` 속성을 지정하지 않았으므로 모든 필드가 포함됩니다.

    HotelId: 3
    Name: Triple Landscape Hotel
    ...

이 단계에서 자습서를 완료하지만 여기서 멈추지 마십시오. \* * 다음 단계는 Azure Search에 대 한 자세한 학습을 위한 추가 리소스를 제공 합니다.

## <a name="next-steps"></a>다음 단계
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) 및 [REST API](https://docs.microsoft.com/rest/api/searchservice/)에 대한 참고 자료를 찾아봅니다.
* [명명 규칙](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) 을 검토하여 다양한 개체 명명에 대한 규칙에 알아보십시오.
* Azure Search에서 [지원되는 데이터 유형](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) 을 검토하십시오.
