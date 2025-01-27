---
title: Windows VM에서 데이터 디스크 분리 - Azure| Microsoft Docs
description: Resource Manager 배포 모델을 사용하여 Azure의 가상 머신에서 데이터 디스크를 분리합니다.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-service-management
ms.assetid: 13180343-ac49-4a3a-85d8-0ead95e2028c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/17/2018
ms.author: cynthn
ms.subservice: disks
ms.openlocfilehash: ca9a4478249e935afb6a52520c77d9df159fe9e7
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67718735"
---
# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>Windows 가상 컴퓨터에서 데이터 디스크를 분리하는 방법

가상 머신에 연결된 데이터 디스크가 더 이상 필요하지 않은 경우 쉽게 분리할 수 있습니다. 디스크를 분리하면 가상 머신에서 디스크가 제거되지만, 저장소에서는 제거되지 않습니다.

> [!WARNING]
> 디스크를 분리해도 자동으로 삭제되지 않습니다. Premium Storage를 구독하는 경우 디스크에 대한 스토리지 요금이 계속 부과됩니다. 자세한 내용은 [Premium Storage 사용 시 가격 책정 및 청구](disks-types.md#billing)를 참조하세요.

디스크에 있는 기존 데이터를 다시 사용하려는 경우 동일한 또는 다른 가상 머신에 다시 연결할 수 있습니다.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="detach-a-data-disk-using-powershell"></a>PowerShell을 사용하여 데이터 디스크 분리

PowerShell을 사용하여 데이터 디스크를 *작동 중* 제거(hot remove)할 수 있지만, VM에서 해당 디스크를 분리할 때 해당 디스크가 전혀 사용되고 있지 않아야 합니다.

이 예제에서는 **myResourceGroup** 리소스 그룹의 VM **myVM**에서 **myDisk**라는 디스크를 제거합니다. 먼저 [Remove-AzVMDataDisk](https://docs.microsoft.com/powershell/module/az.compute/remove-azvmdatadisk) cmdlet을 사용하여 디스크를 제거합니다. 그런 다음, [Update-AzVM](https://docs.microsoft.com/powershell/module/az.compute/update-azvm) cmdlet으로 가상 머신의 상태를 업데이트하여 데이터 디스크 제거 프로세스를 완료합니다.

```azurepowershell-interactive
$VirtualMachine = Get-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM"
Remove-AzVMDataDisk -VM $VirtualMachine -Name "myDisk"
Update-AzVM -ResourceGroupName "myResourceGroup" -VM $VirtualMachine
```

디스크가 저장소에 유지되지만 더 이상 가상 머신에 연결되어 있지 않습니다.

## <a name="detach-a-data-disk-using-the-portal"></a>포털을 사용하여 데이터 디스크 분리

1. 왼쪽 메뉴에서 **Virtual Machines**을 선택합니다.
2. 분리할 데이터 디스크가 있는 가상 머신을 선택하고 **중지**를 클릭하여 VM 할당을 취소합니다.
3. 가상 머신 창에서 **디스크**를 선택합니다.
4. **디스크** 창 상단에서 **편집**을 선택합니다.
5. **디스크** 창에서 분리할 데이터 디스크의 오른쪽 끝에 있는 ![분리 단추 이미지](./media/detach-disk/detach.png) 분리 단추를 클릭합니다.
5. 디스크를 제거한 후에 창 상단에서 **저장**을 클릭합니다.
6. 가상 머신 창에서 **개요**를 클릭한 다음 창 상단에서 **시작** 단추를 클릭하여 VM을 다시 시작합니다.

디스크가 저장소에 유지되지만 더 이상 가상 머신에 연결되어 있지 않습니다.

## <a name="next-steps"></a>다음 단계

데이터 디스크를 다시 사용하려는 경우 [다른 VM에 연결](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)