---
title: Hive 테이블 만들기 및 Blob 스토리지에서 데이터 로드 - Team Data Science Process
description: Hive 쿼리를 사용하여 Hive 테이블을 만들고 Azure Blob 스토리지에서 데이터를 로드합니다. Hive 테이블을 분할하고 ORC(Optimized Row Columnar) 형식을 사용하여 쿼리 성능을 개선합니다.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/04/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: af9c072c428c486cab89288db4c9ee1c26513185
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68250141"
---
# <a name="create-hive-tables-and-load-data-from-azure-blob-storage"></a>Hive 테이블을 만들고 Azure Blob Storage에서 데이터 로드

이 문서에서는 Hive 테이블을 만들고 Azure Blob Storage의 데이터를 로드하는 일반 Hive 쿼리를 보여 줍니다. 또한 Hive 테이블을 분할하고 ORC(Optimized Row Columnar) 형식을 사용하여 쿼리 성능을 개선하는 방법에 대한 지침도 제공됩니다.

## <a name="prerequisites"></a>필수 구성 요소
이 문서에서는 사용자가 다음 작업을 수행한 것으로 가정합니다.

* Azure 저장소 계정을 만들었습니다. 지침이 필요한 경우 [Azure Storage 계정 정보](../../storage/common/storage-introduction.md)를 참조하세요.
* 사용자 지정된 Hadoop 클러스터에 HDInsight 서비스를 프로비전했습니다.  지침이 필요한 경우 [HDInsight에서 클러스터 설정](../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md)을 참조하세요.
* 클러스터에 대한 원격 액세스를 설정하고, 로그인하고, Hadoop 명령줄 콘솔을 열었습니다. 지침이 필요한 경우 [Apache Hadoop 클러스터 관리](../../hdinsight/hdinsight-administer-use-portal-linux.md)를 참조하세요.

## <a name="upload-data-to-azure-blob-storage"></a>Azure blob storage에 데이터 업로드
[고급 분석을 위한 Azure 가상 컴퓨터 설정](../../machine-learning/data-science-virtual-machine/overview.md)의 지침에 따라 Azure 가상 컴퓨터를 만드는 경우 이 스크립트 파일을 가상 컴퓨터의 *C:\\Users\\\<사용자 이름\>\\Documents\\Data Science Scripts* 디렉터리에 다운로드해야 합니다. 이러한 Hive 쿼리는 제출이 가능하도록 적절한 필드에서 사용자 데이터 스키마 및 Azure Blob Storage 구성을 연결하기만 하면 됩니다.

Hive 테이블의 데이터가 **압축되지 않은** 테이블 형식이고 Hadoop 클러스터에서 사용하는 저장소 계정의 기본 또는 추가 컨테이너에 데이터가 업로드된 것으로 가정합니다.

**NYC Taxi Trip Data**로 연습하려면 다음을 수행해야 합니다.

* **NYC Taxi Trip Data** 파일(12개의 Trip 파일과 12개의 Fare 파일)을 [NYC Taxi Trip Data](https://www.andresmh.com/nyctaxitrips) 합니다.
* **압축을 풉니다** .
* Azure Storage 계정의 기본값(또는 해당 컨테이너)에 **업로드**합니다. 해당 계정에 대한 옵션은 [Azure HDInsight 클러스터에서 Azure Storage 사용](../../hdinsight/hdinsight-hadoop-use-blob-storage.md) 항목을 참조하세요. 저장소 계정의 기본 컨테이너에 .csv 파일을 업로드하는 프로세스는 이 [페이지](hive-walkthrough.md#upload)에 나와 있습니다.

## <a name="submit"></a>Hive 쿼리를 제출하는 방법
다음을 사용하여 Hive 쿼리를 제출할 수 있습니다.

1. [Hadoop 클러스터 헤드 노드의 Hadoop 명령줄을 통해 Hive 쿼리 제출](#headnode)
2. [Hive 편집기를 사용하여 Hive 쿼리 제출](#hive-editor)
3. [Azure PowerShell 명령을 사용하여 Hive 쿼리 제출](#ps)

Hive 쿼리는 SQL과 유사하므로, SQL에 익숙한 사용자에게는 [SQL 사용자용 Hive 치트 시트](https://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf)가 유용할 수 있습니다.

또한 Hive 쿼리를 제출할 때 Hive 쿼리에서 화면, 헤드 노드의 로컬 파일, Azure blob 등의 출력 대상을 제어할 수 있습니다.

### <a name="headnode"></a> 1. Hadoop 클러스터 헤드 노드의 Hadoop 명령줄을 통해 Hive 쿼리 제출
Hive 쿼리가 복잡한 경우 Hadoop 클러스터의 헤드 노드에서 바로 Hive 쿼리를 제출하면 일반적으로 Hive 편집기 또는 Azure PowerShell 스크립트를 사용하여 제출하는 것보다 반환 시간이 빠릅니다.

Hadoop 클러스터의 헤드 노드에 로그인하고, 헤드 노드 바탕 화면에서 Hadoop 명령줄을 열고, 명령을 입력합니다 `cd %hive_home%\bin`.

세 가지 방법으로 Hadoop 명령줄에서 Hive 쿼리를 제출할 수 있습니다.

* 직접 제출
* .hql 파일을 사용하여 제출
* Hive 명령 콘솔을 사용하여 제출

#### <a name="submit-hive-queries-directly-in-hadoop-command-line"></a>Hadoop 명령줄에서 직접 Hive 쿼리를 제출합니다.
`hive -e "<your hive query>;` 같은 명령을 실행하여 Hadoop 명령줄에서 바로 간단한 Hive 쿼리를 제출할 수 있습니다. 다음은 그 예제입니다. 빨간색 상자는 Hive 쿼리를 제출하는 명령을, 녹색 상자는 Hive 쿼리의 출력을 보여 줍니다.

![Hive 쿼리의 출력을 사용하여 Hive 쿼리를 제출하는 명령](./media/move-hive-tables/run-hive-queries-1.png)

#### <a name="submit-hive-queries-in-hql-files"></a>.hql 파일로 Hive 쿼리 제출
Hive 쿼리가 좀 더 복잡하고 줄이 여러 개인 경우 명령줄 또는 Hive 명령 콘솔에서 쿼리를 편집하는 방법은 실용적이지 않습니다. 대신 Hadoop 클러스터의 헤드 노드에서 텍스트 편집기를 사용하여 헤드 노드의 로컬 디렉터리에 있는 .hql 파일에 Hive 쿼리를 저장합니다. 그러면 다음과 같이 `-f` 인수를 사용하여 .hql 파일의 Hive 쿼리를 제출할 수 있습니다.

    hive -f "<path to the .hql file>"

![.hql 파일의 Hive 쿼리](./media/move-hive-tables/run-hive-queries-3.png)

**Hive 쿼리의 진행 상태 화면 인쇄 숨기기**

기본적으로 Hadoop 명령줄에서 Hive 쿼리가 제출되면 맵/감소 작업의 진행 상태가 화면에 인쇄됩니다. 맵/감소 작업의 진행 상태 화면 인쇄를 숨기려면 다음과 같이 명령줄에 `-S` 인수("S"는 대문자)를 사용합니다.

    hive -S -f "<path to the .hql file>"
    hive -S -e "<Hive queries>"

#### <a name="submit-hive-queries-in-hive-command-console"></a>Hive 명령 콘솔에서 Hive 쿼리를 제출합니다.
또한 Hadoop 명령줄에서 `hive` 명령을 실행하여 Hive 명령 콘솔을 먼저 입력한 후 Hive 명령 콘솔에서 Hive 쿼리를 제출할 수 있습니다. 다음은 예제입니다. 이 예제에서 두 빨간색 상자는 각각 Hive 명령 콘솔을 입력하는 데 사용된 명령과 Hive 명령 콘솔에서 제출된 Hive 쿼리를 보여 줍니다. 녹색 상자는 Hive 쿼리의 출력을 보여 줍니다.

![Hive 명령 콘솔을 열고 명령을 입력하고, Hive 쿼리 출력 보기](./media/move-hive-tables/run-hive-queries-2.png)

이전 예제에서는 Hive 쿼리 결과가 화면에 바로 출력됩니다. 또한 헤드 로드의 로컬 파일 또는 Azure blob에 출력을 작성할 수 있습니다. 그런 다음 다른 도구를 사용하여 Hive 쿼리 출력을 추가로 분석할 수 있습니다.

**Hive 쿼리 결과를 로컬 파일에 출력합니다.**
Hive 쿼리 결과를 헤드 노드의 로컬 디렉터리에 출력하려면 다음과 같이 Hadoop 명령줄에서 Hive 쿼리를 제출해야 합니다.

    hive -e "<hive query>" > <local path in the head node>

다음 예제에서 Hive 쿼리의 출력은 `C:\apps\temp` 디렉터리의 `hivequeryoutput.txt` 파일에 작성됩니다.

![Hive 쿼리의 출력](./media/move-hive-tables/output-hive-results-1.png)

**Azure blob에 Hive 쿼리 결과 출력**

Hadoop 클러스터의 기본 컨테이너 내에 있는 Azure blob에 Hive 쿼리 결과를 출력할 수도 있습니다. 이 경우 관련 Hive 쿼리는 다음과 같습니다.

    insert overwrite directory wasb:///<directory within the default container> <select clause from ...>

다음 예제에서 Hive 쿼리는 Hadoop 클러스터의 기본 컨테이너 내에 있는 blob 디렉터리 `queryoutputdir` 에 작성됩니다. 이때 사용자는 blob 이름 없이 디렉터리 이름만 입력하면 됩니다. `wasb:///queryoutputdir/queryoutput.txt`처럼 디렉터리 이름과 blob 이름을 모두 입력하면 오류가 발생합니다.

![Hive 쿼리의 출력](./media/move-hive-tables/output-hive-results-2.png)

Azure Storage Explorer를 사용하여 Hadoop 클러스터의 기본 컨테이너를 열면 다음과 같은 Hive 쿼리 출력을 볼 수 있습니다. 필터(빨간색 상자로 강조 표시됨)를 적용하여 이름에 지정된 문자가 포함된 blob만 검색할 수 있습니다.

![Hive 쿼리의 출력을 표시하는 Azure Storage Explorer](./media/move-hive-tables/output-hive-results-3.png)

### <a name="hive-editor"></a> 2. Hive 편집기를 사용하여 Hive 쿼리 제출
웹 브라우저에 *https: Hadoop cluster name >와 같은\//\<* 형식의 URL을 입력 하 여 쿼리 콘솔 (Hive 편집기)을 사용할 수도 있습니다. 이 콘솔을 보려면 로그인해야 하며, Hadoop 클러스터 자격 증명이 필요합니다.

### <a name="ps"></a> 3. Azure PowerShell 명령을 사용하여 Hive 쿼리 제출
PowerShell을 사용하여 Hive 쿼리를 제출할 수도 있습니다. 자세한 내용은 [PowerShell을 사용하여 Hive 작업 제출](../../hdinsight/hadoop/apache-hadoop-use-hive-powershell.md)을 참조하세요.

## <a name="create-tables"></a>Hive 데이터베이스 및 테이블 만들기
Hive 쿼리는 [GitHub 리포지토리](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql)에서 공유되며 여기에서 다운로드할 수 있습니다.

다음은 Hive 테이블을 만드는 Hive 쿼리입니다.

    create database if not exists <database name>;
    CREATE EXTERNAL TABLE if not exists <database name>.<table name>
    (
        field1 string,
        field2 int,
        field3 float,
        field4 double,
        ...,
        fieldN string
    )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' lines terminated by '<line separator>'
    STORED AS TEXTFILE LOCATION '<storage location>' TBLPROPERTIES("skip.header.line.count"="1");

다음은 연결해야 하는 필드와 기타 구성에 대한 설명입니다.

* **\<데이터베이스 이름\>** : 만들려고 하는 데이터베이스 이름입니다. 기본 데이터베이스를 사용하려는 경우 *create database...* 쿼리를 생략할 수 있습니다.
* **\<테이블 이름\>** : 지정된 데이터베이스 내에 만들려는 테이블 이름입니다. 기본 데이터베이스를 사용하려는 경우 \<데이터베이스 이름\> 없이  \<테이블 이름\>을 통해 직접 테이블을 참조할 수 있습니다.
* **\<필드 구분 기호\>** : 데이터 파일에서 Hive 테이블에 업로드할 필드를 구분하는 구분 기호입니다.
* **\<줄 구분 기호\>** : 데이터 파일의 줄을 구분하는 구분 기호입니다.
* **\<스토리지 위치\>** : Hive 테이블의 데이터를 저장할 Azure Storage 위치입니다.  LOCATION \<스토리지 위치\>를 지정하지 않으면 기본적으로 데이터베이스 및 테이블이 Hive 클러스터의 기본 컨테이너에 있는 *hive/warehouse/* 디렉터리에 저장됩니다. 저장소 위치를 지정하려면 저장소 위치가 데이터베이스 및 테이블의 기본 컨테이너 내부에 있어야 합니다. 이 위치는 *' wasb:///\<directory 1 >/'* 또는 *'\<wasb:///directory 1 >/\<directory 2 >/'* 등과 같은 형식의 클러스터 기본 컨테이너를 기준으로 하는 위치를 참조 해야 합니다. 쿼리가 실행된 후 기본 컨테이너 내에 상대 디렉터리가 만들어집니다.
* **TBLPROPERTIES("skip.header.line.count"="1")** : 데이터 파일에 헤더 줄이 있으면 *create table* 쿼리의 **끝**에 이 속성을 추가해야 합니다. 그렇지 않으면 헤더 줄이 테이블의 레코드로 로드됩니다. 데이터 파일에 헤더 줄이 없으면 쿼리에서 이 구성을 생략해도 됩니다.

## <a name="load-data"></a>Hive 테이블에 데이터 로드
다음은 Hive 테이블에 데이터를 로드하는 Hive 쿼리입니다.

    LOAD DATA INPATH '<path to blob data>' INTO TABLE <database name>.<table name>;

* **\<blob 데이터 경로\>** : Hive 테이블에 업로드할 blob 파일이 HDInsight Hadoop 클러스터의 기본 컨테이너에 있는 경우  *\<blob 데이터\> 에 대 한 경로* 는 *이 컨테이너의\<wasb://디렉터리 형식 이어야 합니다 >/blob\<파일 이름 > '* 입니다. blob 파일이 HDInsight Hadoop 클러스터의 추가 컨테이너에 있을 수도 있습니다. 이 경우  *\<blob 데이터\> 에 대 한 경로* 는 *' wasb://\<container name >\<저장소 계정 이름 >\<* 형식 이어야 합니다 (> ').

  > [!NOTE]
  > Hive 테이블에 업로드할 blob 데이터가 Hadoop 클러스터에 대한 저장소 계정의 기본 또는 추가 컨테이너에 있어야 합니다. 그렇지 않으면 데이터에 액세스할 수 없기 때문에 *LOAD DATA* 쿼리가 실패합니다.
  >
  >

## <a name="partition-orc"></a>고급 토픽: 분할된 테이블 및 ORC 형식으로 Hive 데이터 저장
데이터가 큰 경우 테이블을 분할하면 테이블의 파티션 몇 개만 검색하면 되는 쿼리의 속도가 향상됩니다. 예를 들어 웹 사이트의 로그 데이터를 날짜별로 분할하는 것이 합리적입니다.

Hive 테이블 분할 외에도 Hive 데이터를 ORC(Optimized Row Columnar) 형식으로 저장하는 방법 또한 도움이 됩니다. ORC 형식에 대한 자세한 내용은 <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">ORC 파일을 사용하면 Hive에서 데이터를 읽고, 쓰고, 처리할 때 성능 향상</a>을 참조하세요.

### <a name="partitioned-table"></a>분할된 테이블
다음은 분할된 테이블을 만들고 그 테이블에 데이터를 로드하는 Hive 쿼리입니다.

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<partitioned table name>
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

분할된 테이블에 쿼리할 때 `where` 절의 **시작 부분**에 파티션 조건을 추가하면 검색 효율성이 크게 향상됩니다.

    select
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name>
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <a name="orc"></a>ORC 형식으로 Hive 데이터 저장
Blob Storage의 데이터를 ORC 형식으로 저장된 Hive 테이블에 바로 로드할 수 없습니다. Azure blob의 데이터를 ORC 형식으로 저장된 Hive 테이블에 로드하려면 다음 단계를 수행해야 합니다.

외부 테이블 **STORED AS TEXTFILE** 을 만들고 Blob Storage의 데이터를 이 테이블에 로드합니다.

        CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<external textfile table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
            lines terminated by '<line separator>' STORED AS TEXTFILE
            LOCATION 'wasb:///<directory in Azure blob>' TBLPROPERTIES("skip.header.line.count"="1");

        LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<table name>;

1단계의 외부 테이블과 동일한 스키마 및 필드 구분 기호를 사용하여 내부 테이블을 만들고 Hive 데이터를 ORC 형식으로 저장합니다.

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

1단계의 외부 테이블에서 데이터를 선택하여 ORC 테이블에 삽입합니다.

        INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

> [!NOTE]
> TEXTFILE 테이블  \<데이터베이스 이름\>.\<외부 텍스트 파일 테이블 이름\>에 파티션이 있으면 3단계의 `SELECT * FROM <database name>.<external textfile table name>` 명령에서는 반환된 데이터 집합의 필드로 파티션 변수를 선택합니다.  \<데이터베이스 이름\>.\<ORC 테이블 이름\>에 삽입은 실패하는데,  \<데이터베이스 이름\>.\<ORC 테이블 이름\>에는 테이블 스키마의 필드로 해당 파티션 변수가 포함되어 있지 않기 때문입니다. 이 경우  \<데이터베이스 이름\>.\<ORC 테이블 이름\>에 삽입할 필드를 다음과 같이 구체적으로 선택해야 합니다.
>
>

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
           SELECT field1, field2, ..., fieldN
           FROM <database name>.<external textfile table name>
           WHERE <partition variable>=<partition value>;

모든 데이터가  \<데이터베이스 이름\>.\<ORC 테이블 이름\>에 삽입된 후에는 다음 쿼리를 사용할 때  \<외부 텍스트 파일 테이블 이름\>을 삭제하는 것이 안전합니다.

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

이 절차를 모두 수행했다면 이제 ORC 형식의 데이터를 사용할 수 있는 테이블이 준비되었을 것입니다.  
