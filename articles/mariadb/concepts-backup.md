---
title: Azure Database for MariaDB의 백업 및 복원
description: Azure Database for MariaDB 서버를 자동 백업하고 복원하는 방법을 알아봅니다.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: c8e671071226b891668e0874d56e2d4685e1d64d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46958594"
---
# <a name="backup-and-restore-in-azure-database-for-mariadb"></a>Azure Database for MariaDB의 백업 및 복원

Azure Database for MariaDB는 자동으로 서버 백업을 만들어 사용자가 로컬로 구성한 중복 저장소 또는 지역 중복 저장소에 저장합니다. 백업을 사용하여 특정 시점의 서버를 복원할 수 있습니다. 백업 및 복원은 실수로 인한 손상이나 삭제로부터 데이터를 보호하므로 비즈니스 연속성 전략의 필수적인 부분입니다.

## <a name="backups"></a>Backup

Azure Database for MariaDB는 전체, 차등 및 트랜잭션 로그 백업을 수행합니다. 이러한 백업을 사용하면 서버를 구성된 백업 보존 기간 내의 특정 시점으로 복원할 수 있습니다. 기본 백업 보존 기간은 7일입니다. 필요에 따라 최대 35일까지 구성할 수 있습니다. 모든 백업은 AES 256비트 암호화를 사용하여 암호화됩니다.

### <a name="backup-frequency"></a>Backup 주기

일반적으로 전체 백업은 매주 수행되고, 차등 백업은 하루에 두 번 수행되며, 트랜잭션 로그 백업은 5분마다 수행됩니다. 첫 번째 전체 백업은 서버를 만든 직후에 예약됩니다. 초기 백업은 복원된 대형 서버에서 더 오래 걸릴 수 있습니다. 새 서버를 복원할 수 있는 가장 빠른 시점은 초기 전체 백업이 완료되는 시점입니다.

### <a name="backup-redundancy-options"></a>백업 중복 옵션

Azure Database for MariaDB는 범용 및 메모리 최적화 계층에서 로컬로 중복되거나 지리적으로 중복된 백업 저장소 중에서 선택할 수 있는 유연성을 제공합니다. 백업이 지역 중복 백업 저장소에 저장되면 서버가 호스팅되는 지역에 저장될 뿐만 아니라 [쌍으로 연결된 데이터 센터](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)에도 복제됩니다. 이렇게 하면 재해 발생 시 다른 지역에서 서버를 복원하는 데 더 효율적인 보호와 기능을 제공합니다. 기본 계층은 로컬 중복 백업 저장소만 제공합니다.

> [!IMPORTANT]
> 백업을 위한 로컬 중복 또는 지역 중복 저장소를 구성하는 것은 서버를 만드는 동안에만 허용됩니다. 서버가 프로비전되면 백업 저장소 중복 옵션을 변경할 수 없습니다.

### <a name="backup-storage-cost"></a>백업 저장소 비용

Azure Database for MariaDB는 추가 비용 없이 최대 100%의 프로비전된 서버 저장소를 백업 저장소로 제공합니다. 이는 일반적으로 7일의 백업 보존 기간에 적합합니다. 사용되는 추가 백업 저장소는 GB/월 단위로 청구됩니다.

예를 들어 250GB의 서버를 프로비전한 경우, 추가 비용 없이 250GB의 백업 저장소를 사용할 수 있습니다. 그러나 250GB를 초과하는 경우 저장소에 대한 요금이 청구됩니다.

백업 저장소 비용에 대한 자세한 내용은 [MariaDB 가격 책정 페이지](https://azure.microsoft.com/pricing/details/mariadb/)를 참조하세요.

## <a name="restore"></a>복원

Azure Database for MariaDB에서 복원을 수행하면 원래 서버의 백업에서 새 서버가 만들어집니다.

사용할 수 있는 두 가지 유형의 복원이 있습니다.

- **특정 시점 복원**은 백업 중복 옵션에서 사용할 수 있으며, 원본 서버와 동일한 지역에 새 서버를 만듭니다.
- **지역 복원**은 지역 중복 저장소를 위해 서버를 구성하고 이 서버를 다른 지역으로 복원할 수 있는 경우에만 사용할 수 있습니다.

예상 복구 시간은 데이터베이스 크기, 트랜잭션 로그 크기, 네트워크 대역폭 및 동일한 지역에서 동시에 복구되는 데이터베이스의 총 수를 포함한 여러 요소에 따라 달라집니다. 복구 시간은 일반적으로 12시간 미만입니다.

> [!IMPORTANT]
> 삭제된 서버는 복원할 수 **없습니다**. 서버를 삭제하면 해당 서버에 속한 모든 데이터베이스도 삭제되고 복구할 수 없습니다.

### <a name="point-in-time-restore"></a>지정 시간 복원

백업 중복 옵션과는 별도로 백업 보존 기간 내의 특정 시점으로 복원을 수행할 수 있습니다. 새 서버가 원본 서버와 동일한 Azure 지역에 만들어집니다. 이 경우 가격 책정 계층, 세대 계산, vCore 수, 저장소 크기, 백업 보존 기간 및 백업 중복 옵션에 대한 원래 서버의 구성으로 만들어집니다.

특정 시점 복원은 여러 시나리오에서 유용합니다. 예를 들어 사용자가 실수로 데이터를 삭제하거나 중요한 테이블 또는 데이터베이스를 삭제하는 경우, 또는 응용 프로그램의 결함으로 인해 우연히 적절한 데이터를 잘못된 데이터로 덮어쓰는 경우가 있습니다.

마지막 5분 내의 특정 시점으로 복원하려면, 다음 트랜잭션 로그 백업이 완료될 때까지 기다려야 할 수도 있습니다.

### <a name="geo-restore"></a>지역 복원

지역 중복 백업을 위해 서버를 구성한 경우 서비스를 사용할 수 있는 다른 Azure 지역으로 서버를 복원할 수 있습니다. 지역 복원은 서버가 호스팅되는 지역에 사고가 발생하여 서버를 사용할 수 없는 경우에 대비한 기본 복구 옵션입니다. 지역에서 발생한 대규모 사고로 인해 데이터베이스 응용 프로그램을 사용할 수 없는 경우 지역 중복 백업에서 다른 지역에 있는 서버로 서버를 복원할 수 있습니다. 백업을 수행할 때와 다른 지역으로 복제할 때 사이에 지연이 있습니다. 재해가 발생한 경우 최대 1시간 동안의 데이터가 손실되므로 이 지연은 최대 1시간일 수 있습니다.

지역 복원 중에 변경할 수 있는 서버 구성으로는 컴퓨팅 생성, vCore, 백업 보존 기간 및 백업 중복 옵션이 있습니다. 가격 책정 계층(기본, 범용 또는 메모리 최적화) 또는 저장소 크기는 지역 복원 중에 변경할 수 없습니다.

### <a name="perform-post-restore-tasks"></a>복원 후 작업 수행

복구 메커니즘에서 복원한 후에 다음 작업을 수행하여 사용자 및 응용 프로그램이 다시 백업 및 실행되도록 해야 합니다.

- 새 서버가 원래 서버를 교체하기 위한 것이라면 클라이언트와 클라이언트 응용 프로그램을 새 서버로 리디렉션합니다.
- 사용자가 연결할 수 있도록 적절한 서버 수준 방화벽 규칙이 있는지 확인합니다.
- 적절한 로그인 및 데이터베이스 수준 권한이 있는지 확인합니다.
- 필요에 따라 경고를 구성합니다.

## <a name="next-steps"></a>다음 단계

- 비즈니스 연속성에 대한 자세한 내용은  [비즈니스 연속성 개요](concepts-business-continuity.md)를 참조하세요.
- Azure Portal을 사용하여 특정 시점으로 복원하려면  [Azure Portal을 사용하여 특정 시점으로 데이터베이스 복원](howto-restore-server-portal.md)을 참조하세요.
 
<!--
- To restore to a point in time using Azure CLI, see [restore database to a point in time using CLI](howto-restore-server-cli.md).-->