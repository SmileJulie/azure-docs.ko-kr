---
title: Azure Cosmos Emulator로 로컬에서 개발
description: Azure Cosmos Emulator를 사용하면 Azure 구독을 구입하지 않고도 로컬에서 무료로 애플리케이션을 개발하고 테스트할 수 있습니다.
ms.service: cosmos-db
ms.topic: tutorial
author: markjbrown
ms.author: mjbrown
ms.date: 07/09/2019
ms.openlocfilehash: 9649c53f9fc11795449afd78b12fda691239bb18
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797320"
---
# <a name="use-the-azure-cosmos-emulator-for-local-development-and-testing"></a>로컬 개발 및 테스트에 Azure Cosmos Emulator 사용

Azure Cosmos Emulator는 개발 목적으로 Azure Cosmos DB 서비스를 에뮬레이트하는 로컬 환경을 제공합니다. Azure Cosmos Emulator를 사용하면 Azure 구독을 구입하거나 비용을 들이지 않고 로컬에서 애플리케이션을 개발하고 테스트할 수 있습니다. Azure Cosmos Emulator에서 애플리케이션이 작동하는 방식이 만족스러우면, 클라우드에서 Azure Cosmos 계정을 사용하도록 전환할 수 있습니다.

[SQL](local-emulator.md#sql-api), [Cassandra](local-emulator.md#cassandra-api), [MongoDB](local-emulator.md#azure-cosmos-dbs-api-for-mongodb), [Gremlin](local-emulator.md#gremlin-api) 및 [Table](local-emulator.md#table-api) API 계정으로 Azure Cosmos Emulator를 사용하여 개발할 수 있습니다. 단, 현재 에뮬레이터의 Data Explorer 보기는 SQL API에 대해서만 클라이언트를 완벽하게 지원합니다. 

## <a name="how-the-emulator-works"></a>에뮬레이터의 작동 원리

Azure Cosmos Emulator는 Azure Cosmos DB 서비스에 대해 충실도가 높은 에뮬레이션을 제공합니다. Azure Cosmos DB와 동일한 기능을 지원합니다. 여기에는 데이터 만들기 및 쿼리, 컨테이너 프로비저닝 및 크기 조정, 저장 프로시저 및 트리거 실행이 포함됩니다. Azure Cosmos Emulator를 사용하여 애플리케이션을 개발 및 테스트하고 Azure Cosmos DB에 대한 연결 엔드포인트에 대한 구성을 하나만 변경하여 글로벌 규모로 Azure에 배포할 수 있습니다.

Azure Cosmos DB 서비스의 에뮬레이션이 충실한 경우 에뮬레이터의 구현은 서비스와 다릅니다. 예를 들어 에뮬레이터는 로컬 파일 시스템(지속성) 및 HTTPS 프로토콜 스택(연결성)과 같은 표준 OS 구성 요소를 사용합니다. 따라서 글로벌 복제, 한 자리 밀리초 읽기/쓰기 대기 시간, 튜닝 가능한 일관성 수준 등 Azure 인프라를 기반으로 하는 기능은 적용할 수 없습니다.

[Azure Cosmos DB 데이터 마이그레이션 도구](https://github.com/azure/azure-documentdb-datamigrationtool)를 사용하여 Azure Cosmos Emulator와 Azure Cosmos DB 서비스 간에 데이터를 마이그레이션할 수 있습니다.

Windows Docker 컨테이너에서 Azure Cosmos Emulator를 실행할 수 있습니다. docker pull 명령은 [Docker 허브](https://hub.docker.com/r/microsoft/azure-cosmosdb-emulator/)를 참조하고 에뮬레이터 소스 코드는 [GitHub](https://github.com/Azure/azure-cosmos-db-emulator-docker)를 참조하세요.

## <a name="differences-between-the-emulator-and-the-service"></a>에뮬레이터와 서비스 간 차이

Azure Cosmos Emulator는 로컬 개발자 워크스테이션에서 실행되는 에뮬레이트된 환경을 제공하기 때문에 에뮬레이터와 클라우드의 Azure Cosmos 계정 사이에는 기능 면에서 몇 가지 차이가 있습니다.

* 현재 에뮬레이터의 Data Explorer는 SQL API에 대한 클라이언트를 지원합니다. Data Explorer 보기 및 Azure Cosmos DB API에 대한 작업(예: MongoDB, Table, Graph 및 Cassandra API)은 완전히 지원되지 않습니다.
* Azure Cosmos Emulator는 고정된 단일 계정 및 알려진 마스터 키만 지원합니다. Azure Cosmos Emulator에서는 키 재생성이 불가능하지만 명령줄 옵션을 사용하여 기본 키를 변경할 수 있습니다.
* Azure Cosmos Emulator는 확장성 있는 서비스가 아니며 많은 수의 컨테이너를 지원하지 않습니다.
* Azure Cosmos Emulator는 다양한 [Azure Cosmos DB 일관성 수준](consistency-levels.md)을 제공하지 않습니다.
* Azure Cosmos Emulator는 [다중 지역 복제](distribute-data-globally.md)를 제공하지 않습니다.
* Azure Cosmos Emulator의 복사본은 항상 Azure Cosmos DB 서비스의 최신 변경 사항이 적용된 최신 상태로 유지되지 않을 수도 있으므로 [Azure Cosmos DB Capacity Planner](https://www.documentdb.com/capacityplanner)를 사용하여 애플리케이션에 요구되는 프로덕션 처리량(RU)을 정확하게 예측해야 합니다.
* Azure Cosmos Emulator를 사용하는 경우, 기본적으로, 고정 크기 컨테이너를 25개까지 만들거나(Azure Cosmos DB SDK를 통해서만 지원됨) Azure Cosmos Emulator를 사용하여 무제한 컨테이너 5개를 만들 수 있습니다. 이 값을 변경하는 방법에 대한 자세한 내용은 [PartitionCount 값 설정](#set-partitioncount)을 참조하세요.

## <a name="system-requirements"></a>시스템 요구 사항

Azure Cosmos Emulator에는 다음과 같은 하드웨어 및 소프트웨어 요구 사항이 있습니다.

* 소프트웨어 요구 사항
  * Windows Server 2012 R2, Windows Server 2016 또는 Windows 10
  * 64비트 운영 체제
* 최소 하드웨어 요구 사항
  * 2GB RAM
  * 10GB의 하드 디스크 여유 공간

## <a name="installation"></a>설치

[Microsoft 다운로드 센터](https://aka.ms/cosmosdb-emulator)에서 Azure Cosmos Emulator를 다운로드하여 설치하거나 Windows용 Docker에서 에뮬레이터를 실행할 수 있습니다. Windows용 Docker의 에뮬레이터 사용에 대한 지침을 보려면 [Docker에서 실행](#running-on-docker)을 참조하세요.

> [!NOTE]
> Azure Cosmos Emulator를 설치, 구성 및 실행하려면 컴퓨터에 대한 관리자 권한이 있어야 합니다. 에뮬레이터는 인증서를 생성/추가하고 서비스를 실행하기 위해 방화벽 규칙도 설정합니다. 따라서 에뮬레이터가 이러한 작업을 실행할 수 있어야 합니다.

## <a name="running-on-windows"></a>Windows에서 실행

Azure Cosmos Emulator를 시작하려면 시작 단추를 선택하거나 Windows 키를 누릅니다. **Azure Cosmos Emulator** 입력을 시작하고 애플리케이션 목록에서 에뮬레이터를 선택합니다.

![시작 단추를 선택하거나 Windows 키를 누르고 **Azure Cosmos Emulator** 입력을 시작한 후 애플리케이션 목록에서 에뮬레이터를 선택합니다.](./media/local-emulator/database-local-emulator-start.png)

에뮬레이터를 실행 하는 경우 Windows 작업 표시줄 알림 영역에 아이콘이 표시 됩니다. ![Azure Cosmos DB 로컬 에뮬레이터 작업 표시줄 알림](./media/local-emulator/database-local-emulator-taskbar.png)

기본적으로 Azure Cosmos Emulator는 포트 8081에서 수신 대기하는 로컬 머신("localhost")에서 실행됩니다.

Azure Cosmos Emulator는 기본적으로 `C:\Program Files\Azure Cosmos DB Emulator`에 설치됩니다. 명령줄에서 에뮬레이터를 시작 및 중지할 수도 있습니다. 자세한 내용은 [명령줄 도구 참조](#command-line)를 참조하세요.

## <a name="start-data-explorer"></a>데이터 탐색기 시작

Azure Cosmos Emulator를 시작하면 브라우저에서 Azure Cosmos Data Explorer가 자동으로 열립니다. 주소가 `https://localhost:8081/_explorer/index.html`로 표시됩니다. 탐색기를 닫았다가 나중에 다시 열려면, 브라우저에서 URL을 열거나 아래와 같이 Windows 트레이 아이콘의 Azure Cosmos Emulator에서 실행할 수 있습니다.

![Azure Cosmos 로컬 에뮬레이터 데이터 탐색기 시작 관리자](./media/local-emulator/database-local-emulator-data-explorer-launcher.png)

## <a name="checking-for-updates"></a>업데이트 확인

데이터 탐색기에 다운로드할 수 있는 새 업데이트가 있는지 표시됩니다.

> [!NOTE]
> Azure Cosmos Emulator의 특정 버전에서 만든 데이터는(%LOCALAPPDATA%\CosmosDBEmulator 또는 데이터 경로 선택적 설정 참조) 다른 버전을 사용할 때 액세스하지 못할 수도 있습니다. 데이터를 장기간 보존하려면 Azure Cosmos Emulator 대신 Azure Cosmos 계정에 저장하는 것이 좋습니다.

## <a name="authenticating-requests"></a>요청 인증

클라우드의 Azure Cosmos DB와 마찬가지로 Azure Cosmos Emulator에 대한 모든 요청을 인증해야 합니다. Azure Cosmos Emulator는 단일 고정 계정과 마스터 키 인증에 대해 알려진 인증 키를 지원합니다. Azure Cosmos Emulator에서 사용할 수 있는 자격 증명은 이 계정과 키뿐입니다. 아래에 이 계정과 키의 예제가 나와 있습니다.

```bash
Account name: localhost:<port>
Account key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
```

> [!NOTE]
> Azure Cosmos Emulator에서 지원하는 마스터 키는 에뮬레이터 전용입니다. Azure Cosmos Emulator에서는 프로덕션 Azure Cosmos DB 계정 및 키를 사용할 수 없습니다.

> [!NOTE]
> /Key 옵션을 사용하여 에뮬레이터를 시작한 경우에는 `C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==` 대신 생성된 키를 사용하세요. /Key 옵션에 대한 자세한 내용은 [명령줄 도구 참조](#command-line-syntax)를 참조하세요.

Azure Cosmos DB와 마찬가지로 Azure Cosmos Emulator는 SSL을 통한 보안 통신만 지원합니다.

## <a name="running-on-a-local-network"></a>로컬 네트워크에서 실행

로컬 네트워크에서 에뮬레이터를 실행할 수 있습니다. 네트워크 액세스를 사용하도록 설정하려면 [명령줄](#command-line-syntax)에 `/AllowNetworkAccess` 옵션을 지정합니다. 여기에는 `/Key=key_string` 또는 `/KeyFile=file_name`도 지정해야 합니다. `/GenKeyFile=file_name`을 사용하여 미리 설정된 임의 키로 파일을 생성할 수 있습니다. 그런 다음, `/KeyFile=file_name` 또는 `/Key=contents_of_file`에 전달합니다.

처음에 네트워크 액세스를 사용하도록 설정하려면 사용자는 에뮬레이터를 종료하고 에뮬레이터의 데이터 디렉터리(%LOCALAPPDATA%\CosmosDBEmulator)를 삭제해야 합니다.

## <a name="developing-with-the-emulator"></a>에뮬레이터를 사용한 개발

### <a name="sql-api"></a>SQL API

Azure Cosmos Emulator를 데스크톱에서 실행하면, 지원되는 [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md) 또는 [Azure Cosmos DB REST API](/rest/api/cosmos-db/)를 사용하여 에뮬레이터와 상호 작용할 수 있습니다. Azure Cosmos Emulator에는 코드를 작성하지 않고도 SQL API용 컨테이너 또는 Mongodb API용 Cosmos DB를 만들어서 항목을 확인하고 편집할 수 있는 기본 제공 Data Explorer도 포함되어 있습니다.

```csharp
// Connect to the Azure Cosmos Emulator running locally
DocumentClient client = new DocumentClient(
   new Uri("https://localhost:8081"), "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==");

```

### <a name="azure-cosmos-dbs-api-for-mongodb"></a>Azure Cosmos DB의 API for MongoDB

[MongoDB용 Azure Cosmos DB의 API](mongodb-introduction.md)를 사용하는 경우에는 다음 연결 문자열을 사용하세요.

```bash
mongodb://localhost:C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==@localhost:10255/admin?ssl=true
```

### <a name="table-api"></a>테이블 API

Azure Cosmos Emulator를 데스크톱에서 실행하면, [Azure Cosmos DB Table API SDK](table-storage-how-to-use-dotnet.md)를 사용하여 에뮬레이터와 상호 작용할 수 있습니다. “/EnableTableEndpoint”를 사용하여 명령 프롬프트에서 관리자로 에뮬레이터를 시작합니다. 그런 다음, 아래 코드를 실행하여 Table API 계정에 연결합니다.

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Table;
using CloudTable = Microsoft.WindowsAzure.Storage.Table.CloudTable;
using CloudTableClient = Microsoft.WindowsAzure.Storage.Table.CloudTableClient;

string connectionString = "DefaultEndpointsProtocol=http;AccountName=localhost;AccountKey=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==;TableEndpoint=http://localhost:8902/;";

CloudStorageAccount account = CloudStorageAccount.Parse(connectionString);
CloudTableClient tableClient = account.CreateCloudTableClient();
CloudTable table = tableClient.GetTableReference("testtable");
table.CreateIfNotExists();
table.Execute(TableOperation.Insert(new DynamicTableEntity("partitionKey", "rowKey")));
```

### <a name="cassandra-api"></a>Cassandra API

“/EnableCassandraEndpoint”를 사용하여 관리자 명령 프롬프트에서 에뮬레이터를 시작합니다. 또는 환경 변수 `AZURE_COSMOS_EMULATOR_CASSANDRA_ENDPOINT=true`를 설정할 수도 있습니다.

* [Python 2.7 설치](https://www.python.org/downloads/release/python-2716/)

* [Cassandra CLI/CQLSH 설치](https://cassandra.apache.org/download/)

* 일반 명령 프롬프트 창에서 다음 명령을 실행합니다.

  ```bash
  set Path=c:\Python27;%Path%
  cd /d C:\sdk\apache-cassandra-3.11.3\bin
  set SSL_VERSION=TLSv1_2
  set SSL_VALIDATE=false
  cqlsh localhost 10350 -u localhost -p C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw== --ssl
  ```

* CQLSH 셸에서 다음 명령을 실행하여 Cassandra 엔드포인트에 연결합니다.

  ```bash
  CREATE KEYSPACE MyKeySpace WITH replication = {'class':'MyClass', 'replication_factor': 1};
  DESCRIBE keyspaces;
  USE mykeyspace;
  CREATE table table1(my_id int PRIMARY KEY, my_name text, my_desc text);
  INSERT into table1 (my_id, my_name, my_desc) values( 1, 'name1', 'description 1');
  SELECT * from table1;
  EXIT
  ```

### <a name="gremlin-api"></a>Gremlin API

“/EnableGremlinEndpoint”를 사용하여 관리자 명령 프롬프트에서 에뮬레이터를 시작합니다. 또는 환경 변수 `AZURE_COSMOS_EMULATOR_GREMLIN_ENDPOINT=true`를 설정할 수도 있습니다.

* [Install apache-tinkerpop-gremlin-console-3.3.4](https://tinkerpop.apache.org/downloads.html)

* 에뮬레이터의 Data Explorer에서 "db1" 데이터베이스와 "coll1" 컬렉션을 생성합니다. 파티션 키의 경우 "/name"을 선택합니다.

* 일반 명령 프롬프트 창에서 다음 명령을 실행합니다.

  ```bash
  cd /d C:\sdk\apache-tinkerpop-gremlin-console-3.3.4-bin\apache-tinkerpop-gremlin-console-3.3.4
  
  copy /y conf\remote.yaml conf\remote-localcompute.yaml
  notepad.exe conf\remote-localcompute.yaml
    hosts: [localhost]
    port: 8901
    username: /dbs/db1/colls/coll1
    password: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
    connectionPool: {
    enableSsl: false}
    serializer: { className: org.apache.tinkerpop.gremlin.driver.ser.GraphSONMessageSerializerV1d0,
    config: { serializeResultToString: true  }}

  bin\gremlin.bat
  ```

* Gremlin 셸에서 다음 명령을 실행하여 Gremlin 엔드포인트에 연결합니다.

  ```bash
  :remote connect tinkerpop.server conf/remote-localcompute.yaml
  :remote console
  :> g.V()
  :> g.addV('person1').property(id, '1').property('name', 'somename1')
  :> g.addV('person2').property(id, '2').property('name', 'somename2')
  :> g.V()
  ```

## <a name="export-the-ssl-certificate"></a>SSL 인증서 내보내기

.NET 언어 및 런타임은 Windows 인증서 저장소를 사용하여 Azure Cosmos DB 로컬 에뮬레이터에 안전하게 연결합니다. 다른 언어의 경우 해당 언어만의 인증서 관리 및 사용 방법이 있습니다. Java는 자체의 고유 [인증서 저장소](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html)를 사용하고 Python은 [소켓 래퍼](https://docs.python.org/2/library/ssl.html)를 사용합니다.

Windows 인증서 저장소와 통합되지 않는 런타임 및 언어와 함께 사용할 인증서를 가져오기 위해 Windows 인증서 관리자를 사용하여 인증서를 내보내야 합니다. certlm.msc를 실행하거나 [Azure Cosmos Emulator 인증서 내보내기](./local-emulator-export-ssl-certificates.md)의 단계별 지침을 수행하여 이 작업을 시작할 수 있습니다. 인증서 관리자가 실행되면 아래에 표시된 대로 개인 인증서를 열고 BASE-64 인코딩 X.509(.cer) 파일로 "DocumentDBEmulatorCertificate"라는 이름의 인증서를 내보냅니다.

![Azure Cosmos DB 로컬 에뮬레이터 SSL 인증서](./media/local-emulator/database-local-emulator-ssl_certificate.png)

[Java CA 인증서 저장소에 인증서 추가](https://docs.microsoft.com/azure/java-add-certificate-ca-store)의 지침을 따라 X.509 인증서를 Java 인증서 저장소로 가져올 수 있습니다. 인증서를 인증서 저장소에 가져오면 SQL API 및 Azure Cosmos DB의 MongoDB API용 클라이언트에서 Azure Cosmos Emulator에 연결할 수 있습니다.

Python 및 Node.js SDK에서 에뮬레이터에 연결하면 SSL 확인이 비활성화됩니다.

## <a id="command-line"></a>명령줄 도구 참조
설치 위치에서 명령줄을 사용하여 에뮬레이터를 시작 및 중지하고, 옵션을 구성하고, 다른 작업을 수행할 수 있습니다.

### <a name="command-line-syntax"></a>명령줄 구문

    CosmosDB.Emulator.exe [/Shutdown] [/DataPath] [/Port] [/MongoPort] [/DirectPorts] [/Key] [/EnableRateLimiting] [/DisableRateLimiting] [/NoUI] [/NoExplorer] [/EnableMongoDbEndpoint] [/?]

옵션 목록을 보려면 명령 프롬프트에 `CosmosDB.Emulator.exe /?` 을(를) 입력합니다.

|**옵션** | **설명** | **명령**| **인수**|
|---|---|---|---|
|[인수 없음] | 기본 설정으로 Azure Cosmos Emulator를 시작합니다. |CosmosDB.Emulator.exe| |
|[도움말] |지원되는 명령줄 인수 목록을 표시합니다.|CosmosDB.Emulator.exe /? | |
| GetStatus |Azure Cosmos Emulator의 상태를 구합니다. 상태는 종료 코드에 의해 표시됩니다. 1 = 시작, 2 = 실행, 3 = 정지. 부정적인 종료 코드는 오류가 발생 했음을 나타냅니다. 어떤 출력도 생성되지 않습니다. | CosmosDB.Emulator.exe /GetStatus| |
| Shutdown| Azure Cosmos Emulator를 종료합니다.| CosmosDB.Emulator.exe /Shutdown | |
|DataPath | 데이터 파일을 저장할 경로를 지정합니다. 기본값은 %LocalAppdata%\CosmosDBEmulator입니다. | CosmosDB.Emulator.exe /DataPath=\<datapath\> | \<datapath\>: 액세스할 수 있는 경로 |
|포트 | 에뮬레이터에 사용할 포트 번호를 지정합니다. 기본값은 8081입니다. |CosmosDB.Emulator.exe /Port=\<port\> | \<포트\>: 단일 포트 번호 |
| ComputePort | Compute Interop 게이트웨이 서비스에 사용하도록 지정된 포트 번호입니다. 게이트웨이의 HTTP 엔드포인트 프로브 포트는 ComputePort + 79로 계산됩니다. 따라서 ComputePort 및 ComputePort + 79가 열리고 사용할 수 있어야 합니다. 기본값은 8900, 8979입니다. | CosmosDB.Emulator.exe /ComputePort = \<computeport\> | \<computeport\>: 단일 포트 번호 |
| EnableMongoDbEndpoint | MongoDB API 사용 | CosmosDB.Emulator.exe /EnableMongoDbEndpoint | |
| MongoPort | MongoDB 호환성 API에 사용할 포트 번호를 지정합니다. 기본값은 10255입니다. |CosmosDB.Emulator.exe /MongoPort= \<mongoport\>|\<mongoport\>: 단일 포트 번호|
| EnableCassandraEndpoint | Cassandra API를 사용하도록 설정 | CosmosDB.Emulator.exe /EnableCassandraEndpoint | |
| CassandraPort | Cassandra 엔드포인트에 사용할 포트 번호를 지정합니다. 기본값은 10350입니다. | CosmosDB.Emulator.exe /CassandraPort = \<cassandraport\> | \<cassandraport\>: 단일 포트 번호 |
| EnableGremlinEndpoint | Gremlin API를 사용하도록 설정 | CosmosDB.Emulator.exe /EnableGremlinEndpoint | |
| GremlinPort | Gremlin 엔드포인트에 사용할 포트 번호입니다. 기본값은 8901입니다. | CosmosDB.Emulator.exe /GremlinPort=\<port\> | \<포트\>: 단일 포트 번호 |
|EnableTableEndpoint | Azure Table API를 사용하도록 설정 | CosmosDB.Emulator.exe /EnableTableEndpoint | |
|TablePort | Azure 테이블 엔드포인트에 사용할 포트 번호입니다. 기본값은 8902입니다. | CosmosDB.Emulator.exe /TablePort=\<port\> | \<포트\>: 단일 포트 번호|
| KeyFile | 지정된 파일에서 권한 부여 키를 읽어옵니다. keyfile을 생성하려면 /GenKeyFile 옵션을 사용합니다. | CosmosDB.Emulator.exe /KeyFile=\<file_name\> | \<file_name\>: 파일의 경로 |
| ResetDataPath | 지정된 경로에 있는 모든 파일을 재귀적으로 제거합니다. 경로를 지정하지 않으면 기본값은 %LOCALAPPDATA%\CosmosDbEmulator입니다. | CosmosDB.Emulator.exe /ResetDataPath[=\<path>] | \<path\>: 파일 경로  |
| StartTraces  |  디버그 추적 로그 수집을 시작합니다. | CosmosDB.Emulator.exe /StartTraces | |
| StopTraces     | 디버그 추적 로그 수집을 중지합니다. | CosmosDB.Emulator.exe /StopTraces  | |
|FailOnSslCertificateNameMismatch | 기본적으로 에뮬레이터는 인증서의 SAN에 에뮬레이터 호스트의 도메인 이름, 로컬 IPv4 주소, 'localhost' 및 '127.0.0.1'이 포함되어 있지 않으면 자체 서명된 SSL 인증서를 다시 생성합니다. 이 옵션을 사용하면 시작할 때 에뮬레이터가 실패합니다. 그러면 /GenCert 옵션을 사용하여 자체 서명된 SSL 인증서를 새로 만들고 설치해야 합니다. | CosmosDB.Emulator.exe /FailOnSslCertificateNameMismatch  | |
| GenCert | 자체 서명된 SSL 인증서를 새로 생성하고 설치합니다. 선택적으로 네트워크를 통해 에뮬레이터에 액세스하기 위해 쉼표로 구분된 추가 DNS 이름 목록을 포함합니다. | CosmosDB.Emulator.exe /GenCert[ \<comma-separated list of additional dns-names\>] | |
| DirectPorts |직접 연결에 사용할 포트를 지정합니다. 기본값은 10251,10252,10253,10254입니다. | CosmosDB.Emulator.exe /DirectPorts:\<directports\> | \<directports\>: 쉼표로 구분된 4개 포트 목록 |
| 키 |에뮬레이터에 대한 권한 부여 키입니다. 키는 64바이트 벡터의 base 64 인코딩이어야 합니다. | CosmosDB.Emulator.exe /Key:\<key\> | \<키\>: 키는 64바이트 벡터의 base 64 인코딩이어야 합니다.|
| EnableRateLimiting | 요청 속도 제한 동작을 사용하도록 지정합니다. |CosmosDB.Emulator.exe /EnableRateLimiting | |
| DisableRateLimiting |요청 속도 제한 동작을 사용하지 않도록 지정합니다. |CosmosDB.Emulator.exe /DisableRateLimiting | |
| NoUI | 에뮬레이터 사용자 인터페이스를 표시하지 않습니다. | CosmosDB.Emulator.exe /NoUI | |
| NoExplorer | 시작 시 데이터 탐색기를 표시하지 않습니다. |CosmosDB.Emulator.exe /NoExplorer | | 
| PartitionCount | 분할된 컨테이너의 최대 수를 지정합니다. 자세한 내용은 [컨테이너 수 변경](#set-partitioncount)을 참조하세요. | CosmosDB.Emulator.exe /PartitionCount=\<partitioncount\> | \<partitioncount\>: 허용되는 단일 파티션 컨테이너의 최대 수입니다. 기본값은 25입니다. 허용되는 최대값은 250입니다.|
| DefaultPartitionCount| 분할된 컨테이너에 대한 기본 파티션 수를 지정합니다. | CosmosDB.Emulator.exe /DefaultPartitionCount=\<defaultpartitioncount\> | \<defaultpartitioncount\> 기본값은 25입니다.|
| AllowNetworkAccess | 네트워크를 통해 에뮬레이터에 액세스할 수 있도록 합니다. /Key=\<key_string\> 또는 /KeyFile=\<file_name\>을 전달하여 네트워크 액세스를 활성화해야 합니다. | CosmosDB.Emulator.exe /AllowNetworkAccess /Key=\<key_string\> 또는 CosmosDB.Emulator.exe /AllowNetworkAccess /KeyFile=\<file_name\>| |
| NoFirewall | /AllowNetworkAccess 옵션이 사용되는 경우 방화벽 규칙을 조정하지 마십시오. |CosmosDB.Emulator.exe /NoFirewall | |
| GenKeyFile | 새 인증 키를 생성하고 지정된 파일에 저장합니다. 생성된 키를 /Key 또는 /KeyFile 옵션과 함께 사용할 수 있습니다. | CosmosDB.Emulator.exe /GenKeyFile=\<path to key file\> | |
| 일관성 | 계정의 기본 일관성 수준을 설정합니다. | CosmosDB.Emulator.exe /Consistency=\<consistency\> | \<일관성\>: 값은 [일관성 수준](consistency-levels.md) Session, Strong, Eventual 또는 BoundedStaleness 중 하나여야 합니다. 기본값은 Session입니다. |
| ? | 도움말 메시지를 표시합니다.| | |

## <a id="set-partitioncount"></a>컨테이너 수 변경

기본적으로 고정 크기 컨테이너를 25개까지 만들거나(Azure Cosmos DB SDK를 통해서만 지원됨) Azure Cosmos Emulator를 사용하여 무제한 컨테이너 5개를 만들 수 있습니다. **PartitionCount** 값을 수정하여 최대 250개의 고정 크기 컨테이너나 50개의 무제한 컨테이너를 만들거나 250개의 고정 크기 컨테이너를 초과하지 않는 범위에서 두 가지 조합을 만들 수 있습니다(무제한 컨테이너 1개 = 고정 크기 컨테이너 5개). 단, 200개를 초과하는 고정 크기 컨테이너로 실행되도록 에뮬레이터를 설정하는 것은 권장하지 않습니다. 디스크 IO 작업에 추가되는 오버헤드로 인해, 엔드포인트 API를 사용할 때 예기치 않은 시간 초과가 발생합니다.

현재 파티션 수가 초과된 후에 컨테이너를 만들려고 하면 에뮬레이터에서 다음 메시지와 함께 ServiceUnavailable 예외를 throw합니다.

"Sorry, we are currently experiencing high demand in this region, and cannot fulfill your request at this time. (죄송합니다. 현재 이 지역에서 많은 수요가 발생하고 있으며 지금은 귀하의 요청을 수행할 수 없습니다.) We work continuously to bring more and more capacity online, and encourage you to try again. (당사는 온라인 용량을 늘리기 위해 지속적으로 노력하고 있습니다. 다시 시도해 주십시오.)
Please do not hesitate to email askcosmosdb@microsoft.com at any time or for any reason. (언제든, 어떤 이유로든 이메일을 보내주십시오.)
ActivityId: 12345678-1234-1234-1234-123456789abc"

Azure Cosmos Emulator에서 사용 가능한 컨테이너 수를 변경하려면 다음 단계를 수행합니다.

1. 시스템 트레이에서 **Azure Cosmos DB Emulator** 아이콘을 마우스 오른쪽 단추로 클릭한 다음, **데이터 다시 설정...** 을 클릭하여 로컬 Azure Cosmos Emulator 데이터를 모두 삭제합니다.
2. `%LOCALAPPDATA%\CosmosDBEmulator` 폴더의 에뮬레이터 데이터를 모두 삭제합니다.
3. **Azure Cosmos DB 에뮬레이터** 아이콘을 마우스 오른쪽 단추로 클릭한 다음 **마침**을 클릭하여 열려 있는 모든 인스턴스를 종료합니다. 모든 인스턴스를 종료하는 데는 1분 정도 걸립니다.
4. 최신 버전의 [Azure Cosmos Emulator](https://aka.ms/cosmosdb-emulator)를 설치합니다.
5. 250 이하의 값을 설정하여 PartitionCount 플래그로 에뮬레이터를 시작합니다. 예: `C:\Program Files\Azure Cosmos DB Emulator> CosmosDB.Emulator.exe /PartitionCount=100`

## <a name="controlling-the-emulator"></a>에뮬레이터 제어

에뮬레이터는 서비스를 시작, 중지, 제거, 검색하고 상태를 검색할 수 있는 PowerShell 모듈과 함께 제공 됩니다. 다음 cmdlet을 실행하여 PowerShell 모듈을 사용합니다.

```powershell
Import-Module "$env:ProgramFiles\Azure Cosmos DB Emulator\PSModules\Microsoft.Azure.CosmosDB.Emulator"
```

또는 `PSModulesPath`에 `PSModules` 디렉터리를 배치하고 다음 명령을 참고하여 가져옵니다.

```powershell
$env:PSModulesPath += "$env:ProgramFiles\Azure Cosmos DB Emulator\PSModules"
Import-Module Microsoft.Azure.CosmosDB.Emulator
```

다음은 PowerShell에서 에뮬레이터를 제어하는 명령을 요약한 것입니다.

### `Get-CosmosDbEmulatorStatus`

**구문**

`Get-CosmosDbEmulatorStatus`

**주의**

ServiceControllerStatus 값 ServiceControllerStatus.StartPending, ServiceControllerStatus.Running 또는 ServiceControllerStatus.Stopped 중에서 하나를 반환합니다.

### `Start-CosmosDbEmulator`

**구문**

`Start-CosmosDbEmulator [-DataPath <string>] [-DefaultPartitionCount <uint16>] [-DirectPort <uint16[]>] [-MongoPort <uint16>] [-NoUI] [-NoWait] [-PartitionCount <uint16>] [-Port <uint16>] [<CommonParameters>]`

**주의**

에뮬레이터를 시작합니다. 기본적으로 이 명령은 에뮬레이터가 요청을 수락할 준비가 될 때까지 대기합니다. 에뮬레이터가 시작하자 마자 cmdlet을 반환하고자 하는 경우 -NoWait 옵션을 사용하십시오.

### `Stop-CosmosDbEmulator`

**구문**

 `Stop-CosmosDbEmulator [-NoWait]`

**주의**

에뮬레이터를 중지합니다. 기본적으로 이 명령은 에뮬레이터가 완전히 종료할 때까지 대기합니다. 에뮬레이터가 종료를 시작하자 마자 cmdlet을 반환하고자 하는 경우 -NoWait 옵션을 사용하십시오.

### `Uninstall-CosmosDbEmulator`

**구문**

`Uninstall-CosmosDbEmulator [-RemoveData]`

**주의**

에뮬레이터를 제거하고 $env:LOCALAPPDATA\CosmosDbEmulator의 전체 콘텐츠를 선택적으로 제거합니다.
Cmdlet는 에뮬레이터가 제거되기 전에 중지되도록 합니다.

## <a name="running-on-docker"></a>Docker에서 실행

Azure Cosmos Emulator는 Windows용 Docker에서 실행할 수 있습니다. 이 에뮬레이터는 Oracle Linux용 Docker에서 작동하지 않습니다.

[Windows용 Docker](https://www.docker.com/docker-windows)가 설치되면 도구 모음에서 Docker 아이콘을 마우스 오른쪽 단추로 클릭하고 **Windows 컨테이너로 전환**을 선택하여 Windows 컨테이너로 전환합니다.

다음으로 즐겨 찾는 셸에서 다음 명령을 실행하여 Docker 허브에서 에뮬레이터 이미지를 끌어옵니다.

```bash
docker pull microsoft/azure-cosmosdb-emulator
```
이미지를 시작하려면 다음 명령을 실행합니다.

명령줄에서
```cmd

md %LOCALAPPDATA%\CosmosDBEmulator\bind-mount

docker run --name azure-cosmosdb-emulator --memory 2GB --mount "type=bind,source=%LOCALAPPDATA%\CosmosDBEmulator\bind-mount,destination=C:\CosmosDB.Emulator\bind-mount" --interactive --tty -p 8081:8081 -p 8900:8900 -p 8901:8901 -p 8902:8902 -p 10250:10250 -p 10251:10251 -p 10252:10252 -p 10253:10253 -p 10254:10254 -p 10255:10255 -p 10256:10256 -p 10350:10350 microsoft/azure-cosmosdb-emulator
```

PowerShell에서:
```powershell

md $env:LOCALAPPDATA\CosmosDBEmulator\bind-mount 2>null

docker run --name azure-cosmosdb-emulator --memory 2GB --mount "type=bind,source=$env:LOCALAPPDATA\CosmosDBEmulator\bind-mount,destination=C:\CosmosDB.Emulator\bind-mount" --interactive --tty -p 8081:8081 -p 8900:8900 -p 8901:8901 -p 8902:8902 -p 10250:10250 -p 10251:10251 -p 10252:10252 -p 10253:10253 -p 10254:10254 -p 10255:10255 -p 10256:10256 -p 10350:10350 microsoft/azure-cosmosdb-emulator

```

응답은 다음과 유사합니다.

```
Starting emulator
Emulator Endpoint: https://172.20.229.193:8081/
Master Key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
Exporting SSL Certificate
You can import the SSL certificate from an administrator command prompt on the host by running:
cd /d %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
--------------------------------------------------------------------------------------------------
Starting interactive shell
```

이제 클라이언트의 응답에서 엔드포인트 및 마스터 키를 사용하고 SSL 인증서를 호스트로 가져옵니다. SSL 인증서를 가져오려면 관리자 명령 프롬프트에서 다음을 수행합니다.

명령줄에서

```cmd
cd  %LOCALAPPDATA%\CosmosDBEmulator\bind-mount
powershell .\importcert.ps1
```

PowerShell에서:
```powershell
cd $env:LOCALAPPDATA\CosmosDBEmulator\bind-mount
.\importcert.ps1
```

에뮬레이터가 시작된 후 대화형 셸을 닫으면 에뮬레이터의 컨테이너가 종료됩니다.

데이터 탐색기를 열려면 브라우저에서 다음 URL로 이동합니다. 에뮬레이터 엔드포인트가 위에 표시된 응답 메시지에 제공됩니다.

    https://<emulator endpoint provided in response>/_explorer/index.html

## Mac 또는 Linux에서 실행<a id="mac"></a>

현재 Cosmos Emulator는 Windows에서만 실행할 수 있습니다. Mac 또는 Linux를 실행 중인 사용자는 Parallels 또는 VirtualBox 등 하이퍼바이저가 호스트된 Windows 가상 머신에서 에뮬레이터를 실행할 수 있습니다. 이 기능을 사용하도록 설정하는 단계는 다음과 같습니다.

Windows VM 내에서 아래 명령을 실행하고 IPv4 주소를 기록해 둡니다.

```cmd
ipconfig.exe
```

애플리케이션 내에서 DocumentClient 개체의 URI를 변경하여 `ipconfig.exe`가 반환하는 IPv4 주소를 사용해야 합니다. 다음은 DocumentClient 개체를 생성할 때 CA 유효성 검사를 해결하는 단계입니다. 이 경우 ServerCertificateCustomValidationCallback에 대해 자체적으로 구현하는 DocumentClient 생성자에 HttpClientHandler를 제공해야 합니다.

다음은 코드 예입니다.

```csharp
using System;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using System.Net.Http;

namespace emulator
{
    class Program
    {
        static async void Main(string[] args)
        {
            string strEndpoint = "https://10.135.16.197:8081/";  //IPv4 address from ipconfig.exe
            string strKey = "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==";

            //Work around the CA validation
            var httpHandler = new HttpClientHandler()
            {
                ServerCertificateCustomValidationCallback = (req,cert,chain,errors) => true
            };

            //Pass http handler to document client
            using (DocumentClient client = new DocumentClient(new Uri(strEndpoint), strKey, httpHandler))
            {
                Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "myDatabase" });
                Console.WriteLine($"Created Database: id - {database.Id} and selfLink - {database.SelfLink}");
            }
        }
    }
}
```

마지막으로 Windows VM 내에서 다음 옵션을 사용하여 명령줄에서 Cosmos Emulator를 시작합니다.

```cmd
Microsoft.Azure.Cosmos.Emulator.exe /AllowNetworkAccess /Key=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
```

## <a name="troubleshooting"></a>문제 해결

다음은 Azure Cosmos Emulator에서 발생하는 문제를 해결하는 데 참고할만한 팁입니다.

- 새 버전의 에뮬레이터를 설치했는데 오류가 발생하는 경우 데이터를 다시 설정합니다. 시스템 트레이에서 Azure Cosmos Emulator 아이콘을 마우스 오른쪽 단추로 클릭한 다음, 데이터 다시 설정...을 클릭하여 데이터를 다시 설정할 수 있습니다. 이렇게 해도 오류가 해결되지 않으면 에뮬레이터와 에뮬레이터의 이전 버전(있는 경우)을 제거하고 "C:\Program files\Azure Cosmos DB Emulator" 디렉터리를 제거한 다음, 에뮬레이터를 다시 설치합니다. 지침은 [로컬 에뮬레이터 제거](#uninstall)를 참조하세요.

- Azure Cosmos Emulator에 크래시가 발생하는 경우 '%LOCALAPPDATA%\CrashDumps' 폴더에서 덤프 파일을 수집하여 압축하고, 이메일에 첨부하여 [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com)으로 보냅니다.

- `Microsoft.Azure.Cosmos.ComputeServiceStartupEntryPoint.exe`에서 크래시가 발생하는 경우에는 성능 카운터가 손상된 상태에서 발생하는 증상일 수 있습니다. 일반적으로 관리자 명령 프롬프트에서 다음 명령을 실행하면 문제가 해결됩니다.

  ```cmd
  lodctr /R
   ```

- 연결 문제가 발생하는 경우 [추적 파일을 수집](#trace-files)하고, 압축하고, 전자 메일에 첨부하여 [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com)으로 보냅니다.

- **서비스를 사용할 수 없음** 메시지가 나타나는 경우 에뮬레이터에서 네트워크 스택을 초기화하지 못했을 수 있습니다. 해당 네트워크 필터 드라이버로 인해 문제가 발생할 수 있으므로 Pulse 보안 클라이언트 또는 Juniper 네트워크 클라이언트를 설치했는지 확인합니다. 일반적으로 타사 네트워크 필터 드라이버를 제거하면 문제가 해결됩니다. 또는 /DisableRIO를 사용하여 에뮬레이터를 시작하면 에뮬레이터 네트워크 통신이 일반 Winsock으로 전환됩니다. 

- 에뮬레이터 실행 중에 컴퓨터가 절전 모드가 되거나 OS 업데이트를 실행할 경우 **서비스를 현재 사용할 수 없음** 메시지가 표시될 수 있습니다. Windows 알림 트레이에 표시되는 아이콘을 마우스 오른쪽 단추로 클릭하여 **데이터 다시 설정**을 선택하여 에뮬레이터 데이터를 다시 설정합니다.

### <a id="trace-files"></a>추적 파일 수집

디버깅 추적을 수집하려면 관리 명령 프롬프트에서 다음 명령을 실행합니다.

1. `cd /d "%ProgramFiles%\Azure Cosmos DB Emulator"`
2. `CosmosDB.Emulator.exe /shutdown` 프로그램이 종료되었는지 시스템 트레이를 확인합니다. 프로그램 종료에 1분 정도 걸릴 수 있습니다. Azure Cosmos Emulator 사용자 인터페이스에서 **종료**를 클릭해도 됩니다.
3. `CosmosDB.Emulator.exe /starttraces`
4. `CosmosDB.Emulator.exe`
5. 문제를 재현합니다. 데이터 탐색기가 작동하지 않는 경우 브라우저가 몇 초 간 열리고 오류를 catch할 때까지 기다리면 됩니다.
5. `CosmosDB.Emulator.exe /stoptraces`
6. `%ProgramFiles%\Azure Cosmos DB Emulator`로 이동하여 docdbemulator_000001.etl 파일을 찾습니다.
7. 재현 단계와 마찬가지로 디버깅을 위해 .etl 파일을 [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com)으로 보냅니다.

### <a id="uninstall"></a>로컬 에뮬레이터 제거

1. 시스템 트레이에서 Azure Cosmos Emulator 아이콘을 마우스 오른쪽 단추로 클릭한 다음, 종료를 클릭하여 로컬 에뮬레이터의 열려 있는 인스턴스를 모두 종료합니다. 모든 인스턴스를 종료하는 데는 1분 정도 걸립니다.
2. Windows 검색 상자에 **앱 및 기능**을 입력하고 **앱 및 기능(시스템 설정)** 결과를 클릭합니다.
3. 앱 목록에서 **Azure Cosmos DB 에뮬레이터**로 스크롤하여 선택하고, **제거**를 클릭한 다음, 확인하고 **제거**를 다시 클릭합니다.
4. 앱을 제거할 때 `%LOCALAPPDATA%\CosmosDBEmulator`로 이동하여 폴더를 삭제합니다.

## <a name="next-steps"></a>다음 단계

이 자습서에서는 무료 로컬 개발을 위해 로컬 에뮬레이터를 사용하는 방법을 살펴보았습니다. 이제 다음 자습서로 진행하여 에뮬레이터 SSL 인증서를 내보내는 방법을 알아볼 수 있습니다.

> [!div class="nextstepaction"]
> [Azure Cosmos Emulator 인증서 내보내기](local-emulator-export-ssl-certificates.md)
