---
title: '빠른 시작: 문장 길이 가져오기, Python - Translator Text API'
titleSuffix: Azure Cognitive Services
description: 이 빠른 시작에서는 Python 및 Translator Text REST API를 사용하여 문장 길이(문자 수)를 확인하는 방법을 알아봅니다.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: 9408e1b30dc183784f427c61138a0a46e49bc36a
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67704371"
---
# <a name="quickstart-use-the-translator-text-api-to-determine-sentence-length-using-python"></a>빠른 시작: Translator Text API를 사용하여 Python을 통해 문장 길이 확인

이 빠른 시작에서는 Python 및 Translator Text REST API를 사용하여 문장 길이(문자 수)를 확인하는 방법을 알아봅니다.

이 빠른 시작에는Translator Text 리소스와 함께 [Azure Cognitive Services 계정](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)이 필요합니다. 계정이 없는 경우 [평가판](https://azure.microsoft.com/try/cognitive-services/)을 사용하여 구독 키를 가져올 수 있습니다.

>[!TIP]
> 모든 코드를 한꺼번에 볼 수 있도록 [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Python)에 이 샘플의 소스 코드가 제공됩니다.

## <a name="prerequisites"></a>필수 조건

이 빠른 시작에는 다음이 필요합니다.

* Python 2.7.x 또는 3.x
* Translator Text에 대한 Azure 구독 키

## <a name="create-a-project-and-import-required-modules"></a>프로젝트 만들기 및 필요한 모듈 가져오기

즐겨찾는 IDE 또는 편집기를 사용하여 새 Python 프로젝트를 만듭니다. 그런 다음, 아래 코드 조각을 `sentence-length.py`라는 파일의 프로젝트에 복사합니다.

```python
# -*- coding: utf-8 -*-
import os
import requests
import uuid
import json
```

> [!NOTE]
> 이러한 모듈을 사용하지 않았다면 프로그램을 실행하기 전에 설치해야 합니다. 이러한 패키지를 설치하려면 `pip install requests uuid`를 실행합니다.

첫 번째 주석은 Python 해석기가 UTF-8 인코딩을 사용하라고 알려줍니다. 그런 다음, 필요한 모듈을 가져와 환경 변수에서 구독 키를 읽고 http 요청을 구성한 다음, 고유한 ID를 만들고 Translator Text API에서 반환하는 JSON 응답을 처리합니다.

## <a name="set-the-subscription-key-base-url-and-path"></a>구독 키, 기본 url 및 경로를 설정합니다.

이 샘플에서는 환경 변수 `TRANSLATOR_TEXT_KEY`에서 Translator Text 구독 키를 읽으려고 합니다. 환경 변수를 잘 모르는 경우에는 `subscriptionKey`를 문자열로 설정하고 조건문을 주석으로 처리할 수 있습니다.

이 코드를 프로젝트에 복사합니다.

```python
# Checks to see if the Translator Text subscription key is available
# as an environment variable. If you are setting your subscription key as a
# string, then comment these lines out.
if 'TRANSLATOR_TEXT_KEY' in os.environ:
    subscriptionKey = os.environ['TRANSLATOR_TEXT_KEY']
else:
    print('Environment variable for TRANSLATOR_TEXT_KEY is not set.')
    exit()
# If you want to set your subscription key as a string, uncomment the line
# below and add your subscription key.
#subscriptionKey = 'put_your_key_here'
```

Translator Text 글로벌 엔드포인트가 `base_url`로 설정되어 있습니다. `path`는 `breaksentence` 루트를 설정하며 API의 버전 3을 실행하기 원한다는 것을 식별합니다.

이 샘플의 `params`는 제공된 텍스트의 언어를 설정하는 데 사용됩니다. `breaksentence` 경로에는 `params`가 필요하지 않습니다. API는 요청에서 제외될 경우 제공된 텍스트의 언어를 감지하려고 하고, 응답에 신뢰도 점수와 이 정보를 제공합니다.

>[!NOTE]
> 엔드포인트, 루트 및 요청 매개 변수에 대한 자세한 내용은 [Translator Text API 3.0: 언어](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-break-sentence)를 참조하세요.

```python
base_url = 'https://api.cognitive.microsofttranslator.com'
path = '/breaksentence?api-version=3.0'
params = '&language=en'
constructed_url = base_url + path + params
```

## <a name="add-headers"></a>헤더 추가

요청을 인증하는 가장 쉬운 방법은 구독 키에서 `Ocp-Apim-Subscription-Key` 헤더로 전달하는 것이며, 이 샘플에서 이 방법을 사용할 것입니다. 대안으로, 액세스 토큰에 구독 키를 대체하고 `Authorization` 헤더로서 액세스 토큰을 전달하여 요청의 유효성을 검사할 수 있습니다. 자세한 내용은 [인증](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication)을 참조하세요.

이 코드 조각을 프로젝트에 복사합니다.

```python
headers = {
    'Ocp-Apim-Subscription-Key': subscriptionKey,
    'Content-type': 'application/json',
    'X-ClientTraceId': str(uuid.uuid4())
}
```

Cognitive Services 다중 서비스 구독을 사용하는 경우 요청 매개 변수에 `Ocp-Apim-Subscription-Region`도 포함해야 합니다. [다중 서비스 구독을 사용한 인증에 대해 자세히 알아봅니다](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="create-a-request-to-determine-sentence-length"></a>문장 길이를 확인하는 요청 만들기

길이를 확인하려는 문장 정의:

```python
# You can pass more than one object in body.
body = [{
    'text': 'How are you? I am fine. What did you do today?'
}]
```

다음으로, `requests` 모듈을 사용하는 POST 요청을 만듭니다. 세 가지 인수, 즉 잘려진 URL, 요청 헤더 및 요청 본문을 사용합니다.

```python
request = requests.post(constructed_url, headers=headers, json=body)
response = request.json()
```

## <a name="print-the-response"></a>응답 출력

마지막 단계는 결과를 인쇄하는 것입니다. 이 코드 조각은 키를 정렬하고 들여쓰기를 설정하며 항목 및 키 구분 기호를 선언하여 결과를 꾸밉니다.

```python
print(json.dumps(response, sort_keys=True, indent=4,
                 ensure_ascii=False, separators=(',', ': ')))
```

## <a name="put-it-all-together"></a>모든 요소 결합

이것으로, Translator Text API를 호출하여 JSON 응답을 반환하는 간단한 프로그램이 만들어집니다. 이제 프로그램을 실행해 보겠습니다.

```console
python sentence-length.py
```

코드를 우리 것과 비교하고 싶다면 전체 샘플은 [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Python)에 있습니다.

## <a name="sample-response"></a>샘플 응답

```json
[
    {
        "sentLen": [
            13,
            11,
            22
        ]
    }
]
```

## <a name="clean-up-resources"></a>리소스 정리

구독 키를 프로그램으로 하드 코딩한 경우 이 빠른 시작을 마치면 구독 키를 반드시 제거하세요.

## <a name="next-steps"></a>다음 단계

Translator Text API로 할 수 있는 모든 것에 대해 알아보려면 API 참조를 살펴보세요.

> [!div class="nextstepaction"]
> [API 참조](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)

## <a name="see-also"></a>참고 항목

Translator Text API를 사용하여 다음을 수행하는 방법을 알아봅니다.

* [텍스트 번역](quickstart-python-translate.md)
* [텍스트 음역](quickstart-python-transliterate.md)
* [입력으로 언어 식별](quickstart-python-detect.md)
* [대체 번역 가져오기](quickstart-python-dictionary.md)
* [지원되는 언어 목록 가져오기](quickstart-python-languages.md)
