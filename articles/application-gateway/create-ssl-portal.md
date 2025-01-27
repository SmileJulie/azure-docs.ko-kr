---
title: 자습서 - SSL 종료로 애플리케이션 게이트웨이 구성 - Azure Portal
description: 이 자습서에서는 Azure Portal을 사용하여 애플리케이션 게이트웨이를 구성하고 SSL 종료를 위한 인증서를 추가하는 방법을 알아봅니다.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: tutorial
ms.date: 4/17/2019
ms.author: victorh
ms.openlocfilehash: ed4230969e81eee0d77b7e4b69eac3a264068388
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67449161"
---
# <a name="tutorial-configure-an-application-gateway-with-ssl-termination-using-the-azure-portal"></a>자습서: Azure Portal을 사용하여 SSL 종료로 애플리케이션 게이트웨이 구성

Azure Portal을 사용하여 백 엔드 서버에 가상 머신을 사용하는 SSL 종료용 인증서가 있는 [애플리케이션 게이트웨이](overview.md)를 구성할 수 있습니다.

이 자습서에서는 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * 자체 서명된 인증서 만들기
> * 인증서가 있는 애플리케이션 게이트웨이 만들기
> * 백 엔드 서버로 사용되는 가상 머신 만들기
> * 애플리케이션 게이트웨이 테스트

Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)을 만듭니다.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure"></a>Azure에 로그인

[https://portal.azure.com](https://portal.azure.com) 에서 Azure Portal에 로그인합니다.

## <a name="create-a-self-signed-certificate"></a>자체 서명된 인증서 만들기

이 섹션에서는 [New-SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/new-selfsignedcertificate)를 사용하여 자체 서명된 인증서를 만듭니다. 애플리케이션 게이트웨이의 수신기를 만들 때 이 인증서를 Azure Portal에 업로드합니다.

로컬 컴퓨터에서 Windows PowerShell 창을 관리자로 엽니다. 다음 명령을 실행하여 인증서를 만듭니다.

```powershell
New-SelfSignedCertificate \
  -certstorelocation cert:\localmachine\my \
  -dnsname www.contoso.com
```

다음과 같은 응답이 표시됩니다.

```
PSParentPath: Microsoft.PowerShell.Security\Certificate::LocalMachine\my

Thumbprint                                Subject
----------                                -------
E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630  CN=www.contoso.com
```

[Export-PfxCertificate](https://docs.microsoft.com/powershell/module/pkiclient/export-pfxcertificate)을 인증서에서 pfx 파일을 내보내도록 반환된 지문과 함께 사용합니다.

```powershell
$pwd = ConvertTo-SecureString -String "Azure123456!" -Force -AsPlainText
Export-PfxCertificate \
  -cert cert:\localMachine\my\E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630 \
  -FilePath c:\appgwcert.pfx \
  -Password $pwd
```

## <a name="create-an-application-gateway"></a>애플리케이션 게이트웨이 만들기

가상 네트워크는 사용자가 만든 리소스 간의 통신에 필요합니다. 이 예제에서는 두 개의 서브넷을 만듭니다. 하나는 애플리케이션 게이트웨이용이고, 다른 하나는 백 엔드 서버용입니다. 애플리케이션 게이트웨이를 만드는 동시에 가상 네트워크를 만들 수 있습니다.

1. Azure Portal의 왼쪽 위 모서리에 있는 **새로 만들기**를 선택합니다.
2. **네트워킹**을 선택한 다음, 추천 목록에서 **Application Gateway**를 선택합니다.
3. 애플리케이션 게이트웨이의 이름으로 *myAppGateway*를 입력하고 새 리소스 그룹에 대해 *myResourceGroupAG*를 입력합니다.
4. 다른 설정은 기본값을 적용하고 **확인**을 선택합니다.
5. **가상 네트워크 선택**을 클릭하고 **새로 만들기**를 선택한 다음, 가상 네트워크의 다음 값을 입력합니다.

   - *myVNet* - 가상 네트워크 이름
   - *10.0.0.0/16* - 가상 네트워크 주소 공간
   - *myAGSubnet* - 서브넷 이름
   - *10.0.0.0/24* - 서브넷 주소 공간

     ![가상 네트워크 만들기](./media/create-ssl-portal/application-gateway-vnet.png)

6. **확인**을 선택하여 가상 네트워크 및 서브넷을 만듭니다.
7. **공용 IP 주소 선택**을 선택하고 **새로 만들기**를 선택한 다음, 공용 IP 주소의 이름을 입력합니다. 이 예제에서 공용 IP 주소의 이름은 *myAGPublicIPAddress*입니다. 다른 설정은 기본값을 적용하고 **확인**을 선택합니다.
8. 수신기 프로토콜로 **HTTPS**를 선택하고 포트가 **443**으로 정의되어 있는지 확인합니다.
9. 폴더 아이콘을 선택하고 이전에 만든 *appgwcert.pfx* 인증서를 찾아서 업로드합니다.
10. 인증서의 이름에 *mycert1*을 입력하고 암호에 *Azure123456!* 를 입력한 다음, **확인**을 선택합니다.

    ![새 애플리케이션 게이트웨이 만들기](./media/create-ssl-portal/application-gateway-create.png)

11. 요약 페이지에서 설정을 검토한 다음, **확인**을 선택하여 네트워크 리소스와 애플리케이션 게이트웨이를 만듭니다. 애플리케이션 게이트웨이가 생성되는 데 몇 분이 걸릴 수 있습니다. 배포가 완료될 때까지 기다렸다가 다음 섹션으로 이동합니다.

### <a name="add-a-subnet"></a>서브넷 추가

1. 왼쪽 메뉴에서 **모든 리소스**를 선택한 다음, 리소스 목록에서 **myVNet**을 선택합니다.
2. **서브넷**을 선택한 다음, **서브넷**을 선택합니다.

    ![서브넷 만들기](./media/create-ssl-portal/application-gateway-subnet.png)

3. 서브넷 이름으로 *myBackendSubnet*을 입력한 다음, **확인**을 선택합니다.

## <a name="create-backend-servers"></a>백 엔드 서버 만들기

이 예제에서는 애플리케이션 게이트웨이의 백 엔드 서버로 사용되는 두 개의 가상 머신을 만듭니다. 또한 가상 머신에 IIS를 설치하여 애플리케이션 게이트웨이가 성공적으로 만들어졌는지 확인합니다.

### <a name="create-a-virtual-machine"></a>가상 머신 만들기

1. **새로 만들기**를 선택합니다.
2. **계산**을 선택한 다음, 추천 목록에서 **Windows Server 2016 Datacenter**를 선택합니다.
3. 가상 머신에 대해 다음 값을 입력합니다.

    - *myVM* - 가상 머신의 이름
    - *azureuser* - 관리자 사용자 이름
    - *Azure123456!* - 암호
    - **기존 항목 사용**을 선택한 다음, *myResourceGroupAG*를 선택합니다.

4. **확인**을 선택합니다.
5. 가상 머신의 크기로 **DS1_V2**를 선택하고, **선택**을 클릭합니다.
6. 가상 네트워크에 대해 **myVNet**이 선택되어 있고 서브넷이 **myBackendSubnet**인지 확인합니다. 
7. **비활성화됨**을 선택하여 부팅 진단을 비활성화합니다.
8. **확인**을 선택하고, 요약 페이지에서 설정을 검토한 다음, **만들기**를 선택합니다.

### <a name="install-iis"></a>IIS 설치

1. 대화형 셸을 열고 **PowerShell**로 설정되어 있는지 확인합니다.

    ![사용자 지정 확장 설치](./media/create-ssl-portal/application-gateway-extension.png)

2. 다음 명령을 실행하여 가상 머신에 IIS를 설치합니다. 

    ```azurepowershell-interactive
    Set-AzVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -ExtensionName IIS `
      -VMName myVM `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
      -Location EastUS
    ```

3. 두 번째 가상 머신을 만들고, 방금 완료한 단계를 사용하여 IIS를 설치합니다. Set-AzVMExtension의 이름 및 VMName으로 *myVM2*를 입력합니다.

### <a name="add-backend-servers"></a>백 엔드 서버 추가

1. **모든 리소스**를 선택한 다음, **myAppGateway**를 선택합니다.
1. **백 엔드 풀**을 선택합니다. 기본 풀이 애플리케이션 게이트웨이와 함께 자동으로 만들어졌습니다. **appGatewayBackendPool**을 선택합니다.
1. **대상 추가**를 선택하여 앞에서 만든 각 가상 머신을 백 엔드 풀에 추가합니다.

    ![백 엔드 서버 추가](./media/create-ssl-portal/application-gateway-backend.png)

1. **저장**을 선택합니다.

## <a name="test-the-application-gateway"></a>애플리케이션 게이트웨이 테스트

1. **모든 리소스**를 선택한 다음, **myAGPublicIPAddress**를 선택합니다.

    ![애플리케이션 게이트웨이에 대한 공용 IP 주소 기록](./media/create-ssl-portal/application-gateway-ag-address.png)

2. 공용 IP 주소를 복사하여 브라우저의 주소 표시줄에 붙여넣습니다. 자체 서명된 인증서를 사용하는 경우 보안 경고를 수락하려면 세부 정보를 선택한 다음 웹 페이지로 이동을 선택합니다.

    ![보안 경고](./media/create-ssl-portal/application-gateway-secure.png)

    그러면 보안 IIS 웹 사이트가 다음 예제와 같이 표시됩니다.

    ![애플리케이션 게이트웨이의 기준 URL 테스트](./media/create-ssl-portal/application-gateway-iistest.png)

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [Azure Application Gateway를 통해 수행할 수 있는 작업에 대한 자세한 정보](application-gateway-introduction.md)
