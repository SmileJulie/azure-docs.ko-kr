---
title: '자습서: TrackVia와 Azure Active Directory 통합 | Microsoft Docs'
description: Azure Active Directory와 TrackVia 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: e7010023-bdda-4a19-a335-19904e75b813
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: jeedes
ms.openlocfilehash: a1f1346e363318c73e2cba6a6a356cc376a39eac
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36217962"
---
# <a name="tutorial-azure-active-directory-integration-with-trackvia"></a>자습서: TrackVia와 Azure Active Directory 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 TrackVia를 통합하는 방법에 대해 알아봅니다.

TrackVia와 Azure AD를 통합하면 다음과 같은 이점이 제공됩니다.

- TrackVia에 대한 액세스 권한이 있는 사용자를 Azure AD에서 제어할 수 있습니다.
- 사용자가 해당 Azure AD 계정으로 TrackVia에 자동으로 로그온(Single Sign-on)되도록 설정할 수 있습니다.
- 단일 중앙 위치인 Azure Portal에서 계정을 관리할 수 있습니다.

Azure AD와 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory의 응용 프로그램 액세스 및 Single Sign-On이란 무엇인가요?](../manage-apps/what-is-single-sign-on.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

TrackVia와 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

- Azure AD 구독
- TrackVia Single Sign-on이 설정된 구독

> [!NOTE]
> 이 자습서의 단계를 테스트하기 위해 프로덕션 환경을 사용하는 것은 바람직하지 않습니다.

이 자습서의 단계를 테스트하려면 다음 권장 사항을 준수해야 합니다.

- 꼭 필요한 경우가 아니면 프로덕션 환경을 사용하지 마세요.
- Azure AD 평가판 환경이 없으면 [1개월 평가판을 얻을](https://azure.microsoft.com/pricing/free-trial/) 수 있습니다.

## <a name="scenario-description"></a>시나리오 설명
이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 테스트 합니다. 이 자습서에 설명된 시나리오는 다음 두 가지 주요 구성 요소로 이루어져 있습니다.

1. 갤러리에서 TrackVia 추가
2. Azure AD Single Sign-on 구성 및 테스트

## <a name="adding-trackvia-from-the-gallery"></a>갤러리에서 TrackVia 추가
Azure AD와 TrackVia 통합을 구성하려면 갤러리의 TrackVia를 관리되는 SaaS 앱 목록에 추가해야 합니다.

**갤러리에서 TrackVia를 추가하려면 다음 단계를 수행합니다.**

1. **[Azure Portal](https://portal.azure.com)** 의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다. 

    ![Azure Active Directory 단추][1]

2. **엔터프라이즈 응용 프로그램**으로 이동합니다. 그런 후 **모든 응용 프로그램**으로 이동합니다.

    ![엔터프라이즈 응용 프로그램 블레이드][2]
    
3. 새 응용 프로그램을 추가하려면 대화 상자 맨 위 있는 **새 응용 프로그램** 단추를 클릭합니다.

    ![새 응용 프로그램 단추][3]

4. 검색 상자에서 **TrackVia**를 입력하고, 결과 패널에서 **TrackVia**를 선택한 다음, **추가** 단추를 클릭하여 응용 프로그램을 추가합니다.

    ![결과 목록의 TrackVia](./media/trackvia-tutorial/tutorial_trackvia_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성 및 테스트

이 섹션에서는 "Britta Simon"이라는 테스트 사용자를 기반으로 TrackVia에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

Single Sign-On이 작동하려면 Azure AD에서 Azure AD 사용자에 해당하는 TrackVia 사용자가 누군지 알고 있어야 합니다. 즉, Azure AD 사용자와 TrackVia의 관련 사용자 간에 연결 관계가 형성되어야 합니다.

TrackVia에서 Azure AD의 **사용자 이름** 값을 **Username** 값으로 할당하여 링크 관계를 설정합니다.

TrackVia에서 Azure AD Single Sign-On을 구성하고 테스트하려면 다음 구성 요소를 완료해야 합니다.

1. **[Azure AD Single Sign-On 구성](#configure-azure-ad-single-sign-on)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
2. **[Azure AD 테스트 사용자 만들기](#create-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On을 테스트하는 데 사용합니다.
3. **[TrackVia 테스트 사용자 만들기](#create-a-trackvia-test-user)** - Britta Simon의 Azure AD 표현과 연결된 해당 사용자를 TrackVia에 만듭니다.
4. **[Azure AD 테스트 사용자 할당](#assign-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
5. **[Single Sign-On 테스트](#test-single-sign-on)** - 구성이 작동하는지 여부를 확인합니다.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure Portal에서 Azure AD Single Sign-On을 사용하도록 설정하고 TrackVia 응용 프로그램에서 Single Sign-On을 구성합니다.

**TrackVia에서 Azure AD Single Sign-On을 구성하려면 다음 단계를 수행합니다.**

1. Azure Portal의 **TrackVia** 응용 프로그램 통합 페이지에서 **Single Sign-On**을 클릭합니다.

    ![Single Sign-On 구성 링크][4]

2. **Single Sign-On** 대화 상자에서 **모드**를 **SAML 기반 로그온**으로 선택하여 Single Sign-On을 사용하도록 설정합니다.
 
    ![Single Sign-On 대화 상자](./media/trackvia-tutorial/tutorial_trackvia_samlbase.png)

3. **TrackVia 도메인 및 URL** 섹션에서 **IDP** 시작 모드로 응용 프로그램을 구성하려는 경우 다음 단계를 수행합니다.

    ![TrackVia 도메인 및 URL Single Sign-On 정보](./media/trackvia-tutorial/tutorial_trackvia_url.png)

    **식별자** 텍스트 상자에 해당 값으로 `TrackVia`를 입력합니다.

4. **SP** 시작 모드에서 응용 프로그램을 구성하려면 **고급 URL 설정 표시**를 확인하고 다음 단계를 수행합니다.

    ![TrackVia 도메인 및 URL Single Sign-On 정보](./media/trackvia-tutorial/tutorial_trackvia_url1.png)

    **로그온 URL** 텍스트 상자에서 다음 패턴으로 URL을 입력합니다. `https://companyname.trackvia.com`
     
    > [!NOTE] 
    > 로그온 URL 값은 실제 값이 아닙니다. 이 값을 실제 로그온 URL로 업데이트합니다. 이 값을 얻으려면 [TrackVia 클라이언트 지원 팀](mailto:support@trackvia.com)에 문의하세요.

5. **SAML 서명 인증서** 섹션에서 **메타데이터 XML**을 클릭한 후 컴퓨터에 메타데이터 파일을 저장합니다.

    ![인증서 다운로드 링크](./media/trackvia-tutorial/tutorial_trackvia_certificate.png) 

6. **저장** 단추를 클릭합니다.

    ![Single Sign-On 구성 저장 단추](./media/trackvia-tutorial/tutorial_general_400.png)

7. **TrackVia 구성** 섹션에서 **TrackVia 구성**을 클릭하여 **로그온 구성** 창을 엽니다. **빠른 참조 섹션**에서 **SAML 엔터티 ID**를 복사합니다.

    ![TrackVia 구성](./media/trackvia-tutorial/tutorial_trackvia_configure.png)
    
8. 다른 웹 브라우저 창에서 TrackVia 회사 사이트에 관리자로 로그인합니다.

9. Trackvia **내 계정** 설정을 클릭한 다음, **Single Sign-On** 탭에서 다음 단계를 수행합니다.

    ![TrackVia 구성](./media/trackvia-tutorial/configure1.png)

    a. Azure Portal에서 복사한 **SAML 엔터티 ID** 값을 **ID 공급자 엔터티 ID** 텍스트 상자에 붙여넣습니다.

    나. **파일 선택**을 클릭하여 Azure Portal에서 다운로드한 메타데이터 파일을 업로드합니다.

    다. 페이지 맨 아래에 있는 **저장**

### <a name="create-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기

이 섹션의 목적은 Azure Portal에서 Britta Simon이라는 테스트 사용자를 만드는 것입니다.

   ![Azure AD 테스트 사용자 만들기][100]

**Azure AD에서 테스트 사용자를 만들려면 다음 단계를 수행하세요.**

1. Azure Portal의 왼쪽 창에서 **Azure Active Directory** 단추를 클릭합니다.

    ![Azure Active Directory 단추](./media/trackvia-tutorial/create_aaduser_01.png)

2. 사용자 목록을 표시하려면 **사용자 및 그룹**으로 이동한 후 **모든 사용자**를 클릭합니다.

    !["사용자 및 그룹" 및 "모든 사용자" 링크](./media/trackvia-tutorial/create_aaduser_02.png)

3. **사용자** 대화 상자를 열려면 **모든 사용자** 대화 상자 위쪽에서 **추가**를 클릭합니다.

    ![추가 단추](./media/trackvia-tutorial/create_aaduser_03.png)

4. **사용자** 대화 상자에서 다음 단계를 수행합니다.

    ![사용자 대화 상자](./media/trackvia-tutorial/create_aaduser_04.png)

    a. **이름** 상자에 **BrittaSimon**을 입력합니다.

    나. **사용자 이름** 상자에 사용자인 Britta Simon의 전자 메일 주소를 입력합니다.

    다. **암호 표시** 확인란을 선택한 다음 **암호** 상자에 표시된 값을 적어둡니다.

    d. **만들기**를 클릭합니다.
 
### <a name="create-a-trackvia-test-user"></a>TrackVia 테스트 사용자 만들기

이 섹션은 TrackVia에서 Britta Simon이라는 사용자를 만드는 방법을 보여주기 위한 것입니다. TrackVia는 적시에 프로비전을 지원하며 기본적으로 사용하도록 설정합니다. 이 섹션에 작업 항목이 없습니다. 새 사용자가 아직 존재하지 않는 경우 TrackVia에 액세스하는 동안 만들어집니다.
>[!Note]
>사용자를 수동으로 만들어야 하는 경우 [TrackVia 지원 팀](mailto:support@trackvia.com)에 문의하세요.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 Britta Simon에게 TrackVia에 대한 액세스 권한을 부여합니다.

![사용자 역할 할당][200] 

**Britta Simon을 TrackVia에 할당하려면 다음 단계를 수행합니다.**

1. Azure Portal에서 응용 프로그램 보기를 연 다음 디렉터리 보기로 이동하고 **엔터프라이즈 응용 프로그램**으로 이동한 후 **모든 응용 프로그램**을 클릭합니다.

    ![사용자 할당][201] 

2. 응용 프로그램 목록에서 **TrackVia**를 선택합니다.

    ![응용 프로그램 목록의 TrackVia 링크](./media/trackvia-tutorial/tutorial_trackvia_app.png)  

3. 왼쪽 메뉴에서 **사용자 및 그룹**을 클릭합니다.

    !["사용자 및 그룹" 링크][202]

4. **추가** 단추를 클릭합니다. 그런 후 **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![할당 추가 창][203]

5. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택합니다.

6. **사용자 및 그룹** 대화 상자에서 **선택** 단추를 클릭합니다.

7. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.
    
### <a name="test-single-sign-on"></a>Single Sign-On 테스트

이 섹션에서는 액세스 패널을 사용하여 Azure AD Single Sign-On 구성을 테스트합니다.

액세스 패널에서 TrackVia 타일을 클릭하면 TrackVia 응용 프로그램에 자동으로 로그인됩니다.
액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](../active-directory-saas-access-panel-introduction.md)를 참조하세요. 

## <a name="additional-resources"></a>추가 리소스

* [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](tutorial-list.md)
* [Azure Active Directory로 응용 프로그램 액세스 및 Single Sign-On을 구현하는 방법](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/trackvia-tutorial/tutorial_general_01.png
[2]: ./media/trackvia-tutorial/tutorial_general_02.png
[3]: ./media/trackvia-tutorial/tutorial_general_03.png
[4]: ./media/trackvia-tutorial/tutorial_general_04.png

[100]: ./media/trackvia-tutorial/tutorial_general_100.png

[200]: ./media/trackvia-tutorial/tutorial_general_200.png
[201]: ./media/trackvia-tutorial/tutorial_general_201.png
[202]: ./media/trackvia-tutorial/tutorial_general_202.png
[203]: ./media/trackvia-tutorial/tutorial_general_203.png