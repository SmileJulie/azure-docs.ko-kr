---
title: Azure IoT Hub 디바이스 스트림을 통해 C에서 디바이스 앱과 통신(미리 보기) | Microsoft Docs
description: 이 빠른 시작에서는 디바이스 스트림을 통해 IoT 디바이스와 통신하는 C 서비스 쪽 애플리케이션을 실행합니다.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: robinsh
ms.openlocfilehash: 4b6f987c68f9fe3ef95c82017b7d8be1d83083ea
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446124"
---
# <a name="quickstart-communicate-to-a-device-application-in-c-via-iot-hub-device-streams-preview"></a>빠른 시작: IoT Hub 디바이스 스트림을 통해 C에서 디바이스 애플리케이션과 통신(미리 보기)

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

Azure IoT Hub는 현재 디바이스 스트림을 [미리 보기 기능](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)으로 지원합니다.

[IoT Hub 디바이스 스트림](iot-hub-device-streams-overview.md)은 서비스 및 디바이스 애플리케이션이 안전하고 방화벽 친화적인 방식으로 통신할 수 있도록 합니다. 공개 미리 보기 동안 C SDK는 디바이스 쪽의 디바이스 스트림만 지원합니다. 따라서 이 빠른 시작에서는 디바이스 쪽 애플리케이션을 실행하는 지침만 설명합니다. 함께 제공되는 서비스 쪽 애플리케이션을 실행하려면 다음을 참조하세요.
 
   * [IoT Hub 디바이스 스트림을 통해 C#에서 디바이스 앱과 통신](./quickstart-device-streams-echo-csharp.md)
   * [IoT Hub 디바이스 스트림을 통해 Node.js에서 디바이스 앱과 통신](./quickstart-device-streams-echo-nodejs.md)

이 빠른 시작의 디바이스 쪽 C 애플리케이션에는 다음과 같은 기능이 있습니다.

* IoT 디바이스에 디바이스 스트림을 설정합니다.
* 서비스 쪽 애플리케이션에서 전송된 데이터를 받아서 다시 에코합니다.

이 코드는 디바이스 스트림의 초기화 프로세스 및 데이터를 보내고 받는 데 사용하는 방법을 보여줍니다.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure 구독이 아직 없는 경우 시작하기 전에 [무료 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 을 만듭니다.

## <a name="prerequisites"></a>필수 조건

* 디바이스 스트림 미리 보기는 현재 다음 Azure 지역에서 만든 IoT Hub만 지원합니다.

  * 미국 중부
  * 미국 중부 EUAP

* [C++를 사용한 데스크톱 개발](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) 워크로드를 사용하도록 설정하고 [Visual Studio 2017](https://www.visualstudio.com/vs/)을 설치합니다.

* 최신 버전의 [Git](https://git-scm.com/download/)을 설치합니다.

* 다음 명령을 실행하여 Azure CLI용 Azure IoT 확장을 Cloud Shell 인스턴스에 추가합니다. IoT 확장은 IoT Hub, IoT Edge 및 IoT DPS(Device Provisioning Service) 관련 명령을 Azure CLI에 추가합니다.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="prepare-the-development-environment"></a>개발 환경 준비

이 빠른 시작에서는 [C용 Azure IoT 디바이스 SDK](iot-hub-device-sdk-c-intro.md)를 사용합니다. GitHub에서 [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c)를 복제하고 빌드하는 데 사용되는 개발 환경을 준비합니다. GitHub의 SDK에는 이 빠른 시작에 사용되는 샘플 코드가 포함되어 있습니다.

1. [CMake 빌드 시스템](https://cmake.org/download/)을 다운로드합니다.

    CMake 설치를 시작하려면 Visual Studio 필수 구성 요소(Visual Studio 및 *C++를 사용한 데스크톱 개발* 워크로드)가 머신에 설치되어 있어야 합니다. 필수 구성 요소가 준비되고 다운로드가 확인되면 CMake 빌드 시스템을 설치할 수 있습니다.

2. 명령 프롬프트 또는 Git Bash 셸을 엽니다. 다음 명령을 실행하여 [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) GitHub 리포지토리를 복제합니다.

    ```
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive -b public-preview
    ```

    이 작업은 몇 분 정도 걸립니다.

3. 다음 명령과 같이 Git 리포지토리의 루트 디렉터리에 *cmake* 하위 디렉터리를 만든 다음, 해당 폴더로 이동합니다.

    ```
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

4. *cmake* 디렉터리에서 다음 명령을 실행하여 개발 클라이언트 플랫폼과 관련된 SDK 버전을 빌드합니다.

   * Linux:

      ```bash
      cmake ..
      make -j
      ```

   * Windows에서 Visual Studio 2015 또는 2017에 대해 개발자 명령 프롬프트에서 다음 명령을 실행합니다. 시뮬레이션된 디바이스용 Visual Studio 솔루션이 *cmake* 디렉터리에 생성됩니다.

      ```cmd
      rem For VS2015
      cmake .. -G "Visual Studio 14 2015"

      rem Or for VS2017
      cmake .. -G "Visual Studio 15 2017"

      rem Then build the project
      cmake --build . -- /m /p:Configuration=Release
      ```

## <a name="create-an-iot-hub"></a>IoT Hub 만들기

[!INCLUDE [iot-hub-include-create-hub-device-streams](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>디바이스 등록

디바이스를 연결하려면 먼저 디바이스를 IoT Hub에 등록해야 합니다. 이 섹션에서는 [IoT 확장](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest)에서 Azure Cloud Shell을 사용하여 시뮬레이션된 디바이스를 등록합니다.

1. 디바이스 ID를 만들려면 Cloud Shell에서 다음 명령을 실행합니다.

   > [!NOTE]
   > * *YourIoTHubName* 자리 표시자를 IoT Hub에서 선택한 이름으로 바꿉니다.
   > * 다음과 같이 *MyDevice*를 사용합니다. 등록된 디바이스에 지정된 이름입니다. 다른 디바이스 이름을 선택하는 경우 이 문서 전체에서 해당 이름을 사용하고, 샘플 애플리케이션에서 디바이스 이름을 업데이트한 후에 실행하세요.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

2. 방금 등록한 디바이스의 *디바이스 연결 문자열*을 가져오려면 Cloud Shell에서 다음 명령을 실행합니다.

   > [!NOTE]
   > *YourIoTHubName* 자리 표시자를 IoT Hub에서 선택한 이름으로 바꿉니다.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyDevice --output table
    ```

    나중에 이 빠른 시작에서 사용할 수 있도록 디바이스 연결 문자열을 적어 두세요. 다음 예제와 유사합니다.

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

## <a name="communicate-between-the-device-and-the-service-via-device-streams"></a>디바이스 스트림을 통해 디바이스와 서비스 간 통신

이 섹션에서는 디바이스 쪽 애플리케이션과 서비스 쪽 애플리케이션을 둘 다 실행하고 서로 통신합니다.

### <a name="run-the-device-side-application"></a>디바이스 쪽 애플리케이션 실행

디바이스 쪽 애플리케이션을 실행하려면 다음을 수행합니다.

1. *iothub_client/samples/iothub_client_c2d_streaming_sample* 폴더의 *iothub_client_c2d_streaming_sample.c* 원본 파일을 편집한 다음, 디바이스 연결 문자열을 입력하여 디바이스 자격 증명을 제공합니다.

   ```C
   /* Paste in your iothub connection string  */
   static const char* connectionString = "[device connection string]";
   ```

2. 다음과 같이 코드를 컴파일합니다.

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_sample
   make -j
   ```

   ```cmd
   rem In Windows
   rem Go to the cmake folder at the root of repo
   cmake --build . -- /m /p:Configuration=Release
   ```

3. 컴파일된 프로그램을 실행합니다.

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_sample
   ./iothub_client_c2d_streaming_sample
   ```

   ```cmd
   rem In Windows
   rem Go to the sample's release folder cmake\iothub_client\samples\iothub_client_c2d_streaming_sample\Release
   iothub_client_c2d_streaming_sample.exe
   ```

### <a name="run-the-service-side-application"></a>서비스 쪽 애플리케이션 실행

앞서 언급했듯이, IoT Hub C SDK는 디바이스 쪽에서만 디바이스 스트림을 지원합니다. 서비스 쪽 애플리케이션을 빌드하여 실행하려면 다음 빠른 시작 중 하나의 지침을 따르세요.

* [IoT Hub 디바이스 스트림을 통해 C#에서 디바이스 앱과 통신](./quickstart-device-streams-echo-csharp.md)
* [IoT Hub 디바이스 스트림을 통해 Node.js에서 디바이스 앱과 통신](./quickstart-device-streams-echo-nodejs.md)

## <a name="clean-up-resources"></a>리소스 정리

[!INCLUDE [iot-hub-quickstarts-clean-up-resources-device-streams](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>다음 단계

이 빠른 시작에서는 IoT Hub를 설정하고, 디바이스를 등록하고, 디바이스의 C 애플리케이션과 서비스 쪽의 다른 애플리케이션 간에 디바이스 스트림을 설정하고, 스트림을 사용하여 애플리케이션 간에 데이터를 주고 받았습니다.

디바이스 스트림에 대한 자세한 내용은 다음을 참조하세요.

> [!div class="nextstepaction"]
> [디바이스 스트림 개요](./iot-hub-device-streams-overview.md)
