---
title: Azure 음성 서비스를 사용 하 여 음성 번역
titlesuffix: Azure Cognitive Services
description: 음성 서비스를 통해 응용 프로그램, 도구 및 장치와 엔드-투-엔드, 실시간, 다국어 음성 번역을 추가할 수 있습니다. 같은 API를 음성 대 음성 및 음성 대 텍스트 번역 모두에 사용될 수 있습니다.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 95682612b4b0fdb1baa5038039630e74abddb1a9
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2019
ms.locfileid: "57890478"
---
# <a name="what-is-speech-translation"></a>음성 번역 란?

Azure 음성 서비스의 음성 번역을 오디오 스트림 음성-음성 및 음성-텍스트 번역을 실시간, 다중 언어 수 있습니다. Speech SDK를 사용 하 여 응용 프로그램, 도구 및 장치에 액세스할 수 있으며 원본 기록 및 변환 출력에 제공 된 오디오에 대 한 기록 및 번역에 대 한 중간 결과는 음성이 감지 되는, 및 결승전 결과 합성 된 음성으로 변환 될 수 반환 됩니다.

Microsoft의 변환 엔진은 두 가지 방법을 통해 제공 됩니다: 통계 기계 번역 (SMT) 및 중립 기계 번역 (NMT). SMT 단어 몇 개만 컨텍스트에서 최상의 가능한 번역을 예측 하려면 고급 통계 분석을 사용 합니다. NMT를 사용 하 여 신경망 단어 변환할 문장의 전체 컨텍스트를 사용 하 여 보다 정확 하 고, 자연 발음이 번역을 제공 하는 데 사용 됩니다.

현재 Microsoft는 가장 인기 있는 언어로 번역에 대 한 NMT를 사용합니다. [음성 대 음성 번역에 사용할 수 있는 모든 언어](language-support.md#speech-translation)는 NMT를 통해 지원됩니다. 음성 대 텍스트 번역은 언어 쌍에 따라 SMT 또는 NMT를 사용할 수 있습니다. 대상 언어 NMT에서 지원 되는 경우 전체 변환은 NMT 기반 합니다. 변환 대상 언어 NMT에서 지원 되지 않습니다 때 NMT 및 두 언어 사이의 "피벗"으로 영어를 사용 하 여 SMT를 혼합 합니다.

## <a name="core-features"></a>핵심 기능

다음은 Speech SDK 및 REST Api를 통해 사용 가능한 기능:

| 사용 사례 | SDK) | REST (영문) |
|----------|-----|------|
| 인식 결과 사용 하 여 음성-텍스트를 번역 합니다. | 예 | 아닙니다. |
| 음성-음성 번역입니다. | 예 | 아닙니다. |
| 중간 인식 및 변환 결과입니다. | 예 | 아닙니다. |

## <a name="get-started-with-speech-translation"></a>음성 번역을 사용 하 여 시작

10 분 이내에 코드를 실행 하는 경우 제공 하도록 설계 되었습니다 빠른 시작을 제공 합니다. 이 테이블에는 음성 번역 언어를 이끄는 빠른 시작의 목록을 포함 합니다.

| 빠른 시작 | 플랫폼 | API 참조 |
|------------|----------|---------------|
| [C#, .NET Core](quickstart-translate-speech-dotnetcore-windows.md) | Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| [C#, .NET Framework](quickstart-translate-speech-dotnetframework-windows.md) | Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| [C#, UWP](quickstart-translate-speech-uwp.md) | Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| [C++](quickstart-translate-speech-cpp-windows.md) | Windows | [Browse](https://aka.ms/csspeech/cppref)|
| [Java](quickstart-translate-speech-java-jre.md) | Windows | [Browse](https://aka.ms/csspeech/javaref) |

## <a name="sample-code"></a>샘플 코드

Speech SDK에 대 한 예제 코드는 GitHub에서 사용할 수 있습니다. 이러한 샘플 파일 또는 스트림을 지속적이 고 단일 샷 인식/변환에서 오디오를 읽는 사용자 지정 모델을 사용 하 여 작업 등 일반적인 시나리오를 다룹니다.

* [음성-텍스트 및 변환 샘플 (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)

## <a name="migration-guides"></a>마이그레이션 가이드

> [!WARNING]
> Translator Speech 2019 년 10 월 15 일에 폐기 될 예정입니다.

응용 프로그램, 도구 또는 제품을 사용 하는 Translator Speech 음성 서비스를 마이그레이션할 수 있도록 가이드를 만들었습니다.

* [Translator Speech API에서에서 Speech Services로 마이그레이션](how-to-migrate-from-translator-speech-api.md)

## <a name="reference-docs"></a>참조 문서

* [Speech SDK](speech-sdk-reference.md)
* [Speech Devices SDK](speech-devices-sdk.md)
* [REST API: Speech-to-text](rest-speech-to-text.md)
* [REST API: 텍스트 음성 변환](rest-text-to-speech.md)
* [REST API: 일괄 처리 기록 및 사용자 지정](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>다음 단계

* [음성 서비스 등록 키를 위한 무료](get-started.md)
* [Speech SDK 가져오기](speech-sdk.md)
