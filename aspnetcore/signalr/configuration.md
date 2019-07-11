---
title: ASP.NET Core SignalR 組態
author: bradygaster
description: 了解如何設定 ASP.NET Core SignalR 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/03/2019
uid: signalr/configuration
ms.openlocfilehash: 8c9fcaecb04555718f5da6a42a8e56c258e795af
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/11/2019
ms.locfileid: "67813445"
---
# <a name="aspnet-core-signalr-configuration"></a>ASP.NET Core SignalR 組態

## <a name="jsonmessagepack-serialization-options"></a>JSON/MessagePack 序列化選項

ASP.NET Core SignalR 支援兩種通訊協定，針對訊息進行編碼：[JSON](https://www.json.org/)並[MessagePack](https://msgpack.org/index.html)。 每個通訊協定有序列化組態選項。

可以在 伺服器設定 JSON 序列化[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴充方法，可以在之後加入[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)中您`Startup.ConfigureServices`方法。 `AddJsonProtocol`方法採用委派來接收`options`物件。 [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)屬性，該物件是 JSON.NET`JsonSerializerSettings`可用來設定序列化的引數和傳回值的物件。 請參閱[JSON.NET 文件](https://www.newtonsoft.com/json/help/html/Introduction.htm)如需詳細資訊。

例如，若要設定序列化程式使用 「 PascalCase"的屬性名稱，而不是預設的 「 camelCase 」 名稱，使用下列程式碼：

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
        new DefaultContractResolver();
    });
```

在.NET 用戶端，相同`AddJsonProtocol`延伸模組方法存在於[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)。 `Microsoft.Extensions.DependencyInjection`必須匯入命名空間，若要解決的擴充方法：

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    })
    .Build();
```

> [!NOTE]
> 您不可能進行 JSON 序列化的 JavaScript 用戶端中，這一次。

### <a name="messagepack-serialization-options"></a>MessagePack 序列化選項

您可以設定 MessagePack 序列化提供的委派[AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼叫。 請參閱[signalr MessagePack](xref:signalr/messagepackhubprotocol)如需詳細資訊。

> [!NOTE]
> 您不可以在 JavaScript 用戶端設定 MessagePack 序列化，這一次。

## <a name="configure-server-options"></a>設定伺服器選項

下表說明設定 SignalR 中樞的選項：

::: moniker range=">= aspnetcore-3.0"

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | 30 秒 | 伺服器會考慮在用戶端已中斷連線，如果它未在此時間間隔中收到訊息 （包括為 keep-alive）。 可能需要超過用戶端實際上會標示為已中斷連線，因為這如何實作此逾時間隔。 建議的值是雙精度浮點`KeepAliveInterval`值。|
| `HandshakeTimeout` | 15 秒 | 如果用戶端不在此時間間隔內傳送初始信號交換訊息，就會關閉連線。 這是應該只在交握逾時錯誤是因嚴重的網路延遲而未發生才修改進階的設定。 如需詳細的交握程序的詳細資訊，請參閱[SignalR 中樞的通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。 |
| `KeepAliveInterval` | 15 秒 | 如果伺服器尚未在此間隔內傳送一則訊息，是自動的 ping 訊息傳送至保持開啟的連接。 變更時`KeepAliveInterval`，變更`ServerTimeout` / `serverTimeoutInMilliseconds`設定用戶端上。 建議`ServerTimeout` / `serverTimeoutInMilliseconds`的值是雙精度浮點`KeepAliveInterval`值。  |
| `SupportedProtocols` | 所有已安裝的通訊協定 | 此中樞支援的通訊協定。 根據預設，在伺服器上註冊的所有通訊協定，但可以從這個清單，以停用個別的中樞的特定通訊協定中移除通訊協定。 |
| `EnableDetailedErrors` | `false` | 如果`true`詳細例外狀況訊息傳回給用戶端中樞方法中擲回例外狀況時。 預設值是`false`，因為這些例外狀況訊息可以包含機密資訊。 |
| `StreamBufferCapacity` | `10` | 最大用戶端就可以緩衝處理的項目上傳的資料流。 如果達到這個限制時，伺服器處理資料流項目之前封鎖引動過程的處理。|

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | 30 秒 | 伺服器會考慮在用戶端已中斷連線，如果它未在此時間間隔中收到訊息 （包括為 keep-alive）。 可能需要超過用戶端實際上會標示為已中斷連線，因為這如何實作此逾時間隔。 建議的值是雙精度浮點`KeepAliveInterval`值。|
| `HandshakeTimeout` | 15 秒 | 如果用戶端不在此時間間隔內傳送初始信號交換訊息，就會關閉連線。 這是應該只在交握逾時錯誤是因嚴重的網路延遲而未發生才修改進階的設定。 如需詳細的交握程序的詳細資訊，請參閱[SignalR 中樞的通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。 |
| `KeepAliveInterval` | 15 秒 | 如果伺服器尚未在此間隔內傳送一則訊息，是自動的 ping 訊息傳送至保持開啟的連接。 變更時`KeepAliveInterval`，變更`ServerTimeout` / `serverTimeoutInMilliseconds`設定用戶端上。 建議`ServerTimeout` / `serverTimeoutInMilliseconds`的值是雙精度浮點`KeepAliveInterval`值。  |
| `SupportedProtocols` | 所有已安裝的通訊協定 | 此中樞支援的通訊協定。 根據預設，在伺服器上註冊的所有通訊協定，但可以從這個清單，以停用個別的中樞的特定通訊協定中移除通訊協定。 |
| `EnableDetailedErrors` | `false` | 如果`true`詳細例外狀況訊息傳回給用戶端中樞方法中擲回例外狀況時。 預設值是`false`，因為這些例外狀況訊息可以包含機密資訊。 |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | 15 秒 | 如果用戶端不在此時間間隔內傳送初始信號交換訊息，就會關閉連線。 這是應該只在交握逾時錯誤是因嚴重的網路延遲而未發生才修改進階的設定。 如需詳細的交握程序的詳細資訊，請參閱[SignalR 中樞的通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。 |
| `KeepAliveInterval` | 15 秒 | 如果伺服器尚未在此間隔內傳送一則訊息，是自動的 ping 訊息傳送至保持開啟的連接。 變更時`KeepAliveInterval`，變更`ServerTimeout` / `serverTimeoutInMilliseconds`設定用戶端上。 建議`ServerTimeout` / `serverTimeoutInMilliseconds`的值是雙精度浮點`KeepAliveInterval`值。  |
| `SupportedProtocols` | 所有已安裝的通訊協定 | 此中樞支援的通訊協定。 根據預設，在伺服器上註冊的所有通訊協定，但可以從這個清單，以停用個別的中樞的特定通訊協定中移除通訊協定。 |
| `EnableDetailedErrors` | `false` | 如果`true`詳細例外狀況訊息傳回給用戶端中樞方法中擲回例外狀況時。 預設值是`false`，因為這些例外狀況訊息可以包含機密資訊。 |

::: moniker-end

可以針對所有中樞設定選項，藉由提供選項委派`AddSignalR`呼叫中`Startup.ConfigureServices`。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    });
}
```

單一中樞的選項會覆寫中提供的全域選項`AddSignalR`，並可使用設定<xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a>進階的 HTTP 組態選項

使用`HttpConnectionDispatcherOptions`設定傳輸及記憶體緩衝區管理相關的進階的設定。 這些選項會設定由傳遞至委派[MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub)在`Startup.Configure`。

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseSignalR((configure) =>
    {
        var desiredTransports =
            HttpTransportType.WebSockets |
            HttpTransportType.LongPolling;

        configure.MapHub<MyHub>("/myhub", (options) =>
        {
            options.Transports = desiredTransports;
        });
    });
}
```

下表說明用於設定 ASP.NET Core SignalR 的進階的 HTTP 選項的選項：

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | 32 KB | 從用戶端接收的位元組數目上限，伺服器緩衝區。 增加此值可讓伺服器接收較大的訊息，但可能會造成負面影響記憶體耗用量。 |
| `AuthorizationData` | 從自動收集資料`Authorize`套用至 Hub 類別的屬性。 | 一份[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件用來判斷是否授權用戶端來連線至中樞。 |
| `TransportMaxBufferSize` | 32 KB | 應用程式所傳送的位元組數目上限，伺服器緩衝區。 增加此值可讓伺服器傳送較大的訊息，但可能會造成負面影響記憶體耗用量。 |
| `Transports` | 會啟用所有的傳輸。 | 位元遮罩`HttpTransportType`值，可以限制傳輸用戶端可用來連接。 |
| `LongPolling` | 請參閱下方。 | 特有的長輪詢傳輸的其他選項。 |
| `WebSockets` | 請參閱下方。 | Websocket 傳輸特有的其他選項。 |

長輪詢傳輸的其他選項，您可以使用設定`LongPolling`屬性：

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `PollTimeout` | 90 秒 | 伺服器等候訊息傳送至用戶端終止單一的輪詢要求之前的最大時間量。 減少此值會導致更頻繁地發出新的輪詢要求用戶端。 |

WebSocket 傳輸具有您可以使用設定的其他選項`WebSockets`屬性：

| 選項 | 預設值 | 說明 |
| ------ | ------------- | ----------- |
| `CloseTimeout` | 5 秒 | 伺服器關閉，如果用戶端無法在此時間間隔內關閉後，會結束連接。 |
| `SubProtocolSelector` | `null` | 委派，可以用來設定`Sec-WebSocket-Protocol`為某個自訂值的標頭。 委派會接收要求的用戶端，做為輸入值，並預期會傳回所需的值。 |

## <a name="configure-client-options"></a>設定用戶端選項

可設定上的用戶端選項`HubConnectionBuilder`（適用於.NET 和 JavaScript 的用戶端） 的型別。 它也會提供的 Java 用戶端，但`HttpHubConnectionBuilder`子類別是在包含的產生器的組態選項，以及`HubConnection`本身。

### <a name="configure-logging"></a>設定記錄

記錄已在使用.NET 用戶端中`ConfigureLogging`方法。 記錄提供者和篩選可以註冊在相同的方式保持不在伺服器上。 請參閱[ASP.NET Core 中的記錄](xref:fundamentals/logging/index)文件的詳細資訊。

> [!NOTE]
> 若要註冊的記錄提供者，您必須安裝必要的套件。 請參閱[內建記錄提供者](xref:fundamentals/logging/index#built-in-logging-providers)區段的文件，如需完整清單。

例如，若要啟用主控台記錄，請安裝`Microsoft.Extensions.Logging.Console`NuGet 套件。 呼叫`AddConsole`擴充方法：

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

在 JavaScript 用戶端，類似`configureLogging`方法存在。 提供`LogLevel`值，指出產生的記錄檔訊息的最低層級。 記錄會寫入至瀏覽器的 [主控台] 視窗中。

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

而不是`LogLevel`值，您也可以提供`string`值，表示記錄層級的名稱。 設定記錄在環境中的 SignalR 中並沒有存取權時，這是很有用`LogLevel`常數。

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

下表列出可用的記錄層級。 您所提供的值`configureLogging`設定**最小**記錄所記錄的層級。 在此層級，記錄的訊息 **，或之後它列出資料表中的層級**，將會記錄。

| 字串 | LogLevel |
| - | - |
| `"trace"` | `LogLevel.Trace` |
| `"debug"` | `LogLevel.Debug` |
| `"info"` **或** `"information"` | `LogLevel.Information` |
| `"warn"` **或** `"warning"` | `LogLevel.Warning` |
| `"error"` | `LogLevel.Error` |
| `"critical"` | `LogLevel.Critical` |
| `"none"` | `LogLevel.None` |

::: moniker-end

> [!NOTE]
> 若要停用整個記錄，請指定`signalR.LogLevel.None`在`configureLogging`方法。

如需有關記錄的詳細資訊，請參閱 < [SignalR 診斷文件](xref:signalr/diagnostics)。

SignalR Java 用戶端會使用[SLF4J](https://www.slf4j.org/)記錄的程式庫。 它是高層級的記錄 API，可讓程式庫的使用者選擇他們自己的特定記錄實作，藉由將特定記錄相依性。 下列程式碼片段示範如何使用`java.util.logging`與 SignalR Java 用戶端。

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

如果您未設定登入您的相依性，SLF4J 會載入預設的無作業記錄器，並出現下列警告訊息：

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

這可以放心地忽略。

### <a name="configure-allowed-transports"></a>設定允許的傳輸

SignalR 使用的傳輸可以設定為在`WithUrl`呼叫 (`withUrl`在 JavaScript 中)。 位元 OR 之值的`HttpTransportType`可用來限制用戶端只使用指定的傳輸。 預設會啟用所有的傳輸。

例如，若要停用 Server-Sent 事件傳輸，但允許 Websocket 及長輪詢連線：

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

在 JavaScript 用戶端，透過設定來設定傳輸`transport`提供給選項物件欄位`withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

在這個版本的 Java 用戶端 websocket 會是唯一可用的傳輸。

::: moniker-end

::: moniker range="= aspnetcore-3.0"

中的 Java 用戶端中，選取傳輸與`withTransport`方法`HttpHubConnectionBuilder`。 Java 用戶端預設會使用 Websocket 傳輸。

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> SignalR Java 用戶端尚不支援傳輸後援。

::: moniker-end

### <a name="configure-bearer-authentication"></a>設定持有人驗證

若要提供與 SignalR 要求的驗證資料，請使用`AccessTokenProvider`選項 (`accessTokenFactory`在 JavaScript 中) 來指定傳回的所需的存取權杖的函式。 在.NET 用戶端，此存取權杖會傳遞為 HTTP 「 持有人驗證 」 權杖 (使用`Authorization`類型的標頭`Bearer`)。 在 JavaScript 用戶端的存取權杖做為持有人權杖，**除了**在少數情況下，瀏覽器 Api 限制能夠套用標頭 （特別是，在 Server-Sent 事件和 Websocket 要求）。 在這些情況下，存取權杖中提供的查詢字串值`access_token`。

在.NET 用戶端`AccessTokenProvider`可以指定選項，使用中的選項委派`WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

在 JavaScript 用戶端，透過設定來設定存取權杖`accessTokenFactory`欄位中的選項物件`withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        accessTokenFactory: () => {
            // Get and return the access token.
            // This function can return a JavaScript Promise if asynchronous
            // logic is required to retrieve the access token.
        }
    })
    .build();
```

SignalR Java 用戶端，在中，您可以設定要用於驗證所提供的存取語彙基元 factory，以持有人權杖[HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)。 使用[withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJava](https://github.com/ReactiveX/RxJava) [單一\<字串 >](https://reactivex.io/documentation/single.html)。 藉由呼叫[Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)，您可以撰寫邏輯，以針對您的用戶端產生存取權杖。

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a>設定逾時和保持連線選項

設定逾時和持續連線行為的其他選項可供使用`HubConnection`物件本身：

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

| 選項 | 預設值 | 說明 |
| ------ | ------------- | ----------- |
| `ServerTimeout` | 30 秒 （30,000 毫秒） | 伺服器活動的逾時。 如果伺服器未傳送訊息，此時間間隔中，用戶端會視為中斷連線的 server 和觸發程序`Closed`事件 (`onclose`在 JavaScript 中)。 此值必須夠大，從伺服器傳送的 ping 訊息**和**逾時間隔內收到用戶端。 建議的值是數字在至少兩倍的伺服器的`KeepAliveInterval`值，以允許 ping 抵達的時間。 |
| `HandshakeTimeout` | 15 秒 | 初始伺服器交握的逾時。 如果伺服器不會傳送交握回應此時間間隔中，用戶端便會取消交握和觸發程序`Closed`事件 (`onclose`在 JavaScript 中)。 這是應該只在交握逾時錯誤是因嚴重的網路延遲而未發生才修改進階的設定。 如需詳細的交握程序的詳細資訊，請參閱[SignalR 中樞的通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。 |

在.NET 用戶端逾時的值會指定為`TimeSpan`值。

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

| 選項 | 預設值 | 說明 |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | 30 秒 （30,000 毫秒） | 伺服器活動的逾時。 如果伺服器未傳送訊息，此時間間隔中，用戶端會視為中斷連線的 server 和觸發程序`onclose`事件。 此值必須夠大，從伺服器傳送的 ping 訊息**和**逾時間隔內收到用戶端。 建議的值是數字在至少兩倍的伺服器的`KeepAliveInterval`值，以允許 ping 抵達的時間。 |

# <a name="javatabjava"></a>[Java](#tab/java)

| 選項 | 預設值 | 描述 |
| ----------- | ------------- | ----------- |
|`getServerTimeout` `setServerTimeout` | 30 秒 （30,000 毫秒） | 伺服器活動的逾時。 如果伺服器未傳送訊息，此時間間隔中，用戶端會視為中斷連線的 server 和觸發程序`onClose`事件。 此值必須夠大，從伺服器傳送的 ping 訊息**和**逾時間隔內收到用戶端。 建議的值是數字在至少兩倍的伺服器的`KeepAliveInterval`值，以允許 ping 抵達的時間。 |
| `withHandshakeResponseTimeout` | 15 秒 | 初始伺服器交握的逾時。 如果伺服器不會傳送交握回應此時間間隔中，用戶端便會取消交握和觸發程序`onClose`事件。 這是應該只在交握逾時錯誤是因嚴重的網路延遲而未發生才修改進階的設定。 如需詳細的交握程序的詳細資訊，請參閱[SignalR 中樞的通訊協定規格](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。 |

---

### <a name="configure-additional-options"></a>設定其他選項

可以設定其他選項`WithUrl`(`withUrl`在 JavaScript 中) 上的方法`HubConnectionBuilder`或各種組態 Api 上`HttpHubConnectionBuilder`中的 Java 用戶端：

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

| .NET 選項 |  預設值 | 描述 |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | 傳回字串，做為持有人驗證權杖，在 HTTP 要求中提供的函式。 |
| `SkipNegotiation` | `false` | 將此設為`true`略過交涉步驟。 **Websocket 傳輸方式是只啟用的傳輸時，才支援**。 使用 Azure SignalR 服務時，就無法啟用此設定。 |
| `ClientCertificates` | Empty | 傳送驗證要求的 TLS 憑證的集合。 |
| `Cookies` | Empty | 要與每個 HTTP 要求一起傳送的 HTTP cookie 的集合。 |
| `Credentials` | Empty | 要與每個 HTTP 要求一起傳送的認證。 |
| `CloseTimeout` | 5 秒 | 只有 WebSockets。 最大時間量，該用戶端會等待伺服器以確認在關閉要求的結尾後面。 如果伺服器不在此時間內認可關閉，則用戶端中斷連線。 |
| `Headers` | Empty | 要與每個 HTTP 要求一起傳送的其他 HTTP 標頭的對應。 |
| `HttpMessageHandlerFactory` | `null` | 委派，可用來設定或取代`HttpMessageHandler`用來傳送 HTTP 要求。 不使用 WebSocket 連線。 此委派必須傳回非 null 值，並且會收到做為參數的預設值。 修改設定，該預設值，並傳回它，或傳回新`HttpMessageHandler`執行個體。 **當複製您想要保留從提供的處理常式的設定，請務必取代處理常式，否則設定的選項 （例如 Cookie 和標頭） 不會套用至新的處理常式。** |
| `Proxy` | `null` | 傳送 HTTP 要求時要使用 HTTP proxy。 |
| `UseDefaultCredentials` | `false` | 設定這個傳送 HTTP 和 Websocket 要求的預設認證的布林值。 這可讓使用 Windows 驗證。 |
| `WebSocketConfiguration` | `null` | 委派，可用來設定其他的 WebSocket 選項。 收到的執行個體[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) ，可用來設定選項。 |

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

| JavaScript 選項 | 預設值 | 描述 |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | 傳回字串，做為持有人驗證權杖，在 HTTP 要求中提供的函式。 |
| `skipNegotiation` | `false` | 將此設為`true`略過交涉步驟。 **Websocket 傳輸方式是只啟用的傳輸時，才支援**。 使用 Azure SignalR 服務時，就無法啟用此設定。 |

# <a name="javatabjava"></a>[Java](#tab/java)

| Java 選項 | 預設值 | 描述 |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | 傳回字串，做為持有人驗證權杖，在 HTTP 要求中提供的函式。 |
| `shouldSkipNegotiate` | `false` | 將此設為`true`略過交涉步驟。 **Websocket 傳輸方式是只啟用的傳輸時，才支援**。 使用 Azure SignalR 服務時，就無法啟用此設定。 |
| `withHeader` `withHeaders` | Empty | 要與每個 HTTP 要求一起傳送的其他 HTTP 標頭的對應。 |

---

在.NET 用戶端，可以修改這些選項所提供的選項委派`WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

在 JavaScript 用戶端，可以提供這些選項中提供的 JavaScript 物件`withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

在 Java 用戶端，這些選項可以設定的方法上`HttpHubConnectionBuilder`傳回 `HubConnectionBuilder.create("HUB URL")`

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a>其他資源

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
