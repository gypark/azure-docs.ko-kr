---
title: Azure Database for MariaDB의 고가용성 개념
description: 이 항목에서는 Azure Database for MariaDB를 사용하는 경우 고가용성에 대한 정보를 제공합니다.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 12/03/2018
ms.openlocfilehash: ba561cba21b0b83b6a19fffc2fdfebe8ec8b8ed9
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/27/2019
ms.locfileid: "61042149"
---
# <a name="high-availability-concepts-in-azure-database-for-mariadb"></a>Azure Database for MariaDB의 고가용성 개념
Azure Database for MariaDB 서비스는 높은 수준의 가용성을 보장합니다. 재정적으로 지원되는 SLA(서비스 수준 계약)는 일반 공급 시 99.99%입니다. 이 서비스를 사용할 때는 애플리케이션 작동 중단 시간이 거의 없습니다.

## <a name="high-availability"></a>고가용성
HA(고가용성) 모델은 노드 수준의 중단이 발생할 때 기본 제공되는 장애 조치(failover) 메커니즘을 기반으로 합니다. 하드웨어 오류 또는 서비스 배포에 대한 응답으로 인해 노드 수준 중단이 발생할 수 있습니다.

항상 Azure Database for MariaDB 데이터베이스 서버에 대한 변경 내용은 트랜잭션의 컨텍스트에서 발생합니다. 변경 내용은 트랜잭션이 커밋될 때 Azure Storage에 동기적으로 기록됩니다. 노드 수준의 중단이 발생하면 데이터베이스 서버에서 자동으로 새 노드를 만들고 데이터 저장소를 새 노드에 연결합니다. 모든 활성 연결이 끊어지고 처리 중인 트랜잭션이 커밋되지 않습니다.

## <a name="application-retry-logic-is-essential"></a>애플리케이션 재시도 논리는 필수적
MariaDB 데이터베이스 애플리케이션은 끊어진 연결과 실패한 트랜잭션을 감지하고 재시도하도록 빌드되었다는 점이 중요합니다. 애플리케이션이 재시도할 때 애플리케이션의 연결은 새로 생성된 인스턴스로 투명하게 리디렉션되며 이 인스턴스는 실패한 인스턴스를 대신합니다.

게이트웨이는 Azure에서 내부적으로 새 인스턴스로 연결을 리디렉션하는 데 사용됩니다. 중단이 발생하면 전체 장애 조치(failover) 프로세스에 일반적으로 수십 초 소요됩니다. 리디렉션은 게이트웨이에 의해 내부적으로 처리되므로 외부 연결 문자열은 클라이언트 애플리케이션에 대해 동일하게 유지됩니다.

## <a name="scaling-up-or-down"></a>확장 또는 축소
HA 모델과 마찬가지로 Azure Database for MariaDB의 크기를 늘리거나 줄일 때 지정된 크기의 새 서버 인스턴스가 만들어집니다. 기존 데이터 저장소는 원래 인스턴스에서 분리되고 새 인스턴스에 연결됩니다.

크기 조정 작업 중 데이터베이스 연결에 대한 중단이 발생합니다. 클라이언트 애플리케이션의 연결이 끊어지고, 열려 있고 커밋되지 않은 트랜잭션은 취소됩니다. 클라이언트 애플리케이션이 연결을 다시 시도하거나 새 연결을 만들면 게이트웨이는 새로 크기가 지정된 인스턴스로 연결을 지정합니다.

## <a name="next-steps"></a>다음 단계
- 서비스 개요는  [Azure Database for MariaDB 개요](overview.md) 참조
