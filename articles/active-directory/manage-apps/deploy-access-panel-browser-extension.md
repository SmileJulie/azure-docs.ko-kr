---
title: GPO를 사용하여 IE에 대한 Azure 액세스 패널 확장 배포 | Microsoft Docs
description: 그룹 정책을 사용하여 My Apps 포털용 Internet Explorer 추가 기능을 배포하는 방법
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/08/2018
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 71c342ede77349b3f6c22093e5877ad5f5ce6549
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807678"
---
# <a name="how-to-deploy-the-access-panel-extension-for-internet-explorer-using-group-policy"></a>방법: 그룹 정책을 사용 하 여 Internet Explorer 용 액세스 패널 확장을 배포 합니다.

이 자습서에서는 그룹 정책을 사용하여 사용자의 컴퓨터에 Internet Explorer용 액세스 패널 확장을 원격 설치하는 방법을 보여줍니다. 이 확장은 [암호 기반 Single Sign-On](what-is-single-sign-on.md#password-based-sso)을 사용하여 구성된 앱에 로그인하는 Internet Explorer 사용자에게 필요합니다.

관리자는 이 확장의 배포를 자동화하는 것이 좋습니다. 그렇지 않은 경우 사용자가 직접 확장을 다운로드하여 설치해야 하기 때문에 사용자 오류가 발생하기 쉽고 관리자 권한이 필요합니다. 이 자습서에서는 그룹 정책을 사용하여 소프트웨어 배포를 자동화하는 한 가지 방법을 설명합니다. [그룹 정책에 대해 알아봅니다.](https://technet.microsoft.com/windowsserver/bb310732.aspx)

액세스 패널 확장은 [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) 및 [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998)에도 사용할 수 있으며 설치에 관리자 권한이 필요하지 않습니다.

## <a name="prerequisites"></a>필수 구성 요소

* [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx)를 설정하고, 사용자 컴퓨터를 도메인에 가입시킨 상태여야 합니다.
* 그룹 정책 개체(GPO)를 편집하는 "설정 편집" 사용 권한이 있어야 합니다. 기본적으로 다음 보안 그룹의 구성원에게는 이 권한이 있습니다: 도메인 관리자, 엔터프라이즈 관리자 및 그룹 정책 작성 소유자. [자세한 정보](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

## <a name="step-1-create-the-distribution-point"></a>1단계: 배포 지점 만들기

먼저 원격으로 확장을 설치할 컴퓨터에서 액세스할 수 있는 네트워크 위치에 설치 관리자 패키지를 배치해야 합니다. 이렇게 하려면 다음 단계를 수행합니다.

1. 서버에 관리자로 로그인 합니다.
1. **서버 관리자** 창에서 **파일 및 Storage 서비스**로 이동합니다.

    ![파일 및 Storage 서비스 열기](./media/deploy-access-panel-browser-extension/files-services.png)

1. **공유** 탭으로 이동합니다. 그런 다음 **태스크** > **새 공유...** 를 클릭합니다.

    ![스크린샷은 작업 화면에서 새 공유를 찾을 수 있는 위치를 보여 줍니다.](./media/deploy-access-panel-browser-extension/shares.png)

1. **새 공유 마법사** 를 완료하고 사용자의 컴퓨터에서 액세스할 수 있게 권한을 설정합니다. [공유에 대해 알아봅니다.](https://technet.microsoft.com/library/cc753175.aspx)
1. Microsoft Windows Installer 패키지(.msi 파일) [Access Panel Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)를 다운로드합니다.
1. 설치 관리자 패키지를 공유의 원하는 위치에 복사합니다.

    ![.Msi 파일 공유로 복사](./media/deploy-access-panel-browser-extension/copy-package.png)

1. 클라이언트 컴퓨터가 공유에서 설치 관리자 패키지에 액세스할 수 있는지 확인합니다.

## <a name="step-2-create-the-group-policy-object"></a>2단계: 그룹 정책 개체 만들기

1. Active Directory Domain Services (AD DS) 설치를 호스팅하는 서버에 로그인 합니다.
1. 서버 관리자에서 **도구** > **그룹 정책 관리**로 이동합니다.

    ![도구 > 그룹 정책 관리로 이동](./media/deploy-access-panel-browser-extension/tools-gpm.png)

1. **그룹 정책 관리** 창의 왼쪽 창에서 조직 구성 단위(OU) 계층 구조를 확인하고 그룹 정책을 적용할 범위를 결정합니다. 예를 들어, 테스트를 위해 소수의 사용자에게 소규모 OU를 배포하거나, 전 조직에 배포하기 위해 최상위 OU를 선택할 수 있습니다.

   > [!NOTE]
   > 조직 단위(OU)를 만들거나 편집하려면 서버 관리자로 다시 전환하여 **도구** > **Active Directory 사용자 및 컴퓨터**로 이동합니다.

1. OU를 선택한 후에 마우스 오른쪽 단추로 클릭하고 **이 도메인에서 GPO를 만들고 여기에 연결...** 을 선택합니다.

    ![스크린샷은 만들기를 새 GPO 옵션](./media/deploy-access-panel-browser-extension/create-gpo.png)

1. **새 GPO** 프롬프트에 새 그룹 정책 개체의 이름을 입력합니다.
1. 만든 그룹 정책 개체를 마우스 오른쪽 단추로 클릭하고 **편집**을 선택합니다.

## <a name="step-3-assign-the-installation-package"></a>3단계: 설치 패키지를 할당 합니다.

1. 확장을 **컴퓨터 구성** 또는 **사용자 구성**을 기준으로 배포할지 결정합니다. [컴퓨터 구성](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx)을 사용할 경우 사용자가 로그인했는지와 관계없이 컴퓨터에 확장이 설치됩니다. [사용자 구성](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx)의 경우 사용자가 로그온한 컴퓨터인지와 관계없이 사용자에게 확장이 설치됩니다.
1. **그룹 정책 관리 편집기** 창의 왼쪽에서 선택한 구성의 유형에 따라 다음 폴더 경로 중 하나로 이동합니다.

   * `Computer Configuration/Policies/Software Settings/`
   * `User Configuration/Policies/Software Settings/`

1. **소프트웨어 설치**를 마우스 오른쪽 단추로 클릭한 다음 **새로 만들기** > **패키지...** 를 선택합니다.
1. [1단계: 배포 지점 만들기에서 설치 관리자 패키지를 포함하는 공유 폴더로 이동하여 .msi 파일을 선택하 고 **열기**를 클릭합니다.

   > [!IMPORTANT]
   > 공유가 같은 서버에 있는 경우 로컬 파일 경로가 아닌 네트워크 파일 경로를 통해 .msi에 액세스하고 있는지 확인합니다.

    ![공유 폴더에서 설치 패키지를 선택 합니다.](./media/deploy-access-panel-browser-extension/select-package.png)

1. **소프트웨어 배포** 프롬프트에서 배포 방법으로 **할당**을 선택합니다. 그런 다음 **확인**을 클릭합니다.

이제 확장이 선택한OU에 배포되었습니다. [그룹 정책 소프트웨어 설치에 대해 알아봅니다.](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

## <a name="step-4-auto-enable-the-extension-for-internet-explorer"></a>4단계: Internet Explorer 용 확장 자동-사용

설치 프로그램을 실행하는 것 외에도 모든 Internet Explorer용 확장을 명시적으로 활성화해야 사용할 수 있습니다. 다음 단계에 따라 그룹 정책을 사용하여 액세스 패널 확장을 활성화합니다.

1. **그룹 정책 관리 편집기** 창의 [Step 3: 설치 패키지 할당 에서 선택한 구성의 유형에 따라 다음 경로 중 하나로 이동합니다](#step-3-assign-the-installation-package)

   * `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`

1. **추가 기능 목록**을 마우스 오른쪽 단추로 클릭하고 **편집**을 선택합니다.

    !["추가 기능 목록"을 마우스 오른쪽 단추로 클릭 하 고 "편집" 선택](./media/deploy-access-panel-browser-extension/edit-add-on-list.png)

1. **추가 기능 목록** 창에서 **사용**을 선택합니다. 그런 다음 **옵션** 섹션에서 **표시...** 를 클릭합니다.

    ![사용을 클릭한 다음 표시...를 클릭합니다.](./media/deploy-access-panel-browser-extension/edit-add-on-list-window.png)

1. **내용 표시** 창에서 다음 단계를 수행합니다.

   1. 첫번째 열(**값 이름** 필드)에 클래스 ID `{030E9A3F-7B18-4122-9A60-B87235E4F59E}`를 복사하여 붙여 넣습니다.
   1. 두번째 열(**값** 필드)에 `1` 값을 입력합니다.
   1. **확인**을 클릭하여 **내용 표시** 창을 닫습니다.

      ![이전 단계에서 지정한 대로 값을 채웁니다](./media/deploy-access-panel-browser-extension/show-contents.png)

1. **확인**을 클릭하여 변경 내용을 저장하고 **추가 기능 목록** 창을 닫습니다.

이제 선택한 OU의 컴퓨터에서 확장이 활성화되어야 합니다. [그룹 정책을 사용하여 Internet Explorer 추가 기능을 활성화 또는 비활성화하는 방법에 대해 자세히 알아봅니다.](https://technet.microsoft.com/library/dn454941.aspx)

## <a name="step-5-optional-disable-remember-password-prompt"></a>5단계(선택 사항): "암호 저장" 프롬프트를 사용 하지 않도록 설정

사용자가 액세스 패널 확장을 사용하여 웹 사이트에 로그인하면 Internet Explorer에 “암호를 저장하시겠습니까?”라고 묻는 프롬프트가 다음과 같이 표시될 수 있습니다.

![표시 된 "할까요... 암호 저장" 프롬프트](./media/deploy-access-panel-browser-extension/remember-password-prompt.png)

사용자에게 이 프롬프트가 표시되지 않도록 하려면 아래 단계에 따라서 자동 완성이 암호를 저장하지 않도록 합니다.

1. **그룹 정책 관리 편집기** 창에서 아래 나열된 경로로 이동합니다. 이 구성 설정은 **사용자 구성**에서만 사용할 수 있습니다.

   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`
1. 이름이 **양식의 사용자 이름과 암호에 자동 완성 기능 사용**인 설정을 찾습니다.

   > [!NOTE]
   > 이전 버전의 Active Directory에는 이 설정의 이름이 **Do not allow auto-complete to save passwords**(자동 완성이 암호를 저장하도록 허용 안 함)으로 표시될 수 있습니다. 해당 설정에 대한 구성은 이 자습서에 설명되어 있는 설정마다 다릅니다.

    ![사용자 설정에서이 내용을 찾아야합니다](./media/deploy-access-panel-browser-extension/disable-auto-complete.png)

1. 위 설정을 마우스 오른쪽 단추로 클릭하고 **편집**을 선택합니다.
1. **양식의 사용자 이름과 암호에 자동 완성 기능 사용** 창에서 **사용 안함**을 선택합니다.

    ![자동 완성 기능에서 설정에 대 한 "사용 안 함된" 옵션을 선택](./media/deploy-access-panel-browser-extension/disable-passwords.png)

1. **확인** 을 클릭하여 변경 사항을 적용하고 창을 닫습니다.

사용자는 더 이상 자신의 자격 증명을 저장하거나 자동 완성을 사용하여 이전에 저장된 자격 증명에 액세스할 수 없습니다. 하지만 이 정책은 검색 필드와 같은 다른 유형의 양식 필드에는 자동 완성 기능을 계속 사용하도록 허용합니다.

> [!WARNING]
> 사용자가 자격 증명을 저장하도록 선택한 후에 이 정책을 사용하도록 설정하면, 이 정책은 이미 저장된 자격 증명을 지우지 *않습니다* .

## <a name="step-6-testing-the-deployment"></a>6단계: 배포 테스트

다음 단계에 따라 확장 배포가 성공적으로 이루어졌는지 확인합니다.

1. **컴퓨터 구성**을 사용하여 배포한 경우 [2단계: 그룹 정책 개체 만들기에서 선택한 OU에 속한 클라이언트 머신에 로그인합니다. **사용자 구성**을 사용하여 배포한 경우 해당 OU에 속한 사용자로 로그인해야 합니다.
1. 이 컴퓨터를 사용 하 여 완전히 업데이트 하려면 그룹 정책 변경 내용에 대 한 몇 가지 로그인 걸릴 수 있습니다. 업데이트를 강제 실행하려면 **명령 프롬프트** 창을 열고 터미널 창을 열고 `gpupdate /force` 명령을 실행합니다.
1. 설치를 적용하려면 컴퓨터를 다시 시작해야 합니다. 확장 설치 중에는 부팅 시간이 상대적으로 더 오래 소요될 수 있습니다.
1. 다시 시작한 후 **Internet Explorer**를 엽니다. 창의 오른쪽 위 모퉁이에서 **도구**(기어 아이콘)를 클릭한 다음 **추가 기능 관리**를 선택합니다.
1. **추가 기능 관리** 창에서 **액세스 패널 확장**이 설치되었으며 그 **상태**가 **사용**으로 설정되었는지 확인합니다.

   ![액세스 패널 확장이 설치 되어 있고 사용 하도록 설정 확인](./media/deploy-access-panel-browser-extension/verify-install.png)

## <a name="learn-more"></a>자세한 정보

* [Azure Active Directory로 애플리케이션 액세스 및 Single Sign-On](what-is-single-sign-on.md)
* [Internet Explorer용 액세스 패널 확장 문제 해결](manage-access-panel-browser-extension.md)
