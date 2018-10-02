---
title: Azure Active Directory의 단일 및 다중 테넌트 앱
description: Azure AD에서 단일 테넌트와 다중 테넌트 앱의 기능 및 차이점에 대해 알아봅니다.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: justhu
ms.custom: aaddev
ms.openlocfilehash: 8986f18b9f1f2953ad0612679d31996628870cb1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46988226"
---
# <a name="tenancy-in-azure-active-directory"></a>Azure Active Directory의 테넌시

Azure AD(Azure Active Directory)에서는 사용자 및 앱과 같은 개체를 *테넌트*라는 그룹으로 구성합니다. 테넌트를 사용하면 관리자는 조직 및 조직이 소유한 앱 내에서 해당 보안 및 운영 정책을 충족하도록 사용자의 정책을 설정할 수 있습니다. 

## <a name="who-can-sign-in-to-your-app"></a>앱에 로그인할 수 있는 사용자

앱 개발에 있어서 개발자는 [Azure Portal](http://portal.azure.com)에서 앱을 등록하는 동안 해당 앱을 단일 테넌트 또는 다중 테넌트로 구성할 수 있습니다.
* 단일 테넌트 앱은 홈 테넌트라고도 하는 등록된 테넌트에서만 사용할 수 있습니다.
* 다중 테넌트 앱은 해당 홈 테넌트와 기타 테넌트의 사용자에게 지원됩니다.

Azure Portal에서 대상 그룹을 다음과 같이 설정하여 앱을 단일 테넌트 또는 다중 테넌트로 구성할 수 있습니다.

| 대상 | 단일/다중 테넌트 | 로그인할 수 있는 사용자 | 
|----------|--------| ---------|
| 이 디렉터리의 계정에만 해당 | 단일 테넌트 | 디렉터리의 모든 사용자 및 게스트 계정은 사용자의 응용 프로그램 또는 API를 사용할 수 있습니다.<br>*대상 그룹이 조직 내부자인 경우 이 옵션을 사용합니다.* |
| Azure AD 디렉터리의 계정 | 다중 테넌트 | Microsoft에서 회사 또는 학교 계정을 사용하는 모든 사용자 및 게스트는 사용자의 응용 프로그램 또는 API를 사용할 수 있습니다. 여기에는 Office 365를 사용하는 학교 및 회사가 포함됩니다.<br>*대상 그룹이 비즈니스 또는 교육용 고객인 경우 이 옵션을 사용합니다.* |
| Azure AD 디렉터리의 계정 및 개인 Microsoft 계정(예: Skype, Xbox, Outlook.com) | 다중 테넌트 | 회사나 학교 계정 또는 개인 Microsoft 계정을 사용하는 모든 사용자는 사용자의 응용 프로그램 또는 API를 사용할 수 있습니다. Skype 및 Xbox와 같은 서비스에 로그인하는 데 사용되는 개인 계정뿐만 아니라 Office 365를 사용하는 학교 및 회사가 포함됩니다.<br>*다양한 Microsoft 계정을 대상으로 지정하려면 이 옵션을 사용합니다.* | 

## <a name="best-practices-for-multi-tenant-apps"></a>다중 테넌트 앱에 대한 모범 사례

IT 관리자가 해당 테넌트에서 설정할 수 있는 다양한 정책 수로 인해 우수한 다중 테넌트 앱을 빌드하는 작업은 어려울 수 있습니다. 다중 테넌트 앱을 빌드하려는 경우 다음과 같은 모범 사례를 따르세요.

* 테넌트에서 구성된 [조건부 액세스 정책](conditional-access-dev-guide.md)이 포함된 앱을 테스트합니다.
* 앱이 실제로 필요한 사용 권한만 요청하도록 하기 위해 최소 사용자 액세스 원칙을 따릅니다. 사용자가 일부 조직에서 앱을 사용하지 못할 수 있으므로 관리자 동의가 필요한 권한을 요청하지 않는 것이 좋습니다. 
* 앱의 일부로 노출한 사용 권한에 대해 적절한 이름 및 설명을 입력합니다. 이렇게 하면 사용자 및 관리자가 앱의 API를 사용하려고 할 때 동의하는 내용을 알 수 있습니다. 자세한 내용은 [사용 권한 가이드](v1-permissions-and-consent.md)에서 모범 사례 섹션을 참조하세요.

## <a name="next-steps"></a>다음 단계

* [앱을 다중 테넌트로 변환하는 방법](howto-convert-app-to-be-multi-tenant.md)