---
title: 將 ASP.NET Core Razor 元件整合至 Razor Pages 和 MVC 應用程式
author: guardrex
description: 瞭解應用程式中Blazor元件和 DOM 元素的資料系結案例。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/25/2020
no-loc:
- Blazor
- SignalR
uid: blazor/integrate-components
ms.openlocfilehash: 282f96a4198d20b7fa38c186721b48ace5219ca6
ms.sourcegitcommit: c6f5ea6397af2dd202632cf2be66fc30f3357bcc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/26/2020
ms.locfileid: "82159602"
---
# <a name="integrate-aspnet-core-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="b96fa-103">將 ASP.NET Core Razor 元件整合至 Razor Pages 和 MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="b96fa-103">Integrate ASP.NET Core Razor components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="b96fa-104">By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="b96fa-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="b96fa-105">Razor 元件可以整合到 Razor Pages 和 MVC 應用程式中。</span><span class="sxs-lookup"><span data-stu-id="b96fa-105">Razor components can be integrated into Razor Pages and MVC apps.</span></span> <span data-ttu-id="b96fa-106">當頁面或視圖呈現時，可以同時資源清單元件。</span><span class="sxs-lookup"><span data-stu-id="b96fa-106">When the page or view is rendered, components can be prerendered at the same time.</span></span>

<span data-ttu-id="b96fa-107">[準備好應用程式](#prepare-the-app)之後，請根據應用程式的需求，使用下列各節中的指導方針：</span><span class="sxs-lookup"><span data-stu-id="b96fa-107">After [preparing the app](#prepare-the-app), use the guidance in the following sections depending on the app's requirements:</span></span>

* <span data-ttu-id="b96fa-108">可路由&ndash;的元件，適用于直接從使用者要求路由傳送的元件。</span><span class="sxs-lookup"><span data-stu-id="b96fa-108">Routable components &ndash; For components that are directly routable from user requests.</span></span> <span data-ttu-id="b96fa-109">當訪客應該能夠在其瀏覽器中針對具有指示詞的元件提出 HTTP 要求時， [`@page`](xref:mvc/views/razor#page)請遵循此指導方針。</span><span class="sxs-lookup"><span data-stu-id="b96fa-109">Follow this guidance when visitors should be able to make an HTTP request in their browser for a component with an [`@page`](xref:mvc/views/razor#page) directive.</span></span>
  * [<span data-ttu-id="b96fa-110">在 Razor Pages 應用程式中使用可路由的元件</span><span class="sxs-lookup"><span data-stu-id="b96fa-110">Use routable components in a Razor Pages app</span></span>](#use-routable-components-in-a-razor-pages-app)
  * [<span data-ttu-id="b96fa-111">在 MVC 應用程式中使用可路由的元件</span><span class="sxs-lookup"><span data-stu-id="b96fa-111">Use routable components in an MVC app</span></span>](#use-routable-components-in-an-mvc-app)
* <span data-ttu-id="b96fa-112">針對無法直接從使用者要求路由傳送的元件，[呈現頁面或視圖](#render-components-from-a-page-or-view) &ndash;中的元件。</span><span class="sxs-lookup"><span data-stu-id="b96fa-112">[Render components from a page or view](#render-components-from-a-page-or-view) &ndash; For components that aren't directly routable from user requests.</span></span> <span data-ttu-id="b96fa-113">當應用程式使用[元件標記](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper)協助程式將元件內嵌至現有頁面和視圖時，請遵循此指導方針。</span><span class="sxs-lookup"><span data-stu-id="b96fa-113">Follow this guidance when the app embeds components into existing pages and views with the [Component Tag Helper](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper).</span></span>

## <a name="prepare-the-app"></a><span data-ttu-id="b96fa-114">準備應用程式</span><span class="sxs-lookup"><span data-stu-id="b96fa-114">Prepare the app</span></span>

<span data-ttu-id="b96fa-115">現有的 Razor Pages 或 MVC 應用程式可以將 Razor 元件整合至頁面和 views：</span><span class="sxs-lookup"><span data-stu-id="b96fa-115">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="b96fa-116">在應用程式的版面配置檔案（*_Layout. cshtml*）中：</span><span class="sxs-lookup"><span data-stu-id="b96fa-116">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="b96fa-117">將下列`<base>`標記新增至`<head>`元素：</span><span class="sxs-lookup"><span data-stu-id="b96fa-117">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="b96fa-118">上述`href`範例中的值（*應用程式基底路徑*）假設應用程式位於根 URL 路徑（`/`）。</span><span class="sxs-lookup"><span data-stu-id="b96fa-118">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="b96fa-119">如果應用程式是子應用程式，請遵循<xref:host-and-deploy/blazor/index#app-base-path>文章的*應用程式基底路徑*一節中的指導方針。</span><span class="sxs-lookup"><span data-stu-id="b96fa-119">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="b96fa-120">*_Layout. cshtml*檔案位於 MVC 應用程式的 Razor Pages 應用程式或*Views/shared*資料夾中的*Pages/shared*資料夾內。</span><span class="sxs-lookup"><span data-stu-id="b96fa-120">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="b96fa-121">緊接在`<script>`結尾`</body>`標記之前，新增*blazor*的標記：</span><span class="sxs-lookup"><span data-stu-id="b96fa-121">Add a `<script>` tag for the *blazor.server.js* script immediately before of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="b96fa-122">架構會將*blazor*新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="b96fa-122">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="b96fa-123">不需要手動將腳本新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="b96fa-123">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="b96fa-124">使用下列內容，將 *_Imports razor*檔案加入至專案的根資料夾（將最後一個命名空間`MyAppNamespace`變更為應用程式的命名空間）：</span><span class="sxs-lookup"><span data-stu-id="b96fa-124">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

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

1. <span data-ttu-id="b96fa-125">在`Startup.ConfigureServices`中，註冊 Blazor 伺服器服務：</span><span class="sxs-lookup"><span data-stu-id="b96fa-125">In `Startup.ConfigureServices`, register the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="b96fa-126">在`Startup.Configure`中，將 Blazor 中樞端點新增`app.UseEndpoints`至：</span><span class="sxs-lookup"><span data-stu-id="b96fa-126">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="b96fa-127">將元件整合到任何頁面或視圖中。</span><span class="sxs-lookup"><span data-stu-id="b96fa-127">Integrate components into any page or view.</span></span> <span data-ttu-id="b96fa-128">如需詳細資訊，請參閱[從頁面或視圖呈現元件](#render-components-from-a-page-or-view)一節。</span><span class="sxs-lookup"><span data-stu-id="b96fa-128">For more information, see the [Render components from a page or view](#render-components-from-a-page-or-view) section.</span></span>

## <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="b96fa-129">在 Razor Pages 應用程式中使用可路由的元件</span><span class="sxs-lookup"><span data-stu-id="b96fa-129">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="b96fa-130">*本節適用于新增可直接從使用者要求路由傳送的元件。*</span><span class="sxs-lookup"><span data-stu-id="b96fa-130">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="b96fa-131">在 Razor Pages 應用程式中支援可路由的 Razor 元件：</span><span class="sxs-lookup"><span data-stu-id="b96fa-131">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="b96fa-132">請遵循[準備應用程式](#prepare-the-app)一節中的指導方針。</span><span class="sxs-lookup"><span data-stu-id="b96fa-132">Follow the guidance in the [Prepare the app](#prepare-the-app) section.</span></span>

1. <span data-ttu-id="b96fa-133">使用下列內容將*應用程式 razor*檔案新增至專案根目錄：</span><span class="sxs-lookup"><span data-stu-id="b96fa-133">Add an *App.razor* file to the project root with the following content:</span></span>

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

1. <span data-ttu-id="b96fa-134">將 *_Host. cshtml*檔案新增至*Pages*資料夾，其中包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="b96fa-134">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="b96fa-135">元件會使用共用的 *_Layout. cshtml*檔案作為其版面配置。</span><span class="sxs-lookup"><span data-stu-id="b96fa-135">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

   <span data-ttu-id="b96fa-136"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode>設定`App`元件是否：</span><span class="sxs-lookup"><span data-stu-id="b96fa-136"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configures whether the `App` component:</span></span>

   * <span data-ttu-id="b96fa-137">會資源清單到頁面中。</span><span class="sxs-lookup"><span data-stu-id="b96fa-137">Is prerendered into the page.</span></span>
   * <span data-ttu-id="b96fa-138">會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動 Blazor 應用程式所需的資訊。</span><span class="sxs-lookup"><span data-stu-id="b96fa-138">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

   | <span data-ttu-id="b96fa-139">轉譯模式</span><span class="sxs-lookup"><span data-stu-id="b96fa-139">Render Mode</span></span> | <span data-ttu-id="b96fa-140">描述</span><span class="sxs-lookup"><span data-stu-id="b96fa-140">Description</span></span> |
   | ----------- | ----------- |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | <span data-ttu-id="b96fa-141">將`App`元件轉譯為靜態 HTML，並包含 Blazor 伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="b96fa-141">Renders the `App` component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="b96fa-142">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b96fa-142">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | <span data-ttu-id="b96fa-143">呈現 Blazor 伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="b96fa-143">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="b96fa-144">不包含來自`App`元件的輸出。</span><span class="sxs-lookup"><span data-stu-id="b96fa-144">Output from the `App` component isn't included.</span></span> <span data-ttu-id="b96fa-145">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b96fa-145">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | <span data-ttu-id="b96fa-146">將`App`元件轉譯為靜態 HTML。</span><span class="sxs-lookup"><span data-stu-id="b96fa-146">Renders the `App` component into static HTML.</span></span> |

   <span data-ttu-id="b96fa-147">如需元件標記協助程式的詳細資訊， <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>請參閱。</span><span class="sxs-lookup"><span data-stu-id="b96fa-147">For more information on the Component Tag Helper, see <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>.</span></span>

1. <span data-ttu-id="b96fa-148">在中`Startup.Configure`，將 *_Host. cshtml*頁面的低優先順序路由新增至端點設定：</span><span class="sxs-lookup"><span data-stu-id="b96fa-148">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="b96fa-149">將可路由的元件新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="b96fa-149">Add routable components to the app.</span></span> <span data-ttu-id="b96fa-150">例如：</span><span class="sxs-lookup"><span data-stu-id="b96fa-150">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

<span data-ttu-id="b96fa-151">如需命名空間的詳細資訊，請參閱[元件命名空間](#component-namespaces)一節。</span><span class="sxs-lookup"><span data-stu-id="b96fa-151">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="b96fa-152">在 MVC 應用程式中使用可路由的元件</span><span class="sxs-lookup"><span data-stu-id="b96fa-152">Use routable components in an MVC app</span></span>

<span data-ttu-id="b96fa-153">*本節適用于新增可直接從使用者要求路由傳送的元件。*</span><span class="sxs-lookup"><span data-stu-id="b96fa-153">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="b96fa-154">在 MVC 應用程式中支援可路由的 Razor 元件：</span><span class="sxs-lookup"><span data-stu-id="b96fa-154">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="b96fa-155">請遵循[準備應用程式](#prepare-the-app)一節中的指導方針。</span><span class="sxs-lookup"><span data-stu-id="b96fa-155">Follow the guidance in the [Prepare the app](#prepare-the-app) section.</span></span>

1. <span data-ttu-id="b96fa-156">使用下列內容，將*應用程式 razor*檔案新增至專案的根目錄：</span><span class="sxs-lookup"><span data-stu-id="b96fa-156">Add an *App.razor* file to the root of the project with the following content:</span></span>

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

1. <span data-ttu-id="b96fa-157">使用下列內容，將 *_Host. cshtml*檔案新增至*Views/Home*資料夾：</span><span class="sxs-lookup"><span data-stu-id="b96fa-157">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="b96fa-158">元件會使用共用的 *_Layout. cshtml*檔案作為其版面配置。</span><span class="sxs-lookup"><span data-stu-id="b96fa-158">Components use the shared *_Layout.cshtml* file for their layout.</span></span>
   
   <span data-ttu-id="b96fa-159"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode>設定`App`元件是否：</span><span class="sxs-lookup"><span data-stu-id="b96fa-159"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configures whether the `App` component:</span></span>

   * <span data-ttu-id="b96fa-160">會資源清單到頁面中。</span><span class="sxs-lookup"><span data-stu-id="b96fa-160">Is prerendered into the page.</span></span>
   * <span data-ttu-id="b96fa-161">會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動 Blazor 應用程式所需的資訊。</span><span class="sxs-lookup"><span data-stu-id="b96fa-161">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

   | <span data-ttu-id="b96fa-162">轉譯模式</span><span class="sxs-lookup"><span data-stu-id="b96fa-162">Render Mode</span></span> | <span data-ttu-id="b96fa-163">描述</span><span class="sxs-lookup"><span data-stu-id="b96fa-163">Description</span></span> |
   | ----------- | ----------- |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | <span data-ttu-id="b96fa-164">將`App`元件轉譯為靜態 HTML，並包含Blazor伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="b96fa-164">Renders the `App` component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="b96fa-165">當使用者代理程式啟動時，會使用此標記來啟動Blazor應用程式。</span><span class="sxs-lookup"><span data-stu-id="b96fa-165">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | <span data-ttu-id="b96fa-166">呈現Blazor伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="b96fa-166">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="b96fa-167">不包含來自`App`元件的輸出。</span><span class="sxs-lookup"><span data-stu-id="b96fa-167">Output from the `App` component isn't included.</span></span> <span data-ttu-id="b96fa-168">當使用者代理程式啟動時，會使用此標記來啟動Blazor應用程式。</span><span class="sxs-lookup"><span data-stu-id="b96fa-168">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | <span data-ttu-id="b96fa-169">將`App`元件轉譯為靜態 HTML。</span><span class="sxs-lookup"><span data-stu-id="b96fa-169">Renders the `App` component into static HTML.</span></span> |

   <span data-ttu-id="b96fa-170">如需元件標記協助程式的詳細資訊， <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>請參閱。</span><span class="sxs-lookup"><span data-stu-id="b96fa-170">For more information on the Component Tag Helper, see <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>.</span></span>

1. <span data-ttu-id="b96fa-171">將動作新增至主控制器：</span><span class="sxs-lookup"><span data-stu-id="b96fa-171">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="b96fa-172">為控制器動作新增低優先順序的路由，以將 *_Host. cshtml*視圖傳回至中`Startup.Configure`的端點設定：</span><span class="sxs-lookup"><span data-stu-id="b96fa-172">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="b96fa-173">建立*Pages*資料夾，並將可路由的元件新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="b96fa-173">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="b96fa-174">例如：</span><span class="sxs-lookup"><span data-stu-id="b96fa-174">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

<span data-ttu-id="b96fa-175">如需命名空間的詳細資訊，請參閱[元件命名空間](#component-namespaces)一節。</span><span class="sxs-lookup"><span data-stu-id="b96fa-175">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="render-components-from-a-page-or-view"></a><span data-ttu-id="b96fa-176">從頁面或視圖呈現元件</span><span class="sxs-lookup"><span data-stu-id="b96fa-176">Render components from a page or view</span></span>

<span data-ttu-id="b96fa-177">*本節適用于將元件新增至頁面或視圖，其中元件無法直接從使用者要求路由傳送。*</span><span class="sxs-lookup"><span data-stu-id="b96fa-177">*This section pertains to adding components to pages or views, where the components aren't directly routable from user requests.*</span></span>

<span data-ttu-id="b96fa-178">若要從頁面或視圖呈現元件，請使用[元件標記](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper)協助程式。</span><span class="sxs-lookup"><span data-stu-id="b96fa-178">To render a component from a page or view, use the [Component Tag Helper](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper).</span></span>

### <a name="render-stateful-interactive-components"></a><span data-ttu-id="b96fa-179">呈現具狀態的互動式元件</span><span class="sxs-lookup"><span data-stu-id="b96fa-179">Render stateful interactive components</span></span>

<span data-ttu-id="b96fa-180">可設定狀態的互動式元件可以新增至 Razor 頁面或視圖。</span><span class="sxs-lookup"><span data-stu-id="b96fa-180">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="b96fa-181">當頁面或視圖呈現時：</span><span class="sxs-lookup"><span data-stu-id="b96fa-181">When the page or view renders:</span></span>

* <span data-ttu-id="b96fa-182">此元件是使用頁面或視圖所資源清單。</span><span class="sxs-lookup"><span data-stu-id="b96fa-182">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="b96fa-183">用來進行預呈現的初始元件狀態會遺失。</span><span class="sxs-lookup"><span data-stu-id="b96fa-183">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="b96fa-184">建立連接時，會建立新SignalR的元件狀態。</span><span class="sxs-lookup"><span data-stu-id="b96fa-184">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="b96fa-185">下列 Razor 頁面會呈現`Counter`元件：</span><span class="sxs-lookup"><span data-stu-id="b96fa-185">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@functions {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

<span data-ttu-id="b96fa-186">如需詳細資訊，請參閱 <xref:xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>。</span><span class="sxs-lookup"><span data-stu-id="b96fa-186">For more information, see <xref:xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>.</span></span>

### <a name="render-noninteractive-components"></a><span data-ttu-id="b96fa-187">呈現非互動式元件</span><span class="sxs-lookup"><span data-stu-id="b96fa-187">Render noninteractive components</span></span>

<span data-ttu-id="b96fa-188">在下列 Razor 頁面中， `Counter`元件會以靜態方式轉譯，並具有使用表單指定的初始值。</span><span class="sxs-lookup"><span data-stu-id="b96fa-188">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form.</span></span> <span data-ttu-id="b96fa-189">由於元件是以靜態方式呈現，因此元件不是互動式的：</span><span class="sxs-lookup"><span data-stu-id="b96fa-189">Since the component is statically rendered, the component isn't interactive:</span></span>

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

<span data-ttu-id="b96fa-190">如需詳細資訊，請參閱 <xref:xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>。</span><span class="sxs-lookup"><span data-stu-id="b96fa-190">For more information, see <xref:xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>.</span></span>

## <a name="component-namespaces"></a><span data-ttu-id="b96fa-191">元件命名空間</span><span class="sxs-lookup"><span data-stu-id="b96fa-191">Component namespaces</span></span>

<span data-ttu-id="b96fa-192">使用自訂資料夾來存放應用程式的元件時，請將代表資料夾的命名空間新增至頁面/視圖，或加入至 *_ViewImports. cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="b96fa-192">When using a custom folder to hold the app's components, add the namespace representing the folder to either the page/view or to the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="b96fa-193">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="b96fa-193">In the following example:</span></span>

* <span data-ttu-id="b96fa-194">變更`MyAppNamespace`為應用程式的命名空間。</span><span class="sxs-lookup"><span data-stu-id="b96fa-194">Change `MyAppNamespace` to the app's namespace.</span></span>
* <span data-ttu-id="b96fa-195">如果未使用名為「*元件*」的資料夾來存放元件`Components` ，請變更為元件所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="b96fa-195">If a folder named *Components* isn't used to hold the components, change `Components` to the folder where the components reside.</span></span>

```cshtml
@using MyAppNamespace.Components
```

<span data-ttu-id="b96fa-196">*_ViewImports. cshtml*檔案位於 MVC 應用程式的 Razor Pages 應用程式或*Views*資料夾的*Pages*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="b96fa-196">The *_ViewImports.cshtml* file is located in the *Pages* folder of a Razor Pages app or the *Views* folder of an MVC app.</span></span>

<span data-ttu-id="b96fa-197">如需詳細資訊，請參閱 <xref:blazor/components#import-components>。</span><span class="sxs-lookup"><span data-stu-id="b96fa-197">For more information, see <xref:blazor/components#import-components>.</span></span>
