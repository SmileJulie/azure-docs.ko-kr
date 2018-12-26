---
title: Azure Log Analytics 경고로 이벤트에 응답 | Microsoft Docs
description: 이 자습서는 Log Analytics의 경고를 통해 작업 영역의 중요한 정보를 식별하며 문제를 미리 알리거나 작업을 호출하여 문제 해결을 시도합니다.
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 10/05/2018
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 6521688e595230951e0753fd67c2bf9b02e0a6ec
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53102149"
---
# <a name="respond-to-events-with-azure-monitor-alerts"></a>Azure Monitor 경고로 이벤트에 응답
Azure Monitor의 경고는 Log Analytics 리포지토리에서 중요한 정보를 식별할 수 있습니다. 경고는 정기적으로 로그 검색을 자동으로 실행하는 경고 규칙에 의해 작성되며 로그 검색 결과가 특정 기준과 일치하면 경고 레코드가 만들어지며 자동 응답을 수행하도록 구성할 수 있습니다.  이 자습서는 [Log Analytics 데이터의 대시보드 만들기 및 공유](tutorial-logs-dashboards.md) 자습서와 이어집니다.   

이 자습서에서는 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * 경고 규칙 만들기
> * 이메일 알림을 보내도록 작업 그룹 구성

이 자습서의 예제를 완료하려면 [Log Analytics 작업 영역에 연결된](../../azure-monitor/learn/quick-collect-azurevm.md) 기존 가상 머신이 있어야 합니다.  

## <a name="sign-in-to-azure-portal"></a>Azure Portal에 로그인
[https://portal.azure.com](https://portal.azure.com)에서 Azure Portal에 로그인합니다. 

## <a name="create-alerts"></a>경고 만들기
경고는 Azure Monitor에서 경고 규칙에 의해 생성되고 정기적으로 저장된 쿼리 또는 사용자 지정 로그 검색을 자동으로 실행할 수 있습니다.  특정 성능 메트릭을 기반으로 알림을 만들거나 특정 이벤트가 생성되거나 이벤트가 없거나 특정 기간 내에 여러 이벤트가 만들어질 때 알림을 만들 수 있습니다.  예를 들어, 경고는 평균 CPU 사용량이 특정 임계값을 초과하고, 누락된 업데이트가 발견되거나 특정 Windows 서비스 또는 Linux 데몬이 실행 중이 아님을 발견한 상태에서 이벤트가 생성될 때 이를 알리는 데 사용할 수 있습니다.  로그 검색 결과가 특정 조건과 일치하는 경우 경고가 만들어집니다. 그런 다음, 규칙은 경고를 알리거나 다른 프로세스를 호출하는 것과 같은 하나 이상의 작업을 자동으로 실행할 수 있습니다. 

다음 예제에서는 [시각화 데이터 자습서](tutorial-logs-dashboards.md)에 저장된 *Azure VM - 프로세서 사용률* 쿼리에 기반을 둔 메트릭 측정 경고 규칙을 만듭니다.  90%의 임계값을 초과하는 각 가상 머신에 대한 경고가 생성됩니다.  

1. Azure Portal에서 **모든 서비스**를 클릭합니다. 리소스 목록에서 **Log Analytics**를 입력합니다. 입력을 시작하면 입력한 내용을 바탕으로 목록이 필터링됩니다. **Log Analytics**를 선택합니다.
2. 왼쪽 창에서 **경고**를 선택한 다음, 페이지의 위쪽에서 **새 경고 규칙**을 클릭하여 새 경고를 만듭니다.<br><br> ![새 경고 규칙 만들기](./media/tutorial-response/alert-rule-02.png)<br>
3. 첫 번째 단계로 **경고 만들기** 섹션 아래에서 리소스로 Log Analytics 작업 영역을 선택하려고 합니다. 이는 로그 기반 경고 신호이기 때문입니다.  VM 및 이전에 만든 Log Analytics 작업 영역을 포함하는 하나 이상이 있는 경우 드롭다운 목록에서 특정 **구독**을 선택하여 결과를 필터링합니다.  드롭다운 목록에서 **Log Analytics**를 선택하여 **리소스 종류**를 필터링합니다.  마지막으로 **리소스** **DefaultLAWorkspace**를 선택한 다음, **완료**를 클릭합니다.<br><br> ![경고 만들기 1단계 작업](./media/tutorial-response/alert-rule-03.png)<br>
4. **경고 기준** 섹션 아래에서 **기준 추가**를 클릭하여 저장된 쿼리를 선택한 다음, 경고 규칙이 따르는 논리를 지정합니다.  **신호 논리 구성** 창의 목록에서 *Azure VM - 프로세서 사용률*을 선택합니다.  창은 경고에 대한 구성 설정을 표시하도록 업데이트합니다.  위쪽에서 선택한 신호 및 검색 쿼리 자체의 마지막 30분에 대한 결과를 표시합니다.  
5. 다음 정보로 경고를 구성합니다.  
   a. **기준** 드롭다운 목록에서 **미터법**을 선택합니다.  메트릭 측정값은 쿼리에서 지정된 임계값을 초과하는 값을 포함한 각 개체에 대해 경고를 만듭니다.  
   b. **조건**의 경우 **보다 큼**을 선택하고 **임계값**에 대해 **90**을 입력합니다.  
   다. 경고 트리거 기준 섹션 아래에서 **연속 위반**을 선택하고 드롭다운 목록에서 **보다 큼**을 선택하고 3의 값을 입력합니다.  
   d. [평가] 기준 섹션 아래에서 **기간** 값을 **30**분으로 수정합니다. 규칙이 5분마다 실행되고 현재 시간 이전 30분 내에 만들어진 레코드를 반환합니다.  기간을 더 넓은 기간으로 설정하면 잠재적인 데이터 대기 시간이 발생할 수 있으므로 경고가 발생하지 않는 거짓 부정을 방지하기 위해 쿼리에서 데이터를 반환하도록 합니다.  
6. **완료**를 클릭하여 경고 규칙을 완료합니다.<br><br> ![경고 신호 구성](./media/tutorial-response/alert-signal-logic-02.png)<br> 
7. 이제 두 번째 단계로 이동하여 **경고 규칙 이름** 필드에 **백분율 CPU 90%보다 큼**과 같은 경고의 이름을 제공합니다.  경고에 대한 세부 사항을 자세히 설명하는 **설명**을 지정하고 **심각도** 값에 제공된 옵션에서 **위험(Sev 0)** 을 선택합니다.<br><br> ![경고 세부 정보 구성](./media/tutorial-response/alert-signal-logic-04.png)<br>
8. 생성 시 경고 규칙을 즉시 활성화하려면 **규칙을 만들면 바로 사용**에 기본값을 적용합니다.
9. 세 번째 및 마지막 단계는 경고가 트리거되는 각 시간마다 동일한 작업이 수행되고 정의한 각 규칙에 대해 사용할 수 있는지 확인하는 **작업 그룹**을 지정합니다.  다음 정보로 새 작업 그룹을 구성합니다.  
   a. **새 작업 그룹**을 선택하면 **작업 그룹 추가** 창이 나타납니다.  
   b. **동작 그룹 이름**에 **IT 운영 팀 - 알림**과 같은 이름 및 **itops-n**과 같은 **약식 이름**을 지정합니다.  
   다. **구독** 및 **리소스 그룹**에 대한 기본값이 올바른지 확인합니다. 올바르지 않은 경우 드롭다운 목록에서 올바른 값을 선택합니다.   
   d. 작업 섹션 아래에서 **이메일 보내기**와 같은 작업에 대한 이름을 지정하고 **작업 유형** 아래의 드롭다운 목록에서 **이메일/SMS/푸시/음성**을 선택합니다. **이메일/SMS/푸시/음성** 속성 창이 추가 정보를 제공하기 위해 오른쪽에 열립니다.  
   e. **이메일/SMS/푸시/음성** 창에서 **이메일**을 활성화하고 메시지를 전송할 유효한 이메일 SMTP 주소를 제공합니다.  
   f. **확인**을 클릭하여 변경 내용을 저장합니다.<br><br> 

    ![새 작업 그룹 만들기](./media/tutorial-response/action-group-properties-01.png)

10. **확인**을 클릭하여 작업 그룹을 완료합니다. 
11. **경고 규칙 만들기**를 클릭하여 경고 규칙을 완료합니다. 그 즉시 실행이 시작됩니다.<br><br> ![새 경고 규칙 만들기 완료](./media/tutorial-response/alert-rule-01.png)<br> 

## <a name="view-your-alerts-in-azure-portal"></a>Azure Portal에서 경고 보기
이제 경고를 만들었으므로 단일 창에서 Azure 경고를 보고 Azure 구독의 모든 경고 규칙을 관리할 수 있습니다. 모든 경고 규칙(활성화 또는 비활성화)을 나열하고 대상 리소스, 리소스 그룹, 규칙 이름 또는 상태에 따라 정렬될 수 있습니다. 발생한 모든 경고의 집계 요약 및 총 구성/설정 경고 규칙을 포함합니다.<br><br> ![Azure Alerts 상태 페이지](./media/tutorial-response/azure-alerts-02.png)  

경고를 트리거하는 경우 테이블은 선택한 시간 범위 내에서 발생한 조건과 횟수를 반영합니다(기본값은 마지막 6시간).  받은 편지함에 잘못된 가상 머신 및 이 경우 검색 쿼리와 일치한 상위 결과를 보여주는 다음 예제와 비슷한 해당 이메일이 있어야 합니다.<br><br> ![경고 이메일 작업 예제](./media/tutorial-response/azure-alert-email-notification-01.png)

## <a name="next-steps"></a>다음 단계
이 자습서에서는 예약된 간격으로 로그 검색을 실행하고 특정 기준에 일치하는 경우 경고 규칙을 통해 사전에 문제를 식별하여 대응할 수 있는 방법에 대해 알아보았습니다.

미리 빌드된 Log Analytics 스크립트 샘플을 보려면 이 링크를 따릅니다.  

> [!div class="nextstepaction"]
> [Log Analytics 스크립트 샘플](../../azure-monitor/platform/powershell-samples.md)