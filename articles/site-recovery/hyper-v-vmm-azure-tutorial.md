---
title: Site Recovery를 사용 하 여 Azure에 VMM 클라우드의 온-프레미스 Hyper-v Vm에 대 한 재해 복구 설정 | Microsoft Docs
description: Site Recovery를 사용 하 여 Azure에 System Center VMM 클라우드의 온-프레미스 Hyper-v Vm 재해 복구를 설정 하는 방법을 알아봅니다.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 653db1497fcce5981bba7416f073b0330ca2861f
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66398142"
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-in-vmm-clouds-to-azure"></a>VMM 클라우드의 온-프레미스 Hyper-V VM에서 Azure로의 재해 복구 설정

이 문서에서는 온-프레미스 Hyper-v Vm에서 System Center Virtual Machine Manager (VMM)를 사용 하 여 Azure로 재해 복구 관리에 대 한 복제를 사용 하도록 설정 하는 방법을 설명 합니다 [Azure Site Recovery](site-recovery-overview.md) 서비스입니다. VMM을 사용 하지 않는다면 [이 자습서를 따라](hyper-v-azure-tutorial.md) 대신 합니다.

이 자습서는 온-프레미스 VMware VM을 Azure로 재해 복구하도록 설정하는 방법을 보여주는 자습서 시리즈 중 세 번째 자습서입니다. 이전 자습서에서는 우리가 [온-프레미스 Hyper-v 환경 준비](hyper-v-prepare-on-premises-tutorial.md) Azure로 재해 복구에 대 한 합니다.

이 자습서에서는 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * 복제 원본 및 대상을 선택합니다.
> * 온-프레미스 Site Recovery 구성 요소 및 대상 복제 환경을 포함한 원본 복제 환경을 설정합니다.
> * VMM VM 네트워크와 Azure 가상 네트워크 간에 매핑할 네트워크 매핑을 설정 합니다.
> * 복제 정책을 만듭니다.
> * VM의 복제를 사용하도록 설정합니다.

> [!NOTE]
> 자습서는 시나리오의 가장 간단한 배포 경로를 보여줍니다. 가능한 경우 기본 옵션을 사용하고 가능한 모든 설정과 경로를 보여주지 않습니다. 자세한 지침은 [Site Recovery 설명서](https://docs.microsoft.com/azure/site-recovery)의 **방법 가이드** 섹션에 있는 문서를 검토하세요.

## <a name="before-you-begin"></a>시작하기 전에

이 문서는 시리즈의 세 번째 자습서입니다. 여기서는 이전 자습서의 작업을 이미 완료했다고 가정합니다.

1. [Azure 준비](tutorial-prepare-azure.md)
2. [온-프레미스 Hyper-V 준비](tutorial-prepare-on-premises-hyper-v.md)

## <a name="select-a-replication-goal"></a>복제 목표 선택

1. Azure Portal에서 **Recovery Services 자격 증명 모음**으로 이동하여 자격 증명 모음을 선택합니다. 이전 자습서에서는 **ContosoVMVault** 자격 증명 모음을 준비했습니다.
2. **시작**에서 **Site Recovery**를 선택한 다음, **인프라 준비**를 선택합니다.
3. **보호 목표** > **머신이 어디에 있나요?** 에서 **온-프레미스**를 선택합니다.
4. **머신을 복제할 위치를 선택하세요.** 에서 **Azure**를 선택합니다.
5. **머신이 가상화되어 있습니까?** 에서 **예, Hyper-V 사용**을 선택합니다.
6. **System Center VMM에서 Hyper-v 호스트를 관리 하는 데 사용 중 입니까?** 를 선택 **예**합니다.
7.  **확인**을 선택합니다.

    ![복제 목표](./media/hyper-v-vmm-azure-tutorial/replication-goal.png)

## <a name="confirm-deployment-planning"></a>배포 계획 확인

1. **배포 계획**에서 대규모 배포를 계획 중인 경우 페이지의 링크를 통해 Hyper-V용 Deployment Planner를 다운로드합니다. Hyper-V 배포 계획에 대해 [자세히 알아보세요](hyper-v-deployment-planner-overview.md).
2. 이 자습서에서는 Deployment Planner가 필요하지 않습니다. **배포 계획을 완료하셨나요**에서 **나중에 수행**을 선택한 다음, **확인**을 선택합니다.

## <a name="set-up-the-source-environment"></a>원본 환경 설정

원본 환경을 설정할 때 VMM 서버의 Azure Site Recovery Provider를 설치 하 고 자격 증명 모음에 서버를 등록 합니다. 각 Hyper-v 호스트에 Azure Recovery Services 에이전트를 설치 합니다.

1. **인프라 준비**를 선택 **원본**합니다.
2. **원본 준비**를 선택 **+ VMM** VMM 서버를 추가 합니다. **서버 추가**의 **서버 유형**에서 **System Center VMM 서버**가 표시되는지 확인합니다.
3. Microsoft Azure Site Recovery Provider용 설치 관리자를 다운로드합니다.
4. 자격 증명 모음 등록 키를 다운로드합니다. 공급자 설치를 실행 하는 경우이 키가 필요 합니다. 이 키는 생성된 날로부터 5일간 유효합니다.
5. Microsoft Azure Recovery Services 에이전트의 설치 관리자를 다운로드 합니다.

    ![공급자 및 에이전트 등록 키 다운로드](./media/hyper-v-vmm-azure-tutorial/download-vmm.png)

### <a name="install-the-provider-on-the-vmm-server"></a>VMM 서버에 공급자 설치

1. Azure Site Recovery Provider 설치 마법사 > **Microsoft 업데이트**에서 Provider 업데이트를 확인하려면 Microsoft 업데이트를 사용하는 옵션을 선택합니다.
2. **설치가**공급자에 대 한 기본 설치 위치를 선택 **설치**합니다.
3. 설치 후에 Microsoft Azure Site Recovery 등록 마법사 > **자격 증명 모음 설정**를 선택 **찾아보기**, 및 **키 파일**선택, 자격 증명 모음 키 파일 다운로드합니다.
4. Azure Site Recovery 구독 및 자격 증명 모음 이름(**ContosoVMVault**)을 지정합니다. 자격 증명 모음에서 식별할 수 있도록 VMM 서버의 식별 이름을 지정합니다.
5. **프록시 설정**에서 **프록시 서버 없이 Azure Site Recovery에 직접 연결**을 선택합니다.
6. 데이터를 암호화하는 데 사용되는 인증서의 기본 위치를 적용합니다. 장애 조치(failover) 시에는 암호화된 데이터의 암호가 해독됩니다.
7. **클라우드 메타데이터 동기화**에서 **클라우드 메타데이터를 Site Recovery 포털에 동기화하세요.** 를 선택합니다. 이 작업은 각 서버에서 한 번만 수행 해야 합니다. 그런 다음 **등록**을 선택합니다.
8. 서버 자격 증명 모음에 등록 되 면 선택 **완료**합니다.

등록이 완료되면 Azure Site Recovery에서 서버의 메타데이터가 검색되며 VMM 서버가 **Site Recovery 인프라**에 표시됩니다.

### <a name="install-the-recovery-services-agent-on-hyper-v-hosts"></a>Hyper-v 호스트에 Recovery Services 에이전트 설치

복제 하려는 Vm이 포함 된 각 Hyper-v 호스트에 에이전트를 설치 합니다.

1. Microsoft Azure Recovery Services 에이전트 설치 마법사에서 > **필수 구성 요소 확인**를 선택 **다음**합니다. 누락 된 필수 조건은 자동으로 설치 됩니다.
2. **설치 설정을**, 설치 위치 및 캐시 위치를 수락 합니다. 캐시 드라이브에는 5GB 이상의 저장 공간이 필요합니다. 600GB 이상의 여유 공간이 있는 드라이브를 사용하는 것이 좋습니다. 그런 후 **설치**를 선택합니다.
3. **설치**설치가 완료 되 면, 선택 **닫기** 마법사를 마칩니다.

    ![에이전트 설치](./media/hyper-v-vmm-azure-tutorial/mars-install.png)

## <a name="set-up-the-target-environment"></a>대상 환경 설정

1. **인프라 준비** > **대상**을 선택합니다.
2. 구독을 선택하고 장애 조치(failover) 후 Azure VM이 생성될 리소스 그룹 **ContosoRG**를 선택합니다.
3. 선택 된 **Resource Manager** 배포 모델입니다.

Site Recovery가 호환되는 Azure 저장소 계정 및 네트워크가 하나 이상 있는지 확인합니다.

## <a name="configure-network-mapping"></a>네트워크 매핑 구성

1. **Site Recovery 인프라** > **네트워크 매핑** > **네트워크 매핑**을 선택 합니다 **+ 네트워크 매핑** 아이콘입니다.
2. **네트워크 매핑 추가**에서 원본 VMM 서버를 선택합니다. 대상으로 **Azure**를 선택합니다.
3. 장애 조치(failover) 후 구독과 배포 모델을 확인합니다.
4. **원본 네트워크**에서 원본 온-프레미스 VM 네트워크를 선택합니다.
5. **대상 네트워크**에서 장애 조치(failover) 후 만들어지는 복제본 Azure VM가 위치할 Azure 네트워크를 선택합니다. 그런 다음 **확인**을 선택합니다.

    ![네트워크 매핑](./media/hyper-v-vmm-azure-tutorial/network-mapping-vmm.png)

## <a name="set-up-a-replication-policy"></a>복제 정책 설정

1. **인프라 준비** > **복제 설정** >  **+만들기 및 연결**을 차례로 선택합니다.
2. **만들기 및 연결 정책**에서 정책 이름을 지정합니다. 여기서는 **ContosoReplicationPolicy**를 사용합니다.
3. 선택한 기본 설정을 그대로 두고 **확인**합니다.
    - **복사 빈도** 는 초기 복제 후 델타 데이터를 복제 5 분 마다 나타냅니다.
    - **복구 지점 보존** 2 시간 동안 각 복구 지점 보존 되는 것을 나타냅니다.
    - **앱 일치 스냅숏 빈도**는 앱 일치 스냅숏을 포함하는 복구 지점이 1시간마다 만들어짐을 나타냅니다.
    - **초기 복제 시작 시간**은 초기 복제가 즉시 시작됨을 나타냅니다.
    - **Azure에 저장 된 데이터 암호화** 기본값으로 설정 됩니다 (**해제**) 하 고 Azure에서 미사용 데이터 암호화 되지 않습니다 나타냅니다.
4. 정책이 만들어지면 **확인**을 선택합니다. 새 정책을 만들면 VMM 클라우드를 사용 하 여 자동으로 연결 되어 있습니다.

## <a name="enable-replication"></a>복제 사용

1. **애플리케이션 복제**에서 **원본**을 선택합니다.
2. **원본**에서 VMM 클라우드를 선택합니다. 그런 다음 **확인**을 선택합니다.
3. **대상**대상 (Azure) 자격 증명 모음 구독을 확인 하 고 선택 합니다 **Resource Manager** 모델입니다.
4. 선택 된 **contosovmsacct1910171607** 저장소 계정 및 **ContosoASRnet** Azure 네트워크.
5. **가상 머신** > **선택**에서 복제하려는 VM을 선택합니다. 그런 다음 **확인**을 선택합니다.

   **작업** > **Site Recovery 작업**에서 **보호 사용** 작업의 진행률을 추적할 수 있습니다. **보호 완료** 작업이 완료되면 초기 복제가 완료되고 VM에서 장애 조치할 준비가 됩니다.

## <a name="next-steps"></a>다음 단계
> [!div class="nextstepaction"]
> [재해 복구 드릴 실행](tutorial-dr-drill-azure.md)
