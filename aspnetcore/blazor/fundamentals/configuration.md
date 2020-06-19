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
ms.openlocfilehash: b43eae03c71cabbaafa2bc0d704765e89f743279
ms.sourcegitcommit: 490434a700ba8c5ed24d849bd99d8489858538e3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/19/2020
ms.locfileid: "85103614"
---
# <a name="aspnet-core-blazor-configuration"></a><span data-ttu-id="b0f54-103">ASP.NET Core Blazor 設定</span><span class="sxs-lookup"><span data-stu-id="b0f54-103">ASP.NET Core Blazor configuration</span></span>

> [!NOTE]
> <span data-ttu-id="b0f54-104">本主題適用于 Blazor WebAssembly。</span><span class="sxs-lookup"><span data-stu-id="b0f54-104">This topic applies to Blazor WebAssembly.</span></span> <span data-ttu-id="b0f54-105">如需 ASP.NET Core 應用程式設定的一般指引，請參閱 <xref:fundamentals/configuration/index> 。</span><span class="sxs-lookup"><span data-stu-id="b0f54-105">For general guidance on ASP.NET Core app configuration, see <xref:fundamentals/configuration/index>.</span></span>

Blazor<span data-ttu-id="b0f54-106">WebAssembly 會從載入設定：</span><span class="sxs-lookup"><span data-stu-id="b0f54-106"> WebAssembly loads configuration from:</span></span>

* <span data-ttu-id="b0f54-107">應用程式佈建檔案（預設為）：</span><span class="sxs-lookup"><span data-stu-id="b0f54-107">App settings files by default:</span></span>
  * <span data-ttu-id="b0f54-108">*wwwroot/appsettings.js開啟*</span><span class="sxs-lookup"><span data-stu-id="b0f54-108">*wwwroot/appsettings.json*</span></span>
  * <span data-ttu-id="b0f54-109">*wwwroot/appsettings。{環境}. json*</span><span class="sxs-lookup"><span data-stu-id="b0f54-109">*wwwroot/appsettings.{ENVIRONMENT}.json*</span></span>
* <span data-ttu-id="b0f54-110">應用程式註冊的其他設定[提供者](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="b0f54-110">Other [configuration providers](xref:fundamentals/configuration/index) registered by the app.</span></span> <span data-ttu-id="b0f54-111">並非所有提供者都適用于 Blazor WebAssembly apps。</span><span class="sxs-lookup"><span data-stu-id="b0f54-111">Not all providers are appropriate for Blazor WebAssembly apps.</span></span> <span data-ttu-id="b0f54-112">澄清 Blazor [ Blazor WASM （dotnet/AspNetCore.Doc#18134）的設定提供者](https://github.com/dotnet/AspNetCore.Docs/issues/18134)，即可追蹤支援 WebAssembly 的提供者。</span><span class="sxs-lookup"><span data-stu-id="b0f54-112">Clarification on which providers are supported for Blazor WebAssembly is tracked by [Clarify configuration providers for Blazor WASM (dotnet/AspNetCore.Docs #18134)](https://github.com/dotnet/AspNetCore.Docs/issues/18134).</span></span>

> [!WARNING]
> <span data-ttu-id="b0f54-113">Blazor使用者可以看到 WebAssembly 應用程式中的設定。</span><span class="sxs-lookup"><span data-stu-id="b0f54-113">Configuration in a Blazor WebAssembly app is visible to users.</span></span> <span data-ttu-id="b0f54-114">**請勿在設定中儲存應用程式秘密或認證。**</span><span class="sxs-lookup"><span data-stu-id="b0f54-114">**Don't store app secrets or credentials in configuration.**</span></span>

<span data-ttu-id="b0f54-115">如需設定提供者的詳細資訊，請參閱 <xref:fundamentals/configuration/index> 。</span><span class="sxs-lookup"><span data-stu-id="b0f54-115">For more information on configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="app-settings-configuration"></a><span data-ttu-id="b0f54-116">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="b0f54-116">App settings configuration</span></span>

<span data-ttu-id="b0f54-117">*wwwroot/appsettings.js開啟*：</span><span class="sxs-lookup"><span data-stu-id="b0f54-117">*wwwroot/appsettings.json*:</span></span>

```json
{
  "message": "Hello from config!"
}
```

<span data-ttu-id="b0f54-118">將 <xref:Microsoft.Extensions.Configuration.IConfiguration> 實例插入元件以存取設定資料：</span><span class="sxs-lookup"><span data-stu-id="b0f54-118">Inject an <xref:Microsoft.Extensions.Configuration.IConfiguration> instance into a component to access the configuration data:</span></span>

```razor
@page "/"
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<h1>Configuration example</h1>

<p>Message: @Configuration["message"]</p>
```

## <a name="provider-configuration"></a><span data-ttu-id="b0f54-119">提供者設定</span><span class="sxs-lookup"><span data-stu-id="b0f54-119">Provider configuration</span></span>

<span data-ttu-id="b0f54-120">下列範例會使用 <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationSource> 來提供額外的設定：</span><span class="sxs-lookup"><span data-stu-id="b0f54-120">The following example uses a <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationSource> to supply additional configuration:</span></span>

<span data-ttu-id="b0f54-121">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="b0f54-121">`Program.Main`:</span></span>

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

<span data-ttu-id="b0f54-122">將 <xref:Microsoft.Extensions.Configuration.IConfiguration> 實例插入元件以存取設定資料：</span><span class="sxs-lookup"><span data-stu-id="b0f54-122">Inject an <xref:Microsoft.Extensions.Configuration.IConfiguration> instance into a component to access the configuration data:</span></span>

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

<span data-ttu-id="b0f54-123">若要將*wwwroot*資料夾中的其他設定檔讀取到設定中，請使用 <xref:System.Net.Http.HttpClient> 來取得檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="b0f54-123">To read other configuration files from the *wwwroot* folder into configuration, use an <xref:System.Net.Http.HttpClient> to obtain the file's content.</span></span> <span data-ttu-id="b0f54-124">使用此方法時，現有的 <xref:System.Net.Http.HttpClient> 服務註冊可以使用已建立的本機用戶端來讀取檔案，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="b0f54-124">When using this approach, the existing <xref:System.Net.Http.HttpClient> service registration can use the local client created to read the file, as the following example shows:</span></span>

<span data-ttu-id="b0f54-125">*wwwroot/cars.js開啟*：</span><span class="sxs-lookup"><span data-stu-id="b0f54-125">*wwwroot/cars.json*:</span></span>

```json
{
    "size": "tiny"
}
```

<span data-ttu-id="b0f54-126">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="b0f54-126">`Program.Main`:</span></span>

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

## <a name="authentication-configuration"></a><span data-ttu-id="b0f54-127">驗證設定</span><span class="sxs-lookup"><span data-stu-id="b0f54-127">Authentication configuration</span></span>

<span data-ttu-id="b0f54-128">*wwwroot/appsettings.js開啟*：</span><span class="sxs-lookup"><span data-stu-id="b0f54-128">*wwwroot/appsettings.json*:</span></span>

```json
{
  "Local": {
    "Authority": "{AUTHORITY}",
    "ClientId": "{CLIENT ID}"
  }
}
```

<span data-ttu-id="b0f54-129">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="b0f54-129">`Program.Main`:</span></span>

```csharp
builder.Services.AddOidcAuthentication(options =>
    builder.Configuration.Bind("Local", options.ProviderOptions));
```

## <a name="logging-configuration"></a><span data-ttu-id="b0f54-130">記錄設定</span><span class="sxs-lookup"><span data-stu-id="b0f54-130">Logging configuration</span></span>

<span data-ttu-id="b0f54-131">新增[Microsoft.Extensions.Logging.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Configuration/)的套件參考：</span><span class="sxs-lookup"><span data-stu-id="b0f54-131">Add a package reference for [Microsoft.Extensions.Logging.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Configuration/):</span></span>

```xml
<PackageReference Include="Microsoft.Extensions.Logging.Configuration" Version="{VERSION}" />
```

<span data-ttu-id="b0f54-132">*wwwroot/appsettings.js開啟*：</span><span class="sxs-lookup"><span data-stu-id="b0f54-132">*wwwroot/appsettings.json*:</span></span>

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

<span data-ttu-id="b0f54-133">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="b0f54-133">`Program.Main`:</span></span>

```csharp
using Microsoft.Extensions.Logging;

...

builder.Logging.AddConfiguration(
    builder.Configuration.GetSection("Logging"));
```

## <a name="host-builder-configuration"></a><span data-ttu-id="b0f54-134">主機產生器設定</span><span class="sxs-lookup"><span data-stu-id="b0f54-134">Host builder configuration</span></span>

<span data-ttu-id="b0f54-135">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="b0f54-135">`Program.Main`:</span></span>

```csharp
var hostname = builder.Configuration["HostName"];
```

## <a name="cached-configuration"></a><span data-ttu-id="b0f54-136">快取設定</span><span class="sxs-lookup"><span data-stu-id="b0f54-136">Cached configuration</span></span>

<span data-ttu-id="b0f54-137">系統會快取設定檔以供離線使用。</span><span class="sxs-lookup"><span data-stu-id="b0f54-137">Configuration files are cached for offline use.</span></span> <span data-ttu-id="b0f54-138">使用[漸進式 Web 應用程式（pwa）](xref:blazor/progressive-web-app)，您只能在建立新的部署時更新設定檔。</span><span class="sxs-lookup"><span data-stu-id="b0f54-138">With [Progressive Web Applications (PWAs)](xref:blazor/progressive-web-app), you can only update configuration files when creating a new deployment.</span></span> <span data-ttu-id="b0f54-139">在部署之間編輯設定檔沒有任何作用，因為：</span><span class="sxs-lookup"><span data-stu-id="b0f54-139">Editing configuration files between deployments has no effect because:</span></span>

* <span data-ttu-id="b0f54-140">使用者有檔案的快取版本，這些檔案會繼續使用。</span><span class="sxs-lookup"><span data-stu-id="b0f54-140">Users have cached versions of the files that they continue to use.</span></span>
* <span data-ttu-id="b0f54-141">PWA 的*service-worker.js*和*service-worker-assets.js*檔案必須在編譯時重建，這會在使用者下一次線上流覽應用程式，以重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="b0f54-141">The PWA's *service-worker.js* and *service-worker-assets.js* files must be rebuilt on compilation, which signal to the app on the user's next online visit that the app has been redeployed.</span></span>

<span data-ttu-id="b0f54-142">如需 Pwa 如何處理背景更新的詳細資訊，請參閱 <xref:blazor/progressive-web-app#background-updates> 。</span><span class="sxs-lookup"><span data-stu-id="b0f54-142">For more information on how background updates are handled by PWAs, see <xref:blazor/progressive-web-app#background-updates>.</span></span>
