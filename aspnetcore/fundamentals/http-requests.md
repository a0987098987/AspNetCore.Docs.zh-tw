---
title: 在 ASP.NET Core 中使用 IHttpClientFactory 發出 HTTP 要求
author: stevejgordon
description: 深入了解在 ASP.NET Core 中使用 IHttpClientFactory 介面來管理邏輯 HttpClient 執行個體。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/09/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/http-requests
ms.openlocfilehash: ae33218d6944c62a08e677592ac0c66f9026b15f
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82766546"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="2f57e-103">在 ASP.NET Core 中使用 IHttpClientFactory 發出 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="2f57e-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2f57e-104">作者： [Glenn Condron](https://github.com/glennc)、 [Ryan Nowak](https://github.com/rynowak)、 [Steve Gordon](https://github.com/stevejgordon)、 [Rick Anderson](https://twitter.com/RickAndMSFT)和[Kirk Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="2f57e-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="2f57e-105"><xref:System.Net.Http.IHttpClientFactory> 可以註冊及用來在應用程式中設定和建立 <xref:System.Net.Http.HttpClient> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="2f57e-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="2f57e-106">`IHttpClientFactory`提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="2f57e-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="2f57e-107">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="2f57e-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="2f57e-108">例如，名為*github*的用戶端可以註冊並設定為存取[github](https://github.com/)。</span><span class="sxs-lookup"><span data-stu-id="2f57e-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="2f57e-109">預設用戶端可以註冊以進行一般存取。</span><span class="sxs-lookup"><span data-stu-id="2f57e-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="2f57e-110">透過委派中`HttpClient`的處理常式，制訂外寄中介軟體的概念。</span><span class="sxs-lookup"><span data-stu-id="2f57e-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="2f57e-111">提供 Polly 為基礎中介軟體的延伸模組，以利用中`HttpClient`的委派處理常式。</span><span class="sxs-lookup"><span data-stu-id="2f57e-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="2f57e-112">管理基礎`HttpClientMessageHandler`實例的共用和存留期。</span><span class="sxs-lookup"><span data-stu-id="2f57e-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="2f57e-113">自動管理可避免在手動管理`HttpClient`存留期時所發生的常見 DNS （網域名稱系統）問題。</span><span class="sxs-lookup"><span data-stu-id="2f57e-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="2f57e-114">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="2f57e-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="2f57e-115">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="2f57e-115">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="2f57e-116">本主題中的範例程式碼會<xref:System.Text.Json>使用來還原序列化 HTTP 回應中所傳回的 JSON 內容。</span><span class="sxs-lookup"><span data-stu-id="2f57e-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="2f57e-117">如需使用`Json.NET`和`ReadAsAsync<T>`的範例，請使用版本選取器來選取此主題的2.x 版。</span><span class="sxs-lookup"><span data-stu-id="2f57e-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="2f57e-118">耗用量模式</span><span class="sxs-lookup"><span data-stu-id="2f57e-118">Consumption patterns</span></span>

<span data-ttu-id="2f57e-119">有數種方式可將 `IHttpClientFactory` 用於應用程式：</span><span class="sxs-lookup"><span data-stu-id="2f57e-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="2f57e-120">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="2f57e-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="2f57e-121">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="2f57e-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="2f57e-122">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="2f57e-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="2f57e-123">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="2f57e-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="2f57e-124">最佳方法取決於應用程式的需求。</span><span class="sxs-lookup"><span data-stu-id="2f57e-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="2f57e-125">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="2f57e-125">Basic usage</span></span>

<span data-ttu-id="2f57e-126">`IHttpClientFactory`可以藉由呼叫`AddHttpClient`來註冊：</span><span class="sxs-lookup"><span data-stu-id="2f57e-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="2f57e-127">`IHttpClientFactory`可以使用相依性[插入（DI）](xref:fundamentals/dependency-injection)要求。</span><span class="sxs-lookup"><span data-stu-id="2f57e-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="2f57e-128">下列程式碼會`IHttpClientFactory`使用來建立`HttpClient`實例：</span><span class="sxs-lookup"><span data-stu-id="2f57e-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="2f57e-129">在`IHttpClientFactory`上述範例中使用 like，是重構現有應用程式的好方法。</span><span class="sxs-lookup"><span data-stu-id="2f57e-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="2f57e-130">它不會影響使用方式`HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="2f57e-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="2f57e-131">在現有應用`HttpClient`程式中建立實例的位置，使用對的呼叫來<xref:System.Net.Http.IHttpClientFactory.CreateClient*>取代這些專案。</span><span class="sxs-lookup"><span data-stu-id="2f57e-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="2f57e-132">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="2f57e-132">Named clients</span></span>

<span data-ttu-id="2f57e-133">在下列情況中，命名的用戶端是不錯的選擇：</span><span class="sxs-lookup"><span data-stu-id="2f57e-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="2f57e-134">應用程式需要有許多不同的`HttpClient`用途。</span><span class="sxs-lookup"><span data-stu-id="2f57e-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="2f57e-135">許多`HttpClient`都有不同的設定。</span><span class="sxs-lookup"><span data-stu-id="2f57e-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="2f57e-136">在中`Startup.ConfigureServices`的註冊`HttpClient`期間，可以指定名為的設定：</span><span class="sxs-lookup"><span data-stu-id="2f57e-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="2f57e-137">在上述程式碼中，用戶端是使用下列設定：</span><span class="sxs-lookup"><span data-stu-id="2f57e-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="2f57e-138">基底位址`https://api.github.com/`。</span><span class="sxs-lookup"><span data-stu-id="2f57e-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="2f57e-139">使用 GitHub API 時需要兩個標頭。</span><span class="sxs-lookup"><span data-stu-id="2f57e-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="2f57e-140">CreateClient</span><span class="sxs-lookup"><span data-stu-id="2f57e-140">CreateClient</span></span>

<span data-ttu-id="2f57e-141">每次<xref:System.Net.Http.IHttpClientFactory.CreateClient*>呼叫時：</span><span class="sxs-lookup"><span data-stu-id="2f57e-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="2f57e-142">建立的`HttpClient`新實例。</span><span class="sxs-lookup"><span data-stu-id="2f57e-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="2f57e-143">會呼叫設定動作。</span><span class="sxs-lookup"><span data-stu-id="2f57e-143">The configuration action is called.</span></span>

<span data-ttu-id="2f57e-144">若要建立名為的用戶端，請`CreateClient`將其名稱傳遞至：</span><span class="sxs-lookup"><span data-stu-id="2f57e-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="2f57e-145">在上述程式碼中，要求不需要指定主機名稱。</span><span class="sxs-lookup"><span data-stu-id="2f57e-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="2f57e-146">此程式碼只會傳遞路徑，因為會使用為用戶端設定的基底位址。</span><span class="sxs-lookup"><span data-stu-id="2f57e-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="2f57e-147">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="2f57e-147">Typed clients</span></span>

<span data-ttu-id="2f57e-148">具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="2f57e-148">Typed clients:</span></span>

* <span data-ttu-id="2f57e-149">提供與具名用戶端相同的功能，而不需使用字串作為索引鍵。</span><span class="sxs-lookup"><span data-stu-id="2f57e-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="2f57e-150">取用用戶端時提供 IntelliSense 和編譯器說明。</span><span class="sxs-lookup"><span data-stu-id="2f57e-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="2f57e-151">提供單一位置來設定特定的 `HttpClient` 並與其互動。</span><span class="sxs-lookup"><span data-stu-id="2f57e-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="2f57e-152">例如，可能會使用單一具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="2f57e-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="2f57e-153">適用于單一後端端點。</span><span class="sxs-lookup"><span data-stu-id="2f57e-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="2f57e-154">封裝處理端點的所有邏輯。</span><span class="sxs-lookup"><span data-stu-id="2f57e-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="2f57e-155">使用 DI，並可在應用程式中需要的位置插入。</span><span class="sxs-lookup"><span data-stu-id="2f57e-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="2f57e-156">具型別用戶端`HttpClient`會接受其函式中的參數：</span><span class="sxs-lookup"><span data-stu-id="2f57e-156">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="2f57e-157">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="2f57e-157">In the preceding code:</span></span>

* <span data-ttu-id="2f57e-158">設定會移到具型別用戶端。</span><span class="sxs-lookup"><span data-stu-id="2f57e-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="2f57e-159">`HttpClient` 物件會公開為公用屬性。</span><span class="sxs-lookup"><span data-stu-id="2f57e-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="2f57e-160">可以建立可公開`HttpClient`功能的 API 特定方法。</span><span class="sxs-lookup"><span data-stu-id="2f57e-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="2f57e-161">例如， `GetAspNetDocsIssues`方法會封裝程式碼以取得未解決的問題。</span><span class="sxs-lookup"><span data-stu-id="2f57e-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="2f57e-162">下列程式碼會<xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*>呼叫`Startup.ConfigureServices`中的來註冊具型別用戶端類別：</span><span class="sxs-lookup"><span data-stu-id="2f57e-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="2f57e-163">具型別用戶端會向 DI 註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="2f57e-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="2f57e-164">在上述程式碼中`AddHttpClient` ， `GitHubService`會將註冊為暫時性服務。</span><span class="sxs-lookup"><span data-stu-id="2f57e-164">In the preceding code, `AddHttpClient` registers `GitHubService` as a transient service.</span></span> <span data-ttu-id="2f57e-165">此註冊會使用 factory 方法來執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="2f57e-165">This registration uses a factory method to:</span></span>

1. <span data-ttu-id="2f57e-166">建立 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="2f57e-166">Create an instance of `HttpClient`.</span></span>
1. <span data-ttu-id="2f57e-167">建立的實例`GitHubService`，並`HttpClient`將的實例傳入其函式。</span><span class="sxs-lookup"><span data-stu-id="2f57e-167">Create an instance of `GitHubService`, passing in the instance of `HttpClient` to its constructor.</span></span>

<span data-ttu-id="2f57e-168">具型別用戶端可以直接插入並使用：</span><span class="sxs-lookup"><span data-stu-id="2f57e-168">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="2f57e-169">在中`Startup.ConfigureServices`的註冊期間，可以指定具型別用戶端的設定，而不是在具型別用戶端的函式中：</span><span class="sxs-lookup"><span data-stu-id="2f57e-169">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="2f57e-170">`HttpClient`可以封裝在具型別用戶端中。</span><span class="sxs-lookup"><span data-stu-id="2f57e-170">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="2f57e-171">請定義在內部呼叫實例的`HttpClient`方法，而不是將它公開為屬性：</span><span class="sxs-lookup"><span data-stu-id="2f57e-171">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="2f57e-172">在上述`HttpClient`程式碼中，會儲存在私用欄位中。</span><span class="sxs-lookup"><span data-stu-id="2f57e-172">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="2f57e-173">的存取`HttpClient`是透過公用`GetRepos`方法。</span><span class="sxs-lookup"><span data-stu-id="2f57e-173">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="2f57e-174">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="2f57e-174">Generated clients</span></span>

<span data-ttu-id="2f57e-175">`IHttpClientFactory`可以與協力廠商程式庫搭配使用，例如重新[調整。](https://github.com/paulcbetts/refit)</span><span class="sxs-lookup"><span data-stu-id="2f57e-175">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="2f57e-176">Refit 是適用於 .NET 的 REST 程式庫。</span><span class="sxs-lookup"><span data-stu-id="2f57e-176">Refit is a REST library for .NET.</span></span> <span data-ttu-id="2f57e-177">它將 REST API 轉換為即時介面。</span><span class="sxs-lookup"><span data-stu-id="2f57e-177">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="2f57e-178">介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。</span><span class="sxs-lookup"><span data-stu-id="2f57e-178">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="2f57e-179">定義介面及回覆來代表外部 API 和其回應：</span><span class="sxs-lookup"><span data-stu-id="2f57e-179">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="2f57e-180">可以新增具型別用戶端，使用 Refit 產生實作：</span><span class="sxs-lookup"><span data-stu-id="2f57e-180">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="2f57e-181">定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：</span><span class="sxs-lookup"><span data-stu-id="2f57e-181">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="2f57e-182">外寄要求中介軟體</span><span class="sxs-lookup"><span data-stu-id="2f57e-182">Outgoing request middleware</span></span>

<span data-ttu-id="2f57e-183">`HttpClient`具有委派處理常式的概念，可以連結在一起以用於傳出 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="2f57e-183">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="2f57e-184">`IHttpClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="2f57e-184">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="2f57e-185">簡化定義要套用至每個已命名用戶端的處理常式。</span><span class="sxs-lookup"><span data-stu-id="2f57e-185">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="2f57e-186">支援多個處理常式的註冊和連結，以建立外寄要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="2f57e-186">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="2f57e-187">這些處理常式每個都可以在外寄要求之前和之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="2f57e-187">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="2f57e-188">此模式：</span><span class="sxs-lookup"><span data-stu-id="2f57e-188">This pattern:</span></span>

  * <span data-ttu-id="2f57e-189">類似 ASP.NET Core 中的輸入中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="2f57e-189">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="2f57e-190">提供一種機制來管理 HTTP 要求的跨領域考慮，例如：</span><span class="sxs-lookup"><span data-stu-id="2f57e-190">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="2f57e-191">快取</span><span class="sxs-lookup"><span data-stu-id="2f57e-191">caching</span></span>
    * <span data-ttu-id="2f57e-192">錯誤處理</span><span class="sxs-lookup"><span data-stu-id="2f57e-192">error handling</span></span>
    * <span data-ttu-id="2f57e-193">序列化</span><span class="sxs-lookup"><span data-stu-id="2f57e-193">serialization</span></span>
    * <span data-ttu-id="2f57e-194">logging</span><span class="sxs-lookup"><span data-stu-id="2f57e-194">logging</span></span>

<span data-ttu-id="2f57e-195">若要建立委派處理常式：</span><span class="sxs-lookup"><span data-stu-id="2f57e-195">To create a delegating handler:</span></span>

* <span data-ttu-id="2f57e-196">衍生自<xref:System.Net.Http.DelegatingHandler>。</span><span class="sxs-lookup"><span data-stu-id="2f57e-196">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="2f57e-197">覆寫 <xref:System.Net.Http.DelegatingHandler.SendAsync*>。</span><span class="sxs-lookup"><span data-stu-id="2f57e-197">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="2f57e-198">先執行程式碼，再將要求傳遞至管線中的下一個處理常式：</span><span class="sxs-lookup"><span data-stu-id="2f57e-198">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="2f57e-199">上述程式碼會檢查`X-API-KEY`標頭是否在要求中。</span><span class="sxs-lookup"><span data-stu-id="2f57e-199">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="2f57e-200">如果`X-API-KEY`遺失， <xref:System.Net.HttpStatusCode.BadRequest>則會傳回。</span><span class="sxs-lookup"><span data-stu-id="2f57e-200">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="2f57e-201">`HttpClient`有<xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>一個以上的處理常式可以加入至的設定：</span><span class="sxs-lookup"><span data-stu-id="2f57e-201">More than one handler can be added to the configuration for an `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="2f57e-202">在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。</span><span class="sxs-lookup"><span data-stu-id="2f57e-202">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="2f57e-203">`IHttpClientFactory` 會為每個處理常式建立個別的 DI 範圍。</span><span class="sxs-lookup"><span data-stu-id="2f57e-203">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="2f57e-204">處理常式可以相依于任何範圍的服務。</span><span class="sxs-lookup"><span data-stu-id="2f57e-204">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="2f57e-205">處置處理常式時，會處置處理常式所相依的服務。</span><span class="sxs-lookup"><span data-stu-id="2f57e-205">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="2f57e-206">註冊之後，便可以呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>，並傳入處理常式的類型。</span><span class="sxs-lookup"><span data-stu-id="2f57e-206">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="2f57e-207">可以遵循應該執行的順序來註冊多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="2f57e-207">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="2f57e-208">每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：</span><span class="sxs-lookup"><span data-stu-id="2f57e-208">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="2f57e-209">使用下列其中一種方式來與訊息處理常式共用個別要求狀態：</span><span class="sxs-lookup"><span data-stu-id="2f57e-209">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="2f57e-210">使用[HttpRequestMessage](xref:System.Net.Http.HttpRequestMessage.Properties)將資料傳遞至處理常式。</span><span class="sxs-lookup"><span data-stu-id="2f57e-210">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="2f57e-211">使用 <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> 來存取目前的要求。</span><span class="sxs-lookup"><span data-stu-id="2f57e-211">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="2f57e-212">建立自訂 <xref:System.Threading.AsyncLocal`1> 儲存體物件以傳遞資料。</span><span class="sxs-lookup"><span data-stu-id="2f57e-212">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="2f57e-213">使用 Polly 為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="2f57e-213">Use Polly-based handlers</span></span>

<span data-ttu-id="2f57e-214">`IHttpClientFactory`與協力廠商程式庫[Polly](https://github.com/App-vNext/Polly)整合。</span><span class="sxs-lookup"><span data-stu-id="2f57e-214">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="2f57e-215">Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。</span><span class="sxs-lookup"><span data-stu-id="2f57e-215">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="2f57e-216">它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。</span><span class="sxs-lookup"><span data-stu-id="2f57e-216">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="2f57e-217">提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="2f57e-217">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="2f57e-218">Polly 擴充功能支援將以 Polly 為基礎的處理常式新增至用戶端。</span><span class="sxs-lookup"><span data-stu-id="2f57e-218">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="2f57e-219">Polly 需要[Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="2f57e-219">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="2f57e-220">處理暫時性錯誤</span><span class="sxs-lookup"><span data-stu-id="2f57e-220">Handle transient faults</span></span>

<span data-ttu-id="2f57e-221">當外部 HTTP 呼叫是暫時性的時，通常會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="2f57e-221">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="2f57e-222"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*>允許定義原則來處理暫時性錯誤。</span><span class="sxs-lookup"><span data-stu-id="2f57e-222"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="2f57e-223">使用`AddTransientHttpErrorPolicy`設定的原則會處理下列回應：</span><span class="sxs-lookup"><span data-stu-id="2f57e-223">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="2f57e-224">HTTP 5xx</span><span class="sxs-lookup"><span data-stu-id="2f57e-224">HTTP 5xx</span></span>
* <span data-ttu-id="2f57e-225">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="2f57e-225">HTTP 408</span></span>

<span data-ttu-id="2f57e-226">`AddTransientHttpErrorPolicy`提供物件的存取`PolicyBuilder`權，其設定用來處理代表可能暫時性錯誤的錯誤：</span><span class="sxs-lookup"><span data-stu-id="2f57e-226">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="2f57e-227">在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。</span><span class="sxs-lookup"><span data-stu-id="2f57e-227">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="2f57e-228">失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。</span><span class="sxs-lookup"><span data-stu-id="2f57e-228">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="2f57e-229">動態選取原則</span><span class="sxs-lookup"><span data-stu-id="2f57e-229">Dynamically select policies</span></span>

<span data-ttu-id="2f57e-230">提供擴充方法來加入以 Polly 為基礎的處理常式，例如<xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>。</span><span class="sxs-lookup"><span data-stu-id="2f57e-230">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="2f57e-231">下列`AddPolicyHandler`多載會檢查要求以決定要套用的原則：</span><span class="sxs-lookup"><span data-stu-id="2f57e-231">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="2f57e-232">在上述程式碼中，如果外寄要求是 HTTP GET，就會套用 10 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="2f57e-232">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="2f57e-233">任何其他 HTTP 方法會使用 30 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="2f57e-233">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="2f57e-234">新增多個 Polly 處理常式</span><span class="sxs-lookup"><span data-stu-id="2f57e-234">Add multiple Polly handlers</span></span>

<span data-ttu-id="2f57e-235">通常會將 Polly 原則加以嵌套：</span><span class="sxs-lookup"><span data-stu-id="2f57e-235">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="2f57e-236">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="2f57e-236">In the preceding example:</span></span>

* <span data-ttu-id="2f57e-237">新增了兩個處理常式。</span><span class="sxs-lookup"><span data-stu-id="2f57e-237">Two handlers are added.</span></span>
* <span data-ttu-id="2f57e-238">第一個處理常式<xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*>會使用來新增重試原則。</span><span class="sxs-lookup"><span data-stu-id="2f57e-238">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="2f57e-239">失敗的要求會重試最多三次。</span><span class="sxs-lookup"><span data-stu-id="2f57e-239">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="2f57e-240">第二`AddTransientHttpErrorPolicy`個呼叫會加入斷路器原則。</span><span class="sxs-lookup"><span data-stu-id="2f57e-240">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="2f57e-241">如果連續5次嘗試失敗，則會封鎖進一步的外部要求30秒。</span><span class="sxs-lookup"><span data-stu-id="2f57e-241">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="2f57e-242">斷路器原則可設定狀態。</span><span class="sxs-lookup"><span data-stu-id="2f57e-242">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="2f57e-243">透過此用戶端的所有呼叫都會共用相同的線路狀態。</span><span class="sxs-lookup"><span data-stu-id="2f57e-243">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="2f57e-244">從 Polly 登錄新增原則</span><span class="sxs-lookup"><span data-stu-id="2f57e-244">Add policies from the Polly registry</span></span>

<span data-ttu-id="2f57e-245">管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。</span><span class="sxs-lookup"><span data-stu-id="2f57e-245">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="2f57e-246">在下列程式碼中：</span><span class="sxs-lookup"><span data-stu-id="2f57e-246">In the following code:</span></span>

* <span data-ttu-id="2f57e-247">系統會新增「一般」和「長」原則。</span><span class="sxs-lookup"><span data-stu-id="2f57e-247">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="2f57e-248"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>從登錄新增「一般」和「長」原則。</span><span class="sxs-lookup"><span data-stu-id="2f57e-248"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="2f57e-249">如需`IHttpClientFactory`與 Polly 整合的詳細資訊，請參閱[Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory)。</span><span class="sxs-lookup"><span data-stu-id="2f57e-249">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="2f57e-250">HttpClient 和存留期管理</span><span class="sxs-lookup"><span data-stu-id="2f57e-250">HttpClient and lifetime management</span></span>

<span data-ttu-id="2f57e-251">每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回新的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="2f57e-251">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="2f57e-252">會<xref:System.Net.Http.HttpMessageHandler>針對每個命名的用戶端建立。</span><span class="sxs-lookup"><span data-stu-id="2f57e-252">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="2f57e-253">處理站會管理 `HttpMessageHandler` 執行個體的存留期。</span><span class="sxs-lookup"><span data-stu-id="2f57e-253">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="2f57e-254">`IHttpClientFactory` 會將處理站所建立的 `HttpMessageHandler` 執行個體放入集區以減少資源耗用量。</span><span class="sxs-lookup"><span data-stu-id="2f57e-254">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="2f57e-255">建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。</span><span class="sxs-lookup"><span data-stu-id="2f57e-255">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="2f57e-256">將處理常式放入集區非常實用，因為處理常式通常會管理自己專屬的底層 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="2f57e-256">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="2f57e-257">建立比所需數目更多的處理常式，可能會導致連線延遲。</span><span class="sxs-lookup"><span data-stu-id="2f57e-257">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="2f57e-258">有些處理常式也會保持連線無限期地開啟，這可能導致處理常式無法回應 DNS （網域名稱系統）變更。</span><span class="sxs-lookup"><span data-stu-id="2f57e-258">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="2f57e-259">預設處理常式存留時間為兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="2f57e-259">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="2f57e-260">預設值可以根據每個命名的用戶端來覆寫：</span><span class="sxs-lookup"><span data-stu-id="2f57e-260">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="2f57e-261">`HttpClient`實例通常可視為**不**需要處置的 .net 物件。</span><span class="sxs-lookup"><span data-stu-id="2f57e-261">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="2f57e-262">處置會取消傳出的要求，並保證指定的 `HttpClient` 執行個體在呼叫 <xref:System.IDisposable.Dispose*> 之後無法使用。</span><span class="sxs-lookup"><span data-stu-id="2f57e-262">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="2f57e-263">`IHttpClientFactory` 會追蹤並處置 `HttpClient` 執行個體使用的資源。</span><span class="sxs-lookup"><span data-stu-id="2f57e-263">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="2f57e-264">在開始使用 `IHttpClientFactory` 之前，讓單一 `HttpClient` 執行個體維持一段較長的時間，是很常使用的模式。</span><span class="sxs-lookup"><span data-stu-id="2f57e-264">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="2f57e-265">在移轉到 `IHttpClientFactory` 之後，就不再需要此模式。</span><span class="sxs-lookup"><span data-stu-id="2f57e-265">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="2f57e-266">IHttpClientFactory 的替代方案</span><span class="sxs-lookup"><span data-stu-id="2f57e-266">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="2f57e-267">在`IHttpClientFactory`啟用 DI 的應用程式中使用，可避免：</span><span class="sxs-lookup"><span data-stu-id="2f57e-267">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="2f57e-268">共用`HttpMessageHandler`實例的資源耗盡問題。</span><span class="sxs-lookup"><span data-stu-id="2f57e-268">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="2f57e-269">定期迴圈`HttpMessageHandler`實例以過時的 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="2f57e-269">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="2f57e-270">有其他方法可以使用長時間的<xref:System.Net.Http.SocketsHttpHandler>實例來解決上述問題。</span><span class="sxs-lookup"><span data-stu-id="2f57e-270">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="2f57e-271">`SocketsHttpHandler`當應用程式啟動時，建立的實例，並在應用程式的生命週期中使用它。</span><span class="sxs-lookup"><span data-stu-id="2f57e-271">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="2f57e-272">根據<xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> DNS 重新整理時間，設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="2f57e-272">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="2f57e-273">視`HttpClient`需要使用`new HttpClient(handler, disposeHandler: false)`建立實例。</span><span class="sxs-lookup"><span data-stu-id="2f57e-273">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="2f57e-274">上述方法可`IHttpClientFactory`解決以類似方式解決的資源管理問題。</span><span class="sxs-lookup"><span data-stu-id="2f57e-274">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="2f57e-275">會`SocketsHttpHandler`共用實例間`HttpClient`的連接。</span><span class="sxs-lookup"><span data-stu-id="2f57e-275">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="2f57e-276">此共用可防止通訊端耗盡。</span><span class="sxs-lookup"><span data-stu-id="2f57e-276">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="2f57e-277">會`SocketsHttpHandler`根據`PooledConnectionLifetime`來迴圈連接，以避免過時的 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="2f57e-277">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="2f57e-278">Cookie</span><span class="sxs-lookup"><span data-stu-id="2f57e-278">Cookies</span></span>

<span data-ttu-id="2f57e-279">集區`HttpMessageHandler`實例會導致`CookieContainer`共用物件。</span><span class="sxs-lookup"><span data-stu-id="2f57e-279">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="2f57e-280">意外`CookieContainer`的物件共用通常會導致不正確的程式碼。</span><span class="sxs-lookup"><span data-stu-id="2f57e-280">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="2f57e-281">針對需要 cookie 的應用程式，請考慮下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="2f57e-281">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="2f57e-282">停用自動 cookie 處理</span><span class="sxs-lookup"><span data-stu-id="2f57e-282">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="2f57e-283">以免`IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="2f57e-283">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="2f57e-284">呼叫<xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>以停用自動 cookie 處理：</span><span class="sxs-lookup"><span data-stu-id="2f57e-284">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="2f57e-285">記錄</span><span class="sxs-lookup"><span data-stu-id="2f57e-285">Logging</span></span>

<span data-ttu-id="2f57e-286">透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="2f57e-286">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="2f57e-287">在記錄設定中啟用適當的資訊層級，以查看預設記錄檔訊息。</span><span class="sxs-lookup"><span data-stu-id="2f57e-287">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="2f57e-288">額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。</span><span class="sxs-lookup"><span data-stu-id="2f57e-288">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="2f57e-289">用於每個用戶端的記錄檔分類包含用戶端的名稱。</span><span class="sxs-lookup"><span data-stu-id="2f57e-289">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="2f57e-290">例如，名為*MyNamedClient*的用戶端會記錄類別為 "HttpClient" 的訊息。**MyNamedClient**。LogicalHandler "。</span><span class="sxs-lookup"><span data-stu-id="2f57e-290">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="2f57e-291">後面加上 *LogicalHandler* 的訊息發生在要求處理常式管線之外。</span><span class="sxs-lookup"><span data-stu-id="2f57e-291">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="2f57e-292">在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。</span><span class="sxs-lookup"><span data-stu-id="2f57e-292">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="2f57e-293">在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。</span><span class="sxs-lookup"><span data-stu-id="2f57e-293">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="2f57e-294">記錄也會發生在要求處理常式管線之內。</span><span class="sxs-lookup"><span data-stu-id="2f57e-294">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="2f57e-295">在*MyNamedClient*範例中，這些訊息會記錄在記錄檔類別中為 "HttpClient"。**MyNamedClient**。ClientHandler".</span><span class="sxs-lookup"><span data-stu-id="2f57e-295">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="2f57e-296">對於要求，這會在所有其他處理常式都執行之後，且在傳送要求之前立即發生。</span><span class="sxs-lookup"><span data-stu-id="2f57e-296">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="2f57e-297">在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。</span><span class="sxs-lookup"><span data-stu-id="2f57e-297">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="2f57e-298">在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。</span><span class="sxs-lookup"><span data-stu-id="2f57e-298">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="2f57e-299">這可能包括要求標頭的變更或回應狀態碼。</span><span class="sxs-lookup"><span data-stu-id="2f57e-299">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="2f57e-300">在記錄類別中包含用戶端的名稱，可以針對特定的已命名用戶端進行記錄篩選。</span><span class="sxs-lookup"><span data-stu-id="2f57e-300">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="2f57e-301">設定 HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="2f57e-301">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="2f57e-302">可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。</span><span class="sxs-lookup"><span data-stu-id="2f57e-302">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="2f57e-303">新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。</span><span class="sxs-lookup"><span data-stu-id="2f57e-303">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="2f57e-304"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 擴充方法可以用來定義委派。</span><span class="sxs-lookup"><span data-stu-id="2f57e-304">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="2f57e-305">委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：</span><span class="sxs-lookup"><span data-stu-id="2f57e-305">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="2f57e-306">在主控台應用程式中使用 IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="2f57e-306">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="2f57e-307">在主控台應用程式中，將下列套件參考新增至專案：</span><span class="sxs-lookup"><span data-stu-id="2f57e-307">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="2f57e-308">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="2f57e-308">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="2f57e-309">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="2f57e-309">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="2f57e-310">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="2f57e-310">In the following example:</span></span>

* <span data-ttu-id="2f57e-311"><xref:System.Net.Http.IHttpClientFactory> 已在[泛型主機的](xref:fundamentals/host/generic-host)服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="2f57e-311"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="2f57e-312">`MyService` 會從服務建立用戶端 Factory 執行個體，其可用來建立 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="2f57e-312">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="2f57e-313">`HttpClient` 會用來擷取網頁。</span><span class="sxs-lookup"><span data-stu-id="2f57e-313">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="2f57e-314">`Main` 會建立範圍來執行服務的 `GetPage` 方法，並將網頁內容的前 500 個字元寫入至主控台。</span><span class="sxs-lookup"><span data-stu-id="2f57e-314">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="header-propagation-middleware"></a><span data-ttu-id="2f57e-315">標頭傳播中介軟體</span><span class="sxs-lookup"><span data-stu-id="2f57e-315">Header propagation middleware</span></span>

<span data-ttu-id="2f57e-316">標頭傳播是一個 ASP.NET Core 中介軟體，可將 HTTP 標頭從傳入要求傳播到傳出的 HTTP 用戶端要求。</span><span class="sxs-lookup"><span data-stu-id="2f57e-316">Header propagation is an ASP.NET Core middleware to propagate HTTP headers from the incoming request to the outgoing HTTP Client requests.</span></span> <span data-ttu-id="2f57e-317">若要使用標頭傳播：</span><span class="sxs-lookup"><span data-stu-id="2f57e-317">To use header propagation:</span></span>

* <span data-ttu-id="2f57e-318">參考[AspNetCore. HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation)套件。</span><span class="sxs-lookup"><span data-stu-id="2f57e-318">Reference the [Microsoft.AspNetCore.HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation) package.</span></span>
* <span data-ttu-id="2f57e-319">設定中介軟體和`HttpClient`中`Startup`的：</span><span class="sxs-lookup"><span data-stu-id="2f57e-319">Configure the middleware and `HttpClient` in `Startup`:</span></span>

  [!code-csharp[](http-requests/samples/3.x/Startup.cs?highlight=5-9,21&name=snippet)]

* <span data-ttu-id="2f57e-320">用戶端會在輸出要求中包含已設定的標頭：</span><span class="sxs-lookup"><span data-stu-id="2f57e-320">The client includes the configured headers on outbound requests:</span></span>

  ```csharp
  var client = clientFactory.CreateClient("MyForwardingClient");
  var response = client.GetAsync(...);
  ```

## <a name="additional-resources"></a><span data-ttu-id="2f57e-321">其他資源</span><span class="sxs-lookup"><span data-stu-id="2f57e-321">Additional resources</span></span>

* [<span data-ttu-id="2f57e-322">使用 HttpClientFactory 實作復原 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="2f57e-322">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="2f57e-323">使用 HttpClientFactory 和 Polly 原則以指數輪詢實作 HTTP 呼叫重試</span><span class="sxs-lookup"><span data-stu-id="2f57e-323">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="2f57e-324">實作斷路器模式</span><span class="sxs-lookup"><span data-stu-id="2f57e-324">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)
* [<span data-ttu-id="2f57e-325">如何在 .NET 中序列化和還原序列化 JSON</span><span class="sxs-lookup"><span data-stu-id="2f57e-325">How to serialize and deserialize JSON in .NET</span></span>](/dotnet/standard/serialization/system-text-json-how-to)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="2f57e-326">作者：[Glenn Condron](https://github.com/glennc)、[Ryan Nowak](https://github.com/rynowak) 和 [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="2f57e-326">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="2f57e-327"><xref:System.Net.Http.IHttpClientFactory> 可以註冊及用來在應用程式中設定和建立 <xref:System.Net.Http.HttpClient> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="2f57e-327">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="2f57e-328">它提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="2f57e-328">It offers the following benefits:</span></span>

* <span data-ttu-id="2f57e-329">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="2f57e-329">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="2f57e-330">例如，您可以註冊並設定*github*用戶端來存取[github](https://github.com/)。</span><span class="sxs-lookup"><span data-stu-id="2f57e-330">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="2f57e-331">預設用戶端可以註冊用於其他用途。</span><span class="sxs-lookup"><span data-stu-id="2f57e-331">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="2f57e-332">透過委派 `HttpClient` 中的處理常式來撰寫外寄中介軟體的概念，並提供延伸模組以便 Polly 架構中介軟體利用外寄中介軟體。</span><span class="sxs-lookup"><span data-stu-id="2f57e-332">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="2f57e-333">管理基礎 `HttpClientMessageHandler` 執行個體的共用和存留期，以避免在手動管理 `HttpClient` 存留期時，發生的常見 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="2f57e-333">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="2f57e-334">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="2f57e-334">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="2f57e-335">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="2f57e-335">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="2f57e-336">耗用量模式</span><span class="sxs-lookup"><span data-stu-id="2f57e-336">Consumption patterns</span></span>

<span data-ttu-id="2f57e-337">有數種方式可將 `IHttpClientFactory` 用於應用程式：</span><span class="sxs-lookup"><span data-stu-id="2f57e-337">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="2f57e-338">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="2f57e-338">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="2f57e-339">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="2f57e-339">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="2f57e-340">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="2f57e-340">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="2f57e-341">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="2f57e-341">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="2f57e-342">它們全都不會嚴格優先於另一個。</span><span class="sxs-lookup"><span data-stu-id="2f57e-342">None of them are strictly superior to another.</span></span> <span data-ttu-id="2f57e-343">最好的方法取決於應用程式的條件約束。</span><span class="sxs-lookup"><span data-stu-id="2f57e-343">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="2f57e-344">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="2f57e-344">Basic usage</span></span>

<span data-ttu-id="2f57e-345">`IHttpClientFactory` 可以藉由在 `Startup.ConfigureServices` 方法內的 `IServiceCollection` 上呼叫 `AddHttpClient` 擴充方法來註冊。</span><span class="sxs-lookup"><span data-stu-id="2f57e-345">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="2f57e-346">註冊之後，程式碼可以接受`IHttpClientFactory`可以使用相依性[插入（DI）](xref:fundamentals/dependency-injection)插入的任何位置服務。</span><span class="sxs-lookup"><span data-stu-id="2f57e-346">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="2f57e-347">`IHttpClientFactory`可以用來建立`HttpClient`實例：</span><span class="sxs-lookup"><span data-stu-id="2f57e-347">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="2f57e-348">以這種方式使用 `IHttpClientFactory` 是重構現有應用程式的好方法。</span><span class="sxs-lookup"><span data-stu-id="2f57e-348">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="2f57e-349">它對 `HttpClient` 的使用方式沒有任何影響。</span><span class="sxs-lookup"><span data-stu-id="2f57e-349">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="2f57e-350">在目前建立 `HttpClient` 執行個體的位置，將那些項目取代為呼叫 <xref:System.Net.Http.IHttpClientFactory.CreateClient*>。</span><span class="sxs-lookup"><span data-stu-id="2f57e-350">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="2f57e-351">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="2f57e-351">Named clients</span></span>

<span data-ttu-id="2f57e-352">如果應用程式需要使用多個不同的 `HttpClient`，且每個都有不同的設定，可以選擇使用**具名用戶端**。</span><span class="sxs-lookup"><span data-stu-id="2f57e-352">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="2f57e-353">具名 `HttpClient` 的組態可以在 `Startup.ConfigureServices` 中註冊時指定。</span><span class="sxs-lookup"><span data-stu-id="2f57e-353">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="2f57e-354">在上述程式碼中`AddHttpClient` ，會呼叫，並提供名稱*github*。</span><span class="sxs-lookup"><span data-stu-id="2f57e-354">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="2f57e-355">此用戶端已套用一些預設組態&mdash;即使用 GitHub API 所需的基底位址和兩個標頭。</span><span class="sxs-lookup"><span data-stu-id="2f57e-355">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="2f57e-356">每次呼叫 `CreateClient` 時，會建立 `HttpClient` 的新執行個體並呼叫組態動作。</span><span class="sxs-lookup"><span data-stu-id="2f57e-356">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="2f57e-357">若要使用具名用戶端，可以傳遞字串參數至 `CreateClient`。</span><span class="sxs-lookup"><span data-stu-id="2f57e-357">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="2f57e-358">指定要建立之用戶端的名稱：</span><span class="sxs-lookup"><span data-stu-id="2f57e-358">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="2f57e-359">在上述程式碼中，要求不需要指定主機名稱。</span><span class="sxs-lookup"><span data-stu-id="2f57e-359">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="2f57e-360">它可以只傳遞路徑，因為已使用為用戶端設定的基底位址。</span><span class="sxs-lookup"><span data-stu-id="2f57e-360">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="2f57e-361">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="2f57e-361">Typed clients</span></span>

<span data-ttu-id="2f57e-362">具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="2f57e-362">Typed clients:</span></span>

* <span data-ttu-id="2f57e-363">提供與具名用戶端相同的功能，而不需使用字串作為索引鍵。</span><span class="sxs-lookup"><span data-stu-id="2f57e-363">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="2f57e-364">取用用戶端時提供 IntelliSense 和編譯器說明。</span><span class="sxs-lookup"><span data-stu-id="2f57e-364">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="2f57e-365">提供單一位置來設定特定的 `HttpClient` 並與其互動。</span><span class="sxs-lookup"><span data-stu-id="2f57e-365">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="2f57e-366">例如，單一的具型別用戶端可能用於單一的後端端點，並封裝處理該端點的所有邏輯。</span><span class="sxs-lookup"><span data-stu-id="2f57e-366">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="2f57e-367">使用 DI 且可在應用程式中需要之處插入。</span><span class="sxs-lookup"><span data-stu-id="2f57e-367">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="2f57e-368">具型別用戶端`HttpClient`會接受其函式中的參數：</span><span class="sxs-lookup"><span data-stu-id="2f57e-368">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="2f57e-369">在上述程式碼中，組態會移到具型別用戶端。</span><span class="sxs-lookup"><span data-stu-id="2f57e-369">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="2f57e-370">`HttpClient` 物件會公開為公用屬性。</span><span class="sxs-lookup"><span data-stu-id="2f57e-370">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="2f57e-371">您可定義 API 特定的方法，其公開 `HttpClient` 功能。</span><span class="sxs-lookup"><span data-stu-id="2f57e-371">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="2f57e-372">`GetAspNetDocsIssues` 方法會封裝從 GitHub 存放庫查詢和剖析最新開啟問題所需的程式碼。</span><span class="sxs-lookup"><span data-stu-id="2f57e-372">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="2f57e-373">若要註冊具型別用戶端，泛型 <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> 擴充方法可用於 `Startup.ConfigureServices` 內，並指定具型別用戶端類別：</span><span class="sxs-lookup"><span data-stu-id="2f57e-373">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="2f57e-374">具型別用戶端會向 DI 註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="2f57e-374">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="2f57e-375">具型別用戶端可以直接插入並使用：</span><span class="sxs-lookup"><span data-stu-id="2f57e-375">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="2f57e-376">想要的話，具型別用戶端的組態可以在 `Startup.ConfigureServices` 中註冊時指定，而不是在具型別用戶端的建構函式中：</span><span class="sxs-lookup"><span data-stu-id="2f57e-376">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="2f57e-377">可以將 `HttpClient` 完全封裝在具型別用戶端內。</span><span class="sxs-lookup"><span data-stu-id="2f57e-377">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="2f57e-378">可以提供在內部呼叫 `HttpClient` 執行個體的公用方法，而不將它公開為屬性。</span><span class="sxs-lookup"><span data-stu-id="2f57e-378">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="2f57e-379">在上述程式碼，`HttpClient` 儲存為私用欄位。</span><span class="sxs-lookup"><span data-stu-id="2f57e-379">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="2f57e-380">進行外部呼叫的所有存取都會經歷 `GetRepos` 方法。</span><span class="sxs-lookup"><span data-stu-id="2f57e-380">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="2f57e-381">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="2f57e-381">Generated clients</span></span>

<span data-ttu-id="2f57e-382">`IHttpClientFactory` 可和其他協力廠商程式庫一起使用，例如 [Refit](https://github.com/paulcbetts/refit)。</span><span class="sxs-lookup"><span data-stu-id="2f57e-382">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="2f57e-383">Refit 是適用於 .NET 的 REST 程式庫。</span><span class="sxs-lookup"><span data-stu-id="2f57e-383">Refit is a REST library for .NET.</span></span> <span data-ttu-id="2f57e-384">它將 REST API 轉換為即時介面。</span><span class="sxs-lookup"><span data-stu-id="2f57e-384">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="2f57e-385">介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。</span><span class="sxs-lookup"><span data-stu-id="2f57e-385">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="2f57e-386">定義介面及回覆來代表外部 API 和其回應：</span><span class="sxs-lookup"><span data-stu-id="2f57e-386">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="2f57e-387">可以新增具型別用戶端，使用 Refit 產生實作：</span><span class="sxs-lookup"><span data-stu-id="2f57e-387">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="2f57e-388">定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：</span><span class="sxs-lookup"><span data-stu-id="2f57e-388">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="2f57e-389">外寄要求中介軟體</span><span class="sxs-lookup"><span data-stu-id="2f57e-389">Outgoing request middleware</span></span>

<span data-ttu-id="2f57e-390">`HttpClient` 已經有委派可針對外寄 HTTP 要求連結在一起的處理常式的概念。</span><span class="sxs-lookup"><span data-stu-id="2f57e-390">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="2f57e-391">`IHttpClientFactory` 可讓您輕鬆地定義要套用於每個具名用戶端的處理常式。</span><span class="sxs-lookup"><span data-stu-id="2f57e-391">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="2f57e-392">它支援註冊和鏈結多個處理常式，以建置外寄要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="2f57e-392">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="2f57e-393">這些處理常式每個都可以在外寄要求之前和之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="2f57e-393">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="2f57e-394">此模式與 ASP.NET Core 中的輸入中介軟體管線相似。</span><span class="sxs-lookup"><span data-stu-id="2f57e-394">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="2f57e-395">模式提供一個機制來管理 HTTP 要求的跨領域關注，包括快取、錯誤處理、序列化和記錄。</span><span class="sxs-lookup"><span data-stu-id="2f57e-395">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="2f57e-396">若要建立處理常式，請定義衍生自 <xref:System.Net.Http.DelegatingHandler> 的類別。</span><span class="sxs-lookup"><span data-stu-id="2f57e-396">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="2f57e-397">覆寫 `SendAsync` 方法，以在將要求傳遞至管線中的下一個處理常式之前執行程式碼：</span><span class="sxs-lookup"><span data-stu-id="2f57e-397">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="2f57e-398">上述程式碼定義一個基本處理常式。</span><span class="sxs-lookup"><span data-stu-id="2f57e-398">The preceding code defines a basic handler.</span></span> <span data-ttu-id="2f57e-399">它會檢查以查看要求上是否已包含 `X-API-KEY` 標頭。</span><span class="sxs-lookup"><span data-stu-id="2f57e-399">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="2f57e-400">如果遺漏標頭，它可以避免 HTTP 呼叫，並傳回適當的回應。</span><span class="sxs-lookup"><span data-stu-id="2f57e-400">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="2f57e-401">在註冊期間，可以將一或多個處理常式新增至的`HttpClient`設定。</span><span class="sxs-lookup"><span data-stu-id="2f57e-401">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="2f57e-402">這項工作是透過 <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder> 上的擴充方法完成。</span><span class="sxs-lookup"><span data-stu-id="2f57e-402">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="2f57e-403">在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。</span><span class="sxs-lookup"><span data-stu-id="2f57e-403">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="2f57e-404">`IHttpClientFactory` 會為每個處理常式建立個別的 DI 範圍。</span><span class="sxs-lookup"><span data-stu-id="2f57e-404">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="2f57e-405">處理常式可相依於任何範圍的服務。</span><span class="sxs-lookup"><span data-stu-id="2f57e-405">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="2f57e-406">處置處理常式時，會處置處理常式所相依的服務。</span><span class="sxs-lookup"><span data-stu-id="2f57e-406">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="2f57e-407">註冊之後，便可以呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>，並傳入處理常式的類型。</span><span class="sxs-lookup"><span data-stu-id="2f57e-407">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="2f57e-408">可以遵循應該執行的順序來註冊多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="2f57e-408">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="2f57e-409">每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：</span><span class="sxs-lookup"><span data-stu-id="2f57e-409">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="2f57e-410">使用下列其中一種方式來與訊息處理常式共用個別要求狀態：</span><span class="sxs-lookup"><span data-stu-id="2f57e-410">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="2f57e-411">使用 `HttpRequestMessage.Properties` 將資料傳遞到處理常式。</span><span class="sxs-lookup"><span data-stu-id="2f57e-411">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="2f57e-412">使用 `IHttpContextAccessor` 來存取目前的要求。</span><span class="sxs-lookup"><span data-stu-id="2f57e-412">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="2f57e-413">建立自訂 `AsyncLocal` 儲存體物件以傳遞資料。</span><span class="sxs-lookup"><span data-stu-id="2f57e-413">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="2f57e-414">使用 Polly 為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="2f57e-414">Use Polly-based handlers</span></span>

<span data-ttu-id="2f57e-415">`IHttpClientFactory` 整合受歡迎的協力廠商程式庫，稱為 [Polly](https://github.com/App-vNext/Polly)。</span><span class="sxs-lookup"><span data-stu-id="2f57e-415">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="2f57e-416">Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。</span><span class="sxs-lookup"><span data-stu-id="2f57e-416">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="2f57e-417">它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。</span><span class="sxs-lookup"><span data-stu-id="2f57e-417">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="2f57e-418">提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="2f57e-418">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="2f57e-419">Polly 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="2f57e-419">The Polly extensions:</span></span>

* <span data-ttu-id="2f57e-420">支援將以 Polly 為基礎的處理常式新增至用戶端。</span><span class="sxs-lookup"><span data-stu-id="2f57e-420">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="2f57e-421">可在安裝 [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet 套件後使用。</span><span class="sxs-lookup"><span data-stu-id="2f57e-421">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="2f57e-422">該套件並未包含在 ASP.NET Core 共用架構中。</span><span class="sxs-lookup"><span data-stu-id="2f57e-422">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="2f57e-423">處理暫時性錯誤</span><span class="sxs-lookup"><span data-stu-id="2f57e-423">Handle transient faults</span></span>

<span data-ttu-id="2f57e-424">大部分的錯誤發生在外部 HTTP 呼叫是暫時性的時候。</span><span class="sxs-lookup"><span data-stu-id="2f57e-424">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="2f57e-425">包含一個便利的擴充方法，稱為 `AddTransientHttpErrorPolicy`，它可允許定義原則來處理暫時性錯誤。</span><span class="sxs-lookup"><span data-stu-id="2f57e-425">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="2f57e-426">使用此延伸模組方法設定的原則，會處理 `HttpRequestException`、HTTP 5xx 回應和 HTTP 408 回應。</span><span class="sxs-lookup"><span data-stu-id="2f57e-426">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="2f57e-427">`AddTransientHttpErrorPolicy` 延伸模組可用於 `Startup.ConfigureServices` 內。</span><span class="sxs-lookup"><span data-stu-id="2f57e-427">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="2f57e-428">延伸模組能提供 `PolicyBuilder` 物件的存取，該物件已設定來處理代表可能暫時性錯誤的錯誤：</span><span class="sxs-lookup"><span data-stu-id="2f57e-428">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="2f57e-429">在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。</span><span class="sxs-lookup"><span data-stu-id="2f57e-429">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="2f57e-430">失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。</span><span class="sxs-lookup"><span data-stu-id="2f57e-430">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="2f57e-431">動態選取原則</span><span class="sxs-lookup"><span data-stu-id="2f57e-431">Dynamically select policies</span></span>

<span data-ttu-id="2f57e-432">有額外的擴充方法可用來新增 Polly 為基礎的處理常式。</span><span class="sxs-lookup"><span data-stu-id="2f57e-432">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="2f57e-433">其中一個這類延伸模組是 `AddPolicyHandler`，它有多個多載。</span><span class="sxs-lookup"><span data-stu-id="2f57e-433">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="2f57e-434">一個多載可讓您在定義要套用的原則時，檢查要求：</span><span class="sxs-lookup"><span data-stu-id="2f57e-434">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="2f57e-435">在上述程式碼中，如果外寄要求是 HTTP GET，就會套用 10 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="2f57e-435">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="2f57e-436">任何其他 HTTP 方法會使用 30 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="2f57e-436">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="2f57e-437">新增多個 Polly 處理常式</span><span class="sxs-lookup"><span data-stu-id="2f57e-437">Add multiple Polly handlers</span></span>

<span data-ttu-id="2f57e-438">通常會建立巢狀 Polly 原則，以提供增強的功能：</span><span class="sxs-lookup"><span data-stu-id="2f57e-438">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="2f57e-439">在上述範例中，會新增兩個處理常式。</span><span class="sxs-lookup"><span data-stu-id="2f57e-439">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="2f57e-440">第一個使用 `AddTransientHttpErrorPolicy` 延伸模組來新增重試原則。</span><span class="sxs-lookup"><span data-stu-id="2f57e-440">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="2f57e-441">失敗的要求會重試最多三次。</span><span class="sxs-lookup"><span data-stu-id="2f57e-441">Failed requests are retried up to three times.</span></span> <span data-ttu-id="2f57e-442">第二個 `AddTransientHttpErrorPolicy` 呼叫會新增斷路器原則。</span><span class="sxs-lookup"><span data-stu-id="2f57e-442">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="2f57e-443">如果循序發生五次失敗的嘗試，進一步的外部要求會遭到封鎖 30 秒。</span><span class="sxs-lookup"><span data-stu-id="2f57e-443">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="2f57e-444">斷路器原則可設定狀態。</span><span class="sxs-lookup"><span data-stu-id="2f57e-444">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="2f57e-445">透過此用戶端的所有呼叫都會共用相同的線路狀態。</span><span class="sxs-lookup"><span data-stu-id="2f57e-445">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="2f57e-446">從 Polly 登錄新增原則</span><span class="sxs-lookup"><span data-stu-id="2f57e-446">Add policies from the Polly registry</span></span>

<span data-ttu-id="2f57e-447">管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。</span><span class="sxs-lookup"><span data-stu-id="2f57e-447">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="2f57e-448">提供了擴充方法，可以使用來自登錄的原則新增處理常式：</span><span class="sxs-lookup"><span data-stu-id="2f57e-448">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="2f57e-449">在上述程式碼中，當 `PolicyRegistry` 新增至 `ServiceCollection` 時，註冊了兩個原則。</span><span class="sxs-lookup"><span data-stu-id="2f57e-449">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="2f57e-450">為了使用來自登錄的原則，使用了 `AddPolicyHandlerFromRegistry` 方法，並傳遞要套用的原則名稱。</span><span class="sxs-lookup"><span data-stu-id="2f57e-450">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="2f57e-451">關於 `IHttpClientFactory` Polly 整合的詳細資訊，可以在 [Polly Wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory) 上找到。</span><span class="sxs-lookup"><span data-stu-id="2f57e-451">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="2f57e-452">HttpClient 和存留期管理</span><span class="sxs-lookup"><span data-stu-id="2f57e-452">HttpClient and lifetime management</span></span>

<span data-ttu-id="2f57e-453">每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回新的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="2f57e-453">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="2f57e-454">每個具名用戶端都有一個 <xref:System.Net.Http.HttpMessageHandler>。</span><span class="sxs-lookup"><span data-stu-id="2f57e-454">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="2f57e-455">處理站會管理 `HttpMessageHandler` 執行個體的存留期。</span><span class="sxs-lookup"><span data-stu-id="2f57e-455">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="2f57e-456">`IHttpClientFactory` 會將處理站所建立的 `HttpMessageHandler` 執行個體放入集區以減少資源耗用量。</span><span class="sxs-lookup"><span data-stu-id="2f57e-456">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="2f57e-457">建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。</span><span class="sxs-lookup"><span data-stu-id="2f57e-457">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="2f57e-458">將處理常式放入集區非常實用，因為處理常式通常會管理自己專屬的底層 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="2f57e-458">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="2f57e-459">建立比所需數目更多的處理常式，可能會導致連線延遲。</span><span class="sxs-lookup"><span data-stu-id="2f57e-459">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="2f57e-460">有些處理常式也會保持連線無限期地開啟，這可能導致處理常式無法對 DNS 變更回應。</span><span class="sxs-lookup"><span data-stu-id="2f57e-460">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="2f57e-461">預設處理常式存留時間為兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="2f57e-461">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="2f57e-462">可以針對每個具名用戶端覆寫預設值。</span><span class="sxs-lookup"><span data-stu-id="2f57e-462">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="2f57e-463">若要覆寫它，請在建立用戶端時所傳回的 `IHttpClientBuilder` 上呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*>：</span><span class="sxs-lookup"><span data-stu-id="2f57e-463">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="2f57e-464">不需要處置用戶端。</span><span class="sxs-lookup"><span data-stu-id="2f57e-464">Disposal of the client isn't required.</span></span> <span data-ttu-id="2f57e-465">處置會取消傳出的要求，並保證指定的 `HttpClient` 執行個體在呼叫 <xref:System.IDisposable.Dispose*> 之後無法使用。</span><span class="sxs-lookup"><span data-stu-id="2f57e-465">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="2f57e-466">`IHttpClientFactory` 會追蹤並處置 `HttpClient` 執行個體使用的資源。</span><span class="sxs-lookup"><span data-stu-id="2f57e-466">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="2f57e-467">`HttpClient` 執行個體通常可視為 .NET 物件，不需要處置。</span><span class="sxs-lookup"><span data-stu-id="2f57e-467">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="2f57e-468">在開始使用 `IHttpClientFactory` 之前，讓單一 `HttpClient` 執行個體維持一段較長的時間，是很常使用的模式。</span><span class="sxs-lookup"><span data-stu-id="2f57e-468">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="2f57e-469">在移轉到 `IHttpClientFactory` 之後，就不再需要此模式。</span><span class="sxs-lookup"><span data-stu-id="2f57e-469">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="2f57e-470">IHttpClientFactory 的替代方案</span><span class="sxs-lookup"><span data-stu-id="2f57e-470">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="2f57e-471">在`IHttpClientFactory`啟用 DI 的應用程式中使用，可避免：</span><span class="sxs-lookup"><span data-stu-id="2f57e-471">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="2f57e-472">共用`HttpMessageHandler`實例的資源耗盡問題。</span><span class="sxs-lookup"><span data-stu-id="2f57e-472">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="2f57e-473">定期迴圈`HttpMessageHandler`實例以過時的 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="2f57e-473">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="2f57e-474">有其他方法可以使用長時間的<xref:System.Net.Http.SocketsHttpHandler>實例來解決上述問題。</span><span class="sxs-lookup"><span data-stu-id="2f57e-474">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="2f57e-475">`SocketsHttpHandler`當應用程式啟動時，建立的實例，並在應用程式的生命週期中使用它。</span><span class="sxs-lookup"><span data-stu-id="2f57e-475">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="2f57e-476">根據<xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> DNS 重新整理時間，設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="2f57e-476">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="2f57e-477">視`HttpClient`需要使用`new HttpClient(handler, disposeHandler: false)`建立實例。</span><span class="sxs-lookup"><span data-stu-id="2f57e-477">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="2f57e-478">上述方法可`IHttpClientFactory`解決以類似方式解決的資源管理問題。</span><span class="sxs-lookup"><span data-stu-id="2f57e-478">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="2f57e-479">會`SocketsHttpHandler`共用實例間`HttpClient`的連接。</span><span class="sxs-lookup"><span data-stu-id="2f57e-479">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="2f57e-480">此共用可防止通訊端耗盡。</span><span class="sxs-lookup"><span data-stu-id="2f57e-480">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="2f57e-481">會`SocketsHttpHandler`根據`PooledConnectionLifetime`來迴圈連接，以避免過時的 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="2f57e-481">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="2f57e-482">Cookie</span><span class="sxs-lookup"><span data-stu-id="2f57e-482">Cookies</span></span>

<span data-ttu-id="2f57e-483">集區`HttpMessageHandler`實例會導致`CookieContainer`共用物件。</span><span class="sxs-lookup"><span data-stu-id="2f57e-483">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="2f57e-484">意外`CookieContainer`的物件共用通常會導致不正確的程式碼。</span><span class="sxs-lookup"><span data-stu-id="2f57e-484">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="2f57e-485">針對需要 cookie 的應用程式，請考慮下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="2f57e-485">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="2f57e-486">停用自動 cookie 處理</span><span class="sxs-lookup"><span data-stu-id="2f57e-486">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="2f57e-487">以免`IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="2f57e-487">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="2f57e-488">呼叫<xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>以停用自動 cookie 處理：</span><span class="sxs-lookup"><span data-stu-id="2f57e-488">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="2f57e-489">記錄</span><span class="sxs-lookup"><span data-stu-id="2f57e-489">Logging</span></span>

<span data-ttu-id="2f57e-490">透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="2f57e-490">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="2f57e-491">在記錄設定中啟用適當的資訊層級，以查看預設記錄檔訊息。</span><span class="sxs-lookup"><span data-stu-id="2f57e-491">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="2f57e-492">額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。</span><span class="sxs-lookup"><span data-stu-id="2f57e-492">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="2f57e-493">用於每個用戶端的記錄檔分類包含用戶端的名稱。</span><span class="sxs-lookup"><span data-stu-id="2f57e-493">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="2f57e-494">例如，名為*MyNamedClient*的用戶端會記錄分類為的`System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`訊息。</span><span class="sxs-lookup"><span data-stu-id="2f57e-494">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="2f57e-495">後面加上 *LogicalHandler* 的訊息發生在要求處理常式管線之外。</span><span class="sxs-lookup"><span data-stu-id="2f57e-495">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="2f57e-496">在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。</span><span class="sxs-lookup"><span data-stu-id="2f57e-496">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="2f57e-497">在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。</span><span class="sxs-lookup"><span data-stu-id="2f57e-497">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="2f57e-498">記錄也會發生在要求處理常式管線之內。</span><span class="sxs-lookup"><span data-stu-id="2f57e-498">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="2f57e-499">在 *MyNamedClient* 範例中，那些訊息是針對記錄檔分類 `System.Net.Http.HttpClient.MyNamedClient.ClientHandler` 而記錄。</span><span class="sxs-lookup"><span data-stu-id="2f57e-499">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="2f57e-500">對於要求，這是發生在所有其他處理常式都已執行之後，並且緊接在網路上傳送要求之前。</span><span class="sxs-lookup"><span data-stu-id="2f57e-500">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="2f57e-501">在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。</span><span class="sxs-lookup"><span data-stu-id="2f57e-501">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="2f57e-502">在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。</span><span class="sxs-lookup"><span data-stu-id="2f57e-502">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="2f57e-503">例如，這可能包括要求標頭的變更，或是回應狀態碼的變更。</span><span class="sxs-lookup"><span data-stu-id="2f57e-503">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="2f57e-504">在記錄分類中包含用戶端的名稱，可讓您在需要時進行特定具名用戶端的記錄檔篩選。</span><span class="sxs-lookup"><span data-stu-id="2f57e-504">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="2f57e-505">設定 HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="2f57e-505">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="2f57e-506">可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。</span><span class="sxs-lookup"><span data-stu-id="2f57e-506">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="2f57e-507">新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。</span><span class="sxs-lookup"><span data-stu-id="2f57e-507">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="2f57e-508"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 擴充方法可以用來定義委派。</span><span class="sxs-lookup"><span data-stu-id="2f57e-508">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="2f57e-509">委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：</span><span class="sxs-lookup"><span data-stu-id="2f57e-509">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="2f57e-510">在主控台應用程式中使用 IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="2f57e-510">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="2f57e-511">在主控台應用程式中，將下列套件參考新增至專案：</span><span class="sxs-lookup"><span data-stu-id="2f57e-511">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="2f57e-512">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="2f57e-512">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="2f57e-513">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="2f57e-513">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="2f57e-514">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="2f57e-514">In the following example:</span></span>

* <span data-ttu-id="2f57e-515"><xref:System.Net.Http.IHttpClientFactory> 已在[泛型主機的](xref:fundamentals/host/generic-host)服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="2f57e-515"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="2f57e-516">`MyService` 會從服務建立用戶端 Factory 執行個體，其可用來建立 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="2f57e-516">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="2f57e-517">`HttpClient` 會用來擷取網頁。</span><span class="sxs-lookup"><span data-stu-id="2f57e-517">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="2f57e-518">`Main` 會建立範圍來執行服務的 `GetPage` 方法，並將網頁內容的前 500 個字元寫入至主控台。</span><span class="sxs-lookup"><span data-stu-id="2f57e-518">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="2f57e-519">其他資源</span><span class="sxs-lookup"><span data-stu-id="2f57e-519">Additional resources</span></span>

* [<span data-ttu-id="2f57e-520">使用 HttpClientFactory 實作復原 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="2f57e-520">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="2f57e-521">使用 HttpClientFactory 和 Polly 原則以指數輪詢實作 HTTP 呼叫重試</span><span class="sxs-lookup"><span data-stu-id="2f57e-521">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="2f57e-522">實作斷路器模式</span><span class="sxs-lookup"><span data-stu-id="2f57e-522">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="2f57e-523">作者：[Glenn Condron](https://github.com/glennc)、[Ryan Nowak](https://github.com/rynowak) 和 [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="2f57e-523">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="2f57e-524"><xref:System.Net.Http.IHttpClientFactory> 可以註冊及用來在應用程式中設定和建立 <xref:System.Net.Http.HttpClient> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="2f57e-524">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="2f57e-525">它提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="2f57e-525">It offers the following benefits:</span></span>

* <span data-ttu-id="2f57e-526">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="2f57e-526">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="2f57e-527">例如，您可以註冊並設定*github*用戶端來存取[github](https://github.com/)。</span><span class="sxs-lookup"><span data-stu-id="2f57e-527">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="2f57e-528">預設用戶端可以註冊用於其他用途。</span><span class="sxs-lookup"><span data-stu-id="2f57e-528">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="2f57e-529">透過委派 `HttpClient` 中的處理常式來撰寫外寄中介軟體的概念，並提供延伸模組以便 Polly 架構中介軟體利用外寄中介軟體。</span><span class="sxs-lookup"><span data-stu-id="2f57e-529">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="2f57e-530">管理基礎 `HttpClientMessageHandler` 執行個體的共用和存留期，以避免在手動管理 `HttpClient` 存留期時，發生的常見 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="2f57e-530">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="2f57e-531">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="2f57e-531">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="2f57e-532">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="2f57e-532">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2f57e-533">先決條件</span><span class="sxs-lookup"><span data-stu-id="2f57e-533">Prerequisites</span></span>

<span data-ttu-id="2f57e-534">以 .NET Framework 為目標的專案，需要安裝 [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="2f57e-534">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="2f57e-535">以 .NET Core 為目標且參考 [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) 的專案，已包含 `Microsoft.Extensions.Http` 套件。</span><span class="sxs-lookup"><span data-stu-id="2f57e-535">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="2f57e-536">耗用量模式</span><span class="sxs-lookup"><span data-stu-id="2f57e-536">Consumption patterns</span></span>

<span data-ttu-id="2f57e-537">有數種方式可將 `IHttpClientFactory` 用於應用程式：</span><span class="sxs-lookup"><span data-stu-id="2f57e-537">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="2f57e-538">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="2f57e-538">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="2f57e-539">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="2f57e-539">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="2f57e-540">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="2f57e-540">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="2f57e-541">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="2f57e-541">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="2f57e-542">它們全都不會嚴格優先於另一個。</span><span class="sxs-lookup"><span data-stu-id="2f57e-542">None of them are strictly superior to another.</span></span> <span data-ttu-id="2f57e-543">最好的方法取決於應用程式的條件約束。</span><span class="sxs-lookup"><span data-stu-id="2f57e-543">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="2f57e-544">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="2f57e-544">Basic usage</span></span>

<span data-ttu-id="2f57e-545">`IHttpClientFactory` 可以藉由在 `Startup.ConfigureServices` 方法內的 `IServiceCollection` 上呼叫 `AddHttpClient` 擴充方法來註冊。</span><span class="sxs-lookup"><span data-stu-id="2f57e-545">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="2f57e-546">註冊之後，程式碼可以接受`IHttpClientFactory`可以使用相依性[插入（DI）](xref:fundamentals/dependency-injection)插入的任何位置服務。</span><span class="sxs-lookup"><span data-stu-id="2f57e-546">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="2f57e-547">`IHttpClientFactory`可以用來建立`HttpClient`實例：</span><span class="sxs-lookup"><span data-stu-id="2f57e-547">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="2f57e-548">以這種方式使用 `IHttpClientFactory` 是重構現有應用程式的好方法。</span><span class="sxs-lookup"><span data-stu-id="2f57e-548">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="2f57e-549">它對 `HttpClient` 的使用方式沒有任何影響。</span><span class="sxs-lookup"><span data-stu-id="2f57e-549">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="2f57e-550">在目前建立 `HttpClient` 執行個體的位置，將那些項目取代為呼叫 <xref:System.Net.Http.IHttpClientFactory.CreateClient*>。</span><span class="sxs-lookup"><span data-stu-id="2f57e-550">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="2f57e-551">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="2f57e-551">Named clients</span></span>

<span data-ttu-id="2f57e-552">如果應用程式需要使用多個不同的 `HttpClient`，且每個都有不同的設定，可以選擇使用**具名用戶端**。</span><span class="sxs-lookup"><span data-stu-id="2f57e-552">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="2f57e-553">具名 `HttpClient` 的組態可以在 `Startup.ConfigureServices` 中註冊時指定。</span><span class="sxs-lookup"><span data-stu-id="2f57e-553">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="2f57e-554">在上述程式碼中`AddHttpClient` ，會呼叫，並提供名稱*github*。</span><span class="sxs-lookup"><span data-stu-id="2f57e-554">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="2f57e-555">此用戶端已套用一些預設組態&mdash;即使用 GitHub API 所需的基底位址和兩個標頭。</span><span class="sxs-lookup"><span data-stu-id="2f57e-555">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="2f57e-556">每次呼叫 `CreateClient` 時，會建立 `HttpClient` 的新執行個體並呼叫組態動作。</span><span class="sxs-lookup"><span data-stu-id="2f57e-556">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="2f57e-557">若要使用具名用戶端，可以傳遞字串參數至 `CreateClient`。</span><span class="sxs-lookup"><span data-stu-id="2f57e-557">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="2f57e-558">指定要建立之用戶端的名稱：</span><span class="sxs-lookup"><span data-stu-id="2f57e-558">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="2f57e-559">在上述程式碼中，要求不需要指定主機名稱。</span><span class="sxs-lookup"><span data-stu-id="2f57e-559">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="2f57e-560">它可以只傳遞路徑，因為已使用為用戶端設定的基底位址。</span><span class="sxs-lookup"><span data-stu-id="2f57e-560">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="2f57e-561">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="2f57e-561">Typed clients</span></span>

<span data-ttu-id="2f57e-562">具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="2f57e-562">Typed clients:</span></span>

* <span data-ttu-id="2f57e-563">提供與具名用戶端相同的功能，而不需使用字串作為索引鍵。</span><span class="sxs-lookup"><span data-stu-id="2f57e-563">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="2f57e-564">取用用戶端時提供 IntelliSense 和編譯器說明。</span><span class="sxs-lookup"><span data-stu-id="2f57e-564">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="2f57e-565">提供單一位置來設定特定的 `HttpClient` 並與其互動。</span><span class="sxs-lookup"><span data-stu-id="2f57e-565">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="2f57e-566">例如，單一的具型別用戶端可能用於單一的後端端點，並封裝處理該端點的所有邏輯。</span><span class="sxs-lookup"><span data-stu-id="2f57e-566">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="2f57e-567">使用 DI 且可在應用程式中需要之處插入。</span><span class="sxs-lookup"><span data-stu-id="2f57e-567">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="2f57e-568">具型別用戶端`HttpClient`會接受其函式中的參數：</span><span class="sxs-lookup"><span data-stu-id="2f57e-568">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="2f57e-569">在上述程式碼中，組態會移到具型別用戶端。</span><span class="sxs-lookup"><span data-stu-id="2f57e-569">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="2f57e-570">`HttpClient` 物件會公開為公用屬性。</span><span class="sxs-lookup"><span data-stu-id="2f57e-570">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="2f57e-571">您可定義 API 特定的方法，其公開 `HttpClient` 功能。</span><span class="sxs-lookup"><span data-stu-id="2f57e-571">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="2f57e-572">`GetAspNetDocsIssues` 方法會封裝從 GitHub 存放庫查詢和剖析最新開啟問題所需的程式碼。</span><span class="sxs-lookup"><span data-stu-id="2f57e-572">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="2f57e-573">若要註冊具型別用戶端，泛型 <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> 擴充方法可用於 `Startup.ConfigureServices` 內，並指定具型別用戶端類別：</span><span class="sxs-lookup"><span data-stu-id="2f57e-573">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="2f57e-574">具型別用戶端會向 DI 註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="2f57e-574">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="2f57e-575">具型別用戶端可以直接插入並使用：</span><span class="sxs-lookup"><span data-stu-id="2f57e-575">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="2f57e-576">想要的話，具型別用戶端的組態可以在 `Startup.ConfigureServices` 中註冊時指定，而不是在具型別用戶端的建構函式中：</span><span class="sxs-lookup"><span data-stu-id="2f57e-576">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="2f57e-577">可以將 `HttpClient` 完全封裝在具型別用戶端內。</span><span class="sxs-lookup"><span data-stu-id="2f57e-577">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="2f57e-578">可以提供在內部呼叫 `HttpClient` 執行個體的公用方法，而不將它公開為屬性。</span><span class="sxs-lookup"><span data-stu-id="2f57e-578">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="2f57e-579">在上述程式碼，`HttpClient` 儲存為私用欄位。</span><span class="sxs-lookup"><span data-stu-id="2f57e-579">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="2f57e-580">進行外部呼叫的所有存取都會經歷 `GetRepos` 方法。</span><span class="sxs-lookup"><span data-stu-id="2f57e-580">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="2f57e-581">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="2f57e-581">Generated clients</span></span>

<span data-ttu-id="2f57e-582">`IHttpClientFactory` 可和其他協力廠商程式庫一起使用，例如 [Refit](https://github.com/paulcbetts/refit)。</span><span class="sxs-lookup"><span data-stu-id="2f57e-582">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="2f57e-583">Refit 是適用於 .NET 的 REST 程式庫。</span><span class="sxs-lookup"><span data-stu-id="2f57e-583">Refit is a REST library for .NET.</span></span> <span data-ttu-id="2f57e-584">它將 REST API 轉換為即時介面。</span><span class="sxs-lookup"><span data-stu-id="2f57e-584">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="2f57e-585">介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。</span><span class="sxs-lookup"><span data-stu-id="2f57e-585">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="2f57e-586">定義介面及回覆來代表外部 API 和其回應：</span><span class="sxs-lookup"><span data-stu-id="2f57e-586">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="2f57e-587">可以新增具型別用戶端，使用 Refit 產生實作：</span><span class="sxs-lookup"><span data-stu-id="2f57e-587">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="2f57e-588">定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：</span><span class="sxs-lookup"><span data-stu-id="2f57e-588">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="2f57e-589">外寄要求中介軟體</span><span class="sxs-lookup"><span data-stu-id="2f57e-589">Outgoing request middleware</span></span>

<span data-ttu-id="2f57e-590">`HttpClient` 已經有委派可針對外寄 HTTP 要求連結在一起的處理常式的概念。</span><span class="sxs-lookup"><span data-stu-id="2f57e-590">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="2f57e-591">`IHttpClientFactory` 可讓您輕鬆地定義要套用於每個具名用戶端的處理常式。</span><span class="sxs-lookup"><span data-stu-id="2f57e-591">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="2f57e-592">它支援註冊和鏈結多個處理常式，以建置外寄要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="2f57e-592">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="2f57e-593">這些處理常式每個都可以在外寄要求之前和之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="2f57e-593">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="2f57e-594">此模式與 ASP.NET Core 中的輸入中介軟體管線相似。</span><span class="sxs-lookup"><span data-stu-id="2f57e-594">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="2f57e-595">模式提供一個機制來管理 HTTP 要求的跨領域關注，包括快取、錯誤處理、序列化和記錄。</span><span class="sxs-lookup"><span data-stu-id="2f57e-595">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="2f57e-596">若要建立處理常式，請定義衍生自 <xref:System.Net.Http.DelegatingHandler> 的類別。</span><span class="sxs-lookup"><span data-stu-id="2f57e-596">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="2f57e-597">覆寫 `SendAsync` 方法，以在將要求傳遞至管線中的下一個處理常式之前執行程式碼：</span><span class="sxs-lookup"><span data-stu-id="2f57e-597">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="2f57e-598">上述程式碼定義一個基本處理常式。</span><span class="sxs-lookup"><span data-stu-id="2f57e-598">The preceding code defines a basic handler.</span></span> <span data-ttu-id="2f57e-599">它會檢查以查看要求上是否已包含 `X-API-KEY` 標頭。</span><span class="sxs-lookup"><span data-stu-id="2f57e-599">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="2f57e-600">如果遺漏標頭，它可以避免 HTTP 呼叫，並傳回適當的回應。</span><span class="sxs-lookup"><span data-stu-id="2f57e-600">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="2f57e-601">在註冊期間，可以將一或多個處理常式新增至的`HttpClient`設定。</span><span class="sxs-lookup"><span data-stu-id="2f57e-601">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="2f57e-602">這項工作是透過 <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder> 上的擴充方法完成。</span><span class="sxs-lookup"><span data-stu-id="2f57e-602">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="2f57e-603">在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。</span><span class="sxs-lookup"><span data-stu-id="2f57e-603">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="2f57e-604">處理常式**必須**在 DI 中註冊為暫時性服務，無限定範圍。</span><span class="sxs-lookup"><span data-stu-id="2f57e-604">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="2f57e-605">如果處理常式已註冊為範圍服務，而且處理常式所相依的任何服務都是可處置的：</span><span class="sxs-lookup"><span data-stu-id="2f57e-605">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="2f57e-606">處理常式的服務可能會在處理常式超出範圍之前加以處置。</span><span class="sxs-lookup"><span data-stu-id="2f57e-606">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="2f57e-607">已處置的處理常式服務會導致處理常式失敗。</span><span class="sxs-lookup"><span data-stu-id="2f57e-607">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="2f57e-608">註冊之後，便可以呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>，並傳入處理常式類型。</span><span class="sxs-lookup"><span data-stu-id="2f57e-608">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="2f57e-609">可以遵循應該執行的順序來註冊多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="2f57e-609">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="2f57e-610">每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：</span><span class="sxs-lookup"><span data-stu-id="2f57e-610">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="2f57e-611">使用下列其中一種方式來與訊息處理常式共用個別要求狀態：</span><span class="sxs-lookup"><span data-stu-id="2f57e-611">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="2f57e-612">使用 `HttpRequestMessage.Properties` 將資料傳遞到處理常式。</span><span class="sxs-lookup"><span data-stu-id="2f57e-612">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="2f57e-613">使用 `IHttpContextAccessor` 來存取目前的要求。</span><span class="sxs-lookup"><span data-stu-id="2f57e-613">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="2f57e-614">建立自訂 `AsyncLocal` 儲存體物件以傳遞資料。</span><span class="sxs-lookup"><span data-stu-id="2f57e-614">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="2f57e-615">使用 Polly 為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="2f57e-615">Use Polly-based handlers</span></span>

<span data-ttu-id="2f57e-616">`IHttpClientFactory` 整合受歡迎的協力廠商程式庫，稱為 [Polly](https://github.com/App-vNext/Polly)。</span><span class="sxs-lookup"><span data-stu-id="2f57e-616">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="2f57e-617">Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。</span><span class="sxs-lookup"><span data-stu-id="2f57e-617">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="2f57e-618">它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。</span><span class="sxs-lookup"><span data-stu-id="2f57e-618">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="2f57e-619">提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="2f57e-619">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="2f57e-620">Polly 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="2f57e-620">The Polly extensions:</span></span>

* <span data-ttu-id="2f57e-621">支援將以 Polly 為基礎的處理常式新增至用戶端。</span><span class="sxs-lookup"><span data-stu-id="2f57e-621">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="2f57e-622">可在安裝 [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet 套件後使用。</span><span class="sxs-lookup"><span data-stu-id="2f57e-622">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="2f57e-623">該套件並未包含在 ASP.NET Core 共用架構中。</span><span class="sxs-lookup"><span data-stu-id="2f57e-623">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="2f57e-624">處理暫時性錯誤</span><span class="sxs-lookup"><span data-stu-id="2f57e-624">Handle transient faults</span></span>

<span data-ttu-id="2f57e-625">大部分的錯誤發生在外部 HTTP 呼叫是暫時性的時候。</span><span class="sxs-lookup"><span data-stu-id="2f57e-625">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="2f57e-626">包含一個便利的擴充方法，稱為 `AddTransientHttpErrorPolicy`，它可允許定義原則來處理暫時性錯誤。</span><span class="sxs-lookup"><span data-stu-id="2f57e-626">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="2f57e-627">使用此延伸模組方法設定的原則，會處理 `HttpRequestException`、HTTP 5xx 回應和 HTTP 408 回應。</span><span class="sxs-lookup"><span data-stu-id="2f57e-627">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="2f57e-628">`AddTransientHttpErrorPolicy` 延伸模組可用於 `Startup.ConfigureServices` 內。</span><span class="sxs-lookup"><span data-stu-id="2f57e-628">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="2f57e-629">延伸模組能提供 `PolicyBuilder` 物件的存取，該物件已設定來處理代表可能暫時性錯誤的錯誤：</span><span class="sxs-lookup"><span data-stu-id="2f57e-629">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="2f57e-630">在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。</span><span class="sxs-lookup"><span data-stu-id="2f57e-630">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="2f57e-631">失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。</span><span class="sxs-lookup"><span data-stu-id="2f57e-631">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="2f57e-632">動態選取原則</span><span class="sxs-lookup"><span data-stu-id="2f57e-632">Dynamically select policies</span></span>

<span data-ttu-id="2f57e-633">有額外的擴充方法可用來新增 Polly 為基礎的處理常式。</span><span class="sxs-lookup"><span data-stu-id="2f57e-633">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="2f57e-634">其中一個這類延伸模組是 `AddPolicyHandler`，它有多個多載。</span><span class="sxs-lookup"><span data-stu-id="2f57e-634">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="2f57e-635">一個多載可讓您在定義要套用的原則時，檢查要求：</span><span class="sxs-lookup"><span data-stu-id="2f57e-635">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="2f57e-636">在上述程式碼中，如果外寄要求是 HTTP GET，就會套用 10 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="2f57e-636">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="2f57e-637">任何其他 HTTP 方法會使用 30 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="2f57e-637">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="2f57e-638">新增多個 Polly 處理常式</span><span class="sxs-lookup"><span data-stu-id="2f57e-638">Add multiple Polly handlers</span></span>

<span data-ttu-id="2f57e-639">通常會建立巢狀 Polly 原則，以提供增強的功能：</span><span class="sxs-lookup"><span data-stu-id="2f57e-639">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="2f57e-640">在上述範例中，會新增兩個處理常式。</span><span class="sxs-lookup"><span data-stu-id="2f57e-640">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="2f57e-641">第一個使用 `AddTransientHttpErrorPolicy` 延伸模組來新增重試原則。</span><span class="sxs-lookup"><span data-stu-id="2f57e-641">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="2f57e-642">失敗的要求會重試最多三次。</span><span class="sxs-lookup"><span data-stu-id="2f57e-642">Failed requests are retried up to three times.</span></span> <span data-ttu-id="2f57e-643">第二個 `AddTransientHttpErrorPolicy` 呼叫會新增斷路器原則。</span><span class="sxs-lookup"><span data-stu-id="2f57e-643">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="2f57e-644">如果循序發生五次失敗的嘗試，進一步的外部要求會遭到封鎖 30 秒。</span><span class="sxs-lookup"><span data-stu-id="2f57e-644">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="2f57e-645">斷路器原則可設定狀態。</span><span class="sxs-lookup"><span data-stu-id="2f57e-645">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="2f57e-646">透過此用戶端的所有呼叫都會共用相同的線路狀態。</span><span class="sxs-lookup"><span data-stu-id="2f57e-646">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="2f57e-647">從 Polly 登錄新增原則</span><span class="sxs-lookup"><span data-stu-id="2f57e-647">Add policies from the Polly registry</span></span>

<span data-ttu-id="2f57e-648">管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。</span><span class="sxs-lookup"><span data-stu-id="2f57e-648">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="2f57e-649">提供了擴充方法，可以使用來自登錄的原則新增處理常式：</span><span class="sxs-lookup"><span data-stu-id="2f57e-649">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="2f57e-650">在上述程式碼中，當 `PolicyRegistry` 新增至 `ServiceCollection` 時，註冊了兩個原則。</span><span class="sxs-lookup"><span data-stu-id="2f57e-650">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="2f57e-651">為了使用來自登錄的原則，使用了 `AddPolicyHandlerFromRegistry` 方法，並傳遞要套用的原則名稱。</span><span class="sxs-lookup"><span data-stu-id="2f57e-651">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="2f57e-652">關於 `IHttpClientFactory` Polly 整合的詳細資訊，可以在 [Polly Wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory) 上找到。</span><span class="sxs-lookup"><span data-stu-id="2f57e-652">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="2f57e-653">HttpClient 和存留期管理</span><span class="sxs-lookup"><span data-stu-id="2f57e-653">HttpClient and lifetime management</span></span>

<span data-ttu-id="2f57e-654">每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回新的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="2f57e-654">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="2f57e-655">每個具名用戶端都有一個 <xref:System.Net.Http.HttpMessageHandler>。</span><span class="sxs-lookup"><span data-stu-id="2f57e-655">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="2f57e-656">處理站會管理 `HttpMessageHandler` 執行個體的存留期。</span><span class="sxs-lookup"><span data-stu-id="2f57e-656">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="2f57e-657">`IHttpClientFactory` 會將處理站所建立的 `HttpMessageHandler` 執行個體放入集區以減少資源耗用量。</span><span class="sxs-lookup"><span data-stu-id="2f57e-657">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="2f57e-658">建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。</span><span class="sxs-lookup"><span data-stu-id="2f57e-658">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="2f57e-659">將處理常式放入集區非常實用，因為處理常式通常會管理自己專屬的底層 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="2f57e-659">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="2f57e-660">建立比所需數目更多的處理常式，可能會導致連線延遲。</span><span class="sxs-lookup"><span data-stu-id="2f57e-660">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="2f57e-661">有些處理常式也會保持連線無限期地開啟，這可能導致處理常式無法對 DNS 變更回應。</span><span class="sxs-lookup"><span data-stu-id="2f57e-661">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="2f57e-662">預設處理常式存留時間為兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="2f57e-662">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="2f57e-663">可以針對每個具名用戶端覆寫預設值。</span><span class="sxs-lookup"><span data-stu-id="2f57e-663">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="2f57e-664">若要覆寫它，請在建立用戶端時所傳回的 `IHttpClientBuilder` 上呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*>：</span><span class="sxs-lookup"><span data-stu-id="2f57e-664">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="2f57e-665">不需要處置用戶端。</span><span class="sxs-lookup"><span data-stu-id="2f57e-665">Disposal of the client isn't required.</span></span> <span data-ttu-id="2f57e-666">處置會取消傳出的要求，並保證指定的 `HttpClient` 執行個體在呼叫 <xref:System.IDisposable.Dispose*> 之後無法使用。</span><span class="sxs-lookup"><span data-stu-id="2f57e-666">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="2f57e-667">`IHttpClientFactory` 會追蹤並處置 `HttpClient` 執行個體使用的資源。</span><span class="sxs-lookup"><span data-stu-id="2f57e-667">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="2f57e-668">`HttpClient` 執行個體通常可視為 .NET 物件，不需要處置。</span><span class="sxs-lookup"><span data-stu-id="2f57e-668">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="2f57e-669">在開始使用 `IHttpClientFactory` 之前，讓單一 `HttpClient` 執行個體維持一段較長的時間，是很常使用的模式。</span><span class="sxs-lookup"><span data-stu-id="2f57e-669">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="2f57e-670">在移轉到 `IHttpClientFactory` 之後，就不再需要此模式。</span><span class="sxs-lookup"><span data-stu-id="2f57e-670">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="2f57e-671">IHttpClientFactory 的替代方案</span><span class="sxs-lookup"><span data-stu-id="2f57e-671">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="2f57e-672">在`IHttpClientFactory`啟用 DI 的應用程式中使用，可避免：</span><span class="sxs-lookup"><span data-stu-id="2f57e-672">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="2f57e-673">共用`HttpMessageHandler`實例的資源耗盡問題。</span><span class="sxs-lookup"><span data-stu-id="2f57e-673">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="2f57e-674">定期迴圈`HttpMessageHandler`實例以過時的 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="2f57e-674">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="2f57e-675">有其他方法可以使用長時間的<xref:System.Net.Http.SocketsHttpHandler>實例來解決上述問題。</span><span class="sxs-lookup"><span data-stu-id="2f57e-675">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="2f57e-676">`SocketsHttpHandler`當應用程式啟動時，建立的實例，並在應用程式的生命週期中使用它。</span><span class="sxs-lookup"><span data-stu-id="2f57e-676">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="2f57e-677">根據<xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> DNS 重新整理時間，設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="2f57e-677">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="2f57e-678">視`HttpClient`需要使用`new HttpClient(handler, disposeHandler: false)`建立實例。</span><span class="sxs-lookup"><span data-stu-id="2f57e-678">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="2f57e-679">上述方法可`IHttpClientFactory`解決以類似方式解決的資源管理問題。</span><span class="sxs-lookup"><span data-stu-id="2f57e-679">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="2f57e-680">會`SocketsHttpHandler`共用實例間`HttpClient`的連接。</span><span class="sxs-lookup"><span data-stu-id="2f57e-680">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="2f57e-681">此共用可防止通訊端耗盡。</span><span class="sxs-lookup"><span data-stu-id="2f57e-681">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="2f57e-682">會`SocketsHttpHandler`根據`PooledConnectionLifetime`來迴圈連接，以避免過時的 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="2f57e-682">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="2f57e-683">Cookie</span><span class="sxs-lookup"><span data-stu-id="2f57e-683">Cookies</span></span>

<span data-ttu-id="2f57e-684">集區`HttpMessageHandler`實例會導致`CookieContainer`共用物件。</span><span class="sxs-lookup"><span data-stu-id="2f57e-684">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="2f57e-685">意外`CookieContainer`的物件共用通常會導致不正確的程式碼。</span><span class="sxs-lookup"><span data-stu-id="2f57e-685">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="2f57e-686">針對需要 cookie 的應用程式，請考慮下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="2f57e-686">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="2f57e-687">停用自動 cookie 處理</span><span class="sxs-lookup"><span data-stu-id="2f57e-687">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="2f57e-688">以免`IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="2f57e-688">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="2f57e-689">呼叫<xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>以停用自動 cookie 處理：</span><span class="sxs-lookup"><span data-stu-id="2f57e-689">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="2f57e-690">記錄</span><span class="sxs-lookup"><span data-stu-id="2f57e-690">Logging</span></span>

<span data-ttu-id="2f57e-691">透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="2f57e-691">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="2f57e-692">在記錄設定中啟用適當的資訊層級，以查看預設記錄檔訊息。</span><span class="sxs-lookup"><span data-stu-id="2f57e-692">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="2f57e-693">額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。</span><span class="sxs-lookup"><span data-stu-id="2f57e-693">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="2f57e-694">用於每個用戶端的記錄檔分類包含用戶端的名稱。</span><span class="sxs-lookup"><span data-stu-id="2f57e-694">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="2f57e-695">例如，名為*MyNamedClient*的用戶端會記錄分類為的`System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`訊息。</span><span class="sxs-lookup"><span data-stu-id="2f57e-695">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="2f57e-696">後面加上 *LogicalHandler* 的訊息發生在要求處理常式管線之外。</span><span class="sxs-lookup"><span data-stu-id="2f57e-696">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="2f57e-697">在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。</span><span class="sxs-lookup"><span data-stu-id="2f57e-697">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="2f57e-698">在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。</span><span class="sxs-lookup"><span data-stu-id="2f57e-698">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="2f57e-699">記錄也會發生在要求處理常式管線之內。</span><span class="sxs-lookup"><span data-stu-id="2f57e-699">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="2f57e-700">在 *MyNamedClient* 範例中，那些訊息是針對記錄檔分類 `System.Net.Http.HttpClient.MyNamedClient.ClientHandler` 而記錄。</span><span class="sxs-lookup"><span data-stu-id="2f57e-700">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="2f57e-701">對於要求，這是發生在所有其他處理常式都已執行之後，並且緊接在網路上傳送要求之前。</span><span class="sxs-lookup"><span data-stu-id="2f57e-701">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="2f57e-702">在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。</span><span class="sxs-lookup"><span data-stu-id="2f57e-702">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="2f57e-703">在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。</span><span class="sxs-lookup"><span data-stu-id="2f57e-703">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="2f57e-704">例如，這可能包括要求標頭的變更，或是回應狀態碼的變更。</span><span class="sxs-lookup"><span data-stu-id="2f57e-704">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="2f57e-705">在記錄分類中包含用戶端的名稱，可讓您在需要時進行特定具名用戶端的記錄檔篩選。</span><span class="sxs-lookup"><span data-stu-id="2f57e-705">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="2f57e-706">設定 HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="2f57e-706">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="2f57e-707">可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。</span><span class="sxs-lookup"><span data-stu-id="2f57e-707">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="2f57e-708">新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。</span><span class="sxs-lookup"><span data-stu-id="2f57e-708">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="2f57e-709"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 擴充方法可以用來定義委派。</span><span class="sxs-lookup"><span data-stu-id="2f57e-709">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="2f57e-710">委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：</span><span class="sxs-lookup"><span data-stu-id="2f57e-710">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="2f57e-711">在主控台應用程式中使用 IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="2f57e-711">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="2f57e-712">在主控台應用程式中，將下列套件參考新增至專案：</span><span class="sxs-lookup"><span data-stu-id="2f57e-712">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="2f57e-713">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="2f57e-713">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="2f57e-714">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="2f57e-714">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="2f57e-715">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="2f57e-715">In the following example:</span></span>

* <span data-ttu-id="2f57e-716"><xref:System.Net.Http.IHttpClientFactory> 已在[泛型主機的](xref:fundamentals/host/generic-host)服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="2f57e-716"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="2f57e-717">`MyService` 會從服務建立用戶端 Factory 執行個體，其可用來建立 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="2f57e-717">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="2f57e-718">`HttpClient` 會用來擷取網頁。</span><span class="sxs-lookup"><span data-stu-id="2f57e-718">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="2f57e-719">`Main` 會建立範圍來執行服務的 `GetPage` 方法，並將網頁內容的前 500 個字元寫入至主控台。</span><span class="sxs-lookup"><span data-stu-id="2f57e-719">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="header-propagation-middleware"></a><span data-ttu-id="2f57e-720">標頭傳播中介軟體</span><span class="sxs-lookup"><span data-stu-id="2f57e-720">Header propagation middleware</span></span>

<span data-ttu-id="2f57e-721">標頭傳播是一個支援的中介軟體，可將 HTTP 標頭從連入要求傳播到傳出的 HTTP 用戶端要求。</span><span class="sxs-lookup"><span data-stu-id="2f57e-721">Header propagation is a community supported middleware to propagate HTTP headers from the incoming request to the outgoing HTTP Client requests.</span></span> <span data-ttu-id="2f57e-722">若要使用標頭傳播：</span><span class="sxs-lookup"><span data-stu-id="2f57e-722">To use header propagation:</span></span>

* <span data-ttu-id="2f57e-723">參考[HeaderPropagation](https://www.nuget.org/packages/HeaderPropagation)套件的社區支援埠。</span><span class="sxs-lookup"><span data-stu-id="2f57e-723">Reference the community supported port of the package [HeaderPropagation](https://www.nuget.org/packages/HeaderPropagation).</span></span> <span data-ttu-id="2f57e-724">ASP.NET Core 3.1 和更新版本支援[AspNetCore. HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation)。</span><span class="sxs-lookup"><span data-stu-id="2f57e-724">ASP.NET Core 3.1 and later supports [Microsoft.AspNetCore.HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation).</span></span>

* <span data-ttu-id="2f57e-725">設定中介軟體和`HttpClient`中`Startup`的：</span><span class="sxs-lookup"><span data-stu-id="2f57e-725">Configure the middleware and `HttpClient` in `Startup`:</span></span>

  [!code-csharp[](http-requests/samples/2.x/Startup21.cs?highlight=5-9,25&name=snippet)]

* <span data-ttu-id="2f57e-726">用戶端會在輸出要求中包含已設定的標頭：</span><span class="sxs-lookup"><span data-stu-id="2f57e-726">The client includes the configured headers on outbound requests:</span></span>

  ```csharp
  var client = clientFactory.CreateClient("MyForwardingClient");
  var response = client.GetAsync(...);
  ```

## <a name="additional-resources"></a><span data-ttu-id="2f57e-727">其他資源</span><span class="sxs-lookup"><span data-stu-id="2f57e-727">Additional resources</span></span>

* [<span data-ttu-id="2f57e-728">使用 HttpClientFactory 實作復原 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="2f57e-728">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="2f57e-729">使用 HttpClientFactory 和 Polly 原則以指數輪詢實作 HTTP 呼叫重試</span><span class="sxs-lookup"><span data-stu-id="2f57e-729">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="2f57e-730">實作斷路器模式</span><span class="sxs-lookup"><span data-stu-id="2f57e-730">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
