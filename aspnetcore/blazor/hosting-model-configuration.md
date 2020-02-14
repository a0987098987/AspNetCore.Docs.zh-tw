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
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="1744a-103">ASP.NET Core Blazor 裝載模型設定</span><span class="sxs-lookup"><span data-stu-id="1744a-103">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="1744a-104">依[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="1744a-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="1744a-105">本文涵蓋主控模型設定。</span><span class="sxs-lookup"><span data-stu-id="1744a-105">This article covers hosting model configuration.</span></span>

<!-- For future use:

## Blazor WebAssembly

-->

## <a name="blazor-server"></a><span data-ttu-id="1744a-106">Blazor 伺服器</span><span class="sxs-lookup"><span data-stu-id="1744a-106">Blazor Server</span></span>

### <a name="integrate-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="1744a-107">將 Razor 元件整合到 Razor Pages 和 MVC 應用程式中</span><span class="sxs-lookup"><span data-stu-id="1744a-107">Integrate Razor components into Razor Pages and MVC apps</span></span>

#### <a name="use-components-in-pages-and-views"></a><span data-ttu-id="1744a-108">使用頁面和視圖中的元件</span><span class="sxs-lookup"><span data-stu-id="1744a-108">Use components in pages and views</span></span>

<span data-ttu-id="1744a-109">現有的 Razor Pages 或 MVC 應用程式可以將 Razor 元件整合至頁面和 views：</span><span class="sxs-lookup"><span data-stu-id="1744a-109">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="1744a-110">在應用程式的版面配置檔案（ *_Layout. cshtml*）中：</span><span class="sxs-lookup"><span data-stu-id="1744a-110">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="1744a-111">將下列 `<base>` 標記新增至 `<head>` 元素：</span><span class="sxs-lookup"><span data-stu-id="1744a-111">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="1744a-112">上述範例中的 `href` 值（*應用程式基底路徑*）假設應用程式位於根 URL 路徑（`/`）。</span><span class="sxs-lookup"><span data-stu-id="1744a-112">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="1744a-113">如果應用程式是子應用程式，請遵循 <xref:host-and-deploy/blazor/index#app-base-path> 文章的*應用程式基底路徑*一節中的指導方針。</span><span class="sxs-lookup"><span data-stu-id="1744a-113">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="1744a-114">*_Layout. cshtml*檔案位於 MVC 應用程式的 Razor Pages 應用程式或*Views/shared*資料夾中的*Pages/shared*資料夾內。</span><span class="sxs-lookup"><span data-stu-id="1744a-114">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="1744a-115">在結尾 `</body>` 標記之前，新增*blazor*的 `<script>` 標記：</span><span class="sxs-lookup"><span data-stu-id="1744a-115">Add a `<script>` tag for the *blazor.server.js* script right before of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="1744a-116">架構會將*blazor*新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="1744a-116">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="1744a-117">不需要手動將腳本新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="1744a-117">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="1744a-118">使用下列內容，將 *_Imports razor*檔案新增至專案的根資料夾（將最後一個命名空間 `MyAppNamespace`變更為應用程式的命名空間）：</span><span class="sxs-lookup"><span data-stu-id="1744a-118">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

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

1. <span data-ttu-id="1744a-119">在 `Startup.ConfigureServices`中，註冊 Blazor 伺服器服務：</span><span class="sxs-lookup"><span data-stu-id="1744a-119">In `Startup.ConfigureServices`, register the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="1744a-120">在 `Startup.Configure`中，將 Blazor 中樞端點新增至 `app.UseEndpoints`：</span><span class="sxs-lookup"><span data-stu-id="1744a-120">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="1744a-121">將元件整合到任何頁面或視圖中。</span><span class="sxs-lookup"><span data-stu-id="1744a-121">Integrate components into any page or view.</span></span> <span data-ttu-id="1744a-122">如需詳細資訊，請參閱 <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps> 文章的將*元件整合到 Razor Pages 和 MVC 應用程式*一節。</span><span class="sxs-lookup"><span data-stu-id="1744a-122">For more information, see the *Integrate components into Razor Pages and MVC apps* section of the <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps> article.</span></span>

#### <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="1744a-123">在 Razor Pages 應用程式中使用可路由的元件</span><span class="sxs-lookup"><span data-stu-id="1744a-123">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="1744a-124">在 Razor Pages 應用程式中支援可路由的 Razor 元件：</span><span class="sxs-lookup"><span data-stu-id="1744a-124">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="1744a-125">請遵循[使用頁面和 views 中的元件](#use-components-in-pages-and-views)一節中的指導方針。</span><span class="sxs-lookup"><span data-stu-id="1744a-125">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="1744a-126">使用下列內容，將*應用程式 razor*檔案新增至專案的根目錄：</span><span class="sxs-lookup"><span data-stu-id="1744a-126">Add an *App.razor* file to the root of the project with the following content:</span></span>

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

1. <span data-ttu-id="1744a-127">將 *_Host. cshtml*檔案新增至*Pages*資料夾，其中包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="1744a-127">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="1744a-128">元件會使用共用的 *_Layout. cshtml*檔案作為其版面配置。</span><span class="sxs-lookup"><span data-stu-id="1744a-128">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="1744a-129">在 `Startup.Configure`中，將 *_Host. cshtml*頁面的低優先順序路由新增至端點設定：</span><span class="sxs-lookup"><span data-stu-id="1744a-129">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="1744a-130">將可路由的元件新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="1744a-130">Add routable components to the app.</span></span> <span data-ttu-id="1744a-131">例如：</span><span class="sxs-lookup"><span data-stu-id="1744a-131">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="1744a-132">使用自訂資料夾來保存應用程式的元件時，請將代表資料夾的命名空間新增至*Pages/_ViewImports. cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="1744a-132">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Pages/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="1744a-133">如需詳細資訊，請參閱 <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>。</span><span class="sxs-lookup"><span data-stu-id="1744a-133">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

#### <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="1744a-134">在 MVC 應用程式中使用可路由的元件</span><span class="sxs-lookup"><span data-stu-id="1744a-134">Use routable components in an MVC app</span></span>

<span data-ttu-id="1744a-135">在 MVC 應用程式中支援可路由的 Razor 元件：</span><span class="sxs-lookup"><span data-stu-id="1744a-135">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="1744a-136">請遵循[使用頁面和 views 中的元件](#use-components-in-pages-and-views)一節中的指導方針。</span><span class="sxs-lookup"><span data-stu-id="1744a-136">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="1744a-137">使用下列內容，將*應用程式 razor*檔案新增至專案的根目錄：</span><span class="sxs-lookup"><span data-stu-id="1744a-137">Add an *App.razor* file to the root of the project with the following content:</span></span>

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

1. <span data-ttu-id="1744a-138">使用下列內容，將 *_Host. cshtml*檔案新增至*Views/Home*資料夾：</span><span class="sxs-lookup"><span data-stu-id="1744a-138">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="1744a-139">元件會使用共用的 *_Layout. cshtml*檔案作為其版面配置。</span><span class="sxs-lookup"><span data-stu-id="1744a-139">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="1744a-140">將動作新增至主控制器：</span><span class="sxs-lookup"><span data-stu-id="1744a-140">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="1744a-141">為控制器動作新增低優先順序的路由，以將 *_Host. cshtml*視圖傳回 `Startup.Configure`中的端點設定：</span><span class="sxs-lookup"><span data-stu-id="1744a-141">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="1744a-142">建立*Pages*資料夾，並將可路由的元件新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="1744a-142">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="1744a-143">例如：</span><span class="sxs-lookup"><span data-stu-id="1744a-143">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="1744a-144">使用自訂資料夾來存放應用程式的元件時，請將代表資料夾的命名空間新增至*Views/_ViewImports. cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="1744a-144">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="1744a-145">如需詳細資訊，請參閱 <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>。</span><span class="sxs-lookup"><span data-stu-id="1744a-145">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="1744a-146">反映 UI 中的連接狀態</span><span class="sxs-lookup"><span data-stu-id="1744a-146">Reflect the connection state in the UI</span></span>

<span data-ttu-id="1744a-147">當用戶端偵測到連線已遺失時，會在用戶端嘗試重新連線時，向使用者顯示預設的 UI。</span><span class="sxs-lookup"><span data-stu-id="1744a-147">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="1744a-148">如果重新連線失敗，則會提供使用者重試的選項。</span><span class="sxs-lookup"><span data-stu-id="1744a-148">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="1744a-149">若要自訂 UI，請在 *_Host*的 [Razor 頁面] `<body>` 中，定義具有 `components-reconnect-modal` `id` 的元素：</span><span class="sxs-lookup"><span data-stu-id="1744a-149">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="1744a-150">下表描述套用至 `components-reconnect-modal` 元素的 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="1744a-150">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="1744a-151">CSS 類別</span><span class="sxs-lookup"><span data-stu-id="1744a-151">CSS class</span></span>                       | <span data-ttu-id="1744a-152">表示&hellip;</span><span class="sxs-lookup"><span data-stu-id="1744a-152">Indicates&hellip;</span></span> |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | <span data-ttu-id="1744a-153">遺失的連接。</span><span class="sxs-lookup"><span data-stu-id="1744a-153">A lost connection.</span></span> <span data-ttu-id="1744a-154">用戶端正在嘗試重新連線。</span><span class="sxs-lookup"><span data-stu-id="1744a-154">The client is attempting to reconnect.</span></span> <span data-ttu-id="1744a-155">顯示強制回應。</span><span class="sxs-lookup"><span data-stu-id="1744a-155">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="1744a-156">系統會將使用中的連接重新建立至伺服器。</span><span class="sxs-lookup"><span data-stu-id="1744a-156">An active connection is re-established to the server.</span></span> <span data-ttu-id="1744a-157">隱藏強制回應。</span><span class="sxs-lookup"><span data-stu-id="1744a-157">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="1744a-158">重新連接失敗，可能是因為網路失敗。</span><span class="sxs-lookup"><span data-stu-id="1744a-158">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="1744a-159">若要嘗試重新連接，請呼叫 `window.Blazor.reconnect()`。</span><span class="sxs-lookup"><span data-stu-id="1744a-159">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="1744a-160">已拒絕重新連接。</span><span class="sxs-lookup"><span data-stu-id="1744a-160">Reconnection rejected.</span></span> <span data-ttu-id="1744a-161">已達到伺服器但拒絕連線，而且伺服器上的使用者狀態遺失。</span><span class="sxs-lookup"><span data-stu-id="1744a-161">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="1744a-162">若要重載應用程式，請呼叫 `location.reload()`。</span><span class="sxs-lookup"><span data-stu-id="1744a-162">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="1744a-163">當下列情況發生時，可能會產生此連接狀態：</span><span class="sxs-lookup"><span data-stu-id="1744a-163">This connection state may result when:</span></span><ul><li><span data-ttu-id="1744a-164">伺服器端線路發生損毀。</span><span class="sxs-lookup"><span data-stu-id="1744a-164">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="1744a-165">用戶端中斷連線的時間夠長，伺服器才會捨棄使用者的狀態。</span><span class="sxs-lookup"><span data-stu-id="1744a-165">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="1744a-166">系統會處置與使用者互動之元件的實例。</span><span class="sxs-lookup"><span data-stu-id="1744a-166">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="1744a-167">伺服器會重新開機，或回收應用程式的背景工作進程。</span><span class="sxs-lookup"><span data-stu-id="1744a-167">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="render-mode"></a><span data-ttu-id="1744a-168">轉譯模式</span><span class="sxs-lookup"><span data-stu-id="1744a-168">Render mode</span></span>

<span data-ttu-id="1744a-169">在建立伺服器的用戶端連接之前，預設會設定 Blazor 伺服器應用程式，以預先呈現伺服器上的 UI。</span><span class="sxs-lookup"><span data-stu-id="1744a-169">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="1744a-170">這會在 *_Host. cshtml* Razor 頁面中設定：</span><span class="sxs-lookup"><span data-stu-id="1744a-170">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="1744a-171">`RenderMode` 設定元件是否：</span><span class="sxs-lookup"><span data-stu-id="1744a-171">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="1744a-172">會資源清單到頁面中。</span><span class="sxs-lookup"><span data-stu-id="1744a-172">Is prerendered into the page.</span></span>
* <span data-ttu-id="1744a-173">會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動 Blazor 應用程式所需的資訊。</span><span class="sxs-lookup"><span data-stu-id="1744a-173">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="1744a-174">描述</span><span class="sxs-lookup"><span data-stu-id="1744a-174">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="1744a-175">將元件轉譯為靜態 HTML，並包含 Blazor 伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="1744a-175">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="1744a-176">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="1744a-176">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="1744a-177">呈現 Blazor 伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="1744a-177">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="1744a-178">不包含來自元件的輸出。</span><span class="sxs-lookup"><span data-stu-id="1744a-178">Output from the component isn't included.</span></span> <span data-ttu-id="1744a-179">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="1744a-179">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="1744a-180">將元件轉譯為靜態 HTML。</span><span class="sxs-lookup"><span data-stu-id="1744a-180">Renders the component into static HTML.</span></span> |

<span data-ttu-id="1744a-181">不支援從靜態 HTML 網頁轉譯伺服器元件。</span><span class="sxs-lookup"><span data-stu-id="1744a-181">Rendering server components from a static HTML page isn't supported.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="1744a-182">從 Razor 頁面和 views 轉譯具狀態的互動式元件</span><span class="sxs-lookup"><span data-stu-id="1744a-182">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="1744a-183">可設定狀態的互動式元件可以新增至 Razor 頁面或視圖。</span><span class="sxs-lookup"><span data-stu-id="1744a-183">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="1744a-184">當頁面或視圖呈現時：</span><span class="sxs-lookup"><span data-stu-id="1744a-184">When the page or view renders:</span></span>

* <span data-ttu-id="1744a-185">此元件是使用頁面或視圖所資源清單。</span><span class="sxs-lookup"><span data-stu-id="1744a-185">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="1744a-186">用來進行預呈現的初始元件狀態會遺失。</span><span class="sxs-lookup"><span data-stu-id="1744a-186">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="1744a-187">建立 SignalR 連接時，會建立新的元件狀態。</span><span class="sxs-lookup"><span data-stu-id="1744a-187">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="1744a-188">下列 Razor 頁面會呈現 `Counter` 元件：</span><span class="sxs-lookup"><span data-stu-id="1744a-188">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="1744a-189">從 Razor 頁面和 views 轉譯非互動式元件</span><span class="sxs-lookup"><span data-stu-id="1744a-189">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="1744a-190">在下列 Razor 頁面中，`Counter` 元件會以靜態方式轉譯，並使用以表單指定的初始值：</span><span class="sxs-lookup"><span data-stu-id="1744a-190">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

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

<span data-ttu-id="1744a-191">由於 `MyComponent` 是以靜態方式呈現，因此元件不能是互動式的。</span><span class="sxs-lookup"><span data-stu-id="1744a-191">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="1744a-192">設定 Blazor 伺服器應用程式的 SignalR 用戶端</span><span class="sxs-lookup"><span data-stu-id="1744a-192">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="1744a-193">有時候，您需要設定 Blazor 伺服器應用程式所使用的 SignalR 用戶端。</span><span class="sxs-lookup"><span data-stu-id="1744a-193">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="1744a-194">例如，您可能會想要在 SignalR 用戶端上設定記錄，以診斷連接問題。</span><span class="sxs-lookup"><span data-stu-id="1744a-194">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="1744a-195">若要設定*Pages/_Host. cshtml*檔案中的 SignalR 用戶端：</span><span class="sxs-lookup"><span data-stu-id="1744a-195">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="1744a-196">將 `autostart="false"` 屬性加入至 `blazor.server.js` 腳本的 `<script>` 標記。</span><span class="sxs-lookup"><span data-stu-id="1744a-196">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="1744a-197">呼叫 `Blazor.start` 並傳入指定 SignalR 產生器的設定物件。</span><span class="sxs-lookup"><span data-stu-id="1744a-197">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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
