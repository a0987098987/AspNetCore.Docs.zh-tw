---
<span data-ttu-id="912a7-101">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-101">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-102">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-102">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-103">'Blazor'</span></span>
- <span data-ttu-id="912a7-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-104">'Identity'</span></span>
- <span data-ttu-id="912a7-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-106">'Razor'</span></span>
- <span data-ttu-id="912a7-107">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-107">'SignalR' uid:</span></span> 

---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="912a7-108">ASP.NET Core Blazor 裝載模型設定</span><span class="sxs-lookup"><span data-stu-id="912a7-108">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="912a7-109">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="912a7-109">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="912a7-110">本文涵蓋主控模型設定。</span><span class="sxs-lookup"><span data-stu-id="912a7-110">This article covers hosting model configuration.</span></span>

## <a name="blazor-webassembly"></a>Blazor<span data-ttu-id="912a7-111">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="912a7-111"> WebAssembly</span></span>

### <a name="environment"></a><span data-ttu-id="912a7-112">環境</span><span class="sxs-lookup"><span data-stu-id="912a7-112">Environment</span></span>

<span data-ttu-id="912a7-113">在本機執行應用程式時，環境會預設為開發。</span><span class="sxs-lookup"><span data-stu-id="912a7-113">When running an app locally, the environment defaults to Development.</span></span> <span data-ttu-id="912a7-114">當應用程式發佈時，環境會預設為 [生產]。</span><span class="sxs-lookup"><span data-stu-id="912a7-114">When the app is published, the environment defaults to Production.</span></span>

<span data-ttu-id="912a7-115">裝載的 Blazor WebAssembly 應用程式會透過中介軟體來從伺服器挑選環境，藉由新增標頭來將環境傳達給瀏覽器 `blazor-environment` 。</span><span class="sxs-lookup"><span data-stu-id="912a7-115">A hosted Blazor WebAssembly app picks up the environment from the server via a middleware that communicates the environment to the browser by adding the `blazor-environment` header.</span></span> <span data-ttu-id="912a7-116">標頭的值為環境。</span><span class="sxs-lookup"><span data-stu-id="912a7-116">The value of the header is the environment.</span></span> <span data-ttu-id="912a7-117">裝載的 Blazor 應用程式和伺服器應用程式會共用相同的環境。</span><span class="sxs-lookup"><span data-stu-id="912a7-117">The hosted Blazor app and the server app share the same environment.</span></span> <span data-ttu-id="912a7-118">如需詳細資訊，包括如何設定環境，請參閱 <xref:fundamentals/environments> 。</span><span class="sxs-lookup"><span data-stu-id="912a7-118">For more information, including how to configure the environment, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="912a7-119">針對在本機執行的獨立應用程式，開發伺服器會新增 `blazor-environment` 標頭以指定開發環境。</span><span class="sxs-lookup"><span data-stu-id="912a7-119">For a standalone app running locally, the development server adds the `blazor-environment` header to specify the Development environment.</span></span> <span data-ttu-id="912a7-120">若要指定其他裝載環境的環境，請新增 `blazor-environment` 標頭。</span><span class="sxs-lookup"><span data-stu-id="912a7-120">To specify the environment for other hosting environments, add the `blazor-environment` header.</span></span>

<span data-ttu-id="912a7-121">在 IIS 的下列範例中，將自訂標頭加入至*已發佈的 web.config 檔案*。</span><span class="sxs-lookup"><span data-stu-id="912a7-121">In the following example for IIS, add the custom header to the published *web.config* file.</span></span> <span data-ttu-id="912a7-122">Web.config*檔案*位於*bin/RELEASE/{TARGET FRAMEWORK}/publish*資料夾中：</span><span class="sxs-lookup"><span data-stu-id="912a7-122">The *web.config* file is located in the *bin/Release/{TARGET FRAMEWORK}/publish* folder:</span></span>

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
> <span data-ttu-id="912a7-123">若要使用 IIS 的自訂*web.config*檔案，而該檔案不會在應用程式發行至*發行*資料夾時遭到覆寫，請參閱 <xref:host-and-deploy/blazor/webassembly#use-a-custom-webconfig> 。</span><span class="sxs-lookup"><span data-stu-id="912a7-123">To use a custom *web.config* file for IIS that isn't overwritten when the app is published to the *publish* folder, see <xref:host-and-deploy/blazor/webassembly#use-a-custom-webconfig>.</span></span>

<span data-ttu-id="912a7-124">藉由插入和讀取屬性，在元件中取得應用程式的環境 `IWebAssemblyHostEnvironment` `Environment` ：</span><span class="sxs-lookup"><span data-stu-id="912a7-124">Obtain the app's environment in a component by injecting `IWebAssemblyHostEnvironment` and reading the `Environment` property:</span></span>

```razor
@page "/"
@using Microsoft.AspNetCore.Components.WebAssembly.Hosting
@inject IWebAssemblyHostEnvironment HostEnvironment

<h1>Environment example</h1>

<p>Environment: @HostEnvironment.Environment</p>
```

<span data-ttu-id="912a7-125">在啟動期間， `WebAssemblyHostBuilder` `IWebAssemblyHostEnvironment` 會透過屬性公開 `HostEnvironment` ，讓開發人員在其程式碼中具有環境特定邏輯：</span><span class="sxs-lookup"><span data-stu-id="912a7-125">During startup, the `WebAssemblyHostBuilder` exposes the `IWebAssemblyHostEnvironment` through the `HostEnvironment` property, which enables developers to have environment-specific logic in their code:</span></span>

```csharp
if (builder.HostEnvironment.Environment == "Custom")
{
    ...
};
```

<span data-ttu-id="912a7-126">下列便利的擴充方法可讓您檢查目前的環境，以進行開發、生產、預備和自訂環境名稱：</span><span class="sxs-lookup"><span data-stu-id="912a7-126">The following convenience extension methods permit checking the current environment for Development, Production, Staging, and custom environment names:</span></span>

* `IsDevelopment()`
* `IsProduction()`
* `IsStaging()`
* <span data-ttu-id="912a7-127">' IsEnvironment （"{環境名稱}"）</span><span class="sxs-lookup"><span data-stu-id="912a7-127">\`IsEnvironment("{ENVIRONMENT NAME}")</span></span>

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

<span data-ttu-id="912a7-128">`IWebAssemblyHostEnvironment.BaseAddress`當服務無法使用時，可以在啟動期間使用此屬性 `NavigationManager` 。</span><span class="sxs-lookup"><span data-stu-id="912a7-128">The `IWebAssemblyHostEnvironment.BaseAddress` property can be used during startup when the `NavigationManager` service isn't available.</span></span>

### <a name="configuration"></a><span data-ttu-id="912a7-129">組態</span><span class="sxs-lookup"><span data-stu-id="912a7-129">Configuration</span></span>

Blazor<span data-ttu-id="912a7-130">WebAssembly 會從載入設定：</span><span class="sxs-lookup"><span data-stu-id="912a7-130"> WebAssembly loads configuration from:</span></span>

* <span data-ttu-id="912a7-131">應用程式佈建檔案（預設為）：</span><span class="sxs-lookup"><span data-stu-id="912a7-131">App settings files by default:</span></span>
  * <span data-ttu-id="912a7-132">*wwwroot/appsettings. json*</span><span class="sxs-lookup"><span data-stu-id="912a7-132">*wwwroot/appsettings.json*</span></span>
  * <span data-ttu-id="912a7-133">*wwwroot/appsettings。{環境}. json*</span><span class="sxs-lookup"><span data-stu-id="912a7-133">*wwwroot/appsettings.{ENVIRONMENT}.json*</span></span>
* <span data-ttu-id="912a7-134">應用程式註冊的其他設定[提供者](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="912a7-134">Other [configuration providers](xref:fundamentals/configuration/index) registered by the app.</span></span> <span data-ttu-id="912a7-135">並非所有提供者都適用于 Blazor WebAssembly apps。</span><span class="sxs-lookup"><span data-stu-id="912a7-135">Not all providers are appropriate for Blazor WebAssembly apps.</span></span> <span data-ttu-id="912a7-136">澄清 Blazor [ Blazor WASM （dotnet/AspNetCore #18134）的設定提供者](https://github.com/dotnet/AspNetCore.Docs/issues/18134)，即可追蹤支援 WebAssembly 的提供者。</span><span class="sxs-lookup"><span data-stu-id="912a7-136">Clarification on which providers are supported for Blazor WebAssembly is tracked by [Clarify configuration providers for Blazor WASM (dotnet/AspNetCore.Docs #18134)](https://github.com/dotnet/AspNetCore.Docs/issues/18134).</span></span>

> [!WARNING]
> <span data-ttu-id="912a7-137">Blazor使用者可以看到 WebAssembly 應用程式中的設定。</span><span class="sxs-lookup"><span data-stu-id="912a7-137">Configuration in a Blazor WebAssembly app is visible to users.</span></span> <span data-ttu-id="912a7-138">**請勿在設定中儲存應用程式秘密或認證。**</span><span class="sxs-lookup"><span data-stu-id="912a7-138">**Don't store app secrets or credentials in configuration.**</span></span>

<span data-ttu-id="912a7-139">如需設定提供者的詳細資訊，請參閱 <xref:fundamentals/configuration/index> 。</span><span class="sxs-lookup"><span data-stu-id="912a7-139">For more information on configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

#### <a name="app-settings-configuration"></a><span data-ttu-id="912a7-140">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="912a7-140">App settings configuration</span></span>

<span data-ttu-id="912a7-141">*wwwroot/appsettings. json*：</span><span class="sxs-lookup"><span data-stu-id="912a7-141">*wwwroot/appsettings.json*:</span></span>

```json
{
  "message": "Hello from config!"
}
```

<span data-ttu-id="912a7-142">將 <xref:Microsoft.Extensions.Configuration.IConfiguration> 實例插入元件以存取設定資料：</span><span class="sxs-lookup"><span data-stu-id="912a7-142">Inject an <xref:Microsoft.Extensions.Configuration.IConfiguration> instance into a component to access the configuration data:</span></span>

```razor
@page "/"
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<h1>Configuration example</h1>

<p>Message: @Configuration["message"]</p>
```

#### <a name="provider-configuration"></a><span data-ttu-id="912a7-143">提供者設定</span><span class="sxs-lookup"><span data-stu-id="912a7-143">Provider configuration</span></span>

<span data-ttu-id="912a7-144">下列範例會使用 <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationSource> 來提供額外的設定：</span><span class="sxs-lookup"><span data-stu-id="912a7-144">The following example uses a <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationSource> to supply additional configuration:</span></span>

<span data-ttu-id="912a7-145">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="912a7-145">`Program.Main`:</span></span>

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

<span data-ttu-id="912a7-146">將 <xref:Microsoft.Extensions.Configuration.IConfiguration> 實例插入元件以存取設定資料：</span><span class="sxs-lookup"><span data-stu-id="912a7-146">Inject an <xref:Microsoft.Extensions.Configuration.IConfiguration> instance into a component to access the configuration data:</span></span>

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

<span data-ttu-id="912a7-147">若要將*wwwroot*資料夾中的其他設定檔讀取到設定中，請使用 `HttpClient` 來取得檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="912a7-147">To read other configuration files from the *wwwroot* folder into configuration, use an `HttpClient` to obtain the file's content.</span></span> <span data-ttu-id="912a7-148">使用此方法時，現有的 `HttpClient` 服務註冊可以使用已建立的本機用戶端來讀取檔案，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="912a7-148">When using this approach, the existing `HttpClient` service registration can use the local client created to read the file, as the following example shows:</span></span>

<span data-ttu-id="912a7-149">*wwwroot/cars*：</span><span class="sxs-lookup"><span data-stu-id="912a7-149">*wwwroot/cars.json*:</span></span>

```json
{
    "size": "tiny"
}
```

<span data-ttu-id="912a7-150">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="912a7-150">`Program.Main`:</span></span>

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

#### <a name="authentication-configuration"></a><span data-ttu-id="912a7-151">驗證設定</span><span class="sxs-lookup"><span data-stu-id="912a7-151">Authentication configuration</span></span>

<span data-ttu-id="912a7-152">*wwwroot/appsettings. json*：</span><span class="sxs-lookup"><span data-stu-id="912a7-152">*wwwroot/appsettings.json*:</span></span>

```json
{
  "AzureAD": {
    "Authority": "https://login.microsoftonline.com/",
    "ClientId": "aeaebf0f-d416-4d92-a08f-e1d5b51fc494"
  }
}
```

<span data-ttu-id="912a7-153">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="912a7-153">`Program.Main`:</span></span>

```csharp
builder.Services.AddOidcAuthentication(options =>
    builder.Configuration.Bind("AzureAD", options);
```

#### <a name="logging-configuration"></a><span data-ttu-id="912a7-154">記錄設定</span><span class="sxs-lookup"><span data-stu-id="912a7-154">Logging configuration</span></span>

<span data-ttu-id="912a7-155">*wwwroot/appsettings. json*：</span><span class="sxs-lookup"><span data-stu-id="912a7-155">*wwwroot/appsettings.json*:</span></span>

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

<span data-ttu-id="912a7-156">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="912a7-156">`Program.Main`:</span></span>

```csharp
builder.Logging.AddConfiguration(
    builder.Configuration.GetSection("Logging"));
```

#### <a name="host-builder-configuration"></a><span data-ttu-id="912a7-157">主機產生器設定</span><span class="sxs-lookup"><span data-stu-id="912a7-157">Host builder configuration</span></span>

<span data-ttu-id="912a7-158">`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="912a7-158">`Program.Main`:</span></span>

```csharp
var hostname = builder.Configuration["HostName"];
```

#### <a name="cached-configuration"></a><span data-ttu-id="912a7-159">快取設定</span><span class="sxs-lookup"><span data-stu-id="912a7-159">Cached configuration</span></span>

<span data-ttu-id="912a7-160">系統會快取設定檔以供離線使用。</span><span class="sxs-lookup"><span data-stu-id="912a7-160">Configuration files are cached for offline use.</span></span> <span data-ttu-id="912a7-161">使用[漸進式 Web 應用程式（pwa）](xref:blazor/progressive-web-app)，您只能在建立新的部署時更新設定檔。</span><span class="sxs-lookup"><span data-stu-id="912a7-161">With [Progressive Web Applications (PWAs)](xref:blazor/progressive-web-app), you can only update configuration files when creating a new deployment.</span></span> <span data-ttu-id="912a7-162">在部署之間編輯設定檔沒有任何作用，因為：</span><span class="sxs-lookup"><span data-stu-id="912a7-162">Editing configuration files between deployments has no effect because:</span></span>

* <span data-ttu-id="912a7-163">使用者有檔案的快取版本，這些檔案會繼續使用。</span><span class="sxs-lookup"><span data-stu-id="912a7-163">Users have cached versions of the files that they continue to use.</span></span>
* <span data-ttu-id="912a7-164">PWA 的*service-worker*和*service-worker-assets*檔案必須在編譯時重建，在使用者下一次線上流覽應用程式時，該應用程式會發出通知。</span><span class="sxs-lookup"><span data-stu-id="912a7-164">The PWA's *service-worker.js* and *service-worker-assets.js* files must be rebuilt on compilation, which signal to the app on the user's next online visit that the app has been redeployed.</span></span>

<span data-ttu-id="912a7-165">如需 Pwa 如何處理背景更新的詳細資訊，請參閱 <xref:blazor/progressive-web-app#background-updates> 。</span><span class="sxs-lookup"><span data-stu-id="912a7-165">For more information on how background updates are handled by PWAs, see <xref:blazor/progressive-web-app#background-updates>.</span></span>

### <a name="logging"></a><span data-ttu-id="912a7-166">記錄</span><span class="sxs-lookup"><span data-stu-id="912a7-166">Logging</span></span>

<span data-ttu-id="912a7-167">如需 Blazor WebAssembly 記錄支援的詳細資訊，請參閱 <xref:fundamentals/logging/index#create-logs-in-blazor> 。</span><span class="sxs-lookup"><span data-stu-id="912a7-167">For information on Blazor WebAssembly logging support, see <xref:fundamentals/logging/index#create-logs-in-blazor>.</span></span>

## <a name="blazor-server"></a>Blazor<span data-ttu-id="912a7-168">伺服器</span><span class="sxs-lookup"><span data-stu-id="912a7-168"> Server</span></span>

### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="912a7-169">反映 UI 中的連接狀態</span><span class="sxs-lookup"><span data-stu-id="912a7-169">Reflect the connection state in the UI</span></span>

<span data-ttu-id="912a7-170">當用戶端偵測到連線已遺失時，會在用戶端嘗試重新連線時，向使用者顯示預設的 UI。</span><span class="sxs-lookup"><span data-stu-id="912a7-170">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="912a7-171">如果重新連線失敗，則會提供使用者重試的選項。</span><span class="sxs-lookup"><span data-stu-id="912a7-171">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="912a7-172">若要自訂 UI，請 `id` `components-reconnect-modal` 在 [ `<body>` *_Host. cshtml* ] 頁面的中，定義具有之的元素 Razor ：</span><span class="sxs-lookup"><span data-stu-id="912a7-172">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="912a7-173">下表描述套用至元素的 CSS 類別 `components-reconnect-modal` 。</span><span class="sxs-lookup"><span data-stu-id="912a7-173">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="912a7-174">CSS 類別</span><span class="sxs-lookup"><span data-stu-id="912a7-174">CSS class</span></span>                       | <span data-ttu-id="912a7-175">表示&hellip;</span><span class="sxs-lookup"><span data-stu-id="912a7-175">Indicates&hellip;</span></span> |
| ---
<span data-ttu-id="912a7-176">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-176">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-177">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-177">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-178">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-178">'Blazor'</span></span>
- <span data-ttu-id="912a7-179">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-179">'Identity'</span></span>
- <span data-ttu-id="912a7-180">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-180">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-181">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-181">'Razor'</span></span>
- <span data-ttu-id="912a7-182">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-182">'SignalR' uid:</span></span> 

-
<span data-ttu-id="912a7-183">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-183">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-184">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-184">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-185">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-185">'Blazor'</span></span>
- <span data-ttu-id="912a7-186">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-186">'Identity'</span></span>
- <span data-ttu-id="912a7-187">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-187">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-188">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-188">'Razor'</span></span>
- <span data-ttu-id="912a7-189">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-189">'SignalR' uid:</span></span> 

-
<span data-ttu-id="912a7-190">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-190">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-191">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-191">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-192">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-192">'Blazor'</span></span>
- <span data-ttu-id="912a7-193">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-193">'Identity'</span></span>
- <span data-ttu-id="912a7-194">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-194">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-195">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-195">'Razor'</span></span>
- <span data-ttu-id="912a7-196">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-196">'SignalR' uid:</span></span> 

-
<span data-ttu-id="912a7-197">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-197">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-198">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-198">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-199">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-199">'Blazor'</span></span>
- <span data-ttu-id="912a7-200">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-200">'Identity'</span></span>
- <span data-ttu-id="912a7-201">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-201">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-202">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-202">'Razor'</span></span>
- <span data-ttu-id="912a7-203">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-203">'SignalR' uid:</span></span> 

-
<span data-ttu-id="912a7-204">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-204">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-205">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-205">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-206">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-206">'Blazor'</span></span>
- <span data-ttu-id="912a7-207">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-207">'Identity'</span></span>
- <span data-ttu-id="912a7-208">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-208">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-209">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-209">'Razor'</span></span>
- <span data-ttu-id="912a7-210">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-210">'SignalR' uid:</span></span> 

-
<span data-ttu-id="912a7-211">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-211">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-212">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-212">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-213">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-213">'Blazor'</span></span>
- <span data-ttu-id="912a7-214">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-214">'Identity'</span></span>
- <span data-ttu-id="912a7-215">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-215">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-216">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-216">'Razor'</span></span>
- <span data-ttu-id="912a7-217">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-217">'SignalR' uid:</span></span> 

-
<span data-ttu-id="912a7-218">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-218">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-219">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-219">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-220">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-220">'Blazor'</span></span>
- <span data-ttu-id="912a7-221">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-221">'Identity'</span></span>
- <span data-ttu-id="912a7-222">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-222">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-223">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-223">'Razor'</span></span>
- <span data-ttu-id="912a7-224">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-224">'SignalR' uid:</span></span> 

-
<span data-ttu-id="912a7-225">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-225">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-226">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-226">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-227">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-227">'Blazor'</span></span>
- <span data-ttu-id="912a7-228">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-228">'Identity'</span></span>
- <span data-ttu-id="912a7-229">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-229">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-230">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-230">'Razor'</span></span>
- <span data-ttu-id="912a7-231">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-231">'SignalR' uid:</span></span> 

-
<span data-ttu-id="912a7-232">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-232">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-233">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-233">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-234">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-234">'Blazor'</span></span>
- <span data-ttu-id="912a7-235">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-235">'Identity'</span></span>
- <span data-ttu-id="912a7-236">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-236">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-237">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-237">'Razor'</span></span>
- <span data-ttu-id="912a7-238">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-238">'SignalR' uid:</span></span> 

-
<span data-ttu-id="912a7-239">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-239">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-240">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-240">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-241">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-241">'Blazor'</span></span>
- <span data-ttu-id="912a7-242">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-242">'Identity'</span></span>
- <span data-ttu-id="912a7-243">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-243">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-244">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-244">'Razor'</span></span>
- <span data-ttu-id="912a7-245">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-245">'SignalR' uid:</span></span> 

-
<span data-ttu-id="912a7-246">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-246">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-247">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-247">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-248">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-248">'Blazor'</span></span>
- <span data-ttu-id="912a7-249">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-249">'Identity'</span></span>
- <span data-ttu-id="912a7-250">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-250">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-251">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-251">'Razor'</span></span>
- <span data-ttu-id="912a7-252">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-252">'SignalR' uid:</span></span> 

-
<span data-ttu-id="912a7-253">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-253">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-254">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-254">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-255">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-255">'Blazor'</span></span>
- <span data-ttu-id="912a7-256">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-256">'Identity'</span></span>
- <span data-ttu-id="912a7-257">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-257">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-258">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-258">'Razor'</span></span>
- <span data-ttu-id="912a7-259">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-259">'SignalR' uid:</span></span> 

-
<span data-ttu-id="912a7-260">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-260">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-261">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-261">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-262">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-262">'Blazor'</span></span>
- <span data-ttu-id="912a7-263">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-263">'Identity'</span></span>
- <span data-ttu-id="912a7-264">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-264">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-265">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-265">'Razor'</span></span>
- <span data-ttu-id="912a7-266">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-266">'SignalR' uid:</span></span> 

<span data-ttu-id="912a7-267">---------------- |---標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-267">---------------- | --- title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-268">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-268">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-269">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-269">'Blazor'</span></span>
- <span data-ttu-id="912a7-270">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-270">'Identity'</span></span>
- <span data-ttu-id="912a7-271">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-271">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-272">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-272">'Razor'</span></span>
- <span data-ttu-id="912a7-273">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-273">'SignalR' uid:</span></span> 

-
<span data-ttu-id="912a7-274">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-274">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-275">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-275">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-276">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-276">'Blazor'</span></span>
- <span data-ttu-id="912a7-277">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-277">'Identity'</span></span>
- <span data-ttu-id="912a7-278">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-278">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-279">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-279">'Razor'</span></span>
- <span data-ttu-id="912a7-280">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-280">'SignalR' uid:</span></span> 

-
<span data-ttu-id="912a7-281">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-281">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-282">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-282">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-283">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-283">'Blazor'</span></span>
- <span data-ttu-id="912a7-284">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-284">'Identity'</span></span>
- <span data-ttu-id="912a7-285">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-285">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-286">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-286">'Razor'</span></span>
- <span data-ttu-id="912a7-287">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-287">'SignalR' uid:</span></span> 

-
<span data-ttu-id="912a7-288">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-288">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-289">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-289">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-290">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-290">'Blazor'</span></span>
- <span data-ttu-id="912a7-291">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-291">'Identity'</span></span>
- <span data-ttu-id="912a7-292">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-292">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-293">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-293">'Razor'</span></span>
- <span data-ttu-id="912a7-294">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-294">'SignalR' uid:</span></span> 

-
<span data-ttu-id="912a7-295">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-295">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-296">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-296">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-297">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-297">'Blazor'</span></span>
- <span data-ttu-id="912a7-298">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-298">'Identity'</span></span>
- <span data-ttu-id="912a7-299">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-299">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-300">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-300">'Razor'</span></span>
- <span data-ttu-id="912a7-301">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-301">'SignalR' uid:</span></span> 

-
<span data-ttu-id="912a7-302">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-302">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-303">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-303">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-304">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-304">'Blazor'</span></span>
- <span data-ttu-id="912a7-305">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-305">'Identity'</span></span>
- <span data-ttu-id="912a7-306">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-306">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-307">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-307">'Razor'</span></span>
- <span data-ttu-id="912a7-308">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-308">'SignalR' uid:</span></span> 

<span data-ttu-id="912a7-309">--------- | |`components-reconnect-show`     |遺失的連接。</span><span class="sxs-lookup"><span data-stu-id="912a7-309">--------- | | `components-reconnect-show`     | A lost connection.</span></span> <span data-ttu-id="912a7-310">用戶端正在嘗試重新連線。</span><span class="sxs-lookup"><span data-stu-id="912a7-310">The client is attempting to reconnect.</span></span> <span data-ttu-id="912a7-311">顯示強制回應。</span><span class="sxs-lookup"><span data-stu-id="912a7-311">Show the modal.</span></span> <span data-ttu-id="912a7-312">| |`components-reconnect-hide`     |系統會將使用中的連接重新建立至伺服器。</span><span class="sxs-lookup"><span data-stu-id="912a7-312">| | `components-reconnect-hide`     | An active connection is re-established to the server.</span></span> <span data-ttu-id="912a7-313">隱藏強制回應。</span><span class="sxs-lookup"><span data-stu-id="912a7-313">Hide the modal.</span></span> <span data-ttu-id="912a7-314">| |`components-reconnect-failed`   |重新連接失敗，可能是因為網路失敗。</span><span class="sxs-lookup"><span data-stu-id="912a7-314">| | `components-reconnect-failed`   | Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="912a7-315">若要嘗試重新連接，請呼叫 `window.Blazor.reconnect()` 。</span><span class="sxs-lookup"><span data-stu-id="912a7-315">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> <span data-ttu-id="912a7-316">| |`components-reconnect-rejected` |已拒絕重新連接。</span><span class="sxs-lookup"><span data-stu-id="912a7-316">| | `components-reconnect-rejected` | Reconnection rejected.</span></span> <span data-ttu-id="912a7-317">已達到伺服器但拒絕連線，而且伺服器上的使用者狀態遺失。</span><span class="sxs-lookup"><span data-stu-id="912a7-317">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="912a7-318">若要重載應用程式，請呼叫 `location.reload()` 。</span><span class="sxs-lookup"><span data-stu-id="912a7-318">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="912a7-319">當下列情況發生時，可能會產生此連接狀態：</span><span class="sxs-lookup"><span data-stu-id="912a7-319">This connection state may result when:</span></span><ul><li><span data-ttu-id="912a7-320">伺服器端線路發生損毀。</span><span class="sxs-lookup"><span data-stu-id="912a7-320">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="912a7-321">用戶端中斷連線的時間夠長，伺服器才會捨棄使用者的狀態。</span><span class="sxs-lookup"><span data-stu-id="912a7-321">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="912a7-322">系統會處置與使用者互動之元件的實例。</span><span class="sxs-lookup"><span data-stu-id="912a7-322">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="912a7-323">伺服器會重新開機，或回收應用程式的背景工作進程。</span><span class="sxs-lookup"><span data-stu-id="912a7-323">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="render-mode"></a><span data-ttu-id="912a7-324">轉譯模式</span><span class="sxs-lookup"><span data-stu-id="912a7-324">Render mode</span></span>

Blazor<span data-ttu-id="912a7-325">伺服器應用程式預設會設定為伺服器上預先呈現的 UI，然後才會建立與伺服器的用戶端連接。</span><span class="sxs-lookup"><span data-stu-id="912a7-325"> Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="912a7-326">這會在 [ *_Host. cshtml* ] 頁面中設定 Razor ：</span><span class="sxs-lookup"><span data-stu-id="912a7-326">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="912a7-327">`RenderMode`設定元件是否：</span><span class="sxs-lookup"><span data-stu-id="912a7-327">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="912a7-328">會資源清單到頁面中。</span><span class="sxs-lookup"><span data-stu-id="912a7-328">Is prerendered into the page.</span></span>
* <span data-ttu-id="912a7-329">會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動應用程式所需的資訊 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="912a7-329">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="912a7-330">說明</span><span class="sxs-lookup"><span data-stu-id="912a7-330">Description</span></span> |
| ---
<span data-ttu-id="912a7-331">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-331">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-332">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-332">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-333">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-333">'Blazor'</span></span>
- <span data-ttu-id="912a7-334">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-334">'Identity'</span></span>
- <span data-ttu-id="912a7-335">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-335">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-336">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-336">'Razor'</span></span>
- <span data-ttu-id="912a7-337">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-337">'SignalR' uid:</span></span> 

-
<span data-ttu-id="912a7-338">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-338">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-339">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-339">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-340">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-340">'Blazor'</span></span>
- <span data-ttu-id="912a7-341">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-341">'Identity'</span></span>
- <span data-ttu-id="912a7-342">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-342">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-343">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-343">'Razor'</span></span>
- <span data-ttu-id="912a7-344">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-344">'SignalR' uid:</span></span> 

-
<span data-ttu-id="912a7-345">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-345">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-346">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-346">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-347">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-347">'Blazor'</span></span>
- <span data-ttu-id="912a7-348">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-348">'Identity'</span></span>
- <span data-ttu-id="912a7-349">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-349">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-350">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-350">'Razor'</span></span>
- <span data-ttu-id="912a7-351">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-351">'SignalR' uid:</span></span> 

-
<span data-ttu-id="912a7-352">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-352">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-353">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-353">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-354">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-354">'Blazor'</span></span>
- <span data-ttu-id="912a7-355">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-355">'Identity'</span></span>
- <span data-ttu-id="912a7-356">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-356">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-357">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-357">'Razor'</span></span>
- <span data-ttu-id="912a7-358">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-358">'SignalR' uid:</span></span> 

-
<span data-ttu-id="912a7-359">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-359">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-360">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-360">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-361">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-361">'Blazor'</span></span>
- <span data-ttu-id="912a7-362">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-362">'Identity'</span></span>
- <span data-ttu-id="912a7-363">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-363">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-364">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-364">'Razor'</span></span>
- <span data-ttu-id="912a7-365">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-365">'SignalR' uid:</span></span> 

-
<span data-ttu-id="912a7-366">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-366">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-367">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-367">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-368">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-368">'Blazor'</span></span>
- <span data-ttu-id="912a7-369">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-369">'Identity'</span></span>
- <span data-ttu-id="912a7-370">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-370">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-371">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-371">'Razor'</span></span>
- <span data-ttu-id="912a7-372">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-372">'SignalR' uid:</span></span> 

-
<span data-ttu-id="912a7-373">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-373">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-374">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-374">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-375">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-375">'Blazor'</span></span>
- <span data-ttu-id="912a7-376">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-376">'Identity'</span></span>
- <span data-ttu-id="912a7-377">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-377">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-378">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-378">'Razor'</span></span>
- <span data-ttu-id="912a7-379">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-379">'SignalR' uid:</span></span> 

<span data-ttu-id="912a7-380">---------- |---標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-380">---------- | --- title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-381">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-381">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-382">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-382">'Blazor'</span></span>
- <span data-ttu-id="912a7-383">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-383">'Identity'</span></span>
- <span data-ttu-id="912a7-384">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-384">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-385">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-385">'Razor'</span></span>
- <span data-ttu-id="912a7-386">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-386">'SignalR' uid:</span></span> 

-
<span data-ttu-id="912a7-387">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-387">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-388">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-388">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-389">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-389">'Blazor'</span></span>
- <span data-ttu-id="912a7-390">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-390">'Identity'</span></span>
- <span data-ttu-id="912a7-391">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-391">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-392">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-392">'Razor'</span></span>
- <span data-ttu-id="912a7-393">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-393">'SignalR' uid:</span></span> 

-
<span data-ttu-id="912a7-394">標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-394">title: 'ASP.NET Core Blazor hosting model configuration' author: description: 'Learn about Blazor hosting model configuration, including how to integrate Razor components into Razor Pages and MVC apps.'</span></span>
<span data-ttu-id="912a7-395">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="912a7-395">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="912a7-396">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="912a7-396">'Blazor'</span></span>
- <span data-ttu-id="912a7-397">'Identity'</span><span class="sxs-lookup"><span data-stu-id="912a7-397">'Identity'</span></span>
- <span data-ttu-id="912a7-398">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="912a7-398">'Let's Encrypt'</span></span>
- <span data-ttu-id="912a7-399">'Razor'</span><span class="sxs-lookup"><span data-stu-id="912a7-399">'Razor'</span></span>
- <span data-ttu-id="912a7-400">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="912a7-400">'SignalR' uid:</span></span> 

<span data-ttu-id="912a7-401">------ | |`ServerPrerendered` |將元件轉譯為靜態 HTML，並包含 Blazor 伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="912a7-401">------ | | `ServerPrerendered` | Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="912a7-402">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-402">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="912a7-403">| |`Server`            |呈現 Blazor 伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="912a7-403">| | `Server`            | Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="912a7-404">不包含來自元件的輸出。</span><span class="sxs-lookup"><span data-stu-id="912a7-404">Output from the component isn't included.</span></span> <span data-ttu-id="912a7-405">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="912a7-405">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="912a7-406">| |`Static`            |將元件轉譯為靜態 HTML。</span><span class="sxs-lookup"><span data-stu-id="912a7-406">| | `Static`            | Renders the component into static HTML.</span></span> |

<span data-ttu-id="912a7-407">不支援從靜態 HTML 網頁轉譯伺服器元件。</span><span class="sxs-lookup"><span data-stu-id="912a7-407">Rendering server components from a static HTML page isn't supported.</span></span>

### <a name="configure-the-signalr-client-for-blazor-server-apps"></a><span data-ttu-id="912a7-408">設定 SignalR Blazor 伺服器應用程式的用戶端</span><span class="sxs-lookup"><span data-stu-id="912a7-408">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="912a7-409">有時候，您需要設定 SignalR 伺服器應用程式所使用的用戶端 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="912a7-409">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="912a7-410">例如，您可能會想要在用戶端上設定記錄 SignalR 來診斷連線問題。</span><span class="sxs-lookup"><span data-stu-id="912a7-410">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="912a7-411">若要 SignalR 在*Pages/_Host. cshtml*檔案中設定用戶端：</span><span class="sxs-lookup"><span data-stu-id="912a7-411">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="912a7-412">將 `autostart="false"` 屬性加入至 `<script>` 腳本的標記 `blazor.server.js` 。</span><span class="sxs-lookup"><span data-stu-id="912a7-412">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="912a7-413">呼叫 `Blazor.start` 並傳入指定產生器的設定物件 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="912a7-413">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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

### <a name="logging"></a><span data-ttu-id="912a7-414">記錄</span><span class="sxs-lookup"><span data-stu-id="912a7-414">Logging</span></span>

<span data-ttu-id="912a7-415">如需 Blazor 伺服器記錄支援的詳細資訊，請參閱 <xref:fundamentals/logging/index#create-logs-in-blazor> 。</span><span class="sxs-lookup"><span data-stu-id="912a7-415">For information on Blazor Server logging support, see <xref:fundamentals/logging/index#create-logs-in-blazor>.</span></span>
