---
title: Azure Active Directory B2C에서 사용자 지정 정책에서 OpenId Connect 기술 프로필을 정의 합니다. | Microsoft Docs
description: Azure Active Directory B2C에서 사용자 지정 정책에서 OpenId Connect 기술 프로필을 정의 합니다.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 6d16415aa5111388ec2d2a1009ff477574ae42c5
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66512909"
---
# <a name="define-an-openid-connect-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C 사용자 지정 정책에서 OpenId Connect 기술 프로필 정의

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure AD(Azure Active Directory) B2C는 [OpenId Connect](https://openid.net/2015/04/17/openid-connect-certification-program/) 프로토콜 ID 공급자를 지원합니다. OpenID Connect 1.0은 OAuth 2.0을 토대로 ID 계층을 정의하고 최신 인증 프로토콜을 나타냅니다. OpenId Connect 기술 프로필을 사용 하 여 Azure AD와 같은 OpenId Connect 기반된 id 공급자를 사용 하 여 페더레이션 할 수 있습니다. 페더레이션 id 공급자를 사용 하 여 기존 소셜 로그인 또는 엔터프라이즈 id 사용자를를 수 있습니다.

## <a name="protocol"></a>프로토콜

**Protocol** 요소의 **Name** 특성은 `OpenIdConnect`로 설정해야 합니다. 예를 들어 **MSA-OIDC** 기술 프로필의 프로토콜은 `OpenIdConnect`입니다.

```XML
<TechnicalProfile Id="MSA-OIDC">
  <DisplayName>Microsoft Account</DisplayName>
  <Protocol Name="OpenIdConnect" />
  ...    
```

## <a name="input-claims"></a>입력 클레임

**InputClaims** 및 **InputClaimsTransformations** 요소는 필요하지 않습니다. 그러나 ID 공급자에게 추가 매개 변수를 보내는 것이 좋습니다. 다음 예제는 값이 `contoso.com`인 **domain_hint** 쿼리 문자열 매개 변수를 권한 부여 요청에 추가합니다.

```XML
<InputClaims>
  <InputClaim ClaimTypeReferenceId="domain_hint" DefaultValue="contoso.com" />
</InputClaims>
```

## <a name="output-claims"></a>출력 클레임

**OutputClaims** 요소는 OpenId Connect ID 공급자가 반환한 클레임 목록을 포함합니다. 정책에 정의된 클레임 이름을 ID 공급자에 정의된 이름에 매핑해야 할 수도 있습니다. `DefaultValue` 특성만 설정하면, ID 공급자가 반환하지 않은 클레임도 포함할 수 있습니다.

**OutputClaimsTransformations** 요소는 출력 클레임을 수정하거나 새 출력 클레임을 생성하는 데 사용되는 **OutputClaimsTransformation** 요소 컬렉션을 포함할 수 있습니다.

다음 예제는 Microsoft 계정 ID 공급자가 반환한 클레임을 보여 줍니다.

- **sub** 매핑되는 클레임을 **issuerUserId** 클레임입니다.
- **displayName** 클레임에 매핑된 **name** 클레임입니다.
- 이름 매핑이 없는 **email**입니다.

기술 프로필은 ID 공급자가 반환하지 않은 클레임도 반환합니다.

- ID 공급자의 이름을 포함하는 **identityProvider** 클레임입니다.
- 기본값이 **socialIdpAuthentication**인 **authenticationSource** 클레임입니다.

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="live.com" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
  <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="sub" />
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
  <OutputClaim ClaimTypeReferenceId="email" />
</OutputClaims>
```

## <a name="metadata"></a>Metadata

| 특성 | 필수 | 설명 |
| --------- | -------- | ----------- |
| client_id | 예 | ID 공급자의 애플리케이션 식별자입니다. |
| IdTokenAudience | 아닙니다. | id_token의 대상 그룹입니다. 지정된 경우 Azure AD B2C는 토큰이 ID 공급자에서 반환된 클레임에 있고 지정된 토큰과 같은지 확인합니다. |
| METADATA | 예 | 잘 알려진 openid 구성 엔드포인트라고도 하는 OpenID Connect Discovery 사양에 따라 서식이 지정된 JSON 구성 문서를 가리키는 URL입니다. |
| ProviderName | 아닙니다. | ID 공급자의 이름입니다. |
| response_types | 아닙니다. | OpenID Connect Core 1.0 사양에 따른 응답 유형입니다. 가능한 값은 `id_token`, `code` 또는 `token`입니다. |
| response_mode | 아닙니다. | ID 공급자가 결과를 다시 Azure AD B2C에 보내는 데 사용하는 방법입니다. 가능한 값은 `query`, `form_post`(기본값) 또는 `fragment`입니다. |
| scope | 아닙니다. | OpenID Connect Core 1.0 사양에 따라 정의 된 요청의 범위입니다. 예를 들어 `openid`, `profile` 및 `email`입니다. |
| HttpBinding | 아닙니다. | 액세스 토큰 및 클레임 토큰 엔드포인트에 필요한 HTTP 바인딩입니다. 가능한 값은 `GET` 또는 `POST`입니다.  |
| ValidTokenIssuerPrefixes | 아닙니다. | Azure Active Directory와 같은 다중 테넌트 ID 공급자를 사용할 때 각 테넌트에 로그인하는 데 사용할 수 있는 키입니다. |
| UsePolicyInRedirectUri | 아닙니다. | 리디렉션 URI를 구성할 때 정책을 사용할지 여부를 나타냅니다. ID 공급자에서 애플리케이션을 구성할 때 리디렉션 URI를 지정해야 합니다. 리디렉션 URI는 Azure AD B2C, `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`(login.microsoftonline.com은 your-tenant-name.b2clogin.com으로 변경될 수 있음)를 가리킵니다.  `false`를 지정하는 경우 사용하는 각 정책에 대해 리디렉션 URI를 추가해야 합니다. 예: `https://login.microsoftonline.com/te/{tenant}/{policy}/oauth2/authresp` |
| MarkAsFailureOnStatusCode5xx | 아닙니다. | HTTP 상태 코드가 5xx 범위에 있는 경우 외부 서비스에 대한 요청을 실패로 표시할지 여부를 나타냅니다. 기본값은 `false`입니다. |
| DiscoverMetadataByTokenIssuer | 아닙니다. | JWT 토큰의 발급자를 사용하여 OIDC 메타데이터를 검색할지 여부를 나타냅니다. |

## <a name="cryptographic-keys"></a>암호화 키

**CryptographicKeys** 요소에는 다음 특성이 포함됩니다.

| 특성 | 필수 | 설명 |
| --------- | -------- | ----------- |
| client_secret | 예 | ID 공급자 애플리케이션의 클라이언트 암호입니다. 암호화 키는 **response_types** 메타데이터가 `code`로 설정된 경우에만 필요합니다. 이 경우 Azure AD B2C는 액세스 토큰에 대한 인증 코드를 교환하는 다른 호출을 수행합니다. 메타데이터가 `id_token`으로 설정된 경우 암호화 키를 생략할 수 있습니다.  |  

## <a name="redirect-uri"></a>리디렉션 URI
 
ID 공급자의 리디렉션 URI를 구성할 때 `https://login.microsoftonline.com/te/tenant/oauth2/authresp`를 입력합니다. 바꿔야 **테 넌 트** 테 넌 트의 이름 (예: contosob2c.onmicrosoft.com) 또는 테 넌 트의 id입니다. 리디렉션 URI는 모두 소문자여야 합니다.

**login.microsoftonline.com** 대신 **b2clogin.com** 도메인을 사용하는 경우 login.microsoftonline.com 대신 b2clogin.com을 사용해야 합니다.

예제:

- [사용자 지정 정책을 사용하여 MSA(Microsoft 계정)를 ID 공급자로 추가](active-directory-b2c-custom-setup-msa-idp.md)
- [Azure AD 계정을 사용하여 로그인](active-directory-b2c-setup-aad-custom.md)
- [사용자 지정 정책을 사용하여 사용자가 다중 테넌트 Azure AD ID 공급자에 로그인하도록 허용](active-directory-b2c-setup-commonaad-custom.md)

 














