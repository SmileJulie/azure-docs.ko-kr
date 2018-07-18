---
title: '자습서: Slack과 Azure Active Directory 통합 | Microsoft 문서'
description: Azure Active Directory와 Slack 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ffc5e73f-6c38-4bbb-876a-a7dd269d4e1c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 5d16e633f29ca635a9b32118e699cb59d3896353
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36221345"
---
# <a name="tutorial-azure-active-directory-integration-with-slack"></a>자습서: Slack과 Azure Active Directory 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 Slack을 통합하는 방법에 대해 알아봅니다.

Slack과 Azure AD를 통합하면 다음과 같은 이점이 제공됩니다.

- Azure AD에서는 Slack에 대한 액세스 권한이 있는 사용자를 제어할 수 있습니다.
- 사용자가 Azure AD 계정으로 Slack에 자동으로 로그인(Single Sign-On)할 수 있도록 설정할 수 있습니다.
- 단일 중앙 위치인 Azure Portal에서 계정을 관리할 수 있습니다.

Azure AD와 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory의 응용 프로그램 액세스 및 Single Sign-On이란 무엇인가요?](../manage-apps/what-is-single-sign-on.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

Slack과 Azure AD를 통합하도록 구성하려면 다음 항목이 필요합니다.

- Azure AD 구독
- Slack Single Sign-On이 설정된 구독

> [!NOTE]
> 이 자습서의 단계를 테스트하기 위해 프로덕션 환경을 사용하는 것은 바람직하지 않습니다.

이 자습서의 단계를 테스트하려면 다음 권장 사항을 준수해야 합니다.

- 꼭 필요한 경우가 아니면 프로덕션 환경을 사용하지 마세요.
- Azure AD 평가판 환경이 없으면 [1개월 평가판을 얻을](https://azure.microsoft.com/pricing/free-trial/) 수 있습니다.

## <a name="scenario-description"></a>시나리오 설명
이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 테스트 합니다. 이 자습서에 설명된 시나리오는 다음 두 가지 주요 구성 요소로 이루어져 있습니다.

1. 갤러리에서 Slack 추가
2. Azure AD Single Sign-on 구성 및 테스트

## <a name="adding-slack-from-the-gallery"></a>갤러리에서 Slack 추가
Azure AD에 Slack을 통합하도록 구성하려면 갤러리의 Slack을 관리되는 SaaS 앱 목록에 추가해야 합니다.

**갤러리에서 Slack을 추가하려면 다음 단계를 수행합니다.**

1. **[Azure Portal](https://portal.azure.com)** 의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다. 

    ![Active Directory][1]

2. **엔터프라이즈 응용 프로그램**으로 이동합니다. 그런 후 **모든 응용 프로그램**으로 이동합니다.

    ![응용 프로그램][2]
    
3. 새 응용 프로그램을 추가하려면 대화 상자 맨 위 있는 **새 응용 프로그램** 단추를 클릭합니다.

    ![응용 프로그램][3]

4. 검색 상자에서 **Slack**을 입력합니다.

    ![Azure AD 테스트 사용자 만들기](./media/slack-tutorial/tutorial_slack_search.png)

5. 결과 창에서 **Slack**을 선택하고 **추가** 단추를 클릭하여 응용 프로그램을 추가합니다.

    ![Azure AD 테스트 사용자 만들기](./media/slack-tutorial/tutorial_slack_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Azure AD Single Sign-on 구성 및 테스트
이 섹션에서는 "Britta Simon"이라는 테스트 사용자를 기반으로 Slack에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

Single Sign-On이 작동하려면 Azure AD에서 Azure AD 사용자에 해당하는 Slack 사용자가 누구인지 알고 있어야 합니다. 즉 Azure AD 사용자와 Slack의 관련 사용자 간에 연결 관계가 설정되어야 합니다.

Slack에서 Azure AD의 **사용자 이름** 값을 **Username** 값으로 할당하여 링크 관계를 설정합니다.

Slack에서 Azure AD Single Sign-On을 구성하고 테스트하려면 다음 구성 요소를 완료해야 합니다.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
2. **[Azure AD 테스트 사용자 만들기](#creating-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On 테스트하는 데 사용합니다.
3. **[Slack 테스트 사용자 만들기](#creating-a-slack-test-user)** - Britta Simon의 Azure AD 표현과 연결되는 대응 사용자를 Slack에 만듭니다.
4. **[Azure AD 테스트 사용자 할당](#assigning-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
5. **[Single Sign-On 테스트](#testing-single-sign-on)** - 구성이 작동하는지 확인합니다.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure Portal에서 Azure AD Single Sign-On을 사용하도록 설정하고 Slack 응용 프로그램에서 Single Sign-On을 구성합니다.

**Slack에서 Azure AD Single Sign-On을 구성하려면 다음 단계를 수행합니다.**

1. Azure Portal의 **Slack** 응용 프로그램 통합 페이지에서 **Single Sign-On**을 클릭합니다.

    ![Configure Single Sign-On][4]

2. **Single Sign-On** 대화 상자에서 **모드**를 **SAML 기반 로그온**으로 선택하여 Single Sign-On을 사용하도록 설정합니다.
 
    ![Configure Single Sign-On](./media/slack-tutorial/tutorial_slack_samlbase.png)

3. **Slack 도메인 및 URL** 섹션에서 다음 단계를 수행합니다.

    ![Configure Single Sign-On](./media/slack-tutorial/tutorial_slack_url.png)

    a. **로그온 URL** 텍스트 상자에서 다음 패턴으로 URL을 입력합니다. `https://<companyname>.slack.com`

    나. **식별자** 텍스트 상자에 URL `https://slack.com`을 입력합니다.

    > [!NOTE] 
    > 이 값은 실제 값이 아닙니다. 이 값은 실제 로그인 URL로 업데이트해야 합니다. 이러한 값을 얻으려면 [Slack 지원 팀](https://slack.com/help/contact)에 문의하세요.
     
4. Slack 응용 프로그램에는 특정 형식의 SAML 어설션이 필요합니다. 이 응용 프로그램에 대해 다음 클레임을 구성합니다. 응용 프로그램 통합 페이지의 **"사용자 특성"** 섹션에서 이러한 특성의 값을 관리할 수 있습니다. 다음 스크린샷은 이에 대한 예제를 보여 줍니다.
    
    ![Configure Single Sign-On](./media/slack-tutorial/tutorial_slack_attribute.png)

    > [!NOTE] 
    > **이메일 주소**가 할당된 사용자가 Office365 라이선스에 없으면 **User.Email** 클레임이 SAML 토큰에 표시되지 않습니다. 이 경우에서는 **User.Email** 특성 값으로 **user.userprincipalname**을 사용하여 **고유 식별자**로 대신 매핑하는 것이 좋습니다.

5. **Single sign on** 대화 상자의 **사용자 특성** 섹션에서 **사용자 식별자**로 **user.mail**을 선택하고 아래 표에 표시되는 각 행에 대해 다음 단계를 수행합니다.
    
    | 특성 이름 | 특성 값 |
    | --- | --- |
    | first_name | user.givenname |
    | last_name | user.surname |
    | User.Email | user.mail |  
    | User.Username | user.userprincipalname |

    a. **특성**을 클릭하여 **특성 편집** 대화 상자를 열고 다음 단계를 수행합니다.

    ![Configure Single Sign-On](./media/slack-tutorial/tutorial_slack_attribute1.png)

    a. **이름** 텍스트 상자에서 해당 행에 표시된 특성 이름을 입력합니다.

    나. **값** 목록에서 해당 행에 표시된 특성 값을 선택합니다.

    다. **네임스페이스**를 비워 둡니다.

    d. **확인**

6. **SAML 서명 인증서** 섹션에서 **인증서(Base64)** 를 클릭한 후 컴퓨터에 인증서 파일을 저장합니다.

    ![Configure Single Sign-On](./media/slack-tutorial/tutorial_slack_certificate.png)

7. **저장** 단추를 클릭합니다.

    ![Configure Single Sign-On](./media/slack-tutorial/tutorial_general_400.png)

8. **Slack 구성** 섹션에서 **Slack 구성**을 클릭하여 **로그온 구성** 창을 엽니다. **빠른 참조** 섹션에서 **SAML 엔터티 ID 및 SAML Single Sign-On 서비스 URL**을 복사합니다.

    ![Configure Single Sign-On](./media/slack-tutorial/tutorial_slack_configure.png)

9. 다른 웹 브라우저 창에서 Slack 회사 사이트에 관리자로 로그인합니다.

10. **Microsoft Azure AD**, **팀 설정**으로 차례로 이동합니다.

     ![앱 쪽에서 Single Sign-On 구성](./media/slack-tutorial/tutorial_slack_001.png)

11. **팀 설정** 섹션에서 **인증** 탭을 클릭한 다음 **설정 변경**을 클릭합니다.

    ![앱 쪽에서 Single Sign-On 구성](./media/slack-tutorial/tutorial_slack_002.png)

12. **SAML 인증 설정** 대화 상자에서 다음 단계를 수행합니다.

    ![앱 쪽에서 Single Sign-On 구성](./media/slack-tutorial/tutorial_slack_003.png)

    a.  Azure Portal에서 복사한 **SAML Single Sign-On 서비스 URL** 값을 **SAML 2.0 끝점(HTTP)** 텍스트 상자에 붙여 넣습니다.

    나.  Azure Portal에서 복사한 **SAML 엔터티 ID** 값을 **ID 공급자 발급자** 텍스트 상자에 붙여 넣습니다.

    다.  다운로드한 인증서 파일을 메모장에서 열고 내용을 클립보드에 복사한 다음 **공용 인증서** 텍스트 상자에 붙여넣습니다.

    d. 위의 세 가지 설정을 Slack 팀에 적합하게 구성합니다. 설정에 대한 자세한 내용은 **Slack의 SSO 구성 가이드**를 참조하세요. `https://get.slack.help/hc/articles/220403548-Guide-to-single-sign-on-with-Slack%60`

    e.  **구성 저장**을 클릭하십시오.

### <a name="creating-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기
이 섹션의 목적은 Azure Portal에서 Britta Simon이라는 테스트 사용자를 만드는 것입니다.

![Azure AD 사용자 만들기][100]

**Azure AD에서 테스트 사용자를 만들려면 다음 단계를 수행하세요.**

1. **Azure Portal**의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다.

    ![Azure AD 테스트 사용자 만들기](./media/slack-tutorial/create_aaduser_01.png) 

2. 사용자 목록을 표시하려면 **사용자 및 그룹**으로 이동한 후 **모든 사용자**를 클릭합니다.
    
    ![Azure AD 테스트 사용자 만들기](./media/slack-tutorial/create_aaduser_02.png) 

3. **사용자** 대화 상자를 열려면 대화 상자 위쪽에서 **추가**를 클릭합니다.

    ![Azure AD 테스트 사용자 만들기](./media/slack-tutorial/create_aaduser_03.png)

4. **사용자** 대화 상자 페이지에서 다음 단계를 수행합니다.

    ![Azure AD 테스트 사용자 만들기](./media/slack-tutorial/create_aaduser_04.png)

    a. **이름** 텍스트 상자에 **BrittaSimon**을 입력합니다.

    나. **사용자 이름** 텍스트 상자에 BrittaSimon의 **전자 메일 주소**를 입력합니다.

    다. **암호 표시**를 선택하고 **암호** 값을 적어둡니다.

    d. **만들기**를 클릭합니다.

### <a name="creating-a-slack-test-user"></a>Slack 테스트 사용자 만들기

이 섹션에서는 Slack에서 Britta Simon이라는 사용자를 만듭니다. Slack은 just-in-time 프로비전을 지원하며 기본적으로 사용하도록 설정됩니다. 이 섹션에 작업 항목이 없습니다. 새 사용자가 아직 존재하지 않는 경우 Slack에 액세스하는 동안 만들어집니다. Slack은 자동 사용자 프로비전도 지원합니다. 자동 사용자 프로비전 구성 방법에 대한 자세한 내용은 [여기](slack-provisioning-tutorial.md)에서 제공합니다.

> [!NOTE]
> 사용자를 수동으로 만들어야 하는 경우 [Slack 지원 팀](https://slack.com/help/contact)에 문의해야 합니다.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 Britta Simon에게 Slack에 대한 액세스 권한을 부여합니다.

![사용자 할당][200]

**Britta Simon을 Slack에 할당하려면 다음 단계를 수행합니다.**

1. Azure Portal에서 응용 프로그램 보기를 연 다음 디렉터리 보기로 이동하고 **엔터프라이즈 응용 프로그램**으로 이동한 후 **모든 응용 프로그램**을 클릭합니다.

    ![사용자 할당][201] 

2. 응용 프로그램 목록에서 **Slack**를 선택합니다.

    ![Configure Single Sign-On](./media/slack-tutorial/tutorial_slack_app.png) 

3. 왼쪽 메뉴에서 **사용자 및 그룹**을 클릭합니다.

    ![사용자 할당][202] 

4. **추가** 단추를 클릭합니다. 그런 후 **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![사용자 할당][203]

5. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택합니다.

6. **사용자 및 그룹** 대화 상자에서 **선택** 단추를 클릭합니다.

7. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.
    
### <a name="testing-single-sign-on"></a>Single Sign-On 테스트

이 섹션에서는 액세스 패널을 사용하여 Azure AD Single Sign-On 구성을 테스트합니다.

[액세스 패널]에서 [Slack] 타일을 클릭하면 Slack 응용 프로그램에 자동으로 로그온됩니다.

## <a name="additional-resources"></a>추가 리소스

* [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](tutorial-list.md)
* [Azure Active Directory로 응용 프로그램 액세스 및 Single Sign-On을 구현하는 방법](../manage-apps/what-is-single-sign-on.md)
* [사용자 프로비저닝 구성](slack-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/slack-tutorial/tutorial_general_01.png
[2]: ./media/slack-tutorial/tutorial_general_02.png
[3]: ./media/slack-tutorial/tutorial_general_03.png
[4]: ./media/slack-tutorial/tutorial_general_04.png

[100]: ./media/slack-tutorial/tutorial_general_100.png

[200]: ./media/slack-tutorial/tutorial_general_200.png
[201]: ./media/slack-tutorial/tutorial_general_201.png
[202]: ./media/slack-tutorial/tutorial_general_202.png
[203]: ./media/slack-tutorial/tutorial_general_203.png