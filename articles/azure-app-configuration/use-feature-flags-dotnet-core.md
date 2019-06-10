---
title: .NET Core 앱에서 기능 플래그 사용에 대한 자습서 | Microsoft Docs
description: 이 자습서에서는 .NET Core 앱에서 기능 플래그를 구현하는 방법을 알아봅니다.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 04/19/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: b0e48a0db63eded9e9c4921d33b03af39656ce0d
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66299272"
---
# <a name="tutorial-use-feature-flags-in-a-net-core-app"></a>자습서: .NET Core 앱에서 기능 플래그 사용

.NET Core 기능 관리 라이브러리는 .NET 또는 ASP.NET Core 애플리케이션에서 기능 플래그를 구현할 수 있도록 자연스러운 지원을 제공합니다. 기능 관리 라이브러리를 사용하면 기능 플래그를 코드에 보다 선언적으로 추가할 수 있으므로 `if` 문을 수동으로 작성할 필요가 없습니다. 기능 관리 라이브러리는 백그라운드에서 기능 플래그 수명 주기를 관리합니다(예: 플래그 상태 새로 고침 및 캐싱, 요청을 호출하는 동안 플래그 상태를 변경할 수 없도록 보장). 뿐만 아니라 ASP.NET Core 라이브러리는 MVC 컨트롤러 작업, 보기, 경로 및 미들웨어를 포함하여 기본 통합을 제공합니다.

[ASP.NET Core 앱에 기능 플래그 추가](./quickstart-feature-flag-aspnet-core.md) 빠른 시작에서는 ASP.NET Core 애플리케이션에서 기능 플래그를 추가하는 다양한 방법을 보여줍니다. 이 자습서에서는 이러한 방법을 자세히 설명합니다. 전체 내용은 [ASP.NET Core 기능 관리 설명서](https://go.microsoft.com/fwlink/?linkid=2091410)에서 확인할 수 있습니다.

이 자습서에서는 다음 방법을 알아봅니다.

> [!div class="checklist"]
> * 애플리케이션의 핵심 부분에 기능 가용성을 제어하는 기능 플래그를 추가합니다.
> * App Configuration과 통합하여 기능 플래그를 관리하는 데 사용합니다.

## <a name="setup"></a>설정

.NET Core 기능 관리자 `IFeatureManager`는 프레임워크의 기본 구성 시스템에서 기능 플래그를 가져옵니다. 따라서 로컬 *appsettings.json* 또는 환경 변수를 포함하여 .NET Core에서 지원하는 구성 소스를 사용하여 애플리케이션의 기능 플래그를 정의할 수 있습니다 기능 관리자는 .NET Core 종속성 주입을 사용합니다. 표준 규칙을 사용하여 기능 관리 서비스를 등록할 수 있습니다.

```csharp
using Microsoft.FeatureManagement;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement();
    }
}
```

기능 관리자는 기본적으로 .NET Core 구성 데이터의 "FeatureManagement" 섹션에서 기능 플래그를 검색합니다. 다음 예제는 "MyFeatureFlags"라는 다른 섹션에서 읽으라고 기능 관리자에게 지시합니다.

```csharp
using Microsoft.FeatureManagement;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement(options =>
        {
                options.UseConfiguration(Configuration.GetSection("MyFeatureFlags"));
        });
    }
}
```

기능 플래그에서 필터를 사용하는 경우 추가 라이브러리를 포함시키고 등록해야 합니다. 다음 예제에서는 **PercentageFilter"** 라는 기본 제공 기능 필터를 사용하는 방법을 보여줍니다.

```csharp
using Microsoft.FeatureManagement;
using Microsoft.FeatureManagement.FeatureFilters;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement()
                .AddFeatureFilter<PercentageFilter>();
    }
}
```

효과적으로 작동하려면 기능 플래그를 애플리케이션 외부에 유지하면서 별도로 관리해야 합니다. 이렇게 하면 언제든지 플래그 상태를 수정하고 해당 변경 내용을 애플리케이션에 즉시 적용할 수 있습니다. App Configuration은 전용 포털 UI를 통해 모든 기능 플래그를 구성 및 제어할 수 있는 중앙의 한 위치를 제공하며, .NET Core 클라이언트 라이브러리를 통해 애플리케이션에 직접 플래그를 제공합니다. ASP.NET Core 애플리케이션을 App Configuration에 연결하는 가장 쉬운 방법은 `Microsoft.Extensions.Configuration.AzureAppConfiguration` 구성 공급자입니다. 다음을 *Program.cs* 파일에 추가하여 이 NuGet 패키지를 코드에 사용할 수 있습니다.

```csharp
using Microsoft.Extensions.Configuration.AzureAppConfiguration;

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
           .ConfigureAppConfiguration((hostingContext, config) => {
               var settings = config.Build();
               config.AddAzureAppConfiguration(options => {
                   options.Connect(settings["ConnectionStrings:AppConfig"])
                          .UseFeatureFlags();
                });
           })
           .UseStartup<Startup>();
```

기능 플래그 값은 시간이 지나면 변화합니다. 기본적으로 기능 관리자가 30초마다 기능 플래그 값을 새로 고칩니다. 위의 `options.UseFeatureFlags()` 호출에 다른 폴링 간격을 사용할 수 있습니다.

```csharp
config.AddAzureAppConfiguration(options => {
    options.Connect(settings["ConnectionStrings:AppConfig"])
           .UseFeatureFlags(featureFlagOptions => {
                featureFlagOptions.PollInterval = TimeSpan.FromSeconds(5);
           });
});
```

## <a name="feature-flag-declaration"></a>기능 플래그 선언

각 기능 플래그는 기능의 상태가 *on*(즉, 값이 `True`)인지 평가하는 데 사용되는 하나 이상의 필터 이름과 목록으로 구성됩니다. 필터는 기능을 켜야 하는 사용 사례를 정의합니다. 기능 플래그에 여러 필터가 있는 경우 그 중 한 필터가 기능을 사용하도록 설정해야 한다고 확인할 때까지 필터 목록이 순서대로 트래버스됩니다. 현재는 기능 플래그가 *on*으로 간주되므로 나머지 필터 결과를 건너뜁니다. 기능을 사용하도록 설정해야 한다는 필터가 없으면 기능 플래그가 *off*됩니다.

기능 관리자는 기능 플래그의 구성 원본으로 *appsettings.json*을 지원합니다. 다음 예제에서는 json 파일에서 기능 플래그를 설정하는 방법을 보여줍니다.

```JSON
"FeatureManagement": {
    "FeatureX": true, // Feature flag set to on
    "FeatureY": false, // Feature flag set to off
    "FeatureC": {
        "EnabledFor": [
            {
                "Name": "Percentage",
                "Parameters": {
                    "Value": 50
                }
            }
        ]
    }
}
```

규칙에 따라 이 json 문서의 `FeatureManagement` 섹션은 기능 플래그 설정에 사용됩니다. 위의 예제에서는 *EnabledFor* 속성에 필터가 정의된 다음 세 가지 기능 플래그를 보여줍니다.

* **FeatureA**는 *on*입니다.
* **FeatureB**는 *off*입니다.
* **FeatureC**는 *매개 변수* 속성을 사용하여 *백분율*이라는 필터를 지정합니다. *백분율*은 구성 가능한 필터의 예이며 **FeatureC** 플래그가 *on*될 확률을 50%로 지정합니다.

## <a name="referencing"></a>참조

필수 사항은 아니지만, 기능 플래그를 코드에서 쉽게 참조할 수 있도록 `enum` 변수로 정의하는 것이 좋습니다.

```csharp
public enum MyFeatureFlags
{
    FeatureA,
    FeatureB,
    FeatureC
}
```

## <a name="feature-flag-check"></a>기능 플래그 확인

먼저 기능 플래그가 *on*으로 설정되었는지 확인하고, on으로 설정되어 있으면 포함된 작업을 수행하는 것이 기능 관리의 기본 패턴입니다.

```csharp
IFeatureManager featureManager;
...
if (featureManager.IsEnabled(nameof(MyFeatureFlags.FeatureA)))
{
    // Run the following code
}
```

## <a name="dependency-injection"></a>종속성 주입

ASP.NET Core MVC에서 종속성 주입을 통해 `IFeatureManager` 기능 관리자에 액세스할 수 있습니다.

```csharp
public class HomeController : Controller
{
    private readonly IFeatureManager _featureManager;

    public HomeController(IFeatureManager featureManager)
    {
        _featureManager = featureManager;
    }
}
```

## <a name="controller-action"></a>컨트롤러 작업

MVC 컨트롤러에서 `Feature` 특성을 사용하여 전체 컨트롤러 클래스를 사용할 것인지 아니면 특정 작업을 사용할 것인지 제어할 수 있습니다. 다음 `HomeController` 컨트롤러는 *FeatureA*가 *on*으로 설정되어야만 포함된 작업을 실행할 수 있습니다.

```csharp
[Feature(MyFeatureFlags.FeatureA)]
public class HomeController : Controller
{
    ...
}
```

위의 `Index` 작업은 *FeatureA*가 *on*으로 설정되어야만 실행할 수 있습니다.

```csharp
[Feature(MyFeatureFlags.FeatureA)]
public IActionResult Index()
{
    return View();
}
```

제어하는 기능 플래그가 *off*로 설정되어 MVC 컨트롤러 또는 작업이 차단되면 등록된 `IDisabledFeatureHandler`가 호출됩니다. 기본 `IDisabledFeatureHandler`는 응답 본문 없이 404 상태 코드를 클라이언트에 반환합니다.

## <a name="view"></a>보기

MVC 보기에서 `<feature>` 태그를 사용하여 기능 플래그의 사용 여부에 따라 콘텐츠를 렌더링할 수 있습니다.

```html
<feature name="FeatureA">
    <p>This can only be seen if 'FeatureA' is enabled.</p>
</feature>
```

## <a name="mvc-filter"></a>MVC 필터

기능 플래그의 상태에 따라 활성화되도록 MVC 필터를 설정할 수 있습니다. 다음은 `SomeMvcFilter`라는 MVC 필터를 추가합니다. 이 필터는 *FeatureA*가 사용되는 경우에만 MVC 파이프라인 내에서 트리거됩니다.

```csharp
using Microsoft.FeatureManagement.FeatureFilters;

IConfiguration Configuration { get; set;}

public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc(options => {
        options.Filters.AddForFeature<SomeMvcFilter>(nameof(MyFeatureFlags.FeatureA));
    });
}
```

## <a name="route"></a>라우팅

기능 플래그에 따라 경로를 동적으로 노출할 수 있습니다. 다음은 *FeatureA*가 사용되는 경우에만 `Beta`를 기본 컨트롤러로 설정하는 경로를 추가합니다.

```csharp
app.UseMvc(routes => {
    routes.MapRouteForFeature(nameof(MyFeatureFlags.FeatureA), "betaDefault", "{controller=Beta}/{action=Index}/{id?}");
});
```

## <a name="middleware"></a>미들웨어

기능 플래그를 사용하여 조건에 따라 애플리케이션 분기 및 미들웨어를 추가할 수 있습니다. 다음은 *FeatureA*가 사용되는 경우에만 요청 파이프라인에 미들웨어 구성 요소를 삽입합니다.

```csharp
app.UseMiddlewareForFeature<ThirdPartyMiddleware>(nameof(MyFeatureFlags.FeatureA));
```

이렇게 하면 기능 플래그에 따라 전체 애플리케이션을 분기하는 보다 일반적인 기능이 빌드됩니다.

```csharp
app.UseForFeature(featureName, appBuilder => {
    appBuilder.UseMiddleware<T>();
});
```

## <a name="next-steps"></a>다음 단계

이 자습서에서는 ASP.NET Core 애플리케이션에서 `Microsoft.FeatureManagement` 라이브러리를 활용하여 기능 플래그를 구현하는 방법을 배웠습니다. ASP.NET Core 및 App Configuration의 기능 관리 지원에 대한 자세한 내용은 다음 리소스를 참조하세요.

* [ASP.NET Core 기능 플래그 샘플 코드](/azure/azure-app-configuration/quickstart-feature-flag-aspnet-core)
* [Microsoft.FeatureManagement 설명서](https://docs.microsoft.com/dotnet/api/microsoft.featuremanagement)
* [기능 플래그 관리](./manage-feature-flags.md)
