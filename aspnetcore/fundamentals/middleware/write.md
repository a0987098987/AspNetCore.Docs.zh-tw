---
<span data-ttu-id="d72e2-101">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d72e2-101">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d72e2-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d72e2-102">'Blazor'</span></span>
- <span data-ttu-id="d72e2-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d72e2-103">'Identity'</span></span>
- <span data-ttu-id="d72e2-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d72e2-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="d72e2-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d72e2-105">'Razor'</span></span>
- <span data-ttu-id="d72e2-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d72e2-106">'SignalR' uid:</span></span> 

---
# <a name="write-custom-aspnet-core-middleware"></a><span data-ttu-id="d72e2-107">撰寫自訂的 ASP.NET Core 中介軟體</span><span class="sxs-lookup"><span data-stu-id="d72e2-107">Write custom ASP.NET Core middleware</span></span>

<span data-ttu-id="d72e2-108">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫</span><span class="sxs-lookup"><span data-stu-id="d72e2-108">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="d72e2-109">中介軟體為組成應用程式管線的軟體，用以處理要求與回應。</span><span class="sxs-lookup"><span data-stu-id="d72e2-109">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="d72e2-110">ASP.NET Core 提供一組豐富的內建中介軟體元件，但在某些情況下，您可能想要撰寫自訂的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="d72e2-110">ASP.NET Core provides a rich set of built-in middleware components, but in some scenarios you might want to write a custom middleware.</span></span>

> [!NOTE]
> <span data-ttu-id="d72e2-111">本主題說明如何撰寫以*慣例為基礎的*中介軟體。</span><span class="sxs-lookup"><span data-stu-id="d72e2-111">This topic describes how to write *convention-based* middleware.</span></span> <span data-ttu-id="d72e2-112">如需使用強式類型和每個要求啟用的方法，請參閱 <xref:fundamentals/middleware/extensibility> 。</span><span class="sxs-lookup"><span data-stu-id="d72e2-112">For an approach that uses strong typing and per-request activation, see <xref:fundamentals/middleware/extensibility>.</span></span>

## <a name="middleware-class"></a><span data-ttu-id="d72e2-113">中介軟體類別</span><span class="sxs-lookup"><span data-stu-id="d72e2-113">Middleware class</span></span>

<span data-ttu-id="d72e2-114">中介軟體通常封裝在類別中，並以擴充方法公開。</span><span class="sxs-lookup"><span data-stu-id="d72e2-114">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="d72e2-115">請考慮下列中介軟體，其會為來自查詢字串的目前要求設定文化特性：</span><span class="sxs-lookup"><span data-stu-id="d72e2-115">Consider the following middleware, which sets the culture for the current request from a query string:</span></span>

[!code-csharp[](write/snapshot/StartupCulture.cs)]

<span data-ttu-id="d72e2-116">上述範例程式碼用於示範中介軟體元件的建立。</span><span class="sxs-lookup"><span data-stu-id="d72e2-116">The preceding sample code is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="d72e2-117">如需 ASP.NET Core 的內建當地語系化支援，請參閱 <xref:fundamentals/localization>。</span><span class="sxs-lookup"><span data-stu-id="d72e2-117">For ASP.NET Core's built-in localization support, see <xref:fundamentals/localization>.</span></span>

<span data-ttu-id="d72e2-118">藉由傳入文化特性來測試中介軟體。</span><span class="sxs-lookup"><span data-stu-id="d72e2-118">Test the middleware by passing in the culture.</span></span> <span data-ttu-id="d72e2-119">例如，要求 `https://localhost:5001/?culture=no`。</span><span class="sxs-lookup"><span data-stu-id="d72e2-119">For example, request `https://localhost:5001/?culture=no`.</span></span>

<span data-ttu-id="d72e2-120">下列程式碼會將中介軟體委派移至類別：</span><span class="sxs-lookup"><span data-stu-id="d72e2-120">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](write/snapshot/RequestCultureMiddleware.cs)]

<span data-ttu-id="d72e2-121">中介軟體類別必須包含：</span><span class="sxs-lookup"><span data-stu-id="d72e2-121">The middleware class must include:</span></span>

* <span data-ttu-id="d72e2-122">具有 <xref:Microsoft.AspNetCore.Http.RequestDelegate> 類型參數的公用建構函式。</span><span class="sxs-lookup"><span data-stu-id="d72e2-122">A public constructor with a parameter of type <xref:Microsoft.AspNetCore.Http.RequestDelegate>.</span></span>
* <span data-ttu-id="d72e2-123">名為 `Invoke` 或 `InvokeAsync` 的公用方法。</span><span class="sxs-lookup"><span data-stu-id="d72e2-123">A public method named `Invoke` or `InvokeAsync`.</span></span> <span data-ttu-id="d72e2-124">此方法必須：</span><span class="sxs-lookup"><span data-stu-id="d72e2-124">This method must:</span></span>
  * <span data-ttu-id="d72e2-125">傳回 `Task`。</span><span class="sxs-lookup"><span data-stu-id="d72e2-125">Return a `Task`.</span></span>
  * <span data-ttu-id="d72e2-126">接受 <xref:Microsoft.AspNetCore.Http.HttpContext> 類型的第一個參數。</span><span class="sxs-lookup"><span data-stu-id="d72e2-126">Accept a first parameter of type <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>
  
<span data-ttu-id="d72e2-127">建構函式和 `Invoke`/`InvokeAsync` 的其他參數會由[相依性插入 (DI)](xref:fundamentals/dependency-injection) 所填入。</span><span class="sxs-lookup"><span data-stu-id="d72e2-127">Additional parameters for the constructor and `Invoke`/`InvokeAsync` are populated by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware-dependencies"></a><span data-ttu-id="d72e2-128">中介軟體相依性</span><span class="sxs-lookup"><span data-stu-id="d72e2-128">Middleware dependencies</span></span>

<span data-ttu-id="d72e2-129">中介軟體應於其建構函式中公開其相依性，以遵循[明確的相依性原則](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)。</span><span class="sxs-lookup"><span data-stu-id="d72e2-129">Middleware should follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="d72e2-130">中介軟體會在每次「應用程式存留期」\*\* 就建構一次。</span><span class="sxs-lookup"><span data-stu-id="d72e2-130">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="d72e2-131">若您需要在要求內與中介軟體共用服務，請參閱[依要求的中介軟體相依性](#per-request-middleware-dependencies)一節。</span><span class="sxs-lookup"><span data-stu-id="d72e2-131">See the [Per-request middleware dependencies](#per-request-middleware-dependencies) section if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="d72e2-132">中介軟體元件可透過建構函式參數，解析其來自[相依性插入 (DI)](xref:fundamentals/dependency-injection) 的相依性。</span><span class="sxs-lookup"><span data-stu-id="d72e2-132">Middleware components can resolve their dependencies from [dependency injection (DI)](xref:fundamentals/dependency-injection) through constructor parameters.</span></span> <span data-ttu-id="d72e2-133">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) 也可直接接受其它參數。</span><span class="sxs-lookup"><span data-stu-id="d72e2-133">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) can also accept additional parameters directly.</span></span>

## <a name="per-request-middleware-dependencies"></a><span data-ttu-id="d72e2-134">依要求的中介軟體相依性</span><span class="sxs-lookup"><span data-stu-id="d72e2-134">Per-request middleware dependencies</span></span>

<span data-ttu-id="d72e2-135">因為中介軟體建構於應用程式啟動時，而非依要求建構，所以在每個要求期間，中介軟體建構函式使用的「已限定範圍」\*\* 存留期服務不會與其它插入相依性的類型共用。</span><span class="sxs-lookup"><span data-stu-id="d72e2-135">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors aren't shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="d72e2-136">如果您必須在中介軟體和其他類型間共用「已限定範圍」\*\* 的服務，請將這些服務新增至 `Invoke` 方法的簽章。</span><span class="sxs-lookup"><span data-stu-id="d72e2-136">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="d72e2-137">`Invoke` 方法可以接受 DI 所填入的其他參數：</span><span class="sxs-lookup"><span data-stu-id="d72e2-137">The `Invoke` method can accept additional parameters that are populated by DI:</span></span>

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // IMyScopedService is injected into Invoke
    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="middleware-extension-method"></a><span data-ttu-id="d72e2-138">中介軟體擴充方法</span><span class="sxs-lookup"><span data-stu-id="d72e2-138">Middleware extension method</span></span>

<span data-ttu-id="d72e2-139">下列擴充方法透過 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 公開中介軟體：</span><span class="sxs-lookup"><span data-stu-id="d72e2-139">The following extension method exposes the middleware through <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span></span>

[!code-csharp[](write/snapshot/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="d72e2-140">下列程式碼會從 `Startup.Configure` 呼叫中介軟體：</span><span class="sxs-lookup"><span data-stu-id="d72e2-140">The following code calls the middleware from `Startup.Configure`:</span></span>

[!code-csharp[](write/snapshot/Startup.cs?highlight=5)]

## <a name="additional-resources"></a><span data-ttu-id="d72e2-141">其他資源</span><span class="sxs-lookup"><span data-stu-id="d72e2-141">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* <xref:test/middleware>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
