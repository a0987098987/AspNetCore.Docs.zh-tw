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
# <a name="integrate-aspnet-core-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="ac241-103">將ASP.NET核心剃刀元件整合到剃刀頁面和 MVC 應用中</span><span class="sxs-lookup"><span data-stu-id="ac241-103">Integrate ASP.NET Core Razor components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="ac241-104">由[盧克·萊瑟姆](https://github.com/guardrex)和[丹尼爾·羅斯](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="ac241-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="ac241-105">剃刀元件可以集成到剃刀頁面和 MVC 應用中。</span><span class="sxs-lookup"><span data-stu-id="ac241-105">Razor components can be integrated into Razor Pages and MVC apps.</span></span> <span data-ttu-id="ac241-106">呈現頁面或視圖時,可以同時預呈現元件。</span><span class="sxs-lookup"><span data-stu-id="ac241-106">When the page or view is rendered, components can be prerendered at the same time.</span></span>

<span data-ttu-id="ac241-107">[準備應用](#prepare-the-app)后,根據應用的要求,在以下部分中使用指南:</span><span class="sxs-lookup"><span data-stu-id="ac241-107">After [preparing the app](#prepare-the-app), use the guidance in the following sections depending on the app's requirements:</span></span>

* <span data-ttu-id="ac241-108">可路由元件&ndash;用於直接從使用者請求路由的元件。</span><span class="sxs-lookup"><span data-stu-id="ac241-108">Routable components &ndash; For components that are directly routable from user requests.</span></span> <span data-ttu-id="ac241-109">當訪問者應該能夠在其瀏覽器中對帶有指令的[`@page`](xref:mvc/views/razor#page)元件發出 HTTP 請求時,請遵循本指南。</span><span class="sxs-lookup"><span data-stu-id="ac241-109">Follow this guidance when visitors should be able to make an HTTP request in their browser for a component with an [`@page`](xref:mvc/views/razor#page) directive.</span></span>
  * [<span data-ttu-id="ac241-110">在 Razor 頁面套用使用可路由元件</span><span class="sxs-lookup"><span data-stu-id="ac241-110">Use routable components in a Razor Pages app</span></span>](#use-routable-components-in-a-razor-pages-app)
  * [<span data-ttu-id="ac241-111">在 MVC 應用程式使用可路由元件</span><span class="sxs-lookup"><span data-stu-id="ac241-111">Use routable components in an MVC app</span></span>](#use-routable-components-in-an-mvc-app)
* <span data-ttu-id="ac241-112">[從頁面或檢視](#render-components-from-a-page-or-view)&ndash;呈現元件 對於不能直接從使用者請求路由的元件。</span><span class="sxs-lookup"><span data-stu-id="ac241-112">[Render components from a page or view](#render-components-from-a-page-or-view) &ndash; For components that aren't directly routable from user requests.</span></span> <span data-ttu-id="ac241-113">當應用使用[元件標記説明器](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper)將元件嵌入到現有頁面和視圖中時,請遵循本指南。</span><span class="sxs-lookup"><span data-stu-id="ac241-113">Follow this guidance when the app embeds components into existing pages and views with the [Component Tag Helper](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper).</span></span>

## <a name="prepare-the-app"></a><span data-ttu-id="ac241-114">準備應用程式</span><span class="sxs-lookup"><span data-stu-id="ac241-114">Prepare the app</span></span>

<span data-ttu-id="ac241-115">現有的 Razor 頁面或 MVC 應用可以將 Razor 元件整合到頁面和檢視中:</span><span class="sxs-lookup"><span data-stu-id="ac241-115">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="ac241-116">在應用程式的佈局檔中 *(_Layout.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="ac241-116">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="ac241-117">將以下`<base>`標籤加入`<head>`元素:</span><span class="sxs-lookup"><span data-stu-id="ac241-117">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="ac241-118">上`href`例中的值(*應用基本路徑*)假定應用駐留在根`/`URL 路徑 ( )。</span><span class="sxs-lookup"><span data-stu-id="ac241-118">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="ac241-119">如果應用是子應用程式,請按照<xref:host-and-deploy/blazor/index#app-base-path>本文的 *"應用基本路徑*"部分中的指南操作。</span><span class="sxs-lookup"><span data-stu-id="ac241-119">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="ac241-120">*_Layout.cshtml*檔案位於 Razor 頁面應用中的 *「頁面/共用」* 資料夾中,或 MVC 應用中*的「視圖/共用*」資料夾中。</span><span class="sxs-lookup"><span data-stu-id="ac241-120">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="ac241-121">在`<script>``</body>`結束 標記之前為*blazor.server.js*文稿加入標記:</span><span class="sxs-lookup"><span data-stu-id="ac241-121">Add a `<script>` tag for the *blazor.server.js* script immediately before of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="ac241-122">框架將*blazor.server.js*文稿添加到應用程式。</span><span class="sxs-lookup"><span data-stu-id="ac241-122">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="ac241-123">無需手動將腳本添加到應用。</span><span class="sxs-lookup"><span data-stu-id="ac241-123">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="ac241-124">將 *_Imports.razor*檔案新增到具有以下內容的項目根資料夾中(將姓`MyAppNamespace`氏空間 更改為應用的命名空間):</span><span class="sxs-lookup"><span data-stu-id="ac241-124">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

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

1. <span data-ttu-id="ac241-125">在`Startup.ConfigureServices`中,註冊 Blazor 伺服器服務:</span><span class="sxs-lookup"><span data-stu-id="ac241-125">In `Startup.ConfigureServices`, register the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="ac241-126">在`Startup.Configure`中,將 Blazor`app.UseEndpoints`中心終結點加入 :</span><span class="sxs-lookup"><span data-stu-id="ac241-126">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="ac241-127">將元件整合到任何頁面或檢視中。</span><span class="sxs-lookup"><span data-stu-id="ac241-127">Integrate components into any page or view.</span></span> <span data-ttu-id="ac241-128">有關詳細資訊,請參閱[頁面或檢視部分的「渲染」元件](#render-components-from-a-page-or-view)。</span><span class="sxs-lookup"><span data-stu-id="ac241-128">For more information, see the [Render components from a page or view](#render-components-from-a-page-or-view) section.</span></span>

## <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="ac241-129">在 Razor 頁面套用使用可路由元件</span><span class="sxs-lookup"><span data-stu-id="ac241-129">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="ac241-130">*本節涉及添加直接從使用者請求中可路由的元件。*</span><span class="sxs-lookup"><span data-stu-id="ac241-130">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="ac241-131">要在 Razor 頁面應用中支援可路由的剃刀元件,可以:</span><span class="sxs-lookup"><span data-stu-id="ac241-131">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="ac241-132">依「[準備應用程式」 部分的指南進行操作](#prepare-the-app)。</span><span class="sxs-lookup"><span data-stu-id="ac241-132">Follow the guidance in the [Prepare the app](#prepare-the-app) section.</span></span>

1. <span data-ttu-id="ac241-133">將*App.razor*檔案新增到專案根,包含以下內容:</span><span class="sxs-lookup"><span data-stu-id="ac241-133">Add an *App.razor* file to the project root with the following content:</span></span>

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

1. <span data-ttu-id="ac241-134">將 *_Host.cshtml*檔案加入到包含以下內容的 *「頁面」* 資料夾:</span><span class="sxs-lookup"><span data-stu-id="ac241-134">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="ac241-135">元件使用共用 *_Layout.cshtml*文件進行佈局。</span><span class="sxs-lookup"><span data-stu-id="ac241-135">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

   <span data-ttu-id="ac241-136"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode>設定`App`元件是否:</span><span class="sxs-lookup"><span data-stu-id="ac241-136"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configures whether the `App` component:</span></span>

   * <span data-ttu-id="ac241-137">預置到頁面中。</span><span class="sxs-lookup"><span data-stu-id="ac241-137">Is prerendered into the page.</span></span>
   * <span data-ttu-id="ac241-138">在頁面上呈現為靜態 HTML,或者如果它包含從使用者代理引導 Blazor 應用的必要資訊。</span><span class="sxs-lookup"><span data-stu-id="ac241-138">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

   | <span data-ttu-id="ac241-139">成像模式</span><span class="sxs-lookup"><span data-stu-id="ac241-139">Render Mode</span></span> | <span data-ttu-id="ac241-140">描述</span><span class="sxs-lookup"><span data-stu-id="ac241-140">Description</span></span> |
   | ----------- | ----------- |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | <span data-ttu-id="ac241-141">將`App`元件呈現為靜態 HTML,並包含 Blazor Server 應用的標記。</span><span class="sxs-lookup"><span data-stu-id="ac241-141">Renders the `App` component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="ac241-142">當使用者代理啟動時,此標記用於引導 Blazor 應用。</span><span class="sxs-lookup"><span data-stu-id="ac241-142">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | <span data-ttu-id="ac241-143">渲染 Blazor 伺服器應用的標記。</span><span class="sxs-lookup"><span data-stu-id="ac241-143">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="ac241-144">不包括元件的`App`輸出。</span><span class="sxs-lookup"><span data-stu-id="ac241-144">Output from the `App` component isn't included.</span></span> <span data-ttu-id="ac241-145">當使用者代理啟動時,此標記用於引導 Blazor 應用。</span><span class="sxs-lookup"><span data-stu-id="ac241-145">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | <span data-ttu-id="ac241-146">將`App`元件呈現為靜態 HTML。</span><span class="sxs-lookup"><span data-stu-id="ac241-146">Renders the `App` component into static HTML.</span></span> |

   <span data-ttu-id="ac241-147">有關元件標記說明程式的詳細資訊,請參閱<xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>。</span><span class="sxs-lookup"><span data-stu-id="ac241-147">For more information on the Component Tag Helper, see <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>.</span></span>

1. <span data-ttu-id="ac241-148">將 *_Host.cshtml*頁的低優先權路由新增到 中的終結`Startup.Configure`點設定:</span><span class="sxs-lookup"><span data-stu-id="ac241-148">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="ac241-149">向應用添加可路由元件。</span><span class="sxs-lookup"><span data-stu-id="ac241-149">Add routable components to the app.</span></span> <span data-ttu-id="ac241-150">例如：</span><span class="sxs-lookup"><span data-stu-id="ac241-150">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

<span data-ttu-id="ac241-151">有關命名空間的詳細資訊,請參閱[元件命名空間](#component-namespaces)部分。</span><span class="sxs-lookup"><span data-stu-id="ac241-151">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="ac241-152">在 MVC 應用程式使用可路由元件</span><span class="sxs-lookup"><span data-stu-id="ac241-152">Use routable components in an MVC app</span></span>

<span data-ttu-id="ac241-153">*本節涉及添加直接從使用者請求中可路由的元件。*</span><span class="sxs-lookup"><span data-stu-id="ac241-153">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="ac241-154">要在MVC應用中支援可路由的剃刀元件,</span><span class="sxs-lookup"><span data-stu-id="ac241-154">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="ac241-155">依「[準備應用程式」 部分的指南進行操作](#prepare-the-app)。</span><span class="sxs-lookup"><span data-stu-id="ac241-155">Follow the guidance in the [Prepare the app](#prepare-the-app) section.</span></span>

1. <span data-ttu-id="ac241-156">將*App.razor*檔案新增到專案的根目錄,包含以下內容:</span><span class="sxs-lookup"><span data-stu-id="ac241-156">Add an *App.razor* file to the root of the project with the following content:</span></span>

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

1. <span data-ttu-id="ac241-157">將 *_Host.cshtml*檔案加入以下內容的*檢視/主頁*資料夾:</span><span class="sxs-lookup"><span data-stu-id="ac241-157">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="ac241-158">元件使用共用 *_Layout.cshtml*文件進行佈局。</span><span class="sxs-lookup"><span data-stu-id="ac241-158">Components use the shared *_Layout.cshtml* file for their layout.</span></span>
   
   <span data-ttu-id="ac241-159"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode>設定`App`元件是否:</span><span class="sxs-lookup"><span data-stu-id="ac241-159"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configures whether the `App` component:</span></span>

   * <span data-ttu-id="ac241-160">預置到頁面中。</span><span class="sxs-lookup"><span data-stu-id="ac241-160">Is prerendered into the page.</span></span>
   * <span data-ttu-id="ac241-161">在頁面上呈現為靜態 HTML,或者如果它包含從使用者代理引導 Blazor 應用的必要資訊。</span><span class="sxs-lookup"><span data-stu-id="ac241-161">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

   | <span data-ttu-id="ac241-162">成像模式</span><span class="sxs-lookup"><span data-stu-id="ac241-162">Render Mode</span></span> | <span data-ttu-id="ac241-163">描述</span><span class="sxs-lookup"><span data-stu-id="ac241-163">Description</span></span> |
   | ----------- | ----------- |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | <span data-ttu-id="ac241-164">將`App`元件呈現為靜態 HTML,並包含伺服器應用Blazor的標記。</span><span class="sxs-lookup"><span data-stu-id="ac241-164">Renders the `App` component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="ac241-165">當使用者代理啟動時,此標記用於引導Blazor應用。</span><span class="sxs-lookup"><span data-stu-id="ac241-165">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | <span data-ttu-id="ac241-166">渲染伺服器應用的Blazor標記。</span><span class="sxs-lookup"><span data-stu-id="ac241-166">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="ac241-167">不包括元件的`App`輸出。</span><span class="sxs-lookup"><span data-stu-id="ac241-167">Output from the `App` component isn't included.</span></span> <span data-ttu-id="ac241-168">當使用者代理啟動時,此標記用於引導Blazor應用。</span><span class="sxs-lookup"><span data-stu-id="ac241-168">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
   | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | <span data-ttu-id="ac241-169">將`App`元件呈現為靜態 HTML。</span><span class="sxs-lookup"><span data-stu-id="ac241-169">Renders the `App` component into static HTML.</span></span> |

   <span data-ttu-id="ac241-170">有關元件標記說明程式的詳細資訊,請參閱<xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>。</span><span class="sxs-lookup"><span data-stu-id="ac241-170">For more information on the Component Tag Helper, see <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>.</span></span>

1. <span data-ttu-id="ac241-171">新增控制器</span><span class="sxs-lookup"><span data-stu-id="ac241-171">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="ac241-172">為控制器操作新增低優先權路由,將 *_Host.cshtml*檢視傳回`Startup.Configure`到 中的終結點設定:</span><span class="sxs-lookup"><span data-stu-id="ac241-172">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="ac241-173">建立 *「主頁」* 資料夾並將可路由元件添加到應用。</span><span class="sxs-lookup"><span data-stu-id="ac241-173">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="ac241-174">例如：</span><span class="sxs-lookup"><span data-stu-id="ac241-174">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

<span data-ttu-id="ac241-175">有關命名空間的詳細資訊,請參閱[元件命名空間](#component-namespaces)部分。</span><span class="sxs-lookup"><span data-stu-id="ac241-175">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="render-components-from-a-page-or-view"></a><span data-ttu-id="ac241-176">從頁面或檢視成元件</span><span class="sxs-lookup"><span data-stu-id="ac241-176">Render components from a page or view</span></span>

<span data-ttu-id="ac241-177">*本節涉及向頁面或視圖添加元件,其中元件不能直接從使用者請求中路由。*</span><span class="sxs-lookup"><span data-stu-id="ac241-177">*This section pertains to adding components to pages or views, where the components aren't directly routable from user requests.*</span></span>

<span data-ttu-id="ac241-178">要從頁面或檢視呈現元件,請使用[元件標記說明程式](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="ac241-178">To render a component from a page or view, use the [Component Tag Helper](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper).</span></span>

<span data-ttu-id="ac241-179">有關如何呈現元件、元件狀態和`Component`標記説明程式的詳細資訊,請參閱以下文章:</span><span class="sxs-lookup"><span data-stu-id="ac241-179">For more information on how components are rendered, component state, and the `Component` Tag Helper, see the following articles:</span></span>

* <xref:blazor/hosting-models>
* <xref:blazor/hosting-model-configuration>
* <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>

## <a name="component-namespaces"></a><span data-ttu-id="ac241-180">元件命名空間</span><span class="sxs-lookup"><span data-stu-id="ac241-180">Component namespaces</span></span>

<span data-ttu-id="ac241-181">使用自訂資料夾保存應用元件時,將表示資料夾的命名空間添加到頁面/視圖或 *_ViewImports.cshtml*檔中。</span><span class="sxs-lookup"><span data-stu-id="ac241-181">When using a custom folder to hold the app's components, add the namespace representing the folder to either the page/view or to the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="ac241-182">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="ac241-182">In the following example:</span></span>

* <span data-ttu-id="ac241-183">更改為`MyAppNamespace`應用的命名空間。</span><span class="sxs-lookup"><span data-stu-id="ac241-183">Change `MyAppNamespace` to the app's namespace.</span></span>
* <span data-ttu-id="ac241-184">如果名為 *「元件」* 的資料夾不用於儲存元件,則更改為`Components`元件所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="ac241-184">If a folder named *Components* isn't used to hold the components, change `Components` to the folder where the components reside.</span></span>

```cshtml
@using MyAppNamespace.Components
```

<span data-ttu-id="ac241-185">*_ViewImports.cshtml*檔案位於 Razor Pages 應用的 *「頁面」* 資料夾或 MVC 應用的 *「檢視」* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="ac241-185">The *_ViewImports.cshtml* file is located in the *Pages* folder of a Razor Pages app or the *Views* folder of an MVC app.</span></span>

<span data-ttu-id="ac241-186">如需詳細資訊，請參閱 <xref:blazor/components#import-components>。</span><span class="sxs-lookup"><span data-stu-id="ac241-186">For more information, see <xref:blazor/components#import-components>.</span></span>
