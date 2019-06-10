---
title: Bing Video Search API를 사용하여 비디오 인사이트 가져오기
titlesuffix: Azure Cognitive Services
description: Bing Video Search API를 사용하여 관련 비디오와 같은 비디오에 대한 자세한 정보를 가져오는 방법을 알아봅니다.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: scottwhi
ms.openlocfilehash: c5f3a43a6c48714566b5c53b33fe541b50d61589
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66383888"
---
# <a name="get-insights-about-a-video"></a>비디오에 대한 인사이트 가져오기

Bing Video Search API에서 반환되는 각 비디오에는 관련 비디오와 같은 자세한 정보를 가져오는 데 사용할 수 있는 비디오 ID가 포함되어 있습니다. 비디오에 대한 인사이트를 가져오려면 API 응답에서 해당 [videoId](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#video-videoid) 토큰을 가져옵니다. 

```json
    "value" : [
        {
            . . .
            "name" : "How to sail - What to Wear for Dinghy Sailing",
            "description" : "An informative video on what to wear...",
            "contentUrl" : "https:\/\/www.fabrikam.com\/watch?v=vzmPjHBZ--g",
            "videoId" : "6DB795E11A6E3CBAAD636DB795E11A6E3CBAAD63",
            . . .
        }
    ],
```

그런 다음, ID를 사용하여 GET 요청을 비디오 세부 정보 엔드포인트로 보냅니다. [id](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#id) 쿼리 매개 변수를 `videoId` 토큰으로 설정합니다. 가져올 인사이트를 지정하려면 [modules](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#modulesrequested) 쿼리 매개 변수를 설정합니다. 모든 인사이트를 가져오려면 `modules`를 All로 설정합니다. 응답에는 사용 가능한 경우 요청한 모든 인사이트가 포함됩니다.

```cURL
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/details?q=sailiing+dinghies&id=6DB795E11A6E3CBAAD636DB795E11A6E3CBAAD63&modules=All&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
``` 

## <a name="getting-related-videos-insights"></a>관련 비디오 인사이트 가져오기  

지정한 비디오와 관련된 비디오를 가져오려면 [modules](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#modulesrequested) 쿼리 매개 변수를 `RelatedVideos`로 설정합니다.
  
```cURL  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/details?q=sailiing+dinghies&id=6DB795E11A6E3CBAAD636DB795E11A6E3CBAAD63&modules=RelatedVideos&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

이 요청에 대한 응답에는 [Videos](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#videos) 개체 대신 최상위 수준의 [VideoDetails](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#videodetails) 개체가 있습니다.  
  
```json
{
    "_type" : "Api.VideoDetails.VideoDetails",
    "relatedVideos" : {
        "value" : [
            {
                "name" : "How to sail - Reefing a Sail",
                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=7284B07...",
                "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?id=OVP.zt...",
                "datePublished" : "2014-03-04T16:11:09",
                "publisher" : [
                    {
                        "name" : "Fabrikam"
                    }
                ],
                "contentUrl" : "https:\/\/www.fabrikam.com\/watch?v=...",
                "hostPageUrl" : "https:\/\/www.bing.com\/cr?IG=7284B07...",
                "hostPageDisplayUrl" : "https:\/\/www.fabrikam.com\/watch?...",
                "duration" : "PT4M56S",
                "motionThumbnailUrl" : "https:\/\/tse1.mm.bing.net\/th?id=OM...",
                "allowHttpsEmbed" : true,
                "viewCount" : 21756,
                "videoId" : "AC1A157A4DDB571D03D6AC157A4DDB571D03D6",
                "allowMobileEmbed" : false,
                "isSuperfresh" : false
            },
            . . .
        ]
    }
}
```

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [인기 비디오 검색](trending-videos.md)

