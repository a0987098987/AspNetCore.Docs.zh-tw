---
title: 在 ASP.NET Core 中使用 IHttpClientFactory 發出 HTTP 要求
author: stevejgordon
description: 深入了解在 ASP.NET Core 中使用 IHttpClientFactory 介面來管理邏輯 HttpClient 執行個體。
ms.author: scaddie
ms.custom: mvc
ms.date: 11/27/2019
uid: fundamentals/http-requests
ms.openlocfilehash: 746604bc92775a6fac124ee8bfcf37635786fe41
ms.sourcegitcommit: 3b6b0a54b20dc99b0c8c5978400c60adf431072f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2019
ms.locfileid: "74717005"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="db924-103">在 ASP.NET Core 中使用 IHttpClientFactory 發出 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="db924-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="db924-104">作者： [Glenn Condron](https://github.com/glennc)、 [Ryan Nowak](https://github.com/rynowak)、 [Steve Gordon](https://github.com/stevejgordon)、 [Rick Anderson](https://twitter.com/RickAndMSFT)和[Kirk Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="db924-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="db924-105"><xref:System.Net.Http.IHttpClientFactory> 可以註冊及用來在應用程式中設定和建立 <xref:System.Net.Http.HttpClient> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="db924-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="db924-106">`IHttpClientFactory` 提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="db924-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="db924-107">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="db924-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="db924-108">例如，名為*github*的用戶端可以註冊並設定為存取[github](https://github.com/)。</span><span class="sxs-lookup"><span data-stu-id="db924-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="db924-109">預設用戶端可以註冊以進行一般存取。</span><span class="sxs-lookup"><span data-stu-id="db924-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="db924-110">透過 `HttpClient`中的委派處理常式，制訂外寄中介軟體的概念。</span><span class="sxs-lookup"><span data-stu-id="db924-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="db924-111">提供 Polly 為基礎中介軟體的延伸模組，以利用 `HttpClient`中的委派處理常式。</span><span class="sxs-lookup"><span data-stu-id="db924-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="db924-112">管理基礎 `HttpClientMessageHandler` 實例的共用和存留期。</span><span class="sxs-lookup"><span data-stu-id="db924-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="db924-113">自動管理可避免在手動管理 `HttpClient` 存留期時所發生的常見 DNS （網域名稱系統）問題。</span><span class="sxs-lookup"><span data-stu-id="db924-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="db924-114">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="db924-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="db924-115">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="db924-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="db924-116">本主題中的範例程式碼會使用 <xref:System.Text.Json> 來還原序列化 HTTP 回應中所傳回的 JSON 內容。</span><span class="sxs-lookup"><span data-stu-id="db924-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="db924-117">如需使用 `Json.NET` 和 `ReadAsAsync<T>`的範例，請使用版本選取器來選取此主題的2.x 版。</span><span class="sxs-lookup"><span data-stu-id="db924-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="db924-118">耗用模式</span><span class="sxs-lookup"><span data-stu-id="db924-118">Consumption patterns</span></span>

<span data-ttu-id="db924-119">有數種方式可將 `IHttpClientFactory` 用於應用程式：</span><span class="sxs-lookup"><span data-stu-id="db924-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="db924-120">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="db924-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="db924-121">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="db924-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="db924-122">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="db924-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="db924-123">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="db924-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="db924-124">最佳方法取決於應用程式的需求。</span><span class="sxs-lookup"><span data-stu-id="db924-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="db924-125">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="db924-125">Basic usage</span></span>

<span data-ttu-id="db924-126">`IHttpClientFactory` 可以藉由呼叫 `AddHttpClient`進行註冊：</span><span class="sxs-lookup"><span data-stu-id="db924-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="db924-127">`IHttpClientFactory` 可以使用相依性[插入（DI）](xref:fundamentals/dependency-injection)來要求。</span><span class="sxs-lookup"><span data-stu-id="db924-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="db924-128">下列程式碼會使用 `IHttpClientFactory` 來建立 `HttpClient` 實例：</span><span class="sxs-lookup"><span data-stu-id="db924-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="db924-129">使用上述範例中的 `IHttpClientFactory` 就是重構現有應用程式的好方法。</span><span class="sxs-lookup"><span data-stu-id="db924-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="db924-130">這不會影響使用 `HttpClient` 的方式。</span><span class="sxs-lookup"><span data-stu-id="db924-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="db924-131">在現有應用程式中建立 `HttpClient` 實例的位置，使用 <xref:System.Net.Http.IHttpClientFactory.CreateClient*>的呼叫來取代這些專案。</span><span class="sxs-lookup"><span data-stu-id="db924-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="db924-132">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="db924-132">Named clients</span></span>

<span data-ttu-id="db924-133">在下列情況中，命名的用戶端是不錯的選擇：</span><span class="sxs-lookup"><span data-stu-id="db924-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="db924-134">應用程式需要 `HttpClient`的許多不同用途。</span><span class="sxs-lookup"><span data-stu-id="db924-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="db924-135">許多 `HttpClient`都有不同的設定。</span><span class="sxs-lookup"><span data-stu-id="db924-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="db924-136">在 `Startup.ConfigureServices`註冊期間，可以指定已命名 `HttpClient` 的設定：</span><span class="sxs-lookup"><span data-stu-id="db924-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="db924-137">在上述程式碼中，用戶端是使用下列設定：</span><span class="sxs-lookup"><span data-stu-id="db924-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="db924-138">基底位址 `https://api.github.com/`。</span><span class="sxs-lookup"><span data-stu-id="db924-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="db924-139">使用 GitHub API 時需要兩個標頭。</span><span class="sxs-lookup"><span data-stu-id="db924-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="db924-140">CreateClient</span><span class="sxs-lookup"><span data-stu-id="db924-140">CreateClient</span></span>

<span data-ttu-id="db924-141">每次呼叫 <xref:System.Net.Http.IHttpClientFactory.CreateClient*> 時：</span><span class="sxs-lookup"><span data-stu-id="db924-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="db924-142">建立 `HttpClient` 的新實例。</span><span class="sxs-lookup"><span data-stu-id="db924-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="db924-143">會呼叫設定動作。</span><span class="sxs-lookup"><span data-stu-id="db924-143">The configuration action is called.</span></span>

<span data-ttu-id="db924-144">若要建立名為的用戶端，請將其名稱傳遞至 `CreateClient`：</span><span class="sxs-lookup"><span data-stu-id="db924-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="db924-145">在上述程式碼中，要求不需要指定主機名稱。</span><span class="sxs-lookup"><span data-stu-id="db924-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="db924-146">此程式碼只會傳遞路徑，因為會使用為用戶端設定的基底位址。</span><span class="sxs-lookup"><span data-stu-id="db924-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="db924-147">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="db924-147">Typed clients</span></span>

<span data-ttu-id="db924-148">具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="db924-148">Typed clients:</span></span>

* <span data-ttu-id="db924-149">提供與具名用戶端相同的功能，而不需使用字串作為索引鍵。</span><span class="sxs-lookup"><span data-stu-id="db924-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="db924-150">取用用戶端時提供 IntelliSense 和編譯器說明。</span><span class="sxs-lookup"><span data-stu-id="db924-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="db924-151">提供單一位置來設定特定的 `HttpClient` 並與其互動。</span><span class="sxs-lookup"><span data-stu-id="db924-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="db924-152">例如，可能會使用單一具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="db924-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="db924-153">適用于單一後端端點。</span><span class="sxs-lookup"><span data-stu-id="db924-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="db924-154">封裝處理端點的所有邏輯。</span><span class="sxs-lookup"><span data-stu-id="db924-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="db924-155">使用 DI，並可在應用程式中需要的位置插入。</span><span class="sxs-lookup"><span data-stu-id="db924-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="db924-156">具型別用戶端在其建構函式中接受 `HttpClient` 參數：</span><span class="sxs-lookup"><span data-stu-id="db924-156">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="db924-157">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="db924-157">In the preceding code:</span></span>

* <span data-ttu-id="db924-158">設定會移到具型別用戶端。</span><span class="sxs-lookup"><span data-stu-id="db924-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="db924-159">`HttpClient` 物件會公開為公用屬性。</span><span class="sxs-lookup"><span data-stu-id="db924-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="db924-160">可以建立可公開 `HttpClient` 功能的 API 特定方法。</span><span class="sxs-lookup"><span data-stu-id="db924-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="db924-161">例如，`GetAspNetDocsIssues` 方法會封裝程式碼以取得未解決的問題。</span><span class="sxs-lookup"><span data-stu-id="db924-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="db924-162">下列程式碼會呼叫 `Startup.ConfigureServices` 中的 <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> 來註冊具類型的用戶端類別：</span><span class="sxs-lookup"><span data-stu-id="db924-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="db924-163">具型別用戶端會向 DI 註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="db924-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="db924-164">具型別用戶端可以直接插入並使用：</span><span class="sxs-lookup"><span data-stu-id="db924-164">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="db924-165">型別用戶端的設定可以在 `Startup.ConfigureServices`註冊期間指定，而不是在具型別用戶端的函式中：</span><span class="sxs-lookup"><span data-stu-id="db924-165">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="db924-166">`HttpClient` 可以封裝在具型別用戶端中。</span><span class="sxs-lookup"><span data-stu-id="db924-166">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="db924-167">請定義在內部呼叫 `HttpClient` 實例的方法，而不是將它公開為屬性：</span><span class="sxs-lookup"><span data-stu-id="db924-167">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="db924-168">在上述程式碼中，`HttpClient` 儲存在私用欄位中。</span><span class="sxs-lookup"><span data-stu-id="db924-168">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="db924-169">`HttpClient` 的存取權是公用 `GetRepos` 方法。</span><span class="sxs-lookup"><span data-stu-id="db924-169">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="db924-170">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="db924-170">Generated clients</span></span>

<span data-ttu-id="db924-171">`IHttpClientFactory` 可以與協力廠商程式庫搭配使用，例如重新[調整。](https://github.com/paulcbetts/refit)</span><span class="sxs-lookup"><span data-stu-id="db924-171">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="db924-172">Refit 是適用於 .NET 的 REST 程式庫。</span><span class="sxs-lookup"><span data-stu-id="db924-172">Refit is a REST library for .NET.</span></span> <span data-ttu-id="db924-173">它將 REST API 轉換為即時介面。</span><span class="sxs-lookup"><span data-stu-id="db924-173">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="db924-174">介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。</span><span class="sxs-lookup"><span data-stu-id="db924-174">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="db924-175">定義介面及回覆來代表外部 API 和其回應：</span><span class="sxs-lookup"><span data-stu-id="db924-175">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="db924-176">可以新增具型別用戶端，使用 Refit 產生實作：</span><span class="sxs-lookup"><span data-stu-id="db924-176">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddControllers();
}
```

<span data-ttu-id="db924-177">定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：</span><span class="sxs-lookup"><span data-stu-id="db924-177">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="db924-178">外寄要求中介軟體</span><span class="sxs-lookup"><span data-stu-id="db924-178">Outgoing request middleware</span></span>

<span data-ttu-id="db924-179">`HttpClient` 具有委派處理常式的概念，可以針對傳出 HTTP 要求連結在一起。</span><span class="sxs-lookup"><span data-stu-id="db924-179">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="db924-180">`IHttpClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="db924-180">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="db924-181">簡化定義要套用至每個已命名用戶端的處理常式。</span><span class="sxs-lookup"><span data-stu-id="db924-181">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="db924-182">支援多個處理常式的註冊和連結，以建立外寄要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="db924-182">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="db924-183">這些處理常式每個都可以在外寄要求之前和之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="db924-183">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="db924-184">此模式：</span><span class="sxs-lookup"><span data-stu-id="db924-184">This pattern:</span></span>

  * <span data-ttu-id="db924-185">類似 ASP.NET Core 中的輸入中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="db924-185">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="db924-186">提供一種機制來管理 HTTP 要求的跨領域考慮，例如：</span><span class="sxs-lookup"><span data-stu-id="db924-186">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="db924-187">快取</span><span class="sxs-lookup"><span data-stu-id="db924-187">caching</span></span>
    * <span data-ttu-id="db924-188">錯誤處理</span><span class="sxs-lookup"><span data-stu-id="db924-188">error handling</span></span>
    * <span data-ttu-id="db924-189">serialization</span><span class="sxs-lookup"><span data-stu-id="db924-189">serialization</span></span>
    * <span data-ttu-id="db924-190">記錄</span><span class="sxs-lookup"><span data-stu-id="db924-190">logging</span></span>

<span data-ttu-id="db924-191">若要建立委派處理常式：</span><span class="sxs-lookup"><span data-stu-id="db924-191">To create a delegating handler:</span></span>

* <span data-ttu-id="db924-192">衍生自 <xref:System.Net.Http.DelegatingHandler>。</span><span class="sxs-lookup"><span data-stu-id="db924-192">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="db924-193">覆寫 <xref:System.Net.Http.DelegatingHandler.SendAsync*>。</span><span class="sxs-lookup"><span data-stu-id="db924-193">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="db924-194">先執行程式碼，再將要求傳遞至管線中的下一個處理常式：</span><span class="sxs-lookup"><span data-stu-id="db924-194">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="db924-195">上述程式碼會檢查 `X-API-KEY` 標頭是否在要求中。</span><span class="sxs-lookup"><span data-stu-id="db924-195">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="db924-196">如果遺漏 `X-API-KEY`，則會傳回 <xref:System.Net.HttpStatusCode.BadRequest>。</span><span class="sxs-lookup"><span data-stu-id="db924-196">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="db924-197">可以將一個以上的處理常式新增至具有 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>之 `HttpClient` 的設定：</span><span class="sxs-lookup"><span data-stu-id="db924-197">More than one handler can be added to the configuration for a `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="db924-198">在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。</span><span class="sxs-lookup"><span data-stu-id="db924-198">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="db924-199">`IHttpClientFactory` 會為每個處理常式建立個別的 DI 範圍。</span><span class="sxs-lookup"><span data-stu-id="db924-199">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="db924-200">處理常式可以相依于任何範圍的服務。</span><span class="sxs-lookup"><span data-stu-id="db924-200">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="db924-201">處置處理常式時，會處置處理常式所相依的服務。</span><span class="sxs-lookup"><span data-stu-id="db924-201">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="db924-202">註冊之後，便可以呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>，並傳入處理常式的類型。</span><span class="sxs-lookup"><span data-stu-id="db924-202">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="db924-203">可以遵循應該執行的順序來註冊多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="db924-203">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="db924-204">每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：</span><span class="sxs-lookup"><span data-stu-id="db924-204">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="db924-205">使用下列其中一種方式來與訊息處理常式共用個別要求狀態：</span><span class="sxs-lookup"><span data-stu-id="db924-205">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="db924-206">使用[HttpRequestMessage](xref:System.Net.Http.HttpRequestMessage.Properties)將資料傳遞至處理常式。</span><span class="sxs-lookup"><span data-stu-id="db924-206">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="db924-207">使用 <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> 來存取目前的要求。</span><span class="sxs-lookup"><span data-stu-id="db924-207">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="db924-208">建立自訂 <xref:System.Threading.AsyncLocal`1> 儲存體物件以傳遞資料。</span><span class="sxs-lookup"><span data-stu-id="db924-208">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="db924-209">使用 Polly 為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="db924-209">Use Polly-based handlers</span></span>

<span data-ttu-id="db924-210">`IHttpClientFactory` 與協力廠商程式庫[Polly](https://github.com/App-vNext/Polly)整合。</span><span class="sxs-lookup"><span data-stu-id="db924-210">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="db924-211">Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。</span><span class="sxs-lookup"><span data-stu-id="db924-211">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="db924-212">它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。</span><span class="sxs-lookup"><span data-stu-id="db924-212">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="db924-213">提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="db924-213">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="db924-214">Polly 擴充功能支援將以 Polly 為基礎的處理常式新增至用戶端。</span><span class="sxs-lookup"><span data-stu-id="db924-214">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="db924-215">Polly 需要[Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="db924-215">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="db924-216">處理暫時性錯誤</span><span class="sxs-lookup"><span data-stu-id="db924-216">Handle transient faults</span></span>

<span data-ttu-id="db924-217">當外部 HTTP 呼叫是暫時性的時，通常會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="db924-217">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="db924-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> 允許定義原則來處理暫時性錯誤。</span><span class="sxs-lookup"><span data-stu-id="db924-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="db924-219">以 `AddTransientHttpErrorPolicy` 設定的原則會處理下列回應：</span><span class="sxs-lookup"><span data-stu-id="db924-219">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="db924-220">HTTP 5xx</span><span class="sxs-lookup"><span data-stu-id="db924-220">HTTP 5xx</span></span>
* <span data-ttu-id="db924-221">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="db924-221">HTTP 408</span></span>

<span data-ttu-id="db924-222">`AddTransientHttpErrorPolicy` 提供 `PolicyBuilder` 物件的存取權，其設定為處理代表可能暫時性錯誤的錯誤：</span><span class="sxs-lookup"><span data-stu-id="db924-222">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="db924-223">在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。</span><span class="sxs-lookup"><span data-stu-id="db924-223">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="db924-224">失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。</span><span class="sxs-lookup"><span data-stu-id="db924-224">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="db924-225">動態選取原則</span><span class="sxs-lookup"><span data-stu-id="db924-225">Dynamically select policies</span></span>

<span data-ttu-id="db924-226">提供擴充方法來加入以 Polly 為基礎的處理常式，例如 <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>。</span><span class="sxs-lookup"><span data-stu-id="db924-226">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="db924-227">下列 `AddPolicyHandler` 多載會檢查要求以決定要套用的原則：</span><span class="sxs-lookup"><span data-stu-id="db924-227">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="db924-228">在上述程式碼中，如果外寄要求是 HTTP GET，就會套用 10 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="db924-228">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="db924-229">任何其他 HTTP 方法會使用 30 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="db924-229">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="db924-230">新增多個 Polly 處理常式</span><span class="sxs-lookup"><span data-stu-id="db924-230">Add multiple Polly handlers</span></span>

<span data-ttu-id="db924-231">通常會將 Polly 原則加以嵌套：</span><span class="sxs-lookup"><span data-stu-id="db924-231">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="db924-232">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="db924-232">In the preceding example:</span></span>

* <span data-ttu-id="db924-233">新增了兩個處理常式。</span><span class="sxs-lookup"><span data-stu-id="db924-233">Two handlers are added.</span></span>
* <span data-ttu-id="db924-234">第一個處理常式會使用 <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> 來新增重試原則。</span><span class="sxs-lookup"><span data-stu-id="db924-234">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="db924-235">失敗的要求會重試最多三次。</span><span class="sxs-lookup"><span data-stu-id="db924-235">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="db924-236">第二個 `AddTransientHttpErrorPolicy` 呼叫會增加斷路器原則。</span><span class="sxs-lookup"><span data-stu-id="db924-236">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="db924-237">如果連續5次嘗試失敗，則會封鎖進一步的外部要求30秒。</span><span class="sxs-lookup"><span data-stu-id="db924-237">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="db924-238">斷路器原則可設定狀態。</span><span class="sxs-lookup"><span data-stu-id="db924-238">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="db924-239">透過此用戶端的所有呼叫都會共用相同的線路狀態。</span><span class="sxs-lookup"><span data-stu-id="db924-239">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="db924-240">從 Polly 登錄新增原則</span><span class="sxs-lookup"><span data-stu-id="db924-240">Add policies from the Polly registry</span></span>

<span data-ttu-id="db924-241">管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。</span><span class="sxs-lookup"><span data-stu-id="db924-241">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="db924-242">在下列程式碼中：</span><span class="sxs-lookup"><span data-stu-id="db924-242">In the following code:</span></span>

* <span data-ttu-id="db924-243">系統會新增「一般」和「長」原則。</span><span class="sxs-lookup"><span data-stu-id="db924-243">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="db924-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*> 從登錄新增「一般」和「長」原則。</span><span class="sxs-lookup"><span data-stu-id="db924-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="db924-245">如需 `IHttpClientFactory` 和 Polly 整合的詳細資訊，請參閱[Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory)。</span><span class="sxs-lookup"><span data-stu-id="db924-245">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="db924-246">HttpClient 和存留期管理</span><span class="sxs-lookup"><span data-stu-id="db924-246">HttpClient and lifetime management</span></span>

<span data-ttu-id="db924-247">每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回新的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="db924-247">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="db924-248">系統會根據每個命名的用戶端來建立 <xref:System.Net.Http.HttpMessageHandler>。</span><span class="sxs-lookup"><span data-stu-id="db924-248">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="db924-249">處理站會管理 `HttpMessageHandler` 執行個體的存留期。</span><span class="sxs-lookup"><span data-stu-id="db924-249">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="db924-250">`IHttpClientFactory` 會將處理站所建立的 `HttpMessageHandler` 執行個體放入集區以減少資源耗用量。</span><span class="sxs-lookup"><span data-stu-id="db924-250">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="db924-251">建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。</span><span class="sxs-lookup"><span data-stu-id="db924-251">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="db924-252">將處理常式放入集區非常實用，因為處理常式通常會管理自己專屬的底層 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="db924-252">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="db924-253">建立比所需數目更多的處理常式，可能會導致連線延遲。</span><span class="sxs-lookup"><span data-stu-id="db924-253">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="db924-254">有些處理常式也會保持連線無限期地開啟，這可能導致處理常式無法回應 DNS （網域名稱系統）變更。</span><span class="sxs-lookup"><span data-stu-id="db924-254">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="db924-255">預設處理常式存留時間為兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="db924-255">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="db924-256">預設值可以根據每個命名的用戶端來覆寫：</span><span class="sxs-lookup"><span data-stu-id="db924-256">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="db924-257">`HttpClient` 實例通常可視為**不**需要處置的 .net 物件。</span><span class="sxs-lookup"><span data-stu-id="db924-257">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="db924-258">處置會取消傳出的要求，並保證指定的 `HttpClient` 執行個體在呼叫 <xref:System.IDisposable.Dispose*> 之後無法使用。</span><span class="sxs-lookup"><span data-stu-id="db924-258">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="db924-259">`IHttpClientFactory` 會追蹤並處置 `HttpClient` 執行個體使用的資源。</span><span class="sxs-lookup"><span data-stu-id="db924-259">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="db924-260">在開始使用 `IHttpClientFactory` 之前，讓單一 `HttpClient` 執行個體維持一段較長的時間，是很常使用的模式。</span><span class="sxs-lookup"><span data-stu-id="db924-260">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="db924-261">在移轉到 `IHttpClientFactory` 之後，就不再需要此模式。</span><span class="sxs-lookup"><span data-stu-id="db924-261">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="db924-262">IHttpClientFactory 的替代方案</span><span class="sxs-lookup"><span data-stu-id="db924-262">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="db924-263">在啟用 DI 的應用程式中使用 `IHttpClientFactory` 可避免：</span><span class="sxs-lookup"><span data-stu-id="db924-263">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="db924-264">藉由共用 `HttpMessageHandler` 實例的資源耗盡問題。</span><span class="sxs-lookup"><span data-stu-id="db924-264">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="db924-265">以固定間隔迴圈 `HttpMessageHandler` 實例的過時 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="db924-265">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="db924-266">有其他方法可以使用長期 <xref:System.Net.Http.SocketsHttpHandler> 實例來解決上述問題。</span><span class="sxs-lookup"><span data-stu-id="db924-266">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="db924-267">當應用程式啟動時，建立 `SocketsHttpHandler` 的實例，並在應用程式的生命週期中使用它。</span><span class="sxs-lookup"><span data-stu-id="db924-267">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="db924-268">根據 DNS 重新整理時間，將 <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> 設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="db924-268">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="db924-269">視需要使用 `new HttpClient(handler, dispostHandler: false)` 建立 `HttpClient` 實例。</span><span class="sxs-lookup"><span data-stu-id="db924-269">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="db924-270">上述方法可解決 `IHttpClientFactory` 以類似的方式解決的資源管理問題。</span><span class="sxs-lookup"><span data-stu-id="db924-270">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="db924-271">`SocketsHttpHandler` 會共用 `HttpClient` 實例間的連接。</span><span class="sxs-lookup"><span data-stu-id="db924-271">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="db924-272">此共用可防止通訊端耗盡。</span><span class="sxs-lookup"><span data-stu-id="db924-272">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="db924-273">`SocketsHttpHandler` 會根據 `PooledConnectionLifetime` 迴圈連接，以避免過時的 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="db924-273">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="db924-274">Cookies</span><span class="sxs-lookup"><span data-stu-id="db924-274">Cookies</span></span>

<span data-ttu-id="db924-275">集區 `HttpMessageHandler` 實例會導致共用 `CookieContainer` 物件。</span><span class="sxs-lookup"><span data-stu-id="db924-275">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="db924-276">意外的 `CookieContainer` 物件共用通常會導致不正確的程式碼。</span><span class="sxs-lookup"><span data-stu-id="db924-276">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="db924-277">針對需要 cookie 的應用程式，請考慮下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="db924-277">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="db924-278">停用自動 cookie 處理</span><span class="sxs-lookup"><span data-stu-id="db924-278">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="db924-279">避免 `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="db924-279">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="db924-280">呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 以停用自動的 cookie 處理：</span><span class="sxs-lookup"><span data-stu-id="db924-280">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="db924-281">記錄</span><span class="sxs-lookup"><span data-stu-id="db924-281">Logging</span></span>

<span data-ttu-id="db924-282">透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="db924-282">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="db924-283">在記錄設定中啟用適當的資訊層級，以查看預設記錄檔訊息。</span><span class="sxs-lookup"><span data-stu-id="db924-283">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="db924-284">額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。</span><span class="sxs-lookup"><span data-stu-id="db924-284">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="db924-285">用於每個用戶端的記錄檔分類包含用戶端的名稱。</span><span class="sxs-lookup"><span data-stu-id="db924-285">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="db924-286">例如，名為*MyNamedClient*的用戶端會記錄類別為 "HttpClient" 的訊息。**MyNamedClient**。LogicalHandler "。</span><span class="sxs-lookup"><span data-stu-id="db924-286">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="db924-287">後面加上 *LogicalHandler* 的訊息發生在要求處理常式管線之外。</span><span class="sxs-lookup"><span data-stu-id="db924-287">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="db924-288">在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。</span><span class="sxs-lookup"><span data-stu-id="db924-288">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="db924-289">在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。</span><span class="sxs-lookup"><span data-stu-id="db924-289">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="db924-290">記錄也會發生在要求處理常式管線之內。</span><span class="sxs-lookup"><span data-stu-id="db924-290">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="db924-291">在*MyNamedClient*範例中，這些訊息會記錄在記錄檔類別中為 "HttpClient"。**MyNamedClient**。ClientHandler".</span><span class="sxs-lookup"><span data-stu-id="db924-291">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="db924-292">對於要求，這會在所有其他處理常式都執行之後，且在傳送要求之前立即發生。</span><span class="sxs-lookup"><span data-stu-id="db924-292">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="db924-293">在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。</span><span class="sxs-lookup"><span data-stu-id="db924-293">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="db924-294">在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。</span><span class="sxs-lookup"><span data-stu-id="db924-294">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="db924-295">這可能包括要求標頭的變更或回應狀態碼。</span><span class="sxs-lookup"><span data-stu-id="db924-295">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="db924-296">在記錄類別中包含用戶端的名稱，可以針對特定的已命名用戶端進行記錄篩選。</span><span class="sxs-lookup"><span data-stu-id="db924-296">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="db924-297">設定 HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="db924-297">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="db924-298">可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。</span><span class="sxs-lookup"><span data-stu-id="db924-298">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="db924-299">新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。</span><span class="sxs-lookup"><span data-stu-id="db924-299">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="db924-300"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 擴充方法可以用來定義委派。</span><span class="sxs-lookup"><span data-stu-id="db924-300">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="db924-301">委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：</span><span class="sxs-lookup"><span data-stu-id="db924-301">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="db924-302">在主控台應用程式中使用 IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="db924-302">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="db924-303">在主控台應用程式中，將下列套件參考新增至專案：</span><span class="sxs-lookup"><span data-stu-id="db924-303">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="db924-304">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="db924-304">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="db924-305">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="db924-305">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="db924-306">在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="db924-306">In the following example:</span></span>

* <span data-ttu-id="db924-307"><xref:System.Net.Http.IHttpClientFactory> 已在[泛型主機的](xref:fundamentals/host/generic-host)服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="db924-307"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="db924-308">`MyService` 會從服務建立用戶端 Factory 執行個體，其可用來建立 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="db924-308">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="db924-309">`HttpClient` 會用來擷取網頁。</span><span class="sxs-lookup"><span data-stu-id="db924-309">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="db924-310">`Main` 會建立範圍來執行服務的 `GetPage` 方法，並將網頁內容的前 500 個字元寫入至主控台。</span><span class="sxs-lookup"><span data-stu-id="db924-310">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="db924-311">其他資源</span><span class="sxs-lookup"><span data-stu-id="db924-311">Additional resources</span></span>

* [<span data-ttu-id="db924-312">使用 HttpClientFactory 實作復原 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="db924-312">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="db924-313">使用 HttpClientFactory 和 Polly 原則以指數輪詢實作 HTTP 呼叫重試</span><span class="sxs-lookup"><span data-stu-id="db924-313">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="db924-314">實作斷路器模式</span><span class="sxs-lookup"><span data-stu-id="db924-314">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="db924-315">作者：[Glenn Condron](https://github.com/glennc)、[Ryan Nowak](https://github.com/rynowak) 和 [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="db924-315">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="db924-316"><xref:System.Net.Http.IHttpClientFactory> 可以註冊及用來在應用程式中設定和建立 <xref:System.Net.Http.HttpClient> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="db924-316">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="db924-317">它提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="db924-317">It offers the following benefits:</span></span>

* <span data-ttu-id="db924-318">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="db924-318">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="db924-319">例如，可註冊及設定 *github* 用戶端，來存取 [GitHub](https://github.com/)。</span><span class="sxs-lookup"><span data-stu-id="db924-319">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="db924-320">預設用戶端可以註冊用於其他用途。</span><span class="sxs-lookup"><span data-stu-id="db924-320">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="db924-321">透過委派 `HttpClient` 中的處理常式來撰寫外寄中介軟體的概念，並提供延伸模組以便 Polly 架構中介軟體利用外寄中介軟體。</span><span class="sxs-lookup"><span data-stu-id="db924-321">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="db924-322">管理基礎 `HttpClientMessageHandler` 執行個體的共用和存留期，以避免在手動管理 `HttpClient` 存留期時，發生的常見 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="db924-322">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="db924-323">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="db924-323">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="db924-324">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="db924-324">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="db924-325">耗用模式</span><span class="sxs-lookup"><span data-stu-id="db924-325">Consumption patterns</span></span>

<span data-ttu-id="db924-326">有數種方式可將 `IHttpClientFactory` 用於應用程式：</span><span class="sxs-lookup"><span data-stu-id="db924-326">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="db924-327">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="db924-327">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="db924-328">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="db924-328">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="db924-329">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="db924-329">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="db924-330">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="db924-330">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="db924-331">它們全都不會嚴格優先於另一個。</span><span class="sxs-lookup"><span data-stu-id="db924-331">None of them are strictly superior to another.</span></span> <span data-ttu-id="db924-332">最好的方法取決於應用程式的條件約束。</span><span class="sxs-lookup"><span data-stu-id="db924-332">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="db924-333">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="db924-333">Basic usage</span></span>

<span data-ttu-id="db924-334">`IHttpClientFactory` 可以藉由在 `Startup.ConfigureServices` 方法內的 `IServiceCollection` 上呼叫 `AddHttpClient` 擴充方法來註冊。</span><span class="sxs-lookup"><span data-stu-id="db924-334">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="db924-335">註冊之後，程式碼可以在可使用[相依性插入 (DI)](xref:fundamentals/dependency-injection) 插入服務的任何位置，接受 `IHttpClientFactory`。</span><span class="sxs-lookup"><span data-stu-id="db924-335">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="db924-336">`IHttpClientFactory` 可以用來建立 `HttpClient` 執行個體：</span><span class="sxs-lookup"><span data-stu-id="db924-336">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="db924-337">以這種方式使用 `IHttpClientFactory` 是重構現有應用程式的好方法。</span><span class="sxs-lookup"><span data-stu-id="db924-337">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="db924-338">它對 `HttpClient` 的使用方式沒有任何影響。</span><span class="sxs-lookup"><span data-stu-id="db924-338">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="db924-339">在目前建立 `HttpClient` 執行個體的位置，將那些項目取代為呼叫 <xref:System.Net.Http.IHttpClientFactory.CreateClient*>。</span><span class="sxs-lookup"><span data-stu-id="db924-339">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="db924-340">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="db924-340">Named clients</span></span>

<span data-ttu-id="db924-341">如果應用程式需要使用多個不同的 `HttpClient`，且每個都有不同的設定，可以選擇使用**具名用戶端**。</span><span class="sxs-lookup"><span data-stu-id="db924-341">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="db924-342">具名 `HttpClient` 的組態可以在 `Startup.ConfigureServices` 中註冊時指定。</span><span class="sxs-lookup"><span data-stu-id="db924-342">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="db924-343">在上述程式碼中，會呼叫 `AddHttpClient` 並提供名稱 *github*。</span><span class="sxs-lookup"><span data-stu-id="db924-343">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="db924-344">此用戶端已套用一些預設組態&mdash;即使用 GitHub API 所需的基底位址和兩個標頭。</span><span class="sxs-lookup"><span data-stu-id="db924-344">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="db924-345">每次呼叫 `CreateClient` 時，會建立 `HttpClient` 的新執行個體並呼叫組態動作。</span><span class="sxs-lookup"><span data-stu-id="db924-345">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="db924-346">若要使用具名用戶端，可以傳遞字串參數至 `CreateClient`。</span><span class="sxs-lookup"><span data-stu-id="db924-346">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="db924-347">指定要建立之用戶端的名稱：</span><span class="sxs-lookup"><span data-stu-id="db924-347">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="db924-348">在上述程式碼中，要求不需要指定主機名稱。</span><span class="sxs-lookup"><span data-stu-id="db924-348">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="db924-349">它可以只傳遞路徑，因為已使用為用戶端設定的基底位址。</span><span class="sxs-lookup"><span data-stu-id="db924-349">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="db924-350">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="db924-350">Typed clients</span></span>

<span data-ttu-id="db924-351">具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="db924-351">Typed clients:</span></span>

* <span data-ttu-id="db924-352">提供與具名用戶端相同的功能，而不需使用字串作為索引鍵。</span><span class="sxs-lookup"><span data-stu-id="db924-352">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="db924-353">取用用戶端時提供 IntelliSense 和編譯器說明。</span><span class="sxs-lookup"><span data-stu-id="db924-353">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="db924-354">提供單一位置來設定特定的 `HttpClient` 並與其互動。</span><span class="sxs-lookup"><span data-stu-id="db924-354">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="db924-355">例如，單一的具型別用戶端可能用於單一的後端端點，並封裝處理該端點的所有邏輯。</span><span class="sxs-lookup"><span data-stu-id="db924-355">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="db924-356">使用 DI 且可在應用程式中需要之處插入。</span><span class="sxs-lookup"><span data-stu-id="db924-356">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="db924-357">具型別用戶端在其建構函式中接受 `HttpClient` 參數：</span><span class="sxs-lookup"><span data-stu-id="db924-357">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="db924-358">在上述程式碼中，組態會移到具型別用戶端。</span><span class="sxs-lookup"><span data-stu-id="db924-358">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="db924-359">`HttpClient` 物件會公開為公用屬性。</span><span class="sxs-lookup"><span data-stu-id="db924-359">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="db924-360">您可定義 API 特定的方法，其公開 `HttpClient` 功能。</span><span class="sxs-lookup"><span data-stu-id="db924-360">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="db924-361">`GetAspNetDocsIssues` 方法會封裝從 GitHub 存放庫查詢和剖析最新開啟問題所需的程式碼。</span><span class="sxs-lookup"><span data-stu-id="db924-361">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="db924-362">若要註冊具型別用戶端，泛型 <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> 擴充方法可用於 `Startup.ConfigureServices` 內，並指定具型別用戶端類別：</span><span class="sxs-lookup"><span data-stu-id="db924-362">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="db924-363">具型別用戶端會向 DI 註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="db924-363">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="db924-364">具型別用戶端可以直接插入並使用：</span><span class="sxs-lookup"><span data-stu-id="db924-364">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="db924-365">想要的話，具型別用戶端的組態可以在 `Startup.ConfigureServices` 中註冊時指定，而不是在具型別用戶端的建構函式中：</span><span class="sxs-lookup"><span data-stu-id="db924-365">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="db924-366">可以將 `HttpClient` 完全封裝在具型別用戶端內。</span><span class="sxs-lookup"><span data-stu-id="db924-366">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="db924-367">可以提供在內部呼叫 `HttpClient` 執行個體的公用方法，而不將它公開為屬性。</span><span class="sxs-lookup"><span data-stu-id="db924-367">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="db924-368">在上述程式碼，`HttpClient` 儲存為私用欄位。</span><span class="sxs-lookup"><span data-stu-id="db924-368">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="db924-369">進行外部呼叫的所有存取都會經歷 `GetRepos` 方法。</span><span class="sxs-lookup"><span data-stu-id="db924-369">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="db924-370">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="db924-370">Generated clients</span></span>

<span data-ttu-id="db924-371">`IHttpClientFactory` 可和其他協力廠商程式庫一起使用，例如 [Refit](https://github.com/paulcbetts/refit)。</span><span class="sxs-lookup"><span data-stu-id="db924-371">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="db924-372">Refit 是適用於 .NET 的 REST 程式庫。</span><span class="sxs-lookup"><span data-stu-id="db924-372">Refit is a REST library for .NET.</span></span> <span data-ttu-id="db924-373">它將 REST API 轉換為即時介面。</span><span class="sxs-lookup"><span data-stu-id="db924-373">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="db924-374">介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。</span><span class="sxs-lookup"><span data-stu-id="db924-374">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="db924-375">定義介面及回覆來代表外部 API 和其回應：</span><span class="sxs-lookup"><span data-stu-id="db924-375">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="db924-376">可以新增具型別用戶端，使用 Refit 產生實作：</span><span class="sxs-lookup"><span data-stu-id="db924-376">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("https://localhost:5001");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

<span data-ttu-id="db924-377">定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：</span><span class="sxs-lookup"><span data-stu-id="db924-377">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="db924-378">外寄要求中介軟體</span><span class="sxs-lookup"><span data-stu-id="db924-378">Outgoing request middleware</span></span>

<span data-ttu-id="db924-379">`HttpClient` 已經有委派可針對外寄 HTTP 要求連結在一起的處理常式的概念。</span><span class="sxs-lookup"><span data-stu-id="db924-379">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="db924-380">`IHttpClientFactory` 可讓您輕鬆地定義要套用於每個具名用戶端的處理常式。</span><span class="sxs-lookup"><span data-stu-id="db924-380">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="db924-381">它支援註冊和鏈結多個處理常式，以建置外寄要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="db924-381">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="db924-382">這些處理常式每個都可以在外寄要求之前和之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="db924-382">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="db924-383">此模式與 ASP.NET Core 中的輸入中介軟體管線相似。</span><span class="sxs-lookup"><span data-stu-id="db924-383">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="db924-384">模式提供一個機制來管理 HTTP 要求的跨領域關注，包括快取、錯誤處理、序列化和記錄。</span><span class="sxs-lookup"><span data-stu-id="db924-384">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="db924-385">若要建立處理常式，請定義衍生自 <xref:System.Net.Http.DelegatingHandler> 的類別。</span><span class="sxs-lookup"><span data-stu-id="db924-385">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="db924-386">覆寫 `SendAsync` 方法，以在將要求傳遞至管線中的下一個處理常式之前執行程式碼：</span><span class="sxs-lookup"><span data-stu-id="db924-386">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="db924-387">上述程式碼定義一個基本處理常式。</span><span class="sxs-lookup"><span data-stu-id="db924-387">The preceding code defines a basic handler.</span></span> <span data-ttu-id="db924-388">它會檢查以查看要求上是否已包含 `X-API-KEY` 標頭。</span><span class="sxs-lookup"><span data-stu-id="db924-388">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="db924-389">如果遺漏標頭，它可以避免 HTTP 呼叫，並傳回適當的回應。</span><span class="sxs-lookup"><span data-stu-id="db924-389">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="db924-390">在註冊期間，可以新增一或多個處理常式至 `HttpClient` 的組態。</span><span class="sxs-lookup"><span data-stu-id="db924-390">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="db924-391">這項工作是透過 <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder> 上的擴充方法完成。</span><span class="sxs-lookup"><span data-stu-id="db924-391">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="db924-392">在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。</span><span class="sxs-lookup"><span data-stu-id="db924-392">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="db924-393">`IHttpClientFactory` 會為每個處理常式建立個別的 DI 範圍。</span><span class="sxs-lookup"><span data-stu-id="db924-393">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="db924-394">處理常式可相依於任何範圍的服務。</span><span class="sxs-lookup"><span data-stu-id="db924-394">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="db924-395">處置處理常式時，會處置處理常式所相依的服務。</span><span class="sxs-lookup"><span data-stu-id="db924-395">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="db924-396">註冊之後，便可以呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>，並傳入處理常式的類型。</span><span class="sxs-lookup"><span data-stu-id="db924-396">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="db924-397">可以遵循應該執行的順序來註冊多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="db924-397">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="db924-398">每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：</span><span class="sxs-lookup"><span data-stu-id="db924-398">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="db924-399">使用下列其中一種方式來與訊息處理常式共用個別要求狀態：</span><span class="sxs-lookup"><span data-stu-id="db924-399">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="db924-400">使用 `HttpRequestMessage.Properties` 將資料傳遞到處理常式。</span><span class="sxs-lookup"><span data-stu-id="db924-400">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="db924-401">使用 `IHttpContextAccessor` 來存取目前的要求。</span><span class="sxs-lookup"><span data-stu-id="db924-401">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="db924-402">建立自訂 `AsyncLocal` 儲存體物件以傳遞資料。</span><span class="sxs-lookup"><span data-stu-id="db924-402">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="db924-403">使用 Polly 為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="db924-403">Use Polly-based handlers</span></span>

<span data-ttu-id="db924-404">`IHttpClientFactory` 整合受歡迎的協力廠商程式庫，稱為 [Polly](https://github.com/App-vNext/Polly)。</span><span class="sxs-lookup"><span data-stu-id="db924-404">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="db924-405">Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。</span><span class="sxs-lookup"><span data-stu-id="db924-405">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="db924-406">它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。</span><span class="sxs-lookup"><span data-stu-id="db924-406">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="db924-407">提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="db924-407">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="db924-408">Polly 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="db924-408">The Polly extensions:</span></span>

* <span data-ttu-id="db924-409">支援將以 Polly 為基礎的處理常式新增至用戶端。</span><span class="sxs-lookup"><span data-stu-id="db924-409">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="db924-410">可在安裝 [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet 套件後使用。</span><span class="sxs-lookup"><span data-stu-id="db924-410">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="db924-411">該套件並未包含在 ASP.NET Core 共用架構中。</span><span class="sxs-lookup"><span data-stu-id="db924-411">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="db924-412">處理暫時性錯誤</span><span class="sxs-lookup"><span data-stu-id="db924-412">Handle transient faults</span></span>

<span data-ttu-id="db924-413">大部分的錯誤發生在外部 HTTP 呼叫是暫時性的時候。</span><span class="sxs-lookup"><span data-stu-id="db924-413">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="db924-414">包含一個便利的擴充方法，稱為 `AddTransientHttpErrorPolicy`，它可允許定義原則來處理暫時性錯誤。</span><span class="sxs-lookup"><span data-stu-id="db924-414">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="db924-415">使用此延伸模組方法設定的原則，會處理 `HttpRequestException`、HTTP 5xx 回應和 HTTP 408 回應。</span><span class="sxs-lookup"><span data-stu-id="db924-415">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="db924-416">`AddTransientHttpErrorPolicy` 延伸模組可用於 `Startup.ConfigureServices` 內。</span><span class="sxs-lookup"><span data-stu-id="db924-416">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="db924-417">延伸模組能提供 `PolicyBuilder` 物件的存取，該物件已設定來處理代表可能暫時性錯誤的錯誤：</span><span class="sxs-lookup"><span data-stu-id="db924-417">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="db924-418">在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。</span><span class="sxs-lookup"><span data-stu-id="db924-418">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="db924-419">失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。</span><span class="sxs-lookup"><span data-stu-id="db924-419">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="db924-420">動態選取原則</span><span class="sxs-lookup"><span data-stu-id="db924-420">Dynamically select policies</span></span>

<span data-ttu-id="db924-421">有額外的擴充方法可用來新增 Polly 為基礎的處理常式。</span><span class="sxs-lookup"><span data-stu-id="db924-421">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="db924-422">其中一個這類延伸模組是 `AddPolicyHandler`，它有多個多載。</span><span class="sxs-lookup"><span data-stu-id="db924-422">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="db924-423">一個多載可讓您在定義要套用的原則時，檢查要求：</span><span class="sxs-lookup"><span data-stu-id="db924-423">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="db924-424">在上述程式碼中，如果外寄要求是 HTTP GET，就會套用 10 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="db924-424">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="db924-425">任何其他 HTTP 方法會使用 30 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="db924-425">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="db924-426">新增多個 Polly 處理常式</span><span class="sxs-lookup"><span data-stu-id="db924-426">Add multiple Polly handlers</span></span>

<span data-ttu-id="db924-427">通常會建立巢狀 Polly 原則，以提供增強的功能：</span><span class="sxs-lookup"><span data-stu-id="db924-427">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="db924-428">在上述範例中，會新增兩個處理常式。</span><span class="sxs-lookup"><span data-stu-id="db924-428">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="db924-429">第一個使用 `AddTransientHttpErrorPolicy` 延伸模組來新增重試原則。</span><span class="sxs-lookup"><span data-stu-id="db924-429">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="db924-430">失敗的要求會重試最多三次。</span><span class="sxs-lookup"><span data-stu-id="db924-430">Failed requests are retried up to three times.</span></span> <span data-ttu-id="db924-431">第二個 `AddTransientHttpErrorPolicy` 呼叫會新增斷路器原則。</span><span class="sxs-lookup"><span data-stu-id="db924-431">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="db924-432">如果循序發生五次失敗的嘗試，進一步的外部要求會遭到封鎖 30 秒。</span><span class="sxs-lookup"><span data-stu-id="db924-432">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="db924-433">斷路器原則可設定狀態。</span><span class="sxs-lookup"><span data-stu-id="db924-433">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="db924-434">透過此用戶端的所有呼叫都會共用相同的線路狀態。</span><span class="sxs-lookup"><span data-stu-id="db924-434">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="db924-435">從 Polly 登錄新增原則</span><span class="sxs-lookup"><span data-stu-id="db924-435">Add policies from the Polly registry</span></span>

<span data-ttu-id="db924-436">管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。</span><span class="sxs-lookup"><span data-stu-id="db924-436">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="db924-437">提供了擴充方法，可以使用來自登錄的原則新增處理常式：</span><span class="sxs-lookup"><span data-stu-id="db924-437">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="db924-438">在上述程式碼中，當 `PolicyRegistry` 新增至 `ServiceCollection` 時，註冊了兩個原則。</span><span class="sxs-lookup"><span data-stu-id="db924-438">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="db924-439">為了使用來自登錄的原則，使用了 `AddPolicyHandlerFromRegistry` 方法，並傳遞要套用的原則名稱。</span><span class="sxs-lookup"><span data-stu-id="db924-439">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="db924-440">關於 `IHttpClientFactory` Polly 整合的詳細資訊，可以在 [Polly Wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory) 上找到。</span><span class="sxs-lookup"><span data-stu-id="db924-440">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="db924-441">HttpClient 和存留期管理</span><span class="sxs-lookup"><span data-stu-id="db924-441">HttpClient and lifetime management</span></span>

<span data-ttu-id="db924-442">每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回新的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="db924-442">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="db924-443">每個具名用戶端都有一個 <xref:System.Net.Http.HttpMessageHandler>。</span><span class="sxs-lookup"><span data-stu-id="db924-443">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="db924-444">處理站會管理 `HttpMessageHandler` 執行個體的存留期。</span><span class="sxs-lookup"><span data-stu-id="db924-444">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="db924-445">`IHttpClientFactory` 會將處理站所建立的 `HttpMessageHandler` 執行個體放入集區以減少資源耗用量。</span><span class="sxs-lookup"><span data-stu-id="db924-445">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="db924-446">建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。</span><span class="sxs-lookup"><span data-stu-id="db924-446">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="db924-447">將處理常式放入集區非常實用，因為處理常式通常會管理自己專屬的底層 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="db924-447">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="db924-448">建立比所需數目更多的處理常式，可能會導致連線延遲。</span><span class="sxs-lookup"><span data-stu-id="db924-448">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="db924-449">有些處理常式也會保持連線無限期地開啟，這可能導致處理常式無法對 DNS 變更回應。</span><span class="sxs-lookup"><span data-stu-id="db924-449">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="db924-450">預設處理常式存留時間為兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="db924-450">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="db924-451">可以針對每個具名用戶端覆寫預設值。</span><span class="sxs-lookup"><span data-stu-id="db924-451">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="db924-452">若要覆寫它，請在建立用戶端時所傳回的 `IHttpClientBuilder` 上呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*>：</span><span class="sxs-lookup"><span data-stu-id="db924-452">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="db924-453">不需要處置用戶端。</span><span class="sxs-lookup"><span data-stu-id="db924-453">Disposal of the client isn't required.</span></span> <span data-ttu-id="db924-454">處置會取消傳出的要求，並保證指定的 `HttpClient` 執行個體在呼叫 <xref:System.IDisposable.Dispose*> 之後無法使用。</span><span class="sxs-lookup"><span data-stu-id="db924-454">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="db924-455">`IHttpClientFactory` 會追蹤並處置 `HttpClient` 執行個體使用的資源。</span><span class="sxs-lookup"><span data-stu-id="db924-455">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="db924-456">`HttpClient` 執行個體通常可視為 .NET 物件，不需要處置。</span><span class="sxs-lookup"><span data-stu-id="db924-456">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="db924-457">在開始使用 `IHttpClientFactory` 之前，讓單一 `HttpClient` 執行個體維持一段較長的時間，是很常使用的模式。</span><span class="sxs-lookup"><span data-stu-id="db924-457">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="db924-458">在移轉到 `IHttpClientFactory` 之後，就不再需要此模式。</span><span class="sxs-lookup"><span data-stu-id="db924-458">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="db924-459">IHttpClientFactory 的替代方案</span><span class="sxs-lookup"><span data-stu-id="db924-459">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="db924-460">在啟用 DI 的應用程式中使用 `IHttpClientFactory` 可避免：</span><span class="sxs-lookup"><span data-stu-id="db924-460">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="db924-461">藉由共用 `HttpMessageHandler` 實例的資源耗盡問題。</span><span class="sxs-lookup"><span data-stu-id="db924-461">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="db924-462">以固定間隔迴圈 `HttpMessageHandler` 實例的過時 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="db924-462">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="db924-463">有其他方法可以使用長期 <xref:System.Net.Http.SocketsHttpHandler> 實例來解決上述問題。</span><span class="sxs-lookup"><span data-stu-id="db924-463">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="db924-464">當應用程式啟動時，建立 `SocketsHttpHandler` 的實例，並在應用程式的生命週期中使用它。</span><span class="sxs-lookup"><span data-stu-id="db924-464">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="db924-465">根據 DNS 重新整理時間，將 <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> 設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="db924-465">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="db924-466">視需要使用 `new HttpClient(handler, dispostHandler: false)` 建立 `HttpClient` 實例。</span><span class="sxs-lookup"><span data-stu-id="db924-466">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="db924-467">上述方法可解決 `IHttpClientFactory` 以類似的方式解決的資源管理問題。</span><span class="sxs-lookup"><span data-stu-id="db924-467">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="db924-468">`SocketsHttpHandler` 會共用 `HttpClient` 實例間的連接。</span><span class="sxs-lookup"><span data-stu-id="db924-468">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="db924-469">此共用可防止通訊端耗盡。</span><span class="sxs-lookup"><span data-stu-id="db924-469">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="db924-470">`SocketsHttpHandler` 會根據 `PooledConnectionLifetime` 迴圈連接，以避免過時的 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="db924-470">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="db924-471">Cookies</span><span class="sxs-lookup"><span data-stu-id="db924-471">Cookies</span></span>

<span data-ttu-id="db924-472">集區 `HttpMessageHandler` 實例會導致共用 `CookieContainer` 物件。</span><span class="sxs-lookup"><span data-stu-id="db924-472">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="db924-473">意外的 `CookieContainer` 物件共用通常會導致不正確的程式碼。</span><span class="sxs-lookup"><span data-stu-id="db924-473">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="db924-474">針對需要 cookie 的應用程式，請考慮下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="db924-474">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="db924-475">停用自動 cookie 處理</span><span class="sxs-lookup"><span data-stu-id="db924-475">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="db924-476">避免 `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="db924-476">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="db924-477">呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 以停用自動的 cookie 處理：</span><span class="sxs-lookup"><span data-stu-id="db924-477">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="db924-478">記錄</span><span class="sxs-lookup"><span data-stu-id="db924-478">Logging</span></span>

<span data-ttu-id="db924-479">透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="db924-479">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="db924-480">在記錄設定中啟用適當的資訊層級，以查看預設記錄檔訊息。</span><span class="sxs-lookup"><span data-stu-id="db924-480">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="db924-481">額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。</span><span class="sxs-lookup"><span data-stu-id="db924-481">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="db924-482">用於每個用戶端的記錄檔分類包含用戶端的名稱。</span><span class="sxs-lookup"><span data-stu-id="db924-482">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="db924-483">例如，名為 *MyNamedClient* 的用戶端會記錄分類為 `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler` 的訊息。</span><span class="sxs-lookup"><span data-stu-id="db924-483">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="db924-484">後面加上 *LogicalHandler* 的訊息發生在要求處理常式管線之外。</span><span class="sxs-lookup"><span data-stu-id="db924-484">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="db924-485">在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。</span><span class="sxs-lookup"><span data-stu-id="db924-485">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="db924-486">在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。</span><span class="sxs-lookup"><span data-stu-id="db924-486">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="db924-487">記錄也會發生在要求處理常式管線之內。</span><span class="sxs-lookup"><span data-stu-id="db924-487">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="db924-488">在 *MyNamedClient* 範例中，那些訊息是針對記錄檔分類 `System.Net.Http.HttpClient.MyNamedClient.ClientHandler` 而記錄。</span><span class="sxs-lookup"><span data-stu-id="db924-488">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="db924-489">對於要求，這是發生在所有其他處理常式都已執行之後，並且緊接在網路上傳送要求之前。</span><span class="sxs-lookup"><span data-stu-id="db924-489">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="db924-490">在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。</span><span class="sxs-lookup"><span data-stu-id="db924-490">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="db924-491">在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。</span><span class="sxs-lookup"><span data-stu-id="db924-491">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="db924-492">例如，這可能包括要求標頭的變更，或是回應狀態碼的變更。</span><span class="sxs-lookup"><span data-stu-id="db924-492">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="db924-493">在記錄分類中包含用戶端的名稱，可讓您在需要時進行特定具名用戶端的記錄檔篩選。</span><span class="sxs-lookup"><span data-stu-id="db924-493">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="db924-494">設定 HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="db924-494">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="db924-495">可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。</span><span class="sxs-lookup"><span data-stu-id="db924-495">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="db924-496">新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。</span><span class="sxs-lookup"><span data-stu-id="db924-496">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="db924-497"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 擴充方法可以用來定義委派。</span><span class="sxs-lookup"><span data-stu-id="db924-497">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="db924-498">委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：</span><span class="sxs-lookup"><span data-stu-id="db924-498">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="db924-499">在主控台應用程式中使用 IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="db924-499">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="db924-500">在主控台應用程式中，將下列套件參考新增至專案：</span><span class="sxs-lookup"><span data-stu-id="db924-500">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="db924-501">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="db924-501">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="db924-502">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="db924-502">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="db924-503">在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="db924-503">In the following example:</span></span>

* <span data-ttu-id="db924-504"><xref:System.Net.Http.IHttpClientFactory> 已在[泛型主機的](xref:fundamentals/host/generic-host)服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="db924-504"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="db924-505">`MyService` 會從服務建立用戶端 Factory 執行個體，其可用來建立 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="db924-505">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="db924-506">`HttpClient` 會用來擷取網頁。</span><span class="sxs-lookup"><span data-stu-id="db924-506">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="db924-507">`Main` 會建立範圍來執行服務的 `GetPage` 方法，並將網頁內容的前 500 個字元寫入至主控台。</span><span class="sxs-lookup"><span data-stu-id="db924-507">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="db924-508">其他資源</span><span class="sxs-lookup"><span data-stu-id="db924-508">Additional resources</span></span>

* [<span data-ttu-id="db924-509">使用 HttpClientFactory 實作復原 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="db924-509">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="db924-510">使用 HttpClientFactory 和 Polly 原則以指數輪詢實作 HTTP 呼叫重試</span><span class="sxs-lookup"><span data-stu-id="db924-510">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="db924-511">實作斷路器模式</span><span class="sxs-lookup"><span data-stu-id="db924-511">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

<span data-ttu-id="db924-512">作者：[Glenn Condron](https://github.com/glennc)、[Ryan Nowak](https://github.com/rynowak) 和 [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="db924-512">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="db924-513"><xref:System.Net.Http.IHttpClientFactory> 可以註冊及用來在應用程式中設定和建立 <xref:System.Net.Http.HttpClient> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="db924-513">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="db924-514">它提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="db924-514">It offers the following benefits:</span></span>

* <span data-ttu-id="db924-515">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="db924-515">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="db924-516">例如，可註冊及設定 *github* 用戶端，來存取 [GitHub](https://github.com/)。</span><span class="sxs-lookup"><span data-stu-id="db924-516">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="db924-517">預設用戶端可以註冊用於其他用途。</span><span class="sxs-lookup"><span data-stu-id="db924-517">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="db924-518">透過委派 `HttpClient` 中的處理常式來撰寫外寄中介軟體的概念，並提供延伸模組以便 Polly 架構中介軟體利用外寄中介軟體。</span><span class="sxs-lookup"><span data-stu-id="db924-518">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="db924-519">管理基礎 `HttpClientMessageHandler` 執行個體的共用和存留期，以避免在手動管理 `HttpClient` 存留期時，發生的常見 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="db924-519">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="db924-520">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="db924-520">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="db924-521">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="db924-521">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="db924-522">必要條件：</span><span class="sxs-lookup"><span data-stu-id="db924-522">Prerequisites</span></span>

<span data-ttu-id="db924-523">以 .NET Framework 為目標的專案，需要安裝 [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="db924-523">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="db924-524">以 .NET Core 為目標且參考 [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) 的專案，已包含 `Microsoft.Extensions.Http` 套件。</span><span class="sxs-lookup"><span data-stu-id="db924-524">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="db924-525">耗用模式</span><span class="sxs-lookup"><span data-stu-id="db924-525">Consumption patterns</span></span>

<span data-ttu-id="db924-526">有數種方式可將 `IHttpClientFactory` 用於應用程式：</span><span class="sxs-lookup"><span data-stu-id="db924-526">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="db924-527">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="db924-527">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="db924-528">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="db924-528">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="db924-529">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="db924-529">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="db924-530">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="db924-530">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="db924-531">它們全都不會嚴格優先於另一個。</span><span class="sxs-lookup"><span data-stu-id="db924-531">None of them are strictly superior to another.</span></span> <span data-ttu-id="db924-532">最好的方法取決於應用程式的條件約束。</span><span class="sxs-lookup"><span data-stu-id="db924-532">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="db924-533">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="db924-533">Basic usage</span></span>

<span data-ttu-id="db924-534">`IHttpClientFactory` 可以藉由在 `Startup.ConfigureServices` 方法內的 `IServiceCollection` 上呼叫 `AddHttpClient` 擴充方法來註冊。</span><span class="sxs-lookup"><span data-stu-id="db924-534">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="db924-535">註冊之後，程式碼可以在可使用[相依性插入 (DI)](xref:fundamentals/dependency-injection) 插入服務的任何位置，接受 `IHttpClientFactory`。</span><span class="sxs-lookup"><span data-stu-id="db924-535">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="db924-536">`IHttpClientFactory` 可以用來建立 `HttpClient` 執行個體：</span><span class="sxs-lookup"><span data-stu-id="db924-536">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="db924-537">以這種方式使用 `IHttpClientFactory` 是重構現有應用程式的好方法。</span><span class="sxs-lookup"><span data-stu-id="db924-537">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="db924-538">它對 `HttpClient` 的使用方式沒有任何影響。</span><span class="sxs-lookup"><span data-stu-id="db924-538">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="db924-539">在目前建立 `HttpClient` 執行個體的位置，將那些項目取代為呼叫 <xref:System.Net.Http.IHttpClientFactory.CreateClient*>。</span><span class="sxs-lookup"><span data-stu-id="db924-539">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="db924-540">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="db924-540">Named clients</span></span>

<span data-ttu-id="db924-541">如果應用程式需要使用多個不同的 `HttpClient`，且每個都有不同的設定，可以選擇使用**具名用戶端**。</span><span class="sxs-lookup"><span data-stu-id="db924-541">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="db924-542">具名 `HttpClient` 的組態可以在 `Startup.ConfigureServices` 中註冊時指定。</span><span class="sxs-lookup"><span data-stu-id="db924-542">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="db924-543">在上述程式碼中，會呼叫 `AddHttpClient` 並提供名稱 *github*。</span><span class="sxs-lookup"><span data-stu-id="db924-543">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="db924-544">此用戶端已套用一些預設組態&mdash;即使用 GitHub API 所需的基底位址和兩個標頭。</span><span class="sxs-lookup"><span data-stu-id="db924-544">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="db924-545">每次呼叫 `CreateClient` 時，會建立 `HttpClient` 的新執行個體並呼叫組態動作。</span><span class="sxs-lookup"><span data-stu-id="db924-545">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="db924-546">若要使用具名用戶端，可以傳遞字串參數至 `CreateClient`。</span><span class="sxs-lookup"><span data-stu-id="db924-546">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="db924-547">指定要建立之用戶端的名稱：</span><span class="sxs-lookup"><span data-stu-id="db924-547">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="db924-548">在上述程式碼中，要求不需要指定主機名稱。</span><span class="sxs-lookup"><span data-stu-id="db924-548">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="db924-549">它可以只傳遞路徑，因為已使用為用戶端設定的基底位址。</span><span class="sxs-lookup"><span data-stu-id="db924-549">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="db924-550">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="db924-550">Typed clients</span></span>

<span data-ttu-id="db924-551">具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="db924-551">Typed clients:</span></span>

* <span data-ttu-id="db924-552">提供與具名用戶端相同的功能，而不需使用字串作為索引鍵。</span><span class="sxs-lookup"><span data-stu-id="db924-552">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="db924-553">取用用戶端時提供 IntelliSense 和編譯器說明。</span><span class="sxs-lookup"><span data-stu-id="db924-553">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="db924-554">提供單一位置來設定特定的 `HttpClient` 並與其互動。</span><span class="sxs-lookup"><span data-stu-id="db924-554">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="db924-555">例如，單一的具型別用戶端可能用於單一的後端端點，並封裝處理該端點的所有邏輯。</span><span class="sxs-lookup"><span data-stu-id="db924-555">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="db924-556">使用 DI 且可在應用程式中需要之處插入。</span><span class="sxs-lookup"><span data-stu-id="db924-556">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="db924-557">具型別用戶端在其建構函式中接受 `HttpClient` 參數：</span><span class="sxs-lookup"><span data-stu-id="db924-557">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="db924-558">在上述程式碼中，組態會移到具型別用戶端。</span><span class="sxs-lookup"><span data-stu-id="db924-558">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="db924-559">`HttpClient` 物件會公開為公用屬性。</span><span class="sxs-lookup"><span data-stu-id="db924-559">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="db924-560">您可定義 API 特定的方法，其公開 `HttpClient` 功能。</span><span class="sxs-lookup"><span data-stu-id="db924-560">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="db924-561">`GetAspNetDocsIssues` 方法會封裝從 GitHub 存放庫查詢和剖析最新開啟問題所需的程式碼。</span><span class="sxs-lookup"><span data-stu-id="db924-561">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="db924-562">若要註冊具型別用戶端，泛型 <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> 擴充方法可用於 `Startup.ConfigureServices` 內，並指定具型別用戶端類別：</span><span class="sxs-lookup"><span data-stu-id="db924-562">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="db924-563">具型別用戶端會向 DI 註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="db924-563">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="db924-564">具型別用戶端可以直接插入並使用：</span><span class="sxs-lookup"><span data-stu-id="db924-564">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="db924-565">想要的話，具型別用戶端的組態可以在 `Startup.ConfigureServices` 中註冊時指定，而不是在具型別用戶端的建構函式中：</span><span class="sxs-lookup"><span data-stu-id="db924-565">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="db924-566">可以將 `HttpClient` 完全封裝在具型別用戶端內。</span><span class="sxs-lookup"><span data-stu-id="db924-566">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="db924-567">可以提供在內部呼叫 `HttpClient` 執行個體的公用方法，而不將它公開為屬性。</span><span class="sxs-lookup"><span data-stu-id="db924-567">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="db924-568">在上述程式碼，`HttpClient` 儲存為私用欄位。</span><span class="sxs-lookup"><span data-stu-id="db924-568">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="db924-569">進行外部呼叫的所有存取都會經歷 `GetRepos` 方法。</span><span class="sxs-lookup"><span data-stu-id="db924-569">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="db924-570">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="db924-570">Generated clients</span></span>

<span data-ttu-id="db924-571">`IHttpClientFactory` 可和其他協力廠商程式庫一起使用，例如 [Refit](https://github.com/paulcbetts/refit)。</span><span class="sxs-lookup"><span data-stu-id="db924-571">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="db924-572">Refit 是適用於 .NET 的 REST 程式庫。</span><span class="sxs-lookup"><span data-stu-id="db924-572">Refit is a REST library for .NET.</span></span> <span data-ttu-id="db924-573">它將 REST API 轉換為即時介面。</span><span class="sxs-lookup"><span data-stu-id="db924-573">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="db924-574">介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。</span><span class="sxs-lookup"><span data-stu-id="db924-574">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="db924-575">定義介面及回覆來代表外部 API 和其回應：</span><span class="sxs-lookup"><span data-stu-id="db924-575">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="db924-576">可以新增具型別用戶端，使用 Refit 產生實作：</span><span class="sxs-lookup"><span data-stu-id="db924-576">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="db924-577">定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：</span><span class="sxs-lookup"><span data-stu-id="db924-577">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="db924-578">外寄要求中介軟體</span><span class="sxs-lookup"><span data-stu-id="db924-578">Outgoing request middleware</span></span>

<span data-ttu-id="db924-579">`HttpClient` 已經有委派可針對外寄 HTTP 要求連結在一起的處理常式的概念。</span><span class="sxs-lookup"><span data-stu-id="db924-579">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="db924-580">`IHttpClientFactory` 可讓您輕鬆地定義要套用於每個具名用戶端的處理常式。</span><span class="sxs-lookup"><span data-stu-id="db924-580">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="db924-581">它支援註冊和鏈結多個處理常式，以建置外寄要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="db924-581">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="db924-582">這些處理常式每個都可以在外寄要求之前和之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="db924-582">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="db924-583">此模式與 ASP.NET Core 中的輸入中介軟體管線相似。</span><span class="sxs-lookup"><span data-stu-id="db924-583">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="db924-584">模式提供一個機制來管理 HTTP 要求的跨領域關注，包括快取、錯誤處理、序列化和記錄。</span><span class="sxs-lookup"><span data-stu-id="db924-584">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="db924-585">若要建立處理常式，請定義衍生自 <xref:System.Net.Http.DelegatingHandler> 的類別。</span><span class="sxs-lookup"><span data-stu-id="db924-585">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="db924-586">覆寫 `SendAsync` 方法，以在將要求傳遞至管線中的下一個處理常式之前執行程式碼：</span><span class="sxs-lookup"><span data-stu-id="db924-586">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="db924-587">上述程式碼定義一個基本處理常式。</span><span class="sxs-lookup"><span data-stu-id="db924-587">The preceding code defines a basic handler.</span></span> <span data-ttu-id="db924-588">它會檢查以查看要求上是否已包含 `X-API-KEY` 標頭。</span><span class="sxs-lookup"><span data-stu-id="db924-588">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="db924-589">如果遺漏標頭，它可以避免 HTTP 呼叫，並傳回適當的回應。</span><span class="sxs-lookup"><span data-stu-id="db924-589">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="db924-590">在註冊期間，可以新增一或多個處理常式至 `HttpClient` 的組態。</span><span class="sxs-lookup"><span data-stu-id="db924-590">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="db924-591">這項工作是透過 <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder> 上的擴充方法完成。</span><span class="sxs-lookup"><span data-stu-id="db924-591">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="db924-592">在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。</span><span class="sxs-lookup"><span data-stu-id="db924-592">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="db924-593">處理常式**必須**在 DI 中註冊為暫時性服務，無限定範圍。</span><span class="sxs-lookup"><span data-stu-id="db924-593">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="db924-594">如果處理常式已註冊為範圍服務，而且處理常式所相依的任何服務都是可處置的：</span><span class="sxs-lookup"><span data-stu-id="db924-594">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="db924-595">處理常式的服務可能會在處理常式超出範圍之前加以處置。</span><span class="sxs-lookup"><span data-stu-id="db924-595">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="db924-596">已處置的處理常式服務會導致處理常式失敗。</span><span class="sxs-lookup"><span data-stu-id="db924-596">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="db924-597">註冊之後，便可以呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>，並傳入處理常式類型。</span><span class="sxs-lookup"><span data-stu-id="db924-597">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="db924-598">可以遵循應該執行的順序來註冊多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="db924-598">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="db924-599">每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：</span><span class="sxs-lookup"><span data-stu-id="db924-599">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="db924-600">使用下列其中一種方式來與訊息處理常式共用個別要求狀態：</span><span class="sxs-lookup"><span data-stu-id="db924-600">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="db924-601">使用 `HttpRequestMessage.Properties` 將資料傳遞到處理常式。</span><span class="sxs-lookup"><span data-stu-id="db924-601">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="db924-602">使用 `IHttpContextAccessor` 來存取目前的要求。</span><span class="sxs-lookup"><span data-stu-id="db924-602">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="db924-603">建立自訂 `AsyncLocal` 儲存體物件以傳遞資料。</span><span class="sxs-lookup"><span data-stu-id="db924-603">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="db924-604">使用 Polly 為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="db924-604">Use Polly-based handlers</span></span>

<span data-ttu-id="db924-605">`IHttpClientFactory` 整合受歡迎的協力廠商程式庫，稱為 [Polly](https://github.com/App-vNext/Polly)。</span><span class="sxs-lookup"><span data-stu-id="db924-605">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="db924-606">Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。</span><span class="sxs-lookup"><span data-stu-id="db924-606">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="db924-607">它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。</span><span class="sxs-lookup"><span data-stu-id="db924-607">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="db924-608">提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="db924-608">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="db924-609">Polly 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="db924-609">The Polly extensions:</span></span>

* <span data-ttu-id="db924-610">支援將以 Polly 為基礎的處理常式新增至用戶端。</span><span class="sxs-lookup"><span data-stu-id="db924-610">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="db924-611">可在安裝 [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet 套件後使用。</span><span class="sxs-lookup"><span data-stu-id="db924-611">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="db924-612">該套件並未包含在 ASP.NET Core 共用架構中。</span><span class="sxs-lookup"><span data-stu-id="db924-612">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="db924-613">處理暫時性錯誤</span><span class="sxs-lookup"><span data-stu-id="db924-613">Handle transient faults</span></span>

<span data-ttu-id="db924-614">大部分的錯誤發生在外部 HTTP 呼叫是暫時性的時候。</span><span class="sxs-lookup"><span data-stu-id="db924-614">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="db924-615">包含一個便利的擴充方法，稱為 `AddTransientHttpErrorPolicy`，它可允許定義原則來處理暫時性錯誤。</span><span class="sxs-lookup"><span data-stu-id="db924-615">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="db924-616">使用此延伸模組方法設定的原則，會處理 `HttpRequestException`、HTTP 5xx 回應和 HTTP 408 回應。</span><span class="sxs-lookup"><span data-stu-id="db924-616">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="db924-617">`AddTransientHttpErrorPolicy` 延伸模組可用於 `Startup.ConfigureServices` 內。</span><span class="sxs-lookup"><span data-stu-id="db924-617">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="db924-618">延伸模組能提供 `PolicyBuilder` 物件的存取，該物件已設定來處理代表可能暫時性錯誤的錯誤：</span><span class="sxs-lookup"><span data-stu-id="db924-618">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="db924-619">在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。</span><span class="sxs-lookup"><span data-stu-id="db924-619">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="db924-620">失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。</span><span class="sxs-lookup"><span data-stu-id="db924-620">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="db924-621">動態選取原則</span><span class="sxs-lookup"><span data-stu-id="db924-621">Dynamically select policies</span></span>

<span data-ttu-id="db924-622">有額外的擴充方法可用來新增 Polly 為基礎的處理常式。</span><span class="sxs-lookup"><span data-stu-id="db924-622">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="db924-623">其中一個這類延伸模組是 `AddPolicyHandler`，它有多個多載。</span><span class="sxs-lookup"><span data-stu-id="db924-623">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="db924-624">一個多載可讓您在定義要套用的原則時，檢查要求：</span><span class="sxs-lookup"><span data-stu-id="db924-624">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="db924-625">在上述程式碼中，如果外寄要求是 HTTP GET，就會套用 10 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="db924-625">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="db924-626">任何其他 HTTP 方法會使用 30 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="db924-626">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="db924-627">新增多個 Polly 處理常式</span><span class="sxs-lookup"><span data-stu-id="db924-627">Add multiple Polly handlers</span></span>

<span data-ttu-id="db924-628">通常會建立巢狀 Polly 原則，以提供增強的功能：</span><span class="sxs-lookup"><span data-stu-id="db924-628">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="db924-629">在上述範例中，會新增兩個處理常式。</span><span class="sxs-lookup"><span data-stu-id="db924-629">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="db924-630">第一個使用 `AddTransientHttpErrorPolicy` 延伸模組來新增重試原則。</span><span class="sxs-lookup"><span data-stu-id="db924-630">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="db924-631">失敗的要求會重試最多三次。</span><span class="sxs-lookup"><span data-stu-id="db924-631">Failed requests are retried up to three times.</span></span> <span data-ttu-id="db924-632">第二個 `AddTransientHttpErrorPolicy` 呼叫會新增斷路器原則。</span><span class="sxs-lookup"><span data-stu-id="db924-632">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="db924-633">如果循序發生五次失敗的嘗試，進一步的外部要求會遭到封鎖 30 秒。</span><span class="sxs-lookup"><span data-stu-id="db924-633">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="db924-634">斷路器原則可設定狀態。</span><span class="sxs-lookup"><span data-stu-id="db924-634">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="db924-635">透過此用戶端的所有呼叫都會共用相同的線路狀態。</span><span class="sxs-lookup"><span data-stu-id="db924-635">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="db924-636">從 Polly 登錄新增原則</span><span class="sxs-lookup"><span data-stu-id="db924-636">Add policies from the Polly registry</span></span>

<span data-ttu-id="db924-637">管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。</span><span class="sxs-lookup"><span data-stu-id="db924-637">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="db924-638">提供了擴充方法，可以使用來自登錄的原則新增處理常式：</span><span class="sxs-lookup"><span data-stu-id="db924-638">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="db924-639">在上述程式碼中，當 `PolicyRegistry` 新增至 `ServiceCollection` 時，註冊了兩個原則。</span><span class="sxs-lookup"><span data-stu-id="db924-639">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="db924-640">為了使用來自登錄的原則，使用了 `AddPolicyHandlerFromRegistry` 方法，並傳遞要套用的原則名稱。</span><span class="sxs-lookup"><span data-stu-id="db924-640">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="db924-641">關於 `IHttpClientFactory` Polly 整合的詳細資訊，可以在 [Polly Wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory) 上找到。</span><span class="sxs-lookup"><span data-stu-id="db924-641">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="db924-642">HttpClient 和存留期管理</span><span class="sxs-lookup"><span data-stu-id="db924-642">HttpClient and lifetime management</span></span>

<span data-ttu-id="db924-643">每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回新的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="db924-643">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="db924-644">每個具名用戶端都有一個 <xref:System.Net.Http.HttpMessageHandler>。</span><span class="sxs-lookup"><span data-stu-id="db924-644">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="db924-645">處理站會管理 `HttpMessageHandler` 執行個體的存留期。</span><span class="sxs-lookup"><span data-stu-id="db924-645">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="db924-646">`IHttpClientFactory` 會將處理站所建立的 `HttpMessageHandler` 執行個體放入集區以減少資源耗用量。</span><span class="sxs-lookup"><span data-stu-id="db924-646">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="db924-647">建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。</span><span class="sxs-lookup"><span data-stu-id="db924-647">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="db924-648">將處理常式放入集區非常實用，因為處理常式通常會管理自己專屬的底層 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="db924-648">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="db924-649">建立比所需數目更多的處理常式，可能會導致連線延遲。</span><span class="sxs-lookup"><span data-stu-id="db924-649">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="db924-650">有些處理常式也會保持連線無限期地開啟，這可能導致處理常式無法對 DNS 變更回應。</span><span class="sxs-lookup"><span data-stu-id="db924-650">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="db924-651">預設處理常式存留時間為兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="db924-651">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="db924-652">可以針對每個具名用戶端覆寫預設值。</span><span class="sxs-lookup"><span data-stu-id="db924-652">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="db924-653">若要覆寫它，請在建立用戶端時所傳回的 `IHttpClientBuilder` 上呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*>：</span><span class="sxs-lookup"><span data-stu-id="db924-653">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="db924-654">不需要處置用戶端。</span><span class="sxs-lookup"><span data-stu-id="db924-654">Disposal of the client isn't required.</span></span> <span data-ttu-id="db924-655">處置會取消傳出的要求，並保證指定的 `HttpClient` 執行個體在呼叫 <xref:System.IDisposable.Dispose*> 之後無法使用。</span><span class="sxs-lookup"><span data-stu-id="db924-655">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="db924-656">`IHttpClientFactory` 會追蹤並處置 `HttpClient` 執行個體使用的資源。</span><span class="sxs-lookup"><span data-stu-id="db924-656">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="db924-657">`HttpClient` 執行個體通常可視為 .NET 物件，不需要處置。</span><span class="sxs-lookup"><span data-stu-id="db924-657">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="db924-658">在開始使用 `IHttpClientFactory` 之前，讓單一 `HttpClient` 執行個體維持一段較長的時間，是很常使用的模式。</span><span class="sxs-lookup"><span data-stu-id="db924-658">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="db924-659">在移轉到 `IHttpClientFactory` 之後，就不再需要此模式。</span><span class="sxs-lookup"><span data-stu-id="db924-659">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="db924-660">IHttpClientFactory 的替代方案</span><span class="sxs-lookup"><span data-stu-id="db924-660">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="db924-661">在啟用 DI 的應用程式中使用 `IHttpClientFactory` 可避免：</span><span class="sxs-lookup"><span data-stu-id="db924-661">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="db924-662">藉由共用 `HttpMessageHandler` 實例的資源耗盡問題。</span><span class="sxs-lookup"><span data-stu-id="db924-662">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="db924-663">以固定間隔迴圈 `HttpMessageHandler` 實例的過時 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="db924-663">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="db924-664">有其他方法可以使用長期 <xref:System.Net.Http.SocketsHttpHandler> 實例來解決上述問題。</span><span class="sxs-lookup"><span data-stu-id="db924-664">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="db924-665">當應用程式啟動時，建立 `SocketsHttpHandler` 的實例，並在應用程式的生命週期中使用它。</span><span class="sxs-lookup"><span data-stu-id="db924-665">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="db924-666">根據 DNS 重新整理時間，將 <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> 設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="db924-666">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="db924-667">視需要使用 `new HttpClient(handler, dispostHandler: false)` 建立 `HttpClient` 實例。</span><span class="sxs-lookup"><span data-stu-id="db924-667">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="db924-668">上述方法可解決 `IHttpClientFactory` 以類似的方式解決的資源管理問題。</span><span class="sxs-lookup"><span data-stu-id="db924-668">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="db924-669">`SocketsHttpHandler` 會共用 `HttpClient` 實例間的連接。</span><span class="sxs-lookup"><span data-stu-id="db924-669">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="db924-670">此共用可防止通訊端耗盡。</span><span class="sxs-lookup"><span data-stu-id="db924-670">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="db924-671">`SocketsHttpHandler` 會根據 `PooledConnectionLifetime` 迴圈連接，以避免過時的 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="db924-671">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="db924-672">Cookies</span><span class="sxs-lookup"><span data-stu-id="db924-672">Cookies</span></span>

<span data-ttu-id="db924-673">集區 `HttpMessageHandler` 實例會導致共用 `CookieContainer` 物件。</span><span class="sxs-lookup"><span data-stu-id="db924-673">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="db924-674">意外的 `CookieContainer` 物件共用通常會導致不正確的程式碼。</span><span class="sxs-lookup"><span data-stu-id="db924-674">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="db924-675">針對需要 cookie 的應用程式，請考慮下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="db924-675">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="db924-676">停用自動 cookie 處理</span><span class="sxs-lookup"><span data-stu-id="db924-676">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="db924-677">避免 `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="db924-677">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="db924-678">呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 以停用自動的 cookie 處理：</span><span class="sxs-lookup"><span data-stu-id="db924-678">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="db924-679">記錄</span><span class="sxs-lookup"><span data-stu-id="db924-679">Logging</span></span>

<span data-ttu-id="db924-680">透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="db924-680">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="db924-681">在記錄設定中啟用適當的資訊層級，以查看預設記錄檔訊息。</span><span class="sxs-lookup"><span data-stu-id="db924-681">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="db924-682">額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。</span><span class="sxs-lookup"><span data-stu-id="db924-682">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="db924-683">用於每個用戶端的記錄檔分類包含用戶端的名稱。</span><span class="sxs-lookup"><span data-stu-id="db924-683">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="db924-684">例如，名為 *MyNamedClient* 的用戶端會記錄分類為 `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler` 的訊息。</span><span class="sxs-lookup"><span data-stu-id="db924-684">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="db924-685">後面加上 *LogicalHandler* 的訊息發生在要求處理常式管線之外。</span><span class="sxs-lookup"><span data-stu-id="db924-685">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="db924-686">在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。</span><span class="sxs-lookup"><span data-stu-id="db924-686">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="db924-687">在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。</span><span class="sxs-lookup"><span data-stu-id="db924-687">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="db924-688">記錄也會發生在要求處理常式管線之內。</span><span class="sxs-lookup"><span data-stu-id="db924-688">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="db924-689">在 *MyNamedClient* 範例中，那些訊息是針對記錄檔分類 `System.Net.Http.HttpClient.MyNamedClient.ClientHandler` 而記錄。</span><span class="sxs-lookup"><span data-stu-id="db924-689">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="db924-690">對於要求，這是發生在所有其他處理常式都已執行之後，並且緊接在網路上傳送要求之前。</span><span class="sxs-lookup"><span data-stu-id="db924-690">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="db924-691">在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。</span><span class="sxs-lookup"><span data-stu-id="db924-691">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="db924-692">在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。</span><span class="sxs-lookup"><span data-stu-id="db924-692">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="db924-693">例如，這可能包括要求標頭的變更，或是回應狀態碼的變更。</span><span class="sxs-lookup"><span data-stu-id="db924-693">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="db924-694">在記錄分類中包含用戶端的名稱，可讓您在需要時進行特定具名用戶端的記錄檔篩選。</span><span class="sxs-lookup"><span data-stu-id="db924-694">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="db924-695">設定 HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="db924-695">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="db924-696">可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。</span><span class="sxs-lookup"><span data-stu-id="db924-696">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="db924-697">新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。</span><span class="sxs-lookup"><span data-stu-id="db924-697">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="db924-698"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 擴充方法可以用來定義委派。</span><span class="sxs-lookup"><span data-stu-id="db924-698">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="db924-699">委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：</span><span class="sxs-lookup"><span data-stu-id="db924-699">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="db924-700">在主控台應用程式中使用 IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="db924-700">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="db924-701">在主控台應用程式中，將下列套件參考新增至專案：</span><span class="sxs-lookup"><span data-stu-id="db924-701">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="db924-702">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="db924-702">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="db924-703">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="db924-703">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="db924-704">在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="db924-704">In the following example:</span></span>

* <span data-ttu-id="db924-705"><xref:System.Net.Http.IHttpClientFactory> 已在[泛型主機的](xref:fundamentals/host/generic-host)服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="db924-705"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="db924-706">`MyService` 會從服務建立用戶端 Factory 執行個體，其可用來建立 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="db924-706">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="db924-707">`HttpClient` 會用來擷取網頁。</span><span class="sxs-lookup"><span data-stu-id="db924-707">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="db924-708">`Main` 會建立範圍來執行服務的 `GetPage` 方法，並將網頁內容的前 500 個字元寫入至主控台。</span><span class="sxs-lookup"><span data-stu-id="db924-708">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="db924-709">其他資源</span><span class="sxs-lookup"><span data-stu-id="db924-709">Additional resources</span></span>

* [<span data-ttu-id="db924-710">使用 HttpClientFactory 實作復原 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="db924-710">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="db924-711">使用 HttpClientFactory 和 Polly 原則以指數輪詢實作 HTTP 呼叫重試</span><span class="sxs-lookup"><span data-stu-id="db924-711">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="db924-712">實作斷路器模式</span><span class="sxs-lookup"><span data-stu-id="db924-712">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
