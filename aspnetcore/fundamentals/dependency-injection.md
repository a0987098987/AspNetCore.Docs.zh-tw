---
title: .NET Core 中的相依性插入
author: rick-anderson
description: 了解 ASP.NET Core 如何實作相依性插入以及如何使用它。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/dependency-injection
ms.openlocfilehash: 8a4ee8bee09b3d6e9de932dab17bbc5c6494a492
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82767521"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="43413-103">.NET Core 中的相依性插入</span><span class="sxs-lookup"><span data-stu-id="43413-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="43413-104">作者：[Steve Smith](https://ardalis.com/) 和 [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="43413-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="43413-105">ASP.NET Core 支援相依性插入 (DI) 軟體設計模式，這是用來在類別與其相依性之間達成[控制權反轉 (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) \(英文\) 的技術。</span><span class="sxs-lookup"><span data-stu-id="43413-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="43413-106">如需有關 MVC 控制器內相依性插入的特定詳細資訊，請參閱 <xref:mvc/controllers/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="43413-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="43413-107">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="43413-107">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="43413-108">相依性插入概觀</span><span class="sxs-lookup"><span data-stu-id="43413-108">Overview of dependency injection</span></span>

<span data-ttu-id="43413-109">「相依性」\*\* 是另一個物件所需的任何物件。</span><span class="sxs-lookup"><span data-stu-id="43413-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="43413-110">檢查具有 `WriteMessage` 方法的下列 `MyDependency` 物件，應用程式中的其他類別相依於此物件：</span><span class="sxs-lookup"><span data-stu-id="43413-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

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

<span data-ttu-id="43413-111">您可以建立 `MyDependency` 類別的執行個體，讓 `WriteMessage` 方法可供類別使用。</span><span class="sxs-lookup"><span data-stu-id="43413-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="43413-112">`MyDependency` 類別是 `IndexModel` 類別的相依性：</span><span class="sxs-lookup"><span data-stu-id="43413-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

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

<span data-ttu-id="43413-113">該類別會建立 `MyDependency` 執行個體並直接相依於該執行個體。</span><span class="sxs-lookup"><span data-stu-id="43413-113">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="43413-114">程式碼相依性 (例如前一個範例) 有問題，因此基於下列原因應該避免使用：</span><span class="sxs-lookup"><span data-stu-id="43413-114">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="43413-115">若要將 `MyDependency` 取代為不同的實作，必須修改該類別。</span><span class="sxs-lookup"><span data-stu-id="43413-115">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="43413-116">若 `MyDependency` 有相依性，那些相依性必須由該類別設定。</span><span class="sxs-lookup"><span data-stu-id="43413-116">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="43413-117">在具有多個相依於 `MyDependency` 之多個類別的大型專案中，設定程式碼在不同的應用程式之間會變得鬆散。</span><span class="sxs-lookup"><span data-stu-id="43413-117">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="43413-118">此實作難以進行單元測試。</span><span class="sxs-lookup"><span data-stu-id="43413-118">This implementation is difficult to unit test.</span></span> <span data-ttu-id="43413-119">應用程式應該使用模擬 (Mock) 或虛設常式 (Stub) `MyDependency` 類別，這在使用此方法時無法使用。</span><span class="sxs-lookup"><span data-stu-id="43413-119">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="43413-120">相依性插入可透過下列方式解決這些問題：</span><span class="sxs-lookup"><span data-stu-id="43413-120">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="43413-121">使用介面或基底類別來將相依性資訊抽象化。</span><span class="sxs-lookup"><span data-stu-id="43413-121">The use of an interface or base class to abstract the dependency implementation.</span></span>
* <span data-ttu-id="43413-122">在服務容器中註冊相依性。</span><span class="sxs-lookup"><span data-stu-id="43413-122">Registration of the dependency in a service container.</span></span> <span data-ttu-id="43413-123">ASP.NET Core 提供內建服務容器 <xref:System.IServiceProvider>。</span><span class="sxs-lookup"><span data-stu-id="43413-123">ASP.NET Core provides a built-in service container, <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="43413-124">服務會在應用程式的 `Startup.ConfigureServices` 方法中註冊。</span><span class="sxs-lookup"><span data-stu-id="43413-124">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="43413-125">將服務「插入」\*\* 到服務使用位置之類別的建構函式。</span><span class="sxs-lookup"><span data-stu-id="43413-125">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="43413-126">架構會負責建立相依性的執行個體，並在不再需要時將它捨棄。</span><span class="sxs-lookup"><span data-stu-id="43413-126">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="43413-127">在[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples)中，`IMyDependency` 介面定義了服務提供給應用程式的方法：</span><span class="sxs-lookup"><span data-stu-id="43413-127">In the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

<span data-ttu-id="43413-128">這個介面是由具象型別 `MyDependency` 所實作：</span><span class="sxs-lookup"><span data-stu-id="43413-128">This interface is implemented by a concrete type, `MyDependency`:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

<span data-ttu-id="43413-129">`MyDependency` 在其建構函式中會要求 <xref:Microsoft.Extensions.Logging.ILogger`1>。</span><span class="sxs-lookup"><span data-stu-id="43413-129">`MyDependency` requests an <xref:Microsoft.Extensions.Logging.ILogger`1> in its constructor.</span></span> <span data-ttu-id="43413-130">以鏈結方式使用相依性插入並非不尋常。</span><span class="sxs-lookup"><span data-stu-id="43413-130">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="43413-131">每個要求的相依性接著會要求其自己的相依性。</span><span class="sxs-lookup"><span data-stu-id="43413-131">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="43413-132">容器會解決圖形中的相依性，並傳回完全解析的服務。</span><span class="sxs-lookup"><span data-stu-id="43413-132">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="43413-133">必須先解析的相依性集合組通常稱為「相依性樹狀結構」**、「相依性圖形」** 或「物件圖形」\*\*。</span><span class="sxs-lookup"><span data-stu-id="43413-133">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="43413-134">`IMyDependency` 與 `ILogger<TCategoryName>` 必須在服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="43413-134">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="43413-135">`IMyDependency` 是在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="43413-135">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="43413-136">`ILogger<TCategoryName>` 是由記錄抽象基礎結構所註冊，所以它是預設由架構所註冊的[架構提供的服務](#framework-provided-services)。</span><span class="sxs-lookup"><span data-stu-id="43413-136">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="43413-137">容器會利用 [(泛型) 開放類型](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types) 解析 `ILogger<TCategoryName>`，讓您不需註冊每個 [(泛型) 建構的型別](/dotnet/csharp/language-reference/language-specification/types#constructed-types)：</span><span class="sxs-lookup"><span data-stu-id="43413-137">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<>), typeof(Logger<>));
```

<span data-ttu-id="43413-138">在範例應用程式中，`IMyDependency` 服務是使用具象型別 `MyDependency` 所註冊。</span><span class="sxs-lookup"><span data-stu-id="43413-138">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="43413-139">註冊會將服務的存留期範圍限制為單一要求的存留期。</span><span class="sxs-lookup"><span data-stu-id="43413-139">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="43413-140">將在此主題稍後將說明[服務存留期](#service-lifetimes)。</span><span class="sxs-lookup"><span data-stu-id="43413-140">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> <span data-ttu-id="43413-141">每個 `services.Add{SERVICE_NAME}` 擴充方法都會新增 (並且可能會設定) 服務。</span><span class="sxs-lookup"><span data-stu-id="43413-141">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="43413-142">例如，`services.AddMvc()` 會新增 Razor Pages 與 MVC 要求的服務。</span><span class="sxs-lookup"><span data-stu-id="43413-142">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="43413-143">建議應用程式遵循此慣例。</span><span class="sxs-lookup"><span data-stu-id="43413-143">We recommended that apps follow this convention.</span></span> <span data-ttu-id="43413-144">在 [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) 命名空間中放置擴充方法，以封裝服務註冊群組。</span><span class="sxs-lookup"><span data-stu-id="43413-144">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="43413-145">如果服務的建構函式需要[內建類型](/dotnet/csharp/language-reference/keywords/built-in-types-table)，例如 `string`，可以使用[組態](xref:fundamentals/configuration/index)或[選項模式](xref:fundamentals/configuration/options)插入該類型：</span><span class="sxs-lookup"><span data-stu-id="43413-145">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

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

<span data-ttu-id="43413-146">服務執行個體是透過服務使用所在之類別建構函式所要求，而且會指派給私人欄位。</span><span class="sxs-lookup"><span data-stu-id="43413-146">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="43413-147">該欄位會視需要用來存取整個類別中的服務。</span><span class="sxs-lookup"><span data-stu-id="43413-147">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="43413-148">在範例應用程式中，會要求 `IMyDependency` 執行個體並使用它來呼叫服務的 `WriteMessage` 方法：</span><span class="sxs-lookup"><span data-stu-id="43413-148">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="services-injected-into-startup"></a><span data-ttu-id="43413-149">插入至啟動的服務</span><span class="sxs-lookup"><span data-stu-id="43413-149">Services injected into Startup</span></span>

<span data-ttu-id="43413-150">使用泛型主機（ `Startup` <xref:Microsoft.Extensions.Hosting.IHostBuilder>）時，只有下列服務類型可以插入至此函式：</span><span class="sxs-lookup"><span data-stu-id="43413-150">Only the following service types can be injected into the `Startup` constructor when using the Generic Host (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span></span>

* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

<span data-ttu-id="43413-151">服務可以插入`Startup.Configure`：</span><span class="sxs-lookup"><span data-stu-id="43413-151">Services can be injected into `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> options)
{
    ...
}
```

<span data-ttu-id="43413-152">如需詳細資訊，請參閱<xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="43413-152">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="43413-153">架構提供的服務</span><span class="sxs-lookup"><span data-stu-id="43413-153">Framework-provided services</span></span>

<span data-ttu-id="43413-154">`Startup.ConfigureServices`方法負責定義應用程式所使用的服務，包括平臺功能，例如 Entity Framework Core 和 ASP.NET Core MVC。</span><span class="sxs-lookup"><span data-stu-id="43413-154">The `Startup.ConfigureServices` method is responsible for defining the services that the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="43413-155">一開始， `IServiceCollection`提供的`ConfigureServices`會根據[主機的設定方式](xref:fundamentals/index#host)，來擁有架構所定義的服務。</span><span class="sxs-lookup"><span data-stu-id="43413-155">Initially, the `IServiceCollection` provided to `ConfigureServices` has services defined by the framework depending on [how the host was configured](xref:fundamentals/index#host).</span></span> <span data-ttu-id="43413-156">以 ASP.NET Core 範本為基礎的應用程式，在架構中註冊數百項服務並不常見。</span><span class="sxs-lookup"><span data-stu-id="43413-156">It's not uncommon for an app based on an ASP.NET Core template to have hundreds of services registered by the framework.</span></span> <span data-ttu-id="43413-157">下表列出架構註冊服務的小型範例。</span><span class="sxs-lookup"><span data-stu-id="43413-157">A small sample of framework-registered services is listed in the following table.</span></span>

| <span data-ttu-id="43413-158">服務類型</span><span class="sxs-lookup"><span data-stu-id="43413-158">Service Type</span></span> | <span data-ttu-id="43413-159">存留期</span><span class="sxs-lookup"><span data-stu-id="43413-159">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="43413-160">暫時性</span><span class="sxs-lookup"><span data-stu-id="43413-160">Transient</span></span> |
| `IHostApplicationLifetime` | <span data-ttu-id="43413-161">單一</span><span class="sxs-lookup"><span data-stu-id="43413-161">Singleton</span></span> |
| `IWebHostEnvironment` | <span data-ttu-id="43413-162">單一</span><span class="sxs-lookup"><span data-stu-id="43413-162">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="43413-163">單一</span><span class="sxs-lookup"><span data-stu-id="43413-163">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="43413-164">暫時性</span><span class="sxs-lookup"><span data-stu-id="43413-164">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="43413-165">單一</span><span class="sxs-lookup"><span data-stu-id="43413-165">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="43413-166">暫時性</span><span class="sxs-lookup"><span data-stu-id="43413-166">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="43413-167">單一</span><span class="sxs-lookup"><span data-stu-id="43413-167">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="43413-168">單一</span><span class="sxs-lookup"><span data-stu-id="43413-168">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="43413-169">單一</span><span class="sxs-lookup"><span data-stu-id="43413-169">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="43413-170">暫時性</span><span class="sxs-lookup"><span data-stu-id="43413-170">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="43413-171">單一</span><span class="sxs-lookup"><span data-stu-id="43413-171">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="43413-172">單一</span><span class="sxs-lookup"><span data-stu-id="43413-172">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="43413-173">單一</span><span class="sxs-lookup"><span data-stu-id="43413-173">Singleton</span></span> |

## <a name="register-additional-services-with-extension-methods"></a><span data-ttu-id="43413-174">以擴充方法註冊其他服務</span><span class="sxs-lookup"><span data-stu-id="43413-174">Register additional services with extension methods</span></span>

<span data-ttu-id="43413-175">當可以使用服務集合擴充方法來註冊服務 (如果需要，也可以註冊其相依服務) 時，慣例是使用單一 `Add{SERVICE_NAME}` 擴充方法來註冊該服務要求的所有服務。</span><span class="sxs-lookup"><span data-stu-id="43413-175">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="43413-176">下列程式碼範例說明如何使用擴充方法[\<AddDbCoNtext TCoNtext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)和， <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>將其他服務新增至容器：</span><span class="sxs-lookup"><span data-stu-id="43413-176">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) and <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    ...
}
```

<span data-ttu-id="43413-177">如需詳細資訊，請參閱 API 文件中的 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> 類別。</span><span class="sxs-lookup"><span data-stu-id="43413-177">For more information, see the <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> class in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="43413-178">執行個體存留期</span><span class="sxs-lookup"><span data-stu-id="43413-178">Service lifetimes</span></span>

<span data-ttu-id="43413-179">為每個已註冊的服務選擇適當的存留期。</span><span class="sxs-lookup"><span data-stu-id="43413-179">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="43413-180">ASP.NET Core 服務可以使用下列存留期進行設定：</span><span class="sxs-lookup"><span data-stu-id="43413-180">ASP.NET Core services can be configured with the following lifetimes:</span></span>

### <a name="transient"></a><span data-ttu-id="43413-181">暫時性</span><span class="sxs-lookup"><span data-stu-id="43413-181">Transient</span></span>

<span data-ttu-id="43413-182">每次從服務容器要求暫時性存留期服務 (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) 時都會建立它們。</span><span class="sxs-lookup"><span data-stu-id="43413-182">Transient lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) are created each time they're requested from the service container.</span></span> <span data-ttu-id="43413-183">此存留期最適合用於輕量型的無狀態服務。</span><span class="sxs-lookup"><span data-stu-id="43413-183">This lifetime works best for lightweight, stateless services.</span></span>

### <a name="scoped"></a><span data-ttu-id="43413-184">具範圍</span><span class="sxs-lookup"><span data-stu-id="43413-184">Scoped</span></span>

<span data-ttu-id="43413-185">具範圍存留期服務 (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) 會在每次用戶端要求 (連線) 時建立一次。</span><span class="sxs-lookup"><span data-stu-id="43413-185">Scoped lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) are created once per client request (connection).</span></span>

> [!WARNING]
> <span data-ttu-id="43413-186">在中介軟體中使用具範圍服務時，請將該服務插入 `Invoke` 或 `InvokeAsync` 方法中。</span><span class="sxs-lookup"><span data-stu-id="43413-186">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="43413-187">不要透過函式[插入](xref:mvc/controllers/dependency-injection#constructor-injection)來插入，因為它會強制服務的行為就像 singleton 一樣。</span><span class="sxs-lookup"><span data-stu-id="43413-187">Don't inject via [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="43413-188">如需詳細資訊，請參閱<xref:fundamentals/middleware/write#per-request-middleware-dependencies>。</span><span class="sxs-lookup"><span data-stu-id="43413-188">For more information, see <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span></span>

### <a name="singleton"></a><span data-ttu-id="43413-189">單一</span><span class="sxs-lookup"><span data-stu-id="43413-189">Singleton</span></span>

<span data-ttu-id="43413-190">當第一次收到有關單一資料庫存留期服務的要求時 (或是當執行 `Startup.ConfigureServices` 且隨著服務註冊指定執行個體時)，即會建立單一資料庫存留期服務 (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>)。</span><span class="sxs-lookup"><span data-stu-id="43413-190">Singleton lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) are created the first time they're requested (or when `Startup.ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="43413-191">每個後續要求都會使用相同的執行個體。</span><span class="sxs-lookup"><span data-stu-id="43413-191">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="43413-192">若應用程式要求單一資料庫行為，建議您允許服務容器管理服務的存留期。</span><span class="sxs-lookup"><span data-stu-id="43413-192">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="43413-193">不要實作單一資料庫設計模式並為使用者提供可用來管理類別中物件存留期的程式碼。</span><span class="sxs-lookup"><span data-stu-id="43413-193">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="43413-194">從單一資料庫解析具範圍服務是非常危險的。</span><span class="sxs-lookup"><span data-stu-id="43413-194">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="43413-195">處理後續要求時，它可能會導致服務有不正確的狀態。</span><span class="sxs-lookup"><span data-stu-id="43413-195">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

## <a name="service-registration-methods"></a><span data-ttu-id="43413-196">服務註冊方法</span><span class="sxs-lookup"><span data-stu-id="43413-196">Service registration methods</span></span>

<span data-ttu-id="43413-197">服務註冊擴充方法提供在特定案例中很有用的多載。</span><span class="sxs-lookup"><span data-stu-id="43413-197">Service registration extension methods offer overloads that are useful in specific scenarios.</span></span>

| <span data-ttu-id="43413-198">方法</span><span class="sxs-lookup"><span data-stu-id="43413-198">Method</span></span> | <span data-ttu-id="43413-199">自動</span><span class="sxs-lookup"><span data-stu-id="43413-199">Automatic</span></span><br><span data-ttu-id="43413-200">物件</span><span class="sxs-lookup"><span data-stu-id="43413-200">object</span></span><br><span data-ttu-id="43413-201">處置</span><span class="sxs-lookup"><span data-stu-id="43413-201">disposal</span></span> | <span data-ttu-id="43413-202">多個</span><span class="sxs-lookup"><span data-stu-id="43413-202">Multiple</span></span><br><span data-ttu-id="43413-203">實作</span><span class="sxs-lookup"><span data-stu-id="43413-203">implementations</span></span> | <span data-ttu-id="43413-204">傳遞引數</span><span class="sxs-lookup"><span data-stu-id="43413-204">Pass args</span></span> |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br><span data-ttu-id="43413-205">範例：</span><span class="sxs-lookup"><span data-stu-id="43413-205">Example:</span></span><br>`services.AddSingleton<IMyDep, MyDep>();` | <span data-ttu-id="43413-206">是</span><span class="sxs-lookup"><span data-stu-id="43413-206">Yes</span></span> | <span data-ttu-id="43413-207">是</span><span class="sxs-lookup"><span data-stu-id="43413-207">Yes</span></span> | <span data-ttu-id="43413-208">否</span><span class="sxs-lookup"><span data-stu-id="43413-208">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br><span data-ttu-id="43413-209">範例：</span><span class="sxs-lookup"><span data-stu-id="43413-209">Examples:</span></span><br>`services.AddSingleton<IMyDep>(sp => new MyDep());`<br>`services.AddSingleton<IMyDep>(sp => new MyDep("A string!"));` | <span data-ttu-id="43413-210">是</span><span class="sxs-lookup"><span data-stu-id="43413-210">Yes</span></span> | <span data-ttu-id="43413-211">是</span><span class="sxs-lookup"><span data-stu-id="43413-211">Yes</span></span> | <span data-ttu-id="43413-212">是</span><span class="sxs-lookup"><span data-stu-id="43413-212">Yes</span></span> |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br><span data-ttu-id="43413-213">範例：</span><span class="sxs-lookup"><span data-stu-id="43413-213">Example:</span></span><br>`services.AddSingleton<MyDep>();` | <span data-ttu-id="43413-214">是</span><span class="sxs-lookup"><span data-stu-id="43413-214">Yes</span></span> | <span data-ttu-id="43413-215">否</span><span class="sxs-lookup"><span data-stu-id="43413-215">No</span></span> | <span data-ttu-id="43413-216">否</span><span class="sxs-lookup"><span data-stu-id="43413-216">No</span></span> |
| `AddSingleton<{SERVICE}>(new {IMPLEMENTATION})`<br><span data-ttu-id="43413-217">範例：</span><span class="sxs-lookup"><span data-stu-id="43413-217">Examples:</span></span><br>`services.AddSingleton<IMyDep>(new MyDep());`<br>`services.AddSingleton<IMyDep>(new MyDep("A string!"));` | <span data-ttu-id="43413-218">否</span><span class="sxs-lookup"><span data-stu-id="43413-218">No</span></span> | <span data-ttu-id="43413-219">是</span><span class="sxs-lookup"><span data-stu-id="43413-219">Yes</span></span> | <span data-ttu-id="43413-220">是</span><span class="sxs-lookup"><span data-stu-id="43413-220">Yes</span></span> |
| `AddSingleton(new {IMPLEMENTATION})`<br><span data-ttu-id="43413-221">範例：</span><span class="sxs-lookup"><span data-stu-id="43413-221">Examples:</span></span><br>`services.AddSingleton(new MyDep());`<br>`services.AddSingleton(new MyDep("A string!"));` | <span data-ttu-id="43413-222">否</span><span class="sxs-lookup"><span data-stu-id="43413-222">No</span></span> | <span data-ttu-id="43413-223">否</span><span class="sxs-lookup"><span data-stu-id="43413-223">No</span></span> | <span data-ttu-id="43413-224">是</span><span class="sxs-lookup"><span data-stu-id="43413-224">Yes</span></span> |

<span data-ttu-id="43413-225">如需類型處置的詳細資訊，請參閱[＜服務處置＞](#disposal-of-services)一節。</span><span class="sxs-lookup"><span data-stu-id="43413-225">For more information on type disposal, see the [Disposal of services](#disposal-of-services) section.</span></span> <span data-ttu-id="43413-226">多個實作的常見案例是[模擬測試類型](xref:test/integration-tests#inject-mock-services)。</span><span class="sxs-lookup"><span data-stu-id="43413-226">A common scenario for multiple implementations is [mocking types for testing](xref:test/integration-tests#inject-mock-services).</span></span>

<span data-ttu-id="43413-227">如果還未註冊任何實作，則 `TryAdd{LIFETIME}` 方法只會註冊服務。</span><span class="sxs-lookup"><span data-stu-id="43413-227">`TryAdd{LIFETIME}` methods only register the service if there isn't already an implementation registered.</span></span>

<span data-ttu-id="43413-228">在下列範例中，第一行會為 `IMyDependency` 註冊 `MyDependency`。</span><span class="sxs-lookup"><span data-stu-id="43413-228">In the following example, the first line registers `MyDependency` for `IMyDependency`.</span></span> <span data-ttu-id="43413-229">第二行則沒有任何作用，因為 `IMyDependency` 已具有註冊的實作：</span><span class="sxs-lookup"><span data-stu-id="43413-229">The second line has no effect because `IMyDependency` already has a registered implementation:</span></span>

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

<span data-ttu-id="43413-230">如需詳細資訊，請參閱</span><span class="sxs-lookup"><span data-stu-id="43413-230">For more information, see:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

<span data-ttu-id="43413-231">如果還沒有「相同類型的」\*\* 實作，則 [TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) 方法只會註冊服務。</span><span class="sxs-lookup"><span data-stu-id="43413-231">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) methods only register the service if there isn't already an implementation *of the same type*.</span></span> <span data-ttu-id="43413-232">多個服務會透過 `IEnumerable<{SERVICE}>` 解析。</span><span class="sxs-lookup"><span data-stu-id="43413-232">Multiple services are resolved via `IEnumerable<{SERVICE}>`.</span></span> <span data-ttu-id="43413-233">註冊服務時，如果尚未新增相同類型的執行個體，則開發人員只會希望新增執行個體。</span><span class="sxs-lookup"><span data-stu-id="43413-233">When registering services, the developer only wants to add an instance if one of the same type hasn't already been added.</span></span> <span data-ttu-id="43413-234">程式庫作者通常會使用此方法來避免註冊容器中某個執行個體的兩個複本。</span><span class="sxs-lookup"><span data-stu-id="43413-234">Generally, this method is used by library authors to avoid registering two copies of an instance in the container.</span></span>

<span data-ttu-id="43413-235">在下列範例中，第一行會為 `IMyDep1` 註冊 `MyDep`。</span><span class="sxs-lookup"><span data-stu-id="43413-235">In the following example, the first line registers `MyDep` for `IMyDep1`.</span></span> <span data-ttu-id="43413-236">第二行會為 `IMyDep2` 註冊 `MyDep`。</span><span class="sxs-lookup"><span data-stu-id="43413-236">The second line registers `MyDep` for `IMyDep2`.</span></span> <span data-ttu-id="43413-237">第三行則沒有任何作用，因為 `IMyDep1` 已具有 `MyDep` 的已註冊實作：</span><span class="sxs-lookup"><span data-stu-id="43413-237">The third line has no effect because `IMyDep1` already has a registered implementation of `MyDep`:</span></span>

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a><span data-ttu-id="43413-238">建構函式插入行為</span><span class="sxs-lookup"><span data-stu-id="43413-238">Constructor injection behavior</span></span>

<span data-ttu-id="43413-239">服務可以透過兩個機制來解析：</span><span class="sxs-lookup"><span data-stu-id="43413-239">Services can be resolved by two mechanisms:</span></span>

* <xref:System.IServiceProvider>
* <span data-ttu-id="43413-240"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities>&ndash;允許在相依性插入容器中，不需要服務註冊即可建立物件。</span><span class="sxs-lookup"><span data-stu-id="43413-240"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="43413-241">搭配使用者面向抽象 (例如標籤協助程式、MVC 控制器與模型繫結器) 使用 `ActivatorUtilities`。</span><span class="sxs-lookup"><span data-stu-id="43413-241">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="43413-242">建構函式可以接受不是由相依性插入提供的引數，但引數必須指派預設值。</span><span class="sxs-lookup"><span data-stu-id="43413-242">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="43413-243">`IServiceProvider`當服務由或`ActivatorUtilities`解析時，程式的[插入](xref:mvc/controllers/dependency-injection#constructor-injection)需要*公用*的函式。</span><span class="sxs-lookup"><span data-stu-id="43413-243">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) requires a *public* constructor.</span></span>

<span data-ttu-id="43413-244">當服務由`ActivatorUtilities`解析時，程式的[插入](xref:mvc/controllers/dependency-injection#constructor-injection)只需要有一個適用的函式。</span><span class="sxs-lookup"><span data-stu-id="43413-244">When services are resolved by `ActivatorUtilities`, [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) requires that only one applicable constructor exists.</span></span> <span data-ttu-id="43413-245">支援建構函式多載，但只能有一個多載存在，其引數可以藉由相依性插入而完成。</span><span class="sxs-lookup"><span data-stu-id="43413-245">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="43413-246">Entity Framework 內容</span><span class="sxs-lookup"><span data-stu-id="43413-246">Entity Framework contexts</span></span>

<span data-ttu-id="43413-247">因為一般會將 Web 應用程式資料庫作業範圍設定為用戶端要求，所以通常會使用[具範圍存留期](#service-lifetimes)將 Entity Framework 內容新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="43413-247">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="43413-248">註冊資料庫內容時，如果 [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) 多載未指定留存期，則會將範圍設定為預設存留期。</span><span class="sxs-lookup"><span data-stu-id="43413-248">The default lifetime is scoped if a lifetime isn't specified by an [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) overload when registering the database context.</span></span> <span data-ttu-id="43413-249">指定存留期的服務不應該使用存留期比服務還短的資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="43413-249">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="43413-250">留期和註冊選項</span><span class="sxs-lookup"><span data-stu-id="43413-250">Lifetime and registration options</span></span>

<span data-ttu-id="43413-251">為了示範這些存留期和註冊選項之間的差異，請考慮下列介面，以具有唯一識別碼 `OperationId` 的「作業」代表工作。</span><span class="sxs-lookup"><span data-stu-id="43413-251">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="43413-252">視如何針對下列介面設定作業服務存留期而定，當類別要求時，容器會提供相同或不同的服務執行個體：</span><span class="sxs-lookup"><span data-stu-id="43413-252">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

<span data-ttu-id="43413-253">介面是在 `Operation` 類別中實作。</span><span class="sxs-lookup"><span data-stu-id="43413-253">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="43413-254">若未提供 GUID，`Operation` 建構函式會產生 GUID：</span><span class="sxs-lookup"><span data-stu-id="43413-254">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

<span data-ttu-id="43413-255">會註冊相依於每種其他 `Operation` 類型的 `OperationService`。</span><span class="sxs-lookup"><span data-stu-id="43413-255">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="43413-256">當透過相依性插入要求 `OperationService` 時，它會根據相依服務的存留期擷取每個服務的新執行個體或現有執行個體。</span><span class="sxs-lookup"><span data-stu-id="43413-256">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="43413-257">當因收到來自容器的要求而建立暫時性服務時，`IOperationTransient` 服務的 `OperationId` 會與 `OperationService` 的 `OperationId` 不同。</span><span class="sxs-lookup"><span data-stu-id="43413-257">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="43413-258">`OperationService` 會收到 `IOperationTransient` 類別的新執行個體。</span><span class="sxs-lookup"><span data-stu-id="43413-258">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="43413-259">新執行個體會產生不同的 `OperationId`。</span><span class="sxs-lookup"><span data-stu-id="43413-259">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="43413-260">當因用戶端要求而建立具範圍的服務時，`IOperationScoped` 服務與用戶端要求中 `OperationService` 的 `OperationId` 皆相同。</span><span class="sxs-lookup"><span data-stu-id="43413-260">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="43413-261">在不同的用戶端要求中，兩個服務的 `OperationId` 值都不同。</span><span class="sxs-lookup"><span data-stu-id="43413-261">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="43413-262">當建立一次 singleton 與 singleton 執行個體服務，並在所有用戶端要求與所有服務中使用時，`OperationId` 在所有服務要求中會固定不變。</span><span class="sxs-lookup"><span data-stu-id="43413-262">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

<span data-ttu-id="43413-263">在 `Startup.ConfigureServices` 中，每個類型都會根據其具名存留期新增至容器：</span><span class="sxs-lookup"><span data-stu-id="43413-263">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

<span data-ttu-id="43413-264">`IOperationSingletonInstance` 服務使用具有 `Guid.Empty` 已知識別碼的特定執行個體。</span><span class="sxs-lookup"><span data-stu-id="43413-264">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="43413-265">很明顯此型別是使用中 (其 GUID 是所有區域)。</span><span class="sxs-lookup"><span data-stu-id="43413-265">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="43413-266">範例應用程式示範個別要求內與個別要求之間的物件存留期。</span><span class="sxs-lookup"><span data-stu-id="43413-266">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="43413-267">範例應用程式的 `IndexModel` 會要求每種 `IOperation` 型別與 `OperationService`。</span><span class="sxs-lookup"><span data-stu-id="43413-267">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="43413-268">頁面接著會透過屬性指派來顯示所有頁面模型類別與服務的 `OperationId` 值：</span><span class="sxs-lookup"><span data-stu-id="43413-268">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

<span data-ttu-id="43413-269">下面兩個輸出顯示兩個要求的結果：</span><span class="sxs-lookup"><span data-stu-id="43413-269">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="43413-270">**第一個要求：**</span><span class="sxs-lookup"><span data-stu-id="43413-270">**First request:**</span></span>

<span data-ttu-id="43413-271">控制器作業：</span><span class="sxs-lookup"><span data-stu-id="43413-271">Controller operations:</span></span>

<span data-ttu-id="43413-272">暫時性： d233e165-f417-469b-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="43413-272">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="43413-273">具範圍： 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="43413-273">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="43413-274">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="43413-274">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="43413-275">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="43413-275">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="43413-276">`OperationService` 作業：</span><span class="sxs-lookup"><span data-stu-id="43413-276">`OperationService` operations:</span></span>

<span data-ttu-id="43413-277">暫時性： c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="43413-277">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="43413-278">具範圍： 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="43413-278">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="43413-279">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="43413-279">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="43413-280">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="43413-280">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="43413-281">**:第二個要求：**</span><span class="sxs-lookup"><span data-stu-id="43413-281">**Second request:**</span></span>

<span data-ttu-id="43413-282">控制器作業：</span><span class="sxs-lookup"><span data-stu-id="43413-282">Controller operations:</span></span>

<span data-ttu-id="43413-283">暫時性： b63bd538-0a37-4ff1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="43413-283">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="43413-284">具範圍： 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="43413-284">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="43413-285">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="43413-285">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="43413-286">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="43413-286">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="43413-287">`OperationService` 作業：</span><span class="sxs-lookup"><span data-stu-id="43413-287">`OperationService` operations:</span></span>

<span data-ttu-id="43413-288">暫時性： c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="43413-288">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="43413-289">具範圍： 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="43413-289">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="43413-290">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="43413-290">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="43413-291">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="43413-291">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="43413-292">觀察哪些 `OperationId` 值在要求內以及要求之間不同：</span><span class="sxs-lookup"><span data-stu-id="43413-292">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="43413-293">「暫時性」\*\* 物件一律不同。</span><span class="sxs-lookup"><span data-stu-id="43413-293">*Transient* objects are always different.</span></span> <span data-ttu-id="43413-294">第一個與第二個用戶端要求的暫時性 `OperationId` 值在兩個 `OperationService` 作業之間與各用戶端要求之間都不同。</span><span class="sxs-lookup"><span data-stu-id="43413-294">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="43413-295">新執行個體會提供給每個服務要求以及用戶端要求。</span><span class="sxs-lookup"><span data-stu-id="43413-295">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="43413-296">「具範圍」\*\* 物件在同一個用戶端要求內皆相同，但在不同的用戶端要求之間則不同。</span><span class="sxs-lookup"><span data-stu-id="43413-296">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="43413-297">*Singleton*不論是否在中`Operation` `Startup.ConfigureServices`提供實例，每個物件和每個要求的單一物件都是相同的。</span><span class="sxs-lookup"><span data-stu-id="43413-297">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `Startup.ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="43413-298">從主要呼叫服務</span><span class="sxs-lookup"><span data-stu-id="43413-298">Call services from main</span></span>

<span data-ttu-id="43413-299">使用 [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) 建立 <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>，以解析應用程式範圍中的範圍服務。</span><span class="sxs-lookup"><span data-stu-id="43413-299">Create an <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="43413-300">此法可用於在開機時存取範圍服務，以執行初始化工作。</span><span class="sxs-lookup"><span data-stu-id="43413-300">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="43413-301">下列範例示範如何在 `Program.Main` 中取得 `MyScopedService`：</span><span class="sxs-lookup"><span data-stu-id="43413-301">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static async Task Main(string[] args)
    {
        var host = CreateHostBuilder(args).Build();

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
    
        await host.RunAsync();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStartup<Startup>();
            });
}
```

## <a name="scope-validation"></a><span data-ttu-id="43413-302">範圍驗證</span><span class="sxs-lookup"><span data-stu-id="43413-302">Scope validation</span></span>

<span data-ttu-id="43413-303">當應用程式在開發環境中執行並呼叫[CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings)來建立主機時，預設服務提供者會執行檢查以確認：</span><span class="sxs-lookup"><span data-stu-id="43413-303">When the app is running in the Development environment and calls [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) to build the host, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="43413-304">範圍服務不是直接或間接由開機服務提供者解析。</span><span class="sxs-lookup"><span data-stu-id="43413-304">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="43413-305">範圍服務不是直接或間接插入至單一服務。</span><span class="sxs-lookup"><span data-stu-id="43413-305">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="43413-306">根服務提供者會在呼叫 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> 時建立。</span><span class="sxs-lookup"><span data-stu-id="43413-306">The root service provider is created when <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> is called.</span></span> <span data-ttu-id="43413-307">當提供者啟動應用程式時，根服務提供者的存留期與應用程式/伺服器的存留期一致，並會在應用程式關閉時處置。</span><span class="sxs-lookup"><span data-stu-id="43413-307">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="43413-308">範圍服務會由建立這些服務的容器處置。</span><span class="sxs-lookup"><span data-stu-id="43413-308">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="43413-309">若是在根容器中建立範圍服務，因為當應用程式/伺服器關機時，服務只會由根容器處理，所以服務的存留期會提升為單一服務等級。</span><span class="sxs-lookup"><span data-stu-id="43413-309">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="43413-310">當呼叫 `BuildServiceProvider` 時，驗證服務範圍會攔截到這些情況。</span><span class="sxs-lookup"><span data-stu-id="43413-310">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="43413-311">如需詳細資訊，請參閱<xref:fundamentals/host/web-host#scope-validation>。</span><span class="sxs-lookup"><span data-stu-id="43413-311">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="43413-312">要求服務</span><span class="sxs-lookup"><span data-stu-id="43413-312">Request Services</span></span>

<span data-ttu-id="43413-313">來自 `HttpContext`，在 ASP.NET Core 要求內提供的服務是透過 [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) 集合公開。</span><span class="sxs-lookup"><span data-stu-id="43413-313">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) collection.</span></span>

<span data-ttu-id="43413-314">要求服務代表您在應用程式中設定和要求的服務。</span><span class="sxs-lookup"><span data-stu-id="43413-314">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="43413-315">當物件指定相依性時，這些會由在 `RequestServices` 中找到的類型來滿足，而非 `ApplicationServices`。</span><span class="sxs-lookup"><span data-stu-id="43413-315">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="43413-316">一般而言，應用程式不應該直接使用這些屬性。</span><span class="sxs-lookup"><span data-stu-id="43413-316">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="43413-317">相反地，請透過類別的函式要求類別所需的類型，並允許架構插入相依性。</span><span class="sxs-lookup"><span data-stu-id="43413-317">Instead, request the types that classes require via class constructors and allow the framework to inject the dependencies.</span></span> <span data-ttu-id="43413-318">這會產生容易測試的類別。</span><span class="sxs-lookup"><span data-stu-id="43413-318">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="43413-319">偏好要求相依性作為建構函式參數，而不要存取 `RequestServices` 集合。</span><span class="sxs-lookup"><span data-stu-id="43413-319">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="43413-320">針對相依性插入設計服務</span><span class="sxs-lookup"><span data-stu-id="43413-320">Design services for dependency injection</span></span>

<span data-ttu-id="43413-321">最佳做法是：</span><span class="sxs-lookup"><span data-stu-id="43413-321">Best practices are to:</span></span>

* <span data-ttu-id="43413-322">設計服務以使用相依性插入來取得其相依性。</span><span class="sxs-lookup"><span data-stu-id="43413-322">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="43413-323">避免具狀態的靜態類別和成員。</span><span class="sxs-lookup"><span data-stu-id="43413-323">Avoid stateful, static classes and members.</span></span> <span data-ttu-id="43413-324">將應用程式設計成使用單一服務，以避免建立全域狀態。</span><span class="sxs-lookup"><span data-stu-id="43413-324">Design apps to use singleton services instead, which avoid creating global state.</span></span>
* <span data-ttu-id="43413-325">避免直接在服務內具現化相依類別。</span><span class="sxs-lookup"><span data-stu-id="43413-325">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="43413-326">直接具現化會將程式碼耦合到特定實作。</span><span class="sxs-lookup"><span data-stu-id="43413-326">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="43413-327">讓應用程式類別維持在小型、情況良好且可輕鬆測試的狀態。</span><span class="sxs-lookup"><span data-stu-id="43413-327">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="43413-328">若類別有太多插入的相依性，這通常表示類別有太多責任而且正違反[單一責任原則 (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="43413-328">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="43413-329">將類別負責的某些部分移到新的類別，以嘗試重構類別。</span><span class="sxs-lookup"><span data-stu-id="43413-329">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="43413-330">請記住，Razor Pages 頁面模型類別與 MVC 控制器類別應該專注於 UI 考量。</span><span class="sxs-lookup"><span data-stu-id="43413-330">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="43413-331">商務規則和資料存取實作詳細資料應該保存在適合這些[分開考量](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) (Separation of Concerns) 類別中。</span><span class="sxs-lookup"><span data-stu-id="43413-331">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="43413-332">處置服務</span><span class="sxs-lookup"><span data-stu-id="43413-332">Disposal of services</span></span>

<span data-ttu-id="43413-333">容器會為它建立的 <xref:System.IDisposable> 類型呼叫 <xref:System.IDisposable.Dispose*>。</span><span class="sxs-lookup"><span data-stu-id="43413-333">The container calls <xref:System.IDisposable.Dispose*> for the <xref:System.IDisposable> types it creates.</span></span> <span data-ttu-id="43413-334">若執行個體由使用者程式碼新增到容器中，它將不會自動處置。</span><span class="sxs-lookup"><span data-stu-id="43413-334">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

<span data-ttu-id="43413-335">在下列範例中，服務會建立服務容器並自動處置：</span><span class="sxs-lookup"><span data-stu-id="43413-335">In the following example, the services are created by the service container and disposed automatically:</span></span>

```csharp
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}

public interface IService3 {}
public class Service3 : IService3, IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<IService3>(sp => new Service3());
}
```

<span data-ttu-id="43413-336">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="43413-336">In the following example:</span></span>

* <span data-ttu-id="43413-337">服務容器不會建立服務實例。</span><span class="sxs-lookup"><span data-stu-id="43413-337">The service instances aren't created by the service container.</span></span>
* <span data-ttu-id="43413-338">架構不知道預期的服務存留期。</span><span class="sxs-lookup"><span data-stu-id="43413-338">The intended service lifetimes aren't known by the framework.</span></span>
* <span data-ttu-id="43413-339">架構不會自動處置服務。</span><span class="sxs-lookup"><span data-stu-id="43413-339">The framework doesn't dispose of the services automatically.</span></span>
* <span data-ttu-id="43413-340">如果服務未在開發人員程式碼中明確處置，它們會持續存在，直到應用程式關閉為止。</span><span class="sxs-lookup"><span data-stu-id="43413-340">If the services aren't explicitly disposed in developer code, they persist until the app shuts down.</span></span>

```csharp
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<Service1>(new Service1());
    services.AddSingleton(new Service2());
}
```

<span data-ttu-id="43413-341">如需服務處置選項的討論，請參閱[在 ASP.NET Core 中處置 IDisposables 的四種方式](https://andrewlock.net/four-ways-to-dispose-idisposables-in-asp-net-core/)。</span><span class="sxs-lookup"><span data-stu-id="43413-341">For a discussion of service disposal options, see [Four ways to dispose IDisposables in ASP.NET Core](https://andrewlock.net/four-ways-to-dispose-idisposables-in-asp-net-core/).</span></span>

## <a name="default-service-container-replacement"></a><span data-ttu-id="43413-342">預設服務容器取代</span><span class="sxs-lookup"><span data-stu-id="43413-342">Default service container replacement</span></span>

<span data-ttu-id="43413-343">內建的服務容器是設計用來滿足架構和大部分取用者應用程式的需求。</span><span class="sxs-lookup"><span data-stu-id="43413-343">The built-in service container is designed to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="43413-344">我們建議使用內建容器，除非您需要內建容器不支援的特定功能，例如：</span><span class="sxs-lookup"><span data-stu-id="43413-344">We recommend using the built-in container unless you need a specific feature that the built-in container doesn't support, such as:</span></span>

* <span data-ttu-id="43413-345">屬性插入</span><span class="sxs-lookup"><span data-stu-id="43413-345">Property injection</span></span>
* <span data-ttu-id="43413-346">根據名稱插入</span><span class="sxs-lookup"><span data-stu-id="43413-346">Injection based on name</span></span>
* <span data-ttu-id="43413-347">子容器</span><span class="sxs-lookup"><span data-stu-id="43413-347">Child containers</span></span>
* <span data-ttu-id="43413-348">自訂生命週期管理</span><span class="sxs-lookup"><span data-stu-id="43413-348">Custom lifetime management</span></span>
* <span data-ttu-id="43413-349">`Func<T>` 支援延遲初始設定</span><span class="sxs-lookup"><span data-stu-id="43413-349">`Func<T>` support for lazy initialization</span></span>
* <span data-ttu-id="43413-350">以慣例為基礎的註冊</span><span class="sxs-lookup"><span data-stu-id="43413-350">Convention-based registration</span></span>

<span data-ttu-id="43413-351">下列協力廠商容器可以與 ASP.NET Core 應用程式搭配使用：</span><span class="sxs-lookup"><span data-stu-id="43413-351">The following 3rd party containers can be used with ASP.NET Core apps:</span></span>

* [<span data-ttu-id="43413-352">Autofac</span><span class="sxs-lookup"><span data-stu-id="43413-352">Autofac</span></span>](https://autofac.readthedocs.io/en/latest/integration/aspnetcore.html)
* [<span data-ttu-id="43413-353">DryIoc</span><span class="sxs-lookup"><span data-stu-id="43413-353">DryIoc</span></span>](https://www.nuget.org/packages/DryIoc.Microsoft.DependencyInjection)
* [<span data-ttu-id="43413-354">Grace</span><span class="sxs-lookup"><span data-stu-id="43413-354">Grace</span></span>](https://www.nuget.org/packages/Grace.DependencyInjection.Extensions)
* [<span data-ttu-id="43413-355">LightInject</span><span class="sxs-lookup"><span data-stu-id="43413-355">LightInject</span></span>](https://github.com/seesharper/LightInject.Microsoft.DependencyInjection)
* [<span data-ttu-id="43413-356">拉馬爾</span><span class="sxs-lookup"><span data-stu-id="43413-356">Lamar</span></span>](https://jasperfx.github.io/lamar/)
* [<span data-ttu-id="43413-357">Stashbox</span><span class="sxs-lookup"><span data-stu-id="43413-357">Stashbox</span></span>](https://github.com/z4kn4fein/stashbox-extensions-dependencyinjection)
* [<span data-ttu-id="43413-358">Unity</span><span class="sxs-lookup"><span data-stu-id="43413-358">Unity</span></span>](https://www.nuget.org/packages/Unity.Microsoft.DependencyInjection)

### <a name="thread-safety"></a><span data-ttu-id="43413-359">執行緒安全</span><span class="sxs-lookup"><span data-stu-id="43413-359">Thread safety</span></span>

<span data-ttu-id="43413-360">建立具備執行緒安全性的 singleton 服務。</span><span class="sxs-lookup"><span data-stu-id="43413-360">Create thread-safe singleton services.</span></span> <span data-ttu-id="43413-361">如果 singleton 服務相依於暫時性服務，暫時性服務可能也需要具備執行緒安全性，取決於 singleton 如何使用它。</span><span class="sxs-lookup"><span data-stu-id="43413-361">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="43413-362">單一服務的 Factory 方法 (例如 [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*) 的第二個引數) 不需要是安全執行緒。</span><span class="sxs-lookup"><span data-stu-id="43413-362">The factory method of single service, such as the second argument to [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), doesn't need to be thread-safe.</span></span> <span data-ttu-id="43413-363">就像型別 (`static`) 建構函式一樣，它一定會被單一執行緒呼叫一次。</span><span class="sxs-lookup"><span data-stu-id="43413-363">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="43413-364">建議</span><span class="sxs-lookup"><span data-stu-id="43413-364">Recommendations</span></span>

* <span data-ttu-id="43413-365">不支援以 `async/await` 與 `Task` 為基礎的服務解析。</span><span class="sxs-lookup"><span data-stu-id="43413-365">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="43413-366">C# 不支援非同步建構函式，因此建議的模式是以同步方式解析服務後使用非同步方法。</span><span class="sxs-lookup"><span data-stu-id="43413-366">C# does not support asynchronous constructors; therefore, the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="43413-367">避免直接在服務容器中儲存資料與設定。</span><span class="sxs-lookup"><span data-stu-id="43413-367">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="43413-368">例如，使用者的購物車通常不應該新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="43413-368">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="43413-369">組態應該使用[選項模式](xref:fundamentals/configuration/options)。</span><span class="sxs-lookup"><span data-stu-id="43413-369">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="43413-370">同樣地，請避免只存在以允許存取某個其他物件的「資料持有者」物件。</span><span class="sxs-lookup"><span data-stu-id="43413-370">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="43413-371">最好是透過 DI 要求實際項目。</span><span class="sxs-lookup"><span data-stu-id="43413-371">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="43413-372">避免以靜態方式存取服務 (例如，以靜態方式設定 [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) 型別以四處使用)。</span><span class="sxs-lookup"><span data-stu-id="43413-372">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) for use elsewhere).</span></span>

* <span data-ttu-id="43413-373">避免使用「服務定位器模式」\*\*。</span><span class="sxs-lookup"><span data-stu-id="43413-373">Avoid using the *service locator pattern*.</span></span> <span data-ttu-id="43413-374">例如，當您可以改用 DI 時，請勿叫用 <xref:System.IServiceProvider.GetService*> 來取得服務執行個體：</span><span class="sxs-lookup"><span data-stu-id="43413-374">For example, don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead:</span></span>

  <span data-ttu-id="43413-375">**正確**</span><span class="sxs-lookup"><span data-stu-id="43413-375">**Incorrect:**</span></span>

  ```csharp
  public class MyClass()
  {
      public void MyMethod()
      {
          var optionsMonitor = 
              _services.GetService<IOptionsMonitor<MyOptions>>();
          var option = optionsMonitor.CurrentValue.Option;

          ...
      }
  }
  ```

  <span data-ttu-id="43413-376">**正確**：</span><span class="sxs-lookup"><span data-stu-id="43413-376">**Correct**:</span></span>

  ```csharp
  public class MyClass
  {
      private readonly IOptionsMonitor<MyOptions> _optionsMonitor;

      public MyClass(IOptionsMonitor<MyOptions> optionsMonitor)
      {
          _optionsMonitor = optionsMonitor;
      }

      public void MyMethod()
      {
          var option = _optionsMonitor.CurrentValue.Option;

          ...
      }
  }
  ```

* <span data-ttu-id="43413-377">另一個要避免的服務定位器變化是插入在執行階段解析相依性的處理站。</span><span class="sxs-lookup"><span data-stu-id="43413-377">Another service locator variation to avoid is injecting a factory that resolves dependencies at runtime.</span></span> <span data-ttu-id="43413-378">這兩種做法都會混用[控制反轉](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion)策略。</span><span class="sxs-lookup"><span data-stu-id="43413-378">Both of these practices mix [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

* <span data-ttu-id="43413-379">避免以靜態方式存取 `HttpContext` (例如 [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext))。</span><span class="sxs-lookup"><span data-stu-id="43413-379">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span></span>

<span data-ttu-id="43413-380">就像所有的建議集，您可能會遇到需要忽略建議的情況。</span><span class="sxs-lookup"><span data-stu-id="43413-380">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="43413-381">例外狀況很少見&mdash;大部分是架構本身內的特殊案例。</span><span class="sxs-lookup"><span data-stu-id="43413-381">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="43413-382">DI 是靜態/全域物件存取模式的「替代」\*\* 選項。</span><span class="sxs-lookup"><span data-stu-id="43413-382">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="43413-383">如果您將 DI 與靜態物件存取混合，則可能無法實現 DI 的優點。</span><span class="sxs-lookup"><span data-stu-id="43413-383">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="43413-384">其他資源</span><span class="sxs-lookup"><span data-stu-id="43413-384">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="43413-385">在 ASP.NET Core 使用 Dependency Injection 撰寫簡潔的程式碼 (MSDN)</span><span class="sxs-lookup"><span data-stu-id="43413-385">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="43413-386">明確相依性準則</span><span class="sxs-lookup"><span data-stu-id="43413-386">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* <span data-ttu-id="43413-387">[逆轉控制容器和相依性插入模式 (Martin Fowler)](https://www.martinfowler.com/articles/injection.html) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="43413-387">[Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)](https://www.martinfowler.com/articles/injection.html)</span></span>
* <span data-ttu-id="43413-388">[How to register a service with multiple interfaces in ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/) (如何使用 ASP.NET Core DI 中的多個介面註冊服務)</span><span class="sxs-lookup"><span data-stu-id="43413-388">[How to register a service with multiple interfaces in ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="43413-389">ASP.NET Core 支援相依性插入 (DI) 軟體設計模式，這是用來在類別與其相依性之間達成[控制權反轉 (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) \(英文\) 的技術。</span><span class="sxs-lookup"><span data-stu-id="43413-389">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="43413-390">如需有關 MVC 控制器內相依性插入的特定詳細資訊，請參閱 <xref:mvc/controllers/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="43413-390">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="43413-391">[查看或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="43413-391">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="43413-392">相依性插入概觀</span><span class="sxs-lookup"><span data-stu-id="43413-392">Overview of dependency injection</span></span>

<span data-ttu-id="43413-393">「相依性」\*\* 是另一個物件所需的任何物件。</span><span class="sxs-lookup"><span data-stu-id="43413-393">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="43413-394">檢查具有 `WriteMessage` 方法的下列 `MyDependency` 物件，應用程式中的其他類別相依於此物件：</span><span class="sxs-lookup"><span data-stu-id="43413-394">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

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

<span data-ttu-id="43413-395">您可以建立 `MyDependency` 類別的執行個體，讓 `WriteMessage` 方法可供類別使用。</span><span class="sxs-lookup"><span data-stu-id="43413-395">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="43413-396">`MyDependency` 類別是 `IndexModel` 類別的相依性：</span><span class="sxs-lookup"><span data-stu-id="43413-396">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

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

<span data-ttu-id="43413-397">該類別會建立 `MyDependency` 執行個體並直接相依於該執行個體。</span><span class="sxs-lookup"><span data-stu-id="43413-397">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="43413-398">程式碼相依性 (例如前一個範例) 有問題，因此基於下列原因應該避免使用：</span><span class="sxs-lookup"><span data-stu-id="43413-398">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="43413-399">若要將 `MyDependency` 取代為不同的實作，必須修改該類別。</span><span class="sxs-lookup"><span data-stu-id="43413-399">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="43413-400">若 `MyDependency` 有相依性，那些相依性必須由該類別設定。</span><span class="sxs-lookup"><span data-stu-id="43413-400">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="43413-401">在具有多個相依於 `MyDependency` 之多個類別的大型專案中，設定程式碼在不同的應用程式之間會變得鬆散。</span><span class="sxs-lookup"><span data-stu-id="43413-401">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="43413-402">此實作難以進行單元測試。</span><span class="sxs-lookup"><span data-stu-id="43413-402">This implementation is difficult to unit test.</span></span> <span data-ttu-id="43413-403">應用程式應該使用模擬 (Mock) 或虛設常式 (Stub) `MyDependency` 類別，這在使用此方法時無法使用。</span><span class="sxs-lookup"><span data-stu-id="43413-403">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="43413-404">相依性插入可透過下列方式解決這些問題：</span><span class="sxs-lookup"><span data-stu-id="43413-404">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="43413-405">使用介面或基底類別來將相依性資訊抽象化。</span><span class="sxs-lookup"><span data-stu-id="43413-405">The use of an interface or base class to abstract the dependency implementation.</span></span>
* <span data-ttu-id="43413-406">在服務容器中註冊相依性。</span><span class="sxs-lookup"><span data-stu-id="43413-406">Registration of the dependency in a service container.</span></span> <span data-ttu-id="43413-407">ASP.NET Core 提供內建服務容器 <xref:System.IServiceProvider>。</span><span class="sxs-lookup"><span data-stu-id="43413-407">ASP.NET Core provides a built-in service container, <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="43413-408">服務會在應用程式的 `Startup.ConfigureServices` 方法中註冊。</span><span class="sxs-lookup"><span data-stu-id="43413-408">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="43413-409">將服務「插入」\*\* 到服務使用位置之類別的建構函式。</span><span class="sxs-lookup"><span data-stu-id="43413-409">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="43413-410">架構會負責建立相依性的執行個體，並在不再需要時將它捨棄。</span><span class="sxs-lookup"><span data-stu-id="43413-410">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="43413-411">在[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples)中，`IMyDependency` 介面定義了服務提供給應用程式的方法：</span><span class="sxs-lookup"><span data-stu-id="43413-411">In the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

<span data-ttu-id="43413-412">這個介面是由具象型別 `MyDependency` 所實作：</span><span class="sxs-lookup"><span data-stu-id="43413-412">This interface is implemented by a concrete type, `MyDependency`:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

<span data-ttu-id="43413-413">`MyDependency` 在其建構函式中會要求 <xref:Microsoft.Extensions.Logging.ILogger`1>。</span><span class="sxs-lookup"><span data-stu-id="43413-413">`MyDependency` requests an <xref:Microsoft.Extensions.Logging.ILogger`1> in its constructor.</span></span> <span data-ttu-id="43413-414">以鏈結方式使用相依性插入並非不尋常。</span><span class="sxs-lookup"><span data-stu-id="43413-414">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="43413-415">每個要求的相依性接著會要求其自己的相依性。</span><span class="sxs-lookup"><span data-stu-id="43413-415">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="43413-416">容器會解決圖形中的相依性，並傳回完全解析的服務。</span><span class="sxs-lookup"><span data-stu-id="43413-416">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="43413-417">必須先解析的相依性集合組通常稱為「相依性樹狀結構」**、「相依性圖形」** 或「物件圖形」\*\*。</span><span class="sxs-lookup"><span data-stu-id="43413-417">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="43413-418">`IMyDependency` 與 `ILogger<TCategoryName>` 必須在服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="43413-418">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="43413-419">`IMyDependency` 是在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="43413-419">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="43413-420">`ILogger<TCategoryName>` 是由記錄抽象基礎結構所註冊，所以它是預設由架構所註冊的[架構提供的服務](#framework-provided-services)。</span><span class="sxs-lookup"><span data-stu-id="43413-420">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="43413-421">容器會利用 [(泛型) 開放類型](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types) 解析 `ILogger<TCategoryName>`，讓您不需註冊每個 [(泛型) 建構的型別](/dotnet/csharp/language-reference/language-specification/types#constructed-types)：</span><span class="sxs-lookup"><span data-stu-id="43413-421">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<>), typeof(Logger<>));
```

<span data-ttu-id="43413-422">在範例應用程式中，`IMyDependency` 服務是使用具象型別 `MyDependency` 所註冊。</span><span class="sxs-lookup"><span data-stu-id="43413-422">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="43413-423">註冊會將服務的存留期範圍限制為單一要求的存留期。</span><span class="sxs-lookup"><span data-stu-id="43413-423">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="43413-424">將在此主題稍後將說明[服務存留期](#service-lifetimes)。</span><span class="sxs-lookup"><span data-stu-id="43413-424">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> <span data-ttu-id="43413-425">每個 `services.Add{SERVICE_NAME}` 擴充方法都會新增 (並且可能會設定) 服務。</span><span class="sxs-lookup"><span data-stu-id="43413-425">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="43413-426">例如，`services.AddMvc()` 會新增 Razor Pages 與 MVC 要求的服務。</span><span class="sxs-lookup"><span data-stu-id="43413-426">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="43413-427">建議應用程式遵循此慣例。</span><span class="sxs-lookup"><span data-stu-id="43413-427">We recommended that apps follow this convention.</span></span> <span data-ttu-id="43413-428">在 [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) 命名空間中放置擴充方法，以封裝服務註冊群組。</span><span class="sxs-lookup"><span data-stu-id="43413-428">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="43413-429">如果服務的建構函式需要[內建類型](/dotnet/csharp/language-reference/keywords/built-in-types-table)，例如 `string`，可以使用[組態](xref:fundamentals/configuration/index)或[選項模式](xref:fundamentals/configuration/options)插入該類型：</span><span class="sxs-lookup"><span data-stu-id="43413-429">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

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

<span data-ttu-id="43413-430">服務執行個體是透過服務使用所在之類別建構函式所要求，而且會指派給私人欄位。</span><span class="sxs-lookup"><span data-stu-id="43413-430">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="43413-431">該欄位會視需要用來存取整個類別中的服務。</span><span class="sxs-lookup"><span data-stu-id="43413-431">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="43413-432">在範例應用程式中，會要求 `IMyDependency` 執行個體並使用它來呼叫服務的 `WriteMessage` 方法：</span><span class="sxs-lookup"><span data-stu-id="43413-432">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="services-injected-into-startup"></a><span data-ttu-id="43413-433">插入至啟動的服務</span><span class="sxs-lookup"><span data-stu-id="43413-433">Services injected into Startup</span></span>

<span data-ttu-id="43413-434">使用泛型主機（ `Startup` <xref:Microsoft.Extensions.Hosting.IHostBuilder>）時，只有下列服務類型可以插入至此函式：</span><span class="sxs-lookup"><span data-stu-id="43413-434">Only the following service types can be injected into the `Startup` constructor when using the Generic Host (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span></span>

* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

<span data-ttu-id="43413-435">服務可以插入`Startup.Configure`：</span><span class="sxs-lookup"><span data-stu-id="43413-435">Services can be injected into `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> options)
{
    ...
}
```

<span data-ttu-id="43413-436">如需詳細資訊，請參閱<xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="43413-436">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="43413-437">架構提供的服務</span><span class="sxs-lookup"><span data-stu-id="43413-437">Framework-provided services</span></span>

<span data-ttu-id="43413-438">`Startup.ConfigureServices`方法負責定義應用程式所使用的服務，包括平臺功能，例如 Entity Framework Core 和 ASP.NET Core MVC。</span><span class="sxs-lookup"><span data-stu-id="43413-438">The `Startup.ConfigureServices` method is responsible for defining the services that the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="43413-439">一開始， `IServiceCollection`提供的`ConfigureServices`會根據[主機的設定方式](xref:fundamentals/index#host)，來擁有架構所定義的服務。</span><span class="sxs-lookup"><span data-stu-id="43413-439">Initially, the `IServiceCollection` provided to `ConfigureServices` has services defined by the framework depending on [how the host was configured](xref:fundamentals/index#host).</span></span> <span data-ttu-id="43413-440">以 ASP.NET Core 範本為基礎的應用程式，在架構中註冊數百項服務並不常見。</span><span class="sxs-lookup"><span data-stu-id="43413-440">It's not uncommon for an app based on an ASP.NET Core template to have hundreds of services registered by the framework.</span></span> <span data-ttu-id="43413-441">下表列出架構註冊服務的小型範例。</span><span class="sxs-lookup"><span data-stu-id="43413-441">A small sample of framework-registered services is listed in the following table.</span></span>

| <span data-ttu-id="43413-442">服務類型</span><span class="sxs-lookup"><span data-stu-id="43413-442">Service Type</span></span> | <span data-ttu-id="43413-443">存留期</span><span class="sxs-lookup"><span data-stu-id="43413-443">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="43413-444">暫時性</span><span class="sxs-lookup"><span data-stu-id="43413-444">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=fullName> | <span data-ttu-id="43413-445">單一</span><span class="sxs-lookup"><span data-stu-id="43413-445">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=fullName> | <span data-ttu-id="43413-446">單一</span><span class="sxs-lookup"><span data-stu-id="43413-446">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="43413-447">單一</span><span class="sxs-lookup"><span data-stu-id="43413-447">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="43413-448">暫時性</span><span class="sxs-lookup"><span data-stu-id="43413-448">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="43413-449">單一</span><span class="sxs-lookup"><span data-stu-id="43413-449">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="43413-450">暫時性</span><span class="sxs-lookup"><span data-stu-id="43413-450">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="43413-451">單一</span><span class="sxs-lookup"><span data-stu-id="43413-451">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="43413-452">單一</span><span class="sxs-lookup"><span data-stu-id="43413-452">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="43413-453">單一</span><span class="sxs-lookup"><span data-stu-id="43413-453">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="43413-454">暫時性</span><span class="sxs-lookup"><span data-stu-id="43413-454">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="43413-455">單一</span><span class="sxs-lookup"><span data-stu-id="43413-455">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="43413-456">單一</span><span class="sxs-lookup"><span data-stu-id="43413-456">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="43413-457">單一</span><span class="sxs-lookup"><span data-stu-id="43413-457">Singleton</span></span> |

## <a name="register-additional-services-with-extension-methods"></a><span data-ttu-id="43413-458">以擴充方法註冊其他服務</span><span class="sxs-lookup"><span data-stu-id="43413-458">Register additional services with extension methods</span></span>

<span data-ttu-id="43413-459">當可以使用服務集合擴充方法來註冊服務 (如果需要，也可以註冊其相依服務) 時，慣例是使用單一 `Add{SERVICE_NAME}` 擴充方法來註冊該服務要求的所有服務。</span><span class="sxs-lookup"><span data-stu-id="43413-459">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="43413-460">下列程式碼範例說明如何使用擴充方法[\<AddDbCoNtext TCoNtext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)和， <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>將其他服務新增至容器：</span><span class="sxs-lookup"><span data-stu-id="43413-460">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) and <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    ...
}
```

<span data-ttu-id="43413-461">如需詳細資訊，請參閱 API 文件中的 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> 類別。</span><span class="sxs-lookup"><span data-stu-id="43413-461">For more information, see the <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> class in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="43413-462">執行個體存留期</span><span class="sxs-lookup"><span data-stu-id="43413-462">Service lifetimes</span></span>

<span data-ttu-id="43413-463">為每個已註冊的服務選擇適當的存留期。</span><span class="sxs-lookup"><span data-stu-id="43413-463">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="43413-464">ASP.NET Core 服務可以使用下列存留期進行設定：</span><span class="sxs-lookup"><span data-stu-id="43413-464">ASP.NET Core services can be configured with the following lifetimes:</span></span>

### <a name="transient"></a><span data-ttu-id="43413-465">暫時性</span><span class="sxs-lookup"><span data-stu-id="43413-465">Transient</span></span>

<span data-ttu-id="43413-466">每次從服務容器要求暫時性存留期服務 (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) 時都會建立它們。</span><span class="sxs-lookup"><span data-stu-id="43413-466">Transient lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) are created each time they're requested from the service container.</span></span> <span data-ttu-id="43413-467">此存留期最適合用於輕量型的無狀態服務。</span><span class="sxs-lookup"><span data-stu-id="43413-467">This lifetime works best for lightweight, stateless services.</span></span>

### <a name="scoped"></a><span data-ttu-id="43413-468">具範圍</span><span class="sxs-lookup"><span data-stu-id="43413-468">Scoped</span></span>

<span data-ttu-id="43413-469">具範圍存留期服務 (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) 會在每次用戶端要求 (連線) 時建立一次。</span><span class="sxs-lookup"><span data-stu-id="43413-469">Scoped lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) are created once per client request (connection).</span></span>

> [!WARNING]
> <span data-ttu-id="43413-470">在中介軟體中使用具範圍服務時，請將該服務插入 `Invoke` 或 `InvokeAsync` 方法中。</span><span class="sxs-lookup"><span data-stu-id="43413-470">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="43413-471">不要透過函式[插入](xref:mvc/controllers/dependency-injection#constructor-injection)來插入，因為它會強制服務的行為就像 singleton 一樣。</span><span class="sxs-lookup"><span data-stu-id="43413-471">Don't inject via [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="43413-472">如需詳細資訊，請參閱<xref:fundamentals/middleware/write#per-request-middleware-dependencies>。</span><span class="sxs-lookup"><span data-stu-id="43413-472">For more information, see <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span></span>

### <a name="singleton"></a><span data-ttu-id="43413-473">單一</span><span class="sxs-lookup"><span data-stu-id="43413-473">Singleton</span></span>

<span data-ttu-id="43413-474">當第一次收到有關單一資料庫存留期服務的要求時 (或是當執行 `Startup.ConfigureServices` 且隨著服務註冊指定執行個體時)，即會建立單一資料庫存留期服務 (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>)。</span><span class="sxs-lookup"><span data-stu-id="43413-474">Singleton lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) are created the first time they're requested (or when `Startup.ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="43413-475">每個後續要求都會使用相同的執行個體。</span><span class="sxs-lookup"><span data-stu-id="43413-475">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="43413-476">若應用程式要求單一資料庫行為，建議您允許服務容器管理服務的存留期。</span><span class="sxs-lookup"><span data-stu-id="43413-476">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="43413-477">不要實作單一資料庫設計模式並為使用者提供可用來管理類別中物件存留期的程式碼。</span><span class="sxs-lookup"><span data-stu-id="43413-477">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="43413-478">從單一資料庫解析具範圍服務是非常危險的。</span><span class="sxs-lookup"><span data-stu-id="43413-478">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="43413-479">處理後續要求時，它可能會導致服務有不正確的狀態。</span><span class="sxs-lookup"><span data-stu-id="43413-479">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

## <a name="service-registration-methods"></a><span data-ttu-id="43413-480">服務註冊方法</span><span class="sxs-lookup"><span data-stu-id="43413-480">Service registration methods</span></span>

<span data-ttu-id="43413-481">服務註冊擴充方法提供在特定案例中很有用的多載。</span><span class="sxs-lookup"><span data-stu-id="43413-481">Service registration extension methods offer overloads that are useful in specific scenarios.</span></span>

| <span data-ttu-id="43413-482">方法</span><span class="sxs-lookup"><span data-stu-id="43413-482">Method</span></span> | <span data-ttu-id="43413-483">自動</span><span class="sxs-lookup"><span data-stu-id="43413-483">Automatic</span></span><br><span data-ttu-id="43413-484">物件</span><span class="sxs-lookup"><span data-stu-id="43413-484">object</span></span><br><span data-ttu-id="43413-485">處置</span><span class="sxs-lookup"><span data-stu-id="43413-485">disposal</span></span> | <span data-ttu-id="43413-486">多個</span><span class="sxs-lookup"><span data-stu-id="43413-486">Multiple</span></span><br><span data-ttu-id="43413-487">實作</span><span class="sxs-lookup"><span data-stu-id="43413-487">implementations</span></span> | <span data-ttu-id="43413-488">傳遞引數</span><span class="sxs-lookup"><span data-stu-id="43413-488">Pass args</span></span> |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br><span data-ttu-id="43413-489">範例：</span><span class="sxs-lookup"><span data-stu-id="43413-489">Example:</span></span><br>`services.AddSingleton<IMyDep, MyDep>();` | <span data-ttu-id="43413-490">是</span><span class="sxs-lookup"><span data-stu-id="43413-490">Yes</span></span> | <span data-ttu-id="43413-491">是</span><span class="sxs-lookup"><span data-stu-id="43413-491">Yes</span></span> | <span data-ttu-id="43413-492">否</span><span class="sxs-lookup"><span data-stu-id="43413-492">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br><span data-ttu-id="43413-493">範例：</span><span class="sxs-lookup"><span data-stu-id="43413-493">Examples:</span></span><br>`services.AddSingleton<IMyDep>(sp => new MyDep());`<br>`services.AddSingleton<IMyDep>(sp => new MyDep("A string!"));` | <span data-ttu-id="43413-494">是</span><span class="sxs-lookup"><span data-stu-id="43413-494">Yes</span></span> | <span data-ttu-id="43413-495">是</span><span class="sxs-lookup"><span data-stu-id="43413-495">Yes</span></span> | <span data-ttu-id="43413-496">是</span><span class="sxs-lookup"><span data-stu-id="43413-496">Yes</span></span> |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br><span data-ttu-id="43413-497">範例：</span><span class="sxs-lookup"><span data-stu-id="43413-497">Example:</span></span><br>`services.AddSingleton<MyDep>();` | <span data-ttu-id="43413-498">是</span><span class="sxs-lookup"><span data-stu-id="43413-498">Yes</span></span> | <span data-ttu-id="43413-499">否</span><span class="sxs-lookup"><span data-stu-id="43413-499">No</span></span> | <span data-ttu-id="43413-500">否</span><span class="sxs-lookup"><span data-stu-id="43413-500">No</span></span> |
| `AddSingleton<{SERVICE}>(new {IMPLEMENTATION})`<br><span data-ttu-id="43413-501">範例：</span><span class="sxs-lookup"><span data-stu-id="43413-501">Examples:</span></span><br>`services.AddSingleton<IMyDep>(new MyDep());`<br>`services.AddSingleton<IMyDep>(new MyDep("A string!"));` | <span data-ttu-id="43413-502">否</span><span class="sxs-lookup"><span data-stu-id="43413-502">No</span></span> | <span data-ttu-id="43413-503">是</span><span class="sxs-lookup"><span data-stu-id="43413-503">Yes</span></span> | <span data-ttu-id="43413-504">是</span><span class="sxs-lookup"><span data-stu-id="43413-504">Yes</span></span> |
| `AddSingleton(new {IMPLEMENTATION})`<br><span data-ttu-id="43413-505">範例：</span><span class="sxs-lookup"><span data-stu-id="43413-505">Examples:</span></span><br>`services.AddSingleton(new MyDep());`<br>`services.AddSingleton(new MyDep("A string!"));` | <span data-ttu-id="43413-506">否</span><span class="sxs-lookup"><span data-stu-id="43413-506">No</span></span> | <span data-ttu-id="43413-507">否</span><span class="sxs-lookup"><span data-stu-id="43413-507">No</span></span> | <span data-ttu-id="43413-508">是</span><span class="sxs-lookup"><span data-stu-id="43413-508">Yes</span></span> |

<span data-ttu-id="43413-509">如需類型處置的詳細資訊，請參閱[＜服務處置＞](#disposal-of-services)一節。</span><span class="sxs-lookup"><span data-stu-id="43413-509">For more information on type disposal, see the [Disposal of services](#disposal-of-services) section.</span></span> <span data-ttu-id="43413-510">多個實作的常見案例是[模擬測試類型](xref:test/integration-tests#inject-mock-services)。</span><span class="sxs-lookup"><span data-stu-id="43413-510">A common scenario for multiple implementations is [mocking types for testing](xref:test/integration-tests#inject-mock-services).</span></span>

<span data-ttu-id="43413-511">如果還未註冊任何實作，則 `TryAdd{LIFETIME}` 方法只會註冊服務。</span><span class="sxs-lookup"><span data-stu-id="43413-511">`TryAdd{LIFETIME}` methods only register the service if there isn't already an implementation registered.</span></span>

<span data-ttu-id="43413-512">在下列範例中，第一行會為 `IMyDependency` 註冊 `MyDependency`。</span><span class="sxs-lookup"><span data-stu-id="43413-512">In the following example, the first line registers `MyDependency` for `IMyDependency`.</span></span> <span data-ttu-id="43413-513">第二行則沒有任何作用，因為 `IMyDependency` 已具有註冊的實作：</span><span class="sxs-lookup"><span data-stu-id="43413-513">The second line has no effect because `IMyDependency` already has a registered implementation:</span></span>

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

<span data-ttu-id="43413-514">如需詳細資訊，請參閱</span><span class="sxs-lookup"><span data-stu-id="43413-514">For more information, see:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

<span data-ttu-id="43413-515">如果還沒有「相同類型的」\*\* 實作，則 [TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) 方法只會註冊服務。</span><span class="sxs-lookup"><span data-stu-id="43413-515">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) methods only register the service if there isn't already an implementation *of the same type*.</span></span> <span data-ttu-id="43413-516">多個服務會透過 `IEnumerable<{SERVICE}>` 解析。</span><span class="sxs-lookup"><span data-stu-id="43413-516">Multiple services are resolved via `IEnumerable<{SERVICE}>`.</span></span> <span data-ttu-id="43413-517">註冊服務時，如果尚未新增相同類型的執行個體，則開發人員只會希望新增執行個體。</span><span class="sxs-lookup"><span data-stu-id="43413-517">When registering services, the developer only wants to add an instance if one of the same type hasn't already been added.</span></span> <span data-ttu-id="43413-518">程式庫作者通常會使用此方法來避免註冊容器中某個執行個體的兩個複本。</span><span class="sxs-lookup"><span data-stu-id="43413-518">Generally, this method is used by library authors to avoid registering two copies of an instance in the container.</span></span>

<span data-ttu-id="43413-519">在下列範例中，第一行會為 `IMyDep1` 註冊 `MyDep`。</span><span class="sxs-lookup"><span data-stu-id="43413-519">In the following example, the first line registers `MyDep` for `IMyDep1`.</span></span> <span data-ttu-id="43413-520">第二行會為 `IMyDep2` 註冊 `MyDep`。</span><span class="sxs-lookup"><span data-stu-id="43413-520">The second line registers `MyDep` for `IMyDep2`.</span></span> <span data-ttu-id="43413-521">第三行則沒有任何作用，因為 `IMyDep1` 已具有 `MyDep` 的已註冊實作：</span><span class="sxs-lookup"><span data-stu-id="43413-521">The third line has no effect because `IMyDep1` already has a registered implementation of `MyDep`:</span></span>

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a><span data-ttu-id="43413-522">建構函式插入行為</span><span class="sxs-lookup"><span data-stu-id="43413-522">Constructor injection behavior</span></span>

<span data-ttu-id="43413-523">服務可以透過兩個機制來解析：</span><span class="sxs-lookup"><span data-stu-id="43413-523">Services can be resolved by two mechanisms:</span></span>

* <xref:System.IServiceProvider>
* <span data-ttu-id="43413-524"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities>&ndash;允許在相依性插入容器中，不需要服務註冊即可建立物件。</span><span class="sxs-lookup"><span data-stu-id="43413-524"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="43413-525">搭配使用者面向抽象 (例如標籤協助程式、MVC 控制器與模型繫結器) 使用 `ActivatorUtilities`。</span><span class="sxs-lookup"><span data-stu-id="43413-525">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="43413-526">建構函式可以接受不是由相依性插入提供的引數，但引數必須指派預設值。</span><span class="sxs-lookup"><span data-stu-id="43413-526">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="43413-527">`IServiceProvider`當服務由或`ActivatorUtilities`解析時，程式的[插入](xref:mvc/controllers/dependency-injection#constructor-injection)需要*公用*的函式。</span><span class="sxs-lookup"><span data-stu-id="43413-527">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) requires a *public* constructor.</span></span>

<span data-ttu-id="43413-528">當服務由`ActivatorUtilities`解析時，程式的[插入](xref:mvc/controllers/dependency-injection#constructor-injection)只需要有一個適用的函式。</span><span class="sxs-lookup"><span data-stu-id="43413-528">When services are resolved by `ActivatorUtilities`, [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) requires that only one applicable constructor exists.</span></span> <span data-ttu-id="43413-529">支援建構函式多載，但只能有一個多載存在，其引數可以藉由相依性插入而完成。</span><span class="sxs-lookup"><span data-stu-id="43413-529">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="43413-530">Entity Framework 內容</span><span class="sxs-lookup"><span data-stu-id="43413-530">Entity Framework contexts</span></span>

<span data-ttu-id="43413-531">因為一般會將 Web 應用程式資料庫作業範圍設定為用戶端要求，所以通常會使用[具範圍存留期](#service-lifetimes)將 Entity Framework 內容新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="43413-531">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="43413-532">註冊資料庫內容時，如果 [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) 多載未指定留存期，則會將範圍設定為預設存留期。</span><span class="sxs-lookup"><span data-stu-id="43413-532">The default lifetime is scoped if a lifetime isn't specified by an [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) overload when registering the database context.</span></span> <span data-ttu-id="43413-533">指定存留期的服務不應該使用存留期比服務還短的資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="43413-533">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="43413-534">留期和註冊選項</span><span class="sxs-lookup"><span data-stu-id="43413-534">Lifetime and registration options</span></span>

<span data-ttu-id="43413-535">為了示範這些存留期和註冊選項之間的差異，請考慮下列介面，以具有唯一識別碼 `OperationId` 的「作業」代表工作。</span><span class="sxs-lookup"><span data-stu-id="43413-535">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="43413-536">視如何針對下列介面設定作業服務存留期而定，當類別要求時，容器會提供相同或不同的服務執行個體：</span><span class="sxs-lookup"><span data-stu-id="43413-536">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

<span data-ttu-id="43413-537">介面是在 `Operation` 類別中實作。</span><span class="sxs-lookup"><span data-stu-id="43413-537">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="43413-538">若未提供 GUID，`Operation` 建構函式會產生 GUID：</span><span class="sxs-lookup"><span data-stu-id="43413-538">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

<span data-ttu-id="43413-539">會註冊相依於每種其他 `Operation` 類型的 `OperationService`。</span><span class="sxs-lookup"><span data-stu-id="43413-539">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="43413-540">當透過相依性插入要求 `OperationService` 時，它會根據相依服務的存留期擷取每個服務的新執行個體或現有執行個體。</span><span class="sxs-lookup"><span data-stu-id="43413-540">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="43413-541">當因收到來自容器的要求而建立暫時性服務時，`IOperationTransient` 服務的 `OperationId` 會與 `OperationService` 的 `OperationId` 不同。</span><span class="sxs-lookup"><span data-stu-id="43413-541">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="43413-542">`OperationService` 會收到 `IOperationTransient` 類別的新執行個體。</span><span class="sxs-lookup"><span data-stu-id="43413-542">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="43413-543">新執行個體會產生不同的 `OperationId`。</span><span class="sxs-lookup"><span data-stu-id="43413-543">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="43413-544">當因用戶端要求而建立具範圍的服務時，`IOperationScoped` 服務與用戶端要求中 `OperationService` 的 `OperationId` 皆相同。</span><span class="sxs-lookup"><span data-stu-id="43413-544">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="43413-545">在不同的用戶端要求中，兩個服務的 `OperationId` 值都不同。</span><span class="sxs-lookup"><span data-stu-id="43413-545">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="43413-546">當建立一次 singleton 與 singleton 執行個體服務，並在所有用戶端要求與所有服務中使用時，`OperationId` 在所有服務要求中會固定不變。</span><span class="sxs-lookup"><span data-stu-id="43413-546">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

<span data-ttu-id="43413-547">在 `Startup.ConfigureServices` 中，每個類型都會根據其具名存留期新增至容器：</span><span class="sxs-lookup"><span data-stu-id="43413-547">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

<span data-ttu-id="43413-548">`IOperationSingletonInstance` 服務使用具有 `Guid.Empty` 已知識別碼的特定執行個體。</span><span class="sxs-lookup"><span data-stu-id="43413-548">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="43413-549">很明顯此型別是使用中 (其 GUID 是所有區域)。</span><span class="sxs-lookup"><span data-stu-id="43413-549">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="43413-550">範例應用程式示範個別要求內與個別要求之間的物件存留期。</span><span class="sxs-lookup"><span data-stu-id="43413-550">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="43413-551">範例應用程式的 `IndexModel` 會要求每種 `IOperation` 型別與 `OperationService`。</span><span class="sxs-lookup"><span data-stu-id="43413-551">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="43413-552">頁面接著會透過屬性指派來顯示所有頁面模型類別與服務的 `OperationId` 值：</span><span class="sxs-lookup"><span data-stu-id="43413-552">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

<span data-ttu-id="43413-553">下面兩個輸出顯示兩個要求的結果：</span><span class="sxs-lookup"><span data-stu-id="43413-553">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="43413-554">**第一個要求：**</span><span class="sxs-lookup"><span data-stu-id="43413-554">**First request:**</span></span>

<span data-ttu-id="43413-555">控制器作業：</span><span class="sxs-lookup"><span data-stu-id="43413-555">Controller operations:</span></span>

<span data-ttu-id="43413-556">暫時性： d233e165-f417-469b-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="43413-556">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="43413-557">具範圍： 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="43413-557">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="43413-558">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="43413-558">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="43413-559">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="43413-559">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="43413-560">`OperationService` 作業：</span><span class="sxs-lookup"><span data-stu-id="43413-560">`OperationService` operations:</span></span>

<span data-ttu-id="43413-561">暫時性： c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="43413-561">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="43413-562">具範圍： 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="43413-562">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="43413-563">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="43413-563">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="43413-564">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="43413-564">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="43413-565">**:第二個要求：**</span><span class="sxs-lookup"><span data-stu-id="43413-565">**Second request:**</span></span>

<span data-ttu-id="43413-566">控制器作業：</span><span class="sxs-lookup"><span data-stu-id="43413-566">Controller operations:</span></span>

<span data-ttu-id="43413-567">暫時性： b63bd538-0a37-4ff1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="43413-567">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="43413-568">具範圍： 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="43413-568">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="43413-569">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="43413-569">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="43413-570">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="43413-570">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="43413-571">`OperationService` 作業：</span><span class="sxs-lookup"><span data-stu-id="43413-571">`OperationService` operations:</span></span>

<span data-ttu-id="43413-572">暫時性： c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="43413-572">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="43413-573">具範圍： 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="43413-573">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="43413-574">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="43413-574">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="43413-575">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="43413-575">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="43413-576">觀察哪些 `OperationId` 值在要求內以及要求之間不同：</span><span class="sxs-lookup"><span data-stu-id="43413-576">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="43413-577">「暫時性」\*\* 物件一律不同。</span><span class="sxs-lookup"><span data-stu-id="43413-577">*Transient* objects are always different.</span></span> <span data-ttu-id="43413-578">第一個與第二個用戶端要求的暫時性 `OperationId` 值在兩個 `OperationService` 作業之間與各用戶端要求之間都不同。</span><span class="sxs-lookup"><span data-stu-id="43413-578">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="43413-579">新執行個體會提供給每個服務要求以及用戶端要求。</span><span class="sxs-lookup"><span data-stu-id="43413-579">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="43413-580">「具範圍」\*\* 物件在同一個用戶端要求內皆相同，但在不同的用戶端要求之間則不同。</span><span class="sxs-lookup"><span data-stu-id="43413-580">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="43413-581">*Singleton*不論是否在中`Operation` `Startup.ConfigureServices`提供實例，每個物件和每個要求的單一物件都是相同的。</span><span class="sxs-lookup"><span data-stu-id="43413-581">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `Startup.ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="43413-582">從主要呼叫服務</span><span class="sxs-lookup"><span data-stu-id="43413-582">Call services from main</span></span>

<span data-ttu-id="43413-583">使用 [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) 建立 <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>，以解析應用程式範圍中的範圍服務。</span><span class="sxs-lookup"><span data-stu-id="43413-583">Create an <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="43413-584">此法可用於在開機時存取範圍服務，以執行初始化工作。</span><span class="sxs-lookup"><span data-stu-id="43413-584">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="43413-585">下列範例示範如何在 `Program.Main` 中取得 `MyScopedService`：</span><span class="sxs-lookup"><span data-stu-id="43413-585">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;

public class Program
{
    public static async Task Main(string[] args)
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
    
        await host.RunAsync();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

## <a name="scope-validation"></a><span data-ttu-id="43413-586">範圍驗證</span><span class="sxs-lookup"><span data-stu-id="43413-586">Scope validation</span></span>

<span data-ttu-id="43413-587">當應用程式在開發環境中執行時，預設服務提供者會執行檢查以確認：</span><span class="sxs-lookup"><span data-stu-id="43413-587">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="43413-588">範圍服務不是直接或間接由開機服務提供者解析。</span><span class="sxs-lookup"><span data-stu-id="43413-588">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="43413-589">範圍服務不是直接或間接插入至單一服務。</span><span class="sxs-lookup"><span data-stu-id="43413-589">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="43413-590">根服務提供者會在呼叫 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> 時建立。</span><span class="sxs-lookup"><span data-stu-id="43413-590">The root service provider is created when <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> is called.</span></span> <span data-ttu-id="43413-591">當提供者啟動應用程式時，根服務提供者的存留期與應用程式/伺服器的存留期一致，並會在應用程式關閉時處置。</span><span class="sxs-lookup"><span data-stu-id="43413-591">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="43413-592">範圍服務會由建立這些服務的容器處置。</span><span class="sxs-lookup"><span data-stu-id="43413-592">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="43413-593">若是在根容器中建立範圍服務，因為當應用程式/伺服器關機時，服務只會由根容器處理，所以服務的存留期會提升為單一服務等級。</span><span class="sxs-lookup"><span data-stu-id="43413-593">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="43413-594">當呼叫 `BuildServiceProvider` 時，驗證服務範圍會攔截到這些情況。</span><span class="sxs-lookup"><span data-stu-id="43413-594">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="43413-595">如需詳細資訊，請參閱<xref:fundamentals/host/web-host#scope-validation>。</span><span class="sxs-lookup"><span data-stu-id="43413-595">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="43413-596">要求服務</span><span class="sxs-lookup"><span data-stu-id="43413-596">Request Services</span></span>

<span data-ttu-id="43413-597">來自 `HttpContext`，在 ASP.NET Core 要求內提供的服務是透過 [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) 集合公開。</span><span class="sxs-lookup"><span data-stu-id="43413-597">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) collection.</span></span>

<span data-ttu-id="43413-598">要求服務代表您在應用程式中設定和要求的服務。</span><span class="sxs-lookup"><span data-stu-id="43413-598">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="43413-599">當物件指定相依性時，這些會由在 `RequestServices` 中找到的類型來滿足，而非 `ApplicationServices`。</span><span class="sxs-lookup"><span data-stu-id="43413-599">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="43413-600">一般而言，應用程式不應該直接使用這些屬性。</span><span class="sxs-lookup"><span data-stu-id="43413-600">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="43413-601">相反地，透過類別建構函式要求類別所需的型別，以及允許架構插入相依性。</span><span class="sxs-lookup"><span data-stu-id="43413-601">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="43413-602">這會產生容易測試的類別。</span><span class="sxs-lookup"><span data-stu-id="43413-602">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="43413-603">偏好要求相依性作為建構函式參數，而不要存取 `RequestServices` 集合。</span><span class="sxs-lookup"><span data-stu-id="43413-603">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="43413-604">針對相依性插入設計服務</span><span class="sxs-lookup"><span data-stu-id="43413-604">Design services for dependency injection</span></span>

<span data-ttu-id="43413-605">最佳做法是：</span><span class="sxs-lookup"><span data-stu-id="43413-605">Best practices are to:</span></span>

* <span data-ttu-id="43413-606">設計服務以使用相依性插入來取得其相依性。</span><span class="sxs-lookup"><span data-stu-id="43413-606">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="43413-607">避免具狀態的靜態類別和成員。</span><span class="sxs-lookup"><span data-stu-id="43413-607">Avoid stateful, static classes and members.</span></span> <span data-ttu-id="43413-608">將應用程式設計成使用單一服務，以避免建立全域狀態。</span><span class="sxs-lookup"><span data-stu-id="43413-608">Design apps to use singleton services instead, which avoid creating global state.</span></span>
* <span data-ttu-id="43413-609">避免直接在服務內具現化相依類別。</span><span class="sxs-lookup"><span data-stu-id="43413-609">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="43413-610">直接具現化會將程式碼耦合到特定實作。</span><span class="sxs-lookup"><span data-stu-id="43413-610">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="43413-611">讓應用程式類別維持在小型、情況良好且可輕鬆測試的狀態。</span><span class="sxs-lookup"><span data-stu-id="43413-611">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="43413-612">若類別有太多插入的相依性，這通常表示類別有太多責任而且正違反[單一責任原則 (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="43413-612">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="43413-613">將類別負責的某些部分移到新的類別，以嘗試重構類別。</span><span class="sxs-lookup"><span data-stu-id="43413-613">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="43413-614">請記住， Razor頁面頁面模型類別和 MVC 控制器類別應該專注于 UI 考慮。</span><span class="sxs-lookup"><span data-stu-id="43413-614">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="43413-615">商務規則和資料存取實作詳細資料應該保存在適合這些[分開考量](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) (Separation of Concerns) 類別中。</span><span class="sxs-lookup"><span data-stu-id="43413-615">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="43413-616">處置服務</span><span class="sxs-lookup"><span data-stu-id="43413-616">Disposal of services</span></span>

<span data-ttu-id="43413-617">容器會為它建立的 <xref:System.IDisposable> 類型呼叫 <xref:System.IDisposable.Dispose*>。</span><span class="sxs-lookup"><span data-stu-id="43413-617">The container calls <xref:System.IDisposable.Dispose*> for the <xref:System.IDisposable> types it creates.</span></span> <span data-ttu-id="43413-618">若執行個體由使用者程式碼新增到容器中，它將不會自動處置。</span><span class="sxs-lookup"><span data-stu-id="43413-618">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

<span data-ttu-id="43413-619">在下列範例中，服務會建立服務容器並自動處置：</span><span class="sxs-lookup"><span data-stu-id="43413-619">In the following example, the services are created by the service container and disposed automatically:</span></span>

```csharp
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}

public interface IService3 {}
public class Service3 : IService3, IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<IService3>(sp => new Service3());
}
```

<span data-ttu-id="43413-620">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="43413-620">In the following example:</span></span>

* <span data-ttu-id="43413-621">服務容器不會建立服務實例。</span><span class="sxs-lookup"><span data-stu-id="43413-621">The service instances aren't created by the service container.</span></span>
* <span data-ttu-id="43413-622">架構不知道預期的服務存留期。</span><span class="sxs-lookup"><span data-stu-id="43413-622">The intended service lifetimes aren't known by the framework.</span></span>
* <span data-ttu-id="43413-623">架構不會自動處置服務。</span><span class="sxs-lookup"><span data-stu-id="43413-623">The framework doesn't dispose of the services automatically.</span></span>
* <span data-ttu-id="43413-624">如果服務未在開發人員程式碼中明確處置，它們會持續存在，直到應用程式關閉為止。</span><span class="sxs-lookup"><span data-stu-id="43413-624">If the services aren't explicitly disposed in developer code, they persist until the app shuts down.</span></span>

```csharp
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<Service1>(new Service1());
    services.AddSingleton(new Service2());
}
```

## <a name="default-service-container-replacement"></a><span data-ttu-id="43413-625">預設服務容器取代</span><span class="sxs-lookup"><span data-stu-id="43413-625">Default service container replacement</span></span>

<span data-ttu-id="43413-626">內建的服務容器是設計用來滿足架構和大部分取用者應用程式的需求。</span><span class="sxs-lookup"><span data-stu-id="43413-626">The built-in service container is designed to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="43413-627">我們建議使用內建容器，除非您需要內建容器不支援的特定功能，例如：</span><span class="sxs-lookup"><span data-stu-id="43413-627">We recommend using the built-in container unless you need a specific feature that the built-in container doesn't support, such as:</span></span>

* <span data-ttu-id="43413-628">屬性插入</span><span class="sxs-lookup"><span data-stu-id="43413-628">Property injection</span></span>
* <span data-ttu-id="43413-629">根據名稱插入</span><span class="sxs-lookup"><span data-stu-id="43413-629">Injection based on name</span></span>
* <span data-ttu-id="43413-630">子容器</span><span class="sxs-lookup"><span data-stu-id="43413-630">Child containers</span></span>
* <span data-ttu-id="43413-631">自訂生命週期管理</span><span class="sxs-lookup"><span data-stu-id="43413-631">Custom lifetime management</span></span>
* <span data-ttu-id="43413-632">`Func<T>` 支援延遲初始設定</span><span class="sxs-lookup"><span data-stu-id="43413-632">`Func<T>` support for lazy initialization</span></span>
* <span data-ttu-id="43413-633">以慣例為基礎的註冊</span><span class="sxs-lookup"><span data-stu-id="43413-633">Convention-based registration</span></span>

<span data-ttu-id="43413-634">下列協力廠商容器可以與 ASP.NET Core 應用程式搭配使用：</span><span class="sxs-lookup"><span data-stu-id="43413-634">The following 3rd party containers can be used with ASP.NET Core apps:</span></span>

* [<span data-ttu-id="43413-635">Autofac</span><span class="sxs-lookup"><span data-stu-id="43413-635">Autofac</span></span>](https://autofac.readthedocs.io/en/latest/integration/aspnetcore.html)
* [<span data-ttu-id="43413-636">DryIoc</span><span class="sxs-lookup"><span data-stu-id="43413-636">DryIoc</span></span>](https://www.nuget.org/packages/DryIoc.Microsoft.DependencyInjection)
* [<span data-ttu-id="43413-637">Grace</span><span class="sxs-lookup"><span data-stu-id="43413-637">Grace</span></span>](https://www.nuget.org/packages/Grace.DependencyInjection.Extensions)
* [<span data-ttu-id="43413-638">LightInject</span><span class="sxs-lookup"><span data-stu-id="43413-638">LightInject</span></span>](https://github.com/seesharper/LightInject.Microsoft.DependencyInjection)
* [<span data-ttu-id="43413-639">拉馬爾</span><span class="sxs-lookup"><span data-stu-id="43413-639">Lamar</span></span>](https://jasperfx.github.io/lamar/)
* [<span data-ttu-id="43413-640">Stashbox</span><span class="sxs-lookup"><span data-stu-id="43413-640">Stashbox</span></span>](https://github.com/z4kn4fein/stashbox-extensions-dependencyinjection)
* [<span data-ttu-id="43413-641">Unity</span><span class="sxs-lookup"><span data-stu-id="43413-641">Unity</span></span>](https://www.nuget.org/packages/Unity.Microsoft.DependencyInjection)

### <a name="thread-safety"></a><span data-ttu-id="43413-642">執行緒安全</span><span class="sxs-lookup"><span data-stu-id="43413-642">Thread safety</span></span>

<span data-ttu-id="43413-643">建立具備執行緒安全性的 singleton 服務。</span><span class="sxs-lookup"><span data-stu-id="43413-643">Create thread-safe singleton services.</span></span> <span data-ttu-id="43413-644">如果 singleton 服務相依於暫時性服務，暫時性服務可能也需要具備執行緒安全性，取決於 singleton 如何使用它。</span><span class="sxs-lookup"><span data-stu-id="43413-644">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="43413-645">單一服務的 Factory 方法 (例如 [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*) 的第二個引數) 不需要是安全執行緒。</span><span class="sxs-lookup"><span data-stu-id="43413-645">The factory method of single service, such as the second argument to [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), doesn't need to be thread-safe.</span></span> <span data-ttu-id="43413-646">就像型別 (`static`) 建構函式一樣，它一定會被單一執行緒呼叫一次。</span><span class="sxs-lookup"><span data-stu-id="43413-646">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="43413-647">建議</span><span class="sxs-lookup"><span data-stu-id="43413-647">Recommendations</span></span>

* <span data-ttu-id="43413-648">不支援以 `async/await` 與 `Task` 為基礎的服務解析。</span><span class="sxs-lookup"><span data-stu-id="43413-648">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="43413-649">C# 不支援非同步建構函式，因此建議的模式是以同步方式解析服務後使用非同步方法。</span><span class="sxs-lookup"><span data-stu-id="43413-649">C# does not support asynchronous constructors; therefore, the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="43413-650">避免直接在服務容器中儲存資料與設定。</span><span class="sxs-lookup"><span data-stu-id="43413-650">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="43413-651">例如，使用者的購物車通常不應該新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="43413-651">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="43413-652">組態應該使用[選項模式](xref:fundamentals/configuration/options)。</span><span class="sxs-lookup"><span data-stu-id="43413-652">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="43413-653">同樣地，請避免只存在以允許存取某個其他物件的「資料持有者」物件。</span><span class="sxs-lookup"><span data-stu-id="43413-653">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="43413-654">最好是透過 DI 要求實際項目。</span><span class="sxs-lookup"><span data-stu-id="43413-654">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="43413-655">避免以靜態方式存取服務 (例如，以靜態方式設定 [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) 型別以四處使用)。</span><span class="sxs-lookup"><span data-stu-id="43413-655">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) for use elsewhere).</span></span>

* <span data-ttu-id="43413-656">避免使用*服務定位器模式*，這會混用控制策略的[反轉](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion)。</span><span class="sxs-lookup"><span data-stu-id="43413-656">Avoid using the *service locator pattern*, which mixes [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

  * <span data-ttu-id="43413-657">當您<xref:System.IServiceProvider.GetService*>可以改用 DI 時，請勿叫用來取得服務實例：</span><span class="sxs-lookup"><span data-stu-id="43413-657">Don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead:</span></span>

    <span data-ttu-id="43413-658">**正確**</span><span class="sxs-lookup"><span data-stu-id="43413-658">**Incorrect:**</span></span>

    ```csharp
    public class MyClass()
    {
        public void MyMethod()
        {
            var optionsMonitor = 
                _services.GetService<IOptionsMonitor<MyOptions>>();
            var option = optionsMonitor.CurrentValue.Option;

            ...
        }
    }
    ```

    <span data-ttu-id="43413-659">**正確**：</span><span class="sxs-lookup"><span data-stu-id="43413-659">**Correct**:</span></span>

    ```csharp
    public class MyClass
    {
        private readonly IOptionsMonitor<MyOptions> _optionsMonitor;

        public MyClass(IOptionsMonitor<MyOptions> optionsMonitor)
        {
            _optionsMonitor = optionsMonitor;
        }

        public void MyMethod()
        {
            var option = _optionsMonitor.CurrentValue.Option;

            ...
        }
    }
    ```

  * <span data-ttu-id="43413-660">避免在使用<xref:System.IServiceProvider.GetService*>時，插入用來解析相依性的 factory。</span><span class="sxs-lookup"><span data-stu-id="43413-660">Avoid injecting a factory that resolves dependencies at runtime using <xref:System.IServiceProvider.GetService*>.</span></span>

* <span data-ttu-id="43413-661">避免以靜態方式存取 `HttpContext` (例如 [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext))。</span><span class="sxs-lookup"><span data-stu-id="43413-661">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span></span>

<span data-ttu-id="43413-662">就像所有的建議集，您可能會遇到需要忽略建議的情況。</span><span class="sxs-lookup"><span data-stu-id="43413-662">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="43413-663">例外狀況很少見&mdash;大部分是架構本身內的特殊案例。</span><span class="sxs-lookup"><span data-stu-id="43413-663">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="43413-664">DI 是靜態/全域物件存取模式的「替代」\*\* 選項。</span><span class="sxs-lookup"><span data-stu-id="43413-664">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="43413-665">如果您將 DI 與靜態物件存取混合，則可能無法實現 DI 的優點。</span><span class="sxs-lookup"><span data-stu-id="43413-665">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="43413-666">其他資源</span><span class="sxs-lookup"><span data-stu-id="43413-666">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="43413-667">在 ASP.NET Core 使用 Dependency Injection 撰寫簡潔的程式碼 (MSDN)</span><span class="sxs-lookup"><span data-stu-id="43413-667">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="43413-668">明確相依性準則</span><span class="sxs-lookup"><span data-stu-id="43413-668">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* <span data-ttu-id="43413-669">[逆轉控制容器和相依性插入模式 (Martin Fowler)](https://www.martinfowler.com/articles/injection.html) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="43413-669">[Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)](https://www.martinfowler.com/articles/injection.html)</span></span>
* <span data-ttu-id="43413-670">[How to register a service with multiple interfaces in ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/) (如何使用 ASP.NET Core DI 中的多個介面註冊服務)</span><span class="sxs-lookup"><span data-stu-id="43413-670">[How to register a service with multiple interfaces in ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)</span></span>

::: moniker-end
