---
title: 포함 파일
description: 포함 파일
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 03/11/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 7e047ba57a61d2f327544ec795f640f5066962f6
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509884"
---
1. [Firebase 콘솔](https://firebase.google.com/console/)에 로그인합니다. 아직 없는 경우 새 Firebase 프로젝트를 만듭니다.
2. 프로젝트를 만든 후 **Android 앱에 Firebase 추가**를 선택합니다. 

    ![Android 앱에 Firebase 추가](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)

3. **Android 앱에 Firebase 추가** 페이지에서 다음 단계를 수행합니다. 
1. 
    1. **Android 패키지 이름**에는 패키지의 이름을 입력합니다. 예: `tutorials.tutoria1.xamarinfcmapp` 

        ![패키지 이름 지정](./media/notification-hubs-enable-firebase-cloud-messaging/specify-package-name-fcm-settings.png)

2. **앱 등록**을 선택합니다. 
1. 
1. **google-services.json 다운로드**를 선택합니다. 그런 다음 프로젝트의 **app** 폴더에 파일을 저장하고 **다음**을 선택합니다. 

    ![google-services.json 다운로드](./media/notification-hubs-enable-firebase-cloud-messaging/download-google-service-button.png)

6. **다음**을 선택합니다. 
7. **이 단계 건너뛰기**를 선택합니다. 

    ![마지막 단계 건너뛰기](./media/notification-hubs-enable-firebase-cloud-messaging/skip-this-step.png)

8. Firebase 콘솔에서 프로젝트의 톱니바퀴를 선택합니다. 그런 후 **프로젝트 설정**을 선택합니다.

    ![프로젝트 설정 선택](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)

4. **google-services.json** 파일을 다운로드하지 않은 경우 이 페이지에서 다운로드할 수 있습니다. 

1. 맨 위에 있는 **클라우드 메시징** 탭으로 전환합니다. 

1. 나중에 사용하기 위해 **레거시 서버 키**를 복사하고 저장합니다. 이 값은 알림 허브를 구성하는 데 사용됩니다.
