---
title: 네트워크 프록시용 디바이스 구성 - Azure IoT Edge | Microsoft Docs
description: 프록시 서버를 통해 통신하도록 Azure IoT Edge 런타임과 인터넷 연결 IoT Edge 모듈을 구성하는 방법을 설명합니다.
author: kgremban
manager: ''
ms.author: kgremban
ms.date: 06/05/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 1c0da1a768b894f543b9089643622c31d6a8758d
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66730156"
---
# <a name="configure-an-iot-edge-device-to-communicate-through-a-proxy-server"></a>프록시 서버를 통해 통신하도록 IoT Edge 디바이스 구성

IoT Edge 디바이스는 HTTPS 요청을 전송하여 IoT Hub와 통신합니다. 디바이스가 프록시 서버를 사용하는 네트워크에 연결되어 있는 경우에는 해당 서버를 통해 통신하도록 IoT Edge 런타임을 구성해야 합니다. 또한 프록시 서버는 IoT Edge 허브를 통해 라우팅되지 않는 HTTP 또는 HTTPS 요청을 수행하는 개별 IoT Edge 모듈에도 영향을 줄 수 있습니다. 

이 문서는 구성 하 고 프록시 서버 뒤에 IoT Edge 장치를 관리 하려면 다음 네 단계를 안내 합니다. 

1. **장치에서 IoT Edge 런타임을 설치 합니다.**

   IoT Edge 설치 스크립트를 사용 하는 장치에서 해당 요청에 프록시 서버를 통해 통신 해야 하므로 인터넷에서 패키지 파일과 끌어옵니다. 자세한 단계에 대 한 참조를 [프록시를 통해 런타임을 설치](#install-the-runtime-through-a-proxy) 이 문서의 섹션입니다. Windows 장치에 대 한 설치 스크립트를 제공 된 [오프 라인 설치](how-to-install-iot-edge-windows.md#offline-installation) 옵션입니다. 

   이 단계는 처음 설정한 때 IoT Edge 장치에서 수행 하는 일회성 프로세스입니다. IoT Edge 런타임을 업데이트 하는 경우에 동일한 연결 필요도 않습니다. 

2. **장치에서 Docker 디먼 및 IoT Edge 디먼을 구성 합니다.**

   IoT Edge 장치에서 프록시 서버를 통해 웹 요청을 해야 하는 둘 다 두 데몬이 사용 합니다. IoT Edge 디먼은 IoT Hub와의 통신을 담당합니다. 모 비 디먼 컨테이너 관리에 대 한 책임이, 따라서 컨테이너 레지스트리를 사용 하 여 통신 합니다. 자세한 단계에 대 한 참조를 [디먼 구성](#configure-the-daemons) 이 문서의 섹션입니다. 

   이 단계는 처음 설정한 때 IoT Edge 장치에서 수행 하는 일회성 프로세스입니다.

3. **장치의 config.yaml 파일에는 IoT Edge 에이전트 속성을 구성 합니다.**

   IoT Edge 디먼 edge 에이전트 모듈을 처음에 시작 되지만 edge 에이전트 모듈은 IoT Hub에서 배포 매니페스트를 검색 하 고 다른 모든 모듈을 시작 하는 일을 담당 합니다. IoT Hub에 대 한 초기 연결을 확인 하려면 IoT Edge 에이전트에 대 한 edge 에이전트 모듈 환경 변수를 장치 자체에서 수동으로 구성 합니다. 초기 연결 후 edge 에이전트 모듈을 원격으로 구성할 수 있습니다. 자세한 단계에 대 한 참조를 [IoT Edge 에이전트 구성](#configure-the-iot-edge-agent) 이 문서의 섹션입니다.

   이 단계는 처음 설정한 때 IoT Edge 장치에서 수행 하는 일회성 프로세스입니다.

4. **모든 향후 모듈 배포에 대 한 프록시를 통해 통신 하는 모든 모듈에 대 한 환경 변수를 설정 합니다.**

   IoT Edge 장치를 설정 및 프록시 서버를 통해 IoT Hub에 연결 되 면 모든 향후 모듈 배포에 연결을 유지 해야 합니다. 자세한 단계에 대 한 참조를 [배포 매니페스트를 구성](#configure-deployment-manifests) 이 문서의 섹션입니다. 

   이 단계는 모든 새 모듈 또는 배포 업데이트를 유지 관리 장치의 프록시 서버를 통해 통신할 수 있도록 원격으로 수행 하는 지속적인 프로세스입니다. 

## <a name="know-your-proxy-url"></a>프록시 URL 파악

이 문서의 단계를 시작 하기 전에 프록시 URL 알 필요가 있습니다.

프록시 URL은 다음 형식을 사용합니다. **프로토콜**://**proxy_host**:**proxy_port**

* **프로토콜**은 HTTP 또는 HTTPS입니다. Docker 디먼은 컨테이너 레지스트리 설정에 따라 프로토콜 중 하나를 사용할 수 있지만, IoT Edge 디먼 및 런타임 컨테이너는 항상 HTTPS를 사용해야 합니다.

* **proxy_host**는 프록시 서버의 주소입니다. 프록시 호스트의 일부로 다음 형식으로 자격 증명을 제공할 수 있습니다 프록시 서버에 인증이 필요한 경우: **사용자**:**암호**\@**proxy_host** .

* **proxy_port**는 프록시가 네트워크 트래픽에 응답하는 네트워크 포트입니다.

## <a name="install-the-runtime-through-a-proxy"></a>프록시를 통해 런타임을 설치합니다

IoT Edge 장치를 Windows 또는 Linux에서 실행 하는지 여부를 프록시 서버를 통해 설치 패키지에 액세스 해야 합니다. 운영 체제에 따라 프록시 서버를 통해 IoT Edge 런타임을 설치 하는 단계를 수행 합니다. 

### <a name="linux"></a>Linux

Linux 디바이스에서 IoT Edge 런타임을 설치 중인 경우 설치 패키지에 액세스하기 위해 프록시 서버로 이동하도록 패키지 관리자를 구성합니다. 예를 들어 [http-proxy를 사용하도록 apt-get을 설정](https://help.ubuntu.com/community/AptGet/Howto/#Setting_up_apt-get_to_use_a_http-proxy)합니다. 패키지 관리자를 구성하면 일반적으로 [Linux에서 Azure IoT Edge 런타임 설치(ARM32v7/armhf)](how-to-install-iot-edge-linux-arm.md) 또는 [Linux에서 Azure IoT Edge 런타임 설치(x64)](how-to-install-iot-edge-linux.md)의 지침을 따릅니다.

### <a name="windows"></a>Windows

Windows 장치에서 IoT Edge 런타임을 설치 중인 경우 프록시 서버를 두 번 통과 해야 합니다. 첫 번째 연결 설치 관리자 스크립트 파일을 다운로드 하 고 두 번째 연결이 필요한 구성 요소를 다운로드 하려면 설치 하는 동안 키를 누릅니다. Windows 설정에서 프록시 정보를 구성 하거나 PowerShell 명령에 직접 프록시 정보를 포함할 수 있습니다. 

다음 단계를 사용 하 여 windows 설치의 예를 보여 줍니다.는 `-proxy` 인수:

1. Invoke-webrequest 명령을 설치 관리자 스크립트에 액세스 하려면 프록시 정보를 해야 합니다. 그런 다음 배포 IoTEdge 명령을 설치 파일을 다운로드 하려면 프록시 정보를 해야 합니다. 

   ```powershell
   . {Invoke-WebRequest -proxy <proxy URL> -useb aka.ms/iotedge-win} | Invoke-Expression; Deploy-IoTEdge -proxy <proxy URL>
   ```

2. 두 번째 단계 Invoke-webrequest에 대 한 프록시 정보를만 필요 하므로 프록시 서버를 통해 이동할 IoTEdge 초기화 명령이 필요 하지 않습니다.

   ```powershell
   . {Invoke-WebRequest -proxy <proxy URL> -useb aka.ms/iotedge-win} | Invoke-Expression; Initialize-IoTEdge

If you have complicated credentials for the proxy server that can't be included in the URL, use the `-ProxyCredential` parameter within `-InvokeWebRequestParameters`. For example,

```powershell
$proxyCredential = (Get-Credential).GetNetworkCredential()
. {Invoke-WebRequest -proxy <proxy URL> -ProxyCredential $proxyCredential -useb aka.ms/iotedge-win} | Invoke-Expression; `
Deploy-IoTEdge -InvokeWebRequestParameters @{ '-Proxy' = '<proxy URL>'; '-ProxyCredential' = $proxyCredential }
```

프록시 매개 변수에 대한 자세한 내용은 [Invoke-WebRequest](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-webrequest)를 참조하세요. 오프 라인 설치를 포함 하 여 Windows 설치 옵션에 대 한 자세한 내용은 참조 [Windows에 설치할 Azure IoT Edge 런타임을](how-to-install-iot-edge-windows.md)합니다.

## <a name="configure-the-daemons"></a>디먼 구성

IoT Edge는 IoT Edge 장치에서 실행 하는 두 데몬이 의존 합니다. 모 비 디먼 하면 컨테이너 레지스트리에서 컨테이너 이미지 웹 요청 합니다. IoT Edge 디먼은 웹 요청을 전송하여 IoT Hub와 통신합니다.

모 비 및 IoT Edge 디먼 지속적인 장치 기능에 대 한 프록시 서버를 사용 하도록 구성 해야 합니다. 이 단계는 IoT Edge 장치를 초기 장치 설정 중입니다. 

### <a name="moby-daemon"></a>모 비 데몬

모 비는 Docker를 기반으로 하므로 환경 변수를 사용 하 여 모 비 디먼을 구성 하려면 Docker 설명서를 참조 하십시오. DockerHub 및 Azure Container Registry를 비롯한 대부분의 컨테이너 레지스트리는 HTTPS 요청을 지원하므로 **HTTPS_PROXY** 매개 변수를 설정해야 합니다. TLS(전송 계층 보안)를 지원하지 않는 레지스트리에서 이미지를 끌어오는 경우에는 **HTTP_PROXY** 매개 변수를 설정해야 합니다. 

IoT Edge 장치 운영 체제에 적용 되는 문서를 선택 합니다. 

* [Linux에서 Docker 디먼 구성](https://docs.docker.com/config/daemon/systemd/#httphttps-proxy)
    * Linux 장치에서 모 비 디먼을 Docker 이름을 유지합니다.
* [Windows에서 Docker 디먼 구성](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon#proxy-configuration)
    * Windows 장치의 모 비 디먼 iotedge 모 비 라고 합니다. Windows 장치에서 Docker 데스크톱과 모 비를 병렬로 실행할 수 있기 때문에 이름은 서로 다릅니다. 

### <a name="iot-edge-daemon"></a>IoT Edge 디먼

IoT Edge 디먼 모 비 디먼에 비슷한 방식으로 구성 됩니다. 사용 중인 운영 체제에 따라 다음 단계를 수행하여 서비스에 대한 환경 변수를 설정합니다. 

IoT Edge 디먼 항상 요청을 보내도록 IoT Hub에 HTTPS를 사용 합니다.

#### <a name="linux"></a>Linux

터미널에서 편집기를 열어 IoT Edge 디먼을 구성합니다. 

```bash
sudo systemctl edit iotedge
```

아래 텍스트를 입력합니다. **\<proxy URL>** 은 실제 프록시 서버 주소와 포트로 바꾸세요. 그런 다음 입력 내용을 저장하고 편집기를 종료합니다. 

```ini
[Service]
Environment="https_proxy=<proxy URL>"
```

서비스 관리자를 새로 고쳐 IoT Edge의 새 구성을 선택합니다.

```bash
sudo systemctl daemon-reload
```

IoT Edge를 다시 시작하여 변경 내용을 적용합니다.

```bash
sudo systemctl restart iotedge
```

환경 변수가 생성되었으며 새 구성이 로드되었는지 확인합니다. 

```bash
systemctl show --property=Environment iotedge
```

#### <a name="windows"></a>Windows

관리자 권한으로 PowerShell 창을 연 후에 다음 명령을 실행해 새 환경 변수를 사용하여 레지스트리를 편집합니다. **\<proxy url>** 은 실제 프록시 서버 주소와 포트로 바꾸세요. 

```powershell
reg add HKLM\SYSTEM\CurrentControlSet\Services\iotedge /v Environment /t REG_MULTI_SZ /d https_proxy=<proxy URL>
```

IoT Edge를 다시 시작하여 변경 내용을 적용합니다.

```powershell
Restart-Service iotedge
```

## <a name="configure-the-iot-edge-agent"></a>IoT Edge 에이전트 구성

IoT Edge 에이전트는 모든 IoT Edge 디바이스에서 처음으로 시작되는 모듈로, IoT Edge config.yaml 파일의 정보에 따라 처음으로 시작됩니다. 그런 후에 이 에이전트에서 IoT Hub에 연결하여 배포 매니페스트를 검색합니다. 배포 매니페스트는 디바이스에서 배포해야 하는 다른 모듈을 선언합니다.

이 단계 초기 장치 설치 하는 동안 IoT Edge 장치에 한 번 수행 합니다. 

1. IoT Edge 디바이스에서 config.yaml 파일을 엽니다. Linux 시스템에서 이 파일의 위치는 **/etc/iotedge/config.yaml**입니다. Windows 시스템에서 이 파일의 위치는 **C:\ProgramData\iotedge\config.yaml**입니다. 구성 파일은 보호되어 있으므로 관리 권한이 있어야 액세스할 수 있습니다. Linux 시스템에서 사용 된 `sudo` 원하는 텍스트 편집기에서 파일을 열기 전에 명령을 합니다. Windows에서 관리자 권한으로 메모장 같은 텍스트 편집기를 열고 파일을 엽니다. 

2. config.yaml 파일에서 **Edge Agent module spec** 섹션을 찾습니다. IoT Edge 에이전트 정의에는 **env** 매개 변수가 포함되어 있으며, 이 매개 변수에 환경 변수를 추가할 수 있습니다. 

3. env 매개 변수의 자리 표시자인 중괄호를 제거하고 새 줄에 새 변수를 추가합니다. YAML에서 들여쓰기를 하려면 공백 두 개를 추가해야 합니다. 

   ```yaml
   https_proxy: "<proxy URL>"
   ```

4. IoT Edge 런타임은 기본적으로 AMQP를 사용하여 IoT Hub와 통신합니다. 그런데 일부 프록시 서버는 AMQP 포트를 차단합니다. 이 경우에는 WebSocket을 통해 AMQP를 사용하도록 edgeAgent도 구성해야 합니다. 이렇게 하려면 두 번째 환경 변수를 추가합니다.

   ```yaml
   UpstreamProtocol: "AmqpWs"
   ```

   ![환경 변수가 포함된 edgeAgent 정의](./media/how-to-configure-proxy-support/edgeagent-edited.png)

5. config.yaml의 변경 내용을 저장하고 편집기를 닫습니다. IoT Edge를 다시 시작하여 변경 내용을 적용합니다. 

   * Linux: 

      ```bash
      sudo systemctl restart iotedge
      ```

   * Windows:

      ```powershell
      Restart-Service iotedge
      ```

## <a name="configure-deployment-manifests"></a>배포 매니페스트 구성  

IoT Edge 디바이스가 프록시 서버를 사용하도록 구성한 후에는 이후의 배포 매니페스트에서도 환경 변수를 계속 선언해야 합니다. Azure 포털 마법사를 사용 하 여 두 배포 매니페스트를 편집 하거나 배포를 편집 하 여 매니페스트 JSON 파일 수 있습니다. 

두 개의 런타임 모듈인 edgeAgent와 edgeHub가 프록시 서버를 통해 통신하도록 항상 구성하여 IoT Hub와의 연결을 유지할 수 있게 합니다. Edge 에이전트 모듈에서 프록시 정보를 제거 하면 이전 섹션에 설명 된 대로 연결을 다시 설정 하는 유일한 방법은 장치의 config.yaml 파일을 편집 하 여 됩니다. 

인터넷에 연결 하는 다른 IoT Edge 모듈은 너무 프록시 서버를 통해 통신 하도록 구성 되어야 합니다. 그러나 edgeHub를 통해 메시지를 라우팅하거나 디바이스의 다른 모듈하고만 통신하는 모듈에는 프록시 서버 세부 정보가 필요하지 않습니다. 

이 단계는 IoT Edge 장치의 수명 동안 진행 중입니다. 

### <a name="azure-portal"></a>Azure portal

**모듈 설정** 마법사를 사용하여 IoT Edge 디바이스의 배포를 만들 때는 모든 모듈에 **환경 변수** 섹션에 있으므로 이 섹션을 통해 프록시 서버 연결을 구성할 수 있습니다. 

IoT Edge 에이전트 및 IoT Edge 허브 모듈을 구성하려면 마법사의 첫 단계에서 **고급 Edge 런타임 설정 구성**을 선택합니다. 

![고급 Edge 런타임 설정 구성](./media/how-to-configure-proxy-support/configure-runtime.png)

IoT Edge 에이전트 및 IoT Edge 허브 모듈 정의에 모두 **https_proxy** 환경 변수를 추가합니다. IoT Edge 디바이스의 config.yaml 파일에 **UpstreamProtocol** 환경 변수를 포함한 경우에는 IoT Edge 에이전트 모듈 정의에도 해당 환경 변수를 포함합니다. 

![Https_proxy 환경 변수 설정](./media/how-to-configure-proxy-support/edgehub-environmentvar.png)

배포 매니페스트에 추가하는 기타 모든 모듈은 같은 패턴을 따릅니다. 모듈 이름 및 이미지를 설정하는 페이지에는 환경 변수 섹션이 있습니다.

### <a name="json-deployment-manifest-files"></a>JSON 배포 매니페스트 파일

Visual Studio Code에서 템플릿을 사용하거나 JSON 파일을 직접 만들어 IoT Edge 디바이스용 배포를 만든 경우에는 각 모듈 정의에 환경 변수를 직접 추가할 수 있습니다. 

다음 JSON 형식을 사용합니다. 

```json
"env": {
    "https_proxy": {
        "value": "<proxy URL>"
    }
}
```

환경 변수가 포함된 모듈 정의는 다음 edgeHub 예제와 같이 표시됩니다.

```json
"edgeHub": {
    "type": "docker",
    "settings": {
        "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
        "createOptions": ""
    },
    "env": {
        "https_proxy": {
            "value": "https://proxy.example.com:3128"
        }
    },
    "status": "running",
    "restartPolicy": "always"
}
```

IoT Edge 디바이스의 confige.yaml 파일에 **UpstreamProtocol** 환경 변수를 포함한 경우에는 IoT Edge 에이전트 모듈 정의에도 해당 환경 변수를 포함합니다. 

```json
"env": {
    "https_proxy": {
        "value": "<proxy URL>"
    },
    "UpstreamProtocol": {
        "value": "AmqpWs"
    }
}
```

## <a name="next-steps"></a>다음 단계

[IoT Edge 런타임](iot-edge-runtime.md)의 역할에 대해 자세히 알아봅니다.

[Azure IoT Edge와 관련된 일반적인 문제 및 해결 방법](troubleshoot.md)을 사용하여 설치 및 구성 오류가 발생하는 문제를 해결합니다.

