---
title: 測試 ASP.NET Core 中介軟體
author: tratcher
description: 瞭解如何使用 TestServer 來測試 ASP.NET Core 中介軟體。
ms.author: riande
ms.custom: mvc
ms.date: 5/12/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: test/middleware
ms.openlocfilehash: f4ed16b136da37c093a72a8866301a188a8518a2
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85406481"
---
# <a name="test-aspnet-core-middleware"></a><span data-ttu-id="b4cf9-103">測試 ASP.NET Core 中介軟體</span><span class="sxs-lookup"><span data-stu-id="b4cf9-103">Test ASP.NET Core middleware</span></span>

<span data-ttu-id="b4cf9-104">依[Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="b4cf9-104">By [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="b4cf9-105">中介軟體可以與隔離地進行測試 <xref:Microsoft.AspNetCore.TestHost.TestServer> 。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-105">Middleware can be tested in isolation with <xref:Microsoft.AspNetCore.TestHost.TestServer>.</span></span> <span data-ttu-id="b4cf9-106">它可讓您：</span><span class="sxs-lookup"><span data-stu-id="b4cf9-106">It allows you to:</span></span>

* <span data-ttu-id="b4cf9-107">具現化應用程式管線，其中只包含您需要測試的元件。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-107">Instantiate an app pipeline containing only the components that you need to test.</span></span>
* <span data-ttu-id="b4cf9-108">傳送自訂要求以驗證中介軟體行為。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-108">Send custom requests to verify middleware behavior.</span></span>

<span data-ttu-id="b4cf9-109">優點：</span><span class="sxs-lookup"><span data-stu-id="b4cf9-109">Advantages:</span></span>

* <span data-ttu-id="b4cf9-110">要求會在記憶體中傳送，而不是透過網路進行序列化。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-110">Requests are sent in-memory rather than being serialized over the network.</span></span>
* <span data-ttu-id="b4cf9-111">這可避免其他問題，例如埠管理和 HTTPS 憑證。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-111">This avoids additional concerns, such as port management and HTTPS certificates.</span></span>
* <span data-ttu-id="b4cf9-112">中介軟體中的例外狀況可以直接傳回到呼叫測試。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-112">Exceptions in the middleware can flow directly back to the calling test.</span></span>
* <span data-ttu-id="b4cf9-113">您可以 <xref:Microsoft.AspNetCore.Http.HttpContext> 直接在測試中自訂伺服器資料結構，例如。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-113">It's possible to customize server data structures, such as <xref:Microsoft.AspNetCore.Http.HttpContext>, directly in the test.</span></span>

## <a name="set-up-the-testserver"></a><span data-ttu-id="b4cf9-114">設定 TestServer</span><span class="sxs-lookup"><span data-stu-id="b4cf9-114">Set up the TestServer</span></span>

<span data-ttu-id="b4cf9-115">在測試專案中，建立測試：</span><span class="sxs-lookup"><span data-stu-id="b4cf9-115">In the test project, create a test:</span></span>

* <span data-ttu-id="b4cf9-116">建立並啟動使用的主機 <xref:Microsoft.AspNetCore.TestHost.TestServer> 。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-116">Build and start a host that uses <xref:Microsoft.AspNetCore.TestHost.TestServer>.</span></span>
* <span data-ttu-id="b4cf9-117">新增中介軟體所使用的任何必要服務。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-117">Add any required services that the middleware uses.</span></span>
* <span data-ttu-id="b4cf9-118">將[TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost/) NuGet 套件新增至專案：</span><span class="sxs-lookup"><span data-stu-id="b4cf9-118">Add the [Microsoft.AspNetCore.TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost/) NuGet package to the project:</span></span>
  
  ```dotnetcli
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.TestHost" Version="3.1.*" />
  </ItemGroup>
  ```

* <span data-ttu-id="b4cf9-119">設定處理管線以使用中介軟體進行測試。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-119">Configure the processing pipeline to use the middleware for the test.</span></span>

[!code-csharp[](middleware/samples_snapshot/3.x/setup.cs?highlight=4-18)]

## <a name="send-requests-with-httpclient"></a><span data-ttu-id="b4cf9-120">使用 HttpClient 傳送要求</span><span class="sxs-lookup"><span data-stu-id="b4cf9-120">Send requests with HttpClient</span></span>
<span data-ttu-id="b4cf9-121">使用 <xref:System.Net.Http.HttpClient> 下列內容傳送要求：</span><span class="sxs-lookup"><span data-stu-id="b4cf9-121">Send a request using <xref:System.Net.Http.HttpClient>:</span></span>

[!code-csharp[](middleware/samples_snapshot/3.x/request.cs?highlight=20)]

<span data-ttu-id="b4cf9-122">判斷提示結果。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-122">Assert the result.</span></span> <span data-ttu-id="b4cf9-123">首先，請將判斷提示設為與預期結果相反的判斷提示。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-123">First, make an assertion the opposite of the result that you expect.</span></span> <span data-ttu-id="b4cf9-124">具有錯誤正面判斷提示的初始執行，會確認中介軟體正常執行時測試失敗。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-124">An initial run with a false positive assertion confirms that the test fails when the middleware is performing correctly.</span></span> <span data-ttu-id="b4cf9-125">執行測試並確認測試失敗。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-125">Run the test and confirm that the test fails.</span></span>

<span data-ttu-id="b4cf9-126">在下列範例中，當要求根端點時，中介軟體應該會傳回404狀態碼（找*不到*）。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-126">In the following example, the middleware should return a 404 status code (*Not Found*) when the root endpoint is requested.</span></span> <span data-ttu-id="b4cf9-127">使用執行第一次測試 `Assert.NotEqual( ... );` ，這應該會失敗：</span><span class="sxs-lookup"><span data-stu-id="b4cf9-127">Make the first test run with `Assert.NotEqual( ... );`, which should fail:</span></span>

[!code-csharp[](middleware/samples_snapshot/3.x/false-failure-check.cs?highlight=22)]

<span data-ttu-id="b4cf9-128">變更判斷提示，以在正常操作條件下測試中介軟體。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-128">Change the assertion to test the middleware under normal operating conditions.</span></span> <span data-ttu-id="b4cf9-129">最後的測試會使用 `Assert.Equal( ... );` 。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-129">The final test uses `Assert.Equal( ... );`.</span></span> <span data-ttu-id="b4cf9-130">再次執行測試，以確認它通過。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-130">Run the test again to confirm that it passes.</span></span>

[!code-csharp[](middleware/samples_snapshot/3.x/final-test.cs?highlight=22)]

## <a name="send-requests-with-httpcontext"></a><span data-ttu-id="b4cf9-131">使用 HttpCoNtext 傳送要求</span><span class="sxs-lookup"><span data-stu-id="b4cf9-131">Send requests with HttpContext</span></span>

<span data-ttu-id="b4cf9-132">測試應用程式也可以使用[SendAsync （Action \<HttpContext> ，CancellationToken）](xref:Microsoft.AspNetCore.TestHost.TestServer.SendAsync%2A)傳送要求。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-132">A test app can also send a request using [SendAsync(Action\<HttpContext>, CancellationToken)](xref:Microsoft.AspNetCore.TestHost.TestServer.SendAsync%2A).</span></span> <span data-ttu-id="b4cf9-133">在下列範例中，當中間件處理時，會進行幾項檢查 `https://example.com/A/Path/?and=query` ：</span><span class="sxs-lookup"><span data-stu-id="b4cf9-133">In the following example, several checks are made when `https://example.com/A/Path/?and=query` is processed by the middleware:</span></span>

```csharp
[Fact]
public async Task TestMiddleware_ExpectedResponse()
{
    using var host = await new HostBuilder()
        .ConfigureWebHost(webBuilder =>
        {
            webBuilder
                .UseTestServer()
                .ConfigureServices(services =>
                {
                    services.AddMyServices();
                })
                .Configure(app =>
                {
                    app.UseMiddleware<MyMiddleware>();
                });
        })
        .StartAsync();

    var server = host.GetTestServer();
    server.BaseAddress = new Uri("https://example.com/A/Path/");

    var context = await server.SendAsync(c =>
    {
        c.Request.Method = HttpMethods.Post;
        c.Request.Path = "/and/file.txt";
        c.Request.QueryString = new QueryString("?and=query");
    });

    Assert.True(context.RequestAborted.CanBeCanceled);
    Assert.Equal(HttpProtocol.Http11, context.Request.Protocol);
    Assert.Equal("POST", context.Request.Method);
    Assert.Equal("https", context.Request.Scheme);
    Assert.Equal("example.com", context.Request.Host.Value);
    Assert.Equal("/A/Path", context.Request.PathBase.Value);
    Assert.Equal("/and/file.txt", context.Request.Path.Value);
    Assert.Equal("?and=query", context.Request.QueryString.Value);
    Assert.NotNull(context.Request.Body);
    Assert.NotNull(context.Request.Headers);
    Assert.NotNull(context.Response.Headers);
    Assert.NotNull(context.Response.Body);
    Assert.Equal(404, context.Response.StatusCode);
    Assert.Null(context.Features.Get<IHttpResponseFeature>().ReasonPhrase);
}
```

<span data-ttu-id="b4cf9-134"><xref:Microsoft.AspNetCore.TestHost.TestServer.SendAsync%2A>允許直接設定物件， <xref:Microsoft.AspNetCore.Http.HttpContext> 而不是使用 <xref:System.Net.Http.HttpClient> 抽象概念。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-134"><xref:Microsoft.AspNetCore.TestHost.TestServer.SendAsync%2A> permits direct configuration of an <xref:Microsoft.AspNetCore.Http.HttpContext> object rather than using the <xref:System.Net.Http.HttpClient> abstractions.</span></span> <span data-ttu-id="b4cf9-135">使用 <xref:Microsoft.AspNetCore.TestHost.TestServer.SendAsync%2A> 來操作只能在伺服器上使用的結構，例如[HttpcoNtext](xref:Microsoft.AspNetCore.Http.HttpContext.Items)或[HTTPcoNtext 功能](xref:Microsoft.AspNetCore.Http.HttpContext.Features)。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-135">Use <xref:Microsoft.AspNetCore.TestHost.TestServer.SendAsync%2A> to manipulate structures only available on the server, such as [HttpContext.Items](xref:Microsoft.AspNetCore.Http.HttpContext.Items) or [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features).</span></span>

<span data-ttu-id="b4cf9-136">如同先前測試過*404-找不*到回應的範例，請檢查 `Assert` 前述測試中每個語句的相反。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-136">As with the earlier example that tested for a *404 - Not Found* response, check the opposite for each `Assert` statement in the preceding test.</span></span> <span data-ttu-id="b4cf9-137">當中間件正常運作時，此檢查會確認測試是否正確。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-137">The check confirms that the test fails correctly when the middleware is operating normally.</span></span> <span data-ttu-id="b4cf9-138">當您確認錯誤正面測試正常運作之後，請 `Assert` 針對測試的預期條件和值設定最終語句。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-138">After you've confirmed that the false positive test works, set the final `Assert` statements for the expected conditions and values of the test.</span></span> <span data-ttu-id="b4cf9-139">再次執行，以確認測試通過。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-139">Run it again to confirm that the test passes.</span></span>

## <a name="testserver-limitations"></a><span data-ttu-id="b4cf9-140">TestServer 限制</span><span class="sxs-lookup"><span data-stu-id="b4cf9-140">TestServer limitations</span></span>

<span data-ttu-id="b4cf9-141">TestServer</span><span class="sxs-lookup"><span data-stu-id="b4cf9-141">TestServer:</span></span>

* <span data-ttu-id="b4cf9-142">已建立來將伺服器行為複寫至測試中介軟體。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-142">Was created to replicate server behaviors to test middleware.</span></span>
* <span data-ttu-id="b4cf9-143">不***會嘗試複寫***所有 <xref:System.Net.Http.HttpClient> 行為。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-143">Does ***not*** try to replicate all <xref:System.Net.Http.HttpClient> behaviors.</span></span>
* <span data-ttu-id="b4cf9-144">會嘗試讓用戶端對伺服器的存取權限盡可能地控制，而且能夠更瞭解伺服器上的情況。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-144">Attempts to give the client access to as much control over the server as possible, and with as much visibility into what's happening on the server as possible.</span></span> <span data-ttu-id="b4cf9-145">例如，它可能會擲回通常不會擲回的例外 `HttpClient` 狀況，以便直接溝通伺服器狀態。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-145">For example it may throw exceptions not normally thrown by `HttpClient` in order to directly communicate server state.</span></span>
* <span data-ttu-id="b4cf9-146">預設不會設定某些傳輸特定標頭，因為它們通常不會與中介軟體相關。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-146">Doesn't set some transport specific headers by default as those are not usually relevant to middleware.</span></span> <span data-ttu-id="b4cf9-147">如需詳細資訊，請參閱下一節。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-147">For more information, see the next section.</span></span>

### <a name="content-length-and-transfer-encoding-headers"></a><span data-ttu-id="b4cf9-148">內容長度和傳輸-編碼標頭</span><span class="sxs-lookup"><span data-stu-id="b4cf9-148">Content-Length and Transfer-Encoding headers</span></span>

<span data-ttu-id="b4cf9-149">TestServer 不***會設定傳輸***相關的要求或回應標頭，例如[內容長度](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Length)或[傳輸編碼](https://developer.mozilla.org/docs/Web/HTTP/Headers/Transfer-Encoding)。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-149">TestServer does ***not*** set transport related request or response headers such as [Content-Length](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Length) or [Transfer-Encoding](https://developer.mozilla.org/docs/Web/HTTP/Headers/Transfer-Encoding).</span></span> <span data-ttu-id="b4cf9-150">應用程式應該根據這些標頭來避免，因為它們的使用方式會因用戶端、案例和通訊協定而異。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-150">Applications should avoid depending on these headers because their usage varies by client, scenario, and protocol.</span></span> <span data-ttu-id="b4cf9-151">如果 `Content-Length` 和 `Transfer-Encoding` 是測試特定案例的必要項，則在撰寫或時，可以在測試中 <xref:System.Net.Http.HttpRequestMessage> 指定 <xref:Microsoft.AspNetCore.Http.HttpContext> 。</span><span class="sxs-lookup"><span data-stu-id="b4cf9-151">If `Content-Length` and `Transfer-Encoding` are necessary to test a specific scenario, they can be specified in the test when composing the <xref:System.Net.Http.HttpRequestMessage> or <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span> <span data-ttu-id="b4cf9-152">如需詳細資訊，請參閱下列 GitHub 問題：</span><span class="sxs-lookup"><span data-stu-id="b4cf9-152">For more information, see the following GitHub issues:</span></span>

* [<span data-ttu-id="b4cf9-153">dotnet/aspnetcore # 21677</span><span class="sxs-lookup"><span data-stu-id="b4cf9-153">dotnet/aspnetcore#21677</span></span>](https://github.com/dotnet/aspnetcore/issues/21677)
* [<span data-ttu-id="b4cf9-154">dotnet/aspnetcore # 18463</span><span class="sxs-lookup"><span data-stu-id="b4cf9-154">dotnet/aspnetcore#18463</span></span>](https://github.com/dotnet/aspnetcore/issues/18463)
* [<span data-ttu-id="b4cf9-155">dotnet/aspnetcore # 13273</span><span class="sxs-lookup"><span data-stu-id="b4cf9-155">dotnet/aspnetcore#13273</span></span>](https://github.com/dotnet/aspnetcore/issues/13273)
