---
title: Azure Media Services의 스트리밍 정책 | Microsoft Docs
description: 이 문서에서는 스트리밍 정책이란 무엇이며 Azure Media Services에서 스트리밍 정책을 사용하는 방법을 설명합니다.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/28/2019
ms.author: juliako
ms.openlocfilehash: a813c77e81e51bfe13e75ed6c8d0e24b4d0fa645
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66392916"
---
# <a name="streaming-policies"></a>스트리밍 정책

Azure Media Services v3에서 [스트리밍 정책](https://docs.microsoft.com/rest/api/media/streamingpolicies)을 사용하여 [스트리밍 로케이터](streaming-locators-concept.md)의 스트리밍 프로토콜 및 암호화 옵션을 정의할 수 있습니다. Media Services v3에서는 일부 미리 정의 된 스트리밍 정책 평가판 또는 프로덕션에 직접 사용할 수 있도록 제공 합니다. 

현재 사용할 수 있는 미리 정의 된 스트리밍 정책:<br/>
* 'Predefined_DownloadOnly'
* 'Predefined_ClearStreamingOnly'
* 'Predefined_DownloadAndClearStreaming'
* 'Predefined_ClearKey'
* 'Predefined_MultiDrmCencStreaming' 
* 'Predefined_MultiDrmStreaming'

다음 "의사 결정 트리" 시나리오에 대 한 미리 정의 된 스트리밍 정책의 방법을 선택할 수 있습니다.

> [!IMPORTANT]
> * 날짜/시간 형식의 **스트리밍 정책** 속성은 언제나 UTC 형식입니다.
> * Media Services 계정에 대한 제한된 정책 세트를 설계하고 동일한 옵션이 필요할 때마다 스트리밍 로케이터에 해당 세트를 다시 사용하는 것이 좋습니다. 자세한 내용은 [할당량 및 제한 사항](limits-quotas-constraints.md)을 참조하세요.

## <a name="decision-tree"></a>의사 결정 트리

전체 크기로 보려면 이미지를 클릭합니다.  

<a href="./media/streaming-policy/large.png" target="_blank"><img src="./media/streaming-policy/large.png"></a> 

생성 해야 하는 콘텐츠를 암호화 하는 경우는 [콘텐츠 키 정책](content-key-policy-concept.md)의 **콘텐츠 키 정책** clear 스트리밍 또는 다운로드 필요 하지 않습니다. 

할 수 있습니다 (예를 들어, 사용자 지정 키 배달 서비스를 사용 하는 데 필요한 다른 프로토콜을 지정 하려면 있거나 필요한 경우 지우기 오디오 트랙을 사용 하려면) 특별 한 요구 사항이 있는 경우 [만들](https://docs.microsoft.com/rest/api/media/streamingpolicies/create) 사용자 지정 스트리밍 정책입니다. 

## <a name="get-a-streaming-policy-definition"></a>스트리밍 정책 정의 가져오기  

사용 하 여 스트리밍 정책을 정의 하려는 경우 [가져올](https://docs.microsoft.com/rest/api/media/streamingpolicies/get) 정책 이름을 지정 합니다. 예를 들면 다음과 같습니다.

### <a name="rest"></a>REST (영문)

요청:

```
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Media/mediaServices/contosomedia/streamingPolicies/clearStreamingPolicy?api-version=2018-07-01
```

응답:

```
{
  "name": "clearStreamingPolicy",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Media/mediaservices/contosomedia/streamingPolicies/clearStreamingPolicy",
  "type": "Microsoft.Media/mediaservices/streamingPolicies",
  "properties": {
    "created": "2018-08-08T18:29:30.8501486Z",
    "noEncryption": {
      "enabledProtocols": {
        "download": true,
        "dash": true,
        "hls": true,
        "smoothStreaming": true
      }
    }
  }
}
```

## <a name="filtering-ordering-paging"></a>필터링, 정렬, 페이징

[Media Services 엔터티 필터링, 순서 지정, 페이징](entities-overview.md)을 참조하세요.

## <a name="next-steps"></a>다음 단계

* [파일 스트리밍](stream-files-dotnet-quickstart.md)
* [AES-128 동적 암호화 및 키 전달 서비스 사용](protect-with-aes128.md)
* [DRM 동적 암호화 및 라이선스 배달 서비스 사용](protect-with-drm.md)
