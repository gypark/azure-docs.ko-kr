---
title: '빠른 시작: Node.js용 Custom Vision SDK를 사용하여 이미지 분류 프로젝트 만들기'
titlesuffix: Azure Cognitive Services
description: Node.js SDK를 사용하여 프로젝트를 만들고, 태그를 추가하고, 이미지를 업로드하고, 프로젝트를 학습하고, 예측을 수행합니다.
services: cognitive-services
author: areddish
manager: daauld
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: quickstart
ms.date: 03/21/2019
ms.author: areddish
ms.openlocfilehash: 86e9508049635d51726e9d0bcbdc5a4d52539f3a
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66384159"
---
# <a name="quickstart-create-an-image-classification-project-with-the-custom-vision-nodejs-sdk"></a>빠른 시작: Custom Vision Node.js SDK를 사용하여 이미지 분류 프로젝트 만들기

이 문서에서는 Node.js와 함께 Custom Vision SDK를 사용하여 이미지 분류 모델 빌드를 시작할 수 있도록 도와주는 정보와 샘플 코드를 제공합니다. 프로젝트를 만든 후에는 태그를 추가하고, 이미지를 업로드하고, 프로젝트를 학습하고, 프로젝트의 게시된 예측 엔드포인트 URL을 확보하고, 이 엔드포인트를 사용하여 프로그래밍 방식으로 이미지를 테스트할 수 있습니다. Node.js 애플리케이션을 빌드하기 위한 템플릿으로 이 예제를 사용하세요. 코드 _없이_ 분류 모델을 빌드하고 사용하는 방법을 알아보려면 [브라우저 기반 가이드](getting-started-build-a-classifier.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

- [Node.js 8](https://www.nodejs.org/en/download/) 이상이 설치되어 있어야 합니다.
- [npm](https://www.npmjs.com/)이 설치되어 있어야 합니다.

## <a name="install-the-custom-vision-sdk"></a>Custom Vision SDK 설치

Node.js용 Custom Vision Service SDK를 설치하려면 PowerShell에서 다음 명령을 실행합니다.

```powershell
npm install azure-cognitiveservices-customvision-training
npm install azure-cognitiveservices-customvision-prediction
```

[!INCLUDE [get-keys](includes/get-keys.md)]

[!INCLUDE [node-get-images](includes/node-get-images.md)]

## <a name="add-the-code"></a>코드 추가

원하는 프로젝트 디렉터리에 *sample.js*라는 새 파일을 만듭니다.

### <a name="create-the-custom-vision-service-project"></a>Custom Vision Service 프로젝트 만들기

새 Custom Vision Service 프로젝트를 만드는 다음 코드를 스크립트에 추가합니다. 구독 키를 적절한 정의에 삽입합니다.

```javascript
const util = require('util');
const fs = require('fs');
const TrainingApiClient = require("azure-cognitiveservices-customvision-training");
const PredictionApiClient = require("azure-cognitiveservices-customvision-prediction");

const setTimeoutPromise = util.promisify(setTimeout);

const trainingKey = "<your training key>";
const predictionKey = "<your prediction key>";
const predictionResourceId = "<your prediction resource id>";
const sampleDataRoot = "<path to image files>";

const endPoint = "https://southcentralus.api.cognitive.microsoft.com"

const publishIterationName = "classifyModel";

const trainer = new TrainingApiClient(trainingKey, endPoint);

(async () => {
    console.log("Creating project...");
    const sampleProject = await trainer.createProject("Sample Project")
```

### <a name="create-tags-in-the-project"></a>프로젝트에서 태그 만들기

프로젝트의 분류 태그를 만들려면 *sample.js* 파일의 끝에 다음 코드를 추가합니다.

```javascript
    const hemlockTag = await trainer.createTag(sampleProject.id, "Hemlock");
    const cherryTag = await trainer.createTag(sampleProject.id, "Japanese Cherry");
```

### <a name="upload-and-tag-images"></a>이미지 업로드 및 태그 지정

프로젝트에 샘플 이미지를 추가하려면 태그를 만든 후 다음 코드를 삽입합니다. 이 코드는 해당 태그를 사용하여 각 이미지를 업로드합니다. Cognitive Services Node.js SDK 샘플 프로젝트를 다운로드한 위치에 따라 기본 이미지 파일 경로를 입력해야 합니다.

> [!NOTE]
> 이전에 Cognitive Services Node.js SDK 샘플 프로젝트를 다운로드한 위치에 따라 *sampleDataRoot*를 이미지 경로로 변경해야 합니다.

```javascript
    console.log("Adding images...");
    let fileUploadPromises = [];

    const hemlockDir = `${sampleDataRoot}/Hemlock`;
    const hemlockFiles = fs.readdirSync(hemlockDir);
    hemlockFiles.forEach(file => {
        fileUploadPromises.push(trainer.createImagesFromData(sampleProject.id, fs.readFileSync(`${hemlockDir}/${file}`), { tagIds: [hemlockTag.id] }));
    });

    const cherryDir = `${sampleDataRoot}/Japanese Cherry`;
    const japaneseCherryFiles = fs.readdirSync(cherryDir);
    japaneseCherryFiles.forEach(file => {
        fileUploadPromises.push(trainer.createImagesFromData(sampleProject.id, fs.readFileSync(`${cherryDir}/${file}`), { tagIds: [cherryTag.id] }));
    });

    await Promise.all(fileUploadPromises);
```

### <a name="train-the-classifier-and-publish"></a>분류자 학습 및 게시

이 코드는 프로젝트에서 첫 번째 반복을 만든 다음, 이 반복을 예측 엔드포인트에 게시합니다. 게시된 반복에 부여된 이름은 예측 요청을 보내는 데 사용할 수 있습니다. 반복은 게시될 때까지 예측 엔드포인트에서 사용할 수 없습니다.

```javascript
    console.log("Training...");
    let trainingIteration = await trainer.trainProject(sampleProject.id);

    // Wait for training to complete
    console.log("Training started...");
    while (trainingIteration.status == "Training") {
        console.log("Training status: " + trainingIteration.status);
        await setTimeoutPromise(1000, null);
        trainingIteration = await trainer.getIteration(sampleProject.id, trainingIteration.id)
    }
    console.log("Training status: " + trainingIteration.status);
    
    // Publish the iteration to the end point
    await trainer.publishIteration(sampleProject.id, trainingIteration.id, publishIterationName, predictionResourceId);
```

### <a name="get-and-use-the-published-iteration-on-the-prediction-endpoint"></a>게시된 반복을 예측 엔드포인트에서 가져와서 사용합니다.

예측 엔드포인트에 이미지를 보내고 예측을 검색하려면 파일의 끝에 다음 코드를 추가합니다.

```javascript
    const predictor = new PredictionApiClient(predictionKey, endPoint);
    const testFile = fs.readFileSync(`${sampleDataRoot}/Test/test_image.jpg`);

    const results = await predictor.classifyImage(sampleProject.id, publishIterationName, testFile);

    // Step 6. Show results
    console.log("Results:");
    results.predictions.forEach(predictedResult => {
        console.log(`\t ${predictedResult.tagName}: ${(predictedResult.probability * 100.0).toFixed(2)}%`);
    });
})()
```

## <a name="run-the-application"></a>애플리케이션 실행

*sample.js*를 실행합니다.

```powershell
node sample.js
```

애플리케이션의 출력은 다음 텍스트와 비슷할 것입니다.

```
Creating project...
Adding images...
Training...
Training started...
Training status: Training
Training status: Training
Training status: Training
Training status: Completed
Results:
         Hemlock: 94.97%
         Japanese Cherry: 0.01%
```

그러면 테스트 이미지( **<base_image_url>/Images/Test/** 에 있음)에 태그가 적절하게 지정되는지 확인할 수 있습니다. [Custom Vision 웹 사이트](https://customvision.ai)로 돌아가서 새로 만든 프로젝트의 현재 상태를 살펴볼 수도 있습니다.

[!INCLUDE [clean-ic-project](includes/clean-ic-project.md)]

## <a name="next-steps"></a>다음 단계

이미지 분류 프로세스의 모든 단계를 코드로 수행하는 방법을 살펴보았습니다. 이 샘플은 교육을 한 번만 반복하지만, 정확도를 높이기 위해 모델을 여러 차례 교육하고 테스트해야 하는 경우가 많습니다.

> [!div class="nextstepaction"]
> [모델 테스트 및 재교육](test-your-model.md)
