---
title: Azure 엔터프라이즈 비용 보기 문제 해결
description: Azure Portal 내에서 조직의 비용 보기에 발생할 수 있는 문제를 해결하는 방법에 대해 알아봅니다.
author: bandersmsft
manager: amberb
ms.service: billing
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.custom: seodec18
ms.openlocfilehash: 83f7f424b265582a3830c02973cbbb9962ddfbfb
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491271"
---
# <a name="troubleshoot-enterprise-cost-views"></a>기업 비용 보기 문제 해결

기업 등록계약 내에 등록된 사용자에게 비용이 표시되지 않도록 하는 여러 설정이 있습니다.  이러한 설정은 등록 관리자가 관리합니다. 또는 Microsoft를 통해 직접 등록을 구입하지 않은 경우 파트너가 설정을 관리합니다.  이 문서를 통해 이러한 설정이 무엇이고 등록에 미치는 영향을 이해할 수 있습니다. 이러한 설정은 Azure RBAC(역할 기반 액세스 제어) 역할과는 별개입니다.

## <a name="enable-access-to-costs"></a>비용에 대 한 액세스를 사용 하도록 설정

비용 정보를 검색할 때 권한 없음 또는 *"등록에서 비용 보기를 사용할 수 없습니다."* 라는 메시지가 표시되나요?
![구독에 대한 현재 비용 필드에 “권한 없음”이 표시된 스크린샷](media/billing-enterprise-mgmt-groups/unauthorized.png)

다음 이유 중 하나 때문일 수 있습니다.

1. 기업 파트너를 통해 Azure를 구입했으며 파트너가 아직 가격 책정을 릴리스하지 않았습니다. [Enterprise Portal](https://ea.azure.com) 내의 가격 책정 설정을 업데이트하도록 파트너에게 요청하세요.
2. EA Direct 고객인 경우 다음과 같은 몇 가지 가능성이 있습니다.
    * 사용자가 계정 소유자이며 등록 관리자가 **AO 요금 보기** 설정을 사용하지 않도록 설정했습니다.  
    * 사용자가 부서 관리자이며 등록 관리자가 **DA 요금 보기** 설정을 사용하지 않도록 설정했습니다.
    * 등록 관리자에게 액세스 권한을 문의하세요. 등록 관리자는 [Enterprise Portal](https://ea.azure.com/manage/enrollment)에서 설정을 업데이트할 수 있습니다.

      ![요금 보기에 대한 Enterprise Portal 설정을 보여 주는 스크린샷](media/billing-enterprise-mgmt-groups/ea-portal-settings.png)

## <a name="asset-is-unavailable"></a>자산을 사용할 수 없음

오류 메시지가 표시 되 면 **이 자산을 사용할 수 없는** 에서 구독 또는 관리 그룹에 액세스 하려고 하면 다음 없는 올바른 역할이이 항목을 보려면.  

![“자산을 사용할 수 없음” 메시지를 보여 주는 스크린샷](media/billing-enterprise-mgmt-groups/asset-not-found.png)

Azure 구독 또는 관리 그룹 관리자에게 액세스 권한을 요청하세요. 자세한 내용은 [RBAC 및 Azure Portal을 사용하여 액세스 관리](../role-based-access-control/role-assignments-portal.md)를 참조하세요.

## <a name="next-steps"></a>다음 단계
- 질문이 있거나 도움이 필요한 경우 [지원 요청을 만드세요](https://go.microsoft.com/fwlink/?linkid=2083458).
