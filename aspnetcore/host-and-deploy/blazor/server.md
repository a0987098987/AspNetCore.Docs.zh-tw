---
title: 裝載和部署 ASP.NET Core Blazor伺服器
author: guardrex
description: 瞭解如何使用 ASP.NET Core 裝載和部署Blazor伺服器應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/03/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: host-and-deploy/blazor/server
ms.openlocfilehash: e69b91035c65739dde724330e83793c0b8b5481a
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82775150"
---
# <a name="host-and-deploy-blazor-server"></a><span data-ttu-id="8e2b7-103">裝載和部署Blazor伺服器</span><span class="sxs-lookup"><span data-stu-id="8e2b7-103">Host and deploy Blazor Server</span></span>

<span data-ttu-id="8e2b7-104">作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="8e2b7-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="8e2b7-105">主機組態值</span><span class="sxs-lookup"><span data-stu-id="8e2b7-105">Host configuration values</span></span>

<span data-ttu-id="8e2b7-106">伺服器應用程式可以接受[一般主機設定值](xref:fundamentals/host/generic-host#host-configuration)。 [ Blazor ](xref:blazor/hosting-models#blazor-server)</span><span class="sxs-lookup"><span data-stu-id="8e2b7-106">[Blazor Server apps](xref:blazor/hosting-models#blazor-server) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="8e2b7-107">部署</span><span class="sxs-lookup"><span data-stu-id="8e2b7-107">Deployment</span></span>

<span data-ttu-id="8e2b7-108">使用[ Blazor伺服器裝載模型](xref:blazor/hosting-models#blazor-server)， Blazor會在伺服器上從 ASP.NET Core 應用程式中執行。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-108">Using the [Blazor Server hosting model](xref:blazor/hosting-models#blazor-server), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="8e2b7-109">UI 更新、事件處理及 JavaScript 呼叫會透過[SignalR](xref:signalr/introduction)連接來處理。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="8e2b7-110">需要能夠裝載 ASP.NET Core 應用程式的網路伺服器。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="8e2b7-111">Visual Studio 包括\*\* Blazor伺服器應用程式\*\*專案範本（`blazorserverside`使用[dotnet new](/dotnet/core/tools/dotnet-new)命令時的範本）。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-111">Visual Studio includes the **Blazor Server App** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

## <a name="scalability"></a><span data-ttu-id="8e2b7-112">延展性</span><span class="sxs-lookup"><span data-stu-id="8e2b7-112">Scalability</span></span>

<span data-ttu-id="8e2b7-113">規劃部署，以充分利用Blazor伺服器應用程式可用的基礎結構。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-113">Plan a deployment to make the best use of the available infrastructure for a Blazor Server app.</span></span> <span data-ttu-id="8e2b7-114">請參閱下列資源，以Blazor解決伺服器應用程式的擴充性：</span><span class="sxs-lookup"><span data-stu-id="8e2b7-114">See the following resources to address Blazor Server app scalability:</span></span>

* <span data-ttu-id="8e2b7-115">[伺服器應用Blazor程式的基本概念](xref:blazor/hosting-models#blazor-server)</span><span class="sxs-lookup"><span data-stu-id="8e2b7-115">[Fundamentals of Blazor Server apps](xref:blazor/hosting-models#blazor-server)</span></span>
* <xref:security/blazor/server/threat-mitigation>

### <a name="deployment-server"></a><span data-ttu-id="8e2b7-116">部署伺服器</span><span class="sxs-lookup"><span data-stu-id="8e2b7-116">Deployment server</span></span>

<span data-ttu-id="8e2b7-117">在考慮單一伺服器的擴充性（相應增加）時，應用程式可用的記憶體可能是應用程式在使用者需求增加時將耗盡的第一個資源。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-117">When considering the scalability of a single server (scale up), the memory available to an app is likely the first resource that the app will exhaust as user demands increase.</span></span> <span data-ttu-id="8e2b7-118">伺服器上的可用記憶體會影響：</span><span class="sxs-lookup"><span data-stu-id="8e2b7-118">The available memory on the server affects the:</span></span>

* <span data-ttu-id="8e2b7-119">伺服器可支援的現用線路數目。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-119">Number of active circuits that a server can support.</span></span>
* <span data-ttu-id="8e2b7-120">用戶端上的 UI 延遲。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-120">UI latency on the client.</span></span>

<span data-ttu-id="8e2b7-121">如需建立安全且可擴充Blazor之伺服器應用程式的<xref:security/blazor/server/threat-mitigation>指引，請參閱。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-121">For guidance on building secure and scalable Blazor server apps, see <xref:security/blazor/server/threat-mitigation>.</span></span>

<span data-ttu-id="8e2b7-122">每個線路都使用大約 250 KB 的記憶體來進行最小的*Hello World*樣式應用程式。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-122">Each circuit uses approximately 250 KB of memory for a minimal *Hello World*-style app.</span></span> <span data-ttu-id="8e2b7-123">線路的大小取決於應用程式的程式碼，以及與每個元件相關聯的狀態維護需求。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-123">The size of a circuit depends on the app's code and the state maintenance requirements associated with each component.</span></span> <span data-ttu-id="8e2b7-124">我們建議您在開發應用程式和基礎結構期間測量資源需求，但下列基準可以是規劃部署目標的起點：如果您預期應用程式支援5000並行使用者，請考慮將至少 1.3 GB 的伺服器記憶體預算給應用程式（或每位使用者約 273 KB）。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-124">We recommend that you measure resource demands during development for your app and infrastructure, but the following baseline can be a starting point in planning your deployment target: If you expect your app to support 5,000 concurrent users, consider budgeting at least 1.3 GB of server memory to the app (or ~273 KB per user).</span></span>

### <a name="signalr-configuration"></a>SignalR<span data-ttu-id="8e2b7-125">配置</span><span class="sxs-lookup"><span data-stu-id="8e2b7-125"> configuration</span></span>

Blazor<span data-ttu-id="8e2b7-126">伺服器應用程式會SignalR使用 ASP.NET Core 來與瀏覽器通訊。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-126"> Server apps use ASP.NET Core SignalR to communicate with the browser.</span></span> <span data-ttu-id="8e2b7-127">[的裝載和調整規模條件適用于伺服器應用程式。 SignalR ](xref:signalr/publish-to-azure-web-app) Blazor</span><span class="sxs-lookup"><span data-stu-id="8e2b7-127">[SignalR's hosting and scaling conditions](xref:signalr/publish-to-azure-web-app) apply to Blazor Server apps.</span></span>

Blazor<span data-ttu-id="8e2b7-128">因為延遲、可靠性和[安全性](xref:signalr/security)較SignalR低，所以使用 websocket 做為傳輸時，效果最佳。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-128"> works best when using WebSockets as the SignalR transport due to lower latency, reliability, and [security](xref:signalr/security).</span></span> <span data-ttu-id="8e2b7-129">SignalR當 websocket 無法使用時，或當應用程式明確設定為使用長輪詢時，會使用長輪詢。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-129">Long Polling is used by SignalR when WebSockets isn't available or when the app is explicitly configured to use Long Polling.</span></span> <span data-ttu-id="8e2b7-130">部署到 Azure App Service 時，請將應用程式設定為在服務的 Azure 入口網站設定中使用 Websocket。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-130">When deploying to Azure App Service, configure the app to use WebSockets in the Azure portal settings for the service.</span></span> <span data-ttu-id="8e2b7-131">如需設定應用程式以進行 Azure App Service 的詳細資訊，請參閱[ SignalR發佈指導方針](xref:signalr/publish-to-azure-web-app)。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-131">For details on configuring the app for Azure App Service, see the [SignalR publishing guidelines](xref:signalr/publish-to-azure-web-app).</span></span>

#### <a name="azure-signalr-service"></a><span data-ttu-id="8e2b7-132">Azure SignalR服務</span><span class="sxs-lookup"><span data-stu-id="8e2b7-132">Azure SignalR Service</span></span>

<span data-ttu-id="8e2b7-133">我們建議使用適用于Blazor伺服器應用程式的[Azure SignalR服務](/azure/azure-signalr)。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-133">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="8e2b7-134">此服務可讓您將Blazor伺服器應用程式相應增加至大量的並行SignalR連接。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-134">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="8e2b7-135">此外， SignalR服務的全球範圍和高效能資料中心會大幅協助減少因地理位置而造成的延遲。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-135">In addition, the SignalR service's global reach and high-performance data centers significantly aid in reducing latency due to geography.</span></span> <span data-ttu-id="8e2b7-136">若要設定應用程式（並選擇性地布建SignalR ） Azure 服務：</span><span class="sxs-lookup"><span data-stu-id="8e2b7-136">To configure an app (and optionally provision) the Azure SignalR Service:</span></span>

1. <span data-ttu-id="8e2b7-137">啟用服務以支援「固定*會話*」，在此情況下，用戶端會在進行[回溯時重新導向至相同的伺服器](xref:blazor/hosting-models#connection-to-the-server)。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-137">Enable the service to support *sticky sessions*, where clients are [redirected back to the same server when prerendering](xref:blazor/hosting-models#connection-to-the-server).</span></span> <span data-ttu-id="8e2b7-138">將`ServerStickyMode`選項或設定值設為`Required`。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-138">Set the `ServerStickyMode` option or configuration value to `Required`.</span></span> <span data-ttu-id="8e2b7-139">一般而言，應用程式會使用下列**其中一**種方法來建立設定：</span><span class="sxs-lookup"><span data-stu-id="8e2b7-139">Typically, an app creates the configuration using **one** of the following approaches:</span></span>

   * <span data-ttu-id="8e2b7-140">`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8e2b7-140">`Startup.ConfigureServices`:</span></span>
  
     ```csharp
     services.AddSignalR().AddAzureSignalR(options =>
     {
         options.ServerStickyMode = 
             Microsoft.Azure.SignalR.ServerStickyMode.Required;
     });
     ```

   * <span data-ttu-id="8e2b7-141">設定（使用下列**其中一**種方法）：</span><span class="sxs-lookup"><span data-stu-id="8e2b7-141">Configuration (use **one** of the following approaches):</span></span>
  
     * <span data-ttu-id="8e2b7-142">*appsettings. json*：</span><span class="sxs-lookup"><span data-stu-id="8e2b7-142">*appsettings.json*:</span></span>

       ```json
       "Azure:SignalR:ServerStickyMode": "Required"
       ```

     * <span data-ttu-id="8e2b7-143">Azure 入口網站（**名稱** `Azure:SignalR:ServerStickyMode`：，**值** `Required`：）**中的 app** > service**設定應用程式設定**。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-143">The app service's **Configuration** > **Application settings** in the Azure portal (**Name**: `Azure:SignalR:ServerStickyMode`, **Value**: `Required`).</span></span>

1. <span data-ttu-id="8e2b7-144">在伺服器應用程式的Blazor Visual Studio 中建立 Azure 應用程式發佈設定檔。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-144">Create an Azure Apps publish profile in Visual Studio for the Blazor Server app.</span></span>
1. <span data-ttu-id="8e2b7-145">將**Azure SignalR服務**相依性新增至設定檔。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-145">Add the **Azure SignalR Service** dependency to the profile.</span></span> <span data-ttu-id="8e2b7-146">如果 Azure 訂用帳戶沒有要指派給應用程式SignalR的既有 azure 服務實例，請選取 [**建立新的SignalR azure 服務實例**] 以布建新的服務實例。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-146">If the Azure subscription doesn't have a pre-existing Azure SignalR Service instance to assign to the app, select **Create a new Azure SignalR Service instance** to provision a new service instance.</span></span>
1. <span data-ttu-id="8e2b7-147">將應用程式發佈至 Azure。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-147">Publish the app to Azure.</span></span>

#### <a name="iis"></a><span data-ttu-id="8e2b7-148">IIS</span><span class="sxs-lookup"><span data-stu-id="8e2b7-148">IIS</span></span>

<span data-ttu-id="8e2b7-149">使用 IIS 時，請啟用：</span><span class="sxs-lookup"><span data-stu-id="8e2b7-149">When using IIS, enable:</span></span>

* <span data-ttu-id="8e2b7-150">[在 IIS 上的 websocket](xref:fundamentals/websockets#enabling-websockets-on-iis)。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-150">[WebSockets on IIS](xref:fundamentals/websockets#enabling-websockets-on-iis).</span></span>
* <span data-ttu-id="8e2b7-151">[具有應用程式要求路由的粘滯話](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-151">[Sticky sessions with Application Request Routing](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span></span>

#### <a name="kubernetes"></a><span data-ttu-id="8e2b7-152">Kubernetes</span><span class="sxs-lookup"><span data-stu-id="8e2b7-152">Kubernetes</span></span>

<span data-ttu-id="8e2b7-153">使用下列[Kubernetes 注釋來建立輸入定義：適用于粘滯會話](https://kubernetes.github.io/ingress-nginx/examples/affinity/cookie/)。。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-153">Create an ingress definition with the following [Kubernetes annotations for sticky sessions](https://kubernetes.github.io/ingress-nginx/examples/affinity/cookie/):</span></span>

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

#### <a name="linux-with-nginx"></a><span data-ttu-id="8e2b7-154">使用 Nginx 的 Linux</span><span class="sxs-lookup"><span data-stu-id="8e2b7-154">Linux with Nginx</span></span>

<span data-ttu-id="8e2b7-155">若SignalR要讓 websocket 正常運作，請確認 proxy 的`Upgrade`和`Connection`標頭已設定為下列值，且`$connection_upgrade`對應至其中一個：</span><span class="sxs-lookup"><span data-stu-id="8e2b7-155">For SignalR WebSockets to function properly, confirm that the proxy's `Upgrade` and `Connection` headers are set to the following values and that `$connection_upgrade` is mapped to either:</span></span>

* <span data-ttu-id="8e2b7-156">升級標頭值預設為。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-156">The Upgrade header value by default.</span></span>
* <span data-ttu-id="8e2b7-157">`close`當升級標頭遺失或空白時。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-157">`close` when the Upgrade header is missing or empty.</span></span>

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

<span data-ttu-id="8e2b7-158">如需詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="8e2b7-158">For more information, see the following articles:</span></span>

* [<span data-ttu-id="8e2b7-159">NGINX 做為 WebSocket Proxy</span><span class="sxs-lookup"><span data-stu-id="8e2b7-159">NGINX as a WebSocket Proxy</span></span>](https://www.nginx.com/blog/websocket-nginx/)
* [<span data-ttu-id="8e2b7-160">WebSocket 代理</span><span class="sxs-lookup"><span data-stu-id="8e2b7-160">WebSocket proxying</span></span>](http://nginx.org/docs/http/websocket.html)
* <xref:host-and-deploy/linux-nginx>

### <a name="measure-network-latency"></a><span data-ttu-id="8e2b7-161">測量網路延遲</span><span class="sxs-lookup"><span data-stu-id="8e2b7-161">Measure network latency</span></span>

<span data-ttu-id="8e2b7-162">您可以使用[JS interop](xref:blazor/call-javascript-from-dotnet)來測量網路延遲，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="8e2b7-162">[JS interop](xref:blazor/call-javascript-from-dotnet) can be used to measure network latency, as the following example demonstrates:</span></span>

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

<span data-ttu-id="8e2b7-163">如需合理的 UI 體驗，我們建議使用250毫秒或更少的持續性 UI 延遲。</span><span class="sxs-lookup"><span data-stu-id="8e2b7-163">For a reasonable UI experience, we recommend a sustained UI latency of 250ms or less.</span></span>
