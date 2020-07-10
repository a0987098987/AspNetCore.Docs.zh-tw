---
title: 當地語系化擴充性
author: hishamco
description: 了解如何在 ASP.NET Core 應用程式中擴充當地語系化 API。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/03/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/localization-extensibility
ms.openlocfilehash: b743aee87af96ebf064689e94d85ee794de520e7
ms.sourcegitcommit: 14c3d111f9d656c86af36ecb786037bf214f435c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/09/2020
ms.locfileid: "86176195"
---
# <a name="localization-extensibility"></a><span data-ttu-id="2266e-103">當地語系化擴充性</span><span class="sxs-lookup"><span data-stu-id="2266e-103">Localization Extensibility</span></span>

<span data-ttu-id="2266e-104">作者為 [Hisham Bin Ateya](https://github.com/hishamco)</span><span class="sxs-lookup"><span data-stu-id="2266e-104">By [Hisham Bin Ateya](https://github.com/hishamco)</span></span>

<span data-ttu-id="2266e-105">這篇文章：</span><span class="sxs-lookup"><span data-stu-id="2266e-105">This article:</span></span>

* <span data-ttu-id="2266e-106">列出當地語系化 API 的擴充點。</span><span class="sxs-lookup"><span data-stu-id="2266e-106">Lists the extensibility points on the localization APIs.</span></span>
* <span data-ttu-id="2266e-107">提供如何擴充 ASP.NET Core 應用程式當地語系化的指示。</span><span class="sxs-lookup"><span data-stu-id="2266e-107">Provides instructions on how to extend ASP.NET Core app localization.</span></span>

## <a name="extensible-points-in-localization-apis"></a><span data-ttu-id="2266e-108">當地語系化 API 中的擴充點</span><span class="sxs-lookup"><span data-stu-id="2266e-108">Extensible Points in Localization APIs</span></span>

<span data-ttu-id="2266e-109">ASP.NET Core 當地語系化 API 已建置為可擴充的。</span><span class="sxs-lookup"><span data-stu-id="2266e-109">ASP.NET Core localization APIs are built to be extensible.</span></span> <span data-ttu-id="2266e-110">擴充性讓開發人員能夠根據自己的需求自訂當地語系化。</span><span class="sxs-lookup"><span data-stu-id="2266e-110">Extensibility allows developers to customize the localization according to their needs.</span></span> <span data-ttu-id="2266e-111">例如，[OrchardCore](https://github.com/orchardCMS/OrchardCore/) 有一個 `POStringLocalizer`。</span><span class="sxs-lookup"><span data-stu-id="2266e-111">For instance, [OrchardCore](https://github.com/orchardCMS/OrchardCore/) has a `POStringLocalizer`.</span></span> <span data-ttu-id="2266e-112">`POStringLocalizer` 會詳細說明如何使用[可攜式物件當地語系化](xref:fundamentals/portable-object-localization)，使用 `PO` 檔案來儲存當地語系化資源。</span><span class="sxs-lookup"><span data-stu-id="2266e-112">`POStringLocalizer` describes in detail using [Portable Object localization](xref:fundamentals/portable-object-localization) to use `PO` files to store localization resources.</span></span>

<span data-ttu-id="2266e-113">本文列出當地語系化 API 所提供的兩個主要擴充點：</span><span class="sxs-lookup"><span data-stu-id="2266e-113">This article lists the two main extensibility points that localization APIs provide:</span></span> 

* <xref:Microsoft.AspNetCore.Localization.RequestCultureProvider>
* <xref:Microsoft.Extensions.Localization.IStringLocalizer>

## <a name="localization-culture-providers"></a><span data-ttu-id="2266e-114">當地語系化文化特性提供者</span><span class="sxs-lookup"><span data-stu-id="2266e-114">Localization Culture Providers</span></span>

<span data-ttu-id="2266e-115">ASP.NET Core 當地語系化 API 具有四個預設提供者，可判斷執行中要求目前的文化特性：</span><span class="sxs-lookup"><span data-stu-id="2266e-115">ASP.NET Core localization APIs have four default providers that can determine the current culture of an executing request:</span></span>

* <xref:Microsoft.AspNetCore.Localization.QueryStringRequestCultureProvider>
* <xref:Microsoft.AspNetCore.Localization.CookieRequestCultureProvider>
* <xref:Microsoft.AspNetCore.Localization.AcceptLanguageHeaderRequestCultureProvider>
* <xref:Microsoft.AspNetCore.Localization.CustomRequestCultureProvider>

<span data-ttu-id="2266e-116">[當地語系化中介軟體](xref:fundamentals/localization)文件中會詳細說明上述提供者。</span><span class="sxs-lookup"><span data-stu-id="2266e-116">The preceding providers are described in detail in the [Localization Middleware](xref:fundamentals/localization) documentation.</span></span> <span data-ttu-id="2266e-117">如果預設提供者不符合您的需求，請使用下列其中一種方法來建立自訂提供者：</span><span class="sxs-lookup"><span data-stu-id="2266e-117">If the default providers don't meet your needs, build a custom provider using one of the following approaches:</span></span>

### <a name="use-customrequestcultureprovider"></a><span data-ttu-id="2266e-118">使用 CustomRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="2266e-118">Use CustomRequestCultureProvider</span></span>

<span data-ttu-id="2266e-119"><xref:Microsoft.AspNetCore.Localization.CustomRequestCultureProvider> 提供自訂的 <xref:Microsoft.AspNetCore.Localization.RequestCultureProvider>，使用簡單的委派來判斷目前的當地語系化文化特性：</span><span class="sxs-lookup"><span data-stu-id="2266e-119"><xref:Microsoft.AspNetCore.Localization.CustomRequestCultureProvider> provides a custom <xref:Microsoft.AspNetCore.Localization.RequestCultureProvider> that uses a simple delegate to determine the current localization culture:</span></span>

::: moniker range="< aspnetcore-3.0"
```csharp
options.RequestCultureProviders.Insert(0, new CustomRequestCultureProvider(async context =>
{
    var currentCulture = "en";
    var segments = context.Request.Path.Value.Split(new char[] { '/' }, 
        StringSplitOptions.RemoveEmptyEntries);

    if (segments.Length > 1 && segments[0].Length == 2)
    {
        currentCulture = segments[0];
    }

    var requestCulture = new ProviderCultureResult(currentCulture);
    
    return Task.FromResult(requestCulture);
}));
```

::: moniker-end

::: moniker range=">= aspnetcore-3.0"
```csharp
options.AddInitialRequestCultureProvider(new CustomRequestCultureProvider(async context =>
{
    var currentCulture = "en";
    var segments = context.Request.Path.Value.Split(new char[] { '/' }, 
        StringSplitOptions.RemoveEmptyEntries);

    if (segments.Length > 1 && segments[0].Length == 2)
    {
        currentCulture = segments[0];
    }

    var requestCulture = new ProviderCultureResult(currentCulture);
    
    return Task.FromResult(requestCulture);
}));
```

::: moniker-end

### <a name="use-a-new-implemetation-of-requestcultureprovider"></a><span data-ttu-id="2266e-120">使用 RequestCultureProvider 的新實作</span><span class="sxs-lookup"><span data-stu-id="2266e-120">Use a new implemetation of RequestCultureProvider</span></span>

<span data-ttu-id="2266e-121"><xref:Microsoft.AspNetCore.Localization.RequestCultureProvider> 的新實作可建立來從自訂來源判斷要求文化特性資訊。</span><span class="sxs-lookup"><span data-stu-id="2266e-121">A new implementation of <xref:Microsoft.AspNetCore.Localization.RequestCultureProvider> can be created that determines the request culture information from a custom source.</span></span> <span data-ttu-id="2266e-122">例如，自訂來源可以是設定檔或資料庫。</span><span class="sxs-lookup"><span data-stu-id="2266e-122">For example, the custom source can be a configuration file or database.</span></span>

<span data-ttu-id="2266e-123">下列範例顯示 `AppSettingsRequestCultureProvider`，其會擴充 <xref:Microsoft.AspNetCore.Localization.RequestCultureProvider> 以判斷來自 *appsettings.json* 的要求文化特性資訊：</span><span class="sxs-lookup"><span data-stu-id="2266e-123">The following example shows `AppSettingsRequestCultureProvider`, which extends the <xref:Microsoft.AspNetCore.Localization.RequestCultureProvider> to determine the request culture information from *appsettings.json*:</span></span>

```csharp
public class AppSettingsRequestCultureProvider : RequestCultureProvider
{
    public string CultureKey { get; set; } = "culture";

    public string UICultureKey { get; set; } = "ui-culture";

    public override Task<ProviderCultureResult> DetermineProviderCultureResult(HttpContext httpContext)
    {
        if (httpContext == null)
        {
            throw new ArgumentNullException();
        }

        var configuration = httpContext.RequestServices.GetService<IConfigurationRoot>();
        var culture = configuration[CultureKey];
        var uiCulture = configuration[UICultureKey];

        if (culture == null && uiCulture == null)
        {
            return Task.FromResult((ProviderCultureResult)null);
        }

        if (culture != null && uiCulture == null)
        {
            uiCulture = culture;
        }

        if (culture == null && uiCulture != null)
        {
            culture = uiCulture;
        }
        
        var providerResultCulture = new ProviderCultureResult(culture, uiCulture);

        return Task.FromResult(providerResultCulture);
    }
}
```

## <a name="localization-resources"></a><span data-ttu-id="2266e-124">當地語系化資源</span><span class="sxs-lookup"><span data-stu-id="2266e-124">Localization resources</span></span>

<span data-ttu-id="2266e-125">ASP.NET Core 當地語系化會提供 <xref:Microsoft.Extensions.Localization.ResourceManagerStringLocalizer>。</span><span class="sxs-lookup"><span data-stu-id="2266e-125">ASP.NET Core localization provides <xref:Microsoft.Extensions.Localization.ResourceManagerStringLocalizer>.</span></span> <span data-ttu-id="2266e-126"><xref:Microsoft.Extensions.Localization.ResourceManagerStringLocalizer> 是使用 `resx` 來儲存當地語系化資源的 <xref:Microsoft.Extensions.Localization.IStringLocalizer> 實作。</span><span class="sxs-lookup"><span data-stu-id="2266e-126"><xref:Microsoft.Extensions.Localization.ResourceManagerStringLocalizer> is an implementation of <xref:Microsoft.Extensions.Localization.IStringLocalizer> that is uses `resx` to store localization resources.</span></span>

<span data-ttu-id="2266e-127">您不一定要使用 `resx` 檔案。</span><span class="sxs-lookup"><span data-stu-id="2266e-127">You aren't limited to using `resx` files.</span></span> <span data-ttu-id="2266e-128">藉由實作 `IStringLocalized`，即可使用任何資料來源。</span><span class="sxs-lookup"><span data-stu-id="2266e-128">By implementing `IStringLocalized`, any data source can be used.</span></span>

<span data-ttu-id="2266e-129">下列範例專案會實作 <xref:Microsoft.Extensions.Localization.IStringLocalizer>：</span><span class="sxs-lookup"><span data-stu-id="2266e-129">The following example projects implement <xref:Microsoft.Extensions.Localization.IStringLocalizer>:</span></span> 

* [<span data-ttu-id="2266e-130">EFStringLocalizer</span><span class="sxs-lookup"><span data-stu-id="2266e-130">EFStringLocalizer</span></span>](https://github.com/aspnet/Entropy/tree/master/samples/Localization.EntityFramework)
* [<span data-ttu-id="2266e-131">JsonStringLocalizer</span><span class="sxs-lookup"><span data-stu-id="2266e-131">JsonStringLocalizer</span></span>](https://github.com/hishamco/My.Extensions.Localization.Json)
* [<span data-ttu-id="2266e-132">SqlLocalizer</span><span class="sxs-lookup"><span data-stu-id="2266e-132">SqlLocalizer</span></span>](https://github.com/damienbod/AspNetCoreLocalization)
