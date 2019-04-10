---
title: '자습서: Percolate와 Azure Active Directory 통합 | Microsoft Docs'
description: Azure Active Directory 및 Percolate 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 355f9659-b378-44c9-aa88-236e9b529a53
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: 166a9daed40c03ed603d9ecaf4db2d49da0bf93d
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58887062"
---
# <a name="tutorial-azure-active-directory-integration-with-percolate"></a>자습서: Percolate와 Azure Active Directory 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 Percolate를 통합하는 방법을 알아봅니다.
Percolate를 Azure AD와 통합하면 다음과 같은 이점이 있습니다.

* Azure AD에서는 Percolate에 대한 액세스 권한이 있는 사용자를 제어할 수 있습니다.
* 사용자가 자신의 Azure AD 계정을 사용하여 Percolate(Single Sign-On)에 자동으로 로그인되도록 설정할 수 있습니다.
* 단일 중앙 위치인 Azure Portal에서 계정을 관리할 수 있습니다.

Azure AD와의 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory를 사용한 응용 프로그램 액세스 및 Single Sign-On](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)을 참조하세요.
Azure 구독이 아직 없는 경우 시작하기 전에 [체험](https://azure.microsoft.com/free/) 계정을 만듭니다.

## <a name="prerequisites"></a>필수 조건

Percolate와의 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

* Azure AD 구독 Azure AD 환경이 없으면 [체험 계정](https://azure.microsoft.com/free/)을 얻을 수 있습니다.
* Percolate Single Sign-On을 사용하도록 설정된 구독

## <a name="scenario-description"></a>시나리오 설명

이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

* Percolate는 **SP** 및 **IDP**에서 시작된 SSO를 지원합니다.

## <a name="adding-percolate-from-the-gallery"></a>갤러리의 Percolate 추가

Percolate의 Azure AD 통합을 구성하려면 갤러리의 Percolate를 관리형 SaaS 앱 목록에 추가해야 합니다.

**갤러리의 Percolate를 추가하려면 다음 단계를 수행합니다.**

1. **[Azure Portal](https://portal.azure.com)** 의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다.

    ![Azure Active Directory 단추](common/select-azuread.png)

2. **엔터프라이즈 응용 프로그램**으로 이동한 다음, **모든 응용 프로그램** 옵션을 선택합니다.

    ![엔터프라이즈 애플리케이션 블레이드](common/enterprise-applications.png)

3. 새 애플리케이션을 추가하려면 대화 상자 맨 위 있는 **새 애플리케이션** 단추를 클릭합니다.

    ![새 애플리케이션 단추](common/add-new-app.png)

4. 검색 상자에 **Percolate**를 입력하고 결과 패널에서 **Percolate**를 선택한 후 **추가** 단추를 클릭하여 애플리케이션을 추가합니다.

     ![결과 목록의 Percolate](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성 및 테스트

이 섹션에서는 **Britta Simon**이라는 테스트 사용자를 기반으로 Percolate에서 Azure AD Single Sign-On을 구성하고 테스트합니다.
Single Sign-On이 작동하려면 Azure AD 사용자와 Percolate의 관련 사용자 간에 연결 관계를 설정해야 합니다.

Percolate에서 Azure AD Single Sign-On을 구성하고 테스트하려면 다음 구성 요소를 완료해야 합니다.

1. **[Azure AD Single Sign-On 구성](#configure-azure-ad-single-sign-on)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
2. **[Percolate Single Sign-On 구성](#configure-percolate-single-sign-on)** - 애플리케이션 쪽에서 Single Sign-On 설정을 구성합니다.
3. **[Azure AD 테스트 사용자 만들기](#create-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On을 테스트하는 데 사용합니다.
4. **[Azure AD 테스트 사용자 할당](#assign-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
5. **[Percolate 테스트 사용자 만들기](#create-percolate-test-user)** - Britta Simon의 Azure AD 표현과 연결된 해당 사용자를 Percolate에 만듭니다.
6. **[Single Sign-On 테스트](#test-single-sign-on)** - 구성이 작동하는지 여부를 확인합니다.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure Portal에서 Azure AD Single Sign-On을 사용하도록 설정합니다.

Percolate에서 Azure AD Single Sign-on을 구성하려면 다음 단계를 수행합니다.

1. [Azure Portal](https://portal.azure.com/)의 **Percolate** 애플리케이션 통합 페이지에서 **Single Sign-On**을 선택합니다.

    ![Single Sign-On 구성 링크](common/select-sso.png)

2. **Single Sign-On 방법 선택** 대화 상자에서 **SAML/WS-Fed** 모드를 선택하여 Single Sign-On을 사용하도록 설정합니다.

    ![Single Sign-On 선택 모드](common/select-saml-option.png)

3. **SAML로 Single Sign-On 설정** 페이지에서 **편집** 아이콘을 클릭하여 **기본 SAML 구성** 대화 상자를 엽니다.

    ![기본 SAML 구성 편집](common/edit-urls.png)

4. **기본 SAML 구성** 섹션에서  **IDP**  시작 모드로 애플리케이션을 구성하려는 경우에는 앱이 이미 Azure와 사전 통합되어 있으므로 사용자가 수행해야 하는 단계가 없습니다.

    ![Percolate 도메인 및 URL Single Sign-On 정보](common/preintegrated.png)

5. **SP** 시작 모드에서 애플리케이션을 구성하려면 **추가 URL 설정**을 클릭하고 다음 단계를 수행합니다.

    ![Percolate 도메인 및 URL Single Sign-On 정보](common/metadata-upload-additional-signon.png)

    **로그온 URL** 텍스트 상자에 `https://percolate.com/app/login` URL을 입력합니다.

6. **SAML로 Single Sign-On 설정** 페이지의 **SAML 서명 인증서** 섹션에서 복사 단추를 클릭하여 **앱 페더레이션 메타데이터 URL**을 복사한 후 컴퓨터에 저장합니다.

    ![인증서 다운로드 링크](common/copy-metadataurl.png)

7. **Percolate 설정** 섹션에서 요구 사항에 따라 적절한 URL을 복사합니다.

    ![구성 URL 복사](common/copy-configuration-urls.png)

    a. 로그인 URL

    b. Azure AD 식별자

    다. 로그아웃 URL

### <a name="configure-percolate-single-sign-on"></a>Percolate Single Sign-On 구성

1. 다른 웹 브라우저 창에서 Percolate에 관리자로 로그인합니다.

2. 홈 페이지의 왼쪽에서 **설정**을 클릭합니다.
    
    ![Configure Single Sign-On](./media/percolate-tutorial/configure01.png)

3. 메뉴 모음의 왼쪽에서 **조직** 아래에 있는 **SSO**를 클릭합니다.

    ![Configure Single Sign-On](./media/percolate-tutorial/configure02.png)

    a. Azure Portal에서 복사한 **로그인 URL** 값을 **로그인 URL** 텍스트 상자에 붙여넣습니다.

    b. **엔터티 ID** 텍스트 상자에 Azure Portal에서 복사한 **Azure AD 식별자** 값을 붙여넣습니다.

    다. 메모장에서, Azure Portal에서 다운로드한 base-64로 인코딩된 인증서를 열고 콘텐츠를 복사한 다음,  **x509 인증서**  상자에 붙여넣습니다.

    d. **이메일 특성** 텍스트 상자에 **emailaddress**를 입력합니다.

    e. **ID 공급자 메타데이터 URL** 필드는 선택 사항이며, Azure Portal에서 복사한 **앱 페더레이션 메타데이터 URL**이 있다면 **ID 공급자 메타데이터 URL** 텍스트 상자에 붙여넣습니다.

    f. **AuthNRequests에 서명해야 하는지 여부**에 **아니요**를 선택합니다.

    g. **SSO 자동 프로비전 설정**에 **아니요**를 선택합니다.

    h. **저장**을 클릭합니다.

### <a name="create-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기 

이 섹션의 목적은 Azure Portal에서 Britta Simon이라는 테스트 사용자를 만드는 것입니다.

1. Azure Portal의 왼쪽 창에서 **Azure Active Directory**, **사용자**를 차례로 선택하고 **모든 사용자**를 선택합니다.

    !["사용자 및 그룹" 및 "모든 사용자" 링크](common/users.png)

2. 화면 위쪽에서 **새 사용자**를 선택합니다.

    ![새 사용자 단추](common/new-user.png)

3. 사용자 속성에서 다음 단계를 수행합니다.

    ![사용자 대화 상자](common/user-properties.png)

    a. **이름** 필드에 **BrittaSimon**을 입력합니다.
  
    b. **사용자 이름** 필드에 brittasimon@yourcompanydomain.extension을 입력합니다. 예를 들어 BrittaSimon@contoso.com

    c. **암호 표시** 확인란을 선택한 다음, [암호] 상자에 표시된 값을 적어둡니다.

    d. **만들기**를 클릭합니다.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 Britta Simon에게 Percolate에 대한 액세스 권한을 부여합니다.

1. Azure Portal에서 **엔터프라이즈 애플리케이션**, **모든 애플리케이션**, **Percolate**를 차례로 선택합니다.

    ![엔터프라이즈 애플리케이션 블레이드](common/enterprise-applications.png)

2. 애플리케이션 목록에서 **Percolate**를 선택합니다.

    ![애플리케이션 목록의 Percolate 링크](common/all-applications.png)

3. 왼쪽 메뉴에서 **사용자 및 그룹**을 선택합니다.

    !["사용자 및 그룹" 링크](common/users-groups-blade.png)

4. **사용자 추가** 단추를 클릭한 다음, **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![할당 추가 창](common/add-assign-user.png)

5. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택하고 화면 아래쪽에서 **선택** 단추를 클릭합니다.

6. SAML 어설션 및 **역할 선택** 대화 상자에서 모든 역할 값이 필요한 경우 목록에서 적절한 사용자 역할을 선택한 다음, 화면 맨 아래에 있는 **선택** 단추를 클릭합니다.

7. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.

### <a name="create-percolate-test-user"></a>Percolate 테스트 사용자 만들기

Azure AD 사용자가 Percolate에 로그인할 수 있도록 하려면 Percolate로 프로비저닝되어야 합니다. Percolate에서 프로비전 작업은 수동으로 수행됩니다.

**사용자 계정을 프로비저닝하려면 다음 단계를 수행합니다.**

1. Percolate에 관리자로 로그인합니다.

2. 메뉴 모음의 왼쪽에서 **조직** 아래에 있는 **사용자**를 클릭하고 **새 사용자**로 이동합니다.

    ![Configure Single Sign-On](./media/percolate-tutorial/configure03.png)

3. **사용자 만들기** 페이지에서 다음 단계를 수행합니다.

    ![Configure Single Sign-On](./media/percolate-tutorial/configure04.png)

    a.  **이메일**  텍스트 상자에 사용자의 이메일(예: brittasimon@contoso.com)을 입력합니다.

    b.  **전체 이름**  텍스트 상자에 사용자 이름(예:  **Britta Simon**)을 입력합니다.

    다. **사용자 만들기**를 클릭합니다.

### <a name="test-single-sign-on"></a>Single Sign-On 테스트 

이 섹션에서는 액세스 패널을 사용하여 Azure AD Single Sign-On 구성을 테스트합니다.

액세스 패널에서 Percolate 타일을 클릭하면 SSO가 설정된 Percolate에 자동으로 로그인됩니다. 액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)를 참조하세요.

## <a name="additional-resources"></a>추가 리소스

- [ Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록 ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory로 애플리케이션 액세스 및 Single Sign-On이란 무엇입니까? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory의 조건부 액세스란?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
