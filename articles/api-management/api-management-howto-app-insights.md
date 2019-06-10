---
title: Azure Application Insights와 Azure API Management를 통합하는 방법 | Microsoft Docs
description: Azure Application Insights에서 Azure API Management의 이벤트를 기록하고 보는 방법을 알아봅니다.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: apimpm
ms.openlocfilehash: 3bbab82831fba389cd4bf172e7ea762d5971579b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66241837"
---
# <a name="how-to-integrate-azure-api-management-with-azure-application-insights"></a>Azure Application Insights와 Azure API Management를 통합하는 방법

Azure API Management는 Azure Application Insights와 쉽게 통합할 수 있습니다. Azure Application Insights는 웹 개발자가 여러 플랫폼에서 애플리케이션을 구축하고 관리할 수 있는 확장 가능한 서비스입니다. 이 가이드에서는 이러한 통합의 모든 단계를 안내하고, API Management 서비스 인스턴스의 성능에 미치는 영향을 줄이기 위한 전략에 대해 설명합니다.

## <a name="prerequisites"></a>필수 조건

이 가이드를 수행하려면 Azure API Management 인스턴스가 있어야 합니다. 아직 없으면 먼저 [자습서](get-started-create-service-instance.md)를 완료하세요.

## <a name="create-an-azure-application-insights-instance"></a>Azure Application Insights 인스턴스 만들기

Azure Application Insight를 사용하려면 먼저 서비스의 인스턴스를 만들어야 합니다.

1. **Azure Portal**을 열고 **Application Insights**로 이동합니다.  
    ![Application Insights 만들기](media/api-management-howto-app-insights/apim-app-insights-instance-1.png)  
2. **+ 추가**를 클릭합니다.  
    ![Application Insights 만들기](media/api-management-howto-app-insights/apim-app-insights-instance-2.png)  
3. 양식을 채웁니다. **애플리케이션 유형**으로 **일반**을 선택합니다.
4. **만들기**를 클릭합니다.

## <a name="create-a-connection-between-azure-application-insights-and-azure-api-management-service-instance"></a>Azure Application Insight 및 Azure API Management 서비스 인스턴스 간 연결 만들기

1. **Azure Portal**에서 **Azure API Management 서비스 인스턴스**로 이동합니다.
2. 왼쪽 메뉴에서 **Application Insights**를 선택합니다.
3. **+ 추가**를 클릭합니다.  
    ![Application Insights 로거](media/api-management-howto-app-insights/apim-app-insights-logger-1.png)  
4. 앞에서 만든 **Application Insights** 인스턴스를 선택하고 간단한 설명을 제공합니다.
5. **만들기**를 클릭합니다.
6. 방금 계측 키가 있는 Azure Application Insights 로거를 만들었습니다. 이제 목록에 표시됩니다.  
    ![Application Insights 로거](media/api-management-howto-app-insights/apim-app-insights-logger-2.png)  

> [!NOTE]
> 내부적으로 Application Insights 인스턴스의 계측 키를 포함하는 API Management 인스턴스에 [로거](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/logger/createorupdate) 엔터티가 만들어집니다.

## <a name="enable-application-insights-logging-for-your-api"></a>API에 대한 Application Insights 로깅 사용

1. **Azure Portal**에서 **Azure API Management 서비스 인스턴스**로 이동합니다.
2. 왼쪽 메뉴에서 **API**를 선택합니다.
3. API를 클릭합니다(여기서는 **데모 회의 API**).
4. 위쪽 표시줄의 **설정** 탭으로 이동합니다.
5. **진단 로그** 섹션까지 아래로 스크롤합니다.  
    ![Application Insights 로거](media/api-management-howto-app-insights/apim-app-insights-api-1.png)  
6. **사용** 확인란을 선택합니다.
7. **대상** 드롭다운에서 연결된 로거를 선택합니다.
8. **샘플링(%)** 으로 **100**을 입력하고, **항상 오류 로깅** 확인란을 선택합니다.
9. **저장**을 클릭합니다.

> [!WARNING]
> **본문의 첫 번째 바이트** 필드에서 기본값 **0**을 재정의하면 API의 성능이 현저히 저하될 수 있습니다.

> [!NOTE]
> 내부적으로 이름이 'applicationinsights'인 [진단](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/diagnostic/createorupdate) 엔터티가 API 수준에서 만들어집니다.

| 설정 이름                        | 값 유형                        | 설명                                                                                                                                                                                                                                                                                                                                      |
|-------------------------------------|-----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 사용                              | 부울                           | 이 API의 로깅이 사용되는지 여부를 지정합니다.                                                                                                                                                                                                                                                                                                |
| 대상                         | Azure Application Insights 로거 | 사용할 Azure Application Insights 로거를 지정합니다.                                                                                                                                                                                                                                                                                           |
| 샘플링(%)                        | Decimal                           | 0-100의 값(퍼센트)입니다. <br/> Azure Application Insights에 기록할 요청의 백분율을 지정합니다. 0% 샘플링은 기록되는 요청이 없음을 의미하고, 100%는 모든 요청이 기록됨을 의미합니다. <br/> 이 설정은 Azure Application Insights에 대한 로깅 요청으로 인해 성능에 미치는 영향을 줄이기 위해 사용됩니다(아래 섹션 참조). |
| 항상 오류 로깅                   | 부울                           | 이 설정을 선택하면 **샘플링** 설정에 관계없이 모든 오류가 Azure Application Insights에 기록됩니다.                                                                                                                                                                                                                  |
| 기본 옵션: 헤더              | list                              | 요청 및 응답에 대해 Azure Application Insights에 기록할 헤더를 지정합니다.  기본값: 헤더가 기록되지 않음                                                                                                                                                                                                             |
| 기본 옵션: 본문의 첫 번째 바이트  | 정수                           | 요청 및 응답에 대해 Azure Application Insights에 기록할 본문의 첫 번째 바이트 수를 지정합니다.  기본값: 본문이 기록되지 않음                                                                                                                                                                                              |
| 고급 옵션: 프런트 엔드 요청  |                                   | *프런트 엔드 요청*이 Azure Application Insights에 기록되는지 여부와 방법을 지정합니다. *프런트 엔드 요청*은 Azure API Management 서비스로 들어오는 요청입니다.                                                                                                                                                                        |
| 고급 옵션: 프런트 엔드 응답 |                                   | *프런트 엔드 응답*이 Azure Application Insights에 기록되는지 여부와 방법을 지정합니다. *프런트 엔드 응답*은 Azure API Management 서비스에서 나가는 응답입니다.                                                                                                                                                                   |
| 고급 옵션: 백 엔드 요청   |                                   | *백 엔드 요청*이 Azure Application Insights에 기록되는지 여부와 방법을 지정합니다. *백 엔드 요청*은 Azure API Management 서비스에서 나가는 요청입니다.                                                                                                                                                                        |
| 고급 옵션: 백 엔드 응답  |                                   | *백 엔드 응답*이 Azure Application Insights에 기록되는지 여부와 방법을 지정합니다. *백 엔드 응답*은 Azure API Management 서비스로 들어오는 응답입니다.                                                                                                                                                                       |

> [!NOTE]
> 로거(단일 API 로거 또는 모든 API에 대한 로거)는 서로 다른 수준에서 지정할 수 있습니다.
>  
> 둘 다 지정하는 경우:
> + 서로 다른 로거이면 둘 다 사용됩니다(로그 멀티플렉싱).
> + 동일한 로거이지만 서로 다른 설정을 사용하면 단일 API(세분화된 수준)에 대한 로거가 모든 API에 대한 로거를 재정의합니다.

## <a name="what-data-is-added-to-azure-application-insights"></a>Azure Application Insights에 추가되는 데이터

Azure Application Insights에서 받는 항목은 다음과 같습니다.

+ 들어오는 모든 요청(*프런트 엔드 요청*, *프런트 엔드 응답*)에 대한 *요청* 원격 분석 항목
+ 백 엔드 서비스(*백 엔드 요청*, *백 엔드 응답*)에 전달되는 모든 요청에 대한 *종속성* 원격 분석 항목
+ 실패한 모든 요청에 대한 *예외* 원격 분석 항목

실패한 요청은 다음과 같은 요청입니다.

+ 닫힌 클라이언트 연결로 인해 실패한 요청, 또는
+ API 정책의 *on-error*(오류 시) 섹션을 트리거한 요청, 또는
+ 4xx 또는 5xx와 일치하는 응답 HTTP 상태 코드가 있는 요청

## <a name="performance-implications-and-log-sampling"></a>성능에 미치는 영향 및 로그 샘플링

> [!WARNING]
> 모든 이벤트를 기록하면 들어오는 요청 속도에 따라 성능에 심각한 영향을 미칠 수 있습니다.

내부 부하 테스트에 따라 이 기능을 사용하면 요청 속도가 초당 1,000개 요청을 초과할 때 처리량이 40-50% 감소합니다. Azure Application Insights는 애플리케이션 성능을 평가하기 위해 통계 분석을 사용하도록 설계되었습니다. 감사 시스템이 아니며, 대용량 API에 대한 개별 요청 각각을 기록하는 데 적합하지 않습니다.

**샘플링** 설정을 조정하여 기록되는 요청 수를 조작할 수 있습니다(위의 단계 참조). 100% 값은 모든 요청이 기록됨을 의미하고, 0%는 전혀 기록하지 않음을 의미합니다. **샘플링**을 사용하면 원격 분석의 양을 줄이고, 심각한 성능 저하를 효과적으로 방지하면서 로깅의 이점을 계속 유지할 수 있습니다.

또한 요청의 헤더와 본문에 대한 로깅을 건너뛰면 성능 문제를 완화하는 데 긍정적인 영향을 줍니다.

## <a name="video"></a>비디오

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2pkXv]
>
>

## <a name="next-steps"></a>다음 단계

+ [Azure Application Insights](https://docs.microsoft.com/azure/application-insights/)에 대해 자세히 알아봅니다.
+ [Azure Event Hubs에 로깅](api-management-howto-log-event-hubs.md)을 살펴봅니다.
