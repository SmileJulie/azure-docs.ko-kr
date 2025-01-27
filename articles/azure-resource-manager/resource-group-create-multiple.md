---
title: Azure 리소스의 여러 인스턴스 배포 | Microsoft Docs
description: Azure 리소스 관리자 템플릿에서 복사 작업 및 배열을 사용하여 여러 번 반복하는 방법을 설명합니다.
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: tomfitz
ms.openlocfilehash: 22317372a7d954286ebcb0b59aea293c746b2a58
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508179"
---
# <a name="resource-property-or-variable-iteration-in-azure-resource-manager-templates"></a>리소스, 속성 또는 Azure Resource Manager 템플릿의 변수 반복

이 문서에서는 Azure Resource Manager 템플릿에서 리소스, 변수 또는 속성의 둘 이상의 인스턴스를 만드는 방법을 보여 줍니다. 여러 인스턴스를 만들려면 추가 `copy` 템플릿에 개체입니다.

리소스를 사용 하는 경우 복사 개체의 형식은:

```json
"copy": {
    "name": "<name-of-loop>",
    "count": <number-of-iterations>,
    "mode": "serial" <or> "parallel",
    "batchSize": <number-to-deploy-serially>
}
```

변수 또는 속성을 사용 하는 경우 복사 개체의 형식은:

```json
"copy": [
  {
      "name": "<name-of-loop>",
      "count": <number-of-iterations>,
      "input": <values-for-the-property-or-variable>
  }
]
```

모두 사용 하 여가이 문서에 자세히 설명 되어 있습니다. 자습서의 경우 [자습서: Resource Manager 템플릿을 사용하여 여러 리소스 인스턴스 만들기](./resource-manager-tutorial-create-multiple-instances.md)를 참조하세요.

리소스 배포 여부를 지정해야 하는 경우, [조건 요소](resource-group-authoring-templates.md#condition)를 참조하세요.

## <a name="copy-limits"></a>복사 제한

반복 횟수를 지정 하려면 count 속성에 대 한 값을 제공 합니다. 개수는 800을 초과할 수 없습니다.

수는 음수일 수 없습니다. REST API 버전을 사용 하 여 템플릿을 배포 하는 경우 **2019-05-10** 하거나 나중에 개수를 0으로 설정할 수 있습니다. REST API의 이전 버전에는 개수에 0을 지원 하지 않습니다. 현재, Azure CLI 또는 PowerShell 지원 하지 않습니다 개수에 0을 해당 지원은 향후 릴리스에 추가 될 예정입니다.

신중 하 게 사용 하 여 수 [모드 배포 완료](deployment-modes.md) 복사 합니다. 리소스 그룹에 전체 모드를 사용 하 여 다시 배포할 경우에 복사 루프를 해결 한 후 템플릿에 지정 되지 않은 모든 리소스가 삭제 됩니다.

개수에 대 한 제한 리소스, 변수 또는 속성을 사용 하 여 사용 여부를 나타내는 동일 합니다.

## <a name="resource-iteration"></a>리소스 반복

배포 중 리소스의 인스턴스를 하나 이상 만들지 결정해야 할 경우에는 `copy` 요소를 리소스 종류에 추가합니다. 복사 요소에서이 루프에 대 한 이름과 반복의 수를 지정 합니다.

다음 형식으로 리소스를 여러 번 만듭니다.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {},
      "copy": {
        "name": "storagecopy",
        "count": 3
      }
    }
  ],
  "outputs": {}
}
```

각 리소스의 이름에는 `copyIndex()` 함수가 포함되어 있으며 이 함수는 루프에서 현재 반복을 반환합니다. `copyIndex()`는 0부터 시작합니다. 따라서 예제는 다음과 같습니다.

```json
"name": "[concat('storage', copyIndex())]",
```

다음과 같은 이름을 만듭니다.

* storage0
* storage1
* storage2

인덱스 값을 오프셋하려면 copyIndex() 함수에 값을 전달하면 됩니다. 반복 횟수는 복사 요소에서 지정 되지만 copyIndex의 값이 지정된 된 값 만큼 오프셋 됩니다. 따라서 예제는 다음과 같습니다.

```json
"name": "[concat('storage', copyIndex(1))]",
```

다음과 같은 이름을 만듭니다.

* storage1
* storage2
* storage3

복사 작업은 배열의 각 요소를 반복할 수 있으므로 배열을 사용할 때 유용합니다. 배열의 `length` 함수를 사용하여 반복 횟수를 지정하고, `copyIndex`를 사용하여 배열의 현재 인덱스를 검색합니다. 따라서 예제는 다음과 같습니다.

```json
"parameters": { 
  "org": { 
    "type": "array", 
    "defaultValue": [ 
      "contoso", 
      "fabrikam", 
      "coho" 
    ] 
  }
}, 
"resources": [ 
  { 
    "name": "[concat('storage', parameters('org')[copyIndex()])]", 
    "copy": { 
      "name": "storagecopy", 
      "count": "[length(parameters('org'))]" 
    }, 
    ...
  } 
]
```

다음과 같은 이름을 만듭니다.

* storagecontoso
* storagefabrikam
* storagecoho

기본적으로 Resource Manager는 병렬로 리소스를 만듭니다. 생성되는 순서는 정해져 있지 않습니다. 그러나 그 결과로 리소스가 배포되도록 지정하려고 합니다. 예를 들어 프로덕션 환경을 업데이트할 때 특정 수를 한 번에 업데이트하도록 업데이트를 늦추려고 할 수 있습니다.

리소스의 여러 인스턴스를 직렬로 배포하려면 `mode`를 **직렬**로 설정하고 `batchSize`를 한 번에 배포할 인스턴스 수로 설정합니다. Resource Manager는 직렬 모드에서 루프에 이전 인스턴스의 종속성을 만듭니다. 따라서 이전 일괄 처리가 완료될 때까지 하나의 일괄 처리를 시작하지 않습니다.

예를 들어 저장소 계정을 한 번에 두 개씩 직렬로 배포하려면 다음을 사용합니다.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {},
      "copy": {
        "name": "storagecopy",
        "count": 4,
        "mode": "serial",
        "batchSize": 2
      }
    }
  ],
  "outputs": {}
}
```

모드 속성은 기본 값인 **병렬**을 수용합니다.

중첩 된 템플릿을 사용 하 여 복사를 사용 하는 방법에 대 한 내용은 [복사본을 사용 하 여](resource-group-linked-templates.md#using-copy)입니다.

## <a name="property-iteration"></a>속성 반복

리소스의 속성에 대해 여러 값을 만들려면 속성 요소에서 `copy` 배열을 추가합니다. 이 배열에는 개체가 포함되어 있으며 각 개체에는 다음 속성이 포함되어 있습니다.

* name - 여러 값을 만들 속성의 이름
* count - 만들 값 수
* input - 속성에 할당할 값이 포함된 개체  

다음 예제는 가상 머신에서 `copy`를 dataDisks 속성에 적용하는 방법을 보여 줍니다.

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "copy": [{
        "name": "dataDisks",
        "count": 3,
        "input": {
          "lun": "[copyIndex('dataDisks')]",
          "createOption": "Empty",
          "diskSizeGB": "1023"
        }
      }],
      ...
```

속성 반복 내에서 `copyIndex`를 사용하는 경우 반복의 이름을 제공해야 합니다. 리소스 반복과 함께 사용할 경우 이름을 제공할 필요가 없습니다.

Resource Manager는 배포 중 `copy` 배열을 확장합니다. 배열 이름은 속성의 이름이 됩니다. 입력 값은 개체 속성이 됩니다. 배포된 템플릿은 다음과 같습니다.

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "dataDisks": [
        {
          "lun": 0,
          "createOption": "Empty",
          "diskSizeGB": "1023"
        },
        {
          "lun": 1,
          "createOption": "Empty",
          "diskSizeGB": "1023"
        },
        {
          "lun": 2,
          "createOption": "Empty",
          "diskSizeGB": "1023"
        }
      ],
      ...
```

copy 요소는 배열이므로 리소스에 대해 1 초과 속성을 지정할 수 있습니다. 만들려는 각 속성에 대해 개체를 추가합니다.

```json
{
  "name": "string",
  "type": "Microsoft.Network/loadBalancers",
  "apiVersion": "2017-10-01",
  "properties": {
    "copy": [
      {
        "name": "loadBalancingRules",
        "count": "[length(parameters('loadBalancingRules'))]",
        "input": {
          ...
        }
      },
      {
        "name": "probes",
        "count": "[length(parameters('loadBalancingRules'))]",
        "input": {
          ...
        }
      }
    ]
  }
}
```

리소스 및 속성 반복을 함께 사용할 수 있습니다. 이름별로 속성 반복을 참조하세요.

```json
{
  "type": "Microsoft.Network/virtualNetworks",
  "name": "[concat(parameters('vnetname'), copyIndex())]",
  "apiVersion": "2018-04-01",
  "copy":{
    "count": 2,
    "name": "vnetloop"
  },
  "location": "[resourceGroup().location]",
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "[parameters('addressPrefix')]"
      ]
    },
    "copy": [
      {
        "name": "subnets",
        "count": 2,
        "input": {
          "name": "[concat('subnet-', copyIndex('subnets'))]",
          "properties": {
            "addressPrefix": "[variables('subnetAddressPrefix')[copyIndex('subnets')]]"
          }
        }
      }
    ]
  }
}
```

## <a name="variable-iteration"></a>변수 반복

변수의 여러 인스턴스를 만들려면 변수 섹션에서 `copy` 속성을 사용합니다. `input` 속성의 값에서 생성된 요소 배열을 만듭니다. 변수 내부 또는 변수 섹션의 최상위 수준에서 `copy` 속성을 사용할 수 있습니다. 변수 반복 내부에 `copyIndex`를 사용하는 경우 반복의 이름을 제공해야 합니다.

문자열 값의 배열을 만드는 간단한 예제를 보려면 [copy 배열 템플릿을](https://github.com/bmoore-msft/AzureRM-Samples/blob/master/copy-array/azuredeploy.json)합니다.

다음 예제에서는 동적으로 생성된 요소로 배열 변수를 만드는 여러 가지 방법을 보여 줍니다. 변수 내부에 copy를 사용하여 개체 및 문자열의 배열을 만드는 방법을 보여 줍니다. 또한 최상위 수준에서 copy를 사용하여 개체, 문자열 및 정수의 배열을 만드는 방법을 보여 줍니다.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {
    "disk-array-on-object": {
      "copy": [
        {
          "name": "disks",
          "count": 5,
          "input": {
            "name": "[concat('myDataDisk', copyIndex('disks', 1))]",
            "diskSizeGB": "1",
            "diskIndex": "[copyIndex('disks')]"
          }
        },
        {
          "name": "diskNames",
          "count": 5,
          "input": "[concat('myDataDisk', copyIndex('diskNames', 1))]"
        }
      ]
    },
    "copy": [
      {
        "name": "top-level-object-array",
        "count": 5,
        "input": {
          "name": "[concat('myDataDisk', copyIndex('top-level-object-array', 1))]",
          "diskSizeGB": "1",
          "diskIndex": "[copyIndex('top-level-object-array')]"
        }
      },
      {
        "name": "top-level-string-array",
        "count": 5,
        "input": "[concat('myDataDisk', copyIndex('top-level-string-array', 1))]"
      },
      {
        "name": "top-level-integer-array",
        "count": 5,
        "input": "[copyIndex('top-level-integer-array')]"
      }
    ]
  },
  "resources": [],
  "outputs": {
    "exampleObject": {
      "value": "[variables('disk-array-on-object')]",
      "type": "object"
    },
    "exampleArrayOnObject": {
      "value": "[variables('disk-array-on-object').disks]",
      "type" : "array"
    },
    "exampleObjectArray": {
      "value": "[variables('top-level-object-array')]",
      "type" : "array"
    },
    "exampleStringArray": {
      "value": "[variables('top-level-string-array')]",
      "type" : "array"
    },
    "exampleIntegerArray": {
      "value": "[variables('top-level-integer-array')]",
      "type" : "array"
    }
  }
}
```

생성 되는 변수의 형식이 입력된 개체에 따라 달라 집니다. 예를 들어, 명명 된 변수의 **최상위-level-개체-배열** 앞의 예제를 반환 합니다.

```json
[
  {
    "name": "myDataDisk1",
    "diskSizeGB": "1",
    "diskIndex": 0
  },
  {
    "name": "myDataDisk2",
    "diskSizeGB": "1",
    "diskIndex": 1
  },
  {
    "name": "myDataDisk3",
    "diskSizeGB": "1",
    "diskIndex": 2
  },
  {
    "name": "myDataDisk4",
    "diskSizeGB": "1",
    "diskIndex": 3
  },
  {
    "name": "myDataDisk5",
    "diskSizeGB": "1",
    "diskIndex": 4
  }
]
```

및 명명 된 변수의 **최상위-level-문자열-배열** 반환 합니다.

```json
[
  "myDataDisk1",
  "myDataDisk2",
  "myDataDisk3",
  "myDataDisk4",
  "myDataDisk5"
]
```

## <a name="depend-on-resources-in-a-loop"></a>루프의 리소스에 따라 달라짐

`dependsOn` 요소를 사용하여 어떤 리소스를 다른 리소스 다음에 배포하도록 지정합니다. 루프의 리소스 컬렉션에 따라 달라지는 리소스를 배포하려면 dependsOn 요소에 복사 루프의 이름을 제공합니다. 다음 예제에서는 Virtual Machine을 배포하기 전에 저장소 계정 3개를 배포하는 방법을 보여줍니다. 전체 Virtual Machine 정의는 표시되지 않습니다. 참고로 copy 요소의 name은 `storagecopy`로 설정되고 Virtual Machines에 대한 dependsOn 요소도 `storagecopy`로 설정되었습니다.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {},
      "copy": {
        "name": "storagecopy",
        "count": 3
      }
    },
    {
      "apiVersion": "2015-06-15", 
      "type": "Microsoft.Compute/virtualMachines", 
      "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
      "dependsOn": ["storagecopy"],
      ...
    }
  ],
  "outputs": {}
}
```

<a id="looping-on-a-nested-resource" />

## <a name="iteration-for-a-child-resource"></a>자식 리소스에 대한 반복
자식 리소스에 대해 복사 루프를 사용할 수 없습니다. 일반적으로 다른 리소스 내에 중첩된 것으로 정의된 여러 인스턴스를 만들려면 대신 해당 리소스를 최상위 리소스로 만들어야 합니다. 형식 및 이름 속성을 통해 부모 리소스와의 관계를 정의합니다.

예를 들어, 데이터 세트를 데이터 팩터리 내에 자식 리소스로 정의한다고 가정합니다.

```json
"resources": [
{
  "type": "Microsoft.DataFactory/datafactories",
  "name": "exampleDataFactory",
  ...
  "resources": [
    {
      "type": "datasets",
      "name": "exampleDataSet",
      "dependsOn": [
        "exampleDataFactory"
      ],
      ...
    }
  ]
```

둘 이상의 데이터 세트를 만들려면 데이터 팩터리의 외부로 이동합니다. 데이터 세트는 데이터 팩터리와 같은 수준에 있어야 하지만 여전히 데이터 팩터리의 자식 리소스입니다. 형식 및 이름 속성을 통해 데이터 집합과 데이터 팩터리 간의 관계를 유지합니다. 템플릿의 해당 위치에서 형식을 더 이상 유추할 수 없으므로 `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}` 형식으로 정규화된 형식을 제공해야 합니다.

데이터 팩터리 인스턴스로 부모/자식 관계를 설정하려면 부모 리소스 이름을 포함하는 데이터 집합에 대해 이름을 제공합니다. 사용할 형식: `{parent-resource-name}/{child-resource-name}`.  

다음 예제에서는 구현을 보여줍니다.

```json
"resources": [
{
  "type": "Microsoft.DataFactory/datafactories",
  "name": "exampleDataFactory",
  ...
},
{
  "type": "Microsoft.DataFactory/datafactories/datasets",
  "name": "[concat('exampleDataFactory', '/', 'exampleDataSet', copyIndex())]",
  "dependsOn": [
    "exampleDataFactory"
  ],
  "copy": {
    "name": "datasetcopy",
    "count": "3"
  },
  ...
}]
```

## <a name="example-templates"></a>예제 템플릿

다음 예제에서는 여러 리소스 또는 속성 인스턴스를 만들기 위한 일반적인 시나리오를 보여 줍니다.

|Template  |설명  |
|---------|---------|
|[저장소 복사](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystorage.json) |이름의 인덱스 번호를 사용하여 여러 스토리지 계정을 배포합니다. |
|[저장소 직렬 복사](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/serialcopystorage.json) |여러 스토리지 계정을 한 번에 하나씩 배포합니다. 이름에는 인덱스 번호가 포함됩니다. |
|[배열을 사용하여 저장소 복사](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystoragewitharray.json) |여러 스토리지 계정을 배포합니다. 이름에는 배열의 값이 포함됩니다. |
|[가변적인 수의 데이터 디스크를 사용한 VM 배포](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-windows-copy-datadisks) |가상 머신을 사용하여 여러 데이터 디스크를 배포합니다. |
|[변수 복사](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) |변수를 반복하는 다양한 방법을 보여 줍니다. |
|[다중 보안 규칙](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) |네트워크 보안 그룹에 여러 보안 규칙을 배포합니다. 매개 변수에서 보안 규칙을 구성합니다. 매개 변수는 [여러 NSG 매개 변수 파일](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json)을 참조합니다. |

## <a name="next-steps"></a>다음 단계

* 자습서를 살펴보려면 [자습서: Resource Manager 템플릿을 사용하여 여러 리소스 인스턴스 만들기](./resource-manager-tutorial-create-multiple-instances.md)를 참조하세요.

* 템플릿 섹션에 대한 자세한 내용은 [Azure Resource Manager 템플릿 작성](resource-group-authoring-templates.md)을 참조하세요.
* 템플릿 배포 방법에 대한 자세한 내용은 [Azure Resource Manager 템플릿을 사용하여 애플리케이션 배포](resource-group-template-deploy.md)를 참조하세요.

