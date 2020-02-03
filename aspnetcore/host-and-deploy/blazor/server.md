---
title: 裝載和部署 ASP.NET Core Blazor 伺服器
author: guardrex
description: 瞭解如何使用 ASP.NET Core 裝載和部署 Blazor 伺服器應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/17/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/server
ms.openlocfilehash: e8b3a7faaf1dc88059a79abbc7e74657ebb2068c
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726737"
---
# <a name="host-and-deploy-opno-locblazor-server"></a>裝載和部署 Blazor 伺服器

作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>主機組態值

[Blazor 伺服器應用程式](xref:blazor/hosting-models#blazor-server)可以接受[一般主機設定值](xref:fundamentals/host/generic-host#host-configuration)。

## <a name="deployment"></a>部署

使用[Blazor 伺服器裝載模型](xref:blazor/hosting-models#blazor-server)，Blazor 會在伺服器上從 ASP.NET Core 應用程式中執行。 UI 更新、事件處理及 JavaScript 呼叫會透過[SignalR](xref:signalr/introduction)連接來處理。

需要能夠裝載 ASP.NET Core 應用程式的網路伺服器。 Visual Studio 包含 **Blazor 伺服器應用程式**專案範本（使用[dotnet new](/dotnet/core/tools/dotnet-new)命令時的`blazorserverside` 範本）。

## <a name="scalability"></a>延展性

規劃部署，以充分運用 Blazor 伺服器應用程式可用的基礎結構。 請參閱下列資源，以解決 Blazor 伺服器應用程式的擴充性：

* [Blazor 伺服器應用程式的基本概念](xref:blazor/hosting-models#blazor-server)
* <xref:security/blazor/server>

### <a name="deployment-server"></a>部署伺服器

在考慮單一伺服器的擴充性（相應增加）時，應用程式可用的記憶體可能是應用程式在使用者需求增加時將耗盡的第一個資源。 伺服器上的可用記憶體會影響：

* 伺服器可支援的現用線路數目。
* 用戶端上的 UI 延遲。

如需建立安全且可擴充的 Blazor 伺服器應用程式的指引，請參閱 <xref:security/blazor/server>。

每個線路都使用大約 250 KB 的記憶體來進行最小的*Hello World*樣式應用程式。 線路的大小取決於應用程式的程式碼，以及與每個元件相關聯的狀態維護需求。 我們建議您在開發應用程式和基礎結構期間測量資源需求，但下列基準可以是規劃部署目標的起點：如果您預期應用程式支援5000並行使用者，請考慮在應用程式最少 1.3 GB 的伺服器記憶體（或每位使用者約 273 KB）。

### <a name="opno-locsignalr-configuration"></a>SignalR 設定

Blazor 伺服器應用程式會使用 ASP.NET Core SignalR 來與瀏覽器通訊。 [SignalR的裝載和調整規模條件](xref:signalr/publish-to-azure-web-app)適用于 Blazor 伺服器應用程式。

因為延遲、可靠性和[安全性](xref:signalr/security)較低，所以使用 websocket 作為 SignalR 傳輸時，Blazor 的效果最佳。 當 Websocket 無法使用時，或當應用程式明確設定為使用長輪詢時，SignalR 會使用長輪詢。 部署到 Azure App Service 時，請將應用程式設定為在服務的 Azure 入口網站設定中使用 Websocket。 如需設定應用程式以進行 Azure App Service 的詳細資訊，請參閱[SignalR 發佈指導方針](xref:signalr/publish-to-azure-web-app)。

#### <a name="azure-opno-locsignalr-service"></a>Azure SignalR 服務

我們建議使用適用于 Blazor 伺服器應用程式的[Azure SignalR 服務](/azure/azure-signalr)。 此服務可將 Blazor 伺服器應用程式相應增加為大量的並行 SignalR 連線。 此外，SignalR 服務的全球範圍和高效能資料中心會大幅協助減少因地理位置而造成的延遲。 若要設定應用程式（並選擇性地布建） Azure SignalR 服務：

1. 啟用服務以支援「固定*會話*」，在此情況下，用戶端會在進行[回溯時重新導向至相同的伺服器](xref:blazor/hosting-models#reconnection-to-the-same-server)。 將 [`ServerStickyMode` 選項] 或 [設定] 值設為 [`Required`]。 一般而言，應用程式會使用下列**其中一**種方法來建立設定：

   * `Startup.ConfigureServices`:
  
     ```csharp
     services.AddSignalR().AddAzureSignalR(options =>
     {
         options.ServerStickyMode = 
             Microsoft.Azure.SignalR.ServerStickyMode.Required;
     });
     ```

   * 設定（使用下列**其中一**種方法）：
  
     * *appsettings.json*：

       ```json
       "Azure:SignalR:ServerStickyMode": "Required"
       ```

     * App service 的設定 ** > Azure 入口網站中的** **應用程式設定**（**名稱**： `Azure:SignalR:ServerStickyMode`，**值**： `Required`）。

1. 在 Blazor 伺服器應用程式的 Visual Studio 中建立 Azure 應用程式發佈設定檔。
1. 將**Azure SignalR 服務**相依性新增至設定檔。 如果 Azure 訂用帳戶沒有要指派給應用程式的既有 Azure SignalR 服務實例，請選取 [**建立新的 azure SignalR 服務實例**] 以布建新的服務實例。
1. 將應用程式發佈至 Azure。

#### <a name="iis"></a>IIS

使用 IIS 時，會使用應用程式要求路由來啟用「粘滯會話」。 如需詳細資訊，請參閱[使用應用程式要求路由的 HTTP 負載平衡](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)。

#### <a name="kubernetes"></a>Kubernetes

使用下列[Kubernetes 注釋來建立輸入定義：適用于粘滯會話](https://kubernetes.github.io/ingress-nginx/examples/affinity/cookie/)。

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

#### <a name="linux-with-nginx"></a>使用 Nginx 的 Linux

若要讓 SignalR Websocket 正常運作，請將 proxy 的 `Upgrade` 和 `Connection` 標頭設定為下列內容：

```
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection $connection_upgrade;
```

如需詳細資訊，請參閱[NGINX as a WebSocket Proxy](https://www.nginx.com/blog/websocket-nginx/)。

### <a name="measure-network-latency"></a>測量網路延遲

您可以使用[JS interop](xref:blazor/javascript-interop)來測量網路延遲，如下列範例所示：

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

如需合理的 UI 體驗，我們建議使用250毫秒或更少的持續性 UI 延遲。
