---
title: ASP.NET Core Blazor 裝載模型設定
author: guardrex
description: 深入瞭解 Blazor 裝載模型設定，包括如何將 Razor 元件整合到 Razor Pages 和 MVC 應用程式中。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/24/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: 1f71ac63bbe9dc9d56cfca2ded19a5b863be828f
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306426"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="64196-103">ASP.NET Core Blazor 裝載模型設定</span><span class="sxs-lookup"><span data-stu-id="64196-103">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="64196-104">依[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="64196-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="64196-105">本文涵蓋主控模型設定。</span><span class="sxs-lookup"><span data-stu-id="64196-105">This article covers hosting model configuration.</span></span>

## <a name="blazor-webassembly"></a><span data-ttu-id="64196-106">Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="64196-106">Blazor WebAssembly</span></span>

<span data-ttu-id="64196-107">從 ASP.NET Core 3.2 Preview 3 版本開始，Blazor WebAssembly 支援從下列設定：</span><span class="sxs-lookup"><span data-stu-id="64196-107">As of the ASP.NET Core 3.2 Preview 3 release, Blazor WebAssembly supports configuration from:</span></span>

* <span data-ttu-id="64196-108">*wwwroot/appsettings. json*</span><span class="sxs-lookup"><span data-stu-id="64196-108">*wwwroot/appsettings.json*</span></span>
* <span data-ttu-id="64196-109">*wwwroot/appsettings。{環境}. json*</span><span class="sxs-lookup"><span data-stu-id="64196-109">*wwwroot/appsettings.{ENVIRONMENT}.json*</span></span>

<span data-ttu-id="64196-110">在 Blazor 裝載的應用程式中，[執行時間環境](xref:fundamentals/environments)與伺服器應用程式的值相同。</span><span class="sxs-lookup"><span data-stu-id="64196-110">In a Blazor Hosted app, the [runtime environment](xref:fundamentals/environments) is the same as the server app's value.</span></span>

<span data-ttu-id="64196-111">在本機執行應用程式時，環境會預設為開發。</span><span class="sxs-lookup"><span data-stu-id="64196-111">When running the app locally, the environment defaults to Development.</span></span> <span data-ttu-id="64196-112">當應用程式發佈時，環境會預設為 [生產]。</span><span class="sxs-lookup"><span data-stu-id="64196-112">When the app is published, the environment defaults to Production.</span></span> <span data-ttu-id="64196-113">如需詳細資訊，包括如何設定環境，請參閱 <xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="64196-113">For more information, including how to configure the environment, see <xref:fundamentals/environments>.</span></span>

> [!WARNING]
> <span data-ttu-id="64196-114">使用者可以看到 Blazor WebAssembly 應用程式中的設定。</span><span class="sxs-lookup"><span data-stu-id="64196-114">Configuration in a Blazor WebAssembly app is visible to users.</span></span> <span data-ttu-id="64196-115">**請勿在設定中儲存應用程式秘密或認證。**</span><span class="sxs-lookup"><span data-stu-id="64196-115">**Don't store app secrets or credentials in configuration.**</span></span>

<span data-ttu-id="64196-116">系統會快取設定檔以供離線使用。</span><span class="sxs-lookup"><span data-stu-id="64196-116">Configuration files are cached for offline use.</span></span> <span data-ttu-id="64196-117">使用[漸進式 Web 應用程式（pwa）](xref:blazor/progressive-web-app)，您只能在建立新的部署時更新設定檔。</span><span class="sxs-lookup"><span data-stu-id="64196-117">With [Progressive Web Applications (PWAs)](xref:blazor/progressive-web-app), you can only update configuration files when creating a new deployment.</span></span> <span data-ttu-id="64196-118">在部署之間編輯設定檔沒有任何作用，因為：</span><span class="sxs-lookup"><span data-stu-id="64196-118">Editing configuration files between deployments has no effect because:</span></span>

* <span data-ttu-id="64196-119">使用者有檔案的快取版本，這些檔案會繼續使用。</span><span class="sxs-lookup"><span data-stu-id="64196-119">Users have cached versions of the files that they continue to use.</span></span>
* <span data-ttu-id="64196-120">PWA 的*service-worker*和*service-worker-assets*檔案必須在編譯時重建，在使用者下一次線上流覽應用程式時，該應用程式會發出通知。</span><span class="sxs-lookup"><span data-stu-id="64196-120">The PWA's *service-worker.js* and *service-worker-assets.js* files must be rebuilt on compilation, which signal to the app on the user's next online visit that the app has been redeployed.</span></span>

<span data-ttu-id="64196-121">如需 Pwa 如何處理背景更新的詳細資訊，請參閱 <xref:blazor/progressive-web-app#background-updates>。</span><span class="sxs-lookup"><span data-stu-id="64196-121">For more information on how background updates are handled by PWAs, see <xref:blazor/progressive-web-app#background-updates>.</span></span>

## <a name="blazor-server"></a><span data-ttu-id="64196-122">Blazor 伺服器</span><span class="sxs-lookup"><span data-stu-id="64196-122">Blazor Server</span></span>

### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="64196-123">反映 UI 中的連接狀態</span><span class="sxs-lookup"><span data-stu-id="64196-123">Reflect the connection state in the UI</span></span>

<span data-ttu-id="64196-124">當用戶端偵測到連線已遺失時，會在用戶端嘗試重新連線時，向使用者顯示預設的 UI。</span><span class="sxs-lookup"><span data-stu-id="64196-124">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="64196-125">如果重新連線失敗，則會提供使用者重試的選項。</span><span class="sxs-lookup"><span data-stu-id="64196-125">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="64196-126">若要自訂 UI，請在 *_Host*的 [Razor 頁面] `<body>` 中，定義具有 `components-reconnect-modal` `id` 的元素：</span><span class="sxs-lookup"><span data-stu-id="64196-126">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="64196-127">下表描述套用至 `components-reconnect-modal` 元素的 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="64196-127">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="64196-128">CSS 類別</span><span class="sxs-lookup"><span data-stu-id="64196-128">CSS class</span></span>                       | <span data-ttu-id="64196-129">表示&hellip;</span><span class="sxs-lookup"><span data-stu-id="64196-129">Indicates&hellip;</span></span> |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | <span data-ttu-id="64196-130">遺失的連接。</span><span class="sxs-lookup"><span data-stu-id="64196-130">A lost connection.</span></span> <span data-ttu-id="64196-131">用戶端正在嘗試重新連線。</span><span class="sxs-lookup"><span data-stu-id="64196-131">The client is attempting to reconnect.</span></span> <span data-ttu-id="64196-132">顯示強制回應。</span><span class="sxs-lookup"><span data-stu-id="64196-132">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="64196-133">系統會將使用中的連接重新建立至伺服器。</span><span class="sxs-lookup"><span data-stu-id="64196-133">An active connection is re-established to the server.</span></span> <span data-ttu-id="64196-134">隱藏強制回應。</span><span class="sxs-lookup"><span data-stu-id="64196-134">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="64196-135">重新連接失敗，可能是因為網路失敗。</span><span class="sxs-lookup"><span data-stu-id="64196-135">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="64196-136">若要嘗試重新連接，請呼叫 `window.Blazor.reconnect()`。</span><span class="sxs-lookup"><span data-stu-id="64196-136">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="64196-137">已拒絕重新連接。</span><span class="sxs-lookup"><span data-stu-id="64196-137">Reconnection rejected.</span></span> <span data-ttu-id="64196-138">已達到伺服器但拒絕連線，而且伺服器上的使用者狀態遺失。</span><span class="sxs-lookup"><span data-stu-id="64196-138">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="64196-139">若要重載應用程式，請呼叫 `location.reload()`。</span><span class="sxs-lookup"><span data-stu-id="64196-139">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="64196-140">當下列情況發生時，可能會產生此連接狀態：</span><span class="sxs-lookup"><span data-stu-id="64196-140">This connection state may result when:</span></span><ul><li><span data-ttu-id="64196-141">伺服器端線路發生損毀。</span><span class="sxs-lookup"><span data-stu-id="64196-141">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="64196-142">用戶端中斷連線的時間夠長，伺服器才會捨棄使用者的狀態。</span><span class="sxs-lookup"><span data-stu-id="64196-142">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="64196-143">系統會處置與使用者互動之元件的實例。</span><span class="sxs-lookup"><span data-stu-id="64196-143">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="64196-144">伺服器會重新開機，或回收應用程式的背景工作進程。</span><span class="sxs-lookup"><span data-stu-id="64196-144">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="render-mode"></a><span data-ttu-id="64196-145">轉譯模式</span><span class="sxs-lookup"><span data-stu-id="64196-145">Render mode</span></span>

<span data-ttu-id="64196-146">在建立伺服器的用戶端連接之前，預設會設定 Blazor 伺服器應用程式，以預先呈現伺服器上的 UI。</span><span class="sxs-lookup"><span data-stu-id="64196-146">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="64196-147">這會在 *_Host. cshtml* Razor 頁面中設定：</span><span class="sxs-lookup"><span data-stu-id="64196-147">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="64196-148">`RenderMode` 設定元件是否：</span><span class="sxs-lookup"><span data-stu-id="64196-148">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="64196-149">會資源清單到頁面中。</span><span class="sxs-lookup"><span data-stu-id="64196-149">Is prerendered into the page.</span></span>
* <span data-ttu-id="64196-150">會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動 Blazor 應用程式所需的資訊。</span><span class="sxs-lookup"><span data-stu-id="64196-150">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="64196-151">描述</span><span class="sxs-lookup"><span data-stu-id="64196-151">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="64196-152">將元件轉譯為靜態 HTML，並包含 Blazor 伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="64196-152">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="64196-153">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="64196-153">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="64196-154">呈現 Blazor 伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="64196-154">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="64196-155">不包含來自元件的輸出。</span><span class="sxs-lookup"><span data-stu-id="64196-155">Output from the component isn't included.</span></span> <span data-ttu-id="64196-156">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="64196-156">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="64196-157">將元件轉譯為靜態 HTML。</span><span class="sxs-lookup"><span data-stu-id="64196-157">Renders the component into static HTML.</span></span> |

<span data-ttu-id="64196-158">不支援從靜態 HTML 網頁轉譯伺服器元件。</span><span class="sxs-lookup"><span data-stu-id="64196-158">Rendering server components from a static HTML page isn't supported.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="64196-159">從 Razor 頁面和 views 轉譯具狀態的互動式元件</span><span class="sxs-lookup"><span data-stu-id="64196-159">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="64196-160">可設定狀態的互動式元件可以新增至 Razor 頁面或視圖。</span><span class="sxs-lookup"><span data-stu-id="64196-160">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="64196-161">當頁面或視圖呈現時：</span><span class="sxs-lookup"><span data-stu-id="64196-161">When the page or view renders:</span></span>

* <span data-ttu-id="64196-162">此元件是使用頁面或視圖所資源清單。</span><span class="sxs-lookup"><span data-stu-id="64196-162">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="64196-163">用來進行預呈現的初始元件狀態會遺失。</span><span class="sxs-lookup"><span data-stu-id="64196-163">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="64196-164">建立 SignalR 連接時，會建立新的元件狀態。</span><span class="sxs-lookup"><span data-stu-id="64196-164">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="64196-165">下列 Razor 頁面會呈現 `Counter` 元件：</span><span class="sxs-lookup"><span data-stu-id="64196-165">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="64196-166">從 Razor 頁面和 views 轉譯非互動式元件</span><span class="sxs-lookup"><span data-stu-id="64196-166">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="64196-167">在下列 Razor 頁面中，`Counter` 元件會以靜態方式轉譯，並使用以表單指定的初始值：</span><span class="sxs-lookup"><span data-stu-id="64196-167">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

<component type="typeof(Counter)" render-mode="Static" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

<span data-ttu-id="64196-168">由於 `MyComponent` 是以靜態方式呈現，因此元件不能是互動式的。</span><span class="sxs-lookup"><span data-stu-id="64196-168">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="64196-169">設定 Blazor 伺服器應用程式的 SignalR 用戶端</span><span class="sxs-lookup"><span data-stu-id="64196-169">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="64196-170">有時候，您需要設定 Blazor 伺服器應用程式所使用的 SignalR 用戶端。</span><span class="sxs-lookup"><span data-stu-id="64196-170">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="64196-171">例如，您可能會想要在 SignalR 用戶端上設定記錄，以診斷連接問題。</span><span class="sxs-lookup"><span data-stu-id="64196-171">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="64196-172">若要設定*Pages/_Host. cshtml*檔案中的 SignalR 用戶端：</span><span class="sxs-lookup"><span data-stu-id="64196-172">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="64196-173">將 `autostart="false"` 屬性加入至 `blazor.server.js` 腳本的 `<script>` 標記。</span><span class="sxs-lookup"><span data-stu-id="64196-173">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="64196-174">呼叫 `Blazor.start` 並傳入指定 SignalR 產生器的設定物件。</span><span class="sxs-lookup"><span data-stu-id="64196-174">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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
