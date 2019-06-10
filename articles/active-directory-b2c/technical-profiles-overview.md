---
title: Azure Active Directory B2C 사용자 지정 정책의 기술 프로필 정보 | Microsoft Docs
description: Azure Active Directory B2C의 사용자 지정 정책에서 기술 프로필을 사용하는 방법을 알아봅니다.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 80b8969ba657506705db2b1a3bbc5b389d0a992c
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66512446"
---
# <a name="about-technical-profiles-in-azure-active-directory-b2c-custom-policies"></a>Azure Active Directory B2C 사용자 지정 정책의 기술 프로필 정보

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

기술 프로필은 Azure AD(Azure Active Directory) B2C의 사용자 지정 정책을 사용하여 다른 유형의 당사자와 통신하기 위한 기본 제공 메커니즘이 있는 프레임워크를 제공합니다. 기술 프로필은 Azure AD B2C 테넌트와 통신하거나, 사용자를 만들거나, 사용자 프로필을 읽는 데 사용됩니다. 기술 프로필은 사용자와 상호 작용이 가능하도록 자체 어설션될 수 있습니다. 예를 들어, 로그인할 사용자의 자격 증명을 수집한 후 등록 페이지 또는 암호 재설정 페이지를 렌더링합니다. 

## <a name="type-of-technical-profiles"></a>기술 프로필 유형

기술 프로필은 다음 유형의 시나리오를 가능하게 합니다.

- [Azure Active Directory](active-directory-technical-profile.md) - Azure Active Directory B2C 사용자 관리를 지원합니다.
- [JWT 토큰 발급자](jwt-issuer-technical-profile.md) - 다시 신뢰 당사자 애플리케이션으로 반환되는 JWT 토큰을 내보냅니다. 
- **Phone Factor 공급자** - 다단계 인증입니다.
- [OAuth1](oauth1-technical-profile.md) - OAuth 1.0 프로토콜 ID 공급자와 페더레이션됩니다.
- [OAuth2](oauth2-technical-profile.md) - OAuth 2.0 프로토콜 ID 공급자와 페더레이션됩니다.
- [OpenIdConnect](openid-connect-technical-profile.md) - OpenId Connect 프로토콜 ID 공급자와 페더레이션됩니다.
- [클레임 변환](claims-transformation-technical-profile.md) - 출력 클레임 변환을 호출하여 클레임 값을 조작하거나, 클레임의 유효성을 검사하거나, 출력 클레임 집합의 기본값을 설정합니다.
- [Restful 공급자](restful-technical-profile.md) - 사용자 입력 유효성 검사, 사용자 데이터 다양화 또는 기간 업무 애플리케이션과 통합 등의 REST API 서비스 호출입니다.
- [SAML2](saml-technical-profile.md) - SAML 프로토콜 ID 공급자와 페더레이션됩니다.
- [자체 어설션](self-asserted-technical-profile.md) - 사용자와 상호 작용합니다. 예를 들어, 로그인할 사용자의 자격 증명을 수집하고 등록 페이지 또는 암호 재설정을 렌더링합니다.
- **WsFed** - WsFed 프로토콜 ID 공급자와 페더레이션됩니다. 
- [세션 관리](active-directory-b2c-reference-sso-custom.md) - 여러 유형의 세션을 처리합니다. 
- **Application insights**

## <a name="technical-profile-flow"></a>기술 프로필 흐름

모든 유형의 기술 프로필은 동일한 개념을 공유합니다. 입력 클레임을 보내고, 클레임 변환을 실행하고, ID 공급자, REST API 또는 Azure AD 디렉터리 서비스와 같은 구성된 당사자와 통신합니다. 프로세스가 완료되면 기술 프로필이 출력 클레임을 반환하고 출력 클레임 변환을 실행할 수 있습니다. 다음 다이어그램은 기술 프로필에서 참조되는 변환 및 매핑이 처리되는 방법을 보여 줍니다. 기술 프로필이 상호 작용하는 당사자와 관계없이 모든 클레임 변환이 실행된 후 기술 프로필의 출력 클레임은 즉시 클레임 모음에 저장됩니다. 

![기술 프로필 흐름](./media/technical-profiles-overview/technical-profile-idp-saml-flow.png)
 
1. **InputClaimsTransformation** - 모든 입력 [클레임 변환](claimstransformations.md)의 입력 클레임이 클레임 모음에서 선택되고 실행 후 출력 클레임이 다시 클레임 모음에 저장됩니다. 입력 클레임 변환의 출력 클레임이 후속 입력 클레임 변환의 입력 클레임이 될 수 있습니다.
2. **InputClaims** - 클레임이 클레임 모음에서 선택되고 기술 프로필에 사용됩니다. 예를 들어, [자체 어설션 기술 프로필](self-asserted-technical-profile.md)은 입력 클레임을 사용하여 사용자가 제공하는 출력 클레임을 미리 채웁니다. REST API 기술 프로필은 입력 클레임을 사용하여 입력 매개 변수를 REST API 엔드포인트에 보냅니다. Azure Active Directory는 입력 클레임을 고유 식별자로 사용하여 계정을 읽거나, 업데이트하거나, 삭제합니다.
3. **기술 프로필 실행** - 기술 프로필이 클레임을 구성된 당사자와 교환합니다. 예를 들면 다음과 같습니다.
    - 사용자를 ID 공급자로 리디렉션하여 로그인을 완료합니다. 로그인에 성공하면 사용자가 다시 돌아가고 기술 프로필 실행이 계속됩니다.
    - 매개 변수를 InputClaims로 보내고 정보를 다시 OutputClaims로 가져오는 동안 REST API를 호출합니다.
    - 사용자 계정을 만들거나 업데이트합니다.
    - MFA 텍스트 메시지를 보내고 확인합니다.
4. **ValidationTechnicalProfiles** - [자체 어설션 기술 프로필](self-asserted-technical-profile.md)의 경우 입력 [유효성 검사 기술 프로필](validation-technical-profile.md)을 호출할 수 있습니다. 유효성 검사 기술 프로필은 사용자가 프로파일링한 데이터의 유효성을 검사하고 출력 클레임이 있으면 오류 메시지를, 없으면 확인을 반환합니다. 예를 들어, Azure AD B2C는 새 계정을 만들기 전에 사용자가 이미 디렉터리 서비스에 있는지 확인합니다. REST API 기술 프로필을 호출하여 고유한 비즈니스 논리를 추가할 수 있습니다.<p>유효성 검사 기술 프로필의 출력 클레임 범위는 유효성 검사 기술 프로필 및 동일한 기술 프로필에 속한 다른 유효성 검사 기술 프로필을 호출하는 기술 프로필로 제한됩니다. 다음 오케스트레이션 단계에서 출력 클레임을 사용하려면 유효성 검사 기술 프로필을 호출하는 기술 프로필에 출력 클레임을 추가해야 합니다.
5. **OutputClaims** - 클레임이 다시 클레임 모음으로 반환됩니다. 다음 오케스트레이션 단계 또는 출력 클레임 변환에서 해당 클레임을 사용할 수 있습니다.
6. **OutputClaimsTransformations** - 모든 출력 [클레임 변환](claimstransformations.md)의 입력 클레임이 클레임 모음에서 선택됩니다. 이전 단계에서 기술 프로필의 출력 클레임은 출력 클레임 변환의 입력 클레임이 될 수 있습니다. 실행 후 출력 클레임이 다시 청구 모음에 저장됩니다. 출력 클레임 변환의 출력 클레임이 후속 출력 클레임 변환의 입력 클레임이 될 수도 있습니다.
7. **SSO(Single Sign-On) 세션 관리** - [SSO 세션 관리](active-directory-b2c-reference-sso-custom.md)는 사용자가 이미 인증된 후 사용자에 대한 상호 작용을 제어합니다. 예를 들어 관리자는 ID 공급자의 선택이 표시되는지 여부 또는 로컬 계정 세부 정보를 다시 입력해야 하는지 여부를 제어할 수 있습니다.

설정을 변경하거나 새 기능을 추가하기 위해 다른 기술 프로필에서 기술 프로필을 상속할 수 있습니다.  **IncludeTechnicalProfile** 요소는 기술 프로필이 파생된 기본 기술 프로필에 대한 참조입니다.  

예를 들어, **AAD-UserReadUsingAlternativeSecurityId-NoError** 기술 프로필에는 **AAD-UserReadUsingAlternativeSecurityId**가 포함됩니다. 이 기술 프로필은 **RaiseErrorIfClaimsPrincipalDoesNotExist** 메타데이터 항목을 `true`로 설정하고 소셜 계정이 디렉터리에 없는 경우 오류를 발생시킵니다. **AAD-UserReadUsingAlternativeSecurityId-NoError**는 이 동작을 재정의하고 사용자가 없는 경우 오류 메시지를 사용하지 않도록 설정합니다.

```XML
<TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId-NoError">
  <Metadata>
    <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">false</Item>
  </Metadata>
  <IncludeTechnicalProfile ReferenceId="AAD-UserReadUsingAlternativeSecurityId" />
</TechnicalProfile>
``` 

**AAD-UserReadUsingAlternativeSecurityId**에는 `AAD-Common` 기술 프로필이 포함됩니다.

```XML
<TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
  <Metadata>
    <Item Key="Operation">Read</Item>
    <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
    <Item Key="UserMessageIfClaimsPrincipalDoesNotExist">User does not exist. Please sign up before you can sign in.</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="AlternativeSecurityId" PartnerClaimType="alternativeSecurityId" Required="true" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="objectId" />
    <OutputClaim ClaimTypeReferenceId="userPrincipalName" />
    <OutputClaim ClaimTypeReferenceId="displayName" />
    <OutputClaim ClaimTypeReferenceId="otherMails" />
    <OutputClaim ClaimTypeReferenceId="givenName" />
    <OutputClaim ClaimTypeReferenceId="surname" />
  </OutputClaims>
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
</TechnicalProfile>
```

**AAD-UserReadUsingAlternativeSecurityId-NoError** 및 **AAD-UserReadUsingAlternativeSecurityId**는 **AAD-Common** 기술 프로필에 지정되지 않으므로 필수 **프로토콜** 요소를 지정하지 않습니다.

```XML
<TechnicalProfile Id="AAD-Common">
  <DisplayName>Azure Active Directory</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
</TechnicalProfile>
```

기술 프로필은 서로 포함할 수 있는 또 다른 기술 프로필을 포함하거나 상속할 수 있습니다. 수준 수에는 제한이 없습니다. 비즈니스 요구 사항에 따라 사용자 경험은 사용자 소셜 계정이 없는 경우 오류를 발생시키는 **AAD-UserReadUsingAlternativeSecurityId**를 호출하거나 오류를 발생시키지 않는 **AAD-UserReadUsingAlternativeSecurityId-NoError**를 호출할 수 있습니다.











