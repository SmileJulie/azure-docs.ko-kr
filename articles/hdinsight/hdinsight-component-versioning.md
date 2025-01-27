---
title: Apache Hadoop 구성 요소 및 버전 - Azure HDInsight
description: HDInsight의 Apache Hadoop 구성 요소 및 버전과 Hortonworks Data Platform의 이 클라우드 배포판에서 사용할 수 있는 서비스 수준에 대해 알아봅니다.
keywords: hadoop 버전, hadoop 에코시스템 구성 요소, hadoop 구성 요소, hadoop 버전을 확인하는 방법
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 06/07/2019
ms.openlocfilehash: 72be96e89cd46267d0c0795bd5121de76e48cf6f
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67484091"
---
# <a name="what-are-the-apache-hadoop-components-and-versions-available-with-hdinsight"></a>HDInsight에서 사용할 수 있는 Apache Hadoop 구성 요소 및 버전은?

Enterprise Security Package뿐만 아니라 Microsoft Azure HDInsight의 [Apache Hadoop](https://hadoop.apache.org/) 에코시스템 구성 요소 및 버전에 대해 알아봅니다. 또한 HDInsight에서 Hadoop 구성 요소 버전을 확인하는 방법을 알아봅니다.

## <a name="apache-hadoop-components-available-with-different-hdinsight-versions"></a>각 HDInsight 버전에서 사용할 수 있는 Apache Hadoop 구성 요소

Azure HDInsight는 언제든 배포할 수 있는 여러 Hadoop 클러스터 버전을 지원합니다. 각 버전을 선택하면 특정 버전의 HDP 배포 및 배포에 포함된 구성 요소 집합이 만들어집니다. 2017년 4월 4일을 기준으로 Azure HDInsight에서 사용하는 기본 클러스터 버전은 3.6이지만 HDP 2.6을 기반으로 합니다.

HDInsight 클러스터 버전과 연결된 구성 요소 버전은 다음 표에 나열되어 있습니다. 

> [!NOTE]  
> HDInsight 서비스의 기본 버전은 예고 없이 변경될 수 있습니다. 버전 종속성이 있는 경우 Azure PowerShell 및 Azure 클래식 CLI를 사용하여 .NET SDK에서 클러스터를 만들 때 HDInsight 버전을 지정합니다.

| 구성 요소 | HDInsight 4.0 | HDInsight 3.6(기본값) | HDInsight 3.5 | HDInsight 3.4 | HDInsight 3.3 | HDInsight 3.2 |
|---------------------------|---------------|-----------------------------|---------------|---------------|---------------|----------------------|
| Hortonworks Data Platform | 3.0 | 2.6 | 2.5 | 2.4 | 2.3 | 2.2 |
| Apache Hadoop 및 YARN | 3.1.1 | 2.7.3 | 2.7.3 | 2.7.1 | 2.7.1 | 2.6.0 |
| Apache Tez | 0.9.1 | 0.7.0 | 0.7.0 | 0.7.0 | 0.7.0 | 0.5.2 |
| Apache Pig | 0.16.0 | 0.16.0 | 0.16.0 | 0.15.0 | 0.15.0 | 0.14.0 |
| Apache Hive 및 HCatalog | - | 1.2.1 | 1.2.1 | 1.2.1 | 1.2.1 | 0.14.0 |
| Apache Hive | 3.1.0 | 2.1.0 | - | - | - | - |
| Apache Tez Hive2 | - | 0.8.4 | - | - | - | - |
| Apache Ranger | 1.1.0 | 0.7.0 | 0.6.0 | - | - | - |
| Apache HBase | 2.0.1 | 1.1.2 | 1.1.2 | 1.1.2 | 1.1.1 | 0.98.4 |
| Apache Sqoop | 1.4.7 | 1.4.6 | 1.4.6 | 1.4.6 | 1.4.6 | 1.4.5 |
| Apache Oozie | 4.3.1 | 4.2.0 | 4.2.0 | 4.2.0 | 4.2.0 | 4.1.0 |
| Apache Zookeeper | 3.4.6 | 3.4.6 | 3.4.6 | 3.4.6 | 3.4.6 | 3.4.6 |
| Apache Storm | - | 1.1.0 | 1.0.1 | 0.10.0 | 0.10.0 | 0.9.3 |
| Apache Mahout | - | 0.9.0+ | 0.9.0+ | 0.9.0+ | 0.9.0+ | 0.9.0 |
| Apache Phoenix | 5 | 4.7.0 | 4.7.0 | 4.4.0 | 4.4.0 | 4.2.0 |
| Apache Spark | 2.3.1, 2.4 | 2.3.0, 2.2.0, 2.1.0 | 1.6.2, 2.0 | 1.6.0 | 1.5.2 | 1.3.1(Windows만 해당) |
| Apache Livy | 0.5 | 0.4, 0.4, 0.3 | 0.3 | 0.3 | 0.2 | - |
| Apache Kafka | 1.1.1, 2.1 | 1.1, 1.0 *(아래 참고 사항 참조) | 0.10.0 | 0.9.0 | - | - |
| Apache Ambari | 2.7.0 | 2.6.0 | 2.4.0 | 2.2.1 | 2.1.0 | - |
| Apache Zeppelin | 0.8.0 | 0.7.0 | - | - | - | - |
| Mono | 4.2.1 | 4.2.1 | 4.2.1 | 3.2.8 | - | - |

> [!NOTE]
> 시스템 성능 고려 사항으로 인해 2019년 3월에 Kafka 0.10 버전에 대한 지원이 만료되었습니다.

## <a name="check-for-current-hadoop-component-version-information"></a>현재 Hadoop 구성 요소 버전 정보 확인

HDInsight 클러스터 버전과 연결된 Hadoop 에코시스템 구성 요소 버전은 HDInsight에 대한 업데이트로 변경될 수 있습니다. Hadoop 구성 요소를 확인하고 클러스터에 대해 사용 중인 버전을 확인하려면 Ambari REST API를 사용합니다. **GetComponentInformation** 명령은 서비스 구성 요소에 대한 정보를 검색합니다. 자세한 내용은 참조는 [Apache Ambari 설명서](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)합니다.

### <a name="release-notes"></a>릴리스 정보

최신 HDInsight 버전과 관련된 추가 릴리스 정보에 대해서는 [HDInsight 릴리스 정보](hdinsight-release-notes.md) 를 참조하세요.

## <a name="supported-hdinsight-versions"></a>지원되는 HDInsight 버전

다음 표에서는 HDInsight 버전을 보여줍니다. 각 HDInsight 버전에 해당하는 HDP 버전은 제품 출시 날짜와 함께 나열되어 있습니다. 알려진 경우 지원 만료 날짜와 사용 중지 날짜도 나와 있습니다.

### <a name="available-versions"></a>사용 가능한 버전

다음 표에는 PowerShell 및 .NET SDK와 같은 다른 배포 메서드를 비롯하여 Azure Portal에서 사용할 수 있는 HDInsight 버전이 나열되어 있습니다.

| HDInsight 버전 | HDP 버전 | VM OS | 릴리스 날짜 | 지원 만료 날짜 | 사용 중지 날짜 | 고가용성 |  Azure Portal에 대한 사용 가용성 | 
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 4.0 |HDP 3.0 |Ubuntu 16.0.4 LTS |2018년 9월 24일 | | |예 |예 |
| HDInsight 3.6 |HDP 2.6 |Ubuntu 16.0.4 LTS |2017년 4월 4일 | 2020 년 6 월 30 년 |2020 년 12 월 31 |예 |예 |


> [!NOTE]  
> 버전에 대한 지원이 만료되면 Microsoft Azure Portal을 통해 사용하지 못할 수도 있습니다. 그러나 클러스터 버전은 버전 사용 중지 날짜까지 Windows PowerShell [New-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster) 명령의 `Version` 매개 변수 및 .NET SDK를 통해 계속 사용할 수 있습니다.
>

### <a name="retired-versions"></a>사용 중지된 버전

다음 표에는 현재 Azure Portal에서 사용할 수 **없는** HDInsight 버전이 나열되어 있습니다.

| HDInsight 버전 | HDP 버전 | VM OS | 릴리스 날짜 | 지원 만료 날짜 | 사용 중지 날짜 | 고가용성 |  Azure Portal에 대한 사용 가용성 | 
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 3.5 <br> (비 Spark) |HDP 2.5 |Ubuntu 16.0.4 LTS |2016년 9월 30일 |2017년 9월 5일 |2018년 6월 28일 |예 |아닙니다. |
| HDInsight 3.4 |HDP 2.4 |Ubuntu 14.0.4 LTS |2016년 3월 29일 |2016년 12월 29일 |2018년 1월 9일 |예 |아닙니다. |
| HDInsight 3.3 |HDP 2.3 |Windows Server 2012 R2 |2015년 12월 2일 |2016년 6월 27일 |2018년 7월 31일 |예 |아닙니다. |
| HDInsight 3.3 |HDP 2.3 |Ubuntu 14.0.4 LTS |2015년 12월 2일 |2016년 6월 27일 |2017년 7월 31일 |예 |아닙니다. |
| HDInsight 3.2 |HDP 2.2 |Ubuntu 12.04 LTS 또는 Windows Server 2012 R2 |2015년 2월 18일 |2016년 3월 1일 |2017년 4월 1일 |예 |아닙니다. |
| HDInsight 3.1 |HDP 2.1 |Windows Server 2012 R2 |2014년 6월 24일 |2015년 5월 18일 |2016년 6월 30일 |예 |아닙니다. |
| HDInsight 3.0 |HDP 2.0 |Windows Server 2012 R2 |2014년 2월 11일 |2014년 9월 17일 |2015년 6월 30일 |예 |아닙니다. |
| HDInsight 2.1 |HDP 1.3 |Windows Server 2012 R2 |2013년 10월 28일 |2014년 5월 12일 |2015년 5월 31일 |예 |아닙니다. |
| HDInsight 1.6 |HDP 1.1 | |2013년 10월 28일 |2014년 4월 26일 |2015년 5월 31일 |아닙니다. |아닙니다. |

> [!NOTE]  
> 헤드 노드가 2개 있는 고가용성 클러스터는 기본적으로 HDInsight 버전 2.1 이상에 배포됩니다. HDInsight 버전 1.6 클러스터에서는 사용할 수 없습니다.

## <a name="enterprise-security-package-for-hdinsight"></a>HDInsight용 엔터프라이즈 보안 패키지

엔터프라이즈 보안은 HDInsight 클러스터에서 클러스터 워크플로 생성의 일부로 추가할 수 있는 선택적 패키지입니다. 엔터프라이즈 보안 패키지는 다음을 지원합니다.

- 인증을 위한 Active Directory와 통합

    이전에 로컬 관리 사용자 및 로컬 SSH 사용자인 경우에만 HDInsight 클러스터를 만들 수 있었습니다. 로컬 관리 사용자는 모든 파일, 폴더, 테이블 및 열에 액세스할 수 있습니다.  엔터프라이즈 보안 패키지에서 HDInsight 클러스터를 고유한 Active Directory와 통합하여 역할 기반 액세스 제어를 사용할 수 있습니다. 여기에는 온-프레미스 Active Directory, Azure Active Directory Domain Services 또는 IaaS 가상 머신의 Active Directory가 포함됩니다. 클러스터의 도메인 관리자는 사용자가 클러스터에 액세스하도록 고유한 회사 (도메인) 사용자 이름 및 암호를 부여할 수 있습니다. 

    자세한 내용은 다음을 참조하세요.

    - [도메인에 가입된 HDInsight 클러스터를 사용하는 Apache Hadoop 보안 소개](./domain-joined/apache-domain-joined-introduction.md)
    - [HDInsight에서 Azure 도메인에 가입된 Apache Hadoop 클러스터 계획](./domain-joined/apache-domain-joined-architecture.md)
    - [도메인 가입 샌드박스 환경 구성](./domain-joined/apache-domain-joined-configure.md)
    - [Azure Active Directory Domain Services를 사용하여 도메인에 가입된 HDInsight 클러스터 구성](./domain-joined/apache-domain-joined-configure-using-azure-adds.md)

- 데이터에 대한 권한 부여

  - Hive, Spark SQL 및 Yarn 큐의 권한 부여를 위해 Apache Ranger와 통합합니다.
  - 파일 및 폴더에 대한 액세스 제어를 설정할 수 있습니다.

    자세한 내용은 다음을 참조하세요.

  - [도메인에 가입된 HDInsight에서 Apache Hive 정책 구성](./domain-joined/apache-domain-joined-run-hive.md)

- 감사 로그를 보고 액세스 및 구성된 정책을 모니터링합니다. 

### <a name="supported-cluster-types"></a>지원되는 클러스터 형식

현재 다음과 같은 클러스터 형식만이 엔터프라이즈 보안 패키지를 지원합니다.

- Hadoop(HDInsight 3.6에만 해당)
- Spark
- 대화형 쿼리

### <a name="support-for-azure-data-lake-storage"></a>Azure Data Lake Storage에 대한 지원

엔터프라이즈 보안 패키지는 Azure Data Lake Storage를 기본 스토리지 및 추가 기능 스토리지 둘 다로 사용할 수 있도록 지원합니다.

### <a name="pricing-and-service-level-agreement"></a>가격 책정 및 서비스 수준 계약

엔터프라이즈 보안 패키지의 가격 책정 및 SLA에 대한 자세한 내용은 [HDInsight 가격 책정](https://azure.microsoft.com/pricing/details/hdinsight/)을 참조하세요.


## <a name="service-level-agreement-for-hdinsight-cluster-versions"></a>HDInsight 클러스터 버전의 서비스 수준 약정

SLA(서비스 수준 약정)는 _지원 기간_ 면에서 정의됩니다. 지원 기간은 Microsoft 고객 지원에서 HDInsight 클러스터 버전을 지원하는 기간을 나타냅니다. 버전의 _지원 만료 날짜_가 날짜를 지난 경우 HDInsight 클러스터는 지원 기간이 종료된 것입니다. 새로운 X+1 버전이 사용 가능해진 후에 지정된 HDInsight 버전의 지원 만료 날짜는 다음 수식 중 후자로 계산됩니다.  

* 수식 1: HDInsight 클러스터 버전 X가 릴리스된 날짜에 180일을 추가합니다.
* 수식 2: HDInsight 클러스터 버전 X+1이 Azure Portal에서 사용 가능한 날짜에 90일을 추가합니다.

_사용 중지 날짜_는 HDInsight에서 해당 클러스터 버전을 만들 수 없게 되는 날짜입니다. 2017년 7월 31일부터 사용 중지 날짜 이후에는 HDInsight 클러스터의 크기를 조정할 수 없습니다. 

> [!NOTE]  
> HDInsight Windows 클러스터(버전 2.1, 3.0, 3.1, 3.2 및 3.3 포함)는 Azure 게스트 OS 제품군 버전 4에서 실행되고 64비트 버전의 Windows Server 2012 R2를 사용합니다. Azure 게스트 OS 제품군 버전 4는 .NET Framework 버전 4.0, 4.5, 4.5.1 및 4.5.2를 지원합니다.

## <a name="hortonworks-release-notes-associated-with-hdinsight-versions"></a>HDInsight 버전과 관련된 Hortonworks 릴리스 정보

섹션에서는 HDInsight에서 사용되는 Hortonworks Data Platform 배포 및 Apache 구성 요소의 릴리스 정보에 대한 링크를 제공합니다.
* HDInsight 클러스터 버전 4.0에서는 [Hortonworks Data Platform 3.0](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/release-notes/content/relnotes.html)을 기반으로 하는 Hadoop 배포를 사용합니다.
* HDInsight 클러스터 버전 3.6에서는 [Hortonworks Data Platform 2.6](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.0/bk_release-notes/content/ch_relnotes.html)을 기반으로 하는 Hadoop 배포를 사용합니다.
* HDInsight 클러스터 버전 3.5에서는 [Hortonworks Data Platform 2.5](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.5.0/bk_release-notes/content/ch_relnotes_v250.html)를 기반으로 하는 Hadoop 배포를 사용합니다. HDInsight 클러스터 버전 3.5는 Azure Portal에서 만들어진 _기본_ Hadoop 클러스터입니다.
* HDInsight 클러스터 버전 3.4에서는 [Hortonworks Data Platform 2.4](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html)를 기반으로 하는 Hadoop 배포를 사용합니다.
* HDInsight 클러스터 버전 3.3에서는 [Hortonworks Data Platform 2.3](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html)(영문)를 기반으로 하는 Hadoop 배포를 사용합니다.

  * [Apache Storm 릴리스 정보](https://storm.apache.org/2015/11/05/storm0100-released.html)는 Apache 웹 사이트에서 사용할 수 있습니다.
  * [Apache Hive 릴리스 정보](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12332384&styleName=Text&projectId=12310843)는 Apache 웹 사이트에서 사용할 수 있습니다.
* 기반이 되는 Hadoop 배포를 사용 하는 HDInsight 클러스터 버전 3.2 [Hortonworks Data Platform 2.2][hdp-2-2]합니다.

  * 특정 Apache 구성 요소의 릴리스 정보는 다음과 같습니다. [Hive 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450), [Pig 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954), [HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810), [Phoenix 4.2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581), [M/R 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180), [HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181), [YARN 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197), [Common](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179), [Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742), [Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486), [Storm 0.9.3](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112) 및 [Oozie 4.1.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620).
* 기반이 되는 Hadoop 배포를 사용 하는 HDInsight 클러스터 버전 3.1 [Hortonworks Data Platform 2.1.7][hdp-2-1-7]. HDInsight 3.1 clusters created before November, 7, 2014, are based on [Hortonworks Data Platform 2.1.1][hdp-2-1-1]합니다.
* 기반이 되는 Hadoop 배포를 사용 하는 HDInsight 클러스터 버전 3.0 [Hortonworks Data Platform 2.0][hdp-2-0-8]합니다.
* 기반이 되는 Hadoop 배포를 사용 하는 HDInsight 클러스터 버전 2.1 [Hortonworks Data Platform 1.3][hdp-1-3-0]합니다.
* 기반이 되는 Hadoop 배포를 사용 하는 HDInsight 클러스터 버전 1.6 [Hortonworks Data Platform 1.1][hdp-1-1-0]합니다.

## <a name="default-node-configuration-and-virtual-machine-sizes-for-clusters"></a>클러스터에 대한 기본 노드 구성 및 가상 머신 크기

다음 표에는 HDInsight 클러스터에 대한 기본 VM(가상 머신) 크기가 나와 있습니다.  이 차트는 PowerShell 또는 Azure CLI 스크립트를 만들어 HDInsight 클러스터를 배포할 때 사용할 VM 크기를 파악하는 데 필요합니다.

> [!IMPORTANT]  
> 클러스터에 필요한 작업자 노드 수가 32개를 초과하는 경우 최소한 코어 8개와 14GB RAM을 가진 헤드 노드 크기를 선택해야 합니다.

* 브라질 남부 및 일본 서부를 제외하고 지원되는 모든 지역:

|클러스터 유형|Hadoop은|HBase|대화형 쿼리|Storm|Spark|ML Server|Kafka|
|---|---|---|---|---|---|---|---|
|헤드: 기본 VM 크기|D12 v2|D12 v2|D13 v2|A3|D12 v2|D12 v2|D3v2|
|헤드: 권장되는 VM 크기|D3 v2|D3 v2|D13|A4 v2|D12 v2|D12 v2|A2M v2|
||D4 v2|D4 v2|D14|A8 v2|D13 v2|D13 v2|D3 v2|
||D12 v2|D12 v2|E16 v3|A2m v2|D14 v2|D14 v2|D4 v2|
||E4 v3|E4 v3|E32 v3|E4 v3|E4 v3|E4 v3|D12 v2|
|작업자: 기본 VM 크기|D4 v2|D4 v2|D14 v2|D3 v2|D13 v2|D4 v2|4개 D12 v2(broker당 2개 S30 디스크 포함)|
|작업자: 권장되는 VM 크기|D3 v2|D3 v2|D13|D3 v2|D4 v2|D4 v2|D13 v2|
||D4 v2|D4 v2|D14|D4 v2|D12 v2|D12 v2|DS12 v2|
||D12 v2|D12 v2|E16 v3|D12 v2|D13 v2|D13 v2|DS13 v2|
||E4 v3|E4 v3|E20 v3|E4 v3|D14 v2|D14 v2|E4 v3|
||||E32 v3||E16 v3|E16 v3|ES4 v3|
||||E64 v3||E20 v3|E20 v3|E8 v3|
||||||E32 v3|E32 v3|ES8 v3|
||||||E64 v3|E64 v3||
|Zookeeper: 기본 VM 크기||A4 v2|A4 v2|A4 v2||A2 v2|D3v2|
|Zookeeper: 권장되는 VM 크기||A4 v2||A2 v2|||A2M v2|
|||A8 v2||A4 v2|||D3 v2|
|||A2m v2||A8 v2|||E8 v3|
|ML Services: 기본 VM 크기||||||D4 v2||
|ML Services: 권장 VM 크기||||||D4 v2||
|||||||D12 v2||
|||||||D13 v2||
|||||||D14 v2||
|||||||E16 v3||
|||||||E20 v3||
|||||||E32 v3||
|||||||E64 v3||

* 브라질 남부 및 일본 서부만 해당(v2 크기 제외):

  | 클러스터 유형 | Hadoop은 | HBase | 대화형 쿼리 |Storm | Spark | ML 서비스 |
  | --- | --- | --- | --- | --- | --- | --- |
  | 헤드: 기본 VM 크기 |D12 |D12  | D13 |A3 |D12 |D12 |
  | 헤드: 권장되는 VM 크기 |D3,<br/> D4,<br/> D12 |D3,<br/> D4,<br/> D12  | D13,<br/> D14 |A3,<br/> A4,<br/> A5 |D12,<br/> D13,<br/> D14 |D12,<br/> D13,<br/> D14 |
  | 작업자: 기본 VM 크기 |D4 |D4  |  D14 |D3 |D13 |D4 |
  | 작업자: 권장되는 VM 크기 |D3,<br/> D4,<br/> D12 |D3,<br/> D4,<br/> D12  | D13,<br/> D14 |D3,<br/> D4,<br/> D12 |D4,<br/> D12,<br/> D13,<br/> D14 | D4,<br/> D12,<br/> D13,<br/> D14 |
  | Zookeeper: 기본 VM 크기 | |A4 v2 | A4 v2| A4 v2 | | A2 v2|
  | Zookeeper: 권장되는 VM 크기 | |A2,<br/> A3,<br/> A4 | |A2,<br/> A3,<br/> A4 | | |
  | ML Services: 기본 VM 크기 | | | | | |D4 |
  | ML Services: 권장 되는 VM 크기 | | | | | |D4,<br/> D12,<br/> D13,<br/> D14 |

> [!NOTE]
> - 헤드는 Storm 클러스터 유형에 대한 *Nimbus*로 알려져 있습니다.
> - 작업자는 Storm 클러스터 형식에 대한 *감독자*로 알려져 있습니다.
> - 작업자는 HBase 클러스터 형식에 대한 *지역*으로 알려져 있습니다.

## <a name="next-steps"></a>다음 단계
- [HDInsight의 Apache Hadoop, Spark 등에 대한 클러스터 설정](hdinsight-hadoop-provision-linux-clusters.md)
- [Windows PC의 HDInsight에서 Apache Hadoop 작업](hdinsight-hadoop-windows-tools.md)

[hdp-2-2]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.9/bk_HDP_RelNotes/content/ch_relnotes_v229.html

[hdp-2-1-7]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: https://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: https://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.1.1.16_1.html
