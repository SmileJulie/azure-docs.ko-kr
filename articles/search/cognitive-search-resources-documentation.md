---
title: 인식 검색 설명서 리소스 - Azure Search
description: Azure Search에서 인식 검색 작업과 관련된 문서, 자습서, 샘플 및 블로그 게시물에 대해 주석이 달린 목록입니다.
services: search
manager: cgronlun
author: HeidiSteen
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 41637fae5592ac292da22303071d51b43116c78b
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67671901"
---
# <a name="documentation-resources-for-cognitive-search-workloads"></a>인식 검색 작업에 대한 설명서 리소스

현재 일반 공급 cognitive search는 Azure Search 인덱싱에 텍스트가 아닌 원본 및 Azure Search에서 전체 텍스트 검색 가능한 콘텐츠를 변환 하는 구분 되지 않는 텍스트에서 잠재 정보를 검색 하는 새 보강 계층.

다음 문서는 인식 검색에 대한 전체 설명서입니다.

## <a name="getting-started"></a>시작
+ ["인식 검색이란?"](cognitive-search-concept-intro.md)
+ [빠른 시작: 포털에서 인식 검색 시도](cognitive-search-quickstart-blob.md)
+ [자습서: Cognitive Search API 알아보기](cognitive-search-tutorial-blob.md)
+ [예제: Cognitive search에 대 한 사용자 지정 기술 만들기](cognitive-search-create-custom-skill-example.md)

## <a name="how-to-guidance"></a>방법 가이드
+ [기술 집합을 정의하는 방법](cognitive-search-defining-skillset.md)
+ [기술 집합의 주석 참조하는 방법](cognitive-search-concept-annotations-syntax.md)
+ [필드를 인덱스에 매핑하는 방법](cognitive-search-output-field-mapping.md)
+ [이미지에서 정보 처리 및 추출하는 방법](cognitive-search-concept-image-scenarios.md)
+ [Azure Search 인덱스를 다시 빌드하는 방법](search-howto-reindex.md)
+ [사용자 정의 기술 인터페이스를 정의하는 방법](cognitive-search-custom-skill-interface.md)
+ [문제 해결 팁](cognitive-search-concept-troubleshooting.md)

## <a name="reference"></a>참조

+ [미리 정의된 기술](cognitive-search-predefined-skills.md)
  + [Microsoft.Skills.Text.KeyPhraseSkill](cognitive-search-skill-keyphrases.md)
  + [Microsoft.Skills.Text.LanguageDetectionSkill](cognitive-search-skill-language-detection.md)
  + [Microsoft.Skills.Text.NamedEntityRecognitionSkill](cognitive-search-skill-named-entity-recognition.md)
  + [Microsoft.Skills.Text.MergeSkill](cognitive-search-skill-textmerger.md)
  + [Microsoft.Skills.Text.SplitSkill](cognitive-search-skill-textsplit.md)
  + [Microsoft.Skills.Text.SentimentSkill](cognitive-search-skill-sentiment.md)
  + [Microsoft.Skills.Vision.ImageAnalysisSkill](cognitive-search-skill-image-analysis.md)
  + [Microsoft.Skills.Vision.OcrSkill](cognitive-search-skill-ocr.md)
  + [Microsoft.Skills.Util.ShaperSkill](cognitive-search-skill-shaper.md)

+ [REST API](https://docs.microsoft.com/rest/api/searchservice/)
  + [기술 세트 만들기(api-version=2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
  + [인덱서 만들기(api-version=2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

## <a name="see-also"></a>참고자료

+ [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/)
+ [Azure Search의 인덱서](search-indexer-overview.md)
+ [Azure Search란?](search-what-is-azure-search.md)
