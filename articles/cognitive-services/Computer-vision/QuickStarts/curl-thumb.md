---
title: '빠른 시작: 썸네일 생성 - REST, cURL'
titleSuffix: Azure Cognitive Services
description: 이 빠른 시작에서는 cURL과 함께 Computer Vision API를 사용하여 이미지에서 썸네일을 생성합니다.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 55fa5ffbfe8cdb266340df1a407968d542b36c1a
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67605999"
---
# <a name="quickstart-generate-a-thumbnail-using-the-computer-vision-rest-api-and-curl"></a>빠른 시작: Computer Vision REST API 및 cURL을 사용하여 썸네일 생성

이 빠른 시작에서는 Computer Vision의 REST API를 사용하여 이미지에서 썸네일을 생성합니다. 원하는 높이와 너비를 지정합니다. 입력 이미지의 가로 세로 비율에서 다를 수 있습니다. Computer Vision은 스마트 자르기를 사용하여 관심 영역을 지능적으로 식별하고 해당 영역을 기준으로 자르기 좌표를 생성합니다.

Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services)을 만듭니다.

## <a name="prerequisites"></a>필수 조건

- [cURL](https://curl.haxx.se/windows)이 있어야 합니다.
- Computer Vision에 대한 구독 키가 있어야 합니다. [Cognitive Services 사용해보기](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision)에서 평가판 키를 가져올 수 있습니다. 또는 [Cognitive Services 계정 만들기](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)의 지침에 따라 Computer Vision을 구독하고 키를 가져옵니다.

## <a name="get-thumbnail-request"></a>썸네일 가져오기 요청

[썸네일 가져오기 메서드](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb)를 사용하여 이미지의 썸네일을 생성할 수 있습니다.

샘플을 실행하려면 다음 단계를 수행합니다.

1. 다음 코드를 편집기에 복사합니다.
1. `<Subscription Key>`를 유효한 구독 키로 바꿉니다.
1. `<File>`을 썸네일을 저장할 경로와 파일 이름으로 바꿉니다.
1. 필요한 경우 요청 URL(`https://westcentralus.api.cognitive.microsoft.com/vision/v2.0`)을 변경하여 구독 키를 가져온 위치를 사용합니다.
1. 필요에 따라 분석할 이미지(`{\"url\":\"...`)를 변경합니다.
1. cURL이 설치된 컴퓨터에서 명령 창을 엽니다.
1. 창에 코드를 붙여넣고 명령을 실행합니다.

>[!NOTE]
>구독 키를 가져오는 데 사용한 것과 동일한 위치를 REST 호출에서 사용해야 합니다. 예를 들어, westus에서 구독 키를 받은 경우 아래 URL의 "westcentralus"를 "westus"로 바꿉니다.

## <a name="create-and-run-the-sample-command"></a>샘플 명령 만들기 및 실행

샘플을 만들고 실행하려면 다음 단계를 수행합니다.

1. 다음 명령을 텍스트 편집기에 복사합니다.
1. 필요한 경우 명령에서 다음 내용을 변경합니다.
    1. `<subscriptionKey>`의 값을 구독 키로 바꿉니다.
    1. `<thumbnailFile>`의 값을 썸네일을 저장할 파일의 경로 및 이름으로 바꿉니다.
    1. 필요한 경우 요청 URL(`https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail`)을 구독 키를 가져온 Azure 지역의 [썸네일 가져오기](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) 메서드에 대한 엔드포인트 URL로 바꿉니다.
    1. 필요한 경우 요청 본문의 이미지 URL(`https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\`)을 썸네일을 생성할 다른 이미지의 URL로 변경합니다.
1. 명령 프롬프트 창을 엽니다.
1. 텍스트 편집기에서 명령 프롬프트 창으로 명령을 붙여넣은 후 명령을 실행합니다.

    ```bash
    curl -H "Ocp-Apim-Subscription-Key: <subscriptionKey>" -o <thumbnailFile> -H "Content-Type: application/json" "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail?width=100&height=100&smartCropping=true" -d "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\"}"
    ```

## <a name="examine-the-response"></a>응답 검사

성공한 응답은 `<thumbnailFile>`에 지정된 파일에 썸네일 이미지를 기록합니다. 요청이 실패하면 무엇이 잘못되었는지 확인할 수 있도록 응답에 오류 코드 및 메시지가 포함됩니다. 요청이 성공한 것으로 보이지만 생성된 썸네일이 유효한 이미지 파일이 아닌 경우, 구독 키가 유효하지 않을 수 있습니다.

## <a name="next-steps"></a>다음 단계

이미지를 분석하고, 유명인 및 랜드마크를 검색하고, 썸네일을 만들고, 인쇄 및 필기 텍스트를 추출하는 방법에 대한 Computer Vision API를 살펴봅니다. Computer Vision API를 사용하여 신속하게 실험하려면 [API 테스트 콘솔 열기](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console)를 사용하세요.

> [!div class="nextstepaction"]
> [Computer Vision API 살펴보기](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
