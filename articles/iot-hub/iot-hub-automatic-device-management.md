---
title: Azure IoT Hub를 사용 하 여 대규모로 자동 장치 관리 | Microsoft Docs
description: Azure IoT Hub 자동 장치 관리를 사용 하 여 여러 IoT 장치에 구성 할당
author: ChrisGMsft
manager: bruz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: chrisgre
ms.openlocfilehash: ff4e236569cc728b7011ffa26554277f281397fd
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67485853"
---
# <a name="automatic-iot-device-management-at-scale-using-the-azure-portal"></a>Azure portal을 사용 하 여 대규모로 자동 IoT 장치 관리

[!INCLUDE [iot-edge-how-to-deploy-monitor-selector](../../includes/iot-hub-auto-device-config-selector.md)]

Azure IoT Hub에 자동 장치 관리는 다양 한 대규모 장치를 관리 하는 반복적이 고 복잡 한 작업을 자동화 합니다. 자동 장치 관리를 사용 하 여 해당 속성을 기반으로 하는 장치의 대상, 원하는 구성을 정의 다음 범위에 나올 때 장치를 업데이트 하는 IoT Hub를 사용 합니다. 이 업데이트가 완료 되를 사용 하 여는 _자동 장치 구성_, 완료 및 규정 준수, 핸들 병합 및 충돌을 요약 하 고 단계적된 방식으로 구성을 롤아웃할 수 있습니다.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Desired 속성을 사용 하 여 장치 쌍의 집합을 업데이트 하 고 장치 쌍을 기반으로 하는 요약을 보고 하 여 자동 장치 관리가 작동 reported 속성.  새 클래스와 이라고 하는 JSON 문서를 소개 하는 것을 *구성* 세 부분으로 구성 된:

* **대상 조건**은 업데이트할 디바이스 쌍의 범위를 정의합니다. 대상 조건은 디바이스 쌍 태그 및/또는 보고된 속성에서 쿼리로 지정됩니다.

* **대상 콘텐츠**는 대상으로 지정된 디바이스 쌍에 추가하거나 업데이트할 원하는 속성을 정의합니다. 콘텐츠에는 변경할 원하는 속성의 섹션에 대한 경로가 포함됩니다.

* **메트릭**은 **성공**, **진행 중** 및 **오류**와 같은 다양한 구성 상태의 요약 횟수를 정의합니다. 사용자 지정 메트릭은 보고된 디바이스 쌍 속성에서 쿼리로 지정됩니다.  시스템 메트릭은 대상으로 하는 장치 쌍의 수 및 성공적으로 업데이트 된 쌍의 수와 같은 쌍 업데이트 상태를 측정 하는 기본 메트릭입니다.

자동 장치 구성을 처음으로 구성을 만든 후에 곧 고 5 분 간격으로 실행 합니다. 메트릭 쿼리는 자동 장치 구성을 실행 될 때마다를 실행 됩니다.

## <a name="implement-device-twins-to-configure-devices"></a>디바이스 쌍 구현으로 디바이스를 구성

자동 디바이스 구성은 클라우드와 디바이스 간의 상태를 동기화하는 디바이스 쌍을 사용해야 합니다.  디바이스 쌍 사용에 대한 지침은 [IoT Hub에서 디바이스 쌍 이해 및 사용](iot-hub-devguide-device-twins.md)을 참조하세요.

## <a name="identify-devices-using-tags"></a>태그를 사용하여 디바이스 식별

구성의 만들려면 먼저 적용할 장치를 지정 해야 합니다. Azure IoT Hub는 디바이스 쌍의 태그를 사용하여 디바이스를 식별합니다. 각 디바이스는 여러 개의 태그를 갖출 수 있으며 솔루션에 적합한 방식으로 이러한 태그를 정의할 수 있습니다. 예를 들어, 서로 다른 위치에서 장치를 관리 하는 경우 장치 쌍에 다음 태그를 추가 합니다.

```json
"tags": {
    "location": {
        "state": "Washington",
        "city": "Tacoma"
    }
},
```

## <a name="create-a-configuration"></a>구성 만들기

1. [Azure Portal](https://portal.azure.com)에서 IoT Hub로 이동합니다. 

2. **IoT 디바이스 구성**을 선택합니다.

3. **구성 추가**를 선택합니다.

구성을 만드는 데에는 5단계가 있습니다. 다음 섹션에서는 각 단계로 안내합니다. 

### <a name="name-and-label"></a>이름 및 레이블

1. 구성에 최대 128자의 소문자로 된 고유한 이름을 지정합니다. 공백과 잘못된 문자(`& ^ [ ] { } \ | " < > /`)는 사용하지 않도록 합니다.

2. 구성을 추적하는 데 도움이 되는 레이블을 추가합니다. 레이블은 구성을 설명하는 **이름**, **값** 쌍입니다. 예를 들면 `HostPlatform, Linux` 또는 `Version, 3.0.1`과 같습니다.

3. **다음**을 선택하여 다음 단계로 이동합니다. 

### <a name="specify-settings"></a>설정 지정

이 섹션에서는 대상으로 지정된 디바이스 쌍에서 설정할 대상 콘텐츠를 지정합니다. 각 설정 집합에 대한 두 개의 입력이 있습니다. 첫 번째는 디바이스 쌍 경로로, 설정할 원하는 쌍 속성 내 JSON 섹션에 대한 경로입니다.  두 번째는 해당 섹션에 삽입할 JSON 콘텐츠입니다. 예를 들어, 다음으로 디바이스 쌍 경로 및 콘텐츠를 설정합니다.

![디바이스 쌍 경로 및 콘텐츠 설정](./media/iot-hub-auto-device-config/create-configuration-full-browser.png)

또한 디바이스 쌍 경로의 전체 경로 및 콘텐츠의 값을 대괄호를 사용하지 않고 지정하여 개별 설정을 지정할 수 있습니다. 예를 들어, 디바이스 쌍 경로를 `properties.desired.chiller-water.temperature`로 설정하고 콘텐츠를 `66`으로 설정합니다.

두 개 이상의 구성이 동일한 디바이스 쌍 경로를 대상으로 하는 경우 가장 높은 우선 순위 구성의 콘텐츠가 적용됩니다(우선 순위는 4단계에서에서 정의됨).

속성을 제거하려는 경우 속성 값을 `null`로 지정합니다.

**장치 쌍 설정 추가**를 선택하여 추가 설정을 추가할 수 있습니다.

### <a name="specify-metrics-optional"></a>메트릭 지정(선택 사항)

메트릭은은 장치가 구성 콘텐츠를 적용 한 후 다시 보고할 수 있는 다양 한 상태의 요약 수를 제공 합니다. 예를 들어 보류 중인 설정 변경에 대한 메트릭, 오류에 대한 메트릭 및 성공적인 설정 변경에 대한 메트릭을 만들 수 있습니다.

1. **메트릭 이름**에 대한 이름을 입력합니다.

2. **메트릭 조건**에 대한 쿼리를 입력합니다.  쿼리는 보고된 디바이스 쌍 속성을 기준으로 합니다.  메트릭은 쿼리에 의해 반환되는 행 수를 나타냅니다.

예를 들면 다음과 같습니다.

```sql
SELECT deviceId FROM devices 
  WHERE properties.reported.chillerWaterSettings.status='pending'
```

구성이 적용된 절을 포함할 수 있습니다. 예를 들면 다음과 같습니다. 

```sql
/* Include the double brackets. */
SELECT deviceId FROM devices 
  WHERE configurations.[[yourconfigname]].status='Applied'
```

### <a name="target-devices"></a>대상 디바이스

디바이스 쌍의 태그 속성을 사용하여 이 구성을 받아야 하는 특정 디바이스를 대상으로 지정합니다.  또한 보고된 디바이스 쌍 속성을 기준으로 디바이스를 대상으로 지정할 수 있습니다.

여러 구성에서 동일한 디바이스를 대상으로 지정할 수 있으므로 각 구성에 우선 순위 번호를 부여해야 합니다. 충돌하는 경우 우선 순위가 가장 높은 구성이 먼저 적용됩니다. 

1. 구성 **우선 순위**에 대해 양의 정수를 입력합니다. 가장 높은 숫자 값이 우선 순위가 가장 높은 것으로 간주됩니다. 두 구성의 우선 순위 번호가 동일한 경우 가장 최근에 만든 구성이 먼저 적용됩니다. 

2. **대상 조건**을 입력하여 이 구성의 대상으로 지정할 디바이스를 결정합니다. 조건은 디바이스 쌍 태그 또는 보고되는 디바이스 쌍 속성을 기반으로 하며, 표현 형식이 일치해야 합니다. 예를 들면 `tags.environment='test'` 또는 `properties.reported.chillerProperties.model='4000x'`과 같습니다. 모든 디바이스를 대상으로 지정하도록 `*`를 지정할 수 있습니다.

3. **다음**을 선택하여 최종 단계로 이동합니다.

### <a name="review-configuration"></a>구성 검토

구성 정보를 검토한 다음, **제출**을 선택합니다.

## <a name="monitor-a-configuration"></a>구성 모니터링

구성의 세부 정보를 확인하고 이를 실행하는 디바이스를 모니터링하려면 다음 단계를 사용합니다.

1. [Azure Portal](https://portal.azure.com)에서 IoT Hub로 이동합니다. 

2. **IoT 디바이스 구성**을 선택합니다.

3. 구성 목록을 검사합니다. 각 구성의 경우 다음 세부 정보를 볼 수 있습니다.

   * **ID** - 구성의 이름입니다.

   * **대상 조건** - 대상으로 지정된 디바이스를 정의하는 데 사용되는 태그입니다.

   * **우선 순위** - 구성에 할당된 우선 순위 번호입니다.

   * **만든 시간** - 구성을 만든 시기의 타임스탬프입니다. 이 타임스탬프는 두 구성의 우선 순위가 동일한 경우 연결을 중단하는 데 사용됩니다. 

   * **시스템 메트릭** - IoT Hub에 의해 계산되며 개발자가 사용자 지정할 수 없는 메트릭입니다. 대상 지정은 대상 조건과 일치하는 디바이스 쌍의 수를 지정합니다. 구성에 의해 수정된 디바이스 쌍의 수가 지정되어 적용됩니다. 여기에는 별도의 우선 순위가 높은 구성에서 변경이 발생하는 경우의 부분적인 수정이 포함될 수 있습니다. 

   * **사용자 지정 메트릭** - 보고된 디바이스 쌍 속성에 대한 쿼리로 개발자가 지정한 메트릭입니다.  구성마다 최대 5개의 사용자 지정 메트릭을 정의할 수 있습니다. 
   
4. 모니터링하려는 구성을 선택합니다.  

5. 구성 세부 정보를 검사합니다. 탭을 사용하여 구성을 받은 디바이스에 대한 특정 세부 정보를 볼 수 있습니다.

   * **대상 조건** - 대상 조건과 일치하는 디바이스입니다. 

   * **메트릭** - 시스템 메트릭 및 사용자 지정 메트릭의 목록입니다.  드롭다운에서 메트릭을 선택한 다음, **디바이스 보기**를 선택하면 각 메트릭에 대해 카운트된 디바이스의 목록을 볼 수 있습니다.

   * **디바이스 쌍 설정** - 구성에 의해 설정된 디바이스 쌍 설정입니다. 

   * **구성 레이블** - 구성을 설명하는 데 사용되는 키-값 쌍입니다.  레이블은 기능에 영향을 미치지 않습니다. 

## <a name="modify-a-configuration"></a>구성 수정

구성을 수정하면 변경 내용이 즉시 모든 대상 디바이스에 복제됩니다. 

대상 조건을 업데이트하면 다음과 같은 업데이트가 발생합니다.

* 디바이스 쌍에서 이전 대상 조건을 충족하지 않았지만 새 대상 조건은 충족하고 이 구성이 해당 디바이스 쌍에 대해 가장 높은 순위이면 이 구성이 디바이스 쌍에 적용됩니다. 

* 디바이스 쌍이 더 이상 대상 조건을 충족하지 않는 경우 구성에서 설정이 제거되고, 다음으로 가장 높은 우선 순위 구성에 의해 디바이스 쌍이 수정됩니다. 

* 현재 이 구성에서 실행되는 디바이스 쌍이 더 이상 대상 조건을 충족하지 않으며 다른 어떤 구성의 대상 조건도 충족하지 않는 경우에는 구성에서 설정이 제거되며 쌍에 다른 변경이 발생하지 않습니다. 

구성을 수정하려면 다음 단계를 사용합니다. 

1. [Azure Portal](https://portal.azure.com)에서 IoT Hub로 이동합니다. 

2. **IoT 디바이스 구성**을 선택합니다. 

3. 수정하려는 구성을 선택합니다. 

4. 다음 필드를 업데이트합니다. 

   * 대상 조건 
   * 레이블 
   * 우선 순위 
   * 메트릭

4. **저장**을 선택합니다.

5. [구성 모니터링](#monitor-a-configuration)의 단계에 따라 변경 내용이 롤아웃되는지 확인합니다. 

## <a name="delete-a-configuration"></a>구성 삭제

구성을 삭제하면 모든 디바이스 쌍은 다음으로 가장 높은 우선 순위 구성을 적용합니다. 디바이스 쌍이 다른 구성의 대상 조건을 충족하지 않는 경우에는 다른 설정이 적용됩니다. 

1. [Azure Portal](https://portal.azure.com)에서 IoT Hub로 이동합니다. 

2. **IoT 디바이스 구성**을 선택합니다. 

3. 확인란을 사용하여 삭제하려는 구성을 선택합니다. 

4. **삭제**를 선택합니다.

5. 메시지에서 확인을 요청합니다.

## <a name="next-steps"></a>다음 단계

이 문서에서는 구성 및 크기 조정 시 IoT 장치를 모니터링 하는 방법을 알아보았습니다. Azure IoT Hub를 관리하는 방법에 대한 자세한 내용을 알아보려면 다음 링크를 따라가세요.

* [대량으로 IoT Hub 장치 ID 관리](iot-hub-bulk-identity-mgmt.md)
* [IoT Hub 메트릭](iot-hub-metrics.md)
* [작업 모니터링](iot-hub-operations-monitoring.md)

IoT Hub의 기능을 추가로 탐색하려면 다음을 참조하세요.

* [IoT Hub 개발자 가이드](iot-hub-devguide.md)
* [Azure IoT Edge를 사용하여 에지 장치에 AI 배포](../iot-edge/tutorial-simulate-device-linux.md)

IoT Hub Device Provisioning Service를 사용하여 무인 Just-In-Time 프로비저닝을 수행하는 방법을 알아보려면 다음을 참조하세요. 

* [Azure IoT Hub Device Provisioning Service](/azure/iot-dps)
