---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: 79459be30a5a2018dc82486a84895b1a954941bc
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66140491"
---
1. [Azure Portal](https://portal.azure.com/)에서 **모두 찾아보기** > **App Services**를 선택합니다. 그런 다음, Mobile Apps 백엔드를 선택합니다. **설정**에서 **App Service Push**를 선택합니다. 그런 다음, 알림 허브 이름을 선택합니다.
2. **Windows(WNS)** 로 이동합니다. 그런 다음, Live 서비스 사이트에서 가져온 **보안 키**(클라이언트 암호) 및 **패키지 SID**를 입력합니다. 다음으로 **저장**을 선택합니다.

    ![포털에서 WNS 키 설정](./media/app-service-mobile-configure-wns/mobile-push-wns-credentials.png)

이제 백 엔드가 WNS를 사용하여 푸시 알림을 보내도록 구성되었습니다.
