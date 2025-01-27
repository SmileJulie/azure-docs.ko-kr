---
title: BLEU 점수란? - Custom Translator
titleSuffix: Azure Cognitive Services
description: BLEU는 자동 번역 및 동일한 소스 문장에 대해 사용자가 만든 하나 이상의 참조 번역 간의 차이를 측정한 것입니다. BLEU 알고리즘은 자동 번역의 연속 구문을 참조 번역에서 찾은 연속 구문과 비교하고, 가중치가 적용된 방식으로 일치 항목의 수를 계산합니다.
author: swmachan
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: swmachan
ms.openlocfilehash: a77fd1a84c1ffc18a1e0c74000c72db5cdbb00e1
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447371"
---
# <a name="what-is-a-bleu-score"></a>BLEU 점수란?

[BLEU(Bilingual Evaluation Understudy)](https://en.wikipedia.org/wiki/BLEU)는 자동 번역 및 동일한 소스 문장에 대해 사람이 만든 하나 이상의 참조 번역 간의 차이를 측정한 것입니다.

## <a name="scoring-process"></a>점수 매기기 프로세스

BLEU 알고리즘은 자동 번역의 연속 구문을 참조 번역에서 찾은 연속 구문과 비교하고, 가중치가 적용된 방식으로 일치 항목의 수를 계산합니다. 이러한 일치 항목은 위치 독립적입니다. 높은 일치 수준은 참조 번역과 높은 수준의 유사성 및 높은 점수를 나타냅니다. 명확성 및 문법적 정확성은 고려 대상이 아닙니다.

## <a name="how-bleu-works"></a>BLEU가 작동하는 방식은?

BLEU의 장점은 모든 문장에 대한 정확한 사람의 판단을 고안하려 하기보다 테스트 모음에 대한 개별 문장 판단 오류를 평균 내어 사람의 판단과 밀접한 관련이 있다는 점입니다.

BLEU 점수에 대한 자세한 설명은 [여기](https://youtu.be/-UqDljMymMg)에 있습니다.

BLEU 결과는 도메인의 너비, 학습 및 데이터 튜닝을 사용한 테스트 데이터의 일관성 및 학습에 사용할 수 있는 데이터의 양에 따라 다릅니다. 모델을 좁은 도메인에서 학습시키고 학습 데이터가 테스트 데이터와 일치하는 경우 높은 BLEU 점수를 예상할 수 있습니다.

>[!NOTE]
>BLEU 결과를 동일한 테스트 집합, 동일한 언어 쌍 및 동일한 MT 엔진과 비교할 경우 BLEU 점수 간 비교만이 타당합니다. 다른 테스트 집합에서 BLEU 점수는 다르게 마련입니다.
