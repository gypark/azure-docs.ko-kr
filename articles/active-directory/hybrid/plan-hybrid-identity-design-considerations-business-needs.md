---
title: Azure의 하이브리드 클라우드 ID 디자인에 대한 ID 요구 사항 | Microsoft Docs
description: 하이브리드 ID 설계에 대한 요구 사항을 정의하도록 하는 회사의 비즈니스 요구를 식별합니다.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: de690978-84ef-41ad-9dfe-785722d343a1
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/29/2019
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9ecc90e13f49c231d8d3ab0cff1de91443b80f21
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65950904"
---
# <a name="determine-identity-requirements-for-your-hybrid-identity-solution"></a>하이브리드 ID 솔루션에 대한 ID 요구 사항 확인
하이브리드 ID 솔루션을 설계하는 첫 번째 단계는 이 솔루션을 활용하는 비즈니스 조직에 대한 요구 사항을 결정하는 것입니다.  하이브리드 ID는 지원 역할(인증을 제공하여 다른 모든 클라우드 솔루션을 지원)로 시작하고 사용자에 대한 새 워크로드의 잠금을 해제하는 새롭고 흥미로운 기능을 계속하여 제공합니다.  사용자에 대해 채택하려는 이러한 워크로드 또는 서비스는 하이브리드 ID 설계에 대한 요구 사항을 결정합니다.  이러한 서비스와 워크로드는 온-프레미스 및 클라우드에서 하이브리드 ID를 활용해야 합니다.  

현재의 요구 사항 및 미래 회사 계획이 무엇인지 이해하려면 비즈니스의 이러한 주요 측면을 검토해야 합니다. 하이브리드 ID 설계에 대한 장기적인 전략을 표시하지 않는다면 비즈니스가 성장하고 변화해야 할 때 솔루션은 확장이 불가능합니다. 아래 다이어그램에는 하이브리드 id 아키텍처 및 사용자에 대 한 잠금이 해제 되는 작업 부하의 예가 나와 있습니다. 잠금을 해제하고 견고한 하이브리드 ID 전략을 제공할 수 있는 새로운 가능성의 예입니다. 

하이브리드 id 아키텍처의 일부인 일부 구성 요소 ![하이브리드 id 아키텍처](./media/plan-hybrid-identity-design-considerations/hybrid-identity-architechture.png)

## <a name="determine-business-needs"></a>비즈니스 요구 사항 결정
이러한 회사가 같은 업계의 일부이더라도 실제 비즈니스 요구 사항이 다르기 때문에 각 회사는 서로 다른 요구 사항을 갖습니다. 업계에서 모범 사례를 활용할 수 있지만 궁극적으로 하이브리드 ID 설계에 대한 요구 사항을 정의하도록 하는 것은 회사의 비즈니스 요구 사항입니다. 

다음 질문에 대답하여 비즈니스 요구를 식별합니다.

* 회사가 IT 운영 비용 절감을 고민하고 있습니까?
* 회사가 클라우드 자산(SaaS 앱, 인프라)을 보호에 대해 고민하고 있습니까?
* 회사가 IT를 현대화하기 위해 고민하고 있습니까?
  * 사용자가 다른 형태의 트래픽이 다른 리소스에 액세스할 수 있도록 DMZ에 예외를 만들기 위해 더 많은 모바일 및 IT를 요구합니까?
  * 회사에는 이러한 최신 사용자에게 게시하는 데 필요하지만 쉽게 다시 작성하지 않는 레거시 앱이 있습니까?
  * 회사가 이러한 모든 작업을 수행하는 동시에 제어해야 합니까?
* 회사가 Microsoft의 Azure 보안 전문가 온-프레미스의 전문 지식을 활용하는 새로운 도구를 도입하여 사용자 ID를 보호하고 위험을 줄이려고 합니까?
* 회사가 온-프레미스에서 "외부" 계정을 제거하고 온-프레미스 환경 내에서 유휴 위협이 아닌 클라우드로 이동하려 합니까?

## <a name="analyze-on-premises-identity-infrastructure"></a>온-프레미스 ID 인프라 분석
회사의 비즈니스 요구 사항에 관해 아이디어가 있으므로 온-프레미스 ID 인프라를 평가해야 합니다. 이 평가판은 클라우드 ID 관리 시스템에 현재 ID 솔루션을 통합하는 기술 요구 사항을 정의하는 데 중요합니다. 다음 질문에 답변하세요.

* 회사는 온-프레미스에 어떤 인증 및 권한 부여 솔루션을 사용합니까? 
* 회사에는 현재 온-프레미스 동기화 서비스가 있습니까?
* 회사가 타사 ID 공급자(IdP)를 사용합니까?

또한 회사에 있을 수도 있는 클라우드 서비스를 인식해야 합니다. 사용자 환경에서 SaaS IaaS, PaaS 모델을 사용하는 현재 통합을 이해하도록 평가를 수행하는 것이 매우 중요합니다. 이 평가를 진행하는 동안 다음 질문에 답변하세요.

* 귀사가 클라우드 서비스 공급자와 통합된 부분이 있습니까?
* 만약 그렇다면 어떤 서비스를 사용하십니까?
* 이 통합이 현재 운영 중입니까 아니면 파일럿입니까?

> [!NOTE]
> Cloud Discovery는 70개가 넘는 위험 요소를 기준으로 순위 및 점수가 매겨진 16,000개 클라우드 앱의 Microsoft Cloud App Security 클라우드 앱 카탈로그에 대한 트래픽 로그를 분석하여 클라우드 사용, 섀도 IT 및 조직에 발생하는 위험 섀도 IT에 대해 진행 중인 가시성을 제공합니다. 시작하려면 [Cloud Discovery 설정](/cloud-app-security/set-up-cloud-discovery)을 참조하세요.
> 
> 

## <a name="evaluate-identity-integration-requirements"></a>ID 통합 요구 사항 평가
다음으로 ID 통합 요구 사항을 평가해야 합니다. 이 평가는 사용자가 인증하는 방법, 조직의 우선 순위가 클라우드에서 보이는 방법, 조직이 권한을 부여하는 방법 및 사용자 환경이 변할 모습에 대한 기술 요구 사항을 정의하는 데 중요합니다. 다음 질문에 답변하세요.

* 조직이 페더레이션, 표준 인증 또는 둘 모두를 사용합니까?
* 페더레이션 요구 사항입니까?  다음 때문입니다.
  * Kerberos 기반 SSO
  * 회사에는 SAML 또는 유사한 페더레이션 기능을 사용하는 온-프레미스 애플리케이션이 있습니다(기본 제공 또는 타사 제공 중 하나).
  * 스마트 카드를 통한 MFA. RSA SecurID 등.
  * 아래 질문을 해결하는 클라이언트 액세스 규칙:
    1. 클라이언트의 IP 주소에 기반하여 Office 365에 대한 모든 외부 액세스를 차단할 수 있습니까?
    2. Exchange ActiveSync를 제외하고 Office 365에 대한 모든 외부 액세스를 차단할 수 있습니까?
    3. 브라우저 기반 앱(OWA, SPO)을 제외한 Office 365에 모든 외부 액세스를 차단할 수 있습니까?
    4. 지정된 AD 그룹의 멤버에 대한 Office 365의 모든 외부 액세스를 차단할 수 있습니까?
* 보안/감사 문제
* 페더레이션된 인증에서 기존 투자
* 회사는 클라우드의 도메인에 어떤 이름을 사용합니까?
* 조직에는 사용자 지정 도메인이 있습니까?
  1. 해당 도메인이 공용이고 DNS를 통해 쉽게 확인할 수 있습니까?
  2. 그렇지 않다면 대체 UPN를 AD에 등록하는데 사용할 수 있는 공용 도메인이 있습니까?
* 사용자 식별자가 클라우드 표현에 대해 일관됩니까? 
* 조직에는 클라우드 서비스와 통합을 필요로 하는 앱이 있습니까?
* 조직이 여러 도메인을 가지고 표준 또는 페더레이션된 인증을 모두 사용합니까?

## <a name="evaluate-applications-that-run-in-your-environment"></a>사용자 환경에서 실행하는 애플리케이션 평가
온-프레미스 및 클라우드 인프라에 관한 아이디어가 있으므로 이러한 환경에서 실행되는 애플리케이션을 평가해야 합니다. 이 평가판은 클라우드 ID 관리 시스템에 이러한 애플리케이션을 통합하는 기술 요구 사항을 정의하는 데 중요합니다. 다음 질문에 답변하세요.

* 애플리케이션은 어디에 있습니까?
* 사용자는 온-프레미스 애플리케이션에 액세스합니까?  클라우드에서? 또는 둘 모두?
* 기존 애플리케이션 워크로드를 클라우드로 이동할 계획이 있습니까?
* 클라우드 인증을 사용하는 온-프레미스 또는 클라우드에 위치하는 새 애플리케이션을 개발할 계획이 있습니까 ?

## <a name="evaluate-user-requirements"></a>사용자 요구 사항 평가
또한 사용자 요구 사항을 평가해야 합니다. 이 평가판은 사용자가 클라우드에 전환할 때 사용자를 등록 및 지원하기 위해 필요한 단계를 정의하는 데 중요합니다. 다음 질문에 답변하세요.

* 사용자는 애플리케이션 온-프레미스에 액세스합니까?
* 사용자는 클라우드에서 애플리케이션에 액세스합니까?
* 일반적으로 사용자는 해당 온-프레미스 환경에 어떻게 로그인합니까?
* 사용자가 클라우드에 어떻게 로그인합니까?

> [!NOTE]
> 각 답변을 주목하고 답변 이유를 이해해야 합니다. [사고 대응 요구 사항 결정](plan-hybrid-identity-design-considerations-incident-response-requirements.md) 은 사용할 수 있는 옵션 및 각 옵션의 장단점을 검토합니다.  질문에 답변함으로써 비즈니스 요구 사항에 가장 적합한 옵션을 선택할 수 있습니다.
> 
> 

## <a name="next-steps"></a>다음 단계
[디렉터리 동기화 요구 사항 결정](plan-hybrid-identity-design-considerations-directory-sync-requirements.md)

## <a name="see-also"></a>참고 항목
[디자인 고려 사항 개요](plan-hybrid-identity-design-considerations-overview.md)

