---
title: 포함 파일
description: 포함 파일
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 05/31/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 88a4fe8b9f0b477ed851a03742a9957c08b7cbf0
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455171"
---
## <a name="run-the-function-locally"></a>로컬에서 함수 실행

Azure Functions Core Tools를 사용하면 로컬 개발 컴퓨터에서 Azure Functions 프로젝트를 실행할 수 있습니다.

1. 함수를 테스트하려면 함수 코드에 중단점을 설정하고 F5 키를 눌러 함수 앱 프로젝트를 시작합니다. 핵심 도구의 출력이 **터미널** 패널에 표시됩니다.

1. **터미널** 패널에서 HTTP 트리거 함수의 URL 엔드포인트를 복사합니다. 이 URL에는 `code` 쿼리 매개 변수에 전달되는 함수 키가 포함되어 있습니다.

    ![Azure 로컬 출력](./media/functions-run-function-test-local-vs-code/functions-vscode-f5.png)

1. HTTP 요청에 대한 URL을 브라우저의 주소 표시줄에 붙여 넣습니다. 이 URL에 쿼리 문자열 `?name=<yourname>`을 추가하고 요청을 실행합니다. 중단점이 적중되는 경우 실행이 일시 중지됩니다.

1. 계속 실행하는 경우 다음과 같이 GET 요청에 대한 응답이 브라우저에 표시됩니다.

    ![브라우저의 localhost 함수 응답](./media/functions-run-function-test-local-vs-code/functions-test-local-browser.png)

1. 디버깅을 중지하려면 Shift+F5를 누릅니다.