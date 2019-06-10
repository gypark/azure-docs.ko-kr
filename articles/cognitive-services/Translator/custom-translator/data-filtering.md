---
title: 데이터 필터링 - Custom Translator
titleSuffix: Azure Cognitive Services
description: 사용자 지정 시스템을 학습하는 데 사용할 문서를 제출하면 문서는 일련의 처리 및 필터링 단계를 거쳐 학습을 준비합니다.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: v-pawal
ms.topic: conceptual
ms.openlocfilehash: ba6d1ed5bc26f121f32fad548e7a17c1f4998851
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66389349"
---
# <a name="data-filtering"></a>데이터 필터링

사용자 지정 시스템을 학습하는 데 사용할 문서를 제출하면 문서는 일련의 처리 및 필터링 단계를 거쳐 학습을 준비합니다. 이 단계는 여기에서 설명합니다. 필터링에 대한 지식이 있으면 Custom Translator에 표시되는 문장 수를 파악하고 Custom Translator를 사용하여 학습용 문서를 직접 준비하는 단계를 이해할 수 있습니다.

## <a name="sentence-alignment"></a>문장 맞춤
문서가 XLIFF, TMX 또는 ALIGN 형식이 아닌 경우 Custom Translator는 원본 및 대상 문서의 문장을 서로 문장 단위로 정렬합니다. Translator는 문서 정렬을 수행하지 않으며 다른 언어의 일치하는 문서를 찾기 위해 문서의 이름 지정을 따릅니다. 문서 내에서 Custom Translator는 다른 언어로 해당 문장 찾기를 시도합니다. 포함된 HTML 태그와 같은 문서 태그를 사용하여 정렬을 지원합니다.  

표시 한 경우 원본에는 문장 수 사이 큰 차이가 대상 쪽 문서, 문서 않았을 병렬 처음에 또는 다른 이유로 정렬할 수 없습니다. 각각에서 문장 수 차이가 10%를 초과하는 문서 쌍은 반드시 재차 확인하여 실제로 병행되었는지 확인합니다. Custom Translator는 문장 수가 다른 것으로 의심되는 경우 문서 옆에 경고를 표시합니다.  


## <a name="deduplication"></a>중복 제거
Custom Translator는 학습 데이터에서 테스트 및 튜닝 문서에 제시된 문장을 제거합니다. 제거는 데이터 처리 단계가 아닌, 학습 실행 내에서 동적으로 수행됩니다. Custom Translator는 이러한 제거 작업 전에 문장 수를 프로젝트 개요에서 사용자에게 보고합니다.  

## <a name="length-filter"></a>길이 필터
* 원본 및 대상 중 한쪽에 한 단어만 있는 문장을 제거합니다.
* 한쪽에 100개를 초과하는 단어가 있는 문장을 제거합니다.  중국어, 일본어, 한국어는 제외됩니다.
* 3자 미만의 문장을 제거합니다. 중국어, 일본어, 한국어는 제외됩니다.
* 중국어, 일본어, 한국어에서 2,000자를 초과하는 문장을 제거합니다.
* 영문자가 1% 미만 있는 문장을 제거합니다.
* 포함된 단어가 50개를 초과하는 사전 항목을 제거합니다.

## <a name="white-space"></a>공백
* 탭 및 CR/LF 문자를 포함한 공백 문자 시퀀스를 단일 공백 문자로 바꿉니다.
* 문장에서 선행 또는 후행 공백을 제거합니다.

## <a name="sentence-end-punctuation"></a>문장 종료 부호
여러 문장 종료 부호 문자를 단일 항목으로 바꿉니다.  

## <a name="japanese-character-normalization"></a>일본어 문자 정규화
반자 문자를 전체 너비의 문자 및 숫자를 변환 합니다.

## <a name="unescaped-xml-tags"></a>이스케이프되지 않은 XML 태그
필터링은 이스케이프되지 않은 태그를 이스케이프된 태그로 변환합니다.
* `&lt;`는 `&amp;lt;`가 됩니다.
* `&gt;`는 `&amp;gt;`가 됩니다.
* `&amp;`는 `&amp;amp;`가 됩니다.

## <a name="invalid-characters"></a>잘못된 문자
Custom Translator는 유니코드 문자 U+FFFD가 포함된 문장을 제거합니다. 문자 U+FFFD는 실패한 인코딩 변환을 나타냅니다.

## <a name="next-steps"></a>다음 단계

- Custom Translator에서 [모델 학습](how-to-train-model.md)
