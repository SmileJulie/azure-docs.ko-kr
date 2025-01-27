---
title: Azure VPN Gateway에서 진단 로그 이벤트에 대 한 경고 설정
description: VPN Gateway 진단 로그 이벤트에 대 한 경고를 구성 하는 단계
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: alzam
ms.openlocfilehash: c84d457c51f71bdf315bbbcec674ff1186dd905f
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68249008"
---
# <a name="set-up-alerts-on-diagnostic-log-events-from-vpn-gateway"></a>VPN Gateway에서 진단 로그 이벤트에 대 한 경고 설정

이 문서는 azure Log Analytics를 사용 하 여 Azure VPN Gateway의 진단 로그 이벤트를 기반으로 경고를 설정 하는 데 도움이 됩니다. 

Azure에서 사용할 수 있는 로그는 다음과 같습니다.

|***이름*** | ***설명*** |
|---        | ---               |
|GatewayDiagnosticLog | 게이트웨이 구성 이벤트, 기본 변경 내용 및 유지 관리 이벤트에 대 한 진단 로그를 포함 합니다. |
|TunnelDiagnosticLog | 터널 상태 변경 이벤트를 포함 합니다. 해당 하는 경우 터널 연결/연결 끊기 이벤트에 상태 변경에 대 한 요약 된 이유가 있습니다. |
|RouteDiagnosticLog | 게이트웨이에서 발생 하는 고정 경로 및 BGP 이벤트에 대 한 변경 내용을 기록 합니다. |
|IKEDiagnosticLog | 게이트웨이에서 IKE 제어 메시지 및 이벤트를 로깅합니다. |
|P2SDiagnosticLog | 게이트웨이에서 지점 및 사이트 제어 메시지 및 이벤트를 로깅합니다. |

## <a name="setup"></a>경고 설정

다음 예제 단계는 사이트 간 VPN 터널이 포함 된 연결 끊기 이벤트에 대 한 경고를 만듭니다.


1. Azure Portal에서 **모든 서비스** 아래 **Log Analytics** 를 검색 하 고 **Log Analytics 작업 영역**을 선택 합니다.

   ![Log Analytics 작업 영역으로 이동 하기 위한 선택 항목](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert0.png "만들기")

2. **Log Analytics** 페이지에서 **만들기** 를 선택 합니다.

   ![만들기 단추가 있는 Log Analytics 페이지](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert1.png  "선택")

3. **새로 만들기** 를 선택 하 고 세부 정보를 입력 합니다.

   ![Log Analytics 작업 영역을 만드는 방법에 대 한 세부 정보](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert2.png  "선택")

4. **진단 설정** **모니터링** > 블레이드에서 VPN gateway를 찾습니다.

   ![진단 설정에서 VPN gateway를 찾기 위한 선택 항목](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert3.png  "선택")

5. 진단을 켜려면 게이트웨이를 두 번 클릭 한 다음 **진단 켜기**를 선택 합니다.

   ![진단 켜기 선택](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert4.png  "선택")

6. 세부 정보를 입력 하 고 Log Analytics 및 **TunnelDiagnosticLog** **로 보내기** 가 선택 되어 있는지 확인 합니다. 3 단계에서 만든 Log Analytics 작업 영역을 선택 합니다.

   ![선택한 확인란](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert5.png  "선택")

7. 가상 네트워크 게이트웨이 리소스의 개요로 이동 하 고 **모니터링** 탭에서 **경고** 를 선택 합니다. 그런 다음 새 경고 규칙을 만들거나 기존 경고 규칙을 편집 합니다.

   ![새 경고 규칙을 만들기 위한 선택 항목](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert6.png  "선택")

   ![지점 및 사이트] 간 (./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert6.png  "선택")
8. Log Analytics 작업 영역 및 리소스를 선택 합니다.

   ![작업 영역 및 리소스에 대 한 선택](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert7.png  "선택")

9. **조건 추가**에서 신호 논리로 **사용자 지정 로그 검색** 을 선택 합니다.

   ![사용자 지정 로그 검색에 대 한 선택 항목](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert8.png  "선택")

10. **검색 쿼리** 텍스트 상자에 다음 쿼리를 입력합니다. < >의 값을 적절 하 게 바꿉니다.

    ```
    AzureDiagnostics |
      where Category  == "TunnelDiagnosticLog" and ResourceId == toupper("<RESOURCEID OF GATEWAY>") and TimeGenerated > ago(5m) and
      remoteIP_s == "<REMOTE IP OF TUNNEL>" and status_s == "Disconnected"
    ```

    임계값을 0으로 설정 하 고 **완료**를 선택 합니다.

    ![쿼리 입력 및 임계값 선택](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert9.png  "선택")

11. **규칙 만들기** 페이지의 **작업 그룹** 섹션에서 **새로 만들기** 를 선택 합니다. 세부 정보를 입력 하 고 **확인을**선택 합니다.

    ![새 작업 그룹에 대 한 세부 정보](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert10.png  "선택")

12. **규칙 만들기** 페이지에서 **작업 사용자 지정** 에 대 한 세부 정보를 입력 하 고 올바른 이름이 **작업 그룹 이름** 섹션에 표시 되는지 확인 합니다. **경고 규칙 만들기** 를 선택 하 여 규칙을 만듭니다.

    ![규칙을 만들기 위한 선택 항목](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert11.png  "선택")

## <a name="next-steps"></a>다음 단계

터널 메트릭에 대 한 경고를 구성 하려면 [VPN Gateway 메트릭에 대 한 경고 설정](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md)을 참조 하세요.
