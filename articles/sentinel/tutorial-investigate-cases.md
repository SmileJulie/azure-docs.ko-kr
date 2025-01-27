---
title: Azure Sentinel 미리 보기를 사용 하 여 사례를 조사 합니다. | Microsoft Docs
description: 이 자습서를 사용 하 여 Azure Sentinel이 있는 사례를 조사 하는 방법을 알아봅니다.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: a493cd67-dc70-4163-81b8-04a9bc0232ac
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/6/2019
ms.author: rkarlin
ms.openlocfilehash: 82fac23fc2d718aa908f6291241abaa2aedb8815
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67621180"
---
# <a name="tutorial-investigate-cases-with-azure-sentinel-preview"></a>자습서: Azure Sentinel 미리 보기를 사용 하 여 사례를 조사 합니다.

> [!IMPORTANT]
> Azure Sentinel은 현재 공개 미리 보기로 제공됩니다.
> 이 미리 보기 버전은 서비스 수준 계약 없이 제공되며 프로덕션 워크로드에는 사용하지 않는 것이 좋습니다. 특정 기능이 지원되지 않거나 기능이 제한될 수 있습니다. 자세한 내용은 [Microsoft Azure Preview에 대한 추가 사용 약관](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)을 참조하세요.

이 자습서를 사용 하면 Azure Sentinel 하 여 위협을 검색할 수 있습니다.

한 후 [데이터 원본 연결](quickstart-onboard.md) Azure Sentinel 하려는 문제가 의심 스러운 경우 알림을 받을 합니다. 이 작업을 수행할 수 있도록, 만든 Azure Sentinel 있습니다 고급 할당할 수 있는 사례를 생성 하는 경고 규칙 및 비정상 및 위협 환경에 긴밀 하 게 조사를 사용 하 여 있습니다. 

> [!div class="checklist"]
> * 사례 만들기
> * 사례 조사
> * 위협에 대응

## <a name="investigate-cases"></a>사례 조사

경우에는 경고가 여러 개 포함할 수 있습니다. 것이 특정 조사에 대 한 증명 하는 모든 관련 정보를 집계 합니다. 경우에는 정의 된 경고에 따라 만들어집니다 합니다 **Analytics** 페이지입니다. 심각도 및 상태 등의 경고와 관련 된 속성은 사례 수준에서 설정 됩니다. Azure Sentinel 어떤 종류의 위협에 대 한 원하는 및을 찾는 방법을 알고 있어야 하는 것이 있습니다를 후 사례를 조사 하 여 검색 된 위협 요소를 모니터링할 수 있습니다. 

1. 선택 **사례**합니다. 합니다 **사례** 페이지를 사용 하면 알고 있는 사례는 몇 개입니까, 얼마나 많은 횟수로 설정 하면를 열려면 **진행에서**, 얼마나 많은 닫힙니다. 각 사례에 대해이 발생 한 시간 및 대/소문자의 상태를 볼 수 있습니다. 첫 번째 처리를 결정 하려면 심각도 살펴봅니다. 에 **사례** 페이지에서 **경고** 사례에 관련 된 모든 경고를 보려면 탭 합니다. 이전에 해당 부분에서 볼 수 있습니다에 매핑되는 엔터티를 **엔터티** 탭 합니다.  예를 들어 상태 또는 심각도 별로 필요에 따라 사례를 필터링 할 수 있습니다. 살펴볼 때 합니다 **사례** 탭에 정의 된 검색 규칙에 따라 트리거되는 경고를 포함 하는 열린 경우 표시 됩니다 **Analytics**합니다. 위쪽 표시에 활성 케이스, 새 사례를 생성 하 고 진행률의 경우. 또한 심각도 별로 모든 사례에 대 한 개요를 볼 수 있습니다.

   ![경고 대시보드](./media/tutorial-investigate-cases/cases.png)

2. 조사를 시작 하려면 특정 한 경우 클릭 합니다. 오른쪽의 해당 심각도 관련 된 엔터티 수의 요약 (매핑을 기준)를 포함 하는 경우에 대 한 자세한 정보를 볼 수 있습니다. 각각의 경우에는 고유 ID가 있습니다. 사례 심각도 경우에서 포함 된 가장 심각한 경고에 따라 결정 됩니다.  

1. 에서는 경고 및 엔터티에 대 한 자세한 정보를 보려면 클릭 **전체 세부 정보를 보려면** 에서는 페이지 및 사례 정보를 요약 하는 관련 탭을 검토 합니다.  전체 사례 뷰의 모든 증명 경고, 관련된 경고 및 엔터티를 통합합니다.

1. 에 **경고** 탭를 트리거 했을 때와 얼마나 설정한 임계값을 초과 하 여 자체 경고를 검토 합니다. 쿼리, 쿼리 및 경고에서 플레이 북을 실행 하는 기능 당 반환 되는 결과 수 경고를 트리거한 경고에 대 한 모든 관련 정보를 볼 수 있습니다. 드릴 다운 더 많은 경우에 클릭할 적중 횟수입니다. 이 Log Analytics에서 경고를 트리거한 결과 및 결과 생성 하는 쿼리를 엽니다.

3. 에 **엔터티** 탭에서 경고 규칙 정의의 일부로 매핑된 모든 엔터티를 볼 수 있습니다. 

4. 사례 상태를 설정 하는 것이 좋습니다 사례를 적극적으로 조사 하는 경우 것 **진행에서** 를 닫을 때까지 합니다. 경우 닫을 수도 있습니다 위치 **확인할 닫은** 처리 되는 인시던트를 나타내는 경우에 대 한 상태가 하는 동안 **해제 된 닫힌** 처리 필요 하지 않은 경우 상태는 합니다. 설명 사례를 닫기 위한에 추론을 설명 하는 필수 이며

5. 경우 특정 사용자에 게 할당할 수 있습니다. 각 사례에 대해 대/소문자를 설정 하 여 소유자를 할당할 수 있습니다 **소유자** 필드입니다. 으로 지정 하지 않은 모든 경우 시작 합니다. 사례로 이동 하 고 사용자가 소유한 모든 사례를 이름으로 필터링 수 있습니다. 

5. 클릭 **조사** 조사 맵 및 수정 단계를 사용 하 여 위반 범위에 볼 수 있습니다. 



## <a name="respond-to-threats"></a>위협에 대응

Azure Sentinel 플레이 북을 사용 하 여 위협에 대응 하기 위한 두 가지 주요 옵션을 제공 합니다. 경고에 대 한 응답에서을 플레이 북을 수동으로 실행할 수 있습니다 하거나 경고가 트리거될 때 자동으로 실행 되도록 플레이 북을 설정할 수 있습니다.

- 플레이 북을 구성한 경우 경고가 트리거될 때 자동으로 실행 되도록 플레이 북을 설정할 수 있습니다. 

- 클릭 하 여 경고를 내부에서 플레이 북을 수동으로 실행할 수 있습니다 **플레이 북 보기** 을 선택한 다음 플레이 북을 실행 합니다.




## <a name="next-steps"></a>다음 단계
이 자습서에서는 Azure Sentinel를 사용 하 여 사례를 조사를 시작 하는 방법을 알아보았습니다. 에 대 한 자습서를 계속 진행 [자동화 된 플레이 북을 사용 하 여 위협에 대응할 방법을](tutorial-respond-threats-playbook.md)합니다.
> [!div class="nextstepaction"]
> [위협에 대처](tutorial-respond-threats-playbook.md) 위협에 대 한 사용자 응답을 자동화할 수 있습니다.

