---
title: Azure Database for PostgreSQL의 쿼리 저장소 사용 시나리오
description: 이 문서에서는 Azure Database for PostgreSQL의 쿼리 저장소에 대한 몇 가지 시나리오를 설명합니다.
services: postgresql
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 5da10faca653d0eddb50568165eb9d7ad1f877e4
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46950423"
---
# <a name="usage-scenarios-for-query-store"></a>쿼리 저장소 사용 시나리오

**적용 대상:** Azure Database for PostgreSQL 9.6 및 10

> [!IMPORTANT]
> 쿼리 저장소 기능은 공용 미리 보기로 제공됩니다.

예측 가능한 작업 로드 성능을 추적하고 유지 관리하는 것이 중요한 매우 다양한 시나리오에서 쿼리 저장소를 사용할 수 있습니다. 다음 예제를 살펴보세요. 
- 비용이 많이 드는 상위 쿼리 식별 및 조정 
- A/B 테스트 
- 업그레이드 중에 성능을 안정적으로 유지 
- 임시 워크로드 식별 및 개선 

## <a name="identify-and-tune-expensive-queries"></a>비용이 많이 드는 쿼리 식별 및 조정 

### <a name="identify-longest-running-queries"></a>가장 오래 실행되는 쿼리 식별 
Azure Portal의 [Query Performance Insight](concepts-query-performance-insight.md) 보기를 사용하여 가장 오래 실행되는 쿼리를 신속하게 식별하세요. 일반적으로 이러한 쿼리는 상당한 양의 리소스를 소비하는 경향이 있습니다. 가장 오래 실행되는 질문을 최적화하면 시스템에서 실행되는 다른 쿼리가 사용하도록 리소스를 해제하여 성능을 개선할 수 있습니다. 

### <a name="target-queries-with-performance-deltas"></a>성능 델타를 사용하여 쿼리 대상 지정 
쿼리 저장소는 성능 데이터를 시간 창으로 분할하므로 시간 경과에 따라 쿼리 성능을 추적할 수 있습니다. 이를 통해 전체적인 소요 시간 증가의 원인이 되는 쿼리를 정확히 식별할 수 있습니다. 따라서 워크로드에 대한 대상 지정 문제 해결을 수행할 수 있습니다.

### <a name="tuning-expensive-queries"></a>비용이 많이 드는 쿼리 조정 
차선의 성능으로 쿼리를 식별할 경우 수행하는 작업은 문제의 특성에 따라 달라집니다. 
- [성능 권장 사항](concepts-performance-recommendations.md)을 사용하여 제안된 인덱스가 있는지 확인하세요. 있는 경우 인덱스를 만든 다음, 쿼리 저장소를 사용하여 인덱스를 만든 후의 쿼리 성능을 평가합니다. 
- 쿼리에서 사용하는 기본 테이블에 대한 통계가 최신 상태인지 확인하세요.
- 비용이 많이 드는 쿼리를 다시 작성하는 것이 좋습니다. 예를 들어 쿼리 매개 변수화를 사용하고 동적 SQL 사용을 줄이세요. 응용 프로그램 쪽이 아닌 데이터베이스 쪽에서 데이터 필터링을 적용하는 것과 같이 데이터를 읽을 때 최적의 논리를 구현하세요. 


## <a name="ab-testing"></a>A/B 테스트 
쿼리 저장소를 사용하여 응용 프로그램이 도입할 계획을 변경하기 전과 후의 워크로드 성능을 비교하세요. 쿼리 저장소를 사용하여 환경과 응용 프로그램이 워크로드 성능에 미치는 영향을 평가하는 예제는 다음과 같습니다. 
- 응용 프로그램의 새 버전 출시. 
- 서버에 다른 리소스 추가. 
- 비용이 많이 드는 쿼리가 참조하는 테이블에서 누락된 인덱스 만들기. 
 
이러한 시나리오에서 다음 워크플로를 적용합니다. 
1. 성능 기준선을 생성하기 위한 계획된 변경 전에 쿼리 저장소를 사용하여 워크로드를 실행합니다. 
2. 계획된 시점에 응용 프로그램 변경 내용을 적용합니다. 
3. 변경 후 시스템의 성능 이미지를 생성할 만큼 충분히 오래 워크로드를 계속 실행합니다. 
4. 변경 전과 후의 결과를 비교합니다. 
5. 변경 또는 롤백을 유지할지 결정합니다. 


## <a name="identify-and-improve-ad-hoc-workloads"></a>임시 워크로드 식별 및 개선 
일부 워크로드에는 전체 응용 프로그램 성능을 개선하기 위해 조정할 수 있는 주요 쿼리가 없습니다. 일반적으로 이러한 워크로드는 비교적 많은 고유 쿼리로 구성되고, 이러한 각 쿼리는 시스템 리소스의 일부를 소비합니다. 각 고유 쿼리는 자주 실행되지 않으므로 개별적으로 런타임 소비가 중요하지 않습니다. 반면에 응용 프로그램이 항상 새 쿼리를 생성하는 경우 시스템 리소스의 상당한 부분이 쿼리 컴파일에 사용되고 이는 최적 상황이 아닙니다. 일반적으로 이 상황은 응용 프로그램이 쿼리를 생성하는 경우(저장 프로시저 또는 매개 변수가 있는 쿼리를 사용하는 대신) 또는 기본적으로 쿼리를 생성하는 개체 관계형 매핑 프레임워크를 사용하는 경우 발생합니다. 
 
응용 프로그램 코드를 제어하고 있는 경우 저장 프로시저나 매개 변수가 있는 쿼리를 사용하도록 데이터 액세스 레이어를 다시 작성하는 것이 좋습니다. 그러나 전체 데이터베이스(모든 쿼리)에 또는 동일한 쿼리 해시를 사용하는 개별 쿼리 템플릿에 쿼리 매개 변수화를 적용하면 응용 프로그램을 변경하지 않고 이 상황을 개선할 수도 있습니다. 

## <a name="next-steps"></a>다음 단계
- [쿼리 저장소 사용 모범 사례](concepts-query-store-best-practices.md)에 대해 자세히 알아보기