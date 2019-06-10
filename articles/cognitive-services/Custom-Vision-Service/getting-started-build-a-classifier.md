---
title: 분류자 빌드 - Custom Vision Service
titlesuffix: Azure Cognitive Services
description: Custom Vision 웹 사이트를 사용하여 이미지 분류 모델을 만드는 방법을 알아봅니다.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: anroth
ms.openlocfilehash: 3cb67b57f406774b4bcaf57c24b8e7741068ced6
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66497318"
---
# <a name="how-to-build-a-classifier-with-custom-vision"></a>Custom Vision을 사용하여 분류자를 빌드하는 방법

이미지 분류에 Custom Vision Service를 사용하려면 먼저 분류자 모델을 빌드해야 합니다. 이 가이드에서는 Custom Vision 웹 사이트를 통해 분류자를 빌드하는 방법을 알아봅니다.

## <a name="prerequisites"></a>필수 조건

- 유효한 Azure 구독. 평가판 [계정을 만들 수 있습니다](https://azure.microsoft.com/free/).
- 분류자를 학습시키는 데 사용할 이미지 세트. 이미지 선택 팁은 아래 설명을 참조하세요.


## <a name="create-custom-vision-resources-in-the-azure-portal"></a>Azure portal에서 Custom Vision 리소스 만들기
Custom Vision Service를 사용 하려면에서 사용자 지정 비전 교육 및 예측 리소스를 만들 해야 합니다 [Azure portal](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision)합니다. 이렇게 하면 학습 및 예측 리소스가 둘 다 생성됩니다. 

## <a name="create-a-new-project"></a>새 프로젝트 만들기

웹 브라우저에서 [Custom Vision 웹 페이지](https://customvision.ai)로 이동하여 __로그인__ 을 선택합니다. Azure portal에 로그인 할 때 사용한 동일한 계정으로 로그인 합니다.

![로그인 페이지 이미지](./media/browser-home.png)


1. 첫 번째 프로젝트를 만들려면 **새 프로젝트**를 선택합니다. **새 프로젝트 만들기** 대화 상자가 표시됩니다.

    ![새 프로젝트 대화 상자에는 이름, 설명 및 도메인 필드가 있습니다.](./media/getting-started-build-a-classifier/new-project.png)

1. 프로젝트에 대한 이름과 설명을 입력합니다. 그런 다음 리소스 그룹을 선택합니다. 로그인한 계정이 Azure 계정과 연결되어 있으면 리소스 그룹 드롭다운에는 Custom Vision Service 리소스를 포함하는 모든 Azure 리소스 그룹이 표시됩니다. 

   > [!NOTE]
   > 사용 가능한 리소스 그룹이 없는 경우, [Azure portal](https://portal.azure.com/)에 로그인할 때 사용한 동일한 계정으로 [customvision.ai](https://customvision.ai) 로그인했는지 확인하십시오. 또한 Custom Vision 리소스가 있는 Azure Portal의 디렉터리와 동일한 “디렉터리”를 Custom Vision 포털에서 선택했는지 확인합니다. 두 사이트 모두, 화면 오른쪽 위에 있는 드롭다운 계정 메뉴에서 디렉터리를 선택할 수 있습니다. 

1. __프로젝트 형식__ 아래에서 __분류__ 를 선택합니다. 그런 다음 __분류 유형__ 아래에서 사용 사례에 따라 **다중 레이블** 또는 **다중 클래스**를 선택합니다. 다중 레이블 분류에서는 0개 이상의 태그가 이미지에 적용되는 반면 다중 클래스 분류에서는 이미지가 단일 범주로 정렬됩니다. 즉, 제출하는 모든 이미지가 가장 적합한 태그로 정렬됩니다. 원하는 경우 나중에 분류 유형을 변경할 수 있습니다.

1. 다음으로 사용 가능한 도메인 중 하나를 선택합니다. 각 도메인은 다음 표에 설명된 대로 특정 이미지 유형에 맞게 분류자를 최적화합니다. 원하는 경우 나중에 도메인을 변경할 수 있습니다.

    |Domain|목적|
    |---|---|
    |__일반__| 광범위한 이미지 분류 작업에 최적화되었습니다. 다른 도메인이 적절하지 않거나 선택할 도메인을 잘 모르겠으면 일반 도메인을 선택합니다. |
    |__음식__|식당 메뉴에 표시되는 것 같은 음식 사진에 최적화되었습니다. 개별 과일이나 채소 사진을 분류하려면 음식 도메인을 사용합니다.|
    |__랜드마크__|인식 가능한 자연적 및 인공적인 랜드마크에 최적화되었습니다. 이 도메인은 랜드마크가 사진에서 명확하게 표시될 때 가장 효과적입니다. 앞에 서 있는 사람이 랜드마크를 약간 가리고 있는 경우에도 도메인이 작동합니다.|
    |__소매__|쇼핑 카탈로그 또는 쇼핑 웹 사이트에 있는 이미지에 최적화되었습니다. 드레스, 바지 및 셔츠를 높은 정밀도로 분류하려는 경우 이 도메인을 사용합니다.|
    |__압축 도메인__| 모바일 디바이스의 실시간 분류 제약 조건에 최적화되었습니다. 압축 도메인에서 생성된 모델을 로컬에서 실행하기 위해 내보낼 수 있습니다.|

1. 마지막으로 __프로젝트 만들기__ 를 선택합니다.

## <a name="choose-training-images"></a>학습 이미지 선택

최소한 초기 학습 세트에서는 태그당 이미지 30개 이상을 사용하는 것이 좋습니다. 학습된 모델을 테스트하기 위해 이미지 몇 개를 추가로 수집할 수도 있습니다.

모델을 효과적으로 학습시키려면 다양한 시각적 이미지를 사용합니다. 다음과 같은 요소가 각기 다른 이미지를 선택합니다.
* 카메라 각도
* 조명
* background
* 표시 스타일
* 개별/그룹화된 피사체
* size
* 형식

또한 모든 학습 이미지가 다음 기준을 충족하는지 확인합니다.
* .jpg, .png, .bmp 형식
* 크기 6MB 이하(예측 이미지의 경우 4MB)
* 가장 짧은 가장자리가 256픽셀 이상. 256픽셀보다 짧은 이미지는 Custom Vision Service에서 자동으로 확장됩니다.

## <a name="upload-and-tag-images"></a>이미지 업로드 및 태그 지정

이 섹션에서는 분류자 학습을 지원하기 위해 이미지를 업로드하고 수동으로 태그를 지정합니다. 

1. 이미지를 추가하려면 __이미지 추가__ 단추를 클릭하고 __로컬 파일 찾아보기__ 를 선택합니다. __열기__ 를 선택하여 태그 지정으로 이동합니다. 선택한 태그는 업로드하도록 선택한 전체 이미지 그룹에 적용되므로 원하는 태그에 따라 개별 그룹으로 이미지를 업로드하는 것이 더 쉽습니다. 개별 이미지를 업로드한 후에 태그를 변경할 수도 있습니다.

    ![이미지 추가 컨트롤은 왼쪽 위와 맨 아래 가운데 단추로 표시됩니다.](./media/getting-started-build-a-classifier/add-images01.png)


1. 태그를 만들려면 __내 태그__ 필드에 텍스트를 입력하고 Enter 키를 누릅니다. 태그가 이미 있는 경우 드롭다운 메뉴에 표시됩니다. 다중 레이블 프로젝트에서는 이미지에 태그를 여러 개 추가할 수 있지만 다중 클래스 프로젝트에서는 태그를 하나만 추가할 수 있습니다. 이미지 업로드를 완료하려면 __[수]개 파일 업로드__ 단추를 사용합니다. 

    ![태그 및 업로드 페이지 이미지](./media/getting-started-build-a-classifier/add-images03.png)

1. 이미지가 업로드되면 __완료__ 를 선택합니다.

    ![진행 표시줄에 완료된 작업이 모두 표시됩니다.](./media/getting-started-build-a-classifier/add-images04.png)

다른 이미지 세트를 업로드하려면 이 섹션의 맨 위로 돌아가서 해당 단계를 반복합니다.

## <a name="train-the-classifier"></a>분류자 학습

분류자를 학습하려면 **학습** 단추를 선택합니다. 분류자는 현재 이미지를 모두 사용하여 각 태그의 시각적 품질을 식별하는 모델을 만듭니다.

![웹 페이지 헤더 도구 모음 오른쪽 상단의 학습 단추](./media/getting-started-build-a-classifier/train01.png)

학습 프로세스는 몇 분밖에 안 걸립니다. 이 시간 동안에는 학습 프로세스에 대한 정보가 **성능** 탭에 표시됩니다.

![주 섹션에 학습 대화 상자가 표시된 브라우저 창](./media/getting-started-build-a-classifier/train02.png)

## <a name="evaluate-the-classifier"></a>분류자 평가

학습이 완료되고 나면 모델의 예상 성능이 계산되어 표시됩니다. Custom Vision Service는 학습용으로 제출된 이미지를 사용하여 [k중 교차 유효성 검사](https://en.wikipedia.org/wiki/Cross-validation_(statistics))라는 프로세스를 통해 정밀도와 재현율을 계산합니다. 정밀도와 재현율은 분류자의 효율성을 나타내는 각기 다른 두 측정값입니다.

- **정밀도**는 정확한 것으로 식별된 분류 부분을 나타냅니다. 예를 들어 모델에서 100개 이미지를 강아지로 식별했는데 그중 99개가 실제로 강아지 이미지였다면 정밀도는 99%입니다.
- **재현율**은 정확하게 식별된 실제 분류 부분을 나타냅니다. 예를 들어 실제 사과 이미지가 100개인데 모델이 80개를 사과로 식별했다면 재현율은 80%입니다.

![학습 결과는 전체 정밀도 및 재현율과 분류자의 각 태그에 대한 정밀도 및 재현율을 보여 줍니다.](./media/getting-started-build-a-classifier/train03.png)

### <a name="probability-threshold"></a>확률 임계값

**성능** 탭의 왼쪽 창에는 **확률 임계값** 슬라이더가 있습니다. 이 슬라이더는 정밀도와 재현율을 계산할 때 정확한 것으로 간주할 예측 확률의 임계값입니다.

높은 확률 임계값을 사용해 예측 호출을 해석하면 반환되는 결과의 정밀도는 높은 대신 재현율은 낮아지는 경향이 있습니다(발견된 분류는 정확하지만 발견되지 않은 항목이 많음). 반면 낮은 확률 임계값을 사용하는 경우에는 실제 분류가 대부분 발견되기는 하지만 세트 내에 가양성이 많아집니다. 이 점을 고려하여 프로젝트의 구체적인 요구에 따라 예측 임계값을 설정해야 합니다. 나중에 클라이언트 쪽에서 모델의 예측 결과를 수신할 때도 같은 예측 임계값을 필터로 사용해야 합니다.

## <a name="manage-training-iterations"></a>학습 반복 관리

분류자를 학습시킬 때마다 업데이트된 고유 성능 메트릭을 사용하여 새로운 _반복_ 이 생성됩니다. **성능** 탭의 왼쪽 창에서 모든 반복을 확인할 수 있습니다. 왼쪽 창에는 사용하지 않는 반복을 삭제하는 데 사용할 수 있는 **삭제** 단추도 있습니다. 반복을 삭제하면 해당 반복에 고유하게 연결된 이미지도 모두 삭제됩니다.

## <a name="next-steps"></a>다음 단계

이 가이드에서는 Custom Vision 웹 사이트를 사용하여 이미지 분류 모델을 만들고 학습시키는 방법을 알아보았습니다. 다음으로는 모델을 개선하는 반복 프로세스에 대해 자세히 알아봅니다.

[모델 테스트 및 재교육](test-your-model.md)

