---
title: 'Microsoft Azure Data Box Heavy 개요 | 데이터 Microsoft Docs '
description: 많은 양의 데이터를 Azure로 전송할 수 있는 하이브리드 솔루션인 Azure Data Box에 대해 설명합니다.
services: databox
documentationcenter: NA
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: overview
ms.date: 05/20/2019
ms.author: alkohli
ms.openlocfilehash: 0f71d9b4400041db50cb3e24940e922acde55edc
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65991698"
---
# <a name="what-is-azure-data-box-heavy-preview"></a>Azure Data Box Heavy란? (미리 보기)

Azure Data Box Heavy를 사용하면 수백 테라바이트의 데이터를 빠르고, 저렴하고, 신뢰할 수 있는 방법으로 Azure에 보낼 수 있습니다. 데이터로 채워 Microsoft로 다시 보내는 1PB 스토리지 용량의 Data Box Heavy 디바이스를 배송하여 데이터를 Azure로 전송합니다. 디바이스에는 운송 중에 데이터를 안전하게 보호하기 위한 견고한 케이스가 있습니다.

Data Box Heavy는 현재 미리 보기로 있습니다. Azure Portal을 통해 디바이스를 요청하려면 가입해야 합니다. 데이터 센터에서 디바이스를 받으면 로컬 웹 UI를 통해 해당 디바이스를 설정합니다. 서버에서 디바이스로 데이터를 복사하고 디바이스를 다시 Azure에 배송합니다. Azure 데이터 센터에서 데이터는 Azure Storage 계정에 업로드됩니다. Azure Portal에서 엔드투엔드 프로세스 전체를 추적할 수 있습니다.


> [!IMPORTANT]
> - Data Box Heavy는 미리 보기로 제공됩니다. 이 솔루션을 배포하기 전에 [미리 보기에 대한 Azure 서비스 약관](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)을 검토하세요.
> - 디바이스를 요청하려면 [미리 보기 포털](https://aka.ms/azuredatabox)에서 등록합니다.
> - 미리 보기 동안 Data Box Heavy는 미국 및 유럽 연합의 고객에게 배송될 수 있습니다. 자세한 내용은 [지역 가용성](#region-availability)을 참조하세요.

## <a name="use-cases"></a>사용 사례

네트워크 연결이 데이터를 Azure에 업로드하는 데 충분하지 않은 수백 테라바이트 단위의 데이터 크기에는 Data Box Heavy가 가장 적합합니다. 데이터 이동은 일회성 전송, 정기적 전송 또는 초기 대량 데이터 전송 후의 정기적 전송일 수 있습니다. Data Box Heavy를 데이터 전송에 사용할 수 있는 다양한 시나리오는 다음과 같습니다.

 - **일회성 마이그레이션** - 대량의 온-프레미스 데이터를 Azure로 이동하는 경우입니다.
     - 미디어 라이브러리를 오프라인 테이프에서 Azure로 이동하여 온라인 미디어 라이브러리를 만듭니다.
     - VM 팜, SQL 서버 및 애플리케이션을 Azure로 마이그레이션합니다.
     - 심층 분석을 위해 기록 데이터를 Azure로 이동하고 HDInsight를 사용하여 보고합니다.

 - **초기 대량 전송** - Data Box Heavy(초기값)를 사용하여 초기 대량 전송을 수행한 후에 네트워크를 통해 증분 전송이 수행되는 경우입니다.
     - 예를 들어 Data Box Heavy와 백업 솔루션 파트너는 초기 대량 기록 백업을 Azure로 이동하는 데 사용됩니다. 완료되면 증분 데이터가 Azure 저장소로 네트워크를 통해 전송됩니다.

 - **정기적 업로드** - 대량의 데이터를 정기적으로 생성하고 Azure로 이동해야 하는 경우입니다. 예를 들어 에너지를 탐사할 때 석유 굴착 장치와 풍력 발전 시설에서 비디오 콘텐츠를 생성하는 경우가 있습니다.

## <a name="benefits"></a>이점

Data Box Heavy는 네트워크에 거의 영향을 주지 않고 방대한 양의 데이터를 Azure로 이동할 수 있도록 설계되었습니다. 이 솔루션에는 다음과 같은 이점이 있습니다.

- **속도** - Data Box Heavy는 고성능 40Gbps 네트워크 인터페이스를 사용합니다.

- **보안** - Data Box Heavy에는 디바이스, 데이터 및 서비스에 대한 보안 보호 기능이 기본적으로 제공됩니다.
    - 나사 변조 방지 및 변조 방지 스티커로 보호되어 견고하게 포장되는 디바이스입니다.
    - 디바이스의 데이터는 항상 AES 256비트 암호화로 보호됩니다.
    - 디바이스는 Azure Portal에 제공된 암호로만 잠금 해제할 수 있습니다.
    - 서비스는 Azure 보안 기능으로 보호됩니다.
    - 데이터가 Azure에 업로드되면 디바이스의 디스크는 NIST(National Institute of Standards and Technology) 800-88r1 표준에 따라 완전히 초기화됩니다.


## <a name="features-and-specifications"></a>기능 및 사양

이 릴리스의 Data Box Heavy 디바이스에는 다음과 같은 기능이 있습니다.

| 사양                                          | 설명              |
|---------------------------------------------------------|--------------------------|
| 무게                                                  | 500lbs 이하                |
| 차원                                              | 너비: 26인치, 높이: 28인치, 길이: 48인치 |
| 랙 공간                                              | 랙에 탑재할 수 없음|
| 필요한 케이블                                         | 4개 접지형 120V/10A 전원 코드(NEMA 5-15) 포함, <br> 디바이스는 최대 240V 전원을 지원하고 C-13 전원 콘센트가 있음, <br> [Mellanox MCX314A-BCCT](https://store.mellanox.com/products/mellanox-mcx314a-bcct-connectx-3-pro-en-network-interface-card-40-56gbe-dual-port-qsfp-pcie3-0-x8-8gt-s-rohs-r6.html)와 호환되는 네트워크 케이블 사용  |
|거듭제곱                                                    | 두 디바이스 노드 모두에서 공유되는 4개 기본 PSU(전원 공급 장치)|
| Storage 용량                                        | 1PB 이하 원시, 각각 14TB의 70개 디스크 <br> 770TB의 사용 가능한 용량|
|노드 수                                          | 디바이스당 2개 독립 노드(각각 500TB) |
| 노드당 네트워크 인터페이스 수                             | 노드당 4개 네트워크 인터페이스 <br> MGMT, DATA3 <ul><li> 2개 1GbE 인터페이스 </li><li> MGMT - 관리용, 사용자 구성 가능하지 않음, 초기 설치에 사용됨 </li><li> DATA3 - 사용자가 구성할 수 있는 데이터 인터페이스, 기본적으로 DHCP(Dynamic Host Configuration Protocol) 구성</li><li>1GbE 네트워크 인터페이스도 10GbE 인터페이스로 구성할 수 있음</li></ul>DATA1, DATA2 데이터 인터페이스 <ul><li>2개 40GbE 인터페이스 </li><li> 기본적으로 DHCP 구성 또는 사용자가 구성할 수 있는 정적 데이터 인터페이스</li>|


## <a name="components"></a>구성 요소

Data Box Heavy는 다음과 같은 구성 요소를 포함합니다.

* **Data Box Heavy 디바이스** - 데이터를 안전하게 저장하는 외부가 견고한 물리적 디바이스입니다. 이 디바이스에는 770TB의 사용 가능한 스토리지 용량이 있습니다.
    
* **Data Box 서비스** – 다양한 지리적 위치에서 액세스할 수 있는 웹 인터페이스에서 Data Box Heavy 디바이스를 관리할 수 있는 Azure Portal의 확장입니다. Data Box 서비스를 사용하여 Data Box Heavy 디바이스를 관리합니다. 서비스 작업에는 주문 만들기 및 관리, 경고 보기 및 관리, 공유 관리 방법이 포함됩니다.  

* **로컬 웹 사용자 인터페이스** – 로컬 네트워크에 연결할 수 있도록 디바이스를 구성한 다음, Data Box 서비스에 디바이스를 등록하는 데 사용되는 웹 기반 UI입니다. 로컬 웹 UI를 사용하여 디바이스를 종료 및 다시 시작하고, 복사 로그를 보고, 서비스 요청을 위해 Microsoft 지원에 문의하세요.


## <a name="the-workflow"></a>워크플로

일반적인 흐름에 포함되는 단계는 다음과 같습니다.

1. **주문** - Azure Portal에서 주문을 만들고, 배송 정보를 제공하고, 데이터에 대한 대상 Azure 저장소 계정을 만듭니다. 디바이스가 사용 가능 상태가 되면 Azure에서 배송 추적 ID를 사용하여 디바이스를 준비 및 배송합니다.

2. **수신** - 디바이스가 배송되면 네트워크 및 전원 공급을 위해 디바이스를 지정된 케이블을 사용하여 연결합니다. 디바이스를 켜고 연결합니다. 디바이스 네트워크를 구성하고 데이터를 복사하려는 호스트 컴퓨터에서 공유를 탑재합니다.

3. **데이터 복사** - Data Box Heavy 공유에 데이터를 복사합니다.

4. **반환** - 디바이스가 준비되면 전원을 끈 상태로 Azure 데이터 센터로 다시 배송합니다.

5. **업로드** - 데이터가 자동으로 디바이스에서 Azure로 복사됩니다. 디바이스 디스크는 NIST(National Institute of Standard and Technology) 지침에 따라 안전하게 지워집니다.

이 프로세스 전체에서 모든 상태 변경에 대한 알림을 이메일을 통해 받습니다.

## <a name="region-availability"></a>지역 가용성

Data Box Heavy는 서비스가 배포되는 지역, 디바이스가 배송되는 국가/지역 및 데이터를 전송하는 대상 Azure 스토리지 계정에 따라 데이터를 전송할 수 있습니다.

- **서비스 가용성** - 이 릴리스의 경우 Data Box Heavy는 다음과 같은 지역에서 사용할 수 있습니다.
    - 미국 내 모든 공용 클라우드 지역 - 미국 중서부, 미국 서부 2, 미국 서부, 미국 중남부, 미국 중부, 미국 중북부, 미국 동부 및 미국 동부 2.
    - 유럽 연합 - 유럽 서부 및 유럽 북부.
    - 영국 - 영국 남부 및 영국 서부.
    - 프랑스 - 프랑스 중부 및 프랑스 남부.

- **대상 저장소 계정** - 데이터를 저장하는 저장소 계정은 서비스를 사용할 수 있는 모든 Azure 지역에서 사용할 수 있습니다.

Data Box Heavy의 지역 가용성에 대한 최신 정보를 보려면 [지역별 Azure 제품](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all)으로 이동하세요.

## <a name="sign-up"></a>등록

Data Box Heavy는 미리 보기 상태이며 등록이 필요합니다. Data Box Heavy에 가입하려면 다음 단계를 수행합니다.

1. https://aka.ms/azuredatabox에 있는 Azure Portal에 로그인합니다.
2. **+ 리소스 만들기**를 클릭하여 새 리소스를 추가합니다. **Azure Data Box**를 검색합니다. **Azure Data Box** 서비스를 선택합니다.

    <!--![The Data Box Heavy sign up 1]()-->

3. **만들기**를 클릭합니다.

    <!--![The Data Box Heavy sign up 2]()-->

4. Data Box Heavy 미리 보기에서 사용하려는 구독을 선택합니다. Data Box Heavy 리소스를 배포할 지역을 선택합니다. **Data Box Heavy** 옵션에서 **등록**을 클릭합니다.

   <!--![The Data Box Heavy sign up 3]()-->

5. 데이터 상주 국가/지역, 시간 프레임, 데이터 전송 대상 Azure 서비스, 네트워크 대역폭 및 데이터 전송 빈도에 관한 질문에 대답합니다. 개인정보 취급방침 및 약관을 검토하고 Microsoft가 이메일 주소를 사용하여 연락할 수 있다는 확인란을 선택합니다.

    <!--![The Data Box Heavy sign up 4]()-->

가입 후 미리 보기를 사용할 수 있게 되면 Data Box Heavy를 주문할 수 있습니다.

    