---
title: ASP.NET Core Blazor 相依性插入
author: guardrex
description: 瞭解 Blazor 應用程式如何將服務插入元件中。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2020
no-loc:
- Blazor
- SignalR
uid: blazor/dependency-injection
ms.openlocfilehash: 859fd484fc00104575f176fa7d3bf752895475a0
ms.sourcegitcommit: c81ef12a1b6e6ac838e5e07042717cf492e6635b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2020
ms.locfileid: "76885497"
---
# <a name="aspnet-core-blazor-dependency-injection"></a><span data-ttu-id="4d653-103">ASP.NET Core Blazor 相依性插入</span><span class="sxs-lookup"><span data-stu-id="4d653-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="4d653-104">依[Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="4d653-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="4d653-105">Blazor 支援相依性[插入（DI）](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="4d653-105">Blazor supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="4d653-106">應用程式可以使用內建的服務，方法是將它們插入元件中。</span><span class="sxs-lookup"><span data-stu-id="4d653-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="4d653-107">應用程式也可以定義和註冊自訂服務，並透過 DI 讓它們可在整個應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="4d653-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

<span data-ttu-id="4d653-108">DI 是用來存取集中位置所設定之服務的技術。</span><span class="sxs-lookup"><span data-stu-id="4d653-108">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="4d653-109">在 Blazor 應用程式中，這會很有用：</span><span class="sxs-lookup"><span data-stu-id="4d653-109">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="4d653-110">跨許多元件（稱為*單一*服務）共用服務類別的單一實例。</span><span class="sxs-lookup"><span data-stu-id="4d653-110">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="4d653-111">使用參考抽象，將元件與具體的服務類別分離。</span><span class="sxs-lookup"><span data-stu-id="4d653-111">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="4d653-112">例如，請考慮使用介面 `IDataAccess` 來存取應用程式中的資料。</span><span class="sxs-lookup"><span data-stu-id="4d653-112">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="4d653-113">介面是由具體的 `DataAccess` 類別來執行，並在應用程式的服務容器中註冊為服務。</span><span class="sxs-lookup"><span data-stu-id="4d653-113">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="4d653-114">當元件使用 DI 來接收 `IDataAccess` 執行時，該元件不會與具象類型結合。</span><span class="sxs-lookup"><span data-stu-id="4d653-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="4d653-115">可以交換執行，也許是針對單元測試中的 mock 執行。</span><span class="sxs-lookup"><span data-stu-id="4d653-115">The implementation can be swapped, perhaps for a mock implementation in unit tests.</span></span>

## <a name="default-services"></a><span data-ttu-id="4d653-116">預設服務</span><span class="sxs-lookup"><span data-stu-id="4d653-116">Default services</span></span>

<span data-ttu-id="4d653-117">預設服務會自動新增至應用程式的服務集合。</span><span class="sxs-lookup"><span data-stu-id="4d653-117">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="4d653-118">服務</span><span class="sxs-lookup"><span data-stu-id="4d653-118">Service</span></span> | <span data-ttu-id="4d653-119">存留期</span><span class="sxs-lookup"><span data-stu-id="4d653-119">Lifetime</span></span> | <span data-ttu-id="4d653-120">描述</span><span class="sxs-lookup"><span data-stu-id="4d653-120">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="4d653-121">單一</span><span class="sxs-lookup"><span data-stu-id="4d653-121">Singleton</span></span> | <span data-ttu-id="4d653-122">提供方法來傳送 HTTP 要求，以及從 URI 所識別的資源接收 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="4d653-122">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span><br><br><span data-ttu-id="4d653-123">Blazor WebAssembly 應用程式中的 `HttpClient` 實例會使用瀏覽器來處理背景中的 HTTP 流量。</span><span class="sxs-lookup"><span data-stu-id="4d653-123">The instance of `HttpClient` in a Blazor WebAssembly app uses the browser for handling the HTTP traffic in the background.</span></span><br><br><span data-ttu-id="4d653-124">Blazor 伺服器應用程式預設不會包含設定為服務的 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="4d653-124">Blazor Server apps don't include an `HttpClient` configured as a service by default.</span></span> <span data-ttu-id="4d653-125">提供 Blazor 伺服器應用程式的 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="4d653-125">Provide an `HttpClient` to a Blazor Server app.</span></span><br><br><span data-ttu-id="4d653-126">如需詳細資訊，請參閱<xref:blazor/call-web-api>。</span><span class="sxs-lookup"><span data-stu-id="4d653-126">For more information, see <xref:blazor/call-web-api>.</span></span> |
| `IJSRuntime` | <span data-ttu-id="4d653-127">Singleton （Blazor WebAssembly）</span><span class="sxs-lookup"><span data-stu-id="4d653-127">Singleton (Blazor WebAssembly)</span></span><br><span data-ttu-id="4d653-128">限定範圍（Blazor 伺服器）</span><span class="sxs-lookup"><span data-stu-id="4d653-128">Scoped (Blazor Server)</span></span> | <span data-ttu-id="4d653-129">代表在其中分派 JavaScript 呼叫的 JavaScript 執行時間實例。</span><span class="sxs-lookup"><span data-stu-id="4d653-129">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="4d653-130">如需詳細資訊，請參閱<xref:blazor/javascript-interop>。</span><span class="sxs-lookup"><span data-stu-id="4d653-130">For more information, see <xref:blazor/javascript-interop>.</span></span> |
| `NavigationManager` | <span data-ttu-id="4d653-131">Singleton （Blazor WebAssembly）</span><span class="sxs-lookup"><span data-stu-id="4d653-131">Singleton (Blazor WebAssembly)</span></span><br><span data-ttu-id="4d653-132">限定範圍（Blazor 伺服器）</span><span class="sxs-lookup"><span data-stu-id="4d653-132">Scoped (Blazor Server)</span></span> | <span data-ttu-id="4d653-133">包含使用 Uri 和導覽狀態的協助程式。</span><span class="sxs-lookup"><span data-stu-id="4d653-133">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="4d653-134">如需詳細資訊，請參閱[URI 和流覽狀態](xref:blazor/routing#uri-and-navigation-state-helpers)協助程式。</span><span class="sxs-lookup"><span data-stu-id="4d653-134">For more information, see [URI and navigation state helpers](xref:blazor/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="4d653-135">自訂服務提供者不會自動提供表格中所列的預設服務。</span><span class="sxs-lookup"><span data-stu-id="4d653-135">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="4d653-136">如果您使用自訂服務提供者，而且需要資料表中所顯示的任何服務，請將所需的服務新增至新的服務提供者。</span><span class="sxs-lookup"><span data-stu-id="4d653-136">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="4d653-137">將服務新增至應用程式</span><span class="sxs-lookup"><span data-stu-id="4d653-137">Add services to an app</span></span>

### <a name="blazor-webassembly"></a><span data-ttu-id="4d653-138">Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="4d653-138">Blazor WebAssembly</span></span>

<span data-ttu-id="4d653-139">在*Program.cs*的 `Main` 方法中，為應用程式的服務集合設定服務。</span><span class="sxs-lookup"><span data-stu-id="4d653-139">Configure services for the app's service collection in the `Main` method of *Program.cs*.</span></span> <span data-ttu-id="4d653-140">在下列範例中，會為 `IMyDependency`註冊 `MyDependency` 實作為：</span><span class="sxs-lookup"><span data-stu-id="4d653-140">In the following example, the `MyDependency` implementation is registered for `IMyDependency`:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<IMyDependency, MyDependency>();
        builder.RootComponents.Add<App>("app");

        await builder.Build().RunAsync();
    }
}
```

<span data-ttu-id="4d653-141">建立主機之後，您就可以從根 DI 範圍存取服務，然後再呈現任何元件。</span><span class="sxs-lookup"><span data-stu-id="4d653-141">Once the host is built, services can be accessed from the root DI scope before any components are rendered.</span></span> <span data-ttu-id="4d653-142">這有助於在轉譯內容之前執行初始化邏輯：</span><span class="sxs-lookup"><span data-stu-id="4d653-142">This can be useful for running initialization logic before rendering content:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<WeatherService>();
        builder.RootComponents.Add<App>("app");

        var host = builder.Build();

        var weatherService = host.Services.GetRequiredService<WeatherService>();
        await weatherService.InitializeWeatherAsync();

        await host.RunAsync();
    }
}
```

<span data-ttu-id="4d653-143">主機也會提供應用程式的中央設定實例。</span><span class="sxs-lookup"><span data-stu-id="4d653-143">The host also provides a central configuration instance for the app.</span></span> <span data-ttu-id="4d653-144">以先前的範例為基礎，氣象服務的 URL 會從預設設定來源（例如， *appsettings*）傳遞至 `InitializeWeatherAsync`：</span><span class="sxs-lookup"><span data-stu-id="4d653-144">Building on the preceding example, the weather service's URL is passed from a default configuration source (for example, *appsettings.json*) to `InitializeWeatherAsync`:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<WeatherService>();
        builder.RootComponents.Add<App>("app");

        var host = builder.Build();

        var weatherService = host.Services.GetRequiredService<WeatherService>();
        await weatherService.InitializeWeatherAsync(
            host.Configuration["WeatherServiceUrl"]);

        await host.RunAsync();
    }
}
```

### <a name="blazor-server"></a><span data-ttu-id="4d653-145">Blazor 伺服器</span><span class="sxs-lookup"><span data-stu-id="4d653-145">Blazor Server</span></span>

<span data-ttu-id="4d653-146">建立新的應用程式之後，請檢查 `Startup.ConfigureServices` 方法：</span><span class="sxs-lookup"><span data-stu-id="4d653-146">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="4d653-147">`ConfigureServices` 方法會傳遞 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>，這是服務描述元物件（<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>）的清單。</span><span class="sxs-lookup"><span data-stu-id="4d653-147">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="4d653-148">服務會藉由將服務描述項提供給服務集合來新增。</span><span class="sxs-lookup"><span data-stu-id="4d653-148">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="4d653-149">下列範例示範 `IDataAccess` 介面和其具體執行 `DataAccess`的概念：</span><span class="sxs-lookup"><span data-stu-id="4d653-149">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

### <a name="service-lifetime"></a><span data-ttu-id="4d653-150">服務存留期</span><span class="sxs-lookup"><span data-stu-id="4d653-150">Service lifetime</span></span>

<span data-ttu-id="4d653-151">您可以使用下表所示的存留期來設定服務。</span><span class="sxs-lookup"><span data-stu-id="4d653-151">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="4d653-152">存留期</span><span class="sxs-lookup"><span data-stu-id="4d653-152">Lifetime</span></span> | <span data-ttu-id="4d653-153">描述</span><span class="sxs-lookup"><span data-stu-id="4d653-153">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Blazor<span data-ttu-id="4d653-154"> WebAssembly 應用程式目前沒有 DI 範圍的概念。</span><span class="sxs-lookup"><span data-stu-id="4d653-154"> WebAssembly apps don't currently have a concept of DI scopes.</span></span> <span data-ttu-id="4d653-155">`Scoped`註冊的服務行為就像 `Singleton` 服務一樣。</span><span class="sxs-lookup"><span data-stu-id="4d653-155">`Scoped`-registered services behave like `Singleton` services.</span></span> <span data-ttu-id="4d653-156">不過，Blazor 伺服器裝載模型支援 `Scoped` 存留期。</span><span class="sxs-lookup"><span data-stu-id="4d653-156">However, the Blazor Server hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="4d653-157">在 Blazor 伺服器應用程式中，限定範圍的服務註冊的範圍是*連接*。</span><span class="sxs-lookup"><span data-stu-id="4d653-157">In Blazor Server apps, a scoped service registration is scoped to the *connection*.</span></span> <span data-ttu-id="4d653-158">因此，即使目前的意圖是在瀏覽器中執行用戶端，使用範圍服務也適用于應該範圍設定為目前使用者的服務。</span><span class="sxs-lookup"><span data-stu-id="4d653-158">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="4d653-159">DI 會建立服務的*單一實例*。</span><span class="sxs-lookup"><span data-stu-id="4d653-159">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="4d653-160">所有需要 `Singleton` 服務的元件都會收到相同服務的實例。</span><span class="sxs-lookup"><span data-stu-id="4d653-160">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="4d653-161">每當元件從服務容器取得 `Transient` 服務的實例時，就會收到服務的*新實例*。</span><span class="sxs-lookup"><span data-stu-id="4d653-161">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="4d653-162">DI 系統是以 ASP.NET Core 中的 DI 系統為基礎。</span><span class="sxs-lookup"><span data-stu-id="4d653-162">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="4d653-163">如需詳細資訊，請參閱<xref:fundamentals/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="4d653-163">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="4d653-164">要求元件中的服務</span><span class="sxs-lookup"><span data-stu-id="4d653-164">Request a service in a component</span></span>

<span data-ttu-id="4d653-165">將服務新增至服務集合之後，請使用[\@插入](xref:mvc/views/razor#inject)Razor 指示詞，將服務插入元件中。</span><span class="sxs-lookup"><span data-stu-id="4d653-165">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#inject) Razor directive.</span></span> <span data-ttu-id="4d653-166">`@inject` 有兩個參數：</span><span class="sxs-lookup"><span data-stu-id="4d653-166">`@inject` has two parameters:</span></span>

* <span data-ttu-id="4d653-167">輸入 &ndash; 要插入之服務的類型。</span><span class="sxs-lookup"><span data-stu-id="4d653-167">Type &ndash; The type of the service to inject.</span></span>
* <span data-ttu-id="4d653-168">屬性 &ndash; 接收插入的應用程式服務之屬性的名稱。</span><span class="sxs-lookup"><span data-stu-id="4d653-168">Property &ndash; The name of the property receiving the injected app service.</span></span> <span data-ttu-id="4d653-169">屬性不需要手動建立。</span><span class="sxs-lookup"><span data-stu-id="4d653-169">The property doesn't require manual creation.</span></span> <span data-ttu-id="4d653-170">編譯器會建立屬性。</span><span class="sxs-lookup"><span data-stu-id="4d653-170">The compiler creates the property.</span></span>

<span data-ttu-id="4d653-171">如需詳細資訊，請參閱<xref:mvc/views/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="4d653-171">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="4d653-172">使用多個 `@inject` 語句來插入不同的服務。</span><span class="sxs-lookup"><span data-stu-id="4d653-172">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="4d653-173">下列範例示範如何使用 `@inject`。</span><span class="sxs-lookup"><span data-stu-id="4d653-173">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="4d653-174">執行 `Services.IDataAccess` 的服務會插入元件的屬性 `DataRepository`中。</span><span class="sxs-lookup"><span data-stu-id="4d653-174">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="4d653-175">請注意程式碼如何使用 `IDataAccess` 抽象：</span><span class="sxs-lookup"><span data-stu-id="4d653-175">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-razor[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="4d653-176">就內部而言，產生的屬性（`DataRepository`）會使用 `InjectAttribute` 屬性。</span><span class="sxs-lookup"><span data-stu-id="4d653-176">Internally, the generated property (`DataRepository`) uses the `InjectAttribute` attribute.</span></span> <span data-ttu-id="4d653-177">通常不會直接使用這個屬性。</span><span class="sxs-lookup"><span data-stu-id="4d653-177">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="4d653-178">如果元件需要基類，而且基類也需要插入的屬性，請手動加入 `InjectAttribute`：</span><span class="sxs-lookup"><span data-stu-id="4d653-178">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="4d653-179">在衍生自基類的元件中，不需要 `@inject` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="4d653-179">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="4d653-180">基類的 `InjectAttribute` 已足夠：</span><span class="sxs-lookup"><span data-stu-id="4d653-180">The `InjectAttribute` of the base class is sufficient:</span></span>

```razor
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="4d653-181">在服務中使用 DI</span><span class="sxs-lookup"><span data-stu-id="4d653-181">Use DI in services</span></span>

<span data-ttu-id="4d653-182">複雜的服務可能需要額外的服務。</span><span class="sxs-lookup"><span data-stu-id="4d653-182">Complex services might require additional services.</span></span> <span data-ttu-id="4d653-183">在先前的範例中，`DataAccess` 可能需要 `HttpClient` 預設服務。</span><span class="sxs-lookup"><span data-stu-id="4d653-183">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="4d653-184">`@inject` （或 `InjectAttribute`）無法在服務中使用。</span><span class="sxs-lookup"><span data-stu-id="4d653-184">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="4d653-185">必須改為使用*函數插入*。</span><span class="sxs-lookup"><span data-stu-id="4d653-185">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="4d653-186">將參數新增至服務的函式，即可加入必要的服務。</span><span class="sxs-lookup"><span data-stu-id="4d653-186">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="4d653-187">當 DI 建立服務時，它會辨識它在此函式中所需的服務，並據以提供它們。</span><span class="sxs-lookup"><span data-stu-id="4d653-187">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

```csharp
public class DataAccess : IDataAccess
{
    // The constructor receives an HttpClient via dependency
    // injection. HttpClient is a default service.
    public DataAccess(HttpClient client)
    {
        ...
    }
}
```

<span data-ttu-id="4d653-188">函式插入的必要條件：</span><span class="sxs-lookup"><span data-stu-id="4d653-188">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="4d653-189">其中一個函式必須存在，且其引數可由 DI 完成。</span><span class="sxs-lookup"><span data-stu-id="4d653-189">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="4d653-190">如果指定預設值，則允許 DI 未涵蓋的其他參數。</span><span class="sxs-lookup"><span data-stu-id="4d653-190">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="4d653-191">適用的函式必須是*公用*的。</span><span class="sxs-lookup"><span data-stu-id="4d653-191">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="4d653-192">其中一個適用的函數必須存在。</span><span class="sxs-lookup"><span data-stu-id="4d653-192">One applicable constructor must exist.</span></span> <span data-ttu-id="4d653-193">如果發生不明確的情況，DI 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="4d653-193">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a><span data-ttu-id="4d653-194">用來管理 DI 範圍的公用程式基底元件類別</span><span class="sxs-lookup"><span data-stu-id="4d653-194">Utility base component classes to manage a DI scope</span></span>

<span data-ttu-id="4d653-195">在 ASP.NET Core 應用程式中，限域服務的範圍通常是目前的要求。</span><span class="sxs-lookup"><span data-stu-id="4d653-195">In ASP.NET Core apps, scoped services are typically scoped to the current request.</span></span> <span data-ttu-id="4d653-196">要求完成之後，DI 系統會處置任何範圍或暫時性的服務。</span><span class="sxs-lookup"><span data-stu-id="4d653-196">After the request completes, any scoped or transient services are disposed by the DI system.</span></span> <span data-ttu-id="4d653-197">在 Blazor 伺服器應用程式中，要求範圍會在用戶端連線期間持續進行，這可能會導致暫時性和範圍內的服務生活得比預期的長。</span><span class="sxs-lookup"><span data-stu-id="4d653-197">In Blazor Server apps, the request scope lasts for the duration of the client connection, which can result in transient and scoped services living much longer than expected.</span></span>

<span data-ttu-id="4d653-198">若要將服務的範圍限定在元件的存留期間，您可以使用 `OwningComponentBase` 並 `OwningComponentBase<TService>` 基類。</span><span class="sxs-lookup"><span data-stu-id="4d653-198">To scope services to the lifetime of a component, you can use the `OwningComponentBase` and `OwningComponentBase<TService>` base classes.</span></span> <span data-ttu-id="4d653-199">這些基類會公開類型 `IServiceProvider` 的 `ScopedServices` 屬性，其會解析範圍設定為元件存留期的服務。</span><span class="sxs-lookup"><span data-stu-id="4d653-199">These base classes expose a `ScopedServices` property of type `IServiceProvider` that resolve services that are scoped to the lifetime of the component.</span></span> <span data-ttu-id="4d653-200">若要撰寫繼承自 Razor 基類的元件，請使用 `@inherits` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="4d653-200">To author a component that inherits from a base class in Razor, use the `@inherits` directive.</span></span>

```razor
@page "/users"
@attribute [Authorize]
@inherits OwningComponentBase<Data.ApplicationDbContext>

<h1>Users (@Service.Users.Count())</h1>
<ul>
    @foreach (var user in Service.Users)
    {
        <li>@user.UserName</li>
    }
</ul>
```

> [!NOTE]
> <span data-ttu-id="4d653-201">使用 `@inject` 或 `InjectAttribute` 插入元件中的服務不會在元件的範圍內建立，並且會系結至要求範圍。</span><span class="sxs-lookup"><span data-stu-id="4d653-201">Services injected into the component using `@inject` or the `InjectAttribute` aren't created in the component's scope and are tied to the request scope.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4d653-202">其他資源</span><span class="sxs-lookup"><span data-stu-id="4d653-202">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
