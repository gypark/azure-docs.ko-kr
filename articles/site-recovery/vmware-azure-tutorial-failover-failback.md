---
title: Site Recovery를 사용하여 Azure로 재해 복구 중에 VMware VM 및 물리적 서버 장애 조치(Failover) 및 장애 복구(Failback) | Microsoft Docs
description: Azure Site Recovery를 사용하여 Azure로 재해 복구 중에 Site Recovery를 사용하여 VMware VM 및 물리적 서버를 Azure로 장애 조치(Failover)하고, 온-프레미스 사이트로 장애 복구(Failback)하는 방법을 알아봅니다.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.topic: tutorial
ms.date: 05/30/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 854e09600440c28f7505a016566ea0f05e994f5f
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66418322"
---
# <a name="fail-over-and-fail-back-vmware-vms"></a>장애 조치(Failover) 및 장애 복구(Failback) VMware VM

이 문서에서는 온-프레미스 VMware VM(가상 머신)을 [Azure Site Recovery](site-recovery-overview.md)로 장애 조치(failover)하는 방법을 설명합니다.

이는 온-프레미스 머신에 대한 재해 복구를 Azure에 설정하는 방법을 보여 주는 자습서 시리즈 중 다섯 번째 자습서입니다.

이 자습서에서는 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * VMware VM 속성이 Azure 요구 사항을 준수하는지 확인합니다.
> * Azure로의 장애 조치(Failover)를 실행합니다.

> [!NOTE]
> 자습서는 시나리오에 맞는 가장 간단한 배포 경로를 보여줍니다. 가능한 경우 기본 옵션을 사용하고 가능한 모든 설정과 경로를 보여주지 않습니다. 장애 조치(failover)에 대해 자세히 알아보려면 [VM 및 물리적 서버 장애 조치(failover)](site-recovery-failover.md)를 참조하세요.

## <a name="before-you-start"></a>시작하기 전에

이전 자습서 완료:

1. VMware VM, Hyper-V VM 및 물리적 머신의 온-프레미스 재해 복구가 Azure에 수행되도록 [Azure를 설정](tutorial-prepare-azure.md)했는지 확인합니다.
2. 재해 복구용 온-프레미스 [VMware](vmware-azure-tutorial-prepare-on-premises.md) 또는 [Hyper-V](hyper-v-prepare-on-premises-tutorial.md) 환경을 준비합니다. 물리적 서버에 대한 재해 복구를 설정하는 경우 [지원 매트릭스](vmware-physical-secondary-support-matrix.md)를 검토합니다.
3. [VMware VM](vmware-azure-tutorial.md), [Hyper-V VM](hyper-v-azure-tutorial.md) 또는 [물리적 컴퓨터](physical-azure-disaster-recovery.md)에 대한 재해 복구를 설정합니다.
4. [재해 복구 드릴](tutorial-dr-drill-azure.md)을 실행하여 모든 항목이 예상대로 작동하는지 확인합니다.

## <a name="failover-and-failback"></a>장애 조치 및 장애 복구

장애 조치(Failover) 및 장애 복구(Failback)는 다음 4단계로 진행됩니다.

1. **Azure로 장애 조치(failover):** 온-프레미스 기본 사이트가 중단되면 Azure로 머신이 장애 조치(failover)됩니다. 장애 조치(Failover) 후 복제된 데이터에서 Azure VM이 만들어집니다.
2. **Azure VM 다시 보호:** Azure에서 온-프레미스 VMware VM으로 복제를 다시 시작하도록 Azure VM을 다시 보호합니다. 데이터 일관성을 지키기 위해 온-프레미스 VM은 다시 보호하는 동안 꺼집니다.
3. **온-프레미스로 장애 조치(failover):** 온-프레미스 사이트가 실행되면 Azure에서 장애 복구(failback)에 대한 장애 조치(failover)를 실행합니다.
4. **온-프레미스 VM 다시 보호:** 데이터를 장애 복구(failback)한 후에 Azure로 복제를 시작하도록 장애 복구(failback)한 온-프레미스 VM을 다시 보호합니다.

## <a name="verify-vm-properties"></a>VM 속성 확인

장애 조치(failover)를 실행하기 전에 먼저 VM 속성을 확인하여 해당 VM이 [Azure 요구 사항](vmware-physical-azure-support-matrix.md#replicated-machines)을 충족하는지 검토합니다.

다음과 같이 속성을 확인합니다.

1. **보호된 항목**에서 **복제된 항목**을 선택하고 확인하려는 VM을 선택합니다.

2. **복제된 항목** 창에 VM 정보, 상태 및 최신 사용 가능한 복구 지점의 요약이 제공됩니다. 자세한 내용을 보려면 **속성**을 선택합니다.

3. **컴퓨팅 및 네트워크**에서 필요에 따라 이러한 속성을 수정할 수 있습니다.
    * Azure 이름
    * 리소스 그룹
    * 대상 크기
    * [가용성 집합](../virtual-machines/windows/tutorial-availability-sets.md)
    * Managed Disks 설정

4. 다음을 비롯한 네트워크 설정을 보고 수정할 수 있습니다.

    * 장애 조치(failover) 후에 Azure VM이 배치될 네트워크 및 서브넷
    * 여기에 할당될 IP 주소

5. **디스크**에서 VM의 운영 체제 및 데이터 디스크에 대한 정보를 볼 수 있습니다.

## <a name="run-a-failover-to-azure"></a>Azure에 대한 장애 조치(Failover) 실행

1. **설정** > **복제된 항목**에서 장애 조치(failover)하려는 VM을 선택한 후 **장애 조치(failover)** 를 선택합니다.
2. **장애 조치(Failover)** 에서 장애 조치(failover)할 **복구 지점**을 선택합니다. 다음 옵션 중 하나를 사용할 수 있습니다.
   * **최신**: 이 옵션은 Site Recovery로 전송된 모든 데이터를 먼저 처리합니다. 이 옵션은 장애 조치(failover) 후에 생성된 Azure VM은 장애 조치(failover)가 트리거되었을 때 Site Recovery로 복제된 모든 데이터를 보유하므로 가장 낮은 RPO(복구 지점 목표)를 제공합니다.
   * **가장 최근에 처리됨**: 이 옵션은 VM을 Site Recovery에서 처리된 최신 복구 지점으로 장애 조치(failover)합니다. 이 옵션은 처리되지 않은 데이터를 처리하는 데 시간이 투입되지 않으므로 낮은 RTO(복구 시간 목표)를 제공합니다.
   * **최신 앱 일치**: 이 옵션은 VM을 Site Recovery에서 처리된 최신 앱 일치 복구 지점으로 장애 조치(failover)합니다.
   * **사용자 지정**: 이 옵션을 통해 복구 지점을 지정할 수 있습니다.

3. 장애 조치를 트리거하기 전에 원본 VM을 종료하려고 시도하려면 **장애 조치(failover)를 시작하기 전에 머신을 종료합니다.** 를 선택합니다. 종료가 실패하더라도 장애 조치(failover)는 계속됩니다. **작업** 페이지에서 장애 조치 진행 상황 확인을 수행할 수 있습니다.

일부 시나리오에서는 장애 조치(failover)를 위해서는 추가 처리가 필요하며 이러한 작업을 완료하는 데는 약 8~10분이 소요됩니다. 다음에 대한 테스트 장애 조치(failover) 시간이 길어질 수 있습니다.

* 9.8 이전의 모바일 서비스 버전을 실행하는 VMware VM
* 실제 서버
* VMware Linux VM
* 물리적 서버로 보호되는 Hyper-V VM
* DHCP 서비스를 사용하도록 설정되지 않은 VMware VM
* 다음 부팅 드라이버가 없는 VMware VM: storvsc, vmbus, storflt, intelide, atapi.

> [!WARNING]
> 진행 중인 장애 조치(Failover)는 취소하지 마세요. 장애 조치(failover)를 시작하기 전에 VM 복제가 중지됩니다. 진행 중인 장애 조치(failover)를 취소하면 장애 조치(failover)가 중지되지만 VM은 다시 복제되지 않습니다.

## <a name="connect-to-failed-over-vm"></a>장애 조치(failover)된 VM에 연결

1. 장애 조치(failover) 후에 RDP(원격 데스크톱 프로토콜) 및 SSH(보안 셸)를 사용하여 Azure VM에 연결하려는 경우 [요구 사항이 충족되었는지 확인](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)합니다.
2. 장애 조치(failover) 후 VM으로 이동하고 [연결](../virtual-machines/windows/connect-logon.md)하여 유효성을 검사합니다.
3. 장애 조치(failover) 후 다른 복구 지점을 사용하려면 **복구 지점 변경**을 사용합니다. 다음 단계에서 장애 조치(failover)를 커밋하면 이 옵션을 더 이상 사용할 수 없습니다.
4. 유효성 검사 후 **커밋**을 선택하여 장애 조치(failover) 후 VM의 복구 지점을 마무리합니다.
5. 커밋 후 다른 사용 가능한 복구 지점을 모두 삭제합니다. 이 단계를 수행하면 장애 조치(failover)가 완료됩니다.

>[!TIP]
> 장애 조치(failover) 후 연결 문제가 발생하는 경우 [문제 해결 가이드](site-recovery-failover-to-azure-troubleshoot.md)를 따릅니다.

## <a name="next-steps"></a>다음 단계

장애 조치(failover) 후 Azure VM을 온-프레미스로 다시 보호합니다. 그러면 VM이 다시 보호되고 온-프레미스 사이트로 복제된 후 준비가 되었을 때 Azure에서 장애 복구(failback)가 수행됩니다.

> [!div class="nextstepaction"]
> [Azure VM 다시 보호](vmware-azure-reprotect.md)
> [Azure에서 장애 복구(failback)](vmware-azure-failback.md)
