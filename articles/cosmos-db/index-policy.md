---
title: Azure Cosmos DB 인덱싱 정책 | Microsoft Docs
description: Azure Cosmos DB에서 인덱싱의 작동 방식을 파악하고 자동 인덱싱 및 성능 향상을 위해 인덱싱 정책을 구성 및 변경하는 방법에 대해 알아봅니다.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/10/2018
ms.author: mjbrown
ms.openlocfilehash: ffb70ce8c26b7774e90801271c55cd8a80906c90
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51628853"
---
# <a name="indexing-policy-in-azure-cosmos-db"></a>Azure Cosmos DB의 인덱싱 정책

다음 매개 변수를 구성하여 Azure Cosmos 컨테이너에서 기본 인덱싱 정책을 재정의할 수 있습니다.

* **인덱스에서 항목 및 경로 포함 또는 제외**: 컨테이너 내 항목을 삽입하거나 바꿀 때 인덱스에서 특정 항목을 제외하거나 포함할 수 있습니다. 또한 컨테이너 간에 인덱싱할 특정 경로/속성을 포함하거나 제외할 수 있습니다. 경로는 와일드카드 패턴(예: *)을 포함할 수 있습니다.

* **인덱스 유형 구성**: 범위 인덱싱 경로 외에도 공간과 같은 다른 유형의 인덱스를 추가할 수 있습니다.

* **인덱스 모드 구성**: 컨테이너에서 인덱싱 정책을 사용하면 *일관성* 또는 *없음*과 같은 다른 인덱싱 모드를 구성할 수 있습니다.

## <a name="indexing-modes"></a>인덱싱 모드 

Azure Cosmos DB는 Azure Cosmos 컨테이너에서 구성할 수 있는 두 가지 인덱싱 모드를 지원합니다. 인덱싱 정책을 통해 다음 두 가지 인덱싱 모드를 구성할 수 있습니다. 

* **일관성**: Azure Cosmos 컨테이너의 정책이 일관성으로 설정되어 있는 경우 특정 컨테이너의 쿼리는 지점 읽기(강력, 제한된 부실, 세션 또는 최종)에 대해 지정된 것과 동일한 일관성 수준을 따릅니다. 

  인덱스는 항목을 업데이트할 때 동기적으로 업데이트됩니다. 예를 들어, 항목에 대한 삽입, 바꾸기, 업데이트 및 삭제 작업은 인덱스 업데이트로 이어집니다. 일관성 인덱싱은 일관된 쿼리를 지원하는 대신 쓰기 처리량에 영향을 미칩니다. 쓰기 처리량의 감소는 “인덱싱에 포함된 경로” 및 “일관성 수준”에 따라 다릅니다. 일관성 인덱싱 모드는 워크로드를 신속하게 쓰고 즉시 쿼리하기 위해 설계되었습니다.

* **없음**: 없음 인덱스 모드를 사용하는 컨테이너에는 연관된 인덱스가 없습니다. 이 모드는 Azure Cosmos 데이터베이스를 키-값 스토리지로 이용하고 해당 ID 속성에 의해서만 항목에 액세스하는 경우에 흔히 사용됩니다.

  > [!NOTE]
  > 인덱싱 모드를 없음으로 구성하면 기존 인덱스가 삭제되는 부작용이 있습니다. 액세스 패턴에 ID 또는 자체 링크만 필요한 경우 이 옵션을 사용해야 합니다.

쿼리 일관성 수준은 일반 읽기 작업과 비슷하게 유지됩니다. Azure Cosmos 데이터베이스는 없음 인덱싱 모드를 사용하는 컨테이너를 쿼리하는 경우에 오류를 반환합니다. REST API의 명시적  **x-ms-documentdb-enable-scan**  헤더 또는 .NET SDK의  **EnableScanInQuery** 요청 옵션을 통한 검사로 쿼리를 실행할 수 있습니다. ORDER BY와 같은 일부 쿼리 기능은 해당 인덱스를 위임하므로 현재 **EnableScanInQuery**에서 지원되지 않습니다.

## <a name="modifying-the-indexing-policy"></a>인덱싱 정책 수정

Azure Cosmos DB에서는 언제든지 컨테이너의 인덱싱 정책을 업데이트할 수 있습니다. Azure Cosmos 컨테이너에서 인덱싱 정책을 변경하면 인덱스 모양이 변경될 수 있습니다. 이 변경은 인덱싱할 수 있는 경로, 해당 전체 자릿수 및 인덱스 자체의 일관성 모델에 영향을 줍니다. 따라서 인덱싱 정책을 변경하려면 실제로 이전 인덱스를 새 인덱스로 변환해야 합니다.

### <a name="index-transformations"></a>인덱스 변환

모든 인덱스 변환은 온라인으로 이루어집니다. 이전 정책을 기준으로 인덱싱된 항목이 컨테이너의 쓰기 가용성 또는 프로비전된 처리량에 영향을 주지 않고 새 정책을 기준으로 효율적으로 변환됩니다. REST API 또는 SDK를 사용하거나 저장 프로시저 및 트리거 내에서 수행된 읽기 및 쓰기 작업의 일관성은 인덱스 변환 도중에 영향을 받지 않습니다.

인덱싱 정책을 변경하는 것은 비동기 작업이며 작업 완료 시간은 항목의 수, 프로비전된 처리량 및 항목 크기에 따라 달라집니다. 인덱싱이 다시 진행되는 동안 쿼리가 수정 중인 인덱스를 사용하는 경우 쿼리가 일치하는 결과를 일부 반환하지 않을 수 있으며, 쿼리는 오류/실패를 반환하지 않습니다. 인덱싱이 다시 진행되는 동안 쿼리는 결과적으로 인덱싱 모드 구성에 관계 없이 일관됩니다. 인덱스 변환이 완료된 후 계속 일관된 결과를 확인할 수 있습니다. 이는 REST API, SDK 또는 저장 프로시저 및 트리거와 같은 인터페이스에서 발행되는 쿼리에 적용됩니다. 인덱스 변환은 특정 복제본에 대해 사용 가능한 예비 리소스를 사용하여 백그라운드에서 복제본에 대해 비동기적으로 수행됩니다.

모든 인덱스 변환도 수행됩니다. Azure Cosmos DB는 인덱스의 두 복사본을 유지 관리하지 않습니다. 따라서 인덱스 변환을 수행하는 동안 컨테이너에서 디스크 공간이 추가로 필요하거나 사용되지 않습니다.

인덱싱 정책을 변경하면 기본적으로 인덱싱 모드 구성에 따라 변경 내용이 적용되어 이전 인덱스에서 새 인덱스로 이동됩니다. 인덱싱 모드 구성은 포함/제외된 경로, 인덱스 종류 및 전체 자릿수와 같은 다른 속성에 비해 중요한 역할을 합니다.

이전 및 새 인덱싱 정책 모두가 **일관성** 인덱싱을 사용하는 경우, Azure Cosmos 데이터베이스는 온라인 인덱스 변환을 수행합니다. 변환이 진행 중인 동안 일관성 인덱싱 모드인 다른 인덱싱 정책 변경을 적용할 수 없습니다. 없음 인덱싱 모드로 이동하면 인덱스가 즉시 삭제됩니다. 진행 중인 변환을 취소하고 다른 인덱싱 정책으로 새로 시작하려는 경우 없음으로 이동하는 것이 유용합니다.

인덱스 변환을 성공적으로 완료하기 위해 컨테이너에 충분한 스토리지 공간이 있는지 확인해야 합니다. 컨테이너가 해당 스토리지 할당량에 도달하면 인덱스 변환이 일시 중지됩니다. 일부 항목을 삭제하는 것처럼 스토리지 공간을 사용할 수 있게 되면 인덱스 변환이 자동으로 다시 시작됩니다.

## <a name="modifying-the-indexing-policy---examples"></a>인덱싱 정책 수정 - 예제

다음은 인덱싱 정책을 업데이트하는 가장 일반적인 사용 사례입니다.

* 정상 작동 중에는 일관된 결과를 제공하지만, 대량 데이터를 가져오는 동안에는 **없음** 인덱싱 모드로 대체하려는 경우.

* 현재 Azure Cosmos 컨테이너에 인덱싱 기능을 사용하려는 경우. 예를 들어 지리 공간적 쿼리를 사용하려면 공간 인덱스 종류가 필요하고 ORDER BY/문자열 범위 쿼리를 사용하려면 문자열 범위 인덱스 종류가 필요할 수 있습니다.

* 인덱싱할 속성을 수동으로 선택하고 시간에 따라 변경하여 워크로드에 맞게 조정하려는 경우.

* 인덱싱 전체 자릿수를 조정하여 쿼리 성능을 향상하거나 소비되는 스토리지 공간을 줄이려는 경우.

## <a name="next-steps"></a>다음 단계

다음 문서에서 인덱싱에 대해 자세히 알아보세요.

* [인덱싱 개요](index-overview.md)
* [인덱싱 형식](index-types.md)
* [인덱스 경로](index-paths.md)
* [인덱싱 정책을 관리하는 방법](how-to-manage-indexing-policy.md)