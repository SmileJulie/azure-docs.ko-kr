---
title: Azure 함수 앱 설정 구성 | Microsoft Docs
description: Azure 함수 앱 설정을 구성하는 방법에 알아봅니다.
services: ''
documentationcenter: .net
author: ggailey777
manager: jeconnoc
ms.assetid: 81eb04f8-9a27-45bb-bf24-9ab6c30d205c
ms.service: azure-functions
ms.topic: conceptual
ms.date: 03/28/2018
ms.author: glenga
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: 92ca09040836dfc55a9d709b12a0ee01192d6bac
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65957400"
---
# <a name="how-to-manage-a-function-app-in-the-azure-portal"></a>Azure Portal에서 함수 앱을 관리하는 방법 

Azure Functions에서 함수 앱은 개별 함수에 대한 실행 컨텍스트를 제공합니다. 함수 앱 동작은 지정된 함수 앱에서 호스트하는 모든 함수에 적용됩니다. 이 항목에서는 Azure Portal에서 함수 앱을 구성 및 관리하는 방법을 설명합니다.

시작하려면 [Azure Portal](https://portal.azure.com)로 이동한 후 Azure 계정으로 로그인합니다. 포털 맨 위에 있는 검색 표시줄에 함수 앱의 이름을 입력하고 목록에서 선택합니다. 함수 앱을 선택하면 다음 페이지가 표시됩니다.

![Azure Portal의 함수 앱 개요](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png)

특히 개요 페이지에서 함수 앱을 관리 하는 데 필요한 모든 이동할 수 있습니다 합니다 **[응용 프로그램 설정](#settings)** 하 고 **[플랫폼기능](#platform-features)** .

## <a name="settings"></a>애플리케이션 설정

합니다 **응용 프로그램 설정** 탭에는 함수 앱에서 사용 되는 설정을 유지 합니다.

![Azure portal에서 함수 앱 설정입니다.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

이러한 설정은 암호화 저장 됩니다 하 고 선택 해야 합니다 **값을 표시** 포털에서 값을 확인할 수 있습니다.

설정을 추가 하려면 **새 응용 프로그램 설정을** 새 키-값 쌍을 추가 합니다.

[!INCLUDE [functions-environment-variables](../../includes/functions-environment-variables.md)]

로컬 함수 앱을 개발할 때 이러한 값 local.settings.json 프로젝트 파일에 유지 됩니다.

## <a name="platform-features"></a>플랫폼 기능

![함수 앱 플랫폼 기능 탭.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-features-tab.png)

함수 앱은 Azure App Service 플랫폼에서 실행되고 유지 관리됩니다. 따라서 함수 앱은 Azure의 핵심 웹 호스팅 플랫폼 기능 대부분에 액세스할 수 있습니다. **플랫폼 기능** 탭에서는 함수 앱에서 사용할 수 있는 App Service 플랫폼의 많은 기능에 액세스할 수 있습니다. 

> [!NOTE]
> 함수 앱이 소비 호스팅 계획에서 실행될 때 모든 App Service 기능을 사용할 수 있는 것은 아닙니다.

이 항목의 나머지 부분에서는 Functions에 유용한 Azure Portal의 다음과 같은 App Service 기능을 중점적으로 설명합니다.

+ [App Service 편집기](#editor)
+ [콘솔](#console)
+ [고급 도구(Kudu)](#kudu)
+ [배포 옵션](#deployment)
+ [CORS](#cors)
+ [인증](#auth)
+ [API 정의](#swagger)

App Service 설정을 사용하는 방법에 대한 자세한 내용은 [Azure App Service 설정 구성](../app-service/configure-common.md)을 참조하세요.

### <a name="editor"></a>App Service 편집기

| | |
|-|-|
| ![함수 앱 App Service 편집기](./media/functions-how-to-use-azure-function-app-settings/function-app-appsvc-editor.png)  | App Service 편집기는 JSON 구성 파일과 코드 파일을 둘 다 수정하는 데 사용할 수 있는 포털 내 고급 편집기입니다. 이 옵션을 선택하면 기본 편집기와 함께 별도의 브라우저 탭이 실행됩니다. 이를 통해 Git 리포지토리와 통합하고 코드를 실행 및 디버깅하며 함수 앱 설정을 수정할 수 있습니다. 이 편집기는 기본 함수 앱 블레이드와 비교할 때 함수에 대해 고급 개발 환경을 제공합니다.    |

![App Service 편집기](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

### <a name="console"></a>콘솔

| | |
|-|-|
| ![Azure Portal의 함수 앱 콘솔](./media/functions-how-to-use-azure-function-app-settings/function-app-console.png) | 포털 내 콘솔은 명령줄에서 함수 앱과 상호 작용하려는 경우에 이상적인 개발자 도구입니다. 일반적인 명령에는 배치 파일 및 스크립트 실행과 함께 디렉터리 및 파일의 생성 및 탐색이 포함됩니다. |

![함수 앱 콘솔](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

### <a name="kudu"></a>고급 도구(Kudu)

| | |
|-|-|
| ![Azure Portal의 함수 앱 Kudu](./media/functions-how-to-use-azure-function-app-settings/function-app-advanced-tools.png) | App Service용 고급 도구(Kudu라고도 함)를 사용하면 함수 앱의 고급 관리 기능에 액세스할 수 있습니다. Kudu에서 시스템 정보, 앱 설정, 환경 변수, 사이트 확장, HTTP 헤더 및 서버 변수를 관리할 수 있습니다. `https://<myfunctionapp>.scm.azurewebsites.net/`과 같은 함수 앱에 대한 SCM 엔드포인트로 이동하여 **Kudu**를 시작할 수도 있습니다. |

![Kudu 구성](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)


### <a name="a-namedeploymentdeployment-options"></a><a name="deployment">배포 옵션

| | |
|-|-|
| ![Azure Portal의 함수 앱 배포 옵션](./media/functions-how-to-use-azure-function-app-settings/function-app-deployment-source.png) | Functions를 사용하면 로컬 컴퓨터에서 함수 코드를 개발할 수 있습니다. 그런 다음 Azure에 로컬 함수 앱 프로젝트를 업로드할 수 있습니다. 기존 FTP 업로드 외에도 Functions를 사용하면 GitHub, Azure DevOps, Dropbox, Bitbucket 등과 같은 인기 있는 연속 통합 솔루션을 사용하여 함수 앱을 배포할 수 있습니다. 자세한 내용은 [Azure Functions에 대한 연속 배포](functions-continuous-deployment.md)를 참조하세요. FTP 또는 로컬 Git을 사용하여 수동으로 업로드하려면 [배포 자격 증명을 구성](functions-continuous-deployment.md#credentials)해야 합니다. |


### <a name="cors"></a>CORS

| | |
|-|-|
| ![Azure Portal의 함수 앱 CORS](./media/functions-how-to-use-azure-function-app-settings/function-app-cors.png) | 서비스의 악의적인 코드 실행을 방지하기 위해 App Service는 외부 원본의 함수 앱에 대한 호출을 차단합니다. Functions는 CORS(원본 간 리소스 공유)를 지원하여 함수가 원격 요청을 수락할 수 있는 허용 원본을 나타내는 “허용 목록"을 정의할 수 있도록 합니다.  |

![함수 앱 CORS 구성](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

### <a name="auth"></a>인증

| | |
|-|-|
| ![Azure Portal의 함수 앱 인증](./media/functions-how-to-use-azure-function-app-settings/function-app-authentication.png) | 함수가 HTTP 트리거를 사용하는 경우 먼저 호출이 인증되도록 요구할 수 있습니다. App Service는 Facebook, Microsoft 및 Twitter 같은 소셜 공급자를 사용하는 Azure Active Directory 인증 및 로그인을 지원합니다. 특정 인증 공급자를 구성하는 방법에 대한 자세한 내용은 [Azure App Service 인증 개요](../app-service/overview-authentication-authorization.md)를 참조하세요. |

![함수 앱에 대한 인증 구성](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)


### <a name="swagger"></a>API 정의

| | |
|-|-|
| ![Azure Portal의 함수 앱 API swagger 정의](./media/functions-how-to-use-azure-function-app-settings/function-app-api-definition.png) | Functions는 클라이언트가 HTTP에서 트리거한 함수를 더 쉽게 사용할 수 있도록 하는 Swagger를 지원합니다. Swagger를 사용하여 API 정의를 만드는 방법에 대한 자세한 내용은 [Azure App Service에서 CORS를 통해 RESTful API 호스팅](../app-service/app-service-web-tutorial-rest-api.md)을 방문하세요. 또한 Functions 프록시를 사용하여 여러 함수에 대해 단일 API 화면을 정의할 수도 있습니다. 자세한 내용은 [Azure Functions 프록시 사용](functions-proxies.md)을 참조하세요. |

![함수 앱 API 구성](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-apidef.png)



## <a name="next-steps"></a>다음 단계

+ [Azure App Service 설정 구성](../app-service/configure-common.md)
+ [Azure Functions에 대한 연속 배포](functions-continuous-deployment.md)



