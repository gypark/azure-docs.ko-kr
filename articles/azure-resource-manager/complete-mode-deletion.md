---
title: 리소스 종류에 따른 Azure Resource Manager 전체 모드 삭제
description: 리소스 종류가 Azure Resource Manager 템플릿에서 전체 모드 삭제를 처리하는 방법을 보여줍니다.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 04/24/2019
ms.author: tomfitz
ms.openlocfilehash: 21b3972a96c1601b15c403275474d58873753b08
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64713000"
---
# <a name="deletion-of-azure-resources-for-complete-mode-deployments"></a>완료 모드 배포를 위한 Azure 리소스의 삭제
이 문서에서는 완료 모드로 배포된 템플릿에 없는 경우 리소스 종류가 삭제를 처리하는 방법을 설명합니다.

`Yes`로 표시된 리소스 종류는 종류가 완료 모드로 배포된 템플릿에 없는 경우 삭제됩니다. 

`No`로 표시된 리소스 종류는 템플릿에 없는 경우 자동으로 삭제되지 않습니다. 단, 부모 리소스가 삭제되는 경우에는 삭제됩니다. 동작에 대한 전체 설명은 [Azure Resource Manager 배포 모드](deployment-modes.md)를 참조하세요.

쉼표로 구분된 값의 파일과 동일한 데이터를 가져오려면 [complete-mode-deletion.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/complete-mode-deletion.csv)를 다운로드합니다.

## <a name="microsoftaad"></a>Microsoft.AAD
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| DomainServices | 예 | 
| DomainServices/oucontainer | 아닙니다. | 

## <a name="microsoftaadiam"></a>microsoft.aadiam
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| diagnosticSettings | 아닙니다. | 
| diagnosticSettingsCategories | 아닙니다. | 

## <a name="microsoftaddons"></a>Microsoft.Addons
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| supportProviders | 아닙니다. | 

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| aadsupportcases | 아닙니다. | 
| addsservices | 아닙니다. | 
| agents | 아닙니다. | 
| anonymousapiusers | 아닙니다. | 
| 구성 | 아닙니다. | 
| 로그 | 아닙니다. | 
| reports | 아닙니다. | 
| services | 아닙니다. | 

## <a name="microsoftadvisor"></a>Microsoft.Advisor
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| 구성 | 아닙니다. | 
| generateRecommendations | 아닙니다. | 
| 동영상 추천 기능 | 아닙니다. | 
| suppressions | 아닙니다. | 

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| actionRules | 아닙니다. | 
| 경고 | 아닙니다. | 
| alertsList | 아닙니다. | 
| alertsSummary | 아닙니다. | 
| alertsSummaryList | 아닙니다. | 
| smartDetectorAlertRules | 아닙니다. | 
| smartDetectorRuntimeEnvironments | 아닙니다. | 
| smartGroups | 아닙니다. | 

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| servers | 예 | 

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| reportFeedback | 아닙니다. | 
| 서비스 | 예 | 
| validateServiceName | 아닙니다. | 

## <a name="microsoftattestation"></a>Microsoft.Attestation
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| attestationProviders | 아닙니다. | 

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| classicAdministrators | 아닙니다. | 
| denyAssignments | 아닙니다. | 
| elevateAccess | 아닙니다. | 
| locks | 아닙니다. | 
| 권한 | 아닙니다. | 
| policyAssignments | 아닙니다. | 
| policyDefinitions | 아닙니다. | 
| policySetDefinitions | 아닙니다. | 
| providerOperations | 아닙니다. | 
| roleAssignments | 아닙니다. | 
| roleDefinitions | 아닙니다. | 

## <a name="microsoftautomation"></a>Microsoft.Automation
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| automationAccounts | 예 | 
| automationAccounts/configurations | 예 | 
| automationAccounts/jobs | 아닙니다. | 
| automationAccounts/runbooks | 예 | 
| automationAccounts/softwareUpdateConfigurations | 아닙니다. | 
| automationAccounts/webhooks | 아닙니다. | 

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Geneva
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| environments | 아닙니다. | 
| environments/accounts | 아닙니다. | 
| environments/accounts/namespaces | 아닙니다. | 
| environments/accounts/namespaces/configurations | 아닙니다. | 

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| b2cDirectories | 예 | 

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| registrations | 예 | 
| registrations/customerSubscriptions | 아닙니다. | 
| registrations/products | 아닙니다. | 

## <a name="microsoftbatch"></a>Microsoft.Batch
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| batchAccounts | 예 | 

## <a name="microsoftbilling"></a>Microsoft.Billing
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| billingAccounts | 아닙니다. | 
| billingAccounts/billingProfiles | 아닙니다. | 
| billingAccounts/billingProfiles/billingSubscriptions | 아닙니다. | 
| billingAccounts/billingProfiles/invoices | 아닙니다. | 
| billingAccounts/billingProfiles/invoices/pricesheet | 아닙니다. | 
| billingAccounts/billingProfiles/operationStatus | 아닙니다. | 
| billingAccounts/billingProfiles/paymentMethods | 아닙니다. | 
| billingAccounts/billingProfiles/policies | 아닙니다. | 
| billingAccounts/billingProfiles/pricesheet | 아닙니다. | 
| billingAccounts/billingProfiles/products | 아닙니다. | 
| billingAccounts/billingProfiles/transactions | 아닙니다. | 
| billingAccounts/billingSubscriptions | 아닙니다. | 
| billingAccounts/departments | 아닙니다. | 
| billingAccounts/eligibleOffers | 아닙니다. | 
| billingAccounts/enrollmentAccounts | 아닙니다. | 
| billingAccounts/invoices | 아닙니다. | 
| billingAccounts/invoiceSections | 아닙니다. | 
| billingAccounts/invoiceSections/billingSubscriptions | 아닙니다. | 
| billingAccounts/invoiceSections/billingSubscriptions/transfer | 아닙니다. | 
| billingAccounts/invoiceSections/importRequests | 아닙니다. | 
| billingAccounts/invoiceSections/initiateImportRequest | 아닙니다. | 
| billingAccounts/invoiceSections/initiateTransfer | 아닙니다. | 
| billingAccounts/invoiceSections/operationStatus | 아닙니다. | 
| billingAccounts/invoiceSections/products | 아닙니다. | 
| billingAccounts/invoiceSections/transfers | 아닙니다. | 
| billingAccounts/products | 아닙니다. | 
| billingAccounts/projects | 아닙니다. | 
| billingAccounts/projects/billingSubscriptions | 아닙니다. | 
| billingAccounts/projects/importRequests | 아닙니다. | 
| billingAccounts/projects/initiateImportRequest | 아닙니다. | 
| billingAccounts/projects/operationStatus | 아닙니다. | 
| billingAccounts/projects/products | 아닙니다. | 
| billingAccounts/transactions | 아닙니다. | 
| billingPeriods | 아닙니다. | 
| BillingPermissions | 아닙니다. | 
| billingProperty | 아닙니다. | 
| BillingRoleAssignments | 아닙니다. | 
| BillingRoleDefinitions | 아닙니다. | 
| CreateBillingRoleAssignment | 아닙니다. | 
| departments | 아닙니다. | 
| enrollmentAccounts | 아닙니다. | 
| importRequests | 아닙니다. | 
| importRequests/acceptImportRequest | 아닙니다. | 
| importRequests/declineImportRequest | 아닙니다. | 
| invoices | 아닙니다. | 
| transfers | 아닙니다. | 
| transfers/acceptTransfer | 아닙니다. | 
| transfers/declineTransfer | 아닙니다. | 
| transfers/operationStatus | 아닙니다. | 
| usagePlans | 아닙니다. | 

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| mapApis | 예 | 
| updateCommunicationPreference | 아닙니다. | 

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| BizTalk | 예 | 

## <a name="microsoftblueprint"></a>Microsoft.Blueprint
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| blueprintAssignments | 아닙니다. | 
| blueprintAssignments/assignmentOperations | 아닙니다. | 
| blueprintAssignments/operations | 아닙니다. | 
| blueprints | 아닙니다. | 
| blueprints/artifacts | 아닙니다. | 
| blueprints/versions | 아닙니다. | 
| blueprints/versions/artifacts | 아닙니다. | 

## <a name="microsoftbotservice"></a>Microsoft.BotService
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| botServices | 예 | 
| botServices/channels | 아닙니다. | 
| botServices/connections | 아닙니다. | 

## <a name="microsoftcache"></a>Microsoft.Cache
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| Redis | 예 | 
| RedisConfigDefinition | 아닙니다. | 

## <a name="microsoftcapacity"></a>Microsoft.Capacity
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| appliedReservations | 아닙니다. | 
| calculatePrice | 아닙니다. | 
| catalogs | 아닙니다. | 
| commercialReservationOrders | 아닙니다. | 
| reservationOrders | 아닙니다. | 
| reservationOrders/calculateRefund | 아닙니다. | 
| reservationOrders/merge | 아닙니다. | 
| reservationOrders/reservations | 아닙니다. | 
| reservationOrders/reservations/revisions | 아닙니다. | 
| reservationOrders/return | 아닙니다. | 
| reservationOrders/split | 아닙니다. | 
| reservationOrders/swap | 아닙니다. | 
| reservations | 아닙니다. | 
| 리소스 | 아닙니다. | 
| validateReservationOrder | 아닙니다. | 

## <a name="microsoftcdn"></a>Microsoft.Cdn
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| edgenodes | 아닙니다. | 
| 프로필 | 예 | 
| profiles/endpoints | 예 | 
| profiles/endpoints/customdomains | 아닙니다. | 
| profiles/endpoints/origins | 아닙니다. | 
| validateProbe | 아닙니다. | 

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| certificateOrders | 예 | 
| certificateOrders/certificates | 아닙니다. | 
| validateCertificateRegistrationInformation | 아닙니다. | 

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| capabilities | 아닙니다. | 
| domainNames | 아닙니다. | 
| domainNames/capabilities | 아닙니다. | 
| domainNames/internalLoadBalancers | 아닙니다. | 
| domainNames/serviceCertificates | 아닙니다. | 
| domainNames/slots | 아닙니다. | 
| domainNames/slots/roles | 아닙니다. | 
| moveSubscriptionResources | 아닙니다. | 
| operatingSystemFamilies | 아닙니다. | 
| operatingSystems | 아닙니다. | 
| quotas | 아닙니다. | 
| resourceTypes | 아닙니다. | 
| validateSubscriptionMoveAvailability | 아닙니다. | 
| virtualMachines | 아닙니다. | 
| virtualMachines/diagnosticSettings | 아닙니다. | 

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| classicInfrastructureResources | 아닙니다. | 

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| capabilities | 아닙니다. | 
| expressRouteCrossConnections | 아닙니다. | 
| expressRouteCrossConnections/peerings | 아닙니다. | 
| gatewaySupportedDevices | 아닙니다. | 
| networkSecurityGroups | 아닙니다. | 
| quotas | 아닙니다. | 
| reservedIps | 아닙니다. | 
| virtualNetworks | 아닙니다. | 
| virtualNetworks/remoteVirtualNetworkPeeringProxies | 아닙니다. | 
| virtualNetworks/virtualNetworkPeerings | 아닙니다. | 

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| capabilities | 아닙니다. | 
| disks | 아닙니다. | 
| images | 아닙니다. | 
| osImages | 아닙니다. | 
| osPlatformImages | 아닙니다. | 
| publicImages | 아닙니다. | 
| quotas | 아닙니다. | 
| storageAccounts | 아닙니다. | 
| storageAccounts/services | 아닙니다. | 
| storageAccounts/services/diagnosticSettings | 아닙니다. | 
| storageAccounts/vmImages | 아닙니다. | 
| vmImages | 아닙니다. | 

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| 계정 | 예 | 

## <a name="microsoftcommerce"></a>Microsoft.Commerce
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| RateCard | 아닙니다. | 
| UsageAggregates | 아닙니다. | 

## <a name="microsoftcompute"></a>Microsoft.Compute
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| availabilitySets | 예 | 
| disks | 예 | 
| images | 예 | 
| restorePointCollections | 예 | 
| restorePointCollections/restorePoints | 아닙니다. | 
| sharedVMImages | 예 | 
| sharedVMImages/versions | 예 | 
| 스냅샷 | 예 | 
| virtualMachines | 예 | 
| virtualMachines/diagnosticSettings | 아닙니다. | 
| virtualMachines/extensions | 예 | 
| virtualMachineScaleSets | 예 | 
| virtualMachineScaleSets/extensions | 아닙니다. | 
| virtualMachineScaleSets/networkInterfaces | 아닙니다. | 
| virtualMachineScaleSets/publicIPAddresses | 아닙니다. | 
| virtualMachineScaleSets/virtualMachines | 아닙니다. | 
| virtualMachineScaleSets/virtualMachines/networkInterfaces | 아닙니다. | 

## <a name="microsoftconsumption"></a>Microsoft.Consumption
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| AggregatedCost | 아닙니다. | 
| 잔액 | 아닙니다. | 
| 예산 | 아닙니다. | 
| Charges | 아닙니다. | 
| CostTags | 아닙니다. | 
| credits | 아닙니다. | 
| events | 아닙니다. | 
| 예측 | 아닙니다. | 
| lots | 아닙니다. | 
| Marketplace | 아닙니다. | 
| Pricesheets | 아닙니다. | 
| products | 아닙니다. | 
| ReservationDetails | 아닙니다. | 
| ReservationRecommendations | 아닙니다. | 
| ReservationSummaries | 아닙니다. | 
| ReservationTransactions | 아닙니다. | 
| 태그들 | 아닙니다. | 
| 용어 | 아닙니다. | 
| UsageDetails | 아닙니다. | 

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| containerGroups | 예 | 
| serviceAssociationLinks | 아닙니다. | 

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| registries | 예 | 
| registries/builds | 아닙니다. | 
| registries/builds/cancel | 아닙니다. | 
| registries/builds/getLogLink | 아닙니다. | 
| registries/buildTasks | 예 | 
| registries/buildTasks/steps | 아닙니다. | 
| registries/eventGridFilters | 아닙니다. | 
| registries/getBuildSourceUploadUrl | 아닙니다. | 
| registries/GetCredentials | 아닙니다. | 
| registries/importImage | 아닙니다. | 
| registries/queueBuild | 아닙니다. | 
| registries/regenerateCredential | 아닙니다. | 
| registries/regenerateCredentials | 아닙니다. | 
| registries/replications | 예 | 
| registries/runs | 아닙니다. | 
| registries/runs/cancel | 아닙니다. | 
| registries/scheduleRun | 아닙니다. | 
| registries/tasks | 예 | 
| registries/updatePolicies | 아닙니다. | 
| registries/webhooks | 예 | 
| registries/webhooks/getCallbackConfig | 아닙니다. | 
| registries/webhooks/ping | 아닙니다. | 

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| containerServices | 예 | 
| managedClusters | 예 | 

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| 애플리케이션 | 예 | 
| updateCommunicationPreference | 아닙니다. | 

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| 계정 | 예 | 

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| 경고 | 아닙니다. | 
| BillingAccounts | 아닙니다. | 
| 커넥터 | 예 | 
| Departments | 아닙니다. | 
| 차원 | 아닙니다. | 
| EnrollmentAccounts | 아닙니다. | 
| 쿼리 | 아닙니다. | 
| register | 아닙니다. | 
| Reportconfigs | 아닙니다. | 
| 보고서 | 아닙니다. | 

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| hubs | 예 | 
| hubs/authorizationPolicies | 아닙니다. | 
| hubs/connectors | 아닙니다. | 
| hubs/connectors/mappings | 아닙니다. | 
| hubs/interactions | 아닙니다. | 
| hubs/kpi | 아닙니다. | 
| hubs/links | 아닙니다. | 
| hubs/profiles | 아닙니다. | 
| hubs/roleAssignments | 아닙니다. | 
| hubs/roles | 아닙니다. | 
| hubs/suggestTypeSchema | 아닙니다. | 
| hubs/views | 아닙니다. | 
| hubs/widgetTypes | 아닙니다. | 

## <a name="microsoftdatabox"></a>Microsoft.DataBox
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| jobs | 예 | 

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| DataBoxEdgeDevices | 예 | 

## <a name="microsoftdatabricks"></a>Microsoft.Databricks
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| workspaces | 예 | 
| workspaces/virtualNetworkPeerings | 아닙니다. | 

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| catalogs | 예 | 

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| connectionManagers | 예 | 

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| dataFactories | 예 | 
| dataFactories/diagnosticSettings | 아닙니다. | 
| dataFactorySchema | 아닙니다. | 
| factories | 예 | 
| factories/integrationRuntimes | 아닙니다. | 

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| 계정 | 예 | 
| accounts/dataLakeStoreAccounts | 아닙니다. | 
| accounts/storageAccounts | 아닙니다. | 
| accounts/storageAccounts/containers | 아닙니다. | 

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| 계정 | 예 | 
| accounts/eventGridFilters | 아닙니다. | 
| accounts/firewallRules | 아닙니다. | 

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| services | 예 | 
| services/projects | 예 | 

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| servers | 예 | 
| servers/recoverableServers | 아닙니다. | 
| servers/virtualNetworkRules | 아닙니다. | 

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| servers | 예 | 
| servers/recoverableServers | 아닙니다. | 
| servers/virtualNetworkRules | 아닙니다. | 

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| servers | 예 | 
| servers/advisors | 아닙니다. | 
| servers/queryTexts | 아닙니다. | 
| servers/recoverableServers | 아닙니다. | 
| servers/topQueryStatistics | 아닙니다. | 
| servers/virtualNetworkRules | 아닙니다. | 
| servers/waitStatistics | 아닙니다. | 

## <a name="microsoftdevices"></a>Microsoft.Devices
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| IotHubs | 예 | 
| IotHubs/eventGridFilters | 아닙니다. | 
| ProvisioningServices | 예 | 
| usages | 아닙니다. | 

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| controllers | 예 | 

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| labs | 예 | 
| labs/serviceRunners | 예 | 
| labs/virtualMachines | 예 | 
| schedules | 예 | 

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| databaseAccountNames | 아닙니다. | 
| databaseAccounts | 예 | 

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| domains | 예 | 
| domains/domainOwnershipIdentifiers | 아닙니다. | 
| generateSsoRequest | 아닙니다. | 
| topLevelDomains | 아닙니다. | 
| validateDomainRegistrationInformation | 아닙니다. | 

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| lcsprojects | 아닙니다. | 
| lcsprojects/clouddeployments | 아닙니다. | 
| lcsprojects/connectors | 아닙니다. | 

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| domains | 예 | 
| domains/topics | 아닙니다. | 
| eventSubscriptions | 아닙니다. | 
| extensionTopics | 아닙니다. | 
| topics | 예 | 
| topicTypes | 아닙니다. | 

## <a name="microsofteventhub"></a>Microsoft.EventHub
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| clusters | 예 | 
| namespaces | 예 | 
| namespaces/authorizationrules | 아닙니다. | 
| namespaces/disasterrecoveryconfigs | 아닙니다. | 
| namespaces/eventhubs | 아닙니다. | 
| namespaces/eventhubs/authorizationrules | 아닙니다. | 
| namespaces/eventhubs/consumergroups | 아닙니다. | 

## <a name="microsoftfeatures"></a>Microsoft.Features
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| 기능 | 아닙니다. | 
| providers | 아닙니다. | 

## <a name="microsoftgallery"></a>Microsoft.Gallery
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| enroll | 아닙니다. | 
| galleryitems | 아닙니다. | 
| generateartifactaccessuri | 아닙니다. | 
| myareas | 아닙니다. | 
| myareas/areas | 아닙니다. | 
| myareas/areas/areas | 아닙니다. | 
| myareas/areas/areas/galleryitems | 아닙니다. | 
| myareas/areas/galleryitems | 아닙니다. | 
| myareas/galleryitems | 아닙니다. | 
| register | 아닙니다. | 
| 리소스 | 아닙니다. | 
| retrieveresourcesbyid | 아닙니다. | 

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| guestConfigurationAssignments | 아닙니다. | 
| software | 아닙니다. | 

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| hanaInstances | 예 | 

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| clusters | 예 | 
| clusters/applications | 아닙니다. | 

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| jobs | 예 | 

## <a name="microsoftinformationprotection"></a>Microsoft.InformationProtection
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| labelGroups | 아닙니다. | 
| labelGroups/labels | 아닙니다. | 
| labelGroups/labels/conditions | 아닙니다. | 
| labelGroups/labels/subLabels | 아닙니다. | 
| labelGroups/labels/subLabels/conditions | 아닙니다. | 

## <a name="microsoftinsights"></a>microsoft.insights
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| actiongroups | 예 | 
| activityLogAlerts | 예 | 
| alertrules | 예 | 
| automatedExportSettings | 아닙니다. | 
| autoscalesettings | 예 | 
| baseline | 아닙니다. | 
| calculatebaseline | 아닙니다. | 
| components | 예 | 
| components/events | 아닙니다. | 
| components/pricingPlans | 아닙니다. | 
| components/query | 아닙니다. | 
| diagnosticSettings | 아닙니다. | 
| diagnosticSettingsCategories | 아닙니다. | 
| eventCategories | 아닙니다. | 
| eventtypes | 아닙니다. | 
| extendedDiagnosticSettings | 아닙니다. | 
| logDefinitions | 아닙니다. | 
| logprofiles | 아닙니다. | 
| 로그 | 아닙니다. | 
| metricAlerts | 예 |
| migrateToNewPricingModel | 아닙니다. | 
| myWorkbooks | 아닙니다. | 
| 쿼리 | 아닙니다. | 
| rollbackToLegacyPricingModel | 아닙니다. | 
| scheduledqueryrules | 예 | 
| vmInsightsOnboardingStatuses | 아닙니다. | 
| webtests | 예 | 
| workbooks | 예 | 

## <a name="microsoftintune"></a>Microsoft.Intune
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| diagnosticSettings | 아닙니다. | 
| diagnosticSettingsCategories | 아닙니다. | 

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| IoTApps | 예 | 

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| 그래프 | 예 | 

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| deletedVaults | 아닙니다. | 
| vaults | 예 | 
| vaults/accessPolicies | 아닙니다. | 
| vaults/secrets | 아닙니다. | 

## <a name="microsoftkusto"></a>Microsoft.Kusto
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| clusters | 예 | 
| clusters/databases | 아닙니다. | 
| clusters/databases/dataconnections | 아닙니다. | 
| clusters/databases/eventhubconnections | 아닙니다. | 

## <a name="microsoftlabservices"></a>Microsoft.LabServices
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| labaccounts | 예 | 
| users | 아닙니다. | 

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| 계정 | 예 | 

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| 계정 | 예 | 

## <a name="microsoftloganalytics"></a>Microsoft.LogAnalytics
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| 로그 | 아닙니다. | 

## <a name="microsoftlogic"></a>Microsoft.Logic
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| integrationAccounts | 예 | 
| workflows | 예 | 

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| commitmentPlans | 예 | 
| webServices | 예 | 
| 작업 영역 | 예 | 

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| 계정 | 예 | 
| accounts/workspaces | 예 | 
| accounts/workspaces/projects | 예 | 
| teamAccounts | 예 | 
| teamAccounts/workspaces | 예 | 
| teamAccounts/workspaces/projects | 예 | 

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| 계정 | 예 | 

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| workspaces | 예 | 
| workspaces/computes | 아닙니다. | 

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| Identities | 아닙니다. | 
| userAssignedIdentities | 예 | 

## <a name="microsoftmanagement"></a>Microsoft.Management
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| getEntities | 아닙니다. | 
| managementGroups | 아닙니다. | 
| 리소스 | 아닙니다. | 
| startTenantBackfill | 아닙니다. | 
| tenantBackfillStatus | 아닙니다. | 

## <a name="microsoftmaps"></a>Microsoft.Maps
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| 계정 | 예 | 
| accounts/eventGridFilters | 아닙니다. | 

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| offers | 아닙니다. | 
| offerTypes | 아닙니다. | 
| offerTypes/publishers | 아닙니다. | 
| offerTypes/publishers/offers | 아닙니다. | 
| offerTypes/publishers/offers/plans | 아닙니다. | 
| offerTypes/publishers/offers/plans/agreements | 아닙니다. | 
| offerTypes/publishers/offers/plans/configs | 아닙니다. | 
| offerTypes/publishers/offers/plans/configs/importImage | 아닙니다. | 
| privategalleryitems | 아닙니다. | 
| products | 아닙니다. | 

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| classicDevServices | 예 | 
| updateCommunicationPreference | 아닙니다. | 

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| agreements | 아닙니다. | 
| offertypes | 아닙니다. | 

## <a name="microsoftmedia"></a>Microsoft.Media
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| mediaservices | 예 | 
| mediaservices/accountFilters | 아닙니다. | 
| mediaservices/assets | 아닙니다. | 
| mediaservices/assets/assetFilters | 아닙니다. | 
| mediaservices/contentKeyPolicies | 아닙니다. | 
| mediaservices/eventGridFilters | 아닙니다. | 
| mediaservices/liveEventOperations | 아닙니다. | 
| mediaservices/liveEvents | 예 | 
| mediaservices/liveEvents/liveOutputs | 아닙니다. | 
| mediaservices/liveOutputOperations | 아닙니다. | 
| mediaservices/streamingEndpointOperations | 아닙니다. | 
| mediaservices/streamingEndpoints | 예 | 
| mediaservices/streamingLocators | 아닙니다. | 
| mediaservices/streamingPolicies | 아닙니다. | 
| mediaservices/transforms | 아닙니다. | 
| mediaservices/transforms/jobs | 아닙니다. | 

## <a name="microsoftmigrate"></a>Microsoft.Migrate
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| projects | 예 | 

## <a name="microsoftnetwork"></a>Microsoft.Network
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| applicationGateways | 예 | 
| applicationSecurityGroups | 예 | 
| azureFirewallFqdnTags | 아닙니다. | 
| azureFirewalls | 예 | 
| bgpServiceCommunities | 아닙니다. | 
| connections | 예 | 
| ddosCustomPolicies | 예 | 
| ddosProtectionPlans | 예 | 
| dnsOperationStatuses | 아닙니다. | 
| dnszones | 예 | 
| dnszones/A | 아닙니다. | 
| dnszones/AAAA | 아닙니다. | 
| dnszones/all | 아닙니다. | 
| dnszones/CAA | 아닙니다. | 
| dnszones/CNAME | 아닙니다. | 
| dnszones/MX | 아닙니다. | 
| dnszones/NS | 아닙니다. | 
| dnszones/PTR | 아닙니다. | 
| dnszones/recordsets | 아닙니다. | 
| dnszones/SOA | 아닙니다. | 
| dnszones/SRV | 아닙니다. | 
| dnszones/TXT | 아닙니다. | 
| expressRouteCircuits | 예 | 
| expressRouteServiceProviders | 아닙니다. | 
| frontdoors | 예 | 
| frontdoorWebApplicationFirewallPolicies | 예 | 
| getDnsResourceReference | 아닙니다. | 
| interfaceEndpoints | 예 | 
| internalNotify | 아닙니다. | 
| loadBalancers | 예 | 
| localNetworkGateways | 예 | 
| natGateways | 예 | 
| networkIntentPolicies | 예 | 
| networkInterfaces | 예 | 
| networkProfiles | 예 | 
| networkSecurityGroups | 예 | 
| networkWatchers | 예 | 
| networkWatchers/connectionMonitors | 예 | 
| networkWatchers/lenses | 예 | 
| networkWatchers/pingMeshes | 예 | 
| privateLinkServices | 예 | 
| publicIPAddresses | 예 | 
| publicIPPrefixes | 예 | 
| routeFilters | 예 | 
| routeTables | 예 | 
| serviceEndpointPolicies | 예 | 
| trafficManagerGeographicHierarchies | 아닙니다. | 
| trafficmanagerprofiles | 예 | 
| trafficmanagerprofiles/heatMaps | 아닙니다. | 
| virtualHubs | 예 | 
| virtualNetworkGateways | 예 | 
| virtualNetworks | 예 | 
| virtualNetworkTaps | 예 | 
| virtualWans | 예 | 
| vpnGateways | 예 | 
| vpnSites | 예 | 
| webApplicationFirewallPolicies | 예 | 

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| namespaces | 예 | 
| namespaces/notificationHubs | 예 | 

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| devices | 아닙니다. | 
| linkTargets | 아닙니다. | 
| storageInsightConfigs | 아닙니다. | 
| workspaces | 예 | 
| workspaces/dataSources | 아닙니다. | 
| workspaces/linkedServices | 아닙니다. | 
| workspaces/query | 아닙니다. | 

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| managementassociations | 아닙니다. | 
| managementconfigurations | 예 | 
| solutions | 예 | 
| 뷰 | 예 | 

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| policyEvents | 아닙니다. | 
| policyStates | 아닙니다. | 
| policyTrackedResources | 아닙니다. | 
| remediations | 아닙니다. | 

## <a name="microsoftportal"></a>Microsoft.Portal
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| consoles | 아닙니다. | 
| dashboards | 예 | 
| userSettings | 아닙니다. | 

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| workspaceCollections | 예 | 

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| capacities | 예 | 

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| 계정 | 예 | 

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| backupProtectedItems | 아닙니다. | 
| vaults | 예 | 

## <a name="microsoftrelay"></a>Microsoft.Relay
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| namespaces | 예 | 
| namespaces/authorizationrules | 아닙니다. | 
| namespaces/hybridconnections | 아닙니다. | 
| namespaces/hybridconnections/authorizationrules | 아닙니다. | 
| namespaces/wcfrelays | 아닙니다. | 
| namespaces/wcfrelays/authorizationrules | 아닙니다. | 

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| 리소스 | 아닙니다. | 
| subscriptionsStatus | 아닙니다. | 

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| availabilityStatuses | 아닙니다. | 
| childAvailabilityStatuses | 아닙니다. | 
| childResources | 아닙니다. | 
| events | 아닙니다. | 
| impactedResources | 아닙니다. | 
| 알림 | 아닙니다. | 

## <a name="microsoftresources"></a>Microsoft.Resources
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| 배포 | 아닙니다. | 
| 배포/작업 | 아닙니다. | 
| links | 아닙니다. | 
| notifyResourceJobs | 아닙니다. | 
| providers | 아닙니다. | 
| resourceGroups | 아닙니다. | 
| 리소스 | 아닙니다. | 
| 구독 | 아닙니다. | 
| subscriptions/providers | 아닙니다. | 
| subscriptions/resourceGroups | 아닙니다. | 
| subscriptions/resourcegroups/resources | 아닙니다. | 
| subscriptions/resources | 아닙니다. | 
| subscriptions/tagnames | 아닙니다. | 
| subscriptions/tagNames/tagValues | 아닙니다. | 
| tenants | 아닙니다. | 

## <a name="microsoftsaas"></a>Microsoft.SaaS
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| 애플리케이션 | 예 | 
| saasresources | 아닙니다. | 

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| flows | 예 | 
| jobcollections | 예 | 

## <a name="microsoftsearch"></a>Microsoft.Search
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| resourceHealthMetadata | 아닙니다. | 
| searchServices | 예 | 

## <a name="microsoftsecurity"></a>Microsoft.Security
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| advancedThreatProtectionSettings | 아닙니다. | 
| 경고 | 아닙니다. | 
| allowedConnections | 아닙니다. | 
| appliances | 아닙니다. | 
| applicationWhitelistings | 아닙니다. | 
| AutoProvisioningSettings | 아닙니다. | 
| Compliances | 아닙니다. | 
| dataCollectionAgents | 아닙니다. | 
| discoveredSecuritySolutions | 아닙니다. | 
| externalSecuritySolutions | 아닙니다. | 
| InformationProtectionPolicies | 아닙니다. | 
| jitNetworkAccessPolicies | 아닙니다. | 
| monitoring | 아닙니다. | 
| monitoring/antimalware | 아닙니다. | 
| monitoring/baseline | 아닙니다. | 
| monitoring/patch | 아닙니다. | 
| 정책 | 아닙니다. | 
| pricings | 아닙니다. | 
| securityContacts | 아닙니다. | 
| securitySolutions | 아닙니다. | 
| securitySolutionsReferenceData | 아닙니다. | 
| securityStatus | 아닙니다. | 
| securityStatus/endpoints | 아닙니다. | 
| securityStatus/subnets | 아닙니다. | 
| securityStatus/virtualMachines | 아닙니다. | 
| securityStatuses | 아닙니다. | 
| securityStatusesSummaries | 아닙니다. | 
| 설정 | 아닙니다. | 
| 태스크 | 아닙니다. | 
| topologies | 아닙니다. | 
| workspaceSettings | 아닙니다. | 

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| diagnosticSettings | 아닙니다. | 
| diagnosticSettingsCategories | 아닙니다. | 

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| namespaces | 예 | 
| namespaces/authorizationrules | 아닙니다. | 
| namespaces/disasterrecoveryconfigs | 아닙니다. | 
| namespaces/eventgridfilters | 아닙니다. | 
| namespaces/queues | 아닙니다. | 
| namespaces/queues/authorizationrules | 아닙니다. | 
| namespaces/topics | 아닙니다. | 
| namespaces/topics/authorizationrules | 아닙니다. | 
| namespaces/topics/subscriptions | 아닙니다. | 
| namespaces/topics/subscriptions/rules | 아닙니다. | 
| premiumMessagingRegions | 아닙니다. | 

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| clusters | 예 | 
| clusters/applications | 아닙니다. | 

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| 애플리케이션 | 예 | 
| gateways | 예 | 
| networks | 예 | 
| secrets | 예 | 
| volumes | 예 | 

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| SignalR | 예 | 

## <a name="microsoftsolutions"></a>Microsoft.Solutions
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| applianceDefinitions | 예 | 
| appliances | 예 | 
| applicationDefinitions | 예 | 
| 애플리케이션 | 예 | 
| jitRequests | 예 | 

## <a name="microsoftsql"></a>Microsoft.SQL
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| managedInstances | 예 |
| managedInstances/databases | 예(아래 참고를 참조) |
| managedInstances/databases/backupShortTermRetentionPolicies | 아닙니다. |
| managedInstances/databases/schemas/tables/columns/sensitivityLabels | 아닙니다. |
| managedInstances/databases/vulnerabilityAssessments | 아닙니다. |
| managedInstances/databases/vulnerabilityAssessments/rules/baselines | 아닙니다. |
| managedInstances/encryptionProtector | 아닙니다. |
| managedInstances/keys | 아닙니다. |
| managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies | 아닙니다. |
| managedInstances/vulnerabilityAssessments | 아닙니다. |
| servers | 예 | 
| servers/administrators | 아닙니다. | 
| servers/communicationLinks | 아닙니다. | 
| servers/databases | 예(아래 참고를 참조) | 
| servers/encryptionProtector | 아닙니다. | 
| servers/firewallRules | 아닙니다. | 
| servers/keys | 아닙니다. | 
| servers/restorableDroppedDatabases | 아닙니다. | 
| servers/serviceobjectives | 아닙니다. | 
| servers/tdeCertificates | 아닙니다. | 

> [!NOTE]
> master 데이터베이스는 태그를 지원하지 않지만, Data Warehouse 데이터베이스를 포함한 다른 데이터베이스는 태그를 지원합니다.


## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| SqlVirtualMachineGroups | 예 | 
| SqlVirtualMachineGroups/AvailabilityGroupListeners | 아닙니다. | 
| SqlVirtualMachines | 예 | 

## <a name="microsoftstorage"></a>Microsoft.Storage
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| storageAccounts | 예 | 
| storageAccounts/blobServices | 아닙니다. | 
| storageAccounts/fileServices | 아닙니다. | 
| storageAccounts/queueServices | 아닙니다. | 
| storageAccounts/services | 아닙니다. | 
| storageAccounts/tableServices | 아닙니다. | 
| usages | 아닙니다. | 

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| storageSyncServices | 예 | 
| storageSyncServices/registeredServers | 아닙니다. | 
| storageSyncServices/syncGroups | 아닙니다. | 
| storageSyncServices/syncGroups/cloudEndpoints | 아닙니다. | 
| storageSyncServices/syncGroups/serverEndpoints | 아닙니다. | 
| storageSyncServices/workflows | 아닙니다. | 

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| managers | 예 | 

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| streamingjobs | 예(아래 참고를 참조) | 
| streamingjobs/diagnosticSettings | 아닙니다. | 

> [!NOTE]
> streamingjobs를 실행 중일 때 태그를 추가할 수 없습니다. 태그를 추가하려면 리소스를 중지합니다.

## <a name="microsoftsubscription"></a>Microsoft.Subscription
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| CreateSubscription | 아닙니다. | 
| SubscriptionDefinitions | 아닙니다. | 
| SubscriptionOperations | 아닙니다. | 

## <a name="microsoftsupport"></a>microsoft.support
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| supporttickets | 아닙니다. | 

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| providerRegistrations | 예 | 
| 리소스 | 예 | 

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| environments | 예 | 
| environments/accessPolicies | 아닙니다. | 
| environments/eventsources | 예 | 
| environments/referenceDataSets | 예 | 

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| 계정 | 예 | 
| account/extension | 예 | 
| account/project | 예 | 

## <a name="microsoftweb"></a>Microsoft.Web
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| apiManagementAccounts | 아닙니다. | 
| apiManagementAccounts/apiAcls | 아닙니다. | 
| apiManagementAccounts/apis | 아닙니다. | 
| apiManagementAccounts/apis/apiAcls | 아닙니다. | 
| apiManagementAccounts/apis/connectionAcls | 아닙니다. | 
| apiManagementAccounts/apis/connections | 아닙니다. | 
| apiManagementAccounts/apis/connections/connectionAcls | 아닙니다. | 
| apiManagementAccounts/apis/localizedDefinitions | 아닙니다. | 
| apiManagementAccounts/connectionAcls | 아닙니다. | 
| apiManagementAccounts/connections | 아닙니다. | 
| billingMeters | 아닙니다. | 
| certificates | 예 | 
| connectionGateways | 예 | 
| connections | 예 | 
| customApis | 예 | 
| deletedSites | 아닙니다. | 
| functions | 아닙니다. | 
| hostingEnvironments | 예 | 
| hostingEnvironments/multiRolePools | 아닙니다. | 
| hostingEnvironments/multiRolePools/instances | 아닙니다. | 
| hostingEnvironments/workerPools | 아닙니다. | 
| hostingEnvironments/workerPools/instances | 아닙니다. | 
| publishingUsers | 아닙니다. | 
| 동영상 추천 기능 | 아닙니다. | 
| resourceHealthMetadata | 아닙니다. | 
| runtimes | 아닙니다. | 
| serverFarms | 예 | 
| serverFarms/workers | 아닙니다. | 
| sites | 예 | 
| sites/domainOwnershipIdentifiers | 아닙니다. | 
| sites/hostNameBindings | 아닙니다. | 
| sites/instances | 아닙니다. | 
| sites/instances/extensions | 아닙니다. | 
| sites/premieraddons | 예 | 
| sites/recommendations | 아닙니다. | 
| sites/resourceHealthMetadata | 아닙니다. | 
| sites/slots | 예 | 
| sites/slots/hostNameBindings | 아닙니다. | 
| sites/slots/instances | 아닙니다. | 
| sites/slots/instances/extensions | 아닙니다. | 
| sourceControls | 아닙니다. | 
| validate | 아닙니다. | 
| verifyHostingEnvironmentVnet | 아닙니다. | 

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| diagnosticSettings | 아닙니다. | 
| diagnosticSettingsCategories | 아닙니다. | 

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| DeviceServices | 예 | 

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor
| 리소스 종류 | 전체 모드 삭제 |
| ------------- | ----------- |
| components | 아닙니다. | 
| componentsSummary | 아닙니다. | 
| monitorInstances | 아닙니다. | 
| monitorInstancesSummary | 아닙니다. | 
| monitors | 아닙니다. | 
| notificationSettings | 아닙니다. | 

## <a name="next-steps"></a>다음 단계
리소스에 태그를 적용하는 방법을 알아보려면 [태그를 사용하여 Azure 리소스 구성](resource-group-using-tags.md)을 참조하세요.
