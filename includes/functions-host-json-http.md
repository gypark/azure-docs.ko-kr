---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 35b087d13099b975a1c9c6d2dbd449935f5f0d1d
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66248877"
---
```json
{
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 200,
        "maxConcurrentRequests": 100,
        "dynamicThrottlesEnabled": true
    }
}
```

|자산  |Default | 설명 |
|---------|---------|---------| 
|routePrefix|api|모든 경로에 적용되는 경로 접두사입니다. 기본 접두사를 제거하려면 빈 문자열을 사용하십시오. |
|maxOutstandingRequests|200<sup>*</sup>|지정된 시간에 보유할 미해결 요청의 최대 수입니다. 이 제한에는 대기 중이지만 실행이 시작되지 않은 요청과 진행 중인 모든 실행이 포함됩니다. 이 한도를 초과하여 들어오는 요청이 있으면 429 "Too Busy" 응답으로 거부됩니다. 그러면 호출자가 시간 기반 다시 시도 전략을 사용할 수 있고 최대 요청 대기 시간을 제어할 수 있습니다. 이 옵션은 스크립트 호스트 실행 경로 내에서 발생하는 큐만을 제어합니다. ASP.NET 요청 큐와 같은 다른 큐는 여전히 적용되며 이 설정의 영향을 받지 않습니다. <sup>*</sup>기본 버전 1.x 제한 하지 않습니다 (`-1`). 사용량 플랜에서 버전 2.x의 기본값은 200입니다. 기본 버전에 대 한 전용된 계획의 2.x는 바인딩된 (`-1`).|
|maxConcurrentRequests|100<sup>*</sup>|병렬로 실행될 http 함수의 최대 수입니다. 그러면 리소스 사용률을 관리하는 데 도움이 되는 동시성을 제어할 수 있습니다. 예를 들어 많은 시스템 리소스(메모리/cpu/소켓)를 사용하는 http 함수가 있으면 동시성이 너무 높은 문제가 발생할 수 있습니다. 또는 타사 서비스에 아웃바운드 요청을 하는 함수가 있는 경우 해당 호출의 속도가 제한되어야 합니다. 이러한 경우 여기에서 제한을 적용하는 것이 좋습니다. <sup>*</sup>기본 버전 1.x 제한 하지 않습니다 (`-1`). 사용량 플랜에서 버전 2.x의 기본값은 100입니다. 기본 버전에 대 한 전용된 계획의 2.x는 바인딩된 (`-1`).|
|dynamicThrottlesEnabled|true<sup>*</sup>|사용 설정되면, 이 설정은 요청 처리 파이프라인에서 주기적으로 시스템 성능 카운터(연결/스레드/프로세스/메모리/cpu/등)를 확인하고 해당 카운터 중 하나가 기본 제공 임계값(80%)을 초과하는 경우, 요청은 카운터가 일반 수준으로 반환될 때까지 429 "작업 초과" 응답을 표시하여 거부됩니다. <sup>*</sup>기본 버전 1.x는 false입니다. 사용량 플랜에서 버전 2.x의 기본값은 true입니다. 전용 플랜에서 버전 2.x의 기본값은 false입니다.|
