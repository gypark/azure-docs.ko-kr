---
title: '빠른 시작: Java를 사용하여 Bing Entity Search REST API에 검색 요청 보내기'
titlesuffix: Azure Cognitive Services
description: 이 빠른 시작을 사용하여 Java를 통해 Bing Entity Search REST API로 요청을 보내고 JSON 응답을 받습니다.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: a8b25252e861d707568876f75aadd6f436441f8f
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66389431"
---
# <a name="quickstart-send-a-search-request-to-the-bing-entity-search-rest-api-using-java"></a>빠른 시작: Java를 사용하여 Bing Entity Search REST API에 검색 요청 보내기

이 빠른 시작을 사용하여 Bing Entity Search API를 처음 호출하고 JSON 응답을 봅니다. 이 간단한 Java 애플리케이션은 뉴스 검색 쿼리를 API에 보내고, 응답을 표시합니다.

이 애플리케이션은 Java에서 작성되지만 API는 대부분의 프로그래밍 언어와 호환되는 RESTful 웹 서비스입니다.

## <a name="prerequisites"></a>필수 조건

* [JDK(Java Development Kit)](https://www.oracle.com/technetwork/java/javase/downloads/)
* [Gson 라이브러리](https://github.com/google/gson)


[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>프로젝트 만들기 및 초기화

1. 즐겨 찾는 IDE 또는 편집기에서 새 Java 프로젝트를 만들고 다음 라이브러리를 가져옵니다.

   ```java
   import java.io.*;
   import java.net.*;
   import java.util.*;
   import javax.net.ssl.HttpsURLConnection;
   import com.google.gson.Gson;
   import com.google.gson.GsonBuilder;
   import com.google.gson.JsonObject;
   import com.google.gson.JsonParser;
   import com.google.gson.Gson;
   import com.google.gson.GsonBuilder;
   import com.google.gson.JsonObject;
   import com.google.gson.JsonParser;
   ```

2. 새 클래스에서 API 엔드포인트, 구독 키 및 검색 쿼리에 대한 변수를 만듭니다.

   ```java
   public class EntitySearch {

      static String subscriptionKey = "ENTER KEY HERE";
    
        static String host = "https://api.cognitive.microsoft.com";
        static String path = "/bing/v7.0/entities";
    
        static String mkt = "en-US";
        static String query = "italian restaurant near me";
   //...
    
   ```

## <a name="construct-a-search-request-string"></a>검색 요청 문자열 작성

1. JSON `String`을 반환하는 `search()` 함수를 만듭니다. 검색 쿼리를 url로 인코딩한 다음, `&q=`을 사용하여 매개 변수 문자열에 추가합니다. `?mkt=`을 사용하여 문자열에 시장을 추가합니다.
 
2. 해당하는 호스트, 경로 및 매개 변수 문자열을 사용하여 URL 개체를 만듭니다.
    
    ```java
    //...
    public static String search () throws Exception {
        String encoded_query = URLEncoder.encode (query, "UTF-8");
        String params = "?mkt=" + mkt + "&q=" + encoded_query;
        URL url = new URL (host + path + params);
    //...
    ```
      
## <a name="send-a-search-request-and-receive-a-response"></a>검색 요청을 보내고 응답 수신

1. 위에서 만든 `search()` 함수에서 `url.openCOnnection()`을 사용하여 새 `HttpsURLConnection` 개체를 만듭니다. 요청 메서드를 `GET`으로 설정하고, `Ocp-Apim-Subscription-Key` 헤더에 구독 키를 추가합니다.

    ```java
    //...
    HttpsURLConnection connection = (HttpsURLConnection) url.openConnection();
    connection.setRequestMethod("GET");
    connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);
    connection.setDoOutput(true);
    //...
    ```

2. 새 `StringBuilder`를 만듭니다. API 응답을 읽을 수 있도록 `InputStreamReader`를 인스턴스화할 때 새 `BufferedReader`를 매개 변수로 사용합니다.  
    
    ```java
    //...
    StringBuilder response = new StringBuilder ();
    BufferedReader in = new BufferedReader(
        new InputStreamReader(connection.getInputStream()));
    //...
    ```

3. `BufferedReader`의 응답을 저장하는 `String` 개체를 만듭니다. 이 과정을 반복하고, 각 줄을 문자열에 추가합니다. 그런 다음, 판독기를 닫고 응답을 반환합니다. 
    
    ```java
    String line;
    
    while ((line = in.readLine()) != null) {
      response.append(line);
    }
    in.close();
    
    return response.toString();
    ```

## <a name="format-the-json-response"></a>JSON 응답 형식 지정

1. JSON 응답의 형식을 지정하는 새 `prettify` 함수를 만듭니다. 새 `JsonParser`를 만들고, json 텍스트에서 `parse()`를 호출하여 JSON 개체로 저장합니다. 

2. Gson 라이브러리를 사용하여 새 `GsonBuilder()`를 만들고, `setPrettyPrinting().create()`를 사용하여 json 형식을 지정합니다. 그리고 반환합니다.    
  
   ```java
   //...
   public static String prettify (String json_text) {
    JsonParser parser = new JsonParser();
    JsonObject json = parser.parse(json_text).getAsJsonObject();
    Gson gson = new GsonBuilder().setPrettyPrinting().create();
    return gson.toJson(json);
   }
   //...
   ```

## <a name="call-the-search-function"></a>검색 함수 호출

1. 프로젝트의 기본 메서드에서 `search()`를 호출하고, `prettify()`를 사용하여 텍스트 형식을 지정합니다.
    
    ```java
        public static void main(String[] args) {
            try {
                String response = search ();
                System.out.println (prettify (response));
            }
            catch (Exception e) {
                System.out.println (e);
            }
        }
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

* [Bing Entity Search API란?](../overview.md )
* [Bing Entity Search API 참조](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference)
