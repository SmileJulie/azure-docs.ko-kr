---
title: AKS(Azure Kubernetes Service)에 대한 질문과 대답
description: Azure Kubernetes 서비스 (AKS)에 대 한 일반적인 질문에 대 한 답변을 찾습니다.
services: container-service
author: mlearned
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/08/2019
ms.author: mlearned
ms.openlocfilehash: 554eba87efc56e2dadb3fb2d0cb78cd8b7ea7237
ms.sourcegitcommit: af58483a9c574a10edc546f2737939a93af87b73
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68302719"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>AKS(Azure Kubernetes Service)에 대한 질문과 대답

이 문서에서는 AKS(Azure Kubernetes Service)에 대한 질문과 대답을 제공합니다.

## <a name="which-azure-regions-currently-provide-aks"></a>현재 AKS를 제공 하는 Azure 지역은 무엇 인가요?

사용 가능한 지역에 대 한 전체 목록은 [AKS 지역 및 가용성][aks-regions]을 참조 하세요.

## <a name="does-aks-support-node-autoscaling"></a>AKS는 노드 자동 크기 조정 기능을 지원하나요?

예, AKS에서 에이전트 노드의 수평 크기를 자동으로 조정 하는 기능은 현재 미리 보기로 제공 됩니다. AKSfor instructions. AKS autoscaling is based on the [Kubernetes autoscaler][auto-scaler] [의 응용 프로그램 요구에 맞게 클러스터 크기 자동 조정을][aks-cluster-autoscaler] 참조 하세요.

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>AKS를 기존 가상 네트워크에 배포할 수 있습니까?

예, [고급 네트워킹 기능][aks-advanced-networking]을 사용 하 여 기존 가상 네트워크에 AKS 클러스터를 배포할 수 있습니다.

## <a name="can-i-limit-who-has-access-to-the-kubernetes-api-server"></a>Kubernetes API 서버에 대 한 액세스 권한이 있는 사용자를 제한할 수 있나요?

예, 현재 미리 보기 상태인 [Api Server 권한 있는 IP 범위][api-server-authorized-ip-ranges]를 사용 하 여 Kubernetes api 서버에 대 한 액세스를 제한할 수 있습니다.

## <a name="can-i-make-the-kubernetes-api-server-accessible-only-within-my-virtual-network"></a>Kubernetes API 서버를 가상 네트워크 내 에서만 액세스할 수 있나요?

지금은 계획 되지 않았습니다. [AKS GitHub 리포지토리][private-clusters-github-issue]에서 진행률을 추적할 수 있습니다.

## <a name="can-i-have-different-vm-sizes-in-a-single-cluster"></a>단일 클러스터에서 서로 다른 VM 크기를 가질 수 있나요?

예, [여러 노드 풀][multi-node-pools]을 만들어 AKS 클러스터에서 여러 가상 머신 크기를 사용할 수 있습니다 .이는 현재 미리 보기 상태입니다.

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>보안 업데이트가 AKS 에이전트 노드에 적용되나요?

Azure는 야간 일정에 따라 클러스터의 Linux 노드에 보안 패치를 자동으로 적용 합니다. 그러나 필요에 따라 이러한 Linux 노드가 다시 부팅 되도록 할 책임이 있습니다. 노드를 다시 부팅 하기 위한 몇 가지 옵션이 있습니다.

- Azure Portal 또는 Azure CLI를 통해 수동으로.
- AKS 클러스터를 업그레이드하여. 클러스터가 cordon [및 드레이닝 노드][cordon-drain] automatically and then bring a new node online with the latest Ubuntu image and a new patch version or a minor Kubernetes version. For more information, see [Upgrade an AKS cluster][aks-upgrade]를 업그레이드 합니다.
- [Kured](https://github.com/weaveworks/kured)를 사용 하 여 Kubernetes에 대 한 오픈 소스 다시 부팅 디먼을 사용 합니다. Kured는 [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) 로 실행 되며 각 노드에서 다시 부팅 해야 함을 나타내는 파일이 있는지 모니터링 합니다. 클러스터 전체에서 OS 다시 부팅은 클러스터 업그레이드와 동일한 [cordon 및 드레이닝 프로세스][cordon-drain] 를 통해 관리 됩니다.

Kured를 사용 하는 방법에 대 한 자세한 내용은 [AKS에서 노드에 보안 및 커널 업데이트 적용][node-updates-kured]을 참조 하세요.

### <a name="windows-server-nodes"></a>Windows Server 노드

Windows Server 노드(현재 AKS에서 프리뷰)에 대한 Windows Update는 최신 업데이트를 자동으로 실행 및 적용하지 않습니다. Windows 업데이트 릴리스 주기 및 사용자 고유의 유효성 검사 프로세스에서는 정기적인 일정에 따라 AKS 클러스터의 Windows Server 노드 풀에서 업그레이드를 수행해야 합니다. 이 업그레이드 프로세스는 최신 Windows Server 이미지 및 패치를 실행하는 노드를 생성하고 이전 노드를 제거합니다. 이 프로세스에 대한 자세한 내용은 [AKS에서 노드 풀 업그레이드][nodepool-upgrade]를 참조하세요.

## <a name="why-are-two-resource-groups-created-with-aks"></a>AKS를 통해 2개의 리소스 그룹이 생성되는 이유는 무엇인가요?

각 AKS 배포는 두 리소스 그룹에 걸쳐 있습니다.

1. 첫 번째 리소스 그룹을 만듭니다. 이 그룹에는 Kubernetes service 리소스만 포함 됩니다. AKS 리소스 공급자는 배포 하는 동안 두 번째 리소스 그룹을 자동으로 만듭니다. 두 번째 리소스 그룹의 예는 *MC_myResourceGroup_myAKSCluster_eastus*입니다. 이 두 번째 리소스 그룹의 이름을 지정 하는 방법에 대 한 자세한 내용은 다음 섹션을 참조 하세요.
1. *노드 리소스 그룹*이라고 하는 두 번째 리소스 그룹에는 클러스터와 연결 된 모든 인프라 리소스가 포함 되어 있습니다. 이러한 리소스에는 Kubernetes 노드 VM, 가상 네트워킹 및 저장소가 포함됩니다. 기본적으로 노드 리소스 그룹의 이름은 *MC_myResourceGroup_myAKSCluster_eastus*와 같습니다. AKS는 클러스터가 삭제 될 때마다 노드 리소스를 자동으로 삭제 하므로 클러스터의 수명 주기를 공유 하는 리소스에 대해서만 사용 해야 합니다.

## <a name="can-i-provide-my-own-name-for-the-aks-node-resource-group"></a>AKS node 리소스 그룹에 대 한 고유한 이름을 제공할 수 있나요?

예. 기본적으로 AKS는 노드 리소스 그룹의 이름을 *MC_clustername_resourcegroupname_location*로 지정 하지만 사용자 고유의 이름을 제공할 수도 있습니다.

고유한 리소스 그룹 이름을 지정 하려면 [aks-preview][aks-preview-cli] Azure CLI 확장 버전 *0.3.2* 이상을 설치 합니다. [Az AKS create][az-aks-create] 명령을 사용 하 여 AKS 클러스터를 만들 때 *--node-group* 매개 변수를 사용 하 고 리소스 그룹의 이름을 지정 합니다. [Azure Resource Manager 템플릿을 사용][aks-rm-template] 하 여 AKS 클러스터를 배포 하는 경우 *Noderesourcegroup* 속성을 사용 하 여 리소스 그룹 이름을 정의할 수 있습니다.

* 사용자의 구독에서 Azure 리소스 공급자가 자동으로 보조 리소스 그룹을 만듭니다.
* 클러스터를 만들 때만 사용자 지정 리소스 그룹 이름을 지정할 수 있습니다.

노드 리소스 그룹으로 작업할 때 다음을 수행할 수 없다는 점에 유의 하세요.

* 노드 리소스 그룹에 대 한 기존 리소스 그룹을 지정 합니다.
* 노드 리소스 그룹에 대해 다른 구독을 지정 하십시오.
* 클러스터를 만든 후 노드 리소스 그룹 이름을 변경 합니다.
* 노드 리소스 그룹 내에서 관리 되는 리소스의 이름을 지정 합니다.
* 노드 리소스 그룹 내에서 관리 되는 리소스의 태그를 수정 하거나 삭제 합니다. 다음 섹션에서 추가 정보를 참조 하세요.

## <a name="can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-node-resource-group"></a>노드 리소스 그룹에서 AKS 리소스의 태그 및 기타 속성을 수정할 수 있나요?

노드 리소스 그룹에서 Azure에서 만든 태그 및 기타 리소스 속성을 수정 하거나 삭제 하는 경우 오류 크기 조정 및 업그레이드와 같은 예기치 않은 결과를 얻을 수 있습니다. AKS를 사용 하면 사용자 지정 태그를 만들고 수정할 수 있습니다. 예를 들어 사업부 또는 비용 센터를 할당 하는 등 사용자 지정 태그를 만들거나 수정할 수 있습니다. AKS 클러스터의 노드 리소스 그룹에서 리소스를 수정 하면 SLO (서비스 수준 목표)가 중단 됩니다. 자세한 내용은 [AKS에서 서비스 수준 계약을 제공 하나요?](#does-aks-offer-a-service-level-agreement) 를 참조 하세요.

## <a name="what-kubernetes-admission-controllers-does-aks-support-can-admission-controllers-be-added-or-removed"></a>AKS가 지원하는 Kubernetes 허용 컨트롤러는 무엇인가요? 허용 컨트롤러를 추가하거나 제거할 수 있나요?

AKS는 다음과 같은 [허용 컨트롤러][admission-controllers]를 지원 합니다.

- *NamespaceLifecycle*
- *LimitRanger*
- *ServiceAccount*
- *DefaultStorageClass*
- *DefaultTolerationSeconds*
- *MutatingAdmissionWebhook*
- *ValidatingAdmissionWebhook*
- *ResourceQuota*
- *DenyEscalatingExec*
- *AlwaysPullImages*

현재 AKS에서 허용 컨트롤러 목록을 수정할 수 없습니다.

## <a name="is-azure-key-vault-integrated-with-aks"></a>Azure Key Vault는 AKS와 통합되나요?

AKS는 현재 기본적으로 Azure Key Vault와 통합 되지 않습니다. 그러나 [Kubernetes 프로젝트의 Azure Key Vault에 대 한 볼륨][keyvault-flexvolume] 을 사용 하면 Kubernetes pod에서 Key Vault 비밀으로 직접 통합할 수 있습니다.

## <a name="can-i-run-windows-server-containers-on-aks"></a>Windows Server 컨테이너를 AKS에서 실행할 수 있습니까?

예, Windows Server 컨테이너는 미리 보기에서 사용할 수 있습니다. AKS에서 Windows Server 컨테이너를 실행 하려면 게스트 OS로 Windows Server를 실행 하는 노드 풀을 만듭니다. Windows Server 컨테이너는 Windows Server 2019만 사용할 수 있습니다. 시작 하려면 [Windows Server 노드 풀을 사용 하 여 AKS 클러스터 만들기][aks-windows-cli]를 참조 하세요.

노드 풀에 대 한 창 서버 지원에는 Kubernetes 프로젝트의 업스트림 Windows Server에 포함 되는 몇 가지 제한 사항이 포함 되어 있습니다. 이러한 제한 사항에 대 한 자세한 내용은 [Windows Server 컨테이너의 AKS 제한 사항][aks-windows-limitations]을 참조 하세요.

## <a name="does-aks-offer-a-service-level-agreement"></a>AKS는 서비스 수준 계약을 제공 하나요?

SLA (서비스 수준 계약)에서 공급자는 게시 된 서비스 수준이 충족 되지 않는 경우 고객에 게 서비스 비용을 상환에 동의 합니다. AKS는 무료 이므로 상환에는 비용을 사용할 수 없으므로 AKS에는 공식적인 SLA가 없습니다. 그러나 AKS는 Kubernetes API 서버에 대해 최소 99.5%의 가용성을 유지 하려고 합니다.

## <a name="why-cant-i-set-maxpods-below-30"></a>MaxPods를 30 미만으로 설정할 수 없는 이유는 무엇입니까?

AKS에서는 Azure CLI 및 Azure Resource Manager 템플릿을 사용 `maxPods` 하 여 클러스터를 만들 때 값을 설정할 수 있습니다. 그러나 Kubenet 및 Azure CNI에는 최소한의 *값* (생성 시 유효성 검사 됨)이 필요 합니다.

| 네트워킹 | 최소 | 최대값 |
| -- | :--: | :--: |
| Azure CNI | 30 | 250 |
| Kubenet | 30 | 110 |

AKS는 관리 되는 서비스 이므로 클러스터의 일부로 추가 기능 및 pod을 배포 하 고 관리 합니다. 이전에는 사용자가 관리 되는 `maxPods` pod가 실행 하는 데 필요한 값 (예: 30) 보다 낮은 값을 정의할 수 있었습니다. 이제 AKS는 ((maxPods 또는 (maxPods * vm_count)) > 관리 되는 추가 기능 pod minimum 수식을 사용 하 여 최소 pod 수를 계산 합니다.

사용자는 최소 `maxPods` 유효성 검사를 재정의할 수 없습니다.

## <a name="can-i-apply-azure-reservation-discounts-to-my-aks-agent-nodes"></a>AKS agent 노드에 Azure 예약 할인을 적용할 수 있나요?

AKS 에이전트 노드는 표준 Azure virtual machines로 청구 되므로 AKS에서 사용 하는 VM 크기에 대 한 [Azure 예약][reservation-discounts] 을 구매한 경우 해당 할인이 자동으로 적용 됩니다.

## <a name="can-i-movemigrate-my-cluster-between-azure-tenants"></a>Azure 테 넌 트 간에 클러스터를 이동/마이그레이션할 수 있나요?

`az aks update-credentials` 명령을 사용 하 여 Azure 테 넌 트 간에 AKS 클러스터를 이동할 수 있습니다. 선택의 지침에 따라 [서비스 주체를 업데이트 하거나 만든](https://docs.microsoft.com/azure/aks/update-credentials) 다음 [새 자격 증명으로 aks cluster를 업데이트](https://docs.microsoft.com/azure/aks/update-credentials#update-aks-cluster-with-new-credentials)합니다.

## <a name="can-i-movemigrate-my-cluster-between-subscriptions"></a>내 클러스터를 구독 간에 이동/마이그레이션할 수 있나요?

현재 구독 간의 클러스터 이동은 지원 되지 않습니다.

<!-- LINKS - internal -->

[aks-regions]: ./quotas-skus-regions.md#region-availability
[aks-upgrade]: ./upgrade-cluster.md
[aks-cluster-autoscale]: ./autoscaler.md
[virtual-kubelet]: virtual-kubelet.md
[aks-advanced-networking]: ./configure-azure-cni.md
[aks-rbac-aad]: ./azure-ad-integration.md
[node-updates-kured]: node-updates-kured.md
[aks-preview-cli]: /cli/azure/ext/aks-preview/aks
[az-aks-create]: /cli/azure/aks#az-aks-create
[aks-rm-template]: /azure/templates/microsoft.containerservice/2019-06-01/managedclusters
[aks-cluster-autoscaler]: cluster-autoscaler.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[aks-windows-cli]: windows-container-cli.md
[aks-windows-limitations]: windows-node-limitations.md
[reservation-discounts]: ../billing/billing-save-compute-costs-reservations.md
[api-server-authorized-ip-ranges]: ./api-server-authorized-ip-ranges.md
[multi-node-pools]: ./use-multiple-node-pools.md

<!-- LINKS - external -->

[auto-scaler]: https://github.com/kubernetes/autoscaler
[cordon-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
[keyvault-flexvolume]: https://github.com/Azure/kubernetes-keyvault-flexvol
[private-clusters-github-issue]: https://github.com/Azure/AKS/issues/948
