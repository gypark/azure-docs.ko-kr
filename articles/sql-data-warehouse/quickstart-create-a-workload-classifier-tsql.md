---
title: '빠른 시작: 워크로드 분류자 만들기 - T-SQL | Microsoft Docs'
description: T-SQL을 사용하여 중요도가 높은 워크로드 분류자를 만듭니다.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: workload management
ms.date: 05/01/2019
ms.author: rortloff
ms.reviewer: jrasnick
ms.openlocfilehash: 9fd11425d22daa87ec5bf2191a5864251ef86f77
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66241245"
---
# <a name="quickstart-create-a-workload-classifier-using-t-sql"></a>빠른 시작: T-SQL을 사용하여 워크로드 분류자 만들기

이 빠른 시작에서는 조직의 CEO를 위해 중요도가 높은 워크로드 분류자를 신속하게 만듭니다. 워크로드 분류자를 사용하면 CEO 쿼리가 대기열에서 중요도가 낮은 다른 쿼리보다 우선 적용되도록 할 수 있습니다.

Azure 구독이 아직 없는 경우 시작하기 전에 [체험](https://azure.microsoft.com/free/) 계정을 만듭니다.

> [!NOTE]
> SQL Data Warehouse를 만들면 새로운 유료 서비스가 발생할 수 있습니다.  자세한 내용은 [SQL Data Warehouse 가격](https://azure.microsoft.com/pricing/details/sql-data-warehouse/)을 참조하세요.
>
>

## <a name="prerequisites"></a>필수 조건

이 빠른 시작에서는 SQL 데이터 웨어하우스가 이미 있고 CONTROL DATABASE 권한이 있다고 가정합니다. 하나 만들어야 할 경우 [만들기 및 연결 - 포털](create-data-warehouse-portal.md)을 사용하여 **mySampleDataWarehouse**라는 데이터 웨어하우스를 만듭니다.

## <a name="sign-in-to-the-azure-portal"></a>Azure Portal에 로그인

[Azure Portal](https://portal.azure.com/)에 로그인합니다.

## <a name="create-login-for-theceo"></a>TheCEO에 대한 로그인 만들기

'TheCEO'에 [CREATE LOGIN](/sql/t-sql/statements/create-login-transact-sql)을 사용하여 `master` 데이터베이스에 SQL Server 인증 로그인을 만듭니다.

```sql
IF NOT EXISTS (SELECT * FROM sys.sql_logins WHERE name = 'TheCEO')
BEGIN
CREATE LOGIN [TheCEO] WITH PASSWORD='<strongpassword>'
END
;
```

## <a name="create-user"></a>사용자 만들기

mySampleDataWarehouse에 "TheCEO"라는 [사용자 만들기](/sql/t-sql/statements/create-user-transact-sql?view=azure-sqldw-latest)

```sql
IF NOT EXISTS (SELECT * FROM sys.database_principals WHERE name = 'THECEO')
BEGIN
CREATE USER [TheCEO] FOR LOGIN [TheCEO]
END
;
```

## <a name="create-a-workload-classifier"></a>워크로드 분류자 만들기

중요도가 높은 "TheCEO"에 대한 [워크로드 분류자](/sql/t-sql/statements/create-workload-classifier-transact-sql?view=azure-sqldw-latest) 만들기

```sql
DROP WORKLOAD CLASSIFIER [wgcTheCEO];
CREATE WORKLOAD CLASSIFIER [wgcTheCEO]
WITH (WORKLOAD_GROUP = 'xlargerc'
      ,MEMBERNAME = 'TheCEO'
      ,IMPORTANCE = HIGH);
```

## <a name="view-existing-classifiers"></a>기존 분류자 보기

```sql
SELECT * FROM sys.workload_management_workload_classifiers
```

## <a name="clean-up-resources"></a>리소스 정리

```sql
DROP WORKLOAD CLASSIFIER [wgcTheCEO]
DROP USER [TheCEO]
;
```

데이터 웨어하우스 단위 및 데이터 웨어하우스에 저장된 데이터에 대해 요금이 청구됩니다. 이러한 계산 및 스토리지 리소스에 대한 요금이 별도로 청구됩니다.

- 데이터를 스토리지에 보관하려는 경우 데이터 웨어하우스를 사용하지 않을 때 계산을 일시 중지할 수 있습니다. 컴퓨팅을 일시 중지하면 데이터 스토리지 비용만 부과됩니다. 데이터로 작업할 준비가 되면 컴퓨팅을 다시 시작합니다.
- 앞으로 요금이 부과되지 않게 하려면 데이터 웨어하우스를 삭제하면 됩니다.

다음 단계에 따라 리소스를 정리합니다.

1. [Azure Portal](https://portal.azure.com)에 로그인하고 데이터 웨어하우스에서 선택합니다.

    ![리소스 정리](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

2. 컴퓨팅을 일시 중지하려면 **일시 중지** 단추를 선택합니다. 데이터 웨어하우스가 일시 중지되면 **시작** 단추가 표시됩니다.  컴퓨팅을 다시 시작하려면 **시작**을 선택합니다.

3. 컴퓨팅 또는 스토리지에 대한 요금이 청구되지 않도록 데이터 웨어하우스를 제거하려면 **삭제**를 선택합니다.

4. 만든 SQL 서버를 제거하려면 이전 이미지에 있는 **mynewserver-20180430.database.windows.net**을 선택한 다음, **삭제**를 선택합니다.  서버를 삭제하면 해당 서버에 할당된 모든 데이터베이스가 삭제되므로 주의해서 실행해야 합니다.

5. 리소스 그룹을 제거하려면 **myResourceGroup**을 선택한 다음, **리소스 그룹 삭제**를 선택합니다.

## <a name="next-steps"></a>다음 단계

- 이제 작업 분류자가 생성되었습니다. TheCEO로 몇 가지 쿼리를 실행하여 어떻게 수행되는지 확인합니다. 쿼리 및 할당된 중요도를 보려면 [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql)를 참조하세요.
- Azure SQL Data Warehouse 워크로드 관리에 대한 자세한 내용은 [워크로드 중요도](sql-data-warehouse-workload-importance.md)와 [워크로드 분류](sql-data-warehouse-workload-classification.md)를 참조하세요.
- [워크로드 중요도 구성](sql-data-warehouse-how-to-configure-workload-importance.md) 및 [워크로드 관리 모니터링 및 관리](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md) 방법에 대한 문서를 참조하세요.
