---
title: 將 ASP.NET Core Razor 元件整合至 Razor Pages 和 MVC 應用程式
author: guardrex
description: 瞭解 Blazor 應用程式中元件和 DOM 元素的資料系結案例。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/17/2020
no-loc:
- Blazor
- SignalR
uid: blazor/integrate-components
ms.openlocfilehash: cf6056e0985d5433bddecac8dd183ca3f4c2af5b
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218930"
---
# <a name="integrate-aspnet-core-razor-components-into-razor-pages-and-mvc-apps"></a>將 ASP.NET Core Razor 元件整合至 Razor Pages 和 MVC 應用程式

By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)

Razor 元件可以整合到 Razor Pages 和 MVC 應用程式中。 當頁面或視圖呈現時，可以同時資源清單元件。

## <a name="prepare-the-app-to-use-components-in-pages-and-views"></a>準備應用程式以使用頁面和視圖中的元件

現有的 Razor Pages 或 MVC 應用程式可以將 Razor 元件整合至頁面和 views：

1. 在應用程式的版面配置檔案（ *_Layout. cshtml*）中：

   * 將下列 `<base>` 標記新增至 `<head>` 元素：

     ```html
     <base href="~/" />
     ```

     上述範例中的 `href` 值（*應用程式基底路徑*）假設應用程式位於根 URL 路徑（`/`）。 如果應用程式是子應用程式，請遵循 <xref:host-and-deploy/blazor/index#app-base-path> 文章的*應用程式基底路徑*一節中的指導方針。

     *_Layout. cshtml*檔案位於 MVC 應用程式的 Razor Pages 應用程式或*Views/shared*資料夾中的*Pages/shared*資料夾內。

   * 緊接在結尾 `</body>` 標記之前，新增*blazor*的 `<script>` 標籤：

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     架構會將*blazor*新增至應用程式。 不需要手動將腳本新增至應用程式。

1. 使用下列內容，將 *_Imports razor*檔案新增至專案的根資料夾（將最後一個命名空間 `MyAppNamespace`變更為應用程式的命名空間）：

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

1. 在 `Startup.ConfigureServices`中，註冊 Blazor 伺服器服務：

   ```csharp
   services.AddServerSideBlazor();
   ```

1. 在 `Startup.Configure`中，將 Blazor 中樞端點新增至 `app.UseEndpoints`：

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. 將元件整合到任何頁面或視圖中。 如需詳細資訊，請參閱[從頁面或視圖呈現元件](#render-components-from-a-page-or-view)一節。

## <a name="use-routable-components-in-a-razor-pages-app"></a>在 Razor Pages 應用程式中使用可路由的元件

*本節適用于新增可直接從使用者要求路由傳送的元件。*

在 Razor Pages 應用程式中支援可路由的 Razor 元件：

1. 遵循[準備應用程式以使用頁面和 views 中的元件](#prepare-the-app-to-use-components-in-pages-and-views)一節中的指導方針。

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

   如需命名空間的詳細資訊，請參閱[元件命名空間](#component-namespaces)一節。

## <a name="use-routable-components-in-an-mvc-app"></a>在 MVC 應用程式中使用可路由的元件

*本節適用于新增可直接從使用者要求路由傳送的元件。*

在 MVC 應用程式中支援可路由的 Razor 元件：

1. 遵循[準備應用程式以使用頁面和 views 中的元件](#prepare-the-app-to-use-components-in-pages-and-views)一節中的指導方針。

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

   如需命名空間的詳細資訊，請參閱[元件命名空間](#component-namespaces)一節。

## <a name="component-namespaces"></a>元件命名空間

使用自訂資料夾來存放應用程式的元件時，請將代表資料夾的命名空間新增至頁面/視圖，或加入至 *_ViewImports. cshtml*檔案。 在下例中︰

* 將 `MyAppNamespace` 變更為應用程式的命名空間。
* 如果未使用名為「*元件*」的資料夾來存放元件，請將 `Components` 變更為元件所在的資料夾。

```cshtml
@using MyAppNamespace.Components
```

*_ViewImports. cshtml*檔案位於 MVC 應用程式的 Razor Pages 應用程式或*Views*資料夾的*Pages*資料夾中。

如需詳細資訊，請參閱 <xref:blazor/components#import-components>。

## <a name="render-components-from-a-page-or-view"></a>從頁面或視圖呈現元件

*本節適用于將元件新增至頁面或視圖，其中元件無法直接從使用者要求路由傳送。*

若要從頁面或視圖呈現元件，請使用[元件標記](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper)協助程式。

如需有關如何呈現元件、元件狀態和 `Component` 標籤協助程式的詳細資訊，請參閱下列文章：

* <xref:blazor/hosting-models>
* <xref:blazor/hosting-model-configuration>
* <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>
