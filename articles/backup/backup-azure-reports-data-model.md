---
title: Azure Backup용 데이터 모델
description: 이 문서는 Azure Backup 보고서에 대한 Power BI 데이터 모델 정보에 대해 설명합니다.
services: backup
author: adigan
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 06/26/2017
ms.author: adigan
ms.openlocfilehash: c6160570644da108ba713e8229b38f9587495c92
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60337581"
---
# <a name="data-model-for-azure-backup-reports"></a>Azure Backup 보고서용 데이터 모델
이 문서에서는 Azure Backup 보고서를 만드는 데 사용되는 Power BI 데이터 모델을 설명합니다. 이 데이터 모델을 사용하여 관련 필드를 기반으로 하는 기존 보고서를 필터링할 수 있으며 무엇보다도 모델의 테이블 및 필드를 사용하여 사용자 고유의 보고서를 만들 수 있습니다. 

## <a name="creating-new-reports-in-power-bi"></a>Power BI에서 새 보고서 만들기
Power BI는 [데이터 모델을 사용하여 보고서를 만들](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/) 수 있는 사용자 지정 기능을 제공합니다.

## <a name="using-azure-backup-data-model"></a>Azure Backup 데이터 모델 사용
데이터 모델의 일부로 제공된 다음 필드를 사용하여 보고서를 만들고 기존 보고서를 사용자 지정할 수 있습니다.

### <a name="alert"></a>경고
이 표에서는 다양한 경고 관련 필드를 통해 기본 필드 및 집계를 제공합니다.

| 필드 | 데이터 형식 | 설명 |
| --- | --- | --- |
| #AlertsCreatedInPeriod |정수 |선택한 기간에서 만든 알림 수 |
| %ActiveAlertsCreatedInPeriod |백분율 |선택한 기간의 활성 경고 비율 |
| %CriticalAlertsCreatedInPeriod |백분율 |선택한 기간의 중요한 경고 비율 |
| AlertOccurrenceDate |Date |경고를 만든 날짜 |
| AlertSeverity |Text |경고의 심각도(예: 위험) |
| AlertStatus |Text |경고의 상태(예:활성) |
| AlertType |Text |생성된 경고의 형식(예: Backup) |
| AlertUniqueId |Text |생성된 경고의 고유 ID |
| AsOnDateTime |날짜/시간 |선택한 행에 대한 최신 새로 고침 시간 |
| AvgResolutionTimeInMinsForAlertsCreatedInPeriod |10진수 |선택한 기간 동안 경고를 해결하는 평균 시간(분) |
| EntityState |Text |경고 개체의 현재 상태(예: 활성, 삭제됨) |

### <a name="backup-item"></a>Backup 항목
이 표에서는 다양한 백업 항목 관련 필드를 통해 기본 필드 및 집계를 제공합니다.

| 필드 | 데이터 형식 | 설명 |
| --- | --- | --- |
| #BackupItems |정수 |백업 항목의 수 |
| #UnprotectedBackupItems |정수 |보호가 중지되었거나 백업이 구성되었지만 백업이 시작하지 않은 백업 항목의 수|
| AsOnDateTime |날짜/시간 |선택한 행에 대한 최신 새로 고침 시간 |
| BackupItemFriendlyName |Text |백업 항목의 친숙한 이름 |
| BackupItemId |Text |백업 항목의 ID |
| BackupItemName |Text |백업 항목의 이름 |
| BackupItemType |Text |백업 항목의 형식(예: VM, FileFolder) |
| EntityState |Text |백업 항목 개체의 현재 상태(예: 활성, 삭제됨) |
| LastBackupDateTime |날짜/시간 |선택한 백업 항목에 대한 마지막 백업 시간 |
| LastBackupState |Text |선택한 백업 항목에 대한 마지막 백업 상태(예: 성공, 실패) |
| LastSuccessfulBackupDateTime |날짜/시간 |선택한 백업 항목에 대한 마지막으로 성공한 백업 시간 |
| ProtectionState |Text |백업 항목의 현재 보호 상태(예: Protected, ProtectionStopped) |

### <a name="calendar"></a>Calendar
이 표에서는 일정 관련 필드에 대한 세부 정보를 제공합니다.

| 필드 | 데이터 형식 | 설명 |
| --- | --- | --- |
| Date |Date |데이터 필터링에 대해 선택한 날짜 |
| DateKey |Text |각 날짜 항목에 대한 고유 키 |
| DayDiff |10진수 |데이터 필터링에 대한 날의 차이점(예: 0은 현재 날짜의 데이터를 나타냄, -1은 이전 한 날짜의 데이터를 나타냄, 0 및 -1은 현재와 이전 날짜에 대한 데이터를 나타냄)  |
| 월 |Text |데이터 필터링에 선택한 연도의 월, 첫째 날짜에 시작하고 31일에 끝나는 월 |
| MonthDate | Date |데이터 필터링에 대해 선택한 월이 끝날 때 월의 날짜 |
| MonthDiff |10진수 |데이터 필터링에 대한 달의 차이점(예: 0은 현재 달의 데이터를 나타냄, -1은 이전 달의 데이터를 나타냄, 0 및 -1은 현재와 이전 달에 대한 데이터를 나타냄) |
| 주 |Text |데이터 필터링에 대해 선택한 주, 일요일에 시작하고 토요일에 끝나는 주 |
| WeekDate |Date |데이터 필터링에 대해 선택한 주가 끝날 때 주의 날짜 |
| WeekDiff |10진수 |데이터 필터링에 대한 주의 차이점(예: 0은 현재 주의 데이터를 나타냄, -1은 이전 주의 데이터를 나타냄, 0 및 -1은 현재와 이전 주에 대한 데이터를 나타냄) |
| Year |Text |데이터 필터링에 대해 선택한 연도 |
| YearDate |Date |데이터 필터링에 대해 선택한 연도가 끝날 때 연도의 날짜 |

### <a name="job"></a>작업
이 표에서는 다양한 작업 관련 필드를 통해 기본 필드 및 집계를 제공합니다.

| 필드 | 데이터 형식 | 설명 |
| --- | --- | --- |
| #JobsCreatedInPeriod |정수 |선택한 기간에서 만든 작업 수 |
| %FailuresForJobsCreatedInPeriod |백분율 |선택한 기간의 전체 작업 오류 백분율 |
| 80thPercentileDataTransferredInMBForBackupJobsCreatedInPeriod |10진수 |선택한 기간에서 만든 **백업** 작업에 대해 MB로 전송된 데이터의 80번째 백분위수 값 |
| AsOnDateTime |날짜/시간 |선택한 행에 대한 최신 새로 고침 시간 |
| AvgBackupDurationInMinsForJobsCreatedInPeriod |10진수 |선택한 기간에서 만든 **완료된 백업** 작업에 대한 평균 시간(분) |
| AvgRestoreDurationInMinsForJobsCreatedInPeriod |10진수 |선택한 기간에서 만든 **완료된 복원** 작업에 대한 평균 시간(분) |
| BackupStorageDestination |Text |백업 저장소의 대상(예: 클라우드, 디스크)  |
| EntityState |Text |작업 개체의 현재 상태(예: 활성, 삭제됨) |
| JobFailureCode |Text |발생한 작업 실패로 인한 오류 코드 문자열 |
| JobOperation |Text |실행되는 작업에 대한 동작(예: Backup, 복원, Backup 구성) |
| JobStartDate |Date |작업 실행이 시작된 날짜 |
| JobStartTime |Time |작업 실행이 시작된 시간 |
| JobStatus |Text |완료된 작업의 상태(예: 완료됨, 실패) |
| JobUniqueId |Text |작업을 식별하는 고유 ID |

### <a name="policy"></a>정책
이 표에서는 다양한 정책 관련 필드를 통해 기본 필드 및 집계를 제공합니다.

| 필드 | 데이터 형식 | 설명 |
| --- | --- | --- |
| #Policies |정수 |시스템에 있는 백업 정책 수 |
| #PoliciesInUse |정수 |백업 구성에 현재 사용되고 있는 정책 수 |
| AsOnDateTime |날짜/시간 |선택한 행에 대한 최신 새로 고침 시간 |
| BackupDaysOfTheWeek |Text |백업이 예약된 주의 요일 |
| BackupFrequency |Text |백업이 실행되는 빈도(예: 매일, 매주) |
| BackupTimes |Text |백업이 예약된 날짜 및 시간 |
| DailyRetentionDuration |정수 |구성된 백업에 대한 총 보존 기간(일) |
| DailyRetentionTimes |Text |매일 보존이 구성된 날짜 및 시간 |
| EntityState |Text |정책 개체의 현재 상태(예: 활성, 삭제됨) |
| MonthlyRetentionDaysOfTheMonth |Text |매월 보존에 대해 선택한 월의 날짜 |
| MonthlyRetentionDaysOfTheWeek |Text |매월 보존에 대해 선택한 주의 요일 |
| MonthlyRetentionDuration |10진수 |구성된 백업에 대한 총 보존 기간(월) |
| MonthlyRetentionFormat |Text |매월 보존에 대한 구성 형식(예: 요일 기반 매일, 주 기반 매주) |
| MonthlyRetentionTimes |Text |매월 보존이 구성된 날짜 및 시간 |
| MonthlyRetentionWeeksOfTheMonth |Text |매월 보존이 구성된 월의 주(예: 첫 번째, 마지막 등) |
| PolicyName |Text |지정된 정책의 이름 |
| PolicyUniqueId |Text |정책을 식별하는 고유 ID |
| RetentionType |Text |보존 정책의 형식(예: 매일, 매주, 매월, 매년) |
| WeeklyRetentionDaysOfTheWeek |Text |매주 보존에 대해 선택한 주의 요일 |
| WeeklyRetentionDuration |10진수 |구성된 백업에 대한 총 매주 보존 기간 |
| WeeklyRetentionTimes |Text |매주 보존이 구성된 날짜 및 시간 |
| YearlyRetentionDaysOfTheMonth |Text |매년 보존에 대해 선택한 월의 날짜 |
| YearlyRetentionDaysOfTheWeek |Text |매년 보존에 대해 선택한 주의 요일 |
| YearlyRetentionDuration |10진수 |구성된 백업에 대한 총 보존 기간(연) |
| YearlyRetentionFormat |Text |매년 보존에 대한 구성 형식(예: 요일 기반 매일, 주 기반 매주) |
| YearlyRetentionMonthsOfTheYear |Text |매년 보존에 대해 선택한 연도의 달 |
| YearlyRetentionTimes |Text |매년 보존이 구성된 날짜 및 시간 |
| YearlyRetentionWeeksOfTheMonth |Text |매년 보존이 구성된 월의 주(예: 첫 번째, 마지막 등) |

### <a name="protected-server"></a>보호된 서버
이 표에서는 다양한 보호된 서버 관련 필드를 통해 기본 필드 및 집계를 제공합니다.

| 필드 | 데이터 형식 | 설명 |
| --- | --- | --- |
| #ProtectedServers |정수 |보호된 서버의 수 |
| AsOnDateTime |날짜/시간 |선택한 행에 대한 최신 새로 고침 시간 |
| AzureBackupAgentOSType |Text |Azure Backup 에이전트의 OS 유형 |
| AzureBackupAgentOSVersion |Text |Azure Backup 에이전트의 OS 버전 |
| AzureBackupAgentUpdateDate |Text |Azure Backup 에이전트가 업데이트된 날짜 |
| AzureBackupAgentVersion |Text |Azure Backup 에이전트의 버전 번호 |
| BackupManagementType |Text |백업 수행에 대한 공급자 유형(예: IaaSVM, FileFolder) |
| EntityState |Text |보호된 서버 개체의 현재 상태(예: 활성, 삭제됨) |
| ProtectedServerFriendlyName |Text |보호된 서버의 친숙한 이름 |
| ProtectedServerName |Text |보호된 서버의 이름 |
| ProtectedServerType |Text |보호된 서버 백업 형식(예: IaaSVMContainer) |
| ProtectedServerName |Text |백업 항목이 속한 보호된 서버의 이름 |
| RegisteredContainerId |Text |백업에 대해 등록된 컨테이너의 ID |

### <a name="storage"></a>Storage
이 표에서는 다양한 저장소 관련 필드를 통해 기본 필드 및 집계를 제공합니다.

| 필드 | 데이터 형식 | 설명 |
| --- | --- | --- |
| #ProtectedInstances |10진수 |선택된 시간에 최신 값에 따라 계산된, 청구에서 프런트 엔드 저장소 계산에 사용된 보호된 인스턴스 수 |
| AsOnDateTime |날짜/시간 |선택한 행에 대한 최신 새로 고침 시간 |
| CloudStorageInMB |10진수 |선택된 시간에 최신 값에 따라 계산된, 백업에 사용된 클라우드 백업 저장소 |
| EntityState |Text |개체의 현재 상태(예: 활성, 삭제됨) |
| LastUpdatedDate |Date |선택된 행이 마지막으로 업데이트된 날짜 |

### <a name="time"></a>Time
이 표에서는 시간 관련 필드에 대한 세부 정보를 제공합니다.

| 필드 | 데이터 형식 | 설명 |
| --- | --- | --- |
| Hour |Time |하루의 시간(예: 1:00:00 PM) |
| HourNumber |10진수 |하루의 시간 수(예: 13.00) |
| 분 |10진수 |시간의 분 |
| PeriodOfTheDay |Text |하루의 기간 슬롯(예: 12-3 AM) |
| Time |Time |하루의 시간(예: 12:00:01 AM) |
| TimeKey |Text |시간을 나타내는 키 값 |

### <a name="vault"></a>Vault
이 표에서는 다양한 자격 증명 모음 관련 필드를 통해 기본 필드 및 집계를 제공합니다.

| 필드 | 데이터 형식 | 설명 |
| --- | --- | --- |
| #Vaults |정수 |자격 증명 모음의 수 |
| AsOnDateTime |날짜/시간 |선택한 행에 대한 최신 새로 고침 시간 |
| AzureDataCenter |Text |자격 증명 모음이 위치한 데이터 센터 |
| EntityState |Text |자격 증명 모음 개체의 현재 상태(예: 활성, 삭제됨) |
| StorageReplicationType |Text |자격 증명 모음의 저장소 복제 유형(예: GeoRedundant) |
| SubscriptionId |Text |보고서 생성에 대해 선택한 고객의 구독 ID |
| VaultName |Text |자격 증명 모음의 이름 |
| VaultTags |Text |자격 증명 모음에 연결된 태그 |

## <a name="next-steps"></a>다음 단계
Azure Backup 보고서를 만들기 위해 데이터 모델을 검토한 후 Power BI에서 보고서 만들기 및 보기에 대한 자세한 내용은 다음 문서를 참조하세요.

* [Power BI에서 보고서 만들기](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)
* [Power BI에서 보고서 필터링](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
