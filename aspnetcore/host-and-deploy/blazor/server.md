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
# <a name="host-and-deploy-blazor-server"></a>裝載和部署Blazor伺服器

作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>主機組態值

伺服器應用程式可以接受[一般主機設定值](xref:fundamentals/host/generic-host#host-configuration)。 [ Blazor ](xref:blazor/hosting-models#blazor-server)

## <a name="deployment"></a>部署

使用[ Blazor伺服器裝載模型](xref:blazor/hosting-models#blazor-server)， Blazor會在伺服器上從 ASP.NET Core 應用程式中執行。 UI 更新、事件處理及 JavaScript 呼叫會透過[SignalR](xref:signalr/introduction)連接來處理。

需要能夠裝載 ASP.NET Core 應用程式的網路伺服器。 Visual Studio 包括** Blazor伺服器應用程式**專案範本（`blazorserverside`使用[dotnet new](/dotnet/core/tools/dotnet-new)命令時的範本）。

## <a name="scalability"></a>延展性

規劃部署，以充分利用Blazor伺服器應用程式可用的基礎結構。 請參閱下列資源，以Blazor解決伺服器應用程式的擴充性：

* [伺服器應用Blazor程式的基本概念](xref:blazor/hosting-models#blazor-server)
* <xref:security/blazor/server/threat-mitigation>

### <a name="deployment-server"></a>部署伺服器

在考慮單一伺服器的擴充性（相應增加）時，應用程式可用的記憶體可能是應用程式在使用者需求增加時將耗盡的第一個資源。 伺服器上的可用記憶體會影響：

* 伺服器可支援的現用線路數目。
* 用戶端上的 UI 延遲。

如需建立安全且可擴充Blazor之伺服器應用程式的<xref:security/blazor/server/threat-mitigation>指引，請參閱。

每個線路都使用大約 250 KB 的記憶體來進行最小的*Hello World*樣式應用程式。 線路的大小取決於應用程式的程式碼，以及與每個元件相關聯的狀態維護需求。 我們建議您在開發應用程式和基礎結構期間測量資源需求，但下列基準可以是規劃部署目標的起點：如果您預期應用程式支援5000並行使用者，請考慮將至少 1.3 GB 的伺服器記憶體預算給應用程式（或每位使用者約 273 KB）。

### <a name="signalr-configuration"></a>SignalR配置

Blazor伺服器應用程式會SignalR使用 ASP.NET Core 來與瀏覽器通訊。 [的裝載和調整規模條件適用于伺服器應用程式。 SignalR ](xref:signalr/publish-to-azure-web-app) Blazor

Blazor因為延遲、可靠性和[安全性](xref:signalr/security)較SignalR低，所以使用 websocket 做為傳輸時，效果最佳。 SignalR當 websocket 無法使用時，或當應用程式明確設定為使用長輪詢時，會使用長輪詢。 部署到 Azure App Service 時，請將應用程式設定為在服務的 Azure 入口網站設定中使用 Websocket。 如需設定應用程式以進行 Azure App Service 的詳細資訊，請參閱[ SignalR發佈指導方針](xref:signalr/publish-to-azure-web-app)。

#### <a name="azure-signalr-service"></a>Azure SignalR服務

我們建議使用適用于Blazor伺服器應用程式的[Azure SignalR服務](/azure/azure-signalr)。 此服務可讓您將Blazor伺服器應用程式相應增加至大量的並行SignalR連接。 此外， SignalR服務的全球範圍和高效能資料中心會大幅協助減少因地理位置而造成的延遲。 若要設定應用程式（並選擇性地布建SignalR ） Azure 服務：

1. 啟用服務以支援「固定*會話*」，在此情況下，用戶端會在進行[回溯時重新導向至相同的伺服器](xref:blazor/hosting-models#connection-to-the-server)。 將`ServerStickyMode`選項或設定值設為`Required`。 一般而言，應用程式會使用下列**其中一**種方法來建立設定：

   * `Startup.ConfigureServices`:
  
     ```csharp
     services.AddSignalR().AddAzureSignalR(options =>
     {
         options.ServerStickyMode = 
             Microsoft.Azure.SignalR.ServerStickyMode.Required;
     });
     ```

   * 設定（使用下列**其中一**種方法）：
  
     * *appsettings. json*：

       ```json
       "Azure:SignalR:ServerStickyMode": "Required"
       ```

     * Azure 入口網站（**名稱** `Azure:SignalR:ServerStickyMode`：，**值** `Required`：）**中的 app** > service**設定應用程式設定**。

1. 在伺服器應用程式的Blazor Visual Studio 中建立 Azure 應用程式發佈設定檔。
1. 將**Azure SignalR服務**相依性新增至設定檔。 如果 Azure 訂用帳戶沒有要指派給應用程式SignalR的既有 azure 服務實例，請選取 [**建立新的SignalR azure 服務實例**] 以布建新的服務實例。
1. 將應用程式發佈至 Azure。

#### <a name="iis"></a>IIS

使用 IIS 時，請啟用：

* [在 IIS 上的 websocket](xref:fundamentals/websockets#enabling-websockets-on-iis)。
* [具有應用程式要求路由的粘滯話](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)。

#### <a name="kubernetes"></a>Kubernetes

使用下列[Kubernetes 注釋來建立輸入定義：適用于粘滯會話](https://kubernetes.github.io/ingress-nginx/examples/affinity/cookie/)。。

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

若SignalR要讓 websocket 正常運作，請確認 proxy 的`Upgrade`和`Connection`標頭已設定為下列值，且`$connection_upgrade`對應至其中一個：

* 升級標頭值預設為。
* `close`當升級標頭遺失或空白時。

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

如需詳細資訊，請參閱下列文章：

* [NGINX 做為 WebSocket Proxy](https://www.nginx.com/blog/websocket-nginx/)
* [WebSocket 代理](http://nginx.org/docs/http/websocket.html)
* <xref:host-and-deploy/linux-nginx>

### <a name="measure-network-latency"></a>測量網路延遲

您可以使用[JS interop](xref:blazor/call-javascript-from-dotnet)來測量網路延遲，如下列範例所示：

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
