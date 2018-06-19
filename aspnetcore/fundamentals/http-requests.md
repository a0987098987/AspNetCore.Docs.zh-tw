---
title: 初始化 HTTP 要求
author: stevejgordon
description: 深入了解在 ASP.NET Core 中使用 IHttpClientFactory 介面來管理邏輯 HttpClient 執行個體。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 05/02/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/http-requests
ms.openlocfilehash: 1f2c7522a10220cd9520d78846d2e897115447c2
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/07/2018
ms.locfileid: "33838757"
---
# <a name="initiate-http-requests"></a><span data-ttu-id="8256e-103">初始化 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="8256e-103">Initiate HTTP requests</span></span>

<span data-ttu-id="8256e-104">作者：[Glenn Condron](https://github.com/glennc)、[Ryan Nowak](https://github.com/rynowak) 和 [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="8256e-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="8256e-105">`IHttpClientFactory` 可以註冊及用來在應用程式中設定和建立 [HttpClient](/dotnet/api/system.net.http.httpclient) 執行個體。</span><span class="sxs-lookup"><span data-stu-id="8256e-105">An `IHttpClientFactory` can be registered and used to configure and create [HttpClient](/dotnet/api/system.net.http.httpclient) instances in an app.</span></span> <span data-ttu-id="8256e-106">它提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="8256e-106">It offers the following benefits:</span></span>

* <span data-ttu-id="8256e-107">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="8256e-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="8256e-108">例如，"github" 用戶端可以註冊並設定成存取 GitHub。</span><span class="sxs-lookup"><span data-stu-id="8256e-108">For example, a "github" client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="8256e-109">預設用戶端可以註冊用於其他用途。</span><span class="sxs-lookup"><span data-stu-id="8256e-109">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="8256e-110">透過委派 `HttpClient` 中的處理常式來撰寫外寄中介軟體的概念，並提供延伸模組以便 Polly 架構中介軟體利用外寄中介軟體。</span><span class="sxs-lookup"><span data-stu-id="8256e-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="8256e-111">管理基礎 `HttpClientMessageHandler` 執行個體的共用和存留期，以避免在手動管理 `HttpClient` 存留期時，發生的常見 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="8256e-111">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="8256e-112">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="8256e-112">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="8256e-113">耗用模式</span><span class="sxs-lookup"><span data-stu-id="8256e-113">Consumption patterns</span></span>

<span data-ttu-id="8256e-114">有數種方式可將 `IHttpClientFactory` 用於應用程式：</span><span class="sxs-lookup"><span data-stu-id="8256e-114">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="8256e-115">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="8256e-115">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="8256e-116">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="8256e-116">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="8256e-117">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="8256e-117">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="8256e-118">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="8256e-118">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="8256e-119">它們全都不會嚴格優先於另一個。</span><span class="sxs-lookup"><span data-stu-id="8256e-119">None of them are strictly superior to another.</span></span> <span data-ttu-id="8256e-120">最好的方法取決於應用程式的條件約束。</span><span class="sxs-lookup"><span data-stu-id="8256e-120">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="8256e-121">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="8256e-121">Basic usage</span></span>

<span data-ttu-id="8256e-122">`IHttpClientFactory` 可以藉由在 Startup.cs 中的 `ConfigureServices` 方法內，在 `IServiceCollection` 上呼叫 `AddHttpClient` 擴充方法來註冊。</span><span class="sxs-lookup"><span data-stu-id="8256e-122">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `ConfigureServices` method in Startup.cs.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet1)]

<span data-ttu-id="8256e-123">註冊之後，程式碼可以在可使用[相依性插入](xref:fundamentals/dependency-injection) (DI) 插入服務的任何位置，接受 `IHttpClientFactory`。</span><span class="sxs-lookup"><span data-stu-id="8256e-123">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="8256e-124">`IHttpClientFactory` 可以用來建立 `HttpClient` 執行個體：</span><span class="sxs-lookup"><span data-stu-id="8256e-124">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,20)]

<span data-ttu-id="8256e-125">以這種方式使用 `IHttpClientFactory` 是重構現有應用程式的好方法。</span><span class="sxs-lookup"><span data-stu-id="8256e-125">Using `IHttpClientFactory` in this fashion is a great way to refactor an existing app.</span></span> <span data-ttu-id="8256e-126">它對 `HttpClient` 的使用方式沒有任何影響。</span><span class="sxs-lookup"><span data-stu-id="8256e-126">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="8256e-127">在目前建立 `HttpClient` 執行個體的位置，將那些項目取代為呼叫 `CreateClient`。</span><span class="sxs-lookup"><span data-stu-id="8256e-127">In places where `HttpClient` instances are currently created, replace those occurrences with a call to `CreateClient`.</span></span>

### <a name="named-clients"></a><span data-ttu-id="8256e-128">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="8256e-128">Named clients</span></span>

<span data-ttu-id="8256e-129">如果應用程式需要使用多個不同的 `HttpClient`，且每個使用不同的組態，可以選擇使用**具名用戶端**。</span><span class="sxs-lookup"><span data-stu-id="8256e-129">If an app requires multiple distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="8256e-130">具名 `HttpClient` 的組態可以在 `ConfigureServices` 中註冊時指定。</span><span class="sxs-lookup"><span data-stu-id="8256e-130">Configuration for a named `HttpClient` can be specified during registration in `ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet2)]

<span data-ttu-id="8256e-131">在上述程式碼中，會呼叫 `AddHttpClient`，並提供名稱 "github"。</span><span class="sxs-lookup"><span data-stu-id="8256e-131">In the preceding code, `AddHttpClient` is called, providing the name "github".</span></span> <span data-ttu-id="8256e-132">此用戶端已套用一些預設組態&mdash;即使用 GitHub API 所需的基底位址和兩個標頭。</span><span class="sxs-lookup"><span data-stu-id="8256e-132">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="8256e-133">每次呼叫 `CreateClient` 時，會建立 `HttpClient` 的新執行個體並呼叫組態動作。</span><span class="sxs-lookup"><span data-stu-id="8256e-133">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="8256e-134">若要使用具名用戶端，可以傳遞字串參數至 `CreateClient`。</span><span class="sxs-lookup"><span data-stu-id="8256e-134">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="8256e-135">指定要建立之用戶端的名稱：</span><span class="sxs-lookup"><span data-stu-id="8256e-135">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=20)]

<span data-ttu-id="8256e-136">在上述程式碼中，要求不需要指定主機名稱。</span><span class="sxs-lookup"><span data-stu-id="8256e-136">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="8256e-137">它可以只傳遞路徑，因為已使用為用戶端設定的基底位址。</span><span class="sxs-lookup"><span data-stu-id="8256e-137">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="8256e-138">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="8256e-138">Typed clients</span></span>

<span data-ttu-id="8256e-139">具型別用戶端提供與具名用戶端相同的功能，而不需要使用字串作為索引鍵。</span><span class="sxs-lookup"><span data-stu-id="8256e-139">Typed clients provide the same capabilities as named clients without the need to use strings as keys.</span></span> <span data-ttu-id="8256e-140">具型別用戶端方法在使用用戶端時提供 IntelliSense 和編譯器說明。</span><span class="sxs-lookup"><span data-stu-id="8256e-140">The typed client approach provides IntelliSense and compiler help when consuming clients.</span></span> <span data-ttu-id="8256e-141">它們提供單一位置來設定特定 `HttpClient` 並與其互動。</span><span class="sxs-lookup"><span data-stu-id="8256e-141">They provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="8256e-142">例如，單一的具型別用戶端可能用於單一的後端端點，並封裝處理該端點的所有邏輯。</span><span class="sxs-lookup"><span data-stu-id="8256e-142">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span> <span data-ttu-id="8256e-143">另一個優點是它們使用 DI 且可以在應用程式中需要之處插入。</span><span class="sxs-lookup"><span data-stu-id="8256e-143">Another advantage is that they work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="8256e-144">具型別用戶端在其建構函式中接受 `HttpClient` 參數：</span><span class="sxs-lookup"><span data-stu-id="8256e-144">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="8256e-145">在上述程式碼中，組態會移到具型別用戶端。</span><span class="sxs-lookup"><span data-stu-id="8256e-145">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="8256e-146">`HttpClient` 物件會公開為公用屬性。</span><span class="sxs-lookup"><span data-stu-id="8256e-146">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="8256e-147">您可定義 API 特定的方法，其公開 `HttpClient` 功能。</span><span class="sxs-lookup"><span data-stu-id="8256e-147">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="8256e-148">`GetAspNetDocsIssues` 方法會封裝從 GitHub 存放庫查詢和剖析最新開啟問題所需的程式碼。</span><span class="sxs-lookup"><span data-stu-id="8256e-148">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="8256e-149">若要註冊具型別用戶端，泛型 `AddHttpClient` 擴充方法可用於 `ConfigureServices` 內，並指定具型別用戶端類別：</span><span class="sxs-lookup"><span data-stu-id="8256e-149">To register a typed client, the generic `AddHttpClient` extension method can be used within `ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet3)]

<span data-ttu-id="8256e-150">具型別用戶端會向 DI 註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="8256e-150">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="8256e-151">具型別用戶端可以直接插入並使用：</span><span class="sxs-lookup"><span data-stu-id="8256e-151">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="8256e-152">想要的話，具型別用戶端的組態可以在 `ConfigureServices` 中註冊時指定，而不是在具型別用戶端的建構函式中：</span><span class="sxs-lookup"><span data-stu-id="8256e-152">If preferred, the configuration for a typed client can be specified during registration in `ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet4)]

<span data-ttu-id="8256e-153">可以將 `HttpClient` 完全封裝在具型別用戶端內。</span><span class="sxs-lookup"><span data-stu-id="8256e-153">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="8256e-154">可以提供在內部呼叫 `HttpClient` 執行個體的公用方法，而不將它公開為屬性。</span><span class="sxs-lookup"><span data-stu-id="8256e-154">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/GitHub/RepoService.cs?name=snippet1&highlight=3)]

<span data-ttu-id="8256e-155">在上述程式碼，`HttpClient` 儲存為私用欄位。</span><span class="sxs-lookup"><span data-stu-id="8256e-155">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="8256e-156">進行外部呼叫的所有存取都會經歷 `GetRepos` 方法。</span><span class="sxs-lookup"><span data-stu-id="8256e-156">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="8256e-157">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="8256e-157">Generated clients</span></span>

<span data-ttu-id="8256e-158">`IHttpClientFactory` 可和其他協力廠商程式庫一起使用，例如 [Refit](https://github.com/paulcbetts/refit)。</span><span class="sxs-lookup"><span data-stu-id="8256e-158">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="8256e-159">Refit 是適用於 .NET 的 REST 程式庫。</span><span class="sxs-lookup"><span data-stu-id="8256e-159">Refit is a REST library for .NET.</span></span> <span data-ttu-id="8256e-160">它將 REST API 轉換為即時介面。</span><span class="sxs-lookup"><span data-stu-id="8256e-160">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="8256e-161">介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。</span><span class="sxs-lookup"><span data-stu-id="8256e-161">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="8256e-162">定義介面及回覆來代表外部 API 和其回應：</span><span class="sxs-lookup"><span data-stu-id="8256e-162">An interface and a reply are defined to represent the external API and its response:</span></span>

```csharp
public interface IHelloClient
{
    [Get("/helloworld")]
    Task<Reply> GetMessageAsync();
}

public class Reply
{
    public string Message { get; set; }
}
```

<span data-ttu-id="8256e-163">可以新增具型別用戶端，使用 Refit 產生實作：</span><span class="sxs-lookup"><span data-stu-id="8256e-163">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

<span data-ttu-id="8256e-164">定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：</span><span class="sxs-lookup"><span data-stu-id="8256e-164">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

```csharp
[ApiController]
public class ValuesController : ControllerBase
{
    private readonly IHelloClient _client;

    public ValuesController(IHelloClient client)
    {
        _client = client;
    }

    [HttpGet("/")]
    public async Task<ActionResult<Reply>> Index()
    {
        return await _client.GetMessageAsync();
    }
}
```

## <a name="outgoing-request-middleware"></a><span data-ttu-id="8256e-165">外寄要求中介軟體</span><span class="sxs-lookup"><span data-stu-id="8256e-165">Outgoing request middleware</span></span>

<span data-ttu-id="8256e-166">`HttpClient` 已經有委派可針對外寄 HTTP 要求連結在一起的處理常式的概念。</span><span class="sxs-lookup"><span data-stu-id="8256e-166">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="8256e-167">`IHttpClientFactory` 可讓您輕鬆地定義要套用於每個具名用戶端的處理常式。</span><span class="sxs-lookup"><span data-stu-id="8256e-167">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="8256e-168">它支援註冊和鏈結多個處理常式，以建置外寄要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="8256e-168">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="8256e-169">這些處理常式每個都可以在外寄要求之前和之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="8256e-169">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="8256e-170">此模式與 ASP.NET Core 中的輸入中介軟體管線相似。</span><span class="sxs-lookup"><span data-stu-id="8256e-170">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="8256e-171">模式提供一個機制來管理 HTTP 要求的跨領域關注，包括快取、錯誤處理、序列化和記錄。</span><span class="sxs-lookup"><span data-stu-id="8256e-171">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="8256e-172">若要建立處理常式，請定義衍生自 `DelegatingHandler` 的類別。</span><span class="sxs-lookup"><span data-stu-id="8256e-172">To create a handler, define a class deriving from `DelegatingHandler`.</span></span> <span data-ttu-id="8256e-173">覆寫 `SendAsync` 方法，以在將要求傳遞至管線中的下一個處理常式之前執行程式碼：</span><span class="sxs-lookup"><span data-stu-id="8256e-173">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[Main](http-requests/samples/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="8256e-174">上述程式碼定義一個基本處理常式。</span><span class="sxs-lookup"><span data-stu-id="8256e-174">The preceding code defines a basic handler.</span></span> <span data-ttu-id="8256e-175">它會檢查以查看要求上是否已包含 X-API-KEY 標頭。</span><span class="sxs-lookup"><span data-stu-id="8256e-175">It checks to see if an X-API-KEY header has been included on the request.</span></span> <span data-ttu-id="8256e-176">如果遺漏標頭，它可以避免 HTTP 呼叫，並傳回適當的回應。</span><span class="sxs-lookup"><span data-stu-id="8256e-176">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="8256e-177">在註冊期間，可以新增一或多個處理常式至 `HttpClient` 的組態。</span><span class="sxs-lookup"><span data-stu-id="8256e-177">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="8256e-178">這項工作是透過 `IHttpClientBuilder` 上的擴充方法完成。</span><span class="sxs-lookup"><span data-stu-id="8256e-178">This task is accomplished via extension methods on the `IHttpClientBuilder`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet5)]

<span data-ttu-id="8256e-179">在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。</span><span class="sxs-lookup"><span data-stu-id="8256e-179">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="8256e-180">處理常式**必須**在 DI 中註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="8256e-180">The handler **must** be registered in DI as transient.</span></span> <span data-ttu-id="8256e-181">註冊之後，便可以呼叫 `AddHttpMessageHandler`，並傳入處理常式的類型。</span><span class="sxs-lookup"><span data-stu-id="8256e-181">Once registered, `AddHttpMessageHandler` can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="8256e-182">可以遵循應該執行的順序來註冊多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="8256e-182">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="8256e-183">每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：</span><span class="sxs-lookup"><span data-stu-id="8256e-183">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet6)]

## <a name="use-polly-based-handlers"></a><span data-ttu-id="8256e-184">使用 Polly 為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="8256e-184">Use Polly-based handlers</span></span>

<span data-ttu-id="8256e-185">`IHttpClientFactory` 整合受歡迎的協力廠商程式庫，稱為 [Polly](https://github.com/App-vNext/Polly)。</span><span class="sxs-lookup"><span data-stu-id="8256e-185">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="8256e-186">Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。</span><span class="sxs-lookup"><span data-stu-id="8256e-186">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="8256e-187">它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。</span><span class="sxs-lookup"><span data-stu-id="8256e-187">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="8256e-188">提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="8256e-188">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="8256e-189">Polly 延伸模組提供於稱為 'Microsoft.Extensions.Http.Polly' 的 NuGet 套件中。</span><span class="sxs-lookup"><span data-stu-id="8256e-189">The Polly extensions are available in a NuGet package called 'Microsoft.Extensions.Http.Polly'.</span></span> <span data-ttu-id="8256e-190">此套件預設不包含在 'Microsoft.AspNetCore.App' 中繼套件中。</span><span class="sxs-lookup"><span data-stu-id="8256e-190">This package is not included by default by the 'Microsoft.AspNetCore.App' metapackage.</span></span> <span data-ttu-id="8256e-191">若要使用延伸模組，PackageReference 應該明確地包含在專案中。</span><span class="sxs-lookup"><span data-stu-id="8256e-191">To use the extensions, a PackageReference should be explicitly included in the project.</span></span>

[!code-csharp[](http-requests/samples/HttpClientFactorySample.csproj?highlight=9)]

<span data-ttu-id="8256e-192">在還原此套件之後，擴充方法可用來支援將 Polly 為基礎的處理常式新增至用戶端。</span><span class="sxs-lookup"><span data-stu-id="8256e-192">After restoring this package, extension methods are available to support adding Polly-based handlers to clients.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="8256e-193">處理暫時性錯誤</span><span class="sxs-lookup"><span data-stu-id="8256e-193">Handle transient faults</span></span>

<span data-ttu-id="8256e-194">進行外部 HTTP 呼叫時，您可能預期發生的常見錯誤將會是暫時性。</span><span class="sxs-lookup"><span data-stu-id="8256e-194">The most common faults you may expect to occur when making external HTTP calls will be transient.</span></span> <span data-ttu-id="8256e-195">包含一個便利的擴充方法，稱為 `AddTransientHttpErrorPolicy`，它可允許定義原則來處理暫時性錯誤。</span><span class="sxs-lookup"><span data-stu-id="8256e-195">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="8256e-196">使用此延伸模組方法設定的原則，會處理 `HttpRequestException`、HTTP 5xx 回應和 HTTP 408 回應。</span><span class="sxs-lookup"><span data-stu-id="8256e-196">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="8256e-197">`AddTransientHttpErrorPolicy` 延伸模組可用於 `ConfigureServices` 內。</span><span class="sxs-lookup"><span data-stu-id="8256e-197">The `AddTransientHttpErrorPolicy` extension can be used within `ConfigureServices`.</span></span> <span data-ttu-id="8256e-198">延伸模組能提供 `PolicyBuilder` 物件的存取，該物件已設定來處理代表可能暫時性錯誤的錯誤：</span><span class="sxs-lookup"><span data-stu-id="8256e-198">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet7)]

<span data-ttu-id="8256e-199">在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。</span><span class="sxs-lookup"><span data-stu-id="8256e-199">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="8256e-200">失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。</span><span class="sxs-lookup"><span data-stu-id="8256e-200">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="8256e-201">動態選取原則</span><span class="sxs-lookup"><span data-stu-id="8256e-201">Dynamically select policies</span></span>

<span data-ttu-id="8256e-202">有額外的擴充方法可用來新增 Polly 為基礎的處理常式。</span><span class="sxs-lookup"><span data-stu-id="8256e-202">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="8256e-203">其中一個這類延伸模組是 `AddPolicyHandler`，它有多個多載。</span><span class="sxs-lookup"><span data-stu-id="8256e-203">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="8256e-204">一個多載可讓您在定義要套用的原則時，檢查要求：</span><span class="sxs-lookup"><span data-stu-id="8256e-204">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet8)]

<span data-ttu-id="8256e-205">在上述程式碼中，如果外寄要求是 GET，就會套用 10 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="8256e-205">In the preceding code, if the outgoing request is a GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="8256e-206">任何其他 HTTP 方法會使用 30 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="8256e-206">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="8256e-207">新增多個 Polly 處理常式</span><span class="sxs-lookup"><span data-stu-id="8256e-207">Add multiple Polly handlers</span></span>

<span data-ttu-id="8256e-208">通常會建立巢狀 Polly 原則，以提供增強的功能：</span><span class="sxs-lookup"><span data-stu-id="8256e-208">It is common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet9)]

<span data-ttu-id="8256e-209">在上述範例中，會新增兩個處理常式。</span><span class="sxs-lookup"><span data-stu-id="8256e-209">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="8256e-210">第一個使用 `AddTransientHttpErrorPolicy` 延伸模組來新增重試原則。</span><span class="sxs-lookup"><span data-stu-id="8256e-210">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="8256e-211">失敗的要求會重試最多三次。</span><span class="sxs-lookup"><span data-stu-id="8256e-211">Failed requests are retried up to three times.</span></span> <span data-ttu-id="8256e-212">第二個 `AddTransientHttpErrorPolicy` 呼叫會新增斷路器原則。</span><span class="sxs-lookup"><span data-stu-id="8256e-212">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="8256e-213">如果循序發生五次失敗的嘗試，進一步的外部要求會遭到封鎖 30 秒。</span><span class="sxs-lookup"><span data-stu-id="8256e-213">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="8256e-214">斷路器原則可設定狀態。</span><span class="sxs-lookup"><span data-stu-id="8256e-214">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="8256e-215">透過此用戶端的所有呼叫都會共用相同的線路狀態。</span><span class="sxs-lookup"><span data-stu-id="8256e-215">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="8256e-216">從 Polly 登錄新增原則</span><span class="sxs-lookup"><span data-stu-id="8256e-216">Add policies from the Polly registry</span></span>

<span data-ttu-id="8256e-217">管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。</span><span class="sxs-lookup"><span data-stu-id="8256e-217">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="8256e-218">提供了擴充方法，可以使用來自登錄的原則新增處理常式：</span><span class="sxs-lookup"><span data-stu-id="8256e-218">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet10)]

<span data-ttu-id="8256e-219">在上述程式碼中，PolicyRegistry 已新增至 `ServiceCollection`，且兩個原則已向它註冊。</span><span class="sxs-lookup"><span data-stu-id="8256e-219">In the preceding code, a PolicyRegistry is added to the `ServiceCollection` and two policies are registered with it.</span></span> <span data-ttu-id="8256e-220">為了使用來自登錄的原則，使用了 `AddPolicyHandlerFromRegistry` 方法，並傳遞要套用的原則名稱。</span><span class="sxs-lookup"><span data-stu-id="8256e-220">In order to use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="8256e-221">關於 `IHttpClientFactory` Polly 整合的詳細資訊，可以在 [Polly Wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory) 上找到。</span><span class="sxs-lookup"><span data-stu-id="8256e-221">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="8256e-222">HttpClient 和存留期管理</span><span class="sxs-lookup"><span data-stu-id="8256e-222">HttpClient and lifetime management</span></span>

<span data-ttu-id="8256e-223">每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回 `HttpClient` 的新執行個體。</span><span class="sxs-lookup"><span data-stu-id="8256e-223">Each time `CreateClient` is called on the `IHttpClientFactory`, a new instance of a `HttpClient` is returned.</span></span> <span data-ttu-id="8256e-224">每個具名用戶端會有一個 `HttpMessageHandler`。</span><span class="sxs-lookup"><span data-stu-id="8256e-224">There will be a `HttpMessageHandler` per named client.</span></span> <span data-ttu-id="8256e-225">`IHttpClientFactory` 會集合處理站所建立的 `HttpMessageHandler` 執行個體以減少資源耗用量。</span><span class="sxs-lookup"><span data-stu-id="8256e-225">`IHttpClientFactory` will pool the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="8256e-226">建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。</span><span class="sxs-lookup"><span data-stu-id="8256e-226">A `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span> 

<span data-ttu-id="8256e-227">處理常式的集合是需要的做法，因為每個處理常式通常會管理自己的基礎 HTTP 連線；建立比所需數目更多的處理常式可能會導致連線延遲。</span><span class="sxs-lookup"><span data-stu-id="8256e-227">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections; creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="8256e-228">有些處理常式也會保持連線無限期地開啟，這可能導致處理常式無法對 DNS 變更回應。</span><span class="sxs-lookup"><span data-stu-id="8256e-228">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="8256e-229">預設處理常式存留時間為兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="8256e-229">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="8256e-230">可以針對每個具名用戶端覆寫預設值。</span><span class="sxs-lookup"><span data-stu-id="8256e-230">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="8256e-231">若要覆寫它，請在建立用戶端時所傳回的 `IHttpClientBuilder` 上呼叫 `SetHandlerLifetime`：</span><span class="sxs-lookup"><span data-stu-id="8256e-231">To override it, call `SetHandlerLifetime` on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet11)]

## <a name="logging"></a><span data-ttu-id="8256e-232">記錄</span><span class="sxs-lookup"><span data-stu-id="8256e-232">Logging</span></span>

<span data-ttu-id="8256e-233">透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="8256e-233">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="8256e-234">您必須在記錄組態中啟用適當的資訊層級，以查看預設記錄檔訊息。</span><span class="sxs-lookup"><span data-stu-id="8256e-234">You'll need to enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="8256e-235">額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。</span><span class="sxs-lookup"><span data-stu-id="8256e-235">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="8256e-236">用於每個用戶端的記錄檔分類包含用戶端的名稱。</span><span class="sxs-lookup"><span data-stu-id="8256e-236">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="8256e-237">例如，名為 "MyNamedClient" 的用戶端，會記錄分類為 `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler` 的訊息。</span><span class="sxs-lookup"><span data-stu-id="8256e-237">A client named "MyNamedClient", for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="8256e-238">具有後置詞 "LogicalHandler" 的訊息發生在要求處理常式管線之外。</span><span class="sxs-lookup"><span data-stu-id="8256e-238">Messages with the suffix of "LogicalHandler" occur on the outside of request handler pipeline.</span></span> <span data-ttu-id="8256e-239">在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。</span><span class="sxs-lookup"><span data-stu-id="8256e-239">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="8256e-240">在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。</span><span class="sxs-lookup"><span data-stu-id="8256e-240">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="8256e-241">記錄也會發生在要求處理常式管線之內。</span><span class="sxs-lookup"><span data-stu-id="8256e-241">Logging also occurs on the inside of the request handler pipeline.</span></span> <span data-ttu-id="8256e-242">在 "MyNamedClient" 範例的案例中，那些訊息是針對記錄檔分類 `System.Net.Http.HttpClient.MyNamedClient.ClientHandler` 而記錄。</span><span class="sxs-lookup"><span data-stu-id="8256e-242">In the case of the "MyNamedClient" example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="8256e-243">對於要求，這是發生在所有其他處理常式都已執行之後，並且緊接在網路上傳送要求之前。</span><span class="sxs-lookup"><span data-stu-id="8256e-243">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="8256e-244">在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。</span><span class="sxs-lookup"><span data-stu-id="8256e-244">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="8256e-245">在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。</span><span class="sxs-lookup"><span data-stu-id="8256e-245">Enabling logging on the outside and inside of the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="8256e-246">例如，這可能包括要求標頭的變更，或是回應狀態碼的變更。</span><span class="sxs-lookup"><span data-stu-id="8256e-246">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="8256e-247">在記錄分類中包含用戶端的名稱，可讓您在需要時進行特定具名用戶端的記錄檔篩選。</span><span class="sxs-lookup"><span data-stu-id="8256e-247">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="8256e-248">設定 HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="8256e-248">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="8256e-249">可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。</span><span class="sxs-lookup"><span data-stu-id="8256e-249">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="8256e-250">新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。</span><span class="sxs-lookup"><span data-stu-id="8256e-250">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="8256e-251">`ConfigurePrimaryHttpMessageHandler` 擴充方法可以用來定義委派。</span><span class="sxs-lookup"><span data-stu-id="8256e-251">The `ConfigurePrimaryHttpMessageHandler` extension method can be used to define a delegate.</span></span> <span data-ttu-id="8256e-252">委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：</span><span class="sxs-lookup"><span data-stu-id="8256e-252">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet12)]
