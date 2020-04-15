---
title: 將ASP.NET核心剃刀元件整合到剃刀頁面和 MVC 應用中
author: guardrex
description: 瞭解Blazor應用中元件和 DOM 元素的數據繫結方案。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/14/2020
no-loc:
- Blazor
- SignalR
uid: blazor/integrate-components
ms.openlocfilehash: c242fbef70d289929d5c005abc0aa431619862b3
ms.sourcegitcommit: f29a12486313e38e0163a643d8a97c8cecc7e871
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/14/2020
ms.locfileid: "81383961"
---
# <a name="integrate-aspnet-core-razor-components-into-razor-pages-and-mvc-apps"></a>將ASP.NET核心剃刀元件整合到剃刀頁面和 MVC 應用中

由[盧克·萊瑟姆](https://github.com/guardrex)和[丹尼爾·羅斯](https://github.com/danroth27)

剃刀元件可以集成到剃刀頁面和 MVC 應用中。 呈現頁面或視圖時,可以同時預呈現元件。

[準備應用](#prepare-the-app)后,根據應用的要求,在以下部分中使用指南:

* 可路由元件&ndash;用於直接從使用者請求路由的元件。 當訪問者應該能夠在其瀏覽器中對帶有指令的[`@page`](xref:mvc/views/razor#page)元件發出 HTTP 請求時,請遵循本指南。
  * [在 Razor 頁面套用使用可路由元件](#use-routable-components-in-a-razor-pages-app)
  * [在 MVC 應用程式使用可路由元件](#use-routable-components-in-an-mvc-app)
* [從頁面或檢視](#render-components-from-a-page-or-view)&ndash;呈現元件 對於不能直接從使用者請求路由的元件。 當應用使用[元件標記説明器](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper)將元件嵌入到現有頁面和視圖中時,請遵循本指南。

## <a name="prepare-the-app"></a>準備應用程式

現有的 Razor 頁面或 MVC 應用可以將 Razor 元件整合到頁面和檢視中:

1. 在應用程式的佈局檔中 *(_Layout.cshtml*):

   * 將以下`<base>`標籤加入`<head>`元素:

     ```html
     <base href="~/" />
     ```

     上`href`例中的值(*應用基本路徑*)假定應用駐留在根`/`URL 路徑 ( )。 如果應用是子應用程式,請按照<xref:host-and-deploy/blazor/index#app-base-path>本文的 *"應用基本路徑*"部分中的指南操作。

     *_Layout.cshtml*檔案位於 Razor 頁面應用中的 *「頁面/共用」* 資料夾中,或 MVC 應用中*的「視圖/共用*」資料夾中。

   * 在`<script>``</body>`結束 標記之前為*blazor.server.js*文稿加入標記:

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     框架將*blazor.server.js*文稿添加到應用程式。 無需手動將腳本添加到應用。

1. 將 *_Imports.razor*檔案新增到具有以下內容的項目根資料夾中(將姓`MyAppNamespace`氏空間 更改為應用的命名空間):

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

1. 在`Startup.ConfigureServices`中,註冊 Blazor 伺服器服務:

   ```csharp
   services.AddServerSideBlazor();
   ```

1. 在`Startup.Configure`中,將 Blazor`app.UseEndpoints`中心終結點加入 :

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. 將元件整合到任何頁面或檢視中。 有關詳細資訊,請參閱[頁面或檢視部分的「渲染」元件](#render-components-from-a-page-or-view)。

## <a name="use-routable-components-in-a-razor-pages-app"></a>在 Razor 頁面套用使用可路由元件

*本節涉及添加直接從使用者請求中可路由的元件。*

要在 Razor 頁面應用中支援可路由的剃刀元件,可以:

1. 依「[準備應用程式」 部分的指南進行操作](#prepare-the-app)。

1. 將*App.razor*檔案新增到專案根,包含以下內容:

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

1. 將 *_Host.cshtml*檔案加入到包含以下內容的 *「頁面」* 資料夾:

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   元件使用共用 *_Layout.cshtml*文件進行佈局。

   <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode>設定`App`元件是否:

   * 預置到頁面中。
   * 在頁面上呈現為靜態 HTML,或者如果它包含從使用者代理引導 Blazor 應用的必要資訊。

   | 成像模式 | 描述 |
   | ----------- | ----------- |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | 將`App`元件呈現為靜態 HTML,並包含 Blazor Server 應用的標記。 當使用者代理啟動時,此標記用於引導 Blazor 應用。 |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | 渲染 Blazor 伺服器應用的標記。 不包括元件的`App`輸出。 當使用者代理啟動時,此標記用於引導 Blazor 應用。 |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | 將`App`元件呈現為靜態 HTML。 |

   有關元件標記說明程式的詳細資訊,請參閱<xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>。

1. 將 *_Host.cshtml*頁的低優先權路由新增到 中的終結`Startup.Configure`點設定:

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. 向應用添加可路由元件。 例如：

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

有關命名空間的詳細資訊,請參閱[元件命名空間](#component-namespaces)部分。

## <a name="use-routable-components-in-an-mvc-app"></a>在 MVC 應用程式使用可路由元件

*本節涉及添加直接從使用者請求中可路由的元件。*

要在MVC應用中支援可路由的剃刀元件,

1. 依「[準備應用程式」 部分的指南進行操作](#prepare-the-app)。

1. 將*App.razor*檔案新增到專案的根目錄,包含以下內容:

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

1. 將 *_Host.cshtml*檔案加入以下內容的*檢視/主頁*資料夾:

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   元件使用共用 *_Layout.cshtml*文件進行佈局。
   
   <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode>設定`App`元件是否:

   * 預置到頁面中。
   * 在頁面上呈現為靜態 HTML,或者如果它包含從使用者代理引導 Blazor 應用的必要資訊。

   | 成像模式 | 描述 |
   | ----------- | ----------- |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | 將`App`元件呈現為靜態 HTML,並包含伺服器應用Blazor的標記。 當使用者代理啟動時,此標記用於引導Blazor應用。 |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | 渲染伺服器應用的Blazor標記。 不包括元件的`App`輸出。 當使用者代理啟動時,此標記用於引導Blazor應用。 |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | 將`App`元件呈現為靜態 HTML。 |

   有關元件標記說明程式的詳細資訊,請參閱<xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>。

1. 新增控制器

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. 為控制器操作新增低優先權路由,將 *_Host.cshtml*檢視傳回`Startup.Configure`到 中的終結點設定:

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. 建立 *「主頁」* 資料夾並將可路由元件添加到應用。 例如：

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

有關命名空間的詳細資訊,請參閱[元件命名空間](#component-namespaces)部分。

## <a name="render-components-from-a-page-or-view"></a>從頁面或檢視成元件

*本節涉及向頁面或視圖添加元件,其中元件不能直接從使用者請求中路由。*

要從頁面或檢視呈現元件,請使用[元件標記說明程式](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper)。

有關如何呈現元件、元件狀態和`Component`標記説明程式的詳細資訊,請參閱以下文章:

* <xref:blazor/hosting-models>
* <xref:blazor/hosting-model-configuration>
* <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>

## <a name="component-namespaces"></a>元件命名空間

使用自訂資料夾保存應用元件時,將表示資料夾的命名空間添加到頁面/視圖或 *_ViewImports.cshtml*檔中。 在下例中︰

* 更改為`MyAppNamespace`應用的命名空間。
* 如果名為 *「元件」* 的資料夾不用於儲存元件,則更改為`Components`元件所在的資料夾。

```cshtml
@using MyAppNamespace.Components
```

*_ViewImports.cshtml*檔案位於 Razor Pages 應用的 *「頁面」* 資料夾或 MVC 應用的 *「檢視」* 資料夾中。

如需詳細資訊，請參閱 <xref:blazor/components#import-components>。
