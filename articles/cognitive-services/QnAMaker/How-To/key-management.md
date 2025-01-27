---
title: 리소스 및 키 관리 - QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker 서비스는 구독 키와 엔드포인트 키의 두 종류 키를 사용합니다.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 03/04/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: b9be1db9be1d4dd57994e101c07ed430425a5912
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447433"
---
# <a name="how-to-manage-keys-in-qna-maker"></a>QnA Maker에서 키를 관리하는 방법

QnA Maker 서비스는 **구독 키**와 **엔드포인트 키**의 두 종류 키를 사용합니다.

![키 관리](../media/qnamaker-how-to-key-management/key-management.png)

1. **구독 키**: 이러한 키는 [QnA Maker 관리 서비스 API](https://go.microsoft.com/fwlink/?linkid=2092179)에 액세스하는 데 사용됩니다. 이러한 API를 통해 기술 자료를 편집할 수 있습니다.  

2. **엔드포인트 키**: 이러한 키는 사용자 질문에 대한 응답을 가져오기 위해 기술 자료 엔드포인트에 액세스하는 데 사용됩니다. 일반적으로 QnA Maker 서비스를 이용하는 클라이언트 애플리케이션 코드 또는 챗봇에서 이 엔드포인트를 사용합니다.
 
## <a name="subscription-keys"></a>구독 키
QnA Maker 리소스를 만든 Azure Portal에서 구독 키를 보고 다시 설정할 수 있습니다. 
1. Azure Portal에서 QnA Maker 리소스로 이동합니다.

    ![QnA Maker 리소스 목록](../media/qnamaker-how-to-key-management/qnamaker-resource-list.png)

2. **키**로 이동합니다.

    ![구독 키](../media/qnamaker-how-to-key-management/subscription-key.PNG)

## <a name="endpoint-keys"></a>엔드포인트 키

엔드포인트 키는 [QnA Maker 포털](https://qnamaker.ai)에서 관리할 수 있습니다.

1. [QnA Maker 포털](https://qnamaker.ai)에 로그인하고 프로필로 이동한 후 **서비스 설정**을 클릭합니다.

    ![엔드포인트 키](../media/qnamaker-how-to-key-management/Endpoint-keys.png)

2. 키를 보거나 다시 설정합니다.

    ![엔드포인트 키 관리자](../media/qnamaker-how-to-key-management/Endpoint-keys1.png)

    >[!NOTE]
    >손상된 것처럼 보이면 키를 새로 고칩니다. 클라이언트 애플리케이션 또는 봇 코드에 해당 변경 내용을 적용해야 할 수도 있습니다.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [다른 언어로 된 기술 자료 만들기](./language-knowledge-base.md)
