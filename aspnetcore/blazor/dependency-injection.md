---
title: ASP.NET Core Blazor相依性插入
author: guardrex
description: 瞭解應用Blazor程式如何將服務插入元件中。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/04/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/dependency-injection
ms.openlocfilehash: e96698bd0bd8f3f3b290ba24bc8169efb16f1d03
ms.sourcegitcommit: 84b46594f57608f6ac4f0570172c7051df507520
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2020
ms.locfileid: "82967528"
---
# <a name="aspnet-core-blazor-dependency-injection"></a><span data-ttu-id="120bf-103">ASP.NET Core Blazor 相依性插入</span><span class="sxs-lookup"><span data-stu-id="120bf-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="120bf-104">By [Rainer Stropek](https://www.timecockpit.com)和[Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="120bf-104">By [Rainer Stropek](https://www.timecockpit.com) and [Mike Rousos](https://github.com/mjrousos)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="120bf-105">Blazor 支援相依性[插入（DI）](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="120bf-105">Blazor supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="120bf-106">應用程式可以使用內建的服務，方法是將它們插入元件中。</span><span class="sxs-lookup"><span data-stu-id="120bf-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="120bf-107">應用程式也可以定義和註冊自訂服務，並透過 DI 讓它們可在整個應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="120bf-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

<span data-ttu-id="120bf-108">DI 是用來存取集中位置所設定之服務的技術。</span><span class="sxs-lookup"><span data-stu-id="120bf-108">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="120bf-109">在 Blazor 應用程式中，這會很有用：</span><span class="sxs-lookup"><span data-stu-id="120bf-109">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="120bf-110">跨許多元件（稱為*單一*服務）共用服務類別的單一實例。</span><span class="sxs-lookup"><span data-stu-id="120bf-110">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="120bf-111">使用參考抽象，將元件與具體的服務類別分離。</span><span class="sxs-lookup"><span data-stu-id="120bf-111">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="120bf-112">例如，請考慮使用介面`IDataAccess`來存取應用程式中的資料。</span><span class="sxs-lookup"><span data-stu-id="120bf-112">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="120bf-113">介面是由具象`DataAccess`類別來執行，並在應用程式的服務容器中註冊為服務。</span><span class="sxs-lookup"><span data-stu-id="120bf-113">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="120bf-114">當元件使用 DI 來接收實作為`IDataAccess`時，該元件不會結合到具體類型。</span><span class="sxs-lookup"><span data-stu-id="120bf-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="120bf-115">可以交換執行，也許是針對單元測試中的 mock 執行。</span><span class="sxs-lookup"><span data-stu-id="120bf-115">The implementation can be swapped, perhaps for a mock implementation in unit tests.</span></span>

## <a name="default-services"></a><span data-ttu-id="120bf-116">預設服務</span><span class="sxs-lookup"><span data-stu-id="120bf-116">Default services</span></span>

<span data-ttu-id="120bf-117">預設服務會自動新增至應用程式的服務集合。</span><span class="sxs-lookup"><span data-stu-id="120bf-117">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="120bf-118">服務</span><span class="sxs-lookup"><span data-stu-id="120bf-118">Service</span></span> | <span data-ttu-id="120bf-119">存留期</span><span class="sxs-lookup"><span data-stu-id="120bf-119">Lifetime</span></span> | <span data-ttu-id="120bf-120">描述</span><span class="sxs-lookup"><span data-stu-id="120bf-120">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="120bf-121">暫時性</span><span class="sxs-lookup"><span data-stu-id="120bf-121">Transient</span></span> | <span data-ttu-id="120bf-122">提供方法來傳送 HTTP 要求，以及從 URI 所識別的資源接收 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="120bf-122">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span><br><br><span data-ttu-id="120bf-123">Blazor WebAssembly 應用`HttpClient`程式中的實例會使用瀏覽器來處理背景中的 HTTP 流量。</span><span class="sxs-lookup"><span data-stu-id="120bf-123">The instance of `HttpClient` in a Blazor WebAssembly app uses the browser for handling the HTTP traffic in the background.</span></span><br><br><span data-ttu-id="120bf-124">Blazor 伺服器應用程式預設不`HttpClient`包含已設定為服務的。</span><span class="sxs-lookup"><span data-stu-id="120bf-124">Blazor Server apps don't include an `HttpClient` configured as a service by default.</span></span> <span data-ttu-id="120bf-125">將提供`HttpClient`給 Blazor 伺服器應用程式。</span><span class="sxs-lookup"><span data-stu-id="120bf-125">Provide an `HttpClient` to a Blazor Server app.</span></span><br><br><span data-ttu-id="120bf-126">如需詳細資訊，請參閱<xref:blazor/call-web-api>。</span><span class="sxs-lookup"><span data-stu-id="120bf-126">For more information, see <xref:blazor/call-web-api>.</span></span> |
| `IJSRuntime` | <span data-ttu-id="120bf-127">Singleton （Blazor WebAssembly）</span><span class="sxs-lookup"><span data-stu-id="120bf-127">Singleton (Blazor WebAssembly)</span></span><br><span data-ttu-id="120bf-128">限定範圍（Blazor 伺服器）</span><span class="sxs-lookup"><span data-stu-id="120bf-128">Scoped (Blazor Server)</span></span> | <span data-ttu-id="120bf-129">代表在其中分派 JavaScript 呼叫的 JavaScript 執行時間實例。</span><span class="sxs-lookup"><span data-stu-id="120bf-129">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="120bf-130">如需詳細資訊，請參閱<xref:blazor/call-javascript-from-dotnet>。</span><span class="sxs-lookup"><span data-stu-id="120bf-130">For more information, see <xref:blazor/call-javascript-from-dotnet>.</span></span> |
| `NavigationManager` | <span data-ttu-id="120bf-131">Singleton （Blazor WebAssembly）</span><span class="sxs-lookup"><span data-stu-id="120bf-131">Singleton (Blazor WebAssembly)</span></span><br><span data-ttu-id="120bf-132">限定範圍（Blazor 伺服器）</span><span class="sxs-lookup"><span data-stu-id="120bf-132">Scoped (Blazor Server)</span></span> | <span data-ttu-id="120bf-133">包含使用 Uri 和導覽狀態的協助程式。</span><span class="sxs-lookup"><span data-stu-id="120bf-133">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="120bf-134">如需詳細資訊，請參閱[URI 和流覽狀態](xref:blazor/routing#uri-and-navigation-state-helpers)協助程式。</span><span class="sxs-lookup"><span data-stu-id="120bf-134">For more information, see [URI and navigation state helpers](xref:blazor/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="120bf-135">自訂服務提供者不會自動提供表格中所列的預設服務。</span><span class="sxs-lookup"><span data-stu-id="120bf-135">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="120bf-136">如果您使用自訂服務提供者，而且需要資料表中所顯示的任何服務，請將所需的服務新增至新的服務提供者。</span><span class="sxs-lookup"><span data-stu-id="120bf-136">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="120bf-137">將服務新增至應用程式</span><span class="sxs-lookup"><span data-stu-id="120bf-137">Add services to an app</span></span>

### <a name="blazor-webassembly"></a><span data-ttu-id="120bf-138">Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="120bf-138">Blazor WebAssembly</span></span>

<span data-ttu-id="120bf-139">在`Main` *Program.cs*的方法中，為應用程式的服務集合設定服務。</span><span class="sxs-lookup"><span data-stu-id="120bf-139">Configure services for the app's service collection in the `Main` method of *Program.cs*.</span></span> <span data-ttu-id="120bf-140">在下列範例中， `MyDependency`會為`IMyDependency`執行註冊：</span><span class="sxs-lookup"><span data-stu-id="120bf-140">In the following example, the `MyDependency` implementation is registered for `IMyDependency`:</span></span>

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

<span data-ttu-id="120bf-141">建立主機之後，您就可以從根 DI 範圍存取服務，然後再呈現任何元件。</span><span class="sxs-lookup"><span data-stu-id="120bf-141">Once the host is built, services can be accessed from the root DI scope before any components are rendered.</span></span> <span data-ttu-id="120bf-142">這有助於在轉譯內容之前執行初始化邏輯：</span><span class="sxs-lookup"><span data-stu-id="120bf-142">This can be useful for running initialization logic before rendering content:</span></span>

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

<span data-ttu-id="120bf-143">主機也會提供應用程式的中央設定實例。</span><span class="sxs-lookup"><span data-stu-id="120bf-143">The host also provides a central configuration instance for the app.</span></span> <span data-ttu-id="120bf-144">以先前的範例為基礎，氣象服務的 URL 會從預設設定來源（例如*appsettings*）傳遞至`InitializeWeatherAsync`：</span><span class="sxs-lookup"><span data-stu-id="120bf-144">Building on the preceding example, the weather service's URL is passed from a default configuration source (for example, *appsettings.json*) to `InitializeWeatherAsync`:</span></span>

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

### <a name="blazor-server"></a><span data-ttu-id="120bf-145">Blazor 伺服器</span><span class="sxs-lookup"><span data-stu-id="120bf-145">Blazor Server</span></span>

<span data-ttu-id="120bf-146">建立新的應用程式之後，請`Startup.ConfigureServices`檢查方法：</span><span class="sxs-lookup"><span data-stu-id="120bf-146">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="120bf-147">會`ConfigureServices`傳遞方法<xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>，這是服務描述元物件（<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>）的清單。</span><span class="sxs-lookup"><span data-stu-id="120bf-147">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="120bf-148">服務會藉由將服務描述項提供給服務集合來新增。</span><span class="sxs-lookup"><span data-stu-id="120bf-148">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="120bf-149">下列範例示範`IDataAccess`介面和其具體執行`DataAccess`的概念：</span><span class="sxs-lookup"><span data-stu-id="120bf-149">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

### <a name="service-lifetime"></a><span data-ttu-id="120bf-150">服務存留期</span><span class="sxs-lookup"><span data-stu-id="120bf-150">Service lifetime</span></span>

<span data-ttu-id="120bf-151">您可以使用下表所示的存留期來設定服務。</span><span class="sxs-lookup"><span data-stu-id="120bf-151">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="120bf-152">存留期</span><span class="sxs-lookup"><span data-stu-id="120bf-152">Lifetime</span></span> | <span data-ttu-id="120bf-153">描述</span><span class="sxs-lookup"><span data-stu-id="120bf-153">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped%2A> | Blazor<span data-ttu-id="120bf-154">WebAssembly apps 目前不具有 DI 範圍的概念。</span><span class="sxs-lookup"><span data-stu-id="120bf-154"> WebAssembly apps don't currently have a concept of DI scopes.</span></span> <span data-ttu-id="120bf-155">`Scoped`註冊的服務的行為`Singleton`就像服務一樣。</span><span class="sxs-lookup"><span data-stu-id="120bf-155">`Scoped`-registered services behave like `Singleton` services.</span></span> <span data-ttu-id="120bf-156">不過， Blazor伺服器裝載模型支援`Scoped`存留期。</span><span class="sxs-lookup"><span data-stu-id="120bf-156">However, the Blazor Server hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="120bf-157">在Blazor伺服器應用程式中，範圍服務註冊的範圍是*連接*。</span><span class="sxs-lookup"><span data-stu-id="120bf-157">In Blazor Server apps, a scoped service registration is scoped to the *connection*.</span></span> <span data-ttu-id="120bf-158">因此，即使目前的意圖是在瀏覽器中執行用戶端，使用範圍服務也適用于應該範圍設定為目前使用者的服務。</span><span class="sxs-lookup"><span data-stu-id="120bf-158">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton%2A> | <span data-ttu-id="120bf-159">DI 會建立服務的*單一實例*。</span><span class="sxs-lookup"><span data-stu-id="120bf-159">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="120bf-160">所有需要`Singleton`服務的元件都會收到相同服務的實例。</span><span class="sxs-lookup"><span data-stu-id="120bf-160">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient%2A> | <span data-ttu-id="120bf-161">每當元件從服務容器取得`Transient`服務的實例時，就會收到服務的*新實例*。</span><span class="sxs-lookup"><span data-stu-id="120bf-161">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="120bf-162">DI 系統是以 ASP.NET Core 中的 DI 系統為基礎。</span><span class="sxs-lookup"><span data-stu-id="120bf-162">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="120bf-163">如需詳細資訊，請參閱<xref:fundamentals/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="120bf-163">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="120bf-164">要求元件中的服務</span><span class="sxs-lookup"><span data-stu-id="120bf-164">Request a service in a component</span></span>

<span data-ttu-id="120bf-165">將服務新增至服務集合之後，請使用[ \@插入](xref:mvc/views/razor#inject) Razor指示詞將服務插入元件中。</span><span class="sxs-lookup"><span data-stu-id="120bf-165">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#inject) Razor directive.</span></span> <span data-ttu-id="120bf-166">`@inject`有兩個參數：</span><span class="sxs-lookup"><span data-stu-id="120bf-166">`@inject` has two parameters:</span></span>

* <span data-ttu-id="120bf-167">輸入&ndash;要插入之服務的類型。</span><span class="sxs-lookup"><span data-stu-id="120bf-167">Type &ndash; The type of the service to inject.</span></span>
* <span data-ttu-id="120bf-168">屬性&ndash;接收插入的應用程式服務之屬性的名稱。</span><span class="sxs-lookup"><span data-stu-id="120bf-168">Property &ndash; The name of the property receiving the injected app service.</span></span> <span data-ttu-id="120bf-169">屬性不需要手動建立。</span><span class="sxs-lookup"><span data-stu-id="120bf-169">The property doesn't require manual creation.</span></span> <span data-ttu-id="120bf-170">編譯器會建立屬性。</span><span class="sxs-lookup"><span data-stu-id="120bf-170">The compiler creates the property.</span></span>

<span data-ttu-id="120bf-171">如需詳細資訊，請參閱<xref:mvc/views/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="120bf-171">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="120bf-172">使用多`@inject`個語句來插入不同的服務。</span><span class="sxs-lookup"><span data-stu-id="120bf-172">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="120bf-173">下列範例示範如何使用 `@inject`。</span><span class="sxs-lookup"><span data-stu-id="120bf-173">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="120bf-174">執行`Services.IDataAccess`的服務會插入元件的屬性`DataRepository`中。</span><span class="sxs-lookup"><span data-stu-id="120bf-174">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="120bf-175">請注意程式碼如何使用`IDataAccess`抽象概念：</span><span class="sxs-lookup"><span data-stu-id="120bf-175">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-razor[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="120bf-176">就內部而言，產生的`DataRepository`屬性（） `InjectAttribute`會使用屬性。</span><span class="sxs-lookup"><span data-stu-id="120bf-176">Internally, the generated property (`DataRepository`) uses the `InjectAttribute` attribute.</span></span> <span data-ttu-id="120bf-177">通常不會直接使用這個屬性。</span><span class="sxs-lookup"><span data-stu-id="120bf-177">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="120bf-178">如果元件需要基類，而且基類也需要插入的`InjectAttribute`屬性，請手動新增：</span><span class="sxs-lookup"><span data-stu-id="120bf-178">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="120bf-179">在衍生自基類的元件中， `@inject`不需要指示詞。</span><span class="sxs-lookup"><span data-stu-id="120bf-179">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="120bf-180">`InjectAttribute`基類的已足夠：</span><span class="sxs-lookup"><span data-stu-id="120bf-180">The `InjectAttribute` of the base class is sufficient:</span></span>

```razor
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="120bf-181">在服務中使用 DI</span><span class="sxs-lookup"><span data-stu-id="120bf-181">Use DI in services</span></span>

<span data-ttu-id="120bf-182">複雜的服務可能需要額外的服務。</span><span class="sxs-lookup"><span data-stu-id="120bf-182">Complex services might require additional services.</span></span> <span data-ttu-id="120bf-183">在先前的範例中`DataAccess` ，可能需要`HttpClient`預設服務。</span><span class="sxs-lookup"><span data-stu-id="120bf-183">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="120bf-184">`@inject`（或`InjectAttribute`）無法在服務中使用。</span><span class="sxs-lookup"><span data-stu-id="120bf-184">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="120bf-185">必須改為使用*函數插入*。</span><span class="sxs-lookup"><span data-stu-id="120bf-185">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="120bf-186">將參數新增至服務的函式，即可加入必要的服務。</span><span class="sxs-lookup"><span data-stu-id="120bf-186">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="120bf-187">當 DI 建立服務時，它會辨識它在此函式中所需的服務，並據以提供它們。</span><span class="sxs-lookup"><span data-stu-id="120bf-187">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

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

<span data-ttu-id="120bf-188">函式插入的必要條件：</span><span class="sxs-lookup"><span data-stu-id="120bf-188">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="120bf-189">其中一個函式必須存在，且其引數可由 DI 完成。</span><span class="sxs-lookup"><span data-stu-id="120bf-189">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="120bf-190">如果指定預設值，則允許 DI 未涵蓋的其他參數。</span><span class="sxs-lookup"><span data-stu-id="120bf-190">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="120bf-191">適用的函式必須是*公用*的。</span><span class="sxs-lookup"><span data-stu-id="120bf-191">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="120bf-192">其中一個適用的函數必須存在。</span><span class="sxs-lookup"><span data-stu-id="120bf-192">One applicable constructor must exist.</span></span> <span data-ttu-id="120bf-193">如果發生不明確的情況，DI 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="120bf-193">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a><span data-ttu-id="120bf-194">用來管理 DI 範圍的公用程式基底元件類別</span><span class="sxs-lookup"><span data-stu-id="120bf-194">Utility base component classes to manage a DI scope</span></span>

<span data-ttu-id="120bf-195">在 ASP.NET Core 應用程式中，限域服務的範圍通常是目前的要求。</span><span class="sxs-lookup"><span data-stu-id="120bf-195">In ASP.NET Core apps, scoped services are typically scoped to the current request.</span></span> <span data-ttu-id="120bf-196">要求完成之後，DI 系統會處置任何範圍或暫時性的服務。</span><span class="sxs-lookup"><span data-stu-id="120bf-196">After the request completes, any scoped or transient services are disposed by the DI system.</span></span> <span data-ttu-id="120bf-197">在Blazor [伺服器應用程式] 中，要求範圍會在用戶端連線期間持續進行，這可能會導致暫時性和範圍服務的存留時間比預期長。</span><span class="sxs-lookup"><span data-stu-id="120bf-197">In Blazor Server apps, the request scope lasts for the duration of the client connection, which can result in transient and scoped services living much longer than expected.</span></span> <span data-ttu-id="120bf-198">在Blazor WebAssembly apps 中，以限定範圍存留期註冊的服務會視為單次個體，因此其存留時間比一般 ASP.NET Core 應用程式中的限域服務長。</span><span class="sxs-lookup"><span data-stu-id="120bf-198">In Blazor WebAssembly apps, services registered with a scoped lifetime are treated as singletons, so they live longer than scoped services in typical ASP.NET Core apps.</span></span>

<span data-ttu-id="120bf-199">限制應用程式中Blazor服務存留期的方法是使用`OwningComponentBase`類型。</span><span class="sxs-lookup"><span data-stu-id="120bf-199">An approach that limits a service lifetime in Blazor apps is use of the `OwningComponentBase` type.</span></span> <span data-ttu-id="120bf-200">`OwningComponentBase`是衍生自`ComponentBase`的抽象類別型，它會建立與元件存留期相對應的 DI 範圍。</span><span class="sxs-lookup"><span data-stu-id="120bf-200">`OwningComponentBase` is an abstract type derived from `ComponentBase` that creates a DI scope corresponding to the lifetime of the component.</span></span> <span data-ttu-id="120bf-201">使用此範圍時，您可以使用具有限定範圍存留期的 DI 服務，而且只要元件，就可以讓它們正常運作。</span><span class="sxs-lookup"><span data-stu-id="120bf-201">Using this scope, it's possible to use DI services with a scoped lifetime and have them live as long as the component.</span></span> <span data-ttu-id="120bf-202">當元件損毀時，來自元件範圍服務提供者的服務也會一併處置。</span><span class="sxs-lookup"><span data-stu-id="120bf-202">When the component is destroyed, services from the component's scoped service provider are disposed as well.</span></span> <span data-ttu-id="120bf-203">這適用于下列服務：</span><span class="sxs-lookup"><span data-stu-id="120bf-203">This can be useful for services that:</span></span>

* <span data-ttu-id="120bf-204">應該在元件內重複使用，因為暫時性存留期並不適當。</span><span class="sxs-lookup"><span data-stu-id="120bf-204">Should be reused within a component, as the transient lifetime is inappropriate.</span></span>
* <span data-ttu-id="120bf-205">不應跨元件共用，因為單一存留期並不適當。</span><span class="sxs-lookup"><span data-stu-id="120bf-205">Shouldn't be shared across components, as the singleton lifetime is inappropriate.</span></span>

<span data-ttu-id="120bf-206">`OwningComponentBase`類型的兩個版本可供使用：</span><span class="sxs-lookup"><span data-stu-id="120bf-206">Two versions of the `OwningComponentBase` type are available:</span></span>

* <span data-ttu-id="120bf-207">`OwningComponentBase`是`ComponentBase`類型的抽象、可處置子系，具有類型`ScopedServices` `IServiceProvider`的受保護屬性。</span><span class="sxs-lookup"><span data-stu-id="120bf-207">`OwningComponentBase` is an abstract, disposable child of the `ComponentBase` type with a protected `ScopedServices` property of type `IServiceProvider`.</span></span> <span data-ttu-id="120bf-208">這個提供者可以用來解析範圍設定為元件存留期的服務。</span><span class="sxs-lookup"><span data-stu-id="120bf-208">This provider can be used to resolve services that are scoped to the lifetime of the component.</span></span>

  <span data-ttu-id="120bf-209">使用`@inject`或`InjectAttribute` （`[Inject]`）插入元件的 DI 服務不會在元件的範圍內建立。</span><span class="sxs-lookup"><span data-stu-id="120bf-209">DI services injected into the component using `@inject` or the `InjectAttribute` (`[Inject]`) aren't created in the component's scope.</span></span> <span data-ttu-id="120bf-210">若要使用元件的範圍，必須使用`ScopedServices.GetRequiredService`或`ScopedServices.GetService`來解析服務。</span><span class="sxs-lookup"><span data-stu-id="120bf-210">To use the component's scope, services must be resolved using `ScopedServices.GetRequiredService` or `ScopedServices.GetService`.</span></span> <span data-ttu-id="120bf-211">使用此`ScopedServices`提供者解析的任何服務都會從該相同範圍提供其相依性。</span><span class="sxs-lookup"><span data-stu-id="120bf-211">Any services resolved using the `ScopedServices` provider have their dependencies provided from that same scope.</span></span>

  ```razor
  @page "/preferences"
  @using Microsoft.Extensions.DependencyInjection
  @inherits OwningComponentBase

  <h1>User (@UserService.Name)</h1>

  <ul>
      @foreach (var setting in SettingService.GetSettings())
      {
          <li>@setting.SettingName: @setting.SettingValue</li>
      }
  </ul>

  @code {
      private IUserService UserService { get; set; }
      private ISettingService SettingService { get; set; }

      protected override void OnInitialized()
      {
          UserService = ScopedServices.GetRequiredService<IUserService>();
          SettingService = ScopedServices.GetRequiredService<ISettingService>();
      }
  }
  ```

* <span data-ttu-id="120bf-212">`OwningComponentBase<T>`衍生自`OwningComponentBase` ，並加入屬性`Service` ， `T`從範圍 DI 提供者傳回的實例。</span><span class="sxs-lookup"><span data-stu-id="120bf-212">`OwningComponentBase<T>` derives from `OwningComponentBase` and adds a property `Service` that returns an instance of `T` from the scoped DI provider.</span></span> <span data-ttu-id="120bf-213">`IServiceProvider`當應用程式需要使用元件範圍的 DI 容器中的一個主要服務時，此類型可方便您存取已設定範圍的服務，而不需使用的實例。</span><span class="sxs-lookup"><span data-stu-id="120bf-213">This type is a convenient way to access scoped services without using an instance of `IServiceProvider` when there's one primary service the app requires from the DI container using the component's scope.</span></span> <span data-ttu-id="120bf-214">`ScopedServices`屬性可供使用，因此應用程式可以取得其他類型的服務（如有需要）。</span><span class="sxs-lookup"><span data-stu-id="120bf-214">The `ScopedServices` property is available, so the app can get services of other types, if necessary.</span></span>

  ```razor
  @page "/users"
  @attribute [Authorize]
  @inherits OwningComponentBase<AppDbContext>

  <h1>Users (@Service.Users.Count())</h1>

  <ul>
      @foreach (var user in Service.Users)
      {
          <li>@user.UserName</li>
      }
  </ul>
  ```

## <a name="use-of-entity-framework-dbcontext-from-di"></a><span data-ttu-id="120bf-215">從 DI 使用 Entity Framework DbCoNtext</span><span class="sxs-lookup"><span data-stu-id="120bf-215">Use of Entity Framework DbContext from DI</span></span>

<span data-ttu-id="120bf-216">從 web 應用程式中的 DI 取出的一個常見服務類型是 Entity Framework （ `DbContext` EF）物件。</span><span class="sxs-lookup"><span data-stu-id="120bf-216">One common service type to retrieve from DI in web apps is Entity Framework (EF) `DbContext` objects.</span></span> <span data-ttu-id="120bf-217">使用`IServiceCollection.AddDbContext`註冊 EF 服務`DbContext`時，預設會將新增為範圍服務。</span><span class="sxs-lookup"><span data-stu-id="120bf-217">Registering EF services using `IServiceCollection.AddDbContext` adds the `DbContext` as a scoped service by default.</span></span> <span data-ttu-id="120bf-218">將註冊為有範圍的服務可能會導致Blazor應用程式發生問題`DbContext` ，因為它會導致實例長期存留，並在應用程式之間共用。</span><span class="sxs-lookup"><span data-stu-id="120bf-218">Registering as a scoped service can lead to problems in Blazor apps because it causes `DbContext` instances to be long-lived and shared across the app.</span></span> <span data-ttu-id="120bf-219">`DbContext`不是安全線程，而且不得同時使用。</span><span class="sxs-lookup"><span data-stu-id="120bf-219">`DbContext` isn't thread-safe and must not be used concurrently.</span></span>

<span data-ttu-id="120bf-220">根據應用程式而定， `OwningComponentBase`使用將的範圍限制`DbContext`為單一元件，*可能會*解決此問題。</span><span class="sxs-lookup"><span data-stu-id="120bf-220">Depending on the app, using `OwningComponentBase` to limit the scope of a `DbContext` to a single component *may* solve the issue.</span></span> <span data-ttu-id="120bf-221">`DbContext`如果元件未平行使用，則從衍生元件， `OwningComponentBase`並`DbContext`從中`ScopedServices`抓取，就足以確保：</span><span class="sxs-lookup"><span data-stu-id="120bf-221">If a component doesn't use a `DbContext` in parallel, deriving the component from `OwningComponentBase` and retrieving the `DbContext` from `ScopedServices` is sufficient because it ensures that:</span></span>

* <span data-ttu-id="120bf-222">個別元件不會共用`DbContext`。</span><span class="sxs-lookup"><span data-stu-id="120bf-222">Separate components don't share a `DbContext`.</span></span>
* <span data-ttu-id="120bf-223">存留`DbContext`的時間只會取決於元件。</span><span class="sxs-lookup"><span data-stu-id="120bf-223">The `DbContext` lives only as long as the component depending on it.</span></span>

<span data-ttu-id="120bf-224">如果單一元件可能會`DbContext`同時使用（例如，每次使用者選取按鈕時），即使使用`OwningComponentBase`並不會避免並行 EF 作業的問題。</span><span class="sxs-lookup"><span data-stu-id="120bf-224">If a single component might use a `DbContext` concurrently (for example, every time a user selects a button), even using `OwningComponentBase` doesn't avoid issues with concurrent EF operations.</span></span> <span data-ttu-id="120bf-225">在此情況下，請針對`DbContext`每個邏輯 EF 作業使用不同的。</span><span class="sxs-lookup"><span data-stu-id="120bf-225">In that case, use a different `DbContext` for each logical EF operation.</span></span> <span data-ttu-id="120bf-226">請使用下列其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="120bf-226">Use either of the following approaches:</span></span>

* <span data-ttu-id="120bf-227">建立`DbContext`直接使用`DbContextOptions<TContext>`做為引數的，您可以從 DI 抓取它，而且它是安全線程。</span><span class="sxs-lookup"><span data-stu-id="120bf-227">Create the `DbContext` directly using `DbContextOptions<TContext>` as an argument, which can be retrieved from DI and is thread safe.</span></span>

    ```razor
    @page "/example"
    @inject DbContextOptions<AppDbContext> DbContextOptions

    <ul>
        @foreach (var item in data)
        {
            <li>@item</li>
        }
    </ul>

    <button @onclick="LoadData">Load Data</button>

    @code {
        private List<string> data = new List<string>();

        private async Task LoadData()
        {
            data = await GetAsync();
            StateHasChanged();
        }

        public async Task<List<string>> GetAsync()
        {
            using (var context = new AppDbContext(DbContextOptions))
            {
                return await context.Products.Select(p => p.Name).ToListAsync();
            }
        }
    }
    ```

* <span data-ttu-id="120bf-228">`DbContext`在服務容器中，以暫時性存留期註冊：</span><span class="sxs-lookup"><span data-stu-id="120bf-228">Register the `DbContext` in the service container with a transient lifetime:</span></span>
  * <span data-ttu-id="120bf-229">註冊內容時，請使用`ServiceLifetime.Transient`。</span><span class="sxs-lookup"><span data-stu-id="120bf-229">When registering the context, use `ServiceLifetime.Transient`.</span></span> <span data-ttu-id="120bf-230">`AddDbContext`擴充方法會接受兩個類型`ServiceLifetime`的選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="120bf-230">The `AddDbContext` extension method takes two optional parameters of type `ServiceLifetime`.</span></span> <span data-ttu-id="120bf-231">若要使用此方法，只有`contextLifetime`參數需要是`ServiceLifetime.Transient`。</span><span class="sxs-lookup"><span data-stu-id="120bf-231">To use this approach, only the `contextLifetime` parameter needs to be `ServiceLifetime.Transient`.</span></span> <span data-ttu-id="120bf-232">`optionsLifetime`可以保留其預設值`ServiceLifetime.Scoped`。</span><span class="sxs-lookup"><span data-stu-id="120bf-232">`optionsLifetime` can keep its default value of `ServiceLifetime.Scoped`.</span></span>

    ```csharp
    services.AddDbContext<AppDbContext>(options =>
         options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")),
         ServiceLifetime.Transient);
    ```  

  * <span data-ttu-id="120bf-233">暫時性`DbContext`可以像平常一樣（使用`@inject`）插入至不會平行執行多個 EF 作業的元件。</span><span class="sxs-lookup"><span data-stu-id="120bf-233">The transient `DbContext` can be injected as normal (using `@inject`) into components that will not execute multiple EF operations in parallel.</span></span> <span data-ttu-id="120bf-234">可能同時執行多個 EF 作業的人員可以使用`DbContext` `IServiceProvider.GetRequiredService`，針對每個平行作業要求個別的物件。</span><span class="sxs-lookup"><span data-stu-id="120bf-234">Those that may perform multiple EF operations simultaneously can request separate `DbContext` objects for each parallel operation using `IServiceProvider.GetRequiredService`.</span></span>

    ```razor
    @page "/example"
    @using Microsoft.Extensions.DependencyInjection
    @inject IServiceProvider ServiceProvider

    <ul>
        @foreach (var item in data)
        {
            <li>@item</li>
        }
    </ul>

    <button @onclick="LoadData">Load Data</button>

    @code {
        private List<string> data = new List<string>();

        private async Task LoadData()
        {
            data = await GetAsync();
            StateHasChanged();
        }

        public async Task<List<string>> GetAsync()
        {
            using (var context = ServiceProvider.GetRequiredService<AppDbContext>())
            {
                return await context.Products.Select(p => p.Name).ToListAsync();
            }
        }
    }
    ```

## <a name="additional-resources"></a><span data-ttu-id="120bf-235">其他資源</span><span class="sxs-lookup"><span data-stu-id="120bf-235">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
