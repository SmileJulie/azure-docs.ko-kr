---
title: Cosmos DB에 Azure Stream Analytics 출력
description: 이 문서에서는 비구조화된 JSON 데이터에 대한 데이터 보관 및 짧은 대기 시간 쿼리를 위해 Azure Stream Analytics를 사용하여 JSON 출력에 대한 Azure Cosmos DB에 출력을 저장하는 방법을 알아봅니다.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/11/2019
ms.custom: seodec18
ms.openlocfilehash: de5febaeecd176a8718364720132d3fa4433c57f
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443621"
---
# <a name="azure-stream-analytics-output-to-azure-cosmos-db"></a>Azure Cosmos DB에 Azure Stream Analytics 출력  
비구조화된 JSON 데이터에 대한 데이터 보관 및 짧은 대기 시간 쿼리를 사용하기 위해 Stream Analytics에서 JSON 출력의 대상을 [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/)로 지정할 수 있습니다. 이 문서에서는 이 구성을 구현하기 위한 몇 가지 모범 사례를 설명합니다.

Cosmos DB에 대해 잘 모를 경우 먼저 [Azure Cosmos DB의 학습 경로](https://azure.microsoft.com/documentation/learning-paths/documentdb/)를 살펴보세요. 

> [!Note]
> 현재 Azure Stream Analytics는 **SQL API**를 사용한 Azure Cosmos DB 연결만을 지원합니다.
> 다른 Azure Cosmos DB API는 아직 지원되지 않습니다. Azure Stream Analytics를 다른 API로 만든 Azure Cosmos DB 계정에 지정한 경우 데이터는 올바르게 저장되지 않을 수도 있습니다. 

## <a name="basics-of-cosmos-db-as-an-output-target"></a>출력 대상으로서 Cosmos DB의 기본 사항
Stream Analytics에서 Azure Cosmos DB 출력 스트림 처리 결과 JSON 출력에 Cosmos DB 컨테이너에 쓸 수 있습니다. Stream Analytics 사전에 만들어야 할 요구 하는 대신 컨테이너에서 데이터베이스를 만들지 않습니다. 이 Cosmos DB 컨테이너의 청구 비용을에 의해 제어 됩니다 있도록 성능, 일관성 및 직접 사용 하 여 컨테이너의 용량을 조정할 수 있도록 합니다 [Cosmos DB Api](https://msdn.microsoft.com/library/azure/dn781481.aspx)합니다.

> [!Note]
> Azure Cosmos DB 방화벽에서 허용되는 IP 목록에 0.0.0.0을 추가해야 합니다.

Cosmos DB 컨테이너 옵션 중 일부는 다음과 같습니다.

## <a name="tune-consistency-availability-and-latency"></a>일관성, 가용성 및 대기 시간 조정
응용 프로그램 요구 사항에 맞게, Azure Cosmos DB를 사용 하면 데이터베이스 및 컨테이너를 미세 조정 하 고 일관성, 가용성, 대기 시간 및 처리량 간의 균형을 유지할 수 있습니다. 시나리오에서 읽기 및 쓰기 대기 시간에 대해 필요로 하는 읽기 일관성 수준에 따라 데이터베이스 계정에 대한 일관성 수준을 선택할 수 있습니다. 컨테이너에서 요청 Units(RUs) 확장 하 여 처리량을 개선할 수 있습니다. 또한 기본적으로 Azure Cosmos DB에서는 컨테이너에 한 각 CRUD 작업에서 동기 인덱싱을 합니다. 이는 Azure Cosmos DB에서 쓰기/읽기 성능을 제어하는 또 다른 유용한 옵션입니다. 자세한 내용은 [데이터베이스 및 쿼리 일관성 수준 변경](../cosmos-db/consistency-levels.md) 문서를 검토하세요.

## <a name="upserts-from-stream-analytics"></a>Stream Analytics에서 Upsert
Azure Cosmos DB를 사용 하 여 Stream Analytics 통합을 사용 하면 지정된 된 문서 ID 열에 따라 컨테이너에서 레코드를 삽입 하거나 업데이트할 수 있습니다. 이를 *Upsert*라고도 합니다.

Stream Analytics에서는 문서 ID 충돌로 인해 삽입에 실패한 경우에만 업데이트가 수행되는 낙관적 Upsert 방식을 사용합니다. 호환성 수준 1.0을 사용 하 여이 업데이트는 패치를는 문서에 부분 업데이트가 가능 하므로, 새 속성 또는 기존 속성 증분적으로 수행 됩니다 교체 추가 수행 됩니다. 그렇지만 JSON 문서에서 배열 속성 값을 변경하면 전체 배열을 덮어씁니다. 즉, 배열이 병합되지 않습니다. 1\.2를 사용 하 여 upsert 동작 삽입 또는 문서를 교체 하도록 수정 됩니다. 이 호환성 수준 1.2 섹션 아래에서 자세히 설명 합니다.

들어오는 JSON 문서에 기존 ID 필드가 있는 경우 해당 필드는 자동으로 Cosmos DB에서 문서 ID 열로 사용되고 이후 쓰기가 이와 같이 처리되어 다음 상황 중 하나가 발생합니다.
- 삽입으로 이어지는 고유한 ID
- upsert로 이어지는 중복 ID 및 ‘ID’로 설정된 ‘문서 ID’
- 첫 문서 이후 오류로 이어지는 중복 ID 및 설정되지 않은 ‘문서 ID’

중복 ID가 있는 문서를 포함한 <i>모든</i> 문서를 저장하려는 경우 쿼리에서 ID 필드의 이름을 바꾸고(AS 키보드 사용) Cosmos DB에서 ID 필드를 만들거나 ID를 다른 열의 값으로 바꾸도록 합니다(AS 키보드 사용 또는 ‘문서 ID’ 설정 사용).

## <a name="data-partitioning-in-cosmos-db"></a>Cosmos DB의 데이터 분할
Azure Cosmos DB는 워크로드에 따라 파티션 크기를 자동으로 조정하므로 데이터 분할에 Azure Cosmos DB [무제한](../cosmos-db/partition-data.md) 컨테이너를 사용하는 것이 좋습니다. 무제한 컨테이너에 쓸 때 Stream Analytics는 이전 쿼리 단계 또는 입력 분할 구성표만큼 많은 병렬 기록기를 사용합니다.
> [!NOTE]
> 이때 Azure Stream Analytics 최상위 수준에서 파티션 키를 사용 하 여 무제한 컨테이너만 지원합니다. 예를 들어 `/region`이 지원됩니다. 중첩된 파티션 키(예: `/region/name`)는 지원되지 않습니다. 

선택한 파티션 키에 따라이 표시 될 수 있습니다 _경고_:

`CosmosDB Output contains multiple rows and just one row per partition key. If the output latency is higher than expected, consider choosing a partition key that contains at least several hundred records per partition key.`

고유 값 수 있고 이러한 값에 워크 로드를 균등 하 게 배포할 수 있습니다 하는 파티션 키 속성을 선택 하는 것이 반드시 합니다. 분할의 자연 스러운 아티팩트로 동일한 파티션 키를 포함 하는 요청은 단일 파티션의 최대 처리량으로 제한 됩니다. 또한 동일한 파티션 키에 속하는 문서에 대 한 저장소 크기는 10GB로 제한 합니다. 이상적인 파티션 키는 쿼리에서 자주 필터로 표시되며 솔루션 확장에 충분한 카디널리티가 있는 키입니다.

DocumentDB의 저장된 프로시저 및 트리거는 트랜잭션에 대 한 경계를 파티션 키 이기도합니다. 트랜잭션에서 함께 발생 하는 문서가 동일한 파티션 키 값을 공유할 수 있도록 파티션 키를 선택 해야 합니다. 이 문서 [Cosmos DB에서 분할](../cosmos-db/partitioning-overview.md) 파티션 키를 선택 하면 자세한 정보를 제공 합니다.

Stream Analytics는 고정 된 Azure Cosmos DB 컨테이너에 대 한 전체 일단 강화 또는 규모 확장 방법은 없습니다 수 있습니다. 10GB 및 10,000RU/s의 처리량 상한 값이 적용됩니다.  고정 컨테이너에서 무제한 컨테이너(최소 1,000RU/s 처리량 및 파티션 키가 있는 하나의 컨테이너)로 데이터를 마이그레이션하려면 [데이터 마이그레이션 도구](../cosmos-db/import-data.md) 또는 [변경 피드 라이브러리](../cosmos-db/change-feed.md)를 사용해야 합니다.

여러 고정된 컨테이너에 쓸 수는 사용 되지 및 Stream Analytics 작업 규모에 권장 되지 않습니다.

## <a name="improved-throughput-with-compatibility-level-12"></a>호환성 수준 1.2를 사용 하 여 향상 된 처리량
호환성 수준 1.2 사용 하 여 Stream Analytics에서는 네이티브 통합 대량 Cosmos DB로 작성 합니다. 이 Cosmos DB 처리량 및 핸들 조정 요청을 효율적으로 극대화를 효과적으로 쓸 수 있습니다. 향상 된 작성 메커니즘은는 upsert 동작 차이로 인해 새 호환성 수준에서 사용할 수 있습니다.  1\.2 전에 upsert 동작은 삽입 하거나 문서를 병합 합니다. 1\.2를 사용 하 여 upsert 동작 삽입 또는 문서를 교체 하도록 수정 됩니다. 

1\.2 전에 트랜잭션으로 일괄 처리를 쓸 Cosmos DB로 파티션 키 당 대량 upsert 문서에 사용자 지정 저장된 프로시저를 사용 합니다. 단일 레코드 (제한) 일시적인 오류에 도달 하는 경우에 전체 일괄 처리가 다시 시도해 야 합니다. 이 시나리오 에서도 적절 한 제한 상대적으로 속도가 느려질 수 있습니다. 다음 비교 1.2를 사용 하 여 이러한 작업 작동 하는 방법을 보여 줍니다.

다음 예제에서는 동일한 Stream Analytics 작업을 두 개의 동일한 이벤트 허브 입력에서 읽기를 보여 줍니다. 두 Stream Analytics 작업이 [완전히 분할](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#embarrassingly-parallel-jobs) CosmosDB는 동일한 컨테이너에 쓰기를 통과 쿼리를 사용 하 여 합니다. 왼쪽에 대 한 메트릭을 1.0 호환성 수준으로 구성 된 작업에서 되며 오른쪽에 있는 내용과 1.2를 사용 하 여 구성 됩니다. Cosmos DB 컨테이너의 파티션 키는 입력된 이벤트에서 들어오는 고유 GUID입니다.

![stream analytics 메트릭 비교](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-3.png)

Event Hub에서 들어오는 이벤트 속도 2x Cosmos DB가 제한에 유입, Cosmos DB 컨테이너 (20k Ru)은 구성 보다 높은 경우 그러나 1.2 사용 하 여 작업은 더 높은 처리량 (출력 이벤트/분)에서 낮은 평균 SU % 사용률을 사용 하 여 기록 일관 되 게 합니다. 사용자 환경에서이 차이점은 이벤트 형식, 입력된 이벤트/메시지 크기, 파티션 키, 쿼리 등 다양 한 등의 자세한 요소 몇 가지에 따라 달라 집니다.

![cosmos db 메트릭 비교](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-2.png)

1\.2를 사용 하 여 Stream Analytics는 거의 이루어졌기 제한 속도 제한 중에서 Cosmos DB에서 사용할 수 있는 처리량을 100%를 사용 하 여 더 지능적입니다. 동시 컨테이너에서 실행 중인 쿼리 수와 같은 다른 워크 로드에 대 한 더 나은 환경을 제공 합니다. 어떻게 ASA 확장 Cosmos DB를 사용 하 여으로 초당 1 k를 10 k에 대 한 싱크 메시지 사용을 해야 하는 경우 다음은 [azure 샘플 프로젝트](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-cosmosdb) 수 있는 작업을 수행 합니다.
Cosmos DB 출력 처리량 1.0 및 1.1을 사용 하 여 동일 note 하십시오. 1\.2는 현재 기본값 이므로 다음을 할 수 있습니다 [호환성 수준을 설정](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-compatibility-level) 사용 하 여 또는 포털을 사용 하 여 Stream Analytics 작업에 대 한 합니다 [작업 REST API 호출을 만들](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job)합니다. 있기 *좋습니다* Cosmos DB를 사용 하 여 ASA에서 호환성 수준을 1.2를 사용 하도록 합니다. 



## <a name="cosmos-db-settings-for-json-output"></a>JSON 출력에 대한 Cosmos DB 설정

Cosmos DB를 Stream Analytics의 출력으로 만들면 아래와 같은 정보를 묻는 메시지가 생성됩니다. 이 섹션에서는 속성 정의에 대해 설명합니다.

![documentdb Stream Analytics 출력 화면](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-1.png)

|필드           | 설명|
|-------------   | -------------|
|출력 별칭    | ASA 쿼리에서 이 출력을 참조할 별칭입니다.|
|구독    | Azure 구독을 선택 합니다.|
|계정 ID      | Azure Cosmos DB 계정의 이름 또는 엔드포인트 URI입니다.|
|계정 키     | Azure Cosmos DB 계정에 대한 공유 액세스 키입니다.|
|데이터베이스        | Azure Cosmos DB 데이터베이스 이름입니다.|
|컨테이너 이름 | 사용할 컨테이너 이름입니다. `MyContainer` 가 유효한 입력 샘플-하나의 컨테이너 라는 `MyContainer` 존재 해야 합니다.  |
|문서 ID     | 선택 사항입니다. 삽입 또는 업데이트 작업의 기준으로 사용해야 하는 고유 키로 사용되는 출력 이벤트의 열 이름입니다. 이 필드를 비워두면 업데이트 옵션 없이 모든 이벤트가 삽입됩니다.|
