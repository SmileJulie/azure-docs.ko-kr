---
title: Azure Cosmos 컨테이너 및 데이터베이스에 대한 처리량 프로비전
description: Azure Cosmos 컨테이너 및 데이터베이스에 프로비전되는 처리량을 설정하는 방법을 알아봅니다.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/14/2019
ms.author: rimman
ms.openlocfilehash: adf0891203321ca02c47494f1865ca78a833e301
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561388"
---
# <a name="provision-throughput-on-containers-and-databases"></a>컨테이너 및 데이터베이스에 대한 처리량 프로비전

Azure Cosmos 데이터베이스는 컨테이너 세트의 관리 단위입니다. 데이터베이스는 스키마 제약 없는 컨테이너의 집합으로 구성됩니다. Azure Cosmos 컨테이너는 처리량과 스토리지 모두에 대한 확장성 단위입니다. 컨테이너는 Azure 지역 내에 있는 머신 세트에 수평적으로 분할되고 Azure Cosmos 계정과 연결된 모든 Azure 지역에 분산됩니다.

Azure Cosmos DB를 사용 하 여 두 세분성 처리량 프로 비전 할 수 있습니다.
 
- Azure Cosmos 컨테이너
- Azure Cosmos 데이터베이스

## <a name="set-throughput-on-a-container"></a>컨테이너의 처리량 설정  

Azure Cosmos 컨테이너에 프로 비전 된 처리량을 해당 컨테이너에 대 한 단독으로 예약 되어 있습니다. 컨테이너는 항상 프로비전된 처리량을 받습니다. 컨테이너에 프로비전된 처리량은 재정적으로 SLA의 지원을 받습니다. 컨테이너의 처리량을 구성 하는 방법에 알아보려면 참조 [Azure Cosmos 컨테이너에 프로 비전 처리량](how-to-provision-container-throughput.md)합니다.

컨테이너에서 프로 비전 된 처리량을 설정에 자주 사용 하는 옵션입니다. 사용 하 여 원하는 양의 처리량을 프로 비전 하 여 컨테이너에 대 한 처리량을 탄력적으로 확장할 수 있습니다 [요청 단위 (Ru)](request-units.md)합니다. 

Azure Cosmos 컨테이너에서 프로비전된 처리량은 컨테이너의 모든 논리 파티션에 균일하게 분산됩니다. 논리 파티션에 대 한 처리량을 선택적으로 지정할 수 없습니다. 컨테이너에 있는 하나 이상의 논리 파티션은 실제 파티션에서 호스트되므로, 실제 파티션은 컨테이너에 배타적으로 포함되고 컨테이너에서 프로비전된 처리량을 지원합니다. 

논리 파티션에 실행 하는 작업 논리 파티션에 할당 된 처리량 보다 더를 사용 하는 경우 작업 속도가 제한을 가져옵니다. 속도 제한이 발생 하면 전체 컨테이너에 대 한 프로 비전된 된 처리량을 증가 하거나 작업을 다시 시도 합니다. 분할에 대한 자세한 내용은 [논리 파티션](partition-data.md)을 참조하세요.

컨테이너 성능을 보장하려는 경우 컨테이너 세분성에서 처리량을 구성하는 것이 좋습니다.

다음 이미지는 실제 파티션이 컨테이너의 논리 파티션을 하나 이상 호스트하는 방법을 보여줍니다.

![실제 파티션](./media/set-throughput/resource-partition.png)

## <a name="set-throughput-on-a-database"></a>데이터베이스의 처리량 설정

Azure Cosmos 데이터베이스의 처리량을 프로비전하는 경우 데이터베이스의 모든 컨테이너 간에 처리량이 공유됩니다. 예외는 데이터베이스에는 특정 컨테이너에서 프로 비전 된 처리량을 지정한 경우. 데이터베이스 수준 프로 비전된 된 처리량의 컨테이너 간에 공유 하는 것은 컴퓨터의 클러스터에서 데이터베이스를 호스트 비슷합니다. 데이터베이스 내의 모든 컨테이너가 머신에 제공되는 리소스를 공유하므로 당연히 특정 컨테이너에 대한 예상 성능이 제공되지 않습니다. 참조 데이터베이스에 프로 비전 된 처리량을 구성 하는 방법에 알아보려면 [Azure Cosmos 데이터베이스에서 프로 비전 된 처리량 구성](how-to-provision-database-throughput.md)합니다.

나타나는 해당 데이터베이스에 대해 프로 비전 된 처리량을 항상 보장 하는 Azure Cosmos 데이터베이스에서 처리량을 설정 합니다. 데이터베이스 내 모든 컨테이너가 프로비전된 처리량을 공유하므로, Azure Cosmos DB는 해당 데이터베이스의 특정 컨테이너에 대해 예측 가능한 처리량을 보장하지 않습니다. 특정 컨테이너가 받을 수 있는 처리량은 다음 조건에 따라 다릅니다.

* 컨테이너 수
* 다양한 컨테이너에 대해 선택한 파티션 키
* 컨테이너의 다양한 논리 파티션에 분산되는 워크로드 

특정 컨테이너의 전용 처리량으로 지정하지 않고 여러 컨테이너 간에 처리량을 공유하려는 경우 데이터베이스의 처리량을 구성하는 것이 좋습니다. 

다음 예제에서는 데이터베이스 수준에서 처리량을 프로비전하는 것이 좋은 경우를 보여 줍니다.

* 데이터베이스에 프로비전된 처리량을 컨테이너 집합 간에 공유하는 방법은 다중 테넌트 애플리케이션에 유용합니다. 각 사용자를 고유한 Azure Cosmos 컨테이너로 나타낼 수 있습니다.

* 데이터베이스에 프로비전된 처리량을 컨테이너 세트 간에 공유하는 방법은 VM 클러스터에 호스트된 MongoDB, Cassandra 등의 NoSQL 데이터베이스를 마이그레이션하거나 온-프레미스 물리적 서버에서 Azure Cosmos DB로 마이그레이션하는 경우에 유용합니다. Azure Cosmos 데이터베이스에 구성된 프로비전된 처리량은 MongoDB 또는 Cassandra 클러스터의 컴퓨팅 용량과 논리적으로 상응하지만 더 비용 효과적이고 탄력적인 것으로 생각하면 됩니다.  

프로 비전 된 처리량을 사용 하 여 데이터베이스 내에서 만들어진 모든 컨테이너를 사용 하 여 만들어야 합니다는 [파티션 키](partition-data.md)합니다. 데이터베이스 내 컨테이너에 할당된 처리량은 지정된 시간에 해당 컨테이너의 모든 논리 파티션 간에 분산됩니다. 컨테이너는 데이터베이스에 구성 된 프로 비전 된 처리량을 공유 하는 경우, 논리 파티션 또는 특정 컨테이너에 처리량을 선택적으로 적용할 수 없습니다. 

논리 파티션의 워크로드가 특정 논리 파티션에 할당된 처리량보다 많은 양을 사용하는 경우 작업 속도가 제한됩니다. 속도 제한이 발생 하면 전체 데이터베이스에 대 한 처리량을 증가 하거나 작업을 다시 시도 합니다. 분할에 대한 자세한 내용은 [논리 파티션](partition-data.md)을 참조하세요.

데이터베이스에 프로 비전 된 처리량을 공유 하는 다른 컨테이너에 속하는 여러 논리 파티션이 동일한 실제 파티션에 호스트할 수 있습니다. 실제 파티션 내에서 컨테이너의 단일 논리 파티션에 범위는 항상 하는 동안 *"L"* 간에 논리 파티션이 *"C"* 데이터베이스의 프로 비전된 된 처리량을 공유 하는 컨테이너 수 매핑되고에 호스팅된 *"R"* 실제 파티션 합니다. 

다음 이미지는 데이터베이스 내 여러 컨테이너에 속하는 하나 이상의 논리 파티션을 실제 파티션에 호스트할 수 있다는 것을 보여줍니다.

![실제 파티션](./media/set-throughput/resource-partition2.png)

## <a name="set-throughput-on-a-database-and-a-container"></a>데이터베이스 및 컨테이너의 처리량 설정

두 모델을 결합할 수 있습니다. 데이터베이스와 컨테이너의 처리량을 둘 다 프로비전할 수 있습니다. 다음 예제는 Azure Cosmos 데이터베이스 및 컨테이너의 처리량을 프로비전하는 방법을 보여줍니다.

* 명명 된 Azure Cosmos 데이터베이스를 만들 수 있습니다 *Z* 프로 비전 된 처리량 *"K"* Ru 합니다. 
* 다음으로 라는 5 개의 컨테이너를 만듭니다 *A*, *B*를 *C*, *D*, 및 *E* 데이터베이스 내에서. 컨테이너 B를 만들 때 사용 하도록 설정 되었는지 확인 **이 컨테이너에 대 한 전용된 처리량을 프로 비전** 옵션을 명시적으로 구성할 *"P"* 이 컨테이너에서 프로 비전 된 처리량 Ru 합니다. 구성할 수 있는 참고 공유 및 전용 처리량을 데이터베이스 및 컨테이너를 만들 때만 합니다. 

   ![컨테이너 수준에서 처리량을 설정합니다.](./media/set-throughput/coll-level-throughput.png)

* *"K"* Ru 처리량은 4 개의 컨테이너 간에 공유 되 *A*를 *C*, *D*, 및 *E*합니다. 정확한 양의 처리량을 사용할 수 있습니다 *A*를 *C*를 *D*, 또는 *E* 달라 집니다. 각 개별 컨테이너의 처리량에 대한 SLA는 없습니다.
* 라는 컨테이너 *B* 가져오려는 보장 되는 *"P"* Ru 처리량 항상 합니다. SLA가 지원됩니다.

## <a name="update-throughput-on-a-database-or-a-container"></a>업데이트 데이터베이스 또는 컨테이너에 대 한 처리량

컨테이너를 Azure Cosmos 데이터베이스 또는 데이터베이스를 만든 후에 프로 비전된 된 처리량을 업데이트할 수 있습니다. 데이터베이스 또는 컨테이너에서 구성할 수 있는 최대 프로 비전 된 처리량에는 제한이 없습니다. 최소 프로 비전 된 처리량은 다음 요인에 따라 달라 집니다. 

* 컨테이너에 저장할 수 있는 최대 데이터 크기
* 그 어느 때 컨테이너에서 프로 비전 하는 최대 처리량
* 그 어느 때 공유 처리량을 사용 하 여 데이터베이스에서 만든 Azure Cosmos 컨테이너의 최대 수입니다. 

Sdk를 사용 하 여 컨테이너 또는 데이터베이스의 최소 처리량을 프로그래밍 방식으로 검색 하거나 Azure portal에서 값을 확인할 수 있습니다. .NET SDK를 사용 하 여 [DocumentClient.ReplaceOfferAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.replaceofferasync?view=azure-dotnet) 메서드를 사용 하면 프로 비전 된 처리량 값을 조정할 수 있습니다. Java SDK를 사용 하 여 [RequestOptions.setOfferThroughput](sql-api-java-samples.md#offer-examples) 메서드를 사용 하면 프로 비전 된 처리량 값을 조정할 수 있습니다. 

.NET SDK를 사용 하 여 [DocumentClient.ReadOfferAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.readofferasync?view=azure-dotnet) 메서드를 사용 하면 컨테이너 또는 데이터베이스의 최소 처리량을 검색할 수 있습니다. 

언제 든 지 컨테이너 또는 데이터베이스를 프로 비전된 된 처리량을 확장할 수 있습니다. 처리량을 증가 시키려면 크기 조정 작업을 수행 하면 필요한 리소스를 프로 비전 하는 시스템 작업 때문에 더 긴 시간이 걸릴 수 있습니다. Azure 포털 또는 Sdk를 사용 하 여 프로그래밍 방식으로 크기 조정 작업의 상태를 확인할 수 있습니다. .NET SDK를 사용할 때 사용 하 여 크기 조정 작업의 상태를 가져올 수 있습니다는 `DocumentClient.ReadOfferAsync` 메서드.

## <a name="comparison-of-models"></a>모델 비교

|**매개 변수**  |**데이터베이스에 프로비전된 처리량**  |**컨테이너에 프로비전된 처리량**|
|---------|---------|---------|
|최소 RU |400(처음 네 개의 컨테이너 후 각 추가 컨테이너에는 초당 최소 100RU가 필요합니다.) |400|
|컨테이너당 최소 RU|100|400|
|최대 RU|데이터베이스에서 무제한|컨테이너에서 무제한|
|특정 컨테이너에 할당 또는 제공되는 RU|보장되지 않습니다. 지정된 컨테이너에 할당되는 RU는 속성에 따라 다릅니다. 속성은 처리량, 워크로드 분산 및 컨테이너 수를 공유하는 컨테이너 파티션 키의 선택 항목일 수 있습니다. |컨테이너에 구성된 모든 RU는 컨테이너에만 배타적으로 예약됩니다.|
|컨테이너의 최대 스토리지|무제한.|무제한.|
|컨테이너의 논리 파티션당 최대 처리량|10K RU|10K RU|
|컨테이너의 논리 파티션당 최대 스토리지(데이터 + 인덱스)|10 GB|10 GB|

## <a name="next-steps"></a>다음 단계

* [논리 파티션](partition-data.md)에 대해 자세히 알아봅니다.
* [Azure Cosmos 컨테이너의 처리량을 프로비전](how-to-provision-container-throughput.md)하는 방법을 알아봅니다.
* [Azure Cosmos 데이터베이스의 처리량을 프로비전](how-to-provision-database-throughput.md)하는 방법을 알아봅니다.

