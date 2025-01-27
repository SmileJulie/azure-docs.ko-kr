---
title: '자습서: Azure HDInsight에서 대화형 쿼리를 사용하여 ETL(추출, 변환, 로드) 작업 수행'
description: 자습서 - 원시 CSV 데이터 세트에서 데이터를 추출하고, HDInsight에서 대화형 쿼리를 사용하여 변환한 다음, Apache Sqoop을 사용하여 변환된 데이터를 Azure SQL 데이터베이스로 로드하는 방법을 알아봅니다.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: tutorial
ms.date: 06/25/2019
ms.author: hrasheed
ms.custom: hdinsightactive,mvc
ms.openlocfilehash: 403e165d7ebe8365ffa0fd2f5f3779d3b4fab68f
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543640"
---
# <a name="tutorial-extract-transform-and-load-data-using-interactive-query-in-azure-hdinsight"></a>자습서: Azure HDInsight에서 대화형 쿼리를 사용하여 데이터 추출, 변환 및 로드

이 자습서에서는 공개적으로 사용할 수 있는 비행 데이터의 원시 CSV 데이터 파일을 선택하여 HDInsight 클러스터 스토리지로 가져온 다음, Azure HDInsight에서 대화형 쿼리를 사용하여 데이터를 변환합니다. 데이터가 변환된 후 [Apache Sqoop](https://sqoop.apache.org/)을 사용하여 Azure SQL 데이터베이스로 해당 데이터를 로드합니다.

이 자습서에서 다루는 작업은 다음과 같습니다.

> [!div class="checklist"]
> * 샘플 비행 데이터 다운로드
> * HDInsight 클러스터에 데이터 업로드
> * 대화형 쿼리를 사용하여 데이터 변환
> * Azure SQL 데이터베이스에 테이블 만들기
> * Sqoop을 사용하여 Azure SQL 데이터베이스에 데이터 내보내기

## <a name="prerequisites"></a>필수 조건

* HDInsight에서 대화형 쿼리 클러스터. [Azure Portal을 사용하여 Apache Hadoop 클러스터 만들기](../hdinsight-hadoop-create-linux-clusters-portal.md)를 참조하고 **클러스터 유형**에 대한 **대화형 쿼리**를 선택합니다.

* Azure SQL Database. Azure SQL 데이터베이스를 대상 데이터 저장소로 사용합니다. SQL 데이터베이스가 없는 경우 [Azure Portal에서 Azure SQL 데이터베이스 만들기](/azure/sql-database/sql-database-single-database-get-started)를 참조하세요.

* SSH 클라이언트. 자세한 내용은 [SSH를 사용하여 HDInsight(Apache Hadoop)에 연결](../hdinsight-hadoop-linux-use-ssh-unix.md)을 참조하세요.

## <a name="download-the-flight-data"></a>비행 데이터 다운로드

1. [Research and Innovative Technology Administration, Bureau of Transportation Statistics](https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time)로 이동합니다.

2. 페이지에서 모든 필드의 선택을 취소하고 다음 값을 선택합니다.

   | 이름 | 값 |
   | --- | --- |
   | Filter Year |2019 |
   | Filter Period |January |
   | 필드 |Year, FlightDate, Reporting_Airline, DOT_ID_Reporting_Airline, Flight_Number_Reporting_Airline, OriginAirportID, Origin, OriginCityName, OriginState, DestAirportID, Dest, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. |

3. **다운로드**를 선택합니다. 선택한 데이터 필드와 함께 .zip 파일을 가져옵니다.

## <a name="upload-data-to-an-hdinsight-cluster"></a>HDInsight 클러스터에 데이터 업로드

HDInsight 클러스터와 연결된 저장소로 데이터를 업로드하는 여러 가지 방법이 있습니다. 이 섹션에서는 `scp`를 사용하여 데이터를 업로드합니다. 데이터를 업로드하는 다른 방법에 대해 알아보려면 [HDInsight에 데이터 업로드](../hdinsight-upload-data.md)를 참조하세요.

1. .zip 파일을 HDInsight 클러스터 헤드 노드에 업로드합니다. `FILENAME`을 .zip 파일의 이름으로 바꾸고 `CLUSTERNAME`을 HDInsight 클러스터의 이름으로 바꿉니다. 그런 후 명령 프롬프트를 열고 작업 디렉터리를 파일 위치로 설정한 다음, 명령을 입력합니다.

    ```cmd
    scp FILENAME.zip sshuser@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.zip
    ```

2. 업로드를 완료한 후에 SSH를 사용하여 클러스터에 연결합니다. `CLUSTERNAME`을 HDInsight 클러스터의 이름으로 바꾸어 아래 명령을 편집합니다. 다음 명령을 입력합니다.

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

3. SSH 연결이 설정된 후 환경 변수를 설정합니다. `FILE_NAME`, `SQL_SERVERNAME`, `SQL_DATABASE`, `SQL_USER` 및 `SQL_PASWORD`를 해당 값으로 바꿉니다. 그런 후 다음 명령을 입력합니다.

    ```bash
    export FILENAME=FILE_NAME
    export SQLSERVERNAME=SQL_SERVERNAME
    export DATABASE=SQL_DATABASE
    export SQLUSER=SQL_USER
    export SQLPASWORD='SQL_PASWORD'
    ```

4. 아래 명령을 입력하여.zip 파일의 압축을 풉니다.

    ```bash
    unzip $FILENAME.zip
    ```

5. 아래 명령을 입력하여 HDInsight 스토리지에 디렉터리를 만든 다음, .csv 파일을 디렉터리에 복사합니다.

    ```bash
    hdfs dfs -mkdir -p /tutorials/flightdelays/data
    hdfs dfs -put $FILENAME.csv /tutorials/flightdelays/data/
    ```

## <a name="transform-data-using-a-hive-query"></a>Hive 쿼리를 사용하여 데이터 변환

HDInsight 클러스터에서 Hive 작업을 실행하는 여러 가지 방법이 있습니다. 이 섹션에서는 [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline%E2%80%93CommandLineShell)을 사용하여 Hive 작업을 실행합니다. Hive 작업 실행의 다른 메서드에 대한 정보는 [HDInsight의 Apache Hive 사용](../hadoop/hdinsight-use-hive.md)을 참조하세요.

Hive 작업의 일부로 .csv 파일에서 **지연**이라는 Hive 테이블로 데이터를 가져옵니다.

1. HDInsight 클러스터에 대해 이미 있는 SSH 프롬프트에서 다음 명령을 사용하여 **flightdelays.hql**이라는 새 파일을 만들고 편집합니다.

    ```bash
    nano flightdelays.hql
    ```

2. 이 파일의 내용으로 다음 텍스트를 사용합니다.

    ```hiveql
    DROP TABLE delays_raw;
    -- Creates an external table over the csv file
    CREATE EXTERNAL TABLE delays_raw (
        YEAR string,
        FL_DATE string,
        UNIQUE_CARRIER string,
        CARRIER string,
        FL_NUM string,
        ORIGIN_AIRPORT_ID string,
        ORIGIN string,
        ORIGIN_CITY_NAME string,
        ORIGIN_CITY_NAME_TEMP string,
        ORIGIN_STATE_ABR string,
        DEST_AIRPORT_ID string,
        DEST string,
        DEST_CITY_NAME string,
        DEST_CITY_NAME_TEMP string,
        DEST_STATE_ABR string,
        DEP_DELAY_NEW float,
        ARR_DELAY_NEW float,
        CARRIER_DELAY float,
        WEATHER_DELAY float,
        NAS_DELAY float,
        SECURITY_DELAY float,
        LATE_AIRCRAFT_DELAY float)
    -- The following lines describe the format and location of the file
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE
    LOCATION '/tutorials/flightdelays/data';

    -- Drop the delays table if it exists
    DROP TABLE delays;
    -- Create the delays table and populate it with data
    -- pulled in from the CSV file (via the external table defined previously)
    CREATE TABLE delays AS
    SELECT YEAR AS year,
        FL_DATE AS flight_date,
        substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier,
        substring(CARRIER, 2, length(CARRIER) -1) AS carrier,
        substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num,
        ORIGIN_AIRPORT_ID AS origin_airport_id,
        substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code,
        substring(ORIGIN_CITY_NAME, 2) AS origin_city_name,
        substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr,
        DEST_AIRPORT_ID AS dest_airport_id,
        substring(DEST, 2, length(DEST) -1) AS dest_airport_code,
        substring(DEST_CITY_NAME,2) AS dest_city_name,
        substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr,
        DEP_DELAY_NEW AS dep_delay_new,
        ARR_DELAY_NEW AS arr_delay_new,
        CARRIER_DELAY AS carrier_delay,
        WEATHER_DELAY AS weather_delay,
        NAS_DELAY AS nas_delay,
        SECURITY_DELAY AS security_delay,
        LATE_AIRCRAFT_DELAY AS late_aircraft_delay
    FROM delays_raw;
    ```

3. 파일을 저장하려면 **Ctrl + X**, **Y**, Enter를 차례로 누릅니다.

4. Hive를 시작하고 **flightdelays.hql** 파일을 실행하려면 다음 명령을 사용합니다.

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f flightdelays.hql
    ```

5. **flightdelays.hql** 스크립트 실행이 완료된 후에 다음 명령을 사용하여 대화형 Beeline 세션을 엽니다.

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

6. `jdbc:hive2://localhost:10001/>` 프롬프트를 수신하면, 다음 쿼리를 사용하여 가져온 비행 지연 데이터에서 데이터를 검색합니다.

    ```hiveql
    INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    SELECT regexp_replace(origin_city_name, '''', ''),
        avg(weather_delay)
    FROM delays
    WHERE weather_delay IS NOT NULL
    GROUP BY origin_city_name;
    ```

    이 쿼리는 평균 지연 시간과 함께 날씨 지연이 발생된 도시 목록이 검색된 후 `/tutorials/flightdelays/output`에 저장됩니다. 나중에 Sqoop는 이 위치에서 데이터를 읽어 Azure SQL Database로 내보냅니다.

7. Beeline을 종료하려면 프롬프트에 `!quit`을 입력합니다.

## <a name="create-a-sql-database-table"></a>SQL Database 테이블 만들기

여러 가지 방법으로 SQL Database에 연결하고 테이블을 생성할 수 있습니다. 다음 단계는 HDInsight 클러스터의 [FreeTDS](http://www.freetds.org/)를 사용합니다.

1. FreeTDS를 설치하려면 클러스터에 대해 열린 SSH 연결에서 다음 명령을 사용합니다.

    ```bash
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

2. 설치가 완료된 후에 다음 명령을 사용하여 SQL Database 서버에 연결합니다.

    ```bash
    TDSVER=8.0 tsql -H $SQLSERVERNAME.database.windows.net -U $SQLUSER -p 1433 -D $DATABASE -P $SQLPASWORD
    ```

    다음 텍스트와 비슷한 출력이 표시됩니다.

    ```
    locale is "en_US.UTF-8"
    locale charset is "UTF-8"
    using default charset "UTF-8"
    Default database being set to <yourdatabase>
    1>
    ```

3. `1>` 프롬프트에 다음 행을 입력합니다.

    ```hiveql
    CREATE TABLE [dbo].[delays](
    [origin_city_name] [nvarchar](50) NOT NULL,
    [weather_delay] float,
    CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED
    ([origin_city_name] ASC))
    GO
    ```

    `GO` 문을 입력하면 이전 문이 평가됩니다. 이 명령문은 클러스터형 인덱스가 있는 **delays**라는 테이블을 만듭니다.

    다음 쿼리를 사용하여 테이블이 생성되었는지 확인합니다.

    ```hiveql
    SELECT * FROM information_schema.tables
    GO
    ```

    다음 텍스트와 유사하게 출력됩니다.

    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    databaseName       dbo             delays        BASE TABLE
    ```

4. `exit` at the `1>`를 입력하여 tsql 유틸리티를 종료합니다.

## <a name="export-data-to-sql-database-using-apache-sqoop"></a>Apache Sqoop을 사용하여 SQL Database로 데이터 내보내기

이전 섹션에서는 `/tutorials/flightdelays/output`에서 변환된 데이터를 복사했습니다. 이 섹션에서는 Sqoop을 사용하여 `/tutorials/flightdelays/output`에서 Azure SQL 데이터베이스에서 만든 테이블로 데이터를 내보냅니다.

1. 아래 명령을 입력하여 Sqoop이 SQL Database를 볼 수 있는지 확인합니다.

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://$SQLSERVERNAME.database.windows.net:1433 --username $SQLUSER --password $SQLPASWORD
    ```

    이 명령은 앞에서 `delays` 테이블을 만든 데이터베이스를 포함한 데이터베이스 목록을 반환합니다.

2. 아래 명령을 입력하여 `/tutorials/flightdelays/output`의 데이터를 `delays`로 내보냅니다.

    ```bash
    sqoop export --connect "jdbc:sqlserver://$SQLSERVERNAME.database.windows.net:1433;database=$DATABASE" --username $SQLUSER --password $SQLPASWORD --table 'delays' --export-dir '/tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1
    ```

    Sqoop은 `delays` 테이블이 포함된 데이터베이스에 연결하고 `/tutorials/flightdelays/output` 디렉터리의 데이터를 `delays` 테이블로 내보냅니다.

3. Sqoop 명령이 완료된 후 tsql 유틸리티에서 아래 명령을 입력하여 데이터베이스에 연결합니다.

    ```bash
    TDSVER=8.0 tsql -H $SQLSERVERNAME.database.windows.net -U $SQLUSER -p 1433 -D $DATABASE -P $SQLPASWORD
    ```

    다음 문을 사용하여 데이터가 delays 테이블로 내보내졌는지 확인합니다.

    ```sql
    SELECT * FROM delays
    GO
    ```

    테이블에 데이터 목록이 표시됩니다. 테이블은 도시 이름 및 해당 도시에 대한 평균 비행 지연 시간을 포함합니다. 

    `exit` 를 입력하여 tsql 유틸리티를 종료합니다.

## <a name="clean-up-resources"></a>리소스 정리

이 자습서를 완료한 후에 클러스터를 삭제할 수 있습니다. HDInsight를 사용하면 데이터가 Azure Storage에 저장되기 때문에 클러스터를 사용하지 않을 때 안전하게 삭제할 수 있습니다. HDInsight 클러스터를 사용하지 않는 기간에도 요금이 청구됩니다. 클러스터에 대한 요금이 저장소에 대한 요금보다 몇 배 더 많기 때문에, 클러스터를 사용하지 않을 때는 삭제하는 것이 경제적인 면에서 더 합리적입니다.

클러스터를 삭제하려면 [브라우저, PowerShell 또는 Azure CLI를 사용하여 HDInsight 클러스터 삭제](../hdinsight-delete-cluster.md)를 참조하세요.

## <a name="next-steps"></a>다음 단계

이 자습서에서는 원시 CSV 데이터 파일을 선택하고 HDInsight 클러스터 스토리지로 가져온 다음, Azure HDInsight에서 대화형 쿼리를 사용하여 데이터를 변환했습니다.  Apache Hive Warehouse 커넥터에 대해 자세히 알아보려면 다음 자습서로 이동합니다.

> [!div class="nextstepaction"]
>[Hive Warehouse 커넥터를 사용하여 Apache Spark 및 Apache Hive 통합](./apache-hive-warehouse-connector.md)
