---
title: Azure AD 서버 보안 주체(로그인)를 사용하는 Azure SQL Database 관리형 인스턴스 보안 | Microsoft Docs
description: Azure SQL Database에서 관리형 인스턴스를 보호하고 Azure AD 서버 보안 주체(로그인)를 사용하는 기술 및 기능에 대해 알아봅니다.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.topic: tutorial
author: VanMSFT
ms.author: vanto
ms.reviewer: carlrab
manager: craigg
ms.date: 02/20/2019
ms.openlocfilehash: 5d168264cbc392e1ba426707429f47dea70d1ea8
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58882058"
---
# <a name="tutorial-managed-instance-security-in-azure-sql-database-using-azure-ad-server-principals-logins"></a>자습서: Azure SQL Database에서 Azure AD 서버 보안 주체(로그인)를 사용하는 관리형 인스턴스 보안

관리되는 인스턴스는 최신 SQL Server 온-프레미스(Enterprise Edition) 데이터베이스 엔진의 보안 기능을 거의 대부분 제공합니다.

- 격리된 환경에서 액세스 제한
- ID를 요구하는 인증 메커니즘 사용(Azure AD, SQL 인증)
- 역할 기반 멤버 자격 및 권한을 통한 권한 부여 사용
- 보안 기능 사용

이 자습서에서는 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> - 관리되는 인스턴스에 대한 Azure AD(Active Directory) 서버 보안 주체(로그인) 만들기
> - 관리되는 인스턴스의 Azure AD 서버 보안 주체(로그인)에 대한 권한 부여
> - Azure AD 서버 보안 주체(로그인)에서 Azure AD 사용자 만들기
> - Azure AD 사용자에게 권한 할당 및 데이터베이스 보안 관리
> - Azure AD 사용자에 가장 사용
> - Azure AD 사용자에 데이터베이스 간 쿼리 사용
> - 위협 방지, 감사, 데이터 마스킹, 암호화 등의 보안 기능에 대해 알아보기

> [!NOTE]
> 관리되는 인스턴스에 대한 Azure AD 서버 보안 주체(로그인)는 **공개 미리 보기**에 있습니다.

자세한 내용은 [Azure SQL Database 관리형 인스턴스 개요](sql-database-managed-instance-index.yml) 및 [기능](sql-database-managed-instance.md) 문서를 참조하세요.

## <a name="prerequisites"></a>필수 조건

이 자습서를 완료하려면 다음 필수 조건이 충족되어야 합니다.

- [SSMS(SQL Server Management Studio)](/sql/ssms/download-sql-server-management-studio-ssms)
- Azure SQL Database 관리형 인스턴스
  - 다음 문서를 수행합니다. [빠른 시작: Azure SQL Database 관리형 인스턴스 만들기](sql-database-managed-instance-get-started.md)
- 관리되는 인스턴스에 액세스하여 [관리되는 인스턴스에 대한 Azure AD 관리자를 프로비저닝](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-managed-instance)할 수 있습니다. 자세한 내용은 다음을 참조하세요.
    - [애플리케이션을 관리되는 인스턴스에 연결](sql-database-managed-instance-connect-app.md) 
    - [관리되는 인스턴스 연결 아키텍처](sql-database-managed-instance-connectivity-architecture.md)
    - [SQL을 사용하여 Azure Active Directory 인증 구성 및 관리](sql-database-aad-authentication-configure.md)

## <a name="limiting-access-to-your-managed-instance"></a>관리되는 인스턴스에 대한 액세스 제한

관리형 인스턴스는 개인 IP 주소를 통해서만 액세스할 수 있습니다. 관리되는 인스턴스 네트워크 외부에서 관리되는 인스턴스에 연결하는 데 사용할 수 있는 서비스 엔드포인트는 없습니다. 격리된 SQL Server 온-프레미스 환경과 비슷하게 연결을 설정하려면 애플리케이션 또는 사용자가 관리형 인스턴스 네트워크(VNet)에 액세스할 수 있어야 합니다. 자세한 내용은 [애플리케이션을 관리되는 인스턴스에 연결](sql-database-managed-instance-connect-app.md) 문서를 참조하세요.

> [!NOTE] 
> 관리되는 인스턴스는 내부 VNET에만 액세스할 수 있으므로 [SQL Database 방화벽 규칙](sql-database-firewall-configure.md)이 적용되지 않습니다. 관리되는 인스턴스는 자체적인 [기본 방화벽](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md)을 갖추고 있습니다.

## <a name="create-an-azure-ad-server-principal-login-for-a-managed-instance-using-ssms"></a>SSMS를 사용하여 관리되는 인스턴스에 대한 Azure AD 서버 보안 주체(로그인) 만들기

첫 번째 Azure AD 서버 보안 주체(로그인)는 `sysadmin`인 표준 SQL Server 계정(비 Azure AD)에서 만들어야 합니다. 관리되는 인스턴스에 연결하는 예제는 다음 문서를 참조하세요.

- [빠른 시작: 관리되는 인스턴스에 연결하도록 Azure VM 구성](sql-database-managed-instance-configure-vm.md)
- [빠른 시작: 온-프레미스에서 관리되는 인스턴스로의 지점 및 사이트 간 연결 구성](sql-database-managed-instance-configure-p2s.md)

> [!IMPORTANT]
> 관리되는 인스턴스를 설정하는 데 사용된 Azure AD 관리자는 관리되는 인스턴스 내에 Azure AD 서버 보안 주체(로그인)를 만드는 데 사용할 수 없습니다. `sysadmin` SQL Server 계정을 사용하여 첫 번째 Azure AD 서버 보안 주체(로그인)를 만들어야 합니다. 이는 Azure AD 서버 보안 주체(로그인)가 GA(일반 공급)되면 제거될 임시적인 제한입니다. Azure AD 관리자 계정을 사용하여 로그인을 만들려고 시도하면 `Msg 15247, Level 16, State 1, Line 1 User does not have permission to perform this action.` 오류가 발생합니다.

1. 표준 SQL Server 계정(비 azure AD)인 `sysadmin`을 사용하여 [SQL Server Management Studio](sql-database-managed-instance-configure-p2s.md#use-ssms-to-connect-to-the-managed-instance)를 통해 관리되는 인스턴스에 로그인합니다.

2. **개체 탐색기**에서 서버를 마우스 오른쪽 단추로 클릭하고 **새 쿼리**를 선택합니다.

3. 쿼리 창에서 다음 구문을 사용하여 로컬 Azure AD 계정에 대한 로그인을 만듭니다.

    ```sql
    USE master
    GO
    CREATE LOGIN login_name FROM EXTERNAL PROVIDER
    GO
    ```

    이 예제는 nativeuser@aadsqlmi.onmicrosoft.com 계정의 로그인을 만듭니다.

    ```sql
    USE master
    GO
    CREATE LOGIN [nativeuser@aadsqlmi.onmicrosoft.com] FROM EXTERNAL PROVIDER
    GO
    ```

4. 도구 모음에서 **실행**을 선택하여 로그인을 만듭니다.

5. 다음 T-SQL 명령을 실행하여 새로 추가된 로그인을 확인합니다.

    ```sql
    SELECT *  
    FROM sys.server_principals;  
    GO
    ```

    ![native-login.png](media/sql-database-managed-instance-security-tutorial/native-login.png)

자세한 내용은 [로그인 만들기](/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current)를 참조하세요.

## <a name="granting-permissions-to-allow-the-creation-of-managed-instance-logins"></a>관리되는 인스턴스 로그인을 만들 수 있도록 권한 부여

다른 Azure AD 서버 보안 주체(로그인)를 만들려면 SQL Server 역할 또는 권한을 보안 주체(SQL 또는 Azure AD)에 부여해야 합니다.

### <a name="sql-authentication"></a>SQL 인증

- 로그인이 SQL 보안 주체인 경우 `sysadmin` 역할에 포함된 로그인만 create 명령을 사용하여 Azure AD 계정에 대한 로그인을 만들 수 있습니다.

### <a name="azure-ad-authentication"></a>Azure AD 인증

- 새로 만든 Azure AD 서버 보안 주체(로그인)에서 다른 Azure AD 사용자, 그룹 또는 애플리케이션에 대한 다른 로그인을 만들 수 있도록 하려면 로그인 `sysadmin` 또는 `securityadmin` 서버 역할을 부여합니다. 
- 다른 Azure AD 서버 보안 주체(로그인)를 만들려면 적어도 **ALTER ANY LOGIN** 권한을 Azure AD 서버 보안 주체(로그인)에 부여해야 합니다. 
- 마스터에 새로 만든 Azure AD 서버 보안 주체(로그인)에 부여된 표준 권한은 기본적으로 **CONNECT SQL** 및 **VIEW ANY DATABASE**가 부여됩니다.
- `sysadmin` 서버 역할은 관리되는 인스턴스 내의 여러 Azure AD 서버 보안 주체(로그인)에 부여할 수 있습니다.

`sysadmin` 서버 역할에 로그인을 추가하려면:

1. 관리되는 인스턴스에 다시 로그인하거나 `sysadmin`인 SQL 보안 주체와의 기존 연결을 사용합니다.

1. **개체 탐색기**에서 서버를 마우스 오른쪽 단추로 클릭하고 **새 쿼리**를 선택합니다.

1. 다음 T-SQL 구문을 사용하여 `sysadmin` 서버 역할을 Azure AD 서버 보안 주체(로그인)에 부여합니다.

    ```sql
    ALTER SERVER ROLE sysadmin ADD MEMBER login_name
    GO
    ```

    다음 예에서는 로그인 nativeuser@aadsqlmi.onmicrosoft.com에 `sysadmin` 서버 역할을 부여합니다.

    ```sql
    ALTER SERVER ROLE sysadmin ADD MEMBER [nativeuser@aadsqlmi.onmicrosoft.com]
    GO
    ```

## <a name="create-additional-azure-ad-server-principals-logins-using-ssms"></a>SSMS를 사용하여 추가 Azure AD 서버 보안 주체(로그인) 만들기

Azure AD 서버 보안 주체(로그인)가 만들어지고 `sysadmin` 권한이 부여되면, 해당 로그인에서 **CREATE LOGIN**과 함께 **FROM EXTERNAL PROVIDER** 절을 사용하여 추가 로그인을 만들 수 있습니다.

1. SQL Server Management Studio를 사용하여 Azure AD 서버 보안 주체(로그인)로 관리되는 인스턴스에 연결합니다. 관리되는 인스턴스 호스트 이름을 입력합니다. SSMS 인증의 경우 Azure AD 계정으로 로그인할 때 다음 세 가지 옵션 중에서 선택할 수 있습니다.

   - Active Directory - MFA 지원을 통한 유니버설 인증
   - Active Directory - 암호
   - Active Directory - 통합 </br>

     ![ssms-login-prompt.png](media/sql-database-managed-instance-security-tutorial/ssms-login-prompt.png)

     자세한 내용은 다음 문서를 참조하세요. [SQL Database 및 SQL Data Warehouse에 대한 유니버설 인증(MFA에 대한 SSMS 지원)](sql-database-ssms-mfa-authentication.md)

1. **Active Directory - MFA 지원을 통한 유니버설 인증**을 선택합니다. 그러면 MFA(Multi-Factor Authentication) 로그인 창이 열립니다. Azure AD 암호를 사용하여 로그인합니다.

    ![mfa-login-prompt.png](media/sql-database-managed-instance-security-tutorial/mfa-login-prompt.png)

1. SSMS **개체 탐색기**에서 서버를 마우스 오른쪽 단추로 클릭하고 **새 쿼리**를 선택합니다.
1. 쿼리 창에서 다음 구문을 사용하여 다른 Azure AD 계정에 대한 로그인을 만듭니다.

    ```sql
    USE master
    GO
    CREATE LOGIN login_name FROM EXTERNAL PROVIDER
    GO
    ```

    이 예제에서는 Azure AD 사용자 bob@aadsqlmi.net에 대한 로그인을 만들며, 이 사용자의 도메인 aadsqlmi.net은 Azure AD aadsqlmi.onmicrosoft.com과 페더레이션됩니다.

    다음 T-SQL 명령을 실행합니다. 페더레이션된 Azure AD 계정은 온-프레미스 Windows 로그인 및 사용자의 관리형 인스턴스를 대체합니다.

    ```sql
    USE master
    GO
    CREATE LOGIN [bob@aadsqlmi.net] FROM EXTERNAL PROVIDER
    GO
    ```

1. [CREATE DATABASE](/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-mi-current) 구문을 사용하여 관리되는 인스턴스에 데이터베이스를 만듭니다. 이 데이터베이스는 다음 섹션에서 사용자 로그인을 테스트하는 데 사용됩니다.
    1. **개체 탐색기**에서 서버를 마우스 오른쪽 단추로 클릭하고 **새 쿼리**를 선택합니다.
    1. 쿼리 창에서 다음 구문을 사용하여 **MyMITestDB**라는 데이터베이스를 만듭니다.

        ```sql
        CREATE DATABASE MyMITestDB;
        GO
        ```

1. Azure AD의 그룹에 대한 관리되는 인스턴스 로그인을 만듭니다. 그룹이 Azure AD에 있어야만 관리되는 인스턴스에 로그인을 추가할 수 있습니다. [Azure Active Directory를 사용하여 기본 그룹 만들기 및 멤버 추가](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md)를 참조하세요. _mygroup_이라는 그룹을 만들고 이 그룹에 멤버를 추가합니다.

1. SQL Server Management Studio에서 새 쿼리 창을 엽니다.

    이 예에서는 Azure AD에 _mygroup_이라는 그룹이 있다고 가정합니다. 다음 명령을 실행합니다.

    ```sql
    USE master
    GO
    CREATE LOGIN [mygroup] FROM EXTERNAL PROVIDER
    GO
    ```

1. 테스트 삼아 새로 만든 로그인 또는 그룹을 사용하여 관리되는 인스턴스에 로그인합니다. 관리되는 인스턴스에 대한 새 연결을 열고, 인증할 때 새 로그인을 사용합니다.
1. **개체 탐색기**에서 서버를 마우스 오른쪽 단추로 클릭하고 새 연결을 위한 **새 쿼리**를 선택합니다.
1. 다음 명령을 실행하여 새로 만든 Azure AD 서버 보안 주체(로그인)에 대한 서버 권한을 확인합니다.

      ```sql
      SELECT * FROM sys.fn_my_permissions (NULL, 'DATABASE')
      GO
      ```

> [!NOTE]
> Azure AD 게스트 사용자는 Azure AD 그룹의 일부로 추가되는 경우에만 관리되는 인스턴스 로그인에 지원됩니다. Azure AD 게스트 사용자는 다른 Azure AD에서 관리되는 인스턴스가 속한 Azure AD로 초대되는 계정입니다. 예를 들어 joe@contoso.com(Azure AD 계정) 또는 steve@outlook.com(MSA 계정)을 Azure AD aadsqlmi의 그룹에 추가할 수 있습니다. 사용자가 그룹에 추가되면 **CREATE LOGIN** 구문을 사용하여 그룹의 관리되는 인스턴스 **마스터** 데이터베이스에 로그인을 만들 수 있습니다. 이 그룹의 구성원인 게스트 사용자는 현재 로그인(joe@contoso.com 또는 steve@outlook.com)을 사용하여 관리되는 인스턴스에 연결할 수 있습니다.

## <a name="create-an-azure-ad-user-from-the-azure-ad-server-principal-login-and-give-permissions"></a>Azure AD 서버 보안 주체(로그인)에서 Azure AD 사용자 만들기 및 권한 부여

개별 데이터베이스에 권한 부여는 관리되는 인스턴스가 SQL Server 온-프레미스에서 작동하는 방식과 매우 비슷한 방식으로 작동합니다. 데이터베이스의 기존 로그인에서 사용자를 만들고 해당 데이터베이스에 대한 권한을 부여하거나 데이터베이스 역할에 추가할 수 있습니다.

**MyMITestDB**라는 데이터베이스와 기본 권한만 있는 로그인을 만들었으므로 다음 단계는 해당 로그인에서 사용자를 만드는 것입니다. 지금은 로그인에서 관리되는 인스턴스에 연결하고 모든 데이터베이스를 볼 수 있지만, 데이터베이스와 상호 작용할 수는 없습니다. 기본 권한만 있는 Azure AD 계정으로 로그인하여 새로 만든 데이터베이스를 확장하려고 시도하면 다음 오류가 표시됩니다.

![ssms-db-not-accessible.png](media/sql-database-managed-instance-security-tutorial/ssms-db-not-accessible.png)

데이터베이스 권한 부여에 대한 자세한 내용은 [데이터베이스 엔진 권한 시작](/sql/relational-databases/security/authentication-access/getting-started-with-database-engine-permissions)을 참조하세요.

### <a name="create-an-azure-ad-user-and-create-a-sample-table"></a>Azure AD 사용자를 만들고 샘플 테이블 만들기

1. SQL Server Management Studio를 사용하여 `sysadmin` 계정으로 관리되는 인스턴스에 로그인합니다.
1. **개체 탐색기**에서 서버를 마우스 오른쪽 단추로 클릭하고 **새 쿼리**를 선택합니다.
1. 쿼리 창에서 다음 구문을 사용하여 Azure AD 서버 보안 주체(로그인)에서 Azure AD 사용자를 만듭니다.

    ```sql
    USE <Database Name> -- provide your database name
    GO
    CREATE USER user_name FROM LOGIN login_name
    GO
    ```

    다음 예제는 로그인 bob@aadsqlmi.net:에서 사용자 bob@aadsqlmi.net을 만듭니다.

    ```sql
    USE MyMITestDB
    GO
    CREATE USER [bob@aadsqlmi.net] FROM LOGIN [bob@aadsqlmi.net]
    GO
    ```

1. 그룹인 Azure AD 서버 보안 주체(로그인)에서 Azure AD 사용자를 만드는 것도 지원됩니다.

    다음 예제에서는 Azure AD 그룹에 있는 Azure AD 그룹 _mygroup_에 대한 로그인을 만듭니다.

    ```sql
    USE MyMITestDB
    GO
    CREATE USER [mygroup] FROM LOGIN [mygroup]
    GO
    ```

    **mygroup**에 속한 모든 사용자는 **MyMITestDB** 데이터베이스에 액세스할 수 있습니다.

    > [!IMPORTANT]
    > Azure AD 서버 보안 주체(로그인)에서 **USER**를 만들 때 **LOGIN**에서 user_name을 동일한 login_name으로 지정합니다.

    자세한 내용은 [사용자 만들기](/sql/t-sql/statements/create-user-transact-sql?view=azuresqldb-mi-current)를 참조하세요.

1. 새 쿼리 창에서 다음 T-SQL 명령을 사용하여 테스트 테이블을 만듭니다.

    ```sql
    USE MyMITestDB
    GO
    CREATE TABLE TestTable
    (
    AccountNum varchar(10),
    City varchar(255),
    Name varchar(255),
    State varchar(2)
    );
    ```

1. 생성된 사용자를 사용하여 SSMS에서 연결을 만듭니다. 앞에서 `sysadmin`으로 만든 **TestTable** 테이블이 보이지 않을 것입니다. 사용자에게 데이터베이스의 데이터를 읽을 수 있는 권한을 제공해야 합니다.

1. 다음 명령을 실행하여 사용자가 현재 갖고 있는 권한을 확인할 수 있습니다.

    ```sql
    SELECT * FROM sys.fn_my_permissions('MyMITestDB','DATABASE')
    GO
    ```

### <a name="add-users-to-database-level-roles"></a>데이터베이스 수준 역할에 사용자 추가

사용자가 데이터베이스의 데이터를 볼 수 있도록 사용자에게 [데이터베이스 수준 역할](/sql/relational-databases/security/authentication-access/database-level-roles)을 제공할 수 있습니다.

1. SQL Server Management Studio를 사용하여 `sysadmin` 계정으로 관리되는 인스턴스에 로그인합니다.

1. **개체 탐색기**에서 서버를 마우스 오른쪽 단추로 클릭하고 **새 쿼리**를 선택합니다.

1. 다음 T-SQL 구문을 사용하여 Azure AD 사용자에게 `db_datareader` 데이터베이스 역할을 부여합니다.

    ```sql
    Use <Database Name> -- provide your database name
    ALTER ROLE db_datareader ADD MEMBER user_name
    GO
    ```

    다음 예제에서는 사용자 bob@aadsqlmi.net 및 그룹 _mygroup_에 **MyMITestDB** 데이터베이스에 대한 `db_datareader` 권한을 제공합니다.

    ```sql
    USE MyMITestDB
    GO
    ALTER ROLE db_datareader ADD MEMBER [bob@aadsqlmi.net]
    GO
    ALTER ROLE db_datareader ADD MEMBER [mygroup]
    GO
    ```

1. 다음 명령을 실행하여 데이터베이스에 만든 Azure AD 사용자가 있는지 확인합니다.

    ```sql
    SELECT * FROM sys.database_principals
    GO
    ```

1. `db_datareader` 역할에 추가된 사용자로 관리되는 인스턴스에 대한 새 연결을 만듭니다.
1. **개체 탐색기**에서 데이터베이스를 확장하여 테이블을 봅니다.

    ![ssms-test-table.png](media/sql-database-managed-instance-security-tutorial/ssms-test-table.png)

1. 새 쿼리 창을 열고 다음 SELECT 문을 실행합니다.

    ```sql
    SELECT *
    FROM TestTable
    ```

    테이블의 데이터를 볼 수 있습니까? 열이 반환되는 것을 볼 수 있습니다.

    ![ssms-test-table-query.png](media/sql-database-managed-instance-security-tutorial/ssms-test-table-query.png)

## <a name="impersonating-azure-ad-server-level-principals-logins"></a>Azure AD 서버 수준 보안 주체(로그인) 가장

관리되는 인스턴스는 Azure AD 서버 수준 보안 주체(로그인)의 가장을 지원합니다.

### <a name="test-impersonation"></a>가장 테스트

1. SQL Server Management Studio를 사용하여 `sysadmin` 계정으로 관리되는 인스턴스에 로그인합니다.

1. **개체 탐색기**에서 서버를 마우스 오른쪽 단추로 클릭하고 **새 쿼리**를 선택합니다.

1. 쿼리 창에서 다음 명령을 사용하여 새 저장 프로시저를 만듭니다.

    ```sql
    USE MyMITestDB
    GO  
    CREATE PROCEDURE dbo.usp_Demo  
    WITH EXECUTE AS 'bob@aadsqlmi.net'  
    AS  
    SELECT user_name();  
    GO
    ```

1. 다음 명령을 사용하여 저장 프로시저를 실행할 때 가장하는 사용자가 **bob\@aadsqlmi.net**인지 확인합니다.

    ```sql
    Exec dbo.usp_Demo
    ```

1. EXECUTE AS LOGIN 문을 사용하여 가장을 테스트합니다.

    ```sql
    EXECUTE AS LOGIN = 'bob@aadsqlmi.net'
    GO
    SELECT SUSER_SNAME()
    REVERT
    GO
    ```

> [!NOTE]
> `sysadmin` 역할에 포함된 SQL 서버 수준 보안 주체(로그인)만이 Azure AD 보안 주체를 대상으로 하는 다음 작업을 실행할 수 있습니다. 
> - EXECUTE AS USER
> - EXECUTE AS LOGIN

## <a name="using-cross-database-queries-in-managed-instances"></a>관리되는 인스턴스에서 데이터베이스 간 쿼리 사용

Azure AD 서버 보안 주체(로그인)를 사용하는 Azure AD 계정에는 데이터베이스 간 쿼리가 지원됩니다. Azure AD 그룹을 사용하여 데이터베이스 간 쿼리를 테스트하려면 다른 데이터베이스 및 테이블을 만들어야 합니다. 다른 데이터베이스 및 테이블이 있으면 만들기를 건너뛰어도 됩니다.

1. SQL Server Management Studio를 사용하여 `sysadmin` 계정으로 관리되는 인스턴스에 로그인합니다.
1. **개체 탐색기**에서 서버를 마우스 오른쪽 단추로 클릭하고 **새 쿼리**를 선택합니다.
1. 쿼리 창에서 다음 명령을 사용하여 **MyMITestDB2**라는 데이터베이스와 **TestTable2**라는 테이블을 만듭니다.

    ```sql
    CREATE DATABASE MyMITestDB2;
    GO
    USE MyMITestDB2
    GO
    CREATE TABLE TestTable2
    (
    EmpId varchar(10),
    FirstName varchar(255),
    LastName varchar(255),
    Status varchar(10)
    );
    ```

1. 새 쿼리 창에서 다음 명령을 실행하여 새 데이터베이스 **MyMITestDB2**에 사용자 _mygroup_을 만들고, 해당 데이터베이스에 대한 SELECT 권한을 _mygroup_에 부여합니다.

    ```sql
    USE MyMITestDB2
    GO
    CREATE USER [mygroup] FROM LOGIN [mygroup]
    GO
    GRANT SELECT TO [mygroup]
    GO
    ```

1. SQL Server Management Studio를 사용하여 Azure AD 그룹 _mygroup_의 구성원으로 관리되는 인스턴스에 로그인합니다. 새 쿼리 창을 열고 다음 데이터베이스 간 SELECT 문을 실행합니다.

    ```sql
    USE MyMITestDB
    SELECT * FROM MyMITestDB2..TestTable2
    GO
    ```

    **TestTable2**의 테이블 결과가 보일 것입니다.

## <a name="additional-scenarios-supported-for-azure-ad-server-principals-logins-public-preview"></a>Azure AD 서버 보안 주체(로그인)에 지원되는 추가 시나리오(공개 미리 보기) 

- SQL 에이전트 관리 및 작업 실행이 Azure AD 서버 보안 주체(로그인)에서 지원됩니다.
- 데이터베이스 백업 및 복원 작업은 Azure AD 서버 보안 주체(로그인)에서 실행될 수 있습니다.
- Azure AD 서버 보안 주체(로그인) 및 인증 이벤트와 관련된 모든 명령문에 대한 [감사](sql-database-managed-instance-auditing.md) 기능이 있습니다.
- `sysadmin` 서버 역할의 멤버인 Azure AD 서버 보안 주체(로그인)에 대한 전용 관리자 연결 기능이 있습니다.
- Azure AD 서버 보안 주체(로그인)는 [sqlcmd 유틸리티](/sql/tools/sqlcmd-utility) 및 [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) 도구를 통해 지원됩니다.
- 로그온 트리거는 Azure AD 서버 보안 주체(로그인)에서 발생하는 로그온 이벤트에 지원됩니다.
- Service Broker 및 DB 메일은 Azure AD 서버 보안 주체(로그인)를 사용하여 설정할 수 있습니다.


## <a name="next-steps"></a>다음 단계

### <a name="enable-security-features"></a>보안 기능 사용

다음 [관리되는 인스턴스 기능 보안 기능](sql-database-managed-instance.md#azure-sql-database-security-features) 문서에서 데이터베이스를 보호하는 방법에 대한 전체 목록을 확인하세요. 다음과 같은 보안 기능을 설명합니다.

- [관리되는 인스턴스 감사](sql-database-managed-instance-auditing.md) 
- [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine)
- [위협 감지](sql-database-managed-instance-threat-detection.md) 
- [동적 데이터 마스킹](/sql/relational-databases/security/dynamic-data-masking)
- [행 수준 보안](/sql/relational-databases/security/row-level-security) 
- [TDE(투명한 데이터 암호화)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)

### <a name="managed-instance-capabilities"></a>관리되는 인스턴스 기능

관리되는 인스턴스 기능의 전체 개요는

> [!div class="nextstepaction"]
> [관리되는 인스턴스 기능](sql-database-managed-instance.md)을 참조하세요.
