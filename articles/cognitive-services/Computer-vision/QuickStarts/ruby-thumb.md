---
title: '빠른 시작: 썸네일 생성 - REST, Ruby'
titleSuffix: Azure Cognitive Services
description: 이 빠른 시작에서는 Ruby와 함께 Computer Vision API를 사용하여 이미지에서 썸네일을 생성합니다.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 0d6699bbe9a4f3206424032699060d235e8ffbc4
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604174"
---
# <a name="quickstart-generate-a-thumbnail-using-the-computer-vision-rest-api-and-ruby"></a>빠른 시작: Computer Vision REST API 및 Ruby를 사용하여 썸네일 생성

이 빠른 시작에서는 Computer Vision의 REST API를 사용하여 이미지에서 썸네일을 생성합니다. [썸네일 가져오기](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) 메서드를 사용하여 이미지의 썸네일을 생성할 수 있습니다. 높이와 너비를 지정합니다. 입력 이미지의 가로 세로 비율과 다를 수 있습니다. Computer Vision은 스마트 자르기를 사용하여 관심 영역을 지능적으로 식별하고 해당 영역을 기반으로 자르기 좌표를 생성합니다.

Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services)을 만듭니다.

## <a name="prerequisites"></a>필수 조건

- [Ruby](https://www.ruby-lang.org/en/downloads/) 2.4.x 이상이 설치되어 있어야 합니다.
- Computer Vision에 대한 구독 키가 있어야 합니다. [Cognitive Services 사용해보기](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision)에서 평가판 키를 가져올 수 있습니다. 또는 [Cognitive Services 계정 만들기](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)의 지침에 따라 Computer Vision을 구독하고 키를 가져옵니다.

## <a name="create-and-run-the-sample"></a>샘플 만들기 및 실행

샘플을 만들고 실행하려면 다음 단계를 수행합니다.

1. 다음 코드를 텍스트 편집기에 복사합니다.
1. 필요한 경우 코드에서 다음과 같이 변경합니다.
    1. `<Subscription Key>`를 구독 키로 바꿉니다.
    1. 필요한 경우 `https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/analyze`를, 구독 키를 가져온 Azure 지역의 [썸네일 가져오기](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) 메서드에 대한 엔드포인트 URL로 바꿉니다.
    1. 필요에 따라 `https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\`를 썸네일을 생성하려는 다른 이미지의 URL로 바꿉니다.
1. 코드를 `.rb` 확장명의 파일로 저장합니다. 예: `get-thumbnail.rb`
1. 명령 프롬프트 창을 엽니다.
1. 프롬프트에서 `ruby` 명령을 사용하여 샘플을 실행합니다. 예: `ruby get-thumbnail.rb`

```ruby
require 'net/http'

# You must use the same location in your REST call as you used to get your
# subscription keys. For example, if you got your subscription keys from westus,
# replace "westcentralus" in the URL below with "westus".
uri = URI('https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail')
uri.query = URI.encode_www_form({
    # Request parameters
    'width' => '100',
    'height' => '100',
    'smartCropping' => 'true'
})

request = Net::HTTP::Post.new(uri.request_uri)

# Request headers
# Replace <Subscription Key> with your valid subscription key.
request['Ocp-Apim-Subscription-Key'] = '<Subscription Key>'
request['Content-Type'] = 'application/json'

request.body =
    "{\"url\": \"https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/" +
    "Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\"}"

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request(request)
end

#puts response.body
```

## <a name="examine-the-response"></a>응답 검사

성공적인 응답이 썸네일에 대한 이미지 데이터를 나타내는 이진 데이터로 반환됩니다. 요청이 실패하면 응답이 콘솔 창에 표시됩니다. 실패된 요청에 대한 응답은 무엇이 잘못되었는지 확인할 수 있도록 오류 코드 및 메시지를 포함합니다.

## <a name="next-steps"></a>다음 단계

이미지를 분석하고, 유명인 및 랜드마크를 검색하고, 썸네일을 만들고, 인쇄 및 필기 텍스트를 추출하는 데 사용되는 Computer Vision API를 살펴봅니다. Computer Vision API를 사용하여 신속하게 실험하려면 [API 테스트 콘솔 열기](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console)를 사용하세요.

> [!div class="nextstepaction"]
> [Computer Vision API 살펴보기](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
