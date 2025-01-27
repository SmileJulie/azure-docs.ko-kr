---
title: Azure Active Directory Domain Services 개요 | Microsoft Docs
description: Azure Active Directory Azure Active Directory Domain Services 개요
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 0d47178f-773e-45f9-9ff4-9e8cffa4ffa2
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: iainfou
ms.openlocfilehash: e29936915f0cd0b7e7ae7adfdbdb90d31195cd34
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67472743"
---
# <a name="azure-active-directory-ad-domain-services"></a>Azure AD(Active Directory) Domain Services
## <a name="overview"></a>개요
Azure 인프라 서비스를 사용하면 다양한 컴퓨팅 솔루션을 민첩하게 배포할 수 있습니다. Azure Virtual Machines를 사용하여 즉각적으로 배포하고 분 단위로 요금을 지불할 수 있습니다. Windows, Linux, SQL Server, Oracle, IBM, SAP, BizTalk에 대한 지원을 사용하여 거의 모든 운영 체제에서 워크로드를 모든 언어로 배포할 수 있습니다. 이러한 혜택으로 운영 비용을 절감하기 위해 Azure에 레거시 애플리케이션 배포된 온-프레미스를 마이그레이션할 수 있습니다.

온-프레미스 애플리케이션을 Azure에 마이그레이션하는 중요한 측면은 이러한 애플리케이션의 ID 요구를 처리하는 것입니다. 디렉터리 인식 애플리케이션은 회사 디렉터리에 대한 읽기 또는 쓰기 액세스를 LDAP에 의존하거나 Windows 통합 인증(Kerberos 또는 NTLM 인증)에 의존하여 최종 사용자를 인증합니다. Windows 서버에서 실행하는 LOB(기간 업무)는 일반적으로 도메인 가입된 머신에 배포되므로 그룹 정책을 사용하여 안전하게 관리할 수 있습니다. 온-프레미스 애플리케이션을 클라우드에 '들어올려서 옮기기' 위해 회사 ID 인프라에 대한 이러한 종속성을 해결해야 합니다.

Azure에 배포된 해당 애플리케이션의 ID 요구를 충족하기 위해 관리자는 종종 다음 솔루션 중 하나로 돌아갑니다.

* Azure 인프라 서비스 및 회사 디렉터리 온-프레미스에서 실행되는 워크로드 간에 사이트 간 VPN 연결을 배포합니다.
* Azure 가상 머신을 사용하여 복제 도메인 컨트롤러를 설치하여 회사 AD 도메인/포리스트 인프라를 확장합니다.
* Azure 가상 머신으로 배포된 도메인 컨트롤러를 사용하여 Azure에서 독립 실행형 도메인을 배포합니다.

이러한 모든 방법은 높은 비용 및 관리 오버헤드로 성능이 크게 저하됩니다. 관리자는 Azure 가상 머신을 사용하여 도메인 컨트롤러를 배포해야 합니다. 또한 이러한 가상 머신을 관리, 보안, 패치, 모니터링, 백업하고 문제를 해결해야 합니다. 온-프레미스 디렉터리에 대한 VPN 연결에 대한 의존은 Azure에서 배포된 워크로드를 일시적인 네트워크 결함 또는 중단에 취약하게 만듭니다. 이러한 네트워크 중단으로 이러한 애플리케이션에 대한 가동 시간이 더 낮아지고 안정성이 감소하게 됩니다.

더욱 쉽게 대안을 제공하도록 Azure AD 도메인 서비스를 설계했습니다.

### <a name="watch-an-introductory-video"></a>소개 비디오 보기

>[!VIDEO https://www.youtube.com/embed/T1Nd9APNceQ]

## <a name="introducing-azure-ad-domain-services"></a>Azure AD 도메인 서비스 소개

Azure AD 도메인 서비스는 도메인 가입, 그룹 정책, LDAP, Kerberos/NTLM 인증 등과 같이 Windows Server Active Directory와 완전히 호환되는 관리된 도메인 서비스를 제공합니다. 클라우드에서 도메인 컨트롤러를 배포, 관리 및 패치할 필요 없이 이러한 도메인 서비스를 사용할 수 있습니다. Azure AD 도메인 서비스는 기존 Azure AD 테넌트와 통합하므로 사용자가 회사 자격 증명을 사용하여 로그인할 수 있도록 합니다. 또한 기존 그룹 및 사용자 계정을 사용하여 리소스에 대한 액세스를 보호하므로 Azure 인프라 서비스에 온-프레미스 리소스를 원활하게 '들어올려서 옮길' 수 있습니다.

Azure AD 도메인 서비스 기능은 Azure AD 테넌트가 클라우드 전용인지 온-프레미스 Active Directory와 동기화되는지 여부에 관계 없이 원활하게 작동합니다.

### <a name="azure-ad-domain-services-for-cloud-only-organizations"></a>클라우드 전용 조직을 위한 Azure AD 도메인 서비스

클라우드 전용 Azure AD 테넌트('관리된 테넌트' 라고도 함)에는 온-프레미스 ID 공간이 없습니다. 즉, 사용자 계정, 해당 암호 및 그룹 멤버 자격은 클라우드에 모두 기본입니다. 즉, Azure AD에서 만들어지고 관리됩니다. Contoso가 클라우드 전용 Azure AD 테넌트인지 잠시 동안 고려합니다. 다음 그림에서 보여주는 대로 Contoso의 관리자는 Azure 인프라 서비스에서 가상 네트워크를 구성했습니다. 애플리케이션 및 서버 워크로드는 Azure 가상 머신의 해당 가상 네트워크에 배포됩니다. Contoso는 클라우드 전용 테넌트이므로 모든 사용자 ID, 자격 증명 및 그룹 멤버 자격이 Azure AD에서 생성되고 관리됩니다.

![Azure AD 도메인 서비스 개요](./media/active-directory-domain-services-overview/aadds-overview.png)

Contoso의 IT 관리자는 Azure AD 테넌트에 대해 Azure AD 도메인 서비스를 사용할 수 있고 해당 가상 네트워크에서 사용 가능한 도메인 서비스를 만들도록 선택할 수 있습니다. 이후 Azure AD 도메인 서비스는 관리된 도메인을 프로비전하고 가상 네트워크에서 사용할 수 있도록 합니다. Contoso의 Azure AD 테넌트에서 사용할 수 있는 모든 사용자 계정, 그룹 멤버 자격 및 사용자 자격 증명은 이 새로 만든 도메인에서도 사용할 수 있습니다. 이 기능을 사용하면 예를 들어 원격 데스크톱을 통해 도메인에 가입된 컴퓨터에 원격으로 연결할 때, 조직의 사용자가 회사 자격 증명을 사용하여 도메인에 로그인할 수 있습니다. 관리자는 기존 그룹 멤버 자격을 사용하여 도메인의 리소스에 대한 액세스를 프로비전할 수 있습니다. 가상 네트워크의 가상 머신에 배포된 애플리케이션은 도메인 가입, LDAP 읽기, LDAP 바인딩, NTLM 및 Kerberos 인증, 그룹 정책과 같은 기능을 사용할 수 있습니다.

Azure AD 도메인 서비스에서 프로비전되는 관리된 도메인의 두드러진 측면은 다음과 같습니다.

* Contoso의 IT 관리자는 관리된 도메인에 대한 해당 도메인 또는 어떤 도메인 컨트롤러도 관리, 패치 또는 모니터링할 필요가 없습니다.
* 해당 도메인에 AD 복제를 관리할 필요가 없습니다. Contoso의 Azure AD 테넌트에서 사용자 계정, 그룹 멤버 자격 및 자격 증명은 자동으로 관리된 도메인 내에서 사용할 수 있습니다.
* 도메인이 Azure AD 도메인 서비스에서 관리되기 때문에 Contoso의 IT 관리자는 해당 도메인에 도메인 관리자 또는 엔터프라이즈 관리자 권한이 없습니다.

### <a name="azure-ad-domain-services-for-hybrid-organizations"></a>하이브리드 조직을 위한 Azure AD 도메인 서비스
하이브리드 IT 인프라를 가진 조직은 클라우드 리소스 및 온-프레미스 리소스를 혼합하여 사용합니다. 이러한 조직에서는 해당 온-프레미스 디렉터리에서 Azure AD 테넌트에 ID 정보를 동기화합니다. 하이브리드 조직에서는 온-프레미스 애플리케이션, 특히 레거시 디렉터리 인식 애플리케이션을 클라우드에 더 마이그레이션할 방법을 찾기 때문에 Azure AD 도메인 서비스는 유용할 수 있습니다.

Litware Corporation은 해당 온-프레미스 디렉터리에서 Azure AD 테넌트에 ID 정보를 동기화하기 위해 [Azure AD Connect](../active-directory/hybrid/whatis-hybrid-identity.md)를 배포했습니다. 동기화되는 ID 정보에는 사용자 계정, 인증을 위한 자격 증명 해시(암호 해시 동기화) 및 그룹 멤버 자격이 포함됩니다.

> [!NOTE]
> **Azure AD Domain Services를 사용하려는 하이브리드 조직은 암호 해시 동기화를 반드시 수행해야 합니다**. 이러한 요구 사항은 NTLM 또는 Kerberos 인증 방법을 통해 이러한 사용자를 인증하기 위해 Azure AD 도메인 서비스에서 제공하는 관리된 도메인에서 사용자의 자격 증명이 필요하기 때문입니다.
>
>

![Litware Corporation에 대한 Azure AD 도메인 서비스](./media/active-directory-domain-services-overview/aadds-overview-synced-tenant.png)

이전 그림에서는 Litware Corporation과 같이 하이브리드 IT 인프라를 가진 조직이 Azure AD 도메인 서비스를 사용하는 방법을 보여 줍니다. 도메인 서비스를 필요로 하는 Litware의 애플리케이션 및 서버 워크로드는 Azure 인프라 서비스의 가상 네트워크에 배포됩니다. Litware의 IT 관리자는 Azure AD 테넌트에 대해 Azure AD 도메인 서비스를 사용할 수 있고 해당 가상 네트워크에서 사용 가능한 관리된 도메인을 만들도록 선택할 수 있습니다. Litware가 하이브리드 IT 인프라를 가진 조직이므로 사용자 계정, 그룹 및 자격 증명은 해당 온-프레미스 디렉터리에서 Azure AD 테넌트에 동기화됩니다. 이 기능을 사용하면 예를 들어 원격 데스크톱을 통해 도메인에 가입된 컴퓨터에 원격으로 연결할 때, 사용자가 회사 자격 증명을 사용하여 도메인에 로그인할 수 있습니다. 관리자는 기존 그룹 멤버 자격을 사용하여 도메인의 리소스에 대한 액세스를 프로비전할 수 있습니다. 가상 네트워크의 가상 머신에 배포된 애플리케이션은 도메인 가입, LDAP 읽기, LDAP 바인딩, NTLM 및 Kerberos 인증, 그룹 정책과 같은 기능을 사용할 수 있습니다.

Azure AD 도메인 서비스에서 프로비전되는 관리된 도메인의 두드러진 측면은 다음과 같습니다.

* 관리된 도메인은 독립 실행형 도메인입니다. Litware의 온-프레미스 도메인의 확장이 아닙니다.
* Litware의 IT 관리자는 이 관리된 도메인에 대한 도메인 컨트롤러를 관리, 패치 또는 모니터링할 필요가 없습니다.
* 해당 도메인에 AD 복제를 관리할 필요가 없습니다. Litware의 온-프레미스 디렉터리에서 사용자 계정, 그룹 멤버 자격 및 자격 증명은 Azure AD Connect를 통해 Azure AD로 동기화됩니다. 이러한 사용자 계정, 그룹 멤버 자격 및 자격 증명은 자동으로 관리된 도메인 내에서 사용할 수 있습니다.
* 도메인이 Azure AD 도메인 서비스에서 관리되기 때문에 Litware의 IT 관리자는 해당 도메인에 도메인 관리자 또는 엔터프라이즈 관리자 권한이 없습니다.

## <a name="benefits"></a>이점
Azure AD 도메인 서비스를 사용하여 다음과 같은 이점을 누릴 수 있습니다.

* **간편함** – 몇 번 간단히 클릭하여 Azure 인프라 서비스에 배포된 가상 머신의 ID 요구를 충족할 수 있습니다. Azure에서 ID 인프라를 배포하고 관리하거나 온-프레미스 ID 인프라에 다시 연결을 설치할 필요가 없습니다.
* **통합됨** – Azure AD 도메인 서비스는 Azure AD 테넌트와 밀접하게 통합됩니다. 이제 최신 애플리케이션 및 기존의 디렉터리 인식 애플리케이션의 요구를 충족하는 통합된 클라우드 기반 엔터프라이즈 디렉터리로 Azure AD을 사용할 수 있습니다.
* **호환 가능** – Azure AD 도메인 서비스는 Windows Server Active Directory의 입증된 엔터프라이즈 등급 인프라에서 빌드되었습니다. 따라서 애플리케이션은 Windows Server Active Directory 기능과 높은 수준으로 호환될 수 있습니다. Windows Server AD에서 사용할 수 있는 모든 기능을 현재 Azure AD 도메인 서비스에서 사용할 수 있습니다. 그러나 사용할 수 있는 기능은 온-프레미스 인프라에 의존하는 해당 Windows Server AD 기능과 호환 가능합니다. LDAP, Kerberos, NTLM, 그룹 정책 및 도메인 가입 기능은 다양한 Windows Server 릴리스에 대해 테스트되고 구체화되는 완성도 높은 제품을 구성합니다.
* **비용 효율적** – Azure AD 도메인 서비스를 사용하면 기존의 디렉터리 인식 애플리케이션을 지원하기 위해 ID 인프라를 관리와 연관된 인프라 및 관리 부담을 방지할 수 있습니다. 이러한 애플리케이션을 Azure 인프라 서비스로 이동할 수 있으며 운영 비용 절감의 이점을 얻을 수 있습니다.


## <a name="next-steps"></a>다음 단계
### <a name="learn-more-about-azure-ad-domain-services"></a>Azure AD Domain Services에 대한 자세한 정보
* [기능](active-directory-ds-features.md)
* [배포 시나리오](scenarios.md)
* [Azure AD Domain Services가 사용 사례에 적합한 경우 찾기](comparison.md)
* [Azure AD Domain Services를 Azure AD 디렉터리와 동기화하는 방법 이해](synchronization.md)

### <a name="get-started-with-azure-ad-domain-services"></a>Azure AD Domain Services 시작
* [Azure Portal을 사용하여 Azure AD Domain Services 활성화](create-instance.md)
