---
title: Azure Kubernetes Service에서 지원되는 Kubernetes 버전
description: AKS(Azure Kubernetes Service)의 Kubernetes 버전 지원 정책 및 클러스터 수명 주기에 대해 알아봅니다.
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: article
ms.date: 05/20/2019
ms.author: saudas
ms.openlocfilehash: a4726444e2103228cd26be3d3a6d516b31e315ee
ms.sourcegitcommit: 770b060438122f090ab90d81e3ff2f023455213b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68304323"
---
# <a name="supported-kubernetes-versions-in-azure-kubernetes-service-aks"></a>AKS(Azure Kubernetes Service)에서 지원되는 Kubernetes 버전

Kubernetes 커뮤니티에서는 대략 3개월마다 부 버전을 릴리스합니다. 이러한 릴리스에는 새로운 기능과 향상된 기능이 포함됩니다. 패치는 더 자주 릴리스되며(매주 릴리스되기도 함), 부 버전의 중요한 버그 수정용으로만 제공됩니다. 이러한 패치 릴리스에는 Kubernetes 기반 프로덕션 환경에서 실행되는 제품 및 다수의 고객에게 영향을 주는 중요한 버그나 보안 취약성용 수정이 포함됩니다.

AKS는 버전의 안정성에 따라 업스트림 릴리스의 30 일 이내에 새로운 Kubernetes 버전을 인증 하 고 릴리스 하는 것을 목표로 합니다.

## <a name="kubernetes-versions"></a>Kubernetes 버전

Kubernetes는 표준 [의미 체계 버전](https://semver.org/) 관리 체계를 사용 합니다. 즉, 각 버전의 Kubernetes는 다음 번호 매기기 체계를 따릅니다.

```
[major].[minor].[patch]

Example:
  1.12.14
  1.12.15
  1.13.7
```

버전의 각 번호는 이전 버전과의 호환성을 나타냅니다.

* 호환 되지 않는 API 변경 또는 이전 버전과의 호환성이 손상 될 때 주 버전은 변경 됩니다.
* 사소한 버전은 다른 부 릴리스와 이전 버전과 호환 되는 기능이 변경 될 때 변경 됩니다.
* 이전 버전과 호환 되는 버그 수정이 수행 될 때 패치 버전이 변경 됩니다.

일반적으로 사용자는 실행 중인 부 버전의 최신 패치 릴리스를 실행 하도록 노력 해야 합니다. 예를 들어 프로덕션 클러스터가 *1.13.6* 에 있고 *1.13.7* 가 *1.13* 시리즈에 사용할 수 있는 최신 패치 버전입니다. 클러스터가 완전히 패치 되 고 지원 되는지 확인 하는 즉시 *1.13.7* 로 업그레이드 해야 합니다.

## <a name="kubernetes-version-support-policy"></a>Kubernetes 버전 지원 정책

AKS에서 지원하는 Kubernetes의 4개 부 버전은 다음과 같습니다.

* AKS (N)에서 릴리스된 현재 부 버전
* 이전 부 버전 3개. 지원되는 각 부 버전은 안정적인 패치 2개도 지원합니다.

이를 "N-1"-(N (최신 릴리스)-3 (부 버전)) 이라고 합니다.

예를 들어 AKS이 현재 *1.13 x. x* 를 도입 하는 경우 다음 버전에 대 한 지원이 제공 됩니다.

새 부 버전    |    지원 되는 버전 목록
-----------------    |    ----------------------
1.13.x               |    1.12.a, 1.12.b, 1.11.a, 1.11.b, 1.10.a, 1.10.b

여기서 "x"와 ".a" 및 ".b"는 대표적인 패치 버전입니다.

버전 변경 및 기대치와 관련 된 통신에 대 한 자세한 내용은 아래의 "통신"을 참조 하십시오.

새 부 버전이 도입 되 면 지원 되는 가장 오래 된 부 버전 및 패치 릴리스가 더 이상 사용 되지 않고 제거 됩니다. 예를 들어 현재 지원 되는 버전 목록은 다음과 같습니다.

```
1.12.a
1.12.b
1.11.a
1.11.b
1.10.a
1.10.b
1.9.a
1.9.b
```

그리고 AKS 버전을 사용 하는 경우 1.9. x 버전 (모든 1.9 버전)이 제거 되 고 지원 되지 않습니다.

> [!NOTE]
> 고객이 지원 되지 않는 Kubernetes 버전을 실행 하는 경우 클러스터에 대 한 지원을 요청할 때 업그레이드 하 라는 메시지가 표시 됩니다. 지원 되지 않는 Kubernetes 릴리스를 실행 하는 클러스터는 [AKS 지원 정책](https://docs.microsoft.com/azure/aks/support-policies)에 포함 되지 않습니다.


위의 사소한 버전에서 AKS는 지정 된 부 버전의 두 가지 최신 *패치** 릴리스를 지원 합니다. 예를 들어 다음과 같은 지원 되는 버전이 있다고 가정 합니다.

```
Current Supported Version List
------------------------------
1.12.1, 1.12.2, 1.11.4, 1.11.5
```

업스트림 Kubernetes 릴리스된 1.12.3 및 1.11.6 및 AKS가 해당 패치 버전을 해제 하면 가장 오래 된 패치 버전이 더 이상 사용 되지 않고 제거 되며 지원 되는 버전 목록은 다음과 같이 됩니다.

```
New Supported Version List
----------------------
1.12.*2*, 1.12.*3*, 1.11.*5*, 1.11.*6*
```

> [!NOTE]
> 고객은 클러스터 생성, CI 또는 다른 자동화 된 작업을 특정 패치 릴리스에 고정 해서는 안 됩니다. 

### <a name="communications"></a>통신

* Kubernetes의 새 **부** 버전
  * 모든 사용자에 게는 새 버전을 공개적으로 알리고 제거 되는 버전이 표시 됩니다.
  * 새 패치 버전이 릴리스되면 가장 오래 된 패치 릴리스가 동시에 제거 됩니다.
  * 고객은 공개 알림 날짜 로부터 **60 일 이내** 에 지원 되는 부 버전 릴리스로 업그레이드할 수 있습니다.
* Kubernetes의 새로운 **패치** 버전
  * 모든 사용자에 게 릴리스되는 새 패치 버전에 대 한 알림이 표시 되 고 최신 패치 릴리스로 업그레이드 됩니다.
  * 사용자는 **30 일 동안** 최신의 지원 되는 패치 릴리스로 업그레이드 합니다. 사용자는 **30 일** 후에 지원 되는 패치 릴리스로 업그레이드 한 후 가장 오래 된 것을 제거 합니다.

AKS는 일반 공급으로 "릴리스"를 정의 하 고 모든 SLO/서비스 측정 품질에서 사용 가능 하며 모든 지역에서 사용할 수 있습니다.

> [!NOTE]
> Kubernetes 버전 릴리스 및 결함에 대 한 알림이 표시 됩니다. 부 버전이 사용 되지 않거나 제거 되 면 지원 되는 릴리스로 업그레이드 하는 데 60 일이 제공 됩니다. 패치 릴리스의 경우 고객이 지원 되는 릴리스로 업그레이드 하는 데 30 일이 주어 집니다.

알림은 다음을 통해 전송 됩니다.

* [AKS 릴리스 정보](https://aka.ms/aks/releasenotes)
* Azure Portal 알림
* [Azure 업데이트 채널][azure-update-channel]

### <a name="policy-exceptions"></a>정책 예외

AKS는 하나 이상의 중요 한 프로덕션에 영향을 주는 버그 또는 보안 문제를 사전에 확인 하지 않고 식별 된 새/기존 버전을 추가 하거나 제거할 권리를 보유 합니다.

특정 패치 릴리스를 건너뛰거나 버그 또는 보안 문제의 심각도에 따라 롤아웃 가속화를 사용할 수 있습니다.

### <a name="azure-portal-and-cli-default-versions"></a>Azure Portal 및 CLI 기본 버전

포털 또는 Azure CLI에서 AKS 클러스터를 배포 하는 경우 클러스터는 항상 N-1 부 버전 및 최신 패치로 설정 됩니다. 예를 들어 AKS가 1.13, *1.12* + , 1.12.*b*, *1.11* + .*b*, *1.10* + . *x x* *b*를 지 원하는 경우 새 클러스터에 대 한 기본 버전은 *1.12입니다. b* .

AKS은 기본적으로 고객에 게 알려지고 안정적 이며 패치가 적용 된 버전을 제공 하기 위해 기본적으로 N-1 (latestPatch, 예: 1.12. b)로 설정 됩니다.

## <a name="list-currently-supported-versions"></a>현재 지원되는 버전 목록

현재 구독과 지역에서 사용할 수 있는 버전을 확인 하려면 [az aks][az-aks-get-versions] 명령을 사용 합니다. 다음 예제에는 *EastUS* 지역에 제공되는 Kubernetes 버전이 나열되어 있습니다.

```azurecli-interactive
az aks get-versions --location eastus --output table
```

출력은 Kubernetes version *1.13.5* 을 사용할 수 있는 최신 버전 임을 보여 주는 다음 예제와 유사 합니다.

```
KubernetesVersion    Upgrades
-------------------  ------------------------
1.13.5               None available
1.12.7               1.13.5
1.12.6               1.12.7, 1.13.5
1.11.9               1.12.6, 1.12.7
1.11.8               1.11.9, 1.12.6, 1.12.7
1.10.13              1.11.8, 1.11.9
1.10.12              1.10.13, 1.11.8, 1.11.9
```

## <a name="faq"></a>FAQ

**고객이 지원되지 않는 부 버전을 사용하여 Kubernetes 클러스터를 업그레이드하면 어떻게 되나요?**

*N-4* 버전을 사용할 경우에는 지원 되지 않으며 업그레이드 하 라는 메시지가 표시 됩니다. 버전 n-4에서 n-3으로 업그레이드 하는 경우 이제 지원 정책에 포함 됩니다. 예:

- 지원 되는 AKS 버전이 *1.13. x*, *1.12* + .*b*, *1.11. c* + *1.11 d*및 *1.10. e* + *1.10 f* 와 사용자가 *1.9* 또는 *1.9. h*를 사용할 때 지원 되지 않습니다.
- *1.9* 또는 *1.9* 에서 *1.10. e* 로 업그레이드 하는 경우 또는 *1.10. f* 에 성공 하면 지원 정책 내에서로 돌아갑니다.

*n-4*보다 오래된 버전으로의 업그레이드는 지원되지 않습니다. 이러한 경우 고객은 새 AKS 클러스터를 만들고 워크로드를 다시 배포하는 것이 좋습니다.

**' 지원 되지 않음 '의 의미**

' 지원 외부 '는 실행 중인 버전이 지원 되는 버전 목록 외부에 있음을 의미 하며 지원을 요청할 때 클러스터를 지원 되는 버전으로 업그레이드 하 라는 메시지가 표시 됩니다. 또한 AKS은 지원 되는 버전 목록 외부의 클러스터에 대 한 런타임 또는 다른 보증을 수행 하지 않습니다.

**고객이 지원되지 않는 부 버전을 사용하여 Kubernetes 클러스터를 확장하면 어떻게 되나요?**

AKS에서 지원되지 않는 부 버전에서도 규모 감축이나 확장은 문제 없이 계속 작동합니다.

**고객은 특정 Kubernetes 버전을 계속 사용할 수 있나요?**

예. 그러나 클러스터가 AKS에서 지원 되는 버전 중 하나에 있지 않은 경우 클러스터는 AKS 지원 정책에서 제외 됩니다. Azure는 클러스터를 자동으로 업그레이드하거나 삭제하지 않습니다.

**에이전트 클러스터가 지원되는 AKS 버전 중 하나가 아닌 경우 마스터는 어떤 버전을 지원하나요?**

마스터는 지원되는 최신 버전으로 자동 업데이트됩니다.

## <a name="next-steps"></a>다음 단계

클러스터를 업그레이드 하는 방법에 대 한 자세한 내용은 [Azure Kubernetes 서비스 (AKS) 클러스터 업그레이드][aks-upgrade]를 참조 하세요.

<!-- LINKS - External -->
[aks-engine]: https://github.com/Azure/aks-engine
[azure-update-channel]: https://azure.microsoft.com/updates/?product=kubernetes-service

<!-- LINKS - Internal -->
[aks-upgrade]: upgrade-cluster.md
[az-aks-get-versions]: /cli/azure/aks#az-aks-get-versions
