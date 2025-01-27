---
title: Azure Backup 서비스를 사용하여 VM 설정에서 Azure VM 백업
description: Azure Backup 서비스를 사용하여 Azure VM을 백업하는 방법 알아보기
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 06/13/2019
ms.author: raynew
ms.openlocfilehash: 80739fac8317014c74c6a86cef9aa23696cfb42e
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442993"
---
# <a name="back-up-an-azure-vm-from-the-vm-settings"></a>VM 설정에서 Azure VM 백업

이 문서에서는 [Azure Backup](backup-overview.md) 서비스를 사용하여 Azure VM을 백업하는 방법을 설명합니다. 다음과 같은 몇 가지 방법을 사용하여 Azure VM을 백업할 수 있습니다.

- 단일 Azure VM: 이 문서의 지침에서는 VM 설정에서 직접 Azure VM을 백업하는 방법을 설명합니다.
- 여러 Azure VM: Recovery Services 자격 증명 모음을 설정하고 여러 Azure VM에 대한 백업을 구성할 수 있습니다. 이 시나리오에 대해서는 [이 문서](backup-azure-arm-vms-prepare.md)의 지침을 따르세요.



## <a name="before-you-start"></a>시작하기 전에

1. 백업 작동 방식을 [알아보고](backup-architecture.md#how-does-azure-backup-work) 지원 요구 사항을 [확인](backup-support-matrix.md#azure-vm-backup-support)합니다.
2. Azure VM 백업의 [개요를 살펴봅니다](backup-azure-vms-introduction.md).

### <a name="azure-vm-agent-installation"></a>Azure VM 에이전트 설치

Azure VM을 백업하기 위해 Azure Backup은 컴퓨터에서 실행되는 VM 에이전트에 확장을 설치합니다. Azure Marketplace 이미지에서 VM을 만든 경우 에이전트가 실행됩니다. 경우에 따라 사용자 지정 VM을 만들거나 온-프레미스에서 컴퓨터를 마이그레이션할 수 있습니다. 이러한 경우 에이전트를 수동으로 설치해야 할 수 있습니다.

- VM 에이전트를 수동으로 설치해야 하는 경우 [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) 또는 [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) VM에 대한 지침을 따릅니다.
- 에이전트를 설치한 후 백업을 사용하도록 설정하면, Azure Backup은 에이전트에 백업 확장을 설치합니다. 사용자 개입 없이 확장을 업데이트하고 패치합니다.

## <a name="back-up-from-azure-vm-settings"></a>Azure VM 설정에서 백업


1. [Azure Portal](https://portal.azure.com/)에 로그인합니다.
2. **모든 서비스**를 클릭하고 필터에 **가상 머신**을 입력한 후 **가상 머신**을 클릭합니다.
3. VM 목록에서 백업하려는 VM을 선택합니다.
4. VM 메뉴에서 **백업**을 클릭합니다.
5. **Recovery Services 자격 증명 모음**에서 다음을 수행합니다.
   - 자격 증명 모음에 이미 있는 경우 **기존 항목 선택**을 클릭하고 자격 증명 모음을 선택합니다.
   - 자격 증명 모음에 없는 경우 **새로 만들기**를 클릭합니다. 자격 증명 모음의 이름을 지정합니다. 해당 자격 증명 모음은 VM과 동일한 지역 및 리소스 그룹에 생성됩니다. VM 설정에서 직접 백업을 사용하도록 설정하면 이러한 설정을 수정할 수 없습니다.

   ![Backup 마법사 사용](./media/backup-azure-vms-first-look-arm/vm-menu-enable-backup-small.png)

6. **백업 정책 선택**에서 다음을 수행합니다.

   - 기본 정책을 그대로 둡니다. 이렇게 하면 지정된 시간에 하루 1번 VM이 백업되고, 30일 동안 자격 증명 모음에 백업이 유지됩니다.
   - 기존 백업 정책이 있는 경우 선택합니다.
   - 새 정책을 만들고 정책 설정을 정의합니다.  

   ![백업 정책 선택](./media/backup-azure-vms-first-look-arm/set-backup-policy.png)

7. **백업 사용**을 클릭합니다. 이렇게 하면 백업 정책이 VM에 연결됩니다.

    ![Backup 사용 단추](./media/backup-azure-vms-first-look-arm/vm-management-menu-enable-backup-button.png)

8. 포털 알림에서 구성 진행률을 추적할 수 있습니다.
9. 작업이 완료되면 VM 메뉴에서 **백업**을 클릭합니다. 이 페이지는 VM의 백업 상태, 복구 지점에 대한 정보, 실행 중인 작업 및 발생한 경고를 표시합니다.

   ![백업 상태](./media/backup-azure-vms-first-look-arm/backup-item-view-update.png)

10. Backup을 사용 하도록 설정한 후 초기 백업을 실행 합니다. 초기 백업을 즉시 시작하거나, 백업 일정에 따라 시작될 때까지 기다릴 수 있습니다.
    - 초기 백업이 완료될 때까지 **마지막 백업 상태**는 **경고(초기 백업 보류 중)** 으로 표시됩니다.
    - 예약된 다음 백업이 실행될 시기를 보려면 백업 정책 이름을 클릭합니다.


> [!NOTE]
> Azure Backup 서비스는 명명 형식 사용 하 여 스냅숏을 저장 하려면 (VM 리소스 그룹)을 제외한 별도 리소스 그룹을 만듭니다 **AzureBackupRG_geography_number** (예: AzureBackupRG_northeurope_1). 이 리소스 그룹의 데이터를 Azure 가상 머신 백업 정책의 "보존 인스턴트 복구 스냅숏" 섹션에서 지정 된 대로 일 기간에 대 한 유지 됩니다. 이 리소스 그룹에 잠금을 적용 하면 백업 오류가 발생할 수 있습니다.<br>
제한 정책을 다시 백업 실패를 유발에 리소스 지점 컬렉션을 만들 차단 하는 대로도이 리소스 그룹 이름/태그 제한에서 제외 해야 합니다.


## <a name="run-a-backup-immediately"></a>백업 즉시 실행

1. 백업을 즉시 실행하려면 VM 메뉴에서 **백업** > **지금 백업**을 클릭합니다.

    ![백업 실행](./media/backup-azure-vms-first-look-arm/backup-now-update.png)

2. **지금 백업**에서 달력 컨트롤을 사용하여 복구 지점을 보존할 기간을 선택하고 **확인**을 선택합니다.

    ![백업 보존일](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

3. 포털 알림을 통해 백업 작업이 트리거되었는지 알 수 있습니다. 백업 진행률을 모니터링하려면 **모든 작업 보기**를 클릭합니다.




## <a name="back-up-from-the-recovery-services-vault"></a>Recovery Services 자격 증명 모음에서 백업

이 문서의 지침에 따라 Azure Backup Recovery Services 자격 증명 모음을 설정하고 자격 증명 모음에서 백업을 사용하도록 설정하여 Azure VM에 대해 백업을 설정합니다.

## <a name="next-steps"></a>다음 단계

- 이 문서의 절차를 수행하는 데 문제가 있으면 [문제 해결 가이드](backup-azure-vms-troubleshoot.md)를 참조하세요.
- 백업 관리에 대해 [자세히 알아봅니다](backup-azure-manage-vms.md).
