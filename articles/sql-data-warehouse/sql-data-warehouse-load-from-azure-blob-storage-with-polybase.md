---
title: Azure SQL Data Warehouse에 Contoso 소매 데이터 로드 | Microsoft Docs
description: Contoso 소매 데이터에서 Azure SQL Data Warehouse로 두 개의 테이블을 로드하기 위해 PolyBase와 T-SQL 명령을 사용합니다.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: load-data
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: b96b65b7dd38900fccb8d5d3a9133f37ee93949f
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67595512"
---
# <a name="load-contoso-retail-data-to-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse에 Contoso 소매 데이터 로드

이 자습서에서는 PolyBase와 T-SQL 명령을 사용하여 Azure SQL Data Warehouse로 Contoso 소매 데이터의 두 테이블을 로드하는 것을 배웁니다. 

이 자습서에서는 다음 작업을 수행합니다.

1. Azure Blob Storage에서 로드하기 위한 PolyBase 구성
2. 공용 데이터를 데이터베이스에 로드
3. 로드가 완료 된 후에 최적화를 수행합니다.

## <a name="before-you-begin"></a>시작하기 전 주의 사항
이 자습서를 실행 하려면 SQL Data Warehouse에 이미 있는 Azure 계정이 필요 합니다. 프로 비전 하는 데이터 웨어하우스가 없는 경우 [SQL Data Warehouse를 만들고 서버 수준 방화벽 규칙 설정][Create a SQL Data Warehouse]합니다.

## <a name="1-configure-the-data-source"></a>1. 데이터 원본 구성
PolyBase는 T-SQL 외부 개체를 사용하여 외부 데이터의 위치와 특성을 정의합니다. 외부 개체의 정의는 SQL Data Warehouse에 저장됩니다. 데이터는 외부적으로 저장 됩니다.

### <a name="11-create-a-credential"></a>1.1. 자격 증명 만들기
Contoso 공용 데이터를 로드하는 중이라면 **이 단계를 건너뜁니다**. 모든 사용자가 이미 액세스할 수 있으므로 공용 데이터에 대한 보안 액세스는 필요하지 않습니다.

사용자 고유의 데이터를 로드하기 위한 템플릿으로 이 자습서를 사용 중인 경우 **이 단계를 건너뛰지 마세요**. 자격 증명을 통해 데이터에 액세스하려면 다음 스크립트를 사용해 데이터베이스 범위 자격 증명을 생성하고 데이터 원본의 위치를 정의할 때 이를 사용합니다.

```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);
```

### <a name="12-create-the-external-data-source"></a>1.2. 외부 데이터 원본 만들기
이 [CREATE EXTERNAL DATA SOURCE][CREATE EXTERNAL DATA SOURCE] 명령을 사용하여 데이터의 위치와 데이터의 형식을 저장합니다. 

```sql
CREATE EXTERNAL DATA SOURCE AzureStorage_west_public
WITH 
(  
    TYPE = Hadoop 
,   LOCATION = 'wasbs://contosoretaildw-tables@contosoretaildw.blob.core.windows.net/'
); 
```

> [!IMPORTANT]
> Azure Blob Storage 컨테이너를 공용으로 만들도록 선택하는 경우 데이터가 데이터 센터에서 떠날 때 데이터 소유자로서 귀하에게 데이터 송신 요금이 부과될 것임을 염두에 두어야 합니다. 
> 
> 

## <a name="2-configure-data-format"></a>2. 데이터 형식 구성
데이터는 Azure Blob Storage에 텍스트 파일로 저장되고 각 필드는 구분 기호로 구분됩니다. SSMS에서 다음을 실행 [CREATE EXTERNAL FILE FORMAT][CREATE EXTERNAL FILE FORMAT] 텍스트 파일에서 데이터의 형식을 지정 하는 명령입니다. Contoso 데이터는 압축되어 있지 않으며 파이프로 구분됩니다.

```sql
CREATE EXTERNAL FILE FORMAT TextFileFormat 
WITH 
(   FORMAT_TYPE = DELIMITEDTEXT
,    FORMAT_OPTIONS    (   FIELD_TERMINATOR = '|'
                    ,    STRING_DELIMITER = ''
                    ,    DATE_FORMAT         = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,    USE_TYPE_DEFAULT = FALSE 
                    )
);
``` 

## <a name="3-create-the-external-tables"></a>3. 외부 테이블 만들기
데이터 원본과 파일 형식을 지정 했으니 외부 테이블을 만들 준비가 되었습니다. 

### <a name="31-create-a-schema-for-the-data"></a>3.1. 데이터에 대한 스키마를 만듭니다.
데이터베이스에 Contoso 데이터를 저장할 수 있는 위치를 만들려면 스키마를 생성합니다.

```sql
CREATE SCHEMA [asb]
GO
```

### <a name="32-create-the-external-tables"></a>3.2. 외부 테이블을 만듭니다.
DimProduct와 FactOnlineSales 외부 테이블을 만들려면 다음 스크립트를 실행합니다. 여기서 열 이름과 데이터 형식을 정의하고 Azure blob 저장소 파일의 형식과 위치에 바인딩합니다. 해당 정의는 SQL Data Warehouse에 저장되고 데이터는 여전히 Azure Storage Blob에 위치합니다.

**LOCATION** 매개 변수는 Azure Storage Blob의 루트 폴더 아래에 있는 폴더입니다. 각 테이블은 서로 다른 폴더에 있습니다.

```sql
--DimProduct
CREATE EXTERNAL TABLE [asb].DimProduct (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL,
    [ProductDescription] [nvarchar](400) NULL,
    [ProductSubcategoryKey] [int] NULL,
    [Manufacturer] [nvarchar](50) NULL,
    [BrandName] [nvarchar](50) NULL,
    [ClassID] [nvarchar](10) NULL,
    [ClassName] [nvarchar](20) NULL,
    [StyleID] [nvarchar](10) NULL,
    [StyleName] [nvarchar](20) NULL,
    [ColorID] [nvarchar](10) NULL,
    [ColorName] [nvarchar](20) NOT NULL,
    [Size] [nvarchar](50) NULL,
    [SizeRange] [nvarchar](50) NULL,
    [SizeUnitMeasureID] [nvarchar](20) NULL,
    [Weight] [float] NULL,
    [WeightUnitMeasureID] [nvarchar](20) NULL,
    [UnitOfMeasureID] [nvarchar](10) NULL,
    [UnitOfMeasureName] [nvarchar](40) NULL,
    [StockTypeID] [nvarchar](10) NULL,
    [StockTypeName] [nvarchar](40) NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [AvailableForSaleDate] [datetime] NULL,
    [StopSaleDate] [datetime] NULL,
    [Status] [nvarchar](7) NULL,
    [ImageURL] [nvarchar](150) NULL,
    [ProductURL] [nvarchar](150) NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/DimProduct/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;

--FactOnlineSales
CREATE EXTERNAL TABLE [asb].FactOnlineSales 
(
    [OnlineSalesKey] [int]  NOT NULL,
    [DateKey] [datetime] NOT NULL,
    [StoreKey] [int] NOT NULL,
    [ProductKey] [int] NOT NULL,
    [PromotionKey] [int] NOT NULL,
    [CurrencyKey] [int] NOT NULL,
    [CustomerKey] [int] NOT NULL,
    [SalesOrderNumber] [nvarchar](20) NOT NULL,
    [SalesOrderLineNumber] [int] NULL,
    [SalesQuantity] [int] NOT NULL,
    [SalesAmount] [money] NOT NULL,
    [ReturnQuantity] [int] NOT NULL,
    [ReturnAmount] [money] NULL,
    [DiscountQuantity] [int] NULL,
    [DiscountAmount] [money] NULL,
    [TotalCost] [money] NOT NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/FactOnlineSales/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
```

## <a name="4-load-the-data"></a>4. 데이터 로드
외부 데이터에 액세스 하는 방법은 여러 가지입니다.  외부 테이블에서 직접 데이터를 쿼리, 데이터 웨어하우스의 새로운 테이블로 데이터를 로드 하거나 기존 데이터 웨어하우스 테이블에 외부 데이터를 추가할 수 있습니다.  

### <a name="41-create-a-new-schema"></a>4.1. 새 스키마를 만듭니다.
CTAS는 데이터가 포함된 새 테이블을 만듭니다.  먼저 contoso 데이터에 대한 스키마를 만듭니다.

```sql
CREATE SCHEMA [cso]
GO
```

### <a name="42-load-the-data-into-new-tables"></a>4.2. 데이터를 새 테이블에 로드합니다.
데이터 웨어하우스 테이블에 Azure blob storage에서 데이터를 로드 하려면 사용 합니다 [CREATE TABLE AS SELECT (TRANSACT-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] statement. Loading with CTAS leverages the strongly typed external tables you've created. To load the data into new tables, use one [CTAS][CTAS] 테이블당 문입니다. 
 
CTAS는 새 테이블을 만들고 select 문의 결과로 새 테이블을 채웁니다. CTAS는 select 문의 결과와 동일한 열과 데이터 형식을 가지도록 새 테이블을 정의합니다. 외부 테이블에서 모든 열을 선택하는 경우 새 테이블은 외부 테이블의 열과 데이터 형식의 복제본이 됩니다.

이 예제에서는 해시 분산 테이블로 차원과 팩트 테이블을 만듭니다. 

```sql
SELECT GETDATE();
GO

CREATE TABLE [cso].[DimProduct]            WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[DimProduct]             OPTION (LABEL = 'CTAS : Load [cso].[DimProduct]             ');
CREATE TABLE [cso].[FactOnlineSales]       WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[FactOnlineSales]        OPTION (LABEL = 'CTAS : Load [cso].[FactOnlineSales]        ');
```

### <a name="43-track-the-load-progress"></a>4.3 로드 진행률 추적
DMV(동적 관리 뷰)를 사용하여 로드 진행률을 추적할 수 있습니다. 

```sql
-- To see all requests
SELECT * FROM sys.dm_pdw_exec_requests;

-- To see a particular request identified by its label
SELECT * FROM sys.dm_pdw_exec_requests as r
WHERE r.[label] = 'CTAS : Load [cso].[DimProduct]             '
      OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
;

-- To track bytes and files
SELECT
    r.command,
    s.request_id,
    r.status,
    count(distinct input_name) as nbr_files, 
    sum(s.bytes_processed)/1024/1024/1024 as gb_processed
FROM
    sys.dm_pdw_exec_requests r
    inner join sys.dm_pdw_dms_external_work s
        on r.request_id = s.request_id
WHERE 
    r.[label] = 'CTAS : Load [cso].[DimProduct]             '
    OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
GROUP BY
    r.command,
    s.request_id,
    r.status
ORDER BY
    nbr_files desc,
    gb_processed desc;
```

## <a name="5-optimize-columnstore-compression"></a>5. Columnstore 압축을 최적화합니다.
기본적으로 SQL Data Warehouse는 클러스터형 columnstore 인덱스로 테이블을 저장합니다. 로드를 완료한 후 데이터 행 일부는 columnstore로 압축되지 않을 수 있습니다.  이에는 다양한 이유가 있을 수 있습니다. 자세한 내용은 [Columnstore 인덱스 관리][manage columnstore indexes]를 참조하세요.

로드 후 쿼리 성능과 columnstore 압축을 최적화하려면 모든 행을 압축하기 위해 columnstore 인덱스를 강제 적용할 테이블을 다시 빌드합니다. 

```sql
SELECT GETDATE();
GO

ALTER INDEX ALL ON [cso].[DimProduct]               REBUILD;
ALTER INDEX ALL ON [cso].[FactOnlineSales]          REBUILD;
```

Columnstore 인덱스 유지 관리에 대한 자세한 내용은 [columnstore 인덱스 관리][manage columnstore indexes] 문서를 참조하세요.

## <a name="6-optimize-statistics"></a>6. 통계를 최적화합니다.
로드 직후 단일 열 통계를 작성하는 것이 좋습니다. 쿼리 조건자에 특정 열이 사용되지 않을 것을 알고 있는 경우, 해당 열에서 통계 생성을 건너뛸 수 있습니다. 모든 열에 단일 열 통계를 만드는 경우 모든 통계를 다시 작성하는데 시간이 오래 걸릴 수 있습니다. 

단일 열 통계를 모든 테이블의 모든 열에 대해 만들기로 결정한 경우 [통계][statistics] 문서의 저장 프로시저 코드 샘플 `prc_sqldw_create_stats`를 사용할 수 있습니다.

다음 예제는 통계를 만들기 위한 좋은 출발점이 됩니다. 차원 테이블의 각 열과 팩트 테이블의 각 조인 열의 단일 열 통계를 생성합니다. 이후 언제라도 다른 팩트 테이블 열에 단일 또는 여러 열 통계를 추가할 수 있습니다.

```sql
CREATE STATISTICS [stat_cso_DimProduct_AvailableForSaleDate] ON [cso].[DimProduct]([AvailableForSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_BrandName] ON [cso].[DimProduct]([BrandName]);
CREATE STATISTICS [stat_cso_DimProduct_ClassID] ON [cso].[DimProduct]([ClassID]);
CREATE STATISTICS [stat_cso_DimProduct_ClassName] ON [cso].[DimProduct]([ClassName]);
CREATE STATISTICS [stat_cso_DimProduct_ColorID] ON [cso].[DimProduct]([ColorID]);
CREATE STATISTICS [stat_cso_DimProduct_ColorName] ON [cso].[DimProduct]([ColorName]);
CREATE STATISTICS [stat_cso_DimProduct_ETLLoadID] ON [cso].[DimProduct]([ETLLoadID]);
CREATE STATISTICS [stat_cso_DimProduct_ImageURL] ON [cso].[DimProduct]([ImageURL]);
CREATE STATISTICS [stat_cso_DimProduct_LoadDate] ON [cso].[DimProduct]([LoadDate]);
CREATE STATISTICS [stat_cso_DimProduct_Manufacturer] ON [cso].[DimProduct]([Manufacturer]);
CREATE STATISTICS [stat_cso_DimProduct_ProductDescription] ON [cso].[DimProduct]([ProductDescription]);
CREATE STATISTICS [stat_cso_DimProduct_ProductKey] ON [cso].[DimProduct]([ProductKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductLabel] ON [cso].[DimProduct]([ProductLabel]);
CREATE STATISTICS [stat_cso_DimProduct_ProductName] ON [cso].[DimProduct]([ProductName]);
CREATE STATISTICS [stat_cso_DimProduct_ProductSubcategoryKey] ON [cso].[DimProduct]([ProductSubcategoryKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductURL] ON [cso].[DimProduct]([ProductURL]);
CREATE STATISTICS [stat_cso_DimProduct_Size] ON [cso].[DimProduct]([Size]);
CREATE STATISTICS [stat_cso_DimProduct_SizeRange] ON [cso].[DimProduct]([SizeRange]);
CREATE STATISTICS [stat_cso_DimProduct_SizeUnitMeasureID] ON [cso].[DimProduct]([SizeUnitMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_Status] ON [cso].[DimProduct]([Status]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeID] ON [cso].[DimProduct]([StockTypeID]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeName] ON [cso].[DimProduct]([StockTypeName]);
CREATE STATISTICS [stat_cso_DimProduct_StopSaleDate] ON [cso].[DimProduct]([StopSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_StyleID] ON [cso].[DimProduct]([StyleID]);
CREATE STATISTICS [stat_cso_DimProduct_StyleName] ON [cso].[DimProduct]([StyleName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitCost] ON [cso].[DimProduct]([UnitCost]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureID] ON [cso].[DimProduct]([UnitOfMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureName] ON [cso].[DimProduct]([UnitOfMeasureName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitPrice] ON [cso].[DimProduct]([UnitPrice]);
CREATE STATISTICS [stat_cso_DimProduct_UpdateDate] ON [cso].[DimProduct]([UpdateDate]);
CREATE STATISTICS [stat_cso_DimProduct_Weight] ON [cso].[DimProduct]([Weight]);
CREATE STATISTICS [stat_cso_DimProduct_WeightUnitMeasureID] ON [cso].[DimProduct]([WeightUnitMeasureID]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CurrencyKey] ON [cso].[FactOnlineSales]([CurrencyKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CustomerKey] ON [cso].[FactOnlineSales]([CustomerKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_DateKey] ON [cso].[FactOnlineSales]([DateKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_OnlineSalesKey] ON [cso].[FactOnlineSales]([OnlineSalesKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_ProductKey] ON [cso].[FactOnlineSales]([ProductKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_PromotionKey] ON [cso].[FactOnlineSales]([PromotionKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_StoreKey] ON [cso].[FactOnlineSales]([StoreKey]);
```

## <a name="achievement-unlocked"></a>목표를 달성했습니다!
이제 Azure SQL Data Warehouse에 공용 데이터를 성공적으로 로드했습니다. 잘 하셨습니다!

데이터를 탐색 하려면 테이블을 쿼리하여 이제 시작할 수 있습니다. 브랜드 총 판매액을 확인 하려면 다음 쿼리를 실행 합니다.

```sql
SELECT  SUM(f.[SalesAmount]) AS [sales_by_brand_amount]
,       p.[BrandName]
FROM    [cso].[FactOnlineSales] AS f
JOIN    [cso].[DimProduct]      AS p ON f.[ProductKey] = p.[ProductKey]
GROUP BY p.[BrandName]
```

## <a name="next-steps"></a>다음 단계
전체 데이터 집합을 로드하려면, Microsoft SQL Server Samples 리포지토리의 [전체 Contoso 소매 데이터 웨어하우스 로드](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md) 예제를 실행합니다.

더 많은 개발 팁은 [SQL Data Warehouse 개발 개요][SQL Data Warehouse development overview]를 참조하세요.

<!--Image references-->

<!--Article references-->
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Load data into SQL Data Warehouse]: sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[manage columnstore indexes]: sql-data-warehouse-tables-index.md
[Statistics]: sql-data-warehouse-tables-statistics.md
[CTAS]: sql-data-warehouse-develop-ctas.md
[label]: sql-data-warehouse-develop-label.md

<!--MSDN references-->
[CREATE EXTERNAL DATA SOURCE]: https://msdn.microsoft.com/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE TABLE AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[REBUILD]: https://msdn.microsoft.com/library/ms188388.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
[Load the full Contoso Retail Data Warehouse]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md
