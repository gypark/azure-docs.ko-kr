---
title: '빠른 시작: Ruby를 사용하여 Bing Entity Search REST API에 검색 요청 보내기'
titlesuffix: Azure Cognitive Services
description: 이 빠른 시작을 사용하여 Ruby를 통해 Bing Entity Search REST API로 요청을 보내고 JSON 응답을 받습니다.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: b5653ffbfeb22bc59c48dd92b558178fcd89b2de
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66384511"
---
# <a name="quickstart-for-bing-entity-search-api-with-ruby"></a>빠른 시작: Ruby를 통해 Bing Entity Search API 사용

이 빠른 시작을 사용하여 Bing Entity Search API를 처음 호출하고 JSON 응답을 봅니다. 이 간단한 Ruby 애플리케이션은 뉴스 검색 쿼리를 API에 보내고, 응답을 표시합니다. 이 애플리케이션의 소스 코드는 [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/ruby/Search/BingEntitySearchv7.rb)에 제공됩니다.

이 애플리케이션은 Ruby에서 작성되지만 API는 대부분의 프로그래밍 언어와 호환되는 RESTful 웹 서비스입니다.

## <a name="prerequisites"></a>필수 조건

* [Ruby 2.4](https://www.ruby-lang.org/en/downloads/) 이상.

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>애플리케이션 만들기 및 초기화

1. 즐겨 사용하는 IDE 또는 코드 편집기에서 뉴스 Ruby 파일을 만들고 다음과 같은 패키지를 가져옵니다.

    ```ruby
    require 'net/https'
    require 'cgi'
    require 'json'
    ```

2. API 엔드포인트, News Search URL, 구독 키 및 검색 쿼리에 대한 변수를 만듭니다.
    
    ```ruby
    host = 'https://api.cognitive.microsoft.com'
    path = '/bing/v7.0/entities'
    
    mkt = 'en-US'
    query = 'italian restaurants near me'
    ```

## <a name="format-and-make-an-api-request"></a>API 요청 형식 지정 및 수행

1. `?mkt=` 매개 변수에 시장 변수를 추가하여 요청에 대한 매개 변수 문자열을 만듭니다. 쿼리를 인코딩하고 `&q=` 매개 변수에 추가합니다. 요청의 API 호스트, 경로 및 매개 변수를 결합하고 URI 개체로 캐스팅합니다.

    ```ruby
    params = '?mkt=' + mkt + '&q=' + CGI.escape(query)
    uri = URI (host + path + params)
    ```

2. 마지막 단계의 변수를 사용하여 요청을 만듭니다. `Ocp-Apim-Subscription-Key` 헤더에 구독 키를 추가합니다.

    ```ruby
    request = Net::HTTP::Get.new(uri)
    request['Ocp-Apim-Subscription-Key'] = subscriptionKey
    ```

3. 요청 보내기 및 응답 출력

    ```ruby
    response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
        http.request (request)
    end

    puts JSON::pretty_generate (JSON (response.body))
    ```

## <a name="example-json-response"></a>예제 JSON 응답

성공한 응답은 다음 예제와 같이 JSON으로 반환됩니다. 

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "italian restaurant near me",
    "askUserForLocation": true
  },
  "places": {
    "value": [
      {
        "_type": "LocalBusiness",
        "webSearchUrl": "https://www.bing.com/search?q=sinful+bakery&filters=local...",
        "name": "Liberty's Delightful Sinful Bakery & Cafe",
        "url": "https://www.contoso.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98112",
          "addressCountry": "US",
          "neighborhood": "Madison Park"
        },
        "telephone": "(800) 555-1212"
      },

      . . .
      {
        "_type": "Restaurant",
        "webSearchUrl": "https://www.bing.com/search?q=Pickles+and+Preserves...",
        "name": "Munson's Pickles and Preserves Farm",
        "url": "https://www.princi.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness",
            "Restaurant"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98101",
          "addressCountry": "US",
          "neighborhood": "Capitol Hill"
        },
        "telephone": "(800) 555-1212"
      },
      
      . . .
    ]
  }
}
```

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [단일 페이지 웹앱 빌드](../tutorial-bing-entities-search-single-page-app.md)

* [Bing Entity Search API란?](../search-the-web.md)
* [Bing Entity Search API 참조](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference)
