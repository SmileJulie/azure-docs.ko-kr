---
title: 'Azure Active Directory Domain Services: 시작 | Microsoft Docs'
description: Azure Portal을 사용하여 Azure Active Directory Domain Services 활성화
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: iainfou
ms.openlocfilehash: f82fee635dbca4be8f22d9e63dfea40173f472e9
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67474302"
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal"></a>Azure Portal을 사용하여 Azure Active Directory Domain Services 활성화

## <a name="task-3-configure-administrative-group"></a>작업 3: 관리 그룹 구성

이 구성 작업에서는 Azure AD 디렉터리에 관리 그룹을 만듭니다. 이 특수 관리 그룹을 *AAD DC Administrators*라고 합니다. 이 그룹의 구성원에게는 관리되는 도메인에 연결된 컴퓨터에 대한 관리자 권한이 부여됩니다. 이 그룹은 도메인 가입 컴퓨터에서 Administrators 그룹에 추가됩니다. 또한 이 그룹의 구성원은 원격 데스크톱을 사용하여 도메인에 가입한 컴퓨터에 원격으로 연결할 수 있습니다.

> [!NOTE]
> Azure Active Directory Domain Services를 사용하여 만든 관리되는 도메인에 도메인 관리자 또는 엔터프라이즈 관리자 권한이 없습니다. 관리되는 도메인에서 이러한 권한은 서비스별로 예약되며 테넌트 내의 사용자가 사용할 수 없습니다. 하지만, 권한 있는 작업을 수행하기 위해 이 구성 태스크를 통해 만든 특수 관리자 그룹을 사용할 수 있습니다. 이 작업에는 도메인에 가입한 컴퓨터에서 관리자 그룹에 속하는 도메인에 컴퓨터를 가입시키고, 그룹 정책을 구성하는 작업이 포함됩니다.
>

마법사가 자동으로 Azure AD 디렉터리에 관리 그룹을 만듭니다. 이 그룹을 'AAD DC Administrators'라고 합니다. Azure AD 디렉터리에 이 이름 가진 기존 그룹이 있는 경우 마법사는 이 그룹을 선택합니다. **관리자 그룹** 마법사 페이지를 사용하여 그룹 멤버 자격을 구성할 수 있습니다.

1. 그룹 멤버 자격을 구성하려면 **AAD DC Administrators**를 클릭합니다.

    ![그룹 멤버 자격 구성](./media/getting-started/domain-services-blade-admingroup.png)

2. **멤버 추가** 단추를 클릭하여 Azure AD 디렉터리의 사용자를 관리자 그룹에 추가합니다.

3. 완료되면 **확인**을 클릭하여 마법사의 **요약** 페이지로 이동합니다.

## <a name="configure-synchronization"></a>동기화 구성

Azure AD Domain Services에서는 Azure AD에서 사용할 수 있는 모든 사용자 및 그룹의 전체 동기화 중 하나를 허용하거나 특정 그룹만 동기화하려면 범위가 지정된 동기화를 선택할 수 있습니다. 전체 동기화를 선택하는 경우 나중에 범위가 지정된 동기화를 선택할 수 **없습니다**. 범위가 지정된 동기화에 대한 자세한 내용은 [Azure AD Domain Services 범위가 지정된 동기화 문서](scoped-synchronization.md)를 참조하세요.

### <a name="full-synchronization"></a>전체 동기화

1. 전체 동기화의 경우 이미 전체가 선택되어 있으므로 화면 맨 아래에서 "확인"만 클릭하면 됩니다.
    ![전체 동기화](./media/active-directory-domain-services-admin-guide/create-sync-all.PNG)

### <a name="scoped-synchronization"></a>범위가 지정된 동기화

1. 동기화 단추를 "범위 지정됨"으로 설정/해제하면 그룹 선택 페이지가 표시됩니다. 이 페이지에서 관리형 도메인에 동기화되도록 이미 선택된 그룹을 확인할 수 있습니다.
    ![범위가 지정된 동기화](media/active-directory-domain-services-admin-guide/create-sync-scoped.PNG)
2. 맨 위 탐색 모음에서 **그룹 선택**을 클릭합니다. 여기에서 그룹 선택기가 측면에 표시됩니다. 이 선택기를 사용하여 Azure AD Domain Services로 동기화할 모든 추가 그룹을 선택합니다. 완료되면 **선택**을 클릭하여 그룹 선택기를 닫고 선택한 목록에 해당 그룹을 추가합니다.
    ![범위가 지정된 동기화 그룹 선택](media/active-directory-domain-services-admin-guide/create-sync-scoped-groupselect.PNG)
3. **확인**을 클릭하여 요약 페이지로 이동합니다.

## <a name="deploy-your-managed-domain"></a>관리되는 도메인 배포

1. 마법사의 **요약** 페이지에서 관리되는 도메인에 대한 구성 설정을 검토합니다. 필요한 경우 마법사의 임의 단계로 돌아가서 변경할 수 있습니다. 완료되면 **확인**을 클릭하여 새 관리되는 도메인을 만듭니다.

    ![요약](./media/getting-started/domain-services-blade-summary.png)

2. Azure AD Domain Services 배포의 진행 상황을 보여 주는 알림이 표시됩니다. 자세한 배포 진행률을 확인하려면 알림을 클릭합니다.

    ![알림 - 배포 진행 중](./media/getting-started/domain-services-blade-deployment-in-progress.png)

## <a name="check-the-deployment-status-of-your-managed-domain"></a>관리되는 도메인의 배포 상태 확인

관리되는 도메인을 프로비전하는 프로세스는 최대 한 시간 정도 걸릴 수 있습니다.

1. 배포가 진행되는 동안 **리소스 검색** 검색 상자에서 'Domain Services'를 검색할 수 있습니다. 검색 결과에서 **Azure AD Domain Services**를 선택합니다. **Azure AD Domain Services** 블레이드에 프로비전되는 관리되는 도메인이 나열됩니다.

    ![프로비전되는 관리되는 도메인 찾기](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. 관리되는 도메인에 대한 자세한 내용을 보려면 관리되는 도메인의 이름(예: 'contoso100.com')을 클릭합니다.

    ![Domain Services - 프로비저닝 상태](./media/getting-started/domain-services-provisioning-state.png)

3. **개요** 탭은 관리되는 도메인이 현재 프로비전되고 있음을 보여 줍니다. 완전히 프로비전될 때까지 관리되는 도메인을 구성할 수 없습니다. 관리되는 도메인이 완전히 프로비전되려면 최대 한 시간 정도 걸릴 수 있습니다.

    ![Domain Services - 프로비전 상태 중 개요 탭](./media/getting-started/domain-services-provisioning-state-details.png)

4. 관리되는 도메인이 완전히 프로비전되면 **개요** 탭에 도메인 상태가 **실행 중**으로 표시됩니다.

    ![Domain Services - 완전히 프로비전한 후 개요 탭](./media/getting-started/domain-services-provisioned.png)
    >[!NOTE]
    >프로비전 프로세스 중 Azure AD Domain Services는 디렉터리 내에서 "도메인 컨트롤러 서비스" 및 "AzureActiveDirectoryDomainControllerServices"라는 Enterprise 애플리케이션을 만듭니다. 이러한 Enterprise 애플리케이션은 관리되는 도메인을 제공하는 데 필요합니다. 언제든지 삭제되지 않으므로 명령적입니다.
    >

5. **속성** 탭에 가상 네트워크에 도메인 컨트롤러를 사용할 수 있는 두 개의 IP 주소가 표시됩니다.

    ![Domain Services - 완전히 프로비전한 후 속성 탭](./media/getting-started/domain-services-provisioned-properties.png)

## <a name="need-help"></a>도움 필요 시

관리되는 도메인의 도메인 컨트롤러 모두를 프로비전하는 데 한두 시간 정도가 걸릴 수 있습니다. 배포에 실패하거나 2시간 넘게 '보류 중' 상태인 경우 [제품팀에 문의](contact-us.md)하는 것이 좋습니다.

## <a name="next-step"></a>다음 단계

[작업 4: Azure 가상 네트워크에 대한 DNS 설정 업데이트](active-directory-ds-getting-started-dns.md)
