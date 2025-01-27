---
title: Azure AD-Azure Active Directory에서에서 취약 한 암호를 차단 하는 방법
description: Azure AD의 동적으로 금지된 암호를 사용하여 취약한 암호를 환경에서 금지할 수 있습니다.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: f531174c889948308e27109ab4fd80a481ec6bdc
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798197"
---
# <a name="configuring-the-custom-banned-password-list"></a>사용자 지정 금지 암호 목록 구성

많은 조직의 사용자가 일반적인 현지 단어(예: 학교, 스포츠 팀 또는 유명 인사)를 사용하여 암호를 생성하기 때문에 암호를 추측하기가 쉽습니다. Microsoft의 사용자 지정 금지 암호 목록을 사용하면 전역 금지된 암호 목록 이외에도 사용자와 관리자가 암호를 변경하거나 재설정하려고 할 때 평가하여 차단할 수 있는 문자열을 추가할 수 있습니다.

## <a name="add-to-the-custom-list"></a>사용자 지정 목록에 추가

사용자 지정 금지 암호 목록을 구성하려면 Azure Active Directory Premium P1 또는 P2 라이선스가 필요합니다. Azure Active Directory 라이선스에 대 한 정보를 자세한 참조를 [Azure Active Directory 가격 책정 페이지](https://azure.microsoft.com/pricing/details/active-directory/)합니다.

1. 에 로그인 하는 [Azure portal](https://portal.azure.com) 로 이동한 **Azure Active Directory**, **인증 방법**, 한 다음 **암호 보호**합니다.
1. **Enforce custom list**(사용자 지정 목록 적용) 옵션을 **예**로 설정합니다.
1. **Custom banned password list**(사용자 지정 금지 암호 목록)에 문자열을 한 줄당 하나씩 추가합니다.
   * 사용자 지정 차단된 암호 목록을 최대 1,000 개의 용어를 포함할 수 있습니다.
   * 사용자 지정 금지 암호 목록에서는 대/소문자가 구별됩니다.
   * 사용자 지정 금지 암호 목록에는 일반적인 문자 대체가 고려됩니다.
      * 예: "o"와 "0" 또는 "a"와 "\@"
   * 문자열 최소 길이는 4자이고 최대 길이는 16자입니다.
1. 문자열을 모두 추가했으면 **저장**을 클릭합니다.

> [!NOTE]
> 사용자 지정 금지 암호 목록을 적용하기 위한 업데이트에는 몇 시간 정도가 소요됩니다.

> [!NOTE]
> 금지 된 암호를 사용자 지정 목록에 최대 1000 조건의 제한 됩니다. 매우 큰 암호 목록을 차단에 대 한 설계 되지는 않았습니다. 사용자 지정 차단된 암호 목록 이점은 완벽 하 게 활용 하려면 먼저 검토 하는 것이 좋습니다 의도 한 디자인 및 사용자 지정 금지 된 암호 목록의 사용을 이해 하 고 (참조 [사용자 지정 암호 목록을 차단](concept-password-ban-bad.md#custom-banned-password-list)), 알고리즘과 암호 평가 (참조 [암호 평가 되는 방식을](concept-password-ban-bad.md#how-are-passwords-evaluated)).

![Azure Portal의 인증 방법에서 사용자 지정 금지 암호 목록 수정](./media/howto-password-ban-bad/authentication-methods-password-protection.png)

## <a name="how-it-works"></a>작동 방법

사용자나 관리자가 Azure AD 암호를 다시 설정하거나 변경할 때마다 금지된 암호 목록 전체를 확인하여 목록에 없는 항목인지 확인합니다. 이 검사는 Azure AD를 사용하여 설정되거나 변경되는 모든 암호에 포함됩니다.

## <a name="what-do-users-see"></a>사용자에게 표시되는 내용

사용자를 차단 하는 것에 암호를 재설정 하려고 하면 다음 오류 메시지 중 하나를 참조는:

* 암호에 쉽게 추측할 수 있는 단어, 구 또는 패턴이 포함되어 있습니다. 다른 암호로 다시 시도하세요.
* 그러나 관리자가 차단 된 문자나 단어 포함 되어 있으므로 해당 암호를 사용할 수 없습니다. 다른 암호로 다시 시도하세요.

## <a name="next-steps"></a>다음 단계

[금지된 암호 목록에 대한 개념적 개요](concept-password-ban-bad.md)

[Azure AD 암호 보호에 대한 개념적 개요](concept-password-ban-bad-on-premises.md)

[온-프레미스 통합에서 금지된 암호 목록 사용](howto-password-ban-bad-on-premises.md)
