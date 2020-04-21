---
title: ASP.NET核心Blazor範本
author: guardrex
description: 瞭解ASP.NET核心Blazor應用範本和Blazor專案結構。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/19/2020
no-loc:
- Blazor
- SignalR
uid: blazor/templates
ms.openlocfilehash: 0a4a508beeae3d7bc665372d925989aa4e34ad52
ms.sourcegitcommit: 5547d920f322e5a823575c031529e4755ab119de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/21/2020
ms.locfileid: "81661720"
---
# <a name="aspnet-core-opno-locblazor-templates"></a><span data-ttu-id="c0e9d-103">ASP.NET核心Blazor範本</span><span class="sxs-lookup"><span data-stu-id="c0e9d-103">ASP.NET Core Blazor templates</span></span>

<span data-ttu-id="c0e9d-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c0e9d-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="c0e9d-105">該Blazor框架提供樣本,用於為每個Blazor託管模型開發應用:</span><span class="sxs-lookup"><span data-stu-id="c0e9d-105">The Blazor framework provides templates to develop apps for each of the Blazor hosting models:</span></span>

* Blazor<span data-ttu-id="c0e9d-106">網路彙編`blazorwasm`( )</span><span class="sxs-lookup"><span data-stu-id="c0e9d-106"> WebAssembly (`blazorwasm`)</span></span>
* Blazor<span data-ttu-id="c0e9d-107">伺服器`blazorserver`( )</span><span class="sxs-lookup"><span data-stu-id="c0e9d-107"> Server (`blazorserver`)</span></span>

<span data-ttu-id="c0e9d-108">有關Blazor託管模型的詳細資訊,請參閱<xref:blazor/hosting-models>。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-108">For more information on Blazor's hosting models, see <xref:blazor/hosting-models>.</span></span>

<span data-ttu-id="c0e9d-109">有關從範本創建Blazor應用的分步說明,請參閱<xref:blazor/get-started>。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-109">For step-by-step instructions on creating a Blazor app from a template, see <xref:blazor/get-started>.</span></span>

<span data-ttu-id="c0e9d-110">樣本選項可透過選項`--help`傳遞給[dotnet 新](/dotnet/core/tools/dotnet-new)CLI 命令:</span><span class="sxs-lookup"><span data-stu-id="c0e9d-110">Template options are available by passing the `--help` option to the [dotnet new](/dotnet/core/tools/dotnet-new) CLI command:</span></span>

```dotnetcli
dotnet new blazorwasm --help
dotnet new blazorserver --help
```

## <a name="opno-locblazor-project-structure"></a>Blazor<span data-ttu-id="c0e9d-111">專案結構</span><span class="sxs-lookup"><span data-stu-id="c0e9d-111"> project structure</span></span>

<span data-ttu-id="c0e9d-112">以下檔案與資料夾構成從BlazorBlazor樣本產生的應用:</span><span class="sxs-lookup"><span data-stu-id="c0e9d-112">The following files and folders make up a Blazor app generated from a Blazor template:</span></span>

* <span data-ttu-id="c0e9d-113">*Program.cs*&ndash;設定以下功能的應用程式點:</span><span class="sxs-lookup"><span data-stu-id="c0e9d-113">*Program.cs* &ndash; The app's entry point that sets up the:</span></span>

  * <span data-ttu-id="c0e9d-114">ASP.NET核心[主機](xref:fundamentals/host/generic-host)(Blazor伺服器 )</span><span class="sxs-lookup"><span data-stu-id="c0e9d-114">ASP.NET Core [host](xref:fundamentals/host/generic-host) (Blazor Server)</span></span>
  * <span data-ttu-id="c0e9d-115">Web組裝主機Blazor&ndash;(WebAssembly) 此檔案中的代碼對於從 WebAssembly 範本Blazor()`blazorwasm`創建的應用是唯一的。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-115">WebAssembly host (Blazor WebAssembly) &ndash; The code in this file is unique to apps created from the Blazor WebAssembly template (`blazorwasm`).</span></span>
    * <span data-ttu-id="c0e9d-116">元件`App`(是應用的根元件)被指定為`app``Add`方法的 DOM 元素。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-116">The `App` component, which is the root component of the app, is specified as the `app` DOM element to the `Add` method.</span></span>
    * <span data-ttu-id="c0e9d-117">可以使用主機產生器上`ConfigureServices`的方法配置服務(例如`builder.Services.AddSingleton<IMyDependency, MyDependency>();`, 。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-117">Services can be configured with the `ConfigureServices` method on the host builder (for example, `builder.Services.AddSingleton<IMyDependency, MyDependency>();`).</span></span>
    * <span data-ttu-id="c0e9d-118">配置可以透過主機生成器`builder.Configuration`() 提供。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-118">Configuration can be supplied via the host builder (`builder.Configuration`).</span></span>

* <span data-ttu-id="c0e9d-119">*Startup.cs(*Blazor伺服器&ndash;) 包含應用程式的啟動邏輯。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-119">*Startup.cs* (Blazor Server) &ndash; Contains the app's startup logic.</span></span> <span data-ttu-id="c0e9d-120">該`Startup`類別定義兩種方法:</span><span class="sxs-lookup"><span data-stu-id="c0e9d-120">The `Startup` class defines two methods:</span></span>

  * <span data-ttu-id="c0e9d-121">`ConfigureServices`&ndash;配置應用的依賴[項注入 (DI)](xref:fundamentals/dependency-injection)服務。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-121">`ConfigureServices` &ndash; Configures the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) services.</span></span> <span data-ttu-id="c0e9d-122">在Blazor「伺服器」應用中,服務透過呼<xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor*>叫添加`WeatherForecastService`,並將添加到服務容器中,供範`FetchData`例元件使用。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-122">In Blazor Server apps, services are added by calling <xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor*>, and the `WeatherForecastService` is added to the service container for use by the example `FetchData` component.</span></span>
  * <span data-ttu-id="c0e9d-123">`Configure`&ndash;設定應用的要求處理導管:</span><span class="sxs-lookup"><span data-stu-id="c0e9d-123">`Configure` &ndash; Configures the app's request handling pipeline:</span></span>
    * <span data-ttu-id="c0e9d-124"><xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub*>調用 以設置與瀏覽器的即時連接的終結點。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-124"><xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub*> is called to set up an endpoint for the real-time connection with the browser.</span></span> <span data-ttu-id="c0e9d-125">該連接與[SignalR](xref:signalr/introduction)創建,這是一個框架,用於向應用添加即時 Web 功能。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-125">The connection is created with [SignalR](xref:signalr/introduction), which is a framework for adding real-time web functionality to apps.</span></span>
    * <span data-ttu-id="c0e9d-126">[MapFallbackToPage("/_Host")](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*)被調用來設置應用程式的根頁 (*頁面/ _Host.cshtml)* 並啟用導航。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-126">[MapFallbackToPage("/_Host")](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*) is called to set up the root page of the app (*Pages/_Host.cshtml*) and enable navigation.</span></span>

* <span data-ttu-id="c0e9d-127">*wwwroot/index.html* wwwroot/index.html(WebAssembly)&ndash;Blazor作為 HTML 頁面實現的應用程式的根頁:</span><span class="sxs-lookup"><span data-stu-id="c0e9d-127">*wwwroot/index.html* (Blazor WebAssembly) &ndash; The root page of the app implemented as an HTML page:</span></span>
  * <span data-ttu-id="c0e9d-128">當最初請求應用的任何頁面時,此頁將呈現並在回應中返回。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-128">When any page of the app is initially requested, this page is rendered and returned in the response.</span></span>
  * <span data-ttu-id="c0e9d-129">該頁指定根`App`元件的呈現位置。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-129">The page specifies where the root `App` component is rendered.</span></span> <span data-ttu-id="c0e9d-130">`App`元件 *(App.razor*) 被`app`指定為`Startup.Configure`方法`AddComponent`中的 DOM 元素。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-130">The `App` component (*App.razor*) is specified as the `app` DOM element to the `AddComponent` method in `Startup.Configure`.</span></span>
  * <span data-ttu-id="c0e9d-131">載入`_framework/blazor.webassembly.js`JavaScript 檔,其中:</span><span class="sxs-lookup"><span data-stu-id="c0e9d-131">The `_framework/blazor.webassembly.js` JavaScript file is loaded, which:</span></span>
    * <span data-ttu-id="c0e9d-132">下載 .NET 運行時、應用和應用的依賴項。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-132">Downloads the .NET runtime, the app, and the app's dependencies.</span></span>
    * <span data-ttu-id="c0e9d-133">初始化運行時以運行應用。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-133">Initializes the runtime to run the app.</span></span>

* <span data-ttu-id="c0e9d-134">*App.razor*&ndash;<xref:Microsoft.AspNetCore.Components.Routing.Router>使用 元件設置用戶端路由的應用的根元件。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-134">*App.razor* &ndash; The root component of the app that sets up client-side routing using the <xref:Microsoft.AspNetCore.Components.Routing.Router> component.</span></span> <span data-ttu-id="c0e9d-135">元件`Router`攔截瀏覽器導航並呈現與請求的位址匹配的頁面。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-135">The `Router` component intercepts browser navigation and renders the page that matches the requested address.</span></span>

* <span data-ttu-id="c0e9d-136">*頁面*&ndash;資料夾Blazor包含構成 應用的可路由元件Blazor/頁面 *(.razor)* 和伺服器應用的根 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-136">*Pages* folder &ndash; Contains the routable components/pages (*.razor*) that make up the Blazor app and the root Razor page of a Blazor Server app.</span></span> <span data-ttu-id="c0e9d-137">使用指令指定每個頁面的[`@page`](xref:mvc/views/razor#page)路由。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-137">The route for each page is specified using the [`@page`](xref:mvc/views/razor#page) directive.</span></span> <span data-ttu-id="c0e9d-138">該樣本包括以下內容:</span><span class="sxs-lookup"><span data-stu-id="c0e9d-138">The template includes the following:</span></span>
  * <span data-ttu-id="c0e9d-139">*_Host.cshtml(*Blazor伺服器&ndash;) 作為 Razor 頁面實現的應用程式的根頁:</span><span class="sxs-lookup"><span data-stu-id="c0e9d-139">*_Host.cshtml* (Blazor Server) &ndash; The root page of the app implemented as a Razor Page:</span></span>
    * <span data-ttu-id="c0e9d-140">當最初請求應用的任何頁面時,此頁將呈現並在回應中返回。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-140">When any page of the app is initially requested, this page is rendered and returned in the response.</span></span>
    * <span data-ttu-id="c0e9d-141">載入`_framework/blazor.server.js`JavaScript 檔案,該檔設定瀏覽器和伺服器SignalR之間的即時 連接。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-141">The `_framework/blazor.server.js` JavaScript file is loaded, which sets up the real-time SignalR connection between the browser and the server.</span></span>
    * <span data-ttu-id="c0e9d-142">主機"頁指定呈現根`App`元件 (*App.razor*) 的位置。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-142">The Host page specifies where the root `App` component (*App.razor*) is rendered.</span></span>
  * <span data-ttu-id="c0e9d-143">`Counter`(*Counter.razor*反&ndash;. razor ) 實現計數器頁。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-143">`Counter` (*Counter.razor*) &ndash; Implements the Counter page.</span></span>
  * <span data-ttu-id="c0e9d-144">`Error`(*錯誤.剃鬚刀*,Blazor僅限伺服器應用 )&ndash;在應用中發生未處理的異常時呈現。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-144">`Error` (*Error.razor*, Blazor Server app only) &ndash; Rendered when an unhandled exception occurs in the app.</span></span>
  * <span data-ttu-id="c0e9d-145">`FetchData`(*提取資料.razor*)&ndash;實現提取數據頁。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-145">`FetchData` (*FetchData.razor*) &ndash; Implements the Fetch data page.</span></span>
  * <span data-ttu-id="c0e9d-146">`Index`(*索引.razor*)&ndash;實現主頁。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-146">`Index` (*Index.razor*) &ndash; Implements the Home page.</span></span>

* <span data-ttu-id="c0e9d-147">*分享*&ndash;資料夾 包含應用程式使用的其他 UI 元件 (*.razor*):</span><span class="sxs-lookup"><span data-stu-id="c0e9d-147">*Shared* folder &ndash; Contains other UI components (*.razor*) used by the app:</span></span>
  * <span data-ttu-id="c0e9d-148">`MainLayout`(*主佈局.razor*)&ndash;應用程式的佈局元件。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-148">`MainLayout` (*MainLayout.razor*) &ndash; The app's layout component.</span></span>
  * <span data-ttu-id="c0e9d-149">`NavMenu`(*NavMenu.razor*)&ndash;實現側邊欄導航。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-149">`NavMenu` (*NavMenu.razor*) &ndash; Implements sidebar navigation.</span></span> <span data-ttu-id="c0e9d-150">包括[NavLink](xref:blazor/routing#navlink-component)<xref:Microsoft.AspNetCore.Components.Routing.NavLink>元件 ( ),該元件呈現指向其他 Razor 元件的導航連結。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-150">Includes the [NavLink component](xref:blazor/routing#navlink-component) (<xref:Microsoft.AspNetCore.Components.Routing.NavLink>), which renders navigation links to other Razor components.</span></span> <span data-ttu-id="c0e9d-151">元件`NavLink`在載入其元件時自動指示所選狀態,這有助於使用者瞭解當前顯示的元件。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-151">The `NavLink` component automatically indicates a selected state when its component is loaded, which helps the user understand which component is currently displayed.</span></span>

* <span data-ttu-id="c0e9d-152">*_Imports.razor*&ndash;包括要包含在應用元件 *(.razor)* 中的常見 Razor 指令[`@using`](xref:mvc/views/razor#using),例如命名空間的指令。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-152">*_Imports.razor* &ndash; Includes common Razor directives to include in the app's components (*.razor*), such as [`@using`](xref:mvc/views/razor#using) directives for namespaces.</span></span>

* <span data-ttu-id="c0e9d-153">*數據*資料夾Blazor(伺服器&ndash;)`WeatherForecast`包含向應用`FetchData`元件 提供範`WeatherForecastService`例天氣 資料的類和實現。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-153">*Data* folder (Blazor Server) &ndash; Contains the `WeatherForecast` class and implementation of the `WeatherForecastService` that provide example weather data to the app's `FetchData` component.</span></span>

* <span data-ttu-id="c0e9d-154">*wwwroot*&ndash;包含應用的公共靜態資產的應用的[Web 根](xref:fundamentals/index#web-root)資料夾。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-154">*wwwroot* &ndash; The [Web Root](xref:fundamentals/index#web-root) folder for the app containing the app's public static assets.</span></span>

* <span data-ttu-id="c0e9d-155">*應用設置.json* Blazor (&ndash;伺服器 ) 應用程式的配置設置。</span><span class="sxs-lookup"><span data-stu-id="c0e9d-155">*appsettings.json* (Blazor Server) &ndash; Configuration settings for the app.</span></span>
