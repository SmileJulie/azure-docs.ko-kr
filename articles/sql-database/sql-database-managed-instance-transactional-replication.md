---
title: Azure SQL 논리 서버 및 Azure SQL Managed Instance를 사용하여 트랜잭션 복제 | Microsoft Docs "
description: Azure SQL Database 논리 서버 및 SQL Managed Instance에서 SQL Server 트랜잭션 복제를 사용하는 방법 알아보기
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: carlrab
manager: craigg
ms.date: 01/08/2019
ms.openlocfilehash: d94173f9b1940613c26451658b90c956c71876fb
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54353246"
---
# <a name="transactional-replication-with-azure-sql-logical-server-and-azure-sql-managed-instance"></a>Azure SQL 논리 서버 및 Azure SQL Managed Instance를 사용하여 트랜잭션 복제

트랜잭션 복제는 Azure SQL Database의 테이블에서 원격 데이터베이스에 있는 테이블로 데이터를 복제할 수 있도록 하는 Azure SQL Database, Managed Instance 및 SQL Server의 기능입니다. 이 기능을 사용하면 서로 다른 데이터베이스의 여러 테이블을 동기화할 수 있습니다.

## <a name="when-to-use-transactional-replication"></a>트랜잭션 복제를 사용하는 경우

트랜잭션 복제는 다음과 같은 시나리오에서 유용합니다.
- 게시를 수행하면 데이터베이스에 있는 하나 이상의 테이블에서 변경이 수행된 후 변경을 위해 구독되는 하나 이상의 SQL Server 또는 Azure SQL Datrabase로 배포됩니다.
- 여러 개의 분산된 데이터베이스를 동기화된 상태로 유지합니다.
- 변경 내용을 지속적으로 게시하여 SQL Server 또는 Managed Instance의 데이터베이스를 다른 데이터베이스로 마이그레이션합니다.

## <a name="overview"></a>개요 
트랜잭션 복제의 주요 구성 요소는 다음 그림에 나와 있습니다.  

![SQL Database를 사용한 복제](media/replication-to-sql-database/replication-to-sql-database.png)


**게시자**는 배포자에게 업데이트를 전송하여 일부 테이블(문서)에 대해 변경된 내용을 게시하는 인스턴스 또는 서버입니다. 다음 버전의 SQL Server에서는 온-프레미스 SQL Server에서 Azure SQL Database로 게시할 수 있습니다.
    - SQL Server 2019(미리 보기)
    - SQL Server 2016 ~ SQL 2017
    - SQL Server 2014 SP1 CU3 이상(12.00.4427)
    - SQL Server 2014 RTM CU10(12.00.2556)
    - SQL Server 2012 SP3 이상(11.0.6020)
    - SQL Server 2012 SP2 CU8(11.0.5634.0)
    - Azure에서 게시 개체를 지원하지 않는 다른 버전의 SQL Server에서는 [데이터 다시 게시](https://docs.microsoft.com/sql/relational-databases/replication/republish-data) 방법을 사용하여 데이터를 최신 버전의 SQL Server로 이동할 수 있습니다. 

**배포자**는 게시자에서 문서의 변경 내용을 수집하고 구독자에게 배포하는 인스턴스 또는 서버입니다. 배포자는 Azure SQL Database Managed Instance 또는 SQL Server(게시자 버전보다 같거나 높은 모든 버전)일 수 있습니다. 

**구독자**는 게시자에 대한 변경 내용을 수신하는 인스턴스 또는 서버입니다. 구독자는 Azure SQL Database 논리 서버/Managed Instance 또는 SQL Server일 수 있습니다. 논리 서버의 구독자는 푸시 구독자로 구성해야 합니다. 

| 역할 | 논리 서버 | 관리되는 인스턴스 |
| :----| :------------- | :--------------- |
| **게시자** | 아니요 | 예 | 
| **배포자** | 아니요 | 예|
| **끌어오기 구독자** | 아니요 | 예|
| **밀어넣기 구독자**| 예 | 예|
| &nbsp; | &nbsp; | &nbsp; |

다음과 같은 여러 [복제 유형](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication?view=sql-server-2017)이 있습니다.


| 복제 | 논리 서버 | 관리되는 인스턴스 |
| :----| :------------- | :--------------- |
| [**트랜잭션**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/transactional-replication) | 예(구독자로) | 예 | 
| [**스냅숏**](https://docs.microsoft.com/sql/relational-databases/replication/snapshot-replication) | 예(구독자로) | 예|
| [**병합 복제**](https://docs.microsoft.com/sql/relational-databases/replication/merge/merge-replication) | 아니요 | 아니요|
| [**피어-투-피어**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/peer-to-peer-transactional-replication) | 아니요 | 아니요|
| **단방향** | 예 | 예|
| [**양방향**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/bidirectional-transactional-replication) | 아니요 | 예|
| [**업데이트할 수 있는 구독**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication) | 아니요 | 아니요|
| &nbsp; | &nbsp; | &nbsp; |

  >[!NOTE]
  > - 이전 버전을 사용하여 복제를 구성하는 시도는 오류 번호 MSSQL_REPL20084(프로세스가 구독자에 연결할 수 없습니다.) 및 MSSQ_REPL40532(로그인에서 요청한 서버 <name>을 열 수 없습니다. 로그인이 실패했습니다.)가 발생할 수 있습니다.
  > - Azure SQL Database의 모든 기능을 사용하려면 최신 버전의 [SSMS(SQL Server Management Studio)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017) 및 [SSDT(SQL Server Data Tools)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017)를 사용해야 합니다.

## <a name="requirements"></a>요구 사항
- 연결은 복제 참가자 간에 SQL 인증을 사용합니다. 
- 복제에 사용되는 작업 디렉터리의 Azure Storage 계정 공유 
- Azure 파일 공유에 액세스하려면 관리되는 Managed Instance 서브넷의 보안 규칙에서 포트 445(TCP 아웃바운드)를 열어야 합니다. 
- 게시자/배포자가 Managed Instance에 있고 구독자가 온-프레미스에 있는 경우 포트 1433(TCP 아웃바운드)를 열어야 합니다. 

## <a name="common-configurations"></a>일반 구성
일반적으로 게시자와 배포자는 모두 클라우드 또는 온-프레미스에 있어야 합니다. 현재 지원되는 구성은 다음과 같습니다. 

### <a name="publisher-with-local-distributor-on-a-managed-instance"></a>관리되는 인스턴스에서 로컬 배포자로 게시자

![게시자 및 배포자로서의 단일 인스턴스 ](media/replication-with-sql-database-managed-instance/01-single-instance-asdbmi-pubdist.png)

게시자 및 배포자는 단일 Managed Instance 내에서 구성되고, 온-프레미스의 다른 Managed Instance, Single Database 또는 SQL Server로 변경 내용을 배포하고 있습니다. 이 구성에서 게시자/배포자 Managed Instance는 [지역에서 복제 및 자동 장애 조치(failover) 그룹](sql-database-auto-failover-group.md)으로 구성할 수 없습니다.

### <a name="publisher-with-remote-distributor-on-a-managed-instance"></a>Managed Instance에서 원격 배포자로 게시자

이 구성에서는 하나의 Managed Instance가 여러 원본 Managed Instance를 제공하고 Managed Instance, Single Database 또는 SQL Server에 있는 하나 또는 여러 대상으로 변경 내용을 배포할 수 있는 또 다른 Managed Instance에 배치된 배포자에게 변경 내용을 게시합니다.

![게시자 및 배포자에 대한 별도 인스턴스](media/replication-with-sql-database-managed-instance/02-separate-instances-asdbmi-pubdist.png)

게시자 및 배포자는 두 개의 Managed Instance에서 구성됩니다. 이 구성에서 다음이 적용됩니다.

- 두 Managed Instance가 동일한 vNet에 있습니다.
- 두 Managed Instance가 동일한 위치에 있습니다.
- 게시자 및 배포자 데이터베이스를 호스트하는 Managed Instance는 [자동 장애 조치(failover) 그룹을 사용하여 지리적으로 복제](sql-database-auto-failover-group.md)될 수 없습니다.

### <a name="publisher-and-distributor-on-premises-with-a-subscriber-on-a-managed-instance-or-logical-server"></a>Managed Instance 또는 논리 서버에 구독자가 있는 온-프레미스의 게시자 및 배포자 

![구독자로서의 Azure SQL DB](media/replication-with-sql-database-managed-instance/03-azure-sql-db-subscriber.png)
 
이 구성에서 Azure SQL Database(Managed Instance 또는 논리 서버)는 구독자입니다. 이 구성은 온-프레미스에서 Azure로의 마이그레이션을 지원합니다. 구독자가 논리 서버에 있으면 밀어넣기 모드여야 합니다.  

## <a name="next-steps"></a>다음 단계
1. [Managed Instance에 대해 트랜잭션 복제 구성](https://docs.microsoft.com/azure/sql-database/replication-with-sql-database-managed-instance#configure-publishing-and-distribution-example)합니다. 
1. [게시 만들기](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication)
1. 구독자로 Azure SQL Database 논리 서버 이름(예: `N'azuresqldbdns.database.windows.net`) 및 대상 데이터베이스로 Azure SQL Database 이름(예: **Adventureworks**)을 사용하여 [밀어넣기 구독을 만듭니다](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription). )


## <a name="see-also"></a>참고 항목  

- [SQL Database로 복제](replication-to-sql-database.md)
- [Managed Instance로 복제](replication-with-sql-database-managed-instance.md)
- [게시 만들기](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication)
- [밀어넣기 구독 만들기](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/)
- [복제 유형](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication)
- [모니터링(복제)](https://docs.microsoft.com/sql/relational-databases/replication/monitor/monitoring-replication)
- [구독 초기화](https://docs.microsoft.com/sql/relational-databases/replication/initialize-a-subscription)  