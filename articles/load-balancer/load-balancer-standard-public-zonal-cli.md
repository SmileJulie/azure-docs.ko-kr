---
title: Azure CLI를 사용하여 영역별 프런트 엔드가 있는 표준 Load Balancer 만들기
titlesuffix: Azure Load Balancer
description: Azure CLI를 사용하여 영역별 공용 IP 주소 프런트 엔드가 있는 공용 표준 Load Balancer를 만드는 방법을 알아봅니다.
services: load-balancer
documentationcenter: na
author: asudbring
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/2018
ms.author: allensu
ms.openlocfilehash: 7da41456a4f4bb88d402d27b42b31f6d4adfa7f6
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68274311"
---
#  <a name="create-a-standard-load-balancer-with-zonal-frontend-using-azure-cli"></a>Azure CLI를 사용하여 영역별 프런트 엔드가 있는 표준 Load Balancer 만들기

이 문서에서는 공용 IP 표준 주소를 사용하여 영역별 프런트 엔드가 있는 공용 [표준 Load Balancer](https://aka.ms/azureloadbalancerstandard)를 만드는 단계를 안내합니다. 이 시나리오에서는 프런트 엔드 및 백 엔드 인스턴스에 대한 특정 영역을 지정하여 특정 영역으로 데이터 경로 및 리소스를 맞출 수도 있습니다.

Standard Load Balancer에서 가용성 영역 사용에 대한 자세한 내용은 [Standard Load Balancer 및 가용석 영역](load-balancer-standard-availability-zones.md)을 참조하세요.

Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)을 만듭니다.
 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI를 로컬로 설치하고 사용하도록 선택하는 경우 최신 [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)를 설치했고 [az login](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest)을 사용하여 Azure 계정에 로그인했는지 확인합니다.

> [!NOTE]
>  Azure 리소스, 지역 및 VM 크기 제품군을 선택하는 데 가용성 영역에 대한 지원을 사용할 수 있습니다. 시작하는 방법에 대한 자세한 내용 및 가용성 영역을 사용해 볼 수 있는 Azure 리소스, 지역 및 VM 크기 제품군은 [가용성 영역 개요](https://docs.microsoft.com/azure/availability-zones/az-overview)를 참조하세요. 지원을 위해 [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones)에 연결하거나 [Azure 지원 티켓을 열](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json) 수 있습니다.  

## <a name="create-a-resource-group"></a>리소스 그룹 만들기

[az group create](/cli/azure/group#az-group-create)를 사용하여 리소스 그룹을 만듭니다. Azure 리소스 그룹은 Azure 리소스가 배포 및 관리되는 논리적 컨테이너입니다.

다음 예제에서는 *westeurope* 위치에 *myResourceGroupLB*라는 리소스 그룹을 만듭니다.

```azurecli-interactive
az group create \
--name myResourceGroupLB \
--location westeurope
```

## <a name="create-a-zonal-public-ip-standard"></a>영역별 공용 IP 표준 만들기
인터넷에서 앱에 액세스하려면 부하 분산 장치에 대한 공용 IP 주소가 필요합니다. 특정 영역에 생성된 공용 IP 주소는 항상 해당 영역에만 존재합니다. 공용 IP 주소의 영역을 변경하는 것은 불가능합니다.

[az network public-ip create](/cli/azure/network/public-ip#az-network-public-ip-create)를 사용하여 공용 IP 주소를 만듭니다. 다음 예제에서는 영역 1의 *myResourceGroupLoadBalancer* 리소스 그룹에 *myPublicIP*라는 영역별 공용 IP 주소를 만듭니다.

```azurecli-interactive
az network public-ip create \
--resource-group myResourceGroupLB \
--name myPublicIP \
--sku Standard \
--zone 1
```

## <a name="create-azure-standard-load-balancer"></a>Azure 표준 Load Balancer 만들기
이 섹션에서는 다음과 같은 부하 분산 장치 구성 요소를 만들고 구성하는 방법에 대해 자세히 설명합니다.
- 부하 분산 장치에서 들어오는 네트워크 트래픽을 수신하는 프런트 엔드 IP 풀.
- 프런트 엔드 풀에서 부하 분산된 네트워크 트래픽을 전송하는 백 엔드 IP 풀.
- 백 엔드 VM 인스턴스의 상태를 확인하는 상태 프로브
- 트래픽이 VM에 분산되는 방법을 정의하는 부하 분산 장치 규칙

### <a name="create-the-load-balancer"></a>부하 분산 장치 만들기
[az network lb create](/cli/azure/network/lb#az-network-lb-create)를 사용하여 표준 Load Balancer를 만듭니다. 다음 예제에서는 *myLoadBalancer*라는 부하 분산 장치를 만들고 *myPublicIP* 주소를 프런트 엔드 IP 구성에 할당합니다.

```azurecli-interactive
az network lb create \
--resource-group myResourceGroupLB \
--name myLoadBalancer \
--public-ip-address myPublicIP \
--frontend-ip-name myFrontEndPool \
--backend-pool-name myBackEndPool \
--sku Standard
```

## <a name="create-health-probe-on-port-80"></a>포트 80에서 상태 프로브 만들기

상태 프로브는 네트워크 트래픽을 보낼 수 있도록 모든 가상 컴퓨터 인스턴스를 검사합니다. 프로브 검사에 실패한 가상 머신 인스턴스는 다시 온라인 상태가 되어 프로브 검사가 정상으로 나올 때까지 부하 분산 장치에서 제거됩니다. az network lb probe create를 사용하여 가상 머신의 상태를 모니터링하는 상태 프로브를 만듭니다. TCP 상태 프로브를 만들려면 [az network lb probe create](/cli/azure/network/lb/probe#az-network-lb-probe-create)를 사용합니다. 다음 예제에서는 *myHealthProbe*라는 상태 프로브를 만듭니다.

```azurecli-interactive
az network lb probe create \
--resource-group myResourceGroupLB \
--lb-name myLoadBalancer \
--name myHealthProbe \
--protocol tcp \
--port 80
```

## <a name="create-load-balancer-rule-for-port-80"></a>포트 80에 대해 부하 분산 장치 규칙 만들기
부하 분산 장치 규칙은 들어오는 트래픽에 대한 프런트 엔드 IP 구성 및 트래픽을 받을 백 엔드 IP 풀과 필요한 원본 및 대상 포트를 함께 정의합니다. [az network lb rule create](/cli/azure/network/lb/rule#az-network-lb-rule-create)를 사용하여 *myFrontEndPool* 프런트 엔드 풀에서 80 포트를 수신 대기하고, 마찬가지로 80 포트를 통해 *myBackEndPool* 백 엔드 주소 풀에 부하 분산된 네트워크 트래픽을 보내는 *myLoadBalancerRuleWeb* 부하 분산 장치 규칙을 만듭니다.

```azurecli-interactive
az network lb rule create \
--resource-group myResourceGroupLB \
--lb-name myLoadBalancer \
--name myLoadBalancerRuleWeb \
--protocol tcp \
--frontend-port 80 \
--backend-port 80 \
--frontend-ip-name myFrontEndPool \
--backend-pool-name myBackEndPool \
--probe-name myHealthProbe
```

## <a name="configure-virtual-network"></a>가상 네트워크 구성
일부 VM을 배포하고 부하 분산 장치를 테스트하려면, 먼저 지원되는 가상 네트워크 리소스를 만듭니다.

### <a name="create-a-virtual-network"></a>가상 네트워크 만들기

[az network vnet create](/cli/azure/network/vnet#az-network-vnet-create)를 사용하여 myResourceGroup에 *mySubnet*이라는 서브넷이 있는 *myVnet* 가상 네트워크를 만듭니다.


```azurecli-interactive
az network vnet create \
--resource-group myResourceGroupLB \
--location westeurope \
--name myVnet \
--subnet-name mySubnet
```

### <a name="create-a-network-security-group"></a>네트워크 보안 그룹 만들기

[az network nsg create](/cli/azure/network/nsg#az-network-nsg-create)를 사용하여 이름이 *myNetworkSecurityGroup*인 네트워크 보안 그룹을 만들어 가상 네트워크에 대한 인바운드 연결을 정의합니다.

```azurecli-interactive
az network nsg create \
--resource-group myResourceGroupLB \
--name myNetworkSecurityGroup
```

[az network nsg rule create](/cli/azure/network/nsg/rule#az-network-nsg-rule-create)로 포트 80에 대해 이름이 *myNetworkSecurityGroupRule*인 네트워크 보안 그룹을 만듭니다.

```azurecli-interactive
az network nsg rule create \
--resource-group myResourceGroupLB \
--nsg-name myNetworkSecurityGroup \
--name myNetworkSecurityGroupRule \
--protocol tcp \
--direction inbound \
--source-address-prefix '*' \
--source-port-range '*' \
--destination-address-prefix '*' \
--destination-port-range 80 \
--access allow \
--priority 200
```
### <a name="create-nics"></a>NIC 만들기
[az network nic create](/cli/azure/network/nic#az-network-nic-create)를 사용하여 3개의 가상 NIC를 만들고 공용 IP 주소 및 네트워크 보안 그룹에 연결합니다. 다음 예제에서는 3개의 가상 NIC를 만듭니다. (다음 단계에서 앱에 대해 만드는 각 VM에 대해 가상 NIC 하나씩) 언제든지 추가 가상 NIC 및 VM을 만든 후 부하 분산 장치에 추가할 수 있습니다.

```azurecli-interactive
for i in `seq 1 3`; do
    az network nic create \
        --resource-group myResourceGroupLB \
        --name myNic$i \
        --vnet-name myVnet \
        --subnet mySubnet \
        --network-security-group myNetworkSecurityGroup \
        --lb-name myLoadBalancer \
        --lb-address-pools myBackEndPool
done
```
## <a name="create-backend-servers"></a>백 엔드 서버 만들기
이 예제에서는 부하 분산 장치의 백 엔드 서버로 사용될 세 개의 가상 머신을 영역 1에 만듭니다. 또한 부하 분산 장치가 성공적으로 만들어졌는지 확인하기 위해 가상 머신에 NGINX를 설치합니다.

### <a name="create-cloud-init-config"></a>cloud-init 구성 만들기

cloud-init 구성 파일을 사용하여 NGINX를 설치하고 Linux 가상 머신에서 'Hello World' Node.js 앱을 실행할 수 있습니다. 현재 셸에서 cloud-init.txt라는 파일을 만들고 다음 구성을 복사하여 셸에 붙여넣습니다. 전체 cloud-init 파일, 특히 첫 줄이 올바르게 복사해야 합니다.

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

### <a name="create-the-zonal-virtual-machines"></a>영역별 가상 머신 만들기
[az vm create](/cli/azure/vm#az-vm-create)로 VM을 만듭니다. 다음 예제에서는 영역 1에 세 VM을 만들고 SSH 키가 아직 없으면 생성합니다.

```azurecli-interactive
for i in `seq 1 3`; do
 az vm create \
--resource-group myResourceGroupLB \
--name myVM$i \
--nics myNic$i \
--image UbuntuLTS \
--generate-ssh-keys \
--zone 1 \
--custom-data cloud-init.txt
done
```

## <a name="test-the-load-balancer"></a>부하 분산 장치 테스트
[az network public-ip show](/cli/azure/network/public-ip#az-network-public-ip-show)를 사용하여 부하 분산 장치의 공용 IP 주소를 가져옵니다. 

```azurecli-interactive
  az network public-ip show \
    --resource-group myResourceGroupSLB \
    --name myPublicIP \
    --query [ipAddress] \
    --output tsv
``` 

그런 다음 웹 브라우저에 공용 IP 주소를 입력할 수 있습니다. 부하 분산 장치가 트래픽을 분산하도록 시작하기 전에 VM이 준비하는 데 몇 분 정도 걸립니다. 다음 예제와 같이 부하 분산 장치가 트래픽을 분산한 VM의 호스트 이름을 포함하여 앱이 표시됩니다.

![Node.js 앱 실행](./media/load-balancer-standard-public-zonal-cli/running-nodejs-app.png)

앱을 실행하는 영역 1 안의 VM에 부하 분산 장치가 트래픽을 분산하는 것을 확인하기 위해 웹 브라우저를 강제로 새로 고칠 수 있습니다.

## <a name="next-steps"></a>다음 단계
- [Standard Load Balancer](./load-balancer-standard-overview.md)에 대해 자세히 알아보세요.



