---
title: 포함 파일
description: 포함 파일
services: event-hubs
author: sethmanheim
ms.service: event-hubs
ms.topic: include
ms.date: 05/22/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: b105fb14608d53c5c2ef469ab44e211ccdf4d3c8
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67812527"
---
다음 표는 [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)에 지정된 할당량과 제한을 나열합니다. Event Hubs 가격에 대한 자세한 내용은 [Event Hubs 가격 책정](https://azure.microsoft.com/pricing/details/event-hubs/)을 참조하세요.

| 제한 | 범위 | 참고 | 값 |
| --- | --- | --- | --- |
| 구독당 Event Hubs 네임스페이스 수 |구독 |- |100 |
| 네임스페이스당 Event Hubs 개수 |네임스페이스 |새 이벤트 허브를 만들기 위한 후속 요청이 거부 됩니다. |10 |
| Event Hub당 파티션 수 |엔터티 |- |32 |
| Event Hub당 소비자 그룹 수 |엔터티 |- |20 |
| 네임스페이스당 AMQP 연결 수 |네임스페이스 |추가 연결에 대 한 후속 요청은 거부 되 고 호출 코드에서 예외가 수신 됩니다. |5,000 |
| Event Hubs 이벤트의 최대 크기|엔터티 |- |1MB |
| Event Hub 이름의 최대 크기 |엔터티 |- |50자 |
| 소비자 그룹당 비 Epoch 수신자 수 |엔터티 |- |5 |
| 이벤트 데이터의 최대 보존 기간 |엔터티 |- |1-7일 |
| 최대 처리량 단위 |네임스페이스 |처리량 단위 제한을 초과 하면 데이터가 정체 되 고 생성 된 [서버 사용 중 예외](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception)합니다. 많은 수의 표준 계층에 대 한 처리량 단위를 요청 하려면 파일을 [지원 요청](/azure/azure-supportability/how-to-create-azure-support-request)합니다. [추가 처리량 단위](../articles/event-hubs/event-hubs-auto-inflate.md)는 약정된 구매를 기준으로 20개 단위로 사용할 수 있습니다. |20 |
| 네임스페이스당 권한 부여 규칙의 수 |네임스페이스|권한 부여 규칙 만들기 위한 후속 요청이 거부 됩니다.|12 |
| GetRuntimeInformation 메서드 호출 수 | 엔터티 | - | 초 당 50 | 
| 가상 네트워크 (VNet) 및 IP 구성은 규칙 개수 | 엔터티 | - | 128 | 

### <a name="event-hubs-dedicated---quotas-and-limits"></a>Event Hubs Dedicated-할당량 및 제한
Event Hubs Dedicated 제품은 최소 4 시간의 사용량을 사용 하 여 고정된 된 월간 가격으로 청구 됩니다. Dedicated 계층 까다로운 워크 로드를 사용 하 여 고객을 위한 엔터프라이즈급 용량 및 제한 하지만 표준 요금제의 모든 기능을 제공합니다. 

| 기능 | 제한 |
| --- | ---|
| 대역폭 |  20 Cu |
| 네임스페이스 | CU 당 50 |
| Event Hubs |  네임 스페이스 당 1000 |
| 수신 이벤트 | 포함 |
| 메시지 크기 | 1 백만 바이트 |
| 파티션 | CU 당 2000 |
| 소비자 그룹 | 제한 없음, CU 당 event hub 당 1000 |
| 조정된 연결 | 100,000개 포함 |
| 메시지 보존 | 등록 7 일 (90 일 보존 서비스 예정), 10TB 포함 CU 당 |
| 캡처 | 포함 |
