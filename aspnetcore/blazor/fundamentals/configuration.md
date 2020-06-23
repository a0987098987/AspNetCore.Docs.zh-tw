---
title: ASP.NET Core Blazor 設定
author: guardrex
description: 瞭解應用程式的 Blazor 設定，包括應用程式設定、驗證和記錄設定。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/10/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/fundamentals/configuration
ms.openlocfilehash: 0e36b81d771b07e85158724c02210ee50a3ab118
ms.sourcegitcommit: 066d66ea150f8aab63f9e0e0668b06c9426296fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/23/2020
ms.locfileid: "85242676"
---
# <a name="aspnet-core-blazor-configuration"></a><span data-ttu-id="b3078-103">ASP.NET Core Blazor 設定</span><span class="sxs-lookup"><span data-stu-id="b3078-103">ASP.NET Core Blazor configuration</span></span>

> [!NOTE]
> <span data-ttu-id="b3078-104">本主題適用于 Blazor WebAssembly。</span><span class="sxs-lookup"><span data-stu-id="b3078-104">This topic applies to Blazor WebAssembly.</span></span> <span data-ttu-id="b3078-105">如需 ASP.NET Core 應用程式設定的一般指引，請參閱 <xref:fundamentals/configuration/index> 。</span><span class="sxs-lookup"><span data-stu-id="b3078-105">For general guidance on ASP.NET Core app configuration, see <xref:fundamentals/configuration/index>.</span></span>

Blazor<span data-ttu-id="b3078-106">WebAssembly 會從載入設定：</span><span class="sxs-lookup"><span data-stu-id="b3078-106"> WebAssembly loads configuration from:</span></span>

* <span data-ttu-id="b3078-107">應用程式佈建檔案（預設為）：</span><span class="sxs-lookup"><span data-stu-id="b3078-107">App settings files by default:</span></span>
  * `wwwroot/appsettings.json`
  * `wwwroot/appsettings.{ENVIRONMENT}.json`
* <span data-ttu-id="b3078-108">應用程式註冊的其他設定[提供者](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="b3078-108">Other [configuration providers](xref:fundamentals/configuration/index) registered by the app.</span></span> <span data-ttu-id="b3078-109">並非所有提供者都適用于 Blazor WebAssembly apps。</span><span class="sxs-lookup"><span data-stu-id="b3078-109">Not all providers are appropriate for Blazor WebAssembly apps.</span></span> <span data-ttu-id="b3078-110">澄清 Blazor [ Blazor WASM （dotnet/AspNetCore.Doc#18134）的設定提供者](https://github.com/dotnet/AspNetCore.Docs/issues/18134)，即可追蹤支援 WebAssembly 的提供者。</span><span class="sxs-lookup"><span data-stu-id="b3078-110">Clarification on which providers are supported for Blazor WebAssembly is tracked by [Clarify configuration providers for Blazor WASM (dotnet/AspNetCore.Docs #18134)](https://github.com/dotnet/AspNetCore.Docs/issues/18134).</span></span>

> [!WARNING]
> <span data-ttu-id="b3078-111">Blazor使用者可以看到 WebAssembly 應用程式中的設定。</span><span class="sxs-lookup"><span data-stu-id="b3078-111">Configuration in a Blazor WebAssembly app is visible to users.</span></span> <span data-ttu-id="b3078-112">**請勿在設定中儲存應用程式秘密或認證。**</span><span class="sxs-lookup"><span data-stu-id="b3078-112">**Don't store app secrets or credentials in configuration.**</span></span>

<span data-ttu-id="b3078-113">如需設定提供者的詳細資訊，請參閱 <xref:fundamentals/configuration/index> 。</span><span class="sxs-lookup"><span data-stu-id="b3078-113">For more information on configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="app-settings-configuration"></a><span data-ttu-id="b3078-114">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="b3078-114">App settings configuration</span></span>

<span data-ttu-id="b3078-115">`wwwroot/appsettings.json`:</span><span class="sxs-lookup"><span data-stu-id="b3078-115">`wwwroot/appsettings.json`:</span></span>

```json
{
  "message": "Hello from config!"
}
```

<span data-ttu-id="b3078-116">將 <xref:Microsoft.Extensions.Configuration.IConfiguration> 實例插入元件以存取設定資料：</span><span class="sxs-lookup"><span data-stu-id="b3078-116">Inject an <xref:Microsoft.Extensions.Configuration.IConfiguration> instance into a component to access the configuration data:</span></span>

```razor
@page "/"
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<h1>Configuration example</h1>

<p>Message: @Configuration["message"]</p>
```

## <a name="provider-configuration"></a><span data-ttu-id="b3078-117">提供者設定</span><span class="sxs-lookup"><span data-stu-id="b3078-117">Provider configuration</span></span>

<span data-ttu-id="b3078-118">下列範例會使用 <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationSource> 來提供額外的設定：</span><span class="sxs-lookup"><span data-stu-id="b3078-118">The following example uses a <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationSource> to supply additional configuration:</span></span>

<span data-ttu-id="b3078-119">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="b3078-119">`Program.Main`:</span></span>

```csharp
using Microsoft.Extensions.Configuration.Memory;

...

var vehicleData = new Dictionary<string, string>()
{
    { "color", "blue" },
    { "type", "car" },
    { "wheels:count", "3" },
    { "wheels:brand", "Blazin" },
    { "wheels:brand:type", "rally" },
    { "wheels:year", "2008" },
};

var memoryConfig = new MemoryConfigurationSource { InitialData = vehicleData };

...

builder.Configuration.Add(memoryConfig);
```

<span data-ttu-id="b3078-120">將 <xref:Microsoft.Extensions.Configuration.IConfiguration> 實例插入元件以存取設定資料：</span><span class="sxs-lookup"><span data-stu-id="b3078-120">Inject an <xref:Microsoft.Extensions.Configuration.IConfiguration> instance into a component to access the configuration data:</span></span>

```razor
@page "/"
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<h1>Configuration example</h1>

<h2>Wheels</h2>

<ul>
    <li>Count: @Configuration["wheels:count"]</li>
    <li>Brand: @Configuration["wheels:brand"]</li>
    <li>Type: @Configuration["wheels:brand:type"]</li>
    <li>Year: @Configuration["wheels:year"]</li>
</ul>

@code {
    var wheelsSection = Configuration.GetSection("wheels");
    
    ...
}
```

<span data-ttu-id="b3078-121">若要將資料夾中的其他設定檔讀取到設定中 `wwwroot` ，請使用 <xref:System.Net.Http.HttpClient> 來取得檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="b3078-121">To read other configuration files from the `wwwroot` folder into configuration, use an <xref:System.Net.Http.HttpClient> to obtain the file's content.</span></span> <span data-ttu-id="b3078-122">使用此方法時，現有的 <xref:System.Net.Http.HttpClient> 服務註冊可以使用已建立的本機用戶端來讀取檔案，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="b3078-122">When using this approach, the existing <xref:System.Net.Http.HttpClient> service registration can use the local client created to read the file, as the following example shows:</span></span>

<span data-ttu-id="b3078-123">`wwwroot/cars.json`:</span><span class="sxs-lookup"><span data-stu-id="b3078-123">`wwwroot/cars.json`:</span></span>

```json
{
    "size": "tiny"
}
```

<span data-ttu-id="b3078-124">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="b3078-124">`Program.Main`:</span></span>

```csharp
using Microsoft.Extensions.Configuration;

...

var client = new HttpClient()
{
    BaseAddress = new Uri(builder.HostEnvironment.BaseAddress)
};

builder.Services.AddTransient(sp => client);

using var response = await client.GetAsync("cars.json");
using var stream = await response.Content.ReadAsStreamAsync();

builder.Configuration.AddJsonStream(stream);
```

## <a name="authentication-configuration"></a><span data-ttu-id="b3078-125">驗證設定</span><span class="sxs-lookup"><span data-stu-id="b3078-125">Authentication configuration</span></span>

<span data-ttu-id="b3078-126">`wwwroot/appsettings.json`:</span><span class="sxs-lookup"><span data-stu-id="b3078-126">`wwwroot/appsettings.json`:</span></span>

```json
{
  "Local": {
    "Authority": "{AUTHORITY}",
    "ClientId": "{CLIENT ID}"
  }
}
```

<span data-ttu-id="b3078-127">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="b3078-127">`Program.Main`:</span></span>

```csharp
builder.Services.AddOidcAuthentication(options =>
    builder.Configuration.Bind("Local", options.ProviderOptions));
```

## <a name="logging-configuration"></a><span data-ttu-id="b3078-128">記錄設定</span><span class="sxs-lookup"><span data-stu-id="b3078-128">Logging configuration</span></span>

<span data-ttu-id="b3078-129">新增的套件參考 [`Microsoft.Extensions.Logging.Configuration`](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Configuration/) ：</span><span class="sxs-lookup"><span data-stu-id="b3078-129">Add a package reference for [`Microsoft.Extensions.Logging.Configuration`](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Configuration/):</span></span>

```xml
<PackageReference Include="Microsoft.Extensions.Logging.Configuration" Version="{VERSION}" />
```

<span data-ttu-id="b3078-130">`wwwroot/appsettings.json`:</span><span class="sxs-lookup"><span data-stu-id="b3078-130">`wwwroot/appsettings.json`:</span></span>

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  }
}
```

<span data-ttu-id="b3078-131">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="b3078-131">`Program.Main`:</span></span>

```csharp
using Microsoft.Extensions.Logging;

...

builder.Logging.AddConfiguration(
    builder.Configuration.GetSection("Logging"));
```

## <a name="host-builder-configuration"></a><span data-ttu-id="b3078-132">主機產生器設定</span><span class="sxs-lookup"><span data-stu-id="b3078-132">Host builder configuration</span></span>

<span data-ttu-id="b3078-133">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="b3078-133">`Program.Main`:</span></span>

```csharp
var hostname = builder.Configuration["HostName"];
```

## <a name="cached-configuration"></a><span data-ttu-id="b3078-134">快取設定</span><span class="sxs-lookup"><span data-stu-id="b3078-134">Cached configuration</span></span>

<span data-ttu-id="b3078-135">系統會快取設定檔以供離線使用。</span><span class="sxs-lookup"><span data-stu-id="b3078-135">Configuration files are cached for offline use.</span></span> <span data-ttu-id="b3078-136">使用[漸進式 Web 應用程式（pwa）](xref:blazor/progressive-web-app)，您只能在建立新的部署時更新設定檔。</span><span class="sxs-lookup"><span data-stu-id="b3078-136">With [Progressive Web Applications (PWAs)](xref:blazor/progressive-web-app), you can only update configuration files when creating a new deployment.</span></span> <span data-ttu-id="b3078-137">在部署之間編輯設定檔沒有任何作用，因為：</span><span class="sxs-lookup"><span data-stu-id="b3078-137">Editing configuration files between deployments has no effect because:</span></span>

* <span data-ttu-id="b3078-138">使用者有檔案的快取版本，這些檔案會繼續使用。</span><span class="sxs-lookup"><span data-stu-id="b3078-138">Users have cached versions of the files that they continue to use.</span></span>
* <span data-ttu-id="b3078-139">在 `service-worker.js` 編譯時，PWA 的和檔案 `service-worker-assets.js` 必須重建，在使用者下一次線上流覽應用程式時，請造訪應用程式已重新部署。</span><span class="sxs-lookup"><span data-stu-id="b3078-139">The PWA's `service-worker.js` and `service-worker-assets.js` files must be rebuilt on compilation, which signal to the app on the user's next online visit that the app has been redeployed.</span></span>

<span data-ttu-id="b3078-140">如需 Pwa 如何處理背景更新的詳細資訊，請參閱 <xref:blazor/progressive-web-app#background-updates> 。</span><span class="sxs-lookup"><span data-stu-id="b3078-140">For more information on how background updates are handled by PWAs, see <xref:blazor/progressive-web-app#background-updates>.</span></span>
