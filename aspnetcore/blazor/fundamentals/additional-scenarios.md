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
ms.openlocfilehash: 2efc13d5d4ab91ffdf6c4c7021072a2b3f83153f
ms.sourcegitcommit: 066d66ea150f8aab63f9e0e0668b06c9426296fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/23/2020
ms.locfileid: "85242650"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="31adf-103">ASP.NET Core Blazor 裝載模型設定</span><span class="sxs-lookup"><span data-stu-id="31adf-103">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="31adf-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="31adf-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="31adf-105">本文涵蓋主控模型設定。</span><span class="sxs-lookup"><span data-stu-id="31adf-105">This article covers hosting model configuration.</span></span>

### <a name="signalr-cross-origin-negotiation-for-authentication"></a>SignalR<span data-ttu-id="31adf-106">用於驗證的跨原始來源協調</span><span class="sxs-lookup"><span data-stu-id="31adf-106"> cross-origin negotiation for authentication</span></span>

<span data-ttu-id="31adf-107">*本節適用于 Blazor WebAssembly。*</span><span class="sxs-lookup"><span data-stu-id="31adf-107">*This section applies to Blazor WebAssembly.*</span></span>

<span data-ttu-id="31adf-108">若要設定 SignalR 的基礎用戶端來傳送認證，例如 cookie 或 HTTP 驗證標頭：</span><span class="sxs-lookup"><span data-stu-id="31adf-108">To configure SignalR's underlying client to send credentials, such as cookies or HTTP authentication headers:</span></span>

* <span data-ttu-id="31adf-109">用 <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.WebAssemblyHttpRequestMessageExtensions.SetBrowserRequestCredentials%2A> 來設定 <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.BrowserRequestCredentials.Include> 跨原始來源 [`fetch`](https://developer.mozilla.org/docs/Web/API/Fetch_API/Using_Fetch) 要求：</span><span class="sxs-lookup"><span data-stu-id="31adf-109">Use <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.WebAssemblyHttpRequestMessageExtensions.SetBrowserRequestCredentials%2A> to set <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.BrowserRequestCredentials.Include> on cross-origin [`fetch`](https://developer.mozilla.org/docs/Web/API/Fetch_API/Using_Fetch) requests:</span></span>

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

* <span data-ttu-id="31adf-110">將指派給 <xref:System.Net.Http.HttpMessageHandler> <xref:Microsoft.AspNetCore.Http.Connections.Client.HttpConnectionOptions.HttpMessageHandlerFactory> 選項：</span><span class="sxs-lookup"><span data-stu-id="31adf-110">Assign the <xref:System.Net.Http.HttpMessageHandler> to the <xref:Microsoft.AspNetCore.Http.Connections.Client.HttpConnectionOptions.HttpMessageHandlerFactory> option:</span></span>

  ```csharp
  var connection = new HubConnectionBuilder()
      .WithUrl(new Uri("http://signalr.example.com"), options =>
      {
          options.HttpMessageHandlerFactory = innerHandler => 
              new IncludeRequestCredentialsMessagHandler { InnerHandler = innerHandler };
      }).Build();
  ```

<span data-ttu-id="31adf-111">如需詳細資訊，請參閱 <xref:signalr/configuration#configure-additional-options>。</span><span class="sxs-lookup"><span data-stu-id="31adf-111">For more information, see <xref:signalr/configuration#configure-additional-options>.</span></span>

## <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="31adf-112">反映 UI 中的連接狀態</span><span class="sxs-lookup"><span data-stu-id="31adf-112">Reflect the connection state in the UI</span></span>

<span data-ttu-id="31adf-113">*本節適用于 Blazor 伺服器。*</span><span class="sxs-lookup"><span data-stu-id="31adf-113">*This section applies to Blazor Server.*</span></span>

<span data-ttu-id="31adf-114">當用戶端偵測到連線已遺失時，會在用戶端嘗試重新連線時，向使用者顯示預設的 UI。</span><span class="sxs-lookup"><span data-stu-id="31adf-114">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="31adf-115">如果重新連線失敗，則會提供使用者重試的選項。</span><span class="sxs-lookup"><span data-stu-id="31adf-115">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="31adf-116">若要自訂 UI，請 `id` 在頁面的中定義具有之的元素 `components-reconnect-modal` `<body>` `_Host.cshtml` Razor ：</span><span class="sxs-lookup"><span data-stu-id="31adf-116">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the `_Host.cshtml` Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="31adf-117">下表描述套用至元素的 CSS 類別 `components-reconnect-modal` 。</span><span class="sxs-lookup"><span data-stu-id="31adf-117">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="31adf-118">CSS 類別</span><span class="sxs-lookup"><span data-stu-id="31adf-118">CSS class</span></span>                       | <span data-ttu-id="31adf-119">表示&hellip;</span><span class="sxs-lookup"><span data-stu-id="31adf-119">Indicates&hellip;</span></span> |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | <span data-ttu-id="31adf-120">遺失的連接。</span><span class="sxs-lookup"><span data-stu-id="31adf-120">A lost connection.</span></span> <span data-ttu-id="31adf-121">用戶端正在嘗試重新連線。</span><span class="sxs-lookup"><span data-stu-id="31adf-121">The client is attempting to reconnect.</span></span> <span data-ttu-id="31adf-122">顯示強制回應。</span><span class="sxs-lookup"><span data-stu-id="31adf-122">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="31adf-123">系統會將使用中的連接重新建立至伺服器。</span><span class="sxs-lookup"><span data-stu-id="31adf-123">An active connection is re-established to the server.</span></span> <span data-ttu-id="31adf-124">隱藏強制回應。</span><span class="sxs-lookup"><span data-stu-id="31adf-124">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="31adf-125">重新連接失敗，可能是因為網路失敗。</span><span class="sxs-lookup"><span data-stu-id="31adf-125">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="31adf-126">若要嘗試重新連接，請呼叫 `window.Blazor.reconnect()` 。</span><span class="sxs-lookup"><span data-stu-id="31adf-126">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="31adf-127">已拒絕重新連接。</span><span class="sxs-lookup"><span data-stu-id="31adf-127">Reconnection rejected.</span></span> <span data-ttu-id="31adf-128">已達到伺服器但拒絕連線，而且伺服器上的使用者狀態遺失。</span><span class="sxs-lookup"><span data-stu-id="31adf-128">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="31adf-129">若要重載應用程式，請呼叫 `location.reload()` 。</span><span class="sxs-lookup"><span data-stu-id="31adf-129">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="31adf-130">當下列情況發生時，可能會產生此連接狀態：</span><span class="sxs-lookup"><span data-stu-id="31adf-130">This connection state may result when:</span></span><ul><li><span data-ttu-id="31adf-131">伺服器端線路發生損毀。</span><span class="sxs-lookup"><span data-stu-id="31adf-131">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="31adf-132">用戶端中斷連線的時間夠長，伺服器才會捨棄使用者的狀態。</span><span class="sxs-lookup"><span data-stu-id="31adf-132">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="31adf-133">系統會處置與使用者互動之元件的實例。</span><span class="sxs-lookup"><span data-stu-id="31adf-133">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="31adf-134">伺服器會重新開機，或回收應用程式的背景工作進程。</span><span class="sxs-lookup"><span data-stu-id="31adf-134">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

## <a name="render-mode"></a><span data-ttu-id="31adf-135">轉譯模式</span><span class="sxs-lookup"><span data-stu-id="31adf-135">Render mode</span></span>

<span data-ttu-id="31adf-136">*本節適用于 Blazor 伺服器。*</span><span class="sxs-lookup"><span data-stu-id="31adf-136">*This section applies to Blazor Server.*</span></span>

Blazor<span data-ttu-id="31adf-137">伺服器應用程式預設會設定為伺服器上預先呈現的 UI，然後才會建立與伺服器的用戶端連接。</span><span class="sxs-lookup"><span data-stu-id="31adf-137"> Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="31adf-138">這會在頁面中設定 `_Host.cshtml` Razor ：</span><span class="sxs-lookup"><span data-stu-id="31adf-138">This is set up in the `_Host.cshtml` Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="31adf-139"><xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper.RenderMode>設定元件是否：</span><span class="sxs-lookup"><span data-stu-id="31adf-139"><xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper.RenderMode> configures whether the component:</span></span>

* <span data-ttu-id="31adf-140">會資源清單到頁面中。</span><span class="sxs-lookup"><span data-stu-id="31adf-140">Is prerendered into the page.</span></span>
* <span data-ttu-id="31adf-141">會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動應用程式所需的資訊 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="31adf-141">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper.RenderMode> | <span data-ttu-id="31adf-142">描述</span><span class="sxs-lookup"><span data-stu-id="31adf-142">Description</span></span> |
| --- | --- |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | <span data-ttu-id="31adf-143">將元件轉譯為靜態 HTML，並包含 Blazor 伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="31adf-143">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="31adf-144">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="31adf-144">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | <span data-ttu-id="31adf-145">呈現 Blazor 伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="31adf-145">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="31adf-146">不包含來自元件的輸出。</span><span class="sxs-lookup"><span data-stu-id="31adf-146">Output from the component isn't included.</span></span> <span data-ttu-id="31adf-147">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="31adf-147">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | <span data-ttu-id="31adf-148">將元件轉譯為靜態 HTML。</span><span class="sxs-lookup"><span data-stu-id="31adf-148">Renders the component into static HTML.</span></span> |

<span data-ttu-id="31adf-149">不支援從靜態 HTML 網頁轉譯伺服器元件。</span><span class="sxs-lookup"><span data-stu-id="31adf-149">Rendering server components from a static HTML page isn't supported.</span></span>

## <a name="configure-the-signalr-client-for-blazor-server-apps"></a><span data-ttu-id="31adf-150">設定 SignalR Blazor 伺服器應用程式的用戶端</span><span class="sxs-lookup"><span data-stu-id="31adf-150">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="31adf-151">*本節適用于 Blazor 伺服器。*</span><span class="sxs-lookup"><span data-stu-id="31adf-151">*This section applies to Blazor Server.*</span></span>

<span data-ttu-id="31adf-152">有時候，您需要設定 SignalR 伺服器應用程式所使用的用戶端 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="31adf-152">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="31adf-153">例如，您可能會想要在用戶端上設定記錄 SignalR 來診斷連線問題。</span><span class="sxs-lookup"><span data-stu-id="31adf-153">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="31adf-154">若要 SignalR 在檔案中設定用戶端 `Pages/_Host.cshtml` ：</span><span class="sxs-lookup"><span data-stu-id="31adf-154">To configure the SignalR client in the `Pages/_Host.cshtml` file:</span></span>

* <span data-ttu-id="31adf-155">將 `autostart="false"` 屬性加入至 `<script>` 腳本的標記 `blazor.server.js` 。</span><span class="sxs-lookup"><span data-stu-id="31adf-155">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="31adf-156">呼叫 `Blazor.start` 並傳入指定產生器的設定物件 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="31adf-156">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="31adf-157">其他資源</span><span class="sxs-lookup"><span data-stu-id="31adf-157">Additional resources</span></span>

* <xref:fundamentals/logging/index>
