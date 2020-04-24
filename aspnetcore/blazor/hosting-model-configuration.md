---
title: ASP.NET Core Blazor裝載模型設定
author: guardrex
description: 瞭解Blazor主控模型設定，包括如何將 Razor 元件整合到 RAZOR PAGES 和 MVC 應用程式中。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/23/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: cf5776109368dc7353d7e21bcad1e947561e7eb4
ms.sourcegitcommit: 7bb14d005155a5044c7902a08694ee8ccb20c113
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2020
ms.locfileid: "82111054"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a>ASP.NET Core Blazor 裝載模型設定

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

本文涵蓋主控模型設定。

## <a name="blazor-webassembly"></a>Blazor WebAssembly

### <a name="environment"></a>環境

在本機執行應用程式時，環境會預設為開發。 當應用程式發佈時，環境會預設為 [生產]。

Hosted Blazor WebAssembly 應用程式會透過中介軟體來從伺服器挑選環境，藉由新增`blazor-environment`標頭來將環境傳達給瀏覽器。 標頭的值為環境。 託管的 Blazor 應用程式和伺服器應用程式會共用相同的環境。 如需詳細資訊，包括如何設定環境，請參閱<xref:fundamentals/environments>。

針對在本機執行的獨立應用程式，開發伺服器會`blazor-environment`新增標頭以指定開發環境。 若要指定其他裝載環境的環境，請新增`blazor-environment`標頭。

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
> 若要使用 IIS 的自訂*web.config*檔案，而該檔案不會在應用程式發行至*發行*資料夾時遭到<xref:host-and-deploy/blazor/webassembly#use-a-custom-webconfig>覆寫，請參閱。

藉由插入`IWebAssemblyHostEnvironment`和讀取`Environment`屬性，在元件中取得應用程式的環境：

```razor
@page "/"
@using Microsoft.AspNetCore.Components.WebAssembly.Hosting
@inject IWebAssemblyHostEnvironment HostEnvironment

<h1>Environment example</h1>

<p>Environment: @HostEnvironment.Environment</p>
```

在啟動期間， `WebAssemblyHostBuilder`會`IWebAssemblyHostEnvironment`透過`HostEnvironment`屬性公開，讓開發人員在其程式碼中具有環境特定邏輯：

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

當`IWebAssemblyHostEnvironment.BaseAddress` `NavigationManager`服務無法使用時，可以在啟動期間使用此屬性。

### <a name="configuration"></a>設定

Blazor WebAssembly 支援下列設定：

* 應用程式佈建檔案的檔案設定[提供者](xref:fundamentals/configuration/index#file-configuration-provider)預設為：
  * *wwwroot/appsettings. json*
  * *wwwroot/appsettings。{環境}. json*
* 應用程式註冊的其他設定[提供者](xref:fundamentals/configuration/index)。

> [!WARNING]
> 使用者可以看到 Blazor WebAssembly 應用程式中的設定。 **請勿在設定中儲存應用程式秘密或認證。**

如需設定提供者的詳細資訊<xref:fundamentals/configuration/index>，請參閱。

#### <a name="app-settings-configuration"></a>應用程式設定

*wwwroot/appsettings. json*：

```json
{
  "message": "Hello from config!"
}
```

將<xref:Microsoft.Extensions.Configuration.IConfiguration>實例插入元件以存取設定資料：

```razor
@page "/"
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<h1>Configuration example</h1>

<p>Message: @Configuration["message"]</p>
```

#### <a name="provider-configuration"></a>提供者設定

下列範例會使用<xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationSource>和檔案設定[提供者](xref:fundamentals/configuration/index#file-configuration-provider)來提供額外的設定：

`Program.Main`:

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

將<xref:Microsoft.Extensions.Configuration.IConfiguration>實例插入元件以存取設定資料：

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

如需 Pwa 如何處理背景更新的詳細資訊，請參閱<xref:blazor/progressive-web-app#background-updates>。

### <a name="logging"></a>記錄

如需 Blazor WebAssembly 記錄支援的詳細資訊<xref:fundamentals/logging/index#create-logs-in-blazor>，請參閱。

## <a name="blazor-server"></a>Blazor 伺服器

### <a name="reflect-the-connection-state-in-the-ui"></a>反映 UI 中的連接狀態

當用戶端偵測到連線已遺失時，會在用戶端嘗試重新連線時，向使用者顯示預設的 UI。 如果重新連線失敗，則會提供使用者重試的選項。

若要自訂 UI，請在 *_Host. cshtml* Razor 頁面`<body>`的中，定義具有`id`之的`components-reconnect-modal`元素：

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

下表描述套用至`components-reconnect-modal`元素的 CSS 類別。

| CSS 類別                       | 表示&hellip; |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | 遺失的連接。 用戶端正在嘗試重新連線。 顯示強制回應。 |
| `components-reconnect-hide`     | 系統會將使用中的連接重新建立至伺服器。 隱藏強制回應。 |
| `components-reconnect-failed`   | 重新連接失敗，可能是因為網路失敗。 若要嘗試重新連接`window.Blazor.reconnect()`，請呼叫。 |
| `components-reconnect-rejected` | 已拒絕重新連接。 已達到伺服器但拒絕連線，而且伺服器上的使用者狀態遺失。 若要重載應用程式， `location.reload()`請呼叫。 當下列情況發生時，可能會產生此連接狀態：<ul><li>伺服器端線路發生損毀。</li><li>用戶端中斷連線的時間夠長，伺服器才會捨棄使用者的狀態。 系統會處置與使用者互動之元件的實例。</li><li>伺服器會重新開機，或回收應用程式的背景工作進程。</li></ul> |

### <a name="render-mode"></a>轉譯模式

在建立伺服器的用戶端連接之前，預設會設定 Blazor 伺服器應用程式，以預先呈現伺服器上的 UI。 這會在 *_Host. cshtml* Razor 頁面中設定：

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
* 會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動 Blazor 應用程式所需的資訊。

| `RenderMode`        | 說明 |
| ------------------- | ----------- |
| `ServerPrerendered` | 將元件轉譯為靜態 HTML，並包含Blazor伺服器應用程式的標記。 當使用者代理程式啟動時，會使用此標記來啟動Blazor應用程式。 |
| `Server`            | 呈現Blazor伺服器應用程式的標記。 不包含來自元件的輸出。 當使用者代理程式啟動時，會使用此標記來啟動Blazor應用程式。 |
| `Static`            | 將元件轉譯為靜態 HTML。 |

不支援從靜態 HTML 網頁轉譯伺服器元件。

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a>從 Razor 頁面和 views 轉譯具狀態的互動式元件

可設定狀態的互動式元件可以新增至 Razor 頁面或視圖。

當頁面或視圖呈現時：

* 此元件是使用頁面或視圖所資源清單。
* 用來進行預呈現的初始元件狀態會遺失。
* 建立連接時，會建立新SignalR的元件狀態。

下列 Razor 頁面會呈現`Counter`元件：

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a>從 Razor 頁面和 views 轉譯非互動式元件

在下列 Razor 頁面中， `Counter`元件是使用以表單指定的初始值進行靜態轉譯：

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

<component type="typeof(Counter)" render-mode="Static" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

由於`MyComponent`是以靜態方式呈現，因此元件不能是互動式的。

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a>設定Blazor伺服器SignalR應用程式的用戶端

有時候，您需要設定SignalR Blazor伺服器應用程式所使用的用戶端。 例如，您可能會想要在SignalR用戶端上設定記錄來診斷連線問題。

若要在SignalR *Pages/_Host. cshtml*檔案中設定用戶端：

* 將`autostart="false"`屬性加入至`<script>` `blazor.server.js`腳本的標記。
* 呼叫`Blazor.start`並傳入指定SignalR產生器的設定物件。

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

如需Blazor伺服器記錄支援的詳細資訊<xref:fundamentals/logging/index#create-logs-in-blazor>，請參閱。
