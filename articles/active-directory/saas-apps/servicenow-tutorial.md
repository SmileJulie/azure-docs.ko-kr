---
title: '자습서: ServiceNow와 Azure Active Directory 통합 | Microsoft Docs'
description: Azure Active Directory와 ServiceNow 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: a5a1a264-7497-47e7-b129-a1b5b1ebff5b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/27/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ba516aa2c3d2decaa4962f1ccd0394ebe9a4a62
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67706119"
---
# <a name="tutorial-integrate-servicenow-with-azure-active-directory"></a>자습서: ServiceNow와 Azure Active Directory 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 ServiceNow를 통합하는 방법에 대해 알아봅니다. Azure AD와 ServiceNow를 통합하면 다음 작업을 할 수 있습니다.

* ServiceNow에 대한 액세스 권한이 있는 사용자를 Azure AD에서 제어합니다.
* 사용자가 자신의 Azure AD 계정으로 ServiceNow에 자동으로 로그인되도록 합니다.
* 단일 중앙 위치인 Azure Portal에서 계정을 관리합니다.

Azure AD와 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory를 사용한 애플리케이션 액세스 및 Single Sign-On이란 무엇인가요?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

시작하려면 다음 항목이 필요합니다.

* Azure AD 구독 구독이 없는 경우 [체험 계정](https://azure.microsoft.com/free/)을 얻을 수 있습니다.
* ServiceNow SSO(Single Sign-On)가 설정된 구독
* ServiceNow의 경우 ServiceNow의 인스턴스 또는 테넌트, Calgary 버전 이상
* ServiceNow Express의 경우 ServiceNow Express의 인스턴스, Helsinki 버전 이상
* ServiceNow 테넌트에서 [여러 공급자 Single Sign-On 플러그 인](https://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0)을 사용하도록 설정해야 합니다. [서비스 요청을 제출](https://hi.service-now.com)하면 이렇게 설정할 수 있습니다.
* 자동 구성은 ServiceNow에 대한 다중 공급자 플러그 인을 사용하도록 설정합니다.
* ServiceNow Classic(모바일) 애플리케이션을 설치하려면 적절한 스토어로 이동한 다음, ServiceNow Classic(모바일) 애플리케이션을 검색하고 [다운로드]를 클릭합니다.

## <a name="scenario-description"></a>시나리오 설명

이 자습서에서는 테스트 환경에서 Azure AD SSO를 구성하고 테스트합니다. ServiceNow는 **SP** 시작 SSO를 지원하며 [**자동화된** 사용자 프로비저닝](servicenow-provisioning-tutorial.md)을 지원합니다.

이제 ServiceNow Classic(모바일) 애플리케이션은 Azure AD를 사용하여 SSO를 설정할 수 있으며 **Android** 및 **IOS** 사용자를 모두 지원합니다. 이 자습서에서는 테스트 환경에서 Azure AD SSO를 구성하고 테스트합니다.

## <a name="adding-servicenow-from-the-gallery"></a>갤러리에서 ServiceNow 추가

ServiceNow의 Azure AD 통합을 구성하려면 갤러리의 ServiceNow를 관리되는 SaaS 앱 목록에 추가해야 합니다.

1. [Azure Portal](https://portal.azure.com)에 회사 또는 학교 계정, 개인 Microsoft 계정으로 로그인합니다.
1. 왼쪽 탐색 창에서 **Azure Active Directory** 서비스를 선택합니다.
1. **엔터프라이즈 애플리케이션**으로 이동한 다음, **모든 애플리케이션**을 선택합니다.
1. 새 애플리케이션을 추가하려면 **새 애플리케이션**을 선택합니다.
1. **갤러리에서 추가** 섹션의 검색 상자에 **ServiceNow**를 입력합니다.
1. 결과 패널에서 **ServiceNow**를 선택한 다음, 앱을 추가합니다. 앱이 테넌트에 추가될 때까지 잠시 동안 기다려 주세요.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성 및 테스트

**B.Simon**이라는 테스트 사용자를 사용하여 ServiceNow에서 Azure AD SSO를 구성하고 테스트합니다. SSO가 작동하려면 Azure AD 사용자와 ServiceNow의 관련 사용자 간에 연결 관계를 설정해야 합니다.

ServiceNow에서 Azure AD SSO를 구성하고 테스트하려면 다음 구성 요소를 완료합니다.

1. 사용자가 이 기능을 사용할 수 있도록 **[Azure AD SSO를 구성](#configure-azure-ad-sso)** 합니다.
2. **[ServiceNow 구성](#configure-servicenow)** - 애플리케이션 쪽에서 SSO 설정을 구성합니다.
3. **[ServiceNow Express에 대해 Azure AD SSO 구성](#configure-azure-ad-sso-for-servicenow-express)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
4. **[ServiceNow Express SSO 구성](#configure-servicenow-express-sso)** - 애플리케이션 쪽에서 Single Sign-On 설정을 구성합니다.
5. **[Azure AD 테스트 사용자 만들기](#create-an-azure-ad-test-user)** - B.Simon을 사용하여 Azure AD Single Sign-On을 테스트합니다.
6. **[Azure AD 테스트 사용자를 할당](#assign-the-azure-ad-test-user)** 하여 B.Simon이 Azure AD Single Sign-On을 사용할 수 있도록 합니다.
7. **[ServiceNow 테스트 사용자 만들기](#create-servicenow-test-user)** - Azure AD 표현과 연결된 B. Simon에 해당하는 사용자를 ServiceNow에 만듭니다.
8. **[SSO를 테스트](#test-sso)** 하여 구성이 작동하는지 여부를 확인합니다.
9. **[ServiceNow Classic(모바일)에 대한 SSO 테스트](#test-sso-for-servicenow-classic-mobile)** 를 수행하여 구성이 작동하는지 확인합니다.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO 구성

Azure Portal에서 Azure AD SSO를 사용하도록 설정하려면 다음 단계를 수행합니다.

1. [Azure Portal](https://portal.azure.com/)의 **ServiceNow** 애플리케이션 통합 페이지에서 **관리** 섹션을 찾아 **Single Sign-On**을 선택합니다.
1. **Single Sign-On 방법 선택** 페이지에서 **SAML**을 선택합니다.
1. **SAML로 Single Sign-On 설정** 페이지에서 **기본 SAML 구성**에 대한 편집(연필 모양) 아이콘을 클릭하여 설정을 편집합니다.

   ![기본 SAML 구성 편집](common/edit-urls.png)

4. **기본 SAML 구성** 섹션에서 다음 단계를 수행합니다.

    a. **로그온 URL** 텍스트 상자에서 `https://<instance-name>.service-now.com/navpage.do` 패턴을 사용하는 URL을 입력합니다.

    b. **식별자(엔터티 ID)** 텍스트 상자에서 `https://<instance-name>.service-now.com` 패턴을 사용하는 URL을 입력합니다.

    > [!NOTE]
    > 이러한 값은 실제 값이 아닙니다. 이러한 값을 자습서 뒷부분에 설명된 실제 로그온 URL 및 식별자로 업데이트해야 합니다. Azure Portal의 **기본 SAML 구성** 섹션에 표시된 패턴을 참조할 수도 있습니다.

1. **SAML로 Single Sign-On 설정** 페이지의 **SAML 서명 인증서** 섹션에서 **인증서(Base64)** 를 찾은 후 **다운로드**를 선택하여 인증서를 컴퓨터에 다운로드하고 본인의 컴퓨터에 저장합니다.

   ![인증서 다운로드 링크](common/certificatebase64.png)

   a. 복사 단추를 클릭하여 **앱 페더레이션 메타 데이터 URL**을 복사하여 메모장에 붙여넣습니다. 이 앱 페더레이션 메타데이터 URL은 자습서의 뒷부분에서 사용됩니다.

    b. **다운로드**를 클릭하여 **인증서(Base64)** 를 다운로드한 다음 컴퓨터에 인증서 파일을 저장합니다.

1. **ServiceNow 설정** 섹션에서 요구 사항에 따라 적절한 URL을 복사합니다.

   ![구성 URL 복사](common/copy-configuration-urls.png)

### <a name="configure-servicenow"></a>ServiceNow 구성

1. ServiceNow 애플리케이션에 관리자 권한으로 로그온합니다.

2. 다음 단계를 따라 **통합 - 여러 공급자 Single Sign-On 설치 관리자** 플러그 인을 활성화합니다.

    a. 왼쪽 탐색 창의 검색 창에서 **시스템 정의** 섹션을 검색한 다음 **플러그 인**을 클릭합니다.

    ![플러그 인 활성화](./media/servicenow-tutorial/tutorial_servicenow_03.png "플러그 인 활성화")

    b. **통합 - 여러 공급자 Single Sign-On 설치 관리자**를 검색합니다.

     ![플러그 인 활성화](./media/servicenow-tutorial/tutorial_servicenow_04.png "플러그 인 활성화")

    다. 플러그 인을 선택합니다. 마우스 오른쪽 단추를 클릭하고 **활성화/업그레이드**를 선택합니다.

     ![플러그 인 활성화](./media/servicenow-tutorial/tutorial_activate.png "플러그 인 활성화")

    d. **활성화** 단추를 클릭합니다.

     ![플러그 인 활성화](./media/servicenow-tutorial/tutorial_activate1.png "플러그 인 활성화")

3. 왼쪽 탐색 창의 검색 창에서 **Multi-Provider SSO** 섹션을 검색한 다음 **속성**을 클릭합니다.

    ![앱 URL 구성](./media/servicenow-tutorial/tutorial_servicenow_06.png "앱 URL 구성")

4. **여러 공급자 SSO 속성** 대화 상자에서 다음 단계를 수행합니다.

    ![앱 URL 구성](./media/servicenow-tutorial/ic7694981.png "앱 URL 구성")

    * **여러 공급자 SSO 사용**을 **예**로 선택합니다.
  
    * **모든 ID 공급자에서 사용자 테이블로 사용자 자동 가져오기 사용**에서 **예**를 선택합니다.

    * **Multi-Provider SSO 통합에 디버그 로깅 사용**에서 **예**를 선택합니다.

    * **...하는 사용자 테이블의 필드** 텍스트 상자에서 **user_name**을 입력합니다.
  
    * **저장**을 클릭합니다.

5. **ServiceNow**는 자동 및 수동의 두 가지 방식으로 구성할 수 있습니다.

6. **ServiceNow**를 자동으로 구성하려면 다음 단계를 따르세요.

    * Azure Portal의 **ServiceNow** Single-Sign On 페이지로 돌아갑니다.

    * ServiceNow에 대해 한 번 클릭으로 구성할 수 있는 서비스가 제공됩니다. 즉, Azure AD가 SAML 기반 인증을 위해 ServiceNow를 자동으로 구성합니다. 이 서비스를 사용하도록 설정하려면 **ServiceNow 구성** 섹션으로 이동한 다음 **ServiceNow 구성**을 클릭하여 로그온 구성 창을 엽니다.

        ![Configure Single Sign-On](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

    * ServiceNow 인스턴스 이름, 관리자 이름 및 관리자 암호를 **로그온 구성** 양식에 입력하고 **지금 구성**을 클릭합니다. 입력하는 관리자 사용자 이름에 ServiceNow의 **security_admin** 역할이 할당되어 있어야 이 절차를 수행할 수 있습니다. 이 방법을 사용하지 않고 SAML ID 공급자로 Azure AD를 사용하도록 ServiceNow를 수동으로 구성하려는 경우에는 **Single Sign-On 수동 구성**을 클릭하고 빠른 참조 섹션에서 **로그아웃 URL, SAML 엔터티 ID 및 SAML Single Sign-On 서비스 URL**을 복사합니다.

        ![앱 URL 구성](./media/servicenow-tutorial/configure.png "앱 URL 구성")

    * ServiceNow 애플리케이션에 관리자 권한으로 로그온합니다.

    * 자동 구성에서 필요한 모든 설정을 **ServiceNow** 쪽에 구성하지만 **X.509 인증서**를 기본적으로 사용하지 않습니다. 이 인증서는 ServiceNow의 ID 공급자에 수동으로 매핑해야 합니다. ServiceNow의 ID 공급자에 대해 아래 단계를 수행합니다.

    * 왼쪽 탐색 창의 검색 창에서 **Multi-Provider SSO** 섹션을 검색한 다음 **ID 공급자**를 클릭합니다.

        ![Single Sign-On 구성](./media/servicenow-tutorial/tutorial_servicenow_07.png "Single Sign-On 구성")

    * 자동으로 생성되는 ID 공급자를 클릭합니다.

        ![Single Sign-On 구성](./media/servicenow-tutorial/tutorial_servicenow_08.png "Single Sign-On 구성")

    *  **ID 공급자** 섹션에서 다음 단계를 수행합니다.

        ![Single Sign-On 구성](./media/servicenow-tutorial/automatic_config.png "Single Sign-On 구성")

        * **이름** 텍스트 상자에 구성의 이름을 입력합니다(예: **Microsoft Azure 페더레이션된 Single Sign-On**).

        * 텍스트 상자에 입력된 **ID 공급자의 SingleLogoutRequest** 값은 제거합니다.

        * **ServiceNow 홈 페이지** 값을 복사하여 Azure Portal의 **ServiceNow 기본 SAML 구성** 섹션에 있는 **로그온 URL** 텍스트 상자에 붙여넣습니다.

            > [!NOTE]
            > ServiceNow 인스턴스 홈 페이지의 URL은 **ServieNow 테넌트 URL**과 **/navpage.do**가 연결된 형태입니다(예: `https://fabrikam.service-now.com/navpage.do`).

        * **엔터티 ID/발급자** 값을 복사하여 Azure Portal의 **ServiceNow 기본 SAML 구성** 섹션에 있는 **식별자** 텍스트 상자에 붙여넣습니다.

        * **NameID 정책**이 `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified` 값으로 설정되어 있는지 확인합니다. 

    * 아래쪽의 **X.509 인증서** 섹션으로 스크롤하여 **편집**을 선택합니다.

        ![Single Sign-On 구성](./media/servicenow-tutorial/tutorial_servicenow_09.png "Single Sign-On 구성")

    * 인증서를 선택하고 오른쪽 화살표 아이콘을 클릭하여 인증서를 추가합니다.

        ![Single Sign-On 구성](./media/servicenow-tutorial/tutorial_servicenow_11.png "Single Sign-On 구성")

    * **저장**을 클릭합니다.

    * 페이지의 오른쪽 위에서 **연결 테스트**를 클릭합니다.

        ![플러그 인 활성화](./media/servicenow-tutorial/tutorial_activate2.png "플러그 인 활성화")

    * **연결 테스트**를 클릭하면 표시되는 팝업 창에 자격 증명을 입력해야 합니다. 그러면 결과가 포함된 아래 페이지가 표시됩니다. **SSO 로그아웃 테스트 결과** 오류는 원래 발생하는 것이므로 무시하고 **활성화** 단추를 클릭합니다.

        ![Single Sign-On 구성](./media/servicenow-tutorial/servicenowactivate.png "Single Sign-On 구성")
  
7. **ServiceNow**를 수동으로 구성하려면 다음 단계를 따르세요.

    * ServiceNow 애플리케이션에 관리자 권한으로 로그온합니다.

    * 왼쪽의 탐색 창에서 **ID 공급자**를 클릭합니다.

        ![Single Sign-On 구성](./media/servicenow-tutorial/tutorial_servicenow_07.png "Single Sign-On 구성")

    * **ID 공급자** 대화 상자에서 **새로 만들기**를 클릭합니다.

        ![Single Sign-On 구성](./media/servicenow-tutorial/ic7694977.png "Single Sign-On 구성")

    * **ID 공급자** 대화 상자에서 **SAML**을 클릭합니다.

        ![Single Sign-On 구성](./media/servicenow-tutorial/ic7694978.png "Single Sign-On 구성")

    * **ID 공급자 메타데이터 가져오기** 팝업에서 다음 단계를 수행합니다.

        ![Single Sign-On 구성](./media/servicenow-tutorial/idp.png "Single Sign-On 구성")

        * Azure Portal에서 복사한 **앱 페더레이션 메타데이터 URL** 값을 입력합니다.

        * **가져오기**를 클릭합니다.

    * 그러면 IdP 메타데이터 URL을 읽어와서 모든 필드 정보가 채워집니다.

        ![Single Sign-On 구성](./media/servicenow-tutorial/ic7694982.png "Single Sign-On 구성")

        * **이름** 텍스트 상자에 구성의 이름을 입력합니다(예: **Microsoft Azure 페더레이션된 Single Sign-On**).

        * 텍스트 상자에 입력된 **ID 공급자의 SingleLogoutRequest** 값은 제거합니다.

        * **ServiceNow 홈 페이지** 값을 복사하여 Azure Portal의 **ServiceNow 기본 SAML 구성** 섹션에 있는 **로그온 URL** 텍스트 상자에 붙여넣습니다.

            > [!NOTE]
            > ServiceNow 인스턴스 홈 페이지의 URL은 **ServieNow 테넌트 URL**과 **/navpage.do**가 연결된 형태입니다(예: `https://fabrikam.service-now.com/navpage.do`).

        * **엔터티 ID/발급자** 값을 복사하여 Azure Portal의 **ServiceNow 기본 SAML 구성** 섹션에 있는 **식별자** 텍스트 상자에 붙여넣습니다.

        * **NameID 정책**이 `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified` 값으로 설정되어 있는지 확인합니다.

        * **고급**을 클릭합니다. ServiceNow 배포에서 사용자를 고유하게 식별하는 데 사용되는 필드에 따라 **사용자 필드** 텍스트 상자에 **email** 또는 **user_name**을 입력합니다.

            > [!NOTE]
            > Azure Portal의 **ServiceNow > 특성 > Single Sign-On** 섹션으로 이동한 다음 원하는 필드를 **nameidentifier** 특성에 매핑하면 Azure AD 사용자 ID(사용자 계정 이름) 또는 전자 메일 주소를 SAML 토큰의 고유 식별자로 내보내도록 Azure AD를 구성할 수 있습니다. Azure AD에서 선택한 특성에 대해 저장되는 값(예: 사용자 계정 이름)은 입력하는 필드에 대해 ServiceNow에 저장된 값(예: user_name)과 일치해야 합니다.

        * 페이지의 오른쪽 위에서 **연결 테스트**를 클릭합니다.

        * **연결 테스트**를 클릭하면 표시되는 팝업 창에 자격 증명을 입력해야 합니다. 그러면 결과가 포함된 아래 페이지가 표시됩니다. **SSO 로그아웃 테스트 결과** 오류는 원래 발생하는 것이므로 무시하고 **활성화** 단추를 클릭합니다.

          ![Single Sign-On 구성](./media/servicenow-tutorial/servicenowactivate.png "Single Sign-On 구성")

### <a name="configure-azure-ad-sso-for-servicenow-express"></a>ServiceNow Express에 대해 Azure AD SSO 구성

1. [Azure Portal](https://portal.azure.com/)의 **ServiceNow** 애플리케이션 통합 페이지에서 **Single Sign-On**을 선택합니다.

    ![Single Sign-On 구성 링크](common/select-sso.png)

2. **Single Sign-On 방법 선택** 대화 상자에서 **SAML/WS-Fed** 모드를 선택하여 Single Sign-On을 사용하도록 설정합니다.

    ![Single Sign-On 선택 모드](common/select-saml-option.png)

3. **SAML로 Single Sign-On 설정** 페이지에서 **편집** 아이콘을 클릭하여 **기본 SAML 구성** 대화 상자를 엽니다.

    ![기본 SAML 구성 편집](common/edit-urls.png)

4. **기본 SAML 구성** 섹션에서 다음 단계를 수행합니다.

    a. **로그온 URL** 텍스트 상자에서 `https://<instance-name>.service-now.com/navpage.do` 패턴을 사용하는 URL을 입력합니다.

    b. **식별자(엔터티 ID)** 텍스트 상자에서 `https://<instance-name>.service-now.com` 패턴을 사용하는 URL을 입력합니다.

    > [!NOTE]
    > 이러한 값은 실제 값이 아닙니다. 이러한 값을 자습서 뒷부분에 설명된 실제 로그온 URL 및 식별자로 업데이트해야 합니다. Azure Portal의 **기본 SAML 구성** 섹션에 표시된 패턴을 참조할 수도 있습니다.

5. **SAML로 Single Sign-On 설정** 페이지의 **SAML 서명 인증서** 섹션에서 **다운로드**를 클릭하여 요구 사항에 따라 제공된 옵션에서 **인증서(Base64)** 를 다운로드한 다음, 컴퓨터에 저장합니다.

    ![인증서 다운로드 링크](common/certificatebase64.png)

6. ServiceNow에 대해 한 번 클릭으로 구성할 수 있는 서비스가 제공됩니다. 즉, Azure AD가 SAML 기반 인증을 위해 ServiceNow를 자동으로 구성합니다. 이 서비스를 사용하도록 설정하려면 **ServiceNow 설정** 섹션으로 이동한 다음 **단계별 지침 보기**를 클릭하여 로그온 구성 창을 엽니다.

    ![Configure Single Sign-On](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

7. ServiceNow 인스턴스 이름, 관리자 이름 및 관리자 암호를 **로그온 구성** 양식에 입력하고 **지금 구성**을 클릭합니다. 입력하는 관리자 사용자 이름에 ServiceNow의 **security_admin** 역할이 할당되어 있어야 이 절차를 수행할 수 있습니다. 이 방법을 사용하지 않고 SAML ID 공급자로 Azure AD를 사용하도록 ServiceNow를 수동으로 구성하려는 경우에는 **Single Sign-On 수동 구성**을 클릭하고 빠른 참조 섹션에서 **로그아웃 URL, Azure AD 식별자 및 로그인 URL**을 복사합니다.

    ![앱 URL 구성](./media/servicenow-tutorial/configure.png "앱 URL 구성")

### <a name="configure-servicenow-express-sso"></a>ServiceNow Express SSO 구성

1. ServiceNow Express 애플리케이션에 관리자 권한으로 로그온합니다.

2. 왼쪽 탐색 창에서 **Single Sign-On**을 클릭합니다.

    ![앱 URL 구성](./media/servicenow-tutorial/ic7694980ex.png "앱 URL 구성")

3. **Single Sign-on** 대화 상자에서 오른쪽 위의 구성 아이콘을 클릭하고 다음 속성을 설정합니다.

    ![앱 URL 구성](./media/servicenow-tutorial/ic7694981ex.png "앱 URL 구성")

    a. **여러 공급자 SSO 사용**을 오른쪽으로 설정/해제합니다.

    b. **여러 공급자 SSO 통합에 대한 디버그 로깅 활성화**를 오른쪽으로 설정/해제합니다.

    다. **...하는 사용자 테이블의 필드** 텍스트 상자에서 **user_name**을 입력합니다.

4. **Single Sign-On** 대화 상자에서 **새 인증서 추가**를 클릭합니다.

    ![Single Sign-On 구성](./media/servicenow-tutorial/ic7694973ex.png "Single Sign-On 구성")

5. **X.509 Certificates** 대화 상자에서 다음 단계를 수행합니다.

    ![Single Sign-On 구성](./media/servicenow-tutorial/ic7694975.png "Single Sign-On 구성")

    a. **이름** 텍스트 상자에 구성에 사용할 이름을 입력합니다(예: **TestSAML2.0**).

    b. **활성**을 선택합니다.

    다. **형식**으로 **PEM**을 선택합니다.

    d. **형식**으로 **저장소 인증서 신뢰**를 선택합니다.

    e. Azure Portal에서 다운로드한 Base64로 인코딩된 인증서를 메모장에서 열고, 내용을 클립보드에 복사한 다음 **PEM 인증서** 텍스트 상자에 붙여넣습니다.

    f. **업데이트**를 클릭합니다.

6. **Single Sign-On** 대화 상자에서 **새 IdP 추가**를 클릭합니다.

    ![Single Sign-On 구성](./media/servicenow-tutorial/ic7694976ex.png "Single Sign-On 구성")

7. **새 ID 공급자 추가** 대화 상자의 **ID 공급자 구성** 아래에서 다음 단계를 수행합니다.

    ![Single Sign-On 구성](./media/servicenow-tutorial/ic7694982ex.png "Single Sign-On 구성")

    a. **이름** 텍스트 상자에 구성에 사용할 이름을 입력합니다(예: **SAML 2.0**).

    b. **ID 공급자 URL** 필드에 Azure Portal에서 복사한 **ID 공급자 ID** 값을 붙여넣습니다.

    다. **ID 공급자 AuthnRequest** 필드에 Azure Portal에서 복사한 **인증 요청 URL** 값을 붙여넣습니다.

    d. Azure Portal에서 복사한 **로그아웃 URL** 값을 **ID 공급자의 SingleLogoutRequest** 필드에 붙여넣습니다.

    e. **ID 공급자 인증서**로 이전 단계에서 만든 인증서를 선택합니다.

8. **고급 설정**을 클릭하고 **추가 ID 공급자 속성** 아래에서 다음 단계를 수행합니다.

    ![Single Sign-On 구성](./media/servicenow-tutorial/ic7694983ex.png "Single Sign-On 구성")

    a. **IDP의 SingleLogoutRequest에 대한 프로토콜 바인딩** 텍스트 상자에 **urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect**를 입력합니다.

    b. **NameID 정책** 텍스트 상자에 **urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified**를 입력합니다.

    다. **AuthnContextClassRef 메서드**에 `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`를 입력합니다.

    d. **AuthnContextClass 만들기**의 선택을 취소합니다.

9. **추가 서비스 공급자 속성** 아래에서 다음 단계를 수행합니다.

    ![Single Sign-On 구성](./media/servicenow-tutorial/ic7694984ex.png "Single Sign-On 구성")

    a. **ServiceNow 홈페이지** 텍스트 상자에 ServiceNow 인스턴스 홈페이지의 URL을 입력합니다.

    > [!NOTE]
    > ServiceNow 인스턴스 홈 페이지의 URL은 **ServieNow 테넌트 URL**과 **/navpage.do**가 연결된 형태입니다(예: `https://fabrikam.service-now.com/navpage.do`).

    b. **엔터티 ID/발급자** 텍스트 상자에 ServiceNow 테넌트의 URL을 입력합니다.

    다. **대상 URI** 텍스트 상자에 ServiceNow 테넌트의 URL을 입력합니다.

    d. **시계 기울이기** 텍스트 상자에 **60**을 입력합니다.

    e. ServiceNow 배포에서 사용자를 고유하게 식별하는 데 사용되는 필드에 따라 **사용자 필드** 텍스트 상자에 **email** 또는 **user_name**을 입력합니다.

    > [!NOTE]
    > Azure Portal의 **ServiceNow > 특성 > Single Sign-On** 섹션으로 이동한 다음 원하는 필드를 **nameidentifier** 특성에 매핑하면 Azure AD 사용자 ID(사용자 계정 이름) 또는 전자 메일 주소를 SAML 토큰의 고유 식별자로 내보내도록 Azure AD를 구성할 수 있습니다. Azure AD에서 선택한 특성에 대해 저장되는 값(예: 사용자 계정 이름)은 입력하는 필드에 대해 ServiceNow에 저장된 값(예: user_name)과 일치해야 합니다.

    f. **저장**을 클릭합니다.

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

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 B.Simon에게 ServiceNow에 대한 액세스 권한을 부여합니다.

1. Azure Portal에서 **엔터프라이즈 애플리케이션**을 선택한 다음, **모든 애플리케이션**을 선택합니다.
1. 애플리케이션 목록에서 **ServiceNow**를 선택합니다.
1. 앱의 개요 페이지에서 **관리** 섹션을 찾고 **사용자 및 그룹**을 선택합니다.

   !["사용자 및 그룹" 링크](common/users-groups-blade.png)

1. **사용자 추가**를 선택한 다음, **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![사용자 추가 링크](common/add-assign-user.png)

1. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **B.Simon**을 선택한 다음, 화면 아래쪽에서 **선택** 단추를 클릭합니다.
1. SAML 어설션에 역할 값이 필요한 경우 **역할 선택** 대화 상자의 목록에서 사용자에 대한 적절한 역할을 선택한 다음, 화면의 아래쪽에 있는 **선택** 단추를 클릭합니다.
1. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.

### <a name="create-servicenow-test-user"></a>ServiceNow 테스트 사용자 만들기

이 섹션은 ServiceNow에서 Britta Simon이라는 사용자를 만들기 위한 것입니다. ServiceNow는 자동 사용자 프로비전을 지원하며 기본적으로 사용하도록 설정되어 있습니다. 자동 사용자 프로비전 구성 방법에 대한 자세한 내용은 [여기](servicenow-provisioning-tutorial.md)에서 제공합니다.

> [!NOTE]
> 사용자를 수동으로 생성해야 하는 경우 [ServiceNow 클라이언트 지원 팀](https://www.servicenow.com/support/contact-support.html)에 문의해야 합니다.

### <a name="test-sso"></a>SSO 테스트

액세스 패널에서 ServiceNow 타일을 선택하면 SSO를 설정한 ServiceNow에 자동으로 로그인되어야 합니다. 액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)를 참조하세요.

## <a name="test-sso-for-servicenow-classic-mobile"></a>ServiceNow Classic(모바일)에 대한 SSO 테스트

1. **ServiceNow Classic(모바일)** 애플리케이션을 열고 다음 단계를 수행합니다.

    a. 아래 화면의 **추가** 기호를 클릭합니다.

    ![로그인](./media/servicenow-tutorial/test03.png)

    b. ServiceNow 인스턴스 이름을 입력하고 **계속**을 클릭합니다.

    ![로그인](./media/servicenow-tutorial/test04.png)

    다. **로그인** 화면에서 다음 단계를 수행합니다.

    ![로그인](./media/servicenow-tutorial/test01.png)

    *  **사용자 이름**을 입력합니다(예: B.simon@contoso.com).

    *  **외부 로그인 사용**을 클릭합니다. 그러면 로그인할 수 있는 Azure AD 페이지로 리디렉션됩니다.
    
    *  자격 증명을 입력합니다. 타사 인증 또는 다른 보안 기능이 설정된 경우 사용자가 적절하게 대답해야 하며, 적절하게 대답하면 아래처럼 애플리케이션 **홈페이지**가 표시됩니다.

        ![홈페이지](./media/servicenow-tutorial/test02.png)

## <a name="additional-resources"></a>추가 리소스

- [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory로 애플리케이션 액세스 및 Single Sign-On을 구현하는 방법](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory의 조건부 액세스란?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [사용자 프로비저닝 구성](servicenow-provisioning-tutorial.md)