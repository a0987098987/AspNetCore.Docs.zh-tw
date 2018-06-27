---
title: ASP.NET Core SignalR 組態
author: rachelappel
description: 了解如何設定 ASP.NET Core SignalR 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/30/2018
uid: signalr/configuration
ms.openlocfilehash: 167b8828efd093d755aeb1c91e080dbd22b5d485
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961978"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="050d9-103">ASP.NET Core SignalR 組態</span><span class="sxs-lookup"><span data-stu-id="050d9-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="050d9-104">JSON/MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="050d9-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="050d9-105">ASP.NET Core SignalR 支援兩種通訊協定，針對訊息進行編碼： [JSON](https://www.json.org/)和[MessagePack](https://msgpack.org/index.html)。</span><span class="sxs-lookup"><span data-stu-id="050d9-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="050d9-106">每個通訊協定的序列化組態選項。</span><span class="sxs-lookup"><span data-stu-id="050d9-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="050d9-107">JSON 序列化可以設定伺服器使用[ `AddJsonProtocol` ](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)擴充方法，可以在之後加入[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)中您`Startup.ConfigureServices`方法。</span><span class="sxs-lookup"><span data-stu-id="050d9-107">JSON serialization can be configured on the server using the [`AddJsonProtocol`](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="050d9-108">`AddJsonProtocol`方法接受委派，會收到`options`物件。</span><span class="sxs-lookup"><span data-stu-id="050d9-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="050d9-109">[ `PayloadSerializerSettings` ](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)上該物件的屬性是 JSON.NET`JsonSerializerSettings`可用來設定序列化的引數和傳回值的物件。</span><span class="sxs-lookup"><span data-stu-id="050d9-109">The [`PayloadSerializerSettings`](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="050d9-110">請參閱[JSON.NET 文件](https://www.newtonsoft.com/json/help/html/Introduction.htm)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="050d9-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="050d9-111">例如，若要設定的序列化程式，而不是預設的 「 camelCase 」 名稱，使用"PascalCase"屬性的名稱，使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="050d9-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="050d9-112">在.NET 用戶端中，相同`AddJsonHubProtocol`擴充方法位於[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)。</span><span class="sxs-lookup"><span data-stu-id="050d9-112">In the .NET client, the same `AddJsonHubProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="050d9-113">`Microsoft.Extensions.DependencyInjection`解析擴充方法必須匯入命名空間：</span><span class="sxs-lookup"><span data-stu-id="050d9-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
.AddJsonHubProtocol(options => {
    options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
});
```

> [!NOTE]
> <span data-ttu-id="050d9-114">您不可能在這個階段設定中的 JavaScript 用戶端的 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="050d9-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="050d9-115">MessagePack 序列化選項</span><span class="sxs-lookup"><span data-stu-id="050d9-115">MessagePack serialization options</span></span>

<span data-ttu-id="050d9-116">您可以設定 MessagePack 序列化提供的委派[AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼叫。</span><span class="sxs-lookup"><span data-stu-id="050d9-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="050d9-117">請參閱[SignalR 中的 MessagePack](xref:signalr/messagepackhubprotocol)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="050d9-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="050d9-118">您不可以設定 MessagePack 序列化設定中的 JavaScript 用戶端，在此階段。</span><span class="sxs-lookup"><span data-stu-id="050d9-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="050d9-119">設定伺服器選項</span><span class="sxs-lookup"><span data-stu-id="050d9-119">Configure server options</span></span>

<span data-ttu-id="050d9-120">下表描述用於設定 SignalR 中樞選項：</span><span class="sxs-lookup"><span data-stu-id="050d9-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="050d9-121">選項</span><span class="sxs-lookup"><span data-stu-id="050d9-121">Option</span></span> | <span data-ttu-id="050d9-122">描述</span><span class="sxs-lookup"><span data-stu-id="050d9-122">Description</span></span> |
| ------ | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="050d9-123">如果用戶端不會傳送初始信號交換訊息，此時間間隔內，會關閉連線。</span><span class="sxs-lookup"><span data-stu-id="050d9-123">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="050d9-124">如果伺服器尚未在此時間間隔內傳送一則訊息，會自動才能保持連線開啟傳送 ping 訊息。</span><span class="sxs-lookup"><span data-stu-id="050d9-124">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> |
| `SupportedProtocols` | <span data-ttu-id="050d9-125">此主機所支援的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="050d9-125">Protocols supported by this hub.</span></span> <span data-ttu-id="050d9-126">根據預設，在伺服器上註冊的所有通訊協定，但可以從這份清單來停用個別的集線器的特定通訊協定移除通訊協定。</span><span class="sxs-lookup"><span data-stu-id="050d9-126">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | <span data-ttu-id="050d9-127">如果`true`、 詳細例外狀況訊息傳回給用戶端中樞方法中擲回例外狀況時。</span><span class="sxs-lookup"><span data-stu-id="050d9-127">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="050d9-128">預設值是`false`，因為這些例外狀況訊息可以包含機密資訊。</span><span class="sxs-lookup"><span data-stu-id="050d9-128">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="050d9-129">可以藉由提供選項委派之所有設定選項`AddSignalR`呼叫中`Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="050d9-129">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    })
}
```

<span data-ttu-id="050d9-130">單一中樞的選項會覆寫中提供的通用選項`AddSignalR`，可以使用設定[ `AddHubOptions<T>` ](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="050d9-130">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [`AddHubOptions<T>`](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

<span data-ttu-id="050d9-131">使用`HttpConnectionDispatcherOptions`設定進階的傳輸和緩衝區的記憶體管理相關的設定。</span><span class="sxs-lookup"><span data-stu-id="050d9-131">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="050d9-132">藉由傳遞至委派，會設定這些選項[ `MapHub<T>` ](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub)。</span><span class="sxs-lookup"><span data-stu-id="050d9-132">These options are configured by passing a delegate to [`MapHub<T>`](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="050d9-133">選項</span><span class="sxs-lookup"><span data-stu-id="050d9-133">Option</span></span> | <span data-ttu-id="050d9-134">描述</span><span class="sxs-lookup"><span data-stu-id="050d9-134">Description</span></span> |
| ------ | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="050d9-135">從用戶端接收的位元組數目上限，伺服器緩衝區。</span><span class="sxs-lookup"><span data-stu-id="050d9-135">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="050d9-136">增加此值可讓伺服器接收較大的訊息，但可能會造成負面影響記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="050d9-136">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> <span data-ttu-id="050d9-137">預設值為 32 KB。</span><span class="sxs-lookup"><span data-stu-id="050d9-137">The default value is 32KB.</span></span> |
| `AuthorizationData` | <span data-ttu-id="050d9-138">一份[ `IAuthorizeData` ](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)用來判斷是否經授權可連線至中樞的用戶端的物件。</span><span class="sxs-lookup"><span data-stu-id="050d9-138">A list of [`IAuthorizeData`](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> <span data-ttu-id="050d9-139">根據預設，這會填入值，從`Authorize`套用至 Hub 類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="050d9-139">By default, this is populated with values from the `Authorize` attributes applied to the Hub class.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="050d9-140">應用程式所傳送的位元組數目上限，伺服器緩衝區。</span><span class="sxs-lookup"><span data-stu-id="050d9-140">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="050d9-141">增加此值可讓伺服器傳送較大的訊息，但可能會造成負面影響記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="050d9-141">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> <span data-ttu-id="050d9-142">預設值為 32 KB。</span><span class="sxs-lookup"><span data-stu-id="050d9-142">The default value is 32KB.</span></span> |
| `Transports` | <span data-ttu-id="050d9-143">位元遮罩`HttpTransportType`值，可以限制所使用的傳輸用戶端可用來連接。</span><span class="sxs-lookup"><span data-stu-id="050d9-143">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> <span data-ttu-id="050d9-144">依預設會啟用所有的傳輸。</span><span class="sxs-lookup"><span data-stu-id="050d9-144">All transports are enabled by default.</span></span> |
| `LongPolling` | <span data-ttu-id="050d9-145">特有的長輪詢傳輸的其他選項。</span><span class="sxs-lookup"><span data-stu-id="050d9-145">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="050d9-146">Websocket 傳輸特定的其他選項。</span><span class="sxs-lookup"><span data-stu-id="050d9-146">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="050d9-147">長輪詢傳輸有可以使用設定的其他選項`LongPolling`屬性：</span><span class="sxs-lookup"><span data-stu-id="050d9-147">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="050d9-148">選項</span><span class="sxs-lookup"><span data-stu-id="050d9-148">Option</span></span> | <span data-ttu-id="050d9-149">描述</span><span class="sxs-lookup"><span data-stu-id="050d9-149">Description</span></span> |
| ------ | ----------- |
| `PollTimeout` | <span data-ttu-id="050d9-150">伺服器等候訊息傳送至用戶端，終止單一輪詢要求之前的最大時間量。</span><span class="sxs-lookup"><span data-stu-id="050d9-150">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="050d9-151">減少此值會導致更頻繁地發行新輪詢要求用戶端。</span><span class="sxs-lookup"><span data-stu-id="050d9-151">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> <span data-ttu-id="050d9-152">預設值為 90 秒。</span><span class="sxs-lookup"><span data-stu-id="050d9-152">The default value is 90 seconds.</span></span> |

<span data-ttu-id="050d9-153">WebSocket 傳輸有可以使用設定的其他選項`WebSockets`屬性：</span><span class="sxs-lookup"><span data-stu-id="050d9-153">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="050d9-154">選項</span><span class="sxs-lookup"><span data-stu-id="050d9-154">Option</span></span> | <span data-ttu-id="050d9-155">描述</span><span class="sxs-lookup"><span data-stu-id="050d9-155">Description</span></span> |
| ------ | ----------- |
| `CloseTimeout` | <span data-ttu-id="050d9-156">伺服器關閉，如果用戶端無法在此時間間隔內關閉之後，會結束連接。</span><span class="sxs-lookup"><span data-stu-id="050d9-156">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | <span data-ttu-id="050d9-157">委派可以用來設定`Sec-WebSocket-Protocol`標頭的自訂值。</span><span class="sxs-lookup"><span data-stu-id="050d9-157">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="050d9-158">此委派會接收要求的用戶端，做為輸入的值，而且預期會傳回所要的值。</span><span class="sxs-lookup"><span data-stu-id="050d9-158">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="050d9-159">設定用戶端選項</span><span class="sxs-lookup"><span data-stu-id="050d9-159">Configure client options</span></span>

<span data-ttu-id="050d9-160">可以在 設定用戶端選項`HubConnectionBuilder`型別 （適用於.NET 與 JavaScript 用戶端），以及`HubConnection`本身。</span><span class="sxs-lookup"><span data-stu-id="050d9-160">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="050d9-161">設定記錄</span><span class="sxs-lookup"><span data-stu-id="050d9-161">Configure logging</span></span>

<span data-ttu-id="050d9-162">記錄已設定使用的.NET 用戶端中`ConfigureLogging`方法。</span><span class="sxs-lookup"><span data-stu-id="050d9-162">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="050d9-163">記錄提供者和篩選條件可以註冊相同的方式和它們在伺服器上。</span><span class="sxs-lookup"><span data-stu-id="050d9-163">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="050d9-164">請參閱[登入 ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers)文件的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="050d9-164">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="050d9-165">以登錄記錄提供者，您必須安裝必要的套件。</span><span class="sxs-lookup"><span data-stu-id="050d9-165">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="050d9-166">請參閱[內建的記錄提供者](xref:fundamentals/logging/index#built-in-logging-providers)> 一節的文件的完整清單。</span><span class="sxs-lookup"><span data-stu-id="050d9-166">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="050d9-167">例如，若要啟用主控台記錄，請安裝`Microsoft.Extensions.Logging.Console`NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="050d9-167">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="050d9-168">呼叫`AddConsole`擴充方法：</span><span class="sxs-lookup"><span data-stu-id="050d9-168">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="050d9-169">中的 JavaScript 用戶端，類似`configureLogging`方法的存在。</span><span class="sxs-lookup"><span data-stu-id="050d9-169">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="050d9-170">提供`LogLevel`值，指出產生的記錄檔訊息的最低層級。</span><span class="sxs-lookup"><span data-stu-id="050d9-170">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="050d9-171">記錄檔會寫入至瀏覽器主控台視窗。</span><span class="sxs-lookup"><span data-stu-id="050d9-171">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="050d9-172">若要停用整個記錄，請指定`signalR.LogLevel.None`中`configureLogging`方法。</span><span class="sxs-lookup"><span data-stu-id="050d9-172">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="050d9-173">如下所示的 JavaScript 用戶端可用的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="050d9-173">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="050d9-174">將記錄層級設定為其中一個值會在訊息記錄**以上**該層級。</span><span class="sxs-lookup"><span data-stu-id="050d9-174">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="050d9-175">層級</span><span class="sxs-lookup"><span data-stu-id="050d9-175">Level</span></span> | <span data-ttu-id="050d9-176">描述</span><span class="sxs-lookup"><span data-stu-id="050d9-176">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="050d9-177">未記錄訊息。</span><span class="sxs-lookup"><span data-stu-id="050d9-177">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="050d9-178">表示整個應用程式中的失敗的訊息。</span><span class="sxs-lookup"><span data-stu-id="050d9-178">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="050d9-179">指出目前的作業失敗的訊息。</span><span class="sxs-lookup"><span data-stu-id="050d9-179">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="050d9-180">訊息，指出不嚴重的問題。</span><span class="sxs-lookup"><span data-stu-id="050d9-180">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="050d9-181">參考訊息。</span><span class="sxs-lookup"><span data-stu-id="050d9-181">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="050d9-182">適用於偵錯診斷訊息。</span><span class="sxs-lookup"><span data-stu-id="050d9-182">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="050d9-183">設計用來診斷特定問題非常詳細的診斷訊息。</span><span class="sxs-lookup"><span data-stu-id="050d9-183">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="050d9-184">設定允許的傳輸</span><span class="sxs-lookup"><span data-stu-id="050d9-184">Configure allowed transports</span></span>

<span data-ttu-id="050d9-185">SignalR 使用的傳輸可以設定為在`WithUrl`呼叫 (`withUrl`在 JavaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="050d9-185">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="050d9-186">位元 OR 之值的`HttpTransportType`可用來限制用戶端只使用指定的傳輸。</span><span class="sxs-lookup"><span data-stu-id="050d9-186">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="050d9-187">依預設會啟用所有的傳輸。</span><span class="sxs-lookup"><span data-stu-id="050d9-187">All transports are enabled by default.</span></span>

<span data-ttu-id="050d9-188">例如，若要停用 Server-Sent 事件傳輸，但允許 WebSockets 和長輪詢連線：</span><span class="sxs-lookup"><span data-stu-id="050d9-188">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="050d9-189">藉由設定中的 JavaScript 用戶端中，設定傳輸`transport`欄位上的選項物件，提供給`withUrl`:</span><span class="sxs-lookup"><span data-stu-id="050d9-189">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="050d9-190">設定承載驗證</span><span class="sxs-lookup"><span data-stu-id="050d9-190">Configure bearer authentication</span></span>

<span data-ttu-id="050d9-191">若要提供與 SignalR 要求的驗證資料，請使用`AccessTokenProvider`選項 (`accessTokenFactory`在 JavaScript 中) 來指定所需的存取語彙基元傳回的函式。</span><span class="sxs-lookup"><span data-stu-id="050d9-191">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="050d9-192">在.NET 用戶端，此存取權杖會傳遞做為 HTTP 「 承載驗證 」 權杖 (使用`Authorization`具有一種標頭`Bearer`)。</span><span class="sxs-lookup"><span data-stu-id="050d9-192">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="050d9-193">在 JavaScript 用戶端的存取權杖做為持有者權杖，**除了**在少數情況下，因為瀏覽器應用程式開發介面限制能夠套用標頭 （特別是，在 Server-Sent 事件和 Websocket 要求）。</span><span class="sxs-lookup"><span data-stu-id="050d9-193">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="050d9-194">在這些情況下，存取權杖提供做為查詢字串值`access_token`。</span><span class="sxs-lookup"><span data-stu-id="050d9-194">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="050d9-195">在.NET 用戶端`AccessTokenProvider`選項可指定使用中的選項委派`WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="050d9-195">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

<span data-ttu-id="050d9-196">中的 JavaScript 用戶端中，存取權杖已藉由設定`accessTokenFactory`欄位中的選項物件`withUrl`:</span><span class="sxs-lookup"><span data-stu-id="050d9-196">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="050d9-197">設定逾時和保持連線選項</span><span class="sxs-lookup"><span data-stu-id="050d9-197">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="050d9-198">有其他選項來設定逾時和保持行為`HubConnection`物件本身：</span><span class="sxs-lookup"><span data-stu-id="050d9-198">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="050d9-199">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="050d9-199">.NET Option</span></span> | <span data-ttu-id="050d9-200">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="050d9-200">JavaScript Option</span></span> | <span data-ttu-id="050d9-201">描述</span><span class="sxs-lookup"><span data-stu-id="050d9-201">Description</span></span> |
| ----------- | ----------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="050d9-202">伺服器活動的逾時。</span><span class="sxs-lookup"><span data-stu-id="050d9-202">Timeout for server activity.</span></span> <span data-ttu-id="050d9-203">如果伺服器尚未傳送任何訊息，此時間間隔中，用戶端會視為中斷連線的伺服器和觸發程序`Closed`事件 (`onclose`在 JavaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="050d9-203">If the server hasn't sent any message in this interval, the client considers the server disconnected and trigger the `Closed` event (`onclose` in JavaScript).</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="050d9-204">無法設定</span><span class="sxs-lookup"><span data-stu-id="050d9-204">Not configurable</span></span> | <span data-ttu-id="050d9-205">初始伺服器交握逾時。</span><span class="sxs-lookup"><span data-stu-id="050d9-205">Timeout for initial server handshake.</span></span> <span data-ttu-id="050d9-206">如果伺服器不會傳送交握回應，此時間間隔中，用戶端取消交握和觸發程序`Closed`事件 (`onclose`在 JavaScript 中)。</span><span class="sxs-lookup"><span data-stu-id="050d9-206">If the server doesn't send a handshake response in this interval, the client cancels the handshake and trigger the `Closed` event (`onclose` in JavaScript).</span></span> |

<span data-ttu-id="050d9-207">在.NET 用戶端中，指定逾時值為`TimeSpan`值。</span><span class="sxs-lookup"><span data-stu-id="050d9-207">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="050d9-208">JavaScript 用戶端在逾時值被指定數字。</span><span class="sxs-lookup"><span data-stu-id="050d9-208">In the JavaScript client, timeout values are specified as numbers.</span></span> <span data-ttu-id="050d9-209">數字代表時間值以毫秒為單位。</span><span class="sxs-lookup"><span data-stu-id="050d9-209">The numbers represent time values in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="050d9-210">設定其他選項</span><span class="sxs-lookup"><span data-stu-id="050d9-210">Configure additional options</span></span>

<span data-ttu-id="050d9-211">您可以設定其他選項中`WithUrl`(`withUrl`在 JavaScript 中) 上的方法`HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="050d9-211">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="050d9-212">.NET 選項</span><span class="sxs-lookup"><span data-stu-id="050d9-212">.NET Option</span></span> | <span data-ttu-id="050d9-213">JavaScript 選項</span><span class="sxs-lookup"><span data-stu-id="050d9-213">JavaScript Option</span></span> | <span data-ttu-id="050d9-214">描述</span><span class="sxs-lookup"><span data-stu-id="050d9-214">Description</span></span> |
| ----------- | ----------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | <span data-ttu-id="050d9-215">傳回字串，以在 HTTP 要求中的承載驗證權杖的形式的函式。</span><span class="sxs-lookup"><span data-stu-id="050d9-215">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | <span data-ttu-id="050d9-216">將此設`true`略過交涉步驟。</span><span class="sxs-lookup"><span data-stu-id="050d9-216">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="050d9-217">**Websocket 傳輸方式是只啟用的傳輸時，才支援**。</span><span class="sxs-lookup"><span data-stu-id="050d9-217">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="050d9-218">使用 Azure SignalR 服務時，無法啟用此設定。</span><span class="sxs-lookup"><span data-stu-id="050d9-218">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="050d9-219">無法設定 \*</span><span class="sxs-lookup"><span data-stu-id="050d9-219">Not configurable \*</span></span> | <span data-ttu-id="050d9-220">若要驗證要求傳送的 TLS 憑證集合。</span><span class="sxs-lookup"><span data-stu-id="050d9-220">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="050d9-221">無法設定 \*</span><span class="sxs-lookup"><span data-stu-id="050d9-221">Not configurable \*</span></span> | <span data-ttu-id="050d9-222">要與每個 HTTP 要求一起傳送的 HTTP cookie 的集合。</span><span class="sxs-lookup"><span data-stu-id="050d9-222">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="050d9-223">無法設定 \*</span><span class="sxs-lookup"><span data-stu-id="050d9-223">Not configurable \*</span></span> | <span data-ttu-id="050d9-224">要與每個 HTTP 要求一起傳送的認證。</span><span class="sxs-lookup"><span data-stu-id="050d9-224">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="050d9-225">無法設定 \*</span><span class="sxs-lookup"><span data-stu-id="050d9-225">Not configurable \*</span></span> | <span data-ttu-id="050d9-226">只有 WebSockets。</span><span class="sxs-lookup"><span data-stu-id="050d9-226">WebSockets only.</span></span> <span data-ttu-id="050d9-227">最大時間量，用戶端會等待關閉伺服器以確認在關閉要求後。</span><span class="sxs-lookup"><span data-stu-id="050d9-227">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="050d9-228">如果伺服器不在此時間內認可結束時，用戶端中斷連接。</span><span class="sxs-lookup"><span data-stu-id="050d9-228">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="050d9-229">無法設定 \*</span><span class="sxs-lookup"><span data-stu-id="050d9-229">Not configurable \*</span></span> | <span data-ttu-id="050d9-230">要與每個 HTTP 要求一起傳送的其他 HTTP 標頭的字典。</span><span class="sxs-lookup"><span data-stu-id="050d9-230">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="050d9-231">無法設定 \*</span><span class="sxs-lookup"><span data-stu-id="050d9-231">Not configurable \*</span></span> | <span data-ttu-id="050d9-232">委派可以用來設定或取代`HttpMessageHandler`用來傳送 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="050d9-232">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="050d9-233">不使用 WebSocket 連線。</span><span class="sxs-lookup"><span data-stu-id="050d9-233">Not used for WebSocket connections.</span></span> <span data-ttu-id="050d9-234">此委派必須傳回非 null 值，並且會收到做為參數的預設值。</span><span class="sxs-lookup"><span data-stu-id="050d9-234">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="050d9-235">修改設定，該預設值，並傳回它，，或傳回全新`HttpMessageHandler`執行個體。</span><span class="sxs-lookup"><span data-stu-id="050d9-235">Either modify settings on that default value and return it, or return a completely new `HttpMessageHandler` instance.</span></span> |
| `Proxy` | <span data-ttu-id="050d9-236">無法設定 \*</span><span class="sxs-lookup"><span data-stu-id="050d9-236">Not configurable \*</span></span> | <span data-ttu-id="050d9-237">傳送 HTTP 要求時使用 HTTP proxy。</span><span class="sxs-lookup"><span data-stu-id="050d9-237">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="050d9-238">無法設定 \*</span><span class="sxs-lookup"><span data-stu-id="050d9-238">Not configurable \*</span></span> | <span data-ttu-id="050d9-239">設定這個傳送 HTTP 和 Websocket 要求的預設認證的布林值。</span><span class="sxs-lookup"><span data-stu-id="050d9-239">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="050d9-240">這可讓您使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="050d9-240">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="050d9-241">無法設定 \*</span><span class="sxs-lookup"><span data-stu-id="050d9-241">Not configurable \*</span></span> | <span data-ttu-id="050d9-242">委派，可用來設定其他的 WebSocket 選項。</span><span class="sxs-lookup"><span data-stu-id="050d9-242">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="050d9-243">接收執行個體[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) ，可用來設定的選項。</span><span class="sxs-lookup"><span data-stu-id="050d9-243">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="050d9-244">以星號 （\*） 的選項不是可設定中的 JavaScript 用戶端，因為瀏覽器應用程式開發介面中的限制。</span><span class="sxs-lookup"><span data-stu-id="050d9-244">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="050d9-245">在.NET 用戶端，這些選項都可以修改選項委派提供給所`WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="050d9-245">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="050d9-246">在 JavaScript 用戶端，可以提供這些選項中提供的 JavaScript 物件`withUrl`:</span><span class="sxs-lookup"><span data-stu-id="050d9-246">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="050d9-247">其他資源</span><span class="sxs-lookup"><span data-stu-id="050d9-247">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
