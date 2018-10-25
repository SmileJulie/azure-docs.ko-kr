---
title: Azure 파일 동기화 클라우드 계층화 이해 | Microsoft Docs
description: Azure 파일 동기화의 기능인 클라우드 계층화에 대해 알아봅니다.
services: storage
author: sikoo
ms.service: storage
ms.topic: article
ms.date: 09/21/2018
ms.author: sikoo
ms.component: files
ms.openlocfilehash: a11e0a1c20617f3065d5b3f8cf59d67cf7aa0179
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48242025"
---
# <a name="cloud-tiering-overview"></a>클라우드 계층화 개요
Azure 파일 동기화의 선택적 기능인 클라우드 계층화를 사용하는 경우 액세스 빈도가 높은 파일은 서버에 로컬로 캐시되고 그 외의 모든 파일은 정책 설정에 따라 Azure Files에서 계층화됩니다. 파일을 계층화할 경우 Azure 파일 동기화 파일 시스템 필터(StorageSync.sys)는 파일을 포인터 또는 재분석 지점으로 로컬로 대체합니다. 재분석 지점은 Azure Files의 파일에 대한 URL을 나타냅니다. 계층화된 파일의 경우 NTFS에서 FILE_ATTRIBUTE_RECALL_ON_DATA_ACCESS 특성과 “offline” 특성이 모두 설정되므로 타사 응용 프로그램이 계층화된 파일을 안전하게 식별할 수 있습니다.
 
사용자가 계층화된 파일을 열면 Azure 파일 동기화가 Azure Files에서 파일 데이터를 원활하게 회수하므로, 사용자는 파일이 실제로는 Azure에 저장되어 있음을 몰라도 됩니다. 
 
 > [!Important]  
    > 중요: 클라우드 계층화는 Windows 시스템 볼륨의 서버 엔드포인트에서 지원되지 않으며 크기가 64KiB보다 큰 파일만 Azure Files로 계층화할 수 있습니다.
    
Azure 파일 동기화는 64KiB보다 작은 계층화 파일을 지원하지 않습니다. 이렇게 작은 파일을 계층화하고 회수하는 경우 절약할 수 있는 공간보다 성능 오버헤드가 너무 크기 때문입니다.

## <a name="cloud-tiering-faq"></a>클라우드 계층화 FAQ

<a id="afs-cloud-tiering"></a>
### <a name="how-does-cloud-tiering-work"></a>클라우드 계층화는 어떤 방식으로 작동하나요?
Azure 파일 동기화 시스템 필터가 각 서버 엔드포인트에 네임스페이스의 “열 지도”를 작성합니다. 이 필터는 시간별 액세스(읽기 및 쓰기 작업)를 모니터링한 다음 액세스 빈도와 최신 상태에 따라 모든 파일에 열 점수를 할당합니다. 액세스 빈도가 높으며 최근 열었던 파일은 핫 파일로 간주되는 반면 거의 액세스하지 않으며 장시간 액세스하지 않았던 파일은 쿨 파일로 간주됩니다. 서버의 파일 볼륨이 설정된 사용 가능한 볼륨 공간 임계값을 초과하면 사용 가능한 공간 백분율이 충족될 때까지 쿨 점수가 가장 높은 파일이 Azure Files로 계층화됩니다.

<a id="afs-volume-free-space"></a>
### <a name="how-does-the-volume-free-space-tiering-policy-work"></a>사용 가능한 볼륨 공간 계층화 정책은 어떤 방식으로 작동하나요?
사용 가능한 볼륨 공간은 서버 엔드포인트가 있는 볼륨에서 유지하려는 사용 가능한 공간의 양입니다. 예를 들어 서버 엔드포인트가 하나인 볼륨에서 사용 가능한 볼륨 공간을 20%로 설정하면 가장 최근에 액세스한 파일이 볼륨 공간의 최대 80%를 차지하며, 이 공간에 포함되지 않은 나머지 파일이 Azure로 계층화됩니다. 사용 가능한 볼륨 공간은 개별 디렉터리 또는 동기화 그룹 수준이 아닌 볼륨 수준에서 적용됩니다. 

<a id="volume-free-space-fastdr"></a>
### <a name="how-does-the-volume-free-space-tiering-policy-work-with-regards-to-new-server-endpoints"></a>새 서버 엔드포인트에서 볼륨 사용 가능한 공간 계층화 정책은 어떤 방식으로 작동하나요?
서버 엔드포인트를 새로 프로비전하여 Azure 파일 공유에 연결하면 해당 서버는 먼저 네임스페이스를 풀다운한 다음 사용 가능한 볼륨 임계값에 도달할 때까지 실제 파일을 풀다운합니다. 이 프로세스를 빠른 재해 복구 또는 빠른 네임스페이스 복원이라고도 합니다.

<a id="afs-effective-vfs"></a>
### <a name="how-is-volume-free-space-interpreted-when-i-have-multiple-server-endpoints-on-a-volume"></a>볼륨에 여러 서버 엔드포인트가 있는 경우 사용 가능한 볼륨 공간은 어떻게 해석되나요?
볼륨에 서버 엔드포인트가 둘 이상 있으면 효과적인 사용 가능한 볼륨 공간 임계값은 해당 볼륨의 서버 엔드포인트에서 지정된 사용 가능한 최대 볼륨 공간입니다. 파일은 속해 있는 서버 엔드포인트와 관계없이 사용 패턴에 따라 계층화됩니다. 예를 들어 Endpoint1 및 Endpoint2라는 두 개의 서버 엔드포인트가 볼륨에 있고, Endpoint1의 사용 가능한 볼륨 공간 임계값은 25%이고, Endpoint2의 사용 가능한 볼륨 공간 임계값은 50%인 경우, 두 서버 엔드포인트의 사용 가능한 볼륨 공간 임계값은 50%가 됩니다. 

<a id="volume-free-space-guidelines"></a>
### <a name="how-do-i-determine-the-appropriate-amount-of-volume-free-space"></a>사용 가능한 볼륨의 적절한 양은 어떻게 확인할 수 있나요?
로컬에 보관해야 하는 데이터의 양은 대역폭, 데이터 집합 액세스 패턴, 예산 등의 몇 가지 요인에 따라 결정됩니다. 저대역폭 연결을 사용하는 경우 사용자의 지연 시간을 최소화하기 위해 더 많은 데이터를 로컬에 보관해야 할 수 있습니다. 그렇지 않은 경우에는 지정된 기간 동안의 변동률에 따라 보관할 데이터의 양을 결정할 수 있습니다. 예를 들어 매월 1TB 데이터 집합 중 약 10%를 변경하거나 실제로 액세스하는 경우에는 파일을 자주 회수하지 않아도 되도록 100GB의 데이터를 로컬에 보관할 수 있습니다. 볼륨 크기가 2TB라면 5%(100GB)를 로컬에 보관할 수 있습니다. 이 경우 나머지 95%가 사용 가능한 볼륨 공간 백분율입니다. 그러나 변동률이 높은 기간을 고려하여 버퍼를 추가하는 것이 좋습니다. 즉, 일단은 사용 가능한 볼륨 공간 백분율을 낮게 설정했다가 나중에 필요하면 조정하는 것이 좋습니다. 

더 많은 데이터를 로컬에 보관하면 Azure에서 회수하는 파일 수가 감소하므로 송신 비용이 줄어듭니다. 하지만 그와 동시에 더 큰 온-프레미스 저장소를 유지 관리해야 하므로 저장소 비용이 발생합니다. Azure 파일 동기화 인스턴스를 배포한 후에는 저장소 계정의 송신량을 확인해 사용 가능한 볼륨 공간 설정이 사용량에 적합한지를 대략적으로 측정할 수 있습니다. 저장소 계정에 Azure 파일 동기화 클라우드 엔드포인트(동기화 공유)만 포함된다고 가정할 때 송신량이 높으면 클라우드에서 많은 파일을 회수한다는 의미이므로 로컬 캐시를 늘려야 합니다.

<a id="is-my-file-tiered"></a>
### <a name="how-can-i-tell-whether-a-file-has-been-tiered"></a>파일이 계층화되었는지 여부는 어떻게 알 수 있나요?
파일이 Azure 파일 공유로 계층화되었는지 여부를 확인하는 몇 가지 방법이 있습니다.
    
   *  **파일의 파일 특성을 확인합니다.**
     파일을 마우스 오른쪽 단추로 클릭하고 **세부 정보**로 이동한 다음 아래쪽의 **특성** 속성으로 스크롤합니다. 계층화된 파일에는 다음과 같은 특성 집합이 적용됩니다.     
        
        | 특성 문자 | 특성 | 정의 |
        |:----------------:|-----------|------------|
        | A | 보관 | 파일을 백업 소프트웨어로 백업해야 함을 나타냅니다. 이 특성은 파일이 계층화되는지 또는 디스크에 완전히 저장되는지에 관계없이 항상 설정됩니다. |
        | P | 스파스 파일 | 파일이 스파스 파일인지를 나타냅니다. 스파스 파일은 디스크 스트림의 파일이 대부분 비어 있을 때 효율적으로 사용하기 위해 NTFS가 제공하는 특수한 형식의 파일입니다. Azure 파일 동기화는 파일이 완전히 계층화되거나 부분적으로 회수되기 때문에 스파스 파일을 사용합니다. 완전히 계층화된 파일에서 파일 스트림은 클라우드에 저장됩니다. 부분적으로 회수된 파일에서 파일의 해당 부분은 이미 디스크에 있습니다. 파일이 디스크에 완전히 회수되면 Azure 파일 동기화는 스파스 파일에서 일반 파일로 변환합니다. |
        | L | 재분석 지점 | 파일에 재분석 지점이 있음을 나타냅니다. 재분석 지점은 파일 시스템 필터에서 사용되는 특별한 포인터입니다. Azure 파일 동기화는 재분석 지점을 사용하여 Azure 파일 동기화 파일 시스템 필터(StorageSync.sys)에 파일이 저장되는 클라우드 위치를 정의합니다. 원활한 액세스를 지원합니다. 사용자는 Azure 파일 동기화가 사용되는지 또는 Azure 파일 공유에 있는 파일에 액세스하는 방법을 알 필요가 없습니다. 파일을 완전하게 회수되면 Azure 파일 동기화는 파일에서 재분석 지점을 제거합니다. |
        | O | 오프라인 | 파일 콘텐츠 일부 또는 전체가 디스크에 저장되지 않음을 나타냅니다. 파일을 완전하게 회수되면 Azure 파일 동기화는 이 특성을 제거합니다. |

        ![세부 정보 탭이 선택되어 있는 파일에 대한 속성 대화 상자](media/storage-files-faq/azure-file-sync-file-attributes.png)
        
        **특성** 필드를 파일 탐색기의 표 화면에 추가하여 폴더의 모든 파일에 대한 특성을 볼 수 있습니다. 이렇게 하려면 기존 열을 마우스 오른쪽 단추로 클릭하고(예: **크기**) **더 보기**를 선택한 다음 드롭다운 목록에서 **특성**을 선택합니다.
        
   * **`fsutil`을 사용하여 파일에 대한 재분석 지점을 확인합니다.**
       이전 옵션에서 설명된 것과 같이 계층화된 파일은 항상 재분석 지점 집합을 가집니다. 재분석 포인터는 Azure 파일 동기화 파일 시스템 필터(StorageSync.sys)에 대한 특별한 포인터입니다. 파일에 재분석 지점이 있는지 확인하려면 관리자 권한 명령 프롬프트 또는 PowerShell 창에서 `fsutil` 유틸리티를 실행합니다.
    
        ```PowerShell
        fsutil reparsepoint query <your-file-name>
        ```

        파일에 재분석 지점이 있으면 **재분석 태그 값: 0x8000001e**가 표시됩니다. 이 16진수 값은 Azure 파일 동기화에서 소유하는 재분석 지점 값입니다. 또한 출력에는 Azure 파일 공유의 파일 경로를 나타내는 재분석 데이터도 포함됩니다.

        > [!WARNING]  
        > `fsutil reparsepoint` 유틸리티 명령에는 재분석 지점을 삭제하는 기능도 있습니다. Azure 파일 동기화 엔지니어링 팀에서 요청하는 경우가 아니면 이 명령을 실행하지 마십시오. 이 명령을 실행하면 데이터가 손실될 수 있습니다. 

<a id="afs-recall-file"></a>

### <a name="a-file-i-want-to-use-has-been-tiered-how-can-i-recall-the-file-to-disk-to-use-it-locally"></a>사용하려는 파일이 계층화되어 있습니다. 이 파일을 로컬에서 사용하기 위해 디스크로 회수하려면 어떻게 해야 하나요?
디스크로 파일을 회수하는 가장 쉬운 방법은 파일을 여는 것입니다. Azure 파일 동기화 파일 시스템 필터(StorageSync.sys)는 사용자의 별다른 작업 없이도 Azure 파일 공유에서 파일을 원활하게 다운로드합니다. 부분적으로 읽을 수 있는 파일 형식(예: 멀티미디어 또는 zip 파일)의 경우 파일을 열면 전체 파일이 다운로드되지 않습니다.

PowerShell을 사용하여 파일을 강제로 회수할 수도 있습니다. 이 옵션은 많은 파일을 한 번에 회수하려는 경우에(예: 폴더의 모든 파일) 유용할 수 있습니다. Azure 파일 동기화가 설치되어 있는 서버 노드로 PowerShell 세션을 열고 다음 PowerShell 명령을 실행합니다.
    
    ```PowerShell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Invoke-StorageSyncFileRecall -Path <file-or-directory-to-be-recalled>
    ```

<a id="sizeondisk-versus-size"></a>
### <a name="why-doesnt-the-size-on-disk-property-for-a-file-match-the-size-property-after-using-azure-file-sync"></a>Azure 파일 동기화를 사용한 후에 파일의 *디스크 할당 크기* 속성이 *Size* 속성과 일치하지 않는 이유는 무엇인가요? 
Windows 파일 탐색기는 파일 크기를 나타내기 위해 **크기** 및 **디스크 크기**의 두 속성을 표시합니다. 이러한 속성은 의미가 약간 다릅니다. **크기**는 파일의 전체 크기를 나타냅니다. **디스크 크기**는 디스크에 저장된 파일 스트림의 크기를 나타냅니다. 이러한 속성에 대한 값은 압축, 데이터 중복 제거의 사용 또는 Azure 파일 동기화를 사용한 클라우드 계층화 같은 다양한 이유로 차이를 보일 수 있습니다. 파일이 Azure 파일 공유로 계층화되면 파일 스트림이 디스크가 아닌 Azure 파일 공유에 저장되므로 디스크 크기는 0입니다. 파일을 부분적으로 계층화할 수 있습니다(또는 부분적으로 회수). 부분적으로 계층화된 파일에서 파일 일부가 디스크에 있습니다. 멀티미디어 플레이어 또는 압축 유틸리티와 같은 응용 프로그램에 의해 파일이 부분적으로 읽힐 때 이러한 현상이 발생할 수 있습니다. 

<a id="afs-force-tiering"></a>
### <a name="how-do-i-force-a-file-or-directory-to-be-tiered"></a>파일 또는 디렉터리를 강제로 계층화하려면 어떻게 해야 하나요?
클라우드 계층화 기능이 활성화된 경우 클라우드 계층화는 클라우드 엔드포인트에 지정된 사용 가능한 볼륨 공간 비율에 맞게 마지막 액세스 및 수정 시간을 기준으로 파일을 자동으로 계층화합니다. 그러나 경우에 따라 파일을 강제로 계층화하려는 경우도 있을 수 있습니다. 장시간 다시 사용하지 않으려는 큰 파일을 저장하고 다른 파일 및 폴더에 사용하기 위해 볼륨 공간을 확보하려는 경우에 이러한 강제 계층화가 유용할 수 있습니다. 다음 PowerShell 명령을 사용하여 강제로 계층화할 수 있습니다.

    ```PowerShell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Invoke-StorageSyncCloudTiering -Path <file-or-directory-to-be-tiered>
    ```

## <a name="next-steps"></a>다음 단계
* [Azure 파일 동기화 배포에 대한 계획](storage-sync-files-planning.md)