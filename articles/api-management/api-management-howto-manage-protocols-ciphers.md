---
title: Azure API Management에서 프로토콜 및 암호화 관리 | Microsoft Docs
description: Azure API Management에서 프로토콜(TLS, SSL) 및 암호화(DES)를 관리하는 방법을 알아봅니다.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2018
ms.author: apimpm
ms.openlocfilehash: 454ba5c42694581bfa8fb1ec69ce97ac906b53d4
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47094578"
---
# <a name="manage-protocols-and-ciphers-in-azure-api-management"></a>Azure API Management에서 프로토콜 및 암호화 관리

Azure API Management는 클라이언트 및 백 엔드 쪽에서 모두 여러 TLS 프로토콜 버전을 지원할 뿐 아니라 3DES 암호화도 지원합니다.

이 가이드에서는 Azure API Management 인스턴스용 프로토콜 및 암호화 구성을 관리하는 방법을 설명합니다.

![APIM에서 프로토콜 및 암호화 관리](./media/api-management-howto-manage-protocols-ciphers/api-management-protocols-ciphers.png)

## <a name="prerequisites"></a>필수 조건

이 문서의 단계를 따르려면 다음이 있어야 합니다.

* API Management 인스턴스

## <a name="how-to-manage-tls-protocols-and-3des-cipher"></a>TLS 프로토콜 및 3DES 암호화를 관리하는 방법

1. Azure Portal에서 **API Management 인스턴스**로 이동합니다.
2. 메뉴에서 **SSL**을 선택합니다.  
    ![APIM에서 프로토콜 및 암호화 관리 - 메뉴](./media/api-management-howto-manage-protocols-ciphers/api-management-menu.png)
3. 원하는 프로토콜 또는 암호화를 사용하거나 사용하지 않도록 설정합니다.
4. **저장**을 클릭합니다. 변경 내용은 1시간 이내에 적용됩니다.  
    ![APIM에서 프로토콜 및 암호화 관리 - 저장](./media/api-management-howto-manage-protocols-ciphers/api-management-protocols-ciphers-save.png)

## <a name="next-steps"></a>다음 단계

* [TLS(전송 계층 보안)](https://docs.microsoft.com/en-us/dotnet/framework/network-programming/tls)에 대해 자세히 알아봅니다.
* API Management에 대한 추가 [비디오](https://azure.microsoft.com/documentation/videos/index/?services=api-management) 를 확인합니다.