---
title: 'Azure Active Directory Domain Services: 관리되는 도메인에 RHEL VM 조인 | Microsoft Docs'
description: Red Hat Enterprise Linux 가상 머신을 Azure AD 도메인 서비스에 가입
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: d76ae997-2279-46dd-bfc5-c0ee29718096
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: iainfou
ms.openlocfilehash: a6e78ea6a4427043bf3c06a4663029585c99e331
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67473149"
---
# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-to-a-managed-domain"></a>Red Hat Enterprise Linux 7 가상 컴퓨터를 관리되는 도메인에 가입
이 문서에서는 Red Hat Enterprise Linux(RHEL) 7 가상 머신을 Azure AD 도메인 서비스 관리되는 도메인에 가입하는 방법을 보여 줍니다.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>시작하기 전에
이 문서에 나열된 작업을 수행하려면 다음이 필요합니다.  
1. 유효한 **Azure 구독**.
2. **Azure AD 디렉터리** - 온-프레미스 디렉터리 또는 클라우드 전용 디렉터리와 동기화됩니다.
3. **Azure AD 도메인 서비스** 를 사용하도록 설정해야 합니다. 그렇지 않은 경우 [시작 가이드](create-instance.md)에 간략히 설명된 모든 작업을 따릅니다.
4. 관리되는 도메인의 IP 주소를 가상 네트워크에 대한 DNS 서버로 구성했는지 확인합니다. 자세한 내용은 [Azure 가상 네트워크에 대한 DNS 설정을 업데이트하는 방법](active-directory-ds-getting-started-dns.md)을 참조하세요.
5. [Azure AD Domain Services 관리되는 도메인에 암호를 동기화](active-directory-ds-getting-started-password-sync.md)하는 데 필요한 단계를 완료합니다.


## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a>Red Hat Enterprise Linux 가상 머신 프로비전
다음 방법 중 하나를 사용하여 Azure에서 RHEL 7 가상 머신을 프로비전합니다.
* [Azure Portal](../virtual-machines/linux/quick-create-portal.md)
* [Azure CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

> [!IMPORTANT]
> * **Azure AD Domain Services를 활성화한 동일한 가상 네트워크**에 가상 머신을 배포합니다.
> * Azure AD Domain Services를 활성화한 서브넷이 아닌 **다른 서브넷**을 선택합니다.
>


## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>새로 프로비전된 Linux 가상 머신에 원격으로 연결
RHEL 7.2 가상 머신이 Azure에서 프로비전되었습니다. 다음 작업은 VM을 프로비전할 때 만든 로컬 관리자 계정을 사용하여 가상 머신에 원격으로 연결하는 것입니다.

[Linux를 실행하는 가상 머신에 로그온하는 방법](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 문서의 지침을 따르세요.


## <a name="configure-the-hosts-file-on-the-linux-virtual-machine"></a>Linux 가상 컴퓨터에서 호스트 파일 구성
SSH 터미널에서 /etc/hosts 파일을 편집하고 컴퓨터의 IP 주소 및 호스트 이름을 업데이트합니다.

```
sudo vi /etc/hosts
```

호스트 파일에 다음 값을 입력합니다.

```
127.0.0.1 contoso-rhel.contoso100.com contoso-rhel
```
여기서 'contoso100.com'은 관리되는 도메인의 DNS 도메인 이름입니다. 'contoso-rhel'은 관리되는 도메인에 가입한 RHEL 가상 머신의 호스트 이름입니다.


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>Linux 가상 머신에 필요한 패키지 설치
다음으로 가상 머신에서 도메인 가입에 필요한 패키지를 설치합니다. SSH 터미널에서 필수 패키지를 설치하려면 다음 명령을 입력합니다.

    ```
    sudo yum install realmd sssd krb5-workstation krb5-libs samba-common-tools
    ```


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>Linux 가상 컴퓨터를 관리되는 도메인에 가입
이제 필요한 패키지를 Linux 가상 머신에 설치했고 다음 작업은 가상 머신을 관리되는 도메인에 가입하는 것입니다.

1. AAD 도메인 서비스 관리되는 도메인을 검색합니다. SSH 터미널에서 다음 명령을 입력합니다.

    ```
    sudo realm discover CONTOSO100.COM
    ```

   > [!NOTE]
   > **문제 해결:** *영역 검색*을 통해 관리되는 도메인을 찾을 수 없는 경우 다음을 수행합니다.
   >   * 해당 도메인을 가상 머신에서 연결 가능한지 확인합니다(ping 시도).
   >   * 또한 관리되는 도메인을 사용할 수 있는 동일한 가상 네트워크에 가상 머신을 확실히 배포했는지 확인합니다.
   >   * 가상 네트워크가 관리되는 도메인의 도메인 컨트롤러를 가리키도록 DNS 서버 설정을 업데이트했는지 확인합니다.

2. Kerberos를 초기화합니다. SSH 터미널에서 다음 명령을 입력합니다.

    > [!TIP]
    > * 'AAD DC 관리자' 그룹에 속한 사용자를 지정해야 합니다.
    > * 도메인 이름을 대문자로 지정하지 않으면 kinit가 실패합니다.
    >

    ```
    kinit bob@CONTOSO100.COM
    ```

3. 컴퓨터를 도메인에 가입합니다. SSH 터미널에서 다음 명령을 입력합니다.

    > [!TIP]
    > 이전 단계에서 지정한 동일한 사용자 계정을 사용합니다.
    >

    ```
    sudo realm join --verbose CONTOSO100.COM -U 'bob@CONTOSO100.COM'
    ```

컴퓨터가 관리되는 도메인에 성공적으로 가입되면 메시지("Successfully enrolled machine in realm")가 표시됩니다.


## <a name="verify-domain-join"></a>도메인 가입 확인
컴퓨터가 관리되는 도메인에 성공적으로 가입되었는지 여부를 확인합니다. 다른 SSH 연결을 사용하여 도메인에 가입된 RHEL VM에 연결합니다. 도메인 사용자 계정을 사용하고 사용자 계정이 올바른지 확인합니다.

1. SSH 터미널에서 다음 명령을 입력하고 SSH를 사용하여 도메인에 가입된 RHEL 가상 머신에 연결합니다. 관리되는 도메인에 속하는 도메인 계정을 사용합니다(예: 여기서는 ‘bob@CONTOSO100.COM’).
    ```
    ssh -l bob@CONTOSO100.COM contoso-rhel.contoso100.com
    ```

2. SSH 터미널에서 다음 명령을 입력하여 홈 디렉터리가 올바르게 초기화되었는지 확인합니다.
    ```
    pwd
    ```

3. SSH 터미널에서 다음 명령을 입력하여 그룹 멤버 자격이 올바르게 확인되었는지 확인합니다.
    ```
    id
    ```


## <a name="troubleshooting-domain-join"></a>도메인 가입 문제 해결
[도메인 가입 문제 해결](join-windows-vm.md#troubleshoot-joining-a-domain) 문서를 참조하세요.

## <a name="related-content"></a>관련 내용
* [Azure AD 도메인 서비스 - 시작 가이드](create-instance.md)
* [Windows Server 가상 머신을 Azure AD 도메인 서비스 관리되는 도메인에 가입](active-directory-ds-admin-guide-join-windows-vm.md)
* [Linux를 실행하는 가상 머신에 로그온하는 방법](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Kerberos 설치](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)
* [Red Hat Enterprise Linux 7 - Windows 통합 가이드](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
