---
title: Azure IoT Central에서 새 디바이스 유형 정의 | Microsoft Docs
description: 이 자습서에서는 작성기로서 Azure IoT Central 애플리케이션에서 새 장치 유형을 정의하는 방법을 알려줍니다. 유형에 대한 원격 분석, 상태, 속성 및 설정을 정의합니다.
author: dominicbetts
ms.author: dobett
ms.date: 01/28/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: 2506137e03e8677827bb1e2a3914ee10ae24f368
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55810119"
---
# <a name="tutorial-define-a-new-device-type-in-your-azure-iot-central-application-new-ui-design"></a>자습서: Azure IoT Central 애플리케이션에서 새 디바이스 유형 정의(새 UI 디자인)

이 자습서에서는 작성기로서 Microsoft Azure IoT Central 애플리케이션에서 새 장치 유형을 정의하기 위해 장치 템플릿을 사용하는 방법을 알려줍니다. 디바이스 템플릿은 디바이스 유형에 대해 원격 분석, 상태, 속성 및 설정을 정의합니다.

실제 디바이스를 연결하기 전에 애플리케이션을 테스트할 수 있으려면 디바이스를 만들 경우 IoT Central이 디바이스 템플릿에서 시뮬레이션된 디바이스를 생성합니다.

이 자습서에서는 **연결된 공조 디바이스** 템플릿을 만듭니다. 연결된 공조 디바이스:

* 온도 및 습도 같은 원격 분석을 보냅니다.
* 설정 또는 해제 같은 상태를 보고합니다.
* 펌웨어 버전 및 일련 번호 같은 디바이스 속성이 있습니다.
* 목표 온도 같은 설정이 있습니다.

이 자습서에서는 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * 새 디바이스 템플릿 만들기
> * 디바이스에 원격 분석 추가
> * 시뮬레이션된 원격 분석 보기
> * 이벤트 측정 정의
> * 시뮬레이션된 이벤트 보기
> * 상태 측정 정의
> * 시뮬레이션된 상태 보기
> * 설정 및 속성 사용
> * 명령 사용
> * 대시보드에서 시뮬레이션된 디바이스 보기

[!INCLUDE [iot-central-experimental-note](../../includes/iot-central-experimental-note.md)]

## <a name="prerequisites"></a>필수 조건

이 자습서를 완료하려면 Azure IoT Central 애플리케이션이 필요합니다. [Azure IoT Central 애플리케이션](quick-deploy-iot-central-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) 빠른 시작을 완료한 경우 빠른 시작에서 만든 애플리케이션을 다시 사용할 수 있습니다. 그렇지 않은 경우 빈 Azure IoT Central 애플리케이션을 만들려면 다음 단계를 완료합니다.

1. Azure IoT Central [애플리케이션 관리자](https://aka.ms/iotcentral) 페이지로 이동합니다.

2. Azure 구독에 액세스 하는 데 사용하는 이메일 주소와 암호를 입력합니다.

    ![조직 계정 입력](./media/tutorial-define-device-type-experimental/sign-in.png)

3. 새 Azure IoT Central 애플리케이션 만들기를 시작하려면 **새 애플리케이션**을 클릭합니다.

    ![Azure IoT Central 애플리케이션 관리자 페이지](./media/tutorial-define-device-type-experimental/iotcentralhome.png)

4. 새로운 Azure IoT Central 애플리케이션을 만들려면:
    
    * **평가판**을 선택합니다. 평가판 애플리케이션을 만드는 데 Azure 구독이 필요하지 않습니다.
    
       디렉터리 및 구독에 대한 자세한 내용은 [응용 프로그램 만들기 빠른 시작](quick-deploy-iot-central-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)을 참조하세요.
    
    * **사용자 지정 애플리케이션**을 선택합니다.
    
    * 선택적으로 **Contoso 공조 장치** 같은 친숙한 애플리케이션 이름을 선택할 수 있습니다. Azure IoT Central은 사용자를 위해 고유한 URL 접두사를 생성합니다. 이 URL 접두사를 더욱 기억하기 쉬운 것으로 변경할 수 있습니다.
    
    * **만들기**를 클릭합니다.

    ![Azure IoT Central 애플리케이션 페이지](./media/tutorial-define-device-type-experimental/iotcentralcreate.png)

    자세한 내용은 [응용 프로그램 만들기 빠른 시작](quick-deploy-iot-central-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)을 참조하세요.

## <a name="create-a-new-custom-device-template"></a>새 사용자 지정 디바이스 템플릿 만들기

작성기로서 애플리케이션에서 장치 템플릿을 만들고 편집할 수 있습니다. 디바이스 템플릿을 만들 때 Azure IoT Central은 템플릿에서 시뮬레이션된 디바이스를 생성합니다. 시뮬레이션된 디바이스는 원격 분석을 생성하여 실제 디바이스에 연결하기 전에 애플리케이션의 동작을 테스트할 수 있습니다.

애플리케이션에 새 디바이스 템플릿을 추가하려면 **디바이스 템플릿** 페이지로 이동해야 합니다. 이렇게 하려면 왼쪽 탐색 메뉴에서 **디바이스 템플릿**을 클릭합니다.

![디바이스 템플릿 페이지](./media/tutorial-define-device-type-experimental/devicetemplates.png)

## <a name="add-a-device-template"></a>디바이스 템플릿 추가

다음 단계에서는 애플리케이션에 온도 원격 분석을 전송하는 장치에 대한 새 **연결된 공조 장치** 템플릿을 만드는 방법을 보여줍니다.

1. **디바이스 템플릿** 페이지에서 **+새로 만들기**를 클릭합니다.

    ![디바이스 템플릿 페이지, 디바이스 템플릿 만들기](./media/tutorial-define-device-type-experimental/newtemplate.png)

3. **사용자 지정 디바이스 템플릿** 페이지에서 디바이스 이름으로 **연결된 공조 장치**를 입력한 다음, **만들기**를 클릭합니다. 디바이스 탐색기의 연산자에 표시되는 디바이스의 이미지를 업로드할 수 있습니다.

    ![사용자 지정 디바이스](./media/tutorial-define-device-type-experimental/createcustomdevice.png)

4. **연결된 공조 장치** 디바이스 템플릿에서, 원격 분석을 정의하는 **측정** 탭에 있는지 확인합니다. 정의하는 각 디바이스 템플릿에는 다음 작업을 수행할 수 있는 별도의 탭이 있습니다.

    * 디바이스에서 보내는 원격 분석, 이벤트, 상태 등의 _측정값_을 지정합니다.

    * 디바이스 제어에 사용되는 _설정_을 정의합니다.

    * 디바이스 메타데이터인 _속성_을 정의합니다.

    * 디바이스에서 직접 실행되는 _명령_을 정의합니다.

    * 디바이스와 연결되는 _규칙_을 정의합니다.

    * 운영자를 위한 디바이스 _대시보드_를 사용자 지정합니다.

    ![공조 장치 측정값](./media/tutorial-define-device-type-experimental/airconmeasurements.png)

    > [!NOTE]
    > 디바이스 템플릿의 이름을 변경하려면 페이지 위쪽에서 템플릿 이름을 클릭합니다.

5. 온도 원격 분석 측정값을 추가하려면 **+새 측정값**을 클릭합니다. 그런 다음, 측정 유형으로 **원격 분석**을 선택합니다.

    ![연결된 공조 장치 측정값](./media/tutorial-define-device-type-experimental/airconmeasurementsnew.png)

6. 디바이스 템플릿에 대해 정의하는 각 유형의 원격 분석은 다음과 같은 [구성 옵션](howto-set-up-template-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)을 포함합니다.

    * 옵션을 표시합니다.

    * 원격 분석의 세부 정보입니다.

    * 시뮬레이션 매개 변수입니다.

    **온도** 원격 분석을 구성하려면 다음 표의 정보를 사용합니다.

    | 설정              | 값         |
    | -------------------- | -----------   |
    | 표시 이름         | 온도   |
    | 필드 이름           | 온도   |
    | Units                | F             |
    | Min                  | 60            |
    | max                  | 110           |
    | 소수 자릿수       | 0             |

    또한 원격 분석 표시에 대한 색을 선택할 수 있습니다. 원격 분석 정의를 저장하려면 **저장**을 클릭합니다.

    ![온도 시뮬레이션 구성](./media/tutorial-define-device-type-experimental/temperaturesimulation.png)

7. 잠시 후 **측정값** 탭에 연결된 공조 장치 디바이스 시뮬레이션의 온도 원격 분석 차트가 표시됩니다. 컨트롤을 사용하여 표시 유형 및 집계를 관리하거나 원격 분석 정의를 편집합니다.

    ![온도 시뮬레이션 보기](./media/tutorial-define-device-type-experimental/viewsimulation.png)

8. **줄**, **누적** 및 **편집 시간 범위** 컨트롤을 사용하여 차트를 사용자 지정할 수도 있습니다.

    ![차트 사용자 지정](./media/tutorial-define-device-type-experimental/customizechart.png)

## <a name="add-an-event-measurement"></a>이벤트 측정값 추가

이벤트를 사용하여 오류 또는 구성 요소 오류와 같은 이벤트가 있을 때 디바이스에서 전송하는 시점 데이터를 정의합니다. Azure IoT Central은 실제 디바이스에 연결하기 전에 애플리케이션의 동작을 테스트할 수 있도록 디바이스 이벤트를 시뮬레이션할 수 있습니다. **측정값** 보기에서 디바이스 템플릿의 이벤트 측정을 정의합니다.

1. **팬 모터 오류** 이벤트 측정값을 추가하려면 **+ 새 측정값**을 클릭합니다. 그런 다음, 측정 유형으로 **이벤트**를 선택합니다.

    ![연결된 공조 장치 측정값](./media/tutorial-define-device-type-experimental/eventnew.png)

2. 디바이스 템플릿에 대해 정의하는 각 유형의 이벤트는 다음과 같은 [구성 옵션](howto-set-up-template-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)을 포함합니다.

   * 표시 이름입니다.

   * 필드 이름입니다.

   * 심각도입니다.

    **팬 모터 오류** 이벤트를 구성하려면 다음 표의 정보를 사용합니다.

    | 설정              | 값             |
    | -------------------- | -----------       |
    | 표시 이름         | 팬 모터 오류   |
    | 필드 이름           | fanmotorerr       |
    | 심각도             | 오류             |

    이벤트 정의를 저장하려면 **저장**을 클릭합니다.

    ![이벤트 측정 구성](./media/tutorial-define-device-type-experimental/eventconfiguration.png)

3. 잠시 후 **측정값** 탭에 연결된 공조 장치 디바이스 시뮬레이션에서 임의로 생성된 이벤트 차트가 표시됩니다. 컨트롤을 사용하여 표시 유형을 관리하거나 이벤트 정의를 편집합니다.

    ![이벤트 시뮬레이션 보기](./media/tutorial-define-device-type-experimental/eventview.png)

1. 이벤트에 대한 추가 세부 정보를 보려면 차트에서 이벤트를 클릭합니다.

    ![이벤트 세부 정보 보기](./media/tutorial-define-device-type-experimental/eventviewdetail.png)

## <a name="define-a-state-measurement"></a>상태 측정값 정의

상태를 사용하여 일정 시간 동안 디바이스나 해당 구성 요소의 상태를 정의하고 시각화할 수 있습니다. Azure IoT Central은 실제 디바이스에 연결하기 전에 애플리케이션의 동작을 테스트할 수 있도록 디바이스 상태를 시뮬레이션할 수 있습니다. **측정** 보기에서 디바이스 유형에 대한 상태 측정을 정의합니다.

1. **팬 모드** 상태 측정값을 추가하려면 **+ 새 측정값**을 클릭합니다. 그런 다음, 측정 유형으로 **상태**를 선택합니다.

    ![연결된 공조 장치 상태 측정값](./media/tutorial-define-device-type-experimental/statenew.png)

2. 디바이스 템플릿에 대해 정의하는 각 유형의 상태는 다음과 같은 [구성 옵션](howto-set-up-template-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)을 포함합니다.

   * 표시 이름입니다.

   * 필드 이름입니다.

   * 선택 사항으로 값이 레이블을 표시합니다.

   * 각 값에 대한 색.

    **팬 모터** 상태를 구성하려면 다음 표의 정보를 사용합니다.

    | 설정              | 값             |
    | -------------------- | -----------       |
    | 표시 이름         | 팬 모드          |
    | 필드 이름           | fanmode           |
    | 값                | 1                 |
    | 레이블 표시        | 운영         |
    | 값                | 0                 |
    | 레이블 표시        | 중지됨           |

    상태 측정값 정의를 저장하려면 **저장**을 클릭합니다.

    ![상태 측정 구성](./media/tutorial-define-device-type-experimental/stateconfiguration.png)

3. 잠시 후 **측정값** 탭에 연결된 공조 장치 디바이스 시뮬레이션에서 임의로 생성된 상태 차트가 표시됩니다. 컨트롤을 사용하여 표시 유형을 관리하거나 상태 정의를 편집합니다.

    ![상태 시뮬레이션 보기](./media/tutorial-define-device-type-experimental/stateview.png)

4. 짧은 기간 동안 디바이스에서 보낸 데이터 요소가 너무 많은 경우 상태 측정값이 다른 시각적 개체로 표시됩니다. 차트를 클릭하면 해당 기간 내의 모든 데이터 요소가 시간순으로 표시됩니다. 측정값을 더 상세히 보기 위해 시간 범위를 좁힐 수 있습니다.

## <a name="settings-properties-and-commands"></a>설정, 속성 및 명령

설정, 속성 및 명령은 디바이스 템플릿에서 정의되고 각 개별 디바이스와 연결된 다른 값입니다.

* _설정_을 사용하여 애플리케이션에서 구성 데이터를 장치에 보낼 수 있습니다. 예를 들어 연산자는 설정을 사용하여 디바이스 원격 분석 간격을 2초에서 5초로 변경할 수 있습니다. 운영자가 설정을 변경하는 경우 디바이스에서 변경을 확인할 때까지는 설정이 UI에서 보류 중으로 표시됩니다.

* 디바이스와 연결되는 메타데이터를 정의하기 위해 _속성_을 사용합니다. 속성에는 다음 두 카테고리가 있습니다.
    
    * _애플리케이션 속성_은 애플리케이션에서 장치에 대한 정보를 기록하는 데 사용합니다. 예를 들어, 애플리케이션 속성을 사용하여 장치 위치와 마지막 서비스 날짜를 기록할 수 있습니다. 이러한 속성은 애플리케이션에 저장되고 디바이스와 동기화되지 않습니다. 연산자는 속성에 값을 할당할 수 있습니다.

    * 속성 값을 애플리케이션에 보내도록 장치를 설정하려면 _장치 속성_을 사용할 수 있습니다. 이러한 속성은 디바이스에서만 변경할 수 있습니다. 연산자의 경우 디바이스 속성은 읽기 전용입니다. 이 연결된 공조 디바이스 시나리오에서 펌웨어 버전과 디바이스 일련 번호는 디바이스가 보고하는 디바이스 속성입니다.
    
    자세한 내용은 디바이스 템플릿 설정의 방법 가이드에서 [속성](howto-set-up-template-experimental.md#properties)을 참조하세요.

* _명령_을 사용하여 애플리케이션에서 장치를 원격으로 관리합니다. 클라우드에서 디바이스에 대한 명령을 직접 실행하여 디바이스를 제어할 수 있습니다. 예를 들어 운영자는 다시 부팅과 같은 명령을 실행하여 디바이스를 즉시 다시 부팅할 수 있습니다.

## <a name="use-settings"></a>설정 사용

구성 데이터를 디바이스에 보내도록 디바이스를 설정하려면 *설정*을 사용합니다. 이 섹션에서 **연결된 공조 디바이스** 템플릿에 설정을 추가하여 연산자가 연결된 공조 디바이스의 대상 온도를 설정하게 할 수 있습니다.

1. **연결된 공조 장치** 디바이스 템플릿의 **설정** 탭으로 이동합니다.

2. 숫자 또는 텍스트 등 다른 형식의 설정을 만들 수 있습니다. **번호**를 클릭하여 디바이스에 숫자 설정을 추가합니다.

3. **온도 설정**을 구성하려면 다음 표의 정보를 사용합니다.

    | 필드                | 값           |
    | -------------------- | -----------     |
    | 표시 이름         | 온도 설정 |
    | 필드 이름           | setTemperature  |
    | 측정 단위      | F               |
    | 소수 자릿수       | 1               |
    | 최솟값        | 20              |
    | 최댓값        | 200             |
    | 초기 값        | 80              |
    | 설명          | 공조 장치에 대한 대상 온도 설정 |

    그런 다음 **저장**을 클릭합니다.

    ![온도 설정 구성](./media/tutorial-define-device-type-experimental/configuresetting.png)

    > [!NOTE]
    > 디바이스가 설정 변경을 인식하는 경우 설정 상태가 **동기화됨**으로 변경됩니다.

4. 설정 타일을 이동하고 크기 조정하여 **설정** 탭의 레이아웃을 사용자 지정할 수 있습니다.

    ![설정 레이아웃 사용자 지정](./media/tutorial-define-device-type-experimental/settingslayout.png)

## <a name="use-properties"></a>속성 사용

*애플리케이션 속성*을 사용하여 애플리케이션에서 장치에 대한 정보를 저장합니다. 이 섹션에서는 **연결된 공조 장치** 템플릿에 애플리케이션 속성을 추가하여 장치 위치 및 마지막 서비스 날짜를 저장합니다. 이러한 속성은 애플리케이션에서 편집할 수 있습니다. 디바이스는 일련 번호, 펌웨어 버전 등 애플리케이션에서 읽기 전용인 속성도 보고합니다.

1. **연결된 공조 장치** 디바이스 템플릿의 **속성** 탭으로 이동합니다.

1. 숫자 또는 텍스트 등 다른 형식의 디바이스 속성을 만들 수 있습니다. 디바이스 템플릿에 위치 속성을 추가하려면 **위치**를 선택합니다. 위치 속성을 구성하려면 다음 표의 정보를 사용합니다.

    | 필드                | 값                |
    | -------------------- | -------------------- |
    | 표시 이름         | 위치             |
    | 필드 이름           | location             |
    | 초기 값        | 시애틀, WA          |
    | 설명          | 디바이스 위치      |

    다른 필드는 기본값 그대로 둡니다.

    ![디바이스 속성 구성](./media/tutorial-define-device-type-experimental/configureproperties.png)

    **저장**을 클릭합니다.

1. 디바이스 템플릿에 마지막 서비스 날짜 속성을 추가하려면 **날짜**를 선택합니다.

1. 마지막 서비스 날짜 속성을 구성하려면 다음 표의 정보를 사용합니다.

    | 필드                | 값                   |
    | -------------------- | ----------------------- |
    | 표시 이름         | 마지막 서비스 날짜       |
    | 필드 이름           | serviceDate             |
    | 초기 값        | 1/1/2019                |
    | 설명          | 마지막 서비스 날짜           |

    ![디바이스 속성 구성](./media/tutorial-define-device-type-experimental/configureproperties2.png)

    **저장**을 클릭합니다.

1. 속성 타일을 이동하고 크기 조정하여 **속성** 탭의 레이아웃을 사용자 지정할 수 있습니다.

1. 디바이스 템플릿에 펌웨어 버전 같은 디바이스 속성을 추가하려면 **디바이스 속성**을 선택합니다.

1. 펌웨어 버전을 구성하려면 다음 표의 정보를 사용합니다.

    | 필드                | 값                   |
    | -------------------- | ----------------------- |
    | 표시 이름         | 펌웨어 버전        |
    | 필드 이름           | firmwareVersion         |
    | 데이터 형식            | text                    |
    | 설명          | 공조의 펌웨어 버전 |

    ![펌웨어 버전 구성](./media/tutorial-define-device-type-experimental/configureproperties3.png)

    **저장**을 클릭합니다.

1. 디바이스 템플릿에 일련 번호 같은 디바이스 속성을 추가하려면 **디바이스 속성**을 선택합니다.

1. 일련 번호를 구성하려면 다음 표의 정보를 사용합니다.

    | 필드                | 값                   |
    | -------------------- | ----------------------- |
    | 표시 이름         | 일련 번호           |
    | 필드 이름           | serialNumber            |
    | 데이터 형식            | text                    |
    | 설명          | 공조의 일련 번호  |

    ![일련 번호 구성](./media/tutorial-define-device-type-experimental/configureproperties4.png)

    **저장**을 클릭합니다.

    > [!NOTE]
    > 장치 속성은 장치에서 애플리케이션에 전송됩니다. 펌웨어 버전 및 일련 번호의 값은 실제 디바이스가 IoT Central에 연결할 때 업데이트됩니다.

## <a name="use-commands"></a>명령 사용

_명령_을 사용하여 운영자가 디바이스에서 직접 명령을 실행하도록 할 수 있습니다. 이 섹션에서는 운영자가 연결된 공조 디바이스에 특정 메시지를 에코할 수 있도록 해주는 **연결된 공조 디바이스** 템플릿에 명령을 추가합니다.

1. **연결된 공조 장치** 디바이스 템플릿의 **명령** 탭으로 이동하여 템플릿을 편집합니다.

1. **+ 새 명령**을 클릭하여 디바이스에 명령을 추가하고 새 명령 구성을 시작합니다.

1. 새 명령을 구성하려면 다음 표의 정보를 사용합니다.

    | 필드                | 값           |
    | -------------------- | -----------     |
    | 표시 이름         | Echo 명령    |
    | 필드 이름           | echo            |
    | 기본 시간 제한      | 30              |
    | 표시 유형         | text            |
    | 설명          | 디바이스 명령  |  

    **입력 필드**에 대해 **+** 를 클릭하여 명령에 입력을 더 추가할 수 있습니다.

    ![설정 추가 준비](./media/tutorial-define-device-type-experimental/commandsecho1.png)

     **저장**을 클릭합니다.

1. 명령 타일을 이동하고 크기 조정하여 **명령** 탭의 레이아웃을 사용자 지정할 수 있습니다.

## <a name="view-your-simulated-device"></a>시뮬레이션된 디바이스 보기

**연결된 공조 장치** 디바이스 템플릿을 정의했으니, 이제 앞에서 정의한 측정값, 설정 및 속성을 포함하도록 **대시보드**를 사용자 지정할 수 있습니다. 그런 다음, 연산자로서 대시보드를 미리 보기할 수 있습니다.

1. **연결된 공조 장치** 디바이스 템플릿의 **대시보드** 탭을 선택합니다.

1. **꺾은선형 차트**를 클릭하여 **대시보드**에 구성 요소를 추가합니다.

1. 다음 표의 정보를 사용하여 **꺾은선형 차트** 구성 요소를 구성합니다.

    | 설정      | 값       |
    | ------------ | ----------- |
    | 제목        | 온도 |
    | 시간 범위   | 지난 30분 |
    | 측정     | 온도(**온도** 옆에 있는 **표시 유형** 클릭) |

    ![꺾은선형 차트 설정](./media/tutorial-define-device-type-experimental/linechartsettings.png)

    그런 다음 **Save**를 클릭합니다.

1. 다음 표의 정보를 사용하여 **이벤트 기록** 구성 요소를 클릭합니다.

    | 설정      | 값       |
    | ------------ | ----------- |
    | 제목        | 팬 모터 이벤트 |
    | 시간 범위   | 지난 30분 |
    | 측정     | 팬 모터 오류(**팬 모터 오류** 옆에 있는 **표시 유형** 클릭) |

    ![꺾은선형 차트 설정](./media/tutorial-define-device-type-experimental/dashboardeventchartsetting.png)

    그런 다음 **Save**를 클릭합니다.

1. 다음 표의 정보를 사용하여 **상태 기록** 구성 요소를 구성합니다.

    | 설정      | 값       |
    | ------------ | ----------- |
    | 제목        | 팬 모드 |
    | 시간 범위   | 지난 30분 |
    | 측정 | 팬 모드(**팬 모드** 옆에 있는 **표시 유형** 클릭) |

    ![꺾은선형 차트 설정](./media/tutorial-define-device-type-experimental/dashboardstatechartsetting.png)

    그런 다음 **Save**를 클릭합니다.

1. 대시보드에 디바이스 설정 및 속성을 추가하려면 **설정 및 속성**을 선택합니다. **추가/제거**를 클릭하여 대시보드에 표시할 설정이나 속성을 추가합니다.

1. 다음 표의 정보를 사용하여 **설정 및 속성** 구성 요소를 구성합니다.

    | 설정                 | 값         |
    | ----------------------- | ------------- |
    | 제목                   | 디바이스 속성 |
    | 설정 및 속성 | 온도 설정<br/>일련 번호<br/>펌웨어 버전 |

    이전에 **설정 및 속성** 페이지에서 정의한 설정 및 속성은 **사용 가능한 열**에 표시됩니다.

    ![온도 속성 설정 지정](./media/tutorial-define-device-type-experimental/propertysettings4.png)

    그런 다음 **Save**를 클릭합니다.

1. 이제 대시보드에서 연결된 공조 장치의 시뮬레이션 데이터를 볼 수 있습니다. 대시보드의 타일 및 레이아웃을 편집할 수 있습니다.

    ![대시보드 보기](./media/tutorial-define-device-type-experimental/dashboard.png)

## <a name="next-steps"></a>다음 단계

이 자습서에서는 다음 방법에 대해 알아보았습니다.

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * 새 디바이스 템플릿 만들기
> * 디바이스에 원격 분석 추가
> * 시뮬레이션된 원격 분석 보기
> * 디바이스 이벤트 정의
> * 시뮬레이션된 이벤트 보기
> * 상태 정의
> * 시뮬레이션된 상태 보기
> * 설정 및 속성 사용
> * 명령 사용
> * 대시보드에서 시뮬레이션된 디바이스 보기

Azure IoT Central 애플리케이션에서 디바이스 템플릿을 정의했으니, 그 다음으로 권장하는 단계는 다음과 같습니다.

* [디바이스에 대한 규칙 및 작업 구성](tutorial-configure-rules-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
* [연산자의 뷰 사용자 지정](tutorial-customize-operator-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)