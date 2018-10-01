---
title: 使用 ASP.NET Core SignalR MessagePack 中樞通訊協定
author: tdykstra
description: 加入 ASP.NET Core SignalR MessagePack 中樞通訊協定。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 78b708c50ce7a8101c9eaa558171540e61c0d7f0
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2018
ms.locfileid: "39094991"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a>使用 ASP.NET Core SignalR MessagePack 中樞通訊協定

由[brennan Conroy](https://github.com/BrennanConroy)提供

本文假設讀者已熟悉所涵蓋的主題[開始](xref:tutorials/signalr)。

## <a name="what-is-messagepack"></a>MessagePack 是什麼？

[MessagePack](https://msgpack.org/index.html)是快速且精簡的二進位序列化格式。 效能和頻寬是需要考量因為它會建立相較於較小的訊息時很有用[JSON](https://www.json.org/)。 因為它是一種二進位格式時，如果除非位元組都會通過 MessagePack 剖析器，查看網路追蹤和記錄檔訊息就無法讀取。 SignalR 的 MessagePack 格式中，內建支援，並提供用戶端和伺服器使用的 Api。

## <a name="configure-messagepack-on-the-server"></a>在伺服器上設定 MessagePack

若要啟用 MessagePack 中樞通訊協定，在伺服器上，安裝`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`應用程式中的封裝。 在 Startup.cs 檔案中新增`AddMessagePackProtocol`至`AddSignalR`啟用 MessagePack 支援在伺服器上的呼叫。

> [!NOTE]
> 預設會啟用 JSON。 新增 MessagePack 啟用 JSON 和 MessagePack 用戶端的支援。

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

若要自訂 MessagePack 會列印文件的格式，請您的資料，`AddMessagePackProtocol`會接受委派的設定選項。 在該委派，`FormatterResolvers`屬性可用來設定 MessagePack 序列化選項。 如需有關如何解析程式的運作方式的詳細資訊，請瀏覽 MessagePack library [MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp)。 在您想要以定義他們應該如何處理序列化的物件上可用的屬性。

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers = new List<MessagePack.IFormatterResolver>()
        {
            MessagePack.Resolvers.StandardResolver.Instance
        };
    });
```

## <a name="configure-messagepack-on-the-client"></a>用戶端上設定 MessagePack

### <a name="net-client"></a>.NET 用戶端

若要啟用 MessagePack.NET 用戶端中的，安裝`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`封裝並呼叫`AddMessagePackProtocol`上`HubConnectionBuilder`。

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> 這`AddMessagePackProtocol`呼叫會接受委派的設定選項，就像伺服器。

### <a name="javascript-client"></a>JavaScript 用戶端

會提供 JavaScript 用戶端的 MessagePack 支援`@aspnet/signalr-protocol-msgpack`NPM 套件。

```console
npm install @aspnet/signalr-protocol-msgpack
```

安裝 npm 套件之後，此模組可以直接透過 JavaScript 模組載入器或藉由參考匯入至瀏覽器 *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* 檔案。 在瀏覽器`msgpack5`也必須參考程式庫。 使用`<script>`標記，以建立參考。 程式庫，請參閱*node_modules\msgpack5\dist\msgpack5.js*。

> [!NOTE]
> 當使用`<script>`元素的順序很重要。 如果*signalr-protocol-msgpack.js*參考之前*msgpack5.js*，嘗試使用 MessagePack 連線時，就會發生錯誤。 *signalr.js*之前，也需要*signalr-protocol-msgpack.js*。

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

新增`.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())`至`HubConnectionBuilder`會設定用戶端連接到伺服器時，使用 MessagePack 通訊協定。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> 在此階段中，有 MessagePack 通訊協定在 JavaScript 用戶端上的沒有組態選項。

## <a name="related-resources"></a>相關資源

* [開始使用](xref:tutorials/signalr)
* [.NET 用戶端](xref:signalr/dotnet-client)
* [JavaScript 用戶端](xref:signalr/javascript-client)
