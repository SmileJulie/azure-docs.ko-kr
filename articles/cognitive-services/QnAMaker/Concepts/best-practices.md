---
title: 모범 사례 - QnA Maker
titlesuffix: Azure Cognitive Services
description: 모범 사례를 사용하여 기술 자료를 개선하고 애플리케이션/챗봇 최종 사용자에게 보다 나은 결과를 제공할 수 있습니다.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 06/25/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: dd4f85822a5e6615e7ea6e31b4231c04c9d4e88c
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67542842"
---
# <a name="best-practices-of-a-qna-maker-knowledge-base"></a>QnA Maker 기술 자료의 모범 사례

[기술 자료 개발 수명 주기](../Concepts/development-lifecycle-knowledge-base.md)에서는 KB를 종합적으로 관리하는 방법을 안내합니다. 이러한 모범 사례를 사용 하 여 기술 자료를 개선 및 클라이언트 응용 프로그램에 더 나은 결과 제공 하거나 채팅 봇의 최종 사용자입니다.

## <a name="extraction"></a>추출

QnA Maker 서비스는 콘텐츠에서 QnA를 추출하고 지원되는 파일 및 HTML 형식 목록을 확장하는 알고리즘을 지속적으로 개선하고 있습니다. 문서 형식에 따라 데이터 추출에 대한 [지침](../Concepts/data-sources-supported.md)을 수행합니다. 

일반적으로 FAQ 페이지는 독립 실행형이어야 하며, 다른 정보와 결합되지 않아야 합니다. 제품 설명서는 제목에 명확해야 하고 인덱스 페이지까지 명확하면 더욱 좋습니다. 

### <a name="configuring-multi-turn"></a>다중 구성 설정

사용 하도록 설정 하는 다중 턴 추출을 사용 하 여 기술 자료를 만듭니다. 기술 자료에 않습니다 또는 질문 계층 구조를 지원 해야 하는 경우이 계층을 문서에서 추출 또는 문서 추출 된 후 생성 수는 있습니다. 

<!--is this a global setting that can only be configured at kb creation time? -->

## <a name="creating-good-questions-and-answers"></a>좋은 질문 및 답변 만들기

### <a name="good-questions"></a>좋은 질문

가장 좋은 질문은 간단합니다. 각 질문의 키워드 또는 문구를 고려한 다음, 해당 키워드 또는 문구에 대한 간단한 질문을 만듭니다. 

필요한 개수만큼 대체 질문을 추가하되, 간단한 변경으로 유지합니다. 질문의 주요 목표에 속하지 않는 단어나 문구를 더 추가하는 것은 QnA Maker에서 일치 항목을 찾는 데 도움이 되지 않습니다. 


### <a name="add-relevant-alternative-questions"></a>관련 대체 질문 추가

사용자는 텍스트의 대화형 스타일을 사용 하 여 질문을 입력 될 수 있습니다 `How do I add a toner cartridge to my printer?` 또는 같은 검색 키워드 `toner cartridge`합니다. 기술 자료에 가장 적합 한 답변을 올바르게 반환 하기 위해 질문의 스타일을 모두 있어야 합니다. 확실 하지 않은 고객 입력은 키워드, Application Insights 데이터를 사용 하 여 쿼리를 분석 합니다.

### <a name="good-answers"></a>좋은 답변

최상의 답변은 간단한 대답 있지만 너무 간단 하지 않습니다. 와 같은 답변 사용 하지 마십시오 `yes` 고 `no`입니다. 답변은 다른 원본에 연결 또는 미디어 및 링크를 사용 하 여 풍부한 환경을 제공 하는 경우 사용 하 여 [메타 데이터 태깅](./knowledge-base.md#key-knowledge-base-concepts) 답변을 한 다음 구분 [쿼리를 제출](../how-to/metadata-generateanswer-usage.md#generateanswer-request-configuration) 합니다 메타데이터태그를사용하여`strictFilters` 정답 버전을 가져올 속성입니다.

## <a name="chit-chat"></a>잠답
봇에 잡담을 추가하여 적은 노력으로 봇을 대화형이고 몰입도 높게 만듭니다. 쉽게 KB를 만들 때 미리 정의 된 개성에서 chit 채팅 데이터 집합을 추가 하 고 언제 든 지 변경할 수 있습니다. [KB에 잡담을 추가](../How-To/chit-chat-knowledge-base.md)하는 방법을 알아봅니다. 

### <a name="choosing-a-personality"></a>개성 선택
몇 가지 미리 정의 된 개성 chit 채팅 지원 됩니다. 

|개인 정보 |QnA Maker 데이터 집합 파일 |
|---------|-----|
|전문가 |[qna_chitchat_professional.tsv](https://qnamakerstore.blob.core.windows.net/qnamakerdata/editorial/qna_chitchat_professional.tsv) |
|친숙한 |[qna_chitchat_friendly.tsv](https://qnamakerstore.blob.core.windows.net/qnamakerdata/editorial/qna_chitchat_friendly.tsv) |
|Witty |[qna_chitchat_witty.tsv](https://qnamakerstore.blob.core.windows.net/qnamakerdata/editorial/qna_chitchat_witty.tsv) |
|신경쓰지 |[qna_chitchat_caring.tsv](https://qnamakerstore.blob.core.windows.net/qnamakerdata/editorial/qna_chitchat_caring.tsv) |
|열광 |[qna_chitchat_enthusiastic.tsv](https://qnamakerstore.blob.core.windows.net/qnamakerdata/editorial/qna_chitchat_enthusiastic.tsv) |

응답은 격식체에서 비격식체의 범위까지 지정됩니다. 봇에 원하는 어조와 가장 가깝게 맞춰진 개성을 선택해야 합니다. 볼 수 있습니다 합니다 [데이터 집합](https://github.com/Microsoft/BotBuilder-PersonalityChat/tree/master/CSharp/Datasets), 및 봇에 대 한 기준으로 사용 되는 하나를 선택 하 고 다음 응답을 사용자 지정 합니다. 

### <a name="edit-bot-specific-questions"></a>봇 관련 질문 편집
잠답 데이터 집합의 일부이며 일반 답변으로 채워진 몇 가지 봇 관련 질문이 있습니다. 이 대답을 변경하여 봇 세부 정보를 가장 잘 반영합니다. 

잡담 QnA를 구체적으로 만드는 것이 좋습니다.

* 당신은 누구인가요?
* 어떻게 해야 합니까?
* 나이가 어떻게 되세요?
* 만든 사람은 누구인가요?
* Hello
   
### <a name="adding-custom-chit-chat-with-a-metadata-tag"></a>추가 메타 데이터 태그를 사용 하 여 사용자 지정 chit 채팅

사용자 고유의 chit 채팅 QnA 쌍을 추가 하는 경우이 대답이 반환 되도록 메타 데이터를 추가 해야 합니다. 메타 데이터 이름/값 쌍이 `editorial:chitchat`합니다.

## <a name="searching-for-answers"></a>답변 검색

GenerateAnswer API 질문 및 답변을 사용 하 여 사용자의 쿼리를 최상의 답변 검색.

### <a name="searching-questions-only-when-answer-is-not-relevant"></a>검색 질문 대답이 관련 된 경우에

사용 된 [ `RankerType=QuestionOnly` ](#choosing-ranker-type) 답변을 검색 하지 않으려는 경우. 

이 예제는 기술 자료는 답변으로 전체 형식으로 사용 하 여 질문으로 머리 글자어의 카탈로그 하는 경우입니다. 답은 적절 한 답변을 검색 하는 도움이 되지 않습니다.

## <a name="rankingscoring"></a>순위 지정/채점
QnA Maker가 지원하는 순위 기능을 최대로 활용하시기 바랍니다. 그러면 특정 사용자 쿼리에 적절하게 답변할 가능성이 높아집니다.

### <a name="choosing-a-threshold"></a>임계값 선택

하지만 기본값 [신뢰성 점수](confidence-score.md) 할 수 있습니다 임계값은 50으로 사용 되는 [임계값을 변경](confidence-score.md#set-threshold) 필요에 따라 KB에 대 한 합니다. 모든 KB가 다르므로 KB에 가장 적합한 임계값을 에 테스트하고 선택해야 합니다. 

### <a name="choosing-ranker-type"></a>우선 순위 형식 선택
기본적으로 QnA Maker 질문 및 답변을 통해 검색합니다. 질문만 검색 하려는 경우 생성 하려면 답변을 사용 합니다 `RankerType=QuestionOnly` GenerateAnswer 요청 POST 본문에 있습니다.

### <a name="add-alternate-questions"></a>대체 질문 추가
[대체 질문](../How-To/edit-knowledge-base.md)은 사용자 쿼리와 일치할 가능성을 높입니다. 대체 질문은 같은 질문을 여러 가지 방식으로 받을 수 있는 경우에 유용합니다. 여기에는 문장 구조 및 단어 스타일 변경이 포함됩니다.

|원래 쿼리|대체 쿼리|변경| 
|--|--|--|
|주차 가능하나요?|주차장이 있나요?|문장 구조|
 |Hi|Yo<br>Hey there!|단어 스타일 또는 속어|

<a name="use-metadata-filters"></a>

### <a name="use-metadata-tags-to-filter-questions-and-answers"></a>메타데이터 태그를 사용하여 질문과 답변 필터링

[메타 데이터](../How-To/edit-knowledge-base.md) 추가 알고 있는 것을 클라이언트 응용 프로그램에 대 한 기능 메타 데이터 태그를 기준으로 하는 사용자 쿼리의 결과 좁힐 모든 답변을 대신 사용 하지는 않아야 합니다. 쿼리가 같더라도 메타데이터 태그에 따라 기술 자료 답변이 달라질 수 있습니다. 예를 들어 식당 지점의 위치가 다르면 *“주차장 위치는 어디인가요”* 에 대한 대답이 달라질 수 있습니다. 즉, 메타데이터는 *위치: 시애틀* 또는 *위치: 레드몬드*입니다.

### <a name="use-synonyms"></a>동의어 사용
영어 언어에서 동의어 지원은 일부를 사용 하 여을 통해 대/소문자 단어 변경 합니다 [변경할 API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/alterations/replace) 다양 한 폼을 사용 하는 키워드에 동의어를 추가 하려면. 동의어는 QnA Maker 서비스 수준에서 추가 하 고 서비스에서 모든 기술 자료에서 공유 됩니다.

|원래 단어|동의어|
|--|--|
|구입|구매<br>net-banking<br>인터넷 뱅킹|

### <a name="use-distinct-words-to-differentiate-questions"></a>고유한 단어를 사용하여 질문을 구분
사용자 쿼리를 기술 자료의 질문과 일치시키는 QnA Maker 순위 알고리즘은 각 질문이 다른 요구 사항을 다루는 경우에 가장 효과적입니다. 질문 사이에 같은 단어 집합을 반복하면 해당 단어가 포함된 특정 사용자 쿼리에 대해 올바른 대답이 선택될 확률이 감소합니다. 

예를 들어, 다음과 같은 질문을 사용하여 두 개의 별도 QnA가 있을 수 있습니다.

|QnA|
|--|
|주차 *위치*는 어디인가요|
|ATM *위치*는 어디인가요|

이러한 두 QnA가 매우 유사한 단어로 구성되었으므로 이 유사성은 *"`<x>`위치는 어디인가요"* 와 같이 구성된 사용자 쿼리에 대해 유사한 점수가 발생할 수 있습니다. 대신, KB의 많은 질문에 포함된 “위치”와 같은 단어를 방지하여 “주차장은 어디인가요” 및 “ATM은 어디인가요”와 같은 쿼리로 명확하게 구분하려고 합니다.   

## <a name="collaborate"></a>공동 작업
QnA Maker를 통해 사용자들이 기술 자료를 [공동으로 작업](../How-to/collaborate-knowledge-base.md)할 수 있습니다. 사용자는 기술 자료에 액세스하기 위해 Azure QnA Maker 리소스 그룹에 대한 액세스 권한이 필요합니다. 기술 자료 편집 및 유지 관리를 아웃소싱하려는 조직도 있을 것이며, 이 경우에도 여전히 Azure 리소스에 대한 액세스를 보호할 수 있습니다. 이 편집자-승인자 모델은 서로 다른 구독에 최대 2개의 동일한 [QnA Maker 서비스](../How-to/set-up-qnamaker-service-azure.md)를 설정하고 하나를 편집-테스트 주기용으로 지정하여 수행할 수 있습니다. 테스트가 완료되면 [가져오기-내보내기](../Tutorials/migrate-knowledge-base.md) 프로세스를 사용하여 최종적으로 기술 자료를 게시하고 엔드포인트를 업데이트하는 승인자의 QnA Maker 서비스로 기술 자료 콘텐츠를 전송할 수 있습니다.



## <a name="active-learning"></a>능동적 학습

[활성 학습](../How-to/improve-knowledge-base.md)은 품질의 범위가 넓은 사용자 기반 쿼리의 수가 많을 때 가장 적절한 대체 질문을 제안합니다. 그러므로 클라이언트 애플리케이션의 사용자 쿼리가 검열되지 않고 활성 학습 피드백 루프에 참여할 수 있도록 해야 합니다. 질문 QnA Maker 포털에서 제안 된를 수행할 수 있습니다 **[제안으로 필터링](../How-To/improve-knowledge-base.md#accept-an-active-learning-suggestion-in-the-knowledge-base)** 다음 검토 및 적용 또는 이러한 제안 취소 합니다. 

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [기술 자료 편집](../How-to/edit-knowledge-base.md)
