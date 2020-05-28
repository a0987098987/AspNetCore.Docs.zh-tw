---
<span data-ttu-id="53bc7-101">標題：將 ASP.NET Core Razor 元件整合至 Razor 頁面和 MVC 應用程式的作者：描述：「瞭解應用程式中元件和 DOM 元素的資料系結案例」 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="53bc7-101">title: 'Integrate ASP.NET Core Razor components into Razor Pages and MVC apps' author: description: 'Learn about data binding scenarios for components and DOM elements in Blazor apps.'</span></span>
<span data-ttu-id="53bc7-102">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="53bc7-102">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="53bc7-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-103">'Blazor'</span></span>
- <span data-ttu-id="53bc7-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="53bc7-104">'Identity'</span></span>
- <span data-ttu-id="53bc7-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="53bc7-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="53bc7-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-106">'Razor'</span></span>
- <span data-ttu-id="53bc7-107">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="53bc7-107">'SignalR' uid:</span></span> 

---
# <a name="integrate-aspnet-core-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="53bc7-108">將 ASP.NET Core Razor 元件整合至 Razor 頁面和 MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="53bc7-108">Integrate ASP.NET Core Razor components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="53bc7-109">By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="53bc7-109">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

Razor<span data-ttu-id="53bc7-110">元件可以整合至 Razor 頁面和 MVC 應用程式中。</span><span class="sxs-lookup"><span data-stu-id="53bc7-110"> components can be integrated into Razor Pages and MVC apps.</span></span> <span data-ttu-id="53bc7-111">當頁面或視圖呈現時，可以同時資源清單元件。</span><span class="sxs-lookup"><span data-stu-id="53bc7-111">When the page or view is rendered, components can be prerendered at the same time.</span></span>

<span data-ttu-id="53bc7-112">[準備好應用程式](#prepare-the-app)之後，請根據應用程式的需求，使用下列各節中的指導方針：</span><span class="sxs-lookup"><span data-stu-id="53bc7-112">After [preparing the app](#prepare-the-app), use the guidance in the following sections depending on the app's requirements:</span></span>

* <span data-ttu-id="53bc7-113">可路由的元件：適用于直接從使用者要求路由傳送的元件。</span><span class="sxs-lookup"><span data-stu-id="53bc7-113">Routable components: For components that are directly routable from user requests.</span></span> <span data-ttu-id="53bc7-114">當訪客應該能夠在其瀏覽器中針對具有指示詞的元件提出 HTTP 要求時，請遵循此指導 [`@page`](xref:mvc/views/razor#page) 方針。</span><span class="sxs-lookup"><span data-stu-id="53bc7-114">Follow this guidance when visitors should be able to make an HTTP request in their browser for a component with an [`@page`](xref:mvc/views/razor#page) directive.</span></span>
  * <span data-ttu-id="53bc7-115">[在頁面應用程式中使用可路由的元件 Razor](#use-routable-components-in-a-razor-pages-app)</span><span class="sxs-lookup"><span data-stu-id="53bc7-115">[Use routable components in a Razor Pages app](#use-routable-components-in-a-razor-pages-app)</span></span>
  * [<span data-ttu-id="53bc7-116">在 MVC 應用程式中使用可路由的元件</span><span class="sxs-lookup"><span data-stu-id="53bc7-116">Use routable components in an MVC app</span></span>](#use-routable-components-in-an-mvc-app)
* <span data-ttu-id="53bc7-117">[從頁面或視圖呈現元件](#render-components-from-a-page-or-view)：適用于無法直接從使用者要求路由傳送的元件。</span><span class="sxs-lookup"><span data-stu-id="53bc7-117">[Render components from a page or view](#render-components-from-a-page-or-view): For components that aren't directly routable from user requests.</span></span> <span data-ttu-id="53bc7-118">當應用程式使用[元件標記](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper)協助程式將元件內嵌至現有頁面和視圖時，請遵循此指導方針。</span><span class="sxs-lookup"><span data-stu-id="53bc7-118">Follow this guidance when the app embeds components into existing pages and views with the [Component Tag Helper](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper).</span></span>

## <a name="prepare-the-app"></a><span data-ttu-id="53bc7-119">準備應用程式</span><span class="sxs-lookup"><span data-stu-id="53bc7-119">Prepare the app</span></span>

<span data-ttu-id="53bc7-120">現有的 Razor 頁面或 MVC 應用程式可以將 Razor 元件整合至頁面和視圖：</span><span class="sxs-lookup"><span data-stu-id="53bc7-120">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="53bc7-121">在應用程式的版面配置檔案（*_Layout. cshtml*）中：</span><span class="sxs-lookup"><span data-stu-id="53bc7-121">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="53bc7-122">將下列 `<base>` 標記新增至 `<head>` 元素：</span><span class="sxs-lookup"><span data-stu-id="53bc7-122">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="53bc7-123">`href`上述範例中的值（*應用程式基底路徑*）假設應用程式位於根 URL 路徑（ `/` ）。</span><span class="sxs-lookup"><span data-stu-id="53bc7-123">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="53bc7-124">如果應用程式是子應用程式，請遵循文章的*應用程式基底路徑*一節中的指導方針 <xref:host-and-deploy/blazor/index#app-base-path> 。</span><span class="sxs-lookup"><span data-stu-id="53bc7-124">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="53bc7-125">*_Layout 的 cshtml*檔案位於 MVC 應用程式中頁面應用程式或 Views/shared 資料夾的*pages/shared*資料夾中。 Razor *Views/Shared*</span><span class="sxs-lookup"><span data-stu-id="53bc7-125">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="53bc7-126">緊接在 `<script>` 結束記號之前，新增*blazor*的標記 `</body>` ：</span><span class="sxs-lookup"><span data-stu-id="53bc7-126">Add a `<script>` tag for the *blazor.server.js* script immediately before of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="53bc7-127">架構會將*blazor*新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="53bc7-127">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="53bc7-128">不需要手動將腳本新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="53bc7-128">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="53bc7-129">使用下列內容，將 *_Imports razor*檔案加入至專案的根資料夾（將最後一個命名空間變更 `MyAppNamespace` 為應用程式的命名空間）：</span><span class="sxs-lookup"><span data-stu-id="53bc7-129">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

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

1. <span data-ttu-id="53bc7-130">在中 `Startup.ConfigureServices` ，註冊 Blazor 伺服器服務：</span><span class="sxs-lookup"><span data-stu-id="53bc7-130">In `Startup.ConfigureServices`, register the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="53bc7-131">在中 `Startup.Configure` ，將 Blazor 中樞端點新增至 `app.UseEndpoints` ：</span><span class="sxs-lookup"><span data-stu-id="53bc7-131">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="53bc7-132">將元件整合到任何頁面或視圖中。</span><span class="sxs-lookup"><span data-stu-id="53bc7-132">Integrate components into any page or view.</span></span> <span data-ttu-id="53bc7-133">如需詳細資訊，請參閱[從頁面或視圖呈現元件](#render-components-from-a-page-or-view)一節。</span><span class="sxs-lookup"><span data-stu-id="53bc7-133">For more information, see the [Render components from a page or view](#render-components-from-a-page-or-view) section.</span></span>

## <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="53bc7-134">在頁面應用程式中使用可路由的元件 Razor</span><span class="sxs-lookup"><span data-stu-id="53bc7-134">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="53bc7-135">*本節適用于新增可直接從使用者要求路由傳送的元件。*</span><span class="sxs-lookup"><span data-stu-id="53bc7-135">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="53bc7-136">支援 Razor 頁面應用程式中可路由的元件 Razor ：</span><span class="sxs-lookup"><span data-stu-id="53bc7-136">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="53bc7-137">請遵循[準備應用程式](#prepare-the-app)一節中的指導方針。</span><span class="sxs-lookup"><span data-stu-id="53bc7-137">Follow the guidance in the [Prepare the app](#prepare-the-app) section.</span></span>

1. <span data-ttu-id="53bc7-138">使用下列內容將*應用程式 razor*檔案新增至專案根目錄：</span><span class="sxs-lookup"><span data-stu-id="53bc7-138">Add an *App.razor* file to the project root with the following content:</span></span>

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

1. <span data-ttu-id="53bc7-139">將 *_Host. cshtml*檔案新增至*Pages*資料夾，其中包含下列內容：</span><span class="sxs-lookup"><span data-stu-id="53bc7-139">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="53bc7-140">元件會使用共用的 *_Layout. cshtml*檔案作為其版面配置。</span><span class="sxs-lookup"><span data-stu-id="53bc7-140">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

   <span data-ttu-id="53bc7-141"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode>設定元件是否 `App` ：</span><span class="sxs-lookup"><span data-stu-id="53bc7-141"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configures whether the `App` component:</span></span>

   * <span data-ttu-id="53bc7-142">會資源清單到頁面中。</span><span class="sxs-lookup"><span data-stu-id="53bc7-142">Is prerendered into the page.</span></span>
   * <span data-ttu-id="53bc7-143">會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動應用程式所需的資訊 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="53bc7-143">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

   | <span data-ttu-id="53bc7-144">轉譯模式</span><span class="sxs-lookup"><span data-stu-id="53bc7-144">Render Mode</span></span> | <span data-ttu-id="53bc7-145">描述</span><span class="sxs-lookup"><span data-stu-id="53bc7-145">Description</span></span> |
   | ---
<span data-ttu-id="53bc7-146">標題：將 ASP.NET Core Razor 元件整合至 Razor 頁面和 MVC 應用程式的作者：描述：「瞭解應用程式中元件和 DOM 元素的資料系結案例」 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="53bc7-146">title: 'Integrate ASP.NET Core Razor components into Razor Pages and MVC apps' author: description: 'Learn about data binding scenarios for components and DOM elements in Blazor apps.'</span></span>
<span data-ttu-id="53bc7-147">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="53bc7-147">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="53bc7-148">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-148">'Blazor'</span></span>
- <span data-ttu-id="53bc7-149">'Identity'</span><span class="sxs-lookup"><span data-stu-id="53bc7-149">'Identity'</span></span>
- <span data-ttu-id="53bc7-150">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="53bc7-150">'Let's Encrypt'</span></span>
- <span data-ttu-id="53bc7-151">'Razor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-151">'Razor'</span></span>
- <span data-ttu-id="53bc7-152">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="53bc7-152">'SignalR' uid:</span></span> 

-
<span data-ttu-id="53bc7-153">標題：將 ASP.NET Core Razor 元件整合至 Razor 頁面和 MVC 應用程式的作者：描述：「瞭解應用程式中元件和 DOM 元素的資料系結案例」 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="53bc7-153">title: 'Integrate ASP.NET Core Razor components into Razor Pages and MVC apps' author: description: 'Learn about data binding scenarios for components and DOM elements in Blazor apps.'</span></span>
<span data-ttu-id="53bc7-154">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="53bc7-154">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="53bc7-155">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-155">'Blazor'</span></span>
- <span data-ttu-id="53bc7-156">'Identity'</span><span class="sxs-lookup"><span data-stu-id="53bc7-156">'Identity'</span></span>
- <span data-ttu-id="53bc7-157">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="53bc7-157">'Let's Encrypt'</span></span>
- <span data-ttu-id="53bc7-158">'Razor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-158">'Razor'</span></span>
- <span data-ttu-id="53bc7-159">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="53bc7-159">'SignalR' uid:</span></span> 

-
<span data-ttu-id="53bc7-160">標題：將 ASP.NET Core Razor 元件整合至 Razor 頁面和 MVC 應用程式的作者：描述：「瞭解應用程式中元件和 DOM 元素的資料系結案例」 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="53bc7-160">title: 'Integrate ASP.NET Core Razor components into Razor Pages and MVC apps' author: description: 'Learn about data binding scenarios for components and DOM elements in Blazor apps.'</span></span>
<span data-ttu-id="53bc7-161">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="53bc7-161">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="53bc7-162">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-162">'Blazor'</span></span>
- <span data-ttu-id="53bc7-163">'Identity'</span><span class="sxs-lookup"><span data-stu-id="53bc7-163">'Identity'</span></span>
- <span data-ttu-id="53bc7-164">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="53bc7-164">'Let's Encrypt'</span></span>
- <span data-ttu-id="53bc7-165">'Razor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-165">'Razor'</span></span>
- <span data-ttu-id="53bc7-166">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="53bc7-166">'SignalR' uid:</span></span> 

<span data-ttu-id="53bc7-167">------ |---標題：「將 ASP.NET Core Razor 元件整合至 Razor 頁面和 MVC 應用程式的作者：描述：」瞭解應用程式中元件和 DOM 元素的資料系結案例 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="53bc7-167">------ | --- title: 'Integrate ASP.NET Core Razor components into Razor Pages and MVC apps' author: description: 'Learn about data binding scenarios for components and DOM elements in Blazor apps.'</span></span>
<span data-ttu-id="53bc7-168">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="53bc7-168">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="53bc7-169">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-169">'Blazor'</span></span>
- <span data-ttu-id="53bc7-170">'Identity'</span><span class="sxs-lookup"><span data-stu-id="53bc7-170">'Identity'</span></span>
- <span data-ttu-id="53bc7-171">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="53bc7-171">'Let's Encrypt'</span></span>
- <span data-ttu-id="53bc7-172">'Razor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-172">'Razor'</span></span>
- <span data-ttu-id="53bc7-173">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="53bc7-173">'SignalR' uid:</span></span> 

-
<span data-ttu-id="53bc7-174">標題：將 ASP.NET Core Razor 元件整合至 Razor 頁面和 MVC 應用程式的作者：描述：「瞭解應用程式中元件和 DOM 元素的資料系結案例」 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="53bc7-174">title: 'Integrate ASP.NET Core Razor components into Razor Pages and MVC apps' author: description: 'Learn about data binding scenarios for components and DOM elements in Blazor apps.'</span></span>
<span data-ttu-id="53bc7-175">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="53bc7-175">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="53bc7-176">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-176">'Blazor'</span></span>
- <span data-ttu-id="53bc7-177">'Identity'</span><span class="sxs-lookup"><span data-stu-id="53bc7-177">'Identity'</span></span>
- <span data-ttu-id="53bc7-178">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="53bc7-178">'Let's Encrypt'</span></span>
- <span data-ttu-id="53bc7-179">'Razor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-179">'Razor'</span></span>
- <span data-ttu-id="53bc7-180">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="53bc7-180">'SignalR' uid:</span></span> 

-
<span data-ttu-id="53bc7-181">標題：將 ASP.NET Core Razor 元件整合至 Razor 頁面和 MVC 應用程式的作者：描述：「瞭解應用程式中元件和 DOM 元素的資料系結案例」 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="53bc7-181">title: 'Integrate ASP.NET Core Razor components into Razor Pages and MVC apps' author: description: 'Learn about data binding scenarios for components and DOM elements in Blazor apps.'</span></span>
<span data-ttu-id="53bc7-182">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="53bc7-182">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="53bc7-183">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-183">'Blazor'</span></span>
- <span data-ttu-id="53bc7-184">'Identity'</span><span class="sxs-lookup"><span data-stu-id="53bc7-184">'Identity'</span></span>
- <span data-ttu-id="53bc7-185">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="53bc7-185">'Let's Encrypt'</span></span>
- <span data-ttu-id="53bc7-186">'Razor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-186">'Razor'</span></span>
- <span data-ttu-id="53bc7-187">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="53bc7-187">'SignalR' uid:</span></span> 

<span data-ttu-id="53bc7-188">------ | |<xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> |將 `App` 元件轉譯為靜態 HTML，並包含 Blazor 伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="53bc7-188">------ | | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | Renders the `App` component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="53bc7-189">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="53bc7-189">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="53bc7-190">| |<xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> |呈現 Blazor 伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="53bc7-190">| | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="53bc7-191">`App`不包含來自元件的輸出。</span><span class="sxs-lookup"><span data-stu-id="53bc7-191">Output from the `App` component isn't included.</span></span> <span data-ttu-id="53bc7-192">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="53bc7-192">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="53bc7-193">| |<xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> |將 `App` 元件轉譯為靜態 HTML。</span><span class="sxs-lookup"><span data-stu-id="53bc7-193">| | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | Renders the `App` component into static HTML.</span></span> |

   <span data-ttu-id="53bc7-194">如需元件標記協助程式的詳細資訊，請參閱 <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper> 。</span><span class="sxs-lookup"><span data-stu-id="53bc7-194">For more information on the Component Tag Helper, see <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>.</span></span>

1. <span data-ttu-id="53bc7-195">在中，將 *_Host. cshtml*頁面的低優先順序路由新增至端點設定 `Startup.Configure` ：</span><span class="sxs-lookup"><span data-stu-id="53bc7-195">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="53bc7-196">將可路由的元件新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="53bc7-196">Add routable components to the app.</span></span> <span data-ttu-id="53bc7-197">例如：</span><span class="sxs-lookup"><span data-stu-id="53bc7-197">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

<span data-ttu-id="53bc7-198">如需命名空間的詳細資訊，請參閱[元件命名空間](#component-namespaces)一節。</span><span class="sxs-lookup"><span data-stu-id="53bc7-198">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="53bc7-199">在 MVC 應用程式中使用可路由的元件</span><span class="sxs-lookup"><span data-stu-id="53bc7-199">Use routable components in an MVC app</span></span>

<span data-ttu-id="53bc7-200">*本節適用于新增可直接從使用者要求路由傳送的元件。*</span><span class="sxs-lookup"><span data-stu-id="53bc7-200">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="53bc7-201">Razor在 MVC 應用程式中支援可路由的元件：</span><span class="sxs-lookup"><span data-stu-id="53bc7-201">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="53bc7-202">請遵循[準備應用程式](#prepare-the-app)一節中的指導方針。</span><span class="sxs-lookup"><span data-stu-id="53bc7-202">Follow the guidance in the [Prepare the app](#prepare-the-app) section.</span></span>

1. <span data-ttu-id="53bc7-203">使用下列內容，將*應用程式 razor*檔案新增至專案的根目錄：</span><span class="sxs-lookup"><span data-stu-id="53bc7-203">Add an *App.razor* file to the root of the project with the following content:</span></span>

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

1. <span data-ttu-id="53bc7-204">使用下列內容，將 *_Host. cshtml*檔案新增至*Views/Home*資料夾：</span><span class="sxs-lookup"><span data-stu-id="53bc7-204">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="53bc7-205">元件會使用共用的 *_Layout. cshtml*檔案作為其版面配置。</span><span class="sxs-lookup"><span data-stu-id="53bc7-205">Components use the shared *_Layout.cshtml* file for their layout.</span></span>
   
   <span data-ttu-id="53bc7-206"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode>設定元件是否 `App` ：</span><span class="sxs-lookup"><span data-stu-id="53bc7-206"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configures whether the `App` component:</span></span>

   * <span data-ttu-id="53bc7-207">會資源清單到頁面中。</span><span class="sxs-lookup"><span data-stu-id="53bc7-207">Is prerendered into the page.</span></span>
   * <span data-ttu-id="53bc7-208">會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動應用程式所需的資訊 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="53bc7-208">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

   | <span data-ttu-id="53bc7-209">轉譯模式</span><span class="sxs-lookup"><span data-stu-id="53bc7-209">Render Mode</span></span> | <span data-ttu-id="53bc7-210">描述</span><span class="sxs-lookup"><span data-stu-id="53bc7-210">Description</span></span> |
   | ---
<span data-ttu-id="53bc7-211">標題：將 ASP.NET Core Razor 元件整合至 Razor 頁面和 MVC 應用程式的作者：描述：「瞭解應用程式中元件和 DOM 元素的資料系結案例」 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="53bc7-211">title: 'Integrate ASP.NET Core Razor components into Razor Pages and MVC apps' author: description: 'Learn about data binding scenarios for components and DOM elements in Blazor apps.'</span></span>
<span data-ttu-id="53bc7-212">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="53bc7-212">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="53bc7-213">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-213">'Blazor'</span></span>
- <span data-ttu-id="53bc7-214">'Identity'</span><span class="sxs-lookup"><span data-stu-id="53bc7-214">'Identity'</span></span>
- <span data-ttu-id="53bc7-215">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="53bc7-215">'Let's Encrypt'</span></span>
- <span data-ttu-id="53bc7-216">'Razor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-216">'Razor'</span></span>
- <span data-ttu-id="53bc7-217">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="53bc7-217">'SignalR' uid:</span></span> 

-
<span data-ttu-id="53bc7-218">標題：將 ASP.NET Core Razor 元件整合至 Razor 頁面和 MVC 應用程式的作者：描述：「瞭解應用程式中元件和 DOM 元素的資料系結案例」 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="53bc7-218">title: 'Integrate ASP.NET Core Razor components into Razor Pages and MVC apps' author: description: 'Learn about data binding scenarios for components and DOM elements in Blazor apps.'</span></span>
<span data-ttu-id="53bc7-219">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="53bc7-219">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="53bc7-220">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-220">'Blazor'</span></span>
- <span data-ttu-id="53bc7-221">'Identity'</span><span class="sxs-lookup"><span data-stu-id="53bc7-221">'Identity'</span></span>
- <span data-ttu-id="53bc7-222">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="53bc7-222">'Let's Encrypt'</span></span>
- <span data-ttu-id="53bc7-223">'Razor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-223">'Razor'</span></span>
- <span data-ttu-id="53bc7-224">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="53bc7-224">'SignalR' uid:</span></span> 

-
<span data-ttu-id="53bc7-225">標題：將 ASP.NET Core Razor 元件整合至 Razor 頁面和 MVC 應用程式的作者：描述：「瞭解應用程式中元件和 DOM 元素的資料系結案例」 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="53bc7-225">title: 'Integrate ASP.NET Core Razor components into Razor Pages and MVC apps' author: description: 'Learn about data binding scenarios for components and DOM elements in Blazor apps.'</span></span>
<span data-ttu-id="53bc7-226">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="53bc7-226">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="53bc7-227">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-227">'Blazor'</span></span>
- <span data-ttu-id="53bc7-228">'Identity'</span><span class="sxs-lookup"><span data-stu-id="53bc7-228">'Identity'</span></span>
- <span data-ttu-id="53bc7-229">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="53bc7-229">'Let's Encrypt'</span></span>
- <span data-ttu-id="53bc7-230">'Razor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-230">'Razor'</span></span>
- <span data-ttu-id="53bc7-231">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="53bc7-231">'SignalR' uid:</span></span> 

<span data-ttu-id="53bc7-232">------ |---標題：「將 ASP.NET Core Razor 元件整合至 Razor 頁面和 MVC 應用程式的作者：描述：」瞭解應用程式中元件和 DOM 元素的資料系結案例 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="53bc7-232">------ | --- title: 'Integrate ASP.NET Core Razor components into Razor Pages and MVC apps' author: description: 'Learn about data binding scenarios for components and DOM elements in Blazor apps.'</span></span>
<span data-ttu-id="53bc7-233">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="53bc7-233">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="53bc7-234">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-234">'Blazor'</span></span>
- <span data-ttu-id="53bc7-235">'Identity'</span><span class="sxs-lookup"><span data-stu-id="53bc7-235">'Identity'</span></span>
- <span data-ttu-id="53bc7-236">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="53bc7-236">'Let's Encrypt'</span></span>
- <span data-ttu-id="53bc7-237">'Razor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-237">'Razor'</span></span>
- <span data-ttu-id="53bc7-238">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="53bc7-238">'SignalR' uid:</span></span> 

-
<span data-ttu-id="53bc7-239">標題：將 ASP.NET Core Razor 元件整合至 Razor 頁面和 MVC 應用程式的作者：描述：「瞭解應用程式中元件和 DOM 元素的資料系結案例」 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="53bc7-239">title: 'Integrate ASP.NET Core Razor components into Razor Pages and MVC apps' author: description: 'Learn about data binding scenarios for components and DOM elements in Blazor apps.'</span></span>
<span data-ttu-id="53bc7-240">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="53bc7-240">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="53bc7-241">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-241">'Blazor'</span></span>
- <span data-ttu-id="53bc7-242">'Identity'</span><span class="sxs-lookup"><span data-stu-id="53bc7-242">'Identity'</span></span>
- <span data-ttu-id="53bc7-243">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="53bc7-243">'Let's Encrypt'</span></span>
- <span data-ttu-id="53bc7-244">'Razor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-244">'Razor'</span></span>
- <span data-ttu-id="53bc7-245">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="53bc7-245">'SignalR' uid:</span></span> 

-
<span data-ttu-id="53bc7-246">標題：將 ASP.NET Core Razor 元件整合至 Razor 頁面和 MVC 應用程式的作者：描述：「瞭解應用程式中元件和 DOM 元素的資料系結案例」 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="53bc7-246">title: 'Integrate ASP.NET Core Razor components into Razor Pages and MVC apps' author: description: 'Learn about data binding scenarios for components and DOM elements in Blazor apps.'</span></span>
<span data-ttu-id="53bc7-247">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="53bc7-247">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="53bc7-248">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-248">'Blazor'</span></span>
- <span data-ttu-id="53bc7-249">'Identity'</span><span class="sxs-lookup"><span data-stu-id="53bc7-249">'Identity'</span></span>
- <span data-ttu-id="53bc7-250">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="53bc7-250">'Let's Encrypt'</span></span>
- <span data-ttu-id="53bc7-251">'Razor'</span><span class="sxs-lookup"><span data-stu-id="53bc7-251">'Razor'</span></span>
- <span data-ttu-id="53bc7-252">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="53bc7-252">'SignalR' uid:</span></span> 

<span data-ttu-id="53bc7-253">------ | |<xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> |將 `App` 元件轉譯為靜態 HTML，並包含 Blazor 伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="53bc7-253">------ | | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | Renders the `App` component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="53bc7-254">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="53bc7-254">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="53bc7-255">| |<xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> |呈現 Blazor 伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="53bc7-255">| | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="53bc7-256">`App`不包含來自元件的輸出。</span><span class="sxs-lookup"><span data-stu-id="53bc7-256">Output from the `App` component isn't included.</span></span> <span data-ttu-id="53bc7-257">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="53bc7-257">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="53bc7-258">| |<xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> |將 `App` 元件轉譯為靜態 HTML。</span><span class="sxs-lookup"><span data-stu-id="53bc7-258">| | <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | Renders the `App` component into static HTML.</span></span> |

   <span data-ttu-id="53bc7-259">如需元件標記協助程式的詳細資訊，請參閱 <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper> 。</span><span class="sxs-lookup"><span data-stu-id="53bc7-259">For more information on the Component Tag Helper, see <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>.</span></span>

1. <span data-ttu-id="53bc7-260">將動作新增至主控制器：</span><span class="sxs-lookup"><span data-stu-id="53bc7-260">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="53bc7-261">為控制器動作新增低優先順序的路由，以將 *_Host. cshtml*視圖傳回至中的端點設定 `Startup.Configure` ：</span><span class="sxs-lookup"><span data-stu-id="53bc7-261">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="53bc7-262">建立*Pages*資料夾，並將可路由的元件新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="53bc7-262">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="53bc7-263">例如：</span><span class="sxs-lookup"><span data-stu-id="53bc7-263">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

<span data-ttu-id="53bc7-264">如需命名空間的詳細資訊，請參閱[元件命名空間](#component-namespaces)一節。</span><span class="sxs-lookup"><span data-stu-id="53bc7-264">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="render-components-from-a-page-or-view"></a><span data-ttu-id="53bc7-265">從頁面或視圖呈現元件</span><span class="sxs-lookup"><span data-stu-id="53bc7-265">Render components from a page or view</span></span>

<span data-ttu-id="53bc7-266">*本節適用于將元件新增至頁面或視圖，其中元件無法直接從使用者要求路由傳送。*</span><span class="sxs-lookup"><span data-stu-id="53bc7-266">*This section pertains to adding components to pages or views, where the components aren't directly routable from user requests.*</span></span>

<span data-ttu-id="53bc7-267">若要從頁面或視圖呈現元件，請使用[元件標記](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper)協助程式。</span><span class="sxs-lookup"><span data-stu-id="53bc7-267">To render a component from a page or view, use the [Component Tag Helper](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper).</span></span>

### <a name="render-stateful-interactive-components"></a><span data-ttu-id="53bc7-268">呈現具狀態的互動式元件</span><span class="sxs-lookup"><span data-stu-id="53bc7-268">Render stateful interactive components</span></span>

<span data-ttu-id="53bc7-269">可設定狀態的互動式元件可以新增至 Razor 頁面或視圖。</span><span class="sxs-lookup"><span data-stu-id="53bc7-269">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="53bc7-270">當頁面或視圖呈現時：</span><span class="sxs-lookup"><span data-stu-id="53bc7-270">When the page or view renders:</span></span>

* <span data-ttu-id="53bc7-271">此元件是使用頁面或視圖所資源清單。</span><span class="sxs-lookup"><span data-stu-id="53bc7-271">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="53bc7-272">用來進行預呈現的初始元件狀態會遺失。</span><span class="sxs-lookup"><span data-stu-id="53bc7-272">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="53bc7-273">建立連接時，會建立新的元件狀態 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="53bc7-273">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="53bc7-274">下列 Razor 頁面會呈現 `Counter` 元件：</span><span class="sxs-lookup"><span data-stu-id="53bc7-274">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@functions {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

<span data-ttu-id="53bc7-275">如需詳細資訊，請參閱<xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>。</span><span class="sxs-lookup"><span data-stu-id="53bc7-275">For more information, see <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>.</span></span>

### <a name="render-noninteractive-components"></a><span data-ttu-id="53bc7-276">呈現非互動式元件</span><span class="sxs-lookup"><span data-stu-id="53bc7-276">Render noninteractive components</span></span>

<span data-ttu-id="53bc7-277">在下列 Razor 頁面中， `Counter` 元件會使用以表單指定的初始值，以靜態方式呈現。</span><span class="sxs-lookup"><span data-stu-id="53bc7-277">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form.</span></span> <span data-ttu-id="53bc7-278">由於元件是以靜態方式呈現，因此元件不是互動式的：</span><span class="sxs-lookup"><span data-stu-id="53bc7-278">Since the component is statically rendered, the component isn't interactive:</span></span>

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

<span data-ttu-id="53bc7-279">如需詳細資訊，請參閱<xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>。</span><span class="sxs-lookup"><span data-stu-id="53bc7-279">For more information, see <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>.</span></span>

## <a name="component-namespaces"></a><span data-ttu-id="53bc7-280">元件命名空間</span><span class="sxs-lookup"><span data-stu-id="53bc7-280">Component namespaces</span></span>

<span data-ttu-id="53bc7-281">使用自訂資料夾來存放應用程式的元件時，請將代表資料夾的命名空間新增至頁面/視圖，或加入至 *_ViewImports. cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="53bc7-281">When using a custom folder to hold the app's components, add the namespace representing the folder to either the page/view or to the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="53bc7-282">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="53bc7-282">In the following example:</span></span>

* <span data-ttu-id="53bc7-283">變更 `MyAppNamespace` 為應用程式的命名空間。</span><span class="sxs-lookup"><span data-stu-id="53bc7-283">Change `MyAppNamespace` to the app's namespace.</span></span>
* <span data-ttu-id="53bc7-284">如果未使用名為「*元件*」的資料夾來存放元件，請變更 `Components` 為元件所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="53bc7-284">If a folder named *Components* isn't used to hold the components, change `Components` to the folder where the components reside.</span></span>

```cshtml
@using MyAppNamespace.Components
```

<span data-ttu-id="53bc7-285">*_ViewImports. cshtml*檔案位於*Pages* Razor MVC 應用程式的 pages 應用程式或*Views*資料夾的 pages 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="53bc7-285">The *_ViewImports.cshtml* file is located in the *Pages* folder of a Razor Pages app or the *Views* folder of an MVC app.</span></span>

<span data-ttu-id="53bc7-286">如需詳細資訊，請參閱<xref:blazor/components#import-components>。</span><span class="sxs-lookup"><span data-stu-id="53bc7-286">For more information, see <xref:blazor/components#import-components>.</span></span>
