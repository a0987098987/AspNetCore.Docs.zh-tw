---
<span data-ttu-id="230a4-101">標題： ' ASP.NET Core Blazor 範本 ' 作者：描述：「瞭解 ASP.NET Core Blazor 應用程式範本和 Blazor 專案結構」。</span><span class="sxs-lookup"><span data-stu-id="230a4-101">title: 'ASP.NET Core Blazor templates' author: description: 'Learn about ASP.NET Core Blazor app templates and Blazor project structure.'</span></span>
<span data-ttu-id="230a4-102">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="230a4-102">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="230a4-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="230a4-103">'Blazor'</span></span>
- <span data-ttu-id="230a4-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="230a4-104">'Identity'</span></span>
- <span data-ttu-id="230a4-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="230a4-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="230a4-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="230a4-106">'Razor'</span></span>
- <span data-ttu-id="230a4-107">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="230a4-107">'SignalR' uid:</span></span> 

---
# <a name="aspnet-core-blazor-templates"></a><span data-ttu-id="230a4-108">ASP.NET Core Blazor 範本</span><span class="sxs-lookup"><span data-stu-id="230a4-108">ASP.NET Core Blazor templates</span></span>

<span data-ttu-id="230a4-109">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="230a4-109">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="230a4-110">此 Blazor 架構會提供範本來為每個裝載模型開發應用程式 Blazor ：</span><span class="sxs-lookup"><span data-stu-id="230a4-110">The Blazor framework provides templates to develop apps for each of the Blazor hosting models:</span></span>

* Blazor<span data-ttu-id="230a4-111">WebAssembly （ `blazorwasm` ）</span><span class="sxs-lookup"><span data-stu-id="230a4-111"> WebAssembly (`blazorwasm`)</span></span>
* Blazor<span data-ttu-id="230a4-112">伺服器（ `blazorserver` ）</span><span class="sxs-lookup"><span data-stu-id="230a4-112"> Server (`blazorserver`)</span></span>

<span data-ttu-id="230a4-113">如需裝載模型的詳細資訊 Blazor ，請參閱 <xref:blazor/hosting-models> 。</span><span class="sxs-lookup"><span data-stu-id="230a4-113">For more information on Blazor's hosting models, see <xref:blazor/hosting-models>.</span></span>

<span data-ttu-id="230a4-114">如需從範本建立應用程式的逐步指示 Blazor ，請參閱 <xref:blazor/get-started> 。</span><span class="sxs-lookup"><span data-stu-id="230a4-114">For step-by-step instructions on creating a Blazor app from a template, see <xref:blazor/get-started>.</span></span>

<span data-ttu-id="230a4-115">藉由將 `--help` 選項傳遞至[dotnet new](/dotnet/core/tools/dotnet-new) CLI 命令，即可使用範本選項：</span><span class="sxs-lookup"><span data-stu-id="230a4-115">Template options are available by passing the `--help` option to the [dotnet new](/dotnet/core/tools/dotnet-new) CLI command:</span></span>

```dotnetcli
dotnet new blazorwasm --help
dotnet new blazorserver --help
```

## <a name="blazor-project-structure"></a>Blazor<span data-ttu-id="230a4-116">專案結構</span><span class="sxs-lookup"><span data-stu-id="230a4-116"> project structure</span></span>

<span data-ttu-id="230a4-117">下列檔案和資料夾組成 Blazor 從範本產生的應用程式 Blazor ：</span><span class="sxs-lookup"><span data-stu-id="230a4-117">The following files and folders make up a Blazor app generated from a Blazor template:</span></span>

* <span data-ttu-id="230a4-118">*Program.cs*：應用程式的進入點，可設定：</span><span class="sxs-lookup"><span data-stu-id="230a4-118">*Program.cs*: The app's entry point that sets up the:</span></span>

  * <span data-ttu-id="230a4-119">ASP.NET Core[主機](xref:fundamentals/host/generic-host)（ Blazor 伺服器）</span><span class="sxs-lookup"><span data-stu-id="230a4-119">ASP.NET Core [host](xref:fundamentals/host/generic-host) (Blazor Server)</span></span>
  * <span data-ttu-id="230a4-120">WebAssembly 主機（ Blazor WebAssembly）：此檔案中的程式碼對於從 Blazor WebAssembly 範本（）建立的應用程式而言是唯一的 `blazorwasm` 。</span><span class="sxs-lookup"><span data-stu-id="230a4-120">WebAssembly host (Blazor WebAssembly): The code in this file is unique to apps created from the Blazor WebAssembly template (`blazorwasm`).</span></span>
    * <span data-ttu-id="230a4-121">`App`元件（也就是應用程式的根元件）會指定為方法的 `app` DOM 元素 `Add` 。</span><span class="sxs-lookup"><span data-stu-id="230a4-121">The `App` component, which is the root component of the app, is specified as the `app` DOM element to the `Add` method.</span></span>
    * <span data-ttu-id="230a4-122">服務可以使用主機產生器 `ConfigureServices` 上的方法來設定（例如， `builder.Services.AddSingleton<IMyDependency, MyDependency>();` ）。</span><span class="sxs-lookup"><span data-stu-id="230a4-122">Services can be configured with the `ConfigureServices` method on the host builder (for example, `builder.Services.AddSingleton<IMyDependency, MyDependency>();`).</span></span>
    * <span data-ttu-id="230a4-123">您可以透過主機產生器（）提供設定 `builder.Configuration` 。</span><span class="sxs-lookup"><span data-stu-id="230a4-123">Configuration can be supplied via the host builder (`builder.Configuration`).</span></span>

* <span data-ttu-id="230a4-124">*Startup.cs* （ Blazor 伺服器）：包含應用程式的啟動邏輯。</span><span class="sxs-lookup"><span data-stu-id="230a4-124">*Startup.cs* (Blazor Server): Contains the app's startup logic.</span></span> <span data-ttu-id="230a4-125">`Startup`類別會定義兩個方法：</span><span class="sxs-lookup"><span data-stu-id="230a4-125">The `Startup` class defines two methods:</span></span>

  * <span data-ttu-id="230a4-126">`ConfigureServices`：設定應用程式的相依性[插入（DI）](xref:fundamentals/dependency-injection)服務。</span><span class="sxs-lookup"><span data-stu-id="230a4-126">`ConfigureServices`: Configures the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) services.</span></span> <span data-ttu-id="230a4-127">在 Blazor 伺服器應用程式中，會藉由呼叫來新增服務 <xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor%2A> ，並將 `WeatherForecastService` 新增至服務容器，以供範例 `FetchData` 元件使用。</span><span class="sxs-lookup"><span data-stu-id="230a4-127">In Blazor Server apps, services are added by calling <xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor%2A>, and the `WeatherForecastService` is added to the service container for use by the example `FetchData` component.</span></span>
  * <span data-ttu-id="230a4-128">`Configure`：設定應用程式的要求處理管線：</span><span class="sxs-lookup"><span data-stu-id="230a4-128">`Configure`: Configures the app's request handling pipeline:</span></span>
    * <span data-ttu-id="230a4-129"><xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub%2A>呼叫來設定端點，以與瀏覽器進行即時連接。</span><span class="sxs-lookup"><span data-stu-id="230a4-129"><xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub%2A> is called to set up an endpoint for the real-time connection with the browser.</span></span> <span data-ttu-id="230a4-130">連接是使用建立的 [SignalR](xref:signalr/introduction) ，這是將即時 web 功能新增至應用程式的架構。</span><span class="sxs-lookup"><span data-stu-id="230a4-130">The connection is created with [SignalR](xref:signalr/introduction), which is a framework for adding real-time web functionality to apps.</span></span>
    * <span data-ttu-id="230a4-131">呼叫[MapFallbackToPage （"/_Host"）](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*)來設定應用程式的根頁面（*Pages/_Host. cshtml*）並啟用導覽。</span><span class="sxs-lookup"><span data-stu-id="230a4-131">[MapFallbackToPage("/_Host")](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*) is called to set up the root page of the app (*Pages/_Host.cshtml*) and enable navigation.</span></span>

* <span data-ttu-id="230a4-132">*wwwroot/index.html* （ Blazor WebAssembly）：實作為 html 網頁之應用程式的根頁面：</span><span class="sxs-lookup"><span data-stu-id="230a4-132">*wwwroot/index.html* (Blazor WebAssembly): The root page of the app implemented as an HTML page:</span></span>
  * <span data-ttu-id="230a4-133">一開始要求應用程式的任何頁面時，會轉譯此頁面並在回應中傳回。</span><span class="sxs-lookup"><span data-stu-id="230a4-133">When any page of the app is initially requested, this page is rendered and returned in the response.</span></span>
  * <span data-ttu-id="230a4-134">頁面會指定呈現根元件的位置 `App` 。</span><span class="sxs-lookup"><span data-stu-id="230a4-134">The page specifies where the root `App` component is rendered.</span></span> <span data-ttu-id="230a4-135">`App`元件（*razor*）會指定為 `app` 中方法的 DOM 元素 `AddComponent` `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="230a4-135">The `App` component (*App.razor*) is specified as the `app` DOM element to the `AddComponent` method in `Startup.Configure`.</span></span>
  * <span data-ttu-id="230a4-136">`_framework/blazor.webassembly.js`已載入 JavaScript 檔案，其：</span><span class="sxs-lookup"><span data-stu-id="230a4-136">The `_framework/blazor.webassembly.js` JavaScript file is loaded, which:</span></span>
    * <span data-ttu-id="230a4-137">下載 .NET 執行時間、應用程式和應用程式的相依性。</span><span class="sxs-lookup"><span data-stu-id="230a4-137">Downloads the .NET runtime, the app, and the app's dependencies.</span></span>
    * <span data-ttu-id="230a4-138">初始化執行時間以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="230a4-138">Initializes the runtime to run the app.</span></span>

* <span data-ttu-id="230a4-139">*Razor*：應用程式的根元件，會使用元件來設定用戶端路由 <xref:Microsoft.AspNetCore.Components.Routing.Router> 。</span><span class="sxs-lookup"><span data-stu-id="230a4-139">*App.razor*: The root component of the app that sets up client-side routing using the <xref:Microsoft.AspNetCore.Components.Routing.Router> component.</span></span> <span data-ttu-id="230a4-140"><xref:Microsoft.AspNetCore.Components.Routing.Router>元件會攔截瀏覽器導覽，並呈現符合所要求位址的頁面。</span><span class="sxs-lookup"><span data-stu-id="230a4-140">The <xref:Microsoft.AspNetCore.Components.Routing.Router> component intercepts browser navigation and renders the page that matches the requested address.</span></span>

* <span data-ttu-id="230a4-141">*Pages*資料夾：包含組成應用程式的可路由元件/頁面（*razor*）， Blazor 以及 Razor 伺服器應用程式的根頁面 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="230a4-141">*Pages* folder: Contains the routable components/pages (*.razor*) that make up the Blazor app and the root Razor page of a Blazor Server app.</span></span> <span data-ttu-id="230a4-142">每個頁面的路由都是使用指示詞來指定 [`@page`](xref:mvc/views/razor#page) 。</span><span class="sxs-lookup"><span data-stu-id="230a4-142">The route for each page is specified using the [`@page`](xref:mvc/views/razor#page) directive.</span></span> <span data-ttu-id="230a4-143">此範本包含下列各項：</span><span class="sxs-lookup"><span data-stu-id="230a4-143">The template includes the following:</span></span>
  * <span data-ttu-id="230a4-144">*_Host. cshtml* （ Blazor 伺服器）：將應用程式的根頁面實作為 Razor 頁面：</span><span class="sxs-lookup"><span data-stu-id="230a4-144">*_Host.cshtml* (Blazor Server): The root page of the app implemented as a Razor Page:</span></span>
    * <span data-ttu-id="230a4-145">一開始要求應用程式的任何頁面時，會轉譯此頁面並在回應中傳回。</span><span class="sxs-lookup"><span data-stu-id="230a4-145">When any page of the app is initially requested, this page is rendered and returned in the response.</span></span>
    * <span data-ttu-id="230a4-146">`_framework/blazor.server.js`會載入 JavaScript 檔案，以設定 SignalR 瀏覽器與伺服器之間的即時連線。</span><span class="sxs-lookup"><span data-stu-id="230a4-146">The `_framework/blazor.server.js` JavaScript file is loaded, which sets up the real-time SignalR connection between the browser and the server.</span></span>
    * <span data-ttu-id="230a4-147">[主機] 頁面會指定要 `App` 呈現根元件（*razor*）的位置。</span><span class="sxs-lookup"><span data-stu-id="230a4-147">The Host page specifies where the root `App` component (*App.razor*) is rendered.</span></span>
  * <span data-ttu-id="230a4-148">`Counter`（*Razor*）：實作為計數器頁面。</span><span class="sxs-lookup"><span data-stu-id="230a4-148">`Counter` (*Counter.razor*): Implements the Counter page.</span></span>
  * <span data-ttu-id="230a4-149">`Error`（*錯誤。 razor*， Blazor僅限伺服器應用程式）：在應用程式中發生未處理的例外狀況時呈現。</span><span class="sxs-lookup"><span data-stu-id="230a4-149">`Error` (*Error.razor*, Blazor Server app only): Rendered when an unhandled exception occurs in the app.</span></span>
  * <span data-ttu-id="230a4-150">`FetchData`（*FetchData razor*）：實作為提取資料頁面。</span><span class="sxs-lookup"><span data-stu-id="230a4-150">`FetchData` (*FetchData.razor*): Implements the Fetch data page.</span></span>
  * <span data-ttu-id="230a4-151">`Index`（*Index. razor*）：實作為首頁。</span><span class="sxs-lookup"><span data-stu-id="230a4-151">`Index` (*Index.razor*): Implements the Home page.</span></span>

* <span data-ttu-id="230a4-152">*共用*資料夾：包含應用程式所使用的其他 UI 元件（*razor*）：</span><span class="sxs-lookup"><span data-stu-id="230a4-152">*Shared* folder: Contains other UI components (*.razor*) used by the app:</span></span>
  * <span data-ttu-id="230a4-153">`MainLayout`（*MainLayout razor*）：應用程式的版面配置元件。</span><span class="sxs-lookup"><span data-stu-id="230a4-153">`MainLayout` (*MainLayout.razor*): The app's layout component.</span></span>
  * <span data-ttu-id="230a4-154">`NavMenu`（*Navmenu.cshtml razor*）： [執行] 提要欄位導覽。</span><span class="sxs-lookup"><span data-stu-id="230a4-154">`NavMenu` (*NavMenu.razor*): Implements sidebar navigation.</span></span> <span data-ttu-id="230a4-155">包含[NavLink 元件](xref:blazor/routing#navlink-component)（ <xref:Microsoft.AspNetCore.Components.Routing.NavLink> ），它會呈現其他元件的導覽連結 Razor 。</span><span class="sxs-lookup"><span data-stu-id="230a4-155">Includes the [NavLink component](xref:blazor/routing#navlink-component) (<xref:Microsoft.AspNetCore.Components.Routing.NavLink>), which renders navigation links to other Razor components.</span></span> <span data-ttu-id="230a4-156">元件會在 <xref:Microsoft.AspNetCore.Components.Routing.NavLink> 載入元件時自動指出選取的狀態，協助使用者瞭解目前顯示的元件。</span><span class="sxs-lookup"><span data-stu-id="230a4-156">The <xref:Microsoft.AspNetCore.Components.Routing.NavLink> component automatically indicates a selected state when its component is loaded, which helps the user understand which component is currently displayed.</span></span>

* <span data-ttu-id="230a4-157">*_Imports razor*：包含 Razor 要包含在應用程式元件（*razor*）中的通用指示詞，例如命名空間的指示詞 [`@using`](xref:mvc/views/razor#using) 。</span><span class="sxs-lookup"><span data-stu-id="230a4-157">*_Imports.razor*: Includes common Razor directives to include in the app's components (*.razor*), such as [`@using`](xref:mvc/views/razor#using) directives for namespaces.</span></span>

* <span data-ttu-id="230a4-158">*資料*資料夾（ Blazor 伺服器）：包含的 `WeatherForecast` 類別和執行，可 `WeatherForecastService` 提供範例天氣資料給應用程式的 `FetchData` 元件。</span><span class="sxs-lookup"><span data-stu-id="230a4-158">*Data* folder (Blazor Server): Contains the `WeatherForecast` class and implementation of the `WeatherForecastService` that provide example weather data to the app's `FetchData` component.</span></span>

* <span data-ttu-id="230a4-159">*wwwroot*：應用程式的[Web 根](xref:fundamentals/index#web-root)資料夾，其中包含應用程式的公用靜態資產。</span><span class="sxs-lookup"><span data-stu-id="230a4-159">*wwwroot*: The [Web Root](xref:fundamentals/index#web-root) folder for the app containing the app's public static assets.</span></span>

* <span data-ttu-id="230a4-160">*appsettings. json* （ Blazor 伺服器）：應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="230a4-160">*appsettings.json* (Blazor Server): Configuration settings for the app.</span></span>
