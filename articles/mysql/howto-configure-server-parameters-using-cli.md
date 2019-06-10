---
title: Azure Database for MySQL에서 서비스 매개 변수 구성
description: 이 문서에서는 Azure CLI 명령줄 유틸리티를 사용하여 Azure Database for MySQL에서 서비스 매개 변수를 구성하는 방법을 설명합니다.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 07/18/2018
ms.openlocfilehash: b0d7bbdc3e1dcad6f6cecb57b15e2e5df6b3fd28
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/27/2019
ms.locfileid: "60525452"
---
# <a name="customize-server-configuration-parameters-by-using-azure-cli"></a>Azure CLI를 사용하여 서버 구성 매개 변수 사용자 지정
Azure 명령줄 유틸리티인 Azure CLI를 사용하여 Azure Database for MySQL 서버의 구성 매개 변수를 나열하고, 표시하며, 업데이트할 수 있습니다. 엔진 구성의 하위 집합은 서버 수준에서 노출되고 수정할 수 있습니다. 

## <a name="prerequisites"></a>필수 조건
이 방법 가이드를 단계별로 실행하려면 다음이 필요합니다.
- [Azure Database for MySQL 서버](quickstart-create-mysql-server-database-using-azure-cli.md)
- [Azure CLI](/cli/azure/install-azure-cli) 명령줄 유틸리티 또는 브라우저의 Azure Cloud Shell

## <a name="list-server-configuration-parameters-for-azure-database-for-mysql-server"></a>Azure Database for MySQL에 대한 서버 구성 매개 변수 나열
서버의 수정 가능한 모든 매개 변수와 해당 값을 나열하려면 [az mysql server configuration list](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-list) 명령을 실행합니다.

**myresourcegroup** 리소스 그룹에 있는 **mydemoserver.mysql.database.azure.com** 서버에 대한 서버 구성 매개 변수를 나열할 수 있습니다.
```azurecli-interactive
az mysql server configuration list --resource-group myresourcegroup --server mydemoserver
```
나열된 각 매개 변수의 정의를 보려면 [서버 시스템 변수](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html)에서 MySQL 참조 섹션을 참조하세요.

## <a name="show-server-configuration-parameter-details"></a>서버 구성 매개 변수 세부 정보 표시
서버에 대한 특정 구성 매개 변수의 세부 정보를 표시하려면 [az mysql server configuration show](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-show) 명령을 실행합니다.

이 예제에서는 **myresourcegroup** 리소스 그룹에 있는 **mydemoserver.mysql.database.azure.com** 서버에 대한 **slow\_query\_log** 서버 구성 매개 변수의 세부 정보를 보여 줍니다.
```azurecli-interactive
az mysql server configuration show --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```
## <a name="modify-a-server-configuration-parameter-value"></a>서버 구성 매개 변수 값 수정
특정 서버 구성 매개 변수의 값을 수정할 수 있습니다. 그러면 MySQL 서버 엔진에 대한 기본 구성 값이 업데이트됩니다. 구성 값을 업데이트하려면 [az mysql server configuration set](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-set) 명령을 사용합니다. 

**myresourcegroup** 리소스 그룹에 있는 **mydemoserver.mysql.database.azure.com** 서버에 대한 **slow\_query\_log** 서버 구성 매개 변수를 업데이트하려면
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
```
구성 매개 변수 값을 다시 설정하려는 경우 선택 사항인 `--value` 매개 변수를 생략합니다. 그러면 서비스에서 기본값을 적용합니다. 위의 예제에서는 다음과 같이 표시됩니다.
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```
이 코드는 **slow\_query\_log** 구성을 기본값인 **OFF**로 다시 설정합니다. 

## <a name="working-with-the-time-zone-parameter"></a>표준 시간대 매개 변수 작업

### <a name="populating-the-time-zone-tables"></a>표준 시간대 테이블 채우기

MySQL 명령줄 또는 MySQL Workbench와 같은 도구에서 `az_load_timezone` 저장 프로시저를 호출하면 서버의 표준 시간대 테이블을 채울 수 있습니다.

> [!NOTE]
> MySQL Workbench에서 `az_load_timezone` 명령을 실행하는 경우, 먼저 `SET SQL_SAFE_UPDATES=0;`을 사용하여 안전한 업데이트 모드를 꺼야 할 수 있습니다.

```sql
CALL mysql.az_load_timezone();
```

사용 가능한 표준 시간대 값을 보려면 다음 명령을 실행합니다.

```sql
SELECT name FROM mysql.time_zone_name;
```

### <a name="setting-the-global-level-time-zone"></a>전역 수준 표준 시간대 설정

전역 수준 표준 시간대는 [az mysql server configuration set](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-set) 명령을 사용하여 설정할 수 있습니다.

다음 명령은 리소스 그룹 **myresourcegroup** 아래에 있는 **mydemoserver.mysql.database.azure.com** 서버의 **time\_zone** 서버 구성 매개 변수를 **US/Pacific**으로 업데이트합니다.

```azurecli-interactive
az mysql server configuration set --name time_zone --resource-group myresourcegroup --server mydemoserver --value "US/Pacific"
```

### <a name="setting-the-session-level-time-zone"></a>세션 수준 표준 시간대 설정

세션 수준 표준 시간대는 MySQL 명령줄 또는 MySQL Workbench와 같은 도구에서 `SET time_zone` 명령을 실행하여 설정할 수 있습니다. 아래 예제에서는 표준 시간대를 **US/Pacific** 표준 시간대로 설정합니다.  

```sql
SET time_zone = 'US/Pacific';
```

[날짜 및 시간 함수](https://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html#function_convert-tz)에 대한 MySQL 문서를 참조하세요.


## <a name="next-steps"></a>다음 단계

- [Azure Portal에서 서버 매개 변수](howto-server-parameters.md)를 구성하는 방법