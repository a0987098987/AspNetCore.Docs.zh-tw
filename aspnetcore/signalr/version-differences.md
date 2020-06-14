---
title: SignalR和 ASP.NET Core 之間的差異SignalR
author: bradygaster
description: SignalR和 ASP.NET Core 之間的差異SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.date: 11/21/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/version-differences
ms.openlocfilehash: b5899f816dc5a5f8ff4c3f05c8e2c54ded5fc47b
ms.sourcegitcommit: a423e8fcde4b6181a3073ed646a603ba20bfa5f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/13/2020
ms.locfileid: "84756037"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a>ASP.NET SignalR 和 ASP.NET Core 之間的差異SignalR

ASP.NET Core SignalR 與用戶端或伺服器不相容，因此無法進行 ASP.NET SignalR 。 本文詳述已在 ASP.NET Core 中移除或變更的功能 SignalR 。

## <a name="how-to-identify-the-signalr-version"></a>如何識別 SignalR 版本

::: moniker range=">= aspnetcore-3.0"

|                      | ASP.NETSignalR | ASP.NET CoreSignalR |
| -------------------- | --------------- | -------------------- |
| 伺服器 NuGet 封裝 | [Microsoft AspNet。SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | 無。 包含在[AspNetCore](xref:fundamentals/metapackage-app)共用架構中。 |
| 用戶端 NuGet 套件 | [Microsoft AspNet. SignalR 。台](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[Microsoft AspNet. SignalR 。NODE.JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [AspNetCore SignalR 。台](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| JavaScript 用戶端 npm 套件 | [signalr](https://www.npmjs.com/package/signalr) | [`@microsoft/signalr`](https://www.npmjs.com/package/@microsoft/signalr) |
| Java 用戶端 | [GitHub 存放庫](https://github.com/SignalR/java-client)（已淘汰）  | Maven package [com. signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr) |
| 伺服器應用程式類型 | ASP.NET （System.web）或 OWIN 自我裝載 | ASP.NET Core |
| 支援的伺服器平臺 | .NET Framework 4.5 或更新版本 | .NET Core 3.0 或更新版本 |

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

|                      | ASP.NETSignalR | ASP.NET CoreSignalR |
| -------------------- | --------------- | -------------------- |
| 伺服器 NuGet 封裝 | [Microsoft AspNet。SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | [AspNetCore 應用程式](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)（.net Core）<br>[AspNetCore。SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework) |
| 用戶端 NuGet 套件 | [Microsoft AspNet. SignalR 。台](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[Microsoft AspNet. SignalR 。NODE.JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [AspNetCore SignalR 。台](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| JavaScript 用戶端 npm 套件 | [signalr](https://www.npmjs.com/package/signalr) | [`@aspnet/signalr`](https://www.npmjs.com/package/@aspnet/signalr) |
| Java 用戶端 | [GitHub 存放庫](https://github.com/SignalR/java-client)（已淘汰）  | Maven package [com. signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr) |
| 伺服器應用程式類型 | ASP.NET （System.web）或 OWIN 自我裝載 | ASP.NET Core |
| 支援的伺服器平臺 | .NET Framework 4.5 或更新版本 | .NET Framework 4.6.1 或更新版本<br>.NET Core 2.1 或更新版本 |

::: moniker-end

## <a name="feature-differences"></a>功能差異

### <a name="automatic-reconnects"></a>自動重新連接

::: moniker range=">= aspnetcore-3.0"

在 ASP.NET 中 SignalR ：

* 根據預設， SignalR 如果中斷連接，會嘗試重新連接到伺服器。 

在 ASP.NET Core SignalR ：

* 自動重新連接會同時使用[.net 客戶](xref:signalr/dotnet-client#automatically-reconnect)端和[JavaScript 用戶端](xref:signalr/javascript-client#automatically-reconnect)來選擇：

```csharp
HubConnection connection = new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chathub"))
    .WithAutomaticReconnect()
    .Build();
```

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub")
    .withAutomaticReconnect()
    .build();
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

在 ASP.NET Core 3.0 之前， SignalR 不支援自動重新連接。 如果用戶端已中斷連線，使用者必須明確地啟動新的連線以重新連接。 在 ASP.NET 中 SignalR ， SignalR 如果中斷連接，會嘗試重新連接到伺服器。

::: moniker-end

### <a name="protocol-support"></a>通訊協定支援

ASP.NET Core SignalR 支援 JSON，以及以[MessagePack](xref:signalr/messagepackhubprotocol)為基礎的新二進位通訊協定。 此外，也可以建立自訂通訊協定。

### <a name="transports"></a>傳輸

ASP.NET Core 中不支援永久的框架傳輸 SignalR 。

## <a name="differences-on-the-server"></a>伺服器上的差異

ASP.NET Core 的 SignalR 伺服器端程式庫都包含在[AspNetCore](xref:fundamentals/metapackage-app)中，用於和 MVC 專案的**ASP.NET Core Web 應用程式**範本中。 Razor

ASP.NET Core SignalR 是 ASP.NET Core 中介軟體。 您必須在中呼叫來設定它 <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddSignalR%2A> `Startup.ConfigureServices` 。

```csharp
services.AddSignalR()
```

::: moniker range=">= aspnetcore-3.0"

若要設定路由，請 <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints%2A> 在方法中的方法呼叫內，將路由對應至中樞 `Startup.Configure` 。

```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

若要設定路由，請 <xref:Microsoft.AspNetCore.Builder.SignalRAppBuilderExtensions.UseSignalR%2A> 在方法中的方法呼叫內，將路由對應至中樞 `Startup.Configure` 。

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

### <a name="sticky-sessions"></a>粘滯話

ASP.NET 的向外延展模型 SignalR 可讓用戶端重新連線，並將訊息傳送至伺服器陣列中的任何伺服器。 在 ASP.NET Core 中 SignalR ，用戶端必須在連接期間與相同的伺服器互動。 針對使用 Redis 的向外延展，這表示需要有粘滯會話。 針對使用[Azure SignalR 服務](/azure/azure-signalr/)的向外延展，由於服務會處理與用戶端的連線，因此不需要「粘滯話」。

### <a name="single-hub-per-connection"></a>每個連接的單一中樞

在 ASP.NET Core 中 SignalR ，已簡化連接模型。 直接連接到單一中樞，而不是使用單一連線來共用多個中樞的存取權。

### <a name="streaming"></a>串流

ASP.NET Core SignalR 現在支援從中樞將[資料串流](xref:signalr/streaming)至用戶端。

### <a name="state"></a>State

在用戶端與中樞之間傳遞任意狀態（通常稱為）的功能已 `HubState` 移除，並支援進度訊息。 目前沒有任何對應的中樞 proxy。

### <a name="persistentconnection-removal"></a>PersistentConnection 移除

在 ASP.NET Core 中 SignalR ，已移除[PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118))類別。

### <a name="globalhost"></a>GlobalHost

ASP.NET Core 在架構內建相依性插入（DI）。 服務可以使用 DI 來存取[HubCoNtext](xref:signalr/hubcontext)。 `GlobalHost`在 ASP.NET 中用 SignalR 來取得的物件 `HubContext` 不存在於 ASP.NET Core 中 SignalR 。

### <a name="hubpipeline"></a>HubPipeline

ASP.NET Core SignalR 不支援 `HubPipeline` 模組。

## <a name="differences-on-the-client"></a>用戶端上的差異

### <a name="typescript"></a>TypeScript

ASP.NET Core 的 SignalR 用戶端是以[TypeScript](https://www.typescriptlang.org/)撰寫的。 使用[javascript 用戶端](xref:signalr/javascript-client)時，您可以使用 JAVAscript 或 TypeScript 來撰寫。

### <a name="the-javascript-client-is-hosted-at-npm"></a>JavaScript 用戶端裝載于 npm

::: moniker range=">= aspnetcore-3.0"

在 ASP.NET 版本中，JavaScript 用戶端是透過 Visual Studio 中的 NuGet 套件取得。 在 ASP.NET Core 版本中， [`@microsoft/signalr`](https://www.npmjs.com/package/@microsoft/signalr) npm 套件包含 JavaScript 程式庫。 此套件不包含在**ASP.NET Core Web 應用程式**範本中。 使用 npm 取得並安裝 `@microsoft/signalr` npm 套件。

```console
npm init -y
npm install @microsoft/signalr
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

在 ASP.NET 版本中，JavaScript 用戶端是透過 Visual Studio 中的 NuGet 套件取得。 在 ASP.NET Core 版本中， [`@aspnet/signalr`](https://www.npmjs.com/package/@aspnet/signalr) npm 套件包含 JavaScript 程式庫。 此套件不包含在**ASP.NET Core Web 應用程式**範本中。 使用 npm 取得並安裝 `@aspnet/signalr` npm 套件。

```console
npm init -y
npm install @aspnet/signalr
```

::: moniker-end

### <a name="jquery"></a>jQuery

已移除 jQuery 的相依性，不過專案仍然可以使用 jQuery。

### <a name="internet-explorer-support"></a>Internet Explorer 支援

ASP.NET Core SignalR 需要 Microsoft Internet explorer 11 或更新版本（ASP.NET SignalR 支援 Microsoft internet explorer 8 和更新版本）。

### <a name="javascript-client-method-syntax"></a>JavaScript 用戶端方法語法

::: moniker range=">= aspnetcore-3.0"

JavaScript 語法已從的 ASP.NET 版本中變更 SignalR 。 請不要使用 `$connection` 物件，而是使用[HubConnectionBuilder](/javascript/api/@aspnet/signalr/hubconnectionbuilder) API 來建立連接。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

使用[on](/javascript/api/@microsoft/signalr/HubConnection#on)方法來指定中樞可以呼叫的用戶端方法。

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

JavaScript 語法已從的 ASP.NET 版本中變更 SignalR 。 請不要使用 `$connection` 物件，而是使用[HubConnectionBuilder](/javascript/api/@microsoft/signalr/hubconnectionbuilder) API 來建立連接。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

使用[on](/javascript/api/@aspnet/signalr/HubConnection#on)方法來指定中樞可以呼叫的用戶端方法。

::: moniker-end

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = `${user} says ${msg}`;
    console.log(encodedMsg);
});
```

建立用戶端方法之後，請啟動中樞連接。 建立[catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch)方法的鏈，以記錄或處理錯誤。

```javascript
connection.start().catch(err => console.error(err));
```

### <a name="hub-proxies"></a>中樞 proxy

::: moniker range=">= aspnetcore-3.0"

不會再自動產生中樞 proxy。 相反地，方法名稱會以字串形式傳遞至[invoke](/javascript/api/@microsoft/signalr/hubconnection#invoke) API。

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

不會再自動產生中樞 proxy。 相反地，方法名稱會以字串形式傳遞至[invoke](/javascript/api/@aspnet/signalr/hubconnection#invoke) API。

::: moniker-end

### <a name="net-and-other-clients"></a>.NET 和其他用戶端

[AspNetCore。 SignalR用戶端](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client)NuGet 套件包含適用于 ASP.NET Core 的 .net 用戶端程式庫 SignalR 。

使用 <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder> 來建立並建立與中樞連線的實例。

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a>向外延展差異

ASP.NET SignalR 支援 SQL Server 和 Redis。 ASP.NET Core SignalR 支援 Azure SignalR 服務和 Redis。

### <a name="aspnet"></a>ASP.NET

* [SignalR具有 Azure 服務匯流排的向外延展](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [SignalR具有 Redis 的向外延展](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [SignalR具有 SQL Server 的向外延展](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a>ASP.NET Core

* [Azure SignalR 服務](/azure/azure-signalr/)
* [Redis 背板](xref:signalr/redis-backplane)

## <a name="additional-resources"></a>其他資源

* [中樞](xref:signalr/hubs)
* [JavaScript 用戶端](xref:signalr/javascript-client)
* [.NET 用戶端](xref:signalr/dotnet-client)
* [支援的平台](xref:signalr/supported-platforms)
