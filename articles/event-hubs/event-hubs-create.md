---
title: Azure 빠른 시작 - Azure Portal을 사용하여 이벤트 허브 만들기 | Microsoft Docs
description: 이 빠른 시작에서는 Azure Portal을 사용하여 Azure 이벤트 허브를 만든 다음, .NET 표준 SDK를 사용하여 이벤트를 보내고 받는 방법을 알아봅니다.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: quickstart
ms.custom: mvc
ms.date: 01/23/2019
ms.author: shvija
ms.openlocfilehash: 83e33ffa2854b92718828ae870b82431993fac24
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65603523"
---
# <a name="quickstart-create-an-event-hub-using-azure-portal"></a>빠른 시작: Azure Portal을 사용하여 이벤트 허브 만들기
Azure Event Hubs는 초당 수백만 개의 이벤트를 수신하여 처리할 수 있는 빅 데이터 스트리밍 플랫폼이자 이벤트 수집 서비스입니다. Event Hubs는 분산된 소프트웨어와 장치에서 생성된 이벤트, 데이터 또는 원격 분석을 처리하고 저장할 수 있습니다. Event Hub로 전송된 데이터는 실시간 분석 공급자 또는 일괄 처리/저장소 어댑터를 사용하여 변환하고 저장할 수 있습니다. Event Hubs에 대한 자세한 개요는 [Event Hubs 개요](event-hubs-about.md) 및 [Event Hubs 기능](event-hubs-features.md)을 참조하세요.

Azure 빠른 시작에서 [Azure Portal](https://portal.azure.com)을 사용하여 이벤트 허브를 만듭니다.

## <a name="prerequisites"></a>필수 조건

이 빠른 시작을 완료하려면 다음 항목이 있어야 합니다.

- 동작합니다. 구독이 없으면 시작하기 전에 [계정을 만드세요](https://azure.microsoft.com/free/).
- [Visual Studio 2019)](https://www.visualstudio.com/vs) 이상.
- [.NET Standard SDK](https://www.microsoft.com/net/download/windows) 버전 2.0 이상

## <a name="create-a-resource-group"></a>리소스 그룹 만들기

리소스 그룹은 Azure 리소스에 대한 논리적 컬렉션입니다. 모든 리소스는 리소스 그룹에서 배포 및 관리됩니다. 리소스 그룹을 만들려면

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.
2. 왼쪽 탐색에서 **리소스 그룹**을 클릭합니다. 그런 다음, **추가**를 클릭합니다.

   ![리소스 그룹 - 추가 단추](./media/event-hubs-quickstart-portal/resource-groups1.png)

2. **구독**에서 리소스 그룹을 만들려는 Azure 구독의 이름을 선택합니다.
3. 고유한 **리소스 그룹 이름**을 입력합니다. 시스템에서 즉시 이 이름이 현재 선택한 Azure 구독에서 사용 가능한지 확인합니다.
4. 리소스 그룹의 **Azure 지역**을 선택합니다.
5. **검토 + 만들기**를 선택합니다.

   ![리소스 그룹 - 만들기](./media/event-hubs-quickstart-portal/resource-groups2.png)
6. **검토 + 만들기** 창에서 **만들기**를 선택합니다. 

## <a name="create-an-event-hubs-namespace"></a>Event Hubs 네임스페이스 만들기

Event Hubs 네임스페이스는 정규화된 도메인 이름으로 참조되는 고유한 범위 지정 컨테이너를 제공하며, 하나 이상의 이벤트 허브를 만듭니다. 포털을 사용하여 리소스 그룹에 네임스페이스를 만들려면 다음 작업을 수행합니다.

1. Azure Portal 화면의 왼쪽 위에서 **리소스 만들기**를 클릭합니다.
2. 왼쪽 메뉴에서 **모든 서비스**를 선택하고, **분석** 범주의 **Event Hubs** 옆에서 **별표(`*`)** 를 선택합니다. 왼쪽 탐색 메뉴에서 **Event Hubs**가 **즐겨찾기**에 추가되었는지 확인합니다. 
    
   ![Event Hubs 검색](./media/event-hubs-quickstart-portal/select-event-hubs-menu.png)
3. 왼쪽 탐색 메뉴의 **즐겨찾기** 아래에서 **Event Hubs**를 선택하고, 도구 모음에서 **추가**를 선택합니다.

   ![추가 도구 모음 단추](./media/event-hubs-quickstart-portal/event-hubs-add-toolbar.png)
4. **네임스페이스 만들기** 페이지에서 다음 단계를 수행합니다.
    1. 네임스페이스의 이름을 입력합니다. 시스템에서 사용 가능한 이름인지 즉시 확인합니다.
    2. 가격 책정 계층(기본 또는 표준)을 선택합니다.
    3. 네임스페이스를 만들려는 **구독**을 선택합니다.
    4. 네임스페이스의 **위치**를 선택합니다.
    5. **만들기**를 선택합니다. 시스템에서 리소스를 완전히 프로비전하기까지 몇 분 동안 기다려야 할 수 있습니다.

       ![이벤트 허브 네임스페이스 만들기](./media/event-hubs-quickstart-portal/create-event-hub1.png)
5. **Event Hubs** 페이지를 새로 고침하여 이벤트 허브 네임스페이스를 봅니다. 경고에서 이벤트 허브 만들기의 상태를 확인할 수 있습니다. 

    ![이벤트 허브 네임스페이스 만들기](./media/event-hubs-quickstart-portal/event-hubs-refresh.png)
6. 네임스페이스를 선택합니다. 포털에 **Event Hubs 네임스페이스** 홈페이지가 표시됩니다. 

   ![네임스페이스의 홈페이지](./media/event-hubs-quickstart-portal/namespace-home-page.png)
    
## <a name="create-an-event-hub"></a>이벤트 허브 만들기

네임스페이스에 이벤트 허브를 만들려면 다음 작업을 수행합니다.

1. Event Hubs 네임스페이스 페이지의 왼쪽 메뉴에서 **Event Hubs**를 선택합니다.
1. 창의 위쪽에서 **+ 이벤트 허브**를 클릭합니다.
   
    ![이벤트 허브 추가 - 버튼](./media/event-hubs-quickstart-portal/create-event-hub4.png)
1. 이벤트 허브의 이름을 입력한 다음, **만들기**를 클릭합니다.
   
    ![이벤트 허브 만들기](./media/event-hubs-quickstart-portal/create-event-hub5.png)
4. 경고에서 이벤트 허브 만들기의 상태를 확인할 수 있습니다. 이벤트 허브가 만들어지면 다음 이미지처럼 이벤트 허브 목록에 표시됩니다.

    ![생성된 이벤트 허브](./media/event-hubs-quickstart-portal/event-hub-created.png)

축하합니다! 포털을 사용하여 Event Hubs 네임스페이스와, 그 네임스페이스 안에 이벤트 허브를 만들었습니다. 

## <a name="next-steps"></a>다음 단계

이 문서에서는 리소스 그룹, Event Hubs 네임스페이스 및 이벤트 허브를 만들었습니다. 이벤트 허브에서 이벤트를 보내거나 받기 위한 단계별 지침은 **이벤트 보내기 및 받기** 자습서를 참조하세요. 

- [.NET Core](event-hubs-dotnet-standard-getstarted-send.md)
- [.NET Framework](event-hubs-dotnet-framework-getstarted-send.md)
- [Java](event-hubs-java-get-started-send.md)
- [Python](event-hubs-python-get-started-send.md)
- [Node.JS](event-hubs-node-get-started-send.md)
- [Go](event-hubs-go-get-started-send.md)
- [C(보내기 전용)](event-hubs-c-getstarted-send.md)
- [Apache Storm(받기 전용)](event-hubs-storm-getstarted-receive.md)


[Azure portal]: https://portal.azure.com/
[3]: ./media/event-hubs-quickstart-portal/sender1.png
[4]: ./media/event-hubs-quickstart-portal/receiver1.png
