---
title: '빠른 시작: C#을 사용하여 뉴스 검색 수행 - Bing News Search REST API'
titlesuffix: Azure Cognitive Services
description: 이 빠른 시작을 사용하여 C#을 통해 Bing News Search REST API로 요청을 보내고 JSON 응답을 받습니다.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: quickstart
ms.date: 1/10/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 87e328230b346deaa25a1d83ca46c1698dd84882
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65798354"
---
# <a name="quickstart-search-for-news-using-c-and-the-bing-news-search-rest-api"></a>빠른 시작: C# 및 Bing News Search REST API를 사용하여 뉴스 검색

이 빠른 시작을 사용하여 Bing News Search API를 처음 호출하고 JSON 응답을 봅니다. 이 간단한 C# 애플리케이션은 뉴스 검색 쿼리를 API에 보내고, 응답을 표시합니다. 이 샘플에 대한 전체 코드는 [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Search/BingNewsSearchv7.cs)에서 확인할 수 있습니다.

이 애플리케이션은 C#에서 작성되지만 API는 대부분의 프로그래밍 언어와 호환되는 RESTful 웹 서비스입니다.

## <a name="prerequisites"></a>필수 조건

* [Visual Studio 2017 이상](https://www.visualstudio.com/downloads/)의 모든 버전.
* NuGet 패키지로 사용 가능한 [Json.NET](https://www.newtonsoft.com/json) 프레임워크.
* Linux/MacOS를 사용하는 경우 이 애플리케이션은 [Mono](https://www.mono-project.com/)를 사용하여 실행할 수 있습니다.

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../includes/cognitive-services-bing-news-search-signup-requirements.md)]

[Cognitive Services 가격 책정 - Bing Search API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/)도 참조하세요.

## <a name="create-and-initialize-a-project"></a>프로젝트 만들기 및 초기화

1. Visual Studio에서 새 C# 콘솔 솔루션을 만듭니다. 그런 다음, 주 코드 파일에 다음 네임스페이스를 추가합니다.
    
    ```csharp
    using System;
    using System.Text;
    using System.Net;
    using System.IO;
    using System.Collections.Generic;
    ```

2. API 엔드포인트, 구독 키 및 검색 용어에 대한 변수를 만듭니다.

    ```csharp
    const string accessKey = "enter key here";
    const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/news/search";
    const string searchTerm = "Microsoft";
    ```
   ## <a name="create-a-struct-to-format-the-bing-news-search-response"></a>구조체를 만들어 Bing News Search 응답의 형식 지정

1. 이미지 검색 결과 및 JSON 헤더 정보를 포함할 `SearchResult` 구조체를 정의합니다.

    ```csharp
    struct SearchResult
    {
        public String jsonResult;
        public Dictionary<String, String> relevantHeaders;
    }
    ```

## <a name="create-and-handle-a-news-search-request"></a>뉴스 검색 요청 만들기 및 처리

`BingNewsSearch` 메서드를 만들어 API 호출을 수행하고 반환 형식을 이전에 만든 `SearchResult` 구조체로 설정합니다. 메서드에서 다음 단계를 수행합니다.

1. 검색 요청의 URI를 생성합니다. 검색 용어 `toSearch`는 문자열에 추가되기 전에 형식이 지정되어야 합니다.

    ```csharp
    static SearchResult BingNewsSearch(string toSearch){

        var uriQuery = uriBase + "?q=" + Uri.EscapeDataString(toSearch);
    //...
    ```

2. 웹 요청을 수행하고 응답을 JSON 문자열로 가져옵니다.

    ```csharp
    WebRequest request = WebRequest.Create(uriQuery);
    request.Headers["Ocp-Apim-Subscription-Key"] = subscriptionKey;
    HttpWebResponse response = (HttpWebResponse)request.GetResponseAsync().Result;
    string json = new StreamReader(response.GetResponseStream()).ReadToEnd();
    ```

3. 검색 결과 개체를 만들고 Bing HTTP 헤더를 추출합니다. 그런 다음, `searchResult`을 반환합니다.

    ```csharp
    // Create the result object for return
    var searchResult = new SearchResult()
    {
        jsonResult = json,
        relevantHeaders = new Dictionary<String, String>()
    };

    // Extract Bing HTTP headers
    foreach (String header in response.Headers)
    {
        if (header.StartsWith("BingAPIs-") || header.StartsWith("X-MSEdge-"))
            searchResult.relevantHeaders[header] = response.Headers[header];
    }
    return searchResult;
    ```

3. 검색 결과 개체를 만들고 Bing HTTP 헤더를 추출합니다. 그런 다음, `searchResult`을 반환합니다.

    ```csharp
    // Create the result object for return
    var searchResult = new SearchResult()
    {
        jsonResult = json,
        relevantHeaders = new Dictionary<String, String>()
    };

    // Extract Bing HTTP headers
    foreach (String header in response.Headers)
    {
        if (header.StartsWith("BingAPIs-") || header.StartsWith("X-MSEdge-"))
            searchResult.relevantHeaders[header] = response.Headers[header];
    }
    return searchResult;
    ```

## <a name="process-the-response"></a>응답 처리

1. 주 메서드에서 `BingNewsSearch()`를 호출하고 반환된 응답을 저장합니다. 그런 다음, JSON을 개체로 deserialize합니다. 그런 다음, 응답 값을 볼 수 있습니다.

    ```csharp
    SearchResult result = BingNewsSearch(searchTerm);
    //deserialize the JSON response
    dynamic jsonObj = Newtonsoft.Json.JsonConvert.DeserializeObject(result.jsonResult);
    Console.WriteLine(jsonObj["value"][0])
    ```

## <a name="json-response"></a>JSON 응답

성공한 응답은 다음 예제와 같이 JSON으로 반환됩니다.

```json
{
   "_type": "News",
   "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/news\/search?q=Microsoft",
   "totalEstimatedMatches": 36,
   "sort": [
      {
         "name": "Best match",
         "id": "relevance",
         "isSelected": true,
         "url": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/news\/search?q=Microsoft"
      },
      {
         "name": "Most recent",
         "id": "date",
         "isSelected": false,
         "url": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/news\/search?q=Microsoft&sortby=date"
      }
   ],
   "value": [
      {
         "name": "Microsoft to open flagship London brick-and-mortar retail store",
         "url": "http:\/\/www.contoso.com\/article\/microsoft-to-open-flagshi...",
         "image": {
            "thumbnail": {
               "contentUrl": "https:\/\/www.bing.com\/th?id=ON.F9E4A49EC010417...",
               "width": 220,
               "height": 146
            }
         },
         "description": "After years of rumors about Microsoft opening a brick-and-mortar...", 
         "about": [
           {
             "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entiti...", 
             "name": "Microsoft"
           }, 
           {
             "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entit...", 
             "name": "London"
           }
         ], 
         "provider": [
           {
             "_type": "Organization", 
             "name": "Contoso"
           }
         ], 
          "datePublished": "2017-09-21T21:16:00.0000000Z", 
          "category": "ScienceAndTechnology"
      }, 

      . . .

      {
         "name": "Microsoft adds Availability Zones to its Azure cloud platform",
         "url": "https:\/\/contoso.com\/2017\/09\/21\/microsoft-adds-availability...",
         "image": {
            "thumbnail": {
               "contentUrl": "https:\/\/www.bing.com\/th?id=ON.0AE7595B9720...",
               "width": 700,
               "height": 466
            }
         },
         "description": "Microsoft has begun adding Availability Zones to its...",
         "about": [
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/a093e9b...",
               "name": "Microsoft"
            },
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/cf3abf7d-e379-...",
               "name": "Windows Azure"
            },
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/9cdd061c-1fae-d0...",
               "name": "Cloud"
            }
         ],
         "provider": [
            {
               "_type": "Organization",
               "name": "Contoso"
            }
         ],
         "datePublished": "2017-09-21T09:01:00.0000000Z",
         "category": "ScienceAndTechnology"
      }
   ]
}
```

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [단일 페이지 웹앱 만들기](tutorial-bing-news-search-single-page-app.md)
