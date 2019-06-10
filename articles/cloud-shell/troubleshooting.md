---
title: Azure Cloud Shell 문제 해결 | Microsoft Docs
description: Azure Cloud Shell 문제 해결
services: azure
documentationcenter: ''
author: maertendMSFT
manager: hemantm
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: damaerte
ms.openlocfilehash: eb7deacc068661ca9a4f473ee2d36b7d4464c81c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60199462"
---
# <a name="troubleshooting--limitations-of-azure-cloud-shell"></a>Azure Cloud Shell의 문제 해결 및 제한 사항

Azure Cloud Shell의 문제 해결에 대해 알려진 해결 방법은 다음과 같습니다.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="general-troubleshooting"></a>일반적인 문제 해결

### <a name="early-timeouts-in-firefox"></a>FireFox의 조기 시간 초과

- **세부 정보**: Cloud Shell은 개방형 websocket을 사용하여 브라우저에 입/출력을 전달합니다. FireFox에는 Cloud Shell에서 조기 시간 초과를 야기하는 WebSocket을 닫을 수 있는 미리 설정된 정책이 있습니다.
- **해결 방법**: FireFox를 열고 URL 상자에서 "about:config"로 이동합니다. "network.websocket.timeout.ping.request"를 검색하고 값을 0에서 10으로 변경합니다.

### <a name="disabling-cloud-shell-in-a-locked-down-network-environment"></a>잠긴 네트워크 환경에서 Cloud Shell을 사용하지 않도록 설정

- **세부 정보**: 관리자는 사용자를 위해 Cloud Shell에 대한 액세스를 사용하지 않도록 설정할 수 있습니다. Cloud Shell은 거부될 수 있는 `ux.console.azure.com` 도메인에 대한 액세스를 활용하여 portal.azure.com, shell.azure.com, Visual Studio Code Azure 계정 확장 및 docs.microsoft.com을 포함한 Cloud Shell의 진입점에 대한 액세스를 중단합니다.
- **해결 방법**: 네트워크 설정을 통해 `ux.console.azure.com`에 대한 액세스를 사용자 환경으로 제한합니다. Cloud Shell 아이콘은 portal.azure.com에 여전히 존재하지만 서비스에 성공적으로 연결되지 않습니다.

### <a name="storage-dialog---error-403-requestdisallowedbypolicy"></a>스토리지 대화 상자 - 오류: 403 RequestDisallowedByPolicy

- **세부 정보**: Cloud Shell을 통해 스토리지 계정을 만들 때 관리자가 배치한 Azure 정책으로 인해 실패합니다. 오류 메시지에는 다음이 포함됩니다. `The resource action 'Microsoft.Storage/storageAccounts/write' is disallowed by one or more policies.`
- **해결 방법**: Azure 관리자에게 문의하여 스토리지 만들기를 거부하는 Azure 정책을 제거하거나 업데이트합니다.

### <a name="storage-dialog---error-400-disallowedoperation"></a>스토리지 대화 상자 - 오류: 400 DisallowedOperation

- **세부 정보**: Azure Active Directory 구독을 사용할 때 스토리지를 만들 수 없습니다.
- **해결 방법**: 스토리지 리소스를 만들 수 있는 Azure 구독을 사용합니다. Azure AD 구독으로는 Azure 리소스를 만들 수 없습니다.

### <a name="terminal-output---error-failed-to-connect-terminal-websocket-cannot-be-established-press-enter-to-reconnect"></a>터미널 출력 - 오류: 터미널에 연결할 수 없습니다. websocket을 설정할 수 없습니다. `Enter` 키를 눌러 다시 연결하세요.
- **세부 정보**: Cloud Shell을 사용하려면 Cloud Shell 인프라에 websocket 연결을 설정해야 합니다.
- **해결 방법**: https 요청 및 websocket 요청을 *.console.azure.com의 도메인에 보낼 수 있도록 네트워크 설정을 구성했는지 확인합니다.

### <a name="set-your-cloud-shell-connection-to-support-using-tls-12"></a>TLS 1.2를 사용하여 지원을 위한 Cloud Shell 연결 설정
 - **세부 정보**: Cloud Shell에 연결하는 데 사용할 TLS 버전을 정의하려면 브라우저 특정 설정을 지정해야 합니다.
 - **해결 방법**: 브라우저의 보안 설정으로 이동하고 "TLS 1.2 사용" 옆의 확인란을 선택합니다.

## <a name="bash-troubleshooting"></a>Bash 문제 해결

### <a name="cannot-run-the-docker-daemon"></a>Docker 디먼을 실행할 수 없음

- **세부 정보**: Cloud Shell은 컨테이너를 활용하여 셸 환경을 호스팅하므로 디먼 실행이 허용되지 않습니다.
- **해결 방법**: 기본적으로 설치되는 [docker-machine](https://docs.docker.com/machine/overview/)을 활용하여 원격 Docker 호스트에서 Docker 컨테이너를 관리합니다.

## <a name="powershell-troubleshooting"></a>PowerShell 문제 해결

### <a name="gui-applications-are-not-supported"></a>GUI 애플리케이션은 지원되지 않습니다.

- **세부 정보**: 사용자가 GUI 애플리케이션을 시작하면 프롬프트에서 반환하지 않습니다. 예를 들어 사용자가 2단계 인증을 사용할 수 있는 프라이빗 GitHub 리포지토리를 복제할 경우 2단계 인증을 완료하기 위한 대화 상자가 표시됩니다.
- **해결 방법**: 셸을 닫고 다시 엽니다.

### <a name="troubleshooting-remote-management-of-azure-vms"></a>Azure VM의 원격 관리 문제 해결
> [!NOTE]
> Azure VM에는 공용 연결 IP 주소가 있어야 합니다.

- **세부 정보**: WinRM에 대한 기본 Windows 방화벽 설정으로 인해 사용자에게 다음 오류가 나타날 수 있습니다. `Ensure the WinRM service is running. Remote Desktop into the VM for the first time and ensure it can be discovered.`
- **해결 방법**:  `Enable-AzVMPSRemoting`을 실행하여 대상 머신에서 PowerShell 원격의 모든 측면을 사용하도록 설정합니다.

### <a name="dir-does-not-update-the-result-in-azure-drive"></a>`dir`은 결과를 Azure 드라이브에 업데이트합니다.

- **세부 정보**: 기본적으로 사용자 환경을 최적화하기 위해 `dir`의 결과가 Azure 드라이브에 캐시됩니다.
- **해결 방법**: Azure 리소스를 만들거나 업데이트하거나 제거한 후에 `dir -force`를 실행하여 Azure 드라이브에서 결과를 업데이트합니다.

## <a name="general-limitations"></a>일반적인 제한 사항

Azure Cloud Shell에는 다음과 같이 알려진 제한 사항이 있습니다.

### <a name="system-state-and-persistence"></a>시스템 상태 및 지속성

Cloud Shell 세션을 제공하는 컴퓨터는 일시적이며 세션이 20분 동안 비활성화된 후 재순환됩니다. Cloud Shell은 Azure 파일 공유를 탑재해야 합니다. 따라서 Cloud Shell에 액세스하도록 구독에서 저장소 리소스를 설정할 수 있어야 합니다. 기타 고려 사항은 다음과 같습니다.

- 탑재된 저장소에서 `clouddrive` 디렉터리 내 수정 사항만 유지됩니다. Bash에서 `$HOME` 디렉터리도 유지됩니다.
- Azure 파일 공유는 [할당된 지역](persisting-shell-storage.md#mount-a-new-clouddrive) 내에서만 탑재될 수 있습니다.
  - Bash에서 `ACC_LOCATION`로 설정된 해당 지역을 찾으려면 `env`을 실행합니다.
- Azure 파일은 로컬 중복 저장소 및 지역 중복 저장소 계정만 지원합니다.

### <a name="browser-support"></a>브라우저 지원

Cloud Shell은 다음과 같은 최신 버전 브라우저를 지원합니다.

- Microsoft Edge
- Microsoft Internet Explorer
- Google Chrome
- Mozilla Firefox
- Apple Safari
  - Safari는 개인 모드에서 지원되지 않습니다.

### <a name="copy-and-paste"></a>복사 및 붙여넣기

[!INCLUDE [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="usage-limits"></a>사용 제한

Cloud Shell은 대화형 사용 사례를 위한 것입니다. 따라서 비대화형 세션을 오래 실행하면 경고 없이 종료됩니다.

### <a name="user-permissions"></a>사용자 권한

권한은 sudo 액세스 권한이 없는 일반 사용자로 설정됩니다. 사용자 `$Home` 디렉터리 외부에서의 설치는 유지되지 않습니다.

## <a name="bash-limitations"></a>Bash 제한 사항

### <a name="editing-bashrc"></a>.bashrc 편집

.bashrc를 편집할 때는 Cloud Shell에 예기치 않은 오류가 발생할 수 있으니 주의하세요.

## <a name="powershell-limitations"></a>PowerShell 제한 사항

### <a name="preview-version-of-azuread-module"></a>AzureAD 모듈의 미리 보기 버전

현재 .NET 표준 기반의 미리 보기인 `AzureAD.Standard.Preview` 모듈은 사용할 수 있습니다. 이 모듈은 `AzureAD`와 동일한 기능을 제공합니다.

### <a name="sqlserver-module-functionality"></a>`SqlServer` 모듈 기능

Cloud Shell에 포함된 `SqlServer` 모듈은 PowerShell Core에 대해 평가판 지원만 제공합니다. 특히 `Invoke-SqlCmd`는 아직 사용할 수 없습니다.

### <a name="default-file-location-when-created-from-azure-drive"></a>Azure 드라이브에서 만들 때 기본 파일 위치

PowerShell cmdlet을 사용하면 사용자가 Azure 드라이브에 파일을 만들 수 없습니다. 사용자가 vim 또는 nano 등의 다른 도구를 사용하여 새 파일을 만들 때 파일은 기본적으로 `$HOME` 폴더에 저장됩니다.

### <a name="tab-completion-can-throw-psreadline-exception"></a>탭 완성 기능은 PSReadline 예외를 throw할 수 있습니다.

사용자의 PSReadline EditMode가 Emacs로 설정되어 있고 해당 사용자가 탭 완성 기능을 통해 가능한 모든 항목을 표시하려고 하는데, 가능한 모든 항목을 표시하기에 창 크기가 너무 작은 경우 PSReadline은 처리되지 않은 예외를 throw합니다.

### <a name="large-gap-after-displaying-progress-bar"></a>진행률 표시줄을 표시한 후의 큰 간격

명령 또는 사용자 작업이 `Azure:` 드라이브에 있는 동안 탭 완성 기능처럼 진행률 표시줄을 표시하는 경우, 커서가 제대로 설정되지 않고, 진행률 표시줄이 이전에 있던 위치에 간격이 나타날 수 있습니다.

### <a name="random-characters-appear-inline"></a>임의의 문자가 인라인으로 표시됩니다.

커서 위치 시퀀스 코드(예: `5;13R`)가 사용자 입력에 나타날 수 있습니다. 문자는 수동으로 제거할 수 있습니다.

## <a name="personal-data-in-cloud-shell"></a>Cloud Shell에서 개인 데이터

Azure Cloud Shell은 개인 데이터를 중대하게 사용하며, Azure Cloud Shell 서비스에 의해 캡처되고 저장되는 데이터는 가장 최근에 사용된 셸, 기본 설정된 글꼴 크기, 기본 설정된 글꼴 유형 및 클라우드 드라이브로 백업하는 파일 공유 세부 정보와 같은 환경에 대한 기본값을 제공하는 데 사용됩니다. 이 데이터를 내보내거나 삭제하려는 경우 다음 지침을 사용합니다.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

### <a name="export"></a>내보내기
사용자 설정을 **내보내기** 위해 Cloud Shell은 다음 명령을 실행하여 기본 설정된 셸, 글꼴 크기 및 글꼴 종류 등을 저장합니다.

1. [![](https://shell.azure.com/images/launchcloudshell.png "Azure Cloud Shell 시작")](https://shell.azure.com)
2. Bash 또는 PowerShell에서 다음 명령을 실행합니다.

Bash:

  ```
  token="Bearer $(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s | jq -r ".access_token")"
  curl https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -H Authorization:"$token" -s | jq
  ```

PowerShell:

  ```powershell
  $token= ((Invoke-WebRequest -Uri "$env:MSI_ENDPOINT`?resource=https://management.core.windows.net/" -Headers @{Metadata='true'}).content |  ConvertFrom-Json).access_token
  ((Invoke-WebRequest -Uri https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -Headers @{Authorization = "Bearer $token"}).Content | ConvertFrom-Json).properties | Format-List
```

### <a name="delete"></a>삭제
사용자 설정을 **삭제하기** 위해 Cloud Shell은 다음 명령을 실행하여 기본 설정된 셸, 글꼴 크기 및 글꼴 종류 등을 저장합니다. 다음 번에 Cloud Shell을 시작하면 파일 공유를 다시 등록할지 묻는 메시지가 표시됩니다. 

>[!Note]
> 사용자 설정을 삭제하는 경우 실제 Azure Files 공유는 삭제되지 않습니다. 해당 작업을 완료하려면 Azure Files로 이동합니다.

1. [![](https://shell.azure.com/images/launchcloudshell.png "Azure Cloud Shell 시작")](https://shell.azure.com)
2. Bash 또는 PowerShell에서 다음 명령을 실행합니다.

Bash:

  ```
  token="Bearer $(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s | jq -r ".access_token")"
  curl -X DELETE https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -H Authorization:"$token"
  ```

PowerShell:

  ```powershell
  $token= ((Invoke-WebRequest -Uri "$env:MSI_ENDPOINT`?resource=https://management.core.windows.net/" -Headers @{Metadata='true'}).content |  ConvertFrom-Json).access_token
  Invoke-WebRequest -Method Delete -Uri https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -Headers @{Authorization = "Bearer $token"}
  ```
