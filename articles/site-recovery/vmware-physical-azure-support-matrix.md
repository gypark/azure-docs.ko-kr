---
title: Azure Site Recovery를 사용한 VMware VM 및 물리적 서버와 Azure 간 재해 복구를 위한 지원 매트릭스 | Microsoft Docs
description: Azure Site Recovery를 사용한 VMware VM 및 물리적 서버와 Azure 간 재해 복구를 위해 지원되는 운영 체제 및 구성 요소가 요약되어 있습니다.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: raynew
ms.openlocfilehash: 514aaaf7a274e60a17bbae62b3c62e7cf3668e7a
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237312"
---
# <a name="support-matrix-for-disaster-recovery--of-vmware-vms-and-physical-servers-to-azure"></a>VMware VM 또는 물리적 서버와 Azure 간 재해 복구를 위한 지원 매트릭스

이 문서에서는 [Azure Site Recovery](site-recovery-overview.md)를 사용하여 VMware VM을 Azure로 재해 복구하는 데 지원되는 구성 요소와 설정의 요약 정보를 제공합니다.

가장 간단한 배포 시나리오에서 Azure Site Recovery를 사용하여 시작하려면 [자습서](tutorial-prepare-azure.md)를 참조하세요. [여기](vmware-azure-architecture.md)에서 Azure Site Recovery 아키텍처에 대해 자세히 알아볼 수 있습니다.

## <a name="deployment-scenario"></a>배포 시나리오

**시나리오** | **세부 정보**
--- | ---
VMware Vm의 재해 복구 | 온-프레미스 VMware VM을 Azure로 복제. Azure Portal에서 또는 [PowerShell](vmware-azure-disaster-recovery-powershell.md)을 사용하여 이 시나리오를 배포할 수 있습니다.
물리적 서버의 재해 복구 | 온-프레미스 Windows/Linux 실제 서버를 Azure로 복제 Azure Portal에서 이 시나리오를 배포할 수 있습니다.

## <a name="on-premises-virtualization-servers"></a>온-프레미스 가상화 서버

**서버** | **요구 사항** | **세부 정보**
--- | --- | ---
VMware | vCenter Server 6.7, 6.5, 6.0이나 5.5 또는 vSphere 6.7, 6.5, 6.0이나 5.5 | vCenter Server를 사용하는 것이 좋습니다.<br/><br/> vSphere 호스트와 vCenter 서버가 프로세스 서버와 동일한 네트워크에 있는 것이 좋습니다. 기본적으로 프로세스 서버 구성 요소는 구성 서버에서 실행되므로 전용 프로세스 서버를 설정하지 않으면, 구성 서버를 설정한 네트워크가 여기에 해당합니다.
물리적 | N/A

## <a name="site-recovery-configuration-server"></a>Site Recovery 구성 서버

구성 서버는 구성 서버, 프로세스 서버 및 마스터 대상 서버를 포함하여 Site Recovery 구성 요소를 실행하는 온-프레미스 컴퓨터입니다. VMware 복제의 경우 모든 요구 사항에 따라 구성 서버를 설정하고, OVF 템플릿을 사용하여 VMware VM을 만듭니다. 물리적 서버 복제의 경우 구성 서버 컴퓨터를 수동으로 설정합니다.

**구성 요소** | **요구 사항**
--- |---
CPU 코어 | 8
RAM | 16GB
디스크 수 | 3개의 디스크<br/><br/> 디스크에는 OS 디스크, 프로세스 서버 캐시 디스크, 보존 드라이브(장애 복구용)가 포함됩니다.
사용 가능한 디스크 공간 | 프로세스 서버 캐시에 600GB의 공간이 필요합니다.
사용 가능한 디스크 공간 | 보존 드라이브에 600GB의 공간이 필요합니다.
운영 체제  | Windows Server 2012 R2 또는 Windows Server 2016 데스크톱 환경 포함 |
운영 체제 로케일 | 미국 영어(en-us)
PowerCLI | [PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "PowerCLI 6.0") 버전을 사용 하 여 구성 서버에 필수적 요소가 아닙니다 [9.14](https://support.microsoft.com/help/4091311/update-rollup-23-for-azure-site-recovery)합니다.
Windows Server 역할 | 다음을 사용하지 않음 <br/> - Active Directory Domain Services <br/>- 인터넷 정보 서비스 <br/> - Hyper-V |
그룹 정책| 다음을 사용하지 않음 <br/> - 명령 프롬프트에 대한 액세스 방지 <br/> - 레지스트리 편집 도구에 대한 액세스 방지 <br/> - 파일 첨부를 위한 트러스트 논리 <br/> - 스크립트 실행 켜기 <br/> [자세히 알아보기](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)|
IIS | 다음을 확인합니다.<br/><br/> - 기존의 기본 웹 사이트 없음 <br/> - [익명 인증](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) 사용 <br/> - [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) 설정 사용  <br/> - 포트 443에서 수신 대기하는 기존의 웹 사이트/앱 없음<br/>
NIC 유형 | VMXNET3(VMware VM으로 배포될 경우)
IP 주소 유형 | 공용
포트 | 컨트롤 채널 오케스트레이션에 443 사용<br/>데이터 전송에 9443 사용

## <a name="replicated-machines"></a>복제된 컴퓨터

Site Recovery는 지원되는 컴퓨터에서 실행되는 모든 워크로드의 복제를 지원합니다.

**구성 요소** | **세부 정보**
--- | ---
컴퓨터 설정 | Azure로 복제하는 컴퓨터는 [Azure 요구 사항](#azure-vm-requirements)을 충족해야 합니다.
머신 워크로드 | Site Recovery는 지원되는 머신에서 실행되는 모든 워크로드(즉, Active Directory, SQL 서버 등)의 복제를 지원합니다. [자세히 알아보기](https://aka.ms/asr_workload).
Windows 운영 체제 | Windows Server 2019 (에서 [9.22 버전](service-updates-how-to.md#links-to-currently-supported-update-rollups)), 64 비트 Windows Server 2016 (Server Core, 데스크톱 환경 포함 서버), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1 이상. </br> , [9.24 버전](https://support.microsoft.com/en-in/help/4503156)64 비트, 64 비트 Windows 7, Windows 8 64 비트, 64 비트 Windows 8.1, Windows 10 (Windows 7 RTM은 지원 되지 않음)</br>  [SP2 이상을 사용하는 Windows Server 2008 - 32비트 및 64비트](migrate-tutorial-windows-server-2008.md)(마이그레이션만 해당) </br></br> Windows 2016 Nano Server는 지원되지 않습니다.
Linux 운영 체제 아키텍처 | 64 비트 시스템만 지원 됩니다. 32 비트 시스템에서 지원 되지 않습니다.
Linux 운영 체제 | Red Hat Enterprise Linux: 5.2~5.11<b>\*\*</b>, 6.1~6.10<b>\*\*</b>, 7.0~7.6 <br/><br/>CentOS: 5.2~5.11<b>\*\*</b>, 6.1~6.10<b>\*\*</b>, 7.0~7.6 <br/><br/>Ubuntu 14.04 LTS 서버 [(지원 되는 커널 버전)](#ubuntu-kernel-versions)<br/><br/>Ubuntu 16.04 LTS 서버 [(지원 되는 커널 버전)](#ubuntu-kernel-versions)<br/><br/>Debian 7/debian 8 [(지원 되는 커널 버전)](#debian-kernel-versions)<br/><br/>SUSE Linux Enterprise Server 12 SP1, SP2, SP3, SP4 [(지원 되는 커널 버전)](#suse-linux-enterprise-server-12-supported-kernel-versions)<br/><br/>SUSE Linux Enterprise Server 11 SP3<b>\*\*</b>, SUSE Linux Enterprise Server 11 SP4 * </br></br>Oracle Linux 6.4, 6.5, 6.6, 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, Red Hat 호환 커널 또는 Unbreakable Enterprise Kernel Release 3, 4 및 5 (UEK3, UEK4, UEK5)를 실행 하는 경우 7.6 <br/><br/></br>- 복제된 머신을 SUSE Linux Enterprise Server 11 SP3에서 SP4로 업그레이드하는 것은 지원되지 않습니다. 업그레이드하려면 복제를 사용하지 않도록 설정하고, 업그레이드 후에 다시 사용하도록 설정합니다.</br></br> Azure에서 Linux 및 오픈 소스 기술 지원에 대해 - [자세히 알아보세요](https://support.microsoft.com/help/2941892/support-for-linux-and-open-source-technology-in-azure). Site Recovery는 Azure에서 Linux 서버를 실행하도록 장애 조치(failover)를 오케스트레이션합니다. 그러나 Linux 공급 업체 지원 수명 종료에 해당하지 않는 배포 버전만으로 제한될 수 있습니다.<br/><br/> - Linux 배포에서는 배포의 부 버전 릴리스/업데이트에 포함된 스톡 커널만 지원됩니다.<br/><br/> - 주요 Linux 배포 버전에서 보호된 시스템을 업그레이드하는 것은 지원되지 않습니다. 업그레이드하려면 복제를 사용하지 않도록 설정하고, 운영 체제를 업그레이드한 다음, 복제를 다시 사용하도록 설정합니다.<br/><br/> - Red Hat Enterprise Linux 5.2-5.11 또는 CentOS 5.2-5.11을 실행하는 서버의 경우 [LIS(Linux Integration Services) 구성 요소](https://www.microsoft.com/download/details.aspx?id=55106)가 설치되어 있어야 머신이 Azure에서 부팅될 수 있습니다.


### <a name="ubuntu-kernel-versions"></a>Ubuntu 커널 버전


**지원되는 릴리스** | **Azure Site Recovery Mobility Service 버전** | **커널 버전** |
--- | --- | --- |
14.04 LTS | [9.24][9.24 UR] | 3.13.0-24-generic 3.13.0-167-generic를<br/>3.16.0-25-generic에서 3.16.0-77-generic<br/>3.19.0-18-generic에서 3.19.0-80-generic<br/>4.2.0-18-generic에서 4.2.0-42-generic<br/>4.4.0-21-generic 4.4.0-143-generic를<br/>4.15.0-1023-azure 4.15.0-1040-azure |
14.04 LTS | [9.23][9.23 UR] | 3.13.0-24-generic 3.13.0-165-generic를<br/>3.16.0-25-generic에서 3.16.0-77-generic<br/>3.19.0-18-generic에서 3.19.0-80-generic<br/>4.2.0-18-generic에서 4.2.0-42-generic<br/>4.4.0-21-generic 4.4.0-142-generic를<br/>4.15.0-1023-azure 4.15.0-1037-azure |
14.04 LTS | [9.22][9.22 UR] | 3.13.0-24-generic~3.13.0-164-generic<br/>3.16.0-25-generic에서 3.16.0-77-generic<br/>3.19.0-18-generic에서 3.19.0-80-generic<br/>4.2.0-18-generic에서 4.2.0-42-generic<br/>4.4.0-21-generic에서 4.4.0-140-generic<br/>4.15.0-1023-azure~4.15.0-1036-azure |
14.04 LTS | [9.21][9.21 UR] | 3.13.0-24-generic에서 3.13.0-163-generic<br/>3.16.0-25-generic에서 3.16.0-77-generic<br/>3.19.0-18-generic에서 3.19.0-80-generic<br/>4.2.0-18-generic에서 4.2.0-42-generic<br/>4.4.0-21-generic에서 4.4.0-140-generic<br/>4.15.0-1023-azure에서 4.15.0-1035-azure |
|||
16.04 LTS | [9.23][9.24 UR] | 4.4.0-21-generic 4.4.0-143-generic를<br/>4.8.0-34-generic에서 4.8.0-58-generic<br/>4.10.0-14-generic에서 4.10.0-42-generic<br/>4.11.0-13-generic에서 4.11.0-14-generic<br/>4.13.0-16-generic에서 4.13.0-45-generic<br/>4.15.0-13-generic 4.15.0-46-generic<br/>4.11.0-1009-azure 4.11.0-1018-azure를<br/>4.13.0-1005-azure에서 4.13.0-1018-azure <br/>4.15.0-1012-azure 4.15.0-1040-azure|
16.04 LTS | [9.23][9.23 UR] | 4.4.0-21-generic 4.4.0-142-generic를<br/>4.8.0-34-generic에서 4.8.0-58-generic<br/>4.10.0-14-generic에서 4.10.0-42-generic<br/>4.11.0-13-generic에서 4.11.0-14-generic<br/>4.13.0-16-generic에서 4.13.0-45-generic<br/>4.15.0-13-generic 4.15.0-45-generic<br/>4.11.0-1009-azure에서 4.11.0-1016-azure<br/>4.13.0-1005-azure에서 4.13.0-1018-azure <br/>4.15.0-1012-azure 4.15.0-1037-azure|
16.04 LTS | [9.22][9.22 UR] | 4.4.0-21-generic에서 4.4.0-140-generic<br/>4.8.0-34-generic에서 4.8.0-58-generic<br/>4.10.0-14-generic에서 4.10.0-42-generic<br/>4.11.0-13-generic에서 4.11.0-14-generic<br/>4.13.0-16-generic에서 4.13.0-45-generic<br/>4.15.0-13-generic~4.15.0-43-generic<br/>4.11.0-1009-azure에서 4.11.0-1016-azure<br/>4.13.0-1005-azure에서 4.13.0-1018-azure <br/>4.15.0-1012-azure~4.15.0-1036-azure|
16.04 LTS | [9.21][9.21 UR] | 4.4.0-21-generic에서 4.4.0-140-generic<br/>4.8.0-34-generic에서 4.8.0-58-generic<br/>4.10.0-14-generic에서 4.10.0-42-generic<br/>4.11.0-13-generic에서 4.11.0-14-generic<br/>4.13.0-16-generic에서 4.13.0-45-generic<br/>4.15.0-13-generic에서 4.15.0-42-generic<br/>4.11.0-1009-azure에서 4.11.0-1016-azure<br/>4.13.0-1005-azure에서 4.13.0-1018-azure <br/>4.15.0-1012-azure에서 4.15.0-1035-azure|
16.04 LTS | [9.20][9.20 UR] | 4.4.0-21-generic에서 4.4.0-138-generic<br/>4.8.0-34-generic에서 4.8.0-58-generic<br/>4.10.0-14-generic에서 4.10.0-42-generic<br/>4.11.0-13-generic에서 4.11.0-14-generic<br/>4.13.0-16-generic에서 4.13.0-45-generic<br/>4.15.0-13-generic에서 4.15.0-38-generic<br/>4.11.0-1009-azure에서 4.11.0-1016-azure<br/>4.13.0-1005-azure에서 4.13.0-1018-azure <br/>4.15.0-1012-azure에서 4.15.0-1025-azure|

### <a name="debian-kernel-versions"></a>Debian 커널 버전


**지원되는 릴리스** | **Azure Site Recovery Mobility Service 버전** | **커널 버전** |
--- | --- | --- |
Debian 7 | [9.21][9.21 UR], [9.22][9.22 UR],[9.23][9.23 UR], [9.24][9.24 UR]| 3.2.0-4-amd64에서 3.2.0-6-amd64까지, 3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | [9.21][9.21 UR],[9.22][9.22 UR],[9.23][9.23 UR], [9.24][9.24 UR] | 3.16.0-4-amd64에서 3.16.0-7-amd64, 4.9.0-0.bpo.4-amd64에서 4.9.0-0.bpo.8-amd64 |


### <a name="suse-linux-enterprise-server-12-supported-kernel-versions"></a>SUSE Linux Enterprise Server 12 지원되는 커널 버전

**릴리스** | **모바일 서비스 버전** | **커널 버전** |
--- | --- | --- |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4) | [9.24][9.24 UR] | SP1 3.12.49-11-default에서 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default에서 3.12.74-60.64.107-default</br></br> SP2 4.4.21-69-default에서 4.4.120-92.70-default</br></br>4.4.121-92.101-default SP2(LTSS) 4.4.121-92.73-default</br></br>4.4.175-94.79-default에 SP3 4.4.73-5-default</br></br>SP4 4.12.14-94.41-default 4.12.14-95.6-default |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4) | [9.23][9.23 UR] | SP1 3.12.49-11-default에서 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default에서 3.12.74-60.64.107-default</br></br> SP2 4.4.21-69-default에서 4.4.120-92.70-default</br></br>4.4.121-92.101-default SP2(LTSS) 4.4.121-92.73-default</br></br>SP3 4.4.73-5-default에서 4.4.162-94.69-default</br></br>SP4 4.12.14-94.41-default 4.12.14-95.6-default |
SUSE Linux Enterprise Server 12(SP1,SP2,SP3) | [9.22][9.22 UR] | SP1 3.12.49-11-default에서 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default에서 3.12.74-60.64.107-default</br></br> SP2 4.4.21-69-default에서 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default에서 4.4.121-92.98-default</br></br>SP3 4.4.73-5-default에서 4.4.162-94.72-default |
SUSE Linux Enterprise Server 12(SP1,SP2,SP3) | [9.21][9.21 UR] | SP1 3.12.49-11-default에서 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default에서 3.12.74-60.64.107-default</br></br> SP2 4.4.21-69-default에서 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default에서 4.4.121-92.98-default</br></br>SP3 4.4.73-5-default에서 4.4.156-94.72-default |


## <a name="linux-file-systemsguest-storage"></a>Linux 파일 시스템/게스트 저장소

**구성 요소** | **지원됨**
--- | ---
파일 시스템 | ext3, ext4, XFS
볼륨 관리자 | [9.20 버전](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery) 이전 <br/> 1. LVM 지원 됩니다. <br/> 2. /boot LVM 볼륨에 지원 되지 않습니다. <br/> 3. 여러 OS 디스크가 지원 되지 않습니다.<br/><br/>[9.20 버전](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery) LVM에 /boot부터 지원 됩니다. 여러 OS 디스크가 지원 되지 않습니다.
반가상화 저장 디바이스 | 반가상화 드라이버에서 내보낸 디바이스는 지원되지 않습니다.
다중 큐 블록 IO 디바이스 | 지원되지 않습니다.
HP CCISS 저장소 컨트롤러가 있는 물리적 서버 | 지원되지 않습니다.
디바이스/탑재 지점 명명 규칙 | 디바이스 이름과 탑재 지점 이름은 고유해야 합니다. 대/소문자만 다른 두 개의 디바이스/탑재 지점 이름이 없어야 합니다. </br> 예제: 동일한 가상 머신의 디바이스 이름 두 개를 *device1*과 *Device1*로 지정하는 것이 허용되지 않습니다.
디렉터리 | [9.20 버전](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery) 이전 <br/> 1. /(root), /boot, /usr, /usr/local, /var, /etc 디렉터리는(별도의 파티션/파일 시스템으로 설정된 경우) 모두 원본 서버의 동일한 OS 디스크에 있어야 합니다.</br>2. /boot는 디스크 파티션에 있어야 하며 LVM 볼륨이 아니어야 합니다.<br/><br/> [9.20 버전](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery)부터는 위의 제한이 적용되지 않습니다. 둘 이상의 디스크에 걸쳐 있는 LVM 볼륨의 /boot은 지원되지 않습니다.
루트 디렉터리 | 한 가상 머신에 여러 부트 디스크가 있을 수 없습니다. <br/><br/> 부트 디스크가 없는 컴퓨터는 보호할 수 없습니다.
사용 가능한 공간 요구 사항| /root 파티션: 2GB <br/><br/> 설치 폴더: 250MB
XFSv5 | XFS 파일 시스템의 XFSv5 기능(예: 메타데이터 체크섬)은 모바일 서비스 버전 9.10 이상에서 지원됩니다. xfs_info 유틸리티를 사용하여 파티션에 대한 XFS 수퍼 블록을 확인합니다. 경우 `ftype` XFSv5 기능이 사용 중 1로 설정 됩니다.
BTRFS |9.22 버전인 BTRFS 지원 됩니다, 다음과 같은 시나리오를 제외 하 고</br>BTRFS 파일 시스템 하위 볼륨 보호를 사용 하도록 설정한 후 변경 되 면 BTRFS 지원 되지 않습니다. </br>BTRFS 파일 시스템 여러 디스크에 걸쳐 분산 되어, BTRFS 지원 되지 않습니다.</br>BTRFS 파일 시스템에서 RAID를 지 원하는 경우 BTRFS 지원 되지 않습니다.

## <a name="vmdisk-management"></a>VM/디스크 관리

**작업** | **세부 정보**
--- | ---
복제된 VM에서 디스크 크기 조정 |  지원됩니다.
복제된 VM에 디스크 추가 | VM에 대한 복제를 사용하지 않도록 설정하고, 디스크를 추가한 다음, 복제를 다시 사용하도록 설정합니다. 복제 VM에 디스크를 추가하는 기능은 현재 지원되지 않습니다.

## <a name="network"></a>네트워크

**구성 요소** | **지원됨**
--- | ---
호스트 네트워크 NIC 팀 | VMware VM에서 지원됩니다. <br/><br/>물리적 컴퓨터 복제에 지원되지 않습니다.
호스트 네트워크 VLAN | 예.
호스트 네트워크 IPv4 | 예.
호스트 네트워크 IPv6 | 아니요.
게스트/서버 네트워크 NIC 팀 | 아니요.
게스트/서버 네트워크 IPv4 | 예.
게스트/서버 네트워크 IPv6 | 아니요.
게스트/서버 네트워크 정적 IP(Windows) | 예.
게스트/서버 네트워크 정적 IP(Linux) | 예. <br/><br/>VM이 장애 복구(Failback) 시 DHCP를 사용하도록 구성되어 있습니다.
게스트/서버 네트워크 다중 NIC | 예.


## <a name="azure-vm-network-after-failover"></a>Azure VM 네트워크(장애 조치(failover) 후)

**구성 요소** | **지원됨**
--- | ---
Azure ExpressRoute | 예
ILB | 예
ELB | 예
Azure Traffic Manager | 예
다중 NIC | 예
예약된 IP 주소 | 예
IPv4 | 예
원본 IP 주소 유지 | 예
Azure Virtual Network 서비스 엔드포인트<br/> | 예
가속 네트워킹 | 아닙니다.

## <a name="storage"></a>Storage
**구성 요소** | **지원됨**
--- | ---
동적 디스크 | 운영 체제 디스크는 기본 디스크여야 합니다. <br/><br/>데이터 디스크는 동적 디스크일 수 있습니다.
Docker 디스크 구성 | 아닙니다.
호스트 NFS | VMware의 경우 예<br/><br/> 물리적 서버의 경우 아니요
호스트 SAN(iSCSI/FC) | 예
호스트 vSAN | VMware의 경우 예<br/><br/> 물리적 서버의 경우 해당 없음
호스트 다중 경로(MPIO) | 예. 테스트 제품: Microsoft DSM, EMC PowerPath 5.7 SP4, EMC PowerPath DSM for CLARiiON
호스트 가상 볼륨(VVol) | VMware의 경우 예<br/><br/> 물리적 서버의 경우 해당 없음
게스트/서버 VMDK | 예
게스트/서버 공유 클러스터 디스크 | 아닙니다.
게스트/서버 암호화된 디스크 | 아닙니다.
게스트/서버 NFS | 아닙니다.
게스트/서버 iSCSI | 아닙니다.
게스트/서버 SMB 3.0 | 아닙니다.
게스트/서버 RDM | 예<br/><br/> 물리적 서버의 경우 해당 없음
게스트/서버 디스크 > 1 TB | 예<br/><br/>최대 4,095GB<br/><br/> 디스크는 1024MB보다 커야 합니다.
4K 논리적 및 4k 물리적 섹터 크기 포함 게스트/서버 디스크 | 예
4K 논리적 및 512바이트 물리적 섹터 크기 포함 게스트/서버 디스크 | 예
스트라이프 디스크 포함 게스트/서버 볼륨 4TB 이상 <br/><br/>논리 볼륨 관리(LVM)| 예
게스트/서버 - 저장소 공간 | 아닙니다.
게스트/서버 디스크 핫 추가/제거 | 아닙니다.
게스트/서버 - 디스크 제외 | 예
게스트/서버 다중 경로(MPIO) | 아닙니다.
게스트/서버 EFI/UEFI 부팅 | VMware Vm 또는 Windows Server 2012를 실행 하는 물리적 서버를 마이그레이션하는 경우 또는 나중에 Azure를 지원 합니다.<br/><br/> 만 마이그레이션에 대 한 Vm을 복제할 수 있습니다. 온-프레미스로 장애 복구는 지원 되지 않습니다.<br/><br/> 서버는 OS 디스크에 4개 이상의 파티션을 가질 수 없습니다.<br/><br/> Mobility Service 버전 9.13 이상이 필요합니다.<br/><br/> NTFS만 지원 됩니다.

## <a name="replication-channels"></a>복제 채널

|**복제 유형**   |**지원됨**  |
|---------|---------|
|오프 로드 데이터 전송 (ODX)    |       아닙니다.  |
|오프라인 시드        |   아닙니다.      |
| Azure Data Box | 아닙니다.


## <a name="azure-storage"></a>Azure 저장소

**구성 요소** | **지원됨**
--- | ---
로컬 중복 저장소 | 예
지역 중복 저장소 | 예
읽기 액세스 지역 중복 저장소 | 예
쿨 저장소 | 아닙니다.
핫 저장소| 아닙니다.
블록 Blob | 아닙니다.
휴지 상태의 암호화(Storage 서비스 암호화)| 예
Premium Storage | 예
Import/Export 서비스 | 아닙니다.
대상 스토리지/캐시 스토리지(복제 데이터 저장에 사용됨) 계정에 구성된 가상 네트워크용 Azure Storage 방화벽 | 예
범용 v2 저장소 계정(핫 및 쿨 계층 모두) | 아닙니다.

## <a name="azure-compute"></a>Azure Compute

**기능** | **지원됨**
--- | ---
가용성 집합 | 예
가용성 영역 | 아닙니다.
HUB | 예
관리 디스크 | 예

## <a name="azure-vm-requirements"></a>Azure VM 요구 사항

Azure로 복제하는 온-프레미스 VM은 이 표에 요약되어 있는 Azure VM 요구 사항을 충족해야 합니다. Site Recovery에서 필수 구성 요소 검사를 실행할 때 일부 요구 사항이 충족되지 않으면 실패합니다.

**구성 요소** | **요구 사항** | **세부 정보**
--- | --- | ---
게스트 운영 체제 | 복제된 컴퓨터에 대해 [지원되는 운영 체제](#replicated-machines)를 확인합니다. | 지원되지 않는 경우 확인이 실패합니다.
게스트 운영 체제 아키텍처 | 64비트. | 지원되지 않는 경우 확인이 실패합니다.
운영 체제 디스크 크기 | 최대 2,048GB. | 지원되지 않는 경우 확인이 실패합니다.
운영 체제 디스크 수 | 1 | 지원되지 않는 경우 확인이 실패합니다.
데이터 디스크 수 | 64개 이하. | 지원되지 않는 경우 확인이 실패합니다.
데이터 디스크 크기 | 최대 4,095GB | 지원되지 않는 경우 확인이 실패합니다.
네트워크 어댑터 | 여러 어댑터가 지원됩니다. |
공유 VHD | 지원되지 않습니다. | 지원되지 않는 경우 확인이 실패합니다.
FC 디스크 | 지원되지 않습니다. | 지원되지 않는 경우 확인이 실패합니다.
BitLocker | 지원되지 않습니다. | 컴퓨터의 복제를 사용하도록 설정하기 전에 Bitlocker를 사용하지 않도록 설정해야 합니다. |
VM 이름 | 1~63자 사이입니다.<br/><br/> 문자, 숫자 및 하이픈으로 제한됩니다.<br/><br/> 컴퓨터 이름은 문자 또는 숫자로 시작하고 끝나야 합니다. |  Site Recovery에서 컴퓨터 속성의 값을 업데이트합니다.

## <a name="azure-site-recovery-churn-limits"></a>Azure Site Recovery에 대 한 변동 제한 사항

다음 테이블에는 Azure Site Recovery 제한이 제공됩니다. 이러한 한도는 테스트를 기반으로 하지만 모든 가능한 애플리케이션 I/O 조합을 다룰 수는 없습니다. 실제 결과는 애플리케이션 I/O 조합에 따라 달라질 수 있습니다. 최상의 결과 적극 권장 하 [deployment planner 도구 실행](site-recovery-deployment-planner.md) 방법과 테스트 장애 조치를 실행 하 여 테스트는 광범위 한 응용 프로그램을 응용 프로그램의 진정한 성능 상황을 확인할 수 있습니다.

**복제 저장소 대상** | **평균 원본 디스크 I/O 크기** |**평균 원본 디스크 데이터 변동** | **일일 총 원본 디스크 데이터 변동**
---|---|---|---
Standard Storage | 8KB | 2MB/초 | 디스크당 168GB
프리미엄 P10 또는 P15 디스크 | 8KB  | 2MB/초 | 디스크당 168GB
프리미엄 P10 또는 P15 디스크 | 16KB | 4MB/초 |  디스크당 336GB
프리미엄 P10 또는 P15 디스크 | 32KB 이상 | 8MB/초 | 디스크당 672GB
프리미엄 P20 또는 P30 또는 P40 또는 P50 디스크 | 8KB    | 5MB/초 | 디스크당 421GB
프리미엄 P20 또는 P30 또는 P40 또는 P50 디스크 | 16KB 이상 |20 MB/s | 디스크당 1684 GB

**원본 데이터 변동률** | **최대 제한**
---|---
VM당 평균 데이터 변동률| 25MB/초
VM의 모든 디스크에 대한 최고 데이터 변동률 | 54MB/초
프로세스 서버에서 지원하는 1일 최대 데이터 변동률 | 2TB

여기서는 I/O가 30% 겹치고 있다고 가정하는 평균 숫자입니다. Site Recovery는 중첩 비율, 더 큰 쓰기 크기 및 실제 워크로드 I/O 동작에 따라 더 높은 처리량을 다룰 수 있습니다. 앞의 숫자는 약 5분의 일반적인 백로그가 있다고 가정합니다. 즉, 데이터를 업로드한 후에 처리되며 5분 내에 복구 지점이 생성됩니다.

## <a name="vault-tasks"></a>자격 증명 모음 작업

**작업** | **지원됨**
--- | ---
리소스 그룹 간 자격 증명 모음 이동<br/><br/> 구독 내 및 구독 간 | 아닙니다.
저장소 그룹 간 저장소, 네트워크, Azure VM 이동<br/><br/> 구독 내 및 구독 간 | 아닙니다.


## <a name="download-latest-azure-site-recovery-components"></a>최신 Azure Site Recovery 구성 요소 다운로드

**Name** | **설명** | **최신 버전 다운로드 지침**
--- | --- | ---
구성 서버 | 온-프레미스 VMware 서버와 Azure 간 통신 조정  <br/><br/>  온-프레미스 VMware 서버에 설치 | 자세한 내용은 지침의 방문 [새로 설치](vmware-azure-deploy-configuration-server.md) 하 고 [기존 구성 요소를 최신 버전으로 업그레이드](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server)합니다.
프로세스 서버|기본적으로 구성 서버에 설치합니다. 복제 데이터를 수신하고, 캐싱, 압축 및 암호화를 사용하여 최적화하며, Azure Storage로 보냅니다. 배포가 늘어나면 프로세스 서버로 실행하는 별도의 프로세스 서버를 추가하여 더 큰 복제 트래픽을 처리할 수 있습니다.| 자세한 내용은 지침의 방문 [새로 설치](vmware-azure-set-up-process-server-scale.md) 하 고 [기존 구성 요소를 최신 버전으로 업그레이드](vmware-azure-manage-process-server.md#upgrade-a-process-server)합니다.
Mobility Service | 온-프레미스 VMware 서버/물리적 서버 및 Azure/보조 사이트 간 복제 조정<br/><br/> 복제하려는 VMware VM 또는 물리적 서버에 설치 | 자세한 내용은 지침의 방문 [새로 설치](vmware-azure-install-mobility-service.md) 하 고 [기존 구성 요소를 최신 버전으로 업그레이드](vmware-physical-manage-mobility-service.md#update-mobility-service-from-azure-portal)합니다.

최신 기능에 대 한 자세한 내용은 방문 [최신 릴리스](https://aka.ms/ASR_latest_release_notes)합니다.


## <a name="next-steps"></a>다음 단계
VMware VM의 재해 복구용으로 Azure를 준비하는 방법을 [알아봅니다](tutorial-prepare-azure.md).

[9.23 UR]: https://support.microsoft.com/help/4489582/update-rollup-33-for-azure-site-recovery
[9.22 UR]: https://support.microsoft.com/help/4489582/update-rollup-33-for-azure-site-recovery
[9.21 UR]: https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery
[9.20 UR]: https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery
[9.19 UR]: https://support.microsoft.com/help/4468181/azure-site-recovery-update-rollup-30
[9.18 UR]: https://support.microsoft.com/help/4466466/update-rollup-29-for-azure-site-recovery
