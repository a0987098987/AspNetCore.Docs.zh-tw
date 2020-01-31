---
title: 在 ASP.NET Core 中使用 IHttpClientFactory 發出 HTTP 要求
author: stevejgordon
description: 深入了解在 ASP.NET Core 中使用 IHttpClientFactory 介面來管理邏輯 HttpClient 執行個體。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 12/16/2019
uid: fundamentals/http-requests
ms.openlocfilehash: 9b9da82191a587be0603ee114562e9a964f05250
ms.sourcegitcommit: fe41cff0b99f3920b727286944e5b652ca301640
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2020
ms.locfileid: "76870394"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="05425-103">在 ASP.NET Core 中使用 IHttpClientFactory 發出 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="05425-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="05425-104">作者： [Glenn Condron](https://github.com/glennc)、 [Ryan Nowak](https://github.com/rynowak)、 [Steve Gordon](https://github.com/stevejgordon)、 [Rick Anderson](https://twitter.com/RickAndMSFT)和[Kirk Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="05425-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="05425-105"><xref:System.Net.Http.IHttpClientFactory> 可以註冊及用來在應用程式中設定和建立 <xref:System.Net.Http.HttpClient> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="05425-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="05425-106">`IHttpClientFactory` 提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="05425-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="05425-107">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="05425-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="05425-108">例如，名為*github*的用戶端可以註冊並設定為存取[github](https://github.com/)。</span><span class="sxs-lookup"><span data-stu-id="05425-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="05425-109">預設用戶端可以註冊以進行一般存取。</span><span class="sxs-lookup"><span data-stu-id="05425-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="05425-110">透過 `HttpClient`中的委派處理常式，制訂外寄中介軟體的概念。</span><span class="sxs-lookup"><span data-stu-id="05425-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="05425-111">提供 Polly 為基礎中介軟體的延伸模組，以利用 `HttpClient`中的委派處理常式。</span><span class="sxs-lookup"><span data-stu-id="05425-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="05425-112">管理基礎 `HttpClientMessageHandler` 實例的共用和存留期。</span><span class="sxs-lookup"><span data-stu-id="05425-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="05425-113">自動管理可避免在手動管理 `HttpClient` 存留期時所發生的常見 DNS （網域名稱系統）問題。</span><span class="sxs-lookup"><span data-stu-id="05425-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="05425-114">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="05425-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="05425-115">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="05425-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="05425-116">本主題中的範例程式碼會使用 <xref:System.Text.Json> 來還原序列化 HTTP 回應中所傳回的 JSON 內容。</span><span class="sxs-lookup"><span data-stu-id="05425-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="05425-117">如需使用 `Json.NET` 和 `ReadAsAsync<T>`的範例，請使用版本選取器來選取此主題的2.x 版。</span><span class="sxs-lookup"><span data-stu-id="05425-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="05425-118">耗用模式</span><span class="sxs-lookup"><span data-stu-id="05425-118">Consumption patterns</span></span>

<span data-ttu-id="05425-119">有數種方式可將 `IHttpClientFactory` 用於應用程式：</span><span class="sxs-lookup"><span data-stu-id="05425-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="05425-120">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="05425-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="05425-121">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="05425-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="05425-122">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="05425-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="05425-123">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="05425-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="05425-124">最佳方法取決於應用程式的需求。</span><span class="sxs-lookup"><span data-stu-id="05425-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="05425-125">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="05425-125">Basic usage</span></span>

<span data-ttu-id="05425-126">`IHttpClientFactory` 可以藉由呼叫 `AddHttpClient`進行註冊：</span><span class="sxs-lookup"><span data-stu-id="05425-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="05425-127">`IHttpClientFactory` 可以使用相依性[插入（DI）](xref:fundamentals/dependency-injection)來要求。</span><span class="sxs-lookup"><span data-stu-id="05425-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="05425-128">下列程式碼會使用 `IHttpClientFactory` 來建立 `HttpClient` 實例：</span><span class="sxs-lookup"><span data-stu-id="05425-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="05425-129">使用上述範例中的 `IHttpClientFactory` 就是重構現有應用程式的好方法。</span><span class="sxs-lookup"><span data-stu-id="05425-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="05425-130">這不會影響使用 `HttpClient` 的方式。</span><span class="sxs-lookup"><span data-stu-id="05425-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="05425-131">在現有應用程式中建立 `HttpClient` 實例的位置，使用 <xref:System.Net.Http.IHttpClientFactory.CreateClient*>的呼叫來取代這些專案。</span><span class="sxs-lookup"><span data-stu-id="05425-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="05425-132">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="05425-132">Named clients</span></span>

<span data-ttu-id="05425-133">在下列情況中，命名的用戶端是不錯的選擇：</span><span class="sxs-lookup"><span data-stu-id="05425-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="05425-134">應用程式需要 `HttpClient`的許多不同用途。</span><span class="sxs-lookup"><span data-stu-id="05425-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="05425-135">許多 `HttpClient`都有不同的設定。</span><span class="sxs-lookup"><span data-stu-id="05425-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="05425-136">在 `Startup.ConfigureServices`註冊期間，可以指定已命名 `HttpClient` 的設定：</span><span class="sxs-lookup"><span data-stu-id="05425-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="05425-137">在上述程式碼中，用戶端是使用下列設定：</span><span class="sxs-lookup"><span data-stu-id="05425-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="05425-138">基底位址 `https://api.github.com/`。</span><span class="sxs-lookup"><span data-stu-id="05425-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="05425-139">使用 GitHub API 時需要兩個標頭。</span><span class="sxs-lookup"><span data-stu-id="05425-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="05425-140">CreateClient</span><span class="sxs-lookup"><span data-stu-id="05425-140">CreateClient</span></span>

<span data-ttu-id="05425-141">每次呼叫 <xref:System.Net.Http.IHttpClientFactory.CreateClient*> 時：</span><span class="sxs-lookup"><span data-stu-id="05425-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="05425-142">建立 `HttpClient` 的新實例。</span><span class="sxs-lookup"><span data-stu-id="05425-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="05425-143">會呼叫設定動作。</span><span class="sxs-lookup"><span data-stu-id="05425-143">The configuration action is called.</span></span>

<span data-ttu-id="05425-144">若要建立名為的用戶端，請將其名稱傳遞至 `CreateClient`：</span><span class="sxs-lookup"><span data-stu-id="05425-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="05425-145">在上述程式碼中，要求不需要指定主機名稱。</span><span class="sxs-lookup"><span data-stu-id="05425-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="05425-146">此程式碼只會傳遞路徑，因為會使用為用戶端設定的基底位址。</span><span class="sxs-lookup"><span data-stu-id="05425-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="05425-147">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="05425-147">Typed clients</span></span>

<span data-ttu-id="05425-148">具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="05425-148">Typed clients:</span></span>

* <span data-ttu-id="05425-149">提供與具名用戶端相同的功能，而不需使用字串作為索引鍵。</span><span class="sxs-lookup"><span data-stu-id="05425-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="05425-150">取用用戶端時提供 IntelliSense 和編譯器說明。</span><span class="sxs-lookup"><span data-stu-id="05425-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="05425-151">提供單一位置來設定特定的 `HttpClient` 並與其互動。</span><span class="sxs-lookup"><span data-stu-id="05425-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="05425-152">例如，可能會使用單一具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="05425-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="05425-153">適用于單一後端端點。</span><span class="sxs-lookup"><span data-stu-id="05425-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="05425-154">封裝處理端點的所有邏輯。</span><span class="sxs-lookup"><span data-stu-id="05425-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="05425-155">使用 DI，並可在應用程式中需要的位置插入。</span><span class="sxs-lookup"><span data-stu-id="05425-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="05425-156">具型別用戶端會接受其程式化中的 `HttpClient` 參數：</span><span class="sxs-lookup"><span data-stu-id="05425-156">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="05425-157">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="05425-157">In the preceding code:</span></span>

* <span data-ttu-id="05425-158">設定會移到具型別用戶端。</span><span class="sxs-lookup"><span data-stu-id="05425-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="05425-159">`HttpClient` 物件會公開為公用屬性。</span><span class="sxs-lookup"><span data-stu-id="05425-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="05425-160">可以建立可公開 `HttpClient` 功能的 API 特定方法。</span><span class="sxs-lookup"><span data-stu-id="05425-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="05425-161">例如，`GetAspNetDocsIssues` 方法會封裝程式碼以取得未解決的問題。</span><span class="sxs-lookup"><span data-stu-id="05425-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="05425-162">下列程式碼會呼叫 `Startup.ConfigureServices` 中的 <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> 來註冊具類型的用戶端類別：</span><span class="sxs-lookup"><span data-stu-id="05425-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="05425-163">具型別用戶端會向 DI 註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="05425-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="05425-164">具型別用戶端可以直接插入並使用：</span><span class="sxs-lookup"><span data-stu-id="05425-164">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="05425-165">型別用戶端的設定可以在 `Startup.ConfigureServices`註冊期間指定，而不是在具型別用戶端的函式中：</span><span class="sxs-lookup"><span data-stu-id="05425-165">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="05425-166">`HttpClient` 可以封裝在具型別用戶端中。</span><span class="sxs-lookup"><span data-stu-id="05425-166">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="05425-167">請定義在內部呼叫 `HttpClient` 實例的方法，而不是將它公開為屬性：</span><span class="sxs-lookup"><span data-stu-id="05425-167">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="05425-168">在上述程式碼中，`HttpClient` 儲存在私用欄位中。</span><span class="sxs-lookup"><span data-stu-id="05425-168">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="05425-169">`HttpClient` 的存取權是公用 `GetRepos` 方法。</span><span class="sxs-lookup"><span data-stu-id="05425-169">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="05425-170">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="05425-170">Generated clients</span></span>

<span data-ttu-id="05425-171">`IHttpClientFactory` 可以與協力廠商程式庫搭配使用，例如重新[調整。](https://github.com/paulcbetts/refit)</span><span class="sxs-lookup"><span data-stu-id="05425-171">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="05425-172">Refit 是適用於 .NET 的 REST 程式庫。</span><span class="sxs-lookup"><span data-stu-id="05425-172">Refit is a REST library for .NET.</span></span> <span data-ttu-id="05425-173">它將 REST API 轉換為即時介面。</span><span class="sxs-lookup"><span data-stu-id="05425-173">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="05425-174">介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。</span><span class="sxs-lookup"><span data-stu-id="05425-174">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="05425-175">定義介面及回覆來代表外部 API 和其回應：</span><span class="sxs-lookup"><span data-stu-id="05425-175">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="05425-176">可以新增具型別用戶端，使用 Refit 產生實作：</span><span class="sxs-lookup"><span data-stu-id="05425-176">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="05425-177">定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：</span><span class="sxs-lookup"><span data-stu-id="05425-177">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="05425-178">外寄要求中介軟體</span><span class="sxs-lookup"><span data-stu-id="05425-178">Outgoing request middleware</span></span>

<span data-ttu-id="05425-179">`HttpClient` 具有委派處理常式的概念，可以針對傳出 HTTP 要求連結在一起。</span><span class="sxs-lookup"><span data-stu-id="05425-179">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="05425-180">`IHttpClientFactory`：</span><span class="sxs-lookup"><span data-stu-id="05425-180">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="05425-181">簡化定義要套用至每個已命名用戶端的處理常式。</span><span class="sxs-lookup"><span data-stu-id="05425-181">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="05425-182">支援多個處理常式的註冊和連結，以建立外寄要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="05425-182">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="05425-183">這些處理常式每個都可以在外寄要求之前和之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="05425-183">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="05425-184">此模式：</span><span class="sxs-lookup"><span data-stu-id="05425-184">This pattern:</span></span>

  * <span data-ttu-id="05425-185">類似 ASP.NET Core 中的輸入中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="05425-185">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="05425-186">提供一種機制來管理 HTTP 要求的跨領域考慮，例如：</span><span class="sxs-lookup"><span data-stu-id="05425-186">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="05425-187">快取</span><span class="sxs-lookup"><span data-stu-id="05425-187">caching</span></span>
    * <span data-ttu-id="05425-188">錯誤處理</span><span class="sxs-lookup"><span data-stu-id="05425-188">error handling</span></span>
    * <span data-ttu-id="05425-189">serialization</span><span class="sxs-lookup"><span data-stu-id="05425-189">serialization</span></span>
    * <span data-ttu-id="05425-190">記錄</span><span class="sxs-lookup"><span data-stu-id="05425-190">logging</span></span>

<span data-ttu-id="05425-191">若要建立委派處理常式：</span><span class="sxs-lookup"><span data-stu-id="05425-191">To create a delegating handler:</span></span>

* <span data-ttu-id="05425-192">衍生自 <xref:System.Net.Http.DelegatingHandler>。</span><span class="sxs-lookup"><span data-stu-id="05425-192">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="05425-193">覆寫 <xref:System.Net.Http.DelegatingHandler.SendAsync*>。</span><span class="sxs-lookup"><span data-stu-id="05425-193">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="05425-194">先執行程式碼，再將要求傳遞至管線中的下一個處理常式：</span><span class="sxs-lookup"><span data-stu-id="05425-194">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="05425-195">上述程式碼會檢查 `X-API-KEY` 標頭是否在要求中。</span><span class="sxs-lookup"><span data-stu-id="05425-195">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="05425-196">如果遺漏 `X-API-KEY`，則會傳回 <xref:System.Net.HttpStatusCode.BadRequest>。</span><span class="sxs-lookup"><span data-stu-id="05425-196">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="05425-197">可以將一個以上的處理常式新增至具有 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>之 `HttpClient` 的設定：</span><span class="sxs-lookup"><span data-stu-id="05425-197">More than one handler can be added to the configuration for an `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="05425-198">在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。</span><span class="sxs-lookup"><span data-stu-id="05425-198">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="05425-199">`IHttpClientFactory` 會為每個處理常式建立個別的 DI 範圍。</span><span class="sxs-lookup"><span data-stu-id="05425-199">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="05425-200">處理常式可以相依于任何範圍的服務。</span><span class="sxs-lookup"><span data-stu-id="05425-200">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="05425-201">處置處理常式時，會處置處理常式所相依的服務。</span><span class="sxs-lookup"><span data-stu-id="05425-201">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="05425-202">註冊之後，便可以呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>，並傳入處理常式的類型。</span><span class="sxs-lookup"><span data-stu-id="05425-202">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="05425-203">可以遵循應該執行的順序來註冊多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="05425-203">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="05425-204">每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：</span><span class="sxs-lookup"><span data-stu-id="05425-204">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="05425-205">使用下列其中一種方式來與訊息處理常式共用個別要求狀態：</span><span class="sxs-lookup"><span data-stu-id="05425-205">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="05425-206">使用[HttpRequestMessage](xref:System.Net.Http.HttpRequestMessage.Properties)將資料傳遞至處理常式。</span><span class="sxs-lookup"><span data-stu-id="05425-206">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="05425-207">使用 <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> 來存取目前的要求。</span><span class="sxs-lookup"><span data-stu-id="05425-207">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="05425-208">建立自訂 <xref:System.Threading.AsyncLocal`1> 儲存體物件以傳遞資料。</span><span class="sxs-lookup"><span data-stu-id="05425-208">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="05425-209">使用 Polly 為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="05425-209">Use Polly-based handlers</span></span>

<span data-ttu-id="05425-210">`IHttpClientFactory` 與協力廠商程式庫[Polly](https://github.com/App-vNext/Polly)整合。</span><span class="sxs-lookup"><span data-stu-id="05425-210">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="05425-211">Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。</span><span class="sxs-lookup"><span data-stu-id="05425-211">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="05425-212">它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。</span><span class="sxs-lookup"><span data-stu-id="05425-212">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="05425-213">提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="05425-213">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="05425-214">Polly 擴充功能支援將以 Polly 為基礎的處理常式新增至用戶端。</span><span class="sxs-lookup"><span data-stu-id="05425-214">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="05425-215">Polly 需要[Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="05425-215">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="05425-216">處理暫時性錯誤</span><span class="sxs-lookup"><span data-stu-id="05425-216">Handle transient faults</span></span>

<span data-ttu-id="05425-217">當外部 HTTP 呼叫是暫時性的時，通常會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="05425-217">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="05425-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> 允許定義原則來處理暫時性錯誤。</span><span class="sxs-lookup"><span data-stu-id="05425-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="05425-219">以 `AddTransientHttpErrorPolicy` 設定的原則會處理下列回應：</span><span class="sxs-lookup"><span data-stu-id="05425-219">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="05425-220">HTTP 5xx</span><span class="sxs-lookup"><span data-stu-id="05425-220">HTTP 5xx</span></span>
* <span data-ttu-id="05425-221">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="05425-221">HTTP 408</span></span>

<span data-ttu-id="05425-222">`AddTransientHttpErrorPolicy` 提供 `PolicyBuilder` 物件的存取權，其設定為處理代表可能暫時性錯誤的錯誤：</span><span class="sxs-lookup"><span data-stu-id="05425-222">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="05425-223">在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。</span><span class="sxs-lookup"><span data-stu-id="05425-223">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="05425-224">失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。</span><span class="sxs-lookup"><span data-stu-id="05425-224">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="05425-225">動態選取原則</span><span class="sxs-lookup"><span data-stu-id="05425-225">Dynamically select policies</span></span>

<span data-ttu-id="05425-226">提供擴充方法來加入以 Polly 為基礎的處理常式，例如 <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>。</span><span class="sxs-lookup"><span data-stu-id="05425-226">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="05425-227">下列 `AddPolicyHandler` 多載會檢查要求以決定要套用的原則：</span><span class="sxs-lookup"><span data-stu-id="05425-227">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="05425-228">在上述程式碼中，如果外寄要求是 HTTP GET，就會套用 10 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="05425-228">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="05425-229">任何其他 HTTP 方法會使用 30 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="05425-229">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="05425-230">新增多個 Polly 處理常式</span><span class="sxs-lookup"><span data-stu-id="05425-230">Add multiple Polly handlers</span></span>

<span data-ttu-id="05425-231">通常會將 Polly 原則加以嵌套：</span><span class="sxs-lookup"><span data-stu-id="05425-231">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="05425-232">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="05425-232">In the preceding example:</span></span>

* <span data-ttu-id="05425-233">新增了兩個處理常式。</span><span class="sxs-lookup"><span data-stu-id="05425-233">Two handlers are added.</span></span>
* <span data-ttu-id="05425-234">第一個處理常式會使用 <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> 來新增重試原則。</span><span class="sxs-lookup"><span data-stu-id="05425-234">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="05425-235">失敗的要求會重試最多三次。</span><span class="sxs-lookup"><span data-stu-id="05425-235">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="05425-236">第二個 `AddTransientHttpErrorPolicy` 呼叫會增加斷路器原則。</span><span class="sxs-lookup"><span data-stu-id="05425-236">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="05425-237">如果連續5次嘗試失敗，則會封鎖進一步的外部要求30秒。</span><span class="sxs-lookup"><span data-stu-id="05425-237">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="05425-238">斷路器原則可設定狀態。</span><span class="sxs-lookup"><span data-stu-id="05425-238">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="05425-239">透過此用戶端的所有呼叫都會共用相同的線路狀態。</span><span class="sxs-lookup"><span data-stu-id="05425-239">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="05425-240">從 Polly 登錄新增原則</span><span class="sxs-lookup"><span data-stu-id="05425-240">Add policies from the Polly registry</span></span>

<span data-ttu-id="05425-241">管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。</span><span class="sxs-lookup"><span data-stu-id="05425-241">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="05425-242">在下列程式碼中：</span><span class="sxs-lookup"><span data-stu-id="05425-242">In the following code:</span></span>

* <span data-ttu-id="05425-243">系統會新增「一般」和「長」原則。</span><span class="sxs-lookup"><span data-stu-id="05425-243">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="05425-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*> 從登錄新增「一般」和「長」原則。</span><span class="sxs-lookup"><span data-stu-id="05425-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="05425-245">如需 `IHttpClientFactory` 和 Polly 整合的詳細資訊，請參閱[Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory)。</span><span class="sxs-lookup"><span data-stu-id="05425-245">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="05425-246">HttpClient 和存留期管理</span><span class="sxs-lookup"><span data-stu-id="05425-246">HttpClient and lifetime management</span></span>

<span data-ttu-id="05425-247">每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回新的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="05425-247">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="05425-248">系統會根據每個命名的用戶端來建立 <xref:System.Net.Http.HttpMessageHandler>。</span><span class="sxs-lookup"><span data-stu-id="05425-248">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="05425-249">處理站會管理 `HttpMessageHandler` 執行個體的存留期。</span><span class="sxs-lookup"><span data-stu-id="05425-249">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="05425-250">`IHttpClientFactory` 會將處理站所建立的 `HttpMessageHandler` 執行個體放入集區以減少資源耗用量。</span><span class="sxs-lookup"><span data-stu-id="05425-250">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="05425-251">建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。</span><span class="sxs-lookup"><span data-stu-id="05425-251">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="05425-252">將處理常式放入集區非常實用，因為處理常式通常會管理自己專屬的底層 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="05425-252">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="05425-253">建立比所需數目更多的處理常式，可能會導致連線延遲。</span><span class="sxs-lookup"><span data-stu-id="05425-253">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="05425-254">有些處理常式也會保持連線無限期地開啟，這可能導致處理常式無法回應 DNS （網域名稱系統）變更。</span><span class="sxs-lookup"><span data-stu-id="05425-254">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="05425-255">預設處理常式存留時間為兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="05425-255">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="05425-256">預設值可以根據每個命名的用戶端來覆寫：</span><span class="sxs-lookup"><span data-stu-id="05425-256">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="05425-257">`HttpClient` 實例通常可視為**不**需要處置的 .net 物件。</span><span class="sxs-lookup"><span data-stu-id="05425-257">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="05425-258">處置會取消傳出的要求，並保證指定的 `HttpClient` 執行個體在呼叫 <xref:System.IDisposable.Dispose*> 之後無法使用。</span><span class="sxs-lookup"><span data-stu-id="05425-258">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="05425-259">`IHttpClientFactory` 會追蹤並處置 `HttpClient` 執行個體使用的資源。</span><span class="sxs-lookup"><span data-stu-id="05425-259">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="05425-260">在開始使用 `IHttpClientFactory` 之前，讓單一 `HttpClient` 執行個體維持一段較長的時間，是很常使用的模式。</span><span class="sxs-lookup"><span data-stu-id="05425-260">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="05425-261">在移轉到 `IHttpClientFactory` 之後，就不再需要此模式。</span><span class="sxs-lookup"><span data-stu-id="05425-261">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="05425-262">IHttpClientFactory 的替代方案</span><span class="sxs-lookup"><span data-stu-id="05425-262">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="05425-263">在啟用 DI 的應用程式中使用 `IHttpClientFactory` 可避免：</span><span class="sxs-lookup"><span data-stu-id="05425-263">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="05425-264">藉由共用 `HttpMessageHandler` 實例的資源耗盡問題。</span><span class="sxs-lookup"><span data-stu-id="05425-264">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="05425-265">以固定間隔迴圈 `HttpMessageHandler` 實例的過時 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="05425-265">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="05425-266">有其他方法可以使用長期 <xref:System.Net.Http.SocketsHttpHandler> 實例來解決上述問題。</span><span class="sxs-lookup"><span data-stu-id="05425-266">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="05425-267">當應用程式啟動時，建立 `SocketsHttpHandler` 的實例，並在應用程式的生命週期中使用它。</span><span class="sxs-lookup"><span data-stu-id="05425-267">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="05425-268">根據 DNS 重新整理時間，將 <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> 設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="05425-268">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="05425-269">視需要使用 `new HttpClient(handler, disposeHandler: false)` 建立 `HttpClient` 實例。</span><span class="sxs-lookup"><span data-stu-id="05425-269">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="05425-270">上述方法可解決 `IHttpClientFactory` 以類似的方式解決的資源管理問題。</span><span class="sxs-lookup"><span data-stu-id="05425-270">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="05425-271">`SocketsHttpHandler` 會共用 `HttpClient` 實例間的連接。</span><span class="sxs-lookup"><span data-stu-id="05425-271">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="05425-272">此共用可防止通訊端耗盡。</span><span class="sxs-lookup"><span data-stu-id="05425-272">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="05425-273">`SocketsHttpHandler` 會根據 `PooledConnectionLifetime` 迴圈連接，以避免過時的 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="05425-273">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="05425-274">Cookies</span><span class="sxs-lookup"><span data-stu-id="05425-274">Cookies</span></span>

<span data-ttu-id="05425-275">集區 `HttpMessageHandler` 實例會導致共用 `CookieContainer` 物件。</span><span class="sxs-lookup"><span data-stu-id="05425-275">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="05425-276">意外的 `CookieContainer` 物件共用通常會導致不正確的程式碼。</span><span class="sxs-lookup"><span data-stu-id="05425-276">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="05425-277">針對需要 cookie 的應用程式，請考慮下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="05425-277">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="05425-278">停用自動 cookie 處理</span><span class="sxs-lookup"><span data-stu-id="05425-278">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="05425-279">避免 `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="05425-279">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="05425-280">呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 以停用自動的 cookie 處理：</span><span class="sxs-lookup"><span data-stu-id="05425-280">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="05425-281">記錄</span><span class="sxs-lookup"><span data-stu-id="05425-281">Logging</span></span>

<span data-ttu-id="05425-282">透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="05425-282">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="05425-283">在記錄設定中啟用適當的資訊層級，以查看預設記錄檔訊息。</span><span class="sxs-lookup"><span data-stu-id="05425-283">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="05425-284">額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。</span><span class="sxs-lookup"><span data-stu-id="05425-284">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="05425-285">用於每個用戶端的記錄檔分類包含用戶端的名稱。</span><span class="sxs-lookup"><span data-stu-id="05425-285">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="05425-286">例如，名為*MyNamedClient*的用戶端會記錄類別為 "HttpClient" 的訊息。**MyNamedClient**。LogicalHandler "。</span><span class="sxs-lookup"><span data-stu-id="05425-286">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="05425-287">後面加上 *LogicalHandler* 的訊息發生在要求處理常式管線之外。</span><span class="sxs-lookup"><span data-stu-id="05425-287">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="05425-288">在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。</span><span class="sxs-lookup"><span data-stu-id="05425-288">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="05425-289">在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。</span><span class="sxs-lookup"><span data-stu-id="05425-289">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="05425-290">記錄也會發生在要求處理常式管線之內。</span><span class="sxs-lookup"><span data-stu-id="05425-290">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="05425-291">在*MyNamedClient*範例中，這些訊息會記錄在記錄檔類別中為 "HttpClient"。**MyNamedClient**。ClientHandler".</span><span class="sxs-lookup"><span data-stu-id="05425-291">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="05425-292">對於要求，這會在所有其他處理常式都執行之後，且在傳送要求之前立即發生。</span><span class="sxs-lookup"><span data-stu-id="05425-292">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="05425-293">在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。</span><span class="sxs-lookup"><span data-stu-id="05425-293">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="05425-294">在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。</span><span class="sxs-lookup"><span data-stu-id="05425-294">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="05425-295">這可能包括要求標頭的變更或回應狀態碼。</span><span class="sxs-lookup"><span data-stu-id="05425-295">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="05425-296">在記錄類別中包含用戶端的名稱，可以針對特定的已命名用戶端進行記錄篩選。</span><span class="sxs-lookup"><span data-stu-id="05425-296">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="05425-297">設定 HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="05425-297">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="05425-298">可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。</span><span class="sxs-lookup"><span data-stu-id="05425-298">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="05425-299">新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。</span><span class="sxs-lookup"><span data-stu-id="05425-299">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="05425-300"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 擴充方法可以用來定義委派。</span><span class="sxs-lookup"><span data-stu-id="05425-300">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="05425-301">委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：</span><span class="sxs-lookup"><span data-stu-id="05425-301">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="05425-302">在主控台應用程式中使用 IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="05425-302">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="05425-303">在主控台應用程式中，將下列套件參考新增至專案：</span><span class="sxs-lookup"><span data-stu-id="05425-303">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="05425-304">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="05425-304">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="05425-305">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="05425-305">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="05425-306">在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="05425-306">In the following example:</span></span>

* <span data-ttu-id="05425-307"><xref:System.Net.Http.IHttpClientFactory> 已在[泛型主機的](xref:fundamentals/host/generic-host)服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="05425-307"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="05425-308">`MyService` 會從服務建立用戶端 Factory 執行個體，其可用來建立 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="05425-308">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="05425-309">`HttpClient` 會用來擷取網頁。</span><span class="sxs-lookup"><span data-stu-id="05425-309">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="05425-310">`Main` 會建立範圍來執行服務的 `GetPage` 方法，並將網頁內容的前 500 個字元寫入至主控台。</span><span class="sxs-lookup"><span data-stu-id="05425-310">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="header-propagation-middleware"></a><span data-ttu-id="05425-311">標頭傳播中介軟體</span><span class="sxs-lookup"><span data-stu-id="05425-311">Header propagation middleware</span></span>

<span data-ttu-id="05425-312">標頭傳播是一個 ASP.NET Core 中介軟體，可將 HTTP 標頭從傳入要求傳播到傳出的 HTTP 用戶端要求。</span><span class="sxs-lookup"><span data-stu-id="05425-312">Header propagation is an ASP.NET Core middleware to propagate HTTP headers from the incoming request to the outgoing HTTP Client requests.</span></span> <span data-ttu-id="05425-313">若要使用標頭傳播：</span><span class="sxs-lookup"><span data-stu-id="05425-313">To use header propagation:</span></span>

* <span data-ttu-id="05425-314">參考[AspNetCore. HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation)套件。</span><span class="sxs-lookup"><span data-stu-id="05425-314">Reference the [Microsoft.AspNetCore.HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation) package.</span></span>
* <span data-ttu-id="05425-315">在 `Startup`中設定中介軟體和 `HttpClient`：</span><span class="sxs-lookup"><span data-stu-id="05425-315">Configure the middleware and `HttpClient` in `Startup`:</span></span>

  [!code-csharp[](http-requests/samples/3.x/Startup.cs?highlight=5-9,21&name=snippet)]

* <span data-ttu-id="05425-316">用戶端會在輸出要求中包含已設定的標頭：</span><span class="sxs-lookup"><span data-stu-id="05425-316">The client includes the configured headers on outbound requests:</span></span>

  ```C#
  var client = clientFactory.CreateClient("MyForwardingClient");
  var response = client.GetAsync(...);
  ```

## <a name="additional-resources"></a><span data-ttu-id="05425-317">其他資源</span><span class="sxs-lookup"><span data-stu-id="05425-317">Additional resources</span></span>

* [<span data-ttu-id="05425-318">使用 HttpClientFactory 實作復原 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="05425-318">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="05425-319">使用 HttpClientFactory 和 Polly 原則以指數輪詢實作 HTTP 呼叫重試</span><span class="sxs-lookup"><span data-stu-id="05425-319">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="05425-320">實作斷路器模式</span><span class="sxs-lookup"><span data-stu-id="05425-320">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)
* [<span data-ttu-id="05425-321">如何在 .NET 中序列化和還原序列化 JSON</span><span class="sxs-lookup"><span data-stu-id="05425-321">How to serialize and deserialize JSON in .NET</span></span>](/dotnet/standard/serialization/system-text-json-how-to)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="05425-322">作者：[Glenn Condron](https://github.com/glennc)、[Ryan Nowak](https://github.com/rynowak) 和 [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="05425-322">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="05425-323"><xref:System.Net.Http.IHttpClientFactory> 可以註冊及用來在應用程式中設定和建立 <xref:System.Net.Http.HttpClient> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="05425-323">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="05425-324">它提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="05425-324">It offers the following benefits:</span></span>

* <span data-ttu-id="05425-325">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="05425-325">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="05425-326">例如，可註冊及設定 *github* 用戶端，來存取 [GitHub](https://github.com/)。</span><span class="sxs-lookup"><span data-stu-id="05425-326">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="05425-327">預設用戶端可以註冊用於其他用途。</span><span class="sxs-lookup"><span data-stu-id="05425-327">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="05425-328">透過委派 `HttpClient` 中的處理常式來撰寫外寄中介軟體的概念，並提供延伸模組以便 Polly 架構中介軟體利用外寄中介軟體。</span><span class="sxs-lookup"><span data-stu-id="05425-328">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="05425-329">管理基礎 `HttpClientMessageHandler` 執行個體的共用和存留期，以避免在手動管理 `HttpClient` 存留期時，發生的常見 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="05425-329">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="05425-330">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="05425-330">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="05425-331">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="05425-331">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="05425-332">耗用模式</span><span class="sxs-lookup"><span data-stu-id="05425-332">Consumption patterns</span></span>

<span data-ttu-id="05425-333">有數種方式可將 `IHttpClientFactory` 用於應用程式：</span><span class="sxs-lookup"><span data-stu-id="05425-333">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="05425-334">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="05425-334">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="05425-335">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="05425-335">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="05425-336">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="05425-336">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="05425-337">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="05425-337">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="05425-338">它們全都不會嚴格優先於另一個。</span><span class="sxs-lookup"><span data-stu-id="05425-338">None of them are strictly superior to another.</span></span> <span data-ttu-id="05425-339">最好的方法取決於應用程式的條件約束。</span><span class="sxs-lookup"><span data-stu-id="05425-339">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="05425-340">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="05425-340">Basic usage</span></span>

<span data-ttu-id="05425-341">`IHttpClientFactory` 可以藉由在 `Startup.ConfigureServices` 方法內的 `IServiceCollection` 上呼叫 `AddHttpClient` 擴充方法來註冊。</span><span class="sxs-lookup"><span data-stu-id="05425-341">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="05425-342">註冊之後，程式碼可以在可使用[相依性插入 (DI)](xref:fundamentals/dependency-injection) 插入服務的任何位置，接受 `IHttpClientFactory`。</span><span class="sxs-lookup"><span data-stu-id="05425-342">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="05425-343">`IHttpClientFactory` 可以用來建立 `HttpClient` 實例：</span><span class="sxs-lookup"><span data-stu-id="05425-343">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="05425-344">以這種方式使用 `IHttpClientFactory` 是重構現有應用程式的好方法。</span><span class="sxs-lookup"><span data-stu-id="05425-344">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="05425-345">它對 `HttpClient` 的使用方式沒有任何影響。</span><span class="sxs-lookup"><span data-stu-id="05425-345">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="05425-346">在目前建立 `HttpClient` 執行個體的位置，將那些項目取代為呼叫 <xref:System.Net.Http.IHttpClientFactory.CreateClient*>。</span><span class="sxs-lookup"><span data-stu-id="05425-346">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="05425-347">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="05425-347">Named clients</span></span>

<span data-ttu-id="05425-348">如果應用程式需要使用多個不同的 `HttpClient`，且每個都有不同的設定，可以選擇使用**具名用戶端**。</span><span class="sxs-lookup"><span data-stu-id="05425-348">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="05425-349">具名 `HttpClient` 的組態可以在 `Startup.ConfigureServices` 中註冊時指定。</span><span class="sxs-lookup"><span data-stu-id="05425-349">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="05425-350">在上述程式碼中，會呼叫 `AddHttpClient` 並提供名稱 *github*。</span><span class="sxs-lookup"><span data-stu-id="05425-350">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="05425-351">此用戶端已套用一些預設組態&mdash;即使用 GitHub API 所需的基底位址和兩個標頭。</span><span class="sxs-lookup"><span data-stu-id="05425-351">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="05425-352">每次呼叫 `CreateClient` 時，會建立 `HttpClient` 的新執行個體並呼叫組態動作。</span><span class="sxs-lookup"><span data-stu-id="05425-352">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="05425-353">若要使用具名用戶端，可以傳遞字串參數至 `CreateClient`。</span><span class="sxs-lookup"><span data-stu-id="05425-353">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="05425-354">指定要建立之用戶端的名稱：</span><span class="sxs-lookup"><span data-stu-id="05425-354">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="05425-355">在上述程式碼中，要求不需要指定主機名稱。</span><span class="sxs-lookup"><span data-stu-id="05425-355">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="05425-356">它可以只傳遞路徑，因為已使用為用戶端設定的基底位址。</span><span class="sxs-lookup"><span data-stu-id="05425-356">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="05425-357">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="05425-357">Typed clients</span></span>

<span data-ttu-id="05425-358">具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="05425-358">Typed clients:</span></span>

* <span data-ttu-id="05425-359">提供與具名用戶端相同的功能，而不需使用字串作為索引鍵。</span><span class="sxs-lookup"><span data-stu-id="05425-359">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="05425-360">取用用戶端時提供 IntelliSense 和編譯器說明。</span><span class="sxs-lookup"><span data-stu-id="05425-360">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="05425-361">提供單一位置來設定特定的 `HttpClient` 並與其互動。</span><span class="sxs-lookup"><span data-stu-id="05425-361">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="05425-362">例如，單一的具型別用戶端可能用於單一的後端端點，並封裝處理該端點的所有邏輯。</span><span class="sxs-lookup"><span data-stu-id="05425-362">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="05425-363">使用 DI 且可在應用程式中需要之處插入。</span><span class="sxs-lookup"><span data-stu-id="05425-363">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="05425-364">具型別用戶端會接受其程式化中的 `HttpClient` 參數：</span><span class="sxs-lookup"><span data-stu-id="05425-364">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="05425-365">在上述程式碼中，組態會移到具型別用戶端。</span><span class="sxs-lookup"><span data-stu-id="05425-365">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="05425-366">`HttpClient` 物件會公開為公用屬性。</span><span class="sxs-lookup"><span data-stu-id="05425-366">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="05425-367">您可定義 API 特定的方法，其公開 `HttpClient` 功能。</span><span class="sxs-lookup"><span data-stu-id="05425-367">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="05425-368">`GetAspNetDocsIssues` 方法會封裝從 GitHub 存放庫查詢和剖析最新開啟問題所需的程式碼。</span><span class="sxs-lookup"><span data-stu-id="05425-368">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="05425-369">若要註冊具型別用戶端，泛型 <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> 擴充方法可用於 `Startup.ConfigureServices` 內，並指定具型別用戶端類別：</span><span class="sxs-lookup"><span data-stu-id="05425-369">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="05425-370">具型別用戶端會向 DI 註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="05425-370">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="05425-371">具型別用戶端可以直接插入並使用：</span><span class="sxs-lookup"><span data-stu-id="05425-371">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="05425-372">想要的話，具型別用戶端的組態可以在 `Startup.ConfigureServices` 中註冊時指定，而不是在具型別用戶端的建構函式中：</span><span class="sxs-lookup"><span data-stu-id="05425-372">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="05425-373">可以將 `HttpClient` 完全封裝在具型別用戶端內。</span><span class="sxs-lookup"><span data-stu-id="05425-373">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="05425-374">可以提供在內部呼叫 `HttpClient` 執行個體的公用方法，而不將它公開為屬性。</span><span class="sxs-lookup"><span data-stu-id="05425-374">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="05425-375">在上述程式碼，`HttpClient` 儲存為私用欄位。</span><span class="sxs-lookup"><span data-stu-id="05425-375">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="05425-376">進行外部呼叫的所有存取都會經歷 `GetRepos` 方法。</span><span class="sxs-lookup"><span data-stu-id="05425-376">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="05425-377">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="05425-377">Generated clients</span></span>

<span data-ttu-id="05425-378">`IHttpClientFactory` 可和其他協力廠商程式庫一起使用，例如 [Refit](https://github.com/paulcbetts/refit)。</span><span class="sxs-lookup"><span data-stu-id="05425-378">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="05425-379">Refit 是適用於 .NET 的 REST 程式庫。</span><span class="sxs-lookup"><span data-stu-id="05425-379">Refit is a REST library for .NET.</span></span> <span data-ttu-id="05425-380">它將 REST API 轉換為即時介面。</span><span class="sxs-lookup"><span data-stu-id="05425-380">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="05425-381">介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。</span><span class="sxs-lookup"><span data-stu-id="05425-381">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="05425-382">定義介面及回覆來代表外部 API 和其回應：</span><span class="sxs-lookup"><span data-stu-id="05425-382">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="05425-383">可以新增具型別用戶端，使用 Refit 產生實作：</span><span class="sxs-lookup"><span data-stu-id="05425-383">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="05425-384">定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：</span><span class="sxs-lookup"><span data-stu-id="05425-384">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="05425-385">外寄要求中介軟體</span><span class="sxs-lookup"><span data-stu-id="05425-385">Outgoing request middleware</span></span>

<span data-ttu-id="05425-386">`HttpClient` 已經有委派可針對外寄 HTTP 要求連結在一起的處理常式的概念。</span><span class="sxs-lookup"><span data-stu-id="05425-386">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="05425-387">`IHttpClientFactory` 可讓您輕鬆地定義要套用於每個具名用戶端的處理常式。</span><span class="sxs-lookup"><span data-stu-id="05425-387">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="05425-388">它支援註冊和鏈結多個處理常式，以建置外寄要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="05425-388">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="05425-389">這些處理常式每個都可以在外寄要求之前和之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="05425-389">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="05425-390">此模式與 ASP.NET Core 中的輸入中介軟體管線相似。</span><span class="sxs-lookup"><span data-stu-id="05425-390">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="05425-391">模式提供一個機制來管理 HTTP 要求的跨領域關注，包括快取、錯誤處理、序列化和記錄。</span><span class="sxs-lookup"><span data-stu-id="05425-391">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="05425-392">若要建立處理常式，請定義衍生自 <xref:System.Net.Http.DelegatingHandler> 的類別。</span><span class="sxs-lookup"><span data-stu-id="05425-392">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="05425-393">覆寫 `SendAsync` 方法，以在將要求傳遞至管線中的下一個處理常式之前執行程式碼：</span><span class="sxs-lookup"><span data-stu-id="05425-393">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="05425-394">上述程式碼定義一個基本處理常式。</span><span class="sxs-lookup"><span data-stu-id="05425-394">The preceding code defines a basic handler.</span></span> <span data-ttu-id="05425-395">它會檢查以查看要求上是否已包含 `X-API-KEY` 標頭。</span><span class="sxs-lookup"><span data-stu-id="05425-395">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="05425-396">如果遺漏標頭，它可以避免 HTTP 呼叫，並傳回適當的回應。</span><span class="sxs-lookup"><span data-stu-id="05425-396">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="05425-397">在註冊期間，可以將一或多個處理常式新增至 `HttpClient`的設定。</span><span class="sxs-lookup"><span data-stu-id="05425-397">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="05425-398">這項工作是透過 <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder> 上的擴充方法完成。</span><span class="sxs-lookup"><span data-stu-id="05425-398">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="05425-399">在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。</span><span class="sxs-lookup"><span data-stu-id="05425-399">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="05425-400">`IHttpClientFactory` 會為每個處理常式建立個別的 DI 範圍。</span><span class="sxs-lookup"><span data-stu-id="05425-400">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="05425-401">處理常式可相依於任何範圍的服務。</span><span class="sxs-lookup"><span data-stu-id="05425-401">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="05425-402">處置處理常式時，會處置處理常式所相依的服務。</span><span class="sxs-lookup"><span data-stu-id="05425-402">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="05425-403">註冊之後，便可以呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>，並傳入處理常式的類型。</span><span class="sxs-lookup"><span data-stu-id="05425-403">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="05425-404">可以遵循應該執行的順序來註冊多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="05425-404">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="05425-405">每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：</span><span class="sxs-lookup"><span data-stu-id="05425-405">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="05425-406">使用下列其中一種方式來與訊息處理常式共用個別要求狀態：</span><span class="sxs-lookup"><span data-stu-id="05425-406">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="05425-407">使用 `HttpRequestMessage.Properties` 將資料傳遞到處理常式。</span><span class="sxs-lookup"><span data-stu-id="05425-407">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="05425-408">使用 `IHttpContextAccessor` 來存取目前的要求。</span><span class="sxs-lookup"><span data-stu-id="05425-408">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="05425-409">建立自訂 `AsyncLocal` 儲存體物件以傳遞資料。</span><span class="sxs-lookup"><span data-stu-id="05425-409">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="05425-410">使用 Polly 為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="05425-410">Use Polly-based handlers</span></span>

<span data-ttu-id="05425-411">`IHttpClientFactory` 整合受歡迎的協力廠商程式庫，稱為 [Polly](https://github.com/App-vNext/Polly)。</span><span class="sxs-lookup"><span data-stu-id="05425-411">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="05425-412">Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。</span><span class="sxs-lookup"><span data-stu-id="05425-412">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="05425-413">它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。</span><span class="sxs-lookup"><span data-stu-id="05425-413">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="05425-414">提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="05425-414">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="05425-415">Polly 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="05425-415">The Polly extensions:</span></span>

* <span data-ttu-id="05425-416">支援將以 Polly 為基礎的處理常式新增至用戶端。</span><span class="sxs-lookup"><span data-stu-id="05425-416">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="05425-417">可在安裝 [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet 套件後使用。</span><span class="sxs-lookup"><span data-stu-id="05425-417">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="05425-418">該套件並未包含在 ASP.NET Core 共用架構中。</span><span class="sxs-lookup"><span data-stu-id="05425-418">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="05425-419">處理暫時性錯誤</span><span class="sxs-lookup"><span data-stu-id="05425-419">Handle transient faults</span></span>

<span data-ttu-id="05425-420">大部分的錯誤發生在外部 HTTP 呼叫是暫時性的時候。</span><span class="sxs-lookup"><span data-stu-id="05425-420">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="05425-421">包含一個便利的擴充方法，稱為 `AddTransientHttpErrorPolicy`，它可允許定義原則來處理暫時性錯誤。</span><span class="sxs-lookup"><span data-stu-id="05425-421">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="05425-422">使用此延伸模組方法設定的原則，會處理 `HttpRequestException`、HTTP 5xx 回應和 HTTP 408 回應。</span><span class="sxs-lookup"><span data-stu-id="05425-422">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="05425-423">`AddTransientHttpErrorPolicy` 延伸模組可用於 `Startup.ConfigureServices` 內。</span><span class="sxs-lookup"><span data-stu-id="05425-423">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="05425-424">延伸模組能提供 `PolicyBuilder` 物件的存取，該物件已設定來處理代表可能暫時性錯誤的錯誤：</span><span class="sxs-lookup"><span data-stu-id="05425-424">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="05425-425">在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。</span><span class="sxs-lookup"><span data-stu-id="05425-425">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="05425-426">失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。</span><span class="sxs-lookup"><span data-stu-id="05425-426">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="05425-427">動態選取原則</span><span class="sxs-lookup"><span data-stu-id="05425-427">Dynamically select policies</span></span>

<span data-ttu-id="05425-428">有額外的擴充方法可用來新增 Polly 為基礎的處理常式。</span><span class="sxs-lookup"><span data-stu-id="05425-428">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="05425-429">其中一個這類延伸模組是 `AddPolicyHandler`，它有多個多載。</span><span class="sxs-lookup"><span data-stu-id="05425-429">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="05425-430">一個多載可讓您在定義要套用的原則時，檢查要求：</span><span class="sxs-lookup"><span data-stu-id="05425-430">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="05425-431">在上述程式碼中，如果外寄要求是 HTTP GET，就會套用 10 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="05425-431">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="05425-432">任何其他 HTTP 方法會使用 30 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="05425-432">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="05425-433">新增多個 Polly 處理常式</span><span class="sxs-lookup"><span data-stu-id="05425-433">Add multiple Polly handlers</span></span>

<span data-ttu-id="05425-434">通常會建立巢狀 Polly 原則，以提供增強的功能：</span><span class="sxs-lookup"><span data-stu-id="05425-434">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="05425-435">在上述範例中，會新增兩個處理常式。</span><span class="sxs-lookup"><span data-stu-id="05425-435">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="05425-436">第一個使用 `AddTransientHttpErrorPolicy` 延伸模組來新增重試原則。</span><span class="sxs-lookup"><span data-stu-id="05425-436">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="05425-437">失敗的要求會重試最多三次。</span><span class="sxs-lookup"><span data-stu-id="05425-437">Failed requests are retried up to three times.</span></span> <span data-ttu-id="05425-438">第二個 `AddTransientHttpErrorPolicy` 呼叫會新增斷路器原則。</span><span class="sxs-lookup"><span data-stu-id="05425-438">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="05425-439">如果循序發生五次失敗的嘗試，進一步的外部要求會遭到封鎖 30 秒。</span><span class="sxs-lookup"><span data-stu-id="05425-439">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="05425-440">斷路器原則可設定狀態。</span><span class="sxs-lookup"><span data-stu-id="05425-440">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="05425-441">透過此用戶端的所有呼叫都會共用相同的線路狀態。</span><span class="sxs-lookup"><span data-stu-id="05425-441">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="05425-442">從 Polly 登錄新增原則</span><span class="sxs-lookup"><span data-stu-id="05425-442">Add policies from the Polly registry</span></span>

<span data-ttu-id="05425-443">管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。</span><span class="sxs-lookup"><span data-stu-id="05425-443">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="05425-444">提供了擴充方法，可以使用來自登錄的原則新增處理常式：</span><span class="sxs-lookup"><span data-stu-id="05425-444">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="05425-445">在上述程式碼中，當 `PolicyRegistry` 新增至 `ServiceCollection` 時，註冊了兩個原則。</span><span class="sxs-lookup"><span data-stu-id="05425-445">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="05425-446">為了使用來自登錄的原則，使用了 `AddPolicyHandlerFromRegistry` 方法，並傳遞要套用的原則名稱。</span><span class="sxs-lookup"><span data-stu-id="05425-446">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="05425-447">關於 `IHttpClientFactory` Polly 整合的詳細資訊，可以在 [Polly Wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory) 上找到。</span><span class="sxs-lookup"><span data-stu-id="05425-447">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="05425-448">HttpClient 和存留期管理</span><span class="sxs-lookup"><span data-stu-id="05425-448">HttpClient and lifetime management</span></span>

<span data-ttu-id="05425-449">每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回新的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="05425-449">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="05425-450">每個具名用戶端都有一個 <xref:System.Net.Http.HttpMessageHandler>。</span><span class="sxs-lookup"><span data-stu-id="05425-450">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="05425-451">處理站會管理 `HttpMessageHandler` 執行個體的存留期。</span><span class="sxs-lookup"><span data-stu-id="05425-451">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="05425-452">`IHttpClientFactory` 會將處理站所建立的 `HttpMessageHandler` 執行個體放入集區以減少資源耗用量。</span><span class="sxs-lookup"><span data-stu-id="05425-452">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="05425-453">建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。</span><span class="sxs-lookup"><span data-stu-id="05425-453">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="05425-454">將處理常式放入集區非常實用，因為處理常式通常會管理自己專屬的底層 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="05425-454">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="05425-455">建立比所需數目更多的處理常式，可能會導致連線延遲。</span><span class="sxs-lookup"><span data-stu-id="05425-455">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="05425-456">有些處理常式也會保持連線無限期地開啟，這可能導致處理常式無法對 DNS 變更回應。</span><span class="sxs-lookup"><span data-stu-id="05425-456">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="05425-457">預設處理常式存留時間為兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="05425-457">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="05425-458">可以針對每個具名用戶端覆寫預設值。</span><span class="sxs-lookup"><span data-stu-id="05425-458">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="05425-459">若要覆寫它，請在建立用戶端時所傳回的 `IHttpClientBuilder` 上呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*>：</span><span class="sxs-lookup"><span data-stu-id="05425-459">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="05425-460">不需要處置用戶端。</span><span class="sxs-lookup"><span data-stu-id="05425-460">Disposal of the client isn't required.</span></span> <span data-ttu-id="05425-461">處置會取消傳出的要求，並保證指定的 `HttpClient` 執行個體在呼叫 <xref:System.IDisposable.Dispose*> 之後無法使用。</span><span class="sxs-lookup"><span data-stu-id="05425-461">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="05425-462">`IHttpClientFactory` 會追蹤並處置 `HttpClient` 執行個體使用的資源。</span><span class="sxs-lookup"><span data-stu-id="05425-462">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="05425-463">`HttpClient` 執行個體通常可視為 .NET 物件，不需要處置。</span><span class="sxs-lookup"><span data-stu-id="05425-463">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="05425-464">在開始使用 `IHttpClientFactory` 之前，讓單一 `HttpClient` 執行個體維持一段較長的時間，是很常使用的模式。</span><span class="sxs-lookup"><span data-stu-id="05425-464">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="05425-465">在移轉到 `IHttpClientFactory` 之後，就不再需要此模式。</span><span class="sxs-lookup"><span data-stu-id="05425-465">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="05425-466">IHttpClientFactory 的替代方案</span><span class="sxs-lookup"><span data-stu-id="05425-466">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="05425-467">在啟用 DI 的應用程式中使用 `IHttpClientFactory` 可避免：</span><span class="sxs-lookup"><span data-stu-id="05425-467">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="05425-468">藉由共用 `HttpMessageHandler` 實例的資源耗盡問題。</span><span class="sxs-lookup"><span data-stu-id="05425-468">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="05425-469">以固定間隔迴圈 `HttpMessageHandler` 實例的過時 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="05425-469">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="05425-470">有其他方法可以使用長期 <xref:System.Net.Http.SocketsHttpHandler> 實例來解決上述問題。</span><span class="sxs-lookup"><span data-stu-id="05425-470">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="05425-471">當應用程式啟動時，建立 `SocketsHttpHandler` 的實例，並在應用程式的生命週期中使用它。</span><span class="sxs-lookup"><span data-stu-id="05425-471">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="05425-472">根據 DNS 重新整理時間，將 <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> 設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="05425-472">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="05425-473">視需要使用 `new HttpClient(handler, disposeHandler: false)` 建立 `HttpClient` 實例。</span><span class="sxs-lookup"><span data-stu-id="05425-473">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="05425-474">上述方法可解決 `IHttpClientFactory` 以類似的方式解決的資源管理問題。</span><span class="sxs-lookup"><span data-stu-id="05425-474">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="05425-475">`SocketsHttpHandler` 會共用 `HttpClient` 實例間的連接。</span><span class="sxs-lookup"><span data-stu-id="05425-475">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="05425-476">此共用可防止通訊端耗盡。</span><span class="sxs-lookup"><span data-stu-id="05425-476">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="05425-477">`SocketsHttpHandler` 會根據 `PooledConnectionLifetime` 迴圈連接，以避免過時的 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="05425-477">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="05425-478">Cookies</span><span class="sxs-lookup"><span data-stu-id="05425-478">Cookies</span></span>

<span data-ttu-id="05425-479">集區 `HttpMessageHandler` 實例會導致共用 `CookieContainer` 物件。</span><span class="sxs-lookup"><span data-stu-id="05425-479">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="05425-480">意外的 `CookieContainer` 物件共用通常會導致不正確的程式碼。</span><span class="sxs-lookup"><span data-stu-id="05425-480">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="05425-481">針對需要 cookie 的應用程式，請考慮下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="05425-481">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="05425-482">停用自動 cookie 處理</span><span class="sxs-lookup"><span data-stu-id="05425-482">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="05425-483">避免 `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="05425-483">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="05425-484">呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 以停用自動的 cookie 處理：</span><span class="sxs-lookup"><span data-stu-id="05425-484">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="05425-485">記錄</span><span class="sxs-lookup"><span data-stu-id="05425-485">Logging</span></span>

<span data-ttu-id="05425-486">透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="05425-486">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="05425-487">在記錄設定中啟用適當的資訊層級，以查看預設記錄檔訊息。</span><span class="sxs-lookup"><span data-stu-id="05425-487">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="05425-488">額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。</span><span class="sxs-lookup"><span data-stu-id="05425-488">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="05425-489">用於每個用戶端的記錄檔分類包含用戶端的名稱。</span><span class="sxs-lookup"><span data-stu-id="05425-489">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="05425-490">例如，名為 *MyNamedClient* 的用戶端會記錄分類為 `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler` 的訊息。</span><span class="sxs-lookup"><span data-stu-id="05425-490">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="05425-491">後面加上 *LogicalHandler* 的訊息發生在要求處理常式管線之外。</span><span class="sxs-lookup"><span data-stu-id="05425-491">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="05425-492">在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。</span><span class="sxs-lookup"><span data-stu-id="05425-492">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="05425-493">在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。</span><span class="sxs-lookup"><span data-stu-id="05425-493">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="05425-494">記錄也會發生在要求處理常式管線之內。</span><span class="sxs-lookup"><span data-stu-id="05425-494">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="05425-495">在 *MyNamedClient* 範例中，那些訊息是針對記錄檔分類 `System.Net.Http.HttpClient.MyNamedClient.ClientHandler` 而記錄。</span><span class="sxs-lookup"><span data-stu-id="05425-495">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="05425-496">對於要求，這是發生在所有其他處理常式都已執行之後，並且緊接在網路上傳送要求之前。</span><span class="sxs-lookup"><span data-stu-id="05425-496">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="05425-497">在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。</span><span class="sxs-lookup"><span data-stu-id="05425-497">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="05425-498">在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。</span><span class="sxs-lookup"><span data-stu-id="05425-498">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="05425-499">例如，這可能包括要求標頭的變更，或是回應狀態碼的變更。</span><span class="sxs-lookup"><span data-stu-id="05425-499">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="05425-500">在記錄分類中包含用戶端的名稱，可讓您在需要時進行特定具名用戶端的記錄檔篩選。</span><span class="sxs-lookup"><span data-stu-id="05425-500">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="05425-501">設定 HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="05425-501">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="05425-502">可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。</span><span class="sxs-lookup"><span data-stu-id="05425-502">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="05425-503">新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。</span><span class="sxs-lookup"><span data-stu-id="05425-503">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="05425-504"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 擴充方法可以用來定義委派。</span><span class="sxs-lookup"><span data-stu-id="05425-504">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="05425-505">委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：</span><span class="sxs-lookup"><span data-stu-id="05425-505">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="05425-506">在主控台應用程式中使用 IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="05425-506">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="05425-507">在主控台應用程式中，將下列套件參考新增至專案：</span><span class="sxs-lookup"><span data-stu-id="05425-507">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="05425-508">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="05425-508">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="05425-509">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="05425-509">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="05425-510">在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="05425-510">In the following example:</span></span>

* <span data-ttu-id="05425-511"><xref:System.Net.Http.IHttpClientFactory> 已在[泛型主機的](xref:fundamentals/host/generic-host)服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="05425-511"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="05425-512">`MyService` 會從服務建立用戶端 Factory 執行個體，其可用來建立 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="05425-512">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="05425-513">`HttpClient` 會用來擷取網頁。</span><span class="sxs-lookup"><span data-stu-id="05425-513">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="05425-514">`Main` 會建立範圍來執行服務的 `GetPage` 方法，並將網頁內容的前 500 個字元寫入至主控台。</span><span class="sxs-lookup"><span data-stu-id="05425-514">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="05425-515">其他資源</span><span class="sxs-lookup"><span data-stu-id="05425-515">Additional resources</span></span>

* [<span data-ttu-id="05425-516">使用 HttpClientFactory 實作復原 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="05425-516">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="05425-517">使用 HttpClientFactory 和 Polly 原則以指數輪詢實作 HTTP 呼叫重試</span><span class="sxs-lookup"><span data-stu-id="05425-517">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="05425-518">實作斷路器模式</span><span class="sxs-lookup"><span data-stu-id="05425-518">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="05425-519">作者：[Glenn Condron](https://github.com/glennc)、[Ryan Nowak](https://github.com/rynowak) 和 [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="05425-519">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="05425-520"><xref:System.Net.Http.IHttpClientFactory> 可以註冊及用來在應用程式中設定和建立 <xref:System.Net.Http.HttpClient> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="05425-520">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="05425-521">它提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="05425-521">It offers the following benefits:</span></span>

* <span data-ttu-id="05425-522">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="05425-522">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="05425-523">例如，可註冊及設定 *github* 用戶端，來存取 [GitHub](https://github.com/)。</span><span class="sxs-lookup"><span data-stu-id="05425-523">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="05425-524">預設用戶端可以註冊用於其他用途。</span><span class="sxs-lookup"><span data-stu-id="05425-524">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="05425-525">透過委派 `HttpClient` 中的處理常式來撰寫外寄中介軟體的概念，並提供延伸模組以便 Polly 架構中介軟體利用外寄中介軟體。</span><span class="sxs-lookup"><span data-stu-id="05425-525">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="05425-526">管理基礎 `HttpClientMessageHandler` 執行個體的共用和存留期，以避免在手動管理 `HttpClient` 存留期時，發生的常見 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="05425-526">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="05425-527">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="05425-527">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="05425-528">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="05425-528">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="05425-529">必要條件：</span><span class="sxs-lookup"><span data-stu-id="05425-529">Prerequisites</span></span>

<span data-ttu-id="05425-530">以 .NET Framework 為目標的專案，需要安裝 [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="05425-530">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="05425-531">以 .NET Core 為目標且參考 [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) 的專案，已包含 `Microsoft.Extensions.Http` 套件。</span><span class="sxs-lookup"><span data-stu-id="05425-531">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="05425-532">耗用模式</span><span class="sxs-lookup"><span data-stu-id="05425-532">Consumption patterns</span></span>

<span data-ttu-id="05425-533">有數種方式可將 `IHttpClientFactory` 用於應用程式：</span><span class="sxs-lookup"><span data-stu-id="05425-533">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="05425-534">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="05425-534">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="05425-535">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="05425-535">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="05425-536">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="05425-536">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="05425-537">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="05425-537">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="05425-538">它們全都不會嚴格優先於另一個。</span><span class="sxs-lookup"><span data-stu-id="05425-538">None of them are strictly superior to another.</span></span> <span data-ttu-id="05425-539">最好的方法取決於應用程式的條件約束。</span><span class="sxs-lookup"><span data-stu-id="05425-539">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="05425-540">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="05425-540">Basic usage</span></span>

<span data-ttu-id="05425-541">`IHttpClientFactory` 可以藉由在 `Startup.ConfigureServices` 方法內的 `IServiceCollection` 上呼叫 `AddHttpClient` 擴充方法來註冊。</span><span class="sxs-lookup"><span data-stu-id="05425-541">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="05425-542">註冊之後，程式碼可以在可使用[相依性插入 (DI)](xref:fundamentals/dependency-injection) 插入服務的任何位置，接受 `IHttpClientFactory`。</span><span class="sxs-lookup"><span data-stu-id="05425-542">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="05425-543">`IHttpClientFactory` 可以用來建立 `HttpClient` 實例：</span><span class="sxs-lookup"><span data-stu-id="05425-543">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="05425-544">以這種方式使用 `IHttpClientFactory` 是重構現有應用程式的好方法。</span><span class="sxs-lookup"><span data-stu-id="05425-544">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="05425-545">它對 `HttpClient` 的使用方式沒有任何影響。</span><span class="sxs-lookup"><span data-stu-id="05425-545">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="05425-546">在目前建立 `HttpClient` 執行個體的位置，將那些項目取代為呼叫 <xref:System.Net.Http.IHttpClientFactory.CreateClient*>。</span><span class="sxs-lookup"><span data-stu-id="05425-546">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="05425-547">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="05425-547">Named clients</span></span>

<span data-ttu-id="05425-548">如果應用程式需要使用多個不同的 `HttpClient`，且每個都有不同的設定，可以選擇使用**具名用戶端**。</span><span class="sxs-lookup"><span data-stu-id="05425-548">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="05425-549">具名 `HttpClient` 的組態可以在 `Startup.ConfigureServices` 中註冊時指定。</span><span class="sxs-lookup"><span data-stu-id="05425-549">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="05425-550">在上述程式碼中，會呼叫 `AddHttpClient` 並提供名稱 *github*。</span><span class="sxs-lookup"><span data-stu-id="05425-550">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="05425-551">此用戶端已套用一些預設組態&mdash;即使用 GitHub API 所需的基底位址和兩個標頭。</span><span class="sxs-lookup"><span data-stu-id="05425-551">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="05425-552">每次呼叫 `CreateClient` 時，會建立 `HttpClient` 的新執行個體並呼叫組態動作。</span><span class="sxs-lookup"><span data-stu-id="05425-552">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="05425-553">若要使用具名用戶端，可以傳遞字串參數至 `CreateClient`。</span><span class="sxs-lookup"><span data-stu-id="05425-553">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="05425-554">指定要建立之用戶端的名稱：</span><span class="sxs-lookup"><span data-stu-id="05425-554">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="05425-555">在上述程式碼中，要求不需要指定主機名稱。</span><span class="sxs-lookup"><span data-stu-id="05425-555">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="05425-556">它可以只傳遞路徑，因為已使用為用戶端設定的基底位址。</span><span class="sxs-lookup"><span data-stu-id="05425-556">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="05425-557">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="05425-557">Typed clients</span></span>

<span data-ttu-id="05425-558">具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="05425-558">Typed clients:</span></span>

* <span data-ttu-id="05425-559">提供與具名用戶端相同的功能，而不需使用字串作為索引鍵。</span><span class="sxs-lookup"><span data-stu-id="05425-559">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="05425-560">取用用戶端時提供 IntelliSense 和編譯器說明。</span><span class="sxs-lookup"><span data-stu-id="05425-560">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="05425-561">提供單一位置來設定特定的 `HttpClient` 並與其互動。</span><span class="sxs-lookup"><span data-stu-id="05425-561">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="05425-562">例如，單一的具型別用戶端可能用於單一的後端端點，並封裝處理該端點的所有邏輯。</span><span class="sxs-lookup"><span data-stu-id="05425-562">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="05425-563">使用 DI 且可在應用程式中需要之處插入。</span><span class="sxs-lookup"><span data-stu-id="05425-563">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="05425-564">具型別用戶端會接受其程式化中的 `HttpClient` 參數：</span><span class="sxs-lookup"><span data-stu-id="05425-564">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="05425-565">在上述程式碼中，組態會移到具型別用戶端。</span><span class="sxs-lookup"><span data-stu-id="05425-565">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="05425-566">`HttpClient` 物件會公開為公用屬性。</span><span class="sxs-lookup"><span data-stu-id="05425-566">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="05425-567">您可定義 API 特定的方法，其公開 `HttpClient` 功能。</span><span class="sxs-lookup"><span data-stu-id="05425-567">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="05425-568">`GetAspNetDocsIssues` 方法會封裝從 GitHub 存放庫查詢和剖析最新開啟問題所需的程式碼。</span><span class="sxs-lookup"><span data-stu-id="05425-568">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="05425-569">若要註冊具型別用戶端，泛型 <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> 擴充方法可用於 `Startup.ConfigureServices` 內，並指定具型別用戶端類別：</span><span class="sxs-lookup"><span data-stu-id="05425-569">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="05425-570">具型別用戶端會向 DI 註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="05425-570">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="05425-571">具型別用戶端可以直接插入並使用：</span><span class="sxs-lookup"><span data-stu-id="05425-571">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="05425-572">想要的話，具型別用戶端的組態可以在 `Startup.ConfigureServices` 中註冊時指定，而不是在具型別用戶端的建構函式中：</span><span class="sxs-lookup"><span data-stu-id="05425-572">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="05425-573">可以將 `HttpClient` 完全封裝在具型別用戶端內。</span><span class="sxs-lookup"><span data-stu-id="05425-573">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="05425-574">可以提供在內部呼叫 `HttpClient` 執行個體的公用方法，而不將它公開為屬性。</span><span class="sxs-lookup"><span data-stu-id="05425-574">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="05425-575">在上述程式碼，`HttpClient` 儲存為私用欄位。</span><span class="sxs-lookup"><span data-stu-id="05425-575">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="05425-576">進行外部呼叫的所有存取都會經歷 `GetRepos` 方法。</span><span class="sxs-lookup"><span data-stu-id="05425-576">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="05425-577">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="05425-577">Generated clients</span></span>

<span data-ttu-id="05425-578">`IHttpClientFactory` 可和其他協力廠商程式庫一起使用，例如 [Refit](https://github.com/paulcbetts/refit)。</span><span class="sxs-lookup"><span data-stu-id="05425-578">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="05425-579">Refit 是適用於 .NET 的 REST 程式庫。</span><span class="sxs-lookup"><span data-stu-id="05425-579">Refit is a REST library for .NET.</span></span> <span data-ttu-id="05425-580">它將 REST API 轉換為即時介面。</span><span class="sxs-lookup"><span data-stu-id="05425-580">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="05425-581">介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。</span><span class="sxs-lookup"><span data-stu-id="05425-581">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="05425-582">定義介面及回覆來代表外部 API 和其回應：</span><span class="sxs-lookup"><span data-stu-id="05425-582">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="05425-583">可以新增具型別用戶端，使用 Refit 產生實作：</span><span class="sxs-lookup"><span data-stu-id="05425-583">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="05425-584">定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：</span><span class="sxs-lookup"><span data-stu-id="05425-584">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="05425-585">外寄要求中介軟體</span><span class="sxs-lookup"><span data-stu-id="05425-585">Outgoing request middleware</span></span>

<span data-ttu-id="05425-586">`HttpClient` 已經有委派可針對外寄 HTTP 要求連結在一起的處理常式的概念。</span><span class="sxs-lookup"><span data-stu-id="05425-586">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="05425-587">`IHttpClientFactory` 可讓您輕鬆地定義要套用於每個具名用戶端的處理常式。</span><span class="sxs-lookup"><span data-stu-id="05425-587">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="05425-588">它支援註冊和鏈結多個處理常式，以建置外寄要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="05425-588">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="05425-589">這些處理常式每個都可以在外寄要求之前和之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="05425-589">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="05425-590">此模式與 ASP.NET Core 中的輸入中介軟體管線相似。</span><span class="sxs-lookup"><span data-stu-id="05425-590">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="05425-591">模式提供一個機制來管理 HTTP 要求的跨領域關注，包括快取、錯誤處理、序列化和記錄。</span><span class="sxs-lookup"><span data-stu-id="05425-591">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="05425-592">若要建立處理常式，請定義衍生自 <xref:System.Net.Http.DelegatingHandler> 的類別。</span><span class="sxs-lookup"><span data-stu-id="05425-592">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="05425-593">覆寫 `SendAsync` 方法，以在將要求傳遞至管線中的下一個處理常式之前執行程式碼：</span><span class="sxs-lookup"><span data-stu-id="05425-593">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="05425-594">上述程式碼定義一個基本處理常式。</span><span class="sxs-lookup"><span data-stu-id="05425-594">The preceding code defines a basic handler.</span></span> <span data-ttu-id="05425-595">它會檢查以查看要求上是否已包含 `X-API-KEY` 標頭。</span><span class="sxs-lookup"><span data-stu-id="05425-595">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="05425-596">如果遺漏標頭，它可以避免 HTTP 呼叫，並傳回適當的回應。</span><span class="sxs-lookup"><span data-stu-id="05425-596">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="05425-597">在註冊期間，可以將一或多個處理常式新增至 `HttpClient`的設定。</span><span class="sxs-lookup"><span data-stu-id="05425-597">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="05425-598">這項工作是透過 <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder> 上的擴充方法完成。</span><span class="sxs-lookup"><span data-stu-id="05425-598">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="05425-599">在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。</span><span class="sxs-lookup"><span data-stu-id="05425-599">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="05425-600">處理常式**必須**在 DI 中註冊為暫時性服務，無限定範圍。</span><span class="sxs-lookup"><span data-stu-id="05425-600">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="05425-601">如果處理常式已註冊為範圍服務，而且處理常式所相依的任何服務都是可處置的：</span><span class="sxs-lookup"><span data-stu-id="05425-601">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="05425-602">處理常式的服務可能會在處理常式超出範圍之前加以處置。</span><span class="sxs-lookup"><span data-stu-id="05425-602">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="05425-603">已處置的處理常式服務會導致處理常式失敗。</span><span class="sxs-lookup"><span data-stu-id="05425-603">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="05425-604">註冊之後，便可以呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>，並傳入處理常式類型。</span><span class="sxs-lookup"><span data-stu-id="05425-604">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="05425-605">可以遵循應該執行的順序來註冊多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="05425-605">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="05425-606">每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：</span><span class="sxs-lookup"><span data-stu-id="05425-606">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="05425-607">使用下列其中一種方式來與訊息處理常式共用個別要求狀態：</span><span class="sxs-lookup"><span data-stu-id="05425-607">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="05425-608">使用 `HttpRequestMessage.Properties` 將資料傳遞到處理常式。</span><span class="sxs-lookup"><span data-stu-id="05425-608">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="05425-609">使用 `IHttpContextAccessor` 來存取目前的要求。</span><span class="sxs-lookup"><span data-stu-id="05425-609">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="05425-610">建立自訂 `AsyncLocal` 儲存體物件以傳遞資料。</span><span class="sxs-lookup"><span data-stu-id="05425-610">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="05425-611">使用 Polly 為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="05425-611">Use Polly-based handlers</span></span>

<span data-ttu-id="05425-612">`IHttpClientFactory` 整合受歡迎的協力廠商程式庫，稱為 [Polly](https://github.com/App-vNext/Polly)。</span><span class="sxs-lookup"><span data-stu-id="05425-612">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="05425-613">Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。</span><span class="sxs-lookup"><span data-stu-id="05425-613">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="05425-614">它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。</span><span class="sxs-lookup"><span data-stu-id="05425-614">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="05425-615">提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="05425-615">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="05425-616">Polly 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="05425-616">The Polly extensions:</span></span>

* <span data-ttu-id="05425-617">支援將以 Polly 為基礎的處理常式新增至用戶端。</span><span class="sxs-lookup"><span data-stu-id="05425-617">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="05425-618">可在安裝 [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet 套件後使用。</span><span class="sxs-lookup"><span data-stu-id="05425-618">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="05425-619">該套件並未包含在 ASP.NET Core 共用架構中。</span><span class="sxs-lookup"><span data-stu-id="05425-619">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="05425-620">處理暫時性錯誤</span><span class="sxs-lookup"><span data-stu-id="05425-620">Handle transient faults</span></span>

<span data-ttu-id="05425-621">大部分的錯誤發生在外部 HTTP 呼叫是暫時性的時候。</span><span class="sxs-lookup"><span data-stu-id="05425-621">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="05425-622">包含一個便利的擴充方法，稱為 `AddTransientHttpErrorPolicy`，它可允許定義原則來處理暫時性錯誤。</span><span class="sxs-lookup"><span data-stu-id="05425-622">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="05425-623">使用此延伸模組方法設定的原則，會處理 `HttpRequestException`、HTTP 5xx 回應和 HTTP 408 回應。</span><span class="sxs-lookup"><span data-stu-id="05425-623">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="05425-624">`AddTransientHttpErrorPolicy` 延伸模組可用於 `Startup.ConfigureServices` 內。</span><span class="sxs-lookup"><span data-stu-id="05425-624">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="05425-625">延伸模組能提供 `PolicyBuilder` 物件的存取，該物件已設定來處理代表可能暫時性錯誤的錯誤：</span><span class="sxs-lookup"><span data-stu-id="05425-625">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="05425-626">在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。</span><span class="sxs-lookup"><span data-stu-id="05425-626">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="05425-627">失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。</span><span class="sxs-lookup"><span data-stu-id="05425-627">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="05425-628">動態選取原則</span><span class="sxs-lookup"><span data-stu-id="05425-628">Dynamically select policies</span></span>

<span data-ttu-id="05425-629">有額外的擴充方法可用來新增 Polly 為基礎的處理常式。</span><span class="sxs-lookup"><span data-stu-id="05425-629">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="05425-630">其中一個這類延伸模組是 `AddPolicyHandler`，它有多個多載。</span><span class="sxs-lookup"><span data-stu-id="05425-630">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="05425-631">一個多載可讓您在定義要套用的原則時，檢查要求：</span><span class="sxs-lookup"><span data-stu-id="05425-631">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="05425-632">在上述程式碼中，如果外寄要求是 HTTP GET，就會套用 10 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="05425-632">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="05425-633">任何其他 HTTP 方法會使用 30 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="05425-633">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="05425-634">新增多個 Polly 處理常式</span><span class="sxs-lookup"><span data-stu-id="05425-634">Add multiple Polly handlers</span></span>

<span data-ttu-id="05425-635">通常會建立巢狀 Polly 原則，以提供增強的功能：</span><span class="sxs-lookup"><span data-stu-id="05425-635">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="05425-636">在上述範例中，會新增兩個處理常式。</span><span class="sxs-lookup"><span data-stu-id="05425-636">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="05425-637">第一個使用 `AddTransientHttpErrorPolicy` 延伸模組來新增重試原則。</span><span class="sxs-lookup"><span data-stu-id="05425-637">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="05425-638">失敗的要求會重試最多三次。</span><span class="sxs-lookup"><span data-stu-id="05425-638">Failed requests are retried up to three times.</span></span> <span data-ttu-id="05425-639">第二個 `AddTransientHttpErrorPolicy` 呼叫會新增斷路器原則。</span><span class="sxs-lookup"><span data-stu-id="05425-639">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="05425-640">如果循序發生五次失敗的嘗試，進一步的外部要求會遭到封鎖 30 秒。</span><span class="sxs-lookup"><span data-stu-id="05425-640">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="05425-641">斷路器原則可設定狀態。</span><span class="sxs-lookup"><span data-stu-id="05425-641">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="05425-642">透過此用戶端的所有呼叫都會共用相同的線路狀態。</span><span class="sxs-lookup"><span data-stu-id="05425-642">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="05425-643">從 Polly 登錄新增原則</span><span class="sxs-lookup"><span data-stu-id="05425-643">Add policies from the Polly registry</span></span>

<span data-ttu-id="05425-644">管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。</span><span class="sxs-lookup"><span data-stu-id="05425-644">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="05425-645">提供了擴充方法，可以使用來自登錄的原則新增處理常式：</span><span class="sxs-lookup"><span data-stu-id="05425-645">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="05425-646">在上述程式碼中，當 `PolicyRegistry` 新增至 `ServiceCollection` 時，註冊了兩個原則。</span><span class="sxs-lookup"><span data-stu-id="05425-646">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="05425-647">為了使用來自登錄的原則，使用了 `AddPolicyHandlerFromRegistry` 方法，並傳遞要套用的原則名稱。</span><span class="sxs-lookup"><span data-stu-id="05425-647">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="05425-648">關於 `IHttpClientFactory` Polly 整合的詳細資訊，可以在 [Polly Wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory) 上找到。</span><span class="sxs-lookup"><span data-stu-id="05425-648">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="05425-649">HttpClient 和存留期管理</span><span class="sxs-lookup"><span data-stu-id="05425-649">HttpClient and lifetime management</span></span>

<span data-ttu-id="05425-650">每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回新的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="05425-650">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="05425-651">每個具名用戶端都有一個 <xref:System.Net.Http.HttpMessageHandler>。</span><span class="sxs-lookup"><span data-stu-id="05425-651">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="05425-652">處理站會管理 `HttpMessageHandler` 執行個體的存留期。</span><span class="sxs-lookup"><span data-stu-id="05425-652">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="05425-653">`IHttpClientFactory` 會將處理站所建立的 `HttpMessageHandler` 執行個體放入集區以減少資源耗用量。</span><span class="sxs-lookup"><span data-stu-id="05425-653">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="05425-654">建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。</span><span class="sxs-lookup"><span data-stu-id="05425-654">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="05425-655">將處理常式放入集區非常實用，因為處理常式通常會管理自己專屬的底層 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="05425-655">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="05425-656">建立比所需數目更多的處理常式，可能會導致連線延遲。</span><span class="sxs-lookup"><span data-stu-id="05425-656">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="05425-657">有些處理常式也會保持連線無限期地開啟，這可能導致處理常式無法對 DNS 變更回應。</span><span class="sxs-lookup"><span data-stu-id="05425-657">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="05425-658">預設處理常式存留時間為兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="05425-658">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="05425-659">可以針對每個具名用戶端覆寫預設值。</span><span class="sxs-lookup"><span data-stu-id="05425-659">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="05425-660">若要覆寫它，請在建立用戶端時所傳回的 `IHttpClientBuilder` 上呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*>：</span><span class="sxs-lookup"><span data-stu-id="05425-660">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="05425-661">不需要處置用戶端。</span><span class="sxs-lookup"><span data-stu-id="05425-661">Disposal of the client isn't required.</span></span> <span data-ttu-id="05425-662">處置會取消傳出的要求，並保證指定的 `HttpClient` 執行個體在呼叫 <xref:System.IDisposable.Dispose*> 之後無法使用。</span><span class="sxs-lookup"><span data-stu-id="05425-662">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="05425-663">`IHttpClientFactory` 會追蹤並處置 `HttpClient` 執行個體使用的資源。</span><span class="sxs-lookup"><span data-stu-id="05425-663">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="05425-664">`HttpClient` 執行個體通常可視為 .NET 物件，不需要處置。</span><span class="sxs-lookup"><span data-stu-id="05425-664">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="05425-665">在開始使用 `IHttpClientFactory` 之前，讓單一 `HttpClient` 執行個體維持一段較長的時間，是很常使用的模式。</span><span class="sxs-lookup"><span data-stu-id="05425-665">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="05425-666">在移轉到 `IHttpClientFactory` 之後，就不再需要此模式。</span><span class="sxs-lookup"><span data-stu-id="05425-666">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="05425-667">IHttpClientFactory 的替代方案</span><span class="sxs-lookup"><span data-stu-id="05425-667">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="05425-668">在啟用 DI 的應用程式中使用 `IHttpClientFactory` 可避免：</span><span class="sxs-lookup"><span data-stu-id="05425-668">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="05425-669">藉由共用 `HttpMessageHandler` 實例的資源耗盡問題。</span><span class="sxs-lookup"><span data-stu-id="05425-669">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="05425-670">以固定間隔迴圈 `HttpMessageHandler` 實例的過時 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="05425-670">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="05425-671">有其他方法可以使用長期 <xref:System.Net.Http.SocketsHttpHandler> 實例來解決上述問題。</span><span class="sxs-lookup"><span data-stu-id="05425-671">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="05425-672">當應用程式啟動時，建立 `SocketsHttpHandler` 的實例，並在應用程式的生命週期中使用它。</span><span class="sxs-lookup"><span data-stu-id="05425-672">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="05425-673">根據 DNS 重新整理時間，將 <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> 設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="05425-673">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="05425-674">視需要使用 `new HttpClient(handler, disposeHandler: false)` 建立 `HttpClient` 實例。</span><span class="sxs-lookup"><span data-stu-id="05425-674">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="05425-675">上述方法可解決 `IHttpClientFactory` 以類似的方式解決的資源管理問題。</span><span class="sxs-lookup"><span data-stu-id="05425-675">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="05425-676">`SocketsHttpHandler` 會共用 `HttpClient` 實例間的連接。</span><span class="sxs-lookup"><span data-stu-id="05425-676">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="05425-677">此共用可防止通訊端耗盡。</span><span class="sxs-lookup"><span data-stu-id="05425-677">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="05425-678">`SocketsHttpHandler` 會根據 `PooledConnectionLifetime` 迴圈連接，以避免過時的 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="05425-678">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="05425-679">Cookies</span><span class="sxs-lookup"><span data-stu-id="05425-679">Cookies</span></span>

<span data-ttu-id="05425-680">集區 `HttpMessageHandler` 實例會導致共用 `CookieContainer` 物件。</span><span class="sxs-lookup"><span data-stu-id="05425-680">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="05425-681">意外的 `CookieContainer` 物件共用通常會導致不正確的程式碼。</span><span class="sxs-lookup"><span data-stu-id="05425-681">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="05425-682">針對需要 cookie 的應用程式，請考慮下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="05425-682">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="05425-683">停用自動 cookie 處理</span><span class="sxs-lookup"><span data-stu-id="05425-683">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="05425-684">避免 `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="05425-684">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="05425-685">呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 以停用自動的 cookie 處理：</span><span class="sxs-lookup"><span data-stu-id="05425-685">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="05425-686">記錄</span><span class="sxs-lookup"><span data-stu-id="05425-686">Logging</span></span>

<span data-ttu-id="05425-687">透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="05425-687">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="05425-688">在記錄設定中啟用適當的資訊層級，以查看預設記錄檔訊息。</span><span class="sxs-lookup"><span data-stu-id="05425-688">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="05425-689">額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。</span><span class="sxs-lookup"><span data-stu-id="05425-689">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="05425-690">用於每個用戶端的記錄檔分類包含用戶端的名稱。</span><span class="sxs-lookup"><span data-stu-id="05425-690">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="05425-691">例如，名為 *MyNamedClient* 的用戶端會記錄分類為 `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler` 的訊息。</span><span class="sxs-lookup"><span data-stu-id="05425-691">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="05425-692">後面加上 *LogicalHandler* 的訊息發生在要求處理常式管線之外。</span><span class="sxs-lookup"><span data-stu-id="05425-692">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="05425-693">在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。</span><span class="sxs-lookup"><span data-stu-id="05425-693">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="05425-694">在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。</span><span class="sxs-lookup"><span data-stu-id="05425-694">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="05425-695">記錄也會發生在要求處理常式管線之內。</span><span class="sxs-lookup"><span data-stu-id="05425-695">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="05425-696">在 *MyNamedClient* 範例中，那些訊息是針對記錄檔分類 `System.Net.Http.HttpClient.MyNamedClient.ClientHandler` 而記錄。</span><span class="sxs-lookup"><span data-stu-id="05425-696">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="05425-697">對於要求，這是發生在所有其他處理常式都已執行之後，並且緊接在網路上傳送要求之前。</span><span class="sxs-lookup"><span data-stu-id="05425-697">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="05425-698">在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。</span><span class="sxs-lookup"><span data-stu-id="05425-698">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="05425-699">在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。</span><span class="sxs-lookup"><span data-stu-id="05425-699">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="05425-700">例如，這可能包括要求標頭的變更，或是回應狀態碼的變更。</span><span class="sxs-lookup"><span data-stu-id="05425-700">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="05425-701">在記錄分類中包含用戶端的名稱，可讓您在需要時進行特定具名用戶端的記錄檔篩選。</span><span class="sxs-lookup"><span data-stu-id="05425-701">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="05425-702">設定 HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="05425-702">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="05425-703">可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。</span><span class="sxs-lookup"><span data-stu-id="05425-703">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="05425-704">新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。</span><span class="sxs-lookup"><span data-stu-id="05425-704">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="05425-705"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 擴充方法可以用來定義委派。</span><span class="sxs-lookup"><span data-stu-id="05425-705">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="05425-706">委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：</span><span class="sxs-lookup"><span data-stu-id="05425-706">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="05425-707">在主控台應用程式中使用 IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="05425-707">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="05425-708">在主控台應用程式中，將下列套件參考新增至專案：</span><span class="sxs-lookup"><span data-stu-id="05425-708">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="05425-709">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="05425-709">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="05425-710">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="05425-710">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="05425-711">在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="05425-711">In the following example:</span></span>

* <span data-ttu-id="05425-712"><xref:System.Net.Http.IHttpClientFactory> 已在[泛型主機的](xref:fundamentals/host/generic-host)服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="05425-712"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="05425-713">`MyService` 會從服務建立用戶端 Factory 執行個體，其可用來建立 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="05425-713">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="05425-714">`HttpClient` 會用來擷取網頁。</span><span class="sxs-lookup"><span data-stu-id="05425-714">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="05425-715">`Main` 會建立範圍來執行服務的 `GetPage` 方法，並將網頁內容的前 500 個字元寫入至主控台。</span><span class="sxs-lookup"><span data-stu-id="05425-715">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="header-propagation-middleware"></a><span data-ttu-id="05425-716">標頭傳播中介軟體</span><span class="sxs-lookup"><span data-stu-id="05425-716">Header propagation middleware</span></span>

<span data-ttu-id="05425-717">標頭傳播是一個支援的中介軟體，可將 HTTP 標頭從連入要求傳播到傳出的 HTTP 用戶端要求。</span><span class="sxs-lookup"><span data-stu-id="05425-717">Header propagation is a community supported middleware to propagate HTTP headers from the incoming request to the outgoing HTTP Client requests.</span></span> <span data-ttu-id="05425-718">若要使用標頭傳播：</span><span class="sxs-lookup"><span data-stu-id="05425-718">To use header propagation:</span></span>

* <span data-ttu-id="05425-719">參考[HeaderPropagation](https://www.nuget.org/packages/HeaderPropagation)套件的社區支援埠。</span><span class="sxs-lookup"><span data-stu-id="05425-719">Reference the community supported port of the package [HeaderPropagation](https://www.nuget.org/packages/HeaderPropagation).</span></span> <span data-ttu-id="05425-720">ASP.NET Core 3.1 和更新版本支援[AspNetCore. HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation)。</span><span class="sxs-lookup"><span data-stu-id="05425-720">ASP.NET Core 3.1 and later supports [Microsoft.AspNetCore.HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation).</span></span>

* <span data-ttu-id="05425-721">在 `Startup`中設定中介軟體和 `HttpClient`：</span><span class="sxs-lookup"><span data-stu-id="05425-721">Configure the middleware and `HttpClient` in `Startup`:</span></span>

  [!code-csharp[](http-requests/samples/2.x/Startup21.cs?highlight=5-9,25&name=snippet)]

* <span data-ttu-id="05425-722">用戶端會在輸出要求中包含已設定的標頭：</span><span class="sxs-lookup"><span data-stu-id="05425-722">The client includes the configured headers on outbound requests:</span></span>

  ```C#
  var client = clientFactory.CreateClient("MyForwardingClient");
  var response = client.GetAsync(...);
  ```

## <a name="additional-resources"></a><span data-ttu-id="05425-723">其他資源</span><span class="sxs-lookup"><span data-stu-id="05425-723">Additional resources</span></span>

* [<span data-ttu-id="05425-724">使用 HttpClientFactory 實作復原 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="05425-724">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="05425-725">使用 HttpClientFactory 和 Polly 原則以指數輪詢實作 HTTP 呼叫重試</span><span class="sxs-lookup"><span data-stu-id="05425-725">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="05425-726">實作斷路器模式</span><span class="sxs-lookup"><span data-stu-id="05425-726">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
