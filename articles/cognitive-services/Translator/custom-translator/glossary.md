---
title: 용어집 - Custom Translator
titleSuffix: Azure Cognitive Services
description: Custom Translator 용어집
author: swmachan
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: reference
ms.openlocfilehash: 3781969f74b8314ebb4633697b922f47c12196ae
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443389"
---
# <a name="glossary"></a>용어

[Custom Translator](https://portal.customtranslator.azure.ai) 용어집은 Custom Translator로 작업할 때 나타날 수 있는 용어에 대해 설명합니다.

| **단어 또는 구**       | **정의**                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 원본 언어          | 원본 언어는 시작하는 언어로, 다른 언어(“대상”)로 변환할 언어입니다.                                                                                                                                                                                                                                                                                                                                                         |
| 대상 언어          | 대상 언어는 원본 언어를 수신한 후 기계 번역에서 제공해야 하는 언어입니다.                                                                                                                                                                                                                                                                                                                                               |
| 단일어 파일         | 단일어 파일에는 다른 언어의 다른 파일과 쌍을 이루지 않는 하나의 언어가 있습니다.                                                                                                                                                                                                                                                                                                                                                                 |
| 병렬 파일           | 병렬 파일은 해당 텍스트를 사용한 두 파일의 조합입니다. 하나의 파일에는 원본 언어가 있고 다른 파일에는 대상 언어가 있습니다.                                                                                                                                                                                                                                                                                                                                         |
| 문장 정렬       | 병렬 데이터 세트는 문장 대 문장을 정렬하여 동일한 텍스트를 두 언어로 표시해야 합니다. 예를 들어 원본 병렬 파일에서 첫 번째 문장은 이론적으로 대상 병렬 파일에 있는 첫 번째 문장에 매핑되어야 합니다.                                                                                                                                                                                                                               |
| 정렬된 텍스트             | 파일 유효성 검사의 가장 중요한 단계 중 하나는 병렬 문서에 문장을 정렬하는 것입니다. 작업은 다른 언어로 다르게 표현됩니다. 또한 다양한 언어마다 다양한 어순이 있습니다. 이 단계에서는 학습에 사용할 수 있도록 동일한 콘텐츠를 사용하여 문장의 정렬 작업을 수행합니다. 질이 낮은 문장 정렬은 파일 중 하나 또는 모두에 무언가 잘못되었음을 나타냅니다. |
| 단어 분리/분리 해제 | 단어 분리는 단어 간 경계를 만드는 기능입니다. 많은 문자 체계에서는 공백을 사용하여 단어 사이에 경계를 나타냅니다. 단어 분리 해제는 이전 단계에서 단어 사이에 삽입되었을 수 있는 가시적 표식을 제거하는 것을 의미합니다.                                                                                                                                                                                                  |
| 구분 기호               | 구분 기호는 한 문장을 세그먼트로 나누거나 문장 사이에 여백을 구분하는 방법입니다. 예를 들어 영어에서 공백은 단어를 구분하고, 콜론 및 세미콜론은 절을 구분하며, 마침표는 문장을 구분합니다.                                                                                                                                                                                                                                         |
| 학습 파일           | 학습 파일은 기계 번역 시스템에게 한 언어(원본)에서 대상 언어(대상)로 매핑하는 방법을 학습시키는 데 사용됩니다. 더 많은 데이터를 제공할수록 시스템은 번역 시 더 나은 성능을 발휘합니다.                                                                                                                                                                                                               |
| 튜닝 파일             | 이러한 파일은 종종 임의로 학습 세트에서 파생됩니다(튜닝 세트를 선택하지 않는 경우). 시스템을 조정하여 제대로 작동하도록 하는 데 자동 선택된 문장이 사용됩니다. 고유한 튜닝 파일을 만들기로 결정한 경우 범용 번역 모델을 만들려면 해당 파일은 도메인 전체의 임의의 문장 세트여야 합니다.                                                                                 |
| 테스팅 파일            | 이러한 파일은 종종 학습 세트에서 임의로 선택된 파일에서 파생됩니다(테스트 세트를 선택하지 않은 경우). 이러한 문장의 목적은 번역 모델의 정확도를 평가하는 것입니다. 이러한 문장은 시스템이 정확하게 번역하길 원하는 문장입니다. 따라서 이러한 문장이 시스템의 평가(BLEU 점수 생성)에 사용되도록 테스트 세트를 만들고 번역기에 업로드할 수 있습니다.   |
| 콤보 파일               | 원본 및 번역된 문장이 같은 파일에 포함된 파일 형식입니다. 지원되는 파일 형식은 “.tmx”, “.xliff”, “.xlf”, “.lcl”, “.xlsx”입니다.                                                                                                                                                                                                                                                                                                                       |
| 보관 파일             | 다른 파일을 포함하는 파일입니다. 지원되는 파일 형식은 zip, gz, tgz입니다.                                                                                                                                                                                                                                                                                                                                                                                                |
| BLEU 점수               | [BLEU](what-is-bleu-score.md)는 번역 모델의 “정밀도” 또는 정확도를 평가하기 위한 업계 표준 방법입니다. 다른 평가 방법도 존재하지만 Microsoft Translator는 BLEU 방법을 사용하여 프로젝트 소유자에게 정확도를 보고합니다.
