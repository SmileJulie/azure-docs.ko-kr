---
title: 샘플-영국 공식 및 영국 NHS 청사진-컨트롤 매핑
description: UK 공식 및 영국 NHS 청사진 예제의 매핑을 제어 합니다.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/26/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 945898105aab7261ee494a86aeff10337599feb3
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/15/2019
ms.locfileid: "68226017"
---
# <a name="control-mapping-of-the-uk-official-and-uk-nhs-blueprint-samples"></a>영국 공식 및 UK NHS 청사진 예제의 매핑 제어

다음 문서에서는 영국 공식 및 영국 NHS 청사진 샘플이 영국 공식 및 영국 NHS 컨트롤에 매핑되는 방법에 대해 자세히 설명 합니다. 컨트롤에 대 한 자세한 내용은 [영국 공식](https://www.gov.uk/government/publications/government-security-classifications)을 참조 하세요.

다음 매핑은 **영국 공식** 및 **영국 nhs** 컨트롤에 대 한 것입니다. 특정 컨트롤 매핑으로 바로 점프하려면 오른쪽의 탐색 기능을 사용합니다. 많은 매핑된 컨트롤은 [Azure Policy](../../../policy/overview.md) 이니셔티브를 사용하여 구현됩니다. 전체 이니셔티브를 검토하려면 Azure Portal에서 **정책**을 열고 **정의** 페이지를 선택합니다. 그런 다음  **\[Preview\] uk 공식 및 영국 nhs 컨트롤을 찾아 선택 하 고 특정 VM 확장을 배포 하 여 감사 요구 사항** 기본 제공 정책 이니셔티브를 지원 합니다.

## <a name="1-data-in-transit-protection"></a>전송 보호의 데이터 1 개

청사진은 저장소 계정 및 Redis Cache에 대 한 안전 하지 않은 연결을 감사 하는 [Azure Policy](../../../policy/overview.md) 정의를 할당 하 여 Azure 서비스와의 정보 전송을 안전 하 게 해줍니다.

- Redis Cache에 대 한 보안 연결만 사용 하도록 설정 해야 합니다.
- 저장소 계정에 대 한 보안 전송을 사용 하도록 설정 해야 합니다.

## <a name="23-data-at-rest-protection"></a>2.3 미사용 데이터 보호

이 청사진은 특정 고 암호화 컨트롤을 적용 하 고 약한 암호화 설정의 사용을 감사 하는 [Azure Policy](../../../policy/overview.md) 정의를 할당 하 여 고 암호화 컨트롤을 사용할 때 정책을 적용 하는 데 도움이 됩니다.
Azure 리소스의 암호화 구성이 최적이 아닐 수 있는 경우를 이해하면 리소스가 정보 보안 정책에 따라 구성되도록 정정 작업을 수행하는 데 도움이 될 수 있습니다. 특히이 청사진에서 할당 한 정책에는 data lake storage 계정에 대 한 암호화가 필요 합니다. SQL 데이터베이스에서 투명 한 데이터 암호화 필요 저장소 계정, SQL 데이터베이스, 가상 머신 디스크 및 automation 계정 변수에 대 한 누락 된 암호화 감사 저장소 계정 및 Redis Cache에 대 한 안전 하지 않은 연결 감사 약한 가상 컴퓨터 암호 암호화 감사 및는 암호화 되지 않은 Service Fabric 통신을 감사 합니다.

- Azure Security Center에서 암호화 되지 않은 SQL 데이터베이스 모니터링
- 가상 컴퓨터에 디스크 암호화를 적용 해야 합니다.
- Automation 계정 변수는 암호화 되어야 합니다.
- 저장소 계정에 대 한 보안 전송을 사용 하도록 설정 해야 합니다.
- Service Fabric 클러스터는 ClusterProtectionLevel 속성을 EncryptAndSign로 설정 해야 합니다.
- SQL 데이터베이스에 대 한 투명한 데이터 암호화를 사용 하도록 설정 해야 합니다.
- SQL DB 투명한 데이터 암호화 배포
- Data Lake Store 계정 암호화 필요
- 허용 되는 위치 ("영국 남부" 및 "영국 서 부"로 하드 코딩 됨)
- 리소스 그룹에 대해 허용 되는 위치 ("영국 남부" 및 "영국 서 부"로 하드 코딩 됨)

## <a name="52-vulnerability-management"></a>5.2 취약성 관리

이 청사진은 누락 된 endpoint protection, 누락 된 시스템 업데이트, 운영 체제 취약점, SQL 취약성 및 가상을 모니터링 하는 [Azure Policy](../../../policy/overview.md) 정의를 할당 하 여 정보 시스템 취약성을 관리 하는 데 도움이 됩니다. 시스템 취약점. 이러한 인사이트는 배포된 리소스의 보안 상태에 대한 실시간 정보를 제공하며, 업데이트 관리 작업의 우선 순위를 지정하는 데 도움이 될 수 있습니다.

- Azure Security Center에서 누락된 Endpoint Protection 모니터링
- 시스템 업데이트를 컴퓨터에 설치 해야 합니다.
- 컴퓨터에서 보안 구성의 취약성을 재구성 해야 함
- SQL 데이터베이스에 대 한 취약성을 재구성 해야 합니다.
- 취약성 평가 솔루션에서 취약성을 재구성 해야 함

## <a name="53-protective-monitoring"></a>5.3 보호 모니터링

이 청사진은 무제한 액세스, 허용 목록 활동 및 위협에 대 한 보호 모니터링을 제공 하는 [Azure Policy](../../../policy/overview.md) 정의를 할당 하 여 정보 시스템 자산을 보호 하는 데 도움이 됩니다.

- 스토리지 계정에 대한 무제한 네트워크 액세스 감사
- 적응형 응용 프로그램 제어는 가상 머신에서 사용 하도록 설정 해야 합니다.
- SQL 서버에 위협 탐지 배포
- Windows Server에 대 한 기본 Microsoft IaaS 맬웨어 방지 확장 배포

## <a name="9-secure-user-management--10-identity-and-authentication"></a>9 보안 사용자 관리/10 Id 및 인증

Azure는 Azure의 리소스에 대 한 액세스 권한이 있는 사용자를 관리 하는 데 도움이 되는 RBAC (역할 기반 액세스 제어)를 구현 합니다. Azure Portal을 사용하여 Azure 리소스에 대한 액세스 권한이 있는 사용자 및 해당 사용자의 권한을 검토할 수 있습니다. 이 청사진은 소유자 및/또는 읽기/쓰기 권한이 있는 외부 계정을 감사 하는 [Azure Policy](../../../policy/overview.md) 정의를 할당 하 여 액세스 권한을 제한 하 고 제어 하며, 소유자, 읽기 및/또는 쓰기 권한이 있는 계정을 사용 하 여 다단계 인증을 사용 합니다.

- 구독에 대 한 소유자 권한이 있는 계정에서 MFA를 사용 하도록 설정 해야 합니다.
- 구독에 대 한 쓰기 권한이 있는 계정으로 MFA를 사용 하도록 설정 해야 합니다.
- 구독에 대 한 읽기 권한이 있는 계정에서 MFA를 사용 하도록 설정 해야 합니다.
- 소유자 권한이 있는 외부 계정을 구독에서 제거 해야 합니다.
- 쓰기 권한이 있는 외부 계정을 구독에서 제거해야 합니다.
- 읽기 권한이 있는 외부 계정을 구독에서 제거 해야 합니다.

이 청사진은 Azure Policy 정의를 할당 하 여 SQL server 및 Service Fabric에 대 한 Azure Active Directory 인증 사용을 감사 합니다. Azure Active Directory 인증을 사용하면 데이터베이스 사용자 및 기타 Microsoft 서비스의 권한을 간편하게 관리하고 ID를 한 곳에서 집중적으로 관리할 수 있습니다.

- SQL server에 대 한 Azure Active Directory 관리자를 프로 비전 해야 합니다.
- Service Fabric 클러스터는 클라이언트 인증용 으로만 Azure Active Directory를 사용 해야 합니다.

이 청사진은 사용 계정 및 외부 계정을 포함 하 여 검토를 위해 우선 순위를 지정 해야 하는 계정을 감사 하는 Azure Policy 정의도 할당 합니다. 필요한 경우 계정을 로그인하지 못하게 차단하거나 제거하여 Azure 리소스에 대한 액세스 권한을 즉시 제거할 수 있습니다. 이 청사진은 사용 계정을 감사 하는 두 가지 Azure Policy 정의를 할당 하 여 제거를 고려해 야 합니다.

- 사용 되지 않는 계정은 구독에서 제거 해야 합니다.
- 소유자 권한이 있는 사용 되지 않는 계정은 구독에서 제거 해야 합니다.
- 소유자 권한이 있는 외부 계정을 구독에서 제거 해야 합니다.
- 쓰기 권한이 있는 외부 계정을 구독에서 제거해야 합니다.

또한이 청사진은 잘못 설정 된 경우 경고에 대 한 Linux VM 암호 파일 사용 권한을 감사 하는 Azure Policy 정의를 할당 합니다. 이 설계를 통해 인증자 손상 되지 않도록 수정 작업을 수행할 수 있습니다.

- \[미리 보기\]: Audit Linux VM/etc/passwd file 권한이 0644으로 설정 되어 있습니다.

이 청사진은 최소 강도 및 기타 암호 요구 사항을 적용 하지 않는 Windows Vm을 감사 하는 Azure Policy 정의를 할당 하 여 강력한 암호를 적용 하는 데 도움이 됩니다. 암호 강도 정책을 위반하는 VM을 인식하면 모든 VM 사용자 계정의 암호가 정책을 준수하도록 정정 작업을 수행하는 데 도움이 됩니다.

- \[미리 보기\]: 암호 복잡성 설정이 사용 하도록 설정 되지 않은 Windows Vm을 감사 하기 위한 요구 사항 배포
- \[미리 보기\]: 70 일의 최대 암호 사용 기간을 포함 하지 않는 Windows Vm을 감사 하기 위한 요구 사항 배포
- \[미리 보기\]: 최소 암호 사용 기간이 1 일인 Windows Vm을 감사 하기 위한 요구 사항 배포
- \[미리 보기\]: 최소 암호 길이를 14 자로 제한 하지 않는 Windows Vm을 감사 하기 위한 요구 사항 배포
- \[미리 보기\]: 이전 24 개 암호를 다시 사용 하도록 허용 하는 Windows Vm을 감사 하기 위한 요구 사항 배포
- \[미리 보기\]: 암호 복잡성 설정이 사용 하도록 설정 되지 않은 Windows Vm 감사
- \[미리 보기\]: 최대 암호 사용 기간이 70 일인 Windows Vm 감사
- \[미리 보기\]: 최소 암호 사용 기간이 1 일인 Windows Vm 감사
- \[미리 보기\]: 최소 암호 길이를 14 자로 제한 하지 않는 Windows Vm 감사
- \[미리 보기\]: 이전 24 개 암호를 다시 사용 하도록 허용 하는 Windows Vm 감사

이 청사진은 Azure Policy 정의를 할당 하 여 Azure 리소스에 대 한 액세스를 제어 하는 데도 도움이 됩니다. 이 정책은 리소스에 더 많은 권한의 액세스를 허용할 수 있는 리소스 유형의 사용을 감사합니다. 이 정책을 위반하는 리소스를 이해하면 Azure 리소스 액세스가 권한 있는 사용자로 제한되도록 정정 작업을 수행하는 데 도움이 될 수 있습니다.

- \[미리 보기\]: 암호가 없는 계정을 포함 하는 Linux Vm을 감사 하기 위한 요구 사항 배포
- \[미리 보기\]: 암호 없이 계정에서 원격 연결을 허용 하는 Linux Vm을 감사 하기 위한 요구 사항 배포
- \[미리 보기\]: 암호가 없는 계정을 포함 하는 Linux Vm 감사
- \[미리 보기\]: 암호 없이 계정에서 원격 연결을 허용 하는 Linux Vm 감사
- 저장소 계정은 새 Azure Resource Manager 리소스로 마이그레이션해야 합니다.
- 가상 컴퓨터를 새 Azure Resource Manager 리소스로 마이그레이션해야 합니다.
- 관리 디스크를 사용하지 않는 VM 감사

## <a name="11-external-interface-protection"></a>11 외부 인터페이스 보호

이 청사진은 적절 한 보안 사용자 관리에 대해 25 개 이상의 정책을 사용 하는 것 외에는 무제한 저장소 계정을 모니터링 하는 [Azure Policy](../../../policy/overview.md) 정의를 할당 하 여 무단 액세스 로부터 서비스 인터페이스를 보호 하는 데 도움이 됩니다. 무제한 액세스 권한이 있는 저장소 계정은 정보 시스템 내에 포함 된 정보에 대 한 의도 하지 않은 액세스를 허용할 수 있습니다. 또한이 청사진은 가상 컴퓨터에서 적응 응용 프로그램 제어를 사용 하도록 설정 하는 정책을 할당 합니다.

- 스토리지 계정에 대한 무제한 네트워크 액세스 감사
- 적응형 응용 프로그램 제어는 가상 머신에서 사용 하도록 설정 해야 합니다.

## <a name="12-secure-service-administration"></a>12 보안 서비스 관리

Azure는 Azure의 리소스에 대 한 액세스 권한이 있는 사용자를 관리 하는 데 도움이 되는 RBAC (역할 기반 액세스 제어)를 구현 합니다. Azure Portal을 사용하여 Azure 리소스에 대한 액세스 권한이 있는 사용자 및 해당 사용자의 권한을 검토할 수 있습니다. 이 청사진은 5 개의 [Azure Policy](../../../policy/overview.md) 정의를 할당 하 여 권한 있는 액세스 권한을 제한 하 고 제어 합니다 .이를 통해 소유자 및/또는 쓰기 권한 및 소유자가 있는 계정 및/또는 쓰기 권한이 없는 계정을 감사 합니다. multi-factor authentication을 사용 하도록 설정 했습니다.

클라우드 서비스 관리에 사용되는 시스템에는 해당 서비스에 대해 높은 권한의 액세스가 있습니다. 이러한 시스템이 손상되면 보안 제어를 우회하고 대용량의 데이터를 도용하거나 조작할 수단을 비롯하여 상당한 영향을 미칩니다. 서비스 공급자의 관리자가 운영 서비스를 관리 하는 데 사용 하는 방법은 서비스의 보안을 강화 하는 악용 위험을 완화 하도록 설계 되어야 합니다. 이 원칙이 구현 되지 않은 경우 공격자는 보안 제어를 우회 하 고 대량의 데이터를 도용 또는 조작 하는 수단을 가질 수 있습니다.

- 구독에 대 한 소유자 권한이 있는 계정에서 MFA를 사용 하도록 설정 해야 합니다.
- 구독에 대 한 쓰기 권한이 있는 계정으로 MFA를 사용 하도록 설정 해야 합니다.
- 소유자 권한이 있는 외부 계정을 구독에서 제거 해야 합니다.
- 쓰기 권한이 있는 외부 계정을 구독에서 제거해야 합니다.

이 청사진은 Azure Policy 정의를 할당 하 여 SQL server 및 Service Fabric에 대 한 Azure Active Directory 인증 사용을 감사 합니다. Azure Active Directory 인증을 사용하면 데이터베이스 사용자 및 기타 Microsoft 서비스의 권한을 간편하게 관리하고 ID를 한 곳에서 집중적으로 관리할 수 있습니다.

- SQL server에 대 한 Azure Active Directory 관리자를 프로 비전 해야 합니다.
- Service Fabric 클러스터는 클라이언트 인증용 으로만 Azure Active Directory를 사용 해야 합니다.

또한이 청사진은 사용 계정 및 승격 된 권한이 있는 외부 계정을 포함 하 여 검토를 위해 우선 순위를 지정 해야 하는 계정을 감사 하는 Azure Policy 정의를 할당 합니다. 필요한 경우 계정을 로그인하지 못하게 차단하거나 제거하여 Azure 리소스에 대한 액세스 권한을 즉시 제거할 수 있습니다. 이 청사진은 사용 계정을 감사 하는 두 가지 Azure Policy 정의를 할당 하 여 제거를 고려해 야 합니다.

- 사용 되지 않는 계정은 구독에서 제거 해야 합니다.
- 소유자 권한이 있는 사용 되지 않는 계정은 구독에서 제거 해야 합니다.
- 소유자 권한이 있는 외부 계정을 구독에서 제거 해야 합니다.
- 쓰기 권한이 있는 외부 계정을 구독에서 제거해야 합니다.

또한이 청사진은 잘못 설정 된 경우 경고에 대 한 Linux VM 암호 파일 사용 권한을 감사 하는 Azure Policy 정의를 할당 합니다. 이 설계를 통해 인증자 손상 되지 않도록 수정 작업을 수행할 수 있습니다.

- \[미리 보기\]: Audit Linux VM/etc/passwd file 권한이 0644으로 설정 되어 있습니다.

## <a name="13-audit-information-for-users"></a>13 사용자에 대 한 감사 정보

이 청사진은 Azure 리소스에 대 한 로그 설정을 감사 하는 [Azure Policy](../../../policy/overview.md) 정의를 할당 하 여 시스템 이벤트를 기록 하도록 도와줍니다. 또한 할당된 정책은 가상 머신이 로그를 지정된 로그 분석 작업 영역에 보내지 않는지도 감사합니다.

- Azure Security Center에서 감사 되지 않은 SQL server 모니터링
- 진단 설정 감사
- SQL 감사 서버 수준 감사 설정
- \[미리 보기\]: Linux VM용 Log Analytics 에이전트 배포
- \[미리 보기\]: Windows VM용 Log Analytics 에이전트 배포
- 가상 네트워크를 만들 때 Network Watcher 배포

## <a name="next-steps"></a>다음 단계

이제 영국 공식 및 영국 NHS 청사진의 제어 매핑을 검토 했으므로 다음 문서를 방문 하 여이 샘플을 배포 하는 방법 및 개요에 대해 알아보세요.

> [!div class="nextstepaction"]
> [Uk 공식 및 영국 nhs 청사진-개요](./index.md)
> [영국 공식 및 영국 nhs 청사진-배포 단계](./deploy.md)

청사진 및 사용 방법에 대한 추가 문서:

- [청사진 수명 주기](../../concepts/lifecycle.md)에 대해 알아보기
- [정적 및 동적 매개 변수](../../concepts/parameters.md) 사용 방법 이해
- [청사진 시퀀싱 순서](../../concepts/sequencing-order.md)를 사용자 지정하는 방법 알아보기
- [청사진 리소스 잠금](../../concepts/resource-locking.md)을 활용하는 방법 알아보기
- [기존 할당을 업데이트](../../how-to/update-existing-assignments.md)하는 방법 알아보기
