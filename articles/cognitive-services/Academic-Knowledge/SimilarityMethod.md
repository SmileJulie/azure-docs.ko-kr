---
title: Similarity 메서드 - Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Similarity 메서드를 사용하여 두 문자열의 교육적 유사성을 계산합니다.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 01/18/2017
ms.author: alch
ms.openlocfilehash: 7f692c08f8af322bf7e6ab576e2e6f516594a6c4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61336520"
---
# <a name="similarity-method"></a>유사성 메서드

**Similarity** REST API는 두 문자열 간의 교육적 유사성을 계산하는 데 사용됩니다. 
<br>

**REST 엔드포인트:**
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/similarity?
```

## <a name="request-parameters"></a>요청 매개 변수

매개 변수        |데이터 형식      |필수 | 설명
----------|----------|----------|------------
**s1**        |문자열   |예  |비교할 문자열*
**s2**        |String   |예  |비교할 문자열*

<sub> *비교할 문자열의 최대 길이는 1MB입니다. </sub>
<br>

## <a name="response"></a>response

이름 | 설명
--------|---------
**SimilarityScore**        |s1과 s2의 코사인 유사성을 나타내는 부동 소수점 값으로, 1.0에 가까울수록 유사성이 높고 -1.0에 가까울수록 유사성이 낮습니다.

<br>

## <a name="successerror-conditions"></a>성공/오류 조건

HTTP 상태 | 이유 | response
-----------|----------|--------
**200**         |성공 | 부동 소수점 숫자
**400**         | 잘못된 요청 또는 요청이 유효하지 않음 | 오류 메시지      
**500**         |내부 서버 오류 | 오류 메시지
**Timed out**     | 요청 시간이 초과되었습니다.  | 오류 메시지

<br>

## <a name="example-calculate-similarity-of-two-partial-abstracts"></a>예제: 두 부분 요약의 유사성 계산
#### <a name="request"></a>요청:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/similarity?s1=Using complementary priors, we derive a fast greedy algorithm that can learn deep directed belief networks one layer at a time, provided the top two layers form an undirected associative memory
&s2=Deepneural nets with a large number of parameters are very powerful machine learning systems. However, overfitting is a serious problem in such networks
```
이 예에서는 **Similarity** API를 사용하여 두 부분 요약 간의 유사성 점수를 생성합니다.
#### <a name="response"></a>응답:
```
0.520
```
#### <a name="remarks"></a>설명:
유사성 점수는 단어 삽입을 통해 교육적 개념을 평가함으로써 결정됩니다. 이 예에서 0.52는 두 개의 부분 요약이 다소 유사함을 의미합니다.
<br>
