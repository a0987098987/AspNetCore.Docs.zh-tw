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
# <a name="introduction-to-open-web-interface-for-net-owin"></a>若要開啟.NET (OWIN) 的網頁介面的簡介

由[Steve Smith](https://ardalis.com/)和[Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core 支援開啟 Web 介面.NET (OWIN)。 OWIN 可讓 web 應用程式從 web 伺服器。 它會定義可以在管線中用來處理要求和回應相關聯的中介軟體的標準方式。 與 OWIN 應用程式、 伺服器和中介軟體的 ASP.NET Core 應用程式和中介軟體可以交互操作。

OWIN 提供脫鉤層，可讓兩個架構來使用不同的物件模型一起使用。 `Microsoft.AspNetCore.Owin`套件提供兩個配接器實作：
- Owin ASP.NET Core 
- ASP.NET Core 的 OWIN

這可讓 ASP.NET Core 之上 OWIN 相容伺服器/主控件，或執行 ASP.NET Core 之上其他 OWIN 相容元件的裝載。

注意： 使用這些介面卡隨附的效能成本。 使用僅 ASP.NET 核心元件的應用程式不應該使用 Owin 封裝或介面卡。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample)

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a>在 ASP.NET 管線中執行 OWIN 中介軟體

ASP.NET Core OWIN 支援部署為一部分`Microsoft.AspNetCore.Owin`封裝。 您可以匯入您的專案的 OWIN 支援安裝此套件。

OWIN 中介軟體符合[OWIN 規格](http://owin.org/spec/spec/owin-1.0.0.html)，這項工作需要`Func<IDictionary<string, object>, Task>`設定介面和特定的索引鍵 (例如`owin.ResponseBody`)。 下列簡單的 OWIN 中介軟體會顯示"Hello World":

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

範例簽章傳回`Task`並接受`IDictionary<string, object>`依 OWIN。

下列程式碼示範如何加入`OwinHello`（如上所示） 具有的 ASP.NET 管線中介軟體`UseOwin`擴充方法。

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

您可以設定 OWIN 管線中進行其他動作。

> [!NOTE]
> 僅能在第一次寫入至回應資料流之前修改回應標頭。

> [!NOTE]
> 多個呼叫`UseOwin`建議您不要使用基於效能的考量。 如果群組在一起，OWIN 元件會以最能運作。

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

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a>使用 OWIN 架構的伺服器上的 ASP.NET 裝載

OWIN 伺服器可以裝載 ASP.NET 應用程式。 這類伺服器[Nowin](https://github.com/Bobris/Nowin)，.NET OWIN web 伺服器。 在本文範例中，包括了參考 Nowin 並使用它來建立的專案`IServer`能夠自我裝載的 ASP.NET Core。

[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]

`IServer`是需要的介面`Features`屬性和`Start`方法。

`Start`會負責設定和啟動伺服器，在此情況下透過一系列 fluent 應用程式開發的應用程式開發介面呼叫從 IServerAddressesFeature 設定剖析的位址。 請注意，fluent 應用程式開發的設定`_builder`變數會指定要求交由`appFunc`稍早在方法中定義。 這`Func`呼叫的每個要求來處理傳入的要求。

我們也可以加入`IWebHostBuilder`輕鬆加入及設定 Nowin 伺服器的延伸模組。

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

這個位置，所有的要求執行 ASP.NET 應用程式擴充功能呼叫中使用此自訂伺服器*Program.cs*:

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

深入了解 ASP.NET[伺服器](servers/index.md)。

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a>OWIN 架構的伺服器上執行 ASP.NET Core，並使用 WebSockets 的支援

如何 OWIN 架構伺服器功能的另一個範例可以利用 ASP.NET Core 是 WebSockets 等功能的存取。 在上述範例中使用.NET OWIN web 伺服器的 Web 通訊端中，建立以 ASP.NET Core 應用程式的支援。 下列範例顯示簡單的 web 應用程式支援 Web 通訊端及回應透過 Websocket 傳送的所有項目。

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

這[範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample)已設定為使用相同`NowinServer`唯一的差別是應用程式中的設定方式與上一個為其`Configure`方法。 使用測試[簡單 websocket 用戶端](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en)示範應用程式：

![Web 通訊端測試用戶端](owin/_static/websocket-test.png)

## <a name="owin-environment"></a>OWIN 環境

您可以建構的 OWIN 環境 using `HttpContext`。

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a>OWIN 索引鍵

OWIN 取決於`IDictionary<string,object>`通訊內的 HTTP 要求/回應交換資訊的物件。 ASP.NET Core 實作下面所列的索引鍵。 請參閱[規格的主要、 延伸](http://owin.org/#spec)，和[OWIN 索引鍵指導方針和一般索引鍵](http://owin.org/spec/spec/CommonKeys.html)。

### <a name="request-data-owin-v100"></a>要求資料 (OWIN v1.0.0)

| Key               | 值 （類型） | 說明 |
| ----------------- | ------------ | ----------- |
| owin。RequestScheme | `String` |  |
| owin。RequestMethod  | `String` | |    
| owin。RequestPathBase  | `String` | |    
| owin。RequestPath | `String` | |     
| owin。RequestQueryString  | `String` | |    
| owin。RequestProtocol  | `String` | |    
| owin。RequestHeaders | `IDictionary<string,string[]>`  | |
| owin。RequestBody | `Stream`  | |

### <a name="request-data-owin-v110"></a>要求資料 (OWIN v1.1.0)

| Key               | 值 （類型） | 說明 |
| ----------------- | ------------ | ----------- |
| owin。RequestId | `String` | Optional |

### <a name="response-data-owin-v100"></a>回應資料 (OWIN v1.0.0)

| Key               | 值 （類型） | 說明 |
| ----------------- | ------------ | ----------- |
| owin。ResponseStatusCode | `int` | Optional |
| owin。ResponseReasonPhrase | `String` | Optional |
| owin。ResponseHeaders | `IDictionary<string,string[]>`  | |
| owin。ResponseBody | `Stream`  | |


### <a name="other-data-owin-v100"></a>其他資料 (OWIN v1.0.0)

| Key               | 值 （類型） | 說明 |
| ----------------- | ------------ | ----------- |
| owin。CallCancelled | `CancellationToken` |  |
| owin。版本  | `String` | |   


### <a name="common-keys"></a>一般索引鍵

| Key               | 值 （類型） | 說明 |
| ----------------- | ------------ | ----------- |
| ssl。ClientCertificate | `X509Certificate` |  |
| ssl。LoadClientCertAsync  | `Func<Task>` | |    
| 伺服器。RemoteIpAddress  | `String` | |    
| 伺服器。RemotePort | `String` | |     
| 伺服器。LocalIpAddress  | `String` | |    
| 伺服器。LocalPort  | `String` | |    
| 伺服器。IsLocal  | `bool` | |    
| 伺服器。OnSendingHeaders  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a>SendFiles v0.3.0

| Key               | 值 （類型） | 說明 |
| ----------------- | ------------ | ----------- |
| sendfile。SendAsync | 請參閱[委派簽章](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) | 每個要求 |


### <a name="opaque-v030"></a>不透明 v0.3.0

| Key               | 值 （類型） | 說明 |
| ----------------- | ------------ | ----------- |
| 不透明的。版本 | `String` |  |
| 不透明的。升級 | `OpaqueUpgrade` | 請參閱[委派簽章](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| 不透明的。資料流 | `Stream` |  |
| 不透明的。CallCancelled | `CancellationToken` |  |


### <a name="websocket-v030"></a>WebSocket v0.3.0

| Key               | 值 （類型） | 說明 |
| ----------------- | ------------ | ----------- |
| websocket。版本 | `String` |  |
| websocket。接受 | `WebSocketAccept` | 請參閱[委派簽章](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| websocket。AcceptAlt |  | 非規格 |
| websocket。子通訊協定 | `String` | 請參閱[RFC6455 第 4.2.2 節](https://tools.ietf.org/html/rfc6455#section-4.2.2)步驟 5.5 |
| websocket。SendAsync | `WebSocketSendAsync` | 請參閱[委派簽章](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket。ReceiveAsync | `WebSocketReceiveAsync` | 請參閱[委派簽章](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket。CloseAsync | `WebSocketCloseAsync` | 請參閱[委派簽章](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket。CallCancelled | `CancellationToken` |  |
| websocket。ClientCloseStatus | `int` | Optional |
| websocket。ClientCloseDescription | `String` | Optional |


## <a name="additional-resources"></a>其他資源

* [中介軟體](middleware.md)

* [伺服器](servers/index.md)
