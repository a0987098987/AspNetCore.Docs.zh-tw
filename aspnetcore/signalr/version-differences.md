---
title: SignalR 和 ASP.NET Core SignalR 之間的差異
author: rachelappel
description: SignalR 和 ASP.NET Core SignalR 之間的差異
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: fd314d93333bd159aef4bb4863be50c646809cf0
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090059"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a>SignalR 和 ASP.NET Core SignalR 之間的差異

ASP.NET Core SignalR 與不相容用戶端或適用於 ASP.NET SignalR 的伺服器。 這篇文章說明已移除或變更 ASP.NET Core SignalR 中的功能。

## <a name="feature-differences"></a>功能差異

### <a name="automatic-reconnects"></a>自動重新

不再支援自動重新。 先前，SignalR 會嘗試重新連線到伺服器，如果連線已中斷。 現在，如果用戶端中斷連線的使用者必須明確啟動新的連接，如果他們想要重新連線。

### <a name="protocol-support"></a>通訊協定支援

ASP.NET Core SignalR 支援 JSON，以及新的二進位通訊協定，以根據[MessagePack](xref:signalr/messagepackhubprotocol)。 此外，您可以建立自訂通訊協定。

## <a name="differences-on-the-server"></a>在伺服器上的差異

SignalR 的伺服器端程式庫隨附的`Microsoft.AspNetCore.App`所屬的封裝**ASP.NET Core Web 應用程式**Razor 和 MVC 專案範本。

SignalR 是 ASP.NET Core 中介軟體，因此它必須藉由呼叫設定`AddSignalR`中`Startup.ConfigureServices`。

```csharp
services.AddSignalR();
```

若要設定路由，請將路由對應至中樞內`UseSignalR`方法呼叫中`Startup.Configure`方法。

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a>現在必須要有的自黏工作階段

如何向外延展使用過舊版 SignalR 的因為用戶端無法重新連線，並將訊息傳送至伺服器陣列中的任何伺服器。 由於在向外延展模型，以及不支援重新變更，這已不再支援。 用戶端連線到伺服器之後必須立即互動同一部伺服器用於連接持續時間。

### <a name="single-hub-per-connection"></a>針對每個連接的單一中樞

在 ASP.NET Core SignalR，已經過簡化的連接模式。 連接的直接單一中樞，而不是單一連線正在使用共用存取多個中樞。

### <a name="streaming"></a>資料流

SignalR 現在支援[串流資料](xref:signalr/streaming)從用戶端的中樞。

### <a name="state"></a>狀況

用戶端與中樞 （通常稱為 HubState） 之間傳遞任意的狀態的能力被移除了，以及支援進度訊息。 目前的中樞 proxy 沒有對應項目。

## <a name="differences-on-the-client"></a>在用戶端上的差異

### <a name="typescript"></a>TypeScript

SignalR 的 ASP.NET Core 版本撰寫的[TypeScript](https://www.typescriptlang.org/)。 使用時，您可以撰寫的 JavaScript 或 TypeScript [JavaScript 用戶端](xref:signalr/javascript-client)。

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>JavaScript 用戶端會裝載於[npm](https://www.npmjs.com/)

在舊版中，JavaScript 用戶端已取得透過 Visual Studio 中的 NuGet 封裝。 Core 版本中， [ @aspnet/signalr npm 封裝](https://www.npmjs.com/package/@aspnet/signalr)包含 JavaScript 程式庫。 此套件不包含在**ASP.NET Core Web 應用程式**範本。 用於取得並安裝 npm `@aspnet/signalr` npm 封裝。

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

已移除對 jQuery 的相依性，但專案仍然可以使用 jQuery。

### <a name="javascript-client-method-syntax"></a>JavaScript 用戶端方法語法

JavaScript 語法已從舊版 SignalR 的變更。 而不是使用`$connection`物件，請建立連線，使用`HubConnectionBuilder`應用程式開發介面。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

使用`connection.on`指定中樞可以呼叫的用戶端方法。

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

建立用戶端方法之後, 啟動中樞連線。 鏈結`catch`方法，以記錄或處理錯誤。

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>中樞 proxy

不會再自動產生中樞 proxy。 相反地，傳送到的方法名稱`invoke`API 為字串。

### <a name="net-and-other-clients"></a>.NET 和其他用戶端

`Microsoft.AspNetCore.SignalR.Client` NuGet 套件包含適用於 ASP.NET Core SignalR 的.NET 用戶端程式庫。

使用`HubConnectionBuilder`以建立並建置連線至中樞的執行個體。

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a>其他資源

* [中樞](xref:signalr/hubs)
* [JavaScript 用戶端](xref:signalr/javascript-client)
* [.NET 用戶端](xref:signalr/dotnet-client)
* [支援的平台](xref:signalr/supported-platforms)
