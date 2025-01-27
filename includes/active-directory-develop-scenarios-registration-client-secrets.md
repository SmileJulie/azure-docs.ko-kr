---
title: 포함 파일
description: 기밀 클라이언트 시나리오 방문 페이지 (디먼, 웹 앱, Web API)에 대 한 포함 파일
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/18/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: cd37880be6d518105e880b93a0bd748f7c729d88
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68286174"
---
## <a name="registration-of-secrets-or-certificates"></a>비밀 또는 인증서 등록

기밀 클라이언트 응용 프로그램의 경우와 마찬가지로 비밀 또는 인증서를 등록 해야 합니다. [Azure Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview)의 대화형 경험을 통해 또는 명령줄 도구 (예: PowerShell)를 사용 하 여 응용 프로그램 암호를 등록할 수 있습니다.

### <a name="registering-client-secrets-using-the-application-registration-portal"></a>응용 프로그램 등록 포털을 사용 하 여 클라이언트 암호 등록

클라이언트 자격 증명의 관리는 응용 프로그램의 **인증서 & 암호** 페이지에서 발생 합니다.

![image](../articles/active-directory/develop/media/quickstart-update-azure-ad-app-preview/credentials-certificates-secrets-expanded.png)

- 비밀 클라이언트 응용 프로그램을 등록 하는 동안 Azure AD에서 응용 프로그램 암호 (클라이언트 암호 라고도 하는)가 생성 됩니다. 이 생성은 **새 클라이언트 암호**를 선택할 때 발생 합니다. 이 시점에서 **저장**을 선택 하기 전에 앱에서 사용할 암호 문자열을 클립보드에 복사 해야 합니다. 이 문자열은 더 이상 표시 되지 않습니다.
- 인증서는 **인증서 업로드** 단추를 사용 하 여 응용 프로그램 등록에 업로드 됩니다. Azure AD는 응용 프로그램에 직접 등록 된 인증서만 지원 하며 인증서 체인을 따르지 않습니다.

자세한 내용은 [빠른 시작: 웹 Api에 액세스 하도록 클라이언트 응용 프로그램 구성 | 응용 프로그램에 자격 증명 추가](../articles/active-directory/develop/quickstart-configure-app-access-web-apis.md#add-credentials-to-your-web-application)



### <a name="registering-client-secrets-using-powershell"></a>PowerShell을 사용 하 여 클라이언트 암호 등록

또는 명령줄 도구를 사용 하 여 Azure AD에 응용 프로그램을 등록할 수 있습니다. [Dotnetcore-v2](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2) 샘플은 Azure AD 응용 프로그램을 사용 하 여 응용 프로그램 비밀 또는 인증서를 등록 하는 방법을 보여 줍니다.

- 응용 프로그램 암호를 등록 하는 방법에 대 한 자세한 내용은 [AppCreationScripts/Configure.](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/5199032b352a912e7cc0fce143f81664ba1a8c26/AppCreationScripts/Configure.ps1#L190) p s 1을 참조 하세요.
- 응용 프로그램에 인증서를 등록 하는 방법에 대 한 자세한 내용은 [AppCreationScripts-withCert/Configure.](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/5199032b352a912e7cc0fce143f81664ba1a8c26/AppCreationScripts-withCert/Configure.ps1#L162-L178) p s 1을 참조 하세요.
