---
title: Azure의 Windows VM에 대한 FAQ | Microsoft Docs
description: 리소스 관리자 모델을 사용하여 만든 Windows 가상 머신에 대해 가장 일반적인 질문 중 일부에 대한 답변을 제공합니다.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-management
ms.assetid: 757da816-a050-4889-a010-6f75d7978eb7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/08/2019
ms.author: cynthn
ms.openlocfilehash: 69c79db937d70bcc8ae834bf0bdf8f1930bed3c9
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67722749"
---
# <a name="frequently-asked-question-about-windows-virtual-machines"></a>Windows Virtual Machines에 대한 자주 묻는 질문과 대답
이 문서에서는 Azure에서 리소스 관리자 배포 모델을 사용하여 만든 Windows 가상 머신에 대한 일부 일반적인 질문을 해결합니다. 이 항목의 Linux 버전을 참조 하세요 [질문을 Linux Virtual Machines에 대 한 질문과](../linux/faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)합니다.

## <a name="what-can-i-run-on-an-azure-vm"></a>Azure VM에서 무엇을 실행할 수 있습니까?
모든 구독자는 Azure 가상 머신에서 서버 소프트웨어를 실행할 수 있습니다. Azure에서 실행 중인 Microsoft 서버 소프트웨어에 대 한 지원 정책에 대 한 정보를 참조 하세요 [Azure Virtual Machines에 대 한 Microsoft 서버 소프트웨어 지원](https://support.microsoft.com/kb/2721672)합니다.

Windows 7, Windows 8.1 및 Windows 10의 특정 버전은 MSDN Azure 혜택 구독자와 MSDN 개발 및 테스트 종량제 구독자가 개발 및 테스트 작업을 위해 사용할 수 있습니다. 지침과 제한 사항을 포함한 자세한 내용은 [MSDN 구독자를 위한 Windows 클라이언트 이미지](https://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/)를 참조하세요. 

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>가상 머신에 얼마나 많은 용량의 저장소를 사용할 수 있습니까?
각 데이터 디스크는 최대 32,767 GiB 수 있습니다. 사용할 수 있는 데이터 디스크의 수는 가상 머신의 크기에 따라 달라집니다. 자세한 내용은 [Virtual Machines의 크기](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)를 참조하세요.

Azure Managed Disks는 데이터 영구 저장을 위해 Azure Virtual Machines와 함께 사용하기 적합한 디스크 저장소 제품입니다. 각 Virtual Machine과 함께 여러 Managed Disks를 사용할 수 있습니다. Managed Disks는 두 가지 지속형 스토리지 옵션으로 프리미엄 및 표준 Managed Disks를 제공합니다. 가격 책정 정보는 [Managed Disks 가격 책정](https://azure.microsoft.com/pricing/details/managed-disks)을 참조하세요.

Azure Stroage 계정은 운영 체제 디스크 및 모든 데이터 디스크에 대한 저장소도 제공할 수 있습니다. 각 디스크는 페이지 blob으로 저장된 .vhd 파일입니다. 가격 책정에 대한 자세한 내용은 [저장소 가격 세부 정보](https://azure.microsoft.com/pricing/details/storage/)를 참조하세요.

## <a name="how-can-i-access-my-virtual-machine"></a>나의 가상 컴퓨터에 액세스 하려면 어떻게 해야 합니까?
RDP(원격 데스크톱 연결)를 사용하여 Windows VM에 대한 원격 연결을 설정합니다. 자세한 내용은 [연결에 Windows를 실행 하는 Azure 가상 머신에 로그인 하는 방법을](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)합니다. 서버가 원격 데스크톱 서비스 세션 호스트로 구성되지 않으면 최대 2개의 동시 연결이 지원됩니다.  

원격 데스크톱에 문제가 있는 경우 [Windows 기반 Azure Virtual Machine에 대한 원격 데스크톱 연결 문제 해결](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)을 참조하세요. 

Hyper-V에 친숙한 경우 VMConnect와 유사한 도구를 찾을 수 있습니다. 가상 머신에 대한 콘솔 액세스가 지원되지 않으므로 Azure는 유사한 도구를 제공하지 않습니다.

## <a name="can-i-use-the-temporary-disk-the-d-drive-by-default-to-store-data"></a>임시 디스크(기본적으로 D: 드라이브)를 사용하여 데이터를 저장할 수 있나요?
데이터를 저장하는 데 임시 디스크를 사용해서는 안 됩니다. 해당 드라이브는 임시 저장소일 뿐이므로 복구할 수 없는 데이터가 손실될 위험이 있습니다. Virtual Machine가 다른 호스트로 이동하면 데이터가 손실될 수 있습니다. 가상 머신 크기를 조정하고, 호스트를 업데이트 하거나, 호스트의 하드웨어가 실패하는 경우가, 가상 머신이 이동할 수 있는 몇 가지 이유가 됩니다.

D: 드라이브 문자를 사용해야 하는 애플리케이션이 있는 경우 드라이브 문자를 재할당하여 임시 디스크가 D: 외의 다른 드라이브 문자를 사용하도록 할 수 있습니다. 지침에 대한 자세한 내용은 [Windows 임시 디스크의 드라이브 문자 변경](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)을 참조하세요.


## <a name="how-can-i-change-the-drive-letter-of-the-temporary-disk"></a>임시 디스크의 드라이브 문자 변경을 어떻게 합니까?
페이지 파일을 이동하고 드라이브 문자를 다시 할당하여 드라이브 문자를 변경할 수는 있지만, 이렇게 하려면 관련 단계를 특정 순서에 따라 수행해야 합니다. 지침에 대한 자세한 내용은 [Windows 임시 디스크의 드라이브 문자 변경](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)을 참조하세요.

## <a name="can-i-add-an-existing-vm-to-an-availability-set"></a>가용성 집합에 기존 VM을 추가할 수 있나요?
아니요. VM이 가용성 집합에 속하려면 집합 내에서 VM을 만들어야 합니다. 현재는 VM을 만든 이후에 가용성 집합에 추가하는 방법이 없습니다.

## <a name="can-i-upload-a-virtual-machine-to-azure"></a>가상 컴퓨터를 Azure에 업로드할 수 있나요?
예. 자세한 내용은 [온-프레미스 VM을 Azure로 마이그레이션](on-prem-to-azure.md)을 참조하세요.

## <a name="can-i-resize-the-os-disk"></a>OS 디스크의 크기를 조정할 수 있나요?
예. 자세한 내용은 [Azure 리소스 그룹에서 Virtual Machine의 OS 드라이브를 확장하는 방법](expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)을 참조하세요.

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>기존 Azure VM을 복사 또는 복제할 수 있나요?
예. 관리되는 이미지를 사용하여 가상 머신의 이미지를 만든 후 이 이미지를 사용하여 여러 VM을 새로 구축할 수 있습니다. 자세한 내용은 [VM의 사용자 지정 이미지 만들기](tutorial-custom-images.md)를 참조하세요.

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Azure Resource Manager를 통해 캐나다 중부 및 캐나다 동부 지역이 보이지 않는 이유가 무엇인가요?

캐나다 중부 및 캐나다 동부의 새로운 두 지역은 기존의 Azure 구독에 대한 가상 머신 만들기에 자동으로 등록되지 않습니다. 가상 머신이 Azure 포털을 통해 Azure Resource Manager를 사용하는 다른 지역에 배포될 때 자동으로 등록됩니다. 가상 머신이 다른 Azure 지역에 배포된 후 새로운 지역은 다음 가상 머신에 대해 사용할 수 있어야 합니다.

## <a name="does-azure-support-linux-vms"></a>Azure에서 Linux VM을 지원하나요?
예. 사용해 보기 위해 Linux VM을 신속하게 만들려면 [포털을 사용하여 Azure에서 Linux VM 만들기](../linux/quick-create-portal.md)를 참조하세요.

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>VM을 만든 후에 NIC를 추가할 수 있나요?
예, 이제 가능합니다. 먼저 VM에 대한 할당 취소를 중지해야 합니다. 그런 다음 NIC를 추가하거나 제거할 수 있습니다(VM에 있는 마지막 NIC가 아닌 경우). 

## <a name="are-there-any-computer-name-requirements"></a>컴퓨터 이름 요구 사항이 있나요?
예. 컴퓨터 이름은 15자까지 지정할 수 있습니다. [명명 규칙 및 제한 사항](/azure/architecture/best-practices/naming-conventions#compute)을 참조하여 리소스 이름 지정에 대해 자세히 알아보세요.

## <a name="are-there-any-resource-group-name-requirements"></a>리소스 그룹 이름에 대한 요구 사항이 있나요?
예. 리소스 그룹 이름은 90자까지 지정할 수 있습니다. [명명 규칙 및 제한 사항](/azure/architecture/best-practices/naming-conventions#naming-rules-and-restrictions)을 참조하여 리소스 그룹에 대해 자세히 알아보세요.

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>VM을 만들 때의 사용자 이름 요구 사항은 무엇인가요?

사용자 이름은 20자까지 지정할 수 있으며 마침표(".")로 끝날 수 없습니다. 

다음 사용자 이름은 사용할 수 없습니다.

| | | | |
|-----------------|-----------|--------------------|----------|
| `administrator` | `admin`   | `user`             | `user1`  |
| `test`          | `user2`   | `test1`            | `user3`  |
| `admin1`        | `1`       | `123`              | `a`      |
| `actuser`       | `adm`     | `admin2`           | `aspnet` |
| `backup`        | `console` | `david`            | `guest`  |
| `john`          | `owner`   | `root`             | `server` |
| `sql`           | `support` | `support_388945a0` | `sys`    |
| `test2`         | `test3`   | `user4`            | `user5`  |


## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>VM을 만들 때의 암호 요구 사항은 무엇인가요?

다양 한 암호 길이 요구 사항을 사용 하는 도구에 따라
 - 포털-12 ~ 72 자 사이
 - 8 ~ 123 자 사이-PowerShell
 - CLI-12-123 사이

* 소문자 포함
* 대문자 포함
* 숫자 포함
* 특수 문자 포함(정규식 일치 [\W_])

사용할 수 없는 암호:

<table>
    <tr>
        <td>abc@123</td>
        <td>iloveyou!</td>
        <td>P@$$w0rd</td>
        <td>P@ssw0rd</td>
        <td>P@ssword123</td>
    </tr>
    <tr>
        <td>Pa$$word</td>
        <td>pass@word1</td>
        <td>Password!</td>
        <td>Password1</td>
        <td>Password22</td>
    </tr>
</table>
