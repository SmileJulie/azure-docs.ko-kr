---
title: QnA Maker 서비스 업그레이드 - QnA Maker
titleSuffix: Azure Cognitive Services
description: 공유 하거나 더 나은 리소스를 관리 하려면 QnA Maker 서비스를 업그레이드 합니다.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 03/25/2019
ms.author: diberry
ms.openlocfilehash: 2fdbb245f838d92e84d1247faa610a2f1a66c532
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67439757"
---
# <a name="share-or-upgrade-your-qna-maker-service"></a>공유 또는 QnA Maker 서비스 업그레이드
공유 하거나 더 나은 리소스를 관리 하려면 QnA Maker 서비스를 업그레이드 합니다. 

초기 생성 이후 QnA Maker 스택의 개별 구성 요소를 업그레이드하도록 선택할 수 있습니다. [여기](https://aka.ms/qnamaker-docs-capacity)에서 종속 구성 요소 및 SKU 선택의 세부 정보를 참조하세요.

## <a name="share-existing-services-with-qna-maker"></a>QnA Maker를 사용 하 여 기존 서비스를 공유 합니다.

QnA Maker는 여러 Azure 리소스를 만듭니다. 관리를 줄이고 비용 공유에서 혜택을 하기 위해 다음 표에서 사용 하 여 이해 할 수 있는 작업과 공유할 수 없습니다.

|서비스|공유|
|--|--|
|Cognitive Services|X|
|앱 서비스 계획|✔|
|App Service|X|
|Application Insights|✔|
|Search 서비스|✔|

## <a name="upgrade-qna-maker-management-sku"></a>QnA Maker 관리 SKU 업그레이드

기술 자료에서 현재 계층을 넘어서는 더 많은 질문과 답변이 필요한 경우 QnA Maker 서비스 가격 책정 계층을 업그레이드하세요. 

QnA Maker 관리 SKU를 업그레이드하려면

1. Azure Portal의 QnA Maker 리소스로 이동한 다음, **가격 책정 계층**을 선택합니다.

    ![QnA Maker 리소스](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource.png)

2. 적절한 SKU를 선택하고 **선택**을 누릅니다.

    ![QnA Maker 가격 책정](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-pricing-page.png)

## <a name="upgrade-app-service"></a>App Service 업그레이드

 기술 자료에서 클라이언트 앱에서 더 많은 요청을 처리해야 하는 경우 앱 서비스 가격 책정 계층을 업그레이드하세요.

App Service를 [확장](https://docs.microsoft.com/azure/app-service/web-sites-scale)하거나 축소할 수 있습니다.

1. Azure Portal의 App Service 리소스로 이동한 다음, 필요에 따라 **확장** 또는 **축소** 옵션을 선택합니다.

    ![QnA Maker 앱 서비스 크기 조정](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-scale.png)

## <a name="upgrade-azure-search-service"></a>Azure Search 서비스 업그레이드

많은 기술 자료를 포함할 계획인 경우 Azure Search 서비스 가격 책정 계층을 업그레이드하세요. 

지금은 Azure Search SKU의 현재 위치 업그레이드를 수행할 수 없습니다. 그러나 원하는 SKU로 새 Azure Search 리소스를 만들고, 데이터를 새 리소스로 복원한 다음, QnA Maker 스택에 연결할 수 있습니다.

1. Azure Portal에서 새 Azure Search 리소스를 만들고 원하는 SKU를 선택합니다.

    ![QnA Maker Azure Search 리소스](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

2. 원래 Azure Search 리소스의 인덱스를 새 리소스로 복원합니다. [여기](https://github.com/pchoudhari/QnAMakerBackupRestore)에서 백업 복원 샘플 코드를 참조하세요.

3. 데이터가 복원되면 새 Azure Search 리소스로 이동한 다음, **키**를 선택하고 **이름** 및 **관리자 키**를 적어 둡니다.

    ![QnA Maker Azure Search 키](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-keys.png)

4. 새 Azure Search 리소스를 QnA Maker 스택에 연결하려면 QnA Maker 앱 서비스로 이동합니다.

    ![QnA Maker appservice](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource-list-appservice.png)

5. **애플리케이션 설정**을 선택하고 3단계의 **AzureSearchName** 및 **AzureSearchAdminKey** 필드를 바꿉니다.

    ![QnA Maker appservice 설정](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-settings.png)

6. App Service를 다시 시작합니다.

    ![QnA Maker appservice 다시 시작](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [QnA Maker API 사용](../Quickstarts/csharp.md)
