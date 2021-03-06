---
title: '빠른 시작: Bing News Search REST API 및 Go를 사용하여 뉴스 가져오기'
titleSuffix: Azure Cognitive Services
description: Bing News Search API에서 뉴스 결과를 가져오는 방법을 알아봅니다.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 2/21/2019
ms.author: aahi
ms.openlocfilehash: 79e93e3ba0bbf9ac71a01bad0502b84dfee85297
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65798495"
---
# <a name="quickstart-get-news-results-using-the-bing-news-search-rest-api-and-go"></a>빠른 시작: Bing News Search REST API 및 Go를 사용하여 뉴스 결과 가져오기

이 빠른 시작에서는 Go 언어를 사용하여 Bing News Search API를 호출합니다. 결과는 쿼리 문자열로 식별되는 뉴스 원본의 이름 및 URL을 포함합니다.

## <a name="prerequisites"></a>필수 조건
* [Go 이진 파일](https://golang.org/dl/)을 설치합니다.
* 프린터가 깔끔한 결과를 표시하도록 go-spew 라이브러리를 설치합니다.
    * `$ go get -u https://github.com/davecgh/go-spew` 라이브러리를 설치합니다.

[!INCLUDE [bing-web-search-quickstart-signup](../../../includes/bing-web-search-quickstart-signup.md)]

## <a name="create-a-project-and-import-libraries"></a>프로젝트 만들기 및 라이브러리 가져오기

IDE 또는 편집기에서 새 Go 프로젝트를 만듭니다. 그런 다음, 요청하기 위해 `net/http`, 응답을 읽기 위해 `ioutil`, JSON 텍스트 결과를 처리하기 위해 `encoding/json`을 가져옵니다. go-spew 라이브러리는 JSON을 구문 분석하는 데 필요합니다. 

```
package main

import (
    "fmt"
    "net/http"
    "io/ioutil"
    "encoding/json"
    "github.com/davecgh/go-spew/spew"
)

```

## <a name="create-a-struct-to-format-the-news-search-results"></a>뉴스 검색 결과 형식을 지정하는 구조체 만들기

`NewsAnswer` 구조체는 응답에 제공되는 데이터 형식을 지정합니다. 응답 JSON은 매우 복잡하며 다단계로 이루어져 있습니다.  다음 구현에서는 주요 정보를 설명합니다.

```
// This struct formats the answer provided by the Bing News Search API.
type NewsAnswer struct {
    ReadLink       string `json: "readLink"` 
    QueryContext   struct {
        OriginalQuery   string   `json: "originalQuery"`
        AdultIntent     bool        `json: "adultIntent"`
    } `json: "queryContext"`
    TotalEstimatedMatches   int  `json: totalEstimatedMatches"` 
    Sort  []struct {
        Name    string  `json: "name"`
        ID       string    `json: "id"`
        IsSelected       bool  `json: "isSelected"`
        URL      string   `json: "url"`
    }  `json: "sort"` 
    Value   []struct   {
        Name     string   `json: "name"`
        URL   string    `json: "url"`
        Image   struct   {
            Thumbnail   struct  {
                ContentUrl  string  `json: "thumbnail"`
                Width   int  `json: "width"`
                Height  int   `json: "height"`
            } `json: "thumbnail"` 
            Description  string  `json: "description"`
            Provider  []struct   {
                Type   string    `json: "_type"`
                Name  string     `json: "name"`
            } `json: "provider"` 
            DatePublished   string   `json: "datePublished"`
        } `json: "image"` 
    } `json: "value"` 
}

```

## <a name="declare-the-main-function-and-define-variables"></a>main 함수 선언 및 변수 정의  

다음 코드는 main 함수를 선언하고 필요한 변수를 할당합니다. 엔드포인트가 올바른지 확인하고 `token` 값을 Azure 계정의 유효한 구독 키로 바꿉니다.

```
func main() {
    // Verify the endpoint URI and replace the token string with a valid subscription key.  
    const endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/news/search"
    token := "YOUR-ACCESS-KEY"
    searchTerm := "Microsoft Cognitive Services"

    // Declare a new GET request.
    req, err := http.NewRequest("GET", endpoint, nil)
    if err != nil {
        panic(err)
    }

    // The rest of the code in this example goes here in the main function.
}
```

## <a name="query-and-header"></a>쿼리 및 헤더

쿼리 문자열 및 액세스 키 헤더 추가

```
// Add the query to the request.  
param := req.URL.Query()
param.Add("q", searchTerm)
req.URL.RawQuery = param.Encode()

// Insert the subscription-key header.  
req.Header.Add("Ocp-Apim-Subscription-Key", token)

```

## <a name="get-request"></a>Get 요청

클라이언트를 만들고 Get 요청을 보냅니다. 

```
// Instantiate a client.  
client := new(http.Client)

// Send the request to Bing.  
resp, err := client.Do(req)
if err != nil {
    panic(err)
}

```

## <a name="send-the-request"></a>요청 보내기

요청을 보내고 `ioutil`을 사용하여 결과를 읽습니다.

```
resp, err := client.Do(req)
    if err != nil {
        panic(err)
}

// Close the connection.    
defer resp.Body.Close()

// Read the results
body, err := ioutil.ReadAll(resp.Body)
if err != nil {
    panic(err)
}

```

## <a name="handle-the-response"></a>응답 처리

`Unmarshall` 함수는 News Search API에서 반환한 JSON 텍스트에서 정보를 추출합니다.  그러면 `go-spew` 깔끔한 프린터를 사용하여 결과에서 노드를 표시할 수 있습니다.

```
// Create a new answer object 
ans := new(NewsAnswer)
err = json.Unmarshal(body, &ans)
if err != nil {
    fmt.Println(err)
}
    
fmt.Print("Output of BingAnswer: \r\n\r\n")
    
// Iterate over search results and print the result name and URL.
for _, result := range ans.Value{
spew.Dump(result.Name, result.URL)
}

```

## <a name="results"></a>결과

결과는 각 결과의 이름 및 URL을 포함합니다.

```
(string) (len=91) "Cognitive Services Market: Global Industry Analysis and Opportunity Assessment, 2019 - 2025"
(string) (len=142) "https://www.marketwatch.com/press-release/cognitive-services-market-global-industry-analysis-and-opportunity-assessment-2019---2025-2019-02-21"
(string) (len=104) "Microsoft calls for greater collaboration to harness the power of AI to empower people with disabilities"
(string) (len=115) "https://indiaeducationdiary.in/microsoft-calls-greater-collaboration-harness-power-ai-empower-people-disabilities-2/"
(string) (len=70) "Microsoft 'Intelligent Cloud Hub' to build AI-ready workforce in India"
(string) (len=139) "https://cio.economictimes.indiatimes.com/news/cloud-computing/microsoft-intelligent-cloud-hub-to-build-ai-ready-workforce-in-india/67187807"
(string) (len=81) "Skills shortage is stopping many Asian companies from embracing A.I., study shows"
(string) (len=106) "https://www.cnbc.com/2019/02/20/microsoft-idc-study-skills-shortages-stopping-companies-from-using-ai.html"
(string) (len=143) "Cognitive Computing in Healthcare Market Emerging Top Key Vendors- Apixio, MedWhat, Healthcare X.0, Apple, Google, Microsoft, and IBM 2017-2025"
(string) (len=40) "http://www.digitaljournal.com/pr/4163064"
(string) (len=49) "Microsoft launches AI skills initiative in Brazil"
(string) (len=80) "https://www.zdnet.com/article/microsoft-launches-ai-skills-initiative-in-brazil/"
(string) (len=45) "Kuwait's CITRA and Microsoft host AI OpenHack"
(string) (len=70) "http://www.itp.net/618639-kuwaits-citra-and-microsoft-host-ai-openhack"
(string) (len=51) "CITRA and Microsoft collaborate to host AI workshop"
(string) (len=123) "https://www.zawya.com/mena/en/press-releases/story/CITRA_and_Microsoft_collaborate_to_host_AI_workshop-ZAWYA20190212105751/"

```

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [Bing News Search란?](search-the-web.md)
