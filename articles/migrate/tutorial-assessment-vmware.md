---
title: Azure Migrate를 사용하여 Azure로의 마이그레이션에 대한 온-프레미스 VMware VM 검색 및 평가 | Microsoft Docs
description: Azure Migrate 서비스를 사용하여 Azure로의 마이그레이션에 대한 온-프레미스 VMware VM을 검색하고 평가하는 방법에 설명합니다.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 06/10/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: c16c7fdfdf3efe5518e2452871fdeace0a4a8526
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807947"
---
# <a name="discover-and-assess-on-premises-vmware-vms-for-migration-to-azure"></a>Azure로의 마이그레이션에 대한 온-프레미스 VMware VM 검색 및 평가

[Azure Migrate](migrate-overview.md) 서비스는 Azure로의 마이그레이션에 대한 온-프레미스 워크로드를 평가합니다.

이 자습서에서는 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * Azure Migrate에서 온-프레미스 VM을 검색하는 데 사용하는 계정 만들기
> * Azure Migrate 프로젝트를 만듭니다.
> * 평가를 위해 온-프레미스 VMware VM을 검색하도록 온-프레미스 수집기 가상 머신(VM)을 설정합니다.
> * VM을 그룹화하고 평가를 만듭니다.

Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/pricing/free-trial/)을 만듭니다.

## <a name="prerequisites"></a>필수 조건

- **VMware**: 마이그레이션하려는 VM은 5.5, 6.0, 6.5 또는 6.7 버전을 실행하는 vCenter Server에서 관리해야 합니다. 또한 수집기 VM을 배포하려면 5.5 이상을 실행하는 ESXi 호스트가 하나 필요합니다.
- **vCenter Server 계정**: vCenter Server에 액세스하려면 읽기 전용 계정이 필요합니다. Azure Migrate는 이 계정을 사용하여 온-프레미스 VM을 검색합니다.
- **권한**: vCenter Server에서는 파일을 .OVA 형식으로 가져와서 VM을 만들 수 있는 권한이 필요합니다.

## <a name="create-an-account-for-vm-discovery"></a>VM 검색을 위한 계정 만들기

Azure Migrate에서 평가를 위해 VM을 자동으로 검색하려면 VMware 서버에 대한 액세스가 필요합니다. 다음 속성으로 VMware 계정을 만듭니다. 이 계정은 Azure Migrate를 설치하는 동안 지정합니다.

- 사용자 유형: 읽기 전용 사용자(최소)
- 사용 권한: 데이터 센터 개체 –> 자식 개체에 전파, role=Read 전용
- 세부 정보: 사용자는 데이터 센터 수준에서 할당되며 데이터 센터의 모든 개체에 대한 액세스 권한이 있습니다.
- 액세스를 제한하려면 자식에 전파 개체를 사용하여 액세스 권한 없음 역할을 자식 개체(vSphere 호스트, 데이터 저장소, VM 및 네트워크)에 할당합니다.


## <a name="sign-in-to-the-azure-portal"></a>Azure Portal에 로그인

[Azure Portal](https://portal.azure.com)에 로그인합니다.

## <a name="create-a-project"></a>프로젝트 만들기

1. Azure Portal에서 **리소스 만들기**를 클릭합니다.
2. **Azure Migrate**를 검색하고 검색 결과에서 서비스 **Azure Migrate**을 선택합니다. 그런 다음, **만들기**를 클릭합니다.
3. 프로젝트에 대해 프로젝트 이름과 Azure 구독을 지정합니다.
4. 새 리소스 그룹을 만듭니다.
5. 프로젝트를 만들 지리를 지정한 다음, **만들기**를 클릭합니다. 다음 지역에서만 Azure Migrate 프로젝트를 만들 수 있습니다. 그러나 대상 Azure 위치에 대한 마이그레이션을 계속 계획할 수 있습니다. 프로젝트에 대해 지정된 지리는 온-프레미스 VM에서 수집된 메타데이터를 저장하는 데 사용됩니다.

**지리** | **스토리지 위치**
--- | ---
Azure Government | 미국 정부 버지니아
아시아 | 동남아시아
유럽 | 북유럽 또는 유럽 서부
미국 | 미국 동부 또는 미국 중서부

![Azure Migrate](./media/tutorial-assessment-vmware/project-1.png)


## <a name="download-the-collector-appliance"></a>수집기 어플라이언스를 다운로드 합니다.

Azure Migrate는 수집기 어플라이언스로 알려진 온-프레미스 VM을 만듭니다. 이 VM은 온-프레미스 VMware VM을 검색하고 그것들에 대한 메타데이터를 Azure Migrate 서비스에 보냅니다. 수집기 어플라이언스를 설정하려면 OVA 파일을 다운로드하여 VM을 만들기 위해 온-프레미스 vCenter 서버로 가져옵니다.

1. Azure Migrate 프로젝트에서 **시작** > **검색 및 평가** > **컴퓨터 검색**을 클릭합니다.
2. **머신 검색**에서 **다운로드**를 클릭하여 어플라이언스를 다운로드합니다.

    Azure Migrate 어플라이언스는 vCenter Server와 통신하며 각 VM에 대한 실시간 사용률 데이터를 수집하도록 온-프레미스 환경을 지속적으로 프로파일링합니다. 각 메트릭(CPU 사용률, 메모리 사용률 등)에 대한 최대 카운터를 수집합니다. 이 모델은 성능 데이터를 수집을 위해 vCenter Server의 통계 설정을 사용하지 않습니다. 언제든지 어플라이언스에서 연속 프로파일링을 중지할 수 있습니다.

    > [!NOTE]
    > 일회성 검색 어플라이언스는 성능 데이터 지점 가용성에 대한 vCenter Server의 통계 설정에 의존하고 평균 성능 카운터를 수집함에 따라 Azure로 마이그레이션할 VM의 크기가 부족해진다는 이유로 현재는 사용되지 않습니다.

    **빠른 평가:** 지속적인 검색 어플라이언스를 사용하면 검색이 완료되고 난 후(VM의 수에 따라 몇 시간 소요) 즉시 평가를 만들 수 있습니다. 검색을 시작할 때 성능 데이터 수집이 시작되므로 빠른 평가를 원하는 경우 평가의 크기 조정 기준으로 *온-프레미스로*를 선택해야 합니다. 성능 기반 평가의 경우 신뢰할 수 있는 크기 권장 사항을 얻으려면 검색을 시작한 후 하루 이상 대기하는 것이 좋습니다.

    어플라이언스는 성능 데이터만 지속적으로 수집하며 온-프레미스 환경에서의 구성 변경 사항은 감지하지 않습니다(즉, VM 추가, 삭제, 디스크 추가 등). 온-프레미스 환경에서 구성 변경이 있으면 다음을 통해 포털에 변경 내용을 반영할 수 있습니다.

    - 항목 추가(VM, 디스크, 코어 등): 이러한 변경을 Azure Portal에 반영하려면 어플라이언스에서 검색을 중지했다가 다시 시작합니다. 이렇게 하면 Azure Migrate 프로젝트에서 변경 내용이 업데이트됩니다.

    - VM 삭제: 어플라이언스 설계 방식으로 인해 검색을 중지했다 시작해도 VM 삭제가 반영되지 않습니다. 이후 검색의 데이터는 기존 검색에 추가되는 것이지 기존 검색을 덮어쓰는 것이 아니기 때문입니다. 이 경우에는 그룹에서 VM을 제거하고 평가를 다시 계산하여 포털에서 VM을 간단히 무시하면 됩니다.


3. **프로젝트 자격 증명 복사**에서 프로젝트 ID 및 키를 복사합니다. 수집기를 구성할 때 이러한 항목들이 필요합니다.

    ![.ova 파일 다운로드](./media/tutorial-assessment-vmware/download-ova.png)

### <a name="verify-the-collector-appliance"></a>수집기 어플라이언스를 확인합니다.

배포하기 전에 OVA 파일이 안전한지 확인합니다.

1. 파일을 다운로드한 컴퓨터에서 관리자 명령 창을 엽니다.
2. 다음 명령을 실행하여 OVA에 대한 해시를 생성합니다.
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - 사용 예: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. 생성된 해시는 이러한 설정에 일치해야 합니다.

#### <a name="continuous-discovery"></a>연속 검색

  OVA 버전 1.0.10.15의 경우

  **알고리즘** | **해시 값**
    --- | ---
    MD5 | dfa1838b1e64f7cde51915927220cf48
    SHA1 | 24bdbd9c37c7366567ff252db3a37a13dda9de42
    SHA256 | e9f8f16ceb970c27dd068f5a5f7a4b2fd336f2820e9d6247d510ba6824e3f06c

  OVA 버전 1.0.10.11의 경우

  **알고리즘** | **해시 값**
    --- | ---
    MD5 | 5f6b199d8272428ccfa23543b0b5f600
    SHA1 | daa530de6e8674a66a728885a7feb3b0a2e8ccb0
    SHA256 | 85da50a21a7a6ca684418a87ccc1dd4f8aab30152c438a17b216ec401ebb3a21

  OVA 버전 1.0.10.9의 경우

  **알고리즘** | **해시 값**
  --- | ---
  MD5 | 169f6449cc1955f1514059a4c30d138b
  SHA1 | f8d0a1d40c46bbbf78cd0caa594d979f1b587c8f
  SHA256 | d68fe7d94be3127eb35dd80fc5ebc60434c8571dcd0e114b87587f24d6b4ee4d

  OVA 버전 1.0.10.4의 경우

  **알고리즘** | **해시 값**
  --- | ---
  MD5 | 2ca5b1b93ee0675ca794dd3fd216e13d
  SHA1 | 8c46a52b18d36e91daeae62f412f5cb2a8198ee5
  SHA256 | 3b3dec0f995b3dd3c6ba218d436be003a687710abab9fcd17d4bdc90a11276be


#### <a name="one-time-discovery-deprecated-now"></a>일회성 검색(현재 사용 중단)

이 모델은 현재 사용되지 않습니다. 기존 어플라이언스에서는 제공됩니다.

  OVA 버전 1.0.9.15의 경우

  **알고리즘** | **해시 값**
  --- | ---
  MD5 | e9ef16b0c837638c506b5fc0ef75ebfa
  SHA1 | 37b4b1e92b3c6ac2782ff5258450df6686c89864
  SHA256 | 8a86fc17f69b69968eb20a5c4c288c194cdcffb4ee6568d85ae5ba96835559ba

  OVA 버전 1.0.9.14의 경우

  **알고리즘** | **해시 값**
  --- | ---
  MD5 | 6d8446c0eeba3de3ecc9bc3713f9c8bd
  SHA1 | e9f5bdfdd1a746c11910ed917511b5d91b9f939f
  SHA256 | 7f7636d0959379502dfbda19b8e3f47f3a4744ee9453fc9ce548e6682a66f13c

  OVA 버전 1.0.9.12의 경우

  **알고리즘** | **해시 값**
  --- | ---
  MD5 | d0363e5d1b377a8eb08843cf034ac28a
  SHA1 | df4a0ada64bfa59c37acf521d15dcabe7f3f716b
  SHA256 | f677b6c255e3d4d529315a31b5947edfe46f45e4eb4dbc8019d68d1d1b337c2e

## <a name="create-the-collector-vm"></a>수집기 VM을 만듭니다.

다운로드한 파일을 vCenter Server에 가져옵니다.

1. vSphere Client 콘솔에서 **파일** > **OVF 템플릿 배포**를 클릭합니다.

    ![OVF 배포](./media/tutorial-assessment-vmware/vcenter-wizard.png)

2. OVF 템플릿 배포 마법사 > **원본**에서 .ova 파일의 위치를 지정합니다.
3. **이름** 및 **위치**에서 수집기 VM에 대한 익숙한 이름과 VM이 호스트될 인벤토리 개체를 지정합니다.
5. **호스트/클러스터**에서 수집기 VM이 실행될 호스트 또는 클러스터를 지정합니다.
7. 저장소에서 수집기 VM에 대한 저장소 대상을 지정합니다.
8. **디스크 형식**에서 디스크 유형 및 크기를 지정합니다.
9. **네트워크 매핑**에서 수집기 VM이 연결할 네트워크를 지정합니다. 메타데이터를 Azure로 전송하려면 네트워크에 인터넷 연결이 필요합니다.
10. 설정을 검토 및 확인한 다음 **마침**을 클릭합니다.

## <a name="run-the-collector-to-discover-vms"></a>VM을 검색하려면 수집기를 실행합니다.

1. vSphere Client 콘솔에서 VM > **Open Console**을 마우스 오른쪽 단추로 클릭합니다.
2. 어플라이언스에 대한 언어, 표준 시간대 및 기본 암호를 제공합니다.
3. 바탕 화면에서 **수집기 실행** 바로 가기를 클릭합니다.
4. 수집기 UI의 위쪽 표시줄에서 **업데이트 확인**을 클릭하고 수집기가 최신 버전에서 실행되는지를 확인합니다. 그렇지 않으면 링크에서 최신 업그레이드 패키지를 다운로드하고 수집기를 업데이트하도록 선택할 수 있습니다.
5. Azure Migrate Collector에서 **필수 조건 설정**을 엽니다.
   - 마이그레이션할 Azure 클라우드(Azure Global 또는 Azure Government)를 선택합니다.
   - 사용 조건에 동의하고 타사 정보를 읽습니다.
   - 수집기는 VM이 인터넷에 액세스를 수 있는지 확인합니다.
   - VM이 프록시를 통해 인터넷에 액세스하는 경우 **프록시 설정**을 클릭하고, 프록시 주소 및 수신 대기 포트를 지정합니다. 프록시에 인증이 필요한 경우 자격 증명을 지정합니다. 인터넷 연결 요구 사항 및 수집기에서 액세스하는 [URL 목록](https://docs.microsoft.com/azure/migrate/concepts-collector)에 대해 [자세히 알아보세요](https://docs.microsoft.com/azure/migrate/concepts-collector#collector-prerequisites).

     > [!NOTE]
     > 프록시 주소는 http:\//ProxyIPAddress 또는 http:\//ProxyFQDN 형식으로 입력되어야 합니다. HTTP 프록시만 지원됩니다. 가로채는 프록시가 있는 경우 프록시 인증서를 가져오지 않으면 인터넷 연결이 처음에는 실패할 수 있습니다. 수집기 VM에서 프록시 인증서를 신뢰할 수 있는 인증서로 가져와서 이 문제를 해결할 수 있는 방법에 대해 [자세히 알아보세요](https://docs.microsoft.com/azure/migrate/concepts-collector).

   - 수집기는 collectorservice가 실행 중인지 확인합니다. 서비스는 수집기 VM에 기본적으로 설치됩니다.
   - VMware PowerCLI를 다운로드하여 설치합니다.

6. **vCenter Server 세부 정보 지정**에서 다음을 수행합니다.
    - vCenter 서버의 이름(FQDN) 또는 IP 주소를 지정합니다.
    - **사용자 이름** 및 **암호**에서, 수집기가 vCenter 서버에서 VM을 검색하기 위해 사용할 읽기 전용 계정 자격 증명을 지정합니다.
    - **컬렉션 범위**에서 VM 검색에 대한 범위를 선택합니다. 수집기는 지정된 범위 내의 VM만 검색할 수 있습니다. 범위를 특정 폴더, 데이터 센터 또는 클러스터로 설정할 수 있습니다. VM은 1500대 미만이어야 합니다. 더 큰 환경을 검색하는 방법을 [자세히 알아보세요](how-to-scale-assessment.md).

       > [!NOTE]
       > **컬렉션 범위**에는 호스트 및 클러스터의 폴더만 나열됩니다. VM의 폴더는 컬렉션 범위로 직접 선택할 수 없습니다. 그러나 개별 VM에 액세스할 수 있는 vCenter 계정을 사용하여 검색할 수 있습니다. VM의 폴더로 범위를 지정하는 방법을 [자세히 알아보세요](https://docs.microsoft.com/azure/migrate/how-to-scale-assessment#set-up-permissions).

7. **마이그레이션 프로젝트 지정**에서 Azure Migrate 프로젝트 ID를 지정하고 포털에서 복사한 키를 지정합니다. 해당 항목을 복사하지 않은 경우 수집기 VM에서 Azure Portal을 엽니다. 프로젝트 **개요** 페이지에서 **컴퓨터 검색**을 클릭하고 값을 복사합니다.  
8. **수집 진행 상황 보기**에서 검색 상태를 모니터링합니다. Azure Migrate Collector가 어떤 데이터를 수집하는지 [자세히 알아보세요](https://docs.microsoft.com/azure/migrate/concepts-collector).

> [!NOTE]
> 수집기는 "영어(미국)"만을 운영 체제 언어와 수집기 인터페이스 언어로 지원합니다.
> 액세스하려는 컴퓨터에서 설정을 변경하는 경우 평가를 실행하기 전에 검색을 다시 트리거합니다. 수집기에서 이 작업을 수행하려면 **수집 다시 시작** 옵션을 사용합니다. 컬렉션 완료된 후 업데이트된 평가 결과를 얻으려면 포털에서 평가에 대한 **다시 계산** 옵션을 선택합니다.


### <a name="verify-vms-in-the-portal"></a>포털에서 VM 확인

수집기 어플라이언스는 온-프레미스 환경을 지속적으로 프로파일링하고 1시간 간격으로 성능 데이터를 보냅니다. 검색을 시작하고 1시간 후 포털에서 머신을 볼 수 있습니다.

1. 프로젝트에서 **관리** > **컴퓨터**를 클릭합니다.
2. 검색하려는 VM이 포털에 나타나는지 확인합니다.


## <a name="create-and-view-an-assessment"></a>평가 만들기 및 보기

VM을 포털에서 검색한 후 그룹화하여 평가를 만듭니다. VM이 포털에서 검색되면 온-프레미스 평가로 즉시 만들 수 있습니다. 신뢰할 수 있는 크기 권장 사항을 얻으려면 성능 기반 평가를 만들기 전에 하루 이상 대기하는 것이 좋습니다.

1. 프로젝트 **개요** 페이지에서 **+평가 만들기**를 클릭합니다.
2. 평가 속성을 검토하려면 **모두 보기**를 클릭합니다.
3. 그룹을 만들고 그룹 이름을 지정합니다.
4. 그룹에 추가하려는 컴퓨터를 선택합니다.
5. **평가 만들기**를 클릭하고, 그룹 및 평가를 만듭니다.
6. 평가를 만든 후 **개요** > **대시보드**에서 봅니다.
7. **평가 내보내기**를 클릭하고, Excel 파일로 다운로드합니다.

> [!NOTE]
> 평가를 만들기 전에 검색을 시작한 후 하루 이상 기다리는 것이 좋습니다. 최신 성능 데이터로 기존 평가를 업데이트하려는 경우 평가에서 **다시 계산** 명령을 사용하여 업데이트할 수 있습니다.

### <a name="assessment-details"></a>평가 세부 정보

평가에는 온-프레미스 VM의 Azure 호환성, Azure에서 VM을 실행하기에 적합한 크기, 예상 월별 Azure 비용에 대한 정보가 포함됩니다.

![평가 보고서](./media/tutorial-assessment-vmware/assessment-report.png)

#### <a name="azure-readiness"></a>Azure 준비 상태

평가의 Azure 준비 상태 보기는 각 VM의 준비 상태를 보여줍니다. VM의 속성에 따라 각 VM이 다음과 같이 표시됩니다.
- Azure 준비 완료
- 조건부 Azure 준비 완료
- Azure를 사용할 준비 안 됨
- 준비 상태 알 수 없음

준비된 VM의 경우 Azure Migrate는 Azure에서의 VM 크기를 권장합니다. Azure Migrate에서 권장하는 크기는 평가 속성에 지정된 크기 조정 기준에 따라 달라집니다. 크기 조정 기준이 성능 기반인 경우 VM(CPU 및 메모리) 및 디스크(IOPS 및 처리량)의 성능 기록을 고려하여 권장 크기가 결정됩니다. 크기 조정 조건이 '온-프레미스 그대로'인 경우 Azure Migrate는 VM 및 디스크에 대해 성능 데이터를 사용하지 않습니다. Azure에서 VM 크기의 권장 사항은 VM 온-프레미스의 크기를 확인하여 수행됩니다. 디스크 크기 조정은 평가 속성에 지정된 저장소 형식에 따라 수행됩니다(기본값은 프리미엄 디스크임). Azure Migrate의 크기 조정 방식에 대해 [자세히 알아보세요](concepts-assessment-calculation.md).

VM 상태가 Azure 준비 완료 또는 조건부 Azure 준비 완료인 경우 Azure Migrate는 준비 상태 문제를 설명하고 수정 단계를 제공합니다.

Azure Migrate가 Azure 준비 상태를 알 수 없는(데이터를 사용할 수 없어서) VM은 준비 상태 알 수 없음으로 표시됩니다.

Azure 준비 상태 및 크기 조정 외에도, Azure Migrate는 VM 마이그레이션에 사용할 수 있는 도구를 추천합니다. 여기에는 온-프레미스 환경에서 심층 검색이 필요합니다. 온-프레미스 컴퓨터에 에이전트를 설치하여 심층 검색을 수행하는 방법을 [알아보세요](how-to-get-migration-tool.md). 온-프레미스 컴퓨터에 에이전트가 설치되어 있지 않으면 [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)를 사용한 리프트 앤 시프트 마이그레이션이 좋습니다. 온-프레미스 컴퓨터에 에이전트가 설치되어 있지 않은 경우 Azure Migrate은 컴퓨터 내부에서 실행 중인 프로세스를 확인하고 컴퓨터가 데이터베이스 컴퓨터인지 여부를 식별합니다. 컴퓨터가 데이터베이스 컴퓨터이면 마이그레이션 도구로 [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview)가 좋으며, 그렇지 않으면 Azure Site Recovery가 좋습니다.

  ![평가 준비 상태](./media/tutorial-assessment-vmware/assessment-suitability.png)  

#### <a name="monthly-cost-estimate"></a>월별 예상 비용

이 보기는 각 머신의 세부 사항과 함께 Azure에서 VM을 실행하는 데 필요한 총 계산 및 스토리지 비용을 보여 줍니다. 예상 비용은 Azure Migrate에서 컴퓨터, 컴퓨터의 디스크 및 평가 속성에 대해 수행한 크기 권장 사항을 고려하여 계산됩니다.

> [!NOTE]
> Azure Migrate가 제공하는 비용 예측은 온-프레미스 VM을 Azure IaaS(Infrastructure as a service) VM으로 실행하기 위한 것입니다. Azure Migrate는 PaaS(Platform as a Service) 또는 SaaS(Software as a Service) 비용을 고려하지 않습니다.

계산 및 스토리지에 대한 월별 예상 비용은 그룹의 모든 VM에 대해 집계됩니다.

![VM 비용 평가](./media/tutorial-assessment-vmware/assessment-vm-cost.png)

#### <a name="confidence-rating"></a>신뢰 등급

Azure Migrate의 각 성능 기준 평가는 별 1개~5개 사이의 신뢰 등급에 연결됩니다(별 1개가 가장 낮고 5개가 가장 높음). 신뢰 등급은 평가 계산에 필요한 데이터 요소의 가용성에 따라 평가에 할당됩니다. 평가의 신뢰 등급은 Azure Migrate에서 제공하는 권장 크기의 신뢰성을 추정하는 데 도움이 됩니다. 신뢰 등급은 온-프레미스 평가에 "그대로" 적용되지 않습니다.

성능 기반 크기 조정의 경우 Azure Migrate에는 VM의 CPU 및 메모리 사용률 데이터가 필요합니다. 또한 VM에 연결된 각 디스크에는 디스크 IOPS 및 처리량 데이터가 필요합니다. 마찬가지로 VM에 연결된 각 네트워크 어댑터의 경우 성능 기반 크기 조정을 수행하려면 Azure Migrate에는 네트워크 입/출력이 필요합니다. 위의 사용률 데이터를 vCenter Server에서 사용할 수 없는 경우 Azure Migrate가 권장하는 크기의 신뢰성이 떨어질 수 있습니다. 사용 가능한 데이터 요소의 백분율에 따라 아래와 같이 평가의 신뢰 등급이 제공됩니다.

   **데이터 요소 가용성** | **신뢰 등급**
   --- | ---
   0%-20% | 별 1개
   21%-40% | 별 2개
   41%-60% | 별 3개
   61%-80% | 별 4개
   81%-100% | 별 5개

다음과 같은 이유 중 하나로 인해 평가에 일부 데이터 요소가 없을 수도 있습니다.

- 평가를 작성하는 기간 동안 환경을 프로파일링하지 않았습니다. 예를 들어 성능 기간을 1일로 설정하여 평가를 작성하는 경우 검색 시작 후 1일 이상 기다려야 모든 데이터 요소가 수집됩니다.

- 평가 계산 기간에 일부 VM이 종료되었습니다. 평가 기간 중 일부 VM이 꺼지면 해당 기간의 성능 데이터는 수집할 수 없습니다.

- 평가 계산 기간에 일부 VM이 생성되었습니다. 예를 들어 마지막 1달의 성능 기록에 대한 평가를 만들려고 하는데, 일부 VM이 불과 일주일 전에 환경에서 생성되었습니다. 이 경우 전체 기간에 대한 새 VM의 성능 기록이 제공되지 않습니다.

> [!NOTE]
> 평가의 신뢰 등급이 5개 별인 경우 어플라이언스가 환경을 프로파일링하도록 하루 이상 기다린 다음, 평가를 *다시 계산*합니다. 위의 단계를 수행할 수 없는 경우 성능 기반 크기 조정의 신뢰성이 떨어질 수 있으므로 평가 속성을 변경하여 *온-프레미스로 크기 조정*으로 전환하는 것이 좋습니다.

## <a name="next-steps"></a>다음 단계

- 요구 사항에 따라 평가를 사용자 지정하는 방법을 [알아보세요](how-to-modify-assessment.md).
- [컴퓨터 종속성 매핑](how-to-create-group-machine-dependencies.md)을 사용하여 정확도 높은 평가 그룹을 만드는 방법을 알아봅니다.
- 평가를 계산하는 방법에 대해 [자세히 알아봅니다](concepts-assessment-calculation.md).
- 대규모 VMware 환경을 검색하고 평가하는 방법을 [알아봅니다](how-to-scale-assessment.md).
- Azure Migrate의 FAQ에 대한 [자세한 내용](resources-faq.md)