---
title: Node.js 및 VS Code를 사용하여 여러 종속 서비스 실행
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 11/21/2018
ms.topic: tutorial
description: Azure에서 컨테이너 및 마이크로 서비스를 통한 신속한 Kubernetes 개발
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, 컨테이너, Helm, 서비스 메시, 서비스 메시 라우팅, kubectl, k8s
ms.openlocfilehash: 136d48f1c420ac71896eaafa0daab476c7fba6fa
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67503066"
---
# <a name="multi-service-development-with-azure-dev-spaces"></a>Azure Dev Spaces로 다중 서비스 개발

이 자습서에서는 Azure Dev Spaces를 사용하여 다중 서비스 애플리케이션을 개발하는 방법과 Dev Spaces가 제공하는 추가적인 이점 몇 가지를 알아봅니다.

## <a name="call-a-service-running-in-a-separate-container"></a>별도의 컨테이너에서 실행되는 서비스 호출

이 섹션에서는 두 번째 서비스인 `mywebapi`를 만들고 `webfrontend`에서 이 서비스를 호출하도록 합니다. 각 서비스는 별도의 컨테이너에서 실행됩니다. 그런 다음, 두 컨테이너 모두에서 디버그합니다.

![](media/common/multi-container.png)

### <a name="open-sample-code-for-mywebapi"></a>*mywebapi* 샘플 코드 열기
이 가이드에서 사용할 `mywebapi` 샘플 코드가 이미 `samples` 폴더에 있어야 합니다. 그렇지 않으면 https://github.com/Azure/dev-spaces 로 이동하고 **복제 또는 다운로드**를 선택하여 GitHub 리포지토리를 다운로드합니다. 이 섹션에서 사용할 코드는 `samples/nodejs/getting-started/mywebapi` 에 있습니다.

### <a name="run-mywebapi"></a>*mywebapi* 실행
1. *별도의 VS Code 창*에서 `mywebapi` 폴더를 엽니다.
1. **명령 팔레트**(**보기 | 명령 팔레트** 메뉴를 사용하여)를 열고, 자동 완성을 사용하여 입력하고 이 명령을 선택합니다. `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces` 이 명령은 배포 프로젝트를 구성하는 `azds prep` 명령과 혼동하면 안됩니다.
1. F5 키를 누르고, 서비스가 빌드되고 배포될 때까지 기다립니다. *포트 80에서 수신 대기 중* 메시지가 디버그 콘솔에 나타나면 준비가 된 것입니다.
1. 엔드포인트 URL을 적어둡니다. 이는 `http://localhost:<portnumber>`과 비슷합니다. **팁: VS Code 상태 표시줄이 주황색으로 바뀌고 클릭 가능한 URL이 표시됩니다.** 컨테이너가 로컬로 실행되는 것처럼 보이지만, 실제로는 Azure의 개발 환경에서 실행되고 있습니다. localhost 주소를 사용하는 이유는 `mywebapi`에서 공용 엔드포인트를 정의하지 않았고 이로 인해 Kubernetes 인스턴스 내에서만 액세스할 수 있기 때문입니다. 편의상 로컬 컴퓨터에서 프라이빗 서비스와 쉽게 상호 작용할 수 있도록 Azure Dev Spaces는 Azure에서 실행되는 컨테이너에 대한 임시 SSH 터널을 만듭니다.
1. `mywebapi`에서 준비가 되면 브라우저를 localhost 주소로 엽니다. `mywebapi` 서비스의 응답("Hello from mywebapi(mywebapi에서 보낸 Hello)")이 표시됩니다.


### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>*webfrontend*에서 *mywebapi*로 요청
이제 `mywebapi`에 요청하는 코드를 `webfrontend`에 작성해 보겠습니다.
1. `webfrontend`에 대한 VS Code 창으로 전환합니다.
1. `server.js`의 맨 위에 다음 코드 줄을 추가합니다.
    ```javascript
    var request = require('request');
    ```

3. *GET 처리기에 대한 코드를*바꿉니다`/api`. 요청을 처리할 때는 `mywebapi`를 차례로 호출한 다음, 두 서비스의 결과를 반환합니다.

    ```javascript
    app.get('/api', function (req, res) {
       request({
          uri: 'http://mywebapi',
          headers: {
             /* propagate the dev space routing header */
             'azds-route-as': req.headers['azds-route-as']
          }
       }, function (error, response, body) {
           res.send('Hello from webfrontend and ' + body);
       });
    });
    ```
   1. `server.js`의 끝에서 `server.close()` 줄을 *제거*합니다.

위의 코드 예제는 `azds-route-as` 헤더를 수신 요청에서 발신 요청으로 전달합니다. 나중에 이를 통해 공동 작업 개발 팀에 어떻게 도움이 되는지 살펴보겠습니다.

### <a name="debug-across-multiple-services"></a>여러 서비스에서 디버깅
1. 이 시점에서 `mywebapi`는 디버거가 연결된 상태로 계속 실행되고 있습니다. 그렇지 않으면 `mywebapi` 프로젝트에서 F5 키를 누릅니다.
1. [`server.js`의 줄 8에서](https://github.com/Azure/dev-spaces/blob/master/samples/nodejs/getting-started/mywebapi/server.js#L8) 기본 GET `/` 처리기 내에 중단점을 설정합니다.
1. `webfrontend` 프로젝트에서 GET 요청을 `http://mywebapi`로 보내기 바로 전에 중단점을 설정합니다.
1. `webfrontend` 프로젝트에서 F5 키를 누릅니다.
1. 웹앱을 열고 두 서비스에서 코드를 단계별로 실행합니다. 웹앱에 두 서비스가 연결된 "Hello from webfrontend and Hello from mywebapi." 메시지가 표시됩니다.

### <a name="well-done"></a>모두 완료되었습니다!
이제 각 컨테이너를 개별적으로 개발하고 배포할 수 있는 다중 컨테이너 애플리케이션이 있습니다.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [Dev Spaces의 팀 개발에 대해 알아보기](team-development-nodejs.md)
