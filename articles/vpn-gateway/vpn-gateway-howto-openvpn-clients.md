---
title: Azure VPN Gateway에 대해 OpenVPN 클라이언트를 구성하는 방법 | Microsoft Docs
description: Azure VPN Gateway에 대해 OpenVPN 클라이언트를 구성하는 단계
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 5/21/2019
ms.author: cherylmc
ms.openlocfilehash: fdfabf328ddfa6b5e4b578be5a1b329cb3219a18
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65989096"
---
# <a name="configure-openvpn-clients-for-azure-vpn-gateway"></a>Azure VPN Gateway에 대 한 OpenVPN 클라이언트 구성

이 문서를 통해 구성한 **OpenVPN® 프로토콜** 클라이언트입니다.

## <a name="before-you-begin"></a>시작하기 전에

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

VPN 게이트웨이에 대해 OpenVPN을 구성하는 단계를 완료했는지 확인합니다. 자세한 내용은 [Azure VPN Gateway에 대해 OpenVPN 구성](vpn-gateway-howto-openvpn.md)을 참조하세요.

## <a name="windows"></a>Windows 클라이언트

1. 공식 [OpenVPN 웹 사이트](https://openvpn.net/index.php/open-source/downloads.html)에서 OpenVPN 클라이언트를 다운로드하여 설치합니다.
2. 게이트웨이에 대한 VPN 프로필을 다운로드합니다. Azure Portal의 지점 및 사이트 간 구성 탭 또는 PowerShell의 ‘New-AzVpnClientConfiguration’에서 이 작업을 수행할 수 있습니다.
3. 프로필의 압축을 풉니다. 다음으로 메모장을 사용하여 OpenVPN 폴더의 *vpnconfig.ovpn* 구성 파일을 엽니다.
4. 만들고 게이트웨이의 P2S 구성에 업로드한 P2S 클라이언트 인증서를 [내보냅니다](vpn-gateway-certificates-point-to-site.md#clientexport).
5. *.pfx*에서 개인 키 및 base64 지문을 추출합니다. 여러 가지 방법으로 이 작업을 수행할 수 있습니다. 머신에서 OpenSSL을 사용하는 것이 하나의 방법입니다. *profileinfo.txt* 파일은 CA 및 클라이언트 인증서에 대한 개인 키 및 지문을 포함합니다. 클라이언트 인증서의 지문을 사용해야 합니다.

   ```
   openssl pkcs12 -in "filename.pfx" -nodes -out "profileinfo.txt"
   ```
6. 메모장에서 *profileinfo.txt*를 엽니다. 클라이언트(자식) 인증서의 지문을 가져오려면 자식 인증서에 대한 텍스트(포함 및 사이) "-----BEGIN CERTIFICATE-----" 및 "-----END CERTIFICATE-----"를 선택하고 복사합니다. 제목=/ 줄을 확인하여 자식 인증서를 식별할 수 있습니다.
7. 3단계의 메모장에서 연 *vpnconfig.ovpn* 파일로 전환합니다. 아래 표시된 섹션을 찾고 "cert" 및 "/cert" 사이의 모든 항목을 바꿉니다.

   ```
   # P2S client certificate
   # please fill this field with a PEM formatted cert
   <cert>
   $CLIENTCERTIFICATE
   </cert>
   ```
8. 메모장에서 *profileinfo.txt*를 엽니다. 개인 키를 가져오려면 텍스트를 선택 합니다. (포함 및 사이) "---BEGIN PRIVATE KEY---" 및 "---END PRIVATE KEY---" 복사 합니다.
9. 메모장의 vpnconfig.ovpn 파일로 돌아가서 이 섹션을 찾습니다. "key" 및 "/key" 사이의 모든 항목을 바꾸는 프라이빗 키를 붙여넣습니다.

   ```
   # P2S client root certificate private key
   # please fill this field with a PEM formatted key
   <key>
   $PRIVATEKEY
   </key>
   ```
10. 다른 필드는 변경하지 마세요. 클라이언트 입력에 채워진 구성을 사용하여 VPN에 연결합니다.
11. vpnconfig.ovpn 파일을 C:\Program Files\OpenVPN\config 폴더에 복사합니다.
12. 시스템 트레이에서 OpenVPN 아이콘을 마우스 오른쪽 단추로 클릭하고 연결을 클릭합니다.

## <a name="mac"></a>Mac 클라이언트

1. [TunnelBlik](https://tunnelblick.net/downloads.html)와 같은 OpenVPN 클라이언트를 다운로드하여 설치합니다. 
2. 게이트웨이에 대한 VPN 프로필을 다운로드합니다. Azure Portal의 지점 및 사이트 간 구성 탭 또는 PowerShell의 ‘New-AzVpnClientConfiguration’에서 이 작업을 수행할 수 있습니다.
3. 프로필의 압축을 풉니다. 메모장에서 OpenVPN 폴더의 vpnconfig.ovpn 구성 파일을 엽니다.
4. base64에서 P2S 클라이언트 인증서 공개 키를 사용하여 P2S 클라이언트 인증서 섹션을 채웁니다. PEM 형식의 인증서에서 .cer 파일을 열고 인증서 헤더 사이에 base64 키를 복사할 수 있습니다. 인증서를 내보내 인코드된 공용 키를 가져오는 방법에 대한 자세한 내용은 [공용 키 내보내기](vpn-gateway-certificates-point-to-site.md#cer)를 참조하세요.
5. base64에서 P2S 클라이언트 인증서 프라이빗 키를 사용하여 프라이빗 키 섹션을 채웁니다. 프라이빗 키를 추출하는 방법에 대한 자세한 내용은 [프라이빗 키 내보내기](https://openvpn.net/community-resources/how-to/#pki)를 참조하세요.
6. 다른 필드는 변경하지 마세요. 클라이언트 입력에 채워진 구성을 사용하여 VPN에 연결합니다.
7. 프로필 파일을 두 번 클릭하여 tunnelblik에서 프로필을 만듭니다.
8. 애플리케이션 폴더에서 Tunnelblik를 시작합니다.
9. 시스템 트레이에서 Tunnelblik 아이콘을 클릭하고 연결을 선택합니다.

> [!IMPORTANT]
>OpenVPN 프로토콜에서는 IOS 11.0 이상 및 MacOS 10.13 이상만 지원됩니다.
>

## <a name="linux"></a>Linux 클라이언트

1. 새 터미널 세션을 엽니다. ‘Ctrl+Alt+t’를 동시에 누르면 새 세션을 열 수 있습니다.
2. 다음 명령을 입력하여 필요한 구성 요소를 설치합니다.

   ```
   sudo apt-get install openvpn
   sudo apt-get -y install network-manager-openvpn
   sudo service network-manager restart
   ```
3. 게이트웨이에 대한 VPN 프로필을 다운로드합니다. Azure Portal의 지점 및 사이트 간 구성 탭에서 이 작업을 수행할 수 있습니다.
4. 만들고 게이트웨이의 P2S 구성에 업로드한 P2S 클라이언트 인증서를 [내보냅니다](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-certificates-point-to-site#clientexport). 
5. .pfx에서 프라이빗 키 및 base64 지문을 추출합니다. 여러 가지 방법으로 이 작업을 수행할 수 있습니다. 컴퓨터에서 OpenSSL을 사용하는 것이 하나의 방법입니다.

    ```
    openssl.exe pkcs12 -in "filename.pfx" -nodes -out "profileinfo.txt"
    ```
   *profileinfo.txt* 파일은 CA 및 클라이언트 인증서에 대한 개인 키 및 지문을 포함합니다. 클라이언트 인증서의 지문을 사용해야 합니다.

6. 텍스트 편집기에서 *profileinfo.txt*를 엽니다. 클라이언트(자식) 인증서의 지문을 가져오려면 자식 인증서에 대한 "-----BEGIN CERTIFICATE-----" 및 "-----END CERTIFICATE-----"를 포함하는 해당 항목 사이의 텍스트를 선택하고 복사합니다. 제목=/ 줄을 확인하여 자식 인증서를 식별할 수 있습니다.

7. *vpnconfig.ovpn* 파일을 열고 아래에 표시된 섹션을 찾습니다. "cert" 및 "/cert" 사이의 모든 항목을 바꿉니다.

   ```
   # P2S client certificate
   # please fill this field with a PEM formatted cert
   <cert>
   $CLIENTCERTIFICATE
   </cert>
   ```
8. 텍스트 편집기에서 profileinfo.txt를 엽니다. 개인 키를 가져오려면 "---BEGIN PRIVATE KEY---" 간 및 포함 하 여 텍스트를 선택 하 고 "---END PRIVATE KEY---" 복사 합니다.

9. 텍스트 편집기에서 vpnconfig.ovpn 파일을 열고 이 섹션을 찾습니다. "key" 및 "/key" 사이의 모든 항목을 바꾸는 프라이빗 키를 붙여넣습니다.

   ```
   # P2S client root certificate private key
   # please fill this field with a PEM formatted key
   <key>
   $PRIVATEKEY
   </key>
   ```

10. 다른 필드는 변경하지 마세요. 클라이언트 입력에 채워진 구성을 사용하여 VPN에 연결합니다.
11. 명령줄을 사용하여 연결하려면 다음 명령을 입력합니다.
  
    ```
    sudo openvpn –-config <name and path of your VPN profile file>
    ```
12. GUI를 사용하여 연결하려면 시스템 설정으로 이동합니다.
13. **+** 를 클릭하여 새 VPN 연결을 추가합니다.
14. **VPN 추가**에서 **파일에서 가져오기...** 를 선택합니다.
15. 프로필 파일을 찾아보고 **열기**를 두 번 클릭하거나 선택합니다.
16. **VPN 추가** 창에서 **추가**를 클릭합니다.
  
    ![파일에서 가져오기](./media/vpn-gateway-howto-openvpn-clients/importfromfile.png)
17. **네트워크 설정** 페이지 또는 시스템 트레이의 네트워크 아이콘 아래에서 VPN을 **설정**하면 연결할 수 있습니다.

## <a name="next-steps"></a>다음 단계

다른 VNet의 리소스에 액세스 하려면 VPN 클라이언트를 하려는 경우 다음의 지침에 따라 합니다 [VNet 대 VNet](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) vnet 대 vnet 연결을 설정 하는 문서입니다. 게이트웨이 및 연결에서 BGP를 사용하도록 설정해야 합니다. 그렇지 않으면 트래픽이 흐르지 않습니다.

**"OpenVPN"는 OpenVPN i n c.의 상표**
