---
title: CloudSimple 서비스에 대 한 보안
description: CloudSimple 서비스 보안에 대 한 공유 책임 모델을 설명합니다.
author: sharaths-cs
ms.author: b-shsury
ms.date: 04/27/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 325915aae43c905236910382f650730a6daa127a
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67595333"
---
# <a name="cloudsimple-security-overview"></a>CloudSimple 보안 개요

이 문서에서는 CloudSimple 서비스, 인프라 및 데이터 센터에서 보안 Azure VMware 솔루션을 구현 하는 방법에 대 한 개요를 제공 합니다. 데이터 보호 및 보안, 네트워크 보안 및 취약점 및 패치 관리 되는 방법을 알아봅니다.

## <a name="shared-responsibility"></a>공동 책임

CloudSimple 하 여 azure VMware 솔루션 보안에 대 한 공유 책임 모델을 사용합니다. 클라우드에서 신뢰할 수 있는 보안 서비스 공급자 고객 및 Microsoft의 공유 책임을 통해 이루어집니다. 이 매트릭스 책임의 더 높은 보안을 제공 하 고 단일 실패 지점을 제거 합니다.

## <a name="azure-infrastructure"></a>Azure 인프라 

Azure 인프라 보안 고려 사항 장비와 데이터 센터 위치를 포함 합니다.

### <a name="datacenter-security"></a>데이터 센터 보안 

Microsoft 디자인, 빌드 및 Azure를 지 원하는 물리적 시설을 운영에 소요 되는 전체 부서를 있습니다. 이 팀은 최첨단의 물리적 보안을 유지 관리하는 데 투입되었습니다. 물리적 보안에 대 한 자세한 내용은 참조 하세요. [Azure 기능, 온 프레미스, 및 물리적 보안](https://docs.microsoft.com/azure/security/azure-physical-security)합니다.

### <a name="equipment-location"></a>장치 위치

사설 클라우드를 실행 하는 베어 메탈 하드웨어 장비는 Azure 데이터 센터 위치에서 호스트 됩니다. 생체 인식 기반 2 단계 인증이 필요 해당 장비 있는 보관소에 액세스할 수 있습니다.

## <a name="dedicated-hardware"></a>전용 하드웨어

CloudSimple 서비스의 일부로 모든 CloudSimple 고객이 다른 테 넌 트 하드웨어에서 물리적으로 격리 된 로컬 연결 된 디스크를 사용 하 여 전용된 베어 메탈 호스트를 가져옵니다. VSAN 사용 하 여 ESXi 하이퍼바이저는 모든 노드에서 실행 됩니다. 노드는 고객 전용 VMware vCenter에서 고 NSX 통해 관리 됩니다. 테 넌 트 간에 하드웨어를 공유 하지 않는 격리 및 보안 보호 추가 계층을 제공 합니다.

## <a name="data-security"></a>데이터 보안

고객은 컨트롤 및 해당 데이터의 소유권을 유지합니다. 고객 데이터의 데이터 (영문)은 고객의 책임입니다.

### <a name="data-protection-for-data-at-rest-and-data-in-motion-within-internal-networks"></a>미사용 데이터 및 내부 네트워크 내에서 미사용 데이터에 대 한 데이터 보호

사설 클라우드 환경에서 미사용 데이터에 대 한 vSAN 암호화를 사용할 수 있습니다. vSAN 암호화 사용자 고유의 가상 네트워크 또는 온-프레미스 VMware certified 외부 키 관리 서버 (KMS)를 사용 하 여 작동합니다. 데이터 암호화 키를 직접 제어. 사설 클라우드 내의 동작 중인 데이터에 대 한 vSphere vMotion 트래픽을 포함 하는 모든 VMkernel 트래픽에 대 한 연결을 통해 데이터의 암호화를 지원 합니다.

### <a name="data-protection-for-data-thats-required-to-move-through-public-networks"></a>공용 네트워크를 통해 이동 하는 데 필요한 데이터에 대 한 데이터 보호

공용 네트워크를 통해 이동 하는 데이터를 보호 하려면 만들면 IPsec 및 SSL VPN 터널 사설 클라우드에 대 한 합니다. 128 비트 및 256 비트 AES를 포함 하 여 일반적인 암호화 메서드는 지원 됩니다. 인증, 액세스 관리 및 고객 데이터를 포함 하는 전송 중인 데이터의에서 보안 RDP, SSH 및 TLS 1.2 등의 표준 암호화 메커니즘을 사용 하 여 암호화 됩니다. 중요 한 정보를 전송 하는 통신은 표준 암호화 메커니즘을 사용 합니다.

### <a name="secure-disposal"></a>안전한 폐기 

CloudSimple 서비스 만료 또는 종료 될 경우 제거 하거나 데이터를 삭제 하는 일을 담당 합니다. CloudSimple 협력 하 여를 삭제 하거나 범위 CloudSimple 반드시 해당 법규에서 일부 또는 모든 개인 데이터를 유지 하는 점을 제외 하 고 고객 계약에서 제공 된 모든 고객 데이터를 반환 합니다. 모든 개인 데이터를 보존 하려면 필요한 경우 CloudSimple 데이터를 보관 하 고 고객 데이터를 더 이상 처리 하지 않도록 설정 하려면 적절 한 조치를 구현 합니다.

### <a name="data-location"></a>데이터 위치

사설 클라우드를 설정 하는 배포 된 Azure 지역을 선택 합니다. VMware 가상 컴퓨터 데이터는 데이터 마이그레이션 또는 오프 사이트 데이터 백업을 수행 하지 않는 한 해당 물리적 데이터 센터에서 이동 하지 않습니다. 또한 워크 로드를 호스트할를 요구 사항에 해당 하는 경우 여러 Azure 지역 내에 데이터를 저장 합니다.

사설 클라우드 하이퍼 수렴 형 노드에 상주 하는 고객 데이터 위치 테 넌 트 관리자의 명시적 작업 없이 트래버스 하지 않습니다. 것이 항상 사용 가능한 방식으로 워크 로드를 구현 해야 합니다.

### <a name="data-backups"></a>데이터 백업
CloudSimple 백업 하거나 고객 데이터를 보관 하지 않습니다. CloudSimple 관리 서버의 고가용성을 제공 하는 vCenter 및 NSX 데이터의 정기적인 백업을 수행지 않습니다. 백업 하기 전에 VMware Api를 사용 하 여 vCenter 소스의 모든 데이터가 암호화 됩니다. 암호화 된 데이터 전송 되며 Azure blob에 저장 됩니다. 매우 안전한 CloudSimple 관리 되는 자격 증명 모음에 Azure에서 CloudSimple virtual network에서 실행 되는 백업에 대 한 암호화 키 저장 됩니다.

## <a name="network-security"></a>네트워크 보안

CloudSimple 솔루션에서는 네트워크 보안 계층을 사용 합니다.

### <a name="azure-edge-security"></a>Azure 경계 보안

CloudSimple 서비스는 Azure에서 제공 하는 기본 네트워크 보안을 기반으로 합니다. Azure는 탐지 및 비정상적인 수신 또는 송신 트래픽 패턴 및 분산된 서비스 거부 (DDoS) 공격을 사용 하 여 연결 된 네트워크 기반 공격에 시기 적절 하 게 대응 하는 심층적인 방어 기술을 적용 합니다. 이 보안 제어는 사설 클라우드 환경과 CloudSimple 개발한 제어 평면 소프트웨어에 적용 됩니다.

### <a name="segmentation"></a>분할

CloudSimple 서비스에는 사설 클라우드 환경의 고유한 개인 네트워크에 대 한 액세스를 제한 하는 논리적으로 분리 계층 2 네트워크에 있습니다. 추가 방화벽을 사용 하 여 사설 클라우드 네트워크를 보호할 수 있습니다. CloudSimple 포털 간 사설 클라우드 트래픽과 간 사설 클라우드 트래픽, 일반 인터넷 트래픽을 온-프레미스 네트워크 트래픽을 포함 하는 모든 네트워크 트래픽에 대해 동-서 및 북-남 네트워크 트래픽 제어 규칙 정의 IPsec VPN 또는 Azure ExpressRoute 연결.

## <a name="vulnerability-and-patch-management"></a>취약점으로 인 한 및 패치 관리 

CloudSimple는 정기 보안 NSX, ESXi 및 vCenter 등의 관리 되는 VMware 소프트웨어의 패치 하는 일을 담당 합니다.

## <a name="identity-and-access-management"></a>ID 및 액세스 관리

고객은 기본 설정으로 다단계 인증 또는 SSO를 사용 하 여 (Azure Active Directory)에서 Azure 계정에 인증할 수 있습니다. Azure portal에서 자격 증명을 다시 입력 하지 않고도 CloudSimple 포털을 시작할 수 있습니다.

CloudSimple 사설 클라우드 vCenter에 대 한 id 소스는 선택적 구성의 지원합니다. 사용할 수는 [온-프레미스 id 소스](https://docs.azure.cloudsimple.com/set-vcenter-identity), 사설 클라우드에 대 한 새 identity 소스 또는 [Azure Active Directory](https://docs.azure.cloudsimple.com/azure-ad)합니다.

기본적으로 고객에 게는 사설 클라우드 내의 vCenter의 일상적인 작업에 대 한 필요한 권한이 제공 됩니다. 이 권한 수준은 vCenter에 대 한 관리 액세스를 포함 하지 않습니다. 관리 액세스가 일시적으로 필요한 경우 있습니다 [귀하의 권한을 에스컬레이션](https://docs.azure.cloudsimple.com/escalate-private-cloud-privileges) 관리 작업을 완료 하는 동안 제한 된 기간에 대 한 합니다.

## <a name="next-steps"></a>다음 단계

* 에 대해 알아봅니다 하는 방법 [Azure에서 CloudSimple 서비스를 만들](quickstart-create-cloudsimple-service.md)합니다.
* 설명 하는 방법 [사설 클라우드 만들기](https://docs.azure.cloudsimple.com/create-private-cloud/)합니다.
* 설명 하는 방법 [사설 클라우드 환경을 구성](quickstart-create-private-cloud.md)합니다.
