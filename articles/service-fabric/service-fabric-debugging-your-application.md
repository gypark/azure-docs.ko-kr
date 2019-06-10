---
title: Visual Studio에서 애플리케이션 디버그 | Microsoft Docs
description: 로컬 개발 클러스터의 Visual Studio에서 개발하고 디버그하여 서비스의 안정성과 성능을 향상시킵니다.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: cb888532-bcdb-4e47-95e4-bfbb1f644da4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.custom: vs-azure
ms.workload: azure-vs
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 682059914b5d86f5e670e373a4acf3e4ac6246ba
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66428219"
---
# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a>Visual Studio를 사용하여 Service Fabric 애플리케이션 디버그
> [!div class="op_single_selector"]
> * [Visual Studio/CSharp](service-fabric-debugging-your-application.md) 
> * [Eclipse/Java](service-fabric-debugging-your-application-java.md)
>


## <a name="debug-a-local-service-fabric-application"></a>로컬 Service Fabric 애플리케이션 디버깅
로컬 컴퓨터 개발 클러스터에서 Azure Service Fabric 애플리케이션을 배포하고 디버그하여 시간과 비용을 절약할 수 있습니다. Visual Studio 2019 또는 2015 로컬 클러스터에 응용 프로그램을 배포 하 고 자동으로 응용 프로그램의 모든 인스턴스에 디버거를 연결할 수 있습니다. Visual Studio의 디버거를 연결 하려면 관리자 권한으로 실행 되어야 합니다.

1. [서비스 패브릭 개발 환경 설정](service-fabric-get-started.md)의 단계를 따라 로컬 개발 클러스터를 시작합니다.
2. **F5** 키를 누르거나 **디버그** > **디버깅 시작**을 클릭합니다.
   
    ![애플리케이션 디버깅 시작][startdebugging]
3. 코드의 중단점을 설정하고 **디버그** 메뉴의 명령을 클릭하여 애플리케이션의 단계를 진행합니다.
   
   > [!NOTE]
   > Visual Studio는 애플리케이션의 모든 인스턴스에 연결합니다. 단계별로 코드를 실행하는 동안 중단점은 동시 세션에서 발생하는 여러 프로세스에 히트될 수 있습니다. 스레드 ID의 각 중단점을 조건부로 생성하거나 진단 이벤트를 사용하여 히트된 후에 중단점을 사용하지 않도록 설정합니다.
   > 
   > 
4. **진단 이벤트** 창이 자동으로 열려서 실시간으로 진단 이벤트를 볼 수 있습니다.
   
    ![실시간으로 진단 이벤트 보기][diagnosticevents]
5. 클라우드 탐색기의 **진단 이벤트** 창도 열 수 있습니다.  **Service Fabric**에서 아무 노드를 마우스 오른쪽 단추로 클릭하고 **스트리밍 추적 보기**를 선택합니다.
   
    ![진단 이벤트 창 열기][viewdiagnosticevents]
   
    특정 서비스 또는 응용 프로그램 추적을 필터링 하려는 경우에 해당 특정 서비스 또는 응용 프로그램에서 스트리밍 추적을 사용 하도록 설정 합니다.
6. 진단 이벤트는 자동으로 생성되는 **ServiceEventSource.cs** 파일에서 볼 수 있으며 애플리케이션 코드에서 호출됩니다.
   
    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```
7. **진단 이벤트** 창은 필터링, 일시 중지 및 실시간 이벤트 검사를 지원합니다.  필터는 해당 콘텐츠를 포함하는 이벤트 메시지의 단순 문자열 검색입니다.
   
    ![실시간으로 이벤트를 필터링, 일시 중지, 다시 시작 또는 검사합니다.][diagnosticeventsactions]
8. 서비스 디버깅은 다른 모든 애플리케이션의 디버깅과 같습니다. 일반적으로 손쉬운 디버깅을 위해 Visual Studio를 통해 중단점을 설정할 수 있습니다. 신뢰할 수 있는 컬렉션은 여러 노드에 걸쳐 복제하더라도 여전히 IEnumerable을 구현합니다. 이 구현 내에서 저장 한 참조를 디버깅 하는 동안 Visual Studio에서 결과 보기를 사용할 수 있음을 의미 합니다. 이렇게 하려면 중단점을 설정할 어디서 나 코드에서.
   
    ![애플리케이션 디버깅 시작][breakpoint]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a>원격 Service Fabric 애플리케이션 디버깅
Service Fabric 응용 프로그램을 Azure에서 Service Fabric 클러스터에서 실행 하는 경우에 Visual Studio에서 직접 이러한 응용 프로그램을 원격으로 디버깅할 수 있습니다.

> [!NOTE]
> 이 기능은 [Service Fabric SDK 2.0](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) 및 [Azure SDK for .NET 2.9](https://azure.microsoft.com/downloads/)가 필요합니다.    
> 
> 

<!-- -->
> [!WARNING]
> 원격 디버깅은 실행 중인 애플리케이션에 영향을 미치기 때문에 개발/테스트 시나리오를 위한 것이며 프로덕션 환경에서는 사용되지 않습니다.
> 
> 

1. 클러스터로 이동할 **클라우드 탐색기**합니다. 마우스 오른쪽 단추로 클릭 하 고 선택 **디버깅 사용**
   
    ![원격 디버깅 사용][enableremotedebugging]
   
    이 작업은 필요한 네트워크 구성을 확인 하 고 클러스터 노드에서 원격 디버깅 확장을 사용 하는 과정을 시작 합니다.
2. **Cloud Explorer**에 있는 클러스터 노드를 마우스 오른쪽 단추로 클릭하고 **디버거 연결**을 선택합니다.
   
    ![디버거 연결][attachdebugger]
3. **프로세스에 연결** 대화 상자에서 디버깅하려는 프로세스를 선택하고 **연결**을 클릭하세요.
   
    ![프로세스 선택][chooseprocess]
   
    연결하려는 프로세스의 이름은 서비스 프로젝트 어셈블리 이름과 일치합니다.
   
    디버거는 프로세스를 실행하는 모든 노드에 연결됩니다.
   
   * 상태 비저장 서비스를 디버깅 하는 경우에서 모든 노드에서 서비스의 모든 인스턴스를 사용 하면 디버그 세션에 속합니다.
   * 상태 저장 서비스를 디버깅 하는 경우에 모든 파티션의 주 복제본에만 활성 상태이 고 따라서 디버거에 의해 검색 됩니다. 디버그 세션 중에 주 복제본이 이동한 경우, 해당 복제본의 처리는 여전히 디버그 세션의 일부입니다.
   * 관련 파티션 또는 지정된 된 서비스의 인스턴스를만 catch 할 특정 파티션 또는 인스턴스만 에서만 중단 하려면 조건부 중단점을 사용할 수 있습니다.
     
     ![조건부 중단점][conditionalbreakpoint]
     
     > [!NOTE]
     > 현재 동일한 서비스 실행 파일 이름의 인스턴스가 다수인 패브릭 클러스터 디버깅을 지원하지 않습니다.
     > 
     > 
4. 애플리케이션 디버깅을 완료하면 **클라우드 탐색기**에 있는 클러스터를 마우스 오른쪽 단추로 클릭하여 **디버깅 사용 안 함**을 선택하여 원격 디버깅 확장을 사용하지 않을 수 있습니다.
   
    ![원격 디버깅 사용 안 함][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a>원격 클러스터 노드에서 스트리밍 추적
사용자는 또한 수 스트림 추적에 Visual Studio로 원격 클러스터 노드에서 직접입니다. 이 기능을 통해 Service Fabric 클러스터 노드에서 생성된 ETW 추적 이벤트를 스트리밍할 수 있습니다.

> [!NOTE]
> 이 기능은 [Service Fabric SDK 2.0](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) 및 [Azure SDK for .NET 2.9](https://azure.microsoft.com/downloads/)가 필요합니다.
> 이 기능은 Azure에서 실행되는 클러스터만 지원합니다.
> 
> 

<!-- -->
> [!WARNING]
> 스트리밍 추적은 실행 중인 애플리케이션에 영향을 미치기 때문에 개발/테스트 시나리오를 위한 것이며 프로덕션 환경에서는 사용되지 않습니다.
> 프로덕션 시나리오에서는 Azure Diagnostics를 통해 이벤트 전달을 사용해야 합니다.
> 
> 

1. 클러스터로 이동할 **클라우드 탐색기**합니다. 마우스 오른쪽 단추로 클릭 하 고 선택 **스트리밍 추적 사용**
   
    ![원격 스트리밍 추적 사용][enablestreamingtraces]
   
    이 작업에는 필수 네트워크 구성 뿐만 아니라 클러스터 노드에서 스트리밍 추적 확장을 사용 하도록 설정 하는 프로세스 시작 합니다.
2. **Cloud Explorer**에 있는 **노드** 요소를 확장하고, 추적을 스트리밍하려는 노드를 마우스 오른쪽 단추로 클릭하고 **스트리밍 추적 보기**를 선택합니다.
   
    ![원격 스트리밍 추적 보기][viewremotestreamingtraces]
   
    추적을 보려는 노드 개수만큼 2단계를 반복합니다. 각 노드 스트림은 전용 창에 표시됩니다.
   
    이제 서비스 패브릭에서 내보낸 추적과 서비스를 볼 수 있습니다. 특정 애플리케이션에만 표시하도록 이벤트를 필터링하려는 경우, 필터에서 애플리케이션의 이름만 간단히 입력하면 됩니다.
   
    ![스트리밍 추적 보기][viewingstreamingtraces]
3. 클러스터에서 스트리밍 추적을 완료하면 **Cloud Explorer**에 있는 클러스터를 마우스 오른쪽 단추로 클릭하고 **스트리밍 추적 사용 안 함**을 선택하여 원격 스트리밍 추적을 사용하지 않을 수 있습니다.
   
    ![원격 스트리밍 추적 사용 안 함][disablestreamingtraces]

## <a name="next-steps"></a>다음 단계
* [서비스 패브릭 서비스 테스트](service-fabric-testability-overview.md)
* [Visual Studio에서 서비스 패브릭 애플리케이션 관리](service-fabric-manage-application-in-visual-studio.md)

<!--Image references-->
[startdebugging]: ./media/service-fabric-debugging-your-application/startdebugging.png
[diagnosticevents]: ./media/service-fabric-debugging-your-application/diagnosticevents.png
[viewdiagnosticevents]: ./media/service-fabric-debugging-your-application/viewdiagnosticevents.png
[diagnosticeventsactions]: ./media/service-fabric-debugging-your-application/diagnosticeventsactions.png
[breakpoint]: ./media/service-fabric-debugging-your-application/breakpoint.png
[enableremotedebugging]: ./media/service-fabric-debugging-your-application/enableremotedebugging.png
[attachdebugger]: ./media/service-fabric-debugging-your-application/attachdebugger.png
[chooseprocess]: ./media/service-fabric-debugging-your-application/chooseprocess.png
[conditionalbreakpoint]: ./media/service-fabric-debugging-your-application/conditionalbreakpoint.png
[disableremotedebugging]: ./media/service-fabric-debugging-your-application/disableremotedebugging.png
[enablestreamingtraces]: ./media/service-fabric-debugging-your-application/enablestreamingtraces.png
[viewingstreamingtraces]: ./media/service-fabric-debugging-your-application/viewingstreamingtraces.png
[viewremotestreamingtraces]: ./media/service-fabric-debugging-your-application/viewremotestreamingtraces.png
[disablestreamingtraces]: ./media/service-fabric-debugging-your-application/disablestreamingtraces.png
