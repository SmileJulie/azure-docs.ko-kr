---
title: Azure에서 Windows VM 배포 문제 해결 | Microsoft Docs
description: Azure에서 새 Windows 가상 머신 생성 시 Resource Manager 배포 문제 해결
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: JiangChen79
manager: gwallace
editor: ''
tags: top-support-issue, azure-resource-manager
ms.assetid: afc6c1a4-2769-41f6-bbf9-76f9f23bcdf4
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: cjiang
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6bdf0816e40e34b184da0340a8dbaffbcd7657e8
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67710452"
---
# <a name="troubleshoot-deployment-issues-when-creating-a-new-windows-vm-in-azure"></a>Azure에서 새 Windows VM 생성 시 배포 문제 해결
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a>주요 문제
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

VM 배포 문제 및 질문은 [Azure에서 Windows 가상 컴퓨터 배포 문제 해결](troubleshoot-deploy-vm-windows.md)을 참조하세요.

## <a name="collect-activity-logs"></a>활동 로그 선택
문제 해결을 시작하려면 문제와 관련된 오류를 파악하기 위해 활동 로그를 수집합니다. 다음 링크에는 수행할 프로세스에 대한 자세한 내용이 포함되어 있습니다.

[배포 작업 보기](../../azure-resource-manager/resource-manager-deployment-operations.md)

[활동 로그를 보고 Azure 리소스 관리](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**Y:** OS가 일반화된 Windows인 경우, 일반화된 설정을 사용하여 업로드되거나 캡처되고, 오류는 발생하지 않습니다. 마찬가지로 OS가 특수화된 Windows인 경우, 특수화된 설정을 사용하여 업로드되거나 캡처되고, 오류는 발생하지 않습니다.

**업로드 오류:**

**N<sup>1</sup>:** OS가 일반화된 Windows이고 특수화된 것으로 업로드된다면, VM이 OOBE 화면에서 중단되면서 프로비전 시간 제한 오류가 발생합니다.

**N<sup>2</sup>:** OS가 특수화된 Windows이고 일반화된 것으로 업로드된다면, 새 VM이 원래 컴퓨터 이름, 사용자 이름 및 암호로 실행되기 때문에 VM이 OOBE 화면에서 중단되면서 프로비전 실패 오류가 발생합니다.

**해결 방법**

이 두 가지 오류를 모두 해결하려면, OS와 같은 설정(일반화/특수화)으로, [Add-AzVhd를 사용하여 온-프레미스에 있는 원본 VHD를 업로드](https://docs.microsoft.com/powershell/module/az.compute/add-azvhd)합니다. 일반화된 것으로 업로드하려면, 먼저 sysprep을 실행해야 합니다.

**캡처 오류:**

**N<sup>3</sup>:** OS가 일반화된 Windows이고 특수화된 것으로 캡처된다면, 원본 VM이 일반화된 것으로 표시되어 사용할 수 없기 때문에 프로비전 시간 제한 오류가 발생합니다.

**N<sup>4</sup>:** OS가 특수화된 Windows이고 일반화된 것으로 캡처된다면, 새 VM이 원래 컴퓨터 이름, 사용자 이름 및 암호로 실행되기 때문에 프로비전 실패 오류가 발생합니다. 또한, 원본 VM이 일반화된 것으로 표시되기 때문에 사용할 수 없습니다.

**해결 방법**

이 두 가지 오류를 모두 해결하려면, 현재 이미지를 포털에서 제거하고, OS와 같은 설정(일반화/특수화)으로 [현재 VHD에서 다시 캡처](../windows/create-vm-specialized.md)합니다.

## <a name="issue-customgallerymarketplace-image-allocation-failure"></a>문제: 사용자 지정/갤러리/마켓플레이스 이미지, 할당 오류
이 오류는 요청되는 VM 크기를 지원할 수 없거나 요청을 수용할 여유 공간이 없는 클러스터에 새 VM 요청이 고정된 상황에서 발생합니다.

**원인 1:** 클러스터가 요청된 VM 크기를 지원할 수 없습니다.

**해결 방법 1:**

* 더 작은 VM 크기를 사용하여 요청을 다시 시도합니다.
* 요청한 VM의 크기를 변경할 수 없으면:
  * 가용성 집합의 VM을 모두 중지합니다.
    **리소스 그룹** > *사용자의 리소스 그룹* > **리소스** > *사용자의 가용성 집합* > **Virtual Machines** > *사용자의 가상 머신* > **중지**를 클릭합니다.
  * VM을 모두 중지한 후에, 원하는 크기로 VM을 새로 만듭니다.
  * 먼저 VM을 시작한 후에 중지된 각각의 VM을 선택하고 **시작**을 클릭합니다.

**원인 2:** 클러스터에 여유 리소스가 없습니다.

**해결 방법 2:**

* 요청을 나중에 다시 시도합니다.
* 새 VM이 다른 가용성 집합의 일부가 될 수 있다면
  * 다른 가용성 집합(동일한 지역의)에 새 VM을 만듭니다.
  * 새 VM을 동일한 가상 네트워크에 추가합니다.

## <a name="next-steps"></a>다음 단계
중지된 Windows VM을 시작하거나 Azure에서 기존 Windows VM의 크기를 조정할 때 문제가 발생하면 [Azure의 기존 Windows Virtual Machine 재시작 또는 크기 조정 관련 Resource Manager 배포 문제 해결](restart-resize-error-troubleshooting.md)을 참조하세요.


