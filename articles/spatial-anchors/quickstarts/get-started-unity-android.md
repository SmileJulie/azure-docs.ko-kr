---
title: 빠른 시작 - Azure Spatial Anchors를 사용하여 Android Unity 앱 만들기 | Microsoft Docs
description: 이 빠른 시작에서는 Spatial Anchors를 사용하여 Unity 지원 Android 앱을 빌드하는 방법을 알아봅니다.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: e92c812ffc8b72fe79248c602e48ff01ef9fefcb
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56961017"
---
# <a name="quickstart-create-an-android-unity-app-with-azure-spatial-anchors"></a>빠른 시작: Azure Spatial Anchors를 사용하여 Android Unity 앱 만들기

이 빠른 시작에서는 [Azure Spatial Anchors](../overview.md)를 사용하여 Android Unity 앱을 만드는 방법을 다룹니다. Azure Spatial Anchors는 시간이 지남에 따라 디바이스에서 위치를 유지하는 객체를 사용하여 혼합 현실 환경을 만들 수 있는 플랫폼 간 개발자 서비스입니다. 완료되면, Unity 지원 ARCore Android 앱이 있어 공간 앵커를 저장하고 회수할 수 있습니다.

이 문서에서 배울 내용은 다음과 같습니다.

> [!div class="checklist"]
> * Spatial Anchors 계정 만들기
> * Unity 빌드 설정 준비
> * Unity용 ARCore SDK 다운로드 및 가져오기
> * Spatial Anchors 계정 식별자 및 계정 키 구성
> * Android Studio 프로젝트 내보내기
> * Android 디바이스 배포 및 실행

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>필수 조건

이 빠른 시작을 완료하려면 다음 항목이 있어야 합니다.

- <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2018.3+</a> 및 <a href="https://developer.android.com/studio/" target="_blank">Android Studio 3.3+</a>가 설치된 Windows 또는 macOS 머신
- <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">개발자 사용</a> 및 <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">ARCore 지원</a> Android 디바이스

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project-in-unity"></a>Unity에서 샘플 프로젝트 열기

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Android Unity Build Settings](../../../includes/spatial-anchors-unity-android-build-settings.md)]

## <a name="configure-account-identifier-and-key"></a>계정 식별자 및 키 구성

**프로젝트** 창에서 `Assets/AzureSpatialAnchorsPlugin/Examples`로 이동하여 `AzureSpatialAnchorsBasicDemo.unity` 장면 파일을 엽니다.

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

**파일** -> **저장**을 선택하여 장면을 저장합니다.

## <a name="export-the-android-studio-project"></a>Android Studio 프로젝트 내보내기

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

**내보내기**를 선택하여 대화 상자를 엽니다. 그런 다음, Android Studio 프로젝트를 내보낼 폴더를 선택합니다.

내보내기가 완료되면 **HelloAR U3D** 하위 폴더가 있는 내보낸 Android Studio 프로젝트를 포함한 폴더가 표시됩니다.

## <a name="deploy-the-android-application"></a>Android 애플리케이션 배포

Android Studio를 열고 **기존 Android Studio 프로젝트 열기**를 선택합니다. 그런 다음, 내보낸 Android Studio 프로젝트에서 **HelloAR U3D** 하위 폴더를 선택하고 **확인**을 클릭합니다.

열면 Gradle 래퍼를 사용할지 묻는 프롬프트가 나타납니다. **확인**을 선택하여 Gradle 래퍼를 사용하고 프로젝트를 엽니다.

Android 디바이스의 전원을 켜고 로그인한 후 USB 케이블을 사용해 PC에 연결합니다.

Android Studio 도구 모음에서 **실행**을 선택합니다.

![Android Studio 배포 및 실행](./media/get-started-unity-android/android-studio-deploy-run.png)

**배포 대상 선택** 대화 상자에서 Android 디바이스를 선택하고 **확인**을 선택하여 Android 디바이스에서 앱을 실행합니다.

앱의 지침에 따라 앵커를 배치하고 회수합니다.

> [!NOTE]
> 앱 실행 시 백그라운드로 카메라가 보이지 않으면(대신 공백, 파랑 또는 기타 질감 등이 보이면) Unity에서 자산을 다시 가져와야 할 가능성이 높습니다. 앱을 중지합니다. Unity의 맨 위 메뉴에서 **자산 -> 모두 다시 가져오기**를 선택합니다. 그런 다음, 앱을 다시 실행합니다.

Android Studio 도구 모음에서 **중지**를 선택하여 앱을 중지합니다.

![Android Studio 중지](./media/get-started-unity-android/android-studio-stop.png)

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [자습서: 여러 디바이스에서 Spatial Anchors 공유](../tutorials/tutorial-share-anchors-across-devices.md)