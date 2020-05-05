---
title: ASP.NET Core Blazor範本
author: guardrex
description: 深入瞭解 ASP.NET Core Blazor應用程式範本Blazor和專案結構。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/19/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/templates
ms.openlocfilehash: c84d6415728bf56836d98cfa66d1b9d46d2eadc8
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82770897"
---
# <a name="aspnet-core-blazor-templates"></a><span data-ttu-id="97a60-103">ASP.NET Core Blazor範本</span><span class="sxs-lookup"><span data-stu-id="97a60-103">ASP.NET Core Blazor templates</span></span>

<span data-ttu-id="97a60-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="97a60-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="97a60-105">此Blazor架構會提供範本來為每個Blazor裝載模型開發應用程式：</span><span class="sxs-lookup"><span data-stu-id="97a60-105">The Blazor framework provides templates to develop apps for each of the Blazor hosting models:</span></span>

* Blazor<span data-ttu-id="97a60-106">WebAssembly （`blazorwasm`）</span><span class="sxs-lookup"><span data-stu-id="97a60-106"> WebAssembly (`blazorwasm`)</span></span>
* Blazor<span data-ttu-id="97a60-107">伺服器（`blazorserver`）</span><span class="sxs-lookup"><span data-stu-id="97a60-107"> Server (`blazorserver`)</span></span>

<span data-ttu-id="97a60-108">如需裝載模型Blazor的詳細資訊，請<xref:blazor/hosting-models>參閱。</span><span class="sxs-lookup"><span data-stu-id="97a60-108">For more information on Blazor's hosting models, see <xref:blazor/hosting-models>.</span></span>

<span data-ttu-id="97a60-109">如需從範本建立Blazor應用程式的逐步指示，請參閱。 <xref:blazor/get-started></span><span class="sxs-lookup"><span data-stu-id="97a60-109">For step-by-step instructions on creating a Blazor app from a template, see <xref:blazor/get-started>.</span></span>

<span data-ttu-id="97a60-110">藉由將`--help`選項傳遞至[dotnet new](/dotnet/core/tools/dotnet-new) CLI 命令，即可使用範本選項：</span><span class="sxs-lookup"><span data-stu-id="97a60-110">Template options are available by passing the `--help` option to the [dotnet new](/dotnet/core/tools/dotnet-new) CLI command:</span></span>

```dotnetcli
dotnet new blazorwasm --help
dotnet new blazorserver --help
```

## <a name="blazor-project-structure"></a>Blazor<span data-ttu-id="97a60-111">專案結構</span><span class="sxs-lookup"><span data-stu-id="97a60-111"> project structure</span></span>

<span data-ttu-id="97a60-112">下列檔案和資料夾組成從Blazor Blazor範本產生的應用程式：</span><span class="sxs-lookup"><span data-stu-id="97a60-112">The following files and folders make up a Blazor app generated from a Blazor template:</span></span>

* <span data-ttu-id="97a60-113">*Program.cs* &ndash;應用程式的進入點以設定：</span><span class="sxs-lookup"><span data-stu-id="97a60-113">*Program.cs* &ndash; The app's entry point that sets up the:</span></span>

  * <span data-ttu-id="97a60-114">ASP.NET Core[主機](xref:fundamentals/host/generic-host)（Blazor伺服器）</span><span class="sxs-lookup"><span data-stu-id="97a60-114">ASP.NET Core [host](xref:fundamentals/host/generic-host) (Blazor Server)</span></span>
  * <span data-ttu-id="97a60-115">WebAssembly 主機（Blazor WebAssembly） &ndash;此檔案中的程式碼對於從Blazor WebAssembly 範本（`blazorwasm`）建立的應用程式而言是唯一的。</span><span class="sxs-lookup"><span data-stu-id="97a60-115">WebAssembly host (Blazor WebAssembly) &ndash; The code in this file is unique to apps created from the Blazor WebAssembly template (`blazorwasm`).</span></span>
    * <span data-ttu-id="97a60-116">`App`元件（也就是應用程式的根元件）會指定為`Add`方法的`app` DOM 元素。</span><span class="sxs-lookup"><span data-stu-id="97a60-116">The `App` component, which is the root component of the app, is specified as the `app` DOM element to the `Add` method.</span></span>
    * <span data-ttu-id="97a60-117">服務可以使用主機產生器`ConfigureServices`上的方法來設定（例如， `builder.Services.AddSingleton<IMyDependency, MyDependency>();`）。</span><span class="sxs-lookup"><span data-stu-id="97a60-117">Services can be configured with the `ConfigureServices` method on the host builder (for example, `builder.Services.AddSingleton<IMyDependency, MyDependency>();`).</span></span>
    * <span data-ttu-id="97a60-118">您可以透過主機產生器（`builder.Configuration`）提供設定。</span><span class="sxs-lookup"><span data-stu-id="97a60-118">Configuration can be supplied via the host builder (`builder.Configuration`).</span></span>

* <span data-ttu-id="97a60-119">*Startup.cs* （Blazor伺服器） &ndash;包含應用程式的啟動邏輯。</span><span class="sxs-lookup"><span data-stu-id="97a60-119">*Startup.cs* (Blazor Server) &ndash; Contains the app's startup logic.</span></span> <span data-ttu-id="97a60-120">`Startup`類別會定義兩個方法：</span><span class="sxs-lookup"><span data-stu-id="97a60-120">The `Startup` class defines two methods:</span></span>

  * <span data-ttu-id="97a60-121">`ConfigureServices`&ndash;設定應用程式的相依性[插入（DI）](xref:fundamentals/dependency-injection)服務。</span><span class="sxs-lookup"><span data-stu-id="97a60-121">`ConfigureServices` &ndash; Configures the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) services.</span></span> <span data-ttu-id="97a60-122">在Blazor伺服器應用程式中，會藉由<xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor*>呼叫來新增`WeatherForecastService`服務，並將新增至服務容器，以供`FetchData`範例元件使用。</span><span class="sxs-lookup"><span data-stu-id="97a60-122">In Blazor Server apps, services are added by calling <xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor*>, and the `WeatherForecastService` is added to the service container for use by the example `FetchData` component.</span></span>
  * <span data-ttu-id="97a60-123">`Configure`&ndash;設定應用程式的要求處理管線：</span><span class="sxs-lookup"><span data-stu-id="97a60-123">`Configure` &ndash; Configures the app's request handling pipeline:</span></span>
    * <span data-ttu-id="97a60-124"><xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub*>呼叫來設定端點，以與瀏覽器進行即時連接。</span><span class="sxs-lookup"><span data-stu-id="97a60-124"><xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub*> is called to set up an endpoint for the real-time connection with the browser.</span></span> <span data-ttu-id="97a60-125">連接是使用[SignalR](xref:signalr/introduction)建立的，這是將即時 web 功能新增至應用程式的架構。</span><span class="sxs-lookup"><span data-stu-id="97a60-125">The connection is created with [SignalR](xref:signalr/introduction), which is a framework for adding real-time web functionality to apps.</span></span>
    * <span data-ttu-id="97a60-126">呼叫[MapFallbackToPage （"/_Host"）](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*)來設定應用程式的根頁面（*Pages/_Host. cshtml*）並啟用導覽。</span><span class="sxs-lookup"><span data-stu-id="97a60-126">[MapFallbackToPage("/_Host")](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*) is called to set up the root page of the app (*Pages/_Host.cshtml*) and enable navigation.</span></span>

* <span data-ttu-id="97a60-127">*wwwroot/index.html* （Blazor WebAssembly） &ndash;實作為 html 網頁之應用程式的根頁面：</span><span class="sxs-lookup"><span data-stu-id="97a60-127">*wwwroot/index.html* (Blazor WebAssembly) &ndash; The root page of the app implemented as an HTML page:</span></span>
  * <span data-ttu-id="97a60-128">一開始要求應用程式的任何頁面時，會轉譯此頁面並在回應中傳回。</span><span class="sxs-lookup"><span data-stu-id="97a60-128">When any page of the app is initially requested, this page is rendered and returned in the response.</span></span>
  * <span data-ttu-id="97a60-129">頁面會指定呈現根`App`元件的位置。</span><span class="sxs-lookup"><span data-stu-id="97a60-129">The page specifies where the root `App` component is rendered.</span></span> <span data-ttu-id="97a60-130">`App`元件（*razor*）會指定為中`app` `AddComponent` `Startup.Configure`方法的 DOM 元素。</span><span class="sxs-lookup"><span data-stu-id="97a60-130">The `App` component (*App.razor*) is specified as the `app` DOM element to the `AddComponent` method in `Startup.Configure`.</span></span>
  * <span data-ttu-id="97a60-131">已`_framework/blazor.webassembly.js`載入 JavaScript 檔案，其：</span><span class="sxs-lookup"><span data-stu-id="97a60-131">The `_framework/blazor.webassembly.js` JavaScript file is loaded, which:</span></span>
    * <span data-ttu-id="97a60-132">下載 .NET 執行時間、應用程式和應用程式的相依性。</span><span class="sxs-lookup"><span data-stu-id="97a60-132">Downloads the .NET runtime, the app, and the app's dependencies.</span></span>
    * <span data-ttu-id="97a60-133">初始化執行時間以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="97a60-133">Initializes the runtime to run the app.</span></span>

* <span data-ttu-id="97a60-134">*Razor* &ndash;應用程式的根元件，其會使用<xref:Microsoft.AspNetCore.Components.Routing.Router>元件來設定用戶端路由。</span><span class="sxs-lookup"><span data-stu-id="97a60-134">*App.razor* &ndash; The root component of the app that sets up client-side routing using the <xref:Microsoft.AspNetCore.Components.Routing.Router> component.</span></span> <span data-ttu-id="97a60-135">`Router`元件會攔截瀏覽器導覽，並呈現符合所要求位址的頁面。</span><span class="sxs-lookup"><span data-stu-id="97a60-135">The `Router` component intercepts browser navigation and renders the page that matches the requested address.</span></span>

* <span data-ttu-id="97a60-136">*Pages*資料夾&ndash;包含可路由傳送的元件/頁面（*razor*），其組成Blazor應用程式和Razor Blazor伺服器應用程式的根頁面。</span><span class="sxs-lookup"><span data-stu-id="97a60-136">*Pages* folder &ndash; Contains the routable components/pages (*.razor*) that make up the Blazor app and the root Razor page of a Blazor Server app.</span></span> <span data-ttu-id="97a60-137">每個頁面的路由都是使用[`@page`](xref:mvc/views/razor#page)指示詞來指定。</span><span class="sxs-lookup"><span data-stu-id="97a60-137">The route for each page is specified using the [`@page`](xref:mvc/views/razor#page) directive.</span></span> <span data-ttu-id="97a60-138">此範本包含下列各項：</span><span class="sxs-lookup"><span data-stu-id="97a60-138">The template includes the following:</span></span>
  * <span data-ttu-id="97a60-139">*_Host. cshtml* （Blazor伺服器） &ndash;應用程式的根頁面實作為Razor頁面：</span><span class="sxs-lookup"><span data-stu-id="97a60-139">*_Host.cshtml* (Blazor Server) &ndash; The root page of the app implemented as a Razor Page:</span></span>
    * <span data-ttu-id="97a60-140">一開始要求應用程式的任何頁面時，會轉譯此頁面並在回應中傳回。</span><span class="sxs-lookup"><span data-stu-id="97a60-140">When any page of the app is initially requested, this page is rendered and returned in the response.</span></span>
    * <span data-ttu-id="97a60-141">會`_framework/blazor.server.js`載入 JavaScript 檔案，以設定瀏覽器與伺服器之間的SignalR即時連線。</span><span class="sxs-lookup"><span data-stu-id="97a60-141">The `_framework/blazor.server.js` JavaScript file is loaded, which sets up the real-time SignalR connection between the browser and the server.</span></span>
    * <span data-ttu-id="97a60-142">[主機] 頁面會指定要`App`呈現根元件（*razor*）的位置。</span><span class="sxs-lookup"><span data-stu-id="97a60-142">The Host page specifies where the root `App` component (*App.razor*) is rendered.</span></span>
  * <span data-ttu-id="97a60-143">`Counter`（*Razor*） &ndash;會實行計數器頁面。</span><span class="sxs-lookup"><span data-stu-id="97a60-143">`Counter` (*Counter.razor*) &ndash; Implements the Counter page.</span></span>
  * <span data-ttu-id="97a60-144">`Error`（*錯誤。 razor*， Blazor僅限伺服器應用程式）&ndash;當應用程式中發生未處理的例外狀況時轉譯。</span><span class="sxs-lookup"><span data-stu-id="97a60-144">`Error` (*Error.razor*, Blazor Server app only) &ndash; Rendered when an unhandled exception occurs in the app.</span></span>
  * <span data-ttu-id="97a60-145">`FetchData`（*FetchData razor*） &ndash;會執行提取資料頁面。</span><span class="sxs-lookup"><span data-stu-id="97a60-145">`FetchData` (*FetchData.razor*) &ndash; Implements the Fetch data page.</span></span>
  * <span data-ttu-id="97a60-146">`Index`（*Index. razor*） &ndash;實行首頁。</span><span class="sxs-lookup"><span data-stu-id="97a60-146">`Index` (*Index.razor*) &ndash; Implements the Home page.</span></span>

* <span data-ttu-id="97a60-147">*共用*資料夾&ndash;包含應用程式所使用的其他 UI 元件（*razor*）：</span><span class="sxs-lookup"><span data-stu-id="97a60-147">*Shared* folder &ndash; Contains other UI components (*.razor*) used by the app:</span></span>
  * <span data-ttu-id="97a60-148">`MainLayout`（*MainLayout razor*） &ndash;應用程式的版面配置元件。</span><span class="sxs-lookup"><span data-stu-id="97a60-148">`MainLayout` (*MainLayout.razor*) &ndash; The app's layout component.</span></span>
  * <span data-ttu-id="97a60-149">`NavMenu`（*Navmenu.cshtml razor*） &ndash;會執行提要欄位導覽。</span><span class="sxs-lookup"><span data-stu-id="97a60-149">`NavMenu` (*NavMenu.razor*) &ndash; Implements sidebar navigation.</span></span> <span data-ttu-id="97a60-150">包含[NavLink 元件](xref:blazor/routing#navlink-component)（<xref:Microsoft.AspNetCore.Components.Routing.NavLink>），它會呈現其他Razor元件的導覽連結。</span><span class="sxs-lookup"><span data-stu-id="97a60-150">Includes the [NavLink component](xref:blazor/routing#navlink-component) (<xref:Microsoft.AspNetCore.Components.Routing.NavLink>), which renders navigation links to other Razor components.</span></span> <span data-ttu-id="97a60-151">`NavLink`元件會在載入元件時自動指出選取的狀態，協助使用者瞭解目前顯示的元件。</span><span class="sxs-lookup"><span data-stu-id="97a60-151">The `NavLink` component automatically indicates a selected state when its component is loaded, which helps the user understand which component is currently displayed.</span></span>

* <span data-ttu-id="97a60-152">*_Imports razor* &ndash;包含要包含Razor在應用程式元件（*razor*）中的通用指示詞，例如[`@using`](xref:mvc/views/razor#using)命名空間的指示詞。</span><span class="sxs-lookup"><span data-stu-id="97a60-152">*_Imports.razor* &ndash; Includes common Razor directives to include in the app's components (*.razor*), such as [`@using`](xref:mvc/views/razor#using) directives for namespaces.</span></span>

* <span data-ttu-id="97a60-153">*[資料*資料夾Blazor （伺服器&ndash; ）] `WeatherForecast`包含的類別和執行`WeatherForecastService` ，可提供範例天氣資料給應用程式`FetchData`的元件。</span><span class="sxs-lookup"><span data-stu-id="97a60-153">*Data* folder (Blazor Server) &ndash; Contains the `WeatherForecast` class and implementation of the `WeatherForecastService` that provide example weather data to the app's `FetchData` component.</span></span>

* <span data-ttu-id="97a60-154">*wwwroot* &ndash;包含應用程式公用靜態資產之應用程式的[Web 根](xref:fundamentals/index#web-root)資料夾。</span><span class="sxs-lookup"><span data-stu-id="97a60-154">*wwwroot* &ndash; The [Web Root](xref:fundamentals/index#web-root) folder for the app containing the app's public static assets.</span></span>

* <span data-ttu-id="97a60-155">*appsettings*應用程式的Blazor json （ &ndash;伺服器）設定。</span><span class="sxs-lookup"><span data-stu-id="97a60-155">*appsettings.json* (Blazor Server) &ndash; Configuration settings for the app.</span></span>
