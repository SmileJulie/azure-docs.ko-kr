---
title: Java 및 VS Code를 사용하여 Azure Dev Spaces로 팀 개발
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: stepro
ms.author: stephpr
ms.date: 08/01/2018
ms.topic: tutorial
description: Azure에서 컨테이너 및 마이크로 서비스를 통한 신속한 Kubernetes 개발
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers, Helm, service mesh, service mesh routing, kubectl, k8s '
manager: gwallace
ms.openlocfilehash: 7b4785e1fbcc9f7946526920113b453c0a811c42
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67707182"
---
[!INCLUDE [](../../includes/devspaces-team-development-1.md)]

### <a name="make-a-code-change"></a>코드 변경
`mywebapi`에 대한 VS Code 창으로 이동하고, `src/main/java/com/ms/sample/mywebapi/Application.java`에서 `String index()` 메서드에 대한 코드를 편집합니다. 예를 들어 다음과 같습니다.

```java
@RequestMapping(value = "/", produces = "text/plain")
public String index() {
    return "Hello from mywebapi says something new";
}
```

[!INCLUDE [](../../includes/devspaces-team-development-2.md)]
