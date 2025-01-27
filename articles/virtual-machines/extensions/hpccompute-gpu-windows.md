---
title: NVIDIA GPU 드라이버 확장 - Azure Windows VM | Microsoft Docs
description: Windows를 실행하는 N 시리즈 계산 VM에 NVIDIA GPU 드라이버를 설치하는 Microsoft Azure 확장입니다.
services: virtual-machines-windows
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 01/09/2019
ms.author: roiyz
ms.openlocfilehash: 004d6125de6762303db91f3a5ef9ffa16e6e501f
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705949"
---
# <a name="nvidia-gpu-driver-extension-for-windows"></a>Windows용 NVIDIA GPU 드라이버 확장

## <a name="overview"></a>개요

이 확장은 Windows N 시리즈 VM에 NVIDIA GPU 드라이버를 설치합니다. 확장은 VM 제품군에 따라 CUDA 또는 GRID 드라이버를 설치합니다. 이 확장을 사용하여 NVIDIA 드라이버를 설치하면 [NVIDIA 최종 사용자 사용권 계약](https://go.microsoft.com/fwlink/?linkid=874330)을 수락하고 이에 동의하게 됩니다. 설치 프로세스 중에 드라이버 설치를 완료하기 위해 VM이 다시 부팅될 수 있습니다.

사용할 수는 드라이버 및 지원 되는 현재 버전의 수동 설치에 대 한 지침 [여기](https://docs.microsoft.com/azure/virtual-machines/windows/n-series-driver-setup)합니다.
또한 [Linux N 시리즈 VM](hpccompute-gpu-linux.md)에 NVIDIA GPU 드라이버를 설치할 수 있는 확장도 제공됩니다.

## <a name="prerequisites"></a>필수 구성 요소

### <a name="operating-system"></a>운영 체제

확장에서 지원하는 OS는 다음과 같습니다.

| 배포 | 버전 |
|---|---|
| Windows 10 | Core |
| Windows Server 2016 | Core |
| Windows Server 2012R2 | Core |

### <a name="internet-connectivity"></a>인터넷 연결

NVIDIA GPU 드라이버용 Microsoft Azure 확장을 사용하려면 대상 VM이 인터넷에 연결되어 있고 액세스 권한이 있어야 합니다.

## <a name="extension-schema"></a>확장 스키마

확장에 대한 스키마를 보여 주는 JSON은 다음과 같습니다.

```json
{
  "name": "<myExtensionName>",
  "type": "extensions",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <myVM>)]"
  ],
  "properties": {
    "publisher": "Microsoft.HpcCompute",
    "type": "NvidiaGpuDriverWindows",
    "typeHandlerVersion": "1.2",
    "autoUpgradeMinorVersion": true,
    "settings": {
    }
  }
}
```

### <a name="properties"></a>속성

| 이름 | 값/예제 | 데이터 형식 |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | 날짜 |
| publisher | Microsoft.HpcCompute | string |
| type | NvidiaGpuDriverWindows | string |
| typeHandlerVersion | 1.2 | ssNoversion |


## <a name="deployment"></a>배포

### <a name="azure-resource-manager-template"></a>Azure Resource Manager 템플릿 

Azure Resource Manager 템플릿을 사용하여 Azure VM 확장을 배포할 수 있습니다. 배포 후 구성이 필요한 하나 이상의 가상 머신을 배포하는 경우 템플릿을 사용하는 것이 좋습니다.

가상 머신 확장에 대한 JSON 구성은 가상 머신 리소스 내에 중첩되거나 루트 또는 최상위 수준의 Resource Manager JSON 템플릿에 배치될 수 있습니다. JSON 구성의 배치는 리소스 이름 및 형식 값에 영향을 줍니다. 자세한 내용은 [자식 리소스의 이름 및 형식 설정](../../azure-resource-manager/resource-manager-template-child-resource.md)을 참조하세요. 

다음 예제에서는 확장이 가상 머신 리소스 내에 중첩되어 있다고 가정합니다. 확장 리소스를 중첩하는 경우 JSON은 가상 머신의 `"resources": []` 개체에 배치됩니다.

```json
{
  "name": "myExtensionName",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', myVM)]"
  ],
  "properties": {
    "publisher": "Microsoft.HpcCompute",
    "type": "NvidiaGpuDriverWindows",
    "typeHandlerVersion": "1.2",
    "autoUpgradeMinorVersion": true,
    "settings": {
    }
  }
}
```

### <a name="powershell"></a>PowerShell

```powershell
Set-AzVMExtension
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Location "southcentralus" `
    -Publisher "Microsoft.HpcCompute" `
    -ExtensionName "NvidiaGpuDriverWindows" `
    -ExtensionType "NvidiaGpuDriverWindows" `
    -TypeHandlerVersion 1.2 `
    -SettingString '{ `
    }'
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az vm extension set `
  --resource-group myResourceGroup `
  --vm-name myVM `
  --name NvidiaGpuDriverWindows `
  --publisher Microsoft.HpcCompute `
  --version 1.2 `
  --settings '{ `
  }'
```

## <a name="troubleshoot-and-support"></a>문제 해결 및 지원

### <a name="troubleshoot"></a>문제 해결

확장 배포 상태에 대한 데이터는 Azure Portal에서 Azure PowerShell 및 Azure CLI를 사용하여 검색할 수 있습니다. 지정된 VM에 대한 확장의 배포 상태를 보려면 다음 명령을 실행합니다.

```powershell
Get-AzVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

확장 실행 출력은 다음 디렉터리에 기록됩니다.

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.HpcCompute.NvidiaGpuDriverMicrosoft\
```

### <a name="error-codes"></a>오류 코드

| 오류 코드 | 의미 | 가능한 작업 |
| :---: | --- | --- |
| 0 | 작업이 성공했습니다. |
| 1 | 작업이 성공했습니다. 다시 부팅해야 합니다. |
| 100 | 작업이 지원되지 않거나 완료할 수 없습니다. | 가능한 원인: PowerShell 버전이 지원되지 않습니다. VM 크기가 N 시리즈 VM이 아니며, 데이터를 다운로드하지 못했습니다. 로그 파일을 확인하여 오류의 원인을 파악합니다. |
| 240, 840 | 작업 시간이 초과되었습니다. | 작업을 다시 시도합니다. |
| -1 | 예외가 발생했습니다. | 로그 파일을 확인하여 예외의 원인을 파악합니다. |
| -5x | 보류 중인 재부팅으로 인해 작업이 중단되었습니다. | VM을 다시 부팅합니다. 다시 부팅한 후에 설치가 계속됩니다. 제거는 수동으로 호출해야 합니다. |


### <a name="support"></a>Support(지원)

이 문서의 어디에서든 도움이 필요한 경우 [MSDN Azure 및 Stack Overflow 포럼](https://azure.microsoft.com/support/community/)에서 Azure 전문가에게 문의할 수 있습니다. 또는 Azure 기술 지원 인시던트를 제출할 수 있습니다. [Azure 지원 사이트](https://azure.microsoft.com/support/options/)로 가서 지원 받기를 선택합니다. Azure 지원을 사용하는 방법에 대한 자세한 내용은 [Microsoft Azure 지원 FAQ](https://azure.microsoft.com/support/faq/)를 참조하세요.

## <a name="next-steps"></a>다음 단계
확장에 대한 자세한 내용은 [Windows용 가상 머신 확장 및 기능](features-windows.md)을 참조하세요.

N 시리즈 VM에 대한 자세한 내용은 [GPU 최적화 가상 머신 크기](../windows/sizes-gpu.md)를 참조하세요.
