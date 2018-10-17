---
title: Azure Stack에 (k 8) Kubernetes에 배포 문제 해결 | Microsoft Docs
description: Azure Stack에 (k 8) Kubernetes에 배포 문제를 해결 하는 방법을 알아봅니다.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: fbb51d8dc3b1ea4c6b34120e8fe35474ae949cf2
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49116915"
---
# <a name="troubleshoot-your-deployment-to-kubernetes-to-azure-stack"></a>Azure Stack에 Kubernetes에 배포 문제 해결

*적용 대상: Azure Stack 통합 시스템 및 Azure Stack 개발 키트*

> [!Note]  
> Azure Stack에서 Kubernetes 미리 보기입니다.

다음 문서는 Kubernetes 클러스터 문제 해결 살펴봅니다. 배포 경고를 검토 하 고 배포 하는 데 필요한 요소에 의해 배포의 상태를 검토할 수 있습니다. Azure Stack에 또는 호스트 Kubernetes 사용 하는 Linux Vm에서 배포 로그를 수집 해야 합니다. 또한 관리 끝점에서 로그를 검색할 Azure Stack 관리자와 협력 해야 합니다.

## <a name="overview-of-deployment"></a>배포 개요

클러스터의 문제를 해결 하는 단계에 도달 하기 전에 Azure Stack Kubernetes 클러스터 배포 프로세스를 검토 하는 것이 좋습니다. 배포는 Azure Resource Manager 솔루션 템플릿을 사용 하 여 Vm을 만들려면 및 클러스터에 대 한 ACS 엔진을 설치 합니다.

### <a name="deployment-workflow"></a>배포 워크플로

다음 다이어그램에서는 클러스터를 배포 하기 위한 일반적인 프로세스를 보여 줍니다.

![Kubernetes 프로세스 배포](media/azure-stack-solution-template-kubernetes-trouble/002-Kubernetes-Deploy-Flow.png)

### <a name="deployment-steps"></a>배포 단계

1. 수집 된 marketplace 항목의 매개 변수를 입력합니다.

    포함 하는 Kubernetes 클러스터를 설정 하 여 필요한 값을 입력 합니다.
    -  **사용자 이름** Kubernetes 클러스터의 일부인 Linux Virtual Machines 및 dvm이 대 한 사용자 이름입니다.
    -  **SSH 공개 키** dvm이 고 Kubernetes 클러스터의 일부로 생성 하는 모든 Linux 컴퓨터에 대 한 권한 부여에 사용 된 키
    -  **서비스 원칙** Kubernetes Azure 클라우드 공급자에서 사용 하는 ID입니다. 서비스 주체를 만들 때 응용 프로그램 ID로 식별 된 클라이언트 ID입니다. 
    -  **클라이언트 암호** 키에 서비스 주체를 만들 때 만든 것입니다.

2. VM 배포를 만들고 및 사용자 지정 스크립트 확장 합니다.
    -  Marketplace의 Linux 이미지를 사용 하 여 Linux VM 배포를 만듭니다 **Ubuntu Server 16.04-LTS**합니다.
    -  다운로드 하 고 marketplace에서 고객 스크립트 확장을 실행 합니다. 이 스크립트는 합니다 **Linux 2.0에 대 한 사용자 지정 스크립트**합니다.
    -  Dvm이 사용자 지정 스크립트를 실행합니다. 스크립트:
        1. Azure 리소스 관리자 메타 데이터 끝점에서 갤러리 끝점을 가져옵니다.
        2. Azure 리소스 관리자 메타 데이터 끝점에서 active directory 리소스 ID를 가져옵니다.
        3. ACS 엔진에 대 한 API 모델을 로드합니다.
        4. ACS Engine, Kubernetes 클러스터에 배포 하 고 Azure Stack 클라우드 프로필 저장 `/etc/kubernetes/azurestackcloud.json`합니다.
3. 마스터 Vm을 만듭니다.

    다운로드 하 고 고객 스크립트 확장을 실행 합니다.

4. 마스터 스크립트를 실행합니다.

    스크립트:
    - Etcd, Docker 및 Kubernetes 설치 kubelet와 같은 리소스입니다. etcd에 클러스터 컴퓨터에 걸쳐 데이터를 저장 하는 방법을 제공 하는 분산된 키 값 저장소입니다. Docker는 컨테이너 라고 핵심 운영 체제 수준 virtualizations를 지원 합니다. Kubelet 각 Kubernetes 노드에서 실행 되는 노드 에이전트는입니다.
    - Etcd 서비스를 설정합니다.
    - Kubelet 서비스를 설정합니다.
    - Kubelet을 시작합니다. 이 과정은 다음과 같습니다.
        1. API 서비스를 시작합니다.
        2. 컨트롤러 서비스를 시작합니다.
        3. Scheduler 서비스를 시작합니다.
5. Vm 에이전트를 만듭니다.

    다운로드 하 고 고객 스크립트 확장을 실행 합니다.

6. 에이전트 스크립트를 실행합니다. 에이전트 사용자 지정 스크립트:
    - Etcd를 설치 합니다.
    - Kubelet 서비스를 설정 합니다.
    - Kubernetes 클러스터에 연결 합니다.

## <a name="steps-for-troubleshooting"></a>문제 해결 단계

Kubernetes 클러스터를 지 원하는 Vm에서 로그를 수집할 수 있습니다. 또한 배포 로그를 검토할 수 있습니다. 를 사용 하는 Azure Stack의 버전을 확인 하 고 배포와 관련 된 Azure Stack에서 로그를 가져오는 Azure Stack 관리자에 게 문의 해야 합니다.

1. 검토 합니다 [배포 상태](#review-deployment-status) 하며 [로그를 검색할](#get-logs-from-a-vm) Kubernetes 클러스터의 마스터 노드에서.
2. Azure Stack의 최신 버전을 사용 해야 합니다. Azure Stack의 버전을 잘 모르는 경우 Azure Stack 관리자에 게 문의 합니다. Kubernetes 클러스터 marketplace 시간 0.3.0 Azure Stack 버전 1808 이상이 필요합니다.
3.  VM 생성 파일을 검토 합니다. 다음과 같은 문제가 발생 한 것일 수 있습니다.  
    - 공개 키 잘못 되었을 수 있습니다. 사용자가 만든 키를 검토 합니다.  
    - VM 만드는 내부 오류가 있습니다 트리거된 또는 생성 오류를 트리거한. Azure Stack 구독에 대 한 용량 제한은 비롯 한 인수의 수에서 오류가 발생할 수 있습니다.
    - VM에 대 한 정규화 된 도메인 이름 (FDQN)는 중복 접두사를 사용 하 여 시작 되었나요?
4.  VM이 있는 경우 **확인**, 해당는 dvm이 평가 합니다. dvm이 오류 메시지가 있으면:

    - 공개 키 잘못 되었을 수 있습니다. 사용자가 만든 키를 검토 합니다.  
     - 권한 있는 끝점을 사용 하 여 Azure Stack에 대 한 로그를 검색 하 여 Azure Stack 관리자에 게 문의 해야 합니다. 자세한 내용은 [진단 도구를 Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostics)합니다.
5. 배포에 대 한 질문이 있으면 질문을 게시 하거나 경우 사용자에 대 한 답변이 이미 질문에 참조를 [Azure Stack 포럼](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack)합니다. 

## <a name="review-deployment-status"></a>배포 상태를 검토 합니다.

모든 문제를 검토 하 여 Kubernetes 클러스터를 배포할 때 배포 상태를 검토할 수 있습니다.

1. 엽니다는 [Azure Stack 포털](https://portal.local.azurestack.external)합니다.
2. 선택 **리소스 그룹**, 그리고 Kubernetes 클러스터 배포 하 때 사용 되는 리소스 그룹의 이름을 선택 합니다.
3. 선택 **배포** 차례로 합니다 **배포 이름**합니다.

    ![문제 해결](media/azure-stack-solution-template-kubernetes-trouble/azure-stack-kub-trouble-report.png)

4.  문제 해결 창을 참조 하세요. 배포 된 각 리소스에는 다음 정보를 제공합니다.
    
    | 자산 | 설명 |
    | ----     | ----        |
    | 리소스 | 리소스의 이름입니다. |
    | type | 리소스 공급자 및 리소스의 형식입니다. |
    | 상태 | 항목의 상태입니다. |
    | TimeStamp | 시간 UTC 타임 스탬프입니다. |
    | 작업 세부 정보 | 리소스의 이름, 작업 및 리소스 끝점에 관련 된 리소스 공급자와 같은 작업 세부 정보입니다. |

    각 항목에는 녹색 또는 빨간색 상태 아이콘은 해야 합니다.

## <a name="get-logs-from-a-vm"></a>VM에서 로그 가져오기

필요한 클러스터에 대 한 마스터 VM에 연결 하 bash 프롬프트를 열고 하 로그를 생성 하는 스크립트를 실행 합니다. 마스터 클러스터 리소스 그룹에 있을 수 있고 이름이 `k8s-master-<sequence-of-numbers>`합니다. 

### <a name="prerequisites"></a>필수 조건

Bash 해야 Azure Stack 관리에 사용 하 여 컴퓨터에 요청 합니다. Bash를 사용 하 여 로그에 액세스 하는 스크립트를 실행 합니다. Windows 컴퓨터에서 Git를 사용 하 여 설치 된 bash 프롬프트를 사용할 수 있습니다. Git의 최신 버전을 참조 하세요 [git 다운로드](https://git-scm.com/downloads)합니다.

### <a name="get-logs"></a>로그 가져오기

1. Bash 프롬프트를 엽니다. Git를 Windows 컴퓨터에서 사용 하는 경우 다음 경로에서 bash 프롬프트를 열 수 있습니다: `c:\programfiles\git\bin\bash.exe`합니다.
2. 다음 bash 명령을 실행 합니다.

    ```Bash  
    mkdir -p $HOME/kuberneteslogs
    cd $HOME/kuberneteslogs
    curl -O https://raw.githubusercontent.com/msazurestackworkloads/azurestack-gallery/master/diagnosis/getkuberneteslogs.sh
    sudo chmod 744 getkuberneteslogs.sh
    ```

    > [!Note]  
    > Windows를 실행할 필요가 없습니다 `sudo` 만 사용할 수 있습니다 및 `chmod 744 getkuberneteslogs.sh`합니다.

3. 동일한 세션에서 환경에 맞게 업데이트 하는 매개 변수를 사용 하 여 다음 명령을 실행 합니다.

    ```Bash  
    ./getkuberneteslogs.sh --identity-file id_rsa --user azureuser --vmdhost 192.168.102.37
    ```

    매개 변수를 검토 하 고 사용자 환경에 따라 값을 설정 합니다.
    | 매개 변수           | 설명                                                                                                      | 예                                                                       |
    |---------------------|------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
    | -i,-id-파일 | Kubernetes 마스터 VM에 연결할 RSA 개인 키 파일입니다. 키는로 시작 해야 합니다. `-----BEGIN RSA PRIVATE KEY-----` | C:\data\privatekey.pem                                                        |
    | -h,--호스트          | 공용 ip 또는 Kubernetes 클러스터 마스터 VM의 정규화 된 도메인 이름 (FQDN). VM 이름은 시작 `k8s-master-`합니다.                       | IP: 192.168.102.37<br><br>FQDN: k8s 12345.local.cloudapp.azurestack.external      |
    | -u,--사용자          | Kubernetes 클러스터 마스터 VM의 사용자 이름입니다. 마켓플레이스 항목을 구성할 때이 이름을 설정 합니다.                                                                    | azureuser                                                                     |
    | -d,-vmdhost       | 공용 ip 또는 FQDN을 dvm이을 합니다. VM 이름은 시작 `vmd-`합니다.                                                       | IP: 192.168.102.38<br><br>DNS: vmd dnsk8 frog.local.cloudapp.azurestack.external |

   매개 변수 값을 추가한 경우 비슷하게 보일 수 있습니다.

    ```Bash  
    ./getkuberneteslogs.sh --identity-file "C:\secretsecret.pem" --user azureuser --vmdhost 192.168.102.37
     ```

    실행을 성공적으로 로그를 만듭니다.

    ![생성 된 로그](media/azure-stack-solution-template-kubernetes-trouble/azure-stack-generated-logs.png)


4. 명령으로 만든 폴더에 로그를 검색 합니다. 타임 스탬프와 명령을 새 폴더를 만듭니다.
    - KubernetesLogs*YYYY-MM-DD-XX-XX-XX-XXX*
        - Dvmlogs
        - Acsengine-kubernetes-dvm.log

## <a name="next-steps"></a>다음 단계

[Azure Stack에 Kubernetes 배포](azure-stack-solution-template-kubernetes-deploy.md)합니다.

[Kubernetes를 추가할 Marketplace (Azure Stack 연산자)](..\azure-stack-solution-template-kubernetes-cluster-add.md)

[Azure의 Kubernetes](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)