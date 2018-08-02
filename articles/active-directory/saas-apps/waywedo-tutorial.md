---
title: '자습서: Way We Do와 Azure Active Directory 통합 | Microsoft Docs'
description: Azure Active Directory 및 Way We Do 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 84fc4f36-ecd1-42c6-8a70-cb0f3dc15655
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2018
ms.author: jeedes
ms.openlocfilehash: bc415ec7c577e221a1ab5af585dff5b4fc9ab7dc
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39259593"
---
# <a name="tutorial-azure-active-directory-integration-with-way-we-do"></a>자습서: Way We Do와 Azure Active Directory 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 Way We Do를 통합하는 방법에 대해 알아봅니다.

Way We Do를 Azure AD와 통합하면 다음과 같은 이점이 제공됩니다.

- Way We Do에 대한 액세스 권한이 있는 사용자를 Azure AD에서 제어할 수 있습니다.
- 사용자가 해당 Azure AD 계정으로 Way We Do에 자동으로 로그온(Single Sign-on)되도록 설정할 수 있습니다.
- 단일 중앙 위치인 Azure Portal에서 계정을 관리할 수 있습니다.

Azure AD와 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory의 응용 프로그램 액세스 및 Single Sign-On이란 무엇인가요?](../manage-apps/what-is-single-sign-on.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

Way We Do와 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

- Azure AD 구독
- Way We Do Single Sign-on이 설정된 구독

> [!NOTE]
> 이 자습서의 단계를 테스트하기 위해 프로덕션 환경을 사용하는 것은 바람직하지 않습니다.

이 자습서의 단계를 테스트하려면 다음 권장 사항을 준수해야 합니다.

- 꼭 필요한 경우가 아니면 프로덕션 환경을 사용하지 마세요.
- Azure AD 평가판 환경이 없으면 [1개월 평가판을 얻을](https://azure.microsoft.com/pricing/free-trial/) 수 있습니다.

## <a name="scenario-description"></a>시나리오 설명
이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 테스트 합니다. 이 자습서에 설명된 시나리오는 다음 두 가지 주요 구성 요소로 이루어져 있습니다.

1. 갤러리에서 Way We Do 추가
2. Azure AD Single Sign-on 구성 및 테스트

## <a name="adding-way-we-do-from-the-gallery"></a>갤러리에서 Way We Do 추가
Way We Do의 Azure AD 통합을 구성하려면 갤러리의 Way We Do를 관리되는 SaaS 앱 목록에 추가해야 합니다.

**갤러리에서 Way We Do를 추가하려면 다음 단계를 수행합니다.**

1. **[Azure Portal](https://portal.azure.com)** 의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다. 

    ![Azure Active Directory 단추][1]

2. **엔터프라이즈 응용 프로그램**으로 이동합니다. 그런 후 **모든 응용 프로그램**으로 이동합니다.

    ![엔터프라이즈 응용 프로그램 블레이드][2]
    
3. 새 응용 프로그램을 추가하려면 대화 상자 맨 위 있는 **새 응용 프로그램** 단추를 클릭합니다.

    ![새 응용 프로그램 단추][3]

4. 검색 상자에 **Way We Do**를 입력하고 결과 패널에서 **Way We Do**를 선택한 다음, **추가** 단추를 클릭하여 응용 프로그램을 추가합니다.

    ![결과 목록의 Way We Do](./media/waywedo-tutorial/tutorial_waywedo_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성 및 테스트

이 섹션에서는 "Britta Simon"이라는 테스트 사용자를 기반으로 Way We Do에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

Single Sign-On이 작동하려면 Azure AD에서 Azure AD 사용자에 해당하는 Way We Do 사용자가 누구인지 알고 있어야 합니다. 즉, Azure AD 사용자와 Way We Do의 관련 사용자 간에 연결이 형성되어야 합니다.

Way We Do에서 Azure AD Single Sign-On을 구성하고 테스트하려면 다음 구성 요소를 완료해야 합니다.

1. **[Azure AD Single Sign-On 구성](#configure-azure-ad-single-sign-on)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
2. **[Azure AD 테스트 사용자 만들기](#create-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On을 테스트하는 데 사용합니다.
3. **[Way We Do 테스트 사용자 만들기](#create-a-way-we-do-test-user)** - Britta Simon의 Azure AD 표현과 연결된 해당 사용자를 Way We Do에 만듭니다.
4. **[Azure AD 테스트 사용자 할당](#assign-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
5. **[Single Sign-On 테스트](#test-single-sign-on)** - 구성이 작동하는지 여부를 확인합니다.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure Portal에서 Azure AD Single Sign-On을 사용하도록 설정하고 Way We Do 응용 프로그램에서 Single Sign-On을 구성합니다.

**Way We Do에서 Azure AD Single Sign-on을 구성하려면 다음 단계를 수행합니다.**

1. Azure Portal의 **Way We Do** 응용 프로그램 통합 페이지에서 **Single Sign-On**을 클릭합니다.

    ![Single Sign-On 구성 링크][4]

2. **Single Sign-On** 대화 상자에서 **모드**를 **SAML 기반 로그온**으로 선택하여 Single Sign-On을 사용하도록 설정합니다.
 
    ![Single Sign-On 대화 상자](./media/waywedo-tutorial/tutorial_waywedo_samlbase.png)

3. **Way We Do 도메인 및 URL** 섹션에서 다음 단계를 수행합니다.

    ![Way We Do 도메인 및 URL Single Sign-On 정보](./media/waywedo-tutorial/tutorial_waywedo_url.png)

    a. **로그온 URL** 텍스트 상자에서 다음 패턴으로 URL을 입력합니다. `https://<SUBDOMAIN>.waywedo.com/Authentication/ExternalSignIn`

    나. **식별자** 텍스트 상자에서 `https://<SUBDOMAIN>.waywedo.com` 패턴을 사용하여 URL을 입력합니다.

    > [!NOTE] 
    > 이러한 값은 실제 값이 아닙니다. 실제 로그온 URL 및 식별자로 값을 업데이트합니다. 이러한 값을 얻으려면 [Way We Do 클라이언트 지원 팀](mailto:support@waywedo.com)에 문의하세요. 
 
4. **SAML 서명 인증서** 섹션에서 **인증서(원시)** 를 클릭한 후 컴퓨터에 인증서 파일을 저장합니다.

    ![인증서 다운로드 링크](./media/waywedo-tutorial/tutorial_waywedo_certificate.png) 

5. **저장** 단추를 클릭합니다.

    ![Single Sign-On 구성 저장 단추](./media/waywedo-tutorial/tutorial_general_400.png)

6. **Way We Do 구성** 섹션에서 **Way We Do 구성**을 클릭하여 **로그온 구성** 창을 엽니다. **빠른 참조 섹션**에서 **SAML 엔터티 ID 및 SAML Single Sign-On 서비스 URL**을 복사합니다.

    ![Way We Do 구성](./media/waywedo-tutorial/tutorial_waywedo_configure.png) 

7. 다른 웹 브라우저 창에서 Way We Do에 보안 관리자로 로그인합니다.

8. Way We Do 페이지의 오른쪽 위 모서리에서 **사용자 아이콘**을 클릭한 다음, 드롭다운 메뉴에서 **계정**을 클릭합니다.

    ![Way We Do 계정](./media/waywedo-tutorial/tutorial_waywedo_account.png) 

9. **메뉴 아이콘**을 클릭하여 푸시 탐색 메뉴를 열고 **Single Sign On**을 클릭합니다.

    ![Way We Do 단일](./media/waywedo-tutorial/tutorial_waywedo_single.png)

10. **Single Sign-On 설정** 페이지에서 다음 단계를 수행합니다.

    ![Way We Do 저장](./media/waywedo-tutorial/tutorial_waywedo_save.png)

    a. **Single Sign-On 사용** 토글을 **예**로 클릭하여 Single Sign-On을 사용하도록 설정합니다.

    나. **Single Sign-On 이름** 텍스트 상자에 이름을 입력합니다.

    다. Azure Portal에서 복사한 **SAML 엔터티 ID** 값을 **엔터티 ID** 텍스트 상자에 붙여넣습니다.

    d. Azure Portal에서 복사한 **SAML Single Sign-On 서비스 URL** 값을 **SAML SSO URL** 텍스트 상자에 붙여넣습니다.

    e. **인증서** 옆에 있는 **선택** 단추를 클릭하여 인증서를 업로드합니다.

    f. **선택적 설정** -
    
    * 암호 사용 - 이 옵션을 사용하지 않도록 설정하면 사용자가 Single Sign-On만을 사용할 수 있도록 Way We Do에 대한 일반 암호가 작동합니다.

    * 자동 프로비전 사용 - 이 옵션을 사용하도록 설정하면 로그온에 사용된 이메일 주소가 Way We Do의 사용자 목록과 자동으로 비교됩니다. 이메일 주소가 Way We Do에 활성 사용자와 일치하지 않으면 로그인하는 사용자에 대한 새 사용자 계정을 자동으로 추가하여 누락된 정보를 요청합니다.

      > [!NOTE]
      > Single Sign-On을 통해 추가된 사용자는 일반 사용자로 추가되고 시스템에서 역할이 할당되지 않습니다. 관리자는 편집자 또는 관리자 권한으로 해당 보안 역할에 들어가서 수정할 수 있고 하나 또는 여러 개의 조직 자트 역할을 할당할 수도 있습니다. 

    g. **저장**을 클릭하여 설정을 유지합니다.

### <a name="create-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기

이 섹션의 목적은 Azure Portal에서 Britta Simon이라는 테스트 사용자를 만드는 것입니다.

   ![Azure AD 테스트 사용자 만들기][100]

**Azure AD에서 테스트 사용자를 만들려면 다음 단계를 수행하세요.**

1. Azure Portal의 왼쪽 창에서 **Azure Active Directory** 단추를 클릭합니다.

    ![Azure Active Directory 단추](./media/waywedo-tutorial/create_aaduser_01.png)

2. 사용자 목록을 표시하려면 **사용자 및 그룹**으로 이동한 후 **모든 사용자**를 클릭합니다.

    !["사용자 및 그룹" 및 "모든 사용자" 링크](./media/waywedo-tutorial/create_aaduser_02.png)

3. **사용자** 대화 상자를 열려면 **모든 사용자** 대화 상자 위쪽에서 **추가**를 클릭합니다.

    ![추가 단추](./media/waywedo-tutorial/create_aaduser_03.png)

4. **사용자** 대화 상자에서 다음 단계를 수행합니다.

    ![사용자 대화 상자](./media/waywedo-tutorial/create_aaduser_04.png)

    a. **이름** 상자에 **BrittaSimon**을 입력합니다.

    나. **사용자 이름** 상자에 사용자인 Britta Simon의 전자 메일 주소를 입력합니다.

    다. **암호 표시** 확인란을 선택한 다음 **암호** 상자에 표시된 값을 적어둡니다.

    d. **만들기**를 클릭합니다.
 
### <a name="create-a-way-we-do-test-user"></a>Way We Do 테스트 사용자 만들기

이 섹션은 Way We Do에서 Britta Simon이라는 사용자를 만들기 위한 것입니다. Way We Do는 Just-In-Time 프로비전을 지원하며 기본적으로 사용하도록 설정합니다. 이 섹션에 작업 항목이 없습니다. 새 사용자가 아직 존재하지 않는 경우 Way We Do에 액세스하는 동안 만들어집니다.

> [!Note]
> 사용자를 수동으로 만들어야 하는 경우, [Way We Do 클라이언트 지원 팀](mailto:support@waywedo.com)에 문의하세요.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 Britta Simon에게 Way We Do에 대한 액세스 권한을 부여합니다.

![사용자 역할 할당][200] 

**Britta Simon을 Way We Do에 할당하려면 다음 단계를 수행합니다.**

1. Azure Portal에서 응용 프로그램 보기를 연 다음 디렉터리 보기로 이동하고 **엔터프라이즈 응용 프로그램**으로 이동한 후 **모든 응용 프로그램**을 클릭합니다.

    ![사용자 할당][201] 

2. 응용 프로그램 목록에서 **Way We Do**를 선택합니다.

    ![응용 프로그램 목록의 Way We Do 연결](./media/waywedo-tutorial/tutorial_waywedo_app.png)  

3. 왼쪽 메뉴에서 **사용자 및 그룹**을 클릭합니다.

    !["사용자 및 그룹" 링크][202]

4. **추가** 단추를 클릭합니다. 그런 후 **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![할당 추가 창][203]

5. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택합니다.

6. **사용자 및 그룹** 대화 상자에서 **선택** 단추를 클릭합니다.

7. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.
    
### <a name="test-single-sign-on"></a>Single Sign-On 테스트

이 섹션에서는 액세스 패널을 사용하여 Azure AD Single Sign-On 구성을 테스트합니다.

액세스 패널에서 Way We Do 타일을 클릭하면 Way We Do 응용 프로그램에 자동으로 로그온됩니다.
액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](../active-directory-saas-access-panel-introduction.md)를 참조하세요. 

## <a name="additional-resources"></a>추가 리소스

* [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](tutorial-list.md)
* [Azure Active Directory로 응용 프로그램 액세스 및 Single Sign-On을 구현하는 방법](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/waywedo-tutorial/tutorial_general_01.png
[2]: ./media/waywedo-tutorial/tutorial_general_02.png
[3]: ./media/waywedo-tutorial/tutorial_general_03.png
[4]: ./media/waywedo-tutorial/tutorial_general_04.png

[100]: ./media/waywedo-tutorial/tutorial_general_100.png

[200]: ./media/waywedo-tutorial/tutorial_general_200.png
[201]: ./media/waywedo-tutorial/tutorial_general_201.png
[202]: ./media/waywedo-tutorial/tutorial_general_202.png
[203]: ./media/waywedo-tutorial/tutorial_general_203.png
