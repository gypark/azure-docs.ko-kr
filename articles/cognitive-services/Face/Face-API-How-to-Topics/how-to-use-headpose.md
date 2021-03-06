---
title: HeadPose 얼굴 사각형에 맞게 사용
titleSuffix: Azure Cognitive Services
description: 자동으로 얼굴 사각형을 회전 HeadPose 특성을 사용 하는 방법에 알아봅니다.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: pafarley
ms.openlocfilehash: ddc5bc522c0d3ac258581f2a48a5c3b755302f01
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/26/2019
ms.locfileid: "64576524"
---
# <a name="use-the-headpose-attribute-to-adjust-the-face-rectangle"></a>얼굴 사각형에 맞게 HeadPose 특성을 사용 합니다.

이 가이드에서는 얼굴 개체의 사각형을 회전하기 위해 감지된 얼굴 특성 HeadPose를 사용합니다. 이 가이드에서 샘플 코드인 [Cognitive Services Face WPF](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) 샘플 앱에서.NET SDK를 사용 합니다.

각 감지된 얼굴과 함께 반환된 얼굴 사각형은 이미지에서 얼굴의 크기와 위치를 표시합니다. 기본적으로 사각형은 이미지에 항상 맞추어집니다(각 면은 완벽하게 수평 및 수직). 각진 얼굴 프레이밍에 대해서는 비효율적일 수 있습니다. 프로그래밍 방식으로 이미지에서 얼굴을 자르려면, 자르기 위해 사각형을 회전하는 것이 유용합니다.

## <a name="explore-the-sample-code"></a>샘플 코드 탐색

프로그래밍 방식으로 HeadPose 특성을 사용하여 얼굴 사각형을 회전할 수 있습니다. 얼굴을 감지([얼굴을 감지하는 방법](HowtoDetectFacesinImage.md) 참조)하는 경우 이 특성을 지정하게 되면, 나중에 쿼리할 수 있습니다. [Cognitive Services Face WPF](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) 앱에서 다음 메서드는 **DetectedFace** 개체의 목록을 가져오고 **[Face](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/app-samples/Cognitive-Services-Face-WPF/Sample-WPF/Controls/Face.cs)** 개체의 목록을 반환합니다. 여기서 **Face** 는 업데이트된 사각형 좌표를 포함하여 얼굴 데이터를 저장하는 사용자 지정 클래스입니다. 에 대 한 새 값은 계산 **위쪽**, **왼쪽**를 **너비**, 및 **높이**, 새 필드와 **FaceAngle**회전을 지정 합니다.

```csharp
/// <summary>
/// Calculate the rendering face rectangle
/// </summary>
/// <param name="faces">Detected face from service</param>
/// <param name="maxSize">Image rendering size</param>
/// <param name="imageInfo">Image width and height</param>
/// <returns>Face structure for rendering</returns>
public static IEnumerable<Face> CalculateFaceRectangleForRendering(IList<DetectedFace> faces, int maxSize, Tuple<int, int> imageInfo)
{
    var imageWidth = imageInfo.Item1;
    var imageHeight = imageInfo.Item2;
    var ratio = (float)imageWidth / imageHeight;
    int uiWidth = 0;
    int uiHeight = 0;
    if (ratio > 1.0)
    {
        uiWidth = maxSize;
        uiHeight = (int)(maxSize / ratio);
    }
    else
    {
        uiHeight = maxSize;
        uiWidth = (int)(ratio * uiHeight);
    }

    var uiXOffset = (maxSize - uiWidth) / 2;
    var uiYOffset = (maxSize - uiHeight) / 2;
    var scale = (float)uiWidth / imageWidth;

    foreach (var face in faces)
    {
        var left = (int)(face.FaceRectangle.Left * scale + uiXOffset);
        var top = (int)(face.FaceRectangle.Top * scale + uiYOffset);

        // Angle of face rectangles, default value is 0 (not rotated).
        double faceAngle = 0;

        // If head pose attributes have been obtained, re-calculate the left & top (X & Y) positions.
        if (face.FaceAttributes?.HeadPose != null)
        {
            // Head pose's roll value acts directly as the face angle.
            faceAngle = face.FaceAttributes.HeadPose.Roll;
            var angleToPi = Math.Abs((faceAngle / 180) * Math.PI);

            // _____       | / \ |
            // |____|  =>  |/   /|
            //             | \ / |
            // Re-calculate the face rectangle's left & top (X & Y) positions.
            var newLeft = face.FaceRectangle.Left +
                face.FaceRectangle.Width / 2 -
                (face.FaceRectangle.Width * Math.Sin(angleToPi) + face.FaceRectangle.Height * Math.Cos(angleToPi)) / 2;

            var newTop = face.FaceRectangle.Top +
                face.FaceRectangle.Height / 2 -
                (face.FaceRectangle.Height * Math.Sin(angleToPi) + face.FaceRectangle.Width * Math.Cos(angleToPi)) / 2;

            left = (int)(newLeft * scale + uiXOffset);
            top = (int)(newTop * scale + uiYOffset);
        }

        yield return new Face()
        {
            FaceId = face.FaceId?.ToString(),
            Left = left,
            Top = top,
            OriginalLeft = (int)(face.FaceRectangle.Left * scale + uiXOffset),
            OriginalTop = (int)(face.FaceRectangle.Top * scale + uiYOffset),
            Height = (int)(face.FaceRectangle.Height * scale),
            Width = (int)(face.FaceRectangle.Width * scale),
            FaceAngle = faceAngle,
        };
    }
}
```

## <a name="display-the-updated-rectangle"></a>업데이트된 사각형 표시

여기에서 반환된 **Face** 개체를 사용하여 표시할 수 있습니다. [FaceDetectionPage.xaml](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/app-samples/Cognitive-Services-Face-WPF/Sample-WPF/Controls/FaceDetectionPage.xaml) 의 다음 줄은 이 데이터로부터 새 사각형이 렌더링되는 방법을 보여줍니다.

```xaml
 <DataTemplate>
    <Rectangle Width="{Binding Width}" Height="{Binding Height}" Stroke="#FF26B8F4" StrokeThickness="1">
        <Rectangle.LayoutTransform>
            <RotateTransform Angle="{Binding FaceAngle}"/>
        </Rectangle.LayoutTransform>
    </Rectangle>
</DataTemplate>
```

## <a name="next-steps"></a>다음 단계

회전된 얼굴 사각형의 작업 예제는 GitHub의 [Cognitive Services Face WPF](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF)앱을 참조합니다. [Face API HeadPose 샘플](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples) 앱은 다양한 머리 이동(고개를 끄떡이는, 머리를 흔드는)을 감지하기 위해 실시간으로 HeadPose 특성을 추적 하는 앱입니다.