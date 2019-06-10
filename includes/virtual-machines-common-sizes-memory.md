---
title: 포함 파일
description: 포함 파일
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 05/16/2019
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 87fca23cab27ec27bfc9799066c126994167f46e
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66391249"
---
메모리 최적화 VM 크기는 관계형 데이터베이스 서버, 중대형 캐시 및 메모리 내 분석에 적합한 높은 메모리 대 CPU 비율을 제공합니다. 이 문서에서는 이 그룹화에서 각 크기에 대한 저장소 처리량 및 네트워크 대역폭뿐만 아니라 vCPU, 데이터 디스크 및 NIC의 수에 대한 정보를 제공합니다. 

* Mv2 시리즈는 가장 높은 vCPU 수 (최대 208 개의 Vcpu) 및 클라우드에서 모든 VM의 가장 큰 메모리 (최대 5.7 TiB)를 제공합니다. 높은 vCPU 개수 및 많은 양의 메모리를 활용하는 매우 큰 데이터베이스 또는 다른 애플리케이션에 이상적입니다.
 
* M 시리즈는 높은 vCPU 수 (최대 128 개의 Vcpu) 및 많은 양의 메모리 (최대 3.8tib))를 제공합니다. 도 매우 큰 데이터베이스 또는 높은 vCPU 개수 및 많은 양의 메모리를 활용 하는 다른 응용 프로그램에 이상적입니다.

* Dv2 시리즈, G 시리즈 및 DSv2/gs는 더 빠른 Vcpu와 더 나은 임시 저장소 성능을 요구 하거나 더 높은 메모리 요구량을 가진 응용 프로그램에 적합 합니다. 이들은 많은 엔터프라이즈급 애플리케이션을 위한 강력한 조합을 제공합니다.

* 원래 D 시리즈의 후속판인 Dv2 시리즈는 더 강력한 CPU가 특징입니다. Dv2 시리즈 CPU는 D 시리즈 CPU보다 약 35% 빠릅니다. 최신 세대 기반 2.4 GHz Intel Xeon® E5-2673 v3 2.4 GHz (Haswell) 또는 E5 2673 v4 (Broadwell) 2.3 GHz 프로세서, Intel Turbo Boost Technology 2.0을 사용 하 여 최대 3.1ghz까지 올라갈 수 있습니다. Dv2 시리즈는 D 시리즈와 메모리 및 디스크 구성이 같습니다.

* Ev3 시리즈는 하이퍼 스레드 구성에서 E5-2673 v4 2.3GHz(Broadwell) 프로세서를 사용하며, 대부분의 범용 워크로드에 대해 더 나은 가치를 제공하고 Ev3을 다른 대부분 클라우드의 범용 VM과 맞춥니다.  하이퍼 스레딩으로 이동하기 위해 디스크 및 네트워크 제한이 코어 단위로 조정되는 동안 메모리가 확장되었습니다(7GiB/vCPU에서 8GiB/vCPU로).  Ev3는 D/Dv2 제품군의 상위 메모리 VM 크기에 대한 후속 시리즈입니다.

* Azure Compute는 특정 하드웨어 유형에서 격리되고 단일 고객 전용인 가상 머신 크기를 제공합니다.  이러한 가상 머신 크기는 규정 준수 및 규정 요구 사항과 같은 요소를 포함하는 작업에 대해 다른 고객으로부터 높은 수준의 격리가 필요한 작업에 가장 적합합니다.  고객은 [중첩된 가상 머신에 대한 Azure 지원](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)을 사용하여 이러한 격리된 가상 머신의 리소스를 보다 세분화할 수도 있습니다.  격리된 VM 옵션은 아래 가상 머신 제품군의 표를 참조하세요.

## <a name="esv3-series"></a>Esv3 시리즈 

ACU: 160-190 <sup>1</sup>

Premium Storage:  지원됨

Premium Storage 캐싱:  지원됨

ESv3 시리즈 인스턴스는 2.3GHz Intel XEON® E5-2673 v4(Broadwell) 프로세서를 기반으로 하며, Intel Turbo Boost Technology 2.0을 통해 3.5GHz를 달성하고 Premium Storage를 사용할 수 있습니다. Ev3 시리즈 인스턴스는 메모리 집약적 엔터프라이즈 애플리케이션에 적합합니다.


| 크기             | vCPU | 메모리: GiB | 임시 저장소(SSD) GiB | 최대 데이터 디스크 수 | 최대 캐시 및 임시 스토리지 처리량: IOPS/MBps(GiB 단위의 캐시 크기) | 캐시되지 않은 최대 디스크 처리량: IOPS/MBps | 최대 NIC 수 / 예상 네트워크 대역폭(Mbps) |
|------------------|--------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------|
| Standard_E2s_v3 | 2      | 16          | 32             | 4              | 4000 / 32 (50)                                                       | 3200 / 48                                | 2 / 1000                                   |
| Standard_E4s_v3&nbsp;<sup>2</sup> | 4      | 32          | 64             | 8              | 8000 / 64 (100)                                                      | 6400 / 96                                | 2 / 2000                                   |
| Standard_E8s_v3&nbsp;<sup>2</sup> | 8      | 64          | 128            | 16             | 16000 / 128 (200)                                                    | 12800 / 192                              | 4 / 4000                                       |
| Standard_E16s_v3&nbsp;<sup>2</sup> | 16     | 128         | 256            | 32             | 32000 / 256 (400)                                                    | 25600 / 384                              | 8 / 8000                                       |
| Standard_E20s_v3                   | 20     | 160         | 320            | 32             | 40000 / 320 (400)                                                    | 32000 / 480                              | 8 / 10000                                       |
| Standard_E32s_v3&nbsp;<sup>2</sup> | 32     | 256         | 512            | 32             | 64000 / 512 (800)                                                    | 51200 / 768                              | 8 / 16000                             |
| Standard_E64s_v3&nbsp;<sup>2</sup> | 64     | 432         | 864            | 32             | 128000 / 1024 (1600)                                                   | 80000 / 1200                             | 8 / 30000                             |
| Standard_E64is_v3&nbsp;<sup>3</sup> | 64     | 432         | 864            | 32             | 128000 / 1024 (1600)                                                   | 80000 / 1200                             | 8 / 30000                             |


<sup>1</sup> Esv3 시리즈 VM은 Intel® 하이퍼 스레딩 기술 제공

<sup>2</sup> 사용 가능한 코어 크기 제한

<sup>3</sup> 인스턴스는 단일 고객 전용의 하드웨어에 격리되어 있습니다.


## <a name="ev3-series"></a>Ev3 시리즈 

ACU: 160 - 190 <sup>1</sup>

Premium Storage:  지원되지 않음

Premium Storage 캐싱:  지원되지 않음

Ev3 시리즈 인스턴스는 2.3GHz Intel XEON® E5-2673 v4(Broadwell) 프로세서를 기반으로 하며, Intel Turbo Boost Technology 2.0을 통해 3.5GHz를 달성할 수 있습니다. Ev3 시리즈 인스턴스는 메모리 집약적 엔터프라이즈 애플리케이션에 적합합니다.

데이터 디스크 저장소는 가상 머신과 별도로 비용이 청구됩니다. Premium Storage 디스크를 사용하려면 ESv3 크기를 사용합니다. ESv3 크기의 가격 및 요금 청구 기준은 Ev3 시리즈와 동일합니다. 


| 크기            | vCPU | 메모리: GiB | 임시 저장소(SSD) GiB | 최대 데이터 디스크 수 | 최대 임시 스토리지 처리량: IOPS/읽기 MBps/쓰기 MBps | 최대 NIC 수/네트워크 대역폭 |
|-----------------|-----------|-------------|----------------|----------------|----------------------------------------------------------|------------------------------|
| Standard_E2_v3  | 2         | 16          | 50             | 4              | 3000/46/23                                               | 2 / 1000                 |
| Standard_E4_v3  | 4         | 32          | 100            | 8              | 6000/93/46                                               | 2 / 2000                 |
| Standard_E8_v3  | 8         | 64          | 200            | 16             | 12000/187/93                                             | 4 / 4000                     |
| Standard_E16_v3 | 16        | 128         | 400            | 32             | 24000/375/187                                            | 8 / 8000                     |
| Standard_E20_v3 | 20        | 160         | 500            | 32             | 30000/469/234                                            | 8 / 10000                     |
| Standard_E32_v3 | 32        | 256         | 800            | 32             | 48000/750/375                                            | 8 / 16000                 |
| Standard_E64_v3 | 64        | 432         | 1600           | 32             | 96000/1000/500                                           | 8 / 30000           |
| Standard_E64i_v3&nbsp;<sup>2,&nbsp;3</sup> | 64        | 432         | 1600           | 32             | 96000/1000/500                                           | 8 / 30000           |

<sup>1</sup> Ev3 시리즈 VM은 Intel® 하이퍼 스레딩 기술 제공

<sup>2</sup> 사용 가능한 코어 크기 제한

<sup>3</sup> 인스턴스는 단일 고객 전용의 하드웨어에 격리되어 있습니다.


## <a name="mv2-series"></a>Mv2 시리즈

Premium Storage: 지원됨

Premium Storage 캐싱: 지원됨

Write Accelerator [지원됨](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator)

Mv2 시리즈 기능 높은 처리량, 짧은 대기 시간, 로컬 NVMe 저장소에서 실행 되는 하이퍼 스레드 Intel® Xeon® Platinum 8180 M 2.5ghz (Skylake) 프로세서 2.5ghz의 모든 핵심 기본 빈도와 3.8 GHz 최대 터보 빈도 직접 매핑됩니다. 모든 Mv2 시리즈 가상 머신 크기는 표준 및 프리미엄 영구적 디스크를 사용할 수 있습니다. Mv2 시리즈 인스턴스는 메모리 액세스에 최적화 된 대규모 메모리 내 데이터베이스 및 관계형 데이터베이스 서버, 캐시 및 메모리에 이상적인 메모리 대 CPU 비율이 높은 워크 로드를 지원 하기 위해 뛰어난된 계산 성능을 제공 하는 VM 크기 분석 합니다. 

|크기 | vCPU | 메모리: GiB | 임시 저장소(SSD) GiB | 최대 데이터 디스크 수 | 최대 캐시 및 임시 스토리지 처리량: IOPS/MBps(GiB 단위의 캐시 크기) | 캐시되지 않은 최대 디스크 처리량: IOPS/MBps | 최대 NIC 수 / 예상 네트워크 대역폭(Mbps) |
|-----------------|------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------|
| Standard_M208ms_v2<sup>1, 2</sup> | 208 | 5700 | 4096 | 64 | 80000 / 800 (7040) | 40000 / 1000 | 8 / 16000 |
| Standard_M208s_v2<sup>1, 2</sup> | 208 | 2850 | 4096 | 64 | 80000 / 800 (7040) | 40000 / 1000 | 8 / 16000 |

Mv2 시리즈 VM 기능 Intel® 하이퍼 스레딩 기술  

<sup>1</sup> 이러한 대형 Vm 지원 되는 게스트 Os 중 하나가 필요 합니다. Windows Server 2016, Windows Server 2019, SLES 12 SP4, SLES 15.

<sup>2</sup> Mv2 시리즈 Vm은 2 세대입니다. Linux를 사용 하는 SUSE Linux 이미지를 선택 하는 방법에 대 한 다음 섹션을 참조 합니다.

#### <a name="find-a-suse-image"></a>SUSE 이미지를 찾지

Azure portal에서 적절 한 SUSE Linux 이미지를 선택 합니다. 

1. Azure portal에서 선택 **리소스 만들기** 
1. "SUSE SAP" 검색 
1. SLES를 SAP 2 세대 이미지에는 하거나 종 량 제 하거나 사용자 고유의 구독 (BYOS) 상태로 전환 합니다. 검색 결과에서 원하는 이미지 범주를 확장 합니다.

    * SAP 용 SUSE Linux Enterprise Server (SLES)
    * (BYOS) SAP 용 SUSE Linux Enterprise Server (SLES)
    
1. SUSE 이미지는 Mv2 시리즈 호환 이름 접두사가 `GEN2:`합니다. 다음 SUSE 이미지는 Mv2 시리즈 Vm에 사용할 수 있습니다.

    * GEN2: SUSE Linux Enterprise Server (SLES) 12 for SAP Applications SP4
    * GEN2: SUSE Linux Enterprise Server for SAP Applications (SLES) 15
    * GEN2: SUSE Linux Enterprise Server (SLES) 12 for SAP Applications (BYOS) SP4
    * GEN2: SUSE Linux Enterprise Server (SLES) 15 for SAP Applications (BYOS)

#### <a name="select-a-suse-image-via-azure-cli"></a>Azure CLI를 통해 SUSE 이미지를 선택 합니다.

Mv2 시리즈 Vm, SAP 이미지에 대 한 현재 사용 가능한 SLES 목록을 보려면 다음을 사용 하 여 [ `az vm image list` ](https://docs.microsoft.com/cli/azure/vm/image?view=azure-cli-latest#az-vm-image-list) 명령:

```azurecli
az vm image list --output table --publisher SUSE --sku gen2 --all
```

명령은은 Mv2 시리즈 Vm에 대 한 SUSE에서 사용할 수 있는 현재 사용 가능한 세대 2 Vm을 출력합니다. 

예제 출력:

```
Offer          Publisher  Sku          Urn                                        Version
-------------  ---------  -----------  -----------------------------------------  ----------
SLES-SAP       SUSE       gen2-12-sp4  SUSE:SLES-SAP:gen2-12-sp4:2019.05.13       2019.05.13
SLES-SAP       SUSE       gen2-15      SUSE:SLES-SAP:gen2-15:2019.05.13           2019.05.13
SLES-SAP-BYOS  SUSE       gen2-12-sp4  SUSE:SLES-SAP-BYOS:gen2-12-sp4:2019.05.13  2019.05.13
SLES-SAP-BYOS  SUSE       gen2-15      SUSE:SLES-SAP-BYOS:gen2-15:2019.05.13      2019.05.13
```

## <a name="m-series"></a>M 시리즈 

ACU: 160-180 <sup>1</sup>

Premium Storage:  지원됨

Premium Storage 캐싱:  지원됨

Write Accelerator  [지원됨](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator)

| 크기            | vCPU | 메모리: GiB | 임시 저장소(SSD) GiB | 최대 데이터 디스크 수 | 최대 캐시 및 임시 스토리지 처리량: IOPS/MBps(GiB 단위의 캐시 크기) | 캐시되지 않은 최대 디스크 처리량: IOPS/MBps | 최대 NIC 수 / 예상 네트워크 대역폭(Mbps) |
|-----------------|------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------|
| Standard_M8ms&nbsp;<sup>3</sup>    | 8  | 218.75 | 256  | 8  | 10000 / 100 (793)  | 5000  / 125 | 4 / 2000 |
| Standard_M16ms&nbsp;<sup>3</sup>   | 16 | 437.5  | 512  | 16 | 20000 / 200 (1587) | 10000 / 250 | 8 / 4000 |
| Standard_M32ts | 32 | 192    | 1024 | 32 | 40000 / 400 (3174) | 20000 / 500 | 8 / 8000 |
| Standard_M32ls | 32 | 256    | 1024 | 32 | 40000 / 400 (3174) | 20000 / 500 | 8 / 8000 |
| Standard_M32ms&nbsp;<sup>3</sup>   | 32 | 875    | 1024 | 32 | 40000 / 400 (3174) | 20000 / 500 | 8 / 8000 |
| Standard_M64s  | 64 | 1024   | 2048 | 64 | 80000 / 800 (6348)| 40000 / 1000 | 8 / 16000          |
| Standard_M64ls  | 64 | 512    | 2048 | 64 | 80000 / 800 (6348) | 40000 / 1000 | 8 / 16000 |
| Standard_M64ms&nbsp;<sup>3</sup>  | 64   | 1792 | 2048 | 64 | 80000 / 800 (6348)| 40000 / 1000 | 8 / 16000          |
| Standard_M128s&nbsp;<sup>2</sup> | 128  | 2048        | 4096  | 64 | 160000 / 1600 (12696) | 80000 / 2000                            | 8 / 30000          |
| Standard_M128ms&nbsp;<sup>2,&nbsp;3,&nbsp;4</sup> | 128  | 3892  | 4096 | 64 | 160000 / 1600 (12696) | 80000 / 2000                            | 8 / 30000          |
| Standard_M64   | 64  | 1024 | 7168  | 64 | 80000  / 800  (1228) | 40000 / 1000 | 8 / 16000 |
| Standard_M64m  | 64  | 1792 | 7168  | 64 | 80000  / 800  (1228) | 40000 / 1000 | 8 / 16000 |
| Standard_M128&nbsp;<sup>2  | 128 | 2048 | 14336 | 64 | 250000 / 1600 (2456) | 80000 / 2000 | 8/32000 |
| Standard_M128m&nbsp;<sup>2 | 128 | 3892 | 14336 | 64 | 250000 / 1600 (2456) | 80000 / 2000 | 8/32000 |



<sup>1</sup> M 시리즈 VM은 Intel® 하이퍼 스레딩 기술 제공

<sup>2</sup> 64개를 초과하는 vCPU에는 지원되는 게스트 OS인 Windows Server 2016, Ubuntu 16.04 LTS, SLES 12 SP2 및 LIS 4.2.1을 사용하는 Red Hat Enterprise Linux, CentOS 7.3 또는 Oracle Linux 7.3 중 하나가 필요합니다.

<sup>3</sup> 사용 가능한 코어 크기 제한

<sup>4</sup> 인스턴스는 단일 고객 전용의 하드웨어에 격리되어 있습니다.
<br>

## <a name="gs-series"></a>GS 시리즈 

ACU: 180 - 240 <sup>1</sup>

Premium Storage:  지원됨

Premium Storage 캐싱:  지원됨

| 크기 | vCPU | 메모리: GiB | 임시 저장소(SSD) GiB | 최대 데이터 디스크 수 | 최대 캐시 및 임시 스토리지 처리량: IOPS/MBps(GiB 단위의 캐시 크기) | 캐시되지 않은 최대 디스크 처리량: IOPS/MBps | 최대 NIC 수 / 예상 네트워크 대역폭(Mbps) |
|---|---|---|---|---|---|---|---|
| Standard_GS1 |2 |28 |56 |8 |10000 / 100 (264) |5000 / 125 |2 / 2000 |
| Standard_GS2 |4 |56 |112 |16 |20000 / 200 (528) |10000 / 250 |2 / 4000 |
| Standard_GS3 |8 |112 |224 |32 |40000 / 400 (1056) |20000 / 500 |4 / 8000 |
| Standard_GS4&nbsp;<sup>3</sup> |16 |224 |448 |64 |80000 / 800 (2112) |40000 / 1000 |8 / 16000 |
| Standard_GS5&nbsp;<sup>2,&nbsp;3</sup> |32 |448 |896 |64 |160000 / 1600 (4224) |80000 / 2000 |8 / 20000 |

<sup>1</sup> GS 시리즈 VM에서 제공 가능한 최대 디스크 처리량(IOPS 또는 MBps)은 연결된 디스크의 수, 크기 및 스트라이핑에 따라 제한될 수 있습니다. 자세한 내용은 [고성능을 위한 디자인](../articles/virtual-machines/windows/premium-storage-performance.md)을 참조하세요.

<sup>2</sup> 인스턴스는 단일 고객 전용의 하드웨어에 격리되어 있습니다.

<sup>3</sup> 사용 가능한 코어 크기 제한

<br>

## <a name="g-series"></a>G 시리즈

ACU: 180 - 240

Premium Storage:  지원되지 않음

Premium Storage 캐싱:  지원되지 않음

| 크기         | vCPU | 메모리: GiB | 임시 저장소(SSD) GiB | 최대 임시 스토리지 처리량: IOPS/읽기 MBps/쓰기 MBps | 최대 데이터 디스크/처리량: IOPS | 최대 NIC 수 / 예상 네트워크 대역폭(Mbps) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_G1  | 2         | 28          | 384            | 6000 / 93 / 46                                           | 8 / 8 x 500                       | 2 / 2000                     |
| Standard_G2  | 4         | 56          | 768            | 12000 / 187 / 93                                         | 16 / 16 x 500                       | 2 / 4000                     |
| Standard_G3  | 8         | 112         | 1536          | 24000 / 375 / 187                                        | 32 / 32 x 500                     | 4 / 8000                |
| Standard_G4  | 16        | 224         | 3072          | 48000 / 750 / 375                                        | 64 / 64 x 500                     | 8 / 16000          |
| Standard_G5&nbsp;<sup>1</sup> | 32        | 448         | 6144          | 96000 / 1500 / 750                                       | 64 / 64 x 500                     | 8 / 20000           |

<sup>1</sup> 인스턴스는 단일 고객 전용의 하드웨어에 격리되어 있습니다.
<br>

## <a name="dsv2-series-11-15"></a>DSv2 시리즈 11-15

ACU: 210 - 250 <sup>1</sup>

Premium Storage:  지원됨

Premium Storage 캐싱:  지원됨

| 크기 | vCPU | 메모리: GiB | 임시 저장소(SSD) GiB | 최대 데이터 디스크 수 | 최대 캐시 및 임시 스토리지 처리량: IOPS/MBps(GiB 단위의 캐시 크기) | 캐시되지 않은 최대 디스크 처리량: IOPS/MBps | 최대 NIC 수 / 예상 네트워크 대역폭(Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11_v2&nbsp;<sup>3</sup> |2 |14 |28 |8 |8000 / 64 (72) |6400 / 96 |2 / 1500 |
| Standard_DS12_v2&nbsp;<sup>3</sup> |4 |28 |56 |16 |16000 / 128 (144) |12800 / 192 |4 / 3000 |
| Standard_DS13_v2&nbsp;<sup>3</sup> |8 |56 |112 |32 |32000 / 256 (288) |25600 / 384 |8 / 6000 |
| Standard_DS14_v2&nbsp;<sup>3</sup>|16 |112 |224 |64 |64000 / 512 (576) |51200 / 768 |8 / 12000 |
| Standard_DS15_v2&nbsp;<sup>2</sup> |20 |140 |280 |64 |80000 / 640 (720) |64000 / 960 |8 / 25000&nbsp;<sup>4</sup>

<sup>1</sup> DSv2 시리즈 VM에서 제공 가능한 최대 디스크 처리량(IOPS 또는 MBps)은 연결된 디스크의 수, 크기 및 스트라이핑에 따라 제한될 수 있습니다.  자세한 내용은 [고성능을 위한 디자인](../articles/virtual-machines/windows/premium-storage-performance.md)을 참조하세요.  
<sup>2</sup> 인스턴스는 단일 고객 전용의 하드웨어에 격리되어 있습니다.  
<sup>3</sup> 사용 가능한 코어 크기 제한  
<sup>4</sup> 가속 네트워킹 사용 시 25,000Mbps 

<br>

## <a name="dv2-series-11-15"></a>Dv2 시리즈 11-15

ACU: 210 - 250

Premium Storage:  지원되지 않음

Premium Storage 캐싱:  지원되지 않음

| 크기              | vCPU | 메모리: GiB | 임시 저장소(SSD) GiB | 최대 임시 스토리지 처리량: IOPS/읽기 MBps/쓰기 MBps | 최대 데이터 디스크/처리량: IOPS | 최대 NIC 수 / 예상 네트워크 대역폭(Mbps) |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11_v2   | 2         | 14          | 100            | 6000 / 93 / 46                                           | 8 / 8x500                         | 2 / 1500                     |
| Standard_D12_v2   | 4         | 28          | 200            | 12000 / 187 / 93                                         | 16 / 16x500                         | 4 / 3000                     |
| Standard_D13_v2   | 8         | 56          | 400            | 24000 / 375 / 187                                        | 32 / 32x500                       | 8 / 6000                     |
| Standard_D14_v2   | 16        | 112         | 800            | 48000 / 750 / 375                                        | 64 / 64x500                       | 8 / 12000          |
| Standard_D15_v2&nbsp;<sup>1</sup> | 20        | 140         | 1000          | 60000 / 937 / 468                                        | 64 / 64x500                       | 8 / 25000&nbsp;<sup>2</sup> |

<sup>1</sup> 인스턴스는 단일 고객 전용의 하드웨어에 격리되어 있습니다.  
<sup>2</sup> 가속 네트워킹 사용 시 25,000Mbps 
