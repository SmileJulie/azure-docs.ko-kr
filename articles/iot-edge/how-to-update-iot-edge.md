---
title: 디바이스에서 IoT Edge 버전 업데이트 - Azure IoT Edge | Microsoft Docs
description: 최신 버전의 보안 디먼 및 IoT Edge 런타임을 실행하도록 IoT Edge 디바이스를 업데이트하는 방법
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/27/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 0c461da44d3d9075d66a68fe8994a4e970288fca
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543759"
---
# <a name="update-the-iot-edge-security-daemon-and-runtime"></a>IoT Edge 보안 디먼 및 런타임 업데이트

IoT Edge 서비스 릴리스는 새 버전으로 최신 기능 및 향상 된 보안 기능에 대 한 IoT Edge 장치를 업데이트 하는 것이 좋습니다. 이 문서에서는 새 버전을 사용할 수 있을 때 IoT Edge 디바이스를 업데이트하는 방법에 대한 정보를 제공합니다. 

새 버전으로 전환하려면 IoT Edge 디바이스의 두 가지 구성 요소를 업데이트해야 합니다. 첫 번째는 디바이스에서 실행되고 디바이스가 시작될 때 런타임 모듈을 시작하는 보안 디먼입니다. 현재, 보안 디먼은 디바이스 자체에서만 업데이트할 수 있습니다. 두 번째 구성 요소는 런타임으로, IoT Edge 허브 및 IoT Edge 에이전트 모듈로 구성됩니다. 배포 구성 방법에 따라 디바이스에서 또는 원격으로 런타임을 업데이트할 수 있습니다. 

최신 버전의 Azure IoT Edge를 찾으려면 [Azure IoT Edge 릴리스](https://github.com/Azure/azure-iotedge/releases)를 참조하세요.

>[!IMPORTANT]
>Windows 디바이스에서 Azure IoT Edge를 실행할 때 다음 중 하나가 디바이스에 적용되는 경우에는 버전 1.0.5로 업데이트하지 마세요. 
>* 디바이스를 Windows 빌드 17763으로 업그레이드하지 않았습니다. IoT Edge 버전 1.0.5는 17763보다 오래된 Windows 빌드를 지원하지 않습니다.
>* Windows 디바이스에서 Java 또는 Node.js 모듈을 실행합니다. Windows 디바이스를 최신 빌드로 업데이트한 경우에도 버전 1.0.5를 건너뜁니다. 
>
>IoT Edge 버전 1.0.5에 대한 자세한 내용은 [1.0.5 릴리스 정보](https://github.com/Azure/azure-iotedge/releases/tag/1.0.5)를 참조하세요. 개발 도구를 최신 버전으로 업데이트 하지 못하도록 하는 방법에 대 한 자세한 내용은 참조 하세요. [IoT 개발자 블로그](https://devblogs.microsoft.com/iotdev/)합니다.


## <a name="update-the-security-daemon"></a>보안 디먼 업데이트

IoT Edge 보안 디먼은 IoT Edge 디바이스에서 패키지 관리자를 사용하여 업데이트해야 하는 네이티브 구성 요소입니다. 

`iotedge version` 명령을 사용하여 디바이스에서 실행 중인 보안 디먼의 버전을 확인합니다. 

### <a name="linux-devices"></a>Linux 디바이스

X64 Linux 장치에서 보안 디먼 업데이트 apt get 또는 적절 한 패키지 관리자를 사용 합니다. 

```bash
apt-get update
apt-get install libiothsm iotedge
```

단계를 사용 하 여 Linux ARM32 장치의 [(ARM32v7/armhf) Linux에서 Azure IoT Edge 설치 런타임](how-to-install-iot-edge-linux-arm.md) 보안 디먼의 최신 버전을 설치 합니다. 

### <a name="windows-devices"></a>Windows 디바이스

Windows 장치에서 PowerShell 스크립트를 사용 하 여 보안 디먼을 업데이트 합니다. 스크립트는 자동으로 보안 디먼의 최신 버전을 가져옵니다. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Update-IoTEdge -ContainerOs <Windows or Linux>
```

두 런타임 컨테이너 이미지와 함께 장치에서 보안 디먼을 제거 업데이트 IoTEdge 명령을 실행 합니다. Config.yaml 파일 (Windows 컨테이너를 사용 하는) 경우 모 비 컨테이너 엔진에서 데이터 뿐만 아니라 장치에 유지 됩니다. 연결 문자열이 나 다시 업데이트 과정에서 장치에 대 한 Device Provisioning Service 정보를 제공 하지 않아도 구성 정보 즉 유지 합니다. 

보안 디먼의 특정 버전을 설치 하려는 경우에서 적절 한 Microsoft-Azure-IoTEdge.cab 파일을 다운로드 [IoT Edge 해제](https://github.com/Azure/azure-iotedge/releases)합니다. 그런 다음, `-OfflineInstallationPath` 매개 변수를 사용하여 파일 위치를 가리킵니다. 자세한 내용은 [오프라인 설치](how-to-install-iot-edge-windows.md#offline-installation)를 참조하세요.

## <a name="update-the-runtime-containers"></a>런타임 컨테이너 업데이트

IoT Edge 에이전트 및 IoT Edge 허브 컨테이너를 업데이트 하는 방법은 배포 환경에서 롤링 태그 (예: 1.0) 또는 특정 태그 (예: 1.0.7) 사용 되는 여부에 따라 달라 집니다. 

`iotedge logs edgeAgent` 또는 `iotedge logs edgeHub` 명령을 사용하여 현재 사용 중인 디바이스에 있는 IoT Edge 에이전트 및 IoT Edge 허브 모듈 버전을 확인합니다. 

  ![로그의 컨테이너 버전 찾기](./media/how-to-update-iot-edge/container-version.png)

### <a name="understand-iot-edge-tags"></a>IoT Edge 태그 이해

IoT Edge 에이전트 및 IoT Edge 허브 이미지에는 연결된 IoT Edge 버전으로 태그가 지정됩니다. 런타임 이미지에 태그를 사용하는 방법에는 다음 두 가지가 있습니다. 

* **롤링 태그** - 버전 번호의 처음 두 개 값만 사용하여 해당 숫자와 일치하는 최신 이미지를 가져옵니다. 예를 들어, 최신 1.0.x 버전을 가리키는 새 릴리스가 있을 때마다 1.0이 업데이트됩니다. IoT Edge 디바이스의 컨테이너 런타임이 이미지를 다시 끌어오면 런타임 모듈은 최신 버전으로 업데이트됩니다. 이 접근 방법은 개발 목적으로 제안됩니다. Azure Portal에서 배포할 때는 기본적으로 롤링 태그가 사용됩니다. 
* **특정 태그** - 버전 번호의 세 값을 모두 사용하여 이미지 버전을 명시적으로 설정합니다. 예를 들어 1.0.7 처음 출시 후 변경 되지 않습니다. 업데이트할 준비가 되면 배포 매니페스트에서 새 버전 번호를 선언할 수 있습니다. 이 접근 방법은 프로덕션 목적으로 제안됩니다.

### <a name="update-a-rolling-tag-image"></a>롤링 태그 이미지 업데이트

배포에 롤링 태그를 사용하는 경우(예: mcr.microsoft.com/azureiotedge-hub:  **1.0**) 디바이스의 컨테이너 런타임이 강제로 최신 버전의 이미지를 가져오도록 해야 합니다. 

IoT Edge 디바이스에서 이미지의 로컬 버전을 삭제합니다. Windows 머신에서 보안 디먼을 제거하면 런타임 이미지도 제거되므로 이 단계를 다시 수행할 필요가 없습니다. 

```bash
docker rmi mcr.microsoft.com/azureiotedge-hub:1.0
docker rmi mcr.microsoft.com/azureiotedge-agent:1.0
```

이미지를 제거하기 위해 강제 `-f` 플래그를 사용해야 할 수 있습니다. 

IoT Edge 서비스는 최신 버전의 런타임 이미지를 끌어오고 해당 이미지를 디바이스에서 자동으로 다시 시작합니다. 

### <a name="update-a-specific-tag-image"></a>특정 태그 이미지 업데이트

배포에 특정 태그를 사용 하는 경우 (예를 들어 mcr.microsoft.com/azureiotedge-hub:**1.0.7**) 배포 매니페스트에 대 한 태그를 업데이트 하 고 장치에 변경 내용을 적용은 하기만 하면 합니다. 

Azure Portal에서 런타임 배포 이미지는 **고급 에지 런타임 설정 구성** 섹션에서 선언됩니다. 

![고급 edge 런타임 설정 구성](./media/how-to-update-iot-edge/configure-runtime.png)

JSON 배포 매니페스트에서 **systemModules** 섹션의 모듈 이미지를 업데이트합니다. 

```json
"systemModules": {
  "edgeAgent": {
    "type": "docker",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-agent:1.0.7",
      "createOptions": ""
    }
  },
  "edgeHub": {
    "type": "docker",
    "status": "running",
    "restartPolicy": "always",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-hub:1.0.7",
      "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5671/tcp\":[{\"HostPort\":\"5671\"}], \"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}]}}}"
    }
  }
},
```

## <a name="update-to-a-release-candidate-version"></a>릴리스 후보 버전 업데이트

Azure IoT Edge는 IoT Edge 서비스의 새 버전이 정기적으로 해제합니다. 각 안정적인 릴리스 전에 경우 하나 이상의 릴리스 후보 (RC) 버전 RC 버전 릴리스의 경우 계획 된 모든 기능이 포함 되어 있지만 안정적인 릴리스 하는 데 필요한 테스트 및 유효성 검사 프로세스를 통해 계속 됩니다. 새로운 기능을 일찍 테스트 하려는 경우 RC 버전을 설치 하 고 GitHub 통해 사용자 의견을 제공할 수 있습니다. 

릴리스 후보 버전의 릴리스를 동일한 번호 매기기 규칙을 따릅니다 했지만 **-rc** plus 증분값을 끝에 추가 합니다. 동일한 목록에서 릴리스 후보를 볼 수 있습니다 [Azure IoT Edge 해제](https://github.com/Azure/azure-iotedge/releases) 안정적인 버전으로 합니다. 예를 들어 찾을 **1.0.7-rc1** 하 고 **1.0.7-rc2**를 두 릴리스 전에 제공 된 후보 **1.0.7**. RC 버전은 표시 된 함을 확인할 수 있습니다 **시험판** 레이블. 

릴리스 후보 버전으로 미리 보기, 최신 버전으로 포함 하지는 일반 설치 관리자가 대상입니다. 대신 수동으로 테스트 하려는 RC 버전에 대 한 자산을 대상으로 해야 합니다. IoT Edge 장치 운영 체제에 따라 다음 섹션에서는 IoT Edge 특정 버전으로 업데이트를 사용 합니다.

* [Linux X64](how-to-install-iot-edge-linux.md#install-a-specific-version)
* [Linux ARM32](how-to-install-iot-edge-linux-arm.md#install-a-specific-version)
* [Windows](how-to-install-iot-edge-windows.md#offline-installation)

## <a name="next-steps"></a>다음 단계

최신 [Azure IoT Edge 릴리스](https://github.com/Azure/azure-iotedge/releases)를 확인합니다.

[IoT(사물 인터넷) 블로그](https://azure.microsoft.com/blog/topics/internet-of-things/)에서 최신 업데이트 및 알림을 받아 보세요. 