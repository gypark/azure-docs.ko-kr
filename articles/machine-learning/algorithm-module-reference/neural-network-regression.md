---
title: '신경망 회귀: 모듈 참조'
titleSuffix: Azure Machine Learning service
description: 사용자 지정 가능한 신경망 알고리즘을 사용 하 여 회귀 모델을 만드는 Azure Machine Learning 서비스에서 신경망 회귀 모듈을 사용 하는 방법에 알아봅니다.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: bc6a7505ab09e929e5add61eea687f871aedf242
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029312"
---
# <a name="neural-network-regression-module"></a>신경망 회귀 모듈

*신경망 알고리즘을 사용 하 여 회귀 모델 만들기*  
  
 범주: 기계 학습 / 모델 초기화 / 회귀
  
## <a name="module-overview"></a>모듈 개요  

이 문서에서는 Azure Machine Learning 서비스에 대 한 시각적 인터페이스 (미리 보기)의 모듈을 설명 합니다.

이 모듈을 사용 하 여 사용자 지정 가능한 신경망 알고리즘을 사용 하 여 회귀 모델을 만듭니다.
  
 신경망 이미지 인식과 같은 복잡 한 문제의 모델링과 심층 학습에서 사용 하기 위해 널리 알려져 있지만 회귀 문제에 맞게 쉽게 조정 됩니다. 모든 클래스의 통계 모델 적응 가중치를 사용 하며 해당 입력의 비선형 함수를 대략적인 수 하는 경우 신경망을 라고 할 수 있습니다. 따라서 신경망 회귀는 보다 전통적인 회귀 모델을 솔루션에 맞게 수 없습니다. 문제에 적합 합니다.
  
 신경망 회귀는 지도 학습 방법 이며 하므로 *데이터 집합 태그*, 레이블 열을 포함 하는 합니다. 회귀 모델을 숫자 값을 예측 하기 때문에 레이블 열을 숫자 데이터 형식 이어야 합니다.  
  
 모델 및 태그가 지정 된 데이터 집합에 대 한 입력으로 제공 하 여 모델을 학습할 수 있습니다 [모델 학습](./train-model.md)합니다. 그런 다음 학습 된 모델은 새 입력된 예제에 대 한 값을 예측 하 사용할 수 있습니다.  
  
## <a name="configure-neural-network-regression"></a>신경망 회귀를 구성 합니다. 

신경망은 광범위 하 게 사용자 지정할 수 있습니다. 이 섹션에는 두 메서드를 사용 하 여 모델을 만드는 방법을 설명 합니다.
  
+ [기본 아키텍처를 사용 하 여 신경망 모델 만들기](#bkmk_DefaultArchitecture)  
  
    기본 신경망 아키텍처를 수락 하면 사용 합니다 **속성** 창은 신경망, 숨겨진된 계층, 학습 속도 및 정규화에 노드의 수와 같은 동작을 제어 하는 매개 변수를 설정 합니다.

    신경망을 처음 접하는 경우 여기에서 시작 합니다. 모듈에는 많은 사용자 지정 항목 뿐만 아니라 모델 튜닝, 심층적인 신경망 없이 지원 합니다. 

+ 신경망에 대 한 사용자 지정 아키텍처를 정의 합니다. 

    추가 숨겨진된 계층을 추가 하거나 완전히 네트워크 아키텍처, 연결 및 활성화 함수를 사용자 지정 하려는 경우이 옵션을 사용 합니다.
    
    이 옵션은 이미 신경망을 사용 하 여 어느 정도 알고 있다면 가장 적합 합니다. Net # 언어를 사용 하 여 네트워크 아키텍처를 정의 합니다.  

##  <a name="bkmk_DefaultArchitecture"></a> 기본 아키텍처를 사용 하 여 신경망 모델 만들기
  
1.  추가 된 **신경망 회귀** 모듈 인터페이스에서 실험을 합니다. 이 모듈에서 찾을 수 있습니다 **Machine Learning**, **초기화**에서 합니다 **회귀** 범주입니다. 
  
2. 모델을 설정 하 여 학습 하는 방법을 지정 합니다 **트레이너 모드 만들기** 옵션입니다.  
  
    -   **매개 변수를 단일**: 모델을 구성 하려는 방법을 알고 있는 경우이 옵션을 선택 합니다.  

3.  **숨겨진 계층 사양**를 선택 **완벽 하 게 사례를 연결**합니다. 신경망 회귀 모델의 경우 이러한 특성을가지고 있는 기본 신경망 아키텍처를 사용 하는 모델을 만듭니다.  
  
    + 네트워크에 정확히 하나의 숨겨진된 계층이 있습니다.
    + 출력 계층은 숨겨진된 계층에 완전히 연결 하 고 숨겨진된 계층은 입력된 계층에 완전히 연결 됩니다.
    + 사용자가 숨겨진된 계층의 노드 수를 설정할 수 있습니다 (기본값은 100).  
  
    입력된 계층의 노드 수는 학습 데이터의 기능 수에 따라 결정 되므로, 회귀 모델에서에 있을 수 있습니다 하나의 노드만 출력 계층입니다.  
  
4. 에 대 한 **숨겨진 노드의 수**, 숨겨진 노드의 수를 입력 합니다. 기본값은 100 노드와 하나의 숨겨진된 계층이 있습니다. (이 옵션은 Net #을 사용 하 여 사용자 지정 아키텍처를 정의 하는 경우 사용할 수 있습니다.)
  
5.  에 대 한 **학습 속도**를 수정 하기 전에 각 반복에서 수행 되는 단계를 정의 하는 값을 입력 합니다. 학습 속도 값이 크면 모델이 빠르게 수렴 하면 되지만 로컬 최소값이 과도 복원 지점을 지나친 수 있습니다.

6.  에 대 한 **학습 반복 횟수**, 알고리즘이 학습 사례를 처리 하는 최대 횟수를 지정 합니다.

7.  에 대 한 * * 초기 학습 가중치 지름, 학습 프로세스의 시작 부분에 노드 가중치를 결정 하는 값을 입력 합니다.

8.  에 대 한 **운동량**, 이전 반복에서 노드의 가중치를 학습 중에 적용할 값을 입력 합니다.

10. 옵션을 선택 **예제 섞기**반복 간 사례의 순서를 변경 합니다. 이 옵션의 선택을 취소 하면 경우 실험을 실행할 때마다 정확히 동일한 순서로 처리 됩니다.
  
11. 에 대 한 **임의 숫자 초기값**를 초기값으로 사용할 값을 입력할 수도 있습니다. 초기값을 지정 값은 유용 동일한 실험의 실행에서 반복성을 유지 하려는 경우.
  
13. 학습 데이터 집합 및 중 하나에 연결 합니다 [학습 모듈](module-reference.md): 
  
    -   설정 하는 경우 **트레이너 모드 만들기** 하 **단일 매개 변수**를 사용 하 여 [모델 학습](./train-model.md)합니다.  
  
   
14. 실험을 실행합니다.  

## <a name="results"></a>결과

완료 된 후 교육:

+ 가중치를 학습 및 신경망 네트워크의 다른 매개 변수에서 배운 기능와 함께 모델의 매개 변수 요약을 볼 출력을 마우스 오른쪽 단추로 클릭 [모델 학습](./train-model.md), 선택한 **시각화**.  

+ 학습된 된 모델의 스냅숏을 저장 하려면 마우스 오른쪽 단추로 클릭 합니다 **Trained 모델** 선택한 출력 **학습 된 모델을 저장**합니다. 이 모델은 동일한 실험의 후속 실행 시 업데이트 되지 않습니다.


## <a name="next-steps"></a>다음 단계

참조를 [사용할 수 있는 모듈 집합](module-reference.md) Azure Machine Learning 서비스입니다. 