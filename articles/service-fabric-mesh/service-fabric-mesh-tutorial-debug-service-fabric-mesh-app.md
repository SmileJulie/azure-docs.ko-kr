---
title: 자습서 - Azure Service Fabric Mesh 웹 응용 프로그램 디버그 | Microsoft Docs
description: 이 자습서에서는 로컬 클러스터에서 실행 중인 Azure Service Fabric Mesh 응용 프로그램을 디버그합니다.
services: service-fabric-mesh
documentationcenter: .net
author: TylerMSFT
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/17/2018
ms.author: twhitney
ms.custom: mvc, devcenter
ms.openlocfilehash: e39e5861c0606c43a869fe02a7de2dc9b6f489ea
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39125656"
---
# <a name="tutorial-debug-a-service-fabric-mesh-web-application"></a>자습서: Service Fabric Mesh 웹 응용 프로그램 디버그

이 자습서는 시리즈의 2부로, 로컬 개발 클러스터에서 Azure Service Fabric Mesh 웹 응용 프로그램을 디버그하는 방법을 보여줍니다.

이 자습서에서는 다음에 대해 알아봅니다.

> [!div class="checklist"]
> * Azure Service Fabric Mesh 응용 프로그램을 빌드할 때 발생하는 결과
> * 서비스 간 호출을 관찰하도록 중단점을 설정하는 방법

이 자습서 시리즈에서는 다음 방법에 대해 알아봅니다.
> [!div class="checklist"]
> * [Service Fabric Mesh 응용 프로그램 빌드](service-fabric-mesh-tutorial-create-dotnetcore.md)
> * 로컬로 앱 디버그
> * [Azure에 앱 게시](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>필수 조건

이 자습서를 시작하기 전에:

* Azure 구독이 아직 없으면 시작하기 전에 [무료 계정을 만들](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 수 있습니다.

* Service Fabric 런타임, SDK, Docker 및 Visual Studio 2017 설치가 포함된 [개발 환경을 설정](service-fabric-mesh-howto-setup-developer-environment-sdk.md)했는지 확인합니다.

## <a name="download-the-to-do-sample-application"></a>할 일 응용 프로그램 예제 다운로드

[이 자습서 시리즈의 1부](service-fabric-mesh-tutorial-create-dotnetcore.md)에서 할 일 응용 프로그램 예제를 만들지 않은 경우 다운로드할 수 있습니다. 명령 창에서 다음 명령을 실행하여 로컬 컴퓨터에 샘플 앱 리포지토리를 복제합니다.

```
git clone https://github.com/azure-samples/service-fabric-mesh
```

응용 프로그램은 `src\todolistapp` 디렉터리 아래에 있습니다.

## <a name="build-and-debug-on-your-local-cluster"></a>로컬 클러스터에서 빌드 및 디버그

Docker 이미지는 프로젝트가 로드되는 즉시 자동으로 빌드되고 로컬 클러스터에 배포됩니다. 이 프로세스는 시간이 걸릴 수 있습니다. Visual Studio **출력** 창에서 진행률을 모니터링하려면 출력 창 **다음 출력 보기:** 드롭다운을 **Service Fabric 도구**로 설정합니다.

**F5** 키를 눌러 서비스를 로컬로 컴파일하고 실행합니다. 프로젝트를 실행하고 로컬로 디버깅하는 경우 Visual Studio는 다음을 수행합니다.

* Windows용 Docker가 실행 중이며 컨테이너 운영 체제로 Windows를 사용하도록 설정해야 합니다.
* 누락된 Docker 기본 이미지를 다운로드합니다. 이 파트는 다소 시간이 걸릴 수 있습니다.
* 코드 프로젝트를 호스트하는 데 사용되는 Docker 이미지를 빌드(또는 다시 빌드)합니다.
* 로컬 Service Fabric 개발 클러스터에서 컨테이너를 배포하고 실행합니다.
* 서비스를 실행하고 설정한 중단점에 도달합니다.

로컬 배포가 마무리되고 Visual Studio에서 앱을 실행한 후에 기본 샘플 웹 페이지에 대한 브라우저 창이 열립니다.

**디버깅 팁**

* **Service Fabric 로컬 클러스터가 실행되지 않습니다.** 라는 오류가 표시되면 SLCM(Service Local Custer Manager)가 실행되고 있는지 확인하고 작업 표시줄에서 SLCM 아이콘을 마우스 오른쪽 단추로 클릭한 다음, **로컬 클러스터 시작**을 클릭합니다. 작업이 시작되면 Visual Studio로 돌아가서 **F5** 키를 누릅니다.
* 앱이 시작될 때 **404** 오류가 발생하면 아마도 **service.yaml**의 환경 변수가 올바르지 않을 것입니다. `AppName`, `ApiHostPort` 및 `ServiceName`이 [환경 변수 설정](#set-environment-variables)의 지침에 따라 올바르게 설정되어야 합니다.
* **service.yaml**에서 빌드 오류가 발생하면 탭이 아닌 공백을 사용하여 줄을 들여써야 합니다.

### <a name="debug-in-visual-studio"></a>Visual Studio에서 디버그

Visual Studio에서 Service Fabric Mesh 응용 프로그램을 디버깅할 때 로컬 Service Fabric 개발 클러스터를 사용합니다. 백 엔드 서비스에서 할 일 항목을 검색하는 방법을 보려면 OnGet() 메서드를 디버그합니다.
1. **WebFrontEnd** 프로젝트에서 **페이지** > **Index.cshtml** > **Index.cshtml.cs**를 열고 **Get** 메서드(줄 17)에서 중단점을 설정합니다.
2. **ToDoService** 프로젝트에서 **TodoController.cs**를 열고 **OnGet** 메서드(줄 15)에서 중단점을 설정합니다.
3. 브라우저로 돌아가서 페이지를 새로 고칩니다. 웹 프런트 엔드 `OnGet()` 메서드에서 중단점에 도달합니다. `backendUrl` 변수를 검사하여 **service.yaml** 파일에서 정의한 환경 변수를 백 엔드 서비스에 연결하는 데 사용된 URL로 결합하는 방법을 보여줄 수 있습니다.
4. `client.GetAsync(backendUrl).GetAwaiter().GetResult())` 호출(F10)을 넘어서면 컨트롤러의 `Get()` 중단점에 도달합니다. 이 메서드에서는 메모리 내 목록에서 할 일 항목 목록을 검색하는 방법을 볼 수 있습니다.
5. 작업이 완료되면 **shift+F5**를 눌러 Visual Studio에서 프로젝트 디버깅을 중지합니다.
 
## <a name="next-steps"></a>다음 단계

이 자습서 부분에서는 다음에 대해 알아보았습니다.

> [!div class="checklist"]
> * Azure Service Fabric Mesh 응용 프로그램을 빌드할 때 발생하는 결과
> * 서비스 간 호출을 관찰하도록 중단점을 설정하는 방법

다음 자습서를 진행합니다.
> [!div class="nextstepaction"]
> [Service Fabric Mesh 웹 응용 프로그램 게시](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)