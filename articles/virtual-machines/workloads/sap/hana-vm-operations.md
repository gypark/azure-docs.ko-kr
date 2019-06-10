---
title: Azure에서 SAP HANA 인프라 구성 및 작업 | Microsoft Docs
description: Azure Virtual Machines에 배포된 SAP HANA 시스템에 대한 운영 가이드입니다.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/05/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 156bb4cbf43dc71627f9db785dba574f25139285
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66733836"
---
# <a name="sap-hana-infrastructure-configurations-and-operations-on-azure"></a>Azure에서 SAP HANA 인프라 구성 및 작업
이 문서에서는 Azure VM(Virtual Machines)에 배포된 SAP HANA 시스템 운영 및 Azure 인프라 구성을 위한 지침을 제공합니다. 또한 M128s VM SKU용 SAP HANA 스케일 아웃을 위한 구성 정보가 포함됩니다. 이 문서는 다음 내용을 포함하는 표준 SAP 설명서를 대체하기 위한 것이 아닙니다.

- [SAP administration guide](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/330e5550b09d4f0f8b6cceb14a64cd22.html)(SAP 관리 가이드)
- [SAP installation guides](https://service.sap.com/instguides)(SAP 설치 가이드)
- [SAP notes](https://sservice.sap.com/notes)(SAP 참고)

## <a name="prerequisites"></a>필수 조건
이 가이드를 사용하려면 다음 Azure 구성 요소에 대한 기본 지식이 필요합니다.

- [Azure 가상 머신](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Azure 네트워킹 및 가상 네트워크](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Azure Storage](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

Azure의 SAP NetWeaver 및 기타 SAP 구성 요소에 대해 자세히 알아보려면 [Azure 설명서](https://docs.microsoft.com/azure/)의 [Azure의 SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) 섹션을 참조하세요.

## <a name="basic-setup-considerations"></a>기본 설치 고려 사항
다음 섹션에서는 Azure VM에 SAP HANA 시스템을 배포하기 위한 기본 설치 고려 사항을 설명합니다.

### <a name="connect-into-azure-virtual-machines"></a>Azure Virtual Machines에 연결
[Azure Virtual Machines 계획 가이드](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide)에 설명된 대로 Azure VM에 연결하기 위한 두 가지 기본 방법이 있습니다.

- SAP HANA를 실행하는 VM 또는 Jump VM의 공개 엔드포인트와 인터넷을 통해 연결
- [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) 또는 Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/)를 통해 연결

VPN 또는 ExpressRoute를 통한 사이트 간 연결은 프로덕션 시나리오에 필요합니다. 이 유형의 연결은 SAP 소프트웨어가 사용되는 프로덕션 시나리오로 제공되는 비프로덕션 시나리오에도 필요합니다. 다음 그림은 사이트 간 연결의 예를 보여 줍니다.

![사이트 간 연결](media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png)


### <a name="choose-azure-vm-types"></a>Azure VM 유형 선택
프로덕션 시나리오에 사용할 수 있는 Azure VM 유형은 [SAP documentation for IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html)(IAAS에 대한 SAP 설명서)에 나와 있습니다. 비프로덕션 시나리오의 경우 더 다양한 네이티브 Azure VM 유형을 사용할 수 있습니다.

>[!NOTE]
> 비프로덕션 시나리오의 경우 [SAP note #1928533](https://launchpad.support.sap.com/#/notes/1928533)(SAP 참고 #1928533)에 나열된 VM 유형을 사용합니다. 프로덕션 시나리오에 대한 Azure VM의 사용을 위해 SAP 게시된 [인증된 IaaS 플랫폼 목록](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)에서 SAP HANA 인증된 VM을 확인합니다.

다음을 사용하여 Azure에 VM을 배포합니다.

- Azure Portal
- Azure PowerShell cmdlet
- Azure CLI

[SAP 클라우드 플랫폼](https://cal.sap.com/)을 통해 Azure VM 서비스에 전체 설치된 SAP HANA 플랫폼을 배포할 수도 있습니다. 설치 프로세스는 [Azure에서 SAP S/4HANA 또는 BW/4HANA 배포](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h) 또는 [여기](https://github.com/AzureCAT-GSI/SAP-HANA-ARM)에서 릴리스된 자동화로 설명되어 있습니다.

### <a name="storage-configuration-for-sap-hana"></a>SAP HANA에 대 한 저장소 구성
Azure에서 SAP HANA와 함께 사용 될 저장소 형식과 저장소 구성 문서를 읽어 [SAP HANA Azure 가상 머신 저장소 구성](./hana-vm-operations-storage.md)


### <a name="set-up-azure-virtual-networks"></a>Azure 가상 네트워크 설정
VPN 또는 ExpressRoute를 통해 Azure로의 사이트 간 연결이 있다면 가상 게이트웨이를 통해 VPN 또는 ExpressRoute 회로에 연결된 Azure Virtual Network가 하나 이상 있어야 합니다. 간단한 배포에서는 가상 게이트웨이를 SAP HANA 인스턴스를 호스팅하는 Azure VNet(가상 네트워크)의 서브넷에 배포할 수 있습니다. SAP HANA를 설치하려면 Azure Virtual Network 내에 두 개의 서브넷을 추가로 만듭니다. 한 서브넷은 SAP HANA 인스턴스를 실행하는 VM을 호스트하고, 다른 서브넷은 SAP HANA Studio, 기타 관리 소프트웨어 또는 사용자의 애플리케이션 소프트웨어를 호스트하는 Jumpbox 또는 관리 VM을 실행합니다.

> [!IMPORTANT]
> 기능보다는 더 중요한 성능상의 이유로 인해, SAP NetWeaver, Hybris 또는 S/4HANA 기반 SAP 시스템의 DBMS 레이어와 SAP 애플리케이션 간 통신 경로에 [Azure 네트워크 가상 어플라이언스](https://azure.microsoft.com/solutions/network-appliances/)를 구성하는 것이 지원되지 않습니다. SAP 애플리케이션 계층과 DBMS 계층 간의 통신은 직접 통신이어야 합니다. ASG 및 NSG 규칙이 직접 통신을 허용하는 한, 제한에 [Azure ASG 및 NSG 규칙](https://docs.microsoft.com/azure/virtual-network/security-overview)이 포함되지 않습니다. NVA가 지원되지 않는 또 다른 시나리오로는, [SAP 애플리케이션용 SUSE Linux Enterprise Server의 Azure VM에 있는 SAP NetWeaver에 대한 고가용성](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)에 설명된 Linux Pacemaker 클러스터 노드를 나타내는 Azure VM과 SBD 디바이스 간 통신 경로가 있습니다. [Azure에서 파일 공유를 사용하여 Windows 장애 조치(Failover) 클러스터에 SAP ASCS/SCS 인스턴스 클러스터링](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share)에 설명된 대로 설정된 Windows Server SOFS와 Azure VM 간 통신 경로도 있습니다. 통신 경로에 NVA가 있으면 두 통신 파트너 간 네트워크 대기 시간을 쉽게 두 배로 늘릴 수 있고, SAP 애플리케이션 레이어와 DBMS 레이어 간 중요 경로의 처리량을 제한할 수 있습니다. 고객을 통해 관찰된 일부 시나리오에서 NVA로 인해 Linux Pacemaker 클러스터 노드 간 통신이 NVA를 통해 해당 SBD 디바이스와 통신해야 하는 경우에 Pacemaker Linux 클러스터가 실패할 수 있습니다.  
> 

> [!IMPORTANT]
> 지원되지 **않는** 다른 디자인은 SAP 애플리케이션 계층과 DBMS 계층을 서로 [피어링](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)되지 않은 다른 Azure 가상 네트워크로 분리하는 것입니다. 다른 Azure 가상 네트워크를 사용하는 대신, Azure 가상 네트워크 내의 서브넷을 사용하여 SAP 애플리케이션 계층과 DBMS 계층을 분리하는 것이 좋습니다. 권장 사항을 따르지 않고 두 계층을 다른 가상 네트워크로 분리하려는 경우에는 두 가상 네트워크가 [피어링](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)되어야 합니다. [피어링](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)된 두 Azure 가상 네트워크 간의 네트워크 트래픽에는 전송 비용이 부과됩니다. SAP 애플리케이션 계층과 DBMS 계층이 피어링된 두 Azure 가상 네트워크 간에 분리되어 있으면 SAP 애플리케이션 계층과 DBMS 계층 간에 교환되는 수 테라바이트의 거대한 데이터 볼륨으로 인해 상당한 비용이 누적될 수 있습니다. 

SAP HANA를 실행할 VM을 설치할 때 VM에는 다음이 필요합니다.

- 두 가상 NIC 설치: 하나의 NIC는 관리 서브넷에 연결되고 다른 NIC는 온-프레미스 네트워크나 다른 네트워크에서 Azure VM의 SAP HANA 인스턴스에 연결됩니다.
- 두 가상 NIC에 대해 배포된 고정 개인 IP 주소.

> [!NOTE]
> Azure 평균을 통해 고정 IP 주소를 개별 vNIC에 할당해야 합니다. 게스트 OS 내에서 고정 IP 주소를 vNIC에 할당하지 않아야 합니다. Azure Backup 서비스와 같은 일부 Azure 서비스는 최소한 기본 vNIC가 고정 IP 주소가 아닌 DHCP로 설정된다는 사실에 의존합니다. [Azure 가상 머신 백업 문제 해결](https://docs.microsoft.com/azure/backup/backup-azure-vms-troubleshoot#networking) 문서도 참조하세요. VM에 여러 고정 IP 주소를 할당해야 하는 경우 VM에 여러 vNIC를 할당해야 합니다.
>
>

그러나 지속되고 있는 배포의 경우 Azure에서 가상 데이터 센터 네트워크 아키텍처를 만들어야 합니다. 이 아키텍처는 온-프레미스에 연결되어 있는 Azure VNet 게이트웨이를 별도의 Azure VNet으로 분리할 것을 권장합니다. 이 별도의 VNet은 온-프레미스 또는 인터넷 중 하나에 유지되는 모든 트래픽을 호스트해야 합니다. 이 방법을 사용하면 이 별도의 허브 VNet에서 Azure의 가상 데이터 센터로 들어가는 감사 및 로깅 트래픽을 위한 소프트웨어를 배포할 수 있습니다. 따라서 Azure 배포로 들어오고 나가는 트래픽과 관련된 모든 소프트웨어 및 구성을 하나의 VNet이 호스트하도록 해야 합니다.

문서 [Azure Virtual Datacenter: 네트워크 측면](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter) 및 [Azure Virtual Datacenter 및 Enterprise Control Plane](https://docs.microsoft.com/azure/architecture/vdc/)에 가상 데이터 센터 방식 및 관련된 Azure VNet 설계에 대한 자세한 정보가 있습니다.


>[!NOTE]
>[Azure VNet 피어링](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)을 사용하여 허브 VNet과 스포크 VNet 간에 전달되는 트래픽은 추가적인 [비용](https://azure.microsoft.com/pricing/details/virtual-network/)의 문제입니다. 해당 가격을 기준으로 strict 허브와 스포크 네트워크 설계를 실행하는 것과 바이패스 VNet 피어링을 위하여 ‘스포크’에 연결하는 여러 [Azure ExpressRoute 게이트웨이](https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways)를 실행하는 것 사이에서 절충안을 내는 것을 고려해야 할 수도 있습니다. 단, Azure ExpressRoute 게이트웨이에서도 추가 [비용](https://azure.microsoft.com/pricing/details/vpn-gateway/)이 발생합니다. 또한 네트워크 트래픽 로깅, 감사 및 모니터링에 사용하는 타사 소프트웨어에 대한 추가 비용이 발생할 수 있습니다. VNet 피어링을 통한 데이터 교환에 대한 비용 및 추가 Azure ExpressRoute 게이트웨이와 추가 소프트웨어 라이선스로 발생하는 비용에 따라 VNet 대신 격리 단위로 서브넷을 사용하여 하나의 VNet 내에서 마이크로 구분을 결정할 수 있습니다.


IP 주소를 할당하는 다른 방법에 대한 개요는 [IP 주소 유형 및 Azure에서 할당 방법](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm)을 참조하세요. 

SAP HANA를 실행하는 VM의 경우 할당된 고정 IP 주소를 사용해야 합니다. HANA에 대한 일부 구성 특성은 IP 주소를 참조하기 때문입니다.

[Azure NSG(네트워크 보안 그룹)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)는 SAP HANA 인스턴스 또는 Jumpbox로 라우팅된 트래픽을 전달하는 데 사용됩니다. NSG 및 [애플리케이션 보안 그룹](https://docs.microsoft.com/azure/virtual-network/security-overview#application-security-groups)은 SAP HANA 서브넷 및 관리 서브넷에 연결됩니다.

다음 그림은 허브 및 스포크 VNet 아키텍처에 이어 SAP HANA의 대략적인 배포 스키마를 간단하게 보여 줍니다.

![SAP HANA의 대략적인 배포 스키마](media/hana-vm-operations/hana-simple-networking.PNG)

사이트 간 연결 없이 Azure에서 SAP HANA를 배포하기 위해 계속해서 공용 인터넷에서 SAP HANA 인스턴스를 보호하고 정방향 프록시 뒤로 숨기고자 합니다. 이러한 기본 시나리오의 배포에서는 Azure 기본 제공 DNS 서비스가 호스트 이름을 확인해야 합니다. 공용 IP 주소가 사용되는 더 복잡한 배포에서는 Azure 기본 제공 DNS 서비스가 특히 중요합니다. Azure NSG 및 [Azure NVA](https://azure.microsoft.com/solutions/network-appliances/)를 사용하여 인터넷에서 Azure의 Azure VNet 아키텍처로의 라우팅을 제어 및 모니터링합니다. 다음 그림은 사이트 간 연결 없이 허브 및 스포크 VNet 아키텍처에서 SAP HANA를 배포하는 경우의 대략적인 스키마를 보여 줍니다.
  
![사이트 간 연결 없는 SAP HANA의 대략적인 배포 스키마](media/hana-vm-operations/hana-simple-networking2.PNG)
 

Azure NVA를 사용하여 허브 및 스포크 VNet 아키텍처 없이 인터넷에서의 액세스를 제어 및 모니터링하는 방법에 관한 또 다른 설명은 [고가용성 네트워크 가상 어플라이언스 배포](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha) 문서에서 확인할 수 있습니다.


## <a name="configuring-azure-infrastructure-for-sap-hana-scale-out"></a>SAP HANA 스케일 아웃을 위한 Azure 인프라 구성
Microsoft에는 SAP HANA 스케일 아웃 구성에 대해 인증된 하나의 M 시리즈 VM SKU가 있습니다. VM 형식 M128s는 최대 16개 노드의 스케일 아웃에 대해 인증되었습니다. Azure VM에서 SAP HANA 스케일 아웃 인증의 변경 내용은 [인증된 IaaS 플랫폼 목록](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)을 확인하세요.

Azure VM에서 스케일 아웃 구성을 배포하기 위한 최소한의 OS 릴리스는 다음과 같습니다.

- SUSE Linux 12 SP3
- Red hat Linux 7.4

16개 노드 스케일 아웃 인증 중

- 하나의 노드는 마스터 노드
- 최대 15개 노드는 작업자 노드

>[!NOTE]
>Azure VM 스케일 아웃 배포에서 대기 노드를 사용할 가능성은 없습니다.
>

대기 노드를 구성하지 못하는 이유는 두 가지입니다.

- 이 시점에서 Azure에는 네이티브 NFS 서비스가 없습니다. 결과적으로 NFS 공유는 타사 기능을 활용하여 구성해야 합니다.
- Azure에 배포된 솔루션을 사용하여 SAP HANA에 대한 저장소 대기 시간 조건을 만족하는 타사 NFS 구성이 없습니다.

따라서 **hana/data** 및 **/hana/log** 볼륨을 공유할 수 없습니다. 단일 노드 중 이러한 볼륨을 공유하지 않으면 스케일 아웃 구성에서 SAP HANA 대기 노드를 사용하지 못합니다.

결과적으로 스케일 아웃 구성의 단일 노드에 대한 기본 설계는 다음과 같게 됩니다.

![단일 노드의 스케일 아웃 기본 사항](media/hana-vm-operations/scale-out-basics.PNG)

SAP HANA 스케일 아웃에 대한 VM 노드의 기본 구성은 다음과 같습니다.

- **/hana/shared**의 경우 SUSE Linux 12 SP3 기반의 고가용성 NFS 클러스터를 빌드합니다. 이 클러스터 호스트를 **/hana/공유** 스케일 아웃 구성 및 SAP NetWeaver 또는 BW/4HANA 중앙 서비스의 NFS 공유 합니다. 이러한 구성을 빌드하는 데 관한 설명서는 [SUSE Linux Enterprise Server의 Azure VM에 있는 NFS의 고가용성](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) 문서에서 확인할 수 있습니다.
- 모든 다른 디스크 볼륨은 서로 다른 노드 간에 공유되지 **않으며** NFS를 기반으로 하지 **않습니다**. 공유되지 않는 **hana/data** 및 **/hana/log**를 사용한 스케일 아웃 HANA 설치에 관한 설치 구성 및 단계는 이 문서의 뒷부분에서 제공됩니다.

>[!NOTE]
>지금까지 그래픽에 표시된 대로 고가용성 NFS 클러스터는 SUSE Linux에서만 지원됩니다. Red Hat 기반 항상 사용 가능한 NFS 솔루션을 나중에 주의를 것입니다.

노드에 대한 볼륨 크기 조정은 **/hana/shared**를 제외하고는 규모 강화의 경우와 동일합니다. M128s VM SKU의 경우 제안된 크기와 형식은 다음과 같습니다.

| VM SKU | RAM | 최대 VM I/O<br /> 처리량 | /hana/data | /hana/log | /root 볼륨 | /usr/sap | hana/backup |
| --- | --- | --- | --- | --- | --- | --- | --- |
| M128s | 2000GiB | 2000MB/s |3 x P30 | 2 x P20 | 1 x P6 | 1 x P6 | 2 x P40 |


제안 된 다른 볼륨에 대 한 저장소 처리량 실행 하려는 워크 로드를 충족 하는지 확인 합니다. 작업에 **/hana/data** 및 **/hana/log**에 대한 더 높은 볼륨이 필요한 경우 Azure Premium Storage VHD의 수를 증가시켜야 합니다. Azure 가상 머신 유형의 한도 내에서 I/O 처리량을 나열 된 증가 IOPS 보다 더 많은 Vhd 사용 하 여 볼륨 크기를 조정 합니다. 또한 **/hana/log** 볼륨을 구성하는 디스크에 Azure Write Accelerator를 적용합니다.
 
[SAP HANA TDI 스토리지 요구 사항](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) 문서에서 스케일 아웃에 대한 **/hana/shared** 볼륨의 크기를 정의하는 수식은 4개의 작업자 노드당 단일 작업자 노드의 메모리 크기로 지정됩니다.

약 2TB 메모리의 SAP HANA 스케일 아웃 인증 M128s Azure VM을 사용한다고 가정하면 SAP 권장 사항은 다음과 같이 요약할 수 있습니다.

- 1개 마스터 노드 및 최대 4개 작업자 노드의 경우 **/hana/shared** 볼륨의 크기는 2TB여야 합니다. 
- 1개의 마스터 노드와 5 및 8개의 작업자 노드에서 **/hana/shared**는 4TB여야 합니다. 
- 1개의 마스터 노드와 9 ~ 12개의 작업자 노드에서 **/hana/shared**에 대해 6TB 크기가 필요합니다. 
- 1개의 마스터 노드와 12 및 15개의 작업자 노드 사이를 사용하는 경우 8TB 크기의 **/hana/shared** 볼륨을 제공해야 합니다.

스케일 아웃 SAP HANA VM에 대한 단일 노드 구성의 그래픽에 표시되는 다른 중요한 설계는 VNet 또는 그 이상의 서브넷 구성입니다. SAP는 HANA 노드 간 통신으로부터 트래픽 발생 클라이언트/응용 프로그램을 분리할 것을 권장합니다. 그래픽에서와 같이 이는 두 개의 다른 vNIC를 VM에 연결하여 달성됩니다. 두 vNIC는 서로 다른 서브넷에 있고, 서로 다른 두 IP 주소를 가집니다. 그런 다음, NSG 또는 사용자 정의 경로를 사용하여 라우팅 규칙을 통해 트래픽 흐름을 제어합니다.

특히 Azure에서는 특정 vNIC에 서비스 품질 및 할당량을 적용할 방법이나 메서드가 없습니다. 결과적으로, 클라이언트/응용 프로그램 연결 및 노드 내 통신의 분리는 하나의 트래픽 스트림을 다른 것에 대해 우선시하는 기회를 제공하지 않습니다. 대신 분리로 인해 스케일 아웃 구성의 노드 내 통신을 보호할 때 보안 척도가 유지됩니다.  

>[!IMPORTANT]
>SAP는 이 문서에 설명된 대로 클라이언트/응용 프로그램 측에 대한 트래픽 및 노드 내 트래픽을 분리할 것을 권장합니다. 따라서 마지막 그래픽에 표시된 대로 아키텍처를 적절한 위치에 배치하는 것이 좋습니다.
>

네트워킹 관점에서 필요한 최소 네트워크 아키텍처는 다음과 같습니다.

![단일 노드의 스케일 아웃 기본 사항](media/hana-vm-operations/scale-out-networking-overview.PNG)

지금까지 지원되는 제한은 1개의 마스터 노드에 15개의 작업자를 추가하는 것입니다.

저장소의 관점에서 저장소 아키텍처는 다음과 같습니다.


![단일 노드의 스케일 아웃 기본 사항](media/hana-vm-operations/scale-out-storage-overview.PNG)

**/hana/shared** 볼륨은 고가용성 NFS 공유 구성에 위치합니다. 반면 다른 모든 드라이브는 개별 VM에 탑재된 ‘로컬’입니다. 

### <a name="highly-available-nfs-share"></a>고가용성 NFS 공유
현재 고가용성 NFS 클러스터는 SUSE Linux에서만 작동합니다. [SUSE Linux Enterprise Server의 Azure VM에 있는 NFS의 고가용성](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) 문서에 설정 방법이 나와 있습니다. NFS 클러스터를 SAP HANA 인스턴스를 실행하는 Azure VNet 외부의 다른 HANA 구성과 공유하지 않는 경우 동일한 VNet에 설치합니다. 자체 서브넷에 설치하고, 임의 트래픽이 전부 서브넷에 액세스하지는 못하도록 합니다. 대신에 해당 서브넷에 대한 트래픽을 **/hana/shared** 볼륨에 대한 트래픽을 실행하는 VM의 IP 주소로 제한할 수 있습니다.

**/hana/shared** 트래픽으로 라우팅해야 하는 HANA 스케일 아웃 VM의 vNIC와 관련된 권장 사항은 다음과 같습니다.

- **/hana/shared**에 대한 트래픽은 보통이므로 최소 구성에서 클라이언트 네트워크에 할당된 vNIC를 통해 라우팅합니다.
- 결국 **/hana/shared**에 대한 트래픽의 경우 SAP HANA 스케일 아웃 구성을 배포하고 해당 서브넷에 호스트되는 세 번째 vNIC를 할당하는 VNet에 세 번째 서브넷을 배포합니다. NFS 공유에 대한 트래픽의 세 번째 vNIC와 연결된 IP 주소를 사용합니다. 그런 다음, 별도의 액세스 및 라우팅 규칙을 적용할 수 있습니다.

>[!IMPORTANT]
>고가용성 NFS와 배포된 스케일 아웃 방식의 SAP HANA가 있는 VM 간 네트워크 트래픽은 어떠한 경우라도 [NVA](https://azure.microsoft.com/solutions/network-appliances/) 또는 유사한 가상 어플라이언스를 통해 라우팅됩니다. 반면 Azure NSG에는 이러한 디바이스가 없습니다. SAP HANA를 실행하는 VM에서 고가용성 NFS 공유에 액세스할 때 NVA 또는 유사한 가상 어플라이언스가 우회하는지 확인하기 위해 라우팅 규칙을 검사합니다.
> 

SAP HANA 구성 간 고가용성 NFS 클러스터를 공유하려는 경우 이러한 모든 HANA 구성을 동일한 VNet으로 이동합니다. 
 

### <a name="installing-sap-hana-scale-out-n-azure"></a>SAP HANA 스케일 아웃을 Azure에 설치
스케일 아웃 SAP 구성을 설치하려면 다음과 같은 대략적인 단계를 수행해야 합니다.

- 새로 배포하거나 새 Azure VNet 인프라를 조정
- Azure Managed Premium Storage 볼륨을 사용하여 새로운 VM 배포
- 새로 배포하거나 기존의 고가용성 NFS 클러스터를 조정
- 예를 들어 VM 간의 노드 내 통신이 [NVA](https://azure.microsoft.com/solutions/network-appliances/)를 통해 라우팅되지 않도록 네트워크 라우팅을 조정합니다. VM과 고가용성 NFS 클러스터 간 트래픽에 대해서도 마찬가지입니다.
- SAP HANA 마스터 노드를 설치합니다.
- SAP HANA 마스터 노드의 구성 매개 변수 조정
- 계속해서 SAP HANA 작업자 노드 설치 진행

#### <a name="installation-of-sap-hana-in-scale-out-configuration"></a>스케일 아웃 구성에 SAP HANA 설치
Azure VM 인프라가 배포되고 다른 모든 준비 작업이 완료되면 다음과 같은 단계로 SAP HANA 스케일 아웃 구성을 설치해야 합니다.

- SAP의 설명서에 따라 SAP HANA 마스터 노드 설치
- **설치 후에는 global.ini 파일을 변경하고 ‘basepath_shared = no’ 매개 변수를 global.ini에 추가해야 합니다**. 이 매개 변수 없이 '공유' 확장에서 실행 하려면 SAP HANA를 사용 **hana/data** 하 고 **/hana/log** 노드 간의 볼륨입니다. 자세한 내용은 [SAP 정보 #2080991](https://launchpad.support.sap.com/#/notes/2080991)에 설명되어 있습니다.
- global.ini 매개 변수를 변경한 후 SAP HANA 인스턴스 다시 시작
- 추가 작업자 노드를 추가합니다. <https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.00/en-US/0d9fe701e2214e98ad4f8721f6558c34.html>을 참조하세요. 예를 들어 설치 중 또는 나중에 로컬 hdblcm을 사용하여 SAP HANA 노드 간 통신을 위한 내부 네트워크를 지정합니다. 자세한 내용은 [SAP 정보 #2183363](https://launchpad.support.sap.com/#/notes/2183363)을 참조하세요. 

이 설치 루틴에 따라 설치한 스케일 아웃 구성은 **hana/data** 및 **/hana/log** 실행에 공유되지 않은 디스크를 사용하게 됩니다. 반면 합니다 **/hana/공유** 볼륨에 항상 사용 가능한 배치할 것 NFS 공유 합니다.


## <a name="sap-hana-dynamic-tiering-20-for-azure-virtual-machines"></a>Azure 가상 머신에 대한 SAP HANA Dynamic Tiering 2.0

Microsoft Azure에서는 Azure M 시리즈 VM에 대한 SAP HANA 인증 외에도 SAP HANA Dynamic Tiering 2.0을 지원합니다(아래의 SAP HANA Dynamic Tiering 설명서 링크 참조). 예를 들어 Azure Virtual Machine 내에서 SAP HANA Cockpit을 통해 제품을 설치하거나 운영하는 데에는 차이가 없지만 공식적인 Azure 지원에 필수적인 몇 가지 중요한 항목이 있습니다. 이러한 중요 사항은 아래에 설명되어 있습니다. 기사, "DT 2.0"의 약어 동적 계층화 2.0 전체 이름 대신 사용할 것입니다.

SAP HANA Dynamic Tiering 2.0은 SAP BW 또는 S4HANA에서 지원되지 않습니다. 주요 사용 사례는 현재 원시 HANA 애플리케이션입니다.


### <a name="overview"></a>개요

아래 그림에서는 Microsoft Azure의 DT 2.0 지원에 대한 개요를 제공합니다. 공식 인증을 준수하기 위해 따라야 하는 일단의 필수 요구 사항은 다음과 같습니다.

- DT 2.0은 전용 Azure VM에 설치해야 합니다. SAP HANA가 실행되는 동일한 VM에서 실행되지 않을 수 있습니다.
- SAP HANA 및 DT 2.0 VM은 동일한 Azure Vnet 내에 배포해야 합니다.
- Azure 가속 네트워킹을 사용하도록 설정된 SAP HANA 및 DT 2.0 VM을 배포해야 합니다.
- DT 2.0 VM에 대한 스토리지 유형은 Azure Premium Storage여야 합니다.
- DT 2.0 VM에 여러 Azure 디스크를 연결해야 합니다.
- Azure 디스크에서 스트라이핑을 사용하여 소프트웨어 RAID/스트라이프 볼륨을 만들어야 합니다(lvm 또는 mdadm을 통해).

자세한 내용은 다음 섹션에서 설명 하려고 합니다.

![SAP HANA DT 2.0 아키텍처 개요](media/hana-vm-operations/hana-dt-20.PNG)



### <a name="dedicated-azure-vm-for-sap-hana-dt-20"></a>SAP HANA DT 2.0 전용 Azure VM

Azure IaaS에서 DT 2.0은 전용 VM에서만 지원됩니다. HANA 인스턴스가 실행되는 동일한 Azure VM에서 DT 2.0을 실행할 수 없습니다. 처음에는 다음 두 가지 VM 유형을 사용하여 SAP HANA DT 2.0을 실행할 수 있습니다.

- M64-32ms 
- E32sv3 

VM 유형에 대한 설명은 [여기](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory)를 참조하세요.

비용을 절감하기 위해 "웜" 데이터를 오프로드하는 DT 2.0의 기본 개념을 고려하여 해당 VM 크기를 사용하는 것이 좋습니다. 가능한 조합과 관련하여 엄격한 규칙은 없습니다. 이 조합은 특정 고객 워크로드에 따라 다릅니다.

권장되는 구성은 다음과 같습니다.

| SAP HANA VM 유형 | DT 2.0 VM 유형 |
| --- | --- | 
| M128ms | M64-32ms |
| M128s | M64-32ms |
| M64ms | E32sv3 |
| M64s | E32sv3 |


SAP HANA 인증 M 시리즈 VM과 지원되는 DT 2.0 VM(M64-32ms 및 E32sv3)의 모든 조합이 가능합니다.


### <a name="azure-networking-and-sap-hana-dt-20"></a>Azure 네트워킹 및 SAP HANA DT 2.0

전용 VM에 DT 2.0을 설치하려면 DT 2.0 VM과 SAP HANA VM 간의 네트워크 처리량이 10GB 이상이어야 합니다. 따라서 반드시 모든 VM을 동일한 Azure Vnet에 배치하고 Azure 가속 네트워킹을 사용하도록 설정해야 합니다.

Azure 가속 네트워킹에 대한 자세한 내용은 [여기](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli)를 참조하세요.

### <a name="vm-storage-for-sap-hana-dt-20"></a>SAP HANA DT 2.0용 VM 저장소

DT 2.0 모범 사례 지침에 따라 디스크 IO 처리량은 물리적 코어당 50MB/초 이상이어야 합니다. 두 개의 Azure VM 형식에 대 한 사양을 살펴보면, 지원 되는 최대 디스크 IO VM에 대 한 처리량 한도 같은 DT 2.0:

- E32sv3    :   물리적 코어당 48MB/초의 속도를 의미하는 768MB/초(캐시되지 않음)
- M64-32ms  :  물리적 코어당 62.5MB/초의 속도를 의미하는 1,000MB/초(캐시되지 않음)

DT 2.0 VM에 여러 Azure 디스크를 연결하고 OS 수준에서 소프트웨어 RAID(스트라이핑)를 만들어 VM당 최대 디스크 처리량 한도를 달성해야 합니다. 이와 관련하여 단일 Azure 디스크는 최대 VM 한도에 도달하는 처리량을 제공할 수 없습니다. DT 2.0을 실행하려면 Azure Premium Storage가 필수적입니다. 

- 사용 가능한 Azure 디스크 유형에 대한 자세한 내용은 [여기서](../../windows/disks-types.md) 찾을 수 있습니다.
- mdadm을 통해 소프트웨어 RAID를 만드는 방법에 대한 자세한 내용은 [여기서](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid) 찾을 수 있습니다.
- 최대 처리량에 적합한 스트라이프 볼륨을 만들도록 LVM을 구성하는 방법에 대한 자세한 내용은 [여기서](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm) 찾을 수 있습니다.

크기 요구 사항에 따라 VM의 최대 처리량에 도달할 수 있는 다양한 옵션이 있습니다. 여기에는 VM 처리량 상한을 달성하도록 각 DT 2.0 VM 유형에 적용할 수 있는 데이터 볼륨 디스크 구성이 있습니다. E32sv3 VM은 더 작은 워크로드에 대한 항목 수준으로 간주되어야 합니다. 이 경우 속도가 충분히 빠르지 않아 VM의 크기를 M64-32ms로 조정해야 할 수도 있습니다.
M64-32ms VM에는 많은 메모리가 있으므로 특히 읽기 집약적인 워크로드의 경우 IO 로드가 한도에 도달하지 못할 수 있습니다. 따라서 스트라이프 집합에서 적은 수의 디스크가 고객 특정 워크로드에 따라 충분할 수 있습니다. 그러나 만일의 경우에 대비하여 아래의 디스크 구성이 최대 처리량을 보장하기 위해 선택되었습니다.


| VM SKU | 디스크 구성 1 | 디스크 구성 2 | 디스크 구성 3 | 디스크 구성 4 | 디스크 구성 5 | 
| ---- | ---- | ---- | ---- | ---- | ---- | 
| M64-32ms | 4 x P50 -> 16TB | 4 x P40 -> 8TB | 5 x P30 -> 5TB | 7 x P20 -> 3.5TB | 8 x P15 -> 2TB | 
| E32sv3 | 3 x P50 -> 12TB | 3 x P40 -> 6TB | 4 x P30 -> 4TB | 5 x P20 -> 2.5TB | 6 x P15 -> 1.5TB | 


특히 워크로드가 읽기 집약적인 경우 데이터베이스 소프트웨어의 데이터 볼륨에 대한 권장 사항에 따라 IO 성능을 높여 Azure 호스트 캐시를 "read-only(읽기 전용)"로 설정할 수 있습니다. 반면에 트랜잭션 로그의 경우 Azure 호스트 디스크 캐시는 "none"이어야 합니다. 

로그 볼륨의 크기와 관련하여 권장되는 시작점은 데이터 크기의 15%로 추론됩니다. 로그 볼륨은 비용 및 처리량 요구 사항에 따라 다양한 Azure 디스크 유형을 사용하여 만들 수 있습니다. 로그 볼륨의 경우 높은 I/O 처리량이 필요합니다.  VM 형식 M64-32ms를 사용하는 경우 [Write Accelerator](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator)를 사용하도록 설정하는 것이 좋습니다. Azure Write Accelerator는 트랜잭션 로그에 대한 최적 디스크 쓰기 대기 시간을 제공합니다(M 시리즈에서만 사용 가능). VM 유형별 최대 디스크 수와 같이 고려해야 할 몇 가지 항목도 있습니다. Write Accelerator에 대한 자세한 정보는 [여기](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator)를 참조할 수 있습니다.


로그 볼륨의 크기 조정에 대한 몇 가지 예는 다음과 같습니다.

| 데이터 볼륨 크기 및 디스크 유형 | 로그 볼륨 및 디스크 유형 구성 1 | 로그 볼륨 및 디스크 유형 구성 2 |
| --- | --- | --- |
| 4 x P50 -> 16TB | 5 x P20 -> 2.5TB | 3 x P30 -> 3TB |
| 6 x P15 -> 1.5TB | 4 x P6 -> 256GB | 1 x P15 -> 256GB |


SAP HANA 스케일 아웃과 마찬가지로, /hana/shared 디렉터리는 SAP HANA VM과 DT 2.0 VM 간에 공유해야 합니다. 고가용성 NFS 서버 역할을 하는 전용 VM을 사용하는 SAP HANA 스케일 아웃과 동일한 아키텍처를 사용하는 것이 좋습니다. 공유 백업 볼륨을 제공하기 위해 동일한 설계를 사용할 수 있습니다. 그러나 HA가 필요한지, 아니면 백업 서버로 작동할 수 있을 만큼 충분한 저장소 용량을 갖춘 전용 VM만 사용하는 것으로도 충분한지는 고객에게 달려 있습니다.



### <a name="links-to-dt-20-documentation"></a>DT 2.0 설명서에 대한 링크 

- [SAP HANA Dynamic Tiering 설치 및 업데이트 가이드](https://help.sap.com/viewer/88f82e0d010e4da1bc8963f18346f46e/2.0.03/en-US)
- [SAP HANA Dynamic Tiering 자습서 및 리소스](https://help.sap.com/viewer/fb9c3779f9d1412b8de6dd0788fa167b/2.0.03/en-US)
- [SAP HANA Dynamic Tiering PoC](https://blogs.sap.com/2017/12/08/sap-hana-dynamic-tiering-delivering-on-low-tco-with-impressive-performance/)
- [향상된 SAP HANA 2.0 SPS 02 Dynamic Tiering 기능](https://blogs.sap.com/2017/07/31/sap-hana-2.0-sps-02-dynamic-tiering-enhancements/)




## <a name="operations-for-deploying-sap-hana-on-azure-vms"></a>Azure VM에서 SAP HANA를 배포하기 위한 작업
다음 섹션에서는 Azure VM에서 SAP HANA 시스템을 배포하는 일과 관련된 몇 가지 작업을 설명합니다.

### <a name="back-up-and-restore-operations-on-azure-vms"></a>Azure VM의 백업 및 복원 작업
다음 문서에서는 SAP HANA 배포를 백업 및 복원하는 방법을 설명합니다.

- [SAP HANA 백업 개요](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [SAP HANA 파일 수준 백업](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [SAP HANA 저장소 스냅숏 벤치마크](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)


### <a name="start-and-restart-vms-that-contain-sap-hana"></a>SAP HANA를 포함하는 VM 시작 및 다시 시작
Azure 공용 클라우드의 두드러진 기능으로 사용한 컴퓨팅 시간(분)에 대해서만 요금이 부과되는 것을 들 수 있습니다. 예를 들어 SAP HANA를 실행 중인 VM을 종료하는 경우 해당 시간 동안의 저장소 비용만 청구됩니다. 초기 배포에서 VM에 대해 고정 IP 주소를 지정하는 경우 또 다른 기능을 사용할 수 있습니다. SAP HANA가 있는 VM을 다시 시작하면 VM이 이전 IP 주소로 다시 시작됩니다. 


### <a name="use-saprouter-for-sap-remote-support"></a>SAP 원격 지원에 SAProuter 사용
온-프레미스 위치와 Azure 사이에 사이트 간 연결이 있고 SAP 구성 요소를 실행하고 있는 경우 이미 SAProuter를 실행 중일 것입니다. 이 경우 원격 지원을 위해 다음 항목을 완료하세요.

- SAP HANA를 호스트하는 VM의 개인 및 고정 IP 주소를 SAProuter 구성에서 유지 관리합니다.
- HANA VM을 호스트하는 서브넷의 NSG에서 TCP/IP 포트 3299를 통해 트래픽을 허용하도록 구성합니다.

인터넷을 통해 Azure에 연결 중이고 SAP HANA를 사용하는 VM에 대한 SAP 라우터가 없는 경우 이 구성 요소를 설치해야 합니다. 관리 서브넷에서 별도의 VM에 SAProuter를 설치합니다. 다음 그림은 사이트 간 연결 없이 SAProuter를 사용하여 SAP HANA를 배포하는 경우의 대략적인 스키마를 보여 줍니다.

![사이트 간 연결 및 SAProuter 없는 SAP HANA의 대략적인 배포 스키마](media/hana-vm-operations/hana-simple-networking3.PNG)

SAPRouter는 Jumpbox VM이 아니라 별도의 VM에 설치해야 합니다. 별도의 VM에는 고정 IP 주소가 있어야 합니다. SAProuter를 SAP에서 호스트하는 SAProuter에 연결하려면 SAP에 IP 주소를 문의하세요. (SAP에서 호스트하는 SAProuter는 VM에 설치하는 SAProuter 인스턴스에 해당합니다.) SAP의 IP 주소를 사용하여 SAProuter 인스턴스를 구성합니다. 구성 설정에서 필수 포트는 TCP 포트 3299뿐입니다.

SAProuter를 통해 원격 지원 연결을 설정하고 유지 관리하는 방법에 대한 자세한 내용은 [SAP documentation](https://support.sap.com/en/tools/connectivity-tools/remote-support.html)(SAP 설명서)을 참조하세요.

### <a name="high-availability-with-sap-hana-on-azure-native-vms"></a>Azure 네이티브 VM의 SAP HANA 고가용성
SAP Applications 12 SP1 이상용 SUSE Linux Enterprise Server를 실행 중인 경우 STONITH 디바이스를 포함하는 Pacemaker 클러스터를 설정할 수 있습니다. 이 디바이스를 사용하면 HANA 시스템 복제를 통한 동기 복제와 자동 장애 조치(failover)를 사용하는 SAP HANA 구성을 설정할 수 있습니다. 설정 절차에 대한 자세한 내용은 [Azure 가상 머신에 대한 SAP HANA 고가용성 가이드](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview)를 참조하세요.
