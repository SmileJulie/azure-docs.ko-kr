---
title: Academic Graph 엔터티 특성 - Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Academic Knowledge API에서 Academic Graph와 함께 사용할 수 있는 엔터티 특성에 대해 알아봅니다.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: 485775660ecfdf2291365ab98c9188295ea2cbde
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61340182"
---
# <a name="entity-attributes"></a>엔터티 특성

Academic Graph는 7가지의 엔터티 형식으로 구성됩니다. 모든 엔터티에 엔터티 형식과 엔터티 ID가 됩니다.

## <a name="common-entity-attributes"></a>공용 엔터티 특성
이름    |설명                |Type       | 작업
------- | ------------------------- | --------- | ----------------------------
Id      |엔터티 ID                  |Int64      |같음
Ty      |엔터티 형식                |enum   |같음

## <a name="entity-type-enum"></a>엔터티 형식 열거형
이름                                                            |값
----------------------------------------------------------------|-----
[문서](PaperEntityAttributes.md)                               |0
[작성자](AuthorEntityAttributes.md)                             |1
[저널](JournalEntityAttributes.md)                           |2
[컨퍼런스 시리즈](JournalEntityAttributes.md)                 |3
[컨퍼런스 인스턴스](ConferenceInstanceEntityAttributes.md)    |4
[소속](AffiliationEntityAttributes.md)                   |5
[연구 분야](FieldsOfStudyEntityAttributes.md)                      |6

