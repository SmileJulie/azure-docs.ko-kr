---
title: Azure Reserved VM Instances 할인 이해 | Microsoft Docs
description: 실행 중인 VM에 Azure Reserved Virtual Machine Instances 할인이 적용되는 방식을 알아봅니다.
author: yashesvi
manager: yashar
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2019
ms.author: banders
ms.openlocfilehash: 191160035f516d818d5537c5c47f9604998c46f7
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67849983"
---
# <a name="how-the-azure-reservation-discount-is-applied-to-virtual-machines"></a>Virtual machines에 Azure 예약 할인이 적용 되는 방법

Azure Reserved Virtual Machine Instance를 구입한 후, 예약 할인이 예약의 특성 및 수량과 일치하는 가상 머신에 자동으로 적용됩니다. 예약은 가상 머신의 계산 비용을 포함합니다.

예약 할인은 Azure Marketplace에서 구매한 기본 Vm에 적용 됩니다.

SQL Database 예약된 용량의 경우 [Azure Reserved Instances 할인](billing-understand-reservation-charges.md)을 참조하세요.

다음 표에서는 Reserved VM Instance를 구입한 후의 가상 머신에 대한 비용을 설명합니다. 모든 경우에는 정상 요금으로 저장소 및 네트워킹에 대한 요금이 청구됩니다.

| 가상 머신 형식  | Reserved VM Instance 요금 |
|-----------------------|--------------------------------------------|
|추가 소프트웨어가 없는 Linux VM | 예약은 VM 인프라 비용을 포함합니다.|
|소프트웨어 요금이 포함된 Linux VM(예: Red Hat) | 예약은 인프라 비용을 포함합니다. 추가 소프트웨어에 대한 요금이 청구됩니다.|
|추가 소프트웨어가 없는 Windows VM |예약은 인프라 비용을 포함합니다. Windows 소프트웨어에 대한 요금이 청구됩니다.|
|추가 소프트웨어가 있는 Windows VM(예: SQL Server) | 예약은 인프라 비용을 포함합니다. Windows 소프트웨어 및 추가 소프트웨어에 대한 요금이 청구됩니다.|
|[Azure Hybrid Benefit](../virtual-machines/windows/hybrid-use-benefit-licensing.md)을 포함한 Windows VM | 예약은 인프라 비용을 포함합니다. Windows 소프트웨어 비용은 Azure Hybrid Benefit에 포함되어 있습니다. 추가 소프트웨어는 별도로 청구됩니다.|

## <a name="how-reservation-discount-is-applied"></a>예약 할인이 적용 되는 방법

예약 할인은 "*사용-또는 분실*"입니다. 따라서 모든 시간에 대해 일치 하는 리소스가 없는 경우 해당 시간의 예약 수량이 손실 됩니다. 사용 하지 않는 예약 시간을 앞으로 사용할 수 없습니다.

리소스를 종료할 때 예약 할인이 지정 된 범위에서 일치 하는 다른 리소스에 자동으로 적용 됩니다. 지정 된 범위에서 일치 하는 리소스를 찾을 수 없는 경우 예약 된 시간이 *손실*됩니다.

## <a name="reservation-discount-for-non-windows-vms"></a>비 Windows Vm에 대 한 예약 할인

 Azure Reservation 할인은 시간 단위로 실행 중인 VM 인스턴스에 적용됩니다. 구입한 예약은 예약 할인을 적용할 실행 중인 VM의 사용량과 일치합니다. 전체 시간을 실행하지 못한 VM의 경우 예약은 현재 실행 중인 VM을 비롯하여 예약을 사용하지 않는 다른 VM에서 채워집니다. 시간이 다 지날 때쯤 해당 시간의 VM에 대한 예약 애플리케이션이 잠깁니다. 1시간 동안 VM이 실행되지 않거나 시간 내 동시 VM이 예약 시간을 채우지 못하는 경우 예약이 해당 시간 동안 미달 사용됩니다. 다음 그래프는 청구 가능한 VM 사용량에 대한 예약 애플리케이션을 보여줍니다. 일러스트레이션은 하나의 예약 구매 및 일치하는 두 개의 VM 인스턴스를 기반으로 합니다.

![적용된 하나의 예약 및 일치하는 두 개의 VM 인스턴스에 대한 스크린샷](./media/billing-reserved-vm-instance-application/billing-reserved-vm-instance-application.png)

1. 예약 라인을 초과 하는 사용량은 일반 종 량 제 요금으로 청구 됩니다. 예약 선을 초과하지 않는 사용량은 이미 예약 구매의 일부로 지불되었기 때문에 청구되지 않습니다.
2. 시간 1 중에 인스턴스 1은 0.75시간 동안 실행되고 인스턴스 2는 0.5시간 동안 실행됩니다. 시간 1의 총 사용량은 1.25시간입니다. 나머지 0.25시간에 대한 종량제 요금이 청구됩니다.
3. 시간 2 및 시간 3의 경우 두 인스턴스는 1시간 동안 실행되었습니다. 하나의 인스턴스는 예약에 포함되고 다른 하나는 종량제 요금으로 요금이 부과됩니다.
4. 시간 4의 경우 인스턴스 1은 0.5시간 동안 실행되고 인스턴스 2는 1시간 동안 실행됩니다. 인스턴스 1은 예약에 완전히 포함되고 인스턴스 2 중 0.5시간도 포함됩니다. 나머지 0.5시간에 대한 종량제 요금이 청구됩니다.

청구 사용량 보고서에서 Azure 예약 적용 결과를 이해하고 확인하려면 [예약 사용량 이해](billing-understand-reserved-instance-usage-ea.md)를 참조하세요.

## <a name="reservation-discount-for-windows-vms"></a>Windows Vm에 대 한 예약 할인

Windows VM 인스턴스를 실행하면 인프라 비용을 포함하도록 예약이 적용됩니다. Windows VM의 VM 인프라 비용에 예약을 적용하는 것은 Windows 외 VM의 경우와 동일합니다. vCPU를 기준으로 Windows 소프트웨어에 대한 요금이 별도로 청구됩니다. [예약 사용 시 Windows 소프트웨어 비용](billing-reserved-Instance-windows-software-costs.md)을 참조하세요. [Windows Server용 Azure 하이브리드 혜택](../virtual-machines/windows/hybrid-use-benefit-licensing.md)으로 Windows 라이선스 비용을 처리할 수 있습니다.

## <a name="discount-can-apply-to-different-sizes"></a>할인은 다른 크기에 적용 될 수 있습니다.

예약 VM 인스턴스를 구매하는 경우 **최적화 대상**: **인스턴스 크기 유연성**을 선택하면 할인 적용률은 선택하는 VM 크기에 따라 달라집니다. 같은 크기 시리즈 그룹의 VM(가상 머신) 크기에 예약을 적용할 수 있습니다. 자세한 내용은 [예약 VM 인스턴스를 통해 유동적으로 가상 머신 크기 조정](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md)을 참조하세요.

## <a name="discount-applies-to-matching-servicetype-only"></a>할인은 일치 하는 ServiceType에만 적용 됩니다.

예약 할인은의 `ServiceType` `AdditionalInfo` 값이 구매한 예약과 일치 하는 VM 사용에만 적용 됩니다. 예약 할인 응용 프로그램은 Vm에 사용 되는 측정기를 `ServiceType`무시 하 고만 평가 합니다. VM을 구입한 서비스 유형을 파악 합니다. Premium storage 예약에 대 한 프리미엄 저장소 VM 예약을 교환 하거나 그 반대로 할 수 있습니다.

## <a name="services-that-get-vm-reservation-discounts"></a>VM 예약 할인을 가져오는 서비스

Vm 예약은 VM 배포 뿐만 아니라 여러 서비스에서 내보낸 VM 사용에 적용 될 수 있습니다. 예약 할인을 받는 리소스는 인스턴스 크기 유연성 설정에 따라 변경 됩니다.

### <a name="instance-size-flexibility-setting"></a>인스턴스 크기 유연성 설정

인스턴스 크기 유연성 설정은 예약 된 인스턴스 할인을 가져오는 서비스를 결정 합니다.

설정이 on 또는 off 인지 여부는 *ConsumedService* 일 `Microsoft.Compute`때 예약 할인이 일치 하는 VM 사용에 자동으로 적용 됩니다. 따라서 *ConsumedService* 값에 대 한 사용 현황 데이터를 확인 합니다. 일부 사례:

- 가상 머신
- 가상 머신 크기 집합
- 컨테이너 서비스
- Azure Batch 배포 (사용자 구독 모드)
- Azure Kubernetes Service (AKS)
- Service Fabric

설정이 on 이면 *ConsumedService* 가 다음 항목 중 하나에 해당 하는 경우 예약 할인이 일치 하는 VM 사용에 자동으로 적용 됩니다.

- Microsoft.Compute
- Microsoft.ClassicCompute
- Microsoft.Batch
- Microsoft.MachineLearningServices
- Microsoft.Kusto

사용 현황 데이터의 *ConsumedService* 값을 확인 하 여 사용이 예약 할인이 적합 한지 확인 합니다.

인스턴스 크기 유연성에 대 한 자세한 내용은 [예약 된 VM 인스턴스를 사용 하는 가상 머신 크기 유연성](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md)을 참조 하세요.


## <a name="need-help-contact-us"></a>도움 필요 시 문의처

궁금한 사항이 있거나 도움이 필요 하면 [지원 요청을 만드세요](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>다음 단계

Azure 예약에 대한 자세한 내용은 다음 문서를 참조하세요.

- [Azure에 대 한 예약은 무엇 인가요?](billing-save-compute-costs-reservations.md)
- [Azure Reserved VM Instances를 사용하여 Virtual Machines 선불 결제](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure SQL Database 예약 용량을 사용하여 SQL Database 계산 리소스 요금 선결제](../sql-database/sql-database-reserved-capacity.md)
- [Azure에 대 한 예약 관리](billing-manage-reserved-vm-instance.md)
- [종량제 구독의 예약 사용량 이해](billing-understand-reserved-instance-usage.md)
- [엔터프라이즈 등록의 예약 사용량 이해](billing-understand-reserved-instance-usage-ea.md)
- [CSP 구독에서 예약 사용량 이해](/partner-center/azure-reservations)
- [예약에 포함되지 않는 Windows 소프트웨어 비용](billing-reserved-instance-windows-software-costs.md)
