---
title: 자습서-Azure Container Instances 템플릿을 통해 다중 컨테이너 그룹 배포
description: 이 자습서에서는 Azure CLI에 Azure Resource Manager 템플릿을 사용 하 여 Azure Container Instances에 여러 컨테이너가 있는 컨테이너 그룹을 배포 하는 방법에 대해 알아봅니다.
services: container-instances
author: dlepow
manager: gwallace
ms.service: container-instances
ms.topic: article
ms.date: 04/03/2019
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 7438b5a91d3bf0ce8330e33bc1c849a8b0329c6f
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68325906"
---
# <a name="tutorial-deploy-a-multi-container-group-using-a-resource-manager-template"></a>자습서: 리소스 관리자 템플릿을 사용 하 여 다중 컨테이너 그룹 배포

> [!div class="op_single_selector"]
> * [YAML](container-instances-multi-container-yaml.md)
> * [리소스 관리자](container-instances-multi-container-group.md)

Azure Container Instances에서는 [컨테이너 그룹](container-instances-container-groups.md)을 사용하여 여러 컨테이너를 단일 호스트에 배포하도록 지원합니다. 컨테이너 그룹은 로깅, 모니터링 또는 서비스가 두 번째 연결 된 프로세스를 필요로 하는 기타 구성에 대해 사이드카 응용 프로그램을 빌드할 때 유용 합니다.

이 자습서에서는 Azure CLI를 사용 하 여 Azure Resource Manager 템플릿을 배포 하 여 간단한 두 컨테이너 사이드카 구성을 실행 하는 단계를 수행 합니다. 여기에서는 다음과 같은 작업을 수행하는 방법에 대해 배우게 됩니다.

> [!div class="checklist"]
> * 다중 컨테이너 그룹 템플릿 구성
> * 컨테이너 그룹 배포
> * 컨테이너 로그 보기

컨테이너 그룹을 사용 하 여 추가 Azure 서비스 리소스 (예: Azure Files 공유 또는 가상 네트워크)를 배포 해야 하는 경우 시나리오에 대 한 리소스 관리자 템플릿을 쉽게 수정할 수 있습니다. 

> [!NOTE]
> 현재 다중 컨테이너 그룹은 Linux 컨테이너에 제한됩니다. 

Azure 구독이 아직 없는 경우 시작하기 전에 [무료 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)을 만듭니다.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="configure-a-template"></a>템플릿 구성

다음 JSON을 이라는 `azuredeploy.json`새 파일에 복사 하 여 시작 합니다. Azure Cloud Shell에서 Visual Studio Code를 사용 하 여 작업 디렉터리에 파일을 만들 수 있습니다.

```
code azuredeploy.json
```

이 Resource Manager 템플릿은 두 개의 컨테이너, 하나의 공용 IP 주소, 두 개의 노출된 포트가 포함된 컨테이너 그룹을 정의합니다. 그룹의 첫 번째 컨테이너는 인터넷 연결 웹 애플리케이션을 실행합니다. 두 번째 컨테이너인 사이드카는 그룹 로컬 네트워크를 통해 주 웹 애플리케이션에 대한 HTTP 요청을 수행합니다.

```JSON
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "containerGroupName": {
      "type": "string",
      "defaultValue": "myContainerGroup",
      "metadata": {
        "description": "Container Group name."
      }
    }
  },
  "variables": {
    "container1name": "aci-tutorial-app",
    "container1image": "mcr.microsoft.com/azuredocs/aci-helloworld:latest",
    "container2name": "aci-tutorial-sidecar",
    "container2image": "mcr.microsoft.com/azuredocs/aci-tutorial-sidecar"
  },
  "resources": [
    {
      "name": "[parameters('containerGroupName')]",
      "type": "Microsoft.ContainerInstance/containerGroups",
      "apiVersion": "2018-10-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "containers": [
          {
            "name": "[variables('container1name')]",
            "properties": {
              "image": "[variables('container1image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              },
              "ports": [
                {
                  "port": 80
                },
                {
                  "port": 8080
                }
              ]
            }
          },
          {
            "name": "[variables('container2name')]",
            "properties": {
              "image": "[variables('container2image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              }
            }
          }
        ],
        "osType": "Linux",
        "ipAddress": {
          "type": "Public",
          "ports": [
            {
              "protocol": "tcp",
              "port": "80"
            },
            {
                "protocol": "tcp",
                "port": "8080"
            }
          ]
        }
      }
    }
  ],
  "outputs": {
    "containerIPv4Address": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.ContainerInstance/containerGroups/', parameters('containerGroupName'))).ipAddress.ip]"
    }
  }
}
```

프라이빗 컨테이너 이미지 레지스트리를 사용하려면 다음과 같은 형식의 개체를 JSON 문서에 추가합니다. 이 구성의 예제 구현은 [ACI 리소스 관리자 템플릿 참조][template-reference] 설명서를 참조 하세요.

```JSON
"imageRegistryCredentials": [
  {
    "server": "[parameters('imageRegistryLoginServer')]",
    "username": "[parameters('imageRegistryUsername')]",
    "password": "[parameters('imageRegistryPassword')]"
  }
]
```

## <a name="deploy-the-template"></a>템플릿 배포

[az group create][az-group-create] 명령을 사용하여 리소스 그룹을 만듭니다.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

[az group deployment create][az-group-deployment-create] 명령을 사용하여 템플릿을 배포합니다.

```azurecli-interactive
az group deployment create --resource-group myResourceGroup --template-file azuredeploy.json
```

몇 초 정도 지나면 Azure에서 초기 응답이 수신됩니다.

## <a name="view-deployment-state"></a>배포 상태 확인

배포 상태를 보려면 다음 [az container show][az-container-show] 명령을 사용 합니다.

```azurecli-interactive
az container show --resource-group myResourceGroup --name myContainerGroup --output table
```

실행 중인 애플리케이션을 보려면 사용하는 브라우저에서 공용 IP 주소로 이동합니다. 예를 들어 이 예제 출력의 IP는 `52.168.26.124`입니다.

```bash
Name              ResourceGroup    Status    Image                                                                                               IP:ports              Network    CPU/Memory       OsType    Location
----------------  ---------------  --------  --------------------------------------------------------------------------------------------------  --------------------  ---------  ---------------  --------  ----------
myContainerGroup  danlep0318r      Running   mcr.microsoft.com/azuredocs/aci-tutorial-sidecar,mcr.microsoft.com/azuredocs/aci-helloworld:latest  20.42.26.114:80,8080  Public     1.0 core/1.5 gb  Linux     eastus
```

## <a name="view-container-logs"></a>컨테이너 로그 보기

[Az container logs][az-container-logs] 명령을 사용 하 여 컨테이너의 로그 출력을 확인 합니다. `--container-name` 인수는 로그를 가져올 컨테이너를 지정합니다. 이 예제 `aci-tutorial-app` 에서는 컨테이너를 지정 합니다.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-app
```

출력:

```bash
listening on port 80
::1 - - [21/Mar/2019:23:17:48 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [21/Mar/2019:23:17:51 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [21/Mar/2019:23:17:54 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

사이드카 컨테이너에 대 한 로그를 보려면 `aci-tutorial-sidecar` 컨테이너를 지정 하는 비슷한 명령을 실행 합니다.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-sidecar
```

출력:

```bash
Every 3s: curl -I http://localhost                          2019-03-21 20:36:41

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0  1663    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
HTTP/1.1 200 OK
X-Powered-By: Express
Accept-Ranges: bytes
Cache-Control: public, max-age=0
Last-Modified: Wed, 29 Nov 2017 06:40:40 GMT
ETag: W/"67f-16006818640"
Content-Type: text/html; charset=UTF-8
Content-Length: 1663
Date: Thu, 21 Mar 2019 20:36:41 GMT
Connection: keep-alive
```

위 출력에 나와 있는 것처럼, 사이드카는 그룹 로컬 네트워크를 통해 주 웹 애플리케이션에 대한 HTTP 요청을 주기적으로 수행하여 해당 애플리케이션이 실행되고 있는지를 확인합니다. 이 사이드카 예제는 이외의 `200 OK`HTTP 응답 코드를 받은 경우 경고를 트리거하기 위해 확장할 수 있습니다.

## <a name="next-steps"></a>다음 단계

이 자습서에서는 Azure Container Instances에서 Azure Resource Manager 템플릿을 사용 하 여 다중 컨테이너 그룹을 배포 했습니다. 다음 방법에 대해 알아보았습니다.

> [!div class="checklist"]
> * 다중 컨테이너 그룹 템플릿 구성
> * 컨테이너 그룹 배포
> * 컨테이너 로그 보기

추가 템플릿 샘플은 [Azure Container Instances Azure Resource Manager 템플릿](container-instances-samples-rm.md)을 참조 하세요.

[Yaml 파일](container-instances-multi-container-yaml.md)을 사용 하 여 다중 컨테이너 그룹을 지정할 수도 있습니다. YAML 형식의 보다 간결한 특성으로 인해, 배포에 컨테이너 인스턴스만 포함 된 경우에는 YAML 파일을 사용 하 여 배포 하는 것이 좋습니다.


<!-- LINKS - Internal -->
[aci-tutorial]: ./container-instances-tutorial-prepare-app.md
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-create]: /cli/azure/group#az-group-create
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[template-reference]: https://docs.microsoft.com/azure/templates/microsoft.containerinstance/containergroups
