---
title: Azure Database Migration Service 사용을 위한 필수 구성 요소 개요 | Microsoft Docs
description: Azure Database Migration Service를 사용하여 데이터베이스 마이그레이션을 수행하기 위한 필수 구성 요소의 개요를 알아봅니다.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 05/29/2019
ms.openlocfilehash: 4e21014f7b4ed86846a100ed9a2b1cd4b0400974
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66304269"
---
# <a name="overview-of-prerequisites-for-using-the-azure-database-migration-service"></a>Azure Database Migration Service 사용을 위한 필수 구성 요소 개요

데이터베이스 마이그레이션을 수행 하는 경우 Azure Database Migration Service가 원활히 실행 되도록 하는 데 필요한 몇 가지 필수 구성 요소가 있습니다. 일부 필수 구성 요소는 서비스가 지원하는 모든 시나리오(원본-대상 쌍)에 적용되는 반면에 특정 시나리오에만 적용되는 필수 구성 요소도 있습니다.

Azure Database Migration Service 사용과 관련된 필구 구성 요소는 다음 섹션에 나열되어 있습니다.

## <a name="prerequisites-common-across-migration-scenarios"></a>마이그레이션 시나리오의 공통적인 필수 구성 요소

지원되는 모든 마이그레이션 시나리오에 공통적인 Azure Database Migration Service 필구 구성 요소는 다음을 수행해야 합니다.

* Azure Resource Manager 배포 모델을 사용하여 Azure Database Migration Service에 대한 Azure VNet(Virtual Network)을 만듭니다. 그러면 [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) 또는 [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)을 사용하여 온-프레미스 원본 서버에 사이트 간 연결이 제공됩니다.
* VNet 네트워크 보안 그룹 (NSG) 규칙에 다음을 차단 하지는 않도록 통신 포트 443, 53, 9354, 445, 12000과 합니다. Azure VNet NSG 트래픽 필터링에 대한 자세한 내용은 [네트워크 보안 그룹을 사용하여 네트워크 트래픽 필터링](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) 문서를 참조하세요.
* 원본 데이터베이스 앞에 방화벽 어플라이언스를 사용하는 경우 Azure Database Migration Service에서 원본 데이터베이스에 액세스하여 마이그레이션할 수 있도록 허용하는 방화벽 규칙을 추가해야 합니다.
* [데이터베이스 엔진 액세스를 위한 Windows 방화벽](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access)을 구성합니다.
* 문서 [서버 네트워크 프로토콜 사용 또는 사용 안 함](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure)의 지침을 수행하여 SQL Server Express를 설치하는 동안 기본적으로 사용 안 함으로 설정되어 있는 TCP/IP 프로토콜을 사용하도록 설정합니다.

    > [!IMPORTANT]
    > Azure Database Migration Service의 인스턴스를 만들려면 일반적으로 동일한 리소스 그룹 내에 있는 VNet 설정에 대 한 액세스가 필요 합니다. 결과적으로, DMS의 인스턴스를 만들고 사용자 구독 수준에서 권한이 필요 합니다. 필요에 따라, 할당할 수 있는 필요한 역할을 만들려면 다음 스크립트를 실행 합니다.
    >
    > ```
    >
    > $readerActions = `
    > "Microsoft.DataMigration/services/*/read", `
    > "Microsoft.Network/networkInterfaces/ipConfigurations/read"
    >
    > $writerActions = `
    > "Microsoft.DataMigration/services/*/write", `
    > "Microsoft.DataMigration/services/*/delete", `
    > "Microsoft.DataMigration/services/*/action"
    >
    > $writerActions += $readerActions
    >
    > # TODO: replace with actual subscription IDs
    > $subScopes = ,"/subscriptions/00000000-0000-0000-0000-000000000000/","/subscriptions/11111111-1111-1111-1111-111111111111/"
    >
    > function New-DmsReaderRole() {
    > $aRole = [Microsoft.Azure.Commands.Resources.Models.Authorization.PSRoleDefinition]::new()
    > $aRole.Name = "Azure Database Migration Reader"
    > $aRole.Description = "Lets you perform read only actions on DMS service/project/tasks."
    > $aRole.IsCustom = $true
    > $aRole.Actions = $readerActions
    > $aRole.NotActions = @()
    >
    > $aRole.AssignableScopes = $subScopes
    > #Create the role
    > New-AzRoleDefinition -Role $aRole
    > }
    >
    > function New-DmsContributorRole() {
    > $aRole = [Microsoft.Azure.Commands.Resources.Models.Authorization.PSRoleDefinition]::new()
    > $aRole.Name = "Azure Database Migration Contributor"
    > $aRole.Description = "Lets you perform CRUD actions on DMS service/project/tasks."
    > $aRole.IsCustom = $true
    > $aRole.Actions = $writerActions
    > $aRole.NotActions = @()
    >
    >   $aRole.AssignableScopes = $subScopes
    > #Create the role
    > New-AzRoleDefinition -Role $aRole
    > }
    > 
    > function Update-DmsReaderRole() {
    > $aRole = Get-AzRoleDefinition "Azure Database Migration Reader"
    > $aRole.Actions = $readerActions
    > $aRole.NotActions = @()
    > Set-AzRoleDefinition -Role $aRole
    > }
    >
    > function Update-DmsConributorRole() {
    > $aRole = Get-AzRoleDefinition "Azure Database Migration Contributor"
    > $aRole.Actions = $writerActions
    > $aRole.NotActions = @()
    > Set-AzRoleDefinition -Role $aRole
    > }
    >
    > # Invoke above functions
    > New-DmsReaderRole
    > New-DmsContributorRole
    > Update-DmsReaderRole
    > Update-DmsConributorRole
    > ```

## <a name="prerequisites-for-migrating-sql-server-to-azure-sql-database"></a>SQL Server를 Azure SQL Database로 마이그레이션하기 위한 필수 구성 요소

모든 마이그레이션 시나리오에 공통적인 Azure Database Migration Service 필수 구성 요소 외에도 한 시나리오나 다른 시나리오에만 적용되는 필수 구성 요소가 있습니다.

Azure Database Migration Service를 사용하여 SQL Server에서 Azure SQL Database로 마이그레이션을 수행하는 경우, 모든 마이그레이션 시나리오에 공통적인 필수 구성 요소 외에도 다음과 같은 필구 구성 요소를 충족해야 합니다.

* [Azure Portal에서 Azure SQL 데이터베이스 만들기](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal) 문서의 세부 지침을 수행하여 Azure SQL Database 인스턴스를 만듭니다.
* [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) v3.3 이상을 다운로드 및 설치합니다.
* Azure Database Migration Service가 기본적으로 TCP 포트 1433인 원본 SQL Server에 액세스하도록 허용하려면 Windows 방화벽을 엽니다.
* 동적 포트를 사용하여 명명된 여러 SQL Server 인스턴스를 실행하는 경우 SQL Browser 서비스를 사용하도록 설정하고 Azure Database Migration Service가 원본 서버에서 명명된 인스턴스에 연결할 수 있도록 방화벽을 통해 UDP 포트 1434에 액세스할 수 있습니다.
* 대상 데이터베이스에 대한 Azure Database Migration Service 액세스를 허용하도록 Azure SQL Database 서버에 서버 수준 [방화벽 규칙](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure)을 만듭니다. Azure Database Migration Service에 사용되는 VNET의 서브넷 범위를 제공합니다.
* 원본 SQL Server 인스턴스에 연결하는 데 사용되는 자격 증명에는 [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) 권한이 있어야 합니다.
* 대상 Azure SQL Database 인스턴스에 연결하는 데 사용되는 자격 증명에는 대상 Azure SQL 데이터베이스에 대한 CONTROL DATABASE 권한이 있어야 합니다.

   > [!NOTE]
   > Azure Database Migration Service를 사용하여 SQL Server에서 Azure SQL Database로 마이그레이션을 수행하기 위해 필요한 전체 필수 구성 요소 목록은 [SQL Server를 Azure SQL Database로 마이그레이션](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql) 자습서를 참조하세요.
   > 

## <a name="prerequisites-for-migrating-sql-server-to-an-azure-sql-database-managed-instance"></a>Azure SQL Database 관리 되는 인스턴스로 SQL Server로 마이그레이션에 대 한 필수 구성 요소

* 문서의 세부 지침을 수행 하 여 Azure SQL Database 관리 되는 인스턴스를 만듭니다 [Azure portal에서 Azure SQL Database Managed Instance 만들기](https://aka.ms/sqldbmi)합니다.
* 방화벽을 열고 Azure Database Migration Service IP 주소 또는 서브넷 범위에 대한 포트 445에서 SMB 트래픽을 허용합니다.
* Azure Database Migration Service가 기본적으로 TCP 포트 1433인 원본 SQL Server에 액세스하도록 허용하려면 Windows 방화벽을 엽니다.
* 동적 포트를 사용하여 명명된 여러 SQL Server 인스턴스를 실행하는 경우 SQL Browser 서비스를 사용하도록 설정하고 Azure Database Migration Service가 원본 서버에서 명명된 인스턴스에 연결할 수 있도록 방화벽을 통해 UDP 포트 1434에 액세스할 수 있습니다.
* 원본 SQL Server와 대상 Managed Instance를 연결하는 데 사용되는 로그인이 sysadmin 서버 역할의 구성원인지 확인합니다.
* Azure Database Migration Service가 원본 데이터베이스를 백업하는 데 사용할 수 있는 네트워크 공유를 만듭니다.
* 원본 SQL Server 인스턴스를 실행 중인 서비스 계정에 본인이 만든 네트워크 공유에 대한 쓰기 권한이 있고, 원본 서버의 컴퓨터 계정에 동일한 공유에 대한 읽기/쓰기 액세스 권한이 있는지 확인합니다.
* 이전에 만든 네트워크 공유에 대한 전체 제어 권한을 갖고 있는 Windows 사용자(및 암호)를 메모해 둡니다. Azure Database Migration Service는 사용자 자격 증명을 가장하여 복원 작업을 위한 Azure Storage 컨테이너에 백업 파일을 업로드합니다.
* [Storage Explorer를 사용하여 Azure Blob Storage 리소스 관리](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container) 문서의 단계에 따라 Blob 컨테이너를 만들고 해당 SAS URI를 검색합니다. SAS URI를 만드는 동안 정책 창에서 모든 권한(읽기, 쓰기, 삭제, 나열)을 선택해야 합니다.

   > [!NOTE]
   > Azure Database Migration Service를 사용하여 SQL Server에서 Azure SQL Database Managed Instance로 마이그레이션을 수행하기 위해 필요한 전체 필수 구성 요소 목록은 [SQL Server를 Azure SQL Database Managed Instance로 마이그레이션](https://aka.ms/migratetomiusingdms) 자습서를 참조하세요.

## <a name="next-steps"></a>다음 단계

Azure Database Migration Service 및 국가별 가용성에 대한 개요는 [Azure Database Migration Service란?](dms-overview.md) 문서를 참조하세요.
