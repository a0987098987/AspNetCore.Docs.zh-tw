---
title: 測試 ASP.NET Core 中介軟體
author: tratcher
description: 瞭解如何使用 TestServer 來測試 ASP.NET Core 中介軟體。
ms.author: riande
ms.custom: mvc
ms.date: 5/6/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: test/middleware
ms.openlocfilehash: 06ff7167e32fbd613c18709e31ecd078b3dfc926
ms.sourcegitcommit: 30fcf69556b6b6ec54a3879e280d5f61f018b48f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/07/2020
ms.locfileid: "82876434"
---
# <a name="test-aspnet-core-middleware"></a><span data-ttu-id="ba569-103">測試 ASP.NET Core 中介軟體</span><span class="sxs-lookup"><span data-stu-id="ba569-103">Test ASP.NET Core middleware</span></span>

<span data-ttu-id="ba569-104">依[Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="ba569-104">By [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="ba569-105">中介軟體可以與<xref:Microsoft.AspNetCore.TestHost.TestServer>隔離地進行測試。</span><span class="sxs-lookup"><span data-stu-id="ba569-105">Middleware can be tested in isolation with <xref:Microsoft.AspNetCore.TestHost.TestServer>.</span></span> <span data-ttu-id="ba569-106">它可讓您：</span><span class="sxs-lookup"><span data-stu-id="ba569-106">It allows you to:</span></span>

* <span data-ttu-id="ba569-107">具現化應用程式管線，其中只包含您需要測試的元件。</span><span class="sxs-lookup"><span data-stu-id="ba569-107">Instantiate an app pipeline containing only the components that you need to test.</span></span>
* <span data-ttu-id="ba569-108">傳送自訂要求以驗證中介軟體行為。</span><span class="sxs-lookup"><span data-stu-id="ba569-108">Send custom requests to verify middleware behavior.</span></span>

<span data-ttu-id="ba569-109">優點：</span><span class="sxs-lookup"><span data-stu-id="ba569-109">Advantages:</span></span>

* <span data-ttu-id="ba569-110">要求會在記憶體中傳送，而不是透過網路進行序列化。</span><span class="sxs-lookup"><span data-stu-id="ba569-110">Requests are sent in-memory rather than being serialized over the network.</span></span>
* <span data-ttu-id="ba569-111">這可避免其他問題，例如埠管理和 HTTPS 憑證。</span><span class="sxs-lookup"><span data-stu-id="ba569-111">This avoids additional concerns, such as port management and HTTPS certificates.</span></span>
* <span data-ttu-id="ba569-112">中介軟體中的例外狀況可以直接傳回到呼叫測試。</span><span class="sxs-lookup"><span data-stu-id="ba569-112">Exceptions in the middleware can flow directly back to the calling test.</span></span>
* <span data-ttu-id="ba569-113">您可以直接在測試中自訂伺服器資料結構<xref:Microsoft.AspNetCore.Http.HttpContext>，例如。</span><span class="sxs-lookup"><span data-stu-id="ba569-113">It's possible to customize server data structures, such as <xref:Microsoft.AspNetCore.Http.HttpContext>, directly in the test.</span></span>

## <a name="set-up-the-testserver"></a><span data-ttu-id="ba569-114">設定 TestServer</span><span class="sxs-lookup"><span data-stu-id="ba569-114">Set up the TestServer</span></span>

<span data-ttu-id="ba569-115">在測試專案中，建立測試：</span><span class="sxs-lookup"><span data-stu-id="ba569-115">In the test project, create a test:</span></span>

* <span data-ttu-id="ba569-116">建立並啟動使用<xref:Microsoft.AspNetCore.TestHost.TestServer>的主機。</span><span class="sxs-lookup"><span data-stu-id="ba569-116">Build and start a host that uses <xref:Microsoft.AspNetCore.TestHost.TestServer>.</span></span>
* <span data-ttu-id="ba569-117">新增中介軟體所使用的任何必要服務。</span><span class="sxs-lookup"><span data-stu-id="ba569-117">Add any required services that the middleware uses.</span></span>
* <span data-ttu-id="ba569-118">設定處理管線以使用中介軟體進行測試。</span><span class="sxs-lookup"><span data-stu-id="ba569-118">Configure the processing pipeline to use the middleware for the test.</span></span>

[!code-csharp[](middleware/samples_snapshot/3.x/setup.cs?highlight=4-18)]

## <a name="send-requests-with-httpclient"></a><span data-ttu-id="ba569-119">使用 HttpClient 傳送要求</span><span class="sxs-lookup"><span data-stu-id="ba569-119">Send requests with HttpClient</span></span>
<span data-ttu-id="ba569-120">使用<xref:System.Net.Http.HttpClient>下列內容傳送要求：</span><span class="sxs-lookup"><span data-stu-id="ba569-120">Send a request using <xref:System.Net.Http.HttpClient>:</span></span>

[!code-csharp[](middleware/samples_snapshot/3.x/request.cs?highlight=20)]

<span data-ttu-id="ba569-121">判斷提示結果。</span><span class="sxs-lookup"><span data-stu-id="ba569-121">Assert the result.</span></span> <span data-ttu-id="ba569-122">首先，請將判斷提示設為與預期結果相反的判斷提示。</span><span class="sxs-lookup"><span data-stu-id="ba569-122">First, make an assertion the opposite of the result that you expect.</span></span> <span data-ttu-id="ba569-123">具有錯誤正面判斷提示的初始執行，會確認中介軟體正常執行時測試失敗。</span><span class="sxs-lookup"><span data-stu-id="ba569-123">An initial run with a false positive assertion confirms that the test fails when the middleware is performing correctly.</span></span> <span data-ttu-id="ba569-124">執行測試並確認測試失敗。</span><span class="sxs-lookup"><span data-stu-id="ba569-124">Run the test and confirm that the test fails.</span></span>

<span data-ttu-id="ba569-125">在下列範例中，當要求根端點時，中介軟體應該會傳回404狀態碼（找*不到*）。</span><span class="sxs-lookup"><span data-stu-id="ba569-125">In the following example, the middleware should return a 404 status code (*Not Found*) when the root endpoint is requested.</span></span> <span data-ttu-id="ba569-126">使用`Assert.NotEqual( ... );`執行第一次測試，這應該會失敗：</span><span class="sxs-lookup"><span data-stu-id="ba569-126">Make the first test run with `Assert.NotEqual( ... );`, which should fail:</span></span>

[!code-csharp[](middleware/samples_snapshot/3.x/false-failure-check.cs?highlight=22)]

<span data-ttu-id="ba569-127">變更判斷提示，以在正常操作條件下測試中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ba569-127">Change the assertion to test the middleware under normal operating conditions.</span></span> <span data-ttu-id="ba569-128">最後的測試會`Assert.Equal( ... );`使用。</span><span class="sxs-lookup"><span data-stu-id="ba569-128">The final test uses `Assert.Equal( ... );`.</span></span> <span data-ttu-id="ba569-129">再次執行測試，以確認它通過。</span><span class="sxs-lookup"><span data-stu-id="ba569-129">Run the test again to confirm that it passes.</span></span>

[!code-csharp[](middleware/samples_snapshot/3.x/final-test.cs?highlight=22)]

## <a name="send-requests-with-httpcontext"></a><span data-ttu-id="ba569-130">使用 HttpCoNtext 傳送要求</span><span class="sxs-lookup"><span data-stu-id="ba569-130">Send requests with HttpContext</span></span>

<span data-ttu-id="ba569-131">測試應用程式也可以使用[SendAsync （Action\<HttpCoNtext>，CancellationToken）](xref:Microsoft.AspNetCore.TestHost.TestServer.SendAsync%2A)傳送要求。</span><span class="sxs-lookup"><span data-stu-id="ba569-131">A test app can also send a request using [SendAsync(Action\<HttpContext>, CancellationToken)](xref:Microsoft.AspNetCore.TestHost.TestServer.SendAsync%2A).</span></span> <span data-ttu-id="ba569-132">在下列範例中，當中間件處理時`https://example.com/A/Path/?and=query` ，會進行幾項檢查：</span><span class="sxs-lookup"><span data-stu-id="ba569-132">In the following example, several checks are made when `https://example.com/A/Path/?and=query` is processed by the middleware:</span></span>

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

<span data-ttu-id="ba569-133"><xref:Microsoft.AspNetCore.TestHost.TestServer.SendAsync%2A>允許直接設定<xref:Microsoft.AspNetCore.Http.HttpContext>物件，而不是使用<xref:System.Net.Http.HttpClient>抽象概念。</span><span class="sxs-lookup"><span data-stu-id="ba569-133"><xref:Microsoft.AspNetCore.TestHost.TestServer.SendAsync%2A> permits direct configuration of an <xref:Microsoft.AspNetCore.Http.HttpContext> object rather than using the <xref:System.Net.Http.HttpClient> abstractions.</span></span> <span data-ttu-id="ba569-134">使用<xref:Microsoft.AspNetCore.TestHost.TestServer.SendAsync%2A>來操作只能在伺服器上使用的結構，例如[HttpcoNtext](xref:Microsoft.AspNetCore.Http.HttpContext.Items)或[HTTPcoNtext 功能](xref:Microsoft.AspNetCore.Http.HttpContext.Features)。</span><span class="sxs-lookup"><span data-stu-id="ba569-134">Use <xref:Microsoft.AspNetCore.TestHost.TestServer.SendAsync%2A> to manipulate structures only available on the server, such as [HttpContext.Items](xref:Microsoft.AspNetCore.Http.HttpContext.Items) or [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features).</span></span>

<span data-ttu-id="ba569-135">如同先前測試過*404-找不*到回應的範例，請檢查前述測試中每個`Assert`語句的相反。</span><span class="sxs-lookup"><span data-stu-id="ba569-135">As with the earlier example that tested for a *404 - Not Found* response, check the opposite for each `Assert` statement in the preceding test.</span></span> <span data-ttu-id="ba569-136">當中間件正常運作時，此檢查會確認測試是否正確。</span><span class="sxs-lookup"><span data-stu-id="ba569-136">The check confirms that the test fails correctly when the middleware is operating normally.</span></span> <span data-ttu-id="ba569-137">當您確認錯誤正面測試正常運作之後，請針對測試的`Assert`預期條件和值設定最終語句。</span><span class="sxs-lookup"><span data-stu-id="ba569-137">After you've confirmed that the false positive test works, set the final `Assert` statements for the expected conditions and values of the test.</span></span> <span data-ttu-id="ba569-138">再次執行，以確認測試通過。</span><span class="sxs-lookup"><span data-stu-id="ba569-138">Run it again to confirm that the test passes.</span></span>
