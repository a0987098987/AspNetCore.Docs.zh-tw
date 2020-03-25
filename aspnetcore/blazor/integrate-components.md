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
# <a name="integrate-aspnet-core-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="1e9a0-103">將 ASP.NET Core Razor 元件整合至 Razor Pages 和 MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="1e9a0-103">Integrate ASP.NET Core Razor components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="1e9a0-104">By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="1e9a0-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="1e9a0-105">Razor 元件可以整合到 Razor Pages 和 MVC 應用程式中。</span><span class="sxs-lookup"><span data-stu-id="1e9a0-105">Razor components can be integrated into Razor Pages and MVC apps.</span></span> <span data-ttu-id="1e9a0-106">當頁面或視圖呈現時，可以同時資源清單元件。</span><span class="sxs-lookup"><span data-stu-id="1e9a0-106">When the page or view is rendered, components can be prerendered at the same time.</span></span>

## <a name="prepare-the-app-to-use-components-in-pages-and-views"></a><span data-ttu-id="1e9a0-107">準備應用程式以使用頁面和視圖中的元件</span><span class="sxs-lookup"><span data-stu-id="1e9a0-107">Prepare the app to use components in pages and views</span></span>

<span data-ttu-id="1e9a0-108">現有的 Razor Pages 或 MVC 應用程式可以將 Razor 元件整合至頁面和 views：</span><span class="sxs-lookup"><span data-stu-id="1e9a0-108">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="1e9a0-109">在應用程式的版面配置檔案（ *_Layout. cshtml*）中：</span><span class="sxs-lookup"><span data-stu-id="1e9a0-109">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="1e9a0-110">將下列 `<base>` 標記新增至 `<head>` 元素：</span><span class="sxs-lookup"><span data-stu-id="1e9a0-110">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="1e9a0-111">上述範例中的 `href` 值（*應用程式基底路徑*）假設應用程式位於根 URL 路徑（`/`）。</span><span class="sxs-lookup"><span data-stu-id="1e9a0-111">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="1e9a0-112">如果應用程式是子應用程式，請遵循 <xref:host-and-deploy/blazor/index#app-base-path> 文章的*應用程式基底路徑*一節中的指導方針。</span><span class="sxs-lookup"><span data-stu-id="1e9a0-112">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="1e9a0-113">*_Layout. cshtml*檔案位於 MVC 應用程式的 Razor Pages 應用程式或*Views/shared*資料夾中的*Pages/shared*資料夾內。</span><span class="sxs-lookup"><span data-stu-id="1e9a0-113">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="1e9a0-114">緊接在結尾 `</body>` 標記之前，新增*blazor*的 `<script>` 標籤：</span><span class="sxs-lookup"><span data-stu-id="1e9a0-114">Add a `<script>` tag for the *blazor.server.js* script immediately before of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="1e9a0-115">架構會將*blazor*新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="1e9a0-115">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="1e9a0-116">不需要手動將腳本新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="1e9a0-116">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="1e9a0-117">使用下列內容，將 *_Imports razor*檔案新增至專案的根資料夾（將最後一個命名空間 `MyAppNamespace`變更為應用程式的命名空間）：</span><span class="sxs-lookup"><span data-stu-id="1e9a0-117">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

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

1. <span data-ttu-id="1e9a0-118">在 `Startup.ConfigureServices`中，註冊 Blazor 伺服器服務：</span><span class="sxs-lookup"><span data-stu-id="1e9a0-118">In `Startup.ConfigureServices`, register the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="1e9a0-119">在 `Startup.Configure`中，將 Blazor 中樞端點新增至 `app.UseEndpoints`：</span><span class="sxs-lookup"><span data-stu-id="1e9a0-119">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="1e9a0-120">將元件整合到任何頁面或視圖中。</span><span class="sxs-lookup"><span data-stu-id="1e9a0-120">Integrate components into any page or view.</span></span> <span data-ttu-id="1e9a0-121">如需詳細資訊，請參閱[從頁面或視圖呈現元件](#render-components-from-a-page-or-view)一節。</span><span class="sxs-lookup"><span data-stu-id="1e9a0-121">For more information, see the [Render components from a page or view](#render-components-from-a-page-or-view) section.</span></span>

## <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="1e9a0-122">在 Razor Pages 應用程式中使用可路由的元件</span><span class="sxs-lookup"><span data-stu-id="1e9a0-122">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="1e9a0-123">*本節適用于新增可直接從使用者要求路由傳送的元件。*</span><span class="sxs-lookup"><span data-stu-id="1e9a0-123">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="1e9a0-124">在 Razor Pages 應用程式中支援可路由的 Razor 元件：</span><span class="sxs-lookup"><span data-stu-id="1e9a0-124">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="1e9a0-125">遵循[準備應用程式以使用頁面和 views 中的元件](#prepare-the-app-to-use-components-in-pages-and-views)一節中的指導方針。</span><span class="sxs-lookup"><span data-stu-id="1e9a0-125">Follow the guidance in the [Prepare the app to use components in pages and views](#prepare-the-app-to-use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="1e9a0-126">使用下列內容將*應用程式 razor*檔案新增至專案根目錄：</span><span class="sxs-lookup"><span data-stu-id="1e9a0-126">Add an *App.razor* file to the project root with the following content:</span></span>

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

1. <span data-ttu-id="1e9a0-127">將 *_Host. cshtml*檔案新增至*Pages*資料夾，其中包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="1e9a0-127">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="1e9a0-128">元件會使用共用的 *_Layout. cshtml*檔案作為其版面配置。</span><span class="sxs-lookup"><span data-stu-id="1e9a0-128">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="1e9a0-129">在 `Startup.Configure`中，將 *_Host. cshtml*頁面的低優先順序路由新增至端點設定：</span><span class="sxs-lookup"><span data-stu-id="1e9a0-129">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="1e9a0-130">將可路由的元件新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="1e9a0-130">Add routable components to the app.</span></span> <span data-ttu-id="1e9a0-131">例如：</span><span class="sxs-lookup"><span data-stu-id="1e9a0-131">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="1e9a0-132">如需命名空間的詳細資訊，請參閱[元件命名空間](#component-namespaces)一節。</span><span class="sxs-lookup"><span data-stu-id="1e9a0-132">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="1e9a0-133">在 MVC 應用程式中使用可路由的元件</span><span class="sxs-lookup"><span data-stu-id="1e9a0-133">Use routable components in an MVC app</span></span>

<span data-ttu-id="1e9a0-134">*本節適用于新增可直接從使用者要求路由傳送的元件。*</span><span class="sxs-lookup"><span data-stu-id="1e9a0-134">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="1e9a0-135">在 MVC 應用程式中支援可路由的 Razor 元件：</span><span class="sxs-lookup"><span data-stu-id="1e9a0-135">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="1e9a0-136">遵循[準備應用程式以使用頁面和 views 中的元件](#prepare-the-app-to-use-components-in-pages-and-views)一節中的指導方針。</span><span class="sxs-lookup"><span data-stu-id="1e9a0-136">Follow the guidance in the [Prepare the app to use components in pages and views](#prepare-the-app-to-use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="1e9a0-137">使用下列內容，將*應用程式 razor*檔案新增至專案的根目錄：</span><span class="sxs-lookup"><span data-stu-id="1e9a0-137">Add an *App.razor* file to the root of the project with the following content:</span></span>

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

1. <span data-ttu-id="1e9a0-138">使用下列內容，將 *_Host. cshtml*檔案新增至*Views/Home*資料夾：</span><span class="sxs-lookup"><span data-stu-id="1e9a0-138">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="1e9a0-139">元件會使用共用的 *_Layout. cshtml*檔案作為其版面配置。</span><span class="sxs-lookup"><span data-stu-id="1e9a0-139">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="1e9a0-140">將動作新增至主控制器：</span><span class="sxs-lookup"><span data-stu-id="1e9a0-140">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="1e9a0-141">為控制器動作新增低優先順序的路由，以將 *_Host. cshtml*視圖傳回 `Startup.Configure`中的端點設定：</span><span class="sxs-lookup"><span data-stu-id="1e9a0-141">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="1e9a0-142">建立*Pages*資料夾，並將可路由的元件新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="1e9a0-142">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="1e9a0-143">例如：</span><span class="sxs-lookup"><span data-stu-id="1e9a0-143">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="1e9a0-144">如需命名空間的詳細資訊，請參閱[元件命名空間](#component-namespaces)一節。</span><span class="sxs-lookup"><span data-stu-id="1e9a0-144">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="component-namespaces"></a><span data-ttu-id="1e9a0-145">元件命名空間</span><span class="sxs-lookup"><span data-stu-id="1e9a0-145">Component namespaces</span></span>

<span data-ttu-id="1e9a0-146">使用自訂資料夾來存放應用程式的元件時，請將代表資料夾的命名空間新增至頁面/視圖，或加入至 *_ViewImports. cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="1e9a0-146">When using a custom folder to hold the app's components, add the namespace representing the folder to either the page/view or to the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="1e9a0-147">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="1e9a0-147">In the following example:</span></span>

* <span data-ttu-id="1e9a0-148">將 `MyAppNamespace` 變更為應用程式的命名空間。</span><span class="sxs-lookup"><span data-stu-id="1e9a0-148">Change `MyAppNamespace` to the app's namespace.</span></span>
* <span data-ttu-id="1e9a0-149">如果未使用名為「*元件*」的資料夾來存放元件，請將 `Components` 變更為元件所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="1e9a0-149">If a folder named *Components* isn't used to hold the components, change `Components` to the folder where the components reside.</span></span>

```cshtml
@using MyAppNamespace.Components
```

<span data-ttu-id="1e9a0-150">*_ViewImports. cshtml*檔案位於 MVC 應用程式的 Razor Pages 應用程式或*Views*資料夾的*Pages*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="1e9a0-150">The *_ViewImports.cshtml* file is located in the *Pages* folder of a Razor Pages app or the *Views* folder of an MVC app.</span></span>

<span data-ttu-id="1e9a0-151">如需詳細資訊，請參閱 <xref:blazor/components#import-components>。</span><span class="sxs-lookup"><span data-stu-id="1e9a0-151">For more information, see <xref:blazor/components#import-components>.</span></span>

## <a name="render-components-from-a-page-or-view"></a><span data-ttu-id="1e9a0-152">從頁面或視圖呈現元件</span><span class="sxs-lookup"><span data-stu-id="1e9a0-152">Render components from a page or view</span></span>

<span data-ttu-id="1e9a0-153">*本節適用于將元件新增至頁面或視圖，其中元件無法直接從使用者要求路由傳送。*</span><span class="sxs-lookup"><span data-stu-id="1e9a0-153">*This section pertains to adding components to pages or views, where the components aren't directly routable from user requests.*</span></span>

<span data-ttu-id="1e9a0-154">若要從頁面或視圖呈現元件，請使用[元件標記](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper)協助程式。</span><span class="sxs-lookup"><span data-stu-id="1e9a0-154">To render a component from a page or view, use the [Component Tag Helper](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper).</span></span>

<span data-ttu-id="1e9a0-155">如需有關如何呈現元件、元件狀態和 `Component` 標籤協助程式的詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="1e9a0-155">For more information on how components are rendered, component state, and the `Component` Tag Helper, see the following articles:</span></span>

* <xref:blazor/hosting-models>
* <xref:blazor/hosting-model-configuration>
* <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>
