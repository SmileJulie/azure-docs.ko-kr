---
title: '자습서: Datahug와 Azure Active Directory 통합 | Microsoft 문서'
description: Azure Active Directory 및 Datahug 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 5c0dc1ea-7ff4-4554-b60b-0f2fa9f5abaa
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2017
ms.author: jeedes
ms.openlocfilehash: bb2d6194b5a515d89e3204679860ab19a052ba03
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36285167"
---
# <a name="tutorial-azure-active-directory-integration-with-datahug"></a>자습서: Datahug와 Azure Active Directory 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 Datahug를 통합하는 방법에 대해 알아봅니다.

Datahug를 Azure AD와 통합하면 다음과 같은 이점이 제공됩니다.

- Datahug에 대한 액세스 권한이 있는 사용자를 Azure AD에서 제어할 수 있습니다.
- 사용자가 해당 Azure AD 계정으로 Datahug에 자동으로 로그온(Single Sign-on)되도록 설정할 수 있습니다.
- 단일 중앙 위치인 Azure Portal에서 계정을 관리할 수 있습니다.

Azure AD와의 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory를 사용한 응용 프로그램 액세스 및 Single Sign-On이란 무엇인가요?](../manage-apps/what-is-single-sign-on.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

Datahug와 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

- Azure AD 구독
- Datahug Single Sign-On이 설정된 구독

> [!NOTE]
> 이 자습서의 단계를 테스트하기 위해 프로덕션 환경을 사용하는 것은 바람직하지 않습니다.

이 자습서의 단계를 테스트하려면 다음 권장 사항을 준수해야 합니다.

- 꼭 필요한 경우가 아니면 프로덕션 환경을 사용하지 마세요.
- Azure AD 평가판 환경이 없으면 [여기](https://azure.microsoft.com/pricing/free-trial/)에서 1개월 평가판을 얻을 수 있습니다.

## <a name="scenario-description"></a>시나리오 설명
이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 테스트 합니다. 이 자습서에 설명된 시나리오는 다음 두 가지 주요 구성 요소로 이루어져 있습니다.

1. 갤러리에서 Datahug 추가
2. Azure AD Single Sign-on 구성 및 테스트

## <a name="adding-datahug-from-the-gallery"></a>갤러리에서 Datahug 추가
Datahug의 Azure AD 통합을 구성하려면 갤러리의 Datahug를 관리되는 SaaS 앱 목록에 추가해야 합니다.

**갤러리에서 Datahug를 추가하려면 다음 단계를 수행합니다.**

1. **[Azure Portal](https://portal.azure.com)** 의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다. 

    ![Active Directory][1]

2. **엔터프라이즈 응용 프로그램**으로 이동합니다. 그런 후 **모든 응용 프로그램**으로 이동합니다.

    ![응용 프로그램][2]
    
3. 새 응용 프로그램을 추가하려면 대화 상자 맨 위 있는 **새 응용 프로그램** 단추를 클릭합니다.

    ![응용 프로그램][3]

4. 검색 상자에 **Datahug**를 입력합니다.

    ![Azure AD 테스트 사용자 만들기](./media/datahug-tutorial/tutorial_datahug_search.png)

5. 결과 패널에서 **Datahug**를 선택하고 **추가** 단추를 클릭하여 응용 프로그램을 추가합니다.

    ![Azure AD 테스트 사용자 만들기](./media/datahug-tutorial/tutorial_datahug_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Azure AD Single Sign-on 구성 및 테스트
이 섹션에서는 "Britta Simon"이라는 테스트 사용자를 기반으로 Datahug에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

Single Sign-On이 작동하려면 Azure AD에서 Azure AD 사용자에 해당하는 Datahug 사용자가 누구인지 알고 있어야 합니다. 즉, Azure AD 사용자와 Datahug의 관련 사용자 간에 연결이 형성되어야 합니다.

이 연결 관계는 Azure AD의 **사용자 이름** 값을 Datahug의 **Username** 값으로 할당하여 설정합니다.

Datahug에서 Azure AD Single Sign-On을 구성하고 테스트하려면 다음 구성 요소를 완료해야 합니다.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
2. **[Azure AD 테스트 사용자 만들기](#creating-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On 테스트하는 데 사용합니다.
3. **[Datahug 테스트 사용자 만들기](#creating-a-datahug-test-user)** - Britta Simon의 Azure AD 표현과 연결되는 대응 사용자를 Datahug에 만듭니다.
4. **[Azure AD 테스트 사용자 할당](#assigning-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
5. **[Single Sign-On 테스트](#testing-single-sign-on)** - 구성이 작동하는지 확인합니다.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure Portal에서 Azure AD Single Sign-On을 사용하도록 설정하고 Datahug 응용 프로그램에서 Single Sign-On을 구성합니다.

**Datahug에서 Azure AD Single Sign-on을 구성하려면 다음 단계를 수행합니다.**

1. Azure Portal의 **Datahug** 응용 프로그램 통합 페이지에서 **Single Sign-On**을 클릭합니다.

    ![Configure Single Sign-On][4]

2. **Single Sign-On** 대화 상자에서 **모드**를 **SAML 기반 로그온**으로 선택하여 Single Sign-On을 사용하도록 설정합니다.
 
    ![Configure Single Sign-On](./media/datahug-tutorial/tutorial_datahug_samlbase.png)

3. **Datahug 도메인 및 URL** 섹션에서 **IDP 시작 모드**로 응용 프로그램을 구성하려는 경우 다음 단계를 수행합니다.

    ![Configure Single Sign-On](./media/datahug-tutorial/tutorial_datahug_ur1.png)

    a. **식별자** 텍스트 상자에서 `https://apps.datahug.com/identity/<uniqueID>` 패턴을 사용하여 URL을 입력합니다.

    나. **회신 URL** 텍스트 상자에 다음 패턴으로 URL을 입력합니다.`https://apps.datahug.com/identity/<uniqueID>/acs`

4. **고급 URL 설정 표시**를 선택합니다. **SP** 시작 모드에서 응용 프로그램을 구성하려면:

    ![Configure Single Sign-On](./media/datahug-tutorial/tutorial_datahug_url2.png)

    **로그온 URL** 텍스트 상자에 URL을 입력합니다. `https://apps.datahug.com/`
     
    > [!NOTE] 
    > 이러한 값은 실제 값이 아닙니다. 실제 식별자 및 회신 URL로 해당 값을 업데이트합니다. 식별자 및 회신 URL에는 고유한 문자열 값을 사용하는 것이 좋습니다. 이러한 값을 얻으려면 [Datahug 클라이언트 지원 팀](http://datahug.com/about/contact-us/)에 문의하세요. 

5. **SAML 서명 인증서** 섹션에서 **메타데이터 XML**을 클릭한 후 컴퓨터에 메타데이터 파일을 저장합니다.

    ![Configure Single Sign-On](./media/datahug-tutorial/tutorial_datahug_certificate.png) 

6.  **“고급 인증서 서명 설정 표시”** 를 선택하고 다음 단계를 수행합니다.

    ![Configure Single Sign-On](./media/datahug-tutorial/tutorial_datahug_cert.png)

    a. **서명 옵션**에서 **SAML 어설션 서명**을 선택합니다.
    
    나. **서명 알고리즘**에서 **SHA1**을 선택합니다.
 
7. **저장** 단추를 클릭합니다.

    ![Configure Single Sign-On](./media/datahug-tutorial/tutorial_general_400.png)
    
8. **Datahug 구성** 섹션에서 **Datahug 구성**을 클릭하여 **로그온 구성** 창을 엽니다. **빠른 참조 섹션**에서 **SAML 엔터티 ID** 및 **SAML Single Sign-On 서비스 URL**을 복사합니다.

    ![Configure Single Sign-On](./media/datahug-tutorial/tutorial_datahug_configure.png) 

9. **Datahug** 쪽에서 Single Sign-On을 구성하려면 다운로드한 **메타데이터 XML**, **SAML 엔터티 ID** 및 **SAML Single Sign-On 서비스 URL**을 [Datahug 지원](http://datahug.com/about/contact-us/)으로 보내야 합니다. 그러면 이 응용 프로그램에서 SAML SSO 연결이 양쪽에 제대로 설정되도록 설정합니다.

> [!TIP]
> 이제 앱을 설정하는 동안 [Azure Portal](https://portal.azure.com) 내에서 이러한 지침의 간결한 버전을 읽을 수 있습니다.  **Active Directory > 엔터프라이즈 응용 프로그램** 섹션에서 이 앱을 추가한 후에는 **Single Sign-On** 탭을 클릭하고 맨 아래에 있는 **구성** 섹션을 통해 포함된 설명서에 액세스하면 됩니다. 포함된 설명서 기능에 대한 자세한 내용은 [Azure AD 포함된 설명서]( https://go.microsoft.com/fwlink/?linkid=845985)에서 확인할 수 있습니다.
> 

### <a name="creating-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기
이 섹션의 목적은 Azure Portal에서 Britta Simon이라는 테스트 사용자를 만드는 것입니다.

![Azure AD 사용자 만들기][100]

**Azure AD에서 테스트 사용자를 만들려면 다음 단계를 수행하세요.**

1. **Azure Portal**의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다.

    ![Azure AD 테스트 사용자 만들기](./media/datahug-tutorial/create_aaduser_01.png) 

2. 사용자 목록을 표시하려면 **사용자 및 그룹**으로 이동한 후 **모든 사용자**를 클릭합니다.
    
    ![Azure AD 테스트 사용자 만들기](./media/datahug-tutorial/create_aaduser_02.png) 

3. **사용자** 대화 상자를 열려면 대화 상자 위쪽에서 **추가**를 클릭합니다.
 
    ![Azure AD 테스트 사용자 만들기](./media/datahug-tutorial/create_aaduser_03.png) 

4. **사용자** 대화 상자 페이지에서 다음 단계를 수행합니다.
 
    ![Azure AD 테스트 사용자 만들기](./media/datahug-tutorial/create_aaduser_04.png) 

    a. **이름** 텍스트 상자에 **BrittaSimon**을 입력합니다.

    나. **사용자 이름** 텍스트 상자에 BrittaSimon의 **전자 메일 주소**를 입력합니다.

    다. **암호 표시**를 선택하고 **암호** 값을 적어둡니다.

    d. **만들기**를 클릭합니다.
 
### <a name="creating-a-datahug-test-user"></a>Datahug 테스트 사용자 만들기

Azure AD 사용자가 Datahug에 로그인할 수 있도록 하려면 Datahug로 프로비전되어야 합니다.  
Datahug의 경우 프로비전은 수동 작업입니다.

**사용자 계정을 프로비전하려면 다음 단계를 수행합니다.**

1. Datahug 회사 사이트에 관리자 권한으로 로그인합니다.

2. 오른쪽 위 모퉁이에 있는 **cog** 위로 마우스를 가져가고 **설정**을 클릭합니다.
   
   ![직원 추가](./media/datahug-tutorial/1.png)

3. **피플**을 선택하고 **사용자 추가** 탭을 클릭합니다.

    ![직원 추가](./media/datahug-tutorial/2.png)

4. 계정을 만들 사용자의 메일을 입력하고 **추가**를 클릭합니다.

    ![직원 추가](./media/datahug-tutorial/3.png)

    > [!NOTE] 
    > **환영 메일 보내기** 확인란을 선택하여 사용자에게 등록 메일을 보낼 수 있습니다.  
    > Salesforce에 대한 계정을 만들 경우 환영 메일을 보내지 마세요.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 Britta Simon에게 Datahug에 대한 액세스 권한을 부여합니다.

![사용자 할당][200] 

**Britta Simon을 Datahug에 할당하려면 다음 단계를 수행합니다.**

1. Azure Portal에서 응용 프로그램 보기를 연 다음 디렉터리 보기로 이동하고 **엔터프라이즈 응용 프로그램**으로 이동한 후 **모든 응용 프로그램**을 클릭합니다.

    ![사용자 할당][201] 

2. 응용 프로그램 목록에서 **Datahug**를 선택합니다.

    ![Configure Single Sign-On](./media/datahug-tutorial/tutorial_datahug_app.png) 

3. 왼쪽 메뉴에서 **사용자 및 그룹**을 클릭합니다.

    ![사용자 할당][202] 

4. **추가** 단추를 클릭합니다. 그런 후 **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![사용자 할당][203]

5. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택합니다.

6. **사용자 및 그룹** 대화 상자에서 **선택** 단추를 클릭합니다.

7. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.
    
### <a name="testing-single-sign-on"></a>Single Sign-On 테스트

이 섹션에서는 액세스 패널을 사용하여 Azure AD Single Sign-On 구성을 테스트합니다.
액세스 패널에서 Datahug 타일을 클릭하면 Datahug 응용 프로그램에 자동으로 로그온됩니다. 액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](../active-directory-saas-access-panel-introduction.md)를 참조하세요. 

## <a name="additional-resources"></a>추가 리소스

* [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](tutorial-list.md)
* [Azure Active Directory로 응용 프로그램 액세스 및 Single Sign-On을 구현하는 방법](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/datahug-tutorial/tutorial_general_01.png
[2]: ./media/datahug-tutorial/tutorial_general_02.png
[3]: ./media/datahug-tutorial/tutorial_general_03.png
[4]: ./media/datahug-tutorial/tutorial_general_04.png

[100]: ./media/datahug-tutorial/tutorial_general_100.png

[200]: ./media/datahug-tutorial/tutorial_general_200.png
[201]: ./media/datahug-tutorial/tutorial_general_201.png
[202]: ./media/datahug-tutorial/tutorial_general_202.png
[203]: ./media/datahug-tutorial/tutorial_general_203.png
