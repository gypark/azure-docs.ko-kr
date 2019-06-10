---
title: PowerShell 예제 복사 Azure SQL 데이터베이스 새 서버 | Microsoft Docs
description: SQL Database를 새 서버에 복사하는 Azure PowerShell 예제 스크립트
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: b03b2e6096e00097790f5e71783ef388627087c9
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66729394"
---
# <a name="use-powershell-to-copy-a-sql-database-to-a-new-server"></a>PowerShell을 사용하여 SQL Database를 새 서버에 복사

PowerShell 스크립트 예제에서는 새 서버에 기존 데이터베이스의 복사본을 만듭니다.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

PowerShell을 로컬로 설치하고 사용하도록 선택하는 경우 이 자습서에는 AZ PowerShell 1.4.0 이상이 필요합니다. 업그레이드해야 하는 경우 [Azure PowerShell 모듈 설치](/powershell/azure/install-az-ps)를 참조하세요. 또한 PowerShell을 로컬로 실행하는 경우 `Connect-AzAccount`를 실행하여 Azure와 연결해야 합니다.

## <a name="copy-a-database-to-a-new-server"></a>새 서버에 데이터베이스 복사

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/copy-database-to-new-server/copy-database-to-new-server.ps1?highlight=20-23 "Copy database to new server")]

## <a name="clean-up-deployment"></a>배포 정리

다음 명령을 사용하여 리소스 그룹 및 모든 관련 리소스를 제거합니다.

```powershell
Remove-AzResourceGroup -ResourceGroupName $sourceresourcegroupname
Remove-AzResourceGroup -ResourceGroupName $targetresourcegroupname
```

## <a name="script-explanation"></a>스크립트 설명

이 스크립트는 다음 명령을 사용합니다. 테이블에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | 모든 리소스가 저장되는 리소스 그룹을 만듭니다. |
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | 단일 데이터베이스 또는 탄력적 풀을 호스트하는 SQL Database 서버를 만듭니다. |
| [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) | 단일 데이터베이스 또는 탄력적 풀을 만듭니다. |
| [New-AzSqlDatabaseCopy](/powershell/module/az.sql/new-azsqldatabasecopy) | 현재 시간에 스냅샷을 사용하는 데이터베이스의 복사본을 만듭니다. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | 모든 중첩 리소스를 포함한 리소스 그룹을 삭제합니다. |
|||

## <a name="next-steps"></a>다음 단계

Azure PowerShell에 대한 자세한 내용은 [Azure PowerShell 설명서](/powershell/azure/overview)를 참조하세요.

추가 SQL Database PowerShell 스크립트 샘플은 [Azure SQL Database PowerShell 스크립트](../sql-database-powershell-samples.md)에 있습니다.
