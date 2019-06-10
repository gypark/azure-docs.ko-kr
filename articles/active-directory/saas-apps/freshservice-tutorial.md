---
title: '자습서: Freshservice와 Azure Active Directory 통합 | Microsoft Docs'
description: Azure Active Directory 및 Freshservice 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 3dd22b1f-445d-45c6-8eda-30207eb9a1a8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c87b23c338788804be22639f73acfb61ce8d6973
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65989405"
---
# <a name="tutorial-integrate-freshservice-with-azure-active-directory"></a>자습서: Azure Active Directory와 Freshservice 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 Freshservice를 통합하는 방법에 대해 알아봅니다. Azure AD와 Freshservice를 통합하면 다음 작업을 할 수 있습니다.

* Freshservice에 대한 액세스 권한이 있는 사용자를 Azure AD에서 제어합니다.
* 사용자가 해당 Azure AD 계정으로 Freshservice에 자동으로 로그인되도록 설정할 수 있습니다.
* 단일 중앙 위치인 Azure Portal에서 계정을 관리합니다.

Azure AD와 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory를 사용한 애플리케이션 액세스 및 Single Sign-On이란 무엇인가요?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

시작하려면 다음 항목이 필요합니다.

* Azure AD 구독 구독이 없는 경우 [여기](https://azure.microsoft.com/pricing/free-trial/)에서 1개월 평가판을 다운로드할 수 있습니다.
* Freshservice SSO(Single Sign-On)가 가능하도록 설정된 구독

## <a name="scenario-description"></a>시나리오 설명

이 자습서에서는 테스트 환경에서 Azure AD SSO를 구성하고 테스트합니다. Freshservice에서 **SP** 시작 SSO를 지원합니다.

## <a name="adding-freshservice-from-the-gallery"></a>갤러리에서 Freshservice 추가

Freshservice의 Azure AD 통합을 구성하려면 갤러리의 Freshservice를 관리되는 SaaS 앱 목록에 추가해야 합니다.

1. [Azure Portal](https://portal.azure.com)에 회사 또는 학교 계정, 개인 Microsoft 계정으로 로그인합니다.
1. 왼쪽 탐색 창에서 **Azure Active Directory** 서비스를 선택합니다.
1. **엔터프라이즈 애플리케이션**으로 이동한 다음, **모든 애플리케이션**을 선택합니다.
1. 새 애플리케이션을 추가하려면 **새 애플리케이션**을 선택합니다.
1. **갤러리에서 추가** 섹션의 검색 상자에 **Freshservice**를 입력합니다.
1. 결과 패널에서 **Freshservice**를 선택한 후 앱을 추가합니다. 앱이 테넌트에 추가될 때까지 잠시 동안 기다려 주세요.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성 및 테스트

테스트 사용자 **Britta Simon**을 사용하여 Freshservice로 Azure AD SSO를 구성 및 테스트합니다. SSO가 작동하려면 Azure AD 사용자와 Freshservice의 관련 사용자 간에 연결 관계가 설정되어야 합니다.

Freshservice에서 Azure AD SSO를 구성하고 테스트하려면 다음 구성 요소를 완료합니다.

1. **[Azure AD SSO 구성](#configure-azure-ad-sso)** - 사용자가 이 기능을 사용할 수 있도록 설정합니다.
2. **[Freshservice SSO 구성](#configure-freshservice-sso)** - 애플리케이션 쪽에서 Single Sign-On 설정을 구성합니다.
3. **[Azure AD 테스트 사용자 만들기](#create-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On을 테스트하는 데 사용합니다.
4. **[Azure AD 테스트 사용자 할당](#assign-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
5. **[Freshservice 테스트 사용자 만들기](#create-freshservice-test-user)** - Azure AD 표현과 연결된 Britta Simon에 해당하는 사용자를 Freshservice에 만듭니다.
6. **[SSO 테스트](#test-sso)** - 구성이 작동하는지 여부를 확인합니다.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO 구성

Azure Portal에서 Azure AD SSO를 사용하도록 설정하려면 다음 단계를 수행합니다.

1. [Azure Portal](https://portal.azure.com/)의 **Freshservice** 애플리케이션 통합 페이지에서 **관리** 섹션을 찾은 후 **Single Sign-On**을 선택합니다.
1. **Single Sign-On 방법 선택** 페이지에서 **SAML**을 선택합니다.
1. **SAML로 Single Sign-On 설정** 페이지에서 **기본 SAML 구성**에 대한 편집(연필 모양) 아이콘을 클릭하여 설정을 편집합니다.

   ![기본 SAML 구성 편집](common/edit-urls.png)

1. **기본 SAML 구성** 페이지에서 다음 필드에 대한 값을 입력합니다.

    a. **로그온 URL** 텍스트 상자에서 `https://<democompany>.freshservice.com` 패턴을 사용하는 URL을 입력합니다.

    b. **식별자(엔터티 ID)** 텍스트 상자에서 `https://<democompany>.freshservice.com` 패턴을 사용하는 URL을 입력합니다.

    > [!NOTE]
    > 이러한 값은 실제 값이 아닙니다. 실제 로그온 URL 및 식별자로 이러한 값을 업데이트합니다. 이러한 값을 얻으려면 [Freshservice 클라이언트 지원 팀](https://support.freshservice.com/)에 문의하세요. Azure Portal의 **기본 SAML 구성** 섹션에 표시된 패턴을 참조할 수도 있습니다.

1. **SAML 서명 인증서** 섹션에서 **편집** 단추를 클릭하여 **SAML 서명 인증서** 대화 상자를 엽니다.

    ![SAML 서명 인증서 편집](common/edit-certificate.png)

1. **SAML 서명 인증서** 섹션에서 **지문**을 복사하여 컴퓨터에 저장합니다.

    ![지문 값 복사](common/copy-thumbprint.png)

1. **Salesforce 설정** 섹션에서 요구 사항에 따라 적절한 URL을 복사합니다.

    ![구성 URL 복사](common/copy-configuration-urls.png)

    a. 로그인 URL

    b. Azure AD 식별자

    다. 로그아웃 URL

### <a name="configure-freshservice-sso"></a>Freshservice SSO 구성

1. Freshservice 내에서 구성을 자동화하려면 **확장 설치**를 클릭하여 **내 앱 보안 로그인 브라우저 확장**을 설치해야 합니다.

    ![내 앱 확장](common/install-myappssecure-extension.png)

2. 브라우저에 확장을 추가한 후 **Freshservice 설정**을 클릭하면 Freshservice 애플리케이션으로 이동됩니다. 애플리케이션에서 관리자 자격 증명을 입력하여 Freshservice에 로그인합니다. 브라우저 확장이 애플리케이션을 자동으로 구성하고 3-6단계를 자동으로 수행합니다.

    ![구성 설정](common/setup-sso.png)

3. Freshservice를 수동으로 설정하려면 새 웹 브라우저 창을 열고 Freshservice 회사 사이트에 관리자로 로그인한 후에 다음 단계를 수행합니다.

4. 위쪽의 메뉴에서 **관리자**를 클릭합니다.

    ![관리자](./media/freshservice-tutorial/ic790814.png "관리자")

5. **고객 포털**에서 **보안**을 클릭합니다.

    ![보안](./media/freshservice-tutorial/ic790815.png "보안")

6. **보안** 섹션에서 다음 단계를 수행합니다.

    ![Single Sign On](./media/freshservice-tutorial/ic790816.png "Single Sign On")

    a. **Single Sign On**을 전환합니다.

    b. **SAML SSO**를 선택합니다.

    다. Azure Portal에서 복사한 **로그인 URL** 값을 **SAML 로그인 URL** 텍스트 상자에 붙여넣습니다.

    d. Azure Portal에서 복사한 **로그아웃 URL** 값을 **로그아웃 URL** 텍스트 상자에 붙여넣습니다.

    e. Azure Portal에서 복사한 인증서의 **THUMBPRINT** 값을 **보안 인증서 지문** 텍스트 상자에 붙여넣습니다.

    f. 페이지 맨 아래에 있는 **저장**

### <a name="create-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기

이 섹션에서는 Azure Portal에서 Britta Simon이라는 테스트 사용자를 만듭니다.

1. Azure Portal의 왼쪽 창에서 **Azure Active Directory**, **사용자**, **모든 사용자**를 차례로 선택합니다.
1. 화면 위쪽에서 **새 사용자**를 선택합니다.
1. **사용자** 속성에서 다음 단계를 수행합니다.
   1. **이름** 필드에 `Britta Simon`을 입력합니다.  
   1. **사용자 이름** 필드에 username@companydomain.extension을 입력합니다. 예: `BrittaSimon@contoso.com`
   1. **암호 표시** 확인란을 선택한 다음, **암호** 상자에 표시된 값을 적어둡니다.
   1. **만들기**를 클릭합니다.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 Britta Simon에게 Freshservice에 대한 액세스 권한을 부여합니다.

1. Azure Portal에서 **엔터프라이즈 애플리케이션**을 선택한 다음, **모든 애플리케이션**을 선택합니다.
1. 애플리케이션 목록에서 **Freshservice**를 선택합니다.
1. 앱의 개요 페이지에서 **관리** 섹션을 찾고 **사용자 및 그룹**을 선택합니다.

   !["사용자 및 그룹" 링크](common/users-groups-blade.png)

1. **사용자 추가**를 선택한 다음, **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![사용자 추가 링크](common/add-assign-user.png)

1. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택한 다음, 화면의 아래쪽에 있는 **선택** 단추를 클릭합니다.
1. SAML 어설션에서 역할 값이 필요한 경우 **역할 선택** 대화 상자의 목록에서 사용자에게 적절한 역할을 선택한 다음, 화면의 아래쪽에 있는 **선택** 단추를 클릭합니다.
1. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.

### <a name="create-freshservice-test-user"></a>Freshservice 테스트 사용자 만들기

Azure AD 사용자가 FreshService에 로그인할 수 있도록 하려면 FreshService로 프로비저닝되어야 합니다. FreshService의 경우 프로비전은 수동 작업입니다.

**사용자 계정을 프로비전하려면 다음 단계를 수행합니다.**

1. **FreshService** 회사 사이트에 관리자 권한으로 로그인합니다.

2. 위쪽의 메뉴에서 **관리자**를 클릭합니다.

    ![관리자](./media/freshservice-tutorial/ic790814.png "관리자")

3. **사용자 관리**섹션에서 **요청자**를 클릭합니다.

    ![요청자](./media/freshservice-tutorial/ic790818.png "요청자")

4. **새 요청자**를 클릭합니다.

    ![새 요청자](./media/freshservice-tutorial/ic790819.png "새 요청자")

5. **새 요청자** 섹션에서 다음 단계를 수행합니다.

    ![새 요청자](./media/freshservice-tutorial/ic790820.png "새 요청자")  

    a. 관련된 텍스트 상자에 프로비전할 유효한 Azure Active Directory 계정의 **이름** 및 **전자 메일** 특성을 입력합니다.

    b. **저장**을 클릭합니다.

    > [!NOTE]
    > Azure Active Directory 계정 보유자는 활성화되기 전에 계정을 확인하기 위한 링크가 포함된 전자 메일을 받습니다.
    >  

> [!NOTE]
> 다른 FreshService 사용자 계정 생성 도구 또는 FreshService가 제공한 API를 사용하여 AAD 사용자 계정을 프로비전할 수 있습니다.

### <a name="test-sso"></a>SSO 테스트

액세스 패널에서 Freshservice 타일을 클릭하면 SSO를 설정한 Freshservice에 자동으로 로그인됩니다. 액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)를 참조하세요.

## <a name="additional-resources"></a>추가 리소스

- [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory로 애플리케이션 액세스 및 Single Sign-On을 구현하는 방법](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory의 조건부 액세스란?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
