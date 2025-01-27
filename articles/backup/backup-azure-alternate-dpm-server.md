---
title: Azure Backup Server에서 데이터 복구
description: Recovery Services 자격 증명 모음에 보호해 둔 데이터를 해당 자격 증명 모음에 등록된 모든 Azure Backup Server에서 복구할 수 있습니다.
services: backup
author: kasinh
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: kasinh
ms.openlocfilehash: 770baeeacb5f3808eba05f9e262bcbca75c6baad
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705221"
---
# <a name="recover-data-from-azure-backup-server"></a>Azure Backup Server에서 데이터 복구
Azure Backup Server를 사용하여 Recovery Services 자격 증명 모음으로 백업한 데이터를 복구할 수 있습니다. 이 과정이 Azure Backup Server 관리 콘솔에 통합되며 다른 Azure Backup 구성 요소의 복구 워크플로와 유사합니다.

> [!NOTE]
> 이 문서는 [최신 Azure Backup 에이전트](https://aka.ms/azurebackup_agent)와 결합된 [System Center Data Protection Manager 2012 R2 UR7 이상](https://support.microsoft.com/en-us/kb/3065246)에도 적용됩니다.
>
>

Azure Backup Server에서 데이터를 복구하려면

1. Azure Backup Server 관리 콘솔의 **복구** 탭에서 화면 왼쪽 상단에 있는 **‘외부 DPM 추가’** (화면 왼쪽 맨 위에 있음)를 클릭합니다.   
    ![외부 DPM 추가](./media/backup-azure-alternate-dpm-server/add-external-dpm.png)
2. 데이터를 복구할 **Azure Backup Server**와 연결된 자격 증명 모음에서 새 **보관 자격 증명**을 다운로드하고, Recovery Services 자격 증명 모음에 등록된 Azure Backup Server 목록에서 Azure Backup Server를 선택하고, 데이터를 복구할 서버에 연결된 **암호화 암호**를 제공합니다.

    ![외부 DPM 자격 증명](./media/backup-azure-alternate-dpm-server/external-dpm-credentials.png)

   > [!NOTE]
   > 같은 등록 저장소로 연결된 Azure Backup Server끼리만 데이터를 복구할 수 있습니다.
   >
   >

    외부 Azure Backup Server가 성공적으로 추가되면 **복구** 탭에서 외부 Azure Backup Server와 로컬 Azure Backup Server의 데이터를 찾아볼 수 있습니다.
3. 외부 Azure Backup Server에서 보호하는 사용 가능한 프로덕션 서버의 목록을 찾아보고 적절한 데이터 원본을 선택합니다.

    ![외부 DPM 서버 찾아보기](./media/backup-azure-alternate-dpm-server/browse-external-dpm.png)
4. **복구 지점** 드롭다운에서 **월 및 연도**를 선택하고 **복구 날짜**에서 복구 지점이 생성된 날짜를, **복구 시간**에서 시간을 선택합니다.

    아래 창에 파일 및 폴더의 목록이 표시되며 여기서 찾아 어떤 위치로든 복구할 수 있습니다.

    ![외부 DPM 서버 복구 지점](./media/backup-azure-alternate-dpm-server/external-dpm-recoverypoint.png)
5. 적절한 항목을 마우스 오른쪽 단추로 클릭하고 **복구**를 클릭합니다.

    ![외부 DPM 복구](./media/backup-azure-alternate-dpm-server/recover.png)
6. **복구 선택 사항**을 검토합니다. 복구될 백업 복사본의 날짜와 시간, 백업 복사본이 만들어진 원본을 확인합니다. 선택 항목이 올바르지 않으면 **취소** 를 클릭하여 다시 백업 탭으로 돌아가 적절한 복구 지점을 선택합니다. 선택 항목이 올바르면 **다음**을 클릭합니다.

    ![외부 DPM 복구 요약](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-summary.png)
7. **대체 위치로 복구**를 선택합니다. **찾아보기** 를 선택하여 복구할 올바른 위치를 찾습니다.

    ![외부 DPM 복구 대체 위치](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-alternate-location.png)
8. **복사본 만들기**, **건너뛰기** 또는 **덮어쓰기** 옵션 중에서 선택합니다.

   * **복사본 만들기** - 이름이 충돌할 경우 파일의 복사본을 만듭니다.
   * **건너뛰기** - 이름이 충돌할 경우 원본 파일을 남기고 파일은 복구하지 않습니다.
   * **덮어쓰기** - 이름이 충돌할 경우 기존 파일 복사본을 덮어씁니다.

     **복원 보안**에서 적절한 옵션을 선택합니다. 데이터를 복구할 대상 컴퓨터의 보안 설정을 적용하거나 복구 지점이 생성된 시기에 사용된 보안 설정을 적용할 수 있습니다.

     복구가 성공적으로 완료되면 **알림**을 전송할지 여부를 식별합니다.

     ![외부 DPM 복구 알림](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-notifications.png)
9. **요약** 화면에 지금까지 선택한 옵션이 나열됩니다. **'복구'** 를 클릭하면 데이터가 적절한 온-프레미스 위치에 복구됩니다.

    ![외부 DPM 복구 옵션 요약](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-options-summary.png)

   > [!NOTE]
   > 복구 작업은 Azure Backup Server의 **모니터링** 탭에서 모니터링할 수 있습니다.
   >
   >

    ![복구 모니터링](./media/backup-azure-alternate-dpm-server/monitoring-recovery.png)
10. 외부 DPM 서버를 보지 않으려면 **복구** 탭에서 **외부 DPM 지우기**를 클릭하면 됩니다.

    ![외부 DPM 지우기](./media/backup-azure-alternate-dpm-server/clear-external-dpm.png)

## <a name="troubleshooting-error-messages"></a>오류 메시지 문제 해결
| 아니요. | 오류 메시지 | 문제 해결 단계 |
|:---:|:--- |:--- |
| 1. |이 서버는 저장소 자격 증명을 통해 지정된 저장소에 등록되지 않았습니다. |**원인:** 이 오류는 선택한 보관 자격 증명 파일이 복구를 시도하려는 Azure Backup Server와 연결된 Recovery Services 자격 증명 모음에 속해 있지 않을 때 나타납니다. <br> **해결 방법:** Azure Backup Server가 등록된 Recovery Services 자격 증명 모음에서 보관 자격 증명 파일을 다운로드합니다. |
| 2. |복구 가능한 데이터가 없거나 선택한 서버가 DPM 서버가 아닙니다. |**원인:** 다른 Azure Backup 서버가 Recovery Services 자격 증명 모음에 등록 된 서버 메타 데이터를 아직 업로드 하지 또는 선택한 서버 (Windows Server 또는 Windows 클라이언트 사용) Azure Backup Server 아닙니다. <br> **해결 방법:** Recovery Services 자격 증명 모음에 다른 Azure Backup Server가 등록된 경우 최신 Azure Backup 에이전트가 설치되어 있는지 확인합니다. <br>다른 Azure Backup Server가 Recovery Services 자격 증명 모음에 등록된 경우, 설치하고 하루 동안 기다린 다음 복구 프로세스를 시작하세요. 야간 작업을 통해 보호된 모든 백업에 대한 메타데이터가 클라우드로 업로드됩니다. 이제 데이터를 복구할 수 있습니다. |
| 3. |이 저장소에 DPM 서버가 등록되어 있지 않습니다. |**원인:** 복구가 시도된 자격 증명 모음에 등록된 다른 Azure Backup Server가 없습니다.<br>**해결 방법:** Recovery Services 자격 증명 모음에 다른 Azure Backup Server가 등록된 경우 최신 Azure Backup 에이전트가 설치되어 있는지 확인합니다.<br>다른 Azure Backup Server가 Recovery Services 자격 증명 모음에 등록된 경우, 설치하고 하루 동안 기다린 다음 복구 프로세스를 시작하세요. 야간 작업을 통해 보호된 모든 백업에 대한 메타데이터가 클라우드로 업로드됩니다. 이제 데이터를 복구할 수 있습니다. |
| 4. |암호화 암호가 다음 서버에 연결 된 암호와 일치 하지 않습니다:  **\<서버 이름 >** |**원인:** 데이터를 복구하려는 Azure Backup Server의 데이터를 암호화하는 데 사용된 암호화 암호가 입력한 암호화 암호와 일치하지 않습니다. 에이전트는 데이터의 암호를 해독할 수 없습니다. 따라서 복구가 실패합니다.<br>**해결 방법:** 데이터를 복구하려는 Azure Backup Server에 연결된 암호화 암호를 정확하게 입력하세요. |

## <a name="next-steps"></a>다음 단계

다른 FAQ를 읽어보세요.

- [일반적인 질문](backup-azure-vm-backup-faq.md) Azure VM 백업에 대 한
- Azure Backup 에이전트에 대한 [일반 질문](backup-azure-file-folder-backup-faq.md)
