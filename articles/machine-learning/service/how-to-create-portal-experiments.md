---
title: 포털에서 실험 만들기 및 탐색
titleSuffix: Azure Machine Learning service
description: 포털에서 자동화 된 machine learning 실험을 만들고 관리 하는 방법을 알아봅니다.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: cgronlun
author: tsikiksr
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 05/02/2019
ms.openlocfilehash: 5eb3e94ff65e8a8b74f357a4cb8a517fd3837c5a
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/15/2019
ms.locfileid: "67871822"
---
# <a name="create-and-explore-automated-machine-learning-experiments-in-the-azure-portal-preview"></a>Azure Portal에서 자동화 된 machine learning 실험 만들기 및 탐색 (미리 보기)

 이 문서에서는 코드 한 줄 없이 Azure Portal에서 자동화 된 machine learning 실험을 만들고 실행 하 고 탐색 하는 방법에 대해 알아봅니다. 자동화 된 machine learning은 특정 데이터에 사용할 가장 적합 한 알고리즘을 선택 하는 프로세스를 자동화 하므로 기계 학습 모델을 신속 하 게 생성할 수 있습니다. [자동화 된 machine learning에 대해 자세히 알아보세요](concept-automated-ml.md).

 더 많은 코드 기반 환경을 선호 하는 경우 [AZURE MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)를 사용 하 여 [Python에서 자동화 된 machine learning 실험을 구성할](how-to-configure-auto-train.md) 수도 있습니다.

## <a name="prerequisites"></a>필수 구성 요소

* Azure 구독. Azure 구독이 없는 경우 시작하기 전에 체험 계정을 만듭니다. [Azure Machine Learning Service의 평가판 또는 유료 버전](https://aka.ms/AMLFree)을 지금 사용해 보세요.

* Azure Machine Learning 서비스 작업 영역. [Azure Machine Learning 서비스 작업 영역 만들기를](https://docs.microsoft.com/azure/machine-learning/service/setup-create-workspace)참조 하세요.

## <a name="get-started"></a>시작

작업 영역의 왼쪽 창으로 이동 합니다. 제작 (미리 보기) 섹션에서 자동화 된 Machine Learning를 선택 합니다.

![Azure Portal 탐색 창](media/how-to-create-portal-experiments/nav-pane.png)

 자동 Machine Learning를 사용 하 여 처음으로 실험을 수행 하는 경우 다음이 표시 됩니다.

![Azure Portal 실험 방문 페이지](media/how-to-create-portal-experiments/landing-page.png)

그렇지 않으면 자동화 된 machine learning 대시보드가 SDK를 사용 하 여 만든 실험을 포함 하 여 자동화 된 모든 기계 학습 실험의 개요와 함께 표시 됩니다. 여기에서 날짜, 실험 이름 및 실행 상태별로 실행을 필터링 하 고 탐색할 수 있습니다.

![Azure Portal 실험 대시보드](media/how-to-create-portal-experiments/dashboard.png)

## <a name="create-an-experiment"></a>실험 만들기

실험 만들기 단추를 선택 하 여 다음 폼을 채웁니다.

![실험 폼 만들기](media/how-to-create-portal-experiments/create-exp-name-compute.png)

1. 실험 이름을 입력 합니다.

1. 데이터 프로 파일링 및 학습 작업에 대 한 계산을 선택 합니다. 기존 계산 목록은 드롭다운에서 사용할 수 있습니다. 새 계산을 만들려면 3 단계의 지침을 따르세요.

1. 새 계산 만들기 단추를 선택 하 여 아래 창을 열고이 실험의 계산 컨텍스트를 구성 합니다.

    ![실험에 대 한 새 계산 만들기](media/how-to-create-portal-experiments/create-new-compute.png)

    필드|설명
    ---|---
    계산 이름| 계산 컨텍스트를 식별 하는 고유한 이름을 입력 합니다.
    가상 머신 크기| 계산에 대 한 가상 머신 크기를 선택 합니다.
    추가 설정| *최소 노드*: 계산의 최소 노드 수를 입력 합니다. AML 계산에 대 한 최소 노드 수는 0입니다. 데이터 프로 파일링을 사용 하도록 설정 하려면 하나 이상의 노드가 있어야 합니다. <br> *최대 노드*: 계산의 최대 노드 수를 입력 합니다. 기본값은 AML 계산에 대 한 6 노드입니다.

      새 계산 만들기를 시작 하려면 **만들기**를 선택 합니다. 몇 분 정도 걸릴 수 있습니다.

      >[!NOTE]
      > 계산 이름은 선택/만들기 계산을 *프로 파일링 할 수*있는지 여부를 표시 합니다. 데이터 프로 파일링에 대 한 자세한 내용은 7b를 참조 하십시오.

1. 데이터에 대 한 저장소 계정을 선택 합니다. 공개 미리 보기는 로컬 파일 업로드 및 Azure Blob Storage 계정만 지원 합니다.

1. 저장소 컨테이너를 선택 합니다.

1. 저장소 컨테이너에서 데이터 파일을 선택 하거나 로컬 컴퓨터의 파일을 컨테이너에 업로드 합니다.

    ![실험을 위해 데이터 파일 선택](media/how-to-create-portal-experiments/select-file.png)

1. 미리 보기 및 프로필 탭을 사용 하 여이 실험을 위한 데이터를 추가로 구성 합니다.

    1. 미리 보기 탭에서 데이터에 머리글이 포함 되어 있는지 여부를 지정 하 고 각 기능 열의 **포함 된** 스위치 단추를 사용 하 여 학습을 위한 기능 (열)을 선택 합니다.

        ![데이터 미리 보기](media/how-to-create-portal-experiments/data-preview.png)

    1. 프로필 탭에서 기능별로 [데이터 프로필](#profile) 을 볼 수 있을 뿐만 아니라 각각의 분포, 유형 및 요약 통계 (평균, 중앙값, 최대값/최소값 등)를 볼 수 있습니다.

        ![데이터 프로필 탭](media/how-to-create-portal-experiments/data-profile.png)

        >[!NOTE]
        > 계산 컨텍스트가 프로 파일링을 사용 하도록 설정 되어 **있지** 않으면 다음 오류 메시지가 표시 됩니다. *데이터 프로 파일링은 이미 실행 중인 계산 대상에 대해서만 사용할 수*있습니다.

1. 학습 작업 유형 (분류, 회귀 또는 예측)을 선택 합니다.

1. 대상 열을 선택 합니다. 예측을 수행할 열입니다.

1. 예측:
    1. 시간 열 선택: 이 열은 사용할 시간 데이터를 포함 합니다.
    1. 예측 구간을 선택 합니다. 모델에서 미래를 예측할 수 있는 시간 단위 (분/시간/일/주/월/연도)를 표시 합니다. 나중에 예측 하는 데 더 많은 모델이 필요 하며,이는 더 정확 하지 않게 됩니다. [예측 및 예측 마감일에 대해 자세히 알아보세요](how-to-auto-train-forecast.md).

1. 필드 고급 설정: 학습 작업을 보다 효율적으로 제어 하는 데 사용할 수 있는 추가 설정입니다.

    고급 설정|Description
    ------|------
    기본 메트릭| 모델의 점수를 매기는 데 사용 되는 기본 메트릭입니다. [모델 메트릭에 대해 자세히 알아보세요](how-to-configure-auto-train.md#explore-model-metrics).
    종료 조건| 이러한 조건 중 하나라도 충족 되 면 학습 작업은 전체 완료 전에 종료 됩니다. <br> *학습 작업 시간 (분)* : 학습 작업을 실행할 수 있는 기간입니다.  <br> *최대 반복 횟수*: 학습 작업에서 테스트할 최대 파이프라인 (반복) 수입니다. 작업은 지정 된 반복 횟수를 초과 하 여 실행 되지 않습니다. <br> *메트릭 점수 임계값*:  모든 파이프라인에 대 한 최소 메트릭 점수입니다. 이렇게 하면 정의 된 대상 메트릭이 있는 경우 필요한 것 보다 학습 작업에 더 많은 시간이 걸리지 않습니다.
    바꾸면| 자동화 된 machine learning에서 수행 하는 전처리를 사용 하거나 사용 하지 않도록 설정 하려면 선택 합니다. 전처리는 자동 데이터 정리, 준비 및 변환을 포함 하 여 가상 기능을 생성 합니다. [전처리에 대해 자세히 알아보세요](#preprocess).
    유효성 검사| 학습 작업에서 사용할 교차 유효성 검사 옵션 중 하나를 선택 합니다. [교차 유효성 검사에 대해 자세히 알아보세요](how-to-configure-auto-train.md).
    동시성| 다중 코어 계산을 사용할 때 사용할 다중 코어 제한을 선택 합니다.
    차단 된 알고리즘| 학습 작업에서 제외 하려는 알고리즘을 선택 합니다.

   ![고급 설정 양식](media/how-to-create-portal-experiments/advanced-settings.png)

> [!NOTE]
> 필드에 대 한 자세한 내용을 보려면 정보 도구 설명을 클릭 하십시오.

<a name="profile"></a>

### <a name="data-profiling"></a>데이터 프로 파일링

데이터 집합에 대 한 다양 한 요약 통계를 가져와서 데이터 집합이 ML 준비 상태 인지 여부를 확인할 수 있습니다. 숫자가 아닌 열의 경우 min, max 및 error count와 같은 기본 통계만 포함 합니다. 숫자 열의 경우 통계 및 예상 변 위치을 검토할 수도 있습니다. 특히 데이터 프로필에는 다음이 포함 됩니다.

* **기능**: 요약 되는 열의 이름입니다.

* **프로필**: 유추 된 형식을 기반으로 하는 인라인 시각화입니다. 예를 들어 문자열, 부울 및 날짜에는 값이 포함 되 고, decimals (숫자)에는 근사 히스토그램이 있습니다. 이를 통해 데이터 배포를 빠르게 파악할 수 있습니다.

* **형식 분포**: 열 내에서 형식의 인라인 값 수입니다. Null은 고유 형식 이므로이 시각화는 홀수 또는 누락 된 값을 검색 하는 데 유용 합니다.

* **형식**: 열의 유추 된 유형입니다. 가능한 값에는 문자열, 부울, 날짜 및 소수 자릿수가 있습니다.

* **Min**: 열의 최소값입니다. 형식에 고유 순서 (예: 부울)가 없는 기능에 대 한 빈 항목이 표시 됩니다.

* **Max**: 열의 최대값입니다. "Min"과 같이 관련이 없는 형식의 기능에 대해 빈 항목이 표시 됩니다.

* **개수**: 열에서 누락 되거나 누락 된 항목의 총 수입니다.

* **누락 된 개수**: 열에서 누락 된 항목의 수입니다. 빈 문자열 및 오류는 값으로 처리 되므로 "누락 된 개수"에 영향을 주지 않습니다.

* **변 위치** (0.1, 1, 5, 25, 50, 75, 95, 99 및 99.9% 간격): 각 변 위치의 근사 값은 데이터 분포를 제공 합니다. 관련이 없는 형식의 기능에 대해 빈 항목이 표시 됩니다.

* **Mean**: 열의 산술 평균입니다. 관련이 없는 형식의 기능에 대해 빈 항목이 표시 됩니다.

* **표준 편차**: 열의 표준 편차입니다. 관련이 없는 형식의 기능에 대해 빈 항목이 표시 됩니다.

* **Variance**: 열의 분산입니다. 관련이 없는 형식의 기능에 대해 빈 항목이 표시 됩니다.

* **왜곡도**: 열의 왜곡도입니다. 관련이 없는 형식의 기능에 대해 빈 항목이 표시 됩니다.

* **첨도**: 열의 첨도입니다. 관련이 없는 형식의 기능에 대해 빈 항목이 표시 됩니다.

<a name="preprocess"></a>

### <a name="advanced-preprocessing"></a>고급 전처리

실험을 구성할 때 고급 설정을 `Preprocess`사용 하도록 설정할 수 있습니다. 이렇게 하면 다음 데이터 전처리 및 기능화 단계가 자동으로 수행 됩니다.

|전처리&nbsp;단계| Description |
| ------------- | ------------- |
|높은 카디널리티 또는 분산 없는 기능 삭제|모든 값이 누락 된 기능을 포함 하 여 학습 및 유효성 검사 집합에서이를 삭제 하거나, 모든 행에서 동일한 값을 사용 하거나, 해시, Id 또는 Guid와 같은 매우 높은 카디널리티를 사용 합니다.|
|누락 값 입력|숫자 기능의 경우 열에 있는 값의 평균을 돌립니다 합니다.<br/><br/>범주 기능의 경우 가장 자주 사용 되는 값을 돌립니다 합니다.|
|추가 기능 생성|DateTime 기능: 연도, 월, 일, 요일, 연간 일자, 분기, 연간 주, 시간, 분, 초<br/><br/>텍스트 기능: 전체 그램, 양방향 그램 및 세 문자 그램을 기반으로 하는 용어 빈도입니다.|
|변환 및 인코딩 |고유 값이 거의 없는 숫자 기능은 범주 기능으로 변환 됩니다.<br/><br/>낮은 카디널리티 범주에 대해 원 핫 인코딩을 수행 합니다. 높은 카디널리티의 경우 1 핫 해시 인코딩입니다.|
|단어 포함|미리 학습 된 모델을 사용 하 여 텍스트 토큰의 벡터를 문장 벡터로 변환 하는 텍스트 featurizer. 문서에서 각 단어를 포함 하는 벡터는 문서 기능 벡터를 생성 하기 위해 함께 집계 됩니다.|
|대상 인코딩|범주 기능의 경우는 회귀 문제에 대 한 평균 목표 값과 각 범주를 분류 문제에 대 한 각 클래스의 확률에 매핑합니다. 빈도 기반 가중치 및 k-접기 교차 유효성 검사를 적용 하 여 스파스 데이터 범주로 인 한 매핑 및 노이즈를 줄입니다.|
|텍스트 대상 인코딩|텍스트 입력의 경우 단어 모음이 포함 된 누적 선형 모델을 사용 하 여 각 클래스의 확률을 생성 합니다.|
|증명 정보 가중치 (WoE)|대상 열에 대 한 범주 열의 상관 관계를 측정 하는 WoE 계산 합니다. 이는 클래스 내 및 클래스 외부 확률의 비율로 계산 됩니다. 이 단계에서는 클래스 당 하나의 숫자 기능 열을 출력 하 고 누락 된 값 및 이상 값을 명시적으로 돌립니다 필요가 없습니다.|
|클러스터 거리|K를 트레인 하 여 모든 숫자 열에 대 한 클러스터링 모델을 의미 합니다.  각 클러스터의 중심에 대 한 각 샘플의 거리를 포함 하는 새 기능 (클러스터당 새로운 숫자 기능)을 출력 합니다.|

## <a name="run-experiment-and-view-results"></a>실험 실행 및 결과 보기

실험을 실행 하려면 시작을 클릭 합니다. 실험 준비 프로세스는 몇 분 정도 걸립니다.

### <a name="view-experiment-details"></a>실험 세부 정보 보기

실험 준비 단계가 완료 되 면 세부 정보 화면 실행이 표시 됩니다. 그러면 생성 된 모델의 전체 목록이 제공 됩니다. 기본적으로 매개 변수를 기반으로 가장 높은 점수를 나타내는 모델은 목록의 맨 위에 있습니다. 학습 작업은 더 많은 모델을 시도 하므로 반복 목록 및 차트에 추가 됩니다. 반복 차트를 사용 하 여 지금까지 생성 된 모델에 대 한 메트릭을 빠르게 비교할 수 있습니다.

학습 작업은 각 파이프라인이 실행을 완료 하는 데 다소 시간이 걸릴 수 있습니다.

![실행 세부 정보 대시보드](media/how-to-create-portal-experiments/run-details.png)

### <a name="view-training-run-details"></a>학습 실행 세부 정보 보기

성능 메트릭 및 배포 차트와 같은 학습 실행 세부 정보를 보려면 출력 모델을 드릴 다운 합니다. [차트에 대해 자세히 알아보세요](how-to-track-experiments.md#understanding-automated-ml-charts).

![반복 세부 정보](media/how-to-create-portal-experiments/iteration-details.png)

## <a name="deploy-model"></a>모델 배포

최적의 모델을 만든 후에는이를 웹 서비스로 배포 하 여 새 데이터를 예측할 수 있습니다.

자동화 된 ML을 사용 하면 코드를 작성 하지 않고도 모델을 배포할 수 있습니다.

1. 몇 가지 배포 옵션을 사용할 수 있습니다. 
    1. 실험에 대해 설정한 메트릭 조건에 따라 최상의 모델을 배포 하려면 **실행 세부 정보** 페이지에서 **최적 모델 배포** 를 선택 합니다.

        ![모델 배포 단추](media/how-to-create-portal-experiments/deploy-model-button.png)

    1. 특정 모델 반복을 배포 하려면 모델에서 드릴 다운 하 여 특정 실행 세부 정보 페이지를 열고 **모델 배포**를 선택 합니다.

        ![모델 배포 단추](media/how-to-create-portal-experiments/deploy-model-button2.png)

1. 첫 번째 단계는 서비스에 모델을 등록 하는 것입니다. "모델 등록"을 선택 하 고 등록 프로세스가 완료 될 때까지 기다립니다.

    ![모델 배포 블레이드](media/how-to-create-portal-experiments/deploy-model-blade.png)

1. 모델을 등록 하면 배포 중에 사용할 점수 매기기 스크립트 (scoring.py) 및 환경 스크립트 (condaEnv)를 다운로드할 수 있습니다.

1. 점수 매기기 스크립트와 환경 스크립트가 다운로드 되 면 왼쪽 탐색 창의 **자산** 블레이드로 이동 하 여 **모델**을 선택 합니다.

    ![탐색 창 모델](media/how-to-create-portal-experiments/nav-pane-models.png)

1. 등록 한 모델을 선택 하 고 "이미지 만들기"를 선택 합니다.

    다음 형식으로 실행 ID, 반복 번호를 포함 하는 설명으로 모델을 식별할 수 있습니다. *< Run_ID > _ < Iteration_number > _model*

    ![모델인 이미지 만들기](media/how-to-create-portal-experiments/model-create-image.png)

1. 이미지의 이름을 입력 합니다. 
1. "점수 매기기 파일" 상자 옆에 있는 **찾아보기** 단추를 선택 하 여 이전에 다운로드 한 점수 매기기 파일 (scoring.py)을 업로드 합니다.

1. "Conda 파일" 상자 옆에 있는 **찾아보기** 단추를 선택 하 여 이전에 다운로드 한 환경 파일 (condaEnv)을 업로드 합니다.

    사용자 고유의 점수 매기기 스크립트 및 conda 파일을 사용 하 고 추가 파일을 업로드할 수 있습니다. [점수 매기기 스크립트에 대해 자세히 알아보세요](how-to-deploy-and-where.md#script).

      >[!Important]
      > 파일 이름은 32 자 미만 이어야 하며 영숫자를 사용 하 여 시작 하 고 끝나야 합니다. 에는 대시, 밑줄, 점 및 영숫자 ()가 포함 될 수 있습니다. 공백은 허용 되지 않습니다.

    ![이미지 만들기](media/how-to-create-portal-experiments/create-image.png)

1. "만들기" 단추를 선택 하 여 이미지 만들기를 시작 합니다. 이 작업을 완료 하는 데 몇 분 정도 걸릴 수 있습니다. 완료 되 면 위쪽 표시줄에 메시지가 표시 됩니다.
1. "이미지" 탭으로 이동 하 여 배포 하려는 이미지 옆의 확인란을 선택 하 고 "배포 만들기"를 선택 합니다. [배포에](how-to-deploy-and-where.md)대해 자세히 알아보세요.

    배포에는 두 가지 옵션이 있습니다.
     + ACI (Azure Container Instance)-대규모로 운영 배포가 아니라 테스트 목적으로 사용 됩니다. _CPU 예약 용량의_코어 하나 이상에 대 한 값을 입력 하 고 _메모리 예약 용량의_ 기가바이트 (gb)를 하나 이상 입력 해야 합니다.
     + AKS (Azure Kubernetes Service))-이 옵션은 대규모 배포를 위한 것입니다. AKS 기반 계산을 준비 해야 합니다.

     ![이미지 배포 만들기](media/how-to-create-portal-experiments/images-create-deployment.png)

1. 완료되면 **만들기**를 선택합니다. 각 파이프라인의 실행이 완료 될 때까지 모델을 배포 하는 데 몇 분 정도 걸릴 수 있습니다.

1. 이제 끝났습니다! 예측을 생성 하는 작업 웹 서비스가 있습니다.

## <a name="next-steps"></a>다음 단계

* [자동화 된 machine learning 및 Azure Machine Learning에 대해 자세히 알아보세요](concept-automated-ml.md) .
* [웹 서비스를 사용 하는 방법을 알아봅니다](https://docs.microsoft.com/azure/machine-learning/service/how-to-consume-web-service).
