---
title: Azure Portal을 사용한 모의 API 응답 | Microsoft Docs
description: 이 자습서에서는 모의 응답을 반환하도록 APIM(API Management)을 사용하여 API에 대한 정책을 설정하는 방법을 보여 줍니다. 이 메서드를 통해 개발자는 백 엔드가 실제 응답을 보낼 수 없는 경우 API Management 인스턴스의 구현 및 테스트를 진행할 수 있습니다.
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
ms.openlocfilehash: 9b9a691cb2bce2357d184420912ab340aee534e8
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67205315"
---
# <a name="mock-api-responses"></a>모의 API 응답

백 엔드 API를 APIM API로 가져오거나 수동으로 만들고 관리할 수 있습니다. 이 자습서의 단계는 APIM을 사용하여 빈 API를 만들고 수동으로 관리하는 방법을 보여 줍니다. 자습서에서는 모의 응답을 반환하도록 API에 대한 정책을 설정하는 방법을 보여 줍니다. 이 메서드를 통해 개발자는 백 엔드가 실제 응답을 보낼 수 없는 경우에도 APIM 인스턴스의 구현 및 테스트를 진행할 수 있습니다. 모의 응답을 만드는 기능은 다양한 시나리오에서 유용할 수 있습니다.

+ API 외관이 먼저 설계되고 나중에 백 엔드가 구현된 경우 또는 백 엔드가 함께 개발되는 경우
+ 백 엔드가 일시적으로 작동하지 않거나 크기를 조정할 수 없는 경우

이 자습서에서는 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * 테스트 API 만들기 
> * 테스트 API에 작업 추가
> * 모의 응답 사용
> * 모의 API 테스트

![모의 작업 응답](./media/mock-api-responses/mock-api-responses01.png)

## <a name="prerequisites"></a>필수 조건

+ [Azure API Management 용어](api-management-terminology.md)를 익힙니다.
+ [Azure API Management의 정책 개념](api-management-howto-policies.md)을 이해합니다.
+ 다음 빠른 시작을 완료합니다. [Azure API Management 인스턴스 만들기](get-started-create-service-instance.md)

## <a name="create-a-test-api"></a>테스트 API 만들기 

이 섹션의 단계에서는 백 엔드 없는 빈 API를 만드는 방법을 보여 줍니다. 또한 API에 작업을 추가하는 방법을 보여 줍니다. 이 섹션의 단계를 완료한 후 작업을 호출하면 오류가 발생합니다. "모의 응답 사용" 섹션의 단계를 완료하면 오류가 발생하지 않습니다.

![빈 API 만들기](./media/mock-api-responses/03-MockAPIResponses-01-CreateTestAPI.png)

1. **API Management** 서비스에서 **API**를 선택합니다.
2. 왼쪽 메뉴에서 **+API 추가**를 선택합니다.
3. 목록에서 **빈 API**를 선택합니다.
4. **표시 이름**에 "*테스트 API*"를 입력합니다.
5. **제품**에 "*무제한*"을 입력합니다.
6. **만들기**를 선택합니다.

## <a name="add-an-operation-to-the-test-api"></a>테스트 API에 작업 추가

![API에 작업 추가](./media/mock-api-responses/03-MockAPIResponses-02-AddOperation.png)

1. 이전 단계에서 만든 API를 선택합니다.
2. **+작업 추가**를 클릭합니다.

    | 설정             | 값                             | 설명                                                                                                                                                                                   |
    |---------------------|-----------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | **표시 이름**    | *테스트 호출*                       | **개발자 포털**에 표시되는 이름입니다.                                                                                                                                       |
    | **URL**(HTTP 동사) | GET                               | 미리 정의된 HTTP 동사 중 하나를 선택할 수 있습니다.                                                                                                                                         |
    | **URL**             | */테스트*                           | API에 대한 URL 경로입니다.                                                                                                                                                                       |
    | **설명**     |                                   | **개발자 포털**에서 이 API를 사용하는 개발자에게 문서를 제공하는 데 사용되는 작업에 대한 설명을 제공합니다.                                                    |
    | **쿼리** 탭       |                                   | 쿼리 매개 변수를 추가할 수 있습니다. 이름 및 설명 제공 외에도 이 매개 변수에 할당될 수 있는 값을 제공할 수 있습니다. 값 중 하나를 기본값으로 표시할 수 있습니다(선택 사항). |
    | **요청** 탭     |                                   | 요청 콘텐츠 형식, 예제 및 스키마를 정의할 수 있습니다.                                                                                                                                  |
    | **응답** 탭    | 이 테이블 다음에 나오는 단계를 참조하세요. | 응답 상태 코드, 콘텐츠 형식, 예제 및 스키마를 정의합니다.                                                                                                                           |

3. URL, 표시 이름 및 설명 필드 아래에 있는 **응답** 탭을 선택합니다.
4. **+응답 추가**를 클릭합니다.
5. 목록에서 **200 정상**을 선택합니다.
6. 오른쪽의 **표시** 제목 아래에서 **+표시 추가**를 선택합니다.
7. "*애플리케이션/json*"을 검색 상자에 입력하고 **애플리케이션/json** 콘텐츠 형식을 선택합니다.
8. **샘플** 텍스트 상자에 `{ 'sampleField' : 'test' }`를 입력합니다.
9. **만들기**를 선택합니다.

## <a name="enable-response-mocking"></a>모의 응답 사용

![모의 응답 사용](./media/mock-api-responses/03-MockAPIResponses-03-EnableMocking.png)

1. "테스트 API 만들기" 단계에서 만든 API를 선택합니다.
2. 추가한 테스트 작업을 선택합니다.
3. 오른쪽 창에서 **디자인** 탭을 클릭합니다.
4. **인바운드 처리** 창에서 **+ 정책 추가**를 클릭합니다.
5. 갤러리에서 **모의 응답** 타일을 선택합니다.

    ![모의 응답 정책 타일](./media/mock-api-responses/mock-responses-policy-tile.png)

6. **API Management 응답** 텍스트 상자에 **200 정상, 응용 프로그램/json**을 입력합니다. 이 섹션은 API가 이전 섹션에서 정의한 응답을 반환해야 함을 나타냅니다.

    ![모의 응답 사용](./media/mock-api-responses/mock-api-responses-set-mocking.png)

7. **저장**을 클릭합니다.

## <a name="test-the-mocked-api"></a>모의 API 테스트

![모의 API 테스트](./media/mock-api-responses/03-MockAPIResponses-04-TestMocking.png)

1. "테스트 API 만들기" 단계에서 만든 API를 선택합니다.
2. **테스트** 탭을 엽니다.
3. **테스트 호출** API가 선택되었는지 확인합니다.

    > [!TIP]
    > **모의가 활성화됨** 텍스트가 있는 노란색 막대는 응답이 API Management에서 반환되었음을 나타내고, 실제 백 엔드 응답이 아닌 모의 정책을 전송합니다.

4. **보내기**를 선택하여 테스트 호출을 수행합니다.
5. **HTTP 응답**은 자습서의 첫 번째 섹션에서 샘플로 제공된 JSON을 표시합니다.

    ![모의 응답 사용](./media/mock-api-responses/mock-api-responses-test-response.png)

## <a name="video"></a>비디오

> [!VIDEO https://www.youtube.com/embed/i9PjUAvw7DQ]

## <a name="next-steps"></a>다음 단계

이 자습서에서는 다음 방법에 대해 알아보았습니다.

> [!div class="checklist"]
> * 테스트 API 만들기
> * 테스트 API에 작업 추가
> * 모의 응답 사용
> * 모의 API 테스트

다음 자습서를 진행합니다.

> [!div class="nextstepaction"]
> [게시된 API 변환 및 보호](transform-api.md)
