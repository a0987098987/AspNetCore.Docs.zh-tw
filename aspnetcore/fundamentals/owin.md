---
title: 具有 ASP.NET Core 的 Open Web Interface for .NET (OWIN)
author: ardalis
description: 探索 ASP.NET Core 如何支援 Open Web Interface for .NET (OWIN)，這可讓 Web 應用程式與網頁伺服器分離。
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/owin
ms.openlocfilehash: 3ff7b6e02284b4f6c61bf5d31013b4edfe8f7f29
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/18/2018
ms.locfileid: "31483493"
---
# <a name="open-web-interface-for-net-owin-with-aspnet-core"></a><span data-ttu-id="d64fd-103">具有 ASP.NET Core 的 Open Web Interface for .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="d64fd-103">Open Web Interface for .NET (OWIN) with ASP.NET Core</span></span>

<span data-ttu-id="d64fd-104">作者：[Steve Smith](https://ardalis.com/) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d64fd-104">By [Steve Smith](https://ardalis.com/) and  [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d64fd-105">ASP.NET Core 支援Open Web Interface for .NET (OWIN)。</span><span class="sxs-lookup"><span data-stu-id="d64fd-105">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="d64fd-106">OWIN 可讓 Web 應用程式獨立於網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="d64fd-106">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="d64fd-107">它會定義中介軟體要在管線中用來處理要求和相關聯回應的標準方式。</span><span class="sxs-lookup"><span data-stu-id="d64fd-107">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="d64fd-108">ASP.NET Core 應用程式和中介軟體可以與以 OWIN 為基礎的應用程式、伺服器及中介軟體進行交互操作。</span><span class="sxs-lookup"><span data-stu-id="d64fd-108">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="d64fd-109">OWIN 提供分離層，可讓兩個利用不同物件模型的架構一起使用。</span><span class="sxs-lookup"><span data-stu-id="d64fd-109">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="d64fd-110">`Microsoft.AspNetCore.Owin` 套件提供兩個配接器實作：</span><span class="sxs-lookup"><span data-stu-id="d64fd-110">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>
- <span data-ttu-id="d64fd-111">ASP.NET Core 至 OWIN</span><span class="sxs-lookup"><span data-stu-id="d64fd-111">ASP.NET Core to OWIN</span></span> 
- <span data-ttu-id="d64fd-112">OWIN 至 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d64fd-112">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="d64fd-113">這可讓 ASP.NET Core 裝載在 OWIN 相容的伺服器/主機之上，或讓其他 OWIN 相容的元件在 ASP.NET Core 之上執行。</span><span class="sxs-lookup"><span data-stu-id="d64fd-113">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host, or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

<span data-ttu-id="d64fd-114">注意：使用這些配接器將伴隨效能成本增加。</span><span class="sxs-lookup"><span data-stu-id="d64fd-114">Note: Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="d64fd-115">僅使用 ASP.NET Core 元件的應用程式不應使用 Owin 套件或配接器。</span><span class="sxs-lookup"><span data-stu-id="d64fd-115">Applications using only ASP.NET Core components shouldn't use the Owin package or adapters.</span></span>

<span data-ttu-id="d64fd-116">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d64fd-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a><span data-ttu-id="d64fd-117">在 ASP.NET 管線中執行 OWIN 中介軟體</span><span class="sxs-lookup"><span data-stu-id="d64fd-117">Running OWIN middleware in the ASP.NET pipeline</span></span>

<span data-ttu-id="d64fd-118">ASP.NET Core 的 OWIN 支援部署為 `Microsoft.AspNetCore.Owin` 套件的一部分。</span><span class="sxs-lookup"><span data-stu-id="d64fd-118">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="d64fd-119">您可以藉由安裝此套件，將 OWIN 支援匯入您的專案中。</span><span class="sxs-lookup"><span data-stu-id="d64fd-119">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="d64fd-120">OWIN 中介軟體符合 [OWIN 規格](http://owin.org/spec/spec/owin-1.0.0.html)，它需要設定 `Func<IDictionary<string, object>, Task>` 介面和特定的索引鍵 (例如 `owin.ResponseBody`)。</span><span class="sxs-lookup"><span data-stu-id="d64fd-120">OWIN middleware conforms to the [OWIN specification](http://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="d64fd-121">下列簡單的 OWIN 中介軟體會顯示 "Hello World"：</span><span class="sxs-lookup"><span data-stu-id="d64fd-121">The following simple OWIN middleware displays "Hello World":</span></span>

```csharp
public Task OwinHello(IDictionary<string, object> environment)
{
    string responseText = "Hello World via OWIN";
    byte[] responseBytes = Encoding.UTF8.GetBytes(responseText);

    // OWIN Environment Keys: http://owin.org/spec/spec/owin-1.0.0.html
    var responseStream = (Stream)environment["owin.ResponseBody"];
    var responseHeaders = (IDictionary<string, string[]>)environment["owin.ResponseHeaders"];

    responseHeaders["Content-Length"] = new string[] { responseBytes.Length.ToString(CultureInfo.InvariantCulture) };
    responseHeaders["Content-Type"] = new string[] { "text/plain" };

    return responseStream.WriteAsync(responseBytes, 0, responseBytes.Length);
}
```

<span data-ttu-id="d64fd-122">範例簽章會傳回 `Task`，並依照 OWIN 的要求接受 `IDictionary<string, object>`。</span><span class="sxs-lookup"><span data-stu-id="d64fd-122">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="d64fd-123">下列程式碼示範如何使用 `UseOwin` 擴充方法，將 `OwinHello` 中介軟體 (如上所示) 新增至 ASP.NET 管線。</span><span class="sxs-lookup"><span data-stu-id="d64fd-123">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET pipeline with the `UseOwin` extension method.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

<span data-ttu-id="d64fd-124">您可以設定在 OWIN 管線內進行其他動作。</span><span class="sxs-lookup"><span data-stu-id="d64fd-124">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="d64fd-125">只能在第一次寫入回應資料流之前修改回應標頭。</span><span class="sxs-lookup"><span data-stu-id="d64fd-125">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="d64fd-126">基於效能考量，建議您不要多次呼叫 `UseOwin`。</span><span class="sxs-lookup"><span data-stu-id="d64fd-126">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="d64fd-127">OWIN 元件如果群組在一起，其運作效能最佳。</span><span class="sxs-lookup"><span data-stu-id="d64fd-127">OWIN components will operate best if grouped together.</span></span>

```csharp
app.UseOwin(pipeline =>
{
    pipeline(async (next) =>
    {
        // do something before
        await OwinHello(new OwinEnvironment(HttpContext));
        // do something after
    });
});
```

<a name="hosting-on-owin"></a>

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a><span data-ttu-id="d64fd-128">使用以 OWIN 為基礎的伺服器上所裝載的 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d64fd-128">Using ASP.NET Hosting on an OWIN-based server</span></span>

<span data-ttu-id="d64fd-129">以 OWIN 為基礎的伺服器可以裝載 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d64fd-129">OWIN-based servers can host ASP.NET applications.</span></span> <span data-ttu-id="d64fd-130">其中一個這類伺服器是 [Nowin](https://github.com/Bobris/Nowin)，其為 .NET OWIN 網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="d64fd-130">One such server is [Nowin](https://github.com/Bobris/Nowin), a .NET OWIN web server.</span></span> <span data-ttu-id="d64fd-131">在本文的範例中，包含了參考 Nowin 並使用它來建立 `IServer` 能夠自我裝載之 ASP.NET Core 的專案。</span><span class="sxs-lookup"><span data-stu-id="d64fd-131">In the sample for this article, I've included a project that references Nowin and uses it to create an `IServer` capable of self-hosting ASP.NET Core.</span></span>

[!code-csharp[](owin/sample/src/NowinSample/Program.cs?highlight=15)]

<span data-ttu-id="d64fd-132">`IServer` 是需要 `Features` 屬性和 `Start` 方法的介面。</span><span class="sxs-lookup"><span data-stu-id="d64fd-132">`IServer` is an interface that requires a `Features` property and a `Start` method.</span></span>

<span data-ttu-id="d64fd-133">`Start` 負責設定和啟動伺服器，在此情況下，這會透過一系列 Fluent API 呼叫來完成，而這些呼叫設定從 IServerAddressesFeature 剖析的位址。</span><span class="sxs-lookup"><span data-stu-id="d64fd-133">`Start` is responsible for configuring and starting the server, which in this case is done through a series of fluent API calls that set addresses parsed from the IServerAddressesFeature.</span></span> <span data-ttu-id="d64fd-134">請注意，`_builder` 變數的 Fluent 組態會指定要求將由稍早在方法中定義的 `appFunc` 處理。</span><span class="sxs-lookup"><span data-stu-id="d64fd-134">Note that the fluent configuration of the `_builder` variable specifies that requests will be handled by the `appFunc` defined earlier in the method.</span></span> <span data-ttu-id="d64fd-135">每個要求都會呼叫這個 `Func` 來處理傳入的要求。</span><span class="sxs-lookup"><span data-stu-id="d64fd-135">This `Func` is called on each request to process incoming requests.</span></span>

<span data-ttu-id="d64fd-136">我們也將新增 `IWebHostBuilder` 延伸模組，以便能夠輕鬆地新增和設定 Nowin 伺服器。</span><span class="sxs-lookup"><span data-stu-id="d64fd-136">We'll also add an `IWebHostBuilder` extension to make it easy to add and configure the Nowin server.</span></span>

```csharp
using System;
using Microsoft.AspNetCore.Hosting.Server;
using Microsoft.Extensions.DependencyInjection;
using Nowin;
using NowinSample;

namespace Microsoft.AspNetCore.Hosting
{
    public static class NowinWebHostBuilderExtensions
    {
        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder)
        {
            return builder.ConfigureServices(services =>
            {
                services.AddSingleton<IServer, NowinServer>();
            });
        }

        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder, Action<ServerBuilder> configure)
        {
            builder.ConfigureServices(services =>
            {
                services.Configure(configure);
            });
            return builder.UseNowin();
        }
    }
}
```

<span data-ttu-id="d64fd-137">完成上述作業後，請叫用 *Program.cs* 中的延伸模組，以使用這個自訂伺服器來執行 ASP.NET Core 應用程式：</span><span class="sxs-lookup"><span data-stu-id="d64fd-137">With this in place, invoke the extension in *Program.cs* to run an ASP.NET Core app using this custom server:</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;

namespace NowinSample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = new WebHostBuilder()
                .UseNowin()
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseIISIntegration()
                .UseStartup<Startup>()
                .Build();

            host.Run();
        }
    }
}
```

<span data-ttu-id="d64fd-138">深入了解 ASP.NET [伺服器](servers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="d64fd-138">Learn more about ASP.NET [Servers](servers/index.md).</span></span>

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="d64fd-139">在以 OWIN 為基礎的伺服器上執行 ASP.NET Core 並使用其 WebSocket 支援</span><span class="sxs-lookup"><span data-stu-id="d64fd-139">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="d64fd-140">ASP.NET Core 如何利用以 OWIN 為基礎之伺服器功能的另一個範例是存取 WebSocket 等功能。</span><span class="sxs-lookup"><span data-stu-id="d64fd-140">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="d64fd-141">在上述範例中使用的 .NET OWIN 網頁伺服器支援內建的 Web 通訊端，供 ASP.NET Core 應用程式加以利用。</span><span class="sxs-lookup"><span data-stu-id="d64fd-141">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="d64fd-142">下列範例顯示的簡單 Web 應用程式支援 Web 通訊端，並透過 Websocket 回應傳送至伺服器的所有項目。</span><span class="sxs-lookup"><span data-stu-id="d64fd-142">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            if (context.WebSockets.IsWebSocketRequest)
            {
                WebSocket webSocket = await context.WebSockets.AcceptWebSocketAsync();
                await EchoWebSocket(webSocket);
            }
            else
            {
                await next();
            }
        });

        app.Run(context =>
        {
            return context.Response.WriteAsync("Hello World");
        });
    }

    private async Task EchoWebSocket(WebSocket webSocket)
    {
        byte[] buffer = new byte[1024];
        WebSocketReceiveResult received = await webSocket.ReceiveAsync(
            new ArraySegment<byte>(buffer), CancellationToken.None);

        while (!webSocket.CloseStatus.HasValue)
        {
            // Echo anything we receive
            await webSocket.SendAsync(new ArraySegment<byte>(buffer, 0, received.Count), 
                received.MessageType, received.EndOfMessage, CancellationToken.None);

            received = await webSocket.ReceiveAsync(new ArraySegment<byte>(buffer), 
                CancellationToken.None);
        }

        await webSocket.CloseAsync(webSocket.CloseStatus.Value, 
            webSocket.CloseStatusDescription, CancellationToken.None);
    }
}
```

<span data-ttu-id="d64fd-143">此[範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample)使用與上述相同的 `NowinServer` 進行設定 - 唯一的差異是在其 `Configure` 方法中設定應用程式的方式。</span><span class="sxs-lookup"><span data-stu-id="d64fd-143">This [sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) is configured using the same `NowinServer` as the previous one - the only difference is in how the application is configured in its `Configure` method.</span></span> <span data-ttu-id="d64fd-144">使用[簡單 Websocket 用戶端](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en)的測試示範此應用程式：</span><span class="sxs-lookup"><span data-stu-id="d64fd-144">A test using [a simple websocket client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstrates  the application:</span></span>

![Web 通訊端測試用戶端](owin/_static/websocket-test.png)

## <a name="owin-environment"></a><span data-ttu-id="d64fd-146">OWIN 環境</span><span class="sxs-lookup"><span data-stu-id="d64fd-146">OWIN environment</span></span>

<span data-ttu-id="d64fd-147">您可以使用 `HttpContext` 建構 OWIN 環境。</span><span class="sxs-lookup"><span data-stu-id="d64fd-147">You can construct an OWIN environment using the `HttpContext`.</span></span>

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="d64fd-148">OWIN 索引鍵</span><span class="sxs-lookup"><span data-stu-id="d64fd-148">OWIN keys</span></span>

<span data-ttu-id="d64fd-149">OWIN 仰賴 `IDictionary<string,object>` 物件在 HTTP 要求/回應交換中傳遞資訊。</span><span class="sxs-lookup"><span data-stu-id="d64fd-149">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="d64fd-150">ASP.NET Core 會實作下面所列的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="d64fd-150">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="d64fd-151">請參閱[主要規格、模組延伸](http://owin.org/#spec)和 [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html) (OWIN 索引鍵指導方針和共用索引鍵)。</span><span class="sxs-lookup"><span data-stu-id="d64fd-151">See the [primary specification, extensions](http://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="d64fd-152">要求資料 (OWIN 1.0.0 版)</span><span class="sxs-lookup"><span data-stu-id="d64fd-152">Request data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="d64fd-153">Key</span><span class="sxs-lookup"><span data-stu-id="d64fd-153">Key</span></span>               | <span data-ttu-id="d64fd-154">值 (類型)</span><span class="sxs-lookup"><span data-stu-id="d64fd-154">Value (type)</span></span> | <span data-ttu-id="d64fd-155">描述</span><span class="sxs-lookup"><span data-stu-id="d64fd-155">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="d64fd-156">owin.RequestScheme</span><span class="sxs-lookup"><span data-stu-id="d64fd-156">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="d64fd-157">owin.RequestMethod</span><span class="sxs-lookup"><span data-stu-id="d64fd-157">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="d64fd-158">owin.RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="d64fd-158">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="d64fd-159">owin.RequestPath</span><span class="sxs-lookup"><span data-stu-id="d64fd-159">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="d64fd-160">owin.RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="d64fd-160">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="d64fd-161">owin.RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="d64fd-161">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="d64fd-162">owin.RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="d64fd-162">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="d64fd-163">owin.RequestBody</span><span class="sxs-lookup"><span data-stu-id="d64fd-163">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="d64fd-164">要求資料 (OWIN 1.1.0 版)</span><span class="sxs-lookup"><span data-stu-id="d64fd-164">Request data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="d64fd-165">Key</span><span class="sxs-lookup"><span data-stu-id="d64fd-165">Key</span></span>               | <span data-ttu-id="d64fd-166">值 (類型)</span><span class="sxs-lookup"><span data-stu-id="d64fd-166">Value (type)</span></span> | <span data-ttu-id="d64fd-167">描述</span><span class="sxs-lookup"><span data-stu-id="d64fd-167">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="d64fd-168">owin.RequestId</span><span class="sxs-lookup"><span data-stu-id="d64fd-168">owin.RequestId</span></span> | `String` | <span data-ttu-id="d64fd-169">Optional</span><span class="sxs-lookup"><span data-stu-id="d64fd-169">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="d64fd-170">回應資料 (OWIN 1.0.0 版)</span><span class="sxs-lookup"><span data-stu-id="d64fd-170">Response data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="d64fd-171">Key</span><span class="sxs-lookup"><span data-stu-id="d64fd-171">Key</span></span>               | <span data-ttu-id="d64fd-172">值 (類型)</span><span class="sxs-lookup"><span data-stu-id="d64fd-172">Value (type)</span></span> | <span data-ttu-id="d64fd-173">描述</span><span class="sxs-lookup"><span data-stu-id="d64fd-173">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="d64fd-174">owin.ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="d64fd-174">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="d64fd-175">Optional</span><span class="sxs-lookup"><span data-stu-id="d64fd-175">Optional</span></span> |
| <span data-ttu-id="d64fd-176">owin.ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="d64fd-176">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="d64fd-177">Optional</span><span class="sxs-lookup"><span data-stu-id="d64fd-177">Optional</span></span> |
| <span data-ttu-id="d64fd-178">owin.ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="d64fd-178">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="d64fd-179">owin.ResponseBody</span><span class="sxs-lookup"><span data-stu-id="d64fd-179">owin.ResponseBody</span></span> | `Stream`  | |


### <a name="other-data-owin-v100"></a><span data-ttu-id="d64fd-180">其他資料 (OWIN 1.0.0 版)</span><span class="sxs-lookup"><span data-stu-id="d64fd-180">Other data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="d64fd-181">Key</span><span class="sxs-lookup"><span data-stu-id="d64fd-181">Key</span></span>               | <span data-ttu-id="d64fd-182">值 (類型)</span><span class="sxs-lookup"><span data-stu-id="d64fd-182">Value (type)</span></span> | <span data-ttu-id="d64fd-183">描述</span><span class="sxs-lookup"><span data-stu-id="d64fd-183">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="d64fd-184">owin.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="d64fd-184">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="d64fd-185">owin.Version</span><span class="sxs-lookup"><span data-stu-id="d64fd-185">owin.Version</span></span>  | `String` | |   


### <a name="common-keys"></a><span data-ttu-id="d64fd-186">共同索引鍵</span><span class="sxs-lookup"><span data-stu-id="d64fd-186">Common keys</span></span>

| <span data-ttu-id="d64fd-187">Key</span><span class="sxs-lookup"><span data-stu-id="d64fd-187">Key</span></span>               | <span data-ttu-id="d64fd-188">值 (類型)</span><span class="sxs-lookup"><span data-stu-id="d64fd-188">Value (type)</span></span> | <span data-ttu-id="d64fd-189">描述</span><span class="sxs-lookup"><span data-stu-id="d64fd-189">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="d64fd-190">ssl.ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="d64fd-190">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="d64fd-191">ssl.LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="d64fd-191">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="d64fd-192">server.RemoteIpAddress</span><span class="sxs-lookup"><span data-stu-id="d64fd-192">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="d64fd-193">server.RemotePort</span><span class="sxs-lookup"><span data-stu-id="d64fd-193">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="d64fd-194">server.LocalIpAddress</span><span class="sxs-lookup"><span data-stu-id="d64fd-194">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="d64fd-195">server.LocalPort</span><span class="sxs-lookup"><span data-stu-id="d64fd-195">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="d64fd-196">server.IsLocal</span><span class="sxs-lookup"><span data-stu-id="d64fd-196">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="d64fd-197">server.OnSendingHeaders</span><span class="sxs-lookup"><span data-stu-id="d64fd-197">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a><span data-ttu-id="d64fd-198">SendFiles 0.3.0 版</span><span class="sxs-lookup"><span data-stu-id="d64fd-198">SendFiles v0.3.0</span></span>

| <span data-ttu-id="d64fd-199">Key</span><span class="sxs-lookup"><span data-stu-id="d64fd-199">Key</span></span>               | <span data-ttu-id="d64fd-200">值 (類型)</span><span class="sxs-lookup"><span data-stu-id="d64fd-200">Value (type)</span></span> | <span data-ttu-id="d64fd-201">描述</span><span class="sxs-lookup"><span data-stu-id="d64fd-201">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="d64fd-202">sendfile.SendAsync</span><span class="sxs-lookup"><span data-stu-id="d64fd-202">sendfile.SendAsync</span></span> | <span data-ttu-id="d64fd-203">請參閱[委派簽章](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="d64fd-203">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="d64fd-204">每個要求</span><span class="sxs-lookup"><span data-stu-id="d64fd-204">Per Request</span></span> |


### <a name="opaque-v030"></a><span data-ttu-id="d64fd-205">Opaque 0.3.0 版</span><span class="sxs-lookup"><span data-stu-id="d64fd-205">Opaque v0.3.0</span></span>

| <span data-ttu-id="d64fd-206">Key</span><span class="sxs-lookup"><span data-stu-id="d64fd-206">Key</span></span>               | <span data-ttu-id="d64fd-207">值 (類型)</span><span class="sxs-lookup"><span data-stu-id="d64fd-207">Value (type)</span></span> | <span data-ttu-id="d64fd-208">描述</span><span class="sxs-lookup"><span data-stu-id="d64fd-208">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="d64fd-209">opaque.Version</span><span class="sxs-lookup"><span data-stu-id="d64fd-209">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="d64fd-210">opaque.Upgrade</span><span class="sxs-lookup"><span data-stu-id="d64fd-210">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="d64fd-211">請參閱[委派簽章](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="d64fd-211">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="d64fd-212">opaque.Stream</span><span class="sxs-lookup"><span data-stu-id="d64fd-212">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="d64fd-213">opaque.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="d64fd-213">opaque.CallCancelled</span></span> | `CancellationToken` |  |


### <a name="websocket-v030"></a><span data-ttu-id="d64fd-214">WebSocket 0.3.0 版</span><span class="sxs-lookup"><span data-stu-id="d64fd-214">WebSocket v0.3.0</span></span>

| <span data-ttu-id="d64fd-215">Key</span><span class="sxs-lookup"><span data-stu-id="d64fd-215">Key</span></span>               | <span data-ttu-id="d64fd-216">值 (類型)</span><span class="sxs-lookup"><span data-stu-id="d64fd-216">Value (type)</span></span> | <span data-ttu-id="d64fd-217">描述</span><span class="sxs-lookup"><span data-stu-id="d64fd-217">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="d64fd-218">websocket.Version</span><span class="sxs-lookup"><span data-stu-id="d64fd-218">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="d64fd-219">websocket.Accept</span><span class="sxs-lookup"><span data-stu-id="d64fd-219">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="d64fd-220">請參閱[委派簽章](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="d64fd-220">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="d64fd-221">websocket.AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="d64fd-221">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="d64fd-222">非規格</span><span class="sxs-lookup"><span data-stu-id="d64fd-222">Non-spec</span></span> |
| <span data-ttu-id="d64fd-223">websocket.SubProtocol</span><span class="sxs-lookup"><span data-stu-id="d64fd-223">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="d64fd-224">請參閱 [RFC6455 4.2.2 節](https://tools.ietf.org/html/rfc6455#section-4.2.2)的步驟 5.5</span><span class="sxs-lookup"><span data-stu-id="d64fd-224">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="d64fd-225">websocket.SendAsync</span><span class="sxs-lookup"><span data-stu-id="d64fd-225">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="d64fd-226">請參閱[委派簽章](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="d64fd-226">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="d64fd-227">websocket.ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="d64fd-227">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="d64fd-228">請參閱[委派簽章](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="d64fd-228">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="d64fd-229">websocket.CloseAsync</span><span class="sxs-lookup"><span data-stu-id="d64fd-229">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="d64fd-230">請參閱[委派簽章](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="d64fd-230">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="d64fd-231">websocket.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="d64fd-231">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="d64fd-232">websocket.ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="d64fd-232">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="d64fd-233">Optional</span><span class="sxs-lookup"><span data-stu-id="d64fd-233">Optional</span></span> |
| <span data-ttu-id="d64fd-234">websocket.ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="d64fd-234">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="d64fd-235">Optional</span><span class="sxs-lookup"><span data-stu-id="d64fd-235">Optional</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="d64fd-236">其他資源</span><span class="sxs-lookup"><span data-stu-id="d64fd-236">Additional resources</span></span>

* [<span data-ttu-id="d64fd-237">中介軟體</span><span class="sxs-lookup"><span data-stu-id="d64fd-237">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="d64fd-238">伺服器</span><span class="sxs-lookup"><span data-stu-id="d64fd-238">Servers</span></span>](xref:fundamentals/servers/index)
