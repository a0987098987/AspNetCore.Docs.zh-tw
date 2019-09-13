---
title: 裝載和部署 ASP.NET Core Blazor 伺服器
author: guardrex
description: 瞭解如何使用 ASP.NET Core 來裝載和部署 Blazor 伺服器應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/07/2019
uid: host-and-deploy/blazor/server
ms.openlocfilehash: a393d620924d847e674a09972515a8130a15fc6a
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/13/2019
ms.locfileid: "70964279"
---
# <a name="host-and-deploy-blazor-server"></a><span data-ttu-id="d82c7-103">裝載和部署 Blazor 伺服器</span><span class="sxs-lookup"><span data-stu-id="d82c7-103">Host and deploy Blazor Server</span></span>

<span data-ttu-id="d82c7-104">作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="d82c7-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="d82c7-105">主機組態值</span><span class="sxs-lookup"><span data-stu-id="d82c7-105">Host configuration values</span></span>

<span data-ttu-id="d82c7-106">[Blazor 伺服器應用程式](xref:blazor/hosting-models#blazor-server)可以接受[一般主機設定值](xref:fundamentals/host/generic-host#host-configuration)。</span><span class="sxs-lookup"><span data-stu-id="d82c7-106">[Blazor Server apps](xref:blazor/hosting-models#blazor-server) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="d82c7-107">部署</span><span class="sxs-lookup"><span data-stu-id="d82c7-107">Deployment</span></span>

<span data-ttu-id="d82c7-108">使用[Blazor 伺服器裝載模型](xref:blazor/hosting-models#blazor-server)，Blazor 會在伺服器上從 ASP.NET Core 應用程式中執行。</span><span class="sxs-lookup"><span data-stu-id="d82c7-108">Using the [Blazor Server hosting model](xref:blazor/hosting-models#blazor-server), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="d82c7-109">UI 更新、事件處理及 JavaScript 呼叫會透過 [SignalR](xref:signalr/introduction) 連線處理。</span><span class="sxs-lookup"><span data-stu-id="d82c7-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="d82c7-110">需要能夠裝載 ASP.NET Core 應用程式的網路伺服器。</span><span class="sxs-lookup"><span data-stu-id="d82c7-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="d82c7-111">Visual Studio 會包含 **Blazor 伺服器應用程式**專案範本 (使用 [dotnet new](/dotnet/core/tools/dotnet-new) 命令時為 `blazorserverside` 範本)。</span><span class="sxs-lookup"><span data-stu-id="d82c7-111">Visual Studio includes the **Blazor Server App** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

## <a name="scalability"></a><span data-ttu-id="d82c7-112">延展性</span><span class="sxs-lookup"><span data-stu-id="d82c7-112">Scalability</span></span>

<span data-ttu-id="d82c7-113">規劃部署，以充分利用適用于 Blazor 伺服器應用程式的基礎結構。</span><span class="sxs-lookup"><span data-stu-id="d82c7-113">Plan a deployment to make the best use of the available infrastructure for a Blazor Server app.</span></span> <span data-ttu-id="d82c7-114">請參閱下列資源，以解決 Blazor 伺服器應用程式的擴充性：</span><span class="sxs-lookup"><span data-stu-id="d82c7-114">See the following resources to address Blazor Server app scalability:</span></span>

* [<span data-ttu-id="d82c7-115">Blazor 伺服器應用程式的基本概念</span><span class="sxs-lookup"><span data-stu-id="d82c7-115">Fundamentals of Blazor Server apps</span></span>](xref:blazor/hosting-models#blazor-server)
* <xref:security/blazor/server>

### <a name="deployment-server"></a><span data-ttu-id="d82c7-116">部署伺服器</span><span class="sxs-lookup"><span data-stu-id="d82c7-116">Deployment server</span></span>

<span data-ttu-id="d82c7-117">在考慮單一伺服器的擴充性（相應增加）時，應用程式可用的記憶體可能是應用程式在使用者需求增加時將耗盡的第一個資源。</span><span class="sxs-lookup"><span data-stu-id="d82c7-117">When considering the scalability of a single server (scale up), the memory available to an app is likely the first resource that the app will exhaust as user demands increase.</span></span> <span data-ttu-id="d82c7-118">伺服器上的可用記憶體會影響：</span><span class="sxs-lookup"><span data-stu-id="d82c7-118">The available memory on the server affects the:</span></span>

* <span data-ttu-id="d82c7-119">伺服器可支援的現用線路數目。</span><span class="sxs-lookup"><span data-stu-id="d82c7-119">Number of active circuits that a server can support.</span></span>
* <span data-ttu-id="d82c7-120">用戶端上的 UI 延遲。</span><span class="sxs-lookup"><span data-stu-id="d82c7-120">UI latency on the client.</span></span>

<span data-ttu-id="d82c7-121">如需建立安全且可擴充的 Blazor 伺服器應用程式的<xref:security/blazor/server>指引，請參閱。</span><span class="sxs-lookup"><span data-stu-id="d82c7-121">For guidance on building secure and scalable Blazor server apps, see <xref:security/blazor/server>.</span></span>

<span data-ttu-id="d82c7-122">每個線路都使用大約 250 KB 的記憶體來進行最小的*Hello World*樣式應用程式。</span><span class="sxs-lookup"><span data-stu-id="d82c7-122">Each circuit uses approximately 250 KB of memory for a minimal *Hello World*-style app.</span></span> <span data-ttu-id="d82c7-123">線路的大小取決於應用程式的程式碼，以及與每個元件相關聯的狀態維護需求。</span><span class="sxs-lookup"><span data-stu-id="d82c7-123">The size of a circuit depends on the app's code and the state maintenance requirements associated with each component.</span></span> <span data-ttu-id="d82c7-124">我們建議您在開發應用程式和基礎結構期間測量資源需求，但下列基準可以是規劃部署目標的起點：如果您預期您的應用程式支援5000並行使用者，請考慮將至少 1.3 GB 的伺服器記憶體預算給應用程式（或每位使用者 ~ 273 KB）。</span><span class="sxs-lookup"><span data-stu-id="d82c7-124">We recommend that you measure resource demands during development for your app and infrastructure, but the following baseline can be a starting point in planning your deployment target: If you expect your app to support 5,000 concurrent users, consider budgeting at least 1.3 GB of server memory to the app (or ~273 KB per user).</span></span>

### <a name="signalr-configuration"></a><span data-ttu-id="d82c7-125">SignalR 設定</span><span class="sxs-lookup"><span data-stu-id="d82c7-125">SignalR configuration</span></span>

<span data-ttu-id="d82c7-126">Blazor 伺服器應用程式會使用 ASP.NET Core SignalR 來與瀏覽器通訊。</span><span class="sxs-lookup"><span data-stu-id="d82c7-126">Blazor Server apps use ASP.NET Core SignalR to communicate with the browser.</span></span> <span data-ttu-id="d82c7-127">[SignalR 的裝載和調整規模條件](xref:signalr/publish-to-azure-web-app)適用于 Blazor 伺服器應用程式。</span><span class="sxs-lookup"><span data-stu-id="d82c7-127">[SignalR's hosting and scaling conditions](xref:signalr/publish-to-azure-web-app) apply to Blazor Server apps.</span></span>

<span data-ttu-id="d82c7-128">因為延遲、可靠性和[安全性](xref:signalr/security)較低，所以使用 Websocket 作為 SignalR 傳輸時，Blazor 的效果最佳。</span><span class="sxs-lookup"><span data-stu-id="d82c7-128">Blazor works best when using WebSockets as the SignalR transport due to lower latency, reliability, and [security](xref:signalr/security).</span></span> <span data-ttu-id="d82c7-129">當 Websocket 無法使用時，或當應用程式明確設定為使用長輪詢時，SignalR 會使用長輪詢。</span><span class="sxs-lookup"><span data-stu-id="d82c7-129">Long Polling is used by SignalR when WebSockets isn't available or when the app is explicitly configured to use Long Polling.</span></span> <span data-ttu-id="d82c7-130">部署到 Azure App Service 時，請將應用程式設定為在服務的 Azure 入口網站設定中使用 Websocket。</span><span class="sxs-lookup"><span data-stu-id="d82c7-130">When deploying to Azure App Service, configure the app to use WebSockets in the Azure portal settings for the service.</span></span> <span data-ttu-id="d82c7-131">如需設定應用程式以進行 Azure App Service 的詳細資訊，請參閱[SignalR 發行指導方針](xref:signalr/publish-to-azure-web-app)。</span><span class="sxs-lookup"><span data-stu-id="d82c7-131">For details on configuring the app for Azure App Service, see the [SignalR publishing guidelines](xref:signalr/publish-to-azure-web-app).</span></span>

<span data-ttu-id="d82c7-132">我們建議使用 Blazor 伺服器應用程式的[Azure SignalR Service](/azure/azure-signalr) 。</span><span class="sxs-lookup"><span data-stu-id="d82c7-132">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="d82c7-133">此服務可讓您將 Blazor 伺服器應用程式相應增加為大量的並行 SignalR 連接。</span><span class="sxs-lookup"><span data-stu-id="d82c7-133">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="d82c7-134">此外，SignalR 服務的全球範圍和高效能資料中心會大幅協助減少因地理位置而造成的延遲。</span><span class="sxs-lookup"><span data-stu-id="d82c7-134">In addition, the SignalR service's global reach and high-performance data centers significantly aid in reducing latency due to geography.</span></span>

### <a name="measure-network-latency"></a><span data-ttu-id="d82c7-135">測量網路延遲</span><span class="sxs-lookup"><span data-stu-id="d82c7-135">Measure network latency</span></span>

<span data-ttu-id="d82c7-136">您可以使用[JS interop](xref:blazor/javascript-interop)來測量網路延遲，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="d82c7-136">[JS interop](xref:blazor/javascript-interop) can be used to measure network latency, as the following example demonstrates:</span></span>

```cshtml
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

<span data-ttu-id="d82c7-137">如需合理的 UI 體驗，我們建議使用250毫秒或更少的持續性 UI 延遲。</span><span class="sxs-lookup"><span data-stu-id="d82c7-137">For a reasonable UI experience, we recommend a sustained UI latency of 250ms or less.</span></span>
