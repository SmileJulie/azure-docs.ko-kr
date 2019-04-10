---
title: Azure Monitor 통합 문서를 사용 하 여 대화형 보고서 만들기 | Microsoft Docs
description: 복잡 한 보고 사용자 지정 및 미리 정의 된 매개 변수가 있는 통합 문서를 사용 하 여 Azure Monitor에 대 한 Vm에 대 한 간소화 합니다.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/05/2019
ms.author: magoedte
ms.openlocfilehash: 90c236347380bb5d5e51db56d0f431d2659a7258
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59288713"
---
# <a name="create-interactive-reports-with-azure-monitor-workbooks"></a>Azure Monitor 통합 문서를 사용하여 대화형 보고서 만들기

통합 문서에 텍스트를 결합할 [쿼리를 로깅](../log-query/query-language.md), 메트릭 및 풍부한 대화형 보고서에 매개 변수입니다. 통합 문서는 동일한 Azure 리소스에 대한 액세스 권한이 있는 다른 팀 멤버에 의해 편집될 수 있습니다.

통합 문서는와 같은 시나리오에 유용 합니다.

* 알 수 없는 경우 관심 메트릭을 사전에 가상 컴퓨터의 사용 현황 탐색: CPU 사용률, 디스크 공간, 메모리, 네트워크 종속성, 등입니다. 다른 사용 현황 분석 도구와는 달리 통합 문서를 통해 이러한 종류의 자유 형식 탐색에 매우 유용하도록 여러 종류의 시각화 및 분석을 결합할 수 있습니다.
* 팀에 주요 카운터 및 기타 로그 이벤트에 대 한 메트릭을 표시 하 여 최근에 프로 비전 된 VM을 수행 하는 방법을 설명 합니다.
* 팀의 다른 멤버를 사용 하 여 VM의 크기 조정 실험의 결과 공유 합니다. 텍스트를 사용 하 여 실험에 대 한 목표를 설명한 다음 각 사용량 메트릭 및 분석 쿼리를 보여 줍니다. 수 있습니다 각 메트릭이 위 또는 아래-대상 인지 여부에 대 한 명확한 설명을 함께 실험을 평가 하는 데 사용 합니다.
* VM의 데이터, 텍스트 설명 및 다음 단계의 논의 결합 합니다. 나중에 중단을 방지 하기 위해 사용 중단의 영향을 보고 합니다.

다음 표에서 요약해 서 설명 및 Vm에 대 한 azure Monitor를 시작 하는 데 여러 통합 문서를 포함 합니다.

| 통합 문서 | 설명 | 범위 |
|----------|-------------|-------|
| 성능 | Top N 목록 및 모든 설정한 Log Analytics 성능 카운터를 활용 하는 단일 통합 문서에서 차트 보기의 사용자 지정 가능한 버전을 제공 합니다.| 대규모로 |
| 성능 카운터 | 다양 한 성능 카운터에서 상위 N 차트 보기입니다. | 대규모로 |
| 연결 | 연결은 모니터링 된 Vm에서 인바운드 및 아웃 바운드 연결의 세부적인 보기를 제공 합니다. | 대규모로 |
| 활성 포트 | 모니터링 된 Vm 및 선택한 기간에서 해당 활동에서 포트를 바인딩한 프로세스의 목록을 제공 합니다. | 대규모로 |
| 포트 열기 | 모니터링 된 Vm에서 열려 있는 포트의 수 및 포트를 열어야 하는 것에 대 한 세부 정보를 제공 합니다. | 대규모로 |
| 실패한 연결 | 모니터링 된 Vm, 오류 추세에서 실패 한 연결 수를 표시 합니다. 시간이 지남에 따라 오류 비율이 증가 하는 경우. | 대규모로 |
| 보안 및 감사 | 전체 연결을 IP 끝점 전역적으로 상주 하는 악의적인 연결을 보고 하는 TCP/IP 트래픽 분석 합니다.  모든 기능을 사용 하려면 보안 검색을 사용 하도록 설정 해야 합니다. | 대규모로 |
| TCP 트래픽 | 모니터링 된 Vm 및 해당 전송, 수신, 및 총 네트워크에 대 한 순위 보고서 표에 트래픽 및 추세 선으로 표시 합니다. | 대규모로 |
| 트래픽 비교 | 이 통합 문서를 사용 하면 단일 컴퓨터 또는 컴퓨터 그룹에 대 한 네트워크 트래픽 추세를 비교할 수 있습니다. | 대규모로 |
| 성능 | 모든 설정한 Log Analytics 성능 카운터를 활용 하는 성능 보기의 사용자 지정 가능한 버전을 제공 합니다. | 단일 VM | 
| 연결 | 연결 하 여 VM에서 인바운드 및 아웃 바운드 연결의 세부적인 보기를 제공 합니다. | 단일 VM |
 
## <a name="starting-with-a-template-or-saved-workbook"></a>템플릿 또는 저장된 통합 문서 시작

통합 문서는 독립적으로 편집할 수 있는 차트, 테이블, 텍스트 및 입력 컨트롤로 구성된 섹션으로 구성됩니다. 통합 문서를 더 잘 이해 하려면 서식 파일을 열어 시작 하 고 사용자 지정 통합 문서를 만드는 과정을 설명 해 보겠습니다. 

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.

2. **Virtual Machines**를 선택합니다.

3. 목록에서 VM을 선택합니다.

4. VM 페이지에 **모니터링** 섹션에서 **인사이트(미리 보기)** 를 선택합니다.

5. VM 정보 페이지에서 선택 **성능** 또는 **Maps** 탭을 선택한 다음 **뷰 통합 문서** 페이지의 링크에서. 

    ![통합 문서에 대한 탐색 스크린샷](media/vminsights-workbooks/workbook-option-01.png)

6. 드롭다운 목록에서 선택 **갤러리로 이동** 목록 맨 아래에서.

    ![통합 문서 드롭 다운 목록 스크린샷](media/vminsights-workbooks/workbook-dropdown-gallery-01.png)

    다양 한 시작할 수 있도록 하려면 미리 작성 된 통합 문서를 사용 하 여 통합 문서 갤러리를 시작 합니다.

7. **빠른 시작** 제목 아래에 있는 **기본 템플릿**부터 시작하겠습니다.

    ![통합 문서 갤러리 스크린샷](media/vminsights-workbooks/workbook-gallery-01.png)

## <a name="editing-workbook-sections"></a>통합 문서 섹션 편집

통합 문서에는 **편집 모드** 및 **읽기 모드**라는 두 가지 모드가 있습니다. 기본 서식 파일 통합 문서를 먼저 시작에서 열릴 **편집 모드**합니다. 모든 단계와 그렇지 않은 경우 숨겨진 매개 변수를 포함 하 여 통합 문서의 모든 내용이 표시 됩니다. **읽기 모드**는 간소화된 보고서 스타일 보기를 제공합니다. 읽기 모드를 사용 하면 않아도 기반 메커니즘이 숨어만 몇 번의 클릭 번 수정에 필요할 때 보고서를 만드는 발전 된 복잡성을 추상화 수 있습니다.

![Vm 통합 문서 섹션 편집 컨트롤에 대 한 azure Monitor](media/vminsights-workbooks/workbook-new-workbook-editor-01.png)

1. 완료 되 면 섹션을 편집, 클릭 **편집 완료** 섹션의 왼쪽 아래 모퉁이에서.

2. 중복 섹션을 만들려면 **이 섹션 복제** 아이콘을 클릭합니다. 중복 섹션을 만드는 것은 이전 반복을 손실하지 않고 쿼리를 반복하는 좋은 방법입니다.

3. 통합 문서에서 섹션을 이동하려면 **위로 이동** 또는 **아래로 이동** 아이콘을 클릭합니다.

4. 섹션을 영구적으로 제거하려면 **제거** 아이콘을 클릭합니다.

## <a name="adding-text-and-markdown-sections"></a>텍스트 및 Markdown 섹션 추가

통합 문서에 제목, 설명 및 해설을 추가하면 테이블 및 차트의 집합을 설명으로 전환할 수 있습니다. 통합 문서의 텍스트 섹션은 머리글, 굵게, 기울임꼴 및 글머리 기호 목록과 같은 텍스트 서식 지정에 대해 [Markdown 구문](https://daringfireball.net/projects/markdown/)을 지원합니다.

통합 문서에 텍스트 섹션을 추가하려면 통합 문서의 맨 아래 또는 섹션의 맨 아래에 있는 **텍스트 추가** 단추를 사용합니다.

## <a name="adding-query-sections"></a>쿼리 추가 섹션

![통합 문서의 쿼리 섹션](media/vminsights-workbooks/005-workbook-query-section.png)

통합 문서에 쿼리 섹션을 추가하려면 통합 문서의 맨 아래 또는 섹션의 맨 아래에 있는 **쿼리 추가** 단추를 사용합니다.

쿼리 섹션은 매우 유연하고 다음과 같은 질문에 응답하는 데 사용할 수 있습니다.

* 네트워크 트래픽이 증가할 동일한 기간 동안 CPU 사용률이 어떻게 되었나요?
* 사용 가능한 디스크 공간 추세는 지난 한 달 동안 무엇 이었습니까?
* 내 VM에서 마지막으로 2 주 동안 얼마나 많은 네트워크 연결 오류 발생 했지만? 

있습니다도 아닌로 제한에서 통합 문서를 시작할 가상 컴퓨터의 컨텍스트에서 쿼리 합니다. 해당 리소스에 대 한 액세스 권한이 있다면 여러 virtual machines 뿐 아니라 Log Analytics 작업 영역 간에 쿼리할 수 있습니다.

사용 하 여 특정 Application Insights 앱 또는 다른 Log Analytics 작업 영역에서 데이터를 포함 하는 **작업 영역** 식별자입니다. 리소스 간 쿼리에 대 한 자세한 내용은 참조는 [공식 지침](../log-query/cross-workspace-query.md)합니다.

### <a name="advanced-analytic-query-settings"></a>고급 분석 쿼리 설정

각 섹션에 설정을 통해 액세스할 수 있는 고급 설정 자체 ![통합 문서 섹션 편집 컨트롤](media/vminsights-workbooks/006-settings.png) 아이콘의 오른쪽에 있는 합니다 **매개 변수 추가** 단추입니다.

![Vm 통합 문서 섹션 편집 컨트롤에 대 한 azure Monitor](media/vminsights-workbooks/007-settings-expanded.png)

|         |          |
| ---------------- |:-----|
| **사용자 지정 너비**    | 되며 항목 임의의 크기를 더 풍부한 대화형 보고서를 사용자 테이블과 차트를 구성할 수는 한 줄에 여러 항목을 넣을 수 있습니다.  |
| **조건부로 표시** | 읽기 모드에서 매개 변수에 따라 단계를 숨기려면이 옵션을 지정 합니다. |
| **내보내기는 매개 변수**| 표 또는 차트를 값을 변경 하거나 표시 하는 이후 단계에서 선택한 행을 허용 합니다.  |
| **편집하고 있지 않을 때 쿼리 표시** | 읽기 모드에서 차트 또는 테이블에도 위의 쿼리를 표시 합니다.
| **편집하지 않을 때 [Analytics에서 열기] 단추 표시** | 원클릭 액세스할 수 있도록 차트의 오른쪽 모서리에 있는 파란색 Analytics 아이콘을 추가 합니다.|

이러한 설정 중 대부분은 매우 간단하지만 **매개 변수 내보내기**를 이해하려면 이 기능을 사용할 수 있는 통합 문서를 검사하는 것이 좋습니다.

미리 작성 된 통합 문서-중 하나 **TCP 트래픽을**, VM에서 연결 메트릭에 대 한 정보를 제공 합니다.

통합 문서의 첫 번째 섹션은 데이터를 쿼리 하는 로그를 기반으로 합니다. 두 번째 섹션은 또한 로그 쿼리 데이터를 기반으로 하지만 첫 번째 테이블의 행을 선택 하면 차트의 내용을 대화형으로 업데이트 됩니다.

![Vm 통합 문서 섹션 편집 컨트롤에 대 한 azure Monitor](media/vminsights-workbooks/008-workbook-tcp-traffic.png)

동작을 사용 하 여 가능 합니다 **항목을 선택 하면 내보내기는 매개 변수** 고급 테이블의 로그 쿼리에서 사용 되는 설정입니다.

![Vm 통합 문서 섹션 편집 컨트롤에 대 한 azure Monitor](media/vminsights-workbooks/009-settings-export.png)

다음 섹션 제목 및 차트에서 사용 되는 값의 집합을 만드는 행을 선택 하는 경우 두 번째 로그 쿼리를 다음 내보낸된 값을 활용 합니다. 행을 선택 하면 섹션 제목 및 차트 숨깁니다. 

예를 들어, 두 번째 섹션에서 숨겨진된 매개 변수 표에서 선택한 행에서 다음 참조를 사용 합니다.

```
VMConnection
| where TimeGenerated {TimeRange}
| where Computer in ("{ComputerName}") or '*' in ("{ComputerName}") 
| summarize Sent = sum(BytesSent), Received = sum(BytesReceived) by bin(TimeGenerated, {TimeRange:grain})
```

## <a name="adding-metrics-sections"></a>메트릭 추가 섹션

메트릭 섹션에서는 Azure Monitor 메트릭 데이터를 대화형 보고서로 통합할 수 있는 모든 권한을 제공합니다. Vm에 대 한 Azure Monitor에서 메트릭 데이터를 사용 하지 않고 데이터를 분석 쿼리에 일반적으로 미리 작성 된 통합 문서 포함 됩니다.  모두 한곳에서에서 모두 기능의 최고의 활용 하기 위해 수 있도록 하는 메트릭 데이터를 사용 하 여 통합 문서 만들기를 선택할 수 있습니다. 액세스 권한이 있는 구독에 있는 리소스에서 메트릭 데이터를 가져올 수 있는 기능도 있습니다.

CPU 성능의 표 시각화를 제공 하는 통합 문서로 가져온 중인 가상 컴퓨터 데이터의 예는 다음과 같습니다.

![Vm 통합 문서 섹션 편집 컨트롤에 대 한 azure Monitor](media/vminsights-workbooks/010-metrics-grid.png)

## <a name="adding-parameter-sections"></a>매개 변수 추가 섹션

통합 문서 매개 변수를 사용하면 쿼리 또는 텍스트 섹션을 수동으로 편집하지 않고 통합 문서에서 값을 변경할 수 있습니다. 그러면 기본 분석 쿼리 언어를 이해해야 하는 요구 사항을 제거하고 통합 문서 기반 보고의 잠재적인 대상을 크게 확장하게 됩니다.

``{parameterName}``과 같은 중괄호에서 매개 변수의 이름을 입력하여 쿼리, 텍스트 또는 다른 매개 변수 섹션에서 매개 변수 값을 대체합니다. 매개 변수 이름은 JavaScript 식별자, 영문자 또는 밑줄, 영숫자 나 밑줄 뒤에 유사한 규칙으로 제한 됩니다. 예를 들어 **a1**은 허용되지만, **1a**는 허용되지 않습니다.

매개 변수는 선형이며 통합 문서의 맨 위에서 시작하고 이후 단계까지 이어집니다.  통합 문서에서 나중에 선언 된 매개 변수는 이전에 선언 된 매개 변수를 재정의할 수 있습니다. 이렇게 하면 쿼리를 사용 하 여 앞에서 정의한 매개 변수에서 값에 액세스 하는 매개 변수를 수도 있습니다. 또한 매개 변수의 단계 자체 내에서 매개 변수는 왼쪽에서 오른쪽으로 이어지는 선형이며 여기서 오른쪽 매개 변수는 동일한 단계에서 이전에 선언된 매개 변수에 종속될 수 있습니다.
 
네 가지 유형의 현재 지원 되는 매개 변수는

|                  |      |
| ---------------- |:-----|
| **텍스트**    | 사용자가 텍스트 상자를 편집할 수 있으며 필요에 따라 기본 값을 입력 하는 쿼리를 제공할 수 있습니다. |
| **드롭다운** | 값 집합을 선택할 수 있습니다. |
| **시간 범위 선택기**| 시간 범위 값을 미리 정의 된 집합에서 선택 하거나 사용자 지정 시간 범위에서 선택할 수 있습니다.|
| **리소스 선택기** | 통합 문서에 대해 선택한 리소스 중에서 선택할 수 있습니다.|

### <a name="using-a-text-parameter"></a>텍스트 매개 변수 사용

텍스트 상자에 사용자가 직접 쿼리에서 이스케이프 없거나 인용 부호를 사용 하 여 대체 되는 값입니다. 필요한 값이 문자열인 경우 쿼리는 매개 변수에 인용 부호를 표시해야 합니다(예: **'{parameter}'**).

텍스트 매개 변수는 어디에 나 사용할 수 있는 텍스트 상자에 값을 허용 합니다. 테이블 이름, 열 이름, 함수 이름, 연산자 등일 수 있습니다.  Text 매개 변수 형식에는 설정이 **분석 쿼리에서 기본값 가져오기**, 통합 문서 작성자는 쿼리를 사용 하 여 해당 텍스트 상자에 대 한 기본 값을 채울 수 있습니다.

로그 쿼리의 기본 값을 사용 하는 경우 첫 번째 행 (행 0, 0 열)의 첫 번째 값만 기본값으로 사용 됩니다. 따라서 하나의 행과 하나의 열만 반환하도록 쿼리를 제한하는 것이 좋습니다. 쿼리에서 반환된 다른 데이터는 무시됩니다. 

쿼리가 반환하는 값이 어떤 값이든지 이스케이프 또는 인용 부호 없이 직접 대체됩니다. 쿼리에서 행을 반환하지 않는 경우 매개 변수의 결과는 빈 문자열(매개 변수가 필요 없는 경우)이거나 정의되지 않습니다(매개 변수가 필수인 경우).

### <a name="using-a-drop-down"></a>드롭다운을 사용 하 여

드롭다운 매개 변수 형식은 하나 이상의 값을 선택할 수 드롭다운 컨트롤을 만들 수 있습니다.

드롭다운은 로그 쿼리 또는 JSON으로 채워집니다. 쿼리에서 하나의 열을 반환 하는 경우 해당 열의 값은 값 및 드롭다운 컨트롤에서 레이블. 쿼리에서 두 열을 반환 하는 경우 첫 번째 열 값 이며 두 번째 열 드롭다운 목록에 표시 되는 레이블입니다. 쿼리에서 세 개의 열을 반환 하는 경우 세 번째 열은 해당 드롭다운 목록에서 기본 선택 항목을 나타내기 위해 사용 됩니다. 이 열은 어떤 형식이든 될 수 있지만 가장 간단한 형식은 부울 또는 숫자 형식입니다. 여기서 0은 false이고, 1은 true입니다.

열이 문자열 형식인 경우 null/비어 있는 문자열은 false로 간주되고, 다른 모든 값은 true로 간주됩니다. 단일 선택 드롭다운 목록에 대 한 값이 true 인 첫 번째 값을 기본으로 선택이 됩니다.  여러 선택 드롭다운 true 값을 사용 하 여 모든 값 선택한 기본 집합으로 사용 됩니다. 드롭다운 목록에서 항목 쿼리를 원하는 순서로 행을 반환 표시 됩니다. 

연결 개요 보고서에 매개 변수를 살펴보겠습니다. 옆에 있는 편집 기호를 클릭 **방향을**합니다.

![Vm 통합 문서 섹션 편집 컨트롤에 대 한 azure Monitor](media/vminsights-workbooks/011-workbook-using-dropdown.png)

시작 합니다 **매개 변수 편집** 메뉴 항목입니다.

![Vm 통합 문서 섹션 편집 컨트롤에 대 한 azure Monitor](media/vminsights-workbooks/012-workbook-edit-parameter.png)

JSON을 통해 콘텐츠로 채웁니다 임의의 테이블을 생성할 수 있습니다. 예를 들어, 다음 JSON 드롭다운 목록에서 두 값을 생성 합니다.

```
[
    { "value": "inbound", "label": "Inbound"},
    { "value": "outbound", "label": "Outbound"}
]
```

더 적용 하는 예제는 성능 카운터 집합에서 이름으로 선택 드롭다운을 사용 합니다.

```
Perf
| summarize by CounterName, ObjectName
| order by ObjectName asc, CounterName asc
| project Counter = pack('counter', CounterName, 'object', ObjectName), CounterName, group = ObjectName
```

쿼리는 다음과 같이 결과를 표시합니다.

![성능 카운터 드롭다운](media/vminsights-workbooks/013-workbook-edit-parameter-perf-counters.png)

드롭다운은 사용자 지정 하 고 대화형 보고서를 만들기 위한 매우 강력한 도구입니다.

### <a name="time-range-parameters"></a>시간 범위 매개 변수

드롭다운 매개 변수 형식을 통해 고유한 사용자 지정 시간 범위 매개 변수를 만들 수 있는 동시에 동일한 수준의 유연성이 필요하지 않는 경우 기본 시간 범위 매개 변수 형식을 사용할 수도 있습니다. 

시간 범위 매개 변수 형식에는 5분부터 최근 90일까지 이동하는 15개의 기본 범위가 포함됩니다. 사용자 지정 시간 범위 선택 영역을 허용하는 옵션이 있습니다. 이를 통해 보고서의 연산자가 시간 범위에 대해 명시적 시작 및 중지 값을 선택할 수 있습니다.

### <a name="resource-picker"></a>리소스 선택기

리소스 선택기 매개 변수 형식에서는 특정 형식의 리소스로 보고서 범위를 지정하는 기능을 제공합니다. 리소스 선택 종류를 활용 하는 미리 작성 된 통합 문서의 예로 **성능** 통합 문서.

![작업 영역 드롭다운](media/vminsights-workbooks/014-workbook-edit-parameter-workspaces.png)

## <a name="saving-and-sharing-workbooks-with-your-team"></a>팀과 통합 문서 저장 및 공유

통합 문서는 Log Analytics 작업 영역 또는 통합 문서 갤러리에 액세스 하는 방법에 따라 가상 머신 리소스 내에 저장 됩니다. 통합 문서에 저장할 수 있습니다는 **내 보고서** 또는 개인 섹션을 **공유 보고서** 리소스에 대 한 액세스를 사용 하 여 모든 사용자에 게 액세스 하는 섹션입니다. 리소스에서 모든 통합 문서를 보려면 작업 모음에서 **열기** 단추를 클릭합니다.

현재 **내 보고서**에 있는 통합 문서를 공유하려면:

1. 작업 모음에서 **열기**를 클릭합니다.
2. 공유하려는 통합 문서 옆에 있는 "..." 단추를 클릭합니다.
3. **공유 보고서로 이동**을 클릭합니다.

링크 또는 전자 메일을 통해 통합 문서를 공유하려면 작업 모음에서 **공유**를 클릭합니다. 통합 문서를 보려면 링크의 수신자는 Azure Portal에서 이 리소스에 대한 액세스가 필요함을 명심하세요. 편집하려면 수신자는 최소한 리소스에 대한 참가자 권한이 필요합니다.

Azure 대시보드에 통합 문서에 대한 링크를 고정하려면:

1. 작업 모음에서 **열기**를 클릭합니다.
2. 고정하려는 통합 문서 옆에 있는 "..." 단추를 클릭합니다.
3. **대시보드에 고정**을 클릭합니다.

## <a name="next-steps"></a>다음 단계
상태 기능을 사용 하는 방법에 알아보려면 참조 [Azure VM 상태 보기](vminsights-health.md)를 참조 검색 된 응용 프로그램 종속성을 보려면 [Vm 맵에 대 한 Azure Monitor 뷰](vminsights-maps.md)합니다. 