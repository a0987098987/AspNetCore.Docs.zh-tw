---
title: ASP.NET核心Blazor託管模型設定
author: guardrex
description: 瞭解Blazor託管模型配置,包括如何將 Razor 元件整合到 Razor 頁面和 MVC 應用中。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: ca1b3ea9092640ca561b3fbe02ddce6f974c525e
ms.sourcegitcommit: e8dc30453af8bbefcb61857987090d79230a461d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123378"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a>ASP.NET核心布拉佐託管模型配置

由[丹尼爾·羅斯](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

本文介紹託管模型配置。

## <a name="blazor-webassembly"></a>Blazor WebAssembly

### <a name="environment"></a>環境

在本地運行應用時,環境預設為"開發"。 發佈應用時,環境預設為"生產"。

託管的 Blazor WebAssembly 應用透過中間件從伺服器拾取環境,該中間件`blazor-environment`透過添加標頭將環境傳達給瀏覽器。 標頭的值是環境。 託管的 Blazor 應用和伺服器應用共用相同的環境。 有關詳細資訊(包括如何設定環境),請參閱<xref:fundamentals/environments>。

對於在本地運行的獨立應用,開發伺服器將添加`blazor-environment`標頭以指定開發環境。 要指定其他託管環境的環境,請`blazor-environment`添加標頭。

在下面的 IIS 範例中,將自訂標頭添加到已發布*的 Web.config*檔中。 *Web.config*檔案位於*bin/發行/[目標框架]/發佈*資料夾中:

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
> 要對 IIS 使用自訂*Web.config*檔案,該檔在將套用到*的*資料夾時不會覆<xref:host-and-deploy/blazor/webassembly#use-a-custom-webconfig>寫,請參考 。

通過注入`IWebAssemblyHostEnvironment`與讀`Environment`取 屬性,取得元件中的應用環境:

```razor
@page "/"
@using Microsoft.AspNetCore.Components.WebAssembly.Hosting
@inject IWebAssemblyHostEnvironment HostEnvironment

<h1>Environment example</h1>

<p>Environment: @HostEnvironment.Environment</p>
```

### <a name="configuration"></a>組態

自 ASP.NET 酷睿 3.2 預覽 3 版中,Blazor WebAssembly 支援以下配置:

* *wwwroot/appsettings.json*
* *wwwroot/應用設置。[環境].json*

在*wwwroot*資料夾中新增*應用設定.json*檔:

```json
{
  "message": "Hello from config!"
}
```

將<xref:Microsoft.Extensions.Configuration.IConfiguration>實體編寫入元件以存取設定資料:

```razor
@page "/"
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<h1>Configuration example</h1>

<p>Message: @Configuration["message"]</p>
```

> [!WARNING]
> Blazor WebAssembly 應用中的配置對用戶可見。 **不要在配置中存儲應用機密或憑據。**

配置檔將緩存供離線使用。 使用[漸進式 Web 應用程式 (PWA),](xref:blazor/progressive-web-app)只能在建立新部署時更新配置檔。 在部署之間編輯設定檔不起作用,因為:

* 使用者已緩存了他們繼續使用的檔的版本。
* PWA 的服務*工作者.js* *和服務工作者資產.js*檔必須在編譯時重新生成,這向使用者下一次連線訪問時的應用發出信號,表明應用程式已重新部署。

有關 PWA 如何處理後臺更新的詳細資訊,請<xref:blazor/progressive-web-app#background-updates>參閱 。

## <a name="blazor-server"></a>Blazor 伺服器

### <a name="reflect-the-connection-state-in-the-ui"></a>反映 UI 的連線狀態

當用戶端檢測到連接已丟失時,在用戶端嘗試重新連接時向使用者顯示預設 UI。 如果重新連接失敗,則為使用者提供重試的選項。

要自訂 UI,請在 *_Host.cshtml* `<body>`Razor`id`頁`components-reconnect-modal`中定義具有 的元素:

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

下表描述了應用於元素的`components-reconnect-modal`CSS 類。

| CSS 類別                       | 表示&hellip; |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | 失去連接。 用戶端正在嘗試重新連接。 顯示模式。 |
| `components-reconnect-hide`     | 活動連接將重新建立到伺服器。 隱藏模式。 |
| `components-reconnect-failed`   | 重新連接失敗,可能是由於網路故障。 要嘗試重新連線,請`window.Blazor.reconnect()`呼叫 。 |
| `components-reconnect-rejected` | 重新連接被拒絕。 已到達伺服器,但拒絕連接,並且使用者在伺服器上的狀態將丟失。 要重新載入套用,請呼`location.reload()`叫 。 在:<ul><li>伺服器端電路中發生崩潰。</li><li>用戶端斷開連接的時間足夠長,以便伺服器刪除使用者的狀態。 將釋放使用者與之交互的元件的實例。</li><li>重新啟動伺服器,或者回收應用的工作進程。</li></ul> |

### <a name="render-mode"></a>轉譯模式

默認情況下,布拉佐伺服器應用設置為在建立與伺服器的用戶端連接之前在伺服器上預呈現 UI。 這在 *_Host.cshtml* Razor 頁面中設定:

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

`RenderMode`設定元件是否:

* 預置到頁面中。
* 在頁面上呈現為靜態 HTML,或者如果它包含從使用者代理引導 Blazor 應用的必要資訊。

| `RenderMode`        | 描述 |
| ------------------- | ----------- |
| `ServerPrerendered` | 將元件呈現為靜態 HTML,並包含伺服器應用的Blazor標記。 當使用者代理啟動時,此標記用於引導Blazor應用。 |
| `Server`            | 渲染伺服器應用的Blazor標記。 不包括元件的輸出。 當使用者代理啟動時,此標記用於引導Blazor應用。 |
| `Static`            | 將元件呈現為靜態 HTML。 |

不支援從靜態 HTML 頁呈現伺服器元件。

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a>從 Razor 頁面與檢視成有狀態的互動式元件

可有狀態的互動式元件可以添加到 Razor 頁面或檢視中。

當頁面或檢視呈現時:

* 元件預呈現為頁面或視圖。
* 用於預渲染的初始元件狀態將丟失。
* 建立連接時,將創建新的SignalR元件狀態。

以下 Razor 頁面`Counter`呈現元件:

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a>從 Razor 頁面與檢視呈現非互動式元件

在以下 Razor`Counter`頁中 ,元件使用使用表單指定的初始值靜態呈現:

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

由於`MyComponent`是靜態呈現的,因此元件不能是互動式的。

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a>為SignalRBlazor伺服器應用設定客戶端

有時,您需要配置SignalRBlazor伺服器應用使用的用戶端。 例如,您可能希望在用戶端上SignalR配置日誌記錄以診斷連接問題。

要在SignalR*頁面/_Host.cshtml*檔中設定客戶端,請進行以下分析:

* 向`autostart="false"``blazor.server.js`文稿的`<script>`標記添加屬性。
* 呼叫`Blazor.start`並傳遞指定生成器的SignalR配置物件。

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
