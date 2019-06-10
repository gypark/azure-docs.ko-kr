---
title: ContentDefinitions - Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C에서 사용자 지정 정책의 ContentDefinitions 요소를 지정합니다.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: d82785a0f833afb6a9c675fc7022ed19e96c7fc0
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66511307"
---
# <a name="contentdefinitions"></a>ContentDefinitions

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

[자체 어설션된 기술 프로필](self-asserted-technical-profile.md)의 모양과 느낌을 사용자 지정할 수 있습니다. Azure AD(Azure Active Directory) B2C는 고객 브라우저에서 코드를 실행하고 CORS(원본 간 리소스 공유)라는 최신 방법을 사용합니다. 

사용자 인터페이스를 사용자 지정하려면 사용자 지정 HTML 콘텐츠가 포함된 **ContentDefinition** 요소에 URL을 지정합니다. 자체 어설션된 기술 프로필 또는 **OrchestrationStep**에서 해당 콘텐츠 정의 식별자를 가리킵니다. 콘텐츠 정의에는 로드할 지역화된 리소스 목록을 지정하는 **LocalizedResourcesReferences** 요소가 포함될 수 있습니다. Azure AD B2C는 URL에서 로드된 HTML 콘텐츠와 사용자 인터페이스 요소를 병합한 다음, 사용자에게 해당 페이지를 표시합니다.

**ContentDefinitions** 요소는 사용자 경험에 사용할 수 있는 HTML5 템플릿에 대한 URL을 포함합니다. HTML5 페이지 URI는 지정한 사용자 인터페이스 단계에 사용됩니다. 로그인/등록, 암호 재설정, 오류 페이지 등을 예로 들 수 있습니다. HTML5 파일의 LoadUri를 재정의하여 모양과 느낌을 수정할 수 있습니다. 필요에 따라 새 콘텐츠 정의를 만들 수 있습니다. 이 요소는 [Localization](localization.md) 요소에 지정된 지역화 식별자에 대한 지역화된 리소스 참조를 포함할 수 있습니다.

다음 예제는 콘텐츠 정의 식별자 및 지역화된 리소스 정의를 보여 줍니다.

```XML
<ContentDefinition Id="api.localaccountsignup">
  <LoadUri>~/tenant/default/selfAsserted.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:selfasserted:1.1.0</DataUri>
  <Metadata>
    <Item Key="DisplayName">Local account sign up page</Item>
  </Metadata>
  <LoalizedResourcesReferences MergeBehavior="Prepend">
    <LocalizedResourcesReference Language="en" LocalizedResourcesReferenceId="api.localaccountsignup.en" />
    <LocalizedResourcesReference Language="es" LocalizedResourcesReferenceId="api.localaccountsignup.es" />
    ...
```

**LocalAccountSignUpWithLogonEmail** 자체 어설션된 기술 프로필의 메타데이터는 `api.localaccountsignup`으로 설정된 콘텐츠 정의 식별자 **ContentDefinitionReferenceId**를 포함합니다.

```XML
<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
  <DisplayName>Email signup</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ContentDefinitionReferenceId">api.localaccountsignup</Item>
    ...
  </Metadata>
  ...
```


## <a name="contentdefinition"></a>ContentDefinition

**ContentDefinition** 요소에는 다음 특성이 포함됩니다.

| 특성 | 필수 | 설명 |
| --------- | -------- | ----------- |
| Id | 예 | 콘텐츠 정의의 식별자입니다. 값은 이 페이지의 뒷부분에 있는 **콘텐츠 정의 ID** 섹션에서 지정된 값입니다. |

**ContentDefinition** 요소에는 다음 요소가 포함됩니다.

| 요소 | 발생 수 | 설명 |
| ------- | ----------- | ----------- |
| LoadUri | 1:1 | 콘텐츠 정의에 대한 HTML5 페이지의 URL을 포함하는 문자열입니다. |
| RecoveryUri | 0:1 | 콘텐츠 정의와 관련된 오류를 표시하기 위한 HTML 페이지의 URL을 포함하는 문자열입니다. | 
| DataUri | 1:1 | 단계에 대해 호출할 사용자 환경을 제공하는 HTML 파일의 상대 URL을 포함하는 문자열입니다. |  
| Metadata | 1:1 | 콘텐츠 정의에서 사용되는 메타데이터를 포함하는 키/값 쌍의 컬렉션입니다. | 
| LocalizedResourcesReferences | 0:1 | 지역화된 리소스 참조의 컬렉션입니다. 이 요소를 사용하여 사용자 인터페이스 및 클레임 특성의 지역화를 사용자 지정할 수 있습니다. |

### <a name="datauri"></a>DataUri

**DataUri** 요소는 페이지 식별자를 지정하는 데 사용됩니다. Azure AD B2C에서는 페이지 식별자를 사용하여 UI 요소 및 클라이언트 쪽 JavaScript를 로드하고 시작합니다. 값의 형식은 `urn:com:microsoft:aad:b2c:elements:page-name:version`입니다.  다음 표에는 사용할 수 있는 페이지 식별자가 나와 있습니다.

| 값 |   설명 |
| ----- | ----------- |
| `urn:com:microsoft:aad:b2c:elements:globalexception:1.1.0` | 예외 또는 오류가 발생할 때 오류 페이지를 표시합니다. |
| `urn:com:microsoft:aad:b2c:elements:idpselection:1.0.0` | 로그인 중에 사용자가 선택할 수 있는 ID 공급자를 나열합니다. | 
| `urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0` | 메일 주소 또는 사용자 이름을 기반으로 하는 로컬 계정으로 로그인하기 위한 양식을 표시합니다. 이 값은 “로그인 상태 유지 기능” 및 “암호를 잊으셨나요?”도 제공합니다. 링크를 클릭하도록 요청합니다. | 
| `urn:com:microsoft:aad:b2c:elements:unifiedssd:1.0.0` | 메일 주소 또는 사용자 이름을 기반으로 하는 로컬 계정으로 로그인하기 위한 양식을 표시합니다. |
| `urn:com:microsoft:aad:b2c:elements:multifactor:1.1.0` | 등록 또는 로그인 중에 텍스트 또는 음성을 사용하여 전화 번호를 확인합니다. |
| `urn:com:microsoft:aad:b2c:elements:selfasserted:1.1.0` | 사용자가 프로필을 만들거나 업데이트할 수 있는 양식을 표시합니다. | 


### <a name="localizedresourcesreferences"></a>LocalizedResourcesReferences

**LocalizedResourcesReferences** 요소에는 다음 요소가 포함됩니다.

| 요소 | 발생 수 | 설명 |
| ------- | ----------- | ----------- |
| LocalizedResourcesReference | 1:n | 콘텐츠 정의에 대한 지역화된 리소스 참조 목록입니다. | 

**LocalizedResourcesReferences** 요소에는 다음 특성이 포함됩니다.

| 특성 | 필수 | 설명 |
| --------- | -------- | ----------- |
| 언어 | 예 | RFC 5646 - 언어 식별 태그를 기준으로 정책에 대해 지원되는 언어를 포함하는 문자열입니다. |
| LocalizedResourcesReferenceId | 예 | **LocalizedResources** 요소의 식별자입니다. |

다음 예제는 등록 또는 로그인 콘텐츠 정의를 보여 줍니다.

```XML
<ContentDefinition Id="api.signuporsignin">
  <LoadUri>~/tenant/default/unified.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0</DataUri>
  <Metadata>
    <Item Key="DisplayName">Signin and Signup</Item>
  </Metadata>
</ContentDefinition>
```

다음 예제는 영어, 프랑스어 및 스페인어 지역화 참조와 함께 등록 또는 로그인 콘텐츠 정의를 보여 줍니다.

```XML
<ContentDefinition Id="api.signuporsignin">
  <LoadUri>~/tenant/default/unified.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0</DataUri>
  <Metadata>
    <Item Key="DisplayName">Signin and Signup</Item>
  </Metadata>
  <LocalizedResourcesReferences MergeBehavior="Prepend">
    <LocalizedResourcesReference Language="en" LocalizedResourcesReferenceId="api.signuporsignin.en" />
    <LocalizedResourcesReference Language="fr" LocalizedResourcesReferenceId="api.signuporsignin.rf" />
    <LocalizedResourcesReference Language="es" LocalizedResourcesReferenceId="api.signuporsignin.es" />
</LocalizedResourcesReferences>
</ContentDefinition>
```

콘텐츠 정의에 지역화 지원을 추가하는 방법에 대한 자세한 내용은 [지역화](localization.md)를 참조하세요.

## <a name="content-definition-ids"></a>콘텐츠 정의 ID

**ContentDefinition** 요소의 ID 특성은 콘텐츠 정의에 관련된 페이지의 형식을 지정합니다. 이 요소는 사용자 지정 HTML5/CSS 템플릿을 적용할 컨텍스트를 정의합니다. 다음 표에서는 ID 경험 프레임워크가 인식하는 콘텐츠 정의 ID 집합과 이에 관련된 페이지 형식을 설명합니다. 임의 ID로 고유한 콘텐츠 정의를 만들 수 있습니다.

| ID | 기본 템플릿 | 설명 | 
| -- | ---------------- | ----------- |
| **api.error** | [exception.cshtml](https://login.microsoftonline.com/static/tenant/default/exception.cshtml) | **오류 페이지** - 예외 또는 오류가 발생할 때 오류 페이지를 표시합니다. |
| **api.idpselections** | [idpSelector.cshtml](https://login.microsoftonline.com/static/tenant/default/idpSelector.cshtml) | **ID 공급자 선택 페이지** - 로그인 중에 사용자가 선택할 수 있는 ID 공급자를 나열합니다. 이러한 옵션은 일반적으로 엔터프라이즈 ID 공급자, 소셜 ID 공급자(예: Facebook, Google+) 또는 로컬 계정입니다. |
| **api.idpselections.signup** | [idpSelector.cshtml](https://login.microsoftonline.com/static/tenant/default/idpSelector.cshtml) | **등록에 대한 ID 공급자 선택 페이지** -등록 중에 사용자가 선택할 수 있는 ID 공급자를 나열합니다. 이러한 옵션은 일반적으로 엔터프라이즈 ID 공급자, 소셜 ID 공급자(예: Facebook, Google+) 또는 로컬 계정입니다. |
| **api.localaccountpasswordreset** | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **암호를 잊으셨나요? 페이지** - 사용자가 암호 재설정을 시작하기 위해 완성해야 하는 양식을 표시합니다. |
| **api.localaccountsignin** | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **로컬 계정 로그인 페이지** - 메일 주소 또는 사용자 이름을 기반으로 하는 로컬 계정으로 로그인하기 위한 양식을 표시합니다. 양식에는 텍스트 입력 상자 및 암호 입력란이 포함될 수 있습니다. |
| **api.localaccountsignup** | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **로컬 계정 회원가입 페이지** - 메일 주소 또는 사용자 이름을 기반으로 하는 로컬 계정에 대한 등록 양식을 표시합니다. 양식에는 텍스트 입력란, 암호 입력란, 라디오 단추, 단일 선택 드롭다운 상자 및 다중 선택 확인란과 같이 다양한 입력 컨트롤이 포함될 수 있습니다. |
| **api.phonefactor** | [multifactor-1.0.0.cshtml](https://login.microsoftonline.com/static/tenant/default/multifactor-1.0.0.cshtml) | **다단계 인증 페이지** - 등록 또는 로그인 중에 텍스트 또는 음성을 사용하여 전화 번호를 확인합니다. |
| **api.selfasserted** | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **소셜 계정 등록 페이지** - 사용자가 소셜 ID 공급자의 기존 계정을 사용하여 등록할 때 완성해야 하는 양식을 표시합니다. 암호 입력 필드를 제외하고는 위의 소셜 계정 등록 페이지와 비슷합니다. |
| **api.selfasserted.profileupdate** | [updateprofile.html](https://login.microsoftonline.com/static/tenant/default/updateProfile.cshtml) | **프로필 업데이트 페이지** - 사용자가 자신의 프로필을 업데이트하기 위해 액세스할 수 있는 양식을 표시합니다. 암호 입력 필드를 제외하고는 소셜 계정 등록 페이지와 비슷합니다. |
| **api.signuporsignin** | [unified.html](https://login.microsoftonline.com/static/tenant/default/unified.cshtml) | **통합 등록 또는 로그인 페이지** - 사용자 등록 및 로그인 프로세스를 처리합니다. 사용자는 엔터프라이즈 ID 공급자, 소셜 ID 공급자(예: Facebook, Google+) 또는 로컬 계정을 사용할 수 있습니다. |
 
