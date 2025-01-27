---
title: '빠른 시작: 음성 인식, C++(Linux) - Speech Service'
titleSuffix: Azure Cognitive Services
description: Speech SDK를 사용하여 Linux에서 C++로 음성을 인식하는 방법을 알아봅니다.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: e7838fb2560cb51b2b50bbca0c89c5261df30b56
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606438"
---
# <a name="quickstart-recognize-speech-in-c-on-linux-by-using-the-speech-sdk"></a>빠른 시작: Speech SDK를 사용하여 Linux 기반 C++에서 음성 인식

빠른 시작은 [텍스트 음성 변환](quickstart-text-to-speech-cpp-linux.md)에도 사용할 수 있습니다.

원하는 경우 다른 프로그래밍 언어 및/또는 환경으로 전환합니다.<br/>
[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

이 문서에서는 Linux용(Ubuntu 16.04, Ubuntu 18.04, Debian 9) C++ 콘솔 애플리케이션을 만듭니다. Cognitive Services [Speech SDK](speech-sdk.md)를 사용하여 실시간으로 PC의 마이크로 음성을 텍스트로 변환할 수 있습니다. 애플리케이션은 [Linux용 Speech SDK](https://aka.ms/csspeech/linuxbinary) 및 Linux 배포판의 C++ 컴파일러(예: `g++`)를 사용하여 빌드됩니다.

## <a name="prerequisites"></a>필수 조건

이 빠른 시작을 완료하려면 Speech Services 구독 키가 필요합니다. 무료로 가져올 수 있습니다. 자세한 내용은 [Speech Services를 무료로 체험해보기](get-started.md)를 참조하세요.

## <a name="install-speech-sdk"></a>Speech SDK 설치

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Cognitive Services 음성 SDK의 현재 버전은 `1.6.0`입니다.

Linux용 Speech SDK는 64비트 및 32비트 애플리케이션을 빌드하는 데 사용할 수 있습니다. 필요한 라이브러리 및 헤더 파일은 https://aka.ms/csspeech/linuxbinary 에서 tar 파일로 다운로드할 수 있습니다.

SDK를 다음과 같이 다운로드하고 설치합니다.

1. SDK의 종속 요소가 설치되었는지 확인합니다.

   * Ubuntu에서:

     ```sh
     sudo apt-get update
     sudo apt-get install build-essential libssl1.0.0 libasound2 wget
     ```

   * Debian 9:

     ```sh
     sudo apt-get update
     sudo apt-get install build-essential libssl1.0.2 libasound2 wget
     ```

1. Speech SDK 파일을 추출할 디렉터리를 선택하고, 해당 디렉터리를 가리키도록 `SPEECHSDK_ROOT` 환경 변수를 설정합니다. 이 변수는 이후 명령에서 디렉터리를 쉽게 참조할 수 있게 해줍니다. 예를 들어 홈 디렉터리에서 `speechsdk` 디렉터리를 사용하려는 경우 다음과 비슷한 명령을 사용합니다.

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. 아직 디렉터리가 없으면 디렉터리를 만듭니다.

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. Speech SDK 이진 파일이 들어 있는 `.tar.gz` 아카이브를 다운로드하여 추출합니다.

   ```sh
   wget -O SpeechSDK-Linux.tar.gz https://aka.ms/csspeech/linuxbinary
   tar --strip 1 -xzf SpeechSDK-Linux.tar.gz -C "$SPEECHSDK_ROOT"
   ```

1. 추출된 패키지의 최상위 디렉터리의 내용을 확인합니다.

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   디렉터리 목록에는 헤더(`.h`) 파일을 포함하고 있는 `include` 디렉터리와 라이브러리를 포함하고 있는 `lib` 디렉터리뿐 아니라 타사 통지 및 라이선스 파일이 들어 있습니다.

   [!INCLUDE [Linux Binary Archive Content](../../../includes/cognitive-services-speech-service-linuxbinary-content.md)]

## <a name="add-sample-code"></a>샘플 코드 추가

1. `helloworld.cpp`라는 C++ 원본 파일을 만들고 다음 코드를 파일에 붙여넣습니다.

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-linux/helloworld.cpp#code)]

1. 이 새 파일에서 `YourSubscriptionKey` 문자열을 Speech Services 구독 키로 바꿉니다.

1. 문자열 `YourServiceRegion`을 구독과 연결된 [지역](regions.md)으로 바꿉니다(예를 들어 평가판 구독에 대해 `westus`).

## <a name="build-the-app"></a>앱 빌드

> [!NOTE]
> 아래 명령을 _단일 명령줄_로 입력합니다. 이 작업을 수행하는 가장 쉬운 방법은 각 명령 옆에 있는 **복사** 단추를 누른 다음, 셸 프롬프트에 붙여넣는 것입니다.

* **x64**(64비트) 시스템에서 다음 명령을 실행하여 애플리케이션을 빌드합니다.

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x64" -l:libasound.so.2
  ```

* **x86**(32비트) 시스템에서 다음 명령을 실행하여 애플리케이션을 빌드합니다.

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x86" -l:libasound.so.2
  ```

## <a name="run-the-app"></a>앱 실행

1. Speech SDK 라이브러리를 가리키도록 로더의 라이브러리 경로를 구성합니다.

   * **x64**(64비트) 시스템에서 다음 명령을 입력합니다.

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x64"
     ```

   * **x86**(32비트) 시스템에서 다음 명령을 입력합니다.

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x86"
     ```

1. 애플리케이션을 실행합니다.

   ```sh
   ./helloworld
   ```

1. 아무 말이나 하라는 메시지가 콘솔 창에 나타납니다. 영어 구 또는 문장을 말씀하세요. 음성은 Speech Services로 전송되어 텍스트로 변환되고, 동일한 창에 표시됩니다.

   ```text
   Say something...
   We recognized: What's the weather like?
   ```

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [GitHub에서 C++ 샘플 살펴보기](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>참고 항목

- [음향 모델 사용자 지정](how-to-customize-acoustic-models.md)
- [언어 모델 사용자 지정](how-to-customize-language-model.md)
