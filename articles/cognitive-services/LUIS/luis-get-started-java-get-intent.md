---
title: 의도 가져오기, Java
titleSuffix: Language Understanding - Azure Cognitive Services
description: 이 Java 빠른 시작에서는 사용 가능한 공용 LUIS 앱을 통해 대화형 텍스트에서 사용자의 의도를 판단합니다.
author: diberry
manager: nitinme
services: cognitive-services
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 07/16/2019
ms.author: diberry
ms.openlocfilehash: 42240c7b45029684e51c25419eab7f4378785a4d
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68276150"
---
# <a name="quickstart-get-intent-using-java"></a>빠른 시작: Java를 사용하여 의도 가져오기

이 빠른 시작에서는 발언을 LUIS 엔드포인트로 전달하고 의도와 엔터티를 다시 가져옵니다.

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>필수 조건

* [JDK SE](https://aka.ms/azure-jdks)(Java Development Kit, Standard Edition)
* [Visual Studio Code](https://code.visualstudio.com/) 또는 선호하는 IDE
* 공용 앱 ID: df67dcdb-c37d-46af-88e1-8b97951ca1c2

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>LUIS 키 가져오기

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>브라우저를 사용하여 의도 가져오기

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent-programmatically"></a>프로그래밍 방식으로 의도 가져오기

Java를 사용하여 이전 단계의 브라우저 창에서 본 것과 동일한 결과에 액세스할 수 있습니다. 프로젝트에 Apache 라이브러리를 꼭 추가해야 합니다.

1. 다음 코드를 복사하여 `LuisGetRequest.java` 파일에 클래스를 만듭니다.

   [!code-java[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/java/call-endpoint.java)]

2. `YOUR-KEY` 변수 값을 LUIS 키로 바꿉니다.

3. 명령줄 `javac -cp .;<FILE_PATH>\* LuisGetRequest.java`에서 파일 경로를 바꾸고 java 프로그램을 컴파일합니다.

4. 명령줄 `java -cp .;<FILE_PATH>\* LuisGetRequest.java`에서 파일 경로를 바꾸고 애플리케이션을 실행합니다. 브라우저 창에서 앞서 본 것과 동일한 JSON을 표시합니다.

    ![콘솔 창에서는 LUIS의 JSON 결과를 표시합니다.](./media/luis-get-started-java-get-intent/console-turn-on.png)
    
## <a name="luis-keys"></a>LUIS 키

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>리소스 정리

Java 파일/프로젝트 폴더를 삭제합니다.

## <a name="next-steps"></a>다음 단계
> [!div class="nextstepaction"]
> [발언 추가](luis-get-started-java-add-utterance.md)
