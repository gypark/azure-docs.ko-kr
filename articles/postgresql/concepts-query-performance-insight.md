---
title: Azure Database for PostgreSQL-단일 서버에서에서 query Performance Insight
description: 이 문서에서는 PostgreSQL-단일 서버에 대 한 Azure Database에서 Query Performance Insight 기능을 설명 합니다.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: d45b79e2ca3b3d478102bebdcff3c8892bef2cb5
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65067552"
---
# <a name="query-performance-insight"></a>쿼리 

**적용 대상:** Azure Database for PostgreSQL-단일 서버 9.6 및 10

Query Performance Insight를 사용하면 가장 오랫동안 실행되는 쿼리, 쿼리가 시간의 경과에 따라 변경되는 방식 및 쿼리에 영향을 주는 대기 등을 빠르게 파악할 수 있습니다.

## <a name="permissions"></a>권한
Query Performance Insight에서 쿼리 텍스트를 보는 데 필요한 **소유자** 또는 **참가자** 권한입니다. **읽기 권한자**는 차트 및 표를 볼 수 있지만 쿼리 텍스트는 볼 수 없습니다.

## <a name="prerequisites"></a>필수 조건
Query Performance Insight가 작동하려면 [쿼리 저장소](concepts-query-store.md)에 데이터가 있어야 합니다.

## <a name="viewing-performance-insights"></a>Performance Insight 보기
Azure Portal의 [Query Performance Insight](concepts-query-performance-insight.md) 보기에는 쿼리 저장소의 핵심 정보가 시각화되어 표시됩니다. 

PostgreSQL 서버용 Azure Database의 포털 페이지에서 **지능형 성능** 메뉴 섹션 아래의 **Query Performance Insight**를 선택 합니다.

![Query Performance Insight 장기 실행 쿼리](./media/concepts-query-performance-insight/query-performance-insight-landing-page.png)

**장기 실행 쿼리** 탭은 15분 간격으로 집계된 실행당 평균 기간별 상위 5개 쿼리를 보여 줍니다. **쿼리 수** 드롭다운에서 선택하여 더 많은 쿼리를 볼 수 있습니다. 이 작업을 수행하면 특정 쿼리 ID에 대해 차트 색이 변경될 수 있습니다.

차트를 클릭하고 끌어 특정 기간으로 범위를 좁힐 수 있습니다. 확대/축소 아이콘을 사용하여 더 긴 기간이나 더 짧은 기간을 표시할 수도 있습니다.

차트 아래의 표에 해당 기간의 장기 실행 쿼리에 대한 자세한 내용이 나와 있습니다.

서버의 대기 쿼리를 시각화하려면 **대기 통계** 탭을 선택합니다.

![Query Performance Insight 대기 통계](./media/concepts-query-performance-insight/query-performance-insight-wait-statistics.png)

## <a name="next-steps"></a>다음 단계
- Azure Database for PostgreSQL의 [모니터링 및 튜닝](concepts-monitoring.md)에 대해 자세히 알아보세요.


