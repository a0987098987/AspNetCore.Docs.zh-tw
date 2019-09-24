---
title: 裝載和部署 ASP.NET Core Blazor 伺服器
author: guardrex
description: 瞭解如何使用 ASP.NET Core 來裝載和部署 Blazor 伺服器應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: host-and-deploy/blazor/server
ms.openlocfilehash: aedef7fe695dd4a0cbf04d3f3e9947f33f7afa40
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2019
ms.locfileid: "71211615"
---
# <a name="host-and-deploy-blazor-server"></a>裝載和部署 Blazor 伺服器

作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>主機組態值

[Blazor 伺服器應用程式](xref:blazor/hosting-models#blazor-server)可以接受[一般主機設定值](xref:fundamentals/host/generic-host#host-configuration)。

## <a name="deployment"></a>部署

使用[Blazor 伺服器裝載模型](xref:blazor/hosting-models#blazor-server)，Blazor 會在伺服器上從 ASP.NET Core 應用程式中執行。 UI 更新、事件處理及 JavaScript 呼叫會透過 [SignalR](xref:signalr/introduction) 連線處理。

需要能夠裝載 ASP.NET Core 應用程式的網路伺服器。 Visual Studio 會包含 **Blazor 伺服器應用程式**專案範本 (使用 [dotnet new](/dotnet/core/tools/dotnet-new) 命令時為 `blazorserverside` 範本)。

## <a name="scalability"></a>延展性

規劃部署，以充分利用適用于 Blazor 伺服器應用程式的基礎結構。 請參閱下列資源，以解決 Blazor 伺服器應用程式的擴充性：

* [Blazor 伺服器應用程式的基本概念](xref:blazor/hosting-models#blazor-server)
* <xref:security/blazor/server>

### <a name="deployment-server"></a>部署伺服器

在考慮單一伺服器的擴充性（相應增加）時，應用程式可用的記憶體可能是應用程式在使用者需求增加時將耗盡的第一個資源。 伺服器上的可用記憶體會影響：

* 伺服器可支援的現用線路數目。
* 用戶端上的 UI 延遲。

如需建立安全且可擴充的 Blazor 伺服器應用程式的<xref:security/blazor/server>指引，請參閱。

每個線路都使用大約 250 KB 的記憶體來進行最小的*Hello World*樣式應用程式。 線路的大小取決於應用程式的程式碼，以及與每個元件相關聯的狀態維護需求。 我們建議您在開發應用程式和基礎結構期間測量資源需求，但下列基準可以是規劃部署目標的起點：如果您預期您的應用程式支援5000並行使用者，請考慮將至少 1.3 GB 的伺服器記憶體預算給應用程式（或每位使用者 ~ 273 KB）。

### <a name="signalr-configuration"></a>SignalR 設定

Blazor 伺服器應用程式會使用 ASP.NET Core SignalR 來與瀏覽器通訊。 [SignalR 的裝載和調整規模條件](xref:signalr/publish-to-azure-web-app)適用于 Blazor 伺服器應用程式。

因為延遲、可靠性和[安全性](xref:signalr/security)較低，所以使用 Websocket 作為 SignalR 傳輸時，Blazor 的效果最佳。 當 Websocket 無法使用時，或當應用程式明確設定為使用長輪詢時，SignalR 會使用長輪詢。 部署到 Azure App Service 時，請將應用程式設定為在服務的 Azure 入口網站設定中使用 Websocket。 如需設定應用程式以進行 Azure App Service 的詳細資訊，請參閱[SignalR 發行指導方針](xref:signalr/publish-to-azure-web-app)。

我們建議使用 Blazor 伺服器應用程式的[Azure SignalR Service](/azure/azure-signalr) 。 此服務可讓您將 Blazor 伺服器應用程式相應增加為大量的並行 SignalR 連接。 此外，SignalR 服務的全球範圍和高效能資料中心會大幅協助減少因地理位置而造成的延遲。

### <a name="measure-network-latency"></a>測量網路延遲

您可以使用[JS interop](xref:blazor/javascript-interop)來測量網路延遲，如下列範例所示：

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

如需合理的 UI 體驗，我們建議使用250毫秒或更少的持續性 UI 延遲。
