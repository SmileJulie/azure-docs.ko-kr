---
title: 포함 파일
description: 포함 파일
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: ae6d590cdada24638ec2d24c83609b8e6addfaf0
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67182378"
---
## <a name="set-up-your-project"></a>프로젝트 설정

이 섹션에서는 토큰이 필요한 Web API를 쿼리할 수 있도록 Windows Desktop .NET 애플리케이션(XAML)을 *Microsoft에 로그인*과 통합하는 방법을 설명하기 위해 새 프로젝트를 만듭니다.

이 가이드를 사용하여 만든 애플리케이션은 그래프를 호출하는 데 사용된 단추, 화면에 결과를 보여주는 영역 및 로그아웃 단추를 표시합니다.

> [!NOTE]
> 이 샘플의 Visual Studio 프로젝트를 다운로드하고 싶으세요? [프로젝트를 다운로드](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/msal3x.zip)하면 코드 샘플을 실행하기 전에 구성하는 [구성 단계](#register-your-application)를 건너뛸 수 있습니다.
>

애플리케이션을 만들려면 다음을 수행합니다.

1. Visual Studio에서 **파일** > **새로 만들기** > **프로젝트**를 선택합니다.
2. **템플릿**에서 **Visual C#** 을 선택합니다.
3. 사용하고 있는 Visual Studio 버전에 따라 **WPF 앱(.NET Framework)** 을 선택합니다.

## <a name="add-msal-to-your-project"></a>프로젝트에 MSAL 추가

1. Visual Studio에서 **도구** > **NuGet 패키지 관리자**> **패키지 관리자 콘솔**을 선택합니다.
2. 패키지 관리자 콘솔 창에서 다음 Azure PowerShell 명령을 붙여넣습니다.

    ```powershell
    Install-Package Microsoft.Identity.Client -Pre
    ```

    > [!NOTE] 
    > 이 명령은 Microsoft 인증 라이브러리를 설치합니다. MSAL은 Azure Active Directory v2.0으로 보호된 API에 액세스하는 데 사용되는 사용자 토큰의 획득, 캐싱 및 새로 고침을 처리합니다.
    >

## <a name="add-the-code-to-initialize-msal"></a>MSAL을 초기화하는 코드 추가

이 단계에서는 토큰 처리와 같이 MSAL과 상호 작용을 처리하는 클래스를 만들 수 있습니다.

1. *App.xaml.cs* 파일을 열고 MSAL에 대한 참조를 클래스에 추가합니다.

    ```csharp
    using Microsoft.Identity.Client;
    ```
   <!-- Workaround for Docs conversion bug -->

2. 앱 클래스를 다음으로 업데이트합니다.

    ```csharp
    public partial class App : Application
    {
        static App()
        {
            _clientApp = PublicClientApplicationBuilder.Create(ClientId)
                .WithAuthority(AzureCloudInstance.AzurePublic, Tenant)
                .Build();
        }

        // Below are the clientId (Application Id) of your app registration and the tenant information. 
        // You have to replace:
        // - the content of ClientID with the Application Id for your app registration
        // - the content of Tenant by the information about the accounts allowed to sign-in in your application:
        //   - For Work or School account in your org, use your tenant ID, or domain
        //   - for any Work or School accounts, use `organizations`
        //   - for any Work or School accounts, or Microsoft personal account, use `common`
        //   - for Microsoft Personal account, use consumers
        private static string ClientId = "0b8b0665-bc13-4fdc-bd72-e0227b9fc011";

        private static string Tenant = "common";

        private static IPublicClientApplication _clientApp ;

        public static IPublicClientApplication PublicClientApp { get { return _clientApp; } }
    }
    ```

## <a name="create-the-application-ui"></a>애플리케이션 UI 만들기

이 섹션에서는 애플리케이션에서 Microsoft Graph와 같이 보호되는 백 엔드 서버를 쿼리하는 방법을 보여줍니다. 

*MainWindow.xaml* 파일은 프로젝트 템플릿의 일부로 자동으로 생성되어야 합니다. 이 파일을 열고, 애플리케이션의 *\<그리드&gt;* 노드를 다음 코드로 바꿉니다.

```xml
<Grid>
    <StackPanel Background="Azure">
        <StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
            <Button x:Name="CallGraphButton" Content="Call Microsoft Graph API" HorizontalAlignment="Right" Padding="5" Click="CallGraphButton_Click" Margin="5" FontFamily="Segoe Ui"/>
            <Button x:Name="SignOutButton" Content="Sign-Out" HorizontalAlignment="Right" Padding="5" Click="SignOutButton_Click" Margin="5" Visibility="Collapsed" FontFamily="Segoe Ui"/>
        </StackPanel>
        <Label Content="API Call Results" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="ResultText" TextWrapping="Wrap" MinHeight="120" Margin="5" FontFamily="Segoe Ui"/>
        <Label Content="Token Info" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="TokenInfoText" TextWrapping="Wrap" MinHeight="70" Margin="5" FontFamily="Segoe Ui"/>
    </StackPanel>
</Grid>
```
