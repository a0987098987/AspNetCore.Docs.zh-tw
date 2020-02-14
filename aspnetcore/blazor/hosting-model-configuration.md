---
title: ASP.NET Core Blazor 裝載模型設定
author: guardrex
description: 深入瞭解 Blazor 裝載模型設定，包括如何將 Razor 元件整合到 Razor Pages 和 MVC 應用程式中。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/04/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: 91dd1aa32bb5165845eb4794377a50ccbd01adc3
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/14/2020
ms.locfileid: "77215057"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a>ASP.NET Core Blazor 裝載模型設定

依[Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

本文涵蓋主控模型設定。

<!-- For future use:

## Blazor WebAssembly

-->

## <a name="blazor-server"></a>Blazor 伺服器

### <a name="integrate-razor-components-into-razor-pages-and-mvc-apps"></a>將 Razor 元件整合到 Razor Pages 和 MVC 應用程式中

#### <a name="use-components-in-pages-and-views"></a>使用頁面和視圖中的元件

現有的 Razor Pages 或 MVC 應用程式可以將 Razor 元件整合至頁面和 views：

1. 在應用程式的版面配置檔案（ *_Layout. cshtml*）中：

   * 將下列 `<base>` 標記新增至 `<head>` 元素：

     ```html
     <base href="~/" />
     ```

     上述範例中的 `href` 值（*應用程式基底路徑*）假設應用程式位於根 URL 路徑（`/`）。 如果應用程式是子應用程式，請遵循 <xref:host-and-deploy/blazor/index#app-base-path> 文章的*應用程式基底路徑*一節中的指導方針。

     *_Layout. cshtml*檔案位於 MVC 應用程式的 Razor Pages 應用程式或*Views/shared*資料夾中的*Pages/shared*資料夾內。

   * 在結尾 `</body>` 標記之前，新增*blazor*的 `<script>` 標記：

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     架構會將*blazor*新增至應用程式。 不需要手動將腳本新增至應用程式。

1. 使用下列內容，將 *_Imports razor*檔案新增至專案的根資料夾（將最後一個命名空間 `MyAppNamespace`變更為應用程式的命名空間）：

   ```csharp
   @using System.Net.Http
   @using Microsoft.AspNetCore.Authorization
   @using Microsoft.AspNetCore.Components.Authorization
   @using Microsoft.AspNetCore.Components.Forms
   @using Microsoft.AspNetCore.Components.Routing
   @using Microsoft.AspNetCore.Components.Web
   @using Microsoft.JSInterop
   @using MyAppNamespace
   ```

1. 在 `Startup.ConfigureServices`中，註冊 Blazor 伺服器服務：

   ```csharp
   services.AddServerSideBlazor();
   ```

1. 在 `Startup.Configure`中，將 Blazor 中樞端點新增至 `app.UseEndpoints`：

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. 將元件整合到任何頁面或視圖中。 如需詳細資訊，請參閱 <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps> 文章的將*元件整合到 Razor Pages 和 MVC 應用程式*一節。

#### <a name="use-routable-components-in-a-razor-pages-app"></a>在 Razor Pages 應用程式中使用可路由的元件

在 Razor Pages 應用程式中支援可路由的 Razor 元件：

1. 請遵循[使用頁面和 views 中的元件](#use-components-in-pages-and-views)一節中的指導方針。

1. 使用下列內容，將*應用程式 razor*檔案新增至專案的根目錄：

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. 將 *_Host. cshtml*檔案新增至*Pages*資料夾，其中包含下列內容：

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   元件會使用共用的 *_Layout. cshtml*檔案作為其版面配置。

1. 在 `Startup.Configure`中，將 *_Host. cshtml*頁面的低優先順序路由新增至端點設定：

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. 將可路由的元件新增至應用程式。 例如：

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   使用自訂資料夾來保存應用程式的元件時，請將代表資料夾的命名空間新增至*Pages/_ViewImports. cshtml*檔案。 如需詳細資訊，請參閱 <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>。

#### <a name="use-routable-components-in-an-mvc-app"></a>在 MVC 應用程式中使用可路由的元件

在 MVC 應用程式中支援可路由的 Razor 元件：

1. 請遵循[使用頁面和 views 中的元件](#use-components-in-pages-and-views)一節中的指導方針。

1. 使用下列內容，將*應用程式 razor*檔案新增至專案的根目錄：

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. 使用下列內容，將 *_Host. cshtml*檔案新增至*Views/Home*資料夾：

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   元件會使用共用的 *_Layout. cshtml*檔案作為其版面配置。

1. 將動作新增至主控制器：

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. 為控制器動作新增低優先順序的路由，以將 *_Host. cshtml*視圖傳回 `Startup.Configure`中的端點設定：

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. 建立*Pages*資料夾，並將可路由的元件新增至應用程式。 例如：

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   使用自訂資料夾來存放應用程式的元件時，請將代表資料夾的命名空間新增至*Views/_ViewImports. cshtml*檔案。 如需詳細資訊，請參閱 <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>。

### <a name="reflect-the-connection-state-in-the-ui"></a>反映 UI 中的連接狀態

當用戶端偵測到連線已遺失時，會在用戶端嘗試重新連線時，向使用者顯示預設的 UI。 如果重新連線失敗，則會提供使用者重試的選項。

若要自訂 UI，請在 *_Host*的 [Razor 頁面] `<body>` 中，定義具有 `components-reconnect-modal` `id` 的元素：

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

下表描述套用至 `components-reconnect-modal` 元素的 CSS 類別。

| CSS 類別                       | 表示&hellip; |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | 遺失的連接。 用戶端正在嘗試重新連線。 顯示強制回應。 |
| `components-reconnect-hide`     | 系統會將使用中的連接重新建立至伺服器。 隱藏強制回應。 |
| `components-reconnect-failed`   | 重新連接失敗，可能是因為網路失敗。 若要嘗試重新連接，請呼叫 `window.Blazor.reconnect()`。 |
| `components-reconnect-rejected` | 已拒絕重新連接。 已達到伺服器但拒絕連線，而且伺服器上的使用者狀態遺失。 若要重載應用程式，請呼叫 `location.reload()`。 當下列情況發生時，可能會產生此連接狀態：<ul><li>伺服器端線路發生損毀。</li><li>用戶端中斷連線的時間夠長，伺服器才會捨棄使用者的狀態。 系統會處置與使用者互動之元件的實例。</li><li>伺服器會重新開機，或回收應用程式的背景工作進程。</li></ul> |

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

`RenderMode` 設定元件是否：

* 會資源清單到頁面中。
* 會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動 Blazor 應用程式所需的資訊。

| `RenderMode`        | 描述 |
| ------------------- | ----------- |
| `ServerPrerendered` | 將元件轉譯為靜態 HTML，並包含 Blazor 伺服器應用程式的標記。 當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。 |
| `Server`            | 呈現 Blazor 伺服器應用程式的標記。 不包含來自元件的輸出。 當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。 |
| `Static`            | 將元件轉譯為靜態 HTML。 |

不支援從靜態 HTML 網頁轉譯伺服器元件。

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a>從 Razor 頁面和 views 轉譯具狀態的互動式元件

可設定狀態的互動式元件可以新增至 Razor 頁面或視圖。

當頁面或視圖呈現時：

* 此元件是使用頁面或視圖所資源清單。
* 用來進行預呈現的初始元件狀態會遺失。
* 建立 SignalR 連接時，會建立新的元件狀態。

下列 Razor 頁面會呈現 `Counter` 元件：

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

在下列 Razor 頁面中，`Counter` 元件會以靜態方式轉譯，並使用以表單指定的初始值：

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

由於 `MyComponent` 是以靜態方式呈現，因此元件不能是互動式的。

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a>設定 Blazor 伺服器應用程式的 SignalR 用戶端

有時候，您需要設定 Blazor 伺服器應用程式所使用的 SignalR 用戶端。 例如，您可能會想要在 SignalR 用戶端上設定記錄，以診斷連接問題。

若要設定*Pages/_Host. cshtml*檔案中的 SignalR 用戶端：

* 將 `autostart="false"` 屬性加入至 `blazor.server.js` 腳本的 `<script>` 標記。
* 呼叫 `Blazor.start` 並傳入指定 SignalR 產生器的設定物件。

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
