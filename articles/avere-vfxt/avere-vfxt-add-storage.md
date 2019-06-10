---
title: Avere vFXT 저장소 구성 - Azure
description: Avere vFXT for Azure에 백 엔드 저장소 시스템을 추가하는 방법입니다.
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: v-erkell
ms.openlocfilehash: 6d35d5cdeafb80a36f910d71393802a3affb4df8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60515893"
---
# <a name="configure-storage"></a>저장소 구성

이 단계에서는 vFXT 클러스터용 백 엔드 스토리지 시스템을 설정합니다.

> [!TIP]
> Avere vFXT 클러스터와 함께 새 Blob 컨테이너를 만든 경우 해당 컨테이너는 이미 사용하도록 설정되어 있으므로 스토리지를 추가할 필요가 없습니다.

클러스터와 함께 새 Blob 컨테이너를 만들지 않았거나 추가 하드웨어 또는 클라우드 기반 스토리지 시스템을 추가하려는 경우 다음 지침을 따릅니다.

다음 두 가지 주요 작업이 있습니다.

1. [코어 파일러 만들기](#create-a-core-filer) - vFXT 클러스터를 기존 스토리지 시스템 또는 Azure Storage 계정에 연결합니다.

1. [네임스페이스 접합 만들기](#create-a-junction) - 클라이언트에서 탑재할 경로를 정의합니다.

이러한 단계에서는 Avere 제어판을 사용합니다. 사용 방법을 알아보려면 [vFXT 클러스터에 액세스](avere-vfxt-cluster-gui.md)를 참조하세요.

## <a name="create-a-core-filer"></a>코어 파일러 만들기

"코어 파일러"는 백 엔드 저장소 시스템에 대한 vFXT 용어입니다. 저장소는 NetApp 또는 Isilon과 같은 하드웨어 NAS 어플라이언스이거나 클라우드 개체 저장소일 수 있습니다. 코어 파일러에 대한 자세한 내용은 [Avere 클러스터 설정 가이드](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/settings_overview.html#managing-core-filers)에서 찾을 수 있습니다.

코어 파일러를 추가하려면 다음 두 가지 주요 유형 중 하나의 코어 파일러를 선택합니다.

  * [NAS 코어 파일러](#nas-core-filer) - NAS 코어 파일러를 추가하는 방법을 설명합니다 
  * [Azure Storage 클라우드 코어 파일러](#azure-storage-cloud-core-filer) - Azure Storage 계정을 클라우드 코어 파일러로 추가하는 방법을 설명합니다.

### <a name="nas-core-filer"></a>NAS 코어 파일러

NAS 코어 파일러는 온-프레미스 NetApp 또는 Isilon이거나 클라우드의 NAS 엔드포인트일 수 있습니다. 저장소 시스템은 Avere vFXT 클러스터(예: 1GBps ExpressRoute 연결(VPN 아님))에 대한 안정적인 고속 연결이 있어야 하며, 사용되는 NAS 내보내기에 대한 클러스터 루트 액세스 권한을 제공해야 합니다.

NAS 코어 파일러를 추가하는 단계는 다음과 같습니다.

1. Avere 제어판의 위쪽에서 **설정** 탭을 클릭합니다.

1. 왼쪽에서 **코어 파일러** > **코어 파일러 관리**를 차례로 클릭합니다.

1. **만들기**를 클릭합니다.

   ![만들기 단추 위로 커서가 있는 새 코어 필터 추가 페이지의 스크린샷](media/avere-vfxt-add-core-filer-start.png)

1. 마법사에서 필수 정보를 입력합니다. 

   * 코어 파일러의 이름을 지정합니다.
   * 사용 가능한 경우 FQDN(정규화된 도메인 이름)을 제공합니다. 그렇지 않으면 코어 파일러로 확인되는 IP 주소 또는 호스트 이름을 제공합니다.
   * 목록에서 파일러 종류를 선택합니다. 확실하지 않으면 **기타**를 선택합니다.

     ![코어 파일러 이름과 해당 정규화된 도메인 이름이 있는 새 코어 파일러 추가 페이지의 스크린샷](media/avere-vfxt-add-core-filer.png)
  
   * **다음**을 클릭하고 캐시 정책을 선택합니다. 
   * **파일러 추가**를 클릭합니다.
   * 자세한 내용은 Avere 클러스터 설정 가이드의 [새 NAS 코어 파일러 추가](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/new_core_filer_nas.html)를 참조하세요.

그런 다음, [접합 만들기](#create-a-junction)로 계속 진행합니다.  

### <a name="azure-storage-cloud-core-filer"></a>Azure Storage 클라우드 코어 파일러

Azure Blob Storage를 vFXT 클러스터의 백 엔드 스토리지로 사용하려면 코어 파일러로 추가할 빈 컨테이너가 필요합니다.

> [!TIP] 
> Avere vFXT 클러스터를 만드는 동시에 Blob 컨테이너를 만들도록 선택하면 배포 템플릿 또는 스크립트가 스토리지 컨테이너를 만들어 코어 파일러로 정의한 다음 vFXT 클러스터 만들기의 일환으로 네임스페이스 접합을 만듭니다. 또한 템플릿은 클러스터의 가상 네트워크 내에서 저장소 서비스 끝점을 만듭니다. 

Blob Storage를 클러스터에 추가하려면 다음 작업이 필요합니다.

* 저장소 계정 만들기(아래 1단계)
* 빈 Blob 컨테이너 만들기(2-3단계)
* 저장소 액세스 키를 vFXT 클러스터에 대한 클라우드 자격 증명으로 추가(4-6단계)
* Blob 컨테이너를 vFXT 클러스터용 코어 파일러로 추가(7-9단계)
* 클라이언트에서 코어 파일러를 탑재하는 데 사용하는 네임스페이스 접합 만들기([접합 만들기](#create-a-junction), 하드웨어 및 클라우드 저장소 모두에 대해 동일함)

클러스터를 만든 후에 Blob Storage를 추가하려면 다음 단계를 수행합니다. 

1. 다음 설정을 사용하여 범용 V2 저장소 계정을 만듭니다.

   * **구독** - vFXT 클러스터와 동일
   * **리소스 그룹** - vFXT 클러스터 그룹과 동일(선택 사항)
   * **위치** - vFXT 클러스터와 동일
   * **성능** - 표준(Premium Storage는 지원되지 않음)
   * **계정 종류** - 범용 V2(StorageV2)
   * **복제** - LRS(로컬 중복 저장소)
   * **액세스 계층** - 핫
   * **보안 전송 필요** - 이 옵션은 사용하지 않도록 설정함(기본값이 아님)
   * **가상 네트워크** -필수 아님

   Azure Portal을 사용하거나 아래의 "Azure에 배포" 단추를 클릭합니다.

   [![저장소 계정 만들기 단추](media/deploytoazure.png)](https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FAvere%2Fmaster%2Fsrc%2Fvfxt%2Fstorageaccount%2Fazuredeploy.json)

1. 계정이 만들어지면 저장소 계정 페이지로 이동합니다.

   ![Azure Portal의 새 저장소 계정](media/avere-vfxt-new-storage-acct.png)

1. 개요 페이지에서 **Blob**을 클릭한 다음, **+ 컨테이너**를 클릭하여 Blob 컨테이너를 만듭니다. 컨테이너 이름을 사용하고, 액세스가 **프라이빗**으로 설정되어 있는지 확인합니다.

   ![기존 컨테이너가 없는 저장소 Blob 페이지](media/avere-vfxt-blob-no-container.png)

1. **설정** 아래에서 **액세스 키**를 클릭하여 Azure Storage 계정 키를 가져옵니다.

   ![키 복사를 위한 Azure Portal GUI](media/avere-vfxt-copy-storage-key.png) 

1. 클러스터에 대한 Avere 제어판을 엽니다. **설정**을 클릭한 다음, 왼쪽 탐색 창에서 **클러스터** > **클라우드 자격 증명**을 차례로 엽니다. [클라우드 자격 증명] 페이지에서 **자격 증명 추가**를 클릭합니다.

   ![[클라우드 자격 증명] 구성 페이지에서 [자격 증명 추가] 단추 클릭](media/avere-vfxt-new-credential-button.png)

1. 다음 정보를 입력하여 클라우드 코어 파일러에 대한 자격 증명을 만듭니다. 

   | 필드 | Value |
   | --- | --- |
   | 자격 증명 이름 | 설명이 포함된 이름 |
   | 서비스 유형 | (Azure Storage 액세스 키 선택) |
   | 테넌트 | 저장소 계정 이름 |
   | 구독 | 구독 ID |
   | 저장소 액세스 키 | Azure 저장소 계정 키(이전 단계에서 복사됨) | 

   **제출**을 클릭합니다.

   ![Avere 제어판에서 완성된 클라우드 자격 증명 양식](media/avere-vfxt-new-credential-submit.png)

1. 다음으로, 코어 파일러를 만듭니다. Avere 제어판의 왼쪽에서 **코어 파일러** >  **코어 파일러 관리**를 차례로 클릭합니다. 

1. **코어 파일러 관리** 설정 페이지에서 **만들기** 단추를 클릭합니다.

1. 마법사를 작성합니다.

   * **클라우드** 파일러 유형을 선택합니다. 
   * 새 코어 파일러의 이름을 지정하고, **다음**을 클릭합니다.
   * 기본 캐시 정책을 적용하고 세 번째 페이지로 계속 진행합니다.
   * **서비스 유형**에서 **Azure 저장소**를 선택합니다. 
   * 이전에 만든 자격 증명을 선택합니다.
   * **버킷 내용**을 **비어 있음**으로 설정합니다.
   * **인증서 확인**을 **사용 안 함**으로 변경합니다.
   * **압축 모드**를 **없음**으로 변경합니다.  
   * **다음**을 클릭합니다.
   * 네 번째 페이지에서 **버킷 이름**의 컨테이너 이름을 *storage_account_name*/*container_name*으로 입력합니다.
   * 필요에 따라 **암호화 유형**을 **없음**으로 설정합니다.  Azure Storage는 기본적으로 암호화되어 있습니다.
   * **파일러 추가**를 클릭합니다.

   자세한 내용은 Avere 클러스터 구성 가이드의 [새 클라우드 코어 파일러 추가](<https://azure.github.io/Avere/legacy/ops_guide/4_7/html/new_core_filer_cloud.html>)를 참조하세요. 

페이지가 새로 고쳐지거나 페이지를 새로 고쳐 새 코어 파일러를 표시할 수 있습니다.

다음으로, [접합을 만들어야](#create-a-junction) 합니다.

## <a name="create-a-junction"></a>접합 만들기

접합은 클라이언트에 대해 만드는 경로입니다. 클라이언트에서 경로를 탑재하고 선택한 대상에 도달합니다.

예를 들어 `/avere/files`를 만들어 `/vol0/data` NetApp 코어 파일러 내보내기 및 `/project/resources` 하위 디렉터리에 매핑할 수 있습니다.

접합에 대한 자세한 내용은 [Avere 클러스터 구성 가이드의 네임스페이스 섹션](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_namespace.html)에서 찾을 수 있습니다.

Avere 제어판 설정 인터페이스에서 다음 단계를 수행합니다.

* 왼쪽 위에서 **VServer** > **네임스페이스**를 차례로 클릭합니다.
* ``/avere/data``와 같이 /(슬래시)로 시작하는 네임스페이스 경로를 제공합니다.
* 코어 파일러를 선택합니다.
* 코어 파일러 내보내기를 선택합니다.
* **다음**을 클릭합니다.

  ![접합, 코어 파일러 및 내보내기에 대해 완성된 필드가 있는 "새 접합 추가" 페이지의 스크린샷](media/avere-vfxt-add-junction.png)

몇 초 후에 접합이 표시됩니다. 필요에 따라 추가 접합을 만듭니다.

접합이 만들어지면 클라이언트에서 [Avere vFXT 클러스터를 탑재](avere-vfxt-mount-clients.md)하여 파일 시스템에 액세스할 수 있습니다.
