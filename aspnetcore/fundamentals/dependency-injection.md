---
<span data-ttu-id="537fe-101">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-101">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-102">'Blazor'</span></span>
- <span data-ttu-id="537fe-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-103">'Identity'</span></span>
- <span data-ttu-id="537fe-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-105">'Razor'</span></span>
- <span data-ttu-id="537fe-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-106">'SignalR' uid:</span></span> 

---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="537fe-107">.NET Core 中的相依性插入</span><span class="sxs-lookup"><span data-stu-id="537fe-107">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="537fe-108">作者：[Steve Smith](https://ardalis.com/) 和 [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="537fe-108">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="537fe-109">ASP.NET Core 支援相依性插入 (DI) 軟體設計模式，這是用來在類別與其相依性之間達成[控制權反轉 (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) \(英文\) 的技術。</span><span class="sxs-lookup"><span data-stu-id="537fe-109">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="537fe-110">如需有關 MVC 控制器內相依性插入的特定詳細資訊，請參閱 <xref:mvc/controllers/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="537fe-110">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="537fe-111">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="537fe-111">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="537fe-112">相依性插入概觀</span><span class="sxs-lookup"><span data-stu-id="537fe-112">Overview of dependency injection</span></span>

<span data-ttu-id="537fe-113">「相依性」\*\* 是另一個物件所需的任何物件。</span><span class="sxs-lookup"><span data-stu-id="537fe-113">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="537fe-114">檢查具有 `WriteMessage` 方法的下列 `MyDependency` 物件，應用程式中的其他類別相依於此物件：</span><span class="sxs-lookup"><span data-stu-id="537fe-114">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

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

<span data-ttu-id="537fe-115">您可以建立 `MyDependency` 類別的執行個體，讓 `WriteMessage` 方法可供類別使用。</span><span class="sxs-lookup"><span data-stu-id="537fe-115">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="537fe-116">`MyDependency` 類別是 `IndexModel` 類別的相依性：</span><span class="sxs-lookup"><span data-stu-id="537fe-116">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

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

<span data-ttu-id="537fe-117">該類別會建立 `MyDependency` 執行個體並直接相依於該執行個體。</span><span class="sxs-lookup"><span data-stu-id="537fe-117">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="537fe-118">程式碼相依性 (例如前一個範例) 有問題，因此基於下列原因應該避免使用：</span><span class="sxs-lookup"><span data-stu-id="537fe-118">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="537fe-119">若要將 `MyDependency` 取代為不同的實作，必須修改該類別。</span><span class="sxs-lookup"><span data-stu-id="537fe-119">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="537fe-120">若 `MyDependency` 有相依性，那些相依性必須由該類別設定。</span><span class="sxs-lookup"><span data-stu-id="537fe-120">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="537fe-121">在具有多個相依於 `MyDependency` 之多個類別的大型專案中，設定程式碼在不同的應用程式之間會變得鬆散。</span><span class="sxs-lookup"><span data-stu-id="537fe-121">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="537fe-122">此實作難以進行單元測試。</span><span class="sxs-lookup"><span data-stu-id="537fe-122">This implementation is difficult to unit test.</span></span> <span data-ttu-id="537fe-123">應用程式應該使用模擬 (Mock) 或虛設常式 (Stub) `MyDependency` 類別，這在使用此方法時無法使用。</span><span class="sxs-lookup"><span data-stu-id="537fe-123">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="537fe-124">相依性插入可透過下列方式解決這些問題：</span><span class="sxs-lookup"><span data-stu-id="537fe-124">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="537fe-125">使用介面或基底類別來將相依性資訊抽象化。</span><span class="sxs-lookup"><span data-stu-id="537fe-125">The use of an interface or base class to abstract the dependency implementation.</span></span>
* <span data-ttu-id="537fe-126">在服務容器中註冊相依性。</span><span class="sxs-lookup"><span data-stu-id="537fe-126">Registration of the dependency in a service container.</span></span> <span data-ttu-id="537fe-127">ASP.NET Core 提供內建服務容器 <xref:System.IServiceProvider>。</span><span class="sxs-lookup"><span data-stu-id="537fe-127">ASP.NET Core provides a built-in service container, <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="537fe-128">服務會在應用程式的 `Startup.ConfigureServices` 方法中註冊。</span><span class="sxs-lookup"><span data-stu-id="537fe-128">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="537fe-129">將服務「插入」\*\* 到服務使用位置之類別的建構函式。</span><span class="sxs-lookup"><span data-stu-id="537fe-129">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="537fe-130">架構會負責建立相依性的執行個體，並在不再需要時將它捨棄。</span><span class="sxs-lookup"><span data-stu-id="537fe-130">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="537fe-131">在[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples)中，`IMyDependency` 介面定義了服務提供給應用程式的方法：</span><span class="sxs-lookup"><span data-stu-id="537fe-131">In the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

<span data-ttu-id="537fe-132">這個介面是由具象型別 `MyDependency` 所實作：</span><span class="sxs-lookup"><span data-stu-id="537fe-132">This interface is implemented by a concrete type, `MyDependency`:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

<span data-ttu-id="537fe-133">`MyDependency` 在其建構函式中會要求 <xref:Microsoft.Extensions.Logging.ILogger`1>。</span><span class="sxs-lookup"><span data-stu-id="537fe-133">`MyDependency` requests an <xref:Microsoft.Extensions.Logging.ILogger`1> in its constructor.</span></span> <span data-ttu-id="537fe-134">以鏈結方式使用相依性插入並非不尋常。</span><span class="sxs-lookup"><span data-stu-id="537fe-134">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="537fe-135">每個要求的相依性接著會要求其自己的相依性。</span><span class="sxs-lookup"><span data-stu-id="537fe-135">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="537fe-136">容器會解決圖形中的相依性，並傳回完全解析的服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-136">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="537fe-137">必須先解析的相依性集合組通常稱為「相依性樹狀結構」**、「相依性圖形」** 或「物件圖形」\*\*。</span><span class="sxs-lookup"><span data-stu-id="537fe-137">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="537fe-138">`IMyDependency` 與 `ILogger<TCategoryName>` 必須在服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="537fe-138">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="537fe-139">`IMyDependency` 是在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="537fe-139">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="537fe-140">`ILogger<TCategoryName>` 是由記錄抽象基礎結構所註冊，所以它是預設由架構所註冊的[架構提供的服務](#framework-provided-services)。</span><span class="sxs-lookup"><span data-stu-id="537fe-140">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="537fe-141">容器會利用 [(泛型) 開放類型](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types) 解析 `ILogger<TCategoryName>`，讓您不需註冊每個 [(泛型) 建構的型別](/dotnet/csharp/language-reference/language-specification/types#constructed-types)：</span><span class="sxs-lookup"><span data-stu-id="537fe-141">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<>), typeof(Logger<>));
```

<span data-ttu-id="537fe-142">在範例應用程式中，`IMyDependency` 服務是使用具象型別 `MyDependency` 所註冊。</span><span class="sxs-lookup"><span data-stu-id="537fe-142">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="537fe-143">註冊會將服務的存留期範圍限制為單一要求的存留期。</span><span class="sxs-lookup"><span data-stu-id="537fe-143">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="537fe-144">將在此主題稍後將說明[服務存留期](#service-lifetimes)。</span><span class="sxs-lookup"><span data-stu-id="537fe-144">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> <span data-ttu-id="537fe-145">每個 `services.Add{SERVICE_NAME}` 擴充方法都會新增 (並且可能會設定) 服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-145">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="537fe-146">例如，會 `services.AddMvc()` 加入服務 Razor 頁面，而 MVC 則需要。</span><span class="sxs-lookup"><span data-stu-id="537fe-146">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="537fe-147">建議應用程式遵循此慣例。</span><span class="sxs-lookup"><span data-stu-id="537fe-147">We recommended that apps follow this convention.</span></span> <span data-ttu-id="537fe-148">在 [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) 命名空間中放置擴充方法，以封裝服務註冊群組。</span><span class="sxs-lookup"><span data-stu-id="537fe-148">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="537fe-149">如果服務的建構函式需要[內建類型](/dotnet/csharp/language-reference/keywords/built-in-types-table)，例如 `string`，可以使用[組態](xref:fundamentals/configuration/index)或[選項模式](xref:fundamentals/configuration/options)插入該類型：</span><span class="sxs-lookup"><span data-stu-id="537fe-149">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

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

<span data-ttu-id="537fe-150">服務執行個體是透過服務使用所在之類別建構函式所要求，而且會指派給私人欄位。</span><span class="sxs-lookup"><span data-stu-id="537fe-150">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="537fe-151">該欄位會視需要用來存取整個類別中的服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-151">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="537fe-152">在範例應用程式中，會要求 `IMyDependency` 執行個體並使用它來呼叫服務的 `WriteMessage` 方法：</span><span class="sxs-lookup"><span data-stu-id="537fe-152">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="services-injected-into-startup"></a><span data-ttu-id="537fe-153">插入至啟動的服務</span><span class="sxs-lookup"><span data-stu-id="537fe-153">Services injected into Startup</span></span>

<span data-ttu-id="537fe-154">`Startup`使用泛型主機（）時，只有下列服務類型可以插入至此函式 <xref:Microsoft.Extensions.Hosting.IHostBuilder> ：</span><span class="sxs-lookup"><span data-stu-id="537fe-154">Only the following service types can be injected into the `Startup` constructor when using the Generic Host (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span></span>

* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

<span data-ttu-id="537fe-155">服務可以插入 `Startup.Configure` ：</span><span class="sxs-lookup"><span data-stu-id="537fe-155">Services can be injected into `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> options)
{
    ...
}
```

<span data-ttu-id="537fe-156">如需詳細資訊，請參閱<xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="537fe-156">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="537fe-157">架構提供的服務</span><span class="sxs-lookup"><span data-stu-id="537fe-157">Framework-provided services</span></span>

<span data-ttu-id="537fe-158">`Startup.ConfigureServices`方法負責定義應用程式所使用的服務，包括平臺功能，例如 Entity Framework Core 和 ASP.NET CORE MVC。</span><span class="sxs-lookup"><span data-stu-id="537fe-158">The `Startup.ConfigureServices` method is responsible for defining the services that the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="537fe-159">一開始， `IServiceCollection` 提供的會 `ConfigureServices` 根據主機的[設定方式](xref:fundamentals/index#host)，來擁有架構所定義的服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-159">Initially, the `IServiceCollection` provided to `ConfigureServices` has services defined by the framework depending on [how the host was configured](xref:fundamentals/index#host).</span></span> <span data-ttu-id="537fe-160">以 ASP.NET Core 範本為基礎的應用程式，在架構中註冊數百項服務並不常見。</span><span class="sxs-lookup"><span data-stu-id="537fe-160">It's not uncommon for an app based on an ASP.NET Core template to have hundreds of services registered by the framework.</span></span> <span data-ttu-id="537fe-161">下表列出架構註冊服務的小型範例。</span><span class="sxs-lookup"><span data-stu-id="537fe-161">A small sample of framework-registered services is listed in the following table.</span></span>

| <span data-ttu-id="537fe-162">服務類型</span><span class="sxs-lookup"><span data-stu-id="537fe-162">Service Type</span></span> | <span data-ttu-id="537fe-163">存留期</span><span class="sxs-lookup"><span data-stu-id="537fe-163">Lifetime</span></span> |
| ---
<span data-ttu-id="537fe-164">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-164">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-165">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-165">'Blazor'</span></span>
- <span data-ttu-id="537fe-166">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-166">'Identity'</span></span>
- <span data-ttu-id="537fe-167">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-167">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-168">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-168">'Razor'</span></span>
- <span data-ttu-id="537fe-169">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-169">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-170">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-170">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-171">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-171">'Blazor'</span></span>
- <span data-ttu-id="537fe-172">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-172">'Identity'</span></span>
- <span data-ttu-id="537fe-173">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-173">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-174">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-174">'Razor'</span></span>
- <span data-ttu-id="537fe-175">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-175">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-176">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-176">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-177">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-177">'Blazor'</span></span>
- <span data-ttu-id="537fe-178">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-178">'Identity'</span></span>
- <span data-ttu-id="537fe-179">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-179">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-180">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-180">'Razor'</span></span>
- <span data-ttu-id="537fe-181">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-181">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-182">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-182">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-183">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-183">'Blazor'</span></span>
- <span data-ttu-id="537fe-184">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-184">'Identity'</span></span>
- <span data-ttu-id="537fe-185">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-185">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-186">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-186">'Razor'</span></span>
- <span data-ttu-id="537fe-187">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-187">'SignalR' uid:</span></span> 

<span data-ttu-id="537fe-188">------ |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-188">------ | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-189">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-189">'Blazor'</span></span>
- <span data-ttu-id="537fe-190">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-190">'Identity'</span></span>
- <span data-ttu-id="537fe-191">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-191">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-192">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-192">'Razor'</span></span>
- <span data-ttu-id="537fe-193">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-193">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-194">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-194">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-195">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-195">'Blazor'</span></span>
- <span data-ttu-id="537fe-196">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-196">'Identity'</span></span>
- <span data-ttu-id="537fe-197">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-197">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-198">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-198">'Razor'</span></span>
- <span data-ttu-id="537fe-199">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-199">'SignalR' uid:</span></span> 

<span data-ttu-id="537fe-200">---- || <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> |暫時性 || `IHostApplicationLifetime` |Singleton || `IWebHostEnvironment` |Singleton || <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> |Singleton || <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> |暫時性 || <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> |Singleton || <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> |暫時性 || <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> |Singleton || <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> |Singleton || <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> |Singleton || <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> |暫時性 || <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> |Singleton || <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> |Singleton || <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> |Singleton |</span><span class="sxs-lookup"><span data-stu-id="537fe-200">---- | | <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | Transient | | `IHostApplicationLifetime` | Singleton | | `IWebHostEnvironment` | Singleton | | <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | Singleton | | <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | Transient | | <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | Singleton | | <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | Transient | | <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | Singleton | | <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | Singleton | | <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | Singleton | | <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | Transient | | <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | Singleton | | <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | Singleton | | <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | Singleton |</span></span>

## <a name="register-additional-services-with-extension-methods"></a><span data-ttu-id="537fe-201">以擴充方法註冊其他服務</span><span class="sxs-lookup"><span data-stu-id="537fe-201">Register additional services with extension methods</span></span>

<span data-ttu-id="537fe-202">當可以使用服務集合擴充方法來註冊服務 (如果需要，也可以註冊其相依服務) 時，慣例是使用單一 `Add{SERVICE_NAME}` 擴充方法來註冊該服務要求的所有服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-202">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="537fe-203">下列程式碼範例說明如何使用擴充方法[AddDbCoNtext \<TContext> ](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)和，將其他服務新增至容器 <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*> ：</span><span class="sxs-lookup"><span data-stu-id="537fe-203">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) and <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>:</span></span>

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

<span data-ttu-id="537fe-204">如需詳細資訊，請參閱 API 文件中的 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> 類別。</span><span class="sxs-lookup"><span data-stu-id="537fe-204">For more information, see the <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> class in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="537fe-205">執行個體存留期</span><span class="sxs-lookup"><span data-stu-id="537fe-205">Service lifetimes</span></span>

<span data-ttu-id="537fe-206">為每個已註冊的服務選擇適當的存留期。</span><span class="sxs-lookup"><span data-stu-id="537fe-206">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="537fe-207">ASP.NET Core 服務可以使用下列存留期進行設定：</span><span class="sxs-lookup"><span data-stu-id="537fe-207">ASP.NET Core services can be configured with the following lifetimes:</span></span>

### <a name="transient"></a><span data-ttu-id="537fe-208">暫時性</span><span class="sxs-lookup"><span data-stu-id="537fe-208">Transient</span></span>

<span data-ttu-id="537fe-209">每次從服務容器要求暫時性存留期服務 (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) 時都會建立它們。</span><span class="sxs-lookup"><span data-stu-id="537fe-209">Transient lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) are created each time they're requested from the service container.</span></span> <span data-ttu-id="537fe-210">此存留期最適合用於輕量型的無狀態服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-210">This lifetime works best for lightweight, stateless services.</span></span>

### <a name="scoped"></a><span data-ttu-id="537fe-211">具範圍</span><span class="sxs-lookup"><span data-stu-id="537fe-211">Scoped</span></span>

<span data-ttu-id="537fe-212">具範圍存留期服務 (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) 會在每次用戶端要求 (連線) 時建立一次。</span><span class="sxs-lookup"><span data-stu-id="537fe-212">Scoped lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) are created once per client request (connection).</span></span>

> [!WARNING]
> <span data-ttu-id="537fe-213">在中介軟體中使用具範圍服務時，請將該服務插入 `Invoke` 或 `InvokeAsync` 方法中。</span><span class="sxs-lookup"><span data-stu-id="537fe-213">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="537fe-214">不要透過函式[插入](xref:mvc/controllers/dependency-injection#constructor-injection)來插入，因為它會強制服務的行為就像 singleton 一樣。</span><span class="sxs-lookup"><span data-stu-id="537fe-214">Don't inject via [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="537fe-215">如需詳細資訊，請參閱<xref:fundamentals/middleware/write#per-request-middleware-dependencies>。</span><span class="sxs-lookup"><span data-stu-id="537fe-215">For more information, see <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span></span>

### <a name="singleton"></a><span data-ttu-id="537fe-216">單一</span><span class="sxs-lookup"><span data-stu-id="537fe-216">Singleton</span></span>

<span data-ttu-id="537fe-217">當第一次收到有關單一資料庫存留期服務的要求時 (或是當執行 `Startup.ConfigureServices` 且隨著服務註冊指定執行個體時)，即會建立單一資料庫存留期服務 (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>)。</span><span class="sxs-lookup"><span data-stu-id="537fe-217">Singleton lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) are created the first time they're requested (or when `Startup.ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="537fe-218">每個後續要求都會使用相同的執行個體。</span><span class="sxs-lookup"><span data-stu-id="537fe-218">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="537fe-219">若應用程式要求單一資料庫行為，建議您允許服務容器管理服務的存留期。</span><span class="sxs-lookup"><span data-stu-id="537fe-219">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="537fe-220">不要實作單一資料庫設計模式並為使用者提供可用來管理類別中物件存留期的程式碼。</span><span class="sxs-lookup"><span data-stu-id="537fe-220">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="537fe-221">從單一資料庫解析具範圍服務是非常危險的。</span><span class="sxs-lookup"><span data-stu-id="537fe-221">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="537fe-222">處理後續要求時，它可能會導致服務有不正確的狀態。</span><span class="sxs-lookup"><span data-stu-id="537fe-222">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

## <a name="service-registration-methods"></a><span data-ttu-id="537fe-223">服務註冊方法</span><span class="sxs-lookup"><span data-stu-id="537fe-223">Service registration methods</span></span>

<span data-ttu-id="537fe-224">服務註冊擴充方法提供在特定案例中很有用的多載。</span><span class="sxs-lookup"><span data-stu-id="537fe-224">Service registration extension methods offer overloads that are useful in specific scenarios.</span></span>

| <span data-ttu-id="537fe-225">方法</span><span class="sxs-lookup"><span data-stu-id="537fe-225">Method</span></span> | <span data-ttu-id="537fe-226">自動</span><span class="sxs-lookup"><span data-stu-id="537fe-226">Automatic</span></span><br><span data-ttu-id="537fe-227">物件</span><span class="sxs-lookup"><span data-stu-id="537fe-227">object</span></span><br><span data-ttu-id="537fe-228">處置</span><span class="sxs-lookup"><span data-stu-id="537fe-228">disposal</span></span> | <span data-ttu-id="537fe-229">多重</span><span class="sxs-lookup"><span data-stu-id="537fe-229">Multiple</span></span><br><span data-ttu-id="537fe-230">實作</span><span class="sxs-lookup"><span data-stu-id="537fe-230">implementations</span></span> | <span data-ttu-id="537fe-231">傳遞引數</span><span class="sxs-lookup"><span data-stu-id="537fe-231">Pass args</span></span> |
| ---
<span data-ttu-id="537fe-232">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-232">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-233">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-233">'Blazor'</span></span>
- <span data-ttu-id="537fe-234">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-234">'Identity'</span></span>
- <span data-ttu-id="537fe-235">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-235">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-236">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-236">'Razor'</span></span>
- <span data-ttu-id="537fe-237">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-237">'SignalR' uid:</span></span> 

<span data-ttu-id="537fe-238">--- |：---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-238">--- | :--- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-239">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-239">'Blazor'</span></span>
- <span data-ttu-id="537fe-240">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-240">'Identity'</span></span>
- <span data-ttu-id="537fe-241">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-241">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-242">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-242">'Razor'</span></span>
- <span data-ttu-id="537fe-243">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-243">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-244">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-244">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-245">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-245">'Blazor'</span></span>
- <span data-ttu-id="537fe-246">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-246">'Identity'</span></span>
- <span data-ttu-id="537fe-247">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-247">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-248">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-248">'Razor'</span></span>
- <span data-ttu-id="537fe-249">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-249">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-250">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-250">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-251">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-251">'Blazor'</span></span>
- <span data-ttu-id="537fe-252">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-252">'Identity'</span></span>
- <span data-ttu-id="537fe-253">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-253">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-254">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-254">'Razor'</span></span>
- <span data-ttu-id="537fe-255">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-255">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-256">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-256">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-257">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-257">'Blazor'</span></span>
- <span data-ttu-id="537fe-258">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-258">'Identity'</span></span>
- <span data-ttu-id="537fe-259">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-259">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-260">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-260">'Razor'</span></span>
- <span data-ttu-id="537fe-261">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-261">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-262">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-262">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-263">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-263">'Blazor'</span></span>
- <span data-ttu-id="537fe-264">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-264">'Identity'</span></span>
- <span data-ttu-id="537fe-265">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-265">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-266">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-266">'Razor'</span></span>
- <span data-ttu-id="537fe-267">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-267">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-268">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-268">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-269">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-269">'Blazor'</span></span>
- <span data-ttu-id="537fe-270">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-270">'Identity'</span></span>
- <span data-ttu-id="537fe-271">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-271">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-272">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-272">'Razor'</span></span>
- <span data-ttu-id="537fe-273">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-273">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-274">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-274">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-275">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-275">'Blazor'</span></span>
- <span data-ttu-id="537fe-276">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-276">'Identity'</span></span>
- <span data-ttu-id="537fe-277">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-277">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-278">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-278">'Razor'</span></span>
- <span data-ttu-id="537fe-279">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-279">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-280">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-280">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-281">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-281">'Blazor'</span></span>
- <span data-ttu-id="537fe-282">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-282">'Identity'</span></span>
- <span data-ttu-id="537fe-283">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-283">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-284">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-284">'Razor'</span></span>
- <span data-ttu-id="537fe-285">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-285">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-286">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-286">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-287">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-287">'Blazor'</span></span>
- <span data-ttu-id="537fe-288">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-288">'Identity'</span></span>
- <span data-ttu-id="537fe-289">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-289">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-290">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-290">'Razor'</span></span>
- <span data-ttu-id="537fe-291">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-291">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-292">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-292">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-293">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-293">'Blazor'</span></span>
- <span data-ttu-id="537fe-294">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-294">'Identity'</span></span>
- <span data-ttu-id="537fe-295">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-295">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-296">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-296">'Razor'</span></span>
- <span data-ttu-id="537fe-297">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-297">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-298">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-298">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-299">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-299">'Blazor'</span></span>
- <span data-ttu-id="537fe-300">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-300">'Identity'</span></span>
- <span data-ttu-id="537fe-301">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-301">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-302">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-302">'Razor'</span></span>
- <span data-ttu-id="537fe-303">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-303">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-304">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-304">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-305">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-305">'Blazor'</span></span>
- <span data-ttu-id="537fe-306">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-306">'Identity'</span></span>
- <span data-ttu-id="537fe-307">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-307">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-308">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-308">'Razor'</span></span>
- <span data-ttu-id="537fe-309">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-309">'SignalR' uid:</span></span> 

<span data-ttu-id="537fe-310">---------------: |：---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-310">---------------: | :--- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-311">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-311">'Blazor'</span></span>
- <span data-ttu-id="537fe-312">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-312">'Identity'</span></span>
- <span data-ttu-id="537fe-313">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-313">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-314">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-314">'Razor'</span></span>
- <span data-ttu-id="537fe-315">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-315">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-316">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-316">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-317">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-317">'Blazor'</span></span>
- <span data-ttu-id="537fe-318">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-318">'Identity'</span></span>
- <span data-ttu-id="537fe-319">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-319">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-320">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-320">'Razor'</span></span>
- <span data-ttu-id="537fe-321">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-321">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-322">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-322">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-323">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-323">'Blazor'</span></span>
- <span data-ttu-id="537fe-324">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-324">'Identity'</span></span>
- <span data-ttu-id="537fe-325">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-325">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-326">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-326">'Razor'</span></span>
- <span data-ttu-id="537fe-327">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-327">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-328">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-328">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-329">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-329">'Blazor'</span></span>
- <span data-ttu-id="537fe-330">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-330">'Identity'</span></span>
- <span data-ttu-id="537fe-331">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-331">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-332">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-332">'Razor'</span></span>
- <span data-ttu-id="537fe-333">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-333">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-334">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-334">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-335">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-335">'Blazor'</span></span>
- <span data-ttu-id="537fe-336">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-336">'Identity'</span></span>
- <span data-ttu-id="537fe-337">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-337">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-338">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-338">'Razor'</span></span>
- <span data-ttu-id="537fe-339">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-339">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-340">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-340">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-341">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-341">'Blazor'</span></span>
- <span data-ttu-id="537fe-342">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-342">'Identity'</span></span>
- <span data-ttu-id="537fe-343">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-343">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-344">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-344">'Razor'</span></span>
- <span data-ttu-id="537fe-345">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-345">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-346">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-346">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-347">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-347">'Blazor'</span></span>
- <span data-ttu-id="537fe-348">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-348">'Identity'</span></span>
- <span data-ttu-id="537fe-349">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-349">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-350">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-350">'Razor'</span></span>
- <span data-ttu-id="537fe-351">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-351">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-352">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-352">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-353">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-353">'Blazor'</span></span>
- <span data-ttu-id="537fe-354">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-354">'Identity'</span></span>
- <span data-ttu-id="537fe-355">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-355">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-356">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-356">'Razor'</span></span>
- <span data-ttu-id="537fe-357">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-357">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-358">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-358">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-359">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-359">'Blazor'</span></span>
- <span data-ttu-id="537fe-360">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-360">'Identity'</span></span>
- <span data-ttu-id="537fe-361">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-361">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-362">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-362">'Razor'</span></span>
- <span data-ttu-id="537fe-363">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-363">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-364">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-364">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-365">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-365">'Blazor'</span></span>
- <span data-ttu-id="537fe-366">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-366">'Identity'</span></span>
- <span data-ttu-id="537fe-367">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-367">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-368">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-368">'Razor'</span></span>
- <span data-ttu-id="537fe-369">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-369">'SignalR' uid:</span></span> 

<span data-ttu-id="537fe-370">-------------: |：---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-370">-------------: | :--- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-371">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-371">'Blazor'</span></span>
- <span data-ttu-id="537fe-372">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-372">'Identity'</span></span>
- <span data-ttu-id="537fe-373">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-373">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-374">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-374">'Razor'</span></span>
- <span data-ttu-id="537fe-375">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-375">'SignalR' uid:</span></span> 

<span data-ttu-id="537fe-376">----: | | `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`</span><span class="sxs-lookup"><span data-stu-id="537fe-376">----: | | `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`</span></span><br><span data-ttu-id="537fe-377">範例：</span><span class="sxs-lookup"><span data-stu-id="537fe-377">Example:</span></span><br><span data-ttu-id="537fe-378">`services.AddSingleton<IMyDep, MyDep>();`|是 |是 |否 | |`Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`</span><span class="sxs-lookup"><span data-stu-id="537fe-378">`services.AddSingleton<IMyDep, MyDep>();` | Yes | Yes | No | | `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`</span></span><br><span data-ttu-id="537fe-379">範例：</span><span class="sxs-lookup"><span data-stu-id="537fe-379">Examples:</span></span><br>`services.AddSingleton<IMyDep>(sp => new MyDep());`<br><span data-ttu-id="537fe-380">`services.AddSingleton<IMyDep>(sp => new MyDep("A string!"));`|是 |是 |是 | |`Add{LIFETIME}<{IMPLEMENTATION}>()`</span><span class="sxs-lookup"><span data-stu-id="537fe-380">`services.AddSingleton<IMyDep>(sp => new MyDep("A string!"));` | Yes | Yes | Yes | | `Add{LIFETIME}<{IMPLEMENTATION}>()`</span></span><br><span data-ttu-id="537fe-381">範例：</span><span class="sxs-lookup"><span data-stu-id="537fe-381">Example:</span></span><br><span data-ttu-id="537fe-382">`services.AddSingleton<MyDep>();`|是 |否 |否 | |`AddSingleton<{SERVICE}>(new {IMPLEMENTATION})`</span><span class="sxs-lookup"><span data-stu-id="537fe-382">`services.AddSingleton<MyDep>();` | Yes | No | No | | `AddSingleton<{SERVICE}>(new {IMPLEMENTATION})`</span></span><br><span data-ttu-id="537fe-383">範例：</span><span class="sxs-lookup"><span data-stu-id="537fe-383">Examples:</span></span><br>`services.AddSingleton<IMyDep>(new MyDep());`<br><span data-ttu-id="537fe-384">`services.AddSingleton<IMyDep>(new MyDep("A string!"));`|否 |是 |是 | |`AddSingleton(new {IMPLEMENTATION})`</span><span class="sxs-lookup"><span data-stu-id="537fe-384">`services.AddSingleton<IMyDep>(new MyDep("A string!"));` | No | Yes | Yes | | `AddSingleton(new {IMPLEMENTATION})`</span></span><br><span data-ttu-id="537fe-385">範例：</span><span class="sxs-lookup"><span data-stu-id="537fe-385">Examples:</span></span><br>`services.AddSingleton(new MyDep());`<br><span data-ttu-id="537fe-386">`services.AddSingleton(new MyDep("A string!"));`|否 |否 |是 |</span><span class="sxs-lookup"><span data-stu-id="537fe-386">`services.AddSingleton(new MyDep("A string!"));` | No | No | Yes |</span></span>

<span data-ttu-id="537fe-387">如需類型處置的詳細資訊，請參閱[＜服務處置＞](#disposal-of-services)一節。</span><span class="sxs-lookup"><span data-stu-id="537fe-387">For more information on type disposal, see the [Disposal of services](#disposal-of-services) section.</span></span> <span data-ttu-id="537fe-388">多個實作的常見案例是[模擬測試類型](xref:test/integration-tests#inject-mock-services)。</span><span class="sxs-lookup"><span data-stu-id="537fe-388">A common scenario for multiple implementations is [mocking types for testing](xref:test/integration-tests#inject-mock-services).</span></span>

<span data-ttu-id="537fe-389">如果還未註冊任何實作，則 `TryAdd{LIFETIME}` 方法只會註冊服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-389">`TryAdd{LIFETIME}` methods only register the service if there isn't already an implementation registered.</span></span>

<span data-ttu-id="537fe-390">在下列範例中，第一行會為 `IMyDependency` 註冊 `MyDependency`。</span><span class="sxs-lookup"><span data-stu-id="537fe-390">In the following example, the first line registers `MyDependency` for `IMyDependency`.</span></span> <span data-ttu-id="537fe-391">第二行則沒有任何作用，因為 `IMyDependency` 已具有註冊的實作：</span><span class="sxs-lookup"><span data-stu-id="537fe-391">The second line has no effect because `IMyDependency` already has a registered implementation:</span></span>

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

<span data-ttu-id="537fe-392">如需詳細資訊，請參閱</span><span class="sxs-lookup"><span data-stu-id="537fe-392">For more information, see:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

<span data-ttu-id="537fe-393">如果還沒有「相同類型的」\*\* 實作，則 [TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) 方法只會註冊服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-393">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) methods only register the service if there isn't already an implementation *of the same type*.</span></span> <span data-ttu-id="537fe-394">多個服務會透過 `IEnumerable<{SERVICE}>` 解析。</span><span class="sxs-lookup"><span data-stu-id="537fe-394">Multiple services are resolved via `IEnumerable<{SERVICE}>`.</span></span> <span data-ttu-id="537fe-395">註冊服務時，如果尚未新增相同類型的執行個體，則開發人員只會希望新增執行個體。</span><span class="sxs-lookup"><span data-stu-id="537fe-395">When registering services, the developer only wants to add an instance if one of the same type hasn't already been added.</span></span> <span data-ttu-id="537fe-396">程式庫作者通常會使用此方法來避免註冊容器中某個執行個體的兩個複本。</span><span class="sxs-lookup"><span data-stu-id="537fe-396">Generally, this method is used by library authors to avoid registering two copies of an instance in the container.</span></span>

<span data-ttu-id="537fe-397">在下列範例中，第一行會為 `IMyDep1` 註冊 `MyDep`。</span><span class="sxs-lookup"><span data-stu-id="537fe-397">In the following example, the first line registers `MyDep` for `IMyDep1`.</span></span> <span data-ttu-id="537fe-398">第二行會為 `IMyDep2` 註冊 `MyDep`。</span><span class="sxs-lookup"><span data-stu-id="537fe-398">The second line registers `MyDep` for `IMyDep2`.</span></span> <span data-ttu-id="537fe-399">第三行則沒有任何作用，因為 `IMyDep1` 已具有 `MyDep` 的已註冊實作：</span><span class="sxs-lookup"><span data-stu-id="537fe-399">The third line has no effect because `IMyDep1` already has a registered implementation of `MyDep`:</span></span>

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a><span data-ttu-id="537fe-400">建構函式插入行為</span><span class="sxs-lookup"><span data-stu-id="537fe-400">Constructor injection behavior</span></span>

<span data-ttu-id="537fe-401">服務可以透過兩個機制來解析：</span><span class="sxs-lookup"><span data-stu-id="537fe-401">Services can be resolved by two mechanisms:</span></span>

* <xref:System.IServiceProvider>
* <span data-ttu-id="537fe-402"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities>：允許在相依性插入容器中不註冊服務時，建立物件。</span><span class="sxs-lookup"><span data-stu-id="537fe-402"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities>: Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="537fe-403">搭配使用者面向抽象 (例如標籤協助程式、MVC 控制器與模型繫結器) 使用 `ActivatorUtilities`。</span><span class="sxs-lookup"><span data-stu-id="537fe-403">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="537fe-404">建構函式可以接受不是由相依性插入提供的引數，但引數必須指派預設值。</span><span class="sxs-lookup"><span data-stu-id="537fe-404">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="537fe-405">當服務由或解析時，程式的 `IServiceProvider` `ActivatorUtilities` [插入](xref:mvc/controllers/dependency-injection#constructor-injection)需要*公用*的函式。</span><span class="sxs-lookup"><span data-stu-id="537fe-405">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) requires a *public* constructor.</span></span>

<span data-ttu-id="537fe-406">當服務由解析時 `ActivatorUtilities` ，程式的[插入](xref:mvc/controllers/dependency-injection#constructor-injection)只需要有一個適用的函式。</span><span class="sxs-lookup"><span data-stu-id="537fe-406">When services are resolved by `ActivatorUtilities`, [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) requires that only one applicable constructor exists.</span></span> <span data-ttu-id="537fe-407">支援建構函式多載，但只能有一個多載存在，其引數可以藉由相依性插入而完成。</span><span class="sxs-lookup"><span data-stu-id="537fe-407">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="537fe-408">Entity Framework 內容</span><span class="sxs-lookup"><span data-stu-id="537fe-408">Entity Framework contexts</span></span>

<span data-ttu-id="537fe-409">因為一般會將 Web 應用程式資料庫作業範圍設定為用戶端要求，所以通常會使用[具範圍存留期](#service-lifetimes)將 Entity Framework 內容新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="537fe-409">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="537fe-410">如果在註冊資料庫內容時， [AddDbCoNtext \<TContext> ](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)多載未指定存留期，則會將預設存留期設定為範圍。</span><span class="sxs-lookup"><span data-stu-id="537fe-410">The default lifetime is scoped if a lifetime isn't specified by an [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) overload when registering the database context.</span></span> <span data-ttu-id="537fe-411">指定存留期的服務不應該使用存留期比服務還短的資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="537fe-411">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="537fe-412">留期和註冊選項</span><span class="sxs-lookup"><span data-stu-id="537fe-412">Lifetime and registration options</span></span>

<span data-ttu-id="537fe-413">為了示範這些存留期和註冊選項之間的差異，請考慮下列介面，以具有唯一識別碼 `OperationId` 的「作業」代表工作。</span><span class="sxs-lookup"><span data-stu-id="537fe-413">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="537fe-414">視如何針對下列介面設定作業服務存留期而定，當類別要求時，容器會提供相同或不同的服務執行個體：</span><span class="sxs-lookup"><span data-stu-id="537fe-414">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

<span data-ttu-id="537fe-415">介面是在 `Operation` 類別中實作。</span><span class="sxs-lookup"><span data-stu-id="537fe-415">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="537fe-416">若未提供 GUID，`Operation` 建構函式會產生 GUID：</span><span class="sxs-lookup"><span data-stu-id="537fe-416">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

<span data-ttu-id="537fe-417">會註冊相依於每種其他 `Operation` 類型的 `OperationService`。</span><span class="sxs-lookup"><span data-stu-id="537fe-417">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="537fe-418">當透過相依性插入要求 `OperationService` 時，它會根據相依服務的存留期擷取每個服務的新執行個體或現有執行個體。</span><span class="sxs-lookup"><span data-stu-id="537fe-418">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="537fe-419">當因收到來自容器的要求而建立暫時性服務時，`IOperationTransient` 服務的 `OperationId` 會與 `OperationService` 的 `OperationId` 不同。</span><span class="sxs-lookup"><span data-stu-id="537fe-419">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="537fe-420">`OperationService` 會收到 `IOperationTransient` 類別的新執行個體。</span><span class="sxs-lookup"><span data-stu-id="537fe-420">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="537fe-421">新執行個體會產生不同的 `OperationId`。</span><span class="sxs-lookup"><span data-stu-id="537fe-421">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="537fe-422">當因用戶端要求而建立具範圍的服務時，`IOperationScoped` 服務與用戶端要求中 `OperationService` 的 `OperationId` 皆相同。</span><span class="sxs-lookup"><span data-stu-id="537fe-422">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="537fe-423">在不同的用戶端要求中，兩個服務的 `OperationId` 值都不同。</span><span class="sxs-lookup"><span data-stu-id="537fe-423">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="537fe-424">當建立一次 singleton 與 singleton 執行個體服務，並在所有用戶端要求與所有服務中使用時，`OperationId` 在所有服務要求中會固定不變。</span><span class="sxs-lookup"><span data-stu-id="537fe-424">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

<span data-ttu-id="537fe-425">在 `Startup.ConfigureServices` 中，每個類型都會根據其具名存留期新增至容器：</span><span class="sxs-lookup"><span data-stu-id="537fe-425">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

<span data-ttu-id="537fe-426">`IOperationSingletonInstance` 服務使用具有 `Guid.Empty` 已知識別碼的特定執行個體。</span><span class="sxs-lookup"><span data-stu-id="537fe-426">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="537fe-427">很明顯此型別是使用中 (其 GUID 是所有區域)。</span><span class="sxs-lookup"><span data-stu-id="537fe-427">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="537fe-428">範例應用程式示範個別要求內與個別要求之間的物件存留期。</span><span class="sxs-lookup"><span data-stu-id="537fe-428">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="537fe-429">範例應用程式的 `IndexModel` 會要求每種 `IOperation` 型別與 `OperationService`。</span><span class="sxs-lookup"><span data-stu-id="537fe-429">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="537fe-430">頁面接著會透過屬性指派來顯示所有頁面模型類別與服務的 `OperationId` 值：</span><span class="sxs-lookup"><span data-stu-id="537fe-430">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

<span data-ttu-id="537fe-431">下面兩個輸出顯示兩個要求的結果：</span><span class="sxs-lookup"><span data-stu-id="537fe-431">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="537fe-432">**第一個要求：**</span><span class="sxs-lookup"><span data-stu-id="537fe-432">**First request:**</span></span>

<span data-ttu-id="537fe-433">控制器作業：</span><span class="sxs-lookup"><span data-stu-id="537fe-433">Controller operations:</span></span>

<span data-ttu-id="537fe-434">暫時性： d233e165-f417-469b-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="537fe-434">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="537fe-435">具範圍： 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="537fe-435">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="537fe-436">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="537fe-436">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="537fe-437">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="537fe-437">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="537fe-438">`OperationService` 作業：</span><span class="sxs-lookup"><span data-stu-id="537fe-438">`OperationService` operations:</span></span>

<span data-ttu-id="537fe-439">暫時性： c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="537fe-439">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="537fe-440">具範圍： 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="537fe-440">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="537fe-441">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="537fe-441">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="537fe-442">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="537fe-442">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="537fe-443">**:第二個要求：**</span><span class="sxs-lookup"><span data-stu-id="537fe-443">**Second request:**</span></span>

<span data-ttu-id="537fe-444">控制器作業：</span><span class="sxs-lookup"><span data-stu-id="537fe-444">Controller operations:</span></span>

<span data-ttu-id="537fe-445">暫時性： b63bd538-0a37-4ff1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="537fe-445">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="537fe-446">具範圍： 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="537fe-446">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="537fe-447">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="537fe-447">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="537fe-448">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="537fe-448">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="537fe-449">`OperationService` 作業：</span><span class="sxs-lookup"><span data-stu-id="537fe-449">`OperationService` operations:</span></span>

<span data-ttu-id="537fe-450">暫時性： c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="537fe-450">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="537fe-451">具範圍： 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="537fe-451">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="537fe-452">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="537fe-452">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="537fe-453">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="537fe-453">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="537fe-454">觀察哪些 `OperationId` 值在要求內以及要求之間不同：</span><span class="sxs-lookup"><span data-stu-id="537fe-454">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="537fe-455">「暫時性」\*\* 物件一律不同。</span><span class="sxs-lookup"><span data-stu-id="537fe-455">*Transient* objects are always different.</span></span> <span data-ttu-id="537fe-456">第一個與第二個用戶端要求的暫時性 `OperationId` 值在兩個 `OperationService` 作業之間與各用戶端要求之間都不同。</span><span class="sxs-lookup"><span data-stu-id="537fe-456">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="537fe-457">新執行個體會提供給每個服務要求以及用戶端要求。</span><span class="sxs-lookup"><span data-stu-id="537fe-457">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="537fe-458">「具範圍」\*\* 物件在同一個用戶端要求內皆相同，但在不同的用戶端要求之間則不同。</span><span class="sxs-lookup"><span data-stu-id="537fe-458">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="537fe-459">*Singleton*不論是否 `Operation` 在中提供實例，每個物件和每個要求的單一物件都是相同的 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="537fe-459">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `Startup.ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="537fe-460">從主要呼叫服務</span><span class="sxs-lookup"><span data-stu-id="537fe-460">Call services from main</span></span>

<span data-ttu-id="537fe-461">使用 [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) 建立 <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>，以解析應用程式範圍中的範圍服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-461">Create an <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="537fe-462">此法可用於在開機時存取範圍服務，以執行初始化工作。</span><span class="sxs-lookup"><span data-stu-id="537fe-462">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="537fe-463">下列範例示範如何在 `Program.Main` 中取得 `MyScopedService`：</span><span class="sxs-lookup"><span data-stu-id="537fe-463">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="537fe-464">範圍驗證</span><span class="sxs-lookup"><span data-stu-id="537fe-464">Scope validation</span></span>

<span data-ttu-id="537fe-465">當應用程式在開發環境中執行並呼叫[CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings)來建立主機時，預設服務提供者會執行檢查以確認：</span><span class="sxs-lookup"><span data-stu-id="537fe-465">When the app is running in the Development environment and calls [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) to build the host, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="537fe-466">範圍服務不是直接或間接由開機服務提供者解析。</span><span class="sxs-lookup"><span data-stu-id="537fe-466">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="537fe-467">範圍服務不是直接或間接插入至單一服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-467">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="537fe-468">根服務提供者會在呼叫 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> 時建立。</span><span class="sxs-lookup"><span data-stu-id="537fe-468">The root service provider is created when <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> is called.</span></span> <span data-ttu-id="537fe-469">當提供者啟動應用程式時，根服務提供者的存留期與應用程式/伺服器的存留期一致，並會在應用程式關閉時處置。</span><span class="sxs-lookup"><span data-stu-id="537fe-469">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="537fe-470">範圍服務會由建立這些服務的容器處置。</span><span class="sxs-lookup"><span data-stu-id="537fe-470">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="537fe-471">若是在根容器中建立範圍服務，因為當應用程式/伺服器關機時，服務只會由根容器處理，所以服務的存留期會提升為單一服務等級。</span><span class="sxs-lookup"><span data-stu-id="537fe-471">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="537fe-472">當呼叫 `BuildServiceProvider` 時，驗證服務範圍會攔截到這些情況。</span><span class="sxs-lookup"><span data-stu-id="537fe-472">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="537fe-473">如需詳細資訊，請參閱<xref:fundamentals/host/web-host#scope-validation>。</span><span class="sxs-lookup"><span data-stu-id="537fe-473">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="537fe-474">要求服務</span><span class="sxs-lookup"><span data-stu-id="537fe-474">Request Services</span></span>

<span data-ttu-id="537fe-475">來自 `HttpContext`，在 ASP.NET Core 要求內提供的服務是透過 [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) 集合公開。</span><span class="sxs-lookup"><span data-stu-id="537fe-475">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) collection.</span></span>

<span data-ttu-id="537fe-476">要求服務代表您在應用程式中設定和要求的服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-476">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="537fe-477">當物件指定相依性時，這些會由在 `RequestServices` 中找到的類型來滿足，而非 `ApplicationServices`。</span><span class="sxs-lookup"><span data-stu-id="537fe-477">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="537fe-478">一般而言，應用程式不應該直接使用這些屬性。</span><span class="sxs-lookup"><span data-stu-id="537fe-478">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="537fe-479">相反地，請透過類別的函式要求類別所需的類型，並允許架構插入相依性。</span><span class="sxs-lookup"><span data-stu-id="537fe-479">Instead, request the types that classes require via class constructors and allow the framework to inject the dependencies.</span></span> <span data-ttu-id="537fe-480">這會產生容易測試的類別。</span><span class="sxs-lookup"><span data-stu-id="537fe-480">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="537fe-481">偏好要求相依性作為建構函式參數，而不要存取 `RequestServices` 集合。</span><span class="sxs-lookup"><span data-stu-id="537fe-481">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="537fe-482">針對相依性插入設計服務</span><span class="sxs-lookup"><span data-stu-id="537fe-482">Design services for dependency injection</span></span>

<span data-ttu-id="537fe-483">最佳做法是：</span><span class="sxs-lookup"><span data-stu-id="537fe-483">Best practices are to:</span></span>

* <span data-ttu-id="537fe-484">設計服務以使用相依性插入來取得其相依性。</span><span class="sxs-lookup"><span data-stu-id="537fe-484">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="537fe-485">避免具狀態的靜態類別和成員。</span><span class="sxs-lookup"><span data-stu-id="537fe-485">Avoid stateful, static classes and members.</span></span> <span data-ttu-id="537fe-486">將應用程式設計成使用單一服務，以避免建立全域狀態。</span><span class="sxs-lookup"><span data-stu-id="537fe-486">Design apps to use singleton services instead, which avoid creating global state.</span></span>
* <span data-ttu-id="537fe-487">避免直接在服務內具現化相依類別。</span><span class="sxs-lookup"><span data-stu-id="537fe-487">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="537fe-488">直接具現化會將程式碼耦合到特定實作。</span><span class="sxs-lookup"><span data-stu-id="537fe-488">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="537fe-489">讓應用程式類別維持在小型、情況良好且可輕鬆測試的狀態。</span><span class="sxs-lookup"><span data-stu-id="537fe-489">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="537fe-490">若類別有太多插入的相依性，這通常表示類別有太多責任而且正違反[單一責任原則 (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="537fe-490">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="537fe-491">將類別負責的某些部分移到新的類別，以嘗試重構類別。</span><span class="sxs-lookup"><span data-stu-id="537fe-491">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="537fe-492">請記住，頁面 Razor 頁面模型類別和 MVC 控制器類別應該專注于 UI 考慮。</span><span class="sxs-lookup"><span data-stu-id="537fe-492">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="537fe-493">商務規則和資料存取實作詳細資料應該保存在適合這些[分開考量](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) (Separation of Concerns) 類別中。</span><span class="sxs-lookup"><span data-stu-id="537fe-493">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="537fe-494">處置服務</span><span class="sxs-lookup"><span data-stu-id="537fe-494">Disposal of services</span></span>

<span data-ttu-id="537fe-495">容器會為它建立的 <xref:System.IDisposable> 類型呼叫 <xref:System.IDisposable.Dispose*>。</span><span class="sxs-lookup"><span data-stu-id="537fe-495">The container calls <xref:System.IDisposable.Dispose*> for the <xref:System.IDisposable> types it creates.</span></span> <span data-ttu-id="537fe-496">若執行個體由使用者程式碼新增到容器中，它將不會自動處置。</span><span class="sxs-lookup"><span data-stu-id="537fe-496">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

<span data-ttu-id="537fe-497">在下列範例中，服務會建立服務容器並自動處置：</span><span class="sxs-lookup"><span data-stu-id="537fe-497">In the following example, the services are created by the service container and disposed automatically:</span></span>

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

<span data-ttu-id="537fe-498">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="537fe-498">In the following example:</span></span>

* <span data-ttu-id="537fe-499">服務容器不會建立服務實例。</span><span class="sxs-lookup"><span data-stu-id="537fe-499">The service instances aren't created by the service container.</span></span>
* <span data-ttu-id="537fe-500">架構不知道預期的服務存留期。</span><span class="sxs-lookup"><span data-stu-id="537fe-500">The intended service lifetimes aren't known by the framework.</span></span>
* <span data-ttu-id="537fe-501">架構不會自動處置服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-501">The framework doesn't dispose of the services automatically.</span></span>
* <span data-ttu-id="537fe-502">如果服務未在開發人員程式碼中明確處置，它們會持續存在，直到應用程式關閉為止。</span><span class="sxs-lookup"><span data-stu-id="537fe-502">If the services aren't explicitly disposed in developer code, they persist until the app shuts down.</span></span>

```csharp
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<Service1>(new Service1());
    services.AddSingleton(new Service2());
}
```

<span data-ttu-id="537fe-503">如需服務處置選項的討論，請參閱[在 ASP.NET Core 中處置 IDisposables 的四種方式](https://andrewlock.net/four-ways-to-dispose-idisposables-in-asp-net-core/)。</span><span class="sxs-lookup"><span data-stu-id="537fe-503">For a discussion of service disposal options, see [Four ways to dispose IDisposables in ASP.NET Core](https://andrewlock.net/four-ways-to-dispose-idisposables-in-asp-net-core/).</span></span>

## <a name="default-service-container-replacement"></a><span data-ttu-id="537fe-504">預設服務容器取代</span><span class="sxs-lookup"><span data-stu-id="537fe-504">Default service container replacement</span></span>

<span data-ttu-id="537fe-505">內建的服務容器是設計用來滿足架構和大部分取用者應用程式的需求。</span><span class="sxs-lookup"><span data-stu-id="537fe-505">The built-in service container is designed to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="537fe-506">我們建議使用內建容器，除非您需要內建容器不支援的特定功能，例如：</span><span class="sxs-lookup"><span data-stu-id="537fe-506">We recommend using the built-in container unless you need a specific feature that the built-in container doesn't support, such as:</span></span>

* <span data-ttu-id="537fe-507">屬性插入</span><span class="sxs-lookup"><span data-stu-id="537fe-507">Property injection</span></span>
* <span data-ttu-id="537fe-508">根據名稱插入</span><span class="sxs-lookup"><span data-stu-id="537fe-508">Injection based on name</span></span>
* <span data-ttu-id="537fe-509">子容器</span><span class="sxs-lookup"><span data-stu-id="537fe-509">Child containers</span></span>
* <span data-ttu-id="537fe-510">自訂生命週期管理</span><span class="sxs-lookup"><span data-stu-id="537fe-510">Custom lifetime management</span></span>
* <span data-ttu-id="537fe-511">`Func<T>` 支援延遲初始設定</span><span class="sxs-lookup"><span data-stu-id="537fe-511">`Func<T>` support for lazy initialization</span></span>
* <span data-ttu-id="537fe-512">以慣例為基礎的註冊</span><span class="sxs-lookup"><span data-stu-id="537fe-512">Convention-based registration</span></span>

<span data-ttu-id="537fe-513">下列協力廠商容器可以與 ASP.NET Core 應用程式搭配使用：</span><span class="sxs-lookup"><span data-stu-id="537fe-513">The following 3rd party containers can be used with ASP.NET Core apps:</span></span>

* [<span data-ttu-id="537fe-514">Autofac</span><span class="sxs-lookup"><span data-stu-id="537fe-514">Autofac</span></span>](https://autofac.readthedocs.io/en/latest/integration/aspnetcore.html)
* [<span data-ttu-id="537fe-515">DryIoc</span><span class="sxs-lookup"><span data-stu-id="537fe-515">DryIoc</span></span>](https://www.nuget.org/packages/DryIoc.Microsoft.DependencyInjection)
* [<span data-ttu-id="537fe-516">Grace</span><span class="sxs-lookup"><span data-stu-id="537fe-516">Grace</span></span>](https://www.nuget.org/packages/Grace.DependencyInjection.Extensions)
* [<span data-ttu-id="537fe-517">LightInject</span><span class="sxs-lookup"><span data-stu-id="537fe-517">LightInject</span></span>](https://github.com/seesharper/LightInject.Microsoft.DependencyInjection)
* [<span data-ttu-id="537fe-518">拉馬爾</span><span class="sxs-lookup"><span data-stu-id="537fe-518">Lamar</span></span>](https://jasperfx.github.io/lamar/)
* [<span data-ttu-id="537fe-519">Stashbox</span><span class="sxs-lookup"><span data-stu-id="537fe-519">Stashbox</span></span>](https://github.com/z4kn4fein/stashbox-extensions-dependencyinjection)
* [<span data-ttu-id="537fe-520">Unity</span><span class="sxs-lookup"><span data-stu-id="537fe-520">Unity</span></span>](https://www.nuget.org/packages/Unity.Microsoft.DependencyInjection)

### <a name="thread-safety"></a><span data-ttu-id="537fe-521">執行緒安全性</span><span class="sxs-lookup"><span data-stu-id="537fe-521">Thread safety</span></span>

<span data-ttu-id="537fe-522">建立具備執行緒安全性的 singleton 服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-522">Create thread-safe singleton services.</span></span> <span data-ttu-id="537fe-523">如果 singleton 服務相依於暫時性服務，暫時性服務可能也需要具備執行緒安全性，取決於 singleton 如何使用它。</span><span class="sxs-lookup"><span data-stu-id="537fe-523">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="537fe-524">單一服務的 factory 方法（例如[AddSingleton \<TService> （IServiceCollection，Func \<IServiceProvider,TService> ）](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*)的第二個引數）不需要是安全線程。</span><span class="sxs-lookup"><span data-stu-id="537fe-524">The factory method of single service, such as the second argument to [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), doesn't need to be thread-safe.</span></span> <span data-ttu-id="537fe-525">就像型別 (`static`) 建構函式一樣，它一定會被單一執行緒呼叫一次。</span><span class="sxs-lookup"><span data-stu-id="537fe-525">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="537fe-526">建議</span><span class="sxs-lookup"><span data-stu-id="537fe-526">Recommendations</span></span>

* <span data-ttu-id="537fe-527">不支援以 `async/await` 與 `Task` 為基礎的服務解析。</span><span class="sxs-lookup"><span data-stu-id="537fe-527">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="537fe-528">C# 不支援非同步建構函式，因此建議的模式是以同步方式解析服務後使用非同步方法。</span><span class="sxs-lookup"><span data-stu-id="537fe-528">C# does not support asynchronous constructors; therefore, the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="537fe-529">避免直接在服務容器中儲存資料與設定。</span><span class="sxs-lookup"><span data-stu-id="537fe-529">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="537fe-530">例如，使用者的購物車通常不應該新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="537fe-530">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="537fe-531">組態應該使用[選項模式](xref:fundamentals/configuration/options)。</span><span class="sxs-lookup"><span data-stu-id="537fe-531">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="537fe-532">同樣地，請避免只存在以允許存取某個其他物件的「資料持有者」物件。</span><span class="sxs-lookup"><span data-stu-id="537fe-532">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="537fe-533">最好是透過 DI 要求實際項目。</span><span class="sxs-lookup"><span data-stu-id="537fe-533">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="537fe-534">避免以靜態方式存取服務 (例如，以靜態方式設定 [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) 型別以四處使用)。</span><span class="sxs-lookup"><span data-stu-id="537fe-534">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) for use elsewhere).</span></span>

* <span data-ttu-id="537fe-535">避免使用「服務定位器模式」\*\*。</span><span class="sxs-lookup"><span data-stu-id="537fe-535">Avoid using the *service locator pattern*.</span></span> <span data-ttu-id="537fe-536">例如，當您可以改用 DI 時，請勿叫用 <xref:System.IServiceProvider.GetService*> 來取得服務執行個體：</span><span class="sxs-lookup"><span data-stu-id="537fe-536">For example, don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead:</span></span>

  <span data-ttu-id="537fe-537">**正確**</span><span class="sxs-lookup"><span data-stu-id="537fe-537">**Incorrect:**</span></span>

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

  <span data-ttu-id="537fe-538">**正確**：</span><span class="sxs-lookup"><span data-stu-id="537fe-538">**Correct**:</span></span>

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

* <span data-ttu-id="537fe-539">另一個要避免的服務定位器變化是插入在執行階段解析相依性的處理站。</span><span class="sxs-lookup"><span data-stu-id="537fe-539">Another service locator variation to avoid is injecting a factory that resolves dependencies at runtime.</span></span> <span data-ttu-id="537fe-540">這兩種做法都會混用[控制反轉](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion)策略。</span><span class="sxs-lookup"><span data-stu-id="537fe-540">Both of these practices mix [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

* <span data-ttu-id="537fe-541">避免以靜態方式存取 `HttpContext` (例如 [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext))。</span><span class="sxs-lookup"><span data-stu-id="537fe-541">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span></span>

<span data-ttu-id="537fe-542">就像所有的建議集，您可能會遇到需要忽略建議的情況。</span><span class="sxs-lookup"><span data-stu-id="537fe-542">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="537fe-543">例外狀況很少見&mdash;大部分是架構本身內的特殊案例。</span><span class="sxs-lookup"><span data-stu-id="537fe-543">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="537fe-544">DI 是靜態/全域物件存取模式的「替代」\*\* 選項。</span><span class="sxs-lookup"><span data-stu-id="537fe-544">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="537fe-545">如果您將 DI 與靜態物件存取混合，則可能無法實現 DI 的優點。</span><span class="sxs-lookup"><span data-stu-id="537fe-545">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="recommended-patterns-for-multi-tenancy-in-di"></a><span data-ttu-id="537fe-546">DI 中的多租使用者建議模式</span><span class="sxs-lookup"><span data-stu-id="537fe-546">Recommended patterns for multi-tenancy in DI</span></span>

<span data-ttu-id="537fe-547">[Orchard Core](https://github.com/OrchardCMS/OrchardCore)提供多租使用者。</span><span class="sxs-lookup"><span data-stu-id="537fe-547">[Orchard Core](https://github.com/OrchardCMS/OrchardCore) provides multi-tenancy.</span></span> <span data-ttu-id="537fe-548">如需詳細資訊，請參閱[Orchard Core 檔](https://docs.orchardcore.net/en/dev/)。</span><span class="sxs-lookup"><span data-stu-id="537fe-548">For more information, see the [Orchard Core Documentation](https://docs.orchardcore.net/en/dev/).</span></span>

<span data-ttu-id="537fe-549">https://github.com/OrchardCMS/OrchardCore.Samples如需如何使用 Orchard Core Framework 建立模組化和多租使用者應用程式的範例，而不含任何 CMS 特有的功能，請參閱範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="537fe-549">See the samples apps at https://github.com/OrchardCMS/OrchardCore.Samples for examples of how to build modular and multi-tenant apps using just Orchard Core Framework without any of the CMS specific features.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="537fe-550">其他資源</span><span class="sxs-lookup"><span data-stu-id="537fe-550">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="537fe-551">在 ASP.NET Core 使用 Dependency Injection 撰寫簡潔的程式碼 (MSDN)</span><span class="sxs-lookup"><span data-stu-id="537fe-551">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="537fe-552">明確相依性準則</span><span class="sxs-lookup"><span data-stu-id="537fe-552">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* <span data-ttu-id="537fe-553">[逆轉控制容器和相依性插入模式 (Martin Fowler)](https://www.martinfowler.com/articles/injection.html) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="537fe-553">[Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)](https://www.martinfowler.com/articles/injection.html)</span></span>
* <span data-ttu-id="537fe-554">[How to register a service with multiple interfaces in ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/) (如何使用 ASP.NET Core DI 中的多個介面註冊服務)</span><span class="sxs-lookup"><span data-stu-id="537fe-554">[How to register a service with multiple interfaces in ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="537fe-555">ASP.NET Core 支援相依性插入 (DI) 軟體設計模式，這是用來在類別與其相依性之間達成[控制權反轉 (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) \(英文\) 的技術。</span><span class="sxs-lookup"><span data-stu-id="537fe-555">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="537fe-556">如需有關 MVC 控制器內相依性插入的特定詳細資訊，請參閱 <xref:mvc/controllers/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="537fe-556">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="537fe-557">[查看或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="537fe-557">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="537fe-558">相依性插入概觀</span><span class="sxs-lookup"><span data-stu-id="537fe-558">Overview of dependency injection</span></span>

<span data-ttu-id="537fe-559">「相依性」\*\* 是另一個物件所需的任何物件。</span><span class="sxs-lookup"><span data-stu-id="537fe-559">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="537fe-560">檢查具有 `WriteMessage` 方法的下列 `MyDependency` 物件，應用程式中的其他類別相依於此物件：</span><span class="sxs-lookup"><span data-stu-id="537fe-560">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

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

<span data-ttu-id="537fe-561">您可以建立 `MyDependency` 類別的執行個體，讓 `WriteMessage` 方法可供類別使用。</span><span class="sxs-lookup"><span data-stu-id="537fe-561">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="537fe-562">`MyDependency` 類別是 `IndexModel` 類別的相依性：</span><span class="sxs-lookup"><span data-stu-id="537fe-562">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

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

<span data-ttu-id="537fe-563">該類別會建立 `MyDependency` 執行個體並直接相依於該執行個體。</span><span class="sxs-lookup"><span data-stu-id="537fe-563">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="537fe-564">程式碼相依性 (例如前一個範例) 有問題，因此基於下列原因應該避免使用：</span><span class="sxs-lookup"><span data-stu-id="537fe-564">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="537fe-565">若要將 `MyDependency` 取代為不同的實作，必須修改該類別。</span><span class="sxs-lookup"><span data-stu-id="537fe-565">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="537fe-566">若 `MyDependency` 有相依性，那些相依性必須由該類別設定。</span><span class="sxs-lookup"><span data-stu-id="537fe-566">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="537fe-567">在具有多個相依於 `MyDependency` 之多個類別的大型專案中，設定程式碼在不同的應用程式之間會變得鬆散。</span><span class="sxs-lookup"><span data-stu-id="537fe-567">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="537fe-568">此實作難以進行單元測試。</span><span class="sxs-lookup"><span data-stu-id="537fe-568">This implementation is difficult to unit test.</span></span> <span data-ttu-id="537fe-569">應用程式應該使用模擬 (Mock) 或虛設常式 (Stub) `MyDependency` 類別，這在使用此方法時無法使用。</span><span class="sxs-lookup"><span data-stu-id="537fe-569">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="537fe-570">相依性插入可透過下列方式解決這些問題：</span><span class="sxs-lookup"><span data-stu-id="537fe-570">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="537fe-571">使用介面或基底類別來將相依性資訊抽象化。</span><span class="sxs-lookup"><span data-stu-id="537fe-571">The use of an interface or base class to abstract the dependency implementation.</span></span>
* <span data-ttu-id="537fe-572">在服務容器中註冊相依性。</span><span class="sxs-lookup"><span data-stu-id="537fe-572">Registration of the dependency in a service container.</span></span> <span data-ttu-id="537fe-573">ASP.NET Core 提供內建服務容器 <xref:System.IServiceProvider>。</span><span class="sxs-lookup"><span data-stu-id="537fe-573">ASP.NET Core provides a built-in service container, <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="537fe-574">服務會在應用程式的 `Startup.ConfigureServices` 方法中註冊。</span><span class="sxs-lookup"><span data-stu-id="537fe-574">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="537fe-575">將服務「插入」\*\* 到服務使用位置之類別的建構函式。</span><span class="sxs-lookup"><span data-stu-id="537fe-575">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="537fe-576">架構會負責建立相依性的執行個體，並在不再需要時將它捨棄。</span><span class="sxs-lookup"><span data-stu-id="537fe-576">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="537fe-577">在[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples)中，`IMyDependency` 介面定義了服務提供給應用程式的方法：</span><span class="sxs-lookup"><span data-stu-id="537fe-577">In the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

<span data-ttu-id="537fe-578">這個介面是由具象型別 `MyDependency` 所實作：</span><span class="sxs-lookup"><span data-stu-id="537fe-578">This interface is implemented by a concrete type, `MyDependency`:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

<span data-ttu-id="537fe-579">`MyDependency` 在其建構函式中會要求 <xref:Microsoft.Extensions.Logging.ILogger`1>。</span><span class="sxs-lookup"><span data-stu-id="537fe-579">`MyDependency` requests an <xref:Microsoft.Extensions.Logging.ILogger`1> in its constructor.</span></span> <span data-ttu-id="537fe-580">以鏈結方式使用相依性插入並非不尋常。</span><span class="sxs-lookup"><span data-stu-id="537fe-580">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="537fe-581">每個要求的相依性接著會要求其自己的相依性。</span><span class="sxs-lookup"><span data-stu-id="537fe-581">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="537fe-582">容器會解決圖形中的相依性，並傳回完全解析的服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-582">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="537fe-583">必須先解析的相依性集合組通常稱為「相依性樹狀結構」**、「相依性圖形」** 或「物件圖形」\*\*。</span><span class="sxs-lookup"><span data-stu-id="537fe-583">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="537fe-584">`IMyDependency` 與 `ILogger<TCategoryName>` 必須在服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="537fe-584">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="537fe-585">`IMyDependency` 是在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="537fe-585">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="537fe-586">`ILogger<TCategoryName>` 是由記錄抽象基礎結構所註冊，所以它是預設由架構所註冊的[架構提供的服務](#framework-provided-services)。</span><span class="sxs-lookup"><span data-stu-id="537fe-586">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="537fe-587">容器會利用 [(泛型) 開放類型](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types) 解析 `ILogger<TCategoryName>`，讓您不需註冊每個 [(泛型) 建構的型別](/dotnet/csharp/language-reference/language-specification/types#constructed-types)：</span><span class="sxs-lookup"><span data-stu-id="537fe-587">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<>), typeof(Logger<>));
```

<span data-ttu-id="537fe-588">在範例應用程式中，`IMyDependency` 服務是使用具象型別 `MyDependency` 所註冊。</span><span class="sxs-lookup"><span data-stu-id="537fe-588">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="537fe-589">註冊會將服務的存留期範圍限制為單一要求的存留期。</span><span class="sxs-lookup"><span data-stu-id="537fe-589">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="537fe-590">將在此主題稍後將說明[服務存留期](#service-lifetimes)。</span><span class="sxs-lookup"><span data-stu-id="537fe-590">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> <span data-ttu-id="537fe-591">每個 `services.Add{SERVICE_NAME}` 擴充方法都會新增 (並且可能會設定) 服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-591">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="537fe-592">例如，會 `services.AddMvc()` 加入服務 Razor 頁面，而 MVC 則需要。</span><span class="sxs-lookup"><span data-stu-id="537fe-592">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="537fe-593">建議應用程式遵循此慣例。</span><span class="sxs-lookup"><span data-stu-id="537fe-593">We recommended that apps follow this convention.</span></span> <span data-ttu-id="537fe-594">在 [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) 命名空間中放置擴充方法，以封裝服務註冊群組。</span><span class="sxs-lookup"><span data-stu-id="537fe-594">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="537fe-595">如果服務的建構函式需要[內建類型](/dotnet/csharp/language-reference/keywords/built-in-types-table)，例如 `string`，可以使用[組態](xref:fundamentals/configuration/index)或[選項模式](xref:fundamentals/configuration/options)插入該類型：</span><span class="sxs-lookup"><span data-stu-id="537fe-595">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

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

<span data-ttu-id="537fe-596">服務執行個體是透過服務使用所在之類別建構函式所要求，而且會指派給私人欄位。</span><span class="sxs-lookup"><span data-stu-id="537fe-596">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="537fe-597">該欄位會視需要用來存取整個類別中的服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-597">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="537fe-598">在範例應用程式中，會要求 `IMyDependency` 執行個體並使用它來呼叫服務的 `WriteMessage` 方法：</span><span class="sxs-lookup"><span data-stu-id="537fe-598">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="services-injected-into-startup"></a><span data-ttu-id="537fe-599">插入至啟動的服務</span><span class="sxs-lookup"><span data-stu-id="537fe-599">Services injected into Startup</span></span>

<span data-ttu-id="537fe-600">`Startup`使用泛型主機（）時，只有下列服務類型可以插入至此函式 <xref:Microsoft.Extensions.Hosting.IHostBuilder> ：</span><span class="sxs-lookup"><span data-stu-id="537fe-600">Only the following service types can be injected into the `Startup` constructor when using the Generic Host (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span></span>

* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

<span data-ttu-id="537fe-601">服務可以插入 `Startup.Configure` ：</span><span class="sxs-lookup"><span data-stu-id="537fe-601">Services can be injected into `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> options)
{
    ...
}
```

<span data-ttu-id="537fe-602">如需詳細資訊，請參閱<xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="537fe-602">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="537fe-603">架構提供的服務</span><span class="sxs-lookup"><span data-stu-id="537fe-603">Framework-provided services</span></span>

<span data-ttu-id="537fe-604">`Startup.ConfigureServices`方法負責定義應用程式所使用的服務，包括平臺功能，例如 Entity Framework Core 和 ASP.NET CORE MVC。</span><span class="sxs-lookup"><span data-stu-id="537fe-604">The `Startup.ConfigureServices` method is responsible for defining the services that the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="537fe-605">一開始， `IServiceCollection` 提供的會 `ConfigureServices` 根據主機的[設定方式](xref:fundamentals/index#host)，來擁有架構所定義的服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-605">Initially, the `IServiceCollection` provided to `ConfigureServices` has services defined by the framework depending on [how the host was configured](xref:fundamentals/index#host).</span></span> <span data-ttu-id="537fe-606">以 ASP.NET Core 範本為基礎的應用程式，在架構中註冊數百項服務並不常見。</span><span class="sxs-lookup"><span data-stu-id="537fe-606">It's not uncommon for an app based on an ASP.NET Core template to have hundreds of services registered by the framework.</span></span> <span data-ttu-id="537fe-607">下表列出架構註冊服務的小型範例。</span><span class="sxs-lookup"><span data-stu-id="537fe-607">A small sample of framework-registered services is listed in the following table.</span></span>

| <span data-ttu-id="537fe-608">服務類型</span><span class="sxs-lookup"><span data-stu-id="537fe-608">Service Type</span></span> | <span data-ttu-id="537fe-609">存留期</span><span class="sxs-lookup"><span data-stu-id="537fe-609">Lifetime</span></span> |
| ---
<span data-ttu-id="537fe-610">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-610">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-611">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-611">'Blazor'</span></span>
- <span data-ttu-id="537fe-612">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-612">'Identity'</span></span>
- <span data-ttu-id="537fe-613">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-613">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-614">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-614">'Razor'</span></span>
- <span data-ttu-id="537fe-615">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-615">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-616">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-616">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-617">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-617">'Blazor'</span></span>
- <span data-ttu-id="537fe-618">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-618">'Identity'</span></span>
- <span data-ttu-id="537fe-619">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-619">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-620">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-620">'Razor'</span></span>
- <span data-ttu-id="537fe-621">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-621">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-622">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-622">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-623">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-623">'Blazor'</span></span>
- <span data-ttu-id="537fe-624">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-624">'Identity'</span></span>
- <span data-ttu-id="537fe-625">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-625">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-626">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-626">'Razor'</span></span>
- <span data-ttu-id="537fe-627">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-627">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-628">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-628">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-629">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-629">'Blazor'</span></span>
- <span data-ttu-id="537fe-630">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-630">'Identity'</span></span>
- <span data-ttu-id="537fe-631">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-631">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-632">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-632">'Razor'</span></span>
- <span data-ttu-id="537fe-633">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-633">'SignalR' uid:</span></span> 

<span data-ttu-id="537fe-634">------ |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-634">------ | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-635">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-635">'Blazor'</span></span>
- <span data-ttu-id="537fe-636">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-636">'Identity'</span></span>
- <span data-ttu-id="537fe-637">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-637">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-638">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-638">'Razor'</span></span>
- <span data-ttu-id="537fe-639">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-639">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-640">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-640">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-641">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-641">'Blazor'</span></span>
- <span data-ttu-id="537fe-642">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-642">'Identity'</span></span>
- <span data-ttu-id="537fe-643">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-643">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-644">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-644">'Razor'</span></span>
- <span data-ttu-id="537fe-645">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-645">'SignalR' uid:</span></span> 

<span data-ttu-id="537fe-646">---- || <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> |暫時性 || <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=fullName> |Singleton || <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=fullName> |Singleton || <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> |Singleton || <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> |暫時性 || <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> |Singleton || <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> |暫時性 || <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> |Singleton || <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> |Singleton || <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> |Singleton || <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> |暫時性 || <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> |Singleton || <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> |Singleton || <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> |Singleton |</span><span class="sxs-lookup"><span data-stu-id="537fe-646">---- | | <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | Transient | | <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=fullName> | Singleton | | <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=fullName> | Singleton | | <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | Singleton | | <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | Transient | | <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | Singleton | | <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | Transient | | <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | Singleton | | <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | Singleton | | <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | Singleton | | <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | Transient | | <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | Singleton | | <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | Singleton | | <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | Singleton |</span></span>

## <a name="register-additional-services-with-extension-methods"></a><span data-ttu-id="537fe-647">以擴充方法註冊其他服務</span><span class="sxs-lookup"><span data-stu-id="537fe-647">Register additional services with extension methods</span></span>

<span data-ttu-id="537fe-648">當可以使用服務集合擴充方法來註冊服務 (如果需要，也可以註冊其相依服務) 時，慣例是使用單一 `Add{SERVICE_NAME}` 擴充方法來註冊該服務要求的所有服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-648">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="537fe-649">下列程式碼範例說明如何使用擴充方法[AddDbCoNtext \<TContext> ](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)和，將其他服務新增至容器 <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*> ：</span><span class="sxs-lookup"><span data-stu-id="537fe-649">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) and <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>:</span></span>

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

<span data-ttu-id="537fe-650">如需詳細資訊，請參閱 API 文件中的 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> 類別。</span><span class="sxs-lookup"><span data-stu-id="537fe-650">For more information, see the <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> class in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="537fe-651">執行個體存留期</span><span class="sxs-lookup"><span data-stu-id="537fe-651">Service lifetimes</span></span>

<span data-ttu-id="537fe-652">為每個已註冊的服務選擇適當的存留期。</span><span class="sxs-lookup"><span data-stu-id="537fe-652">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="537fe-653">ASP.NET Core 服務可以使用下列存留期進行設定：</span><span class="sxs-lookup"><span data-stu-id="537fe-653">ASP.NET Core services can be configured with the following lifetimes:</span></span>

### <a name="transient"></a><span data-ttu-id="537fe-654">暫時性</span><span class="sxs-lookup"><span data-stu-id="537fe-654">Transient</span></span>

<span data-ttu-id="537fe-655">每次從服務容器要求暫時性存留期服務 (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) 時都會建立它們。</span><span class="sxs-lookup"><span data-stu-id="537fe-655">Transient lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) are created each time they're requested from the service container.</span></span> <span data-ttu-id="537fe-656">此存留期最適合用於輕量型的無狀態服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-656">This lifetime works best for lightweight, stateless services.</span></span>

### <a name="scoped"></a><span data-ttu-id="537fe-657">具範圍</span><span class="sxs-lookup"><span data-stu-id="537fe-657">Scoped</span></span>

<span data-ttu-id="537fe-658">具範圍存留期服務 (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) 會在每次用戶端要求 (連線) 時建立一次。</span><span class="sxs-lookup"><span data-stu-id="537fe-658">Scoped lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) are created once per client request (connection).</span></span>

> [!WARNING]
> <span data-ttu-id="537fe-659">在中介軟體中使用具範圍服務時，請將該服務插入 `Invoke` 或 `InvokeAsync` 方法中。</span><span class="sxs-lookup"><span data-stu-id="537fe-659">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="537fe-660">不要透過函式[插入](xref:mvc/controllers/dependency-injection#constructor-injection)來插入，因為它會強制服務的行為就像 singleton 一樣。</span><span class="sxs-lookup"><span data-stu-id="537fe-660">Don't inject via [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="537fe-661">如需詳細資訊，請參閱<xref:fundamentals/middleware/write#per-request-middleware-dependencies>。</span><span class="sxs-lookup"><span data-stu-id="537fe-661">For more information, see <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span></span>

### <a name="singleton"></a><span data-ttu-id="537fe-662">單一</span><span class="sxs-lookup"><span data-stu-id="537fe-662">Singleton</span></span>

<span data-ttu-id="537fe-663">當第一次收到有關單一資料庫存留期服務的要求時 (或是當執行 `Startup.ConfigureServices` 且隨著服務註冊指定執行個體時)，即會建立單一資料庫存留期服務 (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>)。</span><span class="sxs-lookup"><span data-stu-id="537fe-663">Singleton lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) are created the first time they're requested (or when `Startup.ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="537fe-664">每個後續要求都會使用相同的執行個體。</span><span class="sxs-lookup"><span data-stu-id="537fe-664">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="537fe-665">若應用程式要求單一資料庫行為，建議您允許服務容器管理服務的存留期。</span><span class="sxs-lookup"><span data-stu-id="537fe-665">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="537fe-666">不要實作單一資料庫設計模式並為使用者提供可用來管理類別中物件存留期的程式碼。</span><span class="sxs-lookup"><span data-stu-id="537fe-666">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="537fe-667">從單一資料庫解析具範圍服務是非常危險的。</span><span class="sxs-lookup"><span data-stu-id="537fe-667">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="537fe-668">處理後續要求時，它可能會導致服務有不正確的狀態。</span><span class="sxs-lookup"><span data-stu-id="537fe-668">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

## <a name="service-registration-methods"></a><span data-ttu-id="537fe-669">服務註冊方法</span><span class="sxs-lookup"><span data-stu-id="537fe-669">Service registration methods</span></span>

<span data-ttu-id="537fe-670">服務註冊擴充方法提供在特定案例中很有用的多載。</span><span class="sxs-lookup"><span data-stu-id="537fe-670">Service registration extension methods offer overloads that are useful in specific scenarios.</span></span>

| <span data-ttu-id="537fe-671">方法</span><span class="sxs-lookup"><span data-stu-id="537fe-671">Method</span></span> | <span data-ttu-id="537fe-672">自動</span><span class="sxs-lookup"><span data-stu-id="537fe-672">Automatic</span></span><br><span data-ttu-id="537fe-673">物件</span><span class="sxs-lookup"><span data-stu-id="537fe-673">object</span></span><br><span data-ttu-id="537fe-674">處置</span><span class="sxs-lookup"><span data-stu-id="537fe-674">disposal</span></span> | <span data-ttu-id="537fe-675">多重</span><span class="sxs-lookup"><span data-stu-id="537fe-675">Multiple</span></span><br><span data-ttu-id="537fe-676">實作</span><span class="sxs-lookup"><span data-stu-id="537fe-676">implementations</span></span> | <span data-ttu-id="537fe-677">傳遞引數</span><span class="sxs-lookup"><span data-stu-id="537fe-677">Pass args</span></span> |
| ---
<span data-ttu-id="537fe-678">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-678">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-679">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-679">'Blazor'</span></span>
- <span data-ttu-id="537fe-680">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-680">'Identity'</span></span>
- <span data-ttu-id="537fe-681">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-681">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-682">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-682">'Razor'</span></span>
- <span data-ttu-id="537fe-683">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-683">'SignalR' uid:</span></span> 

<span data-ttu-id="537fe-684">--- |：---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-684">--- | :--- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-685">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-685">'Blazor'</span></span>
- <span data-ttu-id="537fe-686">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-686">'Identity'</span></span>
- <span data-ttu-id="537fe-687">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-687">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-688">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-688">'Razor'</span></span>
- <span data-ttu-id="537fe-689">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-689">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-690">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-690">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-691">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-691">'Blazor'</span></span>
- <span data-ttu-id="537fe-692">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-692">'Identity'</span></span>
- <span data-ttu-id="537fe-693">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-693">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-694">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-694">'Razor'</span></span>
- <span data-ttu-id="537fe-695">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-695">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-696">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-696">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-697">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-697">'Blazor'</span></span>
- <span data-ttu-id="537fe-698">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-698">'Identity'</span></span>
- <span data-ttu-id="537fe-699">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-699">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-700">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-700">'Razor'</span></span>
- <span data-ttu-id="537fe-701">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-701">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-702">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-702">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-703">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-703">'Blazor'</span></span>
- <span data-ttu-id="537fe-704">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-704">'Identity'</span></span>
- <span data-ttu-id="537fe-705">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-705">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-706">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-706">'Razor'</span></span>
- <span data-ttu-id="537fe-707">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-707">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-708">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-708">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-709">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-709">'Blazor'</span></span>
- <span data-ttu-id="537fe-710">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-710">'Identity'</span></span>
- <span data-ttu-id="537fe-711">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-711">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-712">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-712">'Razor'</span></span>
- <span data-ttu-id="537fe-713">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-713">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-714">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-714">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-715">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-715">'Blazor'</span></span>
- <span data-ttu-id="537fe-716">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-716">'Identity'</span></span>
- <span data-ttu-id="537fe-717">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-717">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-718">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-718">'Razor'</span></span>
- <span data-ttu-id="537fe-719">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-719">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-720">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-720">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-721">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-721">'Blazor'</span></span>
- <span data-ttu-id="537fe-722">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-722">'Identity'</span></span>
- <span data-ttu-id="537fe-723">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-723">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-724">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-724">'Razor'</span></span>
- <span data-ttu-id="537fe-725">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-725">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-726">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-726">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-727">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-727">'Blazor'</span></span>
- <span data-ttu-id="537fe-728">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-728">'Identity'</span></span>
- <span data-ttu-id="537fe-729">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-729">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-730">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-730">'Razor'</span></span>
- <span data-ttu-id="537fe-731">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-731">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-732">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-732">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-733">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-733">'Blazor'</span></span>
- <span data-ttu-id="537fe-734">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-734">'Identity'</span></span>
- <span data-ttu-id="537fe-735">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-735">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-736">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-736">'Razor'</span></span>
- <span data-ttu-id="537fe-737">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-737">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-738">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-738">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-739">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-739">'Blazor'</span></span>
- <span data-ttu-id="537fe-740">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-740">'Identity'</span></span>
- <span data-ttu-id="537fe-741">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-741">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-742">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-742">'Razor'</span></span>
- <span data-ttu-id="537fe-743">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-743">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-744">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-744">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-745">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-745">'Blazor'</span></span>
- <span data-ttu-id="537fe-746">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-746">'Identity'</span></span>
- <span data-ttu-id="537fe-747">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-747">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-748">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-748">'Razor'</span></span>
- <span data-ttu-id="537fe-749">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-749">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-750">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-750">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-751">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-751">'Blazor'</span></span>
- <span data-ttu-id="537fe-752">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-752">'Identity'</span></span>
- <span data-ttu-id="537fe-753">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-753">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-754">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-754">'Razor'</span></span>
- <span data-ttu-id="537fe-755">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-755">'SignalR' uid:</span></span> 

<span data-ttu-id="537fe-756">---------------: |：---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-756">---------------: | :--- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-757">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-757">'Blazor'</span></span>
- <span data-ttu-id="537fe-758">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-758">'Identity'</span></span>
- <span data-ttu-id="537fe-759">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-759">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-760">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-760">'Razor'</span></span>
- <span data-ttu-id="537fe-761">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-761">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-762">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-762">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-763">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-763">'Blazor'</span></span>
- <span data-ttu-id="537fe-764">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-764">'Identity'</span></span>
- <span data-ttu-id="537fe-765">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-765">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-766">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-766">'Razor'</span></span>
- <span data-ttu-id="537fe-767">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-767">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-768">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-768">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-769">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-769">'Blazor'</span></span>
- <span data-ttu-id="537fe-770">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-770">'Identity'</span></span>
- <span data-ttu-id="537fe-771">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-771">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-772">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-772">'Razor'</span></span>
- <span data-ttu-id="537fe-773">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-773">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-774">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-774">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-775">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-775">'Blazor'</span></span>
- <span data-ttu-id="537fe-776">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-776">'Identity'</span></span>
- <span data-ttu-id="537fe-777">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-777">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-778">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-778">'Razor'</span></span>
- <span data-ttu-id="537fe-779">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-779">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-780">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-780">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-781">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-781">'Blazor'</span></span>
- <span data-ttu-id="537fe-782">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-782">'Identity'</span></span>
- <span data-ttu-id="537fe-783">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-783">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-784">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-784">'Razor'</span></span>
- <span data-ttu-id="537fe-785">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-785">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-786">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-786">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-787">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-787">'Blazor'</span></span>
- <span data-ttu-id="537fe-788">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-788">'Identity'</span></span>
- <span data-ttu-id="537fe-789">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-789">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-790">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-790">'Razor'</span></span>
- <span data-ttu-id="537fe-791">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-791">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-792">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-792">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-793">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-793">'Blazor'</span></span>
- <span data-ttu-id="537fe-794">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-794">'Identity'</span></span>
- <span data-ttu-id="537fe-795">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-795">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-796">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-796">'Razor'</span></span>
- <span data-ttu-id="537fe-797">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-797">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-798">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-798">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-799">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-799">'Blazor'</span></span>
- <span data-ttu-id="537fe-800">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-800">'Identity'</span></span>
- <span data-ttu-id="537fe-801">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-801">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-802">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-802">'Razor'</span></span>
- <span data-ttu-id="537fe-803">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-803">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-804">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-804">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-805">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-805">'Blazor'</span></span>
- <span data-ttu-id="537fe-806">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-806">'Identity'</span></span>
- <span data-ttu-id="537fe-807">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-807">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-808">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-808">'Razor'</span></span>
- <span data-ttu-id="537fe-809">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-809">'SignalR' uid:</span></span> 

-
<span data-ttu-id="537fe-810">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-810">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-811">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-811">'Blazor'</span></span>
- <span data-ttu-id="537fe-812">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-812">'Identity'</span></span>
- <span data-ttu-id="537fe-813">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-813">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-814">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-814">'Razor'</span></span>
- <span data-ttu-id="537fe-815">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-815">'SignalR' uid:</span></span> 

<span data-ttu-id="537fe-816">-------------: |：---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="537fe-816">-------------: | :--- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="537fe-817">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="537fe-817">'Blazor'</span></span>
- <span data-ttu-id="537fe-818">'Identity'</span><span class="sxs-lookup"><span data-stu-id="537fe-818">'Identity'</span></span>
- <span data-ttu-id="537fe-819">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="537fe-819">'Let's Encrypt'</span></span>
- <span data-ttu-id="537fe-820">'Razor'</span><span class="sxs-lookup"><span data-stu-id="537fe-820">'Razor'</span></span>
- <span data-ttu-id="537fe-821">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="537fe-821">'SignalR' uid:</span></span> 

<span data-ttu-id="537fe-822">----: | | `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`</span><span class="sxs-lookup"><span data-stu-id="537fe-822">----: | | `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`</span></span><br><span data-ttu-id="537fe-823">範例：</span><span class="sxs-lookup"><span data-stu-id="537fe-823">Example:</span></span><br><span data-ttu-id="537fe-824">`services.AddSingleton<IMyDep, MyDep>();`|是 |是 |否 | |`Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`</span><span class="sxs-lookup"><span data-stu-id="537fe-824">`services.AddSingleton<IMyDep, MyDep>();` | Yes | Yes | No | | `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`</span></span><br><span data-ttu-id="537fe-825">範例：</span><span class="sxs-lookup"><span data-stu-id="537fe-825">Examples:</span></span><br>`services.AddSingleton<IMyDep>(sp => new MyDep());`<br><span data-ttu-id="537fe-826">`services.AddSingleton<IMyDep>(sp => new MyDep("A string!"));`|是 |是 |是 | |`Add{LIFETIME}<{IMPLEMENTATION}>()`</span><span class="sxs-lookup"><span data-stu-id="537fe-826">`services.AddSingleton<IMyDep>(sp => new MyDep("A string!"));` | Yes | Yes | Yes | | `Add{LIFETIME}<{IMPLEMENTATION}>()`</span></span><br><span data-ttu-id="537fe-827">範例：</span><span class="sxs-lookup"><span data-stu-id="537fe-827">Example:</span></span><br><span data-ttu-id="537fe-828">`services.AddSingleton<MyDep>();`|是 |否 |否 | |`AddSingleton<{SERVICE}>(new {IMPLEMENTATION})`</span><span class="sxs-lookup"><span data-stu-id="537fe-828">`services.AddSingleton<MyDep>();` | Yes | No | No | | `AddSingleton<{SERVICE}>(new {IMPLEMENTATION})`</span></span><br><span data-ttu-id="537fe-829">範例：</span><span class="sxs-lookup"><span data-stu-id="537fe-829">Examples:</span></span><br>`services.AddSingleton<IMyDep>(new MyDep());`<br><span data-ttu-id="537fe-830">`services.AddSingleton<IMyDep>(new MyDep("A string!"));`|否 |是 |是 | |`AddSingleton(new {IMPLEMENTATION})`</span><span class="sxs-lookup"><span data-stu-id="537fe-830">`services.AddSingleton<IMyDep>(new MyDep("A string!"));` | No | Yes | Yes | | `AddSingleton(new {IMPLEMENTATION})`</span></span><br><span data-ttu-id="537fe-831">範例：</span><span class="sxs-lookup"><span data-stu-id="537fe-831">Examples:</span></span><br>`services.AddSingleton(new MyDep());`<br><span data-ttu-id="537fe-832">`services.AddSingleton(new MyDep("A string!"));`|否 |否 |是 |</span><span class="sxs-lookup"><span data-stu-id="537fe-832">`services.AddSingleton(new MyDep("A string!"));` | No | No | Yes |</span></span>

<span data-ttu-id="537fe-833">如需類型處置的詳細資訊，請參閱[＜服務處置＞](#disposal-of-services)一節。</span><span class="sxs-lookup"><span data-stu-id="537fe-833">For more information on type disposal, see the [Disposal of services](#disposal-of-services) section.</span></span> <span data-ttu-id="537fe-834">多個實作的常見案例是[模擬測試類型](xref:test/integration-tests#inject-mock-services)。</span><span class="sxs-lookup"><span data-stu-id="537fe-834">A common scenario for multiple implementations is [mocking types for testing](xref:test/integration-tests#inject-mock-services).</span></span>

<span data-ttu-id="537fe-835">如果還未註冊任何實作，則 `TryAdd{LIFETIME}` 方法只會註冊服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-835">`TryAdd{LIFETIME}` methods only register the service if there isn't already an implementation registered.</span></span>

<span data-ttu-id="537fe-836">在下列範例中，第一行會為 `IMyDependency` 註冊 `MyDependency`。</span><span class="sxs-lookup"><span data-stu-id="537fe-836">In the following example, the first line registers `MyDependency` for `IMyDependency`.</span></span> <span data-ttu-id="537fe-837">第二行則沒有任何作用，因為 `IMyDependency` 已具有註冊的實作：</span><span class="sxs-lookup"><span data-stu-id="537fe-837">The second line has no effect because `IMyDependency` already has a registered implementation:</span></span>

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

<span data-ttu-id="537fe-838">如需詳細資訊，請參閱</span><span class="sxs-lookup"><span data-stu-id="537fe-838">For more information, see:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

<span data-ttu-id="537fe-839">如果還沒有「相同類型的」\*\* 實作，則 [TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) 方法只會註冊服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-839">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) methods only register the service if there isn't already an implementation *of the same type*.</span></span> <span data-ttu-id="537fe-840">多個服務會透過 `IEnumerable<{SERVICE}>` 解析。</span><span class="sxs-lookup"><span data-stu-id="537fe-840">Multiple services are resolved via `IEnumerable<{SERVICE}>`.</span></span> <span data-ttu-id="537fe-841">註冊服務時，如果尚未新增相同類型的執行個體，則開發人員只會希望新增執行個體。</span><span class="sxs-lookup"><span data-stu-id="537fe-841">When registering services, the developer only wants to add an instance if one of the same type hasn't already been added.</span></span> <span data-ttu-id="537fe-842">程式庫作者通常會使用此方法來避免註冊容器中某個執行個體的兩個複本。</span><span class="sxs-lookup"><span data-stu-id="537fe-842">Generally, this method is used by library authors to avoid registering two copies of an instance in the container.</span></span>

<span data-ttu-id="537fe-843">在下列範例中，第一行會為 `IMyDep1` 註冊 `MyDep`。</span><span class="sxs-lookup"><span data-stu-id="537fe-843">In the following example, the first line registers `MyDep` for `IMyDep1`.</span></span> <span data-ttu-id="537fe-844">第二行會為 `IMyDep2` 註冊 `MyDep`。</span><span class="sxs-lookup"><span data-stu-id="537fe-844">The second line registers `MyDep` for `IMyDep2`.</span></span> <span data-ttu-id="537fe-845">第三行則沒有任何作用，因為 `IMyDep1` 已具有 `MyDep` 的已註冊實作：</span><span class="sxs-lookup"><span data-stu-id="537fe-845">The third line has no effect because `IMyDep1` already has a registered implementation of `MyDep`:</span></span>

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a><span data-ttu-id="537fe-846">建構函式插入行為</span><span class="sxs-lookup"><span data-stu-id="537fe-846">Constructor injection behavior</span></span>

<span data-ttu-id="537fe-847">服務可以透過兩個機制來解析：</span><span class="sxs-lookup"><span data-stu-id="537fe-847">Services can be resolved by two mechanisms:</span></span>

* <xref:System.IServiceProvider>
* <span data-ttu-id="537fe-848"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities>：允許在相依性插入容器中不註冊服務時，建立物件。</span><span class="sxs-lookup"><span data-stu-id="537fe-848"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities>: Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="537fe-849">搭配使用者面向抽象 (例如標籤協助程式、MVC 控制器與模型繫結器) 使用 `ActivatorUtilities`。</span><span class="sxs-lookup"><span data-stu-id="537fe-849">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="537fe-850">建構函式可以接受不是由相依性插入提供的引數，但引數必須指派預設值。</span><span class="sxs-lookup"><span data-stu-id="537fe-850">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="537fe-851">當服務由或解析時，程式的 `IServiceProvider` `ActivatorUtilities` [插入](xref:mvc/controllers/dependency-injection#constructor-injection)需要*公用*的函式。</span><span class="sxs-lookup"><span data-stu-id="537fe-851">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) requires a *public* constructor.</span></span>

<span data-ttu-id="537fe-852">當服務由解析時 `ActivatorUtilities` ，程式的[插入](xref:mvc/controllers/dependency-injection#constructor-injection)只需要有一個適用的函式。</span><span class="sxs-lookup"><span data-stu-id="537fe-852">When services are resolved by `ActivatorUtilities`, [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) requires that only one applicable constructor exists.</span></span> <span data-ttu-id="537fe-853">支援建構函式多載，但只能有一個多載存在，其引數可以藉由相依性插入而完成。</span><span class="sxs-lookup"><span data-stu-id="537fe-853">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="537fe-854">Entity Framework 內容</span><span class="sxs-lookup"><span data-stu-id="537fe-854">Entity Framework contexts</span></span>

<span data-ttu-id="537fe-855">因為一般會將 Web 應用程式資料庫作業範圍設定為用戶端要求，所以通常會使用[具範圍存留期](#service-lifetimes)將 Entity Framework 內容新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="537fe-855">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="537fe-856">如果在註冊資料庫內容時， [AddDbCoNtext \<TContext> ](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)多載未指定存留期，則會將預設存留期設定為範圍。</span><span class="sxs-lookup"><span data-stu-id="537fe-856">The default lifetime is scoped if a lifetime isn't specified by an [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) overload when registering the database context.</span></span> <span data-ttu-id="537fe-857">指定存留期的服務不應該使用存留期比服務還短的資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="537fe-857">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="537fe-858">留期和註冊選項</span><span class="sxs-lookup"><span data-stu-id="537fe-858">Lifetime and registration options</span></span>

<span data-ttu-id="537fe-859">為了示範這些存留期和註冊選項之間的差異，請考慮下列介面，以具有唯一識別碼 `OperationId` 的「作業」代表工作。</span><span class="sxs-lookup"><span data-stu-id="537fe-859">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="537fe-860">視如何針對下列介面設定作業服務存留期而定，當類別要求時，容器會提供相同或不同的服務執行個體：</span><span class="sxs-lookup"><span data-stu-id="537fe-860">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

<span data-ttu-id="537fe-861">介面是在 `Operation` 類別中實作。</span><span class="sxs-lookup"><span data-stu-id="537fe-861">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="537fe-862">若未提供 GUID，`Operation` 建構函式會產生 GUID：</span><span class="sxs-lookup"><span data-stu-id="537fe-862">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

<span data-ttu-id="537fe-863">會註冊相依於每種其他 `Operation` 類型的 `OperationService`。</span><span class="sxs-lookup"><span data-stu-id="537fe-863">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="537fe-864">當透過相依性插入要求 `OperationService` 時，它會根據相依服務的存留期擷取每個服務的新執行個體或現有執行個體。</span><span class="sxs-lookup"><span data-stu-id="537fe-864">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="537fe-865">當因收到來自容器的要求而建立暫時性服務時，`IOperationTransient` 服務的 `OperationId` 會與 `OperationService` 的 `OperationId` 不同。</span><span class="sxs-lookup"><span data-stu-id="537fe-865">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="537fe-866">`OperationService` 會收到 `IOperationTransient` 類別的新執行個體。</span><span class="sxs-lookup"><span data-stu-id="537fe-866">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="537fe-867">新執行個體會產生不同的 `OperationId`。</span><span class="sxs-lookup"><span data-stu-id="537fe-867">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="537fe-868">當因用戶端要求而建立具範圍的服務時，`IOperationScoped` 服務與用戶端要求中 `OperationService` 的 `OperationId` 皆相同。</span><span class="sxs-lookup"><span data-stu-id="537fe-868">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="537fe-869">在不同的用戶端要求中，兩個服務的 `OperationId` 值都不同。</span><span class="sxs-lookup"><span data-stu-id="537fe-869">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="537fe-870">當建立一次 singleton 與 singleton 執行個體服務，並在所有用戶端要求與所有服務中使用時，`OperationId` 在所有服務要求中會固定不變。</span><span class="sxs-lookup"><span data-stu-id="537fe-870">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

<span data-ttu-id="537fe-871">在 `Startup.ConfigureServices` 中，每個類型都會根據其具名存留期新增至容器：</span><span class="sxs-lookup"><span data-stu-id="537fe-871">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

<span data-ttu-id="537fe-872">`IOperationSingletonInstance` 服務使用具有 `Guid.Empty` 已知識別碼的特定執行個體。</span><span class="sxs-lookup"><span data-stu-id="537fe-872">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="537fe-873">很明顯此型別是使用中 (其 GUID 是所有區域)。</span><span class="sxs-lookup"><span data-stu-id="537fe-873">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="537fe-874">範例應用程式示範個別要求內與個別要求之間的物件存留期。</span><span class="sxs-lookup"><span data-stu-id="537fe-874">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="537fe-875">範例應用程式的 `IndexModel` 會要求每種 `IOperation` 型別與 `OperationService`。</span><span class="sxs-lookup"><span data-stu-id="537fe-875">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="537fe-876">頁面接著會透過屬性指派來顯示所有頁面模型類別與服務的 `OperationId` 值：</span><span class="sxs-lookup"><span data-stu-id="537fe-876">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

<span data-ttu-id="537fe-877">下面兩個輸出顯示兩個要求的結果：</span><span class="sxs-lookup"><span data-stu-id="537fe-877">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="537fe-878">**第一個要求：**</span><span class="sxs-lookup"><span data-stu-id="537fe-878">**First request:**</span></span>

<span data-ttu-id="537fe-879">控制器作業：</span><span class="sxs-lookup"><span data-stu-id="537fe-879">Controller operations:</span></span>

<span data-ttu-id="537fe-880">暫時性： d233e165-f417-469b-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="537fe-880">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="537fe-881">具範圍： 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="537fe-881">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="537fe-882">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="537fe-882">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="537fe-883">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="537fe-883">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="537fe-884">`OperationService` 作業：</span><span class="sxs-lookup"><span data-stu-id="537fe-884">`OperationService` operations:</span></span>

<span data-ttu-id="537fe-885">暫時性： c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="537fe-885">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="537fe-886">具範圍： 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="537fe-886">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="537fe-887">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="537fe-887">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="537fe-888">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="537fe-888">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="537fe-889">**:第二個要求：**</span><span class="sxs-lookup"><span data-stu-id="537fe-889">**Second request:**</span></span>

<span data-ttu-id="537fe-890">控制器作業：</span><span class="sxs-lookup"><span data-stu-id="537fe-890">Controller operations:</span></span>

<span data-ttu-id="537fe-891">暫時性： b63bd538-0a37-4ff1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="537fe-891">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="537fe-892">具範圍： 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="537fe-892">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="537fe-893">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="537fe-893">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="537fe-894">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="537fe-894">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="537fe-895">`OperationService` 作業：</span><span class="sxs-lookup"><span data-stu-id="537fe-895">`OperationService` operations:</span></span>

<span data-ttu-id="537fe-896">暫時性： c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="537fe-896">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="537fe-897">具範圍： 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="537fe-897">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="537fe-898">單一資料庫： 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="537fe-898">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="537fe-899">執行個體： 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="537fe-899">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="537fe-900">觀察哪些 `OperationId` 值在要求內以及要求之間不同：</span><span class="sxs-lookup"><span data-stu-id="537fe-900">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="537fe-901">「暫時性」\*\* 物件一律不同。</span><span class="sxs-lookup"><span data-stu-id="537fe-901">*Transient* objects are always different.</span></span> <span data-ttu-id="537fe-902">第一個與第二個用戶端要求的暫時性 `OperationId` 值在兩個 `OperationService` 作業之間與各用戶端要求之間都不同。</span><span class="sxs-lookup"><span data-stu-id="537fe-902">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="537fe-903">新執行個體會提供給每個服務要求以及用戶端要求。</span><span class="sxs-lookup"><span data-stu-id="537fe-903">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="537fe-904">「具範圍」\*\* 物件在同一個用戶端要求內皆相同，但在不同的用戶端要求之間則不同。</span><span class="sxs-lookup"><span data-stu-id="537fe-904">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="537fe-905">*Singleton*不論是否 `Operation` 在中提供實例，每個物件和每個要求的單一物件都是相同的 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="537fe-905">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `Startup.ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="537fe-906">從主要呼叫服務</span><span class="sxs-lookup"><span data-stu-id="537fe-906">Call services from main</span></span>

<span data-ttu-id="537fe-907">使用 [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) 建立 <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>，以解析應用程式範圍中的範圍服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-907">Create an <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="537fe-908">此法可用於在開機時存取範圍服務，以執行初始化工作。</span><span class="sxs-lookup"><span data-stu-id="537fe-908">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="537fe-909">下列範例示範如何在 `Program.Main` 中取得 `MyScopedService`：</span><span class="sxs-lookup"><span data-stu-id="537fe-909">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="537fe-910">範圍驗證</span><span class="sxs-lookup"><span data-stu-id="537fe-910">Scope validation</span></span>

<span data-ttu-id="537fe-911">當應用程式在開發環境中執行時，預設服務提供者會執行檢查以確認：</span><span class="sxs-lookup"><span data-stu-id="537fe-911">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="537fe-912">範圍服務不是直接或間接由開機服務提供者解析。</span><span class="sxs-lookup"><span data-stu-id="537fe-912">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="537fe-913">範圍服務不是直接或間接插入至單一服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-913">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="537fe-914">根服務提供者會在呼叫 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> 時建立。</span><span class="sxs-lookup"><span data-stu-id="537fe-914">The root service provider is created when <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> is called.</span></span> <span data-ttu-id="537fe-915">當提供者啟動應用程式時，根服務提供者的存留期與應用程式/伺服器的存留期一致，並會在應用程式關閉時處置。</span><span class="sxs-lookup"><span data-stu-id="537fe-915">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="537fe-916">範圍服務會由建立這些服務的容器處置。</span><span class="sxs-lookup"><span data-stu-id="537fe-916">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="537fe-917">若是在根容器中建立範圍服務，因為當應用程式/伺服器關機時，服務只會由根容器處理，所以服務的存留期會提升為單一服務等級。</span><span class="sxs-lookup"><span data-stu-id="537fe-917">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="537fe-918">當呼叫 `BuildServiceProvider` 時，驗證服務範圍會攔截到這些情況。</span><span class="sxs-lookup"><span data-stu-id="537fe-918">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="537fe-919">如需詳細資訊，請參閱<xref:fundamentals/host/web-host#scope-validation>。</span><span class="sxs-lookup"><span data-stu-id="537fe-919">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="537fe-920">要求服務</span><span class="sxs-lookup"><span data-stu-id="537fe-920">Request Services</span></span>

<span data-ttu-id="537fe-921">來自 `HttpContext`，在 ASP.NET Core 要求內提供的服務是透過 [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) 集合公開。</span><span class="sxs-lookup"><span data-stu-id="537fe-921">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) collection.</span></span>

<span data-ttu-id="537fe-922">要求服務代表您在應用程式中設定和要求的服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-922">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="537fe-923">當物件指定相依性時，這些會由在 `RequestServices` 中找到的類型來滿足，而非 `ApplicationServices`。</span><span class="sxs-lookup"><span data-stu-id="537fe-923">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="537fe-924">一般而言，應用程式不應該直接使用這些屬性。</span><span class="sxs-lookup"><span data-stu-id="537fe-924">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="537fe-925">相反地，透過類別建構函式要求類別所需的型別，以及允許架構插入相依性。</span><span class="sxs-lookup"><span data-stu-id="537fe-925">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="537fe-926">這會產生容易測試的類別。</span><span class="sxs-lookup"><span data-stu-id="537fe-926">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="537fe-927">偏好要求相依性作為建構函式參數，而不要存取 `RequestServices` 集合。</span><span class="sxs-lookup"><span data-stu-id="537fe-927">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="537fe-928">針對相依性插入設計服務</span><span class="sxs-lookup"><span data-stu-id="537fe-928">Design services for dependency injection</span></span>

<span data-ttu-id="537fe-929">最佳做法是：</span><span class="sxs-lookup"><span data-stu-id="537fe-929">Best practices are to:</span></span>

* <span data-ttu-id="537fe-930">設計服務以使用相依性插入來取得其相依性。</span><span class="sxs-lookup"><span data-stu-id="537fe-930">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="537fe-931">避免具狀態的靜態類別和成員。</span><span class="sxs-lookup"><span data-stu-id="537fe-931">Avoid stateful, static classes and members.</span></span> <span data-ttu-id="537fe-932">將應用程式設計成使用單一服務，以避免建立全域狀態。</span><span class="sxs-lookup"><span data-stu-id="537fe-932">Design apps to use singleton services instead, which avoid creating global state.</span></span>
* <span data-ttu-id="537fe-933">避免直接在服務內具現化相依類別。</span><span class="sxs-lookup"><span data-stu-id="537fe-933">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="537fe-934">直接具現化會將程式碼耦合到特定實作。</span><span class="sxs-lookup"><span data-stu-id="537fe-934">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="537fe-935">讓應用程式類別維持在小型、情況良好且可輕鬆測試的狀態。</span><span class="sxs-lookup"><span data-stu-id="537fe-935">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="537fe-936">若類別有太多插入的相依性，這通常表示類別有太多責任而且正違反[單一責任原則 (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="537fe-936">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="537fe-937">將類別負責的某些部分移到新的類別，以嘗試重構類別。</span><span class="sxs-lookup"><span data-stu-id="537fe-937">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="537fe-938">請記住，頁面 Razor 頁面模型類別和 MVC 控制器類別應該專注于 UI 考慮。</span><span class="sxs-lookup"><span data-stu-id="537fe-938">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="537fe-939">商務規則和資料存取實作詳細資料應該保存在適合這些[分開考量](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) (Separation of Concerns) 類別中。</span><span class="sxs-lookup"><span data-stu-id="537fe-939">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="537fe-940">處置服務</span><span class="sxs-lookup"><span data-stu-id="537fe-940">Disposal of services</span></span>

<span data-ttu-id="537fe-941">容器會為它建立的 <xref:System.IDisposable> 類型呼叫 <xref:System.IDisposable.Dispose*>。</span><span class="sxs-lookup"><span data-stu-id="537fe-941">The container calls <xref:System.IDisposable.Dispose*> for the <xref:System.IDisposable> types it creates.</span></span> <span data-ttu-id="537fe-942">若執行個體由使用者程式碼新增到容器中，它將不會自動處置。</span><span class="sxs-lookup"><span data-stu-id="537fe-942">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

<span data-ttu-id="537fe-943">在下列範例中，服務會建立服務容器並自動處置：</span><span class="sxs-lookup"><span data-stu-id="537fe-943">In the following example, the services are created by the service container and disposed automatically:</span></span>

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

<span data-ttu-id="537fe-944">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="537fe-944">In the following example:</span></span>

* <span data-ttu-id="537fe-945">服務容器不會建立服務實例。</span><span class="sxs-lookup"><span data-stu-id="537fe-945">The service instances aren't created by the service container.</span></span>
* <span data-ttu-id="537fe-946">架構不知道預期的服務存留期。</span><span class="sxs-lookup"><span data-stu-id="537fe-946">The intended service lifetimes aren't known by the framework.</span></span>
* <span data-ttu-id="537fe-947">架構不會自動處置服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-947">The framework doesn't dispose of the services automatically.</span></span>
* <span data-ttu-id="537fe-948">如果服務未在開發人員程式碼中明確處置，它們會持續存在，直到應用程式關閉為止。</span><span class="sxs-lookup"><span data-stu-id="537fe-948">If the services aren't explicitly disposed in developer code, they persist until the app shuts down.</span></span>

```csharp
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<Service1>(new Service1());
    services.AddSingleton(new Service2());
}
```

## <a name="default-service-container-replacement"></a><span data-ttu-id="537fe-949">預設服務容器取代</span><span class="sxs-lookup"><span data-stu-id="537fe-949">Default service container replacement</span></span>

<span data-ttu-id="537fe-950">內建的服務容器是設計用來滿足架構和大部分取用者應用程式的需求。</span><span class="sxs-lookup"><span data-stu-id="537fe-950">The built-in service container is designed to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="537fe-951">我們建議使用內建容器，除非您需要內建容器不支援的特定功能，例如：</span><span class="sxs-lookup"><span data-stu-id="537fe-951">We recommend using the built-in container unless you need a specific feature that the built-in container doesn't support, such as:</span></span>

* <span data-ttu-id="537fe-952">屬性插入</span><span class="sxs-lookup"><span data-stu-id="537fe-952">Property injection</span></span>
* <span data-ttu-id="537fe-953">根據名稱插入</span><span class="sxs-lookup"><span data-stu-id="537fe-953">Injection based on name</span></span>
* <span data-ttu-id="537fe-954">子容器</span><span class="sxs-lookup"><span data-stu-id="537fe-954">Child containers</span></span>
* <span data-ttu-id="537fe-955">自訂生命週期管理</span><span class="sxs-lookup"><span data-stu-id="537fe-955">Custom lifetime management</span></span>
* <span data-ttu-id="537fe-956">`Func<T>` 支援延遲初始設定</span><span class="sxs-lookup"><span data-stu-id="537fe-956">`Func<T>` support for lazy initialization</span></span>
* <span data-ttu-id="537fe-957">以慣例為基礎的註冊</span><span class="sxs-lookup"><span data-stu-id="537fe-957">Convention-based registration</span></span>

<span data-ttu-id="537fe-958">下列協力廠商容器可以與 ASP.NET Core 應用程式搭配使用：</span><span class="sxs-lookup"><span data-stu-id="537fe-958">The following 3rd party containers can be used with ASP.NET Core apps:</span></span>

* [<span data-ttu-id="537fe-959">Autofac</span><span class="sxs-lookup"><span data-stu-id="537fe-959">Autofac</span></span>](https://autofac.readthedocs.io/en/latest/integration/aspnetcore.html)
* [<span data-ttu-id="537fe-960">DryIoc</span><span class="sxs-lookup"><span data-stu-id="537fe-960">DryIoc</span></span>](https://www.nuget.org/packages/DryIoc.Microsoft.DependencyInjection)
* [<span data-ttu-id="537fe-961">Grace</span><span class="sxs-lookup"><span data-stu-id="537fe-961">Grace</span></span>](https://www.nuget.org/packages/Grace.DependencyInjection.Extensions)
* [<span data-ttu-id="537fe-962">LightInject</span><span class="sxs-lookup"><span data-stu-id="537fe-962">LightInject</span></span>](https://github.com/seesharper/LightInject.Microsoft.DependencyInjection)
* [<span data-ttu-id="537fe-963">拉馬爾</span><span class="sxs-lookup"><span data-stu-id="537fe-963">Lamar</span></span>](https://jasperfx.github.io/lamar/)
* [<span data-ttu-id="537fe-964">Stashbox</span><span class="sxs-lookup"><span data-stu-id="537fe-964">Stashbox</span></span>](https://github.com/z4kn4fein/stashbox-extensions-dependencyinjection)
* [<span data-ttu-id="537fe-965">Unity</span><span class="sxs-lookup"><span data-stu-id="537fe-965">Unity</span></span>](https://www.nuget.org/packages/Unity.Microsoft.DependencyInjection)

### <a name="thread-safety"></a><span data-ttu-id="537fe-966">執行緒安全性</span><span class="sxs-lookup"><span data-stu-id="537fe-966">Thread safety</span></span>

<span data-ttu-id="537fe-967">建立具備執行緒安全性的 singleton 服務。</span><span class="sxs-lookup"><span data-stu-id="537fe-967">Create thread-safe singleton services.</span></span> <span data-ttu-id="537fe-968">如果 singleton 服務相依於暫時性服務，暫時性服務可能也需要具備執行緒安全性，取決於 singleton 如何使用它。</span><span class="sxs-lookup"><span data-stu-id="537fe-968">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="537fe-969">單一服務的 factory 方法（例如[AddSingleton \<TService> （IServiceCollection，Func \<IServiceProvider,TService> ）](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*)的第二個引數）不需要是安全線程。</span><span class="sxs-lookup"><span data-stu-id="537fe-969">The factory method of single service, such as the second argument to [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), doesn't need to be thread-safe.</span></span> <span data-ttu-id="537fe-970">就像型別 (`static`) 建構函式一樣，它一定會被單一執行緒呼叫一次。</span><span class="sxs-lookup"><span data-stu-id="537fe-970">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="537fe-971">建議</span><span class="sxs-lookup"><span data-stu-id="537fe-971">Recommendations</span></span>

* <span data-ttu-id="537fe-972">不支援以 `async/await` 與 `Task` 為基礎的服務解析。</span><span class="sxs-lookup"><span data-stu-id="537fe-972">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="537fe-973">C# 不支援非同步建構函式，因此建議的模式是以同步方式解析服務後使用非同步方法。</span><span class="sxs-lookup"><span data-stu-id="537fe-973">C# does not support asynchronous constructors; therefore, the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="537fe-974">避免直接在服務容器中儲存資料與設定。</span><span class="sxs-lookup"><span data-stu-id="537fe-974">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="537fe-975">例如，使用者的購物車通常不應該新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="537fe-975">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="537fe-976">組態應該使用[選項模式](xref:fundamentals/configuration/options)。</span><span class="sxs-lookup"><span data-stu-id="537fe-976">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="537fe-977">同樣地，請避免只存在以允許存取某個其他物件的「資料持有者」物件。</span><span class="sxs-lookup"><span data-stu-id="537fe-977">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="537fe-978">最好是透過 DI 要求實際項目。</span><span class="sxs-lookup"><span data-stu-id="537fe-978">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="537fe-979">避免以靜態方式存取服務 (例如，以靜態方式設定 [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) 型別以四處使用)。</span><span class="sxs-lookup"><span data-stu-id="537fe-979">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) for use elsewhere).</span></span>

* <span data-ttu-id="537fe-980">避免使用*服務定位器模式*，這會混用控制策略的[反轉](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion)。</span><span class="sxs-lookup"><span data-stu-id="537fe-980">Avoid using the *service locator pattern*, which mixes [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

  * <span data-ttu-id="537fe-981"><xref:System.IServiceProvider.GetService*>當您可以改用 DI 時，請勿叫用來取得服務實例：</span><span class="sxs-lookup"><span data-stu-id="537fe-981">Don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead:</span></span>

    <span data-ttu-id="537fe-982">**正確**</span><span class="sxs-lookup"><span data-stu-id="537fe-982">**Incorrect:**</span></span>

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

    <span data-ttu-id="537fe-983">**正確**：</span><span class="sxs-lookup"><span data-stu-id="537fe-983">**Correct**:</span></span>

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

  * <span data-ttu-id="537fe-984">避免在使用時，插入用來解析相依性的 factory <xref:System.IServiceProvider.GetService*> 。</span><span class="sxs-lookup"><span data-stu-id="537fe-984">Avoid injecting a factory that resolves dependencies at runtime using <xref:System.IServiceProvider.GetService*>.</span></span>

* <span data-ttu-id="537fe-985">避免以靜態方式存取 `HttpContext` (例如 [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext))。</span><span class="sxs-lookup"><span data-stu-id="537fe-985">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span></span>

<span data-ttu-id="537fe-986">就像所有的建議集，您可能會遇到需要忽略建議的情況。</span><span class="sxs-lookup"><span data-stu-id="537fe-986">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="537fe-987">例外狀況很少見&mdash;大部分是架構本身內的特殊案例。</span><span class="sxs-lookup"><span data-stu-id="537fe-987">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="537fe-988">DI 是靜態/全域物件存取模式的「替代」\*\* 選項。</span><span class="sxs-lookup"><span data-stu-id="537fe-988">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="537fe-989">如果您將 DI 與靜態物件存取混合，則可能無法實現 DI 的優點。</span><span class="sxs-lookup"><span data-stu-id="537fe-989">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="537fe-990">其他資源</span><span class="sxs-lookup"><span data-stu-id="537fe-990">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="537fe-991">在 ASP.NET Core 使用 Dependency Injection 撰寫簡潔的程式碼 (MSDN)</span><span class="sxs-lookup"><span data-stu-id="537fe-991">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="537fe-992">明確相依性準則</span><span class="sxs-lookup"><span data-stu-id="537fe-992">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* <span data-ttu-id="537fe-993">[逆轉控制容器和相依性插入模式 (Martin Fowler)](https://www.martinfowler.com/articles/injection.html) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="537fe-993">[Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)](https://www.martinfowler.com/articles/injection.html)</span></span>
* <span data-ttu-id="537fe-994">[How to register a service with multiple interfaces in ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/) (如何使用 ASP.NET Core DI 中的多個介面註冊服務)</span><span class="sxs-lookup"><span data-stu-id="537fe-994">[How to register a service with multiple interfaces in ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)</span></span>

::: moniker-end
