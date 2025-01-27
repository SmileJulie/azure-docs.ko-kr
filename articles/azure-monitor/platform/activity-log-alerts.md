---
title: Azure Monitor의 활동 로그 경고
description: 활동 로그에서 특정 이벤트가 발생하면 SMS, 웹후크, SMS 및 메일 등을 통해 알림을 받습니다.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 61b5b96636ea12b5c63da657e006bd3121c34756
ms.sourcegitcommit: 470041c681719df2d4ee9b81c9be6104befffcea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67852605"
---
# <a name="alerts-on-activity-log"></a>활동 로그에 대한 경고 

## <a name="overview"></a>개요
활동 로그 경고는 경고에 지정 된 조건과 일치 하는 새 [활동 로그 이벤트가](activity-log-schema.md) 발생할 때 활성화 되는 경고입니다. [Azure 활동 로그](activity-logs-overview.md)에 기록 된 이벤트의 순서와 볼륨에 따라 경고 규칙이 실행 됩니다. 활동 로그 경고 규칙은 Azure 리소스 이므로 Azure Resource Manager 템플릿을 사용 하 여 만들 수 있습니다. 또한 Azure Portal에서 생성, 업데이트 또는 삭제할 수 있습니다. 이 문서에서는 활동 로그 경고에 대한 개념을 소개합니다. 활동 로그 경고 규칙을 만들거나 사용 하는 방법에 대 한 자세한 내용은 [활동 로그 경고 만들기 및 관리](alerts-activity-log.md)를 참조 하세요.

> [!NOTE]
> 활동 로그의 경고 범주에 있는 이벤트에 대 한 경고를 만들 수 **없습니다** .

일반적으로 다음과 같은 경우에 알림을 수신하도록 활동 로그 경고를 만들 수 있습니다.

* Azure 구독의 리소스에서 특정 작업이 발생하는 경우. 특정 리소스 그룹 또는 리소스로 범위를 지정하기도 합니다. 예를 들어 myProductionResourceGroup에서 가상 머신이 삭제되는 경우 알림을 받도록 할 수 있습니다. 또는 새 규칙이 사용자 구독의 사용자에 할당되는 경우 알림을 받도록 할 수도 있습니다.
* 서비스 상태 이벤트가 발생하는 경우. 서비스 상태 이벤트가 구독에서 리소스에 적용되는 인시던트 및 유지 관리 이벤트의 알림을 포함합니다.

활동 로그에서 경고 규칙을 만들 수 있는 조건을 이해 하는 간단한 비유는 [Azure Portal 활동 로그](activity-log-view.md#azure-portal)를 통해 이벤트를 탐색 하거나 필터링 하는 것입니다. Azure Monitor 활동 로그에서 필요한 이벤트를 필터링 하거나 찾은 다음 **활동 로그 경고 추가** 단추를 사용 하 여 경고를 만들 수 있습니다.

두 경우 모두 활동 로그 경고는 경고가 생성되는 구독의 이벤트만 모니터링합니다.

활동 로그 이벤트에 대한 JSON 개체의 모든 최상위 속성에 따라 활동 로그 경고를 구성할 수 있습니다. 자세한 내용은 [Azure 활동 로그 개요](./activity-logs-overview.md#categories-in-the-activity-log)를 참조하세요. 서비스 상태 이벤트에 대한 자세한 내용은 [서비스 알림에서 활동 로그 경고 수신](./alerts-activity-log-service-notifications.md)을 참조하세요. 

활동 로그 경고에는 다음과 같은 몇 가지 공통 옵션이 있습니다.

- **범주**: 관리, 서비스 상태, 자동 크기 조정, 보안, 정책 및 권장 사항 중 하나입니다. 
- **범위**: 활동 로그에서 경고가 정의되는 개별 리소스 또는 리소스 집합입니다. 활동 로그 경고 범위는 다양한 수준에서 정의할 수 있습니다.
    - 리소스 수준: 예를 들어 특정 가상 머신의 경우
    - 리소스 그룹 수준: 예를 들어 특정 리소스 그룹의 모든 가상 머신
    - 구독 수준: 구독의 모든 가상 머신 (또는) 구독의 모든 리소스
- **리소스 그룹**: 기본적으로 경고 규칙은 범위에 정의된 대상과 동일한 리소스 그룹에 저장됩니다. 경고 규칙을 저장할 리소스 그룹도 정의할 수 있습니다.
- **리소스 종류**: Resource Manager가 경고의 대상 네임스페이스를 정의합니다.
- **작업 이름**: 역할 기반 Access Control에 사용 되는 [Azure Resource Manager 작업](../../role-based-access-control/resource-provider-operations.md) 이름입니다. Azure Resource Manager에 등록 되지 않은 작업은 활동 로그 경고 규칙에서 사용할 수 없습니다.
- **수준**: 이벤트의 심각도 수준(자세한 정보, 정보, 경고, 오류 또는 중요)입니다.
- **상태**: 이벤트의 상태로, 일반적으로 시작됨, 실패 또는 성공입니다.
- **이벤트를 시작한 사람**: "호출자"라고도 합니다. 작업을 수행한 사용자의 이메일 주소 또는 Active Directory 식별자입니다.

> [!NOTE]
> 한 구독에서, 단일 리소스, 리소스 그룹의 모든 리소스 또는 전체 구독 수준 범위의 활동에 대해 최대 100개의 경고 규칙을 만들 수 있습니다.

활동 로그 경고가 활성화되면 작업 그룹을 사용하여 작업 또는 알림을 생성합니다. 작업 그룹은 이메일 주소, 웹후크 URL 또는 SMS 전화 번호와 같은 알림 수신자의 재사용 가능한 집합입니다. 수신자를 여러 경고에서 참조하여 알림 채널을 집중화하고 그룹화할 수 있습니다. 활동 로그 경고를 정의하는 경우 두 가지 옵션이 있습니다. 다음을 할 수 있습니다.

* 활동 로그 경고에서 기존 작업 그룹을 사용합니다.
* 새 작업 그룹을 만듭니다.

작업 그룹에 대해 자세히 알아보려면 [Azure Portal에서 작업 그룹 만들기 및 관리](action-groups.md)를 참조하세요.


## <a name="next-steps"></a>다음 단계

- [경고 개요](alerts-overview.md)를 확인하세요.
- [활동 로그 경고 만들기 및 수정](alerts-activity-log.md)에 관해 알아보세요.
- [활동 로그 경고 웹후크 스키마](activity-log-alerts-webhook.md)를 검토하세요.
- [서비스 상태 알림](service-notifications.md)에 대해 자세히 알아보세요.