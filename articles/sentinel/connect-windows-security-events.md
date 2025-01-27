---
title: Windows 보안 이벤트 데이터를 Azure 미리 보기 Sentinel 연결할 | Microsoft Docs
description: Azure Sentinel에 Windows 보안 이벤트 데이터를 연결 하는 방법을 알아봅니다.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: d51d2e09-a073-41c8-b396-91d60b057e6a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2019
ms.author: rkarlin
ms.openlocfilehash: 188febf090ddb3f685f9d3c3b94d822f15bbcfcb
ms.sourcegitcommit: 80aaf27e3ad2cc4a6599a3b6af0196c6239e6918
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673774"
---
# <a name="connect-windows-security-events"></a>Windows 보안 이벤트 연결 

> [!IMPORTANT]
> Azure Sentinel은 현재 공개 미리 보기로 제공됩니다.
> 이 미리 보기 버전은 서비스 수준 계약 없이 제공되며 프로덕션 워크로드에는 사용하지 않는 것이 좋습니다. 특정 기능이 지원되지 않거나 기능이 제한될 수 있습니다. 자세한 내용은 [Microsoft Azure Preview에 대한 추가 사용 약관](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)을 참조하세요.

Azure Sentinel 작업 영역에 연결 된 Windows 서버에서 모든 보안 이벤트를 스트리밍할 수 있습니다. 이 연결을 사용 하면 대시보드를 볼, 사용자 지정 경고 및 조사를 향상 시킬 수 있습니다. 이 조직의 네트워크에 대 한 자세한 정보를 제공 하며 사용자 보안 작업 기능을 향상 시킵니다.  이벤트 스트림를 선택할 수 있습니다.

- **모든 이벤트** -모든 Windows 보안 및 AppLocker 이벤트입니다.
- **일반적인** -감사 목적에 대 한 이벤트의 표준 집합입니다. 전체 사용자 감사 내역을이 집합에 포함 됩니다. 예를 들어,이 집합에는 사용자 로그인 및 사용자 로그 아웃 이벤트 (이벤트 ID 4634)를 모두 포함합니다. Microsoft는 보안 그룹 변경, 주요 도메인 컨트롤러 Kerberos 작업, 그리고 업계의 조직에서 권장하는 기타 이벤트와 같은 감사 작업을 이 집합에 포함했습니다.

양이 매우 적은 이벤트는 일반 집합에 포함되었습니다. 모든 이벤트를 선택하는 대신 이러한 이벤트를 선택하는 주된 이유는 특정 이벤트를 필터링하여 제외하기 위해서가 아니라 이벤트의 양을 줄이기 위해서이기 때문입니다.
- **최소** -소수의 잠재적인 위협 요소를 나타낼 수 있는 이벤트입니다. 이 옵션을 설정 하면 전체 감사 내역을 얻을 수 없습니다.  이 집합에서는 보안 침해 성공을 나타낼 수 있는 이벤트만 및 매우 낮은 볼륨에 있는 중요 한 이벤트를 다룹니다. 예를 들어 사용자 로그인 성공 / 실패 (이벤트 Id 4624, 4625),이 집합에 포함 되어 있지만 로그인 감사를 위해 중요 하지만 검색에 대 한 의미가 없습니다 고 상대적으로 높은 볼륨 정보를 포함 하지 않습니다. 이 집합의 데이터 볼륨의 대부분에는 이벤트 및 프로세스 생성 이벤트 (이벤트 ID 4688)에 로그인 됩니다.
- **None** -보안 또는 AppLocker 이벤트가 없습니다.

> [!NOTE]
> 
> - 데이터 작업 영역의 Azure Sentinel 실행 하는 지리적 위치에 저장 됩니다.

다음은 각 집합에 대 한 보안 및 App Locker 전체 분류 이벤트 Id를 제공 합니다.

| 데이터 계층 | 수집된 이벤트 표시기 |
| --- | --- |
| 최소 | 1102,4624,4625,4657,4663,4688,4700,4702,4719,4720,4722,4723,4724,4727,4728,4732,4735,4737,4739,4740,4754,4755, |
| | 4756,4767,4799,4825,4946,4948,4956,5024,5033,8001,8002,8003,8004,8005,8006,8007,8222 |
| 일반 | 1,299,300,324,340,403,404,410,411,412,413,431,500,501,1100,1102,1107,1108,4608,4610,4611,4614,4622, |
| |  4624,4625,4634,4647,4648,4649,4657,4661,4662,4663,4665,4666,4667,4688,4670,4672,4673,4674,4675,4689,4697, |
| | 4700,4702,4704,4705,4716,4717,4718,4719,4720,4722,4723,4724,4725,4726,4727,4728,4729,4733,4732,4735,4737, |
| | 4738,4739,4740,4742,4744,4745,4746,4750,4751,4752,4754,4755,4756,4757,4760,4761,4762,4764,4767,4768,4771, |
| | 4774,4778,4779,4781,4793,4797,4798,4799,4800,4801,4802,4803,4825,4826,4870,4886,4887,4888,4893,4898,4902, |
| | 4904,4905,4907,4931,4932,4933,4946,4948,4956,4985,5024,5033,5059,5136,5137,5140,5145,5632,6144,6145,6272, |
| | 6273,6278,6416,6423,6424,8001,8002,8003,8004,8005,8006,8007,8222,26401,30004 |

## <a name="set-up-the-windows-security-events-connector"></a>Windows 보안 이벤트 connector 설정

완전히 통합할 Windows 보안 이벤트 Azure Sentinel:

1. Sentinel Azure portal에서 선택 **데이터 커넥터** 를 클릭 하 고는 **Windows 보안 이벤트** 바둑판식으로 배열 합니다. 
1. 스트리밍 하려는 데이터 유형을 선택 합니다.
1. **업데이트**를 클릭합니다.
6. Log Analytics에서 관련 스키마를 사용 하 여 Windows 보안 이벤트를 검색할 **SecurityEvent**합니다.

## <a name="validate-connectivity"></a>연결 유효성 검사

로그를 Log Analytics에 나타나기 시작 될 때까지 약 20 분 정도 걸릴 수 있습니다. 



## <a name="next-steps"></a>다음 단계
이 문서에서는 Azure Sentinel에 Windows 보안 이벤트를 연결 하는 방법을 알아보았습니다. Azure Sentinel에 대한 자세한 내용은 다음 문서를 참조하세요.
- 에 대해 알아봅니다 하는 방법 [데이터에 잠재적 위협을 파악](quickstart-get-visibility.md)합니다.
- 시작 [사용 하 여 Azure Sentinel 위협을 감지 하도록](tutorial-detect-threats.md)합니다.

