---
title: SignalR 和 ASP.NET Core SignalR 之間的差異
author: tdykstra
description: SignalR 和 ASP.NET Core SignalR 之間的差異
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 11/14/2018
uid: signalr/version-differences
ms.openlocfilehash: fb10d6e62ff28128e6e9e5dcef55e44f25de8ad0
ms.sourcegitcommit: 6548c19f345850ee22b50f7ef9fca732895d9e08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2018
ms.locfileid: "53425116"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a>ASP.NET SignalR 及 ASP.NET Core SignalR 之間的差異

ASP.NET Core SignalR 與不相容用戶端或為 ASP.NET SignalR 的伺服器。 本文詳細說明已經被移除，或在 ASP.NET Core SignalR 中變更的功能。

## <a name="how-to-identify-the-signalr-version"></a>如何找出 SignalR 版本

|                      | ASP.NET SignalR | ASP.NET Core SignalR |
| -------------------- | --------------- | -------------------- |
| 伺服器 NuGet 套件 | [Microsoft.AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)<br>[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework) |
| 用戶端 NuGet 套件 | [Microsoft.AspNet.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[Microsoft.AspNet.SignalR.JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| 用戶端 npm 套件 | [signalr](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| Java 用戶端 | [GitHub 存放庫](https://github.com/SignalR/java-client)（已過時）  | Maven 套件[com.microsoft.signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr) |
| 伺服器應用程式類型 | ASP.NET (System.Web) 或 OWIN 自我裝載 | ASP.NET Core |
| 支援的伺服器平台 | .NET framework 4.5 或更新版本 | .NET Framework 4.6.1 或更新版本<br>.NET core 2.1 或更新版本 |

## <a name="feature-differences"></a>功能差異

### <a name="automatic-reconnects"></a>自動重新連線

ASP.NET Core SignalR 並不支援自動重新連線。 如果用戶端已中斷連接，使用者必須明確啟動新的連線如果他們想要重新連線。 在 ASP.NET SignalR、 SignalR 會嘗試重新連線到伺服器，如果連接已卸除。 

### <a name="protocol-support"></a>通訊協定支援

ASP.NET Core SignalR 支援 JSON，以及新的二進位通訊協定，以根據[MessagePack](xref:signalr/messagepackhubprotocol)。 此外，您可以建立自訂通訊協定。

### <a name="transports"></a>傳輸

在 ASP.NET Core SignalR 中不支援永久框架傳輸。

## <a name="differences-on-the-server"></a>在伺服器上的差異

ASP.NET Core SignalR 的伺服器端程式庫都會納入[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)封裝的一部分**ASP.NET Core Web 應用程式**Razor 和 MVC 範本專案。

ASP.NET Core SignalR 是 ASP.NET Core 中介軟體，因此必須由呼叫[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)在`Startup.ConfigureServices`。

```csharp
services.AddSignalR()
```

若要設定路由，請將路由對應至中樞內[UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr)方法呼叫中`Startup.Configure`方法。

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions"></a>黏性工作階段

ASP.NET SignalR 的向外延展模型可讓用戶端重新連線，並將訊息傳送至伺服器陣列中的任何伺服器。 在 ASP.NET Core SignalR 用戶端必須連線的持續時間與相同的伺服器互動。 使用 Redis 範圍外，這表示黏性工作階段所需。 使用向外延展[Azure SignalR 服務](/azure/azure-signalr/)，因為此服務會處理用戶端連線，不需要黏性工作階段。 

### <a name="single-hub-per-connection"></a>每個連線單一中樞

在 ASP.NET Core SignalR 連線模型已簡化。 連線會對直接單一中樞中，而不是正在使用共用存取權的多個中樞單一連接。

### <a name="streaming"></a>資料流

ASP.NET Core SignalR 現在支援[串流資料](xref:signalr/streaming)從用戶端中樞。

### <a name="state"></a>狀況

能夠將任意的狀態傳遞用戶端與中樞 （通常稱為 HubState） 之間已移除，以及支援進度訊息。 目前沒有任何對應的中樞 proxy。

### <a name="persistentconnection-removal"></a>PersistentConnection 移除

在 ASP.NET Core SignalR [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118))類別已移除。 

### <a name="globalhost"></a>GlobalHost

ASP.NET Core 具有內建於 framework 的相依性插入 (DI)。 服務可以存取使用 DI [HubContext](xref:signalr/hubcontext)。 `GlobalHost`物件，以取得用於 ASP.NET SignalR`HubContext`不存在於 ASP.NET Core SignalR。

### <a name="hubpipeline"></a>HubPipeline

ASP.NET Core SignalR 沒有支援`HubPipeline`模組。

## <a name="differences-on-the-client"></a>在用戶端上的差異

### <a name="typescript"></a>TypeScript

ASP.NET Core SignalR 用戶端以[TypeScript](https://www.typescriptlang.org/)。 使用時，您可以撰寫以 JavaScript 或 TypeScript [JavaScript 用戶端](xref:signalr/javascript-client)。

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>JavaScript 用戶端裝載於[npm](https://www.npmjs.com/)

在舊版中，JavaScript 用戶端已取得透過 Visual Studio 中的 NuGet 套件。 如需核心版本中， [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) npm 套件所包含的 JavaScript 程式庫。 此套件不會包含於**ASP.NET Core Web 應用程式**範本。 使用 npm 來取得並安裝`@aspnet/signalr`npm 套件。

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

已移除對 jQuery 的相依性，但專案仍然可以使用 jQuery。

### <a name="internet-explorer-support"></a>Internet Explorer 支援

ASP.NET Core SignalR 需要 Microsoft Internet Explorer 11 或更新版本 （ASP.NET SignalR 支援 Microsoft Internet Explorer 8 和更新版本）。

### <a name="javascript-client-method-syntax"></a>JavaScript 用戶端方法語法

JavaScript 語法已從舊版 SignalR 的變更。 而不是使用`$connection`物件，請建立連線，使用[HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

使用[上](/javascript/api/@aspnet/signalr/HubConnection#on)方法，以指定用戶端中樞可以呼叫的方法。

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

建立用戶端方法之後, 啟動中樞連線。 鏈結[攔截](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch)記錄或處理錯誤的方法。

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>中樞 proxy

中樞 proxy 不會再自動產生。 相反地，在方法名稱會傳遞至[叫用](/javascript/api/%40aspnet/signalr/hubconnection#invoke)API 做為字串。

### <a name="net-and-other-clients"></a>.NET 和其他用戶端

`Microsoft.AspNetCore.SignalR.Client` NuGet 套件包含.NET 用戶端程式庫的 ASP.NET Core SignalR。

使用[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)來建立及建置連接到中樞執行個體。

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a>向外延展的差異

ASP.NET SignalR 支援 SQL Server 和 Redis。 ASP.NET Core SignalR 支援 Azure SignalR 服務和 Redis。

### <a name="aspnet"></a>ASP.NET

* [SignalR 向外延展與 Azure 服務匯流排](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [使用 Redis 的 SignalR 向外延展](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [SignalR 向外延展與 SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a>ASP.NET Core

* [Azure SignalR Service](/azure/azure-signalr/)

## <a name="additional-resources"></a>其他資源

* [中樞](xref:signalr/hubs)
* [JavaScript 用戶端](xref:signalr/javascript-client)
* [.NET 用戶端](xref:signalr/dotnet-client)
* [支援的平台](xref:signalr/supported-platforms)
