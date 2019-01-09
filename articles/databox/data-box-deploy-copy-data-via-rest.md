---
title: REST API를 통해 Azure Data Box Blob 스토리지에 데이터 복사 | Microsoft Docs
description: REST API를 통해 Azure Data Box Blob 스토리지에 데이터를 복사하는 방법 알아보기
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 12/07/2018
ms.author: alkohli
ms.openlocfilehash: 88880fbfe0f6020a0a982c42b6df0e9eb71b2fc9
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53550449"
---
# <a name="tutorial-copy-data-to-azure-data-box-blob-storage-via-rest-apis"></a>자습서: REST API를 통해 Azure Data Box Blob 스토리지에 데이터 복사  

이 자습서에서는 *http* 또는 *https*를 사용한 REST API를 통해 Azure Data Box Blob 스토리지에 연결하는 절차를 설명합니다. 연결한 후에는 데이터를 Data Box Blob 스토리지에 복사하고 배송할 Data Box를 준비하는 데 필요한 단계에 대해서도 설명합니다.

이 자습서에서는 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * *http* 또는 *https*를 통해 Data Box Blob 스토리지에 연결
> * Data Box에 데이터 복사
> * 배송 준비

## <a name="prerequisites"></a>필수 조건

시작하기 전에 다음 사항을 확인합니다.

1. [자습서: Azure Data Box 설정](data-box-deploy-set-up.md)을 완료했습니다.
2. Data Box를 받았고 포털의 주문 상태가 **배달됨**입니다.
3. [Data Box Blob 스토리지의 시스템 요구 사항](data-box-system-requirements-rest.md)을 검토했으며 API, SDK 및 도구의 지원 버전에 대해 잘 알고 있습니다.
4. Data Box에 복사할 데이터가 포함된 호스트 컴퓨터에 액세스했습니다. 호스트 컴퓨터는 다음 사항이 필수입니다.
    - [지원되는 운영 체제](data-box-system-requirements.md)를 실행합니다.
    - 고속 네트워크에 연결되어 있어야 합니다. 10GbE 연결이 하나 이상 있는 것이 좋습니다. 10GbE 연결을 사용할 수 없으면 1GbE 데이터 링크를 사용할 수 있지만, 이 경우 복사 속도가 떨어집니다.
5. 호스트 컴퓨터에서 [AzCopy 7.1.0을 다운로드](https://aka.ms/azcopyforazurestack20170417)합니다. AzCopy를 사용하여 호스트 컴퓨터에서 Azure Data Box Blob 스토리지에 데이터를 복사합니다.


## <a name="connect-to-data-box-blob-storage"></a>Data Box Blob 스토리지에 연결

*http* 또는 *https*를 통해 Data Box Blob 스토리지에 연결할 수 있습니다. 일반적으로 *https*는 Data Box Blob 스토리지에 연결하기 위한 안전하고 권장되는 방법입니다. *Http*는 신뢰할 수 있는 네트워크를 통해 연결할 때 사용됩니다. *http* 또는 *https* 중 무엇으로 Data Box Blob 스토리지에 연결했는지에 따라 단계가 달라질 수 있습니다.

## <a name="connect-via-http"></a>http를 통해 연결

*http*를 통해 Data Box Blob 스토리지 REST API에 연결하려면 다음 단계를 수행해야 합니다.

- 디바이스 IP 및 Blob 서비스 엔드포인트를 원격 호스트에 추가
- 타사 소프트웨어 구성 및 연결 확인

이러한 각 단계는 다음 섹션에 설명되어 있습니다.

#### <a name="add-device-ip-address-and-blob-service-endpoint-to-the-remote-host"></a>디바이스 IP 주소 및 Blob 서비스 엔드포인트를 원격 호스트에 추가

[!INCLUDE [data-box-add-device-ip](../../includes/data-box-add-device-ip.md)]

#### <a name="configure-partner-software-and-verify-connection"></a>파트너 소프트웨어 구성 및 연결 확인

[!INCLUDE [data-box-configure-partner-software](../../includes/data-box-configure-partner-software.md)]

[!INCLUDE [data-box-verify-connection](../../includes/data-box-verify-connection.md)]

## <a name="connect-via-https"></a>https를 통해 연결

https를 통해 Azure Blob 스토리지 REST API에 연결하려면 다음 단계를 수행해야 합니다.

- Azure Portal에서 인증서 다운로드
- 원격 관리를 위해 호스트 컴퓨터 준비
- 디바이스 IP 및 Blob 서비스 엔드포인트를 원격 호스트에 추가
- 타사 소프트웨어 구성 및 연결 확인

이러한 각 단계는 다음 섹션에 설명되어 있습니다.

### <a name="download-certificate"></a>인증서 다운로드

Azure Portal을 사용하여 인증서를 다운로드합니다.

1. Azure Portal에 로그인합니다.
2. 해당 Data Box 주문으로 이동하고 **일반 > 디바이스 세부 정보**로 이동합니다.
3. **디바이스 자격 증명**에서 **디바이스에 대한 API 액세스**로 이동합니다. **다운로드**를 클릭합니다. 이 작업은 **<your order name>.cer** 인증서 파일을 다운로드합니다. 이 파일을 **저장**합니다. 디바이스에 연결하는 데 사용할 클라이언트 또는 호스트 컴퓨터에 이 인증서를 설치합니다.

    ![Azure Portal에서 인증서 다운로드](media/data-box-deploy-copy-data-via-rest/download-cert-1.png)
 
### <a name="prepare-the-host-for-remote-management"></a>원격 관리를 위해 호스트 준비

다음 단계에 따라 *https* 세션을 사용하는 원격 연결을 위해 Windows 클라이언트를 준비합니다.

- 클라이언트 또는 원격 호스트의 루트 저장소로 .cer 파일을 가져옵니다.
- 디바이스 IP 주소 및 Blob 서비스 엔드포인트를 Windows 클라이언트에서 호스트 파일에 추가합니다.

아래에서는 앞서 나온 각 절차에 대해 설명합니다.

#### <a name="import-the-certificate-on-the-remote-host"></a>원격 호스트에서 인증서 가져오기

Windows PowerShell 또는 Windows Server UI를 사용하여 호스트 시스템에서 인증서를 가져오고 설치할 수 있습니다.

**PowerShell 사용**

1. 관리자 권한으로 Windows PowerShell 세션을 시작합니다.
2. 명령 프롬프트에 다음을 입력합니다.

    ```
    Import-Certificate -FilePath C:\temp\localuihttps.cer -CertStoreLocation Cert:\LocalMachine\Root
    ```

**Windows Server UI 사용**

1.  .cer 파일을 마우스 오른쪽 단추로 클릭하고 **인증서 설치**를 선택합니다. 인증서 가져오기 마법사가 시작됩니다.
2.  **저장소 위치**에 대해 **로컬 컴퓨터**를 선택하고 **다음**을 클릭합니다.

    ![PowerShell을 사용하여 인증서 가져오기](media/data-box-deploy-copy-data-via-rest/import-cert-ws-1.png)

3.  **모든 인증서를 다음 저장소에 저장**을 선택하고 **찾아보기**를 클릭합니다. 원격 호스트의 루트 저장소로 이동한 후 **다음**을 클릭합니다.

    ![PowerShell을 사용하여 인증서 가져오기](media/data-box-deploy-copy-data-via-rest/import-cert-ws-2.png)

4.  **Finish**를 클릭합니다. 가져오기에 성공했음을 알리는 메시지가 나타납니다.

    ![PowerShell을 사용하여 인증서 가져오기](media/data-box-deploy-copy-data-via-rest/import-cert-ws-3.png)

### <a name="to-add-device-ip-address-and-blob-service-endpoint-to-the-remote-host"></a>디바이스 IP 주소 및 Blob 서비스 엔드포인트를 원격 호스트에 추가하려면

수행할 단계는 *http*를 통해 연결하는 동안 사용했던 단계와 동일합니다.

### <a name="configure-partner-software-to-establish-connection"></a>파트너 소프트웨어를 구성하여 연결 설정

수행할 단계는 *http*를 통해 연결하는 동안 사용했던 단계와 동일합니다. 유일한 차이점은 *http 옵션 사용*을 선택하지 않은 상태로 두어야 한다는 것입니다.

## <a name="copy-data-to-data-box"></a>Data Box에 데이터 복사

Data Box Blob 스토리지에 연결한 후 다음 단계는 데이터를 복사하는 것입니다. 데이터를 복사하기 전에, 다음 고려 사항을 검토합니다.

-  데이터를 복사하는 동안 데이터 크기가 [Azure 저장소 및 Data Box 제한](data-box-limits.md)에 설명된 크기 제한을 준수해야 합니다.
- Data Box에 의해 업로드되는 데이터가 Data Box 외부의 다른 애플리케이션에 의해 동시에 업로드되는 경우 업로드 작업이 실패하고 데이터 손상이 발생할 수 있습니다.

이 자습서에서는 Data Box Blob 스토리지에 데이터를 복사하는 데 AzCopy를 사용합니다. Azure Storage 탐색기(GUI 기반 도구를 선호하는 경우) 또는 파트너 소프트웨어를 사용하여 데이터를 복사할 수도 있습니다.
복사 절차에는 다음 단계가 있습니다.

- 컨테이너 만들기
- 폴더의 콘텐츠를 Data Box Blob 스토리지에 업로드
- Data Box Blob 스토리지에 수정된 파일 업로드

이러한 각 단계는 다음 섹션에서 자세히 설명합니다.

### <a name="create-a-container"></a>컨테이너 만들기

Blob은 항상 컨테이너에 업로드하므로 첫 번째 단계는 컨테이너 만들기입니다. 컨테이너는 컴퓨터의 디렉터리에서 파일을 구성하는 것처럼 Blob을 구성합니다. 다음 단계에 따라 Blob 컨테이너를 만듭니다.

1. 저장소 탐색기를 엽니다.
2. 왼쪽 창에서 blob 컨테이너를 만들고자 하는 곳의 저장소 계정을 확장합니다.
3. 마우스 오른쪽 단추로 **Blob 컨테이너**를 클릭하고, 상황에 맞는 메뉴에서 **Blob 컨테이너 만들기**를 선택합니다.

   ![Blob 컨테이너 상황에 맞는 메뉴 만들기](media/data-box-deploy-copy-data-via-rest/create-blob-container-1.png)

4. 텍스트 상자가 **Blob 컨테이너** 폴더 아래에 표시됩니다. Blob 컨테이너에 대한 이름을 입력합니다. Blob 컨테이너 이름 명명 규칙 및 제한 사항에 대한 정보는 [컨테이너 만들기 및 사용 권한 설정](/articles/storage/blobs/storage-quickstart-blobs-dotnet.md#create-the-container-and-set-permissions)을 참조하세요.
5. Blob 컨테이너 만들기가 끝나면 **Enter** 키를 누르거나 **Esc** 키를 눌러 취소합니다. Blob 컨테이너가 성공적으로 만들어졌다면 선택한 스토리지 계정에 대해 **Blob 컨테이너** 폴더 아래에 표시됩니다.

   ![만든 Blob 컨테이너](media/data-box-deploy-copy-data-via-rest/create-blob-container-2.png)

### <a name="upload-contents-of-a-folder-to-data-box-blob-storage"></a>폴더의 콘텐츠를 Data Box Blob 스토리지에 업로드

AzCopy를 사용하여 폴더의 모든 파일을 Windows 또는 Linux의 Blob 스토리지에 업로드합니다. 폴더의 모든 Blob을 업로드하려면 다음 AzCopy 명령을 입력합니다.

#### <a name="linux"></a>Linux

    azcopy \
        --source /mnt/myfolder \
        --destination https://data-box-storage-account-name.blob.device-serial-no.microsoftdatabox.com/container-name/files/ \
        --dest-key <key> \
        --recursive

#### <a name="windows"></a> Windows

    AzCopy /Source:C:\myfolder /Dest:https://data-box-storage-account-name.blob.device-serial-no.microsoftdatabox.com/container-name/files/ /DestKey:<key> /S


`<key>`를 사용자의 계정 키로 바꿉니다. 계정 키를 가져오려면 Azure Portal에서 스토리지 계정으로 이동합니다. **설정 > 액세스 키**로 이동하고 키를 선택하며 AzCopy 명령에 붙여넣습니다.

지정된 대상 컨테이너가 존재하지 않을 경우 AzCopy는 컨테이너를 만든 후 여기에 파일을 업로드합니다. 원본 경로를 데이터 디렉터리로 업데이트하고 대상 URL의 `data-box-storage-account-name`을 사용자의 Data Box와 연결된 스토리지 계정 이름으로 바꿉니다.

지정된 디렉터리의 콘텐츠를 Blob 저장소로 재귀적으로 업로드하려면 `--recursive`(Linux) 또는 `/S`(Windows) 옵션을 지정합니다. 이러한 옵션 중 하나로 AzCopy를 실행하면 모든 하위 폴더 및 해당 파일도 업로드됩니다.

### <a name="upload-modified-files-to-data-box-blob-storage"></a>Data Box Blob 스토리지에 수정된 파일 업로드

AzCopy를 사용하여 마지막 수정 시간을 기반으로 파일을 업로드합니다. 이 작업을 시도하려면 테스트를 위해 원본 디렉터리에서 새 파일을 수정하거나 만듭니다. 업데이트된 파일 또는 새 파일만 업로드하려면 `--exclude-older`(Linux) 또는 `/XO`(Windows) 매개 변수를 AzCopy 명령에 추가합니다.

대상에 없는 원본 리소스만 복사하려는 경우 AzCopy 명령에 `--exclude-older` 및 `--exclude-newer`(Linux) 또는 `/XO` 및 `/XN`(Windows) 매개 변수를 지정합니다. AzCopy는 해당 타임스탬프에 따라 업데이트된 데이터만 업로드합니다.

#### <a name="linux"></a>Linux
    azcopy \
    --source /mnt/myfolder \
    --destination https://data-box-storage-account-name.blob.device-serial-no.microsoftdatabox.com/container-name/files/ \
    --dest-key <key> \
    --recursive \
    --exclude-older

#### <a name="windows"></a> Windows

    AzCopy /Source:C:\myfolder /Dest:https://data-box-storage-account-name.blob.device-serial-no.microsoftdatabox.com/container-name/files/ /DestKey:<key> /S /XO


## <a name="prepare-to-ship"></a>배송 준비

[!INCLUDE [data-box-prepare-to-ship](../../includes/data-box-prepare-to-ship.md)]

## <a name="next-steps"></a>다음 단계

이 자습서에서는 Azure Data Box 항목에 대해 다음과 같은 내용을 알아보았습니다.

> [!div class="checklist"]
> * *http* 또는 *https*를 통해 Data Box Blob 스토리지에 연결
> * Data Box에 데이터 복사
> * 배송 준비

Data Box를 Microsoft로 다시 배송하는 방법을 알아보려면 다음 자습서로 계속 진행하세요.

> [!div class="nextstepaction"]
> [Microsoft로 Azure Data Box 배송](./data-box-deploy-picked-up.md)