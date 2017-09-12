---
title: "開啟 Web 介面 for.NET (OWIN)"
author: ardalis
description: "若要開啟.NET (OWIN) 的網頁介面的簡介。"
keywords: "ASP.NET Core，for.NET，OWIN 開啟 Web 介面"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 70c4e6bc-a773-4039-96ec-6fe557c9369d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/owin
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 796d075d4d0c6b888be4fc2225fc482acdbad498
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a><span data-ttu-id="fa5c9-104">若要開啟.NET (OWIN) 的網頁介面的簡介</span><span class="sxs-lookup"><span data-stu-id="fa5c9-104">Introduction to Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="fa5c9-105">由[Steve Smith](https://ardalis.com/)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fa5c9-105">By [Steve Smith](https://ardalis.com/) and  [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fa5c9-106">ASP.NET Core 支援開啟 Web 介面.NET (OWIN)。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-106">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="fa5c9-107">OWIN 可讓 web 應用程式從 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-107">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="fa5c9-108">它會定義可以在管線中用來處理要求和回應相關聯的中介軟體的標準方式。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-108">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="fa5c9-109">與 OWIN 應用程式、 伺服器和中介軟體的 ASP.NET Core 應用程式和中介軟體可以交互操作。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-109">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="fa5c9-110">OWIN 提供脫鉤層，可讓兩個架構來使用不同的物件模型一起使用。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-110">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="fa5c9-111">`Microsoft.AspNetCore.Owin`套件提供兩個配接器實作：</span><span class="sxs-lookup"><span data-stu-id="fa5c9-111">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>
- <span data-ttu-id="fa5c9-112">Owin ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fa5c9-112">ASP.NET Core to OWIN</span></span> 
- <span data-ttu-id="fa5c9-113">ASP.NET Core 的 OWIN</span><span class="sxs-lookup"><span data-stu-id="fa5c9-113">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="fa5c9-114">這可讓 ASP.NET Core 之上 OWIN 相容伺服器/主控件，或執行 ASP.NET Core 之上其他 OWIN 相容元件的裝載。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-114">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host, or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

<span data-ttu-id="fa5c9-115">注意： 使用這些介面卡隨附的效能成本。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-115">Note: Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="fa5c9-116">使用僅 ASP.NET 核心元件的應用程式不應該使用 Owin 封裝或介面卡。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-116">Applications using only ASP.NET Core components should not use the Owin package or adapters.</span></span>

[<span data-ttu-id="fa5c9-117">檢視或下載範例程式碼</span><span class="sxs-lookup"><span data-stu-id="fa5c9-117">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample)

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a><span data-ttu-id="fa5c9-118">在 ASP.NET 管線中執行 OWIN 中介軟體</span><span class="sxs-lookup"><span data-stu-id="fa5c9-118">Running OWIN middleware in the ASP.NET pipeline</span></span>

<span data-ttu-id="fa5c9-119">ASP.NET Core OWIN 支援部署為一部分`Microsoft.AspNetCore.Owin`封裝。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-119">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="fa5c9-120">您可以匯入您的專案的 OWIN 支援安裝此套件。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-120">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="fa5c9-121">OWIN 中介軟體符合[OWIN 規格](http://owin.org/spec/spec/owin-1.0.0.html)，這項工作需要`Func<IDictionary<string, object>, Task>`設定介面和特定的索引鍵 (例如`owin.ResponseBody`)。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-121">OWIN middleware conforms to the [OWIN specification](http://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="fa5c9-122">下列簡單的 OWIN 中介軟體會顯示"Hello World":</span><span class="sxs-lookup"><span data-stu-id="fa5c9-122">The following simple OWIN middleware displays "Hello World":</span></span>

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

<span data-ttu-id="fa5c9-123">範例簽章傳回`Task`並接受`IDictionary<string, object>`依 OWIN。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-123">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="fa5c9-124">下列程式碼示範如何加入`OwinHello`（如上所示） 具有的 ASP.NET 管線中介軟體`UseOwin`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-124">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET pipeline with the `UseOwin` extension method.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/fundamentals/owin/sample/src/OwinSample/Startup.cs"} -->

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}


```

<span data-ttu-id="fa5c9-125">您可以設定 OWIN 管線中進行其他動作。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-125">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="fa5c9-126">僅能在第一次寫入至回應資料流之前修改回應標頭。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-126">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="fa5c9-127">多個呼叫`UseOwin`建議您不要使用基於效能的考量。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-127">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="fa5c9-128">如果群組在一起，OWIN 元件會以最能運作。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-128">OWIN components will operate best if grouped together.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
app.UseOwin(pipeline =>
{
    pipeline(next =>
    {
        // do something before
        return OwinHello;
        // do something after
    });
});
```

<a name=hosting-on-owin></a>

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a><span data-ttu-id="fa5c9-129">使用 OWIN 架構的伺服器上的 ASP.NET 裝載</span><span class="sxs-lookup"><span data-stu-id="fa5c9-129">Using ASP.NET Hosting on an OWIN-based server</span></span>

<span data-ttu-id="fa5c9-130">OWIN 伺服器可以裝載 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-130">OWIN-based servers can host ASP.NET applications.</span></span> <span data-ttu-id="fa5c9-131">這類伺服器[Nowin](https://github.com/Bobris/Nowin)，.NET OWIN web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-131">One such server is [Nowin](https://github.com/Bobris/Nowin), a .NET OWIN web server.</span></span> <span data-ttu-id="fa5c9-132">在本文範例中，包括了參考 Nowin 並使用它來建立的專案`IServer`能夠自我裝載的 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-132">In the sample for this article, I've included a project that references Nowin and uses it to create an `IServer` capable of self-hosting ASP.NET Core.</span></span>

<span data-ttu-id="fa5c9-133">[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]</span><span class="sxs-lookup"><span data-stu-id="fa5c9-133">[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]</span></span>

<span data-ttu-id="fa5c9-134">`IServer`是需要的介面`Features`屬性和`Start`方法。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-134">`IServer` is an interface that requires an `Features` property and a `Start` method.</span></span>

<span data-ttu-id="fa5c9-135">`Start`會負責設定和啟動伺服器，在此情況下透過一系列 fluent 應用程式開發的應用程式開發介面呼叫從 IServerAddressesFeature 設定剖析的位址。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-135">`Start` is responsible for configuring and starting the server, which in this case is done through a series of fluent API calls that set addresses parsed from the IServerAddressesFeature.</span></span> <span data-ttu-id="fa5c9-136">請注意，fluent 應用程式開發的設定`_builder`變數會指定要求交由`appFunc`稍早在方法中定義。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-136">Note that the fluent configuration of the `_builder` variable specifies that requests will be handled by the `appFunc` defined earlier in the method.</span></span> <span data-ttu-id="fa5c9-137">這`Func`呼叫的每個要求來處理傳入的要求。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-137">This `Func` is called on each request to process incoming requests.</span></span>

<span data-ttu-id="fa5c9-138">我們也可以加入`IWebHostBuilder`輕鬆加入及設定 Nowin 伺服器的延伸模組。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-138">We'll also add an `IWebHostBuilder` extension to make it easy to add and configure the Nowin server.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"hl_lines": [11], "linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/fundamentals/owin/sample/src/NowinSample/NowinWebHostBuilderExtensions.cs"} -->

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

<span data-ttu-id="fa5c9-139">這個位置，所有的要求執行 ASP.NET 應用程式擴充功能呼叫中使用此自訂伺服器*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="fa5c9-139">With this in place, all that's required to run an ASP.NET application using this custom server to call the extension in *Program.cs*:</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"hl_lines": [15], "linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/fundamentals/owin/sample/src/NowinSample/Program.cs"} -->

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

<span data-ttu-id="fa5c9-140">深入了解 ASP.NET[伺服器](servers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-140">Learn more about ASP.NET [Servers](servers/index.md).</span></span>

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="fa5c9-141">OWIN 架構的伺服器上執行 ASP.NET Core，並使用 WebSockets 的支援</span><span class="sxs-lookup"><span data-stu-id="fa5c9-141">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="fa5c9-142">如何 OWIN 架構伺服器功能的另一個範例可以利用 ASP.NET Core 是 WebSockets 等功能的存取。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-142">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="fa5c9-143">在上述範例中使用.NET OWIN web 伺服器的 Web 通訊端中，建立以 ASP.NET Core 應用程式的支援。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-143">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="fa5c9-144">下列範例顯示簡單的 web 應用程式支援 Web 通訊端及回應透過 Websocket 傳送的所有項目。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-144">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"hl_lines": [7, 9, 10], "linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": true, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/fundamentals/owin/sample/src/NowinWebSockets/Startup.cs"} -->

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

<span data-ttu-id="fa5c9-145">這[範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample)已設定為使用相同`NowinServer`唯一的差別是應用程式中的設定方式與上一個為其`Configure`方法。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-145">This [sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) is configured using the same `NowinServer` as the previous one - the only difference is in how the application is configured in its `Configure` method.</span></span> <span data-ttu-id="fa5c9-146">使用測試[簡單 websocket 用戶端](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en)示範應用程式：</span><span class="sxs-lookup"><span data-stu-id="fa5c9-146">A test using [a simple websocket client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstrates  the application:</span></span>

![Web 通訊端測試用戶端](owin/_static/websocket-test.png)

## <a name="owin-environment"></a><span data-ttu-id="fa5c9-148">OWIN 環境</span><span class="sxs-lookup"><span data-stu-id="fa5c9-148">OWIN environment</span></span>

<span data-ttu-id="fa5c9-149">您可以建構的 OWIN 環境 using `HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-149">You can construct a OWIN environment using the `HttpContext`.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="fa5c9-150">OWIN 索引鍵</span><span class="sxs-lookup"><span data-stu-id="fa5c9-150">OWIN keys</span></span>

<span data-ttu-id="fa5c9-151">OWIN 取決於`IDictionary<string,object>`通訊內的 HTTP 要求/回應交換資訊的物件。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-151">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="fa5c9-152">ASP.NET Core 實作下面所列的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-152">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="fa5c9-153">請參閱[規格的主要、 延伸](http://owin.org/#spec)，和[OWIN 索引鍵指導方針和一般索引鍵](http://owin.org/spec/spec/CommonKeys.html)。</span><span class="sxs-lookup"><span data-stu-id="fa5c9-153">See the [primary specification, extensions](http://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="fa5c9-154">要求資料 (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="fa5c9-154">Request Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="fa5c9-155">Key</span><span class="sxs-lookup"><span data-stu-id="fa5c9-155">Key</span></span>               | <span data-ttu-id="fa5c9-156">值 （類型）</span><span class="sxs-lookup"><span data-stu-id="fa5c9-156">Value (type)</span></span> | <span data-ttu-id="fa5c9-157">說明</span><span class="sxs-lookup"><span data-stu-id="fa5c9-157">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="fa5c9-158">owin。RequestScheme</span><span class="sxs-lookup"><span data-stu-id="fa5c9-158">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="fa5c9-159">owin。RequestMethod</span><span class="sxs-lookup"><span data-stu-id="fa5c9-159">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="fa5c9-160">owin。RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="fa5c9-160">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="fa5c9-161">owin。RequestPath</span><span class="sxs-lookup"><span data-stu-id="fa5c9-161">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="fa5c9-162">owin。RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="fa5c9-162">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="fa5c9-163">owin。RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="fa5c9-163">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="fa5c9-164">owin。RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="fa5c9-164">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="fa5c9-165">owin。RequestBody</span><span class="sxs-lookup"><span data-stu-id="fa5c9-165">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="fa5c9-166">要求資料 (OWIN v1.1.0)</span><span class="sxs-lookup"><span data-stu-id="fa5c9-166">Request Data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="fa5c9-167">Key</span><span class="sxs-lookup"><span data-stu-id="fa5c9-167">Key</span></span>               | <span data-ttu-id="fa5c9-168">值 （類型）</span><span class="sxs-lookup"><span data-stu-id="fa5c9-168">Value (type)</span></span> | <span data-ttu-id="fa5c9-169">說明</span><span class="sxs-lookup"><span data-stu-id="fa5c9-169">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="fa5c9-170">owin。RequestId</span><span class="sxs-lookup"><span data-stu-id="fa5c9-170">owin.RequestId</span></span> | `String` | <span data-ttu-id="fa5c9-171">Optional</span><span class="sxs-lookup"><span data-stu-id="fa5c9-171">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="fa5c9-172">回應資料 (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="fa5c9-172">Response Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="fa5c9-173">Key</span><span class="sxs-lookup"><span data-stu-id="fa5c9-173">Key</span></span>               | <span data-ttu-id="fa5c9-174">值 （類型）</span><span class="sxs-lookup"><span data-stu-id="fa5c9-174">Value (type)</span></span> | <span data-ttu-id="fa5c9-175">說明</span><span class="sxs-lookup"><span data-stu-id="fa5c9-175">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="fa5c9-176">owin。ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="fa5c9-176">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="fa5c9-177">Optional</span><span class="sxs-lookup"><span data-stu-id="fa5c9-177">Optional</span></span> |
| <span data-ttu-id="fa5c9-178">owin。ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="fa5c9-178">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="fa5c9-179">Optional</span><span class="sxs-lookup"><span data-stu-id="fa5c9-179">Optional</span></span> |
| <span data-ttu-id="fa5c9-180">owin。ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="fa5c9-180">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="fa5c9-181">owin。ResponseBody</span><span class="sxs-lookup"><span data-stu-id="fa5c9-181">owin.ResponseBody</span></span> | `Stream`  | |


### <a name="other-data-owin-v100"></a><span data-ttu-id="fa5c9-182">其他資料 (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="fa5c9-182">Other Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="fa5c9-183">Key</span><span class="sxs-lookup"><span data-stu-id="fa5c9-183">Key</span></span>               | <span data-ttu-id="fa5c9-184">值 （類型）</span><span class="sxs-lookup"><span data-stu-id="fa5c9-184">Value (type)</span></span> | <span data-ttu-id="fa5c9-185">說明</span><span class="sxs-lookup"><span data-stu-id="fa5c9-185">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="fa5c9-186">owin。CallCancelled</span><span class="sxs-lookup"><span data-stu-id="fa5c9-186">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="fa5c9-187">owin。版本</span><span class="sxs-lookup"><span data-stu-id="fa5c9-187">owin.Version</span></span>  | `String` | |   


### <a name="common-keys"></a><span data-ttu-id="fa5c9-188">一般索引鍵</span><span class="sxs-lookup"><span data-stu-id="fa5c9-188">Common Keys</span></span>

| <span data-ttu-id="fa5c9-189">Key</span><span class="sxs-lookup"><span data-stu-id="fa5c9-189">Key</span></span>               | <span data-ttu-id="fa5c9-190">值 （類型）</span><span class="sxs-lookup"><span data-stu-id="fa5c9-190">Value (type)</span></span> | <span data-ttu-id="fa5c9-191">說明</span><span class="sxs-lookup"><span data-stu-id="fa5c9-191">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="fa5c9-192">ssl。ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="fa5c9-192">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="fa5c9-193">ssl。LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="fa5c9-193">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="fa5c9-194">伺服器。RemoteIpAddress</span><span class="sxs-lookup"><span data-stu-id="fa5c9-194">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="fa5c9-195">伺服器。RemotePort</span><span class="sxs-lookup"><span data-stu-id="fa5c9-195">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="fa5c9-196">伺服器。LocalIpAddress</span><span class="sxs-lookup"><span data-stu-id="fa5c9-196">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="fa5c9-197">伺服器。LocalPort</span><span class="sxs-lookup"><span data-stu-id="fa5c9-197">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="fa5c9-198">伺服器。IsLocal</span><span class="sxs-lookup"><span data-stu-id="fa5c9-198">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="fa5c9-199">伺服器。OnSendingHeaders</span><span class="sxs-lookup"><span data-stu-id="fa5c9-199">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a><span data-ttu-id="fa5c9-200">SendFiles v0.3.0</span><span class="sxs-lookup"><span data-stu-id="fa5c9-200">SendFiles v0.3.0</span></span>

| <span data-ttu-id="fa5c9-201">Key</span><span class="sxs-lookup"><span data-stu-id="fa5c9-201">Key</span></span>               | <span data-ttu-id="fa5c9-202">值 （類型）</span><span class="sxs-lookup"><span data-stu-id="fa5c9-202">Value (type)</span></span> | <span data-ttu-id="fa5c9-203">說明</span><span class="sxs-lookup"><span data-stu-id="fa5c9-203">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="fa5c9-204">sendfile。SendAsync</span><span class="sxs-lookup"><span data-stu-id="fa5c9-204">sendfile.SendAsync</span></span> | <span data-ttu-id="fa5c9-205">請參閱[委派簽章](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="fa5c9-205">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="fa5c9-206">每個要求</span><span class="sxs-lookup"><span data-stu-id="fa5c9-206">Per Request</span></span> |


### <a name="opaque-v030"></a><span data-ttu-id="fa5c9-207">不透明 v0.3.0</span><span class="sxs-lookup"><span data-stu-id="fa5c9-207">Opaque v0.3.0</span></span>

| <span data-ttu-id="fa5c9-208">Key</span><span class="sxs-lookup"><span data-stu-id="fa5c9-208">Key</span></span>               | <span data-ttu-id="fa5c9-209">值 （類型）</span><span class="sxs-lookup"><span data-stu-id="fa5c9-209">Value (type)</span></span> | <span data-ttu-id="fa5c9-210">說明</span><span class="sxs-lookup"><span data-stu-id="fa5c9-210">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="fa5c9-211">不透明的。版本</span><span class="sxs-lookup"><span data-stu-id="fa5c9-211">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="fa5c9-212">不透明的。升級</span><span class="sxs-lookup"><span data-stu-id="fa5c9-212">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="fa5c9-213">請參閱[委派簽章](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="fa5c9-213">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="fa5c9-214">不透明的。資料流</span><span class="sxs-lookup"><span data-stu-id="fa5c9-214">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="fa5c9-215">不透明的。CallCancelled</span><span class="sxs-lookup"><span data-stu-id="fa5c9-215">opaque.CallCancelled</span></span> | `CancellationToken` |  |


### <a name="websocket-v030"></a><span data-ttu-id="fa5c9-216">WebSocket v0.3.0</span><span class="sxs-lookup"><span data-stu-id="fa5c9-216">WebSocket v0.3.0</span></span>

| <span data-ttu-id="fa5c9-217">Key</span><span class="sxs-lookup"><span data-stu-id="fa5c9-217">Key</span></span>               | <span data-ttu-id="fa5c9-218">值 （類型）</span><span class="sxs-lookup"><span data-stu-id="fa5c9-218">Value (type)</span></span> | <span data-ttu-id="fa5c9-219">說明</span><span class="sxs-lookup"><span data-stu-id="fa5c9-219">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="fa5c9-220">websocket。版本</span><span class="sxs-lookup"><span data-stu-id="fa5c9-220">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="fa5c9-221">websocket。接受</span><span class="sxs-lookup"><span data-stu-id="fa5c9-221">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="fa5c9-222">請參閱[委派簽章](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="fa5c9-222">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="fa5c9-223">websocket。AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="fa5c9-223">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="fa5c9-224">非規格</span><span class="sxs-lookup"><span data-stu-id="fa5c9-224">Non-spec</span></span> |
| <span data-ttu-id="fa5c9-225">websocket。子通訊協定</span><span class="sxs-lookup"><span data-stu-id="fa5c9-225">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="fa5c9-226">請參閱[RFC6455 第 4.2.2 節](https://tools.ietf.org/html/rfc6455#section-4.2.2)步驟 5.5</span><span class="sxs-lookup"><span data-stu-id="fa5c9-226">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="fa5c9-227">websocket。SendAsync</span><span class="sxs-lookup"><span data-stu-id="fa5c9-227">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="fa5c9-228">請參閱[委派簽章](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="fa5c9-228">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="fa5c9-229">websocket。ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="fa5c9-229">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="fa5c9-230">請參閱[委派簽章](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="fa5c9-230">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="fa5c9-231">websocket。CloseAsync</span><span class="sxs-lookup"><span data-stu-id="fa5c9-231">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="fa5c9-232">請參閱[委派簽章](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="fa5c9-232">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="fa5c9-233">websocket。CallCancelled</span><span class="sxs-lookup"><span data-stu-id="fa5c9-233">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="fa5c9-234">websocket。ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="fa5c9-234">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="fa5c9-235">Optional</span><span class="sxs-lookup"><span data-stu-id="fa5c9-235">Optional</span></span> |
| <span data-ttu-id="fa5c9-236">websocket。ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="fa5c9-236">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="fa5c9-237">Optional</span><span class="sxs-lookup"><span data-stu-id="fa5c9-237">Optional</span></span> |


## <a name="additional-resources"></a><span data-ttu-id="fa5c9-238">其他資源</span><span class="sxs-lookup"><span data-stu-id="fa5c9-238">Additional Resources</span></span>

* [<span data-ttu-id="fa5c9-239">中介軟體</span><span class="sxs-lookup"><span data-stu-id="fa5c9-239">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="fa5c9-240">伺服器</span><span class="sxs-lookup"><span data-stu-id="fa5c9-240">Servers</span></span>](servers/index.md)
