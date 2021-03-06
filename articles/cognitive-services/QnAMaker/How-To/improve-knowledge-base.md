---
title: 기술 자료 개선 - QnA Maker
titleSuffix: Azure Cognitive Services
description: 활성 학습을 사용하면 사용자가 제출한 정보에 따라 질문과 대답 쌍에 대체 질문을 제안하여 기술 자료 품질을 개선할 수 있습니다. 이 제안을 검토한 다음 기존 질문에 추가하거나 거부합니다. 기술 자료가 자동으로 변경되지는 않습니다. 모든 변경 내용이 적용 되려면 제안을 동의 해야 합니다. 해당 제안과 질문을 수락해도 기존 질문이 변경되거나 제거되지는 않습니다.
author: diberry
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 05/13/2019
ms.author: diberry
ms.openlocfilehash: f80e6a765cc165033a548ba6a5ee7bead0de872e
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65594093"
---
# <a name="use-active-learning-to-improve-your-knowledge-base"></a>활성 학습을 사용 하 여 기술 자료를 개선 하기 위해

활성 학습을 사용하면 사용자가 제출한 정보에 따라 질문과 대답 쌍에 대체 질문을 제안하여 기술 자료 품질을 개선할 수 있습니다. 이 제안을 검토한 다음 기존 질문에 추가하거나 거부합니다. 

기술 자료가 자동으로 변경되지는 않습니다. 즉, 변경 내용을 적용하려면 제안을 수락해야 합니다. 해당 제안과 질문을 수락해도 기존 질문이 변경되거나 제거되지는 않습니다.

## <a name="what-is-active-learning"></a>활성 학습 이란?

QnA Maker는 암시적/명시적 피드백을 사용하여 새로운 질문 변형을 학습합니다.
 
* 암시적 피드백 – 사용자 질문에 점수가 거의 같은 대답이 여러 개 있으면 순위매기기 기능은 해당 대답을 파악하여 피드백으로 간주합니다. 
* 명시적 피드백 – 점수가 약간씩 다른 여러 대답이 기술 자료에서 반환되면 클라이언트 애플리케이션은 올바른 질문을 사용자에게 질문합니다. 이 경우 사용자의 명시적 피드백이 학습 API를 통해 QnA Maker로 전송됩니다. 

둘 중 어떤 방법을 사용하든 순위매기기 기능에 클러스터된 유사 쿼리가 제공됩니다.

## <a name="how-active-learning-works"></a>활성 학습의 작동 방식

활성 학습은 지정된 쿼리에 대해 QnA Maker에서 반환하는 몇 가지 상위 대답 점수를 기준으로 트리거됩니다. 점수에 큰 차이가 없으면 쿼리는 가능한 각 대답의 가능한 _제안_으로 간주됩니다. 

모든 제안은 유사성을 기준으로 클러스터되며, 최종 사용자가 특정 쿼리를 전송한 빈도에 따라 대체 질문의 상위 제안이 표시됩니다. 엔드포인트가 적절한 수와 종류의 사용량 쿼리를 수신하면 활성 학습에서는 가능한 최적의 제안을 제공합니다.

5 개 또는 더 유사한 쿼리는 클러스터 된 경우 30 분 마다 QnA Maker 제안 허용 하거나 거부 하 여 기술 자료 디자이너로 사용자 기반 질문 합니다.

질문 QnA Maker 포털에서 제안 되 면 검토 하 고 허용 하거나 이러한 제안 취소 해야 합니다. 

## <a name="upgrade-your-version-to-use-active-learning"></a>활성 학습을 사용 하려면 버전 업그레이드

활성 학습은 런타임 버전 4.4.0 이상에서 지원됩니다. 기술 자료가 이전 버전에서 작성된 경우 이 기능을 사용하려면 [런타임을 업그레이드](troubleshooting-runtime.md#how-to-get-latest-qnamaker-runtime-updates)합니다. 

## <a name="best-practices"></a>모범 사례

활성 학습 사용 시의 모범 사례는 [모범 사례](../Concepts/best-practices.md#active-learning)를 참조하세요.

## <a name="score-proximity-between-knowledge-base-questions"></a>기술 자료 질문 간의 점수 범위

질문의 점수 신뢰도가 80% 등으로 높은 경우 활성 학습용으로 간주되는 점수의 범위도 넓어집니다(대략 10% 이내). 반면 신뢰도 점수가 40% 등으로 낮아지면 점수 범위도 좁아집니다(대략 4% 이내). 

점수 범위를 결정하는 알고리즘은 단순한 계산이 아닙니다. 앞에서 언급한 예의 범위는 고정된 범위가 아니며, 알고리즘의 영향을 파악하기 위한 참조 범위로만 사용해야 합니다.

## <a name="turn-on-active-learning"></a>활성 학습 설정

활성 학습은 기본적으로 해제되어 있습니다. 제안된 질문을 확인하려면 활성 학습을 설정합니다. 

1. 선택 **게시** 기술 자료를 게시 합니다. 활성 학습 쿼리 GenerateAnswer API 예측 끝점에서 수집 됩니다. QnA Maker 포털에서 테스트 창에 대 한 쿼리로 활성 학습을 영향을 주지 않습니다.

1. 활성에서 학습을 설정 하려면 클릭에 **이름을**로 이동 하세요 [ **서비스 설정** ](https://www.qnamaker.ai/UserSettings) QnA Maker 포털 오른쪽 위 모서리에서.  

    ![서비스 설정 페이지에서 활성 학습의 제안 된 질문 대안을 켭니다. 사용자 이름 오른쪽 위 메뉴에서 선택한 서비스 설정을 선택 합니다.](../media/improve-knowledge-base/Endpoint-Keys.png)


1. QnA Maker 서비스를 찾은 다음 **활성 학습**을 설정 상태로 전환합니다. 

    [![서비스 설정 페이지에서 활성 학습 기능을 설정/해제 합니다. 기능 설정/해제할 수 없는 경우 서비스를 업그레이드 해야 합니다.](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png)](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png#lightbox)

    **활성 학습**이 사용하도록 설정되면 사용자가 제출한 질문에 따라 기술 자료에서 일정한 간격으로 새 질문을 제안합니다. 설정을 다시 전환하여 **활성 학습**을 사용하지 않도록 설정할 수 있습니다.

## <a name="add-active-learning-suggestion-to-knowledge-base"></a>기술 자료에 활성 학습 제안 추가

1. 제안 된 질문을 확인 하기 위해 합니다 **편집** 기술 자료 페이지에서 **보기 옵션**을 선택한 후 **활성 학습 제안 표시**합니다. 

    [![포털의 편집 섹션에서 활성 학습의 새로운 질문 대안을 확인 하기 위해 제안 표시를 선택 합니다.](../media/improve-knowledge-base/show-suggestions-button.png)](../media/improve-knowledge-base/show-suggestions-button.png#lightbox)

1. 질문 및 답변 쌍을 선택 하 여 제안만을 표시 하 고 기술 자료를 필터링 **제안 필터링**합니다.

    [![활성 학습의 제안 된 질문 대안만 보려면 제안 설정/해제 하 여 필터를 사용 합니다.](../media/improve-knowledge-base/filter-by-suggestions.png)](../media/improve-knowledge-base/filter-by-suggestions.png#lightbox)

1.  제안이 포함된 각 질문 섹션에 새 질문이 표시되고 질문을 수락할 수 있는 확인 표시(`✔`)와 제안을 거부할 수 있는 `x`가 함께 표시됩니다. 질문을 추가하려면 확인 표시를 선택합니다. 

    [![선택 하거나 녹색 확인 표시 나 빨간색 삭제 표시를 선택 하 여 활성 학습의 제안 된 질문 대안을 거부 합니다.](../media/improve-knowledge-base/accept-active-learning-suggestions.png)](../media/improve-knowledge-base/accept-active-learning-suggestions.png#lightbox)

    **모두 추가** 또는 **모두 거부**를 선택하여 _모든 제안_을 추가하거나 삭제할 수 있습니다.

1. **저장 및 학습**을 선택하여 기술 자료에 변경 내용을 저장합니다.

1. 선택 **게시** GenerateAnswer API에서 사용 가능 하도록 변경할 수 있도록 합니다.

    5 개 또는 더 유사한 쿼리는 클러스터 된 경우 30 분 마다 QnA Maker 제안 허용 하거나 거부 하 여 기술 자료 디자이너로 사용자 기반 질문 합니다.

## <a name="determine-best-choice-when-several-questions-have-similar-scores"></a>여러 질문의 점수가 비슷할 때 가장 적절한 선택 항목 결정

여러 질문의 점수에 거의 차이가 없으면 클라이언트 애플리케이션 개발자는 가장 적절한 질문 확인을 요청할 수 있습니다.

### <a name="use-the-top-property-in-the-generateanswer-request"></a>GenerateAnswer 요청에서 top 속성 사용

대답을 확인하기 위해 QnA Maker에 질문을 제출할 때는 JSON 본문의 특정 부분에서 상위 대답이 여러 개 반환되도록 코드를 작성할 수 있습니다.

```json
{
    "question": "wi-fi",
    "isTest": false,
    "top": 3
}
```

챗봇 등의 클라이언트 애플리케이션이 응답을 수신하면 상위 3개 질문이 반환됩니다.

```json
{
    "answers": [
        {
            "questions": [
                "Wi-Fi Direct Status Indicator"
            ],
            "answer": "**Wi-Fi Direct Status Indicator**\n\nStatus bar icons indicate your current Wi-Fi Direct connection status:  \n\nWhen your device is connected to another device using Wi-Fi Direct, '$  \n\n+ •+ ' Wi-Fi Direct is displayed in the Status bar.",
            "score": 74.21,
            "id": 607,
            "source": "Bugbash KB.pdf",
            "metadata": []
        },
        {
            "questions": [
                "Wi-Fi - Connections"
            ],
            "answer": "**Wi-Fi**\n\nWi-Fi is a term used for certain types of Wireless Local Area Networks (WLAN). Wi-Fi communication requires access to a wireless Access Point (AP).",
            "score": 74.15,
            "id": 599,
            "source": "Bugbash KB.pdf",
            "metadata": []
        },
        {
            "questions": [
                "Turn Wi-Fi On or Off"
            ],
            "answer": "**Turn Wi-Fi On or Off**\n\nTurning Wi-Fi on makes your device able to discover and connect to compatible in-range wireless APs.  \n\n1.  From a Home screen, tap ::: Apps > e Settings .\n2.  Tap Connections > Wi-Fi , and then tap On/Off to turn Wi-Fi on or off.",
            "score": 69.99,
            "id": 600,
            "source": "Bugbash KB.pdf",
            "metadata": []
        }
    ]
}
```

### <a name="client-application-follow-up-when-questions-have-similar-scores"></a>질문의 점수가 비슷할 때의 클라이언트 애플리케이션 추가 작업

클라이언트 애플리케이션은 사용자가 의도를 가장 적절하게 반영하는 질문을 선택할 수 있는 옵션과 함께 모든 질문을 표시합니다. 

사용자가 기존 질문 중 하나를 선택 되 면 클라이언트 응용 프로그램 사용자가 선택한을 QnA Maker의 학습 API를 사용 하 여 피드백을 보냅니다. 이 피드백 완료 활성 피드백 루프를 학습 합니다. 

사용 합니다 [Azure Bot 샘플](https://aka.ms/activelearningsamplebot) 종단 간 시나리오에서 활성 학습을 확인 합니다.

## <a name="train-api"></a>학습 API

활성 학습 피드백 QnA Maker를 학습 API POST 요청과 함께 전송 됩니다. API 서명은 다음과 같습니다.

```http
POST https://<QnA-Maker-resource-name>.azurewebsites.net/qnamaker/knowledgebases/<knowledge-base-ID>/train
Authorization: EndpointKey <endpoint-key>
Content-Type: application/json
{"feedbackRecords": [{"userId": "1","userQuestion": "<question-text>","qnaId": 1}]}
```

|HTTP 요청 속성|Name|Type|목적|
|--|--|--|--|
|URL 경로 매개 변수|기술 자료 ID|문자열|기술 자료를 위한 GUID입니다.|
|호스트 하위 도메인|QnAMaker 리소스 이름|문자열|Azure 구독에서 QnA Maker에 대 한 호스트 이름입니다. 기술 자료를 게시 한 후 설정 페이지에서 제공 됩니다. |
|헤더|콘텐츠 형식|문자열|API로 전송되는 본문의 미디어 유형입니다. 기본값은입니다. `application/json`|
|헤더|권한 부여|문자열|엔드포인트 키(EndpointKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)입니다.|
|Post 본문|JSON 개체|JSON|교육 피드백|

JSON 본문을 몇 가지 설정은 다음과 같습니다.

|JSON 본문 속성|Type|목적|
|--|--|--|--|
|`feedbackRecords`|array|피드백의 목록입니다.|
|`userId`|문자열|제안 된 질문을 수락 하는 사용자의 사용자 ID입니다. 사용자 ID 형식은 사용자의 몫입니다. 예를 들어 전자 메일 주소에는 아키텍처에 올바른 사용자 ID를 수 있습니다. 선택 사항입니다.|
|`userQuestion`|문자열|질문의 정확한 텍스트입니다. 필수 사항입니다.|
|`qnaID`|번호|질문에 있는 ID를 [GenerateAnswer 응답](metadata-generateanswer-usage.md#generateanswer-response-properties)합니다. |

예제 JSON 본문 다음과 같습니다.

```json
{
    "feedbackRecords": [
        {
            "userId": "1",
            "userQuestion": "<question-text>",
            "qnaId": 1
        }
    ]
}
```

응답에 성공 하면 204 및 JSON 응답 본문의 상태를 반환합니다. 

<a name="active-learning-is-saved-in-the-exported-apps-tsv-file"></a>

## <a name="active-learning-is-saved-in-the-exported-knowledge-base"></a>활성 학습 내보낸된 기술 자료에 저장 됩니다.

앱이 활성 학습 사용 하도록 설정 하 고 앱을 내보낼 때의 `SuggestedQuestions` tsv 파일의 열은 활성 학습 데이터를 유지 합니다. 

`SuggestedQuestions` 열은 암시적 일의 정보는 JSON 개체로 `autosuggested`, 및 명시적 `usersuggested` 피드백. 이 JSON 개체의 단일 사용자가 제출한 질문에 대 한 예가 `help` 됩니다.

```JSON
[
    {
        "clusterHead": "help",
        "totalAutoSuggestedCount": 1,
        "totalUserSuggestedCount": 0,
        "alternateQuestionList": [
            {
                "question": "help",
                "autoSuggestedCount": 1,
                "userSuggestedCount": 0
            }
        ]
    }
]
```

이 앱을 다시 가져오는 경우 활성 학습 정보를 수집 하 고 기술 자료에 대 한 제안을 권장 계속 합니다. 

## <a name="next-steps"></a>다음 단계
 
> [!div class="nextstepaction"]
> [GenerateAnswer API를 사용 하 여 메타 데이터를 사용 합니다.](metadata-generateanswer-usage.md)
