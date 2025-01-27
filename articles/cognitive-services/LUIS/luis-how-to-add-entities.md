---
title: 엔터티 추가
titleSuffix: Language Understanding - Azure Cognitive Services
description: LUIS (Language Understanding) 앱에서 사용자 표현에서 키 데이터를 추출 하는 엔터티를 만듭니다.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 04/01/2019
ms.author: diberry
ms.openlocfilehash: 241e89ac7fa78184e7c55f9e8065e1534cea9143
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65148721"
---
# <a name="create-entities-without-utterances"></a>발화 없는 엔터티 만들기

엔터티는 추출하려는 발화 내의 단어 또는 구를 나타냅니다. 엔터티 (장소, 사물, 사용자, 이벤트 또는 개념)와 유사한 개체의 컬렉션을 포함 하 여 클래스를 나타냅니다. 엔터티는 의도와 관련된 정보를 설명하며 앱이 작업을 수행하는 데 꼭 필요한 경우도 있습니다. (앞 이나 뒤)에서 utterance 의도 또는 간격을 추가 하면 엔터티를 만들 수 있습니다를 utterance 의도에 추가 합니다.

**엔터티** 페이지의 **엔터티 목록**을 통해 LUIS 앱에서 엔터티를 추가, 편집 또는 삭제할 수 있습니다. LUIS는 두 가지 유형의 주요 엔터티인 [미리 빌드된 엔터티](luis-reference-prebuilt-entities.md) 및 고유한 [사용자 지정 엔터티](luis-concept-entity-types.md#types-of-entities)를 제공합니다.

기계 학습 엔터티를 만든 후 해당 엔터티에 있는 모든 의도의 모든 예제 utterance 표시 해야 합니다.

<a name="add-prebuilt-entity"></a>

## <a name="add-a-prebuilt-entity-to-your-app"></a>앱에 미리 작성 된 엔터티를 추가 합니다.

애플리케이션에 추가되는 일반적인 미리 빌드된 엔터티는 *number*와 *datetimeV2*입니다. 

1. 앱의 **빌드** 섹션에서 왼쪽 패널의 **엔터티**를 선택합니다.
 
1. **엔터티** 페이지에서 **미리 빌드된 엔터티 추가**를 선택합니다.

1. **미리 빌드된 엔터티 추가** 대화 상자에서 미리 빌드된 엔터티인 **number** 및 **datetimeV2**를 선택합니다. 그런 후 **완료**를 선택합니다.

    ![미리 빌드된 엔터티 추가 대화 상자 스크린샷](./media/add-entities/list-of-prebuilt-entities.png)

<a name="add-simple-entities"></a>

## <a name="add-simple-entities-for-single-concepts"></a>단일 개념에 대 한 간단한 엔터티를 추가 합니다.

단순 엔터티는 단일 개념을 설명합니다. 다음 프로시저를 사용하여 *인적 자원* 또는 *운영*과 같은 회사 부서명을 추출하는 엔터티를 만듭니다.   

1. 앱의 **빌드** 섹션을 선택한 다음, 왼쪽 패널에서 **엔터티**를 선택하고 **새 엔터티 만들기**를 선택합니다.

1. 팝업 대화 상자의 **엔터티 이름** 상자에 `Location`을 입력하고 **엔터티 형식** 목록에서 **단순**을 선택한 후, **완료**를 선택합니다.

    이 엔터티를 만든 후 엔터티를 포함하는 예제 발화가 있는 모든 의도로 이동합니다. 예제 발화에서 텍스트를 선택하고 엔터티로 텍스트를 표시합니다. 

    [구 목록](luis-concept-feature.md)은 일반적으로 단순한 엔터티의 신호를 향상시키는 데 사용됩니다.

<a name="add-regular-expression-entities"></a>

## <a name="add-regular-expression-entities-for-highly-structured-concepts"></a>고도로 구조화 된 개념에 대 한 정규식 엔터티를 추가 합니다.

정규식 엔터티는 제공된 정규식에 따라 발언에서 데이터를 끌어오는 데 사용됩니다. 

1. 앱의 왼쪽 탐색 영역에서 **엔터티**를 선택하고 **새 엔터티 만들기**를 선택합니다.

1. 팝업 대화 상자의 **엔터티 이름** 상자에 `Human resources form name`를 입력하고 **엔터티 형식** 목록에서 **정규식**을 선택하고 정규식 `hrf-[0-9]{6}`를 입력한 후 **완료**를 선택합니다. 

    이 정규식 리터럴 문자와 대응 `hrf-`, 인적 자원 폼에는 폼에 다음 6 자리 숫자입니다.

<a name="add-composite-entities"></a>

## <a name="add-composite-entities-to-group-into-a-parent-child-relationship"></a>부모-자식 관계를 그룹화 하려면 복합 엔터티 추가

복합 엔터티를 만들어 서로 다른 유형의 엔터티 간 관계를 정의할 수 있습니다. 다음 예제에서 엔터티에는 정규식 및 미리 빌드된 이름의 엔터티가 포함됩니다.  

발화 `Send hrf-123456 to John Smith`에서 텍스트 `hrf-123456`는 인적 자원 [정규식](#add-regular-expression-entities)과 일치하고, `John Smith`는 미리 빌드된 엔터티 personName을 통해 추출됩니다. 각 엔터티는 더 큰 부모 엔터티의 일부입니다. 

1. 앱에서 **빌드** 섹션의 왼쪽 탐색 영역에서 **엔터티**를 선택한 다음, **미리 빌드된 엔터티 추가**를 선택합니다.

1. 미리 빌드된 엔터티 **PersonName**을 추가합니다. 지침을 보려면 [미리 빌드된 엔터티 추가](#add-prebuilt-entity)를 참조하세요. 

1. 왼쪽 탐색 영역에서 **엔터티**를 선택하고 **새 엔터티 만들기**를 선택합니다.

1. 팝업 대화 상자의 **엔터티 이름** 상자에 `SendHrForm`을 입력한 다음, **엔터티 형식** 목록에서 **복합**을 선택합니다.

1. **자식 추가**를 선택하여 새 자식을 추가합니다.

1. **자식 #1**에서 목록에 있는 **number** 엔터티를 선택합니다.

1. **자식 #2**의 목록에서 **인적 자원 양식 이름** 엔터티를 선택합니다. 

1. **완료**를 선택합니다.

<a name="add-pattern-any-entities"></a>

## <a name="add-patternany-entities-to-capture-free-form-entities"></a>자유 형식 엔터티를 캡처할 Pattern.any 엔터티 추가

[Pattern.any](luis-concept-entity-types.md) 엔터티는 의도가 아닌 [패턴](luis-how-to-model-intent-pattern.md)에서만 유효합니다. 이 유형의 엔터티는 LUIS가 다양한 길이 및 단어 선택이 포함된 엔터티의 끝을 찾는 데 도움을 줍니다. 이 엔터티는 패턴에 사용되므로 LUIS는 발화 템플릿에서 엔터티의 끝 위치를 알 수 있습니다.

앱에 `FindHumanResourcesForm` 의도가 있는 경우 추출된 양식 제목이 의도 예측에 방해가 될 수 있습니다. 양식 제목에 있는 단어를 명확하게 하려면 패턴 내에 Pattern.any를 사용합니다. LUIS 예측은 발언에서 시작됩니다. 먼저 발언이 확인되고 엔터티에 대해 일치된 후, 엔터티가 검색되면 패턴이 확인되고 일치됩니다. 

발언 `Where is Request relocation from employee new to the company on the server?`에서 제목이 끝나는 위치와 나머지 발언이 시작되는 위치가 문맥상 명확하지 않으므로 양식 제목을 찾기가 어렵습니다. 제목은 단일 단어, 문장 부호가 포함된 복합 구 및 무의미한 단어 배열을 포함하여 모든 순서의 단어가 될 수 있습니다. 전체 및 정확한 엔터티를 추출할 수 있는 경우 패턴을 사용하여 엔터티를 만들 수 있습니다. 제목이 검색되면 해당 제목이 패턴의 의도이므로 `FindHumanResourcesForm` 의도가 예측됩니다.

1. **빌드** 섹션에서 왼쪽 패널의 **엔터티**를 선택한 다음, **새 엔터티 만들기**를 선택합니다.

1. **엔터티 추가** 대화 상자의 **엔터티 이름** 상자에 `HumanResourcesFormTitle`을 입력하고 **Pattern.any**를 **엔터티 형식**으로 선택합니다.

    pattern.any 엔터티를 사용하려면 올바른 중괄호 구문(예: `Where is **{HumanResourcesFormTitle}** on the server?`)을 사용하여 **앱 성능 개선** 섹션의 **패턴** 페이지에서 패턴을 추가합니다.

    Pattern.any가 포함된 패턴이 엔터티를 잘못 추출한 것을 발견하면 [명시적 목록](luis-concept-patterns.md#explicit-lists)을 사용하여 이 문제를 정정합니다. 

<a name="add-a-role-to-pattern-based-entity"></a>

## <a name="add-a-role-to-distinguish-different-contexts"></a>다른 컨텍스트를 구분 하기 위해 역할 추가

역할은 컨텍스트를 기반으로 명명 된 하위 형식입니다. 미리 빌드된 및 기계 학습 되지 않은 엔터티를 포함 하 여 모든 엔터티에서 제공 됩니다. 

역할에 대 한 구문은 **`{Entityname:Rolename}`** 콜론을 역할 이름으로 엔터티 이름 뒤에 위치 합니다. 예: `Move {personName} from {LocationUsingRoles:Origin} to {LocationUsingRoles:Destination}`.

1. **빌드** 섹션에서 왼쪽 패널의 **엔터티**를 선택합니다.

1. **새 엔터티 만들기**를 선택합니다. `LocationUsingRoles`의 이름을 입력합니다. **단순** 형식을 선택하고 **완료**를 선택합니다. 

1. 왼쪽 패널에서 **엔터티**를 선택한 다음, 이전 단계에서 만든 새 엔터티 **LocationUsingRoles**을 선택합니다.

1. **역할 이름** 텍스트 상자에 역할 이름 `Origin`을 입력한 후 Enter 키를 누릅니다. 두 번째 역할 이름 `Destination`을 추가합니다. 

    ![Location 엔터티에 Origin 역할을 추가하는 스크린샷](./media/add-entities/roles-enter-role-name-text.png)

<a name="add-list-entities"></a>

## <a name="add-list-entities-for-exact-matches"></a>정확한 일치 항목에 대 한 목록 엔터티를 추가 합니다.

목록 엔터티는 관련 단어의 고정된 폐쇄형 집합을 나타냅니다. 

인적 자원 앱의 경우 모든 부서의 동의어와 함께 모든 부서의 목록이 있을 수 있습니다. 엔터티를 만들 때 모든 값을 알 필요는 없습니다. 동의어를 포함하는 실제 사용자 발언을 검토한 후에 더 많은 발언을 추가할 수 있습니다.

1. **빌드** 섹션에서 왼쪽 패널의 **엔터티**를 선택한 다음, **새 엔터티 만들기**를 선택합니다.

1. **엔터티 추가** 대화 상자에서 **엔터티 이름** 상자에 `Department`을 입력하고 **엔터티 형식**으로 **List**를 선택합니다. **완료**를 선택합니다.
  
1. 목록 엔터티 페이지에서 정규화된 이름을 추가할 수 있습니다. **값** 텍스트 상자에 목록의 부서명(예: `HumanResources`)을 입력한 다음, 키보드에서 Enter 키를 누릅니다. 

1. 정규화된 값의 오른쪽에 동의어를 입력하고 각 항목을 입력한 후 키보드에서 Enter 키를 누릅니다.

1. 목록에 대해 정규화된 항목을 더 원할 경우 **권장**을 선택하여 [의미 사전](luis-glossary.md#semantic-dictionary)의 옵션을 표시합니다.

    ![옵션을 보기 위해 권장 기능을 선택하는 스크린샷](./media/add-entities/hr-list-2.png)


1. 권장 목록에서 항목을 선택하여 정규화된 값으로 추가하거나 **모두 추가**를 선택하여 모든 항목을 추가합니다. 
    다음과 같은 JSON 형식을 사용하여 기존 목록 엔터티로 값을 가져올 수 있습니다.

    ```JSON
    [
        {
            "canonicalForm": "Blue",
            "list": [
                "navy",
                "royal",
                "baby"
            ]
        },
        {
            "canonicalForm": "Green",
            "list": [
                "kelly",
                "forest",
                "avacado"
            ]
        }
    ]  
    ```

<a name="change-entity-type"></a>

## <a name="do-not-change-entity-type"></a>엔터티 형식을 변경 하지 마세요

LUIS에서는 해당 엔터티를 생성하기 위해 추가 또는 제거할 항목을 잘 모르기 때문에 엔터티의 형식을 변경하도록 허용하지 않습니다. 형식을 변경하기 위해서는 약간 다른 이름을 사용하여 올바른 형식의 새 엔터티를 만드는 것이 좋습니다. 엔터티가 만들어지면 각 발언에서 이전 레이블이 지정된 엔터티 이름을 제거한 후 새 엔터티 이름을 추가합니다. 모든 발언에 레이블이 다시 지정되면 이전 엔터티를 삭제합니다. 

<a name="create-a-pattern-from-an-utterance"></a>

## <a name="create-a-pattern-from-an-example-utterance"></a>예제 utterance에서 패턴을 만들려면

[의도 또는 엔터티 페이지의 기존 발언에서 패턴 추가](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page)를 참조하세요.

## <a name="train-your-app-after-changing-model-with-entities"></a>엔터티가 포함된 모델을 변경한 후 앱 학습

엔터티를 추가, 편집 또는 제거한 후 변경 내용을 엔드포인트 쿼리에 적용하려면 앱을 [학습](luis-how-to-train.md)시키고 [게시](luis-how-to-publish-app.md)합니다. 

## <a name="next-steps"></a>다음 단계

미리 빌드된 엔터티에 대한 자세한 내용은 [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text) 프로젝트를 참조하세요. 

JSON 엔드포인트 쿼리 응답에 엔터티가 표시되는 방법에 대한 자세한 내용은 [데이터 추출](luis-concept-data-extraction.md) 참조

의도, 발언 및 엔터티가 추가되어 기본 LUIS 앱이 구성되었습니다. 앱을 [학습](luis-how-to-train.md), [테스트](luis-interactive-test.md) 및 [게시](luis-how-to-publish-app.md)하는 방법을 알아봅니다.
 
