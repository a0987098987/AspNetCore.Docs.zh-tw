---
title: .NET Core 中的相依性插入
author: guardrex
description: 了解 ASP.NET Core 如何實作相依性插入以及如何使用它。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/09/2019
uid: fundamentals/dependency-injection
ms.openlocfilehash: 1455aa9ce4ea24eaeb396134f91b6d089b346c17
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724435"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="1d647-103">.NET Core 中的相依性插入</span><span class="sxs-lookup"><span data-stu-id="1d647-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="1d647-104">作者：[Steve Smith](https://ardalis.com/)、[Scott Addie](https://scottaddie.com) 與 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1d647-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1d647-105">ASP.NET Core 支援相依性插入 (DI) 軟體設計模式，這是用來在類別與其相依性之間達成[控制權反轉 (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) \(英文\) 的技術。</span><span class="sxs-lookup"><span data-stu-id="1d647-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="1d647-106">如需有關 MVC 控制器內相依性插入的特定詳細資訊，請參閱 <xref:mvc/controllers/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="1d647-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="1d647-107">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1d647-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="1d647-108">相依性插入概觀</span><span class="sxs-lookup"><span data-stu-id="1d647-108">Overview of dependency injection</span></span>

<span data-ttu-id="1d647-109">「相依性」  是另一個物件所需的任何物件。</span><span class="sxs-lookup"><span data-stu-id="1d647-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="1d647-110">檢查具有 `WriteMessage` 方法的下列 `MyDependency` 物件，應用程式中的其他類別相依於此物件：</span><span class="sxs-lookup"><span data-stu-id="1d647-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

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

<span data-ttu-id="1d647-111">您可以建立 `MyDependency` 類別的執行個體，讓 `WriteMessage` 方法可供類別使用。</span><span class="sxs-lookup"><span data-stu-id="1d647-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="1d647-112">`MyDependency` 類別是 `IndexModel` 類別的相依性：</span><span class="sxs-lookup"><span data-stu-id="1d647-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

```csharp
public class IndexModel : PageModel
{
    MyDependency _dependency = new MyDependency();

    public async Task OnGetAsync()
    {
        await _dependency.WriteMessage(
            "IndexModel.OnGetAsync created this message.");
    }
}
```

<span data-ttu-id="1d647-113">該類別會建立 `MyDependency` 執行個體並直接相依於該執行個體。</span><span class="sxs-lookup"><span data-stu-id="1d647-113">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="1d647-114">程式碼相依性 (例如前一個範例) 有問題，因此基於下列原因應該避免使用：</span><span class="sxs-lookup"><span data-stu-id="1d647-114">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="1d647-115">若要將 `MyDependency` 取代為不同的實作，必須修改該類別。</span><span class="sxs-lookup"><span data-stu-id="1d647-115">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="1d647-116">若 `MyDependency` 有相依性，那些相依性必須由該類別設定。</span><span class="sxs-lookup"><span data-stu-id="1d647-116">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="1d647-117">在具有多個相依於 `MyDependency` 之多個類別的大型專案中，設定程式碼在不同的應用程式之間會變得鬆散。</span><span class="sxs-lookup"><span data-stu-id="1d647-117">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="1d647-118">此實作難以進行單元測試。</span><span class="sxs-lookup"><span data-stu-id="1d647-118">This implementation is difficult to unit test.</span></span> <span data-ttu-id="1d647-119">應用程式應該使用模擬 (Mock) 或虛設常式 (Stub) `MyDependency` 類別，這在使用此方法時無法使用。</span><span class="sxs-lookup"><span data-stu-id="1d647-119">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="1d647-120">相依性插入可透過下列方式解決這些問題：</span><span class="sxs-lookup"><span data-stu-id="1d647-120">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="1d647-121">使用介面或基底類別來將相依性資訊抽象化。</span><span class="sxs-lookup"><span data-stu-id="1d647-121">The use of an interface or base class to abstract the dependency implementation.</span></span>
* <span data-ttu-id="1d647-122">在服務容器中註冊相依性。</span><span class="sxs-lookup"><span data-stu-id="1d647-122">Registration of the dependency in a service container.</span></span> <span data-ttu-id="1d647-123">ASP.NET Core 提供內建服務容器 <xref:System.IServiceProvider>。</span><span class="sxs-lookup"><span data-stu-id="1d647-123">ASP.NET Core provides a built-in service container, <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="1d647-124">服務會在應用程式的 `Startup.ConfigureServices` 方法中註冊。</span><span class="sxs-lookup"><span data-stu-id="1d647-124">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="1d647-125">將服務「插入」  到服務使用位置之類別的建構函式。</span><span class="sxs-lookup"><span data-stu-id="1d647-125">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="1d647-126">架構會負責建立相依性的執行個體，並在不再需要時將它捨棄。</span><span class="sxs-lookup"><span data-stu-id="1d647-126">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="1d647-127">在[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples)中，`IMyDependency` 介面定義了服務提供給應用程式的方法：</span><span class="sxs-lookup"><span data-stu-id="1d647-127">In the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

<span data-ttu-id="1d647-128">這個介面是由具象型別 `MyDependency` 所實作：</span><span class="sxs-lookup"><span data-stu-id="1d647-128">This interface is implemented by a concrete type, `MyDependency`:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

<span data-ttu-id="1d647-129">`MyDependency` 在其建構函式中會要求 <xref:Microsoft.Extensions.Logging.ILogger`1>。</span><span class="sxs-lookup"><span data-stu-id="1d647-129">`MyDependency` requests an <xref:Microsoft.Extensions.Logging.ILogger`1> in its constructor.</span></span> <span data-ttu-id="1d647-130">以鏈結方式使用相依性插入並非不尋常。</span><span class="sxs-lookup"><span data-stu-id="1d647-130">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="1d647-131">每個要求的相依性接著會要求其自己的相依性。</span><span class="sxs-lookup"><span data-stu-id="1d647-131">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="1d647-132">容器會解決圖形中的相依性，並傳回完全解析的服務。</span><span class="sxs-lookup"><span data-stu-id="1d647-132">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="1d647-133">必須先解析的相依性集合組通常稱為「相依性樹狀結構」  、「相依性圖形」  或「物件圖形」  。</span><span class="sxs-lookup"><span data-stu-id="1d647-133">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="1d647-134">`IMyDependency` 與 `ILogger<TCategoryName>` 必須在服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="1d647-134">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="1d647-135">`IMyDependency` 是在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="1d647-135">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1d647-136">`ILogger<TCategoryName>` 是由記錄抽象基礎結構所註冊，所以它是預設由架構所註冊的[架構提供的服務](#framework-provided-services)。</span><span class="sxs-lookup"><span data-stu-id="1d647-136">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="1d647-137">容器會利用 [(泛型) 開放類型](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types) 解析 `ILogger<TCategoryName>`，讓您不需註冊每個 [(泛型) 建構的型別](/dotnet/csharp/language-reference/language-specification/types#constructed-types)：</span><span class="sxs-lookup"><span data-stu-id="1d647-137">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<T>), typeof(Logger<T>));
```

<span data-ttu-id="1d647-138">在範例應用程式中，`IMyDependency` 服務是使用具象型別 `MyDependency` 所註冊。</span><span class="sxs-lookup"><span data-stu-id="1d647-138">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="1d647-139">註冊會將服務的存留期範圍限制為單一要求的存留期。</span><span class="sxs-lookup"><span data-stu-id="1d647-139">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="1d647-140">將在此主題稍後將說明[服務存留期](#service-lifetimes)。</span><span class="sxs-lookup"><span data-stu-id="1d647-140">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> <span data-ttu-id="1d647-141">每個 `services.Add{SERVICE_NAME}` 擴充方法都會新增 (並且可能會設定) 服務。</span><span class="sxs-lookup"><span data-stu-id="1d647-141">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="1d647-142">例如，`services.AddMvc()` 會新增 Razor Pages 與 MVC 要求的服務。</span><span class="sxs-lookup"><span data-stu-id="1d647-142">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="1d647-143">建議應用程式遵循此慣例。</span><span class="sxs-lookup"><span data-stu-id="1d647-143">We recommended that apps follow this convention.</span></span> <span data-ttu-id="1d647-144">在 <xref:Microsoft.Extensions.DependencyInjection?displayProperty=fullName> 命名空間中放置擴充方法，以封裝服務註冊群組。</span><span class="sxs-lookup"><span data-stu-id="1d647-144">Place extension methods in the <xref:Microsoft.Extensions.DependencyInjection?displayProperty=fullName> namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="1d647-145">如果服務的建構函式需要[內建類型](/dotnet/csharp/language-reference/keywords/built-in-types-table)，例如 `string`，可以使用[組態](xref:fundamentals/configuration/index)或[選項模式](xref:fundamentals/configuration/options)插入該類型：</span><span class="sxs-lookup"><span data-stu-id="1d647-145">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

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

<span data-ttu-id="1d647-146">服務執行個體是透過服務使用所在之類別建構函式所要求，而且會指派給私人欄位。</span><span class="sxs-lookup"><span data-stu-id="1d647-146">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="1d647-147">該欄位會視需要用來存取整個類別中的服務。</span><span class="sxs-lookup"><span data-stu-id="1d647-147">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="1d647-148">在範例應用程式中，會要求 `IMyDependency` 執行個體並使用它來呼叫服務的 `WriteMessage` 方法：</span><span class="sxs-lookup"><span data-stu-id="1d647-148">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="framework-provided-services"></a><span data-ttu-id="1d647-149">架構提供的服務</span><span class="sxs-lookup"><span data-stu-id="1d647-149">Framework-provided services</span></span>

<span data-ttu-id="1d647-150">`Startup.ConfigureServices` 方法負責定義應用程式將使用的服務，包括像是 Entity Framework Core 與 ASP.NET Core MVC 等平台功能。</span><span class="sxs-lookup"><span data-stu-id="1d647-150">The `Startup.ConfigureServices` method is responsible for defining the services the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="1d647-151">一開始，提供給 `ConfigureServices` 的 `IServiceCollection` 具有下列已定義的服務 (取決於[主機的設定方式](xref:fundamentals/index#host))：</span><span class="sxs-lookup"><span data-stu-id="1d647-151">Initially, the `IServiceCollection` provided to `ConfigureServices` has the following services defined (depending on [how the host was configured](xref:fundamentals/index#host)):</span></span>

| <span data-ttu-id="1d647-152">服務類型</span><span class="sxs-lookup"><span data-stu-id="1d647-152">Service Type</span></span> | <span data-ttu-id="1d647-153">存留期</span><span class="sxs-lookup"><span data-stu-id="1d647-153">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="1d647-154">暫時性</span><span class="sxs-lookup"><span data-stu-id="1d647-154">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=fullName> | <span data-ttu-id="1d647-155">單一</span><span class="sxs-lookup"><span data-stu-id="1d647-155">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=fullName> | <span data-ttu-id="1d647-156">單一</span><span class="sxs-lookup"><span data-stu-id="1d647-156">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="1d647-157">單一</span><span class="sxs-lookup"><span data-stu-id="1d647-157">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="1d647-158">暫時性</span><span class="sxs-lookup"><span data-stu-id="1d647-158">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="1d647-159">單一</span><span class="sxs-lookup"><span data-stu-id="1d647-159">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="1d647-160">暫時性</span><span class="sxs-lookup"><span data-stu-id="1d647-160">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="1d647-161">單一</span><span class="sxs-lookup"><span data-stu-id="1d647-161">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="1d647-162">單一</span><span class="sxs-lookup"><span data-stu-id="1d647-162">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="1d647-163">單一</span><span class="sxs-lookup"><span data-stu-id="1d647-163">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="1d647-164">暫時性</span><span class="sxs-lookup"><span data-stu-id="1d647-164">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="1d647-165">單一</span><span class="sxs-lookup"><span data-stu-id="1d647-165">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="1d647-166">單一</span><span class="sxs-lookup"><span data-stu-id="1d647-166">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="1d647-167">單一</span><span class="sxs-lookup"><span data-stu-id="1d647-167">Singleton</span></span> |

<span data-ttu-id="1d647-168">當可以使用服務集合擴充方法來註冊服務 (如果需要，也可以註冊其相依服務) 時，慣例是使用單一 `Add{SERVICE_NAME}` 擴充方法來註冊該服務要求的所有服務。</span><span class="sxs-lookup"><span data-stu-id="1d647-168">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="1d647-169">下列程式碼範例示範如何使用擴充方法 <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext*>、<xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*> 和 <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> 將額外服務新增至容器：</span><span class="sxs-lookup"><span data-stu-id="1d647-169">The following code is an example of how to add additional services to the container using the extension methods <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext*>, <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>, and <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>:</span></span>

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

<span data-ttu-id="1d647-170">如需詳細資訊，請參閱 API 文件中的 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> 類別。</span><span class="sxs-lookup"><span data-stu-id="1d647-170">For more information, see the <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> clss in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="1d647-171">執行個體存留期</span><span class="sxs-lookup"><span data-stu-id="1d647-171">Service lifetimes</span></span>

<span data-ttu-id="1d647-172">為每個已註冊的服務選擇適當的存留期。</span><span class="sxs-lookup"><span data-stu-id="1d647-172">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="1d647-173">ASP.NET Core 服務可以使用下列存留期進行設定：</span><span class="sxs-lookup"><span data-stu-id="1d647-173">ASP.NET Core services can be configured with the following lifetimes:</span></span>

### <a name="transient"></a><span data-ttu-id="1d647-174">暫時性</span><span class="sxs-lookup"><span data-stu-id="1d647-174">Transient</span></span>

<span data-ttu-id="1d647-175">每次從服務容器要求暫時性存留期服務 (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) 時都會建立它們。</span><span class="sxs-lookup"><span data-stu-id="1d647-175">Transient lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) are created each time they're requested from the service container.</span></span> <span data-ttu-id="1d647-176">此存留期最適合用於輕量型的無狀態服務。</span><span class="sxs-lookup"><span data-stu-id="1d647-176">This lifetime works best for lightweight, stateless services.</span></span>

### <a name="scoped"></a><span data-ttu-id="1d647-177">具範圍</span><span class="sxs-lookup"><span data-stu-id="1d647-177">Scoped</span></span>

<span data-ttu-id="1d647-178">具範圍存留期服務 (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) 會在每次用戶端要求 (連線) 時建立一次。</span><span class="sxs-lookup"><span data-stu-id="1d647-178">Scoped lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) are created once per client request (connection).</span></span>

> [!WARNING]
> <span data-ttu-id="1d647-179">在中介軟體中使用具範圍服務時，請將該服務插入 `Invoke` 或 `InvokeAsync` 方法中。</span><span class="sxs-lookup"><span data-stu-id="1d647-179">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="1d647-180">因為其會強制服務執行單一服務的行為，所以請勿透過建構函式插入進行插入。</span><span class="sxs-lookup"><span data-stu-id="1d647-180">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="1d647-181">如需詳細資訊，請參閱 <xref:fundamentals/middleware/index>。</span><span class="sxs-lookup"><span data-stu-id="1d647-181">For more information, see <xref:fundamentals/middleware/index>.</span></span>

### <a name="singleton"></a><span data-ttu-id="1d647-182">單一</span><span class="sxs-lookup"><span data-stu-id="1d647-182">Singleton</span></span>

<span data-ttu-id="1d647-183">當第一次收到有關單一資料庫存留期服務的要求時 (或是當執行 `Startup.ConfigureServices` 且隨著服務註冊指定執行個體時)，即會建立單一資料庫存留期服務 (<xref:Microsoft.AspNet.OData.Builder.ODataModelBuilder.AddSingleton*>)。</span><span class="sxs-lookup"><span data-stu-id="1d647-183">Singleton lifetime services (<xref:Microsoft.AspNet.OData.Builder.ODataModelBuilder.AddSingleton*>) are created the first time they're requested (or when `Startup.ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="1d647-184">每個後續要求都會使用相同的執行個體。</span><span class="sxs-lookup"><span data-stu-id="1d647-184">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="1d647-185">若應用程式要求單一資料庫行為，建議您允許服務容器管理服務的存留期。</span><span class="sxs-lookup"><span data-stu-id="1d647-185">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="1d647-186">不要實作單一資料庫設計模式並為使用者提供可用來管理類別中物件存留期的程式碼。</span><span class="sxs-lookup"><span data-stu-id="1d647-186">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="1d647-187">從單一資料庫解析具範圍服務是非常危險的。</span><span class="sxs-lookup"><span data-stu-id="1d647-187">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="1d647-188">處理後續要求時，它可能會導致服務有不正確的狀態。</span><span class="sxs-lookup"><span data-stu-id="1d647-188">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

## <a name="service-registration-methods"></a><span data-ttu-id="1d647-189">服務註冊方法</span><span class="sxs-lookup"><span data-stu-id="1d647-189">Service registration methods</span></span>

<span data-ttu-id="1d647-190">每個服務註冊擴充方法都提供在特定案例中相當有用的多載。</span><span class="sxs-lookup"><span data-stu-id="1d647-190">Each service registration extension method offers overloads that are useful in specific scenarios.</span></span>

| <span data-ttu-id="1d647-191">方法</span><span class="sxs-lookup"><span data-stu-id="1d647-191">Method</span></span> | <span data-ttu-id="1d647-192">自動</span><span class="sxs-lookup"><span data-stu-id="1d647-192">Automatic</span></span><br><span data-ttu-id="1d647-193">object</span><span class="sxs-lookup"><span data-stu-id="1d647-193">object</span></span><br><span data-ttu-id="1d647-194">處置</span><span class="sxs-lookup"><span data-stu-id="1d647-194">disposal</span></span> | <span data-ttu-id="1d647-195">多個</span><span class="sxs-lookup"><span data-stu-id="1d647-195">Multiple</span></span><br><span data-ttu-id="1d647-196">實作</span><span class="sxs-lookup"><span data-stu-id="1d647-196">implementations</span></span> | <span data-ttu-id="1d647-197">傳遞引數</span><span class="sxs-lookup"><span data-stu-id="1d647-197">Pass args</span></span> |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br><span data-ttu-id="1d647-198">範例：</span><span class="sxs-lookup"><span data-stu-id="1d647-198">Example:</span></span><br>`services.AddScoped<IMyDep, MyDep>();` | <span data-ttu-id="1d647-199">是</span><span class="sxs-lookup"><span data-stu-id="1d647-199">Yes</span></span> | <span data-ttu-id="1d647-200">是</span><span class="sxs-lookup"><span data-stu-id="1d647-200">Yes</span></span> | <span data-ttu-id="1d647-201">否</span><span class="sxs-lookup"><span data-stu-id="1d647-201">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br><span data-ttu-id="1d647-202">例如：</span><span class="sxs-lookup"><span data-stu-id="1d647-202">Examples:</span></span><br>`services.AddScoped<IMyDep>(sp => new MyDep());`<br>`services.AddScoped<IMyDep>(sp => new MyDep("A string!"));` | <span data-ttu-id="1d647-203">是</span><span class="sxs-lookup"><span data-stu-id="1d647-203">Yes</span></span> | <span data-ttu-id="1d647-204">[是]</span><span class="sxs-lookup"><span data-stu-id="1d647-204">Yes</span></span> | <span data-ttu-id="1d647-205">是</span><span class="sxs-lookup"><span data-stu-id="1d647-205">Yes</span></span> |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br><span data-ttu-id="1d647-206">範例：</span><span class="sxs-lookup"><span data-stu-id="1d647-206">Example:</span></span><br>`services.AddScoped<MyDep>();` | <span data-ttu-id="1d647-207">是</span><span class="sxs-lookup"><span data-stu-id="1d647-207">Yes</span></span> | <span data-ttu-id="1d647-208">否</span><span class="sxs-lookup"><span data-stu-id="1d647-208">No</span></span> | <span data-ttu-id="1d647-209">否</span><span class="sxs-lookup"><span data-stu-id="1d647-209">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(new {IMPLEMENTATION})`<br><span data-ttu-id="1d647-210">例如：</span><span class="sxs-lookup"><span data-stu-id="1d647-210">Examples:</span></span><br>`services.AddScoped<IMyDep>(new MyDep());`<br>`services.AddScoped<IMyDep>(new MyDep("A string!"));` | <span data-ttu-id="1d647-211">否</span><span class="sxs-lookup"><span data-stu-id="1d647-211">No</span></span> | <span data-ttu-id="1d647-212">yes</span><span class="sxs-lookup"><span data-stu-id="1d647-212">Yes</span></span> | <span data-ttu-id="1d647-213">是</span><span class="sxs-lookup"><span data-stu-id="1d647-213">Yes</span></span> |
| `Add{LIFETIME}(new {IMPLEMENTATION})`<br><span data-ttu-id="1d647-214">例如：</span><span class="sxs-lookup"><span data-stu-id="1d647-214">Examples:</span></span><br>`services.AddScoped(new MyDep());`<br>`services.AddScoped(new MyDep("A string!"));` | <span data-ttu-id="1d647-215">否</span><span class="sxs-lookup"><span data-stu-id="1d647-215">No</span></span> | <span data-ttu-id="1d647-216">否</span><span class="sxs-lookup"><span data-stu-id="1d647-216">No</span></span> | <span data-ttu-id="1d647-217">是</span><span class="sxs-lookup"><span data-stu-id="1d647-217">Yes</span></span> |

<span data-ttu-id="1d647-218">如需類型處置的詳細資訊，請參閱[＜服務處置＞](#disposal-of-services)一節。</span><span class="sxs-lookup"><span data-stu-id="1d647-218">For more information on type disposal, see the [Disposal of services](#disposal-of-services) section.</span></span> <span data-ttu-id="1d647-219">多個實作的常見案例是[模擬測試類型](xref:test/integration-tests#inject-mock-services)。</span><span class="sxs-lookup"><span data-stu-id="1d647-219">A common scenario for multiple implementations is [mocking types for testing](xref:test/integration-tests#inject-mock-services).</span></span>

<span data-ttu-id="1d647-220">如果還未註冊任何實作，則 `TryAdd{LIFETIME}` 方法只會註冊服務。</span><span class="sxs-lookup"><span data-stu-id="1d647-220">`TryAdd{LIFETIME}` methods only register the service if there isn't already an implementation registered.</span></span>

<span data-ttu-id="1d647-221">在下列範例中，第一行會為 `IMyDependency` 註冊 `MyDependency`。</span><span class="sxs-lookup"><span data-stu-id="1d647-221">In the following example, the first line registers `MyDependency` for `IMyDependency`.</span></span> <span data-ttu-id="1d647-222">第二行則沒有任何作用，因為 `IMyDependency` 已具有註冊的實作：</span><span class="sxs-lookup"><span data-stu-id="1d647-222">The second line has no effect because `IMyDependency` already has a registered implementation:</span></span>

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

<span data-ttu-id="1d647-223">如需詳細資訊，請參閱:</span><span class="sxs-lookup"><span data-stu-id="1d647-223">For more information, see:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

<span data-ttu-id="1d647-224">如果還沒有「相同類型的」  實作，則 [TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) 方法只會註冊服務。</span><span class="sxs-lookup"><span data-stu-id="1d647-224">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) methods only register the service if there isn't already an implementation *of the same type*.</span></span> <span data-ttu-id="1d647-225">多個服務會透過 `IEnumerable<{SERVICE}>` 解析。</span><span class="sxs-lookup"><span data-stu-id="1d647-225">Multiple services are resolved via `IEnumerable<{SERVICE}>`.</span></span> <span data-ttu-id="1d647-226">註冊服務時，如果尚未新增相同類型的執行個體，則開發人員只會希望新增執行個體。</span><span class="sxs-lookup"><span data-stu-id="1d647-226">When registering services, the developer only wants to add an instance if one of the same type hasn't already been added.</span></span> <span data-ttu-id="1d647-227">程式庫作者通常會使用此方法來避免註冊容器中某個執行個體的兩個複本。</span><span class="sxs-lookup"><span data-stu-id="1d647-227">Generally, this method is used by library authors to avoid registering two copies of an instance in the container.</span></span>

<span data-ttu-id="1d647-228">在下列範例中，第一行會為 `IMyDep1` 註冊 `MyDep`。</span><span class="sxs-lookup"><span data-stu-id="1d647-228">In the following example, the first line registers `MyDep` for `IMyDep1`.</span></span> <span data-ttu-id="1d647-229">第二行會為 `IMyDep2` 註冊 `MyDep`。</span><span class="sxs-lookup"><span data-stu-id="1d647-229">The second line registers `MyDep` for `IMyDep2`.</span></span> <span data-ttu-id="1d647-230">第三行則沒有任何作用，因為 `IMyDep1` 已具有 `MyDep` 的已註冊實作：</span><span class="sxs-lookup"><span data-stu-id="1d647-230">The third line has no effect because `IMyDep1` already has a registered implementation of `MyDep`:</span></span>

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a><span data-ttu-id="1d647-231">建構函式插入行為</span><span class="sxs-lookup"><span data-stu-id="1d647-231">Constructor injection behavior</span></span>

<span data-ttu-id="1d647-232">服務可以透過兩個機制來解析：</span><span class="sxs-lookup"><span data-stu-id="1d647-232">Services can be resolved by two mechanisms:</span></span>

* <xref:System.IServiceProvider>
* <span data-ttu-id="1d647-233"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; 允許建立物件，而不需要在相依性插入容器中註冊服務。</span><span class="sxs-lookup"><span data-stu-id="1d647-233"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="1d647-234">搭配使用者面向抽象 (例如標籤協助程式、MVC 控制器與模型繫結器) 使用 `ActivatorUtilities`。</span><span class="sxs-lookup"><span data-stu-id="1d647-234">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="1d647-235">建構函式可以接受不是由相依性插入提供的引數，但引數必須指派預設值。</span><span class="sxs-lookup"><span data-stu-id="1d647-235">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="1d647-236">當服務由 `IServiceProvider` 或 `ActivatorUtilities` 解析時，建構函式插入會要求 *public*建構函式。</span><span class="sxs-lookup"><span data-stu-id="1d647-236">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="1d647-237">當服務由 `ActivatorUtilities` 解析時，建構函式插入只要求只能有一個適用的建構函式存在。</span><span class="sxs-lookup"><span data-stu-id="1d647-237">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exists.</span></span> <span data-ttu-id="1d647-238">支援建構函式多載，但只能有一個多載存在，其引數可以藉由相依性插入而完成。</span><span class="sxs-lookup"><span data-stu-id="1d647-238">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="1d647-239">Entity Framework 內容</span><span class="sxs-lookup"><span data-stu-id="1d647-239">Entity Framework contexts</span></span>

<span data-ttu-id="1d647-240">因為一般會將 Web 應用程式資料庫作業範圍設定為用戶端要求，所以通常會使用[具範圍存留期](#service-lifetimes)將 Entity Framework 內容新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="1d647-240">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="1d647-241">註冊資料庫內容時，如果 <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext*> 多載未指定留存期，則會將範圍設定為預設存留期。</span><span class="sxs-lookup"><span data-stu-id="1d647-241">The default lifetime is scoped if a lifetime isn't specified by an <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext*> overload when registering the database context.</span></span> <span data-ttu-id="1d647-242">指定存留期的服務不應該使用存留期比服務還短的資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="1d647-242">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="1d647-243">留期和註冊選項</span><span class="sxs-lookup"><span data-stu-id="1d647-243">Lifetime and registration options</span></span>

<span data-ttu-id="1d647-244">為了示範這些存留期和註冊選項之間的差異，請考慮下列介面，以具有唯一識別碼 `OperationId` 的「作業」代表工作。</span><span class="sxs-lookup"><span data-stu-id="1d647-244">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="1d647-245">視如何針對下列介面設定作業服務存留期而定，當類別要求時，容器會提供相同或不同的服務執行個體：</span><span class="sxs-lookup"><span data-stu-id="1d647-245">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

<span data-ttu-id="1d647-246">介面是在 `Operation` 類別中實作。</span><span class="sxs-lookup"><span data-stu-id="1d647-246">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="1d647-247">若未提供 GUID，`Operation` 建構函式會產生 GUID：</span><span class="sxs-lookup"><span data-stu-id="1d647-247">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

<span data-ttu-id="1d647-248">會註冊相依於每種其他 `Operation` 類型的 `OperationService`。</span><span class="sxs-lookup"><span data-stu-id="1d647-248">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="1d647-249">當透過相依性插入要求 `OperationService` 時，它會根據相依服務的存留期擷取每個服務的新執行個體或現有執行個體。</span><span class="sxs-lookup"><span data-stu-id="1d647-249">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="1d647-250">當因收到來自容器的要求而建立暫時性服務時，`IOperationTransient` 服務的 `OperationId` 會與 `OperationService` 的 `OperationId` 不同。</span><span class="sxs-lookup"><span data-stu-id="1d647-250">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="1d647-251">`OperationService` 會收到 `IOperationTransient` 類別的新執行個體。</span><span class="sxs-lookup"><span data-stu-id="1d647-251">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="1d647-252">新執行個體會產生不同的 `OperationId`。</span><span class="sxs-lookup"><span data-stu-id="1d647-252">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="1d647-253">當因用戶端要求而建立具範圍的服務時，`IOperationScoped` 服務與用戶端要求中 `OperationService` 的 `OperationId` 皆相同。</span><span class="sxs-lookup"><span data-stu-id="1d647-253">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="1d647-254">在不同的用戶端要求中，兩個服務的 `OperationId` 值都不同。</span><span class="sxs-lookup"><span data-stu-id="1d647-254">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="1d647-255">當建立一次 singleton 與 singleton 執行個體服務，並在所有用戶端要求與所有服務中使用時，`OperationId` 在所有服務要求中會固定不變。</span><span class="sxs-lookup"><span data-stu-id="1d647-255">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

<span data-ttu-id="1d647-256">在 `Startup.ConfigureServices` 中，每個類型都會根據其具名存留期新增至容器：</span><span class="sxs-lookup"><span data-stu-id="1d647-256">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

<span data-ttu-id="1d647-257">`IOperationSingletonInstance` 服務使用具有 `Guid.Empty` 已知識別碼的特定執行個體。</span><span class="sxs-lookup"><span data-stu-id="1d647-257">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="1d647-258">很明顯此型別是使用中 (其 GUID 是所有區域)。</span><span class="sxs-lookup"><span data-stu-id="1d647-258">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="1d647-259">範例應用程式示範個別要求內與個別要求之間的物件存留期。</span><span class="sxs-lookup"><span data-stu-id="1d647-259">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="1d647-260">範例應用程式的 `IndexModel` 會要求每種 `IOperation` 型別與 `OperationService`。</span><span class="sxs-lookup"><span data-stu-id="1d647-260">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="1d647-261">頁面接著會透過屬性指派來顯示所有頁面模型類別與服務的 `OperationId` 值：</span><span class="sxs-lookup"><span data-stu-id="1d647-261">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

<span data-ttu-id="1d647-262">下面兩個輸出顯示兩個要求的結果：</span><span class="sxs-lookup"><span data-stu-id="1d647-262">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="1d647-263">**第一個要求：**</span><span class="sxs-lookup"><span data-stu-id="1d647-263">**First request:**</span></span>

<span data-ttu-id="1d647-264">控制器作業：</span><span class="sxs-lookup"><span data-stu-id="1d647-264">Controller operations:</span></span>

<span data-ttu-id="1d647-265">暫時性： d233e165-f417-469b-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="1d647-265">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="1d647-266">具範圍：5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="1d647-266">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="1d647-267">單一資料庫：01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="1d647-267">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="1d647-268">執行個體：00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="1d647-268">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="1d647-269">`OperationService` 作業：</span><span class="sxs-lookup"><span data-stu-id="1d647-269">`OperationService` operations:</span></span>

<span data-ttu-id="1d647-270">暫時性： c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="1d647-270">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="1d647-271">具範圍：5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="1d647-271">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="1d647-272">單一資料庫：01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="1d647-272">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="1d647-273">執行個體：00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="1d647-273">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="1d647-274">**:第二個要求：**</span><span class="sxs-lookup"><span data-stu-id="1d647-274">**Second request:**</span></span>

<span data-ttu-id="1d647-275">控制器作業：</span><span class="sxs-lookup"><span data-stu-id="1d647-275">Controller operations:</span></span>

<span data-ttu-id="1d647-276">暫時性： b63bd538-0a37-4ff1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="1d647-276">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="1d647-277">具範圍：31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="1d647-277">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="1d647-278">單一資料庫：01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="1d647-278">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="1d647-279">執行個體：00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="1d647-279">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="1d647-280">`OperationService` 作業：</span><span class="sxs-lookup"><span data-stu-id="1d647-280">`OperationService` operations:</span></span>

<span data-ttu-id="1d647-281">暫時性： c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="1d647-281">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="1d647-282">具範圍：31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="1d647-282">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="1d647-283">單一資料庫：01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="1d647-283">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="1d647-284">執行個體：00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="1d647-284">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="1d647-285">觀察哪些 `OperationId` 值在要求內以及要求之間不同：</span><span class="sxs-lookup"><span data-stu-id="1d647-285">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="1d647-286">「暫時性」  物件一律不同。</span><span class="sxs-lookup"><span data-stu-id="1d647-286">*Transient* objects are always different.</span></span> <span data-ttu-id="1d647-287">第一個與第二個用戶端要求的暫時性 `OperationId` 值在兩個 `OperationService` 作業之間與各用戶端要求之間都不同。</span><span class="sxs-lookup"><span data-stu-id="1d647-287">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="1d647-288">新執行個體會提供給每個服務要求以及用戶端要求。</span><span class="sxs-lookup"><span data-stu-id="1d647-288">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="1d647-289">「具範圍」  物件在同一個用戶端要求內皆相同，但在不同的用戶端要求之間則不同。</span><span class="sxs-lookup"><span data-stu-id="1d647-289">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="1d647-290">「單一資料庫」  物件對於每個物件與每個要求都相同 (不論 `Operation` 執行個體是否提供於 `Startup.ConfigureServices`)。</span><span class="sxs-lookup"><span data-stu-id="1d647-290">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `Startup.ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="1d647-291">從主要呼叫服務</span><span class="sxs-lookup"><span data-stu-id="1d647-291">Call services from main</span></span>

<span data-ttu-id="1d647-292">使用 [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) 建立 <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>，以解析應用程式範圍中的範圍服務。</span><span class="sxs-lookup"><span data-stu-id="1d647-292">Create an <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="1d647-293">此法可用於在開機時存取範圍服務，以執行初始化工作。</span><span class="sxs-lookup"><span data-stu-id="1d647-293">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="1d647-294">下列範例示範如何在 `Program.Main` 中取得 `MyScopedService`：</span><span class="sxs-lookup"><span data-stu-id="1d647-294">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="1d647-295">範圍驗證</span><span class="sxs-lookup"><span data-stu-id="1d647-295">Scope validation</span></span>

<span data-ttu-id="1d647-296">當應用程式在開發環境中執行時，預設服務提供者會執行檢查以確認：</span><span class="sxs-lookup"><span data-stu-id="1d647-296">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="1d647-297">範圍服務不是直接或間接由根服務提供者解析。</span><span class="sxs-lookup"><span data-stu-id="1d647-297">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="1d647-298">範圍服務不是直接或間接插入至單一服務。</span><span class="sxs-lookup"><span data-stu-id="1d647-298">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="1d647-299">根服務提供者會在呼叫 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> 時建立。</span><span class="sxs-lookup"><span data-stu-id="1d647-299">The root service provider is created when <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> is called.</span></span> <span data-ttu-id="1d647-300">當提供者啟動應用程式時，根服務提供者的存留期與應用程式/伺服器的存留期一致，並會在應用程式關閉時處置。</span><span class="sxs-lookup"><span data-stu-id="1d647-300">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="1d647-301">範圍服務會由建立這些服務的容器處置。</span><span class="sxs-lookup"><span data-stu-id="1d647-301">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="1d647-302">若是在根容器中建立範圍服務，因為當應用程式/伺服器關機時，服務只會由根容器處理，所以服務的存留期會提升為單一服務等級。</span><span class="sxs-lookup"><span data-stu-id="1d647-302">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="1d647-303">當呼叫 `BuildServiceProvider` 時，驗證服務範圍會攔截到這些情況。</span><span class="sxs-lookup"><span data-stu-id="1d647-303">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="1d647-304">如需詳細資訊，請參閱 <xref:fundamentals/host/web-host#scope-validation>。</span><span class="sxs-lookup"><span data-stu-id="1d647-304">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="1d647-305">要求服務</span><span class="sxs-lookup"><span data-stu-id="1d647-305">Request Services</span></span>

<span data-ttu-id="1d647-306">來自 `HttpContext`，在 ASP.NET Core 要求內提供的服務是透過 [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) 集合公開。</span><span class="sxs-lookup"><span data-stu-id="1d647-306">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) collection.</span></span>

<span data-ttu-id="1d647-307">要求服務代表您在應用程式中設定和要求的服務。</span><span class="sxs-lookup"><span data-stu-id="1d647-307">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="1d647-308">當物件指定相依性時，這些會由在 `RequestServices` 中找到的類型來滿足，而非 `ApplicationServices`。</span><span class="sxs-lookup"><span data-stu-id="1d647-308">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="1d647-309">一般而言，應用程式不應該直接使用這些屬性。</span><span class="sxs-lookup"><span data-stu-id="1d647-309">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="1d647-310">相反地，透過類別建構函式要求類別所需的型別，以及允許架構插入相依性。</span><span class="sxs-lookup"><span data-stu-id="1d647-310">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="1d647-311">這會產生容易測試的類別。</span><span class="sxs-lookup"><span data-stu-id="1d647-311">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="1d647-312">偏好要求相依性作為建構函式參數，而不要存取 `RequestServices` 集合。</span><span class="sxs-lookup"><span data-stu-id="1d647-312">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="1d647-313">針對相依性插入設計服務</span><span class="sxs-lookup"><span data-stu-id="1d647-313">Design services for dependency injection</span></span>

<span data-ttu-id="1d647-314">最佳做法是：</span><span class="sxs-lookup"><span data-stu-id="1d647-314">Best practices are to:</span></span>

* <span data-ttu-id="1d647-315">設計服務以使用相依性插入來取得其相依性。</span><span class="sxs-lookup"><span data-stu-id="1d647-315">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="1d647-316">避免具狀態的靜態方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="1d647-316">Avoid stateful, static method calls.</span></span>
* <span data-ttu-id="1d647-317">避免直接在服務內具現化相依類別。</span><span class="sxs-lookup"><span data-stu-id="1d647-317">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="1d647-318">直接具現化會將程式碼耦合到特定實作。</span><span class="sxs-lookup"><span data-stu-id="1d647-318">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="1d647-319">讓應用程式類別維持在小型、情況良好且可輕鬆測試的狀態。</span><span class="sxs-lookup"><span data-stu-id="1d647-319">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="1d647-320">若類別有太多插入的相依性，這通常表示類別有太多責任而且正違反[單一責任原則 (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="1d647-320">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="1d647-321">將類別負責的某些部分移到新的類別，以嘗試重構類別。</span><span class="sxs-lookup"><span data-stu-id="1d647-321">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="1d647-322">請記住，Razor Pages 頁面模型類別與 MVC 控制器類別應該專注於 UI 考量。</span><span class="sxs-lookup"><span data-stu-id="1d647-322">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="1d647-323">商務規則和資料存取實作詳細資料應該保存在適合這些[分開考量](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) (Separation of Concerns) 類別中。</span><span class="sxs-lookup"><span data-stu-id="1d647-323">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="1d647-324">處置服務</span><span class="sxs-lookup"><span data-stu-id="1d647-324">Disposal of services</span></span>

<span data-ttu-id="1d647-325">容器會為它建立的 <xref:System.IDisposable> 類型呼叫 <xref:System.IDisposable.Dispose*>。</span><span class="sxs-lookup"><span data-stu-id="1d647-325">The container calls <xref:System.IDisposable.Dispose*> for the <xref:System.IDisposable> types it creates.</span></span> <span data-ttu-id="1d647-326">若執行個體由使用者程式碼新增到容器中，它將不會自動處置。</span><span class="sxs-lookup"><span data-stu-id="1d647-326">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

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

## <a name="default-service-container-replacement"></a><span data-ttu-id="1d647-327">預設服務容器取代</span><span class="sxs-lookup"><span data-stu-id="1d647-327">Default service container replacement</span></span>

<span data-ttu-id="1d647-328">內建服務容器是要服務架構和大部分取用者應用程式的需求。</span><span class="sxs-lookup"><span data-stu-id="1d647-328">The built-in service container is meant to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="1d647-329">除非您需要內建容器不支援的特定功能，否則建議使用內建容器。</span><span class="sxs-lookup"><span data-stu-id="1d647-329">We recommend using the built-in container unless you need a specific feature that it doesn't support.</span></span> <span data-ttu-id="1d647-330">內建容器中找不到協力廠商容器支援的某些功能：</span><span class="sxs-lookup"><span data-stu-id="1d647-330">Some of the features supported in 3rd party containers not found in the built-in container:</span></span>

* <span data-ttu-id="1d647-331">屬性插入</span><span class="sxs-lookup"><span data-stu-id="1d647-331">Property injection</span></span>
* <span data-ttu-id="1d647-332">根據名稱插入</span><span class="sxs-lookup"><span data-stu-id="1d647-332">Injection based on name</span></span>
* <span data-ttu-id="1d647-333">子容器</span><span class="sxs-lookup"><span data-stu-id="1d647-333">Child containers</span></span>
* <span data-ttu-id="1d647-334">自訂生命週期管理</span><span class="sxs-lookup"><span data-stu-id="1d647-334">Custom lifetime management</span></span>
* <span data-ttu-id="1d647-335">`Func<T>` 支援延遲初始設定</span><span class="sxs-lookup"><span data-stu-id="1d647-335">`Func<T>` support for lazy initialization</span></span>

<span data-ttu-id="1d647-336">如需支援配接器的一些容器清單，請參閱 [DependencyInjection readme.md 檔案](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection)。</span><span class="sxs-lookup"><span data-stu-id="1d647-336">See the [Dependency Injection readme.md file](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection) for a list of some of the containers that support adapters.</span></span>

<span data-ttu-id="1d647-337">下列範例會以 [Autofac](https://autofac.org/) 取代內建容器：</span><span class="sxs-lookup"><span data-stu-id="1d647-337">The following sample replaces the built-in container with [Autofac](https://autofac.org/):</span></span>

* <span data-ttu-id="1d647-338">安裝適當的容器套件：</span><span class="sxs-lookup"><span data-stu-id="1d647-338">Install the appropriate container package(s):</span></span>

  * [<span data-ttu-id="1d647-339">Autofac</span><span class="sxs-lookup"><span data-stu-id="1d647-339">Autofac</span></span>](https://www.nuget.org/packages/Autofac/)
  * [<span data-ttu-id="1d647-340">Autofac.Extensions.DependencyInjection</span><span class="sxs-lookup"><span data-stu-id="1d647-340">Autofac.Extensions.DependencyInjection</span></span>](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/)

* <span data-ttu-id="1d647-341">在 `Startup.ConfigureServices` 中設定容器並傳回 `IServiceProvider`：</span><span class="sxs-lookup"><span data-stu-id="1d647-341">Configure the container in `Startup.ConfigureServices` and return an `IServiceProvider`:</span></span>

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

    <span data-ttu-id="1d647-342">若要使用第三方容器，`Startup.ConfigureServices` 必須傳回 `IServiceProvider`。</span><span class="sxs-lookup"><span data-stu-id="1d647-342">To use a 3rd party container, `Startup.ConfigureServices` must return `IServiceProvider`.</span></span>

* <span data-ttu-id="1d647-343">在 `DefaultModule` 中設定 Autofac：</span><span class="sxs-lookup"><span data-stu-id="1d647-343">Configure Autofac in `DefaultModule`:</span></span>

    ```csharp
    public class DefaultModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
        }
    }
    ```

<span data-ttu-id="1d647-344">在執行階段，Autofac 會用來解析類型及插入相依性。</span><span class="sxs-lookup"><span data-stu-id="1d647-344">At runtime, Autofac is used to resolve types and inject dependencies.</span></span> <span data-ttu-id="1d647-345">若要深入了解如何搭配 ASP.NET Core 使用 Autofac，請參閱 [Autofac 文件 ](https://docs.autofac.org/en/latest/integration/aspnetcore.html) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="1d647-345">To learn more about using Autofac with ASP.NET Core, see the [Autofac documentation](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span></span>

### <a name="thread-safety"></a><span data-ttu-id="1d647-346">執行緒安全</span><span class="sxs-lookup"><span data-stu-id="1d647-346">Thread safety</span></span>

<span data-ttu-id="1d647-347">建立具備執行緒安全性的 singleton 服務。</span><span class="sxs-lookup"><span data-stu-id="1d647-347">Create thread-safe singleton services.</span></span> <span data-ttu-id="1d647-348">如果 singleton 服務相依於暫時性服務，暫時性服務可能也需要具備執行緒安全性，取決於 singleton 如何使用它。</span><span class="sxs-lookup"><span data-stu-id="1d647-348">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="1d647-349">單一服務的 Factory 方法 (例如 [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*) 的第二個引數) 不需要是安全執行緒。</span><span class="sxs-lookup"><span data-stu-id="1d647-349">The factory method of single service, such as the second argument to [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), doesn't need to be thread-safe.</span></span> <span data-ttu-id="1d647-350">就像型別 (`static`) 建構函式一樣，它一定會被單一執行緒呼叫一次。</span><span class="sxs-lookup"><span data-stu-id="1d647-350">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="1d647-351">建議</span><span class="sxs-lookup"><span data-stu-id="1d647-351">Recommendations</span></span>

* <span data-ttu-id="1d647-352">不支援以 `async/await` 與 `Task` 為基礎的服務解析。</span><span class="sxs-lookup"><span data-stu-id="1d647-352">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="1d647-353">C# 不支援非同步建構函式，因此建議的模式是以同步方式解析服務後使用非同步方法。</span><span class="sxs-lookup"><span data-stu-id="1d647-353">C# does not support asynchronous constructors; therefore, the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="1d647-354">避免直接在服務容器中儲存資料與設定。</span><span class="sxs-lookup"><span data-stu-id="1d647-354">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="1d647-355">例如，使用者的購物車通常不應該新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="1d647-355">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="1d647-356">組態應該使用[選項模式](xref:fundamentals/configuration/options)。</span><span class="sxs-lookup"><span data-stu-id="1d647-356">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="1d647-357">同樣地，請避免只存在以允許存取某個其他物件的「資料持有者」物件。</span><span class="sxs-lookup"><span data-stu-id="1d647-357">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="1d647-358">最好是透過 DI 要求實際項目。</span><span class="sxs-lookup"><span data-stu-id="1d647-358">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="1d647-359">避免以靜態方式存取服務 (例如，以靜態方式設定 [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) 型別以四處使用)。</span><span class="sxs-lookup"><span data-stu-id="1d647-359">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) for use elsewhere).</span></span>

* <span data-ttu-id="1d647-360">避免使用「服務定位器模式」  。</span><span class="sxs-lookup"><span data-stu-id="1d647-360">Avoid using the *service locator pattern*.</span></span> <span data-ttu-id="1d647-361">例如，當您可以改用 DI 時，請勿叫用 <xref:System.IServiceProvider.GetService*> 來取得服務執行個體：</span><span class="sxs-lookup"><span data-stu-id="1d647-361">For example, don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead:</span></span>

  <span data-ttu-id="1d647-362">**不正確：**</span><span class="sxs-lookup"><span data-stu-id="1d647-362">**Incorrect:**</span></span>

  ```csharp
  public void MyMethod()
  {
      var options = 
          _services.GetService<IOptionsMonitor<MyOptions>>();
      var option = options.CurrentValue.Option;

      ...
  }
  ```

  <span data-ttu-id="1d647-363">**正確**：</span><span class="sxs-lookup"><span data-stu-id="1d647-363">**Correct**:</span></span>

  ```csharp
  private readonly MyOptions _options;

  public MyClass(IOptionsMonitor<MyOptions> options)
  {
      _options = options.CurrentValue;
  }

  public void MyMethod()
  {
      var option = _options.Option;

      ...
  }
  ```

* <span data-ttu-id="1d647-364">另一個要避免的服務定位器變化是插入在執行階段解析相依性的處理站。</span><span class="sxs-lookup"><span data-stu-id="1d647-364">Another service locator variation to avoid is injecting a factory that resolves dependencies at runtime.</span></span> <span data-ttu-id="1d647-365">這兩種做法都會混用[控制反轉](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion)策略。</span><span class="sxs-lookup"><span data-stu-id="1d647-365">Both of these practices mix [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

* <span data-ttu-id="1d647-366">避免以靜態方式存取 `HttpContext` (例如 [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext))。</span><span class="sxs-lookup"><span data-stu-id="1d647-366">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span></span>

<span data-ttu-id="1d647-367">就像所有的建議集，您可能會遇到需要忽略建議的情況。</span><span class="sxs-lookup"><span data-stu-id="1d647-367">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="1d647-368">例外狀況很少見&mdash;大部分是架構本身內的特殊案例。</span><span class="sxs-lookup"><span data-stu-id="1d647-368">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="1d647-369">DI 是靜態/全域物件存取模式的「替代」  選項。</span><span class="sxs-lookup"><span data-stu-id="1d647-369">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="1d647-370">如果您將 DI 與靜態物件存取混合，則可能無法實現 DI 的優點。</span><span class="sxs-lookup"><span data-stu-id="1d647-370">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1d647-371">其他資源</span><span class="sxs-lookup"><span data-stu-id="1d647-371">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="1d647-372">在 ASP.NET Core 使用 Dependency Injection 撰寫簡潔的程式碼 (MSDN)</span><span class="sxs-lookup"><span data-stu-id="1d647-372">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="1d647-373">明確相依性準則</span><span class="sxs-lookup"><span data-stu-id="1d647-373">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* <span data-ttu-id="1d647-374">[逆轉控制容器和相依性插入模式 (Martin Fowler)](https://www.martinfowler.com/articles/injection.html) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="1d647-374">[Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)](https://www.martinfowler.com/articles/injection.html)</span></span>
* <span data-ttu-id="1d647-375">[How to register a service with multiple interfaces in ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/) (如何使用 ASP.NET Core DI 中的多個介面註冊服務)</span><span class="sxs-lookup"><span data-stu-id="1d647-375">[How to register a service with multiple interfaces in ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)</span></span>
