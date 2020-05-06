---
title: 將 ASP.NET Core Razor元件整合Razor至頁面和 MVC 應用程式
author: guardrex
description: 瞭解應用程式中Blazor元件和 DOM 元素的資料系結案例。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/25/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/integrate-components
ms.openlocfilehash: eb4378223c40594ac52f50b7b890785067515555
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82771770"
---
# <a name="integrate-aspnet-core-razor-components-into-razor-pages-and-mvc-apps"></a>將 ASP.NET Core Razor 元件整合至 Razor Pages 和 MVC 應用程式

By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)

Razor 元件可以整合到 Razor Pages 和 MVC 應用程式中。 當頁面或視圖呈現時，可以同時資源清單元件。

[準備好應用程式](#prepare-the-app)之後，請根據應用程式的需求，使用下列各節中的指導方針：

* 可路由&ndash;的元件，適用于直接從使用者要求路由傳送的元件。 當訪客應該能夠在其瀏覽器中針對具有指示詞的元件提出 HTTP 要求時， [`@page`](xref:mvc/views/razor#page)請遵循此指導方針。
  * [在 Razor Pages 應用程式中使用可路由的元件](#use-routable-components-in-a-razor-pages-app)
  * [在 MVC 應用程式中使用可路由的元件](#use-routable-components-in-an-mvc-app)
* 針對無法直接從使用者要求路由傳送的元件，[呈現頁面或視圖](#render-components-from-a-page-or-view) &ndash;中的元件。 當應用程式使用[元件標記](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper)協助程式將元件內嵌至現有頁面和視圖時，請遵循此指導方針。

## <a name="prepare-the-app"></a>準備應用程式

現有的 Razor Pages 或 MVC 應用程式可以將 Razor 元件整合至頁面和 views：

1. 在應用程式的版面配置檔案（*_Layout. cshtml*）中：

   * 將下列`<base>`標記新增至`<head>`元素：

     ```html
     <base href="~/" />
     ```

     上述`href`範例中的值（*應用程式基底路徑*）假設應用程式位於根 URL 路徑（`/`）。 如果應用程式是子應用程式，請遵循<xref:host-and-deploy/blazor/index#app-base-path>文章的*應用程式基底路徑*一節中的指導方針。

     *_Layout. cshtml*檔案位於 MVC 應用程式的 Razor Pages 應用程式或*Views/shared*資料夾中的*Pages/shared*資料夾內。

   * 緊接在`<script>`結尾`</body>`標記之前，新增*blazor*的標記：

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     架構會將*blazor*新增至應用程式。 不需要手動將腳本新增至應用程式。

1. 使用下列內容，將 *_Imports razor*檔案加入至專案的根資料夾（將最後一個命名空間`MyAppNamespace`變更為應用程式的命名空間）：

   ```razor
   @using System.Net.Http
   @using Microsoft.AspNetCore.Authorization
   @using Microsoft.AspNetCore.Components.Authorization
   @using Microsoft.AspNetCore.Components.Forms
   @using Microsoft.AspNetCore.Components.Routing
   @using Microsoft.AspNetCore.Components.Web
   @using Microsoft.JSInterop
   @using MyAppNamespace
   ```

1. 在`Startup.ConfigureServices`中，註冊 Blazor 伺服器服務：

   ```csharp
   services.AddServerSideBlazor();
   ```

1. 在`Startup.Configure`中，將 Blazor 中樞端點新增`app.UseEndpoints`至：

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. 將元件整合到任何頁面或視圖中。 如需詳細資訊，請參閱[從頁面或視圖呈現元件](#render-components-from-a-page-or-view)一節。

## <a name="use-routable-components-in-a-razor-pages-app"></a>在 Razor Pages 應用程式中使用可路由的元件

*本節適用于新增可直接從使用者要求路由傳送的元件。*

在 Razor Pages 應用程式中支援可路由的 Razor 元件：

1. 請遵循[準備應用程式](#prepare-the-app)一節中的指導方針。

1. 使用下列內容將*應用程式 razor*檔案新增至專案根目錄：

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

   <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode>設定`App`元件是否：

   * 會資源清單到頁面中。
   * 會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動 Blazor 應用程式所需的資訊。

   | 轉譯模式 | 說明 |
   | ----------- | ----------- |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | 將`App`元件轉譯為靜態 HTML，並包含 Blazor 伺服器應用程式的標記。 當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。 |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | 呈現 Blazor 伺服器應用程式的標記。 不包含來自`App`元件的輸出。 當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。 |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | 將`App`元件轉譯為靜態 HTML。 |

   如需元件標記協助程式的詳細資訊， <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>請參閱。

1. 在中`Startup.Configure`，將 *_Host. cshtml*頁面的低優先順序路由新增至端點設定：

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

如需命名空間的詳細資訊，請參閱[元件命名空間](#component-namespaces)一節。

## <a name="use-routable-components-in-an-mvc-app"></a>在 MVC 應用程式中使用可路由的元件

*本節適用于新增可直接從使用者要求路由傳送的元件。*

在 MVC 應用程式中支援可路由的 Razor 元件：

1. 請遵循[準備應用程式](#prepare-the-app)一節中的指導方針。

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
   
   <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode>設定`App`元件是否：

   * 會資源清單到頁面中。
   * 會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動 Blazor 應用程式所需的資訊。

   | 轉譯模式 | 說明 |
   | ----------- | ----------- |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | 將`App`元件轉譯為靜態 HTML，並包含Blazor伺服器應用程式的標記。 當使用者代理程式啟動時，會使用此標記來啟動Blazor應用程式。 |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | 呈現Blazor伺服器應用程式的標記。 不包含來自`App`元件的輸出。 當使用者代理程式啟動時，會使用此標記來啟動Blazor應用程式。 |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | 將`App`元件轉譯為靜態 HTML。 |

   如需元件標記協助程式的詳細資訊， <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>請參閱。

1. 將動作新增至主控制器：

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. 為控制器動作新增低優先順序的路由，以將 *_Host. cshtml*視圖傳回至中`Startup.Configure`的端點設定：

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

如需命名空間的詳細資訊，請參閱[元件命名空間](#component-namespaces)一節。

## <a name="render-components-from-a-page-or-view"></a>從頁面或視圖呈現元件

*本節適用于將元件新增至頁面或視圖，其中元件無法直接從使用者要求路由傳送。*

若要從頁面或視圖呈現元件，請使用[元件標記](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper)協助程式。

### <a name="render-stateful-interactive-components"></a>呈現具狀態的互動式元件

可設定狀態的Razor互動式元件可以新增至頁面或視圖。

當頁面或視圖呈現時：

* 此元件是使用頁面或視圖所資源清單。
* 用來進行預呈現的初始元件狀態會遺失。
* 建立連接時，會建立新SignalR的元件狀態。

下列Razor頁面會呈現`Counter`元件：

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@functions {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

如需詳細資訊，請參閱<xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>。

### <a name="render-noninteractive-components"></a>呈現非互動式元件

在下列Razor頁面中， `Counter`元件會使用以表單指定的初始值，以靜態方式呈現。 由於元件是以靜態方式呈現，因此元件不是互動式的：

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

<component type="typeof(Counter)" render-mode="Static" 
    param-InitialValue="InitialValue" />

@functions {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

如需詳細資訊，請參閱<xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>。

## <a name="component-namespaces"></a>元件命名空間

使用自訂資料夾來存放應用程式的元件時，請將代表資料夾的命名空間新增至頁面/視圖，或加入至 *_ViewImports. cshtml*檔案。 在下例中︰

* 變更`MyAppNamespace`為應用程式的命名空間。
* 如果未使用名為「*元件*」的資料夾來存放元件`Components` ，請變更為元件所在的資料夾。

```cshtml
@using MyAppNamespace.Components
```

*_ViewImports. cshtml*檔案位於 MVC 應用程式的*Pages* Razor pages 應用程式或*Views*資料夾的 pages 資料夾中。

如需詳細資訊，請參閱<xref:blazor/components#import-components>。
