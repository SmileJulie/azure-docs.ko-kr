---
title: Advanced Threat Protection - Azure Database for PostgreSQL | Microsoft Docs
description: Advanced Threat Protection은 데이터베이스에 대한 잠재적인 보안 위협을 나타내는 비정상적인 데이터베이스 활동을 검색합니다.
services: postgresql
author: bolzmj
manager: kfile
ms.service: postgresql
ms.topic: article
ms.date: 09/20/2018
ms.author: mbolz
ms.openlocfilehash: 392947919813753dc57422957029e961a037a616
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49066941"
---
# <a name="azure-database-for-postgresql-advanced-threat-protection"></a>Azure Database for PostgreSQL Advanced Threat Protection

Azure Database for PostgreSQL용 Advanced Threat Protection은 비정상적이며 유해할 가능성이 있는 데이터베이스 액세스 또는 악용 시도를 나타내는 비정상적인 활동을 검색합니다.

Threat Detection은 고급 보안 기능용 통합 패키지인 ATP(Advanced Threat Protection) 제품의 일부입니다. [Azure Portal](https://portal.azure.com)을 통해 액세스 및 관리할 수 있는 Advanced Threat Protection은 현재 미리 보기로 제공됩니다.

> [!NOTE]
> Advanced Threat Protection 기능을 사용할 수 **없는** Azure 정부 및 소버린 클라우드 지역은 US Gov 텍사스, US Gov 애리조나, US Gov 아이오와, US Gov 버지니아, US DoD 동부, US DoD 중부, 독일 중부, 독일 북부, 중국 동부, 중국 동부 2입니다. 전반적인 제품 사용 가능성을 확인하려면 [지역별 사용 가능한 제품](https://azure.microsoft.com/global-infrastructure/services/)을 참조하세요.
>

## <a name="what-is-advanced-threat-protection"></a>Advanced Threat Protection 소개

Azure Database for PostgreSQL용 Advanced Threat Protection은 비정상적인 활동에 대한 보안 경고를 제공하여 잠재적인 위협이 발생하면 고객이 이를 검색하고 대응할 수 있도록 하는 새로운 차원의 보안을 제공합니다. 사용자는 의심스러운 데이터베이스 활동, 잠재적 취약성, 그리고 비정상적인 데이터베이스 액세스 및 쿼리 패턴에 대한 경고를 받습니다. Azure Database for PostgreSQL용 Advanced Threat Detection은 의심스러운 활동에 대한 세부 정보와 위협을 조사하고 완화하는 방법에 대한 권장 작업을 포함한 경고를 [Azure Security Center](https://azure.microsoft.com/services/security-center/)와 통합합니다. Azure Database for PostgreSQL용 Advanced Threat Detection을 사용하면 보안 전문가나 고급 보안 모니터링 시스템 관리자가 아니더라도 데이터베이스에 대한 잠재적 위협을 간편하게 해결할 수 있습니다. 

![Advanced Threat Protection 개념](media/concepts-data-access-and-security-threat-protection/advanced-threat-protection-concept.png)

## <a name="advanced-threat-protection-alerts"></a>Advanced Threat Protection 경고 
Azure Database for PostgreSQL용 Advanced Threat Protection은 비정상적이며 유해할 가능성이 있는 데이터베이스 액세스 또는 악용 시도를 나타내는 비정상적인 활동을 검색하며, 다음 경고를 트리거할 수 있습니다.
- **비정상적인 위치에서 액세스**: 비정상적인 지리적 위치에서 누군가가 Azure Database for PostgreSQL 서버에 로그온하여 해당 서버의 액세스 패턴이 변경되는 경우 이 경고가 트리거됩니다. 일부 경우에서 경고는 합법적인 작업(새 응용 프로그램 또는 개발자 유지 관리)을 검색합니다. 다른 경우에서 경고는 악의적인 작업(퇴사 직원, 외부 공격자)을 검색합니다.
- **비정상적인 Azure 데이터 센터에서 액세스**: 최근 Azure Database for PostgreSQL 서버에 표시된 비정상적인 Azure 데이터 센터에서 누군가가 해당 서버에 로그온하여 서버의 액세스 패턴이 변경되는 경우 이 경고가 트리거됩니다. 경고가 Azure, Power BI, Azure Database for PostgreSQL Query Editor의 새 응용 프로그램이 수행하는 합법적 작업을 검색하는 경우도 있습니다. 다른 경우에서 경고는 Azure 리소스/서비스의 악의적인 작업(퇴사 직원, 외부 공격자)을 검색합니다.
- **알 수 없는 보안 주체의 액세스**: 비정상적인 보안 주체(Azure Database for PostgreSQL 사용자)를 사용해 누군가가 Azure Database for PostgreSQL 서버에 로그온하여 해당 서버의 액세스 패턴이 변경되는 경우 이 경고가 트리거됩니다. 일부 경우에서 경고는 합법적인 작업(새 응용 프로그램, 개발자 유지 관리)을 검색합니다. 다른 경우에서 경고는 악의적인 작업(퇴사 직원, 외부 공격자)을 검색합니다.
- **잠재적으로 위험한 응용 프로그램에서 액세스**: 잠재적으로 위험한 응용 프로그램이 데이터베이스에 액세스하는 데 사용되는 경우 이 경고가 트리거됩니다. 일부 경우에서 경고는 작업의 침투 테스트를 검색합니다. 다른 경우에서 경고는 일반적인 공격 도구를 사용하여 공격을 검색합니다.
- **무차별 암호 대입 Azure Database for PostgreSQL 자격 증명**: 각기 다른 자격 증명을 사용하는 로그인이 비정상적으로 많이 실패하면 이 경고가 트리거됩니다. 일부 경우에서 경고는 작업의 침투 테스트를 검색합니다. 다른 경우에서 경고는 무차별 암호 대입 공격(brute force attack)을 검색합니다.

## <a name="next-steps"></a>다음 단계

* [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)에 대한 자세한 정보
* 가격 책정에 대한 자세한 내용은 [Azure Database for PostgreSQL 가격 책정 페이지](https://azure.microsoft.com/pricing/details/postgresql/)를 참조하세요. 
* Azure Portal을 사용하여 [Azure Database for PostgreSQL Advanced Threat Protection](howto-database-threat-protection-portal.md) 구성  