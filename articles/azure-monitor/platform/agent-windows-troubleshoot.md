---
title: Windows 용 Log Analytics 에이전트의 문제를 해결 하는 방법 | Microsoft Docs
description: Azure Monitor의 Windows 용 Log Analytics 에이전트에 대 한 가장 일반적인 문제에 대 한 증상, 원인 및 해결 방법을 설명 합니다.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/12/2019
ms.author: magoedte
ms.openlocfilehash: 9df389b6e6a73530c9bbf5a2187d6735946e309f
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68249756"
---
# <a name="how-to-troubleshoot-issues-with-the-log-analytics-agent-for-windows"></a>Windows 용 Log Analytics 에이전트 문제를 해결 하는 방법 

이 문서에서는 Azure Monitor의 Windows 용 Log Analytics 에이전트에서 발생할 수 있는 오류를 해결 하는 데 유용한 정보를 제공 하 고 가능한 해결 방법을 제안 합니다.

이 문서의 단계를 수행해도 문제가 해결되지 않으면 다음 지원 채널을 사용할 수 있습니다.

* 프리미어 지원 혜택을 받는 고객은 [프리미어](https://premier.microsoft.com/)를 사용하여 지원 요청을 열 수 있습니다.
* Azure 지원 계약을 맺은 고객은 [Azure Portal](https://manage.windowsazure.com/?getsupport=true)에서 지원 요청을 열 수 있습니다.
* 제출된 아이디어와 버그를 검토하거나 새로운 아이디어를 제출하려면 Log Analytics 피드백 페이지([https://aka.ms/opinsightsfeedback](https://aka.ms/opinsightsfeedback))를 방문하세요. 

## <a name="important-troubleshooting-sources"></a>중요 한 문제 해결 원본

 Windows 용 Log Analytics 에이전트와 관련 된 문제 해결을 지원 하기 위해 에이전트는 이벤트를 Windows 이벤트 로그 ( *응용 프로그램 및 Services\Operations Manager*)에 기록 합니다.  

## <a name="connectivity-issues"></a>연결 문제

에이전트가 프록시 서버 또는 방화벽을 통해 통신 하는 경우 원본 컴퓨터와 Azure Monitor 서비스의 통신을 차단 하는 제한 사항이 있을 수 있습니다. 통신이 차단 된 경우 에이전트를 설치 하는 동안 작업 영역에 대 한 등록이 실패 하 고 에이전트 설치 프로그램이 추가 작업 영역에 보고 하도록 구성 되거나 성공적으로 등록 된 후에 에이전트 통신이 실패 합니다. 이 섹션에서는 이러한 유형의 문제를 해결 하는 방법에 대해 설명 합니다. 

방화벽 또는 프록시가 다음 표에 설명 된 다음 포트 및 Url을 허용 하도록 구성 되어 있는지 확인 합니다. 또한 웹 트래픽에 대해 HTTP 검사를 사용 하도록 설정 하지 않았는지 확인 합니다 .이는 에이전트와 Azure Monitor 간의 보안 TLS 채널을 방지할 수 있기 때문입니다.  

|에이전트 리소스|포트 |Direction |HTTPS 검사 무시|
|------|---------|--------|--------|   
|*.ods.opinsights.azure.com |포트 443 |아웃바운드|예 |  
|*.oms.opinsights.azure.com |포트 443 |아웃바운드|예 |  
|\*.blob.core.windows.net |포트 443 |아웃바운드|예 |  
|\* .azure-automation.net |포트 443 |아웃바운드|예 |  

Azure Government에 필요한 방화벽 정보는 [Azure Government management](../../azure-government/documentation-government-services-monitoringandmanagement.md#azure-monitor-logs)를 참조 하세요. 

에이전트가 Azure Monitor와 성공적으로 통신 하 고 있는지 확인할 수 있는 몇 가지 방법이 있습니다.

- 작업 영역에서 [Azure Log Analytics 에이전트 상태 평가](../insights/solution-agenthealth.md) 를 사용 하도록 설정 합니다. 에이전트 상태 대시보드에서 **응답 하지 않는 에이전트 수** 열을 확인 하 여 에이전트가 나열 되어 있는지 빠르게 확인 합니다.  

- 다음 쿼리를 실행 하 여 에이전트가 보고 하도록 구성 된 작업 영역에 하트 비트를 보내고 있는지 확인 합니다. 을 `<ComputerName>` 컴퓨터의 실제 이름으로 바꿉니다.

    ```
    Heartbeat 
    | where Computer like "<ComputerName>"
    | summarize arg_max(TimeGenerated, * ) by Computer 
    ```

    컴퓨터가 서비스와 성공적으로 통신 하는 경우 쿼리는 결과를 반환 해야 합니다. 쿼리에서 결과를 반환 하지 않은 경우 먼저 에이전트가 올바른 작업 영역에 보고 하도록 구성 되어 있는지 확인 합니다. 올바르게 구성 된 경우 3 단계로 이동 하 고 Windows 이벤트 로그를 검색 하 여 에이전트에서 Azure Monitor와의 통신을 방해할 수 있는 문제를 기록 하는지 확인 합니다.

- 연결 문제를 확인 하는 또 다른 방법은 **Testcloudconnectivity** 도구를 실행 하는 것입니다. 이 도구는 기본적으로 에이전트와 함께 *%SystemRoot%\Program Files\Microsoft Monitoring Agent\Agent*폴더에 설치 됩니다. 관리자 권한 명령 프롬프트에서 폴더로 이동 하 여 도구를 실행 합니다. 이 도구는 결과가 반환 되 고 테스트가 실패 한 위치를 강조 표시 합니다 (예: 차단 된 특정 포트/URL과 관련 된 경우). 

    ![TestCloudConnection 도구 실행 결과](./media/agent-windows-troubleshoot/output-testcloudconnection-tool-01.png)

- *모듈*, *health service*및 *서비스 커넥터* 상태 관리 서비스 **이벤트 원본** - 으로 *Operations Manager* 이벤트 로그를 필터링 하 고 **이벤트 수준** *경고* 및 *오류* 를 기준으로 필터링 합니다. 다음 표의 이벤트를 기록 했는지 확인 합니다. 가능한 경우 각 이벤트에 대해 포함 된 해결 단계를 검토 합니다.

    |이벤트 ID |Source |Description |해결 방법 |
    |---------|-------|------------|-----------|
    |2133 & 2129 |상태 관리 서비스 |에이전트에서 서비스에 연결 하지 못했습니다. |이 오류는 에이전트가 직접 또는 방화벽/프록시 서버를 통해 Azure Monitor 서비스와 통신할 수 없는 경우에 발생할 수 있습니다. 에이전트 프록시 설정을 확인 하거나 네트워크 방화벽/프록시가 컴퓨터에서 서비스로의 TCP 트래픽을 허용 하는지 확인 합니다.|
    |2138 |상태 관리 서비스 모듈 |프록시에 인증 필요 |에이전트 프록시 설정을 구성 하 고 프록시 서버를 인증 하는 데 필요한 사용자 이름/암호를 지정 합니다. |
    |2129 |상태 관리 서비스 모듈 |실패 한 연결/SSL 협상 실패 |네트워크 어댑터 TCP/IP 설정 및 에이전트 프록시 설정을 확인 합니다.|
    |2127 |상태 관리 서비스 모듈 |데이터를 보내는 동안 오류가 발생 했습니다. 오류 코드 |하루 동안만 정기적으로 발생 하는 경우에는 무시할 수 있는 무작위 비정상 것일 수 있습니다. 모니터링 하 여 발생 빈도를 파악 합니다. 하루 종일 자주 발생 하는 경우 먼저 네트워크 구성 및 프록시 설정을 확인 합니다. 설명에 HTTP 오류 코드 404이 포함 되어 있고 에이전트가 처음으로 서비스에 데이터를 보내려고 할 때 내부 404 오류 코드와 함께 500 오류가 포함 됩니다. 404는 새 작업 영역에 대 한 저장소 영역이 아직 프로 비전 되 고 있음을 나타내는를 찾을 수 없음을 의미 합니다. 다음에 다시 시도 하면 데이터가 예상 대로 작업 영역에 성공적으로 기록 됩니다. HTTP 오류 403은 사용 권한 또는 자격 증명 문제를 나타낼 수 있습니다. 문제를 해결 하는 데 도움이 되는 403 오류에 대 한 추가 정보가 포함 되어 있습니다.|
    |4000 |서비스 커넥터 |DNS 이름 확인에 실패 했습니다. |컴퓨터에서 서비스에 데이터를 보낼 때 사용 되는 인터넷 주소를 확인할 수 없습니다. 이는 컴퓨터의 DNS 확인자 설정, 잘못 된 프록시 설정 또는 공급자의 일시적인 DNS 문제일 수 있습니다. 정기적으로 발생 하는 경우 일시적인 네트워크 관련 문제로 인해 발생할 수 있습니다.|
    |4001 |서비스 커넥터 |서비스에 연결 하지 못했습니다. |이 오류는 에이전트가 직접 또는 방화벽/프록시 서버를 통해 Azure Monitor 서비스와 통신할 수 없는 경우에 발생할 수 있습니다. 에이전트 프록시 설정을 확인 하거나 네트워크 방화벽/프록시가 컴퓨터에서 서비스로의 TCP 트래픽을 허용 하는지 확인 합니다.|
    |4002 |서비스 커넥터 |서비스에서 쿼리에 대 한 응답으로 HTTP 상태 코드 403을 반환 했습니다. 서비스 관리자에 게 서비스의 상태를 확인 합니다. 쿼리는 나중에 다시 시도 됩니다. |이 오류는 에이전트의 초기 등록 단계를 수행 하는 동안 기록 되며 다음과 유사한 URL을 볼 수 있습니다 *.\<https://workspaceID >.* 오류 코드 403은 사용할 수 없으며 잘못 된 작업 영역 ID 또는 키로 인해 발생할 수 있으며, 컴퓨터에서 데이터와 시간이 올바르지 않습니다. 시간이 현재 시간에서 +/-15분인 경우 등록이 실패합니다. 이를 해결 하려면 Windows 컴퓨터의 날짜 및/또는 표준 시간대를 업데이트 합니다.|

## <a name="data-collection-issues"></a>데이터 수집 문제

에이전트가 설치 되 고 구성 된 작업 영역 또는 작업 영역에 보고 된 후에는 사용 하도록 설정 하 고 컴퓨터를 대상으로 지정 하는 방법에 따라 구성 수신을 중지 하 고 성능, 로그 또는 기타 데이터를 서비스로 수집 하거나 전달할 수 있습니다. 다음을 확인 해야 합니다.

- 특정 데이터 형식 또는 작업 영역에서 사용할 수 없는 모든 데이터 입니까?
- 솔루션에 지정 된 데이터 형식 이거나 작업 영역 데이터 컬렉션 구성의 일부로 지정 되었습니까?
- 영향을 받는 컴퓨터 수는 몇 개입니까? 작업 영역에 보고 하는 단일 컴퓨터 또는 여러 컴퓨터 입니까?
- 작업을 수행 했 고 특정 시간에 중단 되었거나 수집 되지 않았습니까? 
- 구문적으로 올바른 로그 검색 쿼리를 사용 하 고 있습니까? 
- 에이전트가 Azure Monitor에서 구성을 받았는지 여부

문제 해결의 첫 번째 단계는 컴퓨터가 하트 비트 이벤트를 전송 하 고 있는지 확인 하는 것입니다.

```
Heartbeat 
    | where Computer like "<ComputerName>"
    | summarize arg_max(TimeGenerated, * ) by Computer
```

쿼리에서 결과를 반환 하는 경우 특정 데이터 형식이 수집 되지 않고 서비스로 전달 되는지 확인 해야 합니다. 이 문제는 에이전트가 서비스에서 업데이트 된 구성을 받지 못하거나 에이전트가 정상적으로 작동 하지 않는 다른 증상이 원인일 수 있습니다. 추가로 문제를 해결 하려면 다음 단계를 수행 합니다.

1. 컴퓨터에서 관리자 권한 명령 프롬프트를 열고를 입력 `net stop healthservice && net start healthservice`하 여 에이전트 서비스를 다시 시작 합니다.
2. *Operations Manager* 이벤트 로그를 열고 이벤트 **원본** *health service*에서 **이벤트 id** *7023, 7024, 7025, 7028* 및 *1210* 를 검색 합니다.  이러한 이벤트는 에이전트가 Azure Monitor에서 구성을 성공적으로 수신 하 고 컴퓨터를 적극적으로 모니터링 하 고 있음을 표시 합니다. 이벤트 ID 1210에 대 한 이벤트 설명도 에이전트의 모니터링 범위에 포함 된 모든 솔루션과 정보를 마지막 줄에 지정 합니다.  

    ![이벤트 ID 1210 설명](./media/agent-windows-troubleshoot/event-id-1210-healthservice-01.png)

3. 몇 분 후에 쿼리 결과 또는 시각화에 필요한 데이터가 표시 되지 않는 경우 *Operations Manager* 이벤트 로그에서 **이벤트 소스** 검색에서 데이터를 볼 수 있는지 여부에 따라 쿼리 결과 또는 시각화에 필요한 데이터가 표시 되지 않습니다. *health service* 및는 *모듈을 상태 관리 서비스* 하 고 **이벤트 수준** *경고* 및 *오류* 를 기준으로 필터링 하 여 다음 표의 이벤트를 기록 했는지 확인 합니다.

    |이벤트 ID |Source |설명 |해결 방법 |
    |---------|-------|------------|
    |8000 |HealthService |이 이벤트는 성능, 이벤트 또는 수집 된 다른 데이터 형식과 관련 된 워크플로가 작업 영역에 수집 하기 위해 서비스로 전달할 수 없는 경우를 지정 합니다. | 원본 Health service의 이벤트 ID 2136는이 이벤트와 함께 기록 되며, 에이전트에서 서비스와 통신할 수 없음을 나타낼 수 있습니다. 프록시 및 인증 설정, 네트워크 중단 또는 네트워크 방화벽/의 잘못 된 구성으로 인 한 것일 수 있습니다. 프록시는 컴퓨터에서 서비스로의 TCP 트래픽을 허용 하지 않습니다.| 
    |10102 및 10103 |상태 관리 서비스 모듈 |워크플로에서 데이터 원본을 확인할 수 없습니다. |지정 된 성능 카운터 또는 인스턴스가 컴퓨터에 없거나 작업 영역 데이터 설정에 잘못 정의 된 경우이 문제가 발생할 수 있습니다. 사용자 지정 [성능 카운터](data-sources-performance-counters.md#configuring-performance-counters)인 경우 지정 된 정보가 올바른 형식을 사용 하 고 대상 컴퓨터에 존재 하는지 확인 합니다. |
    |26002 |상태 관리 서비스 모듈 |워크플로에서 데이터 원본을 확인할 수 없습니다. |이 문제는 지정 된 Windows 이벤트 로그가 컴퓨터에 없는 경우에 발생할 수 있습니다. 컴퓨터에이 이벤트 로그가 등록 되지 않은 경우이 오류를 안전 하 게 무시할 수 있습니다. 그렇지 않으면 사용자가 지정한 [이벤트 로그](data-sources-windows-events.md#configuring-windows-event-logs)이면 지정 된 정보가 올바른지 확인 하십시오. |

