---
title: Azure IoT Tools for Visual Studio Code를 사용하여 Azure IoT Hub 클라우드 디바이스 메시지 관리 | Microsoft Docs
description: 디바이스-클라우드 메시지를 모니터링하고 클라우드를 Azure IoT Hub의 디바이스 메시지로 보내기 위해 Azure IoT Tools for Visual Studio Code를 사용하는 방법을 알아봅니다.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 01/18/2019
ms.author: junhan
ms.openlocfilehash: 1289e9c8f8cfc9360c9b2325507b43bab3a69028
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67838502"
---
# <a name="use-azure-iot-tools-for-visual-studio-code-to-send-and-receive-messages-between-your-device-and-iot-hub"></a>Azure IoT Tools for Visual Studio Code를 사용하여 디바이스와 IoT Hub 간에 메시지 보내고 받기

![엔드투엔드 다이어그램](./media/iot-hub-vscode-iot-toolkit-cloud-device-messaging/e-to-e-diagram.png)

[Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)는 IoT Hub 관리 및 IoT 애플리케이션 개발을 더욱 용이하게 해주는 유용한 Visual Studio Code 확장입니다. 이 문서에서는 Azure IoT Tools for Visual Studio Code를 사용하여 디바이스와 IoT Hub 간에 메시지를 보내고 받는 방법에 중점을 둡니다.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="what-you-will-learn"></a>학습 내용

디바이스-클라우드 메시지를 모니터링하고 클라우드-디바이스 메시지를 보내기 위해 Azure IoT Tools for Visual Studio Code를 사용하는 방법을 알아봅니다. 디바이스-클라우드 메시지는 디바이스에서 수집한 다음 IoT Hub로 보내는 센서 데이터일 수 있습니다. 클라우드-디바이스 메시지는 IoT Hub에서 디바이스로 보내 사용자 디바이스에 연결된 LED를 깜박이는 명령일 수 있습니다.

## <a name="what-you-will-do"></a>수행할 사항

* Azure IoT Tools for Visual Studio Code를 사용하여 디바이스-클라우드 메시지를 모니터링합니다.

* Azure IoT Tools for Visual Studio Code를 사용하여 클라우드-디바이스 메시지를 전송합니다.

## <a name="what-you-need"></a>필요한 항목

* 활성화된 Azure 구독.

* 구독 중인 Azure IoT Hub

* [Visual Studio Code](https://code.visualstudio.com/)

* [VS Code 용 azure IoT 도구](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) 나 [Visual Studio Code에서이 링크를 열도록](vscode:extension/vsciot-vscode.azure-iot-tools)합니다.

## <a name="sign-in-to-access-your-iot-hub"></a>로그인하여 IoT Hub에 액세스

1. **탐색기**에서 VS Code를 보고 왼쪽 아래 모서리에서 **Azure IoT Hub 장치** 섹션을 확장합니다.

2. 상황에 맞는 메뉴에서 **IoT Hub 선택**을 클릭합니다.

3. 처음으로 Azure에 로그인할 수 있게 하는 팝업 메시지가 오른쪽 아래 모서리에 표시됩니다.

4. 로그인 후 Azure 구독 목록이 표시되면 IoT Hub 및 Azure 구독을 선택합니다.

5. 잠시 후 장치 목록이 **Azure IoT Hub 장치** 탭에 표시됩니다.

   > [!Note]
   > **IoT Hub 연결 문자열 설정**을 선택하여 설정을 완료할 수도 있습니다. 입력 된 **iothubowner** 팝업 창에서에 연결 하는 IoT 장치는 IoT hub에 대 한 정책 연결 문자열입니다.

## <a name="monitor-device-to-cloud-messages"></a>디바이스-클라우드 메시지 모니터링

디바이스에서 IoT Hub로 보낸 메시지를 모니터링하려면 다음 단계를 수행합니다.

1. 장치를 마우스 오른쪽 단추로 클릭 **기본 제공 이벤트 끝점 모니터링 시작**합니다.

2. 모니터링되는 메시지가 **OUTPUT** > **Azure IoT Hub Toolkit** 보기에 표시됩니다.

3. 모니터링을 중지 하려면 마우스 오른쪽 단추로 클릭 합니다 **출력** 살펴보고 선택 **기본 제공 이벤트 끝점 모니터링 중지**합니다.

## <a name="send-cloud-to-device-messages"></a>클라우드-장치 메시지 보내기

IoT Hub에서 디바이스로 메시지를 보내려면 다음 단계를 수행합니다.

1. 장치를 마우스 오른쪽 단추로 클릭하고 **장치에 C2D 메시지 보내기**를 선택합니다.

2. 입력 상자에 메시지를 입력합니다.

3. 결과가 **OUTPUT** > **Azure IoT Hub Toolkit** 보기에 표시됩니다.

## <a name="next-steps"></a>다음 단계

IoT 디바이스와 Azure IoT Hub 간에 디바이스-클라우드 메시지를 모니터링하고 클라우드-디바이스 메시지를 보내는 방법을 알아보았습니다.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]