---
title: '자습서: Check Point CloudGuard Dome9 Arc와 Azure Active Directory 통합 | Microsoft Docs'
description: Azure Active Directory 및 Check Point CloudGuard Dome9 Arc 간에 Single Sign-On을 구성하는 방법을 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 4c12875f-de71-40cb-b9ac-216a805334e5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: fdaaab8257d3a79130902e1ba0466f9cf15484f4
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67147190"
---
# <a name="tutorial-integrate-check-point-cloudguard-dome9-arc-with-azure-active-directory"></a>자습서: Azure Active Directory와 Check Point CloudGuard Dome9 Arc 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 Check Point CloudGuard Dome9 Arc를 통합하는 방법에 대해 알아봅니다. Azure AD와 Check Point CloudGuard Dome9 Arc를 통합하면 다음을 수행할 수 있습니다.

* Check Point CloudGuard Dome9 Arc에 대한 액세스 권한이 있는 사용자를 Azure AD에서 제어할 수 있습니다.
* 사용자가 해당 Azure AD 계정으로 Check Point CloudGuard Dome9 Arc에 자동으로 로그인되도록 설정할 수 있습니다.
* 단일 중앙 위치인 Azure Portal에서 계정을 관리합니다.

Azure AD와 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory를 사용한 애플리케이션 액세스 및 Single Sign-On이란 무엇인가요?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

시작하려면 다음 항목이 필요합니다.

* Azure AD 구독 구독이 없는 경우 [체험 계정](https://azure.microsoft.com/free/)을 얻을 수 있습니다.
* Check Point CloudGuard Dome9 Arc SSO(Single Sign-On)가 설정된 구독

## <a name="scenario-description"></a>시나리오 설명

이 자습서에서는 테스트 환경에서 Azure AD SSO를 구성하고 테스트합니다. Check Point CloudGuard Dome9 Arc는 **SP 및 IDP** 시작 SSO를 지원합니다.

## <a name="adding-check-point-cloudguard-dome9-arc-from-the-gallery"></a>갤러리의 Check Point CloudGuard Dome9 Arc 추가

Azure AD에 Check Point CloudGuard Dome9 Arc를 통합하도록 구성하려면 갤러리의 Check Point CloudGuard Dome9 Arc를 관리형 SaaS 앱 목록에 추가해야 합니다.

1. [Azure Portal](https://portal.azure.com)에 회사 또는 학교 계정, 개인 Microsoft 계정으로 로그인합니다.
1. 왼쪽 탐색 창에서 **Azure Active Directory** 서비스를 선택합니다.
1. **엔터프라이즈 애플리케이션**으로 이동한 다음, **모든 애플리케이션**을 선택합니다.
1. 새 애플리케이션을 추가하려면 **새 애플리케이션**을 선택합니다.
1. **갤러리에서 추가** 섹션의 검색 상자에 **Check Point CloudGuard Dome9 Arc**를 입력합니다.
1. 결과 패널에서 **Check Point CloudGuard Dome9 Arc**를 선택한 후 앱을 추가합니다. 앱이 테넌트에 추가될 때까지 잠시 동안 기다려 주세요.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성 및 테스트

**B.Simon**이라는 테스트 사용자를 사용하여 Check Point CloudGuard Dome9 Arc에서 Azure AD SSO를 구성하고 테스트합니다. SSO가 작동하려면 Azure AD 사용자와 Check Point CloudGuard Dome9 Arc의 관련 사용자 간에 연결 관계를 설정해야 합니다.

Check Point CloudGuard Dome9 Arc에서 Azure AD SSO를 구성하고 테스트하려면 다음 구성 요소를 완료합니다.

1. 사용자가 이 기능을 사용할 수 있도록 **[Azure AD SSO를 구성](#configure-azure-ad-sso)** 합니다.
2. **[Check Point CloudGuard Dome9 Arc 구성](#configure-check-point-cloudguard-dome9-arc)** - 애플리케이션 쪽에서 SSO 설정을 구성합니다.
3. **[Azure AD 테스트 사용자를 생성](#create-an-azure-ad-test-user)** 하여 B.Simon으로 Azure AD Single Sign-On을 테스트합니다.
4. **[Azure AD 테스트 사용자를 할당](#assign-the-azure-ad-test-user)** 하여 B.Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
5. **[Check Point CloudGuard Dome9 Arc 테스트 사용자를 생성](#create-check-point-cloudguard-dome9-arc-test-user)** 하여 B.Simon의 Azure AD 표현과 연결된 해당 사용자를 Check Point CloudGuard Dome9 Arc에 만듭니다.
6. **[SSO를 테스트](#test-sso)** 하여 구성이 작동하는지 여부를 확인합니다.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO 구성

Azure Portal에서 Azure AD SSO를 사용하도록 설정하려면 다음 단계를 수행합니다.

1. [Azure Portal](https://portal.azure.com/)의 **Check Point CloudGuard Dome9 Arc** 애플리케이션 통합 페이지에서 **관리** 섹션을 찾은 후 **Single Sign-On**을 선택합니다.
1. **Single Sign-On 방법 선택** 페이지에서 **SAML**을 선택합니다.
1. **SAML로 Single Sign-On 설정** 페이지에서 **기본 SAML 구성**에 대한 편집(연필 모양) 아이콘을 클릭하여 설정을 편집합니다.

   ![기본 SAML 구성 편집](common/edit-urls.png)

4. **IDP** 시작 모드에서 애플리케이션을 구성하려면 **기본 SAML 구성** 섹션에서 다음 단계를 수행합니다.

    a. **식별자** 텍스트 상자에서 `https://secure.dome9.com/` 패턴을 사용하여 URL을 입력합니다.

    b. **회신 URL** 텍스트 상자에서 `https://secure.dome9.com/sso/saml/yourcompanyname` 패턴을 사용하여 URL을 입력합니다.

    > [!NOTE]
    > 이 자습서의 뒷부분에서 설명하는 dome9 관리 포털에서 회사 이름 값을 선택합니다.

5. **SP** 시작 모드에서 애플리케이션을 구성하려면 **추가 URL 설정**을 클릭하고 다음 단계를 수행합니다.

    **로그인 URL** 텍스트 상자에서 `https://secure.dome9.com/sso/saml/<yourcompanyname>` 패턴을 사용하여 URL을 입력합니다.

    > [!NOTE]
    > 이러한 값은 실제 값이 아닙니다. 실제 회신 URL 및 로그온 URL을 사용하여 이러한 값을 업데이트합니다. 이러한 값을 확인하려면 [Check Point CloudGuard Dome9 Arc 클라이언트 지원 팀](mailto:Dome9@checkpoint.com)에 문의하세요. Azure Portal의 **기본 SAML 구성** 섹션에 표시된 패턴을 참조할 수도 있습니다.

6. Check Point CloudGuard Dome9 Arc 애플리케이션은 특정 형식의 SAML 어설션을 예상하며, 따라서 SAML 토큰 특성 구성에 사용자 지정 특성 매핑을 추가해야 합니다. 다음 스크린샷에서는 기본 특성의 목록을 보여 줍니다.  **편집**  단추를 클릭하여 사용자 특성 대화 상자를 엽니다.

    ![이미지](common/edit-attribute.png)

7. 위에서 언급한 특성 외에도, Check Point CloudGuard Dome9 Arc 애플리케이션에는 SAML 응답에 다시 전달되어야 하는 몇 가지 특성이 추가로 필요합니다. **사용자 특성** 대화 상자의 **사용자 클레임** 섹션에서 다음 단계를 수행하여 아래 표와 같은 SAML 토큰 특성을 추가합니다. 

    | 이름 |  원본 특성|
    | ---------------| --------------- |
    | memberof | user.assignedroles |

    a. **새 클레임 추가**를 클릭하여 **사용자 클레임 관리** 대화 상자를 엽니다.

    ![이미지](common/new-save-attribute.png)

    ![이미지](common/new-attribute-details.png)

    b. **이름** 텍스트 상자에서 해당 행에 표시된 특성 이름을 입력합니다.

    다. **네임스페이스**를 비워 둡니다.

    d. 원본을 **특성**으로 선택합니다.

    e. **원본 특성** 목록에서 해당 행에 표시된 특성 값을 입력합니다.

    f. **확인**을 클릭합니다.

    g. **저장**을 클릭합니다.

1. **SAML로 Single Sign-On 설정** 페이지의 **SAML 서명 인증서** 섹션에서 **인증서(Base64)** 를 찾은 후 **다운로드**를 선택하여 인증서를 컴퓨터에 다운로드하고 본인의 컴퓨터에 저장합니다.

   ![인증서 다운로드 링크](common/certificatebase64.png)

1. **Check Point CloudGuard Dome9 Arc 설정** 섹션에서 요구 사항에 따라 적절한 URL을 복사합니다.

   ![구성 URL 복사](common/copy-configuration-urls.png)

### <a name="configure-check-point-cloudguard-dome9-arc"></a>Check Point CloudGuard Dome9 Arc 구성

1. 다른 웹 브라우저 창에서 Check Point CloudGuard Dome9 Arc 회사 사이트에 관리자 권한으로 로그인합니다.

2. 오른쪽 위 모서리에서 **프로필 설정**을 클릭한 다음, **계정 설정**을 클릭합니다. 

    ![Check Point CloudGuard Dome9 Arc 구성](./media/dome9arc-tutorial/configure1.png)

3. **SSO**로 이동한 다음 **사용**을 클릭합니다.

    ![Check Point CloudGuard Dome9 Arc 구성](./media/dome9arc-tutorial/configure2.png)

4. [SSO 구성] 섹션에서 다음 단계를 수행합니다.

    ![Check Point CloudGuard Dome9 Arc 구성](./media/dome9arc-tutorial/configure3.png)

    a. **계정 ID** 텍스트 상자에서 회사 이름을 입력합니다. 이 값은 Azure Portal **기본 SAML 구성** 섹션에서 언급한 회신 URL에 사용됩니다.

    b. Azure Portal에서 복사한 **Azure AD 식별자** 값을 **발급자** 텍스트 상자에 붙여넣습니다.

    다. Azure Portal에서 복사한 **로그인 URL** 값을 **Idp 엔드포인트 URL** 텍스트 상자에 붙여넣습니다.

    d. 다운로드한 Base64로 인코딩된 인증서를 메모장에서 열고, 내용을 클립보드에 복사한 다음, **X.509 인증서** 텍스트 상자에 붙여넣습니다.

    e. **저장**을 클릭합니다.

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

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 B.Simon에게 Check Point CloudGuard Dome9 Arc에 대한 액세스 권한을 부여합니다.

1. Azure Portal에서 **엔터프라이즈 애플리케이션**을 선택한 다음, **모든 애플리케이션**을 선택합니다.
1. 애플리케이션 목록에서 **Check Point CloudGuard Dome9 Arc**를 선택합니다.
1. 앱의 개요 페이지에서 **관리** 섹션을 찾고 **사용자 및 그룹**을 선택합니다.

   !["사용자 및 그룹" 링크](common/users-groups-blade.png)

1. **사용자 추가**를 선택한 다음, **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![사용자 추가 링크](common/add-assign-user.png)

1. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **B.Simon**을 선택한 다음, 화면 아래쪽에서 **선택** 단추를 클릭합니다.
1. SAML 어설션에 역할 값이 필요한 경우 **역할 선택** 대화 상자의 목록에서 사용자에 대한 적절한 역할을 선택한 다음, 화면의 아래쪽에 있는 **선택** 단추를 클릭합니다.
1. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.

### <a name="create-check-point-cloudguard-dome9-arc-test-user"></a>Check Point CloudGuard Dome9 Arc 테스트 사용자 만들기

Azure AD 사용자가 Check Point CloudGuard Dome9 Arc에 로그인할 수 있게 하려면 해당 사용자를 애플리케이션에 프로비저닝해야 합니다. Check Point CloudGuard Dome9 Arc는 JIT(Just-In-Time) 프로비저닝을 지원하지만 이 기능이 제대로 작동하려면 사용자가 특정 **역할**을 선택하여 사용자에게 할당해야 합니다.

   >[!Note]
   >**역할**을 만드는 방법 및 다른 자세한 내용은 [Check Point CloudGuard Dome9 Arc 클라이언트 지원 팀](mailto:Dome9@checkpoint.com)에 문의하세요.

**사용자 계정을 수동으로 프로비전하려면 다음 단계를 수행합니다.**

1. Check Point CloudGuard Dome9 Arc 회사 사이트에 관리자 권한으로 로그인합니다.

2. **사용자 및 역할**을 클릭한 다음, **사용자**를 클릭합니다.

    ![직원 추가](./media/dome9arc-tutorial/user1.png)

3. **사용자 추가**를 클릭합니다.

    ![직원 추가](./media/dome9arc-tutorial/user2.png)

4. **사용자 만들기** 섹션에서 다음 단계를 수행합니다.

    ![직원 추가](./media/dome9arc-tutorial/user3.png)

    a. **이메일** 텍스트 상자에서 사용자의 이메일(예: B.Simon@contoso.com)을 입력합니다.

    b. **이름** 텍스트 상자에 사용자의 이름(예: B)을 입력합니다.

    다. **성** 텍스트 상자에 사용자의 성(예: Simon)을 입력합니다.

    d. **SSO 사용자**를 **설정**으로 지정합니다.

    e. **만들기**를 클릭합니다.

### <a name="test-sso"></a>SSO 테스트

액세스 패널에서 Check Point CloudGuard Dome9 Arc 타일을 선택하면 SSO를 설정한 Check Point CloudGuard Dome9 Arc에 자동으로 로그인됩니다. 액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)를 참조하세요.

## <a name="additional-resources"></a>추가 리소스

- [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory로 애플리케이션 액세스 및 Single Sign-On을 구현하는 방법](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory의 조건부 액세스란?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)