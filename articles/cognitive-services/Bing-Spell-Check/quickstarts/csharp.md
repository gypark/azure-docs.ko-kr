---
title: '빠른 시작: Bing Spell Check REST API 및 C#으로 맞춤법 검사'
titlesuffix: Azure Cognitive Services
description: Bing Spell Check REST API를 사용하여 맞춤법 및 문법 검사를 시작합니다.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 04/11/2019
ms.author: aahi
ms.openlocfilehash: e7a1f2572296015aac2d05b36b9b659c85586ff9
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66390255"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-rest-api-and-c"></a>빠른 시작: Bing Spell Check REST API 및 C#으로 맞춤법 검사

이 빠른 시작을 사용하여 Bing Spell Check REST API에 대한 첫 번째 호출을 수행할 수 있습니다. 이 간단한 C# 애플리케이션은 API에 요청을 보내고 제안된 수정 사항을 반환합니다. 이 애플리케이션은 C#에서 작성되지만 API는 대부분의 프로그래밍 언어와 호환되는 RESTful 웹 서비스입니다. 이 애플리케이션의 소스 코드는 [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Search/BingAutosuggestv7.cs)에 제공됩니다.

## <a name="prerequisites"></a>필수 조건

* [Visual Studio 2017 이상](https://www.visualstudio.com/downloads/)의 모든 버전.
* Visual Studio에서 `Newtonsoft.Json`을 NuGet 패키지로 설치하려면 다음을 수행합니다.
    1. **솔루션 탐색기**에서 솔루션 파일을 마우스 오른쪽 단추로 클릭합니다.
    1. **솔루션용 NuGet 패키지 관리**를 선택합니다.
    1. `Newtonsoft.Json`을 검색하고 패키지를 설치합니다.
* Linux/MacOS를 사용하는 경우 이 애플리케이션은 [Mono](https://www.mono-project.com/)를 사용하여 실행할 수 있습니다.

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>프로젝트 만들기 및 초기화

1. Visual Studio에서 `SpellCheckSample`라는 새 콘솔 솔루션을 만듭니다. 그런 다음, 주 코드 파일에 다음 네임스페이스를 추가합니다.
    
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using Newtonsoft.Json;
    ```

2. API 엔드포인트, 구독 키 및 맞춤법을 검사할 텍스트에 대한 변수를 만듭니다.

    ```csharp
    namespace SpellCheckSample
    {
        class Program
        {
            static string host = "https://api.cognitive.microsoft.com";
            static string path = "/bing/v7.0/spellcheck?";
            static string key = "<ENTER-KEY-HERE>";
            //text to be spell-checked
            static string text = "Hollo, wrld!";
        }
    }
    ```

3. 검색 매개 변수에 대한 변수를 만듭니다. `mkt=` 뒤에 시장 코드를 추가합니다. 시장 코드는 요청을 수행한 국가입니다. 또한 `&mode=` 뒤에 맞춤법 검사 모드를 추가합니다. 모드는 `proof`(대부분의 맞춤법/문법 오류 catch) 또는 `spell`(대부분의 맞춤법은 catch하지만 문법 오류는 많지 않음)입니다.
    
    ```csharp
    static string params_ = "mkt=en-US&mode=proof";
    ```

## <a name="create-and-send-a-spell-check-request"></a>맞춤법 검사 요청 만들기 및 보내기

1. `SpellCheck()`라는 비동기식 함수를 만들어서 API에 요청을 보냅니다. `HttpClient`를 만들고 구독 키를 `Ocp-Apim-Subscription-Key` 헤더에 추가합니다. 그런 다음, 함수 내에서 다음 단계를 수행합니다.

    ```csharp
    async static void SpellCheck()
    {
        HttpClient client = new HttpClient();
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", key);

        HttpResponseMessage response = new HttpResponseMessage();
        // add the rest of the code snippets here (except for main())...
    }
    ```

2. 호스트, 경로 및 매개 변수를 추가하여 요청에 대한 URI를 만듭니다.
    
    ```csharp
    string uri = host + path + params_;
    ```

3. 텍스트를 포함하는 `KeyValuePair` 개체를 사용하여 목록을 만들고 이것을 사용하여 `FormUrlEncodedContent` 개체를 만듭니다. 헤더 정보를 설정하고 `PostAsync()`를 사용하여 요청을 보냅니다.

    ```csharp
    List<KeyValuePair<string, string>> values = new List<KeyValuePair<string, string>>();
    values.Add(new KeyValuePair<string, string>("text", text));
    
    using (FormUrlEncodedContent content = new FormUrlEncodedContent(values))
    {
        content.Headers.ContentType = new MediaTypeHeaderValue("application/x-www-form-urlencoded");
        response = await client.PostAsync(uri, content);
    }
    ```

## <a name="get-and-print-the-api-response"></a>API 응답 가져오기 및 출력

### <a name="get-the-client-id-header"></a>클라이언트 ID 헤더 가져오기

응답에 `X-MSEdge-ClientID` 헤더가 포함되어 있으면, 값을 가져와서 출력합니다.

``` csharp
string client_id;
if (response.Headers.TryGetValues("X-MSEdge-ClientID", out IEnumerable<string> header_values))
{
    client_id = header_values.First();
    Console.WriteLine("Client ID: " + client_id);
}
```

### <a name="get-the-response"></a>응답 가져오기

API의 응답을 가져옵니다. JSON 개체를 deserialize하고 콘솔에 출력합니다.

```csharp
string contentString = await response.Content.ReadAsStringAsync();

dynamic jsonObj = JsonConvert.DeserializeObject(contentString);
Console.WriteLine(jsonObj);
```

## <a name="call-the-spell-check-function"></a>맞춤법 검사 함수 호출

프로젝트의 Main 함수에서 `SpellCheck()`를 호출합니다.

```csharp
static void Main(string[] args)
{
    SpellCheck();
    Console.ReadLine();
}
```

## <a name="example-json-response"></a>예제 JSON 응답

성공한 응답은 다음 예제와 같이 JSON으로 반환됩니다. 

```json
{
   "_type": "SpellCheck",
   "flaggedTokens": [
      {
         "offset": 0,
         "token": "Hollo",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "Hello",
               "score": 0.9115257530801
            },
            {
               "suggestion": "Hollow",
               "score": 0.858039839213461
            },
            {
               "suggestion": "Hallo",
               "score": 0.597385084464481
            }
         ]
      },
      {
         "offset": 7,
         "token": "wrld",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "world",
               "score": 0.9115257530801
            }
         ]
      }
   ]
}
```

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [단일 페이지 웹앱 만들기](../tutorials/spellcheck.md)

- [Bing Spell Check API란?](../overview.md)
- [Bing Spell Check API v7 참조](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v7-reference)
