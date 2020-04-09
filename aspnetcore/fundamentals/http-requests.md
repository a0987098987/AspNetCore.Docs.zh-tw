---
title: 在 ASP.NET Core 中使用 IHttpClientFactory 發出 HTTP 要求
author: stevejgordon
description: 深入了解在 ASP.NET Core 中使用 IHttpClientFactory 介面來管理邏輯 HttpClient 執行個體。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/09/2020
uid: fundamentals/http-requests
ms.openlocfilehash: 912be34ae0ee25837a94aab65443f15b17ab4556
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78661683"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="31dc0-103">在 ASP.NET Core 中使用 IHttpClientFactory 發出 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="31dc0-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="31dc0-104">由[葛籣·康德龍](https://github.com/glennc),[里安·諾瓦克](https://github.com/rynowak),[史蒂夫·戈登](https://github.com/stevejgordon),[裡克·安德森](https://twitter.com/RickAndMSFT)和[柯克·拉金](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="31dc0-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="31dc0-105"><xref:System.Net.Http.IHttpClientFactory> 可以註冊及用來在應用程式中設定和建立 <xref:System.Net.Http.HttpClient> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="31dc0-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="31dc0-106">`IHttpClientFactory`提供以下好處:</span><span class="sxs-lookup"><span data-stu-id="31dc0-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="31dc0-107">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="31dc0-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="31dc0-108">例如,可以註冊並配置為訪問*GitHub*的[GitHub](https://github.com/)用戶端。</span><span class="sxs-lookup"><span data-stu-id="31dc0-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="31dc0-109">可以註冊默認用戶端以進行常規訪問。</span><span class="sxs-lookup"><span data-stu-id="31dc0-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="31dc0-110">透過委派處理程式在 中委派傳出中間件的`HttpClient`概念 。</span><span class="sxs-lookup"><span data-stu-id="31dc0-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="31dc0-111">為基於 Polly 的中間件提供擴展,以利用`HttpClient`在中 委派處理程式。</span><span class="sxs-lookup"><span data-stu-id="31dc0-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="31dc0-112">管理基礎`HttpClientMessageHandler`實例的池和存留期。</span><span class="sxs-lookup"><span data-stu-id="31dc0-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="31dc0-113">自動管理避免了手動管理`HttpClient`存留期時發生的常見 DNS(功能變數名稱系統)問題。</span><span class="sxs-lookup"><span data-stu-id="31dc0-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="31dc0-114">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="31dc0-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="31dc0-115">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="31dc0-115">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="31dc0-116">本主題版本中的範例代碼用於<xref:System.Text.Json>取消 HTTP 回應中傳回的 JSON 內容的序列化。</span><span class="sxs-lookup"><span data-stu-id="31dc0-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="31dc0-117">對於使用`Json.NET`和`ReadAsAsync<T>`的範例,請使用版本選擇器選擇本主題的 2.x 版本。</span><span class="sxs-lookup"><span data-stu-id="31dc0-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="31dc0-118">耗用量模式</span><span class="sxs-lookup"><span data-stu-id="31dc0-118">Consumption patterns</span></span>

<span data-ttu-id="31dc0-119">有數種方式可將 `IHttpClientFactory` 用於應用程式：</span><span class="sxs-lookup"><span data-stu-id="31dc0-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="31dc0-120">基本用法</span><span class="sxs-lookup"><span data-stu-id="31dc0-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="31dc0-121">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="31dc0-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="31dc0-122">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="31dc0-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="31dc0-123">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="31dc0-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="31dc0-124">最佳方法取決於應用的要求。</span><span class="sxs-lookup"><span data-stu-id="31dc0-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="31dc0-125">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="31dc0-125">Basic usage</span></span>

<span data-ttu-id="31dc0-126">`IHttpClientFactory`可以通過呼`AddHttpClient`叫 : 註冊:</span><span class="sxs-lookup"><span data-stu-id="31dc0-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="31dc0-127">`IHttpClientFactory`可以使用[相依項性 (DI)](xref:fundamentals/dependency-injection)要求 。</span><span class="sxs-lookup"><span data-stu-id="31dc0-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="31dc0-128">以下代碼用於`IHttpClientFactory``HttpClient`建立 實體:</span><span class="sxs-lookup"><span data-stu-id="31dc0-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="31dc0-129">使用`IHttpClientFactory`上述示例中類似是重構現有應用的好方法。</span><span class="sxs-lookup"><span data-stu-id="31dc0-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="31dc0-130">它對如何使用`HttpClient`沒有影響。</span><span class="sxs-lookup"><span data-stu-id="31dc0-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="31dc0-131">在現有應用中創建`HttpClient`實例的地方,將這些事件替換為<xref:System.Net.Http.IHttpClientFactory.CreateClient*>對的調用。</span><span class="sxs-lookup"><span data-stu-id="31dc0-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="31dc0-132">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="31dc0-132">Named clients</span></span>

<span data-ttu-id="31dc0-133">命名用戶端是一個不錯的選擇,當:</span><span class="sxs-lookup"><span data-stu-id="31dc0-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="31dc0-134">該應用程式需要許多不同的用途`HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="31dc0-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="31dc0-135">許多`HttpClient`s 具有不同的配置。</span><span class="sxs-lookup"><span data-stu-id="31dc0-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="31dc0-136">在 註冊`HttpClient`期間 ,`Startup.ConfigureServices`可以在 中 指定名稱的配置。</span><span class="sxs-lookup"><span data-stu-id="31dc0-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="31dc0-137">在前面的代碼中,用戶端配置了:</span><span class="sxs-lookup"><span data-stu-id="31dc0-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="31dc0-138">基本位址`https://api.github.com/`。</span><span class="sxs-lookup"><span data-stu-id="31dc0-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="31dc0-139">使用 GitHub API 需要兩個標頭。</span><span class="sxs-lookup"><span data-stu-id="31dc0-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="31dc0-140">建立用戶端</span><span class="sxs-lookup"><span data-stu-id="31dc0-140">CreateClient</span></span>

<span data-ttu-id="31dc0-141">每次<xref:System.Net.Http.IHttpClientFactory.CreateClient*>都呼叫:</span><span class="sxs-lookup"><span data-stu-id="31dc0-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="31dc0-142">`HttpClient`建立</span><span class="sxs-lookup"><span data-stu-id="31dc0-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="31dc0-143">調用配置操作。</span><span class="sxs-lookup"><span data-stu-id="31dc0-143">The configuration action is called.</span></span>

<span data-ttu-id="31dc0-144">要建立命名客戶端,請將其名稱傳遞`CreateClient`給 :</span><span class="sxs-lookup"><span data-stu-id="31dc0-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="31dc0-145">在上述程式碼中，要求不需要指定主機名稱。</span><span class="sxs-lookup"><span data-stu-id="31dc0-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="31dc0-146">代碼可以僅傳遞路徑,因為為用戶端配置的基本位址是使用的。</span><span class="sxs-lookup"><span data-stu-id="31dc0-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="31dc0-147">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="31dc0-147">Typed clients</span></span>

<span data-ttu-id="31dc0-148">具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="31dc0-148">Typed clients:</span></span>

* <span data-ttu-id="31dc0-149">提供與具名用戶端相同的功能，而不需使用字串作為索引鍵。</span><span class="sxs-lookup"><span data-stu-id="31dc0-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="31dc0-150">取用用戶端時提供 IntelliSense 和編譯器說明。</span><span class="sxs-lookup"><span data-stu-id="31dc0-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="31dc0-151">提供單一位置來設定特定的 `HttpClient` 並與其互動。</span><span class="sxs-lookup"><span data-stu-id="31dc0-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="31dc0-152">例如,可以使用單個類型用戶端:</span><span class="sxs-lookup"><span data-stu-id="31dc0-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="31dc0-153">對於單個後端終結點。</span><span class="sxs-lookup"><span data-stu-id="31dc0-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="31dc0-154">封裝處理終結點的所有邏輯。</span><span class="sxs-lookup"><span data-stu-id="31dc0-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="31dc0-155">使用 DI,可在應用中需要時注入。</span><span class="sxs-lookup"><span data-stu-id="31dc0-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="31dc0-156">型態化用戶端接受`HttpClient`其 建構函數中的參數:</span><span class="sxs-lookup"><span data-stu-id="31dc0-156">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="31dc0-157">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="31dc0-157">In the preceding code:</span></span>

* <span data-ttu-id="31dc0-158">配置將移動到鍵入的用戶端中。</span><span class="sxs-lookup"><span data-stu-id="31dc0-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="31dc0-159">`HttpClient` 物件會公開為公用屬性。</span><span class="sxs-lookup"><span data-stu-id="31dc0-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="31dc0-160">可以創建公開`HttpClient`功能的特定於 API 的方法。</span><span class="sxs-lookup"><span data-stu-id="31dc0-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="31dc0-161">例如,`GetAspNetDocsIssues`該方法封裝代碼以檢索打開的問題。</span><span class="sxs-lookup"><span data-stu-id="31dc0-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="31dc0-162">以下代碼呼叫<xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*>`Startup.ConfigureServices`以 註冊鍵入的用戶端類別:</span><span class="sxs-lookup"><span data-stu-id="31dc0-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="31dc0-163">具型別用戶端會向 DI 註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="31dc0-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="31dc0-164">在前面的代碼中`AddHttpClient``GitHubService`,註冊為瞬態服務。</span><span class="sxs-lookup"><span data-stu-id="31dc0-164">In the preceding code, `AddHttpClient` registers `GitHubService` as a transient service.</span></span> <span data-ttu-id="31dc0-165">此註冊使用工廠方法:</span><span class="sxs-lookup"><span data-stu-id="31dc0-165">This registration uses a factory method to:</span></span>

1. <span data-ttu-id="31dc0-166">建立 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="31dc0-166">Create an instance of `HttpClient`.</span></span>
1. <span data-ttu-id="31dc0-167">創建`GitHubService`的實體 ,傳入`HttpClient`的實體到其建構函數。</span><span class="sxs-lookup"><span data-stu-id="31dc0-167">Create an instance of `GitHubService`, passing in the instance of `HttpClient` to its constructor.</span></span>

<span data-ttu-id="31dc0-168">具型別用戶端可以直接插入並使用：</span><span class="sxs-lookup"><span data-stu-id="31dc0-168">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="31dc0-169">類型客戶端的設定可以在 註冊期間`Startup.ConfigureServices`在 中指定,而不是在型態化用戶端的建構函數中指定:</span><span class="sxs-lookup"><span data-stu-id="31dc0-169">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="31dc0-170">`HttpClient`可以封裝在類型化的用戶端中。</span><span class="sxs-lookup"><span data-stu-id="31dc0-170">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="31dc0-171">定義在內部調用`HttpClient`實體的方法,而不是將其公開為屬性:</span><span class="sxs-lookup"><span data-stu-id="31dc0-171">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="31dc0-172">在前面的代碼中,`HttpClient`儲存在私有欄位中。</span><span class="sxs-lookup"><span data-stu-id="31dc0-172">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="31dc0-173">透過公共`GetRepos`方法`HttpClient`存取 。</span><span class="sxs-lookup"><span data-stu-id="31dc0-173">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="31dc0-174">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="31dc0-174">Generated clients</span></span>

<span data-ttu-id="31dc0-175">`IHttpClientFactory`可與第三方庫(如[重新安裝](https://github.com/paulcbetts/refit))結合使用。</span><span class="sxs-lookup"><span data-stu-id="31dc0-175">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="31dc0-176">Refit 是適用於 .NET 的 REST 程式庫。</span><span class="sxs-lookup"><span data-stu-id="31dc0-176">Refit is a REST library for .NET.</span></span> <span data-ttu-id="31dc0-177">它將 REST API 轉換為即時介面。</span><span class="sxs-lookup"><span data-stu-id="31dc0-177">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="31dc0-178">介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。</span><span class="sxs-lookup"><span data-stu-id="31dc0-178">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="31dc0-179">定義介面及回覆來代表外部 API 和其回應：</span><span class="sxs-lookup"><span data-stu-id="31dc0-179">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="31dc0-180">可以新增具型別用戶端，使用 Refit 產生實作：</span><span class="sxs-lookup"><span data-stu-id="31dc0-180">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="31dc0-181">定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：</span><span class="sxs-lookup"><span data-stu-id="31dc0-181">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="31dc0-182">外寄要求中介軟體</span><span class="sxs-lookup"><span data-stu-id="31dc0-182">Outgoing request middleware</span></span>

<span data-ttu-id="31dc0-183">`HttpClient`具有委派可以連結用於傳出 HTTP 請求的處理程式的概念。</span><span class="sxs-lookup"><span data-stu-id="31dc0-183">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="31dc0-184">`IHttpClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="31dc0-184">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="31dc0-185">簡化定義要應用於每個命名客戶端的處理程式。</span><span class="sxs-lookup"><span data-stu-id="31dc0-185">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="31dc0-186">支援多個處理程式的註冊和連結,以生成傳出請求中間件管道。</span><span class="sxs-lookup"><span data-stu-id="31dc0-186">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="31dc0-187">這些處理常式每個都可以在外寄要求之前和之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="31dc0-187">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="31dc0-188">此模式:</span><span class="sxs-lookup"><span data-stu-id="31dc0-188">This pattern:</span></span>

  * <span data-ttu-id="31dc0-189">類似於ASP.NET核心中的入站中間件管道。</span><span class="sxs-lookup"><span data-stu-id="31dc0-189">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="31dc0-190">提供一種機制來管理有關 HTTP 請求的跨領域問題,例如:</span><span class="sxs-lookup"><span data-stu-id="31dc0-190">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="31dc0-191">快取</span><span class="sxs-lookup"><span data-stu-id="31dc0-191">caching</span></span>
    * <span data-ttu-id="31dc0-192">錯誤處理</span><span class="sxs-lookup"><span data-stu-id="31dc0-192">error handling</span></span>
    * <span data-ttu-id="31dc0-193">序列化</span><span class="sxs-lookup"><span data-stu-id="31dc0-193">serialization</span></span>
    * <span data-ttu-id="31dc0-194">logging</span><span class="sxs-lookup"><span data-stu-id="31dc0-194">logging</span></span>

<span data-ttu-id="31dc0-195">要建立委派處理程式::</span><span class="sxs-lookup"><span data-stu-id="31dc0-195">To create a delegating handler:</span></span>

* <span data-ttu-id="31dc0-196">派生自<xref:System.Net.Http.DelegatingHandler>。</span><span class="sxs-lookup"><span data-stu-id="31dc0-196">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="31dc0-197">覆寫 <xref:System.Net.Http.DelegatingHandler.SendAsync*>。</span><span class="sxs-lookup"><span data-stu-id="31dc0-197">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="31dc0-198">在將要求傳遞給導管中的下一個處理程式之前執行代碼:</span><span class="sxs-lookup"><span data-stu-id="31dc0-198">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="31dc0-199">前面的代碼檢查`X-API-KEY`標頭是否在請求中。</span><span class="sxs-lookup"><span data-stu-id="31dc0-199">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="31dc0-200">如果`X-API-KEY`遺失<xref:System.Net.HttpStatusCode.BadRequest>, 則傳回。</span><span class="sxs-lookup"><span data-stu-id="31dc0-200">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="31dc0-201">可以將多個處理程式添加到 具有的`HttpClient`配置中<xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>。</span><span class="sxs-lookup"><span data-stu-id="31dc0-201">More than one handler can be added to the configuration for an `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="31dc0-202">在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。</span><span class="sxs-lookup"><span data-stu-id="31dc0-202">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="31dc0-203">`IHttpClientFactory` 會為每個處理常式建立個別的 DI 範圍。</span><span class="sxs-lookup"><span data-stu-id="31dc0-203">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="31dc0-204">處理程式可以依賴於任何作用域的服務。</span><span class="sxs-lookup"><span data-stu-id="31dc0-204">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="31dc0-205">處置處理常式時，會處置處理常式所相依的服務。</span><span class="sxs-lookup"><span data-stu-id="31dc0-205">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="31dc0-206">註冊之後，便可以呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>，並傳入處理常式的類型。</span><span class="sxs-lookup"><span data-stu-id="31dc0-206">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="31dc0-207">可以遵循應該執行的順序來註冊多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="31dc0-207">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="31dc0-208">每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：</span><span class="sxs-lookup"><span data-stu-id="31dc0-208">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="31dc0-209">使用下列其中一種方式來與訊息處理常式共用個別要求狀態：</span><span class="sxs-lookup"><span data-stu-id="31dc0-209">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="31dc0-210">使用[HTTPRequestMessage.屬性](xref:System.Net.Http.HttpRequestMessage.Properties)將數據傳遞到處理程式。</span><span class="sxs-lookup"><span data-stu-id="31dc0-210">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="31dc0-211">使用 <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> 來存取目前的要求。</span><span class="sxs-lookup"><span data-stu-id="31dc0-211">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="31dc0-212">建立自訂 <xref:System.Threading.AsyncLocal`1> 儲存體物件以傳遞資料。</span><span class="sxs-lookup"><span data-stu-id="31dc0-212">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="31dc0-213">使用 Polly 為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="31dc0-213">Use Polly-based handlers</span></span>

<span data-ttu-id="31dc0-214">`IHttpClientFactory`與第三方圖書館[波莉](https://github.com/App-vNext/Polly)集成。</span><span class="sxs-lookup"><span data-stu-id="31dc0-214">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="31dc0-215">Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。</span><span class="sxs-lookup"><span data-stu-id="31dc0-215">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="31dc0-216">它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。</span><span class="sxs-lookup"><span data-stu-id="31dc0-216">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="31dc0-217">提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="31dc0-217">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="31dc0-218">Polly擴展支援將基於波莉的處理程式添加到用戶端。</span><span class="sxs-lookup"><span data-stu-id="31dc0-218">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="31dc0-219">波利需要[微軟.擴展.HTTP.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet包。</span><span class="sxs-lookup"><span data-stu-id="31dc0-219">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="31dc0-220">處理暫時性錯誤</span><span class="sxs-lookup"><span data-stu-id="31dc0-220">Handle transient faults</span></span>

<span data-ttu-id="31dc0-221">當外部 HTTP 調用是瞬態的時,通常會發生故障。</span><span class="sxs-lookup"><span data-stu-id="31dc0-221">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="31dc0-222"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*>允許定義策略以處理瞬態錯誤。</span><span class="sxs-lookup"><span data-stu-id="31dc0-222"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="31dc0-223">設定以下`AddTransientHttpErrorPolicy`回應的原則:</span><span class="sxs-lookup"><span data-stu-id="31dc0-223">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="31dc0-224">HTTP 5xx</span><span class="sxs-lookup"><span data-stu-id="31dc0-224">HTTP 5xx</span></span>
* <span data-ttu-id="31dc0-225">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="31dc0-225">HTTP 408</span></span>

<span data-ttu-id="31dc0-226">`AddTransientHttpErrorPolicy`提供對`PolicyBuilder`設定為處理表示可能暫態故障的錯誤的物件的存取:</span><span class="sxs-lookup"><span data-stu-id="31dc0-226">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="31dc0-227">在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。</span><span class="sxs-lookup"><span data-stu-id="31dc0-227">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="31dc0-228">失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。</span><span class="sxs-lookup"><span data-stu-id="31dc0-228">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="31dc0-229">動態選取原則</span><span class="sxs-lookup"><span data-stu-id="31dc0-229">Dynamically select policies</span></span>

<span data-ttu-id="31dc0-230">提供延伸方法以新增基於 Polly 的處理程式,<xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>例如 。</span><span class="sxs-lookup"><span data-stu-id="31dc0-230">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="31dc0-231">以下`AddPolicyHandler`重新載入檢查要求以決定套用哪個政策:</span><span class="sxs-lookup"><span data-stu-id="31dc0-231">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="31dc0-232">在上述程式碼中，如果外寄要求是 HTTP GET，就會套用 10 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="31dc0-232">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="31dc0-233">任何其他 HTTP 方法會使用 30 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="31dc0-233">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="31dc0-234">新增多個 Polly 處理常式</span><span class="sxs-lookup"><span data-stu-id="31dc0-234">Add multiple Polly handlers</span></span>

<span data-ttu-id="31dc0-235">巢狀波利策略很常見:</span><span class="sxs-lookup"><span data-stu-id="31dc0-235">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="31dc0-236">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="31dc0-236">In the preceding example:</span></span>

* <span data-ttu-id="31dc0-237">將添加兩個處理程式。</span><span class="sxs-lookup"><span data-stu-id="31dc0-237">Two handlers are added.</span></span>
* <span data-ttu-id="31dc0-238">第一個處理程式<xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*>用於添加重試策略。</span><span class="sxs-lookup"><span data-stu-id="31dc0-238">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="31dc0-239">失敗的要求會重試最多三次。</span><span class="sxs-lookup"><span data-stu-id="31dc0-239">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="31dc0-240">第二`AddTransientHttpErrorPolicy`個調用添加了斷路器策略。</span><span class="sxs-lookup"><span data-stu-id="31dc0-240">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="31dc0-241">如果連續發生 5 次失敗嘗試,則其他外部請求將被阻止 30 秒。</span><span class="sxs-lookup"><span data-stu-id="31dc0-241">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="31dc0-242">斷路器原則可設定狀態。</span><span class="sxs-lookup"><span data-stu-id="31dc0-242">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="31dc0-243">透過此用戶端的所有呼叫都會共用相同的線路狀態。</span><span class="sxs-lookup"><span data-stu-id="31dc0-243">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="31dc0-244">從 Polly 登錄新增原則</span><span class="sxs-lookup"><span data-stu-id="31dc0-244">Add policies from the Polly registry</span></span>

<span data-ttu-id="31dc0-245">管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。</span><span class="sxs-lookup"><span data-stu-id="31dc0-245">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="31dc0-246">在下列程式碼中：</span><span class="sxs-lookup"><span data-stu-id="31dc0-246">In the following code:</span></span>

* <span data-ttu-id="31dc0-247">添加了"常規"和"長"警。</span><span class="sxs-lookup"><span data-stu-id="31dc0-247">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="31dc0-248"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>添加註冊表中的"常規"和"長"策略。</span><span class="sxs-lookup"><span data-stu-id="31dc0-248"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="31dc0-249">有關`IHttpClientFactory`和波莉集成的詳細資訊,請參閱[波莉 wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory)。</span><span class="sxs-lookup"><span data-stu-id="31dc0-249">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="31dc0-250">HttpClient 和存留期管理</span><span class="sxs-lookup"><span data-stu-id="31dc0-250">HttpClient and lifetime management</span></span>

<span data-ttu-id="31dc0-251">每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回新的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="31dc0-251">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="31dc0-252">每個<xref:System.Net.Http.HttpMessageHandler>命名客戶端建立 。</span><span class="sxs-lookup"><span data-stu-id="31dc0-252">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="31dc0-253">處理站會管理 `HttpMessageHandler` 執行個體的存留期。</span><span class="sxs-lookup"><span data-stu-id="31dc0-253">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="31dc0-254">`IHttpClientFactory` 會將處理站所建立的 `HttpMessageHandler` 執行個體放入集區以減少資源耗用量。</span><span class="sxs-lookup"><span data-stu-id="31dc0-254">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="31dc0-255">建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。</span><span class="sxs-lookup"><span data-stu-id="31dc0-255">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="31dc0-256">將處理常式放入集區非常實用，因為處理常式通常會管理自己專屬的底層 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="31dc0-256">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="31dc0-257">建立比所需數目更多的處理常式，可能會導致連線延遲。</span><span class="sxs-lookup"><span data-stu-id="31dc0-257">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="31dc0-258">某些處理程式還會無限期地保持連接打開狀態,這可以防止處理程式對 DNS(功能變數名稱系統)更改做出反應。</span><span class="sxs-lookup"><span data-stu-id="31dc0-258">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="31dc0-259">預設處理常式存留時間為兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="31dc0-259">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="31dc0-260">可以依每個命名客戶端覆蓋預設值:</span><span class="sxs-lookup"><span data-stu-id="31dc0-260">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="31dc0-261">`HttpClient`實例通常可以被視為**不需要**處理的 .NET 物件。</span><span class="sxs-lookup"><span data-stu-id="31dc0-261">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="31dc0-262">處置會取消傳出的要求，並保證指定的 `HttpClient` 執行個體在呼叫 <xref:System.IDisposable.Dispose*> 之後無法使用。</span><span class="sxs-lookup"><span data-stu-id="31dc0-262">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="31dc0-263">`IHttpClientFactory` 會追蹤並處置 `HttpClient` 執行個體使用的資源。</span><span class="sxs-lookup"><span data-stu-id="31dc0-263">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="31dc0-264">在開始使用 `IHttpClientFactory` 之前，讓單一 `HttpClient` 執行個體維持一段較長的時間，是很常使用的模式。</span><span class="sxs-lookup"><span data-stu-id="31dc0-264">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="31dc0-265">在移轉到 `IHttpClientFactory` 之後，就不再需要此模式。</span><span class="sxs-lookup"><span data-stu-id="31dc0-265">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="31dc0-266">IHTTPClientFactory 的替代方案</span><span class="sxs-lookup"><span data-stu-id="31dc0-266">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="31dc0-267">`IHttpClientFactory`在開啟 DI 的應用程式使用可避免:</span><span class="sxs-lookup"><span data-stu-id="31dc0-267">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="31dc0-268">通過池化實例解決`HttpMessageHandler`資源耗盡問題。</span><span class="sxs-lookup"><span data-stu-id="31dc0-268">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="31dc0-269">通過定期迴圈`HttpMessageHandler`實例來處理陳舊的 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="31dc0-269">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="31dc0-270">使用長壽命<xref:System.Net.Http.SocketsHttpHandler>實例解決上述問題的替代方法有其他方法。</span><span class="sxs-lookup"><span data-stu-id="31dc0-270">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="31dc0-271">創建應用何時啟動`SocketsHttpHandler`的實例,並將其用於應用的生命週期。</span><span class="sxs-lookup"><span data-stu-id="31dc0-271">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="31dc0-272">根據<xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime>DNS 刷新時間配置到適當的值。</span><span class="sxs-lookup"><span data-stu-id="31dc0-272">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="31dc0-273">根據需要`HttpClient`使用`new HttpClient(handler, disposeHandler: false)`實例創建實例。</span><span class="sxs-lookup"><span data-stu-id="31dc0-273">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="31dc0-274">前面的方法解決了以類似方式`IHttpClientFactory`解決的資源管理問題。</span><span class="sxs-lookup"><span data-stu-id="31dc0-274">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="31dc0-275">跨`SocketsHttpHandler``HttpClient`實例共享連接。</span><span class="sxs-lookup"><span data-stu-id="31dc0-275">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="31dc0-276">此共用可防止套接字耗盡。</span><span class="sxs-lookup"><span data-stu-id="31dc0-276">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="31dc0-277">根據`SocketsHttpHandler`迴圈`PooledConnectionLifetime`連接,以避免陳舊的 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="31dc0-277">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="31dc0-278">Cookie</span><span class="sxs-lookup"><span data-stu-id="31dc0-278">Cookies</span></span>

<span data-ttu-id="31dc0-279">`HttpMessageHandler`池實例會導致`CookieContainer`對象被共用。</span><span class="sxs-lookup"><span data-stu-id="31dc0-279">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="31dc0-280">意外`CookieContainer`的物件共用通常會導致代碼不正確。</span><span class="sxs-lookup"><span data-stu-id="31dc0-280">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="31dc0-281">對於需要 Cookie 的應用,請考慮以下任一:</span><span class="sxs-lookup"><span data-stu-id="31dc0-281">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="31dc0-282">關閉自動 Cookie 處理</span><span class="sxs-lookup"><span data-stu-id="31dc0-282">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="31dc0-283">避免`IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="31dc0-283">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="31dc0-284">呼叫<xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>以關閉自動 Cookie 處理:</span><span class="sxs-lookup"><span data-stu-id="31dc0-284">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="31dc0-285">記錄</span><span class="sxs-lookup"><span data-stu-id="31dc0-285">Logging</span></span>

<span data-ttu-id="31dc0-286">透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="31dc0-286">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="31dc0-287">啟用紀錄記錄配置中的相應資訊等級以查看預設紀錄訊息。</span><span class="sxs-lookup"><span data-stu-id="31dc0-287">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="31dc0-288">額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。</span><span class="sxs-lookup"><span data-stu-id="31dc0-288">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="31dc0-289">用於每個用戶端的記錄檔分類包含用戶端的名稱。</span><span class="sxs-lookup"><span data-stu-id="31dc0-289">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="31dc0-290">例如,名為*MyNamedClient*的用戶端使用"System.Net.http.httpClient"類別記錄郵件。**我的命名客戶**。邏輯處理"。</span><span class="sxs-lookup"><span data-stu-id="31dc0-290">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="31dc0-291">後面加上 *LogicalHandler* 的訊息發生在要求處理常式管線之外。</span><span class="sxs-lookup"><span data-stu-id="31dc0-291">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="31dc0-292">在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。</span><span class="sxs-lookup"><span data-stu-id="31dc0-292">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="31dc0-293">在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。</span><span class="sxs-lookup"><span data-stu-id="31dc0-293">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="31dc0-294">記錄也會發生在要求處理常式管線之內。</span><span class="sxs-lookup"><span data-stu-id="31dc0-294">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="31dc0-295">在*My命名用戶端*範例中,這些消息使用日誌類別"System.Net.http.httpClient」進行記錄。**我的命名客戶**。客戶處理程式"。</span><span class="sxs-lookup"><span data-stu-id="31dc0-295">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="31dc0-296">對於請求,這將在所有其他處理程式運行後以及發送請求之前立即發生。</span><span class="sxs-lookup"><span data-stu-id="31dc0-296">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="31dc0-297">在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。</span><span class="sxs-lookup"><span data-stu-id="31dc0-297">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="31dc0-298">在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。</span><span class="sxs-lookup"><span data-stu-id="31dc0-298">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="31dc0-299">這可能包括對請求標頭或回應狀態代碼的更改。</span><span class="sxs-lookup"><span data-stu-id="31dc0-299">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="31dc0-300">在日誌類別中包括用戶端的名稱可對特定命名客戶端進行日誌篩選。</span><span class="sxs-lookup"><span data-stu-id="31dc0-300">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="31dc0-301">設定 HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="31dc0-301">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="31dc0-302">可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。</span><span class="sxs-lookup"><span data-stu-id="31dc0-302">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="31dc0-303">新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。</span><span class="sxs-lookup"><span data-stu-id="31dc0-303">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="31dc0-304"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 擴充方法可以用來定義委派。</span><span class="sxs-lookup"><span data-stu-id="31dc0-304">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="31dc0-305">委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：</span><span class="sxs-lookup"><span data-stu-id="31dc0-305">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="31dc0-306">在主控台應用程式中使用 IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="31dc0-306">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="31dc0-307">在主控台應用程式中，將下列套件參考新增至專案：</span><span class="sxs-lookup"><span data-stu-id="31dc0-307">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="31dc0-308">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="31dc0-308">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="31dc0-309">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="31dc0-309">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="31dc0-310">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="31dc0-310">In the following example:</span></span>

* <span data-ttu-id="31dc0-311"><xref:System.Net.Http.IHttpClientFactory> 已在[泛型主機的](xref:fundamentals/host/generic-host)服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="31dc0-311"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="31dc0-312">`MyService` 會從服務建立用戶端 Factory 執行個體，其可用來建立 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="31dc0-312">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="31dc0-313">`HttpClient` 會用來擷取網頁。</span><span class="sxs-lookup"><span data-stu-id="31dc0-313">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="31dc0-314">`Main` 會建立範圍來執行服務的 `GetPage` 方法，並將網頁內容的前 500 個字元寫入至主控台。</span><span class="sxs-lookup"><span data-stu-id="31dc0-314">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="header-propagation-middleware"></a><span data-ttu-id="31dc0-315">頭傳播中間件</span><span class="sxs-lookup"><span data-stu-id="31dc0-315">Header propagation middleware</span></span>

<span data-ttu-id="31dc0-316">標頭傳播是一個ASP.NET核心中間件,用於將 HTTP 標頭從傳入請求傳播到傳出的 HTTP 用戶端請求。</span><span class="sxs-lookup"><span data-stu-id="31dc0-316">Header propagation is an ASP.NET Core middleware to propagate HTTP headers from the incoming request to the outgoing HTTP Client requests.</span></span> <span data-ttu-id="31dc0-317">要使用標頭傳播:</span><span class="sxs-lookup"><span data-stu-id="31dc0-317">To use header propagation:</span></span>

* <span data-ttu-id="31dc0-318">引用[Microsoft.AspNetCore.標題傳播](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation)包。</span><span class="sxs-lookup"><span data-stu-id="31dc0-318">Reference the [Microsoft.AspNetCore.HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation) package.</span></span>
* <span data-ttu-id="31dc0-319">設定中間件與`HttpClient` `Startup` :</span><span class="sxs-lookup"><span data-stu-id="31dc0-319">Configure the middleware and `HttpClient` in `Startup`:</span></span>

  [!code-csharp[](http-requests/samples/3.x/Startup.cs?highlight=5-9,21&name=snippet)]

* <span data-ttu-id="31dc0-320">用戶端包括出站要求的設定標頭:</span><span class="sxs-lookup"><span data-stu-id="31dc0-320">The client includes the configured headers on outbound requests:</span></span>

  ```csharp
  var client = clientFactory.CreateClient("MyForwardingClient");
  var response = client.GetAsync(...);
  ```

## <a name="additional-resources"></a><span data-ttu-id="31dc0-321">其他資源</span><span class="sxs-lookup"><span data-stu-id="31dc0-321">Additional resources</span></span>

* [<span data-ttu-id="31dc0-322">使用 HttpClientFactory 實作復原 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="31dc0-322">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="31dc0-323">使用 HttpClientFactory 和 Polly 原則以指數輪詢實作 HTTP 呼叫重試</span><span class="sxs-lookup"><span data-stu-id="31dc0-323">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="31dc0-324">實作斷路器模式</span><span class="sxs-lookup"><span data-stu-id="31dc0-324">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)
* [<span data-ttu-id="31dc0-325">如何在 .NET 中序列化和反序列化 JSON</span><span class="sxs-lookup"><span data-stu-id="31dc0-325">How to serialize and deserialize JSON in .NET</span></span>](/dotnet/standard/serialization/system-text-json-how-to)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="31dc0-326">作者：[Glenn Condron](https://github.com/glennc)、[Ryan Nowak](https://github.com/rynowak) 和 [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="31dc0-326">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="31dc0-327"><xref:System.Net.Http.IHttpClientFactory> 可以註冊及用來在應用程式中設定和建立 <xref:System.Net.Http.HttpClient> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="31dc0-327">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="31dc0-328">它提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="31dc0-328">It offers the following benefits:</span></span>

* <span data-ttu-id="31dc0-329">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="31dc0-329">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="31dc0-330">例如,可以註冊和配置為訪問[GitHub](https://github.com/)的*github*用戶端。</span><span class="sxs-lookup"><span data-stu-id="31dc0-330">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="31dc0-331">預設用戶端可以註冊用於其他用途。</span><span class="sxs-lookup"><span data-stu-id="31dc0-331">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="31dc0-332">透過委派 `HttpClient` 中的處理常式來撰寫外寄中介軟體的概念，並提供延伸模組以便 Polly 架構中介軟體利用外寄中介軟體。</span><span class="sxs-lookup"><span data-stu-id="31dc0-332">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="31dc0-333">管理基礎 `HttpClientMessageHandler` 執行個體的共用和存留期，以避免在手動管理 `HttpClient` 存留期時，發生的常見 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="31dc0-333">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="31dc0-334">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="31dc0-334">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="31dc0-335">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="31dc0-335">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="31dc0-336">耗用量模式</span><span class="sxs-lookup"><span data-stu-id="31dc0-336">Consumption patterns</span></span>

<span data-ttu-id="31dc0-337">有數種方式可將 `IHttpClientFactory` 用於應用程式：</span><span class="sxs-lookup"><span data-stu-id="31dc0-337">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="31dc0-338">基本用法</span><span class="sxs-lookup"><span data-stu-id="31dc0-338">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="31dc0-339">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="31dc0-339">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="31dc0-340">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="31dc0-340">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="31dc0-341">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="31dc0-341">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="31dc0-342">它們全都不會嚴格優先於另一個。</span><span class="sxs-lookup"><span data-stu-id="31dc0-342">None of them are strictly superior to another.</span></span> <span data-ttu-id="31dc0-343">最好的方法取決於應用程式的條件約束。</span><span class="sxs-lookup"><span data-stu-id="31dc0-343">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="31dc0-344">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="31dc0-344">Basic usage</span></span>

<span data-ttu-id="31dc0-345">`IHttpClientFactory` 可以藉由在 `Startup.ConfigureServices` 方法內的 `IServiceCollection` 上呼叫 `AddHttpClient` 擴充方法來註冊。</span><span class="sxs-lookup"><span data-stu-id="31dc0-345">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="31dc0-346">註冊後,代碼可以接受`IHttpClientFactory`任何可以注入[依賴項注入 (DI)](xref:fundamentals/dependency-injection)的服務。</span><span class="sxs-lookup"><span data-stu-id="31dc0-346">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="31dc0-347">`IHttpClientFactory`可建立實體`HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="31dc0-347">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="31dc0-348">以這種方式使用 `IHttpClientFactory` 是重構現有應用程式的好方法。</span><span class="sxs-lookup"><span data-stu-id="31dc0-348">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="31dc0-349">它對 `HttpClient` 的使用方式沒有任何影響。</span><span class="sxs-lookup"><span data-stu-id="31dc0-349">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="31dc0-350">在目前建立 `HttpClient` 執行個體的位置，將那些項目取代為呼叫 <xref:System.Net.Http.IHttpClientFactory.CreateClient*>。</span><span class="sxs-lookup"><span data-stu-id="31dc0-350">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="31dc0-351">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="31dc0-351">Named clients</span></span>

<span data-ttu-id="31dc0-352">如果應用程式需要使用多個不同的 `HttpClient`，且每個都有不同的設定，可以選擇使用**具名用戶端**。</span><span class="sxs-lookup"><span data-stu-id="31dc0-352">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="31dc0-353">具名 `HttpClient` 的組態可以在 `Startup.ConfigureServices` 中註冊時指定。</span><span class="sxs-lookup"><span data-stu-id="31dc0-353">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="31dc0-354">在前面的代碼中,`AddHttpClient`呼叫,提供名稱*github*。</span><span class="sxs-lookup"><span data-stu-id="31dc0-354">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="31dc0-355">此用戶端已套用一些預設組態&mdash;即使用 GitHub API 所需的基底位址和兩個標頭。</span><span class="sxs-lookup"><span data-stu-id="31dc0-355">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="31dc0-356">每次呼叫 `CreateClient` 時，會建立 `HttpClient` 的新執行個體並呼叫組態動作。</span><span class="sxs-lookup"><span data-stu-id="31dc0-356">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="31dc0-357">若要使用具名用戶端，可以傳遞字串參數至 `CreateClient`。</span><span class="sxs-lookup"><span data-stu-id="31dc0-357">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="31dc0-358">指定要建立之用戶端的名稱：</span><span class="sxs-lookup"><span data-stu-id="31dc0-358">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="31dc0-359">在上述程式碼中，要求不需要指定主機名稱。</span><span class="sxs-lookup"><span data-stu-id="31dc0-359">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="31dc0-360">它可以只傳遞路徑，因為已使用為用戶端設定的基底位址。</span><span class="sxs-lookup"><span data-stu-id="31dc0-360">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="31dc0-361">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="31dc0-361">Typed clients</span></span>

<span data-ttu-id="31dc0-362">具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="31dc0-362">Typed clients:</span></span>

* <span data-ttu-id="31dc0-363">提供與具名用戶端相同的功能，而不需使用字串作為索引鍵。</span><span class="sxs-lookup"><span data-stu-id="31dc0-363">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="31dc0-364">取用用戶端時提供 IntelliSense 和編譯器說明。</span><span class="sxs-lookup"><span data-stu-id="31dc0-364">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="31dc0-365">提供單一位置來設定特定的 `HttpClient` 並與其互動。</span><span class="sxs-lookup"><span data-stu-id="31dc0-365">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="31dc0-366">例如，單一的具型別用戶端可能用於單一的後端端點，並封裝處理該端點的所有邏輯。</span><span class="sxs-lookup"><span data-stu-id="31dc0-366">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="31dc0-367">使用 DI 且可在應用程式中需要之處插入。</span><span class="sxs-lookup"><span data-stu-id="31dc0-367">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="31dc0-368">型態化用戶端接受`HttpClient`其 建構函數中的參數:</span><span class="sxs-lookup"><span data-stu-id="31dc0-368">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="31dc0-369">在上述程式碼中，組態會移到具型別用戶端。</span><span class="sxs-lookup"><span data-stu-id="31dc0-369">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="31dc0-370">`HttpClient` 物件會公開為公用屬性。</span><span class="sxs-lookup"><span data-stu-id="31dc0-370">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="31dc0-371">您可定義 API 特定的方法，其公開 `HttpClient` 功能。</span><span class="sxs-lookup"><span data-stu-id="31dc0-371">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="31dc0-372">`GetAspNetDocsIssues` 方法會封裝從 GitHub 存放庫查詢和剖析最新開啟問題所需的程式碼。</span><span class="sxs-lookup"><span data-stu-id="31dc0-372">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="31dc0-373">若要註冊具型別用戶端，泛型 <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> 擴充方法可用於 `Startup.ConfigureServices` 內，並指定具型別用戶端類別：</span><span class="sxs-lookup"><span data-stu-id="31dc0-373">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="31dc0-374">具型別用戶端會向 DI 註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="31dc0-374">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="31dc0-375">具型別用戶端可以直接插入並使用：</span><span class="sxs-lookup"><span data-stu-id="31dc0-375">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="31dc0-376">想要的話，具型別用戶端的組態可以在 `Startup.ConfigureServices` 中註冊時指定，而不是在具型別用戶端的建構函式中：</span><span class="sxs-lookup"><span data-stu-id="31dc0-376">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="31dc0-377">可以將 `HttpClient` 完全封裝在具型別用戶端內。</span><span class="sxs-lookup"><span data-stu-id="31dc0-377">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="31dc0-378">可以提供在內部呼叫 `HttpClient` 執行個體的公用方法，而不將它公開為屬性。</span><span class="sxs-lookup"><span data-stu-id="31dc0-378">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="31dc0-379">在上述程式碼，`HttpClient` 儲存為私用欄位。</span><span class="sxs-lookup"><span data-stu-id="31dc0-379">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="31dc0-380">進行外部呼叫的所有存取都會經歷 `GetRepos` 方法。</span><span class="sxs-lookup"><span data-stu-id="31dc0-380">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="31dc0-381">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="31dc0-381">Generated clients</span></span>

<span data-ttu-id="31dc0-382">`IHttpClientFactory` 可和其他協力廠商程式庫一起使用，例如 [Refit](https://github.com/paulcbetts/refit)。</span><span class="sxs-lookup"><span data-stu-id="31dc0-382">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="31dc0-383">Refit 是適用於 .NET 的 REST 程式庫。</span><span class="sxs-lookup"><span data-stu-id="31dc0-383">Refit is a REST library for .NET.</span></span> <span data-ttu-id="31dc0-384">它將 REST API 轉換為即時介面。</span><span class="sxs-lookup"><span data-stu-id="31dc0-384">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="31dc0-385">介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。</span><span class="sxs-lookup"><span data-stu-id="31dc0-385">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="31dc0-386">定義介面及回覆來代表外部 API 和其回應：</span><span class="sxs-lookup"><span data-stu-id="31dc0-386">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="31dc0-387">可以新增具型別用戶端，使用 Refit 產生實作：</span><span class="sxs-lookup"><span data-stu-id="31dc0-387">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="31dc0-388">定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：</span><span class="sxs-lookup"><span data-stu-id="31dc0-388">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="31dc0-389">外寄要求中介軟體</span><span class="sxs-lookup"><span data-stu-id="31dc0-389">Outgoing request middleware</span></span>

<span data-ttu-id="31dc0-390">`HttpClient` 已經有委派可針對外寄 HTTP 要求連結在一起的處理常式的概念。</span><span class="sxs-lookup"><span data-stu-id="31dc0-390">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="31dc0-391">`IHttpClientFactory` 可讓您輕鬆地定義要套用於每個具名用戶端的處理常式。</span><span class="sxs-lookup"><span data-stu-id="31dc0-391">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="31dc0-392">它支援註冊和鏈結多個處理常式，以建置外寄要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="31dc0-392">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="31dc0-393">這些處理常式每個都可以在外寄要求之前和之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="31dc0-393">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="31dc0-394">此模式與 ASP.NET Core 中的輸入中介軟體管線相似。</span><span class="sxs-lookup"><span data-stu-id="31dc0-394">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="31dc0-395">模式提供一個機制來管理 HTTP 要求的跨領域關注，包括快取、錯誤處理、序列化和記錄。</span><span class="sxs-lookup"><span data-stu-id="31dc0-395">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="31dc0-396">若要建立處理常式，請定義衍生自 <xref:System.Net.Http.DelegatingHandler> 的類別。</span><span class="sxs-lookup"><span data-stu-id="31dc0-396">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="31dc0-397">覆寫 `SendAsync` 方法，以在將要求傳遞至管線中的下一個處理常式之前執行程式碼：</span><span class="sxs-lookup"><span data-stu-id="31dc0-397">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="31dc0-398">上述程式碼定義一個基本處理常式。</span><span class="sxs-lookup"><span data-stu-id="31dc0-398">The preceding code defines a basic handler.</span></span> <span data-ttu-id="31dc0-399">它會檢查以查看要求上是否已包含 `X-API-KEY` 標頭。</span><span class="sxs-lookup"><span data-stu-id="31dc0-399">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="31dc0-400">如果遺漏標頭，它可以避免 HTTP 呼叫，並傳回適當的回應。</span><span class="sxs-lookup"><span data-stu-id="31dc0-400">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="31dc0-401">在註冊期間,可以將一個或多個處理程式加入的設定中`HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="31dc0-401">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="31dc0-402">這項工作是透過 <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder> 上的擴充方法完成。</span><span class="sxs-lookup"><span data-stu-id="31dc0-402">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="31dc0-403">在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。</span><span class="sxs-lookup"><span data-stu-id="31dc0-403">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="31dc0-404">`IHttpClientFactory` 會為每個處理常式建立個別的 DI 範圍。</span><span class="sxs-lookup"><span data-stu-id="31dc0-404">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="31dc0-405">處理常式可相依於任何範圍的服務。</span><span class="sxs-lookup"><span data-stu-id="31dc0-405">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="31dc0-406">處置處理常式時，會處置處理常式所相依的服務。</span><span class="sxs-lookup"><span data-stu-id="31dc0-406">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="31dc0-407">註冊之後，便可以呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>，並傳入處理常式的類型。</span><span class="sxs-lookup"><span data-stu-id="31dc0-407">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="31dc0-408">可以遵循應該執行的順序來註冊多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="31dc0-408">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="31dc0-409">每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：</span><span class="sxs-lookup"><span data-stu-id="31dc0-409">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="31dc0-410">使用下列其中一種方式來與訊息處理常式共用個別要求狀態：</span><span class="sxs-lookup"><span data-stu-id="31dc0-410">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="31dc0-411">使用 `HttpRequestMessage.Properties` 將資料傳遞到處理常式。</span><span class="sxs-lookup"><span data-stu-id="31dc0-411">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="31dc0-412">使用 `IHttpContextAccessor` 來存取目前的要求。</span><span class="sxs-lookup"><span data-stu-id="31dc0-412">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="31dc0-413">建立自訂 `AsyncLocal` 儲存體物件以傳遞資料。</span><span class="sxs-lookup"><span data-stu-id="31dc0-413">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="31dc0-414">使用 Polly 為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="31dc0-414">Use Polly-based handlers</span></span>

<span data-ttu-id="31dc0-415">`IHttpClientFactory` 整合受歡迎的協力廠商程式庫，稱為 [Polly](https://github.com/App-vNext/Polly)。</span><span class="sxs-lookup"><span data-stu-id="31dc0-415">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="31dc0-416">Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。</span><span class="sxs-lookup"><span data-stu-id="31dc0-416">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="31dc0-417">它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。</span><span class="sxs-lookup"><span data-stu-id="31dc0-417">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="31dc0-418">提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="31dc0-418">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="31dc0-419">Polly 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="31dc0-419">The Polly extensions:</span></span>

* <span data-ttu-id="31dc0-420">支援將以 Polly 為基礎的處理常式新增至用戶端。</span><span class="sxs-lookup"><span data-stu-id="31dc0-420">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="31dc0-421">可在安裝 [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet 套件後使用。</span><span class="sxs-lookup"><span data-stu-id="31dc0-421">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="31dc0-422">該套件並未包含在 ASP.NET Core 共用架構中。</span><span class="sxs-lookup"><span data-stu-id="31dc0-422">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="31dc0-423">處理暫時性錯誤</span><span class="sxs-lookup"><span data-stu-id="31dc0-423">Handle transient faults</span></span>

<span data-ttu-id="31dc0-424">大部分的錯誤發生在外部 HTTP 呼叫是暫時性的時候。</span><span class="sxs-lookup"><span data-stu-id="31dc0-424">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="31dc0-425">包含一個便利的擴充方法，稱為 `AddTransientHttpErrorPolicy`，它可允許定義原則來處理暫時性錯誤。</span><span class="sxs-lookup"><span data-stu-id="31dc0-425">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="31dc0-426">使用此延伸模組方法設定的原則，會處理 `HttpRequestException`、HTTP 5xx 回應和 HTTP 408 回應。</span><span class="sxs-lookup"><span data-stu-id="31dc0-426">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="31dc0-427">`AddTransientHttpErrorPolicy` 延伸模組可用於 `Startup.ConfigureServices` 內。</span><span class="sxs-lookup"><span data-stu-id="31dc0-427">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="31dc0-428">延伸模組能提供 `PolicyBuilder` 物件的存取，該物件已設定來處理代表可能暫時性錯誤的錯誤：</span><span class="sxs-lookup"><span data-stu-id="31dc0-428">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="31dc0-429">在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。</span><span class="sxs-lookup"><span data-stu-id="31dc0-429">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="31dc0-430">失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。</span><span class="sxs-lookup"><span data-stu-id="31dc0-430">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="31dc0-431">動態選取原則</span><span class="sxs-lookup"><span data-stu-id="31dc0-431">Dynamically select policies</span></span>

<span data-ttu-id="31dc0-432">有額外的擴充方法可用來新增 Polly 為基礎的處理常式。</span><span class="sxs-lookup"><span data-stu-id="31dc0-432">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="31dc0-433">其中一個這類延伸模組是 `AddPolicyHandler`，它有多個多載。</span><span class="sxs-lookup"><span data-stu-id="31dc0-433">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="31dc0-434">一個多載可讓您在定義要套用的原則時，檢查要求：</span><span class="sxs-lookup"><span data-stu-id="31dc0-434">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="31dc0-435">在上述程式碼中，如果外寄要求是 HTTP GET，就會套用 10 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="31dc0-435">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="31dc0-436">任何其他 HTTP 方法會使用 30 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="31dc0-436">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="31dc0-437">新增多個 Polly 處理常式</span><span class="sxs-lookup"><span data-stu-id="31dc0-437">Add multiple Polly handlers</span></span>

<span data-ttu-id="31dc0-438">通常會建立巢狀 Polly 原則，以提供增強的功能：</span><span class="sxs-lookup"><span data-stu-id="31dc0-438">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="31dc0-439">在上述範例中，會新增兩個處理常式。</span><span class="sxs-lookup"><span data-stu-id="31dc0-439">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="31dc0-440">第一個使用 `AddTransientHttpErrorPolicy` 延伸模組來新增重試原則。</span><span class="sxs-lookup"><span data-stu-id="31dc0-440">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="31dc0-441">失敗的要求會重試最多三次。</span><span class="sxs-lookup"><span data-stu-id="31dc0-441">Failed requests are retried up to three times.</span></span> <span data-ttu-id="31dc0-442">第二個 `AddTransientHttpErrorPolicy` 呼叫會新增斷路器原則。</span><span class="sxs-lookup"><span data-stu-id="31dc0-442">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="31dc0-443">如果循序發生五次失敗的嘗試，進一步的外部要求會遭到封鎖 30 秒。</span><span class="sxs-lookup"><span data-stu-id="31dc0-443">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="31dc0-444">斷路器原則可設定狀態。</span><span class="sxs-lookup"><span data-stu-id="31dc0-444">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="31dc0-445">透過此用戶端的所有呼叫都會共用相同的線路狀態。</span><span class="sxs-lookup"><span data-stu-id="31dc0-445">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="31dc0-446">從 Polly 登錄新增原則</span><span class="sxs-lookup"><span data-stu-id="31dc0-446">Add policies from the Polly registry</span></span>

<span data-ttu-id="31dc0-447">管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。</span><span class="sxs-lookup"><span data-stu-id="31dc0-447">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="31dc0-448">提供了擴充方法，可以使用來自登錄的原則新增處理常式：</span><span class="sxs-lookup"><span data-stu-id="31dc0-448">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="31dc0-449">在上述程式碼中，當 `PolicyRegistry` 新增至 `ServiceCollection` 時，註冊了兩個原則。</span><span class="sxs-lookup"><span data-stu-id="31dc0-449">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="31dc0-450">為了使用來自登錄的原則，使用了 `AddPolicyHandlerFromRegistry` 方法，並傳遞要套用的原則名稱。</span><span class="sxs-lookup"><span data-stu-id="31dc0-450">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="31dc0-451">關於 `IHttpClientFactory` Polly 整合的詳細資訊，可以在 [Polly Wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory) 上找到。</span><span class="sxs-lookup"><span data-stu-id="31dc0-451">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="31dc0-452">HttpClient 和存留期管理</span><span class="sxs-lookup"><span data-stu-id="31dc0-452">HttpClient and lifetime management</span></span>

<span data-ttu-id="31dc0-453">每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回新的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="31dc0-453">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="31dc0-454">每個具名用戶端都有一個 <xref:System.Net.Http.HttpMessageHandler>。</span><span class="sxs-lookup"><span data-stu-id="31dc0-454">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="31dc0-455">處理站會管理 `HttpMessageHandler` 執行個體的存留期。</span><span class="sxs-lookup"><span data-stu-id="31dc0-455">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="31dc0-456">`IHttpClientFactory` 會將處理站所建立的 `HttpMessageHandler` 執行個體放入集區以減少資源耗用量。</span><span class="sxs-lookup"><span data-stu-id="31dc0-456">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="31dc0-457">建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。</span><span class="sxs-lookup"><span data-stu-id="31dc0-457">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="31dc0-458">將處理常式放入集區非常實用，因為處理常式通常會管理自己專屬的底層 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="31dc0-458">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="31dc0-459">建立比所需數目更多的處理常式，可能會導致連線延遲。</span><span class="sxs-lookup"><span data-stu-id="31dc0-459">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="31dc0-460">有些處理常式也會保持連線無限期地開啟，這可能導致處理常式無法對 DNS 變更回應。</span><span class="sxs-lookup"><span data-stu-id="31dc0-460">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="31dc0-461">預設處理常式存留時間為兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="31dc0-461">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="31dc0-462">可以針對每個具名用戶端覆寫預設值。</span><span class="sxs-lookup"><span data-stu-id="31dc0-462">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="31dc0-463">若要覆寫它，請在建立用戶端時所傳回的 `IHttpClientBuilder` 上呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*>：</span><span class="sxs-lookup"><span data-stu-id="31dc0-463">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="31dc0-464">不需要處置用戶端。</span><span class="sxs-lookup"><span data-stu-id="31dc0-464">Disposal of the client isn't required.</span></span> <span data-ttu-id="31dc0-465">處置會取消傳出的要求，並保證指定的 `HttpClient` 執行個體在呼叫 <xref:System.IDisposable.Dispose*> 之後無法使用。</span><span class="sxs-lookup"><span data-stu-id="31dc0-465">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="31dc0-466">`IHttpClientFactory` 會追蹤並處置 `HttpClient` 執行個體使用的資源。</span><span class="sxs-lookup"><span data-stu-id="31dc0-466">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="31dc0-467">`HttpClient` 執行個體通常可視為 .NET 物件，不需要處置。</span><span class="sxs-lookup"><span data-stu-id="31dc0-467">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="31dc0-468">在開始使用 `IHttpClientFactory` 之前，讓單一 `HttpClient` 執行個體維持一段較長的時間，是很常使用的模式。</span><span class="sxs-lookup"><span data-stu-id="31dc0-468">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="31dc0-469">在移轉到 `IHttpClientFactory` 之後，就不再需要此模式。</span><span class="sxs-lookup"><span data-stu-id="31dc0-469">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="31dc0-470">IHTTPClientFactory 的替代方案</span><span class="sxs-lookup"><span data-stu-id="31dc0-470">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="31dc0-471">`IHttpClientFactory`在開啟 DI 的應用程式使用可避免:</span><span class="sxs-lookup"><span data-stu-id="31dc0-471">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="31dc0-472">通過池化實例解決`HttpMessageHandler`資源耗盡問題。</span><span class="sxs-lookup"><span data-stu-id="31dc0-472">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="31dc0-473">通過定期迴圈`HttpMessageHandler`實例來處理陳舊的 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="31dc0-473">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="31dc0-474">使用長壽命<xref:System.Net.Http.SocketsHttpHandler>實例解決上述問題的替代方法有其他方法。</span><span class="sxs-lookup"><span data-stu-id="31dc0-474">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="31dc0-475">創建應用何時啟動`SocketsHttpHandler`的實例,並將其用於應用的生命週期。</span><span class="sxs-lookup"><span data-stu-id="31dc0-475">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="31dc0-476">根據<xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime>DNS 刷新時間配置到適當的值。</span><span class="sxs-lookup"><span data-stu-id="31dc0-476">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="31dc0-477">根據需要`HttpClient`使用`new HttpClient(handler, disposeHandler: false)`實例創建實例。</span><span class="sxs-lookup"><span data-stu-id="31dc0-477">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="31dc0-478">前面的方法解決了以類似方式`IHttpClientFactory`解決的資源管理問題。</span><span class="sxs-lookup"><span data-stu-id="31dc0-478">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="31dc0-479">跨`SocketsHttpHandler``HttpClient`實例共享連接。</span><span class="sxs-lookup"><span data-stu-id="31dc0-479">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="31dc0-480">此共用可防止套接字耗盡。</span><span class="sxs-lookup"><span data-stu-id="31dc0-480">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="31dc0-481">根據`SocketsHttpHandler`迴圈`PooledConnectionLifetime`連接,以避免陳舊的 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="31dc0-481">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="31dc0-482">Cookie</span><span class="sxs-lookup"><span data-stu-id="31dc0-482">Cookies</span></span>

<span data-ttu-id="31dc0-483">`HttpMessageHandler`池實例會導致`CookieContainer`對象被共用。</span><span class="sxs-lookup"><span data-stu-id="31dc0-483">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="31dc0-484">意外`CookieContainer`的物件共用通常會導致代碼不正確。</span><span class="sxs-lookup"><span data-stu-id="31dc0-484">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="31dc0-485">對於需要 Cookie 的應用,請考慮以下任一:</span><span class="sxs-lookup"><span data-stu-id="31dc0-485">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="31dc0-486">關閉自動 Cookie 處理</span><span class="sxs-lookup"><span data-stu-id="31dc0-486">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="31dc0-487">避免`IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="31dc0-487">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="31dc0-488">呼叫<xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>以關閉自動 Cookie 處理:</span><span class="sxs-lookup"><span data-stu-id="31dc0-488">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="31dc0-489">記錄</span><span class="sxs-lookup"><span data-stu-id="31dc0-489">Logging</span></span>

<span data-ttu-id="31dc0-490">透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="31dc0-490">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="31dc0-491">在記錄設定中啟用適當的資訊層級，以查看預設記錄檔訊息。</span><span class="sxs-lookup"><span data-stu-id="31dc0-491">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="31dc0-492">額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。</span><span class="sxs-lookup"><span data-stu-id="31dc0-492">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="31dc0-493">用於每個用戶端的記錄檔分類包含用戶端的名稱。</span><span class="sxs-lookup"><span data-stu-id="31dc0-493">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="31dc0-494">例如,名為*MyNamedClient*的`System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`用戶端使用類別來記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="31dc0-494">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="31dc0-495">後面加上 *LogicalHandler* 的訊息發生在要求處理常式管線之外。</span><span class="sxs-lookup"><span data-stu-id="31dc0-495">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="31dc0-496">在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。</span><span class="sxs-lookup"><span data-stu-id="31dc0-496">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="31dc0-497">在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。</span><span class="sxs-lookup"><span data-stu-id="31dc0-497">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="31dc0-498">記錄也會發生在要求處理常式管線之內。</span><span class="sxs-lookup"><span data-stu-id="31dc0-498">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="31dc0-499">在 *MyNamedClient* 範例中，那些訊息是針對記錄檔分類 `System.Net.Http.HttpClient.MyNamedClient.ClientHandler` 而記錄。</span><span class="sxs-lookup"><span data-stu-id="31dc0-499">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="31dc0-500">對於要求，這是發生在所有其他處理常式都已執行之後，並且緊接在網路上傳送要求之前。</span><span class="sxs-lookup"><span data-stu-id="31dc0-500">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="31dc0-501">在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。</span><span class="sxs-lookup"><span data-stu-id="31dc0-501">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="31dc0-502">在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。</span><span class="sxs-lookup"><span data-stu-id="31dc0-502">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="31dc0-503">例如，這可能包括要求標頭的變更，或是回應狀態碼的變更。</span><span class="sxs-lookup"><span data-stu-id="31dc0-503">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="31dc0-504">在記錄分類中包含用戶端的名稱，可讓您在需要時進行特定具名用戶端的記錄檔篩選。</span><span class="sxs-lookup"><span data-stu-id="31dc0-504">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="31dc0-505">設定 HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="31dc0-505">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="31dc0-506">可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。</span><span class="sxs-lookup"><span data-stu-id="31dc0-506">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="31dc0-507">新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。</span><span class="sxs-lookup"><span data-stu-id="31dc0-507">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="31dc0-508"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 擴充方法可以用來定義委派。</span><span class="sxs-lookup"><span data-stu-id="31dc0-508">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="31dc0-509">委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：</span><span class="sxs-lookup"><span data-stu-id="31dc0-509">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="31dc0-510">在主控台應用程式中使用 IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="31dc0-510">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="31dc0-511">在主控台應用程式中，將下列套件參考新增至專案：</span><span class="sxs-lookup"><span data-stu-id="31dc0-511">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="31dc0-512">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="31dc0-512">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="31dc0-513">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="31dc0-513">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="31dc0-514">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="31dc0-514">In the following example:</span></span>

* <span data-ttu-id="31dc0-515"><xref:System.Net.Http.IHttpClientFactory> 已在[泛型主機的](xref:fundamentals/host/generic-host)服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="31dc0-515"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="31dc0-516">`MyService` 會從服務建立用戶端 Factory 執行個體，其可用來建立 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="31dc0-516">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="31dc0-517">`HttpClient` 會用來擷取網頁。</span><span class="sxs-lookup"><span data-stu-id="31dc0-517">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="31dc0-518">`Main` 會建立範圍來執行服務的 `GetPage` 方法，並將網頁內容的前 500 個字元寫入至主控台。</span><span class="sxs-lookup"><span data-stu-id="31dc0-518">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="31dc0-519">其他資源</span><span class="sxs-lookup"><span data-stu-id="31dc0-519">Additional resources</span></span>

* [<span data-ttu-id="31dc0-520">使用 HttpClientFactory 實作復原 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="31dc0-520">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="31dc0-521">使用 HttpClientFactory 和 Polly 原則以指數輪詢實作 HTTP 呼叫重試</span><span class="sxs-lookup"><span data-stu-id="31dc0-521">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="31dc0-522">實作斷路器模式</span><span class="sxs-lookup"><span data-stu-id="31dc0-522">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="31dc0-523">作者：[Glenn Condron](https://github.com/glennc)、[Ryan Nowak](https://github.com/rynowak) 和 [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="31dc0-523">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="31dc0-524"><xref:System.Net.Http.IHttpClientFactory> 可以註冊及用來在應用程式中設定和建立 <xref:System.Net.Http.HttpClient> 執行個體。</span><span class="sxs-lookup"><span data-stu-id="31dc0-524">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="31dc0-525">它提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="31dc0-525">It offers the following benefits:</span></span>

* <span data-ttu-id="31dc0-526">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="31dc0-526">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="31dc0-527">例如,可以註冊和配置為訪問[GitHub](https://github.com/)的*github*用戶端。</span><span class="sxs-lookup"><span data-stu-id="31dc0-527">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="31dc0-528">預設用戶端可以註冊用於其他用途。</span><span class="sxs-lookup"><span data-stu-id="31dc0-528">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="31dc0-529">透過委派 `HttpClient` 中的處理常式來撰寫外寄中介軟體的概念，並提供延伸模組以便 Polly 架構中介軟體利用外寄中介軟體。</span><span class="sxs-lookup"><span data-stu-id="31dc0-529">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="31dc0-530">管理基礎 `HttpClientMessageHandler` 執行個體的共用和存留期，以避免在手動管理 `HttpClient` 存留期時，發生的常見 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="31dc0-530">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="31dc0-531">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="31dc0-531">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="31dc0-532">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="31dc0-532">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="31dc0-533">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="31dc0-533">Prerequisites</span></span>

<span data-ttu-id="31dc0-534">以 .NET Framework 為目標的專案，需要安裝 [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="31dc0-534">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="31dc0-535">以 .NET Core 為目標且參考 [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) 的專案，已包含 `Microsoft.Extensions.Http` 套件。</span><span class="sxs-lookup"><span data-stu-id="31dc0-535">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="31dc0-536">耗用量模式</span><span class="sxs-lookup"><span data-stu-id="31dc0-536">Consumption patterns</span></span>

<span data-ttu-id="31dc0-537">有數種方式可將 `IHttpClientFactory` 用於應用程式：</span><span class="sxs-lookup"><span data-stu-id="31dc0-537">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="31dc0-538">基本用法</span><span class="sxs-lookup"><span data-stu-id="31dc0-538">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="31dc0-539">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="31dc0-539">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="31dc0-540">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="31dc0-540">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="31dc0-541">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="31dc0-541">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="31dc0-542">它們全都不會嚴格優先於另一個。</span><span class="sxs-lookup"><span data-stu-id="31dc0-542">None of them are strictly superior to another.</span></span> <span data-ttu-id="31dc0-543">最好的方法取決於應用程式的條件約束。</span><span class="sxs-lookup"><span data-stu-id="31dc0-543">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="31dc0-544">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="31dc0-544">Basic usage</span></span>

<span data-ttu-id="31dc0-545">`IHttpClientFactory` 可以藉由在 `Startup.ConfigureServices` 方法內的 `IServiceCollection` 上呼叫 `AddHttpClient` 擴充方法來註冊。</span><span class="sxs-lookup"><span data-stu-id="31dc0-545">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="31dc0-546">註冊後,代碼可以接受`IHttpClientFactory`任何可以注入[依賴項注入 (DI)](xref:fundamentals/dependency-injection)的服務。</span><span class="sxs-lookup"><span data-stu-id="31dc0-546">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="31dc0-547">`IHttpClientFactory`可建立實體`HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="31dc0-547">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="31dc0-548">以這種方式使用 `IHttpClientFactory` 是重構現有應用程式的好方法。</span><span class="sxs-lookup"><span data-stu-id="31dc0-548">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="31dc0-549">它對 `HttpClient` 的使用方式沒有任何影響。</span><span class="sxs-lookup"><span data-stu-id="31dc0-549">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="31dc0-550">在目前建立 `HttpClient` 執行個體的位置，將那些項目取代為呼叫 <xref:System.Net.Http.IHttpClientFactory.CreateClient*>。</span><span class="sxs-lookup"><span data-stu-id="31dc0-550">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="31dc0-551">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="31dc0-551">Named clients</span></span>

<span data-ttu-id="31dc0-552">如果應用程式需要使用多個不同的 `HttpClient`，且每個都有不同的設定，可以選擇使用**具名用戶端**。</span><span class="sxs-lookup"><span data-stu-id="31dc0-552">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="31dc0-553">具名 `HttpClient` 的組態可以在 `Startup.ConfigureServices` 中註冊時指定。</span><span class="sxs-lookup"><span data-stu-id="31dc0-553">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="31dc0-554">在前面的代碼中,`AddHttpClient`呼叫,提供名稱*github*。</span><span class="sxs-lookup"><span data-stu-id="31dc0-554">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="31dc0-555">此用戶端已套用一些預設組態&mdash;即使用 GitHub API 所需的基底位址和兩個標頭。</span><span class="sxs-lookup"><span data-stu-id="31dc0-555">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="31dc0-556">每次呼叫 `CreateClient` 時，會建立 `HttpClient` 的新執行個體並呼叫組態動作。</span><span class="sxs-lookup"><span data-stu-id="31dc0-556">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="31dc0-557">若要使用具名用戶端，可以傳遞字串參數至 `CreateClient`。</span><span class="sxs-lookup"><span data-stu-id="31dc0-557">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="31dc0-558">指定要建立之用戶端的名稱：</span><span class="sxs-lookup"><span data-stu-id="31dc0-558">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="31dc0-559">在上述程式碼中，要求不需要指定主機名稱。</span><span class="sxs-lookup"><span data-stu-id="31dc0-559">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="31dc0-560">它可以只傳遞路徑，因為已使用為用戶端設定的基底位址。</span><span class="sxs-lookup"><span data-stu-id="31dc0-560">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="31dc0-561">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="31dc0-561">Typed clients</span></span>

<span data-ttu-id="31dc0-562">具型別用戶端：</span><span class="sxs-lookup"><span data-stu-id="31dc0-562">Typed clients:</span></span>

* <span data-ttu-id="31dc0-563">提供與具名用戶端相同的功能，而不需使用字串作為索引鍵。</span><span class="sxs-lookup"><span data-stu-id="31dc0-563">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="31dc0-564">取用用戶端時提供 IntelliSense 和編譯器說明。</span><span class="sxs-lookup"><span data-stu-id="31dc0-564">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="31dc0-565">提供單一位置來設定特定的 `HttpClient` 並與其互動。</span><span class="sxs-lookup"><span data-stu-id="31dc0-565">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="31dc0-566">例如，單一的具型別用戶端可能用於單一的後端端點，並封裝處理該端點的所有邏輯。</span><span class="sxs-lookup"><span data-stu-id="31dc0-566">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="31dc0-567">使用 DI 且可在應用程式中需要之處插入。</span><span class="sxs-lookup"><span data-stu-id="31dc0-567">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="31dc0-568">型態化用戶端接受`HttpClient`其 建構函數中的參數:</span><span class="sxs-lookup"><span data-stu-id="31dc0-568">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="31dc0-569">在上述程式碼中，組態會移到具型別用戶端。</span><span class="sxs-lookup"><span data-stu-id="31dc0-569">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="31dc0-570">`HttpClient` 物件會公開為公用屬性。</span><span class="sxs-lookup"><span data-stu-id="31dc0-570">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="31dc0-571">您可定義 API 特定的方法，其公開 `HttpClient` 功能。</span><span class="sxs-lookup"><span data-stu-id="31dc0-571">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="31dc0-572">`GetAspNetDocsIssues` 方法會封裝從 GitHub 存放庫查詢和剖析最新開啟問題所需的程式碼。</span><span class="sxs-lookup"><span data-stu-id="31dc0-572">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="31dc0-573">若要註冊具型別用戶端，泛型 <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> 擴充方法可用於 `Startup.ConfigureServices` 內，並指定具型別用戶端類別：</span><span class="sxs-lookup"><span data-stu-id="31dc0-573">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="31dc0-574">具型別用戶端會向 DI 註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="31dc0-574">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="31dc0-575">具型別用戶端可以直接插入並使用：</span><span class="sxs-lookup"><span data-stu-id="31dc0-575">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="31dc0-576">想要的話，具型別用戶端的組態可以在 `Startup.ConfigureServices` 中註冊時指定，而不是在具型別用戶端的建構函式中：</span><span class="sxs-lookup"><span data-stu-id="31dc0-576">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="31dc0-577">可以將 `HttpClient` 完全封裝在具型別用戶端內。</span><span class="sxs-lookup"><span data-stu-id="31dc0-577">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="31dc0-578">可以提供在內部呼叫 `HttpClient` 執行個體的公用方法，而不將它公開為屬性。</span><span class="sxs-lookup"><span data-stu-id="31dc0-578">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="31dc0-579">在上述程式碼，`HttpClient` 儲存為私用欄位。</span><span class="sxs-lookup"><span data-stu-id="31dc0-579">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="31dc0-580">進行外部呼叫的所有存取都會經歷 `GetRepos` 方法。</span><span class="sxs-lookup"><span data-stu-id="31dc0-580">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="31dc0-581">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="31dc0-581">Generated clients</span></span>

<span data-ttu-id="31dc0-582">`IHttpClientFactory` 可和其他協力廠商程式庫一起使用，例如 [Refit](https://github.com/paulcbetts/refit)。</span><span class="sxs-lookup"><span data-stu-id="31dc0-582">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="31dc0-583">Refit 是適用於 .NET 的 REST 程式庫。</span><span class="sxs-lookup"><span data-stu-id="31dc0-583">Refit is a REST library for .NET.</span></span> <span data-ttu-id="31dc0-584">它將 REST API 轉換為即時介面。</span><span class="sxs-lookup"><span data-stu-id="31dc0-584">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="31dc0-585">介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。</span><span class="sxs-lookup"><span data-stu-id="31dc0-585">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="31dc0-586">定義介面及回覆來代表外部 API 和其回應：</span><span class="sxs-lookup"><span data-stu-id="31dc0-586">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="31dc0-587">可以新增具型別用戶端，使用 Refit 產生實作：</span><span class="sxs-lookup"><span data-stu-id="31dc0-587">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="31dc0-588">定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：</span><span class="sxs-lookup"><span data-stu-id="31dc0-588">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="31dc0-589">外寄要求中介軟體</span><span class="sxs-lookup"><span data-stu-id="31dc0-589">Outgoing request middleware</span></span>

<span data-ttu-id="31dc0-590">`HttpClient` 已經有委派可針對外寄 HTTP 要求連結在一起的處理常式的概念。</span><span class="sxs-lookup"><span data-stu-id="31dc0-590">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="31dc0-591">`IHttpClientFactory` 可讓您輕鬆地定義要套用於每個具名用戶端的處理常式。</span><span class="sxs-lookup"><span data-stu-id="31dc0-591">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="31dc0-592">它支援註冊和鏈結多個處理常式，以建置外寄要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="31dc0-592">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="31dc0-593">這些處理常式每個都可以在外寄要求之前和之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="31dc0-593">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="31dc0-594">此模式與 ASP.NET Core 中的輸入中介軟體管線相似。</span><span class="sxs-lookup"><span data-stu-id="31dc0-594">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="31dc0-595">模式提供一個機制來管理 HTTP 要求的跨領域關注，包括快取、錯誤處理、序列化和記錄。</span><span class="sxs-lookup"><span data-stu-id="31dc0-595">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="31dc0-596">若要建立處理常式，請定義衍生自 <xref:System.Net.Http.DelegatingHandler> 的類別。</span><span class="sxs-lookup"><span data-stu-id="31dc0-596">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="31dc0-597">覆寫 `SendAsync` 方法，以在將要求傳遞至管線中的下一個處理常式之前執行程式碼：</span><span class="sxs-lookup"><span data-stu-id="31dc0-597">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="31dc0-598">上述程式碼定義一個基本處理常式。</span><span class="sxs-lookup"><span data-stu-id="31dc0-598">The preceding code defines a basic handler.</span></span> <span data-ttu-id="31dc0-599">它會檢查以查看要求上是否已包含 `X-API-KEY` 標頭。</span><span class="sxs-lookup"><span data-stu-id="31dc0-599">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="31dc0-600">如果遺漏標頭，它可以避免 HTTP 呼叫，並傳回適當的回應。</span><span class="sxs-lookup"><span data-stu-id="31dc0-600">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="31dc0-601">在註冊期間,可以將一個或多個處理程式加入的設定中`HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="31dc0-601">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="31dc0-602">這項工作是透過 <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder> 上的擴充方法完成。</span><span class="sxs-lookup"><span data-stu-id="31dc0-602">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="31dc0-603">在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。</span><span class="sxs-lookup"><span data-stu-id="31dc0-603">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="31dc0-604">處理常式**必須**在 DI 中註冊為暫時性服務，無限定範圍。</span><span class="sxs-lookup"><span data-stu-id="31dc0-604">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="31dc0-605">如果處理常式已註冊為範圍服務，而且處理常式所相依的任何服務都是可處置的：</span><span class="sxs-lookup"><span data-stu-id="31dc0-605">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="31dc0-606">處理常式的服務可能會在處理常式超出範圍之前加以處置。</span><span class="sxs-lookup"><span data-stu-id="31dc0-606">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="31dc0-607">已處置的處理常式服務會導致處理常式失敗。</span><span class="sxs-lookup"><span data-stu-id="31dc0-607">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="31dc0-608">註冊之後，便可以呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>，並傳入處理常式類型。</span><span class="sxs-lookup"><span data-stu-id="31dc0-608">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="31dc0-609">可以遵循應該執行的順序來註冊多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="31dc0-609">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="31dc0-610">每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：</span><span class="sxs-lookup"><span data-stu-id="31dc0-610">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="31dc0-611">使用下列其中一種方式來與訊息處理常式共用個別要求狀態：</span><span class="sxs-lookup"><span data-stu-id="31dc0-611">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="31dc0-612">使用 `HttpRequestMessage.Properties` 將資料傳遞到處理常式。</span><span class="sxs-lookup"><span data-stu-id="31dc0-612">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="31dc0-613">使用 `IHttpContextAccessor` 來存取目前的要求。</span><span class="sxs-lookup"><span data-stu-id="31dc0-613">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="31dc0-614">建立自訂 `AsyncLocal` 儲存體物件以傳遞資料。</span><span class="sxs-lookup"><span data-stu-id="31dc0-614">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="31dc0-615">使用 Polly 為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="31dc0-615">Use Polly-based handlers</span></span>

<span data-ttu-id="31dc0-616">`IHttpClientFactory` 整合受歡迎的協力廠商程式庫，稱為 [Polly](https://github.com/App-vNext/Polly)。</span><span class="sxs-lookup"><span data-stu-id="31dc0-616">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="31dc0-617">Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。</span><span class="sxs-lookup"><span data-stu-id="31dc0-617">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="31dc0-618">它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。</span><span class="sxs-lookup"><span data-stu-id="31dc0-618">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="31dc0-619">提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="31dc0-619">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="31dc0-620">Polly 延伸模組：</span><span class="sxs-lookup"><span data-stu-id="31dc0-620">The Polly extensions:</span></span>

* <span data-ttu-id="31dc0-621">支援將以 Polly 為基礎的處理常式新增至用戶端。</span><span class="sxs-lookup"><span data-stu-id="31dc0-621">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="31dc0-622">可在安裝 [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet 套件後使用。</span><span class="sxs-lookup"><span data-stu-id="31dc0-622">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="31dc0-623">該套件並未包含在 ASP.NET Core 共用架構中。</span><span class="sxs-lookup"><span data-stu-id="31dc0-623">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="31dc0-624">處理暫時性錯誤</span><span class="sxs-lookup"><span data-stu-id="31dc0-624">Handle transient faults</span></span>

<span data-ttu-id="31dc0-625">大部分的錯誤發生在外部 HTTP 呼叫是暫時性的時候。</span><span class="sxs-lookup"><span data-stu-id="31dc0-625">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="31dc0-626">包含一個便利的擴充方法，稱為 `AddTransientHttpErrorPolicy`，它可允許定義原則來處理暫時性錯誤。</span><span class="sxs-lookup"><span data-stu-id="31dc0-626">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="31dc0-627">使用此延伸模組方法設定的原則，會處理 `HttpRequestException`、HTTP 5xx 回應和 HTTP 408 回應。</span><span class="sxs-lookup"><span data-stu-id="31dc0-627">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="31dc0-628">`AddTransientHttpErrorPolicy` 延伸模組可用於 `Startup.ConfigureServices` 內。</span><span class="sxs-lookup"><span data-stu-id="31dc0-628">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="31dc0-629">延伸模組能提供 `PolicyBuilder` 物件的存取，該物件已設定來處理代表可能暫時性錯誤的錯誤：</span><span class="sxs-lookup"><span data-stu-id="31dc0-629">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="31dc0-630">在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。</span><span class="sxs-lookup"><span data-stu-id="31dc0-630">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="31dc0-631">失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。</span><span class="sxs-lookup"><span data-stu-id="31dc0-631">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="31dc0-632">動態選取原則</span><span class="sxs-lookup"><span data-stu-id="31dc0-632">Dynamically select policies</span></span>

<span data-ttu-id="31dc0-633">有額外的擴充方法可用來新增 Polly 為基礎的處理常式。</span><span class="sxs-lookup"><span data-stu-id="31dc0-633">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="31dc0-634">其中一個這類延伸模組是 `AddPolicyHandler`，它有多個多載。</span><span class="sxs-lookup"><span data-stu-id="31dc0-634">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="31dc0-635">一個多載可讓您在定義要套用的原則時，檢查要求：</span><span class="sxs-lookup"><span data-stu-id="31dc0-635">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="31dc0-636">在上述程式碼中，如果外寄要求是 HTTP GET，就會套用 10 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="31dc0-636">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="31dc0-637">任何其他 HTTP 方法會使用 30 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="31dc0-637">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="31dc0-638">新增多個 Polly 處理常式</span><span class="sxs-lookup"><span data-stu-id="31dc0-638">Add multiple Polly handlers</span></span>

<span data-ttu-id="31dc0-639">通常會建立巢狀 Polly 原則，以提供增強的功能：</span><span class="sxs-lookup"><span data-stu-id="31dc0-639">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="31dc0-640">在上述範例中，會新增兩個處理常式。</span><span class="sxs-lookup"><span data-stu-id="31dc0-640">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="31dc0-641">第一個使用 `AddTransientHttpErrorPolicy` 延伸模組來新增重試原則。</span><span class="sxs-lookup"><span data-stu-id="31dc0-641">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="31dc0-642">失敗的要求會重試最多三次。</span><span class="sxs-lookup"><span data-stu-id="31dc0-642">Failed requests are retried up to three times.</span></span> <span data-ttu-id="31dc0-643">第二個 `AddTransientHttpErrorPolicy` 呼叫會新增斷路器原則。</span><span class="sxs-lookup"><span data-stu-id="31dc0-643">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="31dc0-644">如果循序發生五次失敗的嘗試，進一步的外部要求會遭到封鎖 30 秒。</span><span class="sxs-lookup"><span data-stu-id="31dc0-644">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="31dc0-645">斷路器原則可設定狀態。</span><span class="sxs-lookup"><span data-stu-id="31dc0-645">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="31dc0-646">透過此用戶端的所有呼叫都會共用相同的線路狀態。</span><span class="sxs-lookup"><span data-stu-id="31dc0-646">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="31dc0-647">從 Polly 登錄新增原則</span><span class="sxs-lookup"><span data-stu-id="31dc0-647">Add policies from the Polly registry</span></span>

<span data-ttu-id="31dc0-648">管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。</span><span class="sxs-lookup"><span data-stu-id="31dc0-648">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="31dc0-649">提供了擴充方法，可以使用來自登錄的原則新增處理常式：</span><span class="sxs-lookup"><span data-stu-id="31dc0-649">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="31dc0-650">在上述程式碼中，當 `PolicyRegistry` 新增至 `ServiceCollection` 時，註冊了兩個原則。</span><span class="sxs-lookup"><span data-stu-id="31dc0-650">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="31dc0-651">為了使用來自登錄的原則，使用了 `AddPolicyHandlerFromRegistry` 方法，並傳遞要套用的原則名稱。</span><span class="sxs-lookup"><span data-stu-id="31dc0-651">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="31dc0-652">關於 `IHttpClientFactory` Polly 整合的詳細資訊，可以在 [Polly Wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory) 上找到。</span><span class="sxs-lookup"><span data-stu-id="31dc0-652">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="31dc0-653">HttpClient 和存留期管理</span><span class="sxs-lookup"><span data-stu-id="31dc0-653">HttpClient and lifetime management</span></span>

<span data-ttu-id="31dc0-654">每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回新的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="31dc0-654">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="31dc0-655">每個具名用戶端都有一個 <xref:System.Net.Http.HttpMessageHandler>。</span><span class="sxs-lookup"><span data-stu-id="31dc0-655">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="31dc0-656">處理站會管理 `HttpMessageHandler` 執行個體的存留期。</span><span class="sxs-lookup"><span data-stu-id="31dc0-656">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="31dc0-657">`IHttpClientFactory` 會將處理站所建立的 `HttpMessageHandler` 執行個體放入集區以減少資源耗用量。</span><span class="sxs-lookup"><span data-stu-id="31dc0-657">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="31dc0-658">建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。</span><span class="sxs-lookup"><span data-stu-id="31dc0-658">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="31dc0-659">將處理常式放入集區非常實用，因為處理常式通常會管理自己專屬的底層 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="31dc0-659">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="31dc0-660">建立比所需數目更多的處理常式，可能會導致連線延遲。</span><span class="sxs-lookup"><span data-stu-id="31dc0-660">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="31dc0-661">有些處理常式也會保持連線無限期地開啟，這可能導致處理常式無法對 DNS 變更回應。</span><span class="sxs-lookup"><span data-stu-id="31dc0-661">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="31dc0-662">預設處理常式存留時間為兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="31dc0-662">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="31dc0-663">可以針對每個具名用戶端覆寫預設值。</span><span class="sxs-lookup"><span data-stu-id="31dc0-663">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="31dc0-664">若要覆寫它，請在建立用戶端時所傳回的 `IHttpClientBuilder` 上呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*>：</span><span class="sxs-lookup"><span data-stu-id="31dc0-664">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="31dc0-665">不需要處置用戶端。</span><span class="sxs-lookup"><span data-stu-id="31dc0-665">Disposal of the client isn't required.</span></span> <span data-ttu-id="31dc0-666">處置會取消傳出的要求，並保證指定的 `HttpClient` 執行個體在呼叫 <xref:System.IDisposable.Dispose*> 之後無法使用。</span><span class="sxs-lookup"><span data-stu-id="31dc0-666">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="31dc0-667">`IHttpClientFactory` 會追蹤並處置 `HttpClient` 執行個體使用的資源。</span><span class="sxs-lookup"><span data-stu-id="31dc0-667">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="31dc0-668">`HttpClient` 執行個體通常可視為 .NET 物件，不需要處置。</span><span class="sxs-lookup"><span data-stu-id="31dc0-668">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="31dc0-669">在開始使用 `IHttpClientFactory` 之前，讓單一 `HttpClient` 執行個體維持一段較長的時間，是很常使用的模式。</span><span class="sxs-lookup"><span data-stu-id="31dc0-669">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="31dc0-670">在移轉到 `IHttpClientFactory` 之後，就不再需要此模式。</span><span class="sxs-lookup"><span data-stu-id="31dc0-670">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="31dc0-671">IHTTPClientFactory 的替代方案</span><span class="sxs-lookup"><span data-stu-id="31dc0-671">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="31dc0-672">`IHttpClientFactory`在開啟 DI 的應用程式使用可避免:</span><span class="sxs-lookup"><span data-stu-id="31dc0-672">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="31dc0-673">通過池化實例解決`HttpMessageHandler`資源耗盡問題。</span><span class="sxs-lookup"><span data-stu-id="31dc0-673">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="31dc0-674">通過定期迴圈`HttpMessageHandler`實例來處理陳舊的 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="31dc0-674">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="31dc0-675">使用長壽命<xref:System.Net.Http.SocketsHttpHandler>實例解決上述問題的替代方法有其他方法。</span><span class="sxs-lookup"><span data-stu-id="31dc0-675">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="31dc0-676">創建應用何時啟動`SocketsHttpHandler`的實例,並將其用於應用的生命週期。</span><span class="sxs-lookup"><span data-stu-id="31dc0-676">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="31dc0-677">根據<xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime>DNS 刷新時間配置到適當的值。</span><span class="sxs-lookup"><span data-stu-id="31dc0-677">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="31dc0-678">根據需要`HttpClient`使用`new HttpClient(handler, disposeHandler: false)`實例創建實例。</span><span class="sxs-lookup"><span data-stu-id="31dc0-678">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="31dc0-679">前面的方法解決了以類似方式`IHttpClientFactory`解決的資源管理問題。</span><span class="sxs-lookup"><span data-stu-id="31dc0-679">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="31dc0-680">跨`SocketsHttpHandler``HttpClient`實例共享連接。</span><span class="sxs-lookup"><span data-stu-id="31dc0-680">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="31dc0-681">此共用可防止套接字耗盡。</span><span class="sxs-lookup"><span data-stu-id="31dc0-681">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="31dc0-682">根據`SocketsHttpHandler`迴圈`PooledConnectionLifetime`連接,以避免陳舊的 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="31dc0-682">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="31dc0-683">Cookie</span><span class="sxs-lookup"><span data-stu-id="31dc0-683">Cookies</span></span>

<span data-ttu-id="31dc0-684">`HttpMessageHandler`池實例會導致`CookieContainer`對象被共用。</span><span class="sxs-lookup"><span data-stu-id="31dc0-684">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="31dc0-685">意外`CookieContainer`的物件共用通常會導致代碼不正確。</span><span class="sxs-lookup"><span data-stu-id="31dc0-685">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="31dc0-686">對於需要 Cookie 的應用,請考慮以下任一:</span><span class="sxs-lookup"><span data-stu-id="31dc0-686">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="31dc0-687">關閉自動 Cookie 處理</span><span class="sxs-lookup"><span data-stu-id="31dc0-687">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="31dc0-688">避免`IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="31dc0-688">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="31dc0-689">呼叫<xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>以關閉自動 Cookie 處理:</span><span class="sxs-lookup"><span data-stu-id="31dc0-689">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="31dc0-690">記錄</span><span class="sxs-lookup"><span data-stu-id="31dc0-690">Logging</span></span>

<span data-ttu-id="31dc0-691">透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="31dc0-691">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="31dc0-692">在記錄設定中啟用適當的資訊層級，以查看預設記錄檔訊息。</span><span class="sxs-lookup"><span data-stu-id="31dc0-692">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="31dc0-693">額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。</span><span class="sxs-lookup"><span data-stu-id="31dc0-693">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="31dc0-694">用於每個用戶端的記錄檔分類包含用戶端的名稱。</span><span class="sxs-lookup"><span data-stu-id="31dc0-694">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="31dc0-695">例如,名為*MyNamedClient*的`System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`用戶端使用類別來記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="31dc0-695">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="31dc0-696">後面加上 *LogicalHandler* 的訊息發生在要求處理常式管線之外。</span><span class="sxs-lookup"><span data-stu-id="31dc0-696">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="31dc0-697">在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。</span><span class="sxs-lookup"><span data-stu-id="31dc0-697">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="31dc0-698">在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。</span><span class="sxs-lookup"><span data-stu-id="31dc0-698">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="31dc0-699">記錄也會發生在要求處理常式管線之內。</span><span class="sxs-lookup"><span data-stu-id="31dc0-699">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="31dc0-700">在 *MyNamedClient* 範例中，那些訊息是針對記錄檔分類 `System.Net.Http.HttpClient.MyNamedClient.ClientHandler` 而記錄。</span><span class="sxs-lookup"><span data-stu-id="31dc0-700">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="31dc0-701">對於要求，這是發生在所有其他處理常式都已執行之後，並且緊接在網路上傳送要求之前。</span><span class="sxs-lookup"><span data-stu-id="31dc0-701">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="31dc0-702">在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。</span><span class="sxs-lookup"><span data-stu-id="31dc0-702">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="31dc0-703">在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。</span><span class="sxs-lookup"><span data-stu-id="31dc0-703">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="31dc0-704">例如，這可能包括要求標頭的變更，或是回應狀態碼的變更。</span><span class="sxs-lookup"><span data-stu-id="31dc0-704">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="31dc0-705">在記錄分類中包含用戶端的名稱，可讓您在需要時進行特定具名用戶端的記錄檔篩選。</span><span class="sxs-lookup"><span data-stu-id="31dc0-705">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="31dc0-706">設定 HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="31dc0-706">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="31dc0-707">可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。</span><span class="sxs-lookup"><span data-stu-id="31dc0-707">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="31dc0-708">新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。</span><span class="sxs-lookup"><span data-stu-id="31dc0-708">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="31dc0-709"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 擴充方法可以用來定義委派。</span><span class="sxs-lookup"><span data-stu-id="31dc0-709">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="31dc0-710">委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：</span><span class="sxs-lookup"><span data-stu-id="31dc0-710">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="31dc0-711">在主控台應用程式中使用 IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="31dc0-711">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="31dc0-712">在主控台應用程式中，將下列套件參考新增至專案：</span><span class="sxs-lookup"><span data-stu-id="31dc0-712">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="31dc0-713">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="31dc0-713">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="31dc0-714">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="31dc0-714">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="31dc0-715">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="31dc0-715">In the following example:</span></span>

* <span data-ttu-id="31dc0-716"><xref:System.Net.Http.IHttpClientFactory> 已在[泛型主機的](xref:fundamentals/host/generic-host)服務容器中註冊。</span><span class="sxs-lookup"><span data-stu-id="31dc0-716"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="31dc0-717">`MyService` 會從服務建立用戶端 Factory 執行個體，其可用來建立 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="31dc0-717">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="31dc0-718">`HttpClient` 會用來擷取網頁。</span><span class="sxs-lookup"><span data-stu-id="31dc0-718">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="31dc0-719">`Main` 會建立範圍來執行服務的 `GetPage` 方法，並將網頁內容的前 500 個字元寫入至主控台。</span><span class="sxs-lookup"><span data-stu-id="31dc0-719">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="header-propagation-middleware"></a><span data-ttu-id="31dc0-720">頭傳播中間件</span><span class="sxs-lookup"><span data-stu-id="31dc0-720">Header propagation middleware</span></span>

<span data-ttu-id="31dc0-721">標頭傳播是社區支持的中間件,用於將 HTTP 標頭從傳入請求傳播到傳出的 HTTP 用戶端請求。</span><span class="sxs-lookup"><span data-stu-id="31dc0-721">Header propagation is a community supported middleware to propagate HTTP headers from the incoming request to the outgoing HTTP Client requests.</span></span> <span data-ttu-id="31dc0-722">要使用標頭傳播:</span><span class="sxs-lookup"><span data-stu-id="31dc0-722">To use header propagation:</span></span>

* <span data-ttu-id="31dc0-723">引用包[標題傳播](https://www.nuget.org/packages/HeaderPropagation)的社區支援埠。</span><span class="sxs-lookup"><span data-stu-id="31dc0-723">Reference the community supported port of the package [HeaderPropagation](https://www.nuget.org/packages/HeaderPropagation).</span></span> <span data-ttu-id="31dc0-724">ASP.NET核心3.1,後來支援[微軟.AspNetCore.頭傳播](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation)。</span><span class="sxs-lookup"><span data-stu-id="31dc0-724">ASP.NET Core 3.1 and later supports [Microsoft.AspNetCore.HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation).</span></span>

* <span data-ttu-id="31dc0-725">設定中間件與`HttpClient` `Startup` :</span><span class="sxs-lookup"><span data-stu-id="31dc0-725">Configure the middleware and `HttpClient` in `Startup`:</span></span>

  [!code-csharp[](http-requests/samples/2.x/Startup21.cs?highlight=5-9,25&name=snippet)]

* <span data-ttu-id="31dc0-726">用戶端包括出站要求的設定標頭:</span><span class="sxs-lookup"><span data-stu-id="31dc0-726">The client includes the configured headers on outbound requests:</span></span>

  ```csharp
  var client = clientFactory.CreateClient("MyForwardingClient");
  var response = client.GetAsync(...);
  ```

## <a name="additional-resources"></a><span data-ttu-id="31dc0-727">其他資源</span><span class="sxs-lookup"><span data-stu-id="31dc0-727">Additional resources</span></span>

* [<span data-ttu-id="31dc0-728">使用 HttpClientFactory 實作復原 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="31dc0-728">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="31dc0-729">使用 HttpClientFactory 和 Polly 原則以指數輪詢實作 HTTP 呼叫重試</span><span class="sxs-lookup"><span data-stu-id="31dc0-729">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="31dc0-730">實作斷路器模式</span><span class="sxs-lookup"><span data-stu-id="31dc0-730">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
