---
title: Linux에서 Azure Files 문제 해결 | Microsoft Docs
description: Linux에서 Azure Files 문제 해결
services: storage
author: jeffpatt24
tags: storage
ms.service: storage
ms.topic: article
ms.date: 10/16/2018
ms.author: jeffpatt
ms.subservice: files
ms.openlocfilehash: 232b4ca2ee4f3137069ed155cc82a5c5e3251420
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807269"
---
# <a name="troubleshoot-azure-files-problems-in-linux"></a>Linux에서 Azure Files 문제 해결

이 문서에서는 Linux 클라이언트에서 연결할 때 Azure Files와 관련하여 발생하는 일반적인 문제를 보여 줍니다. 또한 이러한 문제의 가능한 원인과 해결 방법을 제공합니다. 

이 문서의 문제 해결 단계 외에도 [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-02184089)를 사용하여 Linux 클라이언트에서 올바른 필수 구성 요소를 갖출 수 있도록 해야 합니다. AzFileDiagnostics는 이 문서에 언급된 대부분의 증상을 자동으로 검색합니다. 또한 최적의 성능을 얻기 위해 환경을 설정하는 데 도움이 됩니다. 이 정보는 [Azure 파일 공유 문제 해결사](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares)에서도 발견할 수 있습니다. 문제 해결사는 Azure Files 공유를 연결, 매핑 및 탑재하는 문제를 해결하기 위한 단계를 제공합니다.

## <a name="cannot-connect-to-or-mount-an-azure-file-share"></a>Azure 파일 공유에 연결하거나 탑재할 수 없음

### <a name="cause"></a>원인

이 문제에 대한 일반적인 원인은 다음과 같습니다.

- 호환되지 않는 Linux 배포 클라이언트를 사용하고 있습니다. 다음 Linux 배포를 사용하여 Azure 파일 공유에 연결하는 것이 좋습니다.

|   | SMB 2.1 <br>(동일한 Azure 지역 내에서 VM에 탑재) | SMB 3.0 <br>(온-프레미스 및 지역 간 탑재) |
| --- | :---: | :---: |
| Ubuntu Server | 14.04+ | 16.04+ |
| RHEL | 7+ | 7.5+ |
| CentOS | 7+ |  7.5+ |
| Debian | 8+ |   |
| openSUSE | 13.2+ | 42.3+ |
| SUSE Linux Enterprise Server | 12 | 12 SP3+ |

- cfs-utils(CIFS 유틸리티)는 클라이언트에 설치되지 않았습니다.
- 최소 SMB/CIFS 버전 2.1은 클라이언트에 설치되지 않았습니다.
- SMB 3.0 암호화는 클라이언트에서 지원되지 않습니다. 앞의 표에서 온-프레미스 및 지역 간 지원 탑재는 Linux 배포판의 목록을 제공 암호화를 사용 합니다. 기타 배포에는 커널 4.11 이상 버전이 필요합니다.
- 지원되지 않는 TCP 포트 445를 통해 스토리지 계정에 연결하려고 합니다.
- Azure VM에서 Azure 파일 공유에 연결하려고 하며, VM이 스토리지 계정과 동일한 지역에 있지 않습니다.
- 스토리지 계정에서 [보안 전송 필요]( https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) 설정을 사용하도록 설정한 경우 Azure Files에서는 암호화 기능이 포함된 SMB 3.0을 사용하는 연결만 허용합니다.

### <a name="solution"></a>솔루션

이 문제를 해결하려면 [Linux에서 Azure Files 탑재 오류에 대한 문제 해결 도구](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-02184089)를 사용합니다. 이 도구는 다음과 같은 작업을 수행합니다.

* 클라이언트 실행 환경의 유효성을 검사하는 데 도움이 됩니다.
* Azure Files에 대한 액세스 실패를 일으키는 호환되지 않는 클라이언트 구성을 검색합니다.
* 자체 수정에 대한 규범적인 지침을 제공합니다.
* 진단 추적을 수집합니다.

<a id="mounterror13"></a>
## <a name="mount-error13-permission-denied-when-you-mount-an-azure-file-share"></a>Azure 파일 공유를 탑재하면 "탑재 오류(13): 사용 권한 거부됨" 오류가 발생합니다.

### <a name="cause-1-unencrypted-communication-channel"></a>원인 1: 암호화되지 않은 통신 채널

보안상 이유로, 통신 채널이 암호화되지 않고 Azure 파일 공유가 있는 같은 데이터 센터에서 연결 시도가 이루어지지 않을 경우 Azure 파일 공유에 대한 연결이 차단됩니다. 저장소 계정에서 [보안 전송 필요](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) 설정을 사용하도록 설정한 경우에도 동일한 데이터 센터 내의 암호화되지 않은 연결을 차단할 수 있습니다. 사용자의 클라이언트 OS가 SMB 암호화를 지원하는 경우에만 암호화된 통신 채널이 제공됩니다.

자세한 내용은 [Linux 및 cifs-utils 패키지와 함께 Azure 파일 공유를 탑재하기 위한 필수 조건](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-linux#prerequisites-for-mounting-an-azure-file-share-with-linux-and-the-cifs-utils-package)을 참조하세요. 

### <a name="solution-for-cause-1"></a>원인 1의 해결 방법

1. SMB 암호화를 지원하는 클라이언트에서 연결하거나, Azure 파일 공유에 사용되는 Azure Storage 계정과 동일한 데이터 센터에 있는 가상 머신에서 연결합니다.
2. 클라이언트가 SMB 암호화를 지원하지 않는 경우 스토리지 계정에서 [보안 전송 필요](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) 설정이 사용하지 않도록 설정되었는지 확인합니다.

### <a name="cause-2-virtual-network-or-firewall-rules-are-enabled-on-the-storage-account"></a>원인 2: 가상 네트워크 또는 방화벽 규칙이 스토리지 계정에서 사용하도록 설정됨 

가상 네트워크(VNET) 및 방화벽 규칙이 스토리지 계정에 구성된 경우, 클라이언트 IP 주소 또는 가상 네트워크에 액세스가 허용되지 않았다면 네트워크 트래픽이 거부됩니다.

### <a name="solution-for-cause-2"></a>원인 2의 해결 방법

가상 네트워크 및 방화벽 규칙이 스토리지 계정에 제대로 구성되어 있는지 확인합니다. 가상 네트워크 또는 방화벽 규칙에서 문제가 발생하는지 테스트하려면 일시적으로 스토리지 계정의 설정을 **모든 네트워크에서 액세스 허용**으로 변경합니다. 자세한 내용은 [Azure Storage 방화벽 및 가상 네트워크 구성](https://docs.microsoft.com/azure/storage/common/storage-network-security)을 참조하세요.

<a id="permissiondenied"></a>
## <a name="permission-denied-disk-quota-exceeded-when-you-try-to-open-a-file"></a>파일을 열려고 할 때 “[사용 권한 거부됨] 디스크 할당량이 초과됨”

Linux에서는 다음과 같은 오류 메시지가 수신됩니다.

**\<파일 이름 > [사용 권한 거부 됨] 디스크 할당량이 초과 됨**

### <a name="cause"></a>원인

파일에 허용되는 동시 열린 핸들의 상한에 도달했습니다.

단일 파일에 대한 열린 핸들 할당량은 2000개입니다. 2000개의 열린 핸들이 있는 경우 할당량에 도달했다는 오류 메시지가 표시됩니다.

### <a name="solution"></a>솔루션

일부 핸들을 닫아 동시 열린 핸들 수를 줄인 후 작업을 다시 시도하세요.

파일 공유, 디렉터리 또는 파일에 대 한 열린 핸들을 보려면 사용 합니다 [Get AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/get-azstoragefilehandle) PowerShell cmdlet.  

파일 공유, 디렉터리 또는 파일에 대 한 열린 핸들을 닫으려면 다음을 사용 합니다 [닫기를 AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/close-azstoragefilehandle) PowerShell cmdlet.

> [!Note]  
> Get-AzStorageFileHandle 및 닫기 AzStorageFileHandle cmdlet Az PowerShell 모듈 버전 2.4 이상에 포함 됩니다. 최신 Az PowerShell 모듈을 설치 하려면 [Azure PowerShell 모듈을 설치](https://docs.microsoft.com/powershell/azure/install-az-ps)합니다.

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-files-in-linux"></a>Linux에서 Azure Files와 서로 파일을 복사하는 속도 느림

- 최소 I/O 크기에 대한 특정 요구 사항이 없을 경우 최적 성능을 위해 I/O 크기로 1MiB를 사용하는 것이 좋습니다.
- copy 메서드를 다음과 같이 올바르게 사용합니다.
    - 두 파일 공유 간의 전송에는 [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)를 사용합니다.
    - Cp 또는 dd를 사용 하 여 병렬을 사용 하 여 복사 속도 개선할 수 있습니다, 그리고 스레드 수가 사용 사례 및 워크 로드에 따라 달라 집니다. 다음 예제에서는 6 개를 사용합니다. 
    - cp 예제 (cp를 사용 하 여 파일 시스템의 기본 블록 크기 청크 크기로): `find * -type f | parallel --will-cite -j 6 cp {} /mntpremium/ &`합니다.
    - dd 예제 (이 명령은 명시적으로 청크 크기를 설정 1 MiB): `find * -type f | parallel --will-cite-j 6 dd if={} of=/mnt/share/{} bs=1M`
    - 와 같은 오픈 소스 타사 도구:
        - [GNU 병렬](https://www.gnu.org/software/parallel/)합니다.
        - [Fpart](https://github.com/martymac/fpart) -파일을 정렬 하 고 파티션으로 압축 합니다.
        - [Fpsync](https://github.com/martymac/fpart/blob/master/tools/fpsync) -Fpart 사용 및 복사 도구를 src_dir 데이터 dst_url로 여러 인스턴스를 생성 합니다.
        - [다중](https://github.com/pkolano/mutil) -GNU coreutils를 기반으로 다중 스레드 cp 및 md5sum입니다.
- 모든 쓰기를 확장 쓰기로 설정 하는 대신 파일 크기를 미리 설정 하면 파일 크기를 알고 있는 시나리오에서 복사 속도 개선 합니다. 쓰기 필요를 방지할 수를 확장 하는 경우 사용 하 여 대상 파일 크기를 설정할 수 있습니다 `truncate - size <size><file>` 명령입니다. 그 후 `dd if=<source> of=<target> bs=1M conv=notrunc`명령을 반복 하 여 대상 파일의 크기를 업데이트할 필요 없이 소스 파일을 복사 합니다. 예를 들어, 모든 파일을 복사할 대상 파일 크기를 설정할 수 있습니다 (공유/mnt 공유 아래에 탑재 됩니다 가정):
    - `$ for i in `` find * -type f``; do truncate --size ``stat -c%s $i`` /mnt/share/$i; done`
    - 및 다음-동시에서 쓰기를 확장 하지 않고 파일을 복사 합니다. `$find * -type f | parallel -j6 dd if={} of =/mnt/share/{} bs=1M conv=notrunc`

<a id="error115"></a>
## <a name="mount-error115-operation-now-in-progress-when-you-mount-azure-files-by-using-smb-30"></a>SMB 3.0을 사용하여 Azure Files를 탑재할 때 "탑재 오류(115): 작업이 진행되고 있습니다."가 발생합니다.

### <a name="cause"></a>원인

일부 Linux 배포는 아직 SMB 3.0의 암호화 기능을 지원하지 않습니다. 사용자는 SMB 3.0을 사용하여 Azure Files를 탑재할 경우 기능 누락으로 인해 “115” 오류 메시지를 수신할 수 있습니다. 전체 암호화가 적용된 SMB 3.0은 Ubuntu 16.04 이상을 사용할 때만 지원됩니다.

### <a name="solution"></a>솔루션

Linux용 SMB 3.0의 암호화 기능이 4.11 커널에 도입되었습니다. 이 기능을 사용하면 온-프레미스에서 또는 다른 Azure 지역에서 Azure 파일 공유를 탑재할 수 있습니다. 에 나열 된 Linux 배포판에는이 기능이 포함 되어 있습니다 [최소 권장 버전 해당 탑재 기능 (SMB 버전 2.1 및 SMB 버전 3.0)를 사용 하 여](storage-how-to-use-files-linux.md#minimum-recommended-versions-with-corresponding-mount-capabilities-smb-version-21-vs-smb-version-30)입니다. 기타 배포에는 커널 4.11 이상 버전이 필요합니다.

Linux SMB 클라이언트가 암호화를 지원하지 않는 경우 파일 공유와 같은 데이터 센터의 Azure Linux VM에서 SMB 2.1을 사용하여 Azure Files를 탑재합니다. 스토리지 계정에서 [보안 전송 필요]( https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) 설정을 사용하지 않도록 설정했는지 확인합니다. 

<a id="authorizationfailureportal"></a>
## <a name="error-authorization-failure-when-browsing-to-an-azure-file-share-in-the-portal"></a>오류 "권한 부여 실패"는 포털에서 Azure 파일 공유로 이동 하는 경우

포털에서 Azure 파일 공유를 찾을 때 다음 오류가 표시될 수 있습니다.

권한 부여 실패  
액세스 권한이 없음

### <a name="cause-1-your-user-account-does-not-have-access-to-the-storage-account"></a>원인 1: 사용자 계정에 스토리지 계정에 대한 액세스 권한이 없음

### <a name="solution-for-cause-1"></a>원인 1의 해결 방법

Azure 파일 공유가 있는 스토리지 계정을 찾아 **액세스 제어(IAM)** 를 클릭한 다음, 사용자 계정에 스토리지 계정에 대한 액세스 권한이 있는지 확인합니다. 자세한 내용은 [RBAC(역할 기반 액세스 제어)를 사용하여 스토리지 계정의 보안을 유지하는 방법](https://docs.microsoft.com/azure/storage/common/storage-security-guide#how-to-secure-your-storage-account-with-role-based-access-control-rbac)을 참조하세요.

### <a name="cause-2-virtual-network-or-firewall-rules-are-enabled-on-the-storage-account"></a>원인 2: 가상 네트워크 또는 방화벽 규칙이 스토리지 계정에서 사용하도록 설정됨

### <a name="solution-for-cause-2"></a>원인 2의 해결 방법

가상 네트워크 및 방화벽 규칙이 스토리지 계정에 제대로 구성되어 있는지 확인합니다. 가상 네트워크 또는 방화벽 규칙에서 문제가 발생하는지 테스트하려면 일시적으로 스토리지 계정의 설정을 **모든 네트워크에서 액세스 허용**으로 변경합니다. 자세한 내용은 [Azure Storage 방화벽 및 가상 네트워크 구성](https://docs.microsoft.com/azure/storage/common/storage-network-security)을 참조하세요.

<a id="open-handles"></a>
## <a name="unable-to-delete-a-file-or-directory-in-an-azure-file-share"></a>파일 또는 Azure 파일 공유에 디렉터리를 삭제할 수 없습니다.

### <a name="cause"></a>원인
이 문제는 일반적으로 파일 또는 디렉터리에 열린 핸들이 있으면 발생 합니다. 

### <a name="solution"></a>솔루션

SMB 클라이언트는 열려 있는 모든 핸들 닫았는지 하 고 문제가 계속 발생 하는 경우 다음을 수행 합니다.

- 사용 된 [Get AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/get-azstoragefilehandle) 열린 핸들을 보려면 PowerShell cmdlet.

- 사용 된 [닫기 AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/close-azstoragefilehandle) PowerShell cmdlet를 열린 핸들을 닫습니다. 

> [!Note]  
> Get-AzStorageFileHandle 및 닫기 AzStorageFileHandle cmdlet Az PowerShell 모듈 버전 2.4 이상에 포함 됩니다. 최신 Az PowerShell 모듈을 설치 하려면 [Azure PowerShell 모듈을 설치](https://docs.microsoft.com/powershell/azure/install-az-ps)합니다.

<a id="slowperformance"></a>
## <a name="slow-performance-on-an-azure-file-share-mounted-on-a-linux-vm"></a>Linux VM에 탑재된 Azure 파일 공유의 성능 저하

### <a name="cause-1-caching"></a>원인 1: 캐싱

성능 저하의 한 가지 가능한 원인은 캐싱 비활성화입니다. 캐시 하는 것은 파일을 반복적으로 액세스 하는 경우이 고, 그렇지 수 오버 헤드가 유용할 수 있습니다. 이 사용 하지 않도록 설정 하기 전에 캐시를 사용 하 고 있는지 확인 합니다.

### <a name="solution-for-cause-1"></a>원인 1의 해결 방법

캐싱의 비활성화 여부를 확인하려면 **cache=** 항목을 찾습니다.

**cache=none**은 캐싱이 비활성화되었음을 나타냅니다. 기본 캐싱이나 “엄격한” 캐싱 모드를 활성화하는 명령을 탑재하기 위해 기본 탑재 명령을 사용하거나 명시적으로 **cache=strict** 옵션을 추가하여 공유를 다시 탑재하세요.

일부 시나리오에서는 **serverino** 탑재 옵션으로 **ls** 명령을 유도하여 모든 디렉터리 항목에 대해 stat를 실행할 수 있습니다. 이 동작은 큰 디렉터리를 나열 하는 경우 성능 저하가 발생 합니다. **/etc/fstab** 항목에서 탑재 옵션을 확인할 수 있습니다.

`//azureuser.file.core.windows.net/cifs /cifs cifs vers=2.1,serverino,username=xxx,password=xxx,dir_mode=0777,file_mode=0777`

**sudo mount | grep cifs** 명령을 실행하고 해당 출력을 확인하여 올바른 옵션이 사용되는지 확인할 수도 있습니다. 예제 출력은 다음과 같습니다.

```
//azureuser.file.core.windows.net/cifs on /cifs type cifs (rw,relatime,vers=2.1,sec=ntlmssp,cache=strict,username=xxx,domain=X,uid=0,noforceuid,gid=0,noforcegid,addr=192.168.10.1,file_mode=0777, dir_mode=0777,persistenthandles,nounix,serverino,mapposix,rsize=1048576,wsize=1048576,actimeo=1)
```

**cache=strict** 또는 **serverino** 옵션이 없는 경우 [설명서](../storage-how-to-use-files-linux.md)의 mount 명령을 실행하여 Azure Files를 분리했다가 다시 탑재합니다. 그런 다음 **/etc/fstab** 항목에 올바른 옵션이 있는지 다시 확인합니다.

### <a name="cause-2-throttling"></a>원인 2: 제한

제한이 발생 하 고 요청 큐로 보내는 것 같습니다. 이 활용 하 여 확인할 수 있습니다 [Azure Monitor의 Azure Storage 메트릭](../common/storage-metrics-in-azure-monitor.md)합니다.

### <a name="solution-for-cause-2"></a>원인 2의 해결 방법

내 앱을 확인 합니다.는 [Azure Files 확장 대상](storage-files-scale-targets.md#azure-files-scale-targets)합니다.

<a id="timestampslost"></a>
## <a name="time-stamps-were-lost-in-copying-files-from-windows-to-linux"></a>Windows에서 Linux로 파일을 복사할 때 타임 스탬프 손실됨

Linux/Unix 플랫폼에서 파일 1 및 파일 2의 소유자가 다른 경우 **cp-p** 명령이 실패합니다.

### <a name="cause"></a>원인

COPYFILE에서 force 플래그 **f**로 인해 Unix에서 **cp -p -f**가 실행됩니다. 이 명령은 또한 소유하지 않은 파일의 타임 스탬프를 유지하지 않습니다.

### <a name="workaround"></a>해결 방법

파일 복사를 위해 저장소 계정 사용자를 사용합니다.

- `Useadd : [storage account name]`
- `Passwd [storage account name]`
- `Su [storage account name]`
- `Cp -p filename.txt /share`

## <a name="ls-cannot-access-ltpathgt-inputoutput-error"></a>ls: '&lt;path&gt;에 액세스할 수 없음': 입/출력 오류

ls 명령을 사용하여 Azure 파일 공유에서 파일을 나열하려는 경우 파일을 나열할 때 ls 명령이 중지됩니다. 다음과 같은 오류가 표시됩니다.

**ls: '&lt;path&gt;에 액세스할 수 없음': 입/출력 오류**


### <a name="solution"></a>솔루션
이 문제를 수정하는 다음 버전으로 Linux 커널을 업그레이드합니다.

- 4.4.87+
- 4.9.48+
- 4.12.11+
- 4\.13 이상 모든 버전

## <a name="cannot-create-symbolic-links---ln-failed-to-create-symbolic-link-t-operation-not-supported"></a>심볼 링크를 만들 수 없음 - ln: 심볼 링크 't'를 만들 수 없음: 지원되지 않는 작업입니다.

### <a name="cause"></a>원인
기본적으로 CIFS를 사용하여 Linux에 Azure 파일 공유를 탑재하면 symlink(심볼 링크)를 지원할 수 없습니다. 이와 같은 오류가 발생합니다.
```
ln -s linked -n t
ln: failed to create symbolic link 't': Operation not supported
```
### <a name="solution"></a>솔루션
Linux CIFS 클라이언트는 SMB 2 또는 3 프로토콜을 통한 Windows 스타일의 심볼 링크 생성을 지원하지 않습니다. 현재 Linux 클라이언트는 만들기 및 따르기 작업 모두에 대해 [Mishall+French symlinks](https://wiki.samba.org/index.php/UNIX_Extensions#Minshall.2BFrench_symlinks)라는 다른 스타일의 심볼 링크를 지원합니다. 심볼 링크가 필요한 고객은 "mfsymlinks" 탑재 옵션을 사용할 수 있습니다. Mac이 사용하는 형식이기도 하므로 "mfsymlinks"를 사용하는 것이 좋습니다.

symlink를 사용하려면 CIFS 탑재 명령 끝에 다음을 추가합니다.

```
,mfsymlinks
```

그러면 명령이 다음과 같은 모양이 됩니다.

```
sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> <mount-point> -o vers=<smb-version>,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino,mfsymlinks
```

[wiki](https://wiki.samba.org/index.php/UNIX_Extensions#Storing_symlinks_on_Windows_servers)에서 제안된 대로 symlink를 만들 수 있습니다.

[!INCLUDE [storage-files-condition-headers](../../../includes/storage-files-condition-headers.md)]

<a id="error112"></a>
## <a name="mount-error112-host-is-down-because-of-a-reconnection-time-out"></a>다시 연결 시간 제한으로 인해 "탑재 오류(112): 호스트가 중단됨"이 발생합니다.

Linux 클라이언트에서 클라이언트가 장시간 유휴 상태일 경우 "112" 탑재 오류가 발생합니다. 오랫동안 유휴 상태일 경우 클라이언트 연결이 끊어지고 연결 시간이 초과됩니다.  

### <a name="cause"></a>원인

다음과 같은 이유로 연결이 유휴 상태일 수 있습니다.

-   기본 "소프트" 탑재 옵션을 사용하는 경우 서버에 TCP 연결을 다시 설정하지 않는 네트워크 통신 오류입니다.
-   이전 커널에 존재하지 않는 최근 재연결 수정

### <a name="solution"></a>솔루션

Linux 커널의 이러한 재연결 문제는 현재 다음 변경의 일부로 수정되었습니다.

- [소켓 재연결 후에 오랫동안 smb3 세션 재연결이 지연되지 않도록 재연결 수정](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93)
- [소켓 재연결 직후 에코 서비스 호출](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b8c600120fc87d53642476f48c8055b38d6e14c7)
- [CIFS: 재연결 동안 가능한 메모리 손상 수정](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=53e0e11efe9289535b060a51d4cf37c25e0d0f2b)
- [CIFS: 재연결 동안 가능한 뮤텍스 이중 잠금 해결(커널 v4.9 이상)](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=96a988ffeb90dba33a71c3826086fe67c897a183)

그러나 이 변경 내용이 모든 Linux 배포판에 아직 이식되지 않은 것일 수 있습니다. 이 수정 프로그램 및 기타 재연결에서 찾을 수 있습니다 합니다 [최소 권장 버전 해당 탑재 기능 (SMB 버전 2.1 및 SMB 버전 3.0)를 사용 하 여](storage-how-to-use-files-linux.md#minimum-recommended-versions-with-corresponding-mount-capabilities-smb-version-21-vs-smb-version-30) 섹션을 [Linux사용하여AzureFiles를사용하여](storage-how-to-use-files-linux.md)문서. 이러한 권장된 커널 버전 중 하나로 업그레이드하여 이 수정을 가져올 수 있습니다.

### <a name="workaround"></a>해결 방법

하드 탑재를 지정하여 이 문제를 해결할 수 있습니다. 하드 탑재를 사용하면 클라이언트는 연결이 설정될 때까지 또는 연결이 명시적으로 중단될 때까지 대기해야 합니다. 하드 탑재는 네트워크 시간 제한으로 인한 오류를 방지하는 데 사용할 수 있습니다. 그러나 이 해결 방법은 무한 대기를 일으킬 수 있습니다. 필요에 따라 연결을 중지할 준비가 되어 있어야 합니다.

최신 커널 버전으로 업그레이드할 수 없는 경우 매 30초 이하 간격으로 쓰는 Azure 파일 공유에 파일을 보관하여 이 문제를 해결할 수 있습니다. 이 작업은 만든 또는 수정된 날짜를 파일에 다시 쓰는 등의 쓰기 작업이어야 합니다. 그렇지 않으면 캐시된 결과를 얻을 수 있고 작업이 재연결을 트리거하지 않을 수 있습니다.

## <a name="need-help-contact-support"></a>도움 필요 시 지원에 문의

도움이 필요한 경우 [지원에 문의](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)하여 문제를 신속하게 해결하세요.
