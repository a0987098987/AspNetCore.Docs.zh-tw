---
title: ASP.NET Core Blazor 裝載模型設定
author: guardrex
description: 深入瞭解 ASP.NET Core 裝載模型設定的其他案例 Blazor 。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/10/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/fundamentals/additional-scenarios
ms.openlocfilehash: b28e4e43b88fcf8eab9e8959142cca21223c57ff
ms.sourcegitcommit: e216e8f4afa21215dc38124c28d5ee19f5ed7b1e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2020
ms.locfileid: "86239630"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a>ASP.NET Core Blazor 裝載模型設定

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

本文涵蓋主控模型設定。

### <a name="signalr-cross-origin-negotiation-for-authentication"></a>SignalR用於驗證的跨原始來源協調

*本節適用于 Blazor WebAssembly 。*

若要設定 SignalR 的基礎用戶端來傳送認證，例如 cookie 或 HTTP 驗證標頭：

* 用 <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.WebAssemblyHttpRequestMessageExtensions.SetBrowserRequestCredentials%2A> 來設定 <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.BrowserRequestCredentials.Include> 跨原始來源 [`fetch`](https://developer.mozilla.org/docs/Web/API/Fetch_API/Using_Fetch) 要求：

  ```csharp
  public class IncludeRequestCredentialsMessageHandler : DelegatingHandler
  {
      protected override Task<HttpResponseMessage> SendAsync(
          HttpRequestMessage request, CancellationToken cancellationToken)
      {
          request.SetBrowserRequestCredentials(BrowserRequestCredentials.Include);
          return base.SendAsync(request, cancellationToken);
      }
  }
  ```

* 將指派給 <xref:System.Net.Http.HttpMessageHandler> <xref:Microsoft.AspNetCore.Http.Connections.Client.HttpConnectionOptions.HttpMessageHandlerFactory> 選項：

  ```csharp
  var connection = new HubConnectionBuilder()
      .WithUrl(new Uri("http://signalr.example.com"), options =>
      {
          options.HttpMessageHandlerFactory = innerHandler => 
              new IncludeRequestCredentialsMessageHandler { InnerHandler = innerHandler };
      }).Build();
  ```

如需詳細資訊，請參閱 <xref:signalr/configuration#configure-additional-options> 。

## <a name="reflect-the-connection-state-in-the-ui"></a>反映 UI 中的連接狀態

*本節適用于 Blazor Server 。*

當用戶端偵測到連線已遺失時，會在用戶端嘗試重新連線時，向使用者顯示預設的 UI。 如果重新連線失敗，則會提供使用者重試的選項。

若要自訂 UI，請 `id` 在頁面的中定義具有之的元素 `components-reconnect-modal` `<body>` `_Host.cshtml` Razor ：

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

將下列內容新增至應用程式的樣式表單 (`wwwroot/css/app.css` 或 `wwwroot/css/site.css`) ：

```css
#components-reconnect-modal {
    display: none;
}

#components-reconnect-modal.components-reconnect-show {
    display: block;
}
```

下表描述套用至元素的 CSS 類別 `components-reconnect-modal` 。

| CSS 類別                       | 表示&hellip; |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | 遺失的連接。 用戶端正在嘗試重新連線。 顯示強制回應。 |
| `components-reconnect-hide`     | 系統會將使用中的連接重新建立至伺服器。 隱藏強制回應。 |
| `components-reconnect-failed`   | 重新連接失敗，可能是因為網路失敗。 若要嘗試重新連接，請呼叫 `window.Blazor.reconnect()` 。 |
| `components-reconnect-rejected` | 已拒絕重新連接。 已達到伺服器但拒絕連線，而且伺服器上的使用者狀態遺失。 若要重載應用程式，請呼叫 `location.reload()` 。 當下列情況發生時，可能會產生此連接狀態：<ul><li>伺服器端線路發生損毀。</li><li>用戶端中斷連線的時間夠長，伺服器才會捨棄使用者的狀態。 系統會處置與使用者互動之元件的實例。</li><li>伺服器會重新開機，或回收應用程式的背景工作進程。</li></ul> |

## <a name="render-mode"></a>轉譯模式

*本節適用于 Blazor Server 。*

Blazor Server在建立伺服器的用戶端連接之前，預設會將應用程式設定為在伺服器上預先呈現該 UI。 這會在頁面中設定 `_Host.cshtml` Razor ：

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper.RenderMode>設定元件是否：

* 會資源清單到頁面中。
* 會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動應用程式所需的資訊 Blazor 。

| 轉譯模式 | 描述 |
| --- | --- |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | 將元件轉譯為靜態 HTML，並包含 Blazor Server 應用程式的標記。 當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。 |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | 呈現應用程式的標記 Blazor Server 。 不包含來自元件的輸出。 當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。 |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | 將元件轉譯為靜態 HTML。 |

不支援從靜態 HTML 網頁轉譯伺服器元件。

## <a name="configure-the-signalr-client-for-blazor-server-apps"></a>設定 SignalR 應用程式的用戶端 Blazor Server

*本節適用于 Blazor Server 。*

設定 SignalR 應用程式在檔案中使用的用戶端 Blazor Server `Pages/_Host.cshtml` 。 將呼叫的腳本放在 `Blazor.start` `_framework/blazor.server.js` 腳本之後和標記內 `</body>` 。

### <a name="logging"></a>記錄

若要設定 SignalR 用戶端記錄：

* 將 `autostart="false"` 屬性加入至 `<script>` 腳本的標記 `blazor.server.js` 。
* 傳入設定物件 (在 `configureSignalR` 用戶端產生器 `configureLogging` 上通話記錄層級的) 。

```cshtml
    ...

    <script src="_framework/blazor.server.js" autostart="false"></script>
    <script>
      Blazor.start({
        configureSignalR: function (builder) {
          builder.configureLogging("information");
        }
      });
    </script>
</body>
```

在上述範例中， `information` 相當於的記錄層級 <xref:Microsoft.Extensions.Logging.LogLevel.Information?displayProperty=nameWithType> 。

### <a name="modify-the-reconnection-handler"></a>修改重新連接處理常式

重新連線處理常式的線路連接事件可以針對自訂行為進行修改，例如：

* 表示在連接中斷時通知使用者。
* 當線路連線時，從用戶端) 執行記錄 (。

若要修改連接事件：

* 將 `autostart="false"` 屬性加入至 `<script>` 腳本的標記 `blazor.server.js` 。
* 針對已卸載連接的連線變更註冊回呼， (`onConnectionDown`) ，並 () 建立/重新建立的連接 `onConnectionUp` 。 **兩者** `onConnectionDown``onConnectionUp`必須指定和。

```cshtml
    ...

    <script src="_framework/blazor.server.js" autostart="false"></script>
    <script>
      Blazor.start({
        reconnectionHandler: {
          onConnectionDown: (options, error) => console.error(error);
          onConnectionUp: () => console.log("Up, up, and away!");
        }
      });
    </script>
</body>
```

### <a name="adjust-the-reconnection-retry-count-and-interval"></a>調整重新連接重試計數和間隔

若要調整重新連接重試計數和間隔：

* 將 `autostart="false"` 屬性加入至 `<script>` 腳本的標記 `blazor.server.js` 。
* 設定 `maxRetries` 每次重試嘗試 () 的重試次數 () 和允許的期間（以毫秒為單位） `retryIntervalMilliseconds` 。

```cshtml
    ...

    <script src="_framework/blazor.server.js" autostart="false"></script>
    <script>
      Blazor.start({
        reconnectionOptions: {
          maxRetries: 3,
          retryIntervalMilliseconds: 2000
        }
      });
    </script>
</body>
```

### <a name="hide-or-replace-the-reconnection-display"></a>隱藏或取代重新連接顯示

若要隱藏重新連接顯示：

* 將 `autostart="false"` 屬性加入至 `<script>` 腳本的標記 `blazor.server.js` 。
* 將重新連接處理常式的 `_reconnectionDisplay` (或) 設定為空的物件 `{}` `new Object()` 。

```cshtml
    ...

    <script src="_framework/blazor.server.js" autostart="false"></script>
    <script>
      window.addEventListener('beforeunload', function () {
        Blazor.defaultReconnectionHandler._reconnectionDisplay = {};
      });
    </script>
</body>
```

若要取代重新連接顯示，請將 `_reconnectionDisplay` 前述範例中的設定為顯示的元素：

```javascript
Blazor.defaultReconnectionHandler._reconnectionDisplay = 
  document.getElementById("{ELEMENT ID}");
```

預留位置 `{ELEMENT ID}` 是要顯示之 HTML 專案的識別碼。

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/logging/index>
