---
title: Visual Studio Code를 사용하여 Azure에서 첫 번째 함수 만들기
description: Visual Studio Code에서 Azure Functions 확장을 사용하여 간단한 HTTP 트리거 함수를 Azure에 만들고 게시합니다.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure 함수, 함수, 이벤트 처리, 계산, 서버리스 아키텍처
ms.service: azure-functions
ms.devlang: multiple
ms.topic: quickstart
ms.date: 06/25/2019
ms.author: glenga
ms.custom: mvc, devcenter
ms.openlocfilehash: fcf9f1d6420dbbde359d386bc3b67a0866aca30d
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444615"
---
# <a name="create-your-first-function-using-visual-studio-code"></a>Visual Studio Code를 사용하여 첫 번째 함수 만들기

Azure Functions를 사용하면 먼저 VM을 만들거나 웹 애플리케이션을 게시하지 않고도 [서버를 사용하지 않는](https://azure.microsoft.com/solutions/serverless/) 환경에서 코드를 실행할 수 있습니다.

이 문서에서는 [Visual Studio Code용 Azure Functions 확장]을 사용하여 Microsoft Visual Studio Code를 통해 로컬 컴퓨터에 “hello world” 함수를 만들고 테스트하는 방법을 알아봅니다. 그런 다음, Visual Studio Code에서 함수 코드를 Azure에 게시합니다.

![Visual Studio 프로젝트의 Azure Functions 코드](./media/functions-create-first-function-vs-code/functions-vscode-intro.png)

이 확장은 현재 C#, JavaScript 및 Java 함수를  지원하며, Python 지원은 현재 미리 보기로 제공됩니다. 이 문서와 다음 문서에 나오는 단계는 JavaScript 및 C# 함수만 지원합니다. Visual Studio Code를 사용하여 Python 함수를 만들고 게시하는 방법에 대해 알아보려면 [Azure Functions에 Python 배포](https://code.visualstudio.com/docs/python/tutorial-azure-functions)를 참조하세요. Visual Studio Code를 사용하여 PowerShell 함수를 만들고 게시하는 방법에 대해 알아보려면 [Azure에서 첫 번째 PowerShell 함수 만들기](functions-create-first-function-powershell.md)를 참조하세요. 

확장은 현재 미리 보기로 제공되고 있습니다. 자세한 내용은 [Visual Studio Code용 Azure Functions 확장] 확장 페이지를 참조하세요.

## <a name="prerequisites"></a>필수 조건

이 빠른 시작을 완료하려면 다음이 필요합니다.

* [지원되는 플랫폼](https://code.visualstudio.com/docs/supporting/requirements#_platforms) 중 하나에 [Visual Studio Code](https://code.visualstudio.com/)를 설치합니다.

* [Azure Functions Core Tools](functions-run-local.md#v2)의 버전 2.x를 설치합니다.

* 선택한 언어에 대한 특정 요구 사항을 설치합니다.

    | 언어 | 요구 사항 |
    | -------- | --------- |
    | **C#** | [C# 확장](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)  |
    | **JavaScript** | [Node.js](https://nodejs.org/)<sup>*</sup> | 
 
    <sup>*</sup>활성 LTS 및 유지 관리 LTS 버전(8.11.1 및 10.14.1 권장).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [functions-install-vs-code-extension](../../includes/functions-install-vs-code-extension.md)]

[!INCLUDE [functions-create-function-app-vs-code](../../includes/functions-create-function-app-vs-code.md)]

[!INCLUDE [functions-run-function-test-local-vs-code](../../includes/functions-run-function-test-local-vs-code.md)]

함수가 로컬 컴퓨터에서 제대로 실행되는지 확인한 후에 해당 프로젝트를 Azure에 게시해야 합니다.

[!INCLUDE [functions-sign-in-vs-code](../../includes/functions-sign-in-vs-code.md)]

[!INCLUDE [functions-publish-project-vscode](../../includes/functions-publish-project-vscode.md)]

## <a name="run-the-function-in-azure"></a>Azure에서 함수 실행

1. **출력** 패널에서 HTTP 트리거의 URL을 복사합니다. 이전처럼 이 URL 마지막에 `?name=<yourname>` 쿼리 문자열을 추가하고 요청을 실행해야 합니다.

    HTTP 트리거 함수를 호출하는 URL은 다음 형식이어야 합니다.

        http://<functionappname>.azurewebsites.net/api/<functionname>?name=<yourname> 

1. HTTP 요청에 대한 이러한 새 URL을 브라우저의 주소 표시줄에 붙여넣습니다. 다음은 함수에서 반환된 원격 GET 요청에 대한 브라우저의 응답을 보여 줍니다. 

    ![브라우저의 함수 응답](./media/functions-create-first-function-vs-code/functions-test-remote-browser.png)

## <a name="next-steps"></a>다음 단계

Visual Studio Code를 사용하여 간단한 HTTP 트리거 함수가 있는 함수 앱을 만들었습니다. 다음 문서에서는 출력 바인딩을 추가하여 해당 함수를 확장합니다. 이 바인딩은 HTTP 요청의 문자열을 Azure Queue Storage 큐의 메시지에 씁니다. 다음 문서에서는 만든 리소스 그룹을 제거하여 이러한 새 Azure 리소스를 정리하는 방법도 보여 줍니다.

> [!div class="nextstepaction"]
> [함수에 Azure Storage 큐 바인딩 추가](functions-add-output-binding-storage-queue-vs-code.md)

[Azure Functions Core Tools]: functions-run-local.md
[Visual Studio Code용 Azure Functions 확장]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions
