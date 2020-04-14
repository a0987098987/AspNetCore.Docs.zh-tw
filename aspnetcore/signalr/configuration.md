---
title: ASP.NET核心SignalR設定
author: bradygaster
description: 瞭解如何配置ASP.NET核心SignalR應用。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/12/2020
no-loc:
- SignalR
uid: signalr/configuration
ms.openlocfilehash: 9f98a9c6371ef7e38586b0d728c670b0eecb8f6e
ms.sourcegitcommit: fbdb8b9ab5a52656384b117ff6e7c92ae070813c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/13/2020
ms.locfileid: "81228136"
---
# <a name="aspnet-core-signalr-configuration"></a>ASP.NET Core SignalR 組態

::: moniker range=">= aspnetcore-3.1"

## <a name="jsonmessagepack-serialization-options"></a>JSON/訊息套件序列化選項

ASP.NET核心訊號R支援兩種用於編碼訊息的協定[:JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。 每個協定都有序列化配置選項。

JSON 序列化可以使用[AddJson 協定](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴展方法在伺服器上配置。 `AddJsonProtocol`可以在`Startup.ConfigureServices`[中新增SignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)後新增 。 該方法`AddJsonProtocol`採用`options`接收 物件的委託。 該物件的[Payload 序列化器選項](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions)屬性是`System.Text.Json`<xref:System.Text.Json.JsonSerializerOptions>可用於配置參數和返回值序列化的物件。 關於詳細資訊,請參閱[系統.Text.Json 文件](/dotnet/api/system.text.json)。

例如,要將序列化程式設定為不變更屬性名稱的大小寫,而不是預設的「camelCase」名稱,請使用中的`Startup.ConfigureServices`以下代碼:

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

在 .NET`AddJsonProtocol`用戶端中[,HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上存在相同的擴充方法。 必須`Microsoft.Extensions.DependencyInjection`匯入命名空間以解決延伸方法:

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null;
    })
    .Build();
```

> [!NOTE]
> 此時無法在 JavaScript 用戶端中設定 JSON 序列化。

### <a name="switch-to-newtonsoftjson"></a>切換到牛頓軟. Json

如果您需要在 中不`Newtonsoft.Json`支援`System.Text.Json`的功能 ,請參閱[切換到 Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson)。

### <a name="messagepack-serialization-options"></a>訊息套件序列化選項

消息包序列化可以通過提供對[AddMessagePack 協定](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)調用的委託來配置。 有關詳細資訊[,請參閱 SignalR 中的訊息套件](xref:signalr/messagepackhubprotocol)。

> [!NOTE]
> 此時無法在 JavaScript 用戶端中設定 MessagePack 序列化。

## <a name="configure-server-options"></a>設定伺服器選項

下表描述了用於設定 SignalR 集線器的選項:

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | 30 秒 | 如果用戶端未在此間隔內收到消息(包括保持活動狀態),伺服器將考慮用戶端斷開連接。 由於如何實現,實際將客戶端標記為斷開連接的時間可能比此超時間隔長。 建議的值是值的`KeepAliveInterval`兩倍。|
| `HandshakeTimeout` | 15 秒 | 如果用戶端未在此時間間隔內發送初始握手消息,則連接將關閉。 這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。 有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。 |
| `KeepAliveInterval` | 15 秒 | 如果伺服器未在此間隔內發送消息,則會自動發送 ping 消息以保持連接打開狀態。 更改`KeepAliveInterval`時,`ServerTimeout`/`serverTimeoutInMilliseconds`更改 用戶端上的設置。 建議的值是值的`KeepAliveInterval`兩倍。`serverTimeoutInMilliseconds` `ServerTimeout` /  |
| `SupportedProtocols` | 所有已安裝的協定 | 此中心支持的協定。 默認情況下,允許在伺服器上註冊的所有協定,但可以從此清單中刪除協定以禁用各個集線器的特定協定。 |
| `EnableDetailedErrors` | `false` | 如果在`true`Hub 方法中引發異常時,將返回到用戶端的詳細異常消息。 默認值為`false`,因為這些異常消息可能包含敏感資訊。 |
| `StreamBufferCapacity` | `10` | 可緩衝用戶端上載流的最大項數。 如果達到此限制,則在伺服器處理流項之前將阻止調用的處理。|
| `MaximumReceiveMessageSize` | 32 KB | 單個傳入中心消息的最大大小。 |

可以通過在`AddSignalR``Startup.ConfigureServices`中 提供委託給調用的選項,為所有集線器配置選項。

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

單一組集線器的選項覆`AddSignalR`寫 中 提供的全域選項,並且<xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>可以使用 設定 :

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a>進階 HTTP 設定選項

用於`HttpConnectionDispatcherOptions`配置與傳輸和記憶體緩衝區管理相關的高級設置。 這些選項通過將委託傳遞給[MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub)在`Startup.Configure`中配置。

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<MyHub>("/myhub", options =>
        {
            options.Transports =
                HttpTransportType.WebSockets |
                HttpTransportType.LongPolling;
        });
    });
}
```

下表描述了用於設定ASP.NET核心訊號R的進階 HTTP 選項的選項:

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | 32 KB | 在應用背壓之前,伺服器緩衝的用戶端接收的最大位元組數。 增加此值允許伺服器在不施加背壓的情況下更快地接收更大的消息,但會增加記憶體消耗。 |
| `AuthorizationData` | 數據自動從應用於中心`Authorize`類的屬性中收集。 | 用於確定用戶端是否有權連接到集線器的[I授權數據](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件的清單。 |
| `TransportMaxBufferSize` | 32 KB | 在觀察背壓之前,伺服器緩衝的應用發送的最大位元組數。 增加此值允許伺服器更快地緩衝較大的郵件,而無需等待回壓,但會增加記憶體消耗。 |
| `Transports` | 所有傳輸都已啟用。 | 位標記值枚舉,`HttpTransportType`這些值可以限制用戶端可用於連接的傳輸。 |
| `LongPolling` | 請參閱下列內容。 | 特定於長輪詢傳輸的其他選項。 |
| `WebSockets` | 請參閱下列內容。 | 特定於 WebSocket 傳輸的其他選項。 |
| `MinimumProtocolVersion` | 0 | 指定協商協定的最低版本。 這用於將用戶端限制為較新版本。 |

長輪詢傳輸具有可以使用`LongPolling`屬性 設定的其他選項:

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `PollTimeout` | 90 秒 | 伺服器在終止單個輪詢請求之前等待消息發送到用戶端的最大時間。 減小此值會導致用戶端更頻繁地發出新的輪詢請求。 |

WebSocket 傳輸具有可以使用`WebSockets`屬性 設定的其他選項:

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `CloseTimeout` | 5 秒 | 伺服器關閉後,如果用戶端未能在此時間間隔內關閉,連接將終止。 |
| `SubProtocolSelector` | `null` | 可用於將`Sec-WebSocket-Protocol`標頭設置為自定義值的委託。 委託接收用戶端請求的值作為輸入,並預期返回所需的值。 |

## <a name="configure-client-options"></a>設定客戶端選項

可以在類型上`HubConnectionBuilder`配置客戶端選項(在 .NET 和 JavaScript 用戶端中可用)。 它在 Java 用戶端中也可用`HttpHubConnectionBuilder`,但子類包含生成器配置`HubConnection`選項以及 本身。

### <a name="configure-logging"></a>設定記錄

使用`ConfigureLogging`方法在 .NET 用戶端中配置日誌記錄。 日誌記錄提供者和篩選器的註冊方式與在伺服器上的註冊方式相同。 有關詳細資訊[,請參閱登錄ASP.NET核心](xref:fundamentals/logging/index)文檔。

> [!NOTE]
> 要註冊日誌記錄提供程式,必須安裝必要的包。 有關完整清單,請參閱文檔的[內建記錄提供程式](xref:fundamentals/logging/index#built-in-logging-providers)部分。

例如,要啟用主控台日誌記錄`Microsoft.Extensions.Logging.Console`, 請安裝 NuGet 套件。 呼叫`AddConsole`式副用:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

在 JavaScript 用戶端`configureLogging`中, 存在類似的方法。 提供指示`LogLevel`要生產的日誌消息的最小級別的值。 日誌將寫入瀏覽器控制台視窗。

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

還可以提供表示`LogLevel`日誌級別`string`名稱的值,而不是值。 這在無法存`LogLevel`取 常量的環境中配置 SignalR 日誌記錄時非常有用。

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

下表列出了可用的日誌級別。 您提供的值來`configureLogging`設置將記錄的**最小**紀錄級別。 將紀錄在此等級紀錄的訊息,**或表中列出的訊息等級**。

| String                      | LogLevel               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| `info`**或**`information` | `LogLevel.Information` |
| `warn`**或**`warning`     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

> [!NOTE]
> 要完全禁用日誌記錄,請在`signalR.LogLevel.None``configureLogging`方法中指定。

關於紀錄紀錄的詳細資訊,請參考[訊號的文件](xref:signalr/diagnostics)。

SignalR Java 用戶端使用[SLF4J](https://www.slf4j.org/)庫進行日誌記錄。 它是一種高級日誌記錄 API,允許庫的用戶通過引入特定的日誌記錄依賴項來選擇自己的特定日誌記錄實現。 以下代碼段演示如何與 SignalR `java.util.logging` Java 用戶端一起使用。

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

如果不在依賴項中設定日誌記錄,SLF4J 將載入具有以下警告訊息的預設無操作記錄器:

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

這可以安全地忽略。

### <a name="configure-allowed-transports"></a>設定允許的傳輸

SignalR 使用的傳輸`WithUrl`可以在 調用`withUrl`中( 在 JavaScript 中)進行配置。 值`HttpTransportType`的位 OR 可用於限制用戶端僅使用指定的傳輸。 默認情況下,所有傳輸都已啟用。

例如,要禁用伺服器發送事件傳輸,但允許 WebSocket 和長輪詢連接:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

在 JavaScript 用戶端中,透過設定提供給`transport`的選項 物件上的`withUrl`欄位來 設定傳輸:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

在此版本中,JAva 用戶端 Websocket 是唯一可用的傳輸。

在 Java 用戶端中,`withTransport`使用上的方法選擇`HttpHubConnectionBuilder`傳輸。 Java 用戶端預設使用 WebSocket 傳輸。

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> SignalR Java 用戶端還不支援傳輸回退。

### <a name="configure-bearer-authentication"></a>設定無記名身份驗證

要提供身份驗證數據以及 SignalR 請求,`AccessTokenProvider`請`accessTokenFactory`使用 選項 (在 JavaScript 中)指定傳回所需存取權杖的函數。 在 .NET 用戶端中,此訪問權杖作為 HTTP"承載身份驗證"令牌傳`Authorization`入(`Bearer`使用具有的標頭的類型)。 在 JavaScript 用戶端中,存取權杖用作承載權杖,**除非**瀏覽器 API 限制應用標頭的能力(特別是在伺服器發送事件和 WebSocket 請求中)。 在這些情況下,訪問權杖作為查詢字串值`access_token`提供。

在 .NET`AccessTokenProvider`用戶端中 ,`WithUrl`可以使用 中的選項委託指定該選項:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

在 JavaScript 用戶端中,通過`accessTokenFactory``withUrl`設置 中的選項物件上的欄位來設定訪問權杖。

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

在 SignalR Java 用戶端中,可以通過向[HttpHubConnectBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)提供訪問權杖工廠來配置承載權杖以用於身份驗證。 [與AccessTokenFactory一起](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJAVA](https://github.com/ReactiveX/RxJava)[單\<字串>。](https://reactivex.io/documentation/single.html) 調用[Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-),可以編寫邏輯來生成客戶端的訪問權杖。

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a>設定逾時與保持作用選項

`HubConnection`用於設定逾時與保持作用行為的其他選項在物件本身上可用:

# <a name="net"></a>[.NET](#tab/dotnet)

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `ServerTimeout` | 30 秒(30,000 毫秒) | 伺服器活動的超時。 如果伺服器未在此間隔內發送消息,則客戶端將認為伺服器已斷開連接並觸`Closed`發`onclose`事件( 在 JAvaScript 中)。 此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。 建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。 |
| `HandshakeTimeout` | 15 秒 | 初始伺服器握手的超時。 如果伺服器未在此間隔內發送握手回應,用戶端將取消握手並觸發`Closed`事件`onclose`( 在 JavaScript 中)。 這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。 有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。 |
| `KeepAliveInterval` | 15 秒 | 確定客戶端發送 ping 消息的時間間隔。 從用戶端發送任何消息會將計時器重置為間隔的開始。 如果用戶端未在伺服器上的`ClientTimeoutInterval`集中發送消息,則伺服器將認為客戶端已斷開連接。 |

在 .NET 用戶端中,超時值`TimeSpan`指定為值。

# <a name="javascript"></a>[JavaScript](#tab/javascript)

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | 30 秒(30,000 毫秒) | 伺服器活動的超時。 如果伺服器未在此間隔內發送消息,則客戶端認為伺服器已斷開連接並觸`onclose`發 事件。 此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。 建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。 |
| `keepAliveIntervalInMilliseconds` | 15 秒(15,000 毫秒) | 確定客戶端發送 ping 消息的時間間隔。 從用戶端發送任何消息會將計時器重置為間隔的開始。 如果用戶端未在伺服器上的`ClientTimeoutInterval`集中發送消息,則伺服器將認為客戶端已斷開連接。 |

# <a name="java"></a>[Java](#tab/java)

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `getServerTimeout` / `setServerTimeout` | 30 秒(30,000 毫秒) | 伺服器活動的超時。 如果伺服器未在此間隔內發送消息,則客戶端認為伺服器已斷開連接並觸`onClose`發 事件。 此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。 建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。 |
| `withHandshakeResponseTimeout` | 15 秒 | 初始伺服器握手的超時。 如果伺服器未在此間隔內發送握手回應,用戶端將取消握手並觸發`onClose`事件。 這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。 有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。 |
| `getKeepAliveInterval` / `setKeepAliveInterval` | 15 秒(15,000 毫秒) | 確定客戶端發送 ping 消息的時間間隔。 從用戶端發送任何消息會將計時器重置為間隔的開始。 如果用戶端未在伺服器上的`ClientTimeoutInterval`集中發送消息,則伺服器將認為客戶端已斷開連接。 |

---

### <a name="configure-additional-options"></a>設定其他選項

其他選項可以在 Java`WithUrl``withUrl`用戶端 上或`HubConnectionBuilder`JAva 用戶端上 的各種設定 API 上`HttpHubConnectionBuilder`(JAvaScript)方法 中設定:

# <a name="net"></a>[.NET](#tab/dotnet)

| .NET 選項 |  預設值 | 描述 |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | 返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。 |
| `SkipNegotiation` | `false` | 將此`true`設置為跳過協商步驟。 **只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。 使用 Azure SignalR 服務時無法啟用此設定。 |
| `ClientCertificates` | 空白 | 要發送到身份驗證請求的 TLS 證書的集合。 |
| `Cookies` | 空白 | 要隨每個 HTTP 請求一起發送的 HTTP Cookie 集合。 |
| `Credentials` | 空白 | 要隨每個 HTTP 請求一起發送的認證。 |
| `CloseTimeout` | 5 秒 | 僅限 Web 插座。 用戶端關閉後等待伺服器確認關閉請求的最大時間量。 如果伺服器在此時間內未確認關閉,則客戶端將斷開連接。 |
| `Headers` | 空白 | 要隨每個 HTTP 請求一起發送的其他 HTTP 標頭的映射。 |
| `HttpMessageHandlerFactory` | `null` | 可用於配置或替換`HttpMessageHandler`用於發送 HTTP 請求的委託。 不用於 Web 套接字連接。 此委託必須返回一個非空值,並且它將作為參數接收預設值。 修改該預設值上的設置並返回該設置,或返回新`HttpMessageHandler`實例。 **替換處理程式時,請確保從提供的處理程式複製要保留的設置,否則,配置的選項(如 Cookie 和標頭)將不適用於新處理程式。** |
| `Proxy` | `null` | 傳送 HTTP 請求時要使用的 HTTP 代理。 |
| `UseDefaultCredentials` | `false` | 設置此布爾以發送 HTTP 和 WebSocket 請求的預設認證。 這支援使用 Windows 身份驗證。 |
| `WebSocketConfiguration` | `null` | 可用於配置其他 WebSocket 選項的委託。 接收可用於配置選項的[用戶端WebSocket選項](/dotnet/api/system.net.websockets.clientwebsocketoptions)的實例。 |

# <a name="javascript"></a>[JavaScript](#tab/javascript)

| JavaScript 選項 | 預設值 | 描述 |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | 返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。 |
| `skipNegotiation` | `false` | 將此`true`設置為跳過協商步驟。 **只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。 使用 Azure SignalR 服務時無法啟用此設定。 |

# <a name="java"></a>[Java](#tab/java)

| Java 選項 | 預設值 | 描述 |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | 返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。 |
| `shouldSkipNegotiate` | `false` | 將此`true`設置為跳過協商步驟。 **只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。 使用 Azure SignalR 服務時無法啟用此設定。 |
| `withHeader` `withHeaders` | 空白 | 要隨每個 HTTP 請求一起發送的其他 HTTP 標頭的映射。 |

---

在 .NET 用戶端中,這些選項可以`WithUrl`通過提供給 : 提供的選項委託進行修改:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

在 JavaScript 用戶端中,這些選項可以在`withUrl`提供給 的 JavaScript 物件中提供:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

在 Java 用戶端中,`HttpHubConnectionBuilder`可以使用 從`HubConnectionBuilder.create("HUB URL")`

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

::: moniker-end
::: moniker range="= aspnetcore-3.0"

## <a name="jsonmessagepack-serialization-options"></a>JSON/訊息套件序列化選項

ASP.NET核心訊號R支援兩種用於編碼訊息的協定[:JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。 每個協定都有序列化配置選項。

JSON 序列化可以使用[AddJson 協定](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴展方法在伺服器上配置。 `AddJsonProtocol`可以在`Startup.ConfigureServices`[中新增SignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)後新增 。 該方法`AddJsonProtocol`採用`options`接收 物件的委託。 該物件的[Payload 序列化器選項](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions)屬性是`System.Text.Json`<xref:System.Text.Json.JsonSerializerOptions>可用於配置參數和返回值序列化的物件。 關於詳細資訊,請參閱[系統.Text.Json 文件](/dotnet/api/system.text.json)。

例如,要將序列化程式設定為不變更屬性名稱的大小寫,而不是預設的「camelCase」名稱,請使用中的`Startup.ConfigureServices`以下代碼:

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null;
    });
```

在 .NET`AddJsonProtocol`用戶端中[,HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上存在相同的擴充方法。 必須`Microsoft.Extensions.DependencyInjection`匯入命名空間以解決延伸方法:

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null;
    })
    .Build();
```

> [!NOTE]
> 此時無法在 JavaScript 用戶端中設定 JSON 序列化。

### <a name="switch-to-newtonsoftjson"></a>切換到牛頓軟. Json

如果您需要在 中不`Newtonsoft.Json`支援`System.Text.Json`的功能 ,請參閱[切換到 Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson)。

### <a name="messagepack-serialization-options"></a>訊息套件序列化選項

消息包序列化可以通過提供對[AddMessagePack 協定](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)調用的委託來配置。 有關詳細資訊[,請參閱 SignalR 中的訊息套件](xref:signalr/messagepackhubprotocol)。

> [!NOTE]
> 此時無法在 JavaScript 用戶端中設定 MessagePack 序列化。

## <a name="configure-server-options"></a>設定伺服器選項

下表描述了用於設定 SignalR 集線器的選項:

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | 30 秒 | 如果用戶端未在此間隔內收到消息(包括保持活動狀態),伺服器將考慮用戶端斷開連接。 由於如何實現,實際將客戶端標記為斷開連接的時間可能比此超時間隔長。 建議的值是值的`KeepAliveInterval`兩倍。|
| `HandshakeTimeout` | 15 秒 | 如果用戶端未在此時間間隔內發送初始握手消息,則連接將關閉。 這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。 有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。 |
| `KeepAliveInterval` | 15 秒 | 如果伺服器未在此間隔內發送消息,則會自動發送 ping 消息以保持連接打開狀態。 更改`KeepAliveInterval`時,`ServerTimeout`/`serverTimeoutInMilliseconds`更改 用戶端上的設置。 建議的值是值的`KeepAliveInterval`兩倍。`serverTimeoutInMilliseconds` `ServerTimeout` /  |
| `SupportedProtocols` | 所有已安裝的協定 | 此中心支持的協定。 默認情況下,允許在伺服器上註冊的所有協定,但可以從此清單中刪除協定以禁用各個集線器的特定協定。 |
| `EnableDetailedErrors` | `false` | 如果在`true`Hub 方法中引發異常時,將返回到用戶端的詳細異常消息。 默認值為`false`,因為這些異常消息可能包含敏感資訊。 |
| `StreamBufferCapacity` | `10` | 可緩衝用戶端上載流的最大項數。 如果達到此限制,則在伺服器處理流項之前將阻止調用的處理。|
| `MaximumReceiveMessageSize` | 32 KB | 單個傳入中心消息的最大大小。 |

可以通過在`AddSignalR``Startup.ConfigureServices`中 提供委託給調用的選項,為所有集線器配置選項。

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

單一組集線器的選項覆`AddSignalR`寫 中 提供的全域選項,並且<xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>可以使用 設定 :

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a>進階 HTTP 設定選項

用於`HttpConnectionDispatcherOptions`配置與傳輸和記憶體緩衝區管理相關的高級設置。 這些選項通過將委託傳遞給[MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub)在`Startup.Configure`中配置。

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<MyHub>("/myhub", options =>
        {
            options.Transports =
                HttpTransportType.WebSockets |
                HttpTransportType.LongPolling;
        });
    });
}
```

下表描述了用於設定ASP.NET核心訊號R的進階 HTTP 選項的選項:

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | 32 KB | 在應用背壓之前,伺服器緩衝的用戶端接收的最大位元組數。 增加此值允許伺服器在不施加背壓的情況下更快地接收更大的消息,但會增加記憶體消耗。 |
| `AuthorizationData` | 數據自動從應用於中心`Authorize`類的屬性中收集。 | 用於確定用戶端是否有權連接到集線器的[I授權數據](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件的清單。 |
| `TransportMaxBufferSize` | 32 KB | 在觀察背壓之前,伺服器緩衝的應用發送的最大位元組數。 增加此值允許伺服器更快地緩衝較大的郵件,而無需等待回壓,但會增加記憶體消耗。 |
| `Transports` | 所有傳輸都已啟用。 | 位標記值枚舉,`HttpTransportType`這些值可以限制用戶端可用於連接的傳輸。 |
| `LongPolling` | 請參閱下列內容。 | 特定於長輪詢傳輸的其他選項。 |
| `WebSockets` | 請參閱下列內容。 | 特定於 WebSocket 傳輸的其他選項。 |

長輪詢傳輸具有可以使用`LongPolling`屬性 設定的其他選項:

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `PollTimeout` | 90 秒 | 伺服器在終止單個輪詢請求之前等待消息發送到用戶端的最大時間。 減小此值會導致用戶端更頻繁地發出新的輪詢請求。 |

WebSocket 傳輸具有可以使用`WebSockets`屬性 設定的其他選項:

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `CloseTimeout` | 5 秒 | 伺服器關閉後,如果用戶端未能在此時間間隔內關閉,連接將終止。 |
| `SubProtocolSelector` | `null` | 可用於將`Sec-WebSocket-Protocol`標頭設置為自定義值的委託。 委託接收用戶端請求的值作為輸入,並預期返回所需的值。 |

## <a name="configure-client-options"></a>設定客戶端選項

可以在類型上`HubConnectionBuilder`配置客戶端選項(在 .NET 和 JavaScript 用戶端中可用)。 它在 Java 用戶端中也可用`HttpHubConnectionBuilder`,但子類包含生成器配置`HubConnection`選項以及 本身。

### <a name="configure-logging"></a>設定記錄

使用`ConfigureLogging`方法在 .NET 用戶端中配置日誌記錄。 日誌記錄提供者和篩選器的註冊方式與在伺服器上的註冊方式相同。 有關詳細資訊[,請參閱登錄ASP.NET核心](xref:fundamentals/logging/index)文檔。

> [!NOTE]
> 要註冊日誌記錄提供程式,必須安裝必要的包。 有關完整清單,請參閱文檔的[內建記錄提供程式](xref:fundamentals/logging/index#built-in-logging-providers)部分。

例如,要啟用主控台日誌記錄`Microsoft.Extensions.Logging.Console`, 請安裝 NuGet 套件。 呼叫`AddConsole`式副用:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

在 JavaScript 用戶端`configureLogging`中, 存在類似的方法。 提供指示`LogLevel`要生產的日誌消息的最小級別的值。 日誌將寫入瀏覽器控制台視窗。

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

還可以提供表示`LogLevel`日誌級別`string`名稱的值,而不是值。 這在無法存`LogLevel`取 常量的環境中配置 SignalR 日誌記錄時非常有用。

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

下表列出了可用的日誌級別。 您提供的值來`configureLogging`設置將記錄的**最小**紀錄級別。 將紀錄在此等級紀錄的訊息,**或表中列出的訊息等級**。

| String                      | LogLevel               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| `info`**或**`information` | `LogLevel.Information` |
| `warn`**或**`warning`     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

> [!NOTE]
> 要完全禁用日誌記錄,請在`signalR.LogLevel.None``configureLogging`方法中指定。

關於紀錄紀錄的詳細資訊,請參考[訊號的文件](xref:signalr/diagnostics)。

SignalR Java 用戶端使用[SLF4J](https://www.slf4j.org/)庫進行日誌記錄。 它是一種高級日誌記錄 API,允許庫的用戶通過引入特定的日誌記錄依賴項來選擇自己的特定日誌記錄實現。 以下代碼段演示如何與 SignalR `java.util.logging` Java 用戶端一起使用。

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

如果不在依賴項中設定日誌記錄,SLF4J 將載入具有以下警告訊息的預設無操作記錄器:

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

這可以安全地忽略。

### <a name="configure-allowed-transports"></a>設定允許的傳輸

SignalR 使用的傳輸`WithUrl`可以在 調用`withUrl`中( 在 JavaScript 中)進行配置。 值`HttpTransportType`的位 OR 可用於限制用戶端僅使用指定的傳輸。 默認情況下,所有傳輸都已啟用。

例如,要禁用伺服器發送事件傳輸,但允許 WebSocket 和長輪詢連接:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

在 JavaScript 用戶端中,透過設定提供給`transport`的選項 物件上的`withUrl`欄位來 設定傳輸:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

在此版本中,JAva 用戶端 Websocket 是唯一可用的傳輸。

在 Java 用戶端中,`withTransport`使用上的方法選擇`HttpHubConnectionBuilder`傳輸。 Java 用戶端預設使用 WebSocket 傳輸。

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> SignalR Java 用戶端還不支援傳輸回退。

### <a name="configure-bearer-authentication"></a>設定無記名身份驗證

要提供身份驗證數據以及 SignalR 請求,`AccessTokenProvider`請`accessTokenFactory`使用 選項 (在 JavaScript 中)指定傳回所需存取權杖的函數。 在 .NET 用戶端中,此訪問權杖作為 HTTP"承載身份驗證"令牌傳`Authorization`入(`Bearer`使用具有的標頭的類型)。 在 JavaScript 用戶端中,存取權杖用作承載權杖,**除非**瀏覽器 API 限制應用標頭的能力(特別是在伺服器發送事件和 WebSocket 請求中)。 在這些情況下,訪問權杖作為查詢字串值`access_token`提供。

在 .NET`AccessTokenProvider`用戶端中 ,`WithUrl`可以使用 中的選項委託指定該選項:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

在 JavaScript 用戶端中,通過`accessTokenFactory``withUrl`設置 中的選項物件上的欄位來設定訪問權杖。

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

在 SignalR Java 用戶端中,可以通過向[HttpHubConnectBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)提供訪問權杖工廠來配置承載權杖以用於身份驗證。 [與AccessTokenFactory一起](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJAVA](https://github.com/ReactiveX/RxJava)[單\<字串>。](https://reactivex.io/documentation/single.html) 調用[Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-),可以編寫邏輯來生成客戶端的訪問權杖。

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a>設定逾時與保持作用選項

`HubConnection`用於設定逾時與保持作用行為的其他選項在物件本身上可用:

# <a name="net"></a>[.NET](#tab/dotnet)

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `ServerTimeout` | 30 秒(30,000 毫秒) | 伺服器活動的超時。 如果伺服器未在此間隔內發送消息,則客戶端將認為伺服器已斷開連接並觸`Closed`發`onclose`事件( 在 JAvaScript 中)。 此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。 建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。 |
| `HandshakeTimeout` | 15 秒 | 初始伺服器握手的超時。 如果伺服器未在此間隔內發送握手回應,用戶端將取消握手並觸發`Closed`事件`onclose`( 在 JavaScript 中)。 這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。 有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。 |
| `KeepAliveInterval` | 15 秒 | 確定客戶端發送 ping 消息的時間間隔。 從用戶端發送任何消息會將計時器重置為間隔的開始。 如果用戶端未在伺服器上的`ClientTimeoutInterval`集中發送消息,則伺服器將認為客戶端已斷開連接。 |

在 .NET 用戶端中,超時值`TimeSpan`指定為值。

# <a name="javascript"></a>[JavaScript](#tab/javascript)

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | 30 秒(30,000 毫秒) | 伺服器活動的超時。 如果伺服器未在此間隔內發送消息,則客戶端認為伺服器已斷開連接並觸`onclose`發 事件。 此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。 建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。 |
| `keepAliveIntervalInMilliseconds` | 15 秒(15,000 毫秒) | 確定客戶端發送 ping 消息的時間間隔。 從用戶端發送任何消息會將計時器重置為間隔的開始。 如果用戶端未在伺服器上的`ClientTimeoutInterval`集中發送消息,則伺服器將認為客戶端已斷開連接。 |

# <a name="java"></a>[Java](#tab/java)

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `getServerTimeout` / `setServerTimeout` | 30 秒(30,000 毫秒) | 伺服器活動的超時。 如果伺服器未在此間隔內發送消息,則客戶端認為伺服器已斷開連接並觸`onClose`發 事件。 此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。 建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。 |
| `withHandshakeResponseTimeout` | 15 秒 | 初始伺服器握手的超時。 如果伺服器未在此間隔內發送握手回應,用戶端將取消握手並觸發`onClose`事件。 這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。 有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。 |
| `getKeepAliveInterval` / `setKeepAliveInterval` | 15 秒(15,000 毫秒) | 確定客戶端發送 ping 消息的時間間隔。 從用戶端發送任何消息會將計時器重置為間隔的開始。 如果用戶端未在伺服器上的`ClientTimeoutInterval`集中發送消息,則伺服器將認為客戶端已斷開連接。 |

---

### <a name="configure-additional-options"></a>設定其他選項

其他選項可以在 Java`WithUrl``withUrl`用戶端 上或`HubConnectionBuilder`JAva 用戶端上 的各種設定 API 上`HttpHubConnectionBuilder`(JAvaScript)方法 中設定:

# <a name="net"></a>[.NET](#tab/dotnet)

| .NET 選項 |  預設值 | 描述 |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | 返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。 |
| `SkipNegotiation` | `false` | 將此`true`設置為跳過協商步驟。 **只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。 使用 Azure SignalR 服務時無法啟用此設定。 |
| `ClientCertificates` | 空白 | 要發送到身份驗證請求的 TLS 證書的集合。 |
| `Cookies` | 空白 | 要隨每個 HTTP 請求一起發送的 HTTP Cookie 集合。 |
| `Credentials` | 空白 | 要隨每個 HTTP 請求一起發送的認證。 |
| `CloseTimeout` | 5 秒 | 僅限 Web 插座。 用戶端關閉後等待伺服器確認關閉請求的最大時間量。 如果伺服器在此時間內未確認關閉,則客戶端將斷開連接。 |
| `Headers` | 空白 | 要隨每個 HTTP 請求一起發送的其他 HTTP 標頭的映射。 |
| `HttpMessageHandlerFactory` | `null` | 可用於配置或替換`HttpMessageHandler`用於發送 HTTP 請求的委託。 不用於 Web 套接字連接。 此委託必須返回一個非空值,並且它將作為參數接收預設值。 修改該預設值上的設置並返回該設置,或返回新`HttpMessageHandler`實例。 **替換處理程式時,請確保從提供的處理程式複製要保留的設置,否則,配置的選項(如 Cookie 和標頭)將不適用於新處理程式。** |
| `Proxy` | `null` | 傳送 HTTP 請求時要使用的 HTTP 代理。 |
| `UseDefaultCredentials` | `false` | 設置此布爾以發送 HTTP 和 WebSocket 請求的預設認證。 這支援使用 Windows 身份驗證。 |
| `WebSocketConfiguration` | `null` | 可用於配置其他 WebSocket 選項的委託。 接收可用於配置選項的[用戶端WebSocket選項](/dotnet/api/system.net.websockets.clientwebsocketoptions)的實例。 |

# <a name="javascript"></a>[JavaScript](#tab/javascript)

| JavaScript 選項 | 預設值 | 描述 |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | 返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。 |
| `skipNegotiation` | `false` | 將此`true`設置為跳過協商步驟。 **只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。 使用 Azure SignalR 服務時無法啟用此設定。 |

# <a name="java"></a>[Java](#tab/java)

| Java 選項 | 預設值 | 描述 |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | 返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。 |
| `shouldSkipNegotiate` | `false` | 將此`true`設置為跳過協商步驟。 **只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。 使用 Azure SignalR 服務時無法啟用此設定。 |
| `withHeader` `withHeaders` | 空白 | 要隨每個 HTTP 請求一起發送的其他 HTTP 標頭的映射。 |

---

在 .NET 用戶端中,這些選項可以`WithUrl`通過提供給 : 提供的選項委託進行修改:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

在 JavaScript 用戶端中,這些選項可以在`withUrl`提供給 的 JavaScript 物件中提供:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

在 Java 用戶端中,`HttpHubConnectionBuilder`可以使用 從`HubConnectionBuilder.create("HUB URL")`

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

::: moniker-end
::: moniker range="= aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a>JSON/訊息套件序列化選項

ASP.NET核心訊號R支援兩種用於編碼訊息的協定[:JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。 每個協定都有序列化配置選項。

JSON 序列化可以使用[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴展方法在伺服器上配置,該方法`Startup.ConfigureServices`可以在方法中的[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)之後添加。 該方法`AddJsonProtocol`採用`options`接收 物件的委託。 該物件的[Payload 序列化器設定](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)屬性`JsonSerializerSettings`是可用於 配置參數和返回值序列化 JSON.NET 物件。 有關詳細資訊,請參閱[JSON.NET 文檔](https://www.newtonsoft.com/json/help/html/Introduction.htm)。
 
例如,要將序列化程式配置為使用「PascalCase」屬性名稱,而不是預設的「camelCase」名稱,請使用中的`Startup.ConfigureServices`以下代碼:
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

在 .NET`AddJsonProtocol`用戶端中[,HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上存在相同的擴充方法。 必須`Microsoft.Extensions.DependencyInjection`匯入命名空間以解決延伸方法:

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
> 此時無法在 JavaScript 用戶端中設定 JSON 序列化。

### <a name="messagepack-serialization-options"></a>訊息套件序列化選項

消息包序列化可以通過提供對[AddMessagePack 協定](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)調用的委託來配置。 有關詳細資訊[,請參閱 SignalR 中的訊息套件](xref:signalr/messagepackhubprotocol)。

> [!NOTE]
> 此時無法在 JavaScript 用戶端中設定 MessagePack 序列化。

## <a name="configure-server-options"></a>設定伺服器選項

下表描述了用於設定 SignalR 集線器的選項:

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | 30 秒 | 如果用戶端未在此間隔內收到消息(包括保持活動狀態),伺服器將考慮用戶端斷開連接。 由於如何實現,實際將客戶端標記為斷開連接的時間可能比此超時間隔長。 建議的值是值的`KeepAliveInterval`兩倍。|
| `HandshakeTimeout` | 15 秒 | 如果用戶端未在此時間間隔內發送初始握手消息,則連接將關閉。 這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。 有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。 |
| `KeepAliveInterval` | 15 秒 | 如果伺服器未在此間隔內發送消息,則會自動發送 ping 消息以保持連接打開狀態。 更改`KeepAliveInterval`時,`ServerTimeout`/`serverTimeoutInMilliseconds`更改 用戶端上的設置。 建議的值是值的`KeepAliveInterval`兩倍。`serverTimeoutInMilliseconds` `ServerTimeout` /  |
| `SupportedProtocols` | 所有已安裝的協定 | 此中心支持的協定。 默認情況下,允許在伺服器上註冊的所有協定,但可以從此清單中刪除協定以禁用各個集線器的特定協定。 |
| `EnableDetailedErrors` | `false` | 如果在`true`Hub 方法中引發異常時,將返回到用戶端的詳細異常消息。 默認值為`false`,因為這些異常消息可能包含敏感資訊。 |

可以通過在`AddSignalR``Startup.ConfigureServices`中 提供委託給調用的選項,為所有集線器配置選項。

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

單一組集線器的選項覆`AddSignalR`寫 中 提供的全域選項,並且<xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>可以使用 設定 :

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a>進階 HTTP 設定選項

用於`HttpConnectionDispatcherOptions`配置與傳輸和記憶體緩衝區管理相關的高級設置。 這些選項通過將委託傳遞給[MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub)在`Startup.Configure`中配置。

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

下表描述了用於設定ASP.NET核心訊號R的進階 HTTP 選項的選項:

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | 32 KB | 從伺服器緩衝的用戶端接收的最大位元組數。 增加此值允許伺服器接收較大的消息,但可能會對記憶體消耗產生負面影響。 |
| `AuthorizationData` | 數據自動從應用於中心`Authorize`類的屬性中收集。 | 用於確定用戶端是否有權連接到集線器的[I授權數據](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件的清單。 |
| `TransportMaxBufferSize` | 32 KB | 伺服器緩衝的應用發送的最大位元組數。 增加此值允許伺服器發送更大的消息,但可能會對記憶體消耗產生負面影響。 |
| `Transports` | 所有傳輸都已啟用。 | 位標記值枚舉,`HttpTransportType`這些值可以限制用戶端可用於連接的傳輸。 |
| `LongPolling` | 請參閱下列內容。 | 特定於長輪詢傳輸的其他選項。 |
| `WebSockets` | 請參閱下列內容。 | 特定於 WebSocket 傳輸的其他選項。 |

長輪詢傳輸具有可以使用`LongPolling`屬性 設定的其他選項:

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `PollTimeout` | 90 秒 | 伺服器在終止單個輪詢請求之前等待消息發送到用戶端的最大時間。 減小此值會導致用戶端更頻繁地發出新的輪詢請求。 |

WebSocket 傳輸具有可以使用`WebSockets`屬性 設定的其他選項:

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `CloseTimeout` | 5 秒 | 伺服器關閉後,如果用戶端未能在此時間間隔內關閉,連接將終止。 |
| `SubProtocolSelector` | `null` | 可用於將`Sec-WebSocket-Protocol`標頭設置為自定義值的委託。 委託接收用戶端請求的值作為輸入,並預期返回所需的值。 |

## <a name="configure-client-options"></a>設定客戶端選項

可以在類型上`HubConnectionBuilder`配置客戶端選項(在 .NET 和 JavaScript 用戶端中可用)。 它在 Java 用戶端中也可用`HttpHubConnectionBuilder`,但子類包含生成器配置`HubConnection`選項以及 本身。

### <a name="configure-logging"></a>設定記錄

使用`ConfigureLogging`方法在 .NET 用戶端中配置日誌記錄。 日誌記錄提供者和篩選器的註冊方式與在伺服器上的註冊方式相同。 有關詳細資訊[,請參閱登錄ASP.NET核心](xref:fundamentals/logging/index)文檔。

> [!NOTE]
> 要註冊日誌記錄提供程式,必須安裝必要的包。 有關完整清單,請參閱文檔的[內建記錄提供程式](xref:fundamentals/logging/index#built-in-logging-providers)部分。

例如,要啟用主控台日誌記錄`Microsoft.Extensions.Logging.Console`, 請安裝 NuGet 套件。 呼叫`AddConsole`式副用:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

在 JavaScript 用戶端`configureLogging`中, 存在類似的方法。 提供指示`LogLevel`要生產的日誌消息的最小級別的值。 日誌將寫入瀏覽器控制台視窗。

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> 要完全禁用日誌記錄,請在`signalR.LogLevel.None``configureLogging`方法中指定。

關於紀錄紀錄的詳細資訊,請參考[訊號的文件](xref:signalr/diagnostics)。

SignalR Java 用戶端使用[SLF4J](https://www.slf4j.org/)庫進行日誌記錄。 它是一種高級日誌記錄 API,允許庫的用戶通過引入特定的日誌記錄依賴項來選擇自己的特定日誌記錄實現。 以下代碼段演示如何與 SignalR `java.util.logging` Java 用戶端一起使用。

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

如果不在依賴項中設定日誌記錄,SLF4J 將載入具有以下警告訊息的預設無操作記錄器:

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

這可以安全地忽略。

### <a name="configure-allowed-transports"></a>設定允許的傳輸

SignalR 使用的傳輸`WithUrl`可以在 調用`withUrl`中( 在 JavaScript 中)進行配置。 值`HttpTransportType`的位 OR 可用於限制用戶端僅使用指定的傳輸。 默認情況下,所有傳輸都已啟用。

例如,要禁用伺服器發送事件傳輸,但允許 WebSocket 和長輪詢連接:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

在 JavaScript 用戶端中,透過設定提供給`transport`的選項 物件上的`withUrl`欄位來 設定傳輸:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

在此版本中,JAva 用戶端 Websocket 是唯一可用的傳輸。

### <a name="configure-bearer-authentication"></a>設定無記名身份驗證

要提供身份驗證數據以及 SignalR 請求,`AccessTokenProvider`請`accessTokenFactory`使用 選項 (在 JavaScript 中)指定傳回所需存取權杖的函數。 在 .NET 用戶端中,此訪問權杖作為 HTTP"承載身份驗證"令牌傳`Authorization`入(`Bearer`使用具有的標頭的類型)。 在 JavaScript 用戶端中,存取權杖用作承載權杖,**除非**瀏覽器 API 限制應用標頭的能力(特別是在伺服器發送事件和 WebSocket 請求中)。 在這些情況下,訪問權杖作為查詢字串值`access_token`提供。

在 .NET`AccessTokenProvider`用戶端中 ,`WithUrl`可以使用 中的選項委託指定該選項:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

在 JavaScript 用戶端中,通過`accessTokenFactory``withUrl`設置 中的選項物件上的欄位來設定訪問權杖。

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

在 SignalR Java 用戶端中,可以通過向[HttpHubConnectBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)提供訪問權杖工廠來配置承載權杖以用於身份驗證。 [與AccessTokenFactory一起](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJAVA](https://github.com/ReactiveX/RxJava)[單\<字串>。](https://reactivex.io/documentation/single.html) 調用[Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-),可以編寫邏輯來生成客戶端的訪問權杖。

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a>設定逾時與保持作用選項

`HubConnection`用於設定逾時與保持作用行為的其他選項在物件本身上可用:

# <a name="net"></a>[.NET](#tab/dotnet)

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `ServerTimeout` | 30 秒(30,000 毫秒) | 伺服器活動的超時。 如果伺服器未在此間隔內發送消息,則客戶端將認為伺服器已斷開連接並觸`Closed`發`onclose`事件( 在 JAvaScript 中)。 此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。 建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。 |
| `HandshakeTimeout` | 15 秒 | 初始伺服器握手的超時。 如果伺服器未在此間隔內發送握手回應,用戶端將取消握手並觸發`Closed`事件`onclose`( 在 JavaScript 中)。 這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。 有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。 |
| `KeepAliveInterval` | 15 秒 | 確定客戶端發送 ping 消息的時間間隔。 從用戶端發送任何消息會將計時器重置為間隔的開始。 如果用戶端未在伺服器上的`ClientTimeoutInterval`集中發送消息,則伺服器將認為客戶端已斷開連接。 |

在 .NET 用戶端中,超時值`TimeSpan`指定為值。

# <a name="javascript"></a>[JavaScript](#tab/javascript)

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | 30 秒(30,000 毫秒) | 伺服器活動的超時。 如果伺服器未在此間隔內發送消息,則客戶端認為伺服器已斷開連接並觸`onclose`發 事件。 此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。 建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。 |
| `keepAliveIntervalInMilliseconds` | 15 秒(15,000 毫秒) | 確定客戶端發送 ping 消息的時間間隔。 從用戶端發送任何消息會將計時器重置為間隔的開始。 如果用戶端未在伺服器上的`ClientTimeoutInterval`集中發送消息,則伺服器將認為客戶端已斷開連接。 |

# <a name="java"></a>[Java](#tab/java)

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `getServerTimeout` / `setServerTimeout` | 30 秒(30,000 毫秒) | 伺服器活動的超時。 如果伺服器未在此間隔內發送消息,則客戶端認為伺服器已斷開連接並觸`onClose`發 事件。 此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。 建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。 |
| `withHandshakeResponseTimeout` | 15 秒 | 初始伺服器握手的超時。 如果伺服器未在此間隔內發送握手回應,用戶端將取消握手並觸發`onClose`事件。 這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。 有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。 |
| `getKeepAliveInterval` / `setKeepAliveInterval` | 15 秒(15,000 毫秒) | 確定客戶端發送 ping 消息的時間間隔。 從用戶端發送任何消息會將計時器重置為間隔的開始。 如果用戶端未在伺服器上的`ClientTimeoutInterval`集中發送消息,則伺服器將認為客戶端已斷開連接。 |

---

### <a name="configure-additional-options"></a>設定其他選項

其他選項可以在 Java`WithUrl``withUrl`用戶端 上或`HubConnectionBuilder`JAva 用戶端上 的各種設定 API 上`HttpHubConnectionBuilder`(JAvaScript)方法 中設定:

# <a name="net"></a>[.NET](#tab/dotnet)

| .NET 選項 |  預設值 | 描述 |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | 返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。 |
| `SkipNegotiation` | `false` | 將此`true`設置為跳過協商步驟。 **只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。 使用 Azure SignalR 服務時無法啟用此設定。 |
| `ClientCertificates` | 空白 | 要發送到身份驗證請求的 TLS 證書的集合。 |
| `Cookies` | 空白 | 要隨每個 HTTP 請求一起發送的 HTTP Cookie 集合。 |
| `Credentials` | 空白 | 要隨每個 HTTP 請求一起發送的認證。 |
| `CloseTimeout` | 5 秒 | 僅限 Web 插座。 用戶端關閉後等待伺服器確認關閉請求的最大時間量。 如果伺服器在此時間內未確認關閉,則客戶端將斷開連接。 |
| `Headers` | 空白 | 要隨每個 HTTP 請求一起發送的其他 HTTP 標頭的映射。 |
| `HttpMessageHandlerFactory` | `null` | 可用於配置或替換`HttpMessageHandler`用於發送 HTTP 請求的委託。 不用於 Web 套接字連接。 此委託必須返回一個非空值,並且它將作為參數接收預設值。 修改該預設值上的設置並返回該設置,或返回新`HttpMessageHandler`實例。 **替換處理程式時,請確保從提供的處理程式複製要保留的設置,否則,配置的選項(如 Cookie 和標頭)將不適用於新處理程式。** |
| `Proxy` | `null` | 傳送 HTTP 請求時要使用的 HTTP 代理。 |
| `UseDefaultCredentials` | `false` | 設置此布爾以發送 HTTP 和 WebSocket 請求的預設認證。 這支援使用 Windows 身份驗證。 |
| `WebSocketConfiguration` | `null` | 可用於配置其他 WebSocket 選項的委託。 接收可用於配置選項的[用戶端WebSocket選項](/dotnet/api/system.net.websockets.clientwebsocketoptions)的實例。 |

# <a name="javascript"></a>[JavaScript](#tab/javascript)

| JavaScript 選項 | 預設值 | 描述 |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | 返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。 |
| `skipNegotiation` | `false` | 將此`true`設置為跳過協商步驟。 **只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。 使用 Azure SignalR 服務時無法啟用此設定。 |

# <a name="java"></a>[Java](#tab/java)

| Java 選項 | 預設值 | 描述 |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | 返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。 |
| `shouldSkipNegotiate` | `false` | 將此`true`設置為跳過協商步驟。 **只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。 使用 Azure SignalR 服務時無法啟用此設定。 |
| `withHeader` `withHeaders` | 空白 | 要隨每個 HTTP 請求一起發送的其他 HTTP 標頭的映射。 |

---

在 .NET 用戶端中,這些選項可以`WithUrl`通過提供給 : 提供的選項委託進行修改:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

在 JavaScript 用戶端中,這些選項可以在`withUrl`提供給 的 JavaScript 物件中提供:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

在 Java 用戶端中,`HttpHubConnectionBuilder`可以使用 從`HubConnectionBuilder.create("HUB URL")`

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

::: moniker-end
::: moniker range="< aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a>JSON/訊息套件序列化選項

ASP.NET核心訊號R支援兩種用於編碼訊息的協定[:JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。 每個協定都有序列化配置選項。

JSON 序列化可以使用[AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴展方法在伺服器上配置,該方法`Startup.ConfigureServices`可以在方法中的[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)之後添加。 該方法`AddJsonProtocol`採用`options`接收 物件的委託。 該物件的[Payload 序列化器設定](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)屬性`JsonSerializerSettings`是可用於 配置參數和返回值序列化 JSON.NET 物件。 有關詳細資訊,請參閱[JSON.NET 文檔](https://www.newtonsoft.com/json/help/html/Introduction.htm)。
 
例如,要將序列化程式配置為使用「PascalCase」屬性名稱,而不是預設的「camelCase」名稱,請使用中的`Startup.ConfigureServices`以下代碼:
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

在 .NET`AddJsonProtocol`用戶端中[,HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)上存在相同的擴充方法。 必須`Microsoft.Extensions.DependencyInjection`匯入命名空間以解決延伸方法:

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
> 此時無法在 JavaScript 用戶端中設定 JSON 序列化。

### <a name="messagepack-serialization-options"></a>訊息套件序列化選項

消息包序列化可以通過提供對[AddMessagePack 協定](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)調用的委託來配置。 有關詳細資訊[,請參閱 SignalR 中的訊息套件](xref:signalr/messagepackhubprotocol)。

> [!NOTE]
> 此時無法在 JavaScript 用戶端中設定 MessagePack 序列化。

## <a name="configure-server-options"></a>設定伺服器選項

下表描述了用於設定 SignalR 集線器的選項:

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | 15 秒 | 如果用戶端未在此時間間隔內發送初始握手消息,則連接將關閉。 這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。 有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。 |
| `KeepAliveInterval` | 15 秒 | 如果伺服器未在此間隔內發送消息,則會自動發送 ping 消息以保持連接打開狀態。 更改`KeepAliveInterval`時,`ServerTimeout`/`serverTimeoutInMilliseconds`更改 用戶端上的設置。 建議的值是值的`KeepAliveInterval`兩倍。`serverTimeoutInMilliseconds` `ServerTimeout` /  |
| `SupportedProtocols` | 所有已安裝的協定 | 此中心支持的協定。 默認情況下,允許在伺服器上註冊的所有協定,但可以從此清單中刪除協定以禁用各個集線器的特定協定。 |
| `EnableDetailedErrors` | `false` | 如果在`true`Hub 方法中引發異常時,將返回到用戶端的詳細異常消息。 默認值為`false`,因為這些異常消息可能包含敏感資訊。 |

可以通過在`AddSignalR``Startup.ConfigureServices`中 提供委託給調用的選項,為所有集線器配置選項。

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

單一組集線器的選項覆`AddSignalR`寫 中 提供的全域選項,並且<xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>可以使用 設定 :

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a>進階 HTTP 設定選項

用於`HttpConnectionDispatcherOptions`配置與傳輸和記憶體緩衝區管理相關的高級設置。 這些選項通過將委託傳遞給[MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub)在`Startup.Configure`中配置。

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

下表描述了用於設定ASP.NET核心訊號R的進階 HTTP 選項的選項:

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | 32 KB | 從伺服器緩衝的用戶端接收的最大位元組數。 增加此值允許伺服器接收較大的消息,但可能會對記憶體消耗產生負面影響。 |
| `AuthorizationData` | 數據自動從應用於中心`Authorize`類的屬性中收集。 | 用於確定用戶端是否有權連接到集線器的[I授權數據](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)物件的清單。 |
| `TransportMaxBufferSize` | 32 KB | 伺服器緩衝的應用發送的最大位元組數。 增加此值允許伺服器發送更大的消息,但可能會對記憶體消耗產生負面影響。 |
| `Transports` | 所有傳輸都已啟用。 | 位標記值枚舉,`HttpTransportType`這些值可以限制用戶端可用於連接的傳輸。 |
| `LongPolling` | 請參閱下列內容。 | 特定於長輪詢傳輸的其他選項。 |
| `WebSockets` | 請參閱下列內容。 | 特定於 WebSocket 傳輸的其他選項。 |

長輪詢傳輸具有可以使用`LongPolling`屬性 設定的其他選項:

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `PollTimeout` | 90 秒 | 伺服器在終止單個輪詢請求之前等待消息發送到用戶端的最大時間。 減小此值會導致用戶端更頻繁地發出新的輪詢請求。 |

WebSocket 傳輸具有可以使用`WebSockets`屬性 設定的其他選項:

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `CloseTimeout` | 5 秒 | 伺服器關閉後,如果用戶端未能在此時間間隔內關閉,連接將終止。 |
| `SubProtocolSelector` | `null` | 可用於將`Sec-WebSocket-Protocol`標頭設置為自定義值的委託。 委託接收用戶端請求的值作為輸入,並預期返回所需的值。 |

## <a name="configure-client-options"></a>設定客戶端選項

可以在類型上`HubConnectionBuilder`配置客戶端選項(在 .NET 和 JavaScript 用戶端中可用)。 它在 Java 用戶端中也可用`HttpHubConnectionBuilder`,但子類包含生成器配置`HubConnection`選項以及 本身。

### <a name="configure-logging"></a>設定記錄

使用`ConfigureLogging`方法在 .NET 用戶端中配置日誌記錄。 日誌記錄提供者和篩選器的註冊方式與在伺服器上的註冊方式相同。 有關詳細資訊[,請參閱登錄ASP.NET核心](xref:fundamentals/logging/index)文檔。

> [!NOTE]
> 要註冊日誌記錄提供程式,必須安裝必要的包。 有關完整清單,請參閱文檔的[內建記錄提供程式](xref:fundamentals/logging/index#built-in-logging-providers)部分。

例如,要啟用主控台日誌記錄`Microsoft.Extensions.Logging.Console`, 請安裝 NuGet 套件。 呼叫`AddConsole`式副用:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

在 JavaScript 用戶端`configureLogging`中, 存在類似的方法。 提供指示`LogLevel`要生產的日誌消息的最小級別的值。 日誌將寫入瀏覽器控制台視窗。

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> 要完全禁用日誌記錄,請在`signalR.LogLevel.None``configureLogging`方法中指定。

關於紀錄紀錄的詳細資訊,請參考[訊號的文件](xref:signalr/diagnostics)。

SignalR Java 用戶端使用[SLF4J](https://www.slf4j.org/)庫進行日誌記錄。 它是一種高級日誌記錄 API,允許庫的用戶通過引入特定的日誌記錄依賴項來選擇自己的特定日誌記錄實現。 以下代碼段演示如何與 SignalR `java.util.logging` Java 用戶端一起使用。

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

如果不在依賴項中設定日誌記錄,SLF4J 將載入具有以下警告訊息的預設無操作記錄器:

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

這可以安全地忽略。

### <a name="configure-allowed-transports"></a>設定允許的傳輸

SignalR 使用的傳輸`WithUrl`可以在 調用`withUrl`中( 在 JavaScript 中)進行配置。 值`HttpTransportType`的位 OR 可用於限制用戶端僅使用指定的傳輸。 默認情況下,所有傳輸都已啟用。

例如,要禁用伺服器發送事件傳輸,但允許 WebSocket 和長輪詢連接:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

在 JavaScript 用戶端中,透過設定提供給`transport`的選項 物件上的`withUrl`欄位來 設定傳輸:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a>設定無記名身份驗證

要提供身份驗證數據以及 SignalR 請求,`AccessTokenProvider`請`accessTokenFactory`使用 選項 (在 JavaScript 中)指定傳回所需存取權杖的函數。 在 .NET 用戶端中,此訪問權杖作為 HTTP"承載身份驗證"令牌傳`Authorization`入(`Bearer`使用具有的標頭的類型)。 在 JavaScript 用戶端中,存取權杖用作承載權杖,**除非**瀏覽器 API 限制應用標頭的能力(特別是在伺服器發送事件和 WebSocket 請求中)。 在這些情況下,訪問權杖作為查詢字串值`access_token`提供。

在 .NET`AccessTokenProvider`用戶端中 ,`WithUrl`可以使用 中的選項委託指定該選項:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

在 JavaScript 用戶端中,通過`accessTokenFactory``withUrl`設置 中的選項物件上的欄位來設定訪問權杖。

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

在 SignalR Java 用戶端中,可以通過向[HttpHubConnectBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)提供訪問權杖工廠來配置承載權杖以用於身份驗證。 [與AccessTokenFactory一起](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__)提供[RxJAVA](https://github.com/ReactiveX/RxJava)[單\<字串>。](https://reactivex.io/documentation/single.html) 調用[Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-),可以編寫邏輯來生成客戶端的訪問權杖。

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a>設定逾時與保持作用選項

`HubConnection`用於設定逾時與保持作用行為的其他選項在物件本身上可用:

# <a name="net"></a>[.NET](#tab/dotnet)

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `ServerTimeout` | 30 秒(30,000 毫秒) | 伺服器活動的超時。 如果伺服器未在此間隔內發送消息,則客戶端將認為伺服器已斷開連接並觸`Closed`發`onclose`事件( 在 JAvaScript 中)。 此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。 建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。 |
| `HandshakeTimeout` | 15 秒 | 初始伺服器握手的超時。 如果伺服器未在此間隔內發送握手回應,用戶端將取消握手並觸發`Closed`事件`onclose`( 在 JavaScript 中)。 這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。 有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。 |

在 .NET 用戶端中,超時值`TimeSpan`指定為值。

# <a name="javascript"></a>[JavaScript](#tab/javascript)

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | 30 秒(30,000 毫秒) | 伺服器活動的超時。 如果伺服器未在此間隔內發送消息,則客戶端認為伺服器已斷開連接並觸`onclose`發 事件。 此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。 建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。 |

# <a name="java"></a>[Java](#tab/java)

| 選項 | 預設值 | 描述 |
| ------ | ------------- | ----------- |
| `getServerTimeout` / `setServerTimeout` | 30 秒(30,000 毫秒) | 伺服器活動的超時。 如果伺服器未在此間隔內發送消息,則客戶端認為伺服器已斷開連接並觸`onClose`發 事件。 此值必須足夠大,以便**從伺服器發送**ping 消息並由用戶端在超時間隔內接收。 建議的值是一個數位,至少比伺服器`KeepAliveInterval`的值翻倍,以便 ping 有時間到達。 |
| `withHandshakeResponseTimeout` | 15 秒 | 初始伺服器握手的超時。 如果伺服器未在此間隔內發送握手回應,用戶端將取消握手並觸發`onClose`事件。 這是一個高級設置,只有在由於嚴重的網路延遲而發生握手超時錯誤時,才應修改該設置。 有關握手過程的更多詳細資訊,請參閱[SignalR 集線器協定規範](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)。 |

---

### <a name="configure-additional-options"></a>設定其他選項

其他選項可以在 Java`WithUrl``withUrl`用戶端 上或`HubConnectionBuilder`JAva 用戶端上 的各種設定 API 上`HttpHubConnectionBuilder`(JAvaScript)方法 中設定:

# <a name="net"></a>[.NET](#tab/dotnet)

| .NET 選項 |  預設值 | 描述 |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | 返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。 |
| `SkipNegotiation` | `false` | 將此`true`設置為跳過協商步驟。 **只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。 使用 Azure SignalR 服務時無法啟用此設定。 |
| `ClientCertificates` | 空白 | 要發送到身份驗證請求的 TLS 證書的集合。 |
| `Cookies` | 空白 | 要隨每個 HTTP 請求一起發送的 HTTP Cookie 集合。 |
| `Credentials` | 空白 | 要隨每個 HTTP 請求一起發送的認證。 |
| `CloseTimeout` | 5 秒 | 僅限 Web 插座。 用戶端關閉後等待伺服器確認關閉請求的最大時間量。 如果伺服器在此時間內未確認關閉,則客戶端將斷開連接。 |
| `Headers` | 空白 | 要隨每個 HTTP 請求一起發送的其他 HTTP 標頭的映射。 |
| `HttpMessageHandlerFactory` | `null` | 可用於配置或替換`HttpMessageHandler`用於發送 HTTP 請求的委託。 不用於 Web 套接字連接。 此委託必須返回一個非空值,並且它將作為參數接收預設值。 修改該預設值上的設置並返回該設置,或返回新`HttpMessageHandler`實例。 **替換處理程式時,請確保從提供的處理程式複製要保留的設置,否則,配置的選項(如 Cookie 和標頭)將不適用於新處理程式。** |
| `Proxy` | `null` | 傳送 HTTP 請求時要使用的 HTTP 代理。 |
| `UseDefaultCredentials` | `false` | 設置此布爾以發送 HTTP 和 WebSocket 請求的預設認證。 這支援使用 Windows 身份驗證。 |
| `WebSocketConfiguration` | `null` | 可用於配置其他 WebSocket 選項的委託。 接收可用於配置選項的[用戶端WebSocket選項](/dotnet/api/system.net.websockets.clientwebsocketoptions)的實例。 |

# <a name="javascript"></a>[JavaScript](#tab/javascript)

| JavaScript 選項 | 預設值 | 描述 |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | 返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。 |
| `skipNegotiation` | `false` | 將此`true`設置為跳過協商步驟。 **只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。 使用 Azure SignalR 服務時無法啟用此設定。 |

# <a name="java"></a>[Java](#tab/java)

| Java 選項 | 預設值 | 描述 |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | 返回在 HTTP 請求中作為承載身份驗證權杖提供的字串的函數。 |
| `shouldSkipNegotiate` | `false` | 將此`true`設置為跳過協商步驟。 **只使用 WebSocket 傳輸為唯一啟用的傳輸時才受支援**。 使用 Azure SignalR 服務時無法啟用此設定。 |
| `withHeader` `withHeaders` | 空白 | 要隨每個 HTTP 請求一起發送的其他 HTTP 標頭的映射。 |

---

在 .NET 用戶端中,這些選項可以`WithUrl`通過提供給 : 提供的選項委託進行修改:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

在 JavaScript 用戶端中,這些選項可以在`withUrl`提供給 的 JavaScript 物件中提供:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

在 Java 用戶端中,`HttpHubConnectionBuilder`可以使用 從`HubConnectionBuilder.create("HUB URL")`

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

::: moniker-end
