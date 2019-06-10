---
title: 샘플 - ISO 27001 ASE/SQL 워크로드 청사진 - 배포 단계
description: ISO 27001 App Service Environment/SQL Database 워크로드 청사진 샘플의 배포 단계입니다.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/14/2019
ms.topic: sample
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 78f608aedd53aa1071eaf88864f5a63f8f9e6072
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59791014"
---
# <a name="deploy-the-iso-27001-app-service-environmentsql-database-workload-blueprint-sample"></a>ISO 27001 App Service Environment/SQL Database 워크로드 청사진 샘플 배포

Azure Blueprints ISO 27001 App Service Environment/SQL Database 워크로드 청사진 샘플을 배포하려면 다음 단계를 수행해야 합니다.

> [!div class="checklist"]
> - [ISO 27001 Shared Services](../iso27001-shared/index.md) 청사진 샘플 배포
> - 샘플에서 새 청사진 만들기
> - 샘플 사본을 **게시됨**으로 표시
> - 기존 구독에 청사진 사본 할당

Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free)을 만듭니다.

## <a name="deploy-the-iso-27001-shared-services-blueprint-sample"></a>ISO 27001 Shared Services 청사진 샘플 배포

[ISO 27001 Shared Services](../iso27001-shared/index.md) 청사진 샘플을 대상 구독에 배포해야 이 청사진 샘플을 배포할 수 있습니다. ISO 27001 Shared Services 청사진 샘플을 성공적으로 배포하지 않으면 이 청사진 샘플 배포 중에 인프라 종속성이 누락되어 배포가 실패합니다.

> [!IMPORTANT]
> 이 청사진 샘플은 [ISO 27001 Shared Services](../iso27001-shared/index.md) 청사진 샘플과 같은 구독에 할당되어야 합니다.

## <a name="create-blueprint-from-sample"></a>샘플에서 청사진 만들기

먼저 샘플을 스타터로 사용하여 환경 내에 새 청사진을 만들어 청사진 샘플을 구현합니다.

1. **모든 서비스**를 선택하고 왼쪽 창에서 **정책**을 검색하여 선택합니다. **정책** 페이지에서 **청사진**을 선택합니다.

1. **시작** 페이지 왼쪽에서 _청사진 만들기_ 아래의 **만들기** 단추를 선택합니다.

1. **ISO 27001: ASE/SQL 워크로드** 청사진 샘플을 _다른 샘플_ 아래에서 찾은 다음, **이 샘플 사용**을 선택합니다.

1. 청사진 샘플의 _기본 사항_을 입력합니다.

   - **청사진 이름**: ISO 27001 ASE/SQL 워크로드 청사진 샘플 사본의 이름을 지정합니다.
   - **정의 위치**: 줄임표를 사용하고 샘플 사본을 저장할 관리 그룹을 선택합니다.

1. 페이지 위쪽의 _아티팩트_ 탭 또는 페이지 아래쪽의 **다음: 아티팩트**를 선택합니다.

1. 청사진 샘플을 구성하는 아티팩트 목록을 검토합니다. 아티팩트 대부분에 매개 변수는 나중에 정의할 것입니다. 청사진 샘플 검토가 끝나면 **초안 저장**을 선택합니다.

## <a name="publish-the-sample-copy"></a>샘플 사본 게시

이제 작업 환경에 청사진 샘플 사본이 만들어졌습니다. 이 청사진 정의는 **초안** 모드로 만들어졌으며 먼저 **게시**해야만 할당하고 배포할 수 있습니다. 청사진 샘플의 사본을 환경 및 필요에 맞게 사용자 지정할 수 있지만, 해당 수정으로 ISO 27001 표준에서 벗어날 수 있습니다.

1. **모든 서비스**를 선택하고 왼쪽 창에서 **정책**을 검색하여 선택합니다. **정책** 페이지에서 **청사진**을 선택합니다.

1. 왼쪽 페이지에서 **청사진 정의**를 선택합니다. 필터를 사용하여 청사진 샘플의 사본을 찾은 다음, 선택합니다.

1. 페이지 맨 위에서 **청사진 게시**를 선택합니다. 오른쪽의 새 페이지에서 청사진 샘플 사본의 **버전**을 지정합니다. 이 속성은 나중에 수정할 경우에 유용합니다. "ISO 27001 청사진 샘플에서 게시된 첫 번째 버전"과 같은 **변경 메모**를 제공합니다. 그런 후 페이지 아래쪽의 **게시**를 선택합니다.

## <a name="assign-the-sample-copy"></a>샘플 사본 할당

청사진 샘플 사본이 **게시**되면 저장된 관리 그룹 내의 구독에 할당할 수 있습니다. 이 단계에서는 청사진 샘플 사본의 배포를 고유하게 만들기 위해 매개 변수가 제공됩니다.

1. **모든 서비스**를 선택하고 왼쪽 창에서 **정책**을 검색하여 선택합니다. **정책** 페이지에서 **청사진**을 선택합니다.

1. 왼쪽 페이지에서 **청사진 정의**를 선택합니다. 필터를 사용하여 청사진 샘플의 사본을 찾은 다음, 선택합니다.

1. 청사진 정의 페이지 위쪽에서 **청사진 할당**을 선택합니다.

1. 청사진 할당의 매개 변수 값을 다음과 같이 지정합니다.

   - 기본 사항

     - **구독**: 청사진 샘플 사본을 저장한 관리 그룹에 있는 하나 이상의 구독을 선택합니다. 둘 이상의 구독을 선택하면 입력한 매개 변수를 사용하여 각 구독의 할당이 생성됩니다.
     - **할당 이름**: 청사진의 이름에 따라 이 이름이 미리 채워집니다.
       필요에 따라 변경하거나 그대로 둡니다.
     - **위치**: 관리 ID를 만들 지역을 선택합니다. Azure Blueprints는 이 관리 ID를 사용하여 할당된 청사진의 모든 아티팩트를 배포합니다. 자세히 알아보려면 [Azure 리소스의 관리 ID](../../../../active-directory/managed-identities-azure-resources/overview.md)를 참조하세요.
     - **청사진 정의 버전**: 청사진 샘플 사본의 **게시됨** 버전을 선택합니다.

   - 할당 잠금

     환경에 맞는 청사진 잠금 설정을 선택합니다. 자세한 내용은 [청사진 리소스 잠금](../../concepts/resource-locking.md)을 참조하세요.

   - 관리 ID

     기본값 _시스템이 할당한_ 관리 ID 옵션을 유지합니다.

   - 청사진 매개 변수

     이 섹션에 정의된 매개 변수는 일관성을 제공하기 위해 청사진 정의의 아티팩트 대부분에서 사용됩니다.

     - **조직 이름**: 조직에 대한 짧은 이름을 입력합니다. 이 속성은 주로 리소스 이름을 지정하기 위해 사용됩니다.
     - **Shared Service 구독 ID**: [ISO 27001 Shared Services](../iso27001-shared/index.md) 청사진 샘플이 할당된 구독 ID입니다.
     - **기본 서브넷 주소 접두사**: 가상 네트워크 기본 서브넷에 대한 CIDR 표기법입니다.
       기본값은 _10.1.0.0/16_입니다.
     - **워크로드 위치**: 아티팩트를 배포하는 위치를 결정합니다. 일부 서비스는 모든 위치에서 사용할 수 없습니다. 이러한 서비스를 배포하는 아티팩트는 해당 아티팩트를 배포하는 위치에 대한 매개 변수 옵션을 제공합니다.

   - 아티팩트 매개 변수

     이 섹션에 정의된 매개 변수는 정의된 아티팩트에 적용됩니다. 이러한 매개 변수는 청사진 할당 중에 정의되므로 [동적 매개 변수](../../concepts/parameters.md#dynamic-parameters)입니다. 전체 목록 또는 아티팩트 매개 변수 및 해당 설명은 [아티팩트 매개 변수 테이블](#artifact-parameters-table)을 참조하세요.

1. 모든 매개 변수를 입력한 후에는 페이지 아래쪽에서 **할당**을 선택합니다. 청사진 할당이 생성되고 아티팩트 배포가 시작됩니다. 배포에는 약 1 시간이 걸립니다. 배포의 상태를 확인하려면 청사진 할당을 엽니다.

> [!WARNING]
> Azure Blueprints 서비스 및 기본 제공 청사진 샘플은 **무료**입니다. Azure 리소스는 [제품별로 가격이 책정](https://azure.microsoft.com/pricing/)됩니다. 이 청사진 샘플에서 배포되는 리소스를 실행하는 비용을 추정하려면 [가격 계산기](https://azure.microsoft.com/pricing/calculator/)를 사용합니다.

## <a name="artifact-parameters-table"></a>아티팩트 매개 변수 테이블

다음 테이블은 청사진 아티팩트 매개 변수의 목록을 제공합니다.

|아티팩트 이름|아티팩트 형식|매개 변수 이름|설명|
|-|-|-|-|
|Log Analytics 리소스 그룹|리소스 그룹|Name|**잠김** - **조직 이름**을 `-workload-log-rg`와 연결하여 리소스 그룹을 고유하게 만듭니다.|
|Log Analytics 리소스 그룹|리소스 그룹|위치|**잠김** - 청사진 매개 변수를 사용합니다.|
|Log Analytics 템플릿|Resource Manager 템플릿|서비스 계층|Log Analytics 작업 영역의 계층을 설정합니다. 기본값은 _PerNode_입니다.|
|Log Analytics 템플릿|Resource Manager 템플릿|로그 보존(일)|데이터 보존 기간(일)입니다. 기본값은 _365_입니다.|
|Log Analytics 템플릿|Resource Manager 템플릿|위치|Log Analytics 작업 영역을 만드는 데 사용되는 지역입니다. 기본값은 _미국 서부 2_입니다.|
|네트워크 리소스 그룹|리소스 그룹|Name|**잠김** - **조직 이름**을 `-workload-net-rg`와 연결하여 리소스 그룹을 고유하게 만듭니다.|
|네트워크 리소스 그룹|리소스 그룹|위치|**잠김** - 청사진 매개 변수를 사용합니다.|
|네트워크 보안 그룹 템플릿|Resource Manager 템플릿|로그 보존(일)|데이터 보존 기간(일)입니다. 기본값은 _365_입니다.|
|Virtual Network 및 경로 테이블 템플릿|Resource Manager 템플릿|Azure Firewall 프라이빗 IP|[Azure Firewall](../../../../firewall/overview.md)의 개인 IP를 구성합니다. _ISO 27001: Shared Services_ 아티팩트 매개 변수 **Azure Firewall 서브넷 주소 접두사**에 정의된 CIDR 표기법의 일부여야 합니다. 기본값은 _10.0.4.4_입니다.|
|Virtual Network 및 경로 테이블 템플릿|Resource Manager 템플릿|Shared services 구독 ID|워크로드와 Shared Services 간에 VNET 피어링을 사용하도록 설정하는 데 사용되는 값입니다.|
|Virtual Network 및 경로 테이블 템플릿|Resource Manager 템플릿|Virtual Network 주소 접두사|가상 네트워크에 대한 CIDR 표기법입니다. 기본값은 _10.1.0.0/16_입니다.|
|Virtual Network 및 경로 테이블 템플릿|Resource Manager 템플릿|기본 서브넷 주소 접두사|가상 네트워크 기본 서브넷에 대한 CIDR 표기법입니다. 기본값은 _10.1.0.0/16_입니다.|
|Virtual Network 및 경로 테이블 템플릿|Resource Manager 템플릿|ADDS IP 주소|첫 번째 ADDS VM의 IP 주소입니다. 이 값은 사용자 지정 VNET DNS로 사용됩니다.|
|주요 자격 증명 모음 리소스 그룹|리소스 그룹|Name|**잠김** - **조직 이름**을 `-workload-kv-rg`와 연결하여 리소스 그룹을 고유하게 만듭니다.|
|주요 자격 증명 모음 리소스 그룹|리소스 그룹|위치|**잠김** - 청사진 매개 변수를 사용합니다.|
|Key Vault 템플릿|Resource Manager 템플릿|AAD 개체 ID|Key Vault 인스턴스에 액세스해야 하는 계정의 AAD 개체 ID입니다. 기본값은 없으며 비워 둘 수 없습니다. Azure Portal에서 이 값을 찾으려면 _서비스_ 아래에서 "사용자"를 검색하여 선택합니다. _이름_ 상자를 사용하여 계정 이름을 필터링하고 해당 계정을 선택합니다. _사용자 프로필_ 페이지에서 _개체 ID_ 옆의 “복사하려면 클릭” 아이콘을 선택합니다.|
|Key Vault 템플릿|Resource Manager 템플릿|로그 보존(일)|데이터 보존 기간(일)입니다. 기본값은 _365_입니다.|
|Key Vault 템플릿|Resource Manager 템플릿|Key Vault SKU|생성된 Key Vault의 SKU를 지정합니다. 기본값은 _프리미엄_입니다.|
|Key Vault 템플릿|Resource Manager 템플릿|Azure SQL Server 관리 사용자 이름|Azure SQL Server에 액세스하는 데 사용되는 사용자 이름입니다. **Azure SQL Database 템플릿**의 동일한 속성 값과 일치해야 합니다. 기본값은 _sql-admin-user_입니다.|
|Azure SQL Database 리소스 그룹|리소스 그룹|Name|**잠김** - **조직 이름**을 `-workload-azsql-rg`와 연결하여 리소스 그룹을 고유하게 만듭니다.|
|Azure SQL Database 리소스 그룹|리소스 그룹|위치|**잠김** - 청사진 매개 변수를 사용합니다.|
|Azure SQL Database 템플릿|Resource Manager 템플릿|Azure SQL Server 관리 사용자 이름|Azure SQL Server에 대한 사용자 이름입니다. **Key Vault 템플릿**의 동일한 속성 값과 일치해야 합니다. 기본값은 _sql-admin-user_입니다.|
|Azure SQL Database 템플릿|Resource Manager 템플릿|Azure SQL Server 관리자 암호(Key Vault 리소스 ID)|Key Vault의 리소스 ID입니다. "/subscription/{subscriptionId}/resourceGroups/{orgName}-workload-kv/providers/Microsoft.KeyVault/vaults/{orgName}-workload-kv"를 사용하고 `{subscriptionId}`를 구독 ID로, `{orgName}`을 **조직 이름** 청사진 매개 변수로 바꿉니다.|
|Azure SQL Database 템플릿|Resource Manager 템플릿|Azure SQL Server 관리자 암호(Key Vault 비밀 이름)|SQL Server 관리자의 사용자 이름입니다. **Key Vault 템플릿** 속성 **Azure SQL Server 관리 사용자 이름**의 값과 일치해야 합니다.|
|Azure SQL Database 템플릿|Resource Manager 템플릿|로그 보존(일)|데이터 보존 기간(일)입니다. 기본값은 _365_입니다.|
|Azure SQL Database 템플릿|Resource Manager 템플릿|AAD 관리자 개체 ID|Active Directory 관리자로 할당될 사용자의 AAD 개체 ID입니다. 기본값은 없으며 비워 둘 수 없습니다. Azure Portal에서 이 값을 찾으려면 _서비스_ 아래에서 "사용자"를 검색하여 선택합니다. _이름_ 상자를 사용하여 계정 이름을 필터링하고 해당 계정을 선택합니다. _사용자 프로필_ 페이지에서 _개체 ID_ 옆의 “복사하려면 클릭” 아이콘을 선택합니다.|
|Azure SQL Database 템플릿|Resource Manager 템플릿|AAD 관리자 로그인|현재 Microsoft 계정(예: live.com 또는 outlook.com)을 관리자로 설정할 수 없습니다. 조직 내의 사용자 및 보안 그룹만 관리자로 설정할 수 있습니다. 기본값은 없으며 비워 둘 수 없습니다. Azure Portal에서 이 값을 찾으려면 _서비스_ 아래에서 "사용자"를 검색하여 선택합니다. _이름_ 상자를 사용하여 계정 이름을 필터링하고 해당 계정을 선택합니다. _사용자 프로필_ 페이지에서 _사용자 이름_을 복사합니다.|
|App Service Environment 리소스 그룹|리소스 그룹|Name|**잠김** - **조직 이름**을 `-workload-ase-rg`와 연결하여 리소스 그룹을 고유하게 만듭니다.|
|App Service Environment 리소스 그룹|리소스 그룹|위치|**잠김** - 청사진 매개 변수를 사용합니다.|
|App Service Environment 템플릿|Resource Manager 템플릿|도메인 이름|샘플에서 만든 Active Directory의 이름입니다. 기본값은 _contoso.com_입니다.|
|App Service Environment 템플릿|Resource Manager 템플릿|ASE 위치|App Service Environment 위치입니다. 기본값은 _미국 서부 2_입니다.|
|App Service Environment 템플릿|Resource Manager 템플릿|Application Gateway 로그 보존(일)|데이터 보존 기간(일)입니다. 기본값은 _365_입니다.|

## <a name="next-steps"></a>다음 단계

지금까지 ISO 27001 App Service Environment/SQL Database 워크로드 청사진 샘플을 배포하는 단계를 살펴보았으므로 다음 문서를 참조하여 아키텍처 및 컨트롤 매핑에 대해 알아보세요.

> [!div class="nextstepaction"]
> [ISO 27001 App Service Environment/SQL Database 워크로드 청사진 - 개요](./index.md)
> [ISO 27001 App Service Environment/SQL Database 워크로드 청사진 - 컨트롤 매핑](./control-mapping.md)

청사진 및 사용 방법에 대한 추가 문서:

- [청사진 수명 주기](../../concepts/lifecycle.md)에 대해 알아보기
- [정적 및 동적 매개 변수](../../concepts/parameters.md) 사용 방법 이해
- [청사진 시퀀싱 순서](../../concepts/sequencing-order.md)를 사용자 지정하는 방법 알아보기
- [청사진 리소스 잠금](../../concepts/resource-locking.md)을 활용하는 방법 알아보기
- [기존 할당을 업데이트](../../how-to/update-existing-assignments.md)하는 방법 알아보기