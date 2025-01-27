---
title: 포함 파일
description: 포함 파일
services: batch
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 05/04/2018
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: 51b687dc2a6264eb12362616429746c9b182c3b1
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68323878"
---
> [!NOTE]
> 배치 계정을 만들 때는 **사용자 구독** 및 **Batch 서비스**라는 두 가지 *풀 할당* 모드 중에서 선택할 수 있습니다. 대부분의 경우 기본 Batch 서비스를 사용해야 합니다. 이 서비스에서는 풀이 Azure에서 관리하는 구독에서 배후에 할당됩니다. 대체 사용자 구독 모드인 경우 Batch VM 및 기타 리소스는 풀이 만들어질 때 구독에서 직접 만들어집니다. [Azure Reserved VM Instances](https://azure.microsoft.com/pricing/reserved-vm-instances/)를 사용하여 Batch 풀을 만드는 경우 사용자 구독 모드가 필요합니다. 사용자 구독 모드에서 Batch 계정을 만들려면 또한 Azure Batch를 사용하여 구독을 등록하고 Azure Key Vault와 계정을 연결해야 합니다.