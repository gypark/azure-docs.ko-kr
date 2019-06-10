---
title: Azure Lab Services 정보 | Microsoft 문서 도구
description: Lab Services를 사용하면 개발자, 테스터, 교육자, 학생 및 기타 사람들이 사용할 수 있는 가상 머신으로 랩을 쉽게 만들고, 관리하고, 보안을 유지할 수 있는 방법에 대해 알아봅니다.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 07/13/2018
ms.author: spelluru
ms.openlocfilehash: a4ca5cba924a3269f279469f26e68acdb0ad0659
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59257623"
---
# <a name="an-introduction-to-azure-lab-services"></a>Azure Lab Services 소개
Azure Lab Services를 사용하면 클라우드에서 팀 환경(예: 개발 환경, 테스트 환경, 클래스룸 랩 환경)을 신속하게 설정할 수 있습니다. 랩 소유자는 랩을 만들고, Windows 또는 Linux 가상 머신을 프로비전하고, 필요한 소프트웨어 및 도구를 설치하고, 랩 사용자가 사용할 수 있게 합니다. 랩 사용자는 랩의 VM(가상 머신)에 연결하여 그것들을 일상 업무, 단기 프로젝트 또는 클래스룸 연습에 사용합니다. 사용자가 랩에서 리소스를 활용하기 시작하면 랩 관리자는 여러 랩의 비용 및 사용을 분석하고, 조직 또는 팀의 비용을 최적화하기 위해 포괄적인 정책을 설정할 수 있습니다.

> [!IMPORTANT]
> **Azure DevTest Labs**는 새로운 유형의 랩(Azure Lab Services)으로 확장되고 있습니다!
>  
> Azure Lab Services를 사용하면 클래스룸 랩과 같은 관리되는 랩 형식을 만들 수 있습니다. 서비스 자체는 VM 가동에서 오류 처리 및 인프라 확장에 이르기까지 관리되는 랩 형식에 대한 모든 인프라 관리를 처리합니다. 현재 [DevTest Labs](https://azure.microsoft.com/services/devtest-lab/) 및 [Azure Lab Services](https://azure.microsoft.com/services/lab-services/)는 Azure Portal에서 계속 별도의 서비스로 유지됩니다. 

## <a name="key-capabilities"></a>주요 기능

Azure Lab Services는 다음과 같은 주요 기능/특징을 지원합니다.

- **랩의 빠르고 유연한 설정**. 랩 소유자는 Azure Lab Services를 사용하여 필요에 따라 랩을 신속하게 설정할 수 있습니다. 이 서비스는 관리되는 랩 형식의 모든 Azure 인프라 작업을 처리하거나 랩 소유자가 랩 소유자의 구독에서 인프라를 자체 관리하고 사용자 지정할 수 있는 옵션을 제공합니다. 이 서비스는 서비스가 관리하는 랩을에 대한 인프라의 기본 확장 및 복구 기능을 제공합니다.
- **랩 사용자를 위한 간소화된 경험**. 클래스룸 랩과 같은 관리되는 랩 형식에서, 랩 사용자는 등록 코드로 랩에 등록하고 언제든지 랩에 액세스하여 랩 리소스를 사용할 수 있습니다. 랩 소유자는 DevTest Labs에서 만든 랩에서 가상 머신을 만들고 액세스하며, 데이터 디스크를 관리 및 재사용하고, 재사용 할 수 있는 비밀을 설정하도록 랩 사용자들에게 권한을 부여할 수 있습니다.  
- **비용 최적화 및 분석**. 랩 소유자는 랩 일정을 설정하여 가상 머신를 자동으로 종료하고 시작할 수 있습니다. 랩 소유자는 랩의 가상 머신에 사용자가 액세스할 수 있는 시간대를 지정하고, 사용자 또는 랩별로 사용 정책을 설정하여 비용을 최적화하고, 랩에서 사용 및 활동 동향을 분석하기 위해 일정을 설정할 수 있습니다. 클래스룸 랩과 같은 관리되는 랩 형식의 경우, 현재 비용 최적화 및 분석 옵션 중 더 작은 하위 집합을 사용할 수 있습니다.
- **내장된 보안**. 랩 소유자는 랩에 대한 프라이빗 가상 네트워크와 서브넷을 설정하고 공유되는 공용 IP 주소를 사용할 수 있습니다. 랩 사용자는 ExpressRoute 또는 사이트 간 VPN으로 구성된 가상 네트워크를 사용하여 안전하게 리소스에 액세스할 수 있습니다. (현재 DevTest Labs에서만 사용 가능)
- **워크 플로와 도구에 통합**. Azure Lab Services를 사용하여 랩을 조직의 웹 사이트 및 관리 시스템에 통합할 수 있습니다. 지속적인 통합/지속적인 배포(CI/CD) 도구 내에서 환경을 자동으로 프로비전할 수 있습니다. (현재 DevTest Labs에서만 사용 가능)

> [!NOTE]
> 현재 Azure Lab Services는 Azure Marketplace 이미지로 만든 VM만 지원합니다. 사용자 지정 이미지를 사용하거나 랩 환경에서 다른 PaaS 리소스를 만들려면 DevTest Labs를 사용하십시오. 자세한 내용은 [DevTest Labs에서 사용자 지정 이미지 만들기](devtest-lab-create-custom-image-from-vm-using-portal.md)와 [Resource Manager 템플릿을 사용하여 랩 환경 만들기](devtest-lab-create-environment-from-arm.md)를 참조하세요.

## <a name="scenarios"></a>시나리오

Azure Lab Services가 지원하는 몇 가지 시나리오는 다음과 같습니다.

### <a name="set-up-a-resizable-computer-lab-in-the-cloud-for-your-classroom"></a>클래스룸을 위해 크기 조정이 가능한 컴퓨터 랩을 클라우드에 설치  

- 관리되는 클래스룸 랩 만들기 필요한 서비스를 정확히 알려주시면 랩의 기술적 세부 사항이 아닌, 클래스를 가르치는 데 집중할 수 있도록 랩의 인프라를 만들고 관리합니다.
- 학생들에게 클래스에 필요한 것으로 정확히 구성된 가상 머신 랩을 제공합니다. 각 학생에게 VM을 클래스 학습에 사용하도록 제한된 시간을 줍니다.  
- 학교의 실제 컴퓨터 랩을 클라우드로 옮깁니다. VM 수를 랩에 설정한 최대 사용량 및 비용 임계 값에 이르기 까지만 자동으로 조정합니다.
- 마치면 클릭 한 번으로 랩을 삭제합니다.

### <a name="use-devtest-labs-for-development-environments"></a>개발 환경에 DevTest Labs 사용

Azure DevTest Labs를 사용하여 여러 주요 시나리오를 구현할 수 있지만 기본 시나리오 중 하나는 DevTest Labs를 사용하여 개발자를 위한 개발 컴퓨터를 호스트하는 것입니다. 이 시나리오에서 DevTest Labs는 다음과 같은 이점을 제공합니다.

- 개발자는 필요에 따라 개발 컴퓨터를 신속하게 프로비전할 수 있습니다.
- 재사용 가능한 템플릿 및 아티팩트를 사용하여 Windows 및 Linux 환경을 프로비저닝합니다.
- 개발자는 필요할 때마다 개발 컴퓨터를 쉽게 사용자 지정할 수 있습니다.
- 관리자는 개발자가 개발하는 데 필요한 것보다 많은 VM을 얻을 수 없도록 하고 VM을 사용하지 않을 때 종료되도록 함으로써 비용을 제어할 수 있습니다.

자세한 내용은 [개발을 위해 DevTest Labs 사용](devtest-lab-developer-lab.md)을 참조하세요.

### <a name="use-devtest-labs-for-test-environments"></a>테스트 환경에 DevTest Labs 사용

Azure DevTest Labs를 사용하여 여러 주요 시나리오를 구현할 수 있지만 기본 시나리오는 DevTest Labs를 사용하여 테스터를 위한 컴퓨터를 호스트하는 것입니다. 이 시나리오에서 DevTest Labs는 다음과 같은 이점을 제공합니다.

- 테스터가 재사용 가능한 템플릿과 아티팩트를 사용하여 Windows와 Linux 환경을 빠르게 프로비전함으로써 최신 버전의 애플리케이션을 테스트할 수 있습니다.
- 테스터가 다수의 테스트 에이전트를 프로비전하여 부하 테스트를 강화할 수 있습니다.
- 관리자는 테스터가 테스트하는 데 필요한 것보다 많은 VM을 얻을 수 없도록 하고 VM을 사용하지 않을 때 종료되도록 함으로써 비용을 제어할 수 있습니다.

자세한 내용은 [테스트에 DevTest Labs 사용](devtest-lab-test-env.md)을 참조하세요.

## <a name="types-of-labs"></a>랩 형식
두 가지 형식의 랩을 만들 수 있습니다. Azure Lab Services로 **관리되는 랩 형식**을 만들 수 있고 Azure Lab Services로 **랩**을 만들 수 있습니다. 랩에 필요한 것을 입력만 하고 서비스를 통해 랩에 필요한 인프라가 설정되고 관리되도록 하려면 **관리되는 랩 형식** 중 하나를 선택합니다. 현재 **클래스룸 랩**은 Azure Lab Services로 유일하게 만들 수 있는 관리되는 랩 형식입니다. 인프라를 직접 관리하려면 **Azure DevTest Labs**를 사용하여 랩을 만듭니다.

다음 섹션에서는 이러한 랩을 대한 자세한 정보를 제공합니다. 

## <a name="managed-lab-types"></a>관리되는 랩 형식
Azure Lab Services를 사용하면 Azure에서 해당 인프라를 관리하는 랩을 만들 수 있습니다. 이 문서에서는 이러한 랩을 관리되는 랩 형식이라고 합니다. 관리되는 랩 형식은 특정 요구에 맞는 다양한 형식의 랩을 제공합니다. 현재 유일하게 지원되는 관리되는 랩 형식은 **클래스룸 랩**입니다. 

관리되는 랩 형식을 사용하면 최소한의 설정만으로 바로 시작이 가능합니다. 서비스 자체는 VM 가동에서 오류 처리 및 인프라 확장에 이르기까지 랩의 모든 인프라 관리를 처리합니다. 클래스룸 랩과 같은 관리되는 랩 형식을 만들려면 먼저 조직의 랩 계정을 만들어야 합니다. 랩 계정은 조직의 모든 랩을 관리하는 중앙 계정 역할을 합니다. 

관리되는 랩 형식으로 Azure 리소스를 만들고 사용하면, 서비스를 통해 내부 Microsoft 구독에 리소스가 생성되고 관리됩니다. 그것들은 자신의 Azure 구독에서 만들어지지 않았습니다. 이 서비스는 내부 Microsoft 구독에서 이러한 리소스의 사용을 추적합니다. 이 사용량은 랩 계정이 포함된 Azure 구독에 청구됩니다.   

다음은 몇 가지 **관리되는 랩 형식의 사용 사례**입니다. 

- 학생들에게 클래스에 필요한 것으로 정확히 구성된 가상 머신 랩을 제공합니다. 각 학생에게 VM을 숙제나 개인 프로젝트에 사용하도록 제한된 시간을 줍니다.
- 계산 집약적인 또는 그래픽 집약적인 연구를 수행하기 위해 고성능 컴퓨팅 VM 풀을 설정합니다. 필요에 따라 VM을 실행하고 완료되면 컴퓨터를 정리합니다. 
- 학교의 실제 컴퓨터 랩을 클라우드로 옮깁니다. VM 수를 랩에 설정한 최대 사용량 및 비용 임계 값에 이르기 까지만 자동으로 조정합니다.  
- hackathon 호스팅용 가상 머신 랩을 신속하게 프로비전합니다. 마치면 클릭 한 번으로 랩을 삭제합니다. 


## <a name="devtest-labs"></a>DevTest Labs
자신의 구독 내 모든 인프라 및 구성을 직접 관리하려는 시나리오가 있을 수 있습니다. 이렇게 하려면 Azure Portal의 Azure DevTest Labs에서 랩을 만드는 방법을 알아봅니다. 이러한 랩인 경우 랩 계정을 만들 필요가 없습니다. 이러한 랩은 랩 계정(관리되는 랩 형식에 대해 존재함)에 표시되지 않습니다.  

다음은 몇 가지 **DevTest Labs의 사용 사례**입니다. 

- 회의에서 hackathon 또는 실습 세션을 호스트할 가상 머신 랩을 신속하게 프로비전합니다. 마치면 클릭 한 번으로 랩을 삭제합니다. 
- 애플리케이션으로 구성된 VM 풀을 만들고 팀이 버그 잡기를 위해 가상 머신을 쉽게 사용할 수 있게 합니다.  
- 개발자에게 필요한 모든 도구로 구성된 가상 머신을 제공합니다. 자동 시작 및 종료 일정을 계획하여 비용을 최소화합니다. 
- 배포의 일부로 반복적으로 테스트 컴퓨터 랩을 만듭니다. 마쳤다면 최신 비트를 테스트하고 테스트 컴퓨터를 정리합니다. 
- 규모와 성능 테스트를 위해 다르게 구성된 다양한 가상 머신 및 여러 테스트 에이전트를 설정합니다. 
- 최신 버전의 제품으로 구성된 랩을 사용하여 고객에게 교육 세션을 제공합니다. 각 고객에게 랩에서 사용할 제한된 시간을 줍니다. 


## <a name="managed-lab-types-vs-devtest-labs"></a>관리되는 랩 형식과 다음을 비교 DevTest Labs
다음 표는 Azure Lab Services에서 지원하는 두 가지 유형의 랩을 비교합니다. 

| 기능 | 관리되는 랩 형식 | DevTest Labs |
| -------- | ----------------- | ---------- |
| 랩에서 Azure 인프라 관리. |  서비스에 의한 자동 관리 | 직접 관리  |
| 인프라 문제에 대한 기본 복원력 | 서비스에 의한 자동 처리 | 직접 관리  |
| 구독 관리 | 서비스는 서비스를 지원하는 Microsoft 구독 내 리소스 할당을 처리합니다. 크기 조정은 서비스에 의해 자동 처리됩니다. | 자신의 Azure 구독으로 직접 관리합니다. 구독에 대한 자동 크기 조정 기능이 없습니다. |
| 랩 내에서 Azure Resource Manager 배포 | 사용할 수 없음 | 사용 가능 |

## <a name="next-steps"></a>다음 단계

다음 문서를 참조하세요. 

- [클래스룸 랩 정보](./classroom-labs/classroom-labs-overview.md)
- [DevTest 랩 정보](devtest-lab-overview.md)
