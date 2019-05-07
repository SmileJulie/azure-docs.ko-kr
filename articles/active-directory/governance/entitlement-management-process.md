---
title: Azure AD 권한 관리 (미리 보기)-Azure Active Directory에서에서 프로세스 및 전자 메일 알림을 요청합니다
description: 액세스 패키지 하 고 Azure Active Directory 권한 관리 (미리 보기)에 전자 메일 알림이 전송 될 때 요청 프로세스에 알아봅니다.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: mamtakumar
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/26/2019
ms.author: rolyon
ms.reviewer: mamkumar
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4ab18c8f165fc30636cd05091be1181743f9972d
ms.sourcegitcommit: 8a681ba0aaba07965a2adba84a8407282b5762b2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64873636"
---
# <a name="request-process-and-email-notifications-in-azure-ad-entitlement-management-preview"></a>Azure AD 권한 관리 (미리 보기)에서 프로세스 및 전자 메일 알림을 요청합니다

> [!IMPORTANT]
> Azure Active Directory (Azure AD) 권한 관리는 현재 공개 미리 보기로 제공 됩니다.
> 이 미리 보기 버전은 서비스 수준 계약 없이 제공되며 프로덕션 워크로드에는 사용하지 않는 것이 좋습니다. 특정 기능이 지원되지 않거나 기능이 제한될 수 있습니다.
> 자세한 내용은 [Microsoft Azure Preview에 대한 추가 사용 약관](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)을 참조하세요.

사용자가 패키지를 액세스 하는 요청을 제출 하면 해당 요청을 전달할 프로세스가 시작 됩니다. Azure AD 권한 관리도 전자 메일 알림을 보냅니다 요청자 및 승인자를 프로세스 중 주요 이벤트를 발생 합니다.

이 문서에서는 전송 되는 전자 메일 알림을 요청 프로세스를 설명 합니다.

## <a name="request-process"></a>요청 프로세스

액세스 패키지에 액세스 해야 하는 사용자에 대 한 액세스 요청을 제출할 수 있습니다. 정책의 구성에 따라 요청을 승인을 해야 합니다. 요청이 승인 되 면 액세스 패키지의 각 리소스에 대 한 사용자 액세스를 할당할 프로세스를 시작 합니다. 다음 다이어그램에서는 프로세스 및 여러 상태 개요를 보여 줍니다.

![승인 프로세스 다이어그램](./media/entitlement-management-process/request-process.png)

| 시스템 상태 | 설명 |
| --- | --- |
| 제출됨 | 사용자 요청을 제출 합니다. |
| 승인 보류 중 | 액세스 패키지 정책상 승인 요청을 승인 보류 중인 이동 합니다. |
| 만료됨 | 승인자가 없습니다. 승인 요청 시간 제한 내에서 요청을 검토 하는 경우 요청에 만료 됩니다. 다시 시도 하려면 사용자가 요청을 다시 제출 해야 합니다. |
| 거부됨 | 승인자가 요청을 거부합니다. |
| 승인됨 | 승인자가 요청을 승인 합니다. |
| 배달 | 사용자에 게 **되지** 되었습니다 액세스 패키지의 모든 리소스에 대 한 액세스를 할당 합니다. 외부 사용자 인 경우 사용자가 아직 리소스 디렉터리에 액세스 하 고 권한 프롬프트를 수락 합니다. |
| 배달됨 | 사용자가 액세스 패키지의 모든 리소스에 대 한 액세스를 할당 되었습니다. |
| 확장 액세스 | 확장 정책에서 허용 하는 경우 사용자는 할당을 확장 합니다. |
| 액세스 만료 | 액세스 패키지에 대 한 사용자가 만료 되었습니다. 다시 액세스이 사용자 요청을 제출 해야 합니다. |

## <a name="email-notifications"></a>전자 메일 알림

승인자는 액세스 요청을 승인 해야 할 때 및 액세스 요청을 완료 되 면 전자 메일 알림은 전송 됩니다. 요청자 인 경우에 전자 메일 알림은 요청 상태를 나타내는 보내집니다. 다음 다이어그램은 이러한 전자 메일 알림 때 전송 됩니다.

![권한 관리 전자 메일 프로세스](./media/entitlement-management-process/email-notifications.png)

다음 표에서 각 이러한 전자 메일 알림에 대 한 세부 정보를 제공합니다.

| # | 전자 메일 제목 | 전송 하는 경우 | 전송 |
| --- | --- | --- | --- |
| 1 | 작업 필요: 액세스 요청 검토 *[요청자]* 하 *[액세스 패키지]* 여 *[date]* | 요청자는 액세스 패키지에 대 한 요청을 제출 하는 경우 | 모든 승인자 |
| 2 | 작업 필요: 액세스 요청 검토 *[요청자]* 하 *[액세스 패키지]* 여 *[date]* | X 일 전에 승인 요청 시간 초과 | 모든 승인자 |
| 3 | 상태 알림: *[요청자]* 의 액세스 요청을 *[액세스 패키지]* 만료 | 경우 승인자를 승인 하거나 거부 하지 않으면 액세스 요청에서 요청 기간 내 | 요청자 |
| 4 | 상태 알림: *[요청자]* 에 대 한 액세스 요청 *[액세스 패키지]* 완료 | 첫 번째 승인자가 승인 하거나 액세스 요청을 거부 하면 | 모든 승인자 |
| 5 | 액세스가 거부 되었습니다 *[액세스 패키지]* | 경우 요청자 액세스가 거부 된 액세스 패키지 | 요청자 |
| 6 | 에 대 한 액세스를 이제 *[액세스 패키지]*  | 경우는 요청자에 게 부여한 액세스 패키지의 모든 리소스에 대 한 액세스 | 요청자 |
| 7 | 에 대 한 액세스 *[액세스 패키지]* x에서 일 후 만료 됩니다 | 액세스 패키지에 대 한 요청자 액세스 하기 전에 X 일 만료 됩니다. | 요청자 |
| 8 | 에 대 한 액세스 *[액세스 패키지]* 만료 | 액세스 패키지에 대 한 요청자 액세스 만료 될 때 | 요청자 |

### <a name="review-access-request-emails"></a>검토 액세스 요청 전자 메일

요청 자가 승인이 필요 하도록 구성 된 액세스 패키지 액세스 요청을 제출 하면 정책에 구성 된 모든 승인자 요청의 세부 정보를 사용 하 여 전자 메일 알림을 받습니다. 세부 정보는 요청자의 이름, 조직, 시작 및 종료 날짜를 제공 하면 비즈니스 근거는 요청이 제출 된 시간과 요청의 만료 시점에 액세스 합니다. 전자 메일 승인자가 승인 또는 액세스 요청을 거부할 수 있는 링크를 포함 합니다. 샘플 메일 알림을 요청 자가 액세스 요청을 전송할 때 승인자에 게 전송 되는 다음과 같습니다.

![검토 요청 전자 메일 액세스](./media/entitlement-management-shared/email-approve-request.png)

### <a name="approved-or-denied-emails"></a>승인 또는 거부 전자 메일

요청자에 대 한 액세스 요청 승인 및 액세스용으로 사용할 수 있는 경우 또는 해당 액세스 요청이 거부 되 면 알림이 표시 됩니다. 때 승인자에 게 요청 자가 제출한 액세스 요청 검토, 승인 하거나 액세스 요청을 거부 수 있습니다. 승인자는 자신의 결정에 대 한 비즈니스 근거를 추가 해야 합니다.

액세스 요청 승인 되 면 권한 관리는 각 액세스 패키지의 리소스에 대 한 요청자 액세스를 부여 하는 프로세스를 시작 합니다. 요청자에 게 부여 된 액세스 패키지의 모든 리소스에 대 한 액세스 이제 액세스 패키지에 액세스할 수 있는 및 해당 액세스 요청이 승인 되었음을 요청자에 게 전자 메일 알림이 전송 됩니다. 샘플 메일 알림을 액세스 패키지에 대 한 액세스를 부여 하는 경우에 요청자에 게 전송 되는 다음과 같습니다.

액세스 요청을 거부 되 면 요청자에 게 전자 메일 알림이 전송 됩니다. 해당 액세스 요청이 거부 될 때를 요청자에 게 전송 되는 샘플 메일 알림을 다음과 같습니다.

### <a name="expired-access-request-emails"></a>액세스 요청 전자 메일을 만료

요청자에 대 한 액세스 요청을 초과한 경우 알림이 표시 됩니다. 요청자가 액세스 요청을 제출 하면 요청이 만료 되기까지의 요청 기간을 있습니다. 없는 승인자는 승인/거부 의사 결정을 제출 하는 경우 요청이 승인 보류 중 상태로 유지 계속 합니다. 요청이 구성 된 만료 기간이 도달 하면 요청 만료 되 면 및 더 이상 승인 하거나 거부할 수는 승인자가 합니다. 이 경우 요청 만료 된 상태로 전환 합니다. 더 이상 만료 된 요청을 승인 하거나 거부할 수입니다. 만료 된 액세스 요청 하 고 액세스 요청을 다시 제출 해야 하는 요청자에 게 전자 메일 알림이 전송 됩니다. 액세스 요청을 초과한 경우에 요청자에 게 전송 되는 샘플 메일 알림을 다음과 같습니다.

![액세스 요청 전자 메일을 만료](./media/entitlement-management-process/email-expired-access-request.png)

## <a name="next-steps"></a>다음 단계

- [액세스 패키지에 대 한 액세스 요청](entitlement-management-request-access.md)
- [승인 하거나 거부할 액세스 요청](entitlement-management-request-approve.md)