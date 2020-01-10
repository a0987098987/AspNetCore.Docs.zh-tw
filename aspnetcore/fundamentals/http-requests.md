---
title: 在 ASP.NET Core 中使用 IHttpClientFactory 發出 HTTP 要求
author: stevejgordon
description: 深入了解在 ASP.NET Core 中使用 IHttpClientFactory 介面來管理邏輯 HttpClient 執行個體。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 12/16/2019
uid: fundamentals/http-requests
ms.openlocfilehash: 482f8e28c23c621cecaf9ce111d89e9166ea6d85
ms.sourcegitcommit: da2fb2d78ce70accdba903ccbfdcfffdd0112123
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2020
ms.locfileid: "75722722"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a>在 ASP.NET Core 中使用 IHttpClientFactory 發出 HTTP 要求

::: moniker range=">= aspnetcore-3.0"

作者： [Glenn Condron](https://github.com/glennc)、 [Ryan Nowak](https://github.com/rynowak)、 [Steve Gordon](https://github.com/stevejgordon)、 [Rick Anderson](https://twitter.com/RickAndMSFT)和[Kirk Larkin](https://github.com/serpent5)

<xref:System.Net.Http.IHttpClientFactory> 可以註冊及用來在應用程式中設定和建立 <xref:System.Net.Http.HttpClient> 執行個體。 `IHttpClientFactory` 提供下列優點：

* 提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。 例如，名為*github*的用戶端可以註冊並設定為存取[github](https://github.com/)。 預設用戶端可以註冊以進行一般存取。
* 透過 `HttpClient`中的委派處理常式，制訂外寄中介軟體的概念。 提供 Polly 為基礎中介軟體的延伸模組，以利用 `HttpClient`中的委派處理常式。
* 管理基礎 `HttpClientMessageHandler` 實例的共用和存留期。 自動管理可避免在手動管理 `HttpClient` 存留期時所發生的常見 DNS （網域名稱系統）問題。
* 針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([如何下載](xref:index#how-to-download-a-sample))。

本主題中的範例程式碼會使用 <xref:System.Text.Json> 來還原序列化 HTTP 回應中所傳回的 JSON 內容。 如需使用 `Json.NET` 和 `ReadAsAsync<T>`的範例，請使用版本選取器來選取此主題的2.x 版。

## <a name="consumption-patterns"></a>耗用模式

有數種方式可將 `IHttpClientFactory` 用於應用程式：

* [基本使用方式](#basic-usage)
* [具名用戶端](#named-clients)
* [具型別用戶端](#typed-clients)
* [產生的用戶端](#generated-clients)

最佳方法取決於應用程式的需求。

### <a name="basic-usage"></a>基本使用方式

`IHttpClientFactory` 可以藉由呼叫 `AddHttpClient`進行註冊：

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

`IHttpClientFactory` 可以使用相依性[插入（DI）](xref:fundamentals/dependency-injection)來要求。 下列程式碼會使用 `IHttpClientFactory` 來建立 `HttpClient` 實例：

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

使用上述範例中的 `IHttpClientFactory` 就是重構現有應用程式的好方法。 這不會影響使用 `HttpClient` 的方式。 在現有應用程式中建立 `HttpClient` 實例的位置，使用 <xref:System.Net.Http.IHttpClientFactory.CreateClient*>的呼叫來取代這些專案。

### <a name="named-clients"></a>具名用戶端

在下列情況中，命名的用戶端是不錯的選擇：

* 應用程式需要 `HttpClient`的許多不同用途。
* 許多 `HttpClient`都有不同的設定。

在 `Startup.ConfigureServices`註冊期間，可以指定已命名 `HttpClient` 的設定：

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

在上述程式碼中，用戶端是使用下列設定：

* 基底位址 `https://api.github.com/`。
* 使用 GitHub API 時需要兩個標頭。

#### <a name="createclient"></a>CreateClient

每次呼叫 <xref:System.Net.Http.IHttpClientFactory.CreateClient*> 時：

* 建立 `HttpClient` 的新實例。
* 會呼叫設定動作。

若要建立名為的用戶端，請將其名稱傳遞至 `CreateClient`：

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

在上述程式碼中，要求不需要指定主機名稱。 此程式碼只會傳遞路徑，因為會使用為用戶端設定的基底位址。

### <a name="typed-clients"></a>具型別用戶端

具型別用戶端：

* 提供與具名用戶端相同的功能，而不需使用字串作為索引鍵。
* 取用用戶端時提供 IntelliSense 和編譯器說明。
* 提供單一位置來設定特定的 `HttpClient` 並與其互動。 例如，可能會使用單一具型別用戶端：
  * 適用于單一後端端點。
  * 封裝處理端點的所有邏輯。
* 使用 DI，並可在應用程式中需要的位置插入。

具型別用戶端會接受其程式化中的 `HttpClient` 參數：

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

在上述程式碼中：

* 設定會移到具型別用戶端。
* `HttpClient` 物件會公開為公用屬性。

可以建立可公開 `HttpClient` 功能的 API 特定方法。 例如，`GetAspNetDocsIssues` 方法會封裝程式碼以取得未解決的問題。

下列程式碼會呼叫 `Startup.ConfigureServices` 中的 <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> 來註冊具類型的用戶端類別：

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

具型別用戶端會向 DI 註冊為暫時性。 具型別用戶端可以直接插入並使用：

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

型別用戶端的設定可以在 `Startup.ConfigureServices`註冊期間指定，而不是在具型別用戶端的函式中：

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

`HttpClient` 可以封裝在具型別用戶端中。 請定義在內部呼叫 `HttpClient` 實例的方法，而不是將它公開為屬性：

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

在上述程式碼中，`HttpClient` 儲存在私用欄位中。 `HttpClient` 的存取權是公用 `GetRepos` 方法。

### <a name="generated-clients"></a>產生的用戶端

`IHttpClientFactory` 可以與協力廠商程式庫搭配使用，例如重新[調整。](https://github.com/paulcbetts/refit) Refit 是適用於 .NET 的 REST 程式庫。 它將 REST API 轉換為即時介面。 介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。

定義介面及回覆來代表外部 API 和其回應：

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

可以新增具型別用戶端，使用 Refit 產生實作：

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

定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：

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

## <a name="outgoing-request-middleware"></a>外寄要求中介軟體

`HttpClient` 具有委派處理常式的概念，可以針對傳出 HTTP 要求連結在一起。 `IHttpClientFactory`：

* 簡化定義要套用至每個已命名用戶端的處理常式。
* 支援多個處理常式的註冊和連結，以建立外寄要求中介軟體管線。 這些處理常式每個都可以在外寄要求之前和之後執行工作。 此模式：

  * 類似 ASP.NET Core 中的輸入中介軟體管線。
  * 提供一種機制來管理 HTTP 要求的跨領域考慮，例如：

    * 快取
    * 錯誤處理
    * serialization
    * 記錄

若要建立委派處理常式：

* 衍生自 <xref:System.Net.Http.DelegatingHandler>。
* 覆寫 <xref:System.Net.Http.DelegatingHandler.SendAsync*>。 先執行程式碼，再將要求傳遞至管線中的下一個處理常式：

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

上述程式碼會檢查 `X-API-KEY` 標頭是否在要求中。 如果遺漏 `X-API-KEY`，則會傳回 <xref:System.Net.HttpStatusCode.BadRequest>。

可以將一個以上的處理常式新增至具有 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>之 `HttpClient` 的設定：

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。 `IHttpClientFactory` 會為每個處理常式建立個別的 DI 範圍。 處理常式可以相依于任何範圍的服務。 處置處理常式時，會處置處理常式所相依的服務。

註冊之後，便可以呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>，並傳入處理常式的類型。

可以遵循應該執行的順序來註冊多個處理常式。 每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

使用下列其中一種方式來與訊息處理常式共用個別要求狀態：

* 使用[HttpRequestMessage](xref:System.Net.Http.HttpRequestMessage.Properties)將資料傳遞至處理常式。
* 使用 <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> 來存取目前的要求。
* 建立自訂 <xref:System.Threading.AsyncLocal`1> 儲存體物件以傳遞資料。

## <a name="use-polly-based-handlers"></a>使用 Polly 為基礎的處理常式

`IHttpClientFactory` 與協力廠商程式庫[Polly](https://github.com/App-vNext/Polly)整合。 Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。 它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。

提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。 Polly 擴充功能支援將以 Polly 為基礎的處理常式新增至用戶端。 Polly 需要[Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet 套件。

### <a name="handle-transient-faults"></a>處理暫時性錯誤

當外部 HTTP 呼叫是暫時性的時，通常會發生錯誤。 <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> 允許定義原則來處理暫時性錯誤。 以 `AddTransientHttpErrorPolicy` 設定的原則會處理下列回應：

* <xref:System.Net.Http.HttpRequestException>
* HTTP 5xx
* HTTP 408

`AddTransientHttpErrorPolicy` 提供 `PolicyBuilder` 物件的存取權，其設定為處理代表可能暫時性錯誤的錯誤：

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。 失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。

### <a name="dynamically-select-policies"></a>動態選取原則

提供擴充方法來加入以 Polly 為基礎的處理常式，例如 <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>。 下列 `AddPolicyHandler` 多載會檢查要求以決定要套用的原則：

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

在上述程式碼中，如果外寄要求是 HTTP GET，就會套用 10 秒逾時。 任何其他 HTTP 方法會使用 30 秒逾時。

### <a name="add-multiple-polly-handlers"></a>新增多個 Polly 處理常式

通常會將 Polly 原則加以嵌套：

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

在上述範例中：

* 新增了兩個處理常式。
* 第一個處理常式會使用 <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> 來新增重試原則。 失敗的要求會重試最多三次。
* 第二個 `AddTransientHttpErrorPolicy` 呼叫會增加斷路器原則。 如果連續5次嘗試失敗，則會封鎖進一步的外部要求30秒。 斷路器原則可設定狀態。 透過此用戶端的所有呼叫都會共用相同的線路狀態。

### <a name="add-policies-from-the-polly-registry"></a>從 Polly 登錄新增原則

管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。

在下列程式碼中：

* 系統會新增「一般」和「長」原則。
* <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*> 從登錄新增「一般」和「長」原則。

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

如需 `IHttpClientFactory` 和 Polly 整合的詳細資訊，請參閱[Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory)。

## <a name="httpclient-and-lifetime-management"></a>HttpClient 和存留期管理

每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回新的 `HttpClient` 執行個體。 系統會根據每個命名的用戶端來建立 <xref:System.Net.Http.HttpMessageHandler>。 處理站會管理 `HttpMessageHandler` 執行個體的存留期。

`IHttpClientFactory` 會將處理站所建立的 `HttpMessageHandler` 執行個體放入集區以減少資源耗用量。 建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。

將處理常式放入集區非常實用，因為處理常式通常會管理自己專屬的底層 HTTP 連線。 建立比所需數目更多的處理常式，可能會導致連線延遲。 有些處理常式也會保持連線無限期地開啟，這可能導致處理常式無法回應 DNS （網域名稱系統）變更。

預設處理常式存留時間為兩分鐘。 預設值可以根據每個命名的用戶端來覆寫：

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

`HttpClient` 實例通常可視為**不**需要處置的 .net 物件。 處置會取消傳出的要求，並保證指定的 `HttpClient` 執行個體在呼叫 <xref:System.IDisposable.Dispose*> 之後無法使用。 `IHttpClientFactory` 會追蹤並處置 `HttpClient` 執行個體使用的資源。

在開始使用 `IHttpClientFactory` 之前，讓單一 `HttpClient` 執行個體維持一段較長的時間，是很常使用的模式。 在移轉到 `IHttpClientFactory` 之後，就不再需要此模式。

### <a name="alternatives-to-ihttpclientfactory"></a>IHttpClientFactory 的替代方案

在啟用 DI 的應用程式中使用 `IHttpClientFactory` 可避免：

* 藉由共用 `HttpMessageHandler` 實例的資源耗盡問題。
* 以固定間隔迴圈 `HttpMessageHandler` 實例的過時 DNS 問題。

有其他方法可以使用長期 <xref:System.Net.Http.SocketsHttpHandler> 實例來解決上述問題。

- 當應用程式啟動時，建立 `SocketsHttpHandler` 的實例，並在應用程式的生命週期中使用它。
- 根據 DNS 重新整理時間，將 <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> 設定為適當的值。
- 視需要使用 `new HttpClient(handler, disposeHandler: false)` 建立 `HttpClient` 實例。

上述方法可解決 `IHttpClientFactory` 以類似的方式解決的資源管理問題。

- `SocketsHttpHandler` 會共用 `HttpClient` 實例間的連接。 此共用可防止通訊端耗盡。
- `SocketsHttpHandler` 會根據 `PooledConnectionLifetime` 迴圈連接，以避免過時的 DNS 問題。

### <a name="cookies"></a>Cookies

集區 `HttpMessageHandler` 實例會導致共用 `CookieContainer` 物件。 意外的 `CookieContainer` 物件共用通常會導致不正確的程式碼。 針對需要 cookie 的應用程式，請考慮下列其中一項：

 - 停用自動 cookie 處理
 - 避免 `IHttpClientFactory`

呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 以停用自動的 cookie 處理：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a>記錄

透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。 在記錄設定中啟用適當的資訊層級，以查看預設記錄檔訊息。 額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。

用於每個用戶端的記錄檔分類包含用戶端的名稱。 例如，名為*MyNamedClient*的用戶端會記錄類別為 "HttpClient" 的訊息。**MyNamedClient**。LogicalHandler "。 後面加上 *LogicalHandler* 的訊息發生在要求處理常式管線之外。 在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。 在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。

記錄也會發生在要求處理常式管線之內。 在*MyNamedClient*範例中，這些訊息會記錄在記錄檔類別中為 "HttpClient"。**MyNamedClient**。ClientHandler". 對於要求，這會在所有其他處理常式都執行之後，且在傳送要求之前立即發生。 在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。

在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。 這可能包括要求標頭的變更或回應狀態碼。

在記錄類別中包含用戶端的名稱，可以針對特定的已命名用戶端進行記錄篩選。

## <a name="configure-the-httpmessagehandler"></a>設定 HttpMessageHandler

可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。

新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 擴充方法可以用來定義委派。 委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a>在主控台應用程式中使用 IHttpClientFactory

在主控台應用程式中，將下列套件參考新增至專案：

* [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http)

在下列範例中：

* <xref:System.Net.Http.IHttpClientFactory> 已在[泛型主機的](xref:fundamentals/host/generic-host)服務容器中註冊。
* `MyService` 會從服務建立用戶端 Factory 執行個體，其可用來建立 `HttpClient`。 `HttpClient` 會用來擷取網頁。
* `Main` 會建立範圍來執行服務的 `GetPage` 方法，並將網頁內容的前 500 個字元寫入至主控台。

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a>其他資源

* [使用 HttpClientFactory 實作復原 HTTP 要求](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [使用 HttpClientFactory 和 Polly 原則以指數輪詢實作 HTTP 呼叫重試](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [實作斷路器模式](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)
* [如何在 .NET 中序列化和還原序列化 JSON](/dotnet/standard/serialization/system-text-json-how-to)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

作者：[Glenn Condron](https://github.com/glennc)、[Ryan Nowak](https://github.com/rynowak) 和 [Steve Gordon](https://github.com/stevejgordon)

<xref:System.Net.Http.IHttpClientFactory> 可以註冊及用來在應用程式中設定和建立 <xref:System.Net.Http.HttpClient> 執行個體。 它提供下列優點：

* 提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。 例如，可註冊及設定 *github* 用戶端，來存取 [GitHub](https://github.com/)。 預設用戶端可以註冊用於其他用途。
* 透過委派 `HttpClient` 中的處理常式來撰寫外寄中介軟體的概念，並提供延伸模組以便 Polly 架構中介軟體利用外寄中介軟體。
* 管理基礎 `HttpClientMessageHandler` 執行個體的共用和存留期，以避免在手動管理 `HttpClient` 存留期時，發生的常見 DNS 問題。
* 針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="consumption-patterns"></a>耗用模式

有數種方式可將 `IHttpClientFactory` 用於應用程式：

* [基本使用方式](#basic-usage)
* [具名用戶端](#named-clients)
* [具型別用戶端](#typed-clients)
* [產生的用戶端](#generated-clients)

它們全都不會嚴格優先於另一個。 最好的方法取決於應用程式的條件約束。

### <a name="basic-usage"></a>基本使用方式

`IHttpClientFactory` 可以藉由在 `Startup.ConfigureServices` 方法內的 `IServiceCollection` 上呼叫 `AddHttpClient` 擴充方法來註冊。

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

註冊之後，程式碼可以在可使用[相依性插入 (DI)](xref:fundamentals/dependency-injection) 插入服務的任何位置，接受 `IHttpClientFactory`。 `IHttpClientFactory` 可以用來建立 `HttpClient` 實例：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

以這種方式使用 `IHttpClientFactory` 是重構現有應用程式的好方法。 它對 `HttpClient` 的使用方式沒有任何影響。 在目前建立 `HttpClient` 執行個體的位置，將那些項目取代為呼叫 <xref:System.Net.Http.IHttpClientFactory.CreateClient*>。

### <a name="named-clients"></a>具名用戶端

如果應用程式需要使用多個不同的 `HttpClient`，且每個都有不同的設定，可以選擇使用**具名用戶端**。 具名 `HttpClient` 的組態可以在 `Startup.ConfigureServices` 中註冊時指定。

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

在上述程式碼中，會呼叫 `AddHttpClient` 並提供名稱 *github*。 此用戶端已套用一些預設組態&mdash;即使用 GitHub API 所需的基底位址和兩個標頭。

每次呼叫 `CreateClient` 時，會建立 `HttpClient` 的新執行個體並呼叫組態動作。

若要使用具名用戶端，可以傳遞字串參數至 `CreateClient`。 指定要建立之用戶端的名稱：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

在上述程式碼中，要求不需要指定主機名稱。 它可以只傳遞路徑，因為已使用為用戶端設定的基底位址。

### <a name="typed-clients"></a>具型別用戶端

具型別用戶端：

* 提供與具名用戶端相同的功能，而不需使用字串作為索引鍵。
* 取用用戶端時提供 IntelliSense 和編譯器說明。
* 提供單一位置來設定特定的 `HttpClient` 並與其互動。 例如，單一的具型別用戶端可能用於單一的後端端點，並封裝處理該端點的所有邏輯。
* 使用 DI 且可在應用程式中需要之處插入。

具型別用戶端會接受其程式化中的 `HttpClient` 參數：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

在上述程式碼中，組態會移到具型別用戶端。 `HttpClient` 物件會公開為公用屬性。 您可定義 API 特定的方法，其公開 `HttpClient` 功能。 `GetAspNetDocsIssues` 方法會封裝從 GitHub 存放庫查詢和剖析最新開啟問題所需的程式碼。

若要註冊具型別用戶端，泛型 <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> 擴充方法可用於 `Startup.ConfigureServices` 內，並指定具型別用戶端類別：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

具型別用戶端會向 DI 註冊為暫時性。 具型別用戶端可以直接插入並使用：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

想要的話，具型別用戶端的組態可以在 `Startup.ConfigureServices` 中註冊時指定，而不是在具型別用戶端的建構函式中：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

可以將 `HttpClient` 完全封裝在具型別用戶端內。 可以提供在內部呼叫 `HttpClient` 執行個體的公用方法，而不將它公開為屬性。

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

在上述程式碼，`HttpClient` 儲存為私用欄位。 進行外部呼叫的所有存取都會經歷 `GetRepos` 方法。

### <a name="generated-clients"></a>產生的用戶端

`IHttpClientFactory` 可和其他協力廠商程式庫一起使用，例如 [Refit](https://github.com/paulcbetts/refit)。 Refit 是適用於 .NET 的 REST 程式庫。 它將 REST API 轉換為即時介面。 介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。

定義介面及回覆來代表外部 API 和其回應：

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

可以新增具型別用戶端，使用 Refit 產生實作：

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

定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：

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

## <a name="outgoing-request-middleware"></a>外寄要求中介軟體

`HttpClient` 已經有委派可針對外寄 HTTP 要求連結在一起的處理常式的概念。 `IHttpClientFactory` 可讓您輕鬆地定義要套用於每個具名用戶端的處理常式。 它支援註冊和鏈結多個處理常式，以建置外寄要求中介軟體管線。 這些處理常式每個都可以在外寄要求之前和之後執行工作。 此模式與 ASP.NET Core 中的輸入中介軟體管線相似。 模式提供一個機制來管理 HTTP 要求的跨領域關注，包括快取、錯誤處理、序列化和記錄。

若要建立處理常式，請定義衍生自 <xref:System.Net.Http.DelegatingHandler> 的類別。 覆寫 `SendAsync` 方法，以在將要求傳遞至管線中的下一個處理常式之前執行程式碼：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

上述程式碼定義一個基本處理常式。 它會檢查以查看要求上是否已包含 `X-API-KEY` 標頭。 如果遺漏標頭，它可以避免 HTTP 呼叫，並傳回適當的回應。

在註冊期間，可以將一或多個處理常式新增至 `HttpClient`的設定。 這項工作是透過 <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder> 上的擴充方法完成。

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。 `IHttpClientFactory` 會為每個處理常式建立個別的 DI 範圍。 處理常式可相依於任何範圍的服務。 處置處理常式時，會處置處理常式所相依的服務。

註冊之後，便可以呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>，並傳入處理常式的類型。

可以遵循應該執行的順序來註冊多個處理常式。 每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

使用下列其中一種方式來與訊息處理常式共用個別要求狀態：

* 使用 `HttpRequestMessage.Properties` 將資料傳遞到處理常式。
* 使用 `IHttpContextAccessor` 來存取目前的要求。
* 建立自訂 `AsyncLocal` 儲存體物件以傳遞資料。

## <a name="use-polly-based-handlers"></a>使用 Polly 為基礎的處理常式

`IHttpClientFactory` 整合受歡迎的協力廠商程式庫，稱為 [Polly](https://github.com/App-vNext/Polly)。 Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。 它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。

提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。 Polly 延伸模組：

* 支援將以 Polly 為基礎的處理常式新增至用戶端。
* 可在安裝 [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet 套件後使用。 該套件並未包含在 ASP.NET Core 共用架構中。

### <a name="handle-transient-faults"></a>處理暫時性錯誤

大部分的錯誤發生在外部 HTTP 呼叫是暫時性的時候。 包含一個便利的擴充方法，稱為 `AddTransientHttpErrorPolicy`，它可允許定義原則來處理暫時性錯誤。 使用此延伸模組方法設定的原則，會處理 `HttpRequestException`、HTTP 5xx 回應和 HTTP 408 回應。

`AddTransientHttpErrorPolicy` 延伸模組可用於 `Startup.ConfigureServices` 內。 延伸模組能提供 `PolicyBuilder` 物件的存取，該物件已設定來處理代表可能暫時性錯誤的錯誤：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。 失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。

### <a name="dynamically-select-policies"></a>動態選取原則

有額外的擴充方法可用來新增 Polly 為基礎的處理常式。 其中一個這類延伸模組是 `AddPolicyHandler`，它有多個多載。 一個多載可讓您在定義要套用的原則時，檢查要求：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

在上述程式碼中，如果外寄要求是 HTTP GET，就會套用 10 秒逾時。 任何其他 HTTP 方法會使用 30 秒逾時。

### <a name="add-multiple-polly-handlers"></a>新增多個 Polly 處理常式

通常會建立巢狀 Polly 原則，以提供增強的功能：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

在上述範例中，會新增兩個處理常式。 第一個使用 `AddTransientHttpErrorPolicy` 延伸模組來新增重試原則。 失敗的要求會重試最多三次。 第二個 `AddTransientHttpErrorPolicy` 呼叫會新增斷路器原則。 如果循序發生五次失敗的嘗試，進一步的外部要求會遭到封鎖 30 秒。 斷路器原則可設定狀態。 透過此用戶端的所有呼叫都會共用相同的線路狀態。

### <a name="add-policies-from-the-polly-registry"></a>從 Polly 登錄新增原則

管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。 提供了擴充方法，可以使用來自登錄的原則新增處理常式：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

在上述程式碼中，當 `PolicyRegistry` 新增至 `ServiceCollection` 時，註冊了兩個原則。 為了使用來自登錄的原則，使用了 `AddPolicyHandlerFromRegistry` 方法，並傳遞要套用的原則名稱。

關於 `IHttpClientFactory` Polly 整合的詳細資訊，可以在 [Polly Wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory) 上找到。

## <a name="httpclient-and-lifetime-management"></a>HttpClient 和存留期管理

每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回新的 `HttpClient` 執行個體。 每個具名用戶端都有一個 <xref:System.Net.Http.HttpMessageHandler>。 處理站會管理 `HttpMessageHandler` 執行個體的存留期。

`IHttpClientFactory` 會將處理站所建立的 `HttpMessageHandler` 執行個體放入集區以減少資源耗用量。 建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。

將處理常式放入集區非常實用，因為處理常式通常會管理自己專屬的底層 HTTP 連線。 建立比所需數目更多的處理常式，可能會導致連線延遲。 有些處理常式也會保持連線無限期地開啟，這可能導致處理常式無法對 DNS 變更回應。

預設處理常式存留時間為兩分鐘。 可以針對每個具名用戶端覆寫預設值。 若要覆寫它，請在建立用戶端時所傳回的 `IHttpClientBuilder` 上呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*>：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

不需要處置用戶端。 處置會取消傳出的要求，並保證指定的 `HttpClient` 執行個體在呼叫 <xref:System.IDisposable.Dispose*> 之後無法使用。 `IHttpClientFactory` 會追蹤並處置 `HttpClient` 執行個體使用的資源。 `HttpClient` 執行個體通常可視為 .NET 物件，不需要處置。

在開始使用 `IHttpClientFactory` 之前，讓單一 `HttpClient` 執行個體維持一段較長的時間，是很常使用的模式。 在移轉到 `IHttpClientFactory` 之後，就不再需要此模式。

### <a name="alternatives-to-ihttpclientfactory"></a>IHttpClientFactory 的替代方案

在啟用 DI 的應用程式中使用 `IHttpClientFactory` 可避免：

* 藉由共用 `HttpMessageHandler` 實例的資源耗盡問題。
* 以固定間隔迴圈 `HttpMessageHandler` 實例的過時 DNS 問題。

有其他方法可以使用長期 <xref:System.Net.Http.SocketsHttpHandler> 實例來解決上述問題。

- 當應用程式啟動時，建立 `SocketsHttpHandler` 的實例，並在應用程式的生命週期中使用它。
- 根據 DNS 重新整理時間，將 <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> 設定為適當的值。
- 視需要使用 `new HttpClient(handler, disposeHandler: false)` 建立 `HttpClient` 實例。

上述方法可解決 `IHttpClientFactory` 以類似的方式解決的資源管理問題。

- `SocketsHttpHandler` 會共用 `HttpClient` 實例間的連接。 此共用可防止通訊端耗盡。
- `SocketsHttpHandler` 會根據 `PooledConnectionLifetime` 迴圈連接，以避免過時的 DNS 問題。

### <a name="cookies"></a>Cookies

集區 `HttpMessageHandler` 實例會導致共用 `CookieContainer` 物件。 意外的 `CookieContainer` 物件共用通常會導致不正確的程式碼。 針對需要 cookie 的應用程式，請考慮下列其中一項：

 - 停用自動 cookie 處理
 - 避免 `IHttpClientFactory`

呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 以停用自動的 cookie 處理：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a>記錄

透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。 在記錄設定中啟用適當的資訊層級，以查看預設記錄檔訊息。 額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。

用於每個用戶端的記錄檔分類包含用戶端的名稱。 例如，名為 *MyNamedClient* 的用戶端會記錄分類為 `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler` 的訊息。 後面加上 *LogicalHandler* 的訊息發生在要求處理常式管線之外。 在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。 在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。

記錄也會發生在要求處理常式管線之內。 在 *MyNamedClient* 範例中，那些訊息是針對記錄檔分類 `System.Net.Http.HttpClient.MyNamedClient.ClientHandler` 而記錄。 對於要求，這是發生在所有其他處理常式都已執行之後，並且緊接在網路上傳送要求之前。 在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。

在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。 例如，這可能包括要求標頭的變更，或是回應狀態碼的變更。

在記錄分類中包含用戶端的名稱，可讓您在需要時進行特定具名用戶端的記錄檔篩選。

## <a name="configure-the-httpmessagehandler"></a>設定 HttpMessageHandler

可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。

新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 擴充方法可以用來定義委派。 委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a>在主控台應用程式中使用 IHttpClientFactory

在主控台應用程式中，將下列套件參考新增至專案：

* [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http)

在下列範例中：

* <xref:System.Net.Http.IHttpClientFactory> 已在[泛型主機的](xref:fundamentals/host/generic-host)服務容器中註冊。
* `MyService` 會從服務建立用戶端 Factory 執行個體，其可用來建立 `HttpClient`。 `HttpClient` 會用來擷取網頁。
* `Main` 會建立範圍來執行服務的 `GetPage` 方法，並將網頁內容的前 500 個字元寫入至主控台。

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a>其他資源

* [使用 HttpClientFactory 實作復原 HTTP 要求](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [使用 HttpClientFactory 和 Polly 原則以指數輪詢實作 HTTP 呼叫重試](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [實作斷路器模式](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.1"

作者：[Glenn Condron](https://github.com/glennc)、[Ryan Nowak](https://github.com/rynowak) 和 [Steve Gordon](https://github.com/stevejgordon)

<xref:System.Net.Http.IHttpClientFactory> 可以註冊及用來在應用程式中設定和建立 <xref:System.Net.Http.HttpClient> 執行個體。 它提供下列優點：

* 提供一個集中位置以便命名和設定邏輯 `HttpClient` 執行個體。 例如，可註冊及設定 *github* 用戶端，來存取 [GitHub](https://github.com/)。 預設用戶端可以註冊用於其他用途。
* 透過委派 `HttpClient` 中的處理常式來撰寫外寄中介軟體的概念，並提供延伸模組以便 Polly 架構中介軟體利用外寄中介軟體。
* 管理基礎 `HttpClientMessageHandler` 執行個體的共用和存留期，以避免在手動管理 `HttpClient` 存留期時，發生的常見 DNS 問題。
* 針對透過處理站所建立之用戶端傳送的所有要求，新增可設定的記錄體驗 (透過 `ILogger`)。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>必要條件：

以 .NET Framework 為目標的專案，需要安裝 [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet 套件。 以 .NET Core 為目標且參考 [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) 的專案，已包含 `Microsoft.Extensions.Http` 套件。

## <a name="consumption-patterns"></a>耗用模式

有數種方式可將 `IHttpClientFactory` 用於應用程式：

* [基本使用方式](#basic-usage)
* [具名用戶端](#named-clients)
* [具型別用戶端](#typed-clients)
* [產生的用戶端](#generated-clients)

它們全都不會嚴格優先於另一個。 最好的方法取決於應用程式的條件約束。

### <a name="basic-usage"></a>基本使用方式

`IHttpClientFactory` 可以藉由在 `Startup.ConfigureServices` 方法內的 `IServiceCollection` 上呼叫 `AddHttpClient` 擴充方法來註冊。

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

註冊之後，程式碼可以在可使用[相依性插入 (DI)](xref:fundamentals/dependency-injection) 插入服務的任何位置，接受 `IHttpClientFactory`。 `IHttpClientFactory` 可以用來建立 `HttpClient` 實例：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

以這種方式使用 `IHttpClientFactory` 是重構現有應用程式的好方法。 它對 `HttpClient` 的使用方式沒有任何影響。 在目前建立 `HttpClient` 執行個體的位置，將那些項目取代為呼叫 <xref:System.Net.Http.IHttpClientFactory.CreateClient*>。

### <a name="named-clients"></a>具名用戶端

如果應用程式需要使用多個不同的 `HttpClient`，且每個都有不同的設定，可以選擇使用**具名用戶端**。 具名 `HttpClient` 的組態可以在 `Startup.ConfigureServices` 中註冊時指定。

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

在上述程式碼中，會呼叫 `AddHttpClient` 並提供名稱 *github*。 此用戶端已套用一些預設組態&mdash;即使用 GitHub API 所需的基底位址和兩個標頭。

每次呼叫 `CreateClient` 時，會建立 `HttpClient` 的新執行個體並呼叫組態動作。

若要使用具名用戶端，可以傳遞字串參數至 `CreateClient`。 指定要建立之用戶端的名稱：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

在上述程式碼中，要求不需要指定主機名稱。 它可以只傳遞路徑，因為已使用為用戶端設定的基底位址。

### <a name="typed-clients"></a>具型別用戶端

具型別用戶端：

* 提供與具名用戶端相同的功能，而不需使用字串作為索引鍵。
* 取用用戶端時提供 IntelliSense 和編譯器說明。
* 提供單一位置來設定特定的 `HttpClient` 並與其互動。 例如，單一的具型別用戶端可能用於單一的後端端點，並封裝處理該端點的所有邏輯。
* 使用 DI 且可在應用程式中需要之處插入。

具型別用戶端會接受其程式化中的 `HttpClient` 參數：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

在上述程式碼中，組態會移到具型別用戶端。 `HttpClient` 物件會公開為公用屬性。 您可定義 API 特定的方法，其公開 `HttpClient` 功能。 `GetAspNetDocsIssues` 方法會封裝從 GitHub 存放庫查詢和剖析最新開啟問題所需的程式碼。

若要註冊具型別用戶端，泛型 <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> 擴充方法可用於 `Startup.ConfigureServices` 內，並指定具型別用戶端類別：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

具型別用戶端會向 DI 註冊為暫時性。 具型別用戶端可以直接插入並使用：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

想要的話，具型別用戶端的組態可以在 `Startup.ConfigureServices` 中註冊時指定，而不是在具型別用戶端的建構函式中：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

可以將 `HttpClient` 完全封裝在具型別用戶端內。 可以提供在內部呼叫 `HttpClient` 執行個體的公用方法，而不將它公開為屬性。

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

在上述程式碼，`HttpClient` 儲存為私用欄位。 進行外部呼叫的所有存取都會經歷 `GetRepos` 方法。

### <a name="generated-clients"></a>產生的用戶端

`IHttpClientFactory` 可和其他協力廠商程式庫一起使用，例如 [Refit](https://github.com/paulcbetts/refit)。 Refit 是適用於 .NET 的 REST 程式庫。 它將 REST API 轉換為即時介面。 介面的實作由 `RestService` 動態產生，並使用 `HttpClient` 進行外部 HTTP 呼叫。

定義介面及回覆來代表外部 API 和其回應：

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

可以新增具型別用戶端，使用 Refit 產生實作：

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

定義的介面可在需要時使用，並搭配 DI 與 Refit 所提供的實作：

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

## <a name="outgoing-request-middleware"></a>外寄要求中介軟體

`HttpClient` 已經有委派可針對外寄 HTTP 要求連結在一起的處理常式的概念。 `IHttpClientFactory` 可讓您輕鬆地定義要套用於每個具名用戶端的處理常式。 它支援註冊和鏈結多個處理常式，以建置外寄要求中介軟體管線。 這些處理常式每個都可以在外寄要求之前和之後執行工作。 此模式與 ASP.NET Core 中的輸入中介軟體管線相似。 模式提供一個機制來管理 HTTP 要求的跨領域關注，包括快取、錯誤處理、序列化和記錄。

若要建立處理常式，請定義衍生自 <xref:System.Net.Http.DelegatingHandler> 的類別。 覆寫 `SendAsync` 方法，以在將要求傳遞至管線中的下一個處理常式之前執行程式碼：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

上述程式碼定義一個基本處理常式。 它會檢查以查看要求上是否已包含 `X-API-KEY` 標頭。 如果遺漏標頭，它可以避免 HTTP 呼叫，並傳回適當的回應。

在註冊期間，可以將一或多個處理常式新增至 `HttpClient`的設定。 這項工作是透過 <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder> 上的擴充方法完成。

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

在上述程式碼，`ValidateHeaderHandler` 已向 DI 註冊。 處理常式**必須**在 DI 中註冊為暫時性服務，無限定範圍。 如果處理常式已註冊為範圍服務，而且處理常式所相依的任何服務都是可處置的：

* 處理常式的服務可能會在處理常式超出範圍之前加以處置。
* 已處置的處理常式服務會導致處理常式失敗。

註冊之後，便可以呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>，並傳入處理常式類型。

可以遵循應該執行的順序來註冊多個處理常式。 每個處理常式會包裝下一個處理常式，直到最終 `HttpClientHandler` 執行要求：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

使用下列其中一種方式來與訊息處理常式共用個別要求狀態：

* 使用 `HttpRequestMessage.Properties` 將資料傳遞到處理常式。
* 使用 `IHttpContextAccessor` 來存取目前的要求。
* 建立自訂 `AsyncLocal` 儲存體物件以傳遞資料。

## <a name="use-polly-based-handlers"></a>使用 Polly 為基礎的處理常式

`IHttpClientFactory` 整合受歡迎的協力廠商程式庫，稱為 [Polly](https://github.com/App-vNext/Polly)。 Polly 是適用於 .NET 的完整恢復功能和暫時性錯誤處理程式庫。 它可讓開發人員以流暢且執行緒安全的方式表達原則，例如重試、斷路器、逾時、艙隔離與後援。

提供擴充方法來啟用使用 Polly 原則搭配設定的 `HttpClient` 執行個體。 Polly 延伸模組：

* 支援將以 Polly 為基礎的處理常式新增至用戶端。
* 可在安裝 [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet 套件後使用。 該套件並未包含在 ASP.NET Core 共用架構中。

### <a name="handle-transient-faults"></a>處理暫時性錯誤

大部分的錯誤發生在外部 HTTP 呼叫是暫時性的時候。 包含一個便利的擴充方法，稱為 `AddTransientHttpErrorPolicy`，它可允許定義原則來處理暫時性錯誤。 使用此延伸模組方法設定的原則，會處理 `HttpRequestException`、HTTP 5xx 回應和 HTTP 408 回應。

`AddTransientHttpErrorPolicy` 延伸模組可用於 `Startup.ConfigureServices` 內。 延伸模組能提供 `PolicyBuilder` 物件的存取，該物件已設定來處理代表可能暫時性錯誤的錯誤：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

在上述程式碼，已定義了 `WaitAndRetryAsync` 原則。 失敗的要求會重試最多三次，並且在嘗試之間會有 600 毫秒的延遲時間。

### <a name="dynamically-select-policies"></a>動態選取原則

有額外的擴充方法可用來新增 Polly 為基礎的處理常式。 其中一個這類延伸模組是 `AddPolicyHandler`，它有多個多載。 一個多載可讓您在定義要套用的原則時，檢查要求：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

在上述程式碼中，如果外寄要求是 HTTP GET，就會套用 10 秒逾時。 任何其他 HTTP 方法會使用 30 秒逾時。

### <a name="add-multiple-polly-handlers"></a>新增多個 Polly 處理常式

通常會建立巢狀 Polly 原則，以提供增強的功能：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

在上述範例中，會新增兩個處理常式。 第一個使用 `AddTransientHttpErrorPolicy` 延伸模組來新增重試原則。 失敗的要求會重試最多三次。 第二個 `AddTransientHttpErrorPolicy` 呼叫會新增斷路器原則。 如果循序發生五次失敗的嘗試，進一步的外部要求會遭到封鎖 30 秒。 斷路器原則可設定狀態。 透過此用戶端的所有呼叫都會共用相同的線路狀態。

### <a name="add-policies-from-the-polly-registry"></a>從 Polly 登錄新增原則

管理定期使用原則的一個方法是定義一次，並向 `PolicyRegistry` 註冊它們。 提供了擴充方法，可以使用來自登錄的原則新增處理常式：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

在上述程式碼中，當 `PolicyRegistry` 新增至 `ServiceCollection` 時，註冊了兩個原則。 為了使用來自登錄的原則，使用了 `AddPolicyHandlerFromRegistry` 方法，並傳遞要套用的原則名稱。

關於 `IHttpClientFactory` Polly 整合的詳細資訊，可以在 [Polly Wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory) 上找到。

## <a name="httpclient-and-lifetime-management"></a>HttpClient 和存留期管理

每次在 `IHttpClientFactory` 上呼叫 `CreateClient` 時，都會傳回新的 `HttpClient` 執行個體。 每個具名用戶端都有一個 <xref:System.Net.Http.HttpMessageHandler>。 處理站會管理 `HttpMessageHandler` 執行個體的存留期。

`IHttpClientFactory` 會將處理站所建立的 `HttpMessageHandler` 執行個體放入集區以減少資源耗用量。 建立新的 `HttpClient` 執行個體時，如果其存留期間尚未過期，`HttpMessageHandler` 執行個體可從集區重複使用。

將處理常式放入集區非常實用，因為處理常式通常會管理自己專屬的底層 HTTP 連線。 建立比所需數目更多的處理常式，可能會導致連線延遲。 有些處理常式也會保持連線無限期地開啟，這可能導致處理常式無法對 DNS 變更回應。

預設處理常式存留時間為兩分鐘。 可以針對每個具名用戶端覆寫預設值。 若要覆寫它，請在建立用戶端時所傳回的 `IHttpClientBuilder` 上呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*>：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

不需要處置用戶端。 處置會取消傳出的要求，並保證指定的 `HttpClient` 執行個體在呼叫 <xref:System.IDisposable.Dispose*> 之後無法使用。 `IHttpClientFactory` 會追蹤並處置 `HttpClient` 執行個體使用的資源。 `HttpClient` 執行個體通常可視為 .NET 物件，不需要處置。

在開始使用 `IHttpClientFactory` 之前，讓單一 `HttpClient` 執行個體維持一段較長的時間，是很常使用的模式。 在移轉到 `IHttpClientFactory` 之後，就不再需要此模式。

### <a name="alternatives-to-ihttpclientfactory"></a>IHttpClientFactory 的替代方案

在啟用 DI 的應用程式中使用 `IHttpClientFactory` 可避免：

* 藉由共用 `HttpMessageHandler` 實例的資源耗盡問題。
* 以固定間隔迴圈 `HttpMessageHandler` 實例的過時 DNS 問題。

有其他方法可以使用長期 <xref:System.Net.Http.SocketsHttpHandler> 實例來解決上述問題。

- 當應用程式啟動時，建立 `SocketsHttpHandler` 的實例，並在應用程式的生命週期中使用它。
- 根據 DNS 重新整理時間，將 <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> 設定為適當的值。
- 視需要使用 `new HttpClient(handler, disposeHandler: false)` 建立 `HttpClient` 實例。

上述方法可解決 `IHttpClientFactory` 以類似的方式解決的資源管理問題。

- `SocketsHttpHandler` 會共用 `HttpClient` 實例間的連接。 此共用可防止通訊端耗盡。
- `SocketsHttpHandler` 會根據 `PooledConnectionLifetime` 迴圈連接，以避免過時的 DNS 問題。

### <a name="cookies"></a>Cookies

集區 `HttpMessageHandler` 實例會導致共用 `CookieContainer` 物件。 意外的 `CookieContainer` 物件共用通常會導致不正確的程式碼。 針對需要 cookie 的應用程式，請考慮下列其中一項：

 - 停用自動 cookie 處理
 - 避免 `IHttpClientFactory`

呼叫 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 以停用自動的 cookie 處理：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a>記錄

透過 `IHttpClientFactory` 建立的用戶端會記錄所有要求的記錄訊息。 在記錄設定中啟用適當的資訊層級，以查看預設記錄檔訊息。 額外的記錄功能，例如要求標頭的記錄，只會包含在追蹤層級。

用於每個用戶端的記錄檔分類包含用戶端的名稱。 例如，名為 *MyNamedClient* 的用戶端會記錄分類為 `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler` 的訊息。 後面加上 *LogicalHandler* 的訊息發生在要求處理常式管線之外。 在要求中，訊息會在管線中任何其他處理常式處理它之前就記錄。 在回應中，訊息會在任何其他管線處理常式收到回應之後記錄。

記錄也會發生在要求處理常式管線之內。 在 *MyNamedClient* 範例中，那些訊息是針對記錄檔分類 `System.Net.Http.HttpClient.MyNamedClient.ClientHandler` 而記錄。 對於要求，這是發生在所有其他處理常式都已執行之後，並且緊接在網路上傳送要求之前。 在回應中，此記錄會包含回應傳回通過處理常式管線之前的狀態。

在管線內外啟用記錄，可讓您檢查其他管線處理常式所做的變更。 例如，這可能包括要求標頭的變更，或是回應狀態碼的變更。

在記錄分類中包含用戶端的名稱，可讓您在需要時進行特定具名用戶端的記錄檔篩選。

## <a name="configure-the-httpmessagehandler"></a>設定 HttpMessageHandler

可能需要控制用戶端使用之內部 `HttpMessageHandler` 的組態。

新增具名或具型別用戶端時，會傳回 `IHttpClientBuilder`。 <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 擴充方法可以用來定義委派。 委派是用來建立及設定該用戶端所使用的主要 `HttpMessageHandler`：

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a>在主控台應用程式中使用 IHttpClientFactory

在主控台應用程式中，將下列套件參考新增至專案：

* [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http)

在下列範例中：

* <xref:System.Net.Http.IHttpClientFactory> 已在[泛型主機的](xref:fundamentals/host/generic-host)服務容器中註冊。
* `MyService` 會從服務建立用戶端 Factory 執行個體，其可用來建立 `HttpClient`。 `HttpClient` 會用來擷取網頁。
* `Main` 會建立範圍來執行服務的 `GetPage` 方法，並將網頁內容的前 500 個字元寫入至主控台。

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a>其他資源

* [使用 HttpClientFactory 實作復原 HTTP 要求](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [使用 HttpClientFactory 和 Polly 原則以指數輪詢實作 HTTP 呼叫重試](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [實作斷路器模式](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
