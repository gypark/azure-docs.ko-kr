---
title: v1.0 애플리케이션에 대한 범위(Microsoft Authentication Library) | Azure
description: MSAL(Microsoft 인증 라이브러리)을 사용하여 v1.0 애플리케이션에 대한 범위를 알아봅니다.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/23/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 44aad6b2fab7e0ab2ff11d8469782b001b1f4d18
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65545887"
---
# <a name="scopes-for-a-web-api-accepting-v10-tokens"></a>v1.0 토큰을 허용하는 Web API에 대한 범위

OAuth2 권한은 개발자용 Azure AD(v1.0) 웹 API(리소스) 애플리케이션에서 클라이언트 애플리케이션에 공개하는 권한 범위입니다. 이러한 권한 범위를 동의를 통해 클라이언트 애플리케이션에 부여할 수 있습니다. [Azure Active Directory 애플리케이션 매니페스트 참조](reference-app-manifest.md#manifest-reference)에서 `oauth2Permissions` 관련 섹션을 참조하세요.

## <a name="scopes-to-request-access-to-specific-oauth2-permissions-of-a-v10-application"></a>v1.0 애플리케이션의 특정 OAuth2 권한에 대한 액세스를 요청하는 범위
v1.0 애플리케이션의 특정 범위(예: Azure AD 그래프 - https://graph.windows.net)에 대한 토큰을 획득하려면 원하는 리소스 식별자를 해당 리소스에 대해 원하는 OAuth2 권한과 연결하여 범위를 만들어야 합니다.

예를 들어 앱 ID URI가 `ResourceId`인 v1.0 web API에 사용자를 대신하여 액세스하려면 다음과 같습니다.

```csharp
var scopes = new [] {  ResourceId+"/user_impersonation"};
```

```javascript
var scopes = [ ResourceId + "/user_impersonation"];
```

Azure AD 그래프 API(https://graph.windows.net/)를 사용하여 MSAL.NET Azure Active Directory에서 읽고 쓰려면 다음과 같은 범위 목록을 만듭니다.

```csharp
string ResourceId = "https://graph.windows.net/";
var scopes = new [] { ResourceId + "Directory.Read", ResourceID + "Directory.Write"}
```

```javascript
var ResourceId = "https://graph.windows.net/";
var scopes = [ ResourceId + "Directory.Read", ResourceID + "Directory.Write"];
```

Azure Resource Manager API(https://management.core.windows.net/)에 해당하는 범위를 작성하려면 다음 범위를 요청해야 합니다(이중 슬래시에 주의).

```csharp
var scopes = new[] {"https://management.core.windows.net//user_impersonation"};
var result = await app.AcquireTokenInteractive(scopes).ExecuteAsync();

// then call the API: https://management.azure.com/subscriptions?api-version=2016-09-01
```

> [!NOTE]
> Azure Resource Manager API에는 해당 대상 그룹 클레임(aud)에 슬래시가 필요하고, 범위에서 API 이름을 분리하기 위한 슬래시가 있기 때문에 슬래시를 두 개 사용해야 합니다.

Azure AD에서 사용하는 논리는 다음과 같습니다.

- v1.0 액세스 토큰을 사용하는 ADAL(v1.0) 엔드포인트의 경우(유일하게 가능한 경우) audio=resource
- MSAL(Microsoft ID 플랫폼(v2.0) 엔드포인트)에서 v2.0 토큰을 허용하는 리소스에 대한 액세스 토큰을 요청하는 경우 aud=resource.AppId
- MSAL(v2.0 엔드포인트)에서 v1.0 액세스 토큰을 허용하는 리소스(위의 경우)에 대한 액세스 토큰을 요청하는 경우 Azure AD는 마지막 슬래시 앞의 모든 항목을 가져와서 리소스 식별자로 사용하여 요청된 범위에서 원하는 대상 그룹을 구문 분석합니다. 따라서 https://database.windows.net에서 “https://database.windows.net/”이라는 대상 그룹이 필요한 경우 “https://database.windows.net//.default”라는 범위를 요청해야 합니다. GitHub 문제 [#747: 리소스 URL의 후행 슬래시가 누락되어 sql 인증 실패 문제](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/747)도 참조하세요.

## <a name="scopes-to-request-access-to-all-the-permissions-of-a-v10-application"></a>v1.0 애플리케이션의 모든 권한에 대한 액세스를 요청하는 범위
v1.0 애플리케이션의 모든 정적 범위에 대한 토큰을 획득하려면 “default”를 API의 앱 ID URI에 추가합니다.

```csharp
ResourceId = "someAppIDURI";
var scopes = new [] {  ResourceId+"/.default"};
```

```javascript
var ResourceId = "someAppIDURI";
var scopes = [ ResourceId + "/.default"];
```

## <a name="scopes-to-request-for-client-credential-flow--daemon-app"></a>클라이언트 자격 증명 흐름/디먼 앱의 경우 요청하는 범위
클라이언트 자격 증명 흐름의 경우 전달하는 범위도 `/.default`가 됩니다. 이렇게 하면 관리자가 애플리케이션 등록에서 동의한 모든 앱 수준 권한을 요청한다고 Azure AD에 알려줍니다.