---
title: 在中使用 MessagePack 中樞通訊協定 SignalR 來 ASP.NET Core
author: bradygaster
description: 將 MessagePack 中樞通訊協定新增至 ASP.NET Core SignalR 。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/13/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: ff78321cba11b1c91a12b5c777c505b4c9b85309
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85408314"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a>在中使用 MessagePack 中樞通訊協定 SignalR 來 ASP.NET Core

::: moniker range=">= aspnetcore-5.0"

本文假設讀者已熟悉[開始](xref:tutorials/signalr)使用中所涵蓋的主題。

## <a name="what-is-messagepack"></a>什麼是 MessagePack？

[MessagePack](https://msgpack.org/index.html)是快速且精簡的二進位序列化格式。 這在效能和頻寬有所顧慮時很有用，因為它會建立比[JSON](https://www.json.org/)更小的訊息。 在查看網路追蹤和記錄檔時，二進位訊息無法讀取，除非是透過 MessagePack 剖析器傳遞位元組。 SignalR具有 MessagePack 格式的內建支援，並提供 Api 供用戶端和伺服器使用。

## <a name="configure-messagepack-on-the-server"></a>在伺服器上設定 MessagePack

若要在伺服器上啟用 MessagePack Hub 通訊協定，請 `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` 在您的應用程式中安裝套件。 在 `Startup.ConfigureServices` 方法中，將加入 `AddMessagePackProtocol` 至 `AddSignalR` 呼叫以在伺服器上啟用 MessagePack 支援。

> [!NOTE]
> 預設會啟用 JSON。 新增 MessagePack 可同時支援 JSON 和 MessagePack 用戶端。

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

若要自訂 MessagePack 如何將資料格式化，請 `AddMessagePackProtocol` 採用委派來設定選項。 在該委派中， `SerializerOptions` 屬性可以用來設定 MessagePack 序列化選項。 如需解決器如何工作的詳細資訊，請造訪[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp)上的 MessagePack 程式庫。 屬性可以在您要序列化的物件上使用，以定義它們的處理方式。

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.SerializerOptions = MessagePackSerializerOptions.Standard
            .WithResolver(new CustomResolver())
            .WithSecurity(MessagePackSecurity.UntrustedData);
    });
```

> [!WARNING]
> 我們強烈建議您查看[CVE-2020-5234](https://github.com/neuecc/MessagePack-CSharp/security/advisories/GHSA-7q36-4xx7-xcxf) ，並套用建議的修補程式。 例如， `.WithSecurity(MessagePackSecurity.UntrustedData)` 在取代時呼叫 `SerializerOptions` 。

## <a name="configure-messagepack-on-the-client"></a>設定用戶端上的 MessagePack

> [!NOTE]
> 根據預設，支援的用戶端會啟用 JSON。 用戶端只能支援單一通訊協定。 新增 MessagePack 支援將會取代任何先前設定的通訊協定。

### <a name="net-client"></a>.NET 用戶端

若要在 .NET 用戶端中啟用 MessagePack，請安裝 `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` 封裝，並 `AddMessagePackProtocol` 在上呼叫 `HubConnectionBuilder` 。

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chathub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> 此 `AddMessagePackProtocol` 呼叫會使用委派來設定選項，就像伺服器一樣。

### <a name="javascript-client"></a>JavaScript 用戶端

Npm 套件會提供 JavaScript 用戶端的 MessagePack 支援 [@microsoft/signalr-protocol-msgpack](https://www.npmjs.com/package/@microsoft/signalr-protocol-msgpack) 。 在命令 shell 中執行下列命令，以安裝套件：

```bash
npm install @microsoft/signalr-protocol-msgpack
```

安裝 npm 套件之後，您可以直接透過 JavaScript 模組載入器使用模組，或藉由參考下列檔案，將其匯入至瀏覽器：

*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* 

在瀏覽器中， `msgpack5` 也必須參考程式庫。 使用 `<script>` 標記來建立參考。 您可以在*node_modules\msgpack5\dist\msgpack5.js*找到此程式庫。

> [!NOTE]
> 使用元素時 `<script>` ，順序很重要。 如果在*msgpack5.js*之前參考*signalr-protocol-msgpack.js* ，嘗試使用 MessagePack 連接時就會發生錯誤。 *signalr-protocol-msgpack.js*之前也需要*signalr.js* 。

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

`.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())`將加入至 `HubConnectionBuilder` 會將用戶端設定為在連接到伺服器時使用 MessagePack 通訊協定。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> 在此階段中，JavaScript 用戶端上沒有 MessagePack 通訊協定的設定選項。

## <a name="messagepack-quirks"></a>MessagePack 的相容

使用 MessagePack Hub 通訊協定時，有幾個要注意的問題。

### <a name="messagepack-is-case-sensitive"></a>MessagePack 區分大小寫

MessagePack 通訊協定會區分大小寫。 例如，請考慮下列 c # 類別：

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

從 JavaScript 用戶端傳送時，您必須使用 `PascalCased` 屬性名稱，因為大小寫必須完全符合 c # 類別。 例如：

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

使用 `camelCased` 名稱並不會正確地系結至 c # 類別。 您可以使用 `Key` 屬性來指定不同的 MessagePack 屬性名稱，藉此解決此情況。 如需詳細資訊，請參閱[MessagePack-CSharp 檔](https://github.com/neuecc/MessagePack-CSharp#object-serialization)。

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a>序列化/還原序列化時，不會保留 DateTime. Kind

MessagePack 通訊協定不會提供方法來編碼的 `Kind` 值 `DateTime` 。 如此一來，當還原序列化日期時，如果是，MessagePack 中樞通訊協定將會轉換成 UTC 格式， `DateTime.Kind` `DateTimeKind.Local` 否則將不會觸及時間並依原本傳遞。 如果您要使用值，建議您在傳送 `DateTime` 之前先轉換成 UTC。 當您收到時，將它們從 UTC 轉換為當地時間。

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a>JavaScript 中的 MessagePack 不支援 MinValue

JavaScript 用戶端所使用的[msgpack5](https://github.com/mcollina/msgpack5)程式庫 SignalR 不支援 `timestamp96` MessagePack 中的類型。 這個型別是用來編碼非常大的日期值（在過去或未來很久的時候）。 的值 `DateTime.MinValue` 是 `January 1, 0001` ，必須以 `timestamp96` 值編碼。 因此， `DateTime.MinValue` 不支援傳送至 JavaScript 用戶端。 當 `DateTime.MinValue` JavaScript 用戶端收到時，會擲回下列錯誤：

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

通常 `DateTime.MinValue` 會使用來編碼「遺漏」或 `null` 值。 如果您需要在 MessagePack 中將該值編碼，請使用可為 null 的 `DateTime` 值（ `DateTime?` ），或編碼個別的 `bool` 值，以指出日期是否存在。

如需這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/ SignalR #2228](https://github.com/aspnet/SignalR/issues/2228)。

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a>「預先編譯」環境中的 MessagePack 支援

.NET 用戶端和伺服器使用的[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v2.1.90)程式庫會使用程式碼產生來優化序列化。 因此，在使用「預先」編譯的環境（例如 Xamarin iOS 或 Unity）上，預設不支援它。 在這些環境中，可以藉由「預先產生」序列化程式/還原序列化程式碼來使用 MessagePack。 如需詳細資訊，請參閱[MessagePack-CSharp 檔](https://github.com/neuecc/MessagePack-CSharp/tree/v2.1.90#aot-code-generation-to-support-unityxamarin)。 當您預先產生序列化程式之後，您可以使用傳遞至的設定委派來進行註冊 `AddMessagePackProtocol` ：

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        StaticCompositeResolver.Instance.Register(
            MessagePack.Resolvers.GeneratedResolver.Instance,
            MessagePack.Resolvers.StandardResolver.Instance
        );
        options.SerializerOptions = MessagePackSerializerOptions.Standard
            .WithResolver(StaticCompositeResolver.Instance)
            .WithSecurity(MessagePackSecurity.UntrustedData);
    });
```

### <a name="type-checks-are-more-strict-in-messagepack"></a>MessagePack 中的類型檢查較嚴格

JSON 中樞通訊協定會在還原序列化期間執行類型轉換。 例如，如果內送物件的屬性值是數位（ `{ foo: 42 }` ），但 .net 類別上的屬性類型為 `string` ，則會轉換此值。 不過，MessagePack 不會執行這項轉換，而且會擲回例外狀況，可在伺服器端記錄（和主控台）中看到：

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

如需這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/ SignalR #2937](https://github.com/aspnet/SignalR/issues/2937)。

## <a name="related-resources"></a>相關資源

* [開始使用](xref:tutorials/signalr)
* [.NET 用戶端](xref:signalr/dotnet-client)
* [JavaScript 用戶端](xref:signalr/javascript-client)

::: moniker-end

::: moniker range=">= aspnetcore-3.0 < aspnetcore-5.0"

本文假設讀者已熟悉[開始](xref:tutorials/signalr)使用中所涵蓋的主題。

## <a name="what-is-messagepack"></a>什麼是 MessagePack？

[MessagePack](https://msgpack.org/index.html)是快速且精簡的二進位序列化格式。 這在效能和頻寬有所顧慮時很有用，因為它會建立比[JSON](https://www.json.org/)更小的訊息。 在查看網路追蹤和記錄檔時，二進位訊息無法讀取，除非是透過 MessagePack 剖析器傳遞位元組。 SignalR具有 MessagePack 格式的內建支援，並提供 Api 供用戶端和伺服器使用。

## <a name="configure-messagepack-on-the-server"></a>在伺服器上設定 MessagePack

若要在伺服器上啟用 MessagePack Hub 通訊協定，請 `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` 在您的應用程式中安裝套件。 在 `Startup.ConfigureServices` 方法中，將加入 `AddMessagePackProtocol` 至 `AddSignalR` 呼叫以在伺服器上啟用 MessagePack 支援。

> [!NOTE]
> 預設會啟用 JSON。 新增 MessagePack 可同時支援 JSON 和 MessagePack 用戶端。

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

若要自訂 MessagePack 如何將資料格式化，請 `AddMessagePackProtocol` 採用委派來設定選項。 在該委派中， `FormatterResolvers` 屬性可以用來設定 MessagePack 序列化選項。 如需解決器如何工作的詳細資訊，請造訪[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp)上的 MessagePack 程式庫。 屬性可以在您要序列化的物件上使用，以定義它們的處理方式。

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

> [!WARNING]
> 我們強烈建議您查看[CVE-2020-5234](https://github.com/neuecc/MessagePack-CSharp/security/advisories/GHSA-7q36-4xx7-xcxf) ，並套用建議的修補程式。 例如，將 `MessagePackSecurity.Active` 靜態屬性設定為 `MessagePackSecurity.UntrustedData` 。 設定 `MessagePackSecurity.Active` 需要以手動方式安裝[1.9. x 版的 MessagePack](https://www.nuget.org/packages/MessagePack/1.9.3)。 安裝 `MessagePack` 1.9. x 升級版本 SignalR 使用。 當不 `MessagePackSecurity.Active` 是設定為時 `MessagePackSecurity.UntrustedData` ，惡意用戶端可能會導致拒絕服務。 `MessagePackSecurity.Active`在中設定 `Program.Main` ，如下列程式碼所示：

```csharp
public static void Main(string[] args)
{
  MessagePackSecurity.Active = MessagePackSecurity.UntrustedData;

  CreateHostBuilder(args).Build().Run();
}
```

## <a name="configure-messagepack-on-the-client"></a>設定用戶端上的 MessagePack

> [!NOTE]
> 根據預設，支援的用戶端會啟用 JSON。 用戶端只能支援單一通訊協定。 新增 MessagePack 支援將會取代任何先前設定的通訊協定。

### <a name="net-client"></a>.NET 用戶端

若要在 .NET 用戶端中啟用 MessagePack，請安裝 `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` 封裝，並 `AddMessagePackProtocol` 在上呼叫 `HubConnectionBuilder` 。

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chathub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> 此 `AddMessagePackProtocol` 呼叫會使用委派來設定選項，就像伺服器一樣。

### <a name="javascript-client"></a>JavaScript 用戶端

Npm 套件會提供 JavaScript 用戶端的 MessagePack 支援 [@microsoft/signalr-protocol-msgpack](https://www.npmjs.com/package/@microsoft/signalr-protocol-msgpack) 。 在命令 shell 中執行下列命令，以安裝套件：

```bash
npm install @microsoft/signalr-protocol-msgpack
```

安裝 npm 套件之後，您可以直接透過 JavaScript 模組載入器使用模組，或藉由參考下列檔案，將其匯入至瀏覽器：

*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* 

在瀏覽器中， `msgpack5` 也必須參考程式庫。 使用 `<script>` 標記來建立參考。 您可以在*node_modules\msgpack5\dist\msgpack5.js*找到此程式庫。

> [!NOTE]
> 使用元素時 `<script>` ，順序很重要。 如果在*msgpack5.js*之前參考*signalr-protocol-msgpack.js* ，嘗試使用 MessagePack 連接時就會發生錯誤。 *signalr-protocol-msgpack.js*之前也需要*signalr.js* 。

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

`.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())`將加入至 `HubConnectionBuilder` 會將用戶端設定為在連接到伺服器時使用 MessagePack 通訊協定。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> 在此階段中，JavaScript 用戶端上沒有 MessagePack 通訊協定的設定選項。

## <a name="messagepack-quirks"></a>MessagePack 的相容

使用 MessagePack Hub 通訊協定時，有幾個要注意的問題。

### <a name="messagepack-is-case-sensitive"></a>MessagePack 區分大小寫

MessagePack 通訊協定會區分大小寫。 例如，請考慮下列 c # 類別：

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

從 JavaScript 用戶端傳送時，您必須使用 `PascalCased` 屬性名稱，因為大小寫必須完全符合 c # 類別。 例如：

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

使用 `camelCased` 名稱並不會正確地系結至 c # 類別。 您可以使用 `Key` 屬性來指定不同的 MessagePack 屬性名稱，藉此解決此情況。 如需詳細資訊，請參閱[MessagePack-CSharp 檔](https://github.com/neuecc/MessagePack-CSharp#object-serialization)。

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a>序列化/還原序列化時，不會保留 DateTime. Kind

MessagePack 通訊協定不會提供方法來編碼的 `Kind` 值 `DateTime` 。 因此，在還原序列化日期時，MessagePack 中樞通訊協定會假設內送日期是 UTC 格式。 如果您使用 `DateTime` 本機時間的值，建議您在傳送之前先轉換成 UTC。 當您收到時，將它們從 UTC 轉換為當地時間。

如需這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/ SignalR #2632](https://github.com/aspnet/SignalR/issues/2632)。

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a>JavaScript 中的 MessagePack 不支援 MinValue

JavaScript 用戶端所使用的[msgpack5](https://github.com/mcollina/msgpack5)程式庫 SignalR 不支援 `timestamp96` MessagePack 中的類型。 這個型別是用來編碼非常大的日期值（在過去或未來很久的時候）。 的值 `DateTime.MinValue` 是 `January 1, 0001` ，必須以 `timestamp96` 值編碼。 因此， `DateTime.MinValue` 不支援傳送至 JavaScript 用戶端。 當 `DateTime.MinValue` JavaScript 用戶端收到時，會擲回下列錯誤：

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

通常 `DateTime.MinValue` 會使用來編碼「遺漏」或 `null` 值。 如果您需要在 MessagePack 中將該值編碼，請使用可為 null 的 `DateTime` 值（ `DateTime?` ），或編碼個別的 `bool` 值，以指出日期是否存在。

如需這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/ SignalR #2228](https://github.com/aspnet/SignalR/issues/2228)。

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a>「預先編譯」環境中的 MessagePack 支援

.NET 用戶端和伺服器使用的[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8.80)程式庫會使用程式碼產生來優化序列化。 因此，在使用「預先」編譯的環境（例如 Xamarin iOS 或 Unity）上，預設不支援它。 在這些環境中，可以藉由「預先產生」序列化程式/還原序列化程式碼來使用 MessagePack。 如需詳細資訊，請參閱[MessagePack-CSharp 檔](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8.80#pre-code-generationunityxamarin-supports)。 當您預先產生序列化程式之後，您可以使用傳遞至的設定委派來進行註冊 `AddMessagePackProtocol` ：

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

JSON 中樞通訊協定會在還原序列化期間執行類型轉換。 例如，如果內送物件的屬性值是數位（ `{ foo: 42 }` ），但 .net 類別上的屬性類型為 `string` ，則會轉換此值。 不過，MessagePack 不會執行這項轉換，而且會擲回例外狀況，可在伺服器端記錄（和主控台）中看到：

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

如需這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/ SignalR #2937](https://github.com/aspnet/SignalR/issues/2937)。

## <a name="related-resources"></a>相關資源

* [開始使用](xref:tutorials/signalr)
* [.NET 用戶端](xref:signalr/dotnet-client)
* [JavaScript 用戶端](xref:signalr/javascript-client)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

本文假設讀者已熟悉[開始](xref:tutorials/signalr)使用中所涵蓋的主題。

## <a name="what-is-messagepack"></a>什麼是 MessagePack？

[MessagePack](https://msgpack.org/index.html)是快速且精簡的二進位序列化格式。 這在效能和頻寬有所顧慮時很有用，因為它會建立比[JSON](https://www.json.org/)更小的訊息。 在查看網路追蹤和記錄檔時，二進位訊息無法讀取，除非是透過 MessagePack 剖析器傳遞位元組。 SignalR具有 MessagePack 格式的內建支援，並提供 Api 供用戶端和伺服器使用。

## <a name="configure-messagepack-on-the-server"></a>在伺服器上設定 MessagePack

若要在伺服器上啟用 MessagePack Hub 通訊協定，請 `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` 在您的應用程式中安裝套件。 在 `Startup.ConfigureServices` 方法中，將加入 `AddMessagePackProtocol` 至 `AddSignalR` 呼叫以在伺服器上啟用 MessagePack 支援。

> [!NOTE]
> 預設會啟用 JSON。 新增 MessagePack 可同時支援 JSON 和 MessagePack 用戶端。

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

若要自訂 MessagePack 如何將資料格式化，請 `AddMessagePackProtocol` 採用委派來設定選項。 在該委派中， `FormatterResolvers` 屬性可以用來設定 MessagePack 序列化選項。 如需解決器如何工作的詳細資訊，請造訪[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp)上的 MessagePack 程式庫。 屬性可以在您要序列化的物件上使用，以定義它們的處理方式。

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

> [!WARNING]
> 我們強烈建議您查看[CVE-2020-5234](https://github.com/neuecc/MessagePack-CSharp/security/advisories/GHSA-7q36-4xx7-xcxf) ，並套用建議的修補程式。 例如，將 `MessagePackSecurity.Active` 靜態屬性設定為 `MessagePackSecurity.UntrustedData` 。 設定 `MessagePackSecurity.Active` 需要以手動方式安裝[1.9. x 版的 MessagePack](https://www.nuget.org/packages/MessagePack/1.9.3)。 安裝 `MessagePack` 1.9. x 升級版本 SignalR 使用。 當不 `MessagePackSecurity.Active` 是設定為時 `MessagePackSecurity.UntrustedData` ，惡意用戶端可能會導致拒絕服務。 `MessagePackSecurity.Active`在中設定 `Program.Main` ，如下列程式碼所示：

```csharp
public static void Main(string[] args)
{
  MessagePackSecurity.Active = MessagePackSecurity.UntrustedData;

  CreateHostBuilder(args).Build().Run();
}
```

## <a name="configure-messagepack-on-the-client"></a>設定用戶端上的 MessagePack

> [!NOTE]
> 根據預設，支援的用戶端會啟用 JSON。 用戶端只能支援單一通訊協定。 新增 MessagePack 支援將會取代任何先前設定的通訊協定。

### <a name="net-client"></a>.NET 用戶端

若要在 .NET 用戶端中啟用 MessagePack，請安裝 `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` 封裝，並 `AddMessagePackProtocol` 在上呼叫 `HubConnectionBuilder` 。

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chathub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> 此 `AddMessagePackProtocol` 呼叫會使用委派來設定選項，就像伺服器一樣。

### <a name="javascript-client"></a>JavaScript 用戶端

Npm 套件會提供 JavaScript 用戶端的 MessagePack 支援 [@aspnet/signalr-protocol-msgpack](https://www.npmjs.com/package/@aspnet/signalr-protocol-msgpack) 。 在命令 shell 中執行下列命令，以安裝套件：

```bash
npm install @aspnet/signalr-protocol-msgpack
```

安裝 npm 套件之後，您可以直接透過 JavaScript 模組載入器使用模組，或藉由參考下列檔案，將其匯入至瀏覽器：

*node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*

在瀏覽器中， `msgpack5` 也必須參考程式庫。 使用 `<script>` 標記來建立參考。 您可以在*node_modules\msgpack5\dist\msgpack5.js*找到此程式庫。

> [!NOTE]
> 使用元素時 `<script>` ，順序很重要。 如果在*msgpack5.js*之前參考*signalr-protocol-msgpack.js* ，嘗試使用 MessagePack 連接時就會發生錯誤。 *signalr-protocol-msgpack.js*之前也需要*signalr.js* 。

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

`.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())`將加入至 `HubConnectionBuilder` 會將用戶端設定為在連接到伺服器時使用 MessagePack 通訊協定。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> 在此階段中，JavaScript 用戶端上沒有 MessagePack 通訊協定的設定選項。

## <a name="messagepack-quirks"></a>MessagePack 的相容

使用 MessagePack Hub 通訊協定時，有幾個要注意的問題。

### <a name="messagepack-is-case-sensitive"></a>MessagePack 區分大小寫

MessagePack 通訊協定會區分大小寫。 例如，請考慮下列 c # 類別：

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

從 JavaScript 用戶端傳送時，您必須使用 `PascalCased` 屬性名稱，因為大小寫必須完全符合 c # 類別。 例如：

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

使用 `camelCased` 名稱並不會正確地系結至 c # 類別。 您可以使用 `Key` 屬性來指定不同的 MessagePack 屬性名稱，藉此解決此情況。 如需詳細資訊，請參閱[MessagePack-CSharp 檔](https://github.com/neuecc/MessagePack-CSharp#object-serialization)。

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a>序列化/還原序列化時，不會保留 DateTime. Kind

MessagePack 通訊協定不會提供方法來編碼的 `Kind` 值 `DateTime` 。 因此，在還原序列化日期時，MessagePack 中樞通訊協定會假設內送日期是 UTC 格式。 如果您使用 `DateTime` 本機時間的值，建議您在傳送之前先轉換成 UTC。 當您收到時，將它們從 UTC 轉換為當地時間。

如需這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/ SignalR #2632](https://github.com/aspnet/SignalR/issues/2632)。

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a>JavaScript 中的 MessagePack 不支援 MinValue

JavaScript 用戶端所使用的[msgpack5](https://github.com/mcollina/msgpack5)程式庫 SignalR 不支援 `timestamp96` MessagePack 中的類型。 這個型別是用來編碼非常大的日期值（在過去或未來很久的時候）。 的值 `DateTime.MinValue` `January 1, 0001` 必須以 `timestamp96` 值編碼。 因此， `DateTime.MinValue` 不支援傳送至 JavaScript 用戶端。 當 `DateTime.MinValue` JavaScript 用戶端收到時，會擲回下列錯誤：

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

通常 `DateTime.MinValue` 會使用來編碼「遺漏」或 `null` 值。 如果您需要在 MessagePack 中將該值編碼，請使用可為 null 的 `DateTime` 值（ `DateTime?` ），或編碼個別的 `bool` 值，以指出日期是否存在。

如需這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/ SignalR #2228](https://github.com/aspnet/SignalR/issues/2228)。

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a>「預先編譯」環境中的 MessagePack 支援

.NET 用戶端和伺服器使用的[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8.80)程式庫會使用程式碼產生來優化序列化。 因此，在使用「預先」編譯的環境（例如 Xamarin iOS 或 Unity）上，預設不支援它。 在這些環境中，可以藉由「預先產生」序列化程式/還原序列化程式碼來使用 MessagePack。 如需詳細資訊，請參閱[MessagePack-CSharp 檔](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8.80#pre-code-generationunityxamarin-supports)。 當您預先產生序列化程式之後，您可以使用傳遞至的設定委派來進行註冊 `AddMessagePackProtocol` ：

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

JSON 中樞通訊協定會在還原序列化期間執行類型轉換。 例如，如果內送物件的屬性值是數位（ `{ foo: 42 }` ），但 .net 類別上的屬性類型為 `string` ，則會轉換此值。 不過，MessagePack 不會執行這項轉換，而且會擲回例外狀況，可在伺服器端記錄（和主控台）中看到：

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

如需這項限制的詳細資訊，請參閱 GitHub 問題[aspnet/ SignalR #2937](https://github.com/aspnet/SignalR/issues/2937)。

## <a name="related-resources"></a>相關資源

* [開始使用](xref:tutorials/signalr)
* [.NET 用戶端](xref:signalr/dotnet-client)
* [JavaScript 用戶端](xref:signalr/javascript-client)

::: moniker-end
