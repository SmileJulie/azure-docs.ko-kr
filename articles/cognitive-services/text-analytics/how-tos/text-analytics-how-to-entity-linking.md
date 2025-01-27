---
title: 텍스트 분석 API에서 엔터티 인식 사용
titleSuffix: Azure Cognitive Services
description: 텍스트 분석 REST API를 사용하여 엔터티를 인식하는 방법을 알아봅니다.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 04/16/2019
ms.author: aahi
ms.openlocfilehash: ff4f9af82024e9d39ad89a39bcb2fe4130de9101
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67304188"
---
# <a name="how-to-use-named-entity-recognition-in-text-analytics"></a>Text Analytics에서 명명된 엔터티 인식 사용 방법

[명명된 엔터티 인식 API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)는 구조화되지 않은 텍스트를 받아서 각 JSON 문서에 대해 웹(Wikipedia 및 Bing)의 추가 정보에 대한 링크를 포함하는 명확화된 엔터티의 목록을 반환합니다. 

## <a name="entity-linking-and-named-entity-recognition"></a>엔터티 링크 및 명명된 엔터티 인식

텍스트 분석의 `entities` 끝점은 명명된 엔터티 인식(NER) 및 엔터티 연결을 모두 지원합니다.

### <a name="entity-linking"></a>엔터티 링크하기
엔터티 링크 설정은 텍스트에서 찾은 엔터티의 ID를 식별하고 명확하게 구분하는 기능입니다(예: "Mars"가 행성 또는 로마의 전쟁의 신으로 사용되고 있는지 확인). 이 프로세스를 수행하려면 인식된 엔터티가 연결되는 기술 자료가 있어야 합니다. Wikipedia가 `entities` 끝점 텍스트 분석의 기술 자료로 사용됩니다.

### <a name="named-entity-recognition-ner"></a>NER(명명된 엔터티 인식)
NER(명명된 엔터티 인식)은 텍스트의 다른 엔터티를 식별하고 미리 정의된 클래스로 분류하는 기능입니다. 지원되는 엔터티 클래스는 다음과 같습니다.

Text Analytics에 [버전 2.1](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634), 엔터티 연결 및 명명 된 엔터티 인식 (NER)를 모두 여러 언어에 대해 사용할 수 있습니다. 참조 된 [언어 지원](../language-support.md#sentiment-analysis-key-phrase-extraction-and-named-entity-recognition) 자세한 문서. 

### <a name="language-support"></a>언어 지원

여러 언어로 엔터티 링크 설정을 사용하려면 각 언어의 해당하는 기술 자료를 사용해야 합니다. Text Analytics의 엔터티 링크 설정에서 이는 `entities` 끝점에서 지원하는 각 언어가 해당 언어의 Wikipedia 자료에 연결됨을 의미합니다. 자료 크기는 언어마다 다르기 때문에 엔터티 링크 설정 기능의 재현율도 각기 다릅니다.

## <a name="supported-types-for-named-entity-recognition"></a>명명된 엔터티 인식의 지원되는 유형

| 유형  | 하위 유형 | 예 |
|:-----------   |:------------- |:---------|
| 사람        | 해당 없음\*         | "Jeff", "Bill Gates"     |
| Location      | 해당 없음\*         | "Redmond, Washington", "Paris"  |
| 조직  | 해당 없음\*         | "Microsoft"   |
| 수량      | Number        | "6", "six"     | 
| 수량      | 백분율    | "50%", "fifty percent"| 
| 수량      | 서수       | "2nd", "second"     | 
| 수량      | NumberRange   | "4 to 8"     | 
| 수량      | Age           | "90 day old", "30 years old"    | 
| 수량      | 통화      | "$10.99"     | 
| 수량      | 차원     | "10 miles", "40 cm"     | 
| 수량      | 온도   | "32 degrees"    |
| DateTime      | 해당 없음\*         | "6:30PM February 4, 2012"      | 
| DateTime      | Date          | "May 2nd, 2017", "05/02/2017"   | 
| DateTime      | Time          | "8am", "8:00"  | 
| DateTime      | DateRange     | "May 2nd to May 5th"    | 
| DateTime      | TimeRange     | "6pm to 7pm"     | 
| DateTime      | 기간      | "1 minute and 45 seconds"   | 
| DateTime      | 집합           | "every Tuesday"     | 
| DateTime      | TimeZone      |    | 
| URL           | 해당 없음\*         | "https:\//www.bing.com"    |
| Email         | 해당 없음\*         | "support@contoso.com" |

\* 입력 및 추출된 엔터티에 따라, 특정 엔터티에 `SubType`이 없을 수 있습니다.  나열된 모든 지원되는 엔터티 유형은 영어, 중국어 간체, 프랑스어, 독일어 및 스페인어에서만 사용할 수 있습니다.



## <a name="preparation"></a>준비

다음 형식의 JSON 문서가 있어야 합니다. ID, 텍스트, 언어

현재 지원되는 언어는 [이 목록](../text-analytics-supported-languages.md)을 참조하세요.

문서 크기는 문서당 5,120자 미만이어야 하며, 컬렉션당 최대 1,000개의 항목(ID)을 포함할 수 있습니다. 컬렉션은 요청 본문에 제출됩니다. 다음 예제는 엔터티 링크 설정 끝에 제출할 수 있는 콘텐츠에 대한 설명입니다.

```
{"documents": [{"id": "1",
                "language": "en",
                "text": "Jeff bought three dozen eggs because there was a 50% discount."
                },
               {"id": "2",
                "language": "en",
                "text": "The Great Depression began in 1929. By 1933, the GDP in America fell by 25%."
                }
               ]
}
```    
    
## <a name="step-1-structure-the-request"></a>1단계: 요청 구성

요청 정의에 대한 자세한 내용은 [텍스트 분석 API를 호출하는 방법](text-analytics-how-to-call-api.md)에서 찾을 수 있습니다. 편의상 다음 사항을 다시 설명합니다.

+ **POST** 요청을 만듭니다. 이 요청에 대한 API 문서인 [엔터티 연결 API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)

+ 엔터티 추출에 대한 HTTP 끝점을 설정합니다. `/entities` 리소스(`https://[your-region].api.cognitive.microsoft.com/text/analytics/v2.1/entities`)를 포함해야 합니다.

+ Text Analytics 작업에 대한 액세스 키가 포함되도록 요청 헤더를 설정합니다. 자세한 내용은 [끝점 및 액세스 키를 찾는 방법](text-analytics-how-to-access-key.md)을 참조하세요.

+ 이 분석을 위해 준비한 JSON 문서 컬렉션을 요청 본문에 제공합니다.

> [!Tip]
> [Postman](text-analytics-how-to-call-api.md)을 사용하거나 [설명서](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)에서 **API 테스트 콘솔**을 열어 요청을 구성하고, 서비스에 POST합니다.

## <a name="step-2-post-the-request"></a>2단계: 요청 게시

요청을 받으면 분석이 수행됩니다. 참조 된 [데이터 제한](../overview.md#data-limits) 분당 보내고 두 번째 요청의 수와 크기에 대 한 정보에 대 한 개요는 섹션입니다.

상태 비저장 서비스임에 유의하세요. 계정에는 아무 데이터도 저장되지 않습니다. 결과는 응답으로 즉시 반환됩니다.

## <a name="step-3-view-results"></a>3단계: 결과 보기

모든 POST 요청에서는 ID 및 감지된 속성이 있는 JSON 형식의 응답을 반환합니다.

출력은 즉시 반환됩니다. JSON을 승인하는 애플리케이션에 결과를 스트리밍하거나, 로컬 시스템의 파일에 출력을 저장한 후에 데이터를 정렬, 검색, 조작할 수 있는 애플리케이션으로 가져올 수 있습니다.

엔터티 링크 설정의 출력 예는 다음과 같습니다.

```json
{
    "Documents": [
        {
            "Id": "1",
            "Entities": [
                {
                    "Name": "Jeff",
                    "Matches": [
                        {
                            "Text": "Jeff",
                            "Offset": 0,
                            "Length": 4
                        }
                    ],
                    "Type": "Person"
                },
                {
                    "Name": "three dozen",
                    "Matches": [
                        {
                            "Text": "three dozen",
                            "Offset": 12,
                            "Length": 11
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Number"
                },
                {
                    "Name": "50",
                    "Matches": [
                        {
                            "Text": "50",
                            "Offset": 49,
                            "Length": 2
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Number"
                },
                {
                    "Name": "50%",
                    "Matches": [
                        {
                            "Text": "50%",
                            "Offset": 49,
                            "Length": 3
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Percentage"
                }
            ]
        },
        {
            "Id": "2",
            "Entities": [
                {
                    "Name": "Great Depression",
                    "Matches": [
                        {
                            "Text": "The Great Depression",
                            "Offset": 0,
                            "Length": 20
                        }
                    ],
                    "WikipediaLanguage": "en",
                    "WikipediaId": "Great Depression",
                    "WikipediaUrl": "https://en.wikipedia.org/wiki/Great_Depression",
                    "BingId": "d9364681-98ad-1a66-f869-a3f1c8ae8ef8"
                },
                {
                    "Name": "1929",
                    "Matches": [
                        {
                            "Text": "1929",
                            "Offset": 30,
                            "Length": 4
                        }
                    ],
                    "Type": "DateTime",
                    "SubType": "DateRange"
                },
                {
                    "Name": "By 1933",
                    "Matches": [
                        {
                            "Text": "By 1933",
                            "Offset": 36,
                            "Length": 7
                        }
                    ],
                    "Type": "DateTime",
                    "SubType": "DateRange"
                },
                {
                    "Name": "Gross domestic product",
                    "Matches": [
                        {
                            "Text": "GDP",
                            "Offset": 49,
                            "Length": 3
                        }
                    ],
                    "WikipediaLanguage": "en",
                    "WikipediaId": "Gross domestic product",
                    "WikipediaUrl": "https://en.wikipedia.org/wiki/Gross_domestic_product",
                    "BingId": "c859ed84-c0dd-e18f-394a-530cae5468a2"
                },
                {
                    "Name": "United States",
                    "Matches": [
                        {
                            "Text": "America",
                            "Offset": 56,
                            "Length": 7
                        }
                    ],
                    "WikipediaLanguage": "en",
                    "WikipediaId": "United States",
                    "WikipediaUrl": "https://en.wikipedia.org/wiki/United_States",
                    "BingId": "5232ed96-85b1-2edb-12c6-63e6c597a1de",
                    "Type": "Location"
                },
                {
                    "Name": "25",
                    "Matches": [
                        {
                            "Text": "25",
                            "Offset": 72,
                            "Length": 2
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Number"
                },
                {
                    "Name": "25%",
                    "Matches": [
                        {
                            "Text": "25%",
                            "Offset": 72,
                            "Length": 3
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Percentage"
                }
            ]
        }
    ],
    "Errors": []
}
```


## <a name="summary"></a>요약

이 문서에서는 Cognitive Services의 텍스트 분석을 사용하여 엔터티 링크 설정에 대한 개념과 워크플로를 배웠습니다. 요약하자면 다음과 같습니다.

+ [Entities API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)를 선택한 언어로 사용할 수 있습니다.
+ 요청 본문의 JSON 문서에는 ID, 텍스트 및 언어 코드가 포함됩니다.
+ POST 요청은 개인 설정된 [액세스 키와 구독에 유효한 끝점](text-analytics-how-to-access-key.md)을 사용하여 `/entities` 끝점에 대해 수행됩니다.
+ 연결된 엔터티(각 문서 ID에 대한 신뢰도 점수, 오프셋 및 웹 링크 포함)로 구성된 응답 출력은 모든 애플리케이션에서 사용할 수 있습니다.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [텍스트 분석 API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)

* [Text Analytics 개요](../overview.md)  
* [FAQ(질문과 대답)](../text-analytics-resource-faq.md)</br>
* [Text Analytics 제품 페이지](//go.microsoft.com/fwlink/?LinkID=759712) 
