---
title: 在 ASP.NET Core 中使用 IHttpClientFactory 發出 HTTP 要求
author: stevejgordon
description: 深入了解在 ASP.NET Core 中使用 IHttpClientFactory 介面來管理邏輯 HttpClient 執行個體。
ms.author: scaddie
ms.custom: mvc
ms.date: 10/27/2019
uid: fundamentals/http-requests
ms.openlocfilehash: a963833acfa12889c8ae3dac443962682e1cb931
ms.sourcegitcommit: 032113208bb55ecfb2faeb6d3e9ea44eea827950
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2019
ms.locfileid: "73190578"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="6e5fb-103">在 ASP.NET Core 中使用 IHttpClientFactory 發出 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="6e5fb-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6e5fb-104">作者： [Glenn Condron](https://github.com/glennc)、 [Ryan Nowak](https://github.com/rynowak)、 [Steve Gordon](https://github.com/stevejgordon)、 [Rick Anderson](https://twitter.com/RickAndMSFT)和[Kirk Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="6e5fb-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="6e5fb-105"><xref:System.Net.Http.IHttpClientFactory> 可以註冊及用來在應用程式中設定和建立 <xref:System.Net.Http.HttpClient> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="6e5fb-106">`IHttpClientFactory` 提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="6e5fb-107">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="6e5fb-108">例如，名為*github*的用戶端可以註冊並設定為存取[github](https://github.com/)。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="6e5fb-109">預設用戶端可以註冊以進行一般存取。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="6e5fb-110">透過 `HttpClient`中的委派處理常式，制訂外寄中介軟體的概念。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="6e5fb-111">提供 Polly 為基礎中介軟體的延伸模組，以利用 `HttpClient`中的委派處理常式。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="6e5fb-112">管理基礎 `HttpClientMessageHandler` 實例的共用和存留期。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="6e5fb-113">自動管理可避免在手動管理 `HttpClient` 存留期時所發生的常見 DNS （網域名稱系統）問題。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="6e5fb-114">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="6e5fb-115">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="6e5fb-116">本主題中的範例程式碼會使用 <xref:System.Text.Json> 來還原序列化 HTTP 回應中所傳回的 JSON 內容。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="6e5fb-117">如需使用 `Json.NET` 和 `ReadAsAsync<T>`的範例，請使用版本選取器來選取此主題的2.x 版。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="6e5fb-118">耗用模式</span><span class="sxs-lookup"><span data-stu-id="6e5fb-118">Consumption patterns</span></span>

<span data-ttu-id="6e5fb-119">有數種方式可將 `IHttpClientFactory` 用於應用程式：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="6e5fb-120">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="6e5fb-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="6e5fb-121">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="6e5fb-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="6e5fb-122">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="6e5fb-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="6e5fb-123">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="6e5fb-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="6e5fb-124">最佳方法取決於應用程式的需求。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="6e5fb-125">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="6e5fb-125">Basic usage</span></span>

<span data-ttu-id="6e5fb-126">`IHttpClientFactory` 可以藉由呼叫 `AddHttpClient`進行註冊：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="6e5fb-127">`IHttpClientFactory` 可以使用相依性[插入（DI）](xref:fundamentals/dependency-injection)來要求。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="6e5fb-128">下列程式碼會使用 `IHttpClientFactory` 來建立 `HttpClient` 實例：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="6e5fb-129">使用上述範例中的 `IHttpClientFactory` 就是重構現有應用程式的好方法。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="6e5fb-130">這不會影響使用 `HttpClient` 的方式。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="6e5fb-131">在現有應用程式中建立 `HttpClient` 實例的位置，使用 <xref:System.Net.Http.IHttpClientFactory.CreateClient*>的呼叫來取代這些專案。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="6e5fb-132">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="6e5fb-132">Named clients</span></span>

<span data-ttu-id="6e5fb-133">在下列情況中，命名的用戶端是不錯的選擇：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="6e5fb-134">應用程式需要 `HttpClient`的許多不同用途。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="6e5fb-135">許多 `HttpClient`都有不同的設定。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="6e5fb-136">在 `Startup.ConfigureServices`註冊期間，可以指定已命名 `HttpClient` 的設定：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="6e5fb-137">在上述程式碼中，用戶端是使用下列設定：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="6e5fb-138">基底位址 `https://api.github.com/`。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="6e5fb-139">使用 GitHub API 時需要兩個標頭。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="6e5fb-140">CreateClient</span><span class="sxs-lookup"><span data-stu-id="6e5fb-140">CreateClient</span></span>

<span data-ttu-id="6e5fb-141">每次呼叫 <xref:System.Net.Http.IHttpClientFactory.CreateClient*> 時：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="6e5fb-142">建立 `HttpClient` 的新實例。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="6e5fb-143">會呼叫設定動作。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-143">The configuration action is called.</span></span>

<span data-ttu-id="6e5fb-144">若要建立名為的用戶端，請將其名稱傳遞至 `CreateClient`：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="6e5fb-145">在上述程式碼中，要求不需要指定主機名稱。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="6e5fb-146">此程式碼只會傳遞路徑，因為會使用為用戶端設定的基底位址。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="6e5fb-147">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="6e5fb-147">Typed clients</span></span>

<span data-ttu-id="6e5fb-148">具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-148">Typed clients:</span></span>

* <span data-ttu-id="6e5fb-149">提供與具名用戶端相同的功能，而不需使用字串作為索引鍵。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="6e5fb-150">取用用戶端時提供 IntelliSense 和編譯器說明。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="6e5fb-151">提供單一位置來設定特定的 `HttpClient` 並與其互動。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="6e5fb-152">例如，可能會使用單一具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="6e5fb-153">適用于單一後端端點。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="6e5fb-154">封裝處理端點的所有邏輯。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="6e5fb-155">使用 DI，並可在應用程式中需要的位置插入。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="6e5fb-156">具型別用戶端在其建構函式中接受 `HttpClient` 參數：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-156">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="6e5fb-157">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-157">In the preceding code:</span></span>

* <span data-ttu-id="6e5fb-158">設定會移到具型別用戶端。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="6e5fb-159">`HttpClient` 物件會公開為公用屬性。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="6e5fb-160">可以建立可公開 `HttpClient` 功能的 API 特定方法。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="6e5fb-161">例如，`GetAspNetDocsIssues` 方法會封裝程式碼以取得未解決的問題。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="6e5fb-162">下列程式碼會呼叫 `Startup.ConfigureServices` 中的 <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> 來註冊具類型的用戶端類別：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="6e5fb-163">具型別用戶端會向 DI 註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="6e5fb-164">具型別用戶端可以直接插入並使用：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-164">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="6e5fb-165">型別用戶端的設定可以在 `Startup.ConfigureServices`註冊期間指定，而不是在具型別用戶端的函式中：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-165">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="6e5fb-166">`HttpClient` 可以封裝在具型別用戶端中。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-166">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="6e5fb-167">請定義在內部呼叫 `HttpClient` 實例的方法，而不是將它公開為屬性：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-167">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="6e5fb-168">在上述程式碼中，`HttpClient` 儲存在私用欄位中。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-168">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="6e5fb-169">`HttpClient` 的存取權是公用 `GetRepos` 方法。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-169">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="6e5fb-170">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="6e5fb-170">Generated clients</span></span>

<span data-ttu-id="6e5fb-171">`IHttpClientFactory` 可以與協力廠商程式庫搭配使用，例如重新[調整。](https://github.com/paulcbetts/refit)</span><span class="sxs-lookup"><span data-stu-id="6e5fb-171">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="6e5fb-172">Refit 是適用於 .NET 的 REST 程式庫。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-172">Refit is a REST library for .NET.</span></span> <span data-ttu-id="6e5fb-173">它將 REST API 轉換為即時介面。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-173">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="6e5fb-174">介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-174">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="6e5fb-175">定義介面及回覆來代表外部 API 和其回應：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-175">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="6e5fb-176">可以新增具型別用戶端，使用 Refit 產生實作：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-176">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="6e5fb-177">定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-177">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="6e5fb-178">外寄要求中介軟體</span><span class="sxs-lookup"><span data-stu-id="6e5fb-178">Outgoing request middleware</span></span>

<span data-ttu-id="6e5fb-179">`HttpClient` 具有委派處理常式的概念，可以針對傳出 HTTP 要求連結在一起。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-179">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="6e5fb-180">`IHttpClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="6e5fb-180">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="6e5fb-181">簡化定義要套用至每個已命名用戶端的處理常式。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-181">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="6e5fb-182">支援多個處理常式的註冊和連結，以建立外寄要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-182">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="6e5fb-183">這些處理常式每個都可以在外寄要求之前和之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-183">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="6e5fb-184">此模式：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-184">This pattern:</span></span>

  * <span data-ttu-id="6e5fb-185">類似 ASP.NET Core 中的輸入中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-185">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="6e5fb-186">提供一種機制來管理 HTTP 要求的跨領域考慮，例如：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-186">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="6e5fb-187">快取</span><span class="sxs-lookup"><span data-stu-id="6e5fb-187">caching</span></span>
    * <span data-ttu-id="6e5fb-188">錯誤處理</span><span class="sxs-lookup"><span data-stu-id="6e5fb-188">error handling</span></span>
    * <span data-ttu-id="6e5fb-189">序列化</span><span class="sxs-lookup"><span data-stu-id="6e5fb-189">serialization</span></span>
    * <span data-ttu-id="6e5fb-190">記錄</span><span class="sxs-lookup"><span data-stu-id="6e5fb-190">logging</span></span>

<span data-ttu-id="6e5fb-191">若要建立委派處理常式：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-191">To create a delegating handler:</span></span>

* <span data-ttu-id="6e5fb-192">衍生自 <xref:System.Net.Http.DelegatingHandler>。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-192">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="6e5fb-193">覆寫 <xref:System.Net.Http.DelegatingHandler.SendAsync*>。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-193">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="6e5fb-194">先執行程式碼，再將要求傳遞至管線中的下一個處理常式：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-194">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="6e5fb-195">上述程式碼會檢查 `X-API-KEY` 標頭是否在要求中。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-195">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="6e5fb-196">如果遺漏 `X-API-KEY`，則會傳回 <xref:System.Net.HttpStatusCode.BadRequest>。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-196">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="6e5fb-197">可以將一個以上的處理常式新增至具有 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>之 `HttpClient` 的設定：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-197">More than one handler can be added to the configuration for a `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="6e5fb-198">在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-198">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="6e5fb-199">`IHttpClientFactory` 會為每個處理常式建立個別的 DI 範圍。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-199">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="6e5fb-200">處理常式可以相依于任何範圍的服務。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-200">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="6e5fb-201">處置處理常式時，會處置處理常式所相依的服務。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-201">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="6e5fb-202">註冊之後，便可以呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>，並傳入處理常式的類型。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-202">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="6e5fb-203">可以遵循應該執行的順序來註冊多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-203">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="6e5fb-204">每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-204">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="6e5fb-205">使用下列其中一種方式來與訊息處理常式共用個別要求狀態：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-205">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="6e5fb-206">使用[HttpRequestMessage](xref:System.Net.Http.HttpRequestMessage.Properties)將資料傳遞至處理常式。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-206">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="6e5fb-207">使用 <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> 來存取目前的要求。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-207">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="6e5fb-208">建立自訂 <xref:System.Threading.AsyncLocal`1> 儲存體物件以傳遞資料。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-208">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="6e5fb-209">使用 Polly 為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="6e5fb-209">Use Polly-based handlers</span></span>

<span data-ttu-id="6e5fb-210">`IHttpClientFactory` 與協力廠商程式庫[Polly](https://github.com/App-vNext/Polly)整合。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-210">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="6e5fb-211">Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-211">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="6e5fb-212">它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-212">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="6e5fb-213">提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-213">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="6e5fb-214">Polly 擴充功能支援將以 Polly 為基礎的處理常式新增至用戶端。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-214">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="6e5fb-215">Polly 需要[Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-215">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="6e5fb-216">處理暫時性錯誤</span><span class="sxs-lookup"><span data-stu-id="6e5fb-216">Handle transient faults</span></span>

<span data-ttu-id="6e5fb-217">當外部 HTTP 呼叫是暫時性的時，通常會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-217">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="6e5fb-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> 允許定義原則來處理暫時性錯誤。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="6e5fb-219">以 `AddTransientHttpErrorPolicy` 設定的原則會處理下列回應：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-219">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="6e5fb-220">HTTP 5xx</span><span class="sxs-lookup"><span data-stu-id="6e5fb-220">HTTP 5xx</span></span>
* <span data-ttu-id="6e5fb-221">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="6e5fb-221">HTTP 408</span></span>

<span data-ttu-id="6e5fb-222">`AddTransientHttpErrorPolicy` 提供 `PolicyBuilder` 物件的存取權，其設定為處理代表可能暫時性錯誤的錯誤：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-222">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="6e5fb-223">在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-223">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="6e5fb-224">失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-224">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="6e5fb-225">動態選取原則</span><span class="sxs-lookup"><span data-stu-id="6e5fb-225">Dynamically select policies</span></span>

<span data-ttu-id="6e5fb-226">提供擴充方法來加入以 Polly 為基礎的處理常式，例如 <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-226">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="6e5fb-227">下列 `AddPolicyHandler` 多載會檢查要求以決定要套用的原則：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-227">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="6e5fb-228">在上述程式碼中，如果外寄要求是 HTTP GET，就會套用 10 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-228">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="6e5fb-229">任何其他 HTTP 方法會使用 30 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-229">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="6e5fb-230">新增多個 Polly 處理常式</span><span class="sxs-lookup"><span data-stu-id="6e5fb-230">Add multiple Polly handlers</span></span>

<span data-ttu-id="6e5fb-231">通常會將 Polly 原則加以嵌套：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-231">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="6e5fb-232">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-232">In the preceding example:</span></span>

* <span data-ttu-id="6e5fb-233">新增了兩個處理常式。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-233">Two handlers are added.</span></span>
* <span data-ttu-id="6e5fb-234">第一個處理常式會使用 <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> 來新增重試原則。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-234">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="6e5fb-235">失敗的要求會重試最多三次。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-235">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="6e5fb-236">第二個 `AddTransientHttpErrorPolicy` 呼叫會增加斷路器原則。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-236">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="6e5fb-237">如果連續5次嘗試失敗，則會封鎖進一步的外部要求30秒。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-237">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="6e5fb-238">斷路器原則可設定狀態。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-238">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="6e5fb-239">透過此用戶端的所有呼叫都會共用相同的線路狀態。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-239">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="6e5fb-240">從 Polly 登錄新增原則</span><span class="sxs-lookup"><span data-stu-id="6e5fb-240">Add policies from the Polly registry</span></span>

<span data-ttu-id="6e5fb-241">管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-241">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="6e5fb-242">在下列程式碼中：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-242">In the following code:</span></span>

* <span data-ttu-id="6e5fb-243">系統會新增「一般」和「長」原則。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-243">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="6e5fb-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*> 從登錄新增「一般」和「長」原則。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="6e5fb-245">如需 `IHttpClientFactory` 和 Polly 整合的詳細資訊，請參閱[Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory)。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-245">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="6e5fb-246">HttpClient 和存留期管理</span><span class="sxs-lookup"><span data-stu-id="6e5fb-246">HttpClient and lifetime management</span></span>

<span data-ttu-id="6e5fb-247">每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回新的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-247">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="6e5fb-248">系統會根據每個命名的用戶端來建立 <xref:System.Net.Http.HttpMessageHandler>。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-248">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="6e5fb-249">處理站會管理 `HttpMessageHandler` 執行個體的存留期。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-249">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="6e5fb-250">`IHttpClientFactory` 會將處理站所建立的 `HttpMessageHandler` 執行個體放入集區以減少資源耗用量。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-250">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="6e5fb-251">建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-251">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="6e5fb-252">將處理常式放入集區非常實用，因為處理常式通常會管理自己專屬的底層 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-252">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="6e5fb-253">建立比所需數目更多的處理常式，可能會導致連線延遲。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-253">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="6e5fb-254">有些處理常式也會保持連線無限期地開啟，這可能導致處理常式無法回應 DNS （網域名稱系統）變更。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-254">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="6e5fb-255">預設處理常式存留時間為兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-255">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="6e5fb-256">預設值可以根據每個命名的用戶端來覆寫：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-256">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="6e5fb-257">`HttpClient` 實例通常可視為**不**需要處置的 .net 物件。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-257">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="6e5fb-258">處置會取消傳出的要求，並保證指定的 `HttpClient` 執行個體在呼叫 <xref:System.IDisposable.Dispose*> 之後無法使用。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-258">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="6e5fb-259">`IHttpClientFactory` 會追蹤並處置 `HttpClient` 執行個體使用的資源。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-259">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="6e5fb-260">在開始使用 `IHttpClientFactory` 之前，讓單一 `HttpClient` 執行個體維持一段較長的時間，是很常使用的模式。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-260">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="6e5fb-261">在移轉到 `IHttpClientFactory` 之後，就不再需要此模式。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-261">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="6e5fb-262">記錄</span><span class="sxs-lookup"><span data-stu-id="6e5fb-262">Logging</span></span>

<span data-ttu-id="6e5fb-263">透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-263">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="6e5fb-264">在記錄設定中啟用適當的資訊層級，以查看預設記錄檔訊息。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-264">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="6e5fb-265">額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-265">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="6e5fb-266">用於每個用戶端的記錄檔分類包含用戶端的名稱。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-266">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="6e5fb-267">例如，名為*MyNamedClient*的用戶端會記錄類別為 "HttpClient" 的訊息。**MyNamedClient**。LogicalHandler "。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-267">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="6e5fb-268">後面加上 *LogicalHandler* 的訊息發生在要求處理常式管線之外。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-268">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="6e5fb-269">在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-269">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="6e5fb-270">在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-270">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="6e5fb-271">記錄也會發生在要求處理常式管線之內。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-271">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="6e5fb-272">在*MyNamedClient*範例中，這些訊息會記錄在記錄檔類別中為 "HttpClient"。**MyNamedClient**。ClientHandler".</span><span class="sxs-lookup"><span data-stu-id="6e5fb-272">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="6e5fb-273">對於要求，這會在所有其他處理常式都執行之後，且在傳送要求之前立即發生。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-273">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="6e5fb-274">在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-274">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="6e5fb-275">在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-275">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="6e5fb-276">這可能包括要求標頭的變更或回應狀態碼。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-276">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="6e5fb-277">在記錄類別中包含用戶端的名稱，可以針對特定的已命名用戶端進行記錄篩選。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-277">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="6e5fb-278">設定 HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="6e5fb-278">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="6e5fb-279">可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-279">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="6e5fb-280">新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-280">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="6e5fb-281"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 擴充方法可以用來定義委派。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-281">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="6e5fb-282">委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-282">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="6e5fb-283">在主控台應用程式中使用 IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="6e5fb-283">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="6e5fb-284">在主控台應用程式中，將下列套件參考新增至專案：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-284">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="6e5fb-285">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="6e5fb-285">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="6e5fb-286">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="6e5fb-286">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="6e5fb-287">在以下範例中：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-287">In the following example:</span></span>

* <span data-ttu-id="6e5fb-288"><xref:System.Net.Http.IHttpClientFactory> 已在[泛型主機的](xref:fundamentals/host/generic-host)服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-288"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="6e5fb-289">`MyService` 會從服務建立用戶端 Factory 執行個體，其可用來建立 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-289">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="6e5fb-290">`HttpClient` 會用來擷取網頁。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-290">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="6e5fb-291">`Main` 會建立範圍來執行服務的 `GetPage` 方法，並將網頁內容的前 500 個字元寫入至主控台。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-291">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="6e5fb-292">其他資源</span><span class="sxs-lookup"><span data-stu-id="6e5fb-292">Additional resources</span></span>

* [<span data-ttu-id="6e5fb-293">使用 HttpClientFactory 實作復原 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="6e5fb-293">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="6e5fb-294">使用 HttpClientFactory 和 Polly 原則以指數輪詢實作 HTTP 呼叫重試</span><span class="sxs-lookup"><span data-stu-id="6e5fb-294">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="6e5fb-295">實作斷路器模式</span><span class="sxs-lookup"><span data-stu-id="6e5fb-295">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="6e5fb-296">作者：[Glenn Condron](https://github.com/glennc)、[Ryan Nowak](https://github.com/rynowak) 和 [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="6e5fb-296">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="6e5fb-297"><xref:System.Net.Http.IHttpClientFactory> 可以註冊及用來在應用程式中設定和建立 <xref:System.Net.Http.HttpClient> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-297">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="6e5fb-298">它提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-298">It offers the following benefits:</span></span>

* <span data-ttu-id="6e5fb-299">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-299">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="6e5fb-300">例如，可註冊及設定 *github* 用戶端，來存取 [GitHub](https://github.com/)。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-300">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="6e5fb-301">預設用戶端可以註冊用於其他用途。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-301">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="6e5fb-302">透過委派 `HttpClient` 中的處理常式來撰寫外寄中介軟體的概念，並提供延伸模組以便 Polly 架構中介軟體利用外寄中介軟體。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-302">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="6e5fb-303">管理基礎 `HttpClientMessageHandler` 執行個體的共用和存留期，以避免在手動管理 `HttpClient` 存留期時，發生的常見 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-303">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="6e5fb-304">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-304">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="6e5fb-305">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6e5fb-305">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="6e5fb-306">耗用模式</span><span class="sxs-lookup"><span data-stu-id="6e5fb-306">Consumption patterns</span></span>

<span data-ttu-id="6e5fb-307">有數種方式可將 `IHttpClientFactory` 用於應用程式：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-307">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="6e5fb-308">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="6e5fb-308">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="6e5fb-309">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="6e5fb-309">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="6e5fb-310">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="6e5fb-310">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="6e5fb-311">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="6e5fb-311">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="6e5fb-312">它們全都不會嚴格優先於另一個。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-312">None of them are strictly superior to another.</span></span> <span data-ttu-id="6e5fb-313">最好的方法取決於應用程式的條件約束。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-313">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="6e5fb-314">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="6e5fb-314">Basic usage</span></span>

<span data-ttu-id="6e5fb-315">`IHttpClientFactory` 可以藉由在 `Startup.ConfigureServices` 方法內的 `IServiceCollection` 上呼叫 `AddHttpClient` 擴充方法來註冊。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-315">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="6e5fb-316">註冊之後，程式碼可以在可使用[相依性插入 (DI)](xref:fundamentals/dependency-injection) 插入服務的任何位置，接受 `IHttpClientFactory`。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-316">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="6e5fb-317">`IHttpClientFactory` 可以用來建立 `HttpClient` 執行個體：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-317">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="6e5fb-318">以這種方式使用 `IHttpClientFactory` 是重構現有應用程式的好方法。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-318">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="6e5fb-319">它對 `HttpClient` 的使用方式沒有任何影響。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-319">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="6e5fb-320">在目前建立 `HttpClient` 執行個體的位置，將那些項目取代為呼叫 <xref:System.Net.Http.IHttpClientFactory.CreateClient*>。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-320">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="6e5fb-321">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="6e5fb-321">Named clients</span></span>

<span data-ttu-id="6e5fb-322">如果應用程式需要使用多個不同的 `HttpClient`，且每個都有不同的設定，可以選擇使用**具名用戶端**。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-322">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="6e5fb-323">具名 `HttpClient` 的組態可以在 `Startup.ConfigureServices` 中註冊時指定。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-323">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="6e5fb-324">在上述程式碼中，會呼叫 `AddHttpClient` 並提供名稱 *github*。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-324">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="6e5fb-325">此用戶端已套用一些預設組態&mdash;即使用 GitHub API 所需的基底位址和兩個標頭。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-325">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="6e5fb-326">每次呼叫 `CreateClient` 時，會建立 `HttpClient` 的新執行個體並呼叫組態動作。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-326">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="6e5fb-327">若要使用具名用戶端，可以傳遞字串參數至 `CreateClient`。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-327">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="6e5fb-328">指定要建立之用戶端的名稱：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-328">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="6e5fb-329">在上述程式碼中，要求不需要指定主機名稱。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-329">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="6e5fb-330">它可以只傳遞路徑，因為已使用為用戶端設定的基底位址。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-330">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="6e5fb-331">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="6e5fb-331">Typed clients</span></span>

<span data-ttu-id="6e5fb-332">具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-332">Typed clients:</span></span>

* <span data-ttu-id="6e5fb-333">提供與具名用戶端相同的功能，而不需使用字串作為索引鍵。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-333">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="6e5fb-334">取用用戶端時提供 IntelliSense 和編譯器說明。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-334">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="6e5fb-335">提供單一位置來設定特定的 `HttpClient` 並與其互動。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-335">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="6e5fb-336">例如，單一的具型別用戶端可能用於單一的後端端點，並封裝處理該端點的所有邏輯。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-336">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="6e5fb-337">使用 DI 且可在應用程式中需要之處插入。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-337">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="6e5fb-338">具型別用戶端在其建構函式中接受 `HttpClient` 參數：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-338">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="6e5fb-339">在上述程式碼中，組態會移到具型別用戶端。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-339">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="6e5fb-340">`HttpClient` 物件會公開為公用屬性。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-340">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="6e5fb-341">您可定義 API 特定的方法，其公開 `HttpClient` 功能。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-341">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="6e5fb-342">`GetAspNetDocsIssues` 方法會封裝從 GitHub 存放庫查詢和剖析最新開啟問題所需的程式碼。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-342">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="6e5fb-343">若要註冊具型別用戶端，泛型 <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> 擴充方法可用於 `Startup.ConfigureServices` 內，並指定具型別用戶端類別：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-343">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="6e5fb-344">具型別用戶端會向 DI 註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-344">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="6e5fb-345">具型別用戶端可以直接插入並使用：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-345">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="6e5fb-346">想要的話，具型別用戶端的組態可以在 `Startup.ConfigureServices` 中註冊時指定，而不是在具型別用戶端的建構函式中：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-346">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="6e5fb-347">可以將 `HttpClient` 完全封裝在具型別用戶端內。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-347">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="6e5fb-348">可以提供在內部呼叫 `HttpClient` 執行個體的公用方法，而不將它公開為屬性。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-348">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="6e5fb-349">在上述程式碼，`HttpClient` 儲存為私用欄位。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-349">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="6e5fb-350">進行外部呼叫的所有存取都會經歷 `GetRepos` 方法。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-350">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="6e5fb-351">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="6e5fb-351">Generated clients</span></span>

<span data-ttu-id="6e5fb-352">`IHttpClientFactory` 可和其他協力廠商程式庫一起使用，例如 [Refit](https://github.com/paulcbetts/refit)。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-352">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="6e5fb-353">Refit 是適用於 .NET 的 REST 程式庫。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-353">Refit is a REST library for .NET.</span></span> <span data-ttu-id="6e5fb-354">它將 REST API 轉換為即時介面。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-354">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="6e5fb-355">介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-355">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="6e5fb-356">定義介面及回覆來代表外部 API 和其回應：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-356">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="6e5fb-357">可以新增具型別用戶端，使用 Refit 產生實作：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-357">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="6e5fb-358">定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-358">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="6e5fb-359">外寄要求中介軟體</span><span class="sxs-lookup"><span data-stu-id="6e5fb-359">Outgoing request middleware</span></span>

<span data-ttu-id="6e5fb-360">`HttpClient` 已經有委派可針對外寄 HTTP 要求連結在一起的處理常式的概念。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-360">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="6e5fb-361">`IHttpClientFactory` 可讓您輕鬆地定義要套用於每個具名用戶端的處理常式。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-361">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="6e5fb-362">它支援註冊和鏈結多個處理常式，以建置外寄要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-362">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="6e5fb-363">這些處理常式每個都可以在外寄要求之前和之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-363">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="6e5fb-364">此模式與 ASP.NET Core 中的輸入中介軟體管線相似。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-364">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="6e5fb-365">模式提供一個機制來管理 HTTP 要求的跨領域關注，包括快取、錯誤處理、序列化和記錄。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-365">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="6e5fb-366">若要建立處理常式，請定義衍生自 <xref:System.Net.Http.DelegatingHandler> 的類別。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-366">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="6e5fb-367">覆寫 `SendAsync` 方法，以在將要求傳遞至管線中的下一個處理常式之前執行程式碼：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-367">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="6e5fb-368">上述程式碼定義一個基本處理常式。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-368">The preceding code defines a basic handler.</span></span> <span data-ttu-id="6e5fb-369">它會檢查以查看要求上是否已包含 `X-API-KEY` 標頭。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-369">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="6e5fb-370">如果遺漏標頭，它可以避免 HTTP 呼叫，並傳回適當的回應。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-370">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="6e5fb-371">在註冊期間，可以新增一或多個處理常式至 `HttpClient` 的組態。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-371">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="6e5fb-372">這項工作是透過 <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder> 上的擴充方法完成。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-372">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="6e5fb-373">在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-373">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="6e5fb-374">`IHttpClientFactory` 會為每個處理常式建立個別的 DI 範圍。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-374">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="6e5fb-375">處理常式可相依於任何範圍的服務。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-375">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="6e5fb-376">處置處理常式時，會處置處理常式所相依的服務。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-376">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="6e5fb-377">註冊之後，便可以呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>，並傳入處理常式的類型。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-377">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="6e5fb-378">可以遵循應該執行的順序來註冊多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-378">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="6e5fb-379">每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-379">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="6e5fb-380">使用下列其中一種方式來與訊息處理常式共用個別要求狀態：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-380">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="6e5fb-381">使用 `HttpRequestMessage.Properties` 將資料傳遞到處理常式。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-381">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="6e5fb-382">使用 `IHttpContextAccessor` 來存取目前的要求。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-382">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="6e5fb-383">建立自訂 `AsyncLocal` 儲存體物件以傳遞資料。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-383">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="6e5fb-384">使用 Polly 為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="6e5fb-384">Use Polly-based handlers</span></span>

<span data-ttu-id="6e5fb-385">`IHttpClientFactory` 整合受歡迎的協力廠商程式庫，稱為 [Polly](https://github.com/App-vNext/Polly)。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-385">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="6e5fb-386">Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-386">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="6e5fb-387">它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-387">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="6e5fb-388">提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-388">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="6e5fb-389">Polly 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-389">The Polly extensions:</span></span>

* <span data-ttu-id="6e5fb-390">支援將以 Polly 為基礎的處理常式新增至用戶端。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-390">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="6e5fb-391">可在安裝 [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet 套件後使用。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-391">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="6e5fb-392">該套件並未包含在 ASP.NET Core 共用架構中。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-392">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="6e5fb-393">處理暫時性錯誤</span><span class="sxs-lookup"><span data-stu-id="6e5fb-393">Handle transient faults</span></span>

<span data-ttu-id="6e5fb-394">大部分的錯誤發生在外部 HTTP 呼叫是暫時性的時候。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-394">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="6e5fb-395">包含一個便利的擴充方法，稱為 `AddTransientHttpErrorPolicy`，它可允許定義原則來處理暫時性錯誤。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-395">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="6e5fb-396">使用此延伸模組方法設定的原則，會處理 `HttpRequestException`、HTTP 5xx 回應和 HTTP 408 回應。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-396">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="6e5fb-397">`AddTransientHttpErrorPolicy` 延伸模組可用於 `Startup.ConfigureServices` 內。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-397">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="6e5fb-398">延伸模組能提供 `PolicyBuilder` 物件的存取，該物件已設定來處理代表可能暫時性錯誤的錯誤：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-398">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="6e5fb-399">在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-399">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="6e5fb-400">失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-400">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="6e5fb-401">動態選取原則</span><span class="sxs-lookup"><span data-stu-id="6e5fb-401">Dynamically select policies</span></span>

<span data-ttu-id="6e5fb-402">有額外的擴充方法可用來新增 Polly 為基礎的處理常式。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-402">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="6e5fb-403">其中一個這類延伸模組是 `AddPolicyHandler`，它有多個多載。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-403">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="6e5fb-404">一個多載可讓您在定義要套用的原則時，檢查要求：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-404">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="6e5fb-405">在上述程式碼中，如果外寄要求是 HTTP GET，就會套用 10 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-405">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="6e5fb-406">任何其他 HTTP 方法會使用 30 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-406">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="6e5fb-407">新增多個 Polly 處理常式</span><span class="sxs-lookup"><span data-stu-id="6e5fb-407">Add multiple Polly handlers</span></span>

<span data-ttu-id="6e5fb-408">通常會建立巢狀 Polly 原則，以提供增強的功能：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-408">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="6e5fb-409">在上述範例中，會新增兩個處理常式。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-409">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="6e5fb-410">第一個使用 `AddTransientHttpErrorPolicy` 延伸模組來新增重試原則。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-410">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="6e5fb-411">失敗的要求會重試最多三次。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-411">Failed requests are retried up to three times.</span></span> <span data-ttu-id="6e5fb-412">第二個 `AddTransientHttpErrorPolicy` 呼叫會新增斷路器原則。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-412">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="6e5fb-413">如果循序發生五次失敗的嘗試，進一步的外部要求會遭到封鎖 30 秒。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-413">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="6e5fb-414">斷路器原則可設定狀態。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-414">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="6e5fb-415">透過此用戶端的所有呼叫都會共用相同的線路狀態。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-415">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="6e5fb-416">從 Polly 登錄新增原則</span><span class="sxs-lookup"><span data-stu-id="6e5fb-416">Add policies from the Polly registry</span></span>

<span data-ttu-id="6e5fb-417">管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-417">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="6e5fb-418">提供了擴充方法，可以使用來自登錄的原則新增處理常式：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-418">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="6e5fb-419">在上述程式碼中，當 `PolicyRegistry` 新增至 `ServiceCollection` 時，註冊了兩個原則。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-419">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="6e5fb-420">為了使用來自登錄的原則，使用了 `AddPolicyHandlerFromRegistry` 方法，並傳遞要套用的原則名稱。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-420">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="6e5fb-421">關於 `IHttpClientFactory` Polly 整合的詳細資訊，可以在 [Polly Wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory) 上找到。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-421">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="6e5fb-422">HttpClient 和存留期管理</span><span class="sxs-lookup"><span data-stu-id="6e5fb-422">HttpClient and lifetime management</span></span>

<span data-ttu-id="6e5fb-423">每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回新的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-423">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="6e5fb-424">每個具名用戶端都有一個 <xref:System.Net.Http.HttpMessageHandler>。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-424">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="6e5fb-425">處理站會管理 `HttpMessageHandler` 執行個體的存留期。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-425">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="6e5fb-426">`IHttpClientFactory` 會將處理站所建立的 `HttpMessageHandler` 執行個體放入集區以減少資源耗用量。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-426">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="6e5fb-427">建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-427">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="6e5fb-428">將處理常式放入集區非常實用，因為處理常式通常會管理自己專屬的底層 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-428">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="6e5fb-429">建立比所需數目更多的處理常式，可能會導致連線延遲。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-429">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="6e5fb-430">有些處理常式也會保持連線無限期地開啟，這可能導致處理常式無法對 DNS 變更回應。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-430">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="6e5fb-431">預設處理常式存留時間為兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-431">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="6e5fb-432">可以針對每個具名用戶端覆寫預設值。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-432">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="6e5fb-433">若要覆寫它，請在建立用戶端時所傳回的 `IHttpClientBuilder` 上呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*>：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-433">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="6e5fb-434">不需要處置用戶端。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-434">Disposal of the client isn't required.</span></span> <span data-ttu-id="6e5fb-435">處置會取消傳出的要求，並保證指定的 `HttpClient` 執行個體在呼叫 <xref:System.IDisposable.Dispose*> 之後無法使用。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-435">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="6e5fb-436">`IHttpClientFactory` 會追蹤並處置 `HttpClient` 執行個體使用的資源。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-436">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="6e5fb-437">`HttpClient` 執行個體通常可視為 .NET 物件，不需要處置。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-437">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="6e5fb-438">在開始使用 `IHttpClientFactory` 之前，讓單一 `HttpClient` 執行個體維持一段較長的時間，是很常使用的模式。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-438">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="6e5fb-439">在移轉到 `IHttpClientFactory` 之後，就不再需要此模式。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-439">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="6e5fb-440">記錄</span><span class="sxs-lookup"><span data-stu-id="6e5fb-440">Logging</span></span>

<span data-ttu-id="6e5fb-441">透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-441">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="6e5fb-442">在記錄設定中啟用適當的資訊層級，以查看預設記錄檔訊息。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-442">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="6e5fb-443">額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-443">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="6e5fb-444">用於每個用戶端的記錄檔分類包含用戶端的名稱。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-444">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="6e5fb-445">例如，名為 *MyNamedClient* 的用戶端會記錄分類為 `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler` 的訊息。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-445">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="6e5fb-446">後面加上 *LogicalHandler* 的訊息發生在要求處理常式管線之外。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-446">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="6e5fb-447">在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-447">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="6e5fb-448">在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-448">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="6e5fb-449">記錄也會發生在要求處理常式管線之內。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-449">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="6e5fb-450">在 *MyNamedClient* 範例中，那些訊息是針對記錄檔分類 `System.Net.Http.HttpClient.MyNamedClient.ClientHandler` 而記錄。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-450">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="6e5fb-451">對於要求，這是發生在所有其他處理常式都已執行之後，並且緊接在網路上傳送要求之前。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-451">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="6e5fb-452">在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-452">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="6e5fb-453">在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-453">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="6e5fb-454">例如，這可能包括要求標頭的變更，或是回應狀態碼的變更。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-454">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="6e5fb-455">在記錄分類中包含用戶端的名稱，可讓您在需要時進行特定具名用戶端的記錄檔篩選。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-455">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="6e5fb-456">設定 HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="6e5fb-456">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="6e5fb-457">可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-457">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="6e5fb-458">新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-458">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="6e5fb-459"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 擴充方法可以用來定義委派。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-459">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="6e5fb-460">委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-460">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="6e5fb-461">在主控台應用程式中使用 IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="6e5fb-461">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="6e5fb-462">在主控台應用程式中，將下列套件參考新增至專案：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-462">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="6e5fb-463">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="6e5fb-463">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="6e5fb-464">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="6e5fb-464">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="6e5fb-465">在以下範例中：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-465">In the following example:</span></span>

* <span data-ttu-id="6e5fb-466"><xref:System.Net.Http.IHttpClientFactory> 已在[泛型主機的](xref:fundamentals/host/generic-host)服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-466"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="6e5fb-467">`MyService` 會從服務建立用戶端 Factory 執行個體，其可用來建立 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-467">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="6e5fb-468">`HttpClient` 會用來擷取網頁。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-468">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="6e5fb-469">`Main` 會建立範圍來執行服務的 `GetPage` 方法，並將網頁內容的前 500 個字元寫入至主控台。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-469">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="6e5fb-470">其他資源</span><span class="sxs-lookup"><span data-stu-id="6e5fb-470">Additional resources</span></span>

* [<span data-ttu-id="6e5fb-471">使用 HttpClientFactory 實作復原 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="6e5fb-471">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="6e5fb-472">使用 HttpClientFactory 和 Polly 原則以指數輪詢實作 HTTP 呼叫重試</span><span class="sxs-lookup"><span data-stu-id="6e5fb-472">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="6e5fb-473">實作斷路器模式</span><span class="sxs-lookup"><span data-stu-id="6e5fb-473">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

<span data-ttu-id="6e5fb-474">作者：[Glenn Condron](https://github.com/glennc)、[Ryan Nowak](https://github.com/rynowak) 和 [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="6e5fb-474">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="6e5fb-475"><xref:System.Net.Http.IHttpClientFactory> 可以註冊及用來在應用程式中設定和建立 <xref:System.Net.Http.HttpClient> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-475">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="6e5fb-476">它提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-476">It offers the following benefits:</span></span>

* <span data-ttu-id="6e5fb-477">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-477">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="6e5fb-478">例如，可註冊及設定 *github* 用戶端，來存取 [GitHub](https://github.com/)。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-478">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="6e5fb-479">預設用戶端可以註冊用於其他用途。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-479">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="6e5fb-480">透過委派 `HttpClient` 中的處理常式來撰寫外寄中介軟體的概念，並提供延伸模組以便 Polly 架構中介軟體利用外寄中介軟體。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-480">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="6e5fb-481">管理基礎 `HttpClientMessageHandler` 執行個體的共用和存留期，以避免在手動管理 `HttpClient` 存留期時，發生的常見 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-481">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="6e5fb-482">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-482">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="6e5fb-483">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6e5fb-483">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6e5fb-484">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="6e5fb-484">Prerequisites</span></span>

<span data-ttu-id="6e5fb-485">以 .NET Framework 為目標的專案，需要安裝 [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-485">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="6e5fb-486">以 .NET Core 為目標且參考 [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) 的專案，已包含 `Microsoft.Extensions.Http` 套件。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-486">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="6e5fb-487">耗用模式</span><span class="sxs-lookup"><span data-stu-id="6e5fb-487">Consumption patterns</span></span>

<span data-ttu-id="6e5fb-488">有數種方式可將 `IHttpClientFactory` 用於應用程式：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-488">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="6e5fb-489">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="6e5fb-489">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="6e5fb-490">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="6e5fb-490">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="6e5fb-491">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="6e5fb-491">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="6e5fb-492">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="6e5fb-492">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="6e5fb-493">它們全都不會嚴格優先於另一個。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-493">None of them are strictly superior to another.</span></span> <span data-ttu-id="6e5fb-494">最好的方法取決於應用程式的條件約束。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-494">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="6e5fb-495">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="6e5fb-495">Basic usage</span></span>

<span data-ttu-id="6e5fb-496">`IHttpClientFactory` 可以藉由在 `Startup.ConfigureServices` 方法內的 `IServiceCollection` 上呼叫 `AddHttpClient` 擴充方法來註冊。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-496">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="6e5fb-497">註冊之後，程式碼可以在可使用[相依性插入 (DI)](xref:fundamentals/dependency-injection) 插入服務的任何位置，接受 `IHttpClientFactory`。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-497">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="6e5fb-498">`IHttpClientFactory` 可以用來建立 `HttpClient` 執行個體：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-498">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="6e5fb-499">以這種方式使用 `IHttpClientFactory` 是重構現有應用程式的好方法。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-499">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="6e5fb-500">它對 `HttpClient` 的使用方式沒有任何影響。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-500">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="6e5fb-501">在目前建立 `HttpClient` 執行個體的位置，將那些項目取代為呼叫 <xref:System.Net.Http.IHttpClientFactory.CreateClient*>。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-501">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="6e5fb-502">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="6e5fb-502">Named clients</span></span>

<span data-ttu-id="6e5fb-503">如果應用程式需要使用多個不同的 `HttpClient`，且每個都有不同的設定，可以選擇使用**具名用戶端**。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-503">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="6e5fb-504">具名 `HttpClient` 的組態可以在 `Startup.ConfigureServices` 中註冊時指定。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-504">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="6e5fb-505">在上述程式碼中，會呼叫 `AddHttpClient` 並提供名稱 *github*。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-505">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="6e5fb-506">此用戶端已套用一些預設組態&mdash;即使用 GitHub API 所需的基底位址和兩個標頭。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-506">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="6e5fb-507">每次呼叫 `CreateClient` 時，會建立 `HttpClient` 的新執行個體並呼叫組態動作。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-507">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="6e5fb-508">若要使用具名用戶端，可以傳遞字串參數至 `CreateClient`。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-508">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="6e5fb-509">指定要建立之用戶端的名稱：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-509">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="6e5fb-510">在上述程式碼中，要求不需要指定主機名稱。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-510">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="6e5fb-511">它可以只傳遞路徑，因為已使用為用戶端設定的基底位址。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-511">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="6e5fb-512">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="6e5fb-512">Typed clients</span></span>

<span data-ttu-id="6e5fb-513">具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-513">Typed clients:</span></span>

* <span data-ttu-id="6e5fb-514">提供與具名用戶端相同的功能，而不需使用字串作為索引鍵。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-514">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="6e5fb-515">取用用戶端時提供 IntelliSense 和編譯器說明。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-515">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="6e5fb-516">提供單一位置來設定特定的 `HttpClient` 並與其互動。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-516">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="6e5fb-517">例如，單一的具型別用戶端可能用於單一的後端端點，並封裝處理該端點的所有邏輯。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-517">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="6e5fb-518">使用 DI 且可在應用程式中需要之處插入。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-518">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="6e5fb-519">具型別用戶端在其建構函式中接受 `HttpClient` 參數：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-519">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="6e5fb-520">在上述程式碼中，組態會移到具型別用戶端。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-520">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="6e5fb-521">`HttpClient` 物件會公開為公用屬性。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-521">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="6e5fb-522">您可定義 API 特定的方法，其公開 `HttpClient` 功能。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-522">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="6e5fb-523">`GetAspNetDocsIssues` 方法會封裝從 GitHub 存放庫查詢和剖析最新開啟問題所需的程式碼。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-523">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="6e5fb-524">若要註冊具型別用戶端，泛型 <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> 擴充方法可用於 `Startup.ConfigureServices` 內，並指定具型別用戶端類別：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-524">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="6e5fb-525">具型別用戶端會向 DI 註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-525">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="6e5fb-526">具型別用戶端可以直接插入並使用：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-526">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="6e5fb-527">想要的話，具型別用戶端的組態可以在 `Startup.ConfigureServices` 中註冊時指定，而不是在具型別用戶端的建構函式中：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-527">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="6e5fb-528">可以將 `HttpClient` 完全封裝在具型別用戶端內。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-528">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="6e5fb-529">可以提供在內部呼叫 `HttpClient` 執行個體的公用方法，而不將它公開為屬性。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-529">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="6e5fb-530">在上述程式碼，`HttpClient` 儲存為私用欄位。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-530">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="6e5fb-531">進行外部呼叫的所有存取都會經歷 `GetRepos` 方法。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-531">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="6e5fb-532">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="6e5fb-532">Generated clients</span></span>

<span data-ttu-id="6e5fb-533">`IHttpClientFactory` 可和其他協力廠商程式庫一起使用，例如 [Refit](https://github.com/paulcbetts/refit)。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-533">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="6e5fb-534">Refit 是適用於 .NET 的 REST 程式庫。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-534">Refit is a REST library for .NET.</span></span> <span data-ttu-id="6e5fb-535">它將 REST API 轉換為即時介面。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-535">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="6e5fb-536">介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-536">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="6e5fb-537">定義介面及回覆來代表外部 API 和其回應：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-537">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="6e5fb-538">可以新增具型別用戶端，使用 Refit 產生實作：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-538">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="6e5fb-539">定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-539">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="6e5fb-540">外寄要求中介軟體</span><span class="sxs-lookup"><span data-stu-id="6e5fb-540">Outgoing request middleware</span></span>

<span data-ttu-id="6e5fb-541">`HttpClient` 已經有委派可針對外寄 HTTP 要求連結在一起的處理常式的概念。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-541">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="6e5fb-542">`IHttpClientFactory` 可讓您輕鬆地定義要套用於每個具名用戶端的處理常式。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-542">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="6e5fb-543">它支援註冊和鏈結多個處理常式，以建置外寄要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-543">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="6e5fb-544">這些處理常式每個都可以在外寄要求之前和之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-544">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="6e5fb-545">此模式與 ASP.NET Core 中的輸入中介軟體管線相似。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-545">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="6e5fb-546">模式提供一個機制來管理 HTTP 要求的跨領域關注，包括快取、錯誤處理、序列化和記錄。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-546">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="6e5fb-547">若要建立處理常式，請定義衍生自 <xref:System.Net.Http.DelegatingHandler> 的類別。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-547">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="6e5fb-548">覆寫 `SendAsync` 方法，以在將要求傳遞至管線中的下一個處理常式之前執行程式碼：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-548">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="6e5fb-549">上述程式碼定義一個基本處理常式。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-549">The preceding code defines a basic handler.</span></span> <span data-ttu-id="6e5fb-550">它會檢查以查看要求上是否已包含 `X-API-KEY` 標頭。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-550">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="6e5fb-551">如果遺漏標頭，它可以避免 HTTP 呼叫，並傳回適當的回應。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-551">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="6e5fb-552">在註冊期間，可以新增一或多個處理常式至 `HttpClient` 的組態。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-552">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="6e5fb-553">這項工作是透過 <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder> 上的擴充方法完成。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-553">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="6e5fb-554">在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-554">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="6e5fb-555">處理常式**必須**在 DI 中註冊為暫時性服務，無限定範圍。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-555">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="6e5fb-556">如果處理常式已註冊為範圍服務，而且處理常式所相依的任何服務都是可處置的：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-556">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="6e5fb-557">處理常式的服務可能會在處理常式超出範圍之前加以處置。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-557">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="6e5fb-558">已處置的處理常式服務會導致處理常式失敗。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-558">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="6e5fb-559">註冊之後，便可以呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>，並傳入處理常式類型。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-559">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="6e5fb-560">可以遵循應該執行的順序來註冊多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-560">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="6e5fb-561">每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-561">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="6e5fb-562">使用下列其中一種方式來與訊息處理常式共用個別要求狀態：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-562">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="6e5fb-563">使用 `HttpRequestMessage.Properties` 將資料傳遞到處理常式。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-563">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="6e5fb-564">使用 `IHttpContextAccessor` 來存取目前的要求。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-564">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="6e5fb-565">建立自訂 `AsyncLocal` 儲存體物件以傳遞資料。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-565">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="6e5fb-566">使用 Polly 為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="6e5fb-566">Use Polly-based handlers</span></span>

<span data-ttu-id="6e5fb-567">`IHttpClientFactory` 整合受歡迎的協力廠商程式庫，稱為 [Polly](https://github.com/App-vNext/Polly)。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-567">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="6e5fb-568">Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-568">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="6e5fb-569">它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-569">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="6e5fb-570">提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-570">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="6e5fb-571">Polly 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-571">The Polly extensions:</span></span>

* <span data-ttu-id="6e5fb-572">支援將以 Polly 為基礎的處理常式新增至用戶端。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-572">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="6e5fb-573">可在安裝 [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet 套件後使用。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-573">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="6e5fb-574">該套件並未包含在 ASP.NET Core 共用架構中。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-574">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="6e5fb-575">處理暫時性錯誤</span><span class="sxs-lookup"><span data-stu-id="6e5fb-575">Handle transient faults</span></span>

<span data-ttu-id="6e5fb-576">大部分的錯誤發生在外部 HTTP 呼叫是暫時性的時候。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-576">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="6e5fb-577">包含一個便利的擴充方法，稱為 `AddTransientHttpErrorPolicy`，它可允許定義原則來處理暫時性錯誤。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-577">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="6e5fb-578">使用此延伸模組方法設定的原則，會處理 `HttpRequestException`、HTTP 5xx 回應和 HTTP 408 回應。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-578">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="6e5fb-579">`AddTransientHttpErrorPolicy` 延伸模組可用於 `Startup.ConfigureServices` 內。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-579">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="6e5fb-580">延伸模組能提供 `PolicyBuilder` 物件的存取，該物件已設定來處理代表可能暫時性錯誤的錯誤：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-580">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="6e5fb-581">在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-581">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="6e5fb-582">失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-582">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="6e5fb-583">動態選取原則</span><span class="sxs-lookup"><span data-stu-id="6e5fb-583">Dynamically select policies</span></span>

<span data-ttu-id="6e5fb-584">有額外的擴充方法可用來新增 Polly 為基礎的處理常式。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-584">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="6e5fb-585">其中一個這類延伸模組是 `AddPolicyHandler`，它有多個多載。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-585">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="6e5fb-586">一個多載可讓您在定義要套用的原則時，檢查要求：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-586">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="6e5fb-587">在上述程式碼中，如果外寄要求是 HTTP GET，就會套用 10 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-587">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="6e5fb-588">任何其他 HTTP 方法會使用 30 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-588">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="6e5fb-589">新增多個 Polly 處理常式</span><span class="sxs-lookup"><span data-stu-id="6e5fb-589">Add multiple Polly handlers</span></span>

<span data-ttu-id="6e5fb-590">通常會建立巢狀 Polly 原則，以提供增強的功能：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-590">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="6e5fb-591">在上述範例中，會新增兩個處理常式。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-591">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="6e5fb-592">第一個使用 `AddTransientHttpErrorPolicy` 延伸模組來新增重試原則。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-592">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="6e5fb-593">失敗的要求會重試最多三次。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-593">Failed requests are retried up to three times.</span></span> <span data-ttu-id="6e5fb-594">第二個 `AddTransientHttpErrorPolicy` 呼叫會新增斷路器原則。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-594">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="6e5fb-595">如果循序發生五次失敗的嘗試，進一步的外部要求會遭到封鎖 30 秒。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-595">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="6e5fb-596">斷路器原則可設定狀態。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-596">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="6e5fb-597">透過此用戶端的所有呼叫都會共用相同的線路狀態。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-597">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="6e5fb-598">從 Polly 登錄新增原則</span><span class="sxs-lookup"><span data-stu-id="6e5fb-598">Add policies from the Polly registry</span></span>

<span data-ttu-id="6e5fb-599">管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-599">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="6e5fb-600">提供了擴充方法，可以使用來自登錄的原則新增處理常式：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-600">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="6e5fb-601">在上述程式碼中，當 `PolicyRegistry` 新增至 `ServiceCollection` 時，註冊了兩個原則。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-601">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="6e5fb-602">為了使用來自登錄的原則，使用了 `AddPolicyHandlerFromRegistry` 方法，並傳遞要套用的原則名稱。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-602">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="6e5fb-603">關於 `IHttpClientFactory` Polly 整合的詳細資訊，可以在 [Polly Wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory) 上找到。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-603">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="6e5fb-604">HttpClient 和存留期管理</span><span class="sxs-lookup"><span data-stu-id="6e5fb-604">HttpClient and lifetime management</span></span>

<span data-ttu-id="6e5fb-605">每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回新的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-605">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="6e5fb-606">每個具名用戶端都有一個 <xref:System.Net.Http.HttpMessageHandler>。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-606">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="6e5fb-607">處理站會管理 `HttpMessageHandler` 執行個體的存留期。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-607">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="6e5fb-608">`IHttpClientFactory` 會將處理站所建立的 `HttpMessageHandler` 執行個體放入集區以減少資源耗用量。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-608">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="6e5fb-609">建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-609">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="6e5fb-610">將處理常式放入集區非常實用，因為處理常式通常會管理自己專屬的底層 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-610">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="6e5fb-611">建立比所需數目更多的處理常式，可能會導致連線延遲。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-611">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="6e5fb-612">有些處理常式也會保持連線無限期地開啟，這可能導致處理常式無法對 DNS 變更回應。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-612">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="6e5fb-613">預設處理常式存留時間為兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-613">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="6e5fb-614">可以針對每個具名用戶端覆寫預設值。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-614">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="6e5fb-615">若要覆寫它，請在建立用戶端時所傳回的 `IHttpClientBuilder` 上呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*>：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-615">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="6e5fb-616">不需要處置用戶端。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-616">Disposal of the client isn't required.</span></span> <span data-ttu-id="6e5fb-617">處置會取消傳出的要求，並保證指定的 `HttpClient` 執行個體在呼叫 <xref:System.IDisposable.Dispose*> 之後無法使用。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-617">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="6e5fb-618">`IHttpClientFactory` 會追蹤並處置 `HttpClient` 執行個體使用的資源。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-618">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="6e5fb-619">`HttpClient` 執行個體通常可視為 .NET 物件，不需要處置。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-619">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="6e5fb-620">在開始使用 `IHttpClientFactory` 之前，讓單一 `HttpClient` 執行個體維持一段較長的時間，是很常使用的模式。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-620">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="6e5fb-621">在移轉到 `IHttpClientFactory` 之後，就不再需要此模式。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-621">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="6e5fb-622">記錄</span><span class="sxs-lookup"><span data-stu-id="6e5fb-622">Logging</span></span>

<span data-ttu-id="6e5fb-623">透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-623">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="6e5fb-624">在記錄設定中啟用適當的資訊層級，以查看預設記錄檔訊息。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-624">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="6e5fb-625">額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-625">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="6e5fb-626">用於每個用戶端的記錄檔分類包含用戶端的名稱。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-626">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="6e5fb-627">例如，名為 *MyNamedClient* 的用戶端會記錄分類為 `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler` 的訊息。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-627">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="6e5fb-628">後面加上 *LogicalHandler* 的訊息發生在要求處理常式管線之外。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-628">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="6e5fb-629">在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-629">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="6e5fb-630">在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-630">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="6e5fb-631">記錄也會發生在要求處理常式管線之內。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-631">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="6e5fb-632">在 *MyNamedClient* 範例中，那些訊息是針對記錄檔分類 `System.Net.Http.HttpClient.MyNamedClient.ClientHandler` 而記錄。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-632">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="6e5fb-633">對於要求，這是發生在所有其他處理常式都已執行之後，並且緊接在網路上傳送要求之前。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-633">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="6e5fb-634">在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-634">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="6e5fb-635">在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-635">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="6e5fb-636">例如，這可能包括要求標頭的變更，或是回應狀態碼的變更。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-636">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="6e5fb-637">在記錄分類中包含用戶端的名稱，可讓您在需要時進行特定具名用戶端的記錄檔篩選。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-637">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="6e5fb-638">設定 HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="6e5fb-638">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="6e5fb-639">可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-639">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="6e5fb-640">新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-640">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="6e5fb-641"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 擴充方法可以用來定義委派。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-641">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="6e5fb-642">委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-642">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="6e5fb-643">在主控台應用程式中使用 IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="6e5fb-643">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="6e5fb-644">在主控台應用程式中，將下列套件參考新增至專案：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-644">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="6e5fb-645">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="6e5fb-645">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="6e5fb-646">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="6e5fb-646">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="6e5fb-647">在以下範例中：</span><span class="sxs-lookup"><span data-stu-id="6e5fb-647">In the following example:</span></span>

* <span data-ttu-id="6e5fb-648"><xref:System.Net.Http.IHttpClientFactory> 已在[泛型主機的](xref:fundamentals/host/generic-host)服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-648"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="6e5fb-649">`MyService` 會從服務建立用戶端 Factory 執行個體，其可用來建立 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-649">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="6e5fb-650">`HttpClient` 會用來擷取網頁。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-650">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="6e5fb-651">`Main` 會建立範圍來執行服務的 `GetPage` 方法，並將網頁內容的前 500 個字元寫入至主控台。</span><span class="sxs-lookup"><span data-stu-id="6e5fb-651">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="6e5fb-652">其他資源</span><span class="sxs-lookup"><span data-stu-id="6e5fb-652">Additional resources</span></span>

* [<span data-ttu-id="6e5fb-653">使用 HttpClientFactory 實作復原 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="6e5fb-653">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="6e5fb-654">使用 HttpClientFactory 和 Polly 原則以指數輪詢實作 HTTP 呼叫重試</span><span class="sxs-lookup"><span data-stu-id="6e5fb-654">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="6e5fb-655">實作斷路器模式</span><span class="sxs-lookup"><span data-stu-id="6e5fb-655">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
