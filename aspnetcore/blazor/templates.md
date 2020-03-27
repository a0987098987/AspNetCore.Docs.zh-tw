---
title: ASP.NET Core Blazor 範本
author: guardrex
description: 深入瞭解 ASP.NET Core Blazor 應用程式範本和 Blazor 專案結構。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: blazor/templates
ms.openlocfilehash: 71a9d9eee8637dda0b3cecac82ff96a0c3bfedb5
ms.sourcegitcommit: f3b1bcfd108e5d53f73abc0bf2555890869d953b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80320986"
---
# <a name="aspnet-core-opno-locblazor-templates"></a><span data-ttu-id="3cc5e-103">ASP.NET Core Blazor 範本</span><span class="sxs-lookup"><span data-stu-id="3cc5e-103">ASP.NET Core Blazor templates</span></span>

<span data-ttu-id="3cc5e-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3cc5e-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="3cc5e-105">Blazor 架構會提供範本來為每個 Blazor 裝載模型開發應用程式：</span><span class="sxs-lookup"><span data-stu-id="3cc5e-105">The Blazor framework provides templates to develop apps for each of the Blazor hosting models:</span></span>

* Blazor<span data-ttu-id="3cc5e-106"> WebAssembly （`blazorwasm`）</span><span class="sxs-lookup"><span data-stu-id="3cc5e-106"> WebAssembly (`blazorwasm`)</span></span>
* Blazor<span data-ttu-id="3cc5e-107"> Server （`blazorserver`）</span><span class="sxs-lookup"><span data-stu-id="3cc5e-107"> Server (`blazorserver`)</span></span>

<span data-ttu-id="3cc5e-108">如需 Blazor裝載模型的詳細資訊，請參閱 <xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-108">For more information on Blazor's hosting models, see <xref:blazor/hosting-models>.</span></span>

<span data-ttu-id="3cc5e-109">如需從範本建立 Blazor 應用程式的逐步指示，請參閱 <xref:blazor/get-started>。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-109">For step-by-step instructions on creating a Blazor app from a template, see <xref:blazor/get-started>.</span></span>

## <a name="opno-locblazor-project-structure"></a>Blazor<span data-ttu-id="3cc5e-110"> 專案結構</span><span class="sxs-lookup"><span data-stu-id="3cc5e-110"> project structure</span></span>

<span data-ttu-id="3cc5e-111">下列檔案和資料夾組成從 Blazor 範本產生的 Blazor 應用程式：</span><span class="sxs-lookup"><span data-stu-id="3cc5e-111">The following files and folders make up a Blazor app generated from a Blazor template:</span></span>

* <span data-ttu-id="3cc5e-112">*Program.cs* &ndash; 應用程式的進入點來設定：</span><span class="sxs-lookup"><span data-stu-id="3cc5e-112">*Program.cs* &ndash; The app's entry point that sets up the:</span></span>

  * <span data-ttu-id="3cc5e-113">ASP.NET Core[主機](xref:fundamentals/host/generic-host)（Blazor 伺服器）</span><span class="sxs-lookup"><span data-stu-id="3cc5e-113">ASP.NET Core [host](xref:fundamentals/host/generic-host) (Blazor Server)</span></span>
  * <span data-ttu-id="3cc5e-114">WebAssembly 主機（Blazor WebAssembly） &ndash; 此檔案中的程式碼對於從 Blazor WebAssembly 範本（`blazorwasm`）所建立的應用程式而言是唯一的。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-114">WebAssembly host (Blazor WebAssembly) &ndash; The code in this file is unique to apps created from the Blazor WebAssembly template (`blazorwasm`).</span></span>
    * <span data-ttu-id="3cc5e-115">`App` 元件（也就是應用程式的根元件）會指定為 `Add` 方法的 `app` DOM 元素。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-115">The `App` component, which is the root component of the app, is specified as the `app` DOM element to the `Add` method.</span></span>
    * <span data-ttu-id="3cc5e-116">服務可以使用主機產生器上的 `ConfigureServices` 方法來設定（例如，`builder.Services.AddSingleton<IMyDependency, MyDependency>();`）。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-116">Services can be configured with the `ConfigureServices` method on the host builder (for example, `builder.Services.AddSingleton<IMyDependency, MyDependency>();`).</span></span>
    * <span data-ttu-id="3cc5e-117">您可以透過主機產生器（`builder.Configuration`）來提供設定。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-117">Configuration can be supplied via the host builder (`builder.Configuration`).</span></span>

* <span data-ttu-id="3cc5e-118">*Startup.cs* （Blazor Server） &ndash; 包含應用程式的啟動邏輯。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-118">*Startup.cs* (Blazor Server) &ndash; Contains the app's startup logic.</span></span> <span data-ttu-id="3cc5e-119">`Startup` 類別會定義兩個方法：</span><span class="sxs-lookup"><span data-stu-id="3cc5e-119">The `Startup` class defines two methods:</span></span>

  * <span data-ttu-id="3cc5e-120">`ConfigureServices` &ndash; 會設定應用程式的相依性[插入（DI）](xref:fundamentals/dependency-injection)服務。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-120">`ConfigureServices` &ndash; Configures the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) services.</span></span> <span data-ttu-id="3cc5e-121">在 Blazor 伺服器應用程式中，會藉由呼叫 <xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor*>來新增服務，並將 `WeatherForecastService` 新增至服務容器，以供範例 `FetchData` 元件使用。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-121">In Blazor Server apps, services are added by calling <xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor*>, and the `WeatherForecastService` is added to the service container for use by the example `FetchData` component.</span></span>
  * <span data-ttu-id="3cc5e-122">`Configure` &ndash; 設定應用程式的要求處理管線：</span><span class="sxs-lookup"><span data-stu-id="3cc5e-122">`Configure` &ndash; Configures the app's request handling pipeline:</span></span>
    * <span data-ttu-id="3cc5e-123">呼叫 <xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub*> 來設定端點，以與瀏覽器進行即時連接。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-123"><xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub*> is called to set up an endpoint for the real-time connection with the browser.</span></span> <span data-ttu-id="3cc5e-124">連接是使用[SignalR](xref:signalr/introduction)建立的，這是將即時 web 功能新增至應用程式的架構。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-124">The connection is created with [SignalR](xref:signalr/introduction), which is a framework for adding real-time web functionality to apps.</span></span>
    * <span data-ttu-id="3cc5e-125">呼叫[MapFallbackToPage （"/_Host"）](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*)來設定應用程式的根頁面（*Pages/_Host. cshtml*）並啟用導覽。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-125">[MapFallbackToPage("/_Host")](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*) is called to set up the root page of the app (*Pages/_Host.cshtml*) and enable navigation.</span></span>

* <span data-ttu-id="3cc5e-126">*wwwroot/index.html* （Blazor WebAssembly） &ndash; 實作為 html 網頁之應用程式的根頁面：</span><span class="sxs-lookup"><span data-stu-id="3cc5e-126">*wwwroot/index.html* (Blazor WebAssembly) &ndash; The root page of the app implemented as an HTML page:</span></span>
  * <span data-ttu-id="3cc5e-127">一開始要求應用程式的任何頁面時，會轉譯此頁面並在回應中傳回。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-127">When any page of the app is initially requested, this page is rendered and returned in the response.</span></span>
  * <span data-ttu-id="3cc5e-128">此頁面會指定呈現根 `App` 元件的位置。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-128">The page specifies where the root `App` component is rendered.</span></span> <span data-ttu-id="3cc5e-129">`App` 元件（*app.config*）會指定為 `Startup.Configure`中 `AddComponent` 方法的 `app` DOM 元素。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-129">The `App` component (*App.razor*) is specified as the `app` DOM element to the `AddComponent` method in `Startup.Configure`.</span></span>
  * <span data-ttu-id="3cc5e-130">載入 `_framework/blazor.webassembly.js` JavaScript 檔案，其：</span><span class="sxs-lookup"><span data-stu-id="3cc5e-130">The `_framework/blazor.webassembly.js` JavaScript file is loaded, which:</span></span>
    * <span data-ttu-id="3cc5e-131">下載 .NET 執行時間、應用程式和應用程式的相依性。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-131">Downloads the .NET runtime, the app, and the app's dependencies.</span></span>
    * <span data-ttu-id="3cc5e-132">初始化執行時間以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-132">Initializes the runtime to run the app.</span></span>

* <span data-ttu-id="3cc5e-133">*App.config* &ndash; 應用程式的根元件，其會使用 <xref:Microsoft.AspNetCore.Components.Routing.Router> 元件來設定用戶端路由。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-133">*App.razor* &ndash; The root component of the app that sets up client-side routing using the <xref:Microsoft.AspNetCore.Components.Routing.Router> component.</span></span> <span data-ttu-id="3cc5e-134">`Router` 元件會攔截瀏覽器導覽，並呈現符合所要求位址的頁面。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-134">The `Router` component intercepts browser navigation and renders the page that matches the requested address.</span></span>

* <span data-ttu-id="3cc5e-135">*Pages*資料夾 &ndash; 包含組成 Blazor 應用程式的可路由元件/頁面（*razor*），以及 Blazor 伺服器應用程式的根 razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-135">*Pages* folder &ndash; Contains the routable components/pages (*.razor*) that make up the Blazor app and the root Razor page of a Blazor Server app.</span></span> <span data-ttu-id="3cc5e-136">每個頁面的路由都是使用[`@page`](xref:mvc/views/razor#page)指示詞來指定。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-136">The route for each page is specified using the [`@page`](xref:mvc/views/razor#page) directive.</span></span> <span data-ttu-id="3cc5e-137">此範本包含下列各項：</span><span class="sxs-lookup"><span data-stu-id="3cc5e-137">The template includes the following:</span></span>
  * <span data-ttu-id="3cc5e-138">*_Host. cshtml* （Blazor 伺服器） &ndash; 實作為 Razor 頁面的應用程式的根頁面：</span><span class="sxs-lookup"><span data-stu-id="3cc5e-138">*_Host.cshtml* (Blazor Server) &ndash; The root page of the app implemented as a Razor Page:</span></span>
    * <span data-ttu-id="3cc5e-139">一開始要求應用程式的任何頁面時，會轉譯此頁面並在回應中傳回。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-139">When any page of the app is initially requested, this page is rendered and returned in the response.</span></span>
    * <span data-ttu-id="3cc5e-140">載入 `_framework/blazor.server.js` JavaScript 檔案，這會在瀏覽器與伺服器之間設定即時 SignalR 連接。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-140">The `_framework/blazor.server.js` JavaScript file is loaded, which sets up the real-time SignalR connection between the browser and the server.</span></span>
    * <span data-ttu-id="3cc5e-141">[主機] 頁面會指定要呈現根 `App` 元件（*razor*）的位置。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-141">The Host page specifies where the root `App` component (*App.razor*) is rendered.</span></span>
  * <span data-ttu-id="3cc5e-142">`Counter` （*razor*） &ndash; 會實行 [計數器] 頁面。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-142">`Counter` (*Counter.razor*) &ndash; Implements the Counter page.</span></span>
  * <span data-ttu-id="3cc5e-143">`Error` （只有在應用程式中發生未處理的例外狀況時，才會轉譯） &ndash; 轉譯的*錯誤（razor*、Blazor 伺服器應用程式）。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-143">`Error` (*Error.razor*, Blazor Server app only) &ndash; Rendered when an unhandled exception occurs in the app.</span></span>
  * <span data-ttu-id="3cc5e-144">`FetchData` （*FetchData razor*） &ndash; 會執行提取資料頁面。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-144">`FetchData` (*FetchData.razor*) &ndash; Implements the Fetch data page.</span></span>
  * <span data-ttu-id="3cc5e-145">`Index` （*Index. razor*） &ndash; 會執行首頁。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-145">`Index` (*Index.razor*) &ndash; Implements the Home page.</span></span>

* <span data-ttu-id="3cc5e-146">*共用*資料夾 &ndash; 包含應用程式所使用的其他 UI 元件（*razor*）：</span><span class="sxs-lookup"><span data-stu-id="3cc5e-146">*Shared* folder &ndash; Contains other UI components (*.razor*) used by the app:</span></span>
  * <span data-ttu-id="3cc5e-147">`MainLayout` （*MainLayout razor*） &ndash; 應用程式的版面配置元件。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-147">`MainLayout` (*MainLayout.razor*) &ndash; The app's layout component.</span></span>
  * <span data-ttu-id="3cc5e-148">`NavMenu` （*navmenu.cshtml razor*） &ndash; 會執行提要欄位導覽。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-148">`NavMenu` (*NavMenu.razor*) &ndash; Implements sidebar navigation.</span></span> <span data-ttu-id="3cc5e-149">包含[NavLink 元件](xref:blazor/routing#navlink-component)（<xref:Microsoft.AspNetCore.Components.Routing.NavLink>），它會呈現其他 Razor 元件的導覽連結。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-149">Includes the [NavLink component](xref:blazor/routing#navlink-component) (<xref:Microsoft.AspNetCore.Components.Routing.NavLink>), which renders navigation links to other Razor components.</span></span> <span data-ttu-id="3cc5e-150">[`NavLink`] 元件會在載入元件時自動指出選取的狀態，協助使用者瞭解目前顯示的元件。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-150">The `NavLink` component automatically indicates a selected state when its component is loaded, which helps the user understand which component is currently displayed.</span></span>

* <span data-ttu-id="3cc5e-151">*_Imports razor* &ndash; 包含常見的 razor 指示詞，以包含在應用程式的元件（*razor*）中，例如命名空間的[`@using`](xref:mvc/views/razor#using)指示詞。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-151">*_Imports.razor* &ndash; Includes common Razor directives to include in the app's components (*.razor*), such as [`@using`](xref:mvc/views/razor#using) directives for namespaces.</span></span>

* <span data-ttu-id="3cc5e-152">*資料*資料夾（Blazor Server） &ndash; 包含 `WeatherForecastService` 的 `WeatherForecast` 類別和執行，可提供範例天氣資料給應用程式的 `FetchData` 元件。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-152">*Data* folder (Blazor Server) &ndash; Contains the `WeatherForecast` class and implementation of the `WeatherForecastService` that provide example weather data to the app's `FetchData` component.</span></span>

* <span data-ttu-id="3cc5e-153">*wwwroot* &ndash; 應用程式的[Web 根](xref:fundamentals/index#web-root)資料夾，其中包含應用程式的公用靜態資產。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-153">*wwwroot* &ndash; The [Web Root](xref:fundamentals/index#web-root) folder for the app containing the app's public static assets.</span></span>

* <span data-ttu-id="3cc5e-154">應用程式的*appsettings* （Blazor Server） &ndash; 設定。</span><span class="sxs-lookup"><span data-stu-id="3cc5e-154">*appsettings.json* (Blazor Server) &ndash; Configuration settings for the app.</span></span>
