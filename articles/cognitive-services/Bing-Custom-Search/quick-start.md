---
title: '빠른 시작: 처음으로 Bing Custom Search 인스턴스 만들기 | Microsoft Docs'
titlesuffix: Azure Cognitive Services
description: 이 문서를 사용하여 사용자가 정의하는 도메인 및 웹 페이지를 검색할 수 있는 사용자 지정 Bing 인스턴스를 만듭니다.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 06/18/2019
ms.author: aahi
ms.openlocfilehash: 6949824f598194456837544526223b823dcfc3e5
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67273337"
---
# <a name="quickstart-create-your-first-bing-custom-search-instance"></a>빠른 시작: 처음으로 Bing Custom Search 인스턴스 만들기

Bing Custom Search를 사용하려면 웹 보기 또는 조각을 정의하는 사용자 지정 검색 인스턴스를 만들어야 합니다. 이 인스턴스에는 사용자가 원할 수 있는 모든 순위 조정과 함께 검색하려는 공용 도메인, 웹 사이트 및 웹 페이지가 포함됩니다. 

인스턴스를 만들려면 [Bing Custom Search 포털](https://customsearch.ai)을 사용합니다. 

![Bing Custom Search 포털의 이미지](media/blockedCustomSrch.png)

## <a name="prerequisites"></a>필수 조건

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]

## <a name="create-a-custom-search-instance"></a>사용자 지정 검색 인스턴스 만들기

Bing Custom Search 인스턴스를 만들려면

1. [Bing Custom Search 포털](https://customsearch.ai) 웹 페이지에서 **시작**을 클릭하고 Microsoft 계정으로 로그인합니다.

2. **새 인스턴스**를 클릭하고 설명이 포함된 이름을 입력합니다. 언제든지 인스턴스의 이름을 변경할 수 있습니다.
 
3. **검색 환경**의 **활성** 탭에서 검색에 포함할 웹 사이트 하나 이상의 URL을 입력합니다. 

    > [!NOTE]
    > Bing Custom Search 인스턴스는 공개되고 Bing에 의해 인덱싱된 도메인 및 웹 페이지에 대한 결과를 반환합니다.

4. Bing Custom Search 포털의 오른쪽을 사용하여 쿼리를 입력하고 검색 인스턴스에서 반환된 검색 결과를 검사할 수 있습니다. 결과가 반환되지 않는 경우 다른 URL을 입력합니다.  

5. **게시**를 클릭하여 프로덕션 환경에 대한 변경 내용을 게시하고 인스턴스의 엔드포인트를 업데이트합니다.

6.  **엔드포인트**에서 **프로덕션** 탭을 클릭하고 **사용자 지정 구성 ID**를 복사합니다. 이 ID는 호출에서 `customconfig=` 쿼리 매개 변수에 추가하여 Custom Search API를 호출하는 데 필요합니다.


## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [빠른 시작: Bing Custom Search 엔드포인트 호출](./call-endpoint-csharp.md)
