---
title: Azure Data Factory를 사용하여 SFTP 서버에서 데이터 복사 | Microsoft Docs
description: SFTP 서버에서 싱크로 지원되는 데이터 저장소로 데이터를 복사할 수 있게 해주는 Azure Data Factory의 MySQL 커넥터에 대해 알아봅니다.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: 1a62e9e8377705af1a70e356f554cfa549c58f20
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233458"
---
# <a name="copy-data-from-sftp-server-using-azure-data-factory"></a>Azure Data Factory를 사용하여 SFTP 서버에서 데이터 복사
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [버전 1](v1/data-factory-sftp-connector.md)
> * [현재 버전](connector-sftp.md)

이 문서에는 SFTP 서버에서 데이터를 복사 하는 방법을 설명 합니다. Azure Data Factory에 대해 자세히 알아보려면 [소개 문서](introduction.md)를 참조하세요.

## <a name="supported-capabilities"></a>지원되는 기능

이 SFTP 커넥터는 다음 작업에 대 한 지원 됩니다.

- [복사 활동](copy-activity-overview.md) 사용 하 여 [원본/싱크 매트릭스를 지원 합니다.](copy-activity-overview.md)
- [조회 작업](control-flow-lookup-activity.md)
- [GetMetadata 작업](control-flow-get-metadata-activity.md)

특히 이 SFTP 커넥터는 다음을 지원합니다.

- **Basic** 또는 **SshPublicKey** 인증을 사용한 파일 복사
- 파일을 있는 그대로 복사 또는 [지원되는 파일 형식 및 압축 코덱](supported-file-formats-and-compression-codecs.md)을 사용한 파일 구문 분석

## <a name="get-started"></a>시작하기

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

다음 섹션에서는 SFTP에 한정된 Data Factory 엔터티를 정의하는 데 사용되는 속성에 대해 자세히 설명합니다.

## <a name="linked-service-properties"></a>연결된 서비스 속성

SFTP 연결된 서비스에 다음 속성이 지원됩니다.

| 자산 | 설명 | 필수 |
|:--- |:--- |:--- |
| type | type 속성을 다음으로 설정해야 합니다. **Sftp**. |예. |
| host | SFTP 서버의 이름 또는 IP 주소입니다. |예 |
| port | SFTP 서버가 수신하는 포트입니다.<br/>허용되는 값은 정수이며 기본값은 **22**입니다. |아닙니다. |
| skipHostKeyValidation | 호스트 키 유효성 검사를 건너뛸지 여부를 지정합니다.<br/>허용되는 값은 **true**, **false**(기본값)입니다.  | 아닙니다. |
| hostKeyFingerprint | 호스트 키의 지문을 지정합니다. | “skipHostKeyValidation”이 false로 설정된 경우 예  |
| authenticationType | 인증 유형을 지정합니다.<br/>허용되는 값은 다음과 같습니다. **Basic**, **SshPublicKey**. 더 많은 속성 및 각 속성의 JSON 샘플은 [기본 인증 사용](#using-basic-authentication) 및 [SSH 공개 키 인증 사용](#using-ssh-public-key-authentication) 섹션을 참조하세요. |예. |
| connectVia | 데이터 저장소에 연결하는 데 사용할 [Integration Runtime](concepts-integration-runtime.md)입니다. Azure Integration Runtime 또는 자체 호스팅 Integration Runtime을 사용할 수 있습니다(데이터 저장소가 프라이빗 네트워크에 있는 경우). 지정하지 않으면 기본 Azure Integration Runtime을 사용합니다. |아닙니다. |

### <a name="using-basic-authentication"></a>기본 인증 사용

기본 인증을 사용하려면 “authenticationType” 속성을 **Basic**으로 설정하고, 마지막 섹션에서 소개한 SFTP 커넥터 일반 속성 외에 다음 속성을 지정합니다.

| 자산 | 설명 | 필수 |
|:--- |:--- |:--- |
| userName | SFTP 서버에 액세스하는 사용자. |예 |
| password | 사용자(userName) 암호입니다. 이 필드를 SecureString으로 표시하여 Data Factory에 안전하게 저장하거나 [Azure Key Vault에 저장되는 비밀을 참조](store-credentials-in-key-vault.md)합니다. | 예. |

**예제:**

```json
{
    "name": "SftpLinkedService",
    "type": "linkedservices",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<sftp server>",
            "port": 22,
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "authenticationType": "Basic",
            "userName": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="using-ssh-public-key-authentication"></a>SSH 공개 키 인증 사용

SSH 공개 키 인증을 사용하려면 “authenticationType” 속성을 **SshPublicKey**로 설정하고, 마지막 섹션에서 소개한 SFTP 커넥터 일반 속성 외에 다음 속성을 지정합니다.

| 자산 | 설명 | 필수 |
|:--- |:--- |:--- |
| userName | SFTP 서버에 액세스하는 사용자 |예 |
| privateKeyPath | Integration Runtime에서 액세스할 수 있는 프라이빗 키 파일의 절대 경로를 지정합니다. 자체 호스팅된 유형의 Integration Runtime이 “connectVia”에 지정된 경우에만 적용됩니다. | `privateKeyPath` 또는 `privateKeyContent`를 지정합니다.  |
| privateKeyContent | Base64 인코딩된 SSH 프라이빗 키 콘텐츠입니다. SSH 프라이빗 키가 OpenSSH 형식이어야 합니다. 이 필드를 SecureString으로 표시하여 Data Factory에 안전하게 저장하거나 [Azure Key Vault에 저장되는 비밀을 참조](store-credentials-in-key-vault.md)합니다. | `privateKeyPath` 또는 `privateKeyContent`를 지정합니다. |
| passPhrase | 키 파일이 암호문으로 보호되는 경우 프라이빗 키를 해독하는 암호문/암호를 지정합니다. 이 필드를 SecureString으로 표시하여 Data Factory에 안전하게 저장하거나 [Azure Key Vault에 저장되는 비밀을 참조](store-credentials-in-key-vault.md)합니다. | 프라이빗 키 파일이 암호문으로 보호되는 경우에는 필수입니다. |

> [!NOTE]
> SFTP 커넥터는 RSA/DSA OpenSSH 키를 지원합니다. 키 파일 콘텐츠는 "-----BEGIN [RSA/DSA] PRIVATE KEY-----"로 시작되어야 합니다. 프라이빗 키 파일이 ppk 형식 파일인 경우 Putty 도구를 사용하여 .ppk를 OpenSSH 형식으로 변환합니다. 

**예제 1: 프라이빗 키 filePath를 사용한 SshPublicKey 인증**

```json
{
    "name": "SftpLinkedService",
    "type": "Linkedservices",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<sftp server>",
            "port": 22,
            "skipHostKeyValidation": true,
            "authenticationType": "SshPublicKey",
            "userName": "xxx",
            "privateKeyPath": "D:\\privatekey_openssh",
            "passPhrase": {
                "type": "SecureString",
                "value": "<pass phrase>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**예제 2: 프라이빗 키 콘텐츠를 사용한 SshPublicKey 인증**

```json
{
    "name": "SftpLinkedService",
    "type": "Linkedservices",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<sftp server>",
            "port": 22,
            "skipHostKeyValidation": true,
            "authenticationType": "SshPublicKey",
            "userName": "<username>",
            "privateKeyContent": {
                "type": "SecureString",
                "value": "<base64 string of the private key content>"
            },
            "passPhrase": {
                "type": "SecureString",
                "value": "<pass phrase>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>데이터 세트 속성

데이터 세트 정의에 사용할 수 있는 섹션 및 속성의 전체 목록은 [데이터 세트](concepts-datasets-linked-services.md) 문서를 참조하세요. 

- 에 대 한 **Parquet 및 구분 기호로 분리 된 텍스트 형식**를 참조 하세요 [Parquet 및 구분 기호로 분리 된 텍스트 데이터 집합 형식](#parquet-and-delimited-text-format-dataset) 섹션입니다.
- 와 같은 다른 형식에 대 한 **ORC/Avro/JSON/이진 파일 형식**를 참조 [다른 형식으로 데이터 집합을](#other-format-dataset) 섹션입니다.

### <a name="parquet-and-delimited-text-format-dataset"></a>Parquet 및 구분 기호로 분리 된 텍스트 데이터 집합 형식

SFTP에서 데이터를 복사할 **Parquet 또는 구분 기호로 분리 된 텍스트 형식**, 참조 [Parquet 형식](format-parquet.md) 및 [구분 기호로 분리 된 텍스트 형식으로](format-delimited-text.md) 형식 기반의 데이터 집합에 대 한 문서 및 지원 설정. SFTP에서 다음 속성이 지원 됩니다 `location` 형식 기반의 데이터 집합의 설정:

| 자산   | 설명                                                  | 필수 |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | Type 속성은 아래 `location` 데이터 집합에서으로 설정 되어 있어야 **SftpLocation**합니다. | 예.      |
| folderPath | 폴더 경로입니다. 와일드 카드 필터 폴더로 사용 하려는 경우이 설정은 건너뛰고 활동 원본 설정에서 지정 합니다. | 아닙니다.       |
| fileName   | 지정 된 folderPath에서 파일 이름입니다. 와일드 카드를 사용 하 여 파일을 필터링 하려는 경우이 설정은 건너뛰고 활동 원본 설정에서 지정 합니다. | 아닙니다.       |

> [!NOTE]
> **FileShare** 다음 섹션에 언급 된 Parquet/텍스트 형식 사용 하 여 데이터 집합 형식으로 계속 지원-이전 버전과 호환성에 대 한 복사/조회/GetMetadata 작업입니다. 앞으로이 새 모델을 사용 하도록 제안 된 및 UI를 작성 하는 ADF 이러한 새 형식 생성로 전환 되었습니다.

**예제:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<SFTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "SftpLocation",
                "folderPath": "root/folder/subfolder"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

### <a name="other-format-dataset"></a>다른 형식으로 데이터 집합

SFTP에서 데이터를 복사할 **ORC/Avro/JSON/이진 형식**에 다음 속성이 지원 됩니다.

| 자산 | 설명 | 필수 |
|:--- |:--- |:--- |
| type | 데이터 세트의 type 속성을 다음으로 설정해야 합니다. **FileShare** |예 |
| folderPath | 파일의 경로입니다. 와일드카드 필터가 지원되며, 허용되는 와일드카드는 `*`(0개 이상의 문자 일치) 및 `?`(0-1개의 문자 일치)입니다. 실제 파일 이름에 와일드카드 또는 이 이스케이프 문자가 있는 경우 `^`을 사용하여 이스케이프합니다. <br/><br/>예: rootfolder/subfolder/(더 많은 예제는 [폴더 및 파일 필터 예제](#folder-and-file-filter-examples) 참조) |예 |
| fileName |  지정된 "folderPath" 아래의 파일에 대한 **이름 또는 와일드 카드 필터**입니다. 이 속성의 값을 지정하지 않으면 데이터 세트는 폴더에 있는 모든 파일을 가리킵니다. <br/><br/>필터에 허용되는 와일드카드는 `*`(문자 0자 이상 일치) 및 `?`(문자 0자 또는 1자 일치)입니다.<br/>- 예 1: `"fileName": "*.csv"`<br/>- 예 2: `"fileName": "???20180427.txt"`<br/>실제 폴더 이름에 와일드카드 또는 이 이스케이프 문자가 있는 경우 `^`을 사용하여 이스케이프합니다. |아닙니다. |
| modifiedDatetimeStart | 특성에 기반한 파일 필터링: 마지막으로 수정한 날짜 마지막 수정 시간이 `modifiedDatetimeStart`와 `modifiedDatetimeEnd` 사이의 시간 범위 내에 있으면 파일이 선택됩니다. 시간은 UTC 표준 시간대에 "2018-12-01T05:00:00Z" 형식으로 적용됩니다. <br/><br/> 대용량 파일에서에서 필터 파일 수행 하려는 경우이 설정을 사용 하 여 데이터 이동의 전반적인 성능을 영향을 알아야 합니다. <br/><br/> 속성에는 데이터 집합에 없는 파일 특성 필터를 적용할 것을 의미 하는 NULL 일 수 있습니다.  `modifiedDatetimeStart`에 datetime 값이 있지만 `modifiedDatetimeEnd`가 NULL이면, 마지막으로 수정된 특성이 datetime 값보다 크거나 같은 파일이 선택됩니다.  `modifiedDatetimeEnd`에 datetime 값이 있지만 `modifiedDatetimeStart`가 NULL이면, 마지막으로 수정된 특성이 datetime 값보다 작은 파일이 선택됩니다.| 아닙니다. |
| modifiedDatetimeEnd | 특성에 기반한 파일 필터링: 마지막으로 수정한 날짜 마지막 수정 시간이 `modifiedDatetimeStart`와 `modifiedDatetimeEnd` 사이의 시간 범위 내에 있으면 파일이 선택됩니다. 시간은 UTC 표준 시간대에 "2018-12-01T05:00:00Z" 형식으로 적용됩니다. <br/><br/> 대용량 파일에서에서 필터 파일 수행 하려는 경우이 설정을 사용 하 여 데이터 이동의 전반적인 성능을 영향을 알아야 합니다. <br/><br/> 속성에는 데이터 집합에 없는 파일 특성 필터를 적용할 것을 의미 하는 NULL 일 수 있습니다.  `modifiedDatetimeStart`에 datetime 값이 있지만 `modifiedDatetimeEnd`가 NULL이면, 마지막으로 수정된 특성이 datetime 값보다 크거나 같은 파일이 선택됩니다.  `modifiedDatetimeEnd`에 datetime 값이 있지만 `modifiedDatetimeStart`가 NULL이면, 마지막으로 수정된 특성이 datetime 값보다 작은 파일이 선택됩니다.| 아닙니다. |
| format | 파일 기반 저장소(이진 복사) 간에 **파일을 있는 그대로 복사**하려는 경우 입력 및 출력 데이터 세트 정의 둘 다에서 형식 섹션을 건너뜁니다.<br/><br/>특정 형식의 파일을 구문 분석하려는 경우, 지원되는 파일 형식 유형은 **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**입니다. 이 값 중 하나로 서식에서 **type** 속성을 설정합니다. 자세한 내용은 [텍스트 형식](supported-file-formats-and-compression-codecs.md#text-format), [Json 형식](supported-file-formats-and-compression-codecs.md#json-format), [Avro 형식](supported-file-formats-and-compression-codecs.md#avro-format), [Orc 형식](supported-file-formats-and-compression-codecs.md#orc-format) 및 [Parquet 형식](supported-file-formats-and-compression-codecs.md#parquet-format) 섹션을 참조하세요. |아니요(이진 복사 시나리오에만 해당) |
| compression | 데이터에 대한 압축 유형 및 수준을 지정합니다. 자세한 내용은 [지원되는 파일 형식 및 압축 코덱](supported-file-formats-and-compression-codecs.md#compression-support)을 참조하세요.<br/>지원되는 형식은 **GZip**, **Deflate**, **BZip2** 및 **ZipDeflate**입니다.<br/>지원되는 수준은 **최적** 및 **가장 빠름**입니다. |아닙니다. |

>[!TIP]
>폴더 아래에서 모든 파일을 복사하려면 **folderPath**만을 지정합니다.<br>지정된 이름의 단일 파일을 복사하려면 폴더 부분으로 **folderPath** 및 파일 이름으로 **fileName**을 지정합니다.<br>폴더 아래에서 파일의 하위 집합을 복사하려면 폴더 부분으로 **folderPath** 및 와일드카드 필터로 **fileName**을 지정합니다.

>[!NOTE]
>파일 필터에 "fileFilter" 속성을 사용한 경우 그대로 계속 지원되지만, 이후 "fileName"에 추가된 새 필터 기능을 사용하도록 제안합니다.

**예제:**

```json
{
    "name": "SFTPDataset",
    "type": "Datasets",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<SFTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
            "fileName": "*",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>복사 작업 속성

작업 정의에 사용할 수 있는 섹션 및 속성의 전체 목록은 [파이프라인](concepts-pipelines-activities.md) 문서를 참조하세요. 이 섹션에서는 SFTP 원본에서 지원하는 속성의 목록을 제공합니다.

### <a name="sftp-as-source"></a>SFTP를 원본으로

- 복사본에 대 한 **Parquet 및 구분 기호로 분리 된 텍스트 형식**를 참조 하세요 [Parquet 및 구분 기호로 분리 된 텍스트 형식으로 원본](#parquet-and-delimited-text-format-source) 섹션.
- 와 같은 다른 형식에서 복사본에 대 한 **ORC/Avro/JSON/이진 파일 형식**를 참조 [다른 형식으로 원본](#other-format-source) 섹션입니다.

#### <a name="parquet-and-delimited-text-format-source"></a>Parquet 및 구분 기호로 분리 된 텍스트 형식으로 원본

SFTP에서 데이터를 복사할 **Parquet 또는 구분 기호로 분리 된 텍스트 형식**, 참조 [Parquet 형식](format-parquet.md) 및 [구분 기호로 분리 된 텍스트 형식으로](format-delimited-text.md) 형식 기반의 복사 활동 원본에 대 한 문서 및 지원 되는 설정입니다. SFTP에서 다음 속성이 지원 됩니다 `storeSettings` 형식 기반의 복사 원본에 설정 합니다.

| 자산                 | 설명                                                  | 필수                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | 아래에 있는 type 속성은 `storeSettings` 으로 설정 되어 있어야 **SftpReadSetting**합니다. | 예.                                           |
| recursive                | 하위 폴더 또는 지정된 폴더에서만 데이터를 재귀적으로 읽을지 여부를 나타냅니다. recursive를 true로 설정하고 싱크가 파일 기반 저장소인 경우 빈 폴더 또는 하위 폴더가 싱크에 복사되거나 만들어지지 않습니다. 허용되는 값은 **true**(기본값) 및 **false**입니다. | 아닙니다.                                            |
| wildcardFolderPath       | 소스 폴더를 필터링 하려면 와일드 카드 문자를 사용 하 여 폴더 경로입니다. <br>허용되는 와일드카드는 `*`(0개 이상의 문자 일치) 및 `?`(0-1개의 문자 일치)입니다. 실제 폴더 이름에 와일드카드 또는 이 이스케이프 문자가 있는 경우 `^`을 사용하여 이스케이프합니다. <br>더 많은 예는 [폴더 및 파일 필터 예제](#folder-and-file-filter-examples)를 참조하세요. | 아닙니다.                                            |
| wildcardFileName         | 필터 소스 파일에 지정 된 folderPath/wildcardFolderPath에서 와일드 카드 문자를 사용 하 여 파일 이름입니다. <br>허용되는 와일드카드는 `*`(0개 이상의 문자 일치) 및 `?`(0-1개의 문자 일치)입니다. 실제 폴더 이름에 와일드카드 또는 이 이스케이프 문자가 있는 경우 `^`을 사용하여 이스케이프합니다.  더 많은 예는 [폴더 및 파일 필터 예제](#folder-and-file-filter-examples)를 참조하세요. | 경우에 예 `fileName` 데이터 집합에 지정 되지 않은 |
| modifiedDatetimeStart    | 특성에 기반한 파일 필터링: 마지막으로 수정한 날짜 마지막 수정 시간이 `modifiedDatetimeStart`와 `modifiedDatetimeEnd` 사이의 시간 범위 내에 있으면 파일이 선택됩니다. 시간은 UTC 표준 시간대에 "2018-12-01T05:00:00Z" 형식으로 적용됩니다. <br> 속성은 NULL일 수 있습니다. 이 경우 파일 특성 필터가 데이터 세트에 적용되지 않습니다.  `modifiedDatetimeStart`에 datetime 값이 있지만 `modifiedDatetimeEnd`가 NULL이면, 마지막으로 수정된 특성이 datetime 값보다 크거나 같은 파일이 선택됩니다.  `modifiedDatetimeEnd`에 datetime 값이 있지만 `modifiedDatetimeStart`가 NULL이면, 마지막으로 수정된 특성이 datetime 값보다 작은 파일이 선택됩니다. | 아닙니다.                                            |
| modifiedDatetimeEnd      | 위와 동일합니다.                                               | 아닙니다.                                            |
| maxConcurrentConnections | Storage 저장소에 동시에 연결할 연결 횟수입니다. 데이터 저장소에 대 한 동시 연결을 제한 하려는 경우에 지정 합니다. | 아닙니다.                                            |

> [!NOTE]
> Parquet/구분 기호로 분리 된 텍스트 형식에 대 한 **FileSystemSource** 다음 섹션에 언급 된 형식 복사 활동 원본으로 계속 지원 됩니다-이전 버전과 호환성입니다. 앞으로이 새 모델을 사용 하도록 제안 된 및 UI를 작성 하는 ADF 이러한 새 형식 생성로 전환 되었습니다.

**예제:**

```json
"activities":[
    {
        "name": "CopyFromSFTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Delimited text input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DelimitedTextSource",
                "formatSettings":{
                    "type": "DelimitedTextReadSetting",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "SftpReadSetting",
                    "recursive": true,
                    "wildcardFolderPath": "myfolder*A",
                    "wildcardFileName": "*.csv"
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="other-format-source"></a>다른 형식으로 원본

SFTP에서 데이터를 복사할 **ORC/Avro/JSON/이진 파일 형식**를 복사 작업에서 다음 속성이 지원 됩니다 **원본** 섹션:

| 자산 | 설명 | 필수 |
|:--- |:--- |:--- |
| type | 복사 작업 원본의 type 속성을 다음으로 설정해야 합니다. **FileSystemSource** |예 |
| recursive | 하위 폴더에서 또는 지정된 폴더에서만 데이터를 재귀적으로 읽을지 여부를 나타냅니다. recursive가 true로 설정되고 싱크가 파일 기반 저장소인 경우 싱크에서 빈 폴더/하위 폴더가 복사/생성되지 않습니다.<br/>허용되는 값은 **true**(기본값), **false**입니다. | 아닙니다. |
| maxConcurrentConnections | Storage 저장소에 동시에 연결할 연결 횟수입니다. 데이터 저장소에 대 한 동시 연결을 제한 하려는 경우에 지정 합니다. | 아닙니다. |

**예제:**

```json
"activities":[
    {
        "name": "CopyFromSFTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SFTP input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "FileSystemSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>폴더 및 파일 필터 예제

이 섹션에서는 와일드카드 필터가 있는 폴더 경로 및 파일 이름의 결과 동작에 대해 설명합니다.

| folderPath | fileName | recursive | 원본 폴더 구조 및 필터 결과(**굵게** 표시된 파일이 검색됨)|
|:--- |:--- |:--- |:--- |
| `Folder*` | (비어 있음, 기본값 사용) | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | (비어 있음, 기본값 사용) | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

## <a name="next-steps"></a>다음 단계
Azure Data Factory에서 복사 작업의 원본 및 싱크로 지원되는 데이터 저장소 목록은 [지원되는 데이터 저장소](copy-activity-overview.md##supported-data-stores-and-formats)를 참조하세요.
