---
title: Azure 구독 취소 | Microsoft Docs
description: 무료 평가판 구독과 같은 Azure 구독을 취소하는 방법을 설명합니다.
author: bandersmsft
manager: amberb
tags: billing
ms.assetid: 3051d6b0-179f-4e3a-bda4-3fee7135eac5
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: banders
ms.openlocfilehash: 8f4279d9ac085cdd1ded0dfdda4fad9d3fe12fb8
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66480227"
---
# <a name="cancel-your-subscription-for-azure"></a>Azure 구독 취소

Azure 구독에만 [계정 관리자](billing-subscription-transfer.md#whoisaa) Azure 구독을 취소할 수 있습니다. Azure 구독 관리자 수도 [다른 사용자는 구독 관리자로 할당](billing-add-change-azure-subscription-administrator.md#assign-a-user-as-an-administrator-of-a-subscription) 는 구독을 취소할 수 있습니다. 구독을 취소하면 Azure 서비스 및 리소스에 대한 액세스 권한이 종료됩니다.

구독을 취소하기 전에 다음을 수행합니다.

* 데이터를 백업합니다. 예를 들어 Azure Storage 또는 SQL에 데이터를 저장하는 경우 복사본을 다운로드합니다. 가상 머신을 사용하는 경우 해당 이미지를 로컬로 저장합니다.
* 서비스를 종료합니다. [관리 포털의 페이지 리소스](https://ms.portal.azure.com/?flight=1#blade/HubsExtension/Resources/resourceType/Microsoft.Resources%2Fresources)로 이동한 후 실행 중인 가상 머신, 애플리케이션 또는 기타 서비스를 모두 **중지**합니다.
* 데이터를 마이그레이션하는 것이 좋습니다. [새 리소스 그룹 또는 구독으로 리소스 이동](../azure-resource-manager/resource-group-move-resources.md)을 참조하세요.
* 모든 리소스 및 모든 리소스 그룹을 삭제 해야 합니다. 삭제할 구독을 취소 하기 전에 필요 합니다. 각 리소스 그룹을 개별적으로 삭제 되어야 합니다. 리소스 그룹 삭제 하는 동안 리소스 그룹 이름을 입력 하 여 삭제를 확인 해야 합니다.
* 이 구독에서 참조 하는 모든 사용자 지정 역할이 있는 경우 `AssignableScopes`, 구독을 제거 하려면 해당 사용자 지정 역할을 업데이트 해야 합니다. 구독을 취소 한 후 사용자 지정 역할을 업데이트 하려고 하면 오류가 표시 될 수 있습니다. 자세한 내용은 [사용자 지정 역할을 사용 하 여 문제 해결](../role-based-access-control/troubleshooting.md#problems-with-custom-roles) 하 고 [Azure 리소스에 대 한 사용자 지정 역할](../role-based-access-control/custom-roles.md)입니다.

유료 Azure 지원 계획을 취소해도 남은 구독 기간에 대해 요금이 계속 청구됩니다. 자세한 내용은 [Azure 지원 계획](https://azure.microsoft.com/support/plans/)을 참조하세요.

## <a name="cancel-subscription-using-the-azure-portal"></a>Azure Portal을 사용하여 구독 취소

1. [Azure Portal의 구독 페이지](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)에서 구독을 선택합니다.
2. 취소할 구독을 선택합니다.
3. **개요**를 선택한 다음, **구독 취소**를 선택합니다.

    ![취소 단추를 보여 주는 스크린샷](./media/billing-how-to-cancel-azure-subscription/cancel_ibiza.png)
3. 메시지에 따라 취소를 완료합니다.

## <a name="what-happens-after-i-cancel-my-subscription"></a>구독을 취소한 후에는 어떻게 되나요?

을 취소 하면 청구가 즉시 중지 됩니다. 그러나 취소가 포털에 표시되기까지 최대 10분이 걸릴 수 있습니다. 청구 기간 중에 취소하는 경우 기간이 만료된 후 일반적인 청구서 날짜에 최종 청구서를 보냅니다.

을 취소 하면 귀하의 서비스가 비활성화 됩니다. 즉, 가상 머신의 할당이 취소되고 임시 IP 주소가 해제되며 저장소는 읽기 전용이 됩니다.

다시 액세스해야 하거나 마음이 바뀐 경우를 대비해서 90일 동안 기다렸다가 데이터를 영구 삭제합니다. 데이터 보존 요금은 부과되지 않습니다. 자세한 내용은 [Microsoft 보안 센터 - 데이터 관리 방법](https://go.microsoft.com/fwLink/p/?LinkID=822930&clcid=0x409)을 참조하세요.

## <a name="reactivate-subscription"></a>구독 다시 활성화

종량제 구독을 실수로 취소한 경우 [계정 센터에서 다시 활성화](billing-subscription-become-disable.md)할 수 있습니다.

종량제 구독이 아닌 경우 구독을 다시 활성화하려면 취소 후 90일 이내에 지원 센터로 문의하세요.

## <a name="need-help-contact-us"></a>도움 필요 시 문의하세요.

문의 사항이 있거나 도움이 필요한 경우 [지원 요청을 만드는](https://go.microsoft.com/fwlink/?linkid=2083458)합니다.
