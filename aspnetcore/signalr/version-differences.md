---
title: SignalR 和 ASP.NET Core SignalR 之間的差異
author: bradygaster
description: SignalR 和 ASP.NET Core SignalR 之間的差異
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.date: 11/14/2018
uid: signalr/version-differences
ms.openlocfilehash: 70b09493d9b4c96c897465d60e53e93a793c42f9
ms.sourcegitcommit: 387cf29f5d5addef2cbc70670a11d612806b36b2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2019
ms.locfileid: "70746543"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a>ASP.NET SignalR 與 ASP.NET Core SignalR 之間的差異

ASP.NET Core SignalR 與 ASP.NET SignalR 的用戶端或伺服器不相容。 本文詳述已在 ASP.NET Core SignalR 中移除或變更的功能。

## <a name="how-to-identify-the-signalr-version"></a>如何識別 SignalR 版本

|                      | ASP.NET SignalR | ASP.NET Core SignalR |
| -------------------- | --------------- | -------------------- |
| 伺服器 NuGet 封裝 | [Microsoft.AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)<br>[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework) |
| 用戶端 NuGet 套件 | [Microsoft.AspNet.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[Microsoft.AspNet.SignalR.JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| 用戶端 npm 套件 | [signalr](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| JAVA 用戶端 | [GitHub 存放庫](https://github.com/SignalR/java-client)不再  | Maven package [com. signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr) |
| 伺服器應用程式類型 | ASP.NET （System.web）或 OWIN 自我裝載 | ASP.NET Core |
| 支援的伺服器平臺 | .NET Framework 4.5 或更新版本 | .NET Framework 4.6.1 或更新版本<br>.NET Core 2.1 或更新版本 |

## <a name="feature-differences"></a>功能差異

### <a name="automatic-reconnects"></a>自動重新連接

ASP.NET Core SignalR 中不支援自動重新連接。 如果用戶端已中斷連線，則使用者必須明確地啟動新的連線（如果他們想要重新連接）。 在 ASP.NET SignalR 中，如果中斷連接，SignalR 會嘗試重新連線到伺服器。

### <a name="protocol-support"></a>通訊協定支援

ASP.NET Core SignalR 支援 JSON，以及以[MessagePack](xref:signalr/messagepackhubprotocol)為基礎的新二進位通訊協定。 此外，也可以建立自訂通訊協定。

### <a name="transports"></a>傳輸

ASP.NET Core SignalR 中不支援永久的框架傳輸。

## <a name="differences-on-the-server"></a>伺服器上的差異

ASP.NET Core 的 SignalR 伺服器端程式庫包含在 Razor 和 MVC 專案的**ASP.NET Core Web 應用程式**範本中的[中繼套件](xref:fundamentals/metapackage-app)套件中。

ASP.NET Core SignalR 是 ASP.NET Core 中介軟體，因此必須藉由呼叫中`Startup.ConfigureServices`的[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)來設定。

```csharp
services.AddSignalR()
```

::: moniker range=">= aspnetcore-3.0"

若要設定路由，請在`Startup.Configure`方法中的[UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints)方法呼叫內，將路由對應至中樞。


```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

若要設定路由，請在`Startup.Configure`方法中的[UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr)方法呼叫內，將路由對應至中樞。

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

### <a name="sticky-sessions"></a>粘滯話

ASP.NET SignalR 的向外延展模型可讓用戶端重新連線，並將訊息傳送至伺服器陣列中的任何伺服器。 在 ASP.NET Core SignalR 中，用戶端必須在連接期間與相同的伺服器互動。 針對使用 Redis 的向外延展，這表示需要有粘滯會話。 針對使用[Azure SignalR Service](/azure/azure-signalr/)的向外延展，由於服務會處理用戶端的連線，因此不需要粘滯話。

### <a name="single-hub-per-connection"></a>每個連接的單一中樞

在 ASP.NET Core SignalR 中，已簡化連接模型。 直接連接到單一中樞，而不是使用單一連線來共用多個中樞的存取權。

### <a name="streaming"></a>資料流

ASP.NET Core SignalR 現在支援從中樞將[資料串流](xref:signalr/streaming)至用戶端。

### <a name="state"></a>State

在用戶端與中樞之間傳遞任意狀態的能力（通常稱為 HubState）已移除，並支援進度訊息。 目前沒有任何對應的中樞 proxy。

### <a name="persistentconnection-removal"></a>PersistentConnection 移除

在 ASP.NET Core SignalR 中，已移除[PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118))類別。

### <a name="globalhost"></a>GlobalHost

ASP.NET Core 在架構內建相依性插入（DI）。 服務可以使用 DI 來存取[HubCoNtext](xref:signalr/hubcontext)。 在 ASP.NET SignalR 中用來取得的`HubContext` 物件不存在於ASP.NETCoreSignalR中。`GlobalHost`

### <a name="hubpipeline"></a>HubPipeline

ASP.NET Core SignalR 不支援`HubPipeline`模組。

## <a name="differences-on-the-client"></a>用戶端上的差異

### <a name="typescript"></a>TypeScript

ASP.NET Core SignalR 用戶端是以[TypeScript](https://www.typescriptlang.org/)撰寫。 使用[javascript 用戶端](xref:signalr/javascript-client)時，您可以使用 JAVAscript 或 TypeScript 來撰寫。

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>JavaScript 用戶端裝載于[npm](https://www.npmjs.com/)

在先前的版本中，JavaScript 用戶端是透過 Visual Studio 中的 NuGet 套件取得。 針對核心版本， [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm 套件包含 JavaScript 程式庫。 此套件不包含在**ASP.NET Core Web 應用程式**範本中。 使用 npm 取得並安裝`@aspnet/signalr` npm 套件。

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

已移除 jQuery 的相依性，不過專案仍然可以使用 jQuery。

### <a name="internet-explorer-support"></a>Internet Explorer 支援

ASP.NET Core SignalR 需要 Microsoft Internet Explorer 11 或更新版本（ASP.NET SignalR 支援 Microsoft Internet Explorer 8 和更新版本）。

### <a name="javascript-client-method-syntax"></a>JavaScript 用戶端方法語法

JavaScript 語法已從舊版 SignalR 變更。 請不要使用`$connection`物件，而是使用[HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API 來建立連接。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

使用[on](/javascript/api/@aspnet/signalr/HubConnection#on)方法來指定中樞可以呼叫的用戶端方法。

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

建立用戶端方法之後，請啟動中樞連接。 建立[catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch)方法的鏈，以記錄或處理錯誤。

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>中樞 proxy

不會再自動產生中樞 proxy。 相反地，方法名稱會以字串形式傳遞至[invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API。

### <a name="net-and-other-clients"></a>.NET 和其他用戶端

`Microsoft.AspNetCore.SignalR.Client` NuGet 套件包含適用于 ASP.NET Core SignalR 的 .net 用戶端程式庫。

使用[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)來建立和建立與中樞的連接實例。

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a>向外延展差異

ASP.NET SignalR 支援 SQL Server 和 Redis。 ASP.NET Core SignalR 支援 Azure SignalR Service 和 Redis。

### <a name="aspnet"></a>ASP.NET

* [具有 Azure 服務匯流排的 SignalR 向外延展](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [具有 Redis 的 SignalR 向外延展](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [具有 SQL Server 的 SignalR 向外延展](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a>ASP.NET Core

* [Azure SignalR Service](/azure/azure-signalr/)
* [Redis 背板](xref:signalr/redis-backplane)

## <a name="additional-resources"></a>其他資源

* [中樞](xref:signalr/hubs)
* [JavaScript 用戶端](xref:signalr/javascript-client)
* [.NET 用戶端](xref:signalr/dotnet-client)
* [支援的平台](xref:signalr/supported-platforms)
