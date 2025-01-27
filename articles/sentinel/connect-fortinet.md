---
title: Azure 미리 보기 Sentinel Fortinet 데이터 연결할 | Microsoft Docs
description: Azure Sentinel Fortinet 데이터 연결 하는 방법에 알아봅니다.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: add92907-0d7c-42b8-a773-f570f2d705ff
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2019
ms.author: rkarlin
ms.openlocfilehash: f3ab4861e874074e7de059c7c50064d53749748c
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67611293"
---
# <a name="connect-your-fortinet-appliance"></a>Fortinet 어플라이언스에서 연결

> [!IMPORTANT]
> Azure Sentinel은 현재 공개 미리 보기로 제공됩니다.
> 이 미리 보기 버전은 서비스 수준 약정 없이 제공 됩니다. 프로덕션 워크 로드에 대 한 권장 하지 않습니다. 특정 기능이 지원되지 않거나 기능이 제한될 수 있습니다. 자세한 내용은 [Microsoft Azure 미리 보기에 대 한 사용 약관](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)합니다.

로 Syslog 이벤트 CEF (일반적인 형식) 로그 파일을 저장 하 여 모든 Fortinet 어플라이언스로 Azure Sentinel를 연결할 수 있습니다. Azure Sentinel와 통합을 사용 하 여 실행할 수 있습니다 쉽게 분석 및 쿼리 로그 파일 데이터에서 Fortinet에서. Azure Sentinel CEF 데이터를 수집 하는 방법에 대 한 자세한 내용은 참조 하세요. [CEF 연결 어플라이언스](connect-common-event-format.md)합니다.

> [!NOTE]
> 데이터를 실행 하는 Azure Sentinel 영역의 지리적 위치에 저장 됩니다.

## <a name="step-1-connect-your-fortinet-appliance-by-using-an-agent"></a>1단계: 에이전트를 사용 하 여 Fortinet 어플라이언스에서 연결

Fortinet 어플라이언스에서 연결 하기 위해 Azure Sentinel 어플라이언스 및 Azure Sentinel 간의 통신을 지원 하기 위해 전용된 VM 또는 온-프레미스 컴퓨터에서 에이전트를 배포 합니다. 자동 또는 수동으로 에이전트를 배포할 수 있습니다. 자동 배포만 전용된 컴퓨터에 Azure에서 만든 새 VM 인 경우 제공 됩니다.

다른 클라우드의 VM에서 기존 Azure VM에서 수동으로 또는 온-프레미스 컴퓨터에 에이전트를 배포할 수도 있습니다.

두 옵션의 네트워크 다이어그램을 참조 하세요 [데이터 원본 연결](connect-data-sources.md#agent-options)합니다.

### <a name="deploy-the-agent-in-azure"></a>Azure에서 에이전트 배포

1. Sentinel Azure portal에서 선택 **데이터 커넥터** 어플라이언스 유형을 선택 하 고 있습니다.

1. 아래 **Linux Syslog 에이전트 구성을**:
   - 선택할 **자동 배포** 는 Sentinel Azure 에이전트를 사용 하 여 미리 설치 되며 모든 구성 필요에 따라 앞에서 설명한 대로 포함 하 여 새 컴퓨터를 만들려는 경우입니다. 선택 **자동 배포** > **자동 에이전트 배포**합니다. 작업 영역에 자동으로 연결 된 전용된 VM에 대 한 구매 페이지가 표시 됩니다. VM이를 **표준 D2s v3 (2 개 Vcpu, 8GB 메모리)** 있고 공용 IP 주소입니다.
      1. 에 **사용자 지정 배포** 페이지에서 세부 정보를 제공 하 고, 사용자 이름 및 암호를 입력 하 고 사용 약관에 동의 하면 구매할 VM.
      1. 연결 페이지에서 나열 된 설정을 사용 하 여 로그를 전송 하기 위해 어플라이언스를 구성 합니다. 제네릭 일반적인 이벤트 형식 커넥터에 대해 이러한 설정을 사용 합니다.
         - Protocol = UDP
         - Port = 514
         - 기능 = 로컬 4
         - Format = CEF
   - 선택할 **수동 배포** 기존 VM에 Azure Sentinel 에이전트가 설치 된 Linux 컴퓨터 전용된으로 사용 하려는 경우. 
      1. 아래 **Syslog 에이전트 다운로드 및 설치**를 선택 **Azure Linux 가상 머신에**합니다. 
      1. 에 **Virtual machines** 화면을 사용 하 고 선택 하려는 컴퓨터를 선택 **Connect**합니다.
      1. 커넥터 화면에서 아래 **구성 및 정방향 Syslog**설정에 Syslog 디먼 인지 **rsyslog.d** 또는 **syslog ng**합니다. 
      1. 다음이 명령을 복사 하 고 어플라이언스에서 실행:
          - Rsyslog.d 선택한 경우:
              
            1. Syslog 디먼을 시설 local_4에서 수신 대기 하는 데 포트 25226으로 사용 하 여 Azure Sentinel 에이전트 Syslog 메시지 보내기에 알려 줍니다. 이 명령을 사용 합니다. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226\n\n:msg, contains, \"Fortinet\"  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            1. 다운로드 및 설치 합니다 [security_events config 파일](https://aka.ms/asi-syslog-config-file-linux) 포트 25226으로에서 수신 대기할 Syslog 에이전트를 구성 하는 합니다. 이 명령을 사용 하 여: `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` 여기서 {0} 작업 영역의 GUID로 바꿔야 합니다.
            1. 이 명령을 사용 하 여 syslog 디먼을 다시 시작 합니다. `sudo service rsyslog restart`
             
          - Ng syslog를 선택한 경우:

              1. Syslog 디먼을 시설 local_4에서 수신 대기 하는 데 포트 25226으로 사용 하 여 Azure Sentinel 에이전트 Syslog 메시지 보내기에 알려 줍니다. 이 명령을 사용 합니다. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Fortinet\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
              1. 다운로드 및 설치 합니다 [security_events config 파일](https://aka.ms/asi-syslog-config-file-linux) 포트 25226으로에서 수신 대기할 Syslog 에이전트를 구성 하는 합니다. 이 명령을 사용 하 여: `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` 여기서 {0} 작업 영역의 GUID로 바꿔야 합니다.
              1. 이 명령을 사용 하 여 syslog 디먼을 다시 시작 합니다. `sudo service syslog-ng restart`
      1. 이 명령을 사용 하 여 Syslog 에이전트를 다시 시작 합니다. `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. 이 명령을 실행 하 여 에이전트 로그에 오류가 있는지 확인 합니다. `tail /var/opt/microsoft/omsagent/log/omsagent.log`

### <a name="deploy-the-agent-on-an-on-premises-linux-server"></a>온-프레미스 Linux 서버에 에이전트 배포

Azure를 사용 하지 않는 경우 전용된 Linux 서버에서 실행 되도록 Azure Sentinel 에이전트를 수동으로 배포 합니다.

1. Sentinel Azure portal에서 선택 **데이터 커넥터** 어플라이언스 유형을 선택 하 고 있습니다.
1. 아래에 있는 전용된 Linux VM을 만들려면 **Linux Syslog 에이전트 구성을** 선택 **수동 배포**합니다.

    1. 아래 **Syslog 에이전트 다운로드 및 설치**를 선택 **비 Azure Linux 머신**합니다.
    1. 에 **직접 에이전트** 열립니다를 선택 하는 화면 **Agent for Linux** 에이전트를 다운로드 하거나 Linux 컴퓨터에 다운로드 하려면이 명령을 실행 하려면: `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w {workspace GUID} -s gehIk/GvZHJmqlgewMsIcth8H6VqXLM9YXEpu0BymnZEJb6mEjZzCHhZgCx5jrMB1pVjRCMhn+XTQgDTU3DVtQ== -d opinsights.azure.com`

       1. 커넥터 화면에서 아래 **구성 및 정방향 Syslog**설정에 Syslog 디먼 인지 **rsyslog.d** 또는 **syslog ng**합니다.
       1. 다음이 명령을 복사 하 고 어플라이언스에서 실행:

          - Rsyslog를 선택한 경우:

            1. Syslog 디먼을 시설 local_4에서 수신 대기할 포트 25226으로 사용 하 여 Azure Sentinel 에이전트 Syslog 메시지를 보내는 알려 줍니다. 이 명령을 사용 합니다. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226\n\n:msg, contains, \"Fortinet\"  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            1. 다운로드 및 설치 합니다 [security_events config 파일](https://aka.ms/asi-syslog-config-file-linux) 포트 25226으로에서 수신 대기할 Syslog 에이전트를 구성 하는 합니다. 이 명령을 사용 하 여: `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` 여기서 {0} 작업 영역의 GUID로 바꿔야 합니다.
            1. 이 명령을 사용 하 여 syslog 디먼을 다시 시작 합니다. `sudo service rsyslog restart`

          - Ng syslog를 선택한 경우:

            1. Syslog 디먼을 시설 local_4에서 수신 대기 하는 데 포트 25226으로 사용 하 여 Azure Sentinel 에이전트 Syslog 메시지 보내기에 알려 줍니다. 이 명령을 사용 합니다. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Fortinet\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
            1. 다운로드 및 설치 합니다 [security_events config 파일](https://aka.ms/asi-syslog-config-file-linux) 포트 25226으로에서 수신 대기할 Syslog 에이전트를 구성 하는 합니다. 이 명령을 사용 하 여: `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` 여기서 {0} 작업 영역의 GUID로 바꿔야 합니다.
            1. 이 명령을 사용 하 여 syslog 디먼을 다시 시작 합니다. `sudo service syslog-ng restart`

      1. 이 명령을 사용 하 여 Syslog 에이전트를 다시 시작 합니다. `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. 이 명령을 실행 하 여 에이전트 로그에 오류가 있는지 확인 합니다. `tail /var/opt/microsoft/omsagent/log/omsagent.log`
 
## <a name="step-2-forward-fortinet-logs-to-the-syslog-agent"></a>2단계: Syslog 에이전트 Fortinet 로그 전달

Fortinet Syslog 에이전트를 통해 Azure 작업 영역에 CEF 형식의 Syslog 메시지를 전달 하도록 구성 합니다.

1. Fortinet 어플라이언스 CLI를 열고 다음 명령을 실행 합니다.

        config log syslogd setting
        set format cef
        set facility <facility_name>
        set port 514
        set reliable disable
        set server <ip_address_of_Receiver>
        set status enable
        end

    - 서버를 교체 **ip 주소** 에이전트의 IP 주소를 사용 하 여 합니다.
    - 설정할 **facility_name** 에이전트에서 구성 된 기능을 사용 하도록 합니다. 기본적으로 에이전트를이 설정 local4 하 합니다.
    - 설정 된 **syslog 포트** 하 **514** 또는 에이전트에서 설정 된 포트입니다.
    - CEF 형식의 초기 FortiOS 버전에서을 사용 하려면 명령 집합을 실행 해야 **csv 사용 하지 않도록 설정**합니다.
 
   > [!NOTE] 
   > 자세한 내용은 이동 합니다 [Fortinet 문서 라이브러리](https://aka.ms/asi-syslog-fortinet-fortinetdocumentlibrary)합니다. 버전을 선택 하 고 사용 합니다 **안내서** 및 **로그 메시지 참조**합니다.

 Azure Monitor Log Analytics에서 관련 스키마를 사용 하 여 Fortinet 이벤트, 검색 `CommonSecurityLog`합니다.


## <a name="step-3-validate-connectivity"></a>3단계: 연결 유효성 검사

로그를 Log Analytics에 나타나기 시작 될 때까지 20 분 정도 걸릴 수 있습니다. 

1. 올바른 기능을 사용 해야 합니다. 시설 및 Azure Sentinel 어플라이언스에서에서 동일 해야 합니다. Azure Sentinel에서 사용 중이 고 파일의 수정 기능 파일을 확인할 수 있습니다 `security-config-omsagent.conf`합니다. 

2. 오른쪽 포트 Syslog 에이전트에 로그를 가져오고 있는지 확인 합니다. 이 명령은 Syslog 에이전트 컴퓨터에서 실행: `tcpdump -A -ni any  port 514 -vv`합니다. 이 명령은 Syslog 컴퓨터에 장치에서 스트리밍 로그를 보여 줍니다. 로그 원본 어플라이언스 오른쪽 포트에 오른쪽 시설에서에서 수신 되는 있는지 확인 합니다.

3. 보낼 로그를 준수 하는지 확인 [RFC 5424](https://tools.ietf.org/html/rfc542)합니다.

4. 컴퓨터의 Syslog 에이전트를 실행 하는 않은지 포트 514 및 25226 열고 수신 명령을 사용 하 여 `netstat -a -n:`입니다. 이 명령을 사용 하는 방법에 대 한 자세한 내용은 참조 하세요. [netstat(8)-Linux 매뉴얼 페이지](https://linux.die.net/man/8/netstat)합니다. 이 수신 하는 경우 올바르게 표시 합니다.

   ![Azure Sentinel 포트](./media/connect-cef/ports.png) 

5. 로그를 전송 하는 포트 514에서 수신 하도록 디먼을 설정 되어 있는지 확인 합니다.
    - Rsyslog의 경우:<br>확인 파일 `/etc/rsyslog.conf` 이 구성을 포함 합니다.

           # provides UDP syslog reception
           module(load="imudp")
           input(type="imudp" port="514")
        
           # provides TCP syslog reception
           module(load="imtcp")
           input(type="imtcp" port="514")

      자세한 내용은 참조 하세요. [imudp: UDP Syslog 입력된 모듈](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imudp.html#imudp-udp-syslog-input-module) 고 [imtcp: TCP Syslog 입력된 모듈](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imtcp.html#imtcp-tcp-syslog-input-module)합니다.

   - Syslog-ng:<br>확인 파일 `/etc/syslog-ng/syslog-ng.conf` 이 구성을 포함 합니다.

           # source s_network {
            network( transport(UDP) port(514));
             };
     자세한 내용은 참조 하세요. [imudp: UDP Syslog 입력된 모듈](https://rsyslog.readthedocs.io/en/latest/configuration/modules/imudp.html) 하 고 [syslog ng 오픈 소스 버전 3.16-관리 가이드](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.16/administration-guide/19#TOPIC-956455)합니다.

1. Syslog 디먼 및 에이전트 간의 통신 인지 확인 합니다. 이 명령은 Syslog 에이전트 컴퓨터에서 실행: `tcpdump -A -ni any  port 25226 -vv`합니다. 이 명령은 Syslog 컴퓨터에 장치에서 스트리밍 로그를 보여 줍니다. 에이전트에서 로그도 수신 되는 있는지 확인 합니다.

6. 성공적인 결과 제공 하는 모두 해당 명령의 경우에 Log Analytics 로그 도착 하는 경우 참조를 확인 합니다. 이러한 어플라이언스에서 스트리밍되는 모든 이벤트에서 Log Analytics에서 원시 형태로 나타나는 `CommonSecurityLog` 형식입니다.

7. 오류가 있는지 확인 하거나 로그 도착 하지 하는 경우 확인 `tail /var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`합니다. 로그 형식 불일치 오류가 표시 되는 경우 이동할 `/etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` 파일 및 `security_events.conf`합니다. 로그와이 파일에 나오는 regex 형식 일치 하는지 확인 하십시오.

8. 에 Syslog 메시지 기본 크기는 2048 바이트 (2KB)로 제한 해야 합니다. 로그에 너무 긴 경우이 명령을 사용 하 여 security_events.conf를 업데이트 합니다. `message_length_limit 4096`

10. 어떤 유형의 수를 사용 하는 기능을 설정 및 검색 단어 Fortinet 로그의 로그를 설정 하는 Syslog 디먼에 따라이 명령을 실행 하는 Fortinet 로그 에이전트에 의해 수신 되지 않습니다.
       - rsyslog.d: `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226\n\n:msg, contains, \"Fortinet\"  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`

     이 명령을 사용 하 여 Syslog 디먼을 다시 시작 합니다. `sudo service rsyslog restart`
       - syslog-ng: `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Fortinet\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
      
     이 명령을 사용 하 여 Syslog 디먼을 다시 시작 합니다. `sudo service syslog-ng restart`

## <a name="next-steps"></a>다음 단계
이 문서에서는 Azure Sentinel Fortinet 어플라이언스에 연결 하는 방법을 알아보았습니다. Azure Sentinel에 대한 자세한 내용은 다음 문서를 참조하세요.
- 에 대해 알아봅니다 하는 방법 [데이터에 잠재적 위협을 파악](quickstart-get-visibility.md)합니다.
- 시작 [사용 하 여 Azure Sentinel 위협을 감지 하도록](tutorial-detect-threats.md)합니다.

