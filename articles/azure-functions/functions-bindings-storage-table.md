---
title: Azure Functions의 Azure Table Storage 바인딩
description: Azure Functions에서 Azure Table Storage 바인딩을 사용하는 방법을 이해합니다.
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
keywords: Azure 함수, 함수, 이벤트 처리, 동적 계산, 서버리스 아키텍처
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 09/03/2018
ms.author: cshoe
ms.openlocfilehash: 5c3049b5f9f7607b9b75f3bee48b6ccd44601b15
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68254751"
---
# <a name="azure-table-storage-bindings-for-azure-functions"></a>Azure Functions의 Azure Table Storage 바인딩

이 문서에서는 Azure Functions에서 Azure Table Storage 바인딩을 사용하는 방법을 설명합니다. Azure Functions는 Azure Table Storage에 대한 입력 및 출력 바인딩을 지원합니다.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>패키지 - Functions 1.x

Table Storage 바인딩은 [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet 패키지 버전 2.x에서 제공됩니다. 이 패키지에 대한 소스 코드는 [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.Storage/Table) GitHub 리포지토리에 있습니다.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="packages---functions-2x"></a>패키지 - Functions 2.x

Table Storage 바인딩은 [Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage) NuGet 패키지 버전 3.x에서 제공됩니다. 이 패키지에 대한 소스 코드는 [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables) GitHub 리포지토리에 있습니다.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="input"></a>입력

Azure Table Storage 입력 바인딩을 사용하여 Azure Storage 계정에서 테이블을 읽을 수 있습니다.

## <a name="input---example"></a>입력 - 예제

언어 관련 예제를 참조하세요.

* [C# 단일 엔터티 읽기](#input---c-example---one-entity)
* [C# IQueryable에 바인딩](#input---c-example---iqueryable)
* [C# CloudTable에 바인딩](#input---c-example---cloudtable)
* [C# 스크립트 단일 엔터티 읽기](#input---c-script-example---one-entity)
* [C# 스크립트 IQueryable에 바인딩](#input---c-script-example---iqueryable)
* [C# 스크립트 CloudTable에 바인딩](#input---c-script-example---cloudtable)
* [F#](#input---f-example)
* [JavaScript](#input---javascript-example)
* [Java](#input---java-example)

### <a name="input---c-example---one-entity"></a>입력 - C# 예제 - 단일 엔터티

다음 예제에서는 단일 테이블 행을 읽을 수 있는 [C# 함수](functions-dotnet-class-library.md)를 보여줍니다. 

행 키 값 "{queueTrigger}"는 큐 메시지 문자열에서 나온 행 키를 의미합니다.

```csharp
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableInput")]
    public static void TableInput(
        [QueueTrigger("table-items")] string input, 
        [Table("MyTable", "MyPartition", "{queueTrigger}")] MyPoco poco, 
        ILogger log)
    {
        log.LogInformation($"PK={poco.PartitionKey}, RK={poco.RowKey}, Text={poco.Text}");
    }
}
```

### <a name="input---c-example---iqueryable"></a>입력 - C# 예제 - IQueryable

다음 예제에서는 여러 테이블 행을 읽을 수 있는 [C# 함수](functions-dotnet-class-library.md)를 보여줍니다. `MyPoco` 클래스가 `TableEntity`에서 파생되는지 확인합니다.

```csharp
public class TableStorage
{
    public class MyPoco : TableEntity
    {
        public string Text { get; set; }
    }

    [FunctionName("TableInput")]
    public static void TableInput(
        [QueueTrigger("table-items")] string input, 
        [Table("MyTable", "MyPartition")] IQueryable<MyPoco> pocos, 
        ILogger log)
    {
        foreach (MyPoco poco in pocos)
        {
            log.LogInformation($"PK={poco.PartitionKey}, RK={poco.RowKey}, Text={poco.Text}");
        }
    }
}
```

### <a name="input---c-example---cloudtable"></a>입력 - C# 예제 - CloudTable

`IQueryable`은 [Functions v2 런타임](functions-versions.md)에서 지원되지 않습니다. 대신 Azure Storage SDK를 사용하여 테이블을 읽는 `CloudTable` 메서드 매개 변수를 사용합니다. 다음은 Azure Functions 로그 테이블을 쿼리하는 2.x 함수 예제입니다.

```csharp
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;
using Microsoft.WindowsAzure.Storage.Table;
using System;
using System.Threading.Tasks;

namespace FunctionAppCloudTable2
{
    public class LogEntity : TableEntity
    {
        public string OriginalName { get; set; }
    }
    public static class CloudTableDemo
    {
        [FunctionName("CloudTableDemo")]
        public static async Task Run(
            [TimerTrigger("0 */1 * * * *")] TimerInfo myTimer, 
            [Table("AzureWebJobsHostLogscommon")] CloudTable cloudTable,
            ILogger log)
        {
            log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");

            TableQuery<LogEntity> rangeQuery = new TableQuery<LogEntity>().Where(
                TableQuery.CombineFilters(
                    TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, 
                        "FD2"),
                    TableOperators.And,
                    TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.GreaterThan, 
                        "t")));

            // Execute the query and loop through the results
            foreach (LogEntity entity in 
                await cloudTable.ExecuteQuerySegmentedAsync(rangeQuery, null))
            {
                log.LogInformation(
                    $"{entity.PartitionKey}\t{entity.RowKey}\t{entity.Timestamp}\t{entity.OriginalName}");
            }
        }
    }
}
```

CloudTable을 사용하는 방법에 대한 자세한 내용은 [Azure Table Storage 시작](../cosmos-db/table-storage-how-to-use-dotnet.md)을 참조하세요.

`CloudTable`에 바인딩하려고 하면 오류 메시지가 표시되는 경우 [올바른 Storage SDK 버전](#azure-storage-sdk-version-in-functions-1x)에 대한 참조가 있는지 확인합니다.

### <a name="input---c-script-example---one-entity"></a>입력 - C# 스크립트 예제 - 단일 엔터티

다음 예에서는 바인딩을 사용하는 *function.json* 파일 및 [C# 스크립트](functions-reference-csharp.md) 코드에서 테이블 입력 바인딩을 보여줍니다. 함수는 큐 트리거를 사용하여 단일 테이블 행을 읽습니다. 

*function.json* 파일은 `partitionKey` 및 `rowKey`를 지정합니다. `rowKey` 값 "{queueTrigger}"는 큐 메시지 문자열에서 나온 행 키를 의미합니다.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

[구성](#input---configuration) 섹션에서는 이러한 속성을 설명합니다.

C# 스크립트 코드는 다음과 같습니다.

```csharp
public static void Run(string myQueueItem, Person personEntity, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    log.LogInformation($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

### <a name="input---c-script-example---iqueryable"></a>입력 - C# 스크립트 예제 - IQueryable

다음 예에서는 바인딩을 사용하는 *function.json* 파일 및 [C# 스크립트](functions-reference-csharp.md) 코드에서 테이블 입력 바인딩을 보여줍니다. 함수는 큐 메시지에 지정된 파티션 키의 엔터티를 읽습니다.

*function.json* 파일은 다음과 같습니다.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnectionAppSetting",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

[구성](#input---configuration) 섹션에서는 이러한 속성을 설명합니다.

C# 스크립트 코드는 엔터티 형식이 `TableEntity`에서 파생할 수 있도록 Azure Storage SDK에 대한 참조를 추가합니다.

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;
using Microsoft.Extensions.Logging;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.LogInformation($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
```

### <a name="input---c-script-example---cloudtable"></a>입력 - C# 스크립트 예제 - CloudTable

`IQueryable`은 [Functions v2 런타임](functions-versions.md)에서 지원되지 않습니다. 대신 Azure Storage SDK를 사용하여 테이블을 읽는 `CloudTable` 메서드 매개 변수를 사용합니다. 다음은 Azure Functions 로그 테이블을 쿼리하는 2.x 함수 예제입니다.

```json
{
  "bindings": [
    {
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in",
      "schedule": "0 */1 * * * *"
    },
    {
      "name": "cloudTable",
      "type": "table",
      "connection": "AzureWebJobsStorage",
      "tableName": "AzureWebJobsHostLogscommon",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;
using System;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;

public static async Task Run(TimerInfo myTimer, CloudTable cloudTable, ILogger log)
{
    log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");

    TableQuery<LogEntity> rangeQuery = new TableQuery<LogEntity>().Where(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, 
            "FD2"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.GreaterThan, 
            "a")));

    // Execute the query and loop through the results
    foreach (LogEntity entity in 
    await cloudTable.ExecuteQuerySegmentedAsync(rangeQuery, null))
    {
        log.LogInformation(
            $"{entity.PartitionKey}\t{entity.RowKey}\t{entity.Timestamp}\t{entity.OriginalName}");
    }
}

public class LogEntity : TableEntity
{
    public string OriginalName { get; set; }
}
```

CloudTable을 사용하는 방법에 대한 자세한 내용은 [Azure Table Storage 시작](../cosmos-db/table-storage-how-to-use-dotnet.md)을 참조하세요.

`CloudTable`에 바인딩하려고 하면 오류 메시지가 표시되는 경우 [올바른 Storage SDK 버전](#azure-storage-sdk-version-in-functions-1x)에 대한 참조가 있는지 확인합니다.

### <a name="input---f-example"></a>입력 - F# 예제

다음 예에서는 바인딩을 사용하는 *function.json* 파일 및 [F# 스크립트](functions-reference-fsharp.md) 코드에서 테이블 입력 바인딩을 보여줍니다. 함수는 큐 트리거를 사용하여 단일 테이블 행을 읽습니다. 

*function.json* 파일은 `partitionKey` 및 `rowKey`를 지정합니다. `rowKey` 값 "{queueTrigger}"는 큐 메시지 문자열에서 나온 행 키를 의미합니다.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

[구성](#input---configuration) 섹션에서는 이러한 속성을 설명합니다.

F# 코드는 다음과 같습니다.

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.LogInformation(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.LogInformation(sprintf "Name in Person entity: %s" personEntity.Name)
```

### <a name="input---javascript-example"></a>입력 - JavaScript 예제

다음 예에서는 바인딩을 사용하는 *function.json* 파일 및 [JavaScript 코드](functions-reference-node.md)에서 테이블 입력 바인딩을 보여줍니다. 함수는 큐 트리거를 사용하여 단일 테이블 행을 읽습니다. 

*function.json* 파일은 `partitionKey` 및 `rowKey`를 지정합니다. `rowKey` 값 "{queueTrigger}"는 큐 메시지 문자열에서 나온 행 키를 의미합니다.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

[구성](#input---configuration) 섹션에서는 이러한 속성을 설명합니다.

JavaScript 코드는 다음과 같습니다.

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

### <a name="input---java-example"></a>입력 - Java 예제

다음 예는 Table Storage의 지정된 파티션에 있는 총 항목 수를 반환하는 HTTP 트리거 함수를 보여줍니다.

```java
@FunctionName("getallcount")
public int run(
   @HttpTrigger(name = "req",
                 methods = {HttpMethod.GET},
                 authLevel = AuthorizationLevel.ANONYMOUS) Object dummyShouldNotBeUsed,
   @TableInput(name = "items",
                tableName = "mytablename",  partitionKey = "myparkey",
                connection = "myconnvarname") MyItem[] items
) {
    return items.length;
}
```


## <a name="input---attributes"></a>입력 - 특성
 
[C# 클래스 라이브러리](functions-dotnet-class-library.md)에서는 다음 특성을 사용하여 테이블 입력 바인딩을 구성합니다.

* [TableAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables/TableAttribute.cs)

  특성의 생성자는 테이블 이름, 파티션 키 및 행 키를 사용합니다. 다음 예제와 같이 out 매개 변수 또는 함수의 반환 값에서 사용할 수 있습니다.

  ```csharp
  [FunctionName("TableInput")]
  public static void Run(
      [QueueTrigger("table-items")] string input, 
      [Table("MyTable", "Http", "{queueTrigger}")] MyPoco poco, 
      ILogger log)
  {
      ...
  }
  ```

  다음 예와 같이 사용할 저장소 계정을 지정하도록 `Connection` 속성을 설정할 수 있습니다.

  ```csharp
  [FunctionName("TableInput")]
  public static void Run(
      [QueueTrigger("table-items")] string input, 
      [Table("MyTable", "Http", "{queueTrigger}", Connection = "StorageConnectionAppSetting")] MyPoco poco, 
      ILogger log)
  {
      ...
  }
  ```

  전체 예제는 입력 - C# 예제를 참조하세요.

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  사용할 저장소 계정을 지정하는 다른 방법을 제공합니다. 생성자는 저장소 연결 문자열을 포함하는 앱 설정의 이름을 사용합니다. 매개 변수, 메서드 또는 클래스 수준에서 특성을 적용할 수 있습니다. 다음 예제에서는 클래스 수준 및 메서드 수준을 보여줍니다.

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("TableInput")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ...
  }
  ```

사용할 저장소 계정은 다음과 같은 순서로 결정됩니다.

* `Table` 특성의 `Connection` 속성
* `Table` 특성과 동일한 매개 변수에 적용된 `StorageAccount` 특성
* 함수에 적용된 `StorageAccount` 특성
* 클래스에 적용된 `StorageAccount` 특성
* 함수 앱의 기본 스토리지 계정("AzureWebJobsStorage" 앱 설정)

## <a name="input---java-annotations"></a>입력 - Java 주석

[Java 함수 런타임 라이브러리](/java/api/overview/azure/functions/runtime)에서 값이 Table Storage에서 제공되는 매개 변수에 대한 `@TableInput` 주석을 사용합니다.  이 주석은 기본 Java 형식, pojos 또는 선택적\<T >를 사용 하는 nullable 값에 사용할 수 있습니다. 

## <a name="input---configuration"></a>입력 - 구성

다음 표에서는 *function.json* 파일 및 `Table` 특성에 설정된 바인딩 구성 속성을 설명합니다.

|function.json 속성 | 특성 속성 |Description|
|---------|---------|----------------------|
|**type** | n/a | `table`로 설정해야 합니다. 이 속성은 사용자가 Azure Portal에서 바인딩을 만들 때 자동으로 설정됩니다.|
|**direction** | n/a | `in`로 설정해야 합니다. 이 속성은 사용자가 Azure Portal에서 바인딩을 만들 때 자동으로 설정됩니다. |
|**name** | n/a | 함수 코드에서 테이블 또는 엔터티를 나타내는 변수의 이름입니다. | 
|**tableName** | **TableName** | 테이블의 이름입니다.| 
|**partitionKey** | **PartitionKey** |선택 사항입니다. 읽을 테이블 엔터티의 파티션 키입니다. 이 속성을 사용하는 방법에 대한 지침은 [사용](#input---usage) 섹션을 참조하세요.| 
|**rowKey** |**RowKey** | 선택 사항입니다. 읽을 테이블 엔터티의 행 키입니다. 이 속성을 사용하는 방법에 대한 지침은 [사용](#input---usage) 섹션을 참조하세요.| 
|**take** |**Take** | 선택 사항입니다. JavaScript에서 읽을 수 있는 엔터티의 최대 수입니다. 이 속성을 사용하는 방법에 대한 지침은 [사용](#input---usage) 섹션을 참조하세요.| 
|**filter** |**Assert** | 선택 사항입니다. JavaScript에서 테이블 입력에 대한 OData 필터 식입니다. 이 속성을 사용하는 방법에 대한 지침은 [사용](#input---usage) 섹션을 참조하세요.| 
|**연결** |**연결** | 이 바인딩에 사용할 저장소 연결 문자열을 포함하는 앱 설정의 이름입니다. 앱 설정 이름이 "AzureWebJobs"로 시작하는 경우 여기에서 이름의 나머지만을 지정할 수 있습니다. 예를 들어 `connection`을 "MyStorage"로 설정한 경우 함수 런타임 기능은 "AzureWebJobsMyStorage"라는 앱 설정을 찾습니다. `connection`을 비워 두면 함수 런타임 기능은 `AzureWebJobsStorage`라는 앱 설정에서 기본 저장소 연결 문자열을 사용합니다.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>입력 - 사용

Table Storage 입력 바인딩은 다음과 같은 시나리오를 지원합니다.

* **C# 또는 C# 스크립트에서 하나의 행 읽기**

  `partitionKey` 및 `rowKey`를 설정합니다. `T <paramName>` 메서드 매개 변수를 사용하여 테이블 데이터에 액세스합니다. C# 스크립트에서 `paramName`은 *function.json*의 `name` 속성에 지정된 값입니다. `T`는 일반적으로 `ITableEntity`를 구현하거나 `TableEntity`에서 파생되는 형식입니다. `filter` 및 `take` 속성은 이 시나리오에서 사용되지 않습니다. 

* **C# 또는 C# 스크립트에서 하나 이상의 행 읽기**

  `IQueryable<T> <paramName>` 메서드 매개 변수를 사용하여 테이블 데이터에 액세스합니다. C# 스크립트에서 `paramName`은 *function.json*의 `name` 속성에 지정된 값입니다. `T`는 `ITableEntity`를 구현하거나 `TableEntity`에서 파생되는 형식입니다. `IQueryable` 메서드를 사용하여 필요한 필터링을 수행할 수 있습니다 `partitionKey`, `rowKey`, `filter` 및 `take` 속성은 이 시나리오에서 사용되지 않습니다.  

  > [!NOTE]
  > `IQueryable`은 [Functions v2 런타임](functions-versions.md)에서 지원되지 않습니다. 대신 Azure Storage SDK를 사용하여 테이블을 읽는 [CloudTable paramName 메서드 매개 변수를 사용](https://stackoverflow.com/questions/48922485/binding-to-table-storage-in-v2-azure-functions-using-cloudtable)합니다. `CloudTable`에 바인딩하려고 하면 오류 메시지가 표시되는 경우 [올바른 Storage SDK 버전](#azure-storage-sdk-version-in-functions-1x)에 대한 참조가 있는지 확인합니다.

* **JavaScript에서 하나 이상의 행 읽기**

  `filter` 및 `take` 속성을 설정합니다. `partitionKey` 또는 `rowKey`를 설정하지 않습니다. `context.bindings.<name>`을 사용하여 입력 테이블 엔터티(또는 여러 엔터티)에 액세스합니다. Deserialize된 개체는 `RowKey` 및 `PartitionKey` 속성을 가집니다.

## <a name="output"></a>출력

Azure Table Storage 출력 바인딩을 사용하여 Azure Storage 계정에서 테이블에 엔터티를 쓸 수 있습니다.

> [!NOTE]
> 이 출력 바인딩은 기존 엔터티 업데이트를 지원하지 않습니다. [Azure Storage SDK](https://docs.microsoft.com/azure/cosmos-db/tutorial-develop-table-dotnet#delete-an-entity)에서 `TableOperation.Replace` 작업을 사용하여 기존 엔터티를 업데이트합니다.   

## <a name="output---example"></a>출력 - 예제

언어 관련 예제를 참조하세요.

* [C#](#output---c-example)
* [C# 스크립트(.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>출력 - C# 예제

다음 예제에서는 단일 테이블 행을 쓰기 위해 HTTP 트리거를 사용하는 [C# 함수](functions-dotnet-class-library.md)를 보여줍니다. 

```csharp
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableOutput")]
    [return: Table("MyTable")]
    public static MyPoco TableOutput([HttpTrigger] dynamic input, ILogger log)
    {
        log.LogInformation($"C# http trigger function processed: {input.Text}");
        return new MyPoco { PartitionKey = "Http", RowKey = Guid.NewGuid().ToString(), Text = input.Text };
    }
}
```

### <a name="output---c-script-example"></a>출력 - C# 스크립트 예제

다음 예에서는 바인딩을 사용하는 *function.json* 파일 및 [C# 스크립트](functions-reference-csharp.md) 코드에서 테이블 출력 바인딩을 보여줍니다. 함수는 여러 테이블 엔터티를 씁니다.

*function.json* 파일은 다음과 같습니다.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnectionAppSetting",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

[구성](#output---configuration) 섹션에서는 이러한 속성을 설명합니다.

C# 스크립트 코드는 다음과 같습니다.

```csharp
public static void Run(string input, ICollector<Person> tableBinding, ILogger log)
{
    for (int i = 1; i < 10; i++)
        {
            log.LogInformation($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```

### <a name="output---f-example"></a>출력 - F# 예제

다음 예에서는 바인딩을 사용하는 *function.json* 파일 및 [F# 스크립트](functions-reference-fsharp.md) 코드에서 테이블 출력 바인딩을 보여줍니다. 함수는 여러 테이블 엔터티를 씁니다.

*function.json* 파일은 다음과 같습니다.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnectionAppSetting",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

[구성](#output---configuration) 섹션에서는 이러한 속성을 설명합니다.

F# 코드는 다음과 같습니다.

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: ILogger) =
    for i = 1 to 10 do
        log.LogInformation(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

### <a name="output---javascript-example"></a>출력 - JavaScript 예제

다음 예에서는 바인딩을 사용하는 *function.json* 파일 및 [JavaScript 함수](functions-reference-node.md)에서 테이블 출력 바인딩을 보여줍니다. 함수는 여러 테이블 엔터티를 씁니다.

*function.json* 파일은 다음과 같습니다.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnectionAppSetting",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

[구성](#output---configuration) 섹션에서는 이러한 속성을 설명합니다.

JavaScript 코드는 다음과 같습니다.

```javascript
module.exports = function (context) {

    context.bindings.tableBinding = [];

    for (var i = 1; i < 10; i++) {
        context.bindings.tableBinding.push({
            PartitionKey: "Test",
            RowKey: i.toString(),
            Name: "Name " + i
        });
    }
    
    context.done();
};
```

## <a name="output---attributes"></a>출력 - 특성

[C# 클래스 라이브러리](functions-dotnet-class-library.md)에서 [TableAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables/TableAttribute.cs)를 사용합니다.

특성의 생성자는 테이블 이름을 사용합니다. 다음 예제와 같이 `out` 매개 변수 또는 함수의 반환 값에서 사용할 수 있습니다.

```csharp
[FunctionName("TableOutput")]
[return: Table("MyTable")]
public static MyPoco TableOutput(
    [HttpTrigger] dynamic input, 
    ILogger log)
{
    ...
}
```

다음 예와 같이 사용할 저장소 계정을 지정하도록 `Connection` 속성을 설정할 수 있습니다.

```csharp
[FunctionName("TableOutput")]
[return: Table("MyTable", Connection = "StorageConnectionAppSetting")]
public static MyPoco TableOutput(
    [HttpTrigger] dynamic input, 
    ILogger log)
{
    ...
}
```

전체 예제는 [출력 - C# 예제](#output---c-example)를 참조하세요.

`StorageAccount` 특성을 사용하여 클래스, 메서드 또는 매개 변수 수준에서 저장소 계정을 지정합니다. 자세한 내용은 [입력 - 특성](#input---attributes)을 참조하세요.

## <a name="output---configuration"></a>출력 - 구성

다음 표에서는 *function.json* 파일 및 `Table` 특성에 설정된 바인딩 구성 속성을 설명합니다.

|function.json 속성 | 특성 속성 |설명|
|---------|---------|----------------------|
|**type** | n/a | `table`로 설정해야 합니다. 이 속성은 사용자가 Azure Portal에서 바인딩을 만들 때 자동으로 설정됩니다.|
|**direction** | n/a | `out`로 설정해야 합니다. 이 속성은 사용자가 Azure Portal에서 바인딩을 만들 때 자동으로 설정됩니다. |
|**name** | n/a | 테이블 또는 엔터티를 나타내는 함수 코드에서 사용되는 변수 이름입니다. `$return`으로 설정하여 함수 반환 값을 참조합니다.| 
|**tableName** |**TableName** | 테이블의 이름입니다.| 
|**partitionKey** |**PartitionKey** | 쓸 테이블 엔터티의 파티션 키입니다. 이 속성을 사용하는 방법에 대한 지침은 [사용 섹션](#output---usage)을 참조하세요.| 
|**rowKey** |**RowKey** | 쓸 테이블 엔터티의 행 키입니다. 이 속성을 사용하는 방법에 대한 지침은 [사용 섹션](#output---usage)을 참조하세요.| 
|**연결** |**연결** | 이 바인딩에 사용할 저장소 연결 문자열을 포함하는 앱 설정의 이름입니다. 앱 설정 이름이 "AzureWebJobs"로 시작하는 경우 여기에서 이름의 나머지만을 지정할 수 있습니다. 예를 들어 `connection`을 "MyStorage"로 설정한 경우 함수 런타임 기능은 "AzureWebJobsMyStorage"라는 앱 설정을 찾습니다. `connection`을 비워 두면 함수 런타임 기능은 `AzureWebJobsStorage`라는 앱 설정에서 기본 저장소 연결 문자열을 사용합니다.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>출력 - 사용

Table Storage 출력 바인딩은 다음과 같은 시나리오를 지원합니다.

* **모든 언어에서 하나의 행 쓰기**

  C# 및 C# 스크립트에서 `out T paramName` 또는 함수 반환 값과 같은 메서드 매개 변수를 사용하여 출력 테이블 엔터티에 액세스합니다. C# 스크립트에서 `paramName`은 *function.json*의 `name` 속성에 지정된 값입니다. *function.json* 파일 또는 `Table` 특성에서 파티션 키 및 행 키를 제공하는 경우 `T`는 직렬화 가능 형식일 수 있습니다. 그렇지 않으면 `T`는 `PartitionKey` 및 `RowKey` 속성을 포함하는 형식이어야입니다. 이 시나리오에서 `T`는 `ITableEntity`를 구현하거나 `TableEntity`에서 파생되지만 반드시 그런 것은 아닙니다.

* **C# 또는 C# 스크립트에서 하나 이상의 행 쓰기**

  C# 또는 C# 스크립트에서 `ICollector<T> paramName` 또는 `IAsyncCollector<T> paramName` 메서드 매개 변수를 사용하여 출력 테이블 엔터티에 액세스합니다. C# 스크립트에서 `paramName`은 *function.json*의 `name` 속성에 지정된 값입니다. `T`는 추가하려는 엔터티의 스키마를 지정합니다. 일반적으로 `T`는 `TableEntity`에서 파생되거나 `ITableEntity`을 구현하지만 반드시 그런 것은 아닙니다. *function.json* 또는 `Table` 특성 생성자의 파티션 키와 행 키의 값은 이 시나리오에서 사용되지 않습니다.

  대신 Azure Storage SDK를 사용하여 테이블에 쓰는 `CloudTable` 메서드 매개 변수를 사용합니다. `CloudTable`에 바인딩하려고 하면 오류 메시지가 표시되는 경우 [올바른 Storage SDK 버전](#azure-storage-sdk-version-in-functions-1x)에 대한 참조가 있는지 확인합니다. `CloudTable`에 바인딩하는 코드 예제는 이 문서의 앞부분에 있는 [C#](#input---c-example---cloudtable) 또는 [C# 스크립트](#input---c-script-example---cloudtable)에 대한 입력 바인딩 예제를 참조하세요.

* **JavaScript에서 하나 이상의 행 쓰기**

  JavaScript 함수에서 `context.bindings.<name>`을 사용하여 테이블 출력에 액세스합니다.

## <a name="exceptions-and-return-codes"></a>예외 및 반환 코드

| 바인딩 | 참조 |
|---|---|
| Table | [테이블 오류 코드](https://docs.microsoft.com/rest/api/storageservices/fileservices/table-service-error-codes) |
| Blob, 테이블, 큐 | [저장소 오류 코드](https://docs.microsoft.com/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| Blob, 테이블, 큐 | [문제 해결](https://docs.microsoft.com/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [Azure Functions 트리거 및 바인딩에 대한 자세한 정보](functions-triggers-bindings.md)
