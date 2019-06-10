---
title: Azure PowerShell 스크립트 샘플 - 다른 지역의 저장소 계정으로 관리 디스크의 VHD 내보내기/복사 | Microsoft Docs
description: Azure PowerShell 스크립트 샘플 - 같거나 다른 지역의 저장소 계정으로 관리 디스크의 VHD 내보내기/복사
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/17/2018
ms.author: ramankum
ms.openlocfilehash: 1bb116f2a2153515f3b61c050f0c952523c13528
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/09/2019
ms.locfileid: "55976631"
---
# <a name="exportcopy-the-vhd-of-a-managed-disk-to-a-storage-account-in-different-region-with-powershell"></a>PowerShell을 사용하여 다른 지역의 저장소 계정으로 관리 디스크의 VHD 내보내기/복사

이 스크립트는 관리 디스크의 VHD를 다른 지역의 저장소 계정으로 내보냅니다. 먼저 관리 디스크의 SAS URI를 생성한 다음, 이를 사용하여 다른 지역의 저장소 계정으로 기본 VHD를 복사합니다. 이 스크립트를 사용하여 지역별 확장을 위해 관리 디스크를 다른 지역으로 복사합니다.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="sample-script"></a>샘플 스크립트

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-managed-disks-vhd-to-storage-account/copy-managed-disks-vhd-to-storage-account.ps1 "Copy the VHD of a managed disk")]


## <a name="script-explanation"></a>스크립트 설명

이 스크립트에서는 다음 명령을 사용하여 관리 디스크의 SAS URI를 생성하고 SAS URI를 사용하여 저장소 계정에 기본 VHD를 복사합니다. 테이블에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [Grant-AzDiskAccess](https://docs.microsoft.com/powershell/module/az.compute/grant-azdiskaccess) | 저장소 계정에 기본 VHD를 복사하는 데 사용되는 관리 디스크에 대한 SAS URI를 생성합니다. |
| [New-AzureStorageContext](https://docs.microsoft.com/powershell/module/azure.storage/New-AzureStorageContext) | 계정 이름과 키를 사용하여 저장소 계정 컨텍스트를 만듭니다. 이 컨텍스트는 저장소 계정에 대한 읽기/쓰기 작업을 수행하는 데 사용할 수 있습니다. |
| [Start-AzureStorageBlobCopy](https://docs.microsoft.com/powershell/module/azure.storage/Start-AzureStorageBlobCopy) | 스냅샷의 기본 VHD를 저장소 계정에 복사합니다. |

## <a name="next-steps"></a>다음 단계

[VHD에서 관리 디스크 만들기](virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json)

[관리 디스크에서 가상 머신 만들기](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Azure PowerShell 모듈에 대한 자세한 내용은 [Azure PowerShell 설명서](/powershell/azure/overview)를 참조하세요.

추가 가상 머신 PowerShell 스크립트 샘플은 [Azure Windows VM 설명서](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)에서 확인할 수 있습니다.