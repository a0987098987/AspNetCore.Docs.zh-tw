---
title: 初始化 HTTP 要求
author: stevejgordon
description: 深入了解在 ASP.NET Core 中使用 IHttpClientFactory 介面來管理邏輯 HttpClient 執行個體。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 08/07/2018
uid: fundamentals/http-requests
ms.openlocfilehash: 2a1bf78edb5068d8b10d66e5ef306b1ad4395da6
ms.sourcegitcommit: 15d7bd0b2c4e6fe9ac335d658bab71a45ca5bc72
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/20/2018
ms.locfileid: "41751593"
---
# <a name="initiate-http-requests"></a><span data-ttu-id="5c8e0-103">初始化 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="5c8e0-103">Initiate HTTP requests</span></span>

<span data-ttu-id="5c8e0-104">作者：[Glenn Condron](https://github.com/glennc)、[Ryan Nowak](https://github.com/rynowak) 和 [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="5c8e0-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="5c8e0-105">[IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) 可以註冊並用來在應用程式中設定和建立 [HttpClient](/dotnet/api/system.net.http.httpclient) 執行個體。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-105">An [IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) can be registered and used to configure and create [HttpClient](/dotnet/api/system.net.http.httpclient) instances in an app.</span></span> <span data-ttu-id="5c8e0-106">它提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="5c8e0-106">It offers the following benefits:</span></span>

* <span data-ttu-id="5c8e0-107">提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="5c8e0-108">例如，您可以註冊 *github* 並設定用戶端以存取 GitHub。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-108">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="5c8e0-109">預設用戶端可以註冊用於其他用途。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-109">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="5c8e0-110">透過委派 `HttpClient` 中的處理常式來撰寫外寄中介軟體的概念，並提供延伸模組以便 Polly 架構中介軟體利用外寄中介軟體。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="5c8e0-111">管理基礎 `HttpClientMessageHandler` 執行個體的共用和存留期，以避免在手動管理 `HttpClient` 存留期時，發生的常見 DNS 問題。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-111">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="5c8e0-112">針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-112">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="5c8e0-113">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5c8e0-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5c8e0-114">必要條件</span><span class="sxs-lookup"><span data-stu-id="5c8e0-114">Prerequisites</span></span>

<span data-ttu-id="5c8e0-115">以 .NET Framework 為目標的專案，需要安裝 [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-115">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="5c8e0-116">以 .NET Core 為目標且參考 [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) 的專案，已包含 `Microsoft.Extensions.Http` 套件。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-116">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="5c8e0-117">耗用模式</span><span class="sxs-lookup"><span data-stu-id="5c8e0-117">Consumption patterns</span></span>

<span data-ttu-id="5c8e0-118">有數種方式可將 `IHttpClientFactory` 用於應用程式：</span><span class="sxs-lookup"><span data-stu-id="5c8e0-118">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="5c8e0-119">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="5c8e0-119">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="5c8e0-120">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="5c8e0-120">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="5c8e0-121">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="5c8e0-121">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="5c8e0-122">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="5c8e0-122">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="5c8e0-123">它們全都不會嚴格優先於另一個。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-123">None of them are strictly superior to another.</span></span> <span data-ttu-id="5c8e0-124">最好的方法取決於應用程式的條件約束。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-124">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="5c8e0-125">基本使用方式</span><span class="sxs-lookup"><span data-stu-id="5c8e0-125">Basic usage</span></span>

<span data-ttu-id="5c8e0-126">`IHttpClientFactory` 可以藉由在 `Startup.ConfigureServices` 方法內的 `IServiceCollection` 上呼叫 `AddHttpClient` 擴充方法來註冊。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-126">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="5c8e0-127">註冊之後，程式碼可以在可使用[相依性插入](xref:fundamentals/dependency-injection) (DI) 插入服務的任何位置，接受 `IHttpClientFactory`。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-127">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="5c8e0-128">`IHttpClientFactory` 可以用來建立 `HttpClient` 執行個體：</span><span class="sxs-lookup"><span data-stu-id="5c8e0-128">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="5c8e0-129">以這種方式使用 `IHttpClientFactory` 是重構現有應用程式的好方法。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-129">Using `IHttpClientFactory` in this fashion is a great way to refactor an existing app.</span></span> <span data-ttu-id="5c8e0-130">它對 `HttpClient` 的使用方式沒有任何影響。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-130">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="5c8e0-131">在目前建立 `HttpClient` 執行個體的位置，將那些項目取代為呼叫 [CreateClient](/dotnet/api/system.net.http.ihttpclientfactory.createclient)。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-131">In places where `HttpClient` instances are currently created, replace those occurrences with a call to [CreateClient](/dotnet/api/system.net.http.ihttpclientfactory.createclient).</span></span>

### <a name="named-clients"></a><span data-ttu-id="5c8e0-132">具名用戶端</span><span class="sxs-lookup"><span data-stu-id="5c8e0-132">Named clients</span></span>

<span data-ttu-id="5c8e0-133">如果應用程式需要使用多個不同的 `HttpClient`，且每個都有不同的設定，可以選擇使用**具名用戶端**。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-133">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="5c8e0-134">具名 `HttpClient` 的組態可以在 `Startup.ConfigureServices` 中註冊時指定。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-134">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="5c8e0-135">在上述程式碼中，會呼叫 `AddHttpClient` 並提供名稱 *github*。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-135">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="5c8e0-136">此用戶端已套用一些預設組態&mdash;即使用 GitHub API 所需的基底位址和兩個標頭。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-136">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="5c8e0-137">每次呼叫 `CreateClient` 時，會建立 `HttpClient` 的新執行個體並呼叫組態動作。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-137">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="5c8e0-138">若要使用具名用戶端，可以傳遞字串參數至 `CreateClient`。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-138">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="5c8e0-139">指定要建立之用戶端的名稱：</span><span class="sxs-lookup"><span data-stu-id="5c8e0-139">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="5c8e0-140">在上述程式碼中，要求不需要指定主機名稱。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-140">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="5c8e0-141">它可以只傳遞路徑，因為已使用為用戶端設定的基底位址。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-141">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="5c8e0-142">具型別用戶端</span><span class="sxs-lookup"><span data-stu-id="5c8e0-142">Typed clients</span></span>

<span data-ttu-id="5c8e0-143">具型別用戶端提供與具名用戶端相同的功能，而不需要使用字串作為索引鍵。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-143">Typed clients provide the same capabilities as named clients without the need to use strings as keys.</span></span> <span data-ttu-id="5c8e0-144">具型別用戶端方法在使用用戶端時提供 IntelliSense 和編譯器說明。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-144">The typed client approach provides IntelliSense and compiler help when consuming clients.</span></span> <span data-ttu-id="5c8e0-145">它們提供單一位置來設定特定 `HttpClient` 並與其互動。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-145">They provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="5c8e0-146">例如，單一的具型別用戶端可能用於單一的後端端點，並封裝處理該端點的所有邏輯。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-146">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span> <span data-ttu-id="5c8e0-147">另一個優點是它們使用 DI 且可以在應用程式中需要之處插入。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-147">Another advantage is that they work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="5c8e0-148">具型別用戶端在其建構函式中接受 `HttpClient` 參數：</span><span class="sxs-lookup"><span data-stu-id="5c8e0-148">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="5c8e0-149">在上述程式碼中，組態會移到具型別用戶端。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-149">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="5c8e0-150">`HttpClient` 物件會公開為公用屬性。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-150">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="5c8e0-151">您可定義 API 特定的方法，其公開 `HttpClient` 功能。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-151">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="5c8e0-152">`GetAspNetDocsIssues` 方法會封裝從 GitHub 存放庫查詢和剖析最新開啟問題所需的程式碼。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-152">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="5c8e0-153">若要註冊具型別用戶端，泛型 `AddHttpClient` 擴充方法可用於 `Startup.ConfigureServices` 內，並指定具型別用戶端類別：</span><span class="sxs-lookup"><span data-stu-id="5c8e0-153">To register a typed client, the generic `AddHttpClient` extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="5c8e0-154">具型別用戶端會向 DI 註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-154">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="5c8e0-155">具型別用戶端可以直接插入並使用：</span><span class="sxs-lookup"><span data-stu-id="5c8e0-155">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="5c8e0-156">想要的話，具型別用戶端的組態可以在 `Startup.ConfigureServices` 中註冊時指定，而不是在具型別用戶端的建構函式中：</span><span class="sxs-lookup"><span data-stu-id="5c8e0-156">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="5c8e0-157">可以將 `HttpClient` 完全封裝在具型別用戶端內。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-157">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="5c8e0-158">可以提供在內部呼叫 `HttpClient` 執行個體的公用方法，而不將它公開為屬性。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-158">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="5c8e0-159">在上述程式碼，`HttpClient` 儲存為私用欄位。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-159">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="5c8e0-160">進行外部呼叫的所有存取都會經歷 `GetRepos` 方法。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-160">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="5c8e0-161">產生的用戶端</span><span class="sxs-lookup"><span data-stu-id="5c8e0-161">Generated clients</span></span>

<span data-ttu-id="5c8e0-162">`IHttpClientFactory` 可和其他協力廠商程式庫一起使用，例如 [Refit](https://github.com/paulcbetts/refit)。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-162">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="5c8e0-163">Refit 是適用於 .NET 的 REST 程式庫。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-163">Refit is a REST library for .NET.</span></span> <span data-ttu-id="5c8e0-164">它將 REST API 轉換為即時介面。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-164">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="5c8e0-165">介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-165">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="5c8e0-166">定義介面及回覆來代表外部 API 和其回應：</span><span class="sxs-lookup"><span data-stu-id="5c8e0-166">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="5c8e0-167">可以新增具型別用戶端，使用 Refit 產生實作：</span><span class="sxs-lookup"><span data-stu-id="5c8e0-167">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="5c8e0-168">定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：</span><span class="sxs-lookup"><span data-stu-id="5c8e0-168">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="5c8e0-169">外寄要求中介軟體</span><span class="sxs-lookup"><span data-stu-id="5c8e0-169">Outgoing request middleware</span></span>

<span data-ttu-id="5c8e0-170">`HttpClient` 已經有委派可針對外寄 HTTP 要求連結在一起的處理常式的概念。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-170">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="5c8e0-171">`IHttpClientFactory` 可讓您輕鬆地定義要套用於每個具名用戶端的處理常式。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-171">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="5c8e0-172">它支援註冊和鏈結多個處理常式，以建置外寄要求中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-172">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="5c8e0-173">這些處理常式每個都可以在外寄要求之前和之後執行工作。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-173">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="5c8e0-174">此模式與 ASP.NET Core 中的輸入中介軟體管線相似。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-174">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="5c8e0-175">模式提供一個機制來管理 HTTP 要求的跨領域關注，包括快取、錯誤處理、序列化和記錄。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-175">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="5c8e0-176">若要建立處理常式，請定義衍生自 `DelegatingHandler` 的類別。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-176">To create a handler, define a class deriving from `DelegatingHandler`.</span></span> <span data-ttu-id="5c8e0-177">覆寫 `SendAsync` 方法，以在將要求傳遞至管線中的下一個處理常式之前執行程式碼：</span><span class="sxs-lookup"><span data-stu-id="5c8e0-177">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="5c8e0-178">上述程式碼定義一個基本處理常式。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-178">The preceding code defines a basic handler.</span></span> <span data-ttu-id="5c8e0-179">它會檢查以查看要求上是否已包含 `X-API-KEY` 標頭。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-179">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="5c8e0-180">如果遺漏標頭，它可以避免 HTTP 呼叫，並傳回適當的回應。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-180">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="5c8e0-181">在註冊期間，可以新增一或多個處理常式至 `HttpClient` 的組態。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-181">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="5c8e0-182">此工作是透過 [IHttpClientBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.ihttpclientbuilder) 上的擴充方法完成。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-182">This task is accomplished via extension methods on the [IHttpClientBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.ihttpclientbuilder).</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="5c8e0-183">在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-183">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="5c8e0-184">處理常式**必須**在 DI 中註冊為暫時性。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-184">The handler **must** be registered in DI as transient.</span></span> <span data-ttu-id="5c8e0-185">註冊之後，便可以呼叫 [AddHttpMessageHandler](/dotnet/api/microsoft.extensions.dependencyinjection.httpclientbuilderextensions.addhttpmessagehandler)，並傳入處理常式的類型。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-185">Once registered, [AddHttpMessageHandler](/dotnet/api/microsoft.extensions.dependencyinjection.httpclientbuilderextensions.addhttpmessagehandler) can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="5c8e0-186">可以遵循應該執行的順序來註冊多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-186">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="5c8e0-187">每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：</span><span class="sxs-lookup"><span data-stu-id="5c8e0-187">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

## <a name="use-polly-based-handlers"></a><span data-ttu-id="5c8e0-188">使用 Polly 為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="5c8e0-188">Use Polly-based handlers</span></span>

<span data-ttu-id="5c8e0-189">`IHttpClientFactory` 整合受歡迎的協力廠商程式庫，稱為 [Polly](https://github.com/App-vNext/Polly)。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-189">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="5c8e0-190">Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-190">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="5c8e0-191">它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-191">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="5c8e0-192">提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-192">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="5c8e0-193">Polly 延伸模組提供於 [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet 套件中。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-193">The Polly extensions are available in the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="5c8e0-194">[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)未包含此套件。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-194">This package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="5c8e0-195">若要使用延伸模組，專案中應該要包含明確的 `<PackageReference />`。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-195">To use the extensions, an explicit `<PackageReference />` should be included in the project.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/HttpClientFactorySample.csproj?highlight=9)]

<span data-ttu-id="5c8e0-196">在還原此套件之後，擴充方法可用來支援將 Polly 為基礎的處理常式新增至用戶端。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-196">After restoring this package, extension methods are available to support adding Polly-based handlers to clients.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="5c8e0-197">處理暫時性錯誤</span><span class="sxs-lookup"><span data-stu-id="5c8e0-197">Handle transient faults</span></span>

<span data-ttu-id="5c8e0-198">大部分的錯誤發生在外部 HTTP 呼叫是暫時性的時候。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-198">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="5c8e0-199">包含一個便利的擴充方法，稱為 `AddTransientHttpErrorPolicy`，它可允許定義原則來處理暫時性錯誤。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-199">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="5c8e0-200">使用此延伸模組方法設定的原則，會處理 `HttpRequestException`、HTTP 5xx 回應和 HTTP 408 回應。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-200">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="5c8e0-201">`AddTransientHttpErrorPolicy` 延伸模組可用於 `Startup.ConfigureServices` 內。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-201">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="5c8e0-202">延伸模組能提供 `PolicyBuilder` 物件的存取，該物件已設定來處理代表可能暫時性錯誤的錯誤：</span><span class="sxs-lookup"><span data-stu-id="5c8e0-202">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="5c8e0-203">在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-203">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="5c8e0-204">失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-204">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="5c8e0-205">動態選取原則</span><span class="sxs-lookup"><span data-stu-id="5c8e0-205">Dynamically select policies</span></span>

<span data-ttu-id="5c8e0-206">有額外的擴充方法可用來新增 Polly 為基礎的處理常式。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-206">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="5c8e0-207">其中一個這類延伸模組是 `AddPolicyHandler`，它有多個多載。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-207">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="5c8e0-208">一個多載可讓您在定義要套用的原則時，檢查要求：</span><span class="sxs-lookup"><span data-stu-id="5c8e0-208">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="5c8e0-209">在上述程式碼中，如果外寄要求是 GET，就會套用 10 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-209">In the preceding code, if the outgoing request is a GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="5c8e0-210">任何其他 HTTP 方法會使用 30 秒逾時。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-210">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="5c8e0-211">新增多個 Polly 處理常式</span><span class="sxs-lookup"><span data-stu-id="5c8e0-211">Add multiple Polly handlers</span></span>

<span data-ttu-id="5c8e0-212">通常會建立巢狀 Polly 原則，以提供增強的功能：</span><span class="sxs-lookup"><span data-stu-id="5c8e0-212">It is common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="5c8e0-213">在上述範例中，會新增兩個處理常式。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-213">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="5c8e0-214">第一個使用 `AddTransientHttpErrorPolicy` 延伸模組來新增重試原則。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-214">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="5c8e0-215">失敗的要求會重試最多三次。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-215">Failed requests are retried up to three times.</span></span> <span data-ttu-id="5c8e0-216">第二個 `AddTransientHttpErrorPolicy` 呼叫會新增斷路器原則。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-216">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="5c8e0-217">如果循序發生五次失敗的嘗試，進一步的外部要求會遭到封鎖 30 秒。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-217">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="5c8e0-218">斷路器原則可設定狀態。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-218">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="5c8e0-219">透過此用戶端的所有呼叫都會共用相同的線路狀態。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-219">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="5c8e0-220">從 Polly 登錄新增原則</span><span class="sxs-lookup"><span data-stu-id="5c8e0-220">Add policies from the Polly registry</span></span>

<span data-ttu-id="5c8e0-221">管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-221">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="5c8e0-222">提供了擴充方法，可以使用來自登錄的原則新增處理常式：</span><span class="sxs-lookup"><span data-stu-id="5c8e0-222">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="5c8e0-223">在上述程式碼中，當 `PolicyRegistry` 新增至 `ServiceCollection` 時，註冊了兩個原則。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-223">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="5c8e0-224">為了使用來自登錄的原則，使用了 `AddPolicyHandlerFromRegistry` 方法，並傳遞要套用的原則名稱。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-224">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="5c8e0-225">關於 `IHttpClientFactory` Polly 整合的詳細資訊，可以在 [Polly Wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory) 上找到。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-225">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="5c8e0-226">HttpClient 和存留期管理</span><span class="sxs-lookup"><span data-stu-id="5c8e0-226">HttpClient and lifetime management</span></span>

<span data-ttu-id="5c8e0-227">每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回新的 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-227">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="5c8e0-228">每個具名用戶端都會有一個 [HttpMessageHandler](/dotnet/api/system.net.http.httpmessagehandler)。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-228">There's an [HttpMessageHandler](/dotnet/api/system.net.http.httpmessagehandler) per named client.</span></span> <span data-ttu-id="5c8e0-229">`IHttpClientFactory` 會將處理站所建立的 `HttpMessageHandler` 執行個體放入集區以減少資源耗用量。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-229">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="5c8e0-230">建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-230">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="5c8e0-231">將處理常式放入集區非常實用，因為處理常式通常會管理自己專屬的底層 HTTP 連線。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-231">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="5c8e0-232">建立比所需數目更多的處理常式，可能會導致連線延遲。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-232">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="5c8e0-233">有些處理常式也會保持連線無限期地開啟，這可能導致處理常式無法對 DNS 變更回應。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-233">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="5c8e0-234">預設處理常式存留時間為兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-234">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="5c8e0-235">可以針對每個具名用戶端覆寫預設值。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-235">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="5c8e0-236">若要覆寫它，請在於建立用戶端時所傳回的 `IHttpClientBuilder` 上呼叫 [SetHandlerLifetime](/dotnet/api/microsoft.extensions.dependencyinjection.httpclientbuilderextensions.sethandlerlifetime)：</span><span class="sxs-lookup"><span data-stu-id="5c8e0-236">To override it, call [SetHandlerLifetime](/dotnet/api/microsoft.extensions.dependencyinjection.httpclientbuilderextensions.sethandlerlifetime) on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="5c8e0-237">不需要處置用戶端。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-237">Disposal of the client isn't required.</span></span> <span data-ttu-id="5c8e0-238">處置會取消傳出的要求，並保證指定的 `HttpClient` 執行個體在呼叫 [Dispose](/dotnet/api/system.idisposable.dispose#System_IDisposable_Dispose) 之後無法使用。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-238">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling [Dispose](/dotnet/api/system.idisposable.dispose#System_IDisposable_Dispose).</span></span> <span data-ttu-id="5c8e0-239">`IHttpClientFactory` 會追蹤並處置 `HttpClient` 執行個體使用的資源。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-239">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="5c8e0-240">`HttpClient` 執行個體通常可視為 .NET 物件，不需要處置。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-240">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="5c8e0-241">在開始使用 `IHttpClientFactory` 之前，讓單一 `HttpClient` 執行個體維持一段較長的時間，是很常使用的模式。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-241">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="5c8e0-242">在移轉到 `IHttpClientFactory` 之後，就不再需要此模式。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-242">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="5c8e0-243">記錄</span><span class="sxs-lookup"><span data-stu-id="5c8e0-243">Logging</span></span>

<span data-ttu-id="5c8e0-244">透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-244">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="5c8e0-245">在記錄設定中啟用適當的資訊層級，以查看預設記錄檔訊息。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-245">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="5c8e0-246">額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-246">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="5c8e0-247">用於每個用戶端的記錄檔分類包含用戶端的名稱。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-247">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="5c8e0-248">例如，名為 *MyNamedClient* 的用戶端會記錄分類為 `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler` 的訊息。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-248">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="5c8e0-249">後面加上 *LogicalHandler* 的訊息發生在要求處理常式管線之外。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-249">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="5c8e0-250">在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-250">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="5c8e0-251">在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-251">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="5c8e0-252">記錄也會發生在要求處理常式管線之內。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-252">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="5c8e0-253">在 *MyNamedClient* 範例中，那些訊息是針對記錄檔分類 `System.Net.Http.HttpClient.MyNamedClient.ClientHandler` 而記錄。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-253">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="5c8e0-254">對於要求，這是發生在所有其他處理常式都已執行之後，並且緊接在網路上傳送要求之前。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-254">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="5c8e0-255">在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-255">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="5c8e0-256">在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-256">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="5c8e0-257">例如，這可能包括要求標頭的變更，或是回應狀態碼的變更。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-257">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="5c8e0-258">在記錄分類中包含用戶端的名稱，可讓您在需要時進行特定具名用戶端的記錄檔篩選。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-258">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="5c8e0-259">設定 HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="5c8e0-259">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="5c8e0-260">可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-260">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="5c8e0-261">新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-261">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="5c8e0-262">[ConfigurePrimaryHttpMessageHandler](/dotnet/api/microsoft.extensions.dependencyinjection.httpclientbuilderextensions.configureprimaryhttpmessagehandler) 擴充方法可用來定義委派。</span><span class="sxs-lookup"><span data-stu-id="5c8e0-262">The [ConfigurePrimaryHttpMessageHandler](/dotnet/api/microsoft.extensions.dependencyinjection.httpclientbuilderextensions.configureprimaryhttpmessagehandler) extension method can be used to define a delegate.</span></span> <span data-ttu-id="5c8e0-263">委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：</span><span class="sxs-lookup"><span data-stu-id="5c8e0-263">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]
