---
title: '자습서: ClickUp Productivity 플랫폼과 Azure Active Directory 통합 | Microsoft Docs'
description: Azure Active Directory 및 ClickUp Productivity 플랫폼 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 06853edd-e8da-4ad2-a4e6-5555d592cee5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/21/2019
ms.author: jeedes
ms.openlocfilehash: a6958e88e7e20b94a54216a92651c2f6d3fe650e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67105307"
---
# <a name="tutorial-azure-active-directory-integration-with-clickup-productivity-platform"></a>자습서: ClickUp Productivity 플랫폼과 Azure Active Directory 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 ClickUp Productivity 플랫폼을 통합하는 방법에 대해 알아봅니다.
Azure AD와 ClickUp Productivity 플랫폼을 통합하면 다음과 같은 이점이 제공됩니다.

* ClickUp Productivity 플랫폼에 대한 액세스 권한이 있는 사용자를 Azure AD에서 제어할 수 있습니다.
* 사용자가 해당 Azure AD 계정으로 ClickUp Productivity 플랫폼에 자동으로 로그인(Single Sign-On)되도록 설정할 수 있습니다.
* 단일 중앙 위치인 Azure Portal에서 계정을 관리할 수 있습니다.

Azure AD와의 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory를 사용한 응용 프로그램 액세스 및 Single Sign-On](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)을 참조하세요.
Azure 구독이 아직 없는 경우 시작하기 전에 [체험](https://azure.microsoft.com/free/) 계정을 만듭니다.

## <a name="prerequisites"></a>필수 조건

ClickUp Productivity 플랫폼과 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

* Azure AD 구독 Azure AD 환경이 없으면 [여기](https://azure.microsoft.com/pricing/free-trial/)에서 1개월 평가판을 구할 수 있습니다.
* ClickUp Productivity 플랫폼 Single Sign-On이 설정된 구독

## <a name="scenario-description"></a>시나리오 설명

이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

* ClickUp Productivity 플랫폼에서 **SP** 시작 SSO를 지원합니다.

## <a name="adding-clickup-productivity-platform-from-the-gallery"></a>갤러리에서 ClickUp Productivity 플랫폼 추가

ClickUp Productivity 플랫폼의 Azure AD 통합을 구성하려면 갤러리의 ClickUp Productivity 플랫폼을 관리되는 SaaS 앱 목록에 추가해야 합니다.

**갤러리에서 ClickUp Productivity 플랫폼을 추가하려면 다음 단계를 수행합니다.**

1. **[Azure Portal](https://portal.azure.com)** 의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다.

    ![Azure Active Directory 단추](common/select-azuread.png)

2. **엔터프라이즈 응용 프로그램**으로 이동한 다음, **모든 응용 프로그램** 옵션을 선택합니다.

    ![엔터프라이즈 애플리케이션 블레이드](common/enterprise-applications.png)

3. 새 애플리케이션을 추가하려면 대화 상자 맨 위 있는 **새 애플리케이션** 단추를 클릭합니다.

    ![새 애플리케이션 단추](common/add-new-app.png)

4. 검색 상자에 **ClickUp Productivity 플랫폼**을 입력하고, 결과 패널에서 **ClickUp Productivity 플랫폼**을 선택한 다음, **추가** 단추를 클릭하여 애플리케이션을 추가합니다.

     ![결과 목록의 ClickUp Productivity 플랫폼](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성 및 테스트

이 섹션에서는 **Britta Simon**이라는 테스트 사용자를 기반으로 ClickUp Productivity 플랫폼에서 Azure AD Single Sign-On을 구성하고 테스트합니다.
Single Sign-On이 작동하려면 Azure AD 사용자와 ClickUp Productivity 플랫폼의 관련 사용자 간에 연결 관계를 설정해야 합니다.

ClickUp Productivity 플랫폼에서 Azure AD Single Sign-On을 구성하고 테스트하려면 다음 구성 요소를 완료해야 합니다.

1. **[Azure AD Single Sign-On 구성](#configure-azure-ad-single-sign-on)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
2. **[ClickUp Productivity 플랫폼 Single Sign-On 구성](#configure-clickup-productivity-platform-single-sign-on)** - 애플리케이션 쪽에서 Single Sign-On 설정을 구성합니다.
3. **[Azure AD 테스트 사용자 만들기](#create-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On을 테스트하는 데 사용합니다.
4. **[Azure AD 테스트 사용자 할당](#assign-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
5. **[ClickUp Productivity 플랫폼 테스트 사용자 만들기](#create-clickup-productivity-platform-test-user)** - Britta Simon의 Azure AD 표현과 연결된 해당 사용자를 ClickUp Productivity 플랫폼에 만듭니다.
6. **[Single Sign-On 테스트](#test-single-sign-on)** - 구성이 작동하는지 여부를 확인합니다.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure Portal에서 Azure AD Single Sign-On을 사용하도록 설정합니다.

ClickUp Productivity 플랫폼에서 Azure AD Single Sign-On을 구성하려면 다음 단계를 수행합니다.

1. [Azure Portal](https://portal.azure.com/)의 **ClickUp Productivity 플랫폼** 애플리케이션 통합 페이지에서 **Single Sign-On**을 선택합니다.

    ![Single Sign-On 구성 링크](common/select-sso.png)

2. **Single Sign-On 방법 선택** 대화 상자에서 **SAML/WS-Fed** 모드를 선택하여 Single Sign-On을 사용하도록 설정합니다.

    ![Single Sign-On 선택 모드](common/select-saml-option.png)

3. **SAML로 Single Sign-On 설정** 페이지에서 **편집** 아이콘을 클릭하여 **기본 SAML 구성** 대화 상자를 엽니다.

    ![기본 SAML 구성 편집](common/edit-urls.png)

4. **기본 SAML 구성** 섹션에서 다음 단계를 수행합니다.

    ![ClickUp Productivity 플랫폼 도메인 및 URL Single Sign-On 정보](common/sp-identifier.png)

    a. **로그온 URL** 텍스트 상자에 다음 URL을 입력합니다. `https://app.clickup.com/login/sso`

    b. **식별자(엔터티 ID)** 텍스트 상자에서 `https://api.clickup.com/v1/team/<team_id>/microsoft` 패턴을 사용하는 URL을 입력합니다.

    > [!NOTE]
    > 식별자 값은 실제 값이 아닙니다. 자습서 뒷부분에 설명된 실제 식별자 값으로 이 값을 업데이트합니다.

5. **SAML로 Single Sign-On 설정** 페이지의 **SAML 서명 인증서** 섹션에서 복사 단추를 클릭하여 **앱 페더레이션 메타데이터 URL**을 복사한 후 컴퓨터에 저장합니다.

    ![인증서 다운로드 링크](common/copy-metadataurl.png)

### <a name="configure-clickup-productivity-platform-single-sign-on"></a>ClickUp Productivity 플랫폼 Single Sign-On이 설정된 구독

1. 다른 웹 브라우저 창에서 ClickUp Productivity 플랫폼 테넌트에 관리자로 로그인합니다.

2. **사용자 프로필**을 클릭하고 **설정**을 선택합니다.

    ![ClickUp Productivity 구성](./media/clickup-productivity-platform-tutorial/configure1.png)

3. SSO(Single Sign-On) 공급 기업에서 **Microsoft**를 선택합니다.

    ![ClickUp Productivity 구성](./media/clickup-productivity-platform-tutorial/configure2.png)

4. **Microsoft Single Sign On 구성** 페이지에서 다음 단계를 수행합니다.

    ![ClickUp Productivity 구성](./media/clickup-productivity-platform-tutorial/configure3.png)

    a. **복사**를 클릭하여 엔터티 ID 값을 복사하고 Azure Portal에 있는 **기본 SAML 구성** 섹션의 **식별자(엔터티 ID)** 텍스트 상자에 붙여넣습니다.
    
    b. **Azure 페더레이션 메타데이터 URL** 텍스트 상자에 Azure Portal에서 복사한 앱 페더레이션 메타데이터 URL 값을 붙여넣은 다음, **저장**을 클릭합니다.

5. 설정을 완료하려면 **Microsoft로 인증하여 설정 완료**를 클릭하고 Microsoft 계정을 사용하여 인증합니다.

    ![ClickUp Productivity 구성](./media/clickup-productivity-platform-tutorial/configure4.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기

이 섹션의 목적은 Azure Portal에서 Britta Simon이라는 테스트 사용자를 만드는 것입니다.

1. Azure Portal의 왼쪽 창에서 **Azure Active Directory**, **사용자**를 차례로 선택하고 **모든 사용자**를 선택합니다.

    !["사용자 및 그룹" 및 "모든 사용자" 링크](common/users.png)

2. 화면 위쪽에서 **새 사용자**를 선택합니다.

    ![새 사용자 단추](common/new-user.png)

3. 사용자 속성에서 다음 단계를 수행합니다.

    ![사용자 대화 상자](common/user-properties.png)

    a. **이름** 필드에 **BrittaSimon**을 입력합니다.
  
    b. **사용자 이름** 필드에 **brittasimon\@yourcompanydomain.extension**을 입력합니다.  
    예를 들어 BrittaSimon@contoso.com

    c. **암호 표시** 확인란을 선택한 다음, [암호] 상자에 표시된 값을 적어둡니다.

    d. **만들기**를 클릭합니다.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 Britta Simon에게 ClickUp Productivity 플랫폼에 대한 액세스 권한을 부여합니다.

1. Azure Portal에서 **엔터프라이즈 애플리케이션**을 선택하고 **모든 애플리케이션**을 선택한 다음, **ClickUp Productivity 플랫폼**을 선택합니다.

    ![엔터프라이즈 애플리케이션 블레이드](common/enterprise-applications.png)

2. 애플리케이션 목록에서 **ClickUp Productivity 플랫폼**을 선택합니다.

    ![애플리케이션 목록에서 ClickUp Productivity 플랫폼 링크](common/all-applications.png)

3. 왼쪽 메뉴에서 **사용자 및 그룹**을 선택합니다.

    !["사용자 및 그룹" 링크](common/users-groups-blade.png)

4. **사용자 추가** 단추를 클릭한 다음, **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![할당 추가 창](common/add-assign-user.png)

5. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택하고 화면 아래쪽에서 **선택** 단추를 클릭합니다.

6. SAML 어설션 및 **역할 선택** 대화 상자에서 모든 역할 값이 필요한 경우 목록에서 적절한 사용자 역할을 선택한 다음, 화면 맨 아래에 있는 **선택** 단추를 클릭합니다.

7. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.

### <a name="create-clickup-productivity-platform-test-user"></a>ClickUp Productivity 플랫폼 테스트 사용자 만들기

1. 다른 웹 브라우저 창에서 ClickUp Productivity 플랫폼 테넌트에 관리자로 로그인합니다.

2. **사용자 프로필**을 클릭하고 **사용자**를 선택합니다.

    ![ClickUp Productivity 구성](./media/clickup-productivity-platform-tutorial/user1.png)

3. 텍스트 상자에 사용자의 이메일 주소를 입력하고 **초대**를 클릭합니다.

    ![ClickUp Productivity 구성](./media/clickup-productivity-platform-tutorial/user2.png)

    > [!NOTE]
    > 사용자에게 알림이 전송되고 계정을 활성화하려면 초대를 수락해야 합니다.

### <a name="test-single-sign-on"></a>Single Sign-On 테스트

이 섹션에서는 액세스 패널을 사용하여 Azure AD Single Sign-On 구성을 테스트합니다.

액세스 패널에서 ClickUp Productivity 플랫폼 타일을 클릭하면 SSO를 설정한 ClickUp Productivity 플랫폼에 자동으로 로그인됩니다. 액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)를 참조하세요.

## <a name="additional-resources"></a>추가 리소스

- [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory로 애플리케이션 액세스 및 Single Sign-On을 구현하는 방법](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory의 조건부 액세스란?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

