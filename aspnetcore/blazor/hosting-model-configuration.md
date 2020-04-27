---
title: ASP.NET Core Blazor裝載模型設定
author: guardrex
description: 瞭解Blazor主控模型設定，包括如何將 Razor 元件整合到 RAZOR PAGES 和 MVC 應用程式中。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/25/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: c7e8d1f2dcba6432072a5cc11a6c5d78e50c2398
ms.sourcegitcommit: c6f5ea6397af2dd202632cf2be66fc30f3357bcc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/26/2020
ms.locfileid: "82159615"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="458ec-103">ASP.NET Core Blazor 裝載模型設定</span><span class="sxs-lookup"><span data-stu-id="458ec-103">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="458ec-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="458ec-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="458ec-105">本文涵蓋主控模型設定。</span><span class="sxs-lookup"><span data-stu-id="458ec-105">This article covers hosting model configuration.</span></span>

## <a name="blazor-webassembly"></a><span data-ttu-id="458ec-106">Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="458ec-106">Blazor WebAssembly</span></span>

### <a name="environment"></a><span data-ttu-id="458ec-107">環境</span><span class="sxs-lookup"><span data-stu-id="458ec-107">Environment</span></span>

<span data-ttu-id="458ec-108">在本機執行應用程式時，環境會預設為開發。</span><span class="sxs-lookup"><span data-stu-id="458ec-108">When running an app locally, the environment defaults to Development.</span></span> <span data-ttu-id="458ec-109">當應用程式發佈時，環境會預設為 [生產]。</span><span class="sxs-lookup"><span data-stu-id="458ec-109">When the app is published, the environment defaults to Production.</span></span>

<span data-ttu-id="458ec-110">Hosted Blazor WebAssembly 應用程式會透過中介軟體來從伺服器挑選環境，藉由新增`blazor-environment`標頭來將環境傳達給瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="458ec-110">A hosted Blazor WebAssembly app picks up the environment from the server via a middleware that communicates the environment to the browser by adding the `blazor-environment` header.</span></span> <span data-ttu-id="458ec-111">標頭的值為環境。</span><span class="sxs-lookup"><span data-stu-id="458ec-111">The value of the header is the environment.</span></span> <span data-ttu-id="458ec-112">託管的 Blazor 應用程式和伺服器應用程式會共用相同的環境。</span><span class="sxs-lookup"><span data-stu-id="458ec-112">The hosted Blazor app and the server app share the same environment.</span></span> <span data-ttu-id="458ec-113">如需詳細資訊，包括如何設定環境，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="458ec-113">For more information, including how to configure the environment, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="458ec-114">針對在本機執行的獨立應用程式，開發伺服器會`blazor-environment`新增標頭以指定開發環境。</span><span class="sxs-lookup"><span data-stu-id="458ec-114">For a standalone app running locally, the development server adds the `blazor-environment` header to specify the Development environment.</span></span> <span data-ttu-id="458ec-115">若要指定其他裝載環境的環境，請新增`blazor-environment`標頭。</span><span class="sxs-lookup"><span data-stu-id="458ec-115">To specify the environment for other hosting environments, add the `blazor-environment` header.</span></span>

<span data-ttu-id="458ec-116">在 IIS 的下列範例中，將自訂標頭加入至*已發佈的 web.config 檔案*。</span><span class="sxs-lookup"><span data-stu-id="458ec-116">In the following example for IIS, add the custom header to the published *web.config* file.</span></span> <span data-ttu-id="458ec-117">Web.config*檔案*位於*bin/RELEASE/{TARGET FRAMEWORK}/publish*資料夾中：</span><span class="sxs-lookup"><span data-stu-id="458ec-117">The *web.config* file is located in the *bin/Release/{TARGET FRAMEWORK}/publish* folder:</span></span>

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
> <span data-ttu-id="458ec-118">若要使用 IIS 的自訂*web.config*檔案，而該檔案不會在應用程式發行至*發行*資料夾時遭到<xref:host-and-deploy/blazor/webassembly#use-a-custom-webconfig>覆寫，請參閱。</span><span class="sxs-lookup"><span data-stu-id="458ec-118">To use a custom *web.config* file for IIS that isn't overwritten when the app is published to the *publish* folder, see <xref:host-and-deploy/blazor/webassembly#use-a-custom-webconfig>.</span></span>

<span data-ttu-id="458ec-119">藉由插入`IWebAssemblyHostEnvironment`和讀取`Environment`屬性，在元件中取得應用程式的環境：</span><span class="sxs-lookup"><span data-stu-id="458ec-119">Obtain the app's environment in a component by injecting `IWebAssemblyHostEnvironment` and reading the `Environment` property:</span></span>

```razor
@page "/"
@using Microsoft.AspNetCore.Components.WebAssembly.Hosting
@inject IWebAssemblyHostEnvironment HostEnvironment

<h1>Environment example</h1>

<p>Environment: @HostEnvironment.Environment</p>
```

<span data-ttu-id="458ec-120">在啟動期間， `WebAssemblyHostBuilder`會`IWebAssemblyHostEnvironment`透過`HostEnvironment`屬性公開，讓開發人員在其程式碼中具有環境特定邏輯：</span><span class="sxs-lookup"><span data-stu-id="458ec-120">During startup, the `WebAssemblyHostBuilder` exposes the `IWebAssemblyHostEnvironment` through the `HostEnvironment` property, which enables developers to have environment-specific logic in their code:</span></span>

```csharp
if (builder.HostEnvironment.Environment == "Custom")
{
    ...
};
```

<span data-ttu-id="458ec-121">下列便利的擴充方法可讓您檢查目前的環境，以進行開發、生產、預備和自訂環境名稱：</span><span class="sxs-lookup"><span data-stu-id="458ec-121">The following convenience extension methods permit checking the current environment for Development, Production, Staging, and custom environment names:</span></span>

* `IsDevelopment()`
* `IsProduction()`
* `IsStaging()`
* <span data-ttu-id="458ec-122">' IsEnvironment （"{環境名稱}"）</span><span class="sxs-lookup"><span data-stu-id="458ec-122">\`IsEnvironment("{ENVIRONMENT NAME}")</span></span>

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

<span data-ttu-id="458ec-123">當`IWebAssemblyHostEnvironment.BaseAddress` `NavigationManager`服務無法使用時，可以在啟動期間使用此屬性。</span><span class="sxs-lookup"><span data-stu-id="458ec-123">The `IWebAssemblyHostEnvironment.BaseAddress` property can be used during startup when the `NavigationManager` service isn't available.</span></span>

### <a name="configuration"></a><span data-ttu-id="458ec-124">組態</span><span class="sxs-lookup"><span data-stu-id="458ec-124">Configuration</span></span>

<span data-ttu-id="458ec-125">Blazor WebAssembly 支援下列設定：</span><span class="sxs-lookup"><span data-stu-id="458ec-125">Blazor WebAssembly supports configuration from:</span></span>

* <span data-ttu-id="458ec-126">應用程式佈建檔案的檔案設定[提供者](xref:fundamentals/configuration/index#file-configuration-provider)預設為：</span><span class="sxs-lookup"><span data-stu-id="458ec-126">The [File Configuration Provider](xref:fundamentals/configuration/index#file-configuration-provider) for app settings files by default:</span></span>
  * <span data-ttu-id="458ec-127">*wwwroot/appsettings. json*</span><span class="sxs-lookup"><span data-stu-id="458ec-127">*wwwroot/appsettings.json*</span></span>
  * <span data-ttu-id="458ec-128">*wwwroot/appsettings。{環境}. json*</span><span class="sxs-lookup"><span data-stu-id="458ec-128">*wwwroot/appsettings.{ENVIRONMENT}.json*</span></span>
* <span data-ttu-id="458ec-129">應用程式註冊的其他設定[提供者](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="458ec-129">Other [configuration providers](xref:fundamentals/configuration/index) registered by the app.</span></span>

> [!WARNING]
> <span data-ttu-id="458ec-130">使用者可以看到 Blazor WebAssembly 應用程式中的設定。</span><span class="sxs-lookup"><span data-stu-id="458ec-130">Configuration in a Blazor WebAssembly app is visible to users.</span></span> <span data-ttu-id="458ec-131">**請勿在設定中儲存應用程式秘密或認證。**</span><span class="sxs-lookup"><span data-stu-id="458ec-131">**Don't store app secrets or credentials in configuration.**</span></span>

<span data-ttu-id="458ec-132">如需設定提供者的詳細資訊<xref:fundamentals/configuration/index>，請參閱。</span><span class="sxs-lookup"><span data-stu-id="458ec-132">For more information on configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

#### <a name="app-settings-configuration"></a><span data-ttu-id="458ec-133">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="458ec-133">App settings configuration</span></span>

<span data-ttu-id="458ec-134">*wwwroot/appsettings. json*：</span><span class="sxs-lookup"><span data-stu-id="458ec-134">*wwwroot/appsettings.json*:</span></span>

```json
{
  "message": "Hello from config!"
}
```

<span data-ttu-id="458ec-135">將<xref:Microsoft.Extensions.Configuration.IConfiguration>實例插入元件以存取設定資料：</span><span class="sxs-lookup"><span data-stu-id="458ec-135">Inject an <xref:Microsoft.Extensions.Configuration.IConfiguration> instance into a component to access the configuration data:</span></span>

```razor
@page "/"
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<h1>Configuration example</h1>

<p>Message: @Configuration["message"]</p>
```

#### <a name="provider-configuration"></a><span data-ttu-id="458ec-136">提供者設定</span><span class="sxs-lookup"><span data-stu-id="458ec-136">Provider configuration</span></span>

<span data-ttu-id="458ec-137">下列範例會使用<xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationSource>和檔案設定[提供者](xref:fundamentals/configuration/index#file-configuration-provider)來提供額外的設定：</span><span class="sxs-lookup"><span data-stu-id="458ec-137">The following example uses a <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationSource> and the [File Configuration Provider](xref:fundamentals/configuration/index#file-configuration-provider) to supply additional configuration:</span></span>

<span data-ttu-id="458ec-138">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="458ec-138">`Program.Main`:</span></span>

```csharp
using Microsoft.Extensions.Configuration;

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

builder.Configuration
    .Add(memoryConfig)
    .AddJsonFile("cars.json", optional: false, reloadOnChange: true);
```

<span data-ttu-id="458ec-139">將<xref:Microsoft.Extensions.Configuration.IConfiguration>實例插入元件以存取設定資料：</span><span class="sxs-lookup"><span data-stu-id="458ec-139">Inject an <xref:Microsoft.Extensions.Configuration.IConfiguration> instance into a component to access the configuration data:</span></span>

```razor
@page "/"
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<h1>Configuration example</h1>

<h2>Wheels</h2>

<ul>
    <li>Count: @Configuration["wheels:count"]</p>
    <li>Brand: @Configuration["wheels:brand"]</p>
    <li>Type: @Configuration["wheels:brand:type"]</p>
    <li>Year: @Configuration["wheels:year"]</p>
</ul>

@code {
    var wheelsSection = Configuration.GetSection("wheels");
    
    ...
}
```

#### <a name="authentication-configuration"></a><span data-ttu-id="458ec-140">驗證設定</span><span class="sxs-lookup"><span data-stu-id="458ec-140">Authentication configuration</span></span>

<span data-ttu-id="458ec-141">*wwwroot/appsettings. json*：</span><span class="sxs-lookup"><span data-stu-id="458ec-141">*wwwroot/appsettings.json*:</span></span>

```json
{
  "AzureAD": {
    "Authority": "https://login.microsoftonline.com/",
    "ClientId": "aeaebf0f-d416-4d92-a08f-e1d5b51fc494"
  }
}
```

<span data-ttu-id="458ec-142">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="458ec-142">`Program.Main`:</span></span>

```csharp
builder.Services.AddOidcAuthentication(options =>
    builder.Configuration.Bind("AzureAD", options);
```

#### <a name="logging-configuration"></a><span data-ttu-id="458ec-143">記錄設定</span><span class="sxs-lookup"><span data-stu-id="458ec-143">Logging configuration</span></span>

<span data-ttu-id="458ec-144">*wwwroot/appsettings. json*：</span><span class="sxs-lookup"><span data-stu-id="458ec-144">*wwwroot/appsettings.json*:</span></span>

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

<span data-ttu-id="458ec-145">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="458ec-145">`Program.Main`:</span></span>

```csharp
builder.Logging.AddConfiguration(
    builder.Configuration.GetSection("Logging"));
```

#### <a name="host-builder-configuration"></a><span data-ttu-id="458ec-146">主機產生器設定</span><span class="sxs-lookup"><span data-stu-id="458ec-146">Host builder configuration</span></span>

<span data-ttu-id="458ec-147">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="458ec-147">`Program.Main`:</span></span>

```csharp
var hostname = builder.Configuration["HostName"];
```

#### <a name="cached-configuration"></a><span data-ttu-id="458ec-148">快取設定</span><span class="sxs-lookup"><span data-stu-id="458ec-148">Cached configuration</span></span>

<span data-ttu-id="458ec-149">系統會快取設定檔以供離線使用。</span><span class="sxs-lookup"><span data-stu-id="458ec-149">Configuration files are cached for offline use.</span></span> <span data-ttu-id="458ec-150">使用[漸進式 Web 應用程式（pwa）](xref:blazor/progressive-web-app)，您只能在建立新的部署時更新設定檔。</span><span class="sxs-lookup"><span data-stu-id="458ec-150">With [Progressive Web Applications (PWAs)](xref:blazor/progressive-web-app), you can only update configuration files when creating a new deployment.</span></span> <span data-ttu-id="458ec-151">在部署之間編輯設定檔沒有任何作用，因為：</span><span class="sxs-lookup"><span data-stu-id="458ec-151">Editing configuration files between deployments has no effect because:</span></span>

* <span data-ttu-id="458ec-152">使用者有檔案的快取版本，這些檔案會繼續使用。</span><span class="sxs-lookup"><span data-stu-id="458ec-152">Users have cached versions of the files that they continue to use.</span></span>
* <span data-ttu-id="458ec-153">PWA 的*service-worker*和*service-worker-assets*檔案必須在編譯時重建，在使用者下一次線上流覽應用程式時，該應用程式會發出通知。</span><span class="sxs-lookup"><span data-stu-id="458ec-153">The PWA's *service-worker.js* and *service-worker-assets.js* files must be rebuilt on compilation, which signal to the app on the user's next online visit that the app has been redeployed.</span></span>

<span data-ttu-id="458ec-154">如需 Pwa 如何處理背景更新的詳細資訊，請參閱<xref:blazor/progressive-web-app#background-updates>。</span><span class="sxs-lookup"><span data-stu-id="458ec-154">For more information on how background updates are handled by PWAs, see <xref:blazor/progressive-web-app#background-updates>.</span></span>

### <a name="logging"></a><span data-ttu-id="458ec-155">記錄</span><span class="sxs-lookup"><span data-stu-id="458ec-155">Logging</span></span>

<span data-ttu-id="458ec-156">如需 Blazor WebAssembly 記錄支援的詳細資訊<xref:fundamentals/logging/index#create-logs-in-blazor>，請參閱。</span><span class="sxs-lookup"><span data-stu-id="458ec-156">For information on Blazor WebAssembly logging support, see <xref:fundamentals/logging/index#create-logs-in-blazor>.</span></span>

## <a name="blazor-server"></a><span data-ttu-id="458ec-157">Blazor 伺服器</span><span class="sxs-lookup"><span data-stu-id="458ec-157">Blazor Server</span></span>

### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="458ec-158">反映 UI 中的連接狀態</span><span class="sxs-lookup"><span data-stu-id="458ec-158">Reflect the connection state in the UI</span></span>

<span data-ttu-id="458ec-159">當用戶端偵測到連線已遺失時，會在用戶端嘗試重新連線時，向使用者顯示預設的 UI。</span><span class="sxs-lookup"><span data-stu-id="458ec-159">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="458ec-160">如果重新連線失敗，則會提供使用者重試的選項。</span><span class="sxs-lookup"><span data-stu-id="458ec-160">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="458ec-161">若要自訂 UI，請在 *_Host. cshtml* Razor 頁面`<body>`的中，定義具有`id`之的`components-reconnect-modal`元素：</span><span class="sxs-lookup"><span data-stu-id="458ec-161">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="458ec-162">下表描述套用至`components-reconnect-modal`元素的 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="458ec-162">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="458ec-163">CSS 類別</span><span class="sxs-lookup"><span data-stu-id="458ec-163">CSS class</span></span>                       | <span data-ttu-id="458ec-164">表示&hellip;</span><span class="sxs-lookup"><span data-stu-id="458ec-164">Indicates&hellip;</span></span> |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | <span data-ttu-id="458ec-165">遺失的連接。</span><span class="sxs-lookup"><span data-stu-id="458ec-165">A lost connection.</span></span> <span data-ttu-id="458ec-166">用戶端正在嘗試重新連線。</span><span class="sxs-lookup"><span data-stu-id="458ec-166">The client is attempting to reconnect.</span></span> <span data-ttu-id="458ec-167">顯示強制回應。</span><span class="sxs-lookup"><span data-stu-id="458ec-167">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="458ec-168">系統會將使用中的連接重新建立至伺服器。</span><span class="sxs-lookup"><span data-stu-id="458ec-168">An active connection is re-established to the server.</span></span> <span data-ttu-id="458ec-169">隱藏強制回應。</span><span class="sxs-lookup"><span data-stu-id="458ec-169">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="458ec-170">重新連接失敗，可能是因為網路失敗。</span><span class="sxs-lookup"><span data-stu-id="458ec-170">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="458ec-171">若要嘗試重新連接`window.Blazor.reconnect()`，請呼叫。</span><span class="sxs-lookup"><span data-stu-id="458ec-171">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="458ec-172">已拒絕重新連接。</span><span class="sxs-lookup"><span data-stu-id="458ec-172">Reconnection rejected.</span></span> <span data-ttu-id="458ec-173">已達到伺服器但拒絕連線，而且伺服器上的使用者狀態遺失。</span><span class="sxs-lookup"><span data-stu-id="458ec-173">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="458ec-174">若要重載應用程式， `location.reload()`請呼叫。</span><span class="sxs-lookup"><span data-stu-id="458ec-174">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="458ec-175">當下列情況發生時，可能會產生此連接狀態：</span><span class="sxs-lookup"><span data-stu-id="458ec-175">This connection state may result when:</span></span><ul><li><span data-ttu-id="458ec-176">伺服器端線路發生損毀。</span><span class="sxs-lookup"><span data-stu-id="458ec-176">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="458ec-177">用戶端中斷連線的時間夠長，伺服器才會捨棄使用者的狀態。</span><span class="sxs-lookup"><span data-stu-id="458ec-177">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="458ec-178">系統會處置與使用者互動之元件的實例。</span><span class="sxs-lookup"><span data-stu-id="458ec-178">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="458ec-179">伺服器會重新開機，或回收應用程式的背景工作進程。</span><span class="sxs-lookup"><span data-stu-id="458ec-179">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="render-mode"></a><span data-ttu-id="458ec-180">轉譯模式</span><span class="sxs-lookup"><span data-stu-id="458ec-180">Render mode</span></span>

<span data-ttu-id="458ec-181">在建立伺服器的用戶端連接之前，預設會設定 Blazor 伺服器應用程式，以預先呈現伺服器上的 UI。</span><span class="sxs-lookup"><span data-stu-id="458ec-181">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="458ec-182">這會在 *_Host. cshtml* Razor 頁面中設定：</span><span class="sxs-lookup"><span data-stu-id="458ec-182">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="458ec-183">`RenderMode`設定元件是否：</span><span class="sxs-lookup"><span data-stu-id="458ec-183">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="458ec-184">會資源清單到頁面中。</span><span class="sxs-lookup"><span data-stu-id="458ec-184">Is prerendered into the page.</span></span>
* <span data-ttu-id="458ec-185">會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動 Blazor 應用程式所需的資訊。</span><span class="sxs-lookup"><span data-stu-id="458ec-185">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="458ec-186">描述</span><span class="sxs-lookup"><span data-stu-id="458ec-186">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="458ec-187">將元件轉譯為靜態 HTML，並包含Blazor伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="458ec-187">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="458ec-188">當使用者代理程式啟動時，會使用此標記來啟動Blazor應用程式。</span><span class="sxs-lookup"><span data-stu-id="458ec-188">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="458ec-189">呈現Blazor伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="458ec-189">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="458ec-190">不包含來自元件的輸出。</span><span class="sxs-lookup"><span data-stu-id="458ec-190">Output from the component isn't included.</span></span> <span data-ttu-id="458ec-191">當使用者代理程式啟動時，會使用此標記來啟動Blazor應用程式。</span><span class="sxs-lookup"><span data-stu-id="458ec-191">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="458ec-192">將元件轉譯為靜態 HTML。</span><span class="sxs-lookup"><span data-stu-id="458ec-192">Renders the component into static HTML.</span></span> |

<span data-ttu-id="458ec-193">不支援從靜態 HTML 網頁轉譯伺服器元件。</span><span class="sxs-lookup"><span data-stu-id="458ec-193">Rendering server components from a static HTML page isn't supported.</span></span>

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="458ec-194">設定Blazor伺服器SignalR應用程式的用戶端</span><span class="sxs-lookup"><span data-stu-id="458ec-194">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="458ec-195">有時候，您需要設定SignalR Blazor伺服器應用程式所使用的用戶端。</span><span class="sxs-lookup"><span data-stu-id="458ec-195">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="458ec-196">例如，您可能會想要在SignalR用戶端上設定記錄來診斷連線問題。</span><span class="sxs-lookup"><span data-stu-id="458ec-196">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="458ec-197">若要在SignalR *Pages/_Host. cshtml*檔案中設定用戶端：</span><span class="sxs-lookup"><span data-stu-id="458ec-197">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="458ec-198">將`autostart="false"`屬性加入至`<script>` `blazor.server.js`腳本的標記。</span><span class="sxs-lookup"><span data-stu-id="458ec-198">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="458ec-199">呼叫`Blazor.start`並傳入指定SignalR產生器的設定物件。</span><span class="sxs-lookup"><span data-stu-id="458ec-199">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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

### <a name="logging"></a><span data-ttu-id="458ec-200">記錄</span><span class="sxs-lookup"><span data-stu-id="458ec-200">Logging</span></span>

<span data-ttu-id="458ec-201">如需Blazor伺服器記錄支援的詳細資訊<xref:fundamentals/logging/index#create-logs-in-blazor>，請參閱。</span><span class="sxs-lookup"><span data-stu-id="458ec-201">For information on Blazor Server logging support, see <xref:fundamentals/logging/index#create-logs-in-blazor>.</span></span>
