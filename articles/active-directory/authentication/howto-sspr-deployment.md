---
title: 셀프 서비스 암호 재설정 배포 계획-Azure Active Directory
description: Azure AD 셀프 서비스 암호의 성공적인 구현 위한 전략을 다시 설정
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8b566bfc3f6c49f6cb9fe31f166356f6ae351e38
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67440933"
---
# <a name="deploy-azure-ad-self-service-password-reset"></a>배포할 Azure AD 셀프 서비스 암호 재설정

셀프 서비스 암호 재설정 (SSPR)에 IT 직원에 게 문의 하지 않고도 암호를 재설정할 수 있는 Azure Active Directory 기능입니다. 직원에 대 한 등록 해야 합니다 또는 셀프 서비스 암호 재설정 서비스를 사용 하 여 등록 합니다. 등록 하는 동안 직원이 조직에서 사용 하도록 설정 하는 하나 이상의 인증 방법을 선택 합니다.

SSPR에는 직원 들을 빠르게 차단을 위치 또는 하루 중 시간에 관계 없이 작업을 계속할 수 있습니다. 를 허용 자체를 차단 해제 하 여 조직의 생산성 아닌 시간 및 가장 일반적인 암호 관련 문제에 대 한 높은 지원 비용을 줄일 수 있습니다.

다른 응용 프로그램 또는 조직에서 서비스와 함께 SSPR을 배포 하 여 신속 하 게 등록 수 있도록 합니다. 이 작업 대량의 로그인 생성 및 등록 하 게 됩니다.

SSPR을 배포 하기 전에 조직 얼마나 많은 암호 재설정 관련된 문의가 발생 시간 및 각 호출의 평균 비용을 확인할 필요가 있습니다. SSPR 조직에 가져오는 값을 표시 하려면이 데이터 post 배포를 사용할 수 있습니다.  

## <a name="how-sspr-works"></a>SSPR 작동 방법

1. 사용자가 암호를 재설정 하려고 하는 경우는 이전에 등록 된 인증 방법 또는 id를 증명 하는 방법 확인 해야 합니다.
1. 그런 다음 사용자가 새 암호를 입력 합니다.
   1. 클라우드 전용 사용자에 대 한 새 암호는 Azure Active Directory에 저장 됩니다. 자세한 내용은 문서 참조 [어떻게 SSPR 작동](concept-sspr-howitworks.md#how-does-the-password-reset-portal-work)합니다.
   1. 하이브리드 사용자에 대 한 암호는 Azure AD Connect 서비스를 통해 온-프레미스 Active Directory에 다시 기록 됩니다. 자세한 내용은 문서 참조 [비밀 번호 쓰기 저장 이란](concept-sspr-writeback.md#how-password-writeback-works)합니다.

## <a name="licensing-considerations"></a>라이선스 고려 사항

Azure Active Directory는 같지만 기능에 대 한 적절 한 라이선스가 있어야 각 사용자가 라이선스 사용자별 의미 합니다.

- 클라우드 전용 사용자 용 셀프 서비스 암호 재설정은 Azure AD basic 이상에 사용할 수 있습니다.
- 셀프 서비스 암호를 하이브리드 환경에 Azure AD Premium P1 필요 이상의 온-프레미스 쓰기 저장을 사용 하 여 재설정 합니다.

라이선스에 대 한 자세한 내용은에서 확인할 수 있습니다는 [Azure Active Directory 가격 책정 페이지](https://azure.microsoft.com/pricing/details/active-directory/)

## <a name="enable-combined-registration-for-sspr-and-mfa"></a>SSPR 및 MFA를 위한 결합 된 등록을 사용 하도록 설정

조직에서는 SSPR 및 multi-factor authentication에 대 한 결합 된 등록 환경 수 있도록 하는 것이 좋습니다. 이 결합 된 등록 환경을 사용 하도록 설정 하면 사용자는 두 기능을 사용 하도록 설정 하려면 해당 등록 정보를 한 번만 선택 해야 합니다.

![결합 된 보안 정보 등록](./media/howto-sspr-deployment/combined-security-info.png)

결합 된 등록 환경에는 조직에서는 SSPR와 Azure Multi-factor Authentication에 사용할 수 있도록 필요 하지 않습니다. 결합 된 등록 환경 조직이 기존 개별 구성 요소에 비해 더 나은 사용자 환경을 제공 합니다. 결합 된 등록 및을 사용 하도록 설정 하는 방법에 대 한 자세한 내용은 문서에서 찾을 수 있습니다 [보안 정보 등록 (미리 보기)를 결합 합니다.](concept-registration-mfa-sspr-combined.md)

## <a name="plan-the-configuration"></a>구성 계획

권장 되는 값과 함께 SSPR을 사용 하도록 설정 하려면 다음 설정이 필요 합니다.

| 영역 | 설정 | 값 |
| --- | --- | --- |
| **SSPR 속성** | 셀프 서비스 암호 재설정 사용 | **선택한** 파일럿에 대 한 그룹 / **모든** 프로덕션용 |
| **인증 방법** | 등록 하는 데 필요한 인증 방법 | 재설정에 필요한 것 보다 항상 1 개 더 |
|   | 재설정에 필요한 인증 방법 | 1개 또는 2개 |
| **등록** | 로그인 시 사용자가 등록하도록 요구 | 예 |
|   | 사용자가 인증 정보를 다시 확인 하기 전의 일 수 | 90-180 일 |
| **알림** | 사용자에게 암호 재설정에 대해 알림 | 예 |
|   | 다른 관리자가 암호를 재설정하면 모든 관리자에게 알림 | 예 |
| **사용자 지정** | 기술 지원팀 링크 사용자 지정 | 예 |
|   | 사용자 지정 기술 지원팀 전자 메일 또는 URL | 사이트 또는 전자 메일 주소를 지원 합니다. |
| **온-프레미스 통합** | 온-프레미스에 암호 다시 쓰기 AD | 예 |
|   | 암호를 재설정 하지 않고 계정의 잠금을 해제 하는 작업을 할 수 있습니다. | 예 |

### <a name="sspr-properties-recommendations"></a>SSPR 속성 권장 사항

셀프 서비스 암호 재설정을 사용 하도록 설정 하는 경우에 파일럿 기간 동안 사용할 보안 그룹을 선택 합니다.

보다 광범위 하 게 서비스를 시작 하려고 할 때 모든 옵션을 사용 하 여 조직의 모든 사용자에 대 한 SSPR을 적용 하는 것이 좋습니다. 경우 모두, 적절 한 Azure AD 보안 그룹 선택 또는 Azure AD와 동기화 된 AD 그룹에 설정할 수 없습니다.

### <a name="authentication-methods"></a>인증 방법

재설정에 필요한 수보다 하나 이상에 등록 하는 데 필요한 인증 방법을 설정 합니다. 여러 허용 유연 하 게 사용자를 다시 설정 해야 할 때입니다.

설정할 **재설정에 필요한 메서드의 수** 조직에 적합 한 수준으로 합니다. 하나는 최소 마찰을 필요 하 고 일부는 두 보안 태세를 늘릴 수 있습니다.

참조 [인증 방법 이란](concept-authentication-methods.md) 인증 방법을 사용할 수 있습니다 SSPR, 미리 정의 된 보안 질문 및 사용자 지정된 보안 질문을 만드는 방법에 대 한 자세한 내용은 합니다.

### <a name="registration-settings"></a>등록 설정

설정할 **사용자가 로그인 할 때 등록 해야** 하 **예**합니다. 이 설정은 사용자는 강제 로그인 시 등록 하도록 모든 사용자가 보호 하는 것을 의미 합니다.

설정 **사용자가 인증 정보를 다시 확인 하기 전의 일 수** 에 간의 **90** 하 고 **180** 일 조직 비즈니스에 필요 짧은 시간 프레임입니다.

### <a name="notifications-settings"></a>알림 설정

둘 다 구성 합니다 **사용자 암호 재설정에 게 알리시겠습니까** 및 **다른 관리자가 암호를 재설정 하면 모든 관리자에 게 알림** 에 **예**. 선택 **예** 둘 다에서 자신의 암호를 재설정 하 고 암호를 변경할 때 관리자 인식 모든 관리자는 사용자가 인식 함으로써 보안을 강화 합니다. 사용자 또는 관리자가 이러한 알림을 받고 변경 시작하지 없습니다, 하는 경우 즉시는 잠재적인 보안 침해를 보고할 수 있습니다.

### <a name="customization"></a>사용자 지정

사용자 지정 하는 것이 반드시 합니다 **기술 지원팀 전자 메일 또는 URL** 문제가 발생 하는 사용자 도움말을 신속 하 게 볼 수 있도록 합니다. 일반 기술 지원팀 전자 메일 주소 또는 사용자에 잘 알고 있는 웹 페이지에이 옵션을 설정 합니다.

### <a name="on-premises-integration"></a>온-프레미스 통합

하이브리드 환경에 있는 경우 했는지 **온-프레미스에 암호 쓰기 저장 AD** 로 설정 된 **예**합니다. 더 많은 유연성을 제공 하는 대로 Yes로 암호를 재설정 하지 않고 계정을 잠금 해제 허용 사용자를 설정할 수도 있습니다.

### <a name="changingresetting-passwords-of-administrators"></a>관리자 암호 변경/다시 설정

관리자 계정에는 관리자 권한으로 특수 계정입니다. 를 보호 하려면 관리자의 암호를 변경 하려면 다음과 같은 제한 사항이 적용 됩니다.

- 온-프레미스 엔터프라이즈 관리자 또는 도메인 관리자는 SSPR 통해 자신의 암호를 재설정할 수 없습니다. 만 해당 온-프레미스 환경에서 해당 암호를 변경할 수 있습니다. 따라서 권장 하지 동기화 온-프레미스 AD 관리자 계정 Azure AD로 합니다.
- 관리자를 사용할 수 비밀 질문 및 답변 방법으로 암호를 다시 설정 합니다.

### <a name="environments-with-multiple-identity-management-systems"></a>여러 id 관리 시스템을 사용 하 여 환경

SiteMinder, 또는 다른 시스템을 Active Directory에 기록 하는 암호와 같은 도구를 사용 하 여 다른 시스템에 동기화 해야 할 수 있으면 여러 identity 오전 Oracle 같은 온-프레미스 identity 관리자와 같은 환경 내에서 관리 시스템 암호 변경 알림 서비스 (PCNS) 사용 하 여 MIM (Microsoft Identity Manager). 이 더 복잡 한 시나리오에 대 한 정보를 찾기 위해 문서를 참조 [도메인 컨트롤러에 MIM 암호 변경 알림 서비스 배포](https://docs.microsoft.com/microsoft-identity-manager/deploying-mim-password-change-notification-service-on-domain-controller)합니다.

## <a name="plan-deployment-and-support-for-sspr"></a>배포 및 SSPR에 대 한 지원 계획

### <a name="engage-the-right-stakeholders"></a>오른쪽 관련자 참여 유도

기술 프로젝트 실패 하면 일반적으로 그렇게 영향, 결과 및 책임에 일치 하지 않는 기대 때문입니다. 이러한 문제를 방지 하려면 오른쪽 관련자 참여 됩니다 하 고 관련자 프로젝트 입력 및 책임을 문서화 하 여 이해 관계자 역할 프로젝트에는 잘 확인 합니다.

### <a name="communications-plan"></a>통신 계획

통신이 새 서비스의 성공에 중요 합니다. 사전에 서비스를 사용 하는 방법 및 수행할 수 있는 예상 대로 작동 하지 않는 경우 도움을 받는 사용자와 통신 합니다. 검토 합니다 [셀프 서비스 암호 재설정 롤아웃 자료 Microsoft 다운로드 센터에서](https://www.microsoft.com/download/details.aspx?id=56768) 최종 사용자 통신 전략을 계획 하는 방법에 대 한 아이디어입니다.

### <a name="testing-plan"></a>테스트 계획

배포가 예상 대로 작동 하는지 확인 합니다 구현의 유효성을 검사 하는 데 사용할 테스트 사례의 집합 계획 해야 합니다. 다음 표에서 몇 가지 유용한 테스트 시나리오 조직의 정책에 따라 결과 예상 하는 문서에 사용할 수 있습니다.

| 비즈니스 사례 | 예상된 결과 |
| --- | --- |
| SSPR 포털은 회사 네트워크 내에서 액세스할 수 있습니다. | 조직에서 결정 |
| SSPR 포털은 회사 네트워크 외부에서 액세스할 수 | 조직에서 결정 |
| 사용자 암호 재설정을 위해 사용 하지 않는 경우 브라우저에서 사용자 암호 다시 설정 | 사용자가 암호 재설정 흐름에 액세스할 수 없습니다. |
| 사용자가 암호 재설정에 등록 되지 않은 경우 브라우저에서 사용자 암호 다시 설정 | 사용자가 암호 재설정 흐름에 액세스할 수 없습니다. |
| 사용자가 암호 재설정 등록의 경우에 적용 됩니다. | 사용자는 보안 정보를 등록 하 라는 메시지가 표시 됩니다. |
| 사용자가 암호 재설정 등록의 경우 로그인 완료 | 사용자 보안 정보를 등록 하 라는 메시지가 표시 되지 않습니다. |
| 사용자 라이선스가 없는 경우 SSPR 포털에 액세스할 수 | 액세스할 수 있습니다. |
| 사용자가 등록 한 후에 Windows 10 AADJ 또는 H + AADJ 장치 잠금 화면에서 사용자 암호를 다시 설정 | 사용자 암호를 재설정할 수 있습니다. |
| SSPR 등록 및 사용 현황 데이터는 거의 실시간으로 관리자에 게 제공 | 감사 로그를 통해 사용할 수 |

### <a name="support-plan"></a>지원 계획

SSPR 일반적으로 사용자를 만들지 않습니다, 하지만 반드시 지원이 직원 발생할 수 있는 문제를 처리 하도록 준비 합니다.

관리자 수 변경 또는 Azure AD 포털을 통해 최종 사용자에 대 한 암호를 다시 설정, 하는 동안 셀프 서비스 지원 프로세스를 통해 문제를 해결 하는 것이 좋습니다.

이 문서의 운영 가이드 섹션에서는 지원 사례 및, 가능한 원인 목록을 만들고 확인에 대 한 지침을 만듭니다.

### <a name="auditing-and-reporting"></a>감사 및 보고

배포 후에 여러 조직에서는 SSPR(셀프 서비스 암호 다시 설정)을 실제로 사용하는 방법 및 경우를 알아보려고 합니다. Azure AD(Azure Active Directory)에서 제공하는 보고 기능은 미리 작성된 보고서를 사용하여 질문에 대답하도록 도와줍니다.

등록 및 암호 재설정에 대 한 감사 로그는 30 일 동안 사용할 수 있습니다. 따라서 기업 내 보안 감사에 더 긴 보존이 필요한 경우 로그 해야 내보내고를 SIEM 도구와 같은 사용 [Azure Sentinel](../../sentinel/connect-azure-active-directory.md), Splunk, 또는 ArcSight 합니다.

아래와 같은 테이블의 백업 일정, 시스템 및 책임 파티를 문서화 합니다. 감사 및 보고 백업 구분 필요 하지 않을 수 있지만 문제에서 복구할 수 있는 별도 백업 해야 합니다.

|   | 다운로드의 빈도 | 대상 시스템 | 책임 파티 |
| --- | --- | --- | --- |
| 감사 백업 |   |   |   |
| 보고 백업 |   |   |   |
| 재해 복구 백업 |   |   |   |

## <a name="implementation"></a>구현

구현에서 3 단계로 발생합니다.

- 사용자 및 라이선스 구성
- Azure AD SSPR 등록 및 셀프 서비스에 대 한 구성
- 비밀 번호 쓰기 저장에 대 한 Azure AD Connect 구성

### <a name="communicate-the-change"></a>변경 내용을 전달합니다

계획 단계에서 개발 하는 통신 계획의 구현을 시작 합니다.

### <a name="ensure-groups-are-created-and-populated"></a>그룹 만들어지고 채워집니다 확인

계획 암호 인증 방법 섹션을 참조 하 고 파일럿 또는 프로덕션 구현에 대 한 그룹을 사용할 모든 적절 한 사용자 그룹에 추가 됩니다을 확인 합니다.

### <a name="apply-licenses"></a>라이선스를 적용 합니다.

구현 하려는 그룹에는 Azure AD를 사용 해야 합니다. premium 라이선스가 할당 합니다. 그룹에 직접 라이선스를 할당할 수 있습니다 하거나 PowerShell 또는 그룹 기반 라이선스를 통해 같은 기존 라이선스 정책을 사용할 수 있습니다.

문서에서 사용자 그룹에 라이선스를 할당 하는 방법에 대 한 정보를 찾을 수 있습니다 [Azure Active Directory에서 그룹 멤버 자격 별로 사용자에 게 라이선스를 할당](../users-groups-roles/licensing-groups-assign.md)합니다.

### <a name="configure-sspr"></a>SSPR을 구성 합니다.

#### <a name="enable-groups-for-sspr"></a>SSPR에 대 한 그룹을 사용 하도록 설정

1. 관리자 계정으로 Azure portal에 액세스 합니다.
1. 모든 서비스를 선택 하 고 필터 상자에 Azure Active Directory를 입력 하 고 Azure Active Directory를 선택 합니다.
1. Active Directory 블레이드에서 암호 재설정을 선택 합니다.
1. 속성 창에서 선택한을 선택 합니다. 원하는 경우 모든 사용할 수 있는 사용자를 모두 선택 합니다.
1. 기본 암호 정책 블레이드 다시 설정, 첫 번째 그룹의 이름을 입력를 클릭 하 고 화면 맨 아래에 선택한 다음 저장 화면 맨 위에 있는 합니다.
1. 각 그룹에 대해이 프로세스를 반복 합니다.

#### <a name="configure-the-authentication-methods"></a>인증 방법 구성

이 문서의 암호 인증 방법 계획 섹션에서 계획을 참조 합니다.

1. 등록을 선택, 아래에 있는 사용자를 로그인 할 때 등록 예 선택 하 고 만료 전 일 수를 설정 합니다 필요 하 고 저장을 선택 합니다.
1. 알림을 선택 하 고 사용자 계획 당 구성 하 고 저장을 선택 합니다.
1. 사용자 지정을 선택 하 고 사용자 계획 당 구성 하 고 저장을 선택 합니다.
1. 온-프레미스 통합을 선택 하 고 사용자 계획 당 구성 하 고 저장을 선택 합니다.

### <a name="enable-sspr-in-windows"></a>Windows에서 SSPR을 사용 하도록 설정

Windows 10 1803 이상 버전을 실행 하거나 Azure AD에 가입 된 장치나 하이브리드 Azure AD 조인 된 Windows 로그인 화면에서 암호를 재설정할 수 있습니다. 이 문서에서 정보와이 기능을 구성 하는 단계를 찾을 수 있습니다 [로그인 화면에서 Azure AD 암호](tutorial-sspr-windows.md)

### <a name="configure-password-writeback"></a>비밀번호 쓰기 저장 구성

조직의 문서에서 찾을 수 있습니다에 대 한 비밀 번호 쓰기 저장을 구성 하는 단계 [방법: 비밀 번호 쓰기 저장 구성](howto-sspr-writeback.md)합니다.

## <a name="manage-sspr"></a>SSPR 관리

셀프 서비스 암호 재설정을 사용 하 여 연결 하는 기능을 관리 하려면 필요한 역할입니다.

| 비즈니스 역할/가상 사용자 | Azure AD 역할 (필요한 경우) |
| :---: | :---: |
| 수준 1 지원 센터 | 암호 관리자 |
| 수준 2 지원 센터 | 사용자 관리자 |
| SSPR 관리자 | 전역 관리자 |

### <a name="support-scenarios"></a>시나리오를 지원 합니다.

지원 팀 성공의 사용 하려면 사용자가 받게 되는 질문을 바탕으로 하는 FAQ를 만들 수 있습니다. 다음 표에서 일반적인 지원 시나리오를 보여 줍니다.

| 시나리오 | 설명 |
| --- | --- |
| 사용자에 게 사용 가능한 모든 등록 된 인증 방법 | 사용자 자신의 암호를 재설정 하려고 하지만 어떤 인증 방법을 사용할 수 있는 등록 된 (예: 집 해당 휴대폰을 유지 하며 전자 메일에 액세스할 수 없습니다) |
| 사용자가 텍스트 메시지를 받지 않거나 해당 사무실 이나 휴대폰의 호출 | 사용자가 텍스트 또는 호출을 통해 자신의 id를 확인 하려고 했지만 텍스트/호출을 받고 있지 않습니다. |
| 사용자 암호 재설정 포털에 액세스할 수 없습니다. | 사용자는 자신의 암호를 재설정 하려고 하지만 암호 재설정을 위해 사용 되지 않으며 따라서 암호 업데이트 페이지에 액세스할 수 없습니다. |
| 사용자는 새 암호를 설정할 수 없습니다. | 사용자 암호 다시 설정 흐름 중에 확인을 완료 했지만 새 암호를 설정할 수 없습니다. |
| 사용자가 Windows 10 장치에서 암호 재설정 링크를 볼 | 사용자가 Windows 10 잠금 화면에서 암호를 재설정 하려고 하지만 장치가 하거나 Azure AD에 가입 되지 않거나 Intune 장치 정책을 지원 하지 않습니다. |

추가적인 문제 해결에 대 한 다음과 같은 정보를 포함할 수도 있습니다.

- SSPR에 대 한 어떤 그룹이 사용 됩니다.
- 어떤 인증 방법이 구성 되어 있습니다.
- 회사 네트워크의 관련 된 액세스 정책.
- 일반적인 시나리오에 대 한 문제 해결 단계입니다.

셀프 서비스 암호 재설정 SSPR는 가장 일반적인 시나리오에 대 한 일반적인 문제 해결 단계를 이해 하려면 문제 해결에 대 한 온라인 설명서를 참조할 수 있습니다.

## <a name="next-steps"></a>다음 단계

- [Azure AD 암호 보호를 구현 하는 것이 좋습니다.](concept-password-ban-bad.md)

- [Azure AD 스마트 잠금을 구현 하는 것이 좋습니다.](howto-password-smart-lockout.md)
