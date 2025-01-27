---
title: 스크립트 동작을 사용 하 여 Azure HDInsight 클러스터를 사용자 지정
description: '스크립트 동작을 사용하여 Linux 기반 HDInsight 클러스터에 사용자 지정 구성 요소를 추가합니다. 스크립트 동작은 클러스터 구성을 사용자 지정하거나 추가 서비스 및 유틸리티(예: Hue, Solr 또는 R)를 추가하는 데 사용할 수 있는 Bash 스크립트입니다.'
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/02/2019
ms.openlocfilehash: 7885b03e9f92fc8e8c5b2c78049760cbed8d4dc7
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67703972"
---
# <a name="customize-azure-hdinsight-clusters-by-using-script-actions"></a>스크립트 동작을 사용 하 여 Azure HDInsight 클러스터를 사용자 지정

Azure HDInsight는 사용자 지정 스크립트를 호출하여 클러스터를 사용자 지정하는 **스크립트 동작**이라는 구성 메서드를 제공합니다. 이러한 스크립트는 추가 구성 요소를 설치하고 구성 설정을 변경하는 데 사용합니다. 스크립트 작업은 클러스터를 만드는 중이거나 만든 후에 사용할 수 있습니다.

스크립트 작업을 Azure Marketplace에 HDInsight 애플리케이션으로 게시할 수도 있습니다. HDInsight 애플리케이션에 대한 자세한 내용은 [Azure Marketplace에 HDInsight 애플리케이션 게시](hdinsight-apps-publish-applications.md)를 참조하세요.

## <a name="permissions"></a>사용 권한

도메인 조인 HDInsight 클러스터를 사용하는 경우 클러스터에서 스크립트 동작을 사용할 때 다음 두 가지 Apache Ambari 권한이 필요합니다.

* **AMBARI.RUN\_CUSTOM\_COMMAND**. Ambari 관리자 역할은 기본적으로 이 권한을 가집니다.
* **CLUSTER.RUN\_CUSTOM\_COMMAND**. HDInsight 클러스터 관리자와 Ambari 관리자는 기본적으로 이 권한을 가집니다.

도메인 조인 HDInsight에서 권한을 사용하는 방법에 대한 자세한 내용은 [Enterprise Security Package를 사용하여 HDInsight 클러스터 관리](./domain-joined/apache-domain-joined-manage.md)를 참조하세요.

## <a name="access-control"></a>Access Control

Azure 구독의 관리자/소유자가 아닌 경우에는 적어도 HDInsight 클러스터가 포함된 리소스 그룹에 대한 [기여자] 액세스 권한이 있어야 합니다.

HDInsight 클러스터를 만드는 경우 Azure 구독에 대한 [기여자] 이상의 액세스 권한이 있는 사용자는 이전에 HDInsight에 대한 공급자를 등록했어야 합니다. 공급자 등록은 구독에 대해 참가자 액세스가 있는 사용자가 구독에서 처음으로 리소스를 만들 때 이루어집니다. [REST를 사용하여 공급자를 등록](https://msdn.microsoft.com/library/azure/dn790548.aspx)하면 리소스를 만들지 않고도 이 작업을 수행할 수 있습니다.

액세스 관리 작업에 대한 자세한 내용은 다음을 참조하세요.

* [Azure 포털에서 액세스 관리 시작](../role-based-access-control/overview.md)
* [역할 할당을 사용하여 Azure 구독 리소스에 대한 액세스 관리](../role-based-access-control/role-assignments-portal.md)

## <a name="understand-script-actions"></a>스크립트 동작 이해

스크립트 작업은 HDInsight 클러스터의 노드에서 실행되는 Bash 스크립트입니다. 스크립트 동작의 특징과 기능은 다음과 같습니다.

* HDInsight 클러스터에서 액세스할 수 있는 URI에 저장해야 합니다. 가능한 저장소 위치는 다음과 같습니다.
    
    * 일반 클러스터:
    
      * ADLS Gen1: HDInsight에서 Data Lake Storage에 액세스하는 데 사용하는 서비스 주체에는 스크립트에 대한 읽기 권한이 있어야 합니다. Data Lake Storage Gen1에 저장되는 스크립트에 대한 URI 형식은 `adl://DATALAKESTOREACCOUNTNAME.azuredatalakestore.net/path_to_file`입니다.
      
      * HDInsight 클러스터에 대한 기본 또는 추가 스토리지 계정인 Azure Storage 계정의 Blob. HDInsight는 클러스터를 만드는 동안 이러한 두 유형의 저장소 계정 모두에 대해 액세스 권한을 부여받습니다.

        > [!IMPORTANT]  
        > 후속 스크립트 작업 실패를 저장 하는 스크립트를 사용 하 여 발생 하므로이 Azure Storage 계정에 대해 저장소 키를 회전 하지 않습니다.

      * 공용 파일 공유 서비스 http:// 경로 통해 액세스할 수 있습니다. 예제는 Azure Blob, GitHub, OneDrive입니다.

        URI 예제는 [예제 스크립트 동작 스크립트](#example-script-action-scripts)를 참조하세요.

     * ESP 사용 하 여 클러스터:
         
         * wasb: / / 또는 wasbs: / / 또는 http [s]:// Uri 지원 됩니다.
            
* 특정 노드 유형에서만 실행되도록 제한할 수 있습니다. 예를 들어 헤드 노드 또는 작업자 노드가 있습니다.

* 지속형 또는 임시 스크립트일 수 있습니다.

    지속형 스크립트는 크기 조정 작업을 통해 클러스터에 추가되는 새 작업자 노드를 사용자 지정하는 데 사용됩니다. 또한 크기 조정 작업이 수행되면 지속형 스크립트에서 다른 노드 유형에도 변경 내용을 적용할 수 있습니다. 예를 들어 헤드 노드가 있습니다.

  > [!IMPORTANT]  
  > 지속형 스크립트 작업은 고유한 이름이 있어야 합니다.

    임시 스크립트는 유지되지 않습니다. 이 스크립트가 실행되더라도 클러스터에 추가된 작업자 노드에는 적용되지 않습니다. 그러면 임시 스크립트를 지속형 스크립트로 승격하거나 지속형 스크립트를 임시 스크립트로 강등할 수 있습니다.

  > [!IMPORTANT]  
  > 클러스터를 만들 때 사용되는 스크립트 작업은 자동으로 보존됩니다.
  >
  > 사용자가 특별히 유지해야 한다고 지정하더라도 실패한 스크립트는 유지되지 않습니다.

* 실행 중에 스크립트에서 사용하는 매개 변수를 수락할 수 있습니다.

* 클러스터 노드에서 루트 수준 권한으로 실행합니다.

* Azure Portal, Azure PowerShell, Azure 클래식 CLI 또는 HDInsight .NET SDK를 통해 사용할 수 있습니다.

클러스터에서 실행된 모든 스크립트에 대한 기록을 보관합니다. 이 기록은 승격 또는 강등 작업에 사용할 스크립트 ID를 찾아야 할 때 도움이 됩니다.

> [!IMPORTANT]  
> 스크립트 동작으로 인한 변경을 자동으로 실행 취소하는 방법은 없습니다. 변경 사항을 수동으로 되돌리거나 되돌린 스크립트를 제공하세요.

### <a name="script-action-in-the-cluster-creation-process"></a>클러스터 만들기 프로세스의 스크립트 동작

클러스터를 만드는 중에 사용되는 스크립트 동작은 기존 클러스터에서 실행되는 스크립트 동작과 약간 다릅니다.

* 스크립트는 자동으로 유지됩니다.

* 스크립트가 실패하면 클러스터 만들기 프로세스도 실패할 수 있습니다.

다음 다이어그램에서는 만들기 프로세스 중에 스크립트 동작이 실행되는 경우를 보여 줍니다.

![HDInsight 클러스터 사용자 지정 및 클러스터 만드는 동안의 단계][img-hdi-cluster-states]

HDInsight를 구성하는 동안 스크립트가 실행됩니다. 스크립트는 클러스터에 지정된 모든 노드에서 병렬로 실행됩니다. 노드에서 루트 권한으로 실행됩니다.

> [!NOTE]  
> Apache Hadoop 관련 서비스를 포함하여 서비스 중지 및 시작 같은 작업을 수행할 수 있습니다. 서비스를 중지하는 경우 스크립트가 완료되기 전에 Ambari 서비스 및 다른 Hadoop 관련 서비스가 실행되고 있는지 확인해야 합니다. 이러한 서비스는 클러스터를 만드는 동안 해당 클러스터의 상태를 확인하는 데 필요합니다.


클러스터를 만드는 동안 여러 스크립트 동작을 한 번에 사용할 수 있습니다. 이러한 스크립트는 지정된 순서로 호출됩니다.

> [!IMPORTANT]  
> 스크립트 동작은 60분 이내에 완료되어야 하며, 그렇지 않으면 시간이 초과됩니다. 클러스터 프로비전 중에 스크립트가 다른 설정 및 구성 프로세스와 동시에 실행됩니다. CPU 시간 또는 네트워크 대역폭과 같은 리소스에 대한 경쟁으로 인해 스크립트를 완료하는 데 걸리는 시간이 개발 환경에서보다 더 오래 걸릴 수 있습니다.
>
> 스크립트를 실행하는 데 걸리는 시간을 최소화하려면 원본에서 애플리케이션을 다운로드하고 컴파일하는 작업을 수행하지 않습니다. 애플리케이션을 미리 컴파일하고 이진 파일을 Azure Storage에 저장합니다.


### <a name="script-action-on-a-running-cluster"></a>실행 중인 클러스터의 스크립트 작업

이미 실행 중인 클러스터에서 실행되는 스크립트가 실패하더라도 해당 클러스터는 자동으로 '실패' 상태로 변경되지 않습니다. 스크립트가 완료되면 클러스터는 '실행 중' 상태로 돌아갑니다.

> [!IMPORTANT]  
> 클러스터가 '실행 중' 상태인 경우에도 실패한 스크립트가 손상되었을 수 있습니다. 예를 들어 스크립트는 클러스터에 필요한 파일을 삭제할 수 있습니다.
>
> 스크립트 동작은 루트 권한으로 실행됩니다. 먼저 스크립트에서 수행하는 작업을 이해한 후에 스크립트 동작을 클러스터에 적용해야 합니다.

스크립트가 클러스터에 적용되면 클러스터 상태가 **실행 중**에서 **수락됨**으로 변경됩니다. 그런 다음, **HDInsight 구성**으로 변경되고, 마지막으로 스크립트가 성공하면 **실행 중**으로 돌아갑니다. 스크립트 상태는 스크립트 동작 기록에 기록됩니다. 이 정보는 스크립트의 성공 여부를 알려 줍니다. 예를 들어 `Get-AzHDInsightScriptActionHistory` PowerShell cmdlet은 스크립트 상태를 표시합니다. 이 명령은 다음 텍스트와 비슷한 정보를 반환합니다.

    ScriptExecutionId : 635918532516474303
    StartTime         : 8/14/2017 7:40:55 PM
    EndTime           : 8/14/2017 7:41:05 PM
    Status            : Succeeded

> [!IMPORTANT]  
> 클러스터를 만든 후에 클러스터 사용자, 관리자, 암호를 변경하면 이 클러스터에서 실행되는 스크립트 동작이 실패할 수 있습니다. 작업자 노드를 대상으로 하는 지속형 스크립트 동작이 있는 경우 클러스터 크기를 조정할 때 이러한 스크립트가 실패할 수 있습니다.

## <a name="example-script-action-scripts"></a>예제 스크립트 동작 스크립트

스크립트 동작 스크립트는 다음 유틸리티를 통해 사용할 수 있습니다.

* Azure 포털
* Azure PowerShell
* Azure 클래식 CLI
* HDInsight .NET SDK

HDInsight는 HDInsight 클러스터에서 다음 구성 요소를 설치하는 스크립트를 제공합니다.

| 이름 | 스크립트 |
| --- | --- |
| Azure Storage 계정 추가 |`https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh`. [HDInsight에 추가 스토리지 계정 추가](hdinsight-hadoop-add-storage.md) 참조 |
| Hue 설치 |`https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh`. [HDInsight Hadoop 클러스터에 Hue 설치 및 사용](hdinsight-hadoop-hue-linux.md) 참조 |
| Giraph 설치 |`https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh`. [HDInsight Hadoop 클러스터에 Apache Giraph 설치](hdinsight-hadoop-giraph-install-linux.md) 참조 |
| Hive 라이브러리 미리 로드 |`https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh`. [HDInsight 클러스터를 만들 때 사용자 지정 Apache Hive 라이브러리 추가](hdinsight-hadoop-add-hive-libraries.md) 참조 |

## <a name="use-a-script-action-during-cluster-creation"></a>클러스터를 만드는 동안 스크립트 동작 사용

이 섹션에서는 HDInsight 클러스터를 만들 때 스크립트 동작을 사용할 수 있는 다양한 방법에 대해 설명합니다.

### <a name="use-a-script-action-during-cluster-creation-from-the-azure-portal"></a>클러스터를 만드는 동안 Azure Portal에서 스크립트 동작 사용

1. [Apache Hadoop, Apache Spark, Apache Kafka 등을 사용하여 HDInsight에서 클러스터 설정](hdinsight-hadoop-provision-linux-clusters.md)에서 설명한 대로 클러스터를 만들기 시작합니다. 클러스터를 만드는 동안 __클러스터 요약__ 페이지가 표시됩니다. __클러스터 요약__ 페이지에서 __고급 설정__에 대한 __편집__ 링크를 선택합니다.

    ![고급 설정 링크](./media/hdinsight-hadoop-customize-cluster-linux/advanced-settings-link.png)

3. __고급 설정__ 섹션에서 __스크립트 작업__을 선택합니다. __스크립트 동작__ 섹션에서 __+ 새로운 항목 제출__을 선택합니다.

    ![새 스크립트 작업 제출](./media/hdinsight-hadoop-customize-cluster-linux/add-script-action.png)

4. __스크립트 선택__ 항목을 사용하여 미리 만들어져 있는 스크립트를 선택합니다. 사용자 지정 스크립트를 사용하려면 __사용자 지정__을 선택합니다. 그런 다음, 스크립트에 대한 __이름__ 및 __Bash 스크립트 URI__를 제공합니다.

    ![엄선된 스크립트 양식에서 스크립트 추가](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    다음 표에서는 양식의 요소에 대해 설명합니다.

    | 속성 | 값 |
    | --- | --- |
    | 스크립트 선택 | 사용자 소유 스크립트를 사용하려면 __사용자 지정__을 선택합니다. 그렇지 않은 경우 제공된 스크립트 중 하나를 선택합니다. |
    | 이름 |스크립트 작업의 이름을 지정합니다. |
    | Bash 스크립트 URI |스크립트의 URI를 지정합니다. |
    | Head/Worker/Zookeeper |스크립트가 실행되는 노드, 즉 **Head**, **Worker** 또는 **ZooKeeper**를 지정합니다. |
    | 매개 변수 |스크립트에 필요한 경우 매개 변수를 지정합니다. |

    __이 스크립트 동작을 유지__ 항목을 사용하여 크기 조정 작업 시 스크립트가 적용되도록 합니다.

5. __만들기__를 선택하여 스크립트를 저장합니다. 그런 다음, __+ 새로운 항목 제출__을 사용하여 다른 스크립트를 추가할 수 있습니다.

    ![여러 스크립트 작업](./media/hdinsight-hadoop-customize-cluster-linux/multiple-scripts.png)

    스크립트 추가가 완료되면 __선택__, __다음__ 단추를 차례로 선택하여 __클러스터 요약__ 섹션으로 돌아갑니다.

3. 클러스터를 만들려면 __클러스터 요약__ 선택 영역에서 __만들기__를 선택합니다.

### <a name="use-a-script-action-from-azure-resource-manager-templates"></a>Azure Resource Manager 템플릿에서 스크립트 동작 사용

스크립트 동작을 Azure Resource Manager 템플릿과 함께 사용할 수 있습니다. 예를 들어 [HDInsight Linux 클러스터 만들기 및 스크립트 동작](https://azure.microsoft.com/resources/templates/hdinsight-linux-run-script-action/)을 참조하세요.

이 예제에서는 다음 코드를 사용하여 스크립트 동작을 추가합니다.

```json
"scriptActions": [
    {
        "name": "setenvironmentvariable",
        "uri": "[parameters('scriptActionUri')]",
        "parameters": "headnode"
    }
]
```

템플릿을 배포하는 방법에 대한 자세한 내용은 다음을 참조하세요.

* [Resource Manager 템플릿과 Azure PowerShell로 리소스 배포](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy)

* [Resource Manager 템플릿과 Azure CLI로 리소스 배포](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-cli)

### <a name="use-a-script-action-during-cluster-creation-from-azure-powershell"></a>클러스터를 만드는 동안 Azure PowerShell에서 스크립트 동작 사용

이 섹션을 사용 하 여는 [추가 AzHDInsightScriptAction](https://docs.microsoft.com/powershell/module/az.hdinsight/add-azhdinsightscriptaction) cmdlet는 클러스터를 사용자 지정 스크립트를 호출 합니다. 시작하기 전에 Azure PowerShell을 설치하고 구성해야 합니다. 다음 PowerShell 명령을 사용 하려면 필요 합니다 [AZ 모듈](https://docs.microsoft.com/powershell/azure/overview)합니다.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

다음 스크립트에서는 PowerShell을 사용하여 클러스터를 만들 때 스크립트 동작을 적용하는 방법을 보여 줍니다.

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=5-90)]

클러스터가 생성되는 데 몇 분 정도 걸릴 수 있습니다.

### <a name="use-a-script-action-during-cluster-creation-from-the-hdinsight-net-sdk"></a>클러스터를 만드는 동안 HDInsight .NET SDK에서 스크립트 동작 사용

HDInsight .NET SDK는 .NET 애플리케이션에서 HDInsight를 더 쉽게 사용할 있게 하는 클라이언트 라이브러리를 제공합니다. 코드 샘플은 [.NET SDK를 사용하여 HDInsight에서 Linux 기반 클러스터 만들기](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action)를 참조하세요.

## <a name="apply-a-script-action-to-a-running-cluster"></a>실행 중인 클러스터에 스크립트 동작 적용

이 섹션에서는 실행 중인 클러스터에 스크립트 동작을 적용하는 방법에 대해 설명합니다.

### <a name="apply-a-script-action-to-a-running-cluster-from-the-azure-portal"></a>Azure Portal에서 실행 중인 클러스터에 스크립트 동작 적용

[Azure Portal](https://portal.azure.com)로 이동합니다.

1. 왼쪽 메뉴에서 **모든 서비스**를 선택합니다.

1. **ANALYTICS** 아래에서 **HDInsight 클러스터**를 선택합니다.

1. 목록에서 클러스터를 선택합니다. 그러면 기본 보기가 열립니다.

1. 기본 보기의 **설정** 아래에서 **스크립트 동작**을 선택합니다.

1. **스크립트 동작** 페이지의 위쪽에서 **+ 새로운 항목 제출**을 선택합니다.

    ![실행 중인 클러스터에 스크립트 적용](./media/hdinsight-hadoop-customize-cluster-linux/add-script-running-cluster.png)

4. __스크립트 선택__ 항목을 사용하여 미리 만들어져 있는 스크립트를 선택합니다. 사용자 지정 스크립트를 사용하려면 __사용자 지정__을 선택합니다. 그런 다음, 스크립트에 대한 __이름__ 및 __Bash 스크립트 URI__를 제공합니다.

    ![엄선된 스크립트 양식에서 스크립트 추가](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    다음 표에서는 양식의 요소에 대해 설명합니다.

    | 속성 | 값 |
    | --- | --- |
    | 스크립트 선택 | 사용자 소유 스크립트를 사용하려면 __사용자 지정__을 선택합니다. 그렇지 않은 경우 제공된 스크립트를 선택합니다. |
    | 이름 |스크립트 작업의 이름을 지정합니다. |
    | Bash 스크립트 URI |스크립트의 URI를 지정합니다. |
    | Head/Worker/Zookeeper |스크립트가 실행되는 노드, 즉 **Head**, **Worker** 또는 **ZooKeeper**를 지정합니다. |
    | 매개 변수 |스크립트에 필요한 경우 매개 변수를 지정합니다. |

    __이 스크립트 작업을 유지__ 항목을 사용하여 크기 조정 작업 시 스크립트가 적용되도록 합니다.

5. 마지막으로 **만들기** 단추를 선택하여 스크립트를 클러스터에 적용합니다.

### <a name="apply-a-script-action-to-a-running-cluster-from-azure-powershell"></a>Azure PowerShell에서 실행 중인 클러스터에 스크립트 동작 적용

다음 PowerShell 명령을 사용 하려면 필요 합니다 [AZ 모듈](https://docs.microsoft.com/powershell/azure/overview)합니다.

다음 예제에서는 스크립트 동작을 실행 중인 클러스터에 적용하는 방법을 보여 줍니다.

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=105-117)]

작업이 완료되면 다음 텍스트와 비슷한 정보가 표시됩니다.

    OperationState  : Succeeded
    ErrorMessage    :
    Name            : Giraph
    Uri             : https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
    Parameters      :
    NodeTypes       : {HeadNode, WorkerNode}

### <a name="apply-a-script-action-to-a-running-cluster-from-the-azure-cli"></a>Azure CLI에서 실행 중인 클러스터에 스크립트 동작 적용

시작하기 전에 Azure CLI를 설치하고 구성해야 합니다. 자세한 내용은 [Azure 클래식 CLI 설치](https://docs.microsoft.com/cli/azure/install-classic-cli?view=azure-cli-latest)를 참조하세요.

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

1. Azure Resource Manager 모드로 전환합니다.

    ```bash
    azure config mode arm
    ```

2. Azure 구독을 인증합니다.

    ```bash
    azure login
    ```

3. 실행 중인 클러스터에 스크립트 동작을 적용합니다.

    ```bash
    azure hdinsight script-action create <clustername> -g <resourcegroupname> -n <scriptname> -u <scriptURI> -t <nodetypes>
    ```

    이 명령에 대한 매개 변수를 생략하면 해당 매개 변수를 요구하는 메시지가 표시됩니다. `-u`를 사용하여 지정한 스크립트에서 매개 변수를 허용하는 경우 `-p` 매개 변수를 사용하여 지정할 수 있습니다.

    유효한 노드 형식은 `headnode`, `workernode` 및 `zookeeper`입니다. 스크립트를 여러 노드 유형에 적용해야 하는 경우 세미콜론(`;`)으로 구분하여 형식을 지정합니다. `-n headnode;workernode` )을 입력합니다.

    스크립트를 유지하려면 `--persistOnSuccess`를 추가합니다. 나중에 `azure hdinsight script-action persisted set`을(를) 사용하여 스크립트를 지속할 수도 있습니다.

    작업이 완료되면 다음 텍스트와 같이 출력됩니다.

        info:    Executing command hdinsight script-action create
        + Executing Script Action on HDInsight cluster
        data:    Operation Info
        data:    ---------------
        data:    Operation status:
        data:    Operation ID:  b707b10e-e633-45c0-baa9-8aed3d348c13
        info:    hdinsight script-action create command OK

### <a name="apply-a-script-action-to-a-running-cluster-by-using-rest-api"></a>REST API를 사용하여 실행 중인 클러스터에 스크립트 동작 적용

[Azure HDInsight의 클러스터 REST API](https://msdn.microsoft.com/library/azure/mt668441.aspx)를 참조하세요.

### <a name="apply-a-script-action-to-a-running-cluster-from-the-hdinsight-net-sdk"></a>HDInsight .NET SDK에서 실행 중인 클러스터에 스크립트 동작 적용

.NET SDK를 사용하여 스크립트를 클러스터에 적용하는 예제는 [실행 중인 Linux 기반 HDInsight 클러스터에 스크립트 동작 적용](https://github.com/Azure-Samples/hdinsight-dotnet-script-action)을 참조하세요.

## <a name="view-history-and-promote-and-demote-script-actions"></a>기록 보기, 스크립트 동작 승격 및 강등

### <a name="the-azure-portal"></a>Azure 포털

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.

1. 왼쪽 메뉴에서 **모든 서비스**를 선택합니다.

1. **ANALYTICS** 아래에서 **HDInsight 클러스터**를 선택합니다.

1. 목록에서 클러스터를 선택합니다. 그러면 기본 보기가 열립니다.

1. 기본 보기의 **설정** 아래에서 **스크립트 동작**을 선택합니다.

4. 이 클러스터에 대한 스크립트 기록이 스크립트 동작 섹션에 표시됩니다. 이 정보에는 지속된 스크립트 목록이 포함되어 있습니다. 다음 스크린샷에서는 이 클러스터에서 Solr 스크립트가 실행되었음을 보여 줍니다. 이 스크린샷에는 지속형 스크립트가 보이지 않습니다.

    ![스크립트 작업](./media/hdinsight-hadoop-customize-cluster-linux/script-action-history.png)

5. 기록에서 스크립트를 선택하여 이 스크립트의 **속성** 섹션을 표시합니다. 화면 맨 위에서 스크립트를 다시 실행하거나 승격할 수 있습니다.

    ![스크립트 동작, 속성](./media/hdinsight-hadoop-customize-cluster-linux/promote-script-actions.png)

6. 스크립트 동작 섹션에 있는 항목의 오른쪽에서 줄임표( **...** )를 선택하여 해당 동작을 수행할 수도 있습니다.

    ![스크립트 동작, 줄임표](./media/hdinsight-hadoop-customize-cluster-linux/deletepromoted.png)

### <a name="azure-powershell"></a>Azure PowerShell

| Cmdlet | 함수 |
| --- | --- |
| `Get-AzHDInsightPersistedScriptAction` |지속형 스크립트 동작에 대한 정보를 검색합니다. |
| `Get-AzHDInsightScriptActionHistory` |클러스터에 적용된 스크립트 동작의 기록 또는 특정 스크립트에 대한 세부 정보를 검색합니다. |
| `Set-AzHDInsightPersistedScriptAction` |임시 스크립트 동작을 지속형 스크립트 동작으로 승격합니다. |
| `Remove-AzHDInsightPersistedScriptAction` |지속형 스크립트 동작을 임시 스크립트 동작으로 강등합니다. |

> [!IMPORTANT]  
> `Remove-AzHDInsightPersistedScriptAction`은 스크립트에서 수행한 동작을 실행 취소하지 않습니다. 이 cmdlet만 지속된 플래그를 제거합니다.

다음 예제 스크립트에서는 cmdlet을 사용하여 스크립트를 승격한 다음, 강등하는 방법을 보여 줍니다.

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=123-140)]

### <a name="the-azure-classic-cli"></a>Azure 클래식 CLI

| Cmdlet | 함수 |
| --- | --- |
| `azure hdinsight script-action persisted list <clustername>` |지속형 스크립트 동작의 목록을 검색합니다. |
| `azure hdinsight script-action persisted show <clustername> <scriptname>` |특정 지속형 스크립트 동작에 대한 정보를 검색합니다. |
| `azure hdinsight script-action history list <clustername>` |클러스터에 적용된 스크립트 동작의 기록을 검색합니다. |
| `azure hdinsight script-action history show <clustername> <scriptname>` |특정 스크립트 동작에 대한 정보를 검색합니다. |
| `azure hdinsight script action persisted set <clustername> <scriptexecutionid>` |임시 스크립트 동작을 지속형 스크립트 동작으로 승격합니다. |
| `azure hdinsight script-action persisted delete <clustername> <scriptname>` |지속형 스크립트 동작을 임시 스크립트 동작으로 강등합니다. |

> [!IMPORTANT]  
> `azure hdinsight script-action persisted delete`은 스크립트에서 수행한 동작을 실행 취소하지 않습니다. 이 cmdlet만 지속된 플래그를 제거합니다.

### <a name="the-hdinsight-net-sdk"></a>HDInsight .NET SDK

.NET SDK를 사용하여 클러스터에서 스크립트 기록을 검색하거나 스크립트를 승격 또는 강등하는 예제는 [실행 중인 Linux 기반 HDInsight 클러스터에 스크립트 동작 적용](https://github.com/Azure-Samples/hdinsight-dotnet-script-action)을 참조하세요.

> [!NOTE]  
> 이 예제에서는 .NET SDK를 사용하여 HDInsight 애플리케이션을 설치하는 방법도 보여 줍니다.

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>HDInsight 클러스터에서 사용하는 오픈 소스 소프트웨어 지원

Microsoft Azure HDInsight 서비스는 Apache Hadoop으로 형성된 오픈 소스 기술의 에코시스템을 사용합니다. Microsoft Azure는 오픈 소스 기술에 대한 일반 수준의 지원을 제공합니다. 자세한 내용은 [Azure Support FAQ](https://azure.microsoft.com/support/faq/)의 **지원 범위** 섹션을 참조하세요. HDInsight 서비스는 기본 제공 구성 요소에 대해 추가 수준의 지원을 제공합니다.

HDInsight 서비스에서는 두 가지 유형의 오픈 소스 구성 요소를 사용할 수 있습니다.

* **기본 제공 구성 요소**. 이러한 구성 요소는 HDInsight 클러스터에 미리 설치되어 있으며 클러스터의 핵심 기능을 제공합니다. 이 범주에 속하는 구성 요소는 다음과 같습니다.

  * [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) ResourceManager
  * [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) Hive 쿼리 언어
  * [Apache Mahout](https://mahout.apache.org/) 
    
    클러스터 구성 요소의 전체 목록은 [HDInsight에서 사용할 수 있는 Apache Hadoop 구성 요소 및 버전은?](hdinsight-component-versioning.md)에서 사용할 수 있습니다.

* **사용자 지정 구성 요소**. 클러스터 사용자는 커뮤니티에서 사용할 수 있거나 직접 만든 구성 요소를 워크로드에 설치하거나 사용할 수 있습니다.

> [!WARNING]  
> HDInsight 클러스터에 제공되는 구성 요소는 완벽히 지원됩니다. Microsoft 지원은 이러한 구성 요소와 관련된 문제를 격리하고 해결하도록 도와줍니다.
>
> 사용자 지정 구성 요소는 문제를 해결하는 데 도움이 되는 상업적으로 합리적인 지원을 받습니다. Microsoft 지원에서 이 문제를 해결할 수 있습니다. 또는 깊은 전문 지식이 있는 오픈 소스 기술에 사용할 수 있는 채널에 참여하도록 요청할 수도 있습니다. 다양한 커뮤니티 사이트를 사용할 수 있습니다. 예를 들어 [HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight) 및 [Stack Overflow](https://stackoverflow.com)에 대한 MSDN 포럼이 있습니다. 
>
> Apache 프로젝트에는 [Apache 웹 사이트](https://apache.org)의 프로젝트 사이트도 있습니다. 예를 들어 [Hadoop](https://hadoop.apache.org/)이 있습니다.

HDInsight 서비스는 사용자 지정 구성 요소를 사용하는 여러 방법을 제공합니다. 클러스터에서 구성 요소를 사용하거나 설치하는 방법과 상관없이 동일한 수준의 지원이 적용됩니다. 다음 목록에는 HDInsight 클러스터에서 사용자 지정 구성 요소를 사용하는 가장 일반적인 방법이 나와 있습니다.

1. **작업 제출**. 사용자 지정 구성 요소를 실행하거나 사용하는 Hadoop 또는 다른 유형의 작업을 클러스터에 제출할 수 있습니다.

2. **클러스터 사용자 지정**. 클러스터를 만드는 동안 클러스터 노드에 설치되는 추가 설정 및 사용자 지정 구성 요소를 지정할 수 있습니다.

3. **샘플**. 인기 있는 사용자 지정 구성 요소의 경우 Microsoft 및 다른 사용자는 이러한 구성 요소를 HDInsight 클러스터에서 사용하는 방법을 보여 주는 샘플을 제공할 수 있습니다. 이러한 샘플은 지원 없이 제공됩니다.

## <a name="troubleshooting"></a>문제 해결

Ambari 웹 UI를 사용하여 스크립트 동작에서 기록한 정보를 볼 수 있습니다. 또한 클러스터를 만드는 중에 스크립트가 실패할 경우 해당 클러스터와 연결된 기본 스토리지 계정에서 로그를 사용할 수도 있습니다. 이 섹션에서는 이 두 옵션을 모두 사용하여 로그를 검색하는 방법에 대한 정보를 제공합니다.

### <a name="the-apache-ambari-web-ui"></a>Apache Ambari 웹 UI

1. 브라우저에서 https://CLUSTERNAME.azurehdinsight.net 으로 이동합니다. **CLUSTERNAME**을 HDInsight 클러스터의 이름으로 바꿉니다.

    메시지가 표시되면 클러스터에 대한 관리자 계정 이름(**admin**)과 암호를 입력합니다. 웹 양식에서 관리자 자격 증명을 다시 입력해야 할 수도 있습니다.

2. 페이지 위쪽의 모음에서 **작업** 항목을 선택합니다. Ambari를 통해 클러스터에서 수행된 현재 및 이전 작업이 목록에 표시됩니다.

    ![선택한 작업으로 Ambari 웹 UI 모음](./media/hdinsight-hadoop-customize-cluster-linux/ambari-nav.png)

3. **작업** 열에 **run\_customscriptaction**이 있는 항목을 찾습니다. 이러한 항목을 스크립트 동작을 실행할 때 생성됩니다.

    ![작업의 스크린샷](./media/hdinsight-hadoop-customize-cluster-linux/ambariscriptaction.png)

    **STDOUT** 및 **STDERR** 출력을 보려면 **run\customscriptaction** 항목을 선택하고 링크를 통해 드릴다운합니다. 이 출력은 스크립트가 실행될 때 생성되며, 여기에는 유용한 정보가 있을 수 있습니다.

### <a name="access-logs-from-the-default-storage-account"></a>기본 저장소 계정에서 로그 액세스

스크립트 오류로 인해 클러스터를 만드는 데 실패하면 해당 로그가 클러스터 스토리지 계정에 보관됩니다.

* 스토리지 로그는 `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE`에서 지원됩니다.

    ![작업의 스크린샷](./media/hdinsight-hadoop-customize-cluster-linux/script_action_logs_in_storage.png)

    이 디렉터리에서 로그는 **헤드 노드**, **작업자 노드** 및 **Zookeeper 노드**에 대해 별도로 구성됩니다. 다음 예를 참조하십시오.

    * **헤드 노드**: `<uniqueidentifier>AmbariDb-hn0-<generated_value>.cloudapp.net`

    * **작업자 노드**: `<uniqueidentifier>AmbariDb-wn0-<generated_value>.cloudapp.net`

    * **Zookeeper 노드**: `<uniqueidentifier>AmbariDb-zk0-<generated_value>.cloudapp.net`

* 해당 호스트의 모든 **stdout** 및 **stderr**이 스토리지 계정에 업로드됩니다. 각 스크립트 동작에 대해 **output-\*.txt** 및 **errors-\*.txt**가 하나씩 있습니다. **output-*.txt** 파일에는 호스트에서 실행된 스크립트의 URI 정보를 포함되어 있습니다. 다음 텍스트는 이 정보의 예제입니다.

        'Start downloading script locally: ', u'https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh'

* 같은 이름으로 반복해서 스크립트 작업 클러스터를 만들 수 있습니다. 이 경우 **날짜** 폴더 이름에 따라 관련 로그를 구분할 수 있습니다. 예를 들어 서로 다른 날짜에 만든 클러스터(**mycluster**)의 폴더 구조는 다음 로그 항목과 비슷하게 나타납니다.

    `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-04` `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-05`

* 같은 날 같은 이름으로 스크립트 작업 클러스터를 만든 경우 고유의 접두사를 사용하여 관련 로그 파일을 식별할 수 있습니다.

* 오전 12시(자정)쯤에 클러스터를 만든 경우 로그 파일이 이틀에 걸쳐 있을 수 있습니다. 이 경우 동일한 클러스터에 대해 서로 다른 두 개의 날짜 폴더가 표시됩니다.

* 기본 컨테이너에 로그 파일을 업로드하는 경우, 특히 대형 클러스터의 경우 최대 5분이 걸릴 수 있습니다. 따라서 로그에 액세스하려면 스크립트 동작이 실패하는 경우 클러스터를 즉시 삭제하지 않아야 합니다.

### <a name="ambari-watchdog"></a>Ambari 감시

> [!WARNING]  
> Linux 기반 HDInsight 클러스터에서 Ambari Watchdog(hdinsightwatchdog)에 대한 암호는 변경하지 마세요. 이 계정의 암호를 변경하면 HDInsight 클러스터에서 새 스크립트 동작을 실행할 수 없게 됩니다.

### <a name="cant-import-name-blobservice"></a>이름 BlobService를 가져올 수 없음

__증상__. 스크립트 작업이 실패했습니다. Ambari에서 작업을 볼 때 다음 오류와 비슷한 텍스트가 표시됩니다.

```
Traceback (most recent call list):
  File "/var/lib/ambari-agent/cache/custom_actions/scripts/run_customscriptaction.py", line 21, in <module>
    from azure.storage.blob import BlobService
ImportError: cannot import name BlobService
```

__원인__. 이 오류는 HDInsight 클러스터에 포함된 Python Azure Storage 클라이언트를 업그레이드하는 경우에 발생합니다. HDInsight는 Azure Storage 클라이언트 0.20.0을 예상합니다.

__해결 방법__. 이 오류를 해결하려면 `ssh`를 사용하여 각 클러스터 노드에 수동으로 연결합니다. 다음 명령을 실행하여 올바른 스토리지 클라이언트 버전을 다시 설치합니다.

```bash
sudo pip install azure-storage==0.20.0
```

SSH를 사용하여 클러스터에 연결하는 방법에 대한 자세한 내용은 [SSH를 사용하여 HDInsight(Apache Hadoop)에 연결](hdinsight-hadoop-linux-use-ssh-unix.md)을 참조하세요.

### <a name="history-doesnt-show-the-scripts-used-during-cluster-creation"></a>클러스터를 만드는 동안 스크립트가 기록에 표시되지 않음

2016년 3월 15일 이전에 클러스터를 만든 경우에는 스크립트 동작 기록에 항목이 표시되지 않을 수 있습니다. 클러스터의 크기를 조정하면 스크립트가 스크립트 동작 기록에 표시됩니다.

두 가지 예외 사항이 있습니다.

* 클러스터가 2015년 9월 1일 이전에 만들어졌습니다. 스크립트 동작이 이 날에 도입되었습니다. 이 날짜 이전에 만든 클러스터는 클러스터를 만드는 데 스크립트 동작을 사용할 수 없었습니다.

* 클러스터를 만드는 동안 여러 스크립트 동작을 사용했습니다. 또는 여러 스크립트에 대해 동일한 이름을 사용했거나, 여러 스크립트에 대해 동일한 이름과 URI를 사용했지만 서로 다른 매개 변수를 사용했습니다. 이러한 경우 발생하는 오류는 다음과 같습니다.

    기존 스크립트에 충돌하는 스크립트 이름이 있으므로 이 클러스터에서 새 스크립트 동작을 실행할 수 없습니다. 클러스터를 만들 때 제공되는 스크립트 이름은 모두 고유해야 합니다. 크기 조정에서 기존 스크립트가 실행됩니다.

## <a name="next-steps"></a>다음 단계

* [HDInsight용 스크립트 동작 스크립트 개발](hdinsight-hadoop-script-actions-linux.md)
* [HDInsight 클러스터에 Apache Giraph 설치 및 사용](hdinsight-hadoop-giraph-install-linux.md)
* [HDInsight 클러스터에 추가 저장소 추가](hdinsight-hadoop-add-storage.md)

[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster-linux/HDI-Cluster-state.png "클러스터를 만드는 동안의 단계"
