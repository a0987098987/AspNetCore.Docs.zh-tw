---
title: ASP.NET Core Blazor裝載模型設定
author: guardrex
description: 瞭解Blazor主控模型設定，包括如何將元件整合Razor至Razor頁面和 MVC 應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/04/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: 17ed43a12643f067da73658bec72400acbe1be43
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82772070"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="f1914-103">ASP.NET Core Blazor 裝載模型設定</span><span class="sxs-lookup"><span data-stu-id="f1914-103">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="f1914-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f1914-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="f1914-105">本文涵蓋主控模型設定。</span><span class="sxs-lookup"><span data-stu-id="f1914-105">This article covers hosting model configuration.</span></span>

## <a name="blazor-webassembly"></a><span data-ttu-id="f1914-106">Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="f1914-106">Blazor WebAssembly</span></span>

### <a name="environment"></a><span data-ttu-id="f1914-107">環境</span><span class="sxs-lookup"><span data-stu-id="f1914-107">Environment</span></span>

<span data-ttu-id="f1914-108">在本機執行應用程式時，環境會預設為開發。</span><span class="sxs-lookup"><span data-stu-id="f1914-108">When running an app locally, the environment defaults to Development.</span></span> <span data-ttu-id="f1914-109">當應用程式發佈時，環境會預設為 [生產]。</span><span class="sxs-lookup"><span data-stu-id="f1914-109">When the app is published, the environment defaults to Production.</span></span>

<span data-ttu-id="f1914-110">Hosted Blazor WebAssembly 應用程式會透過中介軟體來從伺服器挑選環境，藉由新增`blazor-environment`標頭來將環境傳達給瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="f1914-110">A hosted Blazor WebAssembly app picks up the environment from the server via a middleware that communicates the environment to the browser by adding the `blazor-environment` header.</span></span> <span data-ttu-id="f1914-111">標頭的值為環境。</span><span class="sxs-lookup"><span data-stu-id="f1914-111">The value of the header is the environment.</span></span> <span data-ttu-id="f1914-112">託管的 Blazor 應用程式和伺服器應用程式會共用相同的環境。</span><span class="sxs-lookup"><span data-stu-id="f1914-112">The hosted Blazor app and the server app share the same environment.</span></span> <span data-ttu-id="f1914-113">如需詳細資訊，包括如何設定環境，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="f1914-113">For more information, including how to configure the environment, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="f1914-114">針對在本機執行的獨立應用程式，開發伺服器會`blazor-environment`新增標頭以指定開發環境。</span><span class="sxs-lookup"><span data-stu-id="f1914-114">For a standalone app running locally, the development server adds the `blazor-environment` header to specify the Development environment.</span></span> <span data-ttu-id="f1914-115">若要指定其他裝載環境的環境，請新增`blazor-environment`標頭。</span><span class="sxs-lookup"><span data-stu-id="f1914-115">To specify the environment for other hosting environments, add the `blazor-environment` header.</span></span>

<span data-ttu-id="f1914-116">在 IIS 的下列範例中，將自訂標頭加入至*已發佈的 web.config 檔案*。</span><span class="sxs-lookup"><span data-stu-id="f1914-116">In the following example for IIS, add the custom header to the published *web.config* file.</span></span> <span data-ttu-id="f1914-117">Web.config*檔案*位於*bin/RELEASE/{TARGET FRAMEWORK}/publish*資料夾中：</span><span class="sxs-lookup"><span data-stu-id="f1914-117">The *web.config* file is located in the *bin/Release/{TARGET FRAMEWORK}/publish* folder:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>

    ...

    <httpProtocol>
      <customHeaders>
        <add name="blazor-environment" value="Staging" />
      </customHeaders>
    </httpProtocol>
  </system.webServer>
</configuration>
```

> [!NOTE]
> <span data-ttu-id="f1914-118">若要使用 IIS 的自訂*web.config*檔案，而該檔案不會在應用程式發行至*發行*資料夾時遭到<xref:host-and-deploy/blazor/webassembly#use-a-custom-webconfig>覆寫，請參閱。</span><span class="sxs-lookup"><span data-stu-id="f1914-118">To use a custom *web.config* file for IIS that isn't overwritten when the app is published to the *publish* folder, see <xref:host-and-deploy/blazor/webassembly#use-a-custom-webconfig>.</span></span>

<span data-ttu-id="f1914-119">藉由插入`IWebAssemblyHostEnvironment`和讀取`Environment`屬性，在元件中取得應用程式的環境：</span><span class="sxs-lookup"><span data-stu-id="f1914-119">Obtain the app's environment in a component by injecting `IWebAssemblyHostEnvironment` and reading the `Environment` property:</span></span>

```razor
@page "/"
@using Microsoft.AspNetCore.Components.WebAssembly.Hosting
@inject IWebAssemblyHostEnvironment HostEnvironment

<h1>Environment example</h1>

<p>Environment: @HostEnvironment.Environment</p>
```

<span data-ttu-id="f1914-120">在啟動期間， `WebAssemblyHostBuilder`會`IWebAssemblyHostEnvironment`透過`HostEnvironment`屬性公開，讓開發人員在其程式碼中具有環境特定邏輯：</span><span class="sxs-lookup"><span data-stu-id="f1914-120">During startup, the `WebAssemblyHostBuilder` exposes the `IWebAssemblyHostEnvironment` through the `HostEnvironment` property, which enables developers to have environment-specific logic in their code:</span></span>

```csharp
if (builder.HostEnvironment.Environment == "Custom")
{
    ...
};
```

<span data-ttu-id="f1914-121">下列便利的擴充方法可讓您檢查目前的環境，以進行開發、生產、預備和自訂環境名稱：</span><span class="sxs-lookup"><span data-stu-id="f1914-121">The following convenience extension methods permit checking the current environment for Development, Production, Staging, and custom environment names:</span></span>

* `IsDevelopment()`
* `IsProduction()`
* `IsStaging()`
* <span data-ttu-id="f1914-122">' IsEnvironment （"{環境名稱}"）</span><span class="sxs-lookup"><span data-stu-id="f1914-122">\`IsEnvironment("{ENVIRONMENT NAME}")</span></span>

```csharp
if (builder.HostEnvironment.IsStaging())
{
    ...
};

if (builder.HostEnvironment.IsEnvironment("Custom"))
{
    ...
};
```

<span data-ttu-id="f1914-123">當`IWebAssemblyHostEnvironment.BaseAddress` `NavigationManager`服務無法使用時，可以在啟動期間使用此屬性。</span><span class="sxs-lookup"><span data-stu-id="f1914-123">The `IWebAssemblyHostEnvironment.BaseAddress` property can be used during startup when the `NavigationManager` service isn't available.</span></span>

### <a name="configuration"></a><span data-ttu-id="f1914-124">設定</span><span class="sxs-lookup"><span data-stu-id="f1914-124">Configuration</span></span>

<span data-ttu-id="f1914-125">Blazor WebAssembly 會從載入設定：</span><span class="sxs-lookup"><span data-stu-id="f1914-125">Blazor WebAssembly loads configuration from:</span></span>

* <span data-ttu-id="f1914-126">應用程式佈建檔案（預設為）：</span><span class="sxs-lookup"><span data-stu-id="f1914-126">App settings files by default:</span></span>
  * <span data-ttu-id="f1914-127">*wwwroot/appsettings. json*</span><span class="sxs-lookup"><span data-stu-id="f1914-127">*wwwroot/appsettings.json*</span></span>
  * <span data-ttu-id="f1914-128">*wwwroot/appsettings。{環境}. json*</span><span class="sxs-lookup"><span data-stu-id="f1914-128">*wwwroot/appsettings.{ENVIRONMENT}.json*</span></span>
* <span data-ttu-id="f1914-129">應用程式註冊的其他設定[提供者](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="f1914-129">Other [configuration providers](xref:fundamentals/configuration/index) registered by the app.</span></span> <span data-ttu-id="f1914-130">並非所有提供者都適用于 Blazor WebAssembly 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f1914-130">Not all providers are appropriate for Blazor WebAssembly apps.</span></span> <span data-ttu-id="f1914-131">澄清[BLAZOR WASM 的設定提供者（dotnet/AspNetCore #18134）](https://github.com/dotnet/AspNetCore.Docs/issues/18134)可追蹤 Blazor WebAssembly 支援的提供者。</span><span class="sxs-lookup"><span data-stu-id="f1914-131">Clarification on which providers are supported for Blazor WebAssembly is tracked by [Clarify configuration providers for Blazor WASM (dotnet/AspNetCore.Docs #18134)](https://github.com/dotnet/AspNetCore.Docs/issues/18134).</span></span>

> [!WARNING]
> <span data-ttu-id="f1914-132">使用者可以看到 Blazor WebAssembly 應用程式中的設定。</span><span class="sxs-lookup"><span data-stu-id="f1914-132">Configuration in a Blazor WebAssembly app is visible to users.</span></span> <span data-ttu-id="f1914-133">**請勿在設定中儲存應用程式秘密或認證。**</span><span class="sxs-lookup"><span data-stu-id="f1914-133">**Don't store app secrets or credentials in configuration.**</span></span>

<span data-ttu-id="f1914-134">如需設定提供者的詳細資訊<xref:fundamentals/configuration/index>，請參閱。</span><span class="sxs-lookup"><span data-stu-id="f1914-134">For more information on configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

#### <a name="app-settings-configuration"></a><span data-ttu-id="f1914-135">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="f1914-135">App settings configuration</span></span>

<span data-ttu-id="f1914-136">*wwwroot/appsettings. json*：</span><span class="sxs-lookup"><span data-stu-id="f1914-136">*wwwroot/appsettings.json*:</span></span>

```json
{
  "message": "Hello from config!"
}
```

<span data-ttu-id="f1914-137">將<xref:Microsoft.Extensions.Configuration.IConfiguration>實例插入元件以存取設定資料：</span><span class="sxs-lookup"><span data-stu-id="f1914-137">Inject an <xref:Microsoft.Extensions.Configuration.IConfiguration> instance into a component to access the configuration data:</span></span>

```razor
@page "/"
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<h1>Configuration example</h1>

<p>Message: @Configuration["message"]</p>
```

#### <a name="provider-configuration"></a><span data-ttu-id="f1914-138">提供者設定</span><span class="sxs-lookup"><span data-stu-id="f1914-138">Provider configuration</span></span>

<span data-ttu-id="f1914-139">下列範例會使用來<xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationSource>提供額外的設定：</span><span class="sxs-lookup"><span data-stu-id="f1914-139">The following example uses a <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationSource> to supply additional configuration:</span></span>

<span data-ttu-id="f1914-140">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="f1914-140">`Program.Main`:</span></span>

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

<span data-ttu-id="f1914-141">將<xref:Microsoft.Extensions.Configuration.IConfiguration>實例插入元件以存取設定資料：</span><span class="sxs-lookup"><span data-stu-id="f1914-141">Inject an <xref:Microsoft.Extensions.Configuration.IConfiguration> instance into a component to access the configuration data:</span></span>

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

<span data-ttu-id="f1914-142">若要將*wwwroot*資料夾中的其他設定檔讀取到設定中`HttpClient` ，請使用來取得檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="f1914-142">To read other configuration files from the *wwwroot* folder into configuration, use an `HttpClient` to obtain the file's content.</span></span> <span data-ttu-id="f1914-143">使用此方法時，現有`HttpClient`的服務註冊可以使用已建立的本機用戶端來讀取檔案，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="f1914-143">When using this approach, the existing `HttpClient` service registration can use the local client created to read the file, as the following example shows:</span></span>

<span data-ttu-id="f1914-144">*wwwroot/cars*：</span><span class="sxs-lookup"><span data-stu-id="f1914-144">*wwwroot/cars.json*:</span></span>

```json
{
    "size": "tiny"
}
```

<span data-ttu-id="f1914-145">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="f1914-145">`Program.Main`:</span></span>

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

#### <a name="authentication-configuration"></a><span data-ttu-id="f1914-146">驗證設定</span><span class="sxs-lookup"><span data-stu-id="f1914-146">Authentication configuration</span></span>

<span data-ttu-id="f1914-147">*wwwroot/appsettings. json*：</span><span class="sxs-lookup"><span data-stu-id="f1914-147">*wwwroot/appsettings.json*:</span></span>

```json
{
  "AzureAD": {
    "Authority": "https://login.microsoftonline.com/",
    "ClientId": "aeaebf0f-d416-4d92-a08f-e1d5b51fc494"
  }
}
```

<span data-ttu-id="f1914-148">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="f1914-148">`Program.Main`:</span></span>

```csharp
builder.Services.AddOidcAuthentication(options =>
    builder.Configuration.Bind("AzureAD", options);
```

#### <a name="logging-configuration"></a><span data-ttu-id="f1914-149">記錄設定</span><span class="sxs-lookup"><span data-stu-id="f1914-149">Logging configuration</span></span>

<span data-ttu-id="f1914-150">*wwwroot/appsettings. json*：</span><span class="sxs-lookup"><span data-stu-id="f1914-150">*wwwroot/appsettings.json*:</span></span>

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

<span data-ttu-id="f1914-151">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="f1914-151">`Program.Main`:</span></span>

```csharp
builder.Logging.AddConfiguration(
    builder.Configuration.GetSection("Logging"));
```

#### <a name="host-builder-configuration"></a><span data-ttu-id="f1914-152">主機產生器設定</span><span class="sxs-lookup"><span data-stu-id="f1914-152">Host builder configuration</span></span>

<span data-ttu-id="f1914-153">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="f1914-153">`Program.Main`:</span></span>

```csharp
var hostname = builder.Configuration["HostName"];
```

#### <a name="cached-configuration"></a><span data-ttu-id="f1914-154">快取設定</span><span class="sxs-lookup"><span data-stu-id="f1914-154">Cached configuration</span></span>

<span data-ttu-id="f1914-155">系統會快取設定檔以供離線使用。</span><span class="sxs-lookup"><span data-stu-id="f1914-155">Configuration files are cached for offline use.</span></span> <span data-ttu-id="f1914-156">使用[漸進式 Web 應用程式（pwa）](xref:blazor/progressive-web-app)，您只能在建立新的部署時更新設定檔。</span><span class="sxs-lookup"><span data-stu-id="f1914-156">With [Progressive Web Applications (PWAs)](xref:blazor/progressive-web-app), you can only update configuration files when creating a new deployment.</span></span> <span data-ttu-id="f1914-157">在部署之間編輯設定檔沒有任何作用，因為：</span><span class="sxs-lookup"><span data-stu-id="f1914-157">Editing configuration files between deployments has no effect because:</span></span>

* <span data-ttu-id="f1914-158">使用者有檔案的快取版本，這些檔案會繼續使用。</span><span class="sxs-lookup"><span data-stu-id="f1914-158">Users have cached versions of the files that they continue to use.</span></span>
* <span data-ttu-id="f1914-159">PWA 的*service-worker*和*service-worker-assets*檔案必須在編譯時重建，在使用者下一次線上流覽應用程式時，該應用程式會發出通知。</span><span class="sxs-lookup"><span data-stu-id="f1914-159">The PWA's *service-worker.js* and *service-worker-assets.js* files must be rebuilt on compilation, which signal to the app on the user's next online visit that the app has been redeployed.</span></span>

<span data-ttu-id="f1914-160">如需 Pwa 如何處理背景更新的詳細資訊，請參閱<xref:blazor/progressive-web-app#background-updates>。</span><span class="sxs-lookup"><span data-stu-id="f1914-160">For more information on how background updates are handled by PWAs, see <xref:blazor/progressive-web-app#background-updates>.</span></span>

### <a name="logging"></a><span data-ttu-id="f1914-161">記錄</span><span class="sxs-lookup"><span data-stu-id="f1914-161">Logging</span></span>

<span data-ttu-id="f1914-162">如需 Blazor WebAssembly 記錄支援的詳細資訊<xref:fundamentals/logging/index#create-logs-in-blazor>，請參閱。</span><span class="sxs-lookup"><span data-stu-id="f1914-162">For information on Blazor WebAssembly logging support, see <xref:fundamentals/logging/index#create-logs-in-blazor>.</span></span>

## <a name="blazor-server"></a><span data-ttu-id="f1914-163">Blazor 伺服器</span><span class="sxs-lookup"><span data-stu-id="f1914-163">Blazor Server</span></span>

### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="f1914-164">反映 UI 中的連接狀態</span><span class="sxs-lookup"><span data-stu-id="f1914-164">Reflect the connection state in the UI</span></span>

<span data-ttu-id="f1914-165">當用戶端偵測到連線已遺失時，會在用戶端嘗試重新連線時，向使用者顯示預設的 UI。</span><span class="sxs-lookup"><span data-stu-id="f1914-165">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="f1914-166">如果重新連線失敗，則會提供使用者重試的選項。</span><span class="sxs-lookup"><span data-stu-id="f1914-166">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="f1914-167">若要自訂 UI，請在 *_Host. cshtml* Razor 頁面`<body>`的中，定義具有`id`之的`components-reconnect-modal`元素：</span><span class="sxs-lookup"><span data-stu-id="f1914-167">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="f1914-168">下表描述套用至`components-reconnect-modal`元素的 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="f1914-168">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="f1914-169">CSS 類別</span><span class="sxs-lookup"><span data-stu-id="f1914-169">CSS class</span></span>                       | <span data-ttu-id="f1914-170">表示&hellip;</span><span class="sxs-lookup"><span data-stu-id="f1914-170">Indicates&hellip;</span></span> |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | <span data-ttu-id="f1914-171">遺失的連接。</span><span class="sxs-lookup"><span data-stu-id="f1914-171">A lost connection.</span></span> <span data-ttu-id="f1914-172">用戶端正在嘗試重新連線。</span><span class="sxs-lookup"><span data-stu-id="f1914-172">The client is attempting to reconnect.</span></span> <span data-ttu-id="f1914-173">顯示強制回應。</span><span class="sxs-lookup"><span data-stu-id="f1914-173">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="f1914-174">系統會將使用中的連接重新建立至伺服器。</span><span class="sxs-lookup"><span data-stu-id="f1914-174">An active connection is re-established to the server.</span></span> <span data-ttu-id="f1914-175">隱藏強制回應。</span><span class="sxs-lookup"><span data-stu-id="f1914-175">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="f1914-176">重新連接失敗，可能是因為網路失敗。</span><span class="sxs-lookup"><span data-stu-id="f1914-176">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="f1914-177">若要嘗試重新連接`window.Blazor.reconnect()`，請呼叫。</span><span class="sxs-lookup"><span data-stu-id="f1914-177">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="f1914-178">已拒絕重新連接。</span><span class="sxs-lookup"><span data-stu-id="f1914-178">Reconnection rejected.</span></span> <span data-ttu-id="f1914-179">已達到伺服器但拒絕連線，而且伺服器上的使用者狀態遺失。</span><span class="sxs-lookup"><span data-stu-id="f1914-179">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="f1914-180">若要重載應用程式， `location.reload()`請呼叫。</span><span class="sxs-lookup"><span data-stu-id="f1914-180">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="f1914-181">當下列情況發生時，可能會產生此連接狀態：</span><span class="sxs-lookup"><span data-stu-id="f1914-181">This connection state may result when:</span></span><ul><li><span data-ttu-id="f1914-182">伺服器端線路發生損毀。</span><span class="sxs-lookup"><span data-stu-id="f1914-182">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="f1914-183">用戶端中斷連線的時間夠長，伺服器才會捨棄使用者的狀態。</span><span class="sxs-lookup"><span data-stu-id="f1914-183">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="f1914-184">系統會處置與使用者互動之元件的實例。</span><span class="sxs-lookup"><span data-stu-id="f1914-184">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="f1914-185">伺服器會重新開機，或回收應用程式的背景工作進程。</span><span class="sxs-lookup"><span data-stu-id="f1914-185">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="render-mode"></a><span data-ttu-id="f1914-186">轉譯模式</span><span class="sxs-lookup"><span data-stu-id="f1914-186">Render mode</span></span>

<span data-ttu-id="f1914-187">在建立伺服器的用戶端連接之前，預設會設定 Blazor 伺服器應用程式，以預先呈現伺服器上的 UI。</span><span class="sxs-lookup"><span data-stu-id="f1914-187">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="f1914-188">這會在 *_Host. cshtml* Razor 頁面中設定：</span><span class="sxs-lookup"><span data-stu-id="f1914-188">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="f1914-189">`RenderMode`設定元件是否：</span><span class="sxs-lookup"><span data-stu-id="f1914-189">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="f1914-190">會資源清單到頁面中。</span><span class="sxs-lookup"><span data-stu-id="f1914-190">Is prerendered into the page.</span></span>
* <span data-ttu-id="f1914-191">會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動 Blazor 應用程式所需的資訊。</span><span class="sxs-lookup"><span data-stu-id="f1914-191">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="f1914-192">說明</span><span class="sxs-lookup"><span data-stu-id="f1914-192">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="f1914-193">將元件轉譯為靜態 HTML，並包含Blazor伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="f1914-193">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="f1914-194">當使用者代理程式啟動時，會使用此標記來啟動Blazor應用程式。</span><span class="sxs-lookup"><span data-stu-id="f1914-194">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="f1914-195">呈現Blazor伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="f1914-195">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="f1914-196">不包含來自元件的輸出。</span><span class="sxs-lookup"><span data-stu-id="f1914-196">Output from the component isn't included.</span></span> <span data-ttu-id="f1914-197">當使用者代理程式啟動時，會使用此標記來啟動Blazor應用程式。</span><span class="sxs-lookup"><span data-stu-id="f1914-197">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="f1914-198">將元件轉譯為靜態 HTML。</span><span class="sxs-lookup"><span data-stu-id="f1914-198">Renders the component into static HTML.</span></span> |

<span data-ttu-id="f1914-199">不支援從靜態 HTML 網頁轉譯伺服器元件。</span><span class="sxs-lookup"><span data-stu-id="f1914-199">Rendering server components from a static HTML page isn't supported.</span></span>

### <a name="configure-the-signalr-client-for-blazor-server-apps"></a><span data-ttu-id="f1914-200">設定Blazor伺服器SignalR應用程式的用戶端</span><span class="sxs-lookup"><span data-stu-id="f1914-200">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="f1914-201">有時候，您需要設定SignalR Blazor伺服器應用程式所使用的用戶端。</span><span class="sxs-lookup"><span data-stu-id="f1914-201">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="f1914-202">例如，您可能會想要在SignalR用戶端上設定記錄來診斷連線問題。</span><span class="sxs-lookup"><span data-stu-id="f1914-202">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="f1914-203">若要在SignalR *Pages/_Host. cshtml*檔案中設定用戶端：</span><span class="sxs-lookup"><span data-stu-id="f1914-203">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="f1914-204">將`autostart="false"`屬性加入至`<script>` `blazor.server.js`腳本的標記。</span><span class="sxs-lookup"><span data-stu-id="f1914-204">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="f1914-205">呼叫`Blazor.start`並傳入指定SignalR產生器的設定物件。</span><span class="sxs-lookup"><span data-stu-id="f1914-205">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging("information"); // LogLevel.Information
    }
  });
</script>
```

### <a name="logging"></a><span data-ttu-id="f1914-206">記錄</span><span class="sxs-lookup"><span data-stu-id="f1914-206">Logging</span></span>

<span data-ttu-id="f1914-207">如需Blazor伺服器記錄支援的詳細資訊<xref:fundamentals/logging/index#create-logs-in-blazor>，請參閱。</span><span class="sxs-lookup"><span data-stu-id="f1914-207">For information on Blazor Server logging support, see <xref:fundamentals/logging/index#create-logs-in-blazor>.</span></span>
