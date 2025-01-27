---
title: ISE(통합 서비스 환경)를 사용하여 Azure Logic Apps에서 Azure 가상 네트워크에 액세스
description: 이 개요에서는 ISE(통합 서비스 환경)가 논리 앱이 Azure VNET(가상 네트워크)에 액세스하는 데 도움이 되는 방법 설명
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 07/19/2019
ms.openlocfilehash: 3e14604955a64c7a146a947c5c320b42ea3ebcba
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68325404"
---
# <a name="access-to-azure-virtual-network-resources-from-azure-logic-apps-by-using-integration-service-environments-ises"></a>ISE(통합 서비스 환경)를 사용하여 Azure Logic Apps에서 Azure Virtual Network 리소스에 액세스

경우에 따라 논리 앱 및 통합 계정에는 [Azure virtual network](../virtual-network/virtual-networks-overview.md)내에 있는 vm (가상 머신), 기타 시스템 또는 서비스와 같은 보안 리소스에 대 한 액세스 권한이 필요 합니다. 이 액세스를 설정 하기 위해 논리 앱을 실행 하 고 통합 계정을 만들 수 있는 [ISE ( *통합 서비스 환경* )를 만들](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) 수 있습니다.

ISE를 만들 때 Azure는이 ISE를 Azure 가상 네트워크에 *삽입* 하 여 azure 가상 네트워크에 Logic Apps 서비스의 전용 및 격리 된 인스턴스를 배포 합니다. 이 프라이빗 인스턴스는 스토리지 등의 전용 리소스를 사용하며, 공용 "글로벌" Logic Apps 서비스와는 별도로 실행됩니다. 또한 격리 된 개인 인스턴스와 공용 전역 인스턴스를 분리 하면 다른 Azure 테 넌 트가 응용 프로그램의 성능에 미치는 영향을 줄일 수 있습니다 .이는 ["잡음이 있는 환경" 효과](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors)라고도 합니다.

ISE를 만든 후 논리 앱 또는 통합 계정 만들기로 이동 하면 ISE를 논리 앱 또는 통합 계정의 위치로 선택할 수 있습니다.

![통합 서비스 환경 선택](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

이제 논리 앱에서 다음 항목 중 하나를 사용 하 여 가상 네트워크 내부 또는 연결 된 시스템에 직접 액세스할 수 있습니다.

* 해당 시스템에 대 한 **ISE**레이블 커넥터 (예: SQL Server
* HTTP 트리거 또는 작업과 같은 **핵심**레이블이 지정 된 기본 제공 트리거 또는 동작입니다.
* 사용자 지정 커넥터

이 개요에서는 ISE에서 논리 앱 및 통합 계정에 Azure virtual network에 대 한 직접 액세스를 제공 하 고 ISE와 global Logic Apps 서비스의 차이점을 비교 하는 방법에 대해 자세히 설명 합니다.

> [!IMPORTANT]
> ISE에서 실행 되는 논리 앱, 기본 제공 트리거, 기본 제공 작업 및 커넥터는 소비 기반 요금제와는 다른 요금제를 사용 합니다. ISEs에 대 한 가격 책정 및 청구의 작동 방식에 대 한 자세한 [Logic Apps 내용은 가격 책정 모델](../logic-apps/logic-apps-pricing.md#fixed-pricing)을 참조 하세요. 가격 책정 요금은 [Logic Apps 가격 책정](../logic-apps/logic-apps-pricing.md)을 참조 하세요.
>
> ISE는 실행 지속 시간, 저장소 보존, 처리량, HTTP 요청 및 응답 시간 제한, 메시지 크기 및 사용자 지정 커넥터 요청에 대 한 제한도 증가 시켰습니다. 
> 자세한 내용은 [Azure Logic Apps에 대 한 제한 및 구성](logic-apps-limits-and-config.md)을 참조 하세요.

<a name="difference"></a>

## <a name="isolated-versus-global"></a>격리 방식과 전역 방식 비교

Azure에서 ISE (통합 서비스 환경)를 만들 때 ISE를 *삽입* 하려는 azure virtual network를 선택할 수 있습니다. 그런 다음 Azure는 Logic Apps 서비스의 전용 인스턴스를 가상 네트워크에 삽입 하거나 배포 합니다. 이 작업을 통해 격리된 환경이 만들어지면 전용 리소스에서 논리 앱을 만들고 실행할 수 있습니다. 논리 앱을 만들 때 앱의 위치로 ISE를 선택 하면 논리 앱이 해당 네트워크의 가상 네트워크 및 리소스에 직접 액세스할 수 있습니다.

ISE의 논리 앱은 전역 Logic Apps 서비스와 같은 사용자 환경과 비슷한 기능을 제공합니다. 동일한 기본 제공 트리거, 기본 제공 작업 및 글로벌 Logic Apps 서비스의 커넥터를 사용할 수 있을 뿐만 아니라 ISE 특정 커넥터를 사용할 수도 있습니다. 예를 들어 ISE에서 실행 되는 버전을 제공 하는 몇 가지 표준 커넥터는 다음과 같습니다.

* Azure Blob Storage, File Storage 및 Table Storage
* Azure Queues, Azure Service Bus, Azure Event Hubs 및 IBM MQ
* FTP 및 SFTP-SSH
* SQL Server, SQL Data Warehouse, Azure Cosmos DB
* AS2, X12 및 EDIFACT

ISE 커넥터와 기타 커넥터의 차이는 트리거와 작업이 실행되는 위치입니다.

* ISE에서 기본 제공 트리거 및 HTTP와 같은 작업은 항상 논리 앱과 동일한 ISE에서 실행 되며 **핵심** 레이블을 표시 합니다.

  !["Core" 기본 제공 트리거 및 동작을 선택 합니다.](./media/connect-virtual-network-vnet-isolated-environment-overview/select-core-built-in-actions-triggers.png)

* ISE에서 실행 되는 커넥터에는 전역 Logic Apps 서비스에서 사용할 수 있는 공개적으로 호스팅된 버전이 있습니다. 두 버전을 제공 하는 커넥터의 경우 **ise** 레이블이 있는 커넥터는 항상 논리 앱과 동일한 ise에서 실행 됩니다. **ISE** 레이블이 없는 커넥터는 전역 Logic Apps 서비스에서 실행됩니다.

  ![ISE 커넥터 선택](./media/connect-virtual-network-vnet-isolated-environment-overview/select-ise-connectors.png)

또한 ISE는 실행 지속 시간, 저장소 보존, 처리량, HTTP 요청 및 응답 시간 제한, 메시지 크기 및 사용자 지정 커넥터 요청에 대 한 증가 된 제한을 제공 합니다. 자세한 내용은 [Azure Logic Apps에 대 한 제한 및 구성](logic-apps-limits-and-config.md)을 참조 하세요.

<a name="ise-level"></a>

## <a name="ise-skus"></a>ISE Sku

ISE를 만들 때 개발자 SKU 또는 프리미엄 SKU를 선택할 수 있습니다. 이러한 Sku 간의 차이점은 다음과 같습니다.

* **Developer**

  는 실험, 개발 및 테스트에 사용할 수 있지만 프로덕션 또는 성능 테스트에는 사용할 수 없는 저렴 한 ISE를 제공 합니다. 개발자 SKU에는 고정 월별 가격에 대 한 기본 제공 트리거 및 작업, 표준 커넥터, 엔터프라이즈 커넥터 및 단일 [무료 계층](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits) 통합 계정이 포함 되어 있습니다. 그러나이 SKU에는 SLA (서비스 수준 계약), 용량을 확장 하는 옵션 또는 재활용 중 중복성을 포함 하지 않습니다. 즉, 지연 또는 가동 중지 시간이 발생할 수 있습니다.

* **Premium**

  프로덕션에 사용할 수 있는 ISE를 제공 하 고 SLA 지원, 기본 제공 트리거 및 작업, 표준 커넥터, 엔터프라이즈 커넥터, 단일 [표준 계층](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits) 통합 계정, 용량을 확장 하는 옵션 및 중복성을 포함 합니다. 고정 된 월별 가격에 대 한 재활용

가격 책정 요금은 [Logic Apps 가격 책정](https://azure.microsoft.com/pricing/details/logic-apps/)을 참조 하세요. ISEs에 대 한 가격 책정 및 청구의 작동 방식에 대 한 자세한 [Logic Apps 내용은 가격 책정 모델](../logic-apps/logic-apps-pricing.md#fixed-pricing)을 참조 하세요.

<a name="on-premises"></a>

## <a name="access-to-on-premises-data-sources"></a>온-프레미스 데이터 원본에 대 한 액세스

Azure 가상 네트워크에 연결 된 온-프레미스 시스템의 경우 논리 앱이 다음 항목 중 하나를 사용 하 여 해당 시스템에 직접 액세스할 수 있도록 ISE를 해당 네트워크에 삽입 합니다.

* 해당 시스템에 대 한 ISE 버전 커넥터 (예: SQL Server
* HTTP 동작
* 사용자 지정 커넥터

  * 온-프레미스 데이터 게이트웨이를 필요로 하는 사용자 지정 커넥터를 사용 하 고 ISE 외부에서 커넥터를 만든 경우 ISE의 논리 앱 에서도 이러한 커넥터를 사용할 수 있습니다.
  
  * ISE에서 만든 사용자 지정 커넥터는 온-프레미스 데이터 게이트웨이와 작동 하지 않습니다. 그러나 이러한 커넥터는 ISE를 호스트 하는 가상 네트워크에 연결 된 온-프레미스 데이터 원본에 직접 액세스할 수 있습니다. 따라서 ISE의 논리 앱은 이러한 리소스와 통신할 때 데이터 게이트웨이가 필요 하지 않을 수 있습니다.

가상 네트워크에 연결 되어 있지 않거나 ISE-버전 커넥터가 없는 온-프레미스 시스템의 경우 논리 앱이 해당 시스템에 연결 하기 전에 먼저 [온-프레미스 데이터 게이트웨이를 설정](../logic-apps/logic-apps-gateway-install.md) 해야 합니다.

<a name="create-integration-account-environment"></a>

## <a name="integration-accounts-with-ise"></a>ISE와의 통합 계정

ISE(통합 서비스 환경) 내에서 논리 앱을 통해 통합 계정을 사용할 수 있습니다. 그러나 해당 통합 계정은 연결된 논리 앱과 *동일한 ISE*를 사용해야 합니다. ISE의 논리 앱은 같은 ISE에 있는 통합 계정만 참조할 수 있습니다. 통합 계정을 만들 때는 통합 계정의 위치로 ISE를 선택할 수 있습니다. ISE를 사용 하 여 통합 계정에 대 한 가격 책정 및 청구 작업 방법을 알아보려면 [Logic Apps 가격 책정 모델](../logic-apps/logic-apps-pricing.md#fixed-pricing)을 참조 하세요. 가격 책정 요금은 [Logic Apps 가격 책정](https://azure.microsoft.com/pricing/details/logic-apps/)을 참조 하세요.

## <a name="next-steps"></a>다음 단계

* [격리된 논리 앱에서 Azure 가상 네트워크에 연결](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)하는 방법 알아보기
* [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)에 대해 자세히 알아보기
* [Azure 서비스에 대한 가상 네트워크 통합](../virtual-network/virtual-network-for-azure-services.md)에 대해 알아보기
