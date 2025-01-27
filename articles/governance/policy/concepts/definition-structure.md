---
title: 정책 정의 구조에 대한 세부 정보
description: 정책이 언제 적용되고 어떤 영향이 있는지 설명함으로써 Azure Policy가 조직의 리소스에 대한 규칙을 설정하기 위해 리소스 정책 정의가 어떻게 사용되는지 설명합니다.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/13/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 03c7be9112ed22bb43e259fa72581d382a276163
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67718191"
---
# <a name="azure-policy-definition-structure"></a>Azure Policy 정의 구조

리소스 정책 정의는 Azure Policy에서 리소스에 대한 규칙을 설정하는 데 사용됩니다. 각 정의는 리소스 규정 준수 및 리소스가 규정을 준수하지 않을 때 적용되는 영향에 대해 설명합니다.
규칙을 정의하여 비용을 제어하고 리소스를 보다 쉽게 관리할 수 있습니다. 예를 들어, 특정 유형의 가상 머신만 허용되게 지정할 수 있습니다. 또는 모든 리소스가 특정 태그를 갖도록 요구할 수 있습니다. 정책은 모든 자식 리소스에 의해 상속됩니다. 리소스 그룹에 정책을 적용하면 해당 리소스 그룹의 모든 리소스에 해당 정책을 적용할 수 있습니다.

여기에서는 Azure Policy에서 사용하는 스키마를 찾을 수 있습니다. [https://schema.management.azure.com/schemas/2018-05-01/policyDefinition.json](https://schema.management.azure.com/schemas/2018-05-01/policyDefinition.json)

JSON을 사용하여 정책 정의를 만듭니다. 정책 정의에는 다음 요소가 포함됩니다.

- 모드
- parameters
- 표시 이름
- description
- 정책 규칙
  - 논리 평가
  - 영향

예를 들어 다음 JSON에서는 리소스가 배포되는 위치를 제한하는 정책을 보여줍니다.

```json
{
    "properties": {
        "mode": "all",
        "parameters": {
            "allowedLocations": {
                "type": "array",
                "metadata": {
                    "description": "The list of locations that can be specified when deploying resources",
                    "strongType": "location",
                    "displayName": "Allowed locations"
                },
                "defaultValue": [ "westus2" ]
            }
        },
        "displayName": "Allowed locations",
        "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
        "policyRule": {
            "if": {
                "not": {
                    "field": "location",
                    "in": "[parameters('allowedLocations')]"
                }
            },
            "then": {
                "effect": "deny"
            }
        }
    }
}
```

모든 Azure Policy 샘플은 [Azure Policy 샘플](../samples/index.md)합니다.

[!INCLUDE [az-powershell-update](../../../../includes/updated-for-az.md)]

## <a name="mode"></a>모드

**모드** 되는 Azure 리소스 관리자 속성 또는 속성에 리소스 공급자 정책을 대상으로 하는 경우에 따라 구성 합니다.

### <a name="resource-manager-modes"></a>Resource Manager 모드

**mode**는 정책에 대해 평가할 리소스 종류를 결정합니다. 지원되는 모드는 다음과 같습니다.

- `all`: 리소스 그룹 및 모든 리소스 종류를 평가합니다.
- `indexed`: 태그 및 위치를 지원하는 리소스 종류만 평가합니다.

대부분 **mode**를 `all`로 설정하는 것이 좋습니다. 포털을 통해 생성된 모든 정책 정의는 `all` 모드를 사용합니다. PowerShell 또는 Azure CLI를 사용하는 경우 **mode** 매개 변수를 수동으로 지정할 수 있습니다. 정책 정의에 **mode** 값이 포함되지 않으면 기본적으로 Azure PowerShell에서는 `all`로 설정되고 Azure CLI에서는 `null`로 설정됩니다. `null` 모드는 이전 버전과의 호환성을 지원하기 위해 `indexed`를 사용하는 것과 같습니다.

`indexed`는 태그 또는 위치를 시스템에 적용하는 정책을 만들 때 사용해야 합니다. 이 모드는 반드시 사용해야 하는 것은 아니지만, 사용하는 경우 태그와 위치를 지원하지 않는 리소스가 규정 준수 결과에 미준수 항목으로 표시되지 않습니다. 예외는 **리소스 그룹**입니다. 리소스 그룹에서 위치 또는 태그를 적용하는 정책은 **mode**를 `all`로 설정하고 구체적으로 `Microsoft.Resources/subscriptions/resourceGroups` 형식을 대상으로 지정해야 합니다. 예를 들어 [리소스 그룹 태그 적용](../samples/enforce-tag-rg.md)을 참조하세요. 태그를 지 원하는 리소스의 목록을 참조 하세요 [지원 Azure 리소스에 대 한 태그](../../../azure-resource-manager/tag-support.md)합니다.

### <a name="resource-provider-modes"></a>리소스 공급자 모드

현재 지원 되는 리소스 공급자 모드 `Microsoft.ContainerService.Data` 입학 허가 컨트롤러 규칙에서 관리 하는 것에 대 한 [Azure Kubernetes Service](../../../aks/intro-kubernetes.md)합니다.

> [!NOTE]
> [Kubernetes에 대 한 azure Policy](rego-for-aks.md) 공개 미리 보기로 제공 되며만 기본 제공 정책 정의 지원 합니다.

## <a name="parameters"></a>매개 변수

매개 변수는 정책 정의의 수를 줄여 정책 관리를 간소화하는 데 도움이 됩니다. 양식의 필드 `name`, `address`, `city`, `state`와 같은 매개 변수에 관해 생각해 봅니다. 이러한 매개 변수는 항상 그대로 유지되지만, 그 값은 양식을 작성하는 개별 값에 기초하여 달라집니다.
매개 변수는 정책을 만들 때와 같은 방법으로 작동합니다. 정책 정의에 매개 변수를 포함함으로써 서로 다른 값을 사용하여 다양한 시나리오에 대해 해당 정책을 재사용할 수 있습니다.

> [!NOTE]
> 매개 변수를 기존 및 할당된 정의에 추가할 수 있습니다. 새 매개 변수는 **defaultValue** 속성을 포함해야 합니다. 이렇게 하면 정책 또는 이니셔티브의 기존 지정이 간접적으로 유효하지 않게 됩니다.

### <a name="parameter-properties"></a>매개 변수 속성

매개 변수에는 정책 정의에 사용되는 다음 속성이 있습니다.

- **name**: 매개 변수의 이름입니다. 정책 규칙 내의 `parameters` 배포 함수에서 사용됩니다. 자세한 내용은 [매개 변수 값 사용](#using-a-parameter-value)을 참조하세요.
- `type`: 매개 변수 인지 여부를 확인 한 **문자열**, **배열**, **개체**, **부울**, **정수**, **부동 소수점**, 또는 **datetime**합니다.
- `metadata`: Azure Portal에서 사용자에게 친숙한 정보를 표시하는 데 주로 사용되는 하위 속성을 정의합니다.
  - `description`: 매개 변수의 용도에 대한 설명입니다. 허용 가능한 값의 예를 제공하는 데 사용할 수 있습니다.
  - `displayName`: 매개 변수에 대해 포털에 표시되는 이름입니다.
  - `strongType`: (선택 사항) 포털을 통해 정책 정의를 할당할 때 사용됩니다. 컨텍스트 인식 목록을 제공합니다. 자세한 내용은 [strongType](#strongtype)을 참조하세요.
  - `assignPermissions`: (선택 사항) 로 _true_ 정책 할당 하는 동안 역할 할당을 만들거나 Azure portal이 있어야 합니다. 이 속성은 할당 범위를 벗어나는 사용 권한을 할당 하려는 경우에 유용 합니다. 첫 번째 역할 할당 정책에서 역할 정의 (또는 이니셔티브 정책의 모든 역할 정의 단위) 있습니다. 매개 변수 값은 유효한 리소스 또는 범위 여야 합니다.
- `defaultValue`: (선택 사항) 값이 지정되지 않은 경우 할당에서 매개 변수의 값을 설정합니다.
  할당된 기존 정책 정의를 업데이트할 때 필요합니다.
- `allowedValues`: (선택 사항) 매개 변수에 할당 하는 동안 허용 하는 값의 배열을 제공 합니다.

예를 들어 리소스를 배포할 수 있는 위치를 제한하는 정책 정의를 정의할 수 있습니다. 해당 정책 정의의 매개 변수는 **allowedLocations**일 수 있습니다. 이 매개 변수는 정책 정의의 각 할당에서 허용되는 값을 제한하는 데 사용됩니다. **strongType**을 사용하면 포털을 통해 할당을 완료할 때 경험이 개선됩니다.

```json
"parameters": {
    "allowedLocations": {
        "type": "array",
        "metadata": {
            "description": "The list of allowed locations for resources.",
            "displayName": "Allowed locations",
            "strongType": "location"
        },
        "defaultValue": [ "westus2" ],
        "allowedValues": [
            "eastus2",
            "westus2",
            "westus"
        ]
    }
}
```

### <a name="using-a-parameter-value"></a>매개 변수 값 사용

정책 규칙에서 다음 `parameters` 배포 값 함수 구문을 사용하여 매개 변수를 참조합니다.

```json
{
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

이 샘플은 [매개 변수 속성](#parameter-properties)에 설명된 **allowedLocations** 매개 변수를 참조합니다.

### <a name="strongtype"></a>strongType

`metadata` 속성 안에 **strongType**을 사용하여 Azure Portal 내에서 다중 선택 옵션 목록을 제공할 수 있습니다. **strongType**에서 현재 허용되는 값은 다음과 같습니다.

- `location`
- `resourceTypes`
- `storageSkus`
- `vmSKUs`
- `existingResourceGroups`
- `omsWorkspace`
- `Microsoft.EventHub/Namespaces/EventHubs`
- `Microsoft.EventHub/Namespaces/EventHubs/AuthorizationRules`
- `Microsoft.EventHub/Namespaces/AuthorizationRules`
- `Microsoft.RecoveryServices/vaults`
- `Microsoft.RecoveryServices/vaults/backupPolicies`

## <a name="definition-location"></a>정의 위치

이니셔티브 또는 정책을 만드는 동안 정의 위치를 지정해야 합니다. 정의 위치는 관리 그룹 또는 구독이어야 합니다. 이 위치는 이니셔티브 또는 정책을 할당할 수 있는 범위를 결정합니다. 리소스는 할당 대상으로 지정할 정의 위치의 계층 구조 내의 직접 멤버 또는 자식 멤버여야 합니다.

정의 위치는 다음과 같습니다.

- **구독** - 해당 구독 내의 리소스만 정책에 할당할 수 있습니다.
- **관리 그룹**  - 하위 관리 그룹과 자식 구독 내의 리소스만 정책에 할당할 수 있습니다. 정책 정의를 여러 구독에 적용하려는 경우 위치는 이러한 구독이 포함된 관리 그룹이어야 합니다.

## <a name="display-name-and-description"></a>표시 이름 및 설명

**displayName** 및 **description**을 사용하여 정책 정의를 식별하고 사용하는 시기에 대한 컨텍스트를 제공합니다. **displayName**은 최대 길이가 _128_자이고 **description**은 최대 길이가 _512_자입니다.

## <a name="policy-rule"></a>정책 규칙

정책 규칙은 **If** 및 **Then** 블록으로 이루어집니다. **If** 블록에서 정책이 적용되는 시점을 지정하는 하나 이상의 조건을 정의합니다. 이러한 조건에 논리 연산자를 적용하여 정책에 대한 시나리오를 정확하게 정의할 수 있습니다.

**Then** 블록에서 **If** 조건이 충족된 경우에 발생하는 효과를 정의합니다.

```json
{
    "if": {
        <condition> | <logical operator>
    },
    "then": {
        "effect": "deny | audit | append | auditIfNotExists | deployIfNotExists | disabled"
    }
}
```

### <a name="logical-operators"></a>논리 연산자

지원되는 논리 연산자는 다음과 같습니다.

- `"not": {condition  or operator}`
- `"allOf": [{condition or operator},{condition or operator}]`
- `"anyOf": [{condition or operator},{condition or operator}]`

**not** 구문은 조건의 결과를 반전시킵니다. **allOf** 구문(논리 **And** 작업과 비슷함)은 모든 조건이 true여야 합니다. **anyOf** 구문(논리 **Or** 작업과 비슷함)은 하나 이상의 조건이 true여야 합니다.

논리 연산자를 중첩할 수 있습니다. 다음 예제에서는 **allOf** 작업 내에 중첩된 **not** 작업을 표시합니다.

```json
"if": {
    "allOf": [{
            "not": {
                "field": "tags",
                "containsKey": "application"
            }
        },
        {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
        }
    ]
},
```

### <a name="conditions"></a>조건

조건은 **field** 또는 **value** 접근자가 특정 기준을 충족하는지 여부를 평가합니다. 지원되는 조건은 다음과 같습니다.

- `"equals": "value"`
- `"notEquals": "value"`
- `"like": "value"`
- `"notLike": "value"`
- `"match": "value"`
- `"matchInsensitively": "value"`
- `"notMatch": "value"`
- `"notMatchInsensitively": "value"`
- `"contains": "value"`
- `"notContains": "value"`
- `"in": ["value1","value2"]`
- `"notIn": ["value1","value2"]`
- `"containsKey": "keyName"`
- `"notContainsKey": "keyName"`
- `"less": "value"`
- `"lessOrEquals": "value"`
- `"greater": "value"`
- `"greaterOrEquals": "value"`
- `"exists": "bool"`

**like** 및 **notLike** 조건을 사용하는 경우 값에 와일드카드 `*`를 제공합니다.
값에 와일드카드 `*`를 두 개 이상 포함하면 안 됩니다.

사용 하는 경우는 **일치** 하 고 **notMatch** 조건을 제공 `#` 숫자와 일치 하도록 `?` 문자에 대해 `.` 모든 문자와 일치 하도록 다른 모든 문자를 일치 하도록 해당 실제 문자입니다.
**match** 및 **notMatch**는 대/소문자를 구분합니다. 대/소문자를 구분하지 않는 대안은 **matchInsensitively** 및 **notMatchInsensitively**에서 확인할 수 있습니다. 예를 들어 [여러 이름 패턴 허용](../samples/allow-multiple-name-patterns.md)을 참조하세요.

### <a name="fields"></a>필드

조건은 필드를 사용하여 구성됩니다. 필드는 리소스 요청 페이로드의 속성을 일치시키고 리소스 상태를 설명합니다.

다음 필드가 지원됩니다.

- `name`
- `fullName`
  - 리소스의 전체 이름을 반환합니다. 리소스의 전체 이름은 리소스 이름에 상위 리소스 이름을 접두어로 추가하여 만듭니다(예: "myServer/myDatabase").
- `kind`
- `type`
- `location`
  - 위치에 관계없는 리소스에는 **전역**을 사용합니다. 예를 들어 [샘플 - 허용된 위치](../samples/allowed-locations.md)를 참조하세요.
- `identity.type`
  - 리소스에서 사용 가능한 [관리 ID](../../../active-directory/managed-identities-azure-resources/overview.md) 형식을 반환합니다.
- `tags`
- `tags['<tagName>']`
  - 이 대괄호 구문은 하이픈, 마침표, 공백 등의 문장 부호가 있는 태그 이름을 지원합니다.
  - 여기서 **\<tagName\>** 은 조건의 유효성을 검사하기 위한 태그 이름입니다.
  - 예: `tags['Acct.CostCenter']`. 여기서 **Acct.CostCenter**는 태그 이름입니다.
- `tags['''<tagName>''']`
  - 이 대괄호 구문은 이중 아포스트로피로 이스케이프 처리하여 아포스트로피가 있는 태그 이름을 지원합니다.
  - 여기서 **‘\<tagName\>’** 은 조건의 유효성을 검사할 태그 이름입니다.
  - 예: `tags['''My.Apostrophe.Tag''']`. 여기서 **‘\<tagName\>’** 은 태그 이름입니다.
- 속성 별칭 - 목록은 [별칭](#aliases)을 참조하세요.

> [!NOTE]
> `tags.<tagName>`, `tags[tagName]` 및 `tags[tag.with.dots]`도 여전히 허용되는 태그 필드 선언 방법입니다. 그러나 기본 식은 위에 나열된 식입니다.

#### <a name="use-tags-with-parameters"></a>매개 변수와 함께 태그 사용

매개 변수 값을 태그 필드에 전달할 수 있습니다. 매개 변수를 태그 필드에 전달하면 정책 할당 중에 정책 정의의 유연성이 증가합니다.

다음 예제에서 `concat`는 이름이 **tagName** 매개 변수의 값인 태그에 대한 태그 필드 조회를 만드는 데 사용됩니다. 해당 태그가 없는 경우 **append** 효과를 사용하여 `resourcegroup()` 조회 함수를 통해 감사된 리소스 부모 리소스 그룹에 설정된 동일한 이름의 태그 값을 사용하는 태그를 추가합니다.

```json
{
    "if": {
        "field": "[concat('tags[', parameters('tagName'), ']')]",
        "exists": "false"
    },
    "then": {
        "effect": "append",
        "details": [{
            "field": "[concat('tags[', parameters('tagName'), ']')]",
            "value": "[resourcegroup().tags[parameters('tagName')]]"
        }]
    }
}
```

### <a name="value"></a>값

**value**를 사용하여 조건을 구성할 수도 있습니다. **value**는 [매개 변수](#parameters), [지원되는 템플릿 함수](#policy-functions) 또는 리터럴에 대해 조건을 확인합니다.
**value**는 지원되는 모든 [조건](#conditions)과 쌍을 이룹니다.

> [!WARNING]
> 하는 경우의 결과 _템플릿 함수_ 정책에서 평가 오류가 발생 하면 오류가 발생 합니다. 실패 한 평가 암시적 **거부**합니다. 자세한 내용은 [템플릿 오류를 방지](#avoiding-template-failures)합니다.

#### <a name="value-examples"></a>값 예제

이 정책 규칙 예제는 **value**를 사용하여 `resourceGroup()` 함수의 결과와 반환된 **name** 속성을 `*netrg`의 **like** 조건과 비교합니다. 규칙은 이름이 `*netrg`로 끝나는 리소스 그룹에서 `Microsoft.Network/*` **type**이 아닌 리소스를 모두 거부합니다.

```json
{
    "if": {
        "allOf": [{
                "value": "[resourceGroup().name]",
                "like": "*netrg"
            },
            {
                "field": "type",
                "notLike": "Microsoft.Network/*"
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```

이 정책 규칙 예제에서는 **value**를 사용하여 여러 중첩 함수의 결과가 **equals** `true`인지 확인합니다. 이 규칙은 최소 3개의 태그가 없는 리소스를 모두 거부합니다.

```json
{
    "mode": "indexed",
    "policyRule": {
        "if": {
            "value": "[less(length(field('tags')), 3)]",
            "equals": true
        },
        "then": {
            "effect": "deny"
        }
    }
}
```

#### <a name="avoiding-template-failures"></a>템플릿 오류를 방지합니다.

사용 _템플릿 함수_ 에 **값** 많은 복잡 한 중첩 된 함수에 대 한 허용 합니다. 하는 경우의 결과 _템플릿 함수_ 정책에서 평가 오류가 발생 하면 오류가 발생 합니다. 실패 한 평가 암시적 **거부**합니다. 예는 **값** 특정 시나리오에서 실패 합니다.

```json
{
    "policyRule": {
        "if": {
            "value": "[substring(field('name'), 0, 3)]",
            "equals": "abc"
        },
        "then": {
            "effect": "audit"
        }
    }
}
```

사용 하 여 위의 예에서는 정책 규칙 [substring ()](../../../azure-resource-manager/resource-group-template-functions-string.md#substring) 의 처음 세 문자를 비교할 **이름** 하 **abc**합니다. 하는 경우 **이름을** 3 자 보다 짧은 `substring()` 함수 오류가 발생 합니다. 이 오류로 인해 되도록 정책을 **거부** 적용 합니다.

대신 합니다 [if()](../../../azure-resource-manager/resource-group-template-functions-logical.md#if) 함수를 확인의 처음 세 문자 **이름** 같으면 **abc** 허용 하지 않고를 **이름** 보다 짧은 오류가 발생 하는 세 문자:

```json
{
    "policyRule": {
        "if": {
            "value": "[if(greaterOrEquals(length(field('name')), 3), substring(field('name'), 0, 3), 'not starting with abc')]",
            "equals": "abc"
        },
        "then": {
            "effect": "audit"
        }
    }
}
```

수정 된 정책 규칙을 사용 하 여 `if()` 길이 확인 **이름** 가져오려고 시도 하기 전에 `substring()` 세 대 보다 적은 문자가 포함 된 값입니다. 하는 경우 **이름을** 너무 짧습니다., "abc로 시작 되지 않음" 값이 대신 반환 하 고 비교할 **abc**합니다. 시작 되지 않도록 하는 짧은 이름 사용 하 여 리소스 **abc** 정책 규칙을 여전히 실패 하지만 더 이상 평가 하는 동안 오류가 발생 합니다.

### <a name="effect"></a>영향

Azure Policy에는 다음과 같은 형식의 결과 지원합니다.

- **거부**: 활동 로그에 이벤트를 생성하고 요청을 실패합니다.
- **Audit**: 활동 로그에 경고 이벤트를 생성하지만 요청을 실패하지는 않습니다.
- **추가**는 정의된 필드 집합을 요청에 추가합니다.
- **AuditIfNotExists**: 리소스가 없으면 감사를 사용하도록 설정합니다.
- **DeployIfNotExists**: 아직 존재하지 않는 리소스를 배포합니다.
- **Disabled**: 정책 규칙 준수에 대해 리소스를 평가하지 않습니다.
- **EnforceRegoPolicy**: Azure Kubernetes Service (미리 보기)에서 열린 정책 에이전트 입학 컨트롤러 구성

**append**의 경우 아래와 같이 details(세부 정보)를 제공해야 합니다.

```json
"effect": "append",
"details": [{
    "field": "field name",
    "value": "value of the field"
}]
```

값은 문자열 또는 JSON 형식의 개체일 수 있습니다.

**AuditIfNotExists** 및 **DeployIfNotExists**는 관련 리소스의 존재를 평가하고 규칙을 적용합니다. 리소스가 규칙과 일치하지 않으면 영향이 구현됩니다. 예를 들어 모든 가상 네트워크에 대해 네트워크 감시자를 배포하도록 요구할 수 있습니다. 자세한 내용은 [확장이 없는 경우 감사](../samples/audit-ext-not-exist.md)를 참조하세요.

**DeployIfNotExists** 효과에는 정책 규칙의 **details** 부분에 **roleDefinitionId** 속성이 필요합니다. 자세한 내용은 [수정 - 정책 정의 구성](../how-to/remediate-resources.md#configure-policy-definition)을 참조하세요.

```json
"details": {
    ...
    "roleDefinitionIds": [
        "/subscription/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/{roleGUID}",
        "/providers/Microsoft.Authorization/roleDefinitions/{builtinroleGUID}"
    ]
}
```

각 효과 평가, 속성 및 예제를 순서에 대 한 자세한 내용은 참조 하세요. [이해 Azure 정책 효과](effects.md)합니다.

### <a name="policy-functions"></a>정책 함수

모든 [Resource Manager 템플릿 함수](../../../azure-resource-manager/resource-group-template-functions.md) 다음 함수와 사용자 정의 함수를 제외 하 고 정책 규칙에서 사용할 수 있습니다.

- copyIndex()
- deployment()
- list*
- newGuid()
- pickZones()
- providers()
- reference()
- resourceId()
- variables()

다음 함수는 정책 규칙을 사용 하지만 Azure Resource Manager 템플릿에서 사용 하 여에서 다를 수 있습니다.

- addDays(dateTime, numberOfDaysToAdd)
  - **dateTime**: 범용 ISO 8601 날짜/시간 형식에서 문자열 [필수] 문자열 ' yyyy-MM-ddTHH:mm:ss.fffffffZ'
  - **numberOfDaysToAdd**: [필수] 정수-추가할 일 수
- utcnow ()-는 Resource Manager와는 달리 템플릿에서 defaultValue 밖에 사용할 수 있습니다.
  - 현재 날짜 및 시간을 유니버설 ISO 8601 날짜/시간 형식으로 설정 된 문자열을 반환 합니다 ' yyyy-MM-ddTHH:mm:ss.fffffffZ'

`field` 함수도 정책 규칙에 사용할 수 있습니다. `field`는 주로 평가 중인 리소스의 필드를 참조하기 위해 **AuditIfNotExists** 및 **DeployIfNotExists**와 함께 사용합니다. 이 사용 예제는 [DeployIfNotExists 예제](effects.md#deployifnotexists-example)에서 볼 수 있습니다.

#### <a name="policy-function-example"></a>정책 함수 예제

이 정책 규칙 예제에서는 `resourceGroup` 리소스 함수를 `concat` 배열 및 개체 함수와 함께 사용하여 **name** 속성을 가져오고 리소스 이름을 리소스 그룹 이름으로 시작하도록 하는 `like` 조건을 작성합니다.

```json
{
    "if": {
        "not": {
            "field": "name",
            "like": "[concat(resourceGroup().name,'*')]"
        }
    },
    "then": {
        "effect": "deny"
    }
}
```

## <a name="aliases"></a>Aliases

리소스 유형에 대한 특정 속성에 액세스하려면 속성 별칭을 사용합니다. 별칭을 사용하면 리소스의 속성에 허용되는 값이나 조건을 제한할 수 있습니다. 각 별칭은 주어진 리소스 유형에 대해 서로 다른 API 버전의 경로에 매핑됩니다. 정책 평가 중에 정책 엔진은 해당 API 버전에 대한 속성 경로를 가져옵니다.

별칭의 목록은 항상 업데이트됩니다. 현재 Azure Policy에서 지원하는 별칭을 찾으려면 다음 방법 중 하나를 사용합니다.

- Azure PowerShell

  ```azurepowershell-interactive
  # Login first with Connect-AzAccount if not using Cloud Shell

  # Use Get-AzPolicyAlias to list available providers
  Get-AzPolicyAlias -ListAvailable

  # Use Get-AzPolicyAlias to list aliases for a Namespace (such as Azure Automation -- Microsoft.Automation)
  Get-AzPolicyAlias -NamespaceMatch 'automation'
  ```

- Azure CLI

  ```azurecli-interactive
  # Login first with az login if not using Cloud Shell

  # List namespaces
  az provider list --query [*].namespace

  # Get Azure Policy aliases for a specific Namespace (such as Azure Automation -- Microsoft.Automation)
  az provider show --namespace Microsoft.Automation --expand "resourceTypes/aliases" --query "resourceTypes[].aliases[].name"
  ```

- REST API/ARMClient

  ```http
  GET https://management.azure.com/providers/?api-version=2017-08-01&$expand=resourceTypes/aliases
  ```

### <a name="understanding-the--alias"></a>[*] 별칭 이해

사용 가능한 별칭 중 일부에 ‘정상’ 이름으로 표시되는 버전과 **[\*]** 가 추가된 다른 버전이 있습니다. 예를 들어:

- `Microsoft.Storage/storageAccounts/networkAcls.ipRules`
- `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]`

'Normal' 별칭은 단일 값으로 필드를 나타냅니다. 이 필드는 더 이상 및 더 작은 값의 전체 집합 정의 된 대로 정확 하 게 해야 하는 경우 정확 하 게 일치 비교 시나리오에 대 한 합니다.

합니다 **[\*]** 별칭을 사용 하면 배열에 있는 각 요소의 값 및 각 요소의 특정 속성에 대해 비교할 수 있습니다. 이 이렇게 하면 요소 속성을 비교 하 여 '없으면', '있는 경우 의' 또는 ' 모든 경우의 ' 시나리오입니다. 사용 하 여 **ipRules [\*]** , 예로 유효성을 검사 하는 모든 _동작_ 됩니다 _거부_, 얼마나 많은 규칙이 나 IP 걱정하지않지만_값_ 됩니다. 이 샘플 규칙의 일치 항목을 검사 **ipRules [\*].value** 에 **10.0.4.1** 적용 합니다 **effectType** 적어도 하나의 일치 항목을 찾지 못하면 해당 하는 경우에:

```json
"policyRule": {
    "if": {
        "allOf": [
            {
                "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
                "exists": "true"
            },
            {
                "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].value",
                "notEquals": "10.0.4.1"
            }
        ]
    },
    "then": {
        "effect": "[parameters('effectType')]"
    }
}
```

자세한 내용은 [평가 [\*] 별칭](../how-to/author-policies-for-arrays.md#evaluating-the--alias)합니다.

## <a name="initiatives"></a>이니셔티브

그룹을 단일 항목으로 작업할 수 있기 때문에 이니셔티브를 사용하면 여러 관련 정책 정의를 그룹화할 수 있어 할당 및 관리를 간소화합니다. 예를 들어 관련 태그 지정 정책 정의를 단일 이니셔티브로 그룹화할 수 있습니다. 각 정책을 개별적으로 할당하는 대신 이니셔티브를 적용합니다.

다음 예제에서는 두 태그 `costCenter`과 `productName`를 처리하기 위한 이니셔티브를 만드는 방법을 보여 줍니다. 기본 태그 값을 적용하려면 두 가지 기본 제공 정책을 사용합니다.

```json
{
    "properties": {
        "displayName": "Billing Tags Policy",
        "policyType": "Custom",
        "description": "Specify cost Center tag and product name tag",
        "parameters": {
            "costCenterValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for Cost Center tag"
                }
            },
            "productNameValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for product Name tag"
                }
            }
        },
        "policyDefinitions": [{
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            }
        ]
    },
    "id": "/subscriptions/<subscription-id>/providers/Microsoft.Authorization/policySetDefinitions/billingTagsPolicy",
    "type": "Microsoft.Authorization/policySetDefinitions",
    "name": "billingTagsPolicy"
}
```

## <a name="next-steps"></a>다음 단계

- 예제를 검토 [Azure Policy 샘플](../samples/index.md)합니다.
- [정책 효과 이해](effects.md)를 검토합니다.
- 이해 하는 방법 [프로그래밍 방식으로 정책 만들기](../how-to/programmatically-create.md)합니다.
- 에 대해 알아봅니다 하는 방법 [규정 준수 데이터를 가져올](../how-to/getting-compliance-data.md)합니다.
- 설명 하는 방법 [비준수 리소스를 수정](../how-to/remediate-resources.md)합니다.
- [Azure 관리 그룹으로 리소스 구성](../../management-groups/overview.md)을 포함하는 관리 그룹을 검토합니다.