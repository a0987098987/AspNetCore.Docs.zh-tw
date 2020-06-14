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
ms.openlocfilehash: a54861945d97728336149d5ffb39952c3d61b7bd
ms.sourcegitcommit: d243fadeda20ad4f142ea60301ae5f5e0d41ed60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/12/2020
ms.locfileid: "84724259"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="78d9b-103">在 ASP.NET Core 中使用 IHttpClientFactory 發出 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="78d9b-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="78d9b-104">作者： [Glenn Condron](https://github.com/glennc)、 [Ryan Nowak](https://github.com/rynowak)、 [Steve Gordon](https://github.com/stevejgordon)、 [Rick Anderson](https://twitter.com/RickAndMSFT)和[Kirk Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="78d9b-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="78d9b-105"><xref:System.Net.Http.IHttpClientFactory> 可以註冊及用來在應用程式中設定和建立 <xref:System.Net.Http.HttpClient> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="78d9b-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="78d9b-106">`IHttpClientFactory`提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="78d9b-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="78d9b-107">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="78d9b-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="78d9b-108">例如，名為*github*的用戶端可以註冊並設定為存取[github](https://github.com/)。</span><span class="sxs-lookup"><span data-stu-id="78d9b-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="78d9b-109">預設用戶端可以註冊以進行一般存取。</span><span class="sxs-lookup"><span data-stu-id="78d9b-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="78d9b-110">透過委派中的處理常式，制訂外寄中介軟體的概念 `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="78d9b-111">提供 Polly 為基礎中介軟體的延伸模組，以利用中的委派處理常式 `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="78d9b-112">管理基礎實例的共用和存留期 `HttpClientMessageHandler` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="78d9b-113">自動管理可避免在手動管理存留期時所發生的常見 DNS （網域名稱系統）問題 `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="78d9b-114">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="78d9b-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="78d9b-115">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="78d9b-115">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="78d9b-116">本主題中的範例程式碼會使用 <xref:System.Text.Json> 來還原序列化 HTTP 回應中所傳回的 JSON 內容。</span><span class="sxs-lookup"><span data-stu-id="78d9b-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="78d9b-117">如需使用和的範例 `Json.NET` `ReadAsAsync<T>` ，請使用版本選取器來選取此主題的2.x 版。</span><span class="sxs-lookup"><span data-stu-id="78d9b-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="78d9b-118">耗用量模式</span><span class="sxs-lookup"><span data-stu-id="78d9b-118">Consumption patterns</span></span>

<span data-ttu-id="78d9b-119">有數種方式可將 `IHttpClientFactory` 用於應用程式：</span><span class="sxs-lookup"><span data-stu-id="78d9b-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="78d9b-120">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="78d9b-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="78d9b-121">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="78d9b-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="78d9b-122">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="78d9b-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="78d9b-123">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="78d9b-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="78d9b-124">最佳方法取決於應用程式的需求。</span><span class="sxs-lookup"><span data-stu-id="78d9b-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="78d9b-125">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="78d9b-125">Basic usage</span></span>

<span data-ttu-id="78d9b-126">`IHttpClientFactory`可以藉由呼叫來註冊 `AddHttpClient` ：</span><span class="sxs-lookup"><span data-stu-id="78d9b-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="78d9b-127">`IHttpClientFactory`可以使用相依性[插入（DI）](xref:fundamentals/dependency-injection)要求。</span><span class="sxs-lookup"><span data-stu-id="78d9b-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="78d9b-128">下列程式碼會使用 `IHttpClientFactory` 來建立 `HttpClient` 實例：</span><span class="sxs-lookup"><span data-stu-id="78d9b-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="78d9b-129">在 `IHttpClientFactory` 上述範例中使用 like，是重構現有應用程式的好方法。</span><span class="sxs-lookup"><span data-stu-id="78d9b-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="78d9b-130">它不會影響使用方式 `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="78d9b-131">在 `HttpClient` 現有應用程式中建立實例的位置，使用對的呼叫來取代這些專案 <xref:System.Net.Http.IHttpClientFactory.CreateClient*> 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="78d9b-132">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="78d9b-132">Named clients</span></span>

<span data-ttu-id="78d9b-133">在下列情況中，命名的用戶端是不錯的選擇：</span><span class="sxs-lookup"><span data-stu-id="78d9b-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="78d9b-134">應用程式需要有許多不同的用途 `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="78d9b-135">許多 `HttpClient` 都有不同的設定。</span><span class="sxs-lookup"><span data-stu-id="78d9b-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="78d9b-136">`HttpClient`在中的註冊期間，可以指定名為的設定 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="78d9b-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="78d9b-137">在上述程式碼中，用戶端是使用下列設定：</span><span class="sxs-lookup"><span data-stu-id="78d9b-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="78d9b-138">基底位址 `https://api.github.com/` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="78d9b-139">使用 GitHub API 時需要兩個標頭。</span><span class="sxs-lookup"><span data-stu-id="78d9b-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="78d9b-140">CreateClient</span><span class="sxs-lookup"><span data-stu-id="78d9b-140">CreateClient</span></span>

<span data-ttu-id="78d9b-141">每次 <xref:System.Net.Http.IHttpClientFactory.CreateClient*> 呼叫時：</span><span class="sxs-lookup"><span data-stu-id="78d9b-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="78d9b-142">建立的新實例 `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="78d9b-143">會呼叫設定動作。</span><span class="sxs-lookup"><span data-stu-id="78d9b-143">The configuration action is called.</span></span>

<span data-ttu-id="78d9b-144">若要建立名為的用戶端，請將其名稱傳遞至 `CreateClient` ：</span><span class="sxs-lookup"><span data-stu-id="78d9b-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="78d9b-145">在上述程式碼中，要求不需要指定主機名稱。</span><span class="sxs-lookup"><span data-stu-id="78d9b-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="78d9b-146">此程式碼只會傳遞路徑，因為會使用為用戶端設定的基底位址。</span><span class="sxs-lookup"><span data-stu-id="78d9b-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="78d9b-147">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="78d9b-147">Typed clients</span></span>

<span data-ttu-id="78d9b-148">具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="78d9b-148">Typed clients:</span></span>

* <span data-ttu-id="78d9b-149">提供與具名用戶端相同的功能，而不需使用字串作為索引鍵。</span><span class="sxs-lookup"><span data-stu-id="78d9b-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="78d9b-150">取用用戶端時提供 IntelliSense 和編譯器說明。</span><span class="sxs-lookup"><span data-stu-id="78d9b-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="78d9b-151">提供單一位置來設定特定的 `HttpClient` 並與其互動。</span><span class="sxs-lookup"><span data-stu-id="78d9b-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="78d9b-152">例如，可能會使用單一具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="78d9b-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="78d9b-153">適用于單一後端端點。</span><span class="sxs-lookup"><span data-stu-id="78d9b-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="78d9b-154">封裝處理端點的所有邏輯。</span><span class="sxs-lookup"><span data-stu-id="78d9b-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="78d9b-155">使用 DI，並可在應用程式中需要的位置插入。</span><span class="sxs-lookup"><span data-stu-id="78d9b-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="78d9b-156">具型別用戶端會接受其函式 `HttpClient` 中的參數：</span><span class="sxs-lookup"><span data-stu-id="78d9b-156">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="78d9b-157">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="78d9b-157">In the preceding code:</span></span>

* <span data-ttu-id="78d9b-158">設定會移到具型別用戶端。</span><span class="sxs-lookup"><span data-stu-id="78d9b-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="78d9b-159">`HttpClient` 物件會公開為公用屬性。</span><span class="sxs-lookup"><span data-stu-id="78d9b-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="78d9b-160">可以建立可公開功能的 API 特定方法 `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="78d9b-161">例如，方法會 `GetAspNetDocsIssues` 封裝程式碼以取得未解決的問題。</span><span class="sxs-lookup"><span data-stu-id="78d9b-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="78d9b-162">下列程式碼會呼叫中的來註冊具型別 <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> `Startup.ConfigureServices` 用戶端類別：</span><span class="sxs-lookup"><span data-stu-id="78d9b-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="78d9b-163">具型別用戶端會向 DI 註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="78d9b-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="78d9b-164">在上述程式碼中，會將 `AddHttpClient` 註冊 `GitHubService` 為暫時性服務。</span><span class="sxs-lookup"><span data-stu-id="78d9b-164">In the preceding code, `AddHttpClient` registers `GitHubService` as a transient service.</span></span> <span data-ttu-id="78d9b-165">此註冊會使用 factory 方法來執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="78d9b-165">This registration uses a factory method to:</span></span>

1. <span data-ttu-id="78d9b-166">建立 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="78d9b-166">Create an instance of `HttpClient`.</span></span>
1. <span data-ttu-id="78d9b-167">建立的實例 `GitHubService` ，並將的實例傳入其函式 `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-167">Create an instance of `GitHubService`, passing in the instance of `HttpClient` to its constructor.</span></span>

<span data-ttu-id="78d9b-168">具型別用戶端可以直接插入並使用：</span><span class="sxs-lookup"><span data-stu-id="78d9b-168">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="78d9b-169">在中的註冊期間，可以指定具型別用戶端的設定 `Startup.ConfigureServices` ，而不是在具型別用戶端的函式中：</span><span class="sxs-lookup"><span data-stu-id="78d9b-169">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="78d9b-170">可以封裝在具型別 `HttpClient` 用戶端中。</span><span class="sxs-lookup"><span data-stu-id="78d9b-170">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="78d9b-171">請定義在內部呼叫實例的方法，而不是將它公開為屬性 `HttpClient` ：</span><span class="sxs-lookup"><span data-stu-id="78d9b-171">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="78d9b-172">在上述程式碼中， `HttpClient` 會儲存在私用欄位中。</span><span class="sxs-lookup"><span data-stu-id="78d9b-172">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="78d9b-173">的存取 `HttpClient` 是透過公用 `GetRepos` 方法。</span><span class="sxs-lookup"><span data-stu-id="78d9b-173">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="78d9b-174">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="78d9b-174">Generated clients</span></span>

<span data-ttu-id="78d9b-175">`IHttpClientFactory`可以與協力廠商程式庫搭配使用，例如重新[調整。](https://github.com/paulcbetts/refit)</span><span class="sxs-lookup"><span data-stu-id="78d9b-175">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="78d9b-176">Refit 是適用於 .NET 的 REST 程式庫。</span><span class="sxs-lookup"><span data-stu-id="78d9b-176">Refit is a REST library for .NET.</span></span> <span data-ttu-id="78d9b-177">它將 REST API 轉換為即時介面。</span><span class="sxs-lookup"><span data-stu-id="78d9b-177">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="78d9b-178">介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。</span><span class="sxs-lookup"><span data-stu-id="78d9b-178">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="78d9b-179">定義介面及回覆來代表外部 API 和其回應：</span><span class="sxs-lookup"><span data-stu-id="78d9b-179">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="78d9b-180">可以新增具型別用戶端，使用 Refit 產生實作：</span><span class="sxs-lookup"><span data-stu-id="78d9b-180">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="78d9b-181">定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：</span><span class="sxs-lookup"><span data-stu-id="78d9b-181">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="make-post-put-and-delete-requests"></a><span data-ttu-id="78d9b-182">進行 POST、PUT 和 DELETE 要求</span><span class="sxs-lookup"><span data-stu-id="78d9b-182">Make POST, PUT, and DELETE requests</span></span>

<span data-ttu-id="78d9b-183">在上述範例中，所有 HTTP 要求都會使用 GET HTTP 指令動詞。</span><span class="sxs-lookup"><span data-stu-id="78d9b-183">In the preceding examples, all HTTP requests use the GET HTTP verb.</span></span> <span data-ttu-id="78d9b-184">`HttpClient`也支援其他 HTTP 動詞命令，包括：</span><span class="sxs-lookup"><span data-stu-id="78d9b-184">`HttpClient` also supports other HTTP verbs, including:</span></span>

* <span data-ttu-id="78d9b-185">POST</span><span class="sxs-lookup"><span data-stu-id="78d9b-185">POST</span></span>
* <span data-ttu-id="78d9b-186">PUT</span><span class="sxs-lookup"><span data-stu-id="78d9b-186">PUT</span></span>
* <span data-ttu-id="78d9b-187">刪除</span><span class="sxs-lookup"><span data-stu-id="78d9b-187">DELETE</span></span>
* <span data-ttu-id="78d9b-188">PATCH</span><span class="sxs-lookup"><span data-stu-id="78d9b-188">PATCH</span></span>

<span data-ttu-id="78d9b-189">如需支援的 HTTP 動詞命令的完整清單，請參閱 <xref:System.Net.Http.HttpMethod> 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-189">For a complete list of supported HTTP verbs, see <xref:System.Net.Http.HttpMethod>.</span></span>

<span data-ttu-id="78d9b-190">下列範例顯示如何提出 HTTP POST 要求：</span><span class="sxs-lookup"><span data-stu-id="78d9b-190">The following example shows how to make an HTTP POST request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpRequestsSample/Models/TodoClient.cs?name=snippet_POST)]

<span data-ttu-id="78d9b-191">在上述程式碼中， `CreateItemAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="78d9b-191">In the preceding code, the `CreateItemAsync` method:</span></span>

* <span data-ttu-id="78d9b-192">使用將 `TodoItem` 參數序列化為 JSON `System.Text.Json` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-192">Serializes the `TodoItem` parameter to JSON using `System.Text.Json`.</span></span> <span data-ttu-id="78d9b-193">這會使用的實例 <xref:System.Text.Json.JsonSerializerOptions> 來設定序列化進程。</span><span class="sxs-lookup"><span data-stu-id="78d9b-193">This uses an instance of <xref:System.Text.Json.JsonSerializerOptions> to configure the serialization process.</span></span>
* <span data-ttu-id="78d9b-194">建立的實例 <xref:System.Net.Http.StringContent> ，以封裝序列化的 JSON，以便在 HTTP 要求的主體中傳送。</span><span class="sxs-lookup"><span data-stu-id="78d9b-194">Creates an instance of <xref:System.Net.Http.StringContent> to package the serialized JSON for sending in the HTTP request's body.</span></span>
* <span data-ttu-id="78d9b-195">呼叫 <xref:System.Net.Http.HttpClient.PostAsync%2A> ，將 JSON 內容傳送至指定的 URL。</span><span class="sxs-lookup"><span data-stu-id="78d9b-195">Calls <xref:System.Net.Http.HttpClient.PostAsync%2A> to send the JSON content to the specified URL.</span></span> <span data-ttu-id="78d9b-196">這是新增至 HttpClient 的相對 URL [。 BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress)。</span><span class="sxs-lookup"><span data-stu-id="78d9b-196">This is a relative URL that gets added to the [HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress).</span></span>
* <span data-ttu-id="78d9b-197"><xref:System.Net.Http.HttpResponseMessage.EnsureSuccessStatusCode%2A>如果回應狀態碼未指出成功，呼叫會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="78d9b-197">Calls <xref:System.Net.Http.HttpResponseMessage.EnsureSuccessStatusCode%2A> to throw an exception if the response status code does not indicate success.</span></span>

<span data-ttu-id="78d9b-198">`HttpClient`也支援其他類型的內容。</span><span class="sxs-lookup"><span data-stu-id="78d9b-198">`HttpClient` also supports other types of content.</span></span> <span data-ttu-id="78d9b-199">例如 <xref:System.Net.Http.MultipartContent> 和 <xref:System.Net.Http.StreamContent>。</span><span class="sxs-lookup"><span data-stu-id="78d9b-199">For example, <xref:System.Net.Http.MultipartContent> and <xref:System.Net.Http.StreamContent>.</span></span> <span data-ttu-id="78d9b-200">如需支援內容的完整清單，請參閱 <xref:System.Net.Http.HttpContent> 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-200">For a complete list of supported content, see <xref:System.Net.Http.HttpContent>.</span></span>

<span data-ttu-id="78d9b-201">下列範例顯示 HTTP PUT 要求：</span><span class="sxs-lookup"><span data-stu-id="78d9b-201">The following example shows an HTTP PUT request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpRequestsSample/Models/TodoClient.cs?name=snippet_PUT)]

<span data-ttu-id="78d9b-202">上述程式碼與 POST 範例非常類似。</span><span class="sxs-lookup"><span data-stu-id="78d9b-202">The preceding code is very similar to the POST example.</span></span> <span data-ttu-id="78d9b-203">`SaveItemAsync`方法會呼叫， <xref:System.Net.Http.HttpClient.PutAsync%2A> 而不是 `PostAsync` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-203">The `SaveItemAsync` method calls <xref:System.Net.Http.HttpClient.PutAsync%2A> instead of `PostAsync`.</span></span>

<span data-ttu-id="78d9b-204">下列範例顯示 HTTP DELETE 要求：</span><span class="sxs-lookup"><span data-stu-id="78d9b-204">The following example shows an HTTP DELETE request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpRequestsSample/Models/TodoClient.cs?name=snippet_DELETE)]

<span data-ttu-id="78d9b-205">在上述程式碼中， `DeleteItemAsync` 方法會呼叫 <xref:System.Net.Http.HttpClient.DeleteAsync%2A> 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-205">In the preceding code, the `DeleteItemAsync` method calls <xref:System.Net.Http.HttpClient.DeleteAsync%2A>.</span></span> <span data-ttu-id="78d9b-206">因為 HTTP DELETE 要求通常不會包含任何主體，所以 `DeleteAsync` 方法不會提供接受實例的多載 `HttpContent` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-206">Because HTTP DELETE requests typically contain no body, the `DeleteAsync` method doesn't provide an overload that accepts an instance of `HttpContent`.</span></span>

<span data-ttu-id="78d9b-207">若要深入瞭解如何搭配使用不同的 HTTP 動詞命令 `HttpClient` ，請參閱 <xref:System.Net.Http.HttpClient> 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-207">To learn more about using different HTTP verbs with `HttpClient`, see <xref:System.Net.Http.HttpClient>.</span></span>

## <a name="outgoing-request-middleware"></a><span data-ttu-id="78d9b-208">外寄要求中介軟體</span><span class="sxs-lookup"><span data-stu-id="78d9b-208">Outgoing request middleware</span></span>

<span data-ttu-id="78d9b-209">`HttpClient`具有委派處理常式的概念，可以連結在一起以用於傳出 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="78d9b-209">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="78d9b-210">`IHttpClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="78d9b-210">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="78d9b-211">簡化定義要套用至每個已命名用戶端的處理常式。</span><span class="sxs-lookup"><span data-stu-id="78d9b-211">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="78d9b-212">支援多個處理常式的註冊和連結，以建立外寄要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="78d9b-212">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="78d9b-213">這些處理常式每個都可以在外寄要求之前和之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="78d9b-213">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="78d9b-214">此模式：</span><span class="sxs-lookup"><span data-stu-id="78d9b-214">This pattern:</span></span>

  * <span data-ttu-id="78d9b-215">類似 ASP.NET Core 中的輸入中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="78d9b-215">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="78d9b-216">提供一種機制來管理 HTTP 要求的跨領域考慮，例如：</span><span class="sxs-lookup"><span data-stu-id="78d9b-216">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="78d9b-217">快取</span><span class="sxs-lookup"><span data-stu-id="78d9b-217">caching</span></span>
    * <span data-ttu-id="78d9b-218">錯誤處理</span><span class="sxs-lookup"><span data-stu-id="78d9b-218">error handling</span></span>
    * <span data-ttu-id="78d9b-219">序列化</span><span class="sxs-lookup"><span data-stu-id="78d9b-219">serialization</span></span>
    * <span data-ttu-id="78d9b-220">logging</span><span class="sxs-lookup"><span data-stu-id="78d9b-220">logging</span></span>

<span data-ttu-id="78d9b-221">若要建立委派處理常式：</span><span class="sxs-lookup"><span data-stu-id="78d9b-221">To create a delegating handler:</span></span>

* <span data-ttu-id="78d9b-222">衍生自 <xref:System.Net.Http.DelegatingHandler> 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-222">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="78d9b-223">覆寫 <xref:System.Net.Http.DelegatingHandler.SendAsync*>。</span><span class="sxs-lookup"><span data-stu-id="78d9b-223">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="78d9b-224">先執行程式碼，再將要求傳遞至管線中的下一個處理常式：</span><span class="sxs-lookup"><span data-stu-id="78d9b-224">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="78d9b-225">上述 `X-API-KEY` 程式碼會檢查標頭是否在要求中。</span><span class="sxs-lookup"><span data-stu-id="78d9b-225">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="78d9b-226">如果 `X-API-KEY` 遺失， <xref:System.Net.HttpStatusCode.BadRequest> 則會傳回。</span><span class="sxs-lookup"><span data-stu-id="78d9b-226">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="78d9b-227">有一個以上的處理常式可以加入至的設定 `HttpClient` <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName> ：</span><span class="sxs-lookup"><span data-stu-id="78d9b-227">More than one handler can be added to the configuration for an `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="78d9b-228">在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。</span><span class="sxs-lookup"><span data-stu-id="78d9b-228">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="78d9b-229">`IHttpClientFactory` 會為每個處理常式建立個別的 DI 範圍。</span><span class="sxs-lookup"><span data-stu-id="78d9b-229">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="78d9b-230">處理常式可以相依于任何範圍的服務。</span><span class="sxs-lookup"><span data-stu-id="78d9b-230">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="78d9b-231">處置處理常式時，會處置處理常式所相依的服務。</span><span class="sxs-lookup"><span data-stu-id="78d9b-231">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="78d9b-232">註冊之後，便可以呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>，並傳入處理常式的類型。</span><span class="sxs-lookup"><span data-stu-id="78d9b-232">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="78d9b-233">可以遵循應該執行的順序來註冊多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="78d9b-233">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="78d9b-234">每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：</span><span class="sxs-lookup"><span data-stu-id="78d9b-234">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="78d9b-235">使用下列其中一種方式來與訊息處理常式共用個別要求狀態：</span><span class="sxs-lookup"><span data-stu-id="78d9b-235">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="78d9b-236">使用[HttpRequestMessage](xref:System.Net.Http.HttpRequestMessage.Properties)將資料傳遞至處理常式。</span><span class="sxs-lookup"><span data-stu-id="78d9b-236">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="78d9b-237">使用 <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> 來存取目前的要求。</span><span class="sxs-lookup"><span data-stu-id="78d9b-237">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="78d9b-238">建立自訂 <xref:System.Threading.AsyncLocal`1> 儲存體物件以傳遞資料。</span><span class="sxs-lookup"><span data-stu-id="78d9b-238">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="78d9b-239">使用 Polly 為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="78d9b-239">Use Polly-based handlers</span></span>

<span data-ttu-id="78d9b-240">`IHttpClientFactory`與協力廠商程式庫[Polly](https://github.com/App-vNext/Polly)整合。</span><span class="sxs-lookup"><span data-stu-id="78d9b-240">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="78d9b-241">Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。</span><span class="sxs-lookup"><span data-stu-id="78d9b-241">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="78d9b-242">它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。</span><span class="sxs-lookup"><span data-stu-id="78d9b-242">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="78d9b-243">提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="78d9b-243">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="78d9b-244">Polly 擴充功能支援將以 Polly 為基礎的處理常式新增至用戶端。</span><span class="sxs-lookup"><span data-stu-id="78d9b-244">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="78d9b-245">Polly 需要[Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="78d9b-245">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="78d9b-246">處理暫時性錯誤</span><span class="sxs-lookup"><span data-stu-id="78d9b-246">Handle transient faults</span></span>

<span data-ttu-id="78d9b-247">當外部 HTTP 呼叫是暫時性的時，通常會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="78d9b-247">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="78d9b-248"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*>允許定義原則來處理暫時性錯誤。</span><span class="sxs-lookup"><span data-stu-id="78d9b-248"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="78d9b-249">使用設定的原則會 `AddTransientHttpErrorPolicy` 處理下列回應：</span><span class="sxs-lookup"><span data-stu-id="78d9b-249">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="78d9b-250">HTTP 5xx</span><span class="sxs-lookup"><span data-stu-id="78d9b-250">HTTP 5xx</span></span>
* <span data-ttu-id="78d9b-251">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="78d9b-251">HTTP 408</span></span>

<span data-ttu-id="78d9b-252">`AddTransientHttpErrorPolicy`提供物件的存取權，其 `PolicyBuilder` 設定用來處理代表可能暫時性錯誤的錯誤：</span><span class="sxs-lookup"><span data-stu-id="78d9b-252">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="78d9b-253">在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。</span><span class="sxs-lookup"><span data-stu-id="78d9b-253">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="78d9b-254">失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。</span><span class="sxs-lookup"><span data-stu-id="78d9b-254">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="78d9b-255">動態選取原則</span><span class="sxs-lookup"><span data-stu-id="78d9b-255">Dynamically select policies</span></span>

<span data-ttu-id="78d9b-256">提供擴充方法來加入以 Polly 為基礎的處理常式，例如 <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*> 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-256">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="78d9b-257">下列多載會 `AddPolicyHandler` 檢查要求以決定要套用的原則：</span><span class="sxs-lookup"><span data-stu-id="78d9b-257">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="78d9b-258">在上述程式碼中，如果外寄要求是 HTTP GET，就會套用 10 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="78d9b-258">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="78d9b-259">任何其他 HTTP 方法會使用 30 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="78d9b-259">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="78d9b-260">新增多個 Polly 處理常式</span><span class="sxs-lookup"><span data-stu-id="78d9b-260">Add multiple Polly handlers</span></span>

<span data-ttu-id="78d9b-261">通常會將 Polly 原則加以嵌套：</span><span class="sxs-lookup"><span data-stu-id="78d9b-261">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="78d9b-262">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="78d9b-262">In the preceding example:</span></span>

* <span data-ttu-id="78d9b-263">新增了兩個處理常式。</span><span class="sxs-lookup"><span data-stu-id="78d9b-263">Two handlers are added.</span></span>
* <span data-ttu-id="78d9b-264">第一個處理常式會使用 <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> 來新增重試原則。</span><span class="sxs-lookup"><span data-stu-id="78d9b-264">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="78d9b-265">失敗的要求會重試最多三次。</span><span class="sxs-lookup"><span data-stu-id="78d9b-265">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="78d9b-266">第二個 `AddTransientHttpErrorPolicy` 呼叫會加入斷路器原則。</span><span class="sxs-lookup"><span data-stu-id="78d9b-266">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="78d9b-267">如果連續5次嘗試失敗，則會封鎖進一步的外部要求30秒。</span><span class="sxs-lookup"><span data-stu-id="78d9b-267">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="78d9b-268">斷路器原則可設定狀態。</span><span class="sxs-lookup"><span data-stu-id="78d9b-268">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="78d9b-269">透過此用戶端的所有呼叫都會共用相同的線路狀態。</span><span class="sxs-lookup"><span data-stu-id="78d9b-269">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="78d9b-270">從 Polly 登錄新增原則</span><span class="sxs-lookup"><span data-stu-id="78d9b-270">Add policies from the Polly registry</span></span>

<span data-ttu-id="78d9b-271">管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。</span><span class="sxs-lookup"><span data-stu-id="78d9b-271">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="78d9b-272">在下列程式碼中：</span><span class="sxs-lookup"><span data-stu-id="78d9b-272">In the following code:</span></span>

* <span data-ttu-id="78d9b-273">系統會新增「一般」和「長」原則。</span><span class="sxs-lookup"><span data-stu-id="78d9b-273">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="78d9b-274"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>從登錄新增「一般」和「長」原則。</span><span class="sxs-lookup"><span data-stu-id="78d9b-274"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="78d9b-275">如需與 Polly 整合的詳細資訊 `IHttpClientFactory` ，請參閱[Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory)。</span><span class="sxs-lookup"><span data-stu-id="78d9b-275">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="78d9b-276">HttpClient 和存留期管理</span><span class="sxs-lookup"><span data-stu-id="78d9b-276">HttpClient and lifetime management</span></span>

<span data-ttu-id="78d9b-277">每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回新的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="78d9b-277">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="78d9b-278"><xref:System.Net.Http.HttpMessageHandler>會針對每個命名的用戶端建立。</span><span class="sxs-lookup"><span data-stu-id="78d9b-278">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="78d9b-279">處理站會管理 `HttpMessageHandler` 執行個體的存留期。</span><span class="sxs-lookup"><span data-stu-id="78d9b-279">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="78d9b-280">`IHttpClientFactory` 會將處理站所建立的 `HttpMessageHandler` 執行個體放入集區以減少資源耗用量。</span><span class="sxs-lookup"><span data-stu-id="78d9b-280">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="78d9b-281">建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。</span><span class="sxs-lookup"><span data-stu-id="78d9b-281">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="78d9b-282">將處理常式放入集區非常實用，因為處理常式通常會管理自己專屬的底層 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="78d9b-282">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="78d9b-283">建立比所需數目更多的處理常式，可能會導致連線延遲。</span><span class="sxs-lookup"><span data-stu-id="78d9b-283">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="78d9b-284">有些處理常式也會保持連線無限期地開啟，這可能導致處理常式無法回應 DNS （網域名稱系統）變更。</span><span class="sxs-lookup"><span data-stu-id="78d9b-284">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="78d9b-285">預設處理常式存留時間為兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="78d9b-285">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="78d9b-286">預設值可以根據每個命名的用戶端來覆寫：</span><span class="sxs-lookup"><span data-stu-id="78d9b-286">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="78d9b-287">`HttpClient`實例通常可視為**不**需要處置的 .net 物件。</span><span class="sxs-lookup"><span data-stu-id="78d9b-287">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="78d9b-288">處置會取消傳出的要求，並保證指定的 `HttpClient` 執行個體在呼叫 <xref:System.IDisposable.Dispose*> 之後無法使用。</span><span class="sxs-lookup"><span data-stu-id="78d9b-288">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="78d9b-289">`IHttpClientFactory` 會追蹤並處置 `HttpClient` 執行個體使用的資源。</span><span class="sxs-lookup"><span data-stu-id="78d9b-289">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="78d9b-290">在開始使用 `IHttpClientFactory` 之前，讓單一 `HttpClient` 執行個體維持一段較長的時間，是很常使用的模式。</span><span class="sxs-lookup"><span data-stu-id="78d9b-290">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="78d9b-291">在移轉到 `IHttpClientFactory` 之後，就不再需要此模式。</span><span class="sxs-lookup"><span data-stu-id="78d9b-291">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="78d9b-292">IHttpClientFactory 的替代方案</span><span class="sxs-lookup"><span data-stu-id="78d9b-292">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="78d9b-293">`IHttpClientFactory`在啟用 DI 的應用程式中使用，可避免：</span><span class="sxs-lookup"><span data-stu-id="78d9b-293">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="78d9b-294">共用實例的資源耗盡問題 `HttpMessageHandler` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-294">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="78d9b-295">定期迴圈實例以過時的 DNS 問題 `HttpMessageHandler` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-295">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="78d9b-296">有其他方法可以使用長時間的實例來解決上述問題 <xref:System.Net.Http.SocketsHttpHandler> 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-296">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="78d9b-297">`SocketsHttpHandler`當應用程式啟動時，建立的實例，並在應用程式的生命週期中使用它。</span><span class="sxs-lookup"><span data-stu-id="78d9b-297">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="78d9b-298"><xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime>根據 DNS 重新整理時間，設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="78d9b-298">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="78d9b-299">`HttpClient`視需要使用建立實例 `new HttpClient(handler, disposeHandler: false)` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-299">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="78d9b-300">上述方法可解決 `IHttpClientFactory` 以類似方式解決的資源管理問題。</span><span class="sxs-lookup"><span data-stu-id="78d9b-300">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="78d9b-301">會 `SocketsHttpHandler` 共用實例間的連接 `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-301">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="78d9b-302">此共用可防止通訊端耗盡。</span><span class="sxs-lookup"><span data-stu-id="78d9b-302">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="78d9b-303">會 `SocketsHttpHandler` 根據來迴圈連接 `PooledConnectionLifetime` ，以避免過時的 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="78d9b-303">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="78d9b-304">Cookie</span><span class="sxs-lookup"><span data-stu-id="78d9b-304">Cookies</span></span>

<span data-ttu-id="78d9b-305">集區 `HttpMessageHandler` 實例會導致 `CookieContainer` 共用物件。</span><span class="sxs-lookup"><span data-stu-id="78d9b-305">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="78d9b-306">意外 `CookieContainer` 的物件共用通常會導致不正確的程式碼。</span><span class="sxs-lookup"><span data-stu-id="78d9b-306">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="78d9b-307">針對需要 cookie 的應用程式，請考慮下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="78d9b-307">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="78d9b-308">停用自動 cookie 處理</span><span class="sxs-lookup"><span data-stu-id="78d9b-308">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="78d9b-309">以免`IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="78d9b-309">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="78d9b-310">呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 以停用自動 cookie 處理：</span><span class="sxs-lookup"><span data-stu-id="78d9b-310">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="78d9b-311">記錄</span><span class="sxs-lookup"><span data-stu-id="78d9b-311">Logging</span></span>

<span data-ttu-id="78d9b-312">透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="78d9b-312">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="78d9b-313">在記錄設定中啟用適當的資訊層級，以查看預設記錄檔訊息。</span><span class="sxs-lookup"><span data-stu-id="78d9b-313">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="78d9b-314">額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。</span><span class="sxs-lookup"><span data-stu-id="78d9b-314">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="78d9b-315">用於每個用戶端的記錄檔分類包含用戶端的名稱。</span><span class="sxs-lookup"><span data-stu-id="78d9b-315">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="78d9b-316">例如，名為*MyNamedClient*的用戶端會記錄類別為 "HttpClient" 的訊息。**MyNamedClient**。LogicalHandler "。</span><span class="sxs-lookup"><span data-stu-id="78d9b-316">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="78d9b-317">後面加上 *LogicalHandler* 的訊息發生在要求處理常式管線之外。</span><span class="sxs-lookup"><span data-stu-id="78d9b-317">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="78d9b-318">在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。</span><span class="sxs-lookup"><span data-stu-id="78d9b-318">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="78d9b-319">在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。</span><span class="sxs-lookup"><span data-stu-id="78d9b-319">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="78d9b-320">記錄也會發生在要求處理常式管線之內。</span><span class="sxs-lookup"><span data-stu-id="78d9b-320">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="78d9b-321">在*MyNamedClient*範例中，這些訊息會記錄在記錄檔類別中為 "HttpClient"。**MyNamedClient**。ClientHandler".</span><span class="sxs-lookup"><span data-stu-id="78d9b-321">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="78d9b-322">對於要求，這會在所有其他處理常式都執行之後，且在傳送要求之前立即發生。</span><span class="sxs-lookup"><span data-stu-id="78d9b-322">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="78d9b-323">在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。</span><span class="sxs-lookup"><span data-stu-id="78d9b-323">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="78d9b-324">在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。</span><span class="sxs-lookup"><span data-stu-id="78d9b-324">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="78d9b-325">這可能包括要求標頭的變更或回應狀態碼。</span><span class="sxs-lookup"><span data-stu-id="78d9b-325">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="78d9b-326">在記錄類別中包含用戶端的名稱，可以針對特定的已命名用戶端進行記錄篩選。</span><span class="sxs-lookup"><span data-stu-id="78d9b-326">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="78d9b-327">設定 HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="78d9b-327">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="78d9b-328">可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。</span><span class="sxs-lookup"><span data-stu-id="78d9b-328">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="78d9b-329">新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。</span><span class="sxs-lookup"><span data-stu-id="78d9b-329">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="78d9b-330"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 擴充方法可以用來定義委派。</span><span class="sxs-lookup"><span data-stu-id="78d9b-330">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="78d9b-331">委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：</span><span class="sxs-lookup"><span data-stu-id="78d9b-331">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="78d9b-332">在主控台應用程式中使用 IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="78d9b-332">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="78d9b-333">在主控台應用程式中，將下列套件參考新增至專案：</span><span class="sxs-lookup"><span data-stu-id="78d9b-333">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="78d9b-334">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="78d9b-334">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="78d9b-335">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="78d9b-335">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="78d9b-336">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="78d9b-336">In the following example:</span></span>

* <span data-ttu-id="78d9b-337"><xref:System.Net.Http.IHttpClientFactory> 已在[泛型主機的](xref:fundamentals/host/generic-host)服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="78d9b-337"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="78d9b-338">`MyService` 會從服務建立用戶端 Factory 執行個體，其可用來建立 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="78d9b-338">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="78d9b-339">`HttpClient` 會用來擷取網頁。</span><span class="sxs-lookup"><span data-stu-id="78d9b-339">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="78d9b-340">`Main` 會建立範圍來執行服務的 `GetPage` 方法，並將網頁內容的前 500 個字元寫入至主控台。</span><span class="sxs-lookup"><span data-stu-id="78d9b-340">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="header-propagation-middleware"></a><span data-ttu-id="78d9b-341">標頭傳播中介軟體</span><span class="sxs-lookup"><span data-stu-id="78d9b-341">Header propagation middleware</span></span>

<span data-ttu-id="78d9b-342">標頭傳播是一個 ASP.NET Core 中介軟體，可將 HTTP 標頭從傳入要求傳播到傳出的 HTTP 用戶端要求。</span><span class="sxs-lookup"><span data-stu-id="78d9b-342">Header propagation is an ASP.NET Core middleware to propagate HTTP headers from the incoming request to the outgoing HTTP Client requests.</span></span> <span data-ttu-id="78d9b-343">若要使用標頭傳播：</span><span class="sxs-lookup"><span data-stu-id="78d9b-343">To use header propagation:</span></span>

* <span data-ttu-id="78d9b-344">參考[AspNetCore. HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation)套件。</span><span class="sxs-lookup"><span data-stu-id="78d9b-344">Reference the [Microsoft.AspNetCore.HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation) package.</span></span>
* <span data-ttu-id="78d9b-345">設定中介軟體和 `HttpClient` 中的 `Startup` ：</span><span class="sxs-lookup"><span data-stu-id="78d9b-345">Configure the middleware and `HttpClient` in `Startup`:</span></span>

  [!code-csharp[](http-requests/samples/3.x/Startup.cs?highlight=5-9,21&name=snippet)]

* <span data-ttu-id="78d9b-346">用戶端會在輸出要求中包含已設定的標頭：</span><span class="sxs-lookup"><span data-stu-id="78d9b-346">The client includes the configured headers on outbound requests:</span></span>

  ```csharp
  var client = clientFactory.CreateClient("MyForwardingClient");
  var response = client.GetAsync(...);
  ```

## <a name="additional-resources"></a><span data-ttu-id="78d9b-347">其他資源</span><span class="sxs-lookup"><span data-stu-id="78d9b-347">Additional resources</span></span>

* [<span data-ttu-id="78d9b-348">使用 HttpClientFactory 實作復原 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="78d9b-348">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="78d9b-349">使用 HttpClientFactory 和 Polly 原則以指數輪詢實作 HTTP 呼叫重試</span><span class="sxs-lookup"><span data-stu-id="78d9b-349">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="78d9b-350">實作斷路器模式</span><span class="sxs-lookup"><span data-stu-id="78d9b-350">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)
* [<span data-ttu-id="78d9b-351">如何在 .NET 中序列化和還原序列化 JSON</span><span class="sxs-lookup"><span data-stu-id="78d9b-351">How to serialize and deserialize JSON in .NET</span></span>](/dotnet/standard/serialization/system-text-json-how-to)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="78d9b-352">作者：[Glenn Condron](https://github.com/glennc)、[Ryan Nowak](https://github.com/rynowak) 和 [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="78d9b-352">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="78d9b-353"><xref:System.Net.Http.IHttpClientFactory> 可以註冊及用來在應用程式中設定和建立 <xref:System.Net.Http.HttpClient> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="78d9b-353">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="78d9b-354">它提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="78d9b-354">It offers the following benefits:</span></span>

* <span data-ttu-id="78d9b-355">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="78d9b-355">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="78d9b-356">例如，您可以註冊並設定*github*用戶端來存取[github](https://github.com/)。</span><span class="sxs-lookup"><span data-stu-id="78d9b-356">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="78d9b-357">預設用戶端可以註冊用於其他用途。</span><span class="sxs-lookup"><span data-stu-id="78d9b-357">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="78d9b-358">透過委派 `HttpClient` 中的處理常式來撰寫外寄中介軟體的概念，並提供延伸模組以便 Polly 架構中介軟體利用外寄中介軟體。</span><span class="sxs-lookup"><span data-stu-id="78d9b-358">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="78d9b-359">管理基礎 `HttpClientMessageHandler` 執行個體的共用和存留期，以避免在手動管理 `HttpClient` 存留期時，發生的常見 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="78d9b-359">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="78d9b-360">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="78d9b-360">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="78d9b-361">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="78d9b-361">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="78d9b-362">耗用量模式</span><span class="sxs-lookup"><span data-stu-id="78d9b-362">Consumption patterns</span></span>

<span data-ttu-id="78d9b-363">有數種方式可將 `IHttpClientFactory` 用於應用程式：</span><span class="sxs-lookup"><span data-stu-id="78d9b-363">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="78d9b-364">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="78d9b-364">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="78d9b-365">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="78d9b-365">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="78d9b-366">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="78d9b-366">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="78d9b-367">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="78d9b-367">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="78d9b-368">它們全都不會嚴格優先於另一個。</span><span class="sxs-lookup"><span data-stu-id="78d9b-368">None of them are strictly superior to another.</span></span> <span data-ttu-id="78d9b-369">最好的方法取決於應用程式的條件約束。</span><span class="sxs-lookup"><span data-stu-id="78d9b-369">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="78d9b-370">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="78d9b-370">Basic usage</span></span>

<span data-ttu-id="78d9b-371">`IHttpClientFactory` 可以藉由在 `Startup.ConfigureServices` 方法內的 `IServiceCollection` 上呼叫 `AddHttpClient` 擴充方法來註冊。</span><span class="sxs-lookup"><span data-stu-id="78d9b-371">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="78d9b-372">註冊之後，程式碼可以接受 `IHttpClientFactory` 可以使用相依性[插入（DI）](xref:fundamentals/dependency-injection)插入的任何位置服務。</span><span class="sxs-lookup"><span data-stu-id="78d9b-372">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="78d9b-373">`IHttpClientFactory`可以用來建立 `HttpClient` 實例：</span><span class="sxs-lookup"><span data-stu-id="78d9b-373">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="78d9b-374">以這種方式使用 `IHttpClientFactory` 是重構現有應用程式的好方法。</span><span class="sxs-lookup"><span data-stu-id="78d9b-374">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="78d9b-375">它對 `HttpClient` 的使用方式沒有任何影響。</span><span class="sxs-lookup"><span data-stu-id="78d9b-375">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="78d9b-376">在目前建立 `HttpClient` 執行個體的位置，將那些項目取代為呼叫 <xref:System.Net.Http.IHttpClientFactory.CreateClient*>。</span><span class="sxs-lookup"><span data-stu-id="78d9b-376">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="78d9b-377">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="78d9b-377">Named clients</span></span>

<span data-ttu-id="78d9b-378">如果應用程式需要使用多個不同的 `HttpClient`，且每個都有不同的設定，可以選擇使用**具名用戶端**。</span><span class="sxs-lookup"><span data-stu-id="78d9b-378">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="78d9b-379">具名 `HttpClient` 的組態可以在 `Startup.ConfigureServices` 中註冊時指定。</span><span class="sxs-lookup"><span data-stu-id="78d9b-379">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="78d9b-380">在上述程式碼中， `AddHttpClient` 會呼叫，並提供名稱*github*。</span><span class="sxs-lookup"><span data-stu-id="78d9b-380">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="78d9b-381">此用戶端已套用一些預設組態&mdash;即使用 GitHub API 所需的基底位址和兩個標頭。</span><span class="sxs-lookup"><span data-stu-id="78d9b-381">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="78d9b-382">每次呼叫 `CreateClient` 時，會建立 `HttpClient` 的新執行個體並呼叫組態動作。</span><span class="sxs-lookup"><span data-stu-id="78d9b-382">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="78d9b-383">若要使用具名用戶端，可以傳遞字串參數至 `CreateClient`。</span><span class="sxs-lookup"><span data-stu-id="78d9b-383">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="78d9b-384">指定要建立之用戶端的名稱：</span><span class="sxs-lookup"><span data-stu-id="78d9b-384">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="78d9b-385">在上述程式碼中，要求不需要指定主機名稱。</span><span class="sxs-lookup"><span data-stu-id="78d9b-385">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="78d9b-386">它可以只傳遞路徑，因為已使用為用戶端設定的基底位址。</span><span class="sxs-lookup"><span data-stu-id="78d9b-386">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="78d9b-387">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="78d9b-387">Typed clients</span></span>

<span data-ttu-id="78d9b-388">具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="78d9b-388">Typed clients:</span></span>

* <span data-ttu-id="78d9b-389">提供與具名用戶端相同的功能，而不需使用字串作為索引鍵。</span><span class="sxs-lookup"><span data-stu-id="78d9b-389">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="78d9b-390">取用用戶端時提供 IntelliSense 和編譯器說明。</span><span class="sxs-lookup"><span data-stu-id="78d9b-390">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="78d9b-391">提供單一位置來設定特定的 `HttpClient` 並與其互動。</span><span class="sxs-lookup"><span data-stu-id="78d9b-391">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="78d9b-392">例如，單一的具型別用戶端可能用於單一的後端端點，並封裝處理該端點的所有邏輯。</span><span class="sxs-lookup"><span data-stu-id="78d9b-392">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="78d9b-393">使用 DI 且可在應用程式中需要之處插入。</span><span class="sxs-lookup"><span data-stu-id="78d9b-393">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="78d9b-394">具型別用戶端會接受其函式 `HttpClient` 中的參數：</span><span class="sxs-lookup"><span data-stu-id="78d9b-394">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="78d9b-395">在上述程式碼中，組態會移到具型別用戶端。</span><span class="sxs-lookup"><span data-stu-id="78d9b-395">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="78d9b-396">`HttpClient` 物件會公開為公用屬性。</span><span class="sxs-lookup"><span data-stu-id="78d9b-396">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="78d9b-397">您可定義 API 特定的方法，其公開 `HttpClient` 功能。</span><span class="sxs-lookup"><span data-stu-id="78d9b-397">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="78d9b-398">`GetAspNetDocsIssues` 方法會封裝從 GitHub 存放庫查詢和剖析最新開啟問題所需的程式碼。</span><span class="sxs-lookup"><span data-stu-id="78d9b-398">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="78d9b-399">若要註冊具型別用戶端，泛型 <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> 擴充方法可用於 `Startup.ConfigureServices` 內，並指定具型別用戶端類別：</span><span class="sxs-lookup"><span data-stu-id="78d9b-399">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="78d9b-400">具型別用戶端會向 DI 註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="78d9b-400">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="78d9b-401">具型別用戶端可以直接插入並使用：</span><span class="sxs-lookup"><span data-stu-id="78d9b-401">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="78d9b-402">想要的話，具型別用戶端的組態可以在 `Startup.ConfigureServices` 中註冊時指定，而不是在具型別用戶端的建構函式中：</span><span class="sxs-lookup"><span data-stu-id="78d9b-402">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="78d9b-403">可以將 `HttpClient` 完全封裝在具型別用戶端內。</span><span class="sxs-lookup"><span data-stu-id="78d9b-403">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="78d9b-404">可以提供在內部呼叫 `HttpClient` 執行個體的公用方法，而不將它公開為屬性。</span><span class="sxs-lookup"><span data-stu-id="78d9b-404">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="78d9b-405">在上述程式碼，`HttpClient` 儲存為私用欄位。</span><span class="sxs-lookup"><span data-stu-id="78d9b-405">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="78d9b-406">進行外部呼叫的所有存取都會經歷 `GetRepos` 方法。</span><span class="sxs-lookup"><span data-stu-id="78d9b-406">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="78d9b-407">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="78d9b-407">Generated clients</span></span>

<span data-ttu-id="78d9b-408">`IHttpClientFactory` 可和其他協力廠商程式庫一起使用，例如 [Refit](https://github.com/paulcbetts/refit)。</span><span class="sxs-lookup"><span data-stu-id="78d9b-408">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="78d9b-409">Refit 是適用於 .NET 的 REST 程式庫。</span><span class="sxs-lookup"><span data-stu-id="78d9b-409">Refit is a REST library for .NET.</span></span> <span data-ttu-id="78d9b-410">它將 REST API 轉換為即時介面。</span><span class="sxs-lookup"><span data-stu-id="78d9b-410">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="78d9b-411">介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。</span><span class="sxs-lookup"><span data-stu-id="78d9b-411">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="78d9b-412">定義介面及回覆來代表外部 API 和其回應：</span><span class="sxs-lookup"><span data-stu-id="78d9b-412">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="78d9b-413">可以新增具型別用戶端，使用 Refit 產生實作：</span><span class="sxs-lookup"><span data-stu-id="78d9b-413">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="78d9b-414">定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：</span><span class="sxs-lookup"><span data-stu-id="78d9b-414">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="78d9b-415">外寄要求中介軟體</span><span class="sxs-lookup"><span data-stu-id="78d9b-415">Outgoing request middleware</span></span>

<span data-ttu-id="78d9b-416">`HttpClient` 已經有委派可針對外寄 HTTP 要求連結在一起的處理常式的概念。</span><span class="sxs-lookup"><span data-stu-id="78d9b-416">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="78d9b-417">`IHttpClientFactory` 可讓您輕鬆地定義要套用於每個具名用戶端的處理常式。</span><span class="sxs-lookup"><span data-stu-id="78d9b-417">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="78d9b-418">它支援註冊和鏈結多個處理常式，以建置外寄要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="78d9b-418">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="78d9b-419">這些處理常式每個都可以在外寄要求之前和之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="78d9b-419">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="78d9b-420">此模式與 ASP.NET Core 中的輸入中介軟體管線相似。</span><span class="sxs-lookup"><span data-stu-id="78d9b-420">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="78d9b-421">模式提供一個機制來管理 HTTP 要求的跨領域關注，包括快取、錯誤處理、序列化和記錄。</span><span class="sxs-lookup"><span data-stu-id="78d9b-421">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="78d9b-422">若要建立處理常式，請定義衍生自 <xref:System.Net.Http.DelegatingHandler> 的類別。</span><span class="sxs-lookup"><span data-stu-id="78d9b-422">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="78d9b-423">覆寫 `SendAsync` 方法，以在將要求傳遞至管線中的下一個處理常式之前執行程式碼：</span><span class="sxs-lookup"><span data-stu-id="78d9b-423">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="78d9b-424">上述程式碼定義一個基本處理常式。</span><span class="sxs-lookup"><span data-stu-id="78d9b-424">The preceding code defines a basic handler.</span></span> <span data-ttu-id="78d9b-425">它會檢查以查看要求上是否已包含 `X-API-KEY` 標頭。</span><span class="sxs-lookup"><span data-stu-id="78d9b-425">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="78d9b-426">如果遺漏標頭，它可以避免 HTTP 呼叫，並傳回適當的回應。</span><span class="sxs-lookup"><span data-stu-id="78d9b-426">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="78d9b-427">在註冊期間，可以將一或多個處理常式新增至的設定 `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-427">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="78d9b-428">這項工作是透過 <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder> 上的擴充方法完成。</span><span class="sxs-lookup"><span data-stu-id="78d9b-428">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="78d9b-429">在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。</span><span class="sxs-lookup"><span data-stu-id="78d9b-429">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="78d9b-430">`IHttpClientFactory` 會為每個處理常式建立個別的 DI 範圍。</span><span class="sxs-lookup"><span data-stu-id="78d9b-430">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="78d9b-431">處理常式可相依於任何範圍的服務。</span><span class="sxs-lookup"><span data-stu-id="78d9b-431">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="78d9b-432">處置處理常式時，會處置處理常式所相依的服務。</span><span class="sxs-lookup"><span data-stu-id="78d9b-432">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="78d9b-433">註冊之後，便可以呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>，並傳入處理常式的類型。</span><span class="sxs-lookup"><span data-stu-id="78d9b-433">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="78d9b-434">可以遵循應該執行的順序來註冊多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="78d9b-434">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="78d9b-435">每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：</span><span class="sxs-lookup"><span data-stu-id="78d9b-435">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="78d9b-436">使用下列其中一種方式來與訊息處理常式共用個別要求狀態：</span><span class="sxs-lookup"><span data-stu-id="78d9b-436">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="78d9b-437">使用 `HttpRequestMessage.Properties` 將資料傳遞到處理常式。</span><span class="sxs-lookup"><span data-stu-id="78d9b-437">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="78d9b-438">使用 `IHttpContextAccessor` 來存取目前的要求。</span><span class="sxs-lookup"><span data-stu-id="78d9b-438">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="78d9b-439">建立自訂 `AsyncLocal` 儲存體物件以傳遞資料。</span><span class="sxs-lookup"><span data-stu-id="78d9b-439">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="78d9b-440">使用 Polly 為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="78d9b-440">Use Polly-based handlers</span></span>

<span data-ttu-id="78d9b-441">`IHttpClientFactory` 整合受歡迎的協力廠商程式庫，稱為 [Polly](https://github.com/App-vNext/Polly)。</span><span class="sxs-lookup"><span data-stu-id="78d9b-441">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="78d9b-442">Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。</span><span class="sxs-lookup"><span data-stu-id="78d9b-442">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="78d9b-443">它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。</span><span class="sxs-lookup"><span data-stu-id="78d9b-443">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="78d9b-444">提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="78d9b-444">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="78d9b-445">Polly 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="78d9b-445">The Polly extensions:</span></span>

* <span data-ttu-id="78d9b-446">支援將以 Polly 為基礎的處理常式新增至用戶端。</span><span class="sxs-lookup"><span data-stu-id="78d9b-446">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="78d9b-447">可在安裝 [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet 套件後使用。</span><span class="sxs-lookup"><span data-stu-id="78d9b-447">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="78d9b-448">該套件並未包含在 ASP.NET Core 共用架構中。</span><span class="sxs-lookup"><span data-stu-id="78d9b-448">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="78d9b-449">處理暫時性錯誤</span><span class="sxs-lookup"><span data-stu-id="78d9b-449">Handle transient faults</span></span>

<span data-ttu-id="78d9b-450">大部分的錯誤發生在外部 HTTP 呼叫是暫時性的時候。</span><span class="sxs-lookup"><span data-stu-id="78d9b-450">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="78d9b-451">包含一個便利的擴充方法，稱為 `AddTransientHttpErrorPolicy`，它可允許定義原則來處理暫時性錯誤。</span><span class="sxs-lookup"><span data-stu-id="78d9b-451">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="78d9b-452">使用此延伸模組方法設定的原則，會處理 `HttpRequestException`、HTTP 5xx 回應和 HTTP 408 回應。</span><span class="sxs-lookup"><span data-stu-id="78d9b-452">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="78d9b-453">`AddTransientHttpErrorPolicy` 延伸模組可用於 `Startup.ConfigureServices` 內。</span><span class="sxs-lookup"><span data-stu-id="78d9b-453">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="78d9b-454">延伸模組能提供 `PolicyBuilder` 物件的存取，該物件已設定來處理代表可能暫時性錯誤的錯誤：</span><span class="sxs-lookup"><span data-stu-id="78d9b-454">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="78d9b-455">在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。</span><span class="sxs-lookup"><span data-stu-id="78d9b-455">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="78d9b-456">失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。</span><span class="sxs-lookup"><span data-stu-id="78d9b-456">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="78d9b-457">動態選取原則</span><span class="sxs-lookup"><span data-stu-id="78d9b-457">Dynamically select policies</span></span>

<span data-ttu-id="78d9b-458">有額外的擴充方法可用來新增 Polly 為基礎的處理常式。</span><span class="sxs-lookup"><span data-stu-id="78d9b-458">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="78d9b-459">其中一個這類延伸模組是 `AddPolicyHandler`，它有多個多載。</span><span class="sxs-lookup"><span data-stu-id="78d9b-459">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="78d9b-460">一個多載可讓您在定義要套用的原則時，檢查要求：</span><span class="sxs-lookup"><span data-stu-id="78d9b-460">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="78d9b-461">在上述程式碼中，如果外寄要求是 HTTP GET，就會套用 10 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="78d9b-461">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="78d9b-462">任何其他 HTTP 方法會使用 30 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="78d9b-462">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="78d9b-463">新增多個 Polly 處理常式</span><span class="sxs-lookup"><span data-stu-id="78d9b-463">Add multiple Polly handlers</span></span>

<span data-ttu-id="78d9b-464">通常會建立巢狀 Polly 原則，以提供增強的功能：</span><span class="sxs-lookup"><span data-stu-id="78d9b-464">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="78d9b-465">在上述範例中，會新增兩個處理常式。</span><span class="sxs-lookup"><span data-stu-id="78d9b-465">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="78d9b-466">第一個使用 `AddTransientHttpErrorPolicy` 延伸模組來新增重試原則。</span><span class="sxs-lookup"><span data-stu-id="78d9b-466">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="78d9b-467">失敗的要求會重試最多三次。</span><span class="sxs-lookup"><span data-stu-id="78d9b-467">Failed requests are retried up to three times.</span></span> <span data-ttu-id="78d9b-468">第二個 `AddTransientHttpErrorPolicy` 呼叫會新增斷路器原則。</span><span class="sxs-lookup"><span data-stu-id="78d9b-468">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="78d9b-469">如果循序發生五次失敗的嘗試，進一步的外部要求會遭到封鎖 30 秒。</span><span class="sxs-lookup"><span data-stu-id="78d9b-469">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="78d9b-470">斷路器原則可設定狀態。</span><span class="sxs-lookup"><span data-stu-id="78d9b-470">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="78d9b-471">透過此用戶端的所有呼叫都會共用相同的線路狀態。</span><span class="sxs-lookup"><span data-stu-id="78d9b-471">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="78d9b-472">從 Polly 登錄新增原則</span><span class="sxs-lookup"><span data-stu-id="78d9b-472">Add policies from the Polly registry</span></span>

<span data-ttu-id="78d9b-473">管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。</span><span class="sxs-lookup"><span data-stu-id="78d9b-473">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="78d9b-474">提供了擴充方法，可以使用來自登錄的原則新增處理常式：</span><span class="sxs-lookup"><span data-stu-id="78d9b-474">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="78d9b-475">在上述程式碼中，當 `PolicyRegistry` 新增至 `ServiceCollection` 時，註冊了兩個原則。</span><span class="sxs-lookup"><span data-stu-id="78d9b-475">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="78d9b-476">為了使用來自登錄的原則，使用了 `AddPolicyHandlerFromRegistry` 方法，並傳遞要套用的原則名稱。</span><span class="sxs-lookup"><span data-stu-id="78d9b-476">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="78d9b-477">關於 `IHttpClientFactory` Polly 整合的詳細資訊，可以在 [Polly Wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory) 上找到。</span><span class="sxs-lookup"><span data-stu-id="78d9b-477">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="78d9b-478">HttpClient 和存留期管理</span><span class="sxs-lookup"><span data-stu-id="78d9b-478">HttpClient and lifetime management</span></span>

<span data-ttu-id="78d9b-479">每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回新的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="78d9b-479">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="78d9b-480">每個具名用戶端都有一個 <xref:System.Net.Http.HttpMessageHandler>。</span><span class="sxs-lookup"><span data-stu-id="78d9b-480">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="78d9b-481">處理站會管理 `HttpMessageHandler` 執行個體的存留期。</span><span class="sxs-lookup"><span data-stu-id="78d9b-481">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="78d9b-482">`IHttpClientFactory` 會將處理站所建立的 `HttpMessageHandler` 執行個體放入集區以減少資源耗用量。</span><span class="sxs-lookup"><span data-stu-id="78d9b-482">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="78d9b-483">建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。</span><span class="sxs-lookup"><span data-stu-id="78d9b-483">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="78d9b-484">將處理常式放入集區非常實用，因為處理常式通常會管理自己專屬的底層 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="78d9b-484">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="78d9b-485">建立比所需數目更多的處理常式，可能會導致連線延遲。</span><span class="sxs-lookup"><span data-stu-id="78d9b-485">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="78d9b-486">有些處理常式也會保持連線無限期地開啟，這可能導致處理常式無法對 DNS 變更回應。</span><span class="sxs-lookup"><span data-stu-id="78d9b-486">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="78d9b-487">預設處理常式存留時間為兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="78d9b-487">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="78d9b-488">可以針對每個具名用戶端覆寫預設值。</span><span class="sxs-lookup"><span data-stu-id="78d9b-488">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="78d9b-489">若要覆寫它，請在建立用戶端時所傳回的 `IHttpClientBuilder` 上呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*>：</span><span class="sxs-lookup"><span data-stu-id="78d9b-489">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="78d9b-490">不需要處置用戶端。</span><span class="sxs-lookup"><span data-stu-id="78d9b-490">Disposal of the client isn't required.</span></span> <span data-ttu-id="78d9b-491">處置會取消傳出的要求，並保證指定的 `HttpClient` 執行個體在呼叫 <xref:System.IDisposable.Dispose*> 之後無法使用。</span><span class="sxs-lookup"><span data-stu-id="78d9b-491">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="78d9b-492">`IHttpClientFactory` 會追蹤並處置 `HttpClient` 執行個體使用的資源。</span><span class="sxs-lookup"><span data-stu-id="78d9b-492">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="78d9b-493">`HttpClient` 執行個體通常可視為 .NET 物件，不需要處置。</span><span class="sxs-lookup"><span data-stu-id="78d9b-493">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="78d9b-494">在開始使用 `IHttpClientFactory` 之前，讓單一 `HttpClient` 執行個體維持一段較長的時間，是很常使用的模式。</span><span class="sxs-lookup"><span data-stu-id="78d9b-494">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="78d9b-495">在移轉到 `IHttpClientFactory` 之後，就不再需要此模式。</span><span class="sxs-lookup"><span data-stu-id="78d9b-495">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="78d9b-496">IHttpClientFactory 的替代方案</span><span class="sxs-lookup"><span data-stu-id="78d9b-496">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="78d9b-497">`IHttpClientFactory`在啟用 DI 的應用程式中使用，可避免：</span><span class="sxs-lookup"><span data-stu-id="78d9b-497">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="78d9b-498">共用實例的資源耗盡問題 `HttpMessageHandler` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-498">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="78d9b-499">定期迴圈實例以過時的 DNS 問題 `HttpMessageHandler` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-499">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="78d9b-500">有其他方法可以使用長時間的實例來解決上述問題 <xref:System.Net.Http.SocketsHttpHandler> 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-500">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="78d9b-501">`SocketsHttpHandler`當應用程式啟動時，建立的實例，並在應用程式的生命週期中使用它。</span><span class="sxs-lookup"><span data-stu-id="78d9b-501">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="78d9b-502"><xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime>根據 DNS 重新整理時間，設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="78d9b-502">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="78d9b-503">`HttpClient`視需要使用建立實例 `new HttpClient(handler, disposeHandler: false)` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-503">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="78d9b-504">上述方法可解決 `IHttpClientFactory` 以類似方式解決的資源管理問題。</span><span class="sxs-lookup"><span data-stu-id="78d9b-504">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="78d9b-505">會 `SocketsHttpHandler` 共用實例間的連接 `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-505">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="78d9b-506">此共用可防止通訊端耗盡。</span><span class="sxs-lookup"><span data-stu-id="78d9b-506">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="78d9b-507">會 `SocketsHttpHandler` 根據來迴圈連接 `PooledConnectionLifetime` ，以避免過時的 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="78d9b-507">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="78d9b-508">Cookie</span><span class="sxs-lookup"><span data-stu-id="78d9b-508">Cookies</span></span>

<span data-ttu-id="78d9b-509">集區 `HttpMessageHandler` 實例會導致 `CookieContainer` 共用物件。</span><span class="sxs-lookup"><span data-stu-id="78d9b-509">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="78d9b-510">意外 `CookieContainer` 的物件共用通常會導致不正確的程式碼。</span><span class="sxs-lookup"><span data-stu-id="78d9b-510">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="78d9b-511">針對需要 cookie 的應用程式，請考慮下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="78d9b-511">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="78d9b-512">停用自動 cookie 處理</span><span class="sxs-lookup"><span data-stu-id="78d9b-512">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="78d9b-513">以免`IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="78d9b-513">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="78d9b-514">呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 以停用自動 cookie 處理：</span><span class="sxs-lookup"><span data-stu-id="78d9b-514">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="78d9b-515">記錄</span><span class="sxs-lookup"><span data-stu-id="78d9b-515">Logging</span></span>

<span data-ttu-id="78d9b-516">透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="78d9b-516">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="78d9b-517">在記錄設定中啟用適當的資訊層級，以查看預設記錄檔訊息。</span><span class="sxs-lookup"><span data-stu-id="78d9b-517">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="78d9b-518">額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。</span><span class="sxs-lookup"><span data-stu-id="78d9b-518">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="78d9b-519">用於每個用戶端的記錄檔分類包含用戶端的名稱。</span><span class="sxs-lookup"><span data-stu-id="78d9b-519">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="78d9b-520">例如，名為*MyNamedClient*的用戶端會記錄分類為的訊息 `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-520">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="78d9b-521">後面加上 *LogicalHandler* 的訊息發生在要求處理常式管線之外。</span><span class="sxs-lookup"><span data-stu-id="78d9b-521">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="78d9b-522">在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。</span><span class="sxs-lookup"><span data-stu-id="78d9b-522">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="78d9b-523">在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。</span><span class="sxs-lookup"><span data-stu-id="78d9b-523">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="78d9b-524">記錄也會發生在要求處理常式管線之內。</span><span class="sxs-lookup"><span data-stu-id="78d9b-524">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="78d9b-525">在 *MyNamedClient* 範例中，那些訊息是針對記錄檔分類 `System.Net.Http.HttpClient.MyNamedClient.ClientHandler` 而記錄。</span><span class="sxs-lookup"><span data-stu-id="78d9b-525">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="78d9b-526">對於要求，這是發生在所有其他處理常式都已執行之後，並且緊接在網路上傳送要求之前。</span><span class="sxs-lookup"><span data-stu-id="78d9b-526">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="78d9b-527">在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。</span><span class="sxs-lookup"><span data-stu-id="78d9b-527">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="78d9b-528">在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。</span><span class="sxs-lookup"><span data-stu-id="78d9b-528">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="78d9b-529">例如，這可能包括要求標頭的變更，或是回應狀態碼的變更。</span><span class="sxs-lookup"><span data-stu-id="78d9b-529">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="78d9b-530">在記錄分類中包含用戶端的名稱，可讓您在需要時進行特定具名用戶端的記錄檔篩選。</span><span class="sxs-lookup"><span data-stu-id="78d9b-530">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="78d9b-531">設定 HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="78d9b-531">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="78d9b-532">可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。</span><span class="sxs-lookup"><span data-stu-id="78d9b-532">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="78d9b-533">新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。</span><span class="sxs-lookup"><span data-stu-id="78d9b-533">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="78d9b-534"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 擴充方法可以用來定義委派。</span><span class="sxs-lookup"><span data-stu-id="78d9b-534">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="78d9b-535">委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：</span><span class="sxs-lookup"><span data-stu-id="78d9b-535">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="78d9b-536">在主控台應用程式中使用 IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="78d9b-536">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="78d9b-537">在主控台應用程式中，將下列套件參考新增至專案：</span><span class="sxs-lookup"><span data-stu-id="78d9b-537">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="78d9b-538">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="78d9b-538">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="78d9b-539">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="78d9b-539">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="78d9b-540">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="78d9b-540">In the following example:</span></span>

* <span data-ttu-id="78d9b-541"><xref:System.Net.Http.IHttpClientFactory> 已在[泛型主機的](xref:fundamentals/host/generic-host)服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="78d9b-541"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="78d9b-542">`MyService` 會從服務建立用戶端 Factory 執行個體，其可用來建立 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="78d9b-542">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="78d9b-543">`HttpClient` 會用來擷取網頁。</span><span class="sxs-lookup"><span data-stu-id="78d9b-543">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="78d9b-544">`Main` 會建立範圍來執行服務的 `GetPage` 方法，並將網頁內容的前 500 個字元寫入至主控台。</span><span class="sxs-lookup"><span data-stu-id="78d9b-544">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="78d9b-545">其他資源</span><span class="sxs-lookup"><span data-stu-id="78d9b-545">Additional resources</span></span>

* [<span data-ttu-id="78d9b-546">使用 HttpClientFactory 實作復原 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="78d9b-546">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="78d9b-547">使用 HttpClientFactory 和 Polly 原則以指數輪詢實作 HTTP 呼叫重試</span><span class="sxs-lookup"><span data-stu-id="78d9b-547">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="78d9b-548">實作斷路器模式</span><span class="sxs-lookup"><span data-stu-id="78d9b-548">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="78d9b-549">作者：[Glenn Condron](https://github.com/glennc)、[Ryan Nowak](https://github.com/rynowak) 和 [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="78d9b-549">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="78d9b-550"><xref:System.Net.Http.IHttpClientFactory> 可以註冊及用來在應用程式中設定和建立 <xref:System.Net.Http.HttpClient> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="78d9b-550">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="78d9b-551">它提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="78d9b-551">It offers the following benefits:</span></span>

* <span data-ttu-id="78d9b-552">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="78d9b-552">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="78d9b-553">例如，您可以註冊並設定*github*用戶端來存取[github](https://github.com/)。</span><span class="sxs-lookup"><span data-stu-id="78d9b-553">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="78d9b-554">預設用戶端可以註冊用於其他用途。</span><span class="sxs-lookup"><span data-stu-id="78d9b-554">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="78d9b-555">透過委派 `HttpClient` 中的處理常式來撰寫外寄中介軟體的概念，並提供延伸模組以便 Polly 架構中介軟體利用外寄中介軟體。</span><span class="sxs-lookup"><span data-stu-id="78d9b-555">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="78d9b-556">管理基礎 `HttpClientMessageHandler` 執行個體的共用和存留期，以避免在手動管理 `HttpClient` 存留期時，發生的常見 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="78d9b-556">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="78d9b-557">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="78d9b-557">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="78d9b-558">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="78d9b-558">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="78d9b-559">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="78d9b-559">Prerequisites</span></span>

<span data-ttu-id="78d9b-560">以 .NET Framework 為目標的專案，需要安裝 [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="78d9b-560">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="78d9b-561">以 .NET Core 為目標且參考 [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) 的專案，已包含 `Microsoft.Extensions.Http` 套件。</span><span class="sxs-lookup"><span data-stu-id="78d9b-561">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="78d9b-562">耗用量模式</span><span class="sxs-lookup"><span data-stu-id="78d9b-562">Consumption patterns</span></span>

<span data-ttu-id="78d9b-563">有數種方式可將 `IHttpClientFactory` 用於應用程式：</span><span class="sxs-lookup"><span data-stu-id="78d9b-563">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="78d9b-564">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="78d9b-564">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="78d9b-565">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="78d9b-565">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="78d9b-566">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="78d9b-566">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="78d9b-567">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="78d9b-567">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="78d9b-568">它們全都不會嚴格優先於另一個。</span><span class="sxs-lookup"><span data-stu-id="78d9b-568">None of them are strictly superior to another.</span></span> <span data-ttu-id="78d9b-569">最好的方法取決於應用程式的條件約束。</span><span class="sxs-lookup"><span data-stu-id="78d9b-569">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="78d9b-570">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="78d9b-570">Basic usage</span></span>

<span data-ttu-id="78d9b-571">`IHttpClientFactory` 可以藉由在 `Startup.ConfigureServices` 方法內的 `IServiceCollection` 上呼叫 `AddHttpClient` 擴充方法來註冊。</span><span class="sxs-lookup"><span data-stu-id="78d9b-571">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="78d9b-572">註冊之後，程式碼可以接受 `IHttpClientFactory` 可以使用相依性[插入（DI）](xref:fundamentals/dependency-injection)插入的任何位置服務。</span><span class="sxs-lookup"><span data-stu-id="78d9b-572">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="78d9b-573">`IHttpClientFactory`可以用來建立 `HttpClient` 實例：</span><span class="sxs-lookup"><span data-stu-id="78d9b-573">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="78d9b-574">以這種方式使用 `IHttpClientFactory` 是重構現有應用程式的好方法。</span><span class="sxs-lookup"><span data-stu-id="78d9b-574">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="78d9b-575">它對 `HttpClient` 的使用方式沒有任何影響。</span><span class="sxs-lookup"><span data-stu-id="78d9b-575">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="78d9b-576">在目前建立 `HttpClient` 執行個體的位置，將那些項目取代為呼叫 <xref:System.Net.Http.IHttpClientFactory.CreateClient*>。</span><span class="sxs-lookup"><span data-stu-id="78d9b-576">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="78d9b-577">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="78d9b-577">Named clients</span></span>

<span data-ttu-id="78d9b-578">如果應用程式需要使用多個不同的 `HttpClient`，且每個都有不同的設定，可以選擇使用**具名用戶端**。</span><span class="sxs-lookup"><span data-stu-id="78d9b-578">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="78d9b-579">具名 `HttpClient` 的組態可以在 `Startup.ConfigureServices` 中註冊時指定。</span><span class="sxs-lookup"><span data-stu-id="78d9b-579">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="78d9b-580">在上述程式碼中， `AddHttpClient` 會呼叫，並提供名稱*github*。</span><span class="sxs-lookup"><span data-stu-id="78d9b-580">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="78d9b-581">此用戶端已套用一些預設組態&mdash;即使用 GitHub API 所需的基底位址和兩個標頭。</span><span class="sxs-lookup"><span data-stu-id="78d9b-581">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="78d9b-582">每次呼叫 `CreateClient` 時，會建立 `HttpClient` 的新執行個體並呼叫組態動作。</span><span class="sxs-lookup"><span data-stu-id="78d9b-582">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="78d9b-583">若要使用具名用戶端，可以傳遞字串參數至 `CreateClient`。</span><span class="sxs-lookup"><span data-stu-id="78d9b-583">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="78d9b-584">指定要建立之用戶端的名稱：</span><span class="sxs-lookup"><span data-stu-id="78d9b-584">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="78d9b-585">在上述程式碼中，要求不需要指定主機名稱。</span><span class="sxs-lookup"><span data-stu-id="78d9b-585">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="78d9b-586">它可以只傳遞路徑，因為已使用為用戶端設定的基底位址。</span><span class="sxs-lookup"><span data-stu-id="78d9b-586">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="78d9b-587">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="78d9b-587">Typed clients</span></span>

<span data-ttu-id="78d9b-588">具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="78d9b-588">Typed clients:</span></span>

* <span data-ttu-id="78d9b-589">提供與具名用戶端相同的功能，而不需使用字串作為索引鍵。</span><span class="sxs-lookup"><span data-stu-id="78d9b-589">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="78d9b-590">取用用戶端時提供 IntelliSense 和編譯器說明。</span><span class="sxs-lookup"><span data-stu-id="78d9b-590">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="78d9b-591">提供單一位置來設定特定的 `HttpClient` 並與其互動。</span><span class="sxs-lookup"><span data-stu-id="78d9b-591">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="78d9b-592">例如，單一的具型別用戶端可能用於單一的後端端點，並封裝處理該端點的所有邏輯。</span><span class="sxs-lookup"><span data-stu-id="78d9b-592">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="78d9b-593">使用 DI 且可在應用程式中需要之處插入。</span><span class="sxs-lookup"><span data-stu-id="78d9b-593">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="78d9b-594">具型別用戶端會接受其函式 `HttpClient` 中的參數：</span><span class="sxs-lookup"><span data-stu-id="78d9b-594">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="78d9b-595">在上述程式碼中，組態會移到具型別用戶端。</span><span class="sxs-lookup"><span data-stu-id="78d9b-595">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="78d9b-596">`HttpClient` 物件會公開為公用屬性。</span><span class="sxs-lookup"><span data-stu-id="78d9b-596">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="78d9b-597">您可定義 API 特定的方法，其公開 `HttpClient` 功能。</span><span class="sxs-lookup"><span data-stu-id="78d9b-597">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="78d9b-598">`GetAspNetDocsIssues` 方法會封裝從 GitHub 存放庫查詢和剖析最新開啟問題所需的程式碼。</span><span class="sxs-lookup"><span data-stu-id="78d9b-598">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="78d9b-599">若要註冊具型別用戶端，泛型 <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> 擴充方法可用於 `Startup.ConfigureServices` 內，並指定具型別用戶端類別：</span><span class="sxs-lookup"><span data-stu-id="78d9b-599">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="78d9b-600">具型別用戶端會向 DI 註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="78d9b-600">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="78d9b-601">具型別用戶端可以直接插入並使用：</span><span class="sxs-lookup"><span data-stu-id="78d9b-601">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="78d9b-602">想要的話，具型別用戶端的組態可以在 `Startup.ConfigureServices` 中註冊時指定，而不是在具型別用戶端的建構函式中：</span><span class="sxs-lookup"><span data-stu-id="78d9b-602">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="78d9b-603">可以將 `HttpClient` 完全封裝在具型別用戶端內。</span><span class="sxs-lookup"><span data-stu-id="78d9b-603">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="78d9b-604">可以提供在內部呼叫 `HttpClient` 執行個體的公用方法，而不將它公開為屬性。</span><span class="sxs-lookup"><span data-stu-id="78d9b-604">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="78d9b-605">在上述程式碼，`HttpClient` 儲存為私用欄位。</span><span class="sxs-lookup"><span data-stu-id="78d9b-605">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="78d9b-606">進行外部呼叫的所有存取都會經歷 `GetRepos` 方法。</span><span class="sxs-lookup"><span data-stu-id="78d9b-606">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="78d9b-607">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="78d9b-607">Generated clients</span></span>

<span data-ttu-id="78d9b-608">`IHttpClientFactory` 可和其他協力廠商程式庫一起使用，例如 [Refit](https://github.com/paulcbetts/refit)。</span><span class="sxs-lookup"><span data-stu-id="78d9b-608">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="78d9b-609">Refit 是適用於 .NET 的 REST 程式庫。</span><span class="sxs-lookup"><span data-stu-id="78d9b-609">Refit is a REST library for .NET.</span></span> <span data-ttu-id="78d9b-610">它將 REST API 轉換為即時介面。</span><span class="sxs-lookup"><span data-stu-id="78d9b-610">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="78d9b-611">介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。</span><span class="sxs-lookup"><span data-stu-id="78d9b-611">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="78d9b-612">定義介面及回覆來代表外部 API 和其回應：</span><span class="sxs-lookup"><span data-stu-id="78d9b-612">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="78d9b-613">可以新增具型別用戶端，使用 Refit 產生實作：</span><span class="sxs-lookup"><span data-stu-id="78d9b-613">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="78d9b-614">定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：</span><span class="sxs-lookup"><span data-stu-id="78d9b-614">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="78d9b-615">外寄要求中介軟體</span><span class="sxs-lookup"><span data-stu-id="78d9b-615">Outgoing request middleware</span></span>

<span data-ttu-id="78d9b-616">`HttpClient` 已經有委派可針對外寄 HTTP 要求連結在一起的處理常式的概念。</span><span class="sxs-lookup"><span data-stu-id="78d9b-616">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="78d9b-617">`IHttpClientFactory` 可讓您輕鬆地定義要套用於每個具名用戶端的處理常式。</span><span class="sxs-lookup"><span data-stu-id="78d9b-617">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="78d9b-618">它支援註冊和鏈結多個處理常式，以建置外寄要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="78d9b-618">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="78d9b-619">這些處理常式每個都可以在外寄要求之前和之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="78d9b-619">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="78d9b-620">此模式與 ASP.NET Core 中的輸入中介軟體管線相似。</span><span class="sxs-lookup"><span data-stu-id="78d9b-620">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="78d9b-621">模式提供一個機制來管理 HTTP 要求的跨領域關注，包括快取、錯誤處理、序列化和記錄。</span><span class="sxs-lookup"><span data-stu-id="78d9b-621">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="78d9b-622">若要建立處理常式，請定義衍生自 <xref:System.Net.Http.DelegatingHandler> 的類別。</span><span class="sxs-lookup"><span data-stu-id="78d9b-622">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="78d9b-623">覆寫 `SendAsync` 方法，以在將要求傳遞至管線中的下一個處理常式之前執行程式碼：</span><span class="sxs-lookup"><span data-stu-id="78d9b-623">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="78d9b-624">上述程式碼定義一個基本處理常式。</span><span class="sxs-lookup"><span data-stu-id="78d9b-624">The preceding code defines a basic handler.</span></span> <span data-ttu-id="78d9b-625">它會檢查以查看要求上是否已包含 `X-API-KEY` 標頭。</span><span class="sxs-lookup"><span data-stu-id="78d9b-625">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="78d9b-626">如果遺漏標頭，它可以避免 HTTP 呼叫，並傳回適當的回應。</span><span class="sxs-lookup"><span data-stu-id="78d9b-626">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="78d9b-627">在註冊期間，可以將一或多個處理常式新增至的設定 `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-627">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="78d9b-628">這項工作是透過 <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder> 上的擴充方法完成。</span><span class="sxs-lookup"><span data-stu-id="78d9b-628">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="78d9b-629">在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。</span><span class="sxs-lookup"><span data-stu-id="78d9b-629">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="78d9b-630">處理常式**必須**在 DI 中註冊為暫時性服務，無限定範圍。</span><span class="sxs-lookup"><span data-stu-id="78d9b-630">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="78d9b-631">如果處理常式已註冊為範圍服務，而且處理常式所相依的任何服務都是可處置的：</span><span class="sxs-lookup"><span data-stu-id="78d9b-631">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="78d9b-632">處理常式的服務可能會在處理常式超出範圍之前加以處置。</span><span class="sxs-lookup"><span data-stu-id="78d9b-632">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="78d9b-633">已處置的處理常式服務會導致處理常式失敗。</span><span class="sxs-lookup"><span data-stu-id="78d9b-633">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="78d9b-634">註冊之後，便可以呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>，並傳入處理常式類型。</span><span class="sxs-lookup"><span data-stu-id="78d9b-634">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="78d9b-635">可以遵循應該執行的順序來註冊多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="78d9b-635">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="78d9b-636">每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：</span><span class="sxs-lookup"><span data-stu-id="78d9b-636">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="78d9b-637">使用下列其中一種方式來與訊息處理常式共用個別要求狀態：</span><span class="sxs-lookup"><span data-stu-id="78d9b-637">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="78d9b-638">使用 `HttpRequestMessage.Properties` 將資料傳遞到處理常式。</span><span class="sxs-lookup"><span data-stu-id="78d9b-638">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="78d9b-639">使用 `IHttpContextAccessor` 來存取目前的要求。</span><span class="sxs-lookup"><span data-stu-id="78d9b-639">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="78d9b-640">建立自訂 `AsyncLocal` 儲存體物件以傳遞資料。</span><span class="sxs-lookup"><span data-stu-id="78d9b-640">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="78d9b-641">使用 Polly 為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="78d9b-641">Use Polly-based handlers</span></span>

<span data-ttu-id="78d9b-642">`IHttpClientFactory` 整合受歡迎的協力廠商程式庫，稱為 [Polly](https://github.com/App-vNext/Polly)。</span><span class="sxs-lookup"><span data-stu-id="78d9b-642">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="78d9b-643">Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。</span><span class="sxs-lookup"><span data-stu-id="78d9b-643">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="78d9b-644">它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。</span><span class="sxs-lookup"><span data-stu-id="78d9b-644">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="78d9b-645">提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="78d9b-645">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="78d9b-646">Polly 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="78d9b-646">The Polly extensions:</span></span>

* <span data-ttu-id="78d9b-647">支援將以 Polly 為基礎的處理常式新增至用戶端。</span><span class="sxs-lookup"><span data-stu-id="78d9b-647">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="78d9b-648">可在安裝 [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet 套件後使用。</span><span class="sxs-lookup"><span data-stu-id="78d9b-648">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="78d9b-649">該套件並未包含在 ASP.NET Core 共用架構中。</span><span class="sxs-lookup"><span data-stu-id="78d9b-649">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="78d9b-650">處理暫時性錯誤</span><span class="sxs-lookup"><span data-stu-id="78d9b-650">Handle transient faults</span></span>

<span data-ttu-id="78d9b-651">大部分的錯誤發生在外部 HTTP 呼叫是暫時性的時候。</span><span class="sxs-lookup"><span data-stu-id="78d9b-651">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="78d9b-652">包含一個便利的擴充方法，稱為 `AddTransientHttpErrorPolicy`，它可允許定義原則來處理暫時性錯誤。</span><span class="sxs-lookup"><span data-stu-id="78d9b-652">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="78d9b-653">使用此延伸模組方法設定的原則，會處理 `HttpRequestException`、HTTP 5xx 回應和 HTTP 408 回應。</span><span class="sxs-lookup"><span data-stu-id="78d9b-653">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="78d9b-654">`AddTransientHttpErrorPolicy` 延伸模組可用於 `Startup.ConfigureServices` 內。</span><span class="sxs-lookup"><span data-stu-id="78d9b-654">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="78d9b-655">延伸模組能提供 `PolicyBuilder` 物件的存取，該物件已設定來處理代表可能暫時性錯誤的錯誤：</span><span class="sxs-lookup"><span data-stu-id="78d9b-655">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="78d9b-656">在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。</span><span class="sxs-lookup"><span data-stu-id="78d9b-656">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="78d9b-657">失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。</span><span class="sxs-lookup"><span data-stu-id="78d9b-657">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="78d9b-658">動態選取原則</span><span class="sxs-lookup"><span data-stu-id="78d9b-658">Dynamically select policies</span></span>

<span data-ttu-id="78d9b-659">有額外的擴充方法可用來新增 Polly 為基礎的處理常式。</span><span class="sxs-lookup"><span data-stu-id="78d9b-659">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="78d9b-660">其中一個這類延伸模組是 `AddPolicyHandler`，它有多個多載。</span><span class="sxs-lookup"><span data-stu-id="78d9b-660">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="78d9b-661">一個多載可讓您在定義要套用的原則時，檢查要求：</span><span class="sxs-lookup"><span data-stu-id="78d9b-661">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="78d9b-662">在上述程式碼中，如果外寄要求是 HTTP GET，就會套用 10 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="78d9b-662">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="78d9b-663">任何其他 HTTP 方法會使用 30 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="78d9b-663">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="78d9b-664">新增多個 Polly 處理常式</span><span class="sxs-lookup"><span data-stu-id="78d9b-664">Add multiple Polly handlers</span></span>

<span data-ttu-id="78d9b-665">通常會建立巢狀 Polly 原則，以提供增強的功能：</span><span class="sxs-lookup"><span data-stu-id="78d9b-665">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="78d9b-666">在上述範例中，會新增兩個處理常式。</span><span class="sxs-lookup"><span data-stu-id="78d9b-666">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="78d9b-667">第一個使用 `AddTransientHttpErrorPolicy` 延伸模組來新增重試原則。</span><span class="sxs-lookup"><span data-stu-id="78d9b-667">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="78d9b-668">失敗的要求會重試最多三次。</span><span class="sxs-lookup"><span data-stu-id="78d9b-668">Failed requests are retried up to three times.</span></span> <span data-ttu-id="78d9b-669">第二個 `AddTransientHttpErrorPolicy` 呼叫會新增斷路器原則。</span><span class="sxs-lookup"><span data-stu-id="78d9b-669">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="78d9b-670">如果循序發生五次失敗的嘗試，進一步的外部要求會遭到封鎖 30 秒。</span><span class="sxs-lookup"><span data-stu-id="78d9b-670">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="78d9b-671">斷路器原則可設定狀態。</span><span class="sxs-lookup"><span data-stu-id="78d9b-671">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="78d9b-672">透過此用戶端的所有呼叫都會共用相同的線路狀態。</span><span class="sxs-lookup"><span data-stu-id="78d9b-672">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="78d9b-673">從 Polly 登錄新增原則</span><span class="sxs-lookup"><span data-stu-id="78d9b-673">Add policies from the Polly registry</span></span>

<span data-ttu-id="78d9b-674">管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。</span><span class="sxs-lookup"><span data-stu-id="78d9b-674">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="78d9b-675">提供了擴充方法，可以使用來自登錄的原則新增處理常式：</span><span class="sxs-lookup"><span data-stu-id="78d9b-675">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="78d9b-676">在上述程式碼中，當 `PolicyRegistry` 新增至 `ServiceCollection` 時，註冊了兩個原則。</span><span class="sxs-lookup"><span data-stu-id="78d9b-676">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="78d9b-677">為了使用來自登錄的原則，使用了 `AddPolicyHandlerFromRegistry` 方法，並傳遞要套用的原則名稱。</span><span class="sxs-lookup"><span data-stu-id="78d9b-677">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="78d9b-678">關於 `IHttpClientFactory` Polly 整合的詳細資訊，可以在 [Polly Wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory) 上找到。</span><span class="sxs-lookup"><span data-stu-id="78d9b-678">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="78d9b-679">HttpClient 和存留期管理</span><span class="sxs-lookup"><span data-stu-id="78d9b-679">HttpClient and lifetime management</span></span>

<span data-ttu-id="78d9b-680">每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回新的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="78d9b-680">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="78d9b-681">每個具名用戶端都有一個 <xref:System.Net.Http.HttpMessageHandler>。</span><span class="sxs-lookup"><span data-stu-id="78d9b-681">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="78d9b-682">處理站會管理 `HttpMessageHandler` 執行個體的存留期。</span><span class="sxs-lookup"><span data-stu-id="78d9b-682">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="78d9b-683">`IHttpClientFactory` 會將處理站所建立的 `HttpMessageHandler` 執行個體放入集區以減少資源耗用量。</span><span class="sxs-lookup"><span data-stu-id="78d9b-683">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="78d9b-684">建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。</span><span class="sxs-lookup"><span data-stu-id="78d9b-684">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="78d9b-685">將處理常式放入集區非常實用，因為處理常式通常會管理自己專屬的底層 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="78d9b-685">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="78d9b-686">建立比所需數目更多的處理常式，可能會導致連線延遲。</span><span class="sxs-lookup"><span data-stu-id="78d9b-686">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="78d9b-687">有些處理常式也會保持連線無限期地開啟，這可能導致處理常式無法對 DNS 變更回應。</span><span class="sxs-lookup"><span data-stu-id="78d9b-687">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="78d9b-688">預設處理常式存留時間為兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="78d9b-688">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="78d9b-689">可以針對每個具名用戶端覆寫預設值。</span><span class="sxs-lookup"><span data-stu-id="78d9b-689">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="78d9b-690">若要覆寫它，請在建立用戶端時所傳回的 `IHttpClientBuilder` 上呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*>：</span><span class="sxs-lookup"><span data-stu-id="78d9b-690">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="78d9b-691">不需要處置用戶端。</span><span class="sxs-lookup"><span data-stu-id="78d9b-691">Disposal of the client isn't required.</span></span> <span data-ttu-id="78d9b-692">處置會取消傳出的要求，並保證指定的 `HttpClient` 執行個體在呼叫 <xref:System.IDisposable.Dispose*> 之後無法使用。</span><span class="sxs-lookup"><span data-stu-id="78d9b-692">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="78d9b-693">`IHttpClientFactory` 會追蹤並處置 `HttpClient` 執行個體使用的資源。</span><span class="sxs-lookup"><span data-stu-id="78d9b-693">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="78d9b-694">`HttpClient` 執行個體通常可視為 .NET 物件，不需要處置。</span><span class="sxs-lookup"><span data-stu-id="78d9b-694">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="78d9b-695">在開始使用 `IHttpClientFactory` 之前，讓單一 `HttpClient` 執行個體維持一段較長的時間，是很常使用的模式。</span><span class="sxs-lookup"><span data-stu-id="78d9b-695">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="78d9b-696">在移轉到 `IHttpClientFactory` 之後，就不再需要此模式。</span><span class="sxs-lookup"><span data-stu-id="78d9b-696">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="78d9b-697">IHttpClientFactory 的替代方案</span><span class="sxs-lookup"><span data-stu-id="78d9b-697">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="78d9b-698">`IHttpClientFactory`在啟用 DI 的應用程式中使用，可避免：</span><span class="sxs-lookup"><span data-stu-id="78d9b-698">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="78d9b-699">共用實例的資源耗盡問題 `HttpMessageHandler` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-699">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="78d9b-700">定期迴圈實例以過時的 DNS 問題 `HttpMessageHandler` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-700">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="78d9b-701">有其他方法可以使用長時間的實例來解決上述問題 <xref:System.Net.Http.SocketsHttpHandler> 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-701">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="78d9b-702">`SocketsHttpHandler`當應用程式啟動時，建立的實例，並在應用程式的生命週期中使用它。</span><span class="sxs-lookup"><span data-stu-id="78d9b-702">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="78d9b-703"><xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime>根據 DNS 重新整理時間，設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="78d9b-703">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="78d9b-704">`HttpClient`視需要使用建立實例 `new HttpClient(handler, disposeHandler: false)` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-704">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="78d9b-705">上述方法可解決 `IHttpClientFactory` 以類似方式解決的資源管理問題。</span><span class="sxs-lookup"><span data-stu-id="78d9b-705">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="78d9b-706">會 `SocketsHttpHandler` 共用實例間的連接 `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-706">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="78d9b-707">此共用可防止通訊端耗盡。</span><span class="sxs-lookup"><span data-stu-id="78d9b-707">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="78d9b-708">會 `SocketsHttpHandler` 根據來迴圈連接 `PooledConnectionLifetime` ，以避免過時的 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="78d9b-708">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="78d9b-709">Cookie</span><span class="sxs-lookup"><span data-stu-id="78d9b-709">Cookies</span></span>

<span data-ttu-id="78d9b-710">集區 `HttpMessageHandler` 實例會導致 `CookieContainer` 共用物件。</span><span class="sxs-lookup"><span data-stu-id="78d9b-710">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="78d9b-711">意外 `CookieContainer` 的物件共用通常會導致不正確的程式碼。</span><span class="sxs-lookup"><span data-stu-id="78d9b-711">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="78d9b-712">針對需要 cookie 的應用程式，請考慮下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="78d9b-712">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="78d9b-713">停用自動 cookie 處理</span><span class="sxs-lookup"><span data-stu-id="78d9b-713">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="78d9b-714">以免`IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="78d9b-714">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="78d9b-715">呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 以停用自動 cookie 處理：</span><span class="sxs-lookup"><span data-stu-id="78d9b-715">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="78d9b-716">記錄</span><span class="sxs-lookup"><span data-stu-id="78d9b-716">Logging</span></span>

<span data-ttu-id="78d9b-717">透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="78d9b-717">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="78d9b-718">在記錄設定中啟用適當的資訊層級，以查看預設記錄檔訊息。</span><span class="sxs-lookup"><span data-stu-id="78d9b-718">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="78d9b-719">額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。</span><span class="sxs-lookup"><span data-stu-id="78d9b-719">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="78d9b-720">用於每個用戶端的記錄檔分類包含用戶端的名稱。</span><span class="sxs-lookup"><span data-stu-id="78d9b-720">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="78d9b-721">例如，名為*MyNamedClient*的用戶端會記錄分類為的訊息 `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler` 。</span><span class="sxs-lookup"><span data-stu-id="78d9b-721">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="78d9b-722">後面加上 *LogicalHandler* 的訊息發生在要求處理常式管線之外。</span><span class="sxs-lookup"><span data-stu-id="78d9b-722">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="78d9b-723">在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。</span><span class="sxs-lookup"><span data-stu-id="78d9b-723">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="78d9b-724">在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。</span><span class="sxs-lookup"><span data-stu-id="78d9b-724">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="78d9b-725">記錄也會發生在要求處理常式管線之內。</span><span class="sxs-lookup"><span data-stu-id="78d9b-725">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="78d9b-726">在 *MyNamedClient* 範例中，那些訊息是針對記錄檔分類 `System.Net.Http.HttpClient.MyNamedClient.ClientHandler` 而記錄。</span><span class="sxs-lookup"><span data-stu-id="78d9b-726">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="78d9b-727">對於要求，這是發生在所有其他處理常式都已執行之後，並且緊接在網路上傳送要求之前。</span><span class="sxs-lookup"><span data-stu-id="78d9b-727">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="78d9b-728">在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。</span><span class="sxs-lookup"><span data-stu-id="78d9b-728">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="78d9b-729">在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。</span><span class="sxs-lookup"><span data-stu-id="78d9b-729">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="78d9b-730">例如，這可能包括要求標頭的變更，或是回應狀態碼的變更。</span><span class="sxs-lookup"><span data-stu-id="78d9b-730">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="78d9b-731">在記錄分類中包含用戶端的名稱，可讓您在需要時進行特定具名用戶端的記錄檔篩選。</span><span class="sxs-lookup"><span data-stu-id="78d9b-731">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="78d9b-732">設定 HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="78d9b-732">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="78d9b-733">可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。</span><span class="sxs-lookup"><span data-stu-id="78d9b-733">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="78d9b-734">新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。</span><span class="sxs-lookup"><span data-stu-id="78d9b-734">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="78d9b-735"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 擴充方法可以用來定義委派。</span><span class="sxs-lookup"><span data-stu-id="78d9b-735">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="78d9b-736">委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：</span><span class="sxs-lookup"><span data-stu-id="78d9b-736">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="78d9b-737">在主控台應用程式中使用 IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="78d9b-737">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="78d9b-738">在主控台應用程式中，將下列套件參考新增至專案：</span><span class="sxs-lookup"><span data-stu-id="78d9b-738">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="78d9b-739">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="78d9b-739">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="78d9b-740">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="78d9b-740">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="78d9b-741">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="78d9b-741">In the following example:</span></span>

* <span data-ttu-id="78d9b-742"><xref:System.Net.Http.IHttpClientFactory> 已在[泛型主機的](xref:fundamentals/host/generic-host)服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="78d9b-742"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="78d9b-743">`MyService` 會從服務建立用戶端 Factory 執行個體，其可用來建立 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="78d9b-743">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="78d9b-744">`HttpClient` 會用來擷取網頁。</span><span class="sxs-lookup"><span data-stu-id="78d9b-744">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="78d9b-745">`Main` 會建立範圍來執行服務的 `GetPage` 方法，並將網頁內容的前 500 個字元寫入至主控台。</span><span class="sxs-lookup"><span data-stu-id="78d9b-745">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="header-propagation-middleware"></a><span data-ttu-id="78d9b-746">標頭傳播中介軟體</span><span class="sxs-lookup"><span data-stu-id="78d9b-746">Header propagation middleware</span></span>

<span data-ttu-id="78d9b-747">標頭傳播是一個支援的中介軟體，可將 HTTP 標頭從連入要求傳播到傳出的 HTTP 用戶端要求。</span><span class="sxs-lookup"><span data-stu-id="78d9b-747">Header propagation is a community supported middleware to propagate HTTP headers from the incoming request to the outgoing HTTP Client requests.</span></span> <span data-ttu-id="78d9b-748">若要使用標頭傳播：</span><span class="sxs-lookup"><span data-stu-id="78d9b-748">To use header propagation:</span></span>

* <span data-ttu-id="78d9b-749">參考[HeaderPropagation](https://www.nuget.org/packages/HeaderPropagation)套件的社區支援埠。</span><span class="sxs-lookup"><span data-stu-id="78d9b-749">Reference the community supported port of the package [HeaderPropagation](https://www.nuget.org/packages/HeaderPropagation).</span></span> <span data-ttu-id="78d9b-750">ASP.NET Core 3.1 和更新版本支援[AspNetCore. HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation)。</span><span class="sxs-lookup"><span data-stu-id="78d9b-750">ASP.NET Core 3.1 and later supports [Microsoft.AspNetCore.HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation).</span></span>

* <span data-ttu-id="78d9b-751">設定中介軟體和 `HttpClient` 中的 `Startup` ：</span><span class="sxs-lookup"><span data-stu-id="78d9b-751">Configure the middleware and `HttpClient` in `Startup`:</span></span>

  [!code-csharp[](http-requests/samples/2.x/Startup21.cs?highlight=5-9,25&name=snippet)]

* <span data-ttu-id="78d9b-752">用戶端會在輸出要求中包含已設定的標頭：</span><span class="sxs-lookup"><span data-stu-id="78d9b-752">The client includes the configured headers on outbound requests:</span></span>

  ```csharp
  var client = clientFactory.CreateClient("MyForwardingClient");
  var response = client.GetAsync(...);
  ```

## <a name="additional-resources"></a><span data-ttu-id="78d9b-753">其他資源</span><span class="sxs-lookup"><span data-stu-id="78d9b-753">Additional resources</span></span>

* [<span data-ttu-id="78d9b-754">使用 HttpClientFactory 實作復原 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="78d9b-754">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="78d9b-755">使用 HttpClientFactory 和 Polly 原則以指數輪詢實作 HTTP 呼叫重試</span><span class="sxs-lookup"><span data-stu-id="78d9b-755">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="78d9b-756">實作斷路器模式</span><span class="sxs-lookup"><span data-stu-id="78d9b-756">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
