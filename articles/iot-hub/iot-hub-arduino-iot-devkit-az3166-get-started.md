---
title: IoT DevKit에서 클라우드로 - Azure IoT Hub에 IoT DevKit AZ3166 연결 | Microsoft Docs
description: 이 자습서에서는 Azure 클라우드 플랫폼으로 데이터를 보내도록 DevKit AZ3166을 설정하고 Azure IoT Hub에 연결하는 방법을 알아봅니다.
author: wesmc7777
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 06/25/2019
ms.author: wesmc
ms.openlocfilehash: 4017a3be5e03e1a9b85b4002b8069a1adc3a6b83
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67551579"
---
# <a name="connect-iot-devkit-az3166-to-azure-iot-hub"></a>Azure IoT Hub에 IoT DevKit AZ3166 연결

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Microsoft Azure 서비스를 활용하는 IoT(사물 인터넷) 솔루션을 개발하고 프로토타입하는 데 [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/)를 사용할 수 있습니다. 여기에 풍부한 주변 장치 및 센서, 오픈 소스 보드 패키지를와 다양 한 있는 Arduino 호환 보드 [샘플 갤러리](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/)합니다.

## <a name="what-you-learn"></a>학습 내용

* IoT Hub를 만들고 MXChip IoT DevKit용 디바이스를 등록하는 방법
* IoT DevKit를 Wi-fi에 연결 하 여 IoT Hub 연결 문자열을 구성 하는 방법입니다.
* IoT hub에 DevKit 센서의 원격 분석 데이터를 보내는 방법입니다.
* 개발 환경을 준비 하 고 IoT DevKit에 대 한 응용 프로그램을 개발 하는 방법입니다.

DevKit가 아직 없으세요? [DevKit 시뮬레이터](https://azure-samples.github.io/iot-devkit-web-simulator/)를 시도하거나 [DevKit를 구매](https://aka.ms/iot-devkit-purchase)합니다.

제공 되는 모든 DevKit 자습서에 대 한 소스 코드를 찾을 수 있습니다 합니다 [IoTDevEnvExamples](https://github.com/IoTDevEnvExamples) 리포지토리.

## <a name="what-you-need"></a>필요한 항목

* 마이크로 USB 케이블을 사용 하 여 MXChip IoT DevKit 보드. [지금 사용해 보세요](https://aka.ms/iot-devkit-purchase).
* Windows 10, 18.04 + Ubuntu 또는 macOS 10.10 +을 실행 하는 컴퓨터.
* 활성 Azure 구독. [30일 평가판 Microsoft Azure 계정](https://azureinfo.microsoft.com/us-freetrial.html) 활성화

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]
  
## <a name="prepare-your-hardware"></a>하드웨어 준비

컴퓨터에 다음 하드웨어를 연결합니다.

* DevKit 보드
* 마이크로 USB 케이블

![필수 하드웨어](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/hardware.jpg)

DevKit를 컴퓨터에 연결하려면 다음 단계를 수행합니다.

1. USB 끝을 컴퓨터에 연결합니다.

2. 마이크로 USB 끝을 DevKit에 연결합니다.

3. 전원의 녹색 LED는 연결을 확인합니다.

   ![하드웨어 연결](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect.jpg)

## <a name="quickstart-send-telemetry-from-devkit-to-an-iot-hub"></a>빠른 시작: DevKit에서 IoT Hub로 원격 분석을 전송

빠른 시작 미리 컴파일된 DevKit 펌웨어를 사용 하 여 IoT Hub에 원격 분석을 보냅니다. 를 실행 하기 전에 IoT hub를 만들고 허브에 장치를 등록 합니다.

### <a name="create-an-iot-hub"></a>IoT Hub 만들기

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="register-a-device"></a>디바이스 등록

연결을 위해 디바이스를 IoT Hub에 등록해야 합니다. 이 빠른 시작에서는 Azure Cloud Shell을 사용하여 시뮬레이션된 디바이스를 등록합니다.

1. Azure Cloud Shell에서 다음 명령을 실행하여 디바이스 ID를 만듭니다.

   **YourIoTHubName**: 이 자리 표시자를 IoT 허브용으로 선택한 이름으로 바꿉니다.

   **MyNodeDevice**: 등록 중인 디바이스의 이름입니다. 표시된 것처럼 **MyNodeDevice**를 사용하세요. 다른 디바이스 이름을 선택하는 경우 이 문서 전체에서 해당 이름을 사용해야 하고, 애플리케이션 예제에서 디바이스 이름을 업데이트한 후 실행해야 합니다.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyNodeDevice
    ```
   > [!NOTE]
   > 실행 오류가 발생 하는 경우 `device-identity`를 설치 합니다 [Azure CLI에 대 한 Azure IOT 확장](https://github.com/Azure/azure-iot-cli-extension/blob/dev/README.md) 대 한 자세한 내용은 합니다.
  
1. Azure Cloud Shell에서 다음 명령을 실행하여 방금 등록한 디바이스의 _디바이스 연결 문자열_을 가져옵니다.

   **YourIoTHubName**: 이 자리 표시자를 IoT 허브용으로 선택한 이름으로 바꿉니다.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyNodeDevice --output table
    ```

    다음과 같은 디바이스 연결 문자열을 기록해 둡니다.

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    이 값은 빠른 시작의 뒷부분에서 사용합니다.

### <a name="send-devkit-telemetry"></a>DevKit 원격 분석 전송

DevKit은 IoT hub에서 장치 특정 끝점에 연결 하 고 온도 및 습도 원격 분석을 보냅니다.

1. 최신 버전을 다운로드 [GetStarted 펌웨어](https://aka.ms/devkit/prod/getstarted/latest) IoT DevKit에 대 한 합니다.

1. USB 통해 컴퓨터에 연결 되었는지 IoT DevKit를 확인 합니다. 호출 USB 대용량 저장 장치가 파일 탐색기를 엽니다 **AZ3166**합니다.
    ![열기 Windows 탐색기](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/az3166-usb.png)

1. 끌어서 놓기 하며 대용량 저장 장치를 방금 다운로드 한 펌웨어를 자동으로 깜박입니다.
    ![펌웨어를 복사 합니다.](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/copy-firmware.png)

1. DevKit를의 단추를 길게 누른 **B**, 푸시 및 릴리스는 **재설정** 단추 및 분리 단추 **B**합니다. DevKit AP 모드로 전환 합니다. 를 확인 하려면 화면에 DevKit 및 구성 포털 IP 주소는 서비스 집합 id (SSID) 표시 됩니다.
    ![단추, 단추 B 및 SSID 다시 설정](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/wifi-ap.jpg)

    ![AP 모드 설정](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/set-ap-mode.gif)

1. 이전 단계에서 표시 하는 IoT DevKit SSID에 연결할 장치 (컴퓨터 또는 휴대폰)를 사용 하는 다른 Wi-fi에서 웹 브라우저를 사용 합니다. 암호를 묻는 경우 비워 둡니다.
    ![SSID를 연결 합니다.](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/connect-ssid.png)

1. 오픈 **192.168.0.1** 브라우저에서 합니다. IoT DevKit를 Wi-fi 암호를 입력 한 다음의 이전에 확인을 수행한 장치 연결 문자열을 붙여 넣습니다 연결 하려는 Wi-fi를 선택 합니다. 그런 다음 저장을 클릭합니다.
    ![구성 UI](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/configuration-ui.png)

    > [!NOTE]
    > IoT DevKit는 2.4ghz 네트워크만 지원합니다. 자세한 내용은 [FAQ](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#wi-fi-configuration)를 참조하세요.

1. 결과 페이지를 표시 하는 경우에 IoT DevKit WiFi 정보 및 장치 연결 문자열 저장 됩니다.
    ![구성 결과](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/configuration-ui-result.png)

    > [!NOTE]
    > Wi-Fi를 구성한 후 디바이스가 분리되는 경우에도 자격 증명은 해당 연결에 대해 디바이스에서 유지됩니다.

1. 잠시 후에 IoT DevKit 다시 부팅 됩니다. DevKit 화면에 DevKit에 대 한 IP 주소 온도 포함 하 여 원격 분석 데이터를 따르므로 및 습도 값 메시지 수를 사용 하 여 Azure IoT Hub로 보내는 것이 표시 됩니다.
    ![WiFi IP](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/wifi-ip.jpg)

    ![데이터 전송](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/sending-data.jpg)

1. Azure에 보낸 원격 분석 데이터를 확인 하려면 Azure Cloud Shell에서 다음 명령을 실행 합니다.
    ```bash
    az iot hub monitor-events --hub-name YourIoTHubName --output table
    ```

## <a name="prepare-the-development-environment"></a>개발 환경 준비

DevKit에 대 한 개발 환경을 준비 하려면 다음이 단계를 수행 합니다.

### <a name="install-visual-studio-code-with-azure-iot-tools-extension-package"></a>Azure IoT 도구 확장 패키지를 사용 하 여 Visual Studio Code 설치

1. [Arduino IDE](https://www.arduino.cc/en/Main/Software)를 설치합니다. Arduino 코드를 컴파일 및 업로드하는 데 필요한 도구 체인을 제공합니다.
    * **Windows**: Windows Installer 버전을 사용합니다. 앱 스토어에서 설치 하지 마세요.
    * **macOS**: 추출된 **Arduino.app**을 `/Applications` 폴더로 끌어다 놓습니다.
    * **Ubuntu**: `$HOME/Downloads/arduino-1.8.8` 같은 폴더에 압축을 풉니다.

2. 설치할 [Visual Studio Code](https://code.visualstudio.com/), 코드 완성 기능을 강력한 intellisense 사용 하 여 플랫폼 간 소스 코드 편집기 및 디버깅 지원 뿐만 아니라 다양 한 확장 기능 마켓플레이스에서 설치할 수 있습니다.

3. VS Code를 실행하고, 확장 마켓플레이스에서 **Arduino**를 검색하여 설치합니다. 이 확장은 Arduino 플랫폼에서 개발에 대한 향상된 환경을 제공합니다.
    ![Arduino 설치](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-arduino.png)

4. 확장 마켓플레이스에서 [Azure IoT Tools](https://aka.ms/azure-iot-tools)를 검색하여 설치합니다.
    ![Azure IoT Tools 설치](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-azure-iot-tools.png)

    또는 직접 링크를 사용 합니다.
    > [!div class="nextstepaction"]
    > [Azure IoT 도구 확장 팩 설치](vscode:extension/vsciot-vscode.azure-iot-tools)

    > [!NOTE]
    > Azure IoT 도구 확장 팩을 포함 합니다 [Azure IoT 장치 Workbench](https://aka.ms/iot-workbench) 는 개발 및 다양 한 IoT devkit 장치에서 디버그 하는 데 사용 됩니다. 합니다 [Azure IoT Hub Toolkit](https://aka.ms/iot-toolkit)도 Azure IoT 도구 확장 팩을 사용 하 여, 관리 및 Azure IoT Hub와 상호 작용 하는 데 사용 됩니다.

5. Arduino 설정을 사용하여 VS Code를 구성합니다.

    Visual Studio Code에서 **파일 > 기본 설정 > 설정**을 클릭합니다. 그런 후 **...** 를 클릭하고 **settings.json을 엽니다**.
    ![Azure IoT Tools 설치](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/user-settings-arduino.png)
    
    플랫폼에 따라 다음 줄을 추가하여 Arduino를 구성합니다. 

    * **Windows**:
      
        ```json
        "arduino.path": "C:\\Program Files (x86)\\Arduino",
        "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
        ```

    * **macOS**:

        ```json
        "arduino.path": "/Applications",
        "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
        ```

    * **Ubuntu**:
    
        **{username}** 을 사용자 이름 아래의 자리 표시자로 바꿉니다.

        ```json
        "arduino.path": "/home/{username}/Downloads/arduino-1.8.8",
        "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
        ```

6. `F1` 키를 클릭하여 명령 팔레트를 연 다음, **Arduino: 보드 관리자**를 입력하고 선택합니다. **AZ3166**을 검색하고 최신 버전을 설치합니다.
    ![DevKit SDK 설치](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-az3166-sdk.png)

### <a name="install-st-link-drivers"></a>ST-Link 드라이버 설치

[ST-Link/V2](https://www.st.com/en/development-tools/st-link-v2.html)는 IoT DevKit가 개발 머신과 통신하는 데 사용하는 USB 인터페이스입니다. DevKit에 컴파일된 장치 코드를 Windows에 설치 해야 합니다. 해당 OS의 단계에 따라 머신이 디바이스에 액세스할 수 있도록 허용합니다.

* **Windows**: USB 드라이버를 다운로드 및 설치 [STMicroelectronics 웹 사이트](https://www.st.com/en/development-tools/stsw-link009.html) 하거나 [직접 링크](https://aka.ms/stlink-v2-windows)합니다.
* **macOS**: macOS는 드라이버가 필요 없습니다.
* **Ubuntu**: 터미널에서 명령을 실행 하 고 로그 아웃 하 고 그룹 변경 내용을 적용 하려면 로그인 합니다.
    ```bash
    # Copy the default rules. This grants permission to the group 'plugdev'
    sudo cp ~/.arduino15/packages/AZ3166/tools/openocd/0.10.0/linux/contrib/60-openocd.rules /etc/udev/rules.d/
    sudo udevadm control --reload-rules

    # Add yourself to the group 'plugdev'
    # Logout and log back in for the group to take effect
    sudo usermod -a -G plugdev $(whoami)
    ```

개발 환경을 준비하고 구성하기 위한 작업이 모두 끝났습니다. 알려주세요 방금 실행 한 GetStarted 샘플을 빌드하십시오.

## <a name="build-your-first-project"></a>첫 번째 프로젝트 빌드

### <a name="open-sample-code-from-sample-gallery"></a>샘플 갤러리에서 샘플 코드 열기

다양 한 갤러리를 포함 하는 IoT DevKit 샘플 익히는 데 사용할 수 있는 다양 한 Azure 서비스에 DevKit을 연결 합니다.

1. IoT DevKit가 컴퓨터에 연결되어 있지 **않은지** 확인합니다. VS Code를 시작하고 DevKit를 컴퓨터에 연결합니다.

1. `F1` 키를 클릭하여 명령 팔레트를 열고, **Azure IoT Device Workbench: 예제 열기...** 를 입력하고 선택합니다. 보드로 **IoT DevKit**를 선택합니다.

1. IoT Workbench 예제 페이지에서 **시작**을 찾아서 **샘플 열기**를 클릭합니다. 기본 경로를 선택하여 샘플 코드를 다운로드합니다.
    ![샘플 열기](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/open-sample.png)

### <a name="provision-azure-iot-hub-and-device"></a>프로 비전 Azure IoT Hub 및 장치

Azure IoT Hub를 프로 비전 하 고 Azure portal에서 장치를 대신 개발 환경을 사용 하지 않고 VS Code에서 수행할 수 있습니다.

1. 새로 열린 프로젝트 창에서 `F1` 키를 클릭하여 명령 팔레트를 열고, **Azure IoT Device Workbench: Azure 서비스 프로비전...** 을 입력하고 선택합니다. 단계별 지침에 따라 Azure IoT Hub 프로비전을 완료하고 IoT Hub 디바이스를 만듭니다.
    ![프로 비전 명령](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/provision.png)

    > [!NOTE]
    > Azure 로그인 하지 않은 경우. 로그인에 대 한 팝업 알림을 수행 합니다.

1. 사용할 구독을 선택합니다.
    ![Sub를 선택 합니다.](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-subscription.png)

1. 다음을 선택 하거나 새로 만듭니다 [리소스 그룹](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#terminology)합니다.
    ![리소스 그룹 선택](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-resource-group.png)

1. 지정한 리소스 그룹을 선택 하거나 새 Azure IoT Hub를 만드는 가이드를 따릅니다.
    ![IoT Hub 단계를 선택 합니다.](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/iot-hub-provision.png)

    ![IoT Hub를 선택 합니다.](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-iot-hub.png)

    ![선택한 IoT Hub](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/iot-hub-selected.png)

1. 출력 창에 프로 비전 하 여 Azure IoT Hub 나타납니다 ![IoT Hub는 프로 비전](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/iot-hub-provisioned.png)

1. 선택 하거나 새 장치를 프로 비전 하는 Azure IoT Hub에 만듭니다.
    ![IoT 장치는 단계를 선택 합니다.](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/iot-device-provision.png)

    ![프로 비전 된 IoT 장치를 선택 합니다.](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-iot-device.png)

1. 이제 프로 비전 하는 Azure IoT Hub 및 장치를 만들 수 있습니다. 또한 장치 연결 문자열을 나중에 IoT DevKit를 구성 하는 것에 대 한 VS Code에서 저장 됩니다.
    ![프로 비전 완료](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/provision-done.png)

### <a name="configure-and-compile-device-code"></a>구성 및 장치 코드 컴파일

1. 오른쪽 아래의 상태 표시줄에서 **MXCHIP AZ3166**이 보드로 선택되고 **STMicroelectronics**가 있는 직렬 포트가 사용되는지 확인합니다.
    ![보드 및 COM 선택](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-com.png)

1. `F1` 키를 클릭하여 명령 팔레트를 열고, **Azure IoT Device Workbench: 디바이스 설정 구성...** 을 입력하여 선택한 다음, **디바이스 연결 문자열 구성 > IoT Hub 디바이스 연결 문자열 선택**을 선택합니다.

1. DevKit에서 **A 단추**를 누른 상태로 **다시 설정** 단추를 눌렀다 떼고, **A 단추**를 뗍니다. DevKit가 구성 모드로 전환되고 연결 문자열이 저장됩니다.
    ![연결 문자열](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connection-string.png)

1. `F1` 키를 다시 클릭하고, **Azure IoT Device Workbench: 디바이스 코드 업로드**를 입력하고 선택합니다. 컴파일을 시작하고 코드를 DevKit로 업로드합니다.
    ![Arduino 업로드](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/arduino-upload.png)

DevKit는 다시 부팅하고 코드를 실행하기 시작합니다.

> [!NOTE]
> 오류 또는 중단이 있으면 언제든지 명령을 다시 실행하여 복구할 수 있습니다.

## <a name="test-the-project"></a>프로젝트 테스트

### <a name="view-the-telemetry-sent-to-azure-iot-hub"></a>Azure IoT Hub로 전송된 원격 분석 보기

상태 표시줄의 전원 플러그 아이콘을 클릭하여 직렬 모니터를 엽니다. ![직렬 모니터](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/serial-monitor.png)

다음과 같은 결과가 표시되면 샘플 애플리케이션은 성공적으로 실행됩니다.

* 직렬 모니터는 IoT Hub로 전송된 메시지를 표시합니다.
* MXChip IoT DevKit의 LED가 깜박입니다.

![직렬 모니터 출력](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/result-serial-output.png)

### <a name="view-the-telemetry-received-by-azure-iot-hub"></a>Azure IoT Hub에서 수신한 원격 분석 보기

[Azure IoT 도구](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)를 사용하여 IoT Hub에서 디바이스-클라우드(D2C) 메시지를 모니터링할 수 있습니다.

1. [Azure Portal](https://portal.azure.com/)에 로그인하고, 이전에 만든 IoT Hub를 찾습니다.
    ![Azure Portal](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/azure-iot-hub-portal.png)

1. **공유 액세스 정책** 창에서 **iothubowner** 정책을 클릭하고, IoT Hub의 연결 문자열을 기록해 둡니다.
    ![Azure IoT Hub 연결 문자열](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/azure-portal-conn-string.png)

1. VS Code에서 `F1` 키를 클릭하고, **Azure IoT Hub: IoT Hub 연결 문자열 설정**을 입력하고 선택합니다. 연결 문자열을 복사합니다.
    ![Azure IoT Hub 연결 문자열 설정](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/set-iothub-connection-string.png)

1. 확장 된 **AZURE IOT HUB 장치** 장치 이름을 생성 하 고 선택 창 오른쪽을 마우스 오른쪽 단추로 클릭 **기본 제공 이벤트 끝점 모니터링 시작**합니다.
    ![D2C 메시지 모니터링](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/monitor-d2c.png)

1. **출력** 창에서, IoT Hub로 들어오는 D2C 메시지를 볼 수 있습니다.
    ![D2C 메시지](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/d2c-output.png)

## <a name="review-the-code"></a>코드 검토

`GetStarted.ino` 주 Arduino 스케치 파일입니다.

![D2C 메시지](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/code.png)

Azure IoT Hub에 장치 원격 분석이 전송 되는 방식을 보려면를 `utility.cpp` 동일한 폴더에 파일입니다. 뷰 [API 참조](https://microsoft.github.io/azure-iot-developer-kit/docs/apis/arduino-language-reference/) 에 IoT DevKit에서 센서 및 주변 장치를 사용 하는 방법을 알아봅니다.

`DevKitMQTTClient` 의 래퍼인 사용 합니다 **iothub_client** 에서 [Microsoft Azure IoT Sdk 및 C 라이브러리](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client) Azure IoT Hub와 상호 작용 하 합니다.

## <a name="problems-and-feedback"></a>문제 및 피드백

문제가 있는 경우 [IoT DevKit FAQ](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/)에서 해결 방법을 찾아보거나 [Gitter](https://gitter.im/Microsoft/azure-iot-developer-kit)에서 문의할 수 있습니다. 또한 이 페이지에서 의견을 남겨 피드백을 제공해 주실 수 있습니다.

## <a name="next-steps"></a>다음 단계

IoT Hub에 MXChip IoT DevKit를 성공적으로 연결하고 캡처된 센서 데이터를 IoT Hub에 전송했습니다.

[!INCLUDE [iot-hub-get-started-az3166-next-steps](../../includes/iot-hub-get-started-az3166-next-steps.md)]
