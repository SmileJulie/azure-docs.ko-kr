---
title: Azure IoT Hub Device Provisioning Service에서 X.509 인증서를 배포하는 방법 | Microsoft Docs
description: 장치 프로비저닝 서비스 인스턴스로 X.509 인증서를 배포하는 방법
author: wesmc7777
ms.author: wesmc
ms.date: 08/06/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: 9c73ce159ae7cf5778210e0fb587135f37c73f57
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/09/2018
ms.locfileid: "40024883"
---
# <a name="how-to-roll-x509-device-certificates"></a>X.509 장치 인증서 배포 방법

IoT 솔루션의 수명 주기 동안 인증서를 배포해야 합니다. 인증서를 배포하는 주된 이유 두 가지는 보안 위반 및 인증서 만료가 될 것입니다. 

인증서 배포는 위반 발생 시 시스템을 보호할 수 있는 보안 모범 사례입니다. [보안 위험 가정 방법론](http://download.microsoft.com/download/C/1/9/C1990DBA-502F-4C2A-848D-392B93D9B9C3/Microsoft_Enterprise_Cloud_Red_Teaming.pdf)의 일환으로 Microsoft는 예방 조치와 함께 사후 보안 프로세스의 필요성을 주장하고 있습니다. 장치 인증서 배포는 이러한 보안 프로세스의 일환으로 포함되어야 합니다. 인증서 배포 빈도는 솔루션의 보안 요구에 따라 다릅니다. 중요도가 높은 데이터가 관련된 솔루션의 고객은 매일 증서를 배포할 수 있고, 다른 고객은 2년마다 인증서를 배포할 수도 있습니다.

장치 인증서 배포에는 장치 및 IoT Hub에 저장된 인증서 업데이트가 포함됩니다. 이후 장치는 Device Provisioning Service를 통해 일반 [자동 프로비전](concepts-auto-provisioning.md)을 사용하여 IoT Hub로 스스로 다시 프로비전합니다.


## <a name="obtain-new-certificates"></a>새 인증서 가져오기

몇 가지 방법으로 IoT 장치에 대한 새 인증서를 가져올 수 있습니다. 여기에는 장치 팩토리에서 인증서 가져오기, 자체 인증서 생성, 타사가 인증서 만들기를 관리하게 지정이 포함됩니다. 

인증서는 상호 서명을 통해 루트 CA 인증서에서 [리프 인증서](concepts-security.md#end-entity-leaf-certificate)까지 신뢰 체인을 형성합니다. 서명 인증서는 신뢰 체인의 끝에서 리프 인증서 서명에 사용되는 인증서입니다. 서명 인증서는 루트 CA 인증서이거나 신뢰 체인의 중간 인증서가 될 수 있습니다. 자세한 내용은 [X.509 인증서](concepts-security.md#x509-certificates)를 참조하세요.
 
두 가지 방법으로 서명 인증서를 가져올 수 있습니다. 첫 번쨰 방법은 프로덕션 시스템에 권장되는 것으로, 루트 CA(인증 기관)에서 서명 인증서를 구매하는 것입니다. 이렇게 하면 보안이 신뢰할 수 있는 원본에 연결됩니다. 

두 번째 방법은 OpenSSL와 같은 도구를 사용하여 사용자 고유의 X.509 인증서를 만드는 것입니다. 이 방법은 X.509 인증서 테스트에 적합하지만 보안과 관련한 보증이 거의 없습니다. 자체 CA 공급자 역할을 준비하는 것이 아니라면 이 방법은 테스트 목적으로만 사용하는 것이 좋습니다.
 

## <a name="roll-the-certificate-on-the-device"></a>장치에 인증서 배포

장치의 인증서는 항상 [HSM(하드웨어 보안 모듈)](concepts-device.md#hardware-security-module)처럼 안전한 곳에 저장되어야 합니다. 장치 인증서 배포 방법은 처음 장치에서 만들어 설치한 방법에 따라 달라집니다. 

타사에서 인증서를 가져온 경우 해당하는 인증서 배포 방법을 살펴보어야 합니다.  준비된 프로세스가 포함되어 있거나 별도의 서비스를 제공할 수 있습니다. 

자체 장치 인증서를 관리할 경우 인증서 업데이트를 위한 자체 파이프라인을 작성해야 합니다. 이전 및 새 리프 인증서가 동일한 CN(공통 이름)을 갖는지 확인합니다. 동일한 CN을 통해 장치는 중복 등록 레코드를 만들지 않고 스스로 다시 프로비전할 수 있습니다.


## <a name="roll-the-certificate-in-the-iot-hub"></a>IoT hub에 인증서 배포

장치 인증서를 수동으로 IoT Hub에 추가할 수 있습니다. Device Provisioning Service 인스턴스를 사용하여 인증서를 자동화할 수도 있습니다. 이 문서에서는 프로비전 서비스 인스턴스를 사용하여 자동 프로비전을 지원한다고 가정합니다.

장치가 최초에 자동 프로비전을 통해 프로비전된 경우 부팅하고 프로비전 서비스에 접속합니다. 프로비전 서비스는 장치의 리프 인증서를 자격 증명으로 사용하여 IoT Hub에서 장치 ID를 만들기 전에 ID 검사를 수행하여 응대합니다. 그러면 프로비전 서비스가 할당된 IoT Hub를 장치에게 알리고, 장치가 리프 인증서를 사용하여 해당 IoT Hub에 인증 및 연결합니다. 

새 리프 인증서가 장치에 배포되면 새 인증서를 사용하여 연결하므로 더 이상 IoT hub에 연결할 수 없습니다. IoT Hub는 이전 인증서가 있는 장치만 인식합니다. 장치의 연결 시도 결과는 "권한 없음" 연결 오류입니다. 이 오류를 해결하려면 장치의 새 리프 인증서에 부합하게 장치에 대한 등록 항목을 업데이트해야 합니다. 그러면 장치가 다시 프로비전될 때 프로비전 서비스가 IoT Hub 장치 레지스트리 정보를 필요에 따라 업데이트할 수 있습니다. 

이 연결 실패의 가능한 예외 하나는 프로비전서비스에서 장치에 대해 [등록 그룹](concepts-service.md#enrollment-group)을 만드는 시나리오입니다. 이 상황에서는 장치의 인증서 신뢰 체인에 루트 또는 중간 인증서를 배포하지 않는 경우, 새 인증서가 등록 그룹에 정의된 신뢰 체인의 일부일 때 장치가 인식됩니다. 이 시나리오는 보안 위반에 대한 조치로 발생하며 최소한 위반된 것으로 간주되는 그룹의 특정 장치 인증서를 블랙리스트 처리해야 합니다. 자세한 내용은 [등록 그룹의 특정 장치 블랙리스트 처리](https://docs.microsoft.com/en-us/azure/iot-dps/how-to-revoke-device-access-portal#blacklist-specific-devices-in-an-enrollment-group)를 참조하세요.

등록된 인증서에 대한 등록 항목 업데이트는 **등록 관리** 페이지에서 수행됩니다. 이 페이지에 액세스하려면 다음 단계를 따르세요.

1. [Azure Portal](https://portal.azure.com)에 로그인하고 장치에 대한 등록 항목이 있는 IoT Hub Device Provisioning Service 인스턴스로 이동합니다.

2. **등록 관리**를 클릭합니다.

    ![등록 관리](./media/how-to-roll-certificates/manage-enrollments-portal.png)


등록 항목 업데이트의 처리 방법은 개별 등록 또는 그룹 등록의 사용 여부에 따라 다릅니다. 또한 권장 절차도 인증서 배포의 원인이 보안 위반인가 또는 인증서 만료인가에 따라 달라집니다. 다음 섹션에서는 이러한 업데이트의 처리 방법을 설명합니다.


## <a name="individual-enrollments-and-security-breaches"></a>개별 등록 및 보안 위반

보안 위반에 대응하여 인증서를 배포하는 경우 현재 인증서를 즉시 삭제하는 다음 방법을 사용해야 합니다.

1. **개별 등록**을 클릭하고 목록에서 등록 ID 항목을 클릭합니다. 

2. **현재 인증서 삭제** 단추를 클릭한 다음, 폴더 아이콘을 클릭하여 등록 항목에 대해 업로드할 새 인증서를 선택합니다. 완료되면 **저장**을 클릭합니다.

    기본 인증서와 보조 인증서가 모두 손상된 경우 두 인증서 모두에 대해 이 단계를 완료해야 합니다.

    ![개별 등록 관리](./media/how-to-roll-certificates/manage-individual-enrollments-portal.png)

3. 손상된 인증서를 프로비전 서비스에서 제거한 후에는 IoT hub로 이동하고 손상된 인증서와 연결된 장치 등록을 제거합니다.     

    ![IoT hub 장치 등록 제거](./media/how-to-roll-certificates/remove-hub-device-registration.png)


## <a name="individual-enrollments-and-certificate-expiration"></a>개별 등록 및 인증서 만료

인증서 만료를 처리하기 위해 인증서를 배포하는 경우 다음과 같이 보조 인증서 구성을 사용하여 프로비전을 수행하는 장치에서 가동 중지 시간을 줄입니다.

나중에 보조 인증서도 만료 시점이 가까워져 배포가 필요한 경우 기본 구성을 사용하여 회전할 수 있습니다. 기본 및 보조 인증서를 이런 식으로 회전하면 프로비전을 수행하는 장치에서 가동 중지 시간이 줄어듭니다.


1. **개별 등록**을 클릭하고 목록에서 등록 ID 항목을 클릭합니다. 

2. **보조 인증서**를 클릭한 다음, 폴더 아이콘을 클릭하여 등록 항목에 대해 업로드할 새 인증서를 선택합니다. **저장**을 클릭합니다.

    ![보조 인증서를 사용하여 개별 등록 관리](./media/how-to-roll-certificates/manage-individual-enrollments-secondary-portal.png)

3. 나중에 기본 인증서가 만료되면 다시 돌아와 **현재 인증서 삭제** 단추를 클릭하여 기본 인증서를 삭제합니다.

## <a name="enrollment-groups-and-security-breaches"></a>등록 그룹 및 보안 위반

보안 위반에 대응하여 그룹 등록을 업데이트하려면 현재 루트 CA 또는 중간 인증서를 즉시 삭제하는 다음 방법 중 하나를 사용해야 합니다.

#### <a name="update-compromised-root-ca-certificates"></a>손상된 루트 CA 인증서 업데이트

1. 프로비저닝 서비스 인스턴스에 대해 **인증서** 탭을 클릭합니다.

2. 목록에서 손상된 인증서를 클릭한 다음, **삭제** 단추를 클릭합니다. 인증서 이름을 입력하여 삭제를 확인하고 **확인**을 클릭합니다. 손상된 모든 인증서에 대해 이 프로세스를 반복합니다.

    ![루트 CA 인증서 삭제](./media/how-to-roll-certificates/delete-root-cert.png)

3. [확인된 CA 인증서 구성](how-to-verify-certificates.md)에서 설명한 단계에 따라 새 루트 CA 인증서를 추가 및 확인합니다.

4. 프로비전 서비스 인스턴스에 대한 **등록 관리** 탭을 클릭하고 **등록 그룹** 목록을 클릭합니다. 목록에서 등록 그룹 이름을 클릭합니다.

5. **CA 인증서**를 클릭하고 새 루트 CA 인증서를 선택합니다. 그런 다음 **Save**를 클릭합니다. 

    ![새 루트 CA 인증서 선택](./media/how-to-roll-certificates/select-new-root-cert.png)

6. 손상된 인증서를 프로비전 서비스에서 제거한 후에는 손상된 장치 등록이 포함된 연결된 IoT hub로 이동하고 손상된 인증서와 연결된 등록을 제거합니다.

    ![IoT hub 장치 등록 제거](./media/how-to-roll-certificates/remove-hub-device-registration.png)


#### <a name="update-compromised-intermediate-certificates"></a>손상된 중간 인증서 업데이트 

1. **등록 그룹**을 클릭한 다음, 목록에서 그룹 이름을 클릭합니다. 

2. **중간 인증서** 및 **현재 인증서 삭제**를 클릭합니다. 폴더 아이콘을 클릭하고 등록 그룹에 대해 업로드할 새 중간 인증서로 이동합니다. 완료하면 **저장**을 클릭합니다. 기본 인증서와 보조 인증서가 모두 손상된 경우 두 인증서 모두에 대해 이 단계를 완료해야 합니다.

    이 새 중간 인증서는 이미 프로비전 서비스에 추가된 인증된 루트 CA 인증서로 서명해야 합니다. 자세한 내용은 [X.509 인증서](concepts-security.md#x509-certificates)를 참조하세요.

    ![개별 등록 관리](./media/how-to-roll-certificates/enrollment-group-delete-intermediate-cert.png)


3. 손상된 인증서를 프로비전 서비스에서 제거한 후에는 장치 등록이 포함된 연결된 IoT hub로 이동하고 손상된 인증서와 연결된 등록을 제거합니다.

    ![IoT hub 장치 등록 제거](./media/how-to-roll-certificates/remove-hub-device-registration.png)


## <a name="enrollment-groups-and-certificate-expiration"></a>등록 그룹 및 인증서 만료

인증서 만료를 처리하기 위해 인증서를 배포하는 경우 다음과 같이 보조 인증서 구성을 사용하여 프로비전을 수행하는 장치에 가동 중지 시간이 없도록 합니다.

나중에 보조 인증서도 만료 시점이 가까워져 배포가 필요한 경우 기본 구성을 사용하여 회전할 수 있습니다. 기본 및 보조 인증서를 이런 식으로 회전하면 프로비전을 수행하는 장치에서 가동 중지 시간이 없게 됩니다. 

#### <a name="update-expiring-root-ca-certificates"></a>만료 루트 CA 인증서 업데이트 

1. [확인된 CA 인증서 구성](how-to-verify-certificates.md)에서 설명한 단계에 따라 새 루트 CA 인증서를 추가 및 확인합니다.

2. 프로비전 서비스 인스턴스에 대한 **등록 관리** 탭을 클릭하고 **등록 그룹** 목록을 클릭합니다. 목록에서 등록 그룹 이름을 클릭합니다.

3. **CA 인증서**를 클릭하고 **보조 인증서** 구성에서 새 루트 CA 인증서를 선택합니다. 그런 다음 **Save**를 클릭합니다. 

    ![새 루트 CA 인증서 선택](./media/how-to-roll-certificates/select-new-root-secondary-cert.png)

4. 나중에 기본 인증서가 만료되면 프로비전 서비스 인스턴스에 대한 **인증서** 탭을 클릭합니다. 목록에서 만료된 인증서를 클릭한 다음, **삭제** 단추를 클릭합니다. 인증서 이름을 입력하여 삭제를 확인하고 **확인**을 클릭합니다.

    ![루트 CA 인증서 삭제](./media/how-to-roll-certificates/delete-root-cert.png)



#### <a name="update-expiring-intermediate-certificates"></a>만료된 중간 인증서 업데이트


1. **등록 그룹**을 클릭하고 목록에서 그룹 이름을 클릭합니다. 

2. **보조 인증서**를 클릭한 다음, 폴더 아이콘을 클릭하여 등록 항목에 대해 업로드할 새 인증서를 선택합니다. **저장**을 클릭합니다.

    이 새 중간 인증서는 이미 프로비전 서비스에 추가된 인증된 루트 CA 인증서로 서명해야 합니다. 자세한 내용은 [X.509 인증서](concepts-security.md#x509-certificates)를 참조하세요.

   ![보조 인증서를 사용하여 개별 등록 관리](./media/how-to-roll-certificates/manage-enrollment-group-secondary-portal.png)

3. 나중에 기본 인증서가 만료되면 다시 돌아와 **현재 인증서 삭제** 단추를 클릭하여 기본 인증서를 삭제합니다.


## <a name="reprovision-the-device"></a>장치 다시 프로비전

인증서가 장치와 Device Provisioning Service에 모두 배포된 후에는 장치가 Device Provisioning Service에 연결하여 스스로 다시 프로비전할 수 있습니다. 

장치가 다시 프로비전하도록 프로그래밍하는 한 가지 쉬운 방법은 장치가 IoT hub 연결을 시도하다가 "권한 없음" 오류를 수신하면 프로비전 서비스에 연결하여 프로비전 흐름을 진행하게 프로그래밍하는 것입니다.

다른 방법은 구 인증서와 새 인증서를 모두 짧게 겹치는 시간 동안 유효하게 하고 IoT hub를 사용하여 프로비전 서비스를 통해 스스로를 다시 등록하여 IoT Hub 연결 정보를 업데이트하는 명령을 장치에게 보내는 것입니다. 각 장치는 서로 다른 명령을 처리할 수 있으므로 명령이 호출되었을 때 수행할 작업을 장치가 알 수 있게 프로그래밍해야 합니다. 여러 가지 방법으로 IoT Hub를 통해 장치에 명령할 수 있고, [직접 메서드](../iot-hub/iot-hub-devguide-direct-methods.md) 또는 [작업](../iot-hub/iot-hub-devguide-jobs.md)을 통해 프로세스를 시작하는 것이 좋습니다.

다시 프로비전이 완료되면 장치가 새 인증서를 사용하여 IoT Hub에 연결할 수 있습니다.


## <a name="blacklist-certificates"></a>인증서 블랙리스트 처리

보안 위반에 대한 대처로 장치 인증서를 블랙리스트 처리해야 할 수 있습니다. 장치 인증서를 블랙리스트 처리하려면 대상 장치/인증서에 대한 등록 항목을 사용하지 않게 설정합니다. 자세한 내용은 [등록 취소 관리](how-to-revoke-device-access-portal.md) 문서의 장치 블랙리스트 처리를 참조하세요.

인증서가 사용하지 않는 등록 항목의 일부가 되면 다른 등록 항목의 일부로 사용하게 설정되었다 하더라도 해당 인증서를 사용하는 IoT Hub를 통한 모든 등록 시도가 실패합니다.
 



## <a name="next-steps"></a>다음 단계

- Device Provisioning Service의 X.509 인증서에 대한 자세한 내용은 [보안](concepts-security.md)을 참조하세요. 
- Azure IoT Hub Device Provisioning Service를 사용하여 X.509 CA 인증서에 대해 소유 증명을 수행하는 방법은 [인증서 확인 방법](how-to-verify-certificates.md)을 참조하세요.
- 포털을 사용하여 등록 그룹을 만드는 방법에 대한 자세한 내용은 [Azure Portal에서 장치 등록 관리](how-to-manage-enrollments.md)를 참조하세요.









