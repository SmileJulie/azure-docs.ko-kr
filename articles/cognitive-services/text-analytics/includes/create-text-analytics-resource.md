---
title: Cognitive Services Text Analytics 리소스 만들기
titleSuffix: Azure Cognitive Services
description: Cognitive Services Text Analytics 리소스를 만드는 방법에 대해 알아봅니다.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 7e8b4480911f00afa8524ef4b81d697bb8ee5bcc
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/15/2019
ms.locfileid: "67877466"
---
## <a name="create-a-cognitive-services-text-analytics-resource"></a>Cognitive Services Text Analytics 리소스 만들기

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.
1. **리소스 만들기**를 선택 하 고 **AI + Machine Learning** > **Text Analytics**로 이동 합니다.
   또는 [Create Text Analytics](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics)로 이동 합니다.
1. 필요한 설정을 모두 입력 합니다.

    |설정|값|
    |--|--|
    |이름|이름 입력 (2-64 자)|
    |구독|적절 한 구독을 선택 합니다.|
    |위치|주변 위치 선택|
    |가격 책정 계층| **S**, 표준 가격 책정 계층을 입력 합니다.|
    |리소스 그룹|사용 가능한 리소스 그룹 선택|

1. **만들기** 를 선택 하 고 리소스가 생성 될 때까지 기다립니다. 브라우저가 새로 만든 리소스 페이지로 자동으로 리디렉션됩니다.
1. 구성 `endpoint` 된 및 API 키를 수집 합니다.

    |포털의 리소스 탭|설정|값|
    |--|--|--|
    |**개요**|엔드포인트|끝점을 복사 합니다. 다음과 유사 하 게 표시 됩니다.`https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`|
    |**키**|API 키|두 키 중 하나를 복사 합니다. <`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`>는 공백이 나 대시가 없는 32 자리의 영숫자 문자열입니다.|
