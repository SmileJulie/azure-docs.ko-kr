---
title: CLI 예제 Azure SQL 데이터베이스 이동 SQL 탄력적 풀 | Microsoft Docs
description: SQL 탄력적 풀에서 SQL Database를 이동하는 Azure CLI 예제 스크립트
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
manager: craigg
ms.date: 06/25/2019
ms.openlocfilehash: 173e54dc6cf24676a8bf9833b0b6152a5fd89748
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447851"
---
# <a name="use-cli-to-move-an-azure-sql-database-in-a-sql-elastic-pool"></a>CLI를 사용하여 SQL 탄력적 풀에서 Azure SQL 데이터베이스 이동

이 Azure CLI 스크립트 예제는 두 개의 탄력적 풀을 만들고 한 SQL 탄력적 풀에서 다른 SQL 탄력적 풀로 Azure SQL 데이터베이스를 이동한 다음, 탄력적 풀의 데이터베이스를 단일 데이터베이스 컴퓨팅 크기로 이동합니다.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI를 로컬로 설치하여 사용하도록 선택한 경우 이 항목에서 Azure CLI 버전 2.0 이상을 실행해야 합니다. `az --version`을 실행하여 버전을 찾습니다. 설치 또는 업그레이드가 필요한 경우, [Azure CLI 설치]( /cli/azure/install-azure-cli)를 참조하세요.

## <a name="sample-script"></a>샘플 스크립트

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/move-database-between-pools/move-database-between-pools.sh "Move database between pools")]

## <a name="clean-up-deployment"></a>배포 정리

다음 명령을 사용하여 리소스 그룹 및 모든 관련 리소스를 제거합니다.

```azurecli-interactive
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>스크립트 설명

이 스크립트는 다음 명령을 사용합니다. 테이블에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | 모든 리소스가 저장되는 리소스 그룹을 만듭니다. |
| [az sql server create](https://docs.microsoft.com/cli/azure/sql/server#az-sql-server-create) | 단일 데이터베이스 및 탄력적 풀을 호스트하는 SQL Database 서버를 만듭니다. |
| [az sql elastic-pools create](https://docs.microsoft.com/cli/azure/sql/elastic-pool#az-sql-elastic-pool-create) | 탄력적 풀을 만듭니다. |
| [az sql db create](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-create) | 단일 데이터베이스 또는 탄력적 풀의 데이터베이스를 만듭니다. |
| [az sql db update](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-update) | 데이터베이스 속성을 업데이트하거나 탄력적 풀로/에서 또는 탄력적 풀 간에 데이터베이스를 이동합니다. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az-vm-extension-set) | 모든 중첩 리소스를 포함한 리소스 그룹을 삭제합니다. |

## <a name="next-steps"></a>다음 단계

Azure CLI에 대한 자세한 내용은 [Azure CLI 설명서](https://docs.microsoft.com/cli/azure)를 참조하세요.

추가 SQL Database CLI 스크립트 샘플은 [Azure SQL Database 설명서](../sql-database-cli-samples.md)에서 찾을 수 있습니다.
