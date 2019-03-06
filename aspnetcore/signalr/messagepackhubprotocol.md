---
title: 使用 ASP.NET Core SignalR MessagePack 中樞通訊協定
author: bradygaster
description: 加入 ASP.NET Core SignalR MessagePack 中樞通訊協定。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/27/2019
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 7742f6f8bb53fb3c299ff98ae52a0da519ff396c
ms.sourcegitcommit: 6ddd8a7675c1c1d997c8ab2d4498538e44954cac
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2019
ms.locfileid: "57400667"
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

> [!NOTE]
> 預設會啟用 JSON 支援的用戶端。 用戶端只能支援單一通訊協定。 新增 MessagePack 支援取代任何先前設定的通訊協定。

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

會提供 JavaScript 用戶端的 MessagePack 支援`@aspnet/signalr-protocol-msgpack`npm 套件。

```console
npm install @aspnet/signalr-protocol-msgpack
```

安裝 npm 套件之後，此模組可以直接透過 JavaScript 模組載入器或藉由參考匯入至瀏覽器 *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* 檔案。 在瀏覽器，`msgpack5`也必須參考程式庫。 使用`<script>`標記，以建立參考。 程式庫，請參閱*node_modules\msgpack5\dist\msgpack5.js*。

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

## <a name="messagepack-quirks"></a>MessagePack quirks

有幾個問題時要注意的使用 MessagePack 中樞通訊協定。

### <a name="messagepack-is-case-sensitive"></a>MessagePack 會區分大小寫

MessagePack 通訊協定會區分大小寫。 例如，請考慮下列C#類別：

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

當從 JavaScript 用戶端傳送，您必須使用`PascalCased`屬性的名稱，因為大小寫必須符合C#完全類別。 例如: 

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

使用`camelCased`名稱不會正確地繫結至C#類別。 使用來解決這個`Key`屬性來指定 MessagePack 屬性不同的名稱。 如需詳細資訊，請參閱 < [MessagePack CSharp 文件](https://github.com/neuecc/MessagePack-CSharp#object-serialization)。

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a>當序列化/還原序列化時，不會保留 DateTime.Kind

MessagePack 通訊協定不提供編碼方式`Kind`的值`DateTime`。 如此一來，還原序列化時的日期，MessagePack 中樞通訊協定會假設連入的日期是以 UTC 格式。 如果您正在使用`DateTime`當地時間的值，我們建議您將轉換為 UTC，然後才傳送。 它們從 UTC 轉換為當地時間時接收它們。

如需有關這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/SignalR # 2632年](https://github.com/aspnet/SignalR/issues/2632)。

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a>MessagePack 在 JavaScript 中不支援 DateTime.MinValue

[Msgpack5](https://github.com/mcollina/msgpack5) SignalR JavaScript 用戶端所使用的程式庫不支援`timestamp96`MessagePack 中的型別。 此類型用來編碼 （無論是使用極早期在過去或未來很遠） 非常大的日期值。 值`DateTime.MinValue`已`January 1, 0001`這必須在編碼`timestamp96`值。 因為此，傳送`DateTime.MinValue`javascript 用戶端不支援。 當`DateTime.MinValue`收到 JavaScript 用戶端，會擲回下列錯誤：

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

通常`DateTime.MinValue`用來編碼的 「 遺漏 」 或`null`值。 如果您要編碼 MessagePack 中的值，請使用可為 null`DateTime`值 (`DateTime?`) 或編碼個別`bool`值，指出日期是否存在。

如需有關這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/SignalR # 2228年](https://github.com/aspnet/SignalR/issues/2228)。

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a>MessagePack 「 預先-「 編譯環境中的支援

[MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp) .NET 用戶端和伺服器所使用的程式庫會使用程式碼產生最佳化序列化。 如此一來，它不支援使用 「 預先-「 編譯 （例如 Xamarin iOS 或 Unity） 的環境上的預設值。 可以在這些環境中使用 MessagePack，藉由 「 預先產生"序列化/還原序列化程式程式碼。 如需詳細資訊，請參閱 < [MessagePack CSharp 文件](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports)。 一旦您預先產生序列化程式，您可以將它們註冊使用傳遞至設定委派`AddMessagePackProtocol`:

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers = new List<MessagePack.IFormatterResolver>()
        {
            MessagePack.Resolvers.GeneratedResolver.Instance,
            MessagePack.Resolvers.StandardResolver.Instance
        };
    });
```

### <a name="type-checks-are-more-strict-in-messagepack"></a>型別檢查為 MessagePack 中更嚴格

JSON 中樞通訊協定會在還原序列化期間執行型別轉換。 例如，如果連入物件的屬性值是數字 (`{ foo: 42 }`)，但在.NET 類別上的屬性的類型是`string`，值會轉換。 不過，MessagePack 不會執行這項轉換，而且將會擲回的例外狀況，您所見，在伺服器端記錄檔 （而在主控台中）：

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

如需有關這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/SignalR # 2937年](https://github.com/aspnet/SignalR/issues/2937)。

## <a name="related-resources"></a>相關資源

* [開始使用](xref:tutorials/signalr)
* [.NET 用戶端](xref:signalr/dotnet-client)
* [JavaScript 用戶端](xref:signalr/javascript-client)
