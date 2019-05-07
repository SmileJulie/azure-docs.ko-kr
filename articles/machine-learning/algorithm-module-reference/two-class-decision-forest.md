---
title: '2 클래스 의사 결정 포리스트: 모듈 참조'
titleSuffix: Azure Machine Learning service
description: 의사 결정 포리스트 알고리즘을 기반으로 모델을 학습 하는 컴퓨터를 만드는 Azure Machine Learning 서비스에서 2 클래스 의사 결정 포리스트 모듈을 사용 하는 방법에 알아봅니다.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: f3e7cab6e33fa9373095441685f6ecb04f1d91a5
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029372"
---
# <a name="two-class-decision-forest-module"></a>2 클래스 의사 결정 포리스트 모듈

이 문서에서는 Azure Machine Learning 서비스에 대 한 시각적 인터페이스 (미리 보기)의 모듈을 설명 합니다.

의사 결정 포리스트 알고리즘을 기반으로 모델을 학습 하는 컴퓨터를 만들려면이 모듈을 사용 합니다.  

의사 결정 포리스트는 고속 지도 앙상블 모델입니다. 이 모듈을 것이 좋습니다는 최대 두 개의 결과 사용 하 여 대상을 예측 하려는 경우입니다. 

## <a name="understanding-decision-forests"></a>의사 결정 포리스트 이해

이 의사 결정 포리스트 알고리즘은 앙상블 학습 방법 분류 작업에 대 한 것입니다. 앙상블 메서드는 일반 원칙을 기반으로 하는 단일 모델에 의존 하는 대신 가져올 수 있습니다 더 나은 결과 좀 더 일반화 된 모델을 여러 관련된 모델을 만들고 몇 가지 방식으로 결합 하 여 합니다. 일반적으로 앙상블 모델 검사 및 단일 의사 결정 트리에 비해 정확성 향상을 제공합니다. 

개별 모델을 만들고 앙상블에 결합 하는 방법은 여러 가지가 있습니다. 여러 의사 결정 트리를 작성 하 여 의사 결정 포리스트를이 특정 구현의 작동 차례로 **투표** 가장 인기 있는 출력 클래스에 있습니다. 투표 앙상블 모델의 결과 생성 하기 위한 잘 알려져 있는 메서드 중 하나입니다. 

+ 전체 데이터 집합을 사용 하 여 여러 개별 분류 트리에 생성 되지만 다른 (일반적으로 임의) 시작점입니다. 이는 개별 의사 결정 트리에 사용할 수도 기능 데이터의 임의 일부는 임의 포리스트 방식에서 다릅니다.
+ 의사 결정 포리스트 트리의 각 트리는 레이블 빈도 정규화 되지 않은 히스토그램을 출력합니다. 
+ 집계 프로세스 이러한 히스토그램을 더하고 "확률" 각 레이블에 대해 가져오려는 결과 정규화 합니다. 
+ 높은 예측 신뢰도 있는 트리 앙상블의 최종 결정에 더 높은 가중치가 부여를 해야 합니다.

의사 결정 트리는 분류 작업에 대 한 여러 이점을 일반적 있습니다.
  
- 비선형 의사 결정 경계를 캡처할 수 있습니다.
- 학습 하 고 계산 및 메모리 사용량에서 효율적으로 대량의 데이터를 대 예측할 수 있습니다.
- 기능 선택은 교육 및 분류 프로세스에 통합 됩니다.  
- 트리는 노이즈가 많은 데이터와 다양 한 기능에 수용할 수 있습니다.  
- 이들은 다양 한 배포를 사용 하 여 데이터를 처리할 수 있도록 의미 비 파라메트릭 모델입니다. 

그러나 간단한 의사 결정 트리 데이터를 바로 잡음 수 되며 트리 앙상블 보다 덜 생길 수 있습니다.

자세한 내용은 [의사 결정 포리스트](http://go.microsoft.com/fwlink/?LinkId=403677)합니다.  

## <a name="how-to-configure"></a>구성 방법
  
1.  추가 된 **2 클래스 의사 결정 포리스트** 모듈을 열고 Azure Machine Learning에서 실험을 **속성** 모듈의 창. 

    아래에 있는 모듈을 찾을 수 있습니다 **Machine Learning**합니다. 확장 **초기화할**를 차례로 **분류**합니다.  
  
2.  에 대 한 **메서드를 다시**, 개별 트리를 만드는 데 사용할 메서드를 선택 합니다.  선택할 수 있습니다 **모음** 하거나 **복제**합니다.  
  
    -   **모음**: Bagging 라고 *부트스트랩 집계*합니다. 이 방법에서는 데이터 집합의 원래 크기 될 때까지 대체를 사용 하 여 원래 데이터 집합을 임의로 샘플링 하 여 만든 새 샘플에서 각 트리 증가 합니다.  
  
         모델의 출력으로 결합 되어 *투표*, 집계의 양식입니다. 각 트리에 분류 의사 결정 포리스트를 레이블의 정규화 되지 않은 빈도 히스토그램을 출력합니다. 집계는 이러한 히스토그램 합계를 각 레이블에 대해 "확률" 가져올 정규화 하는 것입니다. 이렇게 하면 높은 예측 신뢰도 있는 트리 앙상블의 최종 결정에 더 높은 가중치가 부여를 해야 합니다.  
  
         자세한 내용은 부트스트랩 집계에 대 한 Wikipedia 항목을 참조 하세요.  
  
    -   **복제**: 복제에서는 각 트리는 정확히 동일한 입력된 데이터에 대해 학습 됩니다. 분할의 각 트리 노드에 대 한 조건자는 결정 임의 유지 하 고 트리는 다양 한 됩니다.   
  
3.  설정 하 여 학습 모델을 지정 합니다 **트레이너 모드 만들기** 옵션입니다.  
  
    -   **매개 변수를 단일**: 모델을 구성 하려는 방법를 알고 있으면 특정 값 집합을 인수로 제공할 수 있습니다.
  
4.  에 대 한 **의사 결정 트리의 수**, 의사 결정 트리 앙상블에서 만들 수 있는 최대 수를 입력 합니다. 추가 의사 결정 트리를 만들면 적용 범위가 확대를 잠재적으로 가져올 수 있습니다 있지만 학습 시간이 증가 합니다.  
  
    > [!NOTE]
    >  이 값에는 또한 학습된 된 모델을 시각화 하는 경우에 표시 되는 트리 수를 제어 합니다. 참조 또는 단일 트리를 인쇄 하려는 경우 값 1로 설정할 수 있습니다. 그러나 하나의 트리만 될 수 있습니다 (초기 매개 변수 집합을 사용 하 여 트리)를 생성 하 고 추가 반복은 수행 합니다.
  
5.  에 대 한 **의사 결정 트리의 최대 깊이**, 의사 결정 트리의 최대 깊이를 제한할 숫자를 입력 합니다. 트리의 수준을 늘리면 높아지지만 학습 시간이 더 길어지고 과잉 맞춤이 정밀도 높아질 수 있습니다.
  
6.  에 대 한 **수의 노드당 임의 분할**, 트리의 각 노드를 작성할 때 사용할 분할의 수를 입력 합니다. A *분할* (노드) 트리의 각 수준에서 기능 즉 임의로 구분 됩니다.
  
7.  에 대 한 **샘플 리프 노드당 최소**, 사례 트리의 터미널 노드 (리프)를 만드는 데 필요한 최소 수를 나타냅니다.
  
     이 값을 늘려 새 규칙 만들기에 대 한 임계값을 늘려야 합니다. 예를 들어, 단일 사례도 1의 기본값을 사용 하 여 새 규칙을 만드는 발생할 수 있습니다. 값을 5로 늘리면 학습 데이터에 동일한 조건을 만족 하는 사례가 다섯 개 이상 포함 해야 합니다.  
  
8.  선택 된 **범주 기능에 대 한 알 수 없는 값 허용** 교육 또는 유효성 검사 집합에서 알 수 없는 값에 대 한 그룹을 만드는 옵션을 합니다. 모델을 알려진된 값에 대해 덜 정확한 수 있지만 새로운 (알 수 없는) 값에 대 한 더 나은 예측을 제공할 수 있습니다. 

     이 옵션의 선택을 취소 하면 모델 학습 데이터에 포함 된 값만 수락할 수 있습니다.
  
9. 레이블이 지정 된 데이터 집합 및 중 하나를 연결 합니다 [학습 모듈](module-reference.md):  
  
    -   설정 하는 경우 **트레이너 모드 만들기** 를 **단일 매개 변수**를 사용 합니다 [모델 학습](./train-model.md) 모듈입니다.  
  
    
## <a name="results"></a>결과

완료 된 후 교육:

+ 각 반복에 대해 생성 된 트리를 보려면 출력을 마우스 오른쪽 단추로 클릭 합니다 [모델 학습](./train-model.md) 모듈과 선택 **시각화**합니다.
  
    각 트리에 분할으로 드릴 다운 하 여 각 노드에 대 한 규칙을 클릭 합니다.

+ 모델의 스냅숏을 저장 하려면 마우스 오른쪽 단추로 클릭 합니다 **학습 된 모델** 출력에서 선택한 **모델 저장**합니다. 저장된 된 모델은 실험의 후속 실행에서 업데이트 되지 않습니다.

+ 점수 매기기에 모델을 사용 하려면 추가 합니다 **모델 점수 매기기** 모듈을 실험 합니다.


## <a name="next-steps"></a>다음 단계

참조를 [사용할 수 있는 모듈 집합](module-reference.md) Azure Machine Learning 서비스입니다. 