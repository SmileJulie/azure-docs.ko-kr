---
title: '자습서: Tableau Server와 Azure Active Directory 통합 | Microsoft Docs'
description: Azure Active Directory와 Tableau Server 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/22/2019
ms.author: jeedes
ms.openlocfilehash: ed8e63a49702e8ba951990f8d7ff19b8f058fa74
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67089399"
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>자습서: Tableau Server와 Azure Active Directory 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 Tableau Server를 통합하는 방법에 대해 알아봅니다.
Tableau Server를 Azure AD와 통합하면 다음과 같은 이점이 제공됩니다.

* Tableau Server에 대한 액세스 권한이 있는 사용자를 Azure AD에서 제어할 수 있습니다.
* 사용자가 자신의 Azure AD 계정으로 Tableau Server에 자동으로 로그인(Single Sign-On)되도록 설정할 수 있습니다.
* 단일 중앙 위치인 Azure Portal에서 계정을 관리할 수 있습니다.

Azure AD와의 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory를 사용한 응용 프로그램 액세스 및 Single Sign-On](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)을 참조하세요.
Azure 구독이 아직 없는 경우 시작하기 전에 [체험](https://azure.microsoft.com/free/) 계정을 만듭니다.

## <a name="prerequisites"></a>필수 조건

Tableau Server와 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

* Azure AD 구독 Azure AD 환경이 없으면 [체험 계정](https://azure.microsoft.com/free/)을 얻을 수 있습니다.
* Tableau Server Single Sign-On을 사용하도록 설정된 구독

## <a name="scenario-description"></a>시나리오 설명

이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

* Tableau Server는 **SP** 시작 SSO를 지원합니다.

## <a name="adding-tableau-server-from-the-gallery"></a>갤러리에서 Tableau Server 추가

Tableau Server의 Azure AD 통합을 구성하려면 갤러리의 Tableau Server를 관리되는 SaaS 앱 목록에 추가해야 합니다.

**갤러리에서 Tableau Server를 추가하려면 다음 단계를 수행합니다.**

1. **[Azure Portal](https://portal.azure.com)** 의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다.

    ![Azure Active Directory 단추](common/select-azuread.png)

2. **엔터프라이즈 응용 프로그램**으로 이동한 다음, **모든 응용 프로그램** 옵션을 선택합니다.

    ![엔터프라이즈 애플리케이션 블레이드](common/enterprise-applications.png)

3. 새 애플리케이션을 추가하려면 대화 상자 맨 위 있는 **새 애플리케이션** 단추를 클릭합니다.

    ![새 애플리케이션 단추](common/add-new-app.png)

4. 검색 상자에 **Tableau Server**를 입력하고, 결과 패널에서 **Tableau Server**를 선택한 다음, **추가** 단추를 클릭하여 애플리케이션을 추가합니다.

    ![결과 목록의 Tableau Server](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성 및 테스트

이 섹션에서는 **Britta Simon**이라는 테스트 사용자를 기반으로 Tableau Server에서 Azure AD Single Sign-On을 구성하고 테스트합니다.
Single Sign-On이 작동하려면 Azure AD 사용자와 Tableau Server의 관련 사용자 간에 연결 관계가 설정되어 있어야 합니다.

Tableau Server에서 Azure AD Single Sign-On을 구성하고 테스트하려면 다음 구성 요소를 완료해야 합니다.

1. **[Azure AD Single Sign-On 구성](#configure-azure-ad-single-sign-on)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
2. **[Tableau Server Single Sign-On 구성](#configure-tableau-server-single-sign-on)** - 애플리케이션 쪽에서 Single Sign-On 설정을 구성합니다.
3. **[Azure AD 테스트 사용자 만들기](#create-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On을 테스트하는 데 사용합니다.
4. **[Azure AD 테스트 사용자 할당](#assign-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
5. **[Tableau Server 테스트 사용자 만들기](#create-tableau-server-test-user)** - Britta Simon의 Azure AD 표현과 연결된 해당 사용자를 Tableau Server에 만듭니다.
6. **[Single Sign-On 테스트](#test-single-sign-on)** - 구성이 작동하는지 여부를 확인합니다.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure Portal에서 Azure AD Single Sign-On을 사용하도록 설정합니다.

Tableau Server에서 Azure AD Single Sign-On을 구성하려면 다음 단계를 수행합니다.

1. [Azure Portal](https://portal.azure.com/)의 **Tableau Server** 애플리케이션 통합 페이지에서 **Single Sign-On**을 선택합니다.

    ![Single Sign-On 구성 링크](common/select-sso.png)

2. **Single Sign-On 방법 선택** 대화 상자에서 **SAML/WS-Fed** 모드를 선택하여 Single Sign-On을 사용하도록 설정합니다.

    ![Single Sign-On 선택 모드](common/select-saml-option.png)

3. **SAML로 Single Sign-On 설정** 페이지에서 **편집** 아이콘을 클릭하여 **기본 SAML 구성** 대화 상자를 엽니다.

    ![기본 SAML 구성 편집](common/edit-urls.png)

4. **기본 SAML 구성** 섹션에서 다음 단계를 수행합니다.

    ![Tableau Server 도메인 및 URL Single Sign-On 정보](common/sp-identifier-reply.png)

    a. **로그인 URL** 텍스트 상자에서 `https://azure.<domain name>.link` 패턴을 사용하여 URL을 입력합니다.

    b. **식별자** 텍스트 상자에서 `https://azure.<domain name>.link` 패턴을 사용하는 URL을 입력합니다.

    다. **회신 URL** 텍스트 상자에서 `https://azure.<domain name>.link/wg/saml/SSO/index.html` 패턴을 사용하여 URL을 입력합니다.

    > [!NOTE]
    > 위의 값은 실제 값이 아닙니다. 이 자습서에서 나중에 설명하겠지만, 이 값을 Tableau Server 구성 페이지의 실제 URL과 식별자로 업데이트합니다.

5. Tableau Server 애플리케이션에서는 사용자 지정 클레임 **username**을 아래와 같이 정의해야 합니다. 이것이 고유한 사용자 ID 클레임 대신 사용자 ID로 사용되고 있습니다. 애플리케이션 통합 페이지의 **사용자 특성 및 클레임** 섹션에서 이러한 특성의 값을 관리할 수 있습니다. **편집** 단추를 클릭하여 **사용자 특성 및 클레임** 대화 상자를 엽니다.

    ![이미지](common/edit-attribute.png)

6. **사용자 특성 및 클레임** 대화 상자의 **사용자 클레임** 섹션에서 위의 이미지에 표시된 것과 같이 SAML 토큰 특성을 구성하고 다음 단계를 수행합니다.

    | 이름 | 원본 특성 | 네임스페이스 |
    | ---------------| --------------- | ----------- |
    | 사용자 이름 | user.userprincipalname | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |
    | | |

    a. **새 클레임 추가**를 클릭하여 **사용자 클레임 관리** 대화 상자를 엽니다.

    ![이미지](common/new-save-attribute.png)

    ![이미지](common/new-attribute-details.png)

    b. **이름** 텍스트 상자에서 해당 행에 표시된 특성 이름을 입력합니다.

    다. **네임스페이스** 값을 입력합니다.

    d. 원본을 **특성**으로 선택합니다.

    e. **원본 특성** 목록에서 해당 행에 표시된 특성 값을 입력합니다.

    f. **확인**을 클릭합니다.

    g. **저장**을 클릭합니다.

7. **SAML로 Single Sign-On 설정** 페이지의 **SAML 서명 인증서** 섹션에서 **다운로드**를 클릭하여 요구 사항에 따라 제공된 옵션에서 **페더레이션 메타데이터 XML**을 다운로드하고 컴퓨터에 저장합니다.

    ![인증서 다운로드 링크](common/metadataxml.png)

8. **Tableau Server 설정** 섹션에서 요구 사항에 따라 적절한 URL을 복사합니다.

    ![구성 URL 복사](common/copy-configuration-urls.png)

    a. 로그인 URL

    b. Azure AD 식별자

    다. 로그아웃 URL

### <a name="configure-tableau-server-single-sign-on"></a>Tableau Server Single Sign-On 구성

1. 애플리케이션에 구성된 SSO를 가져오려면 Tableau Server 테넌트에 관리자로 로그인해야 합니다.

2. **구성** 탭에서 **사용자 ID 및 액세스**를 선택한 다음, **인증** 방법 탭을 선택합니다.

    ![Configure Single Sign-On](./media/tableauserver-tutorial/tutorial-tableauserver-auth.png)

3. **구성** 페이지에서 다음 단계를 수행합니다.

    ![Configure Single Sign-On](./media/tableauserver-tutorial/tutorial-tableauserver-config.png)

    a. **인증 방법**은 SAML을 선택합니다.

    b. **서버 SAML 인증 가능** 확인란을 선택합니다.

    다. Tableau Server 반환 URL - Tableau Server 사용자가 액세스하는 URL(예: <http://tableau_server>)입니다. `http://localhost`을 사용하는 것은 권장되지 않습니다. 후행 슬래시가 있는 URL(예: `http://tableau_server/`)은 지원되지 않습니다. **Tableau Server 반환 URL**을 복사하고 Azure Portal의 **기본 SAML 구성** 섹션에 있는 **로그온 URL** 텍스트 상자에 붙여넣습니다.

    d. SAML 엔터티 ID - 엔터티 ID는 IdP에 대한 Tableau Server 설치를 고유하게 식별합니다. 원하는 경우 여기에 Tableau Server URL을 다시 입력할 수 있지만 반드시 Tableau Server URL을 입력해야 하는 것은 아닙니다. **SAML 엔터티 ID**를 복사하고 Azure Portal의 **기본 SAML 구성** 섹션에 있는 **식별자** 텍스트 상자에 붙여넣습니다.

    e. **XML 메타데이터 파일 다운로드**를 클릭하고 텍스트 편집기 애플리케이션에서 엽니다. Http Post 및 인덱스 0이 포함된 어설션 소비자 서비스 URL을 찾아서 복사합니다. 이제 Azure Portal의 **기본 SAML 구성** 섹션에 있는 **회신 URL** 텍스트 상자에 붙여넣습니다.

    f. Azure Portal에서 다운로드한 페더레이션 메타데이터 파일을 찾은 다음 **SAML Idp 메타데이터 파일**에 업로드합니다.

    g. IdP가 사용자 이름, 표시 이름, 이메일 주소를 확보하는 데 사용하는 특성의 이름을 입력합니다.

    h. 페이지 맨 아래에 있는 **저장**

    > [!NOTE]
    > 고객은 Tableau Server SAML SSO 구성의 모든 인증서를 업로드해야 하며, 인증서는 SSO 흐름에서 무시됩니다. Tableau Server에서 SAML을 구성하는 데 도움이 필요한 경우 [SAML 구성](https://onlinehelp.tableau.com/v2018.2/server/en-us/saml_config_steps_tsm_ui.htm) 문서를 참조하세요.

### <a name="create-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기

이 섹션의 목적은 Azure Portal에서 Britta Simon이라는 테스트 사용자를 만드는 것입니다.

1. Azure Portal의 왼쪽 창에서 **Azure Active Directory**, **사용자**를 차례로 선택하고 **모든 사용자**를 선택합니다.

    !["사용자 및 그룹" 및 "모든 사용자" 링크](common/users.png)

2. 화면 위쪽에서 **새 사용자**를 선택합니다.

    ![새 사용자 단추](common/new-user.png)

3. 사용자 속성에서 다음 단계를 수행합니다.

    ![사용자 대화 상자](common/user-properties.png)

    a. **이름** 필드에 **BrittaSimon**을 입력합니다.
  
    b. **사용자 이름** 필드에 `brittasimon@yourcompanydomain.extension`을 입력합니다.  
    예를 들어 BrittaSimon@contoso.com

    c. **암호 표시** 확인란을 선택한 다음, [암호] 상자에 표시된 값을 적어둡니다.

    d. **만들기**를 클릭합니다.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 Britta Simon에게 Tableau Server에 대한 액세스 권한을 부여합니다.

1. Azure Portal에서 **엔터프라이즈 애플리케이션**, **모든 애플리케이션**, **Tableau Server**를 차례로 선택합니다.

    ![엔터프라이즈 애플리케이션 블레이드](common/enterprise-applications.png)

2. 애플리케이션 목록에서 **Tableau Server**를 선택합니다.

    ![애플리케이션 목록의 Tableau Server 링크](common/all-applications.png)

3. 왼쪽 메뉴에서 **사용자 및 그룹**을 선택합니다.

    !["사용자 및 그룹" 링크](common/users-groups-blade.png)

4. **사용자 추가** 단추를 클릭한 다음, **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![할당 추가 창](common/add-assign-user.png)

5. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택하고 화면 아래쪽에서 **선택** 단추를 클릭합니다.

6. SAML 어설션 및 **역할 선택** 대화 상자에서 모든 역할 값이 필요한 경우 목록에서 적절한 사용자 역할을 선택한 다음, 화면 맨 아래에 있는 **선택** 단추를 클릭합니다.

7. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.

### <a name="create-tableau-server-test-user"></a>Tableau Server 테스트 사용자 만들기

이 섹션은 Tableau Server에서 Britta Simon이라는 사용자를 만들기 위한 것입니다. Tableau Server의 모든 사용자를 프로비전해야 합니다.

사용자의 사용자 이름은 Azure AD에서 구성한 사용자 지정 특성 **username**의 값과 일치해야 합니다. 올바른 매핑을 사용해야 통합 시 Azure AD Single Sign-On 구성이 작동합니다.

> [!NOTE]
> 사용자를 수동으로 만들어야 하는 경우 조직의 Tableau Server 관리자에게 문의해야 합니다.

### <a name="test-single-sign-on"></a>Single Sign-On 테스트

이 섹션에서는 액세스 패널을 사용하여 Azure AD Single Sign-On 구성을 테스트합니다.

액세스 패널에서 Tableau Server 타일을 클릭하면 SSO가 설정된 Tableau Server에 자동으로 로그인됩니다. 액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)를 참조하세요.

## <a name="additional-resources"></a>추가 리소스

- [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory로 애플리케이션 액세스 및 Single Sign-On을 구현하는 방법](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory의 조건부 액세스란?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

