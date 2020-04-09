---
title: 託管與部署ASP.NET核心Blazor伺服器
author: guardrex
description: 瞭解如何使用 ASP.NET 核心Blazor託管和部署伺服器應用。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/03/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/server
ms.openlocfilehash: 866bb348180c872d8ab20787283cfb7217183a8d
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "79025419"
---
# <a name="host-and-deploy-opno-locblazor-server"></a><span data-ttu-id="f74bd-103">託管與部署Blazor伺服器</span><span class="sxs-lookup"><span data-stu-id="f74bd-103">Host and deploy Blazor Server</span></span>

<span data-ttu-id="f74bd-104">作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="f74bd-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="f74bd-105">主機組態值</span><span class="sxs-lookup"><span data-stu-id="f74bd-105">Host configuration values</span></span>

<span data-ttu-id="f74bd-106">伺服器應用可以接受[通用主機設定值](xref:fundamentals/host/generic-host#host-configuration) [ Blazor ](xref:blazor/hosting-models#blazor-server) 。</span><span class="sxs-lookup"><span data-stu-id="f74bd-106">[Blazor Server apps](xref:blazor/hosting-models#blazor-server) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="f74bd-107">部署</span><span class="sxs-lookup"><span data-stu-id="f74bd-107">Deployment</span></span>

<span data-ttu-id="f74bd-108">使用[Blazor伺服器託管模型](xref:blazor/hosting-models#blazor-server),Blazor在伺服器上執行 ASP.NET 酷應用程式。</span><span class="sxs-lookup"><span data-stu-id="f74bd-108">Using the [Blazor Server hosting model](xref:blazor/hosting-models#blazor-server), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="f74bd-109">UI 更新、事件處理和 JAVAScript[SignalR](xref:signalr/introduction)呼叫透過連接處理。</span><span class="sxs-lookup"><span data-stu-id="f74bd-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="f74bd-110">需要能夠裝載 ASP.NET Core 應用程式的網路伺服器。</span><span class="sxs-lookup"><span data-stu-id="f74bd-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="f74bd-111">Visual Studio**Blazor包括伺服器應用**`blazorserverside`專案範本 (使用[dotnet 新](/dotnet/core/tools/dotnet-new)命令時的範本)。</span><span class="sxs-lookup"><span data-stu-id="f74bd-111">Visual Studio includes the **Blazor Server App** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

## <a name="scalability"></a><span data-ttu-id="f74bd-112">延展性</span><span class="sxs-lookup"><span data-stu-id="f74bd-112">Scalability</span></span>

<span data-ttu-id="f74bd-113">規劃部署以充分利用Blazor伺服器應用的可用基礎結構。</span><span class="sxs-lookup"><span data-stu-id="f74bd-113">Plan a deployment to make the best use of the available infrastructure for a Blazor Server app.</span></span> <span data-ttu-id="f74bd-114">請參考以下資源來解決Blazor伺服器應用可伸縮性:</span><span class="sxs-lookup"><span data-stu-id="f74bd-114">See the following resources to address Blazor Server app scalability:</span></span>

* <span data-ttu-id="f74bd-115">[伺服器應用的Blazor基礎知識](xref:blazor/hosting-models#blazor-server)</span><span class="sxs-lookup"><span data-stu-id="f74bd-115">[Fundamentals of Blazor Server apps](xref:blazor/hosting-models#blazor-server)</span></span>
* <xref:security/blazor/server>

### <a name="deployment-server"></a><span data-ttu-id="f74bd-116">部署伺服器</span><span class="sxs-lookup"><span data-stu-id="f74bd-116">Deployment server</span></span>

<span data-ttu-id="f74bd-117">在考慮單個伺服器的可伸縮性(向上擴展)時,應用可用的記憶體可能是應用隨著使用者需求增加而耗盡的第一個資源。</span><span class="sxs-lookup"><span data-stu-id="f74bd-117">When considering the scalability of a single server (scale up), the memory available to an app is likely the first resource that the app will exhaust as user demands increase.</span></span> <span data-ttu-id="f74bd-118">伺服器上的可用記憶體會影響:</span><span class="sxs-lookup"><span data-stu-id="f74bd-118">The available memory on the server affects the:</span></span>

* <span data-ttu-id="f74bd-119">伺服器可以支援的有源電路數。</span><span class="sxs-lookup"><span data-stu-id="f74bd-119">Number of active circuits that a server can support.</span></span>
* <span data-ttu-id="f74bd-120">用戶端上的 UI 延遲。</span><span class="sxs-lookup"><span data-stu-id="f74bd-120">UI latency on the client.</span></span>

<span data-ttu-id="f74bd-121">有關建構安全和可Blazor擴充的伺服器應用的指導,請<xref:security/blazor/server>參閱 。</span><span class="sxs-lookup"><span data-stu-id="f74bd-121">For guidance on building secure and scalable Blazor server apps, see <xref:security/blazor/server>.</span></span>

<span data-ttu-id="f74bd-122">每個電路使用大約 250 KB 的記憶體,以進行最少的*Hello World*風格的應用。</span><span class="sxs-lookup"><span data-stu-id="f74bd-122">Each circuit uses approximately 250 KB of memory for a minimal *Hello World*-style app.</span></span> <span data-ttu-id="f74bd-123">電路的大小取決於應用的代碼以及與每個元件關聯的狀態維護要求。</span><span class="sxs-lookup"><span data-stu-id="f74bd-123">The size of a circuit depends on the app's code and the state maintenance requirements associated with each component.</span></span> <span data-ttu-id="f74bd-124">我們建議您在開發應用和基礎結構期間測量資源需求,但以下基線可能是規劃部署目標的起點:如果您希望應用支援 5,000 個併發使用者,請考慮為應用至少預算 1.3 GB 的伺服器記憶體(或每個使用者 273 KB)。</span><span class="sxs-lookup"><span data-stu-id="f74bd-124">We recommend that you measure resource demands during development for your app and infrastructure, but the following baseline can be a starting point in planning your deployment target: If you expect your app to support 5,000 concurrent users, consider budgeting at least 1.3 GB of server memory to the app (or ~273 KB per user).</span></span>

### <a name="opno-locsignalr-configuration"></a>SignalR<span data-ttu-id="f74bd-125">設定</span><span class="sxs-lookup"><span data-stu-id="f74bd-125"> configuration</span></span>

Blazor<span data-ttu-id="f74bd-126">伺服器應用使用SignalRASP.NET 核心與瀏覽器通信。</span><span class="sxs-lookup"><span data-stu-id="f74bd-126"> Server apps use ASP.NET Core SignalR to communicate with the browser.</span></span> <span data-ttu-id="f74bd-127">託管和縮放條件適用於[SignalR](xref:signalr/publish-to-azure-web-app)Blazor伺服器應用。</span><span class="sxs-lookup"><span data-stu-id="f74bd-127">[SignalR's hosting and scaling conditions](xref:signalr/publish-to-azure-web-app) apply to Blazor Server apps.</span></span>

Blazor<span data-ttu-id="f74bd-128">由於延遲、可靠性和[安全性](xref:signalr/security)較低,使用SignalRWebSocket 作為傳輸時效果最佳。</span><span class="sxs-lookup"><span data-stu-id="f74bd-128"> works best when using WebSockets as the SignalR transport due to lower latency, reliability, and [security](xref:signalr/security).</span></span> <span data-ttu-id="f74bd-129">當 WebSocketSignalR不可用或應用被顯式配置為使用長輪詢時,使用長輪詢。</span><span class="sxs-lookup"><span data-stu-id="f74bd-129">Long Polling is used by SignalR when WebSockets isn't available or when the app is explicitly configured to use Long Polling.</span></span> <span data-ttu-id="f74bd-130">部署到 Azure 應用服務時,請將應用配置為在服務的 Azure 門戶設置中使用 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="f74bd-130">When deploying to Azure App Service, configure the app to use WebSockets in the Azure portal settings for the service.</span></span> <span data-ttu-id="f74bd-131">有關為 Azure 應用服務設定應用的詳細資訊,[SignalR請參閱發佈指南](xref:signalr/publish-to-azure-web-app)。</span><span class="sxs-lookup"><span data-stu-id="f74bd-131">For details on configuring the app for Azure App Service, see the [SignalR publishing guidelines](xref:signalr/publish-to-azure-web-app).</span></span>

#### <a name="azure-opno-locsignalr-service"></a><span data-ttu-id="f74bd-132">AzureSignalR服務</span><span class="sxs-lookup"><span data-stu-id="f74bd-132">Azure SignalR Service</span></span>

<span data-ttu-id="f74bd-133">我們建議將[AzureSignalR服務](/azure/azure-signalr)用於Blazor伺服器應用。</span><span class="sxs-lookup"><span data-stu-id="f74bd-133">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="f74bd-134">該服務允許將Blazor伺服器應用擴展到大量併SignalR發 連接。</span><span class="sxs-lookup"><span data-stu-id="f74bd-134">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="f74bd-135">此外,SignalR該服務的全球覆蓋範圍和高性能資料中心大大有助於減少因地理位置而導致的延遲。</span><span class="sxs-lookup"><span data-stu-id="f74bd-135">In addition, the SignalR service's global reach and high-performance data centers significantly aid in reducing latency due to geography.</span></span> <span data-ttu-id="f74bd-136">要配置應用(並選擇性預配)AzureSignalR服務:</span><span class="sxs-lookup"><span data-stu-id="f74bd-136">To configure an app (and optionally provision) the Azure SignalR Service:</span></span>

1. <span data-ttu-id="f74bd-137">使服務支援*粘滯會話*,其中用戶端在[預算時重定向回同一伺服器](xref:blazor/hosting-models#connection-to-the-server)。</span><span class="sxs-lookup"><span data-stu-id="f74bd-137">Enable the service to support *sticky sessions*, where clients are [redirected back to the same server when prerendering](xref:blazor/hosting-models#connection-to-the-server).</span></span> <span data-ttu-id="f74bd-138">設定`ServerStickyMode`值或設定值值值`Required`為 。</span><span class="sxs-lookup"><span data-stu-id="f74bd-138">Set the `ServerStickyMode` option or configuration value to `Required`.</span></span> <span data-ttu-id="f74bd-139">通常,應用使用以下方法**之一**創建配置:</span><span class="sxs-lookup"><span data-stu-id="f74bd-139">Typically, an app creates the configuration using **one** of the following approaches:</span></span>

   * <span data-ttu-id="f74bd-140">`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f74bd-140">`Startup.ConfigureServices`:</span></span>
  
     ```csharp
     services.AddSignalR().AddAzureSignalR(options =>
     {
         options.ServerStickyMode = 
             Microsoft.Azure.SignalR.ServerStickyMode.Required;
     });
     ```

   * <span data-ttu-id="f74bd-141">設定(使用以下方法**之一**):</span><span class="sxs-lookup"><span data-stu-id="f74bd-141">Configuration (use **one** of the following approaches):</span></span>
  
     * <span data-ttu-id="f74bd-142">*應用程式設定.json*:</span><span class="sxs-lookup"><span data-stu-id="f74bd-142">*appsettings.json*:</span></span>

       ```json
       "Azure:SignalR:ServerStickyMode": "Required"
       ```

     * <span data-ttu-id="f74bd-143">套用服務在 Azure 門戶中的**設定** > `Azure:SignalR:ServerStickyMode`**應用程式設定**( `Required`**名稱**: 、**數值**): 。</span><span class="sxs-lookup"><span data-stu-id="f74bd-143">The app service's **Configuration** > **Application settings** in the Azure portal (**Name**: `Azure:SignalR:ServerStickyMode`, **Value**: `Required`).</span></span>

1. <span data-ttu-id="f74bd-144">在「Blazor伺服器」應用的可視化工作室中創建 Azure 應用發佈配置檔。</span><span class="sxs-lookup"><span data-stu-id="f74bd-144">Create an Azure Apps publish profile in Visual Studio for the Blazor Server app.</span></span>
1. <span data-ttu-id="f74bd-145">將**AzureSignalR服務**依賴項添加到配置檔。</span><span class="sxs-lookup"><span data-stu-id="f74bd-145">Add the **Azure SignalR Service** dependency to the profile.</span></span> <span data-ttu-id="f74bd-146">如果 Azure 訂閱沒有預先存在的SignalRAzure 服務實例分配給應用,請選擇 **「SignalR創建新 Azure 服務實例**以預配新的服務實例」。</span><span class="sxs-lookup"><span data-stu-id="f74bd-146">If the Azure subscription doesn't have a pre-existing Azure SignalR Service instance to assign to the app, select **Create a new Azure SignalR Service instance** to provision a new service instance.</span></span>
1. <span data-ttu-id="f74bd-147">將應用程式發佈至 Azure。</span><span class="sxs-lookup"><span data-stu-id="f74bd-147">Publish the app to Azure.</span></span>

#### <a name="iis"></a><span data-ttu-id="f74bd-148">IIS</span><span class="sxs-lookup"><span data-stu-id="f74bd-148">IIS</span></span>

<span data-ttu-id="f74bd-149">使用 IIS 時,啟用:</span><span class="sxs-lookup"><span data-stu-id="f74bd-149">When using IIS, enable:</span></span>

* <span data-ttu-id="f74bd-150">[IIS 上的 Web 插座](xref:fundamentals/websockets#enabling-websockets-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="f74bd-150">[WebSockets on IIS](xref:fundamentals/websockets#enabling-websockets-on-iis).</span></span>
* <span data-ttu-id="f74bd-151">[具有應用程式要求路由的粘滯工作階段](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)。</span><span class="sxs-lookup"><span data-stu-id="f74bd-151">[Sticky sessions with Application Request Routing](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span></span>

#### <a name="kubernetes"></a><span data-ttu-id="f74bd-152">Kubernetes</span><span class="sxs-lookup"><span data-stu-id="f74bd-152">Kubernetes</span></span>

<span data-ttu-id="f74bd-153">建立具有以下[Kubernetes 註釋的黏滯工作階段](https://kubernetes.github.io/ingress-nginx/examples/affinity/cookie/)入口定義:</span><span class="sxs-lookup"><span data-stu-id="f74bd-153">Create an ingress definition with the following [Kubernetes annotations for sticky sessions](https://kubernetes.github.io/ingress-nginx/examples/affinity/cookie/):</span></span>

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: <ingress-name>
  annotations:
    nginx.ingress.kubernetes.io/affinity: "cookie"
    nginx.ingress.kubernetes.io/session-cookie-name: "affinity"
    nginx.ingress.kubernetes.io/session-cookie-expires: "14400"
    nginx.ingress.kubernetes.io/session-cookie-max-age: "14400"
```

#### <a name="linux-with-nginx"></a><span data-ttu-id="f74bd-154">使用 Nginx 的 Linux</span><span class="sxs-lookup"><span data-stu-id="f74bd-154">Linux with Nginx</span></span>

<span data-ttu-id="f74bd-155">對於SignalRWebSocket 正常工作,請確認`Upgrade``Connection`代理和標頭設定為以下`$connection_upgrade`值, 並映射到以下任一值:</span><span class="sxs-lookup"><span data-stu-id="f74bd-155">For SignalR WebSockets to function properly, confirm that the proxy's `Upgrade` and `Connection` headers are set to the following values and that `$connection_upgrade` is mapped to either:</span></span>

* <span data-ttu-id="f74bd-156">默認情況下,「升級標頭」值。</span><span class="sxs-lookup"><span data-stu-id="f74bd-156">The Upgrade header value by default.</span></span>
* <span data-ttu-id="f74bd-157">`close`當升級標頭丟失或為空時。</span><span class="sxs-lookup"><span data-stu-id="f74bd-157">`close` when the Upgrade header is missing or empty.</span></span>

```
http {
    map $http_upgrade $connection_upgrade {
        default Upgrade;
        ''      close;
    }

    server {
        listen      80;
        server_name example.com *.example.com
        location / {
            proxy_pass         http://localhost:5000;
            proxy_http_version 1.1;
            proxy_set_header   Upgrade $http_upgrade;
            proxy_set_header   Connection $connection_upgrade;
            proxy_set_header   Host $host;
            proxy_cache_bypass $http_upgrade;
            proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header   X-Forwarded-Proto $scheme;
        }
    }
}
```

<span data-ttu-id="f74bd-158">如需詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="f74bd-158">For more information, see the following articles:</span></span>

* [<span data-ttu-id="f74bd-159">NGINX 為 WebSocket 代理程式</span><span class="sxs-lookup"><span data-stu-id="f74bd-159">NGINX as a WebSocket Proxy</span></span>](https://www.nginx.com/blog/websocket-nginx/)
* [<span data-ttu-id="f74bd-160">WebSocket 代理程式</span><span class="sxs-lookup"><span data-stu-id="f74bd-160">WebSocket proxying</span></span>](http://nginx.org/docs/http/websocket.html)
* <xref:host-and-deploy/linux-nginx>

### <a name="measure-network-latency"></a><span data-ttu-id="f74bd-161">測量網路延遲</span><span class="sxs-lookup"><span data-stu-id="f74bd-161">Measure network latency</span></span>

<span data-ttu-id="f74bd-162">[JS 互通](xref:blazor/call-javascript-from-dotnet)可用於測量網路延遲,如下例所示:</span><span class="sxs-lookup"><span data-stu-id="f74bd-162">[JS interop](xref:blazor/call-javascript-from-dotnet) can be used to measure network latency, as the following example demonstrates:</span></span>

```razor
@inject IJSRuntime JS

@if (latency is null)
{
    <span>Calculating...</span>
}
else
{
    <span>@(latency.Value.TotalMilliseconds)ms</span>
}

@code
{
    private DateTime startTime;
    private TimeSpan? latency;

    protected override async Task OnInitializedAsync()
    {
        startTime = DateTime.UtcNow;
        var _ = await JS.InvokeAsync<string>("toString");
        latency = DateTime.UtcNow - startTime;
    }
}
```

<span data-ttu-id="f74bd-163">為了獲得合理的 UI 體驗,我們建議持續延遲 250ms 或更少。</span><span class="sxs-lookup"><span data-stu-id="f74bd-163">For a reasonable UI experience, we recommend a sustained UI latency of 250ms or less.</span></span>
