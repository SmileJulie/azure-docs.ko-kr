---
title: '자습서: monday.com과 Azure Active Directory 통합 | Microsoft Docs'
description: Azure Active Directory 및 monday.com 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 9e8ad807-0664-4e31-91de-731097c768e2
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/08/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 93c5e473c62dc6b38f0b2c2906560d6099842d49
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67718488"
---
# <a name="tutorial-integrate-mondaycom-with-azure-active-directory"></a>자습서: Azure Active Directory와 monday.com 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 monday.com을 연결하는 방법에 대해 알아봅니다. Azure AD와 monday.com을 통합하면 다음을 수행할 수 있습니다.

* monday.com에 대한 액세스 권한이 있는 사용자를 Azure AD에서 제어합니다.
* 사용자가 자신의 Azure AD 계정으로 monday.com에 자동으로 로그인되도록 설정할 수 있습니다.
* 단일 중앙 위치인 Azure Portal에서 계정을 관리합니다.

Azure AD와 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory를 사용한 애플리케이션 액세스 및 Single Sign-On이란 무엇인가요?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

시작하려면 다음 항목이 필요합니다.

* Azure AD 구독 구독이 없는 경우 [체험 계정](https://azure.microsoft.com/free/)을 얻을 수 있습니다.
* monday.com SSO(Single Sign-On)가 설정된 구독

## <a name="scenario-description"></a>시나리오 설명

이 자습서에서는 테스트 환경에서 Azure AD SSO를 구성하고 테스트합니다. monday.com에서 **SP 및 IDP** 시작 SSO 및 **Just-In-Time** 사용자 프로비저닝을 지원합니다.

## <a name="adding-mondaycom-from-the-gallery"></a>갤러리에서 monday.com 추가

Azure AD에 monday.com 연결을 구성하려면 갤러리의 monday.com을 관리형 SaaS 앱 목록에 추가해야 합니다.

1. [Azure Portal](https://portal.azure.com)에 회사 또는 학교 계정, 개인 Microsoft 계정으로 로그인합니다.
1. 왼쪽 탐색 창에서 **Azure Active Directory** 서비스를 선택합니다.
1. **엔터프라이즈 애플리케이션**으로 이동한 다음, **모든 애플리케이션**을 선택합니다.
1. 새 애플리케이션을 추가하려면 **새 애플리케이션**을 선택합니다.
1. **갤러리에서 추가** 섹션의 검색 상자에 **monday.com**을 입력합니다.
1. 결과 패널에서 **monday.com**을 선택한 다음 앱을 추가합니다. 앱이 테넌트에 추가될 때까지 잠시 동안 기다려 주세요.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성 및 테스트

**B.Simon**이라는 테스트 사용자를 사용하여 monday.com에서 Azure AD SSO를 구성하고 테스트합니다. SSO가 작동하려면 Azure AD 사용자와 monday.com의 관련 사용자 간에 연결 관계를 설정해야 합니다.

monday.com에서 Azure AD SSO를 구성하고 테스트하려면 다음 구성 요소를 완료합니다.

1. 사용자가 이 기능을 사용할 수 있도록 **[Azure AD SSO를 구성](#configure-azure-ad-sso)** 합니다.
2. **[monday.com 구성](#configure-mondaycom)** - 애플리케이션 쪽에서 SSO 설정을 구성합니다.
3. **[Azure AD 테스트 사용자 만들기](#create-an-azure-ad-test-user)** - B.Simon을 사용하여 Azure AD Single Sign-On을 테스트합니다.
4. **[Azure AD 테스트 사용자 할당](#assign-the-azure-ad-test-user)** - B.Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
5. **[monday.com 테스트 사용자 만들기](#create-mondaycom-test-user)** - B.Simon의 Azure AD 표현과 연결된 해당 사용자를 monday.com에 만듭니다.
6. **[SSO를 테스트](#test-sso)** 하여 구성이 작동하는지 여부를 확인합니다.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO 구성

Azure Portal에서 Azure AD SSO를 사용하도록 설정하려면 다음 단계를 수행합니다.

1. [Azure Portal](https://portal.azure.com/)의 **monday.com** 애플리케이션 통합 페이지에서 **관리** 섹션을 찾은 다음, **Single Sign-On**을 선택합니다.
1. **Single Sign-On 방법 선택** 페이지에서 **SAML**을 선택합니다.
1. **SAML로 Single Sign-On 설정** 페이지에서 **기본 SAML 구성**에 대한 편집(연필 모양) 아이콘을 클릭하여 설정을 편집합니다.

   ![기본 SAML 구성 편집](common/edit-urls.png)

1. 서비스 공급자 메타데이터 파일이 있고 **IDP 시작 모드**를 구성하려면 **기본 SAML 구성** 섹션에서 다음 단계를 수행합니다.

    1. **메타데이터 파일 업로드**를 선택합니다.

    1. 메타데이터 파일을 선택하려면 폴더 아이콘을 선택하고 **업로드**를 선택합니다.

    1. 메타데이터 파일이 성공적으로 업로드되면 **식별자** 및 **회신 URL** 값이 **기본 SAML 구성** 창에 자동으로 채워집니다.

       > [!Note]
       > **식별자** 및 **회신 URL** 값이 자동으로 입력되지 않으면 수동으로 값을 입력합니다. **식별자** 및 **회신 URL**은 동일하며 값의 패턴은 다음과 같습니다. `https://<your-domain>.monday.com/saml/saml_callback`

1. **SP** 시작 모드에서 애플리케이션을 구성하려면 **추가 URL 설정**을 클릭하고 다음 단계를 수행합니다.

    **로그인 URL** 텍스트 상자에서 `https://<your-domain>.monday.com` 패턴을 사용하여 URL을 입력합니다.

    > [!NOTE]
    > 이러한 값은 실제 값이 아닙니다. 실제 식별자, 회신 URL 및 로그온 URL을 사용하여 이러한 값을 업데이트합니다. 이러한 값을 얻으려면 [monday.com 클라이언트 지원 팀](mailto:dev@food.ee)에 문의하세요. Azure Portal의 **기본 SAML 구성** 섹션에 표시된 패턴을 참조할 수도 있습니다.

1. monday.com 애플리케이션은 특정 서식에서 SAML 어설션을 예상하며, 이는 SAML 토큰 특성 구성에 사용자 지정 특성 매핑을 추가해야 합니다. 다음 스크린샷에서는 기본 특성의 목록을 보여 줍니다.  **편집**  단추를 클릭하여 사용자 특성 대화 상자를 엽니다.

    ![사용자 특성 창](common/edit-attribute.png)

1. 위에서 언급한 특성 외에도, monday.com 애플리케이션에는 SAML 응답에서 다시 전달되어야 하는 몇 가지 특성이 추가로 필요합니다. **사용자 특성** 대화 상자의 **사용자 클레임** 섹션에서 다음 단계를 수행하여 아래 표와 같은 SAML 토큰 특성을 추가합니다.

    | 이름 | 원본 특성|
    | ---------------| --------------- |
    | Email | user.mail |
    | FirstName | user.givenname |
    | LastName | user.surname |

    a. **새 클레임 추가**를 클릭하여 **사용자 클레임 관리** 대화 상자를 엽니다.

    b. **이름** 텍스트 상자에서 해당 행에 표시된 특성 이름을 입력합니다.

    다. **네임스페이스**를 제거합니다.

    d. 원본을 **특성**으로 선택합니다.

    e. **원본 특성** 목록에서 해당 행에 표시된 특성 값을 입력합니다.

    f. **Ok**를 클릭합니다.

    g. **저장**을 클릭합니다.

1. **SAML로 Single Sign-On 설정** 페이지의 **SAML 서명 인증서** 섹션에서 **인증서(Base64)** 를 찾은 후 **다운로드**를 선택하여 인증서를 컴퓨터에 다운로드하고 본인의 컴퓨터에 저장합니다.

   ![인증서 다운로드 링크](common/certificatebase64.png)

1. **monday.com 설정** 섹션에서 요구 사항에 따라 적절한 URL을 복사합니다.

   ![구성 URL 복사](common/copy-configuration-urls.png)

### <a name="configure-mondaycom"></a>monday.com 구성

1. monday.com 내에서 구성을 자동화하려면 **확장 설치**를 클릭하여 **내 앱 보안 로그인 브라우저 확장**을 설치해야 합니다.

    ![내 앱 확장](common/install-myappssecure-extension.png)

1. 브라우저에 확장을 추가한 후 **monday.com 설정**을 클릭하면 monday.com 애플리케이션으로 이동합니다. 여기서 관리자 자격 증명을 입력하여 monday.com에 로그인합니다. 브라우저 확장이 애플리케이션을 자동으로 구성하고 3-6단계를 자동으로 수행합니다.

    ![구성 설정](common/setup-sso.png)

1. monday.com을 수동으로 설정하려면 새 웹 브라우저 창을 열고 monday.com에 관리자로 로그인하여 다음 단계를 수행합니다.

1. 페이지 오른쪽 위 모서리의 **프로필**로 이동하여 **관리자**를 클릭합니다.

    ![monday.com 구성](./media/mondaycom-tutorial/configuration01.png)

1. **보안**을 선택하고 SAML 옆에 있는 **열기**를 클릭해야 합니다.

    ![monday.com 구성](./media/mondaycom-tutorial/configuration02.png)

1. IDP에서 아래 세부 정보를 입력합니다.

    ![monday.com 구성](./media/mondaycom-tutorial/configuration03.png)

    >[!NOTE]
    >자세한 내용은 [이](https://support.monday.com/hc/articles/360000460605-SAML-Single-Sign-on?abcb=34642) 문서를 참조하세요.

### <a name="create-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기

이 섹션에서는 Azure Portal에서 B.Simon이라는 테스트 사용자를 만듭니다.

1. Azure Portal의 왼쪽 창에서 **Azure Active Directory**, **사용자**, **모든 사용자**를 차례로 선택합니다.
1. 화면 위쪽에서 **새 사용자**를 선택합니다.
1. **사용자** 속성에서 다음 단계를 수행합니다.
   1. **이름** 필드에 `B.Simon`을 입력합니다.  
   1. **사용자 이름** 필드에서 username@companydomain.extension을 입력합니다. 예: `B.Simon@contoso.com`
   1. **암호 표시** 확인란을 선택한 다음, **암호** 상자에 표시된 값을 적어둡니다.
   1. **만들기**를 클릭합니다.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 B.Simon에게 monday.com에 대한 액세스 권한을 부여합니다.

1. Azure Portal에서 **엔터프라이즈 애플리케이션**을 선택한 다음, **모든 애플리케이션**을 선택합니다.
1. 애플리케이션 목록에서 **monday.com**을 선택합니다.
1. 앱의 개요 페이지에서 **관리** 섹션을 찾고 **사용자 및 그룹**을 선택합니다.

   !["사용자 및 그룹" 링크](common/users-groups-blade.png)

1. **사용자 추가**를 선택한 다음, **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![사용자 추가 링크](common/add-assign-user.png)

1. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **B.Simon**을 선택한 다음, 화면 아래쪽에서 **선택** 단추를 클릭합니다.
1. SAML 어설션에 역할 값이 필요한 경우 **역할 선택** 대화 상자의 목록에서 사용자에 대한 적절한 역할을 선택한 다음, 화면의 아래쪽에 있는 **선택** 단추를 클릭합니다.
1. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.

### <a name="create-mondaycom-test-user"></a>monday.com 테스트 사용자 만들기

이 섹션에서는 monday.com에서 B.Simon이라는 사용자를 만듭니다. monday.com은 기본적으로 사용하도록 설정되는 Just-In-Time 프로비전을 지원합니다. 이 섹션에 작업 항목이 없습니다. monday.com에 사용자가 아직 없는 경우 monday.com에 액세스하려고 할 때 새 사용자가 만들어집니다.

### <a name="test-sso"></a>SSO 테스트

액세스 패널에서 monday.com 타일을 선택하면 SSO를 설정한 monday.com에 자동으로 로그인되어야 합니다. 액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)를 참조하세요.

## <a name="additional-resources"></a>추가 리소스

- [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory로 애플리케이션 액세스 및 Single Sign-On을 구현하는 방법](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory의 조건부 액세스란?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
