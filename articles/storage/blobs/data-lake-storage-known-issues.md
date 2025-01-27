---
title: Azure Data Lake Storage Gen2에서 알려진 문제 | Microsoft Docs
description: Azure Data Lake Storage Gen2에서 제한 사항 및 알려진 문제에 대해 알아보기
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 07/18/2019
ms.author: normesta
ms.openlocfilehash: 4a8c69dc06b2de08016ae282413402061cdb89d1
ms.sourcegitcommit: da0a8676b3c5283fddcd94cdd9044c3b99815046
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68314400"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage Gen2에서 알려진 문제

이 문서에는 아직 지원 되지 않거나 계층적 네임 스페이스 (Azure Data Lake Storage Gen2)를 포함 하는 저장소 계정 에서만 부분적으로 지원 되는 기능 및 도구가 나열 되어 있습니다.

<a id="blob-apis-disabled" />

## <a name="blob-storage-apis"></a>Blob Storage API

Blob Storage Api가 Azure Data Lake Gen2 Api와 상호 운용할 수 없기 때문에 발생할 수 있는 기능 운용성 문제를 방지 하기 위해 Blob 저장소 Api를 사용할 수 없습니다.

> [!NOTE]
> Data Lake Storage에 대 한 다중 프로토콜 액세스의 공개 미리 보기에 등록 하는 경우 blob Api 및 Data Lake Storage Gen2 Api는 동일한 데이터에 대해 작동할 수 있습니다. 자세한 내용은 [Data Lake Storage에서 다중 프로토콜 액세스](data-lake-storage-multi-protocol-access.md)를 참조 하세요.

### <a name="what-to-do-with-existing-tools-applications-and-services"></a>기존 도구, 응용 프로그램 및 서비스와 관련 하 여 수행할 작업

이러한 방법 중 하나를 사용 하 여 Blob Api를 사용 하 고이를 사용 하 여 계정에 업로드 하는 모든 콘텐츠를 사용 하려는 경우에는 두 가지 옵션이 있습니다.

* **옵션 1**: Blob Api Azure Data Lake Gen2 Api와 상호 운용할 수 있을 때까지 Blob storage 계정에서 계층적 네임 스페이스를 사용 하지 마세요. 계층적 네임 스페이스 없이 저장소 계정을 사용 하면 디렉터리 및 파일 시스템 액세스 제어 목록과 같은 Data Lake Storage Gen2 특정 기능에 액세스할 수 없습니다.

* **옵션 2**: [Data Lake Storage에서 다중 프로토콜 액세스](data-lake-storage-multi-protocol-access.md)의 공개 미리 보기에 등록 합니다. Blob Api를 호출 하는 도구 및 응용 프로그램과 진단 로그와 같은 Blob 저장소 기능은 계층 구조 네임 스페이스를 가진 계정에서 작동할 수 있습니다.

### <a name="what-to-do-if-you-used-blob-apis-to-load-data-before-blob-apis-were-disabled"></a>Blob api를 사용 하지 않도록 설정 하기 전에 Blob Api를 사용 하 여 데이터를 로드 한 경우 수행할 작업

사용하지 않도록 설정하기 전에 이러한 API를 사용하여 데이터를 로드했고 해당 데이터에 액세스하기 위한 프로덕션 요구 사항이 있는 경우에는 다음 정보를 사용하여 Microsoft 지원에 문의하세요.

> [!div class="checklist"]
> * 구독 ID (이름이 아닌 GUID)입니다.
> * 저장소 계정 이름입니다.
> * 프로덕션 환경에서 적극적으로 영향을 받는지 여부와 저장소 계정에 대 한 것입니다.
> * 프로덕션 환경에서 많은 영향을 받지 않더라도 이 데이터를 다른 스토리지 계정에 복사해야 하는지 여부를 알려 주세요. 그렇다면 이유는 무엇인가요?

이러한 상황에서는 이 데이터를 계층 구조 네임스페이스 기능이 사용되지 않는 스토리지 계정에 복사할 수 있도록 제한된 기간 동안 Blob API에 대한 액세스를 복원할 수 있습니다.

### <a name="issues-and-limitations-with-using-blob-apis-on-accounts-that-have-a-hierarchical-namespace"></a>계층 네임 스페이스가 있는 계정에서 Blob Api를 사용 하는 경우의 문제 및 제한 사항

Data Lake Storage에 대 한 다중 프로토콜 액세스의 공개 미리 보기에 등록 하는 경우 blob Api 및 Data Lake Storage Gen2 Api는 동일한 데이터에 대해 작동할 수 있습니다.

이 섹션에서는 동일한 데이터에 대해 작동 하는 blob Api 및 Data Lake Storage Gen2 Api를 사용 하는 경우의 문제 및 제한 사항에 대해 설명 합니다.

이러한 Blob REST Api는 지원 되지 않습니다.

* [Blob 배치 (페이지)](https://docs.microsoft.com/rest/api/storageservices/put-blob)
* [페이지 배치](https://docs.microsoft.com/rest/api/storageservices/put-page)
* [페이지 범위 가져오기](https://docs.microsoft.com/rest/api/storageservices/get-page-ranges)
* [Blob 증분 복사](https://docs.microsoft.com/rest/api/storageservices/incremental-copy-blob)
* [URL에서 페이지 배치](https://docs.microsoft.com/rest/api/storageservices/put-page-from-url)
* [Blob 배치 (추가)](https://docs.microsoft.com/rest/api/storageservices/put-blob)
* [추가 블록](https://docs.microsoft.com/rest/api/storageservices/append-block)
* [URL의 블록 추가](https://docs.microsoft.com/rest/api/storageservices/append-block-from-url)

* Blob Api와 Data Lake Storage Api를 모두 사용 하 여 동일한 파일 인스턴스에 쓸 수는 없습니다.

* Data Lake Storage Gen2 Api를 사용 하 여 파일에 쓰는 경우 해당 파일의 블록이 [Get Block List](https://docs.microsoft.comrest/api/storageservices/get-block-list) blob API에 대 한 호출에 표시 되지 않습니다.

* Data Lake Storage Gen2 Api 또는 Blob Api를 사용 하 여 파일을 덮어쓸 수 있습니다. 이는 파일 속성에 영향을 주지 않습니다.

* 구분 기호를 지정 하지 않고 [Blob 나열](https://docs.microsoft.com/rest/api/storageservices/list-blobs) 작업을 사용 하는 경우 결과에는 디렉터리와 blob이 모두 포함 됩니다.

  구분 기호를 사용 하도록 선택 하는 경우 슬래시 (`/`)만 사용 합니다. 유일 하 게 지원 되는 구분 기호입니다.

* [Blob 삭제](https://docs.microsoft.com/rest/api/storageservices/delete-blob) API를 사용 하 여 디렉터리를 삭제 하는 경우 해당 디렉터리는 비어 있는 경우에만 삭제 됩니다.

  즉, Blob API delete 디렉터리를 재귀적으로 사용할 수 없습니다.

## <a name="issues-with-unmanaged-virtual-machine-vm-disks"></a>관리 되지 않는 VM (가상 컴퓨터) 디스크의 문제

계층 네임 스페이스가 있는 계정에서는 관리 되지 않는 VM 디스크가 지원 되지 않습니다. 저장소 계정에서 계층적 네임 스페이스를 사용 하도록 설정 하려면 계층 구조 네임 스페이스 기능이 사용 하도록 설정 되지 않은 저장소 계정에 관리 되지 않는 VM 디스크를 저장 합니다.


## <a name="support-for-other-blob-storage-features"></a>다른 Blob storage 기능 지원

다음 표에는 아직 지원 되지 않거나 계층적 네임 스페이스 (Azure Data Lake Storage Gen2)를 포함 하는 저장소 계정 에서만 부분적으로 지원 되는 기타 모든 기능 및 도구가 나열 되어 있습니다.

| 기능/도구    | 자세한 정보    |
|--------|-----------|
| **Data Lake Storage Gen2 저장소 계정에 대 한 Api** | 부분적으로 지원됨 <br><br>Data Lake Storage에 대 한 다중 프로토콜 액세스는 현재 공개 미리 보기로 제공 됩니다. 이 미리 보기를 통해 .NET, Java, Python Sdk에서 계층 네임 스페이스가 있는 계정으로 Blob Api를 사용할 수 있습니다.  Sdk는 디렉터리를 조작 하거나 Acl (액세스 제어 목록)을 설정할 수 있는 Api를 아직 포함 하지 않습니다. 이러한 기능을 수행 하기 위해 Data Lake Storage Gen2 **REST** api를 사용할 수 있습니다. |
| **AZCopy** | 버전별 지원 <br><br>최신 버전의 AzCopy ([AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json))만 사용 합니다. AzCopy v 8.1과 같은 이전 버전의 AzCopy는 지원 되지 않습니다.|
| **Azure Blob 저장소 수명 주기 관리 정책** | Data Lake Storage 미리 보기 [에서 멀티 프로토콜 액세스](data-lake-storage-multi-protocol-access.md) 를 등록 하는 경우에만 지원 됩니다. 쿨 및 archive 액세스 계층은 미리 보기 에서만 지원 됩니다. Blob 스냅숏의 삭제는 아직 지원 되지 않습니다. |
| **CDN (Azure Content Delivery Network)** | 아직 지원 되지 않음|
| **Azure 검색** |아직 지원 되지 않음|
| **Azure Storage Explorer** | 버전별 지원 <br><br>버전 `1.6.0` 이상만 사용 합니다. <br>버전 `1.6.0` 은 [무료 다운로드](https://azure.microsoft.com/features/storage-explorer/)로 제공 됩니다.|
| **Blob 컨테이너 Acl** |아직 지원 되지 않음|
| **Blobfuse** |아직 지원 되지 않음|
| **사용자 지정 도메인** |아직 지원 되지 않음|
| **파일 시스템 탐색기** | 제한 된 지원 |
| **변경할 수 없는 저장소** |아직 지원 되지 않음 <br><br>변경할 수 없는 저장소는 데이터를 [웜 (한 번 쓰기, 많은 읽기)](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutable-storage) 상태에 저장 하는 기능을 제공 합니다.|
| **개체 수준 계층** |쿨 및 archive 계층은 Data Lake Storage 미리 보기 [에서 다중 프로토콜 액세스](data-lake-storage-multi-protocol-access.md) 를 등록 하는 경우에만 지원 됩니다. <br><br> 다른 모든 액세스 계층은 아직 지원 되지 않습니다.|
| **Powershell 및 CLI 지원** | 제한 된 기능 <br><br>계정 만들기와 같은 관리 작업이 지원 됩니다. 파일 업로드 및 다운로드 등의 데이터 평면 작업은 [Data Lake Storage에 대 한 다중 프로토콜 액세스](data-lake-storage-multi-protocol-access.md)의 일부로 공개 미리 보기로 제공 됩니다. 디렉터리 작업 및 Acl (액세스 제어 목록) 설정은 아직 지원 되지 않습니다. |
| **정적 웹 사이트** |아직 지원 되지 않음 <br><br>특히 [정적 웹 사이트](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website)에 파일을 제공 하는 기능입니다.|
| **타사 응용 프로그램** | 제한 된 지원 <br><br>REST Api를 사용 하 여 작동 하는 타사 응용 프로그램은 Data Lake Storage Gen2와 함께 사용 하는 경우 계속 작동 합니다. <br>[Data Lake Storage에 대 한 다중 프로토콜 액세스](data-lake-storage-multi-protocol-access.md)의 공개 미리 보기에 등록 하는 경우 Blob api를 호출 하는 응용 프로그램의 작동 가능성이 높습니다. 
| **버전 관리 기능** |아직 지원 되지 않음 <br><br>여기에는 [스냅숏](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob) 및 [일시 삭제가](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete)포함 됩니다.|


