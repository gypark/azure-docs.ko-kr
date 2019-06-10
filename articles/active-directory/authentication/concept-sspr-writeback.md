---
title: Azure AD SSPR-Azure Active Directory 통합과 암호 쓰기 저장 온-프레미스
description: 온-프레미스 AD 인프라에 클라우드 비밀번호 쓰기 저장
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 749216d3fe9164857bd4abce7ba7c766e466e7d3
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65823292"
---
# <a name="what-is-password-writeback"></a>비밀번호 쓰기 저장이란?

클라우드 기반 암호 재설정 유틸리티가 있으면 참 좋지만, 대부분의 회사는 아직 사용자가 온-프레미스 디렉터리에 있습니다. Microsoft는 어떻게 기존 온-프레미스 AD(Active Directory)와 클라우드 암호 변경 내용 동기화를 지원할까요? 비밀번호 쓰기 저장은 [Azure AD Connect](../hybrid/whatis-hybrid-identity.md)를 통해 활성화되며 클라우드의 암호 변경 내용을 실시간으로 기존 온-프레미스 디렉터리에 다시 쓸 수 있는 기능입니다.

비밀번호 쓰기 저장은 다음을 사용하는 환경에서 지원됩니다.

* [ADFS(Active Directory Federation Services)](../hybrid/how-to-connect-fed-management.md)
* [암호 해시 동기화](../hybrid/how-to-connect-password-hash-synchronization.md)
* [통과 인증](../hybrid/how-to-connect-pta.md)

> [!WARNING]
> [2018년 11월 7일에 ACS(Azure Access Control) 서비스 사용이 중지되면](../develop/active-directory-acs-migration.md) Azure AD Connect 1.0.8641.0 이하 버전을 사용 중인 고객의 경우 비밀번호 쓰기 저장 기능의 작동이 중지됩니다. 해당 날짜부터는 Azure AD Connect 1.0.8641.0 이하 버전에서 더 이상 비밀번호 쓰기 저장이 허용되지 않습니다. 이러한 버전에서는 해당 기능에 ACS를 사용하기 때문입니다.
>
> 서비스 중단을 방지하려면 이전 버전 Azure AD Connect에서 최신 버전으로 업그레이드하세요([Azure AD Connect: 이전 버전에서 최신 버전으로 업그레이드](../hybrid/how-to-upgrade-previous-version.md) 문서 참조).
>

비밀번호 쓰기 저장은 다음 기능을 제공합니다.

* **온-프레미스 Active Directory 암호 정책 적용**: 사용자가 자신의 암호를 재설정하는 경우 해당 디렉터리에 커밋하기 전에 온-프레미스 Active Directory 정책을 준수하는지 확인합니다. 이 검토에는 기록, 복잡성, 나이, 암호 필터 및 로컬 Active Directory에서 사용자가 정의한 기타 암호 제한 사항 확인이 포함됩니다.
* **지연 피드백 없음**:  암호 쓰기 저장은 동기식 작업입니다. 사용자의 암호가 정책에 맞지 않거나 어떤 이유로든 재설정 또는 변경할 수 없는 경우 즉시 사용자에게 알려줍니다.
* **액세스 패널 및 Office 365에서 암호 변경 지원**: 페더레이션 또는 암호 해시 동기화된 사용자가 만료되었거나 만료되지 않은 암호를 변경하면 해당 암호는 로컬 Active Directory 환경에 다시 기록됩니다.
* **Azure Portal에서 관리자가 암호를 재설정할 때 비밀번호 쓰기 저장 지원**: 사용자가 페더레이션 또는 암호 해시 동기화된 경우 관리자가 [Azure Portal](https://portal.azure.com)에서 해당 사용자의 암호를 재설정할 때마다 암호가 온-프레미스에 다시 기록됩니다. 이 기능은 현재 Office 관리자 포털에서 지원되지 않습니다.
* **인바운드 방화벽 규칙 필요 없음**: 비밀번호 쓰기 저장은 Azure Service Bus Relay를 기본 통신 채널로 사용합니다. 모든 통신은 포트 443을 통해 아웃바운드됩니다.

> [!NOTE]
> 온-프레미스 Active Directory의 보호 그룹 내에 있는 사용자 계정은 비밀번호 쓰기 저장에 사용할 수 없습니다. 온-프레미스 AD의 보호 그룹 내에 있는 관리자 계정은 비밀번호 쓰기 저장에 사용할 수 없습니다. 보호 그룹에 대한 자세한 내용은 [Active Directory의 보호 계정 및 그룹](https://technet.microsoft.com/library/dn535499.aspx)을 참조하세요.

## <a name="licensing-requirements-for-password-writeback"></a>비밀번호 쓰기 저장에 대한 라이선스 요구 사항

**셀프 서비스 암호 재설정/변경/온-프레미스 쓰기 저장으로 잠금 해제는 Azure AD의 프리미엄 기능입니다**. 라이선스에 대한 자세한 내용은 [Azure Active Directory 가격 책정 페이지](https://azure.microsoft.com/pricing/details/active-directory/)를 참조하세요.

비밀번호 쓰기 저장을 사용하려면 테넌트에 다음과 같은 라이선스 중 하나가 할당되어 있어야 합니다.

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3 또는 A3
* Enterprise Mobility + Security E5 또는 A5
* Microsoft 365 E3 또는 A3
* Microsoft 365 E5 또는 A5
* Microsoft 365 F1
* Microsoft 365 Business

> [!WARNING]
> 독립 실행형 Office 365 라이선스 요금제는 *"셀프 서비스 암호 재설정/변경/온-프레미스 쓰기 저장으로 잠금 해제"를 지원하지 않습니다*. 이 기능을 사용하려면 위의 요금제 중 하나가 필요합니다.

## <a name="how-password-writeback-works"></a>암호 쓰기 저장의 작동 원리

페더레이션 또는 암호 해시 동기화된 사용자가 클라우드에서 자신의 암호를 재설정하거나 변경하려고 시도하면 다음 작업이 수행됩니다.

1. 사용자가 어떤 종류의 암호를 가지고 있는지 확인하기 위한 검사가 수행됩니다. 암호가 온-프레미스에서 관리되는 경우:
   * 쓰기 저장 서비스를 사용 중인지 확인하기 위한 검사가 수행됩니다. 사용 중인 경우 사용자가 계속 진행할 수 있습니다.
   * 쓰기 저장 서비스가 중지된 경우 사용자에게 지금은 암호를 재설정할 수 없다고 알려줍니다.
1. 다음으로, 사용자는 적절한 인증 게이트를 통과하여 **암호 재설정** 페이지로 이동됩니다.
1. 사용자는 새 암호를 선택하고 다시 확인합니다.
1. 사용자가 **제출**을 선택하면 쓰기 저장 설정 프로세스에서 만든 대칭 키로 일반 텍스트 암호가 암호화됩니다.
1. 암호화된 암호는 HTTPS 채널을 통해 테넌트별 Service Bus Relay(쓰기 저장 설정 프로세스에서 자동으로 설정됨)로 전송되는 페이로드에 포함됩니다. 이 릴레이는 온-프레미스 설치만 알고 있는 임의로 생성된 암호에 의해 보호됩니다.
1. 메시지가 서비스 버스에 도달하면 암호 재설정 엔드포인트가 자동으로 절전 모드에서 해제되어 보류 중인 재설정 요청이 있는지 확인합니다.
1. 그런 다음, 서비스에서 클라우드 앵커 특성을 사용하여 해당 사용자를 찾습니다. 이 조회가 성공하려면:

   * 사용자 개체가 Active Directory 커넥터 공간에 있어야 합니다.
   * 사용자 개체가 해당 MV(metaverse) 개체에 연결되어야 합니다.
   * 사용자 개체가 해당 Azure Active Directory 커넥터 개체에 연결되어야 합니다.
   * Active Directory 커넥터 개체에서 MV로의 링크에는 동기화 규칙 `Microsoft.InfromADUserAccountEnabled.xxx`이 있어야 합니다.
   
   클라우드에서 호출이 들어오면 동기화 엔진은 **cloudAnchor** 특성을 사용하여 Azure Active Directory 커넥터 공간 개체를 조회합니다. 그런 후 다시 링크를 따라 MV 개체로 이동한 후 Active Directory 개체로 이동합니다. 동일한 사용자에 대해 여러 Active Directory 개체(다중 포리스트)가 있을 수 있기 때문에 동기화 엔진은 `Microsoft.InfromADUserAccountEnabled.xxx` 링크에 의존하여 정확한 개체를 선택합니다.

1. 사용자 계정을 찾은 후에는 적절한 Active Directory 포리스트에서 직접 암호를 재설정하려는 시도가 수행됩니다.
1. 암호 설정 작업이 성공하면 사용자에게 암호가 변경되었음을 알려줍니다.
   > [!NOTE]
   > 사용자의 암호 해시가 암호 해시 동기화를 통해 Azure AD에 동기화되는 경우 온-프레미스 암호 정책이 클라우드 암호 정책보다 약할 수 있습니다. 이 경우 온-프레미스 정책이 적용됩니다. 이 정책을 사용하면 Single Sign-On을 제공하기 위해 암호 해시 동기화를 사용하든 아니면 페더레이션을 사용하든, 온-프레미스 정책이 클라우드에서 적용됩니다.

1. 암호 설정 작업이 실패하면 다시 시도하라는 오류 메시지가 표시됩니다. 다음 이유로 작업이 실패할 수 있습니다.
    * 서비스가 종료되었습니다.
    * 선택한 암호가 조직 정책을 충족하지 못했습니다.
    * 로컬 Active Directory에서 사용자를 찾을 수 없습니다.

      오류 메시지는 관리자 개입 없이 문제 해결을 시도할 수 있도록 사용자에게 지침을 제공합니다.

## <a name="password-writeback-security"></a>비밀번호 쓰기 저장 보안

비밀번호 쓰기 저장은 매우 안전한 서비스입니다. 사용자 정보를 보호하기 위해 다음과 같이 4계층 보안 모델이 사용됩니다.

* **테넌트별 Service Bus Relay**
   * 사용자가 이 서비스를 설정하면 임의로 생성된 강력한 암호에 의해 보호되는 테넌트별 Service Bus Relay가 설정되며, Microsoft는 절대로 이 암호에 액세스할 수 없습니다.
* **잠겨 있는 강력한 암호 암호화 키**
   * Service Bus Relay를 만든 후 회선을 통해 도착하는 암호를 암호화하는 데 사용할 강력한 대칭 키가 생성됩니다. 이 키는 클라우드의 회사의 암호 저장소에만 존재하며 디렉터리의 다른 암호와 마찬가지로 강력하게 잠겨 있고 감사됩니다.
* **업계 표준 TLS(전송 계층 보안)**
   1. 클라우드에서 암호 재설정 또는 변경 작업이 발생하면 공개 키를 사용하여 일반 텍스트 암호가 암호화됩니다.
   1. 암호화된 암호는 Microsoft의 SSL 인증서를 사용하여 암호화된 채널을 통해 Service Bus Relay로 전송되는 HTTPS 메시지에 배치됩니다.
   1. Service Bus에 메시지가 도착한 후에 온-프레미스 에이전트가 시작하고 이전에 생성된 강력한 암호를 사용하여 Service Bus에 인증합니다.
   1. 온-프레미스 에이전트는 암호화된 메시지를 선택하고 프라이빗 키를 사용하여 암호를 해독합니다.
   1. 온-프레미스 에이전트는 AD DS SetPassword API를 통해 암호를 설정하려고 합니다. 이 단계에서는 클라우드에서 Active Directory 온-프레미스 암호 정책(예: 복잡성, 나이, 기록, 필터)을 적용할 수 있습니다.
* **메시지 만료 정책**
   * 온-프레미스 서비스가 종료되었기 때문에 메시지가 Service Bus에 남아 있는 경우 시간이 초과되고 몇 분 후에 제거됩니다. 메시지의 시간 초과 및 제거를 통해 메시지 보안이 더욱 강화됩니다.

### <a name="password-writeback-encryption-details"></a>비밀번호 쓰기 저장 암호화 세부 정보

사용자가 암호 재설정을 제출한 후 재설정 요청은 여러 암호화 단계를 거친 후 온-프레미스 환경에 도달합니다. 이러한 암호화 단계는 최대의 서비스 안정성과 보안을 보장합니다. 다음과 같이 설명할 수 있습니다.

* **1단계: 2048비트 RSA 키를 사용한 암호 암호화**: 사용자가 온-프레미스에 다시 작성된 암호를 제출하면 제출된 암호 자체를 2048비트 RSA 키로 암호화합니다.
* **2단계: AES-GCM을 사용한 패키지 수준 암호화**: AES-GCM을 사용하여 전체 패키지(암호 + 필수 메타데이터)를 암호화합니다. 이 암호화는 기본 ServiceBus 채널에 직접 액세스할 수 있는 사람이 콘텐츠를 보거나 변조하는 것을 방지합니다.
* **3단계: 모든 통신이 TLS/SSL을 통해 발생**: Service Bus와 모든 통신은 SSL/TLS 채널에서 발생합니다. 이 암호화는 권한이 없는 제3자의 콘텐츠를 보호합니다.
* **6개월마다 자동 키 롤오버**: 최상의 서비스 보안과 안전성을 보장하기 위해, 모든 키는 6개월마다 또는 Azure AD Connect에서 비밀번호 쓰기 저장이 해제되었다가 다시 설정될 때마다 롤오버됩니다.

### <a name="password-writeback-bandwidth-usage"></a>비밀번호 쓰기 저장 대역폭 사용

비밀번호 쓰기 저장은 다음과 같은 경우에만 온-프레미스 에이전트로 요청을 다시 보내는 낮은 대역폭 서비스입니다.

* Azure AD Connect를 통해 이 기능이 사용되거나 사용되지 않도록 설정될 때 2개의 메시지가 전송됩니다.
* 서비스가 실행되는 동안 한 메시지를 서비스 하트 비트로 5분마다 한 번씩 전송합니다.
* 두 개의 메시지는 새 암호를 전송할 때 각각 전송됩니다.
   * 첫 번째 메시지는 작업을 수행하는 요청입니다.
   * 두 번째 메시지는 작업의 결과를 포함하고 다음과 같은 경우에 전송됩니다.
      * 사용자 셀프 서비스 비밀번호 다시 설정 중에 새 비밀번호가 제출될 때마다
      * 사용자 비밀번호 변경 작업 중에 새 비밀번호가 제출될 때마다
      * 관리자가 시작한 사용자 비밀번호 재설정 중에 새 비밀번호가 제출될 때마다(Azure 관리 포털에서만)

#### <a name="message-size-and-bandwidth-considerations"></a>메시지 크기 및 대역폭 고려 사항

앞에서 설명한 각 메시지의 일반적으로 1KB 미만입니다. 극단적인 부하에서도 비밀번호 쓰기 저장 서비스 자체에서 초당 몇 Kb의 대역폭을 소비합니다. 각 메시지가 비밀번호 업데이트 작업에서 요구할 때만 실시간으로 전송되고 메시지 크기가 너무 작기 때문에 쓰기 저장 기능의 대역폭 사용은 실제 측정 가능한 영향을 미치기에는 너무 작습니다.

## <a name="supported-writeback-operations"></a>지원되는 쓰기 저장 작업

암호는 다음과 같은 경우에 모두 다시 기록됩니다.

* **지원되는 최종 사용자 작업**
   * 모든 최종 사용자 셀프 서비스 자발적 변경 암호 작업
   * 모든 최종 사용자 셀프 서비스 적용 변경 암호 작업(예: 암호 만료)
   * [암호 재설정 포털](https://passwordreset.microsoftonline.com)에서 시작되는 모든 최종 사용자 셀프 서비스 암호 재설정
* **지원되는 관리자 작업**
   * 모든 관리자 셀프 서비스 자발적 변경 암호 작업
   * 모든 관리자 셀프 서비스 적용 변경 암호 작업(예: 암호 만료)
   * [암호 재설정 포털](https://passwordreset.microsoftonline.com)에서 시작되는 모든 관리자 셀프 서비스 암호 재설정
   * [Azure Portal](https://portal.azure.com)에서 관리자 시작 최종 사용자 암호 재설정
   * 모든 관리자 시작 최종 사용자 암호를 재설정 합니다 [Microsoft 365 관리 센터](https://admin.microsoft.com)

## <a name="unsupported-writeback-operations"></a>지원되지 않는 쓰기 저장 작업

암호는 다음과 같은 경우에 다시 기록되지 *않습니다*.

* **지원되지 않는 최종 사용자 작업**
   * PowerShell 버전 1, 버전 2 또는 Azure AD Graph API를 사용하여 자신의 암호를 재설정하는 최종 사용자
* **지원되지 않는 관리자 작업**
   * PowerShell 버전 1, 버전 2 또는 Azure AD Graph API에서 관리자 시작 최종 사용자 암호 재설정

> [!WARNING]
> 확인란의 "반드시 암호 변경 다음 로그온 할 때" Active Directory 사용자 및 컴퓨터 또는 Active Directory 관리 센터와 같은 온-프레미스 Active Directory 관리 도구에서 사용 하 여 지원 되지 않습니다. 암호를 변경 하는 경우 온-프레미스가이 옵션을 선택 하지 마십시오.

## <a name="next-steps"></a>다음 단계

다음 자습서를 사용하여 비밀번호 쓰기 저장 설정: [비밀번호 쓰기 저장을 사용하도록 설정](tutorial-enable-writeback.md)
