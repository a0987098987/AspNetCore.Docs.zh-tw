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
# <a name="localization-extensibility"></a>當地語系化擴充性

作者為 [Hisham Bin Ateya](https://github.com/hishamco)

這篇文章：

* 列出當地語系化 API 的擴充點。
* 提供如何擴充 ASP.NET Core 應用程式當地語系化的指示。

## <a name="extensible-points-in-localization-apis"></a>當地語系化 API 中的擴充點

ASP.NET Core 當地語系化 API 已建置為可擴充的。 擴充性讓開發人員能夠根據自己的需求自訂當地語系化。 例如，[OrchardCore](https://github.com/orchardCMS/OrchardCore/) 有一個 `POStringLocalizer`。 `POStringLocalizer` 會詳細說明如何使用[可攜式物件當地語系化](xref:fundamentals/portable-object-localization)，使用 `PO` 檔案來儲存當地語系化資源。

本文列出當地語系化 API 所提供的兩個主要擴充點： 

* <xref:Microsoft.AspNetCore.Localization.RequestCultureProvider>
* <xref:Microsoft.Extensions.Localization.IStringLocalizer>

## <a name="localization-culture-providers"></a>當地語系化文化特性提供者

ASP.NET Core 當地語系化 API 具有四個預設提供者，可判斷執行中要求目前的文化特性：

* <xref:Microsoft.AspNetCore.Localization.QueryStringRequestCultureProvider>
* <xref:Microsoft.AspNetCore.Localization.CookieRequestCultureProvider>
* <xref:Microsoft.AspNetCore.Localization.AcceptLanguageHeaderRequestCultureProvider>
* <xref:Microsoft.AspNetCore.Localization.CustomRequestCultureProvider>

[當地語系化中介軟體](xref:fundamentals/localization)文件中會詳細說明上述提供者。 如果預設提供者不符合您的需求，請使用下列其中一種方法來建立自訂提供者：

### <a name="use-customrequestcultureprovider"></a>使用 CustomRequestCultureProvider

<xref:Microsoft.AspNetCore.Localization.CustomRequestCultureProvider> 提供自訂的 <xref:Microsoft.AspNetCore.Localization.RequestCultureProvider>，使用簡單的委派來判斷目前的當地語系化文化特性：

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

### <a name="use-a-new-implemetation-of-requestcultureprovider"></a>使用 RequestCultureProvider 的新實作

<xref:Microsoft.AspNetCore.Localization.RequestCultureProvider> 的新實作可建立來從自訂來源判斷要求文化特性資訊。 例如，自訂來源可以是設定檔或資料庫。

下列範例顯示 `AppSettingsRequestCultureProvider`，其會擴充 <xref:Microsoft.AspNetCore.Localization.RequestCultureProvider> 以判斷來自 *appsettings.json* 的要求文化特性資訊：

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

## <a name="localization-resources"></a>當地語系化資源

ASP.NET Core 當地語系化會提供 <xref:Microsoft.Extensions.Localization.ResourceManagerStringLocalizer>。 <xref:Microsoft.Extensions.Localization.ResourceManagerStringLocalizer> 是使用 `resx` 來儲存當地語系化資源的 <xref:Microsoft.Extensions.Localization.IStringLocalizer> 實作。

您不一定要使用 `resx` 檔案。 藉由實作 `IStringLocalized`，即可使用任何資料來源。

下列範例專案會實作 <xref:Microsoft.Extensions.Localization.IStringLocalizer>： 

* [EFStringLocalizer](https://github.com/aspnet/Entropy/tree/master/samples/Localization.EntityFramework)
* [JsonStringLocalizer](https://github.com/hishamco/My.Extensions.Localization.Json)
* [SqlLocalizer](https://github.com/damienbod/AspNetCoreLocalization)
