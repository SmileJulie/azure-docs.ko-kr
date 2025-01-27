---
title: 상태 검사에 대 한 오류 참조-Azure Container Registry
description: Azure Container Registry에서 az acr check 상태 진단 명령을 실행 하 여 발견 된 문제에 대 한 오류 코드 및 가능한 해결 방법
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 07/02/2019
ms.author: danlep
ms.openlocfilehash: 672d446fa8dc27612c7b046cac109bfa4ca5fec5
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68309368"
---
# <a name="health-check-error-reference"></a>상태 검사 오류 참조

다음은 [az acr check 상태][az-acr-check-health] 명령에서 반환 하는 오류 코드에 대 한 세부 정보입니다. 각 오류에 대해 가능한 해결 방법이 나열 됩니다.

## <a name="dockercommanderror"></a>DOCKER_COMMAND_ERROR

이 오류는 CLI 용 Docker 클라이언트를 찾을 수 없음을 의미 합니다. 따라서 다음과 같은 추가 검사는 실행 되지 않습니다. Docker 버전 찾기, Docker 데몬 상태 평가 및 Docker pull 명령 실행.

*잠재적 해결 방법*: Docker 클라이언트를 설치 합니다. 시스템 변수에 Docker 경로를 추가 합니다.

## <a name="dockerdaemonerror"></a>DOCKER_DAEMON_ERROR

이 오류는 Docker 디먼 상태를 사용할 수 없거나 CLI를 사용 하 여 연결할 수 없음을 의미 합니다. 따라서 CLI를 통해 Docker 작업 (예: `docker login` 및 `docker pull`)을 사용할 수 없습니다.

*잠재적 해결 방법*: Docker 디먼을 다시 시작 하거나 제대로 설치 되었는지 확인 합니다.

## <a name="dockerversionerror"></a>DOCKER_VERSION_ERROR

이 오류는 CLI에서 명령을 `docker --version`실행할 수 없음을 의미 합니다.

*잠재적 해결 방법*: 명령을 수동으로 실행 하 고, 최신 CLI 버전이 있는지 확인 하 고, 오류 메시지를 조사 하십시오.

## <a name="dockerpullerror"></a>DOCKER_PULL_ERROR

이 오류는 CLI가 샘플 이미지를 환경으로 끌어올 수 없음을 의미 합니다.

*잠재적 해결 방법*: 이미지를 가져오는 데 필요한 모든 구성 요소가 제대로 실행 되 고 있는지 확인 합니다.

## <a name="helmcommanderror"></a>HELM_COMMAND_ERROR

이 오류는 다른 투구 작업을 불가능 하 게 하는 CLI에서 투구 클라이언트를 찾을 수 없음을 의미 합니다.

*잠재적 해결 방법*: 투구 client가 설치 되어 있고 해당 경로가 시스템 환경 변수에 추가 되었는지 확인 합니다.

## <a name="helmversionerror"></a>HELM_VERSION_ERROR

이 오류는 CLI가 설치 된 투구 버전을 확인할 수 없음을 의미 합니다. 이 문제는 사용 중인 Azure CLI 버전 (또는 투구 버전)이 사용 되지 않는 경우에 발생할 수 있습니다.

*잠재적 해결 방법*: 최신 Azure CLI 버전 또는 권장 되는 투구 버전으로 업데이트 합니다. 명령을 수동으로 실행 하 고 오류 메시지를 조사 합니다.

## <a name="connectivitydnserror"></a>CONNECTIVITY_DNS_ERROR

이 오류는 지정 된 레지스트리 로그인 서버에 대 한 DNS를 ping 했지만 응답 하지 않았음을 의미 합니다. 즉, 사용할 수 없습니다. 이는 일부 연결 문제를 나타낼 수 있습니다. 또는 레지스트리가 없거나, 사용자에 게 레지스트리에 대 한 권한이 없거나 (로그인 서버를 제대로 검색할 수 있음), 대상 레지스트리가 Azure CLI에 사용 된 것과 다른 클라우드에 있는 것일 수 있습니다.

*잠재적 해결 방법*: 연결 유효성 검사 레지스트리의 철자와 레지스트리가 있는지 확인 합니다. 사용자에 게이에 대 한 적절 한 권한이 있는지 확인 하 고 레지스트리의 클라우드가 Azure CLI에 사용 된 것과 동일한 지 확인 합니다.

## <a name="connectivityforbiddenerror"></a>CONNECTIVITY_FORBIDDEN_ERROR

이 오류는 지정 된 레지스트리에 대 한 챌린지 끝점이 403 사용할 수 없음 HTTP 상태로 응답 했음을 의미 합니다. 이 오류는 사용자가 레지스트리에 액세스할 수 없음을 의미 합니다. 가상 네트워크 구성 때문입니다.

*잠재적 해결 방법*: 가상 네트워크 규칙을 제거 하거나 현재 클라이언트 IP 주소를 허용 목록에 추가 하십시오.

## <a name="connectivitychallengeerror"></a>CONNECTIVITY_CHALLENGE_ERROR

이 오류는 대상 레지스트리의 챌린지 끝점에서 챌린지를 발행 하지 않았음을 의미 합니다.

*잠재적 해결 방법*: 잠시 후 다시 시도 하세요. 오류가 계속 발생 하면에서 https://aka.ms/acr/issues 문제를 엽니다.

## <a name="connectivityaadloginerror"></a>CONNECTIVITY_AAD_LOGIN_ERROR

이 오류는 대상 레지스트리의 챌린지 끝점에서 챌린지를 발행 했지만 레지스트리가 Azure Active Directory 인증을 지원 하지 않음을 의미 합니다.

*잠재적 해결 방법*: 예를 들어 관리자 자격 증명을 사용 하 여 인증 하는 다른 방법을 사용해 보세요. 사용자가 Azure Active Directory를 사용 하 여 인증 해야 하는 경우 https://aka.ms/acr/issues 에서 문제를 여세요.

## <a name="connectivityrefreshtokenerror"></a>CONNECTIVITY_REFRESH_TOKEN_ERROR

이 오류는 레지스트리 로그인 서버에서 새로 고침 토큰을 사용 하 여 응답 하지 않아 대상 레지스트리에 대 한 액세스가 거부 되었음을 의미 합니다. 이 오류는 사용자에 게 레지스트리에 대 한 적절 한 권한이 없거나 Azure CLI에 대 한 사용자 자격 증명이 유효 하지 않은 경우에 발생할 수 있습니다.

*잠재적 해결 방법*: 사용자에 게 레지스트리에 대 한 적절 한 권한이 있는지 확인 합니다. 을 `az login` 실행 하 여 사용 권한, 토큰 및 자격 증명을 새로 고칩니다.

## <a name="connectivityaccesstokenerror"></a>CONNECTIVITY_ACCESS_TOKEN_ERROR

이 오류는 레지스트리 로그인 서버에서 액세스 토큰을 사용 하 여 응답 하지 않아 대상 레지스트리에 대 한 액세스가 거부 되었음을 의미 합니다. 이 오류는 사용자에 게 레지스트리에 대 한 적절 한 권한이 없거나 Azure CLI에 대 한 사용자 자격 증명이 유효 하지 않은 경우에 발생할 수 있습니다.

*잠재적 해결 방법*: 사용자에 게 레지스트리에 대 한 적절 한 권한이 있는지 확인 합니다. 을 `az login` 실행 하 여 사용 권한, 토큰 및 자격 증명을 새로 고칩니다.

## <a name="loginservererror"></a>LOGIN_SERVER_ERROR

이 오류는 CLI가 지정 된 레지스트리의 로그인 서버를 찾을 수 없고 현재 클라우드에 대 한 기본 접미사를 찾지 못했음을 의미 합니다. 레지스트리에 대 한 적절 한 권한이 사용자에 게 없거나, 레지스트리의 클라우드와 현재 Azure CLI 클라우드가 일치 하지 않거나, Azure CLI 버전이 사용 되지 않는 경우이 오류가 발생할 수 있습니다.

*잠재적 해결 방법*: 철자가 올바른지와 레지스트리가 있는지 확인 합니다. 사용자에 게 레지스트리에 대 한 올바른 사용 권한이 있는지 확인 하 고 레지스트리 및 CLI 환경의 클라우드가 일치 하는지 확인 합니다. Azure CLI를 최신 버전으로 업데이트 합니다.

## <a name="next-steps"></a>다음 단계

레지스트리의 상태를 확인 하는 옵션은 [Azure container registry의 상태 확인](container-registry-check-health.md)을 참조 하세요.

Azure Container Registry에 대 한 질문과 대답 및 기타 알려진 문제에 대 한 [FAQ](container-registry-faq.md) 를 참조 하세요.





<!-- LINKS - internal -->
[az-acr-check-health]: /cli/azure/acr#az-acr-check-health
