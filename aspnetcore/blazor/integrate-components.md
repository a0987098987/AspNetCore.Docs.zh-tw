---
title: 將ASP.NET核心剃刀元件整合到剃刀頁面和 MVC 應用中
author: guardrex
description: 瞭解Blazor應用中元件和 DOM 元素的數據繫結方案。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/17/2020
no-loc:
- Blazor
- SignalR
uid: blazor/integrate-components
ms.openlocfilehash: cf6056e0985d5433bddecac8dd183ca3f4c2af5b
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80218930"
---
# <a name="integrate-aspnet-core-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="43a75-103">將ASP.NET核心剃刀元件整合到剃刀頁面和 MVC 應用中</span><span class="sxs-lookup"><span data-stu-id="43a75-103">Integrate ASP.NET Core Razor components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="43a75-104">由[盧克·萊瑟姆](https://github.com/guardrex)和[丹尼爾·羅斯](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="43a75-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="43a75-105">剃刀元件可以集成到剃刀頁面和 MVC 應用中。</span><span class="sxs-lookup"><span data-stu-id="43a75-105">Razor components can be integrated into Razor Pages and MVC apps.</span></span> <span data-ttu-id="43a75-106">呈現頁面或視圖時,可以同時預呈現元件。</span><span class="sxs-lookup"><span data-stu-id="43a75-106">When the page or view is rendered, components can be prerendered at the same time.</span></span>

## <a name="prepare-the-app-to-use-components-in-pages-and-views"></a><span data-ttu-id="43a75-107">準備應用以在頁面和檢視中使用元件</span><span class="sxs-lookup"><span data-stu-id="43a75-107">Prepare the app to use components in pages and views</span></span>

<span data-ttu-id="43a75-108">現有的 Razor 頁面或 MVC 應用可以將 Razor 元件整合到頁面和檢視中:</span><span class="sxs-lookup"><span data-stu-id="43a75-108">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="43a75-109">在應用程式的佈局檔中 *(_Layout.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="43a75-109">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="43a75-110">將以下`<base>`標籤加入`<head>`元素:</span><span class="sxs-lookup"><span data-stu-id="43a75-110">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="43a75-111">上`href`例中的值(*應用基本路徑*)假定應用駐留在根`/`URL 路徑 ( )。</span><span class="sxs-lookup"><span data-stu-id="43a75-111">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="43a75-112">如果應用是子應用程式,請按照<xref:host-and-deploy/blazor/index#app-base-path>本文的 *"應用基本路徑*"部分中的指南操作。</span><span class="sxs-lookup"><span data-stu-id="43a75-112">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="43a75-113">*_Layout.cshtml*檔案位於 Razor 頁面應用中的 *「頁面/共用」* 資料夾中,或 MVC 應用中*的「視圖/共用*」資料夾中。</span><span class="sxs-lookup"><span data-stu-id="43a75-113">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="43a75-114">在`<script>``</body>`結束 標記之前為*blazor.server.js*文稿加入標記:</span><span class="sxs-lookup"><span data-stu-id="43a75-114">Add a `<script>` tag for the *blazor.server.js* script immediately before of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="43a75-115">框架將*blazor.server.js*文稿添加到應用程式。</span><span class="sxs-lookup"><span data-stu-id="43a75-115">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="43a75-116">無需手動將腳本添加到應用。</span><span class="sxs-lookup"><span data-stu-id="43a75-116">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="43a75-117">將 *_Imports.razor*檔案新增到具有以下內容的項目根資料夾中(將姓`MyAppNamespace`氏空間 更改為應用的命名空間):</span><span class="sxs-lookup"><span data-stu-id="43a75-117">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

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

1. <span data-ttu-id="43a75-118">在`Startup.ConfigureServices`中,Blazor註冊伺服器服務:</span><span class="sxs-lookup"><span data-stu-id="43a75-118">In `Startup.ConfigureServices`, register the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="43a75-119">在`Startup.Configure`中,Blazor將中心終結`app.UseEndpoints`點 新增到 :</span><span class="sxs-lookup"><span data-stu-id="43a75-119">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="43a75-120">將元件整合到任何頁面或檢視中。</span><span class="sxs-lookup"><span data-stu-id="43a75-120">Integrate components into any page or view.</span></span> <span data-ttu-id="43a75-121">有關詳細資訊,請參閱[頁面或檢視部分的「渲染」元件](#render-components-from-a-page-or-view)。</span><span class="sxs-lookup"><span data-stu-id="43a75-121">For more information, see the [Render components from a page or view](#render-components-from-a-page-or-view) section.</span></span>

## <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="43a75-122">在 Razor 頁面套用使用可路由元件</span><span class="sxs-lookup"><span data-stu-id="43a75-122">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="43a75-123">*本節涉及添加直接從使用者請求中可路由的元件。*</span><span class="sxs-lookup"><span data-stu-id="43a75-123">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="43a75-124">要在 Razor 頁面應用中支援可路由的剃刀元件,可以:</span><span class="sxs-lookup"><span data-stu-id="43a75-124">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="43a75-125">依「[準備應用程式」 的指南在頁面與檢視部份中使用元件](#prepare-the-app-to-use-components-in-pages-and-views)。</span><span class="sxs-lookup"><span data-stu-id="43a75-125">Follow the guidance in the [Prepare the app to use components in pages and views](#prepare-the-app-to-use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="43a75-126">將*App.razor*檔案新增到專案根,包含以下內容:</span><span class="sxs-lookup"><span data-stu-id="43a75-126">Add an *App.razor* file to the project root with the following content:</span></span>

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

1. <span data-ttu-id="43a75-127">將 *_Host.cshtml*檔案加入到包含以下內容的 *「頁面」* 資料夾:</span><span class="sxs-lookup"><span data-stu-id="43a75-127">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="43a75-128">元件使用共用 *_Layout.cshtml*文件進行佈局。</span><span class="sxs-lookup"><span data-stu-id="43a75-128">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="43a75-129">將 *_Host.cshtml*頁的低優先權路由新增到 中的終結`Startup.Configure`點設定:</span><span class="sxs-lookup"><span data-stu-id="43a75-129">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="43a75-130">向應用添加可路由元件。</span><span class="sxs-lookup"><span data-stu-id="43a75-130">Add routable components to the app.</span></span> <span data-ttu-id="43a75-131">例如：</span><span class="sxs-lookup"><span data-stu-id="43a75-131">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="43a75-132">有關命名空間的詳細資訊,請參閱[元件命名空間](#component-namespaces)部分。</span><span class="sxs-lookup"><span data-stu-id="43a75-132">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="43a75-133">在 MVC 應用程式使用可路由元件</span><span class="sxs-lookup"><span data-stu-id="43a75-133">Use routable components in an MVC app</span></span>

<span data-ttu-id="43a75-134">*本節涉及添加直接從使用者請求中可路由的元件。*</span><span class="sxs-lookup"><span data-stu-id="43a75-134">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="43a75-135">要在MVC應用中支援可路由的剃刀元件,</span><span class="sxs-lookup"><span data-stu-id="43a75-135">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="43a75-136">依「[準備應用程式」 的指南在頁面與檢視部份中使用元件](#prepare-the-app-to-use-components-in-pages-and-views)。</span><span class="sxs-lookup"><span data-stu-id="43a75-136">Follow the guidance in the [Prepare the app to use components in pages and views](#prepare-the-app-to-use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="43a75-137">將*App.razor*檔案新增到專案的根目錄,包含以下內容:</span><span class="sxs-lookup"><span data-stu-id="43a75-137">Add an *App.razor* file to the root of the project with the following content:</span></span>

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

1. <span data-ttu-id="43a75-138">將 *_Host.cshtml*檔案加入以下內容的*檢視/主頁*資料夾:</span><span class="sxs-lookup"><span data-stu-id="43a75-138">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="43a75-139">元件使用共用 *_Layout.cshtml*文件進行佈局。</span><span class="sxs-lookup"><span data-stu-id="43a75-139">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="43a75-140">新增控制器</span><span class="sxs-lookup"><span data-stu-id="43a75-140">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="43a75-141">為控制器操作新增低優先權路由,將 *_Host.cshtml*檢視傳回`Startup.Configure`到 中的終結點設定:</span><span class="sxs-lookup"><span data-stu-id="43a75-141">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="43a75-142">建立 *「主頁」* 資料夾並將可路由元件添加到應用。</span><span class="sxs-lookup"><span data-stu-id="43a75-142">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="43a75-143">例如：</span><span class="sxs-lookup"><span data-stu-id="43a75-143">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="43a75-144">有關命名空間的詳細資訊,請參閱[元件命名空間](#component-namespaces)部分。</span><span class="sxs-lookup"><span data-stu-id="43a75-144">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="component-namespaces"></a><span data-ttu-id="43a75-145">元件命名空間</span><span class="sxs-lookup"><span data-stu-id="43a75-145">Component namespaces</span></span>

<span data-ttu-id="43a75-146">使用自訂資料夾保存應用元件時,將表示資料夾的命名空間添加到頁面/視圖或 *_ViewImports.cshtml*檔中。</span><span class="sxs-lookup"><span data-stu-id="43a75-146">When using a custom folder to hold the app's components, add the namespace representing the folder to either the page/view or to the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="43a75-147">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="43a75-147">In the following example:</span></span>

* <span data-ttu-id="43a75-148">更改為`MyAppNamespace`應用的命名空間。</span><span class="sxs-lookup"><span data-stu-id="43a75-148">Change `MyAppNamespace` to the app's namespace.</span></span>
* <span data-ttu-id="43a75-149">如果名為 *「元件」* 的資料夾不用於儲存元件,則更改為`Components`元件所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="43a75-149">If a folder named *Components* isn't used to hold the components, change `Components` to the folder where the components reside.</span></span>

```cshtml
@using MyAppNamespace.Components
```

<span data-ttu-id="43a75-150">*_ViewImports.cshtml*檔案位於 Razor Pages 應用的 *「頁面」* 資料夾或 MVC 應用的 *「檢視」* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="43a75-150">The *_ViewImports.cshtml* file is located in the *Pages* folder of a Razor Pages app or the *Views* folder of an MVC app.</span></span>

<span data-ttu-id="43a75-151">如需詳細資訊，請參閱 <xref:blazor/components#import-components>。</span><span class="sxs-lookup"><span data-stu-id="43a75-151">For more information, see <xref:blazor/components#import-components>.</span></span>

## <a name="render-components-from-a-page-or-view"></a><span data-ttu-id="43a75-152">從頁面或檢視成元件</span><span class="sxs-lookup"><span data-stu-id="43a75-152">Render components from a page or view</span></span>

<span data-ttu-id="43a75-153">*本節涉及向頁面或視圖添加元件,其中元件不能直接從使用者請求中路由。*</span><span class="sxs-lookup"><span data-stu-id="43a75-153">*This section pertains to adding components to pages or views, where the components aren't directly routable from user requests.*</span></span>

<span data-ttu-id="43a75-154">要從頁面或檢視呈現元件,請使用[元件標記說明程式](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="43a75-154">To render a component from a page or view, use the [Component Tag Helper](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper).</span></span>

<span data-ttu-id="43a75-155">有關如何呈現元件、元件狀態和`Component`標記説明程式的詳細資訊,請參閱以下文章:</span><span class="sxs-lookup"><span data-stu-id="43a75-155">For more information on how components are rendered, component state, and the `Component` Tag Helper, see the following articles:</span></span>

* <xref:blazor/hosting-models>
* <xref:blazor/hosting-model-configuration>
* <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>
