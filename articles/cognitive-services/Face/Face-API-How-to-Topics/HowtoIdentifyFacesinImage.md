---
title: '예제: 이미지에서 얼굴 식별 - Face API'
titleSuffix: Azure Cognitive Services
description: Face API를 사용하여 이미지에서 얼굴을 식별합니다.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 04/10/2019
ms.author: sbowles
ms.openlocfilehash: c22230545ccbe1ef1b4bfa35a33f0302197463b1
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66124537"
---
# <a name="example-identify-faces-in-images"></a>예제: 이미지에서 얼굴 식별

이 가이드에서는 미리 알려진 사람으로부터 만들어진 PersonGroup 개체를 사용하여 알 수 없는 얼굴을 식별하는 방법을 보여 줍니다. 샘플은 Azure Cognitive Services Face API 클라이언트 라이브러리를 사용하여 C#으로 작성되었습니다.

## <a name="preparation"></a>준비

이 샘플에서 설명하는 방법은 다음과 같습니다.

- PersonGroup을 만드는 방법. 이 PersonGroup에는 알려진 사람의 목록이 포함되어 있습니다.
- 각 사람에게 얼굴을 할당하는 방법. 이러한 얼굴은 사람을 식별하는 기준으로 사용됩니다. 얼굴에 대한 선명한 정면 보기를 사용하는 것이 좋습니다. 예를 들어 사진 ID가 있습니다. 일단의 좋은 사진에는 다양한 자세, 의복 색상 또는 머리 모양을 보여 주는 동일한 사람의 얼굴이 포함되어 있습니다.

이 샘플의 데모를 수행하려면 다음을 준비합니다.

- 사람 얼굴을 포함하는 몇 장의 사진. Anna, Bill 및 Clare에 대한 [샘플 사진을 다운로드](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data)합니다.
- 일련의 테스트 사진. 사진에는 Anna, Bill 또는 Clare의 얼굴이 포함되거나 포함되지 않을 수 있습니다. 식별을 테스트하는 데 사용됩니다. 또한 이전 링크에서 일부 샘플 이미지를 선택합니다.

## <a name="step-1-authorize-the-api-call"></a>1단계: API 호출 권한 부여

Face API를 호출할 때마다 구독 키가 필요합니다. 이 키는 쿼리 문자열 매개 변수를 통해 전달되거나 요청 헤더에서 지정될 수 있습니다. 쿼리 문자열을 통해 구독 키를 전달하려면 다음 예제와 같은 [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)에 대한 요청 URL을 참조하세요.
```
https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes]
&subscription-key=<Subscription key>
```

대안으로 구독 키를 **ocp-apim-subscription-key: &lt;구독 키&gt;** HTTP 요청 헤더에 지정합니다.
클라이언트 라이브러리를 사용하는 경우 구독 키는 FaceServiceClient 클래스의 생성자를 통해 전달됩니다. 예: 
 
```CSharp 
faceServiceClient = new FaceServiceClient("<Subscription Key>");
```
 
구독 키를 가져오려면 Azure Portal에서 Azure Marketplace로 이동합니다. 자세한 내용은 [구독](https://azure.microsoft.com/try/cognitive-services/)을 참조하세요.

## <a name="step-2-create-the-persongroup"></a>2단계: PersonGroup 만들기

이 단계에서는 Anna, Bill 및 Clare가 "MyFriends"라는 PersonGroup에 포함됩니다. 각 사람은 몇 개의 얼굴을 등록했습니다. 얼굴은 이미지에서 감지되어야 합니다. 이러한 모든 단계를 수행하면 다음 이미지와 같은 PersonGroup이 생성됩니다.

![MyFriends](../Images/group.image.1.jpg)

### <a name="step-21-define-people-for-the-persongroup"></a>2.1단계: PersonGroup에 대한 사람 정의
사람은 식별의 기본 단위입니다. 사람은 하나 이상의 알려진 얼굴을 등록할 수 있습니다. PersonGroup은 사람의 컬렉션입니다. 각 사람은 특정 PersonGroup 내에서 정의됩니다. 식별은 PersonGroup에 대해 수행됩니다. 작업은 PersonGroup을 만든 다음, 여기에 Anna, Bill 및 Clare와 같은 사람을 만드는 것입니다.

먼저 [PersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API를 사용하여 새 PersonGroup을 만듭니다. 해당 클라이언트 라이브러리 API는 FaceServiceClient 클래스에 대한 CreatePersonGroupAsync 메서드입니다. 그룹을 만들기 위해 지정된 그룹 ID는 각 구독에 대해 고유합니다. 다른 PersonGroup API를 사용하여 PersonGroup을 가져오거나, 업데이트하거나, 삭제할 수도 있습니다. 

그룹이 정의되면 [PersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API를 사용하여 그룹 내의 사람을 정의할 수 있습니다. 클라이언트 라이브러리 메서드는 CreatePersonAsync입니다. 사람이 만들어지면 각 사람에게 얼굴을 추가할 수 있습니다.

```CSharp 
// Create an empty PersonGroup
string personGroupId = "myfriends";
await faceServiceClient.CreatePersonGroupAsync(personGroupId, "My Friends");
 
// Define Anna
CreatePersonResult friend1 = await faceServiceClient.CreatePersonAsync(
    // Id of the PersonGroup that the person belonged to
    personGroupId,    
    // Name of the person
    "Anna"            
);
 
// Define Bill and Clare in the same way
```
### <a name="step2-2"></a> 2.2단계: 얼굴 감지 및 올바른 사람에게 등록
감지는 HTTP 요청 본문에 이미지 파일을 사용하여 [얼굴 - 감지](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API로 "POST" 웹 요청을 전송하여 진행됩니다. 클라이언트 라이브러리를 사용하는 경우 얼굴 감지는 FaceServiceClient 클래스에 대한 DetectAsync 메서드를 통해 수행됩니다.

감지된 각 얼굴에 대해 [PersonGroup Person – Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b)를 호출하여 해당 얼굴을 올바른 사람에게 추가합니다.

다음 코드에서는 이미지에서 얼굴을 감지한 후 사람에게 추가하는 방법 프로세스를 보여 줍니다.
```CSharp 
// Directory contains image files of Anna
const string friend1ImageDir = @"D:\Pictures\MyFriends\Anna\";
 
foreach (string imagePath in Directory.GetFiles(friend1ImageDir, "*.jpg"))
{
    using (Stream s = File.OpenRead(imagePath))
    {
        // Detect faces in the image and add to Anna
        await faceServiceClient.AddPersonFaceAsync(
            personGroupId, friend1.PersonId, s);
    }
}
// Do the same for Bill and Clare
``` 
이미지에 둘 이상의 얼굴이 포함되는 경우 가장 큰 얼굴만 추가됩니다. 사람에게 다른 얼굴을 추가할 수 있습니다. "targetFace = 왼쪽, 위쪽, 너비, 높이" 형식의 문자열을 [PersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API의 targetFace 쿼리 매개 변수에 전달합니다. 또한 AddPersonFaceAsync 메서드에 대한 targetFace 선택적 매개 변수를 사용하여 다른 얼굴을 추가할 수도 있습니다. 사람에게 추가된 각 얼굴에는 고유한 지속형 얼굴 ID가 지정됩니다. 이 ID는 [PersonGroup 사람 – Delete Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523e) 및 [Face – Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)에서 사용할 수 있습니다.

## <a name="step-3-train-the-persongroup"></a>3단계: PersonGroup 학습

PersonGroup을 사용하여 식별을 수행하려면 먼저 해당 PersonGroup이 학습되어야 합니다. 사람을 추가하거나 제거한 후에 또는 등록된 사람 얼굴을 편집한 경우 PersonGroup이 다시 학습되어야 합니다. 학습은 [PersonGroup – 학습](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) API를 통해 수행됩니다. 클라이언트 라이브러리를 사용하는 경우 TrainPersonGroupAsync 메서드에 대한 호출은 다음과 같습니다.
 
```CSharp 
await faceServiceClient.TrainPersonGroupAsync(personGroupId);
```
 
학습은 비동기 프로세스입니다. TrainPersonGroupAsync 메서드가 반환된 후에도 완료되지 않을 수 있습니다. 학습 상태를 쿼리해야 할 수도 있습니다. [PersonGroup - Get Training Status](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395247) API 또는 클라이언트 라이브러리의 GetPersonGroupTrainingStatusAsync 메서드를 사용합니다. 다음 코드에서는 PersonGroup 학습이 완료될 때까지 기다리는 간단한 논리를 보여 줍니다.
 
```CSharp 
TrainingStatus trainingStatus = null;
while(true)
{
    trainingStatus = await faceServiceClient.GetPersonGroupTrainingStatusAsync(personGroupId);
 
    if (trainingStatus.Status != Status.Running)
    {
        break;
    }
 
    await Task.Delay(1000);
} 
``` 

## <a name="step-4-identify-a-face-against-a-defined-persongroup"></a>4단계: 정의된 PersonGroup에 대해 얼굴 식별

Face API에서 식별을 수행하는 경우 그룹 내의 모든 얼굴에 걸쳐 테스트 얼굴의 유사성을 계산합니다. 테스트 얼굴에 대해 가장 유사한 사람을 반환합니다. 이 프로세스는 [Face - Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) API 또는 클라이언트 라이브러리의 IdentifyAsync 메서드를 통해 수행됩니다.

이전 단계를 사용하여 테스트 얼굴을 감지해야 합니다. 그런 다음, 얼굴 ID를 두 번째 인수로 식별 API에 전달합니다. 여러 얼굴 ID를 한 번에 식별할 수 있습니다. 결과에는 식별된 모든 결과가 포함됩니다. 기본적으로 식별 프로세스는 테스트 얼굴과 가장 많이 일치하는 한 사람만 반환합니다. 원하는 경우 식별 프로세스에서 더 많은 후보를 반환하도록 하려면 선택적인 maxNumOfCandidatesReturned 매개 변수를 지정합니다.

다음 코드에서는 식별 프로세스를 보여 줍니다.

```CSharp 
string testImageFile = @"D:\Pictures\test_img1.jpg";

using (Stream s = File.OpenRead(testImageFile))
{
    var faces = await faceServiceClient.DetectAsync(s);
    var faceIds = faces.Select(face => face.FaceId).ToArray();
 
    var results = await faceServiceClient.IdentifyAsync(personGroupId, faceIds);
    foreach (var identifyResult in results)
    {
        Console.WriteLine("Result of face: {0}", identifyResult.FaceId);
        if (identifyResult.Candidates.Length == 0)
        {
            Console.WriteLine("No one identified");
        }
        else
        {
            // Get top 1 among all candidates returned
            var candidateId = identifyResult.Candidates[0].PersonId;
            var person = await faceServiceClient.GetPersonAsync(personGroupId, candidateId);
            Console.WriteLine("Identified as {0}", person.Name);
        }
    }
}
``` 

단계가 완료되면 다른 얼굴을 식별하려고 시도합니다. 얼굴 감지를 위해 업로드한 이미지에 따라 Anna, Bill 또는 Clare의 얼굴이 정확하게 식별되는지 확인합니다. 다음 예제를 참조하세요.

![다른 얼굴 식별](../Images/identificationResult.1.jpg )

## <a name="step-5-request-for-large-scale"></a>5단계: 대규모 기능에 대한 요청

PersonGroup은 이전의 설계 제한에 따라 최대 10,000명을 포함할 수 있습니다.
백만 명 규모까지 확장하는 시나리오에 대한 자세한 내용은 [대규모 기능을 사용하는 방법](how-to-use-large-scale.md)을 참조하세요.

## <a name="summary"></a>요약

이 가이드에서는 PersonGroup을 만들고 사람을 식별하는 프로세스를 알아보았습니다. 다음 기능에 대해 설명하고 시연했습니다.

- [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d) API를 사용하여 얼굴을 감지합니다.
- [PersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API를 사용하여 PersonGroup을 만듭니다.
- [PersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API를 사용하여 사람을 만듭니다.
- [PersonGroup – Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) API를 사용하여 PersonGroup을 학습시킵니다.
- [Face - Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) API를 사용하여 PersonGroup에 대해 알 수 없는 얼굴을 식별합니다.

## <a name="related-topics"></a>관련된 항목

- [얼굴 인식 개념](../concepts/face-recognition.md)
- [이미지에서 얼굴 감지](HowtoDetectFacesinImage.md)
- [얼굴 추가](how-to-add-faces.md)
- [대규모 기능 사용](how-to-use-large-scale.md)
