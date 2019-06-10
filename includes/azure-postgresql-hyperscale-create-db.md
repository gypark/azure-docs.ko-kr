---
title: 포함 파일
description: 포함 파일
author: jonels-msft
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: include
ms.date: 05/14/2019
ms.author: jonels
ms.custom: include file
ms.openlocfilehash: c07e352288d7dc1d0bf198fd74c8baaded3a2d23
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66154484"
---
Azure 구독이 아직 없는 경우 시작하기 전에 [체험](https://azure.microsoft.com/free/) 계정을 만듭니다.

## <a name="sign-in-to-the-azure-portal"></a>Azure Portal에 로그인

[Azure Portal](https://portal.azure.com)에 로그인합니다.

## <a name="create-an-azure-database-for-postgresql---hyperscale-citus"></a>Azure Database for PostgreSQL 만들기 – 하이퍼스케일(Citus)

다음 단계에 따라 Azure Database for PostgreSQL 서버를 만듭니다.
1. Azure Portal의 왼쪽 위 모서리에서 **리소스 만들기**를 클릭합니다.
2. **새로 만들기** 페이지에서 **데이터베이스**를 선택하고, **데이터베이스** 페이지에서 **PostgreSQL용 Azure Database**를 선택합니다.
3. 배포 옵션을 보려면 **하이퍼스케일(Citus) 서버 그룹 - 미리 보기** 아래에서 **만들기** 단추를 클릭합니다.
4. 새 서버 세부 정보 양식을 다음 정보로 작성합니다.
   - 리소스 그룹: 이 필드의 텍스트 상자 아래에 있는 **새로 만들기** 링크를 클릭합니다. **myresourcegroup**과 같은 이름을 입력합니다.
   - 서버 그룹 이름: 새 서버 그룹의 고유한 이름을 입력합니다. 이 이름은 서버 하위 도메인에도 사용됩니다.
   - 관리자 사용자 이름: 현재 **citus** 값이 필요하며 변경할 수 없음.
   - 암호: 8자 이상이어야 하며, 영어 대문자, 영어 소문자, 숫자(0-9) 및 영숫자가 아닌 문자(!, $, #, % 등) 중 세 가지 범주의 문자를 포함해야 합니다.
   - 위치: 데이터에 가장 빠르게 액세스할 수 있도록 사용자와 가장 가까운 위치를 사용합니다.

   > [!IMPORTANT]
   > 여기에서 지정하는 서버 관리자 암호는 서버 및 해당 데이터베이스에 로그인하는 데 필요합니다. 나중에 사용하기 위해 이 정보를 기억하거나 기록합니다.

5. **서버 그룹 구성**을 클릭합니다. 이 섹션의 설정을 변경하지 않고 **저장**을 클릭합니다.
6. **검토 + 만들기**를 클릭한 다음, **만들기**를 클릭하여 서버를 프로비저닝합니다. 프로비전하는 데 몇 분이 걸립니다.
7. 배포를 모니터링하도록 페이지가 리디렉션됩니다. 실시간 상태가 **배포 진행 중**에서 **배포가 완료됨**으로 바뀌면 페이지 왼쪽에서 **출력** 메뉴 항목을 클릭합니다.
8. 출력 페이지에는 코디네이터 호스트 이름이 포함되어 있고, 그 옆에는 이 값을 클립보드에 복사하는 단추가 있습니다. 나중에 사용할 수 있도록 이 정보를 기록해 둡니다.

## <a name="configure-a-server-level-firewall-rule"></a>서버 수준 방화벽 규칙 구성

Azure Database for PostgreSQL - 하이퍼스케일(Citus)(미리 보기) 서비스는 서버 수준 방화벽을 사용합니다. 기본적으로 방화벽은 모든 외부 애플리케이션 및 도구가 코디네이터 노드와 그 내부의 데이터베이스에 연결하지 못하게 차단합니다. 특정 IP 주소 범위에 대해서는 방화벽을 여는 규칙을 추가해야 합니다.

1. 이전에 코디네이터 노드 호스트 이름을 복사한 **출력** 섹션에서 **개요** 메뉴 항목을 다시 클릭합니다.

2. 배포의 서버 그룹의 이름을 찾아서 클릭합니다. (서버 그룹 이름에는 접미사가 *없습니다*. 예를 들어, 이름이 “-c”, “-w0” “-w1”로 끝나는 항목은 서버 그룹이 아닙니다.)

3. 왼쪽 메뉴의 **보안** 아래에서 **방화벽**을 클릭합니다.

4. **+ 현재 클라이언트 IP 주소에 대한 방화벽 규칙 추가** 링크를 클릭합니다.

5. 마지막으로, **저장** 단추를 클릭합니다.

   > [!NOTE]
   > Azure PostgreSQL 서버는 5432 포트를 통해 통신합니다. 회사 네트워크 내에서 연결하려는 경우 5432 포트를 통한 아웃바운드 트래픽이 네트워크 방화벽에서 허용되지 않을 수 있습니다. 이 경우 IT 부서에서 포트 5432를 열지 않으면 Azure SQL Database 서버에 연결할 수 없습니다.
   >

## <a name="connect-to-the-database-using-psql"></a>psql을 사용하여 데이터베이스에 연결

Azure Database for PostgreSQL을 만들 때 **citus**라는 기본 데이터베이스가 만들어집니다. 데이터베이스 서버에 연결하려면 연결 문자열 및 관리자 암호가 필요합니다.

1. 연결 문자열을 가져옵니다. 서버 그룹 페이지에서 **연결 문자열** 메뉴 항목을 클릭합니다. (**설정** 아래에 있습니다.) **C++(libpq)** 라고 표시되어 있는 문자열을 찾습니다. 다음과 같은 형식입니다.

   ```
   host=hostname.postgres.database.azure.com port=5432 dbname=citus user=citus password={your_password} sslmode=require
   ```

   문자열을 복사합니다. “{your\_password}”를 이전에 선택한 관리 암호로 바꾸어야 합니다. 시스템은 일반 텍스트 암호를 저장하지 않으므로 문자열에 표시할 수도 없습니다.

2. 로컬 컴퓨터에서 터미널 창을 엽니다.

3. 프롬프트에서 [psql](https://www.postgresql.org/docs/current/app-psql.html) 유틸리티를 사용하여 Azure Database for PostgreSQL 서버에 연결합니다. 따옴표 안에 연결 문자열을 전달합니다. 암호가 포함되어 있는지 확인합니다.
   ```bash
   psql "{connection_string}"
   ```

   예를 들어 다음 명령은 **mydemoserver** 서버 그룹의 코디네이터 노드에 연결합니다.

   ```bash
   psql "host=mydemoserver-c.postgres.database.azure.com port=5432 dbname=citus user=citus password={your_password} sslmode=require"
   ```
