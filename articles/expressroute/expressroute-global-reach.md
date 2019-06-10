---
title: Global Reach를 사용하여 Microsoft Cloud에 온-프레미스 네트워크 연결 - Azure ExpressRoute | Microsoft Docs
description: 이 문서에서는 ExpressRoute Global Reach에 대해 설명합니다.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: 28cfbc7183628b00c463f1c3a8a954ba4074409b
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65415969"
---
# <a name="expressroute-global-reach"></a>ExpressRoute에 대 한 글로벌 정보
ExpressRoute는 온-프레미스 네트워크를 Microsoft 클라우드에 연결하기 위한 프라이빗이고 탄력적인 방법입니다. 프라이빗 데이터 센터 또는 회사 네트워크에서 Azure, Office 365 및 Dynamics 365와 같은 다양한 Microsoft 클라우드 서비스에 액세스할 수 있습니다. 예를 들어, 실리콘밸리의 ExpressRoute 회로를 사용하는 샌프란시스코 지사와, 런던의 ExpressRoute 회로를 사용하는 런던 지사가 있을 수 있습니다. 두 지사는 모두 미국 서부와 영국 남부에서 Azure 리소스에 고속으로 연결할 수 있습니다. 그러나 해당 지점은 서로 데이터를 직접 교환할 수 없습니다. 즉, 10.0.1.0/24는 10.0.3.0/24 및 10.0.4.0/24에 데이터를 보낼 수 있지만, 10.0.2.0/24에는 보낼 수 없습니다.

![사용 안 함][1]

**ExpressRoute Global Reach**를 사용하면 온-프레미스 네트워크 간의 개인 네트워크를 설정하기 위해 ExpressRoute 회로를 함께 연결할 수 있습니다. 위의 예제에서 ExpressRoute Global Reach가 추가됨에 따라 샌프란시스코 사무실(10.0.1.0/24)은 기존 ExpressRoute 회로 및 Microsoft의 글로벌 네트워크를 통해 런던 사무실(10.0.2.0/24)과 데이터를 교환할 수 있습니다. 

![사용][2]

## <a name="use-case"></a>사용 사례
ExpressRoute Global Reach는 서비스 공급자의 WAN 구현을 보완하고 전 세계에 있는 지점을 연결하도록 설계되었습니다. 예를 들어, 서비스 공급자가 미국에서 주로 운영되고 미국 내 모든 지점을 연결했지만, 서비스 공급자가 일본과 홍콩에서는 운영되지 않는 경우, ExpressRoute Global Reach를 사용하여 로컬 서비스 공급자와 작업할 수 있고 Microsoft는 ExpressRoute와 글로벌 네트워크를 사용하여 해당 지역의 지점과 미국 내 지점을 연결합니다.

![사용 사례][3]

## <a name="availability"></a>가용성 
현재 ExpressRoute Global Reach는 다음 위치에서 지원됩니다.

* 오스트레일리아
* 캐나다
* 프랑스
* 독일
* 홍콩 특별 행정구
* 아일랜드
* 일본
* 한국
* 네덜란드
* 싱가포르
* 스위스
* 영국
* 미국

ExpressRoute 회로는 위의 국가 또는 지역에 있는 [ExpressRoute 피어링 위치](expressroute-locations.md)에서 만들어야 합니다. [서로 다른 지리적 지역](expressroute-locations.md) 간에 ExpressRoute Global Reach를 사용하도록 설정하려면 회로가 프리미엄 SKU여야 합니다.

## <a name="next-steps"></a>다음 단계
1. [ExpressRoute Global Reach에 대해 자세히 알아봅니다.](expressroute-faqs.md)
2. [ExpressRoute Global Reach 사용 방법](expressroute-howto-set-global-reach.md)
3. [Azure 가상 네트워크에 ExpressRoute 회로 연결](expressroute-howto-linkvnet-arm.md)


<!--Image References-->
[1]: ./media/expressroute-global-reach/1.png "글로벌 환경을 사용하지 않는 다이어그램"
[2]: ./media/expressroute-global-reach/2.png "글로벌 환경을 사용하는 다이어그램"
[3]: ./media/expressroute-global-reach/3.png "글로벌 환경의 사용 사례"
