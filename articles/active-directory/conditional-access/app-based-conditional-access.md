---
title: Azure Active Directory의 조건부 액세스를 사용 하 여 클라우드 앱 액세스를 위해 승인 된 클라이언트 앱을 요구 하는 방법 | Microsoft Docs
description: Azure Active Directory의 조건부 액세스를 사용 하 여 클라우드 앱 액세스를 위해 승인 된 클라이언트 앱을 요구 하는 방법에 알아봅니다.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 06/13/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: spunukol
ms.collection: M365-identity-device-management
ms.openlocfilehash: ff4e81f5533622c8afbfb0d7c683c76b530aefec
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509497"
---
# <a name="how-to-require-approved-client-apps-for-cloud-app-access-with-conditional-access"></a>방법: 조건부 액세스를 사용 하 여 클라우드 앱 액세스를 위해 승인 된 클라이언트 앱 필요 

직원은 개인 및 회사 작업에 대해 모바일 디바이스를 사용합니다. 직원이 생산적이 될 수 있도록 하는 반면 데이터 손실을 방지하려고 합니다. Azure Active Directory (Azure AD) 조건부 액세스를 사용 하 여 회사 데이터를 보호할 수 있는 승인 된 클라이언트 앱에 클라우드 앱에 대 한 액세스를 제한할 수 있습니다.  

이 항목에서는 승인된 클라이언트 앱을 필요로 하는 조건부 액세스 정책을 구성하는 방법을 설명합니다.

## <a name="overview"></a>개요

사용 하 여 [Azure AD 조건부 액세스](overview.md)를 미세 조정할 수 있습니다 권한이 있는 사용자가 리소스를 액세스 하는 방법입니다. 예를 들어 클라우드 앱에 대한 액세스를 신뢰할 수 있는 디바이스로 제한할 수 있습니다.

[Intune 앱 보호 정책](https://docs.microsoft.com/intune/app-protection-policy)을 사용하여 회사의 데이터를 보호할 수 있습니다. Intune 앱 보호 정책은 디바이스 관리 솔루션에 디바이스를 등록하거나 등록하지 않고 회사의 데이터를 보호할 수 있도록 하는 MDM(모바일 디바이스 관리) 솔루션이 필요하지 않습니다.

Azure Active Directory 조건부 액세스를 사용 하면 클라우드 앱에 Intune 앱 보호 정책을 지 원하는 클라이언트 앱에 대 한 액세스를 제한할 수 있습니다. 예를 들어 Exchange Online에 대한 액세스를 Outlook 앱으로 제한할 수 있습니다.

조건부 액세스 용어에서 이러한 클라이언트 앱으로 알려져 **승인 된 클라이언트 앱**합니다.  

![조건부 액세스](./media/app-based-conditional-access/05.png)

승인된 클라이언트 앱의 목록은 [승인된 클라이언트 앱 요구 사항](technical-reference.md#approved-client-app-requirement)을 참조하세요.

와 같은 다른 정책 사용 하 여 앱 기반 조건부 액세스 정책을 결합할 수 있습니다 [장치 기반 조건부 액세스 정책](require-managed-devices.md) 개인 및 회사 장치에 대 한 데이터를 보호 하는 방법에 유연성을 제공 합니다.

## <a name="before-you-begin"></a>시작하기 전에

이 토픽은 다음 항목에 대해 잘 알고 있다고 가정합니다.

- [승인된 클라이언트 앱 요구 사항](technical-reference.md#approved-client-app-requirement) 기술 참조.
- 기본 개념 [Azure Active Directory의 조건부 액세스](overview.md)합니다.
- 하는 방법 [조건부 액세스 정책을 구성할](app-based-mfa.md)합니다.
- 합니다 [조건부 액세스 정책의 마이그레이션](best-practices.md#policy-migration)합니다.

## <a name="prerequisites"></a>필수 조건

앱 기반 조건부 액세스 정책을 만들려면 해야 Enterprise Mobility + Security 또는 Azure Active Directory premium 구독을 및 EMS 또는 Azure AD에 대 한 사용자 라이선스를 취득 해야 합니다. 

## <a name="exchange-online-policy"></a>Exchange Online 정책 

이 시나리오는 Exchange Online에 액세스 하기 위한 앱 기반 조건부 액세스 정책으로 구성 됩니다.

### <a name="scenario-playbook"></a>시나리오 플레이 북

이 시나리오는 다음과 같이 사용자를 가정합니다.

- iOS 또는 Android에는 기본 메일 애플리케이션을 사용하여 Exchange에 연결하도록 이메일을 구성합니다.
- 액세스는 Outlook 앱을 사용할 때만 가능하다는 것을 알리는 전자 메일을 수신합니다.
- 링크가 포함된 애플리케이션을 다운로드합니다.
- Outlook 애플리케이션을 열고 Azure AD 자격 증명으로 로그인합니다.
- 계속하려면 Authenticator(iOS) 또는 회사 포털(Android)을 설치하라는 메시지가 표시되었습니다.
- 애플리케이션을 설치하고 Outlook 앱으로 돌아가서 계속할 수 있습니다.
- 디바이스를 등록하라는 메시지가 표시되었습니다.
- 전자 메일에 액세스할 수 있습니다.

Intune 앱 보호 정책이 회사 데이터에는 액세스 시 활성화 하 고 응용 프로그램을 다시 시작, (응용 프로그램 및 플랫폼에 대해 구성 된) 경우 추가 PIN 등을 사용 하 여 사용자를 묻는 메시지가 나타납니다.

### <a name="configuration"></a>구성 

**1 단계-Exchange Online에 대 한 Azure AD 조건부 액세스 정책 구성**

이 단계에서는 조건부 액세스 정책의 다음 구성 요소를 구성 해야 합니다.

![조건부 액세스](./media/app-based-conditional-access/01.png)

1. 합니다 **이름을** 조건부 액세스 정책입니다.
1. **사용자 및 그룹**: 각 조건부 액세스 정책을 사용자 또는 선택한 그룹을 하나 이상 있어야 합니다.
1. **클라우드 앱:** 클라우드 앱으로 **Office 365 Exchange Online**을 선택해야 합니다.

   ![조건부 액세스](./media/app-based-conditional-access/07.png)

1. **조건:** **조건**으로 **디바이스 플랫폼** 및 **클라이언트 앱**을 구성해야 합니다.
   1. **디바이스 플랫폼**으로 **Android** 및 **iOS**를 선택합니다.

      ![조건부 액세스](./media/app-based-conditional-access/03.png)

   1. **클라이언트 앱(미리 보기)** 으로 **모바일 앱 및 데스크톱 앱** 및 **최신 인증 클라이언트**를 선택합니다.

      ![조건부 액세스](./media/app-based-conditional-access/91.png)

1. **액세스 제어**로 **승인된 클라이언트 앱 필요(미리 보기)** 를 선택해야 합니다.

   ![조건부 액세스](./media/app-based-conditional-access/05.png)

**2 단계-Exchange online Active Sync (EAS) 사용 하 여 Azure AD 조건부 액세스 정책 구성**

이 단계에서는 조건부 액세스 정책의 다음 구성 요소를 구성 해야 합니다.

![조건부 액세스](./media/app-based-conditional-access/06.png)

1. 합니다 **이름을** 조건부 액세스 정책입니다.
1. **사용자 및 그룹**: 각 조건부 액세스 정책을 사용자 또는 선택한 그룹을 하나 이상 있어야 합니다.
1. **클라우드 앱:** 클라우드 앱으로 **Office 365 Exchange Online**을 선택해야 합니다.

   ![조건부 액세스](./media/app-based-conditional-access/07.png)

1. **조건:** **조건**으로 **클라이언트 앱(미리 보기)** 을 구성해야 합니다. 
   1. **클라이언트 앱(미리 보기)** 으로 **모바일 앱 및 데스크톱 클라이언트** 및 **Exchange ActiveSync 클라이언트**를 선택합니다.

      ![조건부 액세스](./media/app-based-conditional-access/92.png)

   1. **액세스 제어**로 **승인된 클라이언트 앱 필요(미리 보기)** 를 선택해야 합니다.

      ![조건부 액세스](./media/app-based-conditional-access/05.png)

**3단계 - iOS 및 Android 클라이언트 애플리케이션에 대한 Intune 앱 보호 정책 구성**

![조건부 액세스](./media/app-based-conditional-access/09.png)

자세한 정보는 [Microsoft Intune으로 앱 및 데이터 보호](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune)를 참조하세요.

## <a name="exchange-online-and-sharepoint-online-policy"></a>Exchange Online 및 SharePoint Online 정책

이 시나리오는 Exchange Online에 액세스 하기 위한 모바일 앱 관리 정책 사용한 조건부 액세스 및 승인 된 앱을 사용 하 여 SharePoint Online으로 구성 됩니다.

### <a name="scenario-playbook"></a>시나리오 플레이 북

이 시나리오는 다음과 같이 사용자를 가정합니다.

- SharePoint 앱을 사용하여 연결하고 해당 회사 사이트를 보려고 시도합니다.
- Outlook 앱 자격 증명과 동일한 자격 증명을 사용 하 여 로그인 하려고 합니다.
- 다시 등록하지 않아도 리소스에 대한 액세스를 얻을 수 있습니다.

### <a name="configuration"></a>구성

**1 단계-Exchange Online 및 SharePoint Online에 대 한 Azure AD 조건부 액세스 정책 구성**

이 단계에서는 조건부 액세스 정책의 다음 구성 요소를 구성 해야 합니다.

![조건부 액세스](./media/app-based-conditional-access/71.png)

1. 합니다 **이름을** 조건부 액세스 정책입니다.
1. **사용자 및 그룹**: 각 조건부 액세스 정책을 사용자 또는 선택한 그룹을 하나 이상 있어야 합니다.
1. **클라우드 앱:** 클라우드 앱으로 **Office 365 Exchange Online** 및 **Office 365 SharePoint Online**을 선택해야 합니다. 

   ![조건부 액세스](./media/app-based-conditional-access/02.png)

1. **조건:** **조건**으로 **디바이스 플랫폼** 및 **클라이언트 앱**을 구성해야 합니다.
   1. **디바이스 플랫폼**으로 **Android** 및 **iOS**를 선택합니다.

      ![조건부 액세스](./media/app-based-conditional-access/03.png)

   1. **클라이언트 앱(미리 보기)** 으로 **모바일 앱 및 데스크톱 클라이언트** 및 **최신 인증 클라이언트**를 선택합니다.

      ![조건부 액세스](./media/app-based-conditional-access/91.png)

1. **액세스 제어**로 **승인된 클라이언트 앱 필요(미리 보기)** 를 선택해야 합니다.

   ![조건부 액세스](./media/app-based-conditional-access/05.png)

**2 단계-Exchange online Active Sync (EAS) 사용 하 여 Azure AD 조건부 액세스 정책 구성**

이 단계에서는 조건부 액세스 정책의 다음 구성 요소를 구성 해야 합니다.

![조건부 액세스](./media/app-based-conditional-access/06.png)

1. 합니다 **이름을** 조건부 액세스 정책입니다.
1. **사용자 및 그룹**: 각 조건부 액세스 정책을 사용자 또는 선택한 그룹을 하나 이상 있어야 합니다.
1. **클라우드 앱:** 클라우드 앱으로 **Office 365 Exchange Online**을 선택해야 합니다. 온라인 

   ![조건부 액세스](./media/app-based-conditional-access/07.png)

1. **조건:** **조건**으로 **클라이언트 앱**을 구성해야 합니다.
   1. **클라이언트 앱(미리 보기)** 으로 **모바일 앱 및 데스크톱 클라이언트** 및 **Exchange ActiveSync 클라이언트**를 선택합니다.

      ![조건부 액세스](./media/app-based-conditional-access/92.png)

   1. **액세스 제어**로 **승인된 클라이언트 앱 필요(미리 보기)** 를 선택해야 합니다.

      ![조건부 액세스](./media/app-based-conditional-access/05.png)

**3단계 - iOS 및 Android 클라이언트 애플리케이션에 대한 Intune 앱 보호 정책 구성**

![조건부 액세스](./media/app-based-conditional-access/09.png)

자세한 정보는 [Microsoft Intune으로 앱 및 데이터 보호](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune)를 참조하세요.

## <a name="app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>Exchange Online 및 SharePoint Online에 대한 앱 기반 또는 준수 디바이스 정책

이 시나리오는 Exchange Online에 대 한 액세스에 대 한 앱 기반 또는 준수 장치 조건부 액세스 정책으로 구성 됩니다.

### <a name="scenario-playbook"></a>시나리오 플레이 북

이 시나리오는 다음을 가정합니다.
 
- 일부 사용자는 (유무와 관계 없이 회사 장치)에 이미 등록 된
- 앱 보호 애플리케이션을 사용하여 Azure AD에 등록되지 않은 사용자는 리소스에 액세스하려면 디바이스를 등록해야 합니다.
- 앱 보호 애플리케이션을 사용하는 등록된 사용자는 디바이스에 다시 등록할 필요가 없습니다.

### <a name="configuration"></a>구성

**1 단계-Exchange Online 및 SharePoint Online에 대 한 Azure AD 조건부 액세스 정책 구성**

이 단계에서는 조건부 액세스 정책의 다음 구성 요소를 구성 해야 합니다.

![조건부 액세스](./media/app-based-conditional-access/62.png)

1. 합니다 **이름을** 조건부 액세스 정책입니다.
1. **사용자 및 그룹**: 각 조건부 액세스 정책을 사용자 또는 선택한 그룹을 하나 이상 있어야 합니다.
1. **클라우드 앱:** 클라우드 앱으로 **Office 365 Exchange Online** 및 **Office 365 SharePoint Online**을 선택해야 합니다. 

     ![조건부 액세스](./media/app-based-conditional-access/02.png)

1. **조건:** **조건**으로 **디바이스 플랫폼** 및 **클라이언트 앱**을 구성해야 합니다. 
   1. **디바이스 플랫폼**으로 **Android** 및 **iOS**를 선택합니다.

      ![조건부 액세스](./media/app-based-conditional-access/03.png)

   1. **클라이언트 앱(미리 보기)** 으로 **모바일 앱 및 데스크톱 클라이언트** 및 **최신 인증 클라이언트**를 선택합니다.

      ![조건부 액세스](./media/app-based-conditional-access/91.png)

1. **액세스 제어**로 다음을 선택해야 합니다.
   - **디바이스를 준수 상태로 표시해야 함**
   - **승인된 클라이언트 앱(미리 보기) 필요**
   - **선택된 컨트롤 중 하나가 필요**   
 
      ![조건부 액세스](./media/app-based-conditional-access/11.png)

**2 단계-Exchange online Active Sync (EAS) 사용 하 여 Azure AD 조건부 액세스 정책 구성**

이 단계에서는 조건부 액세스 정책의 다음 구성 요소를 구성 해야 합니다.

![조건부 액세스](./media/app-based-conditional-access/61.png)

1. 합니다 **이름을** 조건부 액세스 정책입니다.
1. **사용자 및 그룹**: 각 조건부 액세스 정책을 사용자 또는 선택한 그룹을 하나 이상 있어야 합니다.
1. **클라우드 앱:** 클라우드 앱으로 **Office 365 Exchange Online**을 선택해야 합니다. 

   ![조건부 액세스](./media/app-based-conditional-access/07.png)

1. **조건:** **조건**으로 **클라이언트 앱**을 구성해야 합니다. 

   **클라이언트 앱(미리 보기)** 으로 **모바일 앱 및 데스크톱 클라이언트** 및 **Exchange ActiveSync 클라이언트**를 선택합니다.

   ![조건부 액세스](./media/app-based-conditional-access/91.png)

1. **액세스 제어**로 **승인된 클라이언트 앱 필요(미리 보기)** 를 선택해야 합니다.
 
   ![조건부 액세스](./media/app-based-conditional-access/11.png)

**3단계 - iOS 및 Android 클라이언트 애플리케이션에 대한 Intune 앱 보호 정책 구성**

![조건부 액세스](./media/app-based-conditional-access/09.png)

자세한 정보는 [Microsoft Intune으로 앱 및 데이터 보호](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune)를 참조하세요.

## <a name="app-based-and-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>Exchange Online 및 SharePoint Online에 대한 앱 기반 및 준수 디바이스 정책

이 시나리오는 Exchange Online에 대 한 액세스에 대 한 앱 기반 및 준수 장치 조건부 액세스 정책으로 구성 됩니다.

### <a name="scenario-playbook"></a>시나리오 플레이 북

이 시나리오는 다음과 같이 사용자를 가정합니다.
 
- iOS 또는 Android에는 기본 메일 애플리케이션을 사용하여 Exchange에 연결하도록 이메일을 구성합니다.
- 액세스하려면 디바이스를 등록해야 함을 알리는 전자 메일을 수신합니다.
- 회사 포털을 다운로드하고 회사 포털에 로그인합니다.
- 메일을 확인하고 Outlook 앱을 사용하도록 요청받았습니다.
- Outlook 앱을 다운로드합니다.
- Outlook 앱을 열고가 등록에 사용되는 자격 증명을 입력합니다.
- 사용자는 전자 메일에 액세스할 수 있습니다.

회사 데이터에 액세스하는 시기에 Intune 앱 보호 정책이 활성화되어 있으면 사용자에게 애플리케이션을 다시 시작하고 추가 PIN 등을 사용하라는 메시지가 표시될 수도 있습니다(애플리케이션 및 플랫폼에 대해 구성된 경우).

### <a name="configuration"></a>구성

**1 단계-Exchange Online 및 SharePoint Online에 대 한 Azure AD 조건부 액세스 정책 구성**

이 단계에서는 조건부 액세스 정책의 다음 구성 요소를 구성 해야 합니다.

![조건부 액세스](./media/app-based-conditional-access/62.png)

1. 합니다 **이름을** 조건부 액세스 정책입니다.
1. **사용자 및 그룹**: 각 조건부 액세스 정책을 사용자 또는 선택한 그룹을 하나 이상 있어야 합니다.
1. **클라우드 앱:** 클라우드 앱으로 **Office 365 Exchange Online** 및 **Office 365 SharePoint Online**을 선택해야 합니다. 

   ![조건부 액세스](./media/app-based-conditional-access/02.png)

1. **조건:** **조건**으로 **디바이스 플랫폼** 및 **클라이언트 앱**을 구성해야 합니다. 
   1. **디바이스 플랫폼**으로 **Android** 및 **iOS**를 선택합니다.

      ![조건부 액세스](./media/app-based-conditional-access/03.png)

   1. **클라이언트 앱(미리 보기)** 으로 **모바일 앱 및 데스크톱 앱** 및 **최신 인증 클라이언트**를 선택합니다.

      ![조건부 액세스](./media/app-based-conditional-access/91.png)

1. **액세스 제어**로 다음을 선택해야 합니다.
   - **디바이스를 준수 상태로 표시해야 함**
   - **승인된 클라이언트 앱(미리 보기) 필요**
   - **선택된 컨트롤이 모두 필요**   
 
      ![조건부 액세스](./media/app-based-conditional-access/13.png)



**2 단계-Exchange online Active Sync (EAS) 사용 하 여 Azure AD 조건부 액세스 정책 구성**

이 단계에서는 조건부 액세스 정책의 다음 구성 요소를 구성 해야 합니다.

![조건부 액세스](./media/app-based-conditional-access/61.png)

1. 합니다 **이름을** 조건부 액세스 정책입니다.
1. **사용자 및 그룹**: 각 조건부 액세스 정책을 사용자 또는 선택한 그룹을 하나 이상 있어야 합니다.
1. **클라우드 앱:** 클라우드 앱으로 **Office 365 Exchange Online**을 선택해야 합니다. 

   ![조건부 액세스](./media/app-based-conditional-access/07.png)

1. **조건:** **조건**으로 **클라이언트 앱(미리 보기)** 을 구성해야 합니다. 

   **클라이언트 앱(미리 보기)** 으로 **모바일 앱 및 데스크톱 클라이언트** 및 **Exchange ActiveSync 클라이언트**를 선택합니다.

   ![조건부 액세스](./media/app-based-conditional-access/92.png)

1. **액세스 제어**로 다음을 선택해야 합니다.
   - **디바이스를 준수 상태로 표시해야 함**
   - **승인된 클라이언트 앱(미리 보기) 필요**
   - **선택된 컨트롤이 모두 필요**   
 
      ![조건부 액세스](./media/app-based-conditional-access/64.png)

**3단계 - iOS 및 Android 클라이언트 애플리케이션에 대한 Intune 앱 보호 정책 구성**

![조건부 액세스](./media/app-based-conditional-access/09.png)

자세한 정보는 [Microsoft Intune으로 앱 및 데이터 보호](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune)를 참조하세요.

## <a name="next-steps"></a>다음 단계

조건부 액세스 정책을 구성 하는 방법을 알고 싶다면 [Azure Active Directory 조건부 액세스를 사용 하 여 특정 앱에 대 한 MFA 필요](app-based-mfa.md)합니다.

사용자 환경에 대 한 조건부 액세스 정책 구성 준비 인 경우 참조를 [Azure Active Directory의 조건부 액세스 모범 사례](best-practices.md)합니다. 
