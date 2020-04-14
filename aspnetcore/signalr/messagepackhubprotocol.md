---
title: 在 ASP.NET 核心SignalR中使用 訊息套件集線器協定
author: bradygaster
description: 將訊息套件集線器協定加入SignalRASP.NET 核心 。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/13/2020
no-loc:
- SignalR
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: bbc34d790387a96bb3b6f75e841b45685eb137ce
ms.sourcegitcommit: 5af16166977da598953f82da3ed3b7712d38f6cb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/14/2020
ms.locfileid: "81277232"
---
# <a name="use-messagepack-hub-protocol-in-opno-locsignalr-for-aspnet-core"></a>在 ASP.NET 核心SignalR中使用 訊息套件集線器協定

::: moniker range=">= aspnetcore-5.0"

本文假定讀者[熟悉入門中](xref:tutorials/signalr)介紹的主題。

## <a name="what-is-messagepack"></a>什麼是消息包?

[MessagePack](https://msgpack.org/index.html)是一種快速緊湊的二進位序列化格式。 當性能和頻寬是一個問題時,它很有用,因為它創建的消息比[JSON](https://www.json.org/)少。 二進位訊息在查看網路追蹤和日誌時不可讀,除非位元組透過 MessagePack 解析器傳遞。 SignalR具有對 MessagePack 格式的內建支援,並為用戶端和伺服器提供了要使用的 API。

## <a name="configure-messagepack-on-the-server"></a>在伺服器上設定訊息套件

要在伺服器上啟用 MessagePack 中心協定,`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`請在應用中安裝包。 在`Startup.ConfigureServices`方法中,`AddMessagePackProtocol`添加`AddSignalR`到調用以在伺服器上啟用 MessagePack 支援。

> [!NOTE]
> 默認情況下啟用 JSON。 添加消息包支援 JSON 和 MessagePack 用戶端。

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

要自訂 MessagePack 如何格式化`AddMessagePackProtocol`資料, 請取得用於設定選項的委託。 在該委託中,`SerializerOptions`該屬性可用於配置 MessagePack 序列化選項。 有關解析器如何工作的詳細資訊,請造[訪 MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp)中的 MessagePack 庫。 屬性可用於要序列化的物件,以定義如何處理它們。

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
> 我們強烈建議查看[CVE-2020-5234](https://github.com/neuecc/MessagePack-CSharp/security/advisories/GHSA-7q36-4xx7-xcxf)並應用建議的修補程式。 例如,取代`.WithSecurity(MessagePackSecurity.UntrustedData)``SerializerOptions`時呼叫 。

## <a name="configure-messagepack-on-the-client"></a>在用戶端上設定訊息套件

> [!NOTE]
> 默認情況下,為支援的用戶端啟用 JSON。 用戶端只能支援單個協定。 添加 MessagePack 支援將替換任何以前配置的協定。

### <a name="net-client"></a>.NET 用戶端

在 .NET 客戶端開啟 MessagePack,`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`請安裝`AddMessagePackProtocol`套件`HubConnectionBuilder`並呼叫 。

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> 此`AddMessagePackProtocol`調用需要一個委託來配置類似於伺服器的選項。

### <a name="javascript-client"></a>JavaScript 用戶端

JavaScript 用戶端的消息包支援[@microsoft/signalr-protocol-msgpack](https://www.npmjs.com/package/@microsoft/signalr-protocol-msgpack)由 npm 包提供。 透過在命令外殼中執行以下命令來安裝套件:

```bash
npm install @microsoft/signalr-protocol-msgpack
```

安裝 npm 套件後,模組可以直接透過 JavaScript 模組載入程式使用,或者透過參考以下檔案匯入瀏覽器:

*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* 

在瀏覽器中,`msgpack5`還必須引用庫。 使用標記`<script>`創建引用。 庫可以在*node_modules_msgpack5\dist_msgpack5.js*中找到。

> [!NOTE]
> 使用元素`<script>`時,順序很重要。 如果在*msgpack5.js*之前引用*訊號器協定-msgpack.js,* 則嘗試與 MessagePack 連接時會發生錯誤。 *信號器.js*在*信號器協定-msgpack.js*之前也需要信號。

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

將`.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())``HubConnectionBuilder`用戶端配置為在連接到伺服器時使用 MessagePack 協定。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> 此時,JavaScript 用戶端上的 MessagePack 協定沒有配置選項。

## <a name="messagepack-quirks"></a>訊息包怪癖

使用 MessagePack 中心協定時,需要注意一些問題。

### <a name="messagepack-is-case-sensitive"></a>訊息套件區分大小寫

MessagePack 協定區分大小寫。 例如,請考慮以下 C# 類:

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

從 JavaScript 客戶端發送時`PascalCased`,必須使用 屬性名稱,因為大小寫必須與 C# 類完全匹配。 例如：

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

使用`camelCased`名稱不會正確綁定到 C# 類。 可以使用`Key`屬性為 MessagePack 屬性指定其他名稱來解決此問題。 有關詳細資訊,請參閱[MessagePack-CSharp 文件](https://github.com/neuecc/MessagePack-CSharp#object-serialization)。

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a>序列化/反序列化時不保留日期時間.Kind

MessagePack 協定不提供`Kind`對 的值進行編碼`DateTime`的方法。 因此,在取消序列化日期時,MessagePack 中心協定將轉換為 UTC`DateTime.Kind``DateTimeKind.Local`格式, 否則不會觸控時間並按現在的方式傳遞它。 如果您正在使用`DateTime`值,我們建議您在發送值之前轉換為 UTC。 當您收到它們時,將它們從UTC轉換為本地時間。

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a>JavaScript 中的消息套件不支援日期時間.MinValue

JavaScript 用戶端使用的[msgpack5](https://github.com/mcollina/msgpack5)庫不支援 MessagePack`timestamp96`中的類型。 SignalR 此類型用於編碼非常大的日期值(過去很早就或將來非常遠)。 的值`DateTime.MinValue``January 1, 0001`必須 編`timestamp96`碼在 值中。 因此,不支援發送到`DateTime.MinValue`JAVAScript 用戶端。 當`DateTime.MinValue`JavaScript 用戶端收到時,將引發以下錯誤:

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

通常,`DateTime.MinValue`用於編碼"缺失"`null`或值。 如果需要在 MessagePack 中對該值進行編碼,請`DateTime`使用空`DateTime?`值 (`bool`) 或編碼 單獨的值,指示日期是否存在。

有關此限制的詳細資訊,請參閱 GitHub 問題[aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228)。

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a>"提前"編譯環境中的消息包支援

.NET 用戶端和伺服器使用的[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v2.1.90)庫使用代碼生成來優化序列化。 因此,預設情況下不支援使用"提前"編譯(如 Xamarin iOS 或 Unity)的環境。 可以通過「預生成」序列化器/反序列化器代碼在這些環境中使用 MessagePack。 有關詳細資訊,請參閱[MessagePack-CSharp 文件](https://github.com/neuecc/MessagePack-CSharp/tree/v2.1.90#aot-code-generation-to-support-unityxamarin)。 預先生成序列化器後,可以使用傳遞給 的設定委託註冊`AddMessagePackProtocol`它們 。

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

### <a name="type-checks-are-more-strict-in-messagepack"></a>訊息套件中的類型檢查更加嚴格

JSON 中心協定將在反序列化期間執行類型轉換。 例如,如果傳入物件的屬性值為數位 (),`{ foo: 42 }`但 .NET 類`string`上的屬性為類型 ,則將轉換該值。 但是,MessagePack 不執行此轉換,並且將引發在伺服器端日誌(和控制台中)中可以看到的異常:

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

有關此限制的詳細資訊,請參閱 GitHub 問題[aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937)。

## <a name="related-resources"></a>相關資源

* [入門](xref:tutorials/signalr)
* [.NET 用戶端](xref:signalr/dotnet-client)
* [JavaScript 用戶端](xref:signalr/javascript-client)

::: moniker-end

::: moniker range=">= aspnetcore-3.0 < aspnetcore-5.0"

本文假定讀者[熟悉入門中](xref:tutorials/signalr)介紹的主題。

## <a name="what-is-messagepack"></a>什麼是消息包?

[MessagePack](https://msgpack.org/index.html)是一種快速緊湊的二進位序列化格式。 當性能和頻寬是一個問題時,它很有用,因為它創建的消息比[JSON](https://www.json.org/)少。 二進位訊息在查看網路追蹤和日誌時不可讀,除非位元組透過 MessagePack 解析器傳遞。 SignalR具有對 MessagePack 格式的內建支援,並為用戶端和伺服器提供了要使用的 API。

## <a name="configure-messagepack-on-the-server"></a>在伺服器上設定訊息套件

要在伺服器上啟用 MessagePack 中心協定,`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`請在應用中安裝包。 在`Startup.ConfigureServices`方法中,`AddMessagePackProtocol`添加`AddSignalR`到調用以在伺服器上啟用 MessagePack 支援。

> [!NOTE]
> 默認情況下啟用 JSON。 添加消息包支援 JSON 和 MessagePack 用戶端。

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

要自訂 MessagePack 如何格式化`AddMessagePackProtocol`資料, 請取得用於設定選項的委託。 在該委託中,`FormatterResolvers`該屬性可用於配置 MessagePack 序列化選項。 有關解析器如何工作的詳細資訊,請造[訪 MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp)中的 MessagePack 庫。 屬性可用於要序列化的物件,以定義如何處理它們。

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
> 我們強烈建議查看[CVE-2020-5234](https://github.com/neuecc/MessagePack-CSharp/security/advisories/GHSA-7q36-4xx7-xcxf)並應用建議的修補程式。 例如,將`MessagePackSecurity.Active`靜態屬性設定`MessagePackSecurity.UntrustedData`為 。 設定`MessagePackSecurity.Active`需要手動安裝[1.9.x 版本的 MessagePack](https://www.nuget.org/packages/MessagePack/1.9.3)。 安裝`MessagePack`1.9.xSignalR升級 版本使用。 當`MessagePackSecurity.Active`未設置`MessagePackSecurity.UntrustedData`為 時,惡意用戶端可能會導致拒絕服務。 在`MessagePackSecurity.Active``Program.Main`中設定,如以下代碼所示:

```csharp
public static void Main(string[] args)
{
  MessagePackSecurity.Active = MessagePackSecurity.UntrustedData;

  CreateHostBuilder(args).Build().Run();
}
```

## <a name="configure-messagepack-on-the-client"></a>在用戶端上設定訊息套件

> [!NOTE]
> 默認情況下,為支援的用戶端啟用 JSON。 用戶端只能支援單個協定。 添加 MessagePack 支援將替換任何以前配置的協定。

### <a name="net-client"></a>.NET 用戶端

在 .NET 客戶端開啟 MessagePack,`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`請安裝`AddMessagePackProtocol`套件`HubConnectionBuilder`並呼叫 。

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> 此`AddMessagePackProtocol`調用需要一個委託來配置類似於伺服器的選項。

### <a name="javascript-client"></a>JavaScript 用戶端

JavaScript 用戶端的消息包支援[@microsoft/signalr-protocol-msgpack](https://www.npmjs.com/package/@microsoft/signalr-protocol-msgpack)由 npm 包提供。 透過在命令外殼中執行以下命令來安裝套件:

```bash
npm install @microsoft/signalr-protocol-msgpack
```

安裝 npm 套件後,模組可以直接透過 JavaScript 模組載入程式使用,或者透過參考以下檔案匯入瀏覽器:

*node_modules\\@microsoft\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* 

在瀏覽器中,`msgpack5`還必須引用庫。 使用標記`<script>`創建引用。 庫可以在*node_modules_msgpack5\dist_msgpack5.js*中找到。

> [!NOTE]
> 使用元素`<script>`時,順序很重要。 如果在*msgpack5.js*之前引用*訊號器協定-msgpack.js,* 則嘗試與 MessagePack 連接時會發生錯誤。 *信號器.js*在*信號器協定-msgpack.js*之前也需要信號。

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

將`.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())``HubConnectionBuilder`用戶端配置為在連接到伺服器時使用 MessagePack 協定。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> 此時,JavaScript 用戶端上的 MessagePack 協定沒有配置選項。

## <a name="messagepack-quirks"></a>訊息包怪癖

使用 MessagePack 中心協定時,需要注意一些問題。

### <a name="messagepack-is-case-sensitive"></a>訊息套件區分大小寫

MessagePack 協定區分大小寫。 例如,請考慮以下 C# 類:

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

從 JavaScript 客戶端發送時`PascalCased`,必須使用 屬性名稱,因為大小寫必須與 C# 類完全匹配。 例如：

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

使用`camelCased`名稱不會正確綁定到 C# 類。 可以使用`Key`屬性為 MessagePack 屬性指定其他名稱來解決此問題。 有關詳細資訊,請參閱[MessagePack-CSharp 文件](https://github.com/neuecc/MessagePack-CSharp#object-serialization)。

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a>序列化/反序列化時不保留日期時間.Kind

MessagePack 協定不提供`Kind`對 的值進行編碼`DateTime`的方法。 因此,當對日期進行反序列化時,MessagePack 中心協定假定傳入日期為 UTC 格式。 如果您在本地時間使用`DateTime`值,我們建議您在發送之前轉換為 UTC。 當您收到它們時,將它們從UTC轉換為本地時間。

有關此限制的詳細資訊,請參閱 GitHub 問題[aspnet/#2632SignalR](https://github.com/aspnet/SignalR/issues/2632)。

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a>JavaScript 中的消息套件不支援日期時間.MinValue

JavaScript 用戶端使用的[msgpack5](https://github.com/mcollina/msgpack5)庫不支援 MessagePack`timestamp96`中的類型。 SignalR 此類型用於編碼非常大的日期值(過去很早就或將來非常遠)。 的值`DateTime.MinValue``January 1, 0001`必須 編`timestamp96`碼在 值中。 因此,不支援發送到`DateTime.MinValue`JAVAScript 用戶端。 當`DateTime.MinValue`JavaScript 用戶端收到時,將引發以下錯誤:

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

通常,`DateTime.MinValue`用於編碼"缺失"`null`或值。 如果需要在 MessagePack 中對該值進行編碼,請`DateTime`使用空`DateTime?`值 (`bool`) 或編碼 單獨的值,指示日期是否存在。

有關此限制的詳細資訊,請參閱 GitHub 問題[aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228)。

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a>"提前"編譯環境中的消息包支援

.NET 用戶端和伺服器使用的[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8.80)庫使用代碼生成來優化序列化。 因此,預設情況下不支援使用"提前"編譯(如 Xamarin iOS 或 Unity)的環境。 可以通過「預生成」序列化器/反序列化器代碼在這些環境中使用 MessagePack。 有關詳細資訊,請參閱[MessagePack-CSharp 文件](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8.80#pre-code-generationunityxamarin-supports)。 預先生成序列化器後,可以使用傳遞給 的設定委託註冊`AddMessagePackProtocol`它們 。

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

### <a name="type-checks-are-more-strict-in-messagepack"></a>訊息套件中的類型檢查更加嚴格

JSON 中心協定將在反序列化期間執行類型轉換。 例如,如果傳入物件的屬性值為數位 (),`{ foo: 42 }`但 .NET 類`string`上的屬性為類型 ,則將轉換該值。 但是,MessagePack 不執行此轉換,並且將引發在伺服器端日誌(和控制台中)中可以看到的異常:

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

有關此限制的詳細資訊,請參閱 GitHub 問題[aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937)。

## <a name="related-resources"></a>相關資源

* [入門](xref:tutorials/signalr)
* [.NET 用戶端](xref:signalr/dotnet-client)
* [JavaScript 用戶端](xref:signalr/javascript-client)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

本文假定讀者[熟悉入門中](xref:tutorials/signalr)介紹的主題。

## <a name="what-is-messagepack"></a>什麼是消息包?

[MessagePack](https://msgpack.org/index.html)是一種快速緊湊的二進位序列化格式。 當性能和頻寬是一個問題時,它很有用,因為它創建的消息比[JSON](https://www.json.org/)少。 二進位訊息在查看網路追蹤和日誌時不可讀,除非位元組透過 MessagePack 解析器傳遞。 SignalR具有對 MessagePack 格式的內建支援,並為用戶端和伺服器提供了要使用的 API。

## <a name="configure-messagepack-on-the-server"></a>在伺服器上設定訊息套件

要在伺服器上啟用 MessagePack 中心協定,`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`請在應用中安裝包。 在`Startup.ConfigureServices`方法中,`AddMessagePackProtocol`添加`AddSignalR`到調用以在伺服器上啟用 MessagePack 支援。

> [!NOTE]
> 默認情況下啟用 JSON。 添加消息包支援 JSON 和 MessagePack 用戶端。

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

要自訂 MessagePack 如何格式化`AddMessagePackProtocol`資料, 請取得用於設定選項的委託。 在該委託中,`FormatterResolvers`該屬性可用於配置 MessagePack 序列化選項。 有關解析器如何工作的詳細資訊,請造[訪 MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp)中的 MessagePack 庫。 屬性可用於要序列化的物件,以定義如何處理它們。

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
> 我們強烈建議查看[CVE-2020-5234](https://github.com/neuecc/MessagePack-CSharp/security/advisories/GHSA-7q36-4xx7-xcxf)並應用建議的修補程式。 例如,將`MessagePackSecurity.Active`靜態屬性設定`MessagePackSecurity.UntrustedData`為 。 設定`MessagePackSecurity.Active`需要手動安裝[1.9.x 版本的 MessagePack](https://www.nuget.org/packages/MessagePack/1.9.3)。 安裝`MessagePack`1.9.xSignalR升級 版本使用。 當`MessagePackSecurity.Active`未設置`MessagePackSecurity.UntrustedData`為 時,惡意用戶端可能會導致拒絕服務。 在`MessagePackSecurity.Active``Program.Main`中設定,如以下代碼所示:

```csharp
public static void Main(string[] args)
{
  MessagePackSecurity.Active = MessagePackSecurity.UntrustedData;

  CreateHostBuilder(args).Build().Run();
}
```

## <a name="configure-messagepack-on-the-client"></a>在用戶端上設定訊息套件

> [!NOTE]
> 默認情況下,為支援的用戶端啟用 JSON。 用戶端只能支援單個協定。 添加 MessagePack 支援將替換任何以前配置的協定。

### <a name="net-client"></a>.NET 用戶端

在 .NET 客戶端開啟 MessagePack,`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`請安裝`AddMessagePackProtocol`套件`HubConnectionBuilder`並呼叫 。

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> 此`AddMessagePackProtocol`調用需要一個委託來配置類似於伺服器的選項。

### <a name="javascript-client"></a>JavaScript 用戶端

JavaScript 用戶端的消息包支援[@aspnet/signalr-protocol-msgpack](https://www.npmjs.com/package/@aspnet/signalr-protocol-msgpack)由 npm 包提供。 透過在命令外殼中執行以下命令來安裝套件:

```bash
npm install @aspnet/signalr-protocol-msgpack
```

安裝 npm 套件後,模組可以直接透過 JavaScript 模組載入程式使用,或者透過參考以下檔案匯入瀏覽器:

*node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*

在瀏覽器中,`msgpack5`還必須引用庫。 使用標記`<script>`創建引用。 庫可以在*node_modules_msgpack5\dist_msgpack5.js*中找到。

> [!NOTE]
> 使用元素`<script>`時,順序很重要。 如果在*msgpack5.js*之前引用*訊號器協定-msgpack.js,* 則嘗試與 MessagePack 連接時會發生錯誤。 *信號器.js*在*信號器協定-msgpack.js*之前也需要信號。

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

將`.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())``HubConnectionBuilder`用戶端配置為在連接到伺服器時使用 MessagePack 協定。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> 此時,JavaScript 用戶端上的 MessagePack 協定沒有配置選項。

## <a name="messagepack-quirks"></a>訊息包怪癖

使用 MessagePack 中心協定時,需要注意一些問題。

### <a name="messagepack-is-case-sensitive"></a>訊息套件區分大小寫

MessagePack 協定區分大小寫。 例如,請考慮以下 C# 類:

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

從 JavaScript 客戶端發送時`PascalCased`,必須使用 屬性名稱,因為大小寫必須與 C# 類完全匹配。 例如：

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

使用`camelCased`名稱不會正確綁定到 C# 類。 可以使用`Key`屬性為 MessagePack 屬性指定其他名稱來解決此問題。 有關詳細資訊,請參閱[MessagePack-CSharp 文件](https://github.com/neuecc/MessagePack-CSharp#object-serialization)。

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a>序列化/反序列化時不保留日期時間.Kind

MessagePack 協定不提供`Kind`對 的值進行編碼`DateTime`的方法。 因此,當對日期進行反序列化時,MessagePack 中心協定假定傳入日期為 UTC 格式。 如果您在本地時間使用`DateTime`值,我們建議您在發送之前轉換為 UTC。 當您收到它們時,將它們從UTC轉換為本地時間。

有關此限制的詳細資訊,請參閱 GitHub 問題[aspnet/#2632SignalR](https://github.com/aspnet/SignalR/issues/2632)。

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a>JavaScript 中的消息套件不支援日期時間.MinValue

JavaScript 用戶端使用的[msgpack5](https://github.com/mcollina/msgpack5)庫不支援 MessagePack`timestamp96`中的類型。 SignalR 此類型用於編碼非常大的日期值(過去很早就或將來非常遠)。 的值`DateTime.MinValue``January 1, 0001`必須編碼`timestamp96`在 值中。 因此,不支援發送到`DateTime.MinValue`JAVAScript 用戶端。 當`DateTime.MinValue`JavaScript 用戶端收到時,將引發以下錯誤:

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

通常,`DateTime.MinValue`用於編碼"缺失"`null`或值。 如果需要在 MessagePack 中對該值進行編碼,請`DateTime`使用空`DateTime?`值 (`bool`) 或編碼 單獨的值,指示日期是否存在。

有關此限制的詳細資訊,請參閱 GitHub 問題[aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228)。

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a>"提前"編譯環境中的消息包支援

.NET 用戶端和伺服器使用的[MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8.80)庫使用代碼生成來優化序列化。 因此,預設情況下不支援使用"提前"編譯(如 Xamarin iOS 或 Unity)的環境。 可以通過「預生成」序列化器/反序列化器代碼在這些環境中使用 MessagePack。 有關詳細資訊,請參閱[MessagePack-CSharp 文件](https://github.com/neuecc/MessagePack-CSharp/tree/v1.8.80#pre-code-generationunityxamarin-supports)。 預先生成序列化器後,可以使用傳遞給 的設定委託註冊`AddMessagePackProtocol`它們 。

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

### <a name="type-checks-are-more-strict-in-messagepack"></a>訊息套件中的類型檢查更加嚴格

JSON 中心協定將在反序列化期間執行類型轉換。 例如,如果傳入物件的屬性值為數位 (),`{ foo: 42 }`但 .NET 類`string`上的屬性為類型 ,則將轉換該值。 但是,MessagePack 不執行此轉換,並且將引發在伺服器端日誌(和控制台中)中可以看到的異常:

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

有關此限制的詳細資訊,請參閱 GitHub 問題[aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937)。

## <a name="related-resources"></a>相關資源

* [入門](xref:tutorials/signalr)
* [.NET 用戶端](xref:signalr/dotnet-client)
* [JavaScript 用戶端](xref:signalr/javascript-client)

::: moniker-end
