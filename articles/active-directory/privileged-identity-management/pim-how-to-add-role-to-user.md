---
title: PIM-Azure Active Directory에서에서 Azure AD 역할 할당 | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM)에서 Azure AD 역할을 할당 하는 방법에 알아봅니다.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: rolyon
ms.collection: M365-identity-device-management
ms.openlocfilehash: e1760d0e0bd356a05d84c07eda005e0526da5d13
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476519"
---
# <a name="assign-azure-ad-roles-in-pim"></a>PIM에서 Azure AD 역할 할당

Azure Active Directory (Azure AD)를 사용 하 여 전역 관리자를 만들 수 있습니다 **영구** Azure AD 관리자 역할 할당 합니다. 이러한 역할은 [Azure Portal](../users-groups-roles/directory-assign-admin-roles.md) 또는 [PowerShell 명령](/powershell/module/azuread#directory_roles)을 사용하여 할당할 수 있습니다.

또한 Azure AD Privileged Identity Management (PIM) 서비스에는 권한 있는 역할 관리자가 영구 관리자 역할 할당을 확인 수 있습니다. 또한 권한 있는 역할 관리자가 사용자가 가능 **적격** Azure AD 관리자 역할에 대 한 합니다. 적격인 관리자는 필요할 때 역할을 활성화할 수 있으며 작업을 완료하고 나면 권한이 만료됩니다.

## <a name="make-a-user-eligible-for-a-role"></a>사용자를 역할에 적격 사용자로 지정

사용자를 Azure AD 관리자 역할에 대 한 적격 상태로 설정 하려면 다음이 단계를 따릅니다.

1. [권한 있는 역할 관리자](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) 역할의 구성원인 사용자로 [Azure Portal](https://portal.azure.com/)에 로그인합니다.

    다른 관리자에게 PIM 관리를 위한 액세스 권한을 부여하는 방법에 대한 자세한 내용은 [다른 관리자에게 PIM을 관리하기 위한 액세스 권한 부여](pim-how-to-give-access-to-pim.md)를 참조하세요.

1. **Azure AD Privileged Identity Management**를 엽니다.

    Azure Portal에서 PIM을 아직 시작하지 않은 경우 [PIM 사용](pim-getting-started.md)으로 이동합니다.

1. **Azure AD 역할**을 클릭합니다.

1. **역할** 또는 **멤버**를 클릭합니다.

    ![역할 및 멤버에 해당 하는 메뉴의 옵션을 사용 하 여 azure AD 역할 강조 표시](./media/pim-how-to-add-role-to-user/pim-directory-roles.png)

1. **멤버 추가**를 클릭하여 관리되는 멤버 추가를 엽니다.

1. **역할 선택**을 클릭하고 관리하려는 역할을 클릭한 후 **선택**을 클릭합니다.

    ![Azure AD 역할을 나열 하는 역할 창 선택](./media/pim-how-to-add-role-to-user/pim-select-a-role.png)

1. **멤버 선택**을 클릭하고 역할에 할당할 사용자를 선택한 후 **선택**을 클릭합니다.

    ![사용자를 선택할 수 있는 멤버 창 선택](./media/pim-how-to-add-role-to-user/pim-select-members.png)

1. 관리되는 멤버 추가에서 **확인**을 클릭하여 역할에 사용자를 추가합니다.

1. 역할 목록에서 방금 할당한 역할을 클릭하여 멤버 목록을 봅니다.

     역할이 할당되면 선택한 사용자가 멤버 목록에 해당 역할의 **적격** 사용자로 표시됩니다.

    ![역할의 멤버가 해당 정품 인증 상태와 함께 나열 됩니다.](./media/pim-how-to-add-role-to-user/pim-directory-role-eligible.png)

1. 사용자가 역할을 수행할 수는 이제 알 수 있도록의 지침에 따라 활성화할 수 있음을 [PIM에서 내 Azure AD 역할 활성화](pim-how-to-activate-role.md)합니다.

    적격 관리자의 경우 활성화 중에 Azure MFA(Multi-factor Authentication)에 등록하라는 메시지가 표시됩니다. 사용자가 MFA에 등록할 수 없거나 Microsoft 계정(일반적으로 @outlook.com)을 사용하고 있는 경우 사용자 정보는 모든 역할에서 영구적으로 유지되도록 해야 합니다.

## <a name="make-a-role-assignment-permanent"></a>역할 할당을 영구적으로 지정

기본적으로 새 자격이 있는 사용자만 Azure AD 관리자 역할을 합니다. 역할 할당을 영구적으로 지정하려면 다음 단계를 수행합니다.

1. **Azure AD Privileged Identity Management**를 엽니다.

1. **Azure AD 역할**을 클릭합니다.

1. **구성원**을 클릭합니다.

    ![Azure AD 역할-멤버 목록을 보여 주는 역할 및 활성화 상태](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members.png)

1. 영구적으로 유지하려는 **적격** 역할을 클릭합니다.

1. **추가**를 클릭하고 **영구 상태로 만들기**를 클릭합니다.

    ![더 많은 메뉴 옵션을 사용 하 여 역할에 대 한 적합 한 사용자를 나열 하는 창 열기](./media/pim-how-to-add-role-to-user/pim-make-perm.png)

    이제 역할이 **영구**로 표시됩니다.

    ![영구는 역할 및 활성화 상태를 표시 하는 멤버 목록](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members-permanent.png)

## <a name="remove-a-user-from-a-role"></a>역할에서 사용자 제거

역할 할당에서 사용자를 제거할 수 있지만 영구 전역 관리자인 사용자가 최소 한 명은 항상 있어야 합니다. 사용자에게 해당 역할 할당이 여전히 필요한지 확실하지 않은 경우에는 [역할에 대한 액세스 권한을 검토하기 시작](pim-how-to-start-security-review.md)할 수 있습니다.

특정 사용자를 Azure AD 관리자 역할을 제거 하려면 다음이 단계를 수행 합니다.

1. **Azure AD Privileged Identity Management**를 엽니다.

1. **Azure AD 역할**을 클릭합니다.

1. **구성원**을 클릭합니다.

    ![Azure AD 역할-멤버 목록을 보여 주는 역할 및 활성화 상태](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members.png)

1. 제거할 역할 할당을 클릭합니다.

1. **추가**를 클릭한 후 **제거**를 클릭합니다.

    ![더 많은 메뉴 옵션을 사용 하 여 영구 역할이 있는 사용자를 나열 하는 창 열기](./media/pim-how-to-add-role-to-user/pim-remove-role.png)

1. 확인하라는 메시지에서 **예**를 클릭합니다.

    ![역할에서 멤버를 제거할 것인지 묻는 메시지](./media/pim-how-to-add-role-to-user/pim-remove-role-confirm.png)

    역할 할당이 제거됩니다.

## <a name="authorization-error-when-assigning-roles"></a>역할을 할당할 때 권한 부여 오류

최근에 구독에 대 한 PIM을 설정 하 고 사용자를 Azure AD 관리자 역할에 대 한 적격 상태로 설정 하려고 할 때 권한 부여 오류가 발생 하는 경우 MS PIM 서비스 주체에 아직 적절 한 권한이 없기 때문에 수 있습니다. MS-PIM 서비스 주체에는 다른 사용자에게 역할을 할당하는 [사용자 액세스 관리자](../../role-based-access-control/built-in-roles.md#user-access-administrator) 역할이 있어야 합니다. MS-PIM에 사용자 액세스 관리자 역할이 할당될 때까지 대기하는 대신 수동으로 할당할 수 있습니다.

다음의 단계를 따라 구독의 MS-PIM 서비스 주체에 사용자 액세스 관리자 역할을 할당합니다.

1. 글로벌 관리자 권한으로 Azure Portal에 로그인합니다.

1. **모든 서비스**, **구독**을 차례로 선택합니다.

1. 구독을 선택합니다.

1. **액세스 제어(IAM)** 를 선택합니다.

1. **역할 할당**을 선택하면 구독 범위의 현재 역할 할당 목록을 볼 수 있습니다.

   ![구독의 액세스 제어(IAM) 블레이드](./media/pim-how-to-add-role-to-user/ms-pim-access-control.png)

1. **MS-PIM** 서비스 주체가 **사용자 액세스 관리자** 역할로 할당되었는지 여부를 확인합니다.

1. 할당되지 않은 경우 **역할 할당 추가**를 선택하여 **역할 할당 추가** 창을 엽니다.

1. **역할** 드롭다운 목록에서 **사용자 액세스 관리자** 역할을 선택합니다.

1. **선택** 목록에서 **MS-PIM** 서비스 주체를 찾고 선택합니다.

   ![추가 역할 할당 창-MS PIM 서비스 주체에 대 한 권한 추가](./media/pim-how-to-add-role-to-user/ms-pim-add-permissions.png)

1. **저장**을 선택하여 역할을 할당합니다.

   몇 분 후 MS-PIM 서비스 주체가 구독 범위에서 사용자 액세스 관리자 역할로 할당됩니다.

   ![사용자 액세스 관리자 MS PIM에 대 한 역할 할당을 표시 하는 액세스 제어 (IAM) 블레이드](./media/pim-how-to-add-role-to-user/ms-pim-user-access-administrator.png)


## <a name="next-steps"></a>다음 단계

- [PIM에서 Azure AD 관리 역할 설정 구성](pim-how-to-change-default-settings.md)
- [PIM에서 Azure 리소스 역할 할당](pim-resource-roles-assign-roles.md)
