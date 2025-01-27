---
title: Azure Active Directory 조건부 액세스에서 서비스 종속성은 무엇 인가요? | Microsoft Docs
description: Azure Active Directory 조건부 액세스에서 정책을 트리거할 수 조건을 사용 하는 방법을 알아봅니다.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 03/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: b9aca2e4ea5e107358ff72e83562057830ece2cc
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509359"
---
# <a name="what-are-service-dependencies-in-azure-active-directory-conditional-access"></a>Azure Active Directory 조건부 액세스에서 서비스 종속성은 무엇 인가요? 

조건부 액세스 정책을 사용 하 여 웹 사이트 및 서비스에 액세스 요구 사항을 지정할 수 있습니다. 예를 들어, multi-factor authentication (MFA)을 필요한 액세스 요구를 포함할 수 있습니다 또는 [관리 되는 장치](require-managed-devices.md)합니다. 

사이트 또는 서비스에 직접 액세스 하면 관련 정책의 영향 평가 하기 위해 일반적으로 쉽습니다. 예를 들어, 구성 된 SharePoint Online에 대 한 MFA를 요구 하는 정책을 경우 SharePoint 웹 포털에 각 로그인에 MFA 적용 됩니다. 그러나이 아닌 경우 항상 간단 되므로 다른 클라우드 앱에 대 한 종속성을 사용 하 여 클라우드 앱 정책의 영향을 평가 하려면 예를 들어, Microsoft Teams SharePoint Online 리소스에 대 한 액세스를 제공할 수 있습니다. 현재이 시나리오에서 Microsoft Teams 액세스 하면은 또한 SharePoint MFA 정책이 적용 합니다.   

## <a name="policy-enforcement"></a>정책 적용 

구성 서비스 종속성에 있는 경우 초기 바인딩된 또는 런타임에 바인딩된 적용을 사용 하 여 정책을 적용할 수 있습니다. 

- **초기 바인딩된 정책 적용** 의미 사용자 호출한 응용 프로그램에 액세스 하기 전에 종속 서비스가 정책을 충족 해야 합니다. 예를 들어, 사용자는 MS 팀에 로그인 하기 전에 SharePoint 정책을 충족 해야 합니다. 
- **런타임에 바인딩된 정책 적용** 호출한 응용 프로그램에 사용자가 로그인 한 후 발생 합니다. 응용 프로그램 요청을 다운스트림 서비스에 대 한 토큰을 호출 하는 경우 적용으로 지연 됩니다. 예제에는 플래너 및 SharePoint 액세스 Office.com에 액세스 하는 MS 팀 포함 됩니다. 

아래 다이어그램에서는 MS 팀 서비스 종속성을 보여 줍니다. 실선 화살표 Planner 나타냅니다 바인딩된 적용에 대 한 초기 바인딩 적용 파선된 화살표를 나타냅니다. 

![MS 팀 서비스 종속성](./media/service-dependencies/01.png)

모범 사례로, 관련된 앱 및 가능 하면 서비스에서 일반적인 정책을 설정 해야 합니다. 일관 된 보안 상태에 있는 최상의 사용자 환경을 제공 합니다. 예를 들어, 일반적인 정책 설정에서 Exchange Online, SharePoint Online, Microsoft Teams 및 Skype 비즈니스용 크게 다운스트림 서비스에 적용 되 고 다른 정책에서 발생할 수 있는 예기치 않은 프롬프트가 줄어듭니다. 

다음 표에서 클라이언트 앱을 충족 해야 하는 추가 서비스 종속성을 나열  

| 클라이언트 앱         | 다운스트림 서비스                          | 적용 |
| :--                 | :--                                         | ---         | 
| Azure Data Lake     | Microsoft Azure 관리 (포털 및 API) | 초기 바인딩 |
| Microsoft 클래스 룸 | Exchange                                    | 초기 바인딩 |
|                     | SharePoint                                  | 초기 바인딩  |
| Microsoft 팀     | Exchange                                    | 초기 바인딩 |
|                     | MS Planner                                  | 런타임에 바인딩  |
|                     | SharePoint                                  | 초기 바인딩 |
|                     | 비즈니스 온라인용 Skype                   | 초기 바인딩 |
| Office 포털       | Exchange                                    | 런타임에 바인딩  |
|                     | SharePoint                                  | 런타임에 바인딩  |
| Outlook의 그룹      | Exchange                                    | 초기 바인딩 |
|                     | SharePoint                                  | 초기 바인딩 |
| PowerApps           | Microsoft Azure 관리 (포털 및 API) | 초기 바인딩 |
|                     | Windows Azure Active Directory              | 초기 바인딩 |
| 프로젝트             | Dynamics CRM                                | 초기 바인딩 |
| 비즈니스용 Skype  | Exchange                                    | 초기 바인딩 |
| Visual Studio       | Microsoft Azure 관리 (포털 및 API) | 초기 바인딩 |

## <a name="next-steps"></a>다음 단계

참조를 환경에서 조건부 액세스를 구현 하는 방법을 알아보려면 [Azure Active Directory에서 조건부 액세스 배포를 계획](plan-conditional-access.md)합니다.
