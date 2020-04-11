---
title: ASP.NET核心Blazor託管模型設定
author: guardrex
description: 瞭解Blazor託管模型配置,包括如何將 Razor 元件整合到 Razor 頁面和 MVC 應用中。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: ca1b3ea9092640ca561b3fbe02ddce6f974c525e
ms.sourcegitcommit: e8dc30453af8bbefcb61857987090d79230a461d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123378"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="b6109-103">ASP.NET核心布拉佐託管模型配置</span><span class="sxs-lookup"><span data-stu-id="b6109-103">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="b6109-104">由[丹尼爾·羅斯](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="b6109-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="b6109-105">本文介紹託管模型配置。</span><span class="sxs-lookup"><span data-stu-id="b6109-105">This article covers hosting model configuration.</span></span>

## <a name="blazor-webassembly"></a><span data-ttu-id="b6109-106">Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="b6109-106">Blazor WebAssembly</span></span>

### <a name="environment"></a><span data-ttu-id="b6109-107">環境</span><span class="sxs-lookup"><span data-stu-id="b6109-107">Environment</span></span>

<span data-ttu-id="b6109-108">在本地運行應用時,環境預設為"開發"。</span><span class="sxs-lookup"><span data-stu-id="b6109-108">When running an app locally, the environment defaults to Development.</span></span> <span data-ttu-id="b6109-109">發佈應用時,環境預設為"生產"。</span><span class="sxs-lookup"><span data-stu-id="b6109-109">When the app is published, the environment defaults to Production.</span></span>

<span data-ttu-id="b6109-110">託管的 Blazor WebAssembly 應用透過中間件從伺服器拾取環境,該中間件`blazor-environment`透過添加標頭將環境傳達給瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="b6109-110">A hosted Blazor WebAssembly app picks up the environment from the server via a middleware that communicates the environment to the browser by adding the `blazor-environment` header.</span></span> <span data-ttu-id="b6109-111">標頭的值是環境。</span><span class="sxs-lookup"><span data-stu-id="b6109-111">The value of the header is the environment.</span></span> <span data-ttu-id="b6109-112">託管的 Blazor 應用和伺服器應用共用相同的環境。</span><span class="sxs-lookup"><span data-stu-id="b6109-112">The hosted Blazor app and the server app share the same environment.</span></span> <span data-ttu-id="b6109-113">有關詳細資訊(包括如何設定環境),請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="b6109-113">For more information, including how to configure the environment, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="b6109-114">對於在本地運行的獨立應用,開發伺服器將添加`blazor-environment`標頭以指定開發環境。</span><span class="sxs-lookup"><span data-stu-id="b6109-114">For a standalone app running locally, the development server adds the `blazor-environment` header to specify the Development environment.</span></span> <span data-ttu-id="b6109-115">要指定其他託管環境的環境,請`blazor-environment`添加標頭。</span><span class="sxs-lookup"><span data-stu-id="b6109-115">To specify the environment for other hosting environments, add the `blazor-environment` header.</span></span>

<span data-ttu-id="b6109-116">在下面的 IIS 範例中,將自訂標頭添加到已發布*的 Web.config*檔中。</span><span class="sxs-lookup"><span data-stu-id="b6109-116">In the following example for IIS, add the custom header to the published *web.config* file.</span></span> <span data-ttu-id="b6109-117">*Web.config*檔案位於*bin/發行/[目標框架]/發佈*資料夾中:</span><span class="sxs-lookup"><span data-stu-id="b6109-117">The *web.config* file is located in the *bin/Release/{TARGET FRAMEWORK}/publish* folder:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>

    ...

    <httpProtocol>
      <customHeaders>
        <add name="blazor-environment" value="Staging" />
      </customHeaders>
    </httpProtocol>
  </system.webServer>
</configuration>
```

> [!NOTE]
> <span data-ttu-id="b6109-118">要對 IIS 使用自訂*Web.config*檔案,該檔在將套用到*的*資料夾時不會覆<xref:host-and-deploy/blazor/webassembly#use-a-custom-webconfig>寫,請參考 。</span><span class="sxs-lookup"><span data-stu-id="b6109-118">To use a custom *web.config* file for IIS that isn't overwritten when the app is published to the *publish* folder, see <xref:host-and-deploy/blazor/webassembly#use-a-custom-webconfig>.</span></span>

<span data-ttu-id="b6109-119">通過注入`IWebAssemblyHostEnvironment`與讀`Environment`取 屬性,取得元件中的應用環境:</span><span class="sxs-lookup"><span data-stu-id="b6109-119">Obtain the app's environment in a component by injecting `IWebAssemblyHostEnvironment` and reading the `Environment` property:</span></span>

```razor
@page "/"
@using Microsoft.AspNetCore.Components.WebAssembly.Hosting
@inject IWebAssemblyHostEnvironment HostEnvironment

<h1>Environment example</h1>

<p>Environment: @HostEnvironment.Environment</p>
```

### <a name="configuration"></a><span data-ttu-id="b6109-120">組態</span><span class="sxs-lookup"><span data-stu-id="b6109-120">Configuration</span></span>

<span data-ttu-id="b6109-121">自 ASP.NET 酷睿 3.2 預覽 3 版中,Blazor WebAssembly 支援以下配置:</span><span class="sxs-lookup"><span data-stu-id="b6109-121">As of the ASP.NET Core 3.2 Preview 3 release, Blazor WebAssembly supports configuration from:</span></span>

* <span data-ttu-id="b6109-122">*wwwroot/appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="b6109-122">*wwwroot/appsettings.json*</span></span>
* <span data-ttu-id="b6109-123">*wwwroot/應用設置。[環境].json*</span><span class="sxs-lookup"><span data-stu-id="b6109-123">*wwwroot/appsettings.{ENVIRONMENT}.json*</span></span>

<span data-ttu-id="b6109-124">在*wwwroot*資料夾中新增*應用設定.json*檔:</span><span class="sxs-lookup"><span data-stu-id="b6109-124">Add an *appsettings.json* file in the *wwwroot* folder:</span></span>

```json
{
  "message": "Hello from config!"
}
```

<span data-ttu-id="b6109-125">將<xref:Microsoft.Extensions.Configuration.IConfiguration>實體編寫入元件以存取設定資料:</span><span class="sxs-lookup"><span data-stu-id="b6109-125">Inject an <xref:Microsoft.Extensions.Configuration.IConfiguration> instance into a component to access the configuration data:</span></span>

```razor
@page "/"
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<h1>Configuration example</h1>

<p>Message: @Configuration["message"]</p>
```

> [!WARNING]
> <span data-ttu-id="b6109-126">Blazor WebAssembly 應用中的配置對用戶可見。</span><span class="sxs-lookup"><span data-stu-id="b6109-126">Configuration in a Blazor WebAssembly app is visible to users.</span></span> <span data-ttu-id="b6109-127">**不要在配置中存儲應用機密或憑據。**</span><span class="sxs-lookup"><span data-stu-id="b6109-127">**Don't store app secrets or credentials in configuration.**</span></span>

<span data-ttu-id="b6109-128">配置檔將緩存供離線使用。</span><span class="sxs-lookup"><span data-stu-id="b6109-128">Configuration files are cached for offline use.</span></span> <span data-ttu-id="b6109-129">使用[漸進式 Web 應用程式 (PWA),](xref:blazor/progressive-web-app)只能在建立新部署時更新配置檔。</span><span class="sxs-lookup"><span data-stu-id="b6109-129">With [Progressive Web Applications (PWAs)](xref:blazor/progressive-web-app), you can only update configuration files when creating a new deployment.</span></span> <span data-ttu-id="b6109-130">在部署之間編輯設定檔不起作用,因為:</span><span class="sxs-lookup"><span data-stu-id="b6109-130">Editing configuration files between deployments has no effect because:</span></span>

* <span data-ttu-id="b6109-131">使用者已緩存了他們繼續使用的檔的版本。</span><span class="sxs-lookup"><span data-stu-id="b6109-131">Users have cached versions of the files that they continue to use.</span></span>
* <span data-ttu-id="b6109-132">PWA 的服務*工作者.js* *和服務工作者資產.js*檔必須在編譯時重新生成,這向使用者下一次連線訪問時的應用發出信號,表明應用程式已重新部署。</span><span class="sxs-lookup"><span data-stu-id="b6109-132">The PWA's *service-worker.js* and *service-worker-assets.js* files must be rebuilt on compilation, which signal to the app on the user's next online visit that the app has been redeployed.</span></span>

<span data-ttu-id="b6109-133">有關 PWA 如何處理後臺更新的詳細資訊,請<xref:blazor/progressive-web-app#background-updates>參閱 。</span><span class="sxs-lookup"><span data-stu-id="b6109-133">For more information on how background updates are handled by PWAs, see <xref:blazor/progressive-web-app#background-updates>.</span></span>

## <a name="blazor-server"></a><span data-ttu-id="b6109-134">Blazor 伺服器</span><span class="sxs-lookup"><span data-stu-id="b6109-134">Blazor Server</span></span>

### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="b6109-135">反映 UI 的連線狀態</span><span class="sxs-lookup"><span data-stu-id="b6109-135">Reflect the connection state in the UI</span></span>

<span data-ttu-id="b6109-136">當用戶端檢測到連接已丟失時,在用戶端嘗試重新連接時向使用者顯示預設 UI。</span><span class="sxs-lookup"><span data-stu-id="b6109-136">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="b6109-137">如果重新連接失敗,則為使用者提供重試的選項。</span><span class="sxs-lookup"><span data-stu-id="b6109-137">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="b6109-138">要自訂 UI,請在 *_Host.cshtml* `<body>`Razor`id`頁`components-reconnect-modal`中定義具有 的元素:</span><span class="sxs-lookup"><span data-stu-id="b6109-138">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="b6109-139">下表描述了應用於元素的`components-reconnect-modal`CSS 類。</span><span class="sxs-lookup"><span data-stu-id="b6109-139">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="b6109-140">CSS 類別</span><span class="sxs-lookup"><span data-stu-id="b6109-140">CSS class</span></span>                       | <span data-ttu-id="b6109-141">表示&hellip;</span><span class="sxs-lookup"><span data-stu-id="b6109-141">Indicates&hellip;</span></span> |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | <span data-ttu-id="b6109-142">失去連接。</span><span class="sxs-lookup"><span data-stu-id="b6109-142">A lost connection.</span></span> <span data-ttu-id="b6109-143">用戶端正在嘗試重新連接。</span><span class="sxs-lookup"><span data-stu-id="b6109-143">The client is attempting to reconnect.</span></span> <span data-ttu-id="b6109-144">顯示模式。</span><span class="sxs-lookup"><span data-stu-id="b6109-144">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="b6109-145">活動連接將重新建立到伺服器。</span><span class="sxs-lookup"><span data-stu-id="b6109-145">An active connection is re-established to the server.</span></span> <span data-ttu-id="b6109-146">隱藏模式。</span><span class="sxs-lookup"><span data-stu-id="b6109-146">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="b6109-147">重新連接失敗,可能是由於網路故障。</span><span class="sxs-lookup"><span data-stu-id="b6109-147">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="b6109-148">要嘗試重新連線,請`window.Blazor.reconnect()`呼叫 。</span><span class="sxs-lookup"><span data-stu-id="b6109-148">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="b6109-149">重新連接被拒絕。</span><span class="sxs-lookup"><span data-stu-id="b6109-149">Reconnection rejected.</span></span> <span data-ttu-id="b6109-150">已到達伺服器,但拒絕連接,並且使用者在伺服器上的狀態將丟失。</span><span class="sxs-lookup"><span data-stu-id="b6109-150">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="b6109-151">要重新載入套用,請呼`location.reload()`叫 。</span><span class="sxs-lookup"><span data-stu-id="b6109-151">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="b6109-152">在:</span><span class="sxs-lookup"><span data-stu-id="b6109-152">This connection state may result when:</span></span><ul><li><span data-ttu-id="b6109-153">伺服器端電路中發生崩潰。</span><span class="sxs-lookup"><span data-stu-id="b6109-153">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="b6109-154">用戶端斷開連接的時間足夠長,以便伺服器刪除使用者的狀態。</span><span class="sxs-lookup"><span data-stu-id="b6109-154">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="b6109-155">將釋放使用者與之交互的元件的實例。</span><span class="sxs-lookup"><span data-stu-id="b6109-155">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="b6109-156">重新啟動伺服器,或者回收應用的工作進程。</span><span class="sxs-lookup"><span data-stu-id="b6109-156">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="render-mode"></a><span data-ttu-id="b6109-157">轉譯模式</span><span class="sxs-lookup"><span data-stu-id="b6109-157">Render mode</span></span>

<span data-ttu-id="b6109-158">默認情況下,布拉佐伺服器應用設置為在建立與伺服器的用戶端連接之前在伺服器上預呈現 UI。</span><span class="sxs-lookup"><span data-stu-id="b6109-158">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="b6109-159">這在 *_Host.cshtml* Razor 頁面中設定:</span><span class="sxs-lookup"><span data-stu-id="b6109-159">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="b6109-160">`RenderMode`設定元件是否:</span><span class="sxs-lookup"><span data-stu-id="b6109-160">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="b6109-161">預置到頁面中。</span><span class="sxs-lookup"><span data-stu-id="b6109-161">Is prerendered into the page.</span></span>
* <span data-ttu-id="b6109-162">在頁面上呈現為靜態 HTML,或者如果它包含從使用者代理引導 Blazor 應用的必要資訊。</span><span class="sxs-lookup"><span data-stu-id="b6109-162">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="b6109-163">描述</span><span class="sxs-lookup"><span data-stu-id="b6109-163">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="b6109-164">將元件呈現為靜態 HTML,並包含伺服器應用的Blazor標記。</span><span class="sxs-lookup"><span data-stu-id="b6109-164">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="b6109-165">當使用者代理啟動時,此標記用於引導Blazor應用。</span><span class="sxs-lookup"><span data-stu-id="b6109-165">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="b6109-166">渲染伺服器應用的Blazor標記。</span><span class="sxs-lookup"><span data-stu-id="b6109-166">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="b6109-167">不包括元件的輸出。</span><span class="sxs-lookup"><span data-stu-id="b6109-167">Output from the component isn't included.</span></span> <span data-ttu-id="b6109-168">當使用者代理啟動時,此標記用於引導Blazor應用。</span><span class="sxs-lookup"><span data-stu-id="b6109-168">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="b6109-169">將元件呈現為靜態 HTML。</span><span class="sxs-lookup"><span data-stu-id="b6109-169">Renders the component into static HTML.</span></span> |

<span data-ttu-id="b6109-170">不支援從靜態 HTML 頁呈現伺服器元件。</span><span class="sxs-lookup"><span data-stu-id="b6109-170">Rendering server components from a static HTML page isn't supported.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="b6109-171">從 Razor 頁面與檢視成有狀態的互動式元件</span><span class="sxs-lookup"><span data-stu-id="b6109-171">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="b6109-172">可有狀態的互動式元件可以添加到 Razor 頁面或檢視中。</span><span class="sxs-lookup"><span data-stu-id="b6109-172">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="b6109-173">當頁面或檢視呈現時:</span><span class="sxs-lookup"><span data-stu-id="b6109-173">When the page or view renders:</span></span>

* <span data-ttu-id="b6109-174">元件預呈現為頁面或視圖。</span><span class="sxs-lookup"><span data-stu-id="b6109-174">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="b6109-175">用於預渲染的初始元件狀態將丟失。</span><span class="sxs-lookup"><span data-stu-id="b6109-175">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="b6109-176">建立連接時,將創建新的SignalR元件狀態。</span><span class="sxs-lookup"><span data-stu-id="b6109-176">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="b6109-177">以下 Razor 頁面`Counter`呈現元件:</span><span class="sxs-lookup"><span data-stu-id="b6109-177">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="b6109-178">從 Razor 頁面與檢視呈現非互動式元件</span><span class="sxs-lookup"><span data-stu-id="b6109-178">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="b6109-179">在以下 Razor`Counter`頁中 ,元件使用使用表單指定的初始值靜態呈現:</span><span class="sxs-lookup"><span data-stu-id="b6109-179">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

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

<span data-ttu-id="b6109-180">由於`MyComponent`是靜態呈現的,因此元件不能是互動式的。</span><span class="sxs-lookup"><span data-stu-id="b6109-180">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="b6109-181">為SignalRBlazor伺服器應用設定客戶端</span><span class="sxs-lookup"><span data-stu-id="b6109-181">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="b6109-182">有時,您需要配置SignalRBlazor伺服器應用使用的用戶端。</span><span class="sxs-lookup"><span data-stu-id="b6109-182">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="b6109-183">例如,您可能希望在用戶端上SignalR配置日誌記錄以診斷連接問題。</span><span class="sxs-lookup"><span data-stu-id="b6109-183">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="b6109-184">要在SignalR*頁面/_Host.cshtml*檔中設定客戶端,請進行以下分析:</span><span class="sxs-lookup"><span data-stu-id="b6109-184">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="b6109-185">向`autostart="false"``blazor.server.js`文稿的`<script>`標記添加屬性。</span><span class="sxs-lookup"><span data-stu-id="b6109-185">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="b6109-186">呼叫`Blazor.start`並傳遞指定生成器的SignalR配置物件。</span><span class="sxs-lookup"><span data-stu-id="b6109-186">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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
