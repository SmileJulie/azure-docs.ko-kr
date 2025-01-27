---
title: 'Azure Active Directory Domain Services: 배포 시나리오 | Microsoft Docs'
description: Azure AD 도메인 서비스용 배포 시나리오
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: c5216ec9-4c4f-4b7e-830b-9d70cf176b20
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/21/2017
ms.author: iainfou
ms.openlocfilehash: 9e8ac5e83fa3cc9b1e266a9009bad86f41233ed8
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67472660"
---
# <a name="deployment-scenarios-and-use-cases"></a>배포 시나리오 및 사용 사례
이 섹션에서는 Azure AD(Active Directory) 도메인 서비스에서 이익이 되는 몇 가지 시나리오 및 사용 사례를 살펴보겠습니다.

## <a name="secure-easy-administration-of-azure-virtual-machines"></a>Azure 가상 머신의 안전하고 손쉬운 관리
Azure Active Directory Domain Services를 사용하여 효율적인 방식으로 Azure 가상 머신을 관리할 수 있습니다. Azure 가상 머신을 관리되는 도메인에 가입할 수 있으므로 회사 AD 자격 증명을 사용하여 로그인 할 수 있습니다. 이 방법은 각 Azure 가상 머신에서 로컬 관리자 계정 유지 관리와 같은 복잡한 자격 증명 관리 과정을 방지하도록 돕습니다.

그룹 정책을 사용하여 관리되는 도메인에 가입된 서버 가상 머신을 관리하고 보호할 수 있습니다. Azure 가상 머신에 필요한 보안 기준을 적용할 수 있으며 회사 보안 지침에 따라 잠글 수 있습니다. 예를 들어 그룹 정책 관리 기능을 사용하여 이러한 가상 머신에서 시작될 수 있는 애플리케이션 종류를 제한할 수 있습니다.

![Azure 가상 머신의 간소화된 관리](./media/active-directory-domain-services-scenarios/streamlined-vm-administration.png)

서버 및 기타 인프라가 수명 종료에 도달하면 Contoso는 현재 호스팅되는 온-프레미스에서 클라우드로 애플리케이션 다수를 이동시킵니다. 현재 해당 IT 표준은 기업 애플리케이션을 호스팅하는 서버가 그룹 정책을 사용하여 도메인에 가입하고 관리되어야 한다고 규정합니다. Contoso의 IT 관리자는 보다 쉽게 관리하기 위해 Azure에 배포된 도메인 가입 가상 머신을 선호합니다. 결과적으로, 관리자 및 사용자는 회사 자격 증명을 사용하여 로그인할 수 있습니다. 동시에 그룹 정책을 사용하여 필요한 보안 기준을 준수하도록 컴퓨터를 구성할 수 있습니다. Contoso는 Azure 가상 머신을 보호하기 위해 Azure에서 도메인 컨트롤러를 배포, 모니터링 및 관리하지 않는 편을 선호합니다. 따라서 Azure AD 도메인 서비스는 이 사용 사례에 대한 최적의 선택입니다.

**배포 참고 사항**

이 배포 시나리오에 대한 다음 중요 사항을 고려하십시오.

* Azure AD 도메인 서비스에서 제공하는 관리되는 도메인은 단일 플랫 OU(조직 구성 단위) 구조를 기본으로 제공합니다. 모든 도메인에 가입된 컴퓨터는 단일 플랫 OU에 상주합니다. 그러나 사용자 지정 OU를 만들도록 선택할 수 있습니다.
* Azure AD 도메인 서비스는 각 사용자 및 컴퓨터 컨테이너에 기본 제공 GPO 형태로 단순한 그룹 정책을 지원합니다. 사용자 지정 GPO를 만들 수 있으며 사용자 지정 OU로 지정할 수 있습니다.
* Azure AD 도메인 서비스는 기본 AD 컴퓨터 개체 스키마를 지원합니다. 컴퓨터 개체의 스키마를 확장할 수 없습니다.

## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-bind-authentication-to-azure-infrastructure-services"></a>LDAP 바인딩 인증을 사용하는 온-프레미스 애플리케이션을 Azure 인프라 서비스에 들어올려서 옮기기
![LDAP 바인딩](./media/active-directory-domain-services-scenarios/ldap-bind.png)

Contoso에는 ISV에서 몇 년 전에 구입한 온-프레미스 애플리케이션이 있습니다. 애플리케이션은 현재 ISV에서 유지 관리 모드에 있으며 애플리케이션에 대한 변경 요청은 Contoso에 엄청나게 많은 비용이 듭니다. 애플리케이션은 웹 양식을 사용하여 사용자 자격 증명을 수집하는 웹 기반 프런트 엔드를 가지고 회사 Active Directory에 LDAP 바인딩을 수행하여 사용자를 인증합니다. Contoso는 Azure 인프라 서비스에 해당 애플리케이션을 마이그레이션하려고 합니다. 애플리케이션을 변경할 필요 없이 있는 그대로 작동하는 것이 좋습니다. 또한 사용자는 기존 회사 자격 증명을 사용하며 작업을 다르게 할 수 있도록 다시 학습하지 않고 인증할 수 있어야 합니다. 즉, 최종 사용자는 애플리케이션이 실행되고 있는 곳을 감지하지 못해야 하고 마이그레이션은 투명해야 합니다.

**배포 참고 사항**

이 배포 시나리오에 대한 다음 중요 사항을 고려하십시오.

* 애플리케이션이 디렉터리에 수정/쓰기를 하지 않아도 됩니다. Azure AD 도메인 서비스에서 제공하는 관리된 도메인에 대한 LDAP 쓰기 액세스는 지원되지 않습니다.
* 사용자는 관리되는 도메인에 대해 직접 암호를 변경할 수 없습니다. 최종 사용자가 Azure AD의 셀프 서비스 암호 변경 메커니즘을 사용하거나 온-프레미스 디렉터리에 대해 암호를 바꿀 수 있습니다. 이러한 변경은 관리되는 도메인에서 자동으로 동기화되고 사용할 수 있습니다.

## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-read-to-access-the-directory-to-azure-infrastructure-services"></a>디렉터리에 액세스하는 데 LDAP 읽기를 사용하는 온-프레미스 애플리케이션을 Azure 인프라 서비스에 들어올려서 옮기기
Contoso에는 거의 10년 전에 개발된 온-프레미스 LOB(기간 업무) 애플리케이션이 있습니다. 해당 애플리케이션은 디렉터리를 인식하고 Windows Server AD로 작업하도록 설계되었습니다. 애플리케이션은 LDAP(Lightweight Directory Access Protocol)를 사용하여 Active Directory에서 사용자에 대한 정보/특성을 읽습니다. 애플리케이션은 특성을 수정하지 않거나 그렇지 않은 경우 디렉터리에 쓰지 않습니다. Contoso는 Azure 인프라 서비스에 해당 애플리케이션을 마이그레이션하고 현재 해당 애플리케이션을 호스팅하는 오래된 온-프레미스 하드웨어의 사용을 중지하려고 합니다. 애플리케이션은 REST 기반 Azure AD Graph API와 같은 최신 디렉터리 API를 사용하기 위해 다시 작성할 수 없습니다. 따라서 코드를 수정하거나 애플리케이션을 다시 작성하지 않고 클라우드에서 실행하기 위해 애플리케이션을 마이그레이션할 수 있는 들어올려서 옮기기 옵션이 필요합니다.

**배포 참고 사항**

이 배포 시나리오에 대한 다음 중요 사항을 고려하십시오.

* 애플리케이션이 디렉터리에 수정/쓰기를 하지 않아도 됩니다. Azure AD 도메인 서비스에서 제공하는 관리된 도메인에 대한 LDAP 쓰기 액세스는 지원되지 않습니다.
* 애플리케이션에 사용자 지정/확장된 Active Directory 스키마가 필요하지 않습니다. 스키마 확장은 Azure AD 도메인 서비스에서 지원되지 않습니다.

## <a name="migrate-an-on-premises-service-or-daemon-application-to-azure-infrastructure-services"></a>Azure 인프라 서비스에 온-프레미스 서비스 또는 데몬 애플리케이션 마이그레이션
계층 중 하나가 데이터베이스 계층과 같은 백 엔드 계층에 대해 인증된 호출을 수행해야 하는 여러 계층으로 구성된 일부 애플리케이션 Active Directory 서비스 계정은 이러한 사용 사례에 대해 자주 사용됩니다. 이러한 애플리케이션을 Azure 인프라 서비스로 전환하고 이러한 애플리케이션의 ID 필요에 대해 Azure AD 도메인 서비스를 사용할 수 있습니다. 온-프레미스 디렉터리에서 Azure AD로 동기화되는 동일한 서비스 계정을 사용하도록 선택할 수 있습니다. 또는 먼저 사용자 지정 OU를 만든 다음 이러한 애플리케이션을 배포하도록 해당 OU에 별도 서비스 계정을 만들 수 있습니다.

![WIA를 사용하는 서비스 계정](./media/active-directory-domain-services-scenarios/wia-service-account.png)

Contoso에는 웹 프런트 엔드, SQL server 및 백 엔드 FTP 서버를 포함하는 사용자 지정 소프트웨어 자격 증명 모음 애플리케이션이 있습니다. FTP 서버에 웹 프런트 엔드를 인증하는 데 서비스 계정의 Windows 통합된 인증이 사용됩니다. 웹 프런트 엔드는 서비스 계정으로 실행되도록 설정됩니다. 백 엔드 서버는 웹 프런트 엔드에 대한 서비스 계정에서 액세스 권한을 부여하도록 구성됩니다. Contoso는 이 애플리케이션을 Azure 인프라 서비스로 이동하기 위해 클라우드에서 도메인 컨트롤러 가상 머신을 배포하지 않아도 되는 것을 선호합니다. Contoso의 IT 관리자는 Azure 가상 머신에 웹 프런트 엔드, SQL Server 및 FTP 서버를 호스팅하는 서버를 배포할 수 있습니다. 그런 다음 이러한 컴퓨터는 Azure AD 도메인 서비스 관리되는 도메인에 가입됩니다. 그런 다음 앱의 인증을 위해 해당 온-프레미스 디렉터리에 동일한 서비스 계정을 사용할 수 있습니다. 이 서비스 계정은 Azure AD 도메인 서비스 관리되는 도메인으로 동기화되고 사용할 수 있습니다.

**배포 참고 사항**

이 배포 시나리오에 대한 다음 중요 사항을 고려하십시오.

* 애플리케이션이 인증에 사용자 이름/암호를 사용합니다. Azure AD 도메인 서비스에서 인증서/스마트 카드 기반 인증을 지원하지 않습니다.
* 사용자는 관리되는 도메인에 대해 직접 암호를 변경할 수 없습니다. 최종 사용자가 Azure AD의 셀프 서비스 암호 변경 메커니즘을 사용하거나 온-프레미스 디렉터리에 대해 암호를 바꿀 수 있습니다. 이러한 변경은 관리되는 도메인에서 자동으로 동기화되고 사용할 수 있습니다.

## <a name="windows-server-remote-desktop-services-deployments-in-azure"></a>Azure에서 Windows Server 원격 데스크톱 서비스 배포
Azure AD Domain Services를 사용하여 Azure에 배포된 원격 데스크톱 서버에 관리되는 AD 도메인 서비스를 제공할 수 있습니다.

이 배포 시나리오에 대한 자세한 내용은 [RDS 배포와 Azure AD Domain Services를 통합](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-azure-adds)하는 방법을 참조하세요.


## <a name="domain-joined-hdinsight-clusters-preview"></a>도메인에 가입된 HDInsight 클러스터(미리 보기)
Apache Ranger를 사용하여 Azure AD Domain Services 관리되는 도메인에 가입된 Azure HDInsight 클러스터를 설정할 수 있습니다. Apache Ranger를 통해 Hive 정책을 생성 및 적용하고, 사용자(예: 데이터 과학자)가 Excel, Tableau 등의 ODBC 기반 도구를 사용하여 Hive에 연결할 수 있도록 허용합니다. Microsoft는 조만간 HBase, Spark, Storm 등의 다른 워크로드를 도메인에 가입된 HDInsight에 추가하기 위해 준비 중입니다.

이 배포 시나리오에 대한 자세한 내용은 [도메인에 가입된 HDInsight 클러스터를 구성](../hdinsight/domain-joined/apache-domain-joined-configure.md)하는 방법을 참조하세요.
