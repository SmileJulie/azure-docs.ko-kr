---
title: '자습서: SAP Business Object Cloud와 Azure Active Directory 통합 | Microsoft Docs'
description: Azure Active Directory 및 SAP Business Object Cloud 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 6c5e44f0-4e52-463f-b879-834d80a55cdf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/31/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ad2ffddf96aa6ecc886ac5653d2d0b8dcfb0856
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67091708"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-object-cloud"></a>자습서: SAP Business Object Cloud와 Azure Active Directory 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 SAP Business Object Cloud를 통합하는 방법에 대해 알아봅니다.
SAP Business Object Cloud를 Azure AD와 통합하면 다음과 같은 이점이 있습니다.

* SAP Business Object Cloud에 대한 액세스 권한이 있는 사용자를 Azure AD에서 제어할 수 있습니다.
* 사용자가 해당 Azure AD 계정으로 SAP Business Object Cloud에 자동으로 로그인(Single Sign-On)되도록 설정할 수 있습니다.
* 단일 중앙 위치인 Azure Portal에서 계정을 관리할 수 있습니다.

Azure AD와의 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory를 사용한 응용 프로그램 액세스 및 Single Sign-On](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)을 참조하세요.
Azure 구독이 아직 없는 경우 시작하기 전에 [체험](https://azure.microsoft.com/free/) 계정을 만듭니다.

## <a name="prerequisites"></a>필수 조건

SAP Business Object Cloud와 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

* Azure AD 구독 Azure AD 환경이 없으면 [여기](https://azure.microsoft.com/pricing/free-trial/)에서 1개월 평가판을 구할 수 있습니다.
* SAP Business Object Cloud Single Sign-On을 사용하도록 설정된 구독

## <a name="scenario-description"></a>시나리오 설명

이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

* SAP Business Object Cloud에서 **SP** 시작 SSO를 지원합니다.

## <a name="adding-sap-business-object-cloud-from-the-gallery"></a>갤러리에서 SAP Business Object Cloud 추가

SAP Business Object Cloud의 Azure AD 통합을 구성하려면 갤러리의 SAP Business Object Cloud를 관리되는 SaaS 앱 목록에 추가해야 합니다.

**갤러리에서 SAP Business Object Cloud를 추가하려면 다음 단계를 수행합니다.**

1. **[Azure Portal](https://portal.azure.com)** 의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다.

    ![Azure Active Directory 단추](common/select-azuread.png)

2. **엔터프라이즈 응용 프로그램**으로 이동한 다음, **모든 응용 프로그램** 옵션을 선택합니다.

    ![엔터프라이즈 애플리케이션 블레이드](common/enterprise-applications.png)

3. 새 애플리케이션을 추가하려면 대화 상자 맨 위 있는 **새 애플리케이션** 단추를 클릭합니다.

    ![새 애플리케이션 단추](common/add-new-app.png)

4. 검색 상자에서 **SAP Business Object Cloud**를 입력하고, 결과 패널에서 **SAP Business Object Cloud**를 선택한 다음, **추가** 단추를 클릭하여 애플리케이션을 추가합니다.

     ![결과 목록의 SAP Business Object Cloud](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성 및 테스트

이 섹션에서는 **Britta Simon**이라는 테스트 사용자를 기반으로 하여 SAP Business Object Cloud에서 Azure AD Single Sign-On을 구성하고 테스트합니다.
Single Sign-On이 작동하려면 Azure AD 사용자와 SAP Business Object Cloud의 관련 사용자 간에 연결 관계를 설정해야 합니다.

SAP Business Object Cloud에서 Azure AD Single Sign-On을 구성하고 테스트하려면 다음 구성 요소를 완료해야 합니다.

1. **[Azure AD Single Sign-On 구성](#configure-azure-ad-single-sign-on)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
2. **[SAP Business Object Cloud Single Sign-On 구성](#configure-sap-business-object-cloud-single-sign-on)** - 애플리케이션 쪽에서 Single Sign-On 설정을 구성합니다.
3. **[Azure AD 테스트 사용자 만들기](#create-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On을 테스트하는 데 사용합니다.
4. **[Azure AD 테스트 사용자 할당](#assign-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
5. **[SAP Business Object Cloud 테스트 사용자 만들기](#create-sap-business-object-cloud-test-user)** - Azure AD 표현과 연결된 Britta Simon에 해당하는 사용자를 SAP Business Object Cloud에 만듭니다.
6. **[Single Sign-On 테스트](#test-single-sign-on)** - 구성이 작동하는지 여부를 확인합니다.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure Portal에서 Azure AD Single Sign-On을 사용하도록 설정합니다.

SAP Business Object Cloud에서 Azure AD Single Sign-On을 구성하려면 다음 단계를 수행합니다.

1. [Azure Portal](https://portal.azure.com/)의 **SAP Business Object Cloud** 애플리케이션 통합 페이지에서 **Single Sign-On**을 선택합니다.

    ![Single Sign-On 구성 링크](common/select-sso.png)

2. **Single Sign-On 방법 선택** 대화 상자에서 **SAML/WS-Fed** 모드를 선택하여 Single Sign-On을 사용하도록 설정합니다.

    ![Single Sign-On 선택 모드](common/select-saml-option.png)

3. **SAML로 Single Sign-On 설정** 페이지에서 **편집** 아이콘을 클릭하여 **기본 SAML 구성** 대화 상자를 엽니다.

    ![기본 SAML 구성 편집](common/edit-urls.png)

4. **기본 SAML 구성** 섹션에서 다음 단계를 수행합니다.

    ![SAP Business Object Cloud 도메인 및 URL Single Sign-On 정보](common/sp-identifier.png)

    a. **로그온 URL** 텍스트 상자에 다음 패턴을 사용하여 URL을 입력합니다.

    | |
    |-|-|
    | `https://<sub-domain>.sapanalytics.cloud/` |
    | `https://<sub-domain>.sapbusinessobjects.cloud/` |

    b. **식별자(엔터티 ID)** 텍스트 상자에 다음 패턴을 사용하여 URL을 입력합니다.
    
    | |
    |-|-|
    | `<sub-domain>.sapbusinessobjects.cloud` |
    | `<sub-domain>.sapanalytics.cloud` |

    > [!NOTE] 
    > 이러한 URL의 값은 데모용으로만 사용합니다. 실제 로그온 URL 및 식별자 URL로 값을 업데이트합니다. 로그온 URL을 가져오려면 [SAP Business Object Cloud 클라이언트 지원팀](https://help.sap.com/viewer/product/SAP_BusinessObjects_Cloud/release/)에 연락하세요. 관리 콘솔에서 SAP Business Object Cloud 메타데이터를 다운로드하여 식별자 URL을 가져올 수 있습니다. 이것은 자습서의 뒷부분에 설명되어 있습니다.

4. **SAML로 Single Sign-On 설정** 페이지의 **SAML 서명 인증서** 섹션에서 **다운로드**를 클릭하여 요구 사항에 따라 제공된 옵션에서 **페더레이션 메타데이터 XML**을 다운로드하고 컴퓨터에 저장합니다.

    ![인증서 다운로드 링크](common/metadataxml.png)

### <a name="configure-sap-business-object-cloud-single-sign-on"></a>SAP Business Object Cloud Single Sign-On 구성

1. 다른 웹 브라우저 창에서 SAP Business Object Cloud 회사 사이트에 관리자로 로그인합니다.

2. **메뉴** > **시스템** > **관리**를 선택합니다.
    
    ![메뉴, 시스템 및 관리 선택](./media/sapboc-tutorial/config1.png)

3. **보안** 탭에서 **편집**(펜) 아이콘을 선택합니다.
    
    ![보안 탭에서 편집 아이콘 선택](./media/sapboc-tutorial/config2.png)  

4. **인증 방법**으로 **SAML SSO(Single Sign-On)** 를 선택합니다.

    ![인증 방법으로 SAML Single Sign-On 선택](./media/sapboc-tutorial/config3.png)  

5. 서비스 공급자 메타데이터를 다운로드하려면(1단계) **다운로드**를 선택합니다. 메타데이터 파일에서 **entityID** 값을 찾고 복사합니다. Azure Portal의 **기본 SAML 구성** 대화 상자에서 값을 **식별자** 상자에 붙여넣습니다.

    ![entityID 값 복사 및 붙여넣기](./media/sapboc-tutorial/config4.png)  

6. Azure Portal에서 다운로드한 파일에서 서비스 공급자 메타데이터를 업로드하려면(2단계) **ID 공급자 메타데이터 업로드** 아래에서 **업로드**를 선택합니다.  

    ![ID 공급자 메타데이터 업로드 아래에서 업로드 선택](./media/sapboc-tutorial/config5.png)

7. **사용자 특성** 목록에서 구현에 사용할 사용자 특성을 선택합니다(3단계). 이 사용자 특성은 ID 공급자에 매핑됩니다. 사용자의 페이지에서 사용자 지정 특성 입력하려면 **사용자 지정 SAML 매핑** 옵션을 사용합니다. 또는 **전자 메일**이나 **사용자 ID**를 사용자 특성으로 선택할 수 있습니다. 이 예에서는 Azure Portal의 **사용자 특성 및 클레임** 섹션에서 사용자 ID 클레임을 **userprincipalname** 특성으로 매핑하므로 **이메일**을 선택했습니다. 여기서는 고유한 사용자 전자 메일을 제공합니다. 모든 성공적인 SAML 응답에서 SAP Business Object Cloud 애플리케이션에 전송됩니다.

    ![사용자 특성 선택](./media/sapboc-tutorial/config6.png)

8. ID 공급자(4단계)를 사용하여 계정을 확인하려면 **로그인 자격 증명(전자 메일)** 상자에 사용자의 전자 메일 주소를 입력합니다. 그런 다음 **계정 확인**을 선택합니다. 시스템은 사용자 계정에 로그인 자격 증명을 추가합니다.

    ![전자 메일을 입력하고 계정 확인을 선택합니다.](./media/sapboc-tutorial/config7.png)

9. **저장** 아이콘을 선택합니다.

    ![저장 아이콘](./media/sapboc-tutorial/save.png)

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

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 Britta Simon에게 SAP Business Object Cloud에 대한 액세스 권한을 부여합니다.

1. Azure Portal에서 **엔터프라이즈 애플리케이션**, **모든 애플리케이션**, **SAP Business Object Cloud**를 차례로 선택합니다.

    ![엔터프라이즈 애플리케이션 블레이드](common/enterprise-applications.png)

2. 애플리케이션 목록에서 **SAP Business Object Cloud**을 선택합니다.

    ![애플리케이션 목록의 SAP Business Object Cloud 링크](common/all-applications.png)

3. 왼쪽 메뉴에서 **사용자 및 그룹**을 선택합니다.

    !["사용자 및 그룹" 링크](common/users-groups-blade.png)

4. **사용자 추가** 단추를 클릭한 다음, **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![할당 추가 창](common/add-assign-user.png)

5. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택하고 화면 아래쪽에서 **선택** 단추를 클릭합니다.

6. SAML 어설션 및 **역할 선택** 대화 상자에서 모든 역할 값이 필요한 경우 목록에서 적절한 사용자 역할을 선택한 다음, 화면 맨 아래에 있는 **선택** 단추를 클릭합니다.

7. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.

### <a name="create-sap-business-object-cloud-test-user"></a>SAP Business Object Cloud 테스트 사용자 만들기

Azure AD 사용자는 SAP Business Object Cloud에 로그인하기 전에 SAP Business Object Cloud에 프로비전되어야 합니다. SAP Business Object Cloud에서 프로비전은 수동 작업입니다.

사용자 계정을 프로비전하려면:

1. SAP Business Object Cloud 회사 사이트에 관리자 권한으로 로그인합니다.

2. **메뉴** > **보안** > **사용자**를 선택합니다.

    ![직원 추가](./media/sapboc-tutorial/user1.png)

3. **사용자** 페이지에서 새 사용자 세부 정보를 추가하려면 **+** 를 선택합니다. 

    ![사용자 추가 페이지](./media/sapboc-tutorial/user4.png)

    그리고 나서 다음 단계를 완료합니다.

    a. **사용자 ID** 상자에 사용자 ID(예: **Britta**)를 입력합니다.

    b. **이름** 텍스트 상자에 사용자의 이름(예: **Britta**)을 입력합니다.

    다. **성** 텍스트 상자에 사용자의 성(예: **Simon**)을 입력합니다.

    d. **표시 이름** 텍스트 상자에 사용자의 전체 이름(예: **Britta Simon**)을 입력합니다.

    e. **이메일** 상자에 **brittasimon\@contoso.com**과 같은 사용자의 이메일 주소를 입력합니다.

    f. **역할 선택** 페이지에서 사용자에 대한 적절한 역할을 선택하고 **확인**을 선택합니다.

      ![역할 선택](./media/sapboc-tutorial/user3.png)

    g. **저장** 아이콘을 선택합니다.    

### <a name="test-single-sign-on"></a>Single Sign-On 테스트 

이 섹션에서는 액세스 패널을 사용하여 Azure AD Single Sign-On 구성을 테스트합니다.

액세스 패널에서 SAP Business Object Cloud 타일을 클릭하면 SSO를 설정한 SAP Business Object Cloud에 자동으로 로그인됩니다. 액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)를 참조하세요.

## <a name="additional-resources"></a>추가 리소스

- [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory로 애플리케이션 액세스 및 Single Sign-On을 구현하는 방법](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory의 조건부 액세스란?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

