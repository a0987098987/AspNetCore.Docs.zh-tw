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
# <a name="host-and-deploy-opno-locblazor-server"></a>託管與部署Blazor伺服器

作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>主機組態值

伺服器應用可以接受[通用主機設定值](xref:fundamentals/host/generic-host#host-configuration) [ Blazor ](xref:blazor/hosting-models#blazor-server) 。

## <a name="deployment"></a>部署

使用[Blazor伺服器託管模型](xref:blazor/hosting-models#blazor-server),Blazor在伺服器上執行 ASP.NET 酷應用程式。 UI 更新、事件處理和 JAVAScript[SignalR](xref:signalr/introduction)呼叫透過連接處理。

需要能夠裝載 ASP.NET Core 應用程式的網路伺服器。 Visual Studio**Blazor包括伺服器應用**`blazorserverside`專案範本 (使用[dotnet 新](/dotnet/core/tools/dotnet-new)命令時的範本)。

## <a name="scalability"></a>延展性

規劃部署以充分利用Blazor伺服器應用的可用基礎結構。 請參考以下資源來解決Blazor伺服器應用可伸縮性:

* [伺服器應用的Blazor基礎知識](xref:blazor/hosting-models#blazor-server)
* <xref:security/blazor/server>

### <a name="deployment-server"></a>部署伺服器

在考慮單個伺服器的可伸縮性(向上擴展)時,應用可用的記憶體可能是應用隨著使用者需求增加而耗盡的第一個資源。 伺服器上的可用記憶體會影響:

* 伺服器可以支援的有源電路數。
* 用戶端上的 UI 延遲。

有關建構安全和可Blazor擴充的伺服器應用的指導,請<xref:security/blazor/server>參閱 。

每個電路使用大約 250 KB 的記憶體,以進行最少的*Hello World*風格的應用。 電路的大小取決於應用的代碼以及與每個元件關聯的狀態維護要求。 我們建議您在開發應用和基礎結構期間測量資源需求,但以下基線可能是規劃部署目標的起點:如果您希望應用支援 5,000 個併發使用者,請考慮為應用至少預算 1.3 GB 的伺服器記憶體(或每個使用者 273 KB)。

### <a name="opno-locsignalr-configuration"></a>SignalR設定

Blazor伺服器應用使用SignalRASP.NET 核心與瀏覽器通信。 託管和縮放條件適用於[SignalR](xref:signalr/publish-to-azure-web-app)Blazor伺服器應用。

Blazor由於延遲、可靠性和[安全性](xref:signalr/security)較低,使用SignalRWebSocket 作為傳輸時效果最佳。 當 WebSocketSignalR不可用或應用被顯式配置為使用長輪詢時,使用長輪詢。 部署到 Azure 應用服務時,請將應用配置為在服務的 Azure 門戶設置中使用 WebSocket。 有關為 Azure 應用服務設定應用的詳細資訊,[SignalR請參閱發佈指南](xref:signalr/publish-to-azure-web-app)。

#### <a name="azure-opno-locsignalr-service"></a>AzureSignalR服務

我們建議將[AzureSignalR服務](/azure/azure-signalr)用於Blazor伺服器應用。 該服務允許將Blazor伺服器應用擴展到大量併SignalR發 連接。 此外,SignalR該服務的全球覆蓋範圍和高性能資料中心大大有助於減少因地理位置而導致的延遲。 要配置應用(並選擇性預配)AzureSignalR服務:

1. 使服務支援*粘滯會話*,其中用戶端在[預算時重定向回同一伺服器](xref:blazor/hosting-models#connection-to-the-server)。 設定`ServerStickyMode`值或設定值值值`Required`為 。 通常,應用使用以下方法**之一**創建配置:

   * `Startup.ConfigureServices`:
  
     ```csharp
     services.AddSignalR().AddAzureSignalR(options =>
     {
         options.ServerStickyMode = 
             Microsoft.Azure.SignalR.ServerStickyMode.Required;
     });
     ```

   * 設定(使用以下方法**之一**):
  
     * *應用程式設定.json*:

       ```json
       "Azure:SignalR:ServerStickyMode": "Required"
       ```

     * 套用服務在 Azure 門戶中的**設定** > `Azure:SignalR:ServerStickyMode`**應用程式設定**( `Required`**名稱**: 、**數值**): 。

1. 在「Blazor伺服器」應用的可視化工作室中創建 Azure 應用發佈配置檔。
1. 將**AzureSignalR服務**依賴項添加到配置檔。 如果 Azure 訂閱沒有預先存在的SignalRAzure 服務實例分配給應用,請選擇 **「SignalR創建新 Azure 服務實例**以預配新的服務實例」。
1. 將應用程式發佈至 Azure。

#### <a name="iis"></a>IIS

使用 IIS 時,啟用:

* [IIS 上的 Web 插座](xref:fundamentals/websockets#enabling-websockets-on-iis)。
* [具有應用程式要求路由的粘滯工作階段](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)。

#### <a name="kubernetes"></a>Kubernetes

建立具有以下[Kubernetes 註釋的黏滯工作階段](https://kubernetes.github.io/ingress-nginx/examples/affinity/cookie/)入口定義:

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

對於SignalRWebSocket 正常工作,請確認`Upgrade``Connection`代理和標頭設定為以下`$connection_upgrade`值, 並映射到以下任一值:

* 默認情況下,「升級標頭」值。
* `close`當升級標頭丟失或為空時。

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

* [NGINX 為 WebSocket 代理程式](https://www.nginx.com/blog/websocket-nginx/)
* [WebSocket 代理程式](http://nginx.org/docs/http/websocket.html)
* <xref:host-and-deploy/linux-nginx>

### <a name="measure-network-latency"></a>測量網路延遲

[JS 互通](xref:blazor/call-javascript-from-dotnet)可用於測量網路延遲,如下例所示:

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

為了獲得合理的 UI 體驗,我們建議持續延遲 250ms 或更少。
