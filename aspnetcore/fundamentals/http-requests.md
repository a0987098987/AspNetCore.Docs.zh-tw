---
title: 在 ASP.NET Core 中使用 IHttpClientFactory 發出 HTTP 要求
author: stevejgordon
description: 深入了解在 ASP.NET Core 中使用 IHttpClientFactory 介面來管理邏輯 HttpClient 執行個體。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 12/16/2019
uid: fundamentals/http-requests
ms.openlocfilehash: f2494a5815396e693f6fd2a45ad78ebffe4d54a3
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/25/2019
ms.locfileid: "75358078"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="912c8-103">在 ASP.NET Core 中使用 IHttpClientFactory 發出 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="912c8-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="912c8-104">作者： [Glenn Condron](https://github.com/glennc)、 [Ryan Nowak](https://github.com/rynowak)、 [Steve Gordon](https://github.com/stevejgordon)、 [Rick Anderson](https://twitter.com/RickAndMSFT)和[Kirk Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="912c8-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="912c8-105"><xref:System.Net.Http.IHttpClientFactory> 可以註冊及用來在應用程式中設定和建立 <xref:System.Net.Http.HttpClient> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="912c8-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="912c8-106">`IHttpClientFactory` 提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="912c8-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="912c8-107">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="912c8-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="912c8-108">例如，名為*github*的用戶端可以註冊並設定為存取[github](https://github.com/)。</span><span class="sxs-lookup"><span data-stu-id="912c8-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="912c8-109">預設用戶端可以註冊以進行一般存取。</span><span class="sxs-lookup"><span data-stu-id="912c8-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="912c8-110">透過 `HttpClient`中的委派處理常式，制訂外寄中介軟體的概念。</span><span class="sxs-lookup"><span data-stu-id="912c8-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="912c8-111">提供 Polly 為基礎中介軟體的延伸模組，以利用 `HttpClient`中的委派處理常式。</span><span class="sxs-lookup"><span data-stu-id="912c8-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="912c8-112">管理基礎 `HttpClientMessageHandler` 實例的共用和存留期。</span><span class="sxs-lookup"><span data-stu-id="912c8-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="912c8-113">自動管理可避免在手動管理 `HttpClient` 存留期時所發生的常見 DNS （網域名稱系統）問題。</span><span class="sxs-lookup"><span data-stu-id="912c8-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="912c8-114">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="912c8-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="912c8-115">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="912c8-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="912c8-116">本主題中的範例程式碼會使用 <xref:System.Text.Json> 來還原序列化 HTTP 回應中所傳回的 JSON 內容。</span><span class="sxs-lookup"><span data-stu-id="912c8-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="912c8-117">如需使用 `Json.NET` 和 `ReadAsAsync<T>`的範例，請使用版本選取器來選取此主題的2.x 版。</span><span class="sxs-lookup"><span data-stu-id="912c8-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="912c8-118">耗用模式</span><span class="sxs-lookup"><span data-stu-id="912c8-118">Consumption patterns</span></span>

<span data-ttu-id="912c8-119">有數種方式可將 `IHttpClientFactory` 用於應用程式：</span><span class="sxs-lookup"><span data-stu-id="912c8-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="912c8-120">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="912c8-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="912c8-121">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="912c8-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="912c8-122">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="912c8-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="912c8-123">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="912c8-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="912c8-124">最佳方法取決於應用程式的需求。</span><span class="sxs-lookup"><span data-stu-id="912c8-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="912c8-125">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="912c8-125">Basic usage</span></span>

<span data-ttu-id="912c8-126">`IHttpClientFactory` 可以藉由呼叫 `AddHttpClient`進行註冊：</span><span class="sxs-lookup"><span data-stu-id="912c8-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="912c8-127">`IHttpClientFactory` 可以使用相依性[插入（DI）](xref:fundamentals/dependency-injection)來要求。</span><span class="sxs-lookup"><span data-stu-id="912c8-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="912c8-128">下列程式碼會使用 `IHttpClientFactory` 來建立 `HttpClient` 實例：</span><span class="sxs-lookup"><span data-stu-id="912c8-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="912c8-129">使用上述範例中的 `IHttpClientFactory` 就是重構現有應用程式的好方法。</span><span class="sxs-lookup"><span data-stu-id="912c8-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="912c8-130">這不會影響使用 `HttpClient` 的方式。</span><span class="sxs-lookup"><span data-stu-id="912c8-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="912c8-131">在現有應用程式中建立 `HttpClient` 實例的位置，使用 <xref:System.Net.Http.IHttpClientFactory.CreateClient*>的呼叫來取代這些專案。</span><span class="sxs-lookup"><span data-stu-id="912c8-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="912c8-132">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="912c8-132">Named clients</span></span>

<span data-ttu-id="912c8-133">在下列情況中，命名的用戶端是不錯的選擇：</span><span class="sxs-lookup"><span data-stu-id="912c8-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="912c8-134">應用程式需要 `HttpClient`的許多不同用途。</span><span class="sxs-lookup"><span data-stu-id="912c8-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="912c8-135">許多 `HttpClient`都有不同的設定。</span><span class="sxs-lookup"><span data-stu-id="912c8-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="912c8-136">在 `Startup.ConfigureServices`註冊期間，可以指定已命名 `HttpClient` 的設定：</span><span class="sxs-lookup"><span data-stu-id="912c8-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="912c8-137">在上述程式碼中，用戶端是使用下列設定：</span><span class="sxs-lookup"><span data-stu-id="912c8-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="912c8-138">基底位址 `https://api.github.com/`。</span><span class="sxs-lookup"><span data-stu-id="912c8-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="912c8-139">使用 GitHub API 時需要兩個標頭。</span><span class="sxs-lookup"><span data-stu-id="912c8-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="912c8-140">CreateClient</span><span class="sxs-lookup"><span data-stu-id="912c8-140">CreateClient</span></span>

<span data-ttu-id="912c8-141">每次呼叫 <xref:System.Net.Http.IHttpClientFactory.CreateClient*> 時：</span><span class="sxs-lookup"><span data-stu-id="912c8-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="912c8-142">建立 `HttpClient` 的新實例。</span><span class="sxs-lookup"><span data-stu-id="912c8-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="912c8-143">會呼叫設定動作。</span><span class="sxs-lookup"><span data-stu-id="912c8-143">The configuration action is called.</span></span>

<span data-ttu-id="912c8-144">若要建立名為的用戶端，請將其名稱傳遞至 `CreateClient`：</span><span class="sxs-lookup"><span data-stu-id="912c8-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="912c8-145">在上述程式碼中，要求不需要指定主機名稱。</span><span class="sxs-lookup"><span data-stu-id="912c8-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="912c8-146">此程式碼只會傳遞路徑，因為會使用為用戶端設定的基底位址。</span><span class="sxs-lookup"><span data-stu-id="912c8-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="912c8-147">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="912c8-147">Typed clients</span></span>

<span data-ttu-id="912c8-148">具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="912c8-148">Typed clients:</span></span>

* <span data-ttu-id="912c8-149">提供與具名用戶端相同的功能，而不需使用字串作為索引鍵。</span><span class="sxs-lookup"><span data-stu-id="912c8-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="912c8-150">取用用戶端時提供 IntelliSense 和編譯器說明。</span><span class="sxs-lookup"><span data-stu-id="912c8-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="912c8-151">提供單一位置來設定特定的 `HttpClient` 並與其互動。</span><span class="sxs-lookup"><span data-stu-id="912c8-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="912c8-152">例如，可能會使用單一具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="912c8-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="912c8-153">適用于單一後端端點。</span><span class="sxs-lookup"><span data-stu-id="912c8-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="912c8-154">封裝處理端點的所有邏輯。</span><span class="sxs-lookup"><span data-stu-id="912c8-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="912c8-155">使用 DI，並可在應用程式中需要的位置插入。</span><span class="sxs-lookup"><span data-stu-id="912c8-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="912c8-156">具型別用戶端會接受其程式化中的 `HttpClient` 參數：</span><span class="sxs-lookup"><span data-stu-id="912c8-156">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="912c8-157">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="912c8-157">In the preceding code:</span></span>

* <span data-ttu-id="912c8-158">設定會移到具型別用戶端。</span><span class="sxs-lookup"><span data-stu-id="912c8-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="912c8-159">`HttpClient` 物件會公開為公用屬性。</span><span class="sxs-lookup"><span data-stu-id="912c8-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="912c8-160">可以建立可公開 `HttpClient` 功能的 API 特定方法。</span><span class="sxs-lookup"><span data-stu-id="912c8-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="912c8-161">例如，`GetAspNetDocsIssues` 方法會封裝程式碼以取得未解決的問題。</span><span class="sxs-lookup"><span data-stu-id="912c8-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="912c8-162">下列程式碼會呼叫 `Startup.ConfigureServices` 中的 <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> 來註冊具類型的用戶端類別：</span><span class="sxs-lookup"><span data-stu-id="912c8-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="912c8-163">具型別用戶端會向 DI 註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="912c8-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="912c8-164">具型別用戶端可以直接插入並使用：</span><span class="sxs-lookup"><span data-stu-id="912c8-164">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="912c8-165">型別用戶端的設定可以在 `Startup.ConfigureServices`註冊期間指定，而不是在具型別用戶端的函式中：</span><span class="sxs-lookup"><span data-stu-id="912c8-165">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="912c8-166">`HttpClient` 可以封裝在具型別用戶端中。</span><span class="sxs-lookup"><span data-stu-id="912c8-166">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="912c8-167">請定義在內部呼叫 `HttpClient` 實例的方法，而不是將它公開為屬性：</span><span class="sxs-lookup"><span data-stu-id="912c8-167">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="912c8-168">在上述程式碼中，`HttpClient` 儲存在私用欄位中。</span><span class="sxs-lookup"><span data-stu-id="912c8-168">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="912c8-169">`HttpClient` 的存取權是公用 `GetRepos` 方法。</span><span class="sxs-lookup"><span data-stu-id="912c8-169">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="912c8-170">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="912c8-170">Generated clients</span></span>

<span data-ttu-id="912c8-171">`IHttpClientFactory` 可以與協力廠商程式庫搭配使用，例如重新[調整。](https://github.com/paulcbetts/refit)</span><span class="sxs-lookup"><span data-stu-id="912c8-171">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="912c8-172">Refit 是適用於 .NET 的 REST 程式庫。</span><span class="sxs-lookup"><span data-stu-id="912c8-172">Refit is a REST library for .NET.</span></span> <span data-ttu-id="912c8-173">它將 REST API 轉換為即時介面。</span><span class="sxs-lookup"><span data-stu-id="912c8-173">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="912c8-174">介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。</span><span class="sxs-lookup"><span data-stu-id="912c8-174">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="912c8-175">定義介面及回覆來代表外部 API 和其回應：</span><span class="sxs-lookup"><span data-stu-id="912c8-175">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="912c8-176">可以新增具型別用戶端，使用 Refit 產生實作：</span><span class="sxs-lookup"><span data-stu-id="912c8-176">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="912c8-177">定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：</span><span class="sxs-lookup"><span data-stu-id="912c8-177">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="912c8-178">外寄要求中介軟體</span><span class="sxs-lookup"><span data-stu-id="912c8-178">Outgoing request middleware</span></span>

<span data-ttu-id="912c8-179">`HttpClient` 具有委派處理常式的概念，可以針對傳出 HTTP 要求連結在一起。</span><span class="sxs-lookup"><span data-stu-id="912c8-179">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="912c8-180">`IHttpClientFactory`：</span><span class="sxs-lookup"><span data-stu-id="912c8-180">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="912c8-181">簡化定義要套用至每個已命名用戶端的處理常式。</span><span class="sxs-lookup"><span data-stu-id="912c8-181">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="912c8-182">支援多個處理常式的註冊和連結，以建立外寄要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="912c8-182">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="912c8-183">這些處理常式每個都可以在外寄要求之前和之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="912c8-183">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="912c8-184">此模式：</span><span class="sxs-lookup"><span data-stu-id="912c8-184">This pattern:</span></span>

  * <span data-ttu-id="912c8-185">類似 ASP.NET Core 中的輸入中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="912c8-185">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="912c8-186">提供一種機制來管理 HTTP 要求的跨領域考慮，例如：</span><span class="sxs-lookup"><span data-stu-id="912c8-186">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="912c8-187">快取</span><span class="sxs-lookup"><span data-stu-id="912c8-187">caching</span></span>
    * <span data-ttu-id="912c8-188">錯誤處理</span><span class="sxs-lookup"><span data-stu-id="912c8-188">error handling</span></span>
    * <span data-ttu-id="912c8-189">serialization</span><span class="sxs-lookup"><span data-stu-id="912c8-189">serialization</span></span>
    * <span data-ttu-id="912c8-190">記錄</span><span class="sxs-lookup"><span data-stu-id="912c8-190">logging</span></span>

<span data-ttu-id="912c8-191">若要建立委派處理常式：</span><span class="sxs-lookup"><span data-stu-id="912c8-191">To create a delegating handler:</span></span>

* <span data-ttu-id="912c8-192">衍生自 <xref:System.Net.Http.DelegatingHandler>。</span><span class="sxs-lookup"><span data-stu-id="912c8-192">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="912c8-193">覆寫 <xref:System.Net.Http.DelegatingHandler.SendAsync*>。</span><span class="sxs-lookup"><span data-stu-id="912c8-193">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="912c8-194">先執行程式碼，再將要求傳遞至管線中的下一個處理常式：</span><span class="sxs-lookup"><span data-stu-id="912c8-194">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="912c8-195">上述程式碼會檢查 `X-API-KEY` 標頭是否在要求中。</span><span class="sxs-lookup"><span data-stu-id="912c8-195">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="912c8-196">如果遺漏 `X-API-KEY`，則會傳回 <xref:System.Net.HttpStatusCode.BadRequest>。</span><span class="sxs-lookup"><span data-stu-id="912c8-196">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="912c8-197">可以將一個以上的處理常式新增至具有 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>之 `HttpClient` 的設定：</span><span class="sxs-lookup"><span data-stu-id="912c8-197">More than one handler can be added to the configuration for an `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="912c8-198">在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。</span><span class="sxs-lookup"><span data-stu-id="912c8-198">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="912c8-199">`IHttpClientFactory` 會為每個處理常式建立個別的 DI 範圍。</span><span class="sxs-lookup"><span data-stu-id="912c8-199">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="912c8-200">處理常式可以相依于任何範圍的服務。</span><span class="sxs-lookup"><span data-stu-id="912c8-200">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="912c8-201">處置處理常式時，會處置處理常式所相依的服務。</span><span class="sxs-lookup"><span data-stu-id="912c8-201">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="912c8-202">註冊之後，便可以呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>，並傳入處理常式的類型。</span><span class="sxs-lookup"><span data-stu-id="912c8-202">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="912c8-203">可以遵循應該執行的順序來註冊多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="912c8-203">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="912c8-204">每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：</span><span class="sxs-lookup"><span data-stu-id="912c8-204">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="912c8-205">使用下列其中一種方式來與訊息處理常式共用個別要求狀態：</span><span class="sxs-lookup"><span data-stu-id="912c8-205">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="912c8-206">使用[HttpRequestMessage](xref:System.Net.Http.HttpRequestMessage.Properties)將資料傳遞至處理常式。</span><span class="sxs-lookup"><span data-stu-id="912c8-206">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="912c8-207">使用 <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> 來存取目前的要求。</span><span class="sxs-lookup"><span data-stu-id="912c8-207">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="912c8-208">建立自訂 <xref:System.Threading.AsyncLocal`1> 儲存體物件以傳遞資料。</span><span class="sxs-lookup"><span data-stu-id="912c8-208">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="912c8-209">使用 Polly 為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="912c8-209">Use Polly-based handlers</span></span>

<span data-ttu-id="912c8-210">`IHttpClientFactory` 與協力廠商程式庫[Polly](https://github.com/App-vNext/Polly)整合。</span><span class="sxs-lookup"><span data-stu-id="912c8-210">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="912c8-211">Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。</span><span class="sxs-lookup"><span data-stu-id="912c8-211">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="912c8-212">它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。</span><span class="sxs-lookup"><span data-stu-id="912c8-212">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="912c8-213">提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="912c8-213">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="912c8-214">Polly 擴充功能支援將以 Polly 為基礎的處理常式新增至用戶端。</span><span class="sxs-lookup"><span data-stu-id="912c8-214">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="912c8-215">Polly 需要[Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="912c8-215">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="912c8-216">處理暫時性錯誤</span><span class="sxs-lookup"><span data-stu-id="912c8-216">Handle transient faults</span></span>

<span data-ttu-id="912c8-217">當外部 HTTP 呼叫是暫時性的時，通常會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="912c8-217">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="912c8-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> 允許定義原則來處理暫時性錯誤。</span><span class="sxs-lookup"><span data-stu-id="912c8-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="912c8-219">以 `AddTransientHttpErrorPolicy` 設定的原則會處理下列回應：</span><span class="sxs-lookup"><span data-stu-id="912c8-219">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="912c8-220">HTTP 5xx</span><span class="sxs-lookup"><span data-stu-id="912c8-220">HTTP 5xx</span></span>
* <span data-ttu-id="912c8-221">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="912c8-221">HTTP 408</span></span>

<span data-ttu-id="912c8-222">`AddTransientHttpErrorPolicy` 提供 `PolicyBuilder` 物件的存取權，其設定為處理代表可能暫時性錯誤的錯誤：</span><span class="sxs-lookup"><span data-stu-id="912c8-222">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="912c8-223">在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。</span><span class="sxs-lookup"><span data-stu-id="912c8-223">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="912c8-224">失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。</span><span class="sxs-lookup"><span data-stu-id="912c8-224">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="912c8-225">動態選取原則</span><span class="sxs-lookup"><span data-stu-id="912c8-225">Dynamically select policies</span></span>

<span data-ttu-id="912c8-226">提供擴充方法來加入以 Polly 為基礎的處理常式，例如 <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>。</span><span class="sxs-lookup"><span data-stu-id="912c8-226">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="912c8-227">下列 `AddPolicyHandler` 多載會檢查要求以決定要套用的原則：</span><span class="sxs-lookup"><span data-stu-id="912c8-227">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="912c8-228">在上述程式碼中，如果外寄要求是 HTTP GET，就會套用 10 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="912c8-228">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="912c8-229">任何其他 HTTP 方法會使用 30 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="912c8-229">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="912c8-230">新增多個 Polly 處理常式</span><span class="sxs-lookup"><span data-stu-id="912c8-230">Add multiple Polly handlers</span></span>

<span data-ttu-id="912c8-231">通常會將 Polly 原則加以嵌套：</span><span class="sxs-lookup"><span data-stu-id="912c8-231">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="912c8-232">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="912c8-232">In the preceding example:</span></span>

* <span data-ttu-id="912c8-233">新增了兩個處理常式。</span><span class="sxs-lookup"><span data-stu-id="912c8-233">Two handlers are added.</span></span>
* <span data-ttu-id="912c8-234">第一個處理常式會使用 <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> 來新增重試原則。</span><span class="sxs-lookup"><span data-stu-id="912c8-234">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="912c8-235">失敗的要求會重試最多三次。</span><span class="sxs-lookup"><span data-stu-id="912c8-235">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="912c8-236">第二個 `AddTransientHttpErrorPolicy` 呼叫會增加斷路器原則。</span><span class="sxs-lookup"><span data-stu-id="912c8-236">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="912c8-237">如果連續5次嘗試失敗，則會封鎖進一步的外部要求30秒。</span><span class="sxs-lookup"><span data-stu-id="912c8-237">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="912c8-238">斷路器原則可設定狀態。</span><span class="sxs-lookup"><span data-stu-id="912c8-238">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="912c8-239">透過此用戶端的所有呼叫都會共用相同的線路狀態。</span><span class="sxs-lookup"><span data-stu-id="912c8-239">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="912c8-240">從 Polly 登錄新增原則</span><span class="sxs-lookup"><span data-stu-id="912c8-240">Add policies from the Polly registry</span></span>

<span data-ttu-id="912c8-241">管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。</span><span class="sxs-lookup"><span data-stu-id="912c8-241">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="912c8-242">在下列程式碼中：</span><span class="sxs-lookup"><span data-stu-id="912c8-242">In the following code:</span></span>

* <span data-ttu-id="912c8-243">系統會新增「一般」和「長」原則。</span><span class="sxs-lookup"><span data-stu-id="912c8-243">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="912c8-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*> 從登錄新增「一般」和「長」原則。</span><span class="sxs-lookup"><span data-stu-id="912c8-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="912c8-245">如需 `IHttpClientFactory` 和 Polly 整合的詳細資訊，請參閱[Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory)。</span><span class="sxs-lookup"><span data-stu-id="912c8-245">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="912c8-246">HttpClient 和存留期管理</span><span class="sxs-lookup"><span data-stu-id="912c8-246">HttpClient and lifetime management</span></span>

<span data-ttu-id="912c8-247">每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回新的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="912c8-247">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="912c8-248">系統會根據每個命名的用戶端來建立 <xref:System.Net.Http.HttpMessageHandler>。</span><span class="sxs-lookup"><span data-stu-id="912c8-248">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="912c8-249">處理站會管理 `HttpMessageHandler` 執行個體的存留期。</span><span class="sxs-lookup"><span data-stu-id="912c8-249">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="912c8-250">`IHttpClientFactory` 會將處理站所建立的 `HttpMessageHandler` 執行個體放入集區以減少資源耗用量。</span><span class="sxs-lookup"><span data-stu-id="912c8-250">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="912c8-251">建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。</span><span class="sxs-lookup"><span data-stu-id="912c8-251">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="912c8-252">將處理常式放入集區非常實用，因為處理常式通常會管理自己專屬的底層 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="912c8-252">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="912c8-253">建立比所需數目更多的處理常式，可能會導致連線延遲。</span><span class="sxs-lookup"><span data-stu-id="912c8-253">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="912c8-254">有些處理常式也會保持連線無限期地開啟，這可能導致處理常式無法回應 DNS （網域名稱系統）變更。</span><span class="sxs-lookup"><span data-stu-id="912c8-254">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="912c8-255">預設處理常式存留時間為兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="912c8-255">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="912c8-256">預設值可以根據每個命名的用戶端來覆寫：</span><span class="sxs-lookup"><span data-stu-id="912c8-256">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="912c8-257">`HttpClient` 實例通常可視為**不**需要處置的 .net 物件。</span><span class="sxs-lookup"><span data-stu-id="912c8-257">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="912c8-258">處置會取消傳出的要求，並保證指定的 `HttpClient` 執行個體在呼叫 <xref:System.IDisposable.Dispose*> 之後無法使用。</span><span class="sxs-lookup"><span data-stu-id="912c8-258">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="912c8-259">`IHttpClientFactory` 會追蹤並處置 `HttpClient` 執行個體使用的資源。</span><span class="sxs-lookup"><span data-stu-id="912c8-259">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="912c8-260">在開始使用 `IHttpClientFactory` 之前，讓單一 `HttpClient` 執行個體維持一段較長的時間，是很常使用的模式。</span><span class="sxs-lookup"><span data-stu-id="912c8-260">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="912c8-261">在移轉到 `IHttpClientFactory` 之後，就不再需要此模式。</span><span class="sxs-lookup"><span data-stu-id="912c8-261">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="912c8-262">IHttpClientFactory 的替代方案</span><span class="sxs-lookup"><span data-stu-id="912c8-262">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="912c8-263">在啟用 DI 的應用程式中使用 `IHttpClientFactory` 可避免：</span><span class="sxs-lookup"><span data-stu-id="912c8-263">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="912c8-264">藉由共用 `HttpMessageHandler` 實例的資源耗盡問題。</span><span class="sxs-lookup"><span data-stu-id="912c8-264">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="912c8-265">以固定間隔迴圈 `HttpMessageHandler` 實例的過時 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="912c8-265">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="912c8-266">有其他方法可以使用長期 <xref:System.Net.Http.SocketsHttpHandler> 實例來解決上述問題。</span><span class="sxs-lookup"><span data-stu-id="912c8-266">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="912c8-267">當應用程式啟動時，建立 `SocketsHttpHandler` 的實例，並在應用程式的生命週期中使用它。</span><span class="sxs-lookup"><span data-stu-id="912c8-267">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="912c8-268">根據 DNS 重新整理時間，將 <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> 設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="912c8-268">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="912c8-269">視需要使用 `new HttpClient(handler, dispostHandler: false)` 建立 `HttpClient` 實例。</span><span class="sxs-lookup"><span data-stu-id="912c8-269">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="912c8-270">上述方法可解決 `IHttpClientFactory` 以類似的方式解決的資源管理問題。</span><span class="sxs-lookup"><span data-stu-id="912c8-270">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="912c8-271">`SocketsHttpHandler` 會共用 `HttpClient` 實例間的連接。</span><span class="sxs-lookup"><span data-stu-id="912c8-271">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="912c8-272">此共用可防止通訊端耗盡。</span><span class="sxs-lookup"><span data-stu-id="912c8-272">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="912c8-273">`SocketsHttpHandler` 會根據 `PooledConnectionLifetime` 迴圈連接，以避免過時的 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="912c8-273">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="912c8-274">Cookies</span><span class="sxs-lookup"><span data-stu-id="912c8-274">Cookies</span></span>

<span data-ttu-id="912c8-275">集區 `HttpMessageHandler` 實例會導致共用 `CookieContainer` 物件。</span><span class="sxs-lookup"><span data-stu-id="912c8-275">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="912c8-276">意外的 `CookieContainer` 物件共用通常會導致不正確的程式碼。</span><span class="sxs-lookup"><span data-stu-id="912c8-276">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="912c8-277">針對需要 cookie 的應用程式，請考慮下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="912c8-277">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="912c8-278">停用自動 cookie 處理</span><span class="sxs-lookup"><span data-stu-id="912c8-278">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="912c8-279">避免 `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="912c8-279">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="912c8-280">呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 以停用自動的 cookie 處理：</span><span class="sxs-lookup"><span data-stu-id="912c8-280">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="912c8-281">記錄</span><span class="sxs-lookup"><span data-stu-id="912c8-281">Logging</span></span>

<span data-ttu-id="912c8-282">透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="912c8-282">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="912c8-283">在記錄設定中啟用適當的資訊層級，以查看預設記錄檔訊息。</span><span class="sxs-lookup"><span data-stu-id="912c8-283">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="912c8-284">額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。</span><span class="sxs-lookup"><span data-stu-id="912c8-284">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="912c8-285">用於每個用戶端的記錄檔分類包含用戶端的名稱。</span><span class="sxs-lookup"><span data-stu-id="912c8-285">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="912c8-286">例如，名為*MyNamedClient*的用戶端會記錄類別為 "HttpClient" 的訊息。**MyNamedClient**。LogicalHandler "。</span><span class="sxs-lookup"><span data-stu-id="912c8-286">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="912c8-287">後面加上 *LogicalHandler* 的訊息發生在要求處理常式管線之外。</span><span class="sxs-lookup"><span data-stu-id="912c8-287">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="912c8-288">在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。</span><span class="sxs-lookup"><span data-stu-id="912c8-288">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="912c8-289">在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。</span><span class="sxs-lookup"><span data-stu-id="912c8-289">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="912c8-290">記錄也會發生在要求處理常式管線之內。</span><span class="sxs-lookup"><span data-stu-id="912c8-290">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="912c8-291">在*MyNamedClient*範例中，這些訊息會記錄在記錄檔類別中為 "HttpClient"。**MyNamedClient**。ClientHandler".</span><span class="sxs-lookup"><span data-stu-id="912c8-291">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="912c8-292">對於要求，這會在所有其他處理常式都執行之後，且在傳送要求之前立即發生。</span><span class="sxs-lookup"><span data-stu-id="912c8-292">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="912c8-293">在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。</span><span class="sxs-lookup"><span data-stu-id="912c8-293">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="912c8-294">在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。</span><span class="sxs-lookup"><span data-stu-id="912c8-294">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="912c8-295">這可能包括要求標頭的變更或回應狀態碼。</span><span class="sxs-lookup"><span data-stu-id="912c8-295">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="912c8-296">在記錄類別中包含用戶端的名稱，可以針對特定的已命名用戶端進行記錄篩選。</span><span class="sxs-lookup"><span data-stu-id="912c8-296">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="912c8-297">設定 HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="912c8-297">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="912c8-298">可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。</span><span class="sxs-lookup"><span data-stu-id="912c8-298">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="912c8-299">新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。</span><span class="sxs-lookup"><span data-stu-id="912c8-299">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="912c8-300"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 擴充方法可以用來定義委派。</span><span class="sxs-lookup"><span data-stu-id="912c8-300">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="912c8-301">委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：</span><span class="sxs-lookup"><span data-stu-id="912c8-301">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="912c8-302">在主控台應用程式中使用 IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="912c8-302">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="912c8-303">在主控台應用程式中，將下列套件參考新增至專案：</span><span class="sxs-lookup"><span data-stu-id="912c8-303">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="912c8-304">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="912c8-304">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="912c8-305">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="912c8-305">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="912c8-306">在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="912c8-306">In the following example:</span></span>

* <span data-ttu-id="912c8-307"><xref:System.Net.Http.IHttpClientFactory> 已在[泛型主機的](xref:fundamentals/host/generic-host)服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="912c8-307"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="912c8-308">`MyService` 會從服務建立用戶端 Factory 執行個體，其可用來建立 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="912c8-308">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="912c8-309">`HttpClient` 會用來擷取網頁。</span><span class="sxs-lookup"><span data-stu-id="912c8-309">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="912c8-310">`Main` 會建立範圍來執行服務的 `GetPage` 方法，並將網頁內容的前 500 個字元寫入至主控台。</span><span class="sxs-lookup"><span data-stu-id="912c8-310">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="912c8-311">其他資源</span><span class="sxs-lookup"><span data-stu-id="912c8-311">Additional resources</span></span>

* [<span data-ttu-id="912c8-312">使用 HttpClientFactory 實作復原 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="912c8-312">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="912c8-313">使用 HttpClientFactory 和 Polly 原則以指數輪詢實作 HTTP 呼叫重試</span><span class="sxs-lookup"><span data-stu-id="912c8-313">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="912c8-314">實作斷路器模式</span><span class="sxs-lookup"><span data-stu-id="912c8-314">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)
* [<span data-ttu-id="912c8-315">如何在 .NET 中序列化和還原序列化 JSON</span><span class="sxs-lookup"><span data-stu-id="912c8-315">How to serialize and deserialize JSON in .NET</span></span>](/dotnet/standard/serialization/system-text-json-how-to)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="912c8-316">作者：[Glenn Condron](https://github.com/glennc)、[Ryan Nowak](https://github.com/rynowak) 和 [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="912c8-316">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="912c8-317"><xref:System.Net.Http.IHttpClientFactory> 可以註冊及用來在應用程式中設定和建立 <xref:System.Net.Http.HttpClient> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="912c8-317">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="912c8-318">它提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="912c8-318">It offers the following benefits:</span></span>

* <span data-ttu-id="912c8-319">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="912c8-319">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="912c8-320">例如，可註冊及設定 *github* 用戶端，來存取 [GitHub](https://github.com/)。</span><span class="sxs-lookup"><span data-stu-id="912c8-320">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="912c8-321">預設用戶端可以註冊用於其他用途。</span><span class="sxs-lookup"><span data-stu-id="912c8-321">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="912c8-322">透過委派 `HttpClient` 中的處理常式來撰寫外寄中介軟體的概念，並提供延伸模組以便 Polly 架構中介軟體利用外寄中介軟體。</span><span class="sxs-lookup"><span data-stu-id="912c8-322">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="912c8-323">管理基礎 `HttpClientMessageHandler` 執行個體的共用和存留期，以避免在手動管理 `HttpClient` 存留期時，發生的常見 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="912c8-323">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="912c8-324">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="912c8-324">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="912c8-325">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="912c8-325">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="912c8-326">耗用模式</span><span class="sxs-lookup"><span data-stu-id="912c8-326">Consumption patterns</span></span>

<span data-ttu-id="912c8-327">有數種方式可將 `IHttpClientFactory` 用於應用程式：</span><span class="sxs-lookup"><span data-stu-id="912c8-327">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="912c8-328">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="912c8-328">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="912c8-329">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="912c8-329">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="912c8-330">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="912c8-330">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="912c8-331">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="912c8-331">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="912c8-332">它們全都不會嚴格優先於另一個。</span><span class="sxs-lookup"><span data-stu-id="912c8-332">None of them are strictly superior to another.</span></span> <span data-ttu-id="912c8-333">最好的方法取決於應用程式的條件約束。</span><span class="sxs-lookup"><span data-stu-id="912c8-333">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="912c8-334">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="912c8-334">Basic usage</span></span>

<span data-ttu-id="912c8-335">`IHttpClientFactory` 可以藉由在 `Startup.ConfigureServices` 方法內的 `IServiceCollection` 上呼叫 `AddHttpClient` 擴充方法來註冊。</span><span class="sxs-lookup"><span data-stu-id="912c8-335">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="912c8-336">註冊之後，程式碼可以在可使用[相依性插入 (DI)](xref:fundamentals/dependency-injection) 插入服務的任何位置，接受 `IHttpClientFactory`。</span><span class="sxs-lookup"><span data-stu-id="912c8-336">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="912c8-337">`IHttpClientFactory` 可以用來建立 `HttpClient` 實例：</span><span class="sxs-lookup"><span data-stu-id="912c8-337">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="912c8-338">以這種方式使用 `IHttpClientFactory` 是重構現有應用程式的好方法。</span><span class="sxs-lookup"><span data-stu-id="912c8-338">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="912c8-339">它對 `HttpClient` 的使用方式沒有任何影響。</span><span class="sxs-lookup"><span data-stu-id="912c8-339">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="912c8-340">在目前建立 `HttpClient` 執行個體的位置，將那些項目取代為呼叫 <xref:System.Net.Http.IHttpClientFactory.CreateClient*>。</span><span class="sxs-lookup"><span data-stu-id="912c8-340">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="912c8-341">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="912c8-341">Named clients</span></span>

<span data-ttu-id="912c8-342">如果應用程式需要使用多個不同的 `HttpClient`，且每個都有不同的設定，可以選擇使用**具名用戶端**。</span><span class="sxs-lookup"><span data-stu-id="912c8-342">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="912c8-343">具名 `HttpClient` 的組態可以在 `Startup.ConfigureServices` 中註冊時指定。</span><span class="sxs-lookup"><span data-stu-id="912c8-343">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="912c8-344">在上述程式碼中，會呼叫 `AddHttpClient` 並提供名稱 *github*。</span><span class="sxs-lookup"><span data-stu-id="912c8-344">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="912c8-345">此用戶端已套用一些預設組態&mdash;即使用 GitHub API 所需的基底位址和兩個標頭。</span><span class="sxs-lookup"><span data-stu-id="912c8-345">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="912c8-346">每次呼叫 `CreateClient` 時，會建立 `HttpClient` 的新執行個體並呼叫組態動作。</span><span class="sxs-lookup"><span data-stu-id="912c8-346">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="912c8-347">若要使用具名用戶端，可以傳遞字串參數至 `CreateClient`。</span><span class="sxs-lookup"><span data-stu-id="912c8-347">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="912c8-348">指定要建立之用戶端的名稱：</span><span class="sxs-lookup"><span data-stu-id="912c8-348">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="912c8-349">在上述程式碼中，要求不需要指定主機名稱。</span><span class="sxs-lookup"><span data-stu-id="912c8-349">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="912c8-350">它可以只傳遞路徑，因為已使用為用戶端設定的基底位址。</span><span class="sxs-lookup"><span data-stu-id="912c8-350">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="912c8-351">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="912c8-351">Typed clients</span></span>

<span data-ttu-id="912c8-352">具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="912c8-352">Typed clients:</span></span>

* <span data-ttu-id="912c8-353">提供與具名用戶端相同的功能，而不需使用字串作為索引鍵。</span><span class="sxs-lookup"><span data-stu-id="912c8-353">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="912c8-354">取用用戶端時提供 IntelliSense 和編譯器說明。</span><span class="sxs-lookup"><span data-stu-id="912c8-354">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="912c8-355">提供單一位置來設定特定的 `HttpClient` 並與其互動。</span><span class="sxs-lookup"><span data-stu-id="912c8-355">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="912c8-356">例如，單一的具型別用戶端可能用於單一的後端端點，並封裝處理該端點的所有邏輯。</span><span class="sxs-lookup"><span data-stu-id="912c8-356">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="912c8-357">使用 DI 且可在應用程式中需要之處插入。</span><span class="sxs-lookup"><span data-stu-id="912c8-357">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="912c8-358">具型別用戶端會接受其程式化中的 `HttpClient` 參數：</span><span class="sxs-lookup"><span data-stu-id="912c8-358">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="912c8-359">在上述程式碼中，組態會移到具型別用戶端。</span><span class="sxs-lookup"><span data-stu-id="912c8-359">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="912c8-360">`HttpClient` 物件會公開為公用屬性。</span><span class="sxs-lookup"><span data-stu-id="912c8-360">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="912c8-361">您可定義 API 特定的方法，其公開 `HttpClient` 功能。</span><span class="sxs-lookup"><span data-stu-id="912c8-361">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="912c8-362">`GetAspNetDocsIssues` 方法會封裝從 GitHub 存放庫查詢和剖析最新開啟問題所需的程式碼。</span><span class="sxs-lookup"><span data-stu-id="912c8-362">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="912c8-363">若要註冊具型別用戶端，泛型 <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> 擴充方法可用於 `Startup.ConfigureServices` 內，並指定具型別用戶端類別：</span><span class="sxs-lookup"><span data-stu-id="912c8-363">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="912c8-364">具型別用戶端會向 DI 註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="912c8-364">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="912c8-365">具型別用戶端可以直接插入並使用：</span><span class="sxs-lookup"><span data-stu-id="912c8-365">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="912c8-366">想要的話，具型別用戶端的組態可以在 `Startup.ConfigureServices` 中註冊時指定，而不是在具型別用戶端的建構函式中：</span><span class="sxs-lookup"><span data-stu-id="912c8-366">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="912c8-367">可以將 `HttpClient` 完全封裝在具型別用戶端內。</span><span class="sxs-lookup"><span data-stu-id="912c8-367">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="912c8-368">可以提供在內部呼叫 `HttpClient` 執行個體的公用方法，而不將它公開為屬性。</span><span class="sxs-lookup"><span data-stu-id="912c8-368">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="912c8-369">在上述程式碼，`HttpClient` 儲存為私用欄位。</span><span class="sxs-lookup"><span data-stu-id="912c8-369">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="912c8-370">進行外部呼叫的所有存取都會經歷 `GetRepos` 方法。</span><span class="sxs-lookup"><span data-stu-id="912c8-370">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="912c8-371">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="912c8-371">Generated clients</span></span>

<span data-ttu-id="912c8-372">`IHttpClientFactory` 可和其他協力廠商程式庫一起使用，例如 [Refit](https://github.com/paulcbetts/refit)。</span><span class="sxs-lookup"><span data-stu-id="912c8-372">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="912c8-373">Refit 是適用於 .NET 的 REST 程式庫。</span><span class="sxs-lookup"><span data-stu-id="912c8-373">Refit is a REST library for .NET.</span></span> <span data-ttu-id="912c8-374">它將 REST API 轉換為即時介面。</span><span class="sxs-lookup"><span data-stu-id="912c8-374">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="912c8-375">介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。</span><span class="sxs-lookup"><span data-stu-id="912c8-375">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="912c8-376">定義介面及回覆來代表外部 API 和其回應：</span><span class="sxs-lookup"><span data-stu-id="912c8-376">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="912c8-377">可以新增具型別用戶端，使用 Refit 產生實作：</span><span class="sxs-lookup"><span data-stu-id="912c8-377">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="912c8-378">定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：</span><span class="sxs-lookup"><span data-stu-id="912c8-378">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="912c8-379">外寄要求中介軟體</span><span class="sxs-lookup"><span data-stu-id="912c8-379">Outgoing request middleware</span></span>

<span data-ttu-id="912c8-380">`HttpClient` 已經有委派可針對外寄 HTTP 要求連結在一起的處理常式的概念。</span><span class="sxs-lookup"><span data-stu-id="912c8-380">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="912c8-381">`IHttpClientFactory` 可讓您輕鬆地定義要套用於每個具名用戶端的處理常式。</span><span class="sxs-lookup"><span data-stu-id="912c8-381">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="912c8-382">它支援註冊和鏈結多個處理常式，以建置外寄要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="912c8-382">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="912c8-383">這些處理常式每個都可以在外寄要求之前和之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="912c8-383">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="912c8-384">此模式與 ASP.NET Core 中的輸入中介軟體管線相似。</span><span class="sxs-lookup"><span data-stu-id="912c8-384">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="912c8-385">模式提供一個機制來管理 HTTP 要求的跨領域關注，包括快取、錯誤處理、序列化和記錄。</span><span class="sxs-lookup"><span data-stu-id="912c8-385">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="912c8-386">若要建立處理常式，請定義衍生自 <xref:System.Net.Http.DelegatingHandler> 的類別。</span><span class="sxs-lookup"><span data-stu-id="912c8-386">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="912c8-387">覆寫 `SendAsync` 方法，以在將要求傳遞至管線中的下一個處理常式之前執行程式碼：</span><span class="sxs-lookup"><span data-stu-id="912c8-387">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="912c8-388">上述程式碼定義一個基本處理常式。</span><span class="sxs-lookup"><span data-stu-id="912c8-388">The preceding code defines a basic handler.</span></span> <span data-ttu-id="912c8-389">它會檢查以查看要求上是否已包含 `X-API-KEY` 標頭。</span><span class="sxs-lookup"><span data-stu-id="912c8-389">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="912c8-390">如果遺漏標頭，它可以避免 HTTP 呼叫，並傳回適當的回應。</span><span class="sxs-lookup"><span data-stu-id="912c8-390">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="912c8-391">在註冊期間，可以將一或多個處理常式新增至 `HttpClient`的設定。</span><span class="sxs-lookup"><span data-stu-id="912c8-391">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="912c8-392">這項工作是透過 <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder> 上的擴充方法完成。</span><span class="sxs-lookup"><span data-stu-id="912c8-392">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="912c8-393">在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。</span><span class="sxs-lookup"><span data-stu-id="912c8-393">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="912c8-394">`IHttpClientFactory` 會為每個處理常式建立個別的 DI 範圍。</span><span class="sxs-lookup"><span data-stu-id="912c8-394">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="912c8-395">處理常式可相依於任何範圍的服務。</span><span class="sxs-lookup"><span data-stu-id="912c8-395">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="912c8-396">處置處理常式時，會處置處理常式所相依的服務。</span><span class="sxs-lookup"><span data-stu-id="912c8-396">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="912c8-397">註冊之後，便可以呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>，並傳入處理常式的類型。</span><span class="sxs-lookup"><span data-stu-id="912c8-397">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="912c8-398">可以遵循應該執行的順序來註冊多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="912c8-398">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="912c8-399">每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：</span><span class="sxs-lookup"><span data-stu-id="912c8-399">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="912c8-400">使用下列其中一種方式來與訊息處理常式共用個別要求狀態：</span><span class="sxs-lookup"><span data-stu-id="912c8-400">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="912c8-401">使用 `HttpRequestMessage.Properties` 將資料傳遞到處理常式。</span><span class="sxs-lookup"><span data-stu-id="912c8-401">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="912c8-402">使用 `IHttpContextAccessor` 來存取目前的要求。</span><span class="sxs-lookup"><span data-stu-id="912c8-402">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="912c8-403">建立自訂 `AsyncLocal` 儲存體物件以傳遞資料。</span><span class="sxs-lookup"><span data-stu-id="912c8-403">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="912c8-404">使用 Polly 為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="912c8-404">Use Polly-based handlers</span></span>

<span data-ttu-id="912c8-405">`IHttpClientFactory` 整合受歡迎的協力廠商程式庫，稱為 [Polly](https://github.com/App-vNext/Polly)。</span><span class="sxs-lookup"><span data-stu-id="912c8-405">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="912c8-406">Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。</span><span class="sxs-lookup"><span data-stu-id="912c8-406">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="912c8-407">它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。</span><span class="sxs-lookup"><span data-stu-id="912c8-407">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="912c8-408">提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="912c8-408">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="912c8-409">Polly 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="912c8-409">The Polly extensions:</span></span>

* <span data-ttu-id="912c8-410">支援將以 Polly 為基礎的處理常式新增至用戶端。</span><span class="sxs-lookup"><span data-stu-id="912c8-410">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="912c8-411">可在安裝 [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet 套件後使用。</span><span class="sxs-lookup"><span data-stu-id="912c8-411">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="912c8-412">該套件並未包含在 ASP.NET Core 共用架構中。</span><span class="sxs-lookup"><span data-stu-id="912c8-412">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="912c8-413">處理暫時性錯誤</span><span class="sxs-lookup"><span data-stu-id="912c8-413">Handle transient faults</span></span>

<span data-ttu-id="912c8-414">大部分的錯誤發生在外部 HTTP 呼叫是暫時性的時候。</span><span class="sxs-lookup"><span data-stu-id="912c8-414">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="912c8-415">包含一個便利的擴充方法，稱為 `AddTransientHttpErrorPolicy`，它可允許定義原則來處理暫時性錯誤。</span><span class="sxs-lookup"><span data-stu-id="912c8-415">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="912c8-416">使用此延伸模組方法設定的原則，會處理 `HttpRequestException`、HTTP 5xx 回應和 HTTP 408 回應。</span><span class="sxs-lookup"><span data-stu-id="912c8-416">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="912c8-417">`AddTransientHttpErrorPolicy` 延伸模組可用於 `Startup.ConfigureServices` 內。</span><span class="sxs-lookup"><span data-stu-id="912c8-417">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="912c8-418">延伸模組能提供 `PolicyBuilder` 物件的存取，該物件已設定來處理代表可能暫時性錯誤的錯誤：</span><span class="sxs-lookup"><span data-stu-id="912c8-418">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="912c8-419">在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。</span><span class="sxs-lookup"><span data-stu-id="912c8-419">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="912c8-420">失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。</span><span class="sxs-lookup"><span data-stu-id="912c8-420">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="912c8-421">動態選取原則</span><span class="sxs-lookup"><span data-stu-id="912c8-421">Dynamically select policies</span></span>

<span data-ttu-id="912c8-422">有額外的擴充方法可用來新增 Polly 為基礎的處理常式。</span><span class="sxs-lookup"><span data-stu-id="912c8-422">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="912c8-423">其中一個這類延伸模組是 `AddPolicyHandler`，它有多個多載。</span><span class="sxs-lookup"><span data-stu-id="912c8-423">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="912c8-424">一個多載可讓您在定義要套用的原則時，檢查要求：</span><span class="sxs-lookup"><span data-stu-id="912c8-424">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="912c8-425">在上述程式碼中，如果外寄要求是 HTTP GET，就會套用 10 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="912c8-425">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="912c8-426">任何其他 HTTP 方法會使用 30 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="912c8-426">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="912c8-427">新增多個 Polly 處理常式</span><span class="sxs-lookup"><span data-stu-id="912c8-427">Add multiple Polly handlers</span></span>

<span data-ttu-id="912c8-428">通常會建立巢狀 Polly 原則，以提供增強的功能：</span><span class="sxs-lookup"><span data-stu-id="912c8-428">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="912c8-429">在上述範例中，會新增兩個處理常式。</span><span class="sxs-lookup"><span data-stu-id="912c8-429">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="912c8-430">第一個使用 `AddTransientHttpErrorPolicy` 延伸模組來新增重試原則。</span><span class="sxs-lookup"><span data-stu-id="912c8-430">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="912c8-431">失敗的要求會重試最多三次。</span><span class="sxs-lookup"><span data-stu-id="912c8-431">Failed requests are retried up to three times.</span></span> <span data-ttu-id="912c8-432">第二個 `AddTransientHttpErrorPolicy` 呼叫會新增斷路器原則。</span><span class="sxs-lookup"><span data-stu-id="912c8-432">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="912c8-433">如果循序發生五次失敗的嘗試，進一步的外部要求會遭到封鎖 30 秒。</span><span class="sxs-lookup"><span data-stu-id="912c8-433">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="912c8-434">斷路器原則可設定狀態。</span><span class="sxs-lookup"><span data-stu-id="912c8-434">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="912c8-435">透過此用戶端的所有呼叫都會共用相同的線路狀態。</span><span class="sxs-lookup"><span data-stu-id="912c8-435">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="912c8-436">從 Polly 登錄新增原則</span><span class="sxs-lookup"><span data-stu-id="912c8-436">Add policies from the Polly registry</span></span>

<span data-ttu-id="912c8-437">管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。</span><span class="sxs-lookup"><span data-stu-id="912c8-437">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="912c8-438">提供了擴充方法，可以使用來自登錄的原則新增處理常式：</span><span class="sxs-lookup"><span data-stu-id="912c8-438">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="912c8-439">在上述程式碼中，當 `PolicyRegistry` 新增至 `ServiceCollection` 時，註冊了兩個原則。</span><span class="sxs-lookup"><span data-stu-id="912c8-439">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="912c8-440">為了使用來自登錄的原則，使用了 `AddPolicyHandlerFromRegistry` 方法，並傳遞要套用的原則名稱。</span><span class="sxs-lookup"><span data-stu-id="912c8-440">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="912c8-441">關於 `IHttpClientFactory` Polly 整合的詳細資訊，可以在 [Polly Wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory) 上找到。</span><span class="sxs-lookup"><span data-stu-id="912c8-441">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="912c8-442">HttpClient 和存留期管理</span><span class="sxs-lookup"><span data-stu-id="912c8-442">HttpClient and lifetime management</span></span>

<span data-ttu-id="912c8-443">每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回新的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="912c8-443">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="912c8-444">每個具名用戶端都有一個 <xref:System.Net.Http.HttpMessageHandler>。</span><span class="sxs-lookup"><span data-stu-id="912c8-444">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="912c8-445">處理站會管理 `HttpMessageHandler` 執行個體的存留期。</span><span class="sxs-lookup"><span data-stu-id="912c8-445">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="912c8-446">`IHttpClientFactory` 會將處理站所建立的 `HttpMessageHandler` 執行個體放入集區以減少資源耗用量。</span><span class="sxs-lookup"><span data-stu-id="912c8-446">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="912c8-447">建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。</span><span class="sxs-lookup"><span data-stu-id="912c8-447">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="912c8-448">將處理常式放入集區非常實用，因為處理常式通常會管理自己專屬的底層 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="912c8-448">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="912c8-449">建立比所需數目更多的處理常式，可能會導致連線延遲。</span><span class="sxs-lookup"><span data-stu-id="912c8-449">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="912c8-450">有些處理常式也會保持連線無限期地開啟，這可能導致處理常式無法對 DNS 變更回應。</span><span class="sxs-lookup"><span data-stu-id="912c8-450">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="912c8-451">預設處理常式存留時間為兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="912c8-451">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="912c8-452">可以針對每個具名用戶端覆寫預設值。</span><span class="sxs-lookup"><span data-stu-id="912c8-452">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="912c8-453">若要覆寫它，請在建立用戶端時所傳回的 `IHttpClientBuilder` 上呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*>：</span><span class="sxs-lookup"><span data-stu-id="912c8-453">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="912c8-454">不需要處置用戶端。</span><span class="sxs-lookup"><span data-stu-id="912c8-454">Disposal of the client isn't required.</span></span> <span data-ttu-id="912c8-455">處置會取消傳出的要求，並保證指定的 `HttpClient` 執行個體在呼叫 <xref:System.IDisposable.Dispose*> 之後無法使用。</span><span class="sxs-lookup"><span data-stu-id="912c8-455">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="912c8-456">`IHttpClientFactory` 會追蹤並處置 `HttpClient` 執行個體使用的資源。</span><span class="sxs-lookup"><span data-stu-id="912c8-456">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="912c8-457">`HttpClient` 執行個體通常可視為 .NET 物件，不需要處置。</span><span class="sxs-lookup"><span data-stu-id="912c8-457">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="912c8-458">在開始使用 `IHttpClientFactory` 之前，讓單一 `HttpClient` 執行個體維持一段較長的時間，是很常使用的模式。</span><span class="sxs-lookup"><span data-stu-id="912c8-458">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="912c8-459">在移轉到 `IHttpClientFactory` 之後，就不再需要此模式。</span><span class="sxs-lookup"><span data-stu-id="912c8-459">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="912c8-460">IHttpClientFactory 的替代方案</span><span class="sxs-lookup"><span data-stu-id="912c8-460">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="912c8-461">在啟用 DI 的應用程式中使用 `IHttpClientFactory` 可避免：</span><span class="sxs-lookup"><span data-stu-id="912c8-461">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="912c8-462">藉由共用 `HttpMessageHandler` 實例的資源耗盡問題。</span><span class="sxs-lookup"><span data-stu-id="912c8-462">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="912c8-463">以固定間隔迴圈 `HttpMessageHandler` 實例的過時 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="912c8-463">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="912c8-464">有其他方法可以使用長期 <xref:System.Net.Http.SocketsHttpHandler> 實例來解決上述問題。</span><span class="sxs-lookup"><span data-stu-id="912c8-464">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="912c8-465">當應用程式啟動時，建立 `SocketsHttpHandler` 的實例，並在應用程式的生命週期中使用它。</span><span class="sxs-lookup"><span data-stu-id="912c8-465">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="912c8-466">根據 DNS 重新整理時間，將 <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> 設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="912c8-466">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="912c8-467">視需要使用 `new HttpClient(handler, dispostHandler: false)` 建立 `HttpClient` 實例。</span><span class="sxs-lookup"><span data-stu-id="912c8-467">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="912c8-468">上述方法可解決 `IHttpClientFactory` 以類似的方式解決的資源管理問題。</span><span class="sxs-lookup"><span data-stu-id="912c8-468">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="912c8-469">`SocketsHttpHandler` 會共用 `HttpClient` 實例間的連接。</span><span class="sxs-lookup"><span data-stu-id="912c8-469">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="912c8-470">此共用可防止通訊端耗盡。</span><span class="sxs-lookup"><span data-stu-id="912c8-470">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="912c8-471">`SocketsHttpHandler` 會根據 `PooledConnectionLifetime` 迴圈連接，以避免過時的 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="912c8-471">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="912c8-472">Cookies</span><span class="sxs-lookup"><span data-stu-id="912c8-472">Cookies</span></span>

<span data-ttu-id="912c8-473">集區 `HttpMessageHandler` 實例會導致共用 `CookieContainer` 物件。</span><span class="sxs-lookup"><span data-stu-id="912c8-473">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="912c8-474">意外的 `CookieContainer` 物件共用通常會導致不正確的程式碼。</span><span class="sxs-lookup"><span data-stu-id="912c8-474">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="912c8-475">針對需要 cookie 的應用程式，請考慮下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="912c8-475">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="912c8-476">停用自動 cookie 處理</span><span class="sxs-lookup"><span data-stu-id="912c8-476">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="912c8-477">避免 `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="912c8-477">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="912c8-478">呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 以停用自動的 cookie 處理：</span><span class="sxs-lookup"><span data-stu-id="912c8-478">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="912c8-479">記錄</span><span class="sxs-lookup"><span data-stu-id="912c8-479">Logging</span></span>

<span data-ttu-id="912c8-480">透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="912c8-480">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="912c8-481">在記錄設定中啟用適當的資訊層級，以查看預設記錄檔訊息。</span><span class="sxs-lookup"><span data-stu-id="912c8-481">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="912c8-482">額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。</span><span class="sxs-lookup"><span data-stu-id="912c8-482">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="912c8-483">用於每個用戶端的記錄檔分類包含用戶端的名稱。</span><span class="sxs-lookup"><span data-stu-id="912c8-483">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="912c8-484">例如，名為 *MyNamedClient* 的用戶端會記錄分類為 `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler` 的訊息。</span><span class="sxs-lookup"><span data-stu-id="912c8-484">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="912c8-485">後面加上 *LogicalHandler* 的訊息發生在要求處理常式管線之外。</span><span class="sxs-lookup"><span data-stu-id="912c8-485">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="912c8-486">在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。</span><span class="sxs-lookup"><span data-stu-id="912c8-486">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="912c8-487">在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。</span><span class="sxs-lookup"><span data-stu-id="912c8-487">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="912c8-488">記錄也會發生在要求處理常式管線之內。</span><span class="sxs-lookup"><span data-stu-id="912c8-488">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="912c8-489">在 *MyNamedClient* 範例中，那些訊息是針對記錄檔分類 `System.Net.Http.HttpClient.MyNamedClient.ClientHandler` 而記錄。</span><span class="sxs-lookup"><span data-stu-id="912c8-489">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="912c8-490">對於要求，這是發生在所有其他處理常式都已執行之後，並且緊接在網路上傳送要求之前。</span><span class="sxs-lookup"><span data-stu-id="912c8-490">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="912c8-491">在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。</span><span class="sxs-lookup"><span data-stu-id="912c8-491">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="912c8-492">在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。</span><span class="sxs-lookup"><span data-stu-id="912c8-492">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="912c8-493">例如，這可能包括要求標頭的變更，或是回應狀態碼的變更。</span><span class="sxs-lookup"><span data-stu-id="912c8-493">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="912c8-494">在記錄分類中包含用戶端的名稱，可讓您在需要時進行特定具名用戶端的記錄檔篩選。</span><span class="sxs-lookup"><span data-stu-id="912c8-494">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="912c8-495">設定 HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="912c8-495">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="912c8-496">可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。</span><span class="sxs-lookup"><span data-stu-id="912c8-496">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="912c8-497">新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。</span><span class="sxs-lookup"><span data-stu-id="912c8-497">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="912c8-498"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 擴充方法可以用來定義委派。</span><span class="sxs-lookup"><span data-stu-id="912c8-498">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="912c8-499">委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：</span><span class="sxs-lookup"><span data-stu-id="912c8-499">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="912c8-500">在主控台應用程式中使用 IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="912c8-500">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="912c8-501">在主控台應用程式中，將下列套件參考新增至專案：</span><span class="sxs-lookup"><span data-stu-id="912c8-501">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="912c8-502">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="912c8-502">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="912c8-503">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="912c8-503">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="912c8-504">在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="912c8-504">In the following example:</span></span>

* <span data-ttu-id="912c8-505"><xref:System.Net.Http.IHttpClientFactory> 已在[泛型主機的](xref:fundamentals/host/generic-host)服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="912c8-505"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="912c8-506">`MyService` 會從服務建立用戶端 Factory 執行個體，其可用來建立 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="912c8-506">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="912c8-507">`HttpClient` 會用來擷取網頁。</span><span class="sxs-lookup"><span data-stu-id="912c8-507">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="912c8-508">`Main` 會建立範圍來執行服務的 `GetPage` 方法，並將網頁內容的前 500 個字元寫入至主控台。</span><span class="sxs-lookup"><span data-stu-id="912c8-508">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="912c8-509">其他資源</span><span class="sxs-lookup"><span data-stu-id="912c8-509">Additional resources</span></span>

* [<span data-ttu-id="912c8-510">使用 HttpClientFactory 實作復原 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="912c8-510">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="912c8-511">使用 HttpClientFactory 和 Polly 原則以指數輪詢實作 HTTP 呼叫重試</span><span class="sxs-lookup"><span data-stu-id="912c8-511">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="912c8-512">實作斷路器模式</span><span class="sxs-lookup"><span data-stu-id="912c8-512">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="912c8-513">作者：[Glenn Condron](https://github.com/glennc)、[Ryan Nowak](https://github.com/rynowak) 和 [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="912c8-513">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="912c8-514"><xref:System.Net.Http.IHttpClientFactory> 可以註冊及用來在應用程式中設定和建立 <xref:System.Net.Http.HttpClient> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="912c8-514">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="912c8-515">它提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="912c8-515">It offers the following benefits:</span></span>

* <span data-ttu-id="912c8-516">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="912c8-516">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="912c8-517">例如，可註冊及設定 *github* 用戶端，來存取 [GitHub](https://github.com/)。</span><span class="sxs-lookup"><span data-stu-id="912c8-517">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="912c8-518">預設用戶端可以註冊用於其他用途。</span><span class="sxs-lookup"><span data-stu-id="912c8-518">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="912c8-519">透過委派 `HttpClient` 中的處理常式來撰寫外寄中介軟體的概念，並提供延伸模組以便 Polly 架構中介軟體利用外寄中介軟體。</span><span class="sxs-lookup"><span data-stu-id="912c8-519">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="912c8-520">管理基礎 `HttpClientMessageHandler` 執行個體的共用和存留期，以避免在手動管理 `HttpClient` 存留期時，發生的常見 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="912c8-520">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="912c8-521">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="912c8-521">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="912c8-522">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="912c8-522">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="912c8-523">必要條件：</span><span class="sxs-lookup"><span data-stu-id="912c8-523">Prerequisites</span></span>

<span data-ttu-id="912c8-524">以 .NET Framework 為目標的專案，需要安裝 [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="912c8-524">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="912c8-525">以 .NET Core 為目標且參考 [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) 的專案，已包含 `Microsoft.Extensions.Http` 套件。</span><span class="sxs-lookup"><span data-stu-id="912c8-525">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="912c8-526">耗用模式</span><span class="sxs-lookup"><span data-stu-id="912c8-526">Consumption patterns</span></span>

<span data-ttu-id="912c8-527">有數種方式可將 `IHttpClientFactory` 用於應用程式：</span><span class="sxs-lookup"><span data-stu-id="912c8-527">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="912c8-528">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="912c8-528">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="912c8-529">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="912c8-529">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="912c8-530">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="912c8-530">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="912c8-531">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="912c8-531">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="912c8-532">它們全都不會嚴格優先於另一個。</span><span class="sxs-lookup"><span data-stu-id="912c8-532">None of them are strictly superior to another.</span></span> <span data-ttu-id="912c8-533">最好的方法取決於應用程式的條件約束。</span><span class="sxs-lookup"><span data-stu-id="912c8-533">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="912c8-534">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="912c8-534">Basic usage</span></span>

<span data-ttu-id="912c8-535">`IHttpClientFactory` 可以藉由在 `Startup.ConfigureServices` 方法內的 `IServiceCollection` 上呼叫 `AddHttpClient` 擴充方法來註冊。</span><span class="sxs-lookup"><span data-stu-id="912c8-535">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="912c8-536">註冊之後，程式碼可以在可使用[相依性插入 (DI)](xref:fundamentals/dependency-injection) 插入服務的任何位置，接受 `IHttpClientFactory`。</span><span class="sxs-lookup"><span data-stu-id="912c8-536">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="912c8-537">`IHttpClientFactory` 可以用來建立 `HttpClient` 實例：</span><span class="sxs-lookup"><span data-stu-id="912c8-537">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="912c8-538">以這種方式使用 `IHttpClientFactory` 是重構現有應用程式的好方法。</span><span class="sxs-lookup"><span data-stu-id="912c8-538">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="912c8-539">它對 `HttpClient` 的使用方式沒有任何影響。</span><span class="sxs-lookup"><span data-stu-id="912c8-539">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="912c8-540">在目前建立 `HttpClient` 執行個體的位置，將那些項目取代為呼叫 <xref:System.Net.Http.IHttpClientFactory.CreateClient*>。</span><span class="sxs-lookup"><span data-stu-id="912c8-540">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="912c8-541">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="912c8-541">Named clients</span></span>

<span data-ttu-id="912c8-542">如果應用程式需要使用多個不同的 `HttpClient`，且每個都有不同的設定，可以選擇使用**具名用戶端**。</span><span class="sxs-lookup"><span data-stu-id="912c8-542">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="912c8-543">具名 `HttpClient` 的組態可以在 `Startup.ConfigureServices` 中註冊時指定。</span><span class="sxs-lookup"><span data-stu-id="912c8-543">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="912c8-544">在上述程式碼中，會呼叫 `AddHttpClient` 並提供名稱 *github*。</span><span class="sxs-lookup"><span data-stu-id="912c8-544">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="912c8-545">此用戶端已套用一些預設組態&mdash;即使用 GitHub API 所需的基底位址和兩個標頭。</span><span class="sxs-lookup"><span data-stu-id="912c8-545">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="912c8-546">每次呼叫 `CreateClient` 時，會建立 `HttpClient` 的新執行個體並呼叫組態動作。</span><span class="sxs-lookup"><span data-stu-id="912c8-546">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="912c8-547">若要使用具名用戶端，可以傳遞字串參數至 `CreateClient`。</span><span class="sxs-lookup"><span data-stu-id="912c8-547">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="912c8-548">指定要建立之用戶端的名稱：</span><span class="sxs-lookup"><span data-stu-id="912c8-548">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="912c8-549">在上述程式碼中，要求不需要指定主機名稱。</span><span class="sxs-lookup"><span data-stu-id="912c8-549">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="912c8-550">它可以只傳遞路徑，因為已使用為用戶端設定的基底位址。</span><span class="sxs-lookup"><span data-stu-id="912c8-550">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="912c8-551">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="912c8-551">Typed clients</span></span>

<span data-ttu-id="912c8-552">具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="912c8-552">Typed clients:</span></span>

* <span data-ttu-id="912c8-553">提供與具名用戶端相同的功能，而不需使用字串作為索引鍵。</span><span class="sxs-lookup"><span data-stu-id="912c8-553">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="912c8-554">取用用戶端時提供 IntelliSense 和編譯器說明。</span><span class="sxs-lookup"><span data-stu-id="912c8-554">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="912c8-555">提供單一位置來設定特定的 `HttpClient` 並與其互動。</span><span class="sxs-lookup"><span data-stu-id="912c8-555">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="912c8-556">例如，單一的具型別用戶端可能用於單一的後端端點，並封裝處理該端點的所有邏輯。</span><span class="sxs-lookup"><span data-stu-id="912c8-556">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="912c8-557">使用 DI 且可在應用程式中需要之處插入。</span><span class="sxs-lookup"><span data-stu-id="912c8-557">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="912c8-558">具型別用戶端會接受其程式化中的 `HttpClient` 參數：</span><span class="sxs-lookup"><span data-stu-id="912c8-558">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="912c8-559">在上述程式碼中，組態會移到具型別用戶端。</span><span class="sxs-lookup"><span data-stu-id="912c8-559">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="912c8-560">`HttpClient` 物件會公開為公用屬性。</span><span class="sxs-lookup"><span data-stu-id="912c8-560">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="912c8-561">您可定義 API 特定的方法，其公開 `HttpClient` 功能。</span><span class="sxs-lookup"><span data-stu-id="912c8-561">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="912c8-562">`GetAspNetDocsIssues` 方法會封裝從 GitHub 存放庫查詢和剖析最新開啟問題所需的程式碼。</span><span class="sxs-lookup"><span data-stu-id="912c8-562">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="912c8-563">若要註冊具型別用戶端，泛型 <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> 擴充方法可用於 `Startup.ConfigureServices` 內，並指定具型別用戶端類別：</span><span class="sxs-lookup"><span data-stu-id="912c8-563">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="912c8-564">具型別用戶端會向 DI 註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="912c8-564">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="912c8-565">具型別用戶端可以直接插入並使用：</span><span class="sxs-lookup"><span data-stu-id="912c8-565">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="912c8-566">想要的話，具型別用戶端的組態可以在 `Startup.ConfigureServices` 中註冊時指定，而不是在具型別用戶端的建構函式中：</span><span class="sxs-lookup"><span data-stu-id="912c8-566">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="912c8-567">可以將 `HttpClient` 完全封裝在具型別用戶端內。</span><span class="sxs-lookup"><span data-stu-id="912c8-567">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="912c8-568">可以提供在內部呼叫 `HttpClient` 執行個體的公用方法，而不將它公開為屬性。</span><span class="sxs-lookup"><span data-stu-id="912c8-568">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="912c8-569">在上述程式碼，`HttpClient` 儲存為私用欄位。</span><span class="sxs-lookup"><span data-stu-id="912c8-569">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="912c8-570">進行外部呼叫的所有存取都會經歷 `GetRepos` 方法。</span><span class="sxs-lookup"><span data-stu-id="912c8-570">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="912c8-571">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="912c8-571">Generated clients</span></span>

<span data-ttu-id="912c8-572">`IHttpClientFactory` 可和其他協力廠商程式庫一起使用，例如 [Refit](https://github.com/paulcbetts/refit)。</span><span class="sxs-lookup"><span data-stu-id="912c8-572">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="912c8-573">Refit 是適用於 .NET 的 REST 程式庫。</span><span class="sxs-lookup"><span data-stu-id="912c8-573">Refit is a REST library for .NET.</span></span> <span data-ttu-id="912c8-574">它將 REST API 轉換為即時介面。</span><span class="sxs-lookup"><span data-stu-id="912c8-574">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="912c8-575">介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。</span><span class="sxs-lookup"><span data-stu-id="912c8-575">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="912c8-576">定義介面及回覆來代表外部 API 和其回應：</span><span class="sxs-lookup"><span data-stu-id="912c8-576">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="912c8-577">可以新增具型別用戶端，使用 Refit 產生實作：</span><span class="sxs-lookup"><span data-stu-id="912c8-577">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="912c8-578">定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：</span><span class="sxs-lookup"><span data-stu-id="912c8-578">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="912c8-579">外寄要求中介軟體</span><span class="sxs-lookup"><span data-stu-id="912c8-579">Outgoing request middleware</span></span>

<span data-ttu-id="912c8-580">`HttpClient` 已經有委派可針對外寄 HTTP 要求連結在一起的處理常式的概念。</span><span class="sxs-lookup"><span data-stu-id="912c8-580">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="912c8-581">`IHttpClientFactory` 可讓您輕鬆地定義要套用於每個具名用戶端的處理常式。</span><span class="sxs-lookup"><span data-stu-id="912c8-581">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="912c8-582">它支援註冊和鏈結多個處理常式，以建置外寄要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="912c8-582">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="912c8-583">這些處理常式每個都可以在外寄要求之前和之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="912c8-583">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="912c8-584">此模式與 ASP.NET Core 中的輸入中介軟體管線相似。</span><span class="sxs-lookup"><span data-stu-id="912c8-584">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="912c8-585">模式提供一個機制來管理 HTTP 要求的跨領域關注，包括快取、錯誤處理、序列化和記錄。</span><span class="sxs-lookup"><span data-stu-id="912c8-585">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="912c8-586">若要建立處理常式，請定義衍生自 <xref:System.Net.Http.DelegatingHandler> 的類別。</span><span class="sxs-lookup"><span data-stu-id="912c8-586">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="912c8-587">覆寫 `SendAsync` 方法，以在將要求傳遞至管線中的下一個處理常式之前執行程式碼：</span><span class="sxs-lookup"><span data-stu-id="912c8-587">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="912c8-588">上述程式碼定義一個基本處理常式。</span><span class="sxs-lookup"><span data-stu-id="912c8-588">The preceding code defines a basic handler.</span></span> <span data-ttu-id="912c8-589">它會檢查以查看要求上是否已包含 `X-API-KEY` 標頭。</span><span class="sxs-lookup"><span data-stu-id="912c8-589">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="912c8-590">如果遺漏標頭，它可以避免 HTTP 呼叫，並傳回適當的回應。</span><span class="sxs-lookup"><span data-stu-id="912c8-590">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="912c8-591">在註冊期間，可以將一或多個處理常式新增至 `HttpClient`的設定。</span><span class="sxs-lookup"><span data-stu-id="912c8-591">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="912c8-592">這項工作是透過 <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder> 上的擴充方法完成。</span><span class="sxs-lookup"><span data-stu-id="912c8-592">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="912c8-593">在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。</span><span class="sxs-lookup"><span data-stu-id="912c8-593">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="912c8-594">處理常式**必須**在 DI 中註冊為暫時性服務，無限定範圍。</span><span class="sxs-lookup"><span data-stu-id="912c8-594">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="912c8-595">如果處理常式已註冊為範圍服務，而且處理常式所相依的任何服務都是可處置的：</span><span class="sxs-lookup"><span data-stu-id="912c8-595">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="912c8-596">處理常式的服務可能會在處理常式超出範圍之前加以處置。</span><span class="sxs-lookup"><span data-stu-id="912c8-596">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="912c8-597">已處置的處理常式服務會導致處理常式失敗。</span><span class="sxs-lookup"><span data-stu-id="912c8-597">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="912c8-598">註冊之後，便可以呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>，並傳入處理常式類型。</span><span class="sxs-lookup"><span data-stu-id="912c8-598">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="912c8-599">可以遵循應該執行的順序來註冊多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="912c8-599">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="912c8-600">每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：</span><span class="sxs-lookup"><span data-stu-id="912c8-600">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="912c8-601">使用下列其中一種方式來與訊息處理常式共用個別要求狀態：</span><span class="sxs-lookup"><span data-stu-id="912c8-601">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="912c8-602">使用 `HttpRequestMessage.Properties` 將資料傳遞到處理常式。</span><span class="sxs-lookup"><span data-stu-id="912c8-602">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="912c8-603">使用 `IHttpContextAccessor` 來存取目前的要求。</span><span class="sxs-lookup"><span data-stu-id="912c8-603">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="912c8-604">建立自訂 `AsyncLocal` 儲存體物件以傳遞資料。</span><span class="sxs-lookup"><span data-stu-id="912c8-604">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="912c8-605">使用 Polly 為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="912c8-605">Use Polly-based handlers</span></span>

<span data-ttu-id="912c8-606">`IHttpClientFactory` 整合受歡迎的協力廠商程式庫，稱為 [Polly](https://github.com/App-vNext/Polly)。</span><span class="sxs-lookup"><span data-stu-id="912c8-606">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="912c8-607">Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。</span><span class="sxs-lookup"><span data-stu-id="912c8-607">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="912c8-608">它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。</span><span class="sxs-lookup"><span data-stu-id="912c8-608">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="912c8-609">提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="912c8-609">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="912c8-610">Polly 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="912c8-610">The Polly extensions:</span></span>

* <span data-ttu-id="912c8-611">支援將以 Polly 為基礎的處理常式新增至用戶端。</span><span class="sxs-lookup"><span data-stu-id="912c8-611">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="912c8-612">可在安裝 [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet 套件後使用。</span><span class="sxs-lookup"><span data-stu-id="912c8-612">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="912c8-613">該套件並未包含在 ASP.NET Core 共用架構中。</span><span class="sxs-lookup"><span data-stu-id="912c8-613">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="912c8-614">處理暫時性錯誤</span><span class="sxs-lookup"><span data-stu-id="912c8-614">Handle transient faults</span></span>

<span data-ttu-id="912c8-615">大部分的錯誤發生在外部 HTTP 呼叫是暫時性的時候。</span><span class="sxs-lookup"><span data-stu-id="912c8-615">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="912c8-616">包含一個便利的擴充方法，稱為 `AddTransientHttpErrorPolicy`，它可允許定義原則來處理暫時性錯誤。</span><span class="sxs-lookup"><span data-stu-id="912c8-616">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="912c8-617">使用此延伸模組方法設定的原則，會處理 `HttpRequestException`、HTTP 5xx 回應和 HTTP 408 回應。</span><span class="sxs-lookup"><span data-stu-id="912c8-617">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="912c8-618">`AddTransientHttpErrorPolicy` 延伸模組可用於 `Startup.ConfigureServices` 內。</span><span class="sxs-lookup"><span data-stu-id="912c8-618">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="912c8-619">延伸模組能提供 `PolicyBuilder` 物件的存取，該物件已設定來處理代表可能暫時性錯誤的錯誤：</span><span class="sxs-lookup"><span data-stu-id="912c8-619">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="912c8-620">在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。</span><span class="sxs-lookup"><span data-stu-id="912c8-620">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="912c8-621">失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。</span><span class="sxs-lookup"><span data-stu-id="912c8-621">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="912c8-622">動態選取原則</span><span class="sxs-lookup"><span data-stu-id="912c8-622">Dynamically select policies</span></span>

<span data-ttu-id="912c8-623">有額外的擴充方法可用來新增 Polly 為基礎的處理常式。</span><span class="sxs-lookup"><span data-stu-id="912c8-623">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="912c8-624">其中一個這類延伸模組是 `AddPolicyHandler`，它有多個多載。</span><span class="sxs-lookup"><span data-stu-id="912c8-624">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="912c8-625">一個多載可讓您在定義要套用的原則時，檢查要求：</span><span class="sxs-lookup"><span data-stu-id="912c8-625">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="912c8-626">在上述程式碼中，如果外寄要求是 HTTP GET，就會套用 10 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="912c8-626">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="912c8-627">任何其他 HTTP 方法會使用 30 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="912c8-627">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="912c8-628">新增多個 Polly 處理常式</span><span class="sxs-lookup"><span data-stu-id="912c8-628">Add multiple Polly handlers</span></span>

<span data-ttu-id="912c8-629">通常會建立巢狀 Polly 原則，以提供增強的功能：</span><span class="sxs-lookup"><span data-stu-id="912c8-629">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="912c8-630">在上述範例中，會新增兩個處理常式。</span><span class="sxs-lookup"><span data-stu-id="912c8-630">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="912c8-631">第一個使用 `AddTransientHttpErrorPolicy` 延伸模組來新增重試原則。</span><span class="sxs-lookup"><span data-stu-id="912c8-631">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="912c8-632">失敗的要求會重試最多三次。</span><span class="sxs-lookup"><span data-stu-id="912c8-632">Failed requests are retried up to three times.</span></span> <span data-ttu-id="912c8-633">第二個 `AddTransientHttpErrorPolicy` 呼叫會新增斷路器原則。</span><span class="sxs-lookup"><span data-stu-id="912c8-633">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="912c8-634">如果循序發生五次失敗的嘗試，進一步的外部要求會遭到封鎖 30 秒。</span><span class="sxs-lookup"><span data-stu-id="912c8-634">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="912c8-635">斷路器原則可設定狀態。</span><span class="sxs-lookup"><span data-stu-id="912c8-635">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="912c8-636">透過此用戶端的所有呼叫都會共用相同的線路狀態。</span><span class="sxs-lookup"><span data-stu-id="912c8-636">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="912c8-637">從 Polly 登錄新增原則</span><span class="sxs-lookup"><span data-stu-id="912c8-637">Add policies from the Polly registry</span></span>

<span data-ttu-id="912c8-638">管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。</span><span class="sxs-lookup"><span data-stu-id="912c8-638">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="912c8-639">提供了擴充方法，可以使用來自登錄的原則新增處理常式：</span><span class="sxs-lookup"><span data-stu-id="912c8-639">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="912c8-640">在上述程式碼中，當 `PolicyRegistry` 新增至 `ServiceCollection` 時，註冊了兩個原則。</span><span class="sxs-lookup"><span data-stu-id="912c8-640">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="912c8-641">為了使用來自登錄的原則，使用了 `AddPolicyHandlerFromRegistry` 方法，並傳遞要套用的原則名稱。</span><span class="sxs-lookup"><span data-stu-id="912c8-641">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="912c8-642">關於 `IHttpClientFactory` Polly 整合的詳細資訊，可以在 [Polly Wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory) 上找到。</span><span class="sxs-lookup"><span data-stu-id="912c8-642">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="912c8-643">HttpClient 和存留期管理</span><span class="sxs-lookup"><span data-stu-id="912c8-643">HttpClient and lifetime management</span></span>

<span data-ttu-id="912c8-644">每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回新的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="912c8-644">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="912c8-645">每個具名用戶端都有一個 <xref:System.Net.Http.HttpMessageHandler>。</span><span class="sxs-lookup"><span data-stu-id="912c8-645">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="912c8-646">處理站會管理 `HttpMessageHandler` 執行個體的存留期。</span><span class="sxs-lookup"><span data-stu-id="912c8-646">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="912c8-647">`IHttpClientFactory` 會將處理站所建立的 `HttpMessageHandler` 執行個體放入集區以減少資源耗用量。</span><span class="sxs-lookup"><span data-stu-id="912c8-647">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="912c8-648">建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。</span><span class="sxs-lookup"><span data-stu-id="912c8-648">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="912c8-649">將處理常式放入集區非常實用，因為處理常式通常會管理自己專屬的底層 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="912c8-649">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="912c8-650">建立比所需數目更多的處理常式，可能會導致連線延遲。</span><span class="sxs-lookup"><span data-stu-id="912c8-650">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="912c8-651">有些處理常式也會保持連線無限期地開啟，這可能導致處理常式無法對 DNS 變更回應。</span><span class="sxs-lookup"><span data-stu-id="912c8-651">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="912c8-652">預設處理常式存留時間為兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="912c8-652">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="912c8-653">可以針對每個具名用戶端覆寫預設值。</span><span class="sxs-lookup"><span data-stu-id="912c8-653">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="912c8-654">若要覆寫它，請在建立用戶端時所傳回的 `IHttpClientBuilder` 上呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*>：</span><span class="sxs-lookup"><span data-stu-id="912c8-654">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="912c8-655">不需要處置用戶端。</span><span class="sxs-lookup"><span data-stu-id="912c8-655">Disposal of the client isn't required.</span></span> <span data-ttu-id="912c8-656">處置會取消傳出的要求，並保證指定的 `HttpClient` 執行個體在呼叫 <xref:System.IDisposable.Dispose*> 之後無法使用。</span><span class="sxs-lookup"><span data-stu-id="912c8-656">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="912c8-657">`IHttpClientFactory` 會追蹤並處置 `HttpClient` 執行個體使用的資源。</span><span class="sxs-lookup"><span data-stu-id="912c8-657">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="912c8-658">`HttpClient` 執行個體通常可視為 .NET 物件，不需要處置。</span><span class="sxs-lookup"><span data-stu-id="912c8-658">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="912c8-659">在開始使用 `IHttpClientFactory` 之前，讓單一 `HttpClient` 執行個體維持一段較長的時間，是很常使用的模式。</span><span class="sxs-lookup"><span data-stu-id="912c8-659">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="912c8-660">在移轉到 `IHttpClientFactory` 之後，就不再需要此模式。</span><span class="sxs-lookup"><span data-stu-id="912c8-660">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="912c8-661">IHttpClientFactory 的替代方案</span><span class="sxs-lookup"><span data-stu-id="912c8-661">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="912c8-662">在啟用 DI 的應用程式中使用 `IHttpClientFactory` 可避免：</span><span class="sxs-lookup"><span data-stu-id="912c8-662">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="912c8-663">藉由共用 `HttpMessageHandler` 實例的資源耗盡問題。</span><span class="sxs-lookup"><span data-stu-id="912c8-663">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="912c8-664">以固定間隔迴圈 `HttpMessageHandler` 實例的過時 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="912c8-664">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="912c8-665">有其他方法可以使用長期 <xref:System.Net.Http.SocketsHttpHandler> 實例來解決上述問題。</span><span class="sxs-lookup"><span data-stu-id="912c8-665">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="912c8-666">當應用程式啟動時，建立 `SocketsHttpHandler` 的實例，並在應用程式的生命週期中使用它。</span><span class="sxs-lookup"><span data-stu-id="912c8-666">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="912c8-667">根據 DNS 重新整理時間，將 <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> 設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="912c8-667">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="912c8-668">視需要使用 `new HttpClient(handler, dispostHandler: false)` 建立 `HttpClient` 實例。</span><span class="sxs-lookup"><span data-stu-id="912c8-668">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="912c8-669">上述方法可解決 `IHttpClientFactory` 以類似的方式解決的資源管理問題。</span><span class="sxs-lookup"><span data-stu-id="912c8-669">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="912c8-670">`SocketsHttpHandler` 會共用 `HttpClient` 實例間的連接。</span><span class="sxs-lookup"><span data-stu-id="912c8-670">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="912c8-671">此共用可防止通訊端耗盡。</span><span class="sxs-lookup"><span data-stu-id="912c8-671">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="912c8-672">`SocketsHttpHandler` 會根據 `PooledConnectionLifetime` 迴圈連接，以避免過時的 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="912c8-672">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="912c8-673">Cookies</span><span class="sxs-lookup"><span data-stu-id="912c8-673">Cookies</span></span>

<span data-ttu-id="912c8-674">集區 `HttpMessageHandler` 實例會導致共用 `CookieContainer` 物件。</span><span class="sxs-lookup"><span data-stu-id="912c8-674">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="912c8-675">意外的 `CookieContainer` 物件共用通常會導致不正確的程式碼。</span><span class="sxs-lookup"><span data-stu-id="912c8-675">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="912c8-676">針對需要 cookie 的應用程式，請考慮下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="912c8-676">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="912c8-677">停用自動 cookie 處理</span><span class="sxs-lookup"><span data-stu-id="912c8-677">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="912c8-678">避免 `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="912c8-678">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="912c8-679">呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 以停用自動的 cookie 處理：</span><span class="sxs-lookup"><span data-stu-id="912c8-679">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="912c8-680">記錄</span><span class="sxs-lookup"><span data-stu-id="912c8-680">Logging</span></span>

<span data-ttu-id="912c8-681">透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="912c8-681">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="912c8-682">在記錄設定中啟用適當的資訊層級，以查看預設記錄檔訊息。</span><span class="sxs-lookup"><span data-stu-id="912c8-682">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="912c8-683">額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。</span><span class="sxs-lookup"><span data-stu-id="912c8-683">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="912c8-684">用於每個用戶端的記錄檔分類包含用戶端的名稱。</span><span class="sxs-lookup"><span data-stu-id="912c8-684">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="912c8-685">例如，名為 *MyNamedClient* 的用戶端會記錄分類為 `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler` 的訊息。</span><span class="sxs-lookup"><span data-stu-id="912c8-685">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="912c8-686">後面加上 *LogicalHandler* 的訊息發生在要求處理常式管線之外。</span><span class="sxs-lookup"><span data-stu-id="912c8-686">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="912c8-687">在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。</span><span class="sxs-lookup"><span data-stu-id="912c8-687">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="912c8-688">在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。</span><span class="sxs-lookup"><span data-stu-id="912c8-688">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="912c8-689">記錄也會發生在要求處理常式管線之內。</span><span class="sxs-lookup"><span data-stu-id="912c8-689">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="912c8-690">在 *MyNamedClient* 範例中，那些訊息是針對記錄檔分類 `System.Net.Http.HttpClient.MyNamedClient.ClientHandler` 而記錄。</span><span class="sxs-lookup"><span data-stu-id="912c8-690">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="912c8-691">對於要求，這是發生在所有其他處理常式都已執行之後，並且緊接在網路上傳送要求之前。</span><span class="sxs-lookup"><span data-stu-id="912c8-691">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="912c8-692">在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。</span><span class="sxs-lookup"><span data-stu-id="912c8-692">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="912c8-693">在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。</span><span class="sxs-lookup"><span data-stu-id="912c8-693">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="912c8-694">例如，這可能包括要求標頭的變更，或是回應狀態碼的變更。</span><span class="sxs-lookup"><span data-stu-id="912c8-694">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="912c8-695">在記錄分類中包含用戶端的名稱，可讓您在需要時進行特定具名用戶端的記錄檔篩選。</span><span class="sxs-lookup"><span data-stu-id="912c8-695">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="912c8-696">設定 HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="912c8-696">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="912c8-697">可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。</span><span class="sxs-lookup"><span data-stu-id="912c8-697">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="912c8-698">新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。</span><span class="sxs-lookup"><span data-stu-id="912c8-698">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="912c8-699"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 擴充方法可以用來定義委派。</span><span class="sxs-lookup"><span data-stu-id="912c8-699">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="912c8-700">委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：</span><span class="sxs-lookup"><span data-stu-id="912c8-700">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="912c8-701">在主控台應用程式中使用 IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="912c8-701">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="912c8-702">在主控台應用程式中，將下列套件參考新增至專案：</span><span class="sxs-lookup"><span data-stu-id="912c8-702">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="912c8-703">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="912c8-703">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="912c8-704">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="912c8-704">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="912c8-705">在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="912c8-705">In the following example:</span></span>

* <span data-ttu-id="912c8-706"><xref:System.Net.Http.IHttpClientFactory> 已在[泛型主機的](xref:fundamentals/host/generic-host)服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="912c8-706"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="912c8-707">`MyService` 會從服務建立用戶端 Factory 執行個體，其可用來建立 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="912c8-707">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="912c8-708">`HttpClient` 會用來擷取網頁。</span><span class="sxs-lookup"><span data-stu-id="912c8-708">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="912c8-709">`Main` 會建立範圍來執行服務的 `GetPage` 方法，並將網頁內容的前 500 個字元寫入至主控台。</span><span class="sxs-lookup"><span data-stu-id="912c8-709">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="912c8-710">其他資源</span><span class="sxs-lookup"><span data-stu-id="912c8-710">Additional resources</span></span>

* [<span data-ttu-id="912c8-711">使用 HttpClientFactory 實作復原 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="912c8-711">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="912c8-712">使用 HttpClientFactory 和 Polly 原則以指數輪詢實作 HTTP 呼叫重試</span><span class="sxs-lookup"><span data-stu-id="912c8-712">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="912c8-713">實作斷路器模式</span><span class="sxs-lookup"><span data-stu-id="912c8-713">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
