---
title: Azure VM의 SQL Server 릴리스 정보 | Microsoft Docs
description: Azure VM - SQL Server의 새로운 기능 및 향상된 기능에 대한 자세한 정보
services: virtual-machines-windows
author: MashaMSFT
ms.author: mathoma
manager: craigg
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 2/13/2019
ms.openlocfilehash: 47578d5b6bb4a0094ba1a2e9e7dc7f92d0b0e231
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67607099"
---
# <a name="sql-server-on-azure-virtual-machine-release-notes"></a>Azure Virtual Machines의 SQL Server 릴리스 정보

Azure를 사용하면 SQL Server 이미지가 기본 제공된 가상 머신을 배포할 수 있습니다. 이 문서에서는 [Azure 가상 머신의 SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/) 최신 릴리스에 제공되는 새 기능과 향상된 기능을 요약합니다. 이 문서에는 릴리스와 직접적인 관련은 없지만 동일한 시간대에 게시된 주목할 만한 콘텐츠 업데이트도 나열되어 있습니다. 다른 Azure 서비스의 개선 사항은 [서비스 업데이트](https://azure.microsoft.com/updates)를 참조하세요.


## <a name="june-2019"></a>2019년 6월

### <a name="service-improvements"></a>서비스 개선 사항

| 서비스 개선 사항 | 세부 정보 |
| --- | --- |
| **새 SQL IaaS 설치 모드** | SQL IaaS 확장을 설치할 수 됩니다 [경량 모드](virtual-machines-windows-sql-server-agent-extension.md) SQL Server 서비스를 다시 시작 해야 합니다.  |
| **SQL Server edition 수정** | 이제 변경할 수 있습니다 합니다 [edition 속성](virtual-machines-windows-sql-change-edition.md) SQL Server VM에 대 한 합니다. |
| **SQL VM RP 변경** | 할 수 있습니다 [SQL Server VM에 SQL VM 리소스 공급자 등록](virtual-machines-windows-sql-register-with-resource-provider.md#register-with-sql-vm-resource-provider) -도 [Windows 2008 이미지](virtual-machines-windows-sql-register-with-resource-provider.md#register-sql-server-2008r2-on-windows-server-2008-vms) -새 SQL IaaS 모드를 사용 하 여 합니다. |
| **AHUB를 사용 하 여 BYOL 이미지** | BYOL 이미지 marketplace에서 배포 이제 전환할 수 해당 [라이선스 형식에 '종 량 제'](virtual-machines-windows-sql-ahb.md#remarks)합니다.| 
| &nbsp; | &nbsp; |



## <a name="may-2019"></a>2019년 5월

### <a name="service-improvements"></a>서비스 개선 사항

| 서비스 개선 사항 | 세부 정보 |
| --- | --- |
| **Azure portal에서 새 SQL VM 관리** | Azure portal에서 SQL Server VM을 관리 하는 새로운 방법이 되었습니다. 자세한 내용은 [Azure portal에서 SQL Server VM 관리](virtual-machines-windows-sql-manage-portal.md)합니다.  | 
| &nbsp; | &nbsp; |

### <a name="documentation-improvements"></a>설명서 개선

| 설명서 | 세부 정보 |
| --- | --- |
| **새 SQL VM 포털 관리** | 새 SQL VM 관리 포털 환경과 십 문서가 업데이트 되었습니다. | 
| &nbsp; | &nbsp; |




## <a name="april-2019"></a>2019년 4월

### <a name="service-improvements"></a>서비스 개선 사항

| 서비스 개선 사항 | 세부 정보 |
| --- | --- |
| **SQL Server 2008/2008R2에 대해서는 지원 확장** | [지원을 확장할](virtual-machines-windows-sql-server-2008-eos-extend-support.md) SQL Server 2008 및 SQL Server 2008 R2 마이그레이션 *으로-는* Azure VM에 있습니다. | 
| &nbsp; | &nbsp; |


## <a name="march-2019"></a>2019 년 3 월

| 서비스 개선 사항 | 세부 정보 |
| --- | --- |
| **사용자 지정 이미지 지원 가능성** | 이제 설치할 수 있습니다는 [SQL IaaS 확장](virtual-machines-windows-sql-server-agent-extension.md#installation) 사용자 지정 운영 체제 및 SQL 이미지를 제공 하는 제한 된 기능의 [융통성 있는 라이선스](virtual-machines-windows-sql-ahb.md)합니다. 로 라이선스 유형을 'AHUB'으로 지정할 SQL 리소스 공급자를 사용 하 여 사용자 지정 이미지를 등록 하는 경우 그렇지 않은 경우 등록이 실패 합니다. | 
| **명명 된 인스턴스 지원 가능성** | 사용할 수 있습니다 합니다 [SQL IaaS 확장](virtual-machines-windows-sql-server-agent-extension.md#installation) 기본 인스턴스에 제대로 제거 된 경우 명명된 된 인스턴스를 사용 하 여 합니다. | 
| **포털의 향상 된 기능** | SQL Server VM을 배포 하기 위한 Azure portal 환경은 유용성을 향상 시키기 개선 되었습니다 했습니다. 자세한 내용은 참조는 brief [빠른 시작](quickstart-sql-vm-create-portal.md) 보다 정확 하 게 하 고 [방법](virtual-machines-windows-portal-sql-server-provision.md) SQL Server VM을 배포 하는 가이드입니다.|
| &nbsp; | &nbsp; |


## <a name="february-2019"></a>2019년 2월

| 서비스 개선 사항 | 세부 정보 |
| --- | --- |
| **포털 개선** | 가져오기-사용자-라이선스를 사용 하 여 종 량 제에서 SQL Server VM에 대 한 라이선스 모델을 변경할 수는 이제는 [Azure portal](virtual-machines-windows-sql-ahb.md#change-license-for-vms-already-registered-with-resource-provider)|
|**Azure SQL VM CLI를 사용 하 여 AG 배포 간소화** | 이제 이전에 Azure에서 SQL Server VM에 가용성 그룹 배포 보다는 것이 쉽습니다. [Azure SQL VM CLI](/cli/azure/sql/vm?view=azure-cli-2018-03-01-hybrid) 레코드 시간에 모두 명령줄에서에서 WSFC, ILB와 AG 수신기를 만들 수 있습니다. 자세한 내용은 [SQL Server Azure VM에 대 한 Always On 가용성 그룹을 구성 하려면 사용 하 여 Azure SQL VM CLI](virtual-machines-windows-sql-availability-group-cli.md)합니다. | 
| &nbsp; | &nbsp; |


## <a name="december-2018"></a>2018년 12월

| 서비스 개선 사항 | 세부 정보 |
| --- | --- |
| **새 SQL 클러스터 그룹 리소스 공급자** | Windows 장애 조치(failover) 클러스터의 메타데이터를 정의하는 새 리소스 공급자(Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups)입니다. SQL Server VM을 *SqlVirtualMachineGroups*에 연결하면 Windows 장애 조치(failover) 클러스터 서비스가 부트스트랩되고 VM이 클러스터에 연결됩니다.  |
|**Azure 빠른 시작 템플릿을 사용하여 가용성 그룹 배포 설정 자동화** |이제 Windows 장애 조치(failover) 클러스터를 만들고, SQL Server VM을 해당 클러스터에 연결하고, 수신기를 만들고, 두 Azure 빠른 시작 템플릿을 사용하여 내부 Load Balancer를 구성할 수 있습니다. 자세한 내용은 [SQL Server Azure VM에 대 한 Always On 가용성 그룹을 구성 하려면 Azure 빠른 시작 템플릿을 사용 하 여](virtual-machines-windows-sql-availability-group-quickstart-template.md)입니다. | 
| **자동 SQL VM 리소스 공급자 등록** | 이번 달 이후에 배포되는 SQL Server VM은 새로운 SQL Server 리소스 공급자에 자동으로 등록됩니다. 이번 달 이전에 배포된 SQL Server VM은 계속해서 수동으로 등록해야 합니다. 자세한 내용은 [SQL VM 리소스 공급자를 사용 하 여 기존 SQL VM을 등록](virtual-machines-windows-sql-register-with-resource-provider.md)합니다.|
| &nbsp; | &nbsp; |


## <a name="november-2018"></a>2018년 11월

| 서비스 개선 사항 | 세부 정보 |
| --- | --- |
| **새 SQL VM 리소스 공급자**: |  SQL Server VM의 효과적인 관리를 지원하는 새 SQL Server VM용 리소스 공급자(Microsoft.SqlVirtualMachine)입니다. VM 등록에 대한 자세한 내용은 [새 리소스 공급자에 기존 SQL VM 등록](virtual-machines-windows-sql-register-with-resource-provider.md)을 참조하세요. |
|**라이선스 모델 전환** | 이제 Azure CLI 또는 PowerShell을 사용하여 SQL VM의 종량제 모델과 사용자 라이선스 필요 모델 간에 전환할 수 있습니다. 자세한 내용은 [SQL VM의 라이선스 모델을 변경하는 방법](virtual-machines-windows-sql-ahb.md)을 참조하세요. | 
| &nbsp; | &nbsp; |


## <a name="additional-resources"></a>추가 자료

**Windows VM**:

* [Windows VM에서 SQL Server 개요](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server Windows VM 프로비전](virtual-machines-windows-portal-sql-server-provision.md)
* [Azure VM에서 SQL Server로 데이터베이스 마이그레이션](virtual-machines-windows-migrate-sql.md)
* [Azure Virtual Machines의 SQL Server에 대한 고가용성 및 재해 복구](virtual-machines-windows-sql-high-availability-dr.md)
* [Azure Virtual Machines의 SQL Server에 대한 성능 모범 사례](virtual-machines-windows-sql-performance.md)
* [Azure Virtual Machines의 SQL Server에 대한 애플리케이션 패턴 및 개발 전략](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**Linux VM**:

* [Linux VM의 SQL Server 개요](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [SQL Server Linux Virtual Machine 프로비전](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [FAQ(Linux)](../../linux/sql/sql-server-linux-faq.md)
* [Linux의 SQL Server 설명서](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
