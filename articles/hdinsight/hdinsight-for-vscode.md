---
title: Visual Studio Code 용 Azure HDInsight
description: Visual Studio Code에 대해 Spark & Hive 도구 (Azure HDInsight)를 사용 하 여 쿼리와 스크립트를 만들고 제출 하는 방법에 대해 알아봅니다.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: 31f6c34089c1825eca21283b01eae181c8112216
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68312179"
---
# <a name="use-spark--hive-tools-for-visual-studio-code"></a>Visual Studio Code Spark & Hive 도구 사용

Visual Studio Code Spark & Hive 도구를 사용 하 여 Apache Hive batch 작업, 대화형 Hive 쿼리 및 Apache Spark에 대 한 PySpark 스크립트를 만들고 제출 하는 방법에 대해 알아봅니다. 먼저 Visual Studio Code에 Spark & Hive 도구를 설치 하는 방법을 설명 하 고 Hive 및 Spark에 작업을 제출 하는 방법을 안내 합니다.  

Windows, Linux 및 macOS를 포함 하는 Visual Studio Code에서 지원 되는 플랫폼에 Spark & Hive 도구를 설치할 수 있습니다. 아래에서 다양한 플랫폼에 대한 필수 조건을 찾을 수 있습니다.


## <a name="prerequisites"></a>필수 구성 요소

이 문서의 단계를 완료하려면 다음 항목이 필요합니다.

- HDInsight 클러스터. 클러스터를 만들려면 [HDInsight 시작](hadoop/apache-hadoop-linux-create-cluster-get-started-portal.md)을 참조하세요. 또는 Livy 끝점을 지 원하는 Spark/Hive 클러스터.
- 및[Visual Studio Code](https://code.visualstudio.com/)가 있습니다.
- [Mono](https://www.mono-project.com/docs/getting-started/install/). Mono는 Linux 및 macOS에만 필요합니다.
- [Visual Studio Code용 PySpark 대화형 환경 설정](set-up-pyspark-interactive-environment.md)
- **HDexample**이라는 로컬 디렉터리.  이 문서에서는 **C:\HD\HDexample**을 사용합니다.

## <a name="install-spark--hive-tools"></a>Spark & Hive 도구 설치

필수 구성 요소를 완료 한 후에는 Visual Studio Code에 대 한 Spark & Hive 도구를 설치할 수 있습니다.  Spark & Hive 도구를 설치 하려면 다음 단계를 완료 합니다.

1. Visual Studio Code를 엽니다.

2. 메뉴 모음에서 **보기** > **확장**으로 이동합니다.

3. 검색 상자에 **Spark & Hive**를 입력 합니다.

4. 검색 결과에서 **Spark & Hive 도구** 를 선택 하 고 **설치**를 선택 합니다.  

   ![Python 설치 Visual Studio Code에 대 한 Spark & Hive](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

5. 필요한 경우 **다시 로드** 합니다.


## <a name="open-work-folder"></a>작업 폴더 열기

작업 폴더를 열고 Visual Studio Code에서 파일을 만들려면 다음 단계를 완료합니다.

1. 메뉴 모음에서 **파일** > **폴더 열기...**  > **C:\HD\HDexample**로 이동한 다음, **폴더 선택** 단추를 선택합니다. 왼쪽의 **탐색기** 뷰에 폴더가 나타납니다.

2. **탐색기** 뷰에서 폴더, **HDexample**, 작업 폴더 옆의 **새 파일** 아이콘을 차례로 선택합니다.

   ![새 파일](./media/hdinsight-for-vscode/new-file.png)

3. `.hql` (Hive 쿼리) `.py` 또는 (Spark 스크립트) 파일 확장명으로 새 파일의 이름을로 바꿉니다.  이 예제에서는 **HelloWorld.hql**을 사용합니다.

## <a name="set-the-azure-environment"></a>Azure 환경 설정

국가별 클라우드 사용자의 경우 먼저 azure 환경을 설정 하는 단계에 따라 azure를 **사용 합니다.**  로그인 명령을 Azure에 로그인 합니다.
   
1. **File\Preferences\Settings**을 클릭 합니다.
2. Azure **검색: Pnrp**
3. 목록에서 국가 클라우드를 선택 합니다.

   ![기본 로그인 항목 구성 설정](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="connect-to-azure-account"></a>Azure 계정에 연결

Visual Studio Code에서 클러스터에 스크립트를 제출 하려면 먼저 Azure 계정에 연결 하거나 클러스터를 연결 해야 합니다 (Ambari 사용자 이름/암호 또는 도메인 가입 계정 사용).  Azure에 연결하려면 다음 단계를 완료합니다.

1. 메뉴 모음에서 **보기** > **명령 팔레트 ...** 로 이동 하 고 Azure를 입력 **합니다.** 로그인 합니다.

    ![Visual Studio Code 로그인에 대 한 Spark & Hive 도구](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)

2. 로그인 지침에 따라 azure에 로그인 합니다. 연결 되 면 Azure 계정 이름이 Visual Studio Code 창의 아래쪽에 있는 상태 표시줄에 표시 됩니다.  
  

## <a name="link-a-cluster"></a>클러스터 연결

### <a name="link-azure-hdinsight"></a>링크나 Azure HDInsight

[Apache Ambari](https://ambari.apache.org/) 관리형 사용자 이름을 사용하여 정상적인 클러스터를 연결하거나, 도메인 사용자 이름(예: user1@contoso.com)을 사용하여 Enterprise Security Pack 보안 Hadoop 클러스터를 연결할 수 있습니다.

1. 메뉴 모음에서 **보기** > **명령 팔레트 ...** 로 이동 하 여 Spark/Hive를 **입력 합니다. Link a Cluster**를 입력합니다.

   ![클러스터 연결 명령](./media/hdinsight-for-vscode/link-cluster-command.png)

2. 연결된 클러스터 유형 **Azure HDInsight**를 선택합니다.

3. HDInsight 클러스터 URL을 입력합니다.

4. Ambari 사용자 이름을 입력합니다. 기본값은 **admin**입니다.

5. Ambari 암호를 입력합니다.

6. 클러스터 유형을 선택합니다.

7. 클러스터의 표시 이름을 설정 합니다 (선택 사항).

8. 확인을 위해 **출력** 보기를 검토합니다.

   > [!NOTE]  
   > 클러스터가 Azure 구독 및 연결된 클러스터 모두에 로그인되어 있으면, 연결된 사용자 이름 및 암호가 사용됩니다.  


### <a name="link-generic-livy-endpoint"></a>링크나 일반 Livy 엔드포인트

1. 메뉴 모음에서 **보기** > **명령 팔레트 ...** 로 이동 하 여 Spark/Hive를 **입력 합니다. Link a Cluster**를 입력합니다.

2. 연결된 클러스터 유형 **일반 Livy 엔드포인트**를 선택합니다.

3. 일반 Livy 엔드포인트(예: http\://10.172.41.42:18080)를 입력합니다.

4. 인증 형식 **기본** 또는 **없음**을 선택합니다.  **기본**인 경우 다음을 수행합니다.  
    &emsp;a. Ambari 사용자 이름을 입력합니다. 기본값은 **admin**입니다.  
    &emsp;b. Ambari 암호를 입력합니다.

5. 확인을 위해 **출력** 보기를 검토합니다.

## <a name="list-clusters"></a>클러스터 나열

1. 메뉴 모음에서 **보기** > **명령 팔레트 ...** 로 이동 하 여 Spark/Hive를 **입력 합니다. List Cluster**를 입력합니다.

2. 원하는 구독을 선택합니다.

3. **출력** 보기를 검토합니다.  연결된 클러스터와 Azure 구독 아래의 모든 클러스터가 보기에 표시됩니다.

    ![기본 클러스터 구성 설정](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="set-default-cluster"></a>기본 클러스터 설정

1. [이전에](#open-work-folder) 만든 **HDexample** 폴더가 닫힌 경우 다시 엽니다.  

2. [이전에](#open-work-folder) 만든 **HelloWorld.hql** 파일을 선택하면 스크립트 편집기에서 열립니다.

3. 스크립트 편집기를 마우스 오른쪽 단추로 클릭 하 고 **Spark/Hive를 선택 합니다. Set Default Cluster**를 선택합니다.  

4. 아직 수행 하지 않은 경우 Azure 계정에 [연결](#connect-to-azure-account) 하거나 클러스터를 연결 합니다.

5. 현재 스크립트 파일에 대한 기본 클러스터로 사용할 클러스터를 선택합니다. **.VSCode\settings.json** 구성 파일이 자동으로 업데이트됩니다. 

   ![기본 클러스터 구성 설정](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)


## <a name="submit-interactive-hive-queries-hive-batch-scripts"></a>대화형 Hive 쿼리, Hive 배치 스크립트 제출

Visual Studio Code Spark & Hive 도구를 사용 하 여 대화형 Hive 쿼리 및 Hive 배치 스크립트를 클러스터에 제출할 수 있습니다.

1. [이전에](#open-work-folder) 만든 **HDexample** 폴더가 닫힌 경우 다시 엽니다.  

2. [이전에](#open-work-folder) 만든 **HelloWorld.hql** 파일을 선택하면 스크립트 편집기에서 열립니다.


3. 다음 코드를 복사하여 Hive 파일에 붙여넣은 후 파일을 저장합니다.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```

4. 아직 수행 하지 않은 경우 Azure 계정에 [연결](#connect-to-azure-account) 하거나 클러스터를 연결 합니다.

5. 스크립트 편집기를 마우스 오른쪽 단추로 클릭 하 **고 Hive:를 선택 합니다. 대화형** 으로 쿼리를 제출 하거나 **Ctrl + Alt + I**바로 가기를 사용 합니다.  Hive **선택: 일괄** 처리를 사용 하 여 스크립트를 제출 하거나 **Ctrl + Alt + H**바로 가기를 사용 합니다.  

6. 기본 클러스터를 지정하지 않은 경우 클러스터를 선택합니다. 이 도구에서는 상황에 맞는 메뉴를 사용하여 전체 스크립트 파일 대신 코드 블록을 제출할 수도 있습니다. 잠시 후 새 탭에 쿼리 결과가 표시됩니다.

   ![대화형 Hive 결과](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - **결과** 패널: 전체 결과를 CSV, JSON 또는 Excel 파일로 로컬 경로에 저장하거나 여러 줄을 선택할 수 있습니다.

    - **메시지** 패널: **줄** 번호를 선택하면 실행 중인 스크립트의 첫 번째 줄로 이동합니다.

## <a name="submit-interactive-pyspark-queries"></a>대화형 PySpark 쿼리 제출

다음 단계를 수행 하 여 대화형 PySpark 쿼리를 제출할 수 있습니다.

1. [이전에](#open-work-folder) 만든 **HDexample** 폴더가 닫힌 경우 다시 엽니다.  

2. [이전](#open-work-folder) 단계에 따라 새 파일 **HelloWorld.py**를 만듭니다.

3. 다음 코드를 복사하여 스크립트 파일에 붙여넣습니다.
   ```python
   from operator import add
   lines = spark.read.text("/HdiSamples/HdiSamples/FoodInspectionData/README").rdd.map(lambda r: r[0])
   counters = lines.flatMap(lambda x: x.split(' ')) \
                .map(lambda x: (x, 1)) \
                .reduceByKey(add)

   coll = counters.collect()
   sortedCollection = sorted(coll, key = lambda r: r[1], reverse = True)

   for i in range(0, 5):
        print(sortedCollection[i])
   ```

4. 아직 수행 하지 않은 경우 Azure 계정에 [연결](#connect-to-azure-account) 하거나 클러스터를 연결 합니다.

5. 모든 코드를 선택 하 고 스크립트 편집기를 마우스 오른쪽 단추로 클릭 **한 다음 Spark를 선택 합니다. PySpark Interactive**를 선택하여 쿼리를 제출하거나 바로 가기 **Ctrl+Alt+I**를 사용합니다.

   ![pyspark 대화형 상황에 맞는 메뉴](./media/hdinsight-for-vscode/pyspark-interactive-right-click.png)

6. 기본 클러스터를 지정하지 않은 경우 클러스터를 선택합니다. 몇 분 후에 **Python 대화형** 결과가 새 탭에 나타납니다. 이 도구에서는 상황에 맞는 메뉴를 사용하여 전체 스크립트 파일 대신 코드 블록을 제출할 수도 있습니다. 

   ![pyspark 대화형 python 대화형 창](./media/hdinsight-for-vscode/pyspark-interactive-python-interactive-window.png) 

7. **"%% Info"** 를 입력 한 다음 **Shift + enter** 를 눌러 작업 정보를 확인 합니다. (옵션)

   ![작업 정보 보기](./media/hdinsight-for-vscode/pyspark-interactive-view-job-information.png)

8. 이 도구는 **SPARK SQL** 쿼리도 지원 합니다.

   ![Pyspark 대화형 보기 결과](./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png)

   제출 상태는 쿼리를 실행할 때 아래쪽 상태 표시줄의 왼쪽에 표시 됩니다. 상태가 **PySpark 커널(작업 중)** 이면 다른 쿼리를 제출하지 마세요.  

   > [!NOTE] 
   >
   > 설정에서 **Python 확장을 사용 하도록** 설정 하지 않은 경우 (기본 설정이 선택 됨) 제출 된 pyspark 상호 작용 결과는 이전 창을 사용 합니다.
   >
   > ![pyspark 대화형 python 확장 사용 안 함](./media/hdinsight-for-vscode/pyspark-interactive-python-extension-disabled.png)


## <a name="submit-pyspark-batch-job"></a>PySpark 배치 작업 제출

1. [이전에](#open-work-folder) 만든 **HDexample** 폴더가 닫힌 경우 다시 엽니다.  

2. [이전](#open-work-folder) 단계에 따라 새 파일 **BatchFile.py**를 만듭니다.

3. 다음 코드를 복사하여 스크립트 파일에 붙여넣습니다.

    ```python
    from __future__ import print_function
    import sys
    from operator import add
    from pyspark.sql import SparkSession
    if __name__ == "__main__":
        spark = SparkSession\
            .builder\
            .appName("PythonWordCount")\
            .getOrCreate()
    
        lines = spark.read.text('/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv').rdd.map(lambda r: r[0])
        counts = lines.flatMap(lambda x: x.split(' '))\
                    .map(lambda x: (x, 1))\
                    .reduceByKey(add)
        output = counts.collect()
        for (word, count) in output:
            print("%s: %i" % (word, count))
        spark.stop()
    ```

4. 아직 수행 하지 않은 경우 Azure 계정에 [연결](#connect-to-azure-account) 하거나 클러스터를 연결 합니다.

5. 스크립트 편집기를 마우스 오른쪽 단추로 클릭 한 다음 Spark **를 선택 합니다. PySpark Batch**를 선택하거나 바로 가기 **Ctrl + Alt + H**를 사용합니다. 

6. PySpark 작업을 제출할 클러스터를 선택합니다. 

   ![Python 작업 결과 제출](./media/hdinsight-for-vscode/submit-pythonjob-result.png) 

Python 작업을 제출한 후 전송 로그가 Visual Studio Code의 **출력** 창에 나타납니다. **Spark UI URL** 및 **Yarn UI URL**도 표시됩니다. 웹 브라우저에서 URL을 열어 작업 상태를 추적할 수 있습니다.

## <a name="apache-livy-configuration"></a>Apache Livy 구성

[Apache Livy](https://livy.incubator.apache.org/) 구성은 지원되며, 작업 영역 폴더의 **.VSCode\settings.json**에서 설정할 수 있습니다. 현재 Livy 구성은 Python 스크립트만 지원합니다. 자세한 내용은 [Livy 추가 정보](https://github.com/cloudera/livy/blob/master/README.rst )를 참조하세요.

<a id="triggerlivyconf"></a>**Livy 구성을 트리거하는 방법**

방법 1  
1. 메뉴 모음에서 **파일** > **기본 설정** > **설정**으로 이동합니다.  
2. **검색 설정** 텍스트 상자에 **HDInsight Job Sumission: Livy Conf**를 입력합니다.  
3. 관련 검색 결과에 대해 **settings.json에서 편집**을 선택합니다.

방법 2   
파일을 제출합니다. .vscode 폴더가 작업 폴더에 자동으로 추가됩니다. **.vscode\settings.json**을 클릭하여 Livy 구성을 찾을 수 있습니다.

+ 프로젝트 설정:

    ![Livy 구성](./media/hdinsight-for-vscode/hdi-livyconfig.png)

>[!NOTE]
>**driverMomory** 및 **executorMomry** 설정의 경우 단위가 있는 값을 설정합니다(예: 1g 또는 1024m). 

+ 지원되는 Livy 구성:   

    **POST/일괄 처리**   
    요청 본문

    | name | description | type | 
    | :- | :- | :- | 
    | 파일 | 실행할 애플리케이션을 포함하는 파일 | 경로(필수) | 
    | proxyUser | 작업을 실행할 때 가장하는 사용자 | string | 
    | className | 애플리케이션 Java/Spark 주 클래스 | string |
    | args | 애플리케이션에 대한 명령줄 인수 | 문자열 목록 | 
    | jars | 이 세션에 사용할 jars | 문자열 목록 | 
    | pyFiles | 이 세션에 사용할 Python 파일 | 문자열 목록 |
    | files | 이 세션에 사용할 파일 | 문자열 목록 |
    | driverMemory | 드라이버 프로세스에 사용할 메모리의 양 | string |
    | driverCores | 드라이버 프로세스에 사용할 코어 수 | ssNoversion |
    | executorMemory | 실행기 프로세스당 사용할 메모리의 양 | string |
    | executorCores | 각 실행기에 사용할 코어 수 | ssNoversion |
    | numExecutors | 이 세션에 대해 시작할 실행기 수 | ssNoversion |
    | archives | 이 세션에 사용할 아카이브 | 문자열 목록 |
    | queue | 제출되는 YARN 큐의 이름 | string |
    | name | 이 세션의 이름 | string |
    | conf | Spark 구성 속성 | key=val의 맵 |

    응답 본문   
    만든 일괄 처리 개체입니다.

    | name | description | type | 
    | :- | :- | :- | 
    | id | 세션 ID | ssNoversion | 
    | appId | 이 세션의 애플리케이션 ID |  String |
    | appInfo | 자세한 애플리케이션 정보 | key=val의 맵 |
    | log | 로그 줄 | 문자열 목록 |
    | state |   일괄 처리 상태 | string |

>[!NOTE]
>스크립트를 제출하면 할당된 Livy 구성이 출력 창에 표시됩니다.

## <a name="integrate-with-azure-hdinsight-from-explorer"></a>탐색기에서 Azure HDInsight와 통합

**Azure HDInsight**가 탐색기 뷰에 추가되었습니다. **Azure HDInsight**를 통해 직접 클러스터를 찾고 관리할 수 있습니다.

1. 아직 수행 하지 않은 경우 Azure 계정에 [연결](#connect-to-azure-account) 하거나 클러스터를 연결 합니다.

2. 메뉴 모음에서 **보기** > **탐색기**로 이동합니다.

3. 왼쪽 창에서 **AZURE HDINSIGHT**를 펼칩니다.  사용 가능한 구독 및 클러스터(Spark, Hadoop 및 HBase가 지원됨)가 나열됩니다. 

   ![Azure HDInsight 구독](./media/hdinsight-for-vscode/hdi-azure-hdinsight-subscription.png)

4. 클러스터를 확장하여 Hive 메타데이터 데이터베이스 및 테이블 스키마를 확인합니다.

   ![Azure HDInsight 클러스터](./media/hdinsight-for-vscode/hdi-azure-hdinsight-cluster.png)


## <a name="preview-hive-table"></a>Hive 테이블 미리 보기
**Azure HDInsight** 탐색기를 통해 클러스터의 Hive 테이블을 직접 미리 볼 수 있습니다.
1. Azure 계정에 아직 연결하지 않은 경우 [연결](#connect-to-azure-account)합니다.

2. 맨 왼쪽 열에서 **Azure** 아이콘을 클릭 합니다.

3. 왼쪽 창에서 **AZURE HDINSIGHT**를 펼칩니다. 사용 가능한 구독과 클러스터가 나열 됩니다.

4. 클러스터를 확장하여 Hive 메타데이터 데이터베이스 및 테이블 스키마를 확인합니다.

5. Hive 테이블 (예: hivesampletable)을 마우스 오른쪽 단추로 클릭 합니다. **미리 보기**를 선택 합니다. 

   ![Visual studio code 미리 보기 hive 테이블의 Spark & Hive](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-hive-table.png)

6. **미리 보기 결과** 창이 열립니다.

   ![Visual studio code 미리 보기 결과 창에 대 한 Spark & Hive](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-results-window.png)
   
- **결과** 패널

   전체 결과를 CSV, JSON 또는 Excel 파일로 로컬 경로에 저장하거나 여러 줄을 선택할 수 있습니다.

- **메시지** 패널
   1. 테이블의 행 수가 100 행 보다 크면 메시지는 다음과 같이 표시 됩니다. **Hive 테이블에 대해 처음 100 개 행이 표시 됩니다**.
   2. 테이블의 행 수가 100 행 보다 작거나 같으면 메시지에 다음이 표시 됩니다. **60 행이 Hive 테이블에 대해 표시 됩니다**.
   3. 테이블에 내용이 없는 경우 다음 메시지가 표시 됩니다. **Hive 테이블에 대해 0 개의 행이 표시 됩니다**.

>[!NOTE]
>
>Linux에서 xclip을 설치 하 여 테이블 데이터 복사를 사용 하도록 설정 합니다.
>
>![Linux에서 Visual Studio code에 대 한 Spark & Hive](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-linux-install-xclip.png)
## <a name="additional-features"></a>추가 기능

Visual Studio Code에 대 한 Spark & Hive는 다음과 같은 기능을 지원 합니다.

- **IntelliSense 자동 완성**. 키워드, 메서드, 변수 등에 대한 추천 항목이 팝업됩니다. 다음과 같이 다양한 아이콘이 여러 형식의 개체를 나타냅니다.

    ![Visual Studio Code IntelliSense 개체 형식에 대 한 Spark & Hive 도구](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **IntelliSense 오류 표식**. 언어 서비스에서 Hive 스크립트에 대한 편집 오류에 밑줄을 표시합니다.     
- **구문 강조 표시**. 언어 서비스는 변수, 키워드, 데이터 형식, 함수 등을 구분하기 위해 서로 다른 색상을 사용합니다. 

    ![Visual Studio Code 구문 강조 표시를 위한 Spark & Hive 도구](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="reader-only-role"></a>판독기 전용 역할

클러스터 **판독기** **전용** **역할이** 있는 사용자는 더 이상 HDInsight 클러스터에 작업을 제출 하거나 Hive 데이터베이스를 볼 수 없습니다. 클러스터 관리자에 게 문의 하 여 [Azure Portal](https://ms.portal.azure.com/)에서 [ **HDInsight** **클러스터** **운영자** ](https://docs.microsoft.com/azure/hdinsight/hdinsight-migrate-granular-access-cluster-configurations#add-the-hdinsight-cluster-operator-role-assignment-to-a-user) 에 게 역할을 업그레이드 하십시오. Ambari 자격 증명을 알고 있는 경우 아래 지침에 따라 클러스터를 수동으로 연결할 수 있습니다.

### <a name="browse-hdinsight-cluster"></a>HDInsight 클러스터 찾아보기  

Azure HDInsight 탐색기를 클릭 하 여 HDInsight 클러스터를 확장 하는 경우 클러스터에 대 한 독자 전용 역할인 경우 클러스터를 연결 하 라는 메시지가 표시 됩니다. Ambari 자격 증명을 통해 클러스터에 연결 하려면 다음 단계를 수행 합니다. 

### <a name="submit-job-to-hdinsight-cluster"></a>HDInsight 클러스터에 작업 제출

HDInsight 클러스터에 작업을 제출 하는 경우 클러스터에 대 한 독자 전용 역할인 경우 클러스터를 연결 하 라는 메시지가 표시 됩니다. Ambari 자격 증명을 통해 클러스터에 연결 하려면 다음 단계를 수행 합니다. 

### <a name="link-to-cluster"></a>클러스터에 연결

1.  Ambari 사용자 이름을 입력 합니다. 
2.  Ambari 사용자 암호를 입력 합니다.

   ![사용자 이름 Visual Studio Code Spark & Hive 도구](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-username.png)

   ![Visual Studio Code 암호를 위한 Spark & Hive 도구](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-password.png)

> [!NOTE]
>
>Spark/Hive를 사용할 수 있습니다. 클러스터를 나열 하 여 연결 된 클러스터를 확인 합니다.
>
>![연결 된 Visual Studio Code 판독기 용 Spark & Hive 도구](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="azure-data-lake-storage-gen2-adls-gen2"></a>Azure Data Lake Storage Gen2 (ADLS Gen2)

### <a name="browse-an-adls-gen2-account"></a>ADLS Gen2 계정 찾아보기

Azure HDInsight 탐색기를 클릭 하 여 ADLS Gen2 계정을 확장 하는 경우 Azure 계정에 Gen2 저장소에 대 한 액세스 권한이 없으면 저장소 **액세스 키** 를 입력 하 라는 메시지가 표시 됩니다. 액세스 키의 유효성을 검사 하면 ADLS Gen2 계정이 자동으로 확장 됩니다. 

### <a name="submit-jobs-to-hdinsight-cluster-with-adls-gen2"></a>ADLS Gen2를 사용 하 여 HDInsight 클러스터에 작업 제출

ADLS Gen2를 사용 하 여 HDInsight 클러스터에 작업을 제출 하는 경우 Azure 계정에 Gen2 저장소에 대 한 쓰기 권한이 없는 경우 저장소 **액세스 키** 를 입력 하 라는 메시지가 표시 됩니다.  액세스 키의 유효성을 검사 하면 작업이 성공적으로 제출 됩니다. 

![Visual Studio Code AccessKey 용 Spark & Hive 도구](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-accesskey.png)   

> [!NOTE]
> 
>Azure Portal에서 저장소 계정에 대 한 액세스 키를 가져올 수 있습니다. 자세한 내용은 [액세스 키 보기 및 복사](https://docs.microsoft.com/azure/storage/common/storage-account-manage#access-keys)를 참조 하세요.

## <a name="unlink-cluster"></a>클러스터 링크 해제

1. 메뉴 모음에서 **보기** > **명령 팔레트 ...** 로 이동한 다음 Spark/Hive를 입력 **합니다. Unlink a Cluster**를 입력합니다.  

2. 링크 해제할 클러스터를 선택합니다.  

3. 확인을 위해 **출력** 보기를 검토합니다.  

## <a name="sign-out"></a>로그아웃  

메뉴 모음에서 **보기** > **명령 팔레트 ...** 로 이동한 다음 Azure를 입력 **합니다. 로그 아웃**합니다.


## <a name="next-steps"></a>다음 단계
Visual Studio Code에 대 한 Spark & Hive 사용에 대 한 데모 비디오는 [spark & hive for Visual Studio Code](https://go.microsoft.com/fwlink/?linkid=858706) 를 참조 하세요.
