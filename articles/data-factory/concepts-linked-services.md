---
title: Azure Data Factory 서비스에에서 연결 | Microsoft Docs
description: Data Factory에서 연결 된 서비스에 알아봅니다. 연결된 서비스는 계산/데이터 저장소를 데이터 팩터리에 연결합니다.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: shlo
ms.openlocfilehash: ba2041495e1e3c63ee322a0b748753ad6cb68914
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64870134"
---
# <a name="linked-services-in-azure-data-factory"></a>Azure Data Factory에서 연결 된 서비스
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [버전 1](v1/data-factory-create-datasets.md)
> * [현재 버전](concepts-datasets-linked-services.md)

이 문서에서는 JSON 형식으로 정의 하는 방법, 연결 된 서비스를 설명 하 고 Azure Data Factory 파이프라인에서 사용 되는 방법입니다.

Data Factory를 처음 사용하는 경우 [Azure Data Factory 소개](introduction.md)를 참조하세요.

## <a name="overview"></a>개요
데이터 팩터리에는 하나 이상의 파이프라인이 포함될 수 있습니다. **파이프라인**은 함께 하나의 작업을 수행하는 **활동**의 논리적 그룹화입니다. 파이프라인의 활동은 데이터에 수행할 작업을 정의합니다. 예를 들어 복사 활동을 사용하여 온-프레미스 SQL Server에서 Azure Blob Storage로 데이터를 복사할 수 있습니다. 그런 다음 Azure HDInsight 클러스터에서 Hive 스크립트를 실행하는 Hive 활동을 사용하여 출력 데이터를 생성하도록 Blob Storage의 데이터를 처리합니다. 마지막으로 두 번째 복사 활동을 사용하여 BI(비즈니스 인텔리전스) 보고 솔루션의 기반이 되는 Azure SQL Data Warehouse로 출력 데이터를 복사합니다. 파이프라인 및 활동에 대한 자세한 내용은 Azure Data Factory의 [파이프라인 및 활동](concepts-pipelines-activities.md) 문서를 참조하세요.

이제 **데이터 세트**는 입력 및 출력으로 **활동**에서 사용하려는 데이터를 단순히 가리키거나 참조하는 데이터의 명명된 뷰입니다.

데이터 세트를 만들기 전에 **연결된 서비스**를 만들어 데이터 저장소를 데이터 팩터리에 연결해야 합니다. 연결된 서비스는 Data Factory에서 외부 리소스에 연결하는 데 필요한 연결 정보를 정의하는 연결 문자열과 같습니다. 데이터 세트는 연결된 데이터 저장소 내의 데이터 구조를 나타내고, 연결된 서비스는 데이터 원본에 연결을 정의한다고 생각하시면 됩니다. 예를 들어 Azure Storage 연결된 서비스는 스토리지 계정을 데이터 팩터리에 연결합니다. Azure Blob 데이터 세트는 Blob 컨테이너 및 처리할 입력 Blob이 포함된 Azure 저장소 계정 내의 폴더를 나타냅니다.

샘플 시나리오는 다음과 같습니다. Blob Storage에서 SQL Database로 데이터를 복사하려면 두 개의 연결된 서비스, 즉, Azure Storage 및 Azure SQL Database를 만듭니다. 그런 다음, 두 개의 데이터 세트, 즉 Azure Blob 데이터 세트(Azure Storage 연결된 서비스 참조), Azure SQL 테이블 데이터 세트(Azure SQL Database 연결된 서비스 참조)를 만듭니다. Azure Storage 및 Azure SQL Database 연결된 서비스에는 Data Factory가 런타임에 Azure Storage 및 Azure SQL Database 각각에 연결하는 데 사용하는 연결 문자열이 포함되어 있습니다. Azure Blob 데이터 집합은 Blob Storage에 입력 Blob을 포함하는 Blob 컨테이너와 Blob 폴더를 지정합니다. Azure SQL 테이블 데이터 세트는 데이터를 복사할 Azure SQL Database의 SQL 테이블을 지정합니다.

다음 다이어그램에서는 Data Factory의 파이프라인, 활동, 데이터 세트 및 연결된 서비스 간의 관계를 보여 줍니다.

![파이프라인, 활동, 데이터 세트, 연결된 서비스 간 관계](media/concepts-datasets-linked-services/relationship-between-data-factory-entities.png)

## <a name="linked-service-json"></a>연결된 서비스 JSON
Data Factory의 연결된 서비스는 다음과 같이 JSON 형식으로 정의됩니다.

```json
{
    "name": "<Name of the linked service>",
    "properties": {
        "type": "<Type of the linked service>",
        "typeProperties": {
              "<data store or compute-specific type properties>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

다음 표에서는 위의 JSON에서 속성을 설명합니다.

자산 | 설명 | 필수 |
-------- | ----------- | -------- |
이름 | 연결된 서비스의 이름입니다. [Azure Data Factory - 이름 지정 규칙](naming-rules.md)을 참조하세요. |  예 |
형식 | 연결된 서비스의 형식입니다. 예를 들면 다음과 같습니다. AzureStorage(데이터 저장소) 또는 AzureBatch(컴퓨팅). typeProperties에 대한 설명을 참조하세요. | 예 |
typeProperties | 형식 속성은 각 데이터 저장소 또는 계산에 대해 다릅니다. <br/><br/> 지원되는 데이터 저장소 형식 및 해당 형식 속성은 [데이터 세트 형식](concepts-datasets-linked-services.md#dataset-type) 표를 참조하세요. 데이터 저장소 관련 형식 속성에 대해 알아보려면 데이터 저장소 커넥터 문서로 이동하세요. <br/><br/> 지원되는 계산 형식 및 해당 형식 속성은 [컴퓨팅 연결 서비스](compute-linked-services.md)를 참조하세요. | 예 |
connectVia | 데이터 저장소에 연결하는 데 사용할 [Integration Runtime](concepts-integration-runtime.md)입니다. Azure Integration Runtime 또는 자체 호스팅 Integration Runtime을 사용할 수 있습니다(데이터 저장소가 프라이빗 네트워크에 있는 경우). 지정하지 않으면 기본 Azure Integration Runtime을 사용합니다. | 아닙니다.

## <a name="linked-service-example"></a>연결된 서비스 예제
다음의 연결된 서비스는 Azure Storage 연결된 서비스입니다. 형식은 AzureStorage로 설정됩니다. Azure Storage 연결된 서비스의 형식 속성에는 연결 문자열이 포함됩니다. Data Factory 서비스는 이 연결 문자열을 사용하여 런타임에 데이터 저장소에 연결합니다.

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="create-linked-services"></a>연결된 서비스 만들기
이러한 도구 또는 Sdk 중 하나를 사용 하 여 연결 된 서비스를 만들 수 있습니다: [.NET API](quickstart-create-data-factory-dot-net.md)를 [PowerShell](quickstart-create-data-factory-powershell.md)를 [REST API](quickstart-create-data-factory-rest-api.md), Azure Resource Manager 템플릿과 Azure 포털

## <a name="data-store-linked-services"></a>데이터 저장소 연결 된 서비스
데이터 저장소에 연결할 수 있습니다에 우리의 [지원 되는 데이터 저장소 및 형식](copy-activity-overview.md#supported-data-stores-and-formats)합니다. 다른 저장소에 필요한 특정 연결 속성의 목록을 참조 합니다.

## <a name="compute-linked-services"></a>컴퓨팅 연결 서비스
참조 [지 원하는 compute 환경](compute-linked-services.md) 에서 다른 구성 뿐만 아니라 데이터 팩터리에 연결할 수 자세히 다른 계산 환경에 대 한 합니다.

## <a name="next-steps"></a>다음 단계
다음 도구 또는 SDK 중 하나를 사용하여 파이프라인 및 데이터 세트를 만들기 위한 단계별 지침은 다음 자습서를 참조하세요.

- [빠른 시작: .NET을 사용하여 데이터 팩터리 만들기](quickstart-create-data-factory-dot-net.md)
- [빠른 시작: PowerShell을 사용하여 데이터 팩터리 만들기](quickstart-create-data-factory-powershell.md)
- [빠른 시작: REST API를 사용하여 데이터 팩터리 만들기](quickstart-create-data-factory-rest-api.md)
- [빠른 시작: Azure portal을 사용 하 여 데이터 팩터리 만들기](quickstart-create-data-factory-portal.md)
