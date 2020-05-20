---
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a>ASP.NET Core Blazor 裝載模型設定

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

本文涵蓋主控模型設定。

## <a name="blazor-webassembly"></a>BlazorWebAssembly

### <a name="environment"></a>環境

在本機執行應用程式時，環境會預設為開發。 當應用程式發佈時，環境會預設為 [生產]。

裝載的 Blazor WebAssembly 應用程式會透過中介軟體來從伺服器挑選環境，藉由新增標頭來將環境傳達給瀏覽器 `blazor-environment` 。 標頭的值為環境。 裝載的 Blazor 應用程式和伺服器應用程式會共用相同的環境。 如需詳細資訊，包括如何設定環境，請參閱 <xref:fundamentals/environments> 。

針對在本機執行的獨立應用程式，開發伺服器會新增 `blazor-environment` 標頭以指定開發環境。 若要指定其他裝載環境的環境，請新增 `blazor-environment` 標頭。

在 IIS 的下列範例中，將自訂標頭加入至*已發佈的 web.config 檔案*。 Web.config*檔案*位於*bin/RELEASE/{TARGET FRAMEWORK}/publish*資料夾中：

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
> 若要使用 IIS 的自訂*web.config*檔案，而該檔案不會在應用程式發行至*發行*資料夾時遭到覆寫，請參閱 <xref:host-and-deploy/blazor/webassembly#use-a-custom-webconfig> 。

藉由插入和讀取屬性，在元件中取得應用程式的環境 `IWebAssemblyHostEnvironment` `Environment` ：

```razor
@page "/"
@using Microsoft.AspNetCore.Components.WebAssembly.Hosting
@inject IWebAssemblyHostEnvironment HostEnvironment

<h1>Environment example</h1>

<p>Environment: @HostEnvironment.Environment</p>
```

在啟動期間， `WebAssemblyHostBuilder` `IWebAssemblyHostEnvironment` 會透過屬性公開 `HostEnvironment` ，讓開發人員在其程式碼中具有環境特定邏輯：

```csharp
if (builder.HostEnvironment.Environment == "Custom")
{
    ...
};
```

下列便利的擴充方法可讓您檢查目前的環境，以進行開發、生產、預備和自訂環境名稱：

* `IsDevelopment()`
* `IsProduction()`
* `IsStaging()`
* ' IsEnvironment （"{環境名稱}"）

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

`IWebAssemblyHostEnvironment.BaseAddress`當服務無法使用時，可以在啟動期間使用此屬性 `NavigationManager` 。

### <a name="configuration"></a>組態

BlazorWebAssembly 會從載入設定：

* 應用程式佈建檔案（預設為）：
  * *wwwroot/appsettings. json*
  * *wwwroot/appsettings。{環境}. json*
* 應用程式註冊的其他設定[提供者](xref:fundamentals/configuration/index)。 並非所有提供者都適用于 Blazor WebAssembly apps。 澄清 Blazor [ Blazor WASM （dotnet/AspNetCore #18134）的設定提供者](https://github.com/dotnet/AspNetCore.Docs/issues/18134)，即可追蹤支援 WebAssembly 的提供者。

> [!WARNING]
> Blazor使用者可以看到 WebAssembly 應用程式中的設定。 **請勿在設定中儲存應用程式秘密或認證。**

如需設定提供者的詳細資訊，請參閱 <xref:fundamentals/configuration/index> 。

#### <a name="app-settings-configuration"></a>應用程式設定

*wwwroot/appsettings. json*：

```json
{
  "message": "Hello from config!"
}
```

將 <xref:Microsoft.Extensions.Configuration.IConfiguration> 實例插入元件以存取設定資料：

```razor
@page "/"
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<h1>Configuration example</h1>

<p>Message: @Configuration["message"]</p>
```

#### <a name="provider-configuration"></a>提供者設定

下列範例會使用 <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationSource> 來提供額外的設定：

`Program.Main`:

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

將 <xref:Microsoft.Extensions.Configuration.IConfiguration> 實例插入元件以存取設定資料：

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

若要將*wwwroot*資料夾中的其他設定檔讀取到設定中，請使用 `HttpClient` 來取得檔案的內容。 使用此方法時，現有的 `HttpClient` 服務註冊可以使用已建立的本機用戶端來讀取檔案，如下列範例所示：

*wwwroot/cars*：

```json
{
    "size": "tiny"
}
```

`Program.Main`:

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

#### <a name="authentication-configuration"></a>驗證設定

*wwwroot/appsettings. json*：

```json
{
  "AzureAD": {
    "Authority": "https://login.microsoftonline.com/",
    "ClientId": "aeaebf0f-d416-4d92-a08f-e1d5b51fc494"
  }
}
```

`Program.Main`:

```csharp
builder.Services.AddOidcAuthentication(options =>
    builder.Configuration.Bind("AzureAD", options);
```

#### <a name="logging-configuration"></a>記錄設定

*wwwroot/appsettings. json*：

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

`Program.Main`:

```csharp
builder.Logging.AddConfiguration(
    builder.Configuration.GetSection("Logging"));
```

#### <a name="host-builder-configuration"></a>主機產生器設定

`Program.Main`:

```csharp
var hostname = builder.Configuration["HostName"];
```

#### <a name="cached-configuration"></a>快取設定

系統會快取設定檔以供離線使用。 使用[漸進式 Web 應用程式（pwa）](xref:blazor/progressive-web-app)，您只能在建立新的部署時更新設定檔。 在部署之間編輯設定檔沒有任何作用，因為：

* 使用者有檔案的快取版本，這些檔案會繼續使用。
* PWA 的*service-worker*和*service-worker-assets*檔案必須在編譯時重建，在使用者下一次線上流覽應用程式時，該應用程式會發出通知。

如需 Pwa 如何處理背景更新的詳細資訊，請參閱 <xref:blazor/progressive-web-app#background-updates> 。

### <a name="logging"></a>記錄

如需 Blazor WebAssembly 記錄支援的詳細資訊，請參閱 <xref:fundamentals/logging/index#create-logs-in-blazor> 。

## <a name="blazor-server"></a>Blazor伺服器

### <a name="reflect-the-connection-state-in-the-ui"></a>反映 UI 中的連接狀態

當用戶端偵測到連線已遺失時，會在用戶端嘗試重新連線時，向使用者顯示預設的 UI。 如果重新連線失敗，則會提供使用者重試的選項。

若要自訂 UI，請 `id` `components-reconnect-modal` 在 [ `<body>` *_Host. cshtml* ] 頁面的中，定義具有之的元素 Razor ：

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

下表描述套用至元素的 CSS 類別 `components-reconnect-modal` 。

| CSS 類別                       | 表示&hellip; |
| ---
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

---------------- |---標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

--------- | |`components-reconnect-show`     |遺失的連接。 用戶端正在嘗試重新連線。 顯示強制回應。 | |`components-reconnect-hide`     |系統會將使用中的連接重新建立至伺服器。 隱藏強制回應。 | |`components-reconnect-failed`   |重新連接失敗，可能是因為網路失敗。 若要嘗試重新連接，請呼叫 `window.Blazor.reconnect()` 。 | |`components-reconnect-rejected` |已拒絕重新連接。 已達到伺服器但拒絕連線，而且伺服器上的使用者狀態遺失。 若要重載應用程式，請呼叫 `location.reload()` 。 當下列情況發生時，可能會產生此連接狀態：<ul><li>伺服器端線路發生損毀。</li><li>用戶端中斷連線的時間夠長，伺服器才會捨棄使用者的狀態。 系統會處置與使用者互動之元件的實例。</li><li>伺服器會重新開機，或回收應用程式的背景工作進程。</li></ul> |

### <a name="render-mode"></a>轉譯模式

Blazor伺服器應用程式預設會設定為伺服器上預先呈現的 UI，然後才會建立與伺服器的用戶端連接。 這會在 [ *_Host. cshtml* ] 頁面中設定 Razor ：

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

`RenderMode`設定元件是否：

* 會資源清單到頁面中。
* 會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動應用程式所需的資訊 Blazor 。

| `RenderMode`        | 說明 |
| ---
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

---------- |---標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 主控模型設定 ' 作者：描述： ' 瞭解 Blazor 裝載模型設定，包括如何將元件整合 Razor 至 Razor 頁面和 MVC 應用程式。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

------ | |`ServerPrerendered` |將元件轉譯為靜態 HTML，並包含 Blazor 伺服器應用程式的標記。 當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。 | |`Server`            |呈現 Blazor 伺服器應用程式的標記。 不包含來自元件的輸出。 當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。 | |`Static`            |將元件轉譯為靜態 HTML。 |

不支援從靜態 HTML 網頁轉譯伺服器元件。

### <a name="configure-the-signalr-client-for-blazor-server-apps"></a>設定 SignalR Blazor 伺服器應用程式的用戶端

有時候，您需要設定 SignalR 伺服器應用程式所使用的用戶端 Blazor 。 例如，您可能會想要在用戶端上設定記錄 SignalR 來診斷連線問題。

若要 SignalR 在*Pages/_Host. cshtml*檔案中設定用戶端：

* 將 `autostart="false"` 屬性加入至 `<script>` 腳本的標記 `blazor.server.js` 。
* 呼叫 `Blazor.start` 並傳入指定產生器的設定物件 SignalR 。

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

### <a name="logging"></a>記錄

如需 Blazor 伺服器記錄支援的詳細資訊，請參閱 <xref:fundamentals/logging/index#create-logs-in-blazor> 。
