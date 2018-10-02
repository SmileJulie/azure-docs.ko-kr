---
title: VMware에서 Azure Data Box Gateway 프로비전 자습서 | Microsoft Docs
description: Azure Data Box Gateway를 배포하는 두 번째 자습서에는 VMware에서 가상 장치를 프로비전하는 작업이 포함됩니다.
services: databox-edge-gateway
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox-edge-gateway
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/24/2018
ms.author: alkohli
ms.custom: ''
ms.openlocfilehash: d0c6f8723909b71501894c9363932c752c1e130c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46989858"
---
# <a name="tutorial-provision-azure-data-box-gateway-in-vmware-preview"></a>자습서: VMware에서 Azure Data Box Gateway 프로비전(미리 보기)

## <a name="overview"></a>개요

이 자습서는 VMware ESXi 6.0 또는 6.5를 실행하는 호스트 시스템에서 Data Box Gateway를 프로비전하는 방법을 설명합니다. 

가상 장치를 프로비전하고 연결하려면 관리자 권한이 필요합니다. 프로비전 및 초기 설정을 완료하는 데 10분 정도가 소요됩니다.

이 자습서에서는 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * 호스트가 최소 장치 요구 사항을 충족하는지 확인
> * 하이퍼바이저에서 가상 장치 프로비전
> * 가상 장치 시작 및 IP 주소 가져오기

Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)을 만듭니다.

> [!IMPORTANT]
> - Data Box Gateway는 미리 보기 상태로 제공되고 있습니다. 이 솔루션을 주문하고 배포하기 전에 [미리 보기에 대한 Azure 서비스 약관](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)을 검토하세요.

## <a name="prerequisites"></a>필수 조건

VMware ESXi 6.0 또는 6.5를 실행하는 호스트 시스템에서 가상 장치를 프로비전하기 위한 필수 구성 요소는 다음과 같습니다.

### <a name="for-the-data-box-gateway-resource"></a>Data Box Gateway 리소스의 경우

시작하기 전에 다음 사항을 확인합니다.

* [Data Box Gateway에 대한 포털 준비](data-box-gateway-deploy-prep.md)의 모든 단계를 완료했습니다.
* [Data Box Gateway에 대한 포털 준비](data-box-gateway-deploy-prep.md)에 설명된 대로 Azure Portal에서 VMware에 대한 가상 장치 이미지를 다운로드했습니다.

  > [!IMPORTANT]
  > Data Box Gateway에서 실행되는 소프트웨어는 Data Box Gateway 리소스에서만 사용할 수 있습니다.

### <a name="for-the-data-box-gateway-virtual-device"></a>Data Box Gateway 가상 장치의 경우

가상 장치를 배포하기 전에 다음 사항을 확인해야 합니다.

* 장치를 프로비전하는 데 사용될 수 있는 VMware(ESXi 6.0 또는 6.5)를 실행하는 호스트 시스템에 액세스할 수 있습니다.
* 가상 디스크 프로비전을 위해 호스트 시스템에서 다음 리소스를 전용할 수 있습니다.

  * 코어 4개 이상
  * RAM 8GB 이상
  * 네트워크 인터페이스 하나
  * 250GB OS 디스크
  * 시스템 데이터용 가상 디스크 2TB

### <a name="for-the-network-in-datacenter"></a>데이터 센터 네트워크의 경우

시작하기 전에

- Data Box Gateway를 배포하기 위한 네트워킹 요구 사항을 검토하고, 요구 사항에 따라 데이터 센터 네트워크를 구성합니다. 자세한 내용은 [Data Box Gateway 네트워킹 요구 사항](data-box-gateway-system-requirements.md#networking-requirements)을 참조하세요.
- 장치가 최적으로 작동할 수 있도록 최소 인터넷 대역폭이 20Mbps인지 확인합니다.

## <a name="check-the-host-system"></a>호스트 시스템 확인

가상 장치를 만들려면 다음이 필요합니다.

* VMware ESXi Server 6.0 또는 6.5를 실행하는 호스트 시스템에 대한 액세스 호스트 시스템에서는 다음 리소스를 가상 디스크에 전용으로 사용할 수 있습니다.
 
  * 코어 4개 이상
  * RAM 8GB 이상 
  * 인터넷으로 트래픽을 라우팅할 수 있는 네트워크에 연결된 네트워크 인터페이스 하나. 
  * 250GB OS 디스크
  * 데이터용 2TB 가상 디스크
* ESXi 호스트를 관리하기 위한 시스템의 VMware vSphere 클라이언트


## <a name="provision-a-virtual-device-in-hypervisor"></a>하이퍼바이저에서 가상 장치 프로비전

하이퍼바이저에서 가상 장치를 프로비전하려면 다음 단계를 수행합니다.

1. 시스템에서 가상 장치 이미지를 복사합니다. Azure Portal을 통해 이 가상 이미지(두 개 파일)를 다운로드했습니다. 나중에 절차에서 이 이미지를 사용하므로 이미지를 복사한 위치를 적어 둡니다.

2. vSphere 클라이언트를 사용하여 ESXi 서버에 로그인합니다. 가상 머신을 만들려면 관리자 권한이 필요합니다.

   ![](./media/data-box-gateway-deploy-provision-vmware/image1.png)
  
3. ESXi 서버에 VMDK를 업로드합니다. 탐색기 창에서 **저장소**를 선택합니다.

   ![](./media/data-box-gateway-deploy-provision-vmware/image2.png)

4. 오른쪽 창의 **데이터 저장소**아래에서 VMDK를 업로드할 데이터 저장소를 선택합니다. 데이터 저장소는 VMFS 5 형식이어야 합니다. 데이터 저장소에는 OS 및 데이터 디스크에 충분한 여유 공간도 있어야 합니다.
   
5. **데이터 저장소 찾아보기**를 마우스 오른쪽 단추를 클릭하고 선택합니다.

   ![](./media/data-box-gateway-deploy-provision-vmware/image3.png)

6. **데이터 저장소 브라우저** 창이 표시됩니다.

   ![](./media/data-box-gateway-deploy-provision-vmware/image4.png)

7. 도구 모음에서 **디렉터리 만들기** 아이콘을 클릭하여 새 폴더를 만듭니다. 폴더 이름을 지정하고 해당 이름을 적어둡니다. 나중에 가상 머신을 만들 때 이 폴더 이름을 사용합니다(권장 모범 사례). **디렉터리 만들기**를 클릭합니다.

   ![](./media/data-box-gateway-deploy-provision-vmware/image5.png)

8. 새 폴더가 **데이터 저장소 브라우저**의 왼쪽 창에 표시됩니다. **업로드** 아이콘을 클릭하고 **파일 업로드**를 선택합니다.

    ![](./media/data-box-gateway-deploy-provision-vmware/image6.png)

9. 다운로드한 VMDK 파일을 찾아서 가리킵니다. 두 가지 파일이 있습니다. 업로드할 파일을 선택합니다.

    ![](./media/data-box-gateway-deploy-provision-vmware/image7.png)

10. **열기**를 클릭합니다. VMDK 파일을 지정된 데이터 저장소에 업로드하는 작업이 시작됩니다. 파일 업로드에 몇 분 정도 걸릴 수 있습니다.
11. 업로드가 완료되면 사용자가 만든 폴더의 데이터 저장소에 파일이 표시됩니다. 두 번째 VMDK 파일을 동일한 데이터 저장소에 업로드합니다. 두 파일이 모두 업로드되면 두 개의 파일이 단일 파일로 병합됩니다. 그러면 디렉터리에 단일 파일이 표시됩니다.

    ![](./media/data-box-gateway-deploy-provision-vmware/image8.png)

12. vSphere 클라이언트 창으로 돌아갑니다. 탐색기 창에서 **Virtual Machines**를 선택합니다. 오른쪽 창에서 **VM 만들기/등록**을 클릭합니다.

    ![](./media/data-box-gateway-deploy-provision-vmware/image9.png)

13. **새 Virtual Machine 만들기**가 표시됩니다. 만들기 형식 선택에서 **새 가상 머신 만들기**를 선택하고 **다음**을 클릭합니다.
    ![](./media/data-box-gateway-deploy-provision-vmware/image10.png)

14. **이름과 OS 이름 및 위치 선택** 페이지에서 가상 머신의 **이름**을 지정합니다. 이 이름은 앞서 7단계에서 지정한 폴더 이름(권장 모범 사례)과 일치해야 합니다. **게스트 OS 제품군**을 Windows로 선택하고 **게스트 OS 버전**을 Microsoft Windows Server 2016(64비트)로 선택합니다. **다음**을 클릭합니다.

    ![](./media/data-box-gateway-deploy-provision-vmware/image11.png)

15. **저장소 선택** 페이지에서 VM을 프로비전하는 데 사용할 데이터 저장소를 선택합니다. **다음**을 클릭합니다.

    ![](./media/data-box-gateway-deploy-provision-vmware/image12.png)
16. **사용자 지정 설정** 페이지에서 설정는 **CPU**를 4로 설정하고, **메모리**를 8192MB(이상)으로 설정하고, **하드 디스크 1**을 2TB(이상)으로 설정합니다. 추가할 **SCSI 하드 디스크**의 형식을 선택합니다. 이 경우에는 LSI 논리 SAS이었습니다. **정적 IDE 디스크는 지원되지 않습니다.** **하드 디스크 1**은 가상 데이터 디스크입니다. 프로비전한 후에는 디스크를 축소할 수 없습니다.

    ![](./media/data-box-gateway-deploy-provision-vmware/image13.png)

    같은 페이지에서 **하드 디스크 추가**를 클릭한 다음, **기존 하드 디스크**를 선택합니다. 그러면 OS 디스크를 추가합니다. 

     ![](./media/data-box-gateway-deploy-provision-vmware/image14.png)

    **새 하드 디스크**가 표시될 때까지 아래로 스크롤하고 확장하여 설정을 봅니다. **가상 장치 노드**를 **IDE 컨트롤러 0**으로 설정합니다. **다음**을 클릭합니다.

     ![](./media/data-box-gateway-deploy-provision-vmware/image15.png)

27. **Ready to Complete** (완료 준비) 페이지에서 새 가상 머신과 관련된 모든 설정을 검토합니다. CPU가 4개이고, 메모리가 8192MB이고, 네트워크 인터페이스가 1이고, 하드 디스크 2에 IDE 컨트롤러 0이 포함되어 있는지 확인합니다. **Finish**를 클릭합니다. 
   
    ![](./media/data-box-gateway-deploy-provision-vmware/image16.png)
    ![](./media/data-box-gateway-deploy-provision-vmware/image17.png)

가상 머신이 프로비전되어 있습니다. 결과에 대한 알림이 표시되고 새 가상 머신이 VM 목록에 추가됩니다. 

![](./media/data-box-gateway-deploy-provision-vmware/image17.png)

다음 단계는 이 컴퓨터의 전원을 켜고 IP 주소를 가져오는 것입니다.

> [!NOTE]
> 가상 장치에는 VMware 도구를 설치하지 않는 것이 좋습니다(위에서 프로비전됨). VMware 도구를 설치하면 지원되지 않는 구성이 설정됩니다.

## <a name="start-the-virtual-device-and-get-the-ip"></a>가상 장치 시작 및 IP 가져오기

가상 장치를 시작하여 연결하려면 다음 단계를 수행합니다.

#### <a name="to-start-the-virtual-device"></a>가상 장치를 시작하려면
1. 가상 장치를 시작합니다. 오른쪽 창의 VM 목록에서 장치를 선택하고 마우스 오른쪽 단추로 클릭하여 상황에 맞는 메뉴를 표시합니다. **전원**을 선택한 후 **전원 켜기**를 선택합니다. 이렇게 하면 가상 머신의 전원이 켜집니다. 웹 클라이언트의 아래쪽 창에서 상태를 볼 수 있습니다.

    ![](./media/data-box-gateway-deploy-provision-vmware/image19.png)

2. VM을 다시 선택합니다. 마우스 오른쪽 단추로 클릭하고 **콘솔**을 선택한 다음, **새 창에서 열기**를 선택합니다.

    ![](./media/data-box-gateway-deploy-provision-vmware/image20.png)

3. 가상 머신 콘솔이 새 창에서 열립니다. 

    ![](./media/data-box-gateway-deploy-provision-vmware/image21.png)

4. 장치가 실행되면 콘솔 창의 위쪽 가운데 부분에 있는 탭에서 커서를 가리키고 클릭합니다. **게스트 OS > 키 받기 > Ctrl+Alt+Delete**를 선택합니다. 그러면 VM의 잠금을 해제합니다.

   ![](./media/data-box-gateway-deploy-provision-vmware/image22.png)

5. 머신에 로그인하려면 암호를 입력합니다. 기본 암호는 Password1입니다.

   ![](./media/data-box-gateway-deploy-provision-vmware/image23.png)

6. 5-7단계는 DHCP 환경이 아닌 곳에서 부팅하는 경우에만 적용됩니다. DHCP 환경인 경우 이 단계를 건너뛰고 8단계로 이동하세요. 비 DHCP 환경에서 장치를 부팅하는 경우 결과에 대한 메시지가 표시됩니다. **Set-HcsIPAddress cmdlet을 사용하여 네트워크를 구성합니다**. 
   
7. 네트워크를 구성하려면 명령 프롬프트에서 `Get-HcsIpAddress` 명령을 사용하여 가상 장치에서 사용하도록 설정된 네트워크 인터페이스 목록을 표시합니다. 장치에 사용하도록 설정된 네트워크 인터페이스가 하나인 경우에는 `DATA1`이라는 기본 이름이 인터페이스에 할당됩니다.

8. `Set-HcsIpAddress` cmdlet을 사용하여 네트워크를 구성합니다. 아래에 예가 나와 있습니다.

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

9. 초기 설정이 완료된 후 장치가 부팅되면 장치 배너 텍스트가 표시됩니다. 장치 관리를 위해 배너 텍스트에 표시되는 IP 주소와 URL을 기록해 둡니다. IP 주소를 사용하여 가상 장치의 웹 UI에 연결하고 로컬 설정 및 활성화를 완료하겠습니다.

   ![](./media/data-box-gateway-deploy-provision-vmware/image24.png)

장치가 최소 구성 요구 사항을 충족하지 못하면 배너 텍스트에 오류(아래 참고)가 표시됩니다. 최소 요구 사항을 충족하기에 충분한 리소스를 확보하도록 장치 구성을 수정해야 합니다. 그런 다음 다시 시작하고 장치에 연결합니다. [1단계: 호스트 시스템이 최소 가상 장치 요구 사항을 충족하도록 합니다.](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements)에서 최소 구성 요구 사항을 참조하세요.

<!---If you face any other error during the initial configuration using the local web UI, refer to the following workflows:

* Run diagnostic tests to [troubleshoot web UI setup](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).
* [Generate log package and view log files](storsimple-ova-web-ui-admin.md#generate-a-log-package).-->

## <a name="next-steps"></a>다음 단계

이 자습서에서는 다음과 같은 Data Box Gateway 항목에 대해 알아보았습니다.

> [!div class="checklist"]
> * 호스트가 최소 장치 요구 사항을 충족하는지 확인
> * 하이퍼바이저에서 가상 장치 프로비전
> * 가상 장치 시작 및 IP 주소 가져오기

다음 자습서로 이동하여 가상 장치를 연결하고, 설정하고, 활성화하는 방법을 알아보세요.

* [Data Box Gateway에서 공유로 설정 및 연결](data-box-gateway-deploy-connect-setup-activate.md)
