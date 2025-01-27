---
title: 음성 컨테이너를 설치 합니다.
titleSuffix: Azure Cognitive Services
description: 음성 컨테이너를 설치하고 실행합니다. 음성 텍스트 변환은 오디오 스트림을 애플리케이션, 도구 또는 디바이스가 사용하거나 표시할 수 있는 텍스트로 실시간으로 기록합니다. 텍스트 음성 변환은 입력 텍스트를 인간과 유사한 합성 음성으로 변환합니다.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: dapine
ms.openlocfilehash: 8f395788d4dd3c845155a52bd6b4666998838fcd
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490228"
---
# <a name="install-and-run-speech-service-containers"></a>음성 서비스 컨테이너 설치 및 실행


음성 컨테이너를 통해 고객은 강력한 클라우드 기능 및 에지 위치를 활용하기 위해 최적화된 단일 음성 응용 프로그램 아키텍처를 구축할 수 있습니다.


두 음성 컨테이너는 **음성 텍스트 변환**과 **텍스트 음성 변환**입니다. 

|함수|기능|최신 버전|
|-|-|--|

|음성 텍스트 변환| <li>연속 실시간 음성 또는 일괄 처리 오디오 녹음/녹화를 중간 결과 텍스트로 기록합니다.|1.1.3|
|텍스트 음성 변환| <li>일반 텍스트 입력 또는 SSML(Speech Synthesis Markup Language)을 사용하여 텍스트를 자연스럽게 들리는 음성으로 변환합니다. |1.1.0|


Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)을 만듭니다.

## <a name="prerequisites"></a>필수 조건

음성 컨테이너를 사용하기 전에 다음 필수 조건을 충족해야 합니다.

|필수|목적|
|--|--|
|Docker 엔진| [호스트 컴퓨터](#the-host-computer)에 설치된 Docker 엔진이 필요합니다. Docker는 [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) 및 [Linux](https://docs.docker.com/engine/installation/#supported-platforms)에서 Docker 환경을 구성하는 패키지를 제공합니다. Docker 및 컨테이너에 대한 기본 사항은 [Docker 개요](https://docs.docker.com/engine/docker-overview/)를 참조하세요.<br><br> Docker는 컨테이너에서 Azure에 연결하여 청구 데이터를 보낼 수 있도록 구성해야 합니다. <br><br> **Windows**에서 Docker는 Linux 컨테이너를 지원하도록 구성해야 합니다.<br><br>|
|Docker 사용 경험 | 기본 `docker`명령에 대한 지식뿐만 아니라 레지스트리, 리포지토리, 컨테이너 및 컨테이너 이미지와 같은 Docker 개념에 대해 기본적으로 이해해야 합니다.| 
|음성 리소스 |이러한 컨테이너를 사용하려면 다음이 있어야 합니다.<br><br>연결된 청구 키 및 청구 끝점 URI를 가져올 수 있는 _음성_ Azure 리소스. 두 값은 Azure portal의 **음성** 개요 및 키 페이지에서 사용할 수 있으며 컨테이너를 시작하기 위해 필요합니다.<br><br>**{BILLING_KEY}** : 리소스 키<br><br>**{BILLING_ENDPOINT_URI}** : 끝점 URI 예제: `https://westus.api.cognitive.microsoft.com/sts/v1.0`|

## <a name="request-access-to-the-container-registry"></a>컨테이너 레지스트리에 대한 액세스 요청


컨테이너에 액세스를 요청하려면 먼저 [Cognitive Services 음성 컨테이너 요청 양식](https://aka.ms/speechcontainerspreview/)을 작성하여 제출합니다.


[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

## <a name="the-host-computer"></a>호스트 컴퓨터

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="advanced-vector-extension-support"></a>고급 벡터 확장 지원


**호스트**는 Docker 컨테이너를 실행하는 컴퓨터입니다. 호스트는 [Advanced Vector Extensions](https://en.wikipedia.org/wiki/Advanced_Vector_Extensions#CPUs_with_AVX2)(AVX2)를 지원해야 합니다. 다음 명령을 사용하여 Linux 호스트에서의 이 지원을 확인할 수 있습니다.


```console
grep -q avx2 /proc/cpuinfo && echo AVX2 supported || echo No AVX2 support detected
```

### <a name="container-requirements-and-recommendations"></a>컨테이너 요구 사항 및 추천

다음 표는 각 음성 컨테이너에 할당하기 위한 최소 및 권장되는 CPU 코어 및 메모리를 설명합니다.

| 컨테이너 | 최소 | 권장 |
|-----------|---------|-------------|
|cognitive-services-speech-to-text | 2 코어<br>2GB 메모리  | 4 코어<br>4GB 메모리  |
|cognitive-services-text-to-speech | 1 코어, 0.5GB 메모리| 2 코어 및 1GB 메모리 |

* 각 코어는 속도가 2.6GHz 이상이어야 합니다.

코어 및 메모리는 `docker run` 명령의 일부로 사용되는 `--cpus` 및 `--memory` 설정에 해당합니다.

코어 및 메모리는 `docker run` 명령의 일부로 사용되는 `--cpus` 및 `--memory` 설정에 해당합니다.

**참고**: 최소 및 권장 사항은 Docker 제한 기반을 두고 있으며 호스트 컴퓨터 리소스가 *아닙니다*. 예를 들어, 음성-텍스트 컨테이너는 큰 언어 모델의 맵 부분을 기억하며 추가로 4 ~ 6GB의 메모리에 전체 파일을 두는 것을 _권장_ 합니다. 또한 모델은 메모리로 페이징되므로 두 컨테이너의 첫 번째 실행은 더 오래 걸릴 수 있습니다.

## <a name="get-the-container-image-with-docker-pull"></a>`docker pull`을 사용하여 컨테이너 이미지 가져오기

음성에 대한 컨테이너 이미지를 사용할 수 있습니다. 


| 컨테이너 | 리포지토리 |
|-----------|------------|
| cognitive-services-speech-to-text | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text:latest` |
| cognitive-services-text-to-speech | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech:latest` |

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="language-locale-is-in-container-tag"></a>컨테이너 태그의 언어 로캘


`latest` 태그는 `en-us` 로캘 및 `jessarus` 음성을 끌어오기 합니다. 


#### <a name="speech-to-text-locales"></a>텍스트 음성 변환 로캘

`latest`를 제외한 모든 태그는 `<culture>`가 로캘을 나타내고 `<voice>`가 컨테이너의 음성을 나타내는 다음 형식으로 되어 있습니다.

```
<major>.<minor>.<patch>-<platform>-<culture>-<prerelease>
```

다음 태그는 해당 형식의 예입니다.

```
1.1.3-amd64-en-us-preview
```


다음 표는 컨테이너의 버전 1.1.1에서 **음성 텍스트 변환**에 대해 지원되는 로캘을 나열합니다.


|언어 로캘|태그들|
|--|--|
|중국어|`zh-cn`|
|영어 |`en-us`<br>`en-gb`<br>`en-au`<br>`en-in`|
|프랑스어 |`fr-ca`<br>`fr-fr`|
|독일어|`de-de`|
|이탈리아어|`it-it`|
|일본어|`ja-jp`|
|한국어|`ko-kr`|
|포르투갈어|`pt-br`|
|스페인어|`es-es`<br>`es-mx`|

#### <a name="text-to-speech-locales"></a>텍스트 음성 변환 로캘


`latest`를 제외한 모든 태그는 로캘 컨테이너를 나타내는 `<culture>`와 컨테이너의 음성을 나타내는 `<voice>`가 있는 다음 형식입니다.


```
<major>.<minor>.<patch>-<platform>-<culture>-<voice>-<prerelease>
```

다음 태그는 해당 형식의 예입니다.

```
1.1.0-amd64-en-us-jessarus-preview
```

다음 표는 컨테이너의 버전 1.1.0에서 **텍스트 음성 변환**에 대해 지원되는 로캘을 나열합니다.

|언어 로캘|태그들|지원 되는 음성|
|--|--|--|
|중국어|`zh-cn`|huihuirus<br>kangkang apollo<br>yaoyao apollo|
|영어 |`en-au`|catherine<br>hayleyrus|
|영어 |`en-gb`|george-apollo<br>hazelrus<br>susan apollo|
|영어 |`en-in`|heera apollo<br>priyarus<br>ravi apollo<br>|
|영어 |`en-us`|jessarus<br>benjaminrus<br>jessa24krus<br>zirarus<br>guy24krus|
|프랑스어|`fr-ca`|caroline<br>harmonierus|
|프랑스어|`fr-fr`|hortenserus<br>julie apollo<br>paul-apollo|
|독일어|`de-de`|hedda<br>heddarus<br>stefan apollo|
|이탈리아어|`it-it`|cosimo-apollo<br>luciarus|
|일본어|`ja-jp`|ayumi apollo<br>harukarus<br>ichiro apollo|
|한국어|`ko-kr`|heamirus|
|포르투갈어|`pt-br`|daniel-apollo<br>heloisarus|
|스페인어|`es-es`|elenarus<br>laura apollo<br>pablo-apollo<br>|
|스페인어|`es-mx`|hildarus<br>raul-apollo|

### <a name="docker-pull-for-the-speech-containers"></a>음성 컨테이너용 Docker 풀

#### <a name="speech-to-text"></a>음성 텍스트 변환

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text:latest
```

#### <a name="text-to-speech"></a>텍스트 음성 변환

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech:latest
```

## <a name="how-to-use-the-container"></a>컨테이너 사용 방법

컨테이너가 [호스트 컴퓨터](#the-host-computer)에 있으면 다음 프로세스를 사용하여 컨테이너 작업을 수행합니다.

1. 필수이지만 사용되지 않는 청구 설정을 사용하여 [컨테이너를 실행](#run-the-container-with-docker-run)합니다. `docker run` 명령의 자세한 [예제](speech-container-configuration.md#example-docker-run-commands)를 사용할 수 있습니다.
1. [컨테이너의 예측 끝점을 쿼리합니다](#query-the-containers-prediction-endpoint).

## <a name="run-the-container-with-docker-run"></a>`docker run`을 사용하여 컨테이너 실행

[docker run](https://docs.docker.com/engine/reference/commandline/run/) 명령을 사용하여 세 컨테이너 중 하나를 실행합니다. 명령은 다음 매개 변수를 사용합니다.

**미리 보기 동안**, 컨테이너를 시작하려면 청구 설정이 유효해야 합니다. 하지만 사용에 대한 요금은 청구되지 않습니다.

| 자리표시자 | 값 |
|-------------|-------|

|{BILLING_KEY} | 이 키는 컨테이너를 시작하는 데 사용되고 Azure Portal의 음성 키 페이지에서 확인할 수 있습니다. |

|{BILLING_ENDPOINT_URI} | 청구 끝점 URI 값은 Azure portal의 음성 개요 페이지에서 사용할 수 있습니다.|

다음 예제 `docker run` 명령에서 해당 매개 변수를 사용자 고유의 값으로 바꿉니다.

### <a name="text-to-speech"></a>텍스트 음성 변환

```bash
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

### <a name="speech-to-text"></a>음성 텍스트 변환

```bash
docker run --rm -it -p 5000:5000 --memory 2g --cpus 2 \
containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

이 명령은 다음을 수행합니다.

* 컨테이너 이미지에서 음성 컨테이너를 실행합니다.
* 2 CPU 코어 및 2GB(기가바이트) 메모리를 할당합니다.
* TCP 5000 포트를 공개하고 컨테이너에 의사-TTY를 할당합니다.
* 종료 후 자동으로 컨테이너를 제거합니다. 컨테이너 이미지는 호스트 컴퓨터에서 계속 사용할 수 있습니다.

> [!IMPORTANT]
> 컨테이너를 실행하려면 `Eula`, `Billing` 및 `ApiKey` 옵션을 지정해야 합니다. 그렇지 않으면 컨테이너가 시작되지 않습니다. 자세한 내용은 [Billing](#billing)을 참조하세요.

## <a name="query-the-containers-prediction-endpoint"></a>컨테이너의 예측 엔드포인트 쿼리

|컨테이너|엔드포인트|
|--|--|
|음성 텍스트 변환|ws://localhost:5000/speech/recognition/dictation/cognitiveservices/v1|
|텍스트 음성 변환|http://localhost:5000/speech/synthesize/cognitiveservices/v1|

### <a name="speech-to-text"></a>음성 텍스트 변환

컨테이너는 [Speech SDK](index.yml)를 통해 액세스되는 Websocket 기반 쿼리 끝점 API를 제공합니다.

기본적으로 Speech SDK는 온라인 음성 서비스를 사용합니다. 컨테이너를 사용하려면 초기화 메서드를 변경해야 합니다. 아래 예제를 참조하세요.

#### <a name="for-c"></a>C#의 경우

다음 Azure 클라우드 초기화 호출을 

```C#
var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

컨테이너 엔드포인트를 사용하는 다음 호출로 변경합니다.

```C#
var config = SpeechConfig.FromEndpoint(
    new Uri("ws://localhost:5000/speech/recognition/dictation/cognitiveservices/v1"),
    "YourSubscriptionKey");
```

#### <a name="for-python"></a>Python의 경우

다음 Azure 클라우드 초기화 호출을

```python
speech_config = speechsdk.SpeechConfig(
    subscription=speech_key, region=service_region)
```

컨테이너 엔드포인트를 사용하는 다음 호출로 변경합니다.

```python
speech_config = speechsdk.SpeechConfig(
    subscription=speech_key, endpoint="ws://localhost:5000/speech/recognition/dictation/cognitiveservices/v1")
```

### <a name="text-to-speech"></a>텍스트 음성 변환

컨테이너는 [여기](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-text-to-speech)에서 찾을 수 있는 REST 끝점을 제공하며 샘플은 [여기](https://azure.microsoft.com/resources/samples/cognitive-speech-tts/)에서 찾을 수 있습니다.


[!INCLUDE [Validate container is running - Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>컨테이너 중지

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>문제 해결


컨테이너를 실행할 때, 컨테이너는 **stdout** 및 **stderr**을 사용하여 컨테이너를 시작하거나 실행하는 동안 발생하는 문제를 해결하는 데 도움이 되는 정보를 출력합니다. 


## <a name="billing"></a>결제

음성 컨테이너는 Azure 계정의 _음성_ 리소스를 사용하여 청구 정보를 Azure로 보냅니다.

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

이러한 옵션에 대한 자세한 내용은 [컨테이너 구성](speech-container-configuration.md)을 참조하세요.

<!--blogs/samples/video coures -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>요약

이 문서에서는 음성 컨테이너 다운로드, 설치 및 실행에 대한 개념 및 워크플로를 알아보았습니다. 요약하면 다음과 같습니다.

* 음성은 음성을 텍스트로, 텍스트를 음성으로 캡슐화하는 두 개의 Linux 컨테이너 Docker를 제공합니다.
* 컨테이너 이미지는 Azure의 프라이빗 컨테이너 레지스트리에서 다운로드됩니다.
* 컨테이너 이미지는 Docker에서 실행됩니다.
* 음성 컨테이너에서 작업을 호출하기 위해 컨테이너의 호스트 URI를 지정하여 REST API 또는 SDK 중 하나를 사용할 수 있습니다.
* 컨테이너를 인스턴스화할 때 청구 정보를 지정해야 합니다.

> [!IMPORTANT]
>  Cognitive Services 컨테이너는 계량을 위한 청구 끝점에 연결되지 않은 상태에서 실행할 수 있는 권한이 없습니다. 고객은 컨테이너를 항상 청구 끝점과 청구 정보를 통신할 수 있도록 설정해야 합니다. Cognitive Services 컨테이너는 고객 데이터(예: 분석 중인 이미지 또는 텍스트)를 Microsoft에 보내지 않습니다.

## <a name="next-steps"></a>다음 단계

* [컨테이너 구성](speech-container-configuration.md)에서 구성 설정을 검토합니다.
* 추가로 [Cognitive Services 컨테이너](../cognitive-services-container-support.md)를 사용합니다.
