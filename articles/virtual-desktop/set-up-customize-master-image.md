---
title: 준비 및 Azure 마스터 VHD 이미지를 사용자 지정
description: 준비 하 고 사용자 지정 Windows 가상 데스크톱 미리 보기 마스터 이미지를 Azure에 업로드 하는 방법.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: helohr
ms.openlocfilehash: 2413a380adf32755452482d2b68d2055f7db666d
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67620432"
---
# <a name="prepare-and-customize-a-master-vhd-image"></a>마스터 VHD 이미지 준비 및 사용자 지정

이 문서에서는 가상 머신 (Vm) 만들기에 소프트웨어를 설치 하는 방법을 포함 하 여 Azure에 업로드할 마스터 가상 하드 디스크 (VHD) 이미지를 준비 하는 방법을 알려줍니다. 이러한 지침은 조직의 기존 프로세스를 사용 하 여 사용할 수 있는 Windows 가상 데스크톱 미리 보기 전용 구성입니다.

## <a name="create-a-vm"></a>VM 만들기

Windows 10 Enterprise 다중 세션 Azure 이미지 갤러리에서 제공 됩니다. 이 이미지를 사용자 지정 하기 위한 두 가지 옵션이 있습니다.

첫 번째 옵션은 가상 머신 (VM)를 프로 비전 할 Azure에서의 지침에 따라 [관리 되는 이미지에서 VM을 만듭니다](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-generalized-managed), 및 다음 건너 뛰 세요 [소프트웨어 준비 및 설치](set-up-customize-master-image.md#software-preparation-and-installation)합니다.

두 번째 옵션은 이미지를 다운로드, Hyper-v VM을 프로 비전 및 다음 섹션에서 다룰 필요에 맞게 사용자 지정 하 여 로컬 이미지를 만드는 것입니다.

### <a name="local-image-creation"></a>로컬 이미지 만들기

이미지는 로컬 위치에 다운로드 한 후 엽니다 **Hyper-v 관리자** 복사한 VHD를 사용 하 여 VM을 만들려고 합니다. 다음 지침은 간단한 버전인 있지만의 자세한 지침을 찾을 수 있습니다 [Hyper-v에서 가상 컴퓨터를 만들](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v)합니다.

복사 된 VHD를 사용 하 여 VM을 만들려면:

1. 엽니다는 **새 가상 머신 마법사**합니다.

2. 세대 지정 페이지에서 선택 **1 세대**합니다.

    ![세대 지정 페이지의 스크린샷입니다. "1 세대" 옵션을 선택 합니다.](media/a41174fd41302a181e46385e1e701975.png)

3. 검사점 형식에서 검사점을 사용 하지 않으려면 확인란의 선택을 취소 하 여 합니다.

    ![검사점 유형 섹션 검사점 페이지의 스크린샷.](media/20c6dda51d7cafef33251188ae1c0c6a.png)

또한 검사점을 사용 하지 않도록 설정 하려면 PowerShell에서 다음 cmdlet을 실행할 수 있습니다.

```powershell
Set-VM -Name <VMNAME> -CheckpointType Disabled
```

### <a name="fixed-disk"></a>고정된 디스크

기존 VHD에서 VM을 만든 경우 기본적으로 동적 디스크를 만듭니다. 선택 하 여 고정된 된 디스크를 변경할 수 있습니다 **디스크 편집...**  다음 그림과에서 같이 합니다. 자세한 내용은 참조 [Azure에 업로드할 Windows VHD 또는 VHDX 준비](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image)합니다.

![디스크 편집 옵션의 스크린샷입니다.](media/35772414b5a0f81f06f54065561d1414.png)

또한 디스크를 고정된 디스크로 변경 하려면 다음 PowerShell cmdlet을 실행할 수 있습니다.

```powershell
Convert-VHD –Path c:\\test\\MY-VM.vhdx –DestinationPath c:\\test\\MY-NEW-VM.vhd -VHDType Fixed
```

## <a name="software-preparation-and-installation"></a>소프트웨어 준비 및 설치

이 섹션에서는 준비 하 고 FSLogix, Windows Defender 및 기타 일반 응용 프로그램을 설치 하는 방법에 설명 합니다. 

Office 365 ProPlus 및 OneDrive VM에 설치 하는, 참조 [마스터 VHD 이미지에서 Office 설치](install-office-on-wvd-master-image.md)합니다. 이 문서를 반환 하 고 마스터 VHD 프로세스를 완료 하는 문서의 다음 단계에서 링크를 따릅니다.

사용자를 특정 LOB 응용 프로그램에 액세스 해야 하는 경우이 섹션의이 지침을 완료 한 후 설치 하는 것이 좋습니다.

### <a name="disable-automatic-updates"></a>자동 업데이트 사용 안 함

로컬 그룹 정책을 통해 자동 업데이트를 사용 하지 않도록 설정 합니다.

1. 오픈 **로컬 그룹 정책 편집기\\관리 템플릿\\Windows 구성 요소\\Windows 업데이트**합니다.
2. 마우스 오른쪽 단추로 클릭 **자동 업데이트 구성** 로 설정 하 고 **비활성**합니다.

또한 자동 업데이트를 사용 하지 않도록 설정 하려면 명령 프롬프트에서 다음 명령을 실행할 수 있습니다.

```batch
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU" /v NoAutoUpdate /t REG_DWORD /d 1 /f
```

### <a name="specify-start-layout-for-windows-10-pcs-optional"></a>(선택 사항) Windows 10 Pc에 대 한 시작 레이아웃을 지정 합니다.

Windows 10 Pc에 대 한 시작 레이아웃을 지정 하려면이 명령을 실행 합니다.

```batch
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer" /v SpecialRoamingOverrideAllowed /t REG_DWORD /d 1 /f
```

### <a name="set-up-user-profile-container-fslogix"></a>사용자 프로필 컨테이너 (FSLogix) 설정

이미지의 일부로 FSLogix 컨테이너를 포함 하려면의 지침을 따릅니다 [호스트 풀에 대 한 사용자 프로필 공유 설정](create-host-pools-user-profile.md#configure-the-fslogix-profile-container)합니다. 사용 하 여 FSLogix 컨테이너의 기능을 테스트할 수 있습니다 [이 빠른 시작](https://docs.fslogix.com/display/20170529/Profile+Containers+-+Quick+Start)합니다.

### <a name="configure-windows-defender"></a>Windows Defender를 구성 합니다.

Windows Defender를 VM에 구성 된 경우에서 구성 하지 스캔에 VHD 및 VHDX 파일의 전체 콘텐츠를 연결 하는 동안 있는지 확인 합니다.

이 구성만 첨부 파일 하는 동안 VHD 및 VHDX 파일의 검색을 제거 하지만 실시간 검사에 영향을 미치지 않습니다.

자세한 내용을 보려면 Windows Server에서 Windows Defender를 구성 하는 방법에 대 한 지침을 참조 하세요 [Windows Server에서 Windows Defender 바이러스 백신 구성 제외](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/configure-server-exclusions-windows-defender-antivirus)합니다.

Windows Defender 검사에서 특정 파일을 제외를 구성 하는 방법에 대 한 자세한 내용은 참조 하세요 [구성 파일 확장명과 폴더 위치를 기반으로 하는 제외의 유효성을 검사 하 고](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/configure-extension-file-exclusions-windows-defender-antivirus)입니다.

### <a name="configure-session-timeout-policies"></a>세션 시간 제한 정책 구성

호스트 풀의 모든 Vm은 동일한 보안 그룹의 일부 이므로 원격 세션 정책 그룹 정책 수준에서 적용할 수 있습니다.

원격 세션 정책 구성:

1. 이동할 **관리 템플릿** > **Windows 구성 요소** > **원격 데스크톱 서비스**  >  **원격 데스크톱 세션 호스트** > **세션 시간 제한**합니다.
2. 오른쪽 패널에서 선택 합니다 **유휴 활성 원격 데스크톱 서비스 세션에 대 한 시간 제한 설정** 정책입니다.
3. 모달 창이 나타나면 변경에서 정책 옵션을 **구성 되어 있지** 에 **Enabled** 정책을 활성화 하려면.
4. 아래 정책 옵션을 사용 하 여 드롭다운 메뉴를 설정 하는 시간의 양 **4 시간**합니다.

또한 다음 명령을 실행 하 여 원격 세션 정책을 수동으로 구성할 수 있습니다.

```batch
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v RemoteAppLogoffTimeLimit /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fResetBroken /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v MaxConnectionTime /t REG_DWORD /d 10800000 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v RemoteAppLogoffTimeLimit /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v MaxDisconnectionTime /t REG_DWORD /d 5000 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v MaxIdleTime /t REG_DWORD /d 7200000 /f
```

### <a name="set-up-time-zone-redirection"></a>표준 시간대 리디렉션 설정

호스트 풀의 모든 Vm은 동일한 보안 그룹의 일부 이므로 시간대 리디렉션 그룹 정책 수준에서 적용할 수 있습니다.

표준 시간대를 리디렉션하려면:

1. Active Directory 서버에서 엽니다는 **그룹 정책 관리 콘솔**합니다.
2. 사용자 도메인 및 그룹 정책 개체를 확장 합니다.
3. 마우스 오른쪽 단추로 클릭 합니다 **그룹 정책 개체** 선택한 그룹 정책 설정에 대해 만든 **편집**합니다.
4. 에 **그룹 정책 관리 편집기**, 이동할 **컴퓨터 구성** > **정책은** > **관리 템플릿을** > **Windows 구성 요소** > **원격 데스크톱 서비스** > **원격 데스크톱 세션 호스트**   >  **장치 및 리소스 리디렉션**합니다.
5. 사용 하도록 설정 합니다 **표준 시간대의 리디렉션 허용** 설정 합니다.

또한 표준 시간대를 리디렉션할 마스터 이미지에이 명령을 실행할 수 있습니다.

```batch
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fEnableTimeZoneRedirection /t REG_DWORD /d 1 /f
```

### <a name="disable-storage-sense"></a>저장 공간 센스를 사용 하지 않도록 설정

Windows 10 Enterprise 또는 Windows 10 Enterprise 다중 세션을 사용 하는 Windows 가상 데스크톱 세션 호스트에 대 한 저장 공간 센스를 사용 하지 않도록 설정 하는 것이 좋습니다. 설정 메뉴에서 저장 공간 센스를 비활성화할 수 있습니다 **저장소**다음 스크린샷에 표시 된 것 처럼:

![설정에서 저장소 메뉴의 스크린샷입니다. "저장 공간 센스" 옵션이 꺼져 있습니다.](media/storagesense.png)

또한 다음 명령을 실행 하 여 레지스트리를 사용 하 여 설정을 변경할 수 있습니다.

```batch
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\StorageSense\Parameters\StoragePolicy" /v 01 /t REG_DWORD /d 0 /f
```

### <a name="include-additional-language-support"></a>추가 언어 지원을 포함

이 문서에서는 언어 및 국가별 지원 구성 하는 방법을 다루지 않습니다. 자세한 내용은 다음 문서를 참조하세요.

- [Windows 이미지에 언어 추가](https://docs.microsoft.com/windows-hardware/manufacture/desktop/add-language-packs-to-windows)
- [주문형 기능](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities)
- [(해당 FOD) 주문형 기능을 언어 및 지역](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-language-fod)

### <a name="other-applications-and-registry-configuration"></a>다른 응용 프로그램 및 레지스트리 구성

이 섹션에서는 응용 프로그램 및 운영 체제 구성에 설명 합니다. 이 단원의 모든 구성은 명령줄에서 실행 될 수 있는 레지스트리 항목을 통해 수행 됩니다 및 regedit 도구입니다.

>[!NOTE]
>그룹 정책 개체 (Gpo) 또는 레지스트리 가져오기 중 하나를 사용 하 여 구성에서 모범 사례를 구현할 수 있습니다. 관리자는 조직의 요구 사항에 따라 옵션 중 하나를 선택할 수 있습니다.

컬렉션에 대 한 피드백 허브 원격 분석 데이터를 Windows 10 Enterprise 다중 세션에서이 명령을 실행 합니다.

```batch
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\DataCollection" /v AllowTelemetry /t REG_DWORD /d 3 /f
```

Watson 충돌을 해결 하려면 다음 명령을 실행 합니다.

```batch
remove CorporateWerServer* from Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Windows Error Reporting
```

5 k 해상도 지원을 해결 하려면 레지스트리 편집기에 다음 명령을 입력 합니다. Side-by-side-스택 수 있도록 하려면 명령을 실행 해야 합니다.

```batch
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v MaxMonitors /t REG_DWORD /d 4 /f
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v MaxXResolution /t REG_DWORD /d 5120 /f
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v MaxYResolution /t REG_DWORD /d 2880 /f

reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\rdp-sxs" /v MaxMonitors /t REG_DWORD /d 4 /f
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\rdp-sxs" /v MaxXResolution /t REG_DWORD /d 5120 /f
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\rdp-sxs" /v MaxYResolution /t REG_DWORD /d 2880 /f
```

## <a name="prepare-the-image-for-upload-to-azure"></a>Azure에 업로드할 이미지 준비

한 구성을 완료를 시작한 후 설치 된 모든 응용 프로그램의 지침을 따릅니다 [Azure에 업로드할 Windows VHD 또는 VHDX 준비](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image) 이미지 준비를 합니다.

업로드에 대 한 이미지를 준비한 후 VM은 해제 또는 할당 취소 된 상태로 유지 해야 합니다.

## <a name="upload-master-image-to-a-storage-account-in-azure"></a>Azure에서 저장소 계정에 마스터 이미지 업로드

이 섹션에서는 마스터 이미지를 로컬로 만들 때에 적용 됩니다.

다음 지침에서는 Azure storage 계정에 마스터 이미지를 업로드 하는 방법을 알려줍니다. Azure storage 계정에 아직 없는 경우의 지침을 따릅니다 [이 문서에서는](https://code.visualstudio.com/tutorials/static-website/create-storage) 만들어야 합니다.

1. 아직 없는 경우 VM 이미지 (VHD) Fixed로 변환 합니다. 이미지를 고정으로 변환 하지 이미지를 만들 수 없습니다.

2. 저장소 계정의 blob 컨테이너에 VHD를 업로드 합니다. 사용 하 여 신속 하 게 업로드할 수는 [Storage 탐색기 도구](https://azure.microsoft.com/features/storage-explorer/)합니다. Storage 탐색기 도구에 대 한 자세한 내용은 참조 하세요 [이 문서에서는](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows)합니다.

    ![Microsoft Azure 저장소 탐색기 도구의 검색 창의 스크린샷. "(권장) 하는 페이지 blob으로.vhd 또는 vhdx 파일을 업로드 합니다." 확인란 선택 됩니다.](media/897aa9a9b6acc0aa775c31e7fd82df02.png)

3. 다음으로, 브라우저 "이미지입니다."에 대 한 검색에서 Azure portal로 이동 검색 할 수 있어야 합니다 **이미지 만들기** 페이지에서 다음 스크린샷에 표시 된 대로:

    ![Azure portal의 만들기 이미지 페이지의 스크린샷 이미지에 대 한 예제 값으로 채워집니다.](media/d3c840fe3e2430c8b9b1f44b27d2bf4f.png)

4. 이미지를 만든 후 다음 스크린샷과에서 같은 알림이 표시 됩니다.

    !["성공적으로 만든된 이미지" 알림의 스크린샷.](media/1f41b7192824a2950718a2b7bb9e9d69.png)

## <a name="next-steps"></a>다음 단계

이미지를 만들었으므로 만들 수도 있고 호스트 풀 업데이트. 호스트 풀 만들기 및 업데이트 하는 방법에 대 한 자세한 내용은 다음 문서를 참조 합니다.

- [Azure Resource Manager 템플릿으로 호스트 풀 만들기](create-host-pools-arm-template.md)
- [자습서: Azure Marketplace를 사용하여 호스트 풀 만들기](create-host-pools-azure-marketplace.md)
- [PowerShell을 사용한 호스트 풀 만들기](create-host-pools-powershell.md)
- [호스트 풀에 대 한 사용자 프로필 공유 설정](create-host-pools-user-profile.md)
- [Windows 가상 데스크톱 부하 분산 방법 구성](configure-host-pool-load-balancing.md)
