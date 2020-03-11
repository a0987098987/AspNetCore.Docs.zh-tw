---
title: 具有 ASP.NET Core 的 Open Web Interface for .NET (OWIN)
author: ardalis
description: 探索 ASP.NET Core 如何支援 Open Web Interface for .NET (OWIN)，這可讓 Web 應用程式與網頁伺服器分離。
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 12/18/2018
uid: fundamentals/owin
ms.openlocfilehash: 14b23ba6d284413e20417bbd4142e19a656350ac
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666681"
---
# <a name="open-web-interface-for-net-owin-with-aspnet-core"></a>具有 ASP.NET Core 的 Open Web Interface for .NET (OWIN)

作者：[Steve Smith](https://ardalis.com/) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core 支援Open Web Interface for .NET (OWIN)。 OWIN 可讓 Web 應用程式獨立於網頁伺服器。 它會定義中介軟體要在管線中用來處理要求和相關聯回應的標準方式。 ASP.NET Core 應用程式和中介軟體可以與以 OWIN 為基礎的應用程式、伺服器及中介軟體進行交互操作。

OWIN 提供分離層，可讓兩個利用不同物件模型的架構一起使用。 `Microsoft.AspNetCore.Owin` 套件提供兩個配接器實作：

* ASP.NET Core 至 OWIN 
* OWIN 至 ASP.NET Core

這可讓 ASP.NET Core 裝載在 OWIN 相容的伺服器/主機之上，或讓其他 OWIN 相容的元件在 ASP.NET Core 之上執行。

> [!NOTE]
> 使用這些配接器將伴隨效能成本增加。 僅使用 ASP.NET Core 元件的應用程式不應使用 `Microsoft.AspNetCore.Owin` 套件或配接器。

[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/owin/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="running-owin-middleware-in-the-aspnet-core-pipeline"></a>在 ASP.NET Core 管線中執行 OWIN 中介軟體

ASP.NET Core 的 OWIN 支援部署為 `Microsoft.AspNetCore.Owin` 套件的一部分。 您可以藉由安裝此套件，將 OWIN 支援匯入您的專案中。

OWIN 中介軟體符合 [OWIN 規格](https://owin.org/spec/spec/owin-1.0.0.html)，它需要設定 `Func<IDictionary<string, object>, Task>` 介面和特定的索引鍵 (例如 `owin.ResponseBody`)。 下列簡單的 OWIN 中介軟體會顯示 "Hello World"：

```csharp
public Task OwinHello(IDictionary<string, object> environment)
{
    string responseText = "Hello World via OWIN";
    byte[] responseBytes = Encoding.UTF8.GetBytes(responseText);

    // OWIN Environment Keys: https://owin.org/spec/spec/owin-1.0.0.html
    var responseStream = (Stream)environment["owin.ResponseBody"];
    var responseHeaders = (IDictionary<string, string[]>)environment["owin.ResponseHeaders"];

    responseHeaders["Content-Length"] = new string[] { responseBytes.Length.ToString(CultureInfo.InvariantCulture) };
    responseHeaders["Content-Type"] = new string[] { "text/plain" };

    return responseStream.WriteAsync(responseBytes, 0, responseBytes.Length);
}
```

範例簽章會傳回 `Task`，並依照 OWIN 的要求接受 `IDictionary<string, object>`。

下列程式碼示範如何使用 `OwinHello` 擴充方法，將 `UseOwin` 中介軟體 (如上所示) 新增至 ASP.NET Core 管線。

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

您可以設定在 OWIN 管線內進行其他動作。

> [!NOTE]
> 只能在第一次寫入回應資料流之前修改回應標頭。

> [!NOTE]
> 基於效能考量，建議您不要多次呼叫 `UseOwin`。 OWIN 元件如果群組在一起，其運作效能最佳。

```csharp
app.UseOwin(pipeline =>
{
    pipeline(next =>
    {
        return async environment =>
        {
            // Do something before.
            await next(environment);
            // Do something after.
        };
    });
});
```

<a name="hosting-on-owin"></a>

## <a name="using-aspnet-core-hosting-on-an-owin-based-server"></a>使用以 OWIN 為基礎的伺服器上所裝載的 ASP.NET Core

以 OWIN 為基礎的伺服器可以裝載 ASP.NET Core 應用程式。 其中一個這類伺服器是 [Nowin](https://github.com/Bobris/Nowin)，其為 .NET OWIN 網頁伺服器。 在本文的範例中，包含了參考 Nowin 並使用它來建立 `IServer` 能夠自我裝載之 ASP.NET Core 的專案。

[!code-csharp[](owin/sample/src/NowinSample/Program.cs?highlight=15)]

`IServer` 是需要 `Features` 屬性和 `Start` 方法的介面。

`Start` 負責設定和啟動伺服器，在此情況下，這會透過一系列 Fluent API 呼叫來完成，而這些呼叫設定從 IServerAddressesFeature 剖析的位址。 請注意，`_builder` 變數的 Fluent 組態會指定要求將由稍早在方法中定義的 `appFunc` 處理。 每個要求都會呼叫這個 `Func` 來處理傳入的要求。

我們也將新增 `IWebHostBuilder` 延伸模組，以便能夠輕鬆地新增和設定 Nowin 伺服器。

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

完成上述作業後，請叫用 *Program.cs* 中的延伸模組，以使用這個自訂伺服器來執行 ASP.NET Core 應用程式：

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

深入了解 [ASP.NET Core 伺服器](xref:fundamentals/servers/index)。

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a>在以 OWIN 為基礎的伺服器上執行 ASP.NET Core 並使用其 WebSocket 支援

ASP.NET Core 如何利用以 OWIN 為基礎之伺服器功能的另一個範例是存取 WebSocket 等功能。 在上述範例中使用的 .NET OWIN 網頁伺服器支援內建的 Web 通訊端，供 ASP.NET Core 應用程式加以利用。 下列範例顯示的簡單 Web 應用程式支援 Web 通訊端，並透過 Websocket 回應傳送至伺服器的所有項目。

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

此[範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/owin/sample)使用與上述相同的 `NowinServer` 進行設定 - 唯一的差異是在其 `Configure` 方法中設定應用程式的方式。 使用[簡單 Websocket 用戶端](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en)的測試示範此應用程式：

![Web 通訊端測試用戶端](owin/_static/websocket-test.png)

## <a name="owin-environment"></a>OWIN 環境

您可以使用 `HttpContext` 建構 OWIN 環境。

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a>OWIN 索引鍵

OWIN 仰賴 `IDictionary<string,object>` 物件在 HTTP 要求/回應交換中傳遞資訊。 ASP.NET Core 會實作下面所列的索引鍵。 請參閱[主要規格、模組延伸](https://owin.org/#spec)和 [OWIN Key Guidelines and Common Keys](https://owin.org/spec/spec/CommonKeys.html) (OWIN 索引鍵指導方針和共用索引鍵)。

### <a name="request-data-owin-v100"></a>要求資料 (OWIN 1.0.0 版)

| Key               | 值 (類型) | 描述 |
| ----------------- | ------------ | ----------- |
| owin.RequestScheme | `String` |  |
| owin.RequestMethod  | `String` | |    
| owin.RequestPathBase  | `String` | |    
| owin.RequestPath | `String` | |     
| owin.RequestQueryString  | `String` | |    
| owin.RequestProtocol  | `String` | |    
| owin.RequestHeaders | `IDictionary<string,string[]>`  | |
| owin.RequestBody | `Stream`  | |

### <a name="request-data-owin-v110"></a>要求資料 (OWIN 1.1.0 版)

| Key               | 值 (類型) | 描述 |
| ----------------- | ------------ | ----------- |
| owin.RequestId | `String` | 選用 |

### <a name="response-data-owin-v100"></a>回應資料 (OWIN 1.0.0 版)

| Key               | 值 (類型) | 描述 |
| ----------------- | ------------ | ----------- |
| owin.ResponseStatusCode | `int` | 選用 |
| owin.ResponseReasonPhrase | `String` | 選用 |
| owin.ResponseHeaders | `IDictionary<string,string[]>`  | |
| owin.ResponseBody | `Stream`  | |

### <a name="other-data-owin-v100"></a>其他資料 (OWIN 1.0.0 版)

| Key               | 值 (類型) | 描述 |
| ----------------- | ------------ | ----------- |
| owin.CallCancelled | `CancellationToken` |  |
| owin.Version  | `String` | |   

### <a name="common-keys"></a>共同索引鍵

| Key               | 值 (類型) | 描述 |
| ----------------- | ------------ | ----------- |
| ssl.ClientCertificate | `X509Certificate` |  |
| ssl.LoadClientCertAsync  | `Func<Task>` | |    
| server.RemoteIpAddress  | `String` | |    
| server.RemotePort | `String` | |     
| server.LocalIpAddress  | `String` | |    
| server.LocalPort  | `String` | |    
| server.IsLocal  | `bool` | |    
| server.OnSendingHeaders  | `Action<Action<object>,object>` | |

### <a name="sendfiles-v030"></a>SendFiles 0.3.0 版

| Key               | 值 (類型) | 描述 |
| ----------------- | ------------ | ----------- |
| sendfile.SendAsync | 請參閱[委派簽章](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) | 每個要求 |

### <a name="opaque-v030"></a>Opaque 0.3.0 版

| Key               | 值 (類型) | 描述 |
| ----------------- | ------------ | ----------- |
| opaque.Version | `String` |  |
| opaque.Upgrade | `OpaqueUpgrade` | 請參閱[委派簽章](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| opaque.Stream | `Stream` |  |
| opaque.CallCancelled | `CancellationToken` |  |

### <a name="websocket-v030"></a>WebSocket 0.3.0 版

| Key               | 值 (類型) | 描述 |
| ----------------- | ------------ | ----------- |
| websocket.Version | `String` |  |
| websocket.Accept | `WebSocketAccept` | 請參閱[委派簽章](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| websocket.AcceptAlt |  | 非規格 |
| websocket.SubProtocol | `String` | 請參閱 [RFC6455 4.2.2 節](https://tools.ietf.org/html/rfc6455#section-4.2.2)的步驟 5.5 |
| websocket.SendAsync | `WebSocketSendAsync` | 請參閱[委派簽章](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.ReceiveAsync | `WebSocketReceiveAsync` | 請參閱[委派簽章](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.CloseAsync | `WebSocketCloseAsync` | 請參閱[委派簽章](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.CallCancelled | `CancellationToken` |  |
| websocket.ClientCloseStatus | `int` | 選用 |
| websocket.ClientCloseDescription | `String` | 選用 |

## <a name="additional-resources"></a>其他資源

* [中介軟體](xref:fundamentals/middleware/index)
* [伺服器](xref:fundamentals/servers/index)
