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
# <a name="test-aspnet-core-middleware"></a>測試 ASP.NET Core 中介軟體

依[Chris Ross](https://github.com/Tratcher)

中介軟體可以與<xref:Microsoft.AspNetCore.TestHost.TestServer>隔離地進行測試。 它可讓您：

* 具現化應用程式管線，其中只包含您需要測試的元件。
* 傳送自訂要求以驗證中介軟體行為。

優點：

* 要求會在記憶體中傳送，而不是透過網路進行序列化。
* 這可避免其他問題，例如埠管理和 HTTPS 憑證。
* 中介軟體中的例外狀況可以直接傳回到呼叫測試。
* 您可以直接在測試中自訂伺服器資料結構<xref:Microsoft.AspNetCore.Http.HttpContext>，例如。

## <a name="set-up-the-testserver"></a>設定 TestServer

在測試專案中，建立測試：

* 建立並啟動使用<xref:Microsoft.AspNetCore.TestHost.TestServer>的主機。
* 新增中介軟體所使用的任何必要服務。
* 設定處理管線以使用中介軟體進行測試。

[!code-csharp[](middleware/samples_snapshot/3.x/setup.cs?highlight=4-18)]

## <a name="send-requests-with-httpclient"></a>使用 HttpClient 傳送要求
使用<xref:System.Net.Http.HttpClient>下列內容傳送要求：

[!code-csharp[](middleware/samples_snapshot/3.x/request.cs?highlight=20)]

判斷提示結果。 首先，請將判斷提示設為與預期結果相反的判斷提示。 具有錯誤正面判斷提示的初始執行，會確認中介軟體正常執行時測試失敗。 執行測試並確認測試失敗。

在下列範例中，當要求根端點時，中介軟體應該會傳回404狀態碼（找*不到*）。 使用`Assert.NotEqual( ... );`執行第一次測試，這應該會失敗：

[!code-csharp[](middleware/samples_snapshot/3.x/false-failure-check.cs?highlight=22)]

變更判斷提示，以在正常操作條件下測試中介軟體。 最後的測試會`Assert.Equal( ... );`使用。 再次執行測試，以確認它通過。

[!code-csharp[](middleware/samples_snapshot/3.x/final-test.cs?highlight=22)]

## <a name="send-requests-with-httpcontext"></a>使用 HttpCoNtext 傳送要求

測試應用程式也可以使用[SendAsync （Action\<HttpCoNtext>，CancellationToken）](xref:Microsoft.AspNetCore.TestHost.TestServer.SendAsync%2A)傳送要求。 在下列範例中，當中間件處理時`https://example.com/A/Path/?and=query` ，會進行幾項檢查：

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

<xref:Microsoft.AspNetCore.TestHost.TestServer.SendAsync%2A>允許直接設定<xref:Microsoft.AspNetCore.Http.HttpContext>物件，而不是使用<xref:System.Net.Http.HttpClient>抽象概念。 使用<xref:Microsoft.AspNetCore.TestHost.TestServer.SendAsync%2A>來操作只能在伺服器上使用的結構，例如[HttpcoNtext](xref:Microsoft.AspNetCore.Http.HttpContext.Items)或[HTTPcoNtext 功能](xref:Microsoft.AspNetCore.Http.HttpContext.Features)。

如同先前測試過*404-找不*到回應的範例，請檢查前述測試中每個`Assert`語句的相反。 當中間件正常運作時，此檢查會確認測試是否正確。 當您確認錯誤正面測試正常運作之後，請針對測試的`Assert`預期條件和值設定最終語句。 再次執行，以確認測試通過。
