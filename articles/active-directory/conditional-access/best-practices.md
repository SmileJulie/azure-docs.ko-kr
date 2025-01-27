---
title: Azure Active Directory의 조건부 액세스에 대 한 모범 사례 | Microsoft Docs
description: 조건부 액세스 정책을 구성할 때 알아야 할 사항 및 수행 하지 않아야 하는 사항에 대해 알아보세요.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 01/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 08be09f7bee3aa17ff9d1baae4271e994fa235cb
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68248876"
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a>Azure Active Directory의 조건부 액세스에 대 한 모범 사례

[Azure Active Directory (AZURE AD) 조건부 액세스](../active-directory-conditional-access-azure-portal.md)를 사용 하 여 권한 있는 사용자가 클라우드 앱에 액세스 하는 방법을 제어할 수 있습니다. 이 문서에서는 다음에 대한 정보를 제공합니다.

- 알아야 할 사항 
- 조건부 액세스 정책을 구성할 때는이 작업을 수행 하지 않아야 합니다. 

이 문서에서는 [Azure Active Directory의 조건부 액세스 란?](../active-directory-conditional-access-azure-portal.md) 에 설명 된 개념과 용어에 대해 잘 알고 있다고 가정 합니다.

## <a name="whats-required-to-make-a-policy-work"></a>정책을 작동하는 데 무엇이 필요한가요?

새 정책을 만들 경우 선택된 사용자, 그룹, 앱 또는 액세스 제어가 없습니다.

![클라우드 앱](./media/best-practices/02.png)

정책이 작동되려면 다음을 구성해야 합니다.

| 대상           | 방법                                  | 이유 |
| :--            | :--                                  | :-- |
| **클라우드 앱** |하나 이상의 앱을 선택합니다.  | 조건부 액세스 정책의 목표는 권한 있는 사용자가 클라우드 앱에 액세스 하는 방법을 제어할 수 있도록 하는 것입니다.|
| **사용자 및 그룹** | 선택한 클라우드 앱에 대한 액세스 권한을 부여받은 하나 이상의 사용자 또는 그룹을 선택합니다. | 할당 된 사용자 및 그룹이 없는 조건부 액세스 정책은 트리거되지 않습니다. |
| **액세스 제어** | 하나 이상의 액세스 제어를 선택합니다. | 조건이 충족될 경우 정책 프로세서는 수행할 작업을 알고 있어야 합니다. |

## <a name="what-you-should-know"></a>알아야 할 사항

### <a name="how-are-conditional-access-policies-applied"></a>조건부 액세스 정책은 어떻게 적용 되나요?

클라우드 앱에 액세스 하는 경우 조건부 액세스 정책이 둘 이상 적용 될 수 있습니다. 이 경우 적용되는 모든 정책이 충족되어야 합니다. 예를 들어 한 정책에서 MFA를 요구하고 두 번째 정책에서 규정 준수 디바이스를 요구하는 경우 MFA를 통과하고 규정 준수 디바이스를 사용해야 합니다. 

모든 정책은 다음 두 단계로 적용됩니다.

- **첫 번째** 단계에서는 모든 정책이 평가되고 충족되지 않은 모든 액세스 제어가 수집됩니다. 
- **두 번째** 단계에서는 충족되지 않은 요구 사항을 충족시키도록 요구하는 메시지가 표시됩니다. 정책 중 하나가 액세스를 차단 하는 경우 차단 되며 다른 정책 제어를 충족 하 라는 메시지가 표시 되지 않습니다. 정책에서 사용자를 차단 하는 경우 다음 순서로 다른 정책 컨트롤을 충족 하 라는 메시지가 표시 됩니다.

   ![주문](./media/best-practices/06.png)
    
   외부 MFA 공급자와 사용 약관은 다음에 제공 됩니다.

### <a name="how-are-assignments-evaluated"></a>할당은 어떻게 평가됩니까?

모든 할당은 논리적으로 **and**가 적용됩니다. 하나를 초과하여 할당을 구성한 경우 모든 할당이 충족되어야 정책을 트리거할 수 있습니다.  

조직의 네트워크 외부에서 수행되는 모든 연결에 적용되는 위치 조건을 구성해야 하는 경우:

- **모든 위치** 포함
- **모든 신뢰할 수 있는 IP** 제외

### <a name="what-to-do-if-you-are-locked-out-of-the-azure-ad-admin-portal"></a>Azure AD 관리 포털이 잠긴 경우 어떻게 합니까?

조건부 액세스 정책의 잘못 된 설정으로 인해 Azure AD 포털에서 잠긴 경우:

- 아직 차단되지 않은 조직 내 다른 관리자가 있는지 확인합니다. Azure Portal에 액세스할 수 있는 관리자는 로그인에 영향을 주는 정책을 비활성화할 수 있습니다. 
- 정책을 업데이트할 수 있는 조직의 관리자가 없는 경우 지원 요청을 제출해야 합니다. Microsoft 지원에서 액세스를 차단 하는 조건부 액세스 정책을 검토 하 고 업데이트할 수 있습니다.

### <a name="what-happens-if-you-have-policies-in-the-azure-classic-portal-and-azure-portal-configured"></a>Azure 클래식 포털과 Azure Portal에서 정책을 구성하면 어떻게 됩니까?  

두 정책은 모두 Azure Active Directory에서 적용되며 모든 요구 사항을 충족하는 경우에만 사용자가 액세스 권한을 획득할 수 있습니다.

### <a name="what-happens-if-you-have-policies-in-the-intune-silverlight-portal-and-the-azure-portal"></a>Intune Silverlight 포털과 Azure Portal에 정책이 있으면 어떻게 됩니까?

두 정책은 모두 Azure Active Directory에서 적용되며 모든 요구 사항을 충족하는 경우에만 사용자가 액세스 권한을 획득할 수 있습니다.

### <a name="what-happens-if-i-have-multiple-policies-for-the-same-user-configured"></a>동일한 사용자에 대해 여러 정책을 구성하면 어떻게 됩니까?  

Azure Active Directory는 모든 로그인에 대해 모든 정책을 평가하고, 사용자에게 액세스 권한을 부여하기 전에 모든 요구 사항이 충족되는지 확인합니다. 액세스 차단은 다른 모든 구성 설정보다 우선합니다. 

### <a name="does-conditional-access-work-with-exchange-activesync"></a>조건부 액세스는 Exchange ActiveSync에서 작동 하나요?

예, 몇 가지 [제한 사항](block-legacy-authentication.md)으로 조건부 액세스 정책에서 Exchange ActiveSync를 사용할 수 있습니다. 

### <a name="how-should-you-configure-conditional-access-with-office-365-apps"></a>Office 365 앱을 사용 하 여 조건부 액세스를 구성 하려면 어떻게 해야 하나요?

Office 365 앱이 상호 연결 되어 있기 때문에 정책을 만들 때 일반적으로 사용 되는 앱을 함께 할당 하는 것이 좋습니다.

일반적인 상호 연결 된 응용 프로그램에는 Microsoft Flow, Microsoft Planner, Microsoft 팀, Office 365 Exchange Online, Office 365 SharePoint Online 및 Office 365 Yammer가 포함 됩니다.

세션이 나 태스크의 시작 부분에서 액세스를 제어 하는 경우 multi-factor authentication과 같은 사용자 상호 작용이 필요한 정책에 중요 합니다. 그렇지 않으면 사용자가 앱 내에서 일부 작업을 완료할 수 없습니다. 예를 들어 관리 되지 않는 장치에서 다단계 인증을 사용 하 여 SharePoint에 액세스 하지만 전자 메일에 액세스 하지 못하는 경우 전자 메일에서 작업 하는 사용자가 SharePoint 파일을 메시지에 연결할 수 없습니다. 자세한 내용은 [Azure Active Directory 조건부 액세스의 서비스 종속성 이란?](service-dependencies.md)문서에서 찾을 수 있습니다.

## <a name="what-you-should-avoid-doing"></a>금지해야 할 기능

조건부 액세스 프레임 워크는 뛰어난 구성 유연성을 제공 합니다. 그러나 훌륭한 유연성은 잘못된 결과를 방지하기 위해 해제하기 전에 각 구성 정책을 신중하게 검토해야 한다는 것을 의미하기도 합니다. 이 컨텍스트에서 **모든 사용자 / 그룹 / 응용 프로그램**등 완전한 집합에 영향을 미치는 할당에 특별한 주의를 기울여야 합니다.

사용자 환경에서 다음과 같은 구성을 피해야 합니다.

**모든 사용자에 대한 모든 클라우드 앱:**

- **액세스 차단** - 이 구성은 사용자 전체 조직을 차단하므로 사용하지 않는 것이 좋습니다.
- **호환 디바이스가 필요** - 디바이스를 아직 등록하지 않은 사용자의 경우 이 정책은 Intune 포털에 대한 액세스를 비롯한 모든 액세스를 차단합니다. 등록된 디바이스가 없는 관리자의 경우 이 정책은 정책을 변경하기 위해 Azure Portal로 다시 돌아가지 않도록 차단합니다.
- **도메인 가입 필요** - 이 정책 차단 액세스에도 도메인에 가입된 디바이스가 아직 없는 경우에 조직의 모든 사용자에 대한 액세스를 차단할 가능성이 있습니다.
- **앱 보호 정책 필요** -이 정책 블록 액세스에는 Intune 정책이 없는 경우 조직의 모든 사용자에 대 한 액세스를 차단할 수도 있습니다. Intune 앱 보호 정책이 있는 클라이언트 응용 프로그램을 사용 하지 않는 관리자의 경우이 정책은 Intune 및 Azure와 같은 포털을 다시 가져올 수 없도록 차단 합니다.

**모든 사용자 경우 모든 클라우드 앱, 모든 디바이스 플랫폼은 다음과 같습니다.**

- **액세스 차단** - 이 구성은 사용자 전체 조직을 차단하므로 사용하지 않는 것이 좋습니다.

## <a name="how-should-you-deploy-a-new-policy"></a>새 정책을 배포하려면 어떻게 해야 합니까?

첫 단계로, [what if(가정) 도구](what-if-tool.md)를 사용하여 정책을 평가해야 합니다.

환경에 대한 새 정책이 준비되면 다음과 같은 단계로 배포합니다.

1. 일부 사용자 집합에 정책을 적용하고 예상 대로 작동하는지 확인합니다. 
1. 더 많은 사용자를 포함하도록 정책을 확장하는 경우가 있습니다. 정책에서 모든 관리자를 계속 제외하여 아직도 액세스 권한이 있는지 확인하고, 변경이 필요한 경우 정책을 업데이트할 수 있습니다.
1. 필요한 경우에만 모든 사용자에게 정책을 적용합니다. 

모범 사례로, 다음과 같은 사용자 계정을 만듭니다.

- 정책 관리 전용 
- 모든 정책에서 제외

## <a name="policy-migration"></a>정책 마이그레이션

Azure Portal에서 만들지 않은 정책을 마이그레이션하는 것을 고려해야 하는 이유는 다음과 같습니다.

- 이전에는 처리할 수 없었던 시나리오를 이제 처리할 수 있습니다.
- 통합하여 관리해야 하는 정책의 수를 줄일 수 있습니다.   
- 모든 조건부 액세스 정책을 하나의 중앙 위치에서 관리할 수 있습니다.
- Azure 클래식 포털의 사용이 중지됩니다.   

자세한 내용은 [Azure Portal에서 클래식 정책 마이그레이션](policy-migration.md)을 참조하세요.

## <a name="next-steps"></a>다음 단계

다음을 참조하세요.

- 조건부 액세스 정책을 구성 하는 방법에 [대 한 자세한 내용은 Azure Active Directory 조건부 액세스를 사용 하 여 특정 앱에 대해 MFA 필요](app-based-mfa.md)를 참조 하세요.
- 조건부 액세스 정책을 계획 하는 방법 [Azure Active Directory에서 조건부 액세스 배포를 계획 하는 방법](plan-conditional-access.md)을 참조 하세요.
