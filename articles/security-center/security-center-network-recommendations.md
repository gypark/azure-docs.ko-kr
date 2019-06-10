---
title: Azure Security Center에서 네트워크 리소스 보호 | Microsoft Docs
description: 이 문서에서는 Azure 네트워크 리소스를 보호하고 보안 정책을 준수하는 데 도움이 되는 Azure Security Center의 권장 사항에 대해 설명합니다.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 96c55a02-afd6-478b-9c1f-039528f3dea0
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/05/2019
ms.author: v-mohabe
ms.openlocfilehash: 6b3cef32cf79c2448d2e254e27c332e01ea83c62
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66428368"
---
# <a name="protect-your-network-resources-in-azure-security-center"></a>Azure Security Center에서 네트워크 리소스 보호
Azure Security Center는 네트워크 보안 모범 사례에 대한 Azure 리소스의 보안 상태를 지속적으로 분석합니다. Security Center에서 잠재적인 보안 취약점을 식별하는 경우 리소스를 보호하고 강화하는 데 필요한 컨트롤을 구성하는 과정을 안내하는 권장 사항을 만듭니다.

이 문서에서는 네트워크 보안 관점에서 Azure 리소스에 적용되는 권장 사항을 설명합니다. 네트워킹 권장 사항은 차세대 방화벽, 네트워크 보안 그룹, JIT VM 액세스 허용 범위가 과도하게 큰 인바운드 트래픽 규칙 등에 초점을 둡니다. 네트워킹 권장 사항 및 수정 작업 목록은 [Azure Security Center에서 보안 권장 사항 관리](security-center-recommendations.md)를 참조하세요.

> [!NOTE]
> **네트워킹** 페이지에서 네트워크 관점에서 본 Azure 리소스 상태에 대해 자세히 알아볼 수 있습니다. 네트워크 맵 및 적응 네트워크 컨트롤은 Azure Security Center 표준 계층에서만 사용할 수 있습니다. [무료 계층을 사용하는 경우 **레거시 네트워킹 보기** 단추를 클릭하여 네트워킹 리소스 권장 사항을 받을 수 있습니다](#legacy-networking).
>

**네트워킹** 페이지에서는 네트워크 리소스의 상태에 대한 자세한 정보를 볼 수 있는 섹션의 개요를 제공합니다.

- 네트워크 맵(Azure Security Center 표준 계층만 해당)
- 적응형 네트워크 강화
- 네트워킹 보안 권장 사항
- 레거시 **네트워킹** 블레이드(이전 네트워킹 블레이드) 
 
![네트워킹 창](./media/security-center-network-recommendations/networking-pane.png)

## <a name="network-map"></a>네트워크 맵
대화형 네트워크 맵은 네트워크 리소스를 강화하기 위한 권장 사항 및 정보를 제공하는 보안 오버레이를 통해 그래픽 뷰를 제공합니다. 맵을 사용하면 Azure 워크로드의 네트워크 토폴로지, 가상 머신과 서브넷 간 연결, 맵에서 특정 리소스 및 해당 리소스에 대한 권장 사항으로 드릴다운하는 기능을 확인할 수 있습니다.

네트워크 맵을 열려면:

1. Security Center의 리소스 보안 예방 조치에서 **네트워킹**을 선택합니다.
2. **네트워크 맵**에서 **토폴로지 보기**를 클릭합니다.
 
토폴로지 맵의 기본 보기에 다음이 표시됩니다.
- Azure에서 선택한 구독입니다. 맵은 여러 구독을 지원합니다.
- Vm, 서브넷 및 Vnet (클래식 Azure 리소스는 지원 되지 않습니다)의 Resource Manager 리소스 종류의
- 피어 링 된 Vnet
- [네트워크 권장 사항](security-center-recommendations.md)의 심각도가 높거나 보통인 리소스만  
- 인터넷 연결 리소스
- 맵은 Azure에서 선택한 구독에 대해 최적화됩니다. 선택 항목을 수정하는 경우 맵이 다시 계산되고 새 설정을 기반으로 다시 최적화됩니다.  

![네트워킹 토폴로지 맵](./media/security-center-network-recommendations/network-map-info.png)

## <a name="understanding-the-network-map"></a>네트워크 맵 이해

네트워크 맵은 Azure 리소스를 **토폴로지** 보기 및 **트래픽** 보기로 표시할 수 있습니다.

### <a name="the-topology-view"></a>토폴로지 보기

네트워킹 맵의 **토폴로지** 보기에서 네트워킹 리소스에 대한 다음과 같은 정보를 볼 수 있습니다.
- 내부 원에는 선택한 구독 내에 있는 모든 Vnet을 볼 수 있으며, 다음 원은 모든 서브넷이고, 외부 원은 모든 가상 머신입니다.
- 맵에서 리소스를 연결하는 선은 서로 연결된 리소스가 무엇인지, Azure 네트워크가 어떻게 구조화되었는지를 나타냅니다. 
- 심각도 표시기를 사용하여 Security Center에서 열린 권장 사항이 있는 리소스의 개요를 신속하게 가져올 수 있습니다.
- 리소스 중 하나를 클릭하여 드릴다운하고 해당 리소스 및 해당 권장 사항에 대한 세부 사항을 네트워크 맵의 컨텍스트에서 직접 볼 수 있습니다.  
- 맵에 표시되는 리소스가 너무 많은 경우 Azure Security Center는 해당 소유 알고리즘을 사용하여 리소스를 스마트하게 클러스터링함으로써 심각도 수준이 가장 높고, 가장 높은 심각도 권장 사항이 있는 리소스를 강조 표시합니다. 

맵은 대화형이며 동적이므로 모든 노드를 클릭할 수 있고 필터에 따라 보기를 변경할 수 있습니다.

1. 맨 위에 있는 필터를 사용하면 네트워크 맵에서 표시되는 항목을 수정할 수 있습니다. 다음을 기준으로 맵에 집중할 수 있습니다.
   -  **보안 상태**: Azure 리소스의 심각도(높음, 중간, 낮음)에 따라 맵을 필터링할 수 있습니다.
   - **권장 사항**: 해당 리소스에서 활성화된 권장 사항을 기준으로 표시되는 리소스를 선택할 수 있습니다. 예를 들어, Security Center에서 네트워크 보안 그룹 사용을 권장하는 리소스만 볼 수 있습니다.
   - **네트워크 영역**: 기본적으로 맵은 인터넷 연결 리소스만 표시하므로 내부 VM도 선택할 수 있습니다.
 
2. 언제든지 왼쪽 위 모서리에 있는 **재설정**을 클릭하면 맵을 기본 상태로 돌릴 수 있습니다.

리소스로 드릴다운하려면:
1. 맵에서 특정 리소스를 선택하면 오른쪽 창이 열리고 리소스에 대한 일반 정보, 연결된 보안 솔루션(있는 경우) 및 리소스와 관련된 권장 사항을 제공합니다. 선택한 리소스의 각 형식에 대해 동일한 동작 형식입니다. 
2. 맵에서 노드 위로 마우스를 가져가면 구독, 리소스 형식 및 리소스 그룹을 포함한 리소스에 대한 일반 정보를 볼 수 있습니다.
3. 링크를 사용하여 도구 팁을 확대하고, 해당 특정 노드에서 맵에 다시 초점을 맞춥니다. 
4. 특정 노드에서 떨어져서 맵에 초점을 다시 맞추려면 축소합니다.

### <a name="the-traffic-view"></a>트래픽 보기

**트래픽** 보기는 리소스 간의 가능한 모든 트래픽의 맵을 제공합니다. 이렇게 하면 사용자와 통신할 수 있는 리소스를 정의하는 모든 사용자가 구성한 규칙의 시각적 맵이 제공됩니다. 그러면 신속하게 워크로드 내에서 위험이 있는 구성을 식별할 수 있을 뿐 아니라 네트워크 보안 그룹의 기존 구성도 볼 수 있습니다.

### <a name="uncover-unwanted-connections"></a>원치 않는 연결 파악

이 보기의 장점은 이러한 허용된 연결과 존재하는 취약성을 표시하는 기능입니다. 따라서 이 데이터의 교집합 영역을 사용하여 사용자 리소스에 필요한 강화 작업을 수행할 수 있습니다. 

예를 들어, 인식하지 못한 두 개의 머신이 통신할 수 있음을 감지하여 워크로드와 서브넷을 더 잘 격리할 수 있습니다.

### <a name="investigate-resources"></a>리소스 조사

리소스로 드릴다운하려면:
1. 맵에서 특정 리소스를 선택하면 오른쪽 창이 열리고 리소스에 대한 일반 정보, 연결된 보안 솔루션(있는 경우) 및 리소스와 관련된 권장 사항을 제공합니다. 선택한 리소스의 각 형식에 대해 동일한 동작 형식입니다. 
2. 리소스에서 가능한 아웃바운드 및 인바운드 트래픽의 목록을 보려면 **트래픽**을 클릭합니다. 이 목록은 리소스와 통신할 수 있는 사용자 및 프로토콜과 포트를 통해 리소스가 통신할 수 있는 대상의 포괄적인 목록입니다. 예를 들어, 선택 하면 VM, 통신할 수 있는 모든 Vm은 표시 및와 통신할 수 있는 모든 서브넷은 표시 된 서브넷을 선택 하면 합니다.

**이 데이터는 크로스오버 및 상호 작용을 이해하기 위해 여러 규칙을 분석하는 고급 Machine Learning 알고리즘과 함께 네트워크 보안 그룹의 분석을 기반으로 합니다.** 

![네트워킹 트래픽 맵](./media/security-center-network-recommendations/network-map-traffic.png)

## 레거시 네트워킹 <a name ="legacy-networking"></a>

이 섹션에서는 Security Center 표준 계층이 없는 경우 사용 가능한 네트워킹 권장 사항을 확인하는 방법을 설명합니다.

이 정보에 액세스하려면 네트워킹 블레이드에서 **레거시 네트워킹 보기**를 클릭합니다. 

![레거시 네트워킹](./media/security-center-network-recommendations/legacy-networking.png)

### <a name="internet-facing-endpoints-section"></a>인터넷 연결 엔드포인트 섹션
**인터넷 연결 엔드포인트** 섹션에서 현재 인터넷 연결 엔드포인트로 구성된 가상 머신 및 해당 상태를 확인할 수 있습니다.

이 테이블에는 엔드포인트 이름, 인터넷 연결 IP 주소, 네트워크 보안 그룹 및 NGFW 권장 사항의 현재 심각도 상태가 있습니다. 테이블은 심각도별로 정렬됩니다.

### <a name="networking-topology-section"></a>네트워킹 토폴로지 섹션
**네트워킹 토폴로지** 섹션에는 리소스의 계층적 보기가 있습니다.

테이블은 심각도별로 정렬됩니다(가상 머신 및 서브넷).

이 토폴로지 보기에서 첫 번째 수준은 Vnet을 표시합니다. 두 번째 표시에는 서브넷이 있고, 세 번째 수준은 해당 서브넷에 속하는 가상 머신을 표시합니다. 오른쪽 열에는 해당 리소스의 네트워크 보안 그룹 권장 사항의 현재 상태가 표시됩니다.

세 번째 수준은 이전에 설명한 것과 유사한 가상 머신을 표시합니다. 리소스를 클릭하여 필요한 보안 제어 또는 구성을 자세히 알아보거나 적용할 수 있습니다.

## <a name="network-recommendations"></a>네트워크 권장 사항

|리소스 종류|보안 점수|권장 사항|설명|
|----|----|----|----|
|Machine|40|가상 머신에 대 한 네트워크 보안 그룹을 사용 해야|가상 머신의 네트워크 액세스를 제어하기 위해 네트워크 보안 그룹을 활성화합니다.|
|서브넷|35|서브넷 수준에서 네트워크 보안 그룹을 사용 해야|서브넷에 배포된 리소스의 네트워크 액세스를 제어하기 위해 네트워크 보안 그룹을 활성화합니다.|
|Machine|30|가상 머신에서-Just-in-time 네트워크 액세스 제어를 적용 해야|선택한 포트에 대한 액세스를 영구적으로 잠그기 위해 Just-In-Time VM 액세스 제어를 적용하고 권한 있는 사용자가 제한된 시간 동안 동일한 메커니즘을 통해 열 수 있도록 활성화합니다.|
|Machine|20|인터넷 엔드포인트를 통한 액세스 제한|기존 허용 규칙의 액세스를 제한하여 인터넷 연결 VM의 네트워크 보안 그룹을 강화합니다.|
|Machine|10|차세대 방화벽 추가|NGFW(차세대 방화벽) 솔루션을 추가하여 인터넷 연결 VM의 보호를 강화합니다.|
|Machine|5|네트워크 게이트웨이 방화벽을 통해서만 트래픽 라우팅|차세대 방화벽 솔루션 배포를 완료하려면 사용자의 보호된 인터넷 연결 VM으로 가는 트래픽을 차세대 방화벽 솔루션을 통해서만 라우팅해야 합니다.|
|VNet|5|DDoS Protection 표준 사용|이러한 가상 네트워크에서 공용 IP가 있는 애플리케이션은 DDOS 보호 서비스 표준으로 보호되어 있지 않으므로, 대규모 네트워크 및 프로토콜 공격을 완화하기 위해 이 표준을 사용하는 것이 좋습니다.|

## <a name="see-also"></a>참고 항목
다른 Azure 리소스 유형에 적용되는 권장 사항에 대해 자세히 알아보려면 다음을 참조하세요.

* [Azure Security Center에서 가상 머신 보호](security-center-virtual-machine-recommendations.md)
* [Azure Security Center에서 애플리케이션 보호](security-center-application-recommendations.md)
* [Azure Security Center에서 Azure SQL 서비스 보호](security-center-sql-service-recommendations.md)

보안 센터에 대한 자세한 내용은 다음을 참조하세요.

* [Azure Security Center에서 보안 정책 설정](tutorial-security-policy.md) -- Azure 구독 및 리소스 그룹에 대해 보안 정책을 구성하는 방법을 알아봅니다.
* [Azure Security Center에서 보안 경고 관리 및 대응](security-center-managing-and-responding-alerts.md) - 보안 경고를 관리하고 대응하는 방법을 알아봅니다.
* [Azure Security Center FAQ](security-center-faq.md) - 서비스 사용에 관한 질문과 대답을 찾습니다.
