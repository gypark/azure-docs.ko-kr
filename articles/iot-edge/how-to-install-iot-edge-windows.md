---
title: Windows에 Azure IoT Edge 설치 | Microsoft Docs
description: Windows 10, Windows Server 및 Windows IoT Core에 대한 Azure IoT Edge 설치 지침
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: kgremban
ms.custom: seodec18
ms.openlocfilehash: 8907ae61fb03b417a74eb32e1fd09aece75d5e2c
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66151714"
---
# <a name="install-the-azure-iot-edge-runtime-on-windows"></a>Windows에 Azure IoT Edge 런타임 설치

Azure IoT Edge 런타임은 디바이스를 IoT Edge 디바이스로 바꿔줍니다. 런타임은 Raspberry Pi처럼 작은 디바이스 또는 산업용 서버처럼 큰 디바이스에 배포할 수 있습니다. IoT Edge 런타임을 사용하여 디바이스를 구성하면 클라우드에서 디바이스에 비즈니스 논리를 배포할 수 있습니다. 

IoT Edge 런타임에 대한 자세한 내용은 [Azure IoT Edge 런타임 및 해당 아키텍처 이해](iot-edge-runtime.md)를 참조하세요.

이 문서에 Windows x64 (Intel/AMD)에 Azure IoT Edge 런타임을 설치 하는 단계를 나열 합니다. Windows 컨테이너를 사용 하 여 시스템입니다.

> [!NOTE]
> Windows 운영 체제를 눌러도 절전 모드 및 IoT Edge 모듈 (Windows Nano Server 컨테이너 프로세스 격리)을 실행 하는 경우 전원 상태를 최대 절전 모드로 전환할 수 없습니다. 이 문제는 배터리 장치에 영향을 줍니다.
>
> 대 안으로 명령을 사용 하 여 `Stop-Service iotedge` 이러한 전원 상태를 사용 하기 전에 실행 중인 모든 IoT Edge 모듈을 중지 합니다. 

<!--
> [!NOTE]
> Using Linux containers on Windows systems is not a recommended or supported production configuration for Azure IoT Edge. However, it can be used for development and testing purposes.
-->

Windows 시스템에서 Linux 컨테이너를 사용하는 것은 Azure IoT Edge에 추천되거나 지원되는 프로덕션 구성이 아닙니다. 그러나 개발 및 테스트 용도로만 사용할 수 있습니다. 자세한 내용은 참조 하세요 [Linux 컨테이너를 실행 하는 Windows에서 사용 하 여 IoT Edge](how-to-install-iot-edge-windows-with-linux.md)합니다.

IoT Edge의 최신 버전에는 무엇이 포함 하는 방법에 대 한 내용은 [Azure IoT Edge 해제](https://github.com/Azure/azure-iotedge/releases)합니다.

## <a name="prerequisites"></a>필수 조건

이 섹션을 사용하여 Windows 디바이스에서 IoT Edge를 지원할 수 있는지 여부를 검토하고 설치 전에 컨테이너 엔진에 대해 준비합니다. 

### <a name="supported-windows-versions"></a>지원되는 Windows 버전

개발 및 테스트 시나리오에 대 한 Windows 컨테이너를 사용 하 여 Azure IoT Edge는 Windows 10 또는 Windows Server 2019 (빌드 17763) 컨테이너 기능을 지 원하는 모든 버전에 설치할 수 있습니다. 에 대 한 운영 체제는 현재 지원 되는 프로덕션 시나리오에 대 한 내용은 [지원 되는 Azure IoT Edge 체제](support.md#operating-systems)합니다. 

### <a name="prepare-for-a-container-engine"></a>컨테이너 엔진 준비 

Azure IoT Edge는 [OCI 호환](https://www.opencontainers.org/) 컨테이너 엔진을 사용합니다. 프로덕션 시나리오에서는 설치 스크립트에 포함된 Moby 엔진을 사용하여 Windows 디바이스에서 Windows 컨테이너를 실행합니다. 

## <a name="install-iot-edge-on-a-new-device"></a>새 디바이스에 IoT Edge 설치

>[!NOTE]
>Azure IoT Edge 소프트웨어 패키지에는 패키지(LICENSE 디렉터리)에 있는 사용 조건이 적용됩니다. 패키지를 사용하기 전에 사용 조건을 읽어보시기 바랍니다. 패키지를 설치 및 사용하면 이러한 사용 조건에 동의하게 됩니다. 사용 조건에 동의하지 않는 경우, 패키지를 사용하지 마세요.

PowerShell 스크립트가 Azure IoT Edge 보안 디먼을 다운로드하여 설치합니다. 그런 다음, 보안 디먼이 다른 모듈의 원격 배포를 가능하게 하는 두 가지 런타임 모듈 중 첫 번째인 IoT Edge 에이전트를 시작합니다. 

디바이스에 IoT Edge 런타임을 처음 설치하는 경우 IoT 허브의 ID를 사용하여 디바이스를 프로비전해야 합니다. 단일 IoT Edge 장치를 IoT Hub에서 제공 하는 장치 연결 문자열을 사용 하 여 수동으로 프로 비전 할 수 있습니다. 또는 Device Provisioning Service를 사용하여 자동으로 디바이스를 프로비전할 수 있습니다. 이 기능은 많은 디바이스를 설정해야 하는 경우에 유용합니다. 프로비전 선택 사항에 따라 적절한 설치 스크립트를 선택합니다. 

다음 섹션에서는 새 디바이스의 IoT Edge 설치 스크립트에 대한 일반적인 사용 사례 및 매개 변수를 설명합니다. 

### <a name="option-1-install-and-manually-provision"></a>옵션 1: 설치 및 수동으로 프로비전

이 첫 번째 옵션에서 제공 하는 **장치 연결 문자열** IoT Hub 장치 프로 비전 하 여 생성. 

이 예제에서는 Windows 컨테이너를 사용 하 여 수동 설치를 보여 줍니다.

1. 이미 않았다면 새 IoT Edge 장치를 등록 하 고 검색 합니다 **장치 연결 문자열**. 이 섹션에서 나중에 사용 하려면 연결 문자열을 복사 합니다. 다음 도구를 사용 하 여이 단계를 완료할 수 있습니다.

   * [Azure Portal](how-to-register-device-portal.md)
   * [Azure CLI](how-to-register-device-cli.md)
   * [Visual Studio Code](how-to-register-device-vscode.md)

2. PowerShell을 관리자 권한으로 실행합니다.

   >[!NOTE]
   >IoT Edge를 하지 PowerShell (x86)를 설치 하는 AMD64 세션의 PowerShell 사용 합니다. 사용 중인 세션 형식을 잘 모르는 경우 다음 명령을 실행 합니다.
   >
   >```powershell
   >(Get-Process -Id $PID).StartInfo.EnvironmentVariables["PROCESSOR_ARCHITECTURE"]
   >```

3. 합니다 **배포 IoTEdge** 명령 인지 확인 하 Windows 컴퓨터에서 지원 되는 버전, 컨테이너 기능을 켭니다 모 비 런타임 및 IoT Edge 런타임에서 다운로드 합니다. 이 명령은 Windows 컨테이너를 사용 하 여 기본값으로 사용 됩니다. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge
   ```

4. 이 시점에서 IoT Core 장치 수 자동으로 다시 시작 합니다. 다른 Windows 10 또는 Windows Server 장치에서 다시 시작할 것인지 묻는 메시지를 표시할 수 있습니다. 그렇다면 이제 장치 다시 시작 합니다. 일단 장치가 준비 되 면 관리자 권한으로 PowerShell을 다시 실행 합니다.

5. **Initialize IoTEdge** 명령은 사용자의 머신에서 IoT Edge 런타임을 구성합니다. 이 명령은 Windows 컨테이너를 통한 수동 프로비저닝으로 기본 설정됩니다. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge
   ```

6. 메시지가 표시되면 1단계에서 검색된 장치 연결 문자열을 제공합니다. 장치 연결 문자열은 물리적 장치를 IoT Hub의 장치 ID와 연결합니다. 

   장치 연결 문자열을 다음 형식으로 및 따옴표를 포함 하지 않아야 합니다. `HostName={IoT hub name}.azure-devices.net;DeviceId={device name};SharedAccessKey={key}`

7. 단계를 따르세요 [성공적인 설치 확인](#verify-successful-installation) 장치에서 IoT Edge의 상태를 확인 합니다. 

디바이스를 수동으로 설치하고 프로비전하는 경우 추가 매개 변수를 사용하여 다음과 같이 설치를 수정할 수 있습니다.
* 트래픽이 프록시 서버를 통과하도록 설정
* 설치 관리자에서 오프라인 디렉터리를 확인하도록 설정
* 특정 에이전트 컨테이너 이미지를 선언하고, 프라이빗 레지스트리에 있는 경우 자격 증명 제공

이러한 설치 옵션에 대 한 자세한 내용은 건너 뛰 세요에 대해 자세히 알아보려면 [모든 설치 매개 변수](#all-installation-parameters)합니다.

### <a name="option-2-install-and-automatically-provision"></a>옵션 2: 설치 및 자동으로 프로비전

이 두 번째 옵션에서는 IoT Hub Device Provisioning Service를 사용하여 디바이스를 프로비전합니다. Device Provisioning Service 인스턴스의 **범위 ID** 및 디바이스의 **등록 ID**를 제공합니다.

다음 예제에서는 Windows 컨테이너를 사용 하 여 자동 설치를 보여 줍니다.

1. 단계에 따라 [만들기 및 Windows에서 시뮬레이션된 된 TPM IoT Edge 장치를 프로 비전](how-to-auto-provision-simulated-device-windows.md) Device Provisioning Service 설정 및 검색 하려면 해당 **범위 ID**를 TPM 장치를 시뮬레이션 하 고 해당 검색**등록 ID**, 개별 등록을 만듭니다. 장치의 IoT hub에 등록 되 면 설치 단계를 사용 하 여 계속 합니다.  

   >[!TIP]
   >설치 및 테스트를 수행하는 동안 TPM 시뮬레이터를 실행하는 창을 열린 상태로 유지합니다. 

2. PowerShell을 관리자 권한으로 실행합니다.

   >[!NOTE]
   >IoT Edge를 하지 PowerShell (x86)를 설치 하는 AMD64 세션의 PowerShell 사용 합니다. 사용 중인 세션 형식을 잘 모르는 경우 다음 명령을 실행 합니다.
   >
   >```powershell
   >(Get-Process -Id $PID).StartInfo.EnvironmentVariables["PROCESSOR_ARCHITECTURE"]
   >```

3. 합니다 **배포 IoTEdge** 명령 인지 확인 하 Windows 컴퓨터에서 지원 되는 버전, 컨테이너 기능을 켭니다 모 비 런타임 및 IoT Edge 런타임에서 다운로드 합니다. 이 명령은 Windows 컨테이너를 사용 하 여 기본값으로 사용 됩니다. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge
   ```

4. 이 시점에서 IoT Core 장치 수 자동으로 다시 시작 합니다. 다른 Windows 10 또는 Windows Server 장치에서 다시 시작할 것인지 묻는 메시지를 표시할 수 있습니다. 그렇다면 이제 장치 다시 시작 합니다. 일단 장치가 준비 되 면 관리자 권한으로 PowerShell을 다시 실행 합니다.

6. **Initialize IoTEdge** 명령은 사용자의 머신에서 IoT Edge 런타임을 구성합니다. 이 명령은 Windows 컨테이너를 통한 수동 프로비저닝으로 기본 설정됩니다. 사용 된 `-Dps` 수동 프로 비전 하는 대신 Device Provisioning Service를 사용 하는 플래그입니다.

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -Dps
   ```

7. 메시지가 표시 되 면 Device Provisioning Service의 범위 ID 및 장치 둘 다 해야의 검색 1 단계에서에서 등록 ID를 제공 합니다.

8. 단계를 따르세요 [성공적인 설치 확인](#verify-successful-installation) 장치에서 IoT Edge의 상태를 확인 합니다. 

디바이스를 수동으로 설치하고 프로비전하는 경우 추가 매개 변수를 사용하여 다음과 같이 설치를 수정할 수 있습니다.
* 트래픽이 프록시 서버를 통과하도록 설정
* 설치 관리자에서 오프라인 디렉터리를 확인하도록 설정
* 특정 에이전트 컨테이너 이미지를 선언하고, 프라이빗 레지스트리에 있는 경우 자격 증명 제공

이러한 설치 옵션에 대한 자세한 내용을 보려면 이 문서를 계속 읽거나 [모든 설치 매개 변수](#all-installation-parameters)에 대한 자세한 정보로 건너뛰세요.

## <a name="offline-installation"></a>오프라인 설치

설치 하는 동안 두 개의 파일이 다운로드 됩니다. 
* Microsoft Azure IoT Edge cab-IoT Edge 보안 디먼 (iotedged), 모 비 컨테이너 엔진 및 모 비 CLI를 포함 합니다.
* Visual C++ 재배포 가능 패키지(VC 런타임) msi

장치에 사전에 이러한 파일 중 하나 또는 모두 다운로드 다음 파일이 포함 된 디렉터리에 설치 스크립트를 가리킬 수 있습니다. 설치 관리자는 먼저 디렉터리를 확인한 다음, 디렉터리에 없는 구성 요소만 다운로드합니다. 모든 파일을 사용할 수 있는 오프 라인으로 인터넷 연결 없이 설치할 수 있습니다. 구성 요소의 특정 버전을 설치 하려면이 기능을 사용할 수도 있습니다.  

이전 버전과 함께 최신 IoT Edge 설치 파일을 참조 하세요 [Azure IoT Edge 해제](https://github.com/Azure/azure-iotedge/releases)합니다.

오프 라인 구성 요소를 설치 하려면 사용 된 `-OfflineInstallationPath` 매개 변수는 배포 IoTEdge의 일부로 명령 및 파일 디렉터리의 절대 경로 제공 합니다. 예를 들면 다음과 같습니다.

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Deploy-IoTEdge -OfflineInstallationPath C:\Downloads\iotedgeoffline
```

또한이 문서의 뒷부분에 나오는 도입 업데이트 IoTEdge 명령으로 오프 라인 설치 경로 매개 변수를 사용할 수 있습니다. 

## <a name="verify-successful-installation"></a>성공적인 설치 확인

IoT Edge 서비스의 상태를 확인합니다. 실행 중으로 표시되어야 합니다.  

```powershell
Get-Service iotedge
```

최근 5분 간의 서비스 로그를 검사합니다. IoT Edge 런타임 설치를 완료, 실행 간격에서 발생 한 오류 목록이 표시 될 수 있습니다 **배포 IoTEdge** 하 고 **Initialize IoTEdge**합니다. 이러한 오류는 서비스를 구성 하기 전에 시작 하려고으로 예상 됩니다. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

실행 중인 모듈을 나열합니다. 새로 설치를 실행 하는 것을 표시만 모듈 뒤 **edge 에이전트**합니다. 한 후 [IoT Edge 모듈 배포](how-to-deploy-modules-portal.md), 다른 사용자가 표시 됩니다. 

```powershell
iotedge list
```

## <a name="manage-module-containers"></a>모듈 컨테이너 관리

IoT Edge 서비스는 장치에서 실행 하는 컨테이너 엔진에 필요 합니다. 장치에 모듈을 배포할 때 IoT Edge 런타임에서 컨테이너 엔진을 사용 하 여 클라우드에서 레지스트리에서 컨테이너 이미지를 끌어옵니다. IoT Edge 서비스 모듈을 사용 하 여 상호 작용 하 고 로그를 검색할 수 있습니다 하지만 컨테이너 엔진을 사용 하 여 컨테이너 자체와 상호 작용 하도록 하려는 경우가 있습니다. 

모듈 개념에 대 한 자세한 내용은 참조 하세요. [이해 Azure IoT Edge 모듈](iot-edge-modules.md)합니다. 

Windows IoT Edge 장치에서 Windows 컨테이너를 실행 하는 경우 IoT Edge 설치 모 비 컨테이너 엔진을 포함 합니다. 모 비 Docker와 같은 표준을 기반 엔진과 동일한 Docker 데스크톱 컴퓨터에서 병렬로 실행 하도록 설계 되었습니다. 이런 이유로 모 비 엔진에 의해 관리 되는 대상 컨테이너 하려는 경우 특히 Docker 대신 해당 엔진을 대상으로 합니다. 

예를 들어, 모든 Docker 이미지를 나열 하려면 다음 명령을 사용 합니다.

```powershell
docker images
```

모든 모 비 이미지를 나열 하려면 모 비 엔진에 대 한 포인터를 사용 하 여 동일한 명령을 수정 합니다. 

```powershell
docker -H npipe:////./pipe/iotedge_moby_engine images
```

URI 엔진 설치 스크립트의 출력에 나열 된 또는 config.yaml 파일에 대 한 컨테이너 런타임 설정 섹션에서 찾을 수 있습니다. 

![config.yaml moby_runtime uri](./media/how-to-install-iot-edge-windows/moby-runtime-uri.png)

컨테이너 및 장치에서 실행 되는 이미지와 상호 작용 하 여 명령에 대 한 자세한 내용은 참조 [Docker 명령줄 인터페이스](https://docs.docker.com/engine/reference/commandline/docker/)합니다.

## <a name="update-an-existing-installation"></a>기존 설치 업데이트

이미 전에 장치에 IoT Edge 런타임을 설치 하 고 프로 비전 된 IoT Hub의 id를 사용 하 여 한, 장치 정보를 다시 입력 하지 않고도 런타임을 업데이트할 수 있습니다. 

자세한 내용은 [IoT Edge 보안 디먼 및 런타임 업데이트](how-to-update-iot-edge.md)를 참조하세요.

다음 예제는 기존 구성 파일을 가리키고 Windows 컨테이너를 사용하는 설치를 보여 줍니다. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Update-IoTEdge
```

IoT Edge를 업데이트 하는 경우 추가 매개 변수 업데이트를 수정 하 여 포함 합니다.
* 프록시 서버를 통해 트래픽을 또는
* 설치 관리자에서 오프라인 디렉터리를 확인하도록 설정 
* 필요한 경우 프롬프트 없이 다시 시작

이전 설치의 구성 파일에서 해당 정보는 이미 설정 때문에 스크립트 매개 변수를 사용 하 여 IoT Edge 에이전트 컨테이너 이미지를 선언할 수 없습니다. 에이전트 컨테이너 이미지를 수정하려는 경우 config.yaml 파일을 사용합니다. 

이 대 한 자세한 내용은 업데이트 옵션, 명령을 사용 하 여 `Get-Help Update-IoTEdge -full` 참조할 [모든 설치 매개 변수](#all-installation-parameters)합니다.

## <a name="uninstall-iot-edge"></a>IoT Edge 제거

Windows 디바이스에서 IoT Edge 설치를 제거하려는 경우 관리 PowerShell 창에서 다음 명령을 사용합니다. 이 명령은 기존 구성 및 Moby 엔진 데이터와 함께 IoT Edge 런타임을 제거합니다. 

```powershell
Uninstall-IoTEdge
```

제거-IoTEdge 명령을 Windows IoT Core에서 작동 하지 않습니다. Windows IoT Core 장치에서 IoT Edge를 제거 하려면 Windows IoT Core 이미지를 다시 배포 해야 합니다. 

제거 옵션에 대 한 자세한 내용은 명령을 사용 하 여 `Get-Help Uninstall-IoTEdge -full`입니다. 

## <a name="all-installation-parameters"></a>모든 설치 매개 변수

이전 섹션에서는 매개 변수를 사용하여 설치 스크립트를 수정하는 방법의 예제와 함께 일반적인 설치 시나리오를 소개했습니다. 이 섹션에서는 설치, 업데이트 또는 IoT Edge를 제거 하는 데 일반 매개 변수 참조 테이블을 제공 합니다. 

### <a name="deploy-iotedge"></a>Deploy-IoTEdge

배포 IoTEdge 명령을 다운로드 하 고 IoT Edge 보안 디먼 및 해당 종속성을 배포 합니다. 배포 명령은 특히 이러한 일반 매개 변수를 허용합니다. 전체 목록에 대 한 명령을 사용 하 여 `Get-Help Deploy-IoTEdge -full`입니다.  

| 매개 변수 | 허용되는 값 | 설명 |
| --------- | --------------- | -------- |
| **ContainerOs** | **Windows** 또는 **Linux** | 컨테이너 운영 체제가 없는 지정 된 경우 Windows 기본 값입니다.<br><br>Windows 컨테이너에 대 한 IoT Edge 설치에 포함 된 모 비 컨테이너 엔진을 사용 합니다. Linux 컨테이너의 경우 설치를 시작하기 전에 컨테이너 엔진을 설치해야 합니다. |
| **프록시** | 프록시 URL | 디바이스가 프록시 서버를 통해 인터넷에 연결해야 하는 경우 이 매개 변수를 포함합니다. 자세한 내용은 [프록시 서버를 통해 통신하도록 IoT Edge 디바이스 구성](how-to-configure-proxy-support.md)을 참조하세요. |
| **OfflineInstallationPath** | 디렉터리 경로 | 이 매개 변수를 포함 하는 경우 설치 관리자는 IoT Edge cab 및 VC 런타임 MSI 파일 설치에 필요한 나열 된 디렉터리를 확인 합니다. 디렉터리에서 찾을 수 없는 모든 파일이 다운로드 됩니다. 두 파일 디렉터리에 있는 경우 인터넷 연결 없이 IoT Edge를 설치할 수 있습니다. 또한 특정 버전을 사용 하려면이 매개 변수를 사용할 수 있습니다. |
| **InvokeWebRequestParameters** | 매개 변수 및 값의 해시 테이블입니다. | 설치 중에 여러 개의 웹 요청이 생성됩니다. 이 필드를 사용하여 해당 웹 요청에 대한 매개 변수를 설정합니다. 이 매개 변수는 프록시 서버에 대한 자격 증명을 구성하는 데 유용합니다. 자세한 내용은 [프록시 서버를 통해 통신하도록 IoT Edge 디바이스 구성](how-to-configure-proxy-support.md)을 참조하세요. |
| **RestartIfNeeded** | 없음 | 필요한 경우이 플래그를 확인 하지 않고 컴퓨터를 다시 시작 하려면 배포 스크립트를 수 있습니다. |

### <a name="initialize-iotedge"></a>Initialize-IoTEdge

초기화 IoTEdge 명령에 장치 연결 문자열 및 세부 정보를 사용 하 여 IoT Edge를 구성합니다. 이 명령에 의해 생성 된 정보의 상당 그러면 iotedge\config.yaml 파일에 저장 됩니다. 초기화 명령이 특히 이러한 일반 매개 변수를 허용합니다. 전체 목록에 대 한 명령을 사용 하 여 `Get-Help Initialize-IoTEdge -full`입니다. 

| 매개 변수 | 허용되는 값 | 설명 |
| --------- | --------------- | -------- |
| **수동** | 없음 | **매개 변수를 전환**합니다. 프로 비전 형식이 지정 되지 않은, 경우 기본값을 사용 하는 수동입니다.<br><br>디바이스 연결 문자열을 제공하여 디바이스를 수동으로 프로비전할 것을 선언합니다. |
| **Dps** | 없음 | **매개 변수를 전환**합니다. 프로 비전 형식이 지정 되지 않은, 경우 기본값을 사용 하는 수동입니다.<br><br>DPS(Device Provisioning Service) 범위 ID 및 디바이스의 등록 ID를 제공하여 DPS를 통해 프로비전할 것을 선언합니다.  |
| **DeviceConnectionString** | IoT Hub에 등록된 IoT Edge 디바이스의 연결 문자열을 작은따옴표 안에 표시합니다. | 수동 설치에 **필요**합니다. 스크립트 매개 변수에 연결 문자열을 제공하지 않을 경우 설치 중에 확인 메시지가 표시됩니다. |
| **ScopeId** | IoT Hub와 연결된 Device Provisioning Service 인스턴스의 범위 ID입니다. | DPS 설치에 **필요**합니다. 스크립트 매개 변수에 범위 ID를 제공하지 않을 경우 설치 중에 확인 메시지가 표시됩니다. |
| **RegistrationId** | 디바이스에서 생성된 등록 ID입니다. | DPS 설치에 **필요**합니다. 스크립트 매개 변수에 등록 ID를 제공하지 않을 경우 설치 중에 확인 메시지가 표시됩니다. |
| **ContainerOs** | **Windows** 또는 **Linux** | 컨테이너 운영 체제가 없는 지정 된 경우 Windows 기본 값입니다.<br><br>Windows 컨테이너에 대 한 IoT Edge 설치에 포함 된 모 비 컨테이너 엔진을 사용 합니다. Linux 컨테이너의 경우 설치를 시작하기 전에 컨테이너 엔진을 설치해야 합니다. |
| **InvokeWebRequestParameters** | 매개 변수 및 값의 해시 테이블입니다. | 설치 중에 여러 개의 웹 요청이 생성됩니다. 이 필드를 사용하여 해당 웹 요청에 대한 매개 변수를 설정합니다. 이 매개 변수는 프록시 서버에 대한 자격 증명을 구성하는 데 유용합니다. 자세한 내용은 [프록시 서버를 통해 통신하도록 IoT Edge 디바이스 구성](how-to-configure-proxy-support.md)을 참조하세요. |
| **AgentImage** | IoT Edge 에이전트 이미지 URI | 기본적으로 새 IoT Edge 설치는 IoT Edge 에이전트 이미지에 대한 최신 롤링 태그를 사용합니다. 이 매개 변수를 사용하여 이미지 버전에 대한 특정 태그를 설정하거나 사용자 고유의 에이전트 이미지를 제공합니다. 자세한 내용은 [IoT Edge 태그 이해](how-to-update-iot-edge.md#understand-iot-edge-tags)를 참조하세요. |
| **사용자 이름** | 컨테이너 레지스트리 사용자 이름입니다. | -AgentImage 매개 변수를 프라이빗 레지스트리의 컨테이너로 설정한 경우에만 이 매개 변수를 사용합니다. 레지스트리에 대한 액세스 권한이 있는 사용자 이름을 제공합니다. |
| **암호** | 보안 암호 문자열입니다. | -AgentImage 매개 변수를 프라이빗 레지스트리의 컨테이너로 설정한 경우에만 이 매개 변수를 사용합니다. 레지스트리에 액세스하기 위한 암호를 제공합니다. | 

### <a name="update-iotedge"></a>Update-IoTEdge

| 매개 변수 | 허용되는 값 | 설명 |
| --------- | --------------- | -------- |
| **ContainerOs** | **Windows** 또는 **Linux** | 지정 된 OS 컨테이너가 없는, 경우 Windows 기본 값입니다. Windows 컨테이너의 경우 컨테이너 엔진이 설치에 포함됩니다. Linux 컨테이너의 경우 설치를 시작하기 전에 컨테이너 엔진을 설치해야 합니다. |
| **프록시** | 프록시 URL | 디바이스가 프록시 서버를 통해 인터넷에 연결해야 하는 경우 이 매개 변수를 포함합니다. 자세한 내용은 [프록시 서버를 통해 통신하도록 IoT Edge 디바이스 구성](how-to-configure-proxy-support.md)을 참조하세요. |
| **InvokeWebRequestParameters** | 매개 변수 및 값의 해시 테이블입니다. | 설치 중에 여러 개의 웹 요청이 생성됩니다. 이 필드를 사용하여 해당 웹 요청에 대한 매개 변수를 설정합니다. 이 매개 변수는 프록시 서버에 대한 자격 증명을 구성하는 데 유용합니다. 자세한 내용은 [프록시 서버를 통해 통신하도록 IoT Edge 디바이스 구성](how-to-configure-proxy-support.md)을 참조하세요. |
| **OfflineInstallationPath** | 디렉터리 경로 | 이 매개 변수를 포함 하는 경우 설치 관리자는 IoT Edge cab 및 VC 런타임 MSI 파일 설치에 필요한 나열 된 디렉터리를 확인 합니다. 디렉터리에서 찾을 수 없는 모든 파일이 다운로드 됩니다. 두 파일 디렉터리에 있는 경우 인터넷 연결 없이 IoT Edge를 설치할 수 있습니다. 또한 특정 버전을 사용 하려면이 매개 변수를 사용할 수 있습니다. |
| **RestartIfNeeded** | 없음 | 필요한 경우이 플래그를 확인 하지 않고 컴퓨터를 다시 시작 하려면 배포 스크립트를 수 있습니다. |


### <a name="uninstall-iotedge"></a>Uninstall-IoTEdge

| 매개 변수 | 허용되는 값 | 설명 |
| --------- | --------------- | -------- |
| **Force** | 없음 | 이 플래그는 경우 제거 하려는 이전 시도가 실패 했습니다. 제거가 되도록 합니다. 
| **RestartIfNeeded** | 없음 | 필요한 경우이 플래그를 확인 하지 않고 컴퓨터를 다시 시작 하 고 제거 스크립트를 수 있습니다. |


## <a name="next-steps"></a>다음 단계

런타임을 설치하여 IoT Edge 디바이스를 프로비전했으므로 [IoT Edge 모듈을 배포](how-to-deploy-modules-portal.md)할 수 있습니다.

IoT Edge를 제대로 설치하는 데 문제가 있는 경우 [문제 해결](troubleshoot.md) 페이지를 확인합니다.

기존 설치를 최신 버전의 IoT Edge로 업데이트하려면 [IoT Edge 보안 디먼 및 런타임 업데이트](how-to-update-iot-edge.md)를 참조하세요.
