---
title: Azure API Management에서 요청 추적을 사용하여 API 디버그 | Microsoft Docs
description: 이 자습서의 단계에 따라 Azure API Management의 요청 처리 단계를 검사하는 방법을 알아봅니다.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 06/15/2018
ms.author: apimpm
ms.openlocfilehash: ff3dde8ac95b678866ba6f5216ba23357b067765
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203531"
---
# <a name="debug-your-apis-using-request-tracing"></a>요청 추적을 사용하여 API 디버그

이 자습서에서는 API 디버깅 및 문제 해결에 도움을 주기 위해 요청 처리를 검사하는 방법을 설명합니다. 

이 자습서에서는 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * 호출 추적

![API 검사기](media/api-management-howto-api-inspector/api-inspector001.PNG)

## <a name="prerequisites"></a>필수 조건

+ [Azure API Management 용어](api-management-terminology.md)를 익힙니다.
+ 다음 빠른 시작을 완료합니다. [Azure API Management 인스턴스 만들기](get-started-create-service-instance.md)
+ 또한 [첫 번째 API 가져오기 및 게시](import-and-publish.md) 자습서를 완료합니다.

## <a name="trace-a-call"></a>호출 추적

![API 추적](media/api-management-howto-api-inspector/06-DebugYourAPIs-01-TraceCall.png)

1. **API**를 선택합니다.
2. API 목록에서 **Demo Conference API**를 선택합니다.
3. **테스트** 탭으로 전환합니다.
4. **GetSpeakers** 작업을 선택합니다.
5. 값이 **true**로 설정된 **Ocp-Apim-Trace**라는 HTTP 헤더를 포함해야 합니다.

    > [!NOTE]
    > Ocp-Apim-Subscription-Key가 자동으로 채워지지 않는 경우 개발자 포털로 이동하여 프로필 페이지에 키를 노출하면 이 값을 가져올 수 있습니다.

6. **"보내기"** 를 클릭하여 API 호출을 수행합니다. 
7. 호출이 완료될 때까지 기다립니다. 
8. **API 콘솔**의 **추적** 탭으로 이동합니다. **인바운드**, **백 엔드**, **아웃바운드** 링크 중 하나를 클릭하여 자세한 추적 정보로 이동할 수 있습니다.

    **인바운드** 섹션에서는 API Managementrk 호출자로부터 수신한 원래 요청과 2단계에서 추가한 rate-limit 및 set-header 정책을 비롯하여 요청에 적용되는 모든 정책을 확인합니다.

    **백 엔드** 섹션에서는 API Management가 API 백 엔드로 전송한 요청과 수신된 응답을 확인합니다.

    **아웃바운드** 섹션에서는 호출자로 응답을 다시 보내기 전에 응답에 적용되는 모든 정책을 확인합니다.

    > [!TIP]
    > 각 단계는 API Management에서 요청이 수신된 이후에 경과된 시간도 표시합니다.

## <a name="next-steps"></a>다음 단계

이 자습서에서는 다음 방법에 대해 알아보았습니다.

> [!div class="checklist"]
> * 호출 추적

다음 자습서를 진행합니다.

> [!div class="nextstepaction"]
> [수정 버전 사용](api-management-get-started-revise-api.md)
