---
title: '자습서: Azure Active Directory와 Zoho One 통합 | Microsoft Docs'
description: Azure Active Directory와 Zoho One 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: bbc3038c-0d8b-45dd-9645-368bd3d01a0f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.openlocfilehash: 0a37789e7c7efeb71770ff0e8061d57e6603b6c4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67086242"
---
# <a name="tutorial-azure-active-directory-integration-with-zoho-one"></a>자습서: Zoho One과 Azure Active Directory 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 Zoho One을 통합하는 방법에 대해 알아봅니다.
Zoho One을 Azure AD와 통합하면 다음과 같은 이점이 제공됩니다.

* Zoho One에 대한 액세스 권한이 있는 사용자를 Azure AD에서 제어할 수 있습니다.
* 사용자가 자신의 Azure AD 계정으로 Zoho One에 자동으로 로그인(Single Sign-On)되도록 설정할 수 있습니다.
* 단일 중앙 위치인 Azure Portal에서 계정을 관리할 수 있습니다.

Azure AD와의 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory를 사용한 응용 프로그램 액세스 및 Single Sign-On](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)을 참조하세요.
Azure 구독이 아직 없는 경우 시작하기 전에 [체험](https://azure.microsoft.com/free/) 계정을 만듭니다.

## <a name="prerequisites"></a>필수 조건

Zoho One과 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

* Azure AD 구독 Azure AD 환경이 없으면 [체험 계정](https://azure.microsoft.com/free/)을 얻을 수 있습니다.
* Zoho One Single Sign-On이 설정된 구독

## <a name="scenario-description"></a>시나리오 설명

이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

* Zoho One은 **SP** 및 **IDP** 시작 SSO를 지원합니다.

## <a name="adding-zoho-one-from-the-gallery"></a>갤러리에서 Zoho One 추가

Zoho One의 Azure AD 통합을 구성하려면 갤러리의 Zoho One을 관리되는 SaaS 앱 목록에 추가해야 합니다.

**갤러리에서 Zoho One을 추가하려면 다음 단계를 수행합니다.**

1. **[Azure Portal](https://portal.azure.com)** 의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다.

    ![Azure Active Directory 단추](common/select-azuread.png)

2. **엔터프라이즈 응용 프로그램**으로 이동한 다음, **모든 응용 프로그램** 옵션을 선택합니다.

    ![엔터프라이즈 애플리케이션 블레이드](common/enterprise-applications.png)

3. 새 애플리케이션을 추가하려면 대화 상자 맨 위 있는 **새 애플리케이션** 단추를 클릭합니다.

    ![새 애플리케이션 단추](common/add-new-app.png)

4. 검색 상자에 **Zoho One**을 입력하고 결과 패널에서 **Zoho One**을 선택한 다음, **추가** 단추를 클릭하여 애플리케이션을 추가합니다.

     ![결과 목록의 Zoho One](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성 및 테스트

이 섹션에서는 **Britta Simon**이라는 테스트 사용자를 기반으로 Zoho One에서 Azure AD Single Sign-On을 구성하고 테스트합니다.
Single Sign-On이 작동하려면 Azure AD 사용자와 Zoho One의 관련 사용자 간에 연결 관계를 설정해야 합니다.

Zoho One에서 Azure AD Single Sign-On을 구성하고 테스트하려면 다음 구성 요소를 완료해야 합니다.

1. **[Azure AD Single Sign-On 구성](#configure-azure-ad-single-sign-on)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
2. **[Zoho One Single Sign-On 구성](#configure-zoho-one-single-sign-on)** - 애플리케이션 쪽에서 Single Sign-On 설정을 구성합니다.
3. **[Azure AD 테스트 사용자 만들기](#create-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On을 테스트하는 데 사용합니다.
4. **[Azure AD 테스트 사용자 할당](#assign-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
5. **[Zoho One 테스트 사용자 만들기](#create-zoho-one-test-user)** - Britta Simon의 Azure AD 표현과 연결된 해당 사용자를 Zoho One에 만듭니다.
6. **[Single Sign-On 테스트](#test-single-sign-on)** - 구성이 작동하는지 여부를 확인합니다.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure Portal에서 Azure AD Single Sign-On을 사용하도록 설정합니다.

Zoho One에서 Azure AD Single Sign-On을 구성하려면 다음 단계를 수행합니다.

1. [Azure Portal](https://portal.azure.com/)의 **Zoho One** 애플리케이션 통합 페이지에서 **Single Sign-On**을 선택합니다.

    ![Single Sign-On 구성 링크](common/select-sso.png)

2. **Single Sign-On 방법 선택** 대화 상자에서 **SAML/WS-Fed** 모드를 선택하여 Single Sign-On을 사용하도록 설정합니다.

    ![Single Sign-On 선택 모드](common/select-saml-option.png)

3. **SAML로 Single Sign-On 설정** 페이지에서 **편집** 아이콘을 클릭하여 **기본 SAML 구성** 대화 상자를 엽니다.

    ![기본 SAML 구성 편집](common/edit-urls.png)

4. **IDP** 시작 모드에서 애플리케이션을 구성하려면 **기본 SAML 구성** 섹션에서 다음 단계를 수행합니다.

    ![Zoho One 도메인 및 URL Single Sign-On 정보](common/idp-relay.png)

    a. **식별자** 텍스트 상자에 URL을 입력합니다. `one.zoho.com`

    b. **회신 URL** 텍스트 상자에서 `https://accounts.zoho.com/samlresponse/<saml-identifier>` 패턴을 사용하여 URL을 입력합니다.

    > [!NOTE]
    > 위의 **회신 URL** 값은 실제 값이 아닙니다. 자습서의 뒷부분에서 설명하는 **Zoho One Single Sign-On 구성** 섹션의 4단계에서 `<saml-identifier>` 값을 가져올 것입니다.

    다. **추가 URL 설정**을 클릭합니다.

    d. **릴레이 상태** 텍스트 상자에 URL을 입력합니다. `https://one.zoho.com`

5. **SP** 시작 모드로 애플리케이션을 구성하려는 경우 다음 단계를 수행합니다.


    ![Zoho One 도메인 및 URL Single Sign-On 정보](common/both-signonurl.png)

    **로그인 URL** 텍스트 상자에서 `https://accounts.zoho.com/samlauthrequest/<domain_name>?serviceurl=https://one.zoho.com` 패턴을 사용하여 URL을 입력합니다. 

    > [!NOTE] 
    > 위의 **로그온 URL** 값은 실제 값이 아닙니다. 이 값을 자습서의 뒷부분에서 설명하는 **Zoho One Single Sign-On 구성** 섹션의 실제 로그온 URL로 업데이트할 것입니다. 

6. **SAML로 Single Sign-On 설정** 페이지의 **SAML 서명 인증서** 섹션에서 **다운로드**를 클릭하여 요구 사항에 따라 제공된 옵션에서 **인증서(Base64)** 를 다운로드한 다음, 컴퓨터에 저장합니다.

    ![인증서 다운로드 링크](common/certificatebase64.png)

7. **Zoho One 설정** 섹션에서 요구 사항에 따라 적절한 URL을 복사합니다.

    ![구성 URL 복사](common/copy-configuration-urls.png)

    a. 로그인 URL

    b. Azure AD 식별자

    다. 로그아웃 URL

### <a name="configure-zoho-one-single-sign-on"></a>Zoho One Single Sign-On 구성

1. 다른 웹 브라우저 창에서 관리자 권한으로 Zoho One 회사 사이트에 로그인 합니다.

2. **조직** 탭의 **SAML 인증**에서 **설정**을 클릭합니다.

    ![Zoho One org](./media/zohoone-tutorial/tutorial_zohoone_setup.png)

3. 팝업 페이지에서 다음 단계를 수행합니다.

    ![Zoho One sig](./media/zohoone-tutorial/tutorial_zohoone_save.png)

    a. **로그인 URL** 텍스트 상자에 Azure Portal에서 복사한 **로그인 URL** 값을 붙여넣습니다.

    b. **로그아웃 URL** 텍스트 상자에 Azure Portal에서 복사한 **로그아웃 URL** 값을 붙여넣습니다.

    다. **찾아보기**를 클릭하여 Azure Portal에서 다운로드한 **인증서(Base64)** 를 업로드합니다.

    d. **저장**을 클릭합니다.

4. SAML 인증 설정을 저장한 후에는 **SAML 식별자** 값을 복사하고 `https://accounts.zoho.com/samlresponse/one.zoho.com`처럼 `<saml-identifier>` 자리에 **회신 URL**을 추가한 다음, 생성된 값을 **기본 SAML 구성을** 섹션의 **회신 URL** 텍스트 상자에 붙여넣습니다.

    ![Zoho One saml](./media/zohoone-tutorial/tutorial_zohoone_samlidenti.png)

5. **도메인** 탭으로 이동한 다음, **도메인 추가**를 클릭합니다.

    ![Zoho One 도메인](./media/zohoone-tutorial/tutorial_zohoone_domain.png)

6. **도메인 추가** 페이지에서 다음 단계를 수행합니다.

    ![Zoho One 도메인 추가](./media/zohoone-tutorial/tutorial_zohoone_adddomain.png)

    a. **도메인 이름** 텍스트 상자에 contoso.com 같은 도메인을 입력합니다.

    b. **추가**를 클릭합니다.

    >[!Note]
    >도메인을 추가한 후 [이](https://www.zoho.com/one/help/admin-guide/domain-verification.html) 단계에 따라 도메인을 확인합니다. 도메인이 확인되면 Azure Portal에 있는 **기본 SAML 구성** 섹션의 **로그온 URL**에 해당 도메인 이름을 사용합니다.

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

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 Britta Simon에게 Zoho One에 대한 액세스 권한을 부여합니다.

1. Azure Portal에서 **엔터프라이즈 애플리케이션**, **모든 애플리케이션**, **Zoho One**을 차례로 선택합니다.

    ![엔터프라이즈 애플리케이션 블레이드](common/enterprise-applications.png)

2. 애플리케이션 목록에서 **Zoho One**을 선택합니다.

    ![애플리케이션 목록의 Zoho One 링크](common/all-applications.png)

3. 왼쪽 메뉴에서 **사용자 및 그룹**을 선택합니다.

    !["사용자 및 그룹" 링크](common/users-groups-blade.png)

4. **사용자 추가** 단추를 클릭한 다음, **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![할당 추가 창](common/add-assign-user.png)

5. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택하고 화면 아래쪽에서 **선택** 단추를 클릭합니다.

6. SAML 어설션 및 **역할 선택** 대화 상자에서 모든 역할 값이 필요한 경우 목록에서 적절한 사용자 역할을 선택한 다음, 화면 맨 아래에 있는 **선택** 단추를 클릭합니다.

7. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.

### <a name="create-zoho-one-test-user"></a>Zoho One 테스트 사용자 만들기

Azure AD 사용자가 Zoho One에 로그인하려면 Zoho One에 프로비저닝되어야 합니다. Zoho One의 경우 수동으로 프로비전합니다.

**사용자 계정을 프로비전하려면 다음 단계를 수행합니다.**

1. 보안 관리자 권한으로 Zoho One에 로그인합니다.

2. **사용자** 탭에서 **사용자 로고**를 클릭합니다.

    ![Zoho One 사용자](./media/zohoone-tutorial/tutorial_zohoone_users.png)

3. **사용자 추가** 페이지에서 다음 단계를 수행합니다.

    ![Zoho One 사용자 추가](./media/zohoone-tutorial/tutorial_zohoone_adduser.png)
    
    a. **이름** 텍스트 상자에 사용자 이름(예: **Britta Simon**)을 입력합니다.
    
    b. **이메일 주소** 텍스트 상자에 brittasimon@contoso.com과 같은 사용자의 이메일을 입력합니다.

    >[!Note]
    >도메인 목록에서 확인된 도메인을 선택합니다.

    다. **추가**를 클릭합니다.

### <a name="test-single-sign-on"></a>Single Sign-On 테스트 

이 섹션에서는 액세스 패널을 사용하여 Azure AD Single Sign-On 구성을 테스트합니다.

액세스 패널에서 Zoho One 타일을 클릭하면 SSO를 설정한 Zoho One에 자동으로 로그인되어야 합니다. 액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)를 참조하세요.

## <a name="additional-resources"></a>추가 리소스

- [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory로 애플리케이션 액세스 및 Single Sign-On을 구현하는 방법](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory의 조건부 액세스란?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

