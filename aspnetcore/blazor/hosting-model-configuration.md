---
title: ASP.NET Core Blazor 裝載模型設定
author: guardrex
description: 瞭解 Blazor 主控模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
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
uid: blazor/hosting-model-configuration
ms.openlocfilehash: 3cef67806ce0e2e045122bdc962e93795be68572
ms.sourcegitcommit: 6371114344a5f4fbc5d4a119b0be1ad3762e0216
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/10/2020
ms.locfileid: "84679575"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="fae88-103">ASP.NET Core Blazor 裝載模型設定</span><span class="sxs-lookup"><span data-stu-id="fae88-103">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="fae88-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fae88-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="fae88-105">本文涵蓋主控模型設定。</span><span class="sxs-lookup"><span data-stu-id="fae88-105">This article covers hosting model configuration.</span></span>

## <a name="blazor-webassembly"></a>Blazor<span data-ttu-id="fae88-106">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="fae88-106"> WebAssembly</span></span>

### <a name="environment"></a><span data-ttu-id="fae88-107">環境</span><span class="sxs-lookup"><span data-stu-id="fae88-107">Environment</span></span>

<span data-ttu-id="fae88-108">在本機執行應用程式時，環境會預設為開發。</span><span class="sxs-lookup"><span data-stu-id="fae88-108">When running an app locally, the environment defaults to Development.</span></span> <span data-ttu-id="fae88-109">當應用程式發佈時，環境會預設為 [生產]。</span><span class="sxs-lookup"><span data-stu-id="fae88-109">When the app is published, the environment defaults to Production.</span></span>

<span data-ttu-id="fae88-110">裝載的 Blazor WebAssembly 應用程式會透過中介軟體來從伺服器挑選環境，藉由新增標頭來將環境傳達給瀏覽器 `blazor-environment` 。</span><span class="sxs-lookup"><span data-stu-id="fae88-110">A hosted Blazor WebAssembly app picks up the environment from the server via a middleware that communicates the environment to the browser by adding the `blazor-environment` header.</span></span> <span data-ttu-id="fae88-111">標頭的值為環境。</span><span class="sxs-lookup"><span data-stu-id="fae88-111">The value of the header is the environment.</span></span> <span data-ttu-id="fae88-112">裝載的 Blazor 應用程式和伺服器應用程式會共用相同的環境。</span><span class="sxs-lookup"><span data-stu-id="fae88-112">The hosted Blazor app and the server app share the same environment.</span></span> <span data-ttu-id="fae88-113">如需詳細資訊，包括如何設定環境，請參閱 <xref:fundamentals/environments> 。</span><span class="sxs-lookup"><span data-stu-id="fae88-113">For more information, including how to configure the environment, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="fae88-114">針對在本機執行的獨立應用程式，開發伺服器會新增 `blazor-environment` 標頭以指定開發環境。</span><span class="sxs-lookup"><span data-stu-id="fae88-114">For a standalone app running locally, the development server adds the `blazor-environment` header to specify the Development environment.</span></span> <span data-ttu-id="fae88-115">若要指定其他裝載環境的環境，請新增 `blazor-environment` 標頭。</span><span class="sxs-lookup"><span data-stu-id="fae88-115">To specify the environment for other hosting environments, add the `blazor-environment` header.</span></span>

<span data-ttu-id="fae88-116">在 IIS 的下列範例中，將自訂標頭加入至已發佈的*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="fae88-116">In the following example for IIS, add the custom header to the published *web.config* file.</span></span> <span data-ttu-id="fae88-117">*web.config*檔案位於*bin/RELEASE/{TARGET FRAMEWORK}/publish*資料夾中：</span><span class="sxs-lookup"><span data-stu-id="fae88-117">The *web.config* file is located in the *bin/Release/{TARGET FRAMEWORK}/publish* folder:</span></span>

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
> <span data-ttu-id="fae88-118">若要使用 IIS 的自訂*web.config*檔案，而該檔案不會在應用程式發佈至*發行*資料夾時遭到覆寫，請參閱 <xref:host-and-deploy/blazor/webassembly#use-a-custom-webconfig> 。</span><span class="sxs-lookup"><span data-stu-id="fae88-118">To use a custom *web.config* file for IIS that isn't overwritten when the app is published to the *publish* folder, see <xref:host-and-deploy/blazor/webassembly#use-a-custom-webconfig>.</span></span>

<span data-ttu-id="fae88-119">藉由插入和讀取屬性，在元件中取得應用程式的環境 <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.IWebAssemblyHostEnvironment> <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.IWebAssemblyHostEnvironment.Environment> ：</span><span class="sxs-lookup"><span data-stu-id="fae88-119">Obtain the app's environment in a component by injecting <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.IWebAssemblyHostEnvironment> and reading the <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.IWebAssemblyHostEnvironment.Environment> property:</span></span>

```razor
@page "/"
@using Microsoft.AspNetCore.Components.WebAssembly.Hosting
@inject IWebAssemblyHostEnvironment HostEnvironment

<h1>Environment example</h1>

<p>Environment: @HostEnvironment.Environment</p>
```

<span data-ttu-id="fae88-120">在啟動期間， <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.WebAssemblyHostBuilder> <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.IWebAssemblyHostEnvironment> 會透過屬性公開 <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.WebAssemblyHostBuilder.HostEnvironment> ，讓開發人員在其程式碼中具有環境特定邏輯：</span><span class="sxs-lookup"><span data-stu-id="fae88-120">During startup, the <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.WebAssemblyHostBuilder> exposes the <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.IWebAssemblyHostEnvironment> through the <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.WebAssemblyHostBuilder.HostEnvironment> property, which enables developers to have environment-specific logic in their code:</span></span>

```csharp
if (builder.HostEnvironment.Environment == "Custom")
{
    ...
};
```

<span data-ttu-id="fae88-121">下列便利的擴充方法可讓您檢查目前的環境，以進行開發、生產、預備和自訂環境名稱：</span><span class="sxs-lookup"><span data-stu-id="fae88-121">The following convenience extension methods permit checking the current environment for Development, Production, Staging, and custom environment names:</span></span>

* `IsDevelopment()`
* `IsProduction()`
* `IsStaging()`
* `IsEnvironment("{ENVIRONMENT NAME}")`

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

<span data-ttu-id="fae88-122"><xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.IWebAssemblyHostEnvironment.BaseAddress?displayProperty=nameWithType>當服務無法使用時，可以在啟動期間使用此屬性 <xref:Microsoft.AspNetCore.Components.NavigationManager> 。</span><span class="sxs-lookup"><span data-stu-id="fae88-122">The <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.IWebAssemblyHostEnvironment.BaseAddress?displayProperty=nameWithType> property can be used during startup when the <xref:Microsoft.AspNetCore.Components.NavigationManager> service isn't available.</span></span>

### <a name="configuration"></a><span data-ttu-id="fae88-123">組態</span><span class="sxs-lookup"><span data-stu-id="fae88-123">Configuration</span></span>

Blazor<span data-ttu-id="fae88-124">WebAssembly 會從載入設定：</span><span class="sxs-lookup"><span data-stu-id="fae88-124"> WebAssembly loads configuration from:</span></span>

* <span data-ttu-id="fae88-125">應用程式佈建檔案（預設為）：</span><span class="sxs-lookup"><span data-stu-id="fae88-125">App settings files by default:</span></span>
  * <span data-ttu-id="fae88-126">*wwwroot/appsettings.js開啟*</span><span class="sxs-lookup"><span data-stu-id="fae88-126">*wwwroot/appsettings.json*</span></span>
  * <span data-ttu-id="fae88-127">*wwwroot/appsettings。{環境}. json*</span><span class="sxs-lookup"><span data-stu-id="fae88-127">*wwwroot/appsettings.{ENVIRONMENT}.json*</span></span>
* <span data-ttu-id="fae88-128">應用程式註冊的其他設定[提供者](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="fae88-128">Other [configuration providers](xref:fundamentals/configuration/index) registered by the app.</span></span> <span data-ttu-id="fae88-129">並非所有提供者都適用于 Blazor WebAssembly apps。</span><span class="sxs-lookup"><span data-stu-id="fae88-129">Not all providers are appropriate for Blazor WebAssembly apps.</span></span> <span data-ttu-id="fae88-130">澄清 Blazor [ Blazor WASM （dotnet/AspNetCore.Doc#18134）的設定提供者](https://github.com/dotnet/AspNetCore.Docs/issues/18134)，即可追蹤支援 WebAssembly 的提供者。</span><span class="sxs-lookup"><span data-stu-id="fae88-130">Clarification on which providers are supported for Blazor WebAssembly is tracked by [Clarify configuration providers for Blazor WASM (dotnet/AspNetCore.Docs #18134)](https://github.com/dotnet/AspNetCore.Docs/issues/18134).</span></span>

> [!WARNING]
> <span data-ttu-id="fae88-131">Blazor使用者可以看到 WebAssembly 應用程式中的設定。</span><span class="sxs-lookup"><span data-stu-id="fae88-131">Configuration in a Blazor WebAssembly app is visible to users.</span></span> <span data-ttu-id="fae88-132">**請勿在設定中儲存應用程式秘密或認證。**</span><span class="sxs-lookup"><span data-stu-id="fae88-132">**Don't store app secrets or credentials in configuration.**</span></span>

<span data-ttu-id="fae88-133">如需設定提供者的詳細資訊，請參閱 <xref:fundamentals/configuration/index> 。</span><span class="sxs-lookup"><span data-stu-id="fae88-133">For more information on configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

#### <a name="app-settings-configuration"></a><span data-ttu-id="fae88-134">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="fae88-134">App settings configuration</span></span>

<span data-ttu-id="fae88-135">*wwwroot/appsettings.js開啟*：</span><span class="sxs-lookup"><span data-stu-id="fae88-135">*wwwroot/appsettings.json*:</span></span>

```json
{
  "message": "Hello from config!"
}
```

<span data-ttu-id="fae88-136">將 <xref:Microsoft.Extensions.Configuration.IConfiguration> 實例插入元件以存取設定資料：</span><span class="sxs-lookup"><span data-stu-id="fae88-136">Inject an <xref:Microsoft.Extensions.Configuration.IConfiguration> instance into a component to access the configuration data:</span></span>

```razor
@page "/"
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<h1>Configuration example</h1>

<p>Message: @Configuration["message"]</p>
```

#### <a name="provider-configuration"></a><span data-ttu-id="fae88-137">提供者設定</span><span class="sxs-lookup"><span data-stu-id="fae88-137">Provider configuration</span></span>

<span data-ttu-id="fae88-138">下列範例會使用 <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationSource> 來提供額外的設定：</span><span class="sxs-lookup"><span data-stu-id="fae88-138">The following example uses a <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationSource> to supply additional configuration:</span></span>

<span data-ttu-id="fae88-139">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="fae88-139">`Program.Main`:</span></span>

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

<span data-ttu-id="fae88-140">將 <xref:Microsoft.Extensions.Configuration.IConfiguration> 實例插入元件以存取設定資料：</span><span class="sxs-lookup"><span data-stu-id="fae88-140">Inject an <xref:Microsoft.Extensions.Configuration.IConfiguration> instance into a component to access the configuration data:</span></span>

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

<span data-ttu-id="fae88-141">若要將*wwwroot*資料夾中的其他設定檔讀取到設定中，請使用 <xref:System.Net.Http.HttpClient> 來取得檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="fae88-141">To read other configuration files from the *wwwroot* folder into configuration, use an <xref:System.Net.Http.HttpClient> to obtain the file's content.</span></span> <span data-ttu-id="fae88-142">使用此方法時，現有的 <xref:System.Net.Http.HttpClient> 服務註冊可以使用已建立的本機用戶端來讀取檔案，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="fae88-142">When using this approach, the existing <xref:System.Net.Http.HttpClient> service registration can use the local client created to read the file, as the following example shows:</span></span>

<span data-ttu-id="fae88-143">*wwwroot/cars.js開啟*：</span><span class="sxs-lookup"><span data-stu-id="fae88-143">*wwwroot/cars.json*:</span></span>

```json
{
    "size": "tiny"
}
```

<span data-ttu-id="fae88-144">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="fae88-144">`Program.Main`:</span></span>

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

#### <a name="authentication-configuration"></a><span data-ttu-id="fae88-145">驗證設定</span><span class="sxs-lookup"><span data-stu-id="fae88-145">Authentication configuration</span></span>

<span data-ttu-id="fae88-146">*wwwroot/appsettings.js開啟*：</span><span class="sxs-lookup"><span data-stu-id="fae88-146">*wwwroot/appsettings.json*:</span></span>

```json
{
  "Local": {
    "Authority": "{AUTHORITY}",
    "ClientId": "{CLIENT ID}"
  }
}
```

<span data-ttu-id="fae88-147">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="fae88-147">`Program.Main`:</span></span>

```csharp
builder.Services.AddOidcAuthentication(options =>
    builder.Configuration.Bind("Local", options.ProviderOptions));
```

#### <a name="logging-configuration"></a><span data-ttu-id="fae88-148">記錄設定</span><span class="sxs-lookup"><span data-stu-id="fae88-148">Logging configuration</span></span>

<span data-ttu-id="fae88-149">新增[Microsoft.Extensions.Logging.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Configuration/)的套件參考：</span><span class="sxs-lookup"><span data-stu-id="fae88-149">Add a package reference for [Microsoft.Extensions.Logging.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Configuration/):</span></span>

```xml
<PackageReference Include="Microsoft.Extensions.Logging.Configuration" Version="{VERSION}" />
```

<span data-ttu-id="fae88-150">*wwwroot/appsettings.js開啟*：</span><span class="sxs-lookup"><span data-stu-id="fae88-150">*wwwroot/appsettings.json*:</span></span>

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

<span data-ttu-id="fae88-151">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="fae88-151">`Program.Main`:</span></span>

```csharp
using Microsoft.Extensions.Logging;

...

builder.Logging.AddConfiguration(
    builder.Configuration.GetSection("Logging"));
```

#### <a name="host-builder-configuration"></a><span data-ttu-id="fae88-152">主機產生器設定</span><span class="sxs-lookup"><span data-stu-id="fae88-152">Host builder configuration</span></span>

<span data-ttu-id="fae88-153">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="fae88-153">`Program.Main`:</span></span>

```csharp
var hostname = builder.Configuration["HostName"];
```

#### <a name="cached-configuration"></a><span data-ttu-id="fae88-154">快取設定</span><span class="sxs-lookup"><span data-stu-id="fae88-154">Cached configuration</span></span>

<span data-ttu-id="fae88-155">系統會快取設定檔以供離線使用。</span><span class="sxs-lookup"><span data-stu-id="fae88-155">Configuration files are cached for offline use.</span></span> <span data-ttu-id="fae88-156">使用[漸進式 Web 應用程式（pwa）](xref:blazor/progressive-web-app)，您只能在建立新的部署時更新設定檔。</span><span class="sxs-lookup"><span data-stu-id="fae88-156">With [Progressive Web Applications (PWAs)](xref:blazor/progressive-web-app), you can only update configuration files when creating a new deployment.</span></span> <span data-ttu-id="fae88-157">在部署之間編輯設定檔沒有任何作用，因為：</span><span class="sxs-lookup"><span data-stu-id="fae88-157">Editing configuration files between deployments has no effect because:</span></span>

* <span data-ttu-id="fae88-158">使用者有檔案的快取版本，這些檔案會繼續使用。</span><span class="sxs-lookup"><span data-stu-id="fae88-158">Users have cached versions of the files that they continue to use.</span></span>
* <span data-ttu-id="fae88-159">PWA 的*service-worker.js*和*service-worker-assets.js*檔案必須在編譯時重建，這會在使用者下一次線上流覽應用程式，以重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="fae88-159">The PWA's *service-worker.js* and *service-worker-assets.js* files must be rebuilt on compilation, which signal to the app on the user's next online visit that the app has been redeployed.</span></span>

<span data-ttu-id="fae88-160">如需 Pwa 如何處理背景更新的詳細資訊，請參閱 <xref:blazor/progressive-web-app#background-updates> 。</span><span class="sxs-lookup"><span data-stu-id="fae88-160">For more information on how background updates are handled by PWAs, see <xref:blazor/progressive-web-app#background-updates>.</span></span>

### <a name="logging"></a><span data-ttu-id="fae88-161">記錄</span><span class="sxs-lookup"><span data-stu-id="fae88-161">Logging</span></span>

<span data-ttu-id="fae88-162">如需 Blazor WebAssembly 記錄支援的詳細資訊，請參閱 <xref:fundamentals/logging/index#create-logs-in-blazor> 。</span><span class="sxs-lookup"><span data-stu-id="fae88-162">For information on Blazor WebAssembly logging support, see <xref:fundamentals/logging/index#create-logs-in-blazor>.</span></span>

### <a name="signalr-cross-origin-negotiation-for-authentication"></a>SignalR<span data-ttu-id="fae88-163">用於驗證的跨原始來源協調</span><span class="sxs-lookup"><span data-stu-id="fae88-163"> cross-origin negotiation for authentication</span></span>

<span data-ttu-id="fae88-164">若要設定 SignalR 的基礎用戶端來傳送認證，例如 cookie 或 HTTP 驗證標頭：</span><span class="sxs-lookup"><span data-stu-id="fae88-164">To configure SignalR's underlying client to send credentials, such as cookies or HTTP authentication headers:</span></span>

* <span data-ttu-id="fae88-165">用 <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.WebAssemblyHttpRequestMessageExtensions.SetBrowserRequestCredentials%2A> 來設定 <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.BrowserRequestCredentials.Include> 跨原始來源[提取](https://developer.mozilla.org/docs/Web/API/Fetch_API/Using_Fetch)要求：</span><span class="sxs-lookup"><span data-stu-id="fae88-165">Use <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.WebAssemblyHttpRequestMessageExtensions.SetBrowserRequestCredentials%2A> to set <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.BrowserRequestCredentials.Include> on cross-origin [fetch](https://developer.mozilla.org/docs/Web/API/Fetch_API/Using_Fetch) requests:</span></span>

  ```csharp
  public class IncludeRequestCredentialsMessagHandler : DelegatingHandler
  {
      protected override Task<HttpResponseMessage> SendAsync(
          HttpRequestMessage request, CancellationToken cancellationToken)
      {
          request.SetBrowserRequestCredentials(BrowserRequestCredentials.Include);
          return base.SendAsync(request, cancellationToken);
      }
  }
  ```

* <span data-ttu-id="fae88-166">將指派給 <xref:System.Net.Http.HttpMessageHandler> <xref:Microsoft.AspNetCore.Http.Connections.Client.HttpConnectionOptions.HttpMessageHandlerFactory> 選項：</span><span class="sxs-lookup"><span data-stu-id="fae88-166">Assign the <xref:System.Net.Http.HttpMessageHandler> to the <xref:Microsoft.AspNetCore.Http.Connections.Client.HttpConnectionOptions.HttpMessageHandlerFactory> option:</span></span>

  ```csharp
  var client = new HubConnectionBuilder()
      .WithUrl(new Uri("http://signalr.example.com"), options =>
      {
          options.HttpMessageHandlerFactory = innerHandler => 
              new IncludeRequestCredentialsMessagHandler { InnerHandler = innerHandler };
      }).Build();
  ```

<span data-ttu-id="fae88-167">如需詳細資訊，請參閱 <xref:signalr/configuration#configure-additional-options> 。</span><span class="sxs-lookup"><span data-stu-id="fae88-167">For more information, see <xref:signalr/configuration#configure-additional-options>.</span></span>

## <a name="blazor-server"></a>Blazor<span data-ttu-id="fae88-168">伺服器</span><span class="sxs-lookup"><span data-stu-id="fae88-168"> Server</span></span>

### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="fae88-169">反映 UI 中的連接狀態</span><span class="sxs-lookup"><span data-stu-id="fae88-169">Reflect the connection state in the UI</span></span>

<span data-ttu-id="fae88-170">當用戶端偵測到連線已遺失時，會在用戶端嘗試重新連線時，向使用者顯示預設的 UI。</span><span class="sxs-lookup"><span data-stu-id="fae88-170">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="fae88-171">如果重新連線失敗，則會提供使用者重試的選項。</span><span class="sxs-lookup"><span data-stu-id="fae88-171">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="fae88-172">若要自訂 UI，請 `id` `components-reconnect-modal` 在 [ `<body>` *_Host. cshtml* ] 頁面的中，定義具有之的元素 Razor ：</span><span class="sxs-lookup"><span data-stu-id="fae88-172">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="fae88-173">下表描述套用至元素的 CSS 類別 `components-reconnect-modal` 。</span><span class="sxs-lookup"><span data-stu-id="fae88-173">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="fae88-174">CSS 類別</span><span class="sxs-lookup"><span data-stu-id="fae88-174">CSS class</span></span>                       | <span data-ttu-id="fae88-175">表示&hellip;</span><span class="sxs-lookup"><span data-stu-id="fae88-175">Indicates&hellip;</span></span> |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | <span data-ttu-id="fae88-176">遺失的連接。</span><span class="sxs-lookup"><span data-stu-id="fae88-176">A lost connection.</span></span> <span data-ttu-id="fae88-177">用戶端正在嘗試重新連線。</span><span class="sxs-lookup"><span data-stu-id="fae88-177">The client is attempting to reconnect.</span></span> <span data-ttu-id="fae88-178">顯示強制回應。</span><span class="sxs-lookup"><span data-stu-id="fae88-178">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="fae88-179">系統會將使用中的連接重新建立至伺服器。</span><span class="sxs-lookup"><span data-stu-id="fae88-179">An active connection is re-established to the server.</span></span> <span data-ttu-id="fae88-180">隱藏強制回應。</span><span class="sxs-lookup"><span data-stu-id="fae88-180">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="fae88-181">重新連接失敗，可能是因為網路失敗。</span><span class="sxs-lookup"><span data-stu-id="fae88-181">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="fae88-182">若要嘗試重新連接，請呼叫 `window.Blazor.reconnect()` 。</span><span class="sxs-lookup"><span data-stu-id="fae88-182">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="fae88-183">已拒絕重新連接。</span><span class="sxs-lookup"><span data-stu-id="fae88-183">Reconnection rejected.</span></span> <span data-ttu-id="fae88-184">已達到伺服器但拒絕連線，而且伺服器上的使用者狀態遺失。</span><span class="sxs-lookup"><span data-stu-id="fae88-184">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="fae88-185">若要重載應用程式，請呼叫 `location.reload()` 。</span><span class="sxs-lookup"><span data-stu-id="fae88-185">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="fae88-186">當下列情況發生時，可能會產生此連接狀態：</span><span class="sxs-lookup"><span data-stu-id="fae88-186">This connection state may result when:</span></span><ul><li><span data-ttu-id="fae88-187">伺服器端線路發生損毀。</span><span class="sxs-lookup"><span data-stu-id="fae88-187">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="fae88-188">用戶端中斷連線的時間夠長，伺服器才會捨棄使用者的狀態。</span><span class="sxs-lookup"><span data-stu-id="fae88-188">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="fae88-189">系統會處置與使用者互動之元件的實例。</span><span class="sxs-lookup"><span data-stu-id="fae88-189">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="fae88-190">伺服器會重新開機，或回收應用程式的背景工作進程。</span><span class="sxs-lookup"><span data-stu-id="fae88-190">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="render-mode"></a><span data-ttu-id="fae88-191">轉譯模式</span><span class="sxs-lookup"><span data-stu-id="fae88-191">Render mode</span></span>

Blazor<span data-ttu-id="fae88-192">伺服器應用程式預設會設定為伺服器上預先呈現的 UI，然後才會建立與伺服器的用戶端連接。</span><span class="sxs-lookup"><span data-stu-id="fae88-192"> Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="fae88-193">這會在 [ *_Host. cshtml* ] 頁面中設定 Razor ：</span><span class="sxs-lookup"><span data-stu-id="fae88-193">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="fae88-194"><xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper.RenderMode>設定元件是否：</span><span class="sxs-lookup"><span data-stu-id="fae88-194"><xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper.RenderMode> configures whether the component:</span></span>

* <span data-ttu-id="fae88-195">會資源清單到頁面中。</span><span class="sxs-lookup"><span data-stu-id="fae88-195">Is prerendered into the page.</span></span>
* <span data-ttu-id="fae88-196">會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動應用程式所需的資訊 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="fae88-196">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper.RenderMode> | <span data-ttu-id="fae88-197">Description</span><span class="sxs-lookup"><span data-stu-id="fae88-197">Description</span></span> |
| --- | --- |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | <span data-ttu-id="fae88-198">將元件轉譯為靜態 HTML，並包含 Blazor 伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="fae88-198">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="fae88-199">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="fae88-199">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | <span data-ttu-id="fae88-200">呈現 Blazor 伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="fae88-200">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="fae88-201">不包含來自元件的輸出。</span><span class="sxs-lookup"><span data-stu-id="fae88-201">Output from the component isn't included.</span></span> <span data-ttu-id="fae88-202">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="fae88-202">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | <span data-ttu-id="fae88-203">將元件轉譯為靜態 HTML。</span><span class="sxs-lookup"><span data-stu-id="fae88-203">Renders the component into static HTML.</span></span> |

<span data-ttu-id="fae88-204">不支援從靜態 HTML 網頁轉譯伺服器元件。</span><span class="sxs-lookup"><span data-stu-id="fae88-204">Rendering server components from a static HTML page isn't supported.</span></span>

### <a name="configure-the-signalr-client-for-blazor-server-apps"></a><span data-ttu-id="fae88-205">設定 SignalR Blazor 伺服器應用程式的用戶端</span><span class="sxs-lookup"><span data-stu-id="fae88-205">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="fae88-206">有時候，您需要設定 SignalR 伺服器應用程式所使用的用戶端 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="fae88-206">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="fae88-207">例如，您可能會想要在用戶端上設定記錄 SignalR 來診斷連線問題。</span><span class="sxs-lookup"><span data-stu-id="fae88-207">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="fae88-208">若要 SignalR 在*Pages/_Host. cshtml*檔案中設定用戶端：</span><span class="sxs-lookup"><span data-stu-id="fae88-208">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="fae88-209">將 `autostart="false"` 屬性加入至 `<script>` 腳本的標記 `blazor.server.js` 。</span><span class="sxs-lookup"><span data-stu-id="fae88-209">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="fae88-210">呼叫 `Blazor.start` 並傳入指定產生器的設定物件 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="fae88-210">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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

### <a name="logging"></a><span data-ttu-id="fae88-211">記錄</span><span class="sxs-lookup"><span data-stu-id="fae88-211">Logging</span></span>

<span data-ttu-id="fae88-212">如需 Blazor 伺服器記錄支援的詳細資訊，請參閱 <xref:fundamentals/logging/index#create-logs-in-blazor> 。</span><span class="sxs-lookup"><span data-stu-id="fae88-212">For information on Blazor Server logging support, see <xref:fundamentals/logging/index#create-logs-in-blazor>.</span></span>
