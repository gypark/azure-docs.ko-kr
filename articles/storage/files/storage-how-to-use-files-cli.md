---
title: Azure CLI를 사용하여 Azure 파일 공유를 관리하기 위한 빠른 시작
description: 이 빠른 시작을 통해 Azure CLI를 사용하여 Azure Files를 관리하는 방법을 알아봅니다.
services: storage
author: roygara
ms.service: storage
ms.topic: quickstart
ms.date: 10/26/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 43a5a72ac32d8ed3510cecb505f5e62cf91d7106
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64710804"
---
# <a name="quickstart-create-and-manage-azure-file-shares-using-azure-cli"></a>빠른 시작: Azure CLI를 사용하여 Azure 파일 공유 만들기 및 관리
이 가이드에서는 Azure CLI를 사용하여 [Azure 파일 공유](storage-files-introduction.md)로 작업하는 기본 사항을 설명합니다. Azure 파일 공유는 다른 파일 공유와 유사하지만, 클라우드에 저장되고 Azure Platform에서 지원합니다. Azure 파일 공유는 산업 표준 SMB 프로토콜을 지원하며 여러 머신, 애플리케이션 및 인스턴스 전반에서 파일 공유를 활성화합니다. 

Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)을 만듭니다.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Azure CLI를 로컬로 설치하고 사용하려는 경우 이 문서의 단계에 대해 Azure CLI 버전 2.0.4 이상을 실행해야 합니다. **az --version**을 실행하여 Azure CLI 버전을 찾습니다. 설치 또는 업그레이드해야 하는 경우 [Azure CLI 2.0 설치](/cli/azure/install-azure-cli)를 참조하세요. 

기본적으로 Azure CLI 명령은 JSON(JavaScript Object Notation)을 반환합니다. JSON은 REST API에서 메시지를 보내고 받는 표준 방법입니다. JSON 응답으로 작업을 용이하게 하기 위해 이 문서의 일부 예제에서는 Azure CLI 명령에 *쿼리* 매개 변수를 사용합니다. 이 매개 변수는 JSON을 구문 분석하기 위해 [JMESPath 쿼리 언어](http://jmespath.org/)를 사용합니다. JMESPath 쿼리 언어를 따라 Azure CLI 명령의 결과를 사용하는 방법에 대해 자세히 알아보려면 [JMESPath 자습서](http://jmespath.org/tutorial.html)를 참조하세요.

## <a name="sign-in-to-azure"></a>Azure에 로그인
Azure CLI를 로컬로 사용하는 경우 프롬프트를 열고 아직 수행하지 않은 경우 Azure에 로그인합니다.

```bash 
az login
```

## <a name="create-a-resource-group"></a>리소스 그룹 만들기
리소스 그룹은 Azure 리소스가 배포 및 관리되는 논리적 컨테이너입니다. Azure 리소스 그룹이 아직 없는 경우 [az group create](/cli/azure/group) 명령을 사용하여 새 리소스 그룹을 만들 수 있습니다. 

다음 예제에서는 *미국 동부* 위치에 *myResourceGroup*이라는 리소스 그룹을 만듭니다.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-storage-account"></a>저장소 계정 만들기
저장소 계정은 Azure 파일 공유 또는 Blob이나 큐와 같은 다른 저장소 리소스를 배포할 수 있는 저장소의 공유 풀입니다. 저장소 계정에 포함될 수 있는 파일 공유 수에는 제한이 없습니다. 공유에 저장할 수 있는 파일 수에는 제한이 없으며, 저장소 계정의 최대 용량까지 저장할 수 있습니다.

다음 예제에서는 [az storage account create](/cli/azure/storage/account) 명령을 사용하여 *mystorageaccount\<난수\>* 라는 스토리지 계정을 만든 다음, 해당 스토리지 계정의 이름을 `$STORAGEACCT` 변수에 배치합니다. 스토리지 계정 이름은 고유해야 하므로 "mystorageacct"를 고유 이름으로 바꾸어야 합니다.

```azurecli-interactive 
STORAGEACCT=$(az storage account create \
    --resource-group "myResourceGroup" \
    --name "mystorageacct" \
    --location eastus \
    --sku Standard_LRS \
    --query "name" | tr -d '"')
```

### <a name="get-the-storage-account-key"></a>저장소 계정 키 가져오기
저장소 계정 키는 저장소 계정에서 리소스에 대한 액세스를 제어합니다. 키는 저장소 계정을 만들 때 자동으로 만들어집니다. [az storage account keys list](/cli/azure/storage/account/keys) 명령을 사용하여 스토리지 계정에 대한 스토리지 계정 키를 가져올 수 있습니다. 

```azurecli-interactive 
STORAGEKEY=$(az storage account keys list \
    --resource-group "myResourceGroup" \
    --account-name $STORAGEACCT \
    --query "[0].value" | tr -d '"')
```

## <a name="create-an-azure-file-share"></a>Azure 파일 공유 만들기
이제 첫 번째 Azure 파일 공유를 만들 수 있습니다. [az storage share create](/cli/azure/storage/share) 명령을 사용하여 파일 공유를 만듭니다. 이 예제에서는 *myshare*라는 Azure 파일 공유를 만듭니다. 

```azurecli-interactive
az storage share create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" 
```

공유 이름은 소문자, 숫자 및 단일 하이픈만을 포함할 수 있습니다(하지만 하이픈으로 시작할 수 없음). 파일 공유 및 파일 이름 지정에 대한 자세한 내용은 [공유, 디렉터리, 파일 및 메타데이터 이름 지정 및 참조](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata)를 참조하세요.

## <a name="use-your-azure-file-share"></a>Azure 파일 공유 사용
Azure Files는 Azure 파일 공유 내에서 파일 및 폴더를 작업하는 두 가지 방법(업계 표준 [SMB(서버 메시지 블록) 프로토콜](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) 및 [File REST 프로토콜](https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api))을 제공합니다. 

SMB를 사용하여 파일 공유를 탑재하려면 OS에 따라 다음 문서를 참조하세요.
- [Linux](storage-how-to-use-files-linux.md)
- [macOS](storage-how-to-use-files-mac.md)
- [Windows](storage-how-to-use-files-windows.md)

### <a name="using-an-azure-file-share-with-the-file-rest-protocol"></a>File REST 프로토콜로 Azure 파일 공유 사용 
File REST 프로토콜을 직접 작업하는 것이 가능하지만(REST HTTP 호출 직접 생성), File REST 프로토콜을 사용하는 가장 일반적인 방법은 Azure CLI 또는 [Azure PowerShell 모듈](storage-how-to-use-files-powershell.md), 또는 Azure Storage SDK를 사용하는 것입니다. 이들 모두 선택한 스크립팅/프로그래밍 언어에서 File REST 프로토콜에 좋은 래퍼를 제공합니다.  

대부분의 Azure Files 사용에서 사용할 수 있을 것으로 예상하는 기존 애플리케이션 및 도구를 사용할 수 있기 때문에 SMB 프로토콜을 통해 Azure 파일 공유를 작업하길 원할 것으로 예상하지만, SMB보다는 File REST API를 사용하는 것이 이점인 이유가 다음과 같이 몇 가지 있습니다.

- Azure Bash Cloud Shell에서 파일 공유를 탐색하고 있는 경우(SMB를 통해 파일 공유를 탑재할 수 없음)
- 포트 445가 잠금 해제되어 있지 않은 온-프레미스 클라이언트 등, SMB 공유를 탑재할 수 없는 클라이언트에서 스크립트 또는 애플리케이션을 실행해야 하는 경우
- 서버리스 리소스([Azure Functions](../../azure-functions/functions-overview.md) 등)를 활용하고 있는 경우 

다음 예제에서는 Azure CLI를 사용하여 File REST 프로토콜로 Azure 파일 공유를 조작하는 방법을 보여줍니다. 

### <a name="create-a-directory"></a>디렉터리 만들기
Azure 파일 공유의 루트에 *myDirectory*라는 새 디렉터리를 만들려면 [`az storage directory create`](/cli/azure/storage/directory) 명령을 사용합니다.

```azurecli-interactive
az storage directory create \
   --account-name $STORAGEACCT \
   --account-key $STORAGEKEY \
   --share-name "myshare" \
   --name "myDirectory" 
```

### <a name="upload-a-file"></a>파일 업로드
[`az storage file upload`](/cli/azure/storage/file) 명령을 사용하여 파일을 업로드하는 방법을 보여주기 위해 먼저 Cloud Shell의 임시 드라이브에 업로드할 파일을 만듭니다. 다음 예제에서는 파일을 만든 다음, 업로드합니다.

```azurecli-interactive
date > ~/clouddrive/SampleUpload.txt

az storage file upload \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --source "~/clouddrive/SampleUpload.txt" \
    --path "myDirectory/SampleUpload.txt"
```

Azure CLI를 로컬로 실행 중인 경우 `~/clouddrive`를 컴퓨터에 있는 경로로 대체합니다.

파일을 업로드한 후에 [`az storage file list`](/cli/azure/storage/file) 명령을 사용하여 파일이 Azure 파일 공유에 업로드되었는지 확인할 수 있습니다.

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory" \
    --output table
```

### <a name="download-a-file"></a>파일 다운로드
[`az storage file download`](/cli/azure/storage/file) 명령을 사용하여 Cloud Shell의 임시 드라이브에 업로드한 파일의 복사본을 다운로드할 수 있습니다.

```azurecli-interactive
# Delete an existing file by the same name as SampleDownload.txt, if it exists, because you've run this example before
rm -rf ~/clouddrive/SampleDownload.txt

az storage file download \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory/SampleUpload.txt" \
    --dest "~/clouddrive/SampleDownload.txt"
```

### <a name="copy-files"></a>파일 복사
일반적인 작업은 하나의 파일 공유에서 다른 파일 공유로 파일을 복사하거나 Azure Blob Storage 컨테이너 간에 파일을 복사하는 것입니다. 이 기능을 보여주기 위해 새 공유를 만듭니다. [az storage file copy](/cli/azure/storage/file/copy) 명령을 사용하여 이 새 공유에 업로드한 파일을 복사합니다. 

```azurecli-interactive
az storage share create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare2"

az storage directory create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare2" \
    --name "myDirectory2"

az storage file copy start \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --source-share "myshare" \
    --source-path "myDirectory/SampleUpload.txt" \
    --destination-share "myshare2" \
    --destination-path "myDirectory2/SampleCopy.txt"
```

이제 새 공유에서 파일을 나열하는 경우 복사된 파일이 표시됩니다.

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare2" \
    --output table
```

`az storage file copy start` 명령은 Azure 파일 공유와 Azure Blob Storage 컨테이너 간에 파일 이동에 편리하지만 대규모 이동에 AzCopy를 사용하는 것이 좋습니다. (이동되는 파일의 수 또는 크기 측면에서) [Linux용 AzCopy](../common/storage-use-azcopy-linux.md) 및 [Windows용 AzCopy](../common/storage-use-azcopy.md)에 대해 자세히 알아봅니다. AzCopy는 로컬로 설치해야 합니다. AzCopy는 Cloud Shell에서 사용할 수 없습니다. 

## <a name="create-and-manage-share-snapshots"></a>공유 스냅샷 만들기 및 관리
Azure 파일 공유를 사용하여 수행할 수 있는 유용한 다른 작업은 공유 스냅샷을 만드는 것입니다. 스냅샷은 Azure 파일 공유의 지정 시간 복사본을 유지합니다. 공유 스냅샷은 이미 익숙한 다음과 같은 일부 운영 체제 기술과 비슷합니다.

- Linux 시스템용 [LVM(논리 볼륨 관리자)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) 스냅숏
- macOS용 [APFS(Apple 파일 시스템)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) 스냅숏
- NTFS 및 ReFS와 같은 Windows 파일 시스템용 [VSS(볼륨 섀도 복사본 서비스)](https://docs.microsoft.com/windows/desktop/VSS/volume-shadow-copy-service-portal). [`az storage share snapshot`](/cli/azure/storage/share) 명령을 사용하여 공유 스냅숏을 만들 수 있습니다.

```azurecli-interactive
SNAPSHOT=$(az storage share snapshot \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" \
    --query "snapshot" | tr -d '"')
```

### <a name="browse-share-snapshot-contents"></a>공유 스냅샷 콘텐츠 찾아보기
`az storage file list` 명령에 대한 `$SNAPSHOT` 변수에서 캡처한 공유 스냅숏의 타임스탬프를 전달하여 공유 스냅숏의 콘텐츠를 찾아볼 수 있습니다.

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --snapshot $SNAPSHOT \
    --output table
```

### <a name="list-share-snapshots"></a>공유 스냅샷 나열
공유에 만든 스냅샷의 목록을 보려면 다음 명령을 사용합니다.

```azurecli-interactive
az storage share list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --include-snapshot \
    --query "[? name=='myshare' && snapshot!=null]" | tr -d '"'
```

### <a name="restore-from-a-share-snapshot"></a>공유 스냅샷에서 복원
이전에 사용한 `az storage file copy start` 명령을 사용하여 파일을 복원할 수 있습니다. 먼저 스냅샷에서 복원할 수 있도록 업로드한 SampleUpload.txt 파일을 삭제합니다.

```azurecli-interactive
# Delete SampleUpload.txt
az storage file delete \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory/SampleUpload.txt"
 # Build the source URI for a snapshot restore
URI=$(az storage account show \
    --resource-group "myResourceGroup" \
    --name $STORAGEACCT \
    --query "primaryEndpoints.file" | tr -d '"')
 URI=$URI"myshare/myDirectory/SampleUpload.txt?sharesnapshot="$SNAPSHOT
 # Restore SampleUpload.txt from the share snapshot
az storage file copy start \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --source-uri $URI \
    --destination-share "myshare" \
    --destination-path "myDirectory/SampleUpload.txt"
```

### <a name="delete-a-share-snapshot"></a>공유 스냅샷 삭제
[`az storage share delete`](/cli/azure/storage/share) 명령을 사용하여 공유 스냅숏을 삭제할 수 있습니다. `--snapshot` 매개 변수에 대한 `$SNAPSHOT` 참조를 포함하는 변수를 사용합니다.

```azurecli-interactive
az storage share delete \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" \
    --snapshot $SNAPSHOT
```

## <a name="clean-up-resources"></a>리소스 정리
작업을 완료하면 [`az group delete`](/cli/azure/group) 명령을 사용하여 리소스 그룹 및 모든 관련된 리소스를 제거할 수 있습니다. 

```azurecli-interactive 
az group delete --name "myResourceGroup"
```

또는 개별적으로 리소스를 제거할 수 있습니다.
- 이 문서에서 만든 Azure 파일 공유를 제거하려면:

    ```azurecli-interactive
    az storage share delete \
        --account-name $STORAGEACCT \
        --account-key $STORAGEKEY \
        --name "myshare" \
        --delete-snapshots include

    az storage share delete \
        --account-name $STORAGEACCT \
        --account-key $STORAGEKEY \
        --name "myshare2" \
        --delete-snapshots include
    ```

- 저장소 계정 자체를 제거하려면 (이는 만든 Azure 파일 공유 및 Azure Blob Storage 컨테이너와 같은 다른 스토리지 리소스를 암시적으로 제거합니다.)

    ```azurecli-interactive
    az storage account delete \
        --resource-group "myResourceGroup" \
        --name $STORAGEACCT \
        --yes
    ```

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [Azure Files란?](storage-files-introduction.md)
