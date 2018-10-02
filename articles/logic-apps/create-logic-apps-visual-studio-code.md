---
title: Visual Studio Code를 사용하여 자동화된 워크플로 만들기 및 관리 - Azure Logic Apps | Microsoft Docs
description: VS Code(Visual Studio Code)에서 논리 앱 만들기 및 관리
services: logic-apps
ms.service: logic-apps
ms.workload: azure-vs
author: ecfan
ms.author: estfan
ms.topic: article
ms.reviewer: klam, deli, LADocs
ms.suite: integration
ms.date: 09/24/2018
ms.openlocfilehash: fd92b093b70dba2df3ec645b3ecb77233d3ebd5c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46958359"
---
# <a name="create-and-manage-automated-logic-app-workflows---visual-studio-code"></a>자동화된 논리 앱 워크플로 만들기 및 관리 - Visual Studio Code

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) 및 Visual Studio Code를 사용하여 조직 및 기업 전체에서 앱, 데이터, 시스템 및 서비스를 통합하기 위한 작업, 워크플로 및 프로세스를 자동화하는 데 도움이 되는 논리 앱을 만들고 관리할 수 있습니다. 이 문서에서는 코드 기반 환경에서 작업하여 논리 앱 워크플로 정의를 만들고 편집하는 방법을 보여 줍니다. 클라우드에서 <a href="https://docs.microsoft.com/azure/guides/developer/azure-developer-guide" target="_blank">Azure</a>에 이미 배포된 논리 앱에서 작업할 수도 있습니다. 

<a href="https://portal.azure.com" target="_blank">Azure Portal</a> 및 Visual Studio에서 이러한 동일한 작업을 수행할 수 있지만 코드에서 직접 작업하려면 Visual Studio Code에서 더 빨리 시작할 수 있습니다. 예를 들어 이미 생성된 논리 앱을 사용하도록 설정하고, 사용하지 않도록 설정하고, 삭제하고, 새로 고칠 수도 있습니다.

이 문서의 경우에는 기본 개념에 더 초점을 맞추는 [Azure Portal에 논리 앱을 만들기 위한 빠른 시작](../logic-apps/quickstart-create-first-logic-app-workflow.md)의 경우와 동일한 논리 앱을 만들 수 있습니다. Visual Studio Code에서 논리 앱은 다음 예제와 같이 표시됩니다.

![완료된 논리 앱](./media/create-logic-apps-visual-studio-code/overview.png)

## <a name="prerequisites"></a>필수 조건

시작하기 전에 이러한 항목이 있는지 확인합니다.

* Azure 구독이 없는 경우 <a href="https://azure.microsoft.com/free/" target="_blank">체험 Azure 계정에 등록</a>합니다.

* JSON(JavaScript Object Notation)을 사용하는 논리 앱 [워크플로 정의](../logic-apps/logic-apps-workflow-definition-language.md) 및 해당 구조에 대한 기본 지식.

* Azure 및 Azure 구독에 로그인하기 위해 웹에 액세스.

* 다음 도구가 없으면 다운로드하여 설치합니다. 

  * 체험인 <a href="https://code.visualstudio.com/" target="_blank">Visual Studio Code 버전 1.25.1 이상</a>.

  * Azure Logic Apps용 Visual Studio Code 확장

    Visual Studio Code 내부에서 이 확장을 설치할 수 있습니다. 
    설치한 후 Visual Studio Code를 다시 로드해야 합니다. 

    ![“Azure Logic Apps용 Visual Studio Code 확장” 찾기](./media/create-logic-apps-visual-studio-code/find-install-logic-apps-extension.png)

    확장이 올바르게 설치되었는지 확인하기 위해 Visual Studio Code 도구 모음에 Azure 아이콘이 표시됩니다. 

    ![설치된 확장](./media/create-logic-apps-visual-studio-code/installed-extension.png)

    자세한 내용은 <a href="https://code.visualstudio.com/docs/editor/extension-gallery" target="_blank">확장 마켓플레이스</a>를 참조하세요. 

<a name="sign-in-azure"></a>

## <a name="sign-in-to-azure"></a>Azure에 로그인

1. Visual Studio Code를 엽니다. Visual Studio Code 도구 모음에서 Azure 아이콘을 선택합니다. 

   ![Azure 아이콘 선택](./media/create-logic-apps-visual-studio-code/open-extension.png)

1. Azure 창의 **Logic Apps** 아래에서 **Azure에 로그인**을 선택합니다. 

   ![“Azure에 로그인” 선택](./media/create-logic-apps-visual-studio-code/sign-in-azure.png)

   이제 제공된 인증 코드를 사용하여 로그인하라는 메시지가 표시됩니다. 

1. 인증 코드를 복사한 후 **복사 및 열기**를 선택하면 새 브라우저 창이 열립니다.

   ![로그인 프롬프트](./media/create-logic-apps-visual-studio-code/sign-in-prompt.png)

1. 인증 코드를 입력합니다. 메시지가 표시되면 **계속**을 선택합니다.

   ![코드 입력](./media/create-logic-apps-visual-studio-code/authentication-code.png)

1. Azure 계정을 선택합니다. 로그인한 후 브라우저를 닫고 Visual Studio Code로 돌아갈 수 있습니다.

   이제 Azure 창의 [Logic Apps] 창과 [통합 계정] 창에는 해당 계정의 Azure 구독이 표시됩니다. 

   ![구독 선택](./media/create-logic-apps-visual-studio-code/select-azure-subscription.png)

   예상한 구독이 표시되지 않으면 **Logic Apps** 레이블 옆에 있는 **구독 선택**(필터 아이콘)을 선택합니다. 원하는 구독을 찾아 선택합니다.

1. Azure 구독에서 기존 논리 앱 또는 통합 계정을 보려면 구독을 확장합니다.

   ![논리 앱 및 통합 계정 보기](./media/create-logic-apps-visual-studio-code/existing-logic-apps.png)

<a name="create-logic-app"></a>

## <a name="create-logic-app"></a>논리 앱 만들기

1. Visual Studio Code 내부에서 Azure 구독에 로그인하지 않은 경우에는 이 문서의 단계에 따라 [지금 로그인](#sign-in-azure)하세요.

1. 구독의 상황에 맞는 메뉴에서 **만들기**를 선택합니다.

   ![“만들기” 선택](./media/create-logic-apps-visual-studio-code/create-logic-app.png)

1. 구독의 Azure 리소스 그룹을 보여 주는 목록에서 기존 리소스 그룹 또는 **새 리소스 그룹 만들기**를 선택합니다. 

   이 예제는 새 리소스 그룹을 만듭니다.

   ![새 리소스 그룹 만들기](./media/create-logic-apps-visual-studio-code/select-or-create-azure-resource-group.png)

1. Azure 리소스 그룹의 이름을 입력한 후 Enter 키를 누릅니다.

   ![리소스 그룹 이름 지정](./media/create-logic-apps-visual-studio-code/enter-name-resource-group.png)

1. 논리 앱의 메타데이터를 저장할 데이터 센터 위치를 선택합니다.

   ![위치 선택](./media/create-logic-apps-visual-studio-code/select-location.png)

1. 논리 앱의 이름을 입력한 후 Enter 키를 누릅니다.

   ![논리 앱 이름 지정](./media/create-logic-apps-visual-studio-code/enter-name-logic-app.png)

   이제 Azure 창의 Azure 구독 아래에 새 논리 앱이 나타납니다. 이제 논리 앱의 워크플로 정의 만들기를 시작할 수 있습니다.

1. 논리 앱의 바로 가기 메뉴에서 **편집기에서 열기**를 선택합니다. 

   ![편집기에서 논리 앱 열기](./media/create-logic-apps-visual-studio-code/open-new-logic-app.png)

   Visual Studio Code에서 논리 앱 워크플로 정의 템플릿(.logicapp.json 파일)이 열리므로 논리 앱의 워크플로 만들기를 시작할 수 있습니다.

   ![새 논리 앱 워크플로 정의](./media/create-logic-apps-visual-studio-code/blank-logic-app-workflow-definition.png)

1. 논리 앱 워크플로 정의 템플릿 파일에서 논리 앱의 워크플로 정의 빌드를 시작합니다. 기술 참조를 보려면 [Azure Logic Apps에 대한 워크플로 정의 언어 스키마](../logic-apps/logic-apps-workflow-definition-language.md)를 참조하세요.

   다음은 예제 논리 정의입니다. 일반적으로 JSON 요소는 각 섹션 내에 사전순으로 표시되지만, 이 샘플은 논리 앱의 단계가 디자이너에 나타나는 유사한 순서로 이러한 요소를 표시합니다.

   ```json
   {
      "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
         "$connections": {
            "defaultValue": {},
            "type": "Object"
         }
      },
      "triggers": {
         "When_a_feed_item_is_published": {
            "recurrence": {
               "frequency": "Minute",
               "interval": 1
            },
            "splitOn": "@triggerBody()?['value']",
            "type": "ApiConnection",
            "inputs": {
               "host": {
                  "connection": {
                     "name": "@parameters('$connections')['rss']['connectionId']"
                  }
               },
               "method": "get",
               "path": "/OnNewFeed",
               "queries": {
                  "feedUrl": "http://feeds.reuters.com/reuters/topNews"
               }
            }
         }
      },
      "actions": {
         "Send_an_email": {
            "runAfter": {},
            "type": "ApiConnection",
            "inputs": {
               "body": {
                  "Body": "Title: @{triggerBody()?['title']}\n\nDate published: @{triggerBody()?['publishDate']}\n\nLink: @{triggerBody()?['primaryLink']}",
                  "Subject": "New RSS item: @{triggerBody()?['title']}",
                  "To": "Sophie.Owen@contoso.com"
               },
               "host": {
                  "connection": {
                     "name": "@parameters('$connections')['outlook']['connectionId']"
                  }
               },
               "method": "post",
               "path": "/Mail"
            }
         }
      },
      "outputs": {}
   }   
   ```

1. 완료되면 논리 앱 정의 파일을 저장합니다. Visual Studio Code에서 Azure 구독에 논리 앱 정의를 업로드할지 확인하는 메시지를 표시하면 **업로드**를 선택합니다.

   ![새 논리 앱 업로드](./media/create-logic-apps-visual-studio-code/upload-new-logic-app.png)

   Visual Studio Code가 Azure에 논리 앱을 게시한 후 이제 앱이 Azure Portal에서 실시간으로 실행되고 있음을 알 수 있습니다. 

   ![Azure Portal에 게시된 논리 앱](./media/create-logic-apps-visual-studio-code/published-logic-app.png)

<a name="edit-logic-app"></a>

## <a name="edit-logic-app"></a>논리 앱 편집

Azure에 이미 배포된 기존 논리 앱에서 작업하려면 Visual Studio Code에서 해당 앱의 워크플로 정의 파일을 열 수 있습니다.

1. Visual Studio Code 내부에서 Azure 구독에 로그인하지 않은 경우에는 이 문서의 단계에 따라 [지금 로그인](#sign-in-azure)하세요.

1. Azure 창의 **Logic Apps** 아래에서 Azure 구독을 확장하고 원하는 논리 앱을 선택합니다. 

1. 논리 앱의 메뉴에서 **편집기에서 열기**를 선택합니다. 또는 논리 앱 이름 옆에 있는 편집 아이콘을 선택합니다.

   ![기존 논리 앱에 대한 편집기 열기](./media/create-logic-apps-visual-studio-code/open-editor-existing-logic-app.png)

   Visual Studio Code가 논리 앱의 워크플로 정의에 대한 .logicapp.json 파일을 엽니다.

   ![열린 논리 앱 워크플로 정의](./media/create-logic-apps-visual-studio-code/edit-logic-app-workflow-definition-file.png)

1. 논리 앱의 정의의 내용을 변경합니다.

1. 완료되면 변경 사항을 저장합니다.

1. Visual Studio Code가 Azure 구독에서 논리 앱 정의를 업데이트하라는 메시지를 표시하면 **업로드**를 선택합니다. 

   ![편집 내용 업로드](./media/create-logic-apps-visual-studio-code/upload-logic-app-changes.png)

## <a name="get-support"></a>지원 받기

* 질문이 있는 경우 <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">Azure Logic Apps 포럼</a>을 방문해 보세요.
* 기능 아이디어를 제출하거나 투표하려면 <a href="http://aka.ms/logicapps-wish" target="_blank">Logic Apps 사용자 의견 사이트</a>를 방문하세요.
