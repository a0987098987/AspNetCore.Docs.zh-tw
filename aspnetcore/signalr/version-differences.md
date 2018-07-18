---
title: SignalR 和 ASP.NET Core SignalR 之間的差異
author: tdykstra
description: SignalR 和 ASP.NET Core SignalR 之間的差異
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: 6ed7e2e1ecadef08d71c4d7a7c3469738d07bcda
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095004"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a>SignalR 和 ASP.NET Core SignalR 之間的差異

ASP.NET Core SignalR 與不相容用戶端或為 ASP.NET SignalR 的伺服器。 本文詳細說明已經被移除，或在 ASP.NET Core SignalR 中變更的功能。

## <a name="feature-differences"></a>功能差異

### <a name="automatic-reconnects"></a>自動重新連線

不再支援自動重新連線。 先前，SignalR 會嘗試重新連線到伺服器，如果連線已中斷。 現在，如果用戶端中斷連線的使用者必須明確啟動新的連接，如果他們想要重新連線。

### <a name="protocol-support"></a>通訊協定支援

ASP.NET Core SignalR 支援 JSON，以及新的二進位通訊協定，以根據[MessagePack](xref:signalr/messagepackhubprotocol)。 此外，您可以建立自訂通訊協定。

## <a name="differences-on-the-server"></a>在伺服器上的差異

SignalR 的伺服器端程式庫都會納入`Microsoft.AspNetCore.App`封裝的一部分**ASP.NET Core Web 應用程式**Razor，MVC 專案範本。

SignalR 是 ASP.NET Core 中介軟體，因此必須由呼叫`AddSignalR`在`Startup.ConfigureServices`。

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

### <a name="sticky-sessions-now-required"></a>現在需要黏性工作階段

如何向外延展中使用過舊版 SignalR 的因為用戶端無法重新連線，並將訊息傳送至伺服器陣列中的任何伺服器。 由於變更向外延展模型，以及不支援重新連線，因此這是不受支援。 現在，一旦用戶端連接到伺服器它需要使用相同的伺服器在連線期間的互動方式。

### <a name="single-hub-per-connection"></a>每個連線單一中樞

在 ASP.NET Core SignalR 連線模型已簡化。 連線會對直接單一中樞中，而不是正在使用共用存取權的多個中樞單一連接。

### <a name="streaming"></a>資料流

SignalR 現在支援[串流資料](xref:signalr/streaming)從用戶端中樞。

### <a name="state"></a>狀況

能夠將任意的狀態傳遞用戶端與中樞 （通常稱為 HubState） 之間已移除，以及支援進度訊息。 目前沒有任何對應的中樞 proxy。

## <a name="differences-on-the-client"></a>在用戶端上的差異

### <a name="typescript"></a>TypeScript

SignalR 的 ASP.NET Core 版本以[TypeScript](https://www.typescriptlang.org/)。 使用時，您可以撰寫以 JavaScript 或 TypeScript [JavaScript 用戶端](xref:signalr/javascript-client)。

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>JavaScript 用戶端裝載於[npm](https://www.npmjs.com/)

在舊版中，JavaScript 用戶端已取得透過 Visual Studio 中的 NuGet 套件。 如需核心版本中， [ @aspnet/signalr npm 套件](https://www.npmjs.com/package/@aspnet/signalr)包含 JavaScript 程式庫。 此套件不會包含於**ASP.NET Core Web 應用程式**範本。 使用 npm 來取得並安裝`@aspnet/signalr`npm 套件。

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

已移除對 jQuery 的相依性，但專案仍然可以使用 jQuery。

### <a name="javascript-client-method-syntax"></a>JavaScript 用戶端方法語法

JavaScript 語法已從舊版 SignalR 的變更。 而不是使用`$connection`物件，請建立連線，使用`HubConnectionBuilder`API。

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

建立用戶端方法之後, 啟動中樞連線。 鏈結`catch`記錄或處理錯誤的方法。

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>中樞 proxy

中樞 proxy 不會再自動產生。 相反地，傳送到的方法名稱`invoke`API 做為字串。

### <a name="net-and-other-clients"></a>.NET 和其他用戶端

`Microsoft.AspNetCore.SignalR.Client` NuGet 套件包含.NET 用戶端程式庫的 ASP.NET Core SignalR。

使用`HubConnectionBuilder`來建立及建置連接到中樞執行個體。

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
