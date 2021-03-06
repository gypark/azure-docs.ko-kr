---
title: '2 클래스 승격 된 의사 결정 트리: 모듈 참조'
titleSuffix: Azure Machine Learning service
description: 기계 학습의 승격 된 의사 결정 트리 알고리즘을 기반으로 하는 모델을 만들려면 Azure Machine Learning 서비스에서 2 클래스 승격 된 의사 결정 트리 모듈을 사용 하는 방법에 알아봅니다.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 09ea530cac5bdbd62208f134177e5ceaccb545e2
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027947"
---
# <a name="two-class-boosted-decision-tree-module"></a>2 클래스 승격 된 의사 결정 트리 모듈

이 문서에서는 Azure Machine Learning 서비스에 대 한 시각적 인터페이스 (미리 보기)의 모듈을 설명 합니다.

이 모듈을 사용 하 여 승격 된 의사 결정 트리 알고리즘을 기반으로 하는 기계 학습 모델을 만듭니다. 

승격 된 의사 결정 트리는 첫 번째와 두 번째 트리 등의 오류에 대 한 세 번째 트리를 수정 앙상블 학습 방법은 첫 번째 트리 오류에 대 한 두 번째 트리 수정 합니다.  예측을 예측 하는 전체 함께 트리 앙상블을 기반으로 합니다.
  
일반적으로 제대로 구성 되었으면 승격 된 의사 결정 트리는 다양 한 기계 학습 작업에서 최고의 성능을 얻을 수 있는 가장 쉬운 방법입니다. 그러나도 메모리를 많이 학습자 중 하나 이며 현재 구현에서는 모든 항목을 메모리에 저장 합니다. 따라서 향상 된 의사 결정 트리 모델을 일부 선형 학습자가 처리할 수 있는 큰 데이터 집합을 처리 하는 데 못할 수 있습니다.

## <a name="how-to-configure"></a>구성 방법

이 모듈은 학습 되지 않은 분류 모델을 만듭니다. 분류 모델을 학습 하는 지도 학습 방법 이므로 해야는 *데이터 집합을 태그가 지정 된* 모든 행에 대 한 값을 사용 하 여 레이블 열을 포함 하는 합니다.

이 유형의 사용 하 여 모델을 학습 시킬 [모델 학습](././train-model.md)합니다. 

1.  Azure Machine Learning을 추가 합니다 **승격 된 의사 결정 트리** 모듈을 실험 합니다.
  
2.  설정 하 여 학습 모델을 지정 합니다 **트레이너 모드 만들기** 옵션입니다.
  
    + **매개 변수를 단일**: 모델을 구성 하려는 방법를 알고 있으면 특정 값 집합을 인수로 제공할 수 있습니다.
  
  
3.  에 대 한 **트리 당 리프의 최대 수**, 트리의 만들 수 있는 터미널 노드 (리프)의 최대 개수를 나타냅니다.
  
     이 값을 늘려 잠재적으로 트리의 크기를 늘릴을 높아지지만 학습 시간이 길어지고 과잉 맞춤이 향상 전체 자릿수를 가져옵니다.
  
4.  에 대 한 **샘플 리프 노드당 최소**, 트리의 터미널 노드 (리프)를 만드는 데 필요한 최대 사례 수를 나타냅니다.  
  
     이 값을 늘려 새 규칙 만들기에 대 한 임계값을 늘려야 합니다. 예를 들어, 단일 사례도 1의 기본값을 사용 하 여 새 규칙을 만드는 발생할 수 있습니다. 값을 5로 늘리면 학습 데이터에 동일한 조건을 만족 하는 사례가 다섯 개 이상 포함 해야 합니다.
  
5.  에 대 한 **학습 속도**를 학습 하는 동안 단계 크기를 정의 하는 0과 1 사이의 숫자를 입력 합니다.  
  
     학습 속도 학습자 최적의 솔루션에 수렴 속도 결정 합니다. 단계 크기가 너무 큰 경우 최적의 솔루션 복원 지점을 지나친 수 있습니다. 단계 크기를 너무 작은 경우 학습에 가장 적합 한 솔루션에 수렴 오래 걸립니다.
  
6.  에 대 한 **생성 되는 트리의 수**, 의사 결정 트리 앙상블에서 만들려는 총 수를 나타냅니다. 추가 의사 결정 트리를 만들면 적용 범위가 확대를 잠재적으로 가져올 수 있지만 학습 시간이 증가 됩니다.
  
     이 값에는 또한 학습된 된 모델을 시각화 하는 경우에 표시 되는 트리 수를 제어 합니다. 참조 또는 단일 트리를 인쇄 하려는 경우 값 1로 설정 합니다. 그러나 이렇게 하면 하나의 트리만 생성 됩니다 (초기 매개 변수 집합을 사용 하 여 트리) 추가 반복에서 수행 됩니다.
  
7.  에 대 한 **임의 숫자 초기값**에서 필요에 따라 난수 초기값으로 사용할 음수가 아닌 정수를 입력 합니다. 동일한 데이터 및 매개 변수는 실행 간의 재현 가능성을 보장 초기값을 지정 합니다.  
  
     무작위 초기값 0으로, 시스템 클록에서 최초 초기값 가져온 의미를 기본적으로 설정 됩니다.  임의 초기값을 사용 하는 연속 실행 다른 결과 가질 수 있습니다.
  

9. 모델을 학습 합니다.
  
    + 설정 하는 경우 **트레이너 모드 만들기** 에 **단일 매개 변수**, 태그가 지정 된 데이터 집합에 연결 하며 [모델 학습](./train-model.md) 모듈입니다.  
  
   
## <a name="results"></a>결과

모델 학습 완료 되 면 출력을 마우스 오른쪽 단추로 클릭 [모델 학습](./train-model.md) 결과 보려면:

+ 각 반복에 대해 생성 된 트리를 보려면 선택 **시각화**합니다. 
+ 분할으로 드릴 다운 하 고 각 노드에 대 한 규칙을 참조 하려면 각 트리를 클릭 합니다.


## <a name="next-steps"></a>다음 단계

참조를 [사용할 수 있는 모듈 집합](module-reference.md) Azure Machine Learning 서비스입니다. 