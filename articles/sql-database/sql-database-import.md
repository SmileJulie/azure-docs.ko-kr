---
title: BACPAC 파일을 가져와 Azure SQL 데이터베이스 만들기 | Microsoft Docs
description: BACPAC 파일을 가져와 새 Azure SQL 데이터베이스를 만듭니다.
services: sql-database
ms.service: sql-database
ms.subservice: migration
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 06/20/2019
ms.openlocfilehash: 5726a11d37899517674d445711afda31731b901d
ms.sourcegitcommit: 770b060438122f090ab90d81e3ff2f023455213b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68305826"
---
# <a name="quickstart-import-a-bacpac-file-to-a-database-in-azure-sql-database"></a>빠른 시작: Azure SQL Database의 데이터베이스로 BACPAC 파일 가져오기

[BACPAC](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications#bacpac) 파일을 사용하여 Azure SQL Database의 데이터베이스로 SQL Server 데이터베이스를 가져올 수 있습니다. Azure Blob 스토리지(표준 스토리지만 해당)에 저장된 `BACPAC` 파일에서 또는 온-프레미스 위치의 로컬 스토리지에서 데이터를 가져올 수 있습니다. 더 빠른 리소스를 더 많이 제공하여 가져오기 속도를 높이려면 가져오기 프로세스 중에 데이터베이스를 더 높은 서비스 계층 및 컴퓨팅 크기로 확장합니다. 그런 다음, 가져오기가 성공하면 크기를 축소할 수 있습니다.

> [!NOTE]
> 가져온 데이터베이스의 호환성 수준은 원본 데이터베이스의 호환성 수준에 기반합니다.
> [!IMPORTANT]
> 데이터베이스를 가져온 후에는 데이터베이스를 현재 호환성 수준에서(AdventureWorks2008R2 데이터베이스에 대해 수준 100) 또는 더 높은 수준에서 작동하도록 선택할 수 있습니다. 특정 호환성 수준에서 데이터베이스를 운영하기 위한 옵션 및 그 영향에 대한 자세한 내용은 [ALTER DATABASE 호환성 수준](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level)을 참조하세요. 또한 호환성 수준과 관련된 추가 데이터베이스 수준 설정에 대한 자세한 내용은 [ALTER DATABASE SCOPED CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql)을 참조하세요.

## <a name="import-from-a-bacpac-file-in-the-azure-portal"></a>Azure Portal의 BACPAC 파일에서 가져오기

[Azure Portal](https://portal.azure.com)은 Azure SQL Database에서 Azure Blob Storage에 저장된 BACPAC 파일을 통해 단일 데이터베이스를 만드는 기능*만* 지원합니다. 

Azure PowerShell를 사용 하 여 BACPAC 파일에서 관리 되는 [인스턴스로](sql-database-managed-instance.md) 데이터베이스를 마이그레이션하는 것은 현재 지원 되지 않습니다. 대신 SQL Server Management Studio 또는 SQLPackage를 사용 해야 합니다.

> [!NOTE]
> Azure Portal 또는 PowerShell을 통해 제출 된 가져오기/내보내기 요청을 처리 하는 컴퓨터는 BACPAC 파일 뿐만 아니라 데이터 계층 응용 프로그램 프레임 워크 (DacFX)에서 생성 된 임시 파일을 저장 해야 합니다. 필요한 디스크 공간은 크기가 같은 데이터베이스에 따라 크게 달라 지 며, 데이터베이스 크기의 최대 3 배까지 디스크 공간이 필요할 수 있습니다. Import/export 요청을 실행 하는 컴퓨터에는 450GB 로컬 디스크 공간만 있습니다. 따라서 일부 요청은 오류 `There is not enough space on the disk`와 함께 실패할 수 있습니다. 이 경우 해결 방법은 충분 한 로컬 디스크 공간이 있는 컴퓨터에서 sqlpackage를 실행 하는 것입니다. 이 문제를 방지 하기 위해 [SqlPackage](#import-from-a-bacpac-file-using-sqlpackage) 를 사용 하 여 150GB 보다 큰 데이터베이스를 가져오거나 내보내는 것이 좋습니다.
 
1. Azure Portal을 사용하여 BACPAC 파일에서 새 단일 데이터베이스로 가져오려면 적절한 데이터베이스 서버 페이지를 연 다음, 도구 모음에서 **데이터베이스 가져오기**를 선택합니다.  

   ![데이터베이스 가져오기1](./media/sql-database-import/import1.png)

2. BACPAC 파일의 스토리지 계정 및 컨테이너를 선택한 다음, 가져올 BACPAC 파일을 선택합니다.
3. 새 데이터베이스의 크기(일반적으로 원본과 동일)를 지정하고 대상 SQL Server 자격 증명을 제공합니다. 새 Azure SQL 데이터베이스에 가능한 값 목록은 [데이터베이스 만들기](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-current)를 참조하세요.

   ![데이터베이스 가져오기2](./media/sql-database-import/import2.png)

4. **확인**을 클릭합니다.

5. 가져오기 진행률을 모니터링하려면 데이터베이스의 서버 페이지를 열고, **설정** 아래에서 **가져오기/내보내기 기록**을 선택합니다. 성공하면 가져오기가 **완료됨** 상태가 됩니다.

   ![데이터베이스 가져오기 상태](./media/sql-database-import/import-status.png)

6. 데이터베이스가 데이터베이스 서버에서 라이브 상태인지 확인하려면 **SQL 데이터베이스**를 선택하고 새 데이터베이스가 **온라인** 상태인지 확인합니다.

## <a name="import-from-a-bacpac-file-using-sqlpackage"></a>SqlPackage를 사용하여 BACPAC 파일에서 가져오기

[SqlPackage](https://docs.microsoft.com/sql/tools/sqlpackage) 명령줄 유틸리티를 사용하여 SQL Server 데이터베이스를 가져오려면 [매개 변수 및 속성 가져오기](https://docs.microsoft.com/sql/tools/sqlpackage#import-parameters-and-properties)를 참조하세요. SqlPackage에는 최신 [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) 및 [Visual Studio용 SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx)가 있습니다. Microsoft 다운로드 센터에서 최신 [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876)를 다운로드할 수도 있습니다.

규모 및 성능과 관련하여 대부분의 프로덕션 환경에서는 Azure Portal보다 SqlPackage를 사용하는 것이 좋습니다. `BACPAC` 파일을 사용하는 마이그레이션에 대한 SQL Server 고객 자문 팀 블로그는 [BACPAC 파일을 사용하여 SQL Server에서 Azure SQL Database로 마이그레이션](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/)을 참조하세요.

규모와 성능을 위해 대부분의 프로덕션 환경에서 SqlPackage를 사용하는 것이 좋습니다. BACPAC 파일을 사용한 마이그레이션에 관한 SQL Server 고객 자문 팀 블로그는 [BACPAC 파일을 사용하여 SQL Server에서 Azure SQL Database로 마이그레이션](https://blogs.msdn.microsoft.com/sqlcat/20../../migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/)을 참조하세요.

다음 SqlPackage 명령은 로컬 스토리지의 **AdventureWorks2008R2** 데이터베이스를 **mynewserver20170403**이라는 Azure SQL Database 서버로 가져옵니다. **프리미엄** 서비스 계층과 **P6** 서비스 목표로 **myMigratedDatabase**라는 새 데이터베이스를 만듭니다. 이러한 값을 사용자 환경에 적절하게 변경합니다.

```cmd
SqlPackage.exe /a:import /tcs:"Data Source=mynewserver20170403.database.windows.net;Initial Catalog=myMigratedDatabase;User Id=<your_server_admin_account_user_id>;Password=<your_server_admin_account_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

> [!IMPORTANT]
> 회사 방화벽 뒤에서 단일 데이터베이스를 관리하는 SQL Database 서버에 연결하려면 방화벽에서 1433 포트가 열려 있어야 합니다. 관리되는 인스턴스에 연결하려면 [지점 및 사이트 간 연결](sql-database-managed-instance-configure-p2s.md) 또는 기본 경로 연결이 필요합니다.
>

이 예제는 Active Directory 유니버설 인증으로 SqlPackage를 사용하여 데이터베이스를 가져오는 방법을 보여줍니다.

```cmd
SqlPackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="import-into-a-single-database-from-a-bacpac-file-using-powershell"></a>PowerShell을 사용하여 BACPAC 파일에서 단일 데이터베이스로 가져오기

> [!NOTE]
> [관리 되는 인스턴스는](sql-database-managed-instance.md) 현재 Azure PowerShell를 사용 하 여 데이터베이스를 BACPAC 파일에서 인스턴스 데이터베이스로 마이그레이션하는 것을 지원 하지 않습니다. 관리되는 인스턴스로 가져오려면 SQL Server Management Studio 또는 SQLPackage를 사용합니다.

> [!NOTE]
> 포털 또는 Powershell을 통해 제출 된 가져오기/내보내기 요청을 처리 하는 컴퓨터는 DacFX (데이터 계층 응용 프로그램 프레임 워크)에서 생성 된 임시 파일 뿐만 아니라 bacpac 파일을 저장 해야 합니다. 필요한 디스크 공간은 크기가 동일한 Db에 따라 크게 달라 지 며 데이터베이스 크기는 최대 3 배까지 걸릴 수 있습니다. Import/export 요청을 실행 하는 컴퓨터에는 450GB 로컬 디스크 공간만 있습니다. 따라서 일부 요청은 "디스크 공간이 부족 합니다." 오류와 함께 실패할 수 있습니다. 이 경우 해결 방법은 충분 한 로컬 디스크 공간이 있는 컴퓨터에서 sqlpackage를 실행 하는 것입니다. 150GB 보다 큰 데이터베이스를 가져오거나 내보낼 때 [SqlPackage](#import-from-a-bacpac-file-using-sqlpackage) 를 사용 하 여이 문제를 방지 합니다.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure SQL Database, Azure Resource Manager PowerShell 모듈은 계속 지원하지만 모든 향후 개발은 Az.Sql 모듈에 대해 진행됩니다. 이러한 cmdlet에 대한 내용은 [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)을 참조합니다. Az 모듈과 AzureRm 모듈에서 명령의 인수는 실질적으로 동일합니다.

[AzSqlDatabaseImport](/powershell/module/az.sql/new-azsqldatabaseimport) cmdlet을 사용 하 여 Azure SQL Database 서비스에 데이터베이스 가져오기 요청을 제출 합니다. 데이터베이스 크기에 따라 가져오기를 완료하는 데 다소 시간이 걸릴 수 있습니다.

 ```powershell
 $importRequest = New-AzSqlDatabaseImport 
    -ResourceGroupName "<your_resource_group>" `
    -ServerName "<your_server>" `
    -DatabaseName "<your_database>" `
    -DatabaseMaxSizeBytes "<database_size_in_bytes>" `
    -StorageKeyType "StorageAccessKey" `
    -StorageKey $(Get-AzStorageAccountKey -ResourceGroupName "<your_resource_group>" -StorageAccountName "<your_storage_account").Value[0] `
    -StorageUri "https://myStorageAccount.blob.core.windows.net/importsample/sample.bacpac" `
    -Edition "Standard" `
    -ServiceObjectiveName "P6" `
    -AdministratorLogin "<your_server_admin_account_user_id>" `
    -AdministratorLoginPassword $(ConvertTo-SecureString -String "<your_server_admin_account_password>" -AsPlainText -Force)

 ```

 [AzSqlDatabaseImportExportStatus](/powershell/module/az.sql/get-azsqldatabaseimportexportstatus) cmdlet을 사용 하 여 가져오기 진행 상태를 확인할 수 있습니다. 요청 직후에 cmdlet을 실행하면 대개 **상태: 진행 중**이 반환됩니다. **상태: 성공함**이 표시되면 가져오기가 완료된 것입니다.

```powershell
$importStatus = Get-AzSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
[Console]::Write("Importing")
while ($importStatus.Status -eq "InProgress")
{
    $importStatus = Get-AzSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
    [Console]::Write(".")
    Start-Sleep -s 10
}
[Console]::WriteLine("")
$importStatus
```

> [!TIP]
> 다른 스크립트 예제는 [BACPAC 파일에서 데이터베이스 가져오기](scripts/sql-database-import-from-bacpac-powershell.md)를 참조하세요.

## <a name="limitations"></a>제한 사항

탄력적 풀의 데이터베이스로 가져오기는 지원되지 않습니다. 단일 데이터베이스로 데이터를 가져온 다음, 해당 데이터베이스를 탄력적 풀로 이동할 수 있습니다.

## <a name="import-using-wizards"></a>마법사를 사용하여 가져오기

다음 마법사를 사용할 수도 있습니다.

- [SQL Server Management Studio의 데이터 계층 애플리케이션 가져오기 마법사](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database#using-the-import-data-tier-application-wizard)
- [SQL Server 가져오기 및 내보내기 마법사](https://docs.microsoft.com/sql/integration-services/import-export-data/start-the-sql-server-import-and-export-wizard)

## <a name="next-steps"></a>다음 단계

- 가져온 SQL Database에 연결하고 쿼리하는 방법을 알아보려면 [빠른 시작: Azure SQL Database: SQL Server Management Studio를 사용하여 데이터에 연결 및 쿼리](sql-database-connect-query-ssms.md)를 참조하세요.
- BACPAC 파일을 사용한 마이그레이션에 관한 SQL Server 고객 자문 팀 블로그는 [BACPAC 파일을 사용하여 SQL Server에서 Azure SQL Database로 마이그레이션](https://techcommunity.microsoft.com/t5/DataCAT/Migrating-from-SQL-Server-to-Azure-SQL-Database-using-Bacpac/ba-p/305407)을 참조하세요.
- 성능 권장 사항을 비롯한 전체 SQL Server 데이터베이스 마이그레이션 프로세스에 대한 설명은 [Azure SQL Database로 SQL Server 데이터베이스 마이그레이션](sql-database-single-database-migrate.md)을 참조하세요.
- 스토리지 키 및 공유 액세스 서명을 안전하게 관리하고 공유하는 방법을 알아보려면 [Azure Storage 보안 가이드](https://docs.microsoft.com/azure/storage/common/storage-security-guide)를 참조하세요.
