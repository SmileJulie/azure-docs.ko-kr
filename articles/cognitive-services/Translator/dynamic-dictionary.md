---
title: 동적 사전 - Translator Text API
titlesuffix: Azure Cognitive Services
description: Translator Text API의 동적 사전 기능을 사용하는 방법입니다.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: a815434cb8797acf6b92a8fe4a4f1ff69508975d
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67839225"
---
# <a name="how-to-use-a-dynamic-dictionary"></a>동적 사전을 사용 하는 방법

단어나 구에 적용할 번역을 이미 알고 있는 경우 요청 내에 태그로 제공할 수 있습니다. 동적 사전은 적절한 이름 및 제품 이름과 같은 복합 명사에 사용할 때만 안전합니다.

**구문:**

<mstrans:dictionary translation=”구 번역”>구</mstrans:dictionary>

**요구 사항:**

* `From` 및`To` 언어는 달라 야 합니다. 
* 자동 검색 기능을 `From` 사용 하는 대신 API 번역 요청에 매개 변수를 포함 해야 합니다. 

**예제: en-de:**

원본 입력: 단어 <mstrans:dictionary translation=\"wordomatic\">단어 또는 구</mstrans:dictionary>는 사전 항목입니다.

대상 출력: Das Wort "wordomatic" ist ein Wörterbucheintrag.

이 기능은 HTML 모드를 사용할 때와 그렇지 않을 때 같은 결과를 가져옵니다.

이 기능은 자주 사용하지 않는 것이 좋습니다. 번역을 사용자 지정하는 보다 적절하고 훨씬 더 나은 방법은 Custom Translator를 사용하는 것입니다. Custom Translator는 컨텍스트 및 통계적 확률을 최대한 활용합니다. 컨텍스트에 따라 사용자 작업 또는 구를 보여 주는 학습 데이터를 보유하고 있거나 이러한 데이터를 만들 수 있으면 훨씬 더 나은 결과를 얻을 수 있습니다. 사용자 지정 변환기에 대한 자세한 내용은 [https://aka.ms/CustomTranslator](https://aka.ms/CustomTranslator)에서 찾을 수 있습니다.
