---
title: .NET Core 中的相依性插入
author: rick-anderson
description: 了解 ASP.NET Core 如何實作相依性插入以及如何使用它。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/dependency-injection
ms.openlocfilehash: ddb583f69758055500ff63960f469c1cea44c77e
ms.sourcegitcommit: 490434a700ba8c5ed24d849bd99d8489858538e3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/19/2020
ms.locfileid: "85102599"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="8eb6a-103">.NET Core 中的相依性插入</span><span class="sxs-lookup"><span data-stu-id="8eb6a-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="8eb6a-104">作者： [Steve Smith](https://ardalis.com/)、 [Scott Addie](https://scottaddie.com)和[Brandon Dahler](https://github.com/brandondahler)</span><span class="sxs-lookup"><span data-stu-id="8eb6a-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Brandon Dahler](https://github.com/brandondahler)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8eb6a-105">ASP.NET Core 支援相依性插入 (DI) 軟體設計模式，這是用來在類別與其相依性之間達成[控制權反轉 (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) \(英文\) 的技術。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="8eb6a-106">如需有關 MVC 控制器內相依性插入的特定詳細資訊，請參閱 <xref:mvc/controllers/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="8eb6a-107">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="8eb6a-107">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="8eb6a-108">相依性插入概觀</span><span class="sxs-lookup"><span data-stu-id="8eb6a-108">Overview of dependency injection</span></span>

<span data-ttu-id="8eb6a-109">「相依性」\*\* 是另一個物件所需的任何物件。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="8eb6a-110">檢查具有 `WriteMessage` 方法的下列 `MyDependency` 物件，應用程式中的其他類別相依於此物件：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

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

<span data-ttu-id="8eb6a-111">您可以建立 `MyDependency` 類別的執行個體，讓 `WriteMessage` 方法可供類別使用。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="8eb6a-112">`MyDependency` 類別是 `IndexModel` 類別的相依性：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

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

<span data-ttu-id="8eb6a-113">該類別會建立 `MyDependency` 執行個體並直接相依於該執行個體。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-113">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="8eb6a-114">程式碼相依性 (例如前一個範例) 有問題，因此基於下列原因應該避免使用：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-114">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="8eb6a-115">若要將 `MyDependency` 取代為不同的實作，必須修改該類別。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-115">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="8eb6a-116">若 `MyDependency` 有相依性，那些相依性必須由該類別設定。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-116">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="8eb6a-117">在具有多個相依於 `MyDependency` 之多個類別的大型專案中，設定程式碼在不同的應用程式之間會變得鬆散。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-117">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="8eb6a-118">此實作難以進行單元測試。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-118">This implementation is difficult to unit test.</span></span> <span data-ttu-id="8eb6a-119">應用程式應該使用模擬 (Mock) 或虛設常式 (Stub) `MyDependency` 類別，這在使用此方法時無法使用。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-119">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="8eb6a-120">相依性插入可透過下列方式解決這些問題：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-120">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="8eb6a-121">使用介面或基底類別來將相依性資訊抽象化。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-121">The use of an interface or base class to abstract the dependency implementation.</span></span>
* <span data-ttu-id="8eb6a-122">在服務容器中註冊相依性。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-122">Registration of the dependency in a service container.</span></span> <span data-ttu-id="8eb6a-123">ASP.NET Core 提供內建服務容器 <xref:System.IServiceProvider>。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-123">ASP.NET Core provides a built-in service container, <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="8eb6a-124">服務會在應用程式的 `Startup.ConfigureServices` 方法中註冊。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-124">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="8eb6a-125">將服務「插入」\*\* 到服務使用位置之類別的建構函式。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-125">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="8eb6a-126">架構會負責建立相依性的執行個體，並在不再需要時將它捨棄。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-126">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="8eb6a-127">在[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples)中，`IMyDependency` 介面定義了服務提供給應用程式的方法：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-127">In the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

<span data-ttu-id="8eb6a-128">這個介面是由具象型別 `MyDependency` 所實作：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-128">This interface is implemented by a concrete type, `MyDependency`:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

<span data-ttu-id="8eb6a-129">`MyDependency` 在其建構函式中會要求 <xref:Microsoft.Extensions.Logging.ILogger`1>。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-129">`MyDependency` requests an <xref:Microsoft.Extensions.Logging.ILogger`1> in its constructor.</span></span> <span data-ttu-id="8eb6a-130">以鏈結方式使用相依性插入並非不尋常。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-130">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="8eb6a-131">每個要求的相依性接著會要求其自己的相依性。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-131">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="8eb6a-132">容器會解決圖形中的相依性，並傳回完全解析的服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-132">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="8eb6a-133">必須先解析的相依性集合組通常稱為「相依性樹狀結構」**、「相依性圖形」** 或「物件圖形」\*\*。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-133">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="8eb6a-134">`IMyDependency` 與 `ILogger<TCategoryName>` 必須在服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-134">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="8eb6a-135">`IMyDependency` 是在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-135">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8eb6a-136">`ILogger<TCategoryName>` 是由記錄抽象基礎結構所註冊，所以它是預設由架構所註冊的[架構提供的服務](#framework-provided-services)。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-136">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="8eb6a-137">容器會利用 [(泛型) 開放類型](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types) 解析 `ILogger<TCategoryName>`，讓您不需註冊每個 [(泛型) 建構的型別](/dotnet/csharp/language-reference/language-specification/types#constructed-types)：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-137">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<>), typeof(Logger<>));
```

<span data-ttu-id="8eb6a-138">在範例應用程式中，`IMyDependency` 服務是使用具象型別 `MyDependency` 所註冊。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-138">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="8eb6a-139">註冊會將服務的存留期範圍限制為單一要求的存留期。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-139">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="8eb6a-140">將在此主題稍後將說明[服務存留期](#service-lifetimes)。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-140">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> <span data-ttu-id="8eb6a-141">每個 `services.Add{SERVICE_NAME}` 擴充方法都會新增 (並且可能會設定) 服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-141">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="8eb6a-142">例如，會 `services.AddMvc()` 加入服務 Razor 頁面，而 MVC 則需要。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-142">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="8eb6a-143">建議應用程式遵循此慣例。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-143">We recommended that apps follow this convention.</span></span> <span data-ttu-id="8eb6a-144">在 [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) 命名空間中放置擴充方法，以封裝服務註冊群組。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-144">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="8eb6a-145">如果服務的建構函式需要[內建類型](/dotnet/csharp/language-reference/keywords/built-in-types-table)，例如 `string`，可以使用[組態](xref:fundamentals/configuration/index)或[選項模式](xref:fundamentals/configuration/options)插入該類型：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-145">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

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

<span data-ttu-id="8eb6a-146">服務執行個體是透過服務使用所在之類別建構函式所要求，而且會指派給私人欄位。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-146">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="8eb6a-147">該欄位會視需要用來存取整個類別中的服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-147">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="8eb6a-148">在範例應用程式中，會要求 `IMyDependency` 執行個體並使用它來呼叫服務的 `WriteMessage` 方法：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-148">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="services-injected-into-startup"></a><span data-ttu-id="8eb6a-149">插入至啟動的服務</span><span class="sxs-lookup"><span data-stu-id="8eb6a-149">Services injected into Startup</span></span>

<span data-ttu-id="8eb6a-150">`Startup`使用泛型主機（）時，只有下列服務類型可以插入至此函式 <xref:Microsoft.Extensions.Hosting.IHostBuilder> ：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-150">Only the following service types can be injected into the `Startup` constructor when using the Generic Host (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span></span>

* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

<span data-ttu-id="8eb6a-151">服務可以插入 `Startup.Configure` ：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-151">Services can be injected into `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> options)
{
    ...
}
```

<span data-ttu-id="8eb6a-152">如需詳細資訊，請參閱 <xref:fundamentals/startup> 。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-152">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="8eb6a-153">架構提供的服務</span><span class="sxs-lookup"><span data-stu-id="8eb6a-153">Framework-provided services</span></span>

<span data-ttu-id="8eb6a-154">`Startup.ConfigureServices`方法負責定義應用程式所使用的服務，包括平臺功能，例如 Entity Framework Core 和 ASP.NET CORE MVC。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-154">The `Startup.ConfigureServices` method is responsible for defining the services that the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="8eb6a-155">一開始， `IServiceCollection` 提供的會 `ConfigureServices` 根據主機的[設定方式](xref:fundamentals/index#host)，來擁有架構所定義的服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-155">Initially, the `IServiceCollection` provided to `ConfigureServices` has services defined by the framework depending on [how the host was configured](xref:fundamentals/index#host).</span></span> <span data-ttu-id="8eb6a-156">以 ASP.NET Core 範本為基礎的應用程式，在架構中註冊數百項服務並不常見。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-156">It's not uncommon for an app based on an ASP.NET Core template to have hundreds of services registered by the framework.</span></span> <span data-ttu-id="8eb6a-157">下表列出架構註冊服務的小型範例。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-157">A small sample of framework-registered services is listed in the following table.</span></span>

| <span data-ttu-id="8eb6a-158">服務類型</span><span class="sxs-lookup"><span data-stu-id="8eb6a-158">Service Type</span></span> | <span data-ttu-id="8eb6a-159">存留期</span><span class="sxs-lookup"><span data-stu-id="8eb6a-159">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="8eb6a-160">暫時性</span><span class="sxs-lookup"><span data-stu-id="8eb6a-160">Transient</span></span> |
| `IHostApplicationLifetime` | <span data-ttu-id="8eb6a-161">單一</span><span class="sxs-lookup"><span data-stu-id="8eb6a-161">Singleton</span></span> |
| `IWebHostEnvironment` | <span data-ttu-id="8eb6a-162">單一</span><span class="sxs-lookup"><span data-stu-id="8eb6a-162">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="8eb6a-163">單一</span><span class="sxs-lookup"><span data-stu-id="8eb6a-163">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="8eb6a-164">暫時性</span><span class="sxs-lookup"><span data-stu-id="8eb6a-164">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="8eb6a-165">單一</span><span class="sxs-lookup"><span data-stu-id="8eb6a-165">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="8eb6a-166">暫時性</span><span class="sxs-lookup"><span data-stu-id="8eb6a-166">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="8eb6a-167">單一</span><span class="sxs-lookup"><span data-stu-id="8eb6a-167">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="8eb6a-168">單一</span><span class="sxs-lookup"><span data-stu-id="8eb6a-168">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="8eb6a-169">單一</span><span class="sxs-lookup"><span data-stu-id="8eb6a-169">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="8eb6a-170">暫時性</span><span class="sxs-lookup"><span data-stu-id="8eb6a-170">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="8eb6a-171">單一</span><span class="sxs-lookup"><span data-stu-id="8eb6a-171">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="8eb6a-172">單一</span><span class="sxs-lookup"><span data-stu-id="8eb6a-172">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="8eb6a-173">單一</span><span class="sxs-lookup"><span data-stu-id="8eb6a-173">Singleton</span></span> |

## <a name="register-additional-services-with-extension-methods"></a><span data-ttu-id="8eb6a-174">以擴充方法註冊其他服務</span><span class="sxs-lookup"><span data-stu-id="8eb6a-174">Register additional services with extension methods</span></span>

<span data-ttu-id="8eb6a-175">當可以使用服務集合擴充方法來註冊服務 (如果需要，也可以註冊其相依服務) 時，慣例是使用單一 `Add{SERVICE_NAME}` 擴充方法來註冊該服務要求的所有服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-175">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="8eb6a-176">下列程式碼範例說明如何使用擴充方法[AddDbCoNtext \<TContext> ](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)和，將其他服務新增至容器 <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*> ：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-176">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) and <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>:</span></span>

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

<span data-ttu-id="8eb6a-177">如需詳細資訊，請參閱 API 文件中的 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> 類別。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-177">For more information, see the <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> class in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="8eb6a-178">服務存留期</span><span class="sxs-lookup"><span data-stu-id="8eb6a-178">Service lifetimes</span></span>

<span data-ttu-id="8eb6a-179">為每個已註冊的服務選擇適當的存留期。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-179">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="8eb6a-180">ASP.NET Core 服務可以使用下列存留期進行設定：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-180">ASP.NET Core services can be configured with the following lifetimes:</span></span>

### <a name="transient"></a><span data-ttu-id="8eb6a-181">暫時性</span><span class="sxs-lookup"><span data-stu-id="8eb6a-181">Transient</span></span>

<span data-ttu-id="8eb6a-182">每次從服務容器要求暫時性存留期服務 (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) 時都會建立它們。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-182">Transient lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) are created each time they're requested from the service container.</span></span> <span data-ttu-id="8eb6a-183">此存留期最適合用於輕量型的無狀態服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-183">This lifetime works best for lightweight, stateless services.</span></span>

### <a name="scoped"></a><span data-ttu-id="8eb6a-184">具範圍</span><span class="sxs-lookup"><span data-stu-id="8eb6a-184">Scoped</span></span>

<span data-ttu-id="8eb6a-185">具範圍存留期服務 (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) 會在每次用戶端要求 (連線) 時建立一次。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-185">Scoped lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) are created once per client request (connection).</span></span>

> [!WARNING]
> <span data-ttu-id="8eb6a-186">在中介軟體中使用具範圍服務時，請將該服務插入 `Invoke` 或 `InvokeAsync` 方法中。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-186">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="8eb6a-187">不要透過函式[插入](xref:mvc/controllers/dependency-injection#constructor-injection)來插入，因為它會強制服務的行為就像 singleton 一樣。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-187">Don't inject via [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="8eb6a-188">如需詳細資訊，請參閱 <xref:fundamentals/middleware/write#per-request-middleware-dependencies> 。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-188">For more information, see <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span></span>

### <a name="singleton"></a><span data-ttu-id="8eb6a-189">單一</span><span class="sxs-lookup"><span data-stu-id="8eb6a-189">Singleton</span></span>

<span data-ttu-id="8eb6a-190">當第一次收到有關單一資料庫存留期服務的要求時 (或是當執行 `Startup.ConfigureServices` 且隨著服務註冊指定執行個體時)，即會建立單一資料庫存留期服務 (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>)。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-190">Singleton lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) are created the first time they're requested (or when `Startup.ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="8eb6a-191">每個後續要求都會使用相同的執行個體。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-191">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="8eb6a-192">若應用程式要求單一資料庫行為，建議您允許服務容器管理服務的存留期。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-192">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="8eb6a-193">不要實作單一資料庫設計模式並為使用者提供可用來管理類別中物件存留期的程式碼。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-193">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="8eb6a-194">從單一資料庫解析具範圍服務是非常危險的。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-194">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="8eb6a-195">處理後續要求時，它可能會導致服務有不正確的狀態。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-195">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

## <a name="service-registration-methods"></a><span data-ttu-id="8eb6a-196">服務註冊方法</span><span class="sxs-lookup"><span data-stu-id="8eb6a-196">Service registration methods</span></span>

<span data-ttu-id="8eb6a-197">服務註冊擴充方法提供在特定案例中很有用的多載。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-197">Service registration extension methods offer overloads that are useful in specific scenarios.</span></span>

| <span data-ttu-id="8eb6a-198">方法</span><span class="sxs-lookup"><span data-stu-id="8eb6a-198">Method</span></span> | <span data-ttu-id="8eb6a-199">自動</span><span class="sxs-lookup"><span data-stu-id="8eb6a-199">Automatic</span></span><br><span data-ttu-id="8eb6a-200">物件 (object)</span><span class="sxs-lookup"><span data-stu-id="8eb6a-200">object</span></span><br><span data-ttu-id="8eb6a-201">處置</span><span class="sxs-lookup"><span data-stu-id="8eb6a-201">disposal</span></span> | <span data-ttu-id="8eb6a-202">多個</span><span class="sxs-lookup"><span data-stu-id="8eb6a-202">Multiple</span></span><br><span data-ttu-id="8eb6a-203">實作</span><span class="sxs-lookup"><span data-stu-id="8eb6a-203">implementations</span></span> | <span data-ttu-id="8eb6a-204">傳遞引數</span><span class="sxs-lookup"><span data-stu-id="8eb6a-204">Pass args</span></span> |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br><span data-ttu-id="8eb6a-205">範例：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-205">Example:</span></span><br>`services.AddSingleton<IMyDep, MyDep>();` | <span data-ttu-id="8eb6a-206">是</span><span class="sxs-lookup"><span data-stu-id="8eb6a-206">Yes</span></span> | <span data-ttu-id="8eb6a-207">是</span><span class="sxs-lookup"><span data-stu-id="8eb6a-207">Yes</span></span> | <span data-ttu-id="8eb6a-208">否</span><span class="sxs-lookup"><span data-stu-id="8eb6a-208">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br><span data-ttu-id="8eb6a-209">範例：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-209">Examples:</span></span><br>`services.AddSingleton<IMyDep>(sp => new MyDep());`<br>`services.AddSingleton<IMyDep>(sp => new MyDep("A string!"));` | <span data-ttu-id="8eb6a-210">是</span><span class="sxs-lookup"><span data-stu-id="8eb6a-210">Yes</span></span> | <span data-ttu-id="8eb6a-211">是</span><span class="sxs-lookup"><span data-stu-id="8eb6a-211">Yes</span></span> | <span data-ttu-id="8eb6a-212">是</span><span class="sxs-lookup"><span data-stu-id="8eb6a-212">Yes</span></span> |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br><span data-ttu-id="8eb6a-213">範例：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-213">Example:</span></span><br>`services.AddSingleton<MyDep>();` | <span data-ttu-id="8eb6a-214">是</span><span class="sxs-lookup"><span data-stu-id="8eb6a-214">Yes</span></span> | <span data-ttu-id="8eb6a-215">否</span><span class="sxs-lookup"><span data-stu-id="8eb6a-215">No</span></span> | <span data-ttu-id="8eb6a-216">否</span><span class="sxs-lookup"><span data-stu-id="8eb6a-216">No</span></span> |
| `AddSingleton<{SERVICE}>(new {IMPLEMENTATION})`<br><span data-ttu-id="8eb6a-217">範例：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-217">Examples:</span></span><br>`services.AddSingleton<IMyDep>(new MyDep());`<br>`services.AddSingleton<IMyDep>(new MyDep("A string!"));` | <span data-ttu-id="8eb6a-218">否</span><span class="sxs-lookup"><span data-stu-id="8eb6a-218">No</span></span> | <span data-ttu-id="8eb6a-219">是</span><span class="sxs-lookup"><span data-stu-id="8eb6a-219">Yes</span></span> | <span data-ttu-id="8eb6a-220">是</span><span class="sxs-lookup"><span data-stu-id="8eb6a-220">Yes</span></span> |
| `AddSingleton(new {IMPLEMENTATION})`<br><span data-ttu-id="8eb6a-221">範例：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-221">Examples:</span></span><br>`services.AddSingleton(new MyDep());`<br>`services.AddSingleton(new MyDep("A string!"));` | <span data-ttu-id="8eb6a-222">否</span><span class="sxs-lookup"><span data-stu-id="8eb6a-222">No</span></span> | <span data-ttu-id="8eb6a-223">否</span><span class="sxs-lookup"><span data-stu-id="8eb6a-223">No</span></span> | <span data-ttu-id="8eb6a-224">是</span><span class="sxs-lookup"><span data-stu-id="8eb6a-224">Yes</span></span> |

<span data-ttu-id="8eb6a-225">如需類型處置的詳細資訊，請參閱[＜服務處置＞](#disposal-of-services)一節。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-225">For more information on type disposal, see the [Disposal of services](#disposal-of-services) section.</span></span> <span data-ttu-id="8eb6a-226">多個實作的常見案例是[模擬測試類型](xref:test/integration-tests#inject-mock-services)。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-226">A common scenario for multiple implementations is [mocking types for testing](xref:test/integration-tests#inject-mock-services).</span></span>

<span data-ttu-id="8eb6a-227">如果還未註冊任何實作，則 `TryAdd{LIFETIME}` 方法只會註冊服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-227">`TryAdd{LIFETIME}` methods only register the service if there isn't already an implementation registered.</span></span>

<span data-ttu-id="8eb6a-228">在下列範例中，第一行會為 `IMyDependency` 註冊 `MyDependency`。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-228">In the following example, the first line registers `MyDependency` for `IMyDependency`.</span></span> <span data-ttu-id="8eb6a-229">第二行則沒有任何作用，因為 `IMyDependency` 已具有註冊的實作：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-229">The second line has no effect because `IMyDependency` already has a registered implementation:</span></span>

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

<span data-ttu-id="8eb6a-230">如需詳細資訊，請參閱</span><span class="sxs-lookup"><span data-stu-id="8eb6a-230">For more information, see:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

<span data-ttu-id="8eb6a-231">如果還沒有「相同類型的」\*\* 實作，則 [TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) 方法只會註冊服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-231">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) methods only register the service if there isn't already an implementation *of the same type*.</span></span> <span data-ttu-id="8eb6a-232">多個服務會透過 `IEnumerable<{SERVICE}>` 解析。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-232">Multiple services are resolved via `IEnumerable<{SERVICE}>`.</span></span> <span data-ttu-id="8eb6a-233">註冊服務時，如果尚未新增相同類型的執行個體，則開發人員只會希望新增執行個體。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-233">When registering services, the developer only wants to add an instance if one of the same type hasn't already been added.</span></span> <span data-ttu-id="8eb6a-234">程式庫作者通常會使用此方法來避免註冊容器中某個執行個體的兩個複本。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-234">Generally, this method is used by library authors to avoid registering two copies of an instance in the container.</span></span>

<span data-ttu-id="8eb6a-235">在下列範例中，第一行會為 `IMyDep1` 註冊 `MyDep`。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-235">In the following example, the first line registers `MyDep` for `IMyDep1`.</span></span> <span data-ttu-id="8eb6a-236">第二行會為 `IMyDep2` 註冊 `MyDep`。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-236">The second line registers `MyDep` for `IMyDep2`.</span></span> <span data-ttu-id="8eb6a-237">第三行則沒有任何作用，因為 `IMyDep1` 已具有 `MyDep` 的已註冊實作：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-237">The third line has no effect because `IMyDep1` already has a registered implementation of `MyDep`:</span></span>

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a><span data-ttu-id="8eb6a-238">建構函式插入行為</span><span class="sxs-lookup"><span data-stu-id="8eb6a-238">Constructor injection behavior</span></span>

<span data-ttu-id="8eb6a-239">服務可以透過兩個機制來解析：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-239">Services can be resolved by two mechanisms:</span></span>

* <xref:System.IServiceProvider>
* <span data-ttu-id="8eb6a-240"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities>：允許在相依性插入容器中不註冊服務時，建立物件。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-240"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities>: Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="8eb6a-241">搭配使用者面向抽象 (例如標籤協助程式、MVC 控制器與模型繫結器) 使用 `ActivatorUtilities`。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-241">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="8eb6a-242">建構函式可以接受不是由相依性插入提供的引數，但引數必須指派預設值。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-242">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="8eb6a-243">當服務由或解析時，程式的 `IServiceProvider` `ActivatorUtilities` [插入](xref:mvc/controllers/dependency-injection#constructor-injection)需要*公用*的函式。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-243">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) requires a *public* constructor.</span></span>

<span data-ttu-id="8eb6a-244">當服務由解析時 `ActivatorUtilities` ，程式的[插入](xref:mvc/controllers/dependency-injection#constructor-injection)只需要有一個適用的函式。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-244">When services are resolved by `ActivatorUtilities`, [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) requires that only one applicable constructor exists.</span></span> <span data-ttu-id="8eb6a-245">支援建構函式多載，但只能有一個多載存在，其引數可以藉由相依性插入而完成。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-245">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="8eb6a-246">Entity Framework 內容</span><span class="sxs-lookup"><span data-stu-id="8eb6a-246">Entity Framework contexts</span></span>

<span data-ttu-id="8eb6a-247">因為一般會將 Web 應用程式資料庫作業範圍設定為用戶端要求，所以通常會使用[具範圍存留期](#service-lifetimes)將 Entity Framework 內容新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-247">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="8eb6a-248">如果在註冊資料庫內容時， [AddDbCoNtext \<TContext> ](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)多載未指定存留期，則會將預設存留期設定為範圍。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-248">The default lifetime is scoped if a lifetime isn't specified by an [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) overload when registering the database context.</span></span> <span data-ttu-id="8eb6a-249">指定存留期的服務不應該使用存留期比服務還短的資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-249">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="8eb6a-250">留期和註冊選項</span><span class="sxs-lookup"><span data-stu-id="8eb6a-250">Lifetime and registration options</span></span>

<span data-ttu-id="8eb6a-251">為了示範這些存留期和註冊選項之間的差異，請考慮下列介面，以具有唯一識別碼 `OperationId` 的「作業」代表工作。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-251">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="8eb6a-252">視如何針對下列介面設定作業服務存留期而定，當類別要求時，容器會提供相同或不同的服務執行個體：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-252">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

<span data-ttu-id="8eb6a-253">介面是在 `Operation` 類別中實作。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-253">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="8eb6a-254">若未提供 GUID，`Operation` 建構函式會產生 GUID：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-254">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

<span data-ttu-id="8eb6a-255">會註冊相依於每種其他 `Operation` 類型的 `OperationService`。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-255">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="8eb6a-256">當透過相依性插入要求 `OperationService` 時，它會根據相依服務的存留期擷取每個服務的新執行個體或現有執行個體。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-256">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="8eb6a-257">當因收到來自容器的要求而建立暫時性服務時，`IOperationTransient` 服務的 `OperationId` 會與 `OperationService` 的 `OperationId` 不同。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-257">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="8eb6a-258">`OperationService` 會收到 `IOperationTransient` 類別的新執行個體。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-258">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="8eb6a-259">新執行個體會產生不同的 `OperationId`。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-259">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="8eb6a-260">當因用戶端要求而建立具範圍的服務時，`IOperationScoped` 服務與用戶端要求中 `OperationService` 的 `OperationId` 皆相同。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-260">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="8eb6a-261">在不同的用戶端要求中，兩個服務的 `OperationId` 值都不同。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-261">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="8eb6a-262">當建立一次 singleton 與 singleton 執行個體服務，並在所有用戶端要求與所有服務中使用時，`OperationId` 在所有服務要求中會固定不變。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-262">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

<span data-ttu-id="8eb6a-263">在 `Startup.ConfigureServices` 中，每個類型都會根據其具名存留期新增至容器：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-263">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

<span data-ttu-id="8eb6a-264">`IOperationSingletonInstance` 服務使用具有 `Guid.Empty` 已知識別碼的特定執行個體。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-264">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="8eb6a-265">很明顯此型別是使用中 (其 GUID 是所有區域)。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-265">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="8eb6a-266">範例應用程式示範個別要求內與個別要求之間的物件存留期。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-266">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="8eb6a-267">範例應用程式的 `IndexModel` 會要求每種 `IOperation` 型別與 `OperationService`。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-267">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="8eb6a-268">頁面接著會透過屬性指派來顯示所有頁面模型類別與服務的 `OperationId` 值：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-268">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

<span data-ttu-id="8eb6a-269">下面兩個輸出顯示兩個要求的結果：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-269">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="8eb6a-270">**第一個要求：**</span><span class="sxs-lookup"><span data-stu-id="8eb6a-270">**First request:**</span></span>

<span data-ttu-id="8eb6a-271">控制器作業：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-271">Controller operations:</span></span>

<span data-ttu-id="8eb6a-272">暫時性： d233e165-f417-469b-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="8eb6a-272">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="8eb6a-273">具範圍： 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="8eb6a-273">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="8eb6a-274">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="8eb6a-274">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="8eb6a-275">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="8eb6a-275">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="8eb6a-276">`OperationService` 作業：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-276">`OperationService` operations:</span></span>

<span data-ttu-id="8eb6a-277">暫時性： c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="8eb6a-277">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="8eb6a-278">具範圍： 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="8eb6a-278">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="8eb6a-279">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="8eb6a-279">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="8eb6a-280">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="8eb6a-280">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="8eb6a-281">**:第二個要求：**</span><span class="sxs-lookup"><span data-stu-id="8eb6a-281">**Second request:**</span></span>

<span data-ttu-id="8eb6a-282">控制器作業：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-282">Controller operations:</span></span>

<span data-ttu-id="8eb6a-283">暫時性： b63bd538-0a37-4ff1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="8eb6a-283">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="8eb6a-284">具範圍： 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="8eb6a-284">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="8eb6a-285">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="8eb6a-285">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="8eb6a-286">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="8eb6a-286">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="8eb6a-287">`OperationService` 作業：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-287">`OperationService` operations:</span></span>

<span data-ttu-id="8eb6a-288">暫時性： c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="8eb6a-288">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="8eb6a-289">具範圍： 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="8eb6a-289">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="8eb6a-290">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="8eb6a-290">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="8eb6a-291">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="8eb6a-291">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="8eb6a-292">觀察哪些 `OperationId` 值在要求內以及要求之間不同：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-292">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="8eb6a-293">「暫時性」\*\* 物件一律不同。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-293">*Transient* objects are always different.</span></span> <span data-ttu-id="8eb6a-294">第一個與第二個用戶端要求的暫時性 `OperationId` 值在兩個 `OperationService` 作業之間與各用戶端要求之間都不同。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-294">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="8eb6a-295">新執行個體會提供給每個服務要求以及用戶端要求。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-295">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="8eb6a-296">「具範圍」\*\* 物件在同一個用戶端要求內皆相同，但在不同的用戶端要求之間則不同。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-296">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="8eb6a-297">*Singleton*不論是否 `Operation` 在中提供實例，每個物件和每個要求的單一物件都是相同的 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-297">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `Startup.ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="8eb6a-298">從主要呼叫服務</span><span class="sxs-lookup"><span data-stu-id="8eb6a-298">Call services from main</span></span>

<span data-ttu-id="8eb6a-299">使用 [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) 建立 <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>，以解析應用程式範圍中的範圍服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-299">Create an <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="8eb6a-300">此法可用於在開機時存取範圍服務，以執行初始化工作。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-300">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="8eb6a-301">下列範例示範如何在 `Program.Main` 中取得 `MyScopedService`：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-301">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="8eb6a-302">範圍驗證</span><span class="sxs-lookup"><span data-stu-id="8eb6a-302">Scope validation</span></span>

<span data-ttu-id="8eb6a-303">當應用程式在開發環境中執行並呼叫[CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings)來建立主機時，預設服務提供者會執行檢查以確認：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-303">When the app is running in the Development environment and calls [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) to build the host, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="8eb6a-304">範圍服務不是直接或間接由開機服務提供者解析。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-304">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="8eb6a-305">範圍服務不是直接或間接插入至單一服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-305">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="8eb6a-306">根服務提供者會在呼叫 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> 時建立。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-306">The root service provider is created when <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> is called.</span></span> <span data-ttu-id="8eb6a-307">當提供者啟動應用程式時，根服務提供者的存留期與應用程式/伺服器的存留期一致，並會在應用程式關閉時處置。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-307">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="8eb6a-308">範圍服務會由建立這些服務的容器處置。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-308">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="8eb6a-309">若是在根容器中建立範圍服務，因為當應用程式/伺服器關機時，服務只會由根容器處理，所以服務的存留期會提升為單一服務等級。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-309">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="8eb6a-310">當呼叫 `BuildServiceProvider` 時，驗證服務範圍會攔截到這些情況。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-310">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="8eb6a-311">如需詳細資訊，請參閱 <xref:fundamentals/host/web-host#scope-validation> 。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-311">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="8eb6a-312">要求服務</span><span class="sxs-lookup"><span data-stu-id="8eb6a-312">Request Services</span></span>

<span data-ttu-id="8eb6a-313">來自 `HttpContext`，在 ASP.NET Core 要求內提供的服務是透過 [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) 集合公開。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-313">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) collection.</span></span>

<span data-ttu-id="8eb6a-314">要求服務代表您在應用程式中設定和要求的服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-314">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="8eb6a-315">當物件指定相依性時，這些會由在 `RequestServices` 中找到的類型來滿足，而非 `ApplicationServices`。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-315">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="8eb6a-316">一般而言，應用程式不應該直接使用這些屬性。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-316">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="8eb6a-317">相反地，請透過類別的函式要求類別所需的類型，並允許架構插入相依性。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-317">Instead, request the types that classes require via class constructors and allow the framework to inject the dependencies.</span></span> <span data-ttu-id="8eb6a-318">這會產生容易測試的類別。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-318">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="8eb6a-319">偏好要求相依性作為建構函式參數，而不要存取 `RequestServices` 集合。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-319">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="8eb6a-320">針對相依性插入設計服務</span><span class="sxs-lookup"><span data-stu-id="8eb6a-320">Design services for dependency injection</span></span>

<span data-ttu-id="8eb6a-321">最佳做法是：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-321">Best practices are to:</span></span>

* <span data-ttu-id="8eb6a-322">設計服務以使用相依性插入來取得其相依性。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-322">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="8eb6a-323">避免具狀態的靜態類別和成員。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-323">Avoid stateful, static classes and members.</span></span> <span data-ttu-id="8eb6a-324">將應用程式設計成使用單一服務，以避免建立全域狀態。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-324">Design apps to use singleton services instead, which avoid creating global state.</span></span>
* <span data-ttu-id="8eb6a-325">避免直接在服務內具現化相依類別。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-325">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="8eb6a-326">直接具現化會將程式碼耦合到特定實作。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-326">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="8eb6a-327">讓應用程式類別維持在小型、情況良好且可輕鬆測試的狀態。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-327">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="8eb6a-328">若類別有太多插入的相依性，這通常表示類別有太多責任而且正違反[單一責任原則 (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-328">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="8eb6a-329">將類別負責的某些部分移到新的類別，以嘗試重構類別。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-329">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="8eb6a-330">請記住，頁面 Razor 頁面模型類別和 MVC 控制器類別應該專注于 UI 考慮。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-330">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="8eb6a-331">商務規則和資料存取實作詳細資料應該保存在適合這些[分開考量](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) (Separation of Concerns) 類別中。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-331">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="8eb6a-332">處置服務</span><span class="sxs-lookup"><span data-stu-id="8eb6a-332">Disposal of services</span></span>

<span data-ttu-id="8eb6a-333">容器會為它建立的 <xref:System.IDisposable> 類型呼叫 <xref:System.IDisposable.Dispose*>。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-333">The container calls <xref:System.IDisposable.Dispose*> for the <xref:System.IDisposable> types it creates.</span></span> <span data-ttu-id="8eb6a-334">若執行個體由使用者程式碼新增到容器中，它將不會自動處置。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-334">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

<span data-ttu-id="8eb6a-335">在下列範例中，服務會建立服務容器並自動處置：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-335">In the following example, the services are created by the service container and disposed automatically:</span></span>

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

<span data-ttu-id="8eb6a-336">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="8eb6a-336">In the following example:</span></span>

* <span data-ttu-id="8eb6a-337">服務容器不會建立服務實例。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-337">The service instances aren't created by the service container.</span></span>
* <span data-ttu-id="8eb6a-338">架構不知道預期的服務存留期。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-338">The intended service lifetimes aren't known by the framework.</span></span>
* <span data-ttu-id="8eb6a-339">架構不會自動處置服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-339">The framework doesn't dispose of the services automatically.</span></span>
* <span data-ttu-id="8eb6a-340">如果服務未在開發人員程式碼中明確處置，它們會持續存在，直到應用程式關閉為止。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-340">If the services aren't explicitly disposed in developer code, they persist until the app shuts down.</span></span>

```csharp
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<Service1>(new Service1());
    services.AddSingleton(new Service2());
}
```

### <a name="idisposable-guidance-for-transient-and-shared-instances"></a><span data-ttu-id="8eb6a-341">暫時性和共用實例的 IDisposable 指引</span><span class="sxs-lookup"><span data-stu-id="8eb6a-341">IDisposable guidance for Transient and shared instances</span></span>

#### <a name="transient-limited-lifetime"></a><span data-ttu-id="8eb6a-342">暫時性，有限的存留期</span><span class="sxs-lookup"><span data-stu-id="8eb6a-342">Transient, limited lifetime</span></span>

<span data-ttu-id="8eb6a-343">**案例**</span><span class="sxs-lookup"><span data-stu-id="8eb6a-343">**Scenario**</span></span>

<span data-ttu-id="8eb6a-344">應用程式需要在 <xref:System.IDisposable> 下列任一情況下具有暫時性存留期的實例：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-344">The app requires an <xref:System.IDisposable> instance with a transient lifetime for either of the following scenarios:</span></span>

* <span data-ttu-id="8eb6a-345">實例會在根範圍中解析。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-345">The instance is resolved in the root scope.</span></span>
* <span data-ttu-id="8eb6a-346">應該在範圍結束之前處置實例。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-346">The instance should be disposed before the scope ends.</span></span>

<span data-ttu-id="8eb6a-347">**方案**</span><span class="sxs-lookup"><span data-stu-id="8eb6a-347">**Solution**</span></span>

<span data-ttu-id="8eb6a-348">使用 factory 模式，在父範圍外建立實例。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-348">Use the factory pattern to create an instance outside of the parent scope.</span></span> <span data-ttu-id="8eb6a-349">在這種情況下，應用程式通常會有 `Create` 方法，直接呼叫最終型別的函式。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-349">In this situation, the app would generally have a `Create` method that calls the final type's constructor directly.</span></span> <span data-ttu-id="8eb6a-350">如果最終類型具有其他相依性，則 factory 可以：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-350">If the final type has other dependencies, the factory can:</span></span>

* <span data-ttu-id="8eb6a-351"><xref:System.IServiceProvider>在其函式中接收。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-351">Receive an <xref:System.IServiceProvider> in its constructor.</span></span>
* <span data-ttu-id="8eb6a-352">使用 <xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities.CreateInstance%2A?displayProperty=nameWithType> 來具現化容器外的實例，同時使用容器來執行其相依性。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-352">Use <xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities.CreateInstance%2A?displayProperty=nameWithType> to instantiate the instance outside the container, while using the container for its dependencies.</span></span>

#### <a name="shared-instance-limited-lifetime"></a><span data-ttu-id="8eb6a-353">共用實例，有限的存留期</span><span class="sxs-lookup"><span data-stu-id="8eb6a-353">Shared Instance, limited lifetime</span></span>

<span data-ttu-id="8eb6a-354">**案例**</span><span class="sxs-lookup"><span data-stu-id="8eb6a-354">**Scenario**</span></span>

<span data-ttu-id="8eb6a-355">應用程式需要 <xref:System.IDisposable> 多個服務之間的共用實例，但 <xref:System.IDisposable> 應具有有限的存留期。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-355">The app requires a shared <xref:System.IDisposable> instance across multiple services, but the <xref:System.IDisposable> should have a limited lifetime.</span></span>

<span data-ttu-id="8eb6a-356">**方案**</span><span class="sxs-lookup"><span data-stu-id="8eb6a-356">**Solution**</span></span>

<span data-ttu-id="8eb6a-357">註冊具有限定範圍存留期的實例。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-357">Register the instance with a Scoped lifetime.</span></span> <span data-ttu-id="8eb6a-358">使用 <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope%2A?displayProperty=nameWithType> 來啟動並建立新的 <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> 。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-358">Use <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope%2A?displayProperty=nameWithType> to start and create a new <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>.</span></span> <span data-ttu-id="8eb6a-359">使用範圍的 <xref:System.IServiceProvider> 來取得所需的服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-359">Use the scope's <xref:System.IServiceProvider> to get required services.</span></span> <span data-ttu-id="8eb6a-360">在存留期結束時處置範圍。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-360">Dispose the scope when the lifetime should end.</span></span>

#### <a name="general-guidelines"></a><span data-ttu-id="8eb6a-361">一般準則</span><span class="sxs-lookup"><span data-stu-id="8eb6a-361">General Guidelines</span></span>

* <span data-ttu-id="8eb6a-362">不要向 <xref:System.IDisposable> 暫時性範圍註冊實例。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-362">Don't register <xref:System.IDisposable> instances with a Transient scope.</span></span> <span data-ttu-id="8eb6a-363">請改用 factory 模式。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-363">Use the factory pattern instead.</span></span>
* <span data-ttu-id="8eb6a-364">請勿解析 <xref:System.IDisposable> 根範圍中的暫時性或範圍實例。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-364">Don't resolve Transient or Scoped <xref:System.IDisposable> instances in the root scope.</span></span> <span data-ttu-id="8eb6a-365">唯一的例外狀況是當應用程式建立/重新建立和處置時 <xref:System.IServiceProvider> ，這不是理想的模式。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-365">The only general exception is when the app creates/recreates and disposes the <xref:System.IServiceProvider>, which isn't an ideal pattern.</span></span>
* <span data-ttu-id="8eb6a-366">透過 DI 接收相依性 <xref:System.IDisposable> 並不會要求接收者 <xref:System.IDisposable> 自行執行。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-366">Receiving an <xref:System.IDisposable> dependency via DI doesn't require that the receiver implement <xref:System.IDisposable> itself.</span></span> <span data-ttu-id="8eb6a-367">相依性的接收者不 <xref:System.IDisposable> 應呼叫 <xref:System.IDisposable.Dispose%2A> 該相依性上的。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-367">The receiver of the <xref:System.IDisposable> dependency shouldn't call <xref:System.IDisposable.Dispose%2A> on that dependency.</span></span>
* <span data-ttu-id="8eb6a-368">範圍應該用來控制服務的存留期。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-368">Scopes should be used to control lifetimes of services.</span></span> <span data-ttu-id="8eb6a-369">範圍不是階層式，而且範圍之間沒有特殊連接。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-369">Scopes aren't hierarchical, and there's no special connection among scopes.</span></span>

## <a name="default-service-container-replacement"></a><span data-ttu-id="8eb6a-370">預設服務容器取代</span><span class="sxs-lookup"><span data-stu-id="8eb6a-370">Default service container replacement</span></span>

<span data-ttu-id="8eb6a-371">內建的服務容器是設計用來滿足架構和大部分取用者應用程式的需求。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-371">The built-in service container is designed to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="8eb6a-372">我們建議使用內建容器，除非您需要內建容器不支援的特定功能，例如：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-372">We recommend using the built-in container unless you need a specific feature that the built-in container doesn't support, such as:</span></span>

* <span data-ttu-id="8eb6a-373">屬性插入</span><span class="sxs-lookup"><span data-stu-id="8eb6a-373">Property injection</span></span>
* <span data-ttu-id="8eb6a-374">根據名稱插入</span><span class="sxs-lookup"><span data-stu-id="8eb6a-374">Injection based on name</span></span>
* <span data-ttu-id="8eb6a-375">子容器</span><span class="sxs-lookup"><span data-stu-id="8eb6a-375">Child containers</span></span>
* <span data-ttu-id="8eb6a-376">自訂生命週期管理</span><span class="sxs-lookup"><span data-stu-id="8eb6a-376">Custom lifetime management</span></span>
* <span data-ttu-id="8eb6a-377">`Func<T>` 支援延遲初始設定</span><span class="sxs-lookup"><span data-stu-id="8eb6a-377">`Func<T>` support for lazy initialization</span></span>
* <span data-ttu-id="8eb6a-378">以慣例為基礎的註冊</span><span class="sxs-lookup"><span data-stu-id="8eb6a-378">Convention-based registration</span></span>

<span data-ttu-id="8eb6a-379">下列協力廠商容器可以與 ASP.NET Core 應用程式搭配使用：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-379">The following 3rd party containers can be used with ASP.NET Core apps:</span></span>

* [<span data-ttu-id="8eb6a-380">Autofac</span><span class="sxs-lookup"><span data-stu-id="8eb6a-380">Autofac</span></span>](https://autofac.readthedocs.io/en/latest/integration/aspnetcore.html)
* [<span data-ttu-id="8eb6a-381">DryIoc</span><span class="sxs-lookup"><span data-stu-id="8eb6a-381">DryIoc</span></span>](https://www.nuget.org/packages/DryIoc.Microsoft.DependencyInjection)
* [<span data-ttu-id="8eb6a-382">Grace</span><span class="sxs-lookup"><span data-stu-id="8eb6a-382">Grace</span></span>](https://www.nuget.org/packages/Grace.DependencyInjection.Extensions)
* [<span data-ttu-id="8eb6a-383">LightInject</span><span class="sxs-lookup"><span data-stu-id="8eb6a-383">LightInject</span></span>](https://github.com/seesharper/LightInject.Microsoft.DependencyInjection)
* [<span data-ttu-id="8eb6a-384">拉馬爾</span><span class="sxs-lookup"><span data-stu-id="8eb6a-384">Lamar</span></span>](https://jasperfx.github.io/lamar/)
* [<span data-ttu-id="8eb6a-385">Stashbox</span><span class="sxs-lookup"><span data-stu-id="8eb6a-385">Stashbox</span></span>](https://github.com/z4kn4fein/stashbox-extensions-dependencyinjection)
* [<span data-ttu-id="8eb6a-386">Unity</span><span class="sxs-lookup"><span data-stu-id="8eb6a-386">Unity</span></span>](https://www.nuget.org/packages/Unity.Microsoft.DependencyInjection)

### <a name="thread-safety"></a><span data-ttu-id="8eb6a-387">執行緒安全</span><span class="sxs-lookup"><span data-stu-id="8eb6a-387">Thread safety</span></span>

<span data-ttu-id="8eb6a-388">建立具備執行緒安全性的 singleton 服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-388">Create thread-safe singleton services.</span></span> <span data-ttu-id="8eb6a-389">如果 singleton 服務相依於暫時性服務，暫時性服務可能也需要具備執行緒安全性，取決於 singleton 如何使用它。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-389">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="8eb6a-390">單一服務的 factory 方法（例如[AddSingleton \<TService> （IServiceCollection，Func \<IServiceProvider,TService> ）](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*)的第二個引數）不需要是安全線程。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-390">The factory method of single service, such as the second argument to [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), doesn't need to be thread-safe.</span></span> <span data-ttu-id="8eb6a-391">就像型別 (`static`) 建構函式一樣，它一定會被單一執行緒呼叫一次。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-391">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="8eb6a-392">建議</span><span class="sxs-lookup"><span data-stu-id="8eb6a-392">Recommendations</span></span>

* <span data-ttu-id="8eb6a-393">不支援以 `async/await` 與 `Task` 為基礎的服務解析。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-393">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="8eb6a-394">C# 不支援非同步建構函式，因此建議的模式是以同步方式解析服務後使用非同步方法。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-394">C# does not support asynchronous constructors; therefore, the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="8eb6a-395">避免直接在服務容器中儲存資料與設定。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-395">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="8eb6a-396">例如，使用者的購物車通常不應該新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-396">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="8eb6a-397">組態應該使用[選項模式](xref:fundamentals/configuration/options)。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-397">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="8eb6a-398">同樣地，請避免只存在以允許存取某個其他物件的「資料持有者」物件。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-398">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="8eb6a-399">最好是透過 DI 要求實際項目。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-399">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="8eb6a-400">避免以靜態方式存取服務 (例如，以靜態方式設定 [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) 型別以四處使用)。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-400">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) for use elsewhere).</span></span>

* <span data-ttu-id="8eb6a-401">避免使用「服務定位器模式」\*\*。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-401">Avoid using the *service locator pattern*.</span></span> <span data-ttu-id="8eb6a-402">例如，當您可以改用 DI 時，請勿叫用 <xref:System.IServiceProvider.GetService*> 來取得服務執行個體：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-402">For example, don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead:</span></span>

  <span data-ttu-id="8eb6a-403">**正確**</span><span class="sxs-lookup"><span data-stu-id="8eb6a-403">**Incorrect:**</span></span>

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

  <span data-ttu-id="8eb6a-404">**正確**：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-404">**Correct**:</span></span>

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

* <span data-ttu-id="8eb6a-405">另一個要避免的服務定位器變化是插入在執行階段解析相依性的處理站。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-405">Another service locator variation to avoid is injecting a factory that resolves dependencies at runtime.</span></span> <span data-ttu-id="8eb6a-406">這兩種做法都會混用[控制反轉](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion)策略。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-406">Both of these practices mix [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

* <span data-ttu-id="8eb6a-407">避免以靜態方式存取 `HttpContext` (例如 [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext))。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-407">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span></span>

<span data-ttu-id="8eb6a-408">就像所有的建議集，您可能會遇到需要忽略建議的情況。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-408">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="8eb6a-409">例外狀況很罕見，大部分是在架構本身內的特殊案例。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-409">Exceptions are rare, mostly special cases within the framework itself.</span></span>

<span data-ttu-id="8eb6a-410">DI 是靜態/全域物件存取模式的「替代」\*\* 選項。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-410">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="8eb6a-411">如果您將 DI 與靜態物件存取混合，則可能無法實現 DI 的優點。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-411">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="recommended-patterns-for-multi-tenancy-in-di"></a><span data-ttu-id="8eb6a-412">DI 中的多租使用者建議模式</span><span class="sxs-lookup"><span data-stu-id="8eb6a-412">Recommended patterns for multi-tenancy in DI</span></span>

<span data-ttu-id="8eb6a-413">[Orchard Core](https://github.com/OrchardCMS/OrchardCore)提供多租使用者。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-413">[Orchard Core](https://github.com/OrchardCMS/OrchardCore) provides multi-tenancy.</span></span> <span data-ttu-id="8eb6a-414">如需詳細資訊，請參閱[Orchard Core 檔](https://docs.orchardcore.net/en/dev/)。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-414">For more information, see the [Orchard Core Documentation](https://docs.orchardcore.net/en/dev/).</span></span>

<span data-ttu-id="8eb6a-415">https://github.com/OrchardCMS/OrchardCore.Samples如需如何使用 Orchard Core Framework 建立模組化和多租使用者應用程式的範例，而不含任何 CMS 特有的功能，請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-415">See the samples apps at https://github.com/OrchardCMS/OrchardCore.Samples for examples of how to build modular and multi-tenant apps using just Orchard Core Framework without any of the CMS specific features.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8eb6a-416">其他資源</span><span class="sxs-lookup"><span data-stu-id="8eb6a-416">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/fundamentals/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="8eb6a-417">在 ASP.NET Core 中處置 IDisposables 的四種方式</span><span class="sxs-lookup"><span data-stu-id="8eb6a-417">Four ways to dispose IDisposables in ASP.NET Core</span></span>](https://andrewlock.net/four-ways-to-dispose-idisposables-in-asp-net-core/)
* [<span data-ttu-id="8eb6a-418">在 ASP.NET Core 使用 Dependency Injection 撰寫簡潔的程式碼 (MSDN)</span><span class="sxs-lookup"><span data-stu-id="8eb6a-418">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="8eb6a-419">明確相依性準則</span><span class="sxs-lookup"><span data-stu-id="8eb6a-419">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* <span data-ttu-id="8eb6a-420">[逆轉控制容器和相依性插入模式 (Martin Fowler)](https://www.martinfowler.com/articles/injection.html) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="8eb6a-420">[Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)](https://www.martinfowler.com/articles/injection.html)</span></span>
* <span data-ttu-id="8eb6a-421">[How to register a service with multiple interfaces in ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/) (如何使用 ASP.NET Core DI 中的多個介面註冊服務)</span><span class="sxs-lookup"><span data-stu-id="8eb6a-421">[How to register a service with multiple interfaces in ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="8eb6a-422">ASP.NET Core 支援相依性插入 (DI) 軟體設計模式，這是用來在類別與其相依性之間達成[控制權反轉 (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) \(英文\) 的技術。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-422">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="8eb6a-423">如需有關 MVC 控制器內相依性插入的特定詳細資訊，請參閱 <xref:mvc/controllers/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-423">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="8eb6a-424">[查看或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="8eb6a-424">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="8eb6a-425">相依性插入概觀</span><span class="sxs-lookup"><span data-stu-id="8eb6a-425">Overview of dependency injection</span></span>

<span data-ttu-id="8eb6a-426">「相依性」\*\* 是另一個物件所需的任何物件。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-426">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="8eb6a-427">檢查具有 `WriteMessage` 方法的下列 `MyDependency` 物件，應用程式中的其他類別相依於此物件：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-427">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

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

<span data-ttu-id="8eb6a-428">您可以建立 `MyDependency` 類別的執行個體，讓 `WriteMessage` 方法可供類別使用。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-428">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="8eb6a-429">`MyDependency` 類別是 `IndexModel` 類別的相依性：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-429">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

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

<span data-ttu-id="8eb6a-430">該類別會建立 `MyDependency` 執行個體並直接相依於該執行個體。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-430">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="8eb6a-431">程式碼相依性 (例如前一個範例) 有問題，因此基於下列原因應該避免使用：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-431">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="8eb6a-432">若要將 `MyDependency` 取代為不同的實作，必須修改該類別。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-432">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="8eb6a-433">若 `MyDependency` 有相依性，那些相依性必須由該類別設定。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-433">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="8eb6a-434">在具有多個相依於 `MyDependency` 之多個類別的大型專案中，設定程式碼在不同的應用程式之間會變得鬆散。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-434">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="8eb6a-435">此實作難以進行單元測試。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-435">This implementation is difficult to unit test.</span></span> <span data-ttu-id="8eb6a-436">應用程式應該使用模擬 (Mock) 或虛設常式 (Stub) `MyDependency` 類別，這在使用此方法時無法使用。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-436">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="8eb6a-437">相依性插入可透過下列方式解決這些問題：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-437">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="8eb6a-438">使用介面或基底類別來將相依性資訊抽象化。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-438">The use of an interface or base class to abstract the dependency implementation.</span></span>
* <span data-ttu-id="8eb6a-439">在服務容器中註冊相依性。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-439">Registration of the dependency in a service container.</span></span> <span data-ttu-id="8eb6a-440">ASP.NET Core 提供內建服務容器 <xref:System.IServiceProvider>。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-440">ASP.NET Core provides a built-in service container, <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="8eb6a-441">服務會在應用程式的 `Startup.ConfigureServices` 方法中註冊。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-441">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="8eb6a-442">將服務「插入」\*\* 到服務使用位置之類別的建構函式。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-442">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="8eb6a-443">架構會負責建立相依性的執行個體，並在不再需要時將它捨棄。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-443">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="8eb6a-444">在[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples)中，`IMyDependency` 介面定義了服務提供給應用程式的方法：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-444">In the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

<span data-ttu-id="8eb6a-445">這個介面是由具象型別 `MyDependency` 所實作：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-445">This interface is implemented by a concrete type, `MyDependency`:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

<span data-ttu-id="8eb6a-446">`MyDependency` 在其建構函式中會要求 <xref:Microsoft.Extensions.Logging.ILogger`1>。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-446">`MyDependency` requests an <xref:Microsoft.Extensions.Logging.ILogger`1> in its constructor.</span></span> <span data-ttu-id="8eb6a-447">以鏈結方式使用相依性插入並非不尋常。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-447">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="8eb6a-448">每個要求的相依性接著會要求其自己的相依性。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-448">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="8eb6a-449">容器會解決圖形中的相依性，並傳回完全解析的服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-449">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="8eb6a-450">必須先解析的相依性集合組通常稱為「相依性樹狀結構」**、「相依性圖形」** 或「物件圖形」\*\*。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-450">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="8eb6a-451">`IMyDependency` 與 `ILogger<TCategoryName>` 必須在服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-451">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="8eb6a-452">`IMyDependency` 是在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-452">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8eb6a-453">`ILogger<TCategoryName>` 是由記錄抽象基礎結構所註冊，所以它是預設由架構所註冊的[架構提供的服務](#framework-provided-services)。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-453">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="8eb6a-454">容器會利用 [(泛型) 開放類型](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types) 解析 `ILogger<TCategoryName>`，讓您不需註冊每個 [(泛型) 建構的型別](/dotnet/csharp/language-reference/language-specification/types#constructed-types)：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-454">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<>), typeof(Logger<>));
```

<span data-ttu-id="8eb6a-455">在範例應用程式中，`IMyDependency` 服務是使用具象型別 `MyDependency` 所註冊。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-455">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="8eb6a-456">註冊會將服務的存留期範圍限制為單一要求的存留期。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-456">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="8eb6a-457">將在此主題稍後將說明[服務存留期](#service-lifetimes)。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-457">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> <span data-ttu-id="8eb6a-458">每個 `services.Add{SERVICE_NAME}` 擴充方法都會新增 (並且可能會設定) 服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-458">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="8eb6a-459">例如，會 `services.AddMvc()` 加入服務 Razor 頁面，而 MVC 則需要。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-459">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="8eb6a-460">建議應用程式遵循此慣例。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-460">We recommended that apps follow this convention.</span></span> <span data-ttu-id="8eb6a-461">在 [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) 命名空間中放置擴充方法，以封裝服務註冊群組。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-461">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="8eb6a-462">如果服務的建構函式需要[內建類型](/dotnet/csharp/language-reference/keywords/built-in-types-table)，例如 `string`，可以使用[組態](xref:fundamentals/configuration/index)或[選項模式](xref:fundamentals/configuration/options)插入該類型：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-462">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

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

<span data-ttu-id="8eb6a-463">服務執行個體是透過服務使用所在之類別建構函式所要求，而且會指派給私人欄位。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-463">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="8eb6a-464">該欄位會視需要用來存取整個類別中的服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-464">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="8eb6a-465">在範例應用程式中，會要求 `IMyDependency` 執行個體並使用它來呼叫服務的 `WriteMessage` 方法：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-465">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="services-injected-into-startup"></a><span data-ttu-id="8eb6a-466">插入至啟動的服務</span><span class="sxs-lookup"><span data-stu-id="8eb6a-466">Services injected into Startup</span></span>

<span data-ttu-id="8eb6a-467">`Startup`使用泛型主機（）時，只有下列服務類型可以插入至此函式 <xref:Microsoft.Extensions.Hosting.IHostBuilder> ：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-467">Only the following service types can be injected into the `Startup` constructor when using the Generic Host (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span></span>

* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

<span data-ttu-id="8eb6a-468">服務可以插入 `Startup.Configure` ：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-468">Services can be injected into `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> options)
{
    ...
}
```

<span data-ttu-id="8eb6a-469">如需詳細資訊，請參閱 <xref:fundamentals/startup> 。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-469">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="8eb6a-470">架構提供的服務</span><span class="sxs-lookup"><span data-stu-id="8eb6a-470">Framework-provided services</span></span>

<span data-ttu-id="8eb6a-471">`Startup.ConfigureServices`方法負責定義應用程式所使用的服務，包括平臺功能，例如 Entity Framework Core 和 ASP.NET CORE MVC。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-471">The `Startup.ConfigureServices` method is responsible for defining the services that the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="8eb6a-472">一開始， `IServiceCollection` 提供的會 `ConfigureServices` 根據主機的[設定方式](xref:fundamentals/index#host)，來擁有架構所定義的服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-472">Initially, the `IServiceCollection` provided to `ConfigureServices` has services defined by the framework depending on [how the host was configured](xref:fundamentals/index#host).</span></span> <span data-ttu-id="8eb6a-473">以 ASP.NET Core 範本為基礎的應用程式，在架構中註冊數百項服務並不常見。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-473">It's not uncommon for an app based on an ASP.NET Core template to have hundreds of services registered by the framework.</span></span> <span data-ttu-id="8eb6a-474">下表列出架構註冊服務的小型範例。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-474">A small sample of framework-registered services is listed in the following table.</span></span>

| <span data-ttu-id="8eb6a-475">服務類型</span><span class="sxs-lookup"><span data-stu-id="8eb6a-475">Service Type</span></span> | <span data-ttu-id="8eb6a-476">存留期</span><span class="sxs-lookup"><span data-stu-id="8eb6a-476">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="8eb6a-477">暫時性</span><span class="sxs-lookup"><span data-stu-id="8eb6a-477">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=fullName> | <span data-ttu-id="8eb6a-478">單一</span><span class="sxs-lookup"><span data-stu-id="8eb6a-478">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=fullName> | <span data-ttu-id="8eb6a-479">單一</span><span class="sxs-lookup"><span data-stu-id="8eb6a-479">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="8eb6a-480">單一</span><span class="sxs-lookup"><span data-stu-id="8eb6a-480">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="8eb6a-481">暫時性</span><span class="sxs-lookup"><span data-stu-id="8eb6a-481">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="8eb6a-482">單一</span><span class="sxs-lookup"><span data-stu-id="8eb6a-482">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="8eb6a-483">暫時性</span><span class="sxs-lookup"><span data-stu-id="8eb6a-483">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="8eb6a-484">單一</span><span class="sxs-lookup"><span data-stu-id="8eb6a-484">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="8eb6a-485">單一</span><span class="sxs-lookup"><span data-stu-id="8eb6a-485">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="8eb6a-486">單一</span><span class="sxs-lookup"><span data-stu-id="8eb6a-486">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="8eb6a-487">暫時性</span><span class="sxs-lookup"><span data-stu-id="8eb6a-487">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="8eb6a-488">單一</span><span class="sxs-lookup"><span data-stu-id="8eb6a-488">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="8eb6a-489">單一</span><span class="sxs-lookup"><span data-stu-id="8eb6a-489">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="8eb6a-490">單一</span><span class="sxs-lookup"><span data-stu-id="8eb6a-490">Singleton</span></span> |

## <a name="register-additional-services-with-extension-methods"></a><span data-ttu-id="8eb6a-491">以擴充方法註冊其他服務</span><span class="sxs-lookup"><span data-stu-id="8eb6a-491">Register additional services with extension methods</span></span>

<span data-ttu-id="8eb6a-492">當可以使用服務集合擴充方法來註冊服務 (如果需要，也可以註冊其相依服務) 時，慣例是使用單一 `Add{SERVICE_NAME}` 擴充方法來註冊該服務要求的所有服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-492">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="8eb6a-493">下列程式碼範例說明如何使用擴充方法[AddDbCoNtext \<TContext> ](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)和，將其他服務新增至容器 <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*> ：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-493">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) and <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>:</span></span>

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

<span data-ttu-id="8eb6a-494">如需詳細資訊，請參閱 API 文件中的 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> 類別。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-494">For more information, see the <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> class in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="8eb6a-495">服務存留期</span><span class="sxs-lookup"><span data-stu-id="8eb6a-495">Service lifetimes</span></span>

<span data-ttu-id="8eb6a-496">為每個已註冊的服務選擇適當的存留期。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-496">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="8eb6a-497">ASP.NET Core 服務可以使用下列存留期進行設定：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-497">ASP.NET Core services can be configured with the following lifetimes:</span></span>

### <a name="transient"></a><span data-ttu-id="8eb6a-498">暫時性</span><span class="sxs-lookup"><span data-stu-id="8eb6a-498">Transient</span></span>

<span data-ttu-id="8eb6a-499">每次從服務容器要求暫時性存留期服務 (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) 時都會建立它們。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-499">Transient lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) are created each time they're requested from the service container.</span></span> <span data-ttu-id="8eb6a-500">此存留期最適合用於輕量型的無狀態服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-500">This lifetime works best for lightweight, stateless services.</span></span>

### <a name="scoped"></a><span data-ttu-id="8eb6a-501">具範圍</span><span class="sxs-lookup"><span data-stu-id="8eb6a-501">Scoped</span></span>

<span data-ttu-id="8eb6a-502">具範圍存留期服務 (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) 會在每次用戶端要求 (連線) 時建立一次。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-502">Scoped lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) are created once per client request (connection).</span></span>

> [!WARNING]
> <span data-ttu-id="8eb6a-503">在中介軟體中使用具範圍服務時，請將該服務插入 `Invoke` 或 `InvokeAsync` 方法中。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-503">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="8eb6a-504">不要透過函式[插入](xref:mvc/controllers/dependency-injection#constructor-injection)來插入，因為它會強制服務的行為就像 singleton 一樣。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-504">Don't inject via [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="8eb6a-505">如需詳細資訊，請參閱 <xref:fundamentals/middleware/write#per-request-middleware-dependencies> 。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-505">For more information, see <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span></span>

### <a name="singleton"></a><span data-ttu-id="8eb6a-506">單一</span><span class="sxs-lookup"><span data-stu-id="8eb6a-506">Singleton</span></span>

<span data-ttu-id="8eb6a-507">當第一次收到有關單一資料庫存留期服務的要求時 (或是當執行 `Startup.ConfigureServices` 且隨著服務註冊指定執行個體時)，即會建立單一資料庫存留期服務 (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>)。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-507">Singleton lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) are created the first time they're requested (or when `Startup.ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="8eb6a-508">每個後續要求都會使用相同的執行個體。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-508">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="8eb6a-509">若應用程式要求單一資料庫行為，建議您允許服務容器管理服務的存留期。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-509">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="8eb6a-510">不要實作單一資料庫設計模式並為使用者提供可用來管理類別中物件存留期的程式碼。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-510">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="8eb6a-511">從單一資料庫解析具範圍服務是非常危險的。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-511">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="8eb6a-512">處理後續要求時，它可能會導致服務有不正確的狀態。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-512">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

## <a name="service-registration-methods"></a><span data-ttu-id="8eb6a-513">服務註冊方法</span><span class="sxs-lookup"><span data-stu-id="8eb6a-513">Service registration methods</span></span>

<span data-ttu-id="8eb6a-514">服務註冊擴充方法提供在特定案例中很有用的多載。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-514">Service registration extension methods offer overloads that are useful in specific scenarios.</span></span>

| <span data-ttu-id="8eb6a-515">方法</span><span class="sxs-lookup"><span data-stu-id="8eb6a-515">Method</span></span> | <span data-ttu-id="8eb6a-516">自動</span><span class="sxs-lookup"><span data-stu-id="8eb6a-516">Automatic</span></span><br><span data-ttu-id="8eb6a-517">物件 (object)</span><span class="sxs-lookup"><span data-stu-id="8eb6a-517">object</span></span><br><span data-ttu-id="8eb6a-518">處置</span><span class="sxs-lookup"><span data-stu-id="8eb6a-518">disposal</span></span> | <span data-ttu-id="8eb6a-519">多個</span><span class="sxs-lookup"><span data-stu-id="8eb6a-519">Multiple</span></span><br><span data-ttu-id="8eb6a-520">實作</span><span class="sxs-lookup"><span data-stu-id="8eb6a-520">implementations</span></span> | <span data-ttu-id="8eb6a-521">傳遞引數</span><span class="sxs-lookup"><span data-stu-id="8eb6a-521">Pass args</span></span> |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br><span data-ttu-id="8eb6a-522">範例：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-522">Example:</span></span><br>`services.AddSingleton<IMyDep, MyDep>();` | <span data-ttu-id="8eb6a-523">是</span><span class="sxs-lookup"><span data-stu-id="8eb6a-523">Yes</span></span> | <span data-ttu-id="8eb6a-524">是</span><span class="sxs-lookup"><span data-stu-id="8eb6a-524">Yes</span></span> | <span data-ttu-id="8eb6a-525">否</span><span class="sxs-lookup"><span data-stu-id="8eb6a-525">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br><span data-ttu-id="8eb6a-526">範例：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-526">Examples:</span></span><br>`services.AddSingleton<IMyDep>(sp => new MyDep());`<br>`services.AddSingleton<IMyDep>(sp => new MyDep("A string!"));` | <span data-ttu-id="8eb6a-527">是</span><span class="sxs-lookup"><span data-stu-id="8eb6a-527">Yes</span></span> | <span data-ttu-id="8eb6a-528">是</span><span class="sxs-lookup"><span data-stu-id="8eb6a-528">Yes</span></span> | <span data-ttu-id="8eb6a-529">是</span><span class="sxs-lookup"><span data-stu-id="8eb6a-529">Yes</span></span> |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br><span data-ttu-id="8eb6a-530">範例：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-530">Example:</span></span><br>`services.AddSingleton<MyDep>();` | <span data-ttu-id="8eb6a-531">是</span><span class="sxs-lookup"><span data-stu-id="8eb6a-531">Yes</span></span> | <span data-ttu-id="8eb6a-532">否</span><span class="sxs-lookup"><span data-stu-id="8eb6a-532">No</span></span> | <span data-ttu-id="8eb6a-533">否</span><span class="sxs-lookup"><span data-stu-id="8eb6a-533">No</span></span> |
| `AddSingleton<{SERVICE}>(new {IMPLEMENTATION})`<br><span data-ttu-id="8eb6a-534">範例：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-534">Examples:</span></span><br>`services.AddSingleton<IMyDep>(new MyDep());`<br>`services.AddSingleton<IMyDep>(new MyDep("A string!"));` | <span data-ttu-id="8eb6a-535">否</span><span class="sxs-lookup"><span data-stu-id="8eb6a-535">No</span></span> | <span data-ttu-id="8eb6a-536">是</span><span class="sxs-lookup"><span data-stu-id="8eb6a-536">Yes</span></span> | <span data-ttu-id="8eb6a-537">是</span><span class="sxs-lookup"><span data-stu-id="8eb6a-537">Yes</span></span> |
| `AddSingleton(new {IMPLEMENTATION})`<br><span data-ttu-id="8eb6a-538">範例：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-538">Examples:</span></span><br>`services.AddSingleton(new MyDep());`<br>`services.AddSingleton(new MyDep("A string!"));` | <span data-ttu-id="8eb6a-539">否</span><span class="sxs-lookup"><span data-stu-id="8eb6a-539">No</span></span> | <span data-ttu-id="8eb6a-540">否</span><span class="sxs-lookup"><span data-stu-id="8eb6a-540">No</span></span> | <span data-ttu-id="8eb6a-541">是</span><span class="sxs-lookup"><span data-stu-id="8eb6a-541">Yes</span></span> |

<span data-ttu-id="8eb6a-542">如需類型處置的詳細資訊，請參閱[＜服務處置＞](#disposal-of-services)一節。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-542">For more information on type disposal, see the [Disposal of services](#disposal-of-services) section.</span></span> <span data-ttu-id="8eb6a-543">多個實作的常見案例是[模擬測試類型](xref:test/integration-tests#inject-mock-services)。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-543">A common scenario for multiple implementations is [mocking types for testing](xref:test/integration-tests#inject-mock-services).</span></span>

<span data-ttu-id="8eb6a-544">如果還未註冊任何實作，則 `TryAdd{LIFETIME}` 方法只會註冊服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-544">`TryAdd{LIFETIME}` methods only register the service if there isn't already an implementation registered.</span></span>

<span data-ttu-id="8eb6a-545">在下列範例中，第一行會為 `IMyDependency` 註冊 `MyDependency`。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-545">In the following example, the first line registers `MyDependency` for `IMyDependency`.</span></span> <span data-ttu-id="8eb6a-546">第二行則沒有任何作用，因為 `IMyDependency` 已具有註冊的實作：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-546">The second line has no effect because `IMyDependency` already has a registered implementation:</span></span>

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

<span data-ttu-id="8eb6a-547">如需詳細資訊，請參閱</span><span class="sxs-lookup"><span data-stu-id="8eb6a-547">For more information, see:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

<span data-ttu-id="8eb6a-548">如果還沒有「相同類型的」\*\* 實作，則 [TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) 方法只會註冊服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-548">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) methods only register the service if there isn't already an implementation *of the same type*.</span></span> <span data-ttu-id="8eb6a-549">多個服務會透過 `IEnumerable<{SERVICE}>` 解析。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-549">Multiple services are resolved via `IEnumerable<{SERVICE}>`.</span></span> <span data-ttu-id="8eb6a-550">註冊服務時，如果尚未新增相同類型的執行個體，則開發人員只會希望新增執行個體。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-550">When registering services, the developer only wants to add an instance if one of the same type hasn't already been added.</span></span> <span data-ttu-id="8eb6a-551">程式庫作者通常會使用此方法來避免註冊容器中某個執行個體的兩個複本。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-551">Generally, this method is used by library authors to avoid registering two copies of an instance in the container.</span></span>

<span data-ttu-id="8eb6a-552">在下列範例中，第一行會為 `IMyDep1` 註冊 `MyDep`。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-552">In the following example, the first line registers `MyDep` for `IMyDep1`.</span></span> <span data-ttu-id="8eb6a-553">第二行會為 `IMyDep2` 註冊 `MyDep`。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-553">The second line registers `MyDep` for `IMyDep2`.</span></span> <span data-ttu-id="8eb6a-554">第三行則沒有任何作用，因為 `IMyDep1` 已具有 `MyDep` 的已註冊實作：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-554">The third line has no effect because `IMyDep1` already has a registered implementation of `MyDep`:</span></span>

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a><span data-ttu-id="8eb6a-555">建構函式插入行為</span><span class="sxs-lookup"><span data-stu-id="8eb6a-555">Constructor injection behavior</span></span>

<span data-ttu-id="8eb6a-556">服務可以透過兩個機制來解析：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-556">Services can be resolved by two mechanisms:</span></span>

* <xref:System.IServiceProvider>
* <span data-ttu-id="8eb6a-557"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities>：允許在相依性插入容器中不註冊服務時，建立物件。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-557"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities>: Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="8eb6a-558">搭配使用者面向抽象 (例如標籤協助程式、MVC 控制器與模型繫結器) 使用 `ActivatorUtilities`。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-558">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="8eb6a-559">建構函式可以接受不是由相依性插入提供的引數，但引數必須指派預設值。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-559">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="8eb6a-560">當服務由或解析時，程式的 `IServiceProvider` `ActivatorUtilities` [插入](xref:mvc/controllers/dependency-injection#constructor-injection)需要*公用*的函式。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-560">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) requires a *public* constructor.</span></span>

<span data-ttu-id="8eb6a-561">當服務由解析時 `ActivatorUtilities` ，程式的[插入](xref:mvc/controllers/dependency-injection#constructor-injection)只需要有一個適用的函式。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-561">When services are resolved by `ActivatorUtilities`, [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) requires that only one applicable constructor exists.</span></span> <span data-ttu-id="8eb6a-562">支援建構函式多載，但只能有一個多載存在，其引數可以藉由相依性插入而完成。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-562">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="8eb6a-563">Entity Framework 內容</span><span class="sxs-lookup"><span data-stu-id="8eb6a-563">Entity Framework contexts</span></span>

<span data-ttu-id="8eb6a-564">因為一般會將 Web 應用程式資料庫作業範圍設定為用戶端要求，所以通常會使用[具範圍存留期](#service-lifetimes)將 Entity Framework 內容新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-564">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="8eb6a-565">如果在註冊資料庫內容時， [AddDbCoNtext \<TContext> ](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)多載未指定存留期，則會將預設存留期設定為範圍。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-565">The default lifetime is scoped if a lifetime isn't specified by an [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) overload when registering the database context.</span></span> <span data-ttu-id="8eb6a-566">指定存留期的服務不應該使用存留期比服務還短的資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-566">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="8eb6a-567">留期和註冊選項</span><span class="sxs-lookup"><span data-stu-id="8eb6a-567">Lifetime and registration options</span></span>

<span data-ttu-id="8eb6a-568">為了示範這些存留期和註冊選項之間的差異，請考慮下列介面，以具有唯一識別碼 `OperationId` 的「作業」代表工作。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-568">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="8eb6a-569">視如何針對下列介面設定作業服務存留期而定，當類別要求時，容器會提供相同或不同的服務執行個體：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-569">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

<span data-ttu-id="8eb6a-570">介面是在 `Operation` 類別中實作。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-570">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="8eb6a-571">若未提供 GUID，`Operation` 建構函式會產生 GUID：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-571">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

<span data-ttu-id="8eb6a-572">會註冊相依於每種其他 `Operation` 類型的 `OperationService`。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-572">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="8eb6a-573">當透過相依性插入要求 `OperationService` 時，它會根據相依服務的存留期擷取每個服務的新執行個體或現有執行個體。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-573">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="8eb6a-574">當因收到來自容器的要求而建立暫時性服務時，`IOperationTransient` 服務的 `OperationId` 會與 `OperationService` 的 `OperationId` 不同。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-574">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="8eb6a-575">`OperationService` 會收到 `IOperationTransient` 類別的新執行個體。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-575">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="8eb6a-576">新執行個體會產生不同的 `OperationId`。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-576">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="8eb6a-577">當因用戶端要求而建立具範圍的服務時，`IOperationScoped` 服務與用戶端要求中 `OperationService` 的 `OperationId` 皆相同。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-577">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="8eb6a-578">在不同的用戶端要求中，兩個服務的 `OperationId` 值都不同。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-578">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="8eb6a-579">當建立一次 singleton 與 singleton 執行個體服務，並在所有用戶端要求與所有服務中使用時，`OperationId` 在所有服務要求中會固定不變。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-579">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

<span data-ttu-id="8eb6a-580">在 `Startup.ConfigureServices` 中，每個類型都會根據其具名存留期新增至容器：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-580">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

<span data-ttu-id="8eb6a-581">`IOperationSingletonInstance` 服務使用具有 `Guid.Empty` 已知識別碼的特定執行個體。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-581">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="8eb6a-582">很明顯此型別是使用中 (其 GUID 是所有區域)。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-582">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="8eb6a-583">範例應用程式示範個別要求內與個別要求之間的物件存留期。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-583">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="8eb6a-584">範例應用程式的 `IndexModel` 會要求每種 `IOperation` 型別與 `OperationService`。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-584">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="8eb6a-585">頁面接著會透過屬性指派來顯示所有頁面模型類別與服務的 `OperationId` 值：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-585">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

<span data-ttu-id="8eb6a-586">下面兩個輸出顯示兩個要求的結果：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-586">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="8eb6a-587">**第一個要求：**</span><span class="sxs-lookup"><span data-stu-id="8eb6a-587">**First request:**</span></span>

<span data-ttu-id="8eb6a-588">控制器作業：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-588">Controller operations:</span></span>

<span data-ttu-id="8eb6a-589">暫時性： d233e165-f417-469b-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="8eb6a-589">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="8eb6a-590">具範圍： 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="8eb6a-590">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="8eb6a-591">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="8eb6a-591">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="8eb6a-592">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="8eb6a-592">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="8eb6a-593">`OperationService` 作業：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-593">`OperationService` operations:</span></span>

<span data-ttu-id="8eb6a-594">暫時性： c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="8eb6a-594">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="8eb6a-595">具範圍： 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="8eb6a-595">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="8eb6a-596">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="8eb6a-596">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="8eb6a-597">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="8eb6a-597">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="8eb6a-598">**:第二個要求：**</span><span class="sxs-lookup"><span data-stu-id="8eb6a-598">**Second request:**</span></span>

<span data-ttu-id="8eb6a-599">控制器作業：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-599">Controller operations:</span></span>

<span data-ttu-id="8eb6a-600">暫時性： b63bd538-0a37-4ff1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="8eb6a-600">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="8eb6a-601">具範圍： 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="8eb6a-601">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="8eb6a-602">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="8eb6a-602">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="8eb6a-603">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="8eb6a-603">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="8eb6a-604">`OperationService` 作業：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-604">`OperationService` operations:</span></span>

<span data-ttu-id="8eb6a-605">暫時性： c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="8eb6a-605">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="8eb6a-606">具範圍： 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="8eb6a-606">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="8eb6a-607">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="8eb6a-607">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="8eb6a-608">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="8eb6a-608">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="8eb6a-609">觀察哪些 `OperationId` 值在要求內以及要求之間不同：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-609">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="8eb6a-610">「暫時性」\*\* 物件一律不同。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-610">*Transient* objects are always different.</span></span> <span data-ttu-id="8eb6a-611">第一個與第二個用戶端要求的暫時性 `OperationId` 值在兩個 `OperationService` 作業之間與各用戶端要求之間都不同。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-611">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="8eb6a-612">新執行個體會提供給每個服務要求以及用戶端要求。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-612">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="8eb6a-613">「具範圍」\*\* 物件在同一個用戶端要求內皆相同，但在不同的用戶端要求之間則不同。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-613">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="8eb6a-614">*Singleton*不論是否 `Operation` 在中提供實例，每個物件和每個要求的單一物件都是相同的 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-614">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `Startup.ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="8eb6a-615">從主要呼叫服務</span><span class="sxs-lookup"><span data-stu-id="8eb6a-615">Call services from main</span></span>

<span data-ttu-id="8eb6a-616">使用 [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) 建立 <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>，以解析應用程式範圍中的範圍服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-616">Create an <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="8eb6a-617">此法可用於在開機時存取範圍服務，以執行初始化工作。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-617">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="8eb6a-618">下列範例示範如何在 `Program.Main` 中取得 `MyScopedService`：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-618">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="8eb6a-619">範圍驗證</span><span class="sxs-lookup"><span data-stu-id="8eb6a-619">Scope validation</span></span>

<span data-ttu-id="8eb6a-620">當應用程式在開發環境中執行時，預設服務提供者會執行檢查以確認：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-620">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="8eb6a-621">範圍服務不是直接或間接由開機服務提供者解析。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-621">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="8eb6a-622">範圍服務不是直接或間接插入至單一服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-622">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="8eb6a-623">根服務提供者會在呼叫 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> 時建立。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-623">The root service provider is created when <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> is called.</span></span> <span data-ttu-id="8eb6a-624">當提供者啟動應用程式時，根服務提供者的存留期與應用程式/伺服器的存留期一致，並會在應用程式關閉時處置。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-624">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="8eb6a-625">範圍服務會由建立這些服務的容器處置。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-625">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="8eb6a-626">若是在根容器中建立範圍服務，因為當應用程式/伺服器關機時，服務只會由根容器處理，所以服務的存留期會提升為單一服務等級。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-626">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="8eb6a-627">當呼叫 `BuildServiceProvider` 時，驗證服務範圍會攔截到這些情況。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-627">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="8eb6a-628">如需詳細資訊，請參閱 <xref:fundamentals/host/web-host#scope-validation> 。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-628">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="8eb6a-629">要求服務</span><span class="sxs-lookup"><span data-stu-id="8eb6a-629">Request Services</span></span>

<span data-ttu-id="8eb6a-630">來自 `HttpContext`，在 ASP.NET Core 要求內提供的服務是透過 [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) 集合公開。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-630">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) collection.</span></span>

<span data-ttu-id="8eb6a-631">要求服務代表您在應用程式中設定和要求的服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-631">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="8eb6a-632">當物件指定相依性時，這些會由在 `RequestServices` 中找到的類型來滿足，而非 `ApplicationServices`。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-632">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="8eb6a-633">一般而言，應用程式不應該直接使用這些屬性。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-633">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="8eb6a-634">相反地，透過類別建構函式要求類別所需的型別，以及允許架構插入相依性。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-634">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="8eb6a-635">這會產生容易測試的類別。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-635">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="8eb6a-636">偏好要求相依性作為建構函式參數，而不要存取 `RequestServices` 集合。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-636">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="8eb6a-637">針對相依性插入設計服務</span><span class="sxs-lookup"><span data-stu-id="8eb6a-637">Design services for dependency injection</span></span>

<span data-ttu-id="8eb6a-638">最佳做法是：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-638">Best practices are to:</span></span>

* <span data-ttu-id="8eb6a-639">設計服務以使用相依性插入來取得其相依性。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-639">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="8eb6a-640">避免具狀態的靜態類別和成員。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-640">Avoid stateful, static classes and members.</span></span> <span data-ttu-id="8eb6a-641">將應用程式設計成使用單一服務，以避免建立全域狀態。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-641">Design apps to use singleton services instead, which avoid creating global state.</span></span>
* <span data-ttu-id="8eb6a-642">避免直接在服務內具現化相依類別。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-642">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="8eb6a-643">直接具現化會將程式碼耦合到特定實作。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-643">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="8eb6a-644">讓應用程式類別維持在小型、情況良好且可輕鬆測試的狀態。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-644">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="8eb6a-645">若類別有太多插入的相依性，這通常表示類別有太多責任而且正違反[單一責任原則 (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-645">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="8eb6a-646">將類別負責的某些部分移到新的類別，以嘗試重構類別。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-646">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="8eb6a-647">請記住，頁面 Razor 頁面模型類別和 MVC 控制器類別應該專注于 UI 考慮。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-647">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="8eb6a-648">商務規則和資料存取實作詳細資料應該保存在適合這些[分開考量](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) (Separation of Concerns) 類別中。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-648">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="8eb6a-649">處置服務</span><span class="sxs-lookup"><span data-stu-id="8eb6a-649">Disposal of services</span></span>

<span data-ttu-id="8eb6a-650">容器會為它建立的 <xref:System.IDisposable> 類型呼叫 <xref:System.IDisposable.Dispose*>。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-650">The container calls <xref:System.IDisposable.Dispose*> for the <xref:System.IDisposable> types it creates.</span></span> <span data-ttu-id="8eb6a-651">若執行個體由使用者程式碼新增到容器中，它將不會自動處置。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-651">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

<span data-ttu-id="8eb6a-652">在下列範例中，服務會建立服務容器並自動處置：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-652">In the following example, the services are created by the service container and disposed automatically:</span></span>

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

<span data-ttu-id="8eb6a-653">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="8eb6a-653">In the following example:</span></span>

* <span data-ttu-id="8eb6a-654">服務容器不會建立服務實例。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-654">The service instances aren't created by the service container.</span></span>
* <span data-ttu-id="8eb6a-655">架構不知道預期的服務存留期。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-655">The intended service lifetimes aren't known by the framework.</span></span>
* <span data-ttu-id="8eb6a-656">架構不會自動處置服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-656">The framework doesn't dispose of the services automatically.</span></span>
* <span data-ttu-id="8eb6a-657">如果服務未在開發人員程式碼中明確處置，它們會持續存在，直到應用程式關閉為止。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-657">If the services aren't explicitly disposed in developer code, they persist until the app shuts down.</span></span>

```csharp
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<Service1>(new Service1());
    services.AddSingleton(new Service2());
}
```

### <a name="idisposable-guidance-for-transient-and-shared-instances"></a><span data-ttu-id="8eb6a-658">暫時性和共用實例的 IDisposable 指引</span><span class="sxs-lookup"><span data-stu-id="8eb6a-658">IDisposable guidance for Transient and shared instances</span></span>

#### <a name="transient-limited-lifetime"></a><span data-ttu-id="8eb6a-659">暫時性，有限的存留期</span><span class="sxs-lookup"><span data-stu-id="8eb6a-659">Transient, limited lifetime</span></span>

<span data-ttu-id="8eb6a-660">**案例**</span><span class="sxs-lookup"><span data-stu-id="8eb6a-660">**Scenario**</span></span>

<span data-ttu-id="8eb6a-661">應用程式需要在 <xref:System.IDisposable> 下列任一情況下具有暫時性存留期的實例：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-661">The app requires an <xref:System.IDisposable> instance with a transient lifetime for either of the following scenarios:</span></span>

* <span data-ttu-id="8eb6a-662">實例會在根範圍中解析。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-662">The instance is resolved in the root scope.</span></span>
* <span data-ttu-id="8eb6a-663">應該在範圍結束之前處置實例。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-663">The instance should be disposed before the scope ends.</span></span>

<span data-ttu-id="8eb6a-664">**方案**</span><span class="sxs-lookup"><span data-stu-id="8eb6a-664">**Solution**</span></span>

<span data-ttu-id="8eb6a-665">使用 factory 模式，在父範圍外建立實例。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-665">Use the factory pattern to create an instance outside of the parent scope.</span></span> <span data-ttu-id="8eb6a-666">在這種情況下，應用程式通常會有 `Create` 方法，直接呼叫最終型別的函式。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-666">In this situation, the app would generally have a `Create` method that calls the final type's constructor directly.</span></span> <span data-ttu-id="8eb6a-667">如果最終類型具有其他相依性，則 factory 可以：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-667">If the final type has other dependencies, the factory can:</span></span>

* <span data-ttu-id="8eb6a-668"><xref:System.IServiceProvider>在其函式中接收。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-668">Receive an <xref:System.IServiceProvider> in its constructor.</span></span>
* <span data-ttu-id="8eb6a-669">使用 <xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities.CreateInstance%2A?displayProperty=nameWithType> 來具現化容器外的實例，同時使用容器來執行其相依性。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-669">Use <xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities.CreateInstance%2A?displayProperty=nameWithType> to instantiate the instance outside the container, while using the container for its dependencies.</span></span>

#### <a name="shared-instance-limited-lifetime"></a><span data-ttu-id="8eb6a-670">共用實例，有限的存留期</span><span class="sxs-lookup"><span data-stu-id="8eb6a-670">Shared Instance, limited lifetime</span></span>

<span data-ttu-id="8eb6a-671">**案例**</span><span class="sxs-lookup"><span data-stu-id="8eb6a-671">**Scenario**</span></span>

<span data-ttu-id="8eb6a-672">應用程式需要 <xref:System.IDisposable> 多個服務之間的共用實例，但 <xref:System.IDisposable> 應具有有限的存留期。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-672">The app requires a shared <xref:System.IDisposable> instance across multiple services, but the <xref:System.IDisposable> should have a limited lifetime.</span></span>

<span data-ttu-id="8eb6a-673">**方案**</span><span class="sxs-lookup"><span data-stu-id="8eb6a-673">**Solution**</span></span>

<span data-ttu-id="8eb6a-674">註冊具有限定範圍存留期的實例。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-674">Register the instance with a Scoped lifetime.</span></span> <span data-ttu-id="8eb6a-675">使用 <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope%2A?displayProperty=nameWithType> 來啟動並建立新的 <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> 。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-675">Use <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope%2A?displayProperty=nameWithType> to start and create a new <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>.</span></span> <span data-ttu-id="8eb6a-676">使用範圍的 <xref:System.IServiceProvider> 來取得所需的服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-676">Use the scope's <xref:System.IServiceProvider> to get required services.</span></span> <span data-ttu-id="8eb6a-677">在存留期結束時處置範圍。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-677">Dispose the scope when the lifetime should end.</span></span>

#### <a name="general-guidelines"></a><span data-ttu-id="8eb6a-678">一般準則</span><span class="sxs-lookup"><span data-stu-id="8eb6a-678">General Guidelines</span></span>

* <span data-ttu-id="8eb6a-679">不要向 <xref:System.IDisposable> 暫時性範圍註冊實例。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-679">Don't register <xref:System.IDisposable> instances with a Transient scope.</span></span> <span data-ttu-id="8eb6a-680">請改用 factory 模式。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-680">Use the factory pattern instead.</span></span>
* <span data-ttu-id="8eb6a-681">請勿解析 <xref:System.IDisposable> 根範圍中的暫時性或範圍實例。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-681">Don't resolve Transient or Scoped <xref:System.IDisposable> instances in the root scope.</span></span> <span data-ttu-id="8eb6a-682">唯一的例外狀況是當應用程式建立/重新建立和處置時 <xref:System.IServiceProvider> ，這不是理想的模式。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-682">The only general exception is when the app creates/recreates and disposes the <xref:System.IServiceProvider>, which isn't an ideal pattern.</span></span>
* <span data-ttu-id="8eb6a-683">透過 DI 接收相依性 <xref:System.IDisposable> 並不會要求接收者 <xref:System.IDisposable> 自行執行。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-683">Receiving an <xref:System.IDisposable> dependency via DI doesn't require that the receiver implement <xref:System.IDisposable> itself.</span></span> <span data-ttu-id="8eb6a-684">相依性的接收者不 <xref:System.IDisposable> 應呼叫 <xref:System.IDisposable.Dispose%2A> 該相依性上的。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-684">The receiver of the <xref:System.IDisposable> dependency shouldn't call <xref:System.IDisposable.Dispose%2A> on that dependency.</span></span>
* <span data-ttu-id="8eb6a-685">範圍應該用來控制服務的存留期。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-685">Scopes should be used to control lifetimes of services.</span></span> <span data-ttu-id="8eb6a-686">範圍不是階層式，而且範圍之間沒有特殊連接。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-686">Scopes aren't hierarchical, and there's no special connection among scopes.</span></span>

## <a name="default-service-container-replacement"></a><span data-ttu-id="8eb6a-687">預設服務容器取代</span><span class="sxs-lookup"><span data-stu-id="8eb6a-687">Default service container replacement</span></span>

<span data-ttu-id="8eb6a-688">內建的服務容器是設計用來滿足架構和大部分取用者應用程式的需求。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-688">The built-in service container is designed to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="8eb6a-689">我們建議使用內建容器，除非您需要內建容器不支援的特定功能，例如：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-689">We recommend using the built-in container unless you need a specific feature that the built-in container doesn't support, such as:</span></span>

* <span data-ttu-id="8eb6a-690">屬性插入</span><span class="sxs-lookup"><span data-stu-id="8eb6a-690">Property injection</span></span>
* <span data-ttu-id="8eb6a-691">根據名稱插入</span><span class="sxs-lookup"><span data-stu-id="8eb6a-691">Injection based on name</span></span>
* <span data-ttu-id="8eb6a-692">子容器</span><span class="sxs-lookup"><span data-stu-id="8eb6a-692">Child containers</span></span>
* <span data-ttu-id="8eb6a-693">自訂生命週期管理</span><span class="sxs-lookup"><span data-stu-id="8eb6a-693">Custom lifetime management</span></span>
* <span data-ttu-id="8eb6a-694">`Func<T>` 支援延遲初始設定</span><span class="sxs-lookup"><span data-stu-id="8eb6a-694">`Func<T>` support for lazy initialization</span></span>
* <span data-ttu-id="8eb6a-695">以慣例為基礎的註冊</span><span class="sxs-lookup"><span data-stu-id="8eb6a-695">Convention-based registration</span></span>

<span data-ttu-id="8eb6a-696">下列協力廠商容器可以與 ASP.NET Core 應用程式搭配使用：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-696">The following 3rd party containers can be used with ASP.NET Core apps:</span></span>

* [<span data-ttu-id="8eb6a-697">Autofac</span><span class="sxs-lookup"><span data-stu-id="8eb6a-697">Autofac</span></span>](https://autofac.readthedocs.io/en/latest/integration/aspnetcore.html)
* [<span data-ttu-id="8eb6a-698">DryIoc</span><span class="sxs-lookup"><span data-stu-id="8eb6a-698">DryIoc</span></span>](https://www.nuget.org/packages/DryIoc.Microsoft.DependencyInjection)
* [<span data-ttu-id="8eb6a-699">Grace</span><span class="sxs-lookup"><span data-stu-id="8eb6a-699">Grace</span></span>](https://www.nuget.org/packages/Grace.DependencyInjection.Extensions)
* [<span data-ttu-id="8eb6a-700">LightInject</span><span class="sxs-lookup"><span data-stu-id="8eb6a-700">LightInject</span></span>](https://github.com/seesharper/LightInject.Microsoft.DependencyInjection)
* [<span data-ttu-id="8eb6a-701">拉馬爾</span><span class="sxs-lookup"><span data-stu-id="8eb6a-701">Lamar</span></span>](https://jasperfx.github.io/lamar/)
* [<span data-ttu-id="8eb6a-702">Stashbox</span><span class="sxs-lookup"><span data-stu-id="8eb6a-702">Stashbox</span></span>](https://github.com/z4kn4fein/stashbox-extensions-dependencyinjection)
* [<span data-ttu-id="8eb6a-703">Unity</span><span class="sxs-lookup"><span data-stu-id="8eb6a-703">Unity</span></span>](https://www.nuget.org/packages/Unity.Microsoft.DependencyInjection)

### <a name="thread-safety"></a><span data-ttu-id="8eb6a-704">執行緒安全</span><span class="sxs-lookup"><span data-stu-id="8eb6a-704">Thread safety</span></span>

<span data-ttu-id="8eb6a-705">建立具備執行緒安全性的 singleton 服務。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-705">Create thread-safe singleton services.</span></span> <span data-ttu-id="8eb6a-706">如果 singleton 服務相依於暫時性服務，暫時性服務可能也需要具備執行緒安全性，取決於 singleton 如何使用它。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-706">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="8eb6a-707">單一服務的 factory 方法（例如[AddSingleton \<TService> （IServiceCollection，Func \<IServiceProvider,TService> ）](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*)的第二個引數）不需要是安全線程。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-707">The factory method of single service, such as the second argument to [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), doesn't need to be thread-safe.</span></span> <span data-ttu-id="8eb6a-708">就像型別 (`static`) 建構函式一樣，它一定會被單一執行緒呼叫一次。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-708">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="8eb6a-709">建議</span><span class="sxs-lookup"><span data-stu-id="8eb6a-709">Recommendations</span></span>

* <span data-ttu-id="8eb6a-710">不支援以 `async/await` 與 `Task` 為基礎的服務解析。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-710">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="8eb6a-711">C# 不支援非同步建構函式，因此建議的模式是以同步方式解析服務後使用非同步方法。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-711">C# does not support asynchronous constructors; therefore, the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="8eb6a-712">避免直接在服務容器中儲存資料與設定。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-712">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="8eb6a-713">例如，使用者的購物車通常不應該新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-713">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="8eb6a-714">組態應該使用[選項模式](xref:fundamentals/configuration/options)。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-714">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="8eb6a-715">同樣地，請避免只存在以允許存取某個其他物件的「資料持有者」物件。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-715">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="8eb6a-716">最好是透過 DI 要求實際項目。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-716">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="8eb6a-717">避免以靜態方式存取服務 (例如，以靜態方式設定 [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) 型別以四處使用)。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-717">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) for use elsewhere).</span></span>

* <span data-ttu-id="8eb6a-718">避免使用*服務定位器模式*，這會混用控制策略的[反轉](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion)。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-718">Avoid using the *service locator pattern*, which mixes [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

  * <span data-ttu-id="8eb6a-719"><xref:System.IServiceProvider.GetService*>當您可以改用 DI 時，請勿叫用來取得服務實例：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-719">Don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead:</span></span>

    <span data-ttu-id="8eb6a-720">**正確**</span><span class="sxs-lookup"><span data-stu-id="8eb6a-720">**Incorrect:**</span></span>

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

    <span data-ttu-id="8eb6a-721">**正確**：</span><span class="sxs-lookup"><span data-stu-id="8eb6a-721">**Correct**:</span></span>

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

  * <span data-ttu-id="8eb6a-722">避免在使用時，插入用來解析相依性的 factory <xref:System.IServiceProvider.GetService*> 。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-722">Avoid injecting a factory that resolves dependencies at runtime using <xref:System.IServiceProvider.GetService*>.</span></span>

* <span data-ttu-id="8eb6a-723">避免以靜態方式存取 `HttpContext` (例如 [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext))。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-723">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span></span>

<span data-ttu-id="8eb6a-724">就像所有的建議集，您可能會遇到需要忽略建議的情況。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-724">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="8eb6a-725">例外狀況很罕見，大部分是在架構本身內的特殊案例。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-725">Exceptions are rare, mostly special cases within the framework itself.</span></span>

<span data-ttu-id="8eb6a-726">DI 是靜態/全域物件存取模式的「替代」\*\* 選項。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-726">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="8eb6a-727">如果您將 DI 與靜態物件存取混合，則可能無法實現 DI 的優點。</span><span class="sxs-lookup"><span data-stu-id="8eb6a-727">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8eb6a-728">其他資源</span><span class="sxs-lookup"><span data-stu-id="8eb6a-728">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/fundamentals/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="8eb6a-729">在 ASP.NET Core 中處置 IDisposables 的四種方式</span><span class="sxs-lookup"><span data-stu-id="8eb6a-729">Four ways to dispose IDisposables in ASP.NET Core</span></span>](https://andrewlock.net/four-ways-to-dispose-idisposables-in-asp-net-core/)
* [<span data-ttu-id="8eb6a-730">在 ASP.NET Core 使用 Dependency Injection 撰寫簡潔的程式碼 (MSDN)</span><span class="sxs-lookup"><span data-stu-id="8eb6a-730">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="8eb6a-731">明確相依性準則</span><span class="sxs-lookup"><span data-stu-id="8eb6a-731">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* <span data-ttu-id="8eb6a-732">[逆轉控制容器和相依性插入模式 (Martin Fowler)](https://www.martinfowler.com/articles/injection.html) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="8eb6a-732">[Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)](https://www.martinfowler.com/articles/injection.html)</span></span>
* <span data-ttu-id="8eb6a-733">[How to register a service with multiple interfaces in ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/) (如何使用 ASP.NET Core DI 中的多個介面註冊服務)</span><span class="sxs-lookup"><span data-stu-id="8eb6a-733">[How to register a service with multiple interfaces in ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)</span></span>

::: moniker-end
