---
title: 在 SignalR 中使用適用于 ASP.NET Core 的 MessagePack Hub 通訊協定
author: bradygaster
description: 將 MessagePack 中樞通訊協定新增至 ASP.NET Core SignalR。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 1b01357233a9b95a5da052d92e30232c94e78a78
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727230"
---
# <a name="use-messagepack-hub-protocol-in-opno-locsignalr-for-aspnet-core"></a>在 SignalR 中使用適用于 ASP.NET Core 的 MessagePack Hub 通訊協定

依[Brennan Conroy](https://github.com/BrennanConroy)

本文假設讀者已熟悉[開始](xref:tutorials/signalr)使用中所涵蓋的主題。

## <a name="what-is-messagepack"></a>什麼是 MessagePack？

[MessagePack](https://msgpack.org/index.html)是一種快速且精簡的二進位序列化格式。 這在效能和頻寬有所顧慮時很有用，因為它會建立比[JSON](https://www.json.org/)更小的訊息。 因為這是一種二進位格式，所以在查看網路追蹤和記錄檔時，除非透過 MessagePack 剖析器傳遞位元組，否則無法讀取訊息。 SignalR 具有 MessagePack 格式的內建支援，並提供 Api 讓用戶端和伺服器使用。

## <a name="configure-messagepack-on-the-server"></a>在伺服器上設定 MessagePack

若要在伺服器上啟用 MessagePack Hub 通訊協定，請在您的應用程式中安裝 `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` 套件。 在 Startup.cs 檔案中，將 `AddMessagePackProtocol` 新增至 `AddSignalR` 呼叫，以在伺服器上啟用 MessagePack 支援。

> [!NOTE]
> 預設會啟用 JSON。 新增 MessagePack 可同時支援 JSON 和 MessagePack 用戶端。

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

若要自訂 MessagePack 將資料格式化的方式，請 `AddMessagePackProtocol` 取得設定選項的委派。 在該委派中，`FormatterResolvers` 屬性可以用來設定 MessagePack 序列化選項。 如需解決器如何工作的詳細資訊，請造訪[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp)上的 MessagePack 程式庫。 屬性可以在您要序列化的物件上使用，以定義它們的處理方式。

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

## <a name="configure-messagepack-on-the-client"></a>設定用戶端上的 MessagePack

> [!NOTE]
> 根據預設，支援的用戶端會啟用 JSON。 用戶端只能支援單一通訊協定。 新增 MessagePack 支援將會取代任何先前設定的通訊協定。

### <a name="net-client"></a>.NET 用戶端

若要在 .NET 用戶端中啟用 MessagePack，請安裝 `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` 封裝，並在 `HubConnectionBuilder`上呼叫 `AddMessagePackProtocol`。

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> 這個 `AddMessagePackProtocol` 呼叫會採用委派來設定選項，就像伺服器一樣。

### <a name="javascript-client"></a>JavaScript 用戶端

::: moniker range=">= aspnetcore-3.0"

`@microsoft/signalr-protocol-msgpack` npm 套件會提供 JavaScript 用戶端的 MessagePack 支援。 在命令 shell 中執行下列命令，以安裝套件：

```console
npm install @microsoft/signalr-protocol-msgpack
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

`@aspnet/signalr-protocol-msgpack` npm 套件會提供 JavaScript 用戶端的 MessagePack 支援。 在命令 shell 中執行下列命令，以安裝套件：

```console
npm install @aspnet/signalr-protocol-msgpack
```

::: moniker-end

安裝 npm 套件之後，您可以直接透過 JavaScript 模組載入器使用模組，或藉由參考下列檔案，將其匯入至瀏覽器：

::: moniker range=">= aspnetcore-3.0"

*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* 

::: moniker-end

::: moniker range="< aspnetcore-3.0"

*node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* 

::: moniker-end

在瀏覽器中，也必須參考 `msgpack5` 程式庫。 使用 `<script>` 標記來建立參考。 您可以在*node_modules \msgpack5\dist\msgpack5.js*找到此程式庫。

> [!NOTE]
> 使用 `<script>` 元素時，順序很重要。 如果在*msgpack5*之前參考*signalr-protocol-msgpack* ，則嘗試使用 MessagePack 連接時，會發生錯誤。 *signalr-protocol-msgpack*之前也需要*signalr* 。

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

將 `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` 新增至 `HubConnectionBuilder` 會將用戶端設定為在連接到伺服器時使用 MessagePack 通訊協定。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> 在此階段中，JavaScript 用戶端上沒有 MessagePack 通訊協定的設定選項。

## <a name="messagepack-quirks"></a>MessagePack 的相容

使用 MessagePack Hub 通訊協定時，有幾個要注意的問題。

### <a name="messagepack-is-case-sensitive"></a>MessagePack 區分大小寫

MessagePack 通訊協定會區分大小寫。 例如，請考慮下列C#類別：

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

從 JavaScript 用戶端傳送時，您必須使用 `PascalCased` 的屬性名稱，因為大小寫必須完全C#符合類別。 例如：

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

使用 `camelCased` 名稱並不會正確地C#系結至類別。 您可以使用 `Key` 屬性來指定不同的 MessagePack 屬性名稱，藉此解決此情況。 如需詳細資訊，請參閱[MessagePack-CSharp 檔](https://github.com/neuecc/MessagePack-CSharp#object-serialization)。

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a>序列化/還原序列化時，不會保留 DateTime. Kind

MessagePack 通訊協定不會提供方法來編碼 `DateTime`的 `Kind` 值。 因此，在還原序列化日期時，MessagePack 中樞通訊協定會假設內送日期是 UTC 格式。 如果您要在當地時間使用 `DateTime` 值，建議您在傳送之前先轉換成 UTC。 當您收到時，將它們從 UTC 轉換為當地時間。

如需這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632)。

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a>JavaScript 中的 MessagePack 不支援 MinValue

SignalR JavaScript 用戶端所使用的[msgpack5](https://github.com/mcollina/msgpack5)程式庫不支援 MessagePack 中的 `timestamp96` 類型。 這個型別是用來編碼非常大的日期值（在過去或未來很久的時候）。 `DateTime.MinValue` 的值 `January 1, 0001` 必須以 `timestamp96` 值編碼。 因此，不支援將 `DateTime.MinValue` 傳送至 JavaScript 用戶端。 當 JavaScript 用戶端收到 `DateTime.MinValue` 時，會擲回下列錯誤：

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

通常會使用 `DateTime.MinValue` 來編碼「遺漏」或 `null` 值。 如果您需要在 MessagePack 中編碼該值，請使用可為 null 的 `DateTime` 值（`DateTime?`），或編碼個別的 `bool` 值，以指出日期是否存在。

如需這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228)。

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a>「預先編譯」環境中的 MessagePack 支援

.NET 用戶端和伺服器使用的[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8)程式庫會使用程式碼產生來優化序列化。 因此，在使用「預先」編譯的環境（例如 Xamarin iOS 或 Unity）上，預設不支援它。 在這些環境中，可以藉由「預先產生」序列化程式/還原序列化程式碼來使用 MessagePack。 如需詳細資訊，請參閱[MessagePack-CSharp 檔](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8#pre-code-generationunityxamarin-supports)。 預先產生序列化程式之後，您可以使用傳遞至 `AddMessagePackProtocol`的設定委派來註冊它們：

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

### <a name="type-checks-are-more-strict-in-messagepack"></a>MessagePack 中的類型檢查較嚴格

JSON 中樞通訊協定會在還原序列化期間執行類型轉換。 例如，如果內送物件的屬性值是數位（`{ foo: 42 }`），但是 .NET 類別上的屬性是 `string`類型，則會轉換此值。 不過，MessagePack 不會執行這項轉換，而且會擲回例外狀況，可在伺服器端記錄（和主控台）中看到：

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

如需這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937)。

## <a name="related-resources"></a>相關資源

* [開始使用](xref:tutorials/signalr)
* [.NET 用戶端](xref:signalr/dotnet-client)
* [JavaScript 用戶端](xref:signalr/javascript-client)
