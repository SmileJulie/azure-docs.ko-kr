---
title: 포함 파일
description: 포함 파일
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 07/11/2019
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 099bca7483100da1a4ee2f8f10057c416ad145b0
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67841443"
---
메타데이터를 제공하는 Azure 리소스에 태그를 적용하여 분류로 논리적으로 구성합니다. 각 태그는 이름과 값 쌍으로 이루어져 있습니다. 예를 들어 프로덕션의 모든 리소스에 "환경" 이름과 "프로덕션" 값을 적용할 수 있습니다.

태그를 적용한 후에는 구독에서 태그 이름과 값으로 모든 리소스를 검색할 수 있습니다. 태그를 사용하면 서로 다른 리소스 그룹에서 관련 리소스를 검색할 수 있습니다. 이 방법은 청구 또는 관리에 대한 리소스를 구성해야 하는 경우에 유용할 수 있습니다.

분류는 셀프 서비스 메타데이터 태그 지정 전략 및 사용자의 부담을 줄이고 정확도를 높이는 자동 태그 지정 전략을 고려해야 합니다.

다음 제한 사항이 태그에 적용됩니다.

* 일부 리소스 유형은 태그를 지원하지 않습니다. 리소스 유형에 태그를 적용할 수 있는지 확인하려면 [Azure 리소스에 대한 태그 지원](../articles/azure-resource-manager/tag-support.md)을 참조하세요.
* 각 리소스 또는 리소스 그룹 태그 이름/값 쌍은 50 있을 수 있습니다. 현재 15 개의 태그, 저장소 계정만 지원 하지만 이후 릴리스에서 50 제한을 발생 합니다. 최대 허용된 개수 보다 더 많은 태그를 적용 해야 할 경우 태그 값에 대 한 JSON 문자열을 사용 합니다. JSON 문자열은 단일 태그 이름에 적용되는 여러 값을 포함할 수 있습니다. 리소스 그룹 50 태그 이름/값 쌍을 포함 하는 많은 리소스를 포함할 수 있습니다.
* 태그 이름은 512자로 제한되며 태그 값은 256자로 제한됩니다. 저장소 계정에서 태그 이름은 128자로 제한되며 태그 값은 256자로 제한됩니다.
* 일반화 된 Vm 태그를 지원 하지 않습니다.
* 태그는 해당 리소스 그룹의 리소스에 의해 상속되지 않은 리소스 그룹에 적용됩니다.
* Cloud Services와 같은 클래식 리소스에는 태그를 적용할 수 없습니다.
* 태그 이름에는 다음 문자를 포함할 수 없습니다: `<`, `>`, `%`, `&`, `\`, `?`, `/`
