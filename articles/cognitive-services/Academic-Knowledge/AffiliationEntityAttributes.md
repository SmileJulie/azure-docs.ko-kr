---
title: Academic Knowledge API에서 Affiliation 엔터티 특성
titlesuffix: Azure Cognitive Services
description: Academic Knowledge API의 Affiliation 엔터티와 함께 사용할 수 있는 특성에 대해 알아봅니다.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 82e6a5b66342e58e62da029d617cbd1d74c28149
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61340522"
---
# <a name="affiliation-entity"></a>Affiliation 엔터티

<sub> *다음 특성은 Affiliation 엔터티에 한정됩니다. (Ty = '5') </sub>

이름    |설명                            |Type       | 작업
------- | ------------------------------------- | --------- | ----------------------------
Id      |엔터티 ID                              |Int64      |같음
AfN     |소속의 정규화된 이름        |String     |같음
DAfN    |소속 표시 이름       |String     |없음
CC      |소속의 총 인용 횟수           |Int32      |없음  
ECC     |소속의 총 예상 인용 횟수 |Int32      |없음

## <a name="extended-metadata-attributes"></a>확장 메타데이터 특성 ##

Name    | 설명               
--------|---------------------------    
PC      |소속의 문서 수
