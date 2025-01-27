---
title: Azure API Management 인스턴스에 대한 사용자 지정 도메인 이름 구성 | Microsoft Docs
description: 이 항목에서는 Azure API Management 인스턴스에 대해 사용자 지정 도메인 이름을 구성하는 방법에 대해 설명합니다.
services: api-management
documentationcenter: ''
author: vladvino
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 07/01/2019
ms.author: apimpm
ms.openlocfilehash: 9eb03be5cd9704c3b124bfb16fd30c5c3466890d
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68326135"
---
# <a name="configure-a-custom-domain-name"></a>사용자 지정 도메인 이름 구성

Azure API Management 서비스 인스턴스를 만들면 Azure에서 azure-api.net의 하위 도메인 (예: `apim-service-name.azure-api.net`)을 할당 합니다. 그러나 사용자 지정 도메인 이름 (예: **contoso.com**)을 사용 하 여 API Management 끝점을 노출할 수 있습니다. 이 자습서에서는 기존 사용자 지정 DNS 이름을 API Management 인스턴스에 의해 노출 된 끝점에 매핑하는 방법을 보여 줍니다.

> [!WARNING]
> 인증서 고정을 사용하여 애플리케이션의 보안을 개선하고자 하는 고객은 사용자 지정 도메인 이름 및 관리하는 인증서(기본 인증서 아님)를 사용해야 합니다. 대신 기본 인증서를 고정하는 고객은 자신이 제어하지 않는 인증서의 속성에 대한 강한 종속성을 가지게 되므로 해당 방법은 권장하지 않습니다.

## <a name="prerequisites"></a>필수 구성 요소

이 문서에 설명한 단계를 수행하려면 다음 항목이 있어야 합니다.

-   활성화된 Azure 구독.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

-   API Management 인스턴스입니다. 자세한 내용은 [Azure API Management 인스턴스 만들기](get-started-create-service-instance.md)를 참조하세요.
-   사용자가 소유하고 있는 사용자 지정 도메인 이름입니다. 사용하려는 사용자 지정 도메인 이름은 DNS 서버에서 별도로 확보되어 호스트되어야 합니다. 이 항목은 사용자 지정 도메인 이름을 호스트하는 방법에 대해서는 설명하지 않습니다.
-   공개 및 프라이빗 키(.PFX)가 있는 유효한 인증서가 있어야 합니다. 주체 또는 SAN (주체 대체 이름)은 도메인 이름과 일치 해야 합니다. 이렇게 하면 API Management 인스턴스가 SSL을 통해 Url을 안전 하 게 노출할 수 있습니다.

## <a name="use-the-azure-portal-to-set-a-custom-domain-name"></a>Azure Portal을 사용하여 사용자 지정 도메인 이름 설정

1. [Azure Portal](https://portal.azure.com/)에서 API Management 인스턴스로 이동 합니다.
1. **사용자 지정 도메인 및 SSL**을 선택합니다.

    사용자 지정 도메인 이름을 할당할 수 있는 끝점은 여러 가지가 있습니다. 현재는 다음 엔드포인트를 사용할 수 있습니다.

    - **프록시**(기본값: `<apim-service-name>.azure-api.net`),
    - **포털**(기본값: `<apim-service-name>.portal.azure-api.net`),
    - **관리**(기본값: `<apim-service-name>.management.azure-api.net`),
    - **SCM**(기본값: `<apim-service-name>.scm.azure-api.net`).

    > [!NOTE]
    > 이들 중 일부 또는 모든 엔드포인트를 업데이트할 수 있습니다. 일반적으로 고객이 **프록시**(이 URL은 API Management를 통해 노출된 API를 호출하는 데 사용됨) 및 **포털**(개발자 포털 URL)을 업데이트합니다. **관리** 및 **SCM** 끝점은 API Management 인스턴스 소유자만 내부적으로 사용 되므로 사용자 지정 도메인 이름을 할당 하는 빈도가 낮습니다. 대부분의 경우 지정 된 끝점에 대해 단일 사용자 지정 도메인 이름만 설정할 수 있습니다. 그러나 **프리미엄** 계층은 **프록시** 끝점에 대 한 여러 호스트 이름 설정을 지원 합니다.

1. 업데이트하려는 엔드포인트를 선택합니다.
1. 오른쪽 창에서 **사용자 지정**을 클릭합니다.

    - **사용자 지정 도메인 이름**에서 사용하려는 이름을 지정합니다. `api.contoso.com` )을 입력합니다. 와일드 카드 도메인 이름 (예: \*. domain.com)도 지원 됩니다.
    - **인증서**의 Key Vault에서 인증서를 선택 합니다. 유효한를 업로드할 수도 있습니다. 인증서가 암호로 보호 되는 경우 PFX 파일 및 **암호**를 제공 합니다.

    > [!TIP]
    > Azure Key Vault를 사용 하 여 인증서를 관리 하 고이를 autorotate로 설정 하는 것이 좋습니다.
    > Azure Key Vault를 사용 하 여 사용자 지정 도메인 SSL 인증서를 관리 하는 경우 인증서가 _비밀이_아니라 [ _인증서_로](https://docs.microsoft.com/rest/api/keyvault/CreateCertificate/CreateCertificate)Key Vault에 삽입 되어 있는지 확인 합니다.
    >
    > SSL 인증서를 페치 하려면 API Management 인증서를 포함 하는 Azure Key Vault에 대 한 암호 가져오기 권한이 있어야 합니다. Azure Portal 사용 하는 경우 필요한 모든 구성 단계가 자동으로 완료 됩니다. 명령줄 도구 또는 관리 API를 사용 하는 경우 이러한 사용 권한을 수동으로 부여 해야 합니다. 이 작업은 다음 두 단계로 수행됩니다. 먼저 API Management 인스턴스의 관리 되는 id 페이지를 사용 하 여 관리 Id가 사용 하도록 설정 되었는지 확인 하 고 해당 페이지에 표시 된 보안 주체 id를 기록해 둡니다. 그런 다음 인증서를 포함 하는 Azure Key Vault에 대 한 사용 권한 목록을 부여 하 고이 보안 주체 id에 대 한 암호를 가져옵니다.
    >
    > 인증서가 API Management autorotate로 설정 된 경우 서비스의 가동 중지 시간 없이 자동으로 최신 버전이 자동으로 선택 됩니다 (API Management 계층에 개발자 계층을 제외한 모든 계층에 대 한 SLA가 있는 경우).

1. 적용을 클릭합니다.

    > [!NOTE]
    > 배포 크기에 따라, 인증서를 할당하는 과정에 15분 이상 소요될 수 있습니다. 개발자 SKU는 가동 중지되고, 기본 이상의 SKU는 가동 중지되지 않습니다.

[!INCLUDE [api-management-custom-domain](../../includes/api-management-custom-domain.md)]

## <a name="dns-configuration"></a>DNS 구성

사용자 지정 도메인 이름에 대 한 DNS를 구성 하는 경우 두 가지 옵션이 있습니다.

- 구성 된 사용자 지정 도메인 이름의 끝점을 가리키는 CNAME 레코드를 구성 합니다.
- API Management 게이트웨이 IP 주소를 가리키는 A 레코드를 구성 합니다.

> [!NOTE]
> API 관리 인스턴스 IP 주소는 정적 이지만 몇 가지 시나리오에서 변경 될 수 있습니다. 따라서 사용자 지정 도메인을 구성 하는 경우 CNAME을 사용 하는 것이 좋습니다. DNS 구성 방법을 선택할 때 고려해 야 합니다. [API MANANAGEMENT FAQ](https://docs.microsoft.com/azure/api-management/api-management-faq#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules)를 참조 하세요.

## <a name="next-steps"></a>다음 단계

[서비스 업그레이드 및 크기 조정](upgrade-and-scale.md)
