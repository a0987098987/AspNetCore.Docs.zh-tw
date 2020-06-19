---
title: ASP.NET Core Blazor 裝載模型設定
author: guardrex
description: 深入瞭解 ASP.NET Core 裝載模型設定的其他案例 Blazor 。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/10/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/fundamentals/additional-scenarios
ms.openlocfilehash: 726aafd2bf5d3469c30ebce1e4eea8ed8ec8d58e
ms.sourcegitcommit: 490434a700ba8c5ed24d849bd99d8489858538e3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/19/2020
ms.locfileid: "85103623"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a>ASP.NET Core Blazor 裝載模型設定

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

本文涵蓋主控模型設定。

### <a name="signalr-cross-origin-negotiation-for-authentication"></a>SignalR用於驗證的跨原始來源協調

*本節適用于 Blazor WebAssembly。*

若要設定 SignalR 的基礎用戶端來傳送認證，例如 cookie 或 HTTP 驗證標頭：

* 用 <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.WebAssemblyHttpRequestMessageExtensions.SetBrowserRequestCredentials%2A> 來設定 <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.BrowserRequestCredentials.Include> 跨原始來源[提取](https://developer.mozilla.org/docs/Web/API/Fetch_API/Using_Fetch)要求：

  ```csharp
  public class IncludeRequestCredentialsMessagHandler : DelegatingHandler
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
              new IncludeRequestCredentialsMessagHandler { InnerHandler = innerHandler };
      }).Build();
  ```

如需詳細資訊，請參閱 <xref:signalr/configuration#configure-additional-options> 。

## <a name="reflect-the-connection-state-in-the-ui"></a>反映 UI 中的連接狀態

*本節適用于 Blazor 伺服器。*

當用戶端偵測到連線已遺失時，會在用戶端嘗試重新連線時，向使用者顯示預設的 UI。 如果重新連線失敗，則會提供使用者重試的選項。

若要自訂 UI，請 `id` `components-reconnect-modal` 在 [ `<body>` *_Host. cshtml* ] 頁面的中，定義具有之的元素 Razor ：

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

下表描述套用至元素的 CSS 類別 `components-reconnect-modal` 。

| CSS 類別                       | 表示&hellip; |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | 遺失的連接。 用戶端正在嘗試重新連線。 顯示強制回應。 |
| `components-reconnect-hide`     | 系統會將使用中的連接重新建立至伺服器。 隱藏強制回應。 |
| `components-reconnect-failed`   | 重新連接失敗，可能是因為網路失敗。 若要嘗試重新連接，請呼叫 `window.Blazor.reconnect()` 。 |
| `components-reconnect-rejected` | 已拒絕重新連接。 已達到伺服器但拒絕連線，而且伺服器上的使用者狀態遺失。 若要重載應用程式，請呼叫 `location.reload()` 。 當下列情況發生時，可能會產生此連接狀態：<ul><li>伺服器端線路發生損毀。</li><li>用戶端中斷連線的時間夠長，伺服器才會捨棄使用者的狀態。 系統會處置與使用者互動之元件的實例。</li><li>伺服器會重新開機，或回收應用程式的背景工作進程。</li></ul> |

## <a name="render-mode"></a>轉譯模式

*本節適用于 Blazor 伺服器。*

Blazor伺服器應用程式預設會設定為伺服器上預先呈現的 UI，然後才會建立與伺服器的用戶端連接。 這會在 [ *_Host. cshtml* ] 頁面中設定 Razor ：

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

| <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper.RenderMode> | 描述 |
| --- | --- |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | 將元件轉譯為靜態 HTML，並包含 Blazor 伺服器應用程式的標記。 當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。 |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | 呈現 Blazor 伺服器應用程式的標記。 不包含來自元件的輸出。 當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。 |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | 將元件轉譯為靜態 HTML。 |

不支援從靜態 HTML 網頁轉譯伺服器元件。

## <a name="configure-the-signalr-client-for-blazor-server-apps"></a>設定 SignalR Blazor 伺服器應用程式的用戶端

*本節適用于 Blazor 伺服器。*

有時候，您需要設定 SignalR 伺服器應用程式所使用的用戶端 Blazor 。 例如，您可能會想要在用戶端上設定記錄 SignalR 來診斷連線問題。

若要 SignalR 在*Pages/_Host. cshtml*檔案中設定用戶端：

* 將 `autostart="false"` 屬性加入至 `<script>` 腳本的標記 `blazor.server.js` 。
* 呼叫 `Blazor.start` 並傳入指定產生器的設定物件 SignalR 。

```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging("information"); // LogLevel.Information
    }
  });
</script>
```

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/logging/index>
