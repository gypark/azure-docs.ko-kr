---
title: 사용자 지정 정책-Azure Active Directory B2C에 대 한 개발자 노트 | Microsoft Docs
description: 사용자 지정 정책으로 Azure AD B2C를 구성 및 유지 관리하는 개발자를 위한 정보
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 1d0be4ec2ed8feb308839377e0494ef2f4b78368
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66510205"
---
# <a name="developer-notes-for-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C에서 사용자 지정 정책에 대 한 개발자 노트

Azure Active Directory B2C에서 사용자 지정 정책 구성 이제 일반 공급 됩니다. 구성의이 메서드는 복합 id 솔루션을 작성 하는 고급 id 개발자 대상으로 합니다. 사용자 지정 정책은 Azure AD B2C 테 넌 트에서 사용할 수 있는 Id 경험 프레임 워크의 기능을 확인합니다. 고급 id 개발자가 사용자 지정 정책을 사용 하 여 연습을 완료 하 고 참조 문서를 읽는 시간을 투자할 계획 해야 합니다.

이제 대부분 사용할 수 있는 사용자 지정 정책 옵션의 일반 공급 됩니다, 기술 프로필 형식 및 콘텐츠 정의 소프트웨어 수명 주기의 여러 단계에서 하는 Api와 같은 기본 기능을 가지 있습니다. 더 많은 제공 됩니다. 아래 표에서 더 세부적인 수준에서 가용성 수준을 지정합니다.  

## <a name="features-that-are-generally-available"></a>일반적으로 사용할 수 있는 기능

- 사용자 지정 정책을 사용하여 사용자 지정 인증 사용자 경험을 작성 및 업로드합니다.  
    - 클레임 공급자 간의 교환으로 사용자 경험을 단계별로 설명합니다.  
    - 사용자 경험에서 조건부 분기를 정의합니다.  
- 사용자 지정 인증 사용자 경험에서 REST API 사용 서비스와 상호 운용 합니다.  
- OpenIDConnect 프로토콜을 준수 하는 id 공급자와 페더레이션 합니다.  
- SAML 2.0 프로토콜을 준수 하는 id 공급자와 페더레이션 합니다.   

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a>사용자 지정 정책 기능 집합 개발자의 책임

수동 정책 구성은 고유한 보안 프레임 워크 만들기의 결과 및 Azure AD B2C의 기본 플랫폼에 대 한 하위 수준 액세스 권한을 부여합니다. 가능한 순열이 많이 사용자 지정 id 공급자, 트러스트 관계, 외부 서비스 및 단계별 워크플로 사용 하 여 통합 디자인 및 구성과 체계적인 방법이 필요합니다. 

사용자 지정 정책 기능 집합을 사용 하는 개발자는 다음 지침을 준수 해야 합니다.

- 사용자 지정 정책 및 키/암호 관리의 구성 언어를 사용 하 여 파악 합니다. 자세한 내용은 [TrustFrameworkPolicy](trustframeworkpolicy.md)합니다. 
- 시나리오 및 사용자 지정 통합을 직접 소유합니다. 작업을 문서화 하 고 라이브 사이트 조직을 알립니다.  
- 체계적인 시나리오 테스트를 수행합니다. 
- 최소 1개의 개발/테스트 환경과 1개의 프로덕션 환경을 구축하여 소프트웨어 개발 및 스테이징 모범 사례를 준수합니다. 
- 사용자와 통합된 ID 공급자 및 서비스에 대한 새로운 개발 정보를 바로 입수합니다. 예를 들어 기밀 변경 내용과 예정되었거나 갑작스럽게 진행되는 서비스 변경 내용을 추적합니다. 
- 활성 모니터링을 설정하고 프로덕션 환경의 응답성을 모니터링합니다. Application Insights와 통합 하는 방법에 대 한 자세한 내용은 참조 하세요. [Azure Active Directory B2C: 로그 수집](active-directory-b2c-custom-guide-eventlogger-appins.md)합니다. 
- Azure 구독에서 연락처 전자 메일 주소를 최신 상태로 유지하고 Microsoft 라이브 사이트 팀 전자 메일에 즉시 응답합니다. 
- Microsoft 라이브 사이트 팀에서 권고할 때 시기 적절하게 조치를 취합니다.

## <a name="terms-for-features-in-public-preview"></a>공개 미리 보기에서 기능에 대 한 조건

- 평가 목적 으로만 공개 미리 보기 기능을 사용 하는 것이 좋습니다. 
- 서비스 수준 계약 (Sla) 공개 미리 보기 기능에 적용 되지 않습니다.
- 정식 지원 채널을 통해 공개 미리 보기 기능에 대 한 지원 요청을 제출 될 수 있습니다. 

## <a name="features-by-stage-and-known-issues"></a>단계 및 알려진 문제별 기능

사용자 지정 정책/i d 경험 프레임 워크 기능은 지속적이 고 신속 하 게 개발 합니다. 다음 표에서 기능 및 구성 요소 가용성의 인덱스입니다.

### <a name="identity-providers-tokens-protocols"></a>ID 공급자, 토큰, 프로토콜

| 기능 | 개발 | 미리 보기 | GA | 메모 |
|-------- | ----------- | ------- | -- | ----- |
| IDP-OpenIDConnect |  |  | X | 예를 들어, Google +입니다.  |
| IDP-OAUTH2 |  |  | X | 예를 들어 Facebook 합니다.  |
| IDP-oauth1)로 (twitter) |  | X |  | 예를 들어 Twitter입니다. |
| IDP-oauth1)로 (예: twitter) |  |  |  | 지원되지 않음 |
| IDP-SAML |  |   | X | 예를 들어, Salesforce, ADFS 합니다. |
| IDP-WSFED | X |  |  |  |
| 신뢰 당사자 OAUTH1 |  |  |  | 지원되지 않습니다. |
| 신뢰 당사자 OAUTH2 |  |  | X |  |
| 신뢰 당사자 OIDC |  |  | X |  |
| 신뢰 당사자 SAML | X |  |  |  |
| 신뢰 당사자 WSFED | X |  |  |  |
| Basic 사용 하 여 REST API 및 인증서 인증 |  |  | X | 예를 들어 Azure Logic Apps입니다. |

### <a name="component-support"></a>구성 요소 지원

| 기능 | 개발 | 미리 보기 | GA | 메모 |
| ------- | ----------- | ------- | -- | ----- |
| Azure Multi Factor Authentication |  |  | X |  |
| 로컬 디렉터리로서의 Azure Active Directory |  |  | X |  |
| 전자 메일 확인을 위한 azure 전자 메일 하위 시스템 |  |  | X |  |
| 다중 언어 지원|  |  | X |  |
| 조건자 유효성 검사 |  |  | X | 예를 들어, 암호 복잡성입니다. |
| 타사 전자 메일 서비스 공급자를 사용 하 여 | X |  |  |  |

### <a name="content-definition"></a>콘텐츠 정의

| 기능 | 개발 | 미리 보기 | GA | 메모 |
| ------- | ----------- | ------- | -- | ----- |
| 오류 페이지, api.error |  |  | X |  |
| IDP 선택 페이지, api.idpselections |  |  | X |  |
| 등록을 위한 IDP 선택, api.idpselections.signup |  |  | X |  |
| 암호 찾기, api.localaccountpasswordreset |  |  | X |  |
| 로컬 계정 로그인, api.localaccountsignin |  |  | X |  |
| 로컬 계정 등록, api.localaccountsignup |  |  | X |  |
| MFA 페이지, api.phonefactor |  |  | X |  |
| 자체 어설션된 소셜 계정 등록, api.selfasserted |  |  | X |  |
| 자체 어설션 프로필 업데이트, api.selfasserted.profileupdate |  |  | X |  |
| 통합된 등록 또는 로그인 페이지에서 "disableSignup" 매개 변수를 사용 하 여 api.signuporsignin |  |  | X |  |
| JavaScript / 페이지 계약 |  | X |  |  |

### <a name="app-ief-integration"></a>App-IEF 통합

| 기능 | 개발 | 미리 보기 | GA | 메모 |
| ------- | ----------- | ------- | -- | ----- |
| 쿼리 문자열 매개 변수 domain_hint |  |  | X | 사용 가능한 클레임으로 IDP로 전달할 수 있습니다. |
| 쿼리 문자열 매개 변수 login_hint |  |  | X | 사용 가능한 클레임으로 IDP로 전달할 수 있습니다. |
| client_assertion을 통해 UserJourney에 JSON 삽입 | X |  |  | 사용 되지 않습니다. |
| UserJourney에 Id_token_hint로 JSON 삽입 |  | X |  | JSON을 전달 하는 진행 방법입니다. |
| 응용 프로그램에 IDP 토큰 전달 |  | X |  | 예를 들어 Facebook 앱에 있습니다. |

### <a name="session-management"></a>세션 관리

| 기능 | 개발 | 미리 보기 | GA | 메모 |
| ------- | ----------- | ------- | -- | ----- |
| SSO 세션 공급자 |  |  | X |  |
| 외부 로그인 세션 공급자 |  |  | X |  |
| SAML SSO 세션 공급자 |  |  | X |  |
| 기본 SSO 세션 공급자 |  |  | X |  |

### <a name="security"></a>보안

| 기능 | 개발 | 미리 보기 | GA | 메모 |
|-------- | ----------- | ------- | -- | ----- |
| 정책 키 - 생성, 수동, 업로드 |  |  | X |  |
| 정책 키 - RSA/Cert, 비밀 |  |  | X |  |
| 정책 업로드 |  |  | X |  |

### <a name="developer-interface"></a>개발자 인터페이스

| 기능 | 개발 | 미리 보기 | GA | 메모 |
| ------- | ----------- | ------- | -- | ----- |
| Azure Portal-IEF UX |  |  | X |  |
| Application Insights UserJourney 로그 |  | X |  | 개발 중 문제 해결에 사용 합니다.  |
| (오케스트레이션 단계)를 통해 application Insights 이벤트 로그 |  | X |  | 프로덕션 환경에서 사용자 흐름을 모니터링 하는 데 사용 합니다. |

## <a name="next-steps"></a>다음 단계
[사용자 지정 정책 및 사용자 흐름을 사용 하 여 차이점에 자세히 알아보려면](active-directory-b2c-overview-custom.md)합니다.
