---
title: Azure Monitor를 사용하여 메트릭 경고 만들기, 보기 및 관리
description: Azure Portal 또는 CLI를 사용하여 메트릭 경고 규칙을 만들고, 보고, 관리하는 방법을 알아봅니다.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: snmuvva
ms.component: alerts
ms.openlocfilehash: 479fd902ef68bf24306bcd699da0be08415592a0
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956992"
---
# <a name="create-view-and-manage-metric-alerts-using-azure-monitor"></a>Azure Monitor를 사용하여 메트릭 경고 만들기, 보기 및 관리

Azure Monitor의 메트릭 경고를 사용하면 메트릭 중 하나가 임계값을 초과할 경우 알림을 받을 수 있습니다. 메트릭 경고는 다양한 다차원 플랫폼 메트릭, 사용자 지정 메트릭, Application Insights 표준 및 사용자 지정 메트릭에서 작동합니다. 이 문서에서는 Azure Portal 및 Azure CLI를 통해 메트릭 경고 규칙을 만들고, 보고, 관리하는 방법을 설명합니다. 또한 Azure Resource Manager 템플릿을 사용하여 메트릭 경고 규칙을 만들 수 있으며, 이 방법은 [별도의 문서](monitoring-enable-alerts-using-template.md)에 설명되어 있습니다.

메트릭 경고의 작동 원리는 [메트릭 경고 개요](alert-metric-overview.md)에서 자세히 알아볼 수 있습니다.

## <a name="create-a-metric-alert-rule-using-azure-portal"></a>Azure Portal을 사용하여 메트릭 경고 규칙 만들기

다음은 Azure Portal에서 메트릭 경고 규칙을 만드는 절차입니다.

1. [Azure Portal](https://portal.azure.com)에서 **모니터**를 클릭합니다. [모니터] 블레이드는 모든 모니터링 설정 및 데이터를 하나의 뷰에 통합합니다.

2. **경고**를 클릭한 다음, **+ 새로운 경고 규칙**을 클릭합니다.

    > [!TIP]
    > 대부분의 리소스 블레이드도 **모니터링** 아래의 리소스 메뉴에 **경고**가 있으며, 여기서 경고를 만들 수 있습니다.

3. **대상 선택**을 클릭하고, 로드되는 컨텍스트 창에서 수정하려는 대상 리소스를 선택합니다. **구독** 및 **리소스 종류** 드롭다운을 사용하여 모니터링하려는 리소스를 찾습니다. 검색 창을 사용하여 리소스를 검색할 수도 있습니다.

4. 선택한 리소스에 경고를 만들 수 있는 메트릭이 있으면 오른쪽 아래의 **사용 가능한 신호**에 메트릭이 포함됩니다. 이 [문서](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported)의 메트릭 경고에 지원되는 리소스 종류의 전체 목록을 볼 수 있습니다.

5. 대상 리소스를 선택한 후에는 **조건 추가**를 클릭합니다.

6. 리소스에 지원되는 신호 목록이 표시되면 경고를 만들 메트릭을 선택합니다.

7. 지난 6시간의 메트릭에 대한 차트가 표시됩니다. **기간**, **빈도**, **연산자** 및 **임계값**을 정의합니다. 이렇게 하면 메트릭 경고 규칙에서 평가할 논리가 결정됩니다.

8. 메트릭 차트를 사용하여 적절한 임계값을 확인할 수 있습니다.

9. 필요에 따라 메트릭에 차원이 포함된 경우 차원 테이블이 표시됩니다. 차원당 하나 이상의 값을 선택하세요. 메트릭 경고는 선택된 모든 값 조합에 대한 조건 평가를 실행합니다. [다차원 메트릭에 대한 경고의 작동 방식에 대해 자세히 알아보세요](alert-metric-overview.md). 차원에 대해 **\*를 선택**할 수도 있습니다. **\*를 선택**하면 선택 항목이 동적으로 차원의 모든 현재 및 미래 값으로 조정됩니다.

10. **완료**를 클릭합니다.

11. 필요에 따라 복잡한 경고 규칙을 모니터링하려면 다른 조건을 추가합니다.

12. **경고 규칙 이름**, **설명**, **심각도** 등의 **경고 세부 정보**를 입력합니다.

13. 기존 작업 그룹을 선택하거나 새 작업 그룹을 만들어서 경고에 작업 그룹을 추가합니다.

14. **완료**를 클릭하여 메트릭 경고 규칙을 저장합니다.

> [!NOTE]
> 포털을 통해 생성되는 메트릭 경고는 대상 리소스와 동일한 리소스 그룹에 생성됩니다.

## <a name="view-and-manage-metric-alert-rules-using-azure-portal"></a>Azure Portal을 사용하여 메트릭 경고 규칙을 살펴보고 관리

[경고] 아래에서 [규칙 관리] 블레이드를 사용하여 메트릭 경고 규칙을 살펴보고 관리할 수 있습니다. 아래 절차는 메트릭 경고 규칙을 살펴보고 편집하는 방법을 보여줍니다.

1. Azure Portal에서 **모니터**로 이동합니다.

2. **경고**를 클릭하고 **규칙 관리**를 클릭합니다.

3. **규칙 관리** 블레이드에서 구독의 모든 경고 규칙을 볼 수 있습니다. **리소스 그룹**, **리소스 종류**, **리소스**를 사용하여 규칙을 추가로 필터링 할 수 있습니다. 메트릭 경고만 보려면 **신호 유형**을 메트릭으로 선택합니다.

    > [!TIP]
    > **규칙 관리** 블레이드에서 여러 경고 규칙을 선택하고 활성화/비활성화할 수 있습니다. 이 기능은 특정 대상 리소스를 유지 관리해야 하는 경우에 유용하게 사용할 수 있습니다.

4. 편집하려는 메트릭 경고 규칙의 이름을 클릭합니다.

5. [규칙 편집]에서 편집하려는 **경고 조건**을 클릭합니다. 메트릭, 임계값 및 기타 필드를 필요한 대로 변경할 수 있습니다.

    > [!NOTE]
    > 메트릭 경고가 만들어진 후에는 **대상 리소스** 및 **경고 규칙 이름**을 편집할 수 없습니다.

6. **완료**를 클릭하여 편집 내용을 저장합니다.

## <a name="create-view-and-manage-metric-alert-rules-using-azure-cli"></a>Azure CLI를 사용하여 메트릭 경고 규칙을 만들고 살펴보기

이전 섹션에서는 Azure Portal을 사용하여 메트릭 경고 규칙을 만들고, 보고, 관리하는 방법을 설명했습니다. 이번 섹션에서는 플랫폼 간 [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)를 사용하여 동일한 작업을 수행하는 방법을 설명합니다. Azure CLI를 가장 빠르게 사용하는 방법은 [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview?view=azure-cli-latest)입니다. 이 문서에서는 Cloud shell을 사용하겠습니다.

1. Azure Portal로 이동하여 **Cloud shell**을 클릭합니다.

2. 프롬프트에서 ``--help`` 옵션이 있는 명령을 사용하여 명령 및 사용 방법에 대해 자세히 알아볼 수 있습니다. 예를 들어 다음 명령은 메트릭 경고를 만들고, 살펴보고, 관리하는 데 사용할 수 있는 명령 목록을 보여줍니다.

    ```azurecli
    az monitor metrics alert --help
    ```

3. VM의 평균 백분율 CPU가 70을 초과하는지 모니터링하는 간단한 메트릭 경고 규칙을 만들 수 있습니다.

    ```azurecli
    az monitor metrics alert create -n {nameofthealert} -g {ResourceGroup} --scopes {VirtualMachineResourceID} --condition "avg Percentage CPU > 90"
    ```

4. 다음 명령을 사용하여 리소스 그룹의 모든 메트릭 경고를 볼 수 있습니다.

    ```azurecli
    az monitor metrics alert list  -g {ResourceGroup}
    ```

5. 규칙의 이름 또는 리소스 ID를 사용하여 특정 메트릭 경고 규칙의 세부 정보를 볼 수 있습니다.

    ```azurecli
    az monitor metrics alert show -g {ResourceGroup} -n {AlertRuleName}
    ```

    ```azurecli
    az monitor metrics alert show --ids {RuleResourceId}
    ```

6. 다음 명령을 사용하여 메트릭 경고 규칙을 비활성화할 수 있습니다.

    ```azurecli
    az monitor metrics alert update -g {ResourceGroup} -n {AlertRuleName} -enabled false
    ```

7. 다음 명령을 사용하여 메트릭 경고 규칙을 삭제할 수 있습니다.

    ```azurecli
    az monitor metrics alert update -g {ResourceGroup} -n {AlertRuleName} -enabled false
    ```

## <a name="next-steps"></a>다음 단계

- [Azure Resource Manager 템플릿을 사용하여 메트릭 경고를 만듭니다](monitoring-enable-alerts-using-template.md).
- [메트릭 경고의 작동 원리를 이해합니다](alert-metric-overview.md).
- [메트릭 경고에 대한 웹후크 스키마를 이해합니다](monitoring-near-real-time-metric-alerts.md#payload-schema).