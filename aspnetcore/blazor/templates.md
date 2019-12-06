---
title: ASP.NET Core Blazor 範本
author: guardrex
description: 深入瞭解 ASP.NET Core Blazor 應用程式範本和 Blazor 專案結構。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/25/2019
no-loc:
- Blazor
- SignalR
uid: blazor/templates
ms.openlocfilehash: bc0ea4a777e8684a7b0925377b8a19a45c2b531c
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/06/2019
ms.locfileid: "74879663"
---
# <a name="aspnet-core-opno-locblazor-templates"></a><span data-ttu-id="24258-103">ASP.NET Core Blazor 範本</span><span class="sxs-lookup"><span data-stu-id="24258-103">ASP.NET Core Blazor templates</span></span>

<span data-ttu-id="24258-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="24258-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="24258-105">Blazor 架構會提供範本來為每個 Blazor 裝載模型開發應用程式：</span><span class="sxs-lookup"><span data-stu-id="24258-105">The Blazor framework provides templates to develop apps for each of the Blazor hosting models:</span></span>

* Blazor<span data-ttu-id="24258-106"> WebAssembly （`blazorwasm`）</span><span class="sxs-lookup"><span data-stu-id="24258-106"> WebAssembly (`blazorwasm`)</span></span>
* Blazor<span data-ttu-id="24258-107"> Server （`blazorserver`）</span><span class="sxs-lookup"><span data-stu-id="24258-107"> Server (`blazorserver`)</span></span>

<span data-ttu-id="24258-108">如需 Blazor裝載模型的詳細資訊，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="24258-108">For more information on Blazor's hosting models, see <xref:blazor/hosting-models>.</span></span>

<span data-ttu-id="24258-109">如需從範本建立 Blazor 應用程式的逐步指示，請參閱 <xref:blazor/get-started>。</span><span class="sxs-lookup"><span data-stu-id="24258-109">For step-by-step instructions on creating a Blazor app from a template, see <xref:blazor/get-started>.</span></span>

## <a name="opno-locblazor-project-structure"></a>Blazor<span data-ttu-id="24258-110"> 專案結構</span><span class="sxs-lookup"><span data-stu-id="24258-110"> project structure</span></span>

<span data-ttu-id="24258-111">下列檔案和資料夾組成從 Blazor 範本產生的 Blazor 應用程式：</span><span class="sxs-lookup"><span data-stu-id="24258-111">The following files and folders make up a Blazor app generated from a Blazor template:</span></span>

* <span data-ttu-id="24258-112">*Program.cs* &ndash; 設定 ASP.NET Core[主機](xref:fundamentals/host/generic-host)的應用程式進入點。</span><span class="sxs-lookup"><span data-stu-id="24258-112">*Program.cs* &ndash; The app's entry point that sets up the ASP.NET Core [host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="24258-113">此檔案中的程式碼通用於從 ASP.NET Core 範本產生的所有 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="24258-113">The code in this file is common to all ASP.NET Core apps generated from ASP.NET Core templates.</span></span>

* <span data-ttu-id="24258-114">*Startup.cs* &ndash; 包含應用程式的啟動邏輯。</span><span class="sxs-lookup"><span data-stu-id="24258-114">*Startup.cs* &ndash; Contains the app's startup logic.</span></span> <span data-ttu-id="24258-115">`Startup` 類別會定義兩個方法：</span><span class="sxs-lookup"><span data-stu-id="24258-115">The `Startup` class defines two methods:</span></span>

  * <span data-ttu-id="24258-116">`ConfigureServices` &ndash; 會設定應用程式的相依性[插入（DI）](xref:fundamentals/dependency-injection)服務。</span><span class="sxs-lookup"><span data-stu-id="24258-116">`ConfigureServices` &ndash; Configures the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) services.</span></span> <span data-ttu-id="24258-117">在 Blazor 伺服器應用程式中，會藉由呼叫 <xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor*>來新增服務，並將 `WeatherForecastService` 新增至服務容器，以供範例 `FetchData` 元件使用。</span><span class="sxs-lookup"><span data-stu-id="24258-117">In Blazor Server apps, services are added by calling <xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor*>, and the `WeatherForecastService` is added to the service container for use by the example `FetchData` component.</span></span>
  * <span data-ttu-id="24258-118">`Configure` &ndash; 設定應用程式的要求處理管線：</span><span class="sxs-lookup"><span data-stu-id="24258-118">`Configure` &ndash; Configures the app's request handling pipeline:</span></span>
    * Blazor<span data-ttu-id="24258-119"> WebAssembly &ndash; 會將 `App` 元件（指定為 `app` DOM 元素）新增至 `AddComponent` 方法），也就是應用程式的根元件。</span><span class="sxs-lookup"><span data-stu-id="24258-119"> WebAssembly &ndash; Adds the `App` component (specified as the `app` DOM element to the `AddComponent` method), which is the root component of the app.</span></span>
    * Blazor<span data-ttu-id="24258-120"> 伺服器</span><span class="sxs-lookup"><span data-stu-id="24258-120"> Server</span></span>
      * <span data-ttu-id="24258-121">呼叫 <xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub*> 來設定端點，以與瀏覽器進行即時連接。</span><span class="sxs-lookup"><span data-stu-id="24258-121"><xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub*> is called to set up an endpoint for the real-time connection with the browser.</span></span> <span data-ttu-id="24258-122">連接是使用[SignalR](xref:signalr/introduction)建立的，這是將即時 web 功能新增至應用程式的架構。</span><span class="sxs-lookup"><span data-stu-id="24258-122">The connection is created with [SignalR](xref:signalr/introduction), which is a framework for adding real-time web functionality to apps.</span></span>
      * <span data-ttu-id="24258-123">呼叫[MapFallbackToPage （"/_Host"）](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*)來設定應用程式的根頁面（*Pages/_Host. cshtml*）並啟用導覽。</span><span class="sxs-lookup"><span data-stu-id="24258-123">[MapFallbackToPage("/_Host")](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*) is called to set up the root page of the app (*Pages/_Host.cshtml*) and enable navigation.</span></span>

* <span data-ttu-id="24258-124">*wwwroot/index.html* （Blazor WebAssembly） &ndash; 實作為 html 網頁之應用程式的根頁面：</span><span class="sxs-lookup"><span data-stu-id="24258-124">*wwwroot/index.html* (Blazor WebAssembly) &ndash; The root page of the app implemented as an HTML page:</span></span>
  * <span data-ttu-id="24258-125">一開始要求應用程式的任何頁面時，會轉譯此頁面並在回應中傳回。</span><span class="sxs-lookup"><span data-stu-id="24258-125">When any page of the app is initially requested, this page is rendered and returned in the response.</span></span>
  * <span data-ttu-id="24258-126">此頁面會指定呈現根 `App` 元件的位置。</span><span class="sxs-lookup"><span data-stu-id="24258-126">The page specifies where the root `App` component is rendered.</span></span> <span data-ttu-id="24258-127">`App` 元件（*app.config*）會指定為 `Startup.Configure`中 `AddComponent` 方法的 `app` DOM 元素。</span><span class="sxs-lookup"><span data-stu-id="24258-127">The `App` component (*App.razor*) is specified as the `app` DOM element to the `AddComponent` method in `Startup.Configure`.</span></span>
  * <span data-ttu-id="24258-128">*_Framework/Blazor.webassembly.js* JavaScript 檔案已載入，其：</span><span class="sxs-lookup"><span data-stu-id="24258-128">The *_framework/blazor.webassembly.js* JavaScript file is loaded, which:</span></span>
    * <span data-ttu-id="24258-129">下載 .NET 執行時間、應用程式和應用程式的相依性。</span><span class="sxs-lookup"><span data-stu-id="24258-129">Downloads the .NET runtime, the app, and the app's dependencies.</span></span>
    * <span data-ttu-id="24258-130">初始化執行時間以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="24258-130">Initializes the runtime to run the app.</span></span>

* <span data-ttu-id="24258-131">*Pages/_Host. cshtml* （Blazor Server） &ndash; 實作為 Razor 頁面的應用程式的根頁面：</span><span class="sxs-lookup"><span data-stu-id="24258-131">*Pages/_Host.cshtml* (Blazor Server) &ndash; The root page of the app implemented as a Razor Page:</span></span>
  * <span data-ttu-id="24258-132">一開始要求應用程式的任何頁面時，會轉譯此頁面並在回應中傳回。</span><span class="sxs-lookup"><span data-stu-id="24258-132">When any page of the app is initially requested, this page is rendered and returned in the response.</span></span>
  * <span data-ttu-id="24258-133">*_Framework/Blazor.server.js* JavaScript 檔案已載入，這會在瀏覽器與伺服器之間設定即時 SignalR 連線。</span><span class="sxs-lookup"><span data-stu-id="24258-133">The *_framework/blazor.server.js* JavaScript file is loaded, which sets up the real-time SignalR connection between the browser and the server.</span></span>
  * <span data-ttu-id="24258-134">[主機] 頁面會指定要呈現根 `App` 元件（*razor*）的位置。</span><span class="sxs-lookup"><span data-stu-id="24258-134">The Host page specifies where the root `App` component (*App.razor*) is rendered.</span></span>

* <span data-ttu-id="24258-135">*App.config* &ndash; 應用程式的根元件，其會使用 <xref:Microsoft.AspNetCore.Components.Routing.Router> 元件來設定用戶端路由。</span><span class="sxs-lookup"><span data-stu-id="24258-135">*App.razor* &ndash; The root component of the app that sets up client-side routing using the <xref:Microsoft.AspNetCore.Components.Routing.Router> component.</span></span> <span data-ttu-id="24258-136">`Router` 元件會攔截瀏覽器導覽，並呈現符合所要求位址的頁面。</span><span class="sxs-lookup"><span data-stu-id="24258-136">The `Router` component intercepts browser navigation and renders the page that matches the requested address.</span></span>

* <span data-ttu-id="24258-137">*Pages*資料夾 &ndash; 包含組成 Blazor 應用程式的可路由元件/頁面（*razor*）。</span><span class="sxs-lookup"><span data-stu-id="24258-137">*Pages* folder &ndash; Contains the routable components/pages (*.razor*) that make up the Blazor app.</span></span> <span data-ttu-id="24258-138">每個頁面的路由都是使用[`@page`](xref:mvc/views/razor#page)指示詞來指定。</span><span class="sxs-lookup"><span data-stu-id="24258-138">The route for each page is specified using the [`@page`](xref:mvc/views/razor#page) directive.</span></span> <span data-ttu-id="24258-139">此範本包含下列元件：</span><span class="sxs-lookup"><span data-stu-id="24258-139">The template includes the following components:</span></span>
  * <span data-ttu-id="24258-140">`Index` （*Index. razor*） &ndash; 會執行首頁。</span><span class="sxs-lookup"><span data-stu-id="24258-140">`Index` (*Index.razor*) &ndash; Implements the Home page.</span></span>
  * <span data-ttu-id="24258-141">`Counter` （*razor*） &ndash; 會實行 [計數器] 頁面。</span><span class="sxs-lookup"><span data-stu-id="24258-141">`Counter` (*Counter.razor*) &ndash; Implements the Counter page.</span></span>
  * <span data-ttu-id="24258-142">`Error` （只有在應用程式中發生未處理的例外狀況時，才會轉譯） &ndash; 轉譯的*錯誤（razor*、Blazor 伺服器應用程式）。</span><span class="sxs-lookup"><span data-stu-id="24258-142">`Error` (*Error.razor*, Blazor Server app only) &ndash; Rendered when an unhandled exception occurs in the app.</span></span>
  * <span data-ttu-id="24258-143">`FetchData` （*FetchData razor*） &ndash; 會執行提取資料頁面。</span><span class="sxs-lookup"><span data-stu-id="24258-143">`FetchData` (*FetchData.razor*) &ndash; Implements the Fetch data page.</span></span>

* <span data-ttu-id="24258-144">*共用*資料夾 &ndash; 包含應用程式所使用的其他 UI 元件（*razor*）：</span><span class="sxs-lookup"><span data-stu-id="24258-144">*Shared* folder &ndash; Contains other UI components (*.razor*) used by the app:</span></span>
  * <span data-ttu-id="24258-145">`MainLayout` （*MainLayout razor*） &ndash; 應用程式的版面配置元件。</span><span class="sxs-lookup"><span data-stu-id="24258-145">`MainLayout` (*MainLayout.razor*) &ndash; The app's layout component.</span></span>
  * <span data-ttu-id="24258-146">`NavMenu` （*navmenu.cshtml razor*） &ndash; 會執行提要欄位導覽。</span><span class="sxs-lookup"><span data-stu-id="24258-146">`NavMenu` (*NavMenu.razor*) &ndash; Implements sidebar navigation.</span></span> <span data-ttu-id="24258-147">包含[NavLink 元件](xref:blazor/routing#navlink-component)（<xref:Microsoft.AspNetCore.Components.Routing.NavLink>），它會呈現其他 Razor 元件的導覽連結。</span><span class="sxs-lookup"><span data-stu-id="24258-147">Includes the [NavLink component](xref:blazor/routing#navlink-component) (<xref:Microsoft.AspNetCore.Components.Routing.NavLink>), which renders navigation links to other Razor components.</span></span> <span data-ttu-id="24258-148">[`NavLink`] 元件會在載入元件時自動指出選取的狀態，協助使用者瞭解目前顯示的元件。</span><span class="sxs-lookup"><span data-stu-id="24258-148">The `NavLink` component automatically indicates a selected state when its component is loaded, which helps the user understand which component is currently displayed.</span></span>

* <span data-ttu-id="24258-149">*_Imports razor* &ndash; 包含常見的 razor 指示詞，以包含在應用程式的元件（*razor*）中，例如命名空間的[`@using`](xref:mvc/views/razor#using)指示詞。</span><span class="sxs-lookup"><span data-stu-id="24258-149">*_Imports.razor* &ndash; Includes common Razor directives to include in the app's components (*.razor*), such as [`@using`](xref:mvc/views/razor#using) directives for namespaces.</span></span>

* <span data-ttu-id="24258-150">*資料*資料夾（Blazor Server） &ndash; 包含 `WeatherForecastService` 的 `WeatherForecast` 類別和執行，可提供範例天氣資料給應用程式的 `FetchData` 元件。</span><span class="sxs-lookup"><span data-stu-id="24258-150">*Data* folder (Blazor Server) &ndash; Contains the `WeatherForecast` class and implementation of the `WeatherForecastService` that provide example weather data to the app's `FetchData` component.</span></span>

* <span data-ttu-id="24258-151">*wwwroot* &ndash; 應用程式的[Web 根](xref:fundamentals/index#web-root)資料夾，其中包含應用程式的公用靜態資產。</span><span class="sxs-lookup"><span data-stu-id="24258-151">*wwwroot* &ndash; The [Web Root](xref:fundamentals/index#web-root) folder for the app containing the app's public static assets.</span></span>

* <span data-ttu-id="24258-152">應用程式的*appsettings* （Blazor Server） &ndash; 設定。</span><span class="sxs-lookup"><span data-stu-id="24258-152">*appsettings.json* (Blazor Server) &ndash; Configuration settings for the app.</span></span>
