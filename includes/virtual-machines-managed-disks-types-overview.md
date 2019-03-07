---
title: 포함 파일
description: 포함 파일
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/22/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 8a067474fb172d4ff7a7fdf7eb6d24536bd2d017
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/20/2019
ms.locfileid: "56443349"
---
# <a name="what-disk-types-are-available-in-azure"></a>Azure에서 사용할 수 있는 디스크 유형

Azure 관리 디스크는 현재 네 개의 디스크 유형을 제공하고 있습니다. 이 중 세 개는 GA(일반 공급)되며, 나머지 하나는 현재 미리 보기에 있습니다. 이러한 네 개의 디스크 유형에는 각각 적절한 대상 고객 시나리오가 있습니다.

## <a name="disk-comparison"></a>디스크 비교

다음 표에서는 사용할 디스크를 결정하는 데 도움을 주기 위해 관리 디스크로 사용할 수 있는 울트라 SSD(반도체 드라이브)(미리 보기), 프리미엄 SSD, 표준 SSD 및 표준 HDD(하드 디스크 드라이브)를 비교하고 있습니다.

|   | 울트라 SSD(미리 보기)   | 프리미엄 SSD   | 표준 SSD   | 표준 HDD   |
|---------|---------|---------|---------|---------|
|디스크 유형   |SSD   |SSD   |SSD   |HDD   |
|시나리오   |IO 집약적 워크로드 - SAP HANA, 최상위 계층 데이터베이스(예: SQL, Oracle) 및 다른 트랜잭션 집약적 워크로드   |프로덕션 및 성능이 중요한 워크로드   |웹 서버, 적게 사용되는 엔터프라이즈 애플리케이션 및 개발/테스트   |백업, 중요하지 않음, 가끔 액세스   |
|디스크 크기   |65,536GiB(기비바이트)(미리 보기)   |4,095GiB(GA), 32,767GiB(미리 보기)    |4,095GiB(GA), 32,767GiB(미리 보기)   |4,095GiB(GA), 32,767GiB(미리 보기)   |
|최대 처리량   |2,000MiB/s(미리 보기)   |250MiB/s(GA), 750MiB/s(미리 보기)   |60MiB/s(GA), 500MiB/s(미리 보기)   |60MiB/s(GA), 500MiB/s(미리 보기)   |
|최대 IOPS   |160,000(미리 보기)   |7,500(GA), 20,000(미리 보기)   |500(GA), 2,000(미리 보기)   |500(GA), 2,000(미리 보기)   |

## <a name="ultra-ssd-preview"></a>울트라 SSD(미리 보기)

Azure 울트라 SSD(미리 보기)는 Azure IaaS VM에 대한 높은 처리량, 높은 IOPS 및 일관된 짧은 대기 시간 디스크 스토리지를 제공합니다. 울트라 SSD의 몇 가지 추가 이점에는 가상 머신을 다시 시작하지 않고도 워크로드와 함께 디스크의 성능을 동적으로 변경할 수 있다는 것이 포함됩니다. 울트라 SSD는 SAP HANA, 최상위 계층 데이터베이스 및 트랜잭션 집약적 워크로드와 같은 데이터 집약적 워크로드에 적합합니다. 울트라 SSD는 데이터 디스크로만 사용할 수 있습니다. 프리미엄 SSD는 OS 디스크로 사용하는 것이 좋습니다.

### <a name="performance"></a>성능

울트라 SSD를 프로비저닝할 때 디스크의 용량과 성능을 독립적으로 구성할 수 있습니다. 울트라 SSD는 4GiB~64TiB의 몇 가지 고정된 크기로 제공되며, IOPS와 처리량을 독립적으로 구성할 수 있는 유연한 성능 구성 모델을 갖추고 있습니다.

Ultra SSD의 일부 키 기능은 다음과 같습니다.

- 디스크 용량: 울트라 SSD의 용량은 4GiB에서 최대 64TiB입니다.
- 디스크 IOPS: 울트라 SSD는 300IOPS/GiB의 IOPS 제한을 지원하며, 디스크당 최대 160K IOPS입니다. 프로비전한 IOPS를 달성하려면 선택한 디스크 IOPS가 VM IOPS보다 작은지 확인합니다. 최소 디스크 IOPS는 100IOPS입니다.
- 디스크 처리량: 울트라 SSD를 사용하는 경우 단일 디스크의 처리량 한도는 프로비저닝된 IOPS당 256KiB/s이며, 디스크당 최대 2,000MBps입니다(MBps = 초당 10^6바이트). 최소 디스크 처리량은 1MiB입니다.

### <a name="disk-size"></a>디스크 크기

|디스크 크기(GiB)  |IOPS 용량  |처리량 용량(MBps)  |
|---------|---------|---------|
|4     |1,200         |300         |
|8     |2,400         |600         |
|16     |4,800         |1,200         |
|32     |9,600         |2,000         |
|64     |19,200         |2,000         |
|128     |38,400         |2,000         |
|256     |76,800         |2,000         |
|512     |80,000         |2,000         |
|1,024-65,536(1TiB의 단위로 증가하는 이 범위의 크기)     |160,000         |2,000         |

### <a name="preview-scope-and-limitations"></a>미리 보기 범위 및 제한 사항

미리 보기 동안 울트라 SSD는 다음과 같습니다.

- 미국 동부 2의 단일 가용성 영역에서 지원됨  
- 가용성 영역에서만 사용할 수 있음(영역 외부의 가용성 집합 및 단일 VM 배포에는 울트라 디스크를 연결하는 기능이 없음)
- ES/DS v3 VM에서만 지원됨
- 데이터 디스크로만 사용 가능하며 4k 물리적 섹터 크기만 지원함  
- 빈 디스크로만 만들 수 있음  
- 현재 Azure Resource Manager 템플릿, CLI 및 Python SDK를 통해서만 배포할 수 있습니다.
- 디스크 스냅숏, VM 이미지, 가용성 집합, 가상 머신 확장 집합 및 Azure 디스크 암호화는 아직 지원하지 않습니다.
- Azure Backup 또는 Azure Site Recovery와의 통합은 아직 지원하지 않습니다.
-  [대부분의 미리 보기](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)와 마찬가지로, 이 기능은 GA(일반 공급) 이후에만 프로덕션 워크로드에서 사용할 수 있습니다.

## <a name="premium-ssd"></a>프리미엄 SSD

Azure 프리미엄 SSD는 IO(입출력) 집약적 워크로드가 있는 VM(가상 머신)에 대기 시간이 짧은 고성능 디스크를 지원합니다. 프리미엄 스토리지 디스크의 속도와 성능을 활용하기 위해 기존 VM 디스크를 프리미엄 SSD로 마이그레이션할 수 있습니다. 프리미엄 SSD는 중요 업무용 프로덕션 애플리케이션에 적합합니다.

### <a name="disk-size"></a>디스크 크기

별표로 표시된 크기는 현재 미리 보기에 있습니다.

| 프리미엄 SSD 크기  | P4               | P6               | P10             | P15 | P20              | P30              | P40              | P50              | P60*              | P70*              | P80*              |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| 디스크 크기(GiB)           | 32             | 64             | 128            | 256  | 512            | 1,024    | 2,048     | 4,095    | 8,192     | 16,384     | 32,767     |
| 디스크당 IOPS       | 최대 120 | 최대 240              | 최대 500              | 최대 1,100 | 최대 2,300              | 최대 5,000              | 최대 7,500             | 최대 7,500              | 최대 12,500              | 최대 15,000              | 최대 20,000              |
| 디스크당 처리량 | 최대 25MiB/초 | 최대 50MiB/초 | 최대 100MiB/초 | 최대 125MiB/초 | 최대 150MiB/초 | 최대 200MiB/초 | 최대 250MiB/초 | 최대 250MiB/초| 최대 480MiB/초 | 최대 750MiB/초 | 최대 750MiB/초 |

## <a name="standard-ssd"></a>표준 SSD

Azure 표준 SSD는 더 낮은 IOPS 수준에서 일관된 성능이 필요한 워크로드에 최적화된 비용 효율적 스토리지 옵션입니다. 특히 표준 SSD는 온-프레미스 HDD 솔루션에서 실행 중인 다양한 워크로드에 문제가 발생해서 클라우드로 전환하고자 하는 경우에 적합한 입문용 환경을 제공합니다. 표준 SSD는 HDD 디스크에 비해 더 나은 가용성, 일관성, 안정성 및 대기 시간을 제공합니다. 표준 SSD는 웹 서버, 낮은 IOPS 애플리케이션 서버, 가끔 사용되는 엔터프라이즈 애플리케이션 및 개발/테스트 워크로드에 적합합니다.

### <a name="disk-size"></a>디스크 크기

별표로 표시된 크기는 현재 미리 보기에 있습니다.

| 표준 SSD 크기  | E10               | E15               | E20             | E30 | E40              | E50              | E60*              | E70*              | E80*              |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| 디스크 크기(GiB)           | 128             | 256             | 512            | 1,024  | 2,048            | 4,095     | 8,192     | 16,384     | 32,767    |
| 디스크당 IOPS       | 최대 500              | 최대 500              | 최대 500              | 최대 500 | 최대 500              | 최대 500              | 최대 500             | 최대 500              | 최대 1,300              | 최대 2,000              | 최대 2,000              |
| 디스크당 처리량 | 최대 60MiB/초 | 최대 60MiB/초 | 최대 60MiB/초 | 최대 60MiB/초 | 최대 60MiB/초 | 최대 60MiB/초 | 최대 60MiB/초 | 최대 60MiB/초| 최대 300MiB/초 |  최대 500MiB/초 | 최대 500MiB/초 |

## <a name="standard-hdd"></a>표준 HDD

Azure 표준 HDD는 대기 시간에 민감하지 않은 워크로드를 실행하는 VM에 안정적이고 저렴한 디스크를 지원합니다. 또한 Blob, 테이블, 큐 및 파일을 지원합니다. 표준 스토리지를 사용하는 경우 데이터는 HDD(하드 디스크 드라이브)에 저장됩니다. VM을 사용할 때 표준 SSD 및 HDD 디스크는 개발/테스트 시나리오 및 비교적 중요하지 않은 워크로드에 사용할 수 있습니다. 표준 스토리지는 모든 Azure 지역에서 사용할 수 있습니다.

### <a name="disk-size"></a>디스크 크기

별표로 표시된 크기는 현재 미리 보기에 있습니다.

| 표준 디스크 유형  | S4               | S6               | S10             | S15 | S20              | S30              | S40              | S50              | S60*              | S70*              | S80*              |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| 디스크 크기(GiB)          | 32             | 64             | 128            | 256  | 512            | 1,024    | 2,048     | 4,095    | 8,192     | 16,384     | 32,767     |
| 디스크당 IOPS       | 최대 500              | 최대 500              | 최대 500              | 최대 500 | 최대 500              | 최대 500              | 최대 500             | 최대 500              | 최대 1,300              | 최대 2,000              | 최대 2,000              |
| 디스크당 처리량 | 최대 60MiB/초 | 최대 60MiB/초 | 최대 60MiB/초 | 최대 60MiB/초 | 최대 60MiB/초 | 최대 60MiB/초 | 최대 60MiB/초 | 최대 60MiB/초| 최대 300MiB/초 | 최대 500MiB/초 | 최대 500MiB/초 |

## <a name="billing"></a>결제

관리 디스크를 사용할 때 적용되는 청구 고려 사항은 다음과 같습니다.

- 디스크 유형
- 관리 디스크 크기
- 스냅숏
- 아웃바운드 데이터 전송
- 트랜잭션 수

**관리 디스크 크기:** 관리 디스크의 요금은 프로비저닝된 크기에 따라 청구됩니다. Azure에서 프로비저닝된 크기(반올림)는 가장 가깝게 제안되는 디스크 크기에 매핑됩니다. 제안되는 디스크 크기에 대한 자세한 내용은 이전 표를 참조하세요. 각 디스크는 지원되는 프로비저닝된 디스크 크기 제안에 매핑되고 이에 따라 요금이 청구됩니다. 예를 들어 200GiB 표준 SSD를 프로비저닝한 경우 E15(256GiB)의 디스크 크기 제안에 매핑됩니다. 프로비전된 디스크에 대한 청구는 Premium Storage 제품의 월별 가격을 사용하여 시간당 비례합니다. 예를 들어 E10 디스크를 프로비전하고 20시간 후 삭제한 경우 20시간에 비례하여 E10 제품에 대해 청구됩니다. 이는 디스크에 기록되는 실제 데이터 양에 관계없이 적용됩니다.

**스냅숏**: 스냅숏 요금은 사용된 크기에 따라 청구됩니다. 예를 들어 프로비저닝된 용량이 64GiB이고 실제 사용된 데이터 크기가 10GiB인 관리 디스크의 스냅숏을 만들면 사용된 10GiB의 데이터 크기에 대해서만 스냅숏 요금이 청구됩니다.