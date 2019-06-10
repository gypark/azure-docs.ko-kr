---
title: Azure Monitor 로그에 대 한 사용량 및 비용 관리 | Microsoft Docs
description: 가격 책정 계획을 변경 하 고 Azure Monitor에서 Log Analytics 작업 영역에 대 한 데이터 볼륨 및 보존 정책을 관리 하는 방법에 알아봅니다.
services: azure-monitor
documentationcenter: azure-monitor
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: magoedte
ms.subservice: ''
ms.openlocfilehash: 8eeb29b2d1fe17ae5581dab81c34d5c2c635a6c2
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66496336"
---
# <a name="manage-usage-and-costs-with-azure-monitor-logs"></a>Azure Monitor 로그를 통해 사용량 및 비용 관리

> [!NOTE]
> 이 문서에서는 Log Analytics 작업 영역에 대 한 데이터 보존 기간을 설정 하 여 Azure Monitor에서 비용을 제어 하는 방법을 설명 합니다.  관련 내용은 다음 문서를 참조 하세요.
> - [사용량 및 예상 비용 모니터링](usage-estimated-costs.md)에서는 다른 가격 책정 모델에 대해 여러 Azure 모니터링 기능에서 사용량 및 예상 비용을 보는 방법을 설명합니다. 또한 가격 책정 모델을 변경하는 방법을 설명합니다.

Azure Monitor 로그 크기 조정 및 지원 수집, 인덱싱 및 기업에서 엄청난 양의 하루 모든 원본의 데이터를 저장 하도록 설계 되었거나 Azure에 배포 됩니다.  조직에 대한 주 드라이버인 반면 비용 효율성은 궁극적으로 기본 드라이버입니다. 가를 위해 Log Analytics 작업 영역 비용은 선택한 계획에 따라 이기도 하 고 연결 된 소스에서 생성 된 데이터를 저장 하려고 선택한 기간에 수집한 데이터의 볼륨에만 기반 되지 않습니다는 이해 해야 합니다.  

이 문서에서는 데이터 볼륨 및 저장소 증가를 사전에 모니터링하는 방법 및 해당 관련 비용을 제어하려면 제한을 정의하는 방법을 검토합니다. 

데이터 비용은 다음 요인에 따라 상당히 좌우될 수 있습니다. 

- 생성되고 작업 영역에 수집된 데이터 볼륨 
    - 사용하도록 설정된 관리 솔루션 수
    - 모니터링된 시스템 수
    - 모니터링된 각 리소스에서 수집된 데이터 형식 
- 데이터를 보존하기로 결정하는 시간 

## <a name="understand-your-workspaces-usage-and-estimated-cost"></a>작업 영역의 사용량 및 예상 비용 파악

Azure Monitor 로그 사용 하면 쉽게 이해 하는 비용 가능성이 최근 사용 패턴에 기반 합니다. 이 위해 사용 하 여 **Log Analytics 사용량 및 예상 비용** 검토 하 고 데이터 사용량을 분석 합니다. 솔루션별로 수집되는 데이터양, 보유 중인 데이터양, 수집된 데이터양과 포함된 양을 초과하여 추가로 보유 중인 데이터양을 기준으로 한 추정 비용이 표시됩니다.

![사용량 및 예상 비용](media/manage-cost-storage/usage-estimated-cost-dashboard-01.png)

데이터를 더 자세히 탐색하려면 **사용량 및 예상 비용** 페이지의 차트 중 하나의 오른쪽 상단 모서리에 있는 아이콘을 클릭합니다. 이제 이 쿼리에 대해 작업하여 사용량을 자세히 살펴볼 수 있습니다.  

![로그 보기](media/manage-cost-storage/logs.png)

**사용량 및 예상 비용** 페이지에서 해당 월의 데이터 볼륨을 검토할 수 있습니다. 여기에는 Log Analytics 작업 영역에 수신되고 보존된 모든 데이터가 포함됩니다.  클릭 **사용량 세부 정보** 원본, 컴퓨터 및 제품 데이터 볼륨 추세 정보를 사용 하 여 사용량 대시보드를 보려면 페이지 맨 위에서 합니다. 보존 기간을 수정하거나 일일 한도를 설정하고 확인하려면 **데이터 볼륨 관리**를 클릭합니다.
 
Log Analytics 요금은 Azure 청구서에 추가됩니다. Azure 청구서의 자세한 내용은 Azure Portal의 청구 섹션 또는 [Azure 청구 포털](https://account.windowsazure.com/Subscriptions)에서 참고할 수 있습니다.  

## <a name="daily-cap"></a>일일 한도

작업 영역에 대한 일일 한도를 구성하고 일일 수집량을 제한할 수 있지만 목표치가 일일 한도에 도달하지 않도록 주의하십시오.  그렇지 않으면 남은 기간 동안의 데이터가 손실됩니다. 이는 해당 기능이 작업 영역에서 사용할 수 있는 최신 데이터에 의존할 수도 있는 다른 Azure 서비스 및 솔루션에 영향을 줄 수 있습니다.  결과적으로 리소스의 상태 조건이 IT 서비스를 지원할 때 경고를 관찰하고 수신하는 기능이 영향을 받습니다.  일일 한도 관리 되는 리소스에서 데이터 볼륨의 예기치 않은 증가 관리 하 고 작업 영역에 대 한 계획 되지 않은 요금을 제한 하려는 경우 또는 고 한도 내에서 유지 하는 방법으로 사용할 것입니다.  

일일 한도에 도달하면 하루의 나머지 시간 동안 청구 가능한 데이터 형식의 수집을 중지합니다. 선택된 Log Analytics 작업 영역에 대한 페이지의 상단에 경고 배너가 표시되고 작업 이벤트가 **LogManagement** 범주 아래의 *작업* 테이블로 전송됩니다. *일일 한도 아래 정의된 재설정 시간이* 로 설정된 후 데이터 수집이 다시 시작합니다. 일일 데이터 한도에 도달했을 때 알려주도록 구성된 이 작업 이벤트를 기반으로 경고 규칙을 정의하는 것이 좋습니다. 

> [!NOTE]
> 일일 한도 Azure Security Center에서 데이터 수집을 중지 하지 않습니다.

### <a name="identify-what-daily-data-limit-to-define"></a>정의할 일일 데이터 한도 식별

데이터 수집 추세 및 정의할 일일 볼륨 한도를 이해하려면 [Log Analytics 사용량 및 예상 비용](usage-estimated-costs.md)을 검토합니다. 한도에 도달한 후에는 리소스를 모니터링할 수 없으므로 신중하게 고려해야 합니다. 

### <a name="manage-the-maximum-daily-data-volume"></a>최대 일일 데이터 볼륨 관리

다음 단계를 하루 Log Analytics 작업 영역에서 수집 하는 데이터의 볼륨 관리 제한을 구성 하는 방법에 설명 합니다.  

1. 작업 영역의 왼쪽 창에서 **사용량 및 예상 비용**을 선택합니다.
2. 선택한 작업 영역에 대한 **사용량 및 예상 비용** 창의 페이지의 상단에서 **데이터 볼륨 관리**를 클릭합니다. 
3. 일일 한도는 기본으로 **OFF**이며 이를 사용하도록 설정하려면 **ON**을 클릭한 다음, 데이터 볼륨 한도를 GB/일로 설정합니다.

    ![Log Analytics 데이터 제한 구성](media/manage-cost-storage/set-daily-volume-cap-01.png)

### <a name="alert-when-daily-cap-reached"></a>일일 상한에 도달한 경우 경고

데이터 제한 임계값에 도달하는 경우 Azure Portal에 시각적 큐를 표시하는 반면, 이 동작은 즉각적인 주의가 필요한 운영 문제를 관리하는 방법에 맞출 필요는 없습니다.  경고 알림을 수신하려면 Azure Monitor에서 새 경고 규칙을 만들 수 있습니다.  자세한 내용은 참조 하세요 [만들기, 보기 및 경고를 관리 하는 방법을](alerts-metric.md)합니다.

시작하려면 경고에 대한 권장 설정은 다음과 같습니다.

- 대상: Log Analytics 리소스 선택
- 조건: 
   - 신호 이름: 사용자 지정 로그 검색
   - 검색 쿼리: 작업 | 세부 정보에 'OverQuota'가 있는 경우
   - 기준: 결과의 수
   - 조건: 초과
   - 임계값: 0
   - 기간: 5분
   - 빈도: 5분
- 경고 규칙 이름: 일일 데이터 한계 도달
- 심각도: 경고(심각도 1)

경고가 정의되고 한계에 도달하면 경고가 트리거되고 작업 그룹에서 정의된 응답을 수행합니다. 이메일 및 텍스트 메시지를 통해 팀에 알리거나 웹후크, Automation Runbook 또는 [외부 ITSM 솔루션을 사용해 통합](itsmc-overview.md#create-itsm-work-items-from-azure-alerts)을 사용하여 작업을 자동화할 수 있습니다. 

## <a name="change-the-data-retention-period"></a>데이터 보존 기간 변경

다음 단계에서는 로그 데이터가 작업 영역에 보존되는 기간을 구성하는 방법을 설명합니다.
 
1. 작업 영역의 왼쪽 창에서 **사용량 및 예상 비용**을 선택합니다.
2. **사용량 및 예상 비용** 창의 페이지의 상단에서 **데이터 볼륨 관리**를 클릭합니다.
3. 창에서 슬라이더를 이동하여 일 수를 늘리거나 줄인 다음, **확인**을 클릭합니다.  *무료* 계층에서 작업 중인 경우 데이터 보존 기간을 수정할 수 없으며 이 설정을 제어하기 위해 유료 계층으로 업그레이드해야 합니다.

    ![작업 영역 데이터 보존 설정 변경](media/manage-cost-storage/manage-cost-change-retention-01.png)

## <a name="legacy-pricing-tiers"></a>레거시 가격 책정 계층

2018 년 4 월 2 일 이전에 Log Analytics 작업 영역 또는 Application Insights 리소스를 사용 했던 또는 2019 년 2 월 1 일 전에 시작 된 기업 계약에 연결 된 구독을 레거시 가격 책정 계층에 액세스할 수 있도록 계속 됩니다. 무료, 독립 실행형 (GB 당) 및 노드당 (OMS).  무료 가격 책정 계층에서에서 작업 영역에는 일일 데이터 수집 (Azure Security Center에서 수집 된 보안 데이터 형식) 제외 500MB로 제한 해야 하 고 데이터 보존 기간은 7 일로 제한 합니다. 무료 가격 책정 계층은 평가 목적 으로만 제공 합니다. 데이터 보존에 대 한 액세스 2 년 해야 하는 독립 실행형 클러스터 또는 노드당 가격 책정 계층에서 작업 영역입니다. 

> [!NOTE]
> System Center용 OMS E1 Suite, OMS E2 Suite 또는 OMS 추가 기능을 구매할 때 제공되는 자격을 사용하려면 Log Analytics의 *노드별* 가격 책정 계층을 선택합니다.

## <a name="changing-pricing-tier"></a>가격 책정 계층 변경

Log Analytics 작업 영역에 레거시 가격 책정 계층에 대한 액세스 권한이 있는 경우 레거시 가격 책정 계층 간을 변경하려면 다음을 수행합니다.

1. Azure Portal의 Log Analytics 구독 창에서 작업 영역을 선택합니다.

2. 작업 영역 창의 **일반**에서 **가격 책정 계층**을 선택합니다.  

3. **가격 책정 계층**에서 가격 책정 계층을 선택하고 **선택**을 클릭합니다.  
    ![선택된 가격 책정 계획](media/manage-cost-storage/workspace-pricing-tier-info.png)

구독 모니터링을 변경 해야 하는 현재 가격 책정 계층을 작업 영역을 이동 하려는 경우 [Azure Monitor에서 가격 책정 모델](usage-estimated-costs.md#moving-to-the-new-pricing-model) 있으며 해당 구독에서 모든 작업 영역 가격 책정 계층이 변경 됩니다.

> [!NOTE]
> 가격 책정 계층을 설정 하는 방법을 자세히 알아볼 수 있습니다 때 [는 Azure Resource Manager 템플릿을 사용 하 여](template-workspace-configuration.md#create-a-log-analytics-workspace) 작업 영역을 만들고 Azure Resource Manager 템플릿 배포 여부에 관계 없이 성공 한다는 사실을 확인 하는 구독이 기존 또는 새 가격 책정 모델입니다. 


## <a name="troubleshooting-why-log-analytics-is-no-longer-collecting-data"></a>Log Analytics에서 더 이상 데이터를 수집하지 않는 문제 해결

레거시 무료 가격 책정 계층을 사용 중이고 하루에 500MB 이상의 데이터를 보낸 경우 남은 날 동안 데이터 수집이 중지됩니다. 일일 한도에 도달하는 것은 Log Analytics가 데이터 수집을 중지하고 데이터가 사라진 것처럼 표시되는 일반적인 이유입니다.  Log Analytics는 데이터 수집을 시작하고 중지할 때 Operation 형식의 이벤트를 만듭니다. 일일 한도에 도달하고 데이터 누락이 있는지 확인하려면 검색에서 다음 쿼리를 실행합니다. 

```kusto
Operation | where OperationCategory == 'Data Collection Status'
```

데이터 수집이 중지 되는 경우 OperationStatus가 **경고**합니다. 데이터 수집이 시작 되는 경우 OperationStatus가 **Succeeded**합니다. 다음 표에서 데이터 수집을 중지하는 이유 및 데이터 수집을 다시 시작하는 권장되는 작업을 설명합니다.  

|수집 중지 이유| 해결 방법| 
|-----------------------|---------|
|레거시 무료 가격 책정 계층의 일일 한도에 도달함 |수집이 다음 날에 자동으로 다시 시작될 때까지 대기 또는 유료 가격 책정 계층으로 변경합니다.|
|작업 영역의 일일 상한에 도달함|수집이 자동으로 다시 시작될 때까지 대기하거나, 최대 일일 데이터 볼륨 관리의 설명처럼 일일 데이터 볼륨 한도를 늘립니다. 일일 상한 다시 설정 시간이 **데이터 볼륨 관리** 페이지에 표시됩니다. |
|Azure 구독이 다음으로 인해 일시 중단된 상태:<br> 평가판 종료<br> Azure 암호 만료<br> 월별 지출 한도 도달(예: MSDN 또는 Visual Studio 구독에서)|유료 구독으로 전환<br> 한도 제거 또는 한도가 재설정될 때까지 대기|

에 설명 된 단계를 사용 하 여 데이터 수집이 중지 될 때 알릴 *일일 데이터 한도 만들기* 데이터 수집이 중지 될 때 알림을 받으려면 경고 합니다. 에 설명 된 단계를 사용 하 여 [작업 그룹을 만들려면](action-groups.md) 경고 규칙에 대 한 전자 메일, 웹 후크 또는 runbook 작업을 구성 합니다. 

## <a name="troubleshooting-why-usage-is-higher-than-expected"></a>사용량이 예상보다 더 높은 원인 해결

사용량이 높은 원인은 다음과 같습니다.
- Log Analytics 작업 영역에 데이터를 전송 하는 데 예상 보다 더 많은 노드
- Log Analytics 작업 영역으로 전송 되 고 예상 보다 더 많은 데이터

## <a name="understanding-nodes-sending-data"></a>데이터를 전송 하는 노드 이해

지난 달에 매일 하트 비트를 보고 하는 컴퓨터의 수를 이해 하기 위해 사용 하 여

```kusto
Heartbeat | where TimeGenerated > startofday(ago(31d))
| summarize dcount(Computer) by bin(TimeGenerated, 1d)    
| render timechart
```

작업 영역을 레거시 노드당 가격 책정 계층에에서 있으면 노드로 청구 하는 컴퓨터의 목록을 가져오려면을 보내는 노드를 찾습니다 **데이터 형식 청구** (일부 데이터 형식은 무료). 이 작업을 수행 하려면 사용 합니다 `_IsBillable` [속성](log-standard-properties.md#_isbillable) 정규화 된 도메인 이름의 맨 왼쪽에 있는 필드를 사용 하 고 합니다. 이 청구 데이터를 사용 하 여 컴퓨터의 목록을 반환합니다.

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize TotalVolumeBytes=sum(_BilledSize) by computerName
```

표시 되는 청구 가능한 노드 수는 다음과 같은 작업으로 예상할 수 있습니다. 

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| billableNodes=dcount(computerName)
```

> [!NOTE]
> 여러 데이터 형식을 검색할 경우 비용이 많이 들기 때문에 이러한 `union withsource = tt *` 쿼리는 자주 사용하지 않도록 합니다. 이 쿼리는 사용 현황 데이터 형식 사용 하 여 컴퓨터 정보를 쿼리 하는 이전 방식으로 대체 합니다.  

어떤 실제로 청구처 보다 정확 하 게 계산 시간당 청구 데이터 형식을 전송 하는 컴퓨터의 수를 가져오는 것입니다. (레거시 노드당 가격 책정 계층에서 작업 영역에 대 한 Log Analytics를 계산 하는 시간 단위로 요금이 부과 하는 데 필요한 노드 수.) 

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize billableNodes=dcount(computerName) by bin(TimeGenerated, 1h) | sort by TimeGenerated asc
```

## <a name="understanding-ingested-data-volume"></a>이해 수집 된 데이터 볼륨

**사용량 및 예상 비용** 페이지에서 *솔루션당 데이터 수집* 차트는 전송된 총 데이터 양과 각 솔루션이 전송하는 데이터 양을 보여 줍니다. 이를 통해 전체 데이터 사용량(또는 특정 솔루션에 의한 사용량)이 증가하는지, 고정적인지, 감소하는지 여부의 추세를 판단할 수 있습니다. 이를 생성하는 데 사용되는 쿼리는 다음과 같습니다.

```kusto
Usage | where TimeGenerated > startofday(ago(31d))| where IsBillable == true
| summarize TotalVolumeGB = sum(Quantity) / 1024 by bin(TimeGenerated, 1d), Solution| render barchart
```

여기서 “where IsBillable = true” 절은 수집 비용이 없는 특정 솔루션에서 데이터 형식을 필터링합니다. 

데이터 추세를 보다 자세히 조사하여 특정 데이터 형식을 발견할 수 있습니다(예: IIS 로그로 인한 데이터를 연구하려는 경우).

```kusto
Usage | where TimeGenerated > startofday(ago(31d))| where IsBillable == true
| where DataType == "W3CIISLog"
| summarize TotalVolumeGB = sum(Quantity) / 1024 by bin(TimeGenerated, 1d), Solution| render barchart
```

### <a name="data-volume-by-computer"></a>컴퓨터별 데이터 볼륨

보려는 **크기** 컴퓨터 단위로 수집 되는 청구 가능한 이벤트를 사용 합니다 `_BilledSize` [속성](log-standard-properties.md#_billedsize), 크기 (바이트) 제공 하는:

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| summarize Bytes=sum(_BilledSize) by  computerName | sort by Bytes nulls last
```

합니다 `_IsBillable` [속성](log-standard-properties.md#_isbillable) 수집 된 데이터 요금이 발생 하는지 여부를 지정 합니다.

개수를 보려는 **청구 가능한** 컴퓨터 단위로 수집 된 이벤트 사용 

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| summarize eventCount=count() by computerName  | sort by count_ nulls last
```

특정 컴퓨터로 데이터를 전송하는 청구 가능 데이터 형식 수를 확인하려면 다음을 사용합니다.

```kusto
union withsource = tt *
| where Computer == "computer name"
| where _IsBillable == true 
| summarize count() by tt | sort by count_ nulls last
```

### <a name="data-volume-by-azure-resource-resource-group-or-subscription"></a>Azure 리소스, 리소스 그룹 또는 구독으로 데이터 볼륨

Azure에서 호스트 되는 노드에서 데이터를 가져올 수 있습니다는 **크기** 수집 하는 청구 가능한 이벤트 __컴퓨터당__, _ResourceId를 사용 하 여 [속성](log-standard-properties.md#_resourceid), 전체 경로를 제공 하는 리소스:

```kusto
union withsource = tt * 
| where _IsBillable == true 
| summarize Bytes=sum(_BilledSize) by _ResourceId | sort by Bytes nulls last
```

Azure에서 호스트 되는 노드에서 데이터를 가져올 수 있습니다는 **크기** 수집 하는 청구 가능한 이벤트 __Azure 구독 당__, 구문 분석을 `_ResourceId` 속성:

```kusto
union withsource = tt * 
| where _IsBillable == true 
| parse tolower(_ResourceId) with "/subscriptions/" subscriptionId "/resourcegroups/" 
    resourceGroup "/providers/" provider "/" resourceType "/" resourceName   
| summarize Bytes=sum(_BilledSize) by subscriptionId | sort by Bytes nulls last
```

변경 `subscriptionId` 에 `resourceGroup` Azure 리소스 그룹별로 청구 가능한 수집 된 데이터 볼륨을 표시 됩니다. 


> [!NOTE]
> 사용량 데이터 형식의 일부 필드가 여전히 스키마에 있지만 더 이상 사용되지 않으며 해당 값은 더 이상 채워지지 않습니다. 이는 **컴퓨터**일 뿐 아니라 수집과 관련된 필드(**TotalBatches**, **BatchesWithinSla**, **BatchesOutsideSla**, **BatchesCapped** 및 **AverageProcessingTimeMs**)이기도 합니다.

### <a name="querying-for-common-data-types"></a>일반적인 데이터 형식에 대 한 쿼리

특정 데이터 형식의 데이터 소스를 더 자세히 알아보려면 다음 예와 같은 몇 가지 쿼리를 사용합니다.

+ **보안** 솔루션
  - `SecurityEvent | summarize AggregatedValue = count() by EventID`
+ **로그 관리** 솔루션
  - `Usage | where Solution == "LogManagement" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | summarize AggregatedValue = count() by DataType`
+ **성능** 데이터 형식
  - `Perf | summarize AggregatedValue = count() by CounterPath`
  - `Perf | summarize AggregatedValue = count() by CounterName`
+ **이벤트** 데이터 형식
  - `Event | summarize AggregatedValue = count() by EventID`
  - `Event | summarize AggregatedValue = count() by EventLog, EventLevelName`
+ **Syslog** 데이터 형식
  - `Syslog | summarize AggregatedValue = count() by Facility, SeverityLevel`
  - `Syslog | summarize AggregatedValue = count() by ProcessName`
+ **AzureDiagnostics** 데이터 형식
  - `AzureDiagnostics | summarize AggregatedValue = count() by ResourceProvider, ResourceId`

### <a name="tips-for-reducing-data-volume"></a>데이터 볼륨을 줄이기 위한 팁

수집된 로그 볼륨을 줄이기 위한 몇 가지 제안 사항은 다음과 같습니다.

| 높은 데이터 볼륨의 소스 | 데이터 볼륨을 줄이는 방법 |
| -------------------------- | ------------------------- |
| 보안 이벤트            | [일반 또는 최소한의 보안 이벤트](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection#data-collection-tier)를 선택합니다. <br> 보안 감사 정책을 변경하여 필요한 이벤트만을 수집합니다. 특히, 다음 항목에 대한 이벤트를 수집할 필요를 검토합니다. <br> - [감사 필터링 플랫폼](https://technet.microsoft.com/library/dd772749(WS.10).aspx) <br> - [감사 레지스트리](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941614(v%3dws.10))<br> - [감사 파일 시스템](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772661(v%3dws.10))<br> - [감사 커널 개체](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941615(v%3dws.10))<br> - [감사 핸들 조작](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772626(v%3dws.10))<br> - 이동식 저장소 감사 |
| 성능 카운터       | [성능 카운터 구성](data-sources-performance-counters.md)을 다음과 같이 변경합니다. <br> - 컬렉션의 빈도 감소 <br> - 성능 카운터의 수 감소 |
| 이벤트 로그                 | [이벤트 로그 구성](data-sources-windows-events.md)을 다음과 같이 변경합니다. <br> - 수집된 이벤트 로그의 수 감소 <br> - 필수 이벤트 수준만 수집 예를 들어 *정보* 수준 이벤트를 수집하지 않습니다. |
| syslog                     | [syslog 구성](data-sources-syslog.md)을 다음과 같이 변경합니다. <br> - 수집된 기능의 수 감소 <br> - 필수 이벤트 수준만 수집 예를 들어 *정보* 및 *디버그* 수준 이벤트를 수집하지 않습니다. |
| AzureDiagnostics           | 다음 작업을 수행하도록 리소스 로그 컬렉션을 변경합니다. <br> - Log Analytics로 보내는 리소스 송신 로그의 수 축소 <br> - 필요한 로그만 수집 |
| 솔루션을 사용하지 않는 컴퓨터의 솔루션 데이터 | [솔루션 대상](../insights/solution-targeting.md)을 사용하여 필수 그룹의 컴퓨터에서 데이터를 수집합니다. |

### <a name="getting-security-and-automation-node-counts"></a>가져오는 보안 및 자동화 노드 수

“노드별 (OMS)” 가격 책정 계층에 속하는 경우 사용하는 노드 및 솔루션 수에 따라 비용이 청구됩니다. 요금이 청구되는 Insights 및 Analytics 노드의 수는 **사용량 및 예상 비용** 페이지의 표에 표시됩니다.  

고유한 보안 노드의 수를 보려면 다음 쿼리를 사용할 수 있습니다.

```kusto
union
(
    Heartbeat
    | where (Solutions has 'security' or Solutions has 'antimalware' or Solutions has 'securitycenter')
    | project Computer
),
(
    ProtectionStatus
    | where Computer !in~
    (
        (
            Heartbeat
            | project Computer
        )
    )
    | project Computer
)
| distinct Computer
| project lowComputer = tolower(Computer)
| distinct lowComputer
| count
```

고유한 Automation 노드의 수를 보려면 다음 쿼리를 사용합니다.

```kusto
 ConfigurationData 
 | where (ConfigDataType == "WindowsServices" or ConfigDataType == "Software" or ConfigDataType =="Daemons") 
 | extend lowComputer = tolower(Computer) | summarize by lowComputer 
 | join (
     Heartbeat 
       | where SCAgentChannel == "Direct"
       | extend lowComputer = tolower(Computer) | summarize by lowComputer, ComputerEnvironment
 ) on lowComputer
 | summarize count() by ComputerEnvironment | sort by ComputerEnvironment asc
```

## <a name="create-an-alert-when-data-collection-is-high"></a>데이터 수집은 높은 경우 경고를 만듭니다

이 섹션에서는 경고가 발생하는 경우를 설명합니다.
- 데이터 볼륨이 지정된 크기를 초과합니다.
- 데이터 볼륨이 지정된 크기를 초과할 것으로 예측됩니다.

검색 쿼리를 사용하는 Azure 경고 지원 [경고 로그](alerts-unified-log.md)입니다. 

최근 24시간 내에 수집된 데이터가 100GB를 초과하는 경우 다음 쿼리에 결과가 표시됩니다.

```kusto
union withsource = $table Usage 
| where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true 
| extend Type = $table | summarize DataGB = sum((Quantity / 1024)) by Type 
| where DataGB > 100
```

다음 쿼리는 간단한 수식을 사용하여 하루에 100GB를 초과하는 데이터가 전송되는 시기를 예측합니다. 

```kusto
union withsource = $table Usage 
| where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true 
| extend Type = $table 
| summarize EstimatedGB = sum(((Quantity * 8) / 1024)) by Type 
| where EstimatedGB > 100
```

다른 크기의 데이터 볼륨에 대해 경고하려면 쿼리의 100GB를 원하는 수로 변경합니다.

[새 로그 경고 만들기](alerts-metric.md)에 설명한 단계를 사용하여 데이터 컬렉션이 예상보다 높은 경우 알림을 받을 수 있습니다.

첫 번째 쿼리에 대한 경고를 만들 때 즉, 24시간 내에 데이터가 100GB를 초과하는 경우 다음을 설정합니다.  

- **경고 조건 정의**는 리소스 대상으로 Log Analytics 작업 영역을 지정합니다.
- **경고 조건**은 다음을 지정합니다.
   - **신호 이름**은 **로그 검색 사용자 지정**을 선택합니다.
   - **쿼리 검색**을 `union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize DataGB = sum((Quantity / 1024)) by Type | where DataGB > 100`으로
   - **경고 논리**는 *결과 수*에 **기반**하고 **조건**은 *0*의 **임계값**을 *초과*합니다.
   - 사용량 데이터가 시간당 한 번만 업데이트되므로 **기간**은 *1440*분이고 **주파수 경고**는 *60*분마다입니다.
- **경고 세부 정보 정의**는 다음을 지정합니다.
   - **이름**을 *24시간 내에 100GB를 초과하는 데이터 볼륨*으로
   - **심각도**를 *경고*로

기존 또는 새 [작업 그룹](action-groups.md)을 지정하거나 만들어서 로그 경고가 조건과 일치하는 경우 알려줍니다.

두 번째 쿼리에 대한 경고를 만들 때 즉, 24시간 내에 100GB를 초과하는 데이터가 예측되는 경우 다음을 설정합니다.

- **경고 조건 정의**는 리소스 대상으로 Log Analytics 작업 영역을 지정합니다.
- **경고 조건**은 다음을 지정합니다.
   - **신호 이름**은 **로그 검색 사용자 지정**을 선택합니다.
   - **쿼리 검색**을 `union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize EstimatedGB = sum(((Quantity * 8) / 1024)) by Type | where EstimatedGB > 100`으로
   - **경고 논리**는 *결과 수*에 **기반**하고 **조건**은 *0*의 **임계값**을 *초과*합니다.
   - 사용량 데이터가 시간당 한 번만 업데이트되므로 **기간**은 *180*분이고 **주파수 경고**는 *60*분마다입니다.
- **경고 세부 정보 정의**는 다음을 지정합니다.
   - **이름**을 *24시간 내에 100GB를 초과한다고 예측되는 데이터 볼륨*으로
   - **심각도**를 *경고*로

기존 또는 새 [작업 그룹](action-groups.md)을 지정하거나 만들어서 로그 경고가 조건과 일치하는 경우 알려줍니다.

경고를 수신하는 경우 사용량이 예상보다 더 높은 원인을 해결하려면 다음 섹션의 단계를 사용합니다.

## <a name="next-steps"></a>다음 단계

- 참조 [Azure Monitor 로그의 로그 검색](../log-query/log-query-overview.md) 검색 언어를 사용 하는 방법입니다. 사용 데이터에 대한 추가 분석을 수행하려면 검색 쿼리를 사용할 수 있습니다.
- [새 로그 경고 만들기](alerts-metric.md)에 설명한 단계를 사용하여 검색 기준이 충족되는 경우 알림을 받을 수 있습니다.
- [솔루션 대상](../insights/solution-targeting.md)을 사용하여 필수 그룹의 컴퓨터에서 데이터를 수집합니다.
- [Azure Security Center 필터링 정책](../../security-center/security-center-enable-data-collection.md)을 검토하여 효과적인 이벤트 컬렉션 정책을 구성합니다.
- [성능 카운터 구성](data-sources-performance-counters.md)을 변경합니다.
- 이벤트 컬렉션 설정을 수정하려면 [이벤트 로그 구성](data-sources-windows-events.md)을 검토합니다.
- syslog 컬렉션 설정을 수정하려면 [syslog 구성](data-sources-syslog.md)을 검토합니다.