---
title: Azure App Service의 Mobile Apps 정보
description: App Service가 엔터프라이즈 모바일 앱에 제공하는 이점을 알아봅니다.
services: app-service\mobile
documentationcenter: ''
author: elamalani
manager: yochayk
editor: ''
ms.assetid: 4e96cb9d-a632-4cf6-8219-0810d8ade3f9
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-multiple
ms.devlang: na
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: 55f33821a748789fb11b44dc08c367961b4258f4
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67440286"
---
# <a name="getting-started"> </a>Azure App Service의 Mobile Apps 정보

> [!NOTE]
> Visual Studio App Center는 모바일 앱 개발을 위한 중앙 새 및 통합 서비스에 투자 합니다. 개발자가 사용할 수 **빌드**를 **테스트** 하 고 **배포** 연속 통합 및 배달 파이프라인을 설정 하는 서비스입니다. 개발자 상태 및 사용 하 여 해당 앱의 사용량을 모니터링할 수 있습니다, 앱을 배포한 후 합니다 **Analytics** 하 고 **진단** , 서비스를 사용 하 여 사용자와 소통 하세요를 **푸시** 서비스입니다. 개발자가 활용할 수도 있습니다 **인증** 해당 사용자를 인증 하 고 **데이터** 유지 하 고 클라우드에 앱 데이터 동기화 서비스. 체크 아웃 [App Center](https://appcenter.ms/?utm_source=zumo&utm_campaign=app-service-mobile-value-prop) 지금 합니다.
>

Azure App Service는 완전히 관리되는 PaaS([platform as a service](https://azure.microsoft.com/overview/what-is-paas/))로써 전문 개발자를 위해 제공됩니다. 이 서비스는 웹, 모바일 및 통합 시나리오에 풍부한 기능 집합을 제공합니다. 

Azure App Service의 Mobile Apps 기능은 엔터프라이즈 개발자 및 시스템 통합자에게 확장성이 뛰어나고 전 세계에서 사용 가능한 모바일 응용 프로그램 개발 플랫폼을 제공합니다.

![Mobile Apps 기능에 대한 시각적 개요](./media/app-service-mobile-value-prop/overview.png)

## <a name="why-mobile-apps"></a>Mobile Apps 사용 이유
Mobile Apps 기능을 사용하면 다음을 수행할 수 있습니다.

* **네이티브 및 플랫폼 간 앱 빌드**: 빌드하는 앱이 네이티브 iOS, Android 및 Windows 앱인지 또는 크로스 플랫폼 Xamarin 앱이나 Cordova(PhoneGap) 앱인지에 관계없이 네이티브 SDK를 통해 App Service를 사용할 수 있습니다.
* **엔터프라이즈 시스템에 연결**: Mobile Apps 기능을 사용하면 몇 분 내에 회사 로그인을 추가하고 엔터프라이즈 온-프레미스 또는 클라우드 리소스에 연결할 수 있습니다.
* **데이터 동기화를 사용하여 오프라인 지원 앱 빌드**: 오프라인에서 작동하는 앱을 빌드하여 모바일 작업자의 생산성을 높이고 엔터프라이즈 데이터 원본이나 SaaS(software as a service) API와 연결된 경우 Mobile Apps를 사용하여 백그라운드에서 데이터를 동기화합니다.
* **몇 초 내에 수백만 명에게 푸시 알림 전송**: 모든 디바이스에서 고객의 요구에 맞게 개인 설정하고 적절한 시간에 보내는 즉시 푸시 알림을 통해 고객을 끌어들입니다.

## <a name="mobile-apps-features"></a>Mobile Apps 기능
다음 기능은 클라우드 사용 모바일 개발에 중요합니다.

* **인증 및 권한 부여**: 엔터프라이즈 인증용 Azure Active Directory 및 소셜 공급 기업(예: Facebook, Google, Twitter 및 Microsoft 계정)을 포함한 ID 공급 기업을 지원합니다. Mobile Apps은 각 공급자에 대해 OAuth 2.0 서비스를 제공합니다. 또한 공급자 특정 기능에 대한 ID 공급자의 SDK도 통합할 수 있습니다.

    [인증 기능]에 대해 자세히 알아보세요.

* **데이터 액세스**: Mobile Apps는 Azure SQL Database 또는 온-프레미스 SQL Server에 연결된 모바일 친화적인 OData v3 데이터 원본을 제공합니다. 이 서비스가 Entity Framework를 기반으로 할 수 있기 때문에 [Azure Table Storage], MongoDB, [Azure Cosmos DB] 및 SaaS API 공급자(예: Office 365 및 Salesforce.com)를 비롯한 다른 NoSQL 및 SQL 데이터 공급자와 쉽게 통합할 수 있습니다.

* **오프라인 동기화**: 클라이언트 SDK를 통해 오프라인 데이터 세트에서 작동하는 강력하고 응답성이 뛰어난 모바일 애플리케이션을 쉽게 빌드할 수 있습니다. 충돌 해결 지원을 비롯하여 백 엔드 데이터와 이 데이터 세트를 자동으로 동기화할 수 있습니다.

  [데이터 기능]에 대해 자세히 알아보세요.

* **푸시 알림**: 클라이언트 SDK는 Azure Notification Hubs의 등록 기능과 원활하게 통합되어 수백만 명의 사용자에게 푸시 알림을 동시에 보낼 수 있습니다.

  [푸시 알림 기능]에 대해 자세히 알아보세요.

* **클라이언트 SDK**: 완전한 클라이언트 SDK 모음으로, 네이티브 개발([iOS], [Android] 및 [Windows]), 플랫폼 간 개발([Xamarin.iOS 및 Xamarin.Android], [Xamarin.Forms]) 및 하이브리드 애플리케이션 개발([Apache Cordova])을 포함하고 있습니다. 각 클라이언트 SDK는 MIT 라이선스로 사용할 수 있으며 오픈 소스입니다.

## <a name="azure-app-service-features"></a>Azure App Service 기능
다음 플랫폼 기능은 모바일 프로덕션 사이트에 유용합니다.

* **자동 크기 조정**: App Service를 사용하여 들어오는 고객 부하를 처리하기 위해 신속하게 확장하거나 축소할 수 있습니다. VM 수와 크기를 수동으로 선택하거나 부하 또는 일정에 따라 모바일 앱 백 엔드의 크기를 조정하도록 자동 크기 조정을 설정합니다.

  [자동 크기 조정]에 대해 자세히 알아보세요.

* **스테이징 환경**: App Service는 여러 버전의 사이트를 실행할 수 있으므로 A/B 테스트, 대규모 DevOps 계획의 일부로 프로덕션에서 테스트 및 새로운 백 엔드의 현재 위치 스테이징을 수행할 수 있습니다.

  [스테이징 환경]에 대해 자세히 알아보세요.

* **지속적인 배포**: App Service는 일반 _SCM(원본 제어 관리)_ 시스템과 통합되어 새 버전의 백 엔드를 쉽게 배포할 수 있습니다.

  [배포 옵션](../app-service/deploy-local-git.md)에 대해 자세히 알아보세요.

* **가상 네트워킹**: App Service는 가상 네트워크, Azure ExpressRoute 또는 하이브리드 연결을 사용하여 온-프레미스 리소스에 연결할 수 있습니다.

  [하이브리드 연결], [가상 네트워크] 및 [ExpressRoute]에 대해 자세히 알아보세요.

* **격리 및 전용 환경**: Azure App Service 앱을 안전하게 실행하기 위해 완전히 격리된 전용 환경에서 App Service를 실행할 수 있습니다. 이 환경은 높은 확장성, 격리 또는 보안된 네트워크 액세스가 요구되는 애플리케이션 워크로드에 이상적입니다.

  [App Service 환경]에 대해 자세히 알아보세요.

## <a name="next-steps"></a>다음 단계

Azure App Service에서 Mobile Apps를 시작하려면 [시작] 자습서를 완료합니다. 자습서에서는 선택한 모바일 백 엔드과 클라이언트를 생성하는 기본 사항을 설명합니다. 인증 통합, 오프라인 동기화 및 푸시 알림에 대해서도 설명합니다. 각 클라이언트 애플리케이션에 한 번씩, 여러 번 이 자습서를 완료할 수 있습니다.

Mobile Apps에 대한 자세한 내용은 [학습 맵]을 검토하세요.
Azure App Service 플랫폼에 대한 자세한 내용은 [Azure App Service]를 참조하세요.

<!-- URLs. -->
[Migrate your mobile service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[시작]: app-service-mobile-ios-get-started.md
[Azure Table Storage]:../cosmos-db/table-storage-how-to-use-dotnet.md
[Azure Cosmos DB]: ../cosmos-db/sql-api-get-started.md
[인증 기능]: ./app-service-mobile-auth.md
[데이터 기능]: ./app-service-mobile-offline-data-sync.md
[푸시 알림 기능]: ../notification-hubs/notification-hubs-push-notification-overview.md
[iOS]: ./app-service-mobile-ios-how-to-use-client-library.md
[Android]: ./app-service-mobile-android-how-to-use-client-library.md
[Windows]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin.iOS 및 Xamarin.Android]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin.Forms]: ./app-service-mobile-xamarin-forms-get-started.md
[Apache Cordova]: ./app-service-mobile-cordova-how-to-use-client-library.md
[자동 크기 조정]: ../app-service/web-sites-scale.md
[스테이징 환경]: ../app-service/deploy-staging-slots.md
[하이브리드 연결]: ../biztalk-services/integration-hybrid-connection-overview.md
[가상 네트워크]: ../app-service/web-sites-integrate-with-vnet.md
[ExpressRoute]: ../app-service/environment/app-service-app-service-environment-network-configuration-expressroute.md
[App Service 환경]: ../app-service/environment/intro.md
[학습 맵]: https://azure.microsoft.com/documentation/learning-paths/appservice-mobileapps/
