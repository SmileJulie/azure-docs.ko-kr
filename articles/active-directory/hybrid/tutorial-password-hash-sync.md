---
title: '자습서: PHS(암호 해시 동기화)를 사용하여 단일 AD 포리스트를 Azure에 통합 | Microsoft Docs'
description: 암호 해시 동기화를 사용하여 하이브리드 ID 환경을 설정하는 방법을 설명합니다.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 81c88c90ff24cb5e0ab143237fb08678067ea993
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46305542"
---
# <a name="tutorial--integrate-a-single-ad-forest-using-password-hash-sync-phs"></a>자습서: PHS(암호 해시 동기화)를 사용하여 단일 AD 포리스트 통합

![생성](media/tutorial-password-hash-sync/diagram.png)

다음 자습서에서는 암호 해시 동기화를 사용하여 하이브리드 ID 환경을 만드는 방법을 설명합니다.  하이브리드 ID의 작동 방식을 테스트하여 더 익숙해지는 데 이 환경을 사용할 수 있습니다.

## <a name="prerequisites"></a>필수 조건
다음은 이 자습서를 완료하는 데 필요한 필수 구성 요소입니다.
- [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview)가 설치되어 있는 컴퓨터.  [Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os) 또는 [Windows Server 2016](https://docs.microsoft.com/windows-server/virtualization/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) 컴퓨터에서 이를 수행하는 것이 좋습니다.
- 가상 머신이 인터넷과 통신할 수 있는 [외부 네트워크 어댑터](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/connect-to-network).
- [Azure 구독](https://azure.microsoft.com/free)
- Windows Server 2016의 복사본

> [!NOTE]
> 이 자습서는 가장 빠른 시간 내에 자습서 환경을 만들 수 있도록 PowerShell 스크립트를 사용합니다.  각 스크립트는 스크립트의 시작 부분에 선언된 변수를 사용합니다.  사용자 환경에 맞게 변수를 변경할 수 있으며, 변경해야 합니다.
>
>사용된 스크립트는 Azure AD Connect를 설치하기 전에 일반 Active Directory 환경을 만듭니다.  그러한 스크립트는 모든 자습서와 관련이 있습니다.
>
> 이 자습서에 사용된 PowerShell 스크립트의 복사본은 Github [여기](https://github.com/billmath/tutorial-phs)에서 확인할 수 있습니다.

## <a name="create-a-virtual-machine"></a>가상 머신 만들기
하이브리드 ID 환경을 시작하고 실행하기 위해 첫 번째로 수행해야 하는 것은 온-프레미스 Active Directory 서버로 사용할 가상 머신을 만드는 것입니다.  다음을 수행합니다.

1. 관리자 권한으로 PowerShell ISE를 엽니다.
2. 다음 스크립트를 실행합니다.

```powershell
#Declare variables
$VMName = 'DC1'
$Switch = 'External'
$InstallMedia = 'D:\ISO\en_windows_server_2016_updated_feb_2018_x64_dvd_11636692.iso'
$Path = 'D:\VM'
$VHDPath = 'D:\VM\DC1\DC1.vhdx'
$VHDSize = '64424509440'

#Create New Virtual Machine
New-VM -Name $VMName -MemoryStartupBytes 16GB -BootDevice VHD -Path $Path -NewVHDPath $VHDPath -NewVHDSizeBytes $VHDSize  -Generation 2 -Switch $Switch  

#Set the memory to be non-dynamic
Set-VMMemory $VMName -DynamicMemoryEnabled $false

#Add DVD Drive to Virtual Machine
Add-VMDvdDrive -VMName $VMName -ControllerNumber 0 -ControllerLocation 1 -Path $InstallMedia

#Mount Installation Media
$DVDDrive = Get-VMDvdDrive -VMName $VMName

#Configure Virtual Machine to Boot from DVD
Set-VMFirmware -VMName $VMName -FirstBootDevice $DVDDrive 
```

## <a name="complete-the-operating-system-deployment"></a>운영 체제 배포 완료
가상 머신 빌드를 완료하려면 운영 체제 설치를 완료해야 합니다.

1. Hyper-V 관리자에서 가상 머신을 두 번 클릭합니다.
2. 시작 단추를 클릭합니다.
3.  ‘CD 또는 DVD에서 부팅하려면 아무 키나 누르세요’라는 메시지가 표시됩니다. 계속 진행합니다.
4. Windows Server 시작 화면에서 언어를 선택하고 **다음**을 클릭합니다.
5. **지금 설치**를 클릭합니다.
6. 라이선스 키를 입력하고 **다음**을 클릭합니다.
7. **사용 약관에 동의에 확인 표시를 한 후 **다음**을 클릭합니다.
8. **사용자 지정: Windows만 설치(고급)** 를 선택합니다.
9. **다음**을 누릅니다
10. 설치가 완료되고 나면 가상 머신을 다시 시작하고, 로그인한 후, Windows 업데이트를 실행하여 VM이 최신 버전인지 확인합니다.  최신 업데이트를 설치합니다.

## <a name="install-active-directory-prerequisites"></a>Active Directory 설치 필수 조건
가상 머신을 만들었으므로 이제 Active Directory를 설치하기 전에 다음을 수행해야 합니다.  즉, 가상 머신의 이름을 바꾸고, 고정 IP 주소 및 DNS 정보를 설정하고, 원격 서버 관리 도구를 설치해야 합니다.   다음을 수행합니다.

1. 관리자 권한으로 PowerShell ISE를 엽니다.
2. 다음 스크립트를 실행합니다.

```powershell
#Declare variables
$ipaddress = "10.0.1.117" 
$ipprefix = "24" 
$ipgw = "10.0.1.1" 
$ipdns = "10.0.1.117"
$ipdns2 = "8.8.8.8" 
$ipif = (Get-NetAdapter).ifIndex 
$featureLogPath = "c:\poshlog\featurelog.txt" 
$newname = "DC1"
$addsTools = "RSAT-AD-Tools" 

#Set static IP address
New-NetIPAddress -IPAddress $ipaddress -PrefixLength $ipprefix -InterfaceIndex $ipif -DefaultGateway $ipgw 

# Set the DNS servers
Set-DnsClientServerAddress -InterfaceIndex $ipif -ServerAddresses ($ipdns, $ipdns2)

#Rename the computer 
Rename-Computer -NewName $newname -force 

#Install features 
New-Item $featureLogPath -ItemType file -Force 
Add-WindowsFeature $addsTools 
Get-WindowsFeature | Where installed >>$featureLogPath 

#Restart the computer 
Restart-Computer
```

## <a name="create-a-windows-server-ad-environment"></a>Windows Server AD 환경 만들기
이제 VM을 만들고 이름을 변경한 후 고정 IP 주소가 있으므로 설치를 진행하고 Active Directory Domain Services를 구성할 수 있습니다.  다음을 수행합니다.

1. 관리자 권한으로 PowerShell ISE를 엽니다.
2. 다음 스크립트를 실행합니다.

```powershell 
#Declare variables
$DatabasePath = "c:\windows\NTDS"
$DomainMode = "WinThreshold"
$DomainName = "contoso.com"
$DomaninNetBIOSName = "CONTOSO"
$ForestMode = "WinThreshold"
$LogPath = "c:\windows\NTDS"
$SysVolPath = "c:\windows\SYSVOL"
$featureLogPath = "c:\poshlog\featurelog.txt" 
$Password = "Pass1w0rd"

#Install AD DS, DNS and GPMC 
start-job -Name addFeature -ScriptBlock { 
Add-WindowsFeature -Name "ad-domain-services" -IncludeAllSubFeature -IncludeManagementTools 
Add-WindowsFeature -Name "dns" -IncludeAllSubFeature -IncludeManagementTools 
Add-WindowsFeature -Name "gpmc" -IncludeAllSubFeature -IncludeManagementTools } 
Wait-Job -Name addFeature 
Get-WindowsFeature | Where installed >>$featureLogPath

#Create New AD Forest
Install-ADDSForest -CreateDnsDelegation:$false -DatabasePath $DatabasePath -DomainMode $DomainMode -DomainName $DomainName -SafeModeAdministratorPassword $Password -DomainNetbiosName $DomainNetBIOSName -ForestMode $ForestMode -InstallDns:$true -LogPath $LogPath -NoRebootOnCompletion:$false -SysvolPath $SysVolPath -Force:$true
```

## <a name="create-a-windows-server-ad-user"></a>Windows Server AD 사용자 만들기
Active Directory 환경을 만들었으므로 테스트 계정이 필요합니다.  이 계정은 온-프레미스 AD 환경에 생성된 다음, Azure AD에 동기화됩니다.  다음을 수행합니다.

1. 관리자 권한으로 PowerShell ISE를 엽니다.
2. 다음 스크립트를 실행합니다.

```powershell 
#Declare variables
$Givenname = "Allie"
$Surname = "McCray"
$Displayname = "Allie McCray"
$Name = "amccray"
$Password = "Pass1w0rd"
$Identity = "CN=ammccray,CN=Users,DC=contoso,DC=com"
$SecureString = ConvertTo-SecureString $Password -AsPlainText -Force


#Create the user
New-ADUser -Name $Name -GivenName $Givenname -Surname $Surname -DisplayName $Displayname -AccountPassword $SecureString

#Set the password to never expire
Set-ADUser -Identity $Identity -PasswordNeverExpires $true -ChangePasswordAtLogon $false -Enabled $true
```

## <a name="create-an-azure-ad-tenant"></a>Azure AD 테넌트 만들기
이제 클라우드로 사용자를 동기화할 수 있도록 Azure AD 테넌트를 만들어야 합니다.  새 Azure AD 테넌트를 만들려면 다음 단계를 수행합니다.

1. [Azure Portal](https://portal.azure.com)을 찾아 Azure 구독이 있는 계정으로 로그인합니다.
2. **더하기 아이콘(+)** 을 선택하고 **Azure Active Directory**를 검색합니다.
3. 검색 결과에서 **Azure Active Directory**를 선택합니다.
4. **만들기**를 선택합니다.</br>
![만들기](media/tutorial-password-hash-sync/create1.png)</br>
5. **초기 도메인 이름**과 함께 **조직에 사용할 이름**을 입력합니다. 그런 다음 **만들기**를 선택합니다. 그러면 디렉터리가 만들어집니다.
6. 이 작업이 완료되면 **여기** 링크를 클릭하여 디렉터리를 관리합니다.

## <a name="create-a-global-administrator-in-azure-ad"></a>Azure AD에서 글로벌 관리자 만들기
이제 Azure AD 테넌트가 있으므로 글로벌 관리자 계정을 만들겠습니다.  이 계정은 Azure AD Connect를 설치하는 동안 Azure AD Connector 계정을 만드는 데 사용됩니다.  Azure AD Connect 계정은 Azure AD에 정보를 쓰는 데 사용됩니다.   글로벌 관리자 계정을 만들려면 다음을 수행합니다.

1.  **관리**에서 **사용자**를 선택합니다.</br>
![만들기](media/tutorial-password-hash-sync/gadmin1.png)</br>
2.  **모든 사용자**를 선택한 다음, **+새 사용자**를 선택합니다.
3.  이 사용자에 사용할 이름 및 사용자 이름을 입력합니다. 이것이 테넌트에 대한 글로벌 관리자가 됩니다. **디렉터리 역할**을 **글로벌 관리자**로 변경할 수도 있습니다. 또한 임시 암호를 표시할 수 있습니다. 작업이 완료되면 **만들기**를 선택합니다.</br>
![만들기](media/tutorial-password-hash-sync/gadmin2.png)</br>
4. 이 작업이 완료되면 새 웹 브라우저를 열고 새 글로벌 관리자 계정 및 임시 암호를 사용하여 myapps.microsoft.com에 로그인합니다.
5. 글로벌 관리자에 대한 암호를 기억할 수 있는 것으로 변경합니다.

## <a name="download-and-install-azure-ad-connect"></a>Azure AD Connect 다운로드 및 설치
이제 Azure AD Connect를 다운로드하고 설치할 시간입니다.  설치되고 나면 빠른 설치를 통해 실행됩니다.  다음을 수행합니다.

1. [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594)를 다운로드합니다.
2. **AzureADConnect.msi**를 찾아서 두 번 클릭합니다.
3. 시작 화면에서 사용권 계약에 동의하는 상자를 선택하고 **계속**을 클릭합니다.  
4. 기본 설정 화면에서 **Use express settings**(기본 설정 사용)를 클릭합니다.</br>  
![만들기](media/tutorial-password-hash-sync/express1.png)</br>
5. Azure AD에 연결 화면에서 Azure AD에 대한 글로벌 관리자의 사용자 이름 및 암호를 입력합니다. **다음**을 클릭합니다.  
6. AD DS에 연결 화면에서 엔터프라이즈 관리자 계정의 사용자 이름 및 암호를 입력합니다. **다음**을 클릭합니다.  
7. 구성 준비 화면에서 **설치**를 클릭합니다.
8. 설치가 완료되면 **끝내기**를 클릭합니다.
9. 설치가 완료된 후 Synchronization Service Manager 또는 Synchronization Rule Editor를 사용하기 전에 로그아웃하고 다시 로그인합니다.


## <a name="verify-users-are-created-and-synchronization-is-occurring"></a>사용자가 생성되고 동기화가 수행되는지 확인
이제 Microsoft에서 사용자가 온-프레미스 디렉터리에서 동기화되었으며 이제 Azure AD 테넌트에 존재하는지를 확인합니다.  이 작업을 완료하는 데 몇 시간이 걸릴 수 있습니다.  사용자가 동기화되었는지 확인하려면 다음을 수행합니다.


1. [Azure Portal](https://portal.azure.com)을 찾아 Azure 구독이 있는 계정으로 로그인합니다.
2. 왼쪽에서 **Azure Active Directory**를 선택합니다.
3. **관리**에서 **사용자**를 선택합니다.
4. 새 사용자가 테넌트에 표시되는지 확인</br>
![동기화](media/tutorial-password-hash-sync/synch1.png)</br>

## <a name="test-signing-in-with-one-of-our-users"></a>사용자로 로그인 테스트

1.  [http://myapps.microsoft.com](http://myapps.microsoft.com)을 찾아봅니다.
2. 새 테넌트에 생성된 사용자 계정으로 로그인합니다.  다음 형식을 사용하여 로그인해야 합니다(user@domain.onmicrosoft.com). 사용자가 온-프레미스 로그인에 사용한 것과 동일한 암호를 사용합니다.</br>
![Verify](media/tutorial-password-hash-sync/verify1.png)</br>

이제 Azure가 제공하는 기능을 테스트하여 익숙해지는 데 사용할 수 있는 하이브리드 ID 환경을 성공적으로 설치했습니다.

## <a name="next-steps"></a>다음 단계


- [하드웨어 및 필수 구성 요소](how-to-connect-install-prerequisites.md) 
- [Express 설정](how-to-connect-install-express.md)
- [암호 해시 동기화](how-to-connect-password-hash-synchronization.md)|