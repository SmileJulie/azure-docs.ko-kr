---
title: 포함 파일
description: 포함 파일
services: virtual-machines
author: vermagit
ms.service: virtual-machines
ms.topic: include
ms.date: 05/29/2018
ms.author: azcspmt;jonbeck;cynthn;danlep;amverma
ms.custom: include file
ms.openlocfilehash: 88d9491bae1fcc6acb7ab631514aa4f6244f593d
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538049"
---
## <a name="deployment-considerations"></a>배포 고려 사항
* **Azure 구독** - 몇몇 계산 집약적 인스턴스를 배포하려면 종량제 구독 또는 기타 구매 옵션을 고려합니다. [Azure 무료 계정](https://azure.microsoft.com/free/)을 사용하는 경우, 제한된 수의 Azure 계산 코어만 사용할 수 있습니다.

* **가격 책정 및 가용성** - 이러한 VM 크기는 표준 가격 책정 계층에만 제공됩니다. [지역별 사용 가능한 제품](https://azure.microsoft.com/global-infrastructure/services/) 에서 Azure 지역의 가용성을 확인하세요. 
* **코어 할당량** – 기본값에서 Azure 구독의 코어 할당량을 늘려야 합니다. 구독에 따라서도 H 시리즈를 포함하여 특정 VM 크기 제품군에 배포할 수 있는 코어 수가 제한될 수 있습니다. 할당량 증가를 요청하려면 무료로 [온라인 고객 지원 요청을 개설](../articles/azure-supportability/how-to-create-azure-support-request.md) 합니다. (기본 제한은 구독 범주에 따라 달라질 수 있습니다.)
  
  > [!NOTE]
  > 대규모 용량이 필요한 경우 Azure 지원에 문의합니다. Azure 할당량은 신용 제한이며 용량 보증이 아닙니다. 할당량에 관계 없이 사용하는 코어에 대해서만 요금이 청구됩니다.
  > 
  > 
* **가상 네트워크** - Azure [가상 네트워크](https://azure.microsoft.com/documentation/services/virtual-network/) 는 계산 집약적 인스턴스를 사용할 필요가 없습니다. 하지만 많은 배포에서 온-프레미스 리소스에 액세스해야 하는 경우 적어도 클라우드 기반 Azure 가상 네트워크 또는 사이트 간 연결이 필요합니다. 필요한 경우 인스턴스를 배포할 새 가상 네트워크를 만듭니다. 선호도 그룹에서 가상 네트워크에 계산 집약적 VM을 추가하는 것은 지원되지 않습니다.
* **크기 조정** - 특수한 하드웨어로 인해 동일한 크기의 제품군(H 시리즈 또는 계산 집약적 A 시리즈) 내에서만 계산 집약적 인스턴스의 크기를 조정할 수 있습니다. 예를 들어 H 시리즈 VM 크기는 한 H 시리즈에서 다른 H 시리즈로만 조정할 수 있습니다. 또한 계산 집약적이지 않은 크기에서 계산 집약적 크기로 조정하는 것은 지원되지 않습니다.  

## <a name="rdma-capable-instances"></a>RDMA 지원 인스턴스
계산 집약적인 인스턴스 (A8, A9, H16r, H16mr, HB 및 HC)의 하위 집합에는 원격 직접 메모리 액세스 (RDMA) 연결에 대 한 네트워크 인터페이스를 기능입니다. NC24rs 구성 (NC24rs_v2 및 NC24rs_v3) 같은 'r'를 사용 하 여 지정 된 N 시리즈 크기를 선택한 RDMA 지원 됩니다. 이 인터페이스는 다른 VM 크기에서 사용할 수 있는 표준 Azure 네트워크 인터페이스 외에 추가로 사용됩니다. 
  
이 인터페이스는 RDMA 지원 인스턴스가 H16r, H16mr 및 RDMA 지원 N 시리즈 가상 머신에 대 한 요금과 A8 및 A9 가상 머신에서 QDR 속도로 HB HC, FDR EDR 요금 운영 IB (InfiniBand) 네트워크를 통해 통신할 수 있습니다. 이러한 RDMA 기능은 특정 MPI(Message Passing Interface) 애플리케이션의 확장성 및 성능을 향상시킬 수 있습니다. 속도에 대 한 자세한 내용은이 페이지에서 테이블의 세부 정보를 참조 하세요.

> [!NOTE]
> Azure에서 IP over IB 에서만 SR-IOV 사용 (SR-IOV InfiniBand, 현재 HB 및 HC) Vm. RDMA over IB 모든 RDMA 지원 인스턴스가 지원 됩니다.
>

