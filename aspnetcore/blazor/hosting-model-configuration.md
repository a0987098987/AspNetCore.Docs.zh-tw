---
title: ASP.NET核心Blazor託管模型設定
author: guardrex
description: 瞭解Blazor託管模型配置,包括如何將 Razor 元件整合到 Razor 頁面和 MVC 應用中。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/24/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: 1f71ac63bbe9dc9d56cfca2ded19a5b863be828f
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80306426"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="24ba8-103">ASP.NET核心布拉佐託管模型配置</span><span class="sxs-lookup"><span data-stu-id="24ba8-103">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="24ba8-104">由[丹尼爾·羅斯](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="24ba8-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="24ba8-105">本文介紹託管模型配置。</span><span class="sxs-lookup"><span data-stu-id="24ba8-105">This article covers hosting model configuration.</span></span>

## <a name="blazor-webassembly"></a><span data-ttu-id="24ba8-106">Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="24ba8-106">Blazor WebAssembly</span></span>

<span data-ttu-id="24ba8-107">自 ASP.NET 酷睿 3.2 預覽 3 版中,Blazor WebAssembly 支援以下配置:</span><span class="sxs-lookup"><span data-stu-id="24ba8-107">As of the ASP.NET Core 3.2 Preview 3 release, Blazor WebAssembly supports configuration from:</span></span>

* <span data-ttu-id="24ba8-108">*wwwroot/appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="24ba8-108">*wwwroot/appsettings.json*</span></span>
* <span data-ttu-id="24ba8-109">*wwwroot/應用設置。[環境].json*</span><span class="sxs-lookup"><span data-stu-id="24ba8-109">*wwwroot/appsettings.{ENVIRONMENT}.json*</span></span>

<span data-ttu-id="24ba8-110">在 Blazor 託管應用中,[執行時環境](xref:fundamentals/environments)與伺服器應用的值相同。</span><span class="sxs-lookup"><span data-stu-id="24ba8-110">In a Blazor Hosted app, the [runtime environment](xref:fundamentals/environments) is the same as the server app's value.</span></span>

<span data-ttu-id="24ba8-111">在本地運行應用時,環境預設為"開發"。</span><span class="sxs-lookup"><span data-stu-id="24ba8-111">When running the app locally, the environment defaults to Development.</span></span> <span data-ttu-id="24ba8-112">發佈應用時,環境預設為"生產"。</span><span class="sxs-lookup"><span data-stu-id="24ba8-112">When the app is published, the environment defaults to Production.</span></span> <span data-ttu-id="24ba8-113">有關詳細資訊(包括如何設定環境),請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="24ba8-113">For more information, including how to configure the environment, see <xref:fundamentals/environments>.</span></span>

> [!WARNING]
> <span data-ttu-id="24ba8-114">Blazor WebAssembly 應用中的配置對用戶可見。</span><span class="sxs-lookup"><span data-stu-id="24ba8-114">Configuration in a Blazor WebAssembly app is visible to users.</span></span> <span data-ttu-id="24ba8-115">**不要在配置中存儲應用機密或憑據。**</span><span class="sxs-lookup"><span data-stu-id="24ba8-115">**Don't store app secrets or credentials in configuration.**</span></span>

<span data-ttu-id="24ba8-116">配置檔將緩存供離線使用。</span><span class="sxs-lookup"><span data-stu-id="24ba8-116">Configuration files are cached for offline use.</span></span> <span data-ttu-id="24ba8-117">使用[漸進式 Web 應用程式 (PWA),](xref:blazor/progressive-web-app)只能在建立新部署時更新配置檔。</span><span class="sxs-lookup"><span data-stu-id="24ba8-117">With [Progressive Web Applications (PWAs)](xref:blazor/progressive-web-app), you can only update configuration files when creating a new deployment.</span></span> <span data-ttu-id="24ba8-118">在部署之間編輯設定檔不起作用,因為:</span><span class="sxs-lookup"><span data-stu-id="24ba8-118">Editing configuration files between deployments has no effect because:</span></span>

* <span data-ttu-id="24ba8-119">使用者已緩存了他們繼續使用的檔的版本。</span><span class="sxs-lookup"><span data-stu-id="24ba8-119">Users have cached versions of the files that they continue to use.</span></span>
* <span data-ttu-id="24ba8-120">PWA 的服務*工作者.js* *和服務工作者資產.js*檔必須在編譯時重新生成,這向使用者下一次連線訪問時的應用發出信號,表明應用程式已重新部署。</span><span class="sxs-lookup"><span data-stu-id="24ba8-120">The PWA's *service-worker.js* and *service-worker-assets.js* files must be rebuilt on compilation, which signal to the app on the user's next online visit that the app has been redeployed.</span></span>

<span data-ttu-id="24ba8-121">有關 PWA 如何處理後臺更新的詳細資訊,請<xref:blazor/progressive-web-app#background-updates>參閱 。</span><span class="sxs-lookup"><span data-stu-id="24ba8-121">For more information on how background updates are handled by PWAs, see <xref:blazor/progressive-web-app#background-updates>.</span></span>

## <a name="blazor-server"></a><span data-ttu-id="24ba8-122">Blazor 伺服器</span><span class="sxs-lookup"><span data-stu-id="24ba8-122">Blazor Server</span></span>

### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="24ba8-123">反映 UI 的連線狀態</span><span class="sxs-lookup"><span data-stu-id="24ba8-123">Reflect the connection state in the UI</span></span>

<span data-ttu-id="24ba8-124">當用戶端檢測到連接已丟失時,在用戶端嘗試重新連接時向使用者顯示預設 UI。</span><span class="sxs-lookup"><span data-stu-id="24ba8-124">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="24ba8-125">如果重新連接失敗,則為使用者提供重試的選項。</span><span class="sxs-lookup"><span data-stu-id="24ba8-125">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="24ba8-126">要自訂 UI,請在 *_Host.cshtml* `<body>`Razor`id`頁`components-reconnect-modal`中定義具有 的元素:</span><span class="sxs-lookup"><span data-stu-id="24ba8-126">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="24ba8-127">下表描述了應用於元素的`components-reconnect-modal`CSS 類。</span><span class="sxs-lookup"><span data-stu-id="24ba8-127">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="24ba8-128">CSS 類別</span><span class="sxs-lookup"><span data-stu-id="24ba8-128">CSS class</span></span>                       | <span data-ttu-id="24ba8-129">表示&hellip;</span><span class="sxs-lookup"><span data-stu-id="24ba8-129">Indicates&hellip;</span></span> |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | <span data-ttu-id="24ba8-130">失去連接。</span><span class="sxs-lookup"><span data-stu-id="24ba8-130">A lost connection.</span></span> <span data-ttu-id="24ba8-131">用戶端正在嘗試重新連接。</span><span class="sxs-lookup"><span data-stu-id="24ba8-131">The client is attempting to reconnect.</span></span> <span data-ttu-id="24ba8-132">顯示模式。</span><span class="sxs-lookup"><span data-stu-id="24ba8-132">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="24ba8-133">活動連接將重新建立到伺服器。</span><span class="sxs-lookup"><span data-stu-id="24ba8-133">An active connection is re-established to the server.</span></span> <span data-ttu-id="24ba8-134">隱藏模式。</span><span class="sxs-lookup"><span data-stu-id="24ba8-134">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="24ba8-135">重新連接失敗,可能是由於網路故障。</span><span class="sxs-lookup"><span data-stu-id="24ba8-135">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="24ba8-136">要嘗試重新連線,請`window.Blazor.reconnect()`呼叫 。</span><span class="sxs-lookup"><span data-stu-id="24ba8-136">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="24ba8-137">重新連接被拒絕。</span><span class="sxs-lookup"><span data-stu-id="24ba8-137">Reconnection rejected.</span></span> <span data-ttu-id="24ba8-138">已到達伺服器,但拒絕連接,並且使用者在伺服器上的狀態將丟失。</span><span class="sxs-lookup"><span data-stu-id="24ba8-138">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="24ba8-139">要重新載入套用,請呼`location.reload()`叫 。</span><span class="sxs-lookup"><span data-stu-id="24ba8-139">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="24ba8-140">在:</span><span class="sxs-lookup"><span data-stu-id="24ba8-140">This connection state may result when:</span></span><ul><li><span data-ttu-id="24ba8-141">伺服器端電路中發生崩潰。</span><span class="sxs-lookup"><span data-stu-id="24ba8-141">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="24ba8-142">用戶端斷開連接的時間足夠長,以便伺服器刪除使用者的狀態。</span><span class="sxs-lookup"><span data-stu-id="24ba8-142">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="24ba8-143">將釋放使用者與之交互的元件的實例。</span><span class="sxs-lookup"><span data-stu-id="24ba8-143">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="24ba8-144">重新啟動伺服器,或者回收應用的工作進程。</span><span class="sxs-lookup"><span data-stu-id="24ba8-144">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="render-mode"></a><span data-ttu-id="24ba8-145">轉譯模式</span><span class="sxs-lookup"><span data-stu-id="24ba8-145">Render mode</span></span>

<span data-ttu-id="24ba8-146">默認情況下,布拉佐伺服器應用設置為在建立與伺服器的用戶端連接之前在伺服器上預呈現 UI。</span><span class="sxs-lookup"><span data-stu-id="24ba8-146">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="24ba8-147">這在 *_Host.cshtml* Razor 頁面中設定:</span><span class="sxs-lookup"><span data-stu-id="24ba8-147">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="24ba8-148">`RenderMode`設定元件是否:</span><span class="sxs-lookup"><span data-stu-id="24ba8-148">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="24ba8-149">預置到頁面中。</span><span class="sxs-lookup"><span data-stu-id="24ba8-149">Is prerendered into the page.</span></span>
* <span data-ttu-id="24ba8-150">在頁面上呈現為靜態 HTML,或者如果它包含從使用者代理引導 Blazor 應用的必要資訊。</span><span class="sxs-lookup"><span data-stu-id="24ba8-150">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="24ba8-151">描述</span><span class="sxs-lookup"><span data-stu-id="24ba8-151">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="24ba8-152">將元件呈現為靜態 HTML,並包含伺服器應用的Blazor標記。</span><span class="sxs-lookup"><span data-stu-id="24ba8-152">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="24ba8-153">當使用者代理啟動時,此標記用於引導Blazor應用。</span><span class="sxs-lookup"><span data-stu-id="24ba8-153">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="24ba8-154">渲染伺服器應用的Blazor標記。</span><span class="sxs-lookup"><span data-stu-id="24ba8-154">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="24ba8-155">不包括元件的輸出。</span><span class="sxs-lookup"><span data-stu-id="24ba8-155">Output from the component isn't included.</span></span> <span data-ttu-id="24ba8-156">當使用者代理啟動時,此標記用於引導Blazor應用。</span><span class="sxs-lookup"><span data-stu-id="24ba8-156">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="24ba8-157">將元件呈現為靜態 HTML。</span><span class="sxs-lookup"><span data-stu-id="24ba8-157">Renders the component into static HTML.</span></span> |

<span data-ttu-id="24ba8-158">不支援從靜態 HTML 頁呈現伺服器元件。</span><span class="sxs-lookup"><span data-stu-id="24ba8-158">Rendering server components from a static HTML page isn't supported.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="24ba8-159">從 Razor 頁面與檢視成有狀態的互動式元件</span><span class="sxs-lookup"><span data-stu-id="24ba8-159">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="24ba8-160">可有狀態的互動式元件可以添加到 Razor 頁面或檢視中。</span><span class="sxs-lookup"><span data-stu-id="24ba8-160">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="24ba8-161">當頁面或檢視呈現時:</span><span class="sxs-lookup"><span data-stu-id="24ba8-161">When the page or view renders:</span></span>

* <span data-ttu-id="24ba8-162">元件預呈現為頁面或視圖。</span><span class="sxs-lookup"><span data-stu-id="24ba8-162">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="24ba8-163">用於預渲染的初始元件狀態將丟失。</span><span class="sxs-lookup"><span data-stu-id="24ba8-163">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="24ba8-164">建立連接時,將創建新的SignalR元件狀態。</span><span class="sxs-lookup"><span data-stu-id="24ba8-164">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="24ba8-165">以下 Razor 頁面`Counter`呈現元件:</span><span class="sxs-lookup"><span data-stu-id="24ba8-165">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="24ba8-166">從 Razor 頁面與檢視呈現非互動式元件</span><span class="sxs-lookup"><span data-stu-id="24ba8-166">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="24ba8-167">在以下 Razor`Counter`頁中 ,元件使用使用表單指定的初始值靜態呈現:</span><span class="sxs-lookup"><span data-stu-id="24ba8-167">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

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

<span data-ttu-id="24ba8-168">由於`MyComponent`是靜態呈現的,因此元件不能是互動式的。</span><span class="sxs-lookup"><span data-stu-id="24ba8-168">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="24ba8-169">為SignalRBlazor伺服器應用設定客戶端</span><span class="sxs-lookup"><span data-stu-id="24ba8-169">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="24ba8-170">有時,您需要配置SignalRBlazor伺服器應用使用的用戶端。</span><span class="sxs-lookup"><span data-stu-id="24ba8-170">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="24ba8-171">例如,您可能希望在用戶端上SignalR配置日誌記錄以診斷連接問題。</span><span class="sxs-lookup"><span data-stu-id="24ba8-171">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="24ba8-172">要在SignalR*頁面/_Host.cshtml*檔中設定客戶端,請進行以下分析:</span><span class="sxs-lookup"><span data-stu-id="24ba8-172">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="24ba8-173">向`autostart="false"``blazor.server.js`文稿的`<script>`標記添加屬性。</span><span class="sxs-lookup"><span data-stu-id="24ba8-173">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="24ba8-174">呼叫`Blazor.start`並傳遞指定生成器的SignalR配置物件。</span><span class="sxs-lookup"><span data-stu-id="24ba8-174">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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
