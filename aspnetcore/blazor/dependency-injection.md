---
title: ASP.NET核心Blazor相依項
author: guardrex
description: 瞭解應用Blazor如何將服務注入元件。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/20/2020
no-loc:
- Blazor
- SignalR
uid: blazor/dependency-injection
ms.openlocfilehash: 4cdde9ee8c9fd9adf00894a067d32965b180e5ec
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78658071"
---
# <a name="aspnet-core-blazor-dependency-injection"></a><span data-ttu-id="a7b57-103">ASP.NET核心布拉佐爾依賴注入</span><span class="sxs-lookup"><span data-stu-id="a7b57-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="a7b57-104">由[雷納·斯特羅佩克](https://www.timecockpit.com)和[邁克·魯索斯](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="a7b57-104">By [Rainer Stropek](https://www.timecockpit.com) and [Mike Rousos](https://github.com/mjrousos)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="a7b57-105">布拉佐爾支持[依賴注入 (DI)。](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="a7b57-105">Blazor supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="a7b57-106">應用可以通過將它們注入元件來使用內置服務。</span><span class="sxs-lookup"><span data-stu-id="a7b57-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="a7b57-107">應用還可以定義和註冊自定義服務,並通過 DI 在整個應用中提供這些服務。</span><span class="sxs-lookup"><span data-stu-id="a7b57-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

<span data-ttu-id="a7b57-108">DI 是一種用於訪問在中心位置配置的服務的技術。</span><span class="sxs-lookup"><span data-stu-id="a7b57-108">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="a7b57-109">這在 Blazor 應用中非常有用,可用於:</span><span class="sxs-lookup"><span data-stu-id="a7b57-109">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="a7b57-110">跨多個元件(稱為*單例*服務)共用服務類的單個實例。</span><span class="sxs-lookup"><span data-stu-id="a7b57-110">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="a7b57-111">通過使用引用抽象將元件與具體服務類分離。</span><span class="sxs-lookup"><span data-stu-id="a7b57-111">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="a7b57-112">例如,請考慮用於訪問應用中`IDataAccess`數據的介面。</span><span class="sxs-lookup"><span data-stu-id="a7b57-112">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="a7b57-113">該介面由具體`DataAccess`類實現,並在應用的服務容器中註冊為服務。</span><span class="sxs-lookup"><span data-stu-id="a7b57-113">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="a7b57-114">當元件使用 DI 接收`IDataAccess`實現 時,元件不會耦合到具體類型。</span><span class="sxs-lookup"><span data-stu-id="a7b57-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="a7b57-115">可以交換實現,也許是為了單元測試中的模擬實現。</span><span class="sxs-lookup"><span data-stu-id="a7b57-115">The implementation can be swapped, perhaps for a mock implementation in unit tests.</span></span>

## <a name="default-services"></a><span data-ttu-id="a7b57-116">預設服務</span><span class="sxs-lookup"><span data-stu-id="a7b57-116">Default services</span></span>

<span data-ttu-id="a7b57-117">默認服務將自動添加到應用的服務集合中。</span><span class="sxs-lookup"><span data-stu-id="a7b57-117">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="a7b57-118">服務</span><span class="sxs-lookup"><span data-stu-id="a7b57-118">Service</span></span> | <span data-ttu-id="a7b57-119">存留期</span><span class="sxs-lookup"><span data-stu-id="a7b57-119">Lifetime</span></span> | <span data-ttu-id="a7b57-120">描述</span><span class="sxs-lookup"><span data-stu-id="a7b57-120">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="a7b57-121">單一</span><span class="sxs-lookup"><span data-stu-id="a7b57-121">Singleton</span></span> | <span data-ttu-id="a7b57-122">提供了從 URI 識別的資源發送 HTTP 請求和接收 HTTP 回應的方法。</span><span class="sxs-lookup"><span data-stu-id="a7b57-122">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span><br><br><span data-ttu-id="a7b57-123">Blazor `HttpClient` WebAssembly 應用中的實例使用瀏覽器處理後台的 HTTP 流量。</span><span class="sxs-lookup"><span data-stu-id="a7b57-123">The instance of `HttpClient` in a Blazor WebAssembly app uses the browser for handling the HTTP traffic in the background.</span></span><br><br><span data-ttu-id="a7b57-124">默認情況下,Blazor `HttpClient` Server 應用不包括配置為服務的應用。</span><span class="sxs-lookup"><span data-stu-id="a7b57-124">Blazor Server apps don't include an `HttpClient` configured as a service by default.</span></span> <span data-ttu-id="a7b57-125">向`HttpClient`Blazor 伺服器應用提供 。</span><span class="sxs-lookup"><span data-stu-id="a7b57-125">Provide an `HttpClient` to a Blazor Server app.</span></span><br><br><span data-ttu-id="a7b57-126">如需詳細資訊，請參閱 <xref:blazor/call-web-api>。</span><span class="sxs-lookup"><span data-stu-id="a7b57-126">For more information, see <xref:blazor/call-web-api>.</span></span> |
| `IJSRuntime` | <span data-ttu-id="a7b57-127">單頓(布拉佐網路大會)</span><span class="sxs-lookup"><span data-stu-id="a7b57-127">Singleton (Blazor WebAssembly)</span></span><br><span data-ttu-id="a7b57-128">範圍 (布拉佐伺服器)</span><span class="sxs-lookup"><span data-stu-id="a7b57-128">Scoped (Blazor Server)</span></span> | <span data-ttu-id="a7b57-129">表示調度 JavaScript 呼叫的 JavaScript 執行時的實例。</span><span class="sxs-lookup"><span data-stu-id="a7b57-129">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="a7b57-130">如需詳細資訊，請參閱 <xref:blazor/call-javascript-from-dotnet>。</span><span class="sxs-lookup"><span data-stu-id="a7b57-130">For more information, see <xref:blazor/call-javascript-from-dotnet>.</span></span> |
| `NavigationManager` | <span data-ttu-id="a7b57-131">單頓(布拉佐網路大會)</span><span class="sxs-lookup"><span data-stu-id="a7b57-131">Singleton (Blazor WebAssembly)</span></span><br><span data-ttu-id="a7b57-132">範圍 (布拉佐伺服器)</span><span class="sxs-lookup"><span data-stu-id="a7b57-132">Scoped (Blazor Server)</span></span> | <span data-ttu-id="a7b57-133">包含用於處理URI和導航狀態的幫助器。</span><span class="sxs-lookup"><span data-stu-id="a7b57-133">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="a7b57-134">有關詳細資訊,請參閱[URI 和瀏覽狀態說明器](xref:blazor/routing#uri-and-navigation-state-helpers)。</span><span class="sxs-lookup"><span data-stu-id="a7b57-134">For more information, see [URI and navigation state helpers](xref:blazor/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="a7b57-135">自定義服務提供者不會自動提供表中列出的預設服務。</span><span class="sxs-lookup"><span data-stu-id="a7b57-135">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="a7b57-136">如果使用自定義服務提供者並且需要表中顯示的任何服務,則向新的服務提供者添加所需的服務。</span><span class="sxs-lookup"><span data-stu-id="a7b57-136">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="a7b57-137">新增服務新增到應用程式</span><span class="sxs-lookup"><span data-stu-id="a7b57-137">Add services to an app</span></span>

### <a name="blazor-webassembly"></a><span data-ttu-id="a7b57-138">Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="a7b57-138">Blazor WebAssembly</span></span>

<span data-ttu-id="a7b57-139">在`Main`*Program.cs*方法中為應用的服務集合配置服務。</span><span class="sxs-lookup"><span data-stu-id="a7b57-139">Configure services for the app's service collection in the `Main` method of *Program.cs*.</span></span> <span data-ttu-id="a7b57-140">在下面的範例中,`MyDependency`實現註冊`IMyDependency`為 :</span><span class="sxs-lookup"><span data-stu-id="a7b57-140">In the following example, the `MyDependency` implementation is registered for `IMyDependency`:</span></span>

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

<span data-ttu-id="a7b57-141">生成主機后,可以在呈現任何元件之前從根 DI 作用域訪問服務。</span><span class="sxs-lookup"><span data-stu-id="a7b57-141">Once the host is built, services can be accessed from the root DI scope before any components are rendered.</span></span> <span data-ttu-id="a7b57-142">這對於在呈現內容之前執行初始化邏輯非常有用:</span><span class="sxs-lookup"><span data-stu-id="a7b57-142">This can be useful for running initialization logic before rendering content:</span></span>

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

<span data-ttu-id="a7b57-143">主機還為應用提供中央配置實例。</span><span class="sxs-lookup"><span data-stu-id="a7b57-143">The host also provides a central configuration instance for the app.</span></span> <span data-ttu-id="a7b57-144">基於前面的範例,天氣服務的網址從預設設定來源 (例如*appsettings.json)* 傳遞到`InitializeWeatherAsync`:</span><span class="sxs-lookup"><span data-stu-id="a7b57-144">Building on the preceding example, the weather service's URL is passed from a default configuration source (for example, *appsettings.json*) to `InitializeWeatherAsync`:</span></span>

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

### <a name="blazor-server"></a><span data-ttu-id="a7b57-145">Blazor 伺服器</span><span class="sxs-lookup"><span data-stu-id="a7b57-145">Blazor Server</span></span>

<span data-ttu-id="a7b57-146">建立新應用程式後,請檢查以下`Startup.ConfigureServices`方法:</span><span class="sxs-lookup"><span data-stu-id="a7b57-146">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="a7b57-147">方法`ConfigureServices`傳遞的是<xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, 它是服務描述符<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>物件 () 的清單。</span><span class="sxs-lookup"><span data-stu-id="a7b57-147">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="a7b57-148">通過向服務集合提供服務描述符來添加服務。</span><span class="sxs-lookup"><span data-stu-id="a7b57-148">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="a7b57-149">下面的範例展示該概念與`IDataAccess`介面與介面與`DataAccess`:</span><span class="sxs-lookup"><span data-stu-id="a7b57-149">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

### <a name="service-lifetime"></a><span data-ttu-id="a7b57-150">服務壽命</span><span class="sxs-lookup"><span data-stu-id="a7b57-150">Service lifetime</span></span>

<span data-ttu-id="a7b57-151">可以使用下表中顯示的存留期配置服務。</span><span class="sxs-lookup"><span data-stu-id="a7b57-151">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="a7b57-152">存留期</span><span class="sxs-lookup"><span data-stu-id="a7b57-152">Lifetime</span></span> | <span data-ttu-id="a7b57-153">描述</span><span class="sxs-lookup"><span data-stu-id="a7b57-153">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Blazor<span data-ttu-id="a7b57-154">Web組裝應用當前沒有 DI 作用域的概念。</span><span class="sxs-lookup"><span data-stu-id="a7b57-154"> WebAssembly apps don't currently have a concept of DI scopes.</span></span> <span data-ttu-id="a7b57-155">`Scoped`-註冊服務就像服務一`Singleton`樣。</span><span class="sxs-lookup"><span data-stu-id="a7b57-155">`Scoped`-registered services behave like `Singleton` services.</span></span> <span data-ttu-id="a7b57-156">但是,Blazor伺服器託管模型支援`Scoped`存留期。</span><span class="sxs-lookup"><span data-stu-id="a7b57-156">However, the Blazor Server hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="a7b57-157">在Blazor伺服器應用中,作用網域服務註冊的範圍限定到*連接*。</span><span class="sxs-lookup"><span data-stu-id="a7b57-157">In Blazor Server apps, a scoped service registration is scoped to the *connection*.</span></span> <span data-ttu-id="a7b57-158">因此,對於應限定為當前用戶的服務,使用作用域服務是首選,即使當前意圖是在瀏覽器中運行用戶端。</span><span class="sxs-lookup"><span data-stu-id="a7b57-158">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="a7b57-159">DI 建立服務的*單一實體 。*</span><span class="sxs-lookup"><span data-stu-id="a7b57-159">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="a7b57-160">需要`Singleton`服務的所有元件都接收同一服務的實例。</span><span class="sxs-lookup"><span data-stu-id="a7b57-160">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="a7b57-161">每當元件從`Transient`服務容器取得服務的實體時,它都會收到服務*的新實體 。*</span><span class="sxs-lookup"><span data-stu-id="a7b57-161">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="a7b57-162">DI 系統基於 ASP.NET 核心中的 DI 系統。</span><span class="sxs-lookup"><span data-stu-id="a7b57-162">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="a7b57-163">如需詳細資訊，請參閱 <xref:fundamentals/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="a7b57-163">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="a7b57-164">要求元件中的服務</span><span class="sxs-lookup"><span data-stu-id="a7b57-164">Request a service in a component</span></span>

<span data-ttu-id="a7b57-165">將服務添加到服務集合後,使用[\@注入](xref:mvc/views/razor#inject)Razor 指令將服務注入到元件中。</span><span class="sxs-lookup"><span data-stu-id="a7b57-165">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#inject) Razor directive.</span></span> <span data-ttu-id="a7b57-166">`@inject`有兩個參數:</span><span class="sxs-lookup"><span data-stu-id="a7b57-166">`@inject` has two parameters:</span></span>

* <span data-ttu-id="a7b57-167">鍵入&ndash;要注入的服務類型。</span><span class="sxs-lookup"><span data-stu-id="a7b57-167">Type &ndash; The type of the service to inject.</span></span>
* <span data-ttu-id="a7b57-168">屬性&ndash;接收注入的應用服務的屬性的名稱。</span><span class="sxs-lookup"><span data-stu-id="a7b57-168">Property &ndash; The name of the property receiving the injected app service.</span></span> <span data-ttu-id="a7b57-169">屬性不需要手動創建。</span><span class="sxs-lookup"><span data-stu-id="a7b57-169">The property doesn't require manual creation.</span></span> <span data-ttu-id="a7b57-170">編譯器創建該屬性。</span><span class="sxs-lookup"><span data-stu-id="a7b57-170">The compiler creates the property.</span></span>

<span data-ttu-id="a7b57-171">如需詳細資訊，請參閱 <xref:mvc/views/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="a7b57-171">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="a7b57-172">使用多個`@inject`語句注入不同的服務。</span><span class="sxs-lookup"><span data-stu-id="a7b57-172">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="a7b57-173">下列範例示範如何使用 `@inject`。</span><span class="sxs-lookup"><span data-stu-id="a7b57-173">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="a7b57-174">服務實現`Services.IDataAccess`被注入到元件的屬性`DataRepository`中。</span><span class="sxs-lookup"><span data-stu-id="a7b57-174">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="a7b57-175">請注意代碼如何只使用`IDataAccess`抽象:</span><span class="sxs-lookup"><span data-stu-id="a7b57-175">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-razor[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="a7b57-176">在內部,生成的屬性`DataRepository`(`InjectAttribute`) 使用 屬性。</span><span class="sxs-lookup"><span data-stu-id="a7b57-176">Internally, the generated property (`DataRepository`) uses the `InjectAttribute` attribute.</span></span> <span data-ttu-id="a7b57-177">通常,此屬性不會直接使用。</span><span class="sxs-lookup"><span data-stu-id="a7b57-177">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="a7b57-178">如果元件需要基數,並且基類別也需要注入屬性,則手動添加 : `InjectAttribute`</span><span class="sxs-lookup"><span data-stu-id="a7b57-178">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="a7b57-179">在從基類派生的元件中,`@inject`不需要該指令。</span><span class="sxs-lookup"><span data-stu-id="a7b57-179">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="a7b57-180">基`InjectAttribute`類別的已足夠:</span><span class="sxs-lookup"><span data-stu-id="a7b57-180">The `InjectAttribute` of the base class is sufficient:</span></span>

```razor
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="a7b57-181">在服務中使用 DI</span><span class="sxs-lookup"><span data-stu-id="a7b57-181">Use DI in services</span></span>

<span data-ttu-id="a7b57-182">複雜的服務可能需要其他服務。</span><span class="sxs-lookup"><span data-stu-id="a7b57-182">Complex services might require additional services.</span></span> <span data-ttu-id="a7b57-183">在上一個示例中`DataAccess`,可能`HttpClient`需要 默認服務。</span><span class="sxs-lookup"><span data-stu-id="a7b57-183">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="a7b57-184">`@inject`(或`InjectAttribute`) 不適用於服務。</span><span class="sxs-lookup"><span data-stu-id="a7b57-184">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="a7b57-185">必須改用*建構函式注入*。</span><span class="sxs-lookup"><span data-stu-id="a7b57-185">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="a7b57-186">通過向服務的構造函數添加參數來添加所需的服務。</span><span class="sxs-lookup"><span data-stu-id="a7b57-186">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="a7b57-187">當 DI 創建服務時,它會識別構造函數中所需的服務,並相應地提供這些服務。</span><span class="sxs-lookup"><span data-stu-id="a7b57-187">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

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

<span data-ttu-id="a7b57-188">建構函數注入的先決條件:</span><span class="sxs-lookup"><span data-stu-id="a7b57-188">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="a7b57-189">一個構造函數必須存在,其參數都可以由 DI 實現。</span><span class="sxs-lookup"><span data-stu-id="a7b57-189">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="a7b57-190">如果 DI 未涵蓋的其他參數指定預設值,則允許它們。</span><span class="sxs-lookup"><span data-stu-id="a7b57-190">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="a7b57-191">適用的建構函數必須是*公共的*。</span><span class="sxs-lookup"><span data-stu-id="a7b57-191">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="a7b57-192">必須存在一個適用的構造函數。</span><span class="sxs-lookup"><span data-stu-id="a7b57-192">One applicable constructor must exist.</span></span> <span data-ttu-id="a7b57-193">如果模稜兩可,DI 將引發異常。</span><span class="sxs-lookup"><span data-stu-id="a7b57-193">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a><span data-ttu-id="a7b57-194">管理 DI 作用網域的實用程式基礎元件類</span><span class="sxs-lookup"><span data-stu-id="a7b57-194">Utility base component classes to manage a DI scope</span></span>

<span data-ttu-id="a7b57-195">在 ASP.NET 核心應用中,作用域服務通常可限定為當前請求。</span><span class="sxs-lookup"><span data-stu-id="a7b57-195">In ASP.NET Core apps, scoped services are typically scoped to the current request.</span></span> <span data-ttu-id="a7b57-196">請求完成後,任何作用域或瞬態服務將由 DI 系統處理。</span><span class="sxs-lookup"><span data-stu-id="a7b57-196">After the request completes, any scoped or transient services are disposed by the DI system.</span></span> <span data-ttu-id="a7b57-197">在BlazorServer 應用中,請求範圍持續於用戶端連接的持續時間,這可能導致瞬態和作用域服務的壽命比預期長得多。</span><span class="sxs-lookup"><span data-stu-id="a7b57-197">In Blazor Server apps, the request scope lasts for the duration of the client connection, which can result in transient and scoped services living much longer than expected.</span></span> <span data-ttu-id="a7b57-198">在BlazorWebAssembly 應用中,以作用域存留期註冊的服務被視爲單例,因此它們比典型 ASP.NET Core 應用中的作用域服務活得更長。</span><span class="sxs-lookup"><span data-stu-id="a7b57-198">In Blazor WebAssembly apps, services registered with a scoped lifetime are treated as singletons, so they live longer than scoped services in typical ASP.NET Core apps.</span></span>

<span data-ttu-id="a7b57-199">限制Blazor套用的服務存留期的方法`OwningComponentBase`是使用類型 。</span><span class="sxs-lookup"><span data-stu-id="a7b57-199">An approach that limits a service lifetime in Blazor apps is use of the `OwningComponentBase` type.</span></span> <span data-ttu-id="a7b57-200">`OwningComponentBase`是從創建與元件存留期`ComponentBase`對應的 DI 作用域派生的抽象類型。</span><span class="sxs-lookup"><span data-stu-id="a7b57-200">`OwningComponentBase` is an abstract type derived from `ComponentBase` that creates a DI scope corresponding to the lifetime of the component.</span></span> <span data-ttu-id="a7b57-201">使用此作用域,可以使用具有作用域存留期的 DI 服務,並使它們像元件一樣存活。</span><span class="sxs-lookup"><span data-stu-id="a7b57-201">Using this scope, it's possible to use DI services with a scoped lifetime and have them live as long as the component.</span></span> <span data-ttu-id="a7b57-202">銷毀元件后,將釋放來自元件作用域服務提供者的服務。</span><span class="sxs-lookup"><span data-stu-id="a7b57-202">When the component is destroyed, services from the component's scoped service provider are disposed as well.</span></span> <span data-ttu-id="a7b57-203">這對於以下服務非常有用:</span><span class="sxs-lookup"><span data-stu-id="a7b57-203">This can be useful for services that:</span></span>

* <span data-ttu-id="a7b57-204">應在元件中重複使用,因為瞬態存留期不合適。</span><span class="sxs-lookup"><span data-stu-id="a7b57-204">Should be reused within a component, as the transient lifetime is inappropriate.</span></span>
* <span data-ttu-id="a7b57-205">不應跨元件共用,因為單例存留期不合適。</span><span class="sxs-lookup"><span data-stu-id="a7b57-205">Shouldn't be shared across components, as the singleton lifetime is inappropriate.</span></span>

<span data-ttu-id="a7b57-206">`OwningComponentBase`該類型的兩個版本可用:</span><span class="sxs-lookup"><span data-stu-id="a7b57-206">Two versions of the `OwningComponentBase` type are available:</span></span>

* <span data-ttu-id="a7b57-207">`OwningComponentBase`是具有類型`ComponentBase``ScopedServices``IServiceProvider`受保護屬性的抽象的一次性子級。</span><span class="sxs-lookup"><span data-stu-id="a7b57-207">`OwningComponentBase` is an abstract, disposable child of the `ComponentBase` type with a protected `ScopedServices` property of type `IServiceProvider`.</span></span> <span data-ttu-id="a7b57-208">此提供程式可用於解析範圍為元件存留期的服務。</span><span class="sxs-lookup"><span data-stu-id="a7b57-208">This provider can be used to resolve services that are scoped to the lifetime of the component.</span></span>

  <span data-ttu-id="a7b57-209">使用`@inject``InjectAttribute`或`[Inject]`( ) 注入到元件中的 DI 服務不是在元件的作用域中創建的。</span><span class="sxs-lookup"><span data-stu-id="a7b57-209">DI services injected into the component using `@inject` or the `InjectAttribute` (`[Inject]`) aren't created in the component's scope.</span></span> <span data-ttu-id="a7b57-210">要使用元件的範圍,必須使用`ScopedServices.GetRequiredService`或`ScopedServices.GetService`解析服務。</span><span class="sxs-lookup"><span data-stu-id="a7b57-210">To use the component's scope, services must be resolved using `ScopedServices.GetRequiredService` or `ScopedServices.GetService`.</span></span> <span data-ttu-id="a7b57-211">使用`ScopedServices`提供程式解析的任何服務都提供來自同一作用域的依賴項。</span><span class="sxs-lookup"><span data-stu-id="a7b57-211">Any services resolved using the `ScopedServices` provider have their dependencies provided from that same scope.</span></span>

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

* <span data-ttu-id="a7b57-212">`OwningComponentBase<T>`派生並`OwningComponentBase`添加從作用域`Service`DI 提供`T`程式返回 的 實例的屬性。</span><span class="sxs-lookup"><span data-stu-id="a7b57-212">`OwningComponentBase<T>` derives from `OwningComponentBase` and adds a property `Service` that returns an instance of `T` from the scoped DI provider.</span></span> <span data-ttu-id="a7b57-213">此類型是存取作用域服務的便捷方法`IServiceProvider`, 無需使用應用使用元件作用域從 DI 容器中需要一個主服務的實例。</span><span class="sxs-lookup"><span data-stu-id="a7b57-213">This type is a convenient way to access scoped services without using an instance of `IServiceProvider` when there's one primary service the app requires from the DI container using the component's scope.</span></span> <span data-ttu-id="a7b57-214">該`ScopedServices`屬性可用,因此如有必要,應用可以獲取其他類型的服務。</span><span class="sxs-lookup"><span data-stu-id="a7b57-214">The `ScopedServices` property is available, so the app can get services of other types, if necessary.</span></span>

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

## <a name="use-of-entity-framework-dbcontext-from-di"></a><span data-ttu-id="a7b57-215">使用來自 DI 的實體框架 DbContext</span><span class="sxs-lookup"><span data-stu-id="a7b57-215">Use of Entity Framework DbContext from DI</span></span>

<span data-ttu-id="a7b57-216">在 Web 應用中從 DI 檢索的一種`DbContext`常見服務類型是實體框架 (EF) 物件。</span><span class="sxs-lookup"><span data-stu-id="a7b57-216">One common service type to retrieve from DI in web apps is Entity Framework (EF) `DbContext` objects.</span></span> <span data-ttu-id="a7b57-217">預設情況下,使用`IServiceCollection.AddDbContext`將添加`DbContext`作為作用域服務的 EF 服務註冊。</span><span class="sxs-lookup"><span data-stu-id="a7b57-217">Registering EF services using `IServiceCollection.AddDbContext` adds the `DbContext` as a scoped service by default.</span></span> <span data-ttu-id="a7b57-218">註冊為作用域服務可能會導致Blazor應用中出現問題,因為它`DbContext`會導致 實例在應用中長壽命且共用。</span><span class="sxs-lookup"><span data-stu-id="a7b57-218">Registering as a scoped service can lead to problems in Blazor apps because it causes `DbContext` instances to be long-lived and shared across the app.</span></span> <span data-ttu-id="a7b57-219">`DbContext`不帶線程安全,不能同時使用。</span><span class="sxs-lookup"><span data-stu-id="a7b57-219">`DbContext` isn't thread-safe and must not be used concurrently.</span></span>

<span data-ttu-id="a7b57-220">根據應用的不同,使用`OwningComponentBase`將的範圍`DbContext`限制為單個元件*可能會*解決此問題。</span><span class="sxs-lookup"><span data-stu-id="a7b57-220">Depending on the app, using `OwningComponentBase` to limit the scope of a `DbContext` to a single component *may* solve the issue.</span></span> <span data-ttu-id="a7b57-221">`DbContext`如果元件不並行 使用 , 派生`OwningComponentBase`元件 並從 中`DbContext`派生`ScopedServices`並 檢索 就足夠了,因為它可確保:</span><span class="sxs-lookup"><span data-stu-id="a7b57-221">If a component doesn't use a `DbContext` in parallel, deriving the component from `OwningComponentBase` and retrieving the `DbContext` from `ScopedServices` is sufficient because it ensures that:</span></span>

* <span data-ttu-id="a7b57-222">單獨的元件不分享 。 `DbContext`</span><span class="sxs-lookup"><span data-stu-id="a7b57-222">Separate components don't share a `DbContext`.</span></span>
* <span data-ttu-id="a7b57-223">只`DbContext`活只要元件取決於它。</span><span class="sxs-lookup"><span data-stu-id="a7b57-223">The `DbContext` lives only as long as the component depending on it.</span></span>

<span data-ttu-id="a7b57-224">如果單個元件可能同時使用一`DbContext`個元件(例如,每次用戶選擇按鈕時),即使使用`OwningComponentBase`也不會避免併發 EF 操作的問題。</span><span class="sxs-lookup"><span data-stu-id="a7b57-224">If a single component might use a `DbContext` concurrently (for example, every time a user selects a button), even using `OwningComponentBase` doesn't avoid issues with concurrent EF operations.</span></span> <span data-ttu-id="a7b57-225">在這種情況下,`DbContext`對每個邏輯 EF 操作使用不同的操作。</span><span class="sxs-lookup"><span data-stu-id="a7b57-225">In that case, use a different `DbContext` for each logical EF operation.</span></span> <span data-ttu-id="a7b57-226">使用以下任一方法:</span><span class="sxs-lookup"><span data-stu-id="a7b57-226">Use either of the following approaches:</span></span>

* <span data-ttu-id="a7b57-227">直接使用`DbContext``DbContextOptions<TContext>`參數創建參數,可以從 DI 檢索,並且線程安全。</span><span class="sxs-lookup"><span data-stu-id="a7b57-227">Create the `DbContext` directly using `DbContextOptions<TContext>` as an argument, which can be retrieved from DI and is thread safe.</span></span>

    ```razor
    @page "/example"
    @inject DbContextOptions<AppDbContext> DbContextOptions

    <ul>
        @foreach (var item in _data)
        {
            <li>@item</li>
        }
    </ul>

    <button @onclick="LoadData">Load Data</button>

    @code {
        private List<string> _data = new List<string>();

        private async Task LoadData()
        {
            _data = await GetAsync();
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

* <span data-ttu-id="a7b57-228">在具有`DbContext`瞬態存留期的服務容器中註冊:</span><span class="sxs-lookup"><span data-stu-id="a7b57-228">Register the `DbContext` in the service container with a transient lifetime:</span></span>
  * <span data-ttu-id="a7b57-229">註冊上文時,請使用`ServiceLifetime.Transient`。</span><span class="sxs-lookup"><span data-stu-id="a7b57-229">When registering the context, use `ServiceLifetime.Transient`.</span></span> <span data-ttu-id="a7b57-230">擴展`AddDbContext`方法採用兩個類型的`ServiceLifetime`可選參數。</span><span class="sxs-lookup"><span data-stu-id="a7b57-230">The `AddDbContext` extension method takes two optional parameters of type `ServiceLifetime`.</span></span> <span data-ttu-id="a7b57-231">要使用此方法,只需要參數`contextLifetime`為`ServiceLifetime.Transient`。</span><span class="sxs-lookup"><span data-stu-id="a7b57-231">To use this approach, only the `contextLifetime` parameter needs to be `ServiceLifetime.Transient`.</span></span> <span data-ttu-id="a7b57-232">`optionsLifetime`可以保留預設值`ServiceLifetime.Scoped`。</span><span class="sxs-lookup"><span data-stu-id="a7b57-232">`optionsLifetime` can keep its default value of `ServiceLifetime.Scoped`.</span></span>

    ```csharp
    services.AddDbContext<AppDbContext>(options =>
         options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")),
         ServiceLifetime.Transient);
    ```  

  * <span data-ttu-id="a7b57-233">瞬態`DbContext`可以像正常一樣(`@inject`使用 )注入不會並行執行多個 EF 操作的元件中。</span><span class="sxs-lookup"><span data-stu-id="a7b57-233">The transient `DbContext` can be injected as normal (using `@inject`) into components that will not execute multiple EF operations in parallel.</span></span> <span data-ttu-id="a7b57-234">可以同時執行多個 EF 操作的使用者`DbContext``IServiceProvider.GetRequiredService`可以使用 請求每個並行操作的單獨物件。</span><span class="sxs-lookup"><span data-stu-id="a7b57-234">Those that may perform multiple EF operations simultaneously can request separate `DbContext` objects for each parallel operation using `IServiceProvider.GetRequiredService`.</span></span>

    ```razor
    @page "/example"
    @using Microsoft.Extensions.DependencyInjection
    @inject IServiceProvider ServiceProvider

    <ul>
        @foreach (var item in _data)
        {
            <li>@item</li>
        }
    </ul>

    <button @onclick="LoadData">Load Data</button>

    @code {
        private List<string> _data = new List<string>();

        private async Task LoadData()
        {
            _data = await GetAsync();
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

## <a name="additional-resources"></a><span data-ttu-id="a7b57-235">其他資源</span><span class="sxs-lookup"><span data-stu-id="a7b57-235">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
