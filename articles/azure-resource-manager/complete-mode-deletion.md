---
title: 리소스 종류에 따른 Azure Resource Manager 전체 모드 삭제
description: 리소스 종류가 Azure Resource Manager 템플릿에서 전체 모드 삭제를 처리하는 방법을 보여줍니다.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 04/24/2019
ms.author: tomfitz
ms.openlocfilehash: 5ac442c0ae1e397fd1e8b58fdbcf61eb8712046c
ms.sourcegitcommit: af58483a9c574a10edc546f2737939a93af87b73
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68302853"
---
# <a name="deletion-of-azure-resources-for-complete-mode-deployments"></a>완료 모드 배포를 위한 Azure 리소스의 삭제
이 문서에서는 완료 모드로 배포된 템플릿에 없는 경우 리소스 종류가 삭제를 처리하는 방법을 설명합니다.

`Yes`로 표시된 리소스 종류는 종류가 완료 모드로 배포된 템플릿에 없는 경우 삭제됩니다. 

`No`로 표시된 리소스 종류는 템플릿에 없는 경우 자동으로 삭제되지 않습니다. 단, 부모 리소스가 삭제되는 경우에는 삭제됩니다. 동작에 대한 전체 설명은 [Azure Resource Manager 배포 모드](deployment-modes.md)를 참조하세요.


## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | DomainServices | 예 | 
> | DomainServices/oucontainer | 아니요 | 

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | diagnosticSettings | 아니요 | 
> | diagnosticSettingsCategories | 아니요 | 

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | supportProviders | 아니요 | 

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | aadsupportcases | 아니요 | 
> | addsservices | 아니요 | 
> | agents | 아니요 | 
> | anonymousapiusers | 아니요 | 
> | 구성 | 아니요 | 
> | logs | 아니요 | 
> | reports | 아니요 | 
> | services | 아니요 | 

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | 구성 | 아니요 | 
> | generateRecommendations | 아니요 | 
> | 동영상 추천 기능 | 아니요 | 
> | suppressions | 아니요 | 

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | actionRules | 아니요 | 
> | 경고 | 아니요 | 
> | alertsList | 아니요 | 
> | alertsSummary | 아니요 | 
> | alertsSummaryList | 아니요 | 
> | smartDetectorAlertRules | 아니요 | 
> | smartDetectorRuntimeEnvironments | 아니요 | 
> | smartGroups | 아니요 | 

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | servers | 예 | 

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | reportFeedback | 아니요 | 
> | 서비스 | 예 | 
> | validateServiceName | 아니요 | 

## <a name="microsoftattestation"></a>Microsoft.Attestation

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | attestationProviders | 아니요 | 

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | classicAdministrators | 아니요 | 
> | denyAssignments | 아니요 | 
> | elevateAccess | 아니요 | 
> | locks | 아니요 | 
> | 권한 | 아니요 | 
> | policyAssignments | 아니요 | 
> | policyDefinitions | 아니요 | 
> | policySetDefinitions | 아니요 | 
> | providerOperations | 아니요 | 
> | roleAssignments | 아니요 | 
> | roleDefinitions | 아니요 | 

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | automationAccounts | 예 | 
> | automationAccounts/configurations | 예 | 
> | automationAccounts/jobs | 아니요 | 
> | automationAccounts/runbooks | 예 | 
> | automationAccounts/softwareUpdateConfigurations | 아니요 | 
> | automationAccounts/webhooks | 아니요 | 

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Geneva

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | environments | 아니요 | 
> | environments/accounts | 아니요 | 
> | environments/accounts/namespaces | 아니요 | 
> | environments/accounts/namespaces/configurations | 아니요 | 

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | b2cDirectories | 예 | 

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | registrations | 예 | 
> | registrations/customerSubscriptions | 아니요 | 
> | registrations/products | 아니요 | 

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | batchAccounts | 예 | 

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | billingAccounts | 아니요 | 
> | billingAccounts/billingProfiles | 아니요 | 
> | billingAccounts/billingProfiles/billingSubscriptions | 아니요 | 
> | billingAccounts/billingProfiles/invoices | 아니요 | 
> | billingAccounts/billingProfiles/invoices/pricesheet | 아니요 | 
> | billingAccounts/billingProfiles/operationStatus | 아니요 | 
> | billingAccounts/billingProfiles/paymentMethods | 아니요 | 
> | billingAccounts/billingProfiles/policies | 아니요 | 
> | billingAccounts/billingProfiles/pricesheet | 아니요 | 
> | billingAccounts/billingProfiles/products | 아니요 | 
> | billingAccounts/billingProfiles/transactions | 아니요 | 
> | billingAccounts/billingSubscriptions | 아니요 | 
> | billingAccounts/departments | 아니요 | 
> | billingAccounts/eligibleOffers | 아니요 | 
> | billingAccounts/enrollmentAccounts | 아니요 | 
> | billingAccounts/invoices | 아니요 | 
> | billingAccounts/invoiceSections | 아니요 | 
> | billingAccounts/invoiceSections/billingSubscriptions | 아니요 | 
> | billingAccounts/invoiceSections/billingSubscriptions/transfer | 아니요 | 
> | billingAccounts/invoiceSections/importRequests | 아니요 | 
> | billingAccounts/invoiceSections/initiateImportRequest | 아니요 | 
> | billingAccounts/invoiceSections/initiateTransfer | 아니요 | 
> | billingAccounts/invoiceSections/operationStatus | 아니요 | 
> | billingAccounts/invoiceSections/products | 아니요 | 
> | billingAccounts/invoiceSections/transfers | 아니요 | 
> | billingAccounts/products | 아니요 | 
> | billingAccounts/projects | 아니요 | 
> | billingAccounts/projects/billingSubscriptions | 아니요 | 
> | billingAccounts/projects/importRequests | 아니요 | 
> | billingAccounts/projects/initiateImportRequest | 아니요 | 
> | billingAccounts/projects/operationStatus | 아니요 | 
> | billingAccounts/projects/products | 아니요 | 
> | billingAccounts/transactions | 아니요 | 
> | billingPeriods | 아니요 | 
> | BillingPermissions | 아니요 | 
> | billingProperty | 아니요 | 
> | BillingRoleAssignments | 아니요 | 
> | BillingRoleDefinitions | 아니요 | 
> | CreateBillingRoleAssignment | 아니요 | 
> | departments | 아니요 | 
> | enrollmentAccounts | 아니요 | 
> | importRequests | 아니요 | 
> | importRequests/acceptImportRequest | 아니요 | 
> | importRequests/declineImportRequest | 아니요 | 
> | invoices | 아니요 | 
> | transfers | 아니요 | 
> | transfers/acceptTransfer | 아니요 | 
> | transfers/declineTransfer | 아니요 | 
> | transfers/operationStatus | 아니요 | 
> | usagePlans | 아니요 | 

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | mapApis | 예 | 
> | updateCommunicationPreference | 아니요 | 

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | BizTalk | 예 | 

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | blueprintAssignments | 아니요 | 
> | blueprintAssignments/assignmentOperations | 아니요 | 
> | blueprintAssignments/operations | 아니요 | 
> | blueprints | 아니요 | 
> | blueprints/artifacts | 아니요 | 
> | blueprints/versions | 아니요 | 
> | blueprints/versions/artifacts | 아니요 | 

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | botServices | 예 | 
> | botServices/channels | 아니요 | 
> | botServices/connections | 아니요 | 

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | Redis | 예 | 
> | RedisConfigDefinition | 아니요 | 

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | appliedReservations | 아니요 | 
> | calculatePrice | 아니요 | 
> | catalogs | 아니요 | 
> | commercialReservationOrders | 아니요 | 
> | reservationOrders | 아니요 | 
> | reservationOrders/calculateRefund | 아니요 | 
> | reservationOrders/merge | 아니요 | 
> | reservationOrders/reservations | 아니요 | 
> | reservationOrders/reservations/revisions | 아니요 | 
> | reservationOrders/return | 아니요 | 
> | reservationOrders/split | 아니요 | 
> | reservationOrders/swap | 아니요 | 
> | reservations | 아니요 | 
> | 리소스 | 아니요 | 
> | validateReservationOrder | 아니요 | 

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | edgenodes | 아니요 | 
> | profiles | 예 | 
> | profiles/endpoints | 예 | 
> | profiles/endpoints/customdomains | 아니요 | 
> | profiles/endpoints/origins | 아니요 | 
> | validateProbe | 아니요 | 

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | certificateOrders | 예 | 
> | certificateOrders/certificates | 아니요 | 
> | validateCertificateRegistrationInformation | 아니요 | 

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | capabilities | 아니요 | 
> | domainNames | 아니요 | 
> | domainNames/capabilities | 아니요 | 
> | domainNames/internalLoadBalancers | 아니요 | 
> | domainNames/serviceCertificates | 아니요 | 
> | domainNames/slots | 아니요 | 
> | domainNames/slots/roles | 아니요 | 
> | moveSubscriptionResources | 아니요 | 
> | operatingSystemFamilies | 아니요 | 
> | operatingSystems | 아니요 | 
> | quotas | 아니요 | 
> | resourceTypes | 아니요 | 
> | validateSubscriptionMoveAvailability | 아니요 | 
> | virtualMachines | 아니요 | 
> | virtualMachines/diagnosticSettings | 아니요 | 

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | classicInfrastructureResources | 아니요 | 

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | capabilities | 아니요 | 
> | expressRouteCrossConnections | 아니요 | 
> | expressRouteCrossConnections/peerings | 아니요 | 
> | gatewaySupportedDevices | 아니요 | 
> | networkSecurityGroups | 아니요 | 
> | quotas | 아니요 | 
> | reservedIps | 아니요 | 
> | virtualNetworks | 아니요 | 
> | virtualNetworks/remoteVirtualNetworkPeeringProxies | 아니요 | 
> | virtualNetworks/virtualNetworkPeerings | 아니요 | 

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | capabilities | 아니요 | 
> | disks | 아니요 | 
> | 이미지 | 아니요 | 
> | osImages | 아니요 | 
> | osPlatformImages | 아니요 | 
> | publicImages | 아니요 | 
> | quotas | 아니요 | 
> | storageAccounts | 아니요 | 
> | storageAccounts/services | 아니요 | 
> | storageAccounts/services/diagnosticSettings | 아니요 | 
> | storageAccounts/vmImages | 아니요 | 
> | vmImages | 아니요 | 

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | 계정 | 예 | 

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | RateCard | 아니요 | 
> | UsageAggregates | 아니요 | 

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | availabilitySets | 예 | 
> | disks | 예 | 
> | 이미지 | 예 | 
> | restorePointCollections | 예 | 
> | restorePointCollections/restorePoints | 아니요 | 
> | sharedVMImages | 예 | 
> | sharedVMImages/versions | 예 | 
> | 스냅샷 | 예 | 
> | virtualMachines | 예 | 
> | virtualMachines/diagnosticSettings | 아니요 | 
> | virtualMachines/extensions | 예 | 
> | virtualMachineScaleSets | 예 | 
> | virtualMachineScaleSets/extensions | 아니요 | 
> | virtualMachineScaleSets/networkInterfaces | 아니요 | 
> | virtualMachineScaleSets/publicIPAddresses | 아니요 | 
> | virtualMachineScaleSets/virtualMachines | 아니요 | 
> | virtualMachineScaleSets/virtualMachines/networkInterfaces | 아니요 | 

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | AggregatedCost | 아니요 | 
> | 잔액 | 아니요 | 
> | 예산 | 아니요 | 
> | Charges | 아니요 | 
> | CostTags | 아니요 | 
> | credits | 아니요 | 
> | events | 아니요 | 
> | 예측 | 아니요 | 
> | lots | 아니요 | 
> | Marketplace | 아니요 | 
> | Pricesheets | 아니요 | 
> | products | 아니요 | 
> | ReservationDetails | 아니요 | 
> | ReservationRecommendations | 아니요 | 
> | ReservationSummaries | 아니요 | 
> | ReservationTransactions | 아니요 | 
> | Tags | 아니요 | 
> | 용어 | 아니요 | 
> | UsageDetails | 아니요 | 

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | containerGroups | 예 | 
> | serviceAssociationLinks | 아니요 | 

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | registries | 예 | 
> | registries/builds | 아니요 | 
> | registries/builds/cancel | 아니요 | 
> | registries/builds/getLogLink | 아니요 | 
> | registries/buildTasks | 예 | 
> | registries/buildTasks/steps | 아니요 | 
> | registries/eventGridFilters | 아니요 | 
> | registries/getBuildSourceUploadUrl | 아니요 | 
> | registries/GetCredentials | 아니요 | 
> | registries/importImage | 아니요 | 
> | registries/queueBuild | 아니요 | 
> | registries/regenerateCredential | 아니요 | 
> | registries/regenerateCredentials | 아니요 | 
> | registries/replications | 예 | 
> | registries/runs | 아니요 | 
> | registries/runs/cancel | 아니요 | 
> | registries/scheduleRun | 아니요 | 
> | registries/tasks | 예 | 
> | registries/updatePolicies | 아니요 | 
> | registries/webhooks | 예 | 
> | registries/webhooks/getCallbackConfig | 아니요 | 
> | registries/webhooks/ping | 아니요 | 

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | containerServices | 예 | 
> | managedClusters | 예 | 

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | 애플리케이션 | 예 | 
> | updateCommunicationPreference | 아니요 | 

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | 계정 | 예 | 

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | , | 아니요 | 
> | BillingAccounts | 아니요 | 
> | 커넥터 | 예 | 
> | Departments | 아니요 | 
> | 차원 | 아니요 | 
> | EnrollmentAccounts | 아니요 | 
> | Query | 아니요 | 
> | register | 아니요 | 
> | Reportconfigs | 아니요 | 
> | 보고서 | 아니요 | 

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | hubs | 예 | 
> | hubs/authorizationPolicies | 아니요 | 
> | hubs/connectors | 아니요 | 
> | hubs/connectors/mappings | 아니요 | 
> | hubs/interactions | 아니요 | 
> | hubs/kpi | 아니요 | 
> | hubs/links | 아니요 | 
> | hubs/profiles | 아니요 | 
> | hubs/roleAssignments | 아니요 | 
> | hubs/roles | 아니요 | 
> | hubs/suggestTypeSchema | 아니요 | 
> | hubs/views | 아니요 | 
> | hubs/widgetTypes | 아니요 | 

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | jobs | 예 | 

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | DataBoxEdgeDevices | 예 | 

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | workspaces | 예 | 
> | workspaces/virtualNetworkPeerings | 아니요 | 

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | catalogs | 예 | 

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | connectionManagers | 예 | 

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | dataFactories | 예 | 
> | dataFactories/diagnosticSettings | 아니요 | 
> | dataFactorySchema | 아니요 | 
> | factories | 예 | 
> | factories/integrationRuntimes | 아니요 | 

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | 계정 | 예 | 
> | accounts/dataLakeStoreAccounts | 아니요 | 
> | accounts/storageAccounts | 아니요 | 
> | accounts/storageAccounts/containers | 아니요 | 

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | 계정 | 예 | 
> | accounts/eventGridFilters | 아니요 | 
> | accounts/firewallRules | 아니요 | 

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | services | 예 | 
> | services/projects | 예 | 

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | servers | 예 | 
> | servers/recoverableServers | 아니요 | 
> | servers/virtualNetworkRules | 아니요 | 

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | servers | 예 | 
> | servers/recoverableServers | 아니요 | 
> | servers/virtualNetworkRules | 아니요 | 

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | servers | 예 | 
> | servers/advisors | 아니요 | 
> | servers/queryTexts | 아니요 | 
> | servers/recoverableServers | 아니요 | 
> | servers/topQueryStatistics | 아니요 | 
> | servers/virtualNetworkRules | 아니요 | 
> | servers/waitStatistics | 아니요 | 

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | IotHubs | 예 | 
> | IotHubs/eventGridFilters | 아니요 | 
> | ProvisioningServices | 예 | 
> | usages | 아니요 | 

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | controllers | 예 | 

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | labs | 예 | 
> | labs/serviceRunners | 예 | 
> | labs/virtualMachines | 예 | 
> | schedules | 예 | 

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | databaseAccountNames | 아니요 | 
> | databaseAccounts | 예 | 

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | domains | 예 | 
> | domains/domainOwnershipIdentifiers | 아니요 | 
> | generateSsoRequest | 아니요 | 
> | topLevelDomains | 아니요 | 
> | validateDomainRegistrationInformation | 아니요 | 

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | lcsprojects | 아니요 | 
> | lcsprojects/clouddeployments | 아니요 | 
> | lcsprojects/connectors | 아니요 | 

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | domains | 예 | 
> | domains/topics | 아니요 | 
> | eventSubscriptions | 아니요 | 
> | extensionTopics | 아니요 | 
> | topics | 예 | 
> | topicTypes | 아니요 | 

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | clusters | 예 | 
> | namespaces | 예 | 
> | namespaces/authorizationrules | 아니요 | 
> | namespaces/disasterrecoveryconfigs | 아니요 | 
> | namespaces/eventhubs | 아니요 | 
> | namespaces/eventhubs/authorizationrules | 아니요 | 
> | namespaces/eventhubs/consumergroups | 아니요 | 

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | 기능 | 아니요 | 
> | providers | 아니요 | 

## <a name="microsoftgallery"></a>Microsoft.Gallery

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | 등록(enroll) | 아니요 | 
> | galleryitems | 아니요 | 
> | generateartifactaccessuri | 아니요 | 
> | myareas | 아니요 | 
> | myareas/areas | 아니요 | 
> | myareas/areas/areas | 아니요 | 
> | myareas/areas/areas/galleryitems | 아니요 | 
> | myareas/areas/galleryitems | 아니요 | 
> | myareas/galleryitems | 아니요 | 
> | register | 아니요 | 
> | 리소스 | 아니요 | 
> | retrieveresourcesbyid | 아니요 | 

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | guestConfigurationAssignments | 아니요 | 
> | software | 아니요 | 

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | hanaInstances | 예 | 

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | clusters | 예 | 
> | clusters/applications | 아니요 | 

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | jobs | 예 | 

## <a name="microsoftinformationprotection"></a>Microsoft.InformationProtection

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | labelGroups | 아니요 | 
> | labelGroups/labels | 아니요 | 
> | labelGroups/labels/conditions | 아니요 | 
> | labelGroups/labels/subLabels | 아니요 | 
> | labelGroups/labels/subLabels/conditions | 아니요 | 

## <a name="microsoftinsights"></a>microsoft.insights

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | actiongroups | 예 | 
> | activityLogAlerts | 예 | 
> | alertrules | 예 | 
> | automatedExportSettings | 아니요 | 
> | autoscalesettings | 예 | 
> | baseline | 아니요 | 
> | calculatebaseline | 아니요 | 
> | components | 예 | 
> | components/events | 아니요 | 
> | components/pricingPlans | 아니요 | 
> | components/query | 아니요 | 
> | diagnosticSettings | 아니요 | 
> | diagnosticSettingsCategories | 아니요 | 
> | eventCategories | 아니요 | 
> | eventtypes | 아니요 | 
> | extendedDiagnosticSettings | 아니요 | 
> | logDefinitions | 아니요 | 
> | logprofiles | 아니요 | 
> | logs | 아니요 | 
> | metricAlerts | 예 |
> | migrateToNewPricingModel | 아니요 | 
> | myWorkbooks | 아니요 | 
> | 쿼리 | 아니요 | 
> | rollbackToLegacyPricingModel | 아니요 | 
> | scheduledqueryrules | 예 | 
> | vmInsightsOnboardingStatuses | 아니요 | 
> | webtests | 예 | 
> | workbooks | 예 | 

## <a name="microsoftintune"></a>Microsoft.Intune

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | diagnosticSettings | 아니요 | 
> | diagnosticSettingsCategories | 아니요 | 

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | IoTApps | 예 | 

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | 그래프 | 예 | 

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | deletedVaults | 아니요 | 
> | vaults | 예 | 
> | vaults/accessPolicies | 아니요 | 
> | vaults/secrets | 아니요 | 

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | clusters | 예 | 
> | clusters/databases | 아니요 | 
> | clusters/databases/dataconnections | 아니요 | 
> | clusters/databases/eventhubconnections | 아니요 | 

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | labaccounts | 예 | 
> | users | 아니요 | 

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | 계정 | 예 | 

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | 계정 | 예 | 

## <a name="microsoftloganalytics"></a>Microsoft.LogAnalytics

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | logs | 아니요 | 

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | integrationAccounts | 예 | 
> | workflows | 예 | 

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | commitmentPlans | 예 | 
> | webServices | 예 | 
> | 작업 영역 | 예 | 

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | 계정 | 예 | 
> | accounts/workspaces | 예 | 
> | accounts/workspaces/projects | 예 | 
> | teamAccounts | 예 | 
> | teamAccounts/workspaces | 예 | 
> | teamAccounts/workspaces/projects | 예 | 

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | 계정 | 예 | 

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | workspaces | 예 | 
> | workspaces/computes | 아니요 | 

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | Identities | 아니요 | 
> | userAssignedIdentities | 예 | 

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | getEntities | 아니요 | 
> | managementGroups | 아니요 | 
> | 리소스 | 아니요 | 
> | startTenantBackfill | 아니요 | 
> | tenantBackfillStatus | 아니요 | 

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | 계정 | 예 | 
> | accounts/eventGridFilters | 아니요 | 

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | offers | 아니요 | 
> | offerTypes | 아니요 | 
> | offerTypes/publishers | 아니요 | 
> | offerTypes/publishers/offers | 아니요 | 
> | offerTypes/publishers/offers/plans | 아니요 | 
> | offerTypes/publishers/offers/plans/agreements | 아니요 | 
> | offerTypes/publishers/offers/plans/configs | 아니요 | 
> | offerTypes/publishers/offers/plans/configs/importImage | 아니요 | 
> | privategalleryitems | 아니요 | 
> | products | 아니요 | 

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | classicDevServices | 예 | 
> | updateCommunicationPreference | 아니요 | 

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | agreements | 아니요 | 
> | offertypes | 아니요 | 

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | mediaservices | 예 | 
> | mediaservices/accountFilters | 아니요 | 
> | mediaservices/assets | 아니요 | 
> | mediaservices/assets/assetFilters | 아니요 | 
> | mediaservices/contentKeyPolicies | 아니요 | 
> | mediaservices/eventGridFilters | 아니요 | 
> | mediaservices/liveEventOperations | 아니요 | 
> | mediaservices/liveEvents | 예 | 
> | mediaservices/liveEvents/liveOutputs | 아니요 | 
> | mediaservices/liveOutputOperations | 아니요 | 
> | mediaservices/streamingEndpointOperations | 아니요 | 
> | mediaservices/streamingEndpoints | 예 | 
> | mediaservices/streamingLocators | 아니요 | 
> | mediaservices/streamingPolicies | 아니요 | 
> | mediaservices/transforms | 아니요 | 
> | mediaservices/transforms/jobs | 아니요 | 

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | projects | 예 | 

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | applicationGateways | 예 | 
> | applicationSecurityGroups | 예 | 
> | azureFirewallFqdnTags | 아니요 | 
> | azureFirewalls | 예 | 
> | bgpServiceCommunities | 아니요 | 
> | connections | 예 | 
> | ddosCustomPolicies | 예 | 
> | ddosProtectionPlans | 예 | 
> | dnsOperationStatuses | 아니요 | 
> | dnszones | 예 | 
> | dnszones/A | 아니요 | 
> | dnszones/AAAA | 아니요 | 
> | dnszones/all | 아니요 | 
> | dnszones/CAA | 아니요 | 
> | dnszones/CNAME | 아니요 | 
> | dnszones/MX | 아니요 | 
> | dnszones/NS | 아니요 | 
> | dnszones/PTR | 아니요 | 
> | dnszones/recordsets | 아니요 | 
> | dnszones/SOA | 아니요 | 
> | dnszones/SRV | 아니요 | 
> | dnszones/TXT | 아니요 | 
> | expressRouteCircuits | 예 | 
> | expressRouteServiceProviders | 아니요 | 
> | frontdoors | 예 | 
> | frontdoorWebApplicationFirewallPolicies | 예 | 
> | getDnsResourceReference | 아니요 | 
> | interfaceEndpoints | 예 | 
> | internalNotify | 아니요 | 
> | loadBalancers | 예 | 
> | localNetworkGateways | 예 | 
> | natGateways | 예 | 
> | networkIntentPolicies | 예 | 
> | networkInterfaces | 예 | 
> | networkProfiles | 예 | 
> | networkSecurityGroups | 예 | 
> | networkWatchers | 예 | 
> | networkWatchers/connectionMonitors | 예 | 
> | networkWatchers/lenses | 예 | 
> | networkWatchers/pingMeshes | 예 | 
> | privateLinkServices | 예 | 
> | publicIPAddresses | 예 | 
> | publicIPPrefixes | 예 | 
> | routeFilters | 예 | 
> | routeTables | 예 | 
> | serviceEndpointPolicies | 예 | 
> | trafficManagerGeographicHierarchies | 아니요 | 
> | trafficmanagerprofiles | 예 | 
> | trafficmanagerprofiles/heatMaps | 아니요 | 
> | virtualHubs | 예 | 
> | virtualNetworkGateways | 예 | 
> | virtualNetworks | 예 | 
> | virtualNetworkTaps | 예 | 
> | virtualWans | 예 | 
> | vpnGateways | 예 | 
> | vpnSites | 예 | 
> | webApplicationFirewallPolicies | 예 | 

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | namespaces | 예 | 
> | namespaces/notificationHubs | 예 | 

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | devices | 아니요 | 
> | linkTargets | 아니요 | 
> | storageInsightConfigs | 아니요 | 
> | workspaces | 예 | 
> | workspaces/dataSources | 아니요 | 
> | workspaces/linkedServices | 아니요 | 
> | workspaces/query | 아니요 | 

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | managementassociations | 아니요 | 
> | managementconfigurations | 예 | 
> | solutions | 예 | 
> | 뷰 | 예 | 

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | policyEvents | 아니요 | 
> | policyStates | 아니요 | 
> | policyTrackedResources | 아니요 | 
> | remediations | 아니요 | 

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | consoles | 아니요 | 
> | dashboards | 예 | 
> | userSettings | 아니요 | 

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | workspaceCollections | 예 | 

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | capacities | 예 | 

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | 계정 | 예 | 

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | backupProtectedItems | 아니요 | 
> | vaults | 예 | 

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | namespaces | 예 | 
> | namespaces/authorizationrules | 아니요 | 
> | namespaces/hybridconnections | 아니요 | 
> | namespaces/hybridconnections/authorizationrules | 아니요 | 
> | namespaces/wcfrelays | 아니요 | 
> | namespaces/wcfrelays/authorizationrules | 아니요 | 

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | 리소스 | 아니요 | 
> | subscriptionsStatus | 아니요 | 

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | availabilityStatuses | 아니요 | 
> | childAvailabilityStatuses | 아니요 | 
> | childResources | 아니요 | 
> | events | 아니요 | 
> | impactedResources | 아니요 | 
> | 알림 | 아니요 | 

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | 배포 | 아니요 | 
> | 배포/작업 | 아니요 | 
> | links | 아니요 | 
> | notifyResourceJobs | 아니요 | 
> | providers | 아니요 | 
> | resourceGroups | 아니요 | 
> | 리소스 | 아니요 | 
> | 구독 | 아니요 | 
> | subscriptions/providers | 아니요 | 
> | subscriptions/resourceGroups | 아니요 | 
> | subscriptions/resourcegroups/resources | 아니요 | 
> | subscriptions/resources | 아니요 | 
> | subscriptions/tagnames | 아니요 | 
> | subscriptions/tagNames/tagValues | 아니요 | 
> | tenants | 아니요 | 

## <a name="microsoftsaas"></a>Microsoft.SaaS

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | 애플리케이션 | 예 | 
> | saasresources | 아니요 | 

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | flows | 예 | 
> | jobcollections | 예 | 

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | resourceHealthMetadata | 아니요 | 
> | searchServices | 예 | 

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | advancedThreatProtectionSettings | 아니요 | 
> | 경고 | 아니요 | 
> | allowedConnections | 아니요 | 
> | appliances | 아니요 | 
> | applicationWhitelistings | 아니요 | 
> | AutoProvisioningSettings | 아니요 | 
> | Compliances | 아니요 | 
> | dataCollectionAgents | 아니요 | 
> | discoveredSecuritySolutions | 아니요 | 
> | externalSecuritySolutions | 아니요 | 
> | InformationProtectionPolicies | 아니요 | 
> | jitNetworkAccessPolicies | 아니요 | 
> | monitoring | 아니요 | 
> | monitoring/antimalware | 아니요 | 
> | monitoring/baseline | 아니요 | 
> | monitoring/patch | 아니요 | 
> | 정책 | 아니요 | 
> | pricings | 아니요 | 
> | securityContacts | 아니요 | 
> | securitySolutions | 아니요 | 
> | securitySolutionsReferenceData | 아니요 | 
> | securityStatus | 아니요 | 
> | securityStatus/endpoints | 아니요 | 
> | securityStatus/subnets | 아니요 | 
> | securityStatus/virtualMachines | 아니요 | 
> | securityStatuses | 아니요 | 
> | securityStatusesSummaries | 아니요 | 
> | 설정 | 아니요 | 
> | 태스크 | 아니요 | 
> | topologies | 아니요 | 
> | workspaceSettings | 아니요 | 

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | diagnosticSettings | 아니요 | 
> | diagnosticSettingsCategories | 아니요 | 

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | namespaces | 예 | 
> | namespaces/authorizationrules | 아니요 | 
> | namespaces/disasterrecoveryconfigs | 아니요 | 
> | namespaces/eventgridfilters | 아니요 | 
> | namespaces/queues | 아니요 | 
> | namespaces/queues/authorizationrules | 아니요 | 
> | namespaces/topics | 아니요 | 
> | namespaces/topics/authorizationrules | 아니요 | 
> | namespaces/topics/subscriptions | 아니요 | 
> | namespaces/topics/subscriptions/rules | 아니요 | 
> | premiumMessagingRegions | 아니요 | 

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | clusters | 예 | 
> | clusters/applications | 아니요 | 

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | 애플리케이션 | 예 | 
> | gateways | 예 | 
> | networks | 예 | 
> | secrets | 예 | 
> | volumes | 예 | 

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | SignalR | 예 | 

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | applianceDefinitions | 예 | 
> | appliances | 예 | 
> | applicationDefinitions | 예 | 
> | 애플리케이션 | 예 | 
> | jitRequests | 예 | 

## <a name="microsoftsql"></a>Microsoft.SQL

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | managedInstances | 예 |
> | managedInstances/databases | 예 |
> | managedInstances/databases/backupShortTermRetentionPolicies | 아니요 |
> | managedInstances/databases/schemas/tables/columns/sensitivityLabels | 아니요 |
> | managedInstances/databases/vulnerabilityAssessments | 아니요 |
> | managedInstances/databases/vulnerabilityAssessments/rules/baselines | 아니요 |
> | managedInstances/encryptionProtector | 아니요 |
> | managedInstances/keys | 아니요 |
> | managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies | 아니요 |
> | managedInstances/vulnerabilityAssessments | 아니요 |
> | servers | 예 | 
> | servers/administrators | 아니요 | 
> | servers/communicationLinks | 아니요 | 
> | servers/databases | 예 | 
> | servers/encryptionProtector | 아니요 | 
> | servers/firewallRules | 아니요 | 
> | servers/keys | 아니요 | 
> | servers/restorableDroppedDatabases | 아니요 | 
> | servers/serviceobjectives | 아니요 | 
> | servers/tdeCertificates | 아니요 | 


## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | SqlVirtualMachineGroups | 예 | 
> | SqlVirtualMachineGroups/AvailabilityGroupListeners | 아니요 | 
> | SqlVirtualMachines | 예 | 

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | storageAccounts | 예 | 
> | storageAccounts/blobServices | 아니요 | 
> | storageAccounts/fileServices | 아니요 | 
> | storageAccounts/queueServices | 아니요 | 
> | storageAccounts/services | 아니요 | 
> | storageAccounts/tableServices | 아니요 | 
> | usages | 아니요 | 

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | storageSyncServices | 예 | 
> | storageSyncServices/registeredServers | 아니요 | 
> | storageSyncServices/syncGroups | 아니요 | 
> | storageSyncServices/syncGroups/cloudEndpoints | 아니요 | 
> | storageSyncServices/syncGroups/serverEndpoints | 아니요 | 
> | storageSyncServices/workflows | 아니요 | 

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | managers | 예 | 

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | streamingjobs | 예 | 
> | streamingjobs/diagnosticSettings | 아니요 | 

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | CreateSubscription | 아니요 | 
> | SubscriptionDefinitions | 아니요 | 
> | SubscriptionOperations | 아니요 | 

## <a name="microsoftsupport"></a>microsoft.support

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | supporttickets | 아니요 | 

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | providerRegistrations | 예 | 
> | 리소스 | 예 | 

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | environments | 예 | 
> | environments/accessPolicies | 아니요 | 
> | environments/eventsources | 예 | 
> | environments/referenceDataSets | 예 | 

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | 계정 | 예 | 
> | account/extension | 예 | 
> | account/project | 예 | 

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | apiManagementAccounts | 아니요 | 
> | apiManagementAccounts/apiAcls | 아니요 | 
> | apiManagementAccounts/apis | 아니요 | 
> | apiManagementAccounts/apis/apiAcls | 아니요 | 
> | apiManagementAccounts/apis/connectionAcls | 아니요 | 
> | apiManagementAccounts/apis/connections | 아니요 | 
> | apiManagementAccounts/apis/connections/connectionAcls | 아니요 | 
> | apiManagementAccounts/apis/localizedDefinitions | 아니요 | 
> | apiManagementAccounts/connectionAcls | 아니요 | 
> | apiManagementAccounts/connections | 아니요 | 
> | billingMeters | 아니요 | 
> | certificates | 예 | 
> | connectionGateways | 예 | 
> | connections | 예 | 
> | customApis | 예 | 
> | deletedSites | 아니요 | 
> | functions | 아니요 | 
> | hostingEnvironments | 예 | 
> | hostingEnvironments/multiRolePools | 아니요 | 
> | hostingEnvironments/multiRolePools/instances | 아니요 | 
> | hostingEnvironments/workerPools | 아니요 | 
> | hostingEnvironments/workerPools/instances | 아니요 | 
> | publishingUsers | 아니요 | 
> | 동영상 추천 기능 | 아니요 | 
> | resourceHealthMetadata | 아니요 | 
> | runtimes | 아니요 | 
> | serverFarms | 예 | 
> | serverFarms/workers | 아니요 | 
> | sites | 예 | 
> | sites/domainOwnershipIdentifiers | 아니요 | 
> | sites/hostNameBindings | 아니요 | 
> | sites/instances | 아니요 | 
> | sites/instances/extensions | 아니요 | 
> | sites/premieraddons | 예 | 
> | sites/recommendations | 아니요 | 
> | sites/resourceHealthMetadata | 아니요 | 
> | sites/slots | 예 | 
> | sites/slots/hostNameBindings | 아니요 | 
> | sites/slots/instances | 아니요 | 
> | sites/slots/instances/extensions | 아니요 | 
> | sourceControls | 아니요 | 
> | validate | 아니요 | 
> | verifyHostingEnvironmentVnet | 아니요 | 

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | diagnosticSettings | 아니요 | 
> | diagnosticSettingsCategories | 아니요 | 

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | DeviceServices | 예 | 

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tableFixed"]
> | 리소스 종류 | 전체 모드 삭제 |
> | ------------- | ----------- |
> | components | 아니요 | 
> | componentsSummary | 아니요 | 
> | monitorInstances | 아니요 | 
> | monitorInstancesSummary | 아니요 | 
> | monitors | 아니요 | 
> | notificationSettings | 아니요 | 

## <a name="next-steps"></a>다음 단계

쉼표로 구분된 값의 파일과 동일한 데이터를 가져오려면 [complete-mode-deletion.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/complete-mode-deletion.csv)를 다운로드합니다.