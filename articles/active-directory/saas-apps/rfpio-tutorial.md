---
title: '자습서: RFPIO와 Azure Active Directory 통합 | Microsoft Docs'
description: Azure Active Directory와 RFPIO 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 87187076-7b50-4247-814f-f217b052703f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 63d7b6af8ff76c890b98c29ded0e8bdc637b45dd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67092855"
---
# <a name="tutorial-azure-active-directory-integration-with-rfpio"></a>자습서: RFPIO와 Azure Active Directory 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 RFPIO를 통합하는 방법에 대해 알아봅니다.
RFPIO를 Azure AD와 통합하면 다음과 같은 이점이 제공됩니다.

* RFPIO에 대한 액세스 권한이 있는 사용자를 Azure AD에서 제어할 수 있습니다.
* 사용자가 자신의 Azure AD 계정으로 RFPIO에 자동으로 로그온(Single Sign-On)되도록 설정할 수 있습니다.
* 단일 중앙 위치인 Azure Portal에서 계정을 관리할 수 있습니다.

Azure AD와의 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory를 사용한 응용 프로그램 액세스 및 Single Sign-On](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)을 참조하세요.
Azure 구독이 아직 없는 경우 시작하기 전에 [체험](https://azure.microsoft.com/free/) 계정을 만듭니다.

## <a name="prerequisites"></a>필수 조건

RFPIO와 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

* Azure AD 구독 Azure AD 환경이 없으면 [체험 계정](https://azure.microsoft.com/free/)을 얻을 수 있습니다.
* RFPIO Single Sign-On이 설정된 구독

## <a name="scenario-description"></a>시나리오 설명

이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

* RFPIO는 **SP 및 IDP** 시작 SSO를 지원합니다.

## <a name="adding-rfpio-from-the-gallery"></a>갤러리에서 RFPIO 추가

RFPIO의 Azure AD 통합을 구성하려면 갤러리의 RFPIO를 관리되는 SaaS 앱 목록에 추가해야 합니다.

**갤러리에서 RFPIO를 추가하려면 다음 단계를 수행합니다.**

1. **[Azure Portal](https://portal.azure.com)** 의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다.

    ![Azure Active Directory 단추](common/select-azuread.png)

2. **엔터프라이즈 응용 프로그램**으로 이동한 다음, **모든 응용 프로그램** 옵션을 선택합니다.

    ![엔터프라이즈 애플리케이션 블레이드](common/enterprise-applications.png)

3. 새 애플리케이션을 추가하려면 대화 상자 맨 위 있는 **새 애플리케이션** 단추를 클릭합니다.

    ![새 애플리케이션 단추](common/add-new-app.png)

4. 검색 상자에 **RFPIO**를 입력하고 결과 패널에서 **RFPIO**를 선택한 다음, **추가** 단추를 클릭하여 애플리케이션을 추가합니다.

    ![결과 목록의 RFPIO](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성 및 테스트

이 섹션에서는 **Britta Simon**이라는 테스트 사용자를 기반으로 RFPIO에서 Azure AD Single Sign-On을 구성하고 테스트합니다.
Single Sign-On이 작동하려면 Azure AD 사용자와 RFPIO의 관련 사용자 간에 연결 관계를 설정해야 합니다.

RFPIO에서 Azure AD Single Sign-On을 구성하고 테스트하려면 다음 구성 요소를 완료해야 합니다.

1. **[Azure AD Single Sign-On 구성](#configure-azure-ad-single-sign-on)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
2. **[RFPIO Single Sign-On 구성](#configure-rfpio-single-sign-on)** - 애플리케이션 쪽에서 Single Sign-On 설정을 구성합니다.
3. **[Azure AD 테스트 사용자 만들기](#create-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On을 테스트하는 데 사용합니다.
4. **[Azure AD 테스트 사용자 할당](#assign-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
5. **[RFPIO 테스트 사용자 만들기](#create-rfpio-test-user)** - Britta Simon의 Azure AD 표현과 연결된 해당 사용자를 RFPIO에 만듭니다.
6. **[Single Sign-On 테스트](#test-single-sign-on)** - 구성이 작동하는지 여부를 확인합니다.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure Portal에서 Azure AD Single Sign-On을 사용하도록 설정합니다.

RFPIO에서 Azure AD Single Sign-On을 구성하려면 다음 단계를 수행합니다.

1. [Azure Portal](https://portal.azure.com/)의 **RFPIO** 애플리케이션 통합 페이지에서 **Single Sign-On**을 선택합니다.

    ![Single Sign-On 구성 링크](common/select-sso.png)

2. **Single Sign-On 방법 선택** 대화 상자에서 **SAML/WS-Fed** 모드를 선택하여 Single Sign-On을 사용하도록 설정합니다.

    ![Single Sign-On 선택 모드](common/select-saml-option.png)

3. **SAML로 Single Sign-On 설정** 페이지에서 **편집** 아이콘을 클릭하여 **기본 SAML 구성** 대화 상자를 엽니다.

    ![기본 SAML 구성 편집](common/edit-urls.png)

4. **IDP** 시작 모드에서 애플리케이션을 구성하려면 **기본 SAML 구성** 섹션에서 다음 단계를 수행합니다.

    ![RFPIO 도메인 및 URL Single Sign-On 정보](common/idp-identifier.png)

    a. **식별자** 텍스트 상자에서 `https://www.rfpio.com` 패턴을 사용하여 URL을 입력합니다.

    b. **추가 URL 설정**을 클릭합니다.

    다. **릴레이 상태** 텍스트 상자에 문자열 값을 입력합니다. 이 값을 얻으려면 [RFPIO 지원 팀](https://www.rfpio.com/contact/)에 문의하세요.

    ![RFPIO 도메인 및 URL Single Sign-On 정보](common/idp-preintegrated-relay.png)

5. **SP** 시작 모드에서 애플리케이션을 구성하려면 **추가 URL 설정**을 클릭하고 다음 단계를 수행합니다.

    ![이미지](common/both-preintegrated-signon.png)

    **로그인 URL** 텍스트 상자에서 `https://www.app.rfpio.com` 패턴을 사용하여 URL을 입력합니다.

    > [!NOTE]
    > 이러한 값은 실제 값이 아닙니다. 실제 식별자 및 로그온 URL로 해당 값을 업데이트합니다. 이러한 값을 얻으려면 [RFPIO 클라이언트 지원 팀](https://www.rfpio.com/contact/)에 문의하세요. Azure Portal의 **기본 SAML 구성** 섹션에 표시된 패턴을 참조할 수도 있습니다.

6. **SAML로 Single Sign-On 설정** 페이지의 **SAML 서명 인증서** 섹션에서 **다운로드**를 클릭하여 요구 사항에 따라 제공된 옵션에서 **페더레이션 메타데이터 XML**을 다운로드하고 컴퓨터에 저장합니다.

    ![인증서 다운로드 링크](common/metadataxml.png)

7. **RFPIO 설정** 섹션에서 요구 사항에 따라 적절한 URL을 복사합니다.

    ![구성 URL 복사](common/copy-configuration-urls.png)

    a. 로그인 URL

    b. Azure AD 식별자

    다. 로그아웃 URL

### <a name="configure-rfpio-single-sign-on"></a>RFPIO Single Sign-On 구성

1. 다른 웹 브라우저 창에서 **RFPIO** 웹 사이트에 관리자로 로그인합니다.

1. 왼쪽 아래 모서리 드롭다운을 클릭합니다.

    ![Configure Single Sign-On](./media/rfpio-tutorial/app1.png)

1. **조직 설정**을 클릭합니다. 

    ![Configure Single Sign-On](./media/rfpio-tutorial/app2.png)

1. **기능 및 통합**을 클릭합니다.

    ![Configure Single Sign-On](./media/rfpio-tutorial/app4.png)

1. **SAML SSO 구성**에서 **편집**을 클릭합니다.

    ![Configure Single Sign-On](./media/rfpio-tutorial/app3.png)

1. 이 섹션에서 다음 작업을 수행합니다.

    ![Configure Single Sign-On](./media/rfpio-tutorial/app5.png)
    
    a. **다운로드한 메타데이터 XML**의 내용을 복사하여 **ID 구성** 필드에 붙여넣습니다.

    > [!NOTE]
    >다운로드한 **페더레이션 메타데이터 XML**의 콘텐츠를 복사하려면 **메모장++** 또는 적절한 **XML 편집기**를 사용합니다.

    b. **유효성 검사**를 클릭합니다.

    다. **유효성 검사**를 클릭한 후 **SAML(Enabled)** (SAML(사용))을 설정으로 전환합니다.

    d. **제출**을 클릭합니다.

### <a name="create-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기

이 섹션의 목적은 Azure Portal에서 Britta Simon이라는 테스트 사용자를 만드는 것입니다.

1. Azure Portal의 왼쪽 창에서 **Azure Active Directory**, **사용자**를 차례로 선택하고 **모든 사용자**를 선택합니다.

    !["사용자 및 그룹" 및 "모든 사용자" 링크](common/users.png)

2. 화면 위쪽에서 **새 사용자**를 선택합니다.

    ![새 사용자 단추](common/new-user.png)

3. 사용자 속성에서 다음 단계를 수행합니다.

    ![사용자 대화 상자](common/user-properties.png)

    a. **이름** 필드에 **BrittaSimon**을 입력합니다.
  
    b. **사용자 이름** 필드에 `brittasimon@yourcompanydomain.extension`을 입력합니다. 예를 들어 BrittaSimon@contoso.com

    c. **암호 표시** 확인란을 선택한 다음, [암호] 상자에 표시된 값을 적어둡니다.

    d. **만들기**를 클릭합니다.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 Britta Simon에게 RFPIO에 대한 액세스 권한을 부여합니다.

1. Azure Portal에서 **엔터프라이즈 애플리케이션**, **모든 애플리케이션**, **RFPIO**를 차례로 선택합니다.

    ![엔터프라이즈 애플리케이션 블레이드](common/enterprise-applications.png)

2. 애플리케이션 목록에서 **RFPIO**를 선택합니다.

    ![애플리케이션 목록의 RFPIO 링크](common/all-applications.png)

3. 왼쪽 메뉴에서 **사용자 및 그룹**을 선택합니다.

    !["사용자 및 그룹" 링크](common/users-groups-blade.png)

4. **사용자 추가** 단추를 클릭한 다음, **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![할당 추가 창](common/add-assign-user.png)

5. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택하고 화면 아래쪽에서 **선택** 단추를 클릭합니다.

6. SAML 어설션 및 **역할 선택** 대화 상자에서 모든 역할 값이 필요한 경우 목록에서 적절한 사용자 역할을 선택한 다음, 화면 맨 아래에 있는 **선택** 단추를 클릭합니다.

7. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.

### <a name="create-rfpio-test-user"></a>RFPIO 테스트 사용자 만들기

1. RFPIO 회사 사이트에 관리자로 로그인합니다.

1. 왼쪽 아래 모서리 드롭다운을 클릭합니다.

    ![Configure Single Sign-On](./media/rfpio-tutorial/app1.png)

1. **조직 설정**을 클릭합니다. 

    ![Configure Single Sign-On](./media/rfpio-tutorial/app2.png)

1. **TEAM MEMBERS**(팀 멤버)를 클릭합니다.

    ![Configure Single Sign-On](./media/rfpio-tutorial/app6.png)

1. **멤버 추가**를 클릭합니다.

    ![Configure Single Sign-On](./media/rfpio-tutorial/app7.png)

1. **새 멤버 추가** 섹션에서 다음 작업을 수행합니다.

    ![Configure Single Sign-On](./media/rfpio-tutorial/app8.png)

    a. **Enter one email per line**(줄당 하나의 메일 입력) 필드에 **메일 주소**를 입력합니다.

    b. 요구 사항에 따라 **역할**을 선택합니다.

    다. **멤버 추가**를 클릭합니다.

    > [!NOTE]
    > Azure Active Directory 계정 보유자는 활성화되기 전에 메일을 받고 링크를 따라 계정을 확인합니다.

### <a name="test-single-sign-on"></a>Single Sign-On 테스트

이 섹션에서는 액세스 패널을 사용하여 Azure AD Single Sign-On 구성을 테스트합니다.

액세스 패널에서 RFPIO 타일을 클릭하면 SSO를 설정한 RFPIO에 자동으로 로그인됩니다. 액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)를 참조하세요.

## <a name="additional-resources"></a>추가 리소스

- [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory로 애플리케이션 액세스 및 Single Sign-On을 구현하는 방법](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory의 조건부 액세스란?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

