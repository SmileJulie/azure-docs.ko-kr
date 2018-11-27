---
title: Azure Database for MariaDB에 대한 일시적 연결 오류 처리 | Microsoft Docs
description: Azure Database for MariaDB에 대한 일시적 연결 오류를 처리하는 방법을 알아봅니다.
keywords: MySQL 연결, 연결 문자열, 연결 문제, 일시적 오류, 연결 오류
services: mariadb
author: jan-eng
ms.author: janeng
manager: kfile
editor: jasonwhowell
ms.service: mariadb
ms.topic: article
ms.date: 11/09/2018
ms.openlocfilehash: 203401e3842912169371f315048f6930c8dc80eb
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51568100"
---
# <a name="handling-of-transient-connectivity-errors-for-azure-database-for-mariadb"></a>Azure Database for MariaDB에 대한 일시적 연결 오류 처리

이 문서에서는 Azure Database for MariaDB에 대한 일시적 연결 오류를 처리하는 방법에 대해 설명합니다.

## <a name="transient-errors"></a>일시적 오류

일시적 결함이라고도 하는 일시적 오류는 자체적으로 해결되는 오류입니다. 가장 일반적으로 이러한 오류는 삭제되는 데이터베이스 서버에 대한 연결로 나타납니다. 또한 서버에 대한 새 연결을 열 수 없습니다. 예를 들어 하드웨어 또는 네트워크 오류가 발생할 때 일시적 오류가 발생할 수 있습니다. 또 다른 이유로 롤아웃 중인 새 버전의 PaaS 서비스 때문일 수도 있습니다. 이러한 이벤트의 대부분은 60초 이내에 시스템에서 자동으로 완화됩니다. 클라우드에서 애플리케이션을 설계하고 개발하는 가장 좋은 방법은 일시적 오류를 예상하는 것입니다. 언제든지 모든 구성 요소에서 발생할 수 있으며 이러한 상황을 처리할 수 있는 적절한 논리를 갖추고 있다고 가정합니다.

## <a name="handling-transient-errors"></a>일시적 오류 처리

일시적 오류는 다시 시도 논리를 사용하여 처리해야 합니다. 고려해야 할 상황은 다음과 같습니다.

* 연결을 열려고 하면 오류가 발생합니다.
* 서버 쪽에서 유휴 연결이 삭제됩니다. 명령을 실행하려고 하면 실행할 수 없습니다.
* 현재 명령을 실행하는 활성 연결이 삭제됩니다.

첫 번째와 두 번째 경우는 매우 간단하게 처리할 수 있습니다. 연결을 열기 위해 다시 시도합니다. 성공하면 일시적 오류가 시스템에서 완화됩니다. Azure Database for MariaDB를 다시 사용할 수 있습니다. 연결을 다시 시도하기 전에 기다리는 것이 좋습니다. 초기 다시 시도가 실패하면 잠시 그대로 있습니다. 이렇게 하면 오류 상황을 극복하기 위해 시스템에서 사용 가능한 모든 리소스를 사용할 수 있습니다. 따라야 할 좋은 패턴은 다음과 같습니다.

* 5초 동안 기다린 후에 다시 시도합니다.
* 다시 시도할 때마다 대기 시간이 최대 60초까지 기하급수적으로 증가합니다.
* 애플리케이션에서 작업이 실패한 것으로 간주하는 시점의 최대 재시도 횟수를 설정합니다.

활성 트랜잭션과의 연결에 실패하면 복구를 제대로 처리하기가 더 어렵습니다. 두 가지 경우가 있습니다. 트랜잭션이 실제로 읽기 전용인 경우 연결을 다시 열고 트랜잭션을 다시 시도하는 것이 안전합니다. 그러나 트랜잭션이 데이터베이스에도 기록한 경우 트랜잭션이 롤백되었는지 또는 일시적 오류가 발생하기 전에 성공했는지 확인해야 합니다. 이 경우 데이터베이스 서버로부터 커밋 승인을 받지 못했을 수 있습니다.

이 작업을 수행하는 한 가지 방법은 클라이언트에서 모든 다시 시도에 사용되는 고유 ID를 생성하는 것입니다. 이 고유 ID를 트랜잭션의 일환으로 서버에 전달하고, 고유 제약 조건이 있는 열에 이를 저장합니다. 이렇게 하면 트랜잭션을 안전하게 다시 시도할 수 있습니다. 이전 트랜잭션이 롤백되고 클라이언트에서 생성된 고유 ID가 시스템에 아직 없으면 이 작업이 성공합니다. 이전 트랜잭션이 성공적으로 완료되었으므로 고유 ID가 이전에 저장된 경우 중복 키 위반을 나타내는 데 실패합니다.

프로그램에서 타사 미들웨어를 통해 Azure Database for MariaDB와 통신하는 경우 공급업체에 문의하여 일시적 오류에 대한 다시 시도 논리가 미들웨어에 포함되어 있는지를 확인해야 합니다.

다시 시도 논리는 테스트해야 합니다. 예를 들어 Azure Database for MariaDB의 계산 리소스를 확장하거나 축소하는 동안 코드를 실행하려고 합니다. 애플리케이션에서 이 작업 중에 발생하는 짧은 가동 중지 시간을 아무 문제 없이 처리해야 합니다.

## <a name="next-steps"></a>다음 단계

* [Azure Database for MariaDB에 대한 연결 문제 해결](howto-troubleshoot-common-connection-issues.md)