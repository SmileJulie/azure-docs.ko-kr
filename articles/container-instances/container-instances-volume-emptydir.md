---
title: Azure Container Instances에서 emptyDir 볼륨 탑재
description: Azure Container Instances에서 컨테이너 그룹에 있는 컨테이너 간에 데이터를 공유하기 위해 emptyDir 볼륨을 탑재하는 방법을 알아봅니다.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 02/08/2018
ms.author: danlep
ms.openlocfilehash: d91706da898e84effc6194a74dce69a66be0f4ac
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657627"
---
# <a name="mount-an-emptydir-volume-in-azure-container-instances"></a>Azure Container Instances에서 emptyDir 볼륨 탑재

Azure Container Instances 컨테이너 그룹의 컨테이너 간에 데이터를 공유하기 위해 *emptyDir* 볼륨을 탑재하는 방법을 알아봅니다.

> [!NOTE]
> 현재 *emptyDir* 볼륨 탑재는 Linux 컨테이너로 제한됩니다. Windows 컨테이너에 모든 기능을 제공 하는 중, 하는 동안에에서 현재 플랫폼의 차이점을 찾을 수 있습니다 합니다 [개요](container-instances-overview.md#linux-and-windows-containers)합니다.

## <a name="emptydir-volume"></a>emptyDir 볼륨

*emptyDir* 볼륨은 컨테이너 그룹의 각 컨테이너에 액세스할 수 있는 쓰기 가능한 디렉터리를 제공합니다. 그룹의 컨테이너는 볼륨에 있는 동일한 파일을 읽고 쓸 수 있고 같거나 다른 경로를 사용하여 각 컨테이너에 탑재될 수 있습니다.

*emptyDir* 볼륨에 대한 몇 가지 사용 예:

* 스크래치 공백
* 오랫동안 실행 중인 작업에 대한 검사점
* 사이드카 컨테이너에서 검색하고 애플리케이션 컨테이너에서 제공된 데이터 저장

*emptyDir* 볼륨의 데이터는 컨테이너 충돌까지 유지됩니다. 그러나 다시 시작된 컨테이너가 *emptyDir* 볼륨의 데이터를 유지하는 것은 보장되지 않습니다.

## <a name="mount-an-emptydir-volume"></a>emptyDir 볼륨 탑재

컨테이너 인스턴스에서 emptyDir 볼륨을 탑재하려면 [Azure Resource Manager 템플릿](/azure/templates/microsoft.containerinstance/containergroups)을 사용하여 배포해야 합니다.

먼저 템플릿의 `volumes`컨테이너 그룹의 배열`properties` 섹션을 채웁니다. 다음으로 *emptyDir* 볼륨을 탑재하려는 컨테이너 그룹에 있는 각 컨테이너의 경우 컨테이너 정의의 `properties` 섹션에서 `volumeMounts` 배열을 채웁니다.

예를 들어 다음과 같은 Resource Manager 템플릿은 두 컨테이너로 구성된 컨테이너 그룹을 만들고 각 그룹은 *emptyDir* 볼륨을 탑재합니다.

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-emptydir.json -->
[!code-json[volume-emptydir](~/azure-docs-json-samples/container-instances/aci-deploy-volume-emptydir.json)]

Azure Resource Manager 템플릿을 사용하는 컨테이너 인스턴스 배포의 예제를 보려면 [Azure Container Instances에서 다중 컨테이너 그룹 배포](container-instances-multi-container-group.md)를 참조하세요.

## <a name="next-steps"></a>다음 단계

Azure Container Instances에서 다른 볼륨 유형을 탑재하는 방법을 알아봅니다.

* [Azure Container Instances를 사용하여 Azure 파일 공유 탑재](container-instances-volume-azure-files.md)
* [Azure Container Instances에서 gitRepo 볼륨 탑재](container-instances-volume-gitrepo.md)
* [Azure Container Instances에서 비밀 볼륨 탑재](container-instances-volume-secret.md)