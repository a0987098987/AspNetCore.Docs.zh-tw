---
title: .NET Core 中的相依性插入
author: guardrex
description: 了解 ASP.NET Core 如何實作相依性插入以及如何使用它。
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/dependency-injection
ms.openlocfilehash: 861370dc689e2420838f639ea0b1fb8f73927e16
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342415"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="0e681-103">.NET Core 中的相依性插入</span><span class="sxs-lookup"><span data-stu-id="0e681-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="0e681-104">作者：[Steve Smith](https://ardalis.com/)、[Scott Addie](https://scottaddie.com) 與 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0e681-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0e681-105">ASP.NET Core 支援相依性插入 (DI) 軟體設計模式，這是用來在類別與其相依性之間達成[控制權反轉 (IoC)](https://deviq.com/inversion-of-control/) \(英文\) 的技術。</span><span class="sxs-lookup"><span data-stu-id="0e681-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](https://deviq.com/inversion-of-control/) between classes and their dependencies.</span></span>

<span data-ttu-id="0e681-106">如需有關 MVC 控制器內相依性插入的特定詳細資訊，請參閱 <xref:mvc/controllers/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="0e681-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="0e681-107">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0e681-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="0e681-108">相依性插入概觀</span><span class="sxs-lookup"><span data-stu-id="0e681-108">Overview of dependency injection</span></span>

<span data-ttu-id="0e681-109">「相依性」是另一個物件所需的任何物件。</span><span class="sxs-lookup"><span data-stu-id="0e681-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="0e681-110">檢查具有 `WriteMessage` 方法的下列 `MyDependency` 物件，應用程式中的其他類別相依於此物件：</span><span class="sxs-lookup"><span data-stu-id="0e681-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

```csharp
public class MyDependency
{
    public MyDependency()
    {
    }

    public Task WriteMessage(string message)
    {
        Console.WriteLine(
            $"MyDependency.WriteMessage called. Message: {message}");

        return Task.FromResult(0);
    }
}
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="0e681-111">您可以建立 `MyDependency` 類別的執行個體，讓 `WriteMessage` 方法可供類別使用。</span><span class="sxs-lookup"><span data-stu-id="0e681-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="0e681-112">`MyDependency` 類別是 `IndexModel` 類別的相依性：</span><span class="sxs-lookup"><span data-stu-id="0e681-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

```csharp
public class IndexModel : PageModel
{
    MyDependency _dependency = new MyDependency();

    public async Task OnGetAsync()
    {
        await _myDependency.WriteMessage(
            "IndexModel.OnGetAsync created this message.");
    }
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="0e681-113">您可以建立 `MyDependency` 類別的執行個體，讓 `WriteMessage` 方法可供類別使用。</span><span class="sxs-lookup"><span data-stu-id="0e681-113">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="0e681-114">`MyDependency` 類別是 `HomeController` 類別的相依性：</span><span class="sxs-lookup"><span data-stu-id="0e681-114">The `MyDependency` class is a dependency of the `HomeController` class:</span></span>

```csharp
public class HomeController : Controller
{
    MyDependency _dependency = new MyDependency();

    public async Task<IActionResult> Index()
    {
        await _myDependency.WriteMessage(
            "HomeController.Index created this message.");

        return View();
    }
}
```

::: moniker-end

<span data-ttu-id="0e681-115">該類別會建立 `MyDependency` 執行個體並直接相依於該執行個體。</span><span class="sxs-lookup"><span data-stu-id="0e681-115">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="0e681-116">程式碼相依性 (例如前一個範例) 有問題，因此基於下列原因應該避免使用：</span><span class="sxs-lookup"><span data-stu-id="0e681-116">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="0e681-117">若要將 `MyDependency` 取代為不同的實作，必須修改該類別。</span><span class="sxs-lookup"><span data-stu-id="0e681-117">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="0e681-118">若 `MyDependency` 有相依性，那些相依性必須由該類別設定。</span><span class="sxs-lookup"><span data-stu-id="0e681-118">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="0e681-119">在具有多個相依於 `MyDependency` 之多個類別的大型專案中，設定程式碼在不同的應用程式之間會變得鬆散。</span><span class="sxs-lookup"><span data-stu-id="0e681-119">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="0e681-120">此實作難以進行單元測試。</span><span class="sxs-lookup"><span data-stu-id="0e681-120">This implementation is difficult to unit test.</span></span> <span data-ttu-id="0e681-121">應用程式應該使用模擬 (Mock) 或虛設常式 (Stub) `MyDependency` 類別，這在使用此方法時無法使用。</span><span class="sxs-lookup"><span data-stu-id="0e681-121">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="0e681-122">相依性插入可透過下列方式解決這些問題：</span><span class="sxs-lookup"><span data-stu-id="0e681-122">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="0e681-123">使用介面來將相依性資訊抽象化。</span><span class="sxs-lookup"><span data-stu-id="0e681-123">The use of an interface to abstract the dependency implementation.</span></span>
* <span data-ttu-id="0e681-124">在服務容器中註冊相依性。</span><span class="sxs-lookup"><span data-stu-id="0e681-124">Registration of the dependency in a service container.</span></span> <span data-ttu-id="0e681-125">ASP.NET Core 提供內建服務容器 [IServiceProvider](/dotnet/api/system.iserviceprovider)。</span><span class="sxs-lookup"><span data-stu-id="0e681-125">ASP.NET Core provides a built-in service container, [IServiceProvider](/dotnet/api/system.iserviceprovider).</span></span> <span data-ttu-id="0e681-126">服務會在應用程式的 `Startup.ConfigureServices` 方法中註冊。</span><span class="sxs-lookup"><span data-stu-id="0e681-126">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="0e681-127">將服務「插入」到服務使用位置之類別的建構函式。</span><span class="sxs-lookup"><span data-stu-id="0e681-127">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="0e681-128">架構會負責建立相依性的執行個體，並在不再需要時將它捨棄。</span><span class="sxs-lookup"><span data-stu-id="0e681-128">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="0e681-129">在[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples)中，`IMyDependency` 介面定義了服務提供給應用程式的方法：</span><span class="sxs-lookup"><span data-stu-id="0e681-129">In the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0e681-130">這個介面是由具象型別 `MyDependency` 所實作：</span><span class="sxs-lookup"><span data-stu-id="0e681-130">This interface is implemented by a concrete type, `MyDependency`:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0e681-131">`MyDependency` 在其建構函式中要求 [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1)。</span><span class="sxs-lookup"><span data-stu-id="0e681-131">`MyDependency` requests an [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) in its constructor.</span></span> <span data-ttu-id="0e681-132">以鏈結方式使用相依性插入並非不尋常。</span><span class="sxs-lookup"><span data-stu-id="0e681-132">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="0e681-133">每個要求的相依性接著會要求其自己的相依性。</span><span class="sxs-lookup"><span data-stu-id="0e681-133">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="0e681-134">容器會解決圖形中的相依性，並傳回完全解析的服務。</span><span class="sxs-lookup"><span data-stu-id="0e681-134">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="0e681-135">必須先解析的相依性集合組通常稱為「相依性樹狀結構」、「相依性圖形」或「物件圖形」。</span><span class="sxs-lookup"><span data-stu-id="0e681-135">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="0e681-136">`IMyDependency` 與 `ILogger<TCategoryName>` 必須在服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="0e681-136">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="0e681-137">`IMyDependency` 是在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="0e681-137">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0e681-138">`ILogger<TCategoryName>` 是由記錄抽象基礎結構所註冊，所以它是預設由架構所註冊的[架構提供的服務](#framework-provided-services)。</span><span class="sxs-lookup"><span data-stu-id="0e681-138">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="0e681-139">在範例應用程式中，`IMyDependency` 服務是使用具象型別 `MyDependency` 所註冊。</span><span class="sxs-lookup"><span data-stu-id="0e681-139">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="0e681-140">註冊會將服務的存留期範圍限制為單一要求的存留期。</span><span class="sxs-lookup"><span data-stu-id="0e681-140">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="0e681-141">將在此主題稍後將說明[服務存留期](#service-lifetimes)。</span><span class="sxs-lookup"><span data-stu-id="0e681-141">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="0e681-142">每個 `services.Add<ServiceName>` 擴充方法都會新增 (並且可能會設定) 服務。</span><span class="sxs-lookup"><span data-stu-id="0e681-142">Each `services.Add<ServiceName>` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="0e681-143">例如，`services.AddMvc()` 會新增 Razor Pages 與 MVC 要求的服務。</span><span class="sxs-lookup"><span data-stu-id="0e681-143">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="0e681-144">建議應用程式遵循此慣例。</span><span class="sxs-lookup"><span data-stu-id="0e681-144">We recommended that apps follow this convention.</span></span> <span data-ttu-id="0e681-145">在 [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) 命名空間中放置擴充方法，以封裝服務註冊群組。</span><span class="sxs-lookup"><span data-stu-id="0e681-145">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="0e681-146">如果服務的建構函式需要基本類型 (例如 `string`)，基本類型可以藉由使用[設定](xref:fundamentals/configuration/index)或[選項模式](xref:fundamentals/configuration/options)來插入：</span><span class="sxs-lookup"><span data-stu-id="0e681-146">If the service's constructor requires a primitive, such as a `string`, the primitive can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

```csharp
public class MyDependency : IMyDependency
{
    public MyDependency(IConfiguration config)
    {
        var myStringValue = config["MyStringKey"];

        // Use myStringValue
    }

    ...
}
```

<span data-ttu-id="0e681-147">服務執行個體是透過服務使用所在之類別建構函式所要求，而且會指派給私人欄位。</span><span class="sxs-lookup"><span data-stu-id="0e681-147">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="0e681-148">該欄位會視需要用來存取整個類別中的服務。</span><span class="sxs-lookup"><span data-stu-id="0e681-148">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="0e681-149">在範例應用程式中，會要求 `IMyDependency` 執行個體並使用它來呼叫服務的 `WriteMessage` 方法：</span><span class="sxs-lookup"><span data-stu-id="0e681-149">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/MyDependencyController.cs?name=snippet1&highlight=3,5-8,13-14)]

::: moniker-end

## <a name="framework-provided-services"></a><span data-ttu-id="0e681-150">架構提供的服務</span><span class="sxs-lookup"><span data-stu-id="0e681-150">Framework-provided services</span></span>

<span data-ttu-id="0e681-151">`Startup.ConfigureServices` 方法負責定義應用程式將使用的服務，包括像是 Entity Framework Core 與 ASP.NET Core MVC 等平台功能。</span><span class="sxs-lookup"><span data-stu-id="0e681-151">The `Startup.ConfigureServices` method is responsible for defining the services the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="0e681-152">一開始，提供給 `ConfigureServices` 的 `IServiceCollection` 具有下列已定義的服務 (取決於[主機的設定方式](xref:fundamentals/host/index))：</span><span class="sxs-lookup"><span data-stu-id="0e681-152">Initially, the `IServiceCollection` provided to `ConfigureServices` has the following services defined (depending on [how the host was configured](xref:fundamentals/host/index)):</span></span>

| <span data-ttu-id="0e681-153">服務類型</span><span class="sxs-lookup"><span data-stu-id="0e681-153">Service Type</span></span> | <span data-ttu-id="0e681-154">存留期</span><span class="sxs-lookup"><span data-stu-id="0e681-154">Lifetime</span></span> |
| ------------ | -------- |
| [<span data-ttu-id="0e681-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span><span class="sxs-lookup"><span data-stu-id="0e681-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | <span data-ttu-id="0e681-156">暫時性</span><span class="sxs-lookup"><span data-stu-id="0e681-156">Transient</span></span> |
| [<span data-ttu-id="0e681-157">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="0e681-157">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | <span data-ttu-id="0e681-158">單一</span><span class="sxs-lookup"><span data-stu-id="0e681-158">Singleton</span></span> |
| [<span data-ttu-id="0e681-159">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="0e681-159">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | <span data-ttu-id="0e681-160">單一</span><span class="sxs-lookup"><span data-stu-id="0e681-160">Singleton</span></span> |
| [<span data-ttu-id="0e681-161">Microsoft.AspNetCore.Hosting.IStartup</span><span class="sxs-lookup"><span data-stu-id="0e681-161">Microsoft.AspNetCore.Hosting.IStartup</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | <span data-ttu-id="0e681-162">單一</span><span class="sxs-lookup"><span data-stu-id="0e681-162">Singleton</span></span> |
| [<span data-ttu-id="0e681-163">Microsoft.AspNetCore.Hosting.IStartupFilter</span><span class="sxs-lookup"><span data-stu-id="0e681-163">Microsoft.AspNetCore.Hosting.IStartupFilter</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | <span data-ttu-id="0e681-164">暫時性</span><span class="sxs-lookup"><span data-stu-id="0e681-164">Transient</span></span> |
| [<span data-ttu-id="0e681-165">Microsoft.AspNetCore.Hosting.Server.IServer</span><span class="sxs-lookup"><span data-stu-id="0e681-165">Microsoft.AspNetCore.Hosting.Server.IServer</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | <span data-ttu-id="0e681-166">單一</span><span class="sxs-lookup"><span data-stu-id="0e681-166">Singleton</span></span> |
| [<span data-ttu-id="0e681-167">Microsoft.AspNetCore.Http.IHttpContextFactory</span><span class="sxs-lookup"><span data-stu-id="0e681-167">Microsoft.AspNetCore.Http.IHttpContextFactory</span></span>](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | <span data-ttu-id="0e681-168">暫時性</span><span class="sxs-lookup"><span data-stu-id="0e681-168">Transient</span></span> |
| [<span data-ttu-id="0e681-169">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="0e681-169">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.logging.ilogger) | <span data-ttu-id="0e681-170">單一</span><span class="sxs-lookup"><span data-stu-id="0e681-170">Singleton</span></span> |
| [<span data-ttu-id="0e681-171">Microsoft.Extensions.Logging.ILoggerFactory</span><span class="sxs-lookup"><span data-stu-id="0e681-171">Microsoft.Extensions.Logging.ILoggerFactory</span></span>](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | <span data-ttu-id="0e681-172">單一</span><span class="sxs-lookup"><span data-stu-id="0e681-172">Singleton</span></span> |
| [<span data-ttu-id="0e681-173">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span><span class="sxs-lookup"><span data-stu-id="0e681-173">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span></span>](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | <span data-ttu-id="0e681-174">單一</span><span class="sxs-lookup"><span data-stu-id="0e681-174">Singleton</span></span> |
| [<span data-ttu-id="0e681-175">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="0e681-175">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | <span data-ttu-id="0e681-176">暫時性</span><span class="sxs-lookup"><span data-stu-id="0e681-176">Transient</span></span> |
| [<span data-ttu-id="0e681-177">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="0e681-177">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.ioptions-1) | <span data-ttu-id="0e681-178">單一</span><span class="sxs-lookup"><span data-stu-id="0e681-178">Singleton</span></span> |
| [<span data-ttu-id="0e681-179">System.Diagnostics.DiagnosticSource</span><span class="sxs-lookup"><span data-stu-id="0e681-179">System.Diagnostics.DiagnosticSource</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | <span data-ttu-id="0e681-180">單一</span><span class="sxs-lookup"><span data-stu-id="0e681-180">Singleton</span></span> |
| [<span data-ttu-id="0e681-181">System.Diagnostics.DiagnosticListener</span><span class="sxs-lookup"><span data-stu-id="0e681-181">System.Diagnostics.DiagnosticListener</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | <span data-ttu-id="0e681-182">單一</span><span class="sxs-lookup"><span data-stu-id="0e681-182">Singleton</span></span> |

<span data-ttu-id="0e681-183">當可以使用服務集合擴充方法來註冊服務 (如果需要，也可以註冊其相依服務) 時，慣例是使用單一 `Add<ServiceName>` 擴充方法來註冊該服務要求的所有服務。</span><span class="sxs-lookup"><span data-stu-id="0e681-183">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add<ServiceName>` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="0e681-184">下列程式碼範例示範如何使用擴充方法 [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)、[AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity) 與 [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc) 將額外服務新增到容器中：</span><span class="sxs-lookup"><span data-stu-id="0e681-184">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity), and [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddMvc();
}
```

<span data-ttu-id="0e681-185">如需詳細資訊，請參閱 API 文件中的 [ServiceCollection Class](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection)。</span><span class="sxs-lookup"><span data-stu-id="0e681-185">For more information, see the [ServiceCollection Class](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection) in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="0e681-186">執行個體存留期</span><span class="sxs-lookup"><span data-stu-id="0e681-186">Service lifetimes</span></span>

<span data-ttu-id="0e681-187">為每個已註冊的服務選擇適當的存留期。</span><span class="sxs-lookup"><span data-stu-id="0e681-187">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="0e681-188">ASP.NET Core 服務可以使用下列存留期進行設定：</span><span class="sxs-lookup"><span data-stu-id="0e681-188">ASP.NET Core services can be configured with the following lifetimes:</span></span>

<span data-ttu-id="0e681-189">**暫時性**</span><span class="sxs-lookup"><span data-stu-id="0e681-189">**Transient**</span></span>

<span data-ttu-id="0e681-190">每次要求暫時性存留期服務時都會建立它們。</span><span class="sxs-lookup"><span data-stu-id="0e681-190">Transient lifetime services are created each time they're requested.</span></span> <span data-ttu-id="0e681-191">此存留期最適合用於輕量型的無狀態服務。</span><span class="sxs-lookup"><span data-stu-id="0e681-191">This lifetime works best for lightweight, stateless services.</span></span>

<span data-ttu-id="0e681-192">**具範圍**</span><span class="sxs-lookup"><span data-stu-id="0e681-192">**Scoped**</span></span>

<span data-ttu-id="0e681-193">具範圍存留期服務會針對每個要求建立一次。</span><span class="sxs-lookup"><span data-stu-id="0e681-193">Scoped lifetime services are created once per request.</span></span>

> [!WARNING]
> <span data-ttu-id="0e681-194">在中介軟體中使用具範圍服務時，請將該服務插入 `Invoke` 或 `InvokeAsync` 方法中。</span><span class="sxs-lookup"><span data-stu-id="0e681-194">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="0e681-195">因為其會強制服務執行單一服務的行為，所以請勿透過建構函式插入進行插入。</span><span class="sxs-lookup"><span data-stu-id="0e681-195">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="0e681-196">如需詳細資訊，請參閱<xref:fundamentals/middleware/index>。</span><span class="sxs-lookup"><span data-stu-id="0e681-196">For more information, see <xref:fundamentals/middleware/index>.</span></span>

<span data-ttu-id="0e681-197">**單一**</span><span class="sxs-lookup"><span data-stu-id="0e681-197">**Singleton**</span></span>

<span data-ttu-id="0e681-198">當第一次收到有關單一資料庫存留期服務的要求時 (或是當執行 `ConfigureServices` 而且隨著服務註冊指定執行個體時)，即會建立單一資料庫存留期服務。</span><span class="sxs-lookup"><span data-stu-id="0e681-198">Singleton lifetime services are created the first time they're requested (or when `ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="0e681-199">每個後續要求都會使用相同的執行個體。</span><span class="sxs-lookup"><span data-stu-id="0e681-199">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="0e681-200">若應用程式要求單一資料庫行為，建議您允許服務容器管理服務的存留期。</span><span class="sxs-lookup"><span data-stu-id="0e681-200">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="0e681-201">不要實作單一資料庫設計模式並為使用者提供可用來管理類別中物件存留期的程式碼。</span><span class="sxs-lookup"><span data-stu-id="0e681-201">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="0e681-202">從單一資料庫解析具範圍服務是非常危險的。</span><span class="sxs-lookup"><span data-stu-id="0e681-202">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="0e681-203">處理後續要求時，它可能會導致服務有不正確的狀態。</span><span class="sxs-lookup"><span data-stu-id="0e681-203">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

### <a name="constructor-injection-behavior"></a><span data-ttu-id="0e681-204">建構函式插入行為</span><span class="sxs-lookup"><span data-stu-id="0e681-204">Constructor injection behavior</span></span>

<span data-ttu-id="0e681-205">服務可以透過兩個機制來解析：</span><span class="sxs-lookup"><span data-stu-id="0e681-205">Services can be resolved by two mechanisms:</span></span>

* `IServiceProvider`
* <span data-ttu-id="0e681-206">[ActivatorUtilities](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) &ndash; 允許建立物件，而不需要在相依性插入容器中註冊服務。</span><span class="sxs-lookup"><span data-stu-id="0e681-206">[ActivatorUtilities](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="0e681-207">搭配使用者面向抽象 (標籤協助程式、MVC 控制器、SignalR 中樞與模型繫結器) 使用 `ActivatorUtilities`。</span><span class="sxs-lookup"><span data-stu-id="0e681-207">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, SignalR Hubs, and model binders.</span></span>

<span data-ttu-id="0e681-208">建構函式可以接受不是由相依性插入提供的引數，但引數必須指派預設值。</span><span class="sxs-lookup"><span data-stu-id="0e681-208">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="0e681-209">當服務由 `IServiceProvider` 或 `ActivatorUtilities` 解析時，建構函式插入會要求 *public*建構函式。</span><span class="sxs-lookup"><span data-stu-id="0e681-209">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="0e681-210">當服務由 `ActivatorUtilities` 解析時，建構函式插入只要求只能有一個適用的建構函式存在。</span><span class="sxs-lookup"><span data-stu-id="0e681-210">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exist.</span></span> <span data-ttu-id="0e681-211">支援建構函式多載，但只能有一個多載存在，其引數可以藉由相依性插入而完成。</span><span class="sxs-lookup"><span data-stu-id="0e681-211">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="0e681-212">Entity Framework 內容</span><span class="sxs-lookup"><span data-stu-id="0e681-212">Entity Framework contexts</span></span>

<span data-ttu-id="0e681-213">Entity Framework 內容應該使用具範圍存留期新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="0e681-213">Entity Framework contexts should be added to the service container using the scoped lifetime.</span></span> <span data-ttu-id="0e681-214">這是在註冊資料庫內容時透過對 [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) 方法的呼叫來自動處理的。</span><span class="sxs-lookup"><span data-stu-id="0e681-214">This is handled automatically with a call to the [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) method when registering the database context.</span></span> <span data-ttu-id="0e681-215">使用資料庫內容的服務也應該使用具範圍存留期。</span><span class="sxs-lookup"><span data-stu-id="0e681-215">Services that use the database context should also use the scoped lifetime.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="0e681-216">留期和註冊選項</span><span class="sxs-lookup"><span data-stu-id="0e681-216">Lifetime and registration options</span></span>

<span data-ttu-id="0e681-217">為了示範這些存留期和註冊選項之間的差異，請考慮下列介面，以具有唯一識別碼 `OperationId` 的「作業」代表工作。</span><span class="sxs-lookup"><span data-stu-id="0e681-217">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="0e681-218">視如何針對下列介面設定作業服務存留期而定，當類別要求時，容器會提供相同或不同的服務執行個體：</span><span class="sxs-lookup"><span data-stu-id="0e681-218">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0e681-219">介面是在 `Operation` 類別中實作。</span><span class="sxs-lookup"><span data-stu-id="0e681-219">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="0e681-220">若未提供 GUID，`Operation` 建構函式會產生 GUID：</span><span class="sxs-lookup"><span data-stu-id="0e681-220">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0e681-221">會註冊相依於每種其他 `Operation` 類型的 `OperationService`。</span><span class="sxs-lookup"><span data-stu-id="0e681-221">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="0e681-222">當透過相依性插入要求 `OperationService` 時，它會根據相依服務的存留期擷取每個服務的新執行個體或現有執行個體。</span><span class="sxs-lookup"><span data-stu-id="0e681-222">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="0e681-223">若在收到要求時建立暫時性服務，`IOperationTransient` 服務的 `OperationsId` 會與 `OperationService` 的 `OperationsId` 不同。</span><span class="sxs-lookup"><span data-stu-id="0e681-223">If transient services are created when requested, the `OperationsId` of the `IOperationTransient` service is different than the `OperationsId` of the `OperationService`.</span></span> <span data-ttu-id="0e681-224">`OperationService` 會收到 `IOperationTransient` 類別的新執行個體。</span><span class="sxs-lookup"><span data-stu-id="0e681-224">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="0e681-225">新執行個體會產生不同的 `OperationsId`。</span><span class="sxs-lookup"><span data-stu-id="0e681-225">The new instance yields a different `OperationsId`.</span></span>
* <span data-ttu-id="0e681-226">若在收到要求時建立具範圍服務，`IOperationScoped` 服務的 `OperationsId` `OperationService` 的項目相同。</span><span class="sxs-lookup"><span data-stu-id="0e681-226">If scoped services are created per request, the `OperationsId` of the `IOperationScoped` service is the same as that of `OperationService` within a request.</span></span> <span data-ttu-id="0e681-227">在不同的要求中，兩個服務會共用不同的 `OperationsId` 值。</span><span class="sxs-lookup"><span data-stu-id="0e681-227">Across requests, both services share a different `OperationsId` value.</span></span>
* <span data-ttu-id="0e681-228">若建立一資料庫與單一資料庫執行個體服務並在所有要求與所有服務中使用，`OperationsId` 在所有服務要求中會固定不變。</span><span class="sxs-lookup"><span data-stu-id="0e681-228">If singleton and singleton-instance services are created once and used across all requests and all services, the `OperationsId` is constant across all service requests.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0e681-229">在 `Startup.ConfigureServices` 中，每個類型都會根據其具名存留期新增至容器：</span><span class="sxs-lookup"><span data-stu-id="0e681-229">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=12-15,18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

::: moniker-end

<span data-ttu-id="0e681-230">`IOperationSingletonInstance` 服務使用具有 `Guid.Empty` 已知識別碼的特定執行個體。</span><span class="sxs-lookup"><span data-stu-id="0e681-230">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="0e681-231">很明顯此型別是使用中 (其 GUID 是所有區域)。</span><span class="sxs-lookup"><span data-stu-id="0e681-231">It's clear when this type is in use (its GUID is all zeroes).</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="0e681-232">範例應用程式示範個別要求內與個別要求之間的物件存留期。</span><span class="sxs-lookup"><span data-stu-id="0e681-232">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="0e681-233">範例應用程式的 `IndexModel` 會要求每種 `IOperation` 型別與 `OperationService`。</span><span class="sxs-lookup"><span data-stu-id="0e681-233">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="0e681-234">頁面接著會透過屬性指派來顯示所有頁面模型類別與服務的 `OperationId` 值：</span><span class="sxs-lookup"><span data-stu-id="0e681-234">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="0e681-235">範例應用程式示範個別要求內與個別要求之間的物件存留期。</span><span class="sxs-lookup"><span data-stu-id="0e681-235">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="0e681-236">範例應用程式包括會要求每種 `IOperation` 類型與 `OperationService` 的 `OperationsController` 要求。</span><span class="sxs-lookup"><span data-stu-id="0e681-236">The sample app includes an `OperationsController` that requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="0e681-237">`Index` 動作會將服務設定為 `ViewBag` 以顯示服務的 `OperationId` 值：</span><span class="sxs-lookup"><span data-stu-id="0e681-237">The `Index` action sets the services into the `ViewBag` for display of the service's `OperationId` values:</span></span>

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/OperationsController.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0e681-238">下面兩個輸出顯示兩個要求的結果：</span><span class="sxs-lookup"><span data-stu-id="0e681-238">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="0e681-239">**第一個要求：**</span><span class="sxs-lookup"><span data-stu-id="0e681-239">**First request:**</span></span>

<span data-ttu-id="0e681-240">控制器作業：</span><span class="sxs-lookup"><span data-stu-id="0e681-240">Controller operations:</span></span>

<span data-ttu-id="0e681-241">暫時性： d233e165-f417-469b-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="0e681-241">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="0e681-242">具範圍： 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="0e681-242">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="0e681-243">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="0e681-243">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="0e681-244">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="0e681-244">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="0e681-245">`OperationService` 作業：</span><span class="sxs-lookup"><span data-stu-id="0e681-245">`OperationService` operations:</span></span>

<span data-ttu-id="0e681-246">暫時性： c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="0e681-246">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="0e681-247">具範圍： 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="0e681-247">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="0e681-248">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="0e681-248">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="0e681-249">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="0e681-249">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="0e681-250">**:第二個要求：**</span><span class="sxs-lookup"><span data-stu-id="0e681-250">**Second request:**</span></span>

<span data-ttu-id="0e681-251">控制器作業：</span><span class="sxs-lookup"><span data-stu-id="0e681-251">Controller operations:</span></span>

<span data-ttu-id="0e681-252">暫時性： b63bd538-0a37-4ff1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="0e681-252">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="0e681-253">具範圍： 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="0e681-253">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="0e681-254">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="0e681-254">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="0e681-255">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="0e681-255">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="0e681-256">`OperationService` 作業：</span><span class="sxs-lookup"><span data-stu-id="0e681-256">`OperationService` operations:</span></span>

<span data-ttu-id="0e681-257">暫時性： c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="0e681-257">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="0e681-258">具範圍： 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="0e681-258">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="0e681-259">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="0e681-259">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="0e681-260">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="0e681-260">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="0e681-261">觀察哪些 `OperationId` 值在要求內以及要求之間不同：</span><span class="sxs-lookup"><span data-stu-id="0e681-261">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="0e681-262">「暫時性」 物件一律不同。</span><span class="sxs-lookup"><span data-stu-id="0e681-262">*Transient* objects are always different.</span></span> <span data-ttu-id="0e681-263">請注意，第一個與第二個要求的暫時性 `OperationId` 值在兩個 `OperationService` 作業之間與各要求之間都不同。</span><span class="sxs-lookup"><span data-stu-id="0e681-263">Note that the transient `OperationId` value for both the first and second requests are different for both `OperationService` operations and across requests.</span></span> <span data-ttu-id="0e681-264">新執行個體會提供給每個服務與要求。</span><span class="sxs-lookup"><span data-stu-id="0e681-264">A new instance is provided to each service and request.</span></span>
* <span data-ttu-id="0e681-265">「具範圍」物件在要求內相同，但在不同的要求之間會不同。</span><span class="sxs-lookup"><span data-stu-id="0e681-265">*Scoped* objects are the same within a request but different across requests.</span></span>
* <span data-ttu-id="0e681-266">「單一資料庫」物件對於每個物件與每個要求都相同 (不論 `Operation` 執行個體是否提供於 `ConfigureServices`)。</span><span class="sxs-lookup"><span data-stu-id="0e681-266">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="0e681-267">從主要呼叫服務</span><span class="sxs-lookup"><span data-stu-id="0e681-267">Call services from main</span></span>

<span data-ttu-id="0e681-268">使用 [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) 建立 [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope)，以解析應用程式範圍中的具範圍服務。</span><span class="sxs-lookup"><span data-stu-id="0e681-268">Create an [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) with [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="0e681-269">此法可用於在開機時存取具範圍服務，以執行初始化工作。</span><span class="sxs-lookup"><span data-stu-id="0e681-269">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="0e681-270">下列範例示範如何在 `Program.Main` 中取得 `MyScopedService`：</span><span class="sxs-lookup"><span data-stu-id="0e681-270">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = CreateWebHostBuilder(args).Build();

    using (var serviceScope = host.Services.CreateScope())
    {
        var services = serviceScope.ServiceProvider;

        try
        {
            var serviceContext = services.GetRequiredService<MyScopedService>();
            // Use the context here
        }
        catch (Exception ex)
        {
            var logger = services.GetRequiredService<ILogger<Program>>();
            logger.LogError(ex, "An error occurred.");
        }
    }

    host.Run();
}
```

## <a name="scope-validation"></a><span data-ttu-id="0e681-271">範圍驗證</span><span class="sxs-lookup"><span data-stu-id="0e681-271">Scope validation</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0e681-272">當應用程式在開發環境中執行時，預設服務提供者會執行檢查以確認：</span><span class="sxs-lookup"><span data-stu-id="0e681-272">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="0e681-273">具範圍服務不是直接或間接由根服務提供者解析。</span><span class="sxs-lookup"><span data-stu-id="0e681-273">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="0e681-274">具範圍服務不是直接或間接插入至單一服務。</span><span class="sxs-lookup"><span data-stu-id="0e681-274">Scoped services aren't directly or indirectly injected into singletons.</span></span>

::: moniker-end

<span data-ttu-id="0e681-275">根服務提供者會在呼叫 [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) 時建立。</span><span class="sxs-lookup"><span data-stu-id="0e681-275">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="0e681-276">當提供者啟動應用程式時，根服務提供者的存留期與應用程式/伺服器的存留期一致，並會在應用程式關閉時處置。</span><span class="sxs-lookup"><span data-stu-id="0e681-276">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="0e681-277">具範圍服務會由建立這些服務的容器處置。</span><span class="sxs-lookup"><span data-stu-id="0e681-277">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="0e681-278">若是在根容器中建立具範圍服務，因為當應用程式/伺服器關機時，服務只會由根容器處理，所以服務的存留期會提升為單一服務等級。</span><span class="sxs-lookup"><span data-stu-id="0e681-278">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="0e681-279">當呼叫 `BuildServiceProvider` 時，驗證服務範圍會攔截到這些情況。</span><span class="sxs-lookup"><span data-stu-id="0e681-279">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="0e681-280">如需詳細資訊，請參閱<xref:fundamentals/host/web-host#scope-validation>。</span><span class="sxs-lookup"><span data-stu-id="0e681-280">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="0e681-281">要求服務</span><span class="sxs-lookup"><span data-stu-id="0e681-281">Request Services</span></span>

<span data-ttu-id="0e681-282">來自 `HttpContext`，在 ASP.NET Core 要求內提供的服務是透過 [HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices) 集合公開。</span><span class="sxs-lookup"><span data-stu-id="0e681-282">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices) collection.</span></span>

<span data-ttu-id="0e681-283">要求服務代表您在應用程式中設定和要求的服務。</span><span class="sxs-lookup"><span data-stu-id="0e681-283">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="0e681-284">當物件指定相依性時，這些會由在 `RequestServices` 中找到的類型來滿足，而非 `ApplicationServices`。</span><span class="sxs-lookup"><span data-stu-id="0e681-284">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="0e681-285">一般而言，應用程式不應該直接使用這些屬性。</span><span class="sxs-lookup"><span data-stu-id="0e681-285">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="0e681-286">相反地，透過類別建構函式要求類別所需的型別，以及允許架構插入相依性。</span><span class="sxs-lookup"><span data-stu-id="0e681-286">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="0e681-287">如此產生的類別將更較容易測試 (請參閱[測試與偵錯](xref:test/index)主題)。</span><span class="sxs-lookup"><span data-stu-id="0e681-287">This yields classes that are easier to test (see the [Test and debug](xref:test/index) topics).</span></span>

> [!NOTE]
> <span data-ttu-id="0e681-288">偏好要求相依性作為建構函式參數，而不要存取 `RequestServices` 集合。</span><span class="sxs-lookup"><span data-stu-id="0e681-288">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="0e681-289">針對相依性插入設計服務</span><span class="sxs-lookup"><span data-stu-id="0e681-289">Design services for dependency injection</span></span>

<span data-ttu-id="0e681-290">最佳做法是：</span><span class="sxs-lookup"><span data-stu-id="0e681-290">Best practices are to:</span></span>

* <span data-ttu-id="0e681-291">設計服務以使用相依性插入來取得其相依性。</span><span class="sxs-lookup"><span data-stu-id="0e681-291">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="0e681-292">避免具狀態的靜態方法呼叫 (一個稱為[靜電依附](https://deviq.com/static-cling/)的實踐)。</span><span class="sxs-lookup"><span data-stu-id="0e681-292">Avoid stateful, static method calls (a practice known as [static cling](https://deviq.com/static-cling/)).</span></span>
* <span data-ttu-id="0e681-293">避免直接在服務內具現化相依類別。</span><span class="sxs-lookup"><span data-stu-id="0e681-293">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="0e681-294">直接具現化會將程式碼耦合到特定實作。</span><span class="sxs-lookup"><span data-stu-id="0e681-294">Direct instantiation couples the code to a particular implementation.</span></span>

<span data-ttu-id="0e681-295">藉由遵循[物件導向設計的 SOLID 準則](https://deviq.com/solid/)，應用程式類別自然而然會傾向小型、構造良好且可輕鬆地測試。</span><span class="sxs-lookup"><span data-stu-id="0e681-295">By following the [SOLID Principles of Object Oriented Design](https://deviq.com/solid/), app classes naturally tend to be small, well-factored, and easily tested.</span></span>

<span data-ttu-id="0e681-296">若類別有太多插入的相依性，這通常表示類別有太多責任而且正違反[單一責任原則 (SRP)](https://deviq.com/single-responsibility-principle/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="0e681-296">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](https://deviq.com/single-responsibility-principle/).</span></span> <span data-ttu-id="0e681-297">將類別負責的某些部分移到新的類別，以嘗試重構類別。</span><span class="sxs-lookup"><span data-stu-id="0e681-297">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="0e681-298">請記住，Razor Pages 頁面模型類別與 MVC 控制器類別應該專注於 UI 考量。</span><span class="sxs-lookup"><span data-stu-id="0e681-298">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="0e681-299">商務規則和資料存取實作詳細資料應該保存在適合這些[分開考量](https://deviq.com/separation-of-concerns/) (Separation of Concerns) 類別中。</span><span class="sxs-lookup"><span data-stu-id="0e681-299">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](https://deviq.com/separation-of-concerns/).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="0e681-300">處置服務</span><span class="sxs-lookup"><span data-stu-id="0e681-300">Disposal of services</span></span>

<span data-ttu-id="0e681-301">容器會為它建立的 `IDisposable` 類型呼叫 `Dispose`。</span><span class="sxs-lookup"><span data-stu-id="0e681-301">The container calls `Dispose` for the `IDisposable` types it creates.</span></span> <span data-ttu-id="0e681-302">若執行個體由使用者程式碼新增到容器中，它將不會自動處置。</span><span class="sxs-lookup"><span data-stu-id="0e681-302">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

```csharp
// Services that implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    // The container creates the following instances and disposes them automatically:
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // The container doesn't create the following instances, so it doesn't dispose of
    // the instances automatically:
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

::: moniker range="= aspnetcore-1.0"

> [!NOTE]
> <span data-ttu-id="0e681-303">在 ASP.NET Core 1.0 中，容器會對「所有」`IDisposable` 物件呼叫 dispose，包括它未建立的物件。</span><span class="sxs-lookup"><span data-stu-id="0e681-303">In ASP.NET Core 1.0, the container calls dispose on *all* `IDisposable` objects, including those it didn't create.</span></span>

::: moniker-end

## <a name="default-service-container-replacement"></a><span data-ttu-id="0e681-304">預設服務容器取代</span><span class="sxs-lookup"><span data-stu-id="0e681-304">Default service container replacement</span></span>

<span data-ttu-id="0e681-305">內建的服務容器是要服務架構和建置在其上的大部分取用者應用程式的基本需求。</span><span class="sxs-lookup"><span data-stu-id="0e681-305">The built-in service container is meant to serve the basic needs of the framework and most consumer apps built on it.</span></span> <span data-ttu-id="0e681-306">不過，開發人員可以使用其偏好的容器取代內建的容器。</span><span class="sxs-lookup"><span data-stu-id="0e681-306">However, developers can replace the built-in container with their preferred container.</span></span> <span data-ttu-id="0e681-307">`Startup.ConfigureServices` 方法通常會傳回 `void`。</span><span class="sxs-lookup"><span data-stu-id="0e681-307">The `Startup.ConfigureServices` method typically returns `void`.</span></span> <span data-ttu-id="0e681-308">如果方法的簽章變更為傳回 [IServiceProvider](/dotnet/api/system.iserviceprovider)，則可以設定並傳回不同的容器。</span><span class="sxs-lookup"><span data-stu-id="0e681-308">If the method's signature is changed to return an [IServiceProvider](/dotnet/api/system.iserviceprovider), a different container can be configured and returned.</span></span> <span data-ttu-id="0e681-309">.NET 有許多 IoC 容器可用。</span><span class="sxs-lookup"><span data-stu-id="0e681-309">There are many IoC containers available for .NET.</span></span> <span data-ttu-id="0e681-310">在下列範例中，我們使用了 [Autofac](https://autofac.org/) 容器：</span><span class="sxs-lookup"><span data-stu-id="0e681-310">In the following example, the [Autofac](https://autofac.org/) container is used:</span></span>

1. <span data-ttu-id="0e681-311">安裝適當的容器套件：</span><span class="sxs-lookup"><span data-stu-id="0e681-311">Install the appropriate container package(s):</span></span>

    * [<span data-ttu-id="0e681-312">Autofac</span><span class="sxs-lookup"><span data-stu-id="0e681-312">Autofac</span></span>](https://www.nuget.org/packages/Autofac/)
    * [<span data-ttu-id="0e681-313">Autofac.Extensions.DependencyInjection</span><span class="sxs-lookup"><span data-stu-id="0e681-313">Autofac.Extensions.DependencyInjection</span></span>](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/)

2. <span data-ttu-id="0e681-314">在 `Startup.ConfigureServices` 中設定容器並傳回 `IServiceProvider`：</span><span class="sxs-lookup"><span data-stu-id="0e681-314">Configure the container in `Startup.ConfigureServices` and return an `IServiceProvider`:</span></span>

    ```csharp
    public IServiceProvider ConfigureServices(IServiceCollection services)
    {
        services.AddMvc();
        // Add other framework services

        // Add Autofac
        var containerBuilder = new ContainerBuilder();
        containerBuilder.RegisterModule<DefaultModule>();
        containerBuilder.Populate(services);
        var container = containerBuilder.Build();
        return new AutofacServiceProvider(container);
    }
    ```

    <span data-ttu-id="0e681-315">若要使用第三方容器，`Startup.ConfigureServices` 必須傳回 `IServiceProvider`。</span><span class="sxs-lookup"><span data-stu-id="0e681-315">To use a 3rd party container, `Startup.ConfigureServices` must return `IServiceProvider`.</span></span>

3. <span data-ttu-id="0e681-316">在 `DefaultModule` 中設定 Autofac：</span><span class="sxs-lookup"><span data-stu-id="0e681-316">Configure Autofac in `DefaultModule`:</span></span>

    ```csharp
    public class DefaultModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
        }
    }
    ```

<span data-ttu-id="0e681-317">在執行階段，Autofac 會用來解析類型及插入相依性。</span><span class="sxs-lookup"><span data-stu-id="0e681-317">At runtime, Autofac is used to resolve types and inject dependencies.</span></span> <span data-ttu-id="0e681-318">若要深入了解如何搭配 ASP.NET Core 使用 Autofac，請參閱 [Autofac 文件 ](https://docs.autofac.org/en/latest/integration/aspnetcore.html) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="0e681-318">To learn more about using Autofac with ASP.NET Core, see the [Autofac documentation](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span></span>

### <a name="thread-safety"></a><span data-ttu-id="0e681-319">執行緒安全</span><span class="sxs-lookup"><span data-stu-id="0e681-319">Thread safety</span></span>

<span data-ttu-id="0e681-320">單一服務需要具備執行緒安全。</span><span class="sxs-lookup"><span data-stu-id="0e681-320">Singleton services need to be thread safe.</span></span> <span data-ttu-id="0e681-321">如果單一服務相依於暫時性服務，暫時性服務可能也需要具備執行緒安全，視它如何由單一項目使用。</span><span class="sxs-lookup"><span data-stu-id="0e681-321">If a singleton service has a dependency on a transient service, the transient service may also need to be thread safe depending how it's used by the singleton.</span></span>

<span data-ttu-id="0e681-322">單一服務的 Factory 方法 (例如 [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider,TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__) 的第二個引數) 不需要是安全執行緒。</span><span class="sxs-lookup"><span data-stu-id="0e681-322">The factory method of single service, such as the second argument to [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider,TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__), doesn't need to be thread-safe.</span></span> <span data-ttu-id="0e681-323">就像型別 (`static`) 建構函式一樣，它一定會被單一執行緒呼叫一次。</span><span class="sxs-lookup"><span data-stu-id="0e681-323">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="0e681-324">建議</span><span class="sxs-lookup"><span data-stu-id="0e681-324">Recommendations</span></span>

<span data-ttu-id="0e681-325">使用相依性插入時，請記住下列建議：</span><span class="sxs-lookup"><span data-stu-id="0e681-325">When working with dependency injection, keep the following recommendations in mind:</span></span>

* <span data-ttu-id="0e681-326">避免直接在服務容器中儲存資料與設定。</span><span class="sxs-lookup"><span data-stu-id="0e681-326">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="0e681-327">例如，使用者的購物車通常不應該新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="0e681-327">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="0e681-328">組態應該使用[選項模式](xref:fundamentals/configuration/options)。</span><span class="sxs-lookup"><span data-stu-id="0e681-328">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="0e681-329">同樣地，請避免只存在以允許存取某個其他物件的「資料持有者」物件。</span><span class="sxs-lookup"><span data-stu-id="0e681-329">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="0e681-330">最好是盡可能透過相依性插入來要求所需的實際項目。</span><span class="sxs-lookup"><span data-stu-id="0e681-330">It's better to request the actual item via dependency injection, if possible.</span></span>

* <span data-ttu-id="0e681-331">避免以靜態方式存取服務 (例如，以靜態方式設定 [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) 型別以四處使用)。</span><span class="sxs-lookup"><span data-stu-id="0e681-331">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) for use elsewhere).</span></span>

* <span data-ttu-id="0e681-332">避免使用服務定位器模式 (例如 [IServiceProvider.GetService](/dotnet/api/system.iserviceprovider.getservice))。</span><span class="sxs-lookup"><span data-stu-id="0e681-332">Avoid using the service locator pattern (for example, [IServiceProvider.GetService](/dotnet/api/system.iserviceprovider.getservice)).</span></span>

* <span data-ttu-id="0e681-333">避免以靜態方式存取 `HttpContext` (例如 [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext))。</span><span class="sxs-lookup"><span data-stu-id="0e681-333">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext)).</span></span>

<span data-ttu-id="0e681-334">就像所有的建議集，您可能會遇到需要忽略建議的情況。</span><span class="sxs-lookup"><span data-stu-id="0e681-334">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="0e681-335">例外狀況很少見&mdash;大部分是架構本身內的特殊案例。</span><span class="sxs-lookup"><span data-stu-id="0e681-335">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="0e681-336">相依性插入是靜態/全域物件存取模式的「替代」選項。</span><span class="sxs-lookup"><span data-stu-id="0e681-336">Dependency injection is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="0e681-337">如果您將相依性插入與靜態物件存取混合，則可能無法實現相依性插入的優點。</span><span class="sxs-lookup"><span data-stu-id="0e681-337">You may not be able to realize the benefits of dependency injection if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0e681-338">其他資源</span><span class="sxs-lookup"><span data-stu-id="0e681-338">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:fundamentals/repository-pattern>
* <xref:fundamentals/startup>
* <xref:test/index>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="0e681-339">在 ASP.NET Core 使用 Dependency Injection 撰寫簡潔的程式碼 (MSDN)</span><span class="sxs-lookup"><span data-stu-id="0e681-339">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* <span data-ttu-id="0e681-340">[Container-Managed Application Design, Prelude: Where does the Container Belong?](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/) (容器管理的應用程式設計，序曲：容器屬於何處？)</span><span class="sxs-lookup"><span data-stu-id="0e681-340">[Container-Managed Application Design, Prelude: Where does the Container Belong?](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)</span></span>
* [<span data-ttu-id="0e681-341">明確相依性準則</span><span class="sxs-lookup"><span data-stu-id="0e681-341">Explicit Dependencies Principle</span></span>](https://deviq.com/explicit-dependencies-principle/)
* <span data-ttu-id="0e681-342">[逆轉控制容器和相依性插入模式 (Martin Fowler)](https://www.martinfowler.com/articles/injection.html) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="0e681-342">[Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)](https://www.martinfowler.com/articles/injection.html)</span></span>
* <span data-ttu-id="0e681-343">[New is Glue (「黏附」程式碼到特定實作)](https://ardalis.com/new-is-glue) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="0e681-343">[New is Glue ("gluing" code to a particular implementation)](https://ardalis.com/new-is-glue)</span></span>
