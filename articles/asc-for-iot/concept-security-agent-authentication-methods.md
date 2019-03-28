---
title: ASC IoT 미리 보기에 대 한 인증 방법을 | Microsoft Docs
description: IoT 서비스에 대 한 ASC를 사용 하는 경우 사용 가능한 다른 인증 방법에 알아봅니다.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 10b38f20-b755-48cc-8a88-69828c17a108
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 23bc4d0df1c8124ec225ac31239c7acb3f1ab546
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541814"
---
# <a name="security-agent-authentication-methods"></a>보안 에이전트 인증 방법 

> [!IMPORTANT]
> IoT 용 ASC는 현재 공개 미리 보기로 제공 됩니다.
> 이 미리 보기 버전을 서비스 수준 계약 없이 제공 됩니다 및 프로덕션 워크 로드에 권장 되지 않습니다. 특정 기능이 지원되지 않거나 기능이 제한될 수 있습니다. 자세한 내용은 [Microsoft Azure Preview에 대한 추가 사용 약관](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)을 참조하세요.

이 문서에서는 IoT Hub를 사용 하 여 인증 하려면 AzureIoTSecurity 에이전트를 사용 하 여 사용할 수 있습니다 다른 인증 방법을 설명 합니다.

IoT Hub에 IoT ASC에 각 장치 등록, 보안 모듈이 필요 합니다. 장치를 인증 하려면 ASC IoT에 대 한 두 가지 방법 중 하나를 사용할 수 있습니다. 기존 IoT 솔루션에 가장 적합 한 방법을 선택 합니다. 

> [!div class="checklist"]
> * 보안 모듈 옵션
> * 장치 옵션

## <a name="authentication-methods"></a>인증 방법

인증을 수행 하도록 AzureIoTSecurity 에이전트는 두 가지 방법:

 - **모듈** 인증 모드<br>
   모듈은 장치 쌍 독립적으로 인증 됩니다.
   이 유형의 인증에 대 한 Authentication.config 파일에 정의 되어에 필요한 정보를 C# 및 C에 대 한 LocalConfiguration.json
        
 - **장치** 인증 모드<br>
    이 방법에서는 보안 에이전트가 먼저 장치에 대해 인증합니다. IoT 에이전트용 ASC는 초기 인증을 받은 후 다음을 수행 합니다. **Rest** Rest API를 사용 하 여 장치 인증 데이터를 사용 하 여 IoT Hub를 호출 합니다. 그런 다음 ASC IoT 에이전트에 대 한 IoT Hub에서 보안 모듈 인증 방법 및 데이터를 요청합니다. 마지막 단계로, IoT 에이전트용 ASC IoT 모듈에 대 한 ASC에 대 한 인증을 수행합니다.    

참조 [보안 에이전트 설치 매개 변수](#security-agent-installation-parameters) 를 구성 하는 방법을 알아봅니다.
                                
## <a name="authentication-methods-known-limitations"></a>알려진 제한 하는 인증 방법

- **모듈** 인증 모드는 대칭 키 인증만 지원 합니다.
- CA 서명 인증서에서 지원 되지 않습니다 **장치** 인증 모드입니다.  

## <a name="security-agent-installation-parameters"></a>보안 에이전트 설치 매개 변수

때 [보안 에이전트 배포](select-deploy-agent.md), 인수로 인증 세부 정보를 제공 해야 합니다.
이러한 인수는 다음 표에 나와 있습니다.


|매개 변수|설명|옵션|
|---------|---------------|---------------|
|**identity**|인증 모드| **모듈** 또는 **장치**|
|**type**|인증 유형|**SymmetricKey** 또는 **SelfSignedCertificate**|
|**filePath**|인증서 또는 대칭 키를 포함 하는 파일에 대 한 전체 경로 절대| |
|**gatewayHostname**|IoT Hub의 FQDN|예제: ContosoIotHub.azure-devices.net|
|**deviceId**|디바이스 ID|예제: MyDevice1|
|**certificateLocationKind**|인증서 저장소 위치|**LocalFile** 또는 **저장소**|


보안 에이전트 설치 스크립트를 사용 하는 경우 다음 구성은 자동으로 수행 됩니다.

보안 에이전트 인증을 수동으로 편집 하려면 구성 파일을 편집 합니다. 

## <a name="change-authentication-method-after-deployment"></a>배포 후 인증 메서드 변경

설치 스크립트를 사용 하 여 보안 에이전트를 배포, 구성 파일을 자동으로 생성 됩니다.

배포 후 인증 메서드를 변경 하려면 구성 파일을 수동으로 편집할 필요 합니다.


### <a name="c-based-security-agent"></a>C#-보안 에이전트 기반

편집할 _Authentication.config_ 다음 매개 변수를 사용 하 여:

```xml
<Authentication>
  <add key="deviceId" value=""/>
  <add key="gatewayHostname" value=""/>
  <add key="filePath" value=""/>
  <add key="type" value=""/>
  <add key="identity" value=""/>
  <add key="certificateLocationKind" value="" />
</Authentication>
```

### <a name="c-based-security-agent"></a>C 기반 보안 에이전트

편집할 _LocalConfiguration.json_ 다음 매개 변수를 사용 하 여:

```json
"Authentication" : {
    "Identity" : "",
    "AuthenticationMethod" : "",
    "FilePath" : "",
    "DeviceId" : "",
    "HostName" : ""
}
```

## <a name="see-also"></a>참고 항목
- [보안 에이전트 개요](security-agent-architecture.md)
- [보안 에이전트 배포](select-deploy-agent.md)
- [원시 보안 데이터에 액세스](how-to-security-data-access.md)