---
title: Azure Active Directory B2C의 ID 경험 프레임워크 스키마용 문자열 클레임 변환 예제 | Microsoft Docs
description: Azure Active Directory B2C의 ID 경험 프레임워크 스키마용 문자열 클레임 변환의 예제를 제공합니다.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: a06447aaa6579052285e7e2cd93bf40183ed173f
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66512596"
---
# <a name="string-claims-transformations"></a>문자열 클레임 변환

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

이 문서에서는 Azure AD(Active Directory) B2C 내 ID 경험 프레임워크 스키마의 문자열 클레임 변환 사용을 위한 예제를 제공합니다. 자세한 내용은 [ClaimsTransformations](claimstransformations.md)를 참조하세요.

## <a name="assertstringclaimsareequal"></a>AssertStringClaimsAreEqual 

두 클레임을 비교한 다음 지정된 비교 inputClaim1, inputClaim2 및 stringComparison에 따라 두 클레임이 같지 않으면 예외를 throw합니다.

| 항목 | TransformationClaimType | 데이터 형식 | 메모 |
| ---- | ----------------------- | --------- | ----- |
| inputClaim | inputClaim1 | 문자열 | 비교할 첫 번째 클레임의 형식입니다. |
| inputClaim | inputClaim2 | 문자열 | 비교할 두 번째 클레임의 형식입니다. |
| InputParameter | stringComparison | 문자열 | 문자열 비교, 다음 값 중 하나: Ordinal, OrdinalIgnoreCase |

**AssertStringClaimsAreEqual** 클레임 변환은 항상 [자체 어설션된 기술 프로필](self-asserted-technical-profile.md)을 통해 호출되는 [유효성 검사 기술 프로필](validation-technical-profile.md)에서 실행됩니다. **UserMessageIfClaimsTransformationStringsAreNotEqual** 자체 어설션된 기술 프로필 메타데이터는 사용자에게 표시되는 오류 메시지를 제어합니다.

![AssertStringClaimsAreEqual 실행](./media/string-transformations/assert-execution.png)

이 클레임 변환을 사용하면 두 ClaimType의 값이 같은지를 확인할 수 있습니다. 두 값이 같지 않으면 오류 메시지가 throw됩니다. 다음 예제에서는 **strongAuthenticationEmailAddress** ClaimType이 **이메일** ClaimType과 동일한 지 확인합니다. 두 ClaimType이 같지 않으면 오류 메시지가 throw됩니다. 

```XML
<ClaimsTransformation Id="AssertEmailAndStrongAuthenticationEmailAddressAreEqual" TransformationMethod="AssertStringClaimsAreEqual">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="strongAuthenticationEmailAddress" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="stringComparison" DataType="string" Value="ordinalIgnoreCase" />
  </InputParameters>
</ClaimsTransformation>
```


**login-NonInteractive** 유효성 검사 기술 프로필은 **AssertEmailAndStrongAuthenticationEmailAddressAreEqual** 클레임 변환을 호출합니다.
```XML
<TechnicalProfile Id="login-NonInteractive">
  ...
  <OutputClaimsTransformations>
    <OutputClaimsTransformation ReferenceId="AssertEmailAndStrongAuthenticationEmailAddressAreEqual" />
  </OutputClaimsTransformations>
</TechnicalProfile>
```

자체 어설션된 기술 프로필은 유효성 검사 **login-NonInteractive** 기술 프로필을 호출합니다.

```XML
<TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
  <Metadata>
    <Item Key="UserMessageIfClaimsTransformationStringsAreNotEqual">Custom error message the email addresses you provided are not the same.</Item>
  </Metadata>
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="login-NonInteractive" />
  </ValidationTechnicalProfiles>
</TechnicalProfile>
```

### <a name="example"></a>예

- 입력 클레임:
  - **inputClaim1**: someone@contoso.com
  - **inputClaim2**: someone@outlook.com
    - 입력 매개 변수:
  - **stringComparison**:  ordinalIgnoreCase
- 결과: 오류가 throw됨

## <a name="changecase"></a>ChangeCase 

입력한 클레임의 대/소문자를 연산자에 따라 소문자나 대문자로 변경합니다.

| 항목 | TransformationClaimType | 데이터 형식 | 메모 |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim1 | 문자열 | 변경할 ClaimType입니다. |
| InputParameter | toCase | 문자열 | `LOWER` 또는 `UPPER` 값 중 하나입니다. |
| OutputClaim | outputClaim | 문자열 | 이 클레임 변환을 호출하고 나면 생성되는 ClaimType입니다. |

모든 문자열 ClaimType을 소문자나 대문자로 변경하려면 다음 클레임 변환을 사용합니다.  

```XML
<ClaimsTransformation Id="ChangeToLower" TransformationMethod="ChangeCase">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="inputClaim1" />
  </InputClaims>
<InputParameters>
  <InputParameter Id="toCase" DataType="string" Value="LOWER" />
</InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>예

- 입력 클레임:
  - **email**: SomeOne@contoso.com
- 입력 매개 변수:
    - **toCase**: LOWER
- 출력 클레임:
  - **email**: someone@contoso.com

## <a name="createstringclaim"></a>CreateStringClaim 

정책에 제공된 입력 매개 변수에서 문자열 클레임을 만듭니다.

| 항목 | TransformationClaimType | 데이터 형식 | 메모 |
|----- | ----------------------- | --------- | ----- |
| InputParameter | 값 | 문자열 | 설정할 문자열입니다. |
| OutputClaim | createdClaim | 문자열 | 입력 매개 변수에 지정된 값을 사용하여 이 클레임 변환을 호출하고 나면 생성되는 ClaimType입니다. |

문자열 ClaimType 값을 설정하려면 다음 클레임 변환을 사용합니다.

```XML
<ClaimsTransformation Id="CreateTermsOfService" TransformationMethod="CreateStringClaim">
  <InputParameters>
    <InputParameter Id="value" DataType="string" Value="Contoso terms of service..." />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="TOS" TransformationClaimType="createdClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>예

- 입력 매개 변수:
    - **value**: Contoso 서비스 약관...
- 출력 클레임:
    - **createdClaim**: TOS ClaimType은 "Contoso 사용 약관..." 값을 포함합니다.

## <a name="compareclaims"></a>CompareClaims

특정 문자열 클레임이 다른 클레임과 같은지 여부를 확인합니다. 결과는 값이 `true` 또는 `false`인 새 부울 ClaimType입니다.

| 항목 | TransformationClaimType | 데이터 형식 | 메모 |
| ---- | ----------------------- | --------- | ----- |
| inputClaim | inputClaim1 | 문자열 | 비교할 첫 번째 클레임 형식입니다. |
| inputClaim | inputClaim2 | 문자열 | 비교할 두 번째 클레임 형식입니다. |
| InputParameter | operator | 문자열 | 가능한 값은 `EQUAL` 또는 `NOT EQUAL`입니다. |
| InputParameter | ignoreCase | 부울 | 이 비교에서 비교할 문자열의 대/소문자를 무시해야 하는지 여부를 지정합니다. |
| OutputClaim | outputClaim | 부울 | 이 클레임 변환을 호출하고 나면 생성되는 ClaimType입니다. |

특정 클레임이 다른 클레임과 같은지 여부를 확인하려면 다음 클레임 변환을 사용합니다. 예를 들어 다음 클레임 변환에서는 **email** 클레임의 값이 **Verified.Email** 클레임과 같은지를 확인합니다.

```XML
<ClaimsTransformation Id="CheckEmail" TransformationMethod="CompareClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="Email" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="Verified.Email" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="operator" DataType="string" Value="NOT EQUAL" />
    <InputParameter Id="ignoreCase" DataType="string" Value="true" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="SameEmailAddress" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>예

- 입력 클레임:
  - **inputClaim1**: someone@contoso.com
  - **inputClaim2**: someone@outlook.com
- 입력 매개 변수:
    - **operator**:  NOT EQUAL
    - **ignoreCase**: true
- 출력 클레임:
    - **outputClaim**: true

## <a name="compareclaimtovalue"></a>CompareClaimToValue

클레임 값이 입력 매개 변수 값과 같은지 여부를 확인합니다.

| 항목 | TransformationClaimType | 데이터 형식 | 메모 |
| ---- | ----------------------- | --------- | ----- |
| inputClaim | inputClaim1 | 문자열 | 비교할 클레임의 형식입니다. |
| InputParameter | operator | 문자열 | 가능한 값은 `EQUAL` 또는 `NOT EQUAL`입니다. |
| InputParameter | compareTo | 문자열 | 문자열 비교, 다음 값 중 하나: Ordinal, OrdinalIgnoreCase |
| InputParameter | ignoreCase | 부울 | 이 비교에서 비교할 문자열의 대/소문자를 무시해야 하는지 여부를 지정합니다. |
| OutputClaim | outputClaim | 부울 | 이 클레임 변환을 호출하고 나면 생성되는 ClaimType입니다. |

이 클레임 변환을 사용하면 특정 클레임이 지정한 값과 같은지를 확인할 수 있습니다. 예를 들어 다음 클레임 변환에서는 **termsOfUseConsentVersion** 클레임의 값이 `v1`과 같은지를 확인합니다.

```XML
<ClaimsTransformation Id="IsTermsOfUseConsentRequiredForVersion" TransformationMethod="CompareClaimToValue">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="termsOfUseConsentVersion" TransformationClaimType="inputClaim1" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="compareTo" DataType="string" Value="V1" />
    <InputParameter Id="operator" DataType="string" Value="not equal" />
    <InputParameter Id="ignoreCase" DataType="string" Value="true" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="termsOfUseConsentRequired" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>예
- 입력 클레임:
    - **inputClaim1**: v1
- 입력 매개 변수:
    - **compareTo**: V1
    - **operator**: EQUAL 
    - **ignoreCase**: true
- 출력 클레임:
    - **outputClaim**: true

## <a name="createrandomstring"></a>CreateRandomString

난수 생성기를 사용하여 임의 문자열을 만듭니다. 난수 생성기가 `integer` 형식인 경우 필요에 따라 초기값 매개 변수 및 최대 수를 제공할 수 있습니다. 문자열 형식 매개 변수(선택 사항)를 제공하는 경우 해당 매개 변수를 사용하여 출력 서식을 지정할 수 있습니다. base64 매개 변수(선택 사항)는 출력이 base64 인코딩 randomGeneratorType [guid, integer] outputClaim(String)인지 여부를 지정합니다.

| 항목 | TransformationClaimType | 데이터 형식 | 메모 |
| ---- | ----------------------- | --------- | ----- |
| InputParameter | randomGeneratorType | 문자열 | 생성할 임의 값을 지정합니다. `GUID`(Globally Unique ID) 또는 `INTEGER`(숫자)입니다. |
| InputParameter | stringFormat | 문자열 | [선택 사항] 임의 값의 서식을 지정합니다. |
| InputParameter | base64 | 부울 | [선택 사항] 임의 값을 base64로 변환합니다. 문자열 형식을 적용하는 경우에는 문자열 형식 뒤의 값이 base64로 인코딩됩니다. |
| InputParameter | maximumNumber | int | [선택 사항] `INTEGER` randomGeneratorType 전용입니다. 최대 수를 지정합니다. |
| InputParameter | seed  | int | [선택 사항] `INTEGER` randomGeneratorType 전용입니다. 임의 값의 초기값을 지정합니다. 참고: 초기값이 같으면 동일 난수 시퀀스가 생성됩니다. |
| OutputClaim | outputClaim | 문자열 | 이 클레임 변환을 호출하고 나면 생성되는 ClaimType입니다. 임의 값입니다. |

다음 예제에서는 GUID(Global Unique ID)를 생성합니다. 이 클레임은 임의 UPN(사용자 계정 이름)을 만드는 데 사용됩니다.

```XML
<ClaimsTransformation Id="CreateRandomUPNUserName" TransformationMethod="CreateRandomString">
  <InputParameters>
    <InputParameter Id="randomGeneratorType" DataType="string" Value="GUID" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="upnUserName" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```
### <a name="example"></a>예

- 입력 매개 변수:
    - **randomGeneratorType**: GUID
- 출력 클레임: 
    - **outputClaim**: bc8bedd2-aaa3-411e-bdee-2f1810b73dfc

다음 예제에서는 0에서 1000 사이의 임의 정수 값을 생성합니다. 값에는 OTP_{임의 값} 서식이 지정됩니다.

```XML
<ClaimsTransformation Id="SetRandomNumber" TransformationMethod="CreateRandomString">
  <InputParameters>
    <InputParameter Id="randomGeneratorType" DataType="string" Value="INTEGER" />
    <InputParameter Id="maximumNumber" DataType="int" Value="1000" />
    <InputParameter Id="stringFormat" DataType="string" Value="OTP_{0}" />
    <InputParameter Id="base64" DataType="boolean" Value="false" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="randomNumber" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>예

- 입력 매개 변수:
    - **randomGeneratorType**: INTEGER
    - **maximumNumber**: 1000
    - **stringFormat**: OTP_{0}
    - **base64**: false
- 출력 클레임: 
    - **outputClaim**: OTP_853


## <a name="formatstringclaim"></a>FormatStringClaim

입력한 형식 문자열에 따라 클레임 서식을 지정합니다. 이 변환에서는 C# `String.Format` 메서드를 사용합니다.

| 항목 | TransformationClaimType | 데이터 형식 | 메모 |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim |문자열 |문자열 형식 {0} 매개 변수로 사용되는 ClaimType입니다. |
| InputParameter | stringFormat | 문자열 | {0} 매개 변수를 포함하는 문자열 형식입니다. |
| OutputClaim | outputClaim | 문자열 | 이 클레임 변환을 호출하고 나면 생성되는 ClaimType입니다. |

매개 변수 {0} 하나가 포함된 모든 문자열의 서식을 지정하려면 이 클레임 변환을 사용합니다. 다음 예제에서는 **userPrincipalName**을 만듭니다. `Facebook-OAUTH` 등의 모든 소셜 ID 공급자 기술 프로필은 **CreateUserPrincipalName**을 호출하여 **userPrincipalName**을 생성합니다.   

```XML
<ClaimsTransformation Id="CreateUserPrincipalName" TransformationMethod="FormatStringClaim">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="upnUserName" TransformationClaimType="inputClaim" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="stringFormat" DataType="string" Value="cpim_{0}@{RelyingPartyTenantId}" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="userPrincipalName" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>예

- 입력 클레임:
    - **inputClaim**: 5164db16-3eee-4629-bfda-dcc3326790e9
- 입력 매개 변수:
    - **stringFormat**: cpim_{0}@{RelyingPartyTenantId}
- 출력 클레임:
  - **outputClaim**: cpim_5164db16-3eee-4629-bfda-dcc3326790e9@b2cdemo.onmicrosoft.com

## <a name="formatstringmultipleclaims"></a>FormatStringMultipleClaims

입력한 형식 문자열에 따라 두 클레임의 서식을 지정합니다. 이 변환에서는 C# **String.Format** 메서드를 사용합니다.

| 항목 | TransformationClaimType | 데이터 형식 | 메모 |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim |문자열 | 문자열 형식 {0} 매개 변수로 사용되는 ClaimType입니다. |
| InputClaim | inputClaim | 문자열 | 문자열 형식 {1} 매개 변수로 사용되는 ClaimType입니다. |
| InputParameter | stringFormat | 문자열 | {0} 및 {1} 매개 변수를 포함하는 문자열 형식입니다. |
| OutputClaim | outputClaim | 문자열 | 이 클레임 변환을 호출하고 나면 생성되는 ClaimType입니다. |

두 매개 변수({0} 및 {1})가 포함된 모든 문자열의 서식을 지정하려면 이 클레임 변환을 사용합니다. 다음 예제는 지정된 형식으로 **displayName**을 만듭니다.

```XML
<ClaimsTransformation Id="CreateDisplayNameFromFirstNameAndLastName" TransformationMethod="FormatStringMultipleClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="givenName" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="surName" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="stringFormat" DataType="string" Value="{0} {1}" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="displayName" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>예

- 입력 클레임:
    - **inputClaim1**: Joe
    - **inputClaim2**: Fernando
- 입력 매개 변수:
    - **stringFormat**: {0} {1}
- 출력 클레임:
    - **outputClaim**: Joe Fernando

## <a name="getmappedvaluefromlocalizedcollection"></a>GetMappedValueFromLocalizedCollection

클레임 **Restriction** 컬렉션에서 항목을 조회합니다.

| 항목 | TransformationClaimType | 데이터 형식 | 메모 |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | mapFromClaim | 문자열 | **Restriction** 컬렉션이 포함된 **restrictionValueClaim** 클레임에서 조회할 텍스트가 들어 있는 클레임입니다.  |
| OutputClaim | restrictionValueClaim | 문자열 | **Restriction** 컬렉션이 포함된 클레임입니다. 클레임 변환을 호출하고 나면 이 클레임의 값에는 선택한 항목의 값이 포함됩니다. |

다음 예제에서는 오류 키를 기준으로 오류 메시지 설명을 조회합니다. **responseMsg** 클레임은 최종 사용자에게 표시하거나 신뢰 당사자에게 보낼 오류 메시지 컬렉션을 포함합니다.

```XML
<ClaimType Id="responseMsg">
  <DisplayName>Error message: </DisplayName>
  <DataType>string</DataType>
  <UserInputType>Paragraph</UserInputType>
  <Restriction>
    <Enumeration Text="B2C_V1_90001" Value="You cant sign in because you are a minor" />
    <Enumeration Text="B2C_V1_90002" Value="This action can only be performed by gold members" />
    <Enumeration Text="B2C_V1_90003" Value="You have not been enabled for this operation" />
  </Restriction>
</ClaimType>
```
이 클레임 변환 항목의 텍스트를 조회하고 해당 값을 반환합니다. `<LocalizedCollection>`을 사용하여 제한을 지역화하는 경우 클레임 변환은 지역화된 값을 반환합니다.

```XML
<ClaimsTransformation Id="GetResponseMsgMappedToResponseCode" TransformationMethod="GetMappedValueFromLocalizedCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="responseCode" TransformationClaimType="mapFromClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="responseMsg" TransformationClaimType="restrictionValueClaim" />         
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>예

- 입력 클레임:
    - **mapFromClaim**: B2C_V1_90001
- 출력 클레임:
    - **restrictionValueClaim**: You cant sign in because you are a minor.

## <a name="lookupvalue"></a>LookupValue

다른 클레임의 값을 기준으로 값 목록에서 클레임 값을 조회합니다.

| 항목 | TransformationClaimType | 데이터 형식 | 메모 |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputParameterId | 문자열 | 조회 값을 포함하는 클레임입니다. |
| InputParameter | |문자열 | InputParameters 컬렉션입니다. |
| InputParameter | errorOnFailedLookup | 부울 | 조회에서 일치하는 항목이 없으면 오류가 반환되는지 여부를 제어합니다. |
| OutputClaim | inputParameterId | 문자열 | 이 클레임 변환을 호출하고 나면 생성되는 ClaimType입니다. 일치하는 ID의 값입니다. |

다음 예제에서는 inputParameters 컬렉션 중 하나에서 도메인 이름을 조회합니다. 클레임 변환은 식별자에서 도메인 이름을 조회한 다음, 해당 값(애플리케이션 ID)을 반환합니다.

```XML
 <ClaimsTransformation Id="DomainToClientId" TransformationMethod="LookupValue">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="domainName" TransformationClaimType="inputParameterId" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="contoso.com" DataType="string" Value="13c15f79-8fb1-4e29-a6c9-be0d36ff19f1" />
    <InputParameter Id="microsoft.com" DataType="string" Value="0213308f-17cb-4398-b97e-01da7bd4804e" />
    <InputParameter Id="test.com" DataType="string" Value="c7026f88-4299-4cdb-965d-3f166464b8a9" />
    <InputParameter Id="errorOnFailedLookup" DataType="boolean" Value="false" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="domainAppId" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation> 
```

### <a name="example"></a>예

- 입력 클레임:
    - **inputParameterId**: test.com
- 입력 매개 변수:
    - **contoso.com**: 13c15f79-8fb1-4e29-a6c9-be0d36ff19f1
    - **microsoft.com**: 0213308f-17cb-4398-b97e-01da7bd4804e
    - **test.com**: c7026f88-4299-4cdb-965d-3f166464b8a9
    - **errorOnFailedLookup**: false
- 출력 클레임:
    - **outputClaim**:  c7026f88-4299-4cdb-965d-3f166464b8a9

## <a name="nullclaim"></a>NullClaim

지정된 클레임의 값을 정리합니다.

| 항목 | TransformationClaimType | 데이터 형식 | 메모 |
| ---- | ----------------------- | --------- | ----- |
| OutputClaim | claim_to_null | 문자열 | 값을 NULL로 설정할 클레임입니다. |

클레임 속성 모음에서 불필요한 데이터를 제거하려면 이 클레임 변환을 사용합니다. 그러면 세션 쿠키의 크기가 작아집니다. 다음 예제에서는 `TermsOfService` 클레임 형식의 값을 제거합니다.

```XML
<ClaimsTransformation Id="SetTOSToNull" TransformationMethod="NullClaim">
  <OutputClaims>
  <OutputClaim ClaimTypeReferenceId="TermsOfService" TransformationClaimType="claim_to_null" />
  </OutputClaims>
</ClaimsTransformation>
```

- 입력 클레임:
    - **outputClaim**: Welcome to Contoso App. 이 웹 사이트를 계속 검색 및 사용하는 경우 다음 사용 약관을 준수하며 해당 약관에 구속됨에 동의하게 됩니다.
- 출력 클레임:
    - **outputClaim**: NULL

## <a name="parsedomain"></a>ParseDomain

전자 메일 주소의 도메인 부분을 가져옵니다.

| 항목 | TransformationClaimType | 데이터 형식 | 메모 |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | emailAddress | 문자열 | 전자 메일 주소를 포함하는 ClaimType입니다. |
| OutputClaim | 도메인 | 문자열 | 이 클레임 변환을 호출하고 나면 생성되는 ClaimType(도메인)입니다. |

사용자의 @ 기호 뒤에 붙은 도메인 이름을 구문 분석하려면 이 클레임 변환을 사용합니다. 그러면 감사 데이터에서 PII(개인 식별이 가능한 정보)를 제거할 수 있습니다. 다음 클레임 변환은 **email** 클레임에서 도메인 이름을 구문 분석하는 방법을 보여 줍니다.

```XML
<ClaimsTransformation Id="SetDomainName" TransformationMethod="ParseDomain">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="emailAddress" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="domainName" TransformationClaimType="domain" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>예

- 입력 클레임:
  - **emailAddress**: joe@outlook.com
- 출력 클레임:
    - **domain**: outlook.com

## <a name="setclaimsifstringsareequal"></a>SetClaimsIfStringsAreEqual

문자열 클레임 및 `matchTo` 입력 매개 변수가 같은지를 확인하고 `stringMatchMsg` 및 `stringMatchMsgCode` 입력 매개 변수에 있는 값을 사용하여 출력 클레임을 설정하는 동시에 비교 결과 출력 클레임(비교 결과에 따라 `true` 또는 `false`로 설정됨)도 설정합니다.

| 항목 | TransformationClaimType | 데이터 형식 | 메모 |
| ---- | ----------------------- | --------- | ----- |
| inputClaim | inputClaim | 문자열 | 비교할 클레임 형식입니다. |
| InputParameter | matchTo | 문자열 | `inputClaim`과 비교할 문자열입니다. |
| InputParameter | stringComparison | 문자열 | 가능한 값은 `Ordinal` 또는 `OrdinalIgnoreCase`입니다. |
| InputParameter | stringMatchMsg | 문자열 | 문자열이 같으면 설정할 첫 번째 값입니다. |
| InputParameter | stringMatchMsgCode | 문자열 | 문자열이 같으면 설정할 두 번째 값입니다. |
| OutputClaim | outputClaim1 | 문자열 | 문자열이 같으면 이 출력 클레임에는 `stringMatchMsg` 입력 매개 변수의 값이 포함됩니다. |
| OutputClaim | outputClaim2 | 문자열 | 문자열이 같으면 이 출력 클레임에는 `stringMatchMsgCode` 입력 매개 변수의 값이 포함됩니다. |
| OutputClaim | stringCompareResultClaim | 부울 | 비교 결과 출력 클레임 형식입니다. 비교 결과에 따라 `true` 또는 `false`로 설정됩니다. |

이 클레임 변환을 사용하면 특정 클레임이 지정한 값과 같은지를 확인할 수 있습니다. 예를 들어 다음 클레임 변환에서는 **termsOfUseConsentVersion** 클레임의 값이 `v1`과 같은지를 확인합니다. 두 값이 같은 경우 값을 `v2`로 변경합니다. 

```XML
<ClaimsTransformation Id="CheckTheTOS" TransformationMethod="SetClaimsIfStringsAreEqual">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="termsOfUseConsentVersion" TransformationClaimType="inputClaim" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="matchTo" DataType="string" Value="v1" />
    <InputParameter Id="stringComparison" DataType="string" Value="ordinalIgnoreCase" />
    <InputParameter Id="stringMatchMsg" DataType="string" Value="B2C_V1_90005" />
    <InputParameter Id="stringMatchMsgCode" DataType="string" Value="The TOS is upgraded to v2" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="termsOfUseConsentVersion" TransformationClaimType="outputClaim1" />
    <OutputClaim ClaimTypeReferenceId="termsOfUseConsentVersionUpgradeCode" TransformationClaimType="outputClaim2" />
    <OutputClaim ClaimTypeReferenceId="termsOfUseConsentVersionUpgradeResult" TransformationClaimType="stringCompareResultClaim" />
  </OutputClaims>
</ClaimsTransformation>
```
### <a name="example"></a>예

- 입력 클레임:
    - **inputClaim**: v1
- 입력 매개 변수:
    - **matchTo**: V1
    - **stringComparison**: ordinalIgnoreCase 
    - **stringMatchMsg**:  B2C_V1_90005
    - **stringMatchMsgCode**:  The TOS is upgraded to v2
- 출력 클레임:
    - **outputClaim1**: B2C_V1_90005
    - **outputClaim2**: The TOS is upgraded to v2
    - **stringCompareResultClaim**: true

## <a name="setclaimsifstringsmatch"></a>SetClaimsIfStringsMatch

문자열 클레임 및 `matchTo` 입력 매개 변수가 같은지를 확인하고 `outputClaimIfMatched` 입력 매개 변수에 있는 값을 사용하여 출력 클레임을 설정하는 동시에 비교 결과 출력 클레임(비교 결과에 따라 `true` 또는 `false`로 설정됨)도 설정합니다.

| 항목 | TransformationClaimType | 데이터 형식 | 메모 |
| ---- | ----------------------- | --------- | ----- |
| inputClaim | claimToMatch | 문자열 | 비교할 클레임 형식입니다. |
| InputParameter | matchTo | 문자열 | inputClaim과 비교할 문자열입니다. |
| InputParameter | stringComparison | 문자열 | 가능한 값은 `Ordinal` 또는 `OrdinalIgnoreCase`입니다. |
| InputParameter | outputClaimIfMatched | 문자열 | 문자열이 같으면 설정할 값입니다. |
| OutputClaim | outputClaim | 문자열 | 문자열이 같으면 이 출력 클레임에는 `outputClaimIfMatched` 입력 매개 변수의 값이 포함됩니다. 문자열이 일치하지 않는 경우에는 null입니다. |
| OutputClaim | stringCompareResultClaim | 부울 | 비교 결과 출력 클레임 형식입니다. 비교 결과에 따라 `true` 또는 `false`로 설정됩니다. |

예를 들어 다음 클레임 변환에서는 **ageGroup** 클레임의 값이 `Minor`와 같은지를 확인합니다. 두 값이 같으면 값을 `B2C_V1_90001`로 반환합니다. 

```XML
<ClaimsTransformation Id="SetIsMinor" TransformationMethod="SetClaimsIfStringsMatch">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="ageGroup" TransformationClaimType="claimToMatch" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="matchTo" DataType="string" Value="Minor" />
    <InputParameter Id="stringComparison" DataType="string" Value="ordinalIgnoreCase" />
    <InputParameter Id="outputClaimIfMatched" DataType="string" Value="B2C_V1_90001" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isMinor" TransformationClaimType="outputClaim" />
    <OutputClaim ClaimTypeReferenceId="isMinorResponseCode" TransformationClaimType="stringCompareResultClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>예

- 입력 클레임:
    - **claimToMatch**: Minor
- 입력 매개 변수:
    - **matchTo**: Minor
    - **stringComparison**: ordinalIgnoreCase 
    - **outputClaimIfMatched**:  B2C_V1_90001
- 출력 클레임:
    - **isMinorResponseCode**: B2C_V1_90001
    - **isMinor**: true

