---
title: Azure IoT Hub 디바이스 스트림(미리 보기) | Microsoft Docs
description: IoT Hub 디바이스 스트림 개요입니다.
author: rezasherafat
manager: briz
services: iot-hub
ms.service: iot-hub
ms.topic: conceptual
ms.date: 01/15/2019
ms.author: rezas
ms.openlocfilehash: 7ffe4a087ae94d6c96019cc045d3d7ff071780d4
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54830009"
---
# <a name="iot-hub-device-streams-preview"></a>IoT Hub 디바이스 스트림(미리 보기)

## <a name="overview"></a>개요
Azure IoT Hub ‘디바이스 스트림’은 다양한 클라우드-디바이스 통신 시나리오를 위한 보안 양방향 TCP 터널을 쉽게 만들 수 있도록 합니다. 디바이스 스트림은 디바이스와 서비스 엔드포인트 간의 프록시 역할을 하는 IoT Hub ‘스트리밍 엔드포인트’에 의해 조정됩니다. 이 설정은 아래 다이어그램에 설명되어 있으며, 디바이스가 네트워크 방화벽 뒤에 있거나 개인 네트워크 내부에 있을 때 특히 유용합니다. 따라서 IoT Hub 디바이스 스트림은 수신 또는 발신 네트워크 방화벽 포트를 광범위하게 열지 않고도 방화벽에 편리한 방식으로 IoT 디바이스에 연결해야 하는 고객의 요구를 처리하는 데 도움이 됩니다.

![대체 텍스트](./media/iot-hub-device-streams-overview/iot-hub-device-streams-overview.png "IoT Hub 디바이스 스트림 개요")


IoT Hub 디바이스 스트림을 사용하면 디바이스가 안전하게 유지되며, 포트 443을 통해 IoT Hub의 스트리밍 엔드포인트에 대한 아웃바운드 TCP 연결만 열면 됩니다. 스트림이 설정되면 서비스 쪽 및 디바이스 쪽 애플리케이션이 각각 프로그래밍 방식으로 WebSocket 클라이언트 개체에 액세스하여 서로 원시 바이트를 보내고 받을 수 있습니다. 이 터널에서 제공하는 안정성 및 순서 지정 보장은 TCP와 유사합니다.

## <a name="benefits"></a>이점
IoT Hub 디바이스 스트림은 다음과 같은 혜택을 제공합니다.
- **방화벽에 편리한 보안 연결:** 디바이스 또는 네트워크 경계에서 인바운드 방화벽 포트를 열지 않고도 서비스 엔드포인트에서 IoT 디바이스에 연결할 수 있습니다(포트 443을 통해 IoT Hub에 대한 아웃바운드 연결만 있으면 됨).

- **인증:** 터널의 디바이스 쪽과 서비스 쪽 모두, 해당 자격 증명을 사용하여 IoT Hub에 인증해야 합니다.

- **암호화:** 기본적으로 IoT Hub 디바이스 스트림은 TLS 지원 연결을 사용합니다. 이렇게 하면 애플리케이션이 암호화를 사용하는지 여부와 관계없이 트래픽이 항상 암호화됩니다.

- **연결의 단순성:** 디바이스 스트림을 사용하면 IoT 디바이스에 연결하기 위해 가상 사설망의 복잡한 설정을 수행할 필요가 없습니다.

- **TCP/IP 스택과의 호환성:** IoT Hub 디바이스 스트림은 TCP/IP 애플리케이션 트래픽을 수용할 수 있습니다. 즉, 광범위한 소유 프로토콜 및 표준 기반 프로토콜에서 이 기능을 활용할 수 있습니다.

- **개인 네트워크 설정의 사용 편의성:** 서비스에서 IP 주소가 아닌 디바이스 ID를 참조하여 디바이스에 연결할 수 있습니다. 이 기능은 디바이스가 개인 네트워크에 있고 개인 IP 주소를 사용하거나, 해당 IP 주소가 동적으로 할당되고 서비스 쪽에서 알 수 없는 경우에 유용합니다.

## <a name="device-stream-workflows"></a>디바이스 스트림 워크플로
서비스에서 디바이스 ID를 제공하여 디바이스에 대한 연결을 요청할 때 디바이스 스트림이 시작됩니다. 이 워크플로는 사용자가 SSH 또는 RDP 클라이언트 프로그램을 사용하여 디바이스에서 실행 중인 SSH 또는 RDP 서버에 원격으로 연결하려고 하는 SSH 및 RDP를 포함하여 클라이언트/서버 통신 패턴에 특히 적합합니다.

디바이스 스트림 생성 프로세스에는 디바이스, 서비스, IoT 허브의 기본 엔드포인트 및 스트리밍 엔드포인트 간 협상이 포함됩니다. IoT 허브의 기본 엔드포인트는 디바이스 스트림 생성을 오케스트레이션하는 반면, 스트리밍 엔드포인트는 서비스와 디바이스 간에 전달되는 트래픽을 처리합니다.

### <a name="device-stream-creation-flow"></a>디바이스 스트림 생성 흐름
SDK를 사용하여 디바이스 스트림을 프로그래밍 방식으로 만드는 과정에는 아래 그림에도 설명되어 있는 다음 단계가 포함됩니다.

![대체 텍스트](./media/iot-hub-device-streams-overview/iot-hub-device-streams-handshake.png "디바이스 스트림 핸드셰이크 프로세스")


1. 디바이스 애플리케이션은 디바이스에 대해 새 디바이스 스트림이 시작될 때 알림을 받기 위해 미리 콜백을 등록합니다. 이 단계는 일반적으로 디바이스가 부팅되고 IoT Hub에 연결할 때 수행됩니다.

2. 서비스 쪽 프로그램이 디바이스 ID(IP 주소가 ‘아님’)를 제공하여 필요할 때 디바이스 스트림을 시작합니다.

3. IoT 허브는 1단계에서 등록된 콜백을 호출하여 디바이스 쪽 프로그램에 알립니다. 디바이스는 스트림 시작 요청을 수락하거나 거부할 수 있습니다. 이 논리는 애플리케이션 시나리오에만 해당할 수 있습니다. 디바이스가 스트림 요청을 거부하면 IoT Hub가 그에 따라 서비스에 알립니다. 거부하지 않은 경우 아래 단계가 수행됩니다.

4. 디바이스가 포트 443을 통해 스트리밍 엔드포인트에 대한 보안 아웃바운드 TCP 연결을 만들고 WebSocket에 대한 연결을 업그레이드합니다. 스트리밍 엔드포인트의 URL 및 인증에 사용할 자격 증명은 둘 다 3단계에서 보낸 요청의 일부로 IoT Hub가 디바이스에 제공합니다.

5. 서비스는 디바이스의 스트림 수락 결과에 대한 알림을 받고 계속해서 스트리밍 엔드포인트에 대한 고유한 WebSocket을 만듭니다. 마찬가지로, IoT Hub에서 스트리밍 엔드포인트 URL 및 인증 정보를 받습니다.

위의 핸드셰이크 프로세스에서
- 핸드셰이크 프로세스는 60초 이내에 완료되어야 합니다(2-5단계). 완료되지 않으면 핸드셰이크가 시간 초과로 실패하고 그에 따라 서비스가 알림을 받습니다.

- 위의 스트림 생성 흐름이 완료되면 스트리밍 엔드포인트가 프록시로 작동하며, 해당 WebSocket을 통해 서비스와 디바이스 간에 트래픽을 전송합니다.

- 디바이스와 서비스 둘 다에 IoT Hub의 기본 엔드포인트와 포트 443을 통한 스트리밍 엔드포인트에 대한 아웃바운드 연결이 필요합니다. 이러한 엔드포인트의 URL은 IoT Hub 포털의 개요 탭에서 사용할 수 있습니다.

- 설정된 스트림의 안정성 및 순서 지정 보장은 TCP와 유사합니다.

- IoT Hub 및 스트리밍 엔드포인트에 대한 모든 연결은 TLS를 사용하며 암호화됩니다.

### <a name="termination-flow"></a>종료 흐름
게이트웨이에 대한 TCP 연결 중 하나가 서비스 또는 디바이스에 의해 끊기면 설정된 스트림이 종료됩니다. 이 작업은 디바이스 또는 서비스 프로그램에서 WebSocket을 닫아 자발적으로 수행되거나, 네트워크 연결 시간 초과 또는 프로세스 실패 시 비자발적으로 수행될 수 있습니다. 스트리밍 엔드포인트에 대한 디바이스 또는 서비스 연결이 종료되면 다른 TCP 연결도 (강제로) 종료되며, 서비스 및 디바이스가 필요한 경우 스트림을 다시 만들어야 합니다.


## <a name="connectivity-requirements"></a>연결 요구 사항

디바이스 스트림의 디바이스 쪽과 서비스 쪽 둘 다에서 IoT Hub 및 해당 스트리밍 엔드포인트에 대해 TLS 지원 연결을 설정할 수 있어야 합니다. 이렇게 하려면 포트 443을 통해 이 엔드포인트에 대한 아웃바운드 연결이 필요합니다. 이 엔드포인트와 연결된 호스트 이름은 아래 그림과 같이 IoT Hub의 ‘개요’ 탭에서 확인할 수 있습니다. ![대체 텍스트](./media/iot-hub-device-streams-overview/device-stream-portal.png "디바이스 스트림 엔드포인트")

또는 허브 속성 섹션 아래의 Azure CLI(특히 `property.hostname` 및 `property.deviceStreams` 키)를 사용하여 엔드포인트 정보를 검색할 수 있습니다.

```azurecli-interactive
az iot hub show --name <YourIoTHubName>
```

## <a name="troubleshoot-via-device-streams-activity-logs"></a>디바이스 스트림 활동 로그를 통해 문제 해결

IoT Hub에서 디바이스 스트림의 활동 로그를 수집하도록 Azure Log Analytics를 설정할 수 있습니다. 이 기능은 문제 해결 시나리오에서 매우 유용할 수 있습니다.

IoT Hub의 디바이스 스트림 활동에 대해 Azure Log Analytics를 구성하려면 아래 단계를 수행합니다.

1. IoT Hub의 ‘진단 설정’ 탭으로 이동한 다음, ‘진단 켜기’ 링크를 클릭합니다.

  ![대체 텍스트](./media/iot-hub-device-streams-overview/device-streams-diagnostics-settings.PNG "진단 로그 사용")


2. 진단 설정의 이름을 제공하고 ‘Log Analytics에 보내기’ 옵션을 선택합니다. 기존 Log Analytics 리소스를 선택하거나 새로 만들도록 안내됩니다. 또한 목록에서 *DeviceStreams*를 확인합니다.

    ![대체 텍스트](./media/iot-hub-device-streams-overview/device-streams-diagnostics.PNG "디바이스 스트림 로그 사용")

3. 이제 IoT Hub 포털의 ‘로그’ 탭에서 해당 디바이스 스트림 로그에 액세스할 수 있습니다. 디바이스 스트림 활동 로그는 `AzureDiagnostics` 테이블에 표시되고 `Category=DeviceStreams`로 설정됩니다.

    <p>
아래 그림과 같이 대상 디바이스의 ID와 작업 결과도 로그에서 제공됩니다.
    ![대체 텍스트](./media/iot-hub-device-streams-overview/device-streams-log-analytics.PNG "디바이스 스트림 로그 액세스")
    

## <a name="whitelist-device-streaming-endpoints"></a>디바이스 스트리밍 엔드포인트를 허용 목록에 추가

[앞서](#Overview) 설명한 대로 디바이스 스트림 시작 프로세스 중에 디바이스에서 IoT Hub 스트리밍 엔드포인트에 대한 아웃바운드 연결을 만듭니다. 디바이스 또는 해당 네트워크의 방화벽에서 포트 443을 통해 스트리밍 게이트웨이에 대한 아웃바운드 연결을 허용해야 합니다(TLS를 사용하여 암호화된 WebSocket 연결임).

디바이스 스트리밍 엔드포인트의 호스트 이름은 Azure IoT Hub 포털의 개요 탭에서 확인할 수 있습니다. ![대체 텍스트](./media/iot-hub-device-streams-overview/device-stream-portal.PNG "디바이스 스트림 엔드포인트")

또는 Azure CLI를 사용하여 이 정보를 확인할 수 있습니다.
```cmd/sh
az iot hub show --name tcpstreaming-preview
```

## <a name="sdk-availability"></a>SDK 가용성
각 스트림의 양쪽(디바이스 및 서비스 쪽)에서 IoT Hub SDK를 사용하여 터널을 설정합니다. 공개 미리 보기 중에는 고객이 다음 SDK 언어 중에서 선택할 수 있습니다.
- C 및 C# SDK는 디바이스 쪽에서 디바이스 스트림을 지원합니다.

- NodeJS 및 C# SDK는 서비스 쪽에서 디바이스 스트림을 지원합니다.

## <a name="iot-hub-device-stream-samples"></a>IoT Hub 디바이스 스트림 샘플
애플리케이션의 디바이스 스트림 사용을 보여 주기 위해 두 개의 샘플이 포함되었습니다. ‘에코’ 샘플은 SDK API를 호출하여 프로그래밍 방식의 디바이스 스트림 사용을 보여 줍니다. ‘로컬 프록시’ 샘플은 SDK 기능을 사용하여 디바이스 스트림을 통해 상업용 애플리케이션 트래픽(예: SSH, RDP 또는 웹)을 터널링하는 방법을 보여 줍니다.

### <a name="echo-sample"></a>에코 샘플
에코 샘플은 디바이스 스트림을 프로그래밍 방식으로 사용하여 서비스와 디바이스 애플리케이션 간에 바이트를 보내고 받는 방법을 보여 줍니다. 아래 링크를 사용하여 빠른 시작 가이드에 액세스할 수 있습니다. 다른 언어로 서비스 및 디바이스 프로그램을 사용할 수 있습니다. 예를 들어 C 디바이스 프로그램은 C# 서비스 프로그램에서 작동할 수 있습니다.

| SDK)    | 서비스 프로그램                                          | 디바이스 프로그램                                           |
|--------|----------------------------------------------------------|----------------------------------------------------------|
| C#     | [링크](quickstart-device-streams-echo-csharp.md) | [링크](quickstart-device-streams-echo-csharp.md) |
| NodeJS | [링크](quickstart-device-streams-echo-nodejs.md) | -                                                        |
| C      | -                                                        | [링크](quickstart-device-streams-echo-c.md)      |

### <a name="local-proxy-sample-for-ssh-or-rdp"></a>로컬 프록시 샘플(SSH 또는 RDP의 경우)
로컬 프록시 샘플은 클라이언트와 서버 프로그램 간의 통신을 포함하는 기존 애플리케이션의 트래픽을 터널링하는 방법을 보여 줍니다. 이 설정은 SSH 및 RDP와 같은 클라이언트/서버 프로토콜에 대해 작동합니다. 여기서 서비스 쪽은 SSH 또는 RDP 클라이언트 프로그램을 실행하는 클라이언트 역할을 하고, 디바이스 쪽은 SSH 디먼 또는 RDP 서버 프로그램을 실행하는 서버 역할을 합니다. 

이 섹션에서는 디바이스 스트림을 사용하여 디바이스 스트림을 통해 디바이스에 대한 SSH 시나리오를 지원하는 방법을 설명합니다. RDP 또는 기타 클라이언트/서버 프로토콜의 경우도 프로토콜의 해당 포트를 사용하여 유사하게 작동합니다.

이 설정은 아래 그림에 표시된 두 개의 ‘로컬 프록시’ 프로그램(‘디바이스-로컬 프록시’ 및 ‘서비스-로컬 프록시’)을 활용합니다. 로컬 프록시는 IoT Hub를 사용하여 [디바이스 스트림 시작 핸드셰이크](#Device-stream-creation-flow)를 수행하고 일반 클라이언트/서버 소켓 프로그래밍을 사용하여 SSH 클라이언트 및 SSH 디먼과 상호 작용해야 합니다.

![대체 텍스트](./media/iot-hub-device-streams-overview/iot-hub-device-streams-ssh.png "SSH/RDP에 대한 디바이스 스트림 프록시 설정")

1. 사용자는 서비스-로컬 프록시를 실행하여 디바이스에 대한 디바이스 스트림을 시작합니다.

2. 디바이스가 스트림 시작을 수락하고, 위에서 설명한 대로 IoT Hub의 스트리밍 엔드포인트에 대한 터널이 설정됩니다.

3. 디바이스-로컬 프록시가 디바이스의 포트 22에서 수신 대기 중인 SSH 디먼 엔드포인트에 연결합니다.

4. 서비스-로컬 프록시가 사용자로부터 새 SSH 연결을 기다리며 지정된 포트에서 수신 대기합니다(샘플에서 사용되는 포트 2222는 임의 포트임). 사용자가 SSH 클라이언트에서 localhost의 서비스-로컬 프록시 포트를 가리킵니다.

### <a name="notes"></a>메모
- 위 단계는 SSH 클라이언트(오른쪽)와 SSH 디먼(왼쪽) 간의 엔드투엔드 터널을 완료합니다. 

- 그림의 화살표는 엔드포인트 간에 연결이 설정되는 방향을 나타냅니다. 특히, 디바이스로 이동하는 인바운드 연결이 없습니다(방화벽에서 차단되는 경우가 많음).

- 서비스-로컬 프록시에서 포트 `2222` 사용은 임의로 선택한 것입니다. 사용 가능한 다른 포트를 사용하도록 프록시를 구성할 수 있습니다.

- 포트 `22` 선택은 프로토콜에 종속되며 이 경우 SSH에만 해당합니다. RDP의 경우 포트 `3389`를 사용해야 합니다. 이 설정은 제공된 샘플 프로그램에서 구성할 수 있습니다.

선택한 언어로 로컬 프록시 프로그램을 실행하는 방법에 대한 지침을 보려면 아래 링크를 사용합니다. 에코 샘플과 마찬가지로, 언어가 완전히 상호 운영되기 때문에 여러 언어로 디바이스-로컬 프록시와 서비스-로컬 프록시를 실행할 수 있습니다.

| SDK)    | 서비스-로컬 프록시                                       | 디바이스-로컬 프록시                                |
|--------|-----------------------------------------------------------|---------------------------------------------------|
| C#     | [링크](quickstart-device-streams-proxy-csharp.md)  | [링크](quickstart-device-streams-proxy-csharp.md) |
| NodeJS | [링크](quickstart-device-streams-proxy-nodejs.md)         | -                                                 |
| C      | -                                                         | [링크](quickstart-device-streams-proxy-c.md)      |

## <a name="next-steps"></a>다음 단계

디바이스 스트림에 대해 자세히 알아보려면 아래 링크를 사용합니다.

> [!div class="nextstepaction"]
> [IoT 프로그램(Channel 9)의 디바이스 스트림](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fchannel9.msdn.com%2FShows%2FInternet-of-Things-Show%2FAzure-IoT-Hub-Device-Streams&data=02%7C01%7Crezas%40microsoft.com%7Cc3486254a89a43edea7c08d67a88bcea%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636831125031268909&sdata=S6u9qiehBN4tmgII637uJeVubUll0IZ4p2ddtG5pDBc%3D&reserved=0)
> [디바이스 스트림 사용해보기 빠른 시작 가이드](/azure/iot-hub)